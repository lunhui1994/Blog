---
title: CSS揭秘：5.条纹背景（下）
date: 2020-09-23 11:14:27
categories: Css
tags: Css
keywords: linear-gradient, background-size
---

上篇文章讲述了实现条纹效果所使用的CSS特性并实现了水平和垂直条纹，接下来我们来实现斜向条纹。

回忆一下之前的效果

水平条纹

```css
background: linear-gradient(#fb3 33%, #58a 0, #58a 66%, yellowgreen 0);
background-size: 100% 60px;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200703004052995.png)
 垂直条纹
```css
background: linear-gradient(90deg, #fb3 33%, #58a 0, #58a 66%, yellowgreen 0);
background-size: 60px 100%;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200703010130334.png)
## 一、斜向条纹 
现在我们来试一下斜向条纹的效果。
### 1. 斜向 First Try
类似垂直条纹一样，我们是不是可以先试一下45deg角度的效果呢？
```css
background: linear-gradient(45deg, #fb3 33%, #58a 0, #58a 66%, yellowgreen 0);
background-size: 60px 60px;
```

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200707224917963.png)
 看来效果并不是我们想象的那样。
 
 ### 2. 斜向 Second Try 双条纹
 第一次尝试失败了，但是失败的经验还是有的，我们发现事实上，图片是斜过来的，但是是单片斜过来的。那么如果我们每一个单片的部分可以**无缝对接**，那是不是就有这个效果了呢？？？
 先来试一下双色条纹的
```css
background: linear-gradient(45deg, #fb3 25%, #58a 0, #58a 50%, #fb3 0, #fb3 75%, #58a 0, #58a 100%);
background-size: 60px 60px;
```

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200707225023196.png)
 我们通过制造一个个的相同无缝单片来`无缝`组成了斜向的效果，上图中的单片
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020070722525997.png)
 ### 3. 斜向 Third Try 三条纹
来试一下三条纹的
```css
background: linear-gradient(45deg, #fb3 16.6666%, #58a 0, #58a 33.3333%, yellowgreen 0, yellowgreen 50%, #fb3 0, #fb3 66.6666%, #58a 0, #58a 83.3333%, yellowgreen 0, yellowgreen 100%);
background-size: 60px 60px;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200708092318421.png)
 ### 4. 斜向 Forth Try 同色间隔条纹
来试一下同色间隔条纹
```css
background: linear-gradient(45deg, #58a 25%, rgba(85, 136, 170, .5) 0, rgba(85, 136, 170, .5) 50%, #58a 0, #58a 75%, rgba(85, 136, 170, .5) 0, rgba(85, 136, 170, .5) 100%);
background-size: 60px 60px;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200708093023181.png)

斜向的效果现在应该也已经掌握了，我们只需要遵守切片间可以无缝交接的原则即可。
 ### 5. 斜向 Fifth Try
实现了45度角，我们再试一下60度角吧~ 我们把45改为60.
```css
background: linear-gradient(60deg, #58a 25%, rgba(85, 136, 170, .5) 0, rgba(85, 136, 170, .5) 50%, #58a 0, #58a 75%, rgba(85, 136, 170, .5) 0, rgba(85, 136, 170, .5) 100%);
background-size: 60px 60px;
```
很显然失败了啊，除了45度角应该其他的比较难以实现了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200708093435612.png)
 ### 6. 斜向 Sixth Try 60deg
别担心，我们还有一个办法，使用`repeating-linear-gradient`专门用来创建重复的线性渐变图像的。用法和linear-gradient类似。但是使用上还是有一些小细节需要注意。
```css
background: repeating-linear-gradient(60deg, #58a 25%, rgba(85, 136, 170, .5) 0, rgba(85, 136, 170, .5) 50%, #58a 0, #58a 75%, rgba(85, 136, 170, .5) 0, rgba(85, 136, 170, .5) 100%);
```
~~background-size: 60px 60px;~~  记得这次要把size删掉了。我们不再需要了。
看，下面的效果已经实现了，这个方法可以实现任何角度的斜向条纹。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200708094400556.png)
# 总结
到现在我们已经实现了条纹效果的大部分情况，包括横向，垂直，斜向（45deg 和 任意角）。
核心css特性为：`background: linear-gradient(#fb3 33%, #58a 0, #58a 66%, yellowgreen 0);` 如果忘记了用法可以去[上篇](https://blog.csdn.net/lunhui1994_/article/details/106933714)开头再复习一下。
1. 横向条纹 
	```css
	background: linear-gradient(#fb3 33%, #58a 0, #58a 66%, yellowgreen 0);
	background-size: 100% 60px;
	```
2. 垂直条纹 `90deg`
	```css
	background: linear-gradient(90deg, #fb3 33%, #58a 0, #58a 66%, yellowgreen 0);
	background-size: 60px 100%;
	```
3. 斜向条纹
	1.  `45deg` + 可无缝拼接的单片条纹
		```css
		background: linear-gradient(45deg, #fb3 25%, #58a 0, #58a 50%, #fb3 0, #fb3 75%, #58a 0, #58a 100%);
		background-size: 60px 60px;
		```
	2. `repeating-linear-gradient()` 去掉background-size
		 ```css
		background: repeating-linear-gradient(60deg, #58a 25%, rgba(85, 136, 170, .5) 0, rgba(85, 136, 170, .5) 50%, #58a 0, #58a 75%, rgba(85, 136, 170, .5) 0, rgba(85, 136, 170, .5) 100%);
		```
下一篇：[CSS揭秘：6.复杂的背景图案（上）](https://blog.csdn.net/lunhui1994_/article/details/108526888)

# 相关阅读
- [CSS揭秘：1.半透明边框](https://blog.csdn.net/lunhui1994_/article/details/106653195)
- [CSS揭秘：2.多重边框](https://blog.csdn.net/lunhui1994_/article/details/106677231)
- [CSS揭秘：3.灵活的背景定位](https://blog.csdn.net/lunhui1994_/article/details/106699349)
- [CSS揭秘：4.边框内圆角](https://blog.csdn.net/lunhui1994_/article/details/106845534)
- [CSS揭秘：5.条纹背景（上）](https://blog.csdn.net/lunhui1994_/article/details/106933714)
- [CSS揭秘：5.条纹背景（下）](https://blog.csdn.net/lunhui1994_/article/details/107192736)