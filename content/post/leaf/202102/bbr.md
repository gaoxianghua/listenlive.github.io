---
title: "Linux开启bbr加速"
date: 2021-02-09T13:09:10+08:00
draft: false
toc: true
url: "/leaf/202102/bbr.html"
categories: 
- 技术
tags: 
- Linux
- bbr
---
# 安装bbr加速  
## 进入`/usr/local/src`目录，下载`bbr.sh`文件  
```shell script
cd /usr/local/src

sudo wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh

```
![](/images/leaf/202102/bbr01.png)  
## 添加可执行权限   
```shell script
sudo chmod +x bbr.sh

```
![](/images/leaf/202102/bbr02.png)  
## 执行该脚本文件,安装完成后提示按`y`重启系统    
```shell script
sudo sh bbr.sh
```   
![](/images/leaf/202102/bbr03.png)  
## 判断BBR加速有没有开启成功。输入以下命令：  
```shell script
sysctl net.ipv4.tcp_available_congestion_control
```
## 如果返回值为：`net.ipv4.tcp_available_congestion_control = bbr cubic reno`则说明已经开启成功了  
![](/images/leaf/202102/bbr04.png)  
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

