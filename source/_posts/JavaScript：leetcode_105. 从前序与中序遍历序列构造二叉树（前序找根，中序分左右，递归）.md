---
title: JavaScript：leetcode_105. 从前序与中序遍历序列构造二叉树（前序找根，中序分左右，递归）
date: 2020-05-22 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 从前序与中序遍历序列构造二叉树
---

### 题目说明
```
根据一棵树的前序遍历与中序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：

    3
   / \
  9  20
    /  \
   15   7

```

### 解题思路一
1. `前序找根`，`中序分左右`，`递归`即可。
2. 根为前序第一个值。`let root = new TreeNode(preorder[0]);`
3. 找到根在中序中的位置`let rootIndex = inorder.indexOf(root.val);`
4. 左右分开。left为左中序，right为右中序，preLeft为左前序，preRight为右
```javascript
 		let left = inorder.slice(0, rootIndex);
    	let right = inorder.slice(rootIndex + 1, inorder.length);
   	 	let preLeft = preorder.slice(1, left.length + 1);
    	let preRight = preorder.slice(left.length + 1);
```
5. 找到左右各自的前中序列。即可递归找根了。
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
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */

var buildTree = function buildTree(preorder, inorder) {
    if (preorder.length == 0) {
        return null;
    }
    let root = new TreeNode(preorder[0]);
    if (preorder.length == 1) {
        return root;
    }
    let rootIndex = inorder.indexOf(root.val);
    let left = inorder.slice(0, rootIndex);
    let right = inorder.slice(rootIndex + 1, inorder.length);
    let preLeft = preorder.slice(1, left.length + 1);
    let preRight = preorder.slice(left.length + 1);
    left.length && (root.left = buildTree(preLeft, left));
    right.length && (root.right = buildTree(preRight, right));
    return root;
}
```
