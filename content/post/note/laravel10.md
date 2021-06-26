---
title: "Laravel Api开发之Dingo&Jwt的使用"
date: 2020-09-22T15:02:17+08:00
draft: false
toc: true
url: "/note/2020/09/22/laravel_dingo"
categories: 
- 笔记
tags: 
- PHP
- Laravel
- Api
---
## Dingo
### 安装组件
```
composer require dingo/api:2.x
```
### 生成配置文件
```
php artisan vendor:publish
```
***执行命令后会生成config/api.php配置文件***   
### config/api.php 配置说明
```
#接口围绕：[x]本地和私有环境 [prs]公司内部app使用 [vnd]公开接口
'standardsTree' => env('API_STANDARDS_TREE', 'x')

#项目名称
'subtype' => env('API_SUBTYPE', 'hdcms')

#Api前缀 通过 www.hdcms.com/api 来访问 API。
'prefix' => env('API_PREFIX', 'api')

#api域名
'domain' => env('API_DOMAIN', 'api.hdcms.com'),

#版本号
'version' => env('API_VERSION', 'v1')

#开发时开启DEBUG便于发现错误
'debug' => env('API_DEBUG', false)
```
### 接口版本--在routes/api.php中定义
```
$api = app(\Dingo\Api\Routing\Router::class);

#默认配置指定的是v1版本，可以直接通过 {host}/api/version 访问到
$api->version('v1', function ($api) {
    $api->get('version', function () {
        return 'v1';
    });
});

#如果v2不是默认版本，需要设置请求头  
#Accept: application/[配置项 standardsTree].[配置项 subtype].v2+json
$api->version('v2', function ($api) {
    $api->get('version', function () {
        return 'v2';
    });
});
```
### 基础控制器
```
php artisan make:controller Api/Controller
```
```
namespace App\Http\Controllers\Api;

use Dingo\Api\Routing\Helpers;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller as SysController;

class Controller extends SysController
{
    use Helpers;
}
```
### 响应结果--设置响应状态码
```
return $this->response->array(User::get())->setStatusCode(200);

return response()->json(['error' => 'Unauthorized'], 401);
```
### 错误响应
```
// 一个自定义消息和状态码的普通错误。
return $this->response->error('This is an error.', 404);

// 一个没有找到资源的错误，第一个参数可以传递自定义消息。
return $this->response->errorNotFound();

// 一个 bad request 错误，第一个参数可以传递自定义消息。
return $this->response->errorBadRequest();

// 一个服务器拒绝错误，第一个参数可以传递自定义消息。
return $this->response->errorForbidden();

// 一个内部错误，第一个参数可以传递自定义消息。
return $this->response->errorInternal();

// 一个未认证错误，第一个参数可以传递自定义消息。
return $this->response->errorUnauthorized('帐号或密码错误');
```
### 限制请求次数
***使用 api.throttle中间件结合 limit、expires 参数可实现接口次数限制***   
```
$api->version('v1', ['namespace' => '\App\Api'], function ($api) {
    $api->group(['middleware' => 'api.throttle', 'limit' => 2, 'expires' => 1], function ($api) {
        $api->get('user', 'UserController@all');
    });
});
```
***限制1分钟只能访问2次***   
## Jwt
### 安装组件
```
composer require tymon/jwt-auth:2.x
```
### 生成配置文件
```
php artisan vendor:publish
```
### 生成密钥
```
php artisan jwt:secret
```
***这将用 JWT_SECRET=foobar 更新.env文件***   
### 配置说明
***JWT配置文件是 config/jwt.php***   
```
#令牌过期时间(单位分钟)，设置null为永不过期
'ttl' => env('JWT_TTL', 60)

#刷新令牌时间(单位分钟)，设置为null可永久随时刷新
'refresh_ttl' => env('JWT_REFRESH_TTL', 20160)
```
### 更新用户模型示例
***首先，您需要在用户模型上实现 Tymon\JWTAuth\Contracts\JWTSubject 契约，它要求您实现两个方法 getJWTIdentifier() 和 getJWTCustomClaims()***   
```
namespace App;

use Tymon\JWTAuth\Contracts\JWTSubject;
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable implements JWTSubject
{
    use Notifiable;

    /**
     * 获取将存储在JWT主题声明中的标识符.
     * 就是用户表主键 id
     *
     * @return mixed
     */
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    /**
     * 返回一个键值数组，其中包含要添加到JWT的任何自定义声明.
     *
     * @return array
     */
    public function getJWTCustomClaims()
    {
        return [];
    }
}
```
### 配置验证守卫
***修改 config/auth.php 文件以使用jwt保护来为接口身份验证提供支持***   
```
'guards' => [
	'web' => [
		'driver' => 'session',
		'provider' => 'users',
	],
	'api' => [
		'driver' => 'jwt',
		'provider' => 'users',
	],
]
```
***修改dingo配置文件 config/api.php 文件中的身份验证提供者***   
```
'auth' => [
	'jwt' => \Dingo\Api\Auth\Provider\JWT::class,
],
```
### 验证操作
***路由定义***
```
$api = app(\Dingo\Api\Routing\Router::class);
$api->version('v1', ['namespace' => 'App\Http\Controllers\Api',], function ($api) {
    $api->post('login', 'AuthController@login');
    $api->get('logout', 'AuthController@logout');
    $api->get('me', 'AuthController@me');
});
```
***控制器定义***
```
class AuthController extends Controller
{
    public function __construct()
    {
    	// 除login外都需要验证
        $this->middleware('auth:api', ['except' => ['login']]);
    }

    //登录获取token
    public function login()
    {
        $credentials = request(['email', 'password']);

        if (!$token = auth('api')->attempt($credentials)) {
			return $this->response->errorUnauthorized('帐号或密码错误');
        }

        return $this->respondWithToken($token);
    }

    //获取用户资料
    public function me()
    {
        return response()->json(auth('api')->user());
    }

    //销毁token
    public function logout()
    {
        auth('api')->logout();

        return response()->json(['message' => 'Successfully logged out']);
    }

    //刷新token
    public function refresh()
    {
        return $this->respondWithToken(auth('api')->refresh());
    }

    //响应token
    protected function respondWithToken($token)
    {
        return response()->json([
            'access_token' => $token,
            'token_type'   => 'bearer',
            'expires_in'   => auth('api')->factory()->getTTL() * 60,
        ]);
    }
}
```
### 使用令牌
***在postman 工具中可以使用以下方式操作***   
![](/images/note/202009221533.png)
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |