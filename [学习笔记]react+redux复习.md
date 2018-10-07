webpack脚本 / es6模块系统(as default ...)

## 组件化

reactjs的最大的特性就是组件化，组件化的目的

1. 就是为了能够进行**复用**，减少代码的冗余
2. 降低问题复杂度, 分而治之

像搭积木一样去构建应用

考虑复用的时候, 规划好这个组件向外暴露的接口, eventHandler,  (一个规范相关的思考, 这些接口以onXXX开头, 父容器的叫handleXXX)

外部传入: 

1. 内部规定的eventHandlers
2. 显示UI时所需要的数据

自己分装组件的时候, 可以提供一个 static defaultProps


# create-react-app工具: 

1. 安装: `npm install --global create-react-app`
2. 执行: `create-react-app react-app-1`
3. 启动: `cd react-app-1` => `npm start` (默认3000端口)
4. create-react-app工具内部使用了 `react-scripts`包, 想要放弃使用这个工具, 来探索内部配置细节, 使用`npm run eject` - 弹射, 把原生的配置文件暴露出来(不可逆)

# package.json的脚本(scripts)

1. build - 创建生产环境的代码
2. test - 单元测试
3. eject - 弹射, 把原生的配置文件暴露出来


# this的指向问题  ---待完善

普通情况就是谁调用就是谁, 调用的的时候确定的
箭头函数则不一样, 是在定义的时候就确定不变了,(所以bind其实不会对他生效)

#  es6模块系统 / CommonJS:
- `import a from (.path)/  export default a`
-   require('./example.js') / module.exports.x =  (读取变量)
    
# 组件

> 在使用jsx的代码文件中, 必须导入 React,   *这是因为jsx最终会被转义成依赖于react的表达式*
> 绑定事件: 直接onClick
> 原则: 做一件事的代码应该高耦合
> 传统html的onclick和 jsx的onClick 的区别:
    onclick: 全局环境执行, 污染全局变量 / 给很多DOM元素添加onclick事件, 会影响网页性能 / 元素删掉同时需要把事件处理器(事件处理函数)注销, 如果忘记, 会造成内存泄漏
    onClick: 使用了事件委托 / 掌控了组件的生命周期, unmount的时候会自动清除相关的事件处理函数
    
> 样式可以通过 style属性   `<div style={{margin: 123px}}>`

# tips

1. jsx 当props的类型不是字符串, 必须用`{}`把props的值包住
2. 如果一个组件要定义自己的构造函数, 一定要记得在构造函数constructor第一行通过super(props)调用父类(即reactComponent)的构造函数
    因为: 不这样的话, 组件实例被构造之后, 类实例的所有成员函数就无法通过this.props访问到父组件传递过来的props值, 也就是说, 组件内部无法得到父组件赋予的props
    
# 事件绑定的bind

```js
constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);   
}
hanldeClick() {
    ...
}
```
另一种比较风骚的写法是箭头函数:

```js
handleClick = () => {
    ...
}
```
这实际是es7类属性初始化+箭头函数，即：

```js
constructor(props) {
    super(props);
    this.handleClick = () => {
        ...
    }
}
```

# propTypes

声明接口规范:

```js
Counter.propTypes = {
    title: PropTypes.string.isRequired,
    initValue: PropTypes.number
}
```

判断一些props的有无, 没有的话需要给一个默认值, 这样的逻辑充斥在逻辑代码中会比较乱, 所以

```js

Counter.defaulProps = {
    title: '默认标题'
}
```


# state
现在已经不需要通过getInitialState方法来获取初始state了, 
那种方法是老式的React.createClass创建组件类的时候使用的, 
现在只需要在构造函数的末尾直接写state的初始值就可以了


# REACT更新机制

只要父组件的render了，那么默认情况下就会触发子组件的render过程，子组件的render过程又会触发它的子组件的render过程，一直到React元素（即jsx中的<div>这样的元素）。当render过程到了叶子节点，即React元素的时候，diff过程便开始了，这时候diff算法会决定是否切实更新DOM元素。

你可能会觉得这样不是很傻吗，我又没有传递属性给子组件，那父组件更新会触发所有后代组件的重渲染过程不是很低效且没有意义吗？但是React不能检测到你是否给子组件传了属性，所以它必须进行这个重渲染过程（术语叫做reconciliation）。但是这不会使得react有多低效，因为reconciliation过程是执行的JavaScript，而重渲染的性能开销主要是更新DOM导致的，最后diff算法会介入，决定是否要真正更新DOM，JavaScript的执行速度很快的，所以即使父组件render会触发所有后代组件的render过程(reconciliation过程)，这个效率也不会有太大影响。

当然，从道理上讲，既然我没有给子组件传递属性，或者我的程序能够判断出传递的属性并没有发生变化，那么自然无需进行子组件的reconciliation过程。但是react无法自动检测这一点，于是它提供了shouldComponentUpdate回调函数，让程序员根据情况决定是否决定是否要重render本组件。如果某个组件的shouldComponentUpdate总是返回false, 那么当它的父组件render了，会触发该组件的render过程，但是进行到shouldComponentUpdate判断时会被阻止掉，从而就不调用它的render方法了，它自己下面的组件的render过程压根也不会触发了。

也可以`extends react.pureComponent`


# 生命周期

1. 装载

    1. constructor
    2. getInitialState (废弃, 可通过在constructor里直接初始化来替代)
    3. getDefaultProps (废弃, 可通过给类的属性defaultProps属性赋值来替代)
    4. componentWillMount (此时没有任何渲染出来的结果, *可以在这个生命周期里做的, 都可以提前到constructor里去做*)
    5. render(应该是一个纯函数, 不要在里边进行setState之类的操作)返回的只是一个结构
    6. .....(等待DOM同步)
    8. componentDidMount(并不是render完马上执行, render得到的结构同步到DOM需要一段时间, 这个函数里, ***DOM都已经完全渲染好***)
    
    > 多个子组件同时渲染的时候, 每个子组件的ComponentDidMount会在所有子组件渲染完毕后一起顺序触发,即
    >
    > ```md
    > 1st组件 constructor
    > 1st组件 componentWillMount
    > 1st组件 render
    > 2nd组件 constructor
    > 2nd组件 componentWillMount
    > 2nd组件 render
    > 3rd组件 constructor
    > 3rd组件 componentWillMount
    > 3rd组件 render
    > 1st组件 componentDidMount
    > 2nd组件 componentDidMount
    > 3rd组价 componentDidMount
    > ```
    > 也好理解, react需要把所有的组件返回的结果综合起来, 然后才知道如何对DOM进行修改, 修改完后才触发DidMount作为收尾
    
    

2. 更新(state / props 被更新的时候, 会引发组件的更新过程)
    - forceUpdate(强行重绘)


    1. componentWillReceiveProps(nextProps)(1. 只要父组件的render被调用, 子组件就会更新, 不论props是不是真的发生了改变; 2. state变化不会触发这个函数; 3. 这个函数用途是根据props来计算要不要更新state, 如果state改变又导致函数执行, 会陷入死循环)
    2. shouldComponentUpdate(nextProps, nextState) (决定一个组件什么时候不需要渲染; 要求返回布尔值结果)
    3. componentWillUpdate(nextProps, nextState)
    4. render
    5. componentDidUpdate

    
    
3. 卸载

    - componentWillUnmount (DOM删掉之前执行)


        > 在 componentDidMount 中用非 React 的方法创造了一些 DOM 元素，如果撒手不管可能会造 成内存泄露，那就需要在 componentWillUnmount 中把这些创造的 DOM 元素清理掉 。

# ssr

cross-env能跨平台地设置及使用环境变量
大多数情况下，在windows平台下使用类似于: NODE_ENV=production的命令行指令会卡住，windows平台与POSIX在使用命令行时有许多区别（例如在POSIX，使用$ENV_VAR,在windows，使用%ENV_VAR%。。。）

**生命周期不会走`componentDidMount`, 但是会走`componentDidUpdate `, 但正常情况下一个装载过程产出HTML字符串就结束了, 不会走这个生命周期**


**如果要在服务端渲染, 输出不是一个DOM树, 而是一个全是HTML的字符串**



# 求和工具
`array.reduce(callback, initValue)`
**使用reduce数据求和性能更好**

# 只使用react的缺陷

1. state同步问题, 同一个状态不同组件的state存储, 数据不一致要相信谁
2. props多级传递问题

# Flux

1. 创建dispatcher:  new Dispatcher()
2. action: 一般写成两个文件: ActionTypes.js / Actions.js(里面是actionCreator,如下)

    ```js
    import * as ActionTypes from './ActionTypes.js'
    import AppDispatcher from './AppDispatcher.js'
    
    export const action1 = (params) => {
        AppDispatcher.dispatch({
            type: ActionTypes.action1,
            params: params
        })
    }
    ```

3. Store

    作用: 存储状态, 响应action
    注意: 不要修改通过store得到的数据
    写法: 一个对象, 定义了getter/ 挂载变更回调 / 挂载变更回调 / 触发变更事件 的方法

    `store.js`
    
    ```js
    const initValue = {
        xx: 123
    }
    
    const store = Object.assign({}, EventEmitter.prototype, {
        getValue: function () {
           return initValue 
        }
        ...等四个方法
    
    })
    
    ```

4. Dispatcher

    ```js
    store.DispatchToken = AppDispatcher.register((action) => {
        if (action.type === ActionTypes.action1) {
            initValue[xx] ++;
            store.emit触发更新事件
        }
    
    })
    ```
    这是最重要的一个步骤, 把store注册到全局唯一的Dispatcher上
    
    token的作用是 各个store对于action的响应顺序不可控, 如果一个action需要先等待前置store响应, 那么需要这个token(store的唯一表示)
    
    `AppDispatcher.waitFor([CounterStore.dispatchToken]`


* 一个核心思想实现

    当一个动作被派发后,dispatcher就是简单的把所有注册的回调(注册的store函数)调用一遍



# Flux缺点

1. store间依赖处理不优雅
2. 难以进行服务端渲染,由于每个store的全局唯一性
3. 混杂了逻辑和状态


# Redux (Reducer + Flux)

> reducer的故事: 
    
>`array.reduce(reducer, initValue)`
>
> 1. reduce是一个计算机通用的概念, js数组的reduce方法如上, 就是把数组所有元素依次调用一遍'reducer'(做'规约')
> 2. reducer (注意不是 reduce)函数接受两个参数，第一个参数是上一 次规约的结果，第二个参数是这一次规约的元素


三个基本原则

1. 唯一数据源(Single Source ofTruth);
    - store的唯一性


2. 保持状态只读( State is read-only);
    - 只通过action修改, 同时改变状态不是直接修改状态上的值, 而是返回一个新的状态对象
3. 数据改变只能通过纯函数完成( Changes are made with pure functions)。
    - 纯函数是指`reducer(state, action)`
    - reducer要做的事就是根据state和action的值产生一个新的对象返回
    - reducer只负责计算状态, 不负责存储状态

    


redux和flux区别:

1. action: redux的action只返回一个action对象, flux的action会直接dispatch出去
2. dispatcher: flux需要一个dispatcher对象, 作用其实就是把一个action对象分发给指定的注册了的store, redux只有一个store, 所以不需要, 他只是一个store的函数,负责分发action到store
3. reducer(state, action) vs flux里的回调只有action一个参数...是因为flux里state(状态)是由每个store自己管理存储的; redux里state是框架本身存储的, reducer只关心更新state


## reducer

在reducer中, ***绝对不能*** 去修改参数中的state, reducer是一个纯函数, 不应该产生任何副作用

```js
export default (state, action) => {
    const {aa} = action;
    
    switch (action.type) {
        case ...
            return {...state, xxx: 'xxxx'}
        ...
        default:
            return state
    }
}
```

## store

创建store:

```js
import {createStore} from 'redux'
import reducer from './Reducer.js'

const initValue = {
    xx: 123
}

const store = createStore(reducer, initValue)

export default store
```

## view
1. 可以封装一个通用格式函数, 获取当前组件用到的那一部分store数据(里边可以对数据做一些二次加工处理)

    ```js
    getOwnState() {
        return { value: store.getState()[xxx] }
    }
    ```

1. 初始化state
    `store.getState().xxx......`
2. 实时同步组件的state 和 全局的store

    ```js
    componentDidMount() {
        store.subscribe(this.onChange)
    }
    componentWillUnmount() {
        store.unsubscribe(this.onChange)
    }
    
    onchange: function () {
        this.setState(this.getOwnState())
    }
    ```
3. 派发action来更改状态

    `store.dispatch(Actions.xxx(0))`



## 容器组件和傻瓜组件

redux框架下, 一个React组件要完成两个功能:

