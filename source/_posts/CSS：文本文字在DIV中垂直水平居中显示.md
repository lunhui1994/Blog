---
title: CSS：文本的水平垂直居中
date: 2018-01-17 16:59:13
categories: Css
tags: Css
keywords: 文本的水平垂直居中, vertical-align 
---
#文字居中
##垂直居中（vertical-align）
我们都知道有这么一个属性可以让图片，文本等在元素中垂直居中
```
vertical-align:middle;
```
vertical-align值有很多，常用的就是middle，bottom，text-bottom等，我们先说middle。

<!-- more -->

#### vertical-align时而没效果 ####

然而真实使用的时候，我们会发现这个属性“时灵时不灵”，有些情况下我们加了这个属性之后仍然不见img或者text有任何的变化。那是因为vertical-align只作用在inline-block或者inline，还有table-cell等元素内。同时这两种还有有所不同。
>vertical-align并不是在高度内居中，而是对齐在行高内的middle线上。

所以我总结了两种使用vertical-align居中的方法：

 1. 第一种
 

```
<div style="vertical-align: middle;display: table-cell;">
    <img src="02.jpg" alt="">
    <p>文本居中</p>
</div>
```
>这种情况下图片和文字可以分行显示文字在图片下面同时图片和文字作为整体在元素内垂直居中。

 2. 第二种

```
<div style="height:180px;line-height:180px;">
    <img src="02.jpg" alt="图片" style="vertical-align:middle;" />
    这是文本内容.
</div>
```
>这种情况下文字是因为line-height属性而居中，跟vertical-align属性没有关系。同时img对齐在middle线上，但是如果父盒子去掉了line-height属性的话那么将会不起作用。（可以试试bottom和text-bottom的不同。）


----------
##水平居中

```
text-align:center;
```

over