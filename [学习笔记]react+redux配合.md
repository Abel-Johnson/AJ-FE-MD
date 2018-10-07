# react+redux配合

关注了一个知乎提问,有助于深入理解
https://www.zhihu.com/question/41312576

1. 最外层中,把所有内容包裹在Provider组件中,并将之前的创建的store作为props传递给Provider组件
2. 任何子组件,想要使用state的数据,必须被connect过(该函数相当于把自己的组件进行了包装)

	- connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
		- `mapStateToProps(state, ownProps):stateProps`这个函数允许我们将 store 中的数据作为 props 绑定到组件上。会返回一个props对象,名值对就是props,修改state或组件自身的ownProps时,都会自动触发该函数,重新计算后更新组件的属性
		- `mapDispatchToProps(dispatch, ownProps):dispatchProps`可以将acton作为props绑定到子组件上,返回的也是一个props对象,只不过键名对应的是一个函数,函数内需要调用dispatch方法,让action生效