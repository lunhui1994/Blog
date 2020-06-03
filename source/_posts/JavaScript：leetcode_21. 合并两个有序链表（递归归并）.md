---
title: JavaScript：leetcode_21. 合并两个有序链表（递归归并）
date: 2020-05-01 12:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 合并两个有序链表, 递归归并
---
### 题目说明
```
将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

```
<!-- more -->

### 解题思路一
其实就是归并排序的最后一步。合成两个有序序列。只不过用递归的方式去遍历链表。
1. 递归，把min(t1，t2),加入新链。
2. 若t1小，迭代t1 = t1.next，进入下一轮。
### 代码实现一
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
    let res = new ListNode(0);
    if (l1 && l2) {
        mergeL(l1,l2,res)
    } else {
        return l1 || l2
    }
    return res
};

function mergeL(l1, l2, res) {
    if (l1.val <= l2.val) {
        res.next = l1
        res.next.next = l2
    } else {
        res.next = l2
        res.next.next = l1
    }
    if (l1.next && l2.next) {
        mergeL(l1.next, l2.next, res.next.next)
    }
}
```


