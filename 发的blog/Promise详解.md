---
title: Promise详解
id: e5c2cc37-f796-4685-9b57-17e1041d826d
date: 2024-05-06 22:28:44
auther: 406
cover: 
excerpt: Promise详解 为什么会出现Promise 回调地狱 在未出现 Promise前，我们处理异步的方式通常是传递一个回调函数(callback)，这个函数将在异步任务执行完成之后被调用。 如下，我们尝试请求四个不同地址的接口，并拼接： fetch('url1', (res1) => {	fetc
permalink: /archives/promisexiang-jie
categories:
 - qian-duan
tags: 
 - javascript
---

# Promise详解

# 为什么会出现Promise

**回调地狱**
在未出现 `Promise`前，我们处理异步的方式通常是传递一个回调函数(`callback`)，这个函数将在异步任务执行完成之后被调用。
如下，我们尝试请求四个不同地址的接口，并拼接：

```javascript
fetch('url1', (res1) => {
	fetch('url2', (res2) => {
		fetch('url3', (res3) => {
			fetch('url4', (res4) => {
				const ans = res1 + res2 + res3 + res4
				console.log(ans)
			})
		})
	})
})
```

这样层层嵌套的回调函数，如果再加上其他情况的分支条件，嵌套的代码可读性将会急剧下降，导致回调地狱问题。

# 什么是Promise

> Promise 是一个对象，它代表了一个异步操作的最终完成和失败
>
> 它能够把一步操作最终的成功返回值或者失败原因个和相应的处理程序关联起来,使得一步方法可以像同步方法那样有返回值,异步方法不会立即返回最终的值,而是会返回一个 `Promise`对象,以便在未来某刻(结果返回时)把值交给使用者

一个 `Promise` 必然处于以下几种状态之一：

- _待定（pending）_：初始状态，既没有被兑现，也没有被拒绝。
- _已兑现（fulfilled）_：意味着操作成功完成。
- _已拒绝（rejected）_：意味着操作失败。

🌰:

```javascript
let p1 = new Promise((resolve, reject) => {
	console.log('Promise 1 is running')
})

console.log(p1)
/*
output:
Promise 1 is running
Promise { 
	[[prototype]]: Promise  
	[[PromiseState]]: 'pending'
	[[PromiseResult]]: undefined
}
*/
```

此时可以看到回调函数中代码被立即执行,同时 `p1`处于初始状态 `pending`

现在我们在回调函数中调用 `resolve`回调

```javascript
let p1 = new Promise((resolve, reject) => {
	console.log('Promise 1 is running')
	resolve()
})

console.log(p1)
/*
output:
Promise 1 is running
Promise { 
	[[prototype]]: Promise  
	[[PromiseState]]: 'fulfilled'
	[[PromiseResult]]: undefined
}
*/
```

可以看到 `p1`的状态由 `pending`变为 `fulfilled`

同理我们调用 `reject`回调

```javascript
let p1 = new Promise((resolve, reject) => {
	console.log('Promise 1 is running')
	reject()
})

console.log(p1)
/*
output:
Promise 1 is running
Promise { 
	[[prototype]]: Promise  
	[[PromiseState]]: 'rejected'
	[[PromiseResult]]: undefined
}
*/
```

顺利地将 `p1`的状态改为了 `rejected`

#### 一同调用呢?

```javascript
let p1 = new Promise((resolve, reject) => {
	console.log('Promise 1 is running')
	reject()
	resolve()
})

console.log(p1) // output: rejected
```

```javascript
let p1 = new Promise((resolve, reject) => {
	console.log('Promise 1 is running')
	reject()
	resolve()
})

console.log(p1) // output: fulfilled
```

由此可见

> 如果一个 Promise 已经被兑现或拒绝，即不再处于待定状态，那么则称之为已敲定（settled）。

在Promise已经敲定之后,进一步*解决*或*拒绝*它都没有影响
![Pasted image 20240506185447.png](http://apries.xyz/upload/Pasted%20image%2020240506185447.png)

# resolve/reject之后发生了什么

- 在调用 `resolve`回调之后会执行 `then`方法传入的回调
- 在调用 `reject`回调之后会执行 `catch`方法传入的回调

```javascript
let p1 = new Promise((resolve, reject) => {
	resolve()
})

p1.then(() => {
	console.log('p1 fulfilled')
}).catch(() => {
	console.log('p1 rejected')
})

let p2 = new Promise((resolve, reject) => {
	reject()
})

p2.then(() => {
	console.log('p1 fulfilled')
}).catch(() => {
	console.log('p2 rejected')
})

/*
output:
p1 fulfilled
p2 rejected
*/
```

## resolve回调参数处理

### *值*或*普通对象*

如果 `resolve`传入的是一个普通的值或者对象，那么会作为 `then`回调的参数

```javascript
let p = new Promise((resolve, reject) => {
	resolve('这是「值」情况')
})
p.then((res) => {
	console.log(res) /// output: 这是「值」情况
})

let p1 = new Promise((resolve, reject) => {
	resolve({ msg: '这是「普通对象」情况' })
})
p1.then((res) => {
	console.log(res) // output: { msg: '这是「普通对象」情况' }
})
```

### *新Promise对象*

这个*新Promise对象*会决定原Promise的状态，并且新Promise对象的 `resolve`或 `reject`的参数也会传给原来的 `Promise`
如下：

```javascript
let p1 = new Promise((resolve, reject) => {
	resolve(
		new Promise((resolve, reject) => {
			resolve({
				msg: '这是「新Promise对象」情况',
				status: 'fulfilled',
			})
		})
	)
})

p1.then((res) => {
	console.log(res) // output: { "msg": "这是「新Promise对象」情况","status": "fulfilled" }
})

let p1 = new Promise((resolve, reject) => {
	resolve(
		new Promise((resolve, reject) => {
			reject({
				msg: '这是「新Promise对象」情况',
				status: 'rejected',
			})
		})
	)
})

p1.catch((res) => {
	console.log(res) //output: { "msg": "这是「新Promise对象」情况","status": "rejected" }
})
```

### 带有 `then`方法的对象(thenable对象)

如果给 `resolve`对象传入一个带有 `then`方法对象，那么会执行这个 `then`方法，并且根据 `then`方法的结果来决定Promise的状态

```javascript
const obj = {
	name: '张三',
	then: function (resolve, _reject) {
		resolve({
			msg: '这是「带then方法的对象」情况',
			status: 'fulfilled',
		})
	},
}
let p1 = new Promise((resolve, reject) => {
	resolve(obj)
})

p1.then((res) => {
	console.log(res) // output: { "msg": "这是「带then方法的对象」情况", "status": "fulfilled" }
}).catch((res) => {
	console.log(res)
})
```

## reject回调参数处理

### 值或普通对象

其他与 `resolve`一样

### 新Promise对象

无论新Promise对象的状态如何,都执行外界Promise的 `catch`回调,返回的是内层Promise.从形式上来看也与上述(*值与普通对象*)的情况相似

# then/catch的调度

在前面的🌰中，我们的 `then`方法和 `catch`方法都仅仅调度了一次，现在我们来尝试多次调用的情况。

```javascript
const p1 = new Promise((resolve, reject) => {
	resolve('这是「多次回调then」的情况')
})
p1.then((res) => {
	console.log('res1:' + res)
})
p1.then((res) => {
	console.log('res2:' + res)
})
p1.then((res) => {
	console.log('res3:' + res)
})

/*
output:
res1:这是「多次回调then」的情况
res2:这是「多次回调then」的情况
res3:这是「多次回调then」的情况
*/
```

这说明 `then`是可以被多次调用的，每次调用我们都可以传入对应的 `fulfilled`回调，当Promise的状态变为 `fulfilled`的时候，这些回调函数都会被执行。
`catch`也能被多次回调，与 `then`相同。

## then

`then()`是有返回值的，它的返回值是一个*Promise*，所以我们可以对其进行链式调用

```javascript
const p1 = new Promise((resolve, reject) => {
	resolve('这是「链式调用then」的情况')
})
p1.then((res) => {
	console.log('res1:' + res)
	return '这是「链式调用then」的情况的res1返回值'
}).then((res) => {
	console.log('res2:' + res)
	return '这是「链式调用then」的情况的res2返回值'
}).then((res) => {
	console.log('res3:' + res)
})
/*
output:
res1:这是「链式调用then」的情况
res2:这是「链式调用then」的情况的res1返回值
res3:这是「链式调用then」的情况的res2返回值
*/
```

既然能链式调用 `then`说明每一个返回的Promise状态已经被敲定了。
那，这个状态是何时被敲定的呢？
事实上，这个新的Promise是等到 `then`方法传入的回调函数返回值时，才会被敲定。

当 `then`方法中的回调函数在执行时，返回的Promise处于 `pending`状态，当返回一个结果时，会处于 `fulfilled`状态，并将返回的结果作为 `resolve`的参数，因此链式调用的 `then`方法里的回调函数的参数其实是上一个 `then`的返回值。

### 链式then的返回值

#### *值*和*普通对象*

```javascript
const p1 = new Promise((resolve, reject) => {
	resolve('这是「链式调用then」的情况')
})
p1.then((res) => {
	return '这是「链式调用then--值」的情况'
})
	.then((res) => {
		console.log('res2:' + res)
		return { msg: '这是「链式调用then--普通」的情况' }
	})
	.then((res) => {
		console.log(res)
	})
/*
output:
res2:这是「链式调用then--值」的情况
{ "msg": "这是「链式调用then--普通」的情况" }
*/
```

#### *新Promise对象*

```javascript
const p1 = new Promise((resolve, reject) => {
	resolve(
		new Promise((resolve, reject) => {
			resolve('这是「Promise嵌套」的情况')
		})
	)
})
p1.then((res) => {
	console.log(res)
	return '这是「链式调用then--新Promise对象」的情况1'
}).then((res) => {
	console.log(res)
})
/*
output:
这是「Promise嵌套」的情况
这是「链式调用then--新Promise对象」的情况1
*/
```

与最开始的论述是一致的。

### 带有 `then`方法的对象(thenable对象)

与最开始的论述一致

## catch

`catch()`也会返回一个Promise，因此也是支持链式调用的，而且catch后面可以调用 `then`或者 `catch`方法。

```javascript
const p1 = new Promise((resolve, reject) => {
	reject('失败的回调')
})
p1.catch((res) => {
	console.log('catch里面的回调函数')
	return '这次的return传递给了then'
})
	.then((res1) => {
		console.log('then: ' + res1)
	})
	.catch((res2) => {
		console.log('catch: ' + res2)
	})
/*
output:
catch里面的回调函数
then: 这次的return传递给了then
*/
```

那么疑问来了，由于新Promise的敲定方式，我们似乎只能得到 `fulfilled`状态？
如何让新Promise敲定为 `rejected`状态呢？
我们可以*抛出异常*

```javascript
const p1 = new Promise((resolve, reject) => {
	reject('失败的回调')
})
p1.catch((res) => {
	console.log('catch里面的回调函数')
	throw new Error('这次传给了catch')
})
	.then((res1) => {
		console.log('then: ' + res1)
	})
	.catch((res2) => {
		console.log(res2)
	})
/*
output:
catch里面的回调函数
Error: 这次传给了catch
*/
```

# finally

`finally`是$ES9$中新增的一个特性，无论Promise的状态敲定为 `fulfilled`还是 `rejected`，都会执行 `finally`中的回调。其不接收任何参数

- 当Promise敲定为 `fulfilled`时

```javascript
let p1 = new Promise((resolve, reject) => {
	resolve('成功')
})
p1.then((res) => {
	console.log('then: ' + res)
})
	.catch((err) => {
		console.log('catch: ' + err)
	})
	.finally(() => {
		console.log('finally')
	})
/*
output:
then: 成功
finally
*/
```

- 当Promise敲定为 `rejected`时

```javascript
let p1 = new Promise((resolve, reject) => {
	reject('失败')
})
p1.then((res) => {
	console.log('then: ' + res)
})
	.catch((err) => {
		console.log('catch: ' + err)
	})
	.finally(() => {
		console.log('finally')
	})
/*
output:
catch: 失败
finally
*/
```

# 静态方法

### Promise.all

`Promise.all`的作用是将多个Promise包裹(接收的是一个可迭代对象)在一起返回*一个新的Promise*，并且这个新的Promise的状态是由包裹的Promise状态共同敲定的:

- 当接收的所有Promise都被敲定为 `fulfilled`时，返回的Promise也将被敲定为 `fulfilled`(即使传入的是一个空的可迭代对象)，并返回一个包含所有兑现值的数组。

```javascript
let p1 = new Promise((resolve, reject) => {
	resolve('Promise p1')
})
let p2 = p1.then((res) => {
	return res + 'p2'
})
let p3 = p2.then((res) => {
	return res + 'p3'
})
Promise.all([p1, p2, p3]).then((res) => {
	console.log(res) 
/*
output:
["Promise", "Promise p1p2", "Promise p1p2p3"]
*/
```

- 如果接收的Promise有任何一个或多个被敲定为 `rejected`，那么返回的Promise将会被敲定为 `rejected`，并带有第一个被拒绝的Promise的原因

```javascript
let p1 = new Promise((resolve, reject) => {
	resolve('Promise')
})
let p2 = new Promise((resolve, reject) => {
	reject('p2 被敲定为 rejected')
})
Promise.all([p1, p2])
	.then((res) => {
		console.log('res: ' + res)
	})
	.catch((err) => {
		console.log('err: ' + err) 
	})
/*
output:
err:reject状态的Promise
*/
```

## Promise.resolve

`Promise.resolve(res)`方法将给定的值转换为一个Promise

- 该值本身就是一个Promise，则该Promise会被返回。(与上述论述相同)
- 该值是一个thenable对象，Promise.resolve()将调用其 `then`方法及其两个回调函数。(与上述论述相同)
- 该值是一个一般值或普通对象，则转换为Promise。

```javascript
Promise.resolve('fulfilled').then((res) => console.log(res))
// 等价于 
new Promise((resolve, reject)=> resolve('fulfilled'))...
```

## Promise.reject

与 `Promise.resolve(res)`类似，只是将Promise对象敲定为了 `rejected`

```javascript
Promise.reject('rejected').catch((err) => console.log(err))
// 等价于 
new Promise((resolve, reject)=> reject('rejected'))...
```

## Promise.race

`Promise.race()`接收一个promise可迭代对象(数组等)作为输入，并返回一个Promise。这个返回的Promise会随着第一个Promise的敲定而敲定。

```javascript
const p1 = new Promise((resolve, reject) => {
	reject('Promise p1')
})
const p2 = new Promise((resolve, reject) => {
	resolve('Promise p2')
})

Promise.race([p1, p2])
	.then((res) => {
		console.log(res)
	})
	.catch((err) => {
		console.log(err)
	})
/*
output:
Promise p1
*/
```

此时p1先被敲定，于是整个Promise就随着p1的敲定而敲定。

## Promise.any

`Promise.any()`方法是$ES12$中新增的方法，和 `Promise.race()`方法是类似的。不同的是，当输入的任何一个Promise被敲定为 `fulfilled`时，返回的Promise才会被敲定，并返回第一个被兑现的值。当输入的所有的Promise都被拒绝(包括传递了空的可迭代对象)时，他会以一个包含拒绝原因数组的 `AggregateError`拒绝。

```javascript
let p1 = new Promise((resolve, reject) => {
	reject('Promise1')
})
let p2 = new Promise((resolve, reject) => {
	reject('Promise2')
})
Promise.any([p1, p2])
	.then((res) => {
		console.log('res:' + res)
	})
	.catch((err) => {
		console.log(err)
	})
/*
out:
AggregateError: All promises were rejected
*/
```

## Promise.allSettled

`Promise.allSettled()`与 `Promise.all()`类似。但是在 `all()`中，如果有其中一个Promise被敲定为 `rejected`状态，新Promise就会立即敲定。那么对于后续才敲定为 `fulfilled`的，以及仍然处于 `pending`状态的Promise,我们是获取不到的对应结果。

`Promise.allSettled()`会在所有的Promise都被敲定之后，再得到最终的结果，返回的Promise将被敲定为 `fulfilled`状态，并带有描述每个Promise结果的对象数组。

```javascript
let p1 = new Promise((resolve, reject) => {
	reject('Promise1')
})
let p2 = new Promise((resolve, reject) => {
	reject('Promise2')
})
let p3 = new Promise((resolve, reject) => {
	resolve('Promise3')
})
Promise.allSettled([p1, p2, p3])
	.then((res) => {
		console.log(res)
	})
	.catch((err) => {
		console.log(err)
	})
/*
output:
[
	{
		"status": "rejected",
		"reason": "Promise1"
	},
	{
		"status": "rejected",
		"reason": "Promise2"
	},
	{
		"status": "fulfilled",
		"value": "Promise3"
	}
]
*/
```
