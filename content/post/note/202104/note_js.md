---
title: "Js数组操作"
date: 2021-04-08T14:28:34+08:00
draft: false
toc: true
url: "/note/202104/javascript.html"
categories: 
- 笔记
tags: 
- JavaScript
- JS
- 数组
---
# 类型转换  
## 字符串  
大部分数据类型都可以使用`toString()`函数转换为字符串  
```javascript
    console.log(([1,2,3]).toString());      //1,2,3
```
也可以使用函数`String`转换为字符串  
```javascript
    console.log(String([1,2,3]));
```
或者使用`join`连接为字符串  
```javascript
    console.log([1,2,3].join("-"));     //1-2-3
```
## `Array.from`  
使用`Array.from`可将类数组转换为数组，类数组指包含`length`属性或可迭代的对象。  
```javascript
    let str = '你好上海';
    console.log(Array.from(str));       //["你","好","上","海"]
```
为对象设置`length`属性后也可以转换为数组，但要下标为数值或数值字符串
```javascript
    let user = {
        0: '张三',
        '1': 18,
        length: 2
    };
    console.log(Array.from(user)); //["张三", 18]
```
## 展开语法  
### 数组合并  
使用`...`可将数组展开为多个值。
```javascript
    let a = [1,2,3];
    let b = ['a','listenlive.cn',...a];
    console.log(b);     //['a','listenlive.cn',1,2,3]
```
### 函数参数  
使用展示语法可以替接收任意数量的参数
```javascript
    function live(...args){
        console.log(args);
    }
    live(1,2,3,'listenlive.cn');        //[1,2,3,'listenlive.cn']
```
# 解构赋值  
## 基本使用  
```javascript
    let [site,url] = ['listenlive','listenlive.cn'];
    console.log(site);      //listenlive
```
## 剩余解构  
```javascript
    //使用一个变量来接收剩余参数
    let [a,...b] = ['listenlive.cn','beijing','shanghai'];
    console.log(a);     //listenlive.cn
    console.log(b);     //['beijing',shanghai]
```
## 字符串解构  
```javascript
    const [...a] = 'listenlive.cn'
    console.log(a);     //Array(13)
```
# 管理元素  
## 向数组追加元素  
```javascript
    let arr = [1,'listenlive.cn','listenlive'];
    arr[arr.length] = 'dada';
    console.log(arr);       //[1,'listenlive.cn','listenlive','dada']
```
## 使用展开语法批量添加元素  
```javascript
    let arr = [1,2,3,'listen'];
    let live = ['listenlive.cn'];
    live.push(...arr);
    console.log(live);      //['listenlive.cn',1,2,3,'listen']
```
## `push`  
压入元素。返回值为数组元素的数量 
```javascript
    let arr = ['site','listenlive'];
    console.log(arr.push('apple','strawberry'));        //4
    console.log(arr);       //['site','listenlive','apple','strawberry']
```
## `pop`  
从末尾弹出元素，返回值为弹出的元素。  
```javascript
    let arr = ['apple','strawberry','banana'];
    console.log(arr.pop());     //banana
    console.log(arr);     //['apple','strawberry']
```
## `shift`  
从数组前面取出一个元素  
```javascript
    let arr = ['apple','banana'];
    console.log(arr.shift());     //apple
    console.log(arr);     //['banana']
```
## `unshift`  
从数组前面添加元素 
```javascript
    let arr = ['apple','banana'];
    console.log(arr.unshift('strawberry','grape'));     //4
    console.log(arr);     //['strawberry','grape','apple','banana']
```
## `fill`  
### 使用`fill`填充数组元素  
```javascript
    let arr = Array(4).fill('apple');
    console.log(arr);           //['apple','apple','apple','apple']
```
### 指定填充位置  
```javascript
    let arr = [1,2,3,4];
    arr.fill("listenlive.cn",1,2);
    console.log(arr);       //[1,'listenlive.cn',3,4]
```
## `slice`  
数组截取,不会改变原数组  
```javascript
    let arr = [0,1,2,3,4,5];
    console.log(arr.slice(1,3));        //[1,2]
```
## `splice`  
### `splice`可以添加、删除、替换数组中的元素，会对原数组进行改变，返回值为删除的元素  
删除数组元素第一个参数为从哪开始删除，第二个参数为删除的数量。  
```javascript
    let arr = [0, 1, 2, 3, 4, 5, 6];
    console.log(arr.splice(1, 3)); //返回删除的元素 [1, 2, 3] 
    console.log(arr); //删除数据后的原数组 [0, 4, 5, 6]
```
### 通过修改`length`删除最后一个元素  
```javascript
    let arr = ["apple", "banana"];
    arr.length = arr.length - 1;
    console.log(arr);
```
### 通过指定第三个参数来设置在删除位置添加的元素  
```javascript
    let arr = [0, 1, 2, 3, 4, 5, 6];
    console.log(arr.splice(1, 3, 'apple', 'strawberry')); //[1, 2, 3]
    console.log(arr); //[0, "apple", "strawberry", 4, 5, 6]
```
### 向末尾添加元素  
```javascript
    let arr = [0, 1, 2, 3, 4, 5, 6];
    console.log(arr.splice(arr.length, 0, 'apple', 'strawberry')); //[]
    console.log(arr); // [0, 1, 2, 3, 4, 5, 6, "apple", "strawberry"]
```
### 向数组前添加元素  
```javascript
    let arr = [0, 1, 2, 3, 4, 5, 6];
    console.log(arr.splice(0, 0, 'apple', 'strawberry')); //[]
    console.log(arr); // ["apple", "strawberry"，0, 1, 2, 3, 4, 5, 6]
```
# 数组的合并与拆分  
## `join`  
使用`join`连接成字符串  
```javascript
    let arr = [1,'cms','apple'];
    console.log(arr.join('-'));     //1-cms-apple
```
## `split`  
`split` 将字符串分割成数组  
```javascript
    let price = "22,28,aa";
    console.log(price.split(","));      //["22","28","aa"]
```
## `concat`  
`concat`用于连接两个或多个数组  
```javascript
    let arr = ["apple", "strawberrry"];
    let listen = [1, 2];
    let live = [3, 4];
    console.log(array.concat(listen, live)); //["apple", "strawberrry", 1, 2, 3, 4]
```
***也可使用展开语法实现连接***    
```javascript
    console.log([...arr,...listen,...live]);
```
## `copyWithin`  
`copyWithin`从数组中复制一部分到数组中的其他位置  
```javascript
    array.copyWithin(target,start,end); 
    /*
    *参数说明
    *target--必需。复制到指定目标索引位置。
    *start--可选。元素复制的起始位置。
    *end--可选。停止复制的索引位置 (默认为 array.length)。如果为负值，表示倒数。
     */
```
# 查找元素  
## `indexOf`  
`indexOf`从前向后查找元素出现的位置，如果找不到返回-1  
```javascript
    let arr = [7,3,5,8,9];
    console.log(arr.indexOf(3));  //1
    console.log(arr.indexOf(2));  //-1
```
## `includes`  
使用`includes`查找字符串，返回值为布尔类型，方便判断。  
```javascript
    let arr = [7,3,2,6];
    console.log(arr.includes(6));       //true
```
## `find`  
`find`方法找到后会把值返回出来(返回第一次找到的值)，如果找不到返回值为`undefined`  
## `findIndex`  
`findIndex`与`find`的区别是返回索引值  
# 数组排序  
## `reverse`  
反转数组顺序  
```javascript
    let arr = [1,4,2,9];
    console.log(arr.reverse());     //[9,2,4,1]
```
## `sort`  
### 默认从小到大排序  
```javascript
    let arr = [1,4,2,9];
    console.log(arr.sort());     //[1,2,4,9]
```
### 使用排序函数从大到小排序  
```javascript
    let arr = [1, 4, 2, 9];

    console.log(arr.sort(function (v1, v2) {
        return v2 - v1;
    }));       //[9, 4, 2, 1]
```
# 循环遍历  
## `for`  
```javascript
    let lessons = [
        {title: '媒体查询响应式布局',category: 'css'},
        {title: 'FLEX 弹性盒模型',category: 'css'},
        {title: 'MYSQL多表查询随意操作',category: 'mysql'}
    ];
    
    for (let i = 0; i < lessons.length; i++) {
      lessons[i] = `课程: ${lessons[i].title}`;
    }
    console.log(lessons);
```
## `forEach`  
```javascript
    let lessons = [
        {title: '媒体查询响应式布局',category: 'css'},
        {title: 'FLEX 弹性盒模型',category: 'css'},
        {title: 'MYSQL多表查询随意操作',category: 'mysql'}
    ];
    
    lessons.forEach((item, index, array) => {
        item.title = item.title.substr(0, 5);
    });
    console.log(lessons);
```
## `for/in`  
```javascript
    let lessons = [
        {title: '媒体查询响应式布局',category: 'css'},
        {title: 'FLEX 弹性盒模型',category: 'css'},
        {title: 'MYSQL多表查询随意操作',category: 'mysql'}
    ];
    for (const key in lessons) {
        console.log(`标题: ${lessons[key].title}`);
    }
```
## `for/of`  
```javascript
    let lessons = [
        {title: '媒体查询响应式布局',category: 'css'},
        {title: 'FLEX 弹性盒模型',category: 'css'},
        {title: 'MYSQL多表查询随意操作',category: 'mysql'}
    ];

    for (const item of lessons) {
        console.log(`
        标题: ${item.title}
        栏目: ${item.category == "css" ? "前端" : "数据库"}
      `);
    }
```
## 实现取数组最大值的方法  
```javascript
    function arrayMax(array) {
    let max = array[0];
    for (const elem of array) {
        max = max > elem ? max : elem;
        }
        return max;
    }
    
    console.log(arrayMax([1, 3, 2, 9]));
```
# 扩展方法  
## `every`  
every 用于递归的检测元素，要所有元素操作都要返回真结果才为真。  
实现标题的关键词检查  
```javascript
    let words = ['js', 'css', 'php'];
    let title = 'dfgdsgdsfdgjs';
    
    let state = words.every(function (item, index, array) {
      return title.indexOf(item) >= 0;
    });
    
    if (state == false) console.log('标题必须包含所有关键词');
```
## `some`  
使用 some 函数可以递归的检测元素，如果有一个返回true，表达式结果就是真。第一个参数为元素，第二个参数为索引，第三个参数为原数组。  

