---
title: Opacity引发的元素层级变化
date: 2018-06-29 18:14:27
categories: Css
tags: Css
keywords: opacity, 层级
---

### 背景

>  发现这个问题是在图片上定位了一个删除按钮，当我用opacity属性对图片进行透明化处理的时候，发现删除按钮不管用了，最后发现删除按钮是被图片覆盖了，究其原因是因为opacity这个属性造成的层级变化。

 1. 我发现含有opacity属性的元素层级会比其他元素的层级高，这时候z-index是不起作用的，opacity会一直高于其他元素的层级。
 2. 给其他元素加上position属性，会使该元素跟opacity处在同一层级之上，这时候你再给元素附加z-index就可以起作用了。

<!-- more -->

### 总结和解决方案


- 总结：当你使用opacity的时候会对元素层级造成影响

- 解决办法：加上position和z-index可以对opacity元素进行覆盖

```
.box{
            width: 200px;
            height: 200px;
            background-color: red;
            color: #fff;
            cursor: pointer;
        }
        .box1{
            opacity: 0.8;
        }
        .box2{
            background-color: blue;
            margin-left: 30px;
            margin-top: -160px;
            position: relative;
            z-index: 100;
        }
        .box3{
            background-color: green;
            margin-left: 60px;
            margin-top: -160px;
            opacity: 0.7;
        }
```

如上：box2的层级是最高的。