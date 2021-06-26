---
title: "Sequelize操作MySQL数据库"
date: 2020-08-20T14:00:54+08:00
draft: false
toc: true
url: "/note/2020/08/20/sequelize01"
categories: 
- 笔记
tags: 
- Node.js
- Sequelize
---
## 安装sequelize、sequelize-cli和mysql2
```
cnpm install sequelize-cli -g

cnpm install sequelize --save

cnpm install mysql2 --save
```
## 初始化项目
```
sequelize init
```
## 在config.js中配置连接数据库参数
```
//连接数据库
{
  "development": {
    "username": "root",
    "password": null,
    "database": "bilikoa",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "test": {
    "username": "root",
    "password": null,
    "database": "database_test",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "production": {
    "username": "root",
    "password": null,
    "database": "database_production",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}

```
## 建立Article模型和迁移文件
```
sequelize model:generate --name Article --attributes title:string,content:text
```
## 运行迁移，生成数据表
```
sequelize db:migrate
```
## 新建种子文件
```
sequelize seed:generate --name atricle
```
## 配置种子文件seeders/article.js
```
'use strict';

module.exports = {
  up: async (queryInterface, Sequelize) => {
   
      await queryInterface.bulkInsert('Articles', [
        {
        title: 'John Doe',
        content: '内容。。',
        createdAt: new Date(),
        updatedAt: new Date()
      },
      {
        title: 'John Does',
        content: '内容。。',
        createdAt: new Date(),
        updatedAt: new Date()
      }
    ], {});
    
  },

  down: async (queryInterface, Sequelize) => {
   
      await queryInterface.bulkDelete('People', null, {});
     
  }
};

```
## 运行种子文件，即可在articles表中生成测试数据
```
sequelize db:seed:all
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |
