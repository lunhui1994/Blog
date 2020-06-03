---
title: JavaScript：leetcode_572. 另一个树的子树（广度遍历+深度遍历）
date: 2020-05-07 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 另一个树的子树, 广度遍历+深度遍历
---

### 题目说明
```
给定两个非空二叉树 s 和 t，检验 s 中是否包含和 t 具有相同结构和节点值的子树。
s 的一个子树包括 s 的一个节点和这个节点的所有子孙。s 也可以看做它自身的一棵子树。

示例 1:
给定的树 s:

     3
    / \
   4   5
  / \
 1   2
给定的树 t：

   4 
  / \
 1   2
返回 true，因为 t 与 s 的一个子树拥有相同的结构和节点值。

示例 2:
给定的树 s：

     3
    / \
   4   5
  / \
 1   2
    /
   0
给定的树 t：

   4
  / \
 1   2
返回 false。
```
<!-- more -->

### 说明
解题之间看清除问题中的两个例子，什么是子树。
树中的某节点及其`所有子节点`组成的树，叫子树。不可以漏掉一个的。所以看例2，是返回false的哦。
### 解题思路一
广度优先找子树根，深度优先对比s，t。我这种思路可能稍麻烦些，但是好在容易理解。符合人脑回路。

1. 广度优先遍历S树。依赖队列实现（`push`和`shift`配合实现队列先进先出的特点）
2. 直到S树某节点的val值和T树的根节点val值相同时。`(s.val === t.val)`
3. 深度遍历做对比。使用`递归`

还有一种思路是依赖`JSON.stingify`将对象转换成字符串，再判断字符串之间是否包含。投机取巧不太可取就不做展示了。

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
 * @param {TreeNode} s
 * @param {TreeNode} t
 * @return {boolean}
 */
var isSubtree = function(s, t) {
    let sArr = [];
    let tArr = [];
    let dp = [s];
    let flag = 'default';
    function frontTree(s, t) {
    	//这个判断有点多，哈哈
        if (t === null || s === null || s.val !== t.val) {
            return (flag = false)
        }
        if ((s.left && !t.left) || (!s.left && t.left)) {
            return (flag = false)
        }
        if ((s.right && !t.right) || (!s.right && t.right)) {
            return (flag = false)
        }
        if (s.left && t.left) {
            frontTree(s.left, t.left);
        }
        if (s.right && t.right) {
            frontTree(s.right, t.right);
        }
    }   
    while (dp.length) {
        let s = dp.shift();
        if(s.val === t.val) {
           flag = true; //开始深度对比，默认为true
           frontTree(s, t);//如果不匹配，flag会设置为false
           if (flag) { //如果匹配，返回true, 如果不匹配，继续往下找，一直到最后。
               return true
           }
        }
        s.left && dp.push(s.left)
        s.right && dp.push(s.right)
    }
    // 若flag为default,说明没有找到和t根节点相同的节点，返回false
    return flag === 'default' ? false : flag
};
```