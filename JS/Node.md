# cmder

## 关于cmder的一些配置

#### 1. 配置环境变量:

在系统属性里面配置环境变量，将`Cmder.exe`所在文件路径添加至`Path`里

![img](https:////upload-images.jianshu.io/upload_images/13479263-87db9162416bf29b.png?imageMogr2/auto-orient/strip|imageView2/2/w/418/format/webp)



![img](https:////upload-images.jianshu.io/upload_images/13479263-e2f1e706bf445886.png?imageMogr2/auto-orient/strip|imageView2/2/w/390/format/webp)



#### 2. 配置右键快捷启动

以管理员身份打开`cmd`，执行以下命令即可，完了以后在任意地方点击右键即可使用cmder

```cpp
// 设置任意地方鼠标右键启动Cmder
Cmder.exe /REGISTER ALL
```

https://www.jianshu.com/p/5b7c985240a7



# node

## 1.创建一个node文件

```
touch demo.js
```

查看demo.js

```
ls demo.js
```

编写demo.js

```
vim demo.js
```

esc键退出,然后shift+:

:wq

运行demo.js

```
node demo.js
```

2.进入环境:输入node,离开环境:control+c

## 2.node核心概念

输入输出I/O

## npm基本指令集:

-S是指模块的版本保存到生产环境依赖中

-D是指模块在开发环境



1.安装install

2.卸载uninstall



3.node模块导入和导出

导入

```
let a=require("./src/index.js")
斜杠就是根目录下
```

导出

```
module.exports={add}
```

node的模块系统

```js
//demo.js
let {add}=require("./src/math")	//一定要加盘符(否则会在本地里面找)
console.log(add(1,2))

//math.js
function add(x,y){
    return x+y;
}
module.exports={add}
```

导入对象引用对象,导入函数引用函数.

# 

# express

## 中间件（像proxy代理） app.use

数据的流入、流出 =>中间件=>最终业务
用处：

先做一些基本处理，发送数据前做一些基本配置。

静态资源配置工作

express内容分类

```
express()
	.json
	.static
	.Router
	.irlencoded
Application
	Properites
	Events
	Methods
	
```

# Express

1.安装

## 2.目录结构

1.通过express生成器创建的应用都有如下目录结构

```
.
├── app.js				//基本界面
├── bin				    //运行的时候就是运行他
│   └── www
├── package.json		//版本号
├── public				//没啥用的
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes				//路由，后端的方法
│   ├── index.js
│   └── users.js
└── views				//界面错误警告，也没啥用
    ├── error.pug
    ├── index.pug
    └── layout.pug

7 directories, 9 files
```

## 3.路由

### 1.基本路由

1.*路由*是指确定应用程序如何响应客户端对特定端点的请求，该特定端点是URI（或路径）和特定的HTTP请求方法（GET，POST等）。

```js
//实例express.小写的HTTP请求方法(服务器上的路径，执行的函数(req,res,next中间件功能))
```

HTTP请求方法

1.get方法

2.post请求

3.put请求

4.delete方法

### 2.app.use中间件

这里说的app，是指express对象

var express = require(‘express’);

var app = express();

其中，app.use是express“调用中间件的方法”。所谓“中间件（middleware），就是处理HTTP请求的函数，用来完成各种特定的任务，比如检查用户是否登录、分析数据、以及其他在需要最终将数据发送给用户之前完成的任务。

app.use() 里面使用的参数，主要是函数。但这个使用，并不是函数调用，而是使能的意思。当用户在浏览器发出请求的时候，这部分函数才会启用，进行过滤、处理。

Express是一个路由和中间件Web框架，其自身的功能很少：Express应用程序本质上是一系列中间件函数调用。

应用级中间件、路由级、错误处理、内置中间件、第三方中间件。

### 3.应用级中间件

### 4.路由级中间件

路由级中间件与应用程序级中间件的工作方式相同，只不过他绑定到实例express.Router()

```js
var router=express.Router()
```

要跳过路由器的其余中间件功能，请调用`next('router')` 将控制权转回路由器实例。

### 5.路由

顶级`express`对象具有一个[Router（）](https://www.expressjs.com.cn/4x/api.html#express.router)方法，该方法创建一个新`router`对象。

创建了一个路由器的对象，可以添加中间件和HTTP方法路由（如`get`，`put`，`post`，等）

```js
// will handle any request that ends in /events
// depends on where the router is "use()'d"
router.get('/events', function (req, res, next) {
  // ..
})
```

#### res.json([body])

发送JSON响应。此方法发送响应（具有正确的内容类型），该响应是使用[JSON.stringify（）](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)转换为JSON字符串的参数。

参数为任何类型，转值为json。

```
res.json(null)
res.json({ user: 'tobi' })
res.status(500).json({ error: 'message' })
```

## 4.express托管静态文件

将 `public` 目录下的图片、CSS 文件、JavaScript 文件对外开放访问了：

```
app.use(express.static('public'))
```

现在，你就可以访问 `public` 目录中的所有文件了，访问静态资源文件时，`express.static` 中间件函数会根据目录的添加顺序查找所需的文件。

然而，您提供给`express.static`函数的路径是相对于您启动`node`过程的目录的。如果从另一个目录运行express app，则使用要提供服务的目录的绝对路径更为安全：

```javascript
app.use('/static', express.static(path.join(__dirname, 'public')))
```

## API

### cookie-parser

解析`Cookie`标头，并填充`req.cookies`一个由cookie名称作为键的对象。（可选）您可以通过传递一个`secret`字符串来启用签名cookie支持，该 字符串会分配给`req.secret`它，以便其他中间件可以使用它。

The middleware will parse the `Cookie` header on the request and expose the cookie data as the property `req.cookies` and, if a `secret` was provided, as the property `req.signedCookies`. These properties are name value pairs of the cookie name to cookie value.

```js
var express = require('express')
var cookieParser = require('cookie-parser')

var app = express()
app.use(cookieParser())

app.get('/', function (req, res) {
  // Cookies that have not been signed
  console.log('Cookies: ', req.cookies)

  // Cookies that have been signed
  console.log('Signed Cookies: ', req.signedCookies)
})
------------------
 //cookie不安全,可以篡改 所以用session
          req.session.userId=doc.userId;
          req.session.grade=Number.parseInt(doc.grade);
          req.session.userName=doc.userName ||'';
          // res.cookie("userId",doc.userId,{
          //   path:'/',
          //   maxAge:1000*60*60*24
          // });
          // res.cookie("grade",parseInt(doc.grade),{
          //   path:'/',
          //   maxAge:1000*60*60*24
          // });
          // let userName = doc.userName ? doc.userName :'';
          // res.cookie("userName",userName,{
          //   path:'/',
          //   maxAge:1000*60*60*24
          // });

//App.js
app.use(cookieParser()); 
app.use(session({
  secret: 'bgwd',//  secret:用于对sessionID的cookie进行签名，可以是一个string(一个secret)或者数组(多个secret)。如果指定了一个数组那么只会用 第一个元素对sessionID的cookie进行签名，其他的用于验证请求中的签名。
  resave:false,//强制session保存到session store中。
  saveUninitialized:true,//强制没有“初始化”的session保存到storage中
    /*刚被创建没有被修改,如果是要实现登陆的session那么最好设置为false(reducing server storage usage, or complying with laws that require permission before setting a cookie) 而且设置为false还有一个好处，当客户端没有session的情况下并行发送多个请求时。默认是true,但是不建议使用默认值。*/
  cookie: {maxAge: 1000*60*60}//req.session.cookie.maxAge返回这个cookie剩余的毫秒数，当maxAge设置为60000，也就是一分钟
}));
```

### 使用session方法

实现过程

```js
// app.js// 引入express-sessionvar session = require('express-session')// 为应用绑定session中间件app.use(session({  name: 'session-id',  secret: '12345-67890',  saveUninitialized: false,  resave: false}))
```

查看结果

```js
router.get('/session/first', (req, res, next) => { let s = req.session console.log(s) res.send(s)})
```

