##### 浏览器的本地存储
##### Cookie
Cookie 的作用很好理解，就是用来做状态存储的
1. 容量缺陷。Cookie 的体积上限只有4KB，只能用来存储少量的信息
2. 性能缺陷。Cookie 紧跟域名，不管域名下面的某一个地址需不需要这个 Cookie ，请求都会携带上完整的 Cookie，这样随着请求数的增多，其实会造成巨大的性能浪费的，因为请求携带了很多不必要的内容。
3. 安全缺陷。由于 Cookie 以纯文本的形式在浏览器和服务器中传递，很容易被非法用户截获，然后进行一系列的篡改，在 Cookie 的有效期内重新发送给服务器，这是相当危险的。另外，在HttpOnly为 false 的情况下，Cookie 信息能直接通过 JS 脚本来读取。
##### localStorage
localStorage有一点跟Cookie一样，就是针对一个域名，即在同一个域名下，会存储相同的一段localStorage。
1. 容量。localStorage 的容量上限为5M，相比于Cookie的 4K 大大增加。当然这个 5M 是针对一个域名的，因此对于一个域名是持久存储的。
2. 只存在客户端，默认不参与与服务端的通信。这样就很好地避免了 Cookie 带来的性能问题和安全问题。
3. 接口封装。通过localStorage暴露在全局，并通过它的 setItem 和 getItem等方法进行操作，非常方便。
##### 应用场景
利用localStorage的较大容量和持久特性，可以利用localStorage存储一些内容稳定的资源，比如官网的logo，存储Base64格式的图片资源，因此利用localStorage
##### sessionStorage
sessionStorage以下方面和localStorage一致:
* 容量。容量上限也为 5M。
* 只存在客户端，默认不参与与服务端的通信。
* 接口封装。除了sessionStorage名字有所变化，存储方式、操作方式均和localStorage一样。

但sessionStorage和localStorage有一个本质的区别，那就是前者只是会话级别的存储，并不是持久化存储。会话结束，也就是页面关闭，这部分sessionStorage就不复存在了。

应用场景
1. 可以用它对表单信息进行维护，将表单信息存储在里面，可以保证页面即使刷新也不会让之前的表单信息丢失。
2. 可以用它存储本次浏览记录。如果关闭页面后不需要这些记录，用sessionStorage就再合适不过了。事实上微博就采取了这样的存储方式。
