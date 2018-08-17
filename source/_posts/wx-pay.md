---
title: 小程序之支付
date: 2018-08-17 14:20:20
tags: 小程序
categories: Front-End
---

一、小程序调起支付API
---

> 小程序调起支付数据签名字段列表

|字段名|	变量名|	必填|	类型|	示例值|	描述|
|---|---|---|---|---|---|
|小程序ID|	`appId`|	是|	`String`|	`wxd678efh567hg6787`	|微信分配的小程序ID|
|时间戳	|`timeStamp`|	是|	`String`|	`1490840662`	|时间戳从1970年1月1日00:00:00至今的秒数,即当前的时间|
|随机串|	`nonceStr`|	是|	`String`|	`5K8264ILTKCH16CQ2502SI8ZNMTM67VS`|	随机字符串，不长于`32`位。推荐随机数生成算法|
|数据包	|`package`	|是|	`String`|	`prepay_id=wx2017033010242291fcfe0db70013231072`|	统一下单接口返回的 `prepay_id` 参数值，提交格式如：`prepay_id=wx2017033010242291fcfe0db70013231072`|
|签名方式|	`signType`|	是|	`String`|	`MD5`|	签名类型，默认为`MD5`，支持`HMAC-SHA256`和`MD5`。注意此处需与统一下单的签名类型一致|

> `paySign = MD5(appId=wxd678efh567hg6787&nonceStr=5K8264ILTKCH16CQ2502SI8ZNMTM67VS&package=prepay_id=wx2017033010242291fcfe0db70013231072&signType=MD5&timeStamp=1490840662&key=qazwsxedcrfvtgbyhnujmikolp111111) = 22D9B4E54AB1950F51E0649E8810ACD6`

二、调用wx.requestPayment发起微信支付
---


```javascript
wx.request({
    // 调用业务接口主要是返回微信支付需要的参数 具体看后台处理
    url:'http://api.test.com/v1/pre-pay',
    data:{
        // 参数
    }
    success:res=>{
        // 发起微信支付
        wx.requestPayment({
        timeStamp:res.timeStamp,
        nonceStr:res.nonceStr,
        package:res.package,
        signType:res.signType,
        paySign:'',
        success:d=>{},
        fail:d=>{},
        complete:d=>{}//调用成功、失败都会执行
    })
    }
})

// wx.requestPayment @response
success	requestPayment:ok	调用支付成功
fail	requestPayment:fail cancel	用户取消支付
fail	requestPayment:fail (detail message)	调用支付失败，其中 detail message 为后台返回的详细失败原因
```


