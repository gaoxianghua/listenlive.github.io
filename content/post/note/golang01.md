---
title: "Go语言基础01"
date: 2020-11-24T15:28:20+08:00
draft: false
toc: true
url: "/note/2020/11/24/golang01"
categories: 
- 笔记
tags: 
- go
- golang
---
## Golang语言特性
### 垃圾回收
#### 内存自动回收，不需要开发人员管理内存   
#### 开发人员基本只专注业务实现   
#### 只需要new分配内存，无需释放   
###  天然并发  
#### 从语言层面支持并发   
#### goroute,轻量级线程      
#### 基于CSP(Communicating Sequential Process) 模型实现   
### channel  
#### 管道，类似unix/linux中的pipe   
#### 多个goroute之间通过channel进行通信   
#### 支持任何文件类型  
### 多返回值   
#### 一个函数返回多个值   
```
    func calc(a int, b int) (int,int) {
        sum := a + b
        avg := (a+b) / 2
        return sum, avg
    }
``` 
## Go基本语法   
### 新建test.go文件
```
    package main
    
    import(
        "fmt"
    )
    
    func add(a int, b int) int {
        var sum int
        sum = a + b
        return sum
    }
    
    func main(){
        var c int
        c = add(100,200)
        fmt.Println("100+200=",c)
    }
```     
### 运行 go run test.go
![运行结果](/images/note/202011/202011241607.png)  


## 格式化代码
```
    gofmt -w test.go
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |
