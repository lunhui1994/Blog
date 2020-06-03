---
title: JavaScript：leetcode_287. 寻找重复数（二分法）
date: 2020-05-26 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 寻找重复数
---

### 题目说明
```
给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

示例 1:

输入: [1,3,4,2,2]
输出: 2
示例 2:

输入: [3,1,3,4,2]
输出: 3
说明：

不能更改原数组（假设数组是只读的）。
只能使用额外的 O(1) 的空间。
时间复杂度小于 O(n2) 。
数组中只有一个重复的数字，但它可能不止重复出现一次。
```
<!-- more -->

### 解题思路一
1. 先放一种前端比较好理解的。
2. indexOf会返回数组中该元素出现的第一次的位置
3. 我们利用这个特性，当indexOf的值跟目前的index不一致时，说明之前出现过一次。返回即可
### 代码实现一
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */

var findDuplicate = function(nums) {
    let res = 0;
    nums.map((item, key, arr) => {
        if (arr.indexOf(item) !== key) {
            res = item;
        }
    });
    return res;
};
```
### 解题思路二（二分法）
1. 首先看题意：数字范围为`1 ~ n`，那其实就是在`1~n`的范围内找到哪个元素在`nums`中`重复存在`。
2. so，`1 ~ n`的序列。是`有序`的，可以用二分找了，以`1~n`为基础，以`nums`为条件判断的元素。
3. 那怎么找呢。比如我们找到中间节点`mid`，判断`nums`数组中比`mid小`的有多少个`（prev）`，
	1. 按正常来讲比如`mid为3`，那么从`1到n <= 3`的数量应就是`[1,2,3]`,一共是`3`个啦。
	2. 所以如果重复的元素`比3小`的话，那么`3`的`prev`就变成`4以上`了，因为[1,2,3]就变成了`[1,1,2,3]`或者`[1,2,2,3]`,等等，
	3. 所以我们就可以通过`prev`的大小来锁定重复元素的范围是在`1 ~ mid`还是在`mid+1 ~ n`；
4. 接下来就很简单了。就是一个二分法了。
### 代码实现一
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */

var findDuplicate = function(nums) {
    let res = null;
    function find(start, end, nums) {
        if (end == start) { //找到最终目标了
           return void ( res = end );
        } 
        
        let mid = start + ((end - start) >> 1);
        let prev = 0;
        
        for (let i = 0; i < nums.length; i++) {
            if (nums[i] <= mid) {
                prev++;
            }
        }
        
        if (prev > mid) {
            find(start, mid, nums)
        } else {
            find(mid + 1, end, nums)
        }
        
    }
    find(1, nums.length - 1, nums);
    return res;
};
```