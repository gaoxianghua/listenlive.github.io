---
title: "使用Laravel邮件发送实现用户的验证&注册业务"
date: 2020-08-04T17:21:13+08:00
draft: false
url: "/note/2020/08/04/laravel09"
categories: 
- 笔记
tags: 
- PHP
- Laravel
---
# 注册数据处理方法中添加token并触发邮件发送
```
//用户注册数据处理
public function store(Request $request)
{
    $data = $this->validate($request,[   
       'name'  =>  'required|min:4',
       'email'  =>  'required|email|unique:users',
       'password'  => 'required|min:6|confirmed',
    ]);
    $data['password'] = bcrypt($data['password']);
    $data['email_token'] = Str::random(10);
    $user = User::create($data);
    //发送注册邮件
    \Mail::to($user)->send(new RegMail($user));
    session()->flash('success','请查看邮箱完成注册验证');
    return redirect('/');
}
```
# app/Mail/RegMail.php
```
namespace App\Mail;
use Illuminate\Bus\Queueable;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;
use Illuminate\Contracts\Queue\ShouldQueue;
class RegMail extends Mailable
{
    use Queueable, SerializesModels;
    /**
     * Create a new message instance.
     *
     * @return void
     */
    public $user;   //定义公共属性会自动分配到视图模板
    public function __construct($user)
    {
        //
        $this->user = $user;
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->view('mail.reg');
    }
}
```
# 用户邮件验证视图reg.blade.php
```
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>点击链接验证邮箱</title>
</head>
<body>
<div>
    <a href="http://blog01.com/confirmEmailToken/{{$user['email_token']}}">点击链接完成验证</a>
</div>
</body>
</html>
```
# 点击邮件中的链接验证
```
//用户验证邮件路由
Route::get('confirmEmailToken/{token}','UserController@confirmEmailToken')->name('confirmEmailToken');
```
# 邮件验证方法
```
    public function confirmEmailToken($token)
    {
        $user = User::where('email_token',$token)->first();
        if($user){
            $user ->email_active = true;
            $user ->save();
            session()->flash('success','验证成功');
            \Auth::login($user);
            return redirect('/');
        }
        session()->flash('danger','邮箱验证失败');
        return redirect('/');
    }
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |


