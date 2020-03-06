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
 ```
 
* 可能会被合并

