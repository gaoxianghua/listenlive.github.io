---
title: "Koa路由和中间件"
date: 2020-08-09T11:01:49+08:00
draft: false
toc: true
url: "/note/2020/08/09/koa02"
categories: 
- 笔记
tags: 
- Koa
- Node.js
---
# Koa路由的安装和配置
## 安装Koa路由
```
cnpm install koa-router --save
```
## Koa路由的使用
```

var Koa = require('koa');   //引入Koa

var Router = require('koa-router'); //引入路由

var app = new Koa();

var router = new Router();

//配置路由
router.get('/',async function(ctx){   //ctx context 包含request和responses等信息
    ctx.body="首页";
}).get('/news',async (ctx)=>{
    ctx.body="新闻页";
})

//启动路由
app
    .use(router.routes())
    .use(router.allowedMethods());

app.listen(3000);

```
## Koa Get传值及获取get传值
```
router.get('/newscontent',async (ctx)=>{

    /*在 koa2 中 GET 传值通过 request 接收，但是接收的方法有两种：query 和 querystring。
     query：返回的是格式化好的参数对象。
     querystring：返回的是请求字符串。*/

    //从ctx中读取get传值

    console.log(ctx.query);  //{ aid: '123' }       获取的是对象   用的最多的方式      ******推荐

    console.log(ctx.querystring);  //aid=123&name=zhangsan      获取的是一个字符串

    console.log(ctx.url);   //获取url地址

    //ctx里面的request里面获取get传值

    console.log(ctx.request.url);
    console.log(ctx.request.query);   //{ aid: '123', name: 'zhangsan' }  对象
    console.log(ctx.request.querystring);   //aid=123&name=zhangsan

    ctx.body="新闻详情";

})
```
## Koa动态路由
```
router.get('/package/:aid/:cid',async (ctx)=>{

    //获取动态路由的传值

    console.log(ctx.params);  //{ aid: '123', cid: '456' }

    ctx.body="新闻详情";

})
```
# Koa中间件
## 应用级中间件
```
var Koa = require('koa');   //引入Koa

var Router = require('koa-router'); //引入路由

var app = new Koa();

var router = new Router();

//中间件
app.use(async (ctx,next)=>{
    ctx.body='这是一个中间件';
    console.log(new Date());

    await next();   /*当前路由匹配完成后继续向下匹配*/
});

//配置路由
router.get('/',async function(ctx){   //ctx context 包含request和responses等信息
    ctx.body="首页";
});
router.get('/news',async (ctx)=>{
    ctx.body="新闻页";
});
router.get('/login',async (ctx)=>{
    ctx.body="登录";
});

//启动路由
app
    .use(router.routes())
    .use(router.allowedMethods());

app.listen(3000);
```

## 路由级中间件
```
//匹配到news路由之后继续向下匹配路由
router.get('/news',async (ctx，next)=>{

    console.log('这是新闻1');

    await.next();
});

router.get('/news',async (ctx)=>{
    ctx.body="这是新闻2";
});
```
## 错误处理中间件
```
app.use(async (ctx,next)=>{
    next();
    if(ctx.status == 404){
        ctx.status = 404;
        ctx.body = '404页面';
    }
});
```
```
app.use(async (ctx,next)=>{
    console.log('这是一个中间件01');
    next();

    if(ctx.status==404){   /*如果页面找不到*/
        ctx.status = 404;
        ctx.body="这是一个 404 页面"
    }else{
        console.log(ctx.url);
    }
})

router.get('/',async (ctx)=>{

    ctx.body="首页";

})
router.get('/news',async (ctx)=>{
    console.log('这是新闻2');
    ctx.body='这是一个新闻';
})
router.get('/login',async (ctx)=>{
    ctx.body="新闻列表页面";
})


app.use(router.routes());   /*启动路由*/
app.use(router.allowedMethods());
app.listen(3000);
```
## Koa中间件的执行流程
```
app.use(async (ctx,next)=>{
    console.log('1、这是第一个中间件01');
    await next();

    console.log('5、匹配路由完成以后又会返回来执行中间件');
})

app.use(async (ctx,next)=>{
    console.log('2、这是第二个中间件02');
    await next();

    console.log('4、匹配路由完成以后又会返回来执行中间件');
})

router.get('/',async (ctx)=>{

    ctx.body="首页";

})
router.get('/news',async (ctx)=>{

    console.log('3、匹配到了news这个路由');
    ctx.body='这是一个新闻';
})

//运行顺序 1、2、3、4、5、

app.use(router.routes());   /*启动路由*/
app.use(router.allowedMethods());
app.listen(3000);
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |