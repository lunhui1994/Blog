---
title: JavaScript：leetcode_236. 二叉树的最近公共祖先（1. 层序遍历 + 二叉树任意遍历方法 2. 二叉树遍历 + 动态规划）
date: 2020-05-10 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 二叉树的最近公共祖先, 二叉树遍历
---

### 题目说明
```
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，
满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200510171131762.png)
```

示例 1:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
示例 2:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
 

说明:

所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉树中。

```

<!-- more -->

### 解题思路一
1. `层序`遍历二叉树，每遍历到一个节点，就`收集`该节点在内的`所有子节点`。
2. 该节点`子节点集合`中是否同时`存在p,q`，如果`存在`，标记该节点，`flag为true`代表：该节点是p，q的一个`公共祖先`，然后依次遍历其`左右节点`的`子节点集合`。
3. 若不存在，说明其子节点的集合肯定也不存在，就中断递归，没必要再继续了。
4. 最终递归会在左右节点都不存在的情况下终止遍历。形成一个带有标记的树，每个节点上标记有`flag为true`的都是p，q的`公共祖先`
5. 最后再进行一次层序遍历，收集`flag为true`的节点，然后数组的末尾一位就是他们的最近公共祖先。

> 注意： 题目中的5，步骤也可以放在 2-3步骤中同时进行收集。
> 代码实现中，我使用了unshift(),所以输出的是第一位。
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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
// 注意是返回节点，不是返回节点的值！！！
var lowestCommonAncestor = function(root, p, q) {
    p = p.val;
    q = q.val;
    find(root, p, q);
    let help = [root]
    let res = [];
    while (help.length) {
        let now = help.shift();
        if(now.flag) {
            res.unshift(now);
            now.left && help.push(now.left);
            now.right && help.push(now.right);
        }
    }
    return res[0]
};

function find(root, p, q) {
    let help = [root]
    let res = [];
    while (help.length) {
        let now = help.shift();
        res.push(now.val);
        now.left && help.push(now.left);
        now.right && help.push(now.right);
    }
    if (res.indexOf(p) !== -1 && res.indexOf(q) !== -1) {
        root.flag = true;
        root.left && (find(root.left, p, q))
        root.right && (find(root.right, p, q))
    }
}
```
### 解题思路二
1. 收集所有的节点的祖先节点，类似于动态规划，`每个节点`的所有公共祖先都是`父节点所有公共祖先`的加上该`节点本身`。
2. 通过动态规划和递归进行收集。
3. 判断该节点是否是`p`或者`q`，`收集`到对象中。
4. 二叉树遍历完成后，将收集到的p，q所有的祖先节点进行遍历，`倒序遍历`到`第一个相同`的节点就是他们的`最近公共祖先`

> 这个方法在实际提交中，超内存了。。。但是思路应该是没毛病的。
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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
// 注意是返回节点，不是返回节点的值！！！
var lowestCommonAncestor = function(root, p, q) {
    p = p.val;
    q = q.val;
    let res = {
        p: [],
        q: []
    }
    find(root, p, q, [], res);
    for (let i = res.q.length; i >= 0; i--) {
        if (res.p.indexOf(res.q[i]) !== -1) {
            return res.q[i]
        }
    }
};

function find(root, p, q, prev, res) {
    prev = [...prev, root];
    if (root.val === p) {
       res.p = [...prev]
    }
    if (root.val === q) {
       res.q = [...prev]
    }
    root.left && (find(root.left, p, q, prev, res));
    root.right && (find(root.right, p, q, prev, res));
}
```