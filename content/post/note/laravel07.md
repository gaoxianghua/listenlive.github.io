---
title: "Laravel登录注册Auth验证及使用Policy模型策略管理用户权限"
date: 2020-08-03T20:18:44+08:00
draft: false
url: "/note/2020/08/03/laravel07"
categories: 
- 笔记
tags: 
- PHP
- Laravel
---
# 路由routes/web.php
```
Route::get('/', function () {
    return view('welcome');
});
//user资源
Route::resource('user','UserController');
//退出路由
Route::get('logout','LoginController@logout')->name('logout');
//登录路由
Route::get('login','LoginController@login')->name('login');
//登录处理
Route::post('login','LoginController@store')->name('login');
```
# 登录控制器LoginController.php
```
namespace App\Http\Controllers;
use Illuminate\Http\Request;
class LoginController extends Controller
{
    //登录视图
    public function login()
    {
        return view('user.login');
    }
    //登录处理
    public function store(Request $request)
    {
        $data = $this->validate($request,[
            'email' =>  'email|required',
            'password' =>  'required|min:6',
        ]);
        if(\Auth::attempt($data)){
            session()->flash('success','登录成功');
            return redirect('/');
        }
        session()->flash('danger','登录失败');
        return back();
    }
    public function logout()
    {
        \Auth::logout();
        session()->flash('success','退出成功');
        return redirect('/');
    }
}
```
# 用户控制器UserController.php
```
<?php
namespace App\Http\Controllers;
use App\User;
use Illuminate\Http\Request;
class UserController extends Controller
{
    public function __construct()
    {
        $this->middleware('auth',[
            'except'=>['index','show','create','store']
        ]);
        //已登录用户不显示注册（create）方法
        $this->middleware('guest',[
            'only'=>['create','store']
        ]);
    }
    //用户列表
    public function index()
    {
        $users = User::paginate(10);    //分页
        return view('user.index',compact('users'));
    }
    //用户信息查看
    public function show(User $user)
    {
        return view('user.show',compact('user'));
    }
    //用户注册-视图
    public function create()
    {
        return view('user.create');
    }
    //注册处理
    public function store(Request $request)
    {
        $data = $this->validate($request,[
            'name'  =>  'required|min:4',
            'email'  =>  'required|email|unique:users',
            'password'  => 'required|min:6|confirmed',
        ]);
        $data['password'] = bcrypt($data['password']);
        User::create($data);
        //注册成功后自动登录处理
        \Auth::attempt(['email'=>$request->email,'password'=>$request->password]);
        return redirect()->route('/');
    }
    //用户信息修改-视图
    public function edit(User $user)
    {
        $this->authorize('update',$user);   //验证权限-只能修改当前登录的用户资料
        return view('user.edit',compact('user'));
    }
    //用户信息修改-更新数据
    public function update(Request $request,User $user)
    {
        $this->validate($request,[
            'name'  =>  'required|min:4',
            'password'  => 'nullable|min:6|confirmed',
        ]);
        $user->name = $request->name;
        if($request->password){
            $user->password = bcrypt($request->password);
        }
        $user->save();
        session()->flash('success','修改成功');
        return redirect()->route('user.show',$user);
    }
    //删除
    public function destroy(User $user)
    {
        $this->authorize('delete',$user);
        $user->delete();
        session()->flash('success','删除成功');
        return redirect()->route('user.index');
    }
}
```
# 新建UserPolicy
```
php artisan make:policy --model:UserPolicy
```
## 在app/Providers/AuthServiceProvider.php中注册
```
protected $policies = [
        'App\Model' => 'App\Policies\ModelPolicy',
        'App\User'  =>  UserPolicy::class
    ];
```
## app/Policies/UserPolicy.php
```
public function update(User $user, User $model)
{
    //是管理员 或者当前登录的才可以修改资料
    return $user->is_admin || $user->id == $model->id;
}    

public function delete(User $user, User $model)
{
    //验证管理员才可以删除，并且管理员不可以删除自己
    return $user->is_admin && $user->id != $model->id;
}
```
# 用户列表视图
```
@extends('layout/default')
@section('content')
    <div class="card">
        <div class="card-header">用户别表</div>
        <div class="card-body">
            <table class="table">
                <thead>
                <tr>
                    <th>编号</th>
                    <th>昵称</th>
                    <th>操作</th>
                </tr>
                </thead>
                <tbody>
                @foreach($users as $user)
                <tr>
                    <td scope="row">{{$user['id']}}</td>
                    <td>{{$user['name']}}</td>
                    <td>
                        <div class="btn-group">
                            <a href="{{route('user.show',$user)}}" type="button" class="btn btn-success">查看</a>
                            @can('update',$user)
                            <a href="{{route('user.edit',$user)}}" type="button" class="btn btn-info">修改</a>
                            @endcan
                            @can('delete',$user)
                            <form action="{{route('user.destroy',$user)}}" method="post">
                                @csrf
                                @method('DELETE')
                                <button type="submit" class="btn btn-danger">删除</button>
                            </form>
                            @endcan
                        </div>
                    </td>
                </tr>
                @endforeach
                </tbody>
            </table>
        </div>
        <!--分页-->
        <div class="card-footer text-muted">
            {{$users->links()}}
        </div>
    </div>
@endsection
```
# 使用flash闪存处理提示信息
```
{{--提示信息处理--}}
@foreach(['success','danger'] as $t)
    @if(session()->has($t))
        <div class="alert alert-{{$t}}" role="alert">
            <strong>{{session()->get($t)}}</strong>
        </div>
        @endif
@endforeach
```
#错误信息处理
```
{{--错误信息处理--}}
@if(count($errors)>0)
    <div class="alert alert-warning" role="alert">
        @foreach($errors->all() as $error)
            <strong>{{$error}}</strong>
        @endforeach
    </div>
@endif
```
# 父级视图模板/layout/default.blade.php
```
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Blog 01</title>
    <!-- Fonts -->
    <link href="https://fonts.googleapis.com/css?family=Nunito:200,600" rel="stylesheet">
    <!-- Styles -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.0/dist/css/bootstrap.min.css" integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.0/dist/js/bootstrap.min.js" integrity="sha384-OgVRvuATP1z7JjHLkuOU7Xw704+h835Lr+6QL9UvYjZE3Ipu6Tp75j7Bh/kR0JKI" crossorigin="anonymous"></script>
</head>
<body>
<nav>
    <ul class="nav">
        <li class="nav-item">
            <a class="nav-link active" href="/">首页</a>
        </li>
        <li class="nav-item">
            <a class="nav-link active" href="{{route('user.index')}}">用户列表</a>
        </li>
        @auth
            <li class="nav-item">
                <a class="nav-link">欢迎：<strong>{{auth()->user()->name}}</strong></a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="{{route('user.edit',auth()->user())}}">修改资料</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="{{route('logout')}}">退出</a>
            </li>
            @else
                <li class="nav-item">
                    <a class="nav-link" href="{{route('login')}}">登录</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{{route('user.create')}}">注册</a>
                </li>
        @endauth
    </ul>
</nav>
{{--错误信息--}}
@include('layout._errors')
{{--提示信息--}}
@include('layout._message')
@yield('content')
</body>
</html>
```
![](/images/note/202008041017.png)
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |