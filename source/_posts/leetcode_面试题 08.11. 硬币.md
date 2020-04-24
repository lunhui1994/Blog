---
title: leetcode_面试题 08.11. 硬币(数学方法，双百分)
date: 2020-04-23 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 盛最多水的容器
---

### 题目描述
```

硬币。给定数量不限的硬币，币值为25分、10分、5分和1分，编写代码计算n分有几种表示法。(结果可能会很大，你需要将结果模上1000000007)

示例1:

 输入: n = 5
 输出：2
 解释: 有两种方式可以凑成总金额:
	5=5
	5=1+1+1+1+1
	示例2:

 输入: n = 10
 输出：4
 解释: 有四种方式可以凑成总金额:
	10=10
	10=5+5
	10=5+1+1+1+1+1
	10=1+1+1+1+1+1+1+1+1+1
说明：

注意:

你可以假设：

0 <= n (总金额) <= 1000000

```

### 解题思路

在这我只说一个硬刚的办法。没有算法，按照正常人类观察的思路：
1. 一共存在n/25个25分硬币。
2. 求当存在1个25分硬币时，剩余部分由多少个10分硬币组成。
 	1. 在2的条件下，求1个10分硬币之外的部分由多少个5分硬币组成,有几个就加几个，再加上全1分的情况。
3. 求当存在1个25分硬币时，剩余部分由多少个5分硬币组成。有几个就加几个，再加上全1分的情况。

至此我们求出了，组合中含有25分硬币的所有方法。然后按照上面的方法，求只存在10，5，1的方法，最后求只存在5，1的方法。其和即为我们所求的所有组合个数


###  题解一：
```javascript

/**
 * @param {number} n
 * @return {number}
 */
var waysToChange = function(n) {
    let res = 0;
    //求 25分 10分 5分 1分组合情况
    for ( let i = 1; i <= Math.floor(n / 25); i++) {
       for (let j = 1; j <= Math.floor(( n - 25 * i) / 10) ; j++) {
           for (let k = 0; k <= Math.floor((n - 25 * i - 10*j) / 5); k++) {
               res++;
           }
       }
       for (let l = 0; l <= Math.floor(( n - 25 * i) / 5) ; l++) {
            res++
       }
    }
    //求 10分 5分 1分组合情况
    for (let j = 1; j <= Math.floor(n / 10) ; j++) {
        for (let k = 0; k <= Math.floor((n - 10*j) / 5); k++) {
            res++;
        }
    }
    //求 5分 1分组合情况
    for (let k = 0; k <= Math.floor(n / 5); k++) {
        res++;
    }
    return res % 1000000007;
};
```
这个办法，相当的好理解吧。然鹅！！！超时咯~~~

那我就不会优化优化吗~~

1. 我们可以看出来求10，5，1 的情况和求5，1的情况，实际上是求当25分的个数为0时的情况。那我们合并一下，将i的值从0开始循环，那么就可以合并起来啦

#### 结果：
```javascript

/**
 * @param {number} n
 * @return {number}
 */
var waysToChange = function(n) {
    let res = 0;
    //求 25分 10分 5分 1分组合情况
    for ( let i = 0; i <= Math.floor(n / 25); i++) {
       for (let j = 1; j <= Math.floor(( n - 25 * i) / 10) ; j++) {
           for (let k = 0; k <= Math.floor((n - 25 * i - 10*j) / 5); k++) {
               res++;
           }
       }
       for (let l = 0; l <= Math.floor(( n - 25 * i) / 5) ; l++) {
            res++
       }
    }
  
    return res % 1000000007;
};
```

2. 再来看,这是求5，1分的。 
```
 for (let l = 0; l <= Math.floor(( n - 25 * i) / 5) ; l++) {
   	res++
   }
```
其实是求i = 0 到 Math.floor(( n - 25 * i) / 5) 的个数吧，那就是Math.floor(( n - 25 * i) / 5)+1个。

```
	let rest = Math.floor( n - 25 * i) ;
	res+= Math.floor(rest / 5) + 1;
```

3. 同理我们把10，5，1 的for循环也提取一下
```
 for (let j = 1; j <= Math.floor(( n - 25 * i) / 10) ; j++) {
           for (let k = 0; k <= Math.floor((n - 25 * i - 10*j) / 5); k++) {
               res++;
           }
       }

// 先按照2中的方法把内循环优化一下
	let rest = Math.floor( n - 25 * i) ;
	let rest10 = Math.floor(rest / 10);
 	for (let j = 1; j <= rest10 ; j++) {
           res+= Math.floor(rest / 5) + 1 - 2j;
       }
```
=> 到这儿应该很清楚怎么优化了吧 (Math.floor(rest / 5) + 1)*rest10 - (2 + 2 * 2 + 2 * 3+....+2 * rest10)
=> (Math.floor(rest / 5) + 1)*rest10 - (2+2*rest10)/2 再提取一下公因式。
```
	let rest = Math.floor( n - 25 * i) ;
	let rest10 = Math.floor(rest / 10);
	
    res += rest10 * (Math.floor(rest / 5) - rest10)

```
###  题解二：
```javascript
/**
 * @param {number} n
 * @return {number}
 */

var waysToChange = function(n) {
    let res = 0;
    let length25 = Math.floor(n / 25);
     for ( let i = 0; i <= length25; i++) {
        let rest = Math.floor(( n - 25 * i));
        let rest10 = Math.floor(rest / 10);

        res += rest10 * (Math.floor(rest / 5) - rest10)

        res += Math.floor(rest / 5) + 1

    return res % 1000000007;
};
```
就这样完美解决了，并且时间和空间都秒杀了100%