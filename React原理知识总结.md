##### React原理
- 函数式编程
  - 一种编程范式，概念比较多
  - 纯函数
  - 不可变值
- vdom和diff
  - h函数
  - vnode数据结构
  ```json
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
- JSX本质
- 合成事件
- setState batchUpdate
- 组件渲染过程
- 前端路由
