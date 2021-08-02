# Vue

## 18.3 响应式属性原理

### 1.能让数组在视图中更新的操作

push

pop

splice

unshift

shift

sort

reverse



### 2.不能在视图中更新的

1.修改数组的索引

2.数组的长度length



对象是引用变量

对象在视图中更新操作

1.可以修改

2.不能添加和删除属性

# 19

## 19.2 组件模板的写法

定义,挂载,组件

```js
let son={
    template:'<div>zi</div>'
}
let vm=new Vue({
    el:"#app",
    data:{
        
    },
    components:{	//存放组件的子组件
        son:son		//son也可简写
    }
})
```

1.template

```html
<template id="test">
    <div>zi</div>
</template>

let son={
    template:'#test'	//必须要id,class会报错 独一无二的
}
```

2.script

```html
<script type="text/template" id="test">
    <div></div>
 </script>
let son={
    template:'#test'	 
}
```

## 19.3 父子组件传值

非父子4.5

子组件的data必须是一个函数,返回一个对象

```js
data(){
    return {
        
    }
}
```

### 组件细节

#### 1.is属性

是这个template

```html
<table>
    <tr is ="row"></tr>
</table>

Vue.component('row',{
	template:'<tr><td>this is a row</td></tr>'
})
```

#### 2.ref

```js
<div ref="hello"></div>
this.$refs.hello		//本身的元素（若不是组件的话）

<template ref="temp"></template>
this.$refs.temp			//获取到组件的引用
```



### 父组件向子组件传值:

```js
<son :msg="msg"></son>

props:['msg'] 		//数组,多条数据显示,传什么接什么
props:{			//想对传进来的进行过滤
    msg:{
        type:Number,	//限定类型 type:[Number,string]
        default:200		//默认值
        required:true	//设置属性 必传
        validator(value){	//过滤
            return value>10
        }
    }
}
```

### 子组件向父组件传值:

```js
<son @xxx='x'></son>		//左边是子，右边是父

子组件
methods:{
    handleClick(){
        this.$emit('xxx',this.name);	//向父组件发射xxx事件 ,后面可以带多个参数
    }
}

父组件
methods:{
    x(value){
        console.log("xxx",arguments,value
                   );
    }
}
```

## 19.4



# 22.1 自定义事件和插槽

## 1.组件名称

大型项目要加一个前缀

BaseButton,BaseTable,BaseIcon

命名

文件名和组件名

组件名需要用烤肉串式

#### 组件名称要求

(编辑器按字母顺序排序，名称最好前面有功能前缀)

反例：myButton.vue  , VueTable.vue  , Icon.vue等等。。。

正例：1.BaseButton.vue, BaseTable , BaseIcon.vue   2.AppButton.vue ,AppTable.vue    3.VButton ,VTable

**两种命名方式**

1.驼峰

2.以‘-’分隔 如：kebab-case  短横线分隔命名

但是标签最好不使用驼峰，因为浏览器不识别，所以组件标签名最好使用第二种。

#### 自定义事件之事件名称

（不使用驼峰命名）



## 2.组件注册

**全局注册**

```js
//定义一个名为button-counter的全局组件
Vue.component('button-counter',{
	data:function(){
		return{
			count:0
		}
	},
	template: '<button v-on:click="count++">you clicked me {{conut}} times <button>'
})
```

**局部注册**

```js
//定义一个名为button-counter 的新组件，并局部注册。
//1.通过一个普通的 JavaScript 对象来定义组件
var ComponentA = {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
}

//2.注册组件，在使用ComponentA的components 选项中注册想要使用的组件
var ComponentB = {
components: {
	' button-counter': ComponentA    //组件名称：选项对象 ，当两个名称相同时，可以省略一个
},
// ...
}
```

## 3.自定义事件

1.事件名定义要用烤肉串式

2.不能混用

v-model

```js
  Vue.component('test', {
            model: {             //数据的指向和事件触发的选项
                prop: 'checked',
                event: 'change'  //事件
            },
            props: {        //值的基本验证
                checked: Boolean     //值的类型的限制
            },
            template: `
            <div>
                <input type='checkbox' v-bind:checked='checked' v-on:change="$emit('change',$event.target.checked)">
                <p>{{checked}}</p>
            </div>
            `

        })
         new Vue({
            el: '#app',
            data: {
                lovingVue: true
            }
        })
```

父组件的方法放在子组件头会有效果,但是只是他的父级div有效果

```js
 <test v-model='lovingVue' @click.native="clickfn"></test>
```

```JS
  <test @click='clickfn' ></test>

 <script type="text/javascript">
        Vue.component('test', {
            template: `
            <div>
                <button v-on="$listeners">点击</button> //将监听器绑到特定的函数元素里面
            </div>
            `
        })
```



```html
<body>
    <div id="app">
        <test @click='clickfn' v-on:dblclick="dblclickfn" ></test>
        {{lovingVue}}
    </div>
    <script type="text/javascript">
        Vue.component('test', {
            methods:{
                myclickfn(){
                    console.log("this.$listeners");
                    console.log(this.$listeners);
                }
            },
            template: `
            <div>
                <button v-on="$listeners">点击</button> //全部事件在里面
                <button v-on:dblclick="this.$listeners.dblclick">点击</button>      //特定的事件
            </div>
            `

        })
```

## 4.插槽

类似于es6的函数默认参数

例子  （v-slot为vue2.6以上支持，摒弃了slot）

将在标签中间所有内容完全替换掉,而且内容会全部变成string类型

```js
<slot>	</slot>
```

```html
<div id="app">
        <test v-on:click='clickfn' v-on:dblclick='dbclickfn'>
        	<!--写法1 子组件传参-->
            <template v-slot:slot1="slot1props">
                   444{{slot1props.msgdata}}
            </template>
            <!--写法2 子组件传参，解构赋值-->
               <!--slot1两次写完会覆盖-->
			 <template v-slot:slot1={msgdata}>
                   555{{msgdata}}
            </template>
            <template slot="slot2">
                666
            </template>
    		<!-- v-slot可以定义默认插槽内容，slot不可以 -->
            <template v-slot="default">
                777
            </template>
        </test>
    </div>
    <script>
        let vm2 = Vue.component('test', {
    		data() {
                return {
                    msg: 'hello world',
                    name: "zhangsan"
                }
            },
            template: `<div>
            			<h5>test测试</h5>
                        <slot name="slot1" v-bind:msgdata="msg">11111</slot> 
						<p>分割111111</p>
                        <slot name="slot2">22222</slot>
						<p>分割222222</p>
						<slot name="slot3">33333</slot>
                        </div>`
        });

        let vm = new Vue({
            el: "#app"   
        })
    </script>
```

默认插槽

具名插槽

2.插槽可以写name

```html
父组件
<template v-slot:cha1>
    <p>dfsdfasd</p>
    <p>dfsdfasd</p>
    </template>


子组件
    <slot name="cha1"></slot>
```

3.如果定义了具体名字,模板中间没有的话则插不进去

4.不写名字则插的进去

写了名字就 插到名字里面, 没有写名字就全部插到没有名字的里面

```html
父组件
  <test @click='clickfn' v-on:dblclick="dblclickfn" >
	<template v-slot:cha1="cha1props">
  	  <p>{{cha1props.msgdata}}</p>
    </template>
</test>

子组件
    <slot name="cha1" v-bind:msgdata='msg' v-bind:namedata='name'></slot>  //在插槽上绑定了两个值
```

#### 子组件传参

使用默认插槽的话可以省去步骤,只把 v-slot:cha1="cha1props"写在根节点上

```html
父组件
<test @click='clickfn' v-on:dblclick="dblclickfn"  v-slot:default="slotdata">
  	  <p>{{slotdata.msgdata}}</p>
</test>
```

但是不建议 ,且不要内嵌, 一定要清晰

#### 子组件传参,解构赋值

可以直接传值

```html
父组件
  <test @click='clickfn' v-on:dblclick="dblclickfn" >
	<template v-slot:default="{msgdata,namedata}">
  	  <p>{{msgdata}}</p>
      <p>{{namedata}}</p>
    </template>
</test>
```

更可以单个直接赋值

```html
父组件
  <test @click='clickfn' v-on:dblclick="dblclickfn" >
	<template v-slot:default="{msgdata:msg,namedata:name}">
  	  <p>{{msg}}</p>
      <p>{{name}}</p>
    </template>
</test>
```

#### 插槽缩写缩写:

```html
 v-slot:default       #defasult
```

#### 作用域插槽

使用场景：子组件做循环或者某一部分的dom结构应该由外部传进来的时候，dom操作，样式都由父组件来。

```js
			<child>
				<template slot-scope="props">			<!-- 一定要有template占位符 -->
					<li>{{props.item}}</li>
				</template>
			</child>


			Vue.component('child', {					
				data:function(){
					return{
						list:[1,2,3,4]
					}
				},
				template: `<div>
					<ul>
						<slot v-for="item of list" :item=item></slot>
					</ul>
				</div>`
			})
```



### 5.选项卡案例

#### 全局选项卡案例

```html
<body>
    <div id="app">
        <button type="button" v-for="tab in tabs" v-on:click="currentTab=tab">{{tab}} </button>
        <component :is="currentTabComponent"></component>
    </div>
    <script type="text/javascript">
        //全局
        Vue.component('tab-home', {
            template: `
            <div>
                主页选项卡
            </div>
            `
        })
        Vue.component('tab-news', {
            template: `
            <div>
                新闻选项卡
            </div>
            `
        })
        Vue.component('tab-img', {
            template: `
            <div>
                图片选项卡
            </div>
            `
        })

        let vm = new Vue({
            el: '#app',
            data: {
                currentTab: 'home',
                tabs: ['home', 'news', 'img']
            },
            computed: {
                currentTabComponent() {
                    return 'tab-' + this.currentTab;
                }
            }
        })

    </script>
</body>
```

#### 局部选项卡案例

```html
<body>
    <div id="app">
        <button type="button" v-for="tab in tabs" v-on:click="currentTab=tab">{{tab.name}} </button>
        <component :is="currentTab.component"></component>	//is后面跟的要么是对象本身要么是指向对象的地址,比如说某个变量
    </div>
    <script type="text/javascript">
        //局部
        let tab = [{
            name: 'home',
            component: {
                template: `<div>主页选项卡</div>  `
            }
        },
        {
            name: 'news',
            component: {
                template: `<div>新闻选项卡</div>  `
            }
        }, {
            name: 'img',
            component: {
                template: `<div>图片选项卡</div>  `
            }
        }]

        let vm = new Vue({
            el: '#app',
            data: {
                tabs: tab,
                currentTab: tab[0]    
            }
        })

```

# 23 vue 的动画效果

```css
.fade-enter{

}
.fade-enter-active{}
.fade-leave-to{}


<transition name="fade">
	<div>hello world</div>
</transition>
```

transition 内使用v-if和v-show都可以

animated.css	直接用动画库

1.自定义class形式

2.class里面必须包含animated这个类，根据入场和出场不同设置

## 23.1单元素/组件的过渡

1.触发式

```css
  /* name的值来替换 v */
v-enter-active	元素从消失到显示出来后,并且动画完成	0->1
v-enter				显示前					    0
v-enter-to			显示后的					1
v-leave-active		显示到消失
v-leave				消失前						1
v-leave-to			消失后	 					0
```

###  实现过渡效果的部分

```css
 /* 实现过渡效果的部分 */
        .fade-enter-active,.fade-leave-active{
            /* 定义了两个状态,过度参数 */
            /* 动画时间是0.5s,延迟曲线是默认 */
            transition: all .5s;
        }
        .fade-enter,.fade-leave-to{
            /* 监听的过渡样式 */
            opacity: 0;        /* 消失或显示的时间*/
            color: red;
            transform:translateX(50px)
        }
```

2.主动式

```css
@keyframes bounce{
    0%{
        transform:scale(0);
    }
    50%{
         transform:scale(1.5);
    }
    100%{
         transform:scale(1);
    }
}

.enter{
    animation:bounce-in 1s reverse;
    transform-origin:left center;
}
<transition enter-active-class="active">	//可以自定义名字
</transition>
```

3.使用外带动画

```html
<link rel="stylesheet" type="text/css" href="src/animate.css"/>

<transition name="fade" <!-- 可以自定义 -->
            <!-- 可以避免更改css新出的 -->
         	enter-active-class="animated"   
            enter-leave-class="animated"
></transition>
```

## 23.2多元素/组件的过渡

##### 1.多元素的过渡

```html
<transition name="fade"  enter-active-class="animated"    enter-leave-class="animated"
          <!-- 重叠解决方案 --> 
    mode="out-in"		<!-- out-in 当前元素先消失,后面元素再进来  in-out--> >

   <!--写法1 if else 本质是同一个元素	vue会复用 -->
	<p v-if="show" key="one">		    <!-- 使用不同的key来区分是同一元素不同需求 -->
  		{{msg}}  
	</p>
	<p v-else key="two">{{msg+"2"}}</p>
 	<!--写法2 -->
	<p v-bind:key="show">{{show?msg:"hello world"}}</p>

</transition>
```

##### 2.组件间的过渡

```html
<transition>
    <component :is="type"></component>
</transition>
<script>
	Vue.component('child',{
        template:'<div>child</div>'
    })
    Vue.component('child-one',{
        template:'<div>child-one</div>'
    })
    var vm=new Vue({
        el:'#root',
        data:{
            type:'child'
        },
        methods:{
            handleClick(){
                this.type=this.type ==='child'?'child-one':'child'
            }
        }
    })
</script>
```

##### 3.vue中的列表过渡

```html
<tansition-group >		//等于每个div外面都包裹着transition
    <div v-for="item of list" :key="item.id">
        {{item.title}}
    </div>
</tansition-group>

.v-enter,v-leave-to{
	opacity:0;
}
v-enter-active,.v-leave-active{
	transition:opacity 1s;
}
```

##### 4.动画封装

```html
<fade :show="show">
    <div>hello world</div>
</fade>

<fade :show="show">
    <h1>hello world</h1>	//仅改变这里的内容既可
</fade>

<script>
    //可以完整的把一个动画的实现封装在一个组件里面
    Vue.component('fade',{
    props:['show'],
    template:`<transition @before-enter="handlebeforeEnter" @enter="handleEnter" >
				<slot v-if="show"></slot>
    		</transition>`,
    methods:{
        handlebeforeEnter(el){
            el.style.color='red'
        },
        handleClick(){
            this.show=!this.show
        	}
    	}
    })
 
</script>
```



## js实现动画效果

Velocity.js

```html
入场动画效果
<transition 
name="fade" 
@before-enter="handleBeforeEnter"
@enter="handleEnter"
@after-enter="handleEnter"	done执行完调用这个函数
></transition>
出场动画效果
<transition 
name="fade" 
@before-leave="handleBeforeLeave"
@leave="handleLeave"
@after-leave="handleLeave"	done执行完调用这个函数
></transition>
<script>
handleBeforeEnter:function(el){  	//el：动画包裹的div标签
    console.log('beforeEnter')
    el.style.color='red'
}
handleEnter(el,done){
    el.style.color="green"
      done()						//当函数执行完后手动调用done函数，告诉动画已经执行完了
}
</script>
```



# 24 Vue的属性和生命周期

## 24.1 vue的属性方法和生命周期

```js
vm.$el //绑定他的标签 
```

```html
    <div id="app">
        {{name}} 
    </div>
    <script type="text/javascript">
        let data={name:'wanzhang',data:"你好世界"};		 /*此data和下面的data无相关性*/
        let vm = new Vue({
            el: "#app",
      /*key*/data: data, /*这个才是变量*/
            watch:{
                name:function(){
                    console.log("hello world")
                }
            }
        })
    </script>
```

### 生命周期

1.created（数据种类）的数据执行好了，但是template标签模板还未被编译（beforeMount）（元素种类），再编译render函数。

2.所以mounted就是数据与元素的结合。

3.改变数据的时候,生命周期执行beforeUpdate和updated,而且在beforeUpdate的时候，数据已经完成了更新，只是没有渲染到屏幕上。

4.在beforeUpdate的时候，统一数据更新，在updated的时候再统一显示到屏幕上。

5.destoryed和beforeDestory没有基本变化。

**6.切换组件的话**

beforeCreate=>created=>beforeMount=>beforeDestroy=>destroyed=>mounted

至少要把第二个准备好再清掉第一个。不能让用户看空白页面。

7.二次更新的话就是beforeUpdate=>updated

### 2.props父组件传到子组件的数据

```js
props:{
    type:;			//类型
    default:;		//默认值
    required:;		//boolean 是否需要
    validator:;		//验证函数
}
```

### 3.$options 找到自定义属性

```js
definedabcd:"aaa"

使用:
this.$options.definedabcd
```

### 4.$watch (参,callback,[options])

options 选项:deep		deep:true

```js
options选项:
			deep:true		//监听对象此时有的值的变化.
			immediate:true  //vue实例被加载的一瞬间先来执行此函数,但是第一次刷新的一瞬间无oldVal
```

监听对象此时有的值的变化.

什么时候触发?   观察有没有get 和set

```js
//在体内的 
watch:{
                name:function(){					//1.格式被限制住了
                    console.log("hello world")
                }
            },


//在体外  2.观察后会取消  会自我取消监听
vm.$watch("sex",function(newVal,oldVal){
    console.log("oldVal:"+oldVal+"newVal"+newVal)
})
//3.也可以监听函数!
vm.$watch(function(){
    return this.sex+this.name;
    return this.sex.indexOf("a");
},function(newVal,oldVal){
    console.log("oldVal:"+oldVal+"newVal"+newVal)
})
```

### 5.给一个值添加get和set  

```js
vm.$set(target,propertyNAme/index,value);


vm.$set(vm.gfs,"gf5","aaaa");	//给gfs里面添加gf5,先实例再属性
Vue.$set
```

### 6.删除对象属性

```js
vm.$delete(target,propertyNAme/index);
```

具备响应式

### 7.监听自定义事件

```js
vm.$on(event,function)
vm.$once(event,function)		//只触发一次
vm.$off(event,callback)			//移除自定义事件监听器

ex:
  let vm = new Vue({
            el: "#app",
            data: data,
            watch:{
                name:function(){
                    console.log("hello world");
                    this.$emit("sayhi","lai")	//实例方法里有this,但是template里就没有this              
                }
            },
        })
vm.$on("sayhi",function(name){
	console.log("hi, "+name);
})
```

# 25 vue的实例混合技术

```html
    <div id="app">

    </div>
    <script type="text/javascript">
        //局部注册组件
        let con = Vue.extend({
            template: `<p>{{name}}{{age}}{{sex}}</p>`,
            data() {
                return {
                    name: 'tom',
                    age: 18,
                    sex: 'male'
                }
            }
        })

        new con().$mount("#app")
        // new con()准备了实例
        // .$mount()  准备了挂载,挂载到哪个元素上
    </script>
```

实例化的同时直接进行挂载,但是mount之后没有挂载就挂载不了了,所以使用

```js
document.querySelector.appendChild("#app")		//原生domAPI来进行挂载
```

```js
Vue.nextTick(function(){
    console.log("dom更新了");
})
```

27.00

### vm.$nextTick([callback])

回调的this自动绑定到调用它的实例上

### destory内容还在,指令和事件不能更改

## 25.2vue的混入

### mixin搭积木

```html
 <div id="app"> </div>

    <script type="text/javascript">
      let mixData={									//混合模式的数据,可以实现多个组件用
        data(){
            return {
                message:"hello world"
            }
        }
    }

    new Vue({
        el:"#app",
        data:mixData.data()
        
    })

    let com=Vue.extend({
        mixins:[mixData],							//在此处混合,mixins,是数组的形式
        template:`<p v-on:click="clickfn">{{message}}</p>`,
        methods:{
            clickfn(){
                console.log("hello world");
            }
        }
    })
    new com().$mount("#app");
```

1.有key的话后者覆盖前者

2.一旦有冲突,内部数据比外部数据优先

3.事件的话都会被激活,先外部再内部

4.函数也会被覆盖

5.全局混入的话,风险非常大,等于Object.property

## 25.3 自定义指令

```js
 Vue.directive("yuan",{
        bind(){ //bind被插入的时候      beforeMount
            console.log("bind") 
        },
        inserted(){     //仅保证父节点存在   beforeMount
            console.log("inserted")
        },
        update() {  //beforeUpdate
            console.log("update")
        },
        componentUpdated(el,binding,vnode,oldnode){             //beforeUpdate
            console.log("componentUpdated");
            console.log("el")
            console.log(el)
            console.log("binding")
            console.log(binding)
            console.log("vnode")
            console.log(vnode)
            console.log("oldnode")
            console.log(oldnode)
        }
    })
```

# 26 渲染函数

```js
 <div id="app">

    </div>
    <script type="text/javascript">
        let vm = new Vue({
            el: "#app",
            data: {
                message: "hello"
            }
        })
        let com = Vue.component("test", {
            data() {
                return {
                    msg: "hello world",
                    name: "wanz",
                    sex: "male",
                    toggle:false
                }
            },
            render(createElement) {     	 //渲染函数,增加节点
                return createElement(
                    "p", 	//标签名
                    {
                          class:{ //别的类别,class也可以移出去
                                content:this.toggle,
                                sidebar:!this.toggle
                            },
                            attrs:{	//加属性
                                src:"./hello.html"
                            },
                        	style:{
                                color:"red"
                            }
                    },
                    [
                        this.name,					//文本节点,文本节点的内容
                        createElement("span",this.sex),
                        createElement("span",this.sex),
                        createElement("span",this.sex),
                    ]
                );
            }
        })
        
        
    运行:new com().$mount("#app")    
```

## 26.3 过滤器

管道符,上次命令的输入值变成下次命令的返回值  	|

```js
 <div id="app">
        {{message|twoSlice}}
    </div>
    <script type="text/javascript">
        let vm = new Vue({
            el: "#app",
            data: {
                message: "hello"
            },
            filters:{
                twoSlice(value){
                    return value.slice(0,2);
                }
            }
        })
```

26.4

app.vue

vuecli

# vue-router做todolist

1.新建文件夹,

```
npm init -y
```

```
npm install webpack webpack-cli --save-dev
```

2.引入vue项目

你应该将 `vue-loader` 和 `vue-template-compiler` 一起安装——除非你是使用自行 fork 版本的 Vue 模板编译器的高阶用户：

```bash
npm install -D vue-loader vue-template-compiler	-D
```

3.创建webpack.config.js

```js
let path=require("path");


module.exports={
    mode: 'development' ,
    entry:path.resolve(__dirname,"./src/entry.js" ), //入口
    output:{        //出口
        path:path.resolve(__dirname,"bundle" )
       },
     module:{
         rules:[
             {
                test: /\.vue$/,
                loader: 'vue-loader'
             }
         ]
     }
}
```

一个vue 的module

4.创建src文件夹,在src下创建assets，components，以及app.vue，entry.js

5.要用js文件来导入和引用,

安装rules

```
 postcss-loader@4.1.0
+ style-loader@2.0.0
+ file-loader@6.2.0
+ css-loader@5.0.1
+ autoprefixer@10.2.1
babel-loader@8.2.2
+ @babel/preset-env@7.12.11
+ @babel/polyfill@7.12.1
+ @babel/core@7.12.10xxxxxxxxxx  postcss-loader@4.1.0+ style-loader@2.0.0+ file-loader@6.2.0+ css-loader@5.0.1+ autoprefixer@10.2.1+ @babel/preset-env@7.12.11+ @babel/polyfill@7.12.1+ @babel/core@7.12.10npm install vue-loader babel-loader vue-style-loader css-loader -D
```

6.

```
npm install html-webpack-plugin -D
```

7.node用require,js用import



# 开发去哪儿网

## 3 

## 3.5 计算属性

2021/06/15

### 1.computed

```js
data:{
    firstName:'lai',
    lastName:'e'
}
computed:{
	fullName:function(){
		return this.firstName+' '+this.lastName;
	}
}
```

1.内置缓存，依赖值不改变时不执行。（性能最高）

### 2.监听器watch

```js
watch:{
	firstName:function(){
		return this.firstName+' '+this.lastName;
	},
	lastName:function(){
		return this.firstName+' '+this.lastName;
	}
}
```

### 3.getter和setter

```js
computed:{
	get:function(){
		return this.firstName+' '+this.lastName;
	},
	set:function(value){
		console.log(value);
		var arr=value.split(" ")
		this.firstName=arr[0];
		this.lastName=arr[1];	//vm.fullName("nike Wang") 全部都会发生变化
	}
}
```

1.通过设置一个值去改变全部的值，又引起computed的重新计算。

## 3.6 样式绑定

### 1.使用class改变样式

#### 3.6.1 class的对象绑定

```vue
<li :class="{activated:isActivated}" @click="handleDivClick"></li>
```

```js
data:{
	isActivated:false
},methods:{
	handleDivClick(){
		this.isActivated = !this.isActivated
	}
}
```

```css
.activated:{
    color:red;
}
```

#### 3.6.2 class的数组 

```html
<li :class="[activated]" @click="handleDivClick"></li>
```

```js
data:{
	activated:''
},methods:{
	handleDivClick(){
		this.activated = this.activated === ""?"activated":""
	}
}
```

### 2.使用style改变样式

