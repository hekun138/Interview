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
  https://image.prntscr.com/image/-sxjckNhQE2lq-gzi2iO9w.png
- setState batchUpdate
- 组件渲染过程
- 前端路由
