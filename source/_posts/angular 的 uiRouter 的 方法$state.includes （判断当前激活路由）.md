---
layout: angularJS
title: angular 的 uiRouter 的 方法$state.includes （判断当前激活路由）
date: 2018-02-27 18:08:30
categories: Angular
tags: Angular
keywords: uiRouter, angular, $state.includes
---

### angular 的 uiRouter 的 方法$state.includes （判断当前激活路由）
作用：
 1. 该方法用于判断当前激活的也就是地址栏的路由地址是哪个路由。
 
 2.  比如   $state.includes('app') 那么如果页面地址为“www.baidu.com#/app” 或者 “www.baidu.com#/app/xxx” 的时候，该方法的值会返回true。
    （*一般我们定义的路由和地址栏地址是相互对应的，方便管理。举例也是在app.xxx对应app/xxx这样设置路由的情况下*）
 
 3.  如激活的路由为 app.page.page1 那么
    ```
    $state.includes('app')              //返回 true
    $state.includes('app.page')         //返回 true
    $state.includes('app.page.page1')   //返回 true
    ```
<!-- more -->

用法：
> 知道了它的作用，接下来看看它的使用背景。
> 1. 我们可以用来激活当前menu的状态。即使当前路由对应的菜单高亮或激活状态。

举例：

```
<div ng-init="menu_flag= !($state.includes('app.page1') || $state.includes('app.page2'))"> 主菜单</div>
<div ng-hide="menu_flag">
    <div ng-class="{active: $state.includes('app.page1')}">
        <a href="javascript:;" ui-sref="app.page1">子菜单一</a>
    </div>
    <div ng-class="{active: $state.includes('app.page2')}">
        <a href="javascript:;" ui-sref="app.page2">子菜单二</a>
    </div>
</div>

// ......  n个类似结构组成的菜单

<div ng-init="menu_flag1= !($state.includes('app.page3') || $state.includes('app.page4'))"> 主菜单</div>
<div ng-hide="menu_flag1">
    <div ng-class="{active: $state.includes('app.page3')}">
        <a href="javascript:;" ui-sref="app.page1">子菜单一</a>
    </div>
    <div ng-class="{active: $state.includes('app.page4')}">
        <a href="javascript:;" ui-sref="app.page2">子菜单二</a>
    </div>
</div>
```

1. 如上我们的菜单构成是由若干个类似结构构成，主menu控制若干个子menu。active是我们定义的激活菜单的css类名，当我们选中某个菜单时激活该菜单。即可借用$state.includes()来实现该功能。

2. 同时，当我们的主menu要控制闭合和展开的话，当我们刷新的时候，我们通过判断`($state.includes('app.page3') || $state.includes('app.page4'))` 的值来在刷新之后判断该主菜单是否闭合。
3. 当然我们也可以这样定义我们的路由。当然我们也可以这样定义我们的路由。例如： 一层菜单为 app.menu1 , 该主菜单下路由定义为

| 路由名称 | 一级路由定义 | 二级路由定义 |
|--|--|--|
|  一层菜单 | app.menu1 |  |
|  二层菜单 |  | app.menu1.menu1_1 |
|  二层菜单 |  | app.menu1.menu1_2 |

这样的话我们在主菜单闭合上只需要判断 `$state.includes(app.menu1)` 即可。


