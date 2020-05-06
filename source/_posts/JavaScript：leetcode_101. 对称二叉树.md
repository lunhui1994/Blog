---
title: leetcode_101. 对称二叉树
date: 2019-12-12 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 101, 对称二叉树，递归
---
### 题目解析：
给定一个二叉树，检查它是否是镜像对称的。

### 示例	
示例 1:
```
输入: [1,2,2,3,4,4,3]  对称
    1
   / \
  2   2
 / \ / \
3  4 4  3

输出: true
```
示例 2:
```
输入: [1,2,2,null,3,null,3] 非对称
    1
   / \
  2   2
   \   \
   3    3

输出: false
```

说明&&进阶:
```
如果你可以运用递归和迭代两种方法解决这个问题，会很加分。
 
```
<!-- more -->
### 解题思路

    递归，递归判断条件
    1. 左右对称的节点是否相同。
    2. 递归判断直至叶节点。

### 解答
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isSymmetric = function(root) {
    if (root === null) {
        return true;
    }
    return fun(root.left, root.right);
};

var fun = function(left, right) {
    if (left === null && right === null) {
        return true;
    }
    
    if (left === null || right === null) {
        return false;
    }
    
    return (left.val === right.val) && fun(left.left, right.right) && fun(left.right, right.left);
}
```
