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

> `react-native-vector-icons` 是可以直接使用图片名就能加载图片的第三方,类似于`web的 iconfont`矢量图，使用很方便, 你不需要在工程文件夹里塞各种图片, 节省很多空间,下面就来看看怎么使用吧


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

### 3.1 简介

**特性**


> `react-native-router-flux` 是一个路由包.在一个中心区域定义可切换`scene`模块。在使用过程中，跟`react-native`提供的`navigator`的区别是你不需要有`navigator`对象。你可以在任意地方使用简单的语法去控制`scene`的切换，如：` Actions.login({username, password})`  or `Actions.profile({profile})` or 甚至`Actions.profile(123)` ,其中`login` `profile`等是路由的`key`，通过调用`key`来切换路由

- 所有的参数将被注入到`this.props`中给`Sene`组件使用

**功能和亮点**

- 可定制的导航条：由`Scene`或者`Scene`的`state`去控制导航条的`show`／`hide`
- 嵌套导航：每一个`tab`都可以有自己的导航，该导航被嵌套在`root`导航中
- 使用`Action sheet` 来自定义场景渲染器
- 动态路由：动态路由将允许你通过应用的`state`去选着哪个`scene`将被渲染
- `Reset History stack`重置历史栈：新的`reset` 类型将提供清除历史栈河消除导航的返回按钮的功能
- 更加强大的状态控制：在多个`scene`中可以有不同的`state`

```
npm i react-native-router-flux --save
```

**使用方式一**

> 在你的`src/index.js`级别的文件中使用`Scene`组件定义你的`scenes`，并且`Scene`组件作为`Router`的子节点。定义好的`Scene`将由`Router`来控制其行为

```js
import {Scene, Router, Actions} from 'react-native-router-flux';

import {Router, Scene} from "react-native-router-flux";
import PageOne from "./Component/PageOne"; 
import PageTwo from "./Component/PageTwo";

const Root = () => {
  return (
    <Router>
      {/* 这种写法是将全部的跳转页面都放在Root下面 */}
      <Scene key="root">
        {/* key 就是给页面的标签,供Actions使用 */}
        {/* component 设置关联的页面 */}
        {/* title 就是给页面标题 */}
        {/* initial 就是设置默认页面*/}
        <Scene
          key="one"
          component={PageOne}
          title="PageOne"
          initial={true}
        />
        <Scene key="two" component={PageTwo} title="PageTwo" />

      </Scene>
    </Router>
  );
};
```


**第二种使用方式**

> 你可以在编译期定义你所有的`scenes`，并在后面的`Router`里面使用

```jsx
import {Actions, Scene, Router} from 'react-native-router-flux';

const scenes = Actions.create(
  <Scene key="root">
    <Scene key="login" component={Login} title="Login"/>
    <Scene key="register" component={Register} title="Register"/>
    <Scene key="home" component={Home}/>
  </Scene>
);

/* ... */

class App extends React.Component {
  render() {
    return <Router scenes={scenes}/>
  }
}
 {Actions, Scene, Router} from 'react-native-router-flux';

const scenes = Actions.create(
  <Scene key="root">
    <Scene key="login" component={Login} title="Login"/>
    <Scene key="register" component={Register} title="Register"/>
    <Scene key="home" component={Home}/>
  </Scene>
);

/* ... */

class App extends React.Component {
  render() {
    return <Router scenes={scenes}/>
  }
}
```

在任意地方通过导入

```js
import {Actions} from 'react-native-router-flux'
```

> 获得`Actions` 对象，`Actions`对象将是我们操作`Scenes`的遥控器。通过`Actions`我们可以向`Router`发出动作让`Router`控制`Scene`变化。

- 调用`Actions.ACTION_NAME(PARAMS)`可以展示一个`scene`，参数将被注入`scene`中
 (如`Actions.login()`切换到登录页面)
