---
title: ES6 对象数组查找某一个对象 findIndex
date: 2019-01-27 17:50:34
categories: JavaScript
tags: JavaScript
keywords: findIndex
---

>  查找数组特定元素需要用到的方法就是**findIndex()**。

## 用法与定义

 - findIndex() 方法返回传入一个测试**条件**（函数）符合条件的数组**第一个**元素位置。
 - findIndex() 方法为数组中的每个元素都调用一次函数执行：
 	当数组中的元素在测试条件时返回 true 时, findIndex() 返回**符合条件**的元素的**索引位置**，*之后的值不会再调用	执行函数。*
   如果没有符合条件的元素返回 **-1**

<!-- more -->

以上是比较官方的对于findIndex()的定义
---------------- --
**接下来我结合实例来进行自己的解释.**

 1. 第一条的意思如下:
当条件函数返回**true**的时候，findindex会跳出，然后返回当前元素的下标。
```
//首先是普通数组

var dataArr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0];

function fn(num, numIndex, nums){
	//该函数的三个参数，num代表当前项，numIndex代表当前项下标，nums代表该数组。
	return num > 5;
}

dataArr.findIndex(fn);//值为5(即6的下标)

------------------------分割线-----------------------------

//同样的假如你的数组是个对象数组：
var objArr = [{
	name: '小王',
	age: 14
},{
	name: '大王',
	age: 41
},{
	name: '老王',
	age: 61
}]					
				
function objFn(obj, objIndex, objs){
	return num.age > 20;
}

objArr .findIndex(objFn);//值为1(即大王的下标)
```
2. 第二条的意思就更好理解了，因为findindex只返回第一个符合条件的元素下标，所以在找到第一个符合条件的元素之后，他会跳出该函数，之后的数组内的元素将不再调用，相当于加了个break；

**实际用法举例**

假如我们要在所有人里面挑选队友，但是不想重复。在我们通过id查找的时候，就可以这么写
```
var allPeple = [{
	name: '小王',
	id: 14
},{
	name: '大王',
	id: 41
},{
	name: '老王',
	id: 61
}]
				
var myTeamArr = [{
	name: '小王',
	id: 14
}]
				
var PId = 14; //假如这个是要添加的人的ID

function pFn(p){return p.id == PId ;}

//判断myteam里是不是有这个队员，如果==-1 代表没有，在allPeople中找到他，添加入我的队伍

myTeamArr.findIndex(pFn) == -1 ? myTeamArr.push(allPeple.find(pFn)) : alert('已存在该人员');

//这样写可以将两个for循环直接总结成一行代码
```
**另外需要补充的一点**

> 与其相对应的有**find**()函数，用法一致，只不过返回的是**元素本身**，而不是元素的下标。

**兼容性**

> 因为是es6的所以使用的时候要注意兼容性问题，**ie11**及之前的版本都**不可兼容。**
