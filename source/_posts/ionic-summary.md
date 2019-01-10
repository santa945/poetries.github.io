---
title: Ionic小结
date: 2019-01-10 18:10:24
tags: 
 - Ionic
 - Angular
categories: Front-End
---

> 折腾了一段时间`Typescript`、`angular`基础，终于可以学`Ionic4`了。


# 一、介绍

> `Ionic` 是一款基于 `Angular`、`Cordova` 的强大的 `HTML5` 移动应用开发框架 , 可以快速创建一 个跨平台的移动应用。可以快速开发移动 `App`、移动端 WEB 页面、微信公众平台应用，混 合 `app web `页面。

## 1.1 ionic 特点

- `ionic` 基于`Angular`语法，简单易学。
- `ionic` 是一个轻量级框架。
- `ionic` 完美的融合下一代移动框架，支持 `Angularjs` 的特性， `MVC` ，代码易维护。
- `ionic` 提供了漂亮的设计，通过 `SASS` 构建应用程序，它提供了很多 UI 组件来帮助开发者开发强大的应用。
- `ionic` 专注原生，让你看不出混合应用和原生的区别
- `ionic` 提供了强大的命令行工具。
- `ionic` 性能优越，运行速度快。

## 1.2 Ionic 和 Cordova(phonegap)、Angular 关系

> `ionic` = `Cordova` + `Angular` + `ionic CSS`


> `Ionic `是完全基于谷歌的 `Angular` 框架，在 `Angular` 基础上面做了一些封装，让我们可以更快 速和容易的开发移动的项目。`Ionic` 调用原生的功能是基于 `Cordova`,`Cordova` 提供了使用 `JavaScript` 调用 `Native` 功能，`ionic` 自己也封装了一套漂亮的 `CSS UI` 库。



# 二、环境搭建


## 2.1 Ionic初始化构建

> https://ionicframework.com/getting-started#cli

```bash
# 全局安装
npm install -g ionic
```

- `ionic info` (查看当前`ionic`的全部版本信息)

![image.png](https://upload-images.jianshu.io/upload_images/1480597-ddad8ba9108c0116.png)



```bash
ionic start myApp tabs # 建议使用初始化

cd myApp 

ionic serve
```


![image.png](https://upload-images.jianshu.io/upload_images/1480597-514b23b5ea8fd4ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- `ionic serve` 运行项目

## 2.2 Genymotion 安卓模拟器

> 使用方法 https://www.jianshu.com/p/aabc4fd01311

## 2.3 在IOS环境下体验

> 需要配备`mac`，安装`xcode`

```bash
# mac下需要添加sudo
sudo ionic cordova platform add ios

# 注意获取目录权限的问题
chmod -R 777 项目文件夹名
```

> 真机调试与发布需要`Apple`开发者账号

**打开xcode选择platform下中ios文件夹，点击运行项目**

![image.png](https://upload-images.jianshu.io/upload_images/1480597-a88aef237eed3ffe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 2.4 在安卓下体验

**1. 添加android**

```bash
ionic cordova platform add android

# 注意获取目录权限的问题
chmod -R 777 项目文件夹名

# 直接使用Android studio 进行调试链接

# 打包成apk拖入Genymotion调试
```

**2. 下载android studio 打开/platform/andriod文件**


![image.png](https://upload-images.jianshu.io/upload_images/1480597-8491b2c583d0cf3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**3. 然后连接android studio结合geny生成apk调试**

![image.png](https://upload-images.jianshu.io/upload_images/1480597-40771d7cc51e45f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.5 在浏览器/微信下体验


**1. 添加browser文件夹**


```bash
ionic cordova platform add browser
```

**2. 打包**

> `ionic cordova build browser`

![image.png](https://upload-images.jianshu.io/upload_images/1480597-bef93f8f07488d6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/1480597-28d5f0193d87167e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**3. 运行**

```bash
npm run serve

# 浏览器打开 http://localhost:8100
```

**4. 部署**

> 把`www`目录部署到服务器上即可

> 在微信下体验 注意微信`title`问题


## 2.6 ionic的常用命令

**1. 基本命令**

- `ionic g page myPage` 创建页面
- `ionic g provider MyData` 创建`provider`
- `ionic serve` 在浏览器中看
- `ionic platform add/remove android/ios` 添加删除平台
- `ionic build android/ios`  打包

**2. 辅助命令**

- `ionic info` 查看关于`ionic`的系统消息
- `ionic emulate android/ios` 模拟器中打开
- `ionic cordova plugin list` 查看插件安装列表

**3. 正式发布需要的命令**

- `ionic cordova platforms add android`
- `ionic cordova build android --release`


# 三、Ionic3.x+ 目录结构分析及创建组件


## 3.1 Ionic3.x 目录结构分析

**1. 整体目录结构**

![image.png](https://upload-images.jianshu.io/upload_images/1480597-ec4f35d6e3bab428.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- `hooks`:编译 `cordova` 时自定义的脚本命令，方便整合到我们的编译系统和版本控制系统中 
- `node_modules` :`node` 各类依赖包
- `resources` :`android/ios` 资源(更换图标和启动动画) 
- `src`:开发工作目录，页面、样式、脚本和图片都放在这个目录下
- `www`:静态文件
- `platforms`:生成 `android` 或者 `ios` 安装包路径(`platforms\android\build\outputs\apk:apk`
所在位置)执行 `cordova platform add android` 后会生成
- `plugins`:插件文件夹，里面放置各种 `cordova` 安装的插件
- `config.xml`: 打包成 `app` 的配置文件
- `package.json`: 配置项目的元数据和管理项目所需要的依赖
- `tsconfig.json`: `TypeScript` 项目的根目录，指定用来编译这个项目的根文件和编译选项 
- `tslint.json`:格式化和校验 `typescript`

**2. src目录**

![image.png](https://upload-images.jianshu.io/upload_images/1480597-ef37273ff97e9c99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- `app`:应用根目录
- `assets`:资源目录(静态文件(图片，js 框架) 
- `pages`:页面文件，放置编写的页面文件，包括:`html`，`scss`，`ts`
- `theme`:主题文件，里面有一个 `scss` 文件，设置主题信息。

**3. Ionic3.x src 下面文件分析**

![image.png](https://upload-images.jianshu.io/upload_images/1480597-509d5353e4bb462e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**4. app.module.ts 分析**

```js
//这个根模块会告诉 ionic 如何组装该应用

import { NgModule, ErrorHandler } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http';
import { IonicApp, IonicModule, IonicErrorHandler } from 'ionic-angular';
import { IonicStorageModule } from '@ionic/storage';
import { MyApp } from './app.component';

// 其他组件
import { HomePage } from '../pages/home/home';
import { DiscoveryPage } from '../pages/discovery/discovery';
import { ChatPage } from '../pages/chat/chat';
import { NotificationPage } from '../pages/notification/notification';
import { MorePage } from '../pages/more/more';
import { LoginPage } from '../pages/login/login';
import { TabsPage } from '../pages/tabs/tabs';

import { StatusBar } from '@ionic-native/status-bar';
import { SplashScreen } from '@ionic-native/splash-screen';
import { ApiProvider } from '../providers/api/api';

@NgModule({
  declarations: [
    MyApp,
    HomePage,
    DiscoveryPage,
    ChatPage,
    NotificationPage,
    MorePage,
    LoginPage,
    TabsPage
  ],
  imports: [
    BrowserModule,
    HttpClientModule,
    IonicModule.forRoot(MyApp),
    IonicStorageModule.forRoot()
  ],
  bootstrap: [IonicApp],
  entryComponents: [
    MyApp,
    HomePage,
    DiscoveryPage,
    ChatPage,
    NotificationPage,
    MorePage,
    LoginPage,
    TabsPage
  ],
  providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler},
    ApiProvider
  ]
})
export class AppModule {}
```

## 3.2 创建组件

1. `cd` 到我们的项目目录
2. 通过 `ionic g component` 组件名称创建组件

> 输入`ionic g`后，可以创建的组件如下

![image.png](https://upload-images.jianshu.io/upload_images/1480597-05bfbefd2e045576.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 创建完成组件以后会在 `src` 目录下面多一个 `components` 的目录，这个目录里面有我们用命令创建的所有的组件

![image.png](https://upload-images.jianshu.io/upload_images/1480597-c7d29e7ab4f12287.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 如果我们要使用这些组件必须在 `app.module.ts` 里面注册我们的模块，注册完成后就可以在 `pages` 里面的其页面里面使用这些组件


![image.png](https://upload-images.jianshu.io/upload_images/1480597-def738bc118984a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/1480597-a17b3b0709f0fd48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 3.3 创建页面以及页面跳转


**1. 创建页面**

```bash
ionic g page news
```

**2. 跳转**

```html
<button ion-button (click)="pushButton">执行button跳转</button>
```

![image.png](https://upload-images.jianshu.io/upload_images/1480597-e8a74fa40ff90073.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 四、Ionic页面生命周期

```js
// 页面被加载完成后调用的函数，切换页面时并不会进行重新加载，因为有cache的存在
onPageLoaded() {
  console.log('page 1: page loaded.');
}
 
// 页面即将进入的时候
onPageWillEnter() {
  // 在这里可以做页面初始化的一些事情
  console.log('page 1: page will enter.');
}
 
// 页面已经进入的时候
onPageDidEnter() {
  console.log('page 1: page did enter.');
}
 
// 页面即将离开的时候
onPageWillLeave() {
  console.log('page 1: page will leave.');
}
 
// 页面已经离开的时候
onPageDidLeave() {
  console.log('page 1: page did leave.');
}
 
// 从 DOM 中移除的时候执行的生命周期
onPageWillUnload() {
 
}
 
// 从 DOM 中移除执行完成的时候
onPageDidUnload() {
 
}
```


# 五、Ionic打包上线流程


# 六、更多参考

- [Ionic官方文档](https://ionicframework.com/docs/)
- [ionic官方GitHub](https://github.com/ionic-team)
- [Ionic菜鸟教程](http://www.runoob.com/ionic/ionic-tutorial.html)
