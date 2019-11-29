---
title: HTML5 drag & drop 拖拽与拖放
date: 2019-09-07 11:09:34
categories: JavaScript
tags: JavaScript
keywords: html5, drag, drop
---

## 拖拽与拖放

> drop & drag 是html5自带的拖拽与拖放的api

### 语法：
所有需要被拖动的元素都要加上draggable属性，默认除了a，img等标签外不可拖动。

```
// html 行内绑定
<element ondrag="myScript">
// js 绑定元素
object.ondrag=function(){};
// 全局监听
object.addEventListener("drag", myScript);
```

<!-- more -->

###  相关重点api

- 拖拽元素上触发的事件（事件target是拖拽元素）
1. dragstart  被拖拽元素开始被拖拽时触发。
2. drag  被拖拽元素拖拽中触发
3. dragend  完成拖动时触发。

- 拖拽目标容器上的事件（事件target是目标容器）
4. dragenter  被拖拽元素在进入其原始容器内的时候触发。
5. dragleave  跟enter相对应。
6. dragover  在另一容器内时触发（实测，只要我开始拖动之后就一直触发，且该事件需要阻止浏览器默认事件，因为在其他容器内都是默认不能拖动的。）
7. drop 释放鼠标时候触发

### DataTransfer 是拖拽元素的一个媒介对象，可以设置一些功能
- dataTransfer.dropEffect：设置或返回拖放目标上允许发生的拖放行为。如果此设置的拖放行为不在effectAllowed属性设置的多种拖放行为之内，拖放操作将会失败。该属性值只允许none、copy、link、move值之一。

- dataTransfer.effectAllowed：设置或返回被拖动元素允许发生的拖动行为。该属性值可设置为none、copy、copyLink、copyMove、link、linkMove、move、all、uninitialized。

- dataTransfer.items：该属性返回DataTransferItems对象，该对象代表了拖动数据。

- dataTransfer.setDragImage(element x,y)：设置拖放操作的自定义图标。其中element设置自定义图标，x设置图标与鼠标在水平方向的距离；y设置图标与鼠标在垂直方向的距离。

- dataTransfer.addElement(element)：添加自定义图标。

- dataTransfer.types：该属性返回一个DOMStringList对象，该对象包括了存入dataTransfer中数据的所有类型。
- dataTransfer.getData(format)：获取DataTransfer对象中设置format格式的数据。其中format代表数据格式，data代表数据。

- dataTransfer.clearData([format])：清除DataTransfer对象中format格式的数据，如果省略format格式，则意味着清除DataTransfer对象中的全部数据。

### 例子

```
<span  draggable="true" ></span>  // 所有需要被拖动的元素都要加上draggable属性，默认除了a，img等标签外不可拖动。

//start drag end 中 event都是被拖拽的元素

document.addEventListener("dragstart", function (event) { 
    var id = $(event.target).prop('id'); 
    event.dataTransfer.dropEffect = 'move' //设置拖动样式
});

//dragover dragleave   dragenter drop 中event都代表拖放的容器元素  

document.addEventListener("dragover", function(event) {  
    // drop 阻止浏览器默认事件
    event.preventDefault();
    console.log("容器内");
});
```


下一章：