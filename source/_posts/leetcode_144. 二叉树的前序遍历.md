---
title: leetcode_144. 二叉树的前序遍历
date: 2019-10-17 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 144, 二叉树的前序遍历
---
### 题目解析：
给定一个二叉树，返回它的 前序 遍历。

### 示例	
示例 1:
```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [1,2,3]
```


说明&&进阶:
```
 递归算法很简单，你可以通过迭代算法完成吗？


 前序排列的顺序是父节点在前，然后遍历左树，然后遍历右树。
 
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
let frontArr = [];
let addNode = (root) => {
    frontArr.push(root.val)
    root.left && addNode(root.left);
    root.right && addNode(root.right);
}
let preorderTraversal = (root) => {
    frontArr = [];
    if (!root) {
        return frontArr;
    }
    addNode(root);
    return frontArr;
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

let frontArr = [], nodeList = [], nowNode;
let preorderTraversal = (root) => {
    frontArr = [];
    if (!root) {
        return frontArr;
    }
    nodeList = [root];
    while(nodeList.length > 0) {
        nowNode = nodeList.pop();
        frontArr.push(nowNode.val);
        nowNode.right && nodeList.push(nowNode.right);
        nowNode.left && nodeList.push(nowNode.left);        
    }
    return frontArr;
};
```

