---
title: 数组Tips
id: 9393978c-848d-4287-9a50-48ccb31af863
date: 2024-04-25 13:33:04
auther: 406
cover: 
excerpt: 数组小技巧
permalink: /archives/shu-zu-tips
categories:
 - qian-duan
tags: 
 - javascript
---

# 数组Tips

## 数组并/交/差集

```
const arr1 = [33, 22, 22, 55, 33, 11, 33, 5]
const arr2 = [22, 22, 55, 77, 88, 88, 99, 99]

// * 并集
const union = (arr1, arr2) => Array.from(new Set([...arr1, ...arr2]))

// * 交集
const intersection = (arr1, arr2) =>
	Array.from(new Set(arr1.filter((x) => arr2.includes(x))))

// * 差集
const difference = (source, skip) =>
	Array.from(new Set(source.filter((x) => !skip.includes(x))))

console.log(union(arr1, arr2)) // [ 33, 22, 55, 11, 5, 77, 88, 99 ]
console.log(intersection(arr1, arr2)) // [ 22, 55 ]
console.log(difference(arr1, arr2)) // [ 33, 11, 5 ]
console.log(difference(arr2, arr1)) // [ 77, 88, 99 ]

```

## 数组 `type`分类

```javascript
const data = [
	{ type: 'stu', name: '张三', age: 15 },
	{ type: 'stu', name: '李四', age: 17 },
	{ type: 'stu', name: '王五', age: 15 },
	{ type: 'stu', name: '赵六', age: 16 },
	{ type: 'tch', name: '周天', age: 26 },
	{ type: 'tch', name: '钱八', age: 30 },
]

function arrayGroupBy(array, key) {
	const hash = {}
	const res = {}
	array.forEach((item, index) => {
		if (!hash[item[key]]) {
			const arr = []
			arr.push(item)
			res[item[key]] = arr
			hash[item[key]] = true
		} else {
			res[item[key]].push(item)
		}
	})
	return res
}

const res = arrayGroupBy(data, 'type')
console.log(res) 
/*
{
  stu: [
    { type: 'stu', name: '张三', age: 15 },
    { type: 'stu', name: '李四', age: 17 },
    { type: 'stu', name: '王五', age: 15 },
    { type: 'stu', name: '赵六', age: 16 }
  ],
  tch: [
    { type: 'tch', name: '周天', age: 26 },
    { type: 'tch', name: '钱八', age: 30 }
  ]
}
*/


```

## 移除数组元素

> 给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
> 不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。
> 元素的顺序**可以改变**。你不需要考虑数组中超出新长度后面的元素。

### 1. 双指针

由于元素的顺序可以改变，我们可以直接用末尾元素替换当前元素即可。

```javascript
var removeElement = function (nums, val) {
	const len = nums.length
	let l = 0,
		r = len - 1

	while (l <= r) {
		if (nums[l] === val) nums[l] = nums[r--]
		else l++ // 注意是多次比较，不同之后左指针才前进
	}
	return l
}
```
