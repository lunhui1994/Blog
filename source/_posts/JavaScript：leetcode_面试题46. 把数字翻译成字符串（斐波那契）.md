---
title: JavaScript：leetcode_面试题46. 把数字翻译成字符串（斐波那契）
date: 2020-06-09 10:49:48
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode_面试题46, 把数字翻译成字符串（斐波那契）
---
### 题目说明
```
给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

 

示例 1:

输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
 

提示：

0 <= num < 231
```

### 解题思路一（斐波那契数列）
1. 我一般做题前喜欢先测试几个用例, 测试完发现这就是斐波那契数列
2. 该题主要判断 `i-1, i` 两位组合成的数字是否在`(10, 25)`闭区间内. 如果在就是斐波那契`f(n) = f(n-1) + f(n-2)`, 否则跟上一种情况一致`f(n) = f(n-1)`.
	

### 代码实现一
```javascript
/**
 * @param {number} num
 * @return {number}
 */
var translateNum = function(num) {
    let numStr = num + '';
    let res;
    if (+(numStr[0] + numStr[1]) < 26 && +numStr[0] > 0) {
        res = [1, 2];
    } else {
        res = [1, 1];
    }
    for(let i = 2; i < numStr.length; i++) {
        if (+(numStr[i - 1] + numStr[i]) < 26 && +numStr[i - 1] > 0) {
            res[i] = res[i - 1] + res[i - 2];
        } else {
            res[i] = res[i - 1];
        }
    }
    return res[res.length-1]
};
```

