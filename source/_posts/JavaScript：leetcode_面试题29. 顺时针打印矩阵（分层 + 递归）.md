---
title: JavaScript：leetcode_面试题29. 顺时针打印矩阵（分层 + 递归）
date: 2020-06-05 10:11:42
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode_面试题29, 顺时针打印矩阵（分层 + 递归）
---
### 题目说明
```
输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

 

示例 1：

输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
示例 2：

输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]

限制：

0 <= matrix.length <= 100
0 <= matrix[i].length <= 100
注意：本题与主站 54 题相同：https://leetcode-cn.com/problems/spiral-matrix/
```

### 解题思路一（分层+递归）
1. 首先明白题意,题目给出的示例数组是平铺的,不是特别清晰,可能短时间反应不过来它是想干嘛,我们先转换一下.
	

```clike
 [
 [1,2,3],
 [4,5,6],
 [7,8,9]
 ]
 1 -> 2 -> 3 -> 6 -> 9 -> 8 -> 7 -> 4 -> 5

 矩形外围开始,顺时针输出.
```
2. 因为是从外层开始顺时针的,所以我们先看最外层的我们如何得到.
	1. 首先最外层相当于一个正方形的四条边.(我们将顶点放入上下两条边)
	2. `top: [2]`, 加入左上和右上两个顶点为`[1, 2, 3]`
	3. `right: [6]`
	4. `bottom: [8]`, 加入左下和右下两个顶点为 `[7, 8, 9]`
	5. `left: [4]`
	6. 结果`[1,2,3,6,9,8,7,4,5]` 其中`left, bottom` 需要反转过来.(因为顺时针)
3. 剥离之后,原来的矩形只剩下了`[[5]]`, 假若我们矩形是多层的, 那么剩下来的仍然是个矩形,那么我们就可以将剩下的矩阵重新传入, 重复上面第二步求出`top right bottom left` 
### 代码实现一
```javascript
/**
 * @param {number[][]} matrix
 * @return {number[]}
 */
var spiralOrder = function(matrix) {
    if (matrix.length <= 1) {
        return matrix[0] || [];
    }
    if (matrix[0].length <= 1) {
        return matrix.reduce(function(sum, item) {
            return [...sum, ...item];
        }, [])
    }
    let top = matrix.shift();
    let right = [], left = [];
    for (let i = 0; i < matrix.length; i++) {
        right.push(matrix[i].pop());
        left.unshift(matrix[i].shift());
    }
    let bottom = matrix.pop().reverse();
    let res = [...top, ...right, ...bottom, ...left];
    return res.concat(spiralOrder(matrix));
};
```

