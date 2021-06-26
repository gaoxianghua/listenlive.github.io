---
title: "基于Koa框架的Lincms常用操作"
date: 2021-03-03T14:00:49+08:00
draft: false
toc: true
url: "/note/202103/lincms.html"
categories: 
- 笔记
tags: 
- lincms
- Sequelize
- koa
- node.js
---
# Sequelize事务操作  
```javascript
let transaction;        //事务开始
try {
    transaction = await sequelize.transaction();
    const replace_record = {
    p_code: v.get('path.p_code'),
    p_type: v.get('path.p_type')
    };
    const { id: record_id } = await ReplaceRecordModel.create(replace_record, {
    transaction
    });
    await WxReplacesheetsModel.create(
    {
        record_id,
        name: v.get('path.name'),
        phone: v.get('path.phone'),
        province: v.get('path.province'),
        city: v.get('path.city'),
        district: v.get('path.district'),
        question: v.get('path.question'),
        status: 1
    },
    {
        transaction
    }
    );
    await transaction.commit();     //事务提交
    ctx.json({
        code:11,
        message:"创建成功"
    });
}catch (error) {
    console.log(error)
    await transaction.rollback();   //事务回滚
    throw new Failed({
    code:22,
    message:"创建失败",
    });
}
```
# 更新`update`  
```javascript
    await ReplaceRecordModel.update({status:2},{
        where:{
            id:1
        }
    });
```
# koa读取Excel文件  
```javascript
const xlsx = require('xlsx');
const fs = require('fs');
const path = require('path');
 
async function getExcelObjs (arr) {
 const file = 'app/assets/'+arr[0].path;    // 获取上传的excel文件路径
 const reader = fs.createReadStream(file);  // 创建可读流
 const upStream = fs.createWriteStream(file);   // 创建可写流
 const getRes = await getFile(reader, upStream);    //等待数据存储完成
 const datas = [];  //可能存在多个sheet的情况
 if (!getRes) { 
    const workbook = xlsx.readFile(file);
    const sheetNames = workbook.SheetNames; // 返回 ['sheet1', ...]
    for (const sheetName of sheetNames) {
    const worksheet = workbook.Sheets[sheetName];
    const data = xlsx.utils.sheet_to_json(worksheet);
    datas.push(data);
    }
    //删除文件
    fs.unlink(file,function(error){
        if(error){
            console.log(error);
            return false;
        }
    });
    return {
    status: true,
    datas
    };
 } else {
  return {
   status: false,
   msg: 'error message...'
  };
 }
}
 
function getFile (reader, upStream) {
  return new Promise(function (result) {
   let stream = reader.pipe(upStream); // 可读流通过管道写入可写流
   stream.on('finish', function (err) {
     result(err);
   });
  });
 }
module.exports = {
 getExcelObjs
};
```


___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

