---
title: JavaScript：leetcode_974. 和可被 K 整除的子数组（前序和 + 同余定理）
date: 2020-05-27 22:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 和可被 K 整除的子数组
---

### 题目说明
```
给定一个整数数组 A，返回其中元素之和可被 K 整除的（连续、非空）子数组的数目。

 

示例：

输入：A = [4,5,0,-2,-3,1], K = 5
输出：7
解释：
有 7 个子数组满足其元素之和可被 K = 5 整除：
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]
 

提示：

1 <= A.length <= 30000
-10000 <= A[i] <= 10000
2 <= K <= 10000
```
<!-- more -->

### 解题思路一(暴力枚举，O（n^3）)
1. 暴力枚举所有的前序和，判断`对K取模`是否为`0`，为`0`则结果`+1`
### 代码实现一
```javascript
/**
 * @param {number[]} A
 * @param {number} K
 * @return {number}
 */
var subarraysDivByK = function(A, K) {
    let res = 0;
    for (let i = 0; i < A.length; i++) {
        let prev = 0;
        for (let j = i; j >= 0; j--) {
            prev += A[j];
            if (prev % K === 0) {
                res++
            }
        }
    }
    return res;
};
```
### 解题思路二(暴力枚举优化,O（n^2）)
1. 将两层for循环中的求前序和操作，提前求。
2. 那么我们求i之前的所有前序和就变成了，求`p[i] - p[j]` (j的范围是 `0 ~ i-1)`
3. 判断`p[i] - p[j]`对K去模是否为0，为0则结果+1
### 代码实现二
```javascript
/**
 * @param {number[]} A
 * @param {number} K
 * @return {number}
 */
var subarraysDivByK = function(A, K) {
    let res = 0;
    let p = new Array(A.length);
    for (let i = 0; i < A.length; i++) {
        p[i] = A.slice(0, i+1).reduce((sum, item, key, arr) => {
            return sum += item;
        }, 0);
        for( let j = i - 1; j >= 0; j--) {
            if((p[i] - p[j]) % K === 0) {
                res++
            }
        }
        if(p[i] % K === 0) {
            res++
        }
    }
    return res;
};
```

### 解题思路三(同余定理，O（n）)
> 先理解一个数学问题，  假设`a = 8，b = 13`, 同时mod `5`，那么 `a % 5 == 3，b % 5 == 3，即a % 5 ==  b % 5`则`(b - a) % 5 == 0`，`即对同一数取模相同的两个值，其差值可整除该数。`
1. 将两层for循环中的求前序和操作，提前求前序和序列p。
2. 得到所有的前序和p之后，理解说明若 `p[i] % K  ===  p[j] % K` 则`p[i] - p[j] % 5 === 0`那么`j`到`i`就是我们求的一个目标子序列。
3. 所以我们建立一个`hash`，用来存储p序列`取模之后`的值。 `hash`的键值范围是`（0 ~ K -1）`因为是对K取余，所以值只可能出现在该范围中。
4. 由于该hash的标记跟数组下标正好对应，所以hash就声明为一个数组。
5. 以示例为例
		1. 输入：`A = [4,5,0,-2,-3,1], K = 5`
		2. p序列为  `[4, 9, 9, 7, 4, 5]` 取模之后的序列为`[4, 4, 4, 2, 4, 0]` 记录到hash中
		3. hash = `[1, 0, 1, 0, 4]` 
		4. 接下来就是排列组合的问题了，将hash列表中 `> 1` 的值进行计算 `n * ( n - 1 ) / 2` 取和
		5. 最后再加上`hash[0]`的个数，因为`hash[0]`标记的是取模之后为`0`的值的个数，本身就属于目标子序列。
6. 第五步我们是先求出hash表才计算个数，我们也可以在完善hash的同时计算。
	1. 比如p序列为  `[4, 9, 9, 7, 4, 5]` 去模的过程中统计。
	2. 计算第`1`个取模，模值为4，`res += hash[4],` `hash[4]++,`由于`4`是第`1`次出现所以目前`res+=0`
	3. 计算第`2`个取模，模值为4，`res += hash[4],` `hash[4]++,`由于`4`是第`2`次出现所以目前`res+=1`，子序列下标范围是`[0,1]`
	4. 计算第3个取模，模值为4，`res += hash[4],` `hash[4]++,`由于`4`是第`3`次出现所以目前`res+=2`，子序列下标范围是`[0,1,2]，[1,2]`因为4出现3次，所以第3个4可以和前两个组合。
	5. 依次类推。
### 代码实现三
```javascript
/**
 * @param {number[]} A
 * @param {number} K
 * @return {number}
 */
var subarraysDivByK = function(A, K) {
    let hash = new Array(K).fill(0);
    let sum = 0;
    let res = 0;
    for (let i = 0; i < A.length; i++) {
        sum += A[i];
        let key = sum % K;
        key = key < 0 ? (key + K) : key; //处理负数的情况, (3 - (-2)) % 5 === 0
        res += hash[key];
        hash[key]++;
    }
    return res + hash [0];
};
```