1. 和redux打交道, 
    - 读取Store,初始化; 
    - 监听Store变化, 更新组件状态, 驱动视图重新渲染; 
    - 派发action来更新store
2. 根据当前props/ state 渲染用户界面

考虑组件拆分, 一个组件拆成两个组件, 每个组件负责一件事情

原本完成所有任务的一个组件 => 负责redux打交道的外层**容器组件** + 负责渲染界面的内层**傻瓜组件** 

这是react组件的一种设计模式, 不是redux的

### 傻瓜组件

- **纯函数** + **无状态**组件, 只根据props产生结果
- 让傻瓜组件无状态, 是拆分的主要目的之一


```js
function CommentDisplay(props) {
  return <div>
            <div>{props.counterProp}</div>
        <br />
        <button onClick={props.incrementCounter}>+</button>
        <button onClick={props.decrementCounter}>-</button>
        </div>
}
```

另一种写法(解构赋值)


```js
function CommentDisplay({counterProp, incrementCounter, decrementCounter}) {
  return <div>
            <div>{counterProp}</div>
        <br />
        <button onClick={incrementCounter}>+</button>
        <button onClick={decrementCounter}>-</button>
        </div>
}
```


### 容器组件

- state都由他处理, 通过props传递给傻瓜组件




# context


一个应用中，最好只有一个地方需要直接导人 Store，
这个位置当然应该是在调用最 顶层 React组件的位置，其余组件应该避免直接导人 Store。

如果把store存到顶级组件 store多级传递的问题就凸显了出来

所谓 Context，就是“上下文环境”，让一个树状组件上所有组件都能访问一个共同 的对象

1. 首先，上级组件要宣称自己支持 context，并且提供一个函数来返回代表 Context 的 对象。
2. 然后，这个上级组件之下的所有子孙组件，只要宣称自己需要这个 context，就可以 通过 this.context访问到这个共同的环境对象。

## provider

用途: 创建一个 **`专门的context提供者组件`**, 用来包裹顶级组件 

```js
import {PropTypes, Component} from ’react’;
class Provider extends Component { 
    getChildContext{) {
        return {
            store: this.props.store
        }
    }
    render() {
        return this.props.children;
    }
}

Provider.childContextTypes = {
    store: PropTypes.object
}
```

getChildContext是重点, **他返回的就是context对象**, 
为了通用性, 没有在这个组件中引入store,而是要求通过props传递进来

为了让 Provider 能够被 React认可为一个 Context 的提供者
Provider还需要定义类 的 **`childContextTypes`**，必须和 **`getChildContext`**对应，只有这 **两者**都齐备， Provider 的子组件才有可能访问到 context。


这就是后来看到的一个成熟的入口文件的写法

```js
import store from ’./Store.js’
import Provider from ’./Provider.js'
ReactDOM.render(
    <Provider store={store}>
        <ControlPanel />
    </Provider> ,
    document .getElementByid ( ’ root ’)
)
```


## 后代组件在使用的时候

为了让 后代组件 能够访问到 context，必须给 该后代组件类的 context­
Types 赋值和 Provider.childContextTypes 一样的值，两者必须一致，不然就无法访问到 context，代码如下:

```js
ChildComponent1.contextTypes = { 
    store: PropTypes.object
}
```

同时, 如果自定义了构造函数, 要用上第二个参数context

```js
constructor(props, context) {
    super(props, context)
    ...
}
```
这样才能让组件的其他部分使用this.context

> 简单的写法:
> 
> ```js
> constructor () {
    super (... arguments) ;
> }
> ```


建议context只用来传递store, 因为毕竟是全局变量, 容易被误修改, 而store的只读性规避了这种风险



# react-redux

使用库提供的provider

这个库主要的两个功能

1. connect - 傻瓜组件和容器组件
    - `export default connect(mapStateToProps, mapDispatchToProps) (Counter);`
    - 把 Store上的状态转化为内层傻瓜组件的 prop; 
        ####     mapStateToProps(也可以叫 mapState)
        
        ```js
        function mapStateToProps(state, ownProps) { 
            return {
                value: state[ownProps.caption]
            }
        }
        ```
    - 把内层傻瓜组件中的用户动作转化为派送给 Store 的动作(也就是把内层傻瓜组件暴露出来的函数类型的 prop关联上 dispatch 函数的调用)
        ####     mapDispatchToProps(也可以叫 mapDispatch)
        
        ```js
        function mapDispatchToProps(dispatch, ownProps) { 
            return {
                onincrement: () => {
                    dispatch(Action.increment(ownProps.caption));
                }
                onDecrement: () => {
                    dispatch(Action.decrement(ownProps.caption));
                }
            }
        }
        ```
    
2. Provider - 提供包含store的context


    一个Redux的Store的条件: 
    
    1. subscribe
    2. dispatch
    3. getState

    另外, react-redux重新定义了Provider的componentWillReceiveProps函数, 在每次重新渲染时都会调用, 里边会检查props里的store是否一样, 不一样会警告
    

# HOC-React高阶组件

> 替换mixin??

高阶组件的概念应该是来源于JavaScript的高阶函数:

> `高阶函数就是接受函数作为输入或者输出的函数`
> `高阶组件仅仅只是是一个接受组件组作输入并返回组件的函数`

实现方式通常有两种:

1. 属性代理(Props Proxy)
    实质上是通过包裹原来的组件来操作props 
    
    ```js
    import React, { Component } from 'React';
    //高阶组件定义
    const HOC = (WrappedComponent) =>
      class WrapperComponent extends Component {
        render() {
            const newProps = {
                name: 'HOC'
            }
            return <WrappedComponent
                {...this.props}
                {...newProps}
            />;
        }
    }
    //普通的组件
    class WrappedComponent extends Component{
        render(){
            //....
        }
    }
    
    //高阶组件使用
    export default HOC(WrappedComponent)
    ```
    
    > **上面的例子非常简单，但足以说明问题。**
    > 我们可以看见函数HOC返回了新的组件(WrapperComponent)，
    > **这个组件原封不动的返回作为参数的组件(也就是被包裹的组件:WrappedComponent)，**
    > **并将传给它的参数(props)全部传递给被包裹的组件(WrappedComponent)。**
    > 这么看起来好像并没有什么作用，
    > 其实属性代理的作用还是非常强大的。
2. 反向继承(Inheritance Inversion)



-----


登科hoc例子
    
class Header {
  render() {
    <div>
      {this.props.renderAcvatar()}
    </div>;
  }
}

const cicleAvatarHoc = WrappedComponent => class CircleAvatarHeader {
  renderCircleAvatar() {

  }

  render() {
    <WrappedComponent renderAvatar={this.renderCircleAvatar()} />;
  }
};
const CircleAvatarHeader = cicleAvatarHoc(Header);

const squareAvatarHoc = WrappedComponent => class SquareAvatarHeader {
  renderCircleAvatar() {

  }
  render() {
    <WrappedComponent renderAvatar={this.renderCircleAvatar()} />;
  }
};

const CircleAvatarHeader = cicleAvatarHoc(Header);

renderCircleAvatar

  < Header renderAvatar= { this.renderCircleAvatar() } />
  

一、什么是React高阶组件？
> 就是函数接受一个组件，返回一个新组件。

如果我们用过react-redux，就一定看过类似这段代码 `connect(mapDispatchToProps,mapStateToProps)(TabPage)`。这就是react的HOC，把组件当作参数传入，在内部经过处理和额外封装，

**达到额外添加这部分功能代码复用的效果。**

> 他的前身其实是用ES5创建组件时可用的mixin方法，但是在react版本升级过程中，使用ES6语法创建组件时，认为mixin是反模式，影响了react架构组件的封装稳定性，增加了不可控的复杂度，逐渐被HOC所替代。

*此时，我们再次用通俗的语言描述一下高阶组件（higher-order component):*

当react组件被包裹（wrapped）时，高阶组件会返回一个增强版（enhanced）的react组件。
我们可以用过柯里化（curry）逐步确定高阶组件的增加功能特性，并把这些特性包裹给参数组件。它可以做什么？高阶组件让我们的功能性代码更具有复用性、逻辑性与抽象特性，它可以对render方法做劫持，可以控制props和state。

但是我们一旦掌握react的高阶组件，就能极大提高代码复用率，和逻辑代码的可管理度，是我们的代码更加优雅。

我们再用代码来表述一下高阶组件

// 简单一点的，传入组件，返回额定的封装：
`const newComponent = hiderOderFunction(OldComponent);`
// 柯里化程度高一些的,通过两次传参进一步确定和缩小高阶组件功能：
`const newComponent = hiderOderFunction(params)`(OldComponent);


二、React高阶组件的实现
如果我们学过设计模式，同时用了HOC，我们很容易将后者与装饰者模式联系起来。我们通过组合的方式到达很高灵活度的装饰搭配，我们可以将这种思维带到接下来的HOC实现。

实现的高阶组件的方法有两种：

- 属性代理。函数通过返回包裹原组件来添加额外功能。
- 反向继承。函数通过返回继承原组件来控制render。

> **属性代理**
> *属性代理是常见高阶组件的实现方式。*
> 
> ```js
> //我们来写一个最简单的
> const MyContainer = (WrappedComponent) =>class extends Components{
> 	render(){
> 		const newProps={
> 			text:'newText',
> 		}
> 		return <WrappedComponent {...this.props} {...newProps} />
> 	}
> }
> ```
> 就这么简单我们实现了对原组件props的添加，我们还可以在MyContainer内添加各种生命周期和自定义方法实现对render时return组件的各种控制。

<br/>

> **反向继承**
> *反向继承是通过class继承特性来实现高阶组件的一种方式，我们通过简单的代码*来理解一下：
> 
>   ```js
>   const MyContainer = (WrappedComponent) = > class extends WappedComponent {
> 	 render (){
> 	 	return super.render();
> 	 }
>   }
>   ```
> 这种方法和属性代理不太一样，它通过继承WrappedComponent来实现，方法可以通过super来顺序调用。在继承方法中，高阶组件可以使用传入组件的引用，这意味着它可以使用传入组件的state、props、生命周期和render方法。
> 
> 它有两个比较大的特点。
> 
> 渲染劫持
> 渲染劫持就是指高阶组件可以控制传入组件的渲染过程，并渲染各种各样的结果。在这个过程中我们可以对输出的结果进行读取、增加、修改、删除props，或读取或修改React元素树，或条件显示元素树，又或是用样式控制包裹元素树。
> 
> ```js
> const MyContainer = (WrappedComponent) = > class extends WrappedComponent{
> 	render(){
> 		const elementsTree = super.render();
> 		let newProps={};
> 		if(elementsTree && elementsTree.type === 'input'){
> 			newProps = {value:'may the force be with you'};
> 		}
> 		const props =  Object.assign({},elementsTree.props,newProps);
> 		const newElmentsTree = React.cloneElement(elementsTree, props, elmentsTree.props.children);
> 		
> 	}
> }
> ```
> 
> 在这个例子中，WrappedComponent的渲染结果中，顶层的input组件的value被改写成may the force be with you。因为，我们在高阶函数的反向继承实现中可以翻转元素树，改变元素树中的props。
> 
> 控制state
> 高阶组件可以读取、修改或删除WrappedComponent实例中的state，如果有需要，也可以增加state。但这样做，会使组件的内部状态混乱。大部分高阶组件都应该限制读取或增加state，尤其是后者，可以通过重新命名state，以防止混淆。

三、额外的注意点
组件命名
当包裹一个高阶组件时，我们失去了原始WrappedComponents的displayName，而组件名字是方便我们开发与调试的重要属性。

我们参考 react-redux库中的实现：

```js
class HOC extends ...{
 static displayName = `HOC(${getDisplayName(WrappedComponent)})`;
}
//getDisplayName方法可以这样实现：
function getDisplayName（WrappedComponent){
	return WrappedComponent.displayName || 
			WrappedComponent.name ||
			'Component';
}
```
或者可以使用recompose库，他已经帮我们实现了对应的方法。

组件参数
这就是一开始我提到过的一点，如何通过柯里化构建更加精确的高阶组件

```js
function HOCFactoryFactory(...params){
	//可以做一些改变params的事情
	return class HOCFactory(WrappedComponent){
		remder(){
			return <WrappedComponent {...this.props} />
		}
	}
}
```

当我们使用时，通过两次传参构建更为精准的高阶组件

`HOCFactoryFactory(params)(WrappedComponent)`

这也是利用了函数式编程的特性。

四、总结和建议。
吸收HOC最好的时机是，当你的组件代码的复用性出现问题，出现了大量没必要的冗余可复用功能性代码时候，你就可以反过来看看HOC，一段蹩脚的英文 
**`when need now，the best time。`**
高阶组件的出现，代表了react声明式编程的思想方向，我们通过组建组合开发，降低了组件的复杂度，也达到了代码更大的复用度。






# react 事件冒泡问题


两种事件:

1. 合成事件: 通过直接onClick属性绑定函数的事件
2. 原生事件: 通过js原生代码绑定的事件


阻止的时候:

1. 合成事件冒泡到Document原生事件 ` e.nativeEvent.stopImmediatePropagation();`

    > document.addEventListener('click', () => {})
2. 合成事件冒泡到合成事件 `e.stopPropagation();`
3. 合成事件冒泡到普通原生事件 在普通原生事件里 ,判断e.target, 然后在里面`return false`




# react 接入 redux

`npm i xxx  -s`  redux / react-redux / redux-logger / redux-thunk




app.js

```js
import { createStore } from 'redux'
import rootReducer from './reducers/index' // 等价于 ./reducer
import {Provider} from 'react-redux'

const store = createStore(rootReducer)

exxpoer default class App extends Component {
    render() {
        return <Provider store={store}> 
            <Ap>
        </Provider>
    }
}
```

Ap.js

```js
import connect from 'react-redux'
import { addtodo } from './action'

handleAddTodo = () => {
    const action = addTodo({text: xxx})
    cosnt dispatch = this.props.dispatch 
    dispatch(action)
}
function mapStateToProps (state, ownProps) {
// store 是邮局
// state 是仓库
// Provider + store 邮局挂牌成立
// action是信
// dispatch是揽件员
// connect 和邮局建立连接
// reducer是邮件投递出后邮局的分拣员
//这个函数从仓库里拿东西 通过connect 送给这个组件
}
export default connect (mapStateToProps) (Ap)
```


reducers/index.js

```js
export default (state, action) {
    switch (type) {
        case
        default
            return state
        ...
    }
}
```

actions/index.js

```js

import ActionTypes from '...';

export function addTodo(params) {
    return {
        type: ActionTypes.ADD_TODO
        text: params.text
    }
}

```

const/ActionTypes.js

```js
export const ADD_TODO = 'ADD_TODO'
```



# tips

1. connect会自动把dispatch作为props绑到目标组件上
2. 容器组件可以吧dispatch传给子视图组件
3. git checkout -b newBranchName commitID  可以在指定提交节点切出分支


4. 父子传action常用的两种方法
    
    1. 传递dispatch: 子组件强依赖redux, 不利于跨项目复用
    2. 推荐打包传递actions 可以通过combineActionCreate


5. entities最好先传进去, 然后在组件内部定位然后渲染出来
6. 点击事件不想bind传参的做法: 自己分装组件, 专门用来处理这件事情


    ```js
    <AttrSpan 
        record={record} 
        index={index} 
        onClick={this.handleReply}>
        {info === 1 ? '已回复' : '✉️回复'}
    </AttrSpan>
    ```
    
    ```js
    import React from 'react';
    export default class extends React.PureComponent{
      onClick = () => {
        const {onClick,className,children, ...attr } = this.props;
        onClick && onClick(attr);
      }
      render(){
        return(
          <span className={this.props.className} onClick={this.onClick}>{this.props.children}</span>
        )
      }
    }
    ```


## 嵌套数据的增删改
建议最多一层嵌套，以保持 state 的扁平化，深层嵌套会让 reducer 很难写和难以维护。

app.model({
  namespace: 'app',
  state: {
    todos: [],
    loading: false,
  },
  reducers: {
    add(state, { payload: todo }) {
      const todos = state.todos.concat(todo);
      return { ...state, todos };
    },
  },
});
下面是深层嵌套的例子，应尽量避免。

app.model({
  namespace: 'app',
  state: {
    a: {
      b: {
        todos: [],
        loading: false,
      },
    },
  },
  reducers: {
    add(state, { payload: todo }) {
      const todos = state.a.b.todos.concat(todo);
      const b = { ...state.a.b, todos };
      const a = { ...state.a, b };
      return { ...state, a };
    },
  },
});


## 性能优化

1. immutable➖   封装出几种新的数据类型, 他们都是不可改变的, 每次修改都会自动返回一个新的数组
2. reselect➖    把组件和需要用的某一块state进行绑定, 避免不相关的数据更新带来的组件渲染回路的开销

[注]: 简书微信登陆后有个人收藏了两篇文章, 讲的不错, 要用的时候温习一下
