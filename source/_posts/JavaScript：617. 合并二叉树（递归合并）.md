---
title: JavaScript：617. 合并二叉树（递归合并）
date: 2020-04-30 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 合并二叉树, 递归合并
---

### 题目说明
```
给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

示例 1:

输入: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
输出: 
合并后的树:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
注意: 合并必须从两个树的根节点开始。
```
<!-- more -->

### 解题思路一
1. 又是一道二叉树的题，题目要求合并二叉树，即相同位置的val值相加，不同位置的val值互相替换。
2. 一般思路是创建一个新树，去取两树之和。
3. 但是题目没有要求不能改变原来的两棵树，那么我们以t1树为基准，观察t2树。
4. 若t1树对应t2树的位置都有值，则相加，若t1无，t2有，则t2替换t1节点，若t1有，t2无则无需操作。
### 代码实现一
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} t1
 * @param {TreeNode} t2
 * @return {TreeNode}
 */
let mergeTrees = (t1, t2) => {
    if (t1 && t2) {
        mergeNode(t1,t2) 
    }
    return t1 || t2;
};

let mergeNode = (t1, t2, root) => {
    t1.val += t2.val;
    if (t1.left === null) {
        t1.left = t2.left
    } else if( t2.left !== null) {
         mergeNode(t1.left,t2.left)
    }

    if (t1.right === null) {
        t1.right = t2.right
    } else if (t2.right !== null) {
        mergeNode(t1.right,t2.right)
    }
}
```

同样的我们使用递归的方法去遍历。

