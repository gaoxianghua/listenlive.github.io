---
title: "Vue基础-01"
date: 2020-07-13T14:57:32+08:00
draft: false
url: "/note/2020/07/13/vue01"
categories: 
- 笔记
tags: 
- Vue
- 前端
---
# v-if与v-show
v-if：不显示时，第一次就直接不渲染，如果内容已经显示将其改为不显示，将内容直接从DOM去除。**只渲染一次的内容用v-if**  
v-show:不显示时，就会改为display:none,但是会渲染在DOM上。**反复需要切换的内容使用v-show**
# 使用v-show实现点击切换效果
```
<div id="app">
	<div v-show="isShow" id="pane">
		Hello Vue
	</div>
	<button @click="showPane">切换显示内容</button>
</div>
<script type="text/javascript">
	let app = new Vue({
		el:"#app",
		data:{
			isShow:true
		},
		methods:{
			showPane:function(){
				app.isShow = !app.isShow;
			}
		}
	})
</script>
```
# 使用v-if实现点击切换效果
![菜单切换](/images/vue_tab.png)  
```
<div id="app">
	<h3 v-if="tab==1">首页</h3>
	<h3 v-else-if="tab==2">新闻页</h3>
	<h3 v-else>个人中心</h3>
	
	<button @click="tabChange" data-id="1">首页</button>
	<button @click="tabChange" data-id="2">新闻</button>
	<button @click="tabChange" data-id="3">个人中心</button>
</div>
<script type="text/javascript">
	let app = new Vue({
		el:"#app",
		data:{
			tab:1
		},
		methods:{
			tabChange:function(e){
				let tabid = e.target.dataset.id
				app.tab = tabid //等同于 this.tab = tabid
			}
		}
	})
</script>
```
# 列表渲染 v-for
![列表渲染](/images/vue_list.png)  
```
<div id="app">
	<ul>
		<li v-for="item in city">
			{{item}}
		</li>
	</ul>
</div>
<script type="text/javascript">
	var app = new Vue({
		el:"#app",
		data:{
			city:['北京','天津','上海','重庆']
		}
	})
</script>
```
# 动态绑定
```angularjs
<style>
    .font{
        color:red;
    }
</style>
<div id="app">
    <span :class="hello" :id="sid">Hello</span>
</div>
<script type="text/javascript">
    var app = new Vue({
        el:"#app",
        data:{
            hello:'font',
            sid:'bid'
        }
    })
</script>
```
## 双向绑定
![](/images/202007241546.png)
```angularjs
<div id="app">
    <span>{{form}}</span> <br/>
    <input type="text" v-model="form"/>
</div>
<script type="text/javascript">
    var app = new Vue({
        el:"#app",
        data:{
            form:'表单提交'
        }
    })
</script>
```
_数据只绑定一次的话用_***v-once***   
```angularjs
<h3 v-once>{{form}}</h3>
```
## v-text与v-html
![](/images/202007241626.png)
```angularjs
<div id="app">
    <h2 v-text="tname"></h2>
    <h2 v-html="tname"></h2>
</div>
<script type="text/javascript">
    var app = new Vue({
        el:"#app",
        data:{
            tname:'<h1 style="color: skyblue">listenlive.cn</h1>'
        }
    })
</script>
```
## 动态改变属性
![](/images/202007241820.png)
```angularjs
<style>
    .tname1{color:red}
    .tname2{color:green}
</style>
<div id="app">
    <h2 :class="'tname' + n">COLOR</h2>
    <input type="text" v-model="n"> <br/>
    <input type="radio" v-model="n" value="1"> RED
    <input type="radio" v-model="n" value="2"> GREEN
</div>
<script type="text/javascript">
    var app = new Vue({
        el:"#app",
        data:{
            n:1
        }
    })
</script>
```
## computed - 购物车商品价格计算逻辑
![](/images/202007252044.png)   
```
<div id="app">
    <input type="text" v-model="n1"> +
    <input type="text" v-model="n2"> =
    <input type="text" v-model="sum">
</div>
<script>
    var app = new Vue({
       el:'#app',
        computed:{
          sum:function () {
              return this.n1*1 + this.n2*1; //*1表示将字符串转为整型
          }
        },
       data:{
           n1:0,
           n2:1
       }
    });
</script>
```
## watch - 监听变量的更改
_在常用的搜索场景，输入一个词展示相关搜索结果_   
![](/images/202007252307.png)
### 使用npm安装axios - 跨域请求数据
```
npm install axios
```
### 引入axios
```
<script src="node_modules/axios/dist/axios.js"></script>
```
```
<div id="app">
    <input type="text" v-model="word">
    <h3>
        搜索结果：{{result}}
    </h3>
</div>
<script>
    var app = new Vue({
       el:'#app',
       watch:{
           word: function (newV,oldV) {
               axios.get('0725.php?word='+newV).then(function (response) {
                   app.result = response.data;
               })
           }
       },
       data:{
           word:'',
           result:''
       }
    });
</script>
```
## 使用lodash解决实时请求造成资源浪费问题
![](/images/202007252334.png)
### 安装并引入lodash
```
npm install lodash

<script src="node_modules/lodash/lodash.js"></script>
```
```
<div id="app">
    <input type="text" v-model="word">
    <h3>
        搜索结果：{{result}}
    </h3>
</div>
<script>
    var app = new Vue({
       el:'#app',
       watch:{
           word: _.debounce(
               function (newV,oldV) {
                   axios.get('0725.php?word='+newV).then(function (response) {
                       app.result = response.data;
                   })
               },3000
           )
       },
       data:{
           word:'',
           result:''
       }
    });
</script>
```
_使用_***[_.debounce()](https://www.lodashjs.com/docs/lodash.debounce)***_方法，设置3秒后才请求后台数据，减轻服务器压力_   
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |
