---
title: "Ubuntu-安装PHP启动报错php: error while loading shared libraries: libcrypto.so.1.0.0: cannot open shared object file: No such file or directory解决办法"
date: 2021-05-08T10:36:36+08:00
draft: false
toc: true
url: "/note/202105/phperror.html"
categories: 
- 笔记
tags: 
- PHP
---
# Ubuntu-使用宝塔面板安装PHP后启动报错：php: error while loading shared libraries: libcrypto.so.1.0.0: cannot open shared object file: No such file or directory  
![](/images/note/202105/20210507phperror.png)  
# 解决办法  
## 前往http://security.ubuntu.com/ubuntu/pool/main/o/openssl1.0/找到合适的软件包  
![](/images/note/202105/202105081045.png)  
我这里选择`libssl1.0-dev_1.0.2n-1ubuntu5_amd64.deb`  
```
	cd /usr/local/src 
	sudo wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl1.0/libssl1.0-dev_1.0.2n-1ubuntu5_amd64.deb
	sudo dpkg -i libssl1.0-dev_1.0.2n-1ubuntu5_amd64.deb
```
## 再次重启PHP成功  
![](/images/note/202105/202105071714.png)  
  




___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

