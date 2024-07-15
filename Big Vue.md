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
	2. render -> patch 处理不同类型的组件 *$Flags$区分*
		1. 处理组件类型
			1. 组件初始化
				1. 创建 component instance
				2. setup compoent
					1. porps->slots->setup()->render()
				3. setupRenderEffect
					1. render()->BeforeMount->patch(递归)->mouted
3. 组件更新
	1. 响应式更新
	2. 触发组件effect执行，instance.update
	3. 执行当前组件的render获取到vnode(subTree)
	4. beforeMount->onVnodeBeforeUpdate->patch->updated->onVnodeUpdated