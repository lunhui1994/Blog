---
title: JavaScript：leetcode_238. 除自身以外数组的乘积（左右乘积列表）
date: 2020-06-04 09:47:19
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode_238, 除自身以外数组的乘积（左右乘积列表）
---
### 题目说明
```

给你一个长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

 

示例:

输入: [1,2,3,4]
输出: [24,12,8,6]
 

提示：题目数据保证数组之中任意元素的全部前缀元素和后缀（甚至是整个数组）的乘积都在 32 位整数范围内。

说明: 请不要使用除法，且在 O(n) 时间复杂度内完成此题。

```

### 解题思路一（左右乘积列表）
1. 如果可以用除法，那我们只需要求出列表乘积，然后除以`nums[i]`就是所求值了。
2. 不用除法的情况下，我们需要求得`nums[i]`的左侧乘积L，和右侧乘积R，然后`L*R`求得目标值
3. 那么我们创建两个乘积列表`L,R`。`L[i]`代表从`nums`中从`0 ~ i`的乘积，`R[i]`代表`nums`中从`i ~ nums.length - 1`的乘积。
4. 那么我们所求的`output[i]`就变成了`L[I-1] * R[i+1]`;
### 代码实现一
```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var productExceptSelf = function(nums) {
    if (nums.length <= 2) {
        return nums.reverse();
    }
    let L = [nums[0]]
    let R = new Array(nums.length);
    R[nums.length-1] = nums[nums.length-1];
    for(let i = 1, j = nums.length - 2; i < nums.length; i++, j--) {
        L[i] = L[i-1] * nums[i];
        R[j] = R[j + 1] * nums[j];
    }
    for(let i = 1; i < nums.length - 1; i++) {
        nums[i] = L[i-1] * R[i + 1]; 
    }
    nums[0] = R[1];
    nums[nums.length-1] = L[nums.length-2];
    return nums;
};
```

