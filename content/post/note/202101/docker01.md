---
title: "Linux-CentOS上Docker的安装与卸载"
date: 2021-01-06T14:42:00+08:00
draft: false
toc: true
url: "/note/202101/install_docker"
categories: 
- 笔记
tags: 
- CentOS
- Docker
---
### 通过uname -r 命令查看Linux内核版本  
```
    uname -r //Docker 要求 CentOS 系统的内核版本高于 3.10
```
### 确保yum包更新到最新  
```
    yum update
```
### 安装需要的软件包  
```
    yum install -y yum-utils device-mapper-persistent-data lvm2     //yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
```
### 设置yum源  
```
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
### 查看所有仓库中所有docker版本，并选择特定版本安装  
```
    yum list docker-ce --showduplicates | sort -r
```
![](/images/note/202101/202101061502.png) 
### 选择版本并安装  
```
    sudo yum install docker-ce-19.03.9.ce
```
![](/images/note/202101/202101061508.png) 
### 启动并加入开机启动  
```
    systemctl start docker
    systemctl enable docker
```
### 运行docker version命令，查看版本信息  
```
    docker version
```
![](/images/note/202101/202101061512.png) 
### 卸载命令
```
    yum remove docker  docker-common docker-selinux docker-engine
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |
