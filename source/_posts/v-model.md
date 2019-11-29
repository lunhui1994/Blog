---
title: v-model
date: 2019-08-26 16:59:13
categories: Vue
tags: Vue
keywords: v-model
---
最近学习vue，从最基础的文档看起。到目前基础已经看完。有两个问题很迷茫。

 1. vue的动态参数（2.6新增）。demo一直报错。分析原因是因为vue不允许动态添加根一级的变量。（不理解）
 2. 组件中v-model的使用文档中说的让我有点绕。不过已经顺过来了，所以来分享一下。

<!-- more -->

## 官方给出的例子是这样。(可以直接使用)

```
HTML:

<custom-input v-model="searchText"></custom-input>

JS:
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})

```

## v-model可以改写为这种形式，两者完全等价。

```
<input v-model="searchText">

等价于：

<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"> //（ $event.target.value为事件当前目标上的value值。）
```

第二种写法：
  1. 在input上绑定了input事件，通过触发input事件，执行给searchText赋值语句。
  2. v-bind将searchText赋值给input标签的value属性。


再看官方的例子：
```
<custom-input v-model="searchText"></custom-input>
```
等价于：
```
<custom-input  
	v-bind:value="searchText"
  	v-on:input="searchText = $event"   //$event 为组件内部抛出来的值。（这个如果不明白可以看组件的自定义事件部分）
></custom-input> 
```
再加上官方的js写法：

```
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})

```

 1. 我们在组件内的input上监听了input事件，并且用该input事件对组件上的（自定义）input事件进行触发（searchText = $event"）；
 2. 将$  event.target.value作为参数传给 	v-on:input="searchText =  $ event "，$ event === $ event.target.value。执行语句searchText被赋值。
 3. 组件上的v-bind:value = "searchText" 是将searchText的值绑定到组件内部的props里的value上。
 4. 组件内部将props中的value 绑定到input的value上。就这样完成了一次双向绑定。

而根据官方v-model的两种写法，组件上转换回v-modal的写法。就完成了。最终结果就如开头所示。