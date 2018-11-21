---
title: webpack回顾篇
date: 2018-11-21 17:40:08
tags: 
   - webpack
   - 优化
categories: Front-End
---


> 基于`webpack3`

## 一、webpack简介

### 1.1 版本更迭

![image.png](https://upload-images.jianshu.io/upload_images/1480597-323d70783413ae1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**大版本变化**

![image.png](https://upload-images.jianshu.io/upload_images/1480597-4fe90a7f3cb6712e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 1.2 功能进化

**Webpack V1**

- 编译、打包
- `HMR` (模块热更新)
- 代码分割
- 文件处理

**Webpack V2**

- `Tree Shaking`
- `ES module`
- 动态 `Import`
- 新的文档

**Webpack V3**

- `Scope Hoisting` (作用域提升)
- `Magic Comments` （配合动态`import`使用）

> 版本迁移

**V1 -> V2**

迁移指南 https://doc.webpack-china.org/guides/migrating/

**V2 -> V3**

更新升级即可

## 二、webpack核心概念

### 2.1 Entry

- 代码的入口
- 打包的入口
- 单个或多个

> 写法建议使用键值对写法

```js
module.exports = {
  entry: 'index.js'
}
```

```js
module.exports = {
  entry: '[index.js','vendor.js']
}
```

```js
module.exports = {
  entry: {
      index:'index.js'
  }
}
```


```js
module.exports = {
  entry: {
      index:['index.js','app.js'],
      vendor: 'vendor.js'
  }
}
```


### 2.2 Output

- 打包成的文件(`bundle`)
- 一个或多个
- 自定义规则
- 配合`CDN`

```js
module.exports = {
  entry: 'index.js',
  output: {
      filename: 'index.min.js'
  }
}
```

```js
module.exports = {
  entry: {
    index: 'index.js',
    vendor: 'vendor.js'
  },
  output: {
      filename: '[name].min[hash:5].js'
  }
}
```


### 2.3 Loaders

- 处理文件
- 转化为模块

```js
module.exports = {
  module: {
      rules: [
        {
            test: /\.css$/,
            use: 'css-loader'
        }
      ]
  }
}
```

#### 2.3.1 常用Loader

**编译相关**

- `babel-loader`
- `ts-loader`

**样式相关**

- `style-loader`
- `css-loader`
- `less-loader`
- `postcss-loader`

**文件相关**

- `file-loader`
- `url-loader`


### 2.4 Plugins

- 参与打包整个过程
- 打包优化和压缩
- 配置编译时的变量
- 极其灵活

```js
module.exports = {
 plugins: [
    new webpack.optimize.UglifyJsPlugin()
 ]
}
```

#### 2.4.1 常用plugins

**优化相关**

- `CommonsChunkPlugin`
- `UglifyJsWebpackPlugin`

**功能相关**

- `ExtractTextWebpackPlugin` 提取css
- `HtmlWebpackPlugin` 生成HTML模板
- `HotModuleReplacementPlugin` 热模块替换
- `CopyWebpackPlugin` 拷贝文件

### 2.5 名词

- `Chunk` 打包过程分割的代码块
- `Bundle` 打包后的文件
- `Module` 

## 三、初探 webpack 

### 3.1 使用babel打包es6

#### 3.1.1 编译 ES 6/7

**Babel**

```bash
npm install babel-loader@8.0.0-beta.0 @babel/core
npm install –save-dev babel-loader babel-core 
```

**Babel Presets**

> 主要有几种类型选择

- `es2015`
- `es2016`
- `es2017`
- `env`
- `babel-preset-react`
- `babel-preset-stage 0 - 3`


```bash
npm install @babel/preset-env –save-dev
npm install babel-preset-env –save-dev
```

**Babel Polyfill**


> 针对一些不能处理的函数方法(`Generator`、`Set`、`Map`、`Array.from...`)需要用到`babel-Polyfill`处理

- 全局垫片
- 为应用准备

```bash
npm install babel-polyfill –save
```

```
import “babel-polyfill”
```

**Babel Runtime Transform**

- 局部垫片
- 为开发框架准备

```bash
npm install babel-plugin-transform-runtime –save-dev

npm install babel-runtime –save
```


例子

```js
module.exports = {
    entry: {
       app: 'app.js'
    },
    output: {
        filename: '[name].[hash:8].js'
    },
    module: {
        rules:[
            test: /\.js$/,
            use: {
                loader: 'babel-loader',
                options: {
                    presets: [
                        '@babel/preset-env',{
                            //指定target为根据哪些语法编译
                            targets: {
                                browsers: ['> 1%','last 2 versions']
                            }
                        }
                    ]
                }
            },
            exclude: '/node_modules'
        ]
    }
}
```

> 对于`webpack`中`babel`的配置可以单独提取处理`.babelrc`统一管理

```json
{
    "presets": [
        ["@babel/preset-env",{
            targets: {
                browsers: ['> 1%','last 2 versions']
            }
        }]
    ],
    "plugins": [
      "transform-runtime"
    ]
]
```

### 3.2 打包 Typescript


```bash
npm i typescipt ts-loader  --save-dev
npm i typescipt awesome-typescript-loader  --save-dev
```

配置

- `tsconfig.json`
- `webpack.config.js`

**tsconfig**

- 配置选项:官网`/docs/handbook/compiler-options.html`
- 常用选项 `compilerOptions` `include` `exclude`

**声明文件**

> 用于编译时检查错误

以`loadsh`为例，需要安装`@types/lodash`带有声明文件的，而不是安装`lodash`

```bash
npm install @types/lodash
npm install @types/vue
```

**Typings**

> 也可以这样安装带有`type`的包

```bash
npm install typings
typings install lodash
```

例子

```js
module.exports = {
    entry: {
        'app': 'app.js'
    },
    output: {
        filename: '[name].bundle.js'
    },
    module: {
        rules: [
            test: /\.tsx?$/,
            use: {
                loader: 'ts-loader'
            }
        ]
    }
}
```

> 在项目根目录创建`tsconfig.json`

```js
 {
    "compilerOptions": {
         "module": "comonjs",
         "target": "es5", //编译后的文件在哪个环境运行
         "allowJs": true,//允许js语法
    },
    "include": [
        //编译路径
        "./src/*"
    ],
    "exclude": [
        //排除编译文件
        "./node_modules"
    ]
 }
```


### 3.3 提取 js 的公用代码

- 减少代码冗余
- 提高加载速度

> 主要使用内置插件实现`webpack.optimize.CommonsChunkPlugin`

```js
{
	plugins: [
	     new webpack.optimize.CommonsChunkPlugin(option)
	]
}
```

例子

```js
module.exports = {
    entry: {
        "pageA": "./src/pageA",
        "pageB": "./src/pageB",
        "vendor": ['loash']//业务代码和第三方代码区分开,给loash单独打一个包
    },
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: '[name].bundle.js',
        chunkFilename: '[name].chunk.js'
    },
    plugins: [
    // 提取common
    new webpack.optimize.CommonsChunkPlugin({
         name: 'common',
         minChunks:2,//出现两次就打包成common代码
         chunks: ['pageA','pageB']//指定范围提取公共代码
     })
        
    // 提取vendor、取业务代码manifest
     new webpack.optimize.CommonsChunkPlugin({
         //把entry的vendor代码和这里的common（webpack打包的）代码合并
         names: ['vendor','manifest']
         minChunks: Infinity
     })
     // ==== 提取业务代码manifest== 合并到上面names中
     // 如果不想把webpack打包的代码和vendor代码合并 需要提取到manifest
     //new webpack.optimize.CommonsChunkPlugin({
         // webpack代码和vendordiam区分开
       //  name: 'manifest' //manifest即生成
       //  minChunks: Infinity
    // })
     
    ]
}
```

![image.png](https://upload-images.jianshu.io/upload_images/1480597-1c2f22380466b81f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 3.4 代码分割和懒加载

**第一种方式：通过wepack内置方法**

> `require.ensure`动态加载一个模块，接收四个参数

- `[]:dependencies` 初次并不会执行
- `callback`的时候才会执行
- `errorCallback` 可省略
- `chunkName`

**第二种方式：通过ES2015 Loader Spec**

> `System.import()`后面演变为`import()`来动态加载模块

`import()`方式返回一个`promise`在`import`中传入需要依赖的明，动态加载模块，就可以像使用`Promise`一样使用`import().then()`


**代码分割场景**

- 分离业务代码 和 第三方依赖
- 分离业务代码 和 业务公共代码 和 第三方依赖
- 分离首次加载 和 访问后加载的代码

例子

```js
module.exports = {
    entry: {
        "pageA": "./src/pageA"
    },
    output: {
        path: path.resolve(__dirname, './dist'),
        publicPath: './dist/',//动态加载路径
        filename: '[name].bundle.js',
        chunkFilename: '[name].chunk.js'
    }
}
```

> 目标是提取`pageA`、`pageB`中公共的模块`moduleA`

```js
//src/pageA.js

import './subPageA'
import './subPageB'

//ensure的时候代码不会执行 需要在下面加载一次
require.ensure(['lodash'],function(){
    var _ = require('lodash')
    _.join([1,2,3])
},'vendor')// 指定chunk名称


export default 'pageA'
```

运行打包这时`loadash`提取到`vendor`中

![image.png](https://upload-images.jianshu.io/upload_images/1480597-c2189ccbd6ffb75d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时候还不是我们想要的

```js
//src/pageA.js

require.include('./moduleA')

if(page === 'subPageA'){
  require.ensure(['./subPageA'],function(){
    var subPageA = require('./subPageA')
    },'subPageA')// 指定chunk名称
}else{
  require.ensure(['./subPageB'],function(){
    var subPageB = require('./subPageB')
    },'subPageB')// 指定chunk名称
}


//ensure的时候代码不会执行 需要在下面加载一次
require.ensure(['lodash'],function(){
    var _ = require('lodash')
    _.join([1,2,3])
},'vendor')// 指定chunk名称


export default 'pageA'
```
![image.png](https://upload-images.jianshu.io/upload_images/1480597-e12c1ea015bd7570.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 这时候这有`pageA`中才有`moduleA`

![image.png](https://upload-images.jianshu.io/upload_images/1480597-30e7721eab315d18.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 新建一个html验证是否动态加载

```html
<html>
    <body>
        <script src="./dist/pageA.bundle.js"></script>
    </body>
</html>
```

![image.png](https://upload-images.jianshu.io/upload_images/1480597-f6685bdfe1289e17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 


**import()动态加载的写法**

```js
//src/pageA.js

require.include('./moduleA')

var page = 'subPageA'

if(page === 'subPageA'){
 // 指定chunkName /** webpackChunkName: 'subPageA' **/ 
  import(/** webpackChunkName: 'subPageA' **/,'./subPageA').then(function(subPageA){
      console.log(subPageA)
  })
}else{
  import(/** webpackChunkName: 'subPageB' **/'./subPageB').then(function(subPageB){
      console.log(subPageB)
  })
}

// 异步加载lodash
//ensure的时候代码不会执行 需要在下面加载一次
require.ensure(['lodash'],function(){
    var _ = require('lodash')
    _.join([1,2,3])
},'vendor')// 指定chunk名称


export default 'pageA'
```

> 如果`/** webpackChunkName: 'subPageA' **/`相同，则会合并处理

合并了`subPageA`和`subPageB`

![image.png](https://upload-images.jianshu.io/upload_images/1480597-015917ceb59e9741.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

来看看打包后的文件，既有A、B

![image.png](https://upload-images.jianshu.io/upload_images/1480597-c993021d1b8aee12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**在`webpack`代码分割中使用`async`异步加载**

```js
module.exports = {
    entry: {
        "pageA": "./src/pageA",
        "pageB": "./src/pageB",
        "vendor": ['loash']//业务代码和第三方代码区分开,给loash单独打一个包
    },
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: '[name].bundle.js',
        chunkFilename: '[name].chunk.js'
    },
    plugins: [
    // add 异步模块
    new webpack.optimize.CommonsChunkPlugin({
        aysnc:'async-common',//异步共同的东西
        children: true,
        names: ['vendor','manifest']
        minChunks: Infinity
    })
     
     
    // 提取vendor、取业务代码manifest
     new webpack.optimize.CommonsChunkPlugin({
         //把entry的vendor代码和这里的common（webpack打包的）代码合并
         names: ['vendor','manifest']
         minChunks: Infinity
     })
     
    ]
}
```

```js
//src/subPageA.js


import './moduleA'

console.log('this. is subPageA')

export default 'subPageA'
```

```js
//src/subPageB.js


import './moduleA'

console.log('this. is subPageB')

export default 'subPageB'
```

```js
//src/subPageB.js


import './moduleA'

console.log('this. is moduleA')

export default 'moduleA'
```


```js
//src/pageA.js

// 异步加载不能include 否则会和pageA打包到一起
// require.include('./moduleA')

import _ from 'lodash'

var page = 'subPageA'

if(page === 'subPageA'){
 // 指定chunkName /** webpackChunkName: 'subPageA' **/ 
  import(/** webpackChunkName: 'subPageA' **/,'./subPageA').then(function(subPageA){
      console.log(subPageA)
  })
}else{
  import(/** webpackChunkName: 'subPageB' **/'./subPageB').then(function(subPageB){
      console.log(subPageB)
  })
}

// === lodash不在异步加载
//ensure的时候代码不会执行 需要在下面加载一次
//require.ensure(['lodash'],function(){
//    var _ = require('lodash')
//    _.join([1,2,3])
//},'vendor')// 指定chunk名称


export default 'pageA'
```

```js
//src/pageB.js

import _ from 'lodash'

var page = 'subPageB'

if(page === 'subPageA'){
 // 指定chunkName /** webpackChunkName: 'subPageA' **/ 
  import(/** webpackChunkName: 'subPageA' **/,'./subPageA').then(function(subPageA){
      console.log(subPageA)
  })
}else{
  import(/** webpackChunkName: 'subPageB' **/'./subPageB').then(function(subPageB){
      console.log(subPageB)
  })
}

// === lodash不在异步加载
//ensure的时候代码不会执行 需要在下面加载一次
//require.ensure(['lodash'],function(){
//    var _ = require('lodash')
//    _.join([1,2,3])
//},'vendor')// 指定chunk名称


export default 'pageB'
```

![image.png](https://upload-images.jianshu.io/upload_images/1480597-f12f6505f80cd43a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/1480597-29006f18038b558e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 这样就把`subpageA`和`subPageB`共同依赖的`moduleA`异步提取出来


### 3.5 处理 CSS 和 CSS 模块化

**引入css**

> 需要两个`loader`，`style-loader`(创建标签到文档流中)、`css-loader`(可以`import`一个样式文件，使得在`js`中可以使用)

**Style-Loader**

> `style-loader`除了本身，还有这几个`loader`

- `style-loader/url` 可以注入link标签到页面
- `style-loader/useable` 控制样式是否插入到页面中

Style-Loader的options

- `insertAt` （插入位置）
- `insertInto` （插入到`dom`）
- `singleton` （是否只使用一个`style` 标签）
- `transform` （转化，浏览器环境下，插入页面前）

例子

```js
module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        publicPath: './dist/', //指定从项目中哪里引入资源 
        filename: '[name].bundle.js'
    },
    module: {
        // loader解析从后往前处理
        rules: [
            {
                test: /\.css$/,
                use: [
                    {
                        //loader: 'style-loader',
                        loader: 'style-loader/url', //使用这个可以往页面注入link标签 而不是style,这个并不常用
                    },
                    {
                        //loader: 'css-loader',
                        loader: 'file-loader'//使用这个可以往页面注入link标签 而不是style 这个并不常用
                    }
                ]
            }
        ]
    }
}
```

**CSS-Loader**

options

- `alias` （解析的别名）
- `importLoader` （`@import` ）
- `Minimize` （是否压缩）
- `modules` （启用`css-modules`）

**CSS-Modules**

```
localIdentName: '[path][name]__[local]--[hash:base64:5]'
```

例子

```js
module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        publicPath: './dist/', //指定从项目中哪里引入资源 
        filename: '[name].bundle.js'
    },
    module: {
        // loader解析从后往前处理
        rules: [
            {
                test: /\.css$/,
                use: [
                    {
                        loader: 'style-loader',
                        options: { 
                           //合并多个style为一个
                            singleton:true
                        }
                    },
                    {
                        loader: 'css-loader',
                        options: {
                           minimize:true,
                           modules: true,
                           // css模块化
                           localIdentName: '[path][name]_[local]_[hash:base64:5]'
                        }
                    }
                ]
            }
        ]
    }
}
```


**配置Less / Sass**

```bash
npm install less-loader less  --save-dev
npm install sass-loader node-sass --save-dev
```


例子

```js
module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        publicPath: './dist/', //指定从项目中哪里引入资源 
        filename: '[name].bundle.js'
    },
    module: {
        // loader解析从后往前处理
        rules: [
            {
                test: /\.(css|less)$/,
                use: [
                    {
                        loader: 'style-loader',
                        options: { 
                           //合并多个style为一个
                            singleton:true
                        }
                    },
                    {
                        loader: 'css-loader',
                        options: {
                           minimize:true,
                           modules: true,
                           // css模块化
                           localIdentName: '[path][name]_[local]_[hash:base64:5]'
                        }
                    },
                    {
                        loader: 'less-loader'
                    },
                    {
                        loader: 'sass-loader'
                    }
                ]
            }
        ]
    }
}
```


**提取 CSS**

- `extract-loader`
- `ExtractTextWebpackPlugin`


例子

```js
module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        publicPath: './dist/', //指定从项目中哪里引入资源 
        filename: '[name].bundle.js'
    },
    module: {
        // loader解析从后往前处理
        rules: [
            {
                test: /\.(css|less)$/,
                use:
                ExtractTextWebpackPlugin.extra({
                    // 提取css并不会自动加入到文档中，需要在HTML手动加入css文件
                    fallback: {
                        loader: 'style-loader',
                        options: { 
                           //合并多个style为一个
                            singleton:true
                        }
                    },
                    // 处理css
                    use: [
                        {
                            loader: 'css-loader',
                            options: {
                               minimize:true,
                               modules: true,
                               // css模块化
                               localIdentName: '[path][name]_[local]_[hash:base64:5]'
                        }
                        },
                        {
                            loader: 'less-loader'
                        },
                        {
                            loader: 'sass-loader'
                        }
                    ]
                })
            }
        ]
    },
    plugins: [
        new ExtractTextWebpackPlugin({
            filename: '[name].min.css',
            allChunks: false //指定提取css范围，true所有import进来的css
        })
    ]
}
```

![image.png](https://upload-images.jianshu.io/upload_images/1480597-f9ad7e2b0f376596.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 3.6 PostCSS in Webpack

安装

- `postcss`
- `postcss-loader`
- `Autoprefixer`
- `cssnano`
- `postcss-cssnext`

例子

```js
module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        publicPath: './dist/', //指定从项目中哪里引入资源 
        filename: '[name].bundle.js'
    },
    module: {
        // loader解析从后往前处理
        rules: [
            {
                test: /\.(css|less)$/,
                use:
                ExtractTextWebpackPlugin.extra({
                    // 提取css并不会自动加入到文档中，需要在HTML手动加入css文件
                    fallback: {
                        loader: 'style-loader',
                        options: { 
                           //合并多个style为一个
                            singleton:true
                        }
                    },
                    // 处理css
                    use: [
                        {
                            loader: 'css-loader',
                            options: {
                               minimize:true,
                               modules: true,
                               // css模块化
                               localIdentName: '[path][name]_[local]_[hash:base64:5]'
                        }
                        },
                        {
                            loader: 'less-loader'
                        },
                        {
                            loader: 'sass-loader'
                        },
                        {
                            loader: 'postcss-loader',
                            options: {
                                ident: 'postcss',
                                plugins: [
                                    require('antoprefixer')()
                                ]
                            }
                        }
                    ]
                })
            }
        ]
    },
    plugins: [
        new ExtractTextWebpackPlugin({
            filename: '[name].min.css',
            allChunks: false //指定提取css范围，true所有import进来的css
        })
    ]
}
```

### 3.7 Tree shaking

#### 3.7.1 JS Tree shaking

**使用场景**

- 常规优化
- 引入第三方库的某一个功能

 例子

```js
module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        publicPath: './dist/', //指定从项目中哪里引入资源 
        filename: '[name].bundle.js'
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                use: [
                    {
                        loader: 'babel-loader',
                        options: {
                            presets: ['env'],
                            plugins: [
                                // lodash Tree shaking
                                'lodash'
                            ]
                        }
                    }
                ]
            }
        ]
    },
    plugins: [
        new ExtractTextWebpackPlugin({
            filename: '[name].min.css',
            allChunks: false //指定提取css范围，true所有import进来的css
        })
        // Tree shaking
        new webpack.optimize.UglifyJsPlugin({
            
        })
    ]
}
```

> 有些库不是es模块写的，并不能`tree shaking`。需要借助其他工具 

```
npm i babel-loader babel-core babel-preset-env babel-plugin-lodash --save
```

> lodash Tree不生效

- `lodash-es` --> no
- `babel-lugin-lodash` --->working

> 查看模块是否Tree Shaking方式：去第三方库index.js中看模块书写方式是否是es

#### 3.7.2 CSS Tree shaking

> 主要使用 `purifycss-webpack`

例子

```js
const glob = require('glob-all')

module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        publicPath: './dist/', //指定从项目中哪里引入资源 
        filename: '[name].bundle.js'
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                use: [
                    {
                        loader: 'babel-loader',
                        options: {
                            presets: ['env'],
                            plugins: [
                                // lodash Tree shaking
                                'lodash'
                            ]
                        }
                    }
                ]
            }
        ]
    },
    plugins: [
        new ExtractTextWebpackPlugin({
            filename: '[name].min.css',
            allChunks: false //指定提取css范围，true所有import进来的css
        })
        
        // 放到ExtractTextWebpackPlugin后面
        new PurifyCss({
            paths: glob.sync([
                path.join(__dirname,'./*.html'),
                path.join(__dirnname,'./src/*.js')
            ])
        })
        
        // Tree shaking
        new webpack.optimize.UglifyJsPlugin({
            
        })
    ]
}
```


## 四、由浅入深Webpack

### 4.1 文件处理 

#### 4.1.1 图片处理

- `css`中引入图片
- 自动合成雪碧图
- 压缩图片
- `Base64`编码

> 处理需要用到的`loader`

- `file-loader` `css`中引入图片
- `url-loader` `base64`编码
- `img-loader` 压缩图片
- `postcss-sprites`合成雪碧图

#### 4.1.2 处理雪碧图、base64、压缩图片

```js
module.exports = {
    module: {
         rules: [
            {
                test: /\.(css|less)$/,
                use:
                extractLess.extract({
                    // 提取css并不会自动加入到文档中，需要在HTML手动加入css文件
                    fallback: {
                        loader: 'style-loader',
                        options: { 
                           //合并多个style为一个
                            singleton:true
                        }
                    },
                    // 处理css
                    use: [
                        {
                            loader: 'css-loader',
                            options: {
                               minimize:true,
                               modules: true,
                               // css模块化
                               localIdentName: '[path][name]_[local]_[hash:base64:5]'
                        }
                        },
                        {
                            loader: 'less-loader'
                        },
                        {
                            loader: 'sass-loader'
                        },
                        {
                            loader: 'postcss-loader',
                            options: {
                                ident: 'postcss',
                                plugins: [
                                    // 合并雪碧图
                                    require('postcss-sprites')({
                                        // 指定雪碧图输出路径
                                        spritePath: 'dist/assets/imgs/sprites',
                                        retina: true // 处理苹果高清retina 图片命名需要 xx@2x.png,对应的图片的css大小设置也要减小一半 
                                    })
                                ]
                            }
                        }
                    ]
                })
            },
            {
                test: /\.(png|jpg|gif)$/,
                use:[
                    //{
                    //    loader: //'file-loader',//处理图片
                    //    options: {
                    //       publicPath:'',// 使得图片地址可以访问
                    //       outputPath: 'dist/'
                    //       useRelativePath:true
                        //}
                    
                    //}
                    // url-loader会把图片转成base64
                    {
                        loader: 'url-loader',
                        options: {
                            name: '[name].min.[ext]' //5kb内会转成base64 ,否则输出图片路径
                            limit: 1000, 
                            publicPath:'',
                            outputPath: 'dist/',
                            useRelativePath:true
                        }
                    },
                    // 压缩图片
                    {
                        loader: 'img-loader',
                        options: {
                            pngquant: {
                                //图片质量
                                quality:80
                            }
                        }
                    },
                    
                ]
            }
        ]
    }
}
```

#### 4.1.2 处理字体文件

- `file-loader`
- `url-loader`

```js
module.exports = {
    module: {
         rules: [
            {
                test: /\.(css|less)$/,
                use:
                extractLess.extract({
                    // 提取css并不会自动加入到文档中，需要在HTML手动加入css文件
                    fallback: {
                        loader: 'style-loader',
                        options: { 
                           //合并多个style为一个
                            singleton:true
                        }
                    },
                    // 处理css
                    use: [
                        {
                            loader: 'css-loader',
                            options: {
                               minimize:true,
                               modules: true,
                               // css模块化
                               localIdentName: '[path][name]_[local]_[hash:base64:5]'
                        }
                        },
                        {
                            loader: 'less-loader'
                        },
                        {
                            loader: 'sass-loader'
                        },
                        {
                            loader: 'postcss-loader',
                            options: {
                                ident: 'postcss',
                                plugins: [
                                    // 合并雪碧图
                                    require('postcss-sprites')({
                                        // 指定雪碧图输出路径
                                        spritePath: 'dist/assets/imgs/sprites',
                                        retina: true // 处理苹果高清retina 图片命名需要 xx@2x.png,对应的图片的css大小设置也要减小一半 
                                    })
                                ]
                            }
                        }
                    ]
                })
            },
            {
                test: /\.(png|jpg|gif)$/,
                use:[
                    //{
                    //    loader: //'file-loader',//处理图片
                    //    options: {
                    //       publicPath:'',// 使得图片地址可以访问
                    //       outputPath: 'dist/'
                    //       useRelativePath:true
                        //}
                    
                    //}
                    // url-loader会把图片转成base64
                    {
                        loader: 'url-loader',
                        options: {
                            name: '[name].min.[ext]' //5kb内会转成base64 ,否则输出图片路径
                            limit: 1000, 
                            publicPath:'',
                            outputPath: 'dist/',
                            useRelativePath:true
                        }
                    },
                    // 压缩图片
                    {
                        loader: 'img-loader',
                        options: {
                            pngquant: {
                                //图片质量
                                quality:80
                            }
                        }
                    },
                    // 处理字体文件
                    {
                        test: /\.(eot|woff2|woff|ttf|svg)$/,
                        use: [
                            {
                                loader:'url-loader',
                                options: {
                                    name: '[name].min.[ext]' //5kb内会转成base64 ,否则输出图片路径
                                    limit: 5000, 
                                    publicPath:'',
                                    outputPath: 'dist/',
                                    useRelativePath:true
                                }
                            }
                        ]
                    }
                    
                ]
            }
        ]
    }
}
```


#### 4.1.3 处理第三方 JS 库

> 处理第三方库 用到`providePlugin`、`imports-loader`

**1.providePlugin**

以引入`jQuery`为例

```
npm i jquery
```

```
module.exports = {
    plugins: [
        new webpack.ProvidePlugin({
            // 在全局注入jQuery变量
            $:'jquery'
        })
    ]
}
```

引入本地`libs`中的`jQuery`

```
module.exports = {
    resolve: {
        alias: {
            // $确切匹配 把jQuery这个关键字解析到某个目录下
            jquery$:path.resolve(__dirname,'src/libs/jquery.min.js')
        }
    },
    plugins: [
        new webpack.ProvidePlugin({
            // 在全局注入jQuery变量
            $:'jquery'
        })
    ]
}
```

**2.imports-loader**

```js
module.exports = {
    module:{
        rules:[
            {
                test:path.resolve(__dirname,'src/app.js'),
                use:[
                    {
                        loader: 'imports-loader',
                        options: {
                            $: 'jquery'
                        }
                    }
                ]
            }
        ]
    }
}
```


### 4.2 HTML in webpack(自动生成HTML)

> 自动生成`HTML`，把这个页面需要的`js`、`css`插入到页面中


#### 4.2.1 生成 HTML


> `htmlWebpackPlugin`

options

- `template`
- `filename`
- `minify` 是否压缩
- `chunks` 
- `inject` 是否让`css、js`通过标签形式插入到页面中


```js
module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: '[name].bundle.js'
    },
    plugin:[
        new htmlWebpackPlugin({
            filename:'index.html', // 不传默认index.html
            template: './index.html',//传入模板
            inject:true,//控制js\css是否插入到页面
            minify: {
                collapseWhitespace:true //压缩空格
            },
            chunks:['app']//指定chunks会把跟这个入口相关的chunks插入到页面中
        })
    ]
}
```


#### 4.2.2 HTML 中引入图片

> 需要用到`html-loader`

**html-loader**

options

- `attrs: [img:src]`

```js
module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: '[name].bundle.js',
        publicPath:'/' //网站路径为/ 图片等资源引用不会发生错误
    },
    module:{
        rules:[
            {
                loader: 'url-loader',
                options: {
                    name: '[name].min.[ext]' //5kb内会转成base64 ,否则输出图片路径
                    limit: 1000, 
                    publicPath:'',
                    outputPath: 'assets/imgs/',
                    //useRelativePath:true // 这里不能使用这个 因为图片路径在HTML中、css中都存在，打包的时候图片会放错地方。需要用到绝对路径，在output指定publicPath:'/'
                }
            },
            // 处理HTML中的图片引用路径问题
            {
                test: /\.html$/,
                use: [
                    {
                        loader: 'html-loader',
                        options: {
                            attrs: ['img:src','img:data-src']
                        }
                    }
                ]
            }
        ]
    }
}
```

**require在HTML中引入图片**

```html
<img src="${require('./public/imgs/xx.png)}" />
```


#### 4.2.3 配合优化

> 提前载入webpack加载代码

- `inline-manifest-webpack-plugin`
- `html-webpack-inline-chunk-plugin`

建议使用 `html-webpack-inline-chunk-plugin`

```
npm i html-webpack-inline-chunk-plugin
```

```js
var HTMLInlieChunk = require('html-webpack-inline-chunk-plugin')

module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: '[name].bundle.js',
        publicPath:'/'
    },
    plugin:[
        new htmlWebpackPlugin({
            filename:'index.html', // 不传默认index.html
            template: './index.html',//传入模板
            inject:true,//控制js\css是否插入到页面
            minify: {
                collapseWhitespace:true //压缩空格
            },
            chunks:['app']//指定chunks会把跟这个入口相关的chunks插入到页面中
        })
        new webpack.mdtimize.CommonsChunkPlugin({
            name: 'manifest'
        })
        new HTMLInlieChunk({
            inlineChunks: ['manifest'] //把webpack生成的manifest提取到HTML文件script中，减少请求
        })
    ]
}
```


## 五、Webpack 环境配置





## 六、webpack实战场景
