---
title: CSS3：图片的高斯模糊效果
date: 2018-05-18 16:59:13
categories: Css
tags: Css
keywords: css3, 高斯模糊, filter
---

# CSS3：图片的高斯模糊效果

>  最近项目中需要预览视频中加马赛克的效果（高斯模糊），于是找到了css3的一个属性filter来进行高斯模糊

<!-- more -->

filter（滤镜）
-----
> 可以用来定义图片或者div的饱和度，模糊程度，亮度等一系列。具体参考 [CSS3：filter 属性](http://www.runoob.com/cssref/css3-pr-filter.html)

兼容性
-----
![这里写图片描述](https://img-blog.csdn.net/20180518144313540?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

兼容性方案
-----

```
//高斯模糊
-webkit-filter：blur(10px);
filter:blur(10px);
```

高斯模糊的这个属性是对整个元素进行高斯模糊。如果你需要局部模糊，需要结合背景定位。原理就是两层图片叠加，底层清晰，上层模糊。接下来我会结合拖拽和背景定位实现图片的局部模糊。下一篇传送门：**[js拖拽实现](https://www.zsfmyz.top/JavaScript/HTML5%20drag%20&%20drop%20%E6%8B%96%E6%8B%BD%E4%B8%8E%E6%8B%96%E6%94%BE/)**