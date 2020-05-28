---
title: JavaScript：leetcode_152. 乘积最大子数组（动态规划）
date: 2020-05-18 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 动态规划, 乘积最大子数组
---

### 题目说明
```
给你一个整数数组 nums ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。
示例 1:

输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
示例 2:

输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```

### 解题思路一
1. 该题与之前所写的[560.和为K的子数组](https://blog.csdn.net/lunhui1994_/article/details/106136488)很相似，解法也都是一样的。
2. 依旧是对前缀进行操作。不同的是，我们这次不需要保留所有的结果，只需要保留本次结果的最大值`nowMax` 和最小值`nowMin` 。取min主要是为了复数的情况。
3. 我们依赖于上一次的状态，求出本次的最大最小值。然后传入下一次状态。
		1. `nowMax = Math.max(res[0] * nums[i], res[1] * nums[i], nums[i]);`
 		2. `nowMin = Math.min(res[0] * nums[i], res[1] * nums[i], nums[i]);`
	   	3. `res = [nowMax, nowMin  ]`
4. 在此过程中我们可以求出最大值max


### 代码实现一
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxProduct = function(nums) {
    if (!nums.length) {
        return 0;
    }
    let max = nums[0]
    let min = nums[0]
    let res = [nums[0], nums[0]];
    for (let i = 1 ; i < nums.length; i++) {
        let nowMax = Math.max(res[0] * nums[i], res[1] * nums[i], nums[i]);
        let nowMin = Math.min(res[0] * nums[i], res[1] * nums[i], nums[i]);
        max = Math.max(nowMax, max);
        // min = Math.min(nowMin, min);
        res = [nowMax, nowMin];
    }
    return max;
};
```