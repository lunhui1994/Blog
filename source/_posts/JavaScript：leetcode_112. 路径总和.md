---
title: leetcode_112. 路径总和
date: 2019-12-12 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 112, 路径总和, 递归
---
### 题目解析：
给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

### 示例	
示例 1:
```
给定如下二叉树，以及目标和 sum = 22
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1

返回 true, 因为存在目标和为 22 的根节点到叶子节点的路径 5->4->11->2。
```

说明&&进阶:
```
说明: 叶子节点是指没有子节点的节点。
 
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
 * @param {number} sum
 * @return {boolean}
 */
var hasPathSum = function(root, sum) {
    if (root === null) {
        return false;
    }
    return fun(root, 0, sum);
};

var fun = function(root, mysum, sum) {
    root.mysum = root.val + mysum;
    if (root.left === null && root.right === null){
       return root.mysum === sum;
    }
    return (root.left && fun(root.left, root.mysum, sum)) || (root.right && fun(root.right, root.mysum, sum)) || false; 
    // 或（||）会取最后一个转义为false的值。 即可能会出现0，null，undefined等结果
    // 为避免不必要的错误，增加 || false;
}
```
