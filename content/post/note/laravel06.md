---
title: "Laravel Api开发之laravel/passport授权包的使用及基本配置"
date: 2020-07-31T17:18:54+08:00
draft: false
url: "/note/2020/07/31/laravel06"
categories: 
- 笔记
tags: 
- PHP
- Laravel
- Api
---
# 安装laravel/passport
```
composer require laravel/passport=~7.0
```
_指定为7.0版本，laravel5.8不支持8.0及以上版本_   
# 创建所需的表
```
php artisan migrate
```
# 生成secret
```
php artisan passport:install
```
# 在app/Providers/AuthServiceProvider.php中引入并在boot方法中调用
```
use Laravel\Passport\Passport;

public function boot()
    {
        $this->registerPolicies();
        Passport::routes();
    }
```
# 配置config/auth.php,将api默认的token改为passport
```
'api' => [
            'driver' => 'passport',
            'provider' => 'users',
            'hash' => false,
        ],
```
# 将HasApiTokens Trait引入app/User.php中,这个 Trait 为模型提供一些辅助函数，用于检查已认证用户的令牌和使用范围
```
namespace App;

use Laravel\Passport\HasApiTokens;
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use HasApiTokens, Notifiable;
}
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |