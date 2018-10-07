# React

## 概念
- React 是一个用于构建UI视图层(V层)的框架


## JSX语法
一种方便的模板写法,在js中写html,注意:

- 标签的属性:
	- 属性名class=>className
	- 事件属性名,如onclick=>转驼峰onClick
	- style={{xx:xxx}}
- 变量和表达式一定写在{}里才会被解析
- 如果内容是一个数组,则会展开这个数组的所有成员

## 组件
### 组件的基本知识

- 用继承类`react.component`的方法去创建一个组件

	```javascript
	class Main extends React.component {
		constructor(props) {
			super(props);
			this.state = {
				a:1,
				b:2...
			}
		}
		...
	}
	```
- 里面会定义一个**render**方法,去返回想要展示的html结构,
	- render方法就是一个mvvm中的vm(ViewModel)层,
	- 解析时终究会把结构解析为`createElement(元素名,props对象,innerHTML)`


	
### 组件的使用(渲染)
1. 把组件渲染到指定的DOM元素节点里
`React.render(<componentNM>,document.body)`
2. 组件嵌套



### 组件的原理,组成
- props: 使用组件时,传递到组件的**属性**,可以在组件内通过`this.props`访问,这样可以通过props传递组件的配置
- state: 每个组件都可以拥有自己的state(**状态**).这样,重复使用同一个组件也可以有不同的表现
	- this.getInitialState定义初始状态,函数返回一个{stateNM: value}的state对象 
	- this.setState({状态对象})更新状态,这样只要状态更新就会调用render函数重新渲染(此时利用virtualDOM机制,只把发生改变的部分重新渲染)
	- this.state可以访问当前组件的state对象

- virtualDOM: 计算对比差异,对比后产生一堆指令(作用:把差异部分的真实DOM更新),在DOM上执行


### 组件的生命周期
*定义: 一系列可以在组件配置里定义的函数,在组件生命周期里自动调用*
`componentWillReceiveProps(newProps)`: 收到**新的**props会调用


`componentWillMount` (C/S)
**状态: (initRender()=>Mounting: 已插入真实的DOM)**
`componentDidMount`: (C) 通过this.getDOMNode()访问渲染后的DOM,  一般在这个函数中**发送AJAX请求**等操作	
`shouldComponentUpdate(newProps, newState)`返回布尔值,收到state或props会调用,可以用来确认不需要更新组件时使用
`componentWillUpdate(nextProps, nextState)`: 收到新的state或props还没有渲染时调用
**状态: (Updating: 正在被重新渲染)**
`componentDidUpdate(prevProps, prevState)`: 完成更新后立即调用
`componentWillUnmount`: 准备从DOM中移除时调用
**状态: (Unmounting: 渲染替换完毕,已移出真实DOM)**

> 做ajax请求时,思路: 
> 
> 1. 在ReactDOM.render(使用组件)时,通过props传入url
> 1. componentDidMount钩子函数上,
> 	1. 发送请求到 props里的url
> 	2. 把返回的数据setState到state里
> 2. componentWillUnmount钩子函数里abort请求
> 3. render函数里使用state

### 父子组件通信
父组件定义一个函数,然后通过props传给子组件作为事件处理函数,子组件改变,触发事件,更新了父组件的state,重新render,传入的props改变,同步到子组件


### ref属性
- 作用: 获取组件下的render的子元素原生DOM(官方说法:支撑实例)
- 一般用法: 

	```javascript
	class Main extends React.component {
		constructor(props) {
			super(props);	
		}
		..
		通过this.refs.xxx就可以获取到对应的原生DOM子节点
		...
		render() {
			return (
				<div ref='xxx'>
			)
		}
	}	
	```
	
	
10.31 由于要做小板凳的op后台管理系统, 所以复习了react的知识, 查漏补缺



```js
var reactEle = React.createElement('p',{className: 'paragraph'}, child1, child2 ...)
/* 
**  第一个参数：必填，元素类型
        第二个参数：可填，元素属性
        第三个参数：可填，元素子节点(也可以是子元素集合数组)
*/

ReactDOM.render(reactEle, document.getElementById('root'))
```

- 服务端渲染 

