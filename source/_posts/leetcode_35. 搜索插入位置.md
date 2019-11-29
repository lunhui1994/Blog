---
title: leetcode_35. 搜索插入位置
date: 2019-09-20 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: 搜索插入位置, leetcode_35, 二分法
---
### 题目解析：
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。
### 示例	
示例 1:
```
    输入: [1,3,5,6], 5
    输出: 2
```
示例 2:
```
    输入: [1,3,5,6], 0
    输出: 0
```
    
<!-- more -->
### 解题思路

    利用findIndex函数遍历数组。

### 解答
```
    var findFuc = function(el, index, arr) {
        return el >= this
    }
    var searchInsert = function(nums, target) {
        return nums.findIndex(findFuc, target) == -1 ? nums.length : nums.findIndex(findFuc, target)
    }; 
```

### 解题思路二

    利用二分法遍历数组。

### 解答二
```
    var searchInsert = function(nums, target) {
        if (nums[0] >= target) {
            return 0;
        } 
        if (nums[nums.length - 1] < target) {
            return nums.length;
        } 
        let left = 0, right = nums.length - 1, mid = parseInt((nums.length - 1)/2);
        while(left < (right-1)) {
            if (nums[mid] > target) {
                right = mid;
            } else if (nums[mid] < target) {
                left = mid;
            } else {
                return mid;
            }
            mid = parseInt((left + right) / 2);
        }
        return right;
    };

```

