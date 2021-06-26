---
title: "Laravel&QQ邮箱实现邮件发送"
date: 2020-08-04T11:16:11+08:00
draft: false
url: "/note/2020/08/04/laravel08"
categories: 
- 笔记
tags: 
- Laravel
- PHP
---
# 配置.env文件中的邮件部分
```
MAIL_DRIVER=smtp
MAIL_HOST=smtp.qq.com
MAIL_PORT=25
MAIL_USERNAME=1099764281@qq.com
MAIL_PASSWORD=tmdyxmsfwtqzgbje  //QQ邮箱授权码
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=1099764281@qq.com
MAIL_FROM_NAME=1099764281
```
# 新建邮件发送策略
```
php artisan make:mail RegMail
```
# 邮件发送方法
```
use App\Mail\RegMail;

public function sendMail()
{
   $user = User::find(1);
   \Mail::to($user)->send(new RegMail());
   return redirect('/');
}    
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |


