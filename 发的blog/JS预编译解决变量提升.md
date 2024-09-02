---
title: JS预编译解决变量提升
id: d95339df-fa5a-4e75-af42-6b6717bc34a0
date: 2024-04-25 13:42:47
auther: 406
cover: 
excerpt: JS预编译解决变量提升 JS执行三部曲 语法分析 预编译 函数声明整体提升、变量声明提升(预编译造成的现象) 预编译前奏 imply global 暗示全局变量：即任何变量，如果变量未经声明就赋值，此变量就为全局对象所有 a = 123 // * 此时a归window所有var a = b = 1
permalink: /archives/1714023763544
categories:
 - qian-duan
tags: 
 - javascript
---

# JS预编译解决变量提升

## JS执行三部曲

### 语法分析

### 预编译

- **函数声明整体提升、变量声明提升**(预编译造成的现象)

##### 预编译前奏

1. `imply global` 暗示全局变量：即任何变量，如果变量未经声明就赋值，此变量就为**全局对象**所有

```javascript
a = 123 // * 此时a归window所有
var a = b = 123

function test() {
	var c = d = 123 // *此时d归window所有
}
```

2. 一切声明的全局变量，全是 `window`的属性

```javascript
var a = 213; ==> window.a = 123; 
```

**预编译发生在函数执行的前一刻**

#### 四部曲

例子：

```javascript
function fn(a) {
	console.log(a)
	var a = 123
	console.log(a)
	function a() {}
	console.log(a)
	var b = function() {}
	console.log(b)
	function d() {}
}
fn(1)
```

1. **创建AO对象**（$Activation Object$）（执行期上下文）
2. **找形参和变量声明，将变量和形参名作为AO属性名，值为undefined**

```javascript
AO {
	a:undefined // * var a = 123
	b:undefined // * var b = function() {}
}
```

3. **将实参值和形参统一**

```javascript
AO {
	a: 1 // * fn(1)
	b: undefined 
}
```

4. **在函数体里面找函数声明，值赋予结构体**

```javascript
AO {
	a: function a(){}
	b: undefined
	d: function d(){}
}
```

**解析**

```javascript
function fn(a) {
	console.log(a)
	// * 在AO中找到a 打印function a(){}
	var a = 123
	// 执行 a = 123 AO.a = 123
	// var声明(变量提升现象)在AO过程实现
	console.log(a)
	// 打印 123
	function a() {} // 预编译已经做过了
	console.log(a) // 打印123
	var b = function() {} // AO.b = function(){}
	console.log(b) // function(){}
	function d() {} // 预编译做过了
}
fn(1)
```

**全局预编译除无传参外其他一致（GO（Global Object === window）)**

### 解释执行
