---
title: "Linux-使用防火墙firewalld开放端口"
date: 2021-01-22T14:58:14+08:00
draft: false
toc: true
url: "/leaf/20210122/linux-firewalld.html"
categories: 
- 技术
tags:
- firewalld
- 防火墙
---
# 查看当前开了哪些端口其实一个服务对应一个端口，每个服务对应/usr/lib/firewalld/services下面一个xml文件  
```shell
firewall-cmd --list-services
```
# 通过`firewall-cmd --get-services`命令查看可以打开的服务有哪些  
# 新建并编辑`vpn-port.xml`文件 开放`18001`端口  
```xml
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>VPN Port</short>
  <description>VPN Port 18001</description>
  <port protocol="tcp" port="18001"/>
</service>
```
# 可以通过下面的命令添加一个服务到firewalld  
```shell
firewall-cmd --add-service=vpn-port //vpn-port换成想要开放的service,仅当前有效

firewall-cmd --permanent --add-service=vpn-port   //加上--permanent,表示系统重启后也有效
```
# 重启防火墙即可生效
```shell
systemctl restart firewalld.service
```



___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

