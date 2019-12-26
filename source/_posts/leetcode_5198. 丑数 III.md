---
title: leetcode_5198. 丑数 III
date: 2019-09-26 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode_5198, 丑数 III
---
### 题目解析：
请你帮忙设计一个程序，用来找出第 n 个丑数。

丑数是可以被 a 或 b 或 c 整除的 正整数。
### 示例	
示例 1:
```
    输入：n = 3, a = 2, b = 3, c = 5
    输出：4
    解释：丑数序列为 2, 3, 4, 5, 6, 8, 9, 10... 其中第 3 个是 4。
```
示例 2:
```
    输入：n = 4, a = 2, b = 3, c = 4
    输出：6
    解释：丑数序列为 2, 3, 4, 6, 8, 9, 12... 其中第 4 个是 6。
```
示例 3:
```
    输入：n = 5, a = 2, b = 11, c = 13
    输出：10
    解释：丑数序列为 2, 4, 6, 8, 10, 11, 12, 13... 其中第 5 个是 10。

```
示例 4:
```
    输入：n = 1000000000, a = 2, b = 217983653, c = 336916467
    输出：1999999984
```
提示：
```
    1 <= n, a, b, c <= 10^9
    1 <= a * b * c <= 10^18
    本题结果在 [1, 2 * 10^9] 的范围内
```
<!-- more -->
### 解题思路

    方法一暴力循环（毫无疑问超时了）

### 解答
```
    var nthUglyNumber = function(n, a, b, c) {
    var num = 1;
    for (var i = 1; i <= n; num++) {
        if (num % a == 0 || num % b == 0 || num % c == 0) {
            i++;
        }
    }
    return num - 1;
};
```

### 解题思路二

    1.找出该数可能存在的范围，首先可以思考判断得出最小的值为n ，最大为min(a,b,c) * n
    2.假设最终结果为finalValue，求出该值的序列长度nums。
    例如：下面的例子中finalValue为6，而其序列就为[2, 3, 4, 6],长度为4===n。
    ```
        输入：n = 4, a = 2, b = 3, c = 4
        输出：6
        解释：丑数序列为 2, 3, 4, 6, 8, 9, 12... 其中第 4 个是 6。
    ```
    3.通过二分法查找finalValue
    4.判断条件为: nums.length === n ? ，如果相等即为该值。
    ** 注意：finalValue必须符合是a，b，c的倍数，且是所有符合条件的值中最小的一个。 **
### 解答二
```
//最大公约数 ：a和 a%b 的最大公约数 和 a 和 b 的最大公约数一致
var maxComFn = function(a, b) {
    if (a % b === 0) {
        return b;
    } else {
        return maxComFn(b, a % b);
    }
}
//最小公倍数  公式法： a*b === a和b的最大公约数 * a和b的最小公倍数
var minComFn = function(a, b) {
    var maxC = maxComFn(a, b)
    return a * b / maxC;
}
//值的序列长度  容斥定理， 含有a，b，c的个数 - ab，ac，bc的公倍数的个数 + abc公倍数的个数
var inNumsFn = function(a, b, c, num) {
    return Math.floor(num / a) + 
        Math.floor(num / b) +
        Math.floor(num / c) -
        Math.floor(num / minComFn(a, b)) -
        Math.floor(num / minComFn(a, c)) -
        Math.floor(num / minComFn(c, b)) +
        Math.floor(num / minComFn(c, minComFn(a, b)));
}

var nthUglyNumber = function(n, a, b, c) {
    var maxCom = maxComFn(a, maxComFn(b, c));
    let mid,
        left = n,
        right = n * Math.min(a, b, c);
    while (left < right) {
        mid = Math.floor((left + right) / 2);
        if (inNumsFn(a, b, c, mid) < n) {
            left = mid + 1;
        } else {
            right = mid; //一直求到最小。
        }
    }
    return right;
};

```

