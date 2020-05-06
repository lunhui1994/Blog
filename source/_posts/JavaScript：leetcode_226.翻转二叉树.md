---
title: leetcode_226. 翻转二叉树
date: 2020-03-26 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 226, 翻转二叉树
---

### 题目解析
翻转一棵二叉树。

### 示例	
示例 1:
```
输入:
     4
   /   \
  2     7
 / \   / \
1   3 6   9

输出: 
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

### 解题思路

    递归，递归判断条件
    1. 左右节点是否为null，不为null，则翻转其左右节点
<!-- more -->
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
 * @return {TreeNode}
 */

function ex(treeNode) {
    if (treeNode == null) {
        return null;
    }
    let item = treeNode.left;
    treeNode.left = treeNode.right;
    treeNode.right = item;
    treeNode.left && ex(treeNode.left)
    treeNode.right && ex(treeNode.right)
}
var invertTree = function(root) {
    ex(root)
    return root
};

```