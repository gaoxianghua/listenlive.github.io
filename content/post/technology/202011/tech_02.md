---
title: "Go-输出某个区间内所有的素数"
date: 2020-11-30T15:57:21+08:00
draft: false
toc: true
url: "/technology/2020/11/30/tech02"
categories: 
- 技术
tags: 
- go
- golang
---
## Go-输出某个区间内所有的素数
```
    package main
    import "fmt"
    func isPrime(n int) bool {
        for i := 2; i < n; i++ {
            if n%i == 0 {
                return false
            }
        }
        return true
    }
    func main() {
        var n int
        var m int
        fmt.Scanf("%d%d", &n, &m)
    
        for i := m; i < m; i++{
            if isPrime(i) == true {
                fmt.Printf("%d\n",i)
                continue
            }
        }
    }
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |
