---
title: "Koa post提交数据与静态资源中间件"
date: 2020-08-10T12:03:41+08:00
draft: false
toc: true
url: "/note/2020/08/10/koa04"
categories: 
- 笔记
tags: 
- Koa
- Node.js
---
# 原生Node.js获取post提交的数据
## 表单
```
<form action="/doAdd" method="post">
    用户名: <input type="text" name="username"/>
    <br/>
    <br/>
    密　码: <input type="password" name="password"/>
    <br/>
    <br/>
    <button type="submit">提交</button>
</form>
```
## 在module/common.js封装获取数据的方法
```
exports.getPostData=function(ctx){
    //获取数据  异步
    return new Promise(function(resolve,reject){
           try{
               let str='';
               ctx.req.on('data',function(chunk){
                   str+=chunk;
               })

               ctx.req.on('end',function(chunk){

                   resolve(str)
               })
           }catch(err){
                reject(err)
           }
    })
}
```
## 获取post提交的数据
```
//引用获取数据方法
var common = require('./module/common.js');
//接收post提交的数据
router.post('/doAdd',async (ctx)=>{

    //原生nodejs 在koa中获取表单提交的数据
    var data=await common.getPostData(ctx);

    console.log(data);
    ctx.body=data;
})
```
# 使用koa-bodyparser中间件获取post提交的数据
## 安装并引入koa-bodyparser
```
cnpm install koa-bodyparser --save

var bodyParser = require('koa-bodyparser');
```
## 配置kooa-bodyparser中间件
```
app.use(bodyParser());
```
## 获取post提交的数据
```
router.post('/doAdd',async (ctx)=>{

    ctx.body = ctx.request.body;    //获取表单提交的数据
    
})
```
# 静态资源中间件
## 安装koa-static并引入
```
cnpm install koa-static --save

const static = require('koa-static');
```
## 配置koa-static中间件
```
app.use(static('./static'));
```
## 引入静态资源
```
<link rel="stylesheet" href="css/basic.css"/>   //即可成功引入static/css/basic.css
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |


