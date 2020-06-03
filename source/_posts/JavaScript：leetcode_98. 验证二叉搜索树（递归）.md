---
title: JavaScript：leetcode_98. 验证二叉搜索树（递归）
date: 2020-05-05 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 验证二叉搜索树, 递归
---

### 题目说明
```

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。
示例 1:

输入:
    2
   / \
  1   3
输出: true
示例 2:

输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。

```
<!-- more -->

### 解题思路一
解决树的问题，最常用的还是递归的方法。
此题的关键就是找到node.val的取值范围。

其左右树的取值范围是以`根节点为分界线`，`左集合`为`左树取值范围`，`右集合`为`右树取值范围。`

如下 左树取值范围是`[-∞，4]`,右树取值范围是`[6，+∞]`，以此类推。
```
    5
   / \
  1   8
     / \
    6   9
```
1. 以此为例，因为5为根节点，所以，他的取值范围 是`[-∞，+∞]`，也就是没有限制
2. 然后观察左树，左树根节点为1，符合题目要求，要小于其根节点，也就是说其取值范围是`[-∞，4]`
3. 然后观察右树，右树根节点为4，符合题目要求，要大于其根节点，也就是说其取值范围是`[6，+∞]`
4. 观察右树左节点，按要求，它要小于右树根节点（8），并且大于树根节点（5），所以其取值范围是`[6，7]`，同理其右树右节点范围是`[9，+∞]`。
5. 递归...

了解了以上步骤，那么接下来再看代码实现可能会比较清晰。



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
 * @param {TreeNode} root
 * @return {boolean}
 */
 //Number.MIN_SAFE_INTEGER  代表 -∞
 //Number.MAX_SAFE_INTEGER  代表 +∞
let flag
let isValidBST = function(root) {
    let flag = true;
    root && findTree(root, Number.MIN_SAFE_INTEGER, Number.MAX_SAFE_INTEGER);
    return flag
};
function findTree(node, min, max) {
    if (node.val <= min || node.val >= max) {
        return flag = false
    }
    node.left && findTree(node.left, min, node.val) //递归时，传入其左树总的取值范围
    node.right && findTree(node.right, node.val, max)//递归时，传入其右树总的取值范围
}
```