---
title: JavaScript：202. 快乐数（快慢指针）
date: 2020-04-30 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 快乐数, 快慢指针
---


### 题目描述
```
给定一个 没有重复 数字的序列，返回其所有可能的全排列。

示例:

输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]


```


### 解题思路一

广度优先遍历方法，递归解决

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200425150234903.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)
如图所示， 我们需要3个信息：
	1. 和已确定序列，
	2. 剩余序列
###  题解一：
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute = function(nums) {
    let res = [];
    for (let i = 0; i < nums.length; i++) {
        let copy = [...nums];
        join(copy.splice(i,1), copy, res);
    }
    return res;
};
// preArr为已确认的前序列
// arr为剩余的序列
var join = function(preArr, arr, res) {
    if (arr.length === 0) {
        return res.push(preArr);
    }
    for (let i = 0; i < arr.length; i++) {
        let copy = [...arr];
        join(preArr.concat(copy.splice(i,1)), copy, res);
    }
}
```

