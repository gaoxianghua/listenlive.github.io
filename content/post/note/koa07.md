---
title: "Koa使用require-directory实现路由的自动加载"
date: 2020-08-13T14:10:35+08:00
draft: false
toc: true
url: "/note/2020/08/13/koa07"
categories: 
- 笔记
tags: 
- Koa
- Node.js
---
## require-directory的安装与使用
```
const Koa = require('koa');
const app = new Koa();

const requireDirectory = require('require-directory');
const Router = require('koa-router');
const modules = requireDirectory(module,'./api',{
    visit:whenLoadModule
})

function whenLoadModule(obj){
    if(obj instanceof Router){
        app.use(obj.routes())
    }
}

app.listen(3000);
```
## 路由文件的写法api/v1/book.js
```
const Router = require('koa-router');


const router = new Router();

router.get('/v1/book/latest',(ctx,next)=>{
    ctx.body={key:'books'}
    
})

module.exports = router
```
## 目录结构
![目录结构](/images/note/202008131416.png)

___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |