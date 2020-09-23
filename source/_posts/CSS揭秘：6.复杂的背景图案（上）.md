---
title: CSS揭秘：4.边框内圆角
date: 2020-09-23 11:14:27
categories: Css
tags: Css
keywords: 边框内圆角, RGBA/HSLA, background-clip
---



@[TOC](文章目录)

</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

# 前言
上一篇文章中我们学会了如何使用渐变实现条纹状的背景，但是实际上条纹背景并不是我们能实现的唯一的背景图案，利用渐变我们可以实现很多更为复杂的图案，本篇会介绍一些其他的简单而实用的背景图案。

<font color=#999AAA ></font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


# 一、网格


<font color=#999AAA >网格的原理其实很简单，目前我们已经实现了条纹背景，那么如果我们将条纹背景组合呢？互相穿插组合，那么是不是很简单就实现了各种各样的网格背景。

## 1. 实色网格

<font color=#999AAA >代码如下：
```css
	width: 200px;
	height: 200px;
	background-image:
		linear-gradient(rgba(255,187,51, .5) 33%, rgba(85,136,170, .5) 0, rgba(85,136,170, .5) 66%, rgba(173,255,47, .5) 0),
		linear-gradient(90deg, rgba(255,187,51,1) 33%, rgba(85,136,170, 1) 0, rgba(85,136,170, 1) 66%, rgba(173,255,47, .5) 0);
	background-size: 60px 60px;
```

<font color=#999AAA >实际效果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200918100511610.png#pic_center)
## 2. 边框网格
实现类似边框网格的效果，我们只需要将实色的网格色块改变成一条边框就可以了。方法为在渐变中设置一条**1px**长度的颜色，然后剩下的颜色为**透明色**（或者其他底色）。

<font color=#999AAA >代码如下：
```css
	width: 201px;
	height: 201px;
	background-image:
		linear-gradient(rgba(255,187,51, .5) 1px, transparent 0),
		linear-gradient(90deg,rgba(255,187,51, .5) 1px, transparent 0);
	background-size: 20px 20px;
```

<font color=#999AAA >实际效果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200918102106767.png#pic_center)
我们可以通过调整**background-size**来调整**网格**的大小。

**注意**：整个图片的大小为**201x201**，由于我们设置的其实是**左边和上边**，所以同一个单位背景内只有上边和左边，没有下边和右边。所以如果设置总长为200px时，右边和下边会看不到边框。所以我们**长宽各 + 1**，以下一个背景的左上作为我们的右下边框。

## 3. 波点
除了实现类似格子一样的背景，我们还可以实现波点背景样式，这时我们就需要用到另外一个渐变：**径向渐变（radial-gradient）**，和线性渐变类似，效果是从背景中心点出发，向外渐变。

<font color=#999AAA >代码如下：
```css

	width:200px;
	height:200px;
	background-image: radial-gradient(green 3px, yellowgreen 0, yellowgreen 6px, transparent 0);
	background-size: 20px 20px;

```

<font color=#999AAA >实际效果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200921085836761.png#pic_center)

我们可以通过调整**background-size**来调整**波点密度**。

**注意**：使用background-size调整波点背景的单个背景大小，调整的视觉效果是波点的密度大小，有时候会呈现出不一样效果和图案，比如当我们的波点连接起来的时候，我们的图案就变成了一个个菱形。

<font color=#999AAA >实际效果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200921090246511.png#pic_center)
<font color=#999AAA >以上效果只需要将background-size调整为12px，即背景尺寸 = 波点直径。
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


## 4. 棋盘
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020092109251135.png#pic_center)

棋牌有点类似于我们在第一步做到的实色网格，不同的是棋盘是由一个实色和一个透明所组成的图案。所以，看似和实色网格类似，但是实现起来无法使用跟实色网格相同的实现方法（不信邪的话可以使尝试一下），那么我们如何实现棋牌类型的图案呢？也不难，我们使用直角三角形进行拼接。还得我们在实现斜向条纹时做的尝试吗？[CSS揭秘：5.条纹背景（上）](https://blog.csdn.net/lunhui1994_/article/details/106933714)

<font color=#999AAA >代码如下：
```css

	width:200px;
	height:200px;
	background: #eee;
	background-image: linear-gradient(45deg, #bbb 50%, transparent 0);	
	background-size: 30px 30px;

```

<font color=#999AAA >实际效果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200921092328712.png#pic_center)

1. 从刚开始的展示图案可以看出来，我们的正方形色块只是对角线的一半，那么我们的一个三角形就是1/4了，所以我们改造一下


<font color=#999AAA >代码如下：
```css

	width:200px;
	height:200px;
	background: #eee;
	background-image: linear-gradient(45deg, #bbb 25%, transparent 0);
	background-size: 40px 40px;

```

<font color=#999AAA >实际效果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200921092909744.png#pic_center)

2. 现在我们得到一半的三角形，那么拼一个正方形还需要一个相反方向的三角形，我们再来一个背景
```css
	background-image: 
		linear-gradient(45deg, #bbb 25%, transparent 0),
		linear-gradient(45deg, transparent 75%, #bbb 0);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200921092709910.png#pic_center)

3. 到此我们得到了两个直角三角形，那么如何拼接成一个正方形呢？使用**background-position**调整直角三角形的位置即可。把右上角显示三角形的背景 向下 向左移动就阔以了
```css

	width:200px;
	height:200px;
	background: #eee;
	background-image: 
		linear-gradient(45deg, #bbb 25%, transparent 0),
		linear-gradient(45deg, transparent 75%, #bbb 0);
	background-position: 0px 0px, -20px 20px;
	background-size: 40px 40px;

```
<font color=#999AAA >实际效果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200921094902113.png#pic_center)
那么搞定了一个再搞定另外一个就很简单了。
```css

	width:200px;
	height:200px;
	background: #eee;
	background: #eee;background-image:    
		linear-gradient(45deg, #bbb 25%, transparent 0),    
		linear-gradient(45deg, transparent 75%, #bbb 0),    
		linear-gradient(45deg, #bbb 25%, transparent 0),    
		linear-gradient(45deg, transparent 75%, #bbb 0);
	background-position: 0 0, -20px 20px,                     
		20px -20px, 0 0;
	background-size: 40px 40px;

```
<font color=#999AAA >实际效果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200921095309238.png#pic_center)


# 小结
本篇主要对一些更为复杂的背景图案做出了一些介绍，分别有**实色网格图案，边框网格图案，波点图案，棋盘图案**，其中：
1. 实色网格用到了透明叠加的方法，实现了苏格兰裙一样的背景图案。
2. 边框网格实际上是利用1px的渐变边框，实现其实类似纯div的左上border的方法。
3. 波点使用到了**radial-gradient**径向渐变，使用方法和线性渐变相似。
4. 棋盘图案则借助了background-position对直角三角形进行定位，以组合出正方形图案。

# 相关阅读
- [CSS揭秘：1.半透明边框](https://blog.csdn.net/lunhui1994_/article/details/106653195)
- [CSS揭秘：2.多重边框](https://blog.csdn.net/lunhui1994_/article/details/106677231)
- [CSS揭秘：3.灵活的背景定位](https://blog.csdn.net/lunhui1994_/article/details/106699349)
- [CSS揭秘：4.边框内圆角](https://blog.csdn.net/lunhui1994_/article/details/106845534)
- [CSS揭秘：5.条纹背景（上）](https://blog.csdn.net/lunhui1994_/article/details/106933714)
- [CSS揭秘：5.条纹背景（下）](https://blog.csdn.net/lunhui1994_/article/details/107192736)
