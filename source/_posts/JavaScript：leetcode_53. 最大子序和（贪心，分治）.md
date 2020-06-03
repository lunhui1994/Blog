---
title: JavaScript：leetcode_53. 最大子序和（贪心，分治）
date: 2020-05-03 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 最大子序和, 贪心, 分治
---

### 题目说明
```
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
进阶:

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

```
<!-- more -->

### 解题思路一
贪心算法。
1. 依次求数组前i项的和，再求和之前，判断sum的值是否小于0，若小于零，直接抛弃。
2. 若大于0，就sum+=nums[i]
3. 由于子序列是必须要连续的，所以我们需要一个记录最大值的变量,maxSum.
4. 每次sum求和之后，用maxSum记录最大值 ` maxSum= Math.max(sum, maxSum)`;最终返回max即可


### 代码实现一
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
 var maxSubArray = function(nums) {
    let sum = 0;
    let maxSum = Number.MIN_SAFE_INTEGER;
    for (let i = 0; i < nums.length; i++) {
        if (sum < 0) {
            sum = nums[i];
        } else {
            sum += nums[i];
        }
        maxSum = Math.max(maxSum, sum)
    }
    return maxSum
 };
```



### 解题思路二
分治法。
1. 将nums分为3个部分：
	1. 从nums[mid]处向两边求和，取得最大值。
	2. nums的左半边 （仿照1的方式求得左半边的和）
	3. nums的右半边（仿照1的方式求得右半边的和）
2. 求出这三个和之后，返回其最大的一个值。
3. 从步骤一得出，我们需用使用递归计算。

### 代码实现一
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
   
   return fz(0, nums.length-1, nums)
};

function fz(left, right, nums) {
    if (left === right) {
       return nums[left]
    }
    let mid = left + ((right - left) >> 1);
    let leftMax = Number.MIN_SAFE_INTEGER
    let rightMax = Number.MIN_SAFE_INTEGER
    let midMax = crossNum(left, right, nums)
    leftMax = fz(left, mid, nums)
    rightMax = fz(mid+1, right, nums)
    return Math.max(Math.max(midMax, rightMax), leftMax);
}

function crossNum(left, right, nums) {
    if (left === right) {
        return nums[left]
    }
    let mid = left + ((right - left) >> 1)
    let leftSum = 0;
    let leftMax = Number.MIN_SAFE_INTEGER;
    for (let i = mid; i >= left; i--) {
        leftSum += nums[i];
        leftMax = Math.max(leftMax, leftSum)
    }
    let rightSum = 0;
    let rightMax = Number.MIN_SAFE_INTEGER;
    for (let i = mid + 1; i <= right; i++) {
        rightSum += nums[i];
        rightMax = Math.max(rightMax, rightSum)
    }

    return rightMax + leftMax;
}
```

