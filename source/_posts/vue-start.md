---
title: 初识vue与环境搭建（一）
date: 2018-08-26 13:12:32
tags: Vue
categories: Front-End
---


## 一、初识


- `Vue`框架是一个`MVVM`框架，
`Module`和`view`是双向绑定的。`vue`没有控制器的概念，它的核心思想是数据驱动，状态管理，以及组件化。
- 因此在我们js编程中，不会去操作`DOM`、`class`，更多的关注我们的数据层面。去改变一个变量，通过变量来控制我们的视图，通过事件绑定、状态管理来进一步渲染视图。
- `MVVM`框架的特点是没有控制器，通过`view`和`module`来进行交互，实际上底层已经帮我们封装了

### 1.1概况

- `Vue`本身不是一个框架
- `Vue`结合周边的生态构成一个灵活的、渐进式的框架


### 1.2核心思想

- 数据驱动【只关注数据层面】
- 组件化

### 1.3双向数据绑定

> `Object.defineProperty`在双向绑定中起来很重要作用


```html
<input type="text" id="userName">
<span id="uName">
```

```javascript
var obj = {}

object.defineProerty(obj,"userName",{
    get:()=>{
        
    },
    set:(val)=>{
        $("#uName").innerHTML = val
        $("#userName").value = val;
    }
})
$("#userName").on("keyup",function(){
    obj.userName = event.target.value
})
```

### 1.4模板语法

- `Mustache`语法：`{{msg}}`
- `Html`语法：`v-html=""`
- 绑定属性：`v-bind:id=""`
- 使用表达式：`{{ ok?'yes':"no"}}`
- 文本赋值：`v-text=""`
- 指令：`v-if=""`
- 过滤器：`{{ message | capitalize }}`和`v-bind:id="rawld|formatld"`

### 1.5 `Class`和`Style`绑定

- 对象语法：`v-bind:class="{ active:isActive,'text-danger':hasError }"`
- 数组语法： 


```html
<div v-bind:class="[activeClass, errorClass]">

<script>
    new Vue({
        data:{
            activeClass: "active",
            errorClass: 'text-danger'
        }   
    }) 
</script>
```

- `style`绑定-对象语法 `v-bind:style="{color:activeColor,fontSize:fontSize+"px"}"`

