---
title: "Larvel-使用Seeder生成测试数据"
date: 2020-08-24T15:38:00+08:00
draft: false
toc: true
url: "/note/2020/08/24/laravel05"
categories: 
- 笔记
tags: 
- Laravel
- PHP
---
## 生成Seeder文件
```
php artisan make:seeder UserSeeder
```
运行后会在database/seeds目录下生成UserSeeder.php 文件   
## 编辑UserSeeder.php 文件
```
public function run()
    {
        //生成30条用户数据，并指定第一条账号的账号邮箱
        $users = factory(\App\User::class,30)->create();
        $user = $users[0];
        $user->name = 'LISTENLIVE';
        $user->email = 'listenlive@126.com';
        $user->save();

    }
```
## 编辑工厂文件database/factories/UserFactory.php
```
$factory->define(App\User::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'password' => bcrypt('admin888'),
        'remember_token' => str_random(10),
    ];
});
```
## 编辑执行Seeder文件database/seeds/DatabaseSeeder.php
```
public function run()
    {
        // $this->call(UsersTableSeeder::class);
        $this->call(UserSeeder::class);
    }
```
## 执行填充数据命令
```
php artisan db:seed
```
![](/images/note/202008241550.png)
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |
