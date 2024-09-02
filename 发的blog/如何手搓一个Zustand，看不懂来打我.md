---
title: 如何手搓一个Zustand，看不懂来打我
id: 257e221c-abbc-4dea-b6a1-dce2d0634c62
date: 2024-03-10 17:25:25
auther: 406
cover: /upload/bear.jpg
excerpt: Zustand采用发布订阅者模式，实现了createStore方法 简易实现 下面实现一个简单的mini_createStore方法，基于发布订阅 const mini_createStore = () => {    let state;    const listeners = new Se
permalink: /archives/cae67610-48c5-4b2b-a62d-f3e5279f6ba2
categories:
 - qian-duan
 - yuan-ma-si-kao
tags: 
 - javascript
 - zustand
---


Zustand采用发布订阅者模式，实现了`createStore`方法

## 简易实现

下面实现一个简单的`mini_createStore`方法，基于发布订阅

```javascript
const mini_createStore = () => {
	let state;
	const listeners = new Set()
	
	const getState = () => state
	
	const setState = (newState) => {
		state = newState 
		// * 通知订阅者
		listeners.forEach(listener=>linstener())
	}

	// * 创建订阅方法
	const subcribe = (listener) => {
		// * 添加到订阅者列表中
		listeners.add(listener)
		
		// * 返回取消订阅的方法
	    return (listener) => listeners.delete(listener)
	}

	return {
		getState,
		setState,
		subcribe
	}
}
```

## 问题思考

这样简单的代码存在以下问题：

### 初始化问题

我们使用：

```javascript
const store = mini_createStore()
```

后，再使用`store.getState()`查看`store`中`state`的值

```javascript
console.log(store.getState()) // undefined
```

会发现他是`undefined`, but why?

因为我们在`mini_createStore`中这样使用创建了`state`

```javascript
let state
```

但是并没有初始化他，因此我们需要一个初始化方法。

此时我们让`mini_createStroe`接受一个方法，用于初始化`state`，如下：

```javascript
const mini_createStore = (crateInitialState) => {
  let state
  const listeners = new Set()
 

  const getState = () => state
  
  const setState = (newState) => {
    .....  
  }
  
  const subcribe = (listener) => {
    .............
  }
  // * 初始化state
  state = crateInitialState()  
  
  return .....
}
```

现在我们已经可以正确的得到初始值啦：

```javascript
const store = mini_createStore(()=>( {count: 0} ))
console.log(store.getState()) // * { count: 0 }
```

### `newState`方法问题
由于我们这样给`state`赋值，

```javascript
state = newState 
```

有时我们期待使用方法来使用`prevState`改变`state`，如下:

```javascript
store.setState((state) => ({ count: state.count + 1 }))
```

但是当我们打印`store.getState()`得到：

```javascript
console.log(store.getState()) // * [Function (anonymous)]
```

因此我们需要特判一下传入的`newState`类型，如下：

```javascript
state = typeof newState === "Function" ? newState(state) : newState
```

现在我们已经可以通过方法来改变`state`啦

```javascript
const store = mini_createStore(() => ({ count: 0 }))
store.setState((state) => ({ count: state.count + 1 }))
console.log(store.getState()) // * { count: 1}
```
