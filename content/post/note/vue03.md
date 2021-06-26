---
title: "Vue基础-03"
date: 2020-07-26T22:01:50+08:00
draft: false
url: "/note/2020/07/26/vue03"
categories: 
- 笔记
tags: 
- 前端
- Vue
---
# 表单提交与prevent操作表单
```angularjs
<div id="app">
    <form action="" @submit.prevent="post('listenlive.cn')">
        <h2>{{times}}</h2>
        <button>提交</button>
    </form>
</div>
<script>
    var app = new Vue({
        el:'#app',
        data: {
            times:new Date()
        },
        methods:{
            post(num){
                alert(num);
            }
        }
    });
```
_点击提交按钮异步提交，时间不变_   
![](/images/note/202007262200.png)
# 事件修饰符stop&prevent&capture-防止冒泡事件
```angularjs
<div id="app">
    <div @click.capture="box" :style="{border:'solid 2px red'}"> 
        <a href="https://listenlive.cn" @click.stop.prevent="links">Blog</a>
    </div>
</div>
<script>
    var app = new Vue({
        el:"#app",
        methods:{
            box(){
                alert('div');
            },
             links(){
                alert('listenlive.cn');
            }          
        }       
    });
</script>
```
_此外还有once修饰符，表示只绑定一次_   
```angularjs
 <a href="https://listenlive.cn" @click.stop.prevent.once="links">Blog</a>
```
# 键盘事件修饰符
```angularjs
<div id="app">
  <input type="text" @keyup.enter="keyEvent"><br/>  <!--按回车键触发-->
  <input type="text" @keyup.delete="keyEvent"><br/> <!--按Del键触发-->
  <input type="text" @keyup.tab="keyEvent"><br/>     <!--按Tab键触发-->
  <input type="text" @keyup.esc="keyEvent"><br/>     <!--按esc键触发-->
  <input type="text" @keyup.space="keyEvent"><br/>   <!--按空格键触发-->
  <input type="text" @keyup.ctrl.65="keyEvent"><br/>   <!--按Ctrl+A键触发-->
</div>
<script>
    var app = new Vue({
        el:'#app',
        methods:{
            keyEvent(){
                alert('listenlive.cn');
            }
        }
    });
</script>
```
# 鼠标事件
```angularjs
<div id="app">
    <div @click.left="handler"></div>   <!--鼠标左键-->
    <div @click.middle="handler"></div>   <!--鼠标中键-->
    <div @contextmenu.prevent="handler"></div>   <!--鼠标右键（一般用于弹出自定义菜单）-->
</div>
```
# 使用Vue将数据分配到表单，并用bootstrap布局，   
使用 **[BootCDN](https://bootcdn.cn)** 引入**Bootstrap**和**JQuery**   
```angularjs
<link href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/js/bootstrap.min.js"></script>
<link href="https://cdn.bootcdn.net/ajax/libs/font-awesome/5.14.0/css/fontawesome.min.css" rel="stylesheet">
```
```angularjs
<div id="app">
    <div class="panel panel-default">
        <div class="panel-heading">
            <h3 class="panel-title">Panel title</h3>
        </div>
        <div class="panel-body">
            <form action="" class="form-horizontal">
                <div class="form-group">
                    <label for="" class="col-sm-2 control-label">标题</label>
                    <div class="col-sm-10">
                        <input type="text" class="form-control" v-model="field.title"/>
                    </div>
                </div>
                <div class="form-group">
                    <label for="" class="col-sm-2 control-label">点击数</label>
                    <div class="col-sm-10">
                        <input type="text" class="form-control" v-model="field.click"/>
                    </div>
                </div>
                <div class="form-group">
                    <label for="" class="col-sm-2 control-label">链接</label>
                    <div class="col-sm-10">
                        <input type="text" class="form-control" v-model="field.url"/>
                    </div>
                </div>
                <div class="form-group">
                    <label for="" class="col-sm-2 control-label">摘要</label>
                    <div class="col-sm-10">
                        <textarea name="" class="form-control" v-model="field.description"></textarea>
                    </div>
                </div>
            </form>
        </div>
    </div>    
</div>
<script>
    var app = new Vue({
        el:'#app',
        data:{
          field:{
              title:'ListenLive',
              click:999,
              url:'listenlive.cn',
              description:'摘要'
          }
        },
        methods:{
        }
    });
</script>
```
![](/images/note/202007271328.png)
# Vue表单处理控件-复选框CheckBox的使用方式
```angularjs
<div id="app">
    <div class="panel panel-default">
        <form action="" class="form-horizontal">
        <div class="panel-heading">
            <h3 class="panel-title">Panel title</h3>
        </div>
        <div class="panel-body">
            <div class="form-group">
                <label for="" class="col-sm-2 control-label">标题</label>
                <div class="col-sm-10">
                    <input type="text" class="form-control" v-model="field.title"/>
                </div>
            </div>
            <div class="form-group">
                <label for="" class="col-sm-2 control-label">属性</label>
                <div class="col-sm-10">
                    <div class="checkbox-inline">
                        <input type="checkbox" v-model="field.flag" value="hot"/>热门
                    </div>
                    <div class="checkbox-inline">
                        <input type="checkbox" v-model="field.flag" value="recommend"/>推荐
                    </div>
                </div>
            </div>
            <div class="form-group">
                <label for="" class="col-sm-2 control-label">点击数</label>
                <div class="col-sm-10">
                    <input type="text" class="form-control" v-model="field.click"/>
                </div>
            </div>
            <div class="form-group">
                <label for="" class="col-sm-2 control-label">链接</label>
                <div class="col-sm-10">
                    <input type="text" class="form-control" v-model="field.url"/>
                </div>
            </div>
            <div class="form-group">
                <label for="" class="col-sm-2 control-label">摘要</label>
                <div class="col-sm-10">
                    <textarea name="" class="form-control" v-model="field.description"></textarea>
                </div>
            </div>
            <div class="form-group">
                <label for="" class="col-sm-2 control-label">类型</label>
                <div class="col-sm-10">
                    <div class="checkbox-inline">
                        <input type="checkbox" v-model="field.draft"/>草稿
                    </div>
                </div>
            </div>
        </div>
            <button class="btn btn-primary col-sm-offset-2">保存</button>
        </form>
    </div>
</div>
<script>
    var app = new Vue({
        el:'#app',
        data:{
          field:{
              title:'ListenLive',
              click:999,
              url:'listenlive.cn',
              description:'摘要',
              draft:false,      //草稿
              flag:[]           //属性
          }
        },
        methods:{
        }
    });
</script>
```
![](/images/note/202007271351.png)
# 安装Vue调试工具
## 在谷歌网上应用商店搜索vue.js devtools并安装
![](/images/note/202007271400.png)
## 打控制台即可使用vue调试工具
![](/images/note/202007271403.png)
# Vue处理表单控件之radio
```angularjs
<div class="radio-inline">
     <input type="radio" v-model="field.type" value="draft"/>草稿
</div>
<div class="radio-inline">
     <input type="radio" v-model="field.type" value="send"/>发送
</div>
```
_使用radio时为v-model设置同样的值_   
```angularjs
<script>
    var app = new Vue({
        el:'#app',
        data:{
          field:{
              title:'ListenLive',
              click:999,
              url:'listenlive.cn',
              description:'摘要',
              draft:false,
              type:'send',
              flag:[]
          }
        },
        methods:{
        }
    });
</script>
```
![](/images/note/202007271420.png)
# Vue处理表单控件之select
![](/images/note/202007271450.png)
```angularjs
<div class="form-group">
    <label for="" class="col-sm-2 control-label">栏目</label>
    <div class="col-sm-10">
        <select v-model="field.cid" class="form-control">
            <option value="">==请选择栏目==</option>
            <option v-for="v in category" :value="v.cid">{{v.title}}</option>
        </select>
    </div>
</div>
```
```angularjs
<script>
    var app = new Vue({
        el:'#app',
        data:{
            category:[
                {cid:1,title:'北京'},
                {cid:2,title:'天津'},
                {cid:3,title:'上海'},
                {cid:4,title:'重庆'},
                {cid:5,title:'广州'},
            ],
          field:{
              title:'ListenLive',
              click:999,
              url:'listenlive.cn',
              description:'摘要',
              draft:false,
              type:'send',
              flag:[],
              cid:5
          }
        },
        methods:{
        }
    });
</script>
```
# 表单修饰符 number、trim、lazy
**number** 将字符串转为整型
```angularjs
<input type="text" class="form-control" v-model.number="field.vlaue"/>
```
**trim** 去除字符串两端的空格
```angularjs
<input type="text" class="form-control" v-model.trim="field.title"/>
```
**lazy** 懒加载-当光标移出输入框时才触发
```angularjs
<input type="text" class="form-control" v-model.trim.lazy="field.title"/>
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |