---
title: "Laravel基础04-视图"
date: 2020-07-23T10:35:48+08:00
draft: false
url: "/note/2020/07/23/laravel04"
categories: 
- 笔记
tags: 
- Laravel
- PHPstorm
- PHP
---
# Blade模板引擎的模板继承构建后台界面
## 后台视图文件目录
-/resources/views/admin/layout 为父级模板目录   
![父级模板目录](/images/202007231042.png)   
## 使用@yield占位定义活动的页面范围
```angularjs
<div>
    @yield('content')
</div>
```
## 子页面继承父级模板   
```angularjs
@extends('admin.layout.master')
@section('content')
    <div class="panel panel-default">
        <div class="panel-heading">
            <h3 class="panel-title">后台管理</h3>
        </div>
        <div class="panel-body">
            后台管理系统
        </div>
    </div>
@endsection
```
# 使用request进行表单验证
## 创建request
```angularjs
php artisan make:request AdminPost
```
_执行后会在app/Http/Requests目录生成_***AdminPost.php***_文件_   
## 首先引入登录验证的中间件，即在管理员登录状态才进行验证   
```angularjs
use Auth;
use Validator;
use Hash;

public function authorize()
 {
     return Auth::guard('admin')->check();
 }
 /*
  *添加验证规则--用于确认密码比对
  */
public function addValidator()
 {
   Validator::extend('check_password',function($attribute,$value,$parameters,$validator){
       return Hash::check($value,Auth::guard('admin')->user()->password); //验证密码
   });
 }
```
_使用_[***Validator***](https://learnku.com/docs/laravel/5.8/validation/3899)_验证器_
## 设置验证规则
```angularjs
public function rules()
 {
    $this->addValidator();
    return [
        'password' => 'sometimes|required|confirmed',
        'password_confirmation' => 'sometimes|required',
        'original_password' => 'sometimes|required|check_password',
    ];
    /*sometimes表示有表单的时候才会验证*/
 }
```
## 定义错误提示信息
```angularjs
public function message()
 {
       return [
           'password.required' => '密码不能为空',
           'password_confirmation.required' => '请输入确认密码',
           'password_confirmed' => '两次密码输入不一致',
           'original_password.required' => '请输入初始密码',
           'original_password.check_password' => '初始密码错误',
       ];
     }   
```
## 在视图中显示错误提示信息
_定义错误提示模板errors.blade.php_   
```angularjs
@if (count($errors) > 0)
    <div class="modal fade" id="modal_message">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-hidden="true">
                        &times;
                    </button>
                    <h4 class="modal-title">后台 - 友情提示</h4>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-sm-2">
                            <i class="fa fa-info-circle fa-4x"></i>
                        </div>
                        <div class="col-sm-9">
                            @foreach ($errors->all() as $error)
                                {{ $error }}<br>
                            @endforeach
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">关闭</button>
                </div>
            </div>
        </div>
    </div>
    <script>
        require(['bootstrap'], function ($) {
            $('#modal_message').modal('show');
            setTimeout(function () {
                $('#modal_message').modal('hide');
            }, 3000);
        })
    </script>
@endif
```
## 在控制器中引用并使用request验证
```angularjs
use App\Http\Requests\AdminPost;
use Auth;
public function changePassword(AdminPost $request)
 {
    $model = Auth::guard('admin')->user();
    $model->password = bcrypt($request['password']);    //获取密码加密
    $model->save(); //保存更新密码
 }
```
# 使用laracasts-flash在视图中展示错误提示信息
## 安装[laracasts-flash](https://github.com/laracasts/flash)
```angularjs
composer require laracasts/flash
```
## 将此工具在config/app.php服务提供者(providers)里面注册
```angularjs
Laracasts\Flash\FlashServiceProvider::class,
```
## 在父级模板中（master.blade.php）引用
```angularjs
@include('flash::message')
<script>
    require(['bootstrap'], function ($) {
    //使用bootstrap模态块
        $('#flash-overlay-modal').modal();
    });
</script>
```
## 安装flash扩展支持
```angularjs
php artisan vendor:publish --provider="Laracasts\Flash\FlashServiceProvider"
```
执行后会在resources/views/vendor目录下生成flsh支持目录 
## 在控制器中使用
```angularjs
public function changePassword(AdminPost $request)
  {
       $model = Auth::guard('admin')->user();
       $model->password = bcrypt($request['password']);    //获取密码加密          
       $model->save(); //保存更新密码          
       flash('密码修改成功')->overlay();          
       return redirect()->back();             
  }  
```  
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

