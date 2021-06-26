---
title: "MongoDB 安装配置"
date: 2020-07-30T16:57:57+08:00
draft: false
url: "/note/2020/07/30/mongodb01"
categories: 
- 笔记
tags: 
- MongoDB
- nosql
- 数据库
---
# 安装配置（with WAMP）
## 下载[MongoDB](https://www.mongodb.com/try/download/community)，并将安装包移动到wamp环境目录下
![](/images/note/202007311027.png)
## 在mongodb目录新建log.txt文件和dat文件目录
## 启动命令
```
mongod --port 8888 --dbpath d:/wamp64/bin/mongodb/data --logpath d:/wamp64/bin/mongodb/log.txt
```
查看是否启动成功
```
netstat -an
```
![](/images/note/202007311035.png)
## 连接MongoDB数据库
```
mongo localhost:8888
```
![](/images/note/202007311040.png)
# MongoDB基本指令
```
use dbname                          //使用数据库，如果没有则创建
show dbs                            //查看数据库
db.集合名.insert({})                 //插入集合
show tables                         //查看集合
db.集合名.find()                     //查询集合里面的所有文档
db.集合名.findOne()                  //查询集合里面的第一个文档
db.集合名.drop()                     //删除集合
db.dropDatabase()                   //删除数据库（当前use的数据库）
db.集合名.remove({age:5})            //删除文档
db.集合名.update({条件}),{新文档}      //更新文档
db.集合名.find({条件})                //查询文档
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |
