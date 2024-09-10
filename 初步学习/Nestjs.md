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