- `Actions.pop()`方法将会弹出当前的`scene`，他接受如下可选参数 
  - `{popNum:[number]}`允许你去一次弹出多个`scene`
  - `{refresh:{...propsToSetOnPreviousScene}}`允许你去刷新`pop`后的`scene`
- `Actions.refresh(PARAMS)`会更新当前`scene`的属性

### 3.2 简单例子

```js
import {Router, Scene} from "react-native-router-flux";
import PageOne from "./Component/PageOne"; 
import PageTwo from "./Component/PageTwo";

const Root = () => {
  return (
    <Router>
      {/* 这种写法是将全部的跳转页面都放在Root下面 */}
      <Scene key="root">
        {/* key 就是给页面的标签,供Actions使用 */}
        {/* component 设置关联的页面 */}
        {/* title 就是给页面标题 */}
        {/* initial 就是设置默认页面*/}
        <Scene
          key="one"
          component={PageOne}
          title="PageOne"
          initial={true}
        />
        <Scene key="two" component={PageTwo} title="PageTwo" />

      </Scene>
    </Router>
  );
};
```

```js
// PageOne 的核心代码，点击 Text 跳转到下一个页面

//导入Action的包,处理页面跳转
import { Actions } from 'react-native-router-flux'; 

const PageOne = () => {
  return (
    <View style={styles.container}>
      <Text style={styles.welcome}
        onPress={()=>Actions.two()} >
        我是Page One
      </Text>
    </View>
  );
};
```

```js
// PageTwo 的核心代码

export default class PageTwo extends Component {
    render() {
        return (
            <View style={styles.container}>
                <Text style={styles.welcome}>我是Page Two </Text>
            </View>)
    }
}
```

运行就可以看到下面的效果：

![image.png](https://upload-images.jianshu.io/upload_images/1480597-321388619705965b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

简单就完成了两个页面之间的切换


**每一个`Scene component` 有如下属性**

- `key`：一个唯一的字符串，用来标识一个`Scene`，可以理解为`scene`的一个身份牌号码
- `component`：当切换到该`scene`时，`component`属性引用的组件将被渲染出来
- `title`：当切换到对应的`scene`时，屏幕顶部的导航条中间将显示该`title`
- `initial={true}` 表示默认为初始化`scene`

> 在`pageOne`中有一个`Text`组件，当点击`onPress`方法，该方法将调用`Actions.pageTwo`

- 会调用`Actions.SCENE_KEY(PARAMS) `,`SCENE_KEY`即为之前定义的`key`值，参数为可选的
- 我们的`Actions`就会通知`Router`，把`key=pageTwo`的`Scene`显示出来，如果传有参数的话，参数也会传入`Scene`组件中

```js
render() {
  const goToPageTwo = () => Actions.pageTwo({text: 'Hello World!'}); 
  return (
    
      This is PageOne!
    
  )
}
```

> 我们传递一个参数名为`text` 。值为`Hello World`！如下所示，我们就可以在`key=pageTwo`的`scene`的`component`属性置顶的组件中通过`props`获取该参数值

```js
render() {
  return (
    
      This is PageTwo!
      {this.props.text}
    
  )
}
```

> 我们从`pageOne`跳转到了`pageTwo`，如果我们想跳回`pageOne`怎么办呢

- 官方提供的导航栏早已提供了一个`back icon`，我们也可以通过调用`Actions.pop()`方法将当前`scene`弹出栈，我们的`pageOne`就在栈顶了，此时显示的就是`pageOne`了，如果跳回来后我们需要刷新当前`scene`，我们可以调用`Actions.refresh(PARAMS)`

**数据传递与刷新**

> 页面之间的切换自然不会缺少数据的传递，而且这个路由框架可以实时 `refresh` 当前页面

- 先看页面之间传递数据吧，这里添加一个 `PageThree `

```js
import {Actions} from "react-native-router-flux"

const PageThree = () => {
    return (
        <View style={styles.container}>
            <Text style={styles.welcome}
                //Actions.pop是退回到上一层
                  onPress={() => Actions.pop({
                      //refresh用于刷新数据
                      refresh: {
                          data: '从 three 回到 two'
                      }
                  })}>我是Page Three </Text>
        </View>
    );
};
```

`PageTwo` 也要修改一下代码

```js
import {Actions} from 'react-native-router-flux'; // New code

export default class PageTwo extends Component {

    render() {
        const data = this.props.data || "null";
        return (
            <View style={styles.container}>
                <Text style={styles.welcome}
                     //添加点击事件并传递数据到PageThree
                      onPress={() => Actions.three({data: "从 two 传递到 three"})}
                >我是Page Two </Text>
               <Text style={styles.refresh}
                //展示从PageThree传回来的数据
                > refresh:{data}</Text>
            </View>)
    }
}
```

> 最后到 `Root.js` 添加新的 `Scence`

```js
const Root = () => {
    return (
        <Router>
               //...........
                <Scene key="three"
                       component={PageThree}
                       title="PageThree"/>
            </Scene>
        </Router>
    );
};
```

此时运行就可以看到页面数据传递的效果了


![image.png](https://upload-images.jianshu.io/upload_images/1480597-2fc83fc7ce2959b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 可以看到从 `PageThree` 回到 `PageTwo` 数据传递并刷新页面的效果，不过如果需要实时刷新当前页面呢？这时就需要使用 `Actions.refresh` 方法了

```js
export default class PageTwo extends Component {

    render() {
        const data = this.props.data || "null";
        return (
            <View style={styles.container}>
                <Text style={styles.welcome}
                      onPress={() => Actions.three({data: "从 two 传递到 three"})}
                >我是Page Two </Text>
                <Text style={styles.refresh}
                      onPress={() => Actions.refresh({
                          data: 'Changed data',
                      })}
                > refresh:{data}</Text>
            </View>)
    }
}
```

**Tab Scene**

> 通过设置 `Scene` 属性的 `Tabs` 可以设置 `Tabs` 。这个也开发中经常用到的页面效果

```js
//设置tab选中时的字体颜色和标题
const TabIcon = ({focused , title}) => {
    return (
        <Text style={{color: focused  ? 'blue' : 'black'}}>{title}</Text>
    );
};

const Root = () => {
    return (<Router>
        {/*tabBarPosition设置tab是在top还是bottom */}
        <Scene hideNavBar tabBarPosition="bottom">
            <Tabs
                key="tabbar"
                swipeEnabled
                wrap={false}
                // 是否显示标签栏文字
                showLabel={false}
                tabBarStyle={{backgroundColor: "#eee"}}
                //tab选中的颜色
                activeBackgroundColor="white"
                //tab没选中的颜色
                inactiveBackgroundColor="red"
            >
                <Scene
                    key="one"
                    icon={TabIcon}
                    component={PageOne}
                    title="PageOne"
                />

                <Scene
                    key="two"
                    component={PageTwo}
                    title="PageTwo"
                    icon={TabIcon}
                />

                <Scene
                    key="three"
                    component={PageThree}
                    title="PageThree"
                    icon={TabIcon}
                />
            </Tabs>
        </Scene>
    </Router>)
};
```

![image.png](https://upload-images.jianshu.io/upload_images/1480597-d2ba79cd08c0e26d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3.3 react-native-router-flux之API

> 英文版：https://github.com/aksonov/react-native-router-flux/blob/master/docs/API.md

#### 3.3.1 Router

|Property| Type|	Default|	Description|
|---|---|---|---|
|`children`||`required`|页面根组件|
|`wrapBy`	|`Function`||允许集成诸如`Redux`（`connect`）和`Mobx`（`observer`）之类的状态管理方案|
|`sceneStyle`|	`Style`	||	适用于所有场景的`Style`（可选）|
|`backAndroidHandler`|	`Function`||	允许在`Android`中自定义控制返回按钮（可选）|

**backAndroidHandler用法**

```js
const onBackPress = () => {
    if (Actions.state.index !== 0) {
      return false
    }
    Actions.pop()
    return true
}

backAndroidHandler={onBackPress}
```

#### 3.3.2 Scene

> 此路由器的最重要的组件， 所有 `<Scene>` 组件必须要有一个唯一的 `key`。父节点`<Scene>`不能将`component`作为`prop`，因为它将作为其子节点的组件

|Property| Type|	Default|	Description|
|---|---|---|---|
|`key`|`string`|`required`|将用于标识页面，例如`Actions.name(params)`。必须是独一无二的|
|`path`|	`string`|	|将被用来匹配传入的深层链接和传递参数，例如：`/user/:id/`将从`/user/1234/`用`params {id：1234}`调用场景的操作。接受`uri`的模板标准|
|`component`|`React.Component`|`semi-required`|要显示的组件，定义嵌套时不需要`Scene`。|
|`back`|`boolean`|`false`|如果是`true`，则显示后退按钮，而不是由上层容器定义的左侧`/drawer`按钮|
|`backButtonImage`|	`string`||设置返回按钮的图片|
|`backButtonTintColor`|	`string`||自定义后退按钮色调|
|`init`|`boolean`|`false`|如果是`true`后退按钮不会显示|
|`clone`|	`boolean`|	`false`|标有`clone`的场景将被视为模板，并在被推送时克隆到当前场景的父节点中|
|`contentComponent`|`React.Component`|	|用于呈现抽屉内容的组件（例如导航）|
|`drawer`|	`boolean`|	`false`|	载入`DrawerNavigator`内的子页面|
|`failure`|	`Function` || 如果`on`返回一个`“falsey”`值，那么`failure`将被调用|
|`backTitle`	|`string`	||	指定场景的后退按钮标题|
|`backButtonTextStyle`|	`Style`||		用于返回按钮文本的样式|
|`rightTitle`|	`string`||		为场景指定右侧的按钮标题|
|`headerMode` |`string`| `float`|指定标题应该如何呈现：（`float`渲染单个标题，保持在顶部，动画随着屏幕的变化，这是`iOS`上的常见样式。）`screen`（每个屏幕都有一个标题，并且标题淡入，与屏幕一起出现，这是`Android`上的常见模式）如果为`none`（不会显示标题）|
|`hideNavBar`|	`boolean`	|`false`|	隐藏导航栏|
|`hideTabBar`|	`boolean`|	`false`|隐藏标签栏（仅适用于拥有`tabs`指定的场景）|
|`hideBackImage`| `boolean`	| `false` |隐藏返回图片|
|`initial`|`boolean`|	`false`	|设置为`true`后，会默认显示该页面|
|`leftButtonImage`|	`Image`	||	替换左侧按钮图片|
|`leftButtonTextStyle`|	`Style`||		左侧按钮的文字样式|
|`leftButtonStyle`	|`Style`		||左侧按钮的样式|
|`leftButtonIconStyle`|	`Style`		||左侧按钮的图标样式|
|`modal`|	`boolean`|	`false`|将场景容器定义为`modal`，即所有子场景都将从底部弹起到顶部。它仅适用于`containers`（与`v3`版本的语法不同）|
|`navBar`|	`React.Component`||		可以使用自定义的`React`组件来定义导航栏|
|`navBarButtonColor`|	`string`	|	|设置导航栏返回按钮的颜色|
|`navigationBarStyle`|	`Style`	||	导航栏的样式|
|`navigationBarTitleImage`	|`Object`||导航栏中的图像中覆盖`title`的`Image`|
|`navigationBarTitleImageStyle`|	`object`	||	`navigationBarTitleImage`的样式|
|`navTransparent`|	`boolean`|	`false`|	导航栏是否透明|
|`on`	|`Function`	||	又名 `onEnter`|
|`onEnter`|	`Function`	||	当Scene要被跳转时调用。props将被作为参数提供。只支持定义了`component`的场景。|
|`onExit`	|`Function`||当`Scene`要跳转离开时调用。只支持定义了`component`的场景|
|`onLeft`|	`Function`	|	|当导航栏左侧按钮被点击时调用|
|`onRight`|	`Function`	||	当导航栏右侧按钮被点击时调用|
|`renderTitle`	|`React.Component`||		使用`React`组件显示导航栏的`title`|
|`renderLeftButton`	|`React.Component	`	||使用`React`组件显示导航栏的左侧按钮|
|`renderRightButton`|	`React.Component`||使用`React`组件显示导航栏的右侧按钮|
|`renderBackButton`	|`React.Component`		||使用`React`组件显示导航栏的返回按钮|
|`rightButtonTextStyle`	|`Style`|	|	右侧按钮文字的样式|
|`success`|	`Function`	||	如`on`返回一个"真实"的值，那么`success`将被调用|
|`tabs`	|`boolean`|	`false`|将子场景加载为`TabNavigator`。其他标签导航器属性也是适用|
|`title`	|`string`	||	要显示在导航栏中心的文本|
|`titleStyle`|	`Style`||		`title`的样式|
|`type`|	`string`|	`push`|可选的导航操作。你可以使用`replace`来替换此场景中的当前场景|

#### 3.3.3 Tabs (<Tabs> or <Scene tabs>)

标签栏组件

> 你可以使用`<Scene>`中的所有`props`来作为`<Tabs>`的属性。 如果要使用该组件需要设置 ` <Scene tabs={true}>`

|Property| Type|	Default|	Description|
|---|---|---|---|
|`wrap`|	`boolean`	|`true`|	自动使用自己的导航栏包装每个场景（如果不是另一个容器）。|
|`activeBackgroundColor`	|`string`	||	指定焦点的选项卡的选中背景颜色|
|`activeTintColor`	|`string`		||指定标签栏图标的选中色调颜色|
|`inactiveBackgroundColor`|	`string`	||	指定非焦点的选项卡的未选中背景颜色|
|`inactiveTintColor`|	`string`	||	指定标签栏图标的未选中色调颜色|
|`labelStyle`|`object`|		|设置`tabbar`上文字的样式|
|`lazy`|	`boolean`|	`false`|在选项卡处于活动状态之前，不会渲染选项卡场景(推荐设置成`true`)|
|`tabBarComponent`|	`React.Component`	||	使用`React`组件以自定义标签栏|
|`tabBarPosition`|	`string`||		指定标签栏位置。`iOS`上默认为`bottom`，安卓上是`top`|
|`tabBarStyle`|	`object`	||	标签栏样式|
|`tabStyle`	|`object`		||单个选项卡的样式|
|`showLabel`|	`boolean`|	`true`	|是否显示标签栏文字|
|`swipeEnabled`|	`boolean`|	true`	|是否可以滑动选项卡|
|`animationEnabled`|	`boolean`|	`true`|	切换动画|
|`tabBarOnPress`|	`function`	||	自定义`tabbar`点击事件|
|`backToInitial`|	`boolean`|	`false`	|如果选项卡图标被点击，返回到默认选项卡|

#### 3.3.4 Stack (<Stack>)

> 将场景组合在一起的组件，用于自己的基于堆栈实现的导航。使用它将为此堆栈创建一个单独的`navigator`，因此，除非您添加`hideNavBar`，否则将会出现两个导航条

#### 3.3.5 Tab Scene (child <Scene> within Tabs)

> 用于实现`Tabs`的效果展示，可以自定义`icon`和`label`

|Property| Type|	Default|	Description|
|---|---|---|---|
|`icon`|	`component`|	`undefined`|	作为选项卡图标放置的`RN`组件|
|`tabBarLabel`|	`string`||		`tabbar`上的文字|

#### 3.3.6 Drawer (<Drawer> or <Scene drawer>)

> 用于实现抽屉的效果，如果要使用该组件需要设置 `<drawer tabs={true}>`。

|Property| Type|	Default|	Description|
|---|---|---|---|
|`drawerImage`|	`Image`		||替换抽屉'hamburger'图标，你必须把它与`drawer`一起设置|
|`drawerIcon`|	`React.Component`|	|用于抽屉`hamburger`图标的任意组件，您必须将其与`drawer`道具一起设置|
|`hideDrawerButton`|`boolean`|	`false`	|是否显示`drawerImage`或者`drawerIcon`|
|`drawerPosition`|	`string`	||	抽屉是在右边还是左边。可选属性r`ight`或`left`|
|`drawerWidth`	|`number`		||抽屉的宽度（以像素为单位）（可选）|


#### 3.3.7 Modals (<Modal> or <Scene modal>)

> 想要实现模态，您必须将其`<Modal>`作为您`Router`的根场景。在`Modal`将正常呈现第一个场景（应该是你真正的根场景），它将渲染第一个元素作为正常场景，其他所有元素作为弹出窗口（当它们 被`push`）

示例：在下面的示例中，`root`场景嵌套在`<Modal>`中，因为它是第一个嵌套`Scene`，所以它将正常呈现。如果要`push`到`statusModal`，`errorModal`或者`loginModal`，他们将呈现为`Modal`，默认情况下会从屏幕底部向上弹出。重要的是要注意，目前`Modal`不允许透明的背景。

```jsx
//... import components
<Router>
  <Modal>
    <Scene key="root">
      <Scene key="screen1" initial={true} component={Screen1} />
      <Scene key="screen2" component={Screen2} />
    </Scene>
    <Scene key="statusModal" component={StatusModal} />
    <Scene key="errorModal" component={ErrorModal} />
    <Scene key="loginModal" component={LoginModal} />
  </Modal>
</Router>
```

#### 3.3.8 Lightbox (<Lightbox>)

> `Lightbox`是用于将组件渲染在当前组件上`Scene`的组件 。与`Modal`不同，它将允许调整大小和背景的透明度

在下面的示例中，`root`场景嵌套在中`<Lightbox>`，因为它是第一个嵌套`Scene`，所以它将正常呈现。如果要`push`到`loginLightbox`，他们将呈现为`Lightbox`，默认情况下将放置在当前场景的顶部，允许透明的背景

```jsx
//... import components
<Router>
  <Lightbox>
    <Scene key="root">
      <Scene key="screen1" initial={true} component={Screen1} />
      <Scene key="screen2" component={Screen2} />
    </Scene>

    {/* Lightbox components will lay over the screen, allowing transparency*/}
    <Scene key="loginLightbox" component={loginLightbox} />
  </Lightbox>
</Router>
```

#### 3.3.9 Actions

- 该对象的主要工具是为您的应用程序提供导航功能。 假设您的`Router`和`Scenes`配置正确，请使用下列属性在场景之间导航。 有些提供添加的功能，将`React`道具传递到导航场景
- 这些可以直接使用，例如，`Actions.pop()`将在源代码中实现的操作，或者，您可以在场景类型中设置这些常量，当您执行`Actions.main()`时，它将根据您的场景类型或默认值来执行动作

|Property| Type|	Default|	Description|
|---|---|---|---|
|`[key]`|	`Function`	|`Object`|	`Actions`将'自动'使用路由器中的场景`key`进行导航。如果需要跳转页面，可以直接使用`Actions.key()`或`Actions[key].call()`|
|`currentScene`|	`String`	||	返回当前活动的场景|
|`jump`|	`Function`	|`(sceneKey: String, props: Object)`	|用于切换到新选项卡. For Tabs only.|
|`popTo`|	`Function`|	`(sceneKey: String, props: Object)`|	返回到指定的页面|
|`push`|	`Function`|	`(sceneKey: String, props: Object)`	|跳转到新页面|
|`refresh`|	`Function`|	`(props: Object)`|重新加载当前页面|
|`replace`|	`Function`|	`(sceneKey: String, props: Object)`|	从堆栈中弹出当前场景，并将新场景推送到导航堆栈。没有过度动画|
|`reset`|	`Function`|	`(sceneKey: String, props: Object)`|	清除路由堆栈并将场景推入第一个索引. 没有过渡动画|
|`drawerOpen`	| `Function` | |如果可用，打开`Drawer` |
|`drawerClose`|`Function`||如果可用，关闭`Drawer`|

#### 3.3.10 ActionConst

> 键入常量以确定`Scene`转换，这些是优先于手动键入其值，因为项目更新时可能会发生更改

|Property| Type|	Default|	Description|
|---|---|---|---|
|`ActionConst.JUMP`	|`string`|	`'REACT_NATIVE_ROUTER_FLUX_JUMP'`|	`jump`|
|`ActionConst.PUSH`|	`string`|	`'REACT_NATIVE_ROUTER_FLUX_PUSH'`|	`push|
|`ActionConst.PUSH_OR_POP`|	`string`|	`'REACT_NATIVE_ROUTER_FLUX_PUSH_OR_POP'`|	`push`|
|`ActionConst.REPLACE`|	`string`|	`'REACT_NATIVE_ROUTER_FLUX_REPLACE'`|	`replace`|
|`ActionConst.BACK`	|`string`|	`'REACT_NATIVE_ROUTER_FLUX_BACK'`|	`pop`|
|`ActionConst.BACK_ACTION`|	`string`|	`'REACT_NATIVE_ROUTER_FLUX_BACK_ACTION'`|	`pop`|
|`ActionConst.POP_TO`|	`string`|	`'REACT_NATIVE_ROUTER_FLUX_POP_TO'`|	`popTo`|
|`ActionConst.REFRESH`|	`string`|	`'REACT_NATIVE_ROUTER_FLUX_REFRESH'`|	`refresh`|
|`ActionConst.RESET`|	`string`|	`'REACT_NATIVE_ROUTER_FLUX_RESET'`|	`reset`|
|`ActionConst.FOCUS`	|`string`|	`'REACT_NATIVE_ROUTER_FLUX_FOCUS'`|	`N/A`|
|`ActionConst.BLUR`	|`string`|	`'REACT_NATIVE_ROUTER_FLUX_BLUR'`|	`N/A`|
|`ActionConst.ANDROID_BACK`|	`string`	|`'REACT_NATIVE_ROUTER_FLUX_ANDROID_BACK'`	|`N/A`|

#### 3.3.11 Universal and Deep Linking

- 考虑这样一个`web`应用程序和`app`配对,这可能有一个`urlhttps://thesocialnetwork.com/profile/1234/`
- 如果我们同时构建一个`web`应用程序和一个移动应用程序，我们希望能够通过`path /profile/:id/`
- 在web上，我们可能想要用一个路由器来打开我们的`<Profile />`和参数`{ id: 1234 }`
- 在移动设备上，如果我们正确地设置了`Android / iOS`环境来启动我们的应用程序并打开`RNRF<Router />`,，那么我们还需要导航到我们的移动`<Profile />`场景和参数`{ id: 1234 }`

```html
<Router uriPrefix={'thesocialnetwork.com'}>
  <Scene key="root">
     <Scene key={'home'} component={Home} />
     <Scene key={'profile'} path={"/profile/:id/"} component={Profile} />
     <Scene key={'profileForm'} path={"/edit/profile/:id/"} component={ProfileForm} />
  </Scene>
</Router>
```

> 如果用户点击`http://thesocialnetwork.com/profile/1234/`在他们的设备,他们会打开`<Router/ >`,然后调用操作`Actions.profile({ id:1234 })`




## 四、实战开发GitHub客户端

> 进行中，待总结

## 五、参考

- [react-native-elements组件库](https://react-native-training.github.io/react-native-elements/docs/0.19.1/icon.html)
- [redux在react-native上使用(一)--加入redux](https://segmentfault.com/a/1190000008741380)
- [在react-native中使用redux](https://www.jianshu.com/p/2c43860b0532)

