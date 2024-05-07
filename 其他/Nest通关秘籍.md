# 开篇词
一直做前端开发，都会有成为全栈工程师的想法，而 Nest 就是一个很好的途径，它是 Node 最流行的企业级开发框架，提供了 IOC、AOP、微服务等架构特性。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f14b806512ff4bc2973824653e7054cd~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

我们会学习 Nest 全部功能，并且会顺带把 mysql、mongodb、redis、rabbitmq、nacos 等后端中间件学一遍，也会学习 pm2、docker、docker compose 等部署方案，并做几个综合的全栈项目：

**会议室预订系统** ：这是一个单体应用，主要是练习使用 mysql + typeorm + redis + docker + nest 开发后端服务

**电商系统** ：这是微服务架构的项目，练习使用 RabbitMQ + MySQL + Redis + Docker Compose + etcd + Nest 进行开发。

**大众点评系统** ：练习 redis 的各种功能的经典项目，前端是移动端页面，练习使用 rabbitmq + mysql + redis + docker compose + nest 开发

**聊天室项目** ：这是 websocket 项目，练习使用 websocket + mongodb + redis + docker compose + nginx + nest 开发 ws 项目

**教育平台** ：这是 graphql 项目，练习使用 graphql + mongodb + redis + kafaka + docker compose + nginx + nest 开发 graphql 项目

**博客项目** ：这个是 ElasticSearch 项目，练习使用 elasticsearch + mysql + redis + docker compose + nginx + nest 开发 es 项目

课程前面部分是 Nest 基础，会讲解 Nest 各种功能的使用，包括 IOC、AOP、全局模块、动态模块、自定义 provider、middleware、pipe、interceptor、guard 等功能，还有 Nest CLI 的使用，Nest 项目的调试。

接下来会讲 docker 和 mysql、redis 等中间件的使用以及 typeorm 这个 orm 库的使用，还有 jwt、session 登录和 RBAC 权限控制，还会学习 pm2 部署，然后做第一个项目实战：会议室预订系统

然后开始微服务部分，会讲 Nest 如何开发微服务，如何创建 monorepo 项目，会讲微服务相关的中间件，比如用 rabbitmq 做削峰填谷，用 etcd、nacos 做配置中心和注册中心，还会学习 passport 做身份认证和 Docker Compose 部署多个项目以及 nginx，然后开发第二个实战项目：电商系统

接下来会深度练习 redis 的各种功能，然后做 redis 的经典项目：大众点评。这是一个基于地理位置的移动端项目。

之后会学习 WebSocket，我们会自己实现 WebSocket 协议来深入理解它，之后学习 Nest 里如何启动 WebSocket 服务，学习 mongodb，之后会做第四个实战项目：聊天室项目

然后开始学习 graphql，学习 graphql 的基础以及如何在 Nest 里使用，学习第二个消息中间件 kafka，然后用它开发第五个实战项目：教育平台

之后学习 ElasticSearch，学习如何用它做全文检索、它和 mysql 的关系，然后用它来做一个带全文搜索功能的项目：博客项目

最后回到 Nest 本身，会学习 IOC、AOP 的实现原理等

还有不定期的加餐，比如 oauth2.0 做授权，用它实现三方登录等

相信等你学完这本小册后，就已经是一个真正意义上的全栈工程师了。
# 给你5个学Nest的理由，你会心动吗
你可能经常听到 Nest，会觉得它和我的工作也没啥关系呀，为什么要学习 Nest 呢？

这里给出 5 个学习 Nest 的理由：

## 最流行的 Node 企业级框架

开发 node 应用有 3 个层次：

- 直接用 http、https 包的 createServer api
- 使用 express、koa 这种处理请求响应的库
- 使用 nest、egg、midway 这类企业级框架

直接用 createServer api 创建服务适合特别简单的场景，比如工具提供的开发服务。

express、koa 这种处理请求响应的库并不能约束代码的写法，代码可以写的很随意，所以不适合开发大型项目。

大型项目会用企业级开发框架，也就是规定了代码的写法，对很多功能都有开箱即用的解决方案的框架。

比如 Nest 代码一般都是分了很多模块：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/549bc636a9a84b56bfde197d088f7ded~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

每个模块下都是 controller、service、guard、filter、interceptor、dto 等这些代码：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1a6e576d76aa4a0583ad40a183b6229c~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

什么代码放在哪里都是有规范的。

上面这个目录是一个 8.7k 的已经盈利的项目的服务端代码，感兴趣可以看一下：

[github.com/apitable/ap…](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fapitable%2Fapitable%2Ftree%2F35b05eb421d31a4e4b68ca0bacb1fb484186f4ea%2Fpackages%2Froom-server "https://github.com/apitable/apitable/tree/35b05eb421d31a4e4b68ca0bacb1fb484186f4ea/packages/room-server")

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c03ce098c634b8781fc03d5c68c6949~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

那 egg 和 midway 呢？

egg 的 ts 支持不行，在当下 ts 这么主流的情况下，已经不合适了。更何况它是阿里的项目，而阿里 egg 团队也被打包裁了。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/45ed098f827f4b348d434305d04e9f7d~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

midway 呢？

star 数差太多了，和 nest 不在一个量级。

而且你能保证它不是下一个 egg 么？

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d9f81b4885744c6f98ba9bc03e851583~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/38212a2f2e0445b090868b6794c29522~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

Nest 是在全世界都很火，在国内也越来越流行，找不到啥对手。

如果你想学习 Node 框架，那 Nest 基本是唯一的选择了。

## 学习各种后端中间件

后端有很多中间件，比如 mysql、redis、rabbitmq、nacos、elasticsearch 等等，学习 Nest 的过程会用到这些中间件。

比如类似这种的后端架构：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/38a8aa18ae1a40e1ab83767b2558d84f~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

所以我们学的并不只是 Nest，而是整个后端生态。

这些东西就算换了 go 或者 java，也是一样要学的。

## 可以找国外的全栈工作，或者接远程外包

国外远程工作这块，电鸭社区是最有发言权的。

你逛一逛就会发现，很多创业公司都会用 Nest 做服务端：

比如这个：

