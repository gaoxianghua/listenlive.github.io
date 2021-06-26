---
title: "go-输出100-999之间的水仙花数"
date: 2020-11-30T16:55:08+08:00
draft: false
toc: true
url: "/technology/2020/08/10/tech03"
categories: 
- 技术
tags: 
- go
- golang
- 水仙花数
---
***水仙花数是指一个 3 位数，它的每个位上的数字的 3次幂之和等于它本身（例如：1^3 + 5^3+ 3^3 = 153）***
```
    package main
    import "fmt"
    func isSxh(n int) bool{
        var i, j, k int
        i = n % 10              //个位
        j = (n / 10) % 10      //十位
        k = (n / 100) % 10      //百位
    
        sum := i*i*i + j*j*j + k*k*k
        return sum == n
    }
    func main() {
        var n int
        var m int
        fmt.Scanf("%d,%d",&n,&m)
        for i := n; i < m; i++ {
            if isSxh(i) == true {
                fmt.Println(i)
            }
        }
    }
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |