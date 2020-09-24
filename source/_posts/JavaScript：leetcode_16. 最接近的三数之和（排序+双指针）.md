---
title: JavaScript：leetcode_16. 最接近的三数之和（排序+双指针）
date: 2020-06-24 16:27:44
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode_16, 最接近的三数之和（排序+双指针）
--- 
### 题目说明
```
给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

 

示例：

输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
 

提示：

3 <= nums.length <= 10^3
-10^3 <= nums[i] <= 10^3
-10^4 <= target <= 10^4
```
首先这个题可以使用3重for循环来遍历求值，并记录最接近target的值，但是会超时。
### 解题思路一
1. 对数组进行从小到大排列
2. 结果由三个数构成，我们提取出第一个数字`nums[i]`, 剩下的值就是`target-nums[i]`，我们需要在剩余数组元素中找到两个值`nums[pb]`,`nums[pc]`，最接近`target-nums[i]`
3. 由于三个元素不可重复，所以我们首先遍历`nums`，然后在每次遍历中，设定`pb`的初始值为 `i + 1`，`pc`的初始值为`nums.length - 1`。
4. 当前我们所求的当前值为`target_close_help = nums[i] + nums[pb] + nums[pc];`，另外用`target_close`记录最接近target的值。
5. 每次遍历，`nums[i]`是确定的。所以只需要判断`target_close_help` 是否**大于** `target`，如果**大于**则`pc--`，否则`pb++`；若`target_close_help`**等于**`target`则直接返回。
	1. 由于`nums`从**小到大**排列，所以`pc--`代表`nums[pc]`的值会**变小**，`target_close_help`的值也会变**小**，这样才会慢慢接近`target`，反之亦然`pb++`是同样的道理。
6. 最后判读`target_close_help` 是否更接近`target`，若**更接近**则用`target_close`记录该次`target_close_help`
7. 若没有相等于target的情况出现，最后则返回target_close的值。
### 代码实现一
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var threeSumClosest = function(nums, target) {
    nums = nums.sort((a,b) => {
        return a-b;
    })
    let target_close = 100001;
    for (let i = 0; i < nums.length; i++) {
        let pb = i + 1;
        let pc = nums.length - 1;
        while(pc > pb) {
            let target_close_help = nums[i] + nums[pb] + nums[pc];
            if (target_close_help < target) {
                pb++;
            } else if (target_close_help > target) {
                pc--;
            } else {
                return target;
            }
            if (Math.abs(target_close_help - target) < Math.abs(target_close - target)) {
                target_close = target_close_help;
            }
        }
    }
    return target_close;
};
```

