## 虚拟DOM优缺点

### 优点

- `virtual DOM`在牺牲部分性能的前提下，增加了可维护性，这也是很多框架的通性。
- 实现了对`DOM`的集中化操作，在数据改变时先对虚拟`DOM`进行修改，再反映到真实的`DOM`，用最小的代价来更新`DOM`，提高效率。
- [ ] 打开了函数式`UI`的大门。（？）
- 可以渲染到`DOM`以外的端(`React Native`, `Vision Pro`)
- [ ] 可以更好的实现`SSR`，同构渲染 （？）
- [ ] 组件的高度抽象

### 缺点

- 首次渲染大量`DOM`时，由于多了一层虚拟`DOM`的计算，会比`innerHTML`插入慢。
- 虚拟`DOM`需要在内存中维护一部分`DOM`的副本，多占用了部分内存。如果虚拟`DOM`大量更改，这是合适的。但是单一的、频繁的更新的话，虚拟`DOM`将会花费更多的时间处理计算的工作。所以如果你有一个`DOM`节点相对较少页面，使用虚拟`DOM`，它实际上有可能会更慢，但对于大多数单页面应用，这应该都会更快。

## React 组件的 state 和 props

`React`的数据是自顶向下单向流动的，即从父组件到子组件中，组件的数据存储在`props`和`state`中。实际上在任何应用中，数据都是必不可少的。我们需要直接的改变页面上一块的区域来使得视图更新(**这里父组件的的更新实际上会带起其子组件的渲染，但是由于子组件可能为`pure`，因此我们可以用`React.memo`包裹避免不必要的更新**)，或者间接地改变其他地方的数据，在`React`中就使用`props`和`state`存储数据。

### 描述

`state`的主要作用是用于组件保存、控制、修改自己的可变状态。`state`在组件内部初始化，可以被组件自身修改，而外部不能访问也不能修改，可以认为`state`是一个局部的、只能被组件自身控制的数据源。（**但是可以将修改父组件`state`的`setXXX`方法传给子组件，子组件调用即可更改父组件的值。不过这也不与上述看法冲突，我们传入的`setXXX`方法本质上也属于父组件，其更改控制是由子组件控制的而已。（类似与所有权为父组件所有，使用权由父组件传给哪个子组件决定）**）

`props`的主要作用是让使用该组件的父组件可以传入参数来配置该组件，他是外部传进来的配置参数。组件内部无法控制也无法修改，除非外部组件主动传入新的`props`，否则组件的`props`永远不变。

## React 闭包陷阱

`React Hooks`是`React 16.8`引入的一个新特性，其出现让`React FC`也能拥有状态和生命周期方法，其优势在于可以让我们在不编写类组件方法的情况下，更细粒度地复用状态逻辑和副作用代码，但是同时也带来了额外的心智负担，闭包陷阱就是其中之一。

### 闭包

从`React`闭包陷阱的名字就可以看出来，我们的问题是由闭包引起的，那么闭包就是我们必须要探讨的问题了。函数和对其词法环境(`lexical environment`)的引用捆绑在一起构成闭包，也就是说，闭包可以让你从内部函数访问外部函数作用域。

在`javascript`，函数在每次创建时生成闭包。在本质上，闭包是将函数内部和函数外部链接起来的桥梁。通常来说，一段程序代码中所用到的名字并不总是有效或者可用的，而限定这个名字的可用性的代码范围就是这个名字的作用域(`scope`)。当一个方法或成员被声明，他就用于当前执行上下文(`context`)环境，在有具体的`context`中，表达式是可见的也能够被引用，如果一个变量或者其他表达式不在当前的作用域，则将无法使用。

作用域可以根据代码层次分层，以便子作用域可以访问父作用域，通常是指沿着链式的作用域链查找，而不能从父作用域引用子作用域中的变量和引用。

为了定义一个闭包。首先需要一个函数来套一个匿名函数。闭包是需要使用局部变量的，定义使用全局变量就失去了使用闭包的意义，是最外层定义的函数可实现局部作用域从而定义局部变量，函数外部无法直接访问到内部定义的变量。

从下边这个例子中我们可以看到定义在函数内部的`name`变量并没有被销毁，我们仍然可以在外部使用函数访问这个局部变量。

使用闭包，可以把局部变量驻留在内存中，从而避免使用全局变量，因为全局变量污染会导致应用程序不可预测性，每个模块都可调用必将带来灾难。

```javascript
const Student = () => {
	const name = "Ming"
	const sayMyName = function () {
		console.log(name)
	}
	console.log(sayMyName)
	return sayMyName
}
const stu = Student()
stu() // "Ming"
```

实际开发中使用闭包的场景非常多，例如我们常常使用的回调函数。

#### 回调函数
回调函数就是一个典型的闭包，回调函数可以访问父级函数作用域中的变量，而不需要将变量作为参数传递到回调函数中，这样就可以减少参数的传递，提高代码的可读性。

```javascript
const cb = () => {
	const local = 1
	return () => {
	console.log(local)
	}
 }

setTimeout(cb(), 1000) // 1
```

#### 接口频率

`Node`在调用其他第三方服务接口的时候常常会被限制频率，比如对于该接口`1s`最多请求`3`次，此时我们通常有两种解决方案:
- 在请求时就限制发起请求的频率，直接在发起时候就控制好，被限制的请求需要排队
- 不限制发起请求的频率，而是采用一种基于重试的机制，当请求的结果是被限制的时候，我们就延迟一段时间再发起请求。可以用指数退避算法等方式来控制重试时间，实际上以太网在拥堵的时候就采用了这种方法，每次发生碰撞后，设备会根据指数退避算法来计算等待时间，等待时间会逐渐增加，从而降低了设备再次发生碰撞的概率。

这里我们需要关注第二种方案中如何进行重试，我们在发起请求的时候通常会携带比较多的信息，比如`url`，`token`，`body`等数据进行查询，如果我们需要进行重试，那么肯定需要找一个地方把这些数据储存下来以备下次发起请求。

那么，在何处储存这些变量呢？当然我们可以在`global/window`中构造一个全局对象来存储，但是之前也提到过了全局变量污染会导致程序的不可预测性，所以我们这里更希望使用闭包来存储。

```javascript
const requestFactory = (url, token) {
	return () => {{url, token}} // * 假设这个函数会发起请求并返回结果
}

const req1=  requestFactory("url1", "token1")
console.log(req1()) // * 发起请求，`{url: "url1", token: "token1"}`
console.log(req1()) // * 重试请求，`{url: "url1", token: "token1"}`

const req2 = requestFactory("url2", "token2")
console.log(req2()) // * 发起请求，`{url: "url2", token: "token2"}`
console.log(req2()) // * 重试请求，`{url: "url2", token: "token2"}`

```

`js`是静态作用域，但是`this`对象是一个例外。`this`的指向问题就类似于动态作用域，其不关心函数和作用域是如何生命以及在何处声明的，只关心是从何处调用的，`this`的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定`this`到底指向谁，当然实际上`this`的最终指向是那个调用的对象。

`this`的设计主要是为了能够在函数体内部获取当前的运行环境`content`,因为在`js`的内存设计中`Function`是独立的一个堆地址空间，不和`Object`直接先管，所以才需要绑定一个运行环境


### 闭包陷阱

组件多次执行的实例：
```tsx
import React, { useState } from "react";

const collect: (() => number)[] = [];

export const MultiCount: React.FC = () => {
  const [count, setCount] = useState(0);

  const click = () => {
    setCount(count + 1);
  };

  collect.push(() => count);

  const logCollect = () => {
    collect.forEach((fn) => console.log(fn()));
  };

  return (
    <div>
      <div>{count}</div>
      <button onClick={click}>count++</button>
      <button onClick={logCollect}>log {">>"} collect</button>
    </div>
  );
};
```
按三次`count++`按钮，点击`log >> collect`时，打印`0 1 2 3`

这是因为闭包`+`函数多次执行造成的问题，因为`Hooks`实际上无非就是函数，`React`通过内置的 `use`为函数赋予了特殊的意义，使其能够访问`Fiber`从而做到**数据与节点相互绑定**，那么既然 上一个函数，并且在`setState`的时候还会重新执行，那么在重新执行的时候，点击按钮之前的`add`函数地址与点击按钮之后的`add`函数地址是不同的，因为这个函数实际上是被重新定义了一遍。

