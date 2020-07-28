---
title: CSS：z-index失效了？
date: 2019-08-27 18:14:27
categories: Css
tags: Css
keywords: z-index, z-index失效
---

### 背景

> 我们在使用z-index的时候，有时候会发现，无论怎么样增大z-index的值，都无法改变目标元素的层级。

 -  ** 其实是这几种情况在作怪 **
 1. **position**
 2.  **float**
 3.  **opcity** 
<!-- more -->
  
### 父盒子的层级问题

- 父盒子的层级问题，当你的父盒子层级小于另一个父盒子层级的时候，你子元素的z-index再高都是没有用的。该元素的层级只在该元素所在的容器内起作用。该种情况大家应该容易理解，不过也可以看下面的例子。
  
```
<style type="text/css">
        .a1{
            position:relative;
            z-index: 2;
            width:400px;
            height:400px;
            background: black;
            border:1px solid #eee;
            margin-bottom: -100px;
        }
        .a{
            position:relative;
            z-index: 1;
            width:400px;
            height:400px;
            background: yellow;
            left:0;
            border:1px solid #eee;
        }

        .b{
            position:absolute;
            left:0;
            top:0;
            z-index: 50;
            width:200px;
            height:200px;
            background: red;

        }

        .c{
            position: absolute;
            left:0;
            top:0;
            z-index:1111;
            width:100px;
            height:100px;
            background: blue;
        }
    </style>
</head>
<body>
<div class="a1"></div>
<div class="a">
    <div class="b"></div>

    <div class="c"></div>
</div>
</body>
```
---------
  
### opcity属性

 - opcity属性，当你要设置两个元素的z-index值的时候，要注意是否给其中一个元素添加了opcity属性，如果添加了，那么添加了opcity属性的元素将一直在最上层，这个我在另一篇文章中讲过。
 - [opcity引发的元素层级变化](https://www.zsfmyz.top/2018/06/29/Opcity%E5%BC%95%E5%8F%91%E7%9A%84%E5%85%83%E7%B4%A0%E5%B1%82%E7%BA%A7%E5%8F%98%E5%8C%96/)
   
-------------

### position影响

 - 还有就是比较普遍的，我们设置层级一般都是因为设置了position，当你设置的两个子元素，一个有position属性，另一个没有position的时候，拥有position属性的元素将一直在其他元素上方。如下面的例子
  
```
<style type="text/css">
        .a{
            position:relative;
            z-index: 1;
            width:400px;
            height:400px;
            background: yellow;
            left:0;
            border:1px solid #eee;
        }

        .b{
            position:absolute;
            left:0;
            top:0;
            z-index: 50;
            width:200px;
            height:200px;
            background: red;

        }

        .c{
            z-index:100;
            width:100px;
            height:100px;
            background: blue;
        }
    </style>
</head>
<body>
<div class="a">
    <div class="b"></div>
    <div class="c"></div>
</div>
</body>
```
需要给c也加上position属性才让bc可以先同处一级，然后z-index才会起作用。


### float影响

 - 那如果我把子元素的position都去掉呢？当父盒子有position而所有子元素都没有position属性的时候，z-index一样全部失效，后面的盒子将会覆盖前面的盒子，z-index无效。
 - 同样的我们两个都加上浮动的效果跟上面的效果是一样的，后覆盖前，z-index无效
 - 一个加float另一个不加，则是加float的元素一直浮动在最上层。

```
<style type="text/css">
        .a{
            position:absolute;
            z-index: 1;
            width:400px;
            height:400px;
            background: yellow;
            left:0;
            border:1px solid #eee;
        }

        .b{
            z-index: -10;
            width:200px;
            height:200px;
            background: red;
            margin-bottom: -100px;

        }

        .c{
            z-index:101;
            width:100px;
            height:100px;
            background: blue;
        }
    </style>
</head>
<body>
<div class="a">
    <div class="b"></div>
    <div class="c"></div>
</div>
</body>
```
其他情况欢迎补充。