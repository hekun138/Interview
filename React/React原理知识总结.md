##### React原理
- 函数式编程
  - 一种编程范式，概念比较多
  - 纯函数
  - 不可变值
- vdom和diff
  - h函数
  - vnode数据结构
  ```javascript
   {
      tag: 'div',
      props: {
        className: 'container',
        id: 'div1'
      },
      children: [
        {
          tag: 'p',
          children: 'vdom'
        },
        {
          tag: 'ul',
          props: { style: 'font-size: 20px'},
          children: [
            {
              tag: 'li',
              children: 'a'
            } 
            //....
          ]
        }
      ]
   }
  ```
  - patch函数
  - diff
    - 只比较同一层级，不跨级比较
    - tag不相同，则直接删掉重建，不再深度比较
    - tag和key,两者都相同，则认为是相同节点，不再深度比较
- JSX本质
  - JSX等同于Vue模板
  - Vue模板不是html
  - JSX也不是JS
  ```javascript
  //JSX 基本用法
  const imgElem = <div id="div1">
    <p>some text</p>
    <img src={imgUrl}/>
  </div>
  
  //babel转换后
  React.createElement("div", {
    id: "div1"
  }, React.createElement("p", null, "some text"), 
    React.createElement("img", {
      src: imgUrl
    })
  )
  ```
  ```javascript
  //JSX list
  const listElem = <ul>
    {
      this.state.list.map((item, index) => {
        return <li key={item.id}>
          index{index}; title {item.title}
        </li>
      })
    }
  </ul>
  
  //JSX list
  var listElem = React.createElement("ul", null, (void0).state.list.map(function (item, index) {
    return React.createElement("li", {
      key: item.id
    }, "index ", index, "; title", item.title)
  }))
  ```
  - React.createElement即h函数，返回vnode
  - 第一个参数，可能是组件，也可能是html tag
  - 组件名，首字母必须要大写（React规定） 
- 合成事件
  - 所有事件挂载到document上
  - event不是原生的，是SyntheticEvent合成事件对象
  - 和Vue事件不同，和DOM事件也不同
  [![](https://image.prntscr.com/image/-sxjckNhQE2lq-gzi2iO9w.png)](https://image.prntscr.com/image/-sxjckNhQE2lq-gzi2iO9w.png "markdown")
  - 为何要合成事件机制？
    - 更好的兼容性和跨平台（自己实现一套逻辑更能保证兼容性和跨平台React Native）
    - 挂载到document，减少内存消耗，避免频繁解绑
    - 方便事件的统一管理（如事务机制）
- setState batchUpdate
  - 有时异步（普通使用），有时同步（setTimeout、DOM事件）
  - 有时合并（对象形式），有时不合并（函数形式）
  - 后者比较好理解（像Object.assign）,主要讲解前者
  
  - 核心要点 
    - setState主流程
      1. setState无所谓异步还是同步
      2. 看是否能命中batchUpdate机制
      3. 判断isBatchingUpdates
      [![](https://image.prntscr.com/image/_PDWekLzRwyUsLvc9RliKw.png)](https://image.prntscr.com/image/_PDWekLzRwyUsLvc9RliKw.png "markdown")  
    - batchUpdate机制
      [![](https://image.prntscr.com/image/Lsaq4J0tT1yBNqbo6TCyEw.png)](https://image.prntscr.com/image/Lsaq4J0tT1yBNqbo6TCyEw.png "markdown")
      [![](https://image.prntscr.com/image/9Cw861haTt2iaMt7fIaVcA.png)](https://image.prntscr.com/image/9Cw861haTt2iaMt7fIaVcA.png "markdown")
      - 哪些能命中batchUpdate机制
        1. 生命周期（和它调用的函数）
        2. React中注册的事件（和它调用的函数）
        3. React可以“管理”的入口
      - 哪些不能命中batchUpdate机制
        1. setTimeout setInterval等（和它调用的函数）
        2. 自定义的DOM事件（和它调用的函数）
        3. React“管不到”的入口
    - transaction（事务）机制 
- 组件渲染过程
- 前端路由
