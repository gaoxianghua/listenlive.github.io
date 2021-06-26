---
title: "Koa框架实现导出Excel文件"
date: 2020-12-11T17:29:57+08:00
draft: false
toc: true
url: "/note/2020/12/11/koa_import"
categories: 
- 笔记
tags: 
- Koa2
- Node.js
---
## 安装引入excel-export包
```
    cnpm install excel-export --save
```
```
    const nodeExcel = require('excel-export');
```
## 实现导出方法  
```
/**
   * 数据导出--导出方法
   * @param resultData [{},{},] //从数据库中获取的数据格式
   */
  static async exportData(resultData,ctx){
    let conf ={};
        conf.name = "worksheet";//表格名
        let alldata = new Array();
        for(let i = 0;i<resultData.length;i++){
            let arr = new Array();
            arr.push(resultData[i].name);
            arr.push(resultData[i].phone);
            arr.push(resultData[i].age);
            alldata.push(arr);
        }
        //决定列名和类型
        conf.cols = [{
            caption:'姓名',
            type:'string'
        },{
            caption:'手机号',
            type:'string'
        },{
            caption:'年龄',
            type:'string'
        }];
        conf.rows = alldata;//填充数据
        let result = nodeExcel.execute(conf);
        //最后3行express框架是这样写
        // res.setHeader('Content-Type', 'application/vnd.openxmlformats');
        // res.setHeader("Content-Disposition", "attachment; filename=" + "Report.xlsx");
        // res.end(result, 'binary');
        ctx.set('Content-Type', 'application/vnd.openxmlformats');
        ctx.set("Content-Disposition", "attachment; filename=" + "Report.xlsx");
        ctx.body=data;
  }
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

