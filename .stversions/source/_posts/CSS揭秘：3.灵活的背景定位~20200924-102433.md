---
title: CSS揭秘：3.灵活的背景定位
date: 2020-06-13 18:14:27
categories: Css
tags: Css
keywords: 灵活的背景定位, background-position, background-origin, calc()
---
# 灵活的背景定位
> 背景知识：`background-position`的扩展语法，`background-origin，calc()`

## background-position扩展语法
1. **background-position扩展语法：** css3 中background-position 语法可以通过在偏移量前指定关键字，来设置四条边的偏移量。
	```css
	background-positon: right 20px bottom 10px;
	```
## background-origin
2.  **background-origin：** css3 中 background-origin 可以指定背景图片的显示范围，默认以padding-box为准，即padding的外边沿。此时背景图片的位置将和padding一致。通常此方案更适合开发需求。
	```css
	padding:20px;
	background-origin: content-box;
	background-position: 100% 100%;
	```
## calc()
3. **calc()：** 允许填入 任意 `+ - * /` 四则运算组合的表达式。
	```css
	background-position: calc(100% - 20px) calc(100% - 10px);
	```
> 扩展：`border-box` `边框`的外边沿；`padding-box` `内边距`的外边沿； `content-box` `内容`的外边沿。

### 原效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061200291226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)
### 最终效果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200612002948787.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)