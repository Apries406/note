### 谈谈你对闭包的了解？闭包的使用场景

#### 什么是闭包
一个函数和对其周围状态（lexical environment，词法环境）的引用捆绑在一起（或者说函数 被引用包围），这样的组合就是闭包（closure）

也就是说，闭包让你可以在一个内层函数中访问到其外层函数的作用域

在 `JavaScript`中，每当创建一个函数，闭包就会在函数创建的同时被创建出来，作为函数内部与外部连接起来的一座桥梁

下面给出一个简单的例子
```javascript
function init() {
    var name = "Mozilla"; // name 是一个被 init 创建的局部变量
    function displayName() { // displayName() 是内部函数，一个闭包
        alert(name); // 使用了父函数中声明的变量
    }
    displayName();
}
init();

```

`displayName()` 没有自己的局部变量。然而，由于闭包的特性，它可以访问到外部函数的变量

#### 使用场景

任何闭包的使用场景都离不开这两点：
- 创建私有变量
- 延长变量的生命周期
>一般函数的词法环境在函数返回后就被销毁，但是闭包会保存创建时所在词法环境的引用，即使创建时所在执行上下文被销毁，但创建时所在词法环境仍然存在，以达到延长变量生命周期的目的

下面举个例子：

在页面上添加一些可以调整字号的按钮:
```javascript
function makeSizer(size) {
  return function() {
    document.body.style.fontSize = size + 'px';
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);

document.getElementById('size-12').onclick = size12;
document.getElementById('size-14').onclick = size14;
document.getElementById('size-16').onclick = size16
```

##### 柯里化函数

科里化的目的在于避免频繁调用具有相同参数函数的同时，又能够轻松的重用
```javascript
// 假设我们有一个求长方形面积的函数
function getArea(width, height) {
    return width * height
}
// 如果我们碰到的长方形的宽老是10
const area1 = getArea(10, 20)
const area2 = getArea(10, 30)
const area3 = getArea(10, 40)

// 我们可以使用闭包柯里化这个计算面积的函数
function getArea(width) {
    return height => {
        return width * height
    }
}

const getTenWidthArea = getArea(10)
// 之后碰到宽度为10的长方形就可以这样计算面积
const area1 = getTenWidthArea(20)

// 而且如果遇到宽度偶尔变化也可以轻松复用
const getTwentyWidthArea = getArea(20)
```

##### 使用闭包模拟私有方法

在`javascript`中，没有支持声明私有变量，但我们可以使用闭包来模拟私有方法
```javascript
var Counter = (function() {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  }
})();

var Counter1 = makeCounter();
var Counter2 = makeCounter();
console.log(Counter1.value()); /* logs 0 */
Counter1.increment();
Counter1.increment();
console.log(Counter1.value()); /* logs 2 */
Counter1.decrement();
console.log(Counter1.value()); /* logs 1 */
console.log(Counter2.value()); /* logs 0 */
```
上述通过使用闭包来定义公共函数，并令其可以访问私有函数和变量，这种方式也叫模块方式

两个计数器 `Counter1` 和 `Counter2` 是维护它们各自的独立性的，每次调用其中一个计数器时，通过改变这个变量的值，会改变这个闭包的词法环境，不会影响另一个闭包中的变量

##### 其他

例如计数器、延迟调用、回调等闭包的应用，其核心思想还是创建私有变量和延长变量的生命周期
#### 注意事项
如果不是某些特定任务需要使用闭包，在其它函数中创建函数是不明智的，因为闭包在处理速度和内存消耗方面对脚本性能具有负面影响

例如，在创建新的对象或者类时，方法通常应该关联于对象的原型，而不是定义到对象的构造器中。

原因在于每个对象的创建，方法都会被重新赋值
```javascript
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
  this.getName = function() {
    return this.name;
  };

  this.getMessage = function() {
    return this.message;
  };
}
```
上面的代码中，我们并没有利用到闭包的好处，因此可以避免使用闭包。修改成如下：
```javascript
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
}
MyObject.prototype.getName = function() {
  return this.name;
};
MyObject.prototype.getMessage = function() {
  return this.message;
};
```

## parseInt/Math.round/ceil/floor的区别

### parseInt()

该方法取整是吧小数点后面的小数去掉，只保留整数部分。
- 正数：类似`Math.floor()`
- 负数：类似`Math.ceil()`

### Math.ceil()

**向上取整**，即小数部分直接舍去，并向正数部分进`1`

### Math.floor()

**向下取整**，小数部分直接舍去。

### Math.round()

**四舍五入**


## Promise

在`promiseA+`中，把每一个异步任务都看做一个`promise`对象，每一个任务对象都有两个阶段（`unsettled`，`settled`）,三个状态（`pending`，`fulfilled`，`rejected`）

两个阶段：
- unsettled：事情未解决阶段，事情还没有结果。
- settled：事情已解决阶段，无论是成功，还是失败，已经有结果了。
- 
三个状态：
- pending：挂起，处于未解决阶段。事情还没有结果输出，promise对象为挂起状态。
- fulfilled：已处理，处于已解决阶段。已经有了成功的结果，可以按照正常逻辑进行下去，并可以传递一个成功信息，后续可以用.then的onFulfiled处理。
- rejected：已拒绝，处于已解决阶段，已经有了失败的结果，无法按照正常逻辑进行下去，可以传递一个错误信息，后续可以用.then的onRejected处理，或使用catch处理。

两个阶段是由unsettle -> settled，**不可逆**。

三个状态是由pending -> fulfilled/rejected，**不可逆**，且fulfilled和rejected之间的状态不可以互相切换。

也就是说，promise对象一旦有了结果，结果**不可改变**，**不可消失**。

![[Pasted image 20240312223318.png]]

### 基本使用

```javascript
const p1 = new Promise((resolve, reject) => {
  //此部分为同步部分
  //此部分决定Promise的状态，未决定之前为Padding不能进行后续对该Promise的处理
  //resolve和reject只能接受和传递一个参数
  //resolve将Promise对象推入已处理状态
  //reject将Promise对象推入已拒绝状态
  resolve("请求成功");
  //状态一旦改变就不可以更改
  //reject('请求失败')
});
​
//处理Promise的结果，为异步部分。
//若对应的promise为pending，则会加入作业队列，等待Promise结果。
//若Promise结果为resolved或rejected，加入微任务队列等待执行JS引擎轮询到立马执行。
p1.then(
  (value) => {
    //此部分为onFulfilled，也就是thenabled处理，Promise状态为resolved接收参数并执行此部分代码。
    console.log(`Promise已处理，接收到传值：${value}`);
  },
  (err) => {
    //此部分为onRejected，也就是catchabled处理，Promise状态为reject接收参数并执行此部分代码。
    //Promise对象的then方法调用中也可以不写此部分，不进行错误处理，在Promise对象的catch方法中进行错误处理。
    console.log(`Promise已拒绝,接收到原因：${err}`);
  }
);
//Promise并没有消除回调，只是让回调变得可控了。
```

## 作用域

 作用域`scope`规定了变量能被访问的范围，在这个范围之外的变量不能被外部访问。

作用域有：
- **局部作用域**
- **全局作用域**
### 局部作用域

局部作用域分为**函数作用域**和**块级作用域**

#### 函数作用域

在函数内部声明的变量，只能在函数内部被访问， 外部无法直接访问
- **函数的参数**也是函数内部的局部变量
- 函数执行完毕之后，函数内部的变量实际被清空了

