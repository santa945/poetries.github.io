---
title: RN之开发GitHub适配IOS、Android总结篇
date: 2018-12-02 16:50:12
tags: 
 - RN
 - react
categories: Front-End
---


## 一、环境搭建

### 1.1 React Native环境搭建


- **注意**：不要使用 `cnpm`！`cnpm` 安装的模块路径比较奇怪，`packager` 不能正常识别！

```
npm install -g yarn react-native-cli
```

**1. 创建新项目**

> `init` 命令默认会创建最新的版本，而目前最新的`0.45` 及以上版本需要下载 `boost` 等几个第三方库编译。这些库在国内即便翻墙也很难下载成功，导致很多人无法运行`iOS`项目。可以暂时创建`0.44.3`的版本

```
react-native init MyApp --version 0.44.3
```

**2. 编译并运行 React Native 应用**

1). **运行方式一** 在你的项目目录中运行`react-native run-ios`

```
cd MyApp
react-native run-ios
```

2). **运行方式二** 在`xCode`中运行

> 打开`xcode`选择项目中`myApp/ios/myApp.xcodeproj`，然后点击左上角运行即可


> 更多详情 https://reactnative.cn/docs/getting-started.html

**3. 远程调试**

- `ctrl + R`刷新
- `ctrl + D` 选择对应的工具调试

![image.png](https://upload-images.jianshu.io/upload_images/1480597-f7ed3063bc1956c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 在浏览器中打开 http://localhost:8081/debugger-ui

![image.png](https://upload-images.jianshu.io/upload_images/1480597-4e63e2074634f9fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 1.2 移除vscode装饰器报错

> 点击`Visual Studio Code`左下角的配置按钮。在搜索框内输入“experimentalDecorators”，发现竟然能够找到选项，如下

```
"javascript.implicitProjectConfig.experimentalDecorators": false
```

试着将`false`改为`true`，重启`Visual Studio Code`

> https://blog.csdn.net/yiifaa/article/details/78862507


## 二、矢量图标的运用

https://github.com/oblador/react-native-vector-icons

> `react-native-vector-icons` 是可以直接使用图片名就能加载图片的第三方,类似于`web的`iconfont`矢量图，使用很方便, 你不需要在工程文件夹里塞各种图片, 节省很多空间,下面就来看看怎么使用吧


```
npm install react-native-vector-icons --save
npm install rnpm -g
```

### 2.1 android平台

**1. 自动配置**

```bash
react-native link react-native-vector-icons
# 或者
npm install -g rnpm
rnpm link react-native-vector-icons
```

> 会为你配置好所有，但是这是成功的情况下，你不需要操心任何事，但是往往不能如愿。如果你这步成功了，而且能够正常运行，下面这些你就可以跳过


**2. 手动配置**

- 第一步：复制字体文件（这一步千万不能忘记，不然就算运行成功你也看不到图标）

> 找到项目`node_modules/react-native-vector-icons/Fonts`，里面有很多已经内置的图标库字体文件，依照自己的需求，复制你需要的字体文件到 `android/app/src/main/assets/fonts`，（如果没有这个目录就自行创建）

![](https://upload-images.jianshu.io/upload_images/1967155-f9368d8824726fb1.png)

- 第二步：配置 `android/settings.gradle`

在现有的代码基础上添加如下代码


```
include ':react-native-vector-icons'
project(':react-native-vector-icons').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-vector-icons/android')
```

- 第三步：配置`android/app/build.gradle`

```js
dependencies {
    compile project(':react-native-vector-icons') //添加
    compile fileTree(dir: "libs", include: ["*.jar"])
    compile "com.android.support:appcompat-v7:23.0.1"
    compile "com.facebook.react:react-native:+"  // From node_modules
    compile project(':react-native-navigation')
}
```

- 第四步：配置 `android/app/src/main/java/com/xxxx/MainApplication.java`

```js
import com.oblador.vectoricons.VectorIconsPackage;
@Override
  protected List<ReactPackage> getPackages() {
    return Arrays.<ReactPackage>asList(
      new MainReactPackage()
+   , new VectorIconsPackage()
    );
  }
```

> 到这里配置就全部完成，接下来就可以在`rn`项目中使用`iconfont`


![](https://upload-images.jianshu.io/upload_images/1967155-e7dda5a9ec5c3ad2.png)



### 2.2 IOS平台

> 打开你的`Xcode`项目工程，右键工程文件，选择`react`项目下的`node_modules/react-native-vector-icons/Fonts`文件

![image.png](https://upload-images.jianshu.io/upload_images/1480597-7a01826c75e89111.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**在xcode的Info.plist文件中,加入: Fonts provided by application数组**

![image.png](https://upload-images.jianshu.io/upload_images/1480597-ba6ef6e04410250f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 打开终端，输入：`rnpm link`，回车后会看到`Fonts provided by application`下加入如下字体

![image.png](https://upload-images.jianshu.io/upload_images/1480597-7b5d151f1cfec6ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 重新运行`react`项目，终端输入：`react-native run-ios`，可以看到效果了


## 三、react-native-router-flux的使用

> https://github.com/aksonov/react-native-router-flux


## 四、实战开发GitHub客户端

> 进行中，待总结

## 参考

- [react-native-elements组件库](https://react-native-training.github.io/react-native-elements/docs/0.19.1/icon.html)
- [redux在react-native上使用(一)--加入redux](https://segmentfault.com/a/1190000008741380)
- [在react-native中使用redux](https://www.jianshu.com/p/2c43860b0532)
