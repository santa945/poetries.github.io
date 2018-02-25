---
title: 实现数据的双向绑定mvvm-剖析Vue的原理
date: 2018-02-25 17:12:32
tags: MVVM
categories: Front-End
---

> 完成的效果

```html
<div id="mvvm-app">
    <input type="text" v-model="word">
    <p>{{word}}</p>
    <button v-on:click="sayHi">change model</button>
</div>

<script src="observer.js"></script>
<script src="watcher.js"></script>
<script src="compile.js"></script>
<script src="mvvm.js"></script>
<script>
var vm = new MVVM({
    el: '#mvvm-app',
    data: {
        word: 'Hello World!'
    },
    methods: {
        sayHi: function() {
            this.word = 'Hi, everybody!';
        }
    }
    });
</script>
```

![](https://github.com/honeydlp/mvvm/raw/master/defineProperty/img/1.gif)

## 一、几种实现双向绑定的做法

> 目前几种主流的`mvc(vm)`框架都实现了单向数据绑定，而我所理解的双向数据绑定无非就是在单向绑定的基础上给可输入元素（`input`、`textare`等）添加了`change(input)`事件，来动态修改`model`和 `view`，并没有多高深。所以无需太过介怀是实现的单向或双向绑定。

- 发布者-订阅者模式（`backbone.js`）
- 脏值检查（`angular.js`） 
- 数据劫持（`vue.js`） 

### 1.1 发布者-订阅者模式

- 一般通过`sub`, `pub`的方式实现数据和视图的绑定监听，更新数据方式通常做法是 `vm.set('property', value)`，[这里有篇文章讲的比较详细](http://www.html-js.com/article/Study-of-twoway-data-binding-JavaScript-talk-about-JavaScript-every-day)
- 这种方式现在毕竟太low了，我们更希望通过 `vm.property = value `这种方式更新数据，同时自动更新视图，于是有了下面两种方式

### 1.2 脏值检查

> `angular.js` 是通过脏值检测的方式比对数据是否有变更，来决定是否更新视图，最简单的方式就是通过 `setInterval()` 定时轮询检测数据变动，当然Google不会这么low，angular只有在指定的事件触发时进入脏值检测，大致如下：

- `DOM`事件，譬如用户输入文本，点击按钮等。( `ng-click` ) 
- `XHR`响应事件 ( `$http` ) 
- 浏览器`Location`变更事件 ( `$location` ) 
- `Timer`事件( `$timeout` , `$interval` ) 
- 执行 `$digest()` 或 `$apply()`

### 1.3 数据劫持 

- `vue.js`则是采用数据劫持结合发布者-订阅者模式的方式，通过`Object.defineProperty()`来劫持各个属性的`setter`，`getter`，在数据变动时发布消息给订阅者，触发相应的监听回调。

## 三、实现MVVM

> `MVVM`作为数据绑定的入口，整合`Observer`、`Compile`和`Watcher`三者，通过`Observer`来监听自己的`model`数据变化，通过`Compile`来解析编译模板指令，最终利用`Watcher`搭起`Observer`和`Compile`之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(`input`) -> 数据`model`变更的双向绑定效果。

- 一个简单的`MVVM`构造器是这样子：

```javascript
function MVVM(options) {
    this.$options = options;
    var data = this._data = this.$options.data;
    observe(data, this);
    this.$compile = new Compile(options.el || document.body, this)
}
```

- 但是这里有个问题，从代码中可看出监听的数据对象是`options.data`，每次需要更新视图，则必须通过`var vm = new MVVM({data:{name: 'kindeng'}}); vm._data.name = 'dmq'; `这样的方式来改变数据。
- 显然不符合我们一开始的期望，我们所期望的调用方式应该是这样的：
`var vm = new MVVM({data: {name: 'kindeng'}}); vm.name = 'dmq';`
- 所以这里需要给`MVVM`实例添加一个属性代理的方法，使访问`vm`的属性代理为访问`vm._data`的属性，改造后的代码如下：

```javascript
function MVVM(options) {
    this.$options = options;
    var data = this._data = this.$options.data, me = this;
    // 属性代理，实现 vm.xxx -> vm._data.xxx
    Object.keys(data).forEach(function(key) {
        me._proxy(key);
    });
    observe(data, this);
    this.$compile = new Compile(options.el || document.body, this)
}

MVVM.prototype = {
	_proxy: function(key) {
		var me = this;
        Object.defineProperty(me, key, {
            configurable: false,
            enumerable: true,
            get: function proxyGetter() {
                return me._data[key];
            },
            set: function proxySetter(newVal) {
                me._data[key] = newVal;
            }
        });
	}
}
```

完整代码 https://github.com/poetries/mvvm/blob/master/mvvm.js

- 这里主要还是利用了`Object.defineProperty()`这个方法来劫持了`vm`实例对象的属性的读写权，使读写`vm`实例的属性转成读写了`vm._data`的属性值，达到鱼目混珠的效果

