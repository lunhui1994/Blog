---
title: leetcode 每日一题 旋转矩阵 （逆列 => 行）
date: 2020-04-07 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 旋转矩阵
---

### 题目描述
给你一幅由 N × N 矩阵表示的图像，其中每个像素的大小为 4 字节。请你设计一种算法，将图像旋转 90 度。

> 不占用额外内存空间能否做到？



示例 1:
 ```
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],
 ```
原地旋转输入矩阵，使其变为:
 ```
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
] 
```

### 解题思路

目标阵列的每一**横列**为对应原始阵列每一**竖列**的**倒序**。



###  题解一：
```

/**
 * @param {number[][]} matrix
 * @return {void} Do not return anything, modify matrix in-place instead.
 */
var rotate = function(matrix) {
    let N = matrix.length;
   	let newM = []
    for (let i = 0; i < N; i++) {
        newM[i] = Array.from(matrix[i]);
    }
    for (let i = 0; i < N; i++) {
        for(let j = 0; j < N; j++) {
            matrix[i][j] = newM [N - j - 1][i]; 
        }
    }
};
```



###  题解二：不创建新的数组空间
```

/**
 * @param {number[][]} matrix
 * @return {void} Do not return anything, modify matrix in-place instead.
 */
var rotate = function(matrix) {
    let N = matrix.length;
    for (let i = 0; i < N; i++) {
        for(let j = 0; j < N; j++) {
            matrix[i][j + N] = matrix[N - j - 1][i];
        }
    }
    for (let i = 0; i < N; i++) {
        matrix[i] = matrix[i].slice(N, 2*N);
    }
};
```

事实上这种方法是取巧了的，数组扩容了。数组检测到需要扩容时，就会开辟新的内存空间，最终大小为1.5倍+16。当数组内存空间>=length*2+16的时候会进行缩容。如果数组长度比之前缩短了1，则只回收多余容量的一半，若长度比之前缩小的更多，则全部回收多余容量。