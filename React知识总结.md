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

