---
title: JavaScript：leetcode_33. 搜索旋转排序数组（二分法）
date: 2020-04-26 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 搜索旋转排序数组, 二分法
---


### 题目描述
```
假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

示例 1:

输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
示例 2:

输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1

```
<!-- more -->


### 解题思路一

时间复杂度要O(log n) ，那么就不能直接用遍历了。遍历是O(n)

其实一个无序的序列，查找的最小时间复杂度就是O(n)

但是，根据题意，原序列为升序序列，那么旋转后的序列其实是两个有序的序列的拼接，且没有交集。

我的思路是**找到旋转点**，然后分别使用**二分法**查找。二分法的时间复杂度就是O(log n)。

寻找**旋转点**同样使用**二分法**。

寻找**旋转点**时，**左右序列都要包含mid**，进行判断，否则会出现正好在旋转点处分割左右序列的情况。

这样的方法时间复杂度为**O(2 log n)** 应该还是符合题意的。

###  题解一：
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function(nums, target) {
    let left = 0;
    let right = nums.length - 1;
    let flag = ef1(left, right, nums);
    let leftV = ef(left, flag, target, nums)
    let rightV = ef(flag + 1, right, target, nums)
    return leftV > -1 ? leftV : rightV
};
// 查找旋转点
var ef1 = function(left, right, nums) {
    let mid = Math.floor(left + (right - left) / 2); 
    if (right - left === 1) {
        if (nums[left] > nums[right]) {
            return left
        }
    }
    // 左序列 如果左大于右，那说明旋转点在左序列中。
    if (nums[left] > nums[mid]) {
        return ef1(left, mid, nums);
    }
     // 右序列 如果左大于右，那说明旋转点在左序列中。
    if (nums[mid] > nums[right]) {
        return ef1(mid, right, nums);
    }
    return 0;
}
// 查找target
var ef = function(left, right, target, nums) {
    let mid = Math.floor(left + (right - left) / 2); 
    if (right - left === 1 || right - left === 0) {
        if (target === nums[left]) {
            return left
        }
        if (target === nums[right]) {
            return right
        }
        return -1
    }
    if (nums[left] <= target && nums[mid] >= target) {
        return ef(left, mid, target, nums);
    }
    if (nums[mid + 1] <= target && nums[right] >= target) {
        return ef(mid + 1, right, target, nums);
    }
    return -1
}
```

