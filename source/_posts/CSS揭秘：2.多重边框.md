---
title: CSS揭秘：2.多重边框
date: 2020-06-12 18:14:27
categories: Css
tags: Css
keywords: 多重边框, box-shadow, outline
---
# 多重边框
> 背景知识：box-shadow的基本用法，outline基本用法

```
/* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
```
```css
box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
```
以上是box-shadow的基本参数。box-shadow是为元素添加阴影效果的样式。但是我们可以通过对其属性的设置，呈现边框效果。
<!-- more -->
## box-shadow 方案
### box-shadow First Try
1. 将x偏移量 ，y偏移量设置为0px，此时阴影会在元素下面不会超出元素本身。
2. 模糊度设为0px，使阴影呈现实体效果。
3. 增大扩散半径，可以理解为阴影向外扩展半径。
4. 此时阴影就像一条宽度为扩散半径的实线边框
```css
box-shadow: 0px 0px 0px 10px rgba(0, 0, 0, 0.6);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200610231531723.png)
### box-shadow Second Try
box-shadow属性可以通过逗号分割添加多条阴影。
```css
box-shadow: 0px 0px 0px 20px rgba(0, 0, 0, 0.6),
            0px 0px 0px 40px rgba(120, 120, 120, 0.6);
```
![!\[在这里插入图片描述\](https://img-blog.csdnimg.cn/20200610231706575.pn](https://img-blog.csdnimg.cn/20200610231807105.png)
要注意的是，阴影是`层层叠加`的，第一条阴影在最上层，以此类推，且阴影的半径都是以元素`border`的`外边沿为起点`。所以如果你想要两条宽`20px`的阴影，那么两条阴影的`扩散半径`需要分别设置`20px`，`40px`

---
我们需要注意的是，阴影不会影响元素的布局，我们可以从它的字面意思，阴影来理解，它不占用任何空间。并且元素上的绑定事件，并不会在阴影上触发。效果如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200610232610906.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)
### box-shadow Third Try
那么如果我们需要阴影像我们预期的一样，跟border有相同的表现，我们可以增加同样的外边框margin来模拟出阴影占据的空间。
```css
margin: 40px;
box-shadow: 0px 0px 0px 20px rgba(0, 0, 0, 0.6),
            0px 0px 0px 40px rgba(120, 120, 120, 0.6);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061023293232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)
### box-shadow Fourth Try
目前来讲，阴影的扩展方向都是从`border外边沿向外扩。`它虽然模拟出了空间，但是仍旧不会触发元素上的事件，如果你想在`事件`上也同`border`的表现一样，那么可以设置`inset`属性，使其向内扩散，并通过内边距`padding`来模拟空间。
```css
margin: 40px;
background-clip: content-box;
box-shadow: 0px 0px 0px 20px rgba(0, 0, 0, 0.6),
            0px 0px 0px 40px rgba(120, 120, 120, 0.6);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200610233456274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)
需要注意的是，边框的顺序发生了反转。如果阴影边框设置了透明度，因为涉及到透明度颜色叠加，需要自己取色。同样的透明度也会被背景色穿透，如果不想被背景色影响，可设置`background-clip: content-box;`

## outline 方案
outline可以实现两条边框的方案，同时更加灵活可以实现虚线边框。
**border 和 outline 很类似，但有如下区别：**
1. 轮廓不占据空间（同阴影），绘制于元素内容周围。
2. outline不一定贴合圆角。
3. 我们可以通过outline-offset设置负值，来使轮廓显示在元素内部。
### outline  First Try
```css
border: 10px solid blue;
outline: 10px solid skyblue;
```
不占据空间
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061023585033.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)
### outline Second Try
```css
border: 10px solid blue;
outline: 10px solid skyblue;
border-radius: 20px;
```
不贴合圆角
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200611000119188.png)
### outline Third Try
`outline-offset` 属性实现的缝边效果

```css
background-color: #333;
outline: 2px dashed white;
border-radius: 20px;
outline-offset: -30px;
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200611000409118.png)