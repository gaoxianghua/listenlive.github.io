---
title: "Koa ejs模板引擎与art-template模板引擎"
date: 2020-08-10T10:19:42+08:00
draft: false
toc: true
url: "/note/2020/08/10/koa03"
categories: 
- 笔记
tags: 
- Koa
- 前端框架
- Node.js
---
# Koa ejs的安装与使用
## 安装koa-views
```
cnpm install koa-views --save
```
## 安装ejs
```
cnpm install ejs --save 
```
## 配置模板引擎中间件
```
var views = require('koa-views');

app.use(views('views',{
    extension:'ejs'
}));        //这样配置模板的后缀名是.ejs

//第二种配置方式
app.use(views('views',{map:{html:'ejs'}})); //这样配置模板的后缀名是.html
```
## 渲染模板
```
router.get('/',async function(ctx){
    await ctx.render('index');  //渲染模板
});
```
## 将数据渲染到模板
```
router.get('/',async function(ctx){
    let title = '首页|home' 
    await ctx.render('index',{
        title:title
    });
});
```
模板文件   
```
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title><%=title%></title>
</head>
<body>
    <h3>ejs模板</h3>
</body>
</html>
```
## 循环遍历数据
```
router.get('/',async function(ctx){   //ctx context 包含request和responses等信息
    let title = 'Home'
    await ctx.render('index',{
        title:title,
    });
}).get('/news',async (ctx)=>{
    let list = ['11','aa','zz','bb','ff','kk'];
    await ctx.render('news',{
        list:list,
    });
});
```
模板文件写法
```
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>新闻列表</title>
</head>
<body>
    <div>
        <ul>
        <%for(var i=0;i<list.length;i++){%>
            <li><%=list[i]%></li>
        <%}%>
        </ul>
    </div>
</body>
</html>
```
![新闻列表](/images/note/202008101124.png)
## 模板的引用
```
<% include public/header.ejs%>  //表示引用public目录下的header.ejs模板
```
## 解析HTML标签
```
let content = '<h2>Content</h2>';

<%-content%>        //解析HTML标签
```
## 使用中间件配置公共的数据 在模板中可以直接使用
```
app.use(async (ctx,next)=>{
    ctx.state.userinfo='zhangsan';
    
    await next();   /*继续向下匹配路由*/
})
```
# art-template模板引擎（推荐使用）
## 安装[art-template](http://aui.github.io/art-template/)并引入
```
cnpm install --save art-template
cnpm install --save koa-art-template

path=require('path');
const render = require('koa-art-template');
```
## 配置art-template模板引擎
```
render(app, {
    root: path.join(__dirname, 'views'),   // 视图的位置
    extname: '.html',  // 后缀名
    debug: process.env.NODE_ENV !== 'production'  //是否开启调试模式

});
```
## 渲染模板
```
await ctx.render('user');
```
## 模板语法
[**官方文档**](http://aui.github.io/art-template/zh-cn/docs/syntax.html)
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

