---
layout: '[layout]'
title: JavaScript：call，apply，bind的区别及实现
date: 2019-01-11 15:10:03
categories: JavaScript
tags: JavaScript
keywords: call, apply, bind, 原理实现
---
> call，apply，bind的区别
> 这三个函数的左右都是为了指定当前的this。

不同点：

1. call，apply 两者都是对函数的直接调用，bind的返回值仍然是一个函数。
 	举例：`a.call(b)  或者  a.apply(b)   而bind需要 a.bind(b)()这样才能执行`
2. call和apply的传参方式不同，第一个值都是this的指向，第二个举例

<!-- more -->	

```
a.call(b, 参数1，参数2，参数3)
a.apply(b, [参数1，参数2，参数3])

//同样的bind的传参方式和call相同，但又因为bind返回的是函数，所以我们可以像正常函数传参一样

a.bind(b)(参数1，参数2，参数3)
```
以上就是三者的差别。

接下来我们实现他们的功能函数。

1. call：
> call的传入参数是（ctx,...[]）
```
Function.prototype.mycall = funciton (ctx) {
	if (typeof this != "function") {
		throw new TypeError('error');
	}
	ctx = ctx || window;
	ctx.fn = this;
	var args = ...arguments.splice(1);
	var result = ctx.fn(args);
	delete ctx.fn;
	return result;
}
```
2. apply:
> apply的传入参数跟call不同，第二个参数是数组。

```
Function.prototype.myapply = funciton (ctx) {
	if (typeof this != 'function') {
		throw new TypeError('error');
	}
	ctx = ctx || window;
	ctx.fn = this;
	var result;
	if (arguments[1]) {
		result = ctx.fn(arguments[1]);
	} else {
		result = ctx.fn();
	}
	return result;
}
```
3. bind 返回的是个函数
> bind 返回的是个函数，同样可以执行传参 ，我们可以用myapply实现

```
Funtion.prototype.myBind = function(ctx,...args1){
	return (...args2) => {
		this.myapply(ctx, args1.concat(args2))
	}
}
```
