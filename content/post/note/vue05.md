---
title: "Vue基础-05"
date: 2020-07-30T11:17:51+08:00
draft: false
url: "/note/2020/07/30/vue05"
categories: 
- 笔记
tags: 
- Vue
- 前端
---
# 使用Vue-cli初始化单页面应用
## 使用npm或[cnpm](http://npm.taobao.org)安装 vue-cli
```
cnpm install -g vue-cli
```
## 使用vue命令安装项目包
```
vue init webpack cli
```
_安装vue-route时选Yes,其他都选No_   
## 进入到项目目录，安装依赖包并运行
```
cd cli  
cnpm install
npm run dev
```
![](/images/note/202007301135.png)
## 引入vue-router.js
```
<script src="https://cdn.bootcdn.net/ajax/libs/vue-router/3.2.0/vue-router.js"></script>
```
## 路由的使用
```
<div id="app">
    <router-link to="/applive">Live</router-link>
    <router-link to="/applisten">Listen</router-link>
    <router-view></router-view>
</div>
<script>
    const applive={
        template:'<h1>appLive</h1>'
    };
    const applisten={
        template:'<h1>appListen</h1>'
    };
    //定义路由器
    let route = new VueRouter({
       routes:[
           {path:'/live',component:applive},
           {path:'/listen',component:applisten},
       ]
    });
    new Vue({
        el:'#app',
        router:route,
        data:{
        },
    });
</script>
```
# vue-router 路由参数以及伪静态
```
<div id="app">
    <router-link to="/content">内容</router-link>
    {{$route.params.cid}}
    {{$route.params.cid}}   <!--在页面中接收参数-->
    <router-view></router-view>

</div>
<script type="text/x-template" id="content">
    <div>
        <button @click="show">参数</button>
    </div>
</script>
<script>
    const content={
        template:'#content',
        methods:{
            show()
            {
                console.log(this.$route.params);    //在控制台接收参数
            }
        }
    };
    //定义路由器
    let routes = [
        {path:'/content-:cid-show-:id.html',component:content},
    ];
    //把组件交给路由
    let router = new VueRouter({routes});

    var app = new Vue({
        el:'#app',
        router,
        data:{
        },
    });
</script>
```
# vue路由参数验证处理，保证路由安全
```
let routes = [
    {path:'/content/:id(\\{2})',component:content},   //正则验证，表示限制id必须为两位数字
];
```
# 内容列表单页面实例与路由别名的使用
```
<div id="app">
    <router-view></router-view>
</div>
<script type="text/x-template" id="home">
    <div>
        <li v-for="v in news">
            <router-link :to="{name:'content',params:{id:v.id}}">{{v.title}}</router-link>
        </li>
    </div>
</script>
<script type="text/x-template" id="content">
    <div>
        <h1>{{field.title}}</h1>
        <p>
            {{field.content}}
        </p>
        <router-link to="/">返回首页</router-link>
    </div>
</script>
<script>
    var data =[
        {id:1,title:'listen',content:'listen内容。。。'},
        {id:2,title:'live',content:'live内容。。。'}
    ];
    const home={
        template:'#home',
        data(){
            return{
                news:data
            }
        }
    };
    const content ={
      template:'#content',
        data(){
          return{
              field:{}
          }
        },
        mounted(){
            var id = this.$route.params.id;
            for(let k=0;k<data.length;k++){
                if(data[k].id==id){
                    this.field = data[k];
                }
            }
        }
    };
    //定义路由器
    let routes = [
        {path:'/',component:home},
        {path:'/content/:id',component:content,name:'content'},
    ];
    //把组件交给路由
    let router = new VueRouter({routes});

    var app = new Vue({
        el:'#app',
        router,
    });
</script>
```
![](/images/note/202007301524.png)
# vue-router路由嵌套
![](/images/note/202007301541.png)
```
<div id="app">
    <router-view></router-view>
</div>
<script type="text/x-template" id="home">
    <div>
        <li v-for="v in news">
            <router-link :to="{name:'content',params:{id:v.id}}">{{v.title}}</router-link>
        </li>
        <router-view></router-view>
    </div>
</script>
<script type="text/x-template" id="content">
    <div>
        <h1>{{field.title}}</h1>
        <p>
            {{field.content}}
        </p>
    </div>
</script>
<script>
    var data =[
        {id:1,title:'listen',content:'listen内容。。。'},
        {id:2,title:'live',content:'live内容。。。'}
    ];
    const home={
        template:'#home',
        data(){
            return{
                news:data
            }
        }
    };
    const content = {
        template: '#content',
        data() {
            return {
                field: {}
            }
        },
        //监听路由变化
        watch: {
            '$route'(to, from) {
                this.load();
            }
        },
        mounted() {
            this.load();
        },
        methods: {
            load() {
                var id = this.$route.params.id;
                for (let k = 0; k < data.length; k++) {
                    if (data[k].id == id) {
                        this.field = data[k];
                    }
                }
            }
        }
    };
    //定义路由器
    let routes = [
        {path:'/',component:home,children:[
            {path:'/content/:id',component:content,name:'content'},
        ]},
    ];
    //把组件交给路由
    let router = new VueRouter({routes});
    var app = new Vue({
        el:'#app',
        router,
    });
</script>
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

