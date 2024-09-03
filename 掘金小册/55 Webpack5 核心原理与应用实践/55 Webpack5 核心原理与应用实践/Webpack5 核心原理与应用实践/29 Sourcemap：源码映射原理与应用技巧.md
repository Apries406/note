[Sourcemap 协议](https://link.juejin.cn/?target=https%3A%2F%2Fdocs.google.com%2Fdocument%2Fd%2F1U1RGAehQwRypUTovF1KRlpiOFze0b-_2gc6fAH0KY0k%2Fedit%23heading%3Dh.qz3o9nc69um5) 最初由 Google 设计并率先在 Closure Inspector 实现，它的主要作用就是将经过压缩、混淆、合并的产物代码还原回未打包的原始形态，帮助开发者在生产环境中精确定位问题发生的行列位置，例如：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2fa7318e270d451684c3f8da78bbab5c~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

在 Webpack 内部，这段生成 Sourcemap 映射数据的逻辑并不复杂，一句话总结：在 [processAssets](https://link.juejin.cn/?target=https%3A%2F%2Fwebpack.js.org%2Fapi%2Fcompilation-hooks%2F%23processassets) 钩子遍历产物文件 `assets` 数组，调用 `webpack-sources` 提供的 `map` 方法，最终计算出 `asset` 与源码 `originSource` 之间的映射关系。

这个过程真正的难点在于 「如何计算映射关系」，因此本文会展开详细讲解 Sourcemap 映射结构与 VLQ 编码规则，以及 Webpack 提供的 `devtool` 配置项的详细用法。

## Sourcemap 映射结构

Sourcemap 最初版本生成的 `.map` 文件非常大，体积大概为编译产物的 10 倍；V2 之后引入 Base64 编码等算法，将之减少 20% ~ 30%；而最新版本 V3 又在 V2 基础上引入 VLQ 算法，体积进一步压缩了 50%。

这一系列进化造就了一个效率极高的 Sourcemap 体系，但伴随而来的则是较为复杂的 `mappings` 编码规则。V3 版本 Sourcemap 文件由三部分组成:

- 开发者编写的原始代码；
- 经过 Webpack 压缩、转化、合并后的产物，且产物中必须包含指向 Sourcemap 文件地址的 `//# sourceMappingURL=https://xxxx/bundle.js.map` 指令；
- 记录原始代码与经过工程化处理代码之间位置映射关系 Map 文件。

页面初始运行时只会加载编译构建产物，直到特定事件发生 —— 例如在 Chrome 打开 Devtool 面板时，才会根据 `//# sourceMappingURL` 内容自动加载 Map 文件，并按 Sourcemap 协议约定的映射规则将代码重构还原回原始形态，这既能保证终端用户的性能体验，又能帮助开发者快速还原现场，提升线上问题的定位与调试效率。

例如，在 Webpack 中设置 `devtool = 'source-map'` 即可同时打包出代码产物 `xxx.js` 文件与同名 `xxx.js.map` 文件，Map 文件通常为 JSON 格式，内容如：

```json
{
    "version": 3,
    "sources": [
        "webpack:///./src/index.js"
    ],
    "names": ["name", "console", "log"],
    "mappings": ";;;;;AAAA,IAAMA,IAAI,GAAG,QAAb;AAEAC,OAAO,CAACC,GAAR,CAAYF,IAAZ,E",
    "file": "main.js",
    "sourcesContent": [
        "const name = 'tecvan';\n\nconsole.log(name)"
    ],
    "sourceRoot": ""
}
```

各字段含义分别为：

- `version`： 指代 Sourcemap 版本，目前最新版本为 `3`；
- `names`：字符串数组，记录原始代码中出现的变量名；
- `file`：字符串，该 Sourcemap 文件对应的编译产物文件名；
- `sourcesContent`：字符串数组，原始代码的内容；
- `sourceRoot`：字符串，源文件根目录；
- `sources`：字符串数组，原始文件路径名，与 `sourcesContent` 内容一一对应；
- `mappings`：字符串数组，记录打包产物与原始代码的位置映射关系。

使用时，浏览器会按照 `mappings` 记录的数值关系，将产物代码映射回 `sourcesContent` 数组所记录的原始代码文件、行、列位置，这里面最复杂难懂的点就在于 `mappings` 字段的规则。

举个例子，对于下面的代码：

| 编译前                                       | 编译后                                                       |
| -------------------------------------------- | ------------------------------------------------------------ |
| `const name = 'tecvan';  console.log(name) ` | `/******/ (() => { // webpackBootstrap var __webpack_exports__ = {}; /*!**********************!*\   !*** ./src/index.js ***!   \**********************/ var name = 'tecvan'; console.log(name); /******/ })() ; //# sourceMappingURL=main.js.map ` |



当 `devtool = 'source-map'` 时，Webpack 生成的 `mappings` 字段为：

```objectivec
;;;;;AAAA,IAAMA,IAAI,GAAG,QAAb;AAEAC,OAAO,CAACC,GAAR,CAAYF,IAAZ,E
```

字段内容包含三层结构：

- 以 `;` 分割的**行映射**，每一个 `;` 对应编译产物每一行到源码的映射，上例经过分割后：

```js
[
  // 产物第 1-5 行内容为 Webpack 生成的 runtime，不需要记录映射关系
  '', '', '', '', '', 
  // 产物第 6 行的映射信息
  'AAAA,IAAMA,IAAI,GAAG,QAAb', 
  // 产物第 7 行的映射信息
  'AAEAC,OAAO,CAACC,GAAR,CAAYF,IAAZ,E'
]
```

- 以 `,` 分割的**片段映射**，每一个 `,` 对应该行中每一个代码片段到源码的映射，上例经过分割后：

```js
[
  // 产物第 1-5 行内容为 Webpack 生成的 runtime，不需要记录映射关系
  '', '', '', '', '', 
  // 产物第 6 行的映射信息
  [
    // 片段 `var` 到 `const` 的映射
    'AAAA', 
    // 片段 `name` 到 `name` 的映射
    'IAAMA', 
    // 等等
    'IAAI', 'GAAG', 'QAAb'], 
  // 产物第 7 行的映射信息
  ['AAEAC', 'OAAO', 'CAACC', 'GAAR', 'CAAYF', 'IAAZ', 'E']
]
```

- 第三层逻辑为片段映射到源码的具体位置，以上例

   

  ```
  IAAMA
  ```

   

  为例：

  - 第一位 `I` 代表该代码片段在产物中列数；
  - 第二位 `A` 代表源码文件的索引，即该片段对标到 `sources` 数组的元素下标；
  - 第三位 `A` 代表片段在源码文件的行数；
  - 第四位 `M` 代表片段在源码文件的列数；
  - 第五位 `A` 代表该片段对应的名称索引，即该片段对标到 `names` 数组的元素下标。

上述第1、2层逻辑比较简单，唯一需要注意的是片段之间是一种相对偏移关系，例如对于上例第六行映射值：`AAAA,IAAMA,IAAI,GAAG,QAAb`，每一个片段的第一位 —— 即片段列数为 `A,I,I,G,Q`，分别代表：

- `A` ：第 `A` 列；
- `I` ：第 `A + I` 列；
- `I` ：第 `A + I + I` 列；
- `G` ：第 `A + I + I + G` 列；
- `Q` ：第 `A + I + I + G + Q` 列。

这种相对偏移能减少 Sourcemap 产物的体积，提升整体性能。注意，第三层逻辑中的片段位置映射则用到了一种比较高效数值编码算法 —— VLQ(Variable-length Quantity)。

## VLQ 编码

[VLQ](https://link.juejin.cn/?target=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FVariable-lengsth_quantity) 是一种将整数数值转换为 Base64 的编码算法，它先将任意大的整数转换为一系列六位字节码，再按 Base64 规则转换为一串可见字符。VLQ 使用六位比特存储一个编码分组，例如：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3885498f30a7462789d05fe8af482c6f~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

数字 7 经过 VLQ 编码后，结果为 `001110`，其中：

- 第一位为连续标志位，标识后续分组是否为同一数字；
- 第六位表示该数字的正负符号，0为正整数，1为负整数；
- 中间第 2-5 为实际数值。

这样一个六位编码分组，就可以按照 Base64 的映射规则转换为 `ABC` 等可见字符，例如上述数字 7 编码结果 `001110`，等于十进制的 14，按 Base64 字码表可映射为字母 `O`。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1ebe7f1a58974319badd51afa21d9f62~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

但是，分组中只有中间的 4 个字节用于表示数值，因此单个分组只能表达 **-15 ~ 15** 之间的数值范围，对于超过这个范围的整数，需要组合多个分组，共同表达同一数字，具体规则：

- 第一个分组的最后一位为符号位，其它分组从 2-6 均为数值位；
- 取二进制值最后四位为第一个分组值，之后从后到前，每 5 位划分为一个分组；
- 除最后一个分组外，其余分组的连续标志位都设置为 1。

例如，对于十进制 -17，其二进制为 `10001` (取 17 的二进制) 共 5 位，首先从后到前拆分为两组，后四位 `0001` 为第一组，连续标志位为 1，符号位为 1，结果为 `1,0001,1`；剩下的 `1` 分配到第二个 —— 也是最后一个分组，连续标志位为 0，结果为 `0,00001`。按 Base64 规则 `[100011, 000001]` 最终映射为 `jA`。

```dart
十进制     二进制               VLQ    Base64
  -17 => 1,0001 => 100011, 000001 =>     jA
```

同样的，对于更大的数字，例如 1200，其二进制为 `10010110000`，分组为 `[10, 01011, 0000]`，从后到前编码，第一个分组为 `1,0000,0`；第二个分组为 `1,01011`；最后一个分组为 `0,00010`。按 Base64 映射为 `grC`。

```ini
十进制            二进制                     VLQ    Base64
 1200 => 10;01011;0000 => 100000,101011,000010 =>    grC
```

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5a3c5cc523044b959ec2ae1fcefee6a4~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

结合 VLQ 编码规则，我们再回过头来解读本章开头的例子，对于代码：

| 编译前                                       | 编译后                                                       |
| -------------------------------------------- | ------------------------------------------------------------ |
| `const name = 'tecvan';  console.log(name) ` | `/******/ (() => { // webpackBootstrap var __webpack_exports__ = {}; /*!**********************!*\   !*** ./src/index.js ***!   \**********************/ var name = 'tecvan'; console.log(name); /******/ })() ; //# sourceMappingURL=main.js.map ` |



编译生成 `mappings`：

```objectivec
;;;;;AAAA,IAAMA,IAAI,GAAG,QAAb;AAEAC,OAAO,CAACC,GAAR,CAAYF,IAAZ,E
```

按行、片段规则分割后，得出如下片段：

```csharp
[
  // 产物第 1-5 行内容为 Webpack 生成的 runtime，不需要记录映射关系
  '', '', '', '', '', 
  // 产物第 6 行的映射信息
  ['AAAA', 'IAAMA', 'IAAI', 'GAAG', 'QAAb'], 
  // 产物第 7 行的映射信息
  ['AAEAC', 'OAAO', 'CAACC', 'GAAR', 'CAAYF', 'IAAZ', 'E']
]
```

以第 6 行 `['AAAA', 'IAAMA', 'IAAI', 'GAAG', 'QAAb']` 为例：

- `AAAA` 解码结果为 `[000000, 000000, 000000, 000000]`，即产物第 6 行**第** **0** **列**映射到 `sources[0]` 文件的**第** **0** **行**，**第** **0** **列**，实际对应 `var` 到 `const` 的位置映射；
- `IAAMA` 解码结果为 `[001000, 000000, 000000, 001100, 000000]`，即产物第 6 行第 4 列映射到 `sources[0]` 文件的**第** **0** **行**，**第** **6** **列**，实际对应产物 `name` 到源码 `name` 的位置映射。

其它片段以此类推，Webpack 生成 `.map` 文件时，只需要在 `webpack-sources` 中，按照这个编码规则计算好编译前后的代码映射关系即可。

## `devtool` 规则详解

Webpack 提供了两种设置 Sourcemap 的方式，一是通过 `devtool` 配置项设置 Sourcemap 规则短语；二是直接使用 `SourceMapDevToolPlugin` 或 `EvalSourceMapDevToolPlugin` 插件深度定制 Sourcemap 的生成逻辑。

[devtool](https://link.juejin.cn/?target=https%3A%2F%2Fwebpack.js.org%2Fconfiguration%2Fdevtool%2F) 支持 25 种字符串枚举值，包括 `eval`、`source-map`、`eval-source-map` 等：

| devtool                                  | performance                     | production | quality        | comment                                                      |
| ---------------------------------------- | ------------------------------- | ---------- | -------------- | ------------------------------------------------------------ |
| (none)                                   | build: fastest rebuild: fastest | yes        | bundle         | Recommended choice for production builds with maximum performance. |
| eval                                     | build: fast rebuild: fastest    | no         | generated      | Recommended choice for development builds with maximum performance. |
| eval-cheap-source-map                    | build: ok rebuild: fast         | no         | transformed    | Tradeoff choice for development builds.                      |
| eval-cheap-module-source-map             | build: slow rebuild: fast       | no         | original lines | Tradeoff choice for development builds.                      |
| eval-source-map                          | build: slowest rebuild: ok      | no         | original       | Recommended choice for development builds with high quality SourceMaps. |
| cheap-source-map                         | build: ok rebuild: slow         | no         | transformed    |                                                              |
| cheap-module-source-map                  | build: slow rebuild: slow       | no         | original lines |                                                              |
| source-map                               | build: slowest rebuild: slowest | yes        | original       | Recommended choice for production builds with high quality SourceMaps. |
| inline-cheap-source-map                  | build: ok rebuild: slow         | no         | transformed    |                                                              |
| inline-cheap-module-source-map           | build: slow rebuild: slow       | no         | original lines |                                                              |
| inline-source-map                        | build: slowest rebuild: slowest | no         | original       | Possible choice when publishing a single file                |
| eval-nosources-cheap-source-map          | build: ok rebuild: fast         | no         | transformed    | source code not included                                     |
| eval-nosources-cheap-module-source-map   | build: slow rebuild: fast       | no         | original lines | source code not included                                     |
| eval-nosources-source-map                | build: slowest rebuild: ok      | no         | original       | source code not included                                     |
| inline-nosources-cheap-source-map        | build: ok rebuild: slow         | no         | transformed    | source code not included                                     |
| inline-nosources-cheap-module-source-map | build: slow rebuild: slow       | no         | original lines | source code not included                                     |
| inline-nosources-source-map              | build: slowest rebuild: slowest | no         | original       | source code not included                                     |
| nosources-cheap-source-map               | build: ok rebuild: slow         | no         | transformed    | source code not included                                     |
| nosources-cheap-module-source-map        | build: slow rebuild: slow       | no         | original lines | source code not included                                     |
| nosources-source-map                     | build: slowest rebuild: slowest | yes        | original       | source code not included                                     |
| hidden-nosources-cheap-source-map        | build: ok rebuild: slow         | no         | transformed    | no reference, source code not included                       |
| hidden-nosources-cheap-module-source-map | build: slow rebuild: slow       | no         | original lines | no reference, source code not included                       |
| hidden-nosources-source-map              | build: slowest rebuild: slowest | yes        | original       | no reference, source code not included                       |
| hidden-cheap-source-map                  | build: ok rebuild: slow         | no         | transformed    | no reference                                                 |
| hidden-cheap-module-source-map           | build: slow rebuild: slow       | no         | original lines | no reference                                                 |
| hidden-source-map                        | build: slowest rebuild: slowest | yes        | original       | no reference. Possible choice when using SourceMap only for error reporting purposes. |



> 提示：内容摘抄自 Webpack [官网](https://link.juejin.cn/?target=https%3A%2F%2Fwebpack.js.org%2Fconfiguration%2Fdevtool%2F)。

分开来看都特别晦涩，但这些枚举值内在有一个潜规则：都是由 `inline`、`eval`、`source-map`、`nosources`、`hidden`、`cheap`、`module` 七种关键字组合而成，这些关键词各自代表一项 Sourcemap 规则，拆开来看：

1. **`eval` 关键字**：当 `devtool` 值包含 `eval` 时，生成的模块代码会被包裹进一段 `eval` 函数中，且模块的 Sourcemap 信息通过 `//# sourceURL` 直接挂载在模块代码内。例如：

```ini
eval("var foo = 'bar'\n\n\n//# sourceURL=webpack:///./src/index.ts?")
```

`eval` 模式编译速度通常比较快，但产物中直接包含了 Sourcemap 信息，因此只推荐在开发环境中使用。

1. **`source-map` 关键字**：当 `devtool` 包含 `source-map` 时，Webpack 才会生成 Sourcemap 内容。例如，对于 `devtool = 'source-map'`，产物会额外生成 `.map` 文件，形如：

```json
{
    "version": 3,
    "sources": [
        "webpack:///./src/index.ts"
    ],
    "names": [
        "console",
        "log"
    ],
    "mappings": "AACAA,QAAQC,IADI",
    "file": "bundle.js",
    "sourcesContent": [
        "const foo = 'bar';\nconsole.log(foo);"
    ],
    "sourceRoot": ""
}
```

实际上，除 `eval` 之外的其它枚举值都包含该字段。

1. **`cheap` 关键字**：当 `devtool` 包含 `cheap` 时，生成的 Sourcemap 内容会抛弃**列**维度的信息，这就意味着浏览器只能映射到代码行维度。例如 `devtool = 'cheap-source-map'` 时，产物：

```swift
{
    "version": 3,
    "file": "bundle.js",
    "sources": [
        "webpack:///bundle.js"
    ],
    "sourcesContent": [
        "console.log(\"bar\");"
    ],
    // 带 cheap 效果：
    "mappings": "AAAA",
    // 不带 cheap 效果：
    // "mappings": "AACAA,QAAQC,IADI",
    "sourceRoot": ""
}
```

浏览器映射效果：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/830f8d072cfd4b8bae4c95e7d29c974b~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

虽然 Sourcemap 提供的映射功能可精确定位到文件、行、列粒度，但有时在**行**级别已经足够帮助我们达到调试定位的目的，此时可选择使用 `cheap` 关键字，简化 Sourcemap 内容，减少 Sourcemap 文件体积。

1. **`module` 关键字**：`module` 关键字只在 `cheap` 场景下生效，例如 `cheap-module-source-map`、`eval-cheap-module-source-map`。当 `devtool` 包含 `cheap` 时，Webpack 根据 `module` 关键字判断按 loader 联调处理结果作为 source，还是按处理之前的代码作为 source。例如：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0630aad5c07744f8be3d2a3d0198d959~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

注意观察上例 `sourcesContent` 字段，左边 `devtool` 带 `module` 关键字，因此此处映射的，是包含 `class Person` 的最原始代码；而右边生成的 `sourcesContent` ，则是经过 babel-loader 编译处理的内容。

1. **`nosources` 关键字**：当 `devtool` 包含 `nosources` 时，生成的 Sourcemap 内容中不包含源码内容 —— 即 `sourcesContent` 字段。例如 `devtool = 'nosources-source-map'` 时，产物：

```json
{
    "version": 3,
    "sources": [
        "webpack:///./src/index.ts"
    ],
    "names": [
        "console",
        "log"
    ],
    "mappings": "AACAA,QAAQC,IADI",
    "file": "bundle.js",
    "sourceRoot": ""
}
```

虽然没有带上源码，但 `.map` 产物中还带有文件名、 `mappings` 字段、变量名等信息，依然能够帮助开发者定位到代码对应的原始位置，配合 `sentry` 等工具提供的源码映射功能，可在异地还原诸如错误堆栈之类的信息。

1. **`inline` 关键字**：当 `devtool` 包含 `inline` 时，Webpack 会将 Sourcemap 内容编码为 Base64 DataURL，直接追加到产物文件中。例如对于 `devtool = 'inline-source-map'`，产物：

```arduino
console.log("bar");
//# sourceMappingURL=data:application/json;charset=utf-8;base64,eyJ2ZXJzaW9uIjozLCJzb3VyY2VzIjpbIndlYnBhY2s6Ly8vLi9zcmMvaW5kZXgudHMiXSwibmFtZXMiOlsiY29uc29sZSIsImxvZyJdLCJtYXBwaW5ncyI6IkFBQ0FBLFFBQVFDLElBREkiLCJmaWxlIjoiYnVuZGxlLmpzIiwic291cmNlc0NvbnRlbnQiOlsiY29uc3QgZm9vID0gJ2Jhcic7XG5jb25zb2xlLmxvZyhmb28pOyJdLCJzb3VyY2VSb290IjoiIn0=
```

`inline` 模式编译速度较慢，且产物体积非常大，只适合开发环境使用。

1. **`hidden` 关键字**：通常，产物中必须携带 `//# sourceMappingURL=` 指令，浏览器才能正确找到 Sourcemap 文件，当 `devtool` 包含 `hidden` 时，编译产物中不包含 `//# sourceMappingURL=` 指令。例如：

| `devtool = 'hidden-source-map'`                              | `devtool = 'source-map'`                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `/******/ (() => { // webpackBootstrap var __webpack_exports__ = {}; /*!**********************!*\   !*** ./src/index.ts ***!   \**********************/ var Person = /** @class */ (function () { }());  /******/ })(); ` | `/******/ (() => { // webpackBootstrap var __webpack_exports__ = {}; /*!**********************!*\   !*** ./src/index.ts ***!   \**********************/ var Person = /** @class */ (function () { }());  /******/ })(); //# sourceMappingURL=bundle.js.map ` |



两者区别仅在于编译产物最后一行的 `//# sourceMappingURL=` 指令，当你需要 Sourcemap 功能，又不希望浏览器 Devtool 工具自动加载时，可使用此选项。需要打开 Sourcemap 时，可在浏览器中手动加载：

![f412b094-f908-42a3-9b51-a3f3622a71c0.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/98e6b0cd9aa54f6b8f3090dcd36c91b9~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

总结一下，Webpack 的 `devtool` 值都是由以上七种关键字的一个或多个组成，虽然提供了 27 种候选项，但逻辑上都是由上述规则叠加而成，例如：

- `cheap-source-map`：代表 **不带列映射** 的 Sourcemap ；
- `eval-nosources-cheap-source-map`：代表 **以 `eval` 包裹模块代码** ，且 **`.map` 映射文件中不带源码**，且 **不带列映射** 的 Sourcemap。

其它选项以此类推。最后再总结一下：

- 对于开发环境，适合使用：
  - `eval`：速度极快，但只能看到原始文件结构，看不到打包前的代码内容；
  - `cheap-eval-source-map`：速度比较快，可以看到打包前的代码内容，但看不到 loader 处理之前的源码；
  - `cheap-module-eval-source-map`：速度比较快，可以看到 loader 处理之前的源码，不过定位不到列级别；
  - `eval-source-map`：初次编译较慢，但定位精度最高；
- 对于生产环境，则适合使用：
  - `source-map`：信息最完整，但安全性最低，外部用户可轻易获取到压缩、混淆之前的源码，慎重使用；
  - `hidden-source-map`：信息较完整，安全性较低，外部用户获取到 `.map` 文件地址时依然可以拿到源码；
  - `nosources-source-map`：源码信息缺失，但安全性较高，需要配合 Sentry 等工具实现完整的 Sourcemap 映射。

## 使用 `source-map` 插件

上面介绍的 `devtool` 配置项，本质上只是一种方便记忆、使用的规则缩写短语，Sourcemap 的底层处理逻辑实际由 `SourceMapDevToolPlugin` 与 `EvalSourceMapDevToolPlugin` 插件实现。

在 `devtool` 基础上，插件还提供了更多、更细粒度的配置项，用于满足更复杂的需求场景，包括：

- 使用 `test`、`include`、`exclude` 配置项过滤需要生成 Sourcemap 的 Bundle；
- 使用 `append`、`filename`、`moduleFilenameTemplate`、`publicPath` 配置项设定 Sourcemap 文件的文件名、URL 。

使用方法与其它插件无异，如：

```js
const webpack = require('webpack');
module.exports = {
  // ...
  devtool: false,
  plugins: [new webpack.SourceMapDevToolPlugin({
      exclude: ['vendor.js']
  })],
};
```

插件配置规则较简单，此处不赘述。

## 总结

综上，Sourcemap 是一种高效的位置映射算法，它将产物到源码之间的位置关系表达为 `mappings` 分层设计与 VLQ 编码，再通过 Chrome、Safari、VS Code、Sentry 等工具异地还原为接近开发状态的源码形式。

在 Webpack 中，通常只需要选择适当的 `devtool` 短语即可满足大多数场景需求，特殊情况下也可以直接使用 `SourceMapDevToolPlugin` 做更深度的定制化。

## 思考题

为什么 Sourcemap 要设计分层结构 + VLQ 编码做行列映射？假设直接记录行列号，会有什么问题？