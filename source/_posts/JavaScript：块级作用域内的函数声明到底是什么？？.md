---
title: JavaScript：块级作用域内的函数声明到底是什么？？
date: 2020-04-16 12:19:13
categories: JavaScript
tags: JavaScript
keywords: 函数声明, 块级作用域
---

### 说明
在一篇文章里看到这样一个问题。[文章地址](https://mp.weixin.qq.com/s/MlKRNfK3blGJA7bfXzdsNg)
> 文章中会扩展一些其他的内容， 大家可以看过之后再来看我这篇个人的总结

函数声明写在块级作用域中（ES6）
```javascript
	var a = 0;
	if(true){
	    a = 1;
	    function a(){}
	    a = 21;
	    console.log("里面",a);
	}
	console.log("外部",a);
	
```

请问输出是什么？ 答案是：**里面21 外面1**。

![小朋友，ni'you](https://imgconvert.csdnimg.cn/aHR0cDovLzViMDk4OGU1OTUyMjUuY2RuLnNvaHVjcy5jb20vaW1hZ2VzLzIwMjAwMzIxL2Q3MTRmMzU1YjUwYTRlYjU4YWIyZTI3MTliYTNkMjUzLmpwZWc?x-oss-process=image/format,png#pic_center)
正如我一样，看到答案的我蒙蔽了，然后就继续阅读完全文，感觉还是有点不太明白。最后经过我查询资料我得到了答案。

### 转换结果
代码转化如下：

```javascript

   		var a;
        var a = 0;
        if (true) {
            let a = function a() { 
            }
            a = 1; 
            window.a = a;  //此处为原函数声明的位置
            a = 21;
            console.log("里面",a);
        }
        console.log("外部",a);
```

其实，函数声明放在块级作用域内做了以下几件事。
so， 我想看到了转换后的代码，估计大家就豁然开朗了。

1. {}内部修改的是let定义的块级a，跟外部没关系。

2. 所以 a = 1 的时候，外部其实还是为0。

3. 外部的全局a原本为0， 被window.a = a 同步为了1.

4. a =21 块级a变成了21，全局a无变化还是1.

**那么为啥会这么转换呢????**  


### 实质
1.  函数声明会被提升到**块级作用域**顶部。

2.  使用了类似let的方式定义了一个**块级作用域**的函数**同名变量**，并赋值。（个人总结）

3.  函数声明的**变量被声明**到了**全局作用域**（或者函数作用域）顶部。

4.  在函数**声明的位置**，会将目前**块级作用域内的变量的值**，**同步**到全局作用域（函数作用域）下。

### 解释
如下标记 1234 ，我想这样是最直观的。

（window 代表的外层的作用域上下文，意思是if块所在的作用域，因为此处为全局，所以为window）。
	
```javascript

   		var a; // 3
        var a = 0;
        if (true) {
            let a = function a() { 
            	// 1，2
            }
            a = 1; 
            window.a = a;  // 4：此处为原函数声明的位置
            a = 21;
            console.log("里面",a);
        }
        console.log("外部",a);
```

### 扩展

```javascript

if(true) {
	function a( ){}
}

```
转换为es5为


```javascript
"use strict";

if (true) {
  var a = function a() {};
}

```

所以经过我的查找，所有人都不推荐直接在块级作用域内进行函数声明，如果非要，就请使用es5的函数表达式写法。

