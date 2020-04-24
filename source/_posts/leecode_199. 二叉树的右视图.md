---
title: leecode_199. 二叉树的右视图(二叉树中右左遍历）
date: 2020-04-22 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 二叉树
---

### 题目描述
```
给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

示例:

输入: [1,2,3,null,5,null,4]
输出: [1, 3, 4]
解释:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---

```

### 解题思路

对二叉树进行**中右左**顺序遍历，以此顺序，记录每个层级第一个被遍历的节点。



###  题解一：
```javascript

/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
var rightSideView = function(root) {
    let resArr = [];
    let level = 0;
    if (!root) {return []}    
    nodeFn(root, 0, resArr)
    return resArr;
};

var nodeFn = function (node, level, resArr) {
    if (resArr[level] === undefined) {
        resArr[level] = node.val;
    }
    node.right && nodeFn(node.right, level + 1, resArr)
    node.left && nodeFn(node.left, level + 1, resArr)
}
```

相当于二叉树的后序遍历的反序（中右左），只不过需要标记一下每个层级的第一个遍历节点即可。