---
title: "Git 版 本 控 制"
date: 2020-08-07T11:44:45+08:00
draft: false
url: "/leaf/2020/08/07/gitcommand"
categories: 
- 杂记
tags: 
- Git
- Command
---
# Git基本命令
git config -l                   查看配置   
git config -global user.name "username" 全局设置用户名   
git config -global user.email "email" 全局设置用户邮箱   
git init                        初始化版本库   
git add a.php                   添加文件到版本库   
git add .                       添加所有文件到版本库   
git status                      查看状态   
git commit -m 'message'         提交到版本库并添加提交信息   
git rm -\-cached readme.txt     移除版本库中的文件   
git log                         查看日志   
git commit -\-amend             修改最近一次提交的信息   
git reset HEAD a.php            撤销添加   
git checkout -\- a.php          撤销文件修改   
   
git clone -b 分支名   仓库地址     使用Git下载指定分支
git branch                      查看分支   
git branch ask                  创建分支   
git checkout ask                切换分支
git checkout -b bbs             创建并切换到bbs分支 
git merge ask                   合并分支   
git branch -d ask               删除分支   
git branch -\-no-merge          查看未合并的分支   
git branch -D test              删除未合并的分支   
   
git stash                       暂存   
git stash list                  暂存列表   
git stash apply                 恢复暂存   

git archive master -\-prefix='bbs/' -\-forma=zip > bbs.zip 生成zip发布压缩包   
git remote add origin https://github.com/gaoxianghua/listenlive.cn.git  本地与GitHub远程关联   
git push -u origin master   推送到远程master分支   
git pull origin ask:ask     将远程ask分支拉到本地


# .gitignore 忽略文件
a.php                           忽略提交此文件   
vendor                          忽略提交此文件夹   
vendor/\*.txt                   忽略提交此文件目录下的TXT文件   
!a.php                          不忽略此文件    
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |


