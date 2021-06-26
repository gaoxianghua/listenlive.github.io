---
title: "使用阿里云/腾讯云国外服务器+Shadowsocks实现科学上网"
date: 2021-01-22T14:06:41+08:00
draft: true
toc: true
url: "/leaf/20210122/ssr.html"
categories: 
- 技术
tags: 
- Shadowsocks
- VPN
- 科学上网
---
# 购买阿里云/腾讯云国外服务器
## 根据自己情况可以在阿里云或腾讯云购买合适的云服务器，可选择包年、包月或按流量付费  
![Aliyun ECS选择界面](/images/leaf/202101/aliyunECS.png)  
## 设置安全组 开放`18001`端口
![设置ECS界面](/images/leaf/202101/ecs-setting.png)
## 手动添加 开放`18001`端口  
![](/images/leaf/202101/port-setting.png)
# 配置服务器文件  
## 使用WinSCP等FTP工具登录刚创建的实例,进入/opt目录，创建如下几个文件  
![](/images/leaf/202101/img-opt.png)
## 启动文件 `ali_start.sh`  
```shell
yum install python-setuptools
easy_install pip
pip install shadowsocks

nohup ssserver -c shadowsocks.json -d start &

```
## Shadowsocks配置文件 `shadowsocks.json`  
```json
{
    "server":"0.0.0.0",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "port_password":{
	"18001":"your-password"      //ssr连接密码
	},
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```
## 停止文件 `stop.sh`  
```shell
ssserver -c shadowsocks.json -d stop
```
## 使用`sh`命令运行ali_start.sh启动成功即可  
```shell
sh /opt/ali_start.sh 
```
# 配置Shadowsocks并连接
## 在[Github](https://github.com/shadowsocks/shadowsocks-windows/releases)等搜索下载shadowsocks
## 进行如下配置，完成即可！  
![](/images/leaf/202101/ssr-setting.png)




___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

