---
title: vue-axios请求数据（十二）
date: 2018-08-28 15:35:32
tags: Vue
categories: Front-End
---

## 一、axios 简介


> `axios` 是一个基于`Promise `用于浏览器和 `nodejs` 的 `HTTP` 客户端，它本身具有以下特征：

- 从浏览器中创建 `XMLHttpRequest`
- 从 `node.js` 发出 `http` 请求
- 支持` Promise API`
- 拦截请求和响应
- 转换请求和响应数据
- 取消请求
- 自动转换`JSON`数据
- 客户端支持防止` CSRF/XSRF`



## 二、axios基础


- `axios.request（config）`
- `axios.get（url [，config]）`
- `axios.delete（url [，config]）`
- `axios.head（url [，config]）`
- `axios.options（url [，config]）`
- `axios.post（url [，data [，config]]`）
- `axios.put（url [，data [，config]]）`
- `axios.patch（url [，data [，config]]）`



## 三、执行 GET 请求


```javascript
// 向具有指定ID的用户发出请求
axios.get('/user?ID=12345')
.then(function (res) {
    console.log(res);
})
.catch(function (error) {
    console.log(error);
});
 
// 也可以通过 params 对象传递参数
axios.get('/user', {
    params: {
        ID: 12345
    }
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});
```

## 四、执行 POST 请求



```javascript
axios.post('/user', {
    userId:"123"
},{
    headers:{
        token:"abc"
    }
})
.then(function (res) {
    console.log(res);
})
.catch(function (error) {
    console.log(error);
});
```

## 五、通过配置方式发送请求


> `get`请求是发送参数，在`params`中定义。而`POST`请求是发送`request body`,需要在`data`中定义

```javascript
// get 在params中定义
axios({
    url:"pakage.json",
    method:"get",
    params:{
        userId:"123"
    },
    headers:{
        token:"http-test"
    }
}).then(res=>{
    console.log(res.data);
})

// post 在data中定义
axios({
    url:"pakage.json",
    method:"post",
    data:{
        userId:"123"
    },
    headers:{
        token:"http-test"
    }
}).then(res=>{
    console.log(res.data);
})
```


## 六、执行多个并发请求


```javascript
function getUserAcount(){
    // 返回一个promise对象
    return axios.get("/user/1234");
}
function getUserPermissions(){
    // 返回一个promise对象
    return axios.get("/user/1234/getUserPermissions");
}

//一次性返回两个接口
axios.all([getUserAccount(),getUserPerssions()]).then(axios.spread((acct, perms) => {
    // spread展开两个返回的结果
    //两个请求现已完成
}))
```

## 七、全局拦截


```javascript
new Vue({
    el:"app",
    data:{
        msg:""
    },
    // 初始化生命周期的一个函数
    mounted:function(){
     //拦截请求之前
       axios.interceptors.request.use(config=>{
           //这里做一些拦截操作,拦截用户的请求 请求之前做一些loading处理
           return config;
       })
       //拦截响应之后处理
       axios.interceptors.response.use(response=>{
           //这里做一些拦截操作,响应以后做什么，在返回数据
           return response;
       })
    },

    methods: {
        get: function() {
        
        },
        post:function() {
           
        }
    }
})
```

## 八、请求封装、异常统一处理

> vue中采用axios处理网络请求，避免请求接口重复代码，以及各种网络情况造成的异常情况的判断，采用axios请求封装和异常拦截操作

### 8.1 axios 请求封装

```javascript
//  引入axios文件包
import axios from 'axios'

// POST 方法封装  (参数处理)
export const postRequest = (url, params) => {
  return axios({
    method: 'post',
    url: url,
    data: params,
    transformRequest: [function (data) {
      let ret = ''
      for (let it in data) {
        ret += encodeURIComponent(it) + '=' + encodeURIComponent(data[it]) + '&'
      }
      return ret
    }],
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded'
    }
  });
}

// POST 方法封装  (文件上传)
export const uploadFileRequest = (url, params) => {
  return axios({
    method: 'post',
    url: url,
    data: params,
    headers: {
      'Content-Type': 'multipart/form-data'
    }
  });
}

//  GET 方法封装
export const getRequest = (url) => {
  return axios({
    method: 'get',
    url: url
  });
}

//  PUT 方法封装
export const putRequest = (url, params) => {
  return axios({
    method: 'put',
    url: url,
    data: params,
    transformRequest: [function (data) {
      let ret = ''
      for (let it in data) {
        ret += encodeURIComponent(it) + '=' + encodeURIComponent(data[it]) + '&'
      }
      return ret
    }],
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded'
    }
  });
}

//  DELETE 方法封装
export const deleteRequest = (url) => {
  return axios({
    method: 'delete',
    url: url
  });
}
```

