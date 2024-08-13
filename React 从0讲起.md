# React 理念
React 官方认为：
>React 是用 JavaScript 构建**快速响应**的大型 Web 应用程序的首选方式。

因此他的设计理念关键是 「**快速响应**」
所以需要思考的就是影响「**快速响应**」的因素

对于浏览器来说，无非两类：
- 复杂计算CPU耗时
- 异步请求等待过程阻塞了IO
## CPU 瓶颈问题

假如我们需要渲染十万个数据如下：
```tsx
function App() {
	const list = new Array(10000).fill(0)
	return (<ul>
		{
			list.map((_, i) => {
				<li>{i}</li>
			})
		}
	<ul/>)
}
```

我们需要完成以下操作
```text
JS脚本执行 --> 样式布局 --> 样式渲染
```

但是，主流浏览器的刷新率为`60Hz`，即`16.6ms`浏览器就会刷新一次。
而，「渲染线程」和「JS线程」是互斥的。
所以以上三个任务并**不能**同时进行

当JS脚本执行时间过长，就会卡顿，掉帧。

如何解决？
=> **时间切片**
简单实现就是`requestAnimationFrame()`

在浏览器每帧的时间里都留一些空给 `JS` 线程，React 就利用这部分时间更新组件，（源码钟初始预留 `5ms`）
当预留时间不够用时，React 就把线程权交给浏览器渲染，等待下一个空闲时间。

可以使用`ReactDOM.unstable_createRoot`来开启时间切片 - `Concurrent Mode`

这样就把**同步的更新**转为了**可中断的异步更新**

## IO 瓶颈
网络延迟使用 suspence和 useDeferredValue

## React 15架构

- Reconciler - 协调器 - 决定更新什么组件
- Renderer - 渲染器 - 渲染更新后的组件
### Reconciler

触发更新时，Reconciler 做如下工作
- 调用函数组件，或者类组件的`render`方法，把 jsx 转换为 vDOM
- 将 vDOM 和上次更新的 vDOM 对比
- 找出本次更新变化的 vDOM
- 通知 Renderer 渲染更新的 vDOM 

### Renderer

由于 React 是一个支持跨平台的框架，所以在不同的平台有不同的 Renderer。我们最熟悉最常用的就是浏览器环境的 Renderer -> ReactDOM 了

除此之外，还有：

- [ReactNative](https://www.npmjs.com/package/react-native)渲染器，渲染 App 原生组件
- [ReactTest](https://www.npmjs.com/package/react-test-renderer)渲染器，渲染出纯 Js 对象用于测试
- [ReactArt](https://www.npmjs.com/package/react-art)渲染器，渲染到 Canvas, SVG 或 VML (IE8)