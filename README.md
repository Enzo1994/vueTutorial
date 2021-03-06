## Vue
#### 特色
- 异步渲染（都加载完毕一起渲染）

渐进 响应式
第一步：需要new Vue一个根实例作为入口，挂载到body（已作废）


实例对象包含构造参数：el 、 template 、 data 、 method
var my = new Vue({    el:'#app-4',    template:'<b>{{black}}</b>',    data:{        black:'RealBlack'    },    methods:{}})
第二步：把指令写到template里
注意Build-in components

1、修改innerHTML:{{message}}
2、修改标签内属性:v-bind:'style'， :class＝"{类名:第几个list（index％2）}"
3、给DOM元素绑定事件: html:v-on:click = 'reverseMessage' , js: app.methods.reverseMessage=function(){}
4、条件：html：v-if="seen" 
5、循环：html：
针对data里的数组：v-for="(item,index) in list"，标签内写{{text}} 
针对data里的对象：v-for="(value,key) in objList" 
app4.todos.push({ text: '新项目' })添加新li
6、输入内容：html：（读取输入的内容in input tag）v-model="nba"，js：data：nba：‘此处同placeholder’
一、文本插值（innerHTML）
v-html：会识别标签
v-text：不会识别标签
二、绑定HTML元素属性（setAttribute）

- 数据绑定：
**{{msg}}**：更新数据变化
**{{msg|filterA|filterB}}**：filter:lowercase/uppercase/capitalize/currency '￥'  - currency可通过空格传参以在开头显示 / 
**{{*msg}}**：更新数据变化



- v-bind绑定html属性：
```javascript
<div id="app-2">
  <span v-bind:title="message">   //渲染出来结果是<span title="this is title"></span>
  </span>
</div>

var app2 = new Vue({
    el: '#app-2',
    data: {
      message: 'this is title'
    }})
```
v-bind称为指令
将这个元素节点的 title 特性和 Vue 实例的 message 属性保持一致
还有其他指令：

- v-show、v-if隐藏显示
控制切换一个元素是否显示：
```javascript
<div id="app-3">
  <p v-if="seen">现在你看到我了</p>
</div>

var app3 = new Vue({
    el: '#app-3',  
    data: { 
         seen: true  }})
```

- v-for循环：
  * 数组循环：v-for =“(v,i) in arr” :key="index"
    {{value}}
  * json循环：
    - v-for =“(v,k,i) in json“
    - {{k}} {{v}}
  * filter：(2.0取消内置filter)
    - limitBy 2 1- 取2个，从index 1开始
    - filterBy '谁' - 过滤数据，得到包含 谁 的数据
    - orderBy
一个项目列表：
```javascript
<div id="app-4">
  <ol>
      <li v-for="todo in todos">
            {{ todo.text }}
      </li>
  </ol>
</div>

var app4 = new Vue({
    el: '#app-4',
    data: {
      todos: [
        { text: '学习 JavaScript' },
        { text: '学习 Vue' },
        { text: '整个牛项目' }
        ]}})
```


- v-on绑定事件：mouseover mouseout dblclick
```javascript
<button @click="a=false"> 点击消失
</button>
<div id="app-5">
  <p>{{ message }}</p>
    <button v-on:keydown.enter="reverseMessage">逆转消息</button>
</div>

var app5 = new Vue({
    el: '#app-5',
    data: {    
      message: 'Hello Vue.js!'  
      },  methods: {    
        reverseMessage: function () {      
          this.message = this.message.split('').reverse().join('')    
          }}})
```
- **自定义组件用`@click.native="doSomethin"`加代理**

- v-model双向绑定：
```javascript
<div id="app-6">
  <p>{{ message }}</p>
    <input v-model="message">
</div>
var app6 = new Vue({
    el: '#app-6', 
    data: {    
      message: 'Hello Vue!'  
      }})
```

- 交互：
get/post/jsonp
**创建axios实例：**
```javaScript
var instance = axios.create({
baseURL: 'https://some-domain.com/api/',
timeout: 1000,
headers: {'X-Custom-Header': 'foobar'}
});
```
**获取数据：**