[eleduck.com/posts/oQfOD…](https://link.juejin.cn/?target=https%3A%2F%2Feleduck.com%2Fposts%2FoQfOD7 "https://eleduck.com/posts/oQfOD7")

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/003b04cad510485e8c9728e0d65b89c2~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/88e83771b51d49da901e9ba4856f846a~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

这个：

[eleduck.com/tposts/Baf0…](https://link.juejin.cn/?target=https%3A%2F%2Feleduck.com%2Ftposts%2FBaf0Dy "https://eleduck.com/tposts/Baf0Dy")

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/776effcee7ba4ccab9ad4d9fb9f019b0~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

或者这个：

[eleduck.com/posts/njfbz…](https://link.juejin.cn/?target=https%3A%2F%2Feleduck.com%2Fposts%2FnjfbzD "https://eleduck.com/posts/njfbzD")

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8af15eea44a345138f436fe5c9302be9~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

还有这个：

[eleduck.com/posts/XNfBX…](https://link.juejin.cn/?target=https%3A%2F%2Feleduck.com%2Fposts%2FXNfBXN "https://eleduck.com/posts/XNfBXN")

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9fbdb09abe634dbfabf069040efbca6b~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

如果你会 React + Nest 技术栈，找个远程全栈工作不也很香么。

## 可以独立做自己的产品

如果你想做一个自己的产品，不管是网站、app、小程序还是游戏等应用，都得配上后端吧，如果你熟悉 js/ts，那 Nest 是最适合的选择了。

比如刚才我们看到这个国外的这个盈利的产品，它就是用 Nest 做的后端：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5dad2ada4c884466b5604f4d5cdc8560~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

或者你是大学生，想做一个写在简历里的前端项目，那只写前端就可以了么？

肯定不行呀，你得前后端都搞定，在简历上写一个完整的项目，这时候 Nest 就很合适了。因为如果你是想做前端工作，那用 java 并不咋加分。

## 学习优秀的架构设计

Nest 的架构很优雅，因为它用了不少设计模式。

比如 Nest 并不和 Express 耦合，你可以轻松切换到 Fastify。

就是因为它用了适配器的设计模式：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/20b41feed8d54e8bb264e508cd55c9c3~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

Nest 本身只依赖 HttpServer 接口，并不和具体的库耦合。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/237d45ff113a4fe1b7386d3dc7847355~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

你想换别的 http 处理的库，只要写一个新的适配器就好了。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/baeb2549d43c4ab79b65d8f3d717abd1~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

再比如 Nest 内构建复杂对象很多地方都用到了 builder 的设计模式：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3f6ccae877dc4078b115317f3c8d8c13~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/738ce1e89cdc438cbe6092b3d3aabe40~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

类似这样的设计有很多。

当你把 Nest用熟之后，潜移默化中，你就知道了什么地方用什么模式是最好的，应该怎么设计。无形中就提升了架构设计能力。

## 总结

不管是你想学 Node 框架，学习各种后端中间件，找国外的远程工作或远程外包，独立开发自己的产品，还是想学习优秀的设计，提升架构能力。Nest 都是一个非常好的选择。

心动了么？赶紧上车。

# 快速掌握Nest CLI
项目开发离不开工程化的部分，比如创建项目、编译构建、开发时 watch 文件变动自动构建等。

Nest 项目自然也是这样，所以它在 @nestjs/cli 这个包里提供了 nest 命令。

可以直接 npx 执行，npm 会把它下载下来然后执行：

```shell
npx @nestjs/cli new 项目名
```


也可以安装到全局，然后执行，更推荐这种：

java

复制代码

`npm install -g @nestjs/cli nest new 项目名`

不过后者要时不时升级下版本，不然可能用它创建的项目版本不是最新的：

sql

复制代码

`npm update -g @nestjs/cli`

那 nest 都提供了啥命令呢？

nest -h 看看:

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5825f0b71ed243ccaff39c6d5010110d~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

有创建新项目的 nest new，有生成某些代码的 nest generate，还有编译构建的 nest build，开发模式的 nest start 等。

分别看一下：

## nest new

nest new 我们用过，就是创建一个新的 nest 项目的。

它有这么几个选项：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6b94ca38aaef4d13acb999234c0ea3df~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

--skip-git 和 --skip-install 很容易理解，就是跳过 git 的初始化，跳过 npm install。

--package-manager 是指定包管理器的，之前创建项目的时候会让我们选择：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/74fe9a9c3c77465c904b587574c128ed~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

指定之后，就跳过包管理器选择这步了：

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/33871e97763d4ddc983f0a36099164c5~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

--language 可以指定 typescript 和 javascript，一般我们都选择 ts，用默认的就好。

--strict 是指定 ts 的编译选项是否开启严格模式的，也就是这么 5 个选项：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e004a861e0146fd8b2b48c7d954a744~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

默认是 false，也可以指定为 true：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3e7c8a78c688481695933d04055634b4~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

这个之后需要的话再改就行。

## nest generate'

nest 命令除了可以生成整个项目外，还可以生成一些别的代码，比如 controller、service、module 等。

比如生成 module：

arduino

复制代码

`nest generate module aaa`

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4ffdcbbd08fc4d048b59a860b55b4312~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

它会生成 module 的代码：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/14fdef17ae594e62b25623a4254a34d8~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

还会自动在 AppModule 里引入：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/357fecc074a540eeb6e4f70a1b5f5471~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

当然你也可以生成 controller、service 等代码：

复制代码

`nest generate controller aaa`

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/df4ffe08e5ee40a792f3ca7927bcca16~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

同样，它也会更新到 module 的依赖里去。

生成 service 也是一样：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1545c50932654314b02f35e4f9dac3be~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

当然，如果是要完整生成一个模块的代码，不需要一个个生成，可以用

复制代码

`nest generate resource xxx`

它会让你选择是哪种代码，因为 nest 支持 http、websocket、graphql、tcp 等，这里我们选择 http 的 REST 风格 api：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1480b030b48b4517b9ee38e8bda06e81~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

然后会让你选择是否生成 CRUD 代码：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/90edc9b8005547cb9d7c6e10e8f2b73a~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

选择是。

然后就会生成整个模块的 CRUD + REST api 的代码：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/94ebae465ba94991a154ffe7652b3557~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0d1c3305fcc647fea46629933fb4432f~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

当然，它同样会自动在 AppModule 引入：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e09b9ac107e840b2883c81764d0dc48d~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

这就是 nest generate，可以快速生成各种代码：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a33cc387c39b4870b698dbc518a98530~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

这些代码模版的集合是在 @nestjs/schematics 这个包里定义的。

nest new 创建项目的时候有个 --collection 选项，就是配置这个的。

不过一般我们不需要配置。

你可以在 [@nestjs/schematics](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fnestjs%2Fschematics%2Ftree%2Fmaster%2Fsrc%2Flib "https://github.com/nestjs/schematics/tree/master/src/lib") 里看到这些代码模版的定义：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0d9109ed36fa4287b05803b841ca5754~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

它的实现原理很简单，就是模版引擎填充变量，打印成代码：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6e441a62a8904bb8b4832df6b48508bb~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

nest generate 也有不少选项：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/76df459728cd4f69bfede05aec788658~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

--flat 和 --no-flat 是指定是否生成对应目录的：

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d37577a1f5ef4d9aa7b1ccb4c2502cda~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

--spec 和 --no-spec 是指定是否生成测试文件：

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d8f052d8088f41c3a881ab289c551c3e~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

--skip-import 是指定不在 AppModule 里引入：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a635e5d8a0b6494eb59109bfe5e10fce~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

也就是不生成这部分代码：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ef965983632c43b6a314aada5d2e572c~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

至于 --project，这是指定生成代码在哪个子项目的，等之后用到 monorepo 项目的时候再说。

这就是 nest cli 提供的快速生成各种代码的能力，是不是还挺方便的？

## nest build

然后就是 nest build 了，它是用来构建项目的:

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fe5cb526a5724f3a95a299e51193ff6e~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

执行 nest build，会在 dist 目录下生成编译后的代码。

同样，它也有一些选项：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2e74a180fe0f481cbe6d99f38071488e~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

--wepback 和 --tsc 是指定用什么编译，默认是 tsc 编译，也可以切换成 webpack。

这是 tsc 的编译产物：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/afac24d943e14446aa9c3d593770b06a~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

这是 webpack 的编译产物：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/706b166b0afc4fc18a6af653f846f63a~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

tsc 不做打包、webpack 会做打包，两种方式都可以。

node 模块本来就不需要打包，但是打包成单模块能提升加载的性能。

--watch 是监听文件变动，自动 build 的。

但是 --watch 默认只是监听 ts、js 文件，加上 --watchAssets 会连别的文件一同监听变化，并输出到 dist 目录，比如 md、yml 等文件。

--path 是指定 tsc 配置文件的路径的。

那 --config 是指定什么配置文件呢？

是 nest cli 的配置文件。

## nest-cli.json

刚刚我们说的那些选项都可以在 nest-cli.json 里配置：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a2297cf557e54157961a7e6ee67ee180~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

比如 compilerOptions 里设置 webpack 为 true 就相当于 nest build --webpack，一样的效果：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fad42dc73a404089abbd5f921c43350a~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

webpack 设置为 false 就是用 tsc 了。

deleteOutDir 设置为 true，每次 build 都会都清空 dist 目录。

而 assets 是指定 nest build 的时候，把那些非 js、ts 文件也复制到 dist 目录下。

可以通过 include、exclude 来精确匹配，并且可以单独指定是否 watchAssets。

不过只支持 src 下文件的复制，如果是非 src 下的，可以自己写脚本复制：

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4335579ee0a846fdbfc0ef022eed5c8a~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

然后是 generateOptions，这些就和我们 nest generate 时的 --no-spec、--no-flat 一样的效果。

比如我把 flat 设置为 false、spec 设置为 false，那再 generate 代码时就是这样的：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eb9a3995fd3a47138eb2be3f12169954~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

生成了一层目录，并且没有生成测试文件。

sourceRoot 是指定源码目录。

entryFile 是指定入口文件的名字，默认是 main。

而 $schema 是指定 nest-cli.json 的 schema，也就是可以有哪些属性的：

[json.schemastore.org/nest-cli](https://link.juejin.cn/?target=https%3A%2F%2Fjson.schemastore.org%2Fnest-cli "https://json.schemastore.org/nest-cli")

这是一种 json schema 的规范，还是挺容易看懂的：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6afce9b2ea204fd49f1ef2fef2c59312~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

如果想全面了解 nest-cli.json 都有啥属性，可以看看这个 schema 定义。

## nest start

最后，再来看下 nest start 命令：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/42a6eb5dd0f94712ace4e9db607701a7~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

可以看到每次重新 build 了，并且用 node 把 main.js 跑了起来。

它有这些选项：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c961834fd8124bcb8803a39769f30679~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

--watch 是最常用的选项了，也就是改动文件之后自动重新 build：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/35e9594f302f4344be3adc4e84af0342~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

--debug 是启动调试的 websocket 服务，用来 debug。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/921f0364a9ef4f5799a7217b0ebef375~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

--exec 可以指定用什么来跑，默认是用 node 跑，你也可以切换别的 runtime。

其余选项和 nest build 一样，就不复述了。

## nest info

最后还有个 nest info 命令，这个就是查看项目信息的，包括系统信息、 node、npm 和依赖版本：

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/71eaf4f2c4d24447bab2aa00e598c7dc~tplv-k3u1fbpfcp-jj-mark:2835:0:0:0:q75.awebp)

## 总结

nest 在 @nestjs/cli 包里提供了 nest 命令，它可以用来做很多事情：

- 生成项目结构和各种代码
- 编译代码
- 监听文件变动自动编译
- 打印项目依赖信息

也就是这些子命令：

- nest new 快速创建项目
- nest generate 快速生成各种代码
- nest build 使用 tsc 或者 webpack 构建代码
- nest start 启动开发服务，支持 watch 和调试
- nest info 打印 node、npm、nest 包的依赖版本

并且，很多选项都可以在 nest-cli.json 里配置，比如 generateOptions、compilerOptions 等。

学会用 nest cli，是学好 nest 很重要的一步。