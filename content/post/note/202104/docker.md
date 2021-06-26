---
title: "Ubuntu-Docker的安装与使用"
date: 2021-04-29T12:34:31+08:00
draft: false
toc: true
url: "/note/202104/docker.html"
categories: 
- 笔记
tags: 
- Docker
- Ubuntu
---
# 环境配置  
## 删除旧版本  
```
    sudo apt-get remove docker docker-engine docker.io containerd runc
```
## 更新apt包索引  
```
    sudo apt-get update
```
## 安装包以允许apt通过HTTPS使用存储库  
```
    sudo apt-get install apt-transport-https
    sudo apt-get install ca-certificates
    sudo apt-get install curl
    sudo apt-get install gnupg-agent
    sudo apt-get install software-properties-common
```
## 添加Docker的官方GPG密钥  
```
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
`9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`通过搜索指纹的最后8个字符，验证您现在拥有带指纹的密钥  
```
    sudo apt-key fingerprint 0EBFCD88
```
## 添加软件源  
```
    sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```   
## 更新apt索引  
```
    sudo apt-get update
```
# 执行安装  
```
    sudo apt-get install docker-ce docker-ce-cli containerd.io
```
## 通过运行hello-world 映像验证是否正确安装了Docker CE  
```
    sudo docker run hello-world
```
## 查看docker版本  
```
    sudo docker version
```
# 进程维护  
## 停止、启动、重启docker
```
    sudo systemctl start | stop | restart docker.service
```
## 加入开机自启  
```
    sudo systemctl enable docker
```
## 开机启动检测  
```
    sudo systemctl list-unit-files | grep docker
```
# Docker的升级维护  
## 升级  
```
    sudo apt-get update
```
## 卸载  
```
    sudo apt-get purge docker-ce docker-ce-cli containerd.io docker docker.io
    sudo rm -rf /var/lib/docker
    sudo apt autoremove
```    
# 账号权限  
由于每次运行docker都要在命令前加sudo,下面设置当前登录账号执行docker命令方法  
## 创建docker组  
```
    sudo groupadd docker
```
## 将当前用户添加到docker组
```
    sudo usermod -aG docker $USER
```
## 注销账号并重新登录即可  
# 镜像管理  
## 搜索镜像  
```
    docker search nginx
```
## 安装镜像  
```
    docker pull nginx
```
## 查看镜像  
```
    docker images
```
## 删除镜像  
```
    docker rmi -f nginx
```        
# 容器管理  
## 查看运行的容器  
```
    docker ps
```
## 查看所有容器  
```
    docker ps -a
```
## 查看容器进程  
```
    docker top nginx
```
## 查看容器端口映射  
```
    docker port xx
```
## 查看容器元信息（如IP）  
```
    docker inspect xx
```
## 查看容器日志  
```
    docker logs nginx
```
## 删除容器
```
    docker rm -f xx
```

___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