## Vue生命周期： v-1.0
- 钩子函数：
  - beforeCreate函数：组件实例刚被创建
  - created函数：实例创建完成，渲染html和路由前执行-此时静态页面占位数据还未被替换
  - beforeMount
  - **mounted函数挂载：渲染html和路由后执行-ajax或数据初始化以后，用实际数据覆盖掉占位数据以后**
  - beforeUpdate函数：组件更新之前
  - **updated函数：组件更新完毕**


## cloak：
办法一：`<span v-text="msg"></span>`
办法一：`<span v-html="msg"></span>`
方法二：`<span v-cloak>{{msg}}</span>` + `[v-cloak]{display:none}`

## computed计算属性：是属性，return的值就是计算属性的值，可以和其他属性联动，别的属性发生变化，计算属性跟着发生变化
```javascript
data(){
  return {
    a:3
  } 
},
computed:{
  b:function(){  //这是默认的get方法
    return 1
  }
}
```
```javascript
data(){
  return {
    a:3
  } 
},
computed:{
  b:{
    get:function(){  //这是默认的get方法
      return 1
    },
    set:function(val){

    }
  }
}
```

## 自定义过滤器：
- “ | ”能将上一个过滤器输出内容作为下一个过滤器的输入内容。（`<div>{{ content | reverseStr | removeNum }}</div>`）
```javascript
//全局注册vue过滤器，
Vue.filter('filtername',function(input,a,b){ //input:过滤器'|'前的内容，a、b：过滤器的参数：{{input | filtre a b}}
  return input>3?2:1
})
```
- 举例：
  - 时间转换器，
  - 标签过滤器：
    ```javascript
      Vue.filter('filterHtml',{ 
          read:function(val){  /*val是读到的html值*/
              return val.replace(/<[^<]+>/g);
          },
          write:function(){
              return val;
          }
      });
    ```
**Vue2.0的过滤器写法：**
```javascript
<div id="app">  
    <span>SearchByName: </span>  
    <input v-model="searchQuery">  
    <table>  
        <thead>  
        <tr>  
            <td v-for="col in columns">{{col|capitalize}}</td>  
        </tr>  
        </thead>  
        <tbody>  
        <tr v-for="entry in filteredData">  
            <td v-for="col in columns">{{entry[col]}}</td>  
        </tr>  
        </tbody>  
    </table>  
</div>  
<script src="vue.js"></script>  
<script>  
    new Vue({  
        el: '#app',  
        data: {  
            searchQuery: '',  
            columns: ['name', 'gender', 'age'],  
            data: [{  
                name: 'Jack',  
                gender: 'male',  
                age: 26  
            }, {  
                name: 'John',  
                gender: 'female',  
                age: 20  
            }, {  
                name: 'Lucy',  
                gender: 'female',  
                age: 16  
            }]  
        },  
        filters: {  
            capitalize: function (value) {   //方法一：用filters
                return value.charAt(0).toUpperCase() + value.slice(1);  
            }  
        },  
        computed: {  
            filteredData: function () {  
                var self = this;  
                return this.data.filter(function (item) {  
                    return item.name.toLowerCase().indexOf(self.searchQuery.toLowerCase()) !== -1;  
                })  
            }  
        }  
    });  
</script>  
```


## 自定义指令：
拖拽只能放到指令里
```javascript
<div v-drag></div>
Vue.directive('drag',function(){
  var oDiv = this.el //原生DOM元素
  oDiv.onmousedown = function(e){
    var disX = e.clientX-oDiv.offsetLeft;
    var disY = e.clientY-oDiv.offsetTop;
    document.onmousemove = function(e){
      var l = e.clientX-disX;
      var t = e.clientY-disY;
      oDiv.style.left = l+'px';
      oDiv.style.top = t+'px';
    }
  };
  document.onmouseup = function(){
    document.onmousemove = null;
    document.onmouseup = null;
  }
})

```

## 绑定键盘信息：
```javascript
Vue.config.keyCodes.ctrl=17
```

## watch监听数据变化：
```javascript
vm.$watch('dataName',cb) //浅监听
vm.$watch('dataName',cb,{deep:true}) //深监听
```

## Vue实例的简单方法：
- vm.$el => 就是元素
- vm.$data => 就是数据对象
- vm.$mount('#div') => 手动挂在到元素上
- vm.$options => 获取到自定义属性（除data、el、methods等之外的）
- vm.$destory => 销毁对象
- vm.$log() => 查看当前数据状态


## 安装vue-cli:
```
show vue-cli 看版本
npm install -g vue-cli@2.9.6
```

## vue-cli指令：
###### init
生成新项目newProject模板
```
vue init webpack my-project
vue init 模板名 项目名
```
###### list 
查看可以使用的模板vuejs-template
```
  ★  browserify - A full-featured Browserify + vueify setup with hot-reload, linting & unit testing.
  ★  browserify-simple - A simple Browserify + vueify setup for quick prototyping.
  ★  pwa - PWA template for vue-cli based on the webpack template
  ★  simple - The simplest possible Vue setup in a single HTML file
  ★  webpack - A full-featured Webpack + vue-loader setup with hot reload, linting, testing & css extraction.
  ★  webpack-simple - A simple Webpack + vue-loader setup for quick prototyping.
```
###### build
打包发布组件 (打包后会在根目录生成dist文件夹，里面包括加密后的完整项目html js css文件)
###### help
```vue help init```

To get started:

  cd myweb
  npm run dev  开发时使用  开启localhost的8080端口
  npm run build  发布打包  自动生成dist目录，所有文件打包加密 css js html
  
  
```json
 "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "build": "node build/build.js"
  },
 "dependencies": {
    "vue": "2.5.16",
    "vue-router": "3.0.1"
  },
 ```
 

 ## webpack模板工程结构:

* build
  + webpack设置文件
* config
  + 工程设置文件
* src
  + 源文件
* static
  + 静态文件夹(编译时直接拷贝至发布文件夹)
* index.html
  + 网页启动入口文件,头部可以改适应爬虫


 
## 往工程内引入bootstrap

##### 安装：
```cd myweb```
```npm install bootstrap --save --save-exact```  ==--save-exact在生产环境中必须写==

##### 引入：
在src的main.js内引入```import 'bootstrap/dist/css/bootstrap.min.css```

##### 使用：
在src下的App.vue内使用```<button class = "btn btn-primary">确定</button>```



## 组件:


###### 组建基础:
每个模板都必须只有一个根元素

###### 组件位置:
所有组件都在src/components文件夹下

###### 组件格式:  
template:html内容  
script:js脚本(ES6)   
style:组件样式单 

###### 全局组件：
- 声明：
```javascript
const component = {
  template:'<div>This is a component</div>'
}
Vue.component('CompOne',component) //可选写法：自定义组件名（组件可以认为是类，所以大写开头）和组建模板
```
- 使用：
```javascript
new Vue({
  /*components:{
    compOne:component  也可以这么写
  }*/ 
  el:'#root',
  template:'<comp-one></comp-one>'
})
```

## 组件通信：
- this.$emit this.$on
- **父子组件同步办法：父组件每次传一个对象给子组件**
- 单一事件管理组件通信：
  - 通过new一个空vue实例来作为中介：
  ```javascript
  const Event = new Vue();
  ```
  - 通过Event.$emit 和 Event.$on来统一集成和分发通信数据
  ```
  Event.$emit(事件名称，数据)
  Event.$on(事件名称，function(data){}.bind(this))
  ```

###### 子组件获取父组件数据：
- 父组件发送数据： 
  ```javascript
  
  template:"<div>我是局部父组件{{pMsg}}<child :parentMsg='pMsg'></child></div>",//父组件通过子组件标签<child></child>绑定:parentMsg='需要传递的数据'
  ```
