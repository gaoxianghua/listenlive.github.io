---
title: "php安装Oracle扩展oci8"
date: 2020-10-26T10:30:00+08:00
draft: false
toc: true
url: "/leaf/2020/10/26/php_oci8"
categories: 
- 杂记
tags: 
- PHP
---
## 安装oracle-instantclient
### [下载地址](http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html)   
### 分别下载 
**oracle-instantclient11.2-basic-11.2.0.4.0-1.x86_64.rpm**   
**oracle-instantclient11.2-devel-11.2.0.4.0-1.x86_64.rpm**   
### 运行 rpm -ivh oracle-instantclient11.2-*   
![](/images/leaf/202010/202010261044.png)   
***此时会生成/usr/lib/oracle/11/2/client64/lib 目录***
### 修改 /etc/ld.so.conf 配置文件
追加以下内容   
**/usr/lib/oracle/11.2/client64/lib/**   
**保存并退出**   
**执行命令 ldconfig**
## 安装 oci8
### 下载oci8组件
### [下载oci-2.0.8.tgz](http://pecl.php.net/package/oci8)  
### 解压并安装
**tar -xvzf oci-2.0.8.tag**   
**cd oci-2.0.8**  
```
/www/server/php/56/bin/phpize （用phpize生成configure配置文件)

./configure --with-php-config=/www/server/php/56/bin/php-config --with-oci8=shared,instantclient,/usr/lib/oracle/11.2/client64/lib
``` 
**make && make install**
## 修改 php.ini
### 添加extension="oci8.so"   
![](/images/leaf/202010/202010261104.png)   
## 重启PHP，查看phpinfo
![](/images/leaf/202010/202010261110.png)   
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

