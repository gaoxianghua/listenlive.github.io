---
title: "解决Hugo/Hexo主题目录themes/无法通过Git提交问题"
date: 2021-01-05T13:26:51+08:00
draft: false
toc: true
url: "/leaf/2021/01/git_commit"
categories: 
- 杂记
tags: 
- Git
- Hugo
---
### 剪切themes/.git文件夹到其他目录  
### 从暂存区删除该文件夹  
```
    git rm --cache ./themes/next
```
### 再重新添加并提交即可  
```
    git add .
    git commit -m ''
    git push
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

