---
title: "Koa Cookie与Session"
date: 2020-08-10T14:52:11+08:00
draft: false
toc: true
url: "/note/2020/08/10/koa05"
categories: 
- 笔记
tags: 
- Koa
- Node.js
---
# Cookie
## Koa中设置Cookie值
```
ctx.cookies.set(name,value,[options]);
```
![](/images/note/202008101529.png)
## Koa中获取Cookie值
```
ctx.cookies.get('name');
```
## Koa中使用Buffer设置中文Cookie
```
//设置中文Cookie
router.get('/',async (ctx)=>{

    var userinfo=new Buffer('张三').toString('base64');
     ctx.cookies.set('userinfo',userinfo,{
        maxAge:60*1000*60
     });
})
//获取中文Cookie
router.get('/news',async (ctx)=>{

    var data=ctx.cookies.get('userinfo');
    var userinfo=new Buffer(data, 'base64').toString();

    console.log(userinfo);
})
```
# Session
## 安装并引入koa-session
```
cnpm install koa-session --save

const session = require('koa-session');
```
## 配置koa-session中间件
```
app.keys = ['some secret hurr'];
const CONFIG = {
key: 'koa:sess', //cookie key (default is koa:sess)
maxAge: 86400000, // cookie 的过期时间 maxAge in ms (default is 1 days)
overwrite: true, //是否可以 overwrite (默认 default true)
httpOnly: true, //cookie 是否只有服务器端可以访问 httpOnly or not (default true)
signed: true, //签名默认 true
rolling: false, //在每次请求时强行设置 cookie，这将重置 cookie 过期时间（默认：false）
renew: false, //(boolean) renew session when session is nearly expired,
};
app.use(session(CONFIG, app));
```
## 设置与获取Session值
```
//设置值
 ctx.session.username = "张三";
//获取值
 ctx.session.username
```

___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |