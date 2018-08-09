---
title: 小程序入门总结篇
date: 2018-08-09 16:20:20
tags: 小程序
categories: Front-End
---


## 一、小程序代码组成

![](https://upload-images.jianshu.io/upload_images/1519620-e7b4608440bc35a5.png?imageMogr2/auto-orient/)

**须知**

- `App()` 必须在 `app.js` 中注册，且不能注册多个。
- 编译后的代码包大小需小于 `1MB`，否则代码包将上传失败。
- 每个页面需要手动在`app.json`中进行注册，否则不能访问。
- `app.json`中`pages`数组的第一项代表小程序的初始页面，小程序中新增/减少页面，都需要对 `pages` 数组进行修改。
- 直接修改 `this.data`无效，无法改变页面的状态，还会造成数据不一致。
- 单次设置的数据不能超过`1024kB`，请尽量避免一次设置过多的数据。
- 不要尝试修改页面栈，会导致路由以及页面状态错误。
- `tabBar`只能配置最少2个、最多5个，`tab` 按数组的顺序排序。
- 小程序页面只能同时打开 5 个，如果交互流程较长难以支持。
- 同时只能存在 5 个`url`请求。
- 无法跳转小程序以外的`url`。
- 没有`cookie`。
- 没有开放加载`web`页面
- 没有a标签链接，不可嵌套`iframe`
- 没有`window`变量，但微信提供了wx全局方法集
- 事件绑定和条件渲染类似`Angular`，全部写在`WXML`中

### 1.1 主体

> 由`app.js`、`app.json`、`app.wxss`三个文件组成，放在根目录

- `app.js` 根目录的`app.js`很有用,因为在它内部注册的变量或方法，都是可以被所有页面获取到。可以监听并处理小程序的生命周期、声明全局变量。其余的`.js`文 件可以通过`var app = getApp()` 获取其实例，调用其中定义的方法和变量，但不可以调用生命周期的方法
- `app.json `是小程序的全局配置

```
pages 配置小程序的组成页面，第一个代表小程序的初始页面
window  设置小程序的状态栏、标题栏、导航条、窗口背景颜色
tabBar  配置小程序tab栏的样式和对应的页面
```

- `app.wxss` 是小程序的公共样式表，可以在其他`.wxss`文件中直接使用


> app.json

```javascript
"pages": [ //设置页面的路径
  "pages/index/index", //不需要写index.wxml,index.js,index,wxss,框架会自动寻找并整合
  "pages/logs/logs"
],
"window": { //设置默认窗口的表现形式
  "navigationBarBackgroundColor": "#ffffff", //顶部导航栏背景色
  "navigationBarTextStyle": "black", //顶部导航文字的颜色 black/white
  "navigationBarTitleText": "微信接口功能演示", //顶部导航的显示文字
  "backgroundColor": "#eeeeee", //窗口的背景色
  "backgroundTextStyle": "light", //下拉背景字体、loading 图的样式，仅支持 dark/light
  "enablePullDownRefresh": "false"， //是否支持下拉刷新 ，不支持的话就直接不写！
  "disableScroll": true, //  设置true不能上下滚动，true/false，注意！只能在 page.json 中有效，无法在 app.json 中设置该项。
},
"tabBar": { //底部tab或者顶部tab的表现，是个数组，最少配置2个，最多5个
  "list": [{ //设置tab的属性，最少2个，最多5个
    "pagePath": "pages/index/index", //点击底部 tab 跳转的路径
    "text": "首页", //tab 按钮上的文字
    "iconPath": "../img/a.png", //tab图片的路径
    "selectedIconPath": "../img/a.png" //tab 在当前页，也就是选中状态的路径
  }, {
    "pagePath": "pages/logs/logs",
    "text": "日志"
  }],
  "color": "red", //tab 的字体颜色
  "selectedColor": "#673ab7", //当前页 tab 的颜色，也就是选中页的
  "backgroundColor": "#2196f3", //tab 的背景色
  "borderStyle": "white", //边框的颜色 black/white
  "position": "bottom" //tab处于窗口的位置 top/bottom
  },
"networkTimeout": { //默认都是 60000 秒一分钟
    "request": 10000, //请求网络超时时间 10000 秒
    "downloadFile": 10000， //链接服务器超时时间 10000 秒
      "uploadFile": "10000", //上传图片 10000 秒
    "downloadFile": "10000" //下载图片超时时间 10000 秒
  },
"debug": true //项目上线后，建议关闭此项，或者不写此项
```

### 1.2 pages

> `pages`文件夹里是小程序的各个页面，每个界面一般都由`.wxml`、`.wxss`、`.js`、`.json`四个文件组成，四个文件必须是相同的名字和路径

- `.js` 是页面的脚本代码，通过`Page()`函数注册页面。可以指定页面的初始数据、生命周期、事件处理等 
- `.wxml` 是页面的布局文件，只能使用微信定义的组件 
- `.wxss` 是样式表，需要注意
  - 尺寸单位：`rpx` 可以根据屏幕的宽带进行自适应
  - 样式导入：`@import`导入外联样式表，如：`@import "test.wxss"`;
  - 定义在`app.wxss`中的全局样式，作用于每个页面。定义在`page`的`.wxss`文件只作用于对应的页面，会覆盖`app.wxss`中相同的选择器
- `.json` 是页面的配置文件，只能设置`app.json`中的`window`配置内容，会覆盖`app.json`中`window`的相同配置项，即使不配置任何东西也需要写`{}`,否则会报错

### 1.3 utils

> `utils` 里面包含一些公共的代码抽取的`js`文件，作为模块方便使用。模块通过`module.exports`对外暴露接口

- 其他地方使用是`var utils = require('../../utils/util.js')`;进行引用

## 二、视图层 WXML

### 2.1 数据绑定

> 传统的视图和数据绑定

![image.png](https://upload-images.jianshu.io/upload_images/1480597-5c24282ab5c92ea3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**那么微信小程序是通过什么方法来管理视图和对象绑定的呢？状态模式-单向数据流**

![image.png](https://upload-images.jianshu.io/upload_images/1480597-1212f4ef9f8b9b86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
> 数据流向是单向的，即视图变化不会影响对象状态

- 用户触发事件不仅要考虑当前UI元素更新，还会通过当前元素更新其他视图。
- 所以视图上的数据都必须用过事件传递给对象，只有用户操作视图，才能获取到数据，并更新对象状态

![image.png](https://upload-images.jianshu.io/upload_images/1480597-cf9e543ac2446352.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> `.wxml` 中的动态数据都来自`Page`中的`data`。数据绑定使用数据绑定使用双大括号将变量包起来，可以作用于内容、组件属性(需要在双引号之内)、控制属性(需要在双引号之内)、关键字(需要在双引号之内)

```javascript
Page({
    data: {
        message: "Hello",
        id:0,
        status: true
    }
})
```

```
<view> {{message}} </view>
<view id="item-{{id}}"> </view>
<view wx:if="{{status}}"> </view>
<view hidden="{{status}}"> </checkbox>
```

> 还可以进行简单的运算在`{{ }}`里

```
<view hidden="{{status ? true : false}}"> Hidden </view>
<view> {{a + b}} + c </view> 
<view wx:if="{{num > 6}}"> </view>
<view>{{"hello" + word}}</view>
```

### 2.2 条件渲染

> 用 `wx:if=”{{status}}”`来判断是否渲染代码块

```
<view wx:if="{{status}}"> isShow </view>
```

> 还可以添加else块

```
<view wx:if="{{num > 5}}"> A </view>
<view wx:elif="{{num > 2}}"> B </view>
<view wx:else> C </view>
```

### 2.3 列表渲染

- 在组件上使用 `wx:for`属性绑定一个数组，就可以渲染组件了 
- 默认情况下数组的当前下标变量名为`index`,当前项的变量名为`item`

```
<view wx:for="{{array}}">
  {{index}}: {{item.message}}
</view>
```

```javascript
Page({
  data: {
    array: ["AA","BB","CC"]
  }
})
```

> 使用 `wx:for-item` 可以指定数组当前元素的变量名，使用 `wx:for-index` 可以指定数组当前下标的变量名

```
<view wx:for="{{array}}" wx:for-index="num" wx:for-item="itemName">
  {{num}}: {{itemName}}
</view>
```

### 2.4 模板template

- `name` 定义组件模版的名称，引用模版的时候使用 is 属性指定模版的名字，is 可以进行简单的三目运算，需要传入模版需要的 `data` 数据。
- 因为模版拥有自己的作用域，所以只能使用 `data` 传入数据，而不接受双花括号的写法

```
<template name="msgItem">
<view>
<text> {{index}}: {{msg}} </text>
<text> Time: {{time}} </text>
</view>
</template>

<!-- 其他代码 -->
<template is="msgItem" data="{{...item}}"/>
```

### 2.5 公共模块的引用

- `WXML` 提供 `import` 和 `include` 两种文件引用方式。
- `import` 有作用域的概念，不能多重引用

```
<!-- B.wxml -->
<import src="a.wxml"/>

<!-- A.wxml -->
<template name="A">
 <text> A template </text>
</template>
```

> include 就可以多重引用

```
<!--引用 header、其中 header.wxml 中也引用了 footer.wxml-->
<include src="header.wxml"/>
<view> body </view>

<!-- header.wxml -->
<view> header </view>
<include src="footer.wxml"/>
```

### 2.6 事件

- 名称以 `bind` 开头的事件不阻止冒泡，名称以 `catch` 开头的事件冒泡是阻止的。如 `bindTap` 和 `catchTab`
- 在 `WXM`L 中，可以使用 `dataset` 定义` data `中的数据，会通过事件传递。它的事件以 `data- `开头，多个单词以 - 链接，如 `data-a-b`

## 三、生命周期
