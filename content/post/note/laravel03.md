---
title: "Laravel基础03-中间件的使用"
date: 2020-07-22T17:06:05+08:00
draft: false
url: "/note/2020/07/22/laravel03"
categories: 
- 笔记
tags: 
- Laravel
- PHP
- PHPstorm
---
# 使用中间件进行后台权限验证
## 创建中间件
```angularjs
php artisan make:middleware AdminMiddleware
```
执行后会在app/Http/Middleware目录下生成**AdminMiddleware.php**文件   
## 在AdminMiddleware类handle方法中使用Auth验证
```angularjs
use Auth;
```
```angularjs
public function handle($request, Closure $next)
 {
        if(!Auth::guard('admin')->check()){
            return redirect('/admin/login');
        }
        return $next($request);
 }
```
## 路由中挂载此中间件
_配置app/Http/Kernel.php_
```angularjs
protected $routeMiddleware = [
        'admin.auth' => AdminMiddleware::class,      //在路由中挂载登录验证中间件
        'auth' => \App\Http\Middleware\Authenticate::class,
        'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
        'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
        'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
        'can' => \Illuminate\Auth\Middleware\Authorize::class,
        'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
        'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
        'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
        'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
    ];
```
## 在控制器中使用登录验证中间件
```angularjs
public function __construct()
 {
        //登录验证中间件
        $this->middleware('admin.auth')->except(['loginForm','login']);
 }
```
_except() 排除loginForm、login方法_   
# Auth&guard完成退出功能
## 退出方法
```angularjs
 public function logout()
 {
    Auth::guard('admin')->logout();
    return redirect('/admin/login');
 }
```

___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |


