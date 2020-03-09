##### 单项数据流概述
- dispatch(action)
- reducer -> newState
- subscribe 触发通知

##### react-redux
- <Provider>
- connect
- mapStateToProps mapDispatchToProps

##### 异步action
- redux-thunk
- redux-promise
- redux-saga
```
//同步action
export const addTodo = text => {
  //返回 action 对象
  return {
    type: 'ADD_TODO',
    id: nextTodoId++,
    text
  }
}
```
```
//异步action
export const addTodoAsync = text => {
  //返回函数,其中有dispatch
  return (dispatch) => {
    //ajax 异步获取数据
    fetch(url).then(res => {
      //执行异步 action
      dispatch(addTodo(res.text))
    })
  }
}
```
##### redux中间件
[![](https://image.prntscr.com/image/FO0T4rZGTIK_YYTxmOQaCg.png)](https://image.prntscr.com/image/FO0T4rZGTIK_YYTxmOQaCg.png "markdown")

```
import { applyMiddleware, createStore } from 'redux';
import createLogger from 'redux-logger';
import thunk from 'redux-thunk';
const logger = createLogger();

const store = createStore(
  reducer,
  applyMiddleware(thunk, logger) //会被顺序执行
)
```
- logger实现
```
//自己修改dispatch, 增加logger
let next = store.dispatch;
store.dispatch = function dispatchAndLog(action) {
  console.log('dispatching', action);
  next(action);
  console.log('next state', store.getState());
}
```
- redux数据流程图
[![](https://image.prntscr.com/image/QAe5HTD7T26YNyOHzKRFyQ.png)](https://image.prntscr.com/image/QAe5HTD7T26YNyOHzKRFyQ.png "markdown")
