# 概念理解

**Controller** 处理路由

请求体一般会传递 Json
```json
{
	username: 'xxx',
	password: 'xxx'
}
```

我们会通过 **dto**(data transfer object)来接收
![[9c5a32fca1704b23b62698a3028b73ae~tplv-k3u1fbpfcp-jj-mark_2495_0_0_0_q75 1.webp]]

**Service**处理业务逻辑


Service 作为**providers**，controller 作为**controllers**。
为什么 service 是 providers 呢？
**IOC（inverse of control)** 控制反转
**entities** 是封装对应数据库表的实体的。
![[580375b654ac445cb2cd07784824104c~tplv-k3u1fbpfcp-jj-mark_2495_0_0_0_q75.webp]]controller 接收请求参数，交给 model 处理（model 就是处理 service 业务逻辑，处理 repository 数据库访问），然后返回 view，也就是响应。
![[9f99087120e847eab901738bf8504d21~tplv-k3u1fbpfcp-jj-mark_2495_0_0_0_q75.webp]]
具体来说，有 Middleware、Guard、Interceptor、Pipe、Exception Filter 这五种。![[a4d0291cafa9449ca4702617464c5979~tplv-k3u1fbpfcp-jj-mark_2495_0_0_0_q75.webp]]
