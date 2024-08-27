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
简单实现就是`requestIdleCallback()`

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

当有更新发生时，Reconciler 通知 Renderer ，将变化后的需要更新的组件渲染到宿主环境。

### 缺点

在 `Reconciler` 时，`mount`组件调用`mountComponent`，`update`组件调用`updateComponent`组件。两个方法都是递归更新子组件的。

由于是递归更新的，所以更新一旦开始，就不能停止，停止了就会导致深层级的组件没有更新。

因此我们提出来使用**可中断的异步更新**来**取代**这样的**同步更新**

## 新架构！！！React 16

新的架构新增了调度器 Scheduler

- Scheduler - 调度器 - 调度任务优先级，高优先级任务先进入 Reconciler
- Reconciler - 协调器 - 决定更新什么组件
- Renderer - 渲染器 - 渲染更新后的组件

### Scheduler

我们需要在浏览器有剩余时间时通知我们，来作为任务中断的信号。

其实在部分浏览器中已经有了这样的一个Web API，即`requestCallback`。
其弊端：
- 浏览器兼容性
- 触发频率不稳定，受多种因素影响，比如：切换浏览器Tab。

因此，React 实现了功能更强大的`requestIdleCallback`这一 Ployfill, 这就是 **Scheduler**。除了在空闲时间时触发回调的功能外， Scheduler还提供了多种调度优先级任务设置。

>**ployfill 的解释**
>A shim that mimics a future API providing fallback functionality to older browsers.  
 模拟未来api的垫片,为旧版本的浏览器提供了应变计划.


### Reconciler

我们知道，在 React15 中，Reconciler 是递归处理 vDom 的。
而 React16 之后，我们需要一个可中断的异步更新， 源码：[ReactFiberWorkLoop.js](https://github.com/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/packages/react-reconciler/src/ReactFiberWorkLoop.new.js#L1673)

我们可以看见，更新操作从**递归处理**变成了**可中断的循环**。每次循环都会调用`shouldYield`判断

```javascript
function workLoopConcurrent() {
// Perform work until Scheduler asks us to yield
// * 一直工作直到调度器要求我们让出资源
  while (workInProgress !== null && !shouldYield()) {
    performUnitOfWork(workInProgress);
  }
}
```

但是这里出现了一个新问题，就是在中断更新时，我们会遇到**DOM渲染不完全**的问题。
React 16是怎么处理的呢？

在React 16 中，**`Reconciler`与`Renderer`不再是交替工作。当`Scheduler`将任务交给`Reconciler`后，`Reconciler`会为变化的虚拟 DOM 打上*增/删/改*的标记**
```javascript
export type SideEffectTag = number;

// 请不要更改这两个值。它们由 React Dev Tools 使用。
export const NoEffect = /*                     */ 0b000000000000000;
export const PerformedWork = /*                */ 0b000000000000001;

// 您可以更改其余部分(并添加更多内容)。
export const Placement = /*                    */ 0b000000000000010;
export const Update = /*                       */ 0b000000000000100;
export const PlacementAndUpdate = /*           */ 0b000000000000110;
export const Deletion = /*                     */ 0b000000000001000;
export const ContentReset = /*                 */ 0b000000000010000;
export const Callback = /*                     */ 0b000000000100000;
export const DidCapture = /*                   */ 0b000000001000000;
export const Ref = /*                          */ 0b000000010000000;
export const Snapshot = /*                     */ 0b000000100000000;
export const Passive = /*                      */ 0b000001000000000;
export const PassiveUnmountPendingDev = /*     */ 0b010000000000000;
export const Hydrating = /*                    */ 0b000010000000000;
export const HydratingAndUpdate = /*           */ 0b000010000000100;

// 被动 & 更新 & 回调 & Ref & 快照
export const LifecycleEffectMask = /*          */ 0b000001110100100;

// Union of all host effects
// 所有宿主效果的联合
export const HostEffectMask = /*               */ 0b000011111111111;

// These are not really side effects, but we still reuse this field.
// 这些都不是真正的副作用，但我们仍然重用这个字段。
export const Incomplete = /*                   */ 0b000100000000000;
export const ShouldCapture = /*                */ 0b001000000000000;
export const ForceUpdateForLegacySuspense = /* */ 0b100000000000000;

// Union of side effect groupings as pertains to subtreeTag
// 合并与subtreeTag相关的副作用分组
export const BeforeMutationMask = /*           */ 0b000001100001010;
export const MutationMask = /*                 */ 0b000010010011110;
export const LayoutMask = /*                   */ 0b000000010100100;
```

整个 **Scheduler** 与 **Reconciler** 的工作都在内存中进行，只有**当所有组件**都完成**Reconciler**的工作，才会统一交给**Renderer**。

React 这样解释 Reconciler
>[!info]
>![[Pasted image 20240827154515.png]]

### Renderer

`Renderer` 根据 `Reconciler` 为 vDom 打的 Tag， 同步执行对应的 DOM 操作

以下列代码为例
```tsx
import { useState } from 'react'
import './App.css'

function App() {
  const [count, setCount] = useState(1)

  const handleClick = () => { 
    setCount((count)=>count+1)
  }
  return (
      <ul>
      <button onClick={handleClick}>乘以 {count}</button>
      <li>{1 * count}</li>
      <li>{2 * count}</li>
      <li>{3 * count}</li>
      </ul>
  )
}

export default App
```

他的更新流程为：
![[Pasted image 20240827164635.png]]

<span style="border="2px solid red">红色框</span>内