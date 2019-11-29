---
title: leetcode_264. 丑数 II
date: 2019-10-12 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 264, 丑数 II
---
### 题目解析：
编写一个程序，找出第 n 个丑数。

丑数就是只包含质因数 2, 3, 5 的正整数。
### 示例	
示例 1:
```
    输入: n = 10
    输出: 12
    解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```
提示：
```
    1 是丑数。
    n 不超过1690。
```
<!-- more -->
### 解题思路

    方法一暴力循环（毫无疑问超时了）

### 解答
```
var isUgly = function(num) {
    if (num < 1) {
        return false;
    }
     while(num % 2 == 0) {
             num/=2
         } 
     while(num % 3 == 0) {
             num/=3
         } 
     while(num % 5 == 0) {
             num/=5
         }
    
    return (num == 1) ? true : false;
};
    var nthUglyNumber = function(n) {
    var ugly = 1;
    for (var i = 0; i < n; ugly++) {
        if (isUgly(ugly)) {
            i++;
        }
    }
    return ugly;
};
```

### 解题思路二

    1. 根据题目的意思，我们首先知道丑数的因子只能是(2, 3, 5).
    当我们要从[1],推算出[1,2,3,4,5,6,8,9...]丑数序列时,过程如下
    

    var arr = [1]; 
    推算第二个数：比较arr[0]*2 和 arr[0]*3 和arr[0]*5 中取最小的一个arr[0]*2
    放进数组中： [1, arr[0]*2]。
    依次类推下次比较：arr[1]*2 和 arr[0]*3 和 arr[0]*5 中取最小arr[0]*3
    放进数组中： [1, arr[0]*2, arr[0]*3]
    依次类推下次比较：arr[1]*2 和 arr[1]*3 和 arr[0]*5 中取最小arr[1]*2
    放进数组中： [1, arr[0]*2, arr[0]*3, arr[1]*2]
    等等。。


    2. 由上可以看出，我们需要丑数组arr, 还有2,3,5三个质因数分别乘到了arr的第几个数。
    拿2作例子：我们需要知道数组的前多少个已经乘过2了。当arr[0]*2 之后，
    下次就该arr[1]*2跟其他的作比较了。

    即这2，3，5需要三个标记。在上面举例中第三次之后的下标为：[2, 1, 0].
    即下次比较应该用arr[2]*2 和 arr[1]*3 和arr[0]*5 来比较哪个小。

    3. 中间会遇到比如 arr[2]*2 == arr[1]*3 这样的情况。
    此时把2，3 的下标都+1即可.

    最后依次求到目标数组arr的第n个数即为答案。
### 解答二
```
var nthUglyNumber = function(n) {
    var arr = [1], indexArr = [0, 0, 0],v2,v3,v5,temp;
    for(var i = 0; i <= n; i++) {
        v2 = arr[indexArr[0]] * 2;        
        v3 = arr[indexArr[1]] * 3;
        v5 = arr[indexArr[2]] * 5;
        temp = Math.min(v2,Math.min(v3,v5)); // 判断最小
        if (temp == v2) {
            indexArr[0]++;
        }
        if (temp == v3) {
            indexArr[1]++;
        }
        if (temp == v5) {
            indexArr[2]++;
        }
        arr.push(temp);
    }
    return arr[n-1];
};

```

