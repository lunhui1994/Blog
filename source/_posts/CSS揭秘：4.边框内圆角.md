---
title: CSS揭秘：4.边框内圆角
date: 2020-06-22 18:14:27
categories: Css
tags: Css
keywords: 边框内圆角, RGBA/HSLA, background-clip
---
# 边框内圆角
> 背景知识：box-shadow，outline，“多重边框”
## 一、两个div嵌套
两个div实现内圆角很容易，只需要内圆角外直角即可。
### div First Try
```css
.box{
    	width: 200px;
        padding: 20px;
        background-color: #655;
    }
.content{
       background-color: tan;
       border-radius: .8em;
       padding: 20px;
   }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061823401650.png)
这种方案更加灵活，我们可以在box上设置更多的样式，但是需要两个元素才能实现。
## 二、box-shadow + outline 方案
还记得上篇中，outline和box-shadow对于圆角的区别显示吗？box-shadow会贴合border的圆角，outline不会。当我们仅需要实现一个实色的边框加内圆角，使用这个方案可以达到相同的效果。
### box-shadow + outline First Try

```css
width: 160px;
background: tan;
border-radius: .8em;
padding: 20px;
margin: 20px;
box-shadow: 0px 0px 0px .4em #655;
outline: 20px solid #655;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061823401650.png)
这种方案中，`box-shadow`是用来填补`outline`和`border`之间的间隙的，如果不加`box-shadow`效果会是这样的。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200618235718311.png)
1.  所以我们需要设置`box-shadow`的扩散半径来弥补四个类似三角形的空隙。
2. 至于扩散半径的大小，我们可以用勾股定理设置。
3. 也就是`border-radius`的圆角**圆心到角的距离 - 半径**。
4. `(√2 - 1)r`； `√2 ≈ 1.4`那么`(√2 - 1)` 也就是在`0.4 - 0.5`之间,我们可以按0.5计算即可。也就是0.5r。

5. 最后再回顾一下`box-shadow`的用法，`outline`和`border`用法一样，同时可以使用`outline-offset: -30px;`调整位置。
```
/* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
```
```css
box-shadow: 2px 2px 2px .4em #655;
```

说起这个有人会说，为什么不用`outline-offset:-10px;`这样来顶替`box-shadow`呢？试一下就知道了。`outline`的显示层级较`border`更高，所以border的圆角会被覆盖掉。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200618235913302.png)

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
            width: 200px;
            padding: 20px;
            background-color: #655;
        }
        .content{
            background-color: tan;
            border-radius: .8em;
            padding: 20px;
        }
        .box2 {
            width: 160px;
            background: tan;
            border-radius: .8em;
            padding: 20px;
            margin: 20px;
            box-shadow: 0px 0px 0px .4em #655;
            outline: 20px solid #655;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="content">
            对酒当歌，人生几何。
        </div>
    </div>
    <hr>
    <hr>
    <hr>
    <div class="box2">
        对酒当歌，人生几何。
    </div>   
</body>
</html>
```
