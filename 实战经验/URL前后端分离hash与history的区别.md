## 【转载】Vue路由 hash与history 的区别

> 两种路由模式原因 对于 [Vue](https://links.jianshu.com/go?to=https%3A%2F%2Fcn.vuejs.org%2F) 这类渐进式前端开发框架，
>  为了构建 SPA（单页面应用），
>  需要引入前端路由系统，
>  这也就是 [Vue-Router](https://links.jianshu.com/go?to=https%3A%2F%2Frouter.vuejs.org%2Fzh%2F) 存在的意义。
>  前端路由的核心，就在于 —— 改变视图的同时不会向后端发出请求。
>  因此有两种路由

1. `hash`: 即地址栏 URL 中的`#`符号（此 `hash` 不是密码学里的散列运算）。
     比如这个 URL：`http://www.abc.com/#/hello`，
     `hash` 的值为 `#/hello`。它的特点在于：==hash 虽然出现在 URL 中，
     但不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。==
2. `history`: 利用了 `HTML5 History Interface`
     中新增的`pushState()`和`replaceState()`方法。（需要特定浏览器支持）
     这两个方法应用于浏览器的历史记录栈，在当前已有的`back、forward、go`的基础之上，
     它们提供了对历史记录进行修改的功能。只是当它们执行修改时，==虽然改变了当前的 URL，
     但浏览器不会立即向后端发送请求。==

​		`hash` 模式和 `history` 模式都属于浏览器自身的特性，Vue-Router只是利用了这两个特性
 	 （通过调用浏览器提供的接口）来实现前端路由。

### 使用场景

一般场景下，hash 和 history 都可以，除非你更在意颜值，`#`符号夹杂在 URL 里看起来确实有些不太美丽。

> 如果不想要很丑的 hash，我们可以用路由的 history 模式，这种模式充分利用
>  `history.pushState API` 来完成
>  URL 跳转而无须重新加载页面。

调用`history.pushState()`相比于直接修改`hash`，存在以下优势：

- pushState()设置的新 URL 可以是与当前 URL 同源的任意 URL；而`hash`只可修改`#`
     后面的部分，因此只能设置与当前 URL 同文档的 URL；

- pushState()设置的新 URL 可以与当前 URL 一模一样，这样也会把记录添加到栈中；
     而`hash`设置的新值必须与原来不一样才会触发动作将记录添加到栈中；

- pushState()通过`stateObject`参数可以添加任意类型的数据到记录中；
     而`hash`只可添加短字符串；

- pushState()可额外设置`title`属性供后续使用。

    

    当然啦，`history`也不是样样都好。

    SPA 虽然在浏览器里游刃有余，但真要通过 URL 向后端发起 HTTP 请求时，
     两者的差异就来了。尤其在用户手动输入 URL 后回车，或者刷新（重启）浏览器的时候。

1. `hash`模式下，仅`hash`符号之前的内容会被包含在请求中，
     如`http://www.abc.com`，因此对于后端来说，即使没有做到对路由的全覆盖，
     也不会返回 404 错误。
2. `history`模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致，
     如`http://www.abc.com/book/id`如果后端缺少对`/book/id`的路由处理，将返回 404 错误。
     Vue-Router 官网里如此描述：“不过这种模式要玩好，还需要后台配置支持所以呢，
     你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，
     则应该返回同一个 `index.html`页面，这个页面就是你 app 依赖的页面。”

### history缺点

- 通过`history api`，我们丢掉了丑陋的#，但是它也有个毛病：不怕前进，不怕后退，就怕刷新，f5，
     （如果后端没有准备的话）,因为刷新是实实在在地去请求服务器的,不玩虚的。

- 在hash模式下，前端路由修改的是#中的信息，而浏览器请求时是不带它玩的，所以没有问题.但是在`history`下，

    你可以自由的修改path，当刷新时，如果服务器中没有相应的响应或者资源，会分分钟刷出一个404来。

### 小结

 结合自身例子，对于一般的`Vue + Vue-Router + Webpack + XXX`
 形式的 Web 开发场景，用`history`模式即可，
 只需在后端（Apache 或 Nginx）进行简单的路由配置，
 同时搭配前端路由的 404 页面支持。


原文链接：https://www.jianshu.com/p/ab085ae33ee5