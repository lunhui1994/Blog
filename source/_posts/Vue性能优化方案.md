---
title: Vue性能优化方案
date: 2020-03-10 17:05:27
categories: Vue
tags: Vue
keywords: Vue性能优化
---

### 背景

> 总结一下使用Vue涉及到的性能优化方法，从我实践和查阅资源总结出来的集合。

简介：
1. **v-if 和 v-show**
    1. **v-if**
    2. **v-show**
2. **computed 和 watch**


<!-- more -->
  
### 一、 v-if 和 v-show

- 一般使用过vue或者angular及其相同框架的人都会知道他们的区别。

##### v-if

它是真正的条件判断语句，会根据条件对条件内的组件进行重建和销毁。
它是惰性的，在第一次判断条件为true时，才会去创建相应的组件。即初始化为false时，该条件内组件不会加载。

##### v-show

它仅仅是通过控制css的display属性来控制组件的显示和隐藏。
所以，无论是否为false，该组件都会在页面构建时加载。

#### 优化：

我们可以在需要*频繁切换*显示隐藏的组件上使用v-show，在只需要*一次或少数判断*的时候使用v-if。
  

### 二、 computed 和 watch

- computed 和 watch 都可以对个变量进行监听依赖，但是用法上还是有很大区别的。

如 name = 'xiaoming'
```
<div>{{ name.split('').reverse().join('') }}</div>
```
如上我们想把name反转，如此写在html中略显混乱，并对阅读不友好。

##### computed

它表示计算属性，目的是用来方便控制需要计算之后再进行展示的数据。
computed属性会被缓存，只有computed所依赖的属性发生变化之后，才会触发computed重新计算。

使用computed

```
<div>{{ nameReverse }}</div>
// ...
    data() {
        return {
            name: 'xiaoming'
        };
    },
    computed: {
        nameReverse: {
            get: function () {
                    return this.name.split('').reverse().join('');
                },
            set: function (newValue) {
                    //同时computed也有set函数，可以在计算变量赋值时触发。
                    //this.name = ...
                },
        }
    }
// ...
```
##### watch

watch事实上就是一个监听函数，通过监听该变量来执行一些更加复杂的操作。它是比computed更耗费性能的。

注意watch在进入页面之后是不会立即触发的。

使用watch

```
<div>{{ nameReverse }}</div>
data() {
    return {
        name: 'xiaoming',
        nameReverse: '' //定义
    };
},
method: {
    nameHandler: function (newQuestion, oldQuestion) {
            this.nameReverse = newQuestion.split('').reverse().join('');
        },
}
watch: {
    // 'obj.name': 'xxxhandler' 也可以监听深层属性  
    name:{ 
        handler: "nameHandler",
        immediate: true //该属性设置之后watch会以当前值立即触发回调函数。
    } 
}
```

#### 优化：

所以我们可以根据computed和watch的不同用法，来区分使用场景。
当我们仅仅需要获取一个通过一系列操作后的计算结果，我们应该使用computed。
当我们需要在属性变化时进行更加复杂的操作，比如异步操作，设置中间状态进行节流等，这些都需要使用watch实现。


 
### 三、 v-for 和 key 避免使用v-if

- 我们都知道使用v-for的时候要在每个节点加上唯一的key值，否则即采用就地复用的原则，同时避免使用v-if使用computed来代替。

##### 不加key

官方文档对key的解释很清楚，key的用途主要用于虚拟DOM对比时对vnode进行辨别。

那么不加key的就地复用是什么意思呢?
即当节点被删除或者位置被移动，节点不会被删除或者替换。
虚拟DOM将会遍历vnode，直接更改其内容。（该操作是高效的）
但是其只适用于渲染列表Dom结构极其简单的情况。


##### 加key

加上key之后，对vnode增加了独特的标记，虚拟DOM的对比将不会再遍历vnodes，而是直接采用key的映射进行对比。
将会基于key冲i性能排序元素顺序，删除key不存在的节点，替换和删除节点。
它也能触发完整的生命周期函数。

##### v-if和computed

如果我们的列表只需要渲染其中的一部分，我们可以使用computed

#### 优化：
官方建议：建议尽可能在使用 v-for 时提供 key attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
注意： key值需使用使用string和数值型基础类型，不要使用对象数组等复杂类型。同时key值需要独特性。可以使用列如id属性作为key。
  


### 四、 keepalive

使用keep-alive包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们；
keep-alive是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在父组件链中；

#### 使用keepalive
当组件在 <keep-alive> 内被切换，它的 activated 和 deactivated 这两个生命周期钩子函数将会被对应执行。
可以保存组件的状态，比如一个下拉框，选择了第二项，切换其他组件再返回时依旧是第二个组件。
防止组件重复的销毁和重建。


```
<!-- include表示匹配要缓存的组件名称，exclude表示不缓存的组件名称，exclude优先级更高。 max表示最大缓存数 -->

<keep-alive :include="" :exclude="" :max="">
  <comp-a v-if="a > 1"></comp-a>
  <comp-b v-else></comp-b>
</keep-alive>

```

#### keepalive 多了解一点

keepalive是vue的一个组件，它自己的生命周期有created和destoryed,。

**created**用来创建一个cache用来保存需要缓存的Vnode节点，和一个keys数组用来保存cache中对应的key值。
**destoryed**用来遍历缓存节点并逐个调用$destory()进行销毁。

**rander**首先会获取第一个子组件，获取到名称，然后进行include和exclude过滤，判断是否匹配缓存。
如果不符合缓存条件，直接返回vnode，如果符合，则以名称为key值去keys中查找。
若命中，则返回cache中的缓存，并将cache中key位置的缓存删除，并添加到cache的末尾。
若没命中，则进行缓存并返回vnode，并将其添加至cache尾部。

返回前会将它们的keepAlive属性设置为true用于后面调用activated与deactivated函数。 

**watch**同时keepalive会监听include和exclude的变化，不存在的key将进行销毁并以出cache列表。

keepalive 组件本身并不会生成节点，在keep-alive中，设置了 abstract: true ，该属性表示此组件为抽象组件意思就是不会生成实际节点，包括<trai>。

[相关荐文]("https://www.cnblogs.com/wangjiachen666/p/11497200.html")

#### 优化：
如果你的组件需要保持切换前的状态就需要加上keepalive，或者对于频繁切换的组件也需要加上，以保证它避免被销毁，保存渲染状态，提高性能。


### 五、 Object.freeze 冻结对象，长列表优化

  
有时候我们会有一些比较长的列表要进行展示，而且这些数据展示完之后并不会发生变化。
但是vue对数据都是使用了Object.defineProperty进行了数据劫持，所以初始化时就会造成大量的无用劫持。
这时候我们就可以使用Object.freeze进行冻结，冻结之后数据就不会再被修改了。

```
this.list = Object.freeze(list);
```