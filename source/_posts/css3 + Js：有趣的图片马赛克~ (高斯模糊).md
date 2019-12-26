---
title: css3 + Js：有趣的图片马赛克~ (高斯模糊)
date: 2018-05-21 16:59:13
categories: Css
tags: Css
keywords: css3, 马赛克, filter, blur
top: true
---

>  前两篇文介绍了css3的过滤器filter用来实现图片的高斯模糊效果，还有js拖拽的功能。
>  要实现局部模糊就要把两者结合起来，计算位移就可以了。

### *实现原理**
> 原理其实很简单，就是两张图的叠加。底部一张**清晰**的图，上面一个**高斯模糊**过的图，将**高斯模糊**的图当作上层元素的背景，利用背景定位使其只显示一部分，然后把这个高斯模糊的窗口放置在高清图的上层，背景图片的位置与下面的图片位置一致，这样看起来就像一张高清的图片打上了马赛克一样。讲起来不是很清楚，大家可以看一下代码。

<!-- more -->

## 基础版本
### HTML
```
<style type="text/css">
    #css_box {
        position: relative;
        width: 100%;
        height: 500px;
        background: url('https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1567844929804&di=75928af77a3db7ff54cd4eab49361bc0&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fblog%2F201307%2F19%2F20130719082039_k2NHG.jpeg') no-repeat;
    }
    
    #css_target {
        position: absolute;
        left: 0px;
        top: 0px;
        width: 200px;
        height: 200px;
        background: inherit;
        filter: blur(10px);
        /* 继承父元素的background属性 */
    }
</style>
<div class="demo post white-box article-type-post">
    <div id="css_box">
        <div id="css_target">

        </div>
    </div>
</div>

<script>
    var bar = document.getElementById("css_box");
    var target = document.getElementById("css_target");

    startDrag(bar, target, function(x, y) {
        target.style.backgroundPosition = (-1 * x) + "px " + (-1 * y) + "px";
    });
</script>    
```

### JS (当作JS文件引入)

```
var getCss = function(o, key) {
    return o.currentStyle ? o.currentStyle[key] : document.defaultView.getComputedStyle(o, false)[key];
};
// 拖拽
var startDrag = function(bar, target, callback) {
    maxX = parseInt(getCss(bar, "width")) - parseInt(getCss(target, "width"));
    maxY = parseInt(getCss(bar, "height")) - parseInt(getCss(target, "height"));
    var resX = 0,
        resY = 0;
    var params = {
        left: 0,
        top: 0,
        currentX: 0,
        currentY: 0,
        flag: false
    };

    if (getCss(target, "left") !== "auto") {
        params.left = getCss(target, "left");
    }

    if (getCss(target, "top") !== "auto") {
        params.top = getCss(target, "top");
    }

    target.onmousedown = function(event) {
        params.flag = true;
        if (!event) {
            event = window.event;
            bar.onselectstart = function() {
                return false;
            }
        }
        var e = event;
        params.currentX = e.clientX;
        params.currentY = e.clientY;
    };

    document.onmouseup = function() {
        params.flag = false;
        if (getCss(target, "left") !== "auto") {
            params.left = getCss(target, "left");
        }
        if (getCss(target, "top") !== "auto") {
            params.top = getCss(target, "top");
        }
    };

    document.onmousemove = function(event) {
        var e = event ? event : window.event;
        if (params.flag) {
            var nowX = e.clientX,
                nowY = e.clientY;

            var disX = nowX - params.currentX,
                disY = nowY - params.currentY;
            if ((parseInt(params.left) + disX) > maxX) {
                resX = maxX;
            } else if ((parseInt(params.left) + disX) < 0) {
                resX = 0;
            } else {
                resX = parseInt(params.left) + disX;
            }

            if ((parseInt(params.top) + disY) > maxY) {
                resY = maxY;
            } else if ((parseInt(params.top) + disY) < 0) {
                resY = 0;
            } else {
                resY = parseInt(params.top) + disY;
            }

            target.style.left = resX + 'px'

            target.style.top = resY + 'px'

            if (typeof callback == "function") {
                callback(resX, resY);
            }
            if (event.preventDefault) {
                event.preventDefault();
            }
            return false;
        }
    }
};

```


## 兼容移动端优化版本

### HTML
```
    <style type="text/css">
        #css_box {
            position: relative;
            touch-action: none;
            width: 100%;
            height: 500px;
            background: url('https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1567844929804&di=75928af77a3db7ff54cd4eab49361bc0&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fblog%2F201307%2F19%2F20130719082039_k2NHG.jpeg') no-repeat;
            background-size: contain;
        }
        
        #css_target {
            position: absolute;
            left: 0px;
            top: 0px;
            background: inherit;
            filter: blur(10px);
            touch-action: none;
            /* 继承父元素的background属性 */
        }
    </style>
    <div class="demo post white-box article-type-post">
        <h2 class="title">mosaic Demo <a style="color: #bd4117;" href="http://www.zsfmyz.top/Css/Css3%20+%20Js%EF%BC%9A%E5%9B%BE%E7%89%87%E7%9A%84%E5%B1%80%E9%83%A8%E6%A8%A1%E7%B3%8A%EF%BC%88%E9%A9%AC%E8%B5%9B%E5%85%8B%EF%BC%89/">传送门</a></h2>

        <div id="css_box">
            <div id="css_target">

            </div>
        </div>
        <p id="position_img">X: 0 px Y: 0 px</p>
    </div>
    <script>
     $(document).ready(function() {
        var bar = document.getElementById("css_box");
        var target = document.getElementById("css_target");
        var p_img = document.getElementById("position_img");

        // 兼容分辨率
        bar.style.height = (parseInt(getCss(bar, "width")) * 636 / 900) + 'px';
        target.style.backgroundSize = parseInt(getCss(bar, "width")) + 'px ' + (parseInt(getCss(bar, "width")) * 636 / 900) + 'px';

        startDrag(bar, target, function(x, y) {
            target.style.backgroundPosition = (-1 * x) + "px " + (-1 * y) + "px";
            p_img.innerText = "X: " + target.style.left + " Y: " + target.style.top; 
        });
    })
    </script>  
```
### JS (当作JS文件引入)

```
<script src="https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js"></script>
<script>
        // 拖拽
        var startDrag = function(bar, target, callback) {
            var getCss = function(o, key) {
                return o.currentStyle ? o.currentStyle[key] : document.defaultView.getComputedStyle(o, false)[key];
            };

            var down = function(event) {
                // 兼容分辨率
                bar.style.height = (parseInt(getCss(bar, "width")) * 636 / 900) + 'px';
                target.style.backgroundSize = parseInt(getCss(bar, "width")) + 'px ' + (parseInt(getCss(bar, "width")) * 636 / 900) + 'px';

                params.flag = true;
                if (!event) {
                    event = window.event;
                    bar.onselectstart = function() {
                        return false;
                    }
                }
                var e = event;
                params.currentX = e.clientX || e.changedTouches[0].clientX;
                params.currentY = e.clientY || e.changedTouches[0].clientY;
            };

            var up = function() {
                params.flag = false;
                if (getCss(target, "left") !== "auto") {
                    params.left = getCss(target, "left");
                }
                if (getCss(target, "top") !== "auto") {
                    params.top = getCss(target, "top");
                }
            };

            var move = function(event) {
                var e = event ? event : window.event;
                if (params.flag) {
                    var nowX = e.clientX || e.changedTouches[0].clientX,
                        nowY = e.clientY || e.changedTouches[0].clientY;
                    var disX = nowX - params.currentX,
                        disY = nowY - params.currentY;
                    if ((parseInt(params.left) + disX) > maxX) {
                        resX = maxX;
                    } else if ((parseInt(params.left) + disX) < 0) {
                        resX = 0;
                    } else {
                        resX = parseInt(params.left) + disX;
                    }

                    if ((parseInt(params.top) + disY) > maxY) {
                        resY = maxY;
                    } else if ((parseInt(params.top) + disY) < 0) {
                        resY = 0;
                    } else {
                        resY = parseInt(params.top) + disY;
                    }

                    target.style.left = resX + 'px'

                    target.style.top = resY + 'px'

                    if (typeof callback == "function") {
                        callback(resX, resY);
                    }
                    if (event.preventDefault) {
                        event.preventDefault();
                    }
                    return false;
                }
            }

            var resX = 0,
                resY = 0;
            var params = {
                left: 0,
                top: 0,
                currentX: 0,
                currentY: 0,
                flag: false
            };

            if (getCss(target, "left") !== "auto") {
                params.left = getCss(target, "left");
            }

            if (getCss(target, "top") !== "auto") {
                params.top = getCss(target, "top");
            }

            if ((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
                console.log("mobile");
                target.style.width = '100px';
                target.style.height = '100px';
                maxX = parseInt(getCss(bar, "width")) - parseInt(getCss(target, "width"));
                maxY = parseInt(getCss(bar, "height")) - parseInt(getCss(target, "height"));

                target.ontouchstart = down

                document.ontouchend = up

                document.ontouchmove = move
            } else {
                console.log("pc");
                target.style.width = '150px';
                target.style.height = '150px';
                maxX = parseInt(getCss(bar, "width")) - parseInt(getCss(target, "width"));
                maxY = parseInt(getCss(bar, "height")) - parseInt(getCss(target, "height"));

                target.onmousedown = down

                document.onmouseup = up

                document.onmousemove = move
            }

        };
</script>
```

> 以上就是局部模糊的实现方法。可以直接套用。将JS部分当作文件引入。否则要把初始化函数放在函数声明的后面。

最后可查看实际效果：[demo演示地址](http://www.zsfmyz.top/demo/mosaic/)


*借鉴于张大神*
