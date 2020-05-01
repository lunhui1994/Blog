---
title: JavaScript：面56 - I. 数组中数字出现的次数（分组异或）
date: 2020-04-25 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 数组中数字出现的次数, 分组异或
---
### 题目说明
```

一个整型数组 nums 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。

 

示例 1：

输入：nums = [4,1,4,6]
输出：[1,6] 或 [6,1]
示例 2：

输入：nums = [1,2,10,4,1,4,3,3]
输出：[2,10] 或 [10,2]
 

限制：

2 <= nums <= 10000

```

### 解题思路
1. 求出数组异或的结果。因为相同值的异或为0，所以最后异或的结果为两个不同数的异或结果res。
2. 将两个不同的值分别分割到两个不同的数组中。取res二进制中任意一位值为1的位置，作为区分标志flag。（1，说明两个不同值的二进制，在该位处的值一个为0，一个为1）
3. 遍历数组  根据（nums[i] & flag）的值区分为两个数组，并求出两个数组的异或结果。
4. 由于两个不同值被分开，并且相同值对同一值的位与（&）操作是相同的。所以，两个数组内除了不同值，其他都是由n对相同值构成，所以最后的异或操做是排除了n对相同值，最后分别得出了两个不同值。

### 代码实现
```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var singleNumbers = function(nums) {
    if (nums.length === 2){
        return nums;
    }
    nums.unshift(0);
    res = 0;
    for (let i = 1; i < nums.length; i++) {
        res ^= nums[i];
    }
    let flag = 1;
    while((flag & res) == 0) {
        flag <<= 1
    }
    let left = 0, right = 0;
    for (let i = 1; i < nums.length; i++) {
        if (nums[i] & flag) {
            left ^= nums[i]
        } else {
            right ^= nums[i]
        }
    }
    return [left, right]
};
```