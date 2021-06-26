---
title: "pip install 出错"
date: 2021-02-08T15:28:06+08:00
draft: false
toc: true
url: "/leaf/202102/pip.html"
categories: 
- 技术
tags: 
- pip
---
# Linux下使用pip命令报错  
## 最近使用腾讯云的实例遇到的坑，运行`pip install`命令后，报错如下：  
```shell script
Traceback (most recent call last):
  File "/usr/bin/pip", line 9, in <module>
    load_entry_point('pip==21.0', 'console_scripts', 'pip')()
  File "/usr/lib/python2.7/site-packages/pkg_resources.py", line 378, in load_entry_point
    return get_distribution(dist).load_entry_point(group, name)
  File "/usr/lib/python2.7/site-packages/pkg_resources.py", line 2566, in load_entry_point
    return ep.load()
  File "/usr/lib/python2.7/site-packages/pkg_resources.py", line 2260, in load
    entry = __import__(self.module_name, globals(),globals(), ['__name__'])
  File "/usr/lib/python2.7/site-packages/pip/_internal/cli/main.py", line 60
    sys.stderr.write(f"ERROR: {exc}")
```
# 解决方式
## 重新安装pip
```shell script
yum remove python-pip

cd /usr/local/src

wget https://bootstrap.pypa.io/2.7/get-pip.py

python get-pip.py
 
pip -V
```


___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

