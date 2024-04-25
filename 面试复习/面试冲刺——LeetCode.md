## 3.6
***
### **1. 两数之和**
```typescript
function twoSum(nums: number[], target: number): number[] {
    const has = new Map<number,number>()
    return nums.map((item,index)=>{
        if(has.has(target - item)) return [has.get(target - item), index]
        has.set(item, index)
    }).flat().filter(item => item != undefined)
}
```
**思路解析：**
- 暴力解决所耗时间在查询`x+y ?= target`中，因此选用`hashmap`来优化
- 当前`map`中若含有`target-current.value`则证明已经找到
- 若无，则将当前值加入`map`
#### 复习打卡
- 3.8
### **17.电话号码的字母组合**
```typescript
function letterCombinations(digits: string): string[] {
    if(digits === "") return []
    // * 非空情况
    const numberBoard = {
        2:['a','b','c'],
        3:['d','e','f'],
        4:['g','h','i'],
        5:['j','k','l'],
        6:['m','n','o'],
        7:['p','q','r','s'],
        8:['t','u','v'],
        9:['w','x','y','z']
    }
    const res: string[] = [] // * 路径数组
    const end: number = digits.length // * 路径长度
    const dfs = (initial:number, str:string) => {
        if(str.length === end) {
            // * 走完路径长度
            res.push(str)
            return
        }
        const ans = numberBoard[digits[initial]] // * 当前分支路径
        for(let i = 0; i < ans.length; i ++)
            dfs(initial + 1, str + ans[i])
        return res
    }
    return dfs(0,"")
};
```
**思路解析：**
- 虽然一个号码对应多个字母，但是`digits`长度是给定了的，我们仅需要返回每一个长度为`digits.length`的字符串数组即可。则路径长度为`digits.length`
- 剩下的就是一个简单的`dfs`
#### 复习打卡

- 3.8
	今日理解，创建路径和路径长度。如果走完一个路径，证明得到了一个串
	每一个串都是由`digits`字符串中的数字从上往下全排列得到的，因此要加上一个当前串的遍历
## 3.7

### 438.找到字符串中所有字母异位词
```typescript
function findAnagrams(s: string, p: string): number[] {
    const ans: number[] = []
    const pMap: number[] = new Array(26).fill(0) // * p串中每个字母出现的次数
    const len = s.length
    const sMap: number[][] = new Array(len + 1)
        .fill([])
        .map(() => new Array(26).fill(0))
    // * s串中每个字母出现的次数
    for (let i = 0; i < p.length; i++) pMap[p.charCodeAt(i) - 97]++ // * 统计p串中每个字母出现的次数
    for (let i = 0; i < len; i++) {
        // * 为了后面构建前缀和数组(从1开始比较方便)
        sMap[i + 1][s.charCodeAt(i) - 97]++ // * 统计s串中每个字母出现的次数
    }

    // * 构建前缀和数组
    for (let i = 1; i <= len; i++) {
        for (let j = 0; j < 26; j++) {
           sMap[i][j] += sMap[i - 1][j]
        }
    }
    // * 枚举p串的子串
    for (let i = p.length; i <= len; i++) {
        let isMatched = true
        for (let j = 0; j < 26; j++) {
            if (sMap[i][j] - sMap[i - p.length][j] != pMap[j]) isMatched = false
        }
        if (isMatched) ans.push(i - p.length)
    }
    return ans
}
```
#### 复习打卡
- 3.9 
	- 坑：声明二维数组的方法+需要new sMap为len+1长度，因为从1开始构建的前缀和数组我
- 3.11

### 3. 无重复字符的最长子串
```typescript
function lengthOfLongestSubstring(s: string): number {

  let ans = 0 // * 返回的答案
  let head = 0 // * 窗口左
  let tail = 0 // * 窗口右

  let wMap = new Array(26).fill(0) // * 窗口中字符hash表
  while (tail < s.length) {
    // * 如果没走到底,而且窗口里面没有这个元素
    if (tail < s.length && !wMap[s.charCodeAt(tail) - 97]) {
      // * 加入窗口
      wMap[s.charCodeAt(tail) - 97] = 1
      // * 窗口右移
      tail++
    }
    // * 更新一下窗口长度
    ans = Math.max(ans, tail - head)

    if (wMap[s.charCodeAt(tail) - 97]) {
      // * 移出窗口
      wMap[s.charCodeAt(head) - 97] = 0
      // * 窗口左移
      head++
    }
  }
  return ans
}
```

#### 复习打卡
- 3.9
	- 坑！！！检查有无的时候检查的是tail(当前插入的数)，右移tail，左移head
