---
title: "Vue基础-04"
date: 2020-07-27T16:15:11+08:00
draft: false
url: "/note/2020/07/27/vue04"
categories: 
- 笔记
tags: 
- Vue
- 前端
---
# Vue声明组件(component)的方式
```
<div id="app">
    <live-slide></live-slide>
    <live-news></live-news>
</div>
<script>
    //全局组件
    Vue.component('liveSlide',{
        template:'<h2>listenlive.cn</h2>'
    });
     //局部组件
    var liveNews = {
        template:"<h2>ListenLive</h2>"
    };
    var app = new Vue({
        el:'#app',
        components:{liveNews},
        data:{
        },
    });
</script>
```
# 子组件中data返回数据的实例及text/x-template的使用
```
<div id="app">
    <live-news></live-news>
    <hr/>
    <live-news></live-news>
    <hr/>
    <live-news></live-news>
</div>
<script type="text/x-template" id="liveNews">
    <div>
        <li v-for="(v,k) in news">
            {{v.title}}
            <button @click="del(k)">删除</button>
        </li>
    </div>
</script>
<script>
    //局部组件
    var liveNews = {
        template:"#liveNews",
        // 子组件返回data要使用对象
        data(){
            return {
                news:[
                    {title:'live'},
                    {title:'Taipei'},
                ]
            }
        },
        methods:{
            del(k){
                this.news.splice(k,1);
            }
        }
    };

    //根组件
    var app = new Vue({
        el:'#app',
        components:{liveNews},
        data:{
        },
    });
</script>
```
![](/images/note/202007271700.png)
# 组件间参数的传递
## 父级组件向子组件传递数据
```
<div id="app">
    <live-news :show-del-button="true" :lists="news"></live-news>
</div>
<script type="text/x-template" id="liveNews">
    <div>
        <li v-for="(v,k) in lists">
            {{v.title}}
            <button @click="del(k)">删除</button>
        </li>
    </div>
</script>
<script>
    //局部组件(子组件)
    var liveNews = {
        template:"#liveNews",
        // 子组件返回data要使用对象
        props:['shoeDelButton','lists'],   //使用props接收数据
        methods:{
            del(k){
                this.news.splice(k,1);
            }
        }
    };
    //根组件
    var app = new Vue({
        el:'#app',
        components:{liveNews},
        data:{
            news:[
                {title:'live'},
                {title:'hello'},
                {title:'广州'},
            ]
        },
    });
</script>
```
![](/images/note/202007271732.png)
# props数据的验证
```
<div id="app">
    <live-news :show-del-button="true" :lists="news"></live-news>
</div>
```
```
var liveNews = {
        template:"#liveNews",
        props:{
            showDelButton:{
                type:[Boolean,Number],      //验证值为布尔或数字类型才通过
                required:true       //必须验证
            }
        },
        methods:{
            del(k){
                this.news.splice(k,1);
            }
        }
    };
```
# 子组件使用$emit事件触发父组件实现购物车商品计算功能
![](/images/note/202007271830.png)
```
<div id="app">
    <live-news :lists="goods" @sum="total"></live-news>
    总计：<span>{{totalPrice}}</span>元
</div>
<script type="text/x-template" id="liveNews">
    <table border="1">
        <tr>
            <th>商品名称</th>
            <th>商品价格</th>
            <th>商品数量</th>
        </tr>
        <tr v-for="(v,k) in lists">
            <td>{{v.title}}</td>
            <td>{{v.price}}</td>
            <td>
                <input type="text" v-model="v.num" @blur="sum"/> <!--@blur当元素失去焦点时触发-->
            </td>
        </tr>
    </table>
</script>
<script>
    //局部组件
    var liveNews = {
        template:"#liveNews",
        // 子组件返回data要使用对象
        props:['lists'],   //使用props接收数据
        methods:{
            sum(){
                this.$emit('sum');
            }
        }
    };
    //根组件
    var app = new Vue({
        el:'#app',
        components:{liveNews},
        mounted(){      //挂载点 - 当页面加载时就执行
            this.total();
        },
        data:{
            totalPrice:0,
            goods:[
                {title:'iPhone11',price:5000,num:1},
                {title:'显示器',price:1000,num:1},
                {title:'Macbook',price:8000,num:1},
            ]
        },
        methods:{
            total(){
                this.totalPrice=0;
                this.goods.forEach((v)=>{
                    this.totalPrice += v.num*v.price;
                })
            }
        }
    });
</script>
```
# 使用sync修饰符与computed计算属性 实现购物车原理
```
<div id="app">
    <live-news :lists.sync="goods" @sum="total"></live-news>
    总计：<span>{{totalPrice}}</span>元
</div>
<script type="text/x-template" id="liveNews">
    <table border="1">
        <tr>
            <th>商品名称</th>
            <th>商品价格</th>
            <th>商品数量</th>
        </tr>
        <tr v-for="(v,k) in lists">
            <td>{{v.title}}</td>
            <td>{{v.price}}</td>
            <td>
                <input type="text" v-model="v.num" @blur="sum"/> <!--@blur当元素失去焦点时触发-->
            </td>
        </tr>
    </table>
</script>
<script>
    //局部组件
    var liveNews = {
        template:"#liveNews",
        // 子组件返回data要使用对象
        props:['lists'],   //使用props接收数据
        methods:{
            sum(){
                this.$emit('sum');
            }
        }
    };
    //根组件
    var app = new Vue({
        el:'#app',
        components:{liveNews},
        computed:{
            totalPrice(){
                var sum = 0;
                this.goods.forEach((v)=>{
                    sum += v.num*v.price;
                })
                return sum;
            }
        },
        data:{
            totalPrice:0,
            goods:[
                {title:'iPhone11',price:5000,num:1},
                {title:'显示器',price:1000,num:1},
                {title:'Macbook',price:8000,num:1},
            ]
        },
    });
</script>
```
# 使用动态组件灵活设置页面布局--针对不同用户展示不同界面场景
![](/images/note/202007291758.png)
```
<div id="app">
   <div :is="formType"></div>
    <input type="radio" v-model="formType" value="appInput"/> 文本框
    <input type="radio" v-model="formType" value="appTextarea"/> 文本域
</div>
<script>
    var appInput = {
        template:"<div><input/></div>",
    };
    var appTextarea = {
        template:"<div><textarea></textarea></div>",
    };
    var app = new Vue({
        el:'#app',
        components:{appInput,appTextarea},
        data:{
            formType:"appTextarea",
        },
    });
</script>
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |