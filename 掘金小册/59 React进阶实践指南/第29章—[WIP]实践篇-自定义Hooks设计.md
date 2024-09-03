
          
## 一 前言

本章节将围绕自定义 hooks 展开，本章节含的知识点如下：

* 自定义 hooks 的设计和编写。
* 几个自定义 hooks 实战。

## 二 全面理解自定义 hooks 

在 hooks 原理章节，详细介绍了 `React Hooks` 原理，在其他的章节，也陆续讲解了所有常用的 hooks 用法。接下来针对 hooks 进行功能性拓展，来研究一下在 React 中一种逻辑复用，组件强化方式——自定义 hooks 。

### 1 概念

自定义 hooks 是在 React Hooks 基础上的一个拓展，可以根据业务需求制定满足业务需要的组合 hooks ，更注重的是逻辑单元。通过业务场景不同，到底需要React Hooks 做什么，怎么样把一段逻辑封装起来，做到复用，这是自定义 hooks 产生的初衷。

自定义 hooks 也可以说是 React Hooks 聚合产物，其内部有一个或者多个 React Hooks 组成，用于解决一些复杂逻辑。

一个传统自定义 hooks 长如下的样子：

编写：
````js
function useXXX(参数A,参数B){
    /*  
     ...自定义 hooks 逻辑
     内部应用了其他 React Hooks —— useState | useEffect | useRef ...
    */
    return [xxx,...]
}

````
使用：

````js
const [ xxx , ... ] = useXXX(参数A,参数B...)
````

实际上自定义 hooks 的编写很简单，开发者只需要关心，传入什么参数（也可以没有参数），和返回什么内容就可以了，当然有一些监听和执行副作用的自定义 hooks ，根本无需返回值。

自定义 hooks 参数可能是以下内容：

*  hooks 初始化值。
* 一些副作用或事件的回调函数。
* 可以是 useRef 获取的 DOM 元素或者组件实例。
* 不需要参数

自定义 hooks 返回值可能是以下内容：

* 负责渲染视图获取的状态。
* 更新函数组件方法，本质上是 useState 或者 useReducer。
* 一些传递给子孙组件的状态。
* 没有返回值。

### 2 特性

上述讲到了自定义 hooks 基本概念，接下来分析一下它的特性。

#### ① 驱动条件

首先要明白一点，开发者写的自定义 hooks 本质上就是一个函数，而且函数在函数组件中被执行。那么**自定义 hooks 驱动本质上就是函数组件的执行**。

自定义 hooks 驱动条件：

* props 改变带来的函数组件执行。
* useState | useReducer 改变 state 引起函数组件的更新。



![1.jpg](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9fca8bd0234c43fda413c71b2023d8a2~tplv-k3u1fbpfcp-watermark.image)

#### ② 顺序原则

自定义 hooks 内部至少有一个 React Hooks ，那么自定义 hooks 也要遵循 hooks 的规则，**不能放在条件语句中，而且要保持执行顺序的一致性。** 至于为什么？ 在 hooks 原理章节已经讲过了。

#### ③ 条件限定

在自定义 hooks 中，条件限定**特别重要**。为什么这么说呢，因为考虑 hooks 的限定条件，是一个出色的自定义 hooks 重要因素。举个例子：

一些同学容易滥用自定义 hooks 导致一些问题的发生 ，比如在一个自定义这里写：

````js
function useXXX(){
    const value = React.useContext(defaultContext)
    /* .....用上下文中 value 一段初始化逻辑  */
    const newValue = initValueFunction(value) /* 初始化 value 得到新的 newValue  */
    /* ...... */
    return newValue
}
````
比如上述一个非常简单自定义 hooks ，从 `context` 取出状态 value ，通过 `initValueFunction` 加工 value ，得到并返回最新的 newValue 。如果直接按照上述这么写，会导致什么发生呢？

首先每一次函数组件更新，就会执行此自定义 hooks ，那么就会重复执行初始化逻辑，重复执行`initValueFunction` ，每一次都会得到一个最新的 newValue 。 如果 newValue 作为 `useMemo` 和 `useEffect` 的 deps ，或者作为子组件的 props ，那么子组件的浅比较 props 将失去作用，那么会带来一串麻烦。

那么如何解决这个问题呢？答案很简单，可以通过 useRef 对 newValue 缓存，然后每次执行自定义 hooks 判断有无缓存值。如下：

````js
function useXXX(){
    const newValue =  React.useRef(null)  /* 创建一个 value 保存状态。  */
    const value = React.useContext(defaultContext)
    if(!newValue.current){  /* 如果 newValue 不存在 */
          newValue.current = initValueFunction(value)
    }
    return newValue.current
}
````

* 用一个 useRef 保存初始化过程中产生的 value 值 。
* 判断如果 value 不存在，那么通过 initValueFunction 创建，如果存在直接返回 newValue.current 。

如上加了条件判断之后，会让自定义 hooks 内部按照期望的方向发展。条件限定是编写出色的 hooks 重要的因素。

#### ④ 考虑可变性

在编写自定义 hooks 的时候，可变性也是一个非常重要的 hooks 特性。什么叫做可变性，**就是考虑一些状态值发生变化，是否有依赖于当前值变化的执行逻辑或执行副作用。**

比如上面的例子🌰中，如果 defaultContext 中的 value 是可变的，那么如果还像上述用 useRef 这么写，就会造成 context 变化，得不到最新的 value 值的情况发生。

所以为了解决上述可变性的问题：

* 对于依赖于可变性状态的执行逻辑，可以用 `useMemo` 来处理。
* 对于可变性状态的执行副作用，可以用 `useEffect` 来处理。 
* 对于依赖可变性状态的函数或者属性，可以用`useCallback`来处理。
于是需要把上述自定义 hooks 改版。

````js
function useXXX(){
    const value = React.useContext(defaultContext)
    const newValue = React.useMemo(()=> initValueFunction(value) ,[  value  ] )  
    return  newValue
}
````

* 用 React.useMemo 来对 initValueFunction 初始化逻辑做缓存，当上下文 value 改变的时候，重新生成新的 newValue 。

这只是一个简单例子，在实际开发中，要比这种情况复杂。开发者应该注意在自定义 hooks 中，哪些状态是可变的，状态改变，又会紧跟着哪些影响。

#### ⑤ 闭包效应

闭包也是自定义 hooks 应该注意的问题。这个问题和 ④ 本质一样。首先函数组件更新就是函数本身执行，一次更新所有含有状态的 hooks （ `useState` 和 `useReducer` ）产生的状态 state 是重新声明的。但是如果像 `useEffect` ， `useMemo` ，`useCallback` 等，它们内部如果引用了 state 或 props 的值，而且这些状态最后保存在了函数组件对应的 fiber 上，那么此次函数组件执行完毕后，这些状态就不会被垃圾回收机制回收释放。这样造成的影响是，上述 hooks 如果没有把内部使用的 state 或 props 作为依赖项，那么内部就一直无法使用最新的 props 或者 state 。

比如我举个简单的例子。

````js
function useTest(){
    const [ number ] = React.useState(0)
    const value = React.useMemo(()=>{
         // 内部引用了 number 进行计算
    },[])
}
````
* 如上 useMemo 内部使用了 state 中的 number 进行计算，当 number 改变但是无法得到最新的 value 。这就是上面我说到的闭包问题。解决方法就是 useMemo 的 deps 中加入 number。

但是有的时候这种依赖关系往往是更复杂的。我将如上 demo 修改。

````js
function useTest(){
    const [ number ] = React.useState(0)
    const value = React.useMemo(()=>{
         // 内部引用了 number 进行计算
    },[ number ])
    const callback = React.useCallback(function(){
         // 内部引用了 useEffect
    },[ value ])
    
}
````
* 如上，在之前的基础上，又加了 useCallback 而且内部引用了 useMemo 生成的 value。 这个时候如果 useCallback 执行，内部想要获取新的状态值 value，那么就需要把 value 放在 useCallback 的 deps 中。

**🤔思考：如何分清楚依赖关系呢？**

* **第一步**：找到 hooks 内部可能发生变化的状态 ， 这个状态可以是 state 或者 props。
* **第二步**：分析 useMemo 或者 useCallback 内部是否使用上述状态，或者是否**关联使用** useMemo 或者 useCallback 派生出来的状态（ 比如上述的 value ，就是 useMemo 派生的状态 ） ，如果有使用，那么加入到 deps 。
* **第三步**：分析 useEffect ，useLayoutEffect ，useImperativeHandle 内部是否使用上述两个步骤产生的值，而且还要这些值做一些副作用，如果有，那么加入到 deps 。

## 三 自定义 hooks 设计

上述介绍了自定义 hooks 的概念和特性，接下来重点分析一下，如何去设计一个自定义 hooks 。

首先明确的一点是，自定义 hooks 解决逻辑复用的问题，那么在正常的业务开发过程中，要明白哪些逻辑是重复性强的逻辑，这段逻辑主要功能是什么。

下面我把自定义 hooks 能实现的功能化整为零，在实际开发中，可能是下面一种或者几种的结合。

### 1 接收状态

自定义 hooks ，可以通过函数参数来直接接收组件传递过来的状态，也可以通过 useContext ，来隐式获取上下文中的状态。比如 React Router 中最简单的一个自定义 hooks —— useHistory ，用于获取 history 对象。

````js
export default function useHistory() {
    return useContext(RouterContext).history
}
````

注意⚠️：**如果使用了内部含有 useContext 的自定义 hooks ，那么当 context 上下文改变，会让使用自定义 hooks 的组件自动渲染。**

### 2 存储｜管理状态

**储存状态**

自定义 hooks 也可以用来储存和管理状态。本质上应用 useRef 保存原始对象的特性。

比如 `rc-form` 中的 `useForm` 里面就是用 useRef 来保存表单状态管理 Store 的。简化流程如下

````js
function useForm(){
    const formCurrent = React.useRef(null)
    if(!formCurrent.current){
        formCurrent.current = new FormStore()
    }
    return formCurrent.current
}
````

**记录状态**

当然 useRef 和 useEffect 可以配合记录函数组件的内部的状态。举个例子，我编写一个自定义 hooks 用于记录函数组件执行次数，和是否第一次渲染。

````js
function useRenderCount(){
    const isFirstRender = React.useRef(true) /* 记录是否是第一次渲染 */
    const renderCount = React.useRef(1)      /* 记录渲染次数 */
    useEffect(()=>{
        isFirstRender.current = false        /* 第一次渲染完成，改变状态 */
    },[])
    useEffect(()=>{
        if(!isFirstRender.current) renderCount.current++ /* 如果不是第一次渲染，那么添加渲染次数  */
    })  
    return [ renderCount.current , isFirstRender.current ]
} 
````
* 如上用 isFirstRender  记录是否是第一次渲染 ，用 renderCount 记录渲染次数，第一个 useEffect 依赖项为空，只执行一次，第二个 useEffect 没有依赖项，每一次函数组件执行，都会执行，统计渲染次数。

上述只是举了一个例子，当然在具体开发中，可以用自定义 hooks 去记录一些其他的东西。比如元素的信息，因为可以在 useEffect 中获取到最新的 DOM 元素信息的。 

### 3 更新状态

**改变状态**

自定义 hooks 内部可以保存状态，可以把更新状态的方法暴露出去，来改变 hooks 内部状态。而更新状态的方法可以是组合多态的。

比如实现一个防抖节流的自定义 hooks ：

````js
export function debounce(fn, time) {
    let timer = null;
    return function(...arg) {
      if (timer) clearTimeout(timer);
      timer = setTimeout(() => {
        fn.apply(this, arg);
      }, time);
    };
}

function useDebounceState(defauleValue,time){
    const [ value , changeValue ] = useState(defauleValue)
    /* 对 changeValue 做防抖处理   */
    const newChange = React.useMemo(()=> debounce(changeValue,time) ,[ time ])
    return [ value , newChange ]
}
````
使用：

````js
export default function Index(){
    const [ value , setValue ] = useDebounceState('',300)
    console.log(value)
    return <div style={{ marginTop:'50px' }} >
        《React 进阶实践指南》
        <input placeholder="" onChange={(e)=>setValue(e.target.value)}  />
    </div>
}
````

效果：

![3.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5aa90ff8fe8f401e8ba669af510c35d3~tplv-k3u1fbpfcp-watermark.image)

**组合state**

自定义 hooks 可以维护多个 state ，然后可以组合更新函数。我这么说可能很多同学不理解，下面我来举一个例子，比如控制数据加载和loading效果，

````js
function useControlData(){
    const [ isLoading , setLoading ] = React.useState(false)
    const [ data,  setData ] = React.useState([])
    const getData = (data)=> { /* 获取到数据，清空 loading 效果  */
        setLoading(false)
        setData(data)
    }  
    // ... 其他逻辑
    const resetData = () =>{  /* 请求数据之前，添加 loading 效果 */
        setLoading(true)
        setData([])
    }
    return [ getData , resetData , ...  ] 
}
````

**合理state**

useState 和 useRef 都可以保存状态：

