# Redux中间件(升级dispatch)

> 参考: https://segmentfault.com/a/1190000007843340

**作用**: 封装一个新的dispatch,拦截从action==>reducer的过程,也就是可以加工action,也可以在这过程中干点别的事情


**应用中间件**:applyMiddleware-----可以应用多个中间件

**中间件例子=>创建store-使用日志输出中间件**:
	
> ```javascript	
> `import {createStore, applyMiddleware} from 'redux'`-----创建store的函数,应用中间件的函数
`import createLogger from 'redux-logger'`----一个中间件生成函数
`import rootReducer from './reducers'`----reducer用来接收action输出一个新的state
>
`const loggerMiddleware = createLogger()`---创建一个中间件
`const initialState = {}`---初始状态state
> return createStore(
> 	*第一个参数reducer,
> 	*第二个参数默认的state,
> 	*第三个参数就可以传 执行使用中间件的函数
> 		rootReducer,
> 		initialState,
> 		applyMiddleware(
> 			loggerMiddleware
> 		)
> 	)
> ```

**中间件写法**

```javascript
function createLogger(options = {}) {
  /**
   * 传入 applyMiddleWare 的函数
   * @param  {Function} { getState      }) [description]
   * @return {[type]}      [description]
   */
  return ({ getState }) => (next) => (action) => {
    let returnedValue;//承载输出的值
    const logEntry = {};
    logEntry.prevState = stateTransformer(getState());
    logEntry.action = action;
    // .... 
    returnedValue = next(action);
    // ....
    logEntry.nextState = stateTransformer(getState());
    // ....
    return returnedValue;
  };
}

export default createLogger;
```
------

### 源码分析(8月23): 

#### 首先创建store的方法有两种:

> ```js
> let store = applyMiddleware(logger)(createStore)(rootReducer);
> ```
> 
> 也可以直接这样，可以参考createStore
> 
> ```js
> let store = createStore(
>     rootReducer,
>     applyMiddleware(logger)
> )
> ```


*[注]: 第一种以applyMiddleWare作为唯一调用者, 比较直观, 所以依照第一个来分析*

#### **前置知识**


> reduceRight是compose方法的**核心**
> 
> ```js
> //reduceRight遍历介绍(一个数组方法,从右往左,执行)
> [0, 1, 2, 3, 4].reduceRight(function(previousValue, currentValue, index, array) {
>     return previousValue + currentValue;
> }, 10);
> 
> //结果 10+4+3+2+1+0 = 20
> ```

#### 源码

*applyMiddleware*

```js
export default function applyMiddleware(...middlewares) {
    return (createStore) => (reducer, initialState, enhancer) => {
        var store = createStore(reducer, initialState, enhancer)
        var dispatch = store.dispatch
        var chain = []
        var middlewareAPI = {
            getState: store.getState,
            dispatch: (action) => dispatch(action)
        }
        chain = middlewares.map(middleware => middleware(middlewareAPI))
        dispatch = compose(...chain)(store.dispatch)
        return {
            ...store,
            dispatch
        }
    }
}
```
*compose*

```js
export default function compose(...funcs) {
    if (funcs.length === 0) {
        return arg => arg
    }

    if (funcs.length === 1) {
        return funcs[0]
    }

    const last = funcs[funcs.length - 1]
    const rest = funcs.slice(0, -1)
    return (...args) => rest.reduceRight((composed, f) => f(composed), last(...args))
}
```

#### 自己的话讲一遍:

创建store的代码执行完以后就是一段代码:
store = (() => {}) .call(reducer)

> 函数体是关键, 主要做了这么几件事: 
> 1. 创建了redux原生store(之后增强dispatch后要改造这个store, 最后返回, 被外边的store赋值语句拿到)
> 3. 传入store的缩略版(只有两个重要函数: getStore/dispatch)调用每个中间件,  把每个中间件去了一层
> 4. dispatch 传入上一步的处理后的中间件数组, 在原生store里的dispatch的基础上改造,重新赋值,
> 5. 最后把升级后的store返回

***核心语句是第四步, 升级dispatch, 下面分析***

1. 当只有一个中间件的时候, compose函数里会直接返回这个中间件, 此时他的结构是: next => action => ...,    传入store的dispatch,  我们可以知道, 这个时候next就是dispatch,  然后正常的执行就可以了

2. 当有多个中间件 A, B, C时, compose函数会返回一个函数(这个函数将来的参数其实就是dispatch):
`(...args) => rest.reduceRight((composed, f) => f(composed), last(...args))`

rest是除了最后一个中间件以外的中间件
last是最后一个中间件

所以这个函数做的事情就是利用reduceRight函数, 先执行last(...args) , 也就是C(dispatch)
然后`newDispatch = A(B(C(dispatch)))`

进一步分析:

这个嵌套函数的最终格式其实是:A中间件传入next后返回的函数

```js
A: (action) => {
    next(action???)
    /*
        这个next其实是 B: (action) => {
            next(action???)
            /*
                这个next其实是 C: (action) => {
                    next(action???)
                    // 这是最后一个中间件, next是dispatch
                }
            /*
            ....
        }
    */
    ....
}
```

所以当组件dispatch一个action的时候, 就进入了这个"嵌套函数"
`A => B => C => C(next后的逻辑) => B(next后的逻辑) => A(next后的逻辑)`


> 关于返回值,  常见的是异步中间件里返回一个promise对象,之后的回调可以通过then,catch绑定
>   如果他不是第一个中间件, 而且之前的中间件没有对他的返回值做捕获, 就没办法用, 所以他之前如果有自己封装的中间件的话, 建议把next()后的结果return出去, 这样任何中间件想要返回东西的时候不会有障碍
> 
> 一句话, 建议在写中间件的时候, 在最后都要 return next(xxx)  [thunk, logger都是这么做的, 目的就是传递返回值]