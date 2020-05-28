---
title: JavaScript：leetcode_69. x 的平方根
date: 2020-05-09 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, x 的平方根
---

### 题目说明
```
实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

示例 1:

输入: 4
输出: 2
示例 2:

输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。

```


### 解题思路一
1. 求平方根，一种是利用我们Math.sqrt()直接求，这种就不说了。
2. 然后说我们手动求的方式，最简单的方式，就是for循环遍历从1到x，求出x/i === i 那么这个i就是他的平方根。
3. 问题是你遇到非整平方根你可就求不出来了。
4. 所以再进行一次判断`(i * i > x && (i-1)*(i-1) < x)`判断x是否存在于这个范围内，如果再，取i-1，因为我们是向下取整的。
### 代码实现一
```javascript
/**
 * @param {number} x
 * @return {number}
 */
var mySqrt = function(x) {
    let length = (String(x).length >> 1) + 1;

    let max = ((new Array(length).fill((String(x)[0] - 0 >> 1) + 2).fill(0,1)).join('')) - 0;

    for(let i = max; i >= 0; i--) {
        if(i * i === x) {
            return i
        }
        if((i * i > x && (i-1)*(i-1) < x)) {
            return i - 1
        }
    }
    return 0
};
```
