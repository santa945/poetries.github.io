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

```js
 mainWindow.webContents.openDevTools();
 ```
 
![image.png](https://upload-images.jianshu.io/upload_images/1480597-62c31b57f8b94838.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 四、Electron 模块介绍

> `Electron` 模块介绍、`remote` 模块、通 过 `BrowserWindow` 打开新窗口

## 4.1 Electron 主进程和渲染进程中的模块

![image.png](https://upload-images.jianshu.io/upload_images/1480597-3c15bb39e7080862.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 4.2 Electron remote 模块

> `remote` 模块提供了一种在渲染进程(网页)和主进程之间进行进程间通讯(`IPC`)的简便途径

> `Electron` 中, 与 `GUI` 相关的模块(如 `dialog`, `menu` 等)只存在于主进程，而不在渲染进程中 。为了能从渲染进程中使用它们，需要用`ipc`模块来给主进程发送进程间消息。使用 `remote` 模块，可以调用主进程对象的方法，而无需显式地发送进程间消息，这类似于 `Java` 的 `RMI`

## 4.3 通过BrowserWindow 打开新窗口

> `Electron` 渲染进程中通过 `remote` 模块调用主进程中的 `BrowserWindow` 打开新窗口


```js
// 主进程代码


const electron = require('electron'); 

// 控制应用生命周期的模块 
const {app} = electron;

// 创建本地浏览器窗口的模块 
const {BrowserWindow} = electron;

// 指向窗口对象的一个全局引用，如果没有这个引用，那么当该 javascript 对象被垃圾回收 的
// 时候该窗口将会自动关闭
let win;

function createWindow() {
    // 创建一个新的浏览器窗口
    win = new BrowserWindow({width: 1104, height: 620});//570+50
    
    // 并且装载应用的 index.html 页面
    win.loadURL(`file://${__dirname}/html/index.html`);
    
    // 打开开发工具页面
    win.webContents.openDevTools();
    
    //当窗口关闭时调用的方法
    win.on('closed', () => {
        // 解除窗口对象的引用，通常而言如果应用支持多个窗口的话，你会在一个数组里 // 存放窗口对象，在窗口关闭的时候应当删除相应的元素。
        win = null;
    });
}

// 当 Electron 完成初始化并且已经创建了浏览器窗口，则该方法将会被调用。
// 有些 API 只能在该事件发生后才能被使用
app.on('ready', createWindow);

// 当所有的窗口被关闭后退出应用 
app.on('window-all-closed', () => {
    // 对于 OS X 系统，应用和相应的菜单栏会一直激活直到用户通过 Cmd + Q 显式退出 
    if (process.platform !== 'darwin') {
        app.quit(); 
    }
});


app.on('activate', () => {
    // 对于 OS X 系统，当 dock 图标被点击后会重新创建一个 app 窗口，并且不会有其他
    // 窗口打开
    if (win === null) {
        createWindow(); 
    }
});

// 在这个文件后面你可以直接包含你应用特定的由主进程运行的代码。 
// 也可以把这些代码放在另一个文件中然后在这里导入
```

```js
// 渲染进程代码

const btn = document.querySelector('#btn');
const path = require('path');
const BrowerWindow = require('electron').remote.BrowserWindow;

btn.onclick = () => {
    win = new BrowerWindow({ 
        width: 300,
        height: 200, 
        frame: false, 
        transparent: true // fullscreen:true
    }) 

    win.loadURL(path.join('file:',__dirname,'news.html'));

    win.on('close',()=>{win = null});
}
```

# 五、自定义顶部菜单/右键菜单


## 5.1 自定义软件顶部菜单

> `Electron` 中 `Menu` 模块可以用来创建原生菜单，它可用作应用菜单和 `context` 菜单

> 这个模块是一个主进程的模块，并且可以通过 `remote` 模块给渲染进程调用

```js
const { Menu } = require('electron');

let template = [
{
    label: '文件',
    submenu: [ 
        {
            label: '新建窗口', 
            click: () => {
                console.log('aaa') 
            }
        }, 
        {
            type: 'separator' 
        },
        {
            label: '打开文件',
            accelerator:'ctrl+x', 
            click: () => {
                console.log('bbb') 
            }
        },
    ] 
},
{
    label :'编辑',
    submenu: [ 
        {
            role:'cut', 
            label:'剪切'
        },
        {
            role:'copy', 
            label:'复制'
        }
    ]
}
]

var m=Menu.buildFromTemplate(template); 

Menu.setApplicationMenu(m);
```

## 5.2 自定义右键菜单

```js
window.addEventListener('contextmenu', (e) = >{
    e.preventDefault();
    m.popup({
        window: remote.getCurrentWindow()
    });
},
false);
```

# 六、进程通信

## 6.1 主进程与渲染进程之间的通信

> 有时候我们想在渲染进程中通过一个事件去执行主进程里面的方法。或者在渲染进程中通知 主进程处理事件，主进程处理完成后广播一个事件让渲染进程去处理一些事情。这个时候就 用到了主进程和渲染进程之间的相互通信

> `Electron` 主进程，和渲染进程的通信主要用到两个模块:`ipcMain` 和 `ipcRenderer`

- `ipcMain`:当在主进程中使用时，它处理从渲染器进程(网页)发送出来的异步和同步信息,当然也有可能从主进程向渲染进程发送消息。
- `ipcRenderer`: 使用它提供的一些方法从渲染进程 (`web` 页面) 发送同步或异步的消息到主进程。 也可以接收主进程回复的消息

**场景 1:渲染进程给主进程发送异步消息**

```js
//渲染进程
const { ipcRenderer } = require('electron')

ipcRenderer.send('msg',{name:'张三'}); //异步
```

```js
//主进程
const { ipcMain } = require('electron');

ipcMain.on('msg',(event,arg) => {
    
})
```

**场景 2:渲染进程给主进程发送异步消息，主进程接收到异步消息以后通知渲染进程:**

```js
//渲染进程
const { ipcRenderer } = require('electron')

ipcRenderer.send('msg',{name:'张三'}); //异步

```

```js
//主进程
const { ipcMain } = require('electron');

ipcMain.on('msg',(event,arg) => { 
    event.sender.send('reply', 'pong');
})
```

```js
//渲染进程
const { ipcRenderer } = require('electron')

ipcRenderer.on('reply', function(event, arg) { 
    console.log(arg); // prints "pong"
});
```

**场景 3:渲染进程给主进程发送同步消息**

```js
//渲染进程
const { ipcRenderer } = require('electron')

const msg = ipcRenderer.sendSync('msg-a');

console.log(msg)
```

```js
//主进程 
ipcMain.on('msg-a',(event)=> {
    event.returnValue = 'hello';
})
```

**通过主进程渲染进程的通信改造右键菜单**

> @TODO

## 6.2 渲染进程与渲染进程之间的通信

**1. Electron 渲染进程通过 localstorage 给另一个渲染进程传值**

```js
localStorage.setItem(key,value);

localStorage.getItem(key);
```

**2. 通过 BrowserWindow 和 webContents 模块实现渲染进 程和渲染进程的通信**

> `webContents` 是一个事件发出者.它负责渲染并控制网页，也是 `BrowserWindow` 对象的属性

**需要了解的几个知识点**

1. 获取当前窗口的 `id`

```js
const winId = BrowserWindow.getFocusedWindow().id;
```

2. 监听当前窗口加载完成的事件

```js
win.webContents.on('did-finish-load',(event) => {
    
})
```

3. 同一窗口之间广播数据

```js
win.webContents.on('did-finish-load',(event) => {
    win.webContents.send('msg',winId,'我是 index.html 的数据');
})
```

4. 通过 `id` 查找窗口

```js
let win = BrowserWindow.fromId(winId);
```

# 七、Electron Shell 模块

> `Electron Shell` 模块在用户默认浏览器 中打开 `URL` 以及 `Electron DOM webview` 标签

> `shell` 模块提供了集成其他桌面客户端的关联功能.

```js
var shell = require('shell');

shell.openExternal('https://github.com')
```

**Electron DOM <webview> 标签**

> `Webview` 与 `iframe` 有点相似，但是与 `iframe` 不同, `webview` 和你的应用运行的是不同的进程。它不拥有渲染进程的权限，并且应用和嵌入内容之间的交互全部都是异步的。因为这能 保证应用的安全性不受嵌入内容的影响。

```html
<webview id="webview" src="https://www.itying.com" style="position:fixed; width:100%; height:100%">
</webview>
```

# 八、Electron dialog 弹出框

> `dialog` 模块提供了 `api` 来展示原生的系统对话框，例如打开文件框，`alert` 框， 所以 `web` 应用可以给用户带来跟系统应用相同的体验


![image.png](https://upload-images.jianshu.io/upload_images/1480597-de7cfb527bf5ea19.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```js
dialog.showErrorBox('title','content');
```

![image.png](https://upload-images.jianshu.io/upload_images/1480597-f94a34889a4b1f1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```js
dialog.showMessageBox({
    type: 'info',
    title: 'message',
    message: 'hello',
    buttons: ['ok', 'cancel']
},
(index) = >{
    if (index == 0) {
        console.log('You click ok.');
    } else {
        console.log('You click cancel');
    }
})
```

```js
// 打开目录  选择文件

dialog.showOpenDialog({
    properties: ['openFile', 'openDirectory']
},
(files) = >{
    console.log(files);
})
```

```js
dialog.showSaveDialog({
    title: 'save some file',
    filters: [{
        name: 'Images',
        extensions: ['jpg', 'png', 'gif']
    },
    {
        name: 'Movies',
        extensions: ['mkv', 'avi', 'mp4']
    },
    {
        name: 'Custom File Type',
        extensions: ['as']
    },
    {
        name: 'All Files',
        extensions: ['*']
    }]
},
(filename) = >{
    console.log(filename);
})
```

![image.png](https://upload-images.jianshu.io/upload_images/1480597-281ce997d8a71eee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 九、系统托盘、托盘右键菜单、托盘图标闪烁 

> 系统托盘 托盘右键菜单、托 盘图标闪烁 点击右上角关闭按钮隐 藏到托盘(仿杀毒软件)

**1. Electron 系统托盘、任务通知栏图标介绍**

![image.png](https://upload-images.jianshu.io/upload_images/1480597-d8cb64eb94d030a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**2. Electron 创建任务栏图标以及任务栏图标右键菜单**

```js
var {
    Menu, Tray, app, BrowserWindow
} = require('electron');

const path = require('path');

var appIcon = new Tray(path.join(__dirname, 'lover.png'));

const menu = Menu.buildFromTemplate([
    {
        label: '设置',
        click: function() {} //打开相应页面 
    },
    {
        label: '帮助',
        click: function() {}
    },
    {
        label: '关于',
        click: function() {}
    },
    {
        label: '退出',
        click: function() { 
            // BrowserWindow.getFocusedWindow().webContents().send('close-main-window');
            app.quit();
    }
}])
appIcon.setToolTip('my best app');
appIcon.setContextMenu(menu);
```

**3. 监听任务栏图标的单击、双击事件**

```js
var {
    Menu, Tray, app, BrowserWindow
} = require('electron');

var appIcon = new Tray(path.join(__dirname, 'lover.png'));

appIcon.on('double-click', () = >{
    console.log(win);
    win.show();
})
```

**4. Electron 点击右上角关闭按钮隐藏任务栏图标**


```js
const win = BrowserWindow.getFocusedWindow();

win.on('close', (e) = >{

    console.log(win.isFocused());
    
    if (!win.isFocused()) {
        win = null;
    } else {
        e.preventDefault();/*阻止应用退出*/
        win.hide();/*隐藏当前窗口*/
    }
})
```

**5. Electron 实现任务栏闪烁图标**

```js
timer = setInterval(function() {
    count++;
    if (count % 2 == 0) {
        appIcon.setImage(path.join(__dirname, 'empty.ico'))
    } else {
        appIcon.setImage(path.join(__dirname, 'lover.png'))
    }
},
500);
```

# 十、消息通知、监听网络变 化、网络变化弹出通知框

**1. Electron 实现消息通知**

> `Electron` 里面的消息通知是基于 `h5` 的通知 `api` 实现的

![image.png](https://upload-images.jianshu.io/upload_images/1480597-b09035ca49b73ba3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```js
const option = {
    title: 'title',
    body: 'body',
    icon: path.join('main-process/favicon2.ico')
}
const myNotification = new window.Notification(option.title, option);

myNotification.onclick = () = >{
    console.log('clicked');
}
```

**2. Electron 监听网络变化**

```js
 window.addEventListener('online', function(){}); window.addEventListener('offline', function(){});
 ```
 
 # 十一、注册全局快捷键/剪切板事件/nativeImage 模块
 
 > `Electron` 注册全局快捷键 (`globalShortcut`) 以及 `clipboard` 剪 切板事件以及 `nativeImage` 模块(实现类似播放器点击机器码自动复制功 能)
 
 **1. Electron 注册全局快捷键(globalShortcut)**
 
```js
var app = require('app');
var globalShortcut = require('electron').globalShortcut;

app.on('ready',function() {
    // Register a 'ctrl+x' shortcut listener.
    var ret = globalShortcut.register('ctrl+x',function() {
        console.log('ctrl+x is pressed');
    }) 
    
    if (!ret) {
        console.log('registration failed');
    } 
    // Check whether a shortcut is registered.
    console.log(globalShortcut.isRegistered('ctrl+x'));
});

app.on('will-quit',function() {
    // Unregister a shortcut.
    globalShortcut.unregister('ctrl+x'); 
    
    // Unregister all shortcuts.
    globalShortcut.unregisterAll();
});
```

**2. clipboard 剪切板事件 clipboard 模块以及 nativeImage 模块**

```js
const {
    clipboard,
    nativeImage
} = require('electron');

clipboard.writeText("这是一个test");

console.log(clipboard.readText());

let img = nativeImage.createFromPath('static/favicon2.ico');

clipboard.writeImage(img);

const imgDataURL = clipboard.readImage().toDataURL();
const img3 = new Image();

img3.src = imgDataURL;
document.body.appendChild(img3);
```


# 十二、结合electron-vue

## 12.1 electron-vue 的使用

**1. electron-vue 的一些资源**

> https://github.com/SimulatedGREG/electron-vue


`Electron-vue` 文档 https://simulatedgreg.gitbooks.io/electron-vue/content/cn

**2. electron-vue 环境搭建、创建项目**

```bash
npm install -g vue-cli

vue init simulatedgreg/electron-vue my-project

cd my-project

yarn # or npm install

yarn run dev # or npm run dev
```

**3. electron-vue 目录结构分析**

![image.png](https://upload-images.jianshu.io/upload_images/1480597-3137d0001e34cf1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 12.2 electron-vue 中使用 sass/ElementUi

**1. electron-vue UI 框架 ElementUi 的使用**

> http://element-cn.eleme.io/#/zh-CN


**2. electron-vue 中使用 sass**

- [electron-vue 中使用 sass](https://simulatedgreg.gitbooks.io/electron-vue/content/cn/using_pre-processors.html)


```bash
# 安装 sass-loader:

npm install --save-dev sass-loader node-sass
```

```html
<!--vue 文件中修改 style 为如下代码:-->

<style lang="scss"> 
    body {
        /* SCSS */ 
    }
</style>
```

## 12.3 electron-vue 中隐藏顶部菜单隐藏

> electron-vue 中隐藏顶部菜单隐藏顶部最大化、最小化、关闭按钮 自定最大化、最小化 、关闭按钮

**1. electron-vue 中隐藏顶部菜单**

```js
mainWindow.setMenu(null)
```

**2. electron-vue 中隐藏关闭 最大化 最小化按钮**

```js
mainWindow = new BrowserWindow({
    height: 620,
    useContentSize: true,
    width: 1280 frame: false /*去掉顶部导航 去掉关闭按钮 最大化最小化按钮*/
})
```

**3 .electron-vue 自定义关闭/最大化最小化按钮**

```js
ipc.on('window-min',function() {
    mainWindow.minimize();
})

//登录窗口最大化 
ipc.on('window-max',function(){
    if (mainWindow.isMaximized()) {
        mainWindow.restore();
    } else {
        mainWindow.maximize();
    }
}) 

ipc.on('window-close',function() {
    mainWindow.close();
})
```

**4. electron-vue 自定义导航可拖拽**

- 可拖拽的 `css`: `-webkit-app-region: drag; `
- 不可拖拽的 `css`:  `-webkit-app-region: no-drag;`


# 十三、更多参考

- [electron常用工具](https://github.com/poetries/electron-wiki)
- [electron中文文档](https://wizardforcel.gitbooks.io/electron-doc/content/faq/electron-faq.html)

