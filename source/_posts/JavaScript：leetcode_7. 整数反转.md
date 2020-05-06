---
title: leetcode_7. 整数反转
date: 2019-07-27 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 整数反转
---


### 题目解析：
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

### 示例	
示例 1:

>   输入: 123
>   输出: 321

示例 2:

>   输入: -123
>   输出: -321
示例 3:

>   输入: 120
>   输出: 21

### 注意
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

<!-- more -->
### 解题思路
题目就是反转，问题在于反转过程中是否溢出。

1. 先判断正负。
2. 10位以内直接反转。
3. 接下来就是10位数字的反转，反转过程中判断是否会溢出。（不能反转后再判断是否溢出，因为环境只能存储32位有符号整数，所以反转之后的如果真的溢出是保存不了的）
### 解答

```
var max;
var flag;
var reverse = function(x) {
    x < 0 ? (flag = '-', max = '2147483648') : (flag = '+', max = '2147483647');
    var val = (Math.abs(x) + '').split('');
    if (val.length < 10) { return ((flag + val.reverse().join('')) - 0) }
    if (val.length = 10) {
        for (var i = 0; i < 10; i++) {
            if (val[9 - i] > max[i]) {
                return 0;
            } else if (val[9 - i] < max[i]) {
                return ((flag + val.reverse().join('')) - 0)
            }
        }
    }
};
```

> 题目很简单，肯定还是可以优化的，如果有更好的办法可以留言。