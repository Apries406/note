![[Pasted image 20240715235152.png]]
- @vue/compiler-sfc
	- 编译 SFC 2 JS
- @vue/compiler-dom & @vue/compiler-core 
	- 解析 template 2 render function+
- h 函数创建虚拟节点
- @vue/runtime-dom 
- @vue/runtime-core
	- 跑起来全部靠这个
- @vue/reactivity
	- HE核中核
	- Proxy 对象，get/set
# 初始化流程
1. 创建 app
2. 初始化 mount 过程
	1. rootComponent -> VNode
	2. render -> patch 处理不同类型的组件