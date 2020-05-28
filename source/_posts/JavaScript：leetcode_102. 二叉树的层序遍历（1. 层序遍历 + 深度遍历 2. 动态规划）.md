---
title: JavaScript：leetcode_102. 二叉树的层序遍历（1. 层序遍历 + 深度遍历 2. 动态规划）
date: 2020-05-14 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 二叉树的层序遍历, 层序遍历, 深度遍历, 动态规划
---

### 题目说明
```

给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

 

示例：
二叉树：[3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]

```
### 说明
该题对于我来说一共有两种思路，四种方案。

### 解题思路一 （层序+深度）
1. 该题如果去掉分组，就是一个层序遍历的问题。加上分组也不过是多深度遍历一遍

### 代码实现一 (1)
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
 * @return {number[][]}
 */
 var levelOrder = function(root) {
     if (!root) {
         return []
     }
     let res = [];
     function deepNode(root, node) {
         node.deep = root.deep + 1;
         node.left && deepNode(node, node.left);
         node.right && deepNode(node, node.right);
     }
     deepNode({deep: -1}, root);
     let help = [root]
     while(help.length) {
         let node = help.shift();
         node.right && help.unshift(node.right);
         node.left && help.unshift(node.left);
         if (!res[node.deep]) {
             res[node.deep] = [];
         }
         res[node.deep].push(node.val);
     }
     return res;
 };
```
### 代码实现一 (2) 

可以看到实现方式二去掉了while遍历，使用了一组数组。因为数组的顺序是`前序`遍历的结果，所以标记过每个节点的层级之后，我们其实按顺序将它分别放到二维数组中就可以了。
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
 * @return {number[][]}
 */
var levelOrder = function(root) {
    if (!root) {
        return []
    }
    let res = [];
    let nodeList = [];
    function deepNode(root, node, nodeList) {
        node.deep = root.deep + 1;
        nodeList.push({
            val: node.val,
            deep: node.deep
        })
        node.left && deepNode(node, node.left, nodeList);
        node.right && deepNode(node, node.right, nodeList);
    }
    deepNode({deep: -1}, root, nodeList);
    for (let i = 0; i < nodeList.length; i++) {
        if (!res[nodeList[i].deep]) {
            res[nodeList[i].deep] = [];
        }
        res[nodeList[i].deep].push(nodeList[i].val);
    }
    return res;
};
```


### 解题思路二 （递归 + 动态规划）
1. 首先我们可以这么想：根节点属于数组的第一层。
2. 那么第二层该如何得到呢，其实就是按顺序遍历第一层所有节点的左右节点。
3. 第三层就是遍历第二层的所有左右节点。
4. 按照这样理解，这个题就更加清晰了。 
5. 状态转移的方式是将`当前层`的所有`子节点`放入`下一层`。

### 代码实现二 (1)
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
 * @return {number[][]}
 */
 var levelOrder = function(root) {
     if (!root) {
         return []
     }
     let nodeList = [[root]];
     let res = [[root.val]];
     function deepNode(nodeList, row, res) {
         nodeList[row + 1] = [];
         res[row + 1] = [];
         for (let i = 0; i < nodeList[row].length; i++) {
             nodeList[row][i].left && (nodeList[row + 1].push(nodeList[row][i].left), res[row+1].push(nodeList[row][i].left.val));
             nodeList[row][i].right && (nodeList[row + 1].push(nodeList[row][i].right), res[row+1].push(nodeList[row][i].right.val));
         }
         if (nodeList[row + 1].length) {
             deepNode(nodeList, row + 1, res)
         }
     }
     deepNode(nodeList, 0, res);
     res.length -= 1;
     return res;
 };
```
### 代码实现二 (2) 

去掉了递归，使用了for循环。
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
 * @return {number[][]}
 */
var levelOrder = function(root) {
    if (!root) {
        return []
    }
    let nodeList = [[root]];
    let res = [[root.val]];
    for (let i = 0; ; i++) {
        nodeList[i + 1] = [];
        res[i + 1] = [];
        for (let j = 0; j < nodeList[i].length; j++) {
            nodeList[i][j].left && (nodeList[i + 1].push(nodeList[i][j].left), res[i + 1].push(nodeList[i][j].left.val));
            nodeList[i][j].right && (nodeList[i + 1].push(nodeList[i][j].right), res[i + 1].push(nodeList[i][j].right.val));
        }
        if (!nodeList[i + 1].length) {
            break;
        }
    }
    res.length -= 1;
    return res;
};
```