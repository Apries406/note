# Cookies

- `HTTP`是无状态协议。简单地说，当你浏览了一个页面，然后转到同一个网站的另一个页面，服务器无法认识到这是同一个浏览器在访问同一个网站。每一次的访问，都是没有任何关系的。如果我们要实现多个页面之间共享数据的话，我们就可以使用`Cookie`或者`Session`实现

- `Cookie`是存储于访问者的计算机中的变量。可以让我们用同一个浏览器访问同一个域名的时候共享数据
# Session

`session`是另一种记录客户状态的机制，不同的是`cookie`保存在客户端浏览器中，而`session`保存在服务器上
## 工作流程

当浏览访问服务器并发送第一次请求时，服务器端会创建以后跟`session`对象，生成一个类似于`key,value`的键值对，然后将`key(cookie)`返回到浏览器（客户）端，浏览器下次再访问时，携带`key(cookie)`找到对应的`session(value)`。客户的信息都保存在`session`中。