---
title: leetcode_145. 二叉树的后序遍历
date: 2019-11-17 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 145, 二叉树的后序遍历
---
### 题目解析：
给定一个二叉树，返回它的 后序 遍历。

### 示例	
示例 1:
```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [3,2,1]
```


说明&&进阶:
```
 递归算法很简单，你可以通过迭代算法完成吗？


 前序排列的顺序是左，右，父。
 
```
<!-- more -->
### 解题思路


    递归，递归判断条件，该节点左右节点是否为null，递归时先左后右。
    跟前序遍历唯一的差别是最后再push。

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
 let postorderArr = [];
 let addNode = (root) => {
     root.left && addNode(root.left);
     root.right && addNode(root.right);
     postorderArr.push(root.val) // 唯一的差别
 }
 let postorderTraversal  = (root) => {
     postorderArr = [];
     if (!root) {
         return postorderArr;
    }
     addNode(root);
     return postorderArr;
 };
```

### 解题思路二

   迭代法，基于栈的特性将递推出来的节点压进栈中。然后遵循先进后出的原则，直至将栈排空。

   后序遍历的迭代法比较前序遍历要复杂一些。

   我的思路是打表，已经遍历过的节点需要标记（截断）。

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

let postorderArr = [], nodeList = [], nowNode;
let postorderTraversal  = (root) => {
    postorderArr = [];
    if (!root) {
        return postorderArr;
    }
    nodeList = [], nowNode = root;
     while(nodeList.length > 0 || nowNode) {
        while (nowNode) {
            nowNode.flag = true; // 标记该节点已经进入过数组
            nodeList.push(nowNode);
            
            //如果该节点的左节点已经遍历过了就不需要遍历了  先左
            
            if (nowNode.left && !nowNode.left.flag) { 
                nowNode = nowNode.left;
            
            //如果该节点的右节点已经遍历过了就不需要遍历了  后右

            } else if (nowNode.right && !nowNode.right.flag) { 
                nowNode = nowNode.right;
            } else {
            
                //左右节点都遍历过的相当于叶节点（度为0，没有子节点）
            
                nowNode = null; 
            }
        }
        // 从栈中取值
        nowNode = nodeList.pop(); 

        // 用来区分是否为叶节点 若为叶则赋值null，遍历下一轮。
        
        if (!nowNode.right || nowNode.right.flag) {
            postorderArr.push(nowNode.val);
            nowNode = null;
        }
    }
    return postorderArr;
};
```

