---
title: "go-求阶乘之和"
date: 2020-11-30T17:06:22+08:00
draft: false
toc: true
url: "/technology/2020/11/30/tech04"
categories: 
- 技术
tags: 
- go
- golang
- 阶乘
---

### 求1!+2!+3!+...+n!阶乘之和
```
package main
    import "fmt"
    func sum(n int) uint64{
        var s uint64 = 1
        var sum uint64 = 0
        for i := 1; i <= n; i++ {
            s = s * uint64(i)
            sum += s
        }
        return sum
    }
    func main() {
        var n int
        fmt.Scanf("%d",&n)
        s := sum(n)
        fmt.Println(n)
    }
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |