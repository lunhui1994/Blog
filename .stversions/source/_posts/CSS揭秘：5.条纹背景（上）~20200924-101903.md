---
title: CSS揭秘：5.条纹背景（上）
date: 2020-09-23 11:14:27
categories: Css
tags: Css
keywords: linear-gradient, background-size
---

# 条纹背景
> 背景知识：CSS线性渐变，background-size

 CSS线性渐变
 ---
```css
  background: linear-gradient(red, yellow, blue);
  background: linear-gradient(red 0%, yellow 50%, blue 100%);
  background: linear-gradient(to right, red 0%, yellow 50%, blue 100%);
  background: linear-gradient(90deg, red 0%, yellow 50%, blue 100%);
```
1. `to right`代表渐变偏移角度，`to right` (相当于`90deg`)
2. `red，yellow，blue`代表渐变色，表示从`red - yellow - blue (相当于red 0% - yellow 50% - blue 100%)`。 意思是从0%距离处为red，通过0%-50%的距离渐变到yellow，再通过50%-100%的距离渐变到blue。
	1. linear-gradient(90deg, red 0%, yellow 50%, blue `0`) 等价于 linear-gradient(90deg, red 0%, yellow 50%, blue `50%`) 因为当你的色标位置值设置为0时，会自动调整为前一个色标位置值。
	2. linear-gradient(90deg, red 20%, yellow 50%, blue 100%); 代表从**0-20%都为red色**。
	3. linear-gradient(90deg, red `20%`, yellow `20%`, blue 100%); 代表从20%处颜色**突然**变化为yellow。（**20%-20%之间没有渐变距离**）
	4. linear-gradient(90deg, red 20%, yellow `20%`, yellow `50%`, blue 100%); 代表从`20%-50%`处都是**黄色**，然后从50%处开始渐变直到100%变化为blue

css线性渐变小结
---
3. line-gradient中相邻的两个颜色值代表，从色标A渐变到色标B。
4. 颜色后紧跟的数值，代表AB两个颜色之间的渐变区间。（差值为渐变区间的长度，若差值为0，则为突变）
5. 颜色后的数值为0时，自动取前一位的数值。

background-size
---
```css
/* 关键字 */
background-size: cover
background-size: contain

/* 一个值: 这个值指定图片的宽度，图片的高度隐式的为auto */
background-size: 50%
background-size: 3em
background-size: 12px
background-size: auto

/* 两个值 */
/* 第一个值指定图片的宽度，第二个值指定图片的高度 */
background-size: 50% auto
background-size: 3em 25%
background-size: auto 6px
background-size: auto auto
```
1. `cover`代表背景图片覆盖背景尺寸，可能只能看到放大后的背景图片的一部分。
2. `contain`代表背景图片装入背景尺寸，可能会看到背景留白。
3. `两个值`分别为宽，高，高可以省略，默认auto
> 摘自MDN [background-size](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-size)
## 水平条纹
首先我们来实现一下水平条纹的效果。
### 水平 First Try
我们了解了line-gradient的能力之后。我们可以通过极端缩小两个颜色之间的过渡间距来实现颜色突变的效果。`background: linear-gradient(#fb3 50%, #58a 50%);`
> CSS图像（第三版）：如果多个色标具有相同的位置，它们会产生一个无限小的过渡区域。过渡的起止色分别是第一个和最后一个指定值。从效果上看，颜色会突然变化，而不是一个平滑的渐变过程。
>
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200703002131380.png)
 ### 水平 Second Try
 目前看来我们已经实现了两个巨大的条纹，分别占据一半的高度。接下来我们再加上background-size的能力。`background-size: 100% 20px;`
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200703002828821.png)
 我们通过控制背景的尺寸，高度设置为20px，那么，各占50%背景尺寸高度的实际尺寸就变成了10px高。再加上背景的默认是重复平铺的，所以就实现了条纹的效果了。
  ### 水平 Third Try
  现在实现了两种颜色的交叉条纹。那么如果三种颜色，四种颜色怎么办呢？我想看到这里大家思考之后都会知道如何实现。
```css
background: linear-gradient(#fb3 33%, #58a 0, #58a 66%, yellowgreen 0);
background-size: 100% 60px;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200703004052995.png)
水平条纹的颜色基本都可以实现了
 ### 水平 Forth Try
 我们再来尝试尝试加上透明度的效果
 

```css
background: linear-gradient(rgba(255, 187, 51, 0.9) 0%, rgba(255, 187, 51, 0.2) 33%, rgba(85, 136, 170, 0.9) 0, rgba(85, 136, 170, 0.2) 66%, rgba(154, 205, 50, 0.9) 0, rgba(154, 205, 50, 0.2) 100%);
/* background: linear-gradient(rgba(255, 187, 51, 0.2) 0%, rgba(255, 187, 51, 0.9) 33%, rgba(85, 136, 170, 0.2) 0, rgba(85, 136, 170, 0.9) 66%, rgba(154, 205, 50, 0.2) 0, rgba(154, 205, 50, 0.9) 100%); */
background-size: 100% 60px;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200703005540472.png)
```css

background: linear-gradient(rgba(255, 187, 51, 0.2) 0%, rgba(255, 187, 51, 0.9) 33%, rgba(85, 136, 170, 0.2) 0, rgba(85, 136, 170, 0.9) 66%, rgba(154, 205, 50, 0.2) 0, rgba(154, 205, 50, 0.9) 100%);
background-size: 100% 60px;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200703005620954.png)
加上了透明度配合渐变，实现了一些立体凹陷和饱满的条纹效果。
## 垂直条纹
实现过了水平条纹，垂直条纹还不是手到擒来。
我们只需要在上述的水平条纹代码中做两处改变即可。
1. 渐变里参数添加角度，to right 或者 90deg 
2. background-size 参数互换位置。

```css
background: linear-gradient(90deg, #fb3 33%, #58a 0, #58a 66%, yellowgreen 0);
background-size: 60px 100%;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200703010130334.png)
其他效果留给大家去实践~

下一篇：[CSS揭秘：5.条纹背景（下）](https://blog.csdn.net/lunhui1994_/article/details/107192736)

# 相关阅读
- [CSS揭秘：1.半透明边框](https://blog.csdn.net/lunhui1994_/article/details/106653195)
- [CSS揭秘：2.多重边框](https://blog.csdn.net/lunhui1994_/article/details/106677231)
- [CSS揭秘：3.灵活的背景定位](https://blog.csdn.net/lunhui1994_/article/details/106699349)
- [CSS揭秘：4.边框内圆角](https://blog.csdn.net/lunhui1994_/article/details/106845534)
- [CSS揭秘：5.条纹背景（上）](https://blog.csdn.net/lunhui1994_/article/details/106933714)
- [CSS揭秘：5.条纹背景（下）](https://blog.csdn.net/lunhui1994_/article/details/107192736)