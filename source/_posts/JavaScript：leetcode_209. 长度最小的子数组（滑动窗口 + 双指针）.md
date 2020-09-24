---
title: JavaScript：leetcode_209. 长度最小的子数组（滑动窗口 + 双指针）
date: 2020-06-29 00:32:16
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode_209, 长度最小的子数组（滑动窗口 + 双指针）
---
### 题目说明

给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的连续子数组，并返回其长度。如果不存在符合条件的连续子数组，返回 0。

 

示例：

输入：s = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的连续子数组。
 

进阶：

如果你已经完成了 O(n) 时间复杂度的解法, 请尝试 O(n log n) 时间复杂度的解法。
```

### 解题思路一
1. 第一种方法，比较常规，符合我们的脑回路。
2. 遍历数组，求遍历过的和sum。
3. 当sum >= s时，依次减去sum前面的数字，并判断是否依旧符合sum >= s。
4. 直到sum >= s不成立，记录start--到end的长度。
5. 继续遍历数组的下一位，循环2-3步骤，比较之后取最小长度。
### 代码实现一
```javascript
/**
 * @param {number} s
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function(s, nums) {
    let sum = 0;
    let start = 0;
    let end = 0;
    let min = nums.length + 1;
    for(let i = 0; i < nums.length; i++) {
        sum += nums[i];
        if (sum >= s) {
            end = i;
            while(sum >= s) {
                sum -= nums[start++];
            }
            start--;
            sum += nums[start];
            if (min > (end - start)) {
                min = end - start + 1;
            }
        }
    }
    if (min > nums.length) {
        return 0;
    }
    return min;
};
```

### 解题思路二（双指针）
1. 先求出nums数组中第一次符合条件的情况。也就是看nums的前多少位>=s。记录start，end。
2. start，end即为我们的双指针，分别代表子序列的开头和结尾。
3. 如果sum >= s, 我们就右移动start，求最小长度。
4. 如果sum < s, 我们就右移动end，求符合条件的情况。
5. 直到end到达nums的尾部结束。
### 代码实现二
```javascript
/**
 * @param {number} s
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function(s, nums) {
    let sum = 0;
    let start = 0;
    let end = 0;
    let min = nums.length + 1;
    while (sum < s) {
        sum += nums[end++];
    }
    min = Math.min(end - start, min);
    while(end <= nums.length) {
        if (sum >= s) {
            sum -= nums[start++];
             if (sum >= s) {
                min = Math.min(end - start, min);
            }
        } else {
            sum += nums[end++];
        }
    }
    if (min > nums.length) {
        return 0;
    }
    return min;
};
```
