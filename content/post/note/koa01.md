---
title: "Koa框架安装及ES6基础语法"
date: 2020-08-09T08:59:12+08:00
draft: false
toc: true
url: "/note/2020/08/09/koa01"
categories: 
- 笔记
tags: 
- Koa
- Node.js
---
# 安装Koa框架
## 初始化
```
npm init --yes
```
## 安装Koa
```
cnpm install koa --save
```
## 第一个Koa程序app.js
```
var Koa = require('koa');   //引入Koa
var app = new Koa();        //实例化

//中间件
app.use(async(ctx)=>{
ctx.body='Hello Koa';
});
app.listen(3000);       
```
# ES6基本语法
## let与const
let 块作用域,其他等同于var   
const   定义常量
## 模板字符串
```
var name = '张三';
var age = 20;

console.log(`${name}的年龄是${age}`);
```
## 属性的简写
```
var name = 'zhangsan';
var app={
    name        //等同于name:name
}
console.log(app.name);
```
## 方法的简写
```
var name = 'zhangsan';
var app={
    name,
    run:function(){
        console.log(`${this.name}在跑步`);
    }
}

app.run();
```
## 箭头函数
```
setTimeout(function(){
    console.log('Hello');
},1000)

//箭头函数写法

setTimeout(()=>{
    console.log('Hello');
},1000)

```
## 回调函数 获取异步方法里面的数据
```
function getData(callback){
//ajax 模拟异步
setTimeout(function(){
        var name = 'zhangsan';
        callback(name);
    },1000);
}

//外部获取异步方法里面的数据
getData(function(data){
    console.log(data);
})
```
## Promise处理异步
```
//resolve   成功的回调函数
//reject    失败的回调函数
var p =new Promise(function(resolve,reject){
    //ajax 模拟异步
    setTimeout(function(){
        var name = 'zhangsan';
        resolve(name);
        },1000);
})

p.then(function(data){
    console.log(data);
})
```
# Koa异步处理Async Await和Promise的使用
## Async与Await
async   让方法变为异步方法   
await    等待异步方法执行完成
```
async function getData(){
    return '这是一个数据';
}
//运行结果：Promise {'这是一个数据'} - 返回的是promise对象
```
## 获取async异步方法里面的数据
```
async function getData(){
    return '这是一个数据';
}
var p = getData();

p.then((data)=>{
    console.log(data);
})
```
## await用在异步方法里面
```
async function getData(){
    return '这是一个数据';
}

async function test(){
    var d = await getData();
    console.log(d);
}

//调用
test();
```
## await阻塞功能，把异步改为同步
```
async function getData(){
    console.log(2);
}

async function test(){
    console.log(1);
    var d = await getData();
    console.log(3);
}
test();  //运行结果 1 2 3
```

## Promise的使用及获取Promise的数据
```
function getData(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            var username = 'zhangsan';
            resolve(username);
        },1000)
    })
}

async function test(){
    var data = await getData();
    console.log(data);
}

test();
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