- 3.9
思考：双指针指向窗口头尾，每一次在尾添加都是在增加窗口长度，所以才需要更新ans,而出现了相同的字符需要head指针右移时，窗口在减小，并不需要更新。在tail入队时，我们已经保证了其的首见性。
### 206.反转链表
#### 迭代方法
```typescript
function reverseList(head: ListNode | null): ListNode | null {
    if(head === null) return null
    let prev:ListNode = null
    let cur: ListNode = head
    while(cur) {
        const next = cur.next
        cur.next = prev
        prev = cur    
        cur = next
    }
    return prev
```
##### 复习打卡
- 3.9
	- prev保存上一个，cur是这一个。迭代完后,cur->null,prev->当前头/原始尾
## 3.8

### 160.相交链表
```typescript
function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
    if(headA == null || headB == null) return null
    let pa = headA
    let pb = headB
    while(pa != pb) {
	    pa = pa != null ? pa.next : headB
        pb = pb != null ? pb.next : headA
    }
    return pa;
};
```
- 在`pa`,`pb`交换链表起始点的时候，就消除了步长差
#### 复习打卡
- 3.11补3.11打卡。
### 208.实现Trie(前缀树)
```typescript
class Trie {
    Word_Node_Son: number[][]
    current_word: number
    end_word_number: number[]
    constructor() {
        this.Word_Node_Son = new Array(200000)
            .fill([])
            .map(() => new Array(26).fill(0))
        this.current_word = 0
        this.end_word_number = new Array(200000).fill(0)
    }
    insert(word: string): void {
        let next = 0
        for (let i = 0; i < word.length; i++) {
            if (!this.Word_Node_Son[next][word.charCodeAt(i) - 97])
                this.Word_Node_Son[next][word.charCodeAt(i) - 97] = ++this.current_word
            next = this.Word_Node_Son[next][word.charCodeAt(i) - 97]
        }
	        this.end_word_number[next]++
   }
    search(word: string): boolean {
        let next = 0
        for (let i = 0; i < word.length; i++) {
            if (!this.Word_Node_Son[next][word.charCodeAt(i) - 97]) return false
            next = this.Word_Node_Son[next][word.charCodeAt(i) - 97]
        }
        return this.end_word_number[next] > 0
    }
    startsWith(prefix: string): boolean {
        let next = 0
        for (let i = 0; i < prefix.length; i++) {
            if (!this.Word_Node_Son[next][prefix.charCodeAt(i) - 97]) return false
            next = this.Word_Node_Son[next][prefix.charCodeAt(i) - 97]
        }
       return this.end_word_number[next] >= 0
    }
}
```       

#### 复习打卡
- 3.11补3.10打卡
思考：`startWith`的条件是`>=0`，是因为当以之为单词时也满足
大概思路：遍历一个单词，根据`currentWord`确定路径，在路径上出现过的就继续走，没出现过的就加入。某一个单词的子节点是由他的值确定的。

## 3.9

### 70.爬楼梯
```typescript
function climbStairs(n: number): number {
    const ans: number[] = [0,1,2]
    for(let i = 3; i <= 45; i ++) {
        ans[i] = ans[i-1] + ans[i-2]
    }
    return ans[n]
};
```

### 118.杨辉三角
```typescript
function generate(numRows: number): number[][] {
    const ans: number[][] = new Array(30).fill([]).map((_item, index) => {
        return new Array(index + 1).fill(1)
    })

    for(let i = 0; i < numRows; i++) {
        // * 不给首位赋值
        for(let j = 1; j < ans[i].length - 1; j++) {
            ans[i][j] = ans[i-1][j] + ans[i-1][j-1]
        }   
    }
    return ans.slice(0,numRows)
}
```

### 198.打家劫舍
```typescript
function rob(nums: number[]): number {
    const len: number = nums.length // * 抢前len间房子的钱
    if(nums == null || len == 0) return 0
    if(len == 1) return nums[0]
    if(len == 2) return Math.max(...nums)
    const money: number[] = new Array(len).fill(0) // * 今晚前i间房子能最多抢这么多钱
    money[0] = nums[0]
    money[1] = Math.max(nums[0],nums[1])

    for (let i = 2; i < len; i++) {
        money[i] = Math.max( nums[i] + money[i - 2],money[i - 1])
    }
    return money[len - 1]
}
```

- 问题划分为子问题
	- 求前k间（仅有1间则是此间，2间则是二者之和）
	- 如果选择前k间的方案，则不能选择k+1间
	- 选择了k+1间则选用前k-2间的方案
	- 状态转移方程`money[i] = Math.max(nums[i]+money[i-2],money[i-1])`

## 3.10
### 94.二叉树的中序遍历
```typescript
function inorderTraversal(root: TreeNode | null): number[] {
    const ans: number[] = []
  
    const forTree = (node: TreeNode | null) => {
        if(node === null) return
        forTree(node.left)
        ans.push(node.val)
        forTree(node.right)
    }
    
    forTree(root)
    return ans
};
```
