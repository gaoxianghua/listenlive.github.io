---
title: "Vue基础-02"
date: 2020-07-23T17:32:32+08:00
draft: false
url: "/note/2020/07/23/vue02"
categories: 
- 笔记
tags: 
- Vue
- 前端
---
# 使用object与array控制class
```
<style>
    .font{font-size: 20px}
    .color{color: skyblue}
    .red{color: red}
</style>
<div id="app">
   <h2 :class="obj">Vue Object</h2>
    <hr/>
    <h2 :class="[succ,font]">listenlive.cn</h2>
</div>
<script>
    var app = new Vue({
        el:'#app',
        data:{
           obj:{color:true,font:true},  //对象形式
            //数组形式
            succ:'red',
            font:'font'
        }
    });
</script>
```
![](/images/202007261051.png)
# 使用class表达式实现“删除-恢复”功能
```angularjs
<style>
    .font{font-size: 20px}
    .success{color: green}
    .error{color: red}
</style>
<div id="app">
    <li v-for="v in news">
        <span :class="v.status?'success':'error'">{{v.title}}</span>
        <button v-on:click="changeStatus(v,false)" v-if="v.status"> 删除</button>
        <button v-on:click="changeStatus(v,true)" v-if="!v.status"> 恢复</button>
    </li>
</div>
<script>
    var app = new Vue({
        el:'#app',
        methods:{
          changeStatus:function (item,status) {
              item.status = status;
          }
        },
        data:{
            news:[
                {title:'北京',status:true},
                {title:'上海',status:true},
                {title:'广州',status:true},
            ]
        }
    });
</script>
```
![](/images/note/202007261141.png)
# 使用Vue设置行内样式与动态改变style样式
```angularjs
<div id="app">
   <h2 :style="{color:'red',fontSize:size+'px'}">动态改变大小</h2>
   <h2 :style="style">Listenlive</h2>
   <h2 :style="[live]">listenlive.cn</h2>
    <input type="number" v-model="size"/>
</div>
<script>
    var app = new Vue({
        el:'#app',
        data:{
            red:'green',
            size:16,
            style:{
                color:'blue'
            },
            live:{
                color:'yellow',
                backgroundColor:'blue'
            }
        }
    });
</script>
```
![](/images/note/202007261200.png)
# v-if 在注册场景下的实际应用
```angularjs
<div id="app">
  <input type="text" v-model="age"/>
    <span v-if="age<20">小孩</span>
    <span v-else-if="age<30">青年</span>
    <span v-else-if="age<50">中年</span>
    <span v-else="age<50">老年</span>
    <hr/>
    <input type="checkbox" v-model="copyright">接受协议<br/>
    <span v-if="copyright==false">请先接受协议</span>
    <button v-else>注册</button>
</div>
<script>
    var app = new Vue({
        el:'#app',
        data:{
            copyright:false,
            age:0
        }
    });
</script>
```
![](/images/note/202007261638.png)
# 使用key值解决表单切换混乱问题
```angularjs
<div id="app">
    <template v-if="regType=='mail'">
        邮箱：<input type="text" name="account" key="mail-register">
    </template>
    <template v-else>
        手机：<input type="text" name="account" key="phone-register">
    </template>
    <hr/>
    <input type="radio" value="mail" v-model="regType">邮箱注册
    <input type="radio" value="phone" v-model="regType">手机注册
</div>
<script>
    var app = new Vue({
        el:'#app',
        data:{
            regType:'mail'
        }
    });
</script>
```
_此时点击切换注册方式则会清空表单内容_   
![](/images/note/202007261653.png)
# v-for 循环列表数据，并使用key添加序号
```angularjs
<div id="app">
    <table border="1">
        <tr>
            <th>序号</th>
            <th>编号</th>
            <th>标题</th>
            <th>城市</th>
        </tr>
        <tbody>
        <tr v-for="(field,key) in news">
            <td>{{key+1}}</td>
            <td>{{field.id}}</td>
            <td>{{field.title}}</td>
            <td>{{city}}</td>
        </tr>
        </tbody>
    </table>
</div>
<script>
    var app = new Vue({
        el:'#app',
        data:{
           city:'台北',
            news:[
                {id:20200725,title:'listenlive.cn'},
                {id:20200726,title:'Hello Taipei'}
            ]
        }
    });
</script>
```
![](/images/note/202007261713.png)
# v-for 结合计算属性（computed）实现根据条件筛选数据功能
![](/images/note/202007261741.png)
```angularjs
<div id="app">
    <li v-for="v in stus">
        {{v.name}} - {{v.sex}}
    </li>
    <input type="radio" v-model="type" value="girl">Girl
    <input type="radio" v-model="type" value="boy">Boy
</div>
<script>
    var app = new Vue({
        el:'#app',
        computed:{
          stus(){
              if(this.type=='all'){
                  return this.user;
              }else{
                  var Type = this.type;
                  return this.user.filter(function (v) {
                      return v.sex == Type;
                  })
              }
          }
        },
        data: {
            type:'all',
            user:[
                {name:'小明',sex:'boy'},
                {name:'小麗',sex:'girl'},
                {name:'小芳',sex:'girl'},
                {name:'小五',sex:'boy'},
                {name:'張三',sex:'boy'},
                {name:'李四',sex:'girl'},
                {name:'貓貓',sex:'girl'},
                {name:'狗狗',sex:'boy'},
            ]
        }
    });
</script>
```
# Vue改良push语法-发表评论功能的实现
```angularjs
<div id="app">
    <li v-for="v in comments">
        {{v.content}}
    </li>
    <textarea v-model="current_content" cols="30" rows="10"></textarea><br/>
    <button v-on:click="push">發表評論</button>
</div>
<script>
    var app = new Vue({
        el:'#app',
        data: {
            current_content:'',
            comments:[
                {content:'小明'},
                {content:'小麗'},
                {content:'小芳'},
                {content:'小五'},
                {content:'張三'},
                {content:'李四'},
            ]
        },
        methods:{
            push(){
                var content = {content:this.current_content};
                this.comments.push(content);
                this.current_content = '';  //清空文本域
            }
        }
    });
</script>
```
![](/images/note/202007261806.png)   
# 使用unshift和pop函数实现数据插入及删除功能
![](/images/note/202007261827.png)
```angularjs
<div id="app">
    <li v-for="v in comments">
        {{v.content}}
    </li>
    <textarea v-model="current_content" cols="30" rows="10"></textarea><br/>
    <button v-on:click="push('pre')">發表到前面</button>
    <button v-on:click="push('end')">發表到後面</button>
    <br/>
    <button v-on:click="del('last')">刪除最後一條評論</button>
    <button v-on:click="del('first')">刪除第一條評論</button>
</div>
<script>
    var app = new Vue({
        el:'#app',
        data: {
            current_content:'',
            comments:[
                {content:'小明'},
                {content:'小麗'},
                {content:'小芳'},
                {content:'小五'},
                {content:'張三'},
                {content:'李四'},
            ]
        },
        methods:{
            push(type){
                var content = {content:this.current_content};
                switch(type){
                    case 'end':
                        this.comments.push(content);       //发表到最后一条评论
                        this.current_content = '';
                        break;
                    case 'pre':
                        this.comments.unshift(content);    //发表到第一条评论
                        this.current_content = '';
                        break;
                }
            },
            del(type){
                switch (type) {
                    case 'last':
                        this.comments.pop();    //删除最后一条评论
                        break;
                    case 'first':
                        this.comments.shift();  //删除第一条评论
                        break;
                }
            }
        }
    });
</script>
```
# 使用splice函数实现单条数据的删除
![](/images/note/202007261846.png)
```angularjs
<div id="app">
    <li v-for="(v,k) in comments">
        {{v.content}} <button v-on:click="remove(k)">刪除</button>
    </li>
    <textarea v-model="current_content" cols="30" rows="10"></textarea><br/>
    <button v-on:click="push('pre')">發表到前面</button>
    <button v-on:click="push('end')">發表到後面</button>
    <br/>
    <button v-on:click="del('last')">刪除最後一條評論</button>
    <button v-on:click="del('first')">刪除第一條評論</button>
</div>
<script>
    var app = new Vue({
        el:'#app',
        data: {
            current_content:'',
            comments:[
                {content:'小明'},
                {content:'小麗'},
                {content:'小芳'},
                {content:'小五'},
                {content:'張三'},
                {content:'李四'},
            ]
        },
        methods:{
            //单条数据刪除
            remove(k){
                this.comments.splice(k,1);
            },
            push(type){
                var content = {content:this.current_content};
                switch(type){
                    case 'end':
                        this.comments.push(content);        //发表到最后一条评论
                        this.current_content = '';
                        break;
                    case 'pre':
                        this.comments.unshift(content);     //发表到第一条评论
                        this.current_content = '';
                        break;
                }
            },
            del(type){
                switch (type) {
                    case 'last':
                        this.comments.pop();    //删除最后一条评论
                        break;
                    case 'first':
                        this.comments.shift();  //删除第一条评论
                        break;
                }
            }
        }
    });
</script>
```
# sort排序与reverse反向排序
```angularjs
  //排序
 sort(){
          this.comments.sort(function (a,b) {
           return a.id > b.id;
          })
         },
  //反向排序
 reverse(){
             this.comments.reverse();
            },
```
# 使用filter与RegExp方法实现字符匹配搜索功能
![](/images/note/202007262017.png)
```angularjs
<div id="app">
    <li v-for="(v,k) in comments">
        {{v.id}} - {{v.content}} <button v-on:click="remove(k)">刪除</button>
    </li>
    <textarea v-model="current_content" cols="30" rows="10"></textarea><br/>
    <button v-on:click="push('pre')">發表到前面</button>
    <button v-on:click="push('end')">發表到後面</button>
    <br/>
    <button v-on:click="del('last')">刪除最後一條評論</button>
    <button v-on:click="del('first')">刪除第一條評論</button><br/>
    <button v-on:click="sort()">排序</button>
    <button v-on:click="reverse()">反向排序</button><br/>
    <input type="text" v-on:keyup.enter="search" v-model="search_content"/>
</div>
<script>
    var app = new Vue({
        el:'#app',
        data: {
            current_content:'',
            //搜索内容
            search_content:'',
            comments:[
                {id:5,content:'小明'},
                {id:6,content:'小麗'},
                {id:1,content:'Live'},
                {id:4,content:'Listen'},
                {id:2,content:'張三'},
                {id:3,content:'李四'},
            ]
        },
        methods:{
            //搜索
            search(){
              this.comments = this.comments.filter((item)=>{
                  var reg = new RegExp(this.search_content,'i');    //'i'模式修正符，不区分大小写进行搜索
                  return reg.test(item.content);
              })
            },
            //排序
            sort(){
                this.comments.sort(function (a,b) {
                    return a.id > b.id;
                })
            },
            //反向排序
            reverse(){
              this.comments.reverse();
            },
            //单条数据刪除
            remove(k){
                this.comments.splice(k,1);
            },
            push(type){
                var content = {content:this.current_content};
                switch(type){
                    case 'end':
                        this.comments.push(content);
                        this.current_content = '';
                        break;
                    case 'pre':
                        this.comments.unshift(content);
                        this.current_content = '';
                        break;
                }
            },
            del(type){
                switch (type) {
                    case 'last':
                        this.comments.pop();
                        break;
                    case 'first':
                        this.comments.shift();
                        break;
                }
            }
        }
    });
</script>
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |