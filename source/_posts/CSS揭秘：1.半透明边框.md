---
title: CSS揭秘：1.半透明边框
date: 2020-06-07 18:14:27
categories: Css
tags: Css
keywords: 半透明边框, RGBA/HSLA, background-clip
---
# 半透明边框
> 背景知识：`RGBA/HSLA` 半透明颜色， 它们同样是一种颜色，并非只适用于背景。
> `background-clip` 背景裁切属性，定义了背景的延伸范围，是否延伸到`边框、内边距盒子、内容盒子，内容文字`下面。分别对应`border-box、padding-box、content-box、text`四个属性值
<!-- more -->

## First Try
首先我们来尝试一下，假如我们想要实现一个半透明的边框，该如何写样式

```css
border: 10px solid hsla(0, 0%, 0%, 0.5);
background-color: white
```
这段css样式，我们期待的效果是有一个**半透明的边框**。而实际效果是怎么样的呢？
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200609231145786.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)
  **！！！？border 不见了**

## Second Try
没错，边框不见了。这跟我们所期待的效果并不符合。原因在于，默认情况下，**背景颜色会延伸到边框上**，这点我们可以通过虚线边框来发现实际发生了什么。

```css
border: 30px dashed orange;           
background-color: skyblue;
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200609231721914.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)
所以，实际上我们的透明边框其实是存在的，只不过由于**背景颜色透过边框**，导致最终呈现出如同一个跟背景颜色一致的边框的效果。
## Third Try
好在目前我们可以通过`background-clip`来处理背景色的延伸范围。
```css
border: 30px dashed orange;           
background-color: skyblue;
background-clip: padding-box;
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200609232056471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)
## Finally Finashed

```css
border: 30px solid hsla(0, 0%, 100%, 0.3); 
background-color: white;
background-clip: padding-box;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200609232537463.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)

## 总结
1. 我们为了实现最终的效果，使用到了`background-color`， `border`，`background-clip` 和 `hsla/rgba`半透明颜色。
2. `background-clip` 定义了背景的延伸范围，默认`border-box（从border的外边沿裁切背景）`，通过将其值设置为`padding-box（从padding外边沿开始裁切背景）`避免背景色延伸到边框下。
3. `hsla/rgba` 实现了边框的半透明颜色

## 最终案例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            display: flex;
            align-items: center;
            justify-content: center;
            width: 400px;
            height: 300px;
            background: #ccc url("http://img.mp.itc.cn/upload/20170621/79ecb57d6e234e7891b6c9da3dfc12f9_th.jpg") no-repeat center;
        }
        .content {
            width: 200px;
            height: 150px;
            padding: 20px;
            border: 30px solid hsla(0, 0%, 100%, 0.3);
            /* border: 30px solid rgba(120, 120, 120, 0.5); */
            background-color: white;
            background-clip: padding-box;
        }
        p{
            text-align: center;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="content">
            <p>半透明边框效果展示</p>
        </div>
    </div>
</body>
</html>
```
