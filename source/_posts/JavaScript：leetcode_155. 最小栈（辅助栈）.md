---
title: JavaScript：leetcode_155. 最小栈（辅助栈）
date: 2020-05-12 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 最小栈, 辅助栈
---

### 题目说明
```
设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。
 

示例:

输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
 

提示：

pop、top 和 getMin 操作总是在 非空栈 上调用。

```
<!-- more -->


### 解题思路一
1. 首先确定栈的特点吧，`先进后出`，只能从`栈顶进栈出栈`，然后我们用数组来模拟他，将`数组末尾`当作`栈顶`，在此进栈出栈。
2. 其实就是实现一个数组的`push，pop`功能，然后增加获取`最小值`的api和返回数组`最后一位`的api
3. 由于最开始栈为空，所以栈是通过`push`，或者`pop`得到的。并且题目要求最小值要通过常数次操作得到，也就`getMin`的时间复杂度为`O(1)`.那我们可以在push，pop的过程中，确定最小值。
### 代码实现一
```javascript
/**
 * initialize your data structure here.
 */
var MinStack = function() {
    return void (
            this.stack = [],
            this.min = [Number.MAX_SAFE_INTEGER], //整数类型的最大值
            this.topValue = null
        );
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function(x) {
    return void (this.stack[this.stack.length] = x, this.topValue = x, this.min[this.min.length] = (x > this.min[this.min.length - 1] ? this.min[this.min.length - 1] : x));
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    return void (this.stack.length -= 1, this.topValue = this.stack[this.stack.length - 1], this.min.length -= 1);
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
   return this.topValue;
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function() {
    return this.min[this.min.length - 1];
};

/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```