```html
<div :style="styleObj">		//:style="[styleObj,{fontSize:'20px'}]"
    hello world
</div>

data:{
	styleObj:{
		color:"balck"
	}
}
```

也可以挂载多个对象（数组

## 3.8 key

```html
<div v-for="(item,index) of list" :key="item.id">	
    item.id是后端返回值来的唯一标识，这样可以使性能最高，key就不需要:key="index"降低性能
</div>
```

1.只能使用 push pop shift unshift splice sort reverse去操作数组，仅改变下标时数据变了页面不会跟着变。

2.改变引用地址

<template> 模板占位符（帮助我们包裹元素，并不会放在 界面上）

3.使用vue.set方法

```js
Vue.set( target, propertyName/index, value )
-----------------------------------------------------
ex:
Vue.set(vm.userInfo,"address","beijing")

vm.$set(vm.userInfo,"address","beijing")
```

## 4.4 给组件绑定原生事件

1.在组件中@click绑定的是自定义事件，子组件中绑定的@click是原生事件

2.就想监听原生事件

```html
<child @click.native="handleClick"></child>
```

## 4.5 非父子组件中的传值

父子组件19.3

```js
vue.prototype.$on("change",{
    function(){
        
    }
})
```

4.6 插槽

## 4.7 作用域插槽

```html
<child>
    
</child>
```

4.9 动态组件

```html
<component :is="type"></component>

data:type:child-one
methods 改变data中type的值
```

v-once 放在内存里，性能更高（提高静态内容的展示效率

## 第六章

### 1.环境配置

1.1下载node

​		终端工具

```
node -v  //版本号
npm -v	//安装工具
```

1.2码云

个人主页-private-创建项目 MIT

1.3通过git进行关联		安装好git

终端

```
git --version
```

1.4 git-bash（小型的linux操作系统）

1.右上角-我的主页-ssh公钥，复制代码

2.在git-bash里粘贴代码，将邮箱改成自己的邮箱	回车，生成公钥和私钥

3.git-bash代码里粘贴第二个复制的代码，生成公钥

4.公钥，回到码云，点击ssh公钥界面复制进去

5.将线上的项目克隆到本地，以ssh协议复制后git clone xxx

6.创建vue-cli项目

### 3.单文件组件与路由

### 4.单页面应用，多页面应用

##### 1.多页应用

1.1页面跳转，返回html

- ​	优点：首屏时间快（只经历了一个http请求过程，SEO效果好（搜索引擎可以识别html的
- 缺点：页面切换慢，会有卡顿。

##### 2.单页应用

```html
<router-link to="/list">列表页</router-link>		和a标签差不多
```

vue写的都是单页面，js感知到url变化，将这个清除掉再显示新的组件

2.1页面跳转，JS渲染

- 优点：页面切换快
- 缺点：首屏时间慢，SEO差
- 解决：服务器端渲染

### 5 项目代码初始化

#### 1.在网上下载reset.css

1.1assets-styles-reset.css 基础样式修饰，保证每一个浏览器样式一致。

1.2在main.js里面引入

```
import './assets/styles/reset.css'
```

#### 2.border.css

一像素边框的问题。

2.1 border.css 放在reset.css同一个目录下

（scale的像素实现

#### 3.fastClick

300ms点击延迟问题

1.使用fastclick的库

```
npm install fastclick --save
```

2.在主文件main.js 里面引用

```
import attachFastClick from 'fastclick' //引用该组件的目的是为了解决移动端300ms延迟的问题

attachFastClick.attach(document.body);	//激活组件
```

#### 4.iconfont

图标管理-我的项目-右侧加号创建项目。

5.无用代码删除

lang

## 7.8

1.只有把静态的界面放在static目录下才能去访问。

static-mock-index.json

2..gitgnore  加static/mock  则不会被加到线上的仓库里  **不会被提交到仓库之中**

3.在config-index.js下的-proxyTable

```js
 dev: {
    // Paths webpack-dev-serve 改动配置项文件的时候需重启
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
      '/api': {
        target: 'http://localhost:8080',
        pathRewrite: {
          '^/api': '/static/mock'
        }
      }
    },
```

4.改动配置项文件的时候需要重启服务器

App.vue

```html
<template>
  <div id="app">
    <router-view/>
  </div>
</template>
```

@符号指的是src下的目录

sourceTree切换分支：检出分支

## 8.1

