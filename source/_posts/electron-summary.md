---
title: 初识Electron
date: 2019-01-06 17:40:43
tags: 
   - Electron
   - Node
categories: Front-End
---


# 一、前言

- `NW.js` 和 `Electron` 都可以用前端的知识来开发桌面应用。`NW.js` 和 `Electron `起初是同一 个作者开发。后来种种原因分为两个产品。一个命名为 `NW.js`(英特尔公司提供技术支持)、 另一命名为 `Electron`(Github 公司提供技术支持)。
- `NW.js` 和 `Electron` 可以用 `Nodejs` 中几乎所有的模块。`NW.js` 和 `Electron `不仅可以把 `html` 写的 `web` 页面打包成跨平台可以安装到电脑上面的软件，也可以通过 `javascript` 访问操作 系统原生的 `UI` 和 `Api`(控制窗口、添加菜单项目、托盘应用菜单、读写文件、访问剪贴板)。

>  `github` 的 `atom` 编辑器、微软的 `vscode` 编辑器，包括阿里内部的一些 软件也是用 `electron` 开发的


### 1.1 electron介绍

**1. Electron 是由谁开发的?**

> `Electron` 是由 `Github` 开发

**2.  Electron 是什么?**

> `Electron` 是一个用 `HTML`，`CSS` 和 `JavaScript` 来构建跨平台桌面应用程序的一个开源库

**3. Electron 把 HTML，CSS 和 JavaScript 组合的程序构建为跨平台桌面应用程序的原理 是什么?**

> 原理为 `Electron` 通过将 `Chromium` 和 `Node.js` 合并到同一个运行时环境中，并将其打包为 `Mac`，`Windows` 和 `Linux` 系统下的应用来实现这一目的。

**4. Electron 何时出现的，为什么会出现?**

> `Electron` 于 `2013` 年作为构建 `Atom` 的框架而被开发出来。这两个项目在 `2014` 春季开源。 (Atom:为 Github 上可编程的文本编辑器)

**一些历史:**
- `2013` 年 `4` 月 `Atom Shell` 项目启动 。
- `2014` 年 `5` 月 `Atom Shell` 被开源 。
- `2015` 年 `4` 月 `Atom Shell` 被重命名为 `Electron` 
- `2016` 年 `5` 月 `Electron` 发布了 `v1.0.0` 版本 

**5. Electron 当前流行程度?**

> 目前 `Electron` 已成为开源开发者、初创企业和老牌公司常用的开发工具。

**6. Electron 当前由那些人在维护支持?**

> `Electron` 当前由 `Github` 上的一支团队和一群活跃的贡献者维护。有些贡献者是独立开发者，有些则在用 `Electron` 构建应用的大型公司里工作。

**7. Electron 新版本多久发布一次?**

> `Electron` 的版本发布相当频繁。每当 `Chromium`、`Node.js` 有重要的 `bug` 修复，新 `API` 或是版本更新时 `Electron` 会发布新版本。

- 一般 `Chromium` 发行新的稳定版后的一到两周之内，`Electron` 中 `Chromium` 的版本会对其进行更新，具体时间根据升级所需的工作量而定。
一般 `Node.js` 发行新的稳定版一个月后，`Electron` 中 `Node.js` 的版本会对其进行更新，具 体时间根据升级所需的工作量而定。

**8. Electron 的核心理念是什么?**

> `Electron` 的核心理念是:保持 `Electron` 的体积小和可持续性开发。
如:为了保持 `Electron` 的小巧 (文件体积) 和可持续性开发 (以防依赖库和 `API` 的泛滥) ， `Electron` 限制了所使用的核心项目的数量。
比如 `Electron` 只用了 `Chromium` 的渲染库而不是其全部组件。这使得升级 `Chromium` 更加容易，但也意味着 `Electron` 缺少了 `Google Chrome` 里的一些浏览器相关的特性。 添加到 `Electron` 的新功能应该主要是原生 `API`。 如果可以的话，一个功能应该尽可能的成 为一个 `Node.js` 模块。

**9. Electron 当前的最新版本为多少?**

> `Electron` 当前的最新版本为 `4.0.1` (当前时间为 `2019` 年 `1` 月 `6` 号)

# 二、环境搭建

**1. 安装 electron**

```bash
npm install -g electron
```

**2. 克隆一个仓库、快速启动一个项目**

```bash
# 克隆示例项目的仓库
git clone https://github.com/electron/electron-quick-start

# 进入这个仓库
cd electron-quick-start

# 安装依赖并运行
npm install && npm start
```

**3. 手动搭建一个 electron 项目**

1. 新建一个项目目录 例如: `electrondemo01`
2. 在 `electrondemo01` 目录下面新建三个文件: `index.html`、`main.js` 、`package.json `
3. `index.html` 里面用 `css` 进行布局(以前怎么写现在还是怎么写)
4. 在 `main.js` 中写如下代码

```js
var electron =require('electron'); //electron 对象的引用
const app=electron.app; //BrowserWindow 类的引用
const BrowserWindow=electron.BrowserWindow;

let mainWindow=null; //监听应用准备完成的事件 app.on('ready',function(){

//监听应用准备完成的事件
app.on('ready',function(){
    //创建窗口
    mainWindow=new BrowserWindow({width: 800, height: 600}); mainWindow.loadFile('index.html');
    
    mainWindow.on('closed', function () {
        mainWindow = null; })
    })
})

//监听所有窗口关闭的事件 
app.on('window-all-closed', function () {
    // On OS X it is common for applications and their menu bar // to stay active until the user quits explicitly with Cmd + Q 
    if (process.platform !== 'darwin') {
        app.quit(); 
    }
})
```

5. 运行

```bash
electron . #注意:命令后面有个点
```

**4. electron-forge 搭建一个 electron 项目**

> `electron-forge` 相当于 `electron` 的一个脚手架，可以让我们更方便的创建、运行、打包 `electron` 项目

```bash
npm install -g electron-forge 

electron-forge init my-new-app 

cd my-new-app

npm start
```

# 三、Electron 运行流程

## 3.1 Electron 运行的流程

![image.png](https://upload-images.jianshu.io/upload_images/1480597-7d07da8dccab3159.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3.2 Electron 主进程和渲染进程

- `Electron` 运行 `package.json` 的 `main` 脚本的进程被称为主进程。 
- 在主进程中运行的脚本通过创建 `web` 页面来展示用户界面。 一个 `Electron` 应用总是有且只有一个主进程。
- 由于 `Electron` 使用了 `Chromium`(谷歌浏览器)来展示 `web` 页面，所以 `Chromium` 的 多进程架构也被使用到。 每个 `Electron` 中的 `web` 页面运行在它自己的渲染进程中。
- 主进程使用 `BrowserWindow` 实例创建页面。每个 `BrowserWindow` 实例都在自己的渲 染进程里运行页面。 当一个 `BrowserWindow`实例被销毁后，相应的渲染进程也会被终止

![image.png](https://upload-images.jianshu.io/upload_images/1480597-ed01221126a5fe76.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/1480597-d84ba41d44a3e672.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 进程:进程是计算机中的程序关于某数据集合上的一次运行活动，是 系统进行资源分配和调度的基本单位，是操作系统结构的基础。
- 线程:在一个程序里的一个执行路线就叫做线程(`thread`)。更准确的定义是: 线程是“一个进程内部的控制序列”。
- 线程和进程:一个程序至少有一个进程,一个进程至少有一个线程

## 3.3 Electron 渲染进程中通过 Nodejs 读取本地文件

> 在普通的浏览器中，`web`页面通常在一个沙盒环境中运行，不被允许去接触原生的资源。 然而 `Electron` 的用户在 `Node.js` 的 `API`支持下可以在页面中和操作系统进行一些底层交 互。
`Nodejs` 在主进程和渲染进程中都可以使用。渲染进程因为安全限制，不能直接操作生 `GUI`。虽然如此，因为集成了 Nodejs，渲染进程也有了操作系统底层 `API`的能力，`Nodejs` 中常用的 `Path`、`fs`、`Crypto` 等模块在 `Electron` 可以直接使用，方便我们处理链接、路径、 文件 `MD5` 等，同时 `npm` 还有成千上万的模块供我们选择。

```js
var fs = require('fs');
var content = document.getElementById('content'); 
var button = document.getElementById('button');

button.addEventListener('click',function(e){
    fs.readFile('package.json','utf8',function(err,data){ 
        content.textContent = data;
        console.log(data);
    }); 
});
```

## 3.4 Electron 开启调试模式

```bash
  mainWindow.webContents.openDevTools();
```
 
![image.png](https://upload-images.jianshu.io/upload_images/1480597-62c31b57f8b94838.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

