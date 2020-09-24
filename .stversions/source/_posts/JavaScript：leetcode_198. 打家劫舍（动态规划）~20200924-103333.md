---
title: JavaScript：leetcode_198. 打家劫舍（动态规划）
date: 2019-09-26 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode_198, 打家劫舍
---

### 题目说明
```
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

示例 1:

输入: [1,2,3,1]
输出: 4
解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 2:

输入: [2,7,9,3,1]
输出: 12
解释: 偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```
<!-- more -->
### 解题思路一(动态规划，记录最优解)
1. 假如有一间房，偷取的最大值即为`nums[0].`
2. 假如有两间房，由于相邻房间不可取，偷取的最大值即为`Max(nums[0],nums[1])`
3. 假如有三间房，其取值有两种选择1. 偷取 `[1， 3]`房， 2. 偷取 `[2]` 房,取其最大值即可。
4. 那么，对于第`i`间房就有：`stole[i] = Math.max((stole[i - 2] + nums[i]), stole[i - 1]);`
5. 最后，stole末尾的值即为最大值
### 代码实现一
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    nums = [0, 0, ...nums]
    let stole = [0, 0]
    for (let i = 2; i < nums.length; i++) {
       stole[i] = Math.max((stole[i - 2] + nums[i]), stole[i - 1]);
    }
    return stole[stole.length - 1];
};
```
### 解题思路二（空间复杂度优化）
1. 以上代码，stole中其实只用到了末尾两位的值，所以我们可以将stole数组长度固定在2.
2. 不需要保存所有的stole情况。
### 代码实现二
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    nums = [0, 0, ...nums]
    let stole = [0, 0]
    for (let i = 2; i < nums.length; i++) {
       let help = stole[1];
       stole[1] = Math.max((stole[0] + nums[i]), stole[1]);
       stole[0] = help;
    }
    return stole[1];
};
```