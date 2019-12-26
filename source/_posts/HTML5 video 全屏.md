---
title: HTML5 video 全屏
date: 2018-04-13 18:14:27
categories: JavaScript
tags: JavaScript
keywords: video全屏
---
#####  当我们使用video标签的时候，有时候因为更多的需要，我们要自己自定义控制栏，而进入和退出全屏也是其中的一部分

----------
- 不同的浏览器有不同的实现方法
```
// Webkit
element.webkitRequestFullScreen();//进入全屏
document.webkitCancelFullScreen();//退出全屏

// Firefox
element.mozRequestFullScreen();
document.mozCancelFullScreen();
 
// W3C 
element.requestFullscreen();
document.exitFullscreen();
 
```
<!-- more -->
- 一般兼容性写法，我们先使用w3c标准的方法，如果不可以在兼容不同浏览器。

```
//进入全屏
function FullScreen() {
    var ele = document.documentElement;
    if (ele .requestFullscreen) {
        ele .requestFullscreen();
    } else if (ele .mozRequestFullScreen) {
        ele .mozRequestFullScreen();
    } else if (ele .webkitRequestFullScreen) {
        ele .webkitRequestFullScreen();
    }
}
//退出全屏
function exitFullscreen() {
    var de = document;
    if (de.exitFullscreen) {
        de.exitFullscreen();
    } else if (de.mozCancelFullScreen) {
        de.mozCancelFullScreen();
    } else if (de.webkitCancelFullScreen) {
        de.webkitCancelFullScreen();
    }
}
```
-  接下来是用例

```
$(ele).on('click',function(){
    FullScreen();
   // exitFullscreen();
});
```
