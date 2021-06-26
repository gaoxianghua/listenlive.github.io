---
title: "Windows-Laravel开发环境Homestead搭建"
date: 2021-05-18T15:20:48+08:00
draft: false
toc: true
url: "/note/202105/installhd.html"
categories: 
- 笔记
tags: 
- Laravel
- Homestead
---
# 准备工作  
## 安装所需软件
+ Git  
+ Vagrant  
+ VitrualBox  
+ xshell  
+ Navicat  
# 生成SSH Key  
在用户`家`目录运行：    
```shell
 cd ~
 ssh-keygen -t rsa -C "youemail@homestead.com"
```
![](/images/note/202105/ssh.png)  
此时，会生成以下的文件  
![](/images/note/202105/ssh-img.png)  
# 下载Homestead的box文件
## 打开命令终端以管理员权限运行，cd到用户`家`目录  
![](/images/note/202105/command.png)  
## 运行命令：  
```shell
vagrant box add laravel/homestead
```
## 输入编号，选择vitrualbox选项  
## 等待下载安装即可（现在国内的下载速度还是可以的，所以不用去找box文件啦）  
## 完成后运行`vagrant box list`命令查看是否安装成功  
# 配置Homestead  
## 克隆homestead配置文件到用户`家`目录   
```shell
cd ~ 
git clone https://github.com/laravel/homestead.git
```
## 执行命令`bash init.sh`初始化  
```shell
cd homestead
bash init.sh
```
## 配置`Homestead.yaml`文件  
```shell
---
ip: "192.168.1.10"
memory: 2048
cpus: 2
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: D:\workspace           /**自定义自己的工作目录(注释删除)**/
      to: /home/vagrant/code

sites:
    - map: homestead.test
      to: /home/vagrant/code/homestead/public
    - map: api.test
      to: /home/vagrant/code/shopApi/public
    - map: tp5.test
      to: /home/vagrant/code/tp5/public
      php: "7.2"                /**指定php版本(注释内容删除)**/
      type: thinkphp            /**TP框架需指定thinkphp类型(为了支持pathinfo)(注释内容删除)**/

databases:
    - homestead
    - api

features:
    - mysql: true
    - mariadb: false
    - postgresql: false
    - ohmyzsh: false
    - webdriver: false

#services:
#    - enabled:
#        - "postgresql@12-main"
#    - disabled:
#        - "postgresql@11-main"

# ports:
#     - send: 50000
#       to: 5000
#     - send: 7777
#       to: 777
#       protocol: udp
```
# 运行`vagrant up`命令，启动Homestead    
```shell
    vagrant up
```
## xshell连接虚拟机的IP为yaml配置文件中的IP，用户名和密码均为vagrant  
![](/images/note/202105/xshell_link.png)  
## Navicat连接MySQL数据库的IP为yaml配置文件中的IP，用户名为homestead，密码为secret  
# 修改Homestead中PHP的版本
```shell
# 查看所有 php 版本和当前版本
sudo update-alternatives --display php 
# 执行后，会列出当前 php 所有版本和编号，输入编号，切换到执行的版本
sudo update-alternatives --config php 
```
![](/images/note/202105/installhd.png)  

___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

