---
title: leetcode_94. 二叉树的中序遍历
date: 2019-10-17 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 94, 二叉树的中序遍历
---
### 题目解析：
给定一个二叉树，返回它的 中序 遍历。

### 示例	
示例 1:
```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [1,3,2]
```


说明&&进阶:
```
 递归算法很简单，你可以通过迭代算法完成吗？


 前序排列的顺序是左，父，右。
 
```
<!-- more -->
### 解题思路


    递归，递归判断条件，该节点左右节点是否为null，递归时先左后右。

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
 * @return {number[]}
 */
var addNode = function (root) {
    root.left && addNode(root.left);
    inorderArr.push(root.val); // 前，中序遍历唯一区别 
    root.right && addNode(root.right);
}
let inorderArr = [];
var inorderTraversal = function(root) {
    inorderArr = [];
    if (!root) {
        return inorderArr;
    }
    addNode(root);
    return inorderArr
};
```

### 解题思路二

   迭代法，基于栈的特性将递推出来的节点压进栈中。然后遵循先进后出的原则，直至将栈排空。

### 解答二
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
 * @return {number[]}
 */
let inorderArr = [], nodeList = [], nowNode;
var inorderTraversal = function(root) {
    inorderArr = [], nodeList = [], nowNode = root;
    if (!root) {
        return inorderArr;
    }
    
    while(nodeList.length > 0 || nowNode) {
        while (nowNode) {
            nodeList.push(nowNode);
            nowNode = nowNode.left;
        }
        nowNode = nodeList.pop();
        inorderArr.push(nowNode.val);
        nowNode = nowNode.right;
    }
    
    return inorderArr
};
```

