---
title: "Laravel基础01-安装配置"
date: 2020-07-16T10:30:47+08:00
draft: false
url: "/note/2020/07/16/laravel01"
categories: 
- 笔记
tags: 
- Laravel
- PHP
- PHPstorm
---
# 安装
## composer安装
composer命令：
```
composer create-project --prefer-dist laravel/laravel hd
```
Laravel目录：
![laravel目录](/images/LaravelFolder.jpg) 
## 安装提示工具
前往PHP应用商店[packagist.org](https://packagist.org)下载安装代码提示工具 **barryvdh/laravel-ide-helper** 
![](/images/ide_helper.png) 
### 使用composer安装以上ide增强工具
```angular2html
composer require barryvdh/laravel-ide-helper
```
### 把增强工具引入laravel项目中
在/config/app.php providers扩展中添加代码：
```angular2html
Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,
```
![](/images/202007201511.png)
### 执行artisan命令初始化组件
```angular2html
php artisan ide-helper:generate
```
### 重启PHPstorm，完成工具安装 
## 数据库配置
laravel有两个地方可以配置数据库：
1) **/.env 文件 （默认配置项）**
2) **/config/database.php 文件** 
### 使用migration建表
1) 执行**make:migration**命令创建hd数据表PHP文件
```angularjs
php artisan make:migration create_hd_table --create=hd 
```
执行成功后会在**database/migrations**目录中生成PHP文件
![](/images/202007201557.png) 
2) 执行**migrate**命令创建hd数据表
```angularjs
php artisan migrate
```
### 解决MySQL 5.6版本执行migrate命令报错，无法创建表问题
![](/images/202007201631.png) 
错误信息：Illuminate\Database\QueryException  
**解决方法1**：将MySQL升级至5.7+  
**解决方法2**：将/config/database.php mysql配置由**utf8bm4**改为**utf8**  
**解决方法3**：将/app/Providers/AppServiceProvider.php 文件中引入**Schema**  
```angularjs
use Schema;
```
在**boot**方法中添加：  
```angularjs
Schema::defaultStringLength(191);
```
![](/images/202007201650.png)
再次执行**migrate**命令数据表 数据表创建成功  
![](/images/202007201641.png)  
## 前后端路由配置
# 在/routes目录下新建admin目录作为后台路由
# 在/routes/web.php中包含admin目录文件
```angularjs
include __DIR__.'/admin/web.php';
```
![](/images/202007201708.png) 
# 在routes/admin/web.php中配置路由组 
```angularjs
Route::group(['prefix' => 'admin'],function(){
   Route::get('/abc',function(){
     return 'abc';
   });
});
``` 
![](/images/202007201712.png)  
# 输入'域名/admin/abc'即可访问到后台admin路由  
![](/images/202007211009.png)  
## 使用模型（Model）创建表
```angularjs
php artisan make:model Model/Admin -m
```
_-m 表示同时创建migration_
### 编辑database/migrations/下的文件，设置表字段
```angularjs
public function up()
    {
        Schema::create('admins', function (Blueprint $table) {
            $table->increments('id');
            $table->timestamps();
            $table->string('username')->unique(); //表示username字段不允许重复
            $table->string('password'); 
        });
    }
```
### 执行migrate命令创建admins表
```angularjs
php artisan migrate
```
### 为admins表添加数据   
编辑database/factories/Modelfactory.php文件，添加如下代码：   
```angularjs
$factory->define(\App\Model\Admin::class, function (Faker $faker) {
    return [
        'username' => $faker->name,
        'password' => bcrypt('admin888'),
    ];
});
```
接下来使用**tinker**添加数据
```angularjs
php artisan tinker      #启动tinker

>>>factory(App\Model\Admin::class,3)->create();
```
此时admins表会新增3条数据
![](/images/202007211106.png)   

___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |





