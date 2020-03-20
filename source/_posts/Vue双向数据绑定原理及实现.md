---
title: Vue双向数据绑定原理及实现
date: 2020-03-14 16:59:13
categories: Vue
tags: Vue
keywords: Vue, 双向数据绑定, Vue响应式
---

#### 说明

vue实现双向绑定原理，主要是利用Object.defineProperty 来给实例data的属性添加 setter和getter. 
并通过发布订阅模式（一对多的依赖关系，当状态发生改变，它的所有依赖都将被通知）来实现响应。

这个环节中包含了三个部分

- Observer 用来监听拦截data的属性为监察者。 

- Dep用来添加订阅者，为订阅器

- Watcher 就是订阅者

监察者通过 Dep 向 Watcher发布更新消息

 <!-- more -->
#### 简单实现

那么首先

1. 通过对set和get的拦截，在get阶段进行依赖收集，在set阶段对通知该属性上所啊绑定的依赖。

如下我们就已经实现了一个简单的双向绑定了。

我们将data的value属性绑定上set和get，通过 _value 来进行操作。

```
<!-- HTML部分 -->

<input type="text" id="inp" oninput="inputFn(this.value)">
<div id='div'></div>

<!-- JS部分 -->
var inp = document.getElementById('inp');
var div = document.getElementById('div');
var data = {
    value:''
}
  Object.defineProperty(data, 'value', {
    enumerable: true,
    configurable: true,
    set: function (newValue) {
        this._value = newValue; 
        div.innerText = data._value = value; //watcher
    },
    get: function () {
        return this._value; 
    }
})
function inputFn(value) {
  data._value = value;
}

```


如果只是实现一个简单的双向绑定那么上面的代码就已经实现了。

 
#### 进一步完善模拟vue实现

首先我们将watcher抽出来 备用

```
  function watcher(params) {
    div.innerText = inp.value = params; // 派发watcher
  }
```

声明一个vm来模拟vue的实例,并初始化。

```
    var vm = {

        //类似vue实例上的data
        data: {
            value: ''
        }, 

        // vue私有, _data的所有属性为data中的所有属性被改造为 getter/setter 之后的。
        _data: {
            value: ''
        }, 

        // 代理到vm对象上，可以实现vm.value
        value: '', 

        //value的订阅器用来收集订阅者 
        valueWatchers:[] 
    }

```

遍历其data上的属性 进行改造 这里我们还是只举一个例子

```
  // 利用 Object.defineProperty 定义一个属性 (eg：value) 描述符为存取描述符的属性
  Object.defineProperty(vm._data, 'value', {
      enumerable: true,
      configurable: true,
      set: function (newValue) { //set 派发watchers
        vm.data.value = newValue; 
        vm.valueWatchers.map(fn => fn(newValue));
      },
      get: function () { 
          
          // 收集wachter vue中会在compile解析器中通过 显示调用 (this.xxx) 来触发get进行收集
          vm.valueWatchers.length = 0; 
          vm.valueWatchers.push(watcher); 
          return vm.data.value; 
      }
  })

    <!--直接通过显示调用来触发get进行绑定 vue中是在compile解析器中来进行这一步-->
    vm._data.value 

```

进行到这儿也已经实现了绑定，但是我们平时使用vue ，都是可以直接通过 this.xxx来获取和定义数据

那么我们还需要进行一步Proxy 代理

```

  Object.defineProperty(vm, 'value', {
      enumerable: true,
      configurable: true,
      set: function (newValue) {
          this._data.value = newValue; //借助
      },
      get: function () {
          return this._data.value; 
      }
  })

```

这样我们就把vm._data.value 代理到vm.value上了，可以通过其直接操作了。

那么按照官方的写法

```

  function proxy (target, sourceKey, key) {
      Object.defineProperty(target, key, {
          enumerable: true,
          configurable: true,
          get() {
              return this[sourceKey][key];
          },
          set(val) {
              this[sourceKey][key] = val;
          }
      });
  }
    
  proxy(vm, '_data', 'value');

```

#### 完善后的完整代码

以下为整个页面，可以直接运行

```

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>双向绑定简单实现</title>
</head>
<body>
<input type="text" id="inp" oninput="inputFn(this.value)">
<br>
<input type="text" id="inp2" oninput="inputFn(this.value)">
<div id='div'></div>
<script>
    var inp = document.getElementById('inp');
    var inp2 = document.getElementById('inp2');
    var div = document.getElementById('div');

    
    function inputFn(value) {
        div.innerText = vm.value = value;
    }

    function watcher(params) {
        console.log(1)
        div.innerText = inp.value = params; // 派发watcher
    }

    function watcher2(params) {
        console.log(2)

        div.innerText = inp2.value = params; // 派发watcher
    }

    function proxy (target, sourceKey, key) {
        Object.defineProperty(target, key, {
            enumerable: true,
            configurable: true,
            get() {
                return this[sourceKey][key];
            },
            set(val) {
                this[sourceKey][key] = val;
            }
        });
    }

    var vm = {
        data: {},
        _data: {},
        value: '', 
        valueWatchers: [] 
    }
    Object.defineProperty(vm._data, 'value', {
        enumerable: true,
        configurable: true,
        set: function (newValue) {
            vm.data.value = newValue; 
            vm.valueWatchers.map(fn => fn(newValue));
        },
        get: function () {
            vm.valueWatchers = []; //防止重复添加, 
            vm.valueWatchers.push(watcher); 
            vm.valueWatchers.push(watcher2); 
            return vm.data.value; 
        }
    })

    proxy(vm, '_data', 'value');

    vm.value;  //显示调用绑定

</script>
</body>
</html>
```

#### 解释

再多讲一点。实际上vue在初始化的时候是用解析器解析过程中将wathcer进行绑定的。

它会利用一个全局的Dep.target = watcher 

然后在get收集中，只收集全局上Dep.target, 添加完毕后会重新初始化全局Dep.target = null;

类似如下操作

```

    Dep.target = watcher;
    vm.value;    // 触发get => Dep.target && valueWatchers.push(Dep.target);
    Dep.target = null;


```

这样也会防止我们在调用时触发get重复去添加watcher。

而我们的例子中只是每次都初始化为[]. 实际订阅器也不只是一个watcher数组。 

此例跟官方实现还是有很多差距，只是简单模拟。

