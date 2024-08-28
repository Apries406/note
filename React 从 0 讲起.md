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

<span style="border: 2px dashed red; padding: 2px;">红色框</span> 内的部分随时都可能由于以下原因被打乱：
- 有其他更高优先级任务需要先更新
- 当前帧无空余时间
由于框内任务都在内存中进行，不会刷新页面上的 DOM , 因此即使我们反复中断任务，用户也不会看见更新不完全的 DOM

## Fiber 架构

`Fiber`并不是计算机术语中的新名词，他的中文翻译叫做`纤程`，与进程（Process）、线程（Thread）、协程（Coroutine）同为程序执行过程。

`React Fiber`可以理解为：

`React`内部实现的一套状态更新机制。支持任务不同`优先级`，可中断与恢复，并且恢复后可以复用之前的`中间状态`。

虚拟 DOM 在 React 中有一个正式的称呼 —— `fiber`。

### Fiber 的起源

在 React15 及以前， Reconciler 采用递归的方式创建、处理 vDom，递归过程是不能中断的。如果组件树的层级很深，递归会占用线程很多时间，造成卡顿。

为了解决这个问题， React16 将**无法中断的递归更新**重构为了**可中断的异步更新**，由于曾经用于递归的**虚拟 DOM**数据结构已经无法满足需要，于是推出了全新的**Fiber**架构

### Fiber 的三种含义

1. 从**架构**上来说，之前的 Reconciler 采用递归的方式执行，数据保存在递归调用栈中，所以被称为`stack Reconciler`。React 16 的 Reconciler 基于 `Fiber 节点` 实现，被称为`Fiber Reconciler`
2. 从**数据结构**上来说，每个`Fiber 节点`对应一个`React Element`，保存了该组件的类型，对应的 DOM 节点等信息
3. 从**动态工作单元**上来说，每个`Fiber 节点`保存了本次更新中**组件改变的状态**、**要执行的工作**

### Fiber 的结构

```typescript
function FiberNode(
  tag: WorkTag,
  pendingProps: mixed,
  key: null | string,
  mode: TypeOfMode,
) {
  // 静态数据结构属性
  this.tag = tag;
  this.key = key;
  this.elementType = null;
  this.type = null;
  this.stateNode = null;

  // 用于链接其他 Fiber 节点形成 Fiber 树
  this.return = null;
  this.child = null;
  this.sibling = null;
  this.index = 0;

  this.ref = null;

  // 作为动态工作单元的属性
  this.pendingProps = pendingProps;
  this.memoizedProps = null;
  this.updateQueue = null;
  this.memoizedState = null;
  this.dependencies = null;

  this.mode = mode;

  // Effects
  this.effectTag = NoEffect;
  this.subtreeTag = NoSubtreeEffect;
  this.deletions = null;
  this.nextEffect = null;

  this.firstEffect = null;
  this.lastEffect = null;

  // Scheduler 调度优先级相关
  this.lanes = NoLanes;
  this.childLanes = NoLanes;

  // 指向该 Fiber 在领一次更新时对应的 Fiber
  this.alternate = null;
  }
```

#### 架构解析 Fiber

每个 Fiber 节点都对应着一个 React Element, 那么多个 Fiber 节点是如何链接形成 Fiber 树的呢？ 依靠如下三个属性:
```typescript
{
	this.return = null;  // 指向父级 Fiber
	this.child = null;   // 指向最左第一个子 Fiber
	this.sibling = null; // 指向右边第一个兄弟 Fiber
}
```

现在来举个例子，例如下列的组件结构：
```tsx
function App() {
  return (
    <div>
      <h1>child 1</h1>
      <h2>child 2</h2>
    </div>
  )
}
```

这样的一个组件对应着如下 Fiber 树结构：
![[Pasted image 20240827214917.png]]
#### 静态数据结构解析 Fiber
```typescript
{
  // 静态数据结构属性

  // Fiber 对应组件的类型：Function / Class / Host
  this.tag = tag; 
  // key 属性
  this.key = key;
  // 大部分情况同 type, 某些情况不同，比如用 React.memo 包裹 FC
  this.elementType = null;
  // FC -> 函数本身， CC -> Class，HC -> DOM 节点 TagName
  this.type = null;
  // 指向真实 DOM 节点 
  this.stateNode = null;
}
```

#### 动态工作单元解析 Fiber
```typescript
{
 // 作为动态工作单元的属性
  this.pendingProps = pendingProps;
  this.memoizedProps = null;
  this.updateQueue = null;
  this.memoizedState = null;
  this.dependencies = null;

  this.mode = mode;

  // 本次更新会造成的 DOM 操作
  this.effectTag = NoEffect;
  this.subtreeTag = NoSubtreeEffect;
  this.deletions = null;
  this.nextEffect = null;

  this.firstEffect = null;
  this.lastEffect = null;

  // Scheduler 调度优先级相关
  this.lanes = NoLanes;
  this.childLanes = NoLanes;
}
```

## Fiber 架构的工作原理

从 Fiber 架构中我们知道
- Fiber 节点能保存 DOM 节点
- Fiber 节点构成的 Fiber 树就对应了 DOM 树
那我们该如何更新 DOM 呢？=> 这就需要使用“双缓存”技术

### “双缓存”技术

我们在使用`canvas`绘制动画时，每一帧绘制前都会调用`ctx.clearRect`清除上一帧。
但是如果当前帧需要庞大的计算量时，我们清除上一帧的画面再绘制当前帧的画面就会有较长的时间间隔，从而出现白屏现象。

为了解决这个问题，我们可以在内存中抢先绘制当前帧动画，绘制完毕后直接替换上一帧。这样就节省了两帧替换时产生的计算时间，就不会出现白屏闪烁现象。

这种在**内存中构建并替换**的技术叫做**双缓存**

`React` 使用“双缓存”来完成`Fiber Tree`的构建与替换，即`DOM Tree`的**创建**与**更新**

### 双缓冲 Fiber 树
