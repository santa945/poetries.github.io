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


> `app.json`


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




