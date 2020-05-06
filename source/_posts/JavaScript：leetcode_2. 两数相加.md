---
title: leetcode_2. 两数相加
date: 2019-06-27 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 两数相加
---
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：
```
    输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
    输出：7 -> 0 -> 8
    原因：342 + 465 = 807
```
<!-- more -->
题目解析：
	该题给出两个链表，求出两个链表各个结点的和，那首先我们需要知道链表是什么样子。
	

```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
```
如上，链表的每个结点就是这个样子，next指向下一个结点，当然这是在js中简单的实现了链表。

然后知道了链表，那么就开始各位相加吧~，这个题唯一的难点就是在于各个结点相加的时候可能会产生进位。
比如9+2=11，那么就需要进一位，本位取10的余数为值，然后我们就需要在下一个结点相加时把前两位可能产生的进位也算进去。

如下是我的js解答：

```
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
 * y 为进位值
 * i 代表和的位
 * n 代表了和的每一位的值
 * arr 代表和（我们用数组表示更方便一点）
 */
var addTwoNumbers = function(l1, l2) {
    var arr = [], i = 0, n = 0, y = 0;   //初始化
    
    //因为链表不一定同样长的，所以只要有一个链表的结点不为空，我们都需要继续计算；
    //同时我们也要考虑进位，即使两个链表结束了，如果有进位的话，那我们还是需要再计算一次的。
    
    while (l1 !==  null || l2 !== null || y !== 0) {  

        //判断链表l1的该结点是否为null，如果为null初始化成值为0的结点，方便运算。
        l1 = l1 ? l1 : new ListNode(0); 

         // 同理 l2
        l2 = l2 ? l2 : new ListNode(0);

         // 计算该位的两个结点的和，同时要加上前两位的进位。
       	var num = l1.val + l2.val + y;
       
        // 判断该位是否需要进位，需要进位的话就该位取10的余数，然后进一位（y = 1），反之初始化进位值为0。
        num > 9 ? (y = 1, n = num - 10) : (n = num, y = 0); 
        
        arr[i] = n; //给和的每一位赋值
        
        i++; //进入下一次循环
        
        l1 = l1.next;  // 进入l1下一个结点
        
        l2 = l2.next; // 进入l2下一个结点
    }

    //这里反转一下，原本是[7, 0, 8]反转为[8, 0, 7],方便后面生成链表。
    arr = arr.reverse(); 

    //取第一个值创建第一个结点（也是最终链表的最后一个结点）
    let listNode = new ListNode(arr.shift()) //取第一个值创建第一个结点（也是最终链表的最后一个结点）
    
    return arr.reduce((ori,cur)=>{
    
        let ln = new ListNode(cur) //生成当前结点
    
        ln.next = ori //将当前结点的next指向之前生成的链表的第一个结点
    
        return ln //返回新的链表（赋值给了 ori ）
    
    }, listNode)
    // listNode是初始值。
    // 即链表末端的第一个值，我们之所以从最后一个开始创建链表也是因为方便。
    // 因为我们要操作结点的next，所以用新结点的next指向原来的链表，要比找到链表的最后一个结点的next指向新结点要方便一些。
    // 即从后往前生成链。
};
```