使用 some 检测规则关键词的示例，如果匹配到一个词就提示违规。  
```javascript
    let words = ['ss', '蛤蟆', 'wa'];
    let title = 'wawawawawwww'
    
    let state = words.some(function (item, index, array) {
        return title.indexOf(item) >= 0;
    });
    
    if (state) console.log('标题含有违规关键词');
```
## `filter`  
使用 filter 可以过滤数据中元素  
下面是获取所有在CSS栏目的课程。  
```javascript
    let lessons = [
      {title: '媒体查询响应式布局',category: 'css'},
      {title: 'FLEX 弹性盒模型',category: 'css'},
      {title: 'MYSQL多表查询随意操作',category: 'mysql'}
    ];
    
    let cssLessons = lessons.filter(function (item, index, array) {
      if (item.category.toLowerCase() == 'css') {
        return true;
      }
    });
    
    console.log(cssLessons);
```
## `map`  
使用 map 映射可以在数组的所有元素上应用函数，用于映射出新的值。
获取数组所有标题组合的新数组  
```javascript
    let lessons = [
      {title: '媒体查询响应式布局',category: 'css'},
      {title: 'FLEX 弹性盒模型',category: 'css'},
      {title: 'MYSQL多表查询随意操作',category: 'mysql'}
    ];
    
    console.log(lessons.map(item => item.title));
```
为所有标题添加上 APPLE  
```javascript
    let lessons = [
      {title: '媒体查询响应式布局',category: 'css'},
      {title: 'FLEX 弹性盒模型',category: 'css'},
      {title: 'MYSQL多表查询随意操作',category: 'mysql'}
    ];
    
    lessons = lessons.map(function (item, index, array) {
        item.title = `[APPLE] ${item['title']}`;
        return item;
    });
    console.log(lessons);
```
## `reduce`  
使用 reduce 与 reduceRight 函数可以迭代数组的所有元素，reduce 从前开始 reduceRight 从后面开始  
### 统计元素出现的次数  
```javascript
    function countArrayELem(array, elem) {
      return array.reduce((total, cur) => (total += cur == elem ? 1 : 0), 0);
    }
    
    let numbers = [1, 2, 3, 1, 5];
    console.log(countArrayELem(numbers, 1)); //2
```
### 取数组中的最大值  
```javascript
    function arrayMax(array) {
    return array.reduce(
        (max, elem) => (max > elem ? max : elem), array[0]
        );
    }
    
    console.log(arrayMax([1, 3, 2, 9]));
```
### 取价格最高的商品  
```javascript
    let cart = [
      { name: "iphone", price: 12000 },
      { name: "imac", price: 25000 },
      { name: "ipad", price: 3600 }
    ];
    
    function maxPrice(array) {
      return array.reduce(
        (goods, elem) => (goods.price > elem.price ? goods : elem),
        array[0]
      );
    }
    console.log(maxPrice(cart));
```
### 计算购物车中的商品总价  
```javascript
    let cart = [
        { name: "iphone", price: 12000 },
        { name: "imac", price: 25000 },
        { name: "ipad", price: 3600 }
    ];
    
    const total = cart.reduce(
        (total, goods) => total += goods.price, 0
    );
    console.log(total); //40600
```
### 获取价格超过1万的商品名称  
```javascript
    let goods = [
        { name: "iphone", price: 12000 },
        { name: "imac", price: 25000 },
        { name: "ipad", price: 3600 }
    ];
    
    function getNameByPrice(array, price) {
        return array.reduce((goods, elem) => {
            if (elem.price > price) {
                goods.push(elem);
            }
            return goods;
        }, []).map(elem => elem.name);
    }
    console.table(getNameByPrice(goods, 10000));
```
###  实现数组去重  
```javascript
    let arr = [1, 2, 6, 2, 1];
    let filterArr = arr.reduce((pre, cur, index, array) => {
        if (pre.includes(cur) === false) {
            pre = [...pre, cur];
        }
        return pre;
    }, []);
    console.log(filterArr); // [1,2,6]
```

___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

