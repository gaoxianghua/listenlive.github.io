---
title: "Laravel基础02-后台登录验证"
date: 2020-07-21T13:29:04+08:00
draft: false
url: "/note/2020/07/21/laravel02"
categories: 
- 笔记
tags: 
- Laravel
- PHP
- PHPstorm
---
# 创建后台控制器
```angularjs
php artisan make:controller Admin/EntryController
```
## 配置路由
```angularjs
Route::get('/login','Admin\EntryController@loginForm');
```
或者采用配置命名空间的方式指定到Admin目录，简化一些重复代码   
```angularjs
Route::group(['prefix' => 'admin','namespace' => 'Admin'],function(){
   Route::get('/login','EntryController@loginForm');
});
```
# 使用Auth登录验证
## 编辑config/auth.php文件，在guards里定义admin入口   
```angularjs
 'admin' => [
            'driver' => 'session',
            'provider' => 'admins',
        ],
```
## 在providers里添加admins,并指定model   
```angularjs
'admins' => [
            'driver' => 'eloquent',
            'model' => App\Model\Admin::class,
        ],
```
## 在控制器里面引入Auth   
```angularjs
use Auth;
```
## 添加登录验证方法login并验证   
```angularjs
public function login()
    {
        $status = Auth::guard('admin')->attempt([
           'username' => Request::input('username'),
           'password' => Request::input('password'),
        ]);
        dd($status);
    }
```
_laravel在表单提交数据的时候要在form中使用csrf验证_
```angularjs
{{ csrf_field() }}
```
## 验证登录
```angularjs
public function login()
    {
        $status = Auth::guard('admin')->attempt([
           'username' => Request::input('username'),
           'password' => Request::input('password'),
        ]);
        if($status){
            return redirect('/admin/index');
        }
        return redirect('/admin/login')->with('error','用户名或密码错误');
    }
```
## 使用with闪存(只存储一次)向视图传递错误信息   
```angularjs
@if(session('error'))
   <div class="alert alert-danger">
         {{session('error')}}
   </div>
@endif
```
![](/images/202007221645.png)

___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |