# React-Router

```html
  <Router>
    <Route path="/" component={App}>
      <IndexRoute component={Dashboard} />
      <Route path="about" component={About} />
      <Route path="inbox" component={Inbox}>
        <Route path="/messages/:id" component={Message} />

        {/* 跳转 /inbox/messages/:id 到 /messages/:id */}
        <Redirect from="messages/:id" to="/messages/:id" />
      </Route>
    </Route>
  </Router>
```
```js
const routeConfig = [
  { path: '/',
    component: App,
    indexRoute: { component: Dashboard },
    childRoutes: [
      { path: 'about', component: About },
      { path: 'inbox',
        component: Inbox,
        childRoutes: [
          { path: '/messages/:id', component: Message },
          { path: 'messages/:id',
            onEnter: function (nextState, replaceState) {
              replaceState(null, '/messages/' + nextState.params.id)
            }
          }
        ]
      }
    ]
  }
]
return <Router routes={routeConfig} />
```


- 可以指定默认子组件`<IndexRoute component={Dashboard} />`
- path属性可以写绝对路径, 这样就不用严格的一层层写路由组件的对应, 比如上面的Message组件会匹配/inbox/message/123, 当路由不想要这么长的时候,但是依然想在UI层面保持这种嵌套关系, 就要使用绝对路由: `<Route path="/messages/:id" component={Message} />`   
    **但是**: **绝对路径可能在动态路由中无法使用。**

- 重定向: `<Redirect from="messages/:id" to="/messages/:id" />`

- 一些钩子(Hook)函数: 
    - onEnter / onLeave, 可以用来做权限验证或者在路由跳转前将一些数据持久化保存起来。
    - 在路由跳转过程中，onLeave hook 会在所有将离开的路由中触发，从最下层的子路由开始直到最外层父路由结束。然后onEnter hook会从最外层的父路由开始直到最下层子路由结束。
    - 从messages/123跳转到/about, 就要执行:
        - message/:id的onLeave
        - /inbox的onLeave
        - /about的onEnter
- 路由匹配时, 会有优先级, 匹配前一个就不会匹配后面的了


- IndexLink

- 路由跳转的两种方式:

    - this.context.router
    - `import { browserHistory } from 'react-router'`
    `browserHistory.push('/some/path')`




## 路由拆分的原则:

**要根据UI的复用部分来拆, 不要根据业务来拆**
