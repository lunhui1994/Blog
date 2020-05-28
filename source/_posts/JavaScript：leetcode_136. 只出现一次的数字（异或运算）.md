---
title: JavaScript：leetcode_136. 只出现一次的数字（异或运算）
date: 2020-05-14 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 只出现一次的数字, 异或运算
---

### 题目说明
```
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

输入: [2,2,1]
输出: 1
示例 2:

输入: [4,1,2,1,2]
输出: 4
```

### 解题思路一
主要看一下题目的要求，线性复杂度，不适用额外的空间。
1. 这里可以巧妙的使用异或运算符的特性，`相同值的异或为0；所有的值，异或0都是本身。`数组的所有项向异或之后的结果就是只出现一次的值
2. 我使用了reduce的数组方法，事实上可能也新开辟了空间，我们完全按照题意的话，可以直接使用数组第1项进行代替。


### 代码实现一
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    return nums.reduce((sum, item, key, arr) => {
        return (sum ^= item)
    }, 0)
};
```