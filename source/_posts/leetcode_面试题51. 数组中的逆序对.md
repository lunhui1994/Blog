---
title: leetcode_面试题51. 数组中的逆序对（归并排序记录逆序对）
date: 2020-04-24 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 数组中的逆序对, 归并排序
---

### 题目描述
```
在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

 

示例 1:

输入: [7,5,6,4]
输出: 5
 

```

### 解题思路一

N*N遍历求出逆序对的总数



###  题解一：
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
 var reversePairs = function(nums) {
     let res = 0;
     for (let i = nums.length - 1; i >= 0; i++) {
         for (let j = i; j >=0; j++) {
             if (nums[j] < nums[i]) {
                res++;
             }
        }
     }
     return res;
 };
```

很简单，但是毫无疑问超时。


### 解题思路二

归并排序算法，排序过程中记录逆序对的数量。最终nums还是一个升序序列。相当于我们在归并排序中顺手得到了逆序对的数量。

归并排序：它的思路就是，找到数组中间下标，然后排序左序列（left，middle）和右序列（middle+1，right）。结合递归一直分到序列长度为2，然后出栈排序。

归并排序的思路即：
1. [4,2,3,1,8,7,6,5] => [4,2,3,1]和[8,7,6,5]
2.	[4,2,3,1] => [4,2] 和 [3,1]
3.	到length==2,递归到终点，排序[4,2] => [2,4]。[3,1] => [1,3]。
4.	然后得到[2,4,1,3],再对其进行排序，因为子序列的顺序都是排好的。所以，我们只需要对比，[2,4]和[1,3]哪个序列中前面的值小，就摘出来，放在help中。
	1.	比如1和2比，1小，那么help就变成了[1]，左序列还是[2,4],右序列变成了[3].
	2.	然后3和2比，2小，那么help就变成了[1,2], 左序列变成了[4],右序列还是[3].
	3.	一直比到左右序列有一个序列为空时，再将另外一个序列依次加入help。


###  题解一：
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
 
let res = 0;

function mergeSort(nums, left, right) {
    if (left === right) return;
    let mid = Math.floor(left + ((right - left) >> 1));
    mergeSort(nums, left, mid)
    mergeSort(nums, mid + 1, right)
    let p1 = left;
    let p2 = mid + 1;
    let help = [];
    let i = 0;
    while(p1 <= mid && p2 <= right) {
        if (nums[p1] <= nums[p2]) {
            res += p2 - (mid + 1)//左序列的值，在赋值之前要看下help中，在它之前有几个右序列的值。就有几个逆序对。
        }
        help[i++] = nums[p1] > nums[p2] ? nums[p2++] : nums[p1++];
        
    }
    while(p1 <= mid) {
        help[i++] = nums[p1++];
        res += right - mid //若左序列还有剩余，那么剩余的都比右序列大，所以每个都要加上右序列的长度。
    }
    while(p2 <= right) {
        help[i++] = nums[p2++];
    }
    nums.splice(left, right - left + 1, ...help);
}

var reversePairs = function(nums) {
    if (!nums.length) {
        return 0;
    }
    res = 0;
    mergeSort(nums, 0, nums.length - 1);
    //nums此时是一个升序的序列
    return res 
};
```
