React18 提供了一个新的 API `useSyncExternalStore`，在我们日常开发中不会用到，但对于状态管理库来说则非常重要。`useSyncExternalStore` 提供了一种标准化的方式来共享外部状态，并保证组件与这些外部状态源的同步，简化了跨组件状态共享的复杂度，也解决了我们在上文提到的 React Tearing 的问题。

`useSyncExternalStore` 的基本用法如下：
```javascript
const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?)
```

- `subscribe`：一个函数，接受一个监听器（listener）回调。当外部数据源更新时，这个监听器应该被调用，以通知 React 组件需要重新渲染。这个函数应该返回一个取消订阅的函数。

- `getSnapshot`：一个函数，返回当前的外部状态快照。React 会在订阅外部数据源时调用它来获取最初的状态，之后每当外部数据源通知 React 更新时，也会调用它来获取最新状态。

- `getServerSnapshot`（可选）：在服务端渲染（SSR）中使用，返回当前外部状态的快照，类似于 getSnapshot，但专门用于 SSR 使用。