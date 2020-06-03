---
title: JavaScript：leetcode_560. 和为K的子数组（前序和）
date: 2020-05-15 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 和为K的子数组, 前序和
---

### 题目说明
```
给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。

示例 1 :

输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
说明 :

数组的长度为 [1, 20,000]。
数组中元素的范围是 [-1000, 1000] ，且整数 k 的范围是 [-1e7, 1e7]。
```
<!-- more -->

### 解题思路一
1. 该题可以使用前序和进行计算和为k的个数，也可以用动态规划的思路来理解
2. 我们记录该元素之前的所有前缀和。
3. 然后利用上一次的结果，分别加上该元素的值，获取该元素所有的前缀和。（注意不要漏掉只有本身的结果）。
4. 判断前缀和集合中有几个值为k的情况。就是该元素对k个数的解。
5. 记录所有元素的解的个数，求和。即为结果


### 代码实现一
```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var subarraySum = function(nums, k) {
    let kNums = nums[0] === k ? 1 : 0;
    let now = [nums[0]]
    for (let i = 1; i < nums.length; i++) {
        now = now.map((item) => {
            if (item + nums[i] == k) {
                kNums++;
            }
            return item + nums[i];
        })
        nums[i] === k ? kNums++ : ""
        now.push(nums[i]);
    }
    return kNums
};
```