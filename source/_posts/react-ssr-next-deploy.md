---
title: next项目部署到服务器pm2进程守护
date: 2018-11-18 13:32:33
tags: 
   - React
   - SSR
categories: Front-End
---


## 一、 `npm run export`导出文件上传到`CDN`

> 在项目中执行`npm run export`后导出`outCDN`文件上传到`CDN`

```js
// scripts/upload.js

const fs = require('fs');
const path = require('path');
const OSS = require('ali-oss');

const filePath = path.join(__dirname,'../outCDN');
const excludeFiles = ['index.html']

const client = new OSS({
  region: 'oss-cn-shenzhen',
  accessKeyId: '',
  accessKeySecret: '',
  bucket: ''
});

// 遍历文件夹中所有文件
async function uploadFile(filePath){
    //根据文件路径读取文件，返回文件列表
    fs.readdir(filePath,async function(err,files){
        if(err){
            console.warn(err)
        }else{
            //遍历读取到的文件列表
            files.forEach(async function(filename){
                //获取当前文件的绝对路径
                const filedir = path.join(filePath,filename);
                //根据文件路径获取文件信息，返回一个fs.Stats对象
                fs.stat(filedir,async function(eror,stats){
                    if(eror){
                        console.warn('获取文件stats失败');
                    }else{
                        const isFile = stats.isFile();//是文件
                        const isDir = stats.isDirectory();//是文件夹
                        if(!excludeFiles.includes(filename) && isFile){
                            const fileKey = `${filedir.split('outCDN/').pop()}`

                            try {
                                // object表示上传到OSS的Object名称，localfile表示本地文件或者文件路径
                                let data = await client.put(fileKey,filedir);

                                console.error('upload success: %j', data);
                            } catch(err) {
                                console.error('upload failed: %j', err);
                            }
                        }
                        if(isDir){
                            uploadFile(filedir);//递归，如果是文件夹，就继续遍历该文件夹下面的文件
                        }
                    }
                })
            });
        }
    });
}


uploadFile(filePath)
```

## 二、处理`next build`后的文件

> 执行`next build`以后,把`.next`、`package.json`、`server.js`、`next.config.js`、`ecosystem.json` 拷贝到一个文件夹统一管理，最后部署这个文件夹下的内容即可


```js
// scripts/copyFiles.js

const fs = require( 'fs' ),
    stat = fs.stat;

const path = require('path')

const includeFiles = ['package.json','server.js','next.config.js','ecosystem.json']

/*
 * 复制目录中的所有文件包括子目录
 * @param{ String } 需要复制的目录
 * @param{ String } 复制到指定的目录
 */
const readDir = function( src, dst ){
    // 读取目录中的所有文件/目录
    fs.readdir( src, function( err, paths ){
        if( err ){
            throw err;
        }
        paths.forEach(function( filename ){
            var _src = src + '/' + filename,
                _dst = dst + '/' + filename,
                readable, writable;

            stat( _src, function( err, st ){
                if( err ){
                    throw err;
                }
                // 判断是否为文件
                if( st.isFile()){
                    // 创建读取流
                    readable = fs.createReadStream( _src );
                    // 创建写入流
                    writable = fs.createWriteStream( _dst );
                    // 通过管道来传输流
                    readable.pipe( writable );
                }
                // 如果是目录则递归调用自身
                else if( st.isDirectory()){
                    copyDir( _src, _dst, readDir );
                }
            });
        });
    });
};

// 在复制目录前需要判断该目录是否存在，不存在需要先创建目录
const copyDir = function( src, dst, callback ){
    fs.exists( dst, function( exists ){
        // 已存在
        if( exists ){
            callback( src, dst );
        }
        // 不存在
        else{
            fs.mkdir( dst, function(){
                callback( src, dst );
            });
        }
    });
};

const copyFile = ()=>{
  includeFiles.forEach(filename=>{
    fs.createReadStream(path.join(__dirname,'../'+filename)).pipe(fs.createWriteStream(path.join(__dirname,'../deployBuildFiles',filename)))
    console.log('拷贝完成!')
  })
}

// 复制目录
copyDir( '.next', 'deployBuildFiles/.next', readDir);

// 拷贝文件
copyFile()
```

## 三、pm2之ecosystem部署项目

> `PM2`部署应用流程，通过`pm2`的配置文件来部署
> http://pm2.keymetrics.io/docs/usage/deployment/

### 3.1 配置部署脚本文件

> 在项目根目录添加`pm2`的部署脚本文件 `ecosystem.json`

```nginx
{
  "apps": [
      {
        "name": "goodsapp", //pm2运行的应用名称
        "script": "server.js",//服务启动入口
        "env":{
            "COMON_VARIABLE": "true"
        },
        "env_production": {
            "NODE_ENV": "production", //env
            "HOST": "localhost"
        }
      }
  ],
  "deploy": {
      // 应用名称，可以自定义，最后这样使用 pm2 deploy ecosystem.json goodsapp
      "goodsapp": {
          "user": "user_00",// 服务器用户名
          "host": ['192.68.1.201'],//服务器ip地址 可写多个
          "ref": "origin/master",//从指定分支拉取代码
          "repo": "http://p.yesdat.com/diffusion/49/goodsh.git",
          "path": "/data/poetry/testDir/prev-goods.yesdat.com", //上传本地目录到服务器
          "ssh_options": "StrictHostKeyChecking=no",
          "post-deploy": "npm install --registry=https://registry.npm.taobao.org && npm install && pm2 startOrRestart ecosystem.json --env production",//部署脚本
          "env": {
              "NODE_ENV": "production"
          }
      }
  }
}
```

> 或者简单`scp`上传到服务器

```
scp -P36000  -r deployBuildFiles/.next user_00@192.168.1.201:/home/data/services/goods-prev.yesdat.com/
```

### 3.2 部署Nginx配置规则

> 在`nginx`安装目录下的`vhost`中新建一个`xx-3000.conf`的配置文件

- 在Nginx目录`/etc/nginx`下执行 `sudo /usr/sbin/nginx -t` 检测配置文件是否成功

![image.png](https://upload-images.jianshu.io/upload_images/1480597-b20774768f5457db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```nginx
upstream goodsapp { // website项目的目录名称
  server 127.0.0.1:3000; // 服务器上的本地启动入口，端口对应项目中server.js中的端口
}

// 配置server
server {
    listen 80;
    server_name prev-goods.yesdat.com; //指向的域名

    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-Nginx-Proxy true;

        proxy_pass http://goodsapp; // 请求将会转发到goodsapp的node服务下
        proxy_redirect off;
    }

    // 处理静态资源
    location ~* ^.+\.(jpg|jpeg|gif|png|ico|css|js|pdf|txt) {
        root /data/goodsapp/static; //请求转发到静态资源路径
    }
}
```

### 3.3 本地项目根执行的命令

- `pm2 deploy ecosystem.json goodsapp setup` 初始化
- `pm2 deploy ecosystem.json goodsapp` 部署


### 3.4 部署更多参考

- [next.js、nuxt.js等服务端渲染框架构建的项目部署到服务器，并用PM2守护程序](https://segmentfault.com/a/1190000012774650)
- [学习 Next.js: 部署](https://segmentfault.com/a/1190000010992618)
- [Deployment on Nginx's reverse proxy](https://github.com/zeit/next.js/wiki/Deployment-on-Nginx's-reverse-proxy)