- 子组件内需要写props属性来接受父组件传过来的数据：
  - props方法一：`props: ['parentMsg']`
  - props方法二：
    ``` javascript
    props: {
      parentMsg: String //这里指定了字符串类型，如果类型不一 致会警告的哦
    }
    ```
  - props方法三：
    ```javascript
    props: {
        parentMsg : {
            type: String,
            default: 'sichaoyun' 
        }
    }
    ```
###### 子组件给父组件传递数据：
- 子组件通过事件传递，父组件通过事件方法接收
    - 子组件：**this.$emit('父组件接收的事件方法名',传递的数据)**
    - 
- 流程：
    - 子组件把$emit绑定到事件（click）上 => 点击子组件=> 父组件内的子组件模板`<child>`进行接收（注意此处接收依然是子模板，与父模板无关）
    ```javascript
      methods: {
        emitToParent:function() {
          this.$emit("child-event", "我是子组件传上来的数据");
        }
      }
    ```
- 父组件接收：
  - `<child @child-event='getFromChild'></child>`
  ```javascript
  template:
    "<div>我是局部父组件{{pMsg}}<child @child-event='getFromChild'></child></div>",//  注意此时接收还是在child模板内接收
    //...
  methods: {
    getFromChild:function(data) {
      this.pMsg = data
    }
  ```

## slot插槽：
- 在子组件标签内放dom，不会显示，需要提前预留槽位slot，让dom插入
```javascript
//子组件
child:{
  template:"<div>我是子组件,放个插槽<slot></slot></div>"
}
//父组件
parent:{
  template:
    "<div>我是局部父组件<child @child-event='getFromChild'><p>我是插头，我插入上面留给我的slot槽位了</p></child></div>"
}
```
- 多插槽：
```javascript
//子组件
child:{
  template:"<div>我是子组件,在我肚子里放个插槽<slot name='slot2'></slot></div>"
}
//父组件
parent:{
  template:
    "<div>我是父组件<child><p name='slot2'>我是插头2，我插入上面子组件留给我的slot槽位了</p></child></div>"
}
```


## 往工程内安装axios


##### 安装：
```cd myweb```
```npm install --save --save-exact axios vue-axios```

##### 引入：在src的main.js内引入
```
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios,axios)  //注册到vue全局组件，使所有组件都可以使用axios库 
```

##### 使用：在src下的components下组件HelloWorld.vue内使用
```javascript
<template>
  <div class="hello">
   <pre>{{content}}</pre>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data () {
    return {
      content: ""
    }
  },
  mounted(){
    this.axios.post("http://api.komavideo.com/news/list").then(res=>{
      this.content = res.data
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
```

## 自定义css文件
##### 创建：
css文件在src目录的assets文件夹下新建my.css ```cd assets  touch my.css```
##### 引入：
```JavaScript
<script>  //组件的script标签内
import '../assets/my.css'
</script>
```


## Vue动画：
```html
<transition name='fade'
  @before-enter="beforeEnter(el)"  //可以声明事件，回调默认el参数能拿到当前元素，可用el.style
  @enter = "enter"
  @after-enter="after-enter"  //进入动画执行完毕

  @before-leave="beforeLeave(el)"  //离开之前
  @leave = "leave"  //开始离开，离开过程中
  @after-leave="after-leave"  //离开动画执行完毕 要记得还原最初样式
  >
  需要运动的元素
</transition>

class定义：
.fade-enter{} //初始状态

.fade-enter-active{} //最后变化成什么样

.fade-leave{} //最终进入以后的状态

.fade-leave-active{} //最后一步离开后的样子
```
- **transition标签 时间必须要给 1s all ease**

推荐animate.css
- 第一步：新建transition标签，把进入和离开的样式类写进去
  `<transition enter-active-class="bounceInLeft" leave-active-class="bounceOutRight">`
- 第二步：给想要的元素加入class="animated",放进transition标签
    ```html
    <transition enter-active-class="bounceInLeft" leave-active-class="bounceOutRight">
      <p v-show ="show" class="animated"></p>
    </transition>
    ```
- 注：如果多个元素需要动画transition-group + :key：
    `<transition-group> <p class = "animated" v-for="(val,index) in arr" :key="index"> </p></transition-group>`
  
