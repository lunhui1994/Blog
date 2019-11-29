---
title: angular中ng-repeat的track by 的使用（$index）
date: 2017-11-27 18:14:27
categories: Angular
tags: Angular
keywords: ng-repeat, angular, track by
---

### 在angular中使用ng-repeat时数组中有重复元素

-  当我们在循环的数组中存在有重复的元素时候，angular的ng-repeat就会报错，那是因为其不允许collection有相同的id（相同的元素会形成相同的id）出现。而基本的数据类型它的id就是它自身的值。

<!-- more -->
-  我们要避免这种情况通常会使用`track by $index` 来让其生成自己不同的id，这样是最常用的直接通过索引来生成id。我们也可以通过自己设置业务上的id，然后用其进行遍历`track by item.id`.

-  总结一下，解决重复问题的方法就是`item in items track by $index`

###  使用$index会出现的问题。

-  我们使用`$index`不仅仅是为了避免重复元素的问题，有时候会被使用`$index`的索引来进行一部分操作，这里有一个坑需要注意。

> 当我们使用`$index`的时候，下列情况会出现`$index`跟原序列不匹配的情况

 1. 当我们改变列表的顺序的时候
 2. 当我们在列表中插入或者删除的时候


----------


-  由于`$index`会跟随item上移下移，或者随之被删除。（例如你把列表第二条和第一条位置互换，这时候列表现在第一条的$index依旧为2，第二条还是原来的1）这时候你再使用`$index`传值就不在是新数组的索引了，不再匹配。

*所以使用$index的时候要特别注意这些问题*

