---
title: "Go-strings和strconv函数的使用"
date: 2020-11-30T17:57:04+08:00
draft: false
toc: true
url: "/note/2020/11/30/golang02"
categories: 
- 笔记
tags: 
- go
- golang
---
### strings.HasPrefix(s string, prefix string) bool :判断字符串s是否以prefix开头  
***例：判断一个url是否以http://开头，如果不是则加上http://***   
```
    func urlProcess(url string) string {
        result := strings.HasPrefix(url, "http://")
        if !result {
            url = fmt.Sprintf("http://%s", url)
        }
        return url
    }
```
### strings.HasSuffix(s string, suffix string) bool:判断字符串s是否以suffix结尾  
***例：判断一个路径是否以“/”结尾，如果不是则加上“/”***   
```
    func pathProcess(path string) string {
        result := strings.HasSuffix(path "/")
        if !result {
            path = fmt.Sprintf("%s/",path)
        }
        return path
    } 
```
### strings.Index(s string, str string) int :判断str在s中首次出现的位置，如果没有出现，则返回-1  
### strings.LastIndex(s string,str string) int :判断str在s中最后出现的位置，如果没有出现，则返回-1   
### strings.Replace(str string, old string,new string, n int) : 字符串替换  
### strings.Count(str string, substr string) int : 字符串计数  
### strings.Repeat(str string, count int) string : 重复count次str  
### strings.ToLower(str string) string : 转为小写  
### strings.ToUpper(str string) string : 转为大写  
### strings.TrimSpace(str string) : 去除字符串首尾空白字符  
### strings.Trim(str string, cut string) : 去除字符串首尾cut字符  
### strings.TrimLeft(str string, cut string) : 去除字符串首cut字符  
### strings.TrimRight(str string, cut string) : 去除字符串尾cut字符  
### strings.Field(str string) : 返回str空格分隔的所有子串slice   
### strings.Split(str string, split string) : 返回str split分隔的所有子串的slice  
### strings.Join(s1 []string, sep string) : 用sep把s1中的所有元素连接起来  
### strconv.Itoa(i int) : 把一个整数i转成字符串   
### strconv.Atoi(str string)(int,error) : 把一个字符串转成整型  
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |


