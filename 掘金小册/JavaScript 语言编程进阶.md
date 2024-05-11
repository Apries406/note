# 开篇|你真的熟悉JavaScript吗？
> [!info]
> 任何能够用 JavaScript 实现的应用系统，最终都必将用 JavaScript 实现。
> 
> —— Atwood 定律

近十年来，JavaScript 语言早已逃离出 Web 前端的束缚，广泛应用在各个领域。我们不但可以用来写前端页面，也可以利用 `Node.js` 写后端服务，还可以利用 `Electron` 写桌面应用，亦或是用 `React Native`、`NativeScript` 来写移动客户端，甚至我们还可以为`嵌入式设备`编写应用。

JavaScript 不但是有志于成为前端工程师的同学的必备技能，也越来越成为其他领域工程师的第二技能，比如`客户端工程师`学习 JavaScript 来编写跨端应用，在开发效率上更上一层楼；再比如`服务端工程师`学习 JavaScript 来搭建简单的后台系统，从而减轻对前端工程师的依赖。

可以说，**JavaScript 已经成为了跨领域开发的首选语言，越来越成为了衡量工程师综合素质能力的标尺**。正如那条著名的 `Atwood 定律`：**任何能够用 JavaScript 实现的应用系统，最终都必将用 JavaScript 实现**。

不过，要学习好 JavaScript，我们还是得先简单回顾一下它的历史。

## JavaScript 的演化历程

JavaScript 最初于 1995 年诞生于网景公司（Netscape）与太阳微系统公司（Sun Microsystems）的合作，并在 1997 年由 Ecma 标准化为第一个版本，称之为 `ECMAScript`，并沿用至今。可以说，我们今天使用的 JavaScript 学名应该叫做 ECMAScript，各大引擎（比如 `V8`、`SpiderMonkey`、`Webkit`）都是 ECMAScript 规范的各自实现。

> [!info]
> **冷知识**
>
> JavaScript 这个名字虽然使用得更广泛，但是它目前却是数据库厂商甲骨文（Oracle）的商标，这源于 Oracle 在 2009 年对 Sun 公司的收购。不过好在 Oracle 几乎并没有使用此商标做任何事，否则我们也许会看到 Oracle 和 Google 在 Java 上的世纪诉讼盛况。

ECMAScript 在 1999 年发布第三版（`ES3`）后便陷入了持续十年的停滞。这十年间，微软的 IE 浏览器异军突起，击败了网景浏览器，占用了绝大部分浏览器市场。`IE6` 版本成为了后续数年间前端工程师的梦魇，直到后来 Firefox、Chrome 的兴起。

在那个特殊的年代，前端工程师编写 JavaScript 代码很难讲是一种享受：

