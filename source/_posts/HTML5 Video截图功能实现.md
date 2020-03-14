---
title: HTML5 Video截图功能实现
date: 2019-08-30 15:37:09
categories: JavaScript
tags: JavaScript
keywords: video, 截图, canvas, drawImage, toDataURL
top: true
---

### 背景

> 因为所在公司业务为视频编解码，所以项目大多数是围绕视频展开的，之前做了一个快编项目，可以当作一个web端的小型视频编辑器。当中就需要对视频当前帧图进行截取，然后后续当作视频的封面或者海报图。 那么我就需要实现这么一个视频截图的功能。

<!-- more -->

### API简介

1. 首先视频截图在我们大前端实现，就要借助canvas的drawImage()这个api了。
    - drawImage 该api的功能简单讲就是将一张图片(Source image)绘制在canvas(Destination canvas)上. 
    - 语法：
    >void ctx.drawImage(image, dx, dy);
    >void ctx.drawImage(image, dx, dy, dWidth, dHeight);
    >void ctx.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
    
    - image： 原图片Element.
    - sx, sy, sWidth, sHeight : 需要在原图取起点x，y，宽，高；
    - dx, dy, dWidth, dHeight：在目标canvas上绘制的起点x，y，宽，高。
    - image，dx，dy为必填

2. 当我们截取并绘制完图片之后，就需要将在canvas上绘制的图片转化为图片了，那就需要 toDataURL('image/png') 将其转换。
    - 该方法返回一个用作展示的图片地址。 
    - 语法：
    > canvas.toDataURL(type, encoderOptions); 
    
    - type: 默认为 image/png， 可选 image/jpeg或者image/webp 
    - encoderOptions：当type为 jpeg或者webp 时，可以选择0-1区间内的值作为输出的图片质量。


  
 **MDN**
- [HTMLCanvasElement.toDataURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/toDataURL) 
- [CanvasRenderingContext2D.drawImage()](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/toDataURL)

----

回到正题，函数实现
   
### 代码实现

**HTML** 
```
    <video id="video" controls="controls">
        <source src="../videos/xxx.mp4" />
    </video>
    <button id="capture">Capture</button>
    <div id="output"></div>
```

**Script** 
```

    (function() {
        var video, $output;
        var scale = 0.25;
        var initialize = function() {
            $output = $("#output");
            video = $("#video").get(0);
            $("#capture").click(captureImage);
        };

        var captureImage = function() {
            var canvas = document.createElement("canvas");
            canvas.width = video.videoWidth * scale;
            canvas.height = video.videoHeight * scale;

            canvas.getContext('2d').drawImage(video, 0, 0, canvas.width, canvas.height);
            
            var img = document.createElement("img");

            img.src = canvas.toDataURL('image/png');
            
            $output.prepend(img);
        };

        $(initialize);
    }());

```
### Demo

[demo演示地址](http://www.zsfmyz.top/demo/1/)

大家可以用webstrom内置的服务器进行测试。

使用chrome浏览器需要一个服务器环境，否则canvas的toDataURL方法会报错。