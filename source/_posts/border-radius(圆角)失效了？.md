---
title: border-radius(圆角)失效了？
date: 2019-08-29 10:53:27
categories: Css
tags: Css
keywords: border-radius, border-radius失效
---

> 今天涉及两个属性：
> 1. overscroll: scroll-y / scroll-x / scroll;
> 2. border-radius: 10px;

<!-- more -->

原本这两个属性是没有什么联系的，但是当同时出现在同一个元素上时，就会发生圆角效果被滑动条覆盖的情况。
例如：

```
<style>
	div {
		border-radius:10px;
		overflow:scroll-y;
	}
</style>
<div></div>
```

这种情况下，div的右上角和右下角都会因为滚动条的存在而显示的是直角。
其实div的四个角确实已经有了圆角的效果，但是滚动条属于div内部的元素，层级高，所以将div的圆角遮挡住了。那么我们解决这种情况的方法也很简单。



```
<style>
	div {
		border-radius: 10px;
		overflow: hidden;
	}
	ul {
		overflow: scroll-y;
	}
</style>
<div>
	<ul></ul>
</div>
```

我们在原本需要滚动的盒子内部加上一层滚动元素，将滚动的效果放在内部的滚动元素上，外部div加上overflow: hidden;并设置圆角，就可以达到我们需要的效果。

同样的我们也可以在当前元素的外层加上圆角遮罩，最终效果同上。

如有其他情况欢迎补充