- 没有类语法，自然不能继承，你可以使用 [prototype.js](https://link.juejin.cn/?target=http%3A%2F%2Fprototypejs.org%2F "http://prototypejs.org/")，或者学习大神 [John Resig 的办法](https://link.juejin.cn/?target=https%3A%2F%2Fjohnresig.com%2Fblog%2Fsimple-javascript-inheritance%2F "https://johnresig.com/blog/simple-javascript-inheritance/")，看懂这段代码可不太容易；
- 没有 JSON，需要引用第三方库，比如 [JSON3](https://link.juejin.cn/?target=https%3A%2F%2Fbestiejs.github.io%2Fjson3%2F "https://bestiejs.github.io/json3/")；
- 没有模块化系统，很晚才出现了遵循 AMD 规范的 [require.js](https://link.juejin.cn/?target=https%3A%2F%2Frequirejs.org%2F "https://requirejs.org/")，兼具打包功能；
- 没有视图框架，所有都是基于模板引擎的，比如 [backbone.js](https://link.juejin.cn/?target=https%3A%2F%2Fbackbonejs.org%2F "https://backbonejs.org/")。

> [!info]
> `jQuery` 是这些方案的集大成者，虽然它的主业是抹平不同浏览器之间在 DOM 操作上的差异。

这些第三方工具虽然在现在已经没落了，但它们确实也完成了自己的使命——将更多的特性带给了 ECMAScript，比如 class 语法、JSON 对象，比如 ES Modules、Dynamic Import，再比如模板字符串、Proxy，等等，正是它们的贡献才让我们今天的前端开发如此现代化。

不过，这些特性的引入不是一蹴而就的。在停滞十年之后，也就是 Chrome 浏览器发布的第二年，ECMAScript 终于更新到了第五版（第四版胎死腹中），称之为 `ES5` 或 ES2009。

ES5 在 ES3 基础上添加了很多新的 API，比如 JSON、Array.prototype.map、Array.prototype.filter、Array.prototype.every、Function.prototype.bind 等等。但我认为，ES5 最具革命性的是**重新定义了对象的属性结构**，引入了`属性描述符`的概念，对 getter/setter 的支持大大增强了对象结构的数据灵活性，为后面数年间扩展更多数据类型打下了不朽的基础。

2015 年发布的 `ES6` 扩展了数据类型，比如 Set、Map、Symbol 被引入进来，var 被 `let` 和 `const` 替代，引入了 class，当然最著名的当属`箭头函数`和`解构运算符`（...）。

自此以后到现在，ECMAScript 保持每年一版本的频率更新着。虽然浏览器需要一定的版本迭代时间，但是 Node.js 环境总是能低成本地升级到最新版本，以支持最新的特性。

## 正确认识 JavaScript 的学习曲线

JavaScript 是简单的，它没有真正意义上的类（JavaScript 中的类仅仅是函数的语法糖而已）、没有范型（TypeScript 才引入了一定的范型）、不需要编译即可运行（动态语言，解释执行），更没有多线程（只能依赖各种 Worker 来模拟）。同时 JavaScript 也是复杂的，它的原型链上和属性描述符上的设计，都不同于其他语言。

加上随着 ECMAScript 规范的快速迭代，相当一部分陈旧的知识已经被淘汰，但考虑到兼容性，浏览器环境仍然保留了对大部分旧 API 的支持，新旧 API 的共存也让初学者更加困惑。

在以上种种因素的作用下，积累了大量的边界知识和陷阱，只需要阅读一段代码，有经验的人一眼就能看出编写者的水平高低。

那么，对于刚刚入门的前端同学，亦或是来自其他领域的跨界开发者，**如何能够在短时间内快速提升自己的 JavaScript 水平，高效编写高质量的代码**，应用于高要求的生产环境中，应该是各位关心的难题之一。

我本人在过去也是忙碌于各种做不完的业务。普通的业务通常属于“上层建筑”，依赖现成的框架和工具很容易就能跑起来，但是却鲜有对比，看不出自己有没有提升，与别人的差距有多少。

直到近年来我开始有机会编写通用型框架和工具，涉足业务的“地基”，越来越能体会到 **`能写业务`和`写好业务`之间的差距还是很大的，而具备后者能力的同事明显更有机会承担更关键的项目和角色**，所谓“能力越大、责任越大”。

写好业务的公认能力即是对编程语言的充分掌握。**我写这本小册的目的，就是希望能为大家摘取和整理最关键的 JavaScript 编程知识，让大家快速掌握 JavaScript 高阶编程能力，写出高质量的生产型代码**。

小册的覆盖范围基于语言规范，因此足够全面；指导意见和结论来自于大厂实际开发环境，因此足够实用。

如何评价一个人具备了一定的高级 JavaScript 能力呢？我总结有以下几点：

1. **知道常用 API 的能力范围和不足，避免或减少写出有缺陷的代码；**
2. **知道相关联的对象或 API 之间的联系，灵活拓展非现成的能力；**
3. **知道最新的语法和 API，能在适当的场景优化代码质量和运行效率。**

## 应该怎样学习 JavaScript

ECMAScript 的这么多版本迭代更新了大量的知识，理论上阅读 ECMAScript 规范文档确实能做到精通，轻松满足上述三种能力的要求，然而更简单的 HTML 语言，恐怕也没几个人通盘阅读过 W3C 规范吧。

事实上大部分规范的细节都是我们不需要关心的，或者很难能用得上的。我将基于过去数年间的团队开发经验，摘取日常 **`使用概率更高、犯错概率更高以及忽略概率更高`** 的知识，分`三个阶段`讲解，以呼应我对高级 JavaScript 工程师能力的认知。

1. **基础篇**：学习 JavaScript 基础类型的高级知识点，这是日常最常使用的知识，我们要学习到不同类型数据的结构、操作 API，梳理不同需求下更推荐的编码方式。
2. **进阶篇**：学习 JavaScript 的重要对象和概念，比如原型链、Set、Map、Reflect、Proxy，能够让我们在实现一些高级功能时更具选择性。
3. **高级篇**：学习 JavaScript 语言层面的概念和操作，比如迭代器、模块化、异步，通过它们我们可以高效优化编写出的代码。

我编写这本小册的过程，也是一个学习的过程。我希望我通过总结实际生产环境、阅读整理规范文档所花费的这些时间，能够真正帮助到大家实现快速成长。如果有任何错误和纰漏，还请及时指正！
# 基础篇|作用域:变量的可访问性原理
早年间有一种说法叫做 “JavaScript 没有作用域”，当然这是一种夸张的讲法，其表达的意思应该是：JavaScript 没有块级作用域。

比如下面这样的代码是可以工作的：
```javascript
{ var foo = "Hello"; }
console.log(foo) // "Hello"
```
还有这样的代码：
```javascript
for (var i = 0; i < 10; i++); 
console.log(i) // 10
```

大括号和 for 语句并没有束缚住变量的作用范围。

在现代的 JavaScript 执行环境中，基本已经没有了这样的困扰，“没有块级作用域”也不再适用于 JavaScript。

## 作用域

作用域，或者称之为“上下文”，是变量被承载的容器。

在最新的 ECMAScript 规范中，定义了一个叫做`环境记录（Environment Record）`的抽象概念，可理解为就是作用域。从 `Record` 这种词我们就能联想到它是用来记录变量的。这里的变量不仅仅包括 var 声明的变量，还包括 `const`、`let`、`class`、`function`、`with`、`catch` 等声明的变量或参数。一旦这些语句被执行，那么就会创建一个新的 Environment Record。

Environment Record 是抽象的（abstract），它有三个子类，分别是：

1. `Declarative Environment Record`，包括 var、const、let、class、module、function、catch；
2. `Object Environment Record`，包括 with；
3. `Global Environment Record`，浏览器中指 globalThis，Node.js 中指 global。

其中，Declarative Environment Record 还有两个子类：

1. `Function Environment Record` —— function；
2. `Module Environment Record` —— module。

因此，目前官方规范中定义的这几种作用域的关系是：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/deaeadc0385348b28646db1919532c8b~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

每一个 Environment Record 有一个 `OuterEnv` 属性，指向另一个 Environment Record 实例。从这一点上我们就能看出来，**作用域是有上下层级关系的，所有作用域应该可以组成一个树形结构**，这和我们的认知是一致的。我举一个例子：

```javascript
var foo = 1; // Env3

function onload() {
    var bar = 2; // Env2
    return function callback() {
        var baz = 3; // Env1
        return foo + bar + baz;
    };
}

```


在上面这段代码中，我们至少定义了三个作用域：Env1、Env2 和 Env3。当需要计算 `foo + bar + baz` 的时候，需要依次`从下向上`搜索作用域内是否有对应的变量声明。

- 首先，在 Env1 中查找 foo，不存在，向上在 Env2 中查找，也不存在，继续向上，直到在 Env3 中找到，取其值；
- 其次，在 Env1 中查找 bar，不存在，向上在 Env2 中找到，取其值；
- 最后，在 Env1 中找到 baz，取其值。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/368b79dd5f774af488d5084e890ed0af~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

由此可见，这三个作用域的关系：Env1 的 OuterEnv 是 Env2，Env2 的 OuterEnv 是 Env3，这一条关系链称为`作用域链`。

不同类型的 Environment Record，其 `OuterEnv` 类型是受限的。比如，**Global Environment Record** 的 OuterEnv 总是 `null`，而 **Module Environment Record** 的 OuterEnv 总是 **Global Environment Record**。

前者容易理解，毕竟树形结构总有一个根结点。至于后者，我们可以看下面这个例子：
```javascript
// index.js
var foo = 1;

function onload() {
    var bar = 2;
    import("./dynamic.js");
}
```

```javascript
// dynamic.js
var baz = 3;
console.log(foo + bar + baz);=
```

dynamic.js 所在的作用域，是一个 **Module Environment Record**，由于其 `OuterEnv` 是 **Global Environment Record**，因此它可以访问到 foo 变量，但是访问不到 bar 变量，于是在 console.log 那一行就会报错。

除了主动声明的变量，我们还经常会使用到 `this`。

## this

`this` 通常与对象相关，在对象上调用一个函数，这个函数内部的 `this` 通常就指向这个对象：

```javascript
const dog = {
    name: 'spark',
    bark: function() {
        return this.name;
    },
};

console.log(dog.bark()); // "spark"
```


那 this 具体是如何工作的呢？这一节我们就来理清一下。

ECMAScript 规定，在 Environment Record 的抽象定义上，有一个函数叫做 `HasThisBinding()`，不同的子类对此函数的实现不同。

当你的代码在执行过程中遇到 `this` 的时候，具体的计算规则是这样的：

1. 设 **env** 等于当前的 `Environment Record`；
2. 设 **exist** 等于 `env.HasThisBinding()` 返回值；
3. 如果 **exist** 为 true，则返回 `env.GetThisBinding()`，终止；
4. 赋值 **env** 等于 `env.OuterEnv`，跳到步骤 2 继续执行。

> [!info]
> GetThisBinding() 下面会提到。

可见这个过程就是一个向上递归遍历的过程，哪一级的 Environment Record 有 `ThisBinding`，就返回它，非常类似于`作用域链`。

你可能担心这个的算法不会死循环或者抛异常么？不会，因为 Environment Record 结构的最顶层是一个 Global Environment Record，它一定有 `ThisBinding`，下面会讲到。

我们来看看各种各样的 Environment Record 的 `ThisBinding` 是怎样的。

### Declarative Environment Record

Declarative Environment Record 的 `HasThisBinding()` 始终返回 false，因此像下面这样的代码，this 其实指向的上一层 Environment Record，即 `globalThis` 对象：
```html
<script>
{
    console.log(this);
}
</script>
```

### Function Environment Record

Function Environment Record 是 Declarative Environment Record 的子类，并设计有额外的属性或函数，比如 `ThisValue`、`BindThisValue(V)`、`GetThisBinding()`。

我们还是关注 `HasThisBinding()` 的行为。

ECMAScript 规定，如果函数是箭头函数（`=>`），那么`HasThisBinding()` 返回 false，否则返回 true。从这一点上就能看到，ES6 引入箭头函数这一语法后，我们就具备了锁定 this 的能力，试看下面这个例子：

html

复制代码

`<script>     const person = {         say: () => {             console.log(this);         }     };     console.log(person.say()); // window </script>`

虽然函数 `say` 是在 `person` 对象上调用的，但是其 this 并不指向 `person`。即便使用 `Function.prototype.bind/call/apply` 函数尝试修改 this 也不行：

js

复制代码

`person.say.call("Hello"); // window person.say.apply("Hello", []); // window person.say.bind("Hello")(); // window`

关于 Function Environment Record 的其他属性和函数，我们在以后的章节中还会讲到，现在来看 Global Environment Record。

### Global Environment Record

Global Environment Record 也有自己专属的属性和函数，如 `GlobalThisValue`、`GetThisBinding()`。它的 `HasThisBinding()` 始终返回 true，因此在全局环境下，this 是有值的，浏览器下是 `window/globalThis`，Node.js 环境下是 `global`：

html

复制代码

`<script> console.log(this); // window </script>`

sh

复制代码

`$ node -e "console.log(this)"  # global`

### Module Environment Record

最后来看 Module Environment Record，它也是 Declarative Environment Record 的子类，提供了 `GetThisBinding()` 函数。

Module Environment Record 的 `HasThisBinding()` 始终返回 true，但是 `GetThisBinding()` 却始终返回 undefined，这样的效果就是：**在 ES Modules 里面的全局 this 始终是 undefined**。

js

复制代码

`// index.js import("./lib.js"); // lib.js console.log(this); // undefined`

这样的设计能够避免一些潜在的歧义，比如下面这段代码在顶层上下文中运行：

scss

复制代码

`function foo() {     console.log(this); } foo();`

但是如果不是 ES Modules，那么 this 指向将取决于是否是 `strict` 模式：

1. strict 模式下，this 为 `undefined`；
2. 非 strict 模式下， this 为 `window/globalThis`。

ES Modules 环境避免了这种歧义，this 始终是 undefined，不会意外地修改到全局的数据。

> 我们忽略了对 `Object Environment Record` 的讨论，因为它代表的 `with` 是不建议使用的。

我们总结一下可以发现：函数 `GetThisBinding()` 并非像 `HasThisBinding()` 定义在 Environment Record 中，而是被 Global Environment Record、Function Environment Record 和 Module Environment Record 各种子类分别实现的，也只有它们才有 this。

this 可以任意访问，最多也就是 undefined 而已，但普通变量则不一定，声明普通变量有着不一样的规则。

## 变量提升与 TDZ

在 ES6 之前，我们只能用 `var` 来声明变量，我还记得有一条不成文的规矩是：**应该把所有 var 语句提到当前作用域的最前面**，后来被 `ESLint` 收录成 [vars-on-top](https://link.juejin.cn/?target=https%3A%2F%2Feslint.org%2Fdocs%2Flatest%2Frules%2Fvars-on-top "https://eslint.org/docs/latest/rules/vars-on-top") 规则。之所以要这样做，是因为 var 声明的变量具有提升的效果，也就是我们可以在声明之前访问到它，只不过值肯定是 **undefined**。

js

复制代码

`console.log(foo); // undefined var foo = "hello";`

把 var 声明提到最上面的初衷是想让开发者对上下文数据环境有明确的预期，不会不小心访问到未经过初始化的变量，导致带来意外的错误。

var 声明的变量也确实呼应了前面对于 “JavaScript 没有块级作用域”的特征，一个大括号根本无法阻止 var 的作用范围：

js

复制代码

`{     var foo = 100; } console.log(foo); // 100`

甚至是一个 `try...catch` 语句：

js

复制代码

`try {     var foo = 8;     throw Error(); } catch {     var bar = 9; } console.log(foo, bar); // 8 9`

`for` 语句亦如此：

js

复制代码

`for (var i = 0; i< 5; ++i); console.log(i); // 5`

所以很容易一个不小心就会造成变量的冲突。为了解决这个问题，ES6 引入了 `let` 和 `const` 关键字来声明具有块级作用域的变量，它们的区别就是一个的值可变，一个不可变。

我们以 let 为例：

js

复制代码

`{     let foo = 100; } console.log(foo); // ❌ Uncaught ReferenceError: foo is not defined`

类似的，在 `try...catch` 和 `for` 语句中声明变量，外边均不能访问到，也杜绝了冲突的可能。

如果在 let 声明之前使用变量，则会触发未初始化异常：

js

复制代码

`console.log(foo); // ❌ Uncaught ReferenceError: Cannot access 'foo' before initialization let foo = 100;`

这里是一个面试常见的考点，只要你说出 `TDZ` 一词，基本上就算合格了。

**TDZ** 全称 Temporal Dead Zone，即`暂行性死区`。这个词语是一种约定俗成的说法，因为你**在 ECMAScript 的官方规范文档中根本找不到对 TDZ 的表述**。

要理解 TDZ，我们可以从 ECMAScript 规范中对 `var` 和 `let/const` 的定义来一窥究竟：

|var|let/const|
|---|---|
|A var statement declares variables that are scoped to the running execution context's VariableEnvironment. **`Var variables are created when their containing Environment Record is instantiated and are initialized to undefined when created.`** Within the scope of any VariableEnvironment a common BindingIdentifier may appear in more than one VariableDeclaration but those declarations collectively define only one variable. A variable defined by a VariableDeclaration with an Initializer is assigned the value of its Initializer's AssignmentExpression when the VariableDeclaration is executed, not when the variable is created.|let and const declarations define variables that are scoped to the running execution context's LexicalEnvironment. **`The variables are created when their containing Environment Record is instantiated but may not be accessed in any way until the variable's LexicalBinding is evaluated.`** A variable defined by a LexicalBinding with an Initializer is assigned the value of its Initializer's AssignmentExpression when the LexicalBinding is evaluated, not when the variable is created. If a LexicalBinding in a let declaration does not have an Initializer the variable is assigned the value undefined when the LexicalBinding is evaluated.|

它们之间最关键的区别体现在第二句话上面。**var 声明的变量在 Environment Record 初始化的时候就被赋值为 undefined，而 let/const 是直到词法绑定（`LexicalBinding`）被执行才可以被访问**。

什么是词法绑定？我们可以简单地理解为就是 let/const 所在的那一句代码。不到这一句，都不可以访问变量，这也解释了为什么在 let/const 声明之前不可以使用变量的现象。

因此，虽然规范没有 TDZ 这个术语，但是我们可以给它做这样的定义：`TDZ 就是作用域初始化到执行 let/const 语句之间的这段区域`。总之一句话：不可以在声明之前使用。我们应该始终开启 ESLint 的 [no-use-before-define](https://link.juejin.cn/?target=https%3A%2F%2Feslint.org%2Fdocs%2Flatest%2Frules%2Fno-use-before-define "https://eslint.org/docs/latest/rules/no-use-before-define") 规则。

## 小结

作用域与变量声明是 JavaScript 乃至任何编程语言的基础。JavaScript 相对更特殊一些，初期它不具备块级作用域的能力，直到 ES6 引入了 `let/const`，但是也有了业界常用的 `TDZ` 概念。

在如今的 ECMAScript 规范中，以 `Environment Record` 术语来代指作用域或者上下文的概念，并派生出了不同的子类，不同子类对 `this` 的支持不尽相同，有的压根不支持（如 Declarative Environment Record），有的算支持但一直是 undefined（如 Module Environment Record），有的则还需要考虑额外的条件（如 Function Environment Record），有的则无条件支持（如 Global Environment Record）。

无论哪种 Environment Record，都有 `OuterEnv` 属性，不过不同种类也不同，比如 Module Environment Record 的 `OuterEnv` 一直是 Global Environment Record；而 Global Environment Record 的 `OuterEnv` 必定是 null。所有的 Environment Record 则组成了一个**树形结构**。

我没有专门提`闭包`的概念，因为它并没有什么特殊的，像下面这个例子一样，animalFactory 创建的 Animal 函数中（**Env1**）可以自由访问 animalFactory 的作用域（**Env2**），而 **Env2** 再也无法通过其他方式访问得到，相当于是“封闭的”，但又是对 Animal 开放的。

js

复制代码

`function animalFactory(home) {     // Env2     return function Animal() {         // Env1         this.address = home;     } }`

学习完变量的声明，下一节我们来关注变量的类型。