### 8.2 axios请求异常统一处理

```javascript
// 引入依赖包
import axios from 'axios'
import {Message} from 'element-ui'

//  REQUEST 请求异常拦截
axios.interceptors.request.use(config=> {
 //==========  所有请求之前都要执行的操作  ==============
  return config;
}, err=> {
 //==================  错误处理  ====================
  Message.error({message: '请求超时!'});
  return Promise.resolve(err);
})

//  RESPONSE 响应异常拦截
axios.interceptors.response.use(data=> {
//==============  所有请求完成后都要执行的操作  ==================

// 第一种方式

    // 根据返回的code值来做不同的处理（和后端约定）
    switch (data.code) {
        case '0':
            // exp: 修复iPhone 6+ 微信点击返回出现页面空白的问题
            if (isIOS()) {
                // 异步以保证数据已渲染到页面上
                setTimeout(() => {
                    // 通过滚动强制浏览器进行页面重绘
                    document.body.scrollTop += 1
                }, 0)
            }
            // 这一步保证数据返回，如果没有return则会走接下来的代码，不是未登录就是报错
            return data

        // 需要重新登录
        case 'SHIRO_E5001':
            // 微信生产环境下授权登录
            if (isWeChat() && IS_PRODUCTION) {
                axios.get(apis.common.wechat.authorizeUrl).then(({ result }) => {
                    location.replace(global.decodeURIComponent(result))
                })
            } else {
                // 否则跳转到h5登录并带上跳转路由
                const search = encodeSearchParams({
                    next: location.href,
                })

                location.replace(`/user/login`)
            }

            // 不显示提示消息
            data.description = ''
            break

        default:
    }
    // 若不是正确的返回code，且已经登录，就抛出错误
    const err = new Error(data.description)

    err.data = data
    err.response = response

  // 第二种方式，仅对200和error状态处理
  if (data.status && data.status == 200 && data.data.status == 'error') {
    Message.error({message: data.data.msg});
    return;
  }
  return data;
}, err=> {
//==============  错误处理  ====================
   if (err && err.response) {
        switch (err.response.status) {
            case 400: err.message = '请求错误(400)'; break;
            case 401: err.message = '未授权，请重新登录(401)'; break;
            case 403: err.message = '拒绝访问(403)'; break;
            case 404: err.message = '请求出错(404)'; break;
            case 408: err.message = '请求超时(408)'; break;
            case 500: err.message = '服务器错误(500)'; break;
            case 501: err.message = '服务未实现(501)'; break;
            case 502: err.message = '网络错误(502)'; break;
            case 503: err.message = '服务不可用(503)'; break;
            case 504: err.message = '网络超时(504)'; break;
            case 505: err.message = 'HTTP版本不受支持(505)'; break;
            default: err.message = `连接出错(${err.response.status})!`;
        }
    } else {
        err.message = '连接服务器失败!'
    }
  Message.err( {message: err.message } )
  return Promise.resolve(err);
})
```

> 请求出错的时候执行的是：`Promise.resolve(err)`;，而不是`Promise.reject(err)`;，这样无论请求成功还是失败，在成功的回调中都能收到通知


### 8.3 vue 项目中使用该方法


> 在main.js中导入所有请求方法

```javascript
//  导入所有请求方法
import {getRequest,postRequest,deleteRequest,putRequest} from './utils/api'
```


> 将请求方法添加至 `Vue.prototype` 上

```javascript
//  向VUE的原型上添加请求方法
Vue.prototype.getRequest = getRequest;
Vue.prototype.postRequest = postRequest;
Vue.prototype.deleteRequest = deleteRequest;
Vue.prototype.putRequest = putRequest;
```

> 发送请求（请求方法的调用）

```javascript
//  发送网络请求
this.postRequest('/login', {userName，password}).then(resp=> {
        ...
    }
});

```
