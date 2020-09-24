---
title: JavaScript：leetcode_9. 回文数（水题，三种方法）
date: 2019-09-26 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode_9, 回文数（水题，三种方法）
---
### 题目说明
```
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

示例 1:

输入: 121
输出: true
示例 2:

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
示例 3:

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
进阶:

你能不将整数转为字符串来解决这个问题吗？
```

### 解题思路一（字符串反转）
1. 第一反应数字转字符串，然后前后对比。可以解决问题。
	

### 代码实现一
```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    let xStr = x + '';
    let mid = +(xStr.length >> 1) + (+xStr.length % 2);
    for (let i = 0; i < mid; i++) {
        if (xStr.charAt(i) != xStr.charAt(xStr.length - i - 1)) {
            return false
        }
    }
    return true;
};
```

### 解题思路二（求得x的位数，xLen =  Math.log10(x))

1. 求得长度，接下来跟字符串一样了。
2. parseInt(x / Math.pow(10, xLen)) 得到首位
3. x % 10 得到尾位
4. 对比是否相等
	

### 代码实现二
```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    if(x < 0) {
        return false
    }
    let xLen = parseInt(Math.log10(x));
    while(x >= 1) {
        if(x % 10 != parseInt(x / Math.pow(10, xLen))) {
            return false
        }
        x = x % Math.pow(10, xLen);
        x = parseInt(x /= 10);
        xLen-=2;
    }
    return true
};
```
### 解题思路三（反转尾部)

1. 这个是官方题解，反转尾部，直到前半部分小于等于反转后的尾部
2. 将x的尾部移出来，反转。一直到x剩下的前半部分等于或者小于反转的尾部。
3. 前半部分小于尾部，说明到了x的中间位。
	1. 跳出循环时，回文数只有两种情况。
	2. 第一种，x为偶数，直接判断首尾是否相同。
	3. 第二种，x为奇数，尾部除以10，再进行判断。

	

### 代码实现三
```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    if(x < 0 || (x % 10 == 0 && x > 9)) {
        return false
    }
    let xTail = 0;
    let length = 0;
    while(x > xTail) {
        xTail *= 10;
        xTail += x % 10;
        x = parseInt(x / 10);
        length++;
    }
    if (x == xTail || x == parseInt(xTail / 10)) {
        return true
    } else {
        return false
    }
};
```