* useRef 只要组件不销毁，一直存在，而且可以随时访问最新状态值。
* useState 可以让组件更新，但是 state 需要在下一次函数组件执行的时候才更新，而且如果想让 useEffect 或者 useMemo 访问最新的 state 值，需要将 state 添加到 deps 依赖项中。

自定义 hooks 可以通过 useState + useRef 的特性，取其精华，更合理的管理 state。比如如下实现一个**同步的state**

````js
function useSyncState(defaultValue){
   const value = React.useRef(defaultValue)        /* useRef 用于保存状态 */
   const [ ,forceUpdate ] = React.useState(null)   /* useState 用于更新组件 */
   const dispatch = (fn) => {                      /* 模拟一个更新函数 */
       let newValue
       if( typeof fn === 'function' ){
            newValue = fn(value.current)           /* 当参数为函数的情况 */
       }else{
           newValue = fn                           /* 当参数为其他的情况 */
       }
       value.current = newValue
       forceUpdate({})                             /* 强制更新 */
   } 
   return [  value , dispatch  ]                   /* 返回和 useState 一样的格式 */
}
````

* useRef 用于保存状态 ，useState 用于更新组件。 
* 做一个 `dispatch` 处理参数为函数的情况。在 dispatch 内部用 forceUpdate 触发真正的更新。
* 返回的结构和 useState 结构相同。不过注意的是使用的时候要用 value.current 。

使用：

````js
export default function Index(){
    const [ data , setData  ] = useSyncState(0)
    return <div style={{ marginTop:'50px' }} >
        《React 进阶实践指南》 点赞 👍 { data.current }
       <button onClick={ ()=> {
           setData(num => num + 1)
           console.log(data.current) //打印到最新的值
       } } >点击</button>
    </div>
}
````


### 4 操纵 DOM / 组件实例

自定义 hooks 也可以设计成对原生 DOM 的操纵控制。究其原理用 useRef 获取元素， 在 useEffect 中做元素的监听。

比如如下场景，用一个自定义 hooks 做一些基于 DOM 的操作 。

````js
/* TODO: 操纵原生dom  */
function useGetDOM(){
    const dom = React.useRef()
    React.useEffect(()=>{
       /* 做一些基于 dom 的操作 */
       console.log(dom.current)
    },[])
    return dom
}
````
* 自定义 useGetDOM ，用 useRef 获取 DOM 元素，在 useEffect 中做一些基于 DOM 的操作。

使用：

````js
export default function Index(){
    const dom = useGetDOM()
    return <div ref={ dom } >
        《React进阶实践指南》
        <button >点赞</button>
    </div>
}
````


### 5 执行副作用

自定义 hooks 也可以执行一些副作用，比如说监听一些 props 或 state 变化而带来的副作用。比如如下监听，当 `value` 改变的时候，执行 `cb`。

````js
function useEffectProps(value,cb){
    const isMounted = React.useRef(false)
    React.useEffect(()=>{
         /* 防止第一次执行 */
        isMounted.current && cb && cb()
    },[ value ])
    React.useEffect(()=>{
          /* 第一次挂载 */
         isMounted.current = true
    },[])
}
````
* 用 useRef 保存是否第一次的状态。然后在一个 useEffect 改变加载完成状态。
* 只有当不是第一次加载且 value 改变的时候，执行回调函数 cb 。
* 当使用这个自定义 hooks 就可以监听，props 或者 state 变化。接下来尝试一下。

使用组件和父组件：
````js
function Index(props){
    useEffectProps( props.a ,()=>{/* 监听 a 变化 */
        console.log('props a 变化:', props.a  )
    } )
    return <div>子组件</div>
}
export default function Home(){
    const [ a , setA ] = React.useState(0)
    const [ b , setB ] = React.useState(0)
    return <div>
        <Index a={a}  b={b} />
        <button onClick={()=> setA(a+1)} >改变 props a  </button>
        <button onClick={()=> setB(b+1)} >改变 props b  </button>
    </div>
}
````
效果：


![2.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/abe8ac2134bf40c890a71e0b12688879~tplv-k3u1fbpfcp-watermark.image)


* 当动态监听 props.a ，props.a 变化，监听函数执行。


### 6 持续维护中～

本章节，第二十七章节，第十四章节为持续维护章节，会有更多精彩的自定义 hooks 设计场景。

## 四 总结

本章节学习的内容如下：

* 自定义 hooks 的概念与特性。
* 自定义 hooks 设计方式。

下一章将介绍自定义 hooks 实践。
