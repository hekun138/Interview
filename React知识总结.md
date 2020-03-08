##### React组件如何通讯
 

##### React原生事件和合成事件
- target: event.target----指向当前元素，即当前元素触发
- current target: event.currentTarget //指向当前元素，假象

1. event合成事件 
其实是React封装的。event.__proto__.constructor是SyntheticEvent
这里的event不是原生的Event,原生的是MouseEvent

2. event原生事件
event.nativeEvent.__proto__.constructor是MouseEvent
event.nativeEvent.target 指向当前元素，即当前元素触发
event.nativeEvent.currentTarget 指向document

- 总结
1. event是SyntheticEvent,模拟出来DOM事件所有能力
2. event.nativeEvent是原生事件对象
3. 所有的事件，都被挂载到document上
4. 和DOM事件不一样，和Vue事件也不一样

##### setState
* 不可变值
1. 不要直接修改state,使用不可变数据
 `this.state.count++` 错误
 ```
 this.setState({
    count: this.state.count + 1
  })
  ```
* 可能是异步更新
1. 
 ```
 this.setState({
   count: this.state.count + 1
 })
 console.log('count', this.state.count) //异步的，拿不到最新值
 ```
 2. 
 ```
 this.setState({
   count: this.state.count + 1
 },() => {
   //联想 vue $nextTick
   console.log('count by callback', this.state.count) //回调函数可以拿到值
 })
 ```
 3. 
 ```
 //setTimeout 中 setState是同步的
 setTimeout(() => {
   this.setState({
     count: this.state.count + 1
   })
   console.log('count in setTimeout', this.state.count)
 }, 0)
 ```
 4.
 ```
 //自己定义的DOM事件，setState是同步的
 document.body.addEventListener('click', () => {
   this.setState({
     count: this.state.count + 1
   })
   console.log('count in body event', this.state.count)
 })
 ```
* 可能会被合并
1.
```
// setState异步更新的话，更新前会被合并
// 传入对象，会被合并，执行结果只一次 + 1（类似Object.assign）
this.setState({
  count: this.state.count + 1
})
this.setState({
  count: this.state.count + 1
})
this.setState({
  count: this.state.count + 1
})
``` 
2. 
```
// 传入函数，不会被合并。执行结果是 + 3
this.setState((prevState,props) => {
  return {
    count: prevState.count + 1
  }
})
this.setState((prevState, props) => {
  return {
    count:prevState.count + 1
  }
})
this.setState((prevState, props) => {
  return {
    count: prevState.count + 1
  }
})
```
##### React组件生命周期
[![](https://image.prntscr.com/image/2UaUnNvWR6ut4VFyXdXkdQ.png)](https://image.prntscr.com/image/2UaUnNvWR6ut4VFyXdXkdQ.png "markdown")
[![](https://image.prntscr.com/image/weCxqslTQtWX6tNugFGcFQ.png)](https://image.prntscr.com/image/weCxqslTQtWX6tNugFGcFQ.png "markdown")

http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

##### React高级特性
* 函数组件
1. 纯函数，输入props,输出JSX
2. 没有实例，没有生命周期，没有state
3. 不能扩展其他方法
* 非受控组件
- ref
- defaultValue defaultChecked
- 手动操作DOM元素
- 使用场景
  - 必须手动操作DOM元素，setState实现不了
  - 文件上传<input type=file>
  - 某些富文本编辑器，需要传入DOM元素
###### 受控组件vs非受控组件
- 优先使用受控组件，符合React设计原则
- 必须操作DOM时，再使用非受控组件
* Portals
- 组件默认会按照既定层次嵌套渲染
- 如何让组件渲染到父组件以外
- 使用场景
  - overflow: hidden
  - 父组件z-index值太小
  - fixed需要放在body第一层级
* context
- 公共信息（语言、主题）如何传递给每个组件
- 用props太繁琐
- 用redux小题大做
* 异步组件
- import()
- React.lazy
- React.Suspense
```
const ContextDemo = React.lazy(() => import('./ContextDemo'));

render(){
  return (
    <div>
      <p>引入一个动态组件</p>
      <hr />
      <React.Suspense fallback={<div>Loading...</div>}>
        <ContextDemo />
      </React.Suspense>
    </div>
  )
}
```
* 性能优化
  - 前言
  - React默认：父组件有更新，子组件则无条件也更新
  - 性能优化对于React更加重要
  - SCU 一定要每次都用吗？需要的时候才优化
    - SCU默认返回true,即React默认重新渲染所有子组件
    - 必须配合"不可变值"一起使用
    - 可先不用SCU，有性能问题时再考虑使用
  - shouldComponentUpdate (简称SCU)
  ```
  shouldComponentUpdate(nextProps, nextState) {
    if(nextState.count !== this.state.count) {
      return true //可以渲染
    }
    return false //不重复渲染
  }
  ```
 - PureComponent和React.memo
   - PureComponent,SCU中实现了浅比较（class组件）
   - memo，函数组件中的PureComponent（函数组件）
   ```
   function MyComponent(props) {
     /*使用props渲染*/
   }
   function areEqual(prevProps, nextProps) {
     /*
       如果把 nextProps 传入 render 方法的返回结果与将prevProps传入
       render方法的返回结果一致则返回 true,否则返回false
     */
   }
   export default React.memo(MyComponent, areEqual)
   ```
   - 浅比较已使用大部分情况（尽量不要做深度比较）
 - 不可变值immutable.js
   - 彻底拥抱”不可变值“
   - 基于共享数据（不是深拷贝），速度好
* 高阶组件HOC
  ```
  //高阶组件不是一种功能，而是一种模式,类似工厂模式
  const HOCFactory = (Component) => {
    class HOC extends React.Component {
      //在此定义多个组件的公共逻辑
      render() {
        return <Component {...this.props}/> //返回拼装的结果
      }
    }
    return HOC
  }
  const EnhancedComponent1 = HOCFactory(WrappedComponent1)
  const EnhancedComponent2 = HOCFactory(WrappedComponent2)
  ```
* Render Props