#### 陷阱开始

闭包陷阱常由==依赖更新不及时==导致，例如`useEffect`,`useCallback`的依赖定义的不合适，导致函数内部保持了对上海一次组件刷新时定义的作用域，从而导致了问题。

```tsx
import { useEffect, useRef, useState } from "react";

export const BindEventCount: React.FC = () => {
  const ref1 = useRef<HTMLButtonElement>(null);
  const [count, setCount] = useState(0);

  const add = () => {
    setCount(count + 1);
  };

  useEffect(() => {
    const el = ref1.current;
    const handler = () => console.log(count);
    el?.addEventListener("click", handler);
    return () => {
      el?.removeEventListener("click", handler);
    };
  }, []);

  return (
    <div>
      {count}
      <div>
        <button onClick={add}>count++</button>
        <button ref={ref1}>log count 1</button>
      </div>
    </div>
  );
};
```

当我们多次点击`count++`按钮之后，再去点击`log count 1`按钮，发现控制台输出的内容还是`0`，这就是因为我们的`useEffect`保持了旧的函数作用域，而那个函数作用的`count`为`0`，那么打印的值当然就是`0`，同样的`useCallback`也会出现类似的问题，解决这个问题的一个简单的办法就是在依赖数组中加入`count`变量，当`count`发生变化的时候，就会重新执行`useEffect`，从而更新函数作用域。那么问题来了，这样就能解决所有问题吗，显然是不能的，副作用依赖可能会造成非常长的函数依赖，可能会导致整个项目变得越来越难以维护。

`useRef`是解决闭包问题的万金油，其能存储一个不变的引用值。设想一下我们只是因为读取了旧的作用域中的内容而导致了问题，如果我们能够得到一个对象使得其无论更新了几次作用域，我们都能够保持对同一个对象的引用，那么更新之后直接取得这个值不就可以解决这个问题了嘛。在`React`中我们就可以借助`useRef`来做到这点，通过保持对象的引用来解决上述的问题。

```tsx
type noop = (this: any, ...args: any[]) => any;

type PickFunction<T extends noop> = (
  this: ThisParameterType<T>,
  ...args: Parameters<T>
) => ReturnType<T>;

function useMemoizedFn<T extends noop>(fn: T) {
  const fnRef = useRef<T>(fn);

  // why not write `fnRef.current = fn`?
  // https://github.com/alibaba/hooks/issues/728
  fnRef.current = useMemo(() => fn, [fn]);

  const memoizedFn = useRef<PickFunction<T>>();
  if (!memoizedFn.current) {
    memoizedFn.current = function (this, ...args) {
      return fnRef.current.apply(this, args);
    };
  }

  return memoizedFn.current as T;
}
```

## 受控组件与非受控组件

在表单处理中，我们绕不开*受控组件*与*非受控组件*。
那，什么是受控与非受控呢？

表单无非时有两种情况，`readonly`和`writeable`。

也就是当组件自身管理自己的`value`,无法从外界对他造成影响时，这就是受控的。能被用户输入修改自身`value`的，就是非受控的。


### 非受控

我们利用`input`来展示一下非受控模式
```tsx
import { ChangeEvent } from 'react'

const App = () => {
	function onChange(event: ChangeEvent<HTMLInputElement>) {
		console.log(event.target.value)
	}
	return <input defaultValue={'apries'} onChange={onChange}></input>
}

export default App
```

此时我们可以通过输入事件来改变`input.value`。

### 受控组件
```tsx
import { ChangeEvent, useState } from 'react'

const App = () => {
	const [value, setValue] = useState('apries')
	function onChange(event: ChangeEvent<HTMLInputElement>) {
		console.log(event.target.value)
		setValue(event.target.value)
	}
	console.log('render ....')
	return <input value={value} onChange={onChange}></input>
}

export default App
```

这里我们可以看到，`input.value`是由自身状态决定的。
但是每一次的输入都会引起整个组件渲染。

## 高阶组件(HOC)

