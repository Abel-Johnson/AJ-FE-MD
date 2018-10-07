# Redux学习
*作用: 复杂应用的state(状态)管理*
### 常见的state包括: 
- 服务端返回的数据
- 本地对响应的缓存
- 本地创建的数据(例如:表单数据)
- UI状态信息(例如: 路由、选中的 tab、是否显示下拉列表、页码控制等等)

*随着页面逻辑越来越复杂,意识到state需要统一管理*

### store
- Redux 的核心是一个**store**对象,其中存储了一个state对象。
- reducer(previousState, action) => newState 
- store可以通过`createStore(reducers,[默认的state])` 方法创建。(在这就绑定了reducer,以后只要dispatch(action),就可以自动更新状态)
- 两个核心方法： .getState() 和 .dispatch()。
	- .getState()返回当前状态state
	- .dispatch(action)将action分发给所有订阅了更新的 reducer。
		- 约定action的格式: {type: '(大写)表示将要执行的动作的字符串', xx:xx}
- 还有两个方法
	- .subscribe(listener) 注册监听器;
   - subscribe(listener) 返回的函数用来注销监听器。




### 过程
组件产生action-->
store.dispatch(action)把action分发给所有的reducer-->
reducer(previousState, action)返回新的state


