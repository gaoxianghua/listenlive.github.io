---
title: "使用Linux定时任务-Crontab 完成MySQL数据库的备份和表操作"
date: 2021-01-19T17:39:31+08:00
draft: false
toc: true
url: "/leaf/202101/crontab_linux.html"
categories: 
- 技术
tags: 
- 定时任务
- crontab
---
# crontab基本命令  
```shell script
crontab -l  //查看定时任务
crontab -e  //编辑定时任务
crontab -r  //删除定时任务
```
# crontab常用写法  
```shell script
每分钟执行             */1 * * * *     command
每小时0分执行           0 * * * *       command
每天0点执行             0 0 * * *       command
每周日0点执行           0 0 * * 0       command
每月1号执行             0 0 1 * *       command
每年1月1日执行          0 0 1 1 *       command
```
# 使用crontab定时任务备份数据库 
## 首先编写shell脚本 `back_mysql.sh`   
```shell script
t = `date '+%Y-%m-%d %H:%M:%S'`   //使用时间做文件名
mysqldump -h127.0.0.1 -uusername -ppwd dbname > /var/back_mysql/dbname"$t".sql
```
## 将shell脚本加入到定时任务  
```shell script
00 6 * * *  /opt/back_mysql.sh    //每天六点备份数据库
```  
# 使用定时任务定期更新数据库表字段  
```shell script
//每年1月1日更新年龄字段，让 age + 1
0 0 1 1  * mysql -uusername -ppwd -e "use dbname;UPDATE tablename SET age = age + 1;quit;"
```


___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

