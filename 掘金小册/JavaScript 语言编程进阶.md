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

```html
<script>
    const person = {
        say: () => {
            console.log(this);
        }
    };

    console.log(person.say()); // window
</script>
```

虽然函数 `say` 是在 `person` 对象上调用的，但是其 this 并不指向 `person`。即便使用 `Function.prototype.bind/call/apply` 函数尝试修改 this 也不行：

```javascript
person.say.call("Hello"); // window
person.say.apply("Hello", []); // window
person.say.bind("Hello")(); // window
```


关于 Function Environment Record 的其他属性和函数，我们在以后的章节中还会讲到，现在来看 Global Environment Record。

### Global Environment Record

Global Environment Record 也有自己专属的属性和函数，如 `GlobalThisValue`、`GetThisBinding()`。它的 `HasThisBinding()` 始终返回 true，因此在全局环境下，this 是有值的，浏览器下是 `window/globalThis`，Node.js 环境下是 `global`：

```html
<script>
console.log(this); // window
</script>
```

```shell
$ node -e "console.log(this)"  # global
```

### Module Environment Record

最后来看 Module Environment Record，它也是 Declarative Environment Record 的子类，提供了 `GetThisBinding()` 函数。

Module Environment Record 的 `HasThisBinding()` 始终返回 true，但是 `GetThisBinding()` 却始终返回 undefined，这样的效果就是：**在 ES Modules 里面的全局 this 始终是 undefined**。

```javascript
// index.js
import("./lib.js");

// lib.js
console.log(this); // undefined
```

这样的设计能够避免一些潜在的歧义，比如下面这段代码在顶层上下文中运行：

```javascript
function foo() {
    console.log(this);
}

foo();

```

但是如果不是 ES Modules，那么 this 指向将取决于是否是 `strict` 模式：

1. strict 模式下，this 为 `undefined`；
2. 非 strict 模式下， this 为 `window/globalThis`。

ES Modules 环境避免了这种歧义，this 始终是 undefined，不会意外地修改到全局的数据。

> [!info]
>我们忽略了对 `Object Environment Record` 的讨论，因为它代表的 `with` 是不建议使用的。

我们总结一下可以发现：函数 `GetThisBinding()` 并非像 `HasThisBinding()` 定义在 Environment Record 中，而是被 Global Environment Record、Function Environment Record 和 Module Environment Record 各种子类分别实现的，也只有它们才有 this。

this 可以任意访问，最多也就是 undefined 而已，但普通变量则不一定，声明普通变量有着不一样的规则。

## 变量提升与 TDZ

在 ES6 之前，我们只能用 `var` 来声明变量，我还记得有一条不成文的规矩是：**应该把所有 var 语句提到当前作用域的最前面**，后来被 `ESLint` 收录成 [vars-on-top](https://link.juejin.cn/?target=https%3A%2F%2Feslint.org%2Fdocs%2Flatest%2Frules%2Fvars-on-top "https://eslint.org/docs/latest/rules/vars-on-top") 规则。之所以要这样做，是因为 var 声明的变量具有提升的效果，也就是我们可以在声明之前访问到它，只不过值肯定是 **undefined**。

```javascript
console.log(foo); // undefined
var foo = "hello";
```

把 var 声明提到最上面的初衷是想让开发者对上下文数据环境有明确的预期，不会不小心访问到未经过初始化的变量，导致带来意外的错误。

var 声明的变量也确实呼应了前面对于 “JavaScript 没有块级作用域”的特征，一个大括号根本无法阻止 var 的作用范围：

```javascript
{
    var foo = 100;
}

console.log(foo); // 100
```

甚至是一个 `try...catch` 语句：
```javascript
try {
    var foo = 8;
    throw Error();
} catch {
    var bar = 9;
}

console.log(foo, bar); // 8 9
```

`for` 语句亦如此：
```javascript
for (var i = 0; i< 5; ++i);
console.log(i); // 5
```

所以很容易一个不小心就会造成变量的冲突。为了解决这个问题，ES6 引入了 `let` 和 `const` 关键字来声明具有块级作用域的变量，它们的区别就是一个的值可变，一个不可变。

我们以 let 为例：
```javascript
{
    let foo = 100;
}
console.log(foo); // ❌ Uncaught ReferenceError: foo is not defined
```

类似的，在 `try...catch` 和 `for` 语句中声明变量，外边均不能访问到，也杜绝了冲突的可能。

如果在 let 声明之前使用变量，则会触发未初始化异常：
```javascript
console.log(foo); // ❌ Uncaught ReferenceError: Cannot access 'foo' before initialization
let foo = 100;
```

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

```javascript
function animalFactory(home) {
    // Env2
    return function Animal() {
        // Env1
        this.address = home;
    }
}
```

学习完变量的声明，下一节我们来关注变量的类型。

# 基础篇|如何合理地判断变量的类型?

作为动态语言，JavaScript 在变量声明时无需指定类型，因此你可以编写非常灵活的代码。但“不需要”不代表“没有”，在使用时肯定免不了要判断变量的类型，比如实现一个加法函数 `add(a, b)`，变量是数字还是字符串，显然最后的结果就很不一样。更多的时候我们要做的是对输入数据的合法性校验，不合法直接抛出错误。

**类型判断**在面试活动中，很可能影响到面试官对你表现的判断，比如一些题目的边界问题、健壮性问题等加分项；在日常开发中，影响到的是你编写业务代码的质量，最终体现的是你的 Bug 数量，甚至是领导对你能力高低的判断。

大家可能觉得危言耸听，既然如此，是不是用 TypeScript 就变成强类型语言了呢？

这显然是一个误解。没错，TypeScript 是带有类型系统，其实不仅仅 TypeScript，Facebook 也有一个更早实现的类型注解系统：[flow](https://link.juejin.cn/?target=https%3A%2F%2Fflow.org%2F "https://flow.org/")，用在 React 项目中。无论哪一个，都没有把 JavaScript 变成强类型语言，它们只是对变量做一个约束，提醒你应该处理哪些种类型，比如 TypeScript 中的一个变量，依然可以声明成多类型混合的：

```typescript
let foo: string | number;
```

在运行时仍然需要判断它是 string 还是 number 的，如果是 string，那么就可以调用 concat 方法；如果是 number，就可以调用 toFixed 方法。

本章我们就来探讨 JavaScript 中如何判断变量的类型，大家一定先想到了 `typeof`，不过不要着急，我们先来明确一共有哪些类型需要判断，然后再去看 typeof 是不是我们期望的判断方法。

## ECMA262 中的类型

到 ES2023 为止，规范一共定义了 7 种变量类型：**Undefined、Null、Boolean、String、Symbol、Numeric 和 Object**。除了 Object 之外，其他都称为 `Primitive` 类型。

怎么理解 Primitive 呢？简单来说就是可以用字面量一眼能看出其值的，比如 null、undefined、100、"中国"、false。虽然 Object 有的可以用 JSON 结构来表示，但是稍微复杂一点，比如函数、原型链、循环引用等这些特性是无法表述的。另一种理解 Primitive 的方式是值与引用。在作为函数参数传递时，Primitive 变量都是通过拷贝的方式传递的，修改一处，另一处不会被影响；而 Object 类型是传递引用的，一处修改，全局可见。

Numeric 又可以向下划分成 `Number` 和 `BigInt`，所以我们也可以说 ECMA262 定义了 8 种类型，如下图所示：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fb8376732aba4a5784095066cb86fa6c~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

这可能和大家的印象中的认知有冲突，一定有人会问：“我经常要判断的函数 Function 不是一种类型吗？”

答案是，ECMA262 规范中的函数确实不是独立的类型，而是一类特殊的 Object，能执行（execute）。不但函数不是一种独立类型，我们经常使用的数组 Array、日期 Date、参数 Arguments、正则 RegExp 等都不是，它们通通都只是 Object。

所以，这里就引出了一个矛盾：`规范中定义的类型不一定是我们想要的，而我们想要的，规范中也并不一定做了区分`。这就需要我们开发者来自行实现，本章的重点就是**通过理论联系实践，面向日常的真正需求来实现尽可能可靠的类型判断逻辑，提升大家编写更安全、更健壮代码的能力**。

一般来说，我们最常需要做类型判断的是未定义 undefined、空 null、字符串 string、数字 number 或 bigint、布尔 boolean、符号 symbol、数组 Array、函数 Function、正则 RegExp，除此之外，都可归类为普通对象。

## typeof 的能力

typeof 是做类型判断绝对避不开的一个概念，应该说大家在入门 JavaScript 最开始都会接触到它。然而我相信还有很多同学都对它有误解，把它当作一个函数，写作：

```typescript
typeof(foo)
```

这样在语法虽然是合法的，不过 **typeof 却不是函数，而是一个操作符**，你不可以声明一个叫做 typeof 的变量：

```typescript
var typeof = 3 // ❌ Uncaught SyntaxError: Unexpected token 'typeof'
typeof typeof  // ❌ Uncaught SyntaxError: Unexpected end of input
```

typeof 返回值一定是这 8 种字符串之一："undefined"、"string"、"boolean"、"number"、"bigint"、"symbol"、"object" 或 "function"。

可见这 8 种类型和前面我们讲到的规范定义的 8 种类型并不是一一对应的。

**首先是空类型 null 用 typeof 判断不出来，这是 Primitive 类型中唯一一种 typeof 不支持** **的**。据说这是 JavaScript 当年的设计者引入的一个 Bug，由于兼容性的考虑一代代传承了下来，以至于 20 多年后我们编写下面的代码仍然是不安全的：

```javascript
if ("object" === typeof variable) {}
```

好在不使用 typeof 也可以安全、简单地判断 null：

```javascript
 if (foo === null) {}
```

**其次，函数 Function 类型得到了 typeof 的单独支持，极大方便了我们日常使用。**

明确了以上两点之后，我们就可以清晰地得到下面这张图，牢记这张图，你可以非常灵活地运用 typeof。

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cafa665352064c34aa57bae148564069~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

总体来说，除了 null 有点特殊以外，typeof 在判断 Primitive 类型变量上的能力还是可圈可点的。其实，在日常使用中，undefined 也有一些额外的故事。

## undefined 的特殊之处

一般来说，在我们的认知当中，只有`typeof undefined`才会返回`"undefined"`：
```typescript
if ("undefined" === typeof variable) {}
```

有人可能会问到，和`undefined`做全等判断岂不是更简单：

```typescript
if (undefined === variable) {}
```

大部分场景下是可以的，但有例外。

在 ES5 之前的 ES3 时代，全局的 undefined 是能被重写的，比如`undefined=1`是可以被正确执行的。

这一现象从 ES5 开始得到了改进，你现在可以打开 Chrome 的开发者工具，在控制台中输入`undefined=1`，然后用`console.log(undefined)`打印出来，会发现 undefined 的值并没有变化。事实上，在 strict 模式（后面章节会讲到）下，下面的代码会直接抛出 TypeError 错误，告诉你 undefined 是只读的：

```typescript
"use strict";
undefined = 1; // ❌ Uncaught TypeError: Cannot assign to read only property 'undefined' of object '#<Window>'
```

虽然现代浏览器已经没有了这个顾虑，但是 undefined 还不是关键字，它依然可以作为变量名在局部上下文中声明：

```javascript
{
    const undefined = 1;
    console.log(typeof undefined); // "number"
}
```

显然在这个上下文中用全等`===`来判断，是不可以得出期望的结果的。因此，为了保守起见，我们在判断一个变量是否是 undefined 的时候，推荐的写法还是`"undefined" === typeof variable`。ESLint 有一条规则 [no-undefined](https://link.juejin.cn/?target=https%3A%2F%2Feslint.org%2Fdocs%2Flatest%2Frules%2Fno-undefined "https://eslint.org/docs/latest/rules/no-undefined") 就是应对这件事的。

戏剧性的是，在这个例外之中，还有例外。

ECMA262 自 1999 年发布 ES3 到 2009 年发布 ES5 一共有接近 10 年时间的断层，在这期间微软的 IE 浏览器占据了绝大部分市场份额，其 JavaScript 的 API 实现了很多标准之外的特性，现在我们就遇到了这样一个例子。

IE 在`document`对象上有一个属性`all`，会返回当前页面中的所有元素，它是可遍历的：

```javascript
for (const element of document.all) {
    console.log(element.tagName);
}
```

然而它在早年间却有着更重要的使命——判断是否是 IE 浏览器：

```javascript
if (document.all) {
    // IE
} else {
    // not IE
}
```

因为只有 IE 浏览器实现了这样一个非 W3C 标准的 API。后面的浏览器虽然实现了`document.all`的数据结构，但是却有着完全不一样的 typeof 表现和布尔值表现：

```javascript
typeof document.all // "undefined"

if (document.all) {
    // never enter
}
```

是的，虽然 document.all 不是 undefined，但它在 typeof 下却表现得像 undefined，而且在逻辑上是假值。这一切都是为了不破坏过去编写的网站代码，ECMA262 专门为其抽象了一个叫做 [([IsHTMLDDA])](https://link.juejin.cn/?target=https%3A%2F%2Ftc39.es%2Fecma262%2F%23sec-IsHTMLDDA-internal-slot "https://tc39.es/ecma262/#sec-IsHTMLDDA-internal-slot") 的概念。

## 典型对象的判断

typeof 基本能解决 Primitive 变量的判断，我们还有几个典型对象类型的判断需求，比如数组 Array 和正则 RegExp。

先来看数组。

在早年间，由于 IE 环境下跨 iframe 调用时，`[] instanceof Array`不成立，所以业界普遍推荐的判断方法是`Object.prototype.toString.call(variable) === "[object Array]"`，其实你也可以使用`variable.constructor === Array`。

后面，ES5 引入了 `Array.isArray` 静态方法，如果不考虑 IE 环境的话，它又有什么优势呢？

答案是 **Array.isArray 比 instanceof 或者 constructor 能胜任对 Proxy 的判定工作**。我们不妨看下面一段代码：

```typescript
const proxy = new Proxy([], {
    get(target, p) {
        if ('constructor' === p) return String;
        return Reflect.get(target, p);
    },
    getPrototypeOf() {
        return null;
    }
});

console.log(`Array.isArray(proxy)`, Array.isArray(proxy)); // true
console.log(`proxy instanceof Array`, proxy instanceof Array); // false
console.log(`proxy.constructor === Array`, proxy.constructor === Array); // false
```

Proxy 的知识在后面的章节中还会讲到。在上面的例子中，我们篡改了 Proxy 对象的 constructor 和原型链，这使得通过 constructor 或者 instanceof 的方式判断类型的尝试都失效了，唯独 `Array.isArray` 仍然能正常工作，这就是它的优势。

因此，**无论是从使用便利性上来说，还是从能力范围上来讲，都更建议使用 `Array.isArray` 来判断数组类型**。

其他对象类型就没有这种待遇了，比如我们常用的正则 RegExp。除了它有自己独立的字面量语法之外，RegExp 没有其他任何特别之处。假设我们声明一个自定义类：

```typescript
class Animal {}
```

那么，实现 isAnimal 的原理和实现 isRegExp 的原理是等价的。那我这里使用 Animal 来代指任意对象类型，包括 RegExp、Date、Arguments，也包括 Window、Document。

通常有两种方法来做判断。

第一种办法，判别其构造函数，不过对象的 constructor 属性一般是可以被覆写的，因此有被伪造的可能：

```typescript
function isAnimal(variable) {
    return variable?.constructor === Animal;
}
```

第二种办法，用 instanceof 做原型链判别，不过对象的原型链也是可以被篡改的：

```typescript
function isAnimal(variable) {
    return variable instanceof Animal;
}
```

如果你在类中定义了这样一个特殊属性：
```typescript
class Animal {
    get [Symbol.toStringTag]() {
        return 'Animal';
    }
}
```

那么也有第三种办法，使用对象基类的 toString 方法：

```typescript
function isAnimal(variable) {
    return Object.prototype.toString.call(variable) === "[object Animal]";
}
```

Array、RegExp、Date、Arguments、Window、Document 甚至 undefined、null 都可以应用这种办法。不过显然字符串对比的方式更容易被篡改，你可以轻松定义一个伪装类来骗过这个判断。

以上三种办法都可以被骗过，那是不是没有完美的办法来判断对象变量类型呢？我认为是的，“animal 是 Animal 类型”这句话本身就是需要被定义的，什么样叫做是，一定是需要条件的，是被 Animal 构造出来就是，还是原型链相关就是呢？

遗憾的是，JavaScript 中的构造函数，甚至 class 语法本身都是语法糖，原型链基本上可以被任意修改，因此可以说，在对象类型的判断上，本身就没有严格的定义，只要不涉及安全攻防，按照你自己认可的方式实现即可，不必在完备性上过于执着。

这种不完美也会传导到 Primitive 类型的变量上。

## Primitive 的对象封装

除了 null 和 undefined 外，其余的 Primitive 类型都可以封装成 Object：

```javascript
Object(123)             // 等价于 new Number(123)
Object(123n)            // 等价于 new BigInt(123n)
Object("str")           // 等价于 new String("str")
Object(true)            // 等价于 new Boolean(true)
Object(Symbol("sym"))
```

> [!info]
> 有 Java 语言背景的同学应该对装箱/拆箱的概念不陌生，和这相比是极其类似的。

虽然这些值在 typeof 下一定都返回 “object”，但是却仍然有着原本的语义，比如：

```javascript
new Number(3) + new Number(4) // 7
new String("a") + new String("b") // "ab"
```

如果你想实现一个 `concatString(a, b)` 函数，a 和 b 除了应该是 string 类型之外，也许你也想兼容一下字符串对象，这是非常常见的需求，那么 `isString` 函数的逻辑就应该分成两个部分：

```javascript
function isString(str) {
    return "string" === typeof str || Object.prototype.toString.call(str) === "[object String]";
}
```

这也是著名的 lodash.isString 实现的主要原理。同样的道理，对于 number、boolean、symbol 也是适用的，唯一需要特别关注的是，布尔对象在逻辑语义上始终为真：

```javascript
if (new Boolean(false)) {
    // enter
}

if (new Boolean(true)) {
    // enter
}

new Boolean(new Boolean(false)).valueOf() // true
```

## 小结

到此为止，我们了解了 JavaScript 类型系统的关键知识，ECMA262 定义的 8 种变量类型，和我们日常开发常常需要分辨的并不完全一致，typeof 也不能满足，所以必须增加对特定对象类型的判定。

总结来看，根据不同的目标类型，可以采取如下的判定方法：

1. 用全等`===`来判断 null 类型；
2. 用 `typeof` 来判断其他 Primitive 类型，注意 `document.all` 的例外；
3. 用 `Array.isArray`来判断数组类型；
4. 没有完美的特定对象类型判断方法，可以酌情选择 constructor、instanceof 或者 toString。

Primitive 的对象封装类型也可以被使用，以增加代码的兼容性和能力范畴，但是也需要额外添加像上面第 4 点的对象类型判断逻辑才行，注意这个逻辑是可以被绕过的。
# 基础篇|字符串:揭开字符序列的深层奥秘

**字符串（String）应该是 JavaScript 所有 8 种类型中最常见的一种**。我们经常会去组装、比较、截取、分解、搜索字符串，当然最常见的莫过于获取字符串长度。我相信大家都或多或少接触过下面这些需求：

1. 限制输入框中的内容长度，保证在具体的位置能够显示得下；
2. 拼接服务端的数据为一个阅读良好的提示语，展现给用户，比如“操作失败，错误码为XXX”；
3. 在固定格式中舍弃一部分信息，保留剩余内容，比如在身份证号中获取生日信息；
4. 在用户输入数据或者服务端数据中替换关键词，比如将“您`${0}`收入为`${1}`”的占位符 “`${}`” 替换成有效数据。

这些操作看似非常基础，但是这些年来受到 Unicode、Emoji 快速扩展，以及 ECMA262 规范新增 API 的影响之下，已经产生了很多在使用、认知上的误区，即便这些基础操作也可能产生错误的结果，比如文本经处理后产生乱码，又或者展示超出了显示区域。

要捋清字符串常用操作的关键点，减少处理出错的可能，首先我们要从认识其基本结构开始。

## 基本结构

简单来说，**字符串可以定义为以“字符”为基本单位的有序序列**。但是这里的字符却并不是我们直觉意义上能看到的一个文字符号，比如英文字母“A”，或者汉字“中”，亦或是标点符号“·”，而是一个叫做“**码元（Code Unit）** ”的东西，占据 16bit，即 2byte 空间。

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5d9d7c8ad8b4404fae468edaac27c12e~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

我们应该知道，无论什么数据在计算机内容中都是二进制的数字表示的，因此一个码元就是一个 16 位无符号整数，最大为 0xFFFF。这个数字是需要映射到可见的、有意义的文字符号的，这个映射关系本来是由字符集 Unicode 来负责的，比如数字 65 映射为大写英文字母“A”，数字 0x4E2D 映射为汉字“中”，数字 0x20BFF 映射为“𠯿”。但是，通常来说一个字符的 Unicode 编码值不适合用来直接存储和传输，原因包括：

1. 纠错问题，即便错了一二进制位，就会变成另一个合法的字符；
2. 前导问题，一个字符的编码值可能是另一个字符的前面一部分。

因此，Unicode 编码值会通过算法再次编码转换成另一个数字，来进行存储和传输。这个过程称为**字符编码**，我们常见的字符编码比如 GBK、GB2312、UTF-8 等等。这个编码过程虽然会浪费一部分空间，但是却拥有了一定的纠错能力，并且规避了前导问题，使得应用程序可以高效、无误地读取。

回到字符串的结构上来，JavaScript 的码元就是经过字符编码后的整数，只不过这个编码并不是我们耳熟能详的，而是一个叫做 `UTF-16` 的编码方案，这是历史原因造成的。相较于 UTF-8 编码后可能是 1byte、2byte、3byte、4byte 而言，大家注意，UTF-16 编码后只可能是 2byte 或 4byte 的。

下图展示了文字符号是如何转换成码元的过程：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ff6cca1ef064bc2815f43c4fa72b915~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

由于一个码元固定是 2byte 的，于是**一个 Unicode 字符在 JavaScript 字符串中可能占据 1 个码元，也可能占据 2 个**。

记住这个结论，它将给我们后面要讲到的很多字符串操作带来麻烦，首当其冲地，就是获取字符串长度。

## 获取长度

获取字符串长度最简便的方法莫过于访问其 length 属性：
```javascript
"我爱学习ABC。".length // 8
```

拿到 length 的目的通常有下面两种：

1. 受到数据库的存储容量限制，推算占用字节数；
2. 推算可能的渲染宽度，做截断，避免显示不下。

先来看第一个问题，如何计算一个字符串的占用字节数呢？首先一个码元占用 2byte 这是固定的规则，我们只需要知道字符串含有几个码元即可。好在 length 的定义很清晰，就是码元的个数，一个码元 2 字节，最终字符串的内存占用就是 `length × 2byte`。下面这个例子中，字符串“我是中国人”占用 10 字节。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d3a48f1bf75341ba9582a05023f1ffae~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

一个字符串最多包含253−1253−1个码元，这也是 JavaScript 所能表示的最大整数，即 `Number.MAX_SAFE_INTEGER`。

再来看第二个问题，好多人都还在用 length 来推算文本在页面上的显示宽度。由于英文字母、标点符号和汉字的显示宽度明显不一样，因此有人做了改进，判断字符是否为汉字，如果是则算作两个英文字母的宽度。

显然这样的做法是非常粗糙的，英文字母本身也不是等宽的，再说汉字也不像字母文字那样符号数量是有限的，数以万计的汉字，再加上类似的韩文、日文，几乎没有完美的方法可以覆盖全面。即便只考虑常用汉字，那么也无法枚举其他语言，比如阿拉伯文。下图显示了不同字符的自身显示宽度，可见很难找到什么规律。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8a62a8ecc7bf44dc88bfa888f3b6ebf0~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

因此，**建议大家不要依赖 length 来推断显示宽度，视觉问题还得依赖 CSS 解决才更健壮**。如果你认为这些特殊字符不常见，不必考虑，那么我们看下面这个例子：

```javascript
"🧑🏽‍🚒".length // 7
```

不同于他国文字，Emoji 在汉语环境中会常常被使用，来辅助表达语气、心境。很多人第一次看到上面的结果都会特别震惊，明明是一个字符，为什么 length 会等于 7 ？

我们先以更简单一点的例子入手，汉字“𠯿”的 length 等于 2，也是反直觉的：
```javascript
"🧑🏽‍🚒".length // 7
```

在上面一小节中，我们得出一个结论，Unicode 字符在 JavaScript 的 UTF-16 编码中，会占据 2byte 或者 4byte 空间，也即 1 码元或者 2 码元。而 length 反映的是码元的数量，所以“𠯿”就是占据 4byte 的那一种，在上一小节中我们已经看到了它经过 UTF-16 编码后是 0xD842DFFFF，就是 2 个码元。

Unicode 的编码区间是 0x0000～0x10FFFF，其范围是 1 码元所能表示范围（0x0000~0xFFFF）的 17 倍之多。因此，有相当多的字符在 JavaScript 中都必须编码成 2 码元才行。但是应该最多也就 2 码元了，刚刚那个 Emoji 返回的 7 是怎么回事呢？

其实这是由于 Emoji 的一个叫做“修饰序列（Modifier Sequence）”的功能造成的：在一个基础的 Emoji 编码后面，添加修饰符的编码，中间用 0x200D 来分隔，这样产生的一系列码元，只会显示成一个 Emoji 外观。

“🧑🏽‍🚒”实际是这样几个字符共同组成的：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7d9fedf43fe94fea87a44ec85ad9978c~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

其中，第一个 Unicode 字符 0x1F9D1 占据了 2 个码元，这是一个女性面孔“🧑”，修饰符 0x1F3FD 代表一种深色皮肤，无法单独显示，也占据 2 个码元。紧跟着一个固定的 0x200D 分隔符，占 1 码元，后面还是一个占据 2 码元的修饰符 0x1F692，即“🚒”消防车。这样最后产生的就是一个深色皮肤的女消防员，共占据 7 个码元，14 字节。

修饰序列功能大大拓展了 Emoji 所能表示的范围，但是也进一步破坏了 length 与展示字符之间的关系，大家就更不要以 length 来推断文本的展示宽度了。

修饰序列和双码元字符共同作用，使得字符串对于字符位置的操作更敏感，如果不从正确的位置读取，那么很可能读取到的是无意义的字符碎片，几乎一切基于位置的字符串操作都会被影响到，典型的如字符串截取。

## 字符串截取

截取字符串可分为`截取单个字符`和`截取片段`，前者需要一个下标参数，后者需要 start 和 end 两个下标参数。

注意，这里的“截取”并非指修改原有字符串的字符序列。像很多高级语言一样，JavaScript 中的字符串也是不可变的，任何看似修改其内容的操作，无一例外都是拷贝返回新的字符串实例，比如 slice、replace 等等。我见过有人尝试这样去修改其中某个字符：

```javascript
var str = "我在工作"
str[1] = "不"
console.log(str) // "我在工作"
```

结果是显而易见的。其实在 strict 模式下，上面的代码是会抛出异常的。

### 截取字符

先来看截取单个字符，如果把字符串看作是一个字符数组的话，那么一个下标就能唯一定位一个字符：

```javascript
"我是中国人"[1] // "是"
```

不过，JavaScript 的字符串还提供了另外两个方法：charAt 和 at。这三种方法仅在细节上有差异：

1. **数组下标**，如果参数在 `[0, length - 1]` 之外的，返回 undefined，如`"我是中国人"[5] === undefined`；
2. **charAt 函数**，如果参数在 `[0, length - 1]` 之外的，返回空串，如`"我是中国人".charAt(5) === ""`；
3. **at 函数**，如果序号是负数，则会加上字符串的 length 作为新的序号，如`"我是中国人".at(-1) === "人"`，但如果新序号不在有效范围之内（小于 0 或者大于 `length - 1`），则会返回 undefined

> [!info]
> 注意：at 函数还比较新，从 Chrome 92、Firefox 90，Safari 15.4 才开始支持。

以上这三种方法的参数都是指码元的位置，因此都会受到双码元的影响，比如我们想截取“𠯿a”中的“a”，由于前面为双码元，导致“a”处于第三个码元中，这时候无论你用上面哪种方法，传入下标 1 都不能得到“a”，只有传入 2 才可以：
```javascript
"𠯿a"[2]        // "a"
"𠯿a".charAt(2) // "a"
"𠯿a".at(2)     // "a"
```

而你如果想得到双码元“𠯿”，那么上面任何方法都不能实现，因为它们都只能返回一个码元的内容，必须这样遍历：
```javascript
Array.from("𠯿a")[0] // "𠯿"
[..."𠯿a"][0]        // "𠯿"
```

在有 Emoji 修饰序列的场景下，因为它不是一个 Unicode 整体，上述任何方法都无法工作：

```javascript
Array.from("🧑🏾🎓a")[0] // "🧑"
[..."🧑🏾🎓a"][0]        // "🧑"
```
### 截取片段

再来看截取片段。ECMA262 提供了两个推荐使用的字符串截取函数：**substring** 和 **slice**，使用方式很类似，比较容易混淆。

它们的参数都是 `[startIndex, endIndex)`，注意是半开区间，区别在于：

1. substring 允许 endIndex<startIndex，会自动交换这两个值，而 slice 不允许，会返回空串；

```javascript
"我是中国人".substring(3, 2) // "中"
"我是中国人".slice(3, 2)     // ""
```

2. substring 的参数如果是负数，会当作 0 处理，slice 对负数会加上字符串长度。
```javascript
"我是中国人".substring(-2, -1) // ""
"我是中国人".slice(-2, -1)     // "国"
```

使用哪个取决于你的习惯，二者在能力上基本是等价的。它们同样也都会受到双码元和 Emoji 修饰序列的影响：

```javascript
"🧑🏾🎓a".substring(0, 2) // "🧑"
"🧑🏾🎓a".slice(0, 2)     // "🧑"
"🧑🏾🎓a".substring(7)    // "a"
"🧑🏾🎓a".slice(7)        // "a"
```

如果你想通过截取的方式来实现省略号的效果，那么很有可能得到“莫名其妙”的结果：

```javascript
"今天天气晴朗🧑🏾🎓！".slice(0, 9)  // "今天天气晴朗🧑\uD83C"
"今天天气晴朗🧑🏾🎓！".slice(0, 10) // "今天天气晴朗🧑🏾"
```

目前还没有特别好的简单办法来避免这种意外发生，只能根据修饰序列的规范去尽可能识别它，进而避免拆散它。

## 小结

可见，不仅仅是通过 length 获取字符串长度，还是用 charAt、at、substring、slice 截取字符串，所传入的参数都是指以**码元**为最小单位的元素位置，而不是 Unicode 字符的位置，更不是你看到的文字符号的位置。这一特点还作用于更多的字符串操作上，比如：

1. 用 charCodeAt、codePointAt 获取字符 Unicode 值；
2. 用 fromCharCode、fromCodePoint 来构造字符串；
3. 用 indexOf、lastIndexOf 来搜索子串；
4. 用 split 来分解字符串；
5. 用 >、< 来比较大小；
6. 用 match、search、replace 来匹配、搜索和替换子串。

可以说几乎全部字符串的操作都是基于码元结构的，在双码元字符、Emoji 修饰序列的影响下就形成了对三种不同字符概念的认知，一是码元字符，二是 Unicode 字符，三是可见的符号字符。大家需关注不同场合下的字符定义，以制定相应的应对之策。

# 基础篇|高效、优雅地利用正则表达式

在遇到模糊搜索、替换这类字符串高级操作的需求时，我们第一时间就会想到正则，这也成为了一名合格程序员的必备基础能力。**在 JavaScript 中，使用正则既可以从 RegExp 对象入手，也可以从 String 的一些方法入手**。它们之间有差异，但也有不少共性。在解决一个问题的时候，往往有不止一种方式。

本章我们就来学习如何从不同的需求出发，选择最简单、高效的正则使用方法。首先，我们来看如何构造一个正则。

## 构造正则对象

在 JavaScript 中构造正则对象有两种方法：一种是表达式语法，如 `/.+/`；一种使用 RegExp 构造函数，如 `new RegExp(".+")`。

表达式语法用来声明静态的正则，里面的内容是写死的，就如同你声明了一个字符串常量、一个数字常量一样。

而构造函数可以指定更灵活的动态内容：

```javascript
const avaliableLetters = ["A", "K"].join("|");
new RegExp(`\d${avaliableLetters}\d`);

```

事实上，RegExp 不仅仅是一个构造函数，还是一个类型转换函数，这一功能和 String、Number、Boolean 是一样的。

而且，RegExp 用作类型转换函数的话，它仍然可以处理第二个参数 flags，就如同它是一个构造函数一样：

```javascript
RegExp("\w+", 'g') // /\w+/g
```

不过，这两种方式还是有着细微的差别，即在于传入参数本身就是 RegExp 且不存在 flags 的情况下，作为构造函数会返回一个新的 RegExp 对象，而作为类型转换函数则会直接把参数原封不动返回：

```javascript
const originReg = new RegExp(`\w{2}`, 'g');

const typeReg = RegExp(originReg);
const newReg = new RegExp(originReg);

console.log(originReg === typeReg); // true
console.log(originReg === newReg); // false
```

但是，只要传入第二个参数 flags，哪怕是空字符串，两者就都会返回新的 RegExp 对象了：

```javascript
const typeReg = RegExp(originReg, "");
const newReg = new RegExp(originReg, "");

console.log(originReg === typeReg); // false
console.log(originReg === newReg); // false
```

大多数场景下我们并不需要关心这种差别，因此构造正则的策略可以是这样的：

1. 如果是**静态内容**，那么建议用表达式的方式构造正则；
2. 如果是**动态内容**，则可以用类型转换的方式构造正则。

无论哪种方式，构造出来正则对象都是由两部分构成的：`source` 和 `flags`（修饰符）。如下：

```javascript
var reg = /\w\d/img;

console.log(reg.source); // "\w\d"
console.log(reg.flags);  // "gim"
```

**修饰符在字符串的搜索、替换上都起到了至关重要的作用，控制着许多 API 的行为结果**。随着 ECMA262 新规范的不断引入，近年来有一些新的修饰符被加入了进来，并且浏览器的支持也比较广泛，我们必须对其有所了解，才能正确、高效地使用正则。

## 修饰符

ignoreCase(i)、multiline(m)、global(g)是最基础的三个修饰符，ES6 又引入了三个，并且都会影响到匹配的结果。

### unicode（u）

unicode 修饰符允许匹配双码元字符。在过去，这样的写法也是支持的：

```javascript
"a".match(/\u61/)
```

但只针对于单码元字符，有了 u 修饰符，我们可以做到：

```javascript
var reg = /\u{20bff}/u;
"𠯿".match(reg) // ['𠯿', index: 0, input: '𠯿', groups: undefined]
```

如果没有 u 的支持，那么匹配双码元字符会非常别扭，几乎没有实际应用的价值：

```javascript
var reg = /\ud842\udfff/; // 不建议如此
"𠯿".match(reg) // ['𠯿', index: 0, input: '𠯿', groups: undefined]
```

### dotAll（s）

注意 dotAll 简写为 s，不要和正则中的 `\s` 混淆。dotAll 顾名思义是控制英文点号“.”的语义的。

默认“.”能匹配任何除了换行（U+000A、U+000D、U+2028、U+2029）以外的单码元字符，开启 dotAll 之后，连这四个特殊字符也被包含了进去，看这个例子：

```javascript
`
AB
CD
`.match(/A(.+)D/) // null

`
AB
CD
`.match(/A(.+)D/s) // ['AB\nCD', 'B\nC', index: 1, input: '\nAB\nCD\n', groups: undefined]
```

如果再开启 u 修饰符，那么“.”将可以匹配任何字符。

### sticky（y）

sticky 简写为 y，意为“粘连”。开启 y 后，正则将只从上一次匹配到子串的后面一个位置开始匹配，举例说明：

```javascript
"123456zy78".match(/\d{2}/g) // ['12', '34', '56', '78']
"123456zy78".match(/\d{2}/gy) // ['12', '34', '56']
```

第二个语句由于开启了 y，在“56”后面遇到字母“zy”与正则不匹配，将不再继续，因此比第一个少了一个匹配结果。

大家可能已经注意到了，g 和 y 是有一些语义重合的，如果不开启 g，那么 y 应该也没有意义。事实上，ECMA262 规定：如果开启了 y，那么 g 修饰符将会被忽略掉。

## 字符串搜索

RegExp 和 String 都有函数可以执行正则搜索操作。我把实际需求从简单到复杂分成三层，分别是：

1. 判断有无匹配；
2. 获取首个匹配；
3. 获取全部匹配。

从后向前是包容的关系，可以说我们只要掌握了最下面的那一种，所有场景都迎刃而解了，不过从执行效率和语义上来说，只要能满足需求，还是建议使用最简单、效率最高的那一种。

### 判断有无匹配

如果仅仅需要一个布尔值，那么 RegExp 的 `test` 函数就是不二之选：

```javascript
/\d{2}/.test("AB23CD87K") // true
```

如果 test 的参数不是字符串的话，它就会调用内部方法 ToString 来强制转换为字符串。注意这一步是可能抛出异常的，例如传入一个无法转换为字符串的 Symbol 实例：
```javascript
/\d{2}/.test(Symbol()) // ❌ TypeError
```

test 之于 RegExp 就如同 includes 之于 String，都是从左到右寻找匹配，没找到则返回 false。如果想知道匹配到的位置，String 有 indexOf，而 RegExp 仍然需要依赖 String 的一个函数，叫做 `search`。

String 的 search 方法传入一个正则，从左到右返回第一个匹配到的子串的首字母位置：

```javascript
"AB23CD87K".search(/\d{2}/) // 2
```

这一步已经带有首个匹配的位置信息了，如果不满足于只知道位置的话，那么就进入下一层需求——获取首个匹配的全量信息。

### 获取首个匹配

获取首个匹配，显然不需要 g 修饰符。这一层需求我们在 RegExp 和 String 各有方法可以实现，而且有意思的是，它们竟然是等价的：

```javascript
/\w{4}/.exec("What is your name?")  // ['What', index: 0, input: 'What is your name?', groups: undefined]
"What is your name?".match(/\w{4}/) // ['What', index: 0, input: 'What is your name?', groups: undefined]
```

注意，只有在没有 g 的条件下它们才等价，有兴趣的同学可以试一试加上 g 修饰符后它们有怎样的输出。

`exec` 和 `test` 是 RegExp 唯二的两个核心函数，而且它们之间也有着微妙的关系：test 正是依靠 exec 来实现的，甚至你也可以写出这样一个 test 出来。如下：

```javascript
// Custom test
RegExp.prototype.test = function (str) {
    return this.exec(str) ? true : false;
}
```

String 的 match 与 search 一样，都会尝试把参数转换成正则。它在 ECMA262 的定义中与 exec 复用了一部分逻辑分支，在没有 g 修饰符之下刚好是等价的。

不过大家可能发现了，exec 返回的数组不太寻常，除了它在 0 下标上有数据外，还在普通数组没有的其他属性（`index`、`input`、`groups` 等）上有数据。我们知道，数组本身也是对象，因此外挂其他属性是完全合法的，只是不太常见。**注意这里的 index 指的就是正则匹配到的子串在原始字符串中的位置，等同于上面 search 的返回值**。

为什么要使用这么奇怪的数据结构呢？可能有一些历史原因，不过就结果上来看也是可以理解的，如果我们想匹配正则中的一部分：


```javascript
/Wh([a-z]+)\b/.exec("What is your name?") //  ['What', 'at', index: 0, input: 'What is your name?', groups: undefined]
```
`

看到正则中的小括号了么，再看看结果，字串 “at” 出现在了数组的第二个位置上，如果你的正则有 N 个小括号匹配，那么 exec 的最终结果中就会按照顺序多出 N 个数据出来，这样的数据结构可扩展性更好一些。

> [!info]
> 我们过去常常使用正则的 `$1`、`$2`、`$3`...属性来拿到这些值，但这些都不是标准属性，不提倡被使用。

接下来我们提高需求难度，来到第三层。

### 获取全部匹配

如果只是想简单地拿到所有匹配到的子串，那么使用 String 的 `match` 就好了，只不过不同于刚才，这一次我们必须带上 g（和 y） 修饰符才行：

```javascript
"Contributors are respected.".match(/\w{4}/g) // ['Cont', 'ribu', 'tors', 'resp', 'ecte']
"Contributors are respected.".match(/\w{4}/gy) // ['Cont', 'ribu', 'tors']
```

可惜却丢失了位置（index）信息。为了弥补这个遗憾，我们就得搬请功能最强大的 API：RegExp 的 exec 和 String 的 `matchAll`。

exec 刚刚已经被用来捕获首个匹配了，不过那是在没有 g 和 y 修饰符的环境下。一旦开启这两个修饰符中的任意一个，才算是释放了 exec 的潜力。

**exec 的使用“秘诀”是遍历**。比如说这样一个例子，前端收到后端发送过来的一大篇 HTML 代码，这里面会有数量不定占位符，比如 `<!-- video: https://someurl -->`，我们需要把这个占位符替换成具有丰富交互的视频播放器，怎么做呢？

我们必然要搜索到所有的占位符位置：

```javascript
const reg = /<!--\s*video:\s*(.+)\s*-->/g;
const html = `
<p>text</p>
<!-- video: http://someurl/1.mp4 -->
<div>sep</div>
<!-- video: http://someurl/2.mp4 -->
<footer></footer>
`;
let result;
while(result = reg.exec(html)) { // 依次调用exec
    // ['\x3C!-- video: http://someurl/1.mp4 -->', 'http://someurl/1.mp4 ', index: 13...
    // ['\x3C!-- video: http://someurl/2.mp4 -->', 'http://someurl/2.mp4 ', index: 65...
    console.log(result);
}
```

看到了吗？我们循环调用 exec，直到返回 null 为止。这里隐含着一个特性：在带有修饰符 g（或 y）的 RegExp 对象上调用 exec，RegExp 对象本身是带有状态的，这个状态就是 `lastIndex`，代表下一次搜索的起始位置，初始为 0。

如果嫌 exec 使用起来麻烦，我们还有 String 的 matchAll 可用：

```javascript
const result = "What is your name?".matchAll(/\w{4}/g)
// [
//   [ 'What', index: 0, input: 'What is your name?', groups: undefined ],
//   [ 'your', index: 8, input: 'What is your name?', groups: undefined ],
//   [ 'name', index: 13, input: 'What is your name?', groups: undefined ]
// ]
Array.from(result);
```

matchAll 的名字已经说明了它的行为特点，同时为了语义上的一致性，它强制要求传入的正则带有 g 修饰符，否则就会抛出异常。

注意，matchAll 返回的是一个迭代器对象而不是简单的数组，你可以用 `for...of` 来依次获取每一个匹配结果，也可以像上面那样用 `Array.from` 来整体转换成数组。

这就是在搜索、匹配子串上最复杂的两种方案了，现在还是更建议使用 exec，虽然麻烦一点，但因为 matchAll 是最近几年才引入的，旧一些的浏览器版本不一定都支持。

以上三种需求的渐进关系，我总结出这样一张图：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8c16314db4cd45e5b34477e5e36e11fb~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

## 字符串替换

严格来说，替换能力是搜索能力的子集，我们学会了搜索，知道每一个子串的位置了，自然就能够实现替换。不过 String 还是提供了两个函数，`replace` 和 `replaceAll`，能简化这一过程。和上面一样，替换也可以分为只替换第一个和替换全部这两种需求。

如果只需要替换第一个匹配的话，那么用 replace 即可，参数可以是字符串，也可以是不带 g 的正则：

```javascript
"ABCB".replace('B', "-") // "A-CB"
"ABCB".replace(/B/, "-") // "A-CB"
```

如果要替换全部匹配的话，可以依旧使用 replace，只不过必须传入带 g 的正则：

```javascript

```
ts

复制代码

`"ABCB".replace(/B/g, "-") // "A-C-"`

在同样的参数下，也可以使用 replaceAll。replaceAll 相比 replace 而言可以实现在传入字符串参数的情况下也能做到全部替换：

ts

复制代码

`"ABCB".replaceAll('B', "-") // "A-C-"`

我总结出下面这张图，可以用作使用哪一个函数的判断之用：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6ebb68e31c1540d8be7e3cfdf69d6b79~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

## 小结

虽然正则表达式本身的语法可以写得很复杂，但其实从编程语言上来说只需要一个 API 就够了。像 JavaScript 这样，我们完全可以用 RegExp 的 exec 函数来实现任意的搜索和替换功能，剩余的其他 API 可以认为是为了使用上的方便，在不同需求下进行的一层包装。

从这一点上看，JavaScript 提供的这么多与正则有关的函数并没有那么杂乱，你可以这样记：

1. **test、search 就是用来判断是否匹配的**；
2. **exec、match、matchAll 就是用来获取匹配结果的**；
3. **replace、replaceAll 就是用来做替换的**。

ES6 新增的三个修饰符也需要关注，在特殊的需求下往往能取得“四两拨千斤”的效果。