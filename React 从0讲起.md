# React 理念
React 官方认为：
>React 是用 JavaScript 构建**快速响应**的大型 Web 应用程序的首选方式。

因此他的设计理念关键是 「**快速响应**」
所以需要思考的就是影响「**快速响应**」的因素

对于浏览器来说，无非两类：
- 复杂计算CPU耗时
- 异步请求等待过程阻塞了IO

### CPU 瓶颈问题

加入我们需要渲染十万个数据如下：
```tsx
function App() {
	const list = new Array(10000).fill(0)
	return (<>
		{
			list.map((_, i) => {
			})
		}
	</>)
}
```