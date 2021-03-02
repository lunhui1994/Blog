---
title: CSS：文本的水平垂直居中
date: 2018-01-17 16:59:13
categories: Css
tags: Css
keywords: 文本的水平垂直居中, vertical-align 
---

# 文字居中
## 垂直居中（vertical-align）
我们都知道有这么一个属性可以让图片，文本等在元素中垂直居中
```css
vertical-align:middle;
```
vertical-align值有很多，常用的就是middle，bottom，text-bottom等，我们先说middle。
#### vertical-align时而没效果 ####

然而真实使用的时候，我们会发现这个属性“时灵时不灵”，有些情况下我们加了这个属性之后仍然不见img或者text有任何的变化。**那是因为vertical-align只作用在inline-block或者inline，还有table-cell等元素内**。同时这两种还有所不同。

<!-- more -->
1. 对于行内元素
	1. text-top
使元素的顶部与父元素的字体顶部对齐。
	2. text-bottom
使元素的底部与父元素的字体底部对齐。
	3. middle
使元素的中部与父元素的基线加上父元素x-height（译注："x"高度）的一半（交叉处）对齐。
	
2. 对于table-cell 的常用

	1. top
		使单元格内边距的上边缘与该行顶部对齐。
	2. middle
		使单元格内边距盒模型在该行内居中对齐。
	3. bottom
		使单元格内边距的下边缘与该行底部对齐。

>vertical-align并不是在高度内居中，而是对齐在行高内的middle线上。

所以我总结了两种使用vertical-align居中的方法：

 1. 第一种
 
```html
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
>这种情况下文字是因为line-height属性而居中，同时img上的vertical-align：middle属性使其对齐在middle线上，如果父盒子去掉了line-height属性的话那么文字将不会再垂直居中。（可以试试bottom和text-bottom的不同。）

3. Flex

	flex布局需要使用`align-items`来进行上下居中, 这个属性是`父元素`的属性, 可以统一设置`子元素`的上下对齐方式.
```css
.box {
	display:flex;
	align-items: center 
}
```

这样.box元素内的子元素是可以垂直居中的。那么要单独设置某个子元素的上下对齐方式的话需要使用` align-self: center ` 这个属性. 它可以单独设置某个子元素的对齐方式, 且覆盖父元素的样式. 

```css
.box {
	display:flex;
	align-items: center 
}
.child {
	align-self: center 
}
```
----------
## 水平居中

```css
text-align:center;
```

over