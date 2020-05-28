---
title: JavaScript：leetcode_50. Pow(x, n)（幂计算）
date: 2020-05-11 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, Pow(x, n), 幂计算
---

### 题目说明
```
1.实现 pow(x, n) ，即计算 x 的 n 次幂函数。

示例 1:

输入: 2.00000, 10
输出: 1024.00000
示例 2:

输入: 2.10000, 3
输出: 9.26100
示例 3:

输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
说明:

-100.0 < x < 100.0
n 是 32 位有符号整数，其数值范围是 [−231, 231 − 1] 。

```


### 解题思路一
1. 计算X的n次幂。首先要理解
	1. **x^n^ === (x^2^)^n/2^**  理解幂计算就可以了。
	2. 当n为奇数的时候，我们记得计算完平方之后再乘以 x
2. 请看代码实现。 

### 代码实现一
```javascript
/**
 * @param {number} x
 * @param {number} n
 * @return {number}
//  */
var myPow = function(x, n) {
    n < 0 ? (x = 1/x ,n = -n) : ''
    let res = 1;
    while(n) {
        if(n & 1) res = res * x; // 当n为奇数时，我们需要收集一下落单的x
        x = x * x;
        n = Math.floor(n / 2)
        // n >>>= 1 ; 需要用>>> 因为数字2147483648 用二进制 2^32 - 1 位表示不了了，所以要
    }
    return res
};
```