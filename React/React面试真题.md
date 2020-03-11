##### 组件之间如何通讯
- 父子组件props
  - 父组件通过props往子组件传递数据，或者父组件通过props往子组件传递一个函数让子组件调用
- 自定义事件
- Redux和Context

##### JSX本质是什么
- createElement
- 执行返回vnode

##### Context是什么，如何应用
- 父组件，向其下所有子孙组件传递信息
- 如一些简单的公共信息：主题色，语言
- 复杂的公共信息，请用redux

##### shouldComponentUpdate用途
- 性能优化
- 配合”不可变值“一起使用，否则会出错

##### setState场景题
```javascript
componentDidMount() {
  // count初始值为0
  this.setState({count: this.state.count + 1})
  console.log('1', this.state.count) //0
  this.setState({count: this.state.count + 1)
  console.log('2', this.state.count) //0
  setTimeout(() => {
    this.setState({count: this.state.count + 1})
    console.log('3', this.state.count) //2
  })
  setTimeout(() => {
    this.setState({count: this.state.count + 1})
    console.log('4', this.state.count) //3
  })
}
```
##### 什么是纯函数
- 返回一个新值，没有副作用（不会”偷偷“修改其他值）
- 重点：不可变值
- 如arr1 = arr.slice

##### React组件生命周期
- 单组件生命周期
- 父子组件生命周期
- 注意SCU

##### React发起ajax应该在哪个生命周期
- 同Vuw
- componentDidMount

##### 渲染列表，为何使用key
- 同Vue。必须用key, 且不能是index和random
- diff算法中通过tag和key来判断，是否是sameNode
- 减少渲染次数，提升渲染性能

##### 函数组件和class组件区别
- 纯函数，输入props,输出JSX
- 没有实例，没有生命周期，没有state
- 不能扩展其他方法

##### 什么受控组件
- 表单的值，受state控制
- 需要自行监听onChange,更新state
- 对比非受控组件

##### 何时使用异步组件
- 同Vue
- 加载大组件
- 路由懒加载

##### 多个组件有公共逻辑，如何抽离
- 高阶组件HOC
- Render Props
- mixin已被React废弃

##### redux如何进行异步请求
- 使用异步action
- 如redux-thunk

##### react-router如何配置懒加载
```javascript
import { BroserRouter as Router, Route, Switch } from 'react-router-dom'
import React, { Suspense, lazy } from 'react';

const Home = lazy(() => import('./routes/Home'))
const About = lazy(() => import('./routes/About'))

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
)
```

##### PureComponent有何区别
- 实现了浅比较的shouldComponentUpdate
- 优化性能
- 但要结合不可变值使用

##### React事件和DOM事件的区别
- 所有事件挂载到document上
- event不是原生的，是SyntheticEvent合成事件对象
- dispatchEvent

##### React性能优化
- 渲染列表时加Key
- 自定义事件、DOM事件及时销毁
- 合理使用异步组件
- 减少函数bind this的次数
- 合理使用SCU PureComponent和memo
- 合理使用Immutable.js
- webpack层面的优化
- 前端通用的性能优化，如图片懒加载
- 使用SSR

##### React和Vue的区别
- 共同点
  - 都支持组件化
  - 都是数据驱动视图
  - 都是使用vdom操作DOM
- 区别
  - React使用JSX拥抱js，Vue使用模板拥抱html
  - React函数式编程，Vue声明式编程
  - React更多需要自力更生，Vue把想要的都给你
