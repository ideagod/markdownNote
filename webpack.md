# 搭建webpack

#### 默认下载完node.js

**在终端：**

- npm init -y

- 根目录创建dist文件夹（存放打包后的代码），src文件夹（入口），在src里面创建index.html和main.js

- 根目录下 安装webpack和webpack-cli，命令：npm i webpack webpack-cli -D                  (-D为局部安装),但是为了防止版本冲突问题，推荐不用上述安装，而是指定版本安装，即：npm i webpack@4.43.0 -D，和：   npm i webpack-cli@3.3.12 -D

- 根目录下创建webpack.config.js,如下：

  ```js
  //向外暴露一个打包的配置文件
  module.exports={
  	mode:'development'  /* 这里配置development开发模式，方便维护，mode的默认配置就是production，就是说如果你不配置mode默认值是生产模式（压缩后的）*/
  }
  ```

- 最后在根目录下   执行命令：webpack，然后可以在index.html的body标签结束前引入查看效果



**但是，我们每一次更新代码，都要打包一次略显麻烦，所以引入 webpack-dev-server**

- 跟目录下执行命令： npm i webpack-dev-server@3.11.0 -D  
- 在package.json文件下找到"scripts"，添加：*<u>"dev":"webpack-dev-server"</u>*（这样运行就能直接npm run dev）

​	**再次配置一下webpack.config.js**

```js
const path = require('path');
const HtmlWebPackPlugin = require('html-webpack-plugin');
//创建一个插件的实例对象
const htmlPlugin = new HtmlWebPackPlugin({
    template: path.join(_dirname,'./src/index.html'),
    filename: 'index.html'
})
module.exports = {
    'mode': 'development',
    plugins:[
        htmlPlugin
    ]
}
```

然后安装 ：npm i html-webpack-plugin -D

最后运行npm run dev，这样就简单可以实时保存更新打包内容了

# Node.js

模拟服务端，用来跑js的后台代码，前端向nodejs发送请求，然后nodejs根据写的函数向数据库发送请求，然后返回前端。响应请求、查询、返回。

根据网上教程配置的

![0265678d77ba55f3083a647a5b4fe94](C:\Users\lenovo\AppData\Local\Temp\WeChat Files\0265678d77ba55f3083a647a5b4fe94.png)

# 1.webpack使用

## 1.webpack配置

1.创建文件夹,

2.打开cmd,输入npm init

```cmd
npm init
```

3.生成json文件

```json
{
  "name": "2021webpacklearing",
  "version": "1.0.0",
  "description": "",
  "main": "index.js", //相当于入口文件
  "scripts": {	//脚本,命令,通过某些批处理的代码实现命令的映射
    "test": "echo \"Error: no test specified\" && exit 1"	//npm run test相当于echo \"Error: no test specified\" && exit 1
  },
  "author": "",		//作者
  "license": "ISC"	//许可证,开源的话就是MIT
}

```

4.安装webpack和webpack-cli  局部安装

```cmd
npm install webpack --save-dev		//我只开发保存,发布不保存
```

```cmd
npm install webpack-cli --save-dev
```

查看版本号

```cmd
npx webpack -v
```

```cmd
npx webpack-cli -v
```

5.删除

```
npm uninstall webpack -g
```

当文件夹中出现node_modules时则 配置完成

## 2.第一个webpack

1.在主文件夹下创建index.html

2.在主文件夹下创建src/js文件夹,再在下创建index.js

3.在index.html里加入

```html
 <script src="src/js/index.js"></script>
```

4.当我在打包的时候遇到问题,原本是npx webpack js/index.js,但是出错

```cmd
[webpack-cli] Unknown command 'js/index.js'
[webpack-cli] Run 'webpack --help' to see available commands and options
```

发现是版本问题.

webpack4.x是以项目根目录下的`'./src/index.js'`作为入口,所以我们需**将index`.js`移动到`'./src'`**

现在如果我们再次执行

```
webpack index.js bundle.js
```

会提示can.t resolve相关的错误。

原因是，webpack4.x的打包已经不能用`webpack 文件a 文件b `的方式，而是直接运行`webpack --mode development`或者`webpack --mode production`，这样便会默认进行打包，入口文件是`'./src/index.js'`，输出路径是`'./dist/main.js'`，其中src目录即index.js文件需要手动创建，而dist目录及main.js会自动生成。 
因此我们不再按`webpack 文件a 文件b`的方式运行webpack指令，而是直接运行

```
webpack --mode development
```

或者

```
webpack --mode production。
```

这样便能够实现将`'./src/index.js'`打包成`'./dist/main.js'`。 
不过每次都要输入这个命令，非常麻烦，我们在`package.json`中`scripts`中加入两个成员：

```
"dev":"webpack --mode development",
 "build":"webpack --mode production"
```

以后我们只需要在命令行执行`npm run dev`便相当于执行`webpack --mode development`，执行`npm run build`便相当于执行`webpack --mode production`。 
我们在根目录执行：

```
npm run dev
```

可以看到根目录下生成了dist目录，并且dist目录下生成了main.js文件，main.js文件已经打包了src目录下index.js文件的代码。

5.在index.html里使用

```js
    <script src="dist/main.js"></script>
```

内容就出来了

6.更新

```
npm run dev
```

7.根目录上创建webpack.config.js

配置文件,每次运行时会优先寻找

```js
let path=require("path");//node  导入path模块

//手动定义入口
module.exports={
    entry:"./src/index.js" , //入口
    output:{        //出口
        filename:"end.js",                //目录里新建一个end.js
        path:path.resolve(__dirname,"bundle")   //模块加载成功,在当前文件下创建一个bunble目录  node,path给我们提供的一个变量,指的是当前的路径webpack.config.js
    },
        mode: 'development' // 设置mode,4.x后需要设置,模式是生产环境production和development开发环境
}
```

则在index.html里使用

```html
<script src="bundle/end.js"></script>	    
<!--出口文件-->
```

最后在cmd中使用即可:

```cmd
npx webpack
```

##### chocolatey软件 更新node

# 2.loader

## 1.file-loader引入文件

1.index.js里

```
import img from "../image/81.jpg"
```

2.在配置里写

```js
 module:{
        rules:[
            {
                test:/\.(jpg|png|gif|webp|jpeg)$/,      //正则匹配后缀
                use:{
                    loader:'file-loader',
                    options:{   //选项
                        name:'[name]_[hash].[ext]',  //指定具体名称 可选择的名字,哈希,和后缀
                        outputPath:"images",     //打包后存放的路径
                    }
                }
            }
        ]
    }
```

3.安装file-loader,在cmd里输入

```
npm install file-loader --save-dev
```

fileloader如何运行:

1.移到bundle后目录的相对位置

2.将新的地址访问值返回给我们import参数

```
   name:'[name]_[hash].[ext]',  //指定具体名称 可选择的名字,哈希,和后缀
   
   name的配置
   [ext]	String   资源扩展名
   [name]
   [path]				资源相对于context的路径
   [hash]				内容的哈希值
   [N]		Number		当前文件名按照查询参数regExp 匹配后获得到第N个匹配结果
```

## 2.url-loader

将文件挪成链接,su4,但是文件太大就不适合用url-loader

1.安装

打开cmder,输入

```
npm install url-loader --save-dev
```

2.配置文件

```js
  module:{
        rules:[
            {
                test:/\.(jpg|png|gif|webp|jpeg)$/,      //正则匹配后缀
                use:{
                    loader:'url-loader',
                    options:{   //选项
                        name:'[name]_[hash].[ext]',  //指定具体名称 可选择的名字,哈希,和后缀
                        outputPath:"images",     //打包后存放的路径
                            limit:2048              //小于这个文件2kb则用此编码
                    }
                }
            }
        ]
    }
```

3.运行

```cmd
npm run bundle
```

4.报错,只打包了end.js

```
Z:1 Failed to load resource: net::ERR_FILE_NOT_FOUND
```

5.将eleimg.js的"bundle"去掉,再次运行成功显示,但是bundle没有图片,说明index.js的import img from "../img/2.jpg" 的img已经是文件经过编码之后的地址

如何将html也打包进去

## 3.css-loader

1.在src/css文件夹内创建index.css

```css
@import "./slider.css";	/* 将slider引入,可以互相导入 */
p{
    text-align: center;
    background-color: red;
}
```

2.在index.js里引入css

```js
import "../css/index.css"
```

3.安装css-loader和style-loader

```cmd
npm install style-loader css-loader --save-dev
```

安装完成

```
+ css-loader@5.0.1
+ style-loader@2.0.0
added 19 packages from 14 contributors in 2.393s
```

4.config配置,在rules里添加

```js
 {
                test:/\.css$/,      //正则匹配后缀
                use:[
                    "style-loader",          //从下往上执行,所以css-loader一定要放在style-loader下面
                   "css-loader"
                ]
            }
```

## 4.scss(sass)-loader

sass不能使用css

1.安装

```cmd
npm install sass-loader node-sass --save-dev
```

Ruby模块,现在移到了node上,sass-loader是用来解析的

2.config配置,在rules里添加

```js
 {
                test:/\.scss$/,      //正则匹配后缀
                use:[
                    "style-loader",     
                   "css-loader",
                   "sass-loader"
                    "node-sass",          
                 
                ]
            }
```

3.在src/css文件夹内编写

```css
body{
    div{
        text-align:left;
    }
    .side{
        color:green;
    }
}
```

4.在index.js里引入scss

```js
import "../css/index.scss"
```

5.运行

## 5.postcss-loader

不用自己写css要怎么兼容的打包

1.安装

```cmd
npm install postcss-loader --save-dev
```

2.安装

```cmd
npm install autoprefixer --save-dev
```

3.package.json下

```json
  "devDependencies": {    
    "autoprefixer": "^10.2.1",
    "css-loader": "^5.0.1",
    "file-loader": "^6.2.0",
    "node-sass": "^5.0.0",
    "postcss-loader": "^4.1.0",
    "sass-loader": "^10.1.1",
    "style-loader": "^2.0.0",
    "url-loader": "^4.1.1",
    "webpack": "^5.12.2",
    "webpack-cli": "^4.3.1"
  }
```

都是开发的依赖,打包上去就会没有

 插件的作者怎么思考,postcss-loader是新建一个插件

4.单独配置一个文件引用插件,所以在主文件下创建postcss.config.js

```js
module.exports={
    plugins:[
        require("autoprefixer")
    ]
}
```

5. webpack.config.js

```js
{
                test:/\.scss$/,      
                use:[
                    "style-loader",       
                    {
                        loader:"css-loader",
                        options:{
                            importLoaders:2	//看下面有多少个loader,默认一个
                        }
                    },
                    "sass-loader",
                    "postcss-loader"
                ]
            }
```

6.没有告诉浏览器为谁做兼容

要在package.json加上

### 浏览器兼容

```json
 "browserslist":[	//浏览器列表	//告诉浏览器列表加属性,可以上github上面查
    "ie 6-8",
    ">0.5%",
    "last 10 versions"
  ]
```

## 6.icon图标使用

1.下载iconfont,下载到本地

2.解压,删除.json,.js,.html,demo

3.放入src/font下

4.在index.scss内导入

```css
@import "../font/iconfont.css";
```

5.在引用的地方写

```css
    elSilder.innerHTML="<i class='iconfont icon-hongbao '  style='font-size:25px'></i>网页侧边fsdg内容";
```

6.config下

```js
  {
                test:/\.(eot|svg|ttf|woff|woff2)$/,     
                use:{
                    loader:'file-loader',
                    options:{
                        name:'[name].[ext]',  
                        outputPath:"font",    
                    }
                }
            }

```

# 3.plugins插件

帮助我们用webpack进行打包的某个时间点做一件事,像是钩子函数

## 1.html-webpack-plugin

  生成模板文件

1.安装

```cmd
npm install html-webpack-plugin --save-dev
```

2.git管理

cmd中写入

```
git init
```

生成.gitignore文件,

node_modules和bundle

3.引用.在config里写入

```js
let htmlWebpackPlugin=require("html-webpack-plugin");
```

4.在module下同级

```js
  plugins:[
        new htmlWebpackPlugin()
    ]
```

5.打包,会在bundle里自动创建index.html

6.在src里创建一个html文件夹来存储html模板,创建template.html

```html
<!-- head -->
<title>用户自定义的模板文件</title>
<!-- body -->
 <p>这是模板文件自带的一个基本元素</p>
```

7.config设置

```js
   plugins:[
        new htmlWebpackPlugin({
            template:"./src/html/template.html"
        })
    ]
```

这样之前的就不用自己加路径了,外面的html也可以删掉了

## 

名字一样覆盖,名字不一样增加,则我们想打包时所有都清空掉

## 2.clean-webpack-plugin

清空

可以删除bundle以前的东西

1.安装

```
npm install clean-webpack-plugin --save-dev
```

2.config引入 **重要**

```
let {CleanWebpackPlugin}=require("clean-webpack-plugin")	//第一个单词一定要大写,并且是输出了一个有名函数,得用解构赋值的方法
```

3.config

```js
   plugins:[
        new htmlWebpackPlugin({
            template:"./src/html/template.html"
        })
    ],
    new CleanWebpackPlugin()	//一定要大写!
```

4.运行,如果出错了,则可能是线程占用了

## 3.注意事项

1.

```
Error: EPERM: operation not permitted, lstat 'D:\程序\2021webpackLearing\bundle\font\iconfont.eot'
    at Object.lstatSync (fs.js:1046:3)	
    说字体文件占线程了
```

2.config

```js
 entry:{
        firstIn:"./src/js/index.js" 
    }, //入口直接加入名字,到时候bundle会变成firstIn
```

也可以多个,则会导出不同的js.

3.

```js
 output:{        //出口
        publicPath:"www.cdn.com/",  //引用会加公共前缀,输出目录
        path:path.resolve(__dirname,"bundle")   //模块加载成功,在当前文件下创建一个;bunble目录
    },
```

4.默认情况下会加上config

## 4.sourceMap

(默认)代码出错了是对应哪个源文件的错误

在webpack.config.js的定义处填写

```js
devtool:'none'				//不显示在个文件出错
devtool:'source-map'		//默认保存,source-map就是生成.map文件,想只看自己的js文件里的出错的话
devtool:'cheap-source-map'		//自己代码错了才映射
devtool:'inline-source-map'      //把map文件放进index.html里面来(内嵌)
devtool:'module-source-map'//把模块里面的文件处理出来 
devtool:'cheap-module-eval-source-map' 
```

# 4.热更新技术

项目不会因为修改而长时间的宕机,过渡技术.在开发环境下的.

## 1.文件监听实现自动打包

1.在package.json里的script里添加

```js
"watch" :"wabpack --watch"
```

2.cmd中输入

```cmd
npm run watch
```

3.停止监听在cmd中control+c即可

## 2.webpack-dev-server

1.安装

```cmd
npm install webpack-dev-server --save-dev
```

打包的工具不需要,所以放在-dev中

2.在webpack.config.js里添加定义

```js
  devServer:{
      //我们自己在用webpack开发的时候,搭建一个临时服务器
       contentBase:"./bundle",	
       open:true, //临时服务器的资源根目录  运行了webpack-dev-server会自动打开浏览器并且进入该服务器地址
       port:9527	//可以手动定义端口,默认8080
    },
```

3.在package.json里面的scripts对象里添加定义

```js
    "start":"webpack-dev-server"
```

4.运行

```cmd
npm run start
```

开启sever后,bundle文件下什么都没有了.全部都放在了内存里面.更快.可以直接从内存里访问.

### webpack 4.0.0带来的问题

错误的原因是webpack与webpack-dev-server的版本不兼容,

```
Error: Cannot find module 'webpack-cli/bin/config-yargs'
```

现在的(2021/1/13)版本号为

```
    "webpack": "^5.12.2",
    "webpack-cli": "^4.3.1",
    "webpack-dev-server": "^3.11.1"
    node : v14.13.1
    npm  : 6.14.8
```

解决方案:

降低`webpack-cli`的版本为 `"^3.3.12"`即可

```
npm install webpack-cli@3.3.12 --save-dev
```

参考:

https://blog.csdn.net/qq_36446728/article/details/109108533

实时预览,本地服务,节省时间.

但是整个网页都会被重新渲染

## 3.热更新

1.在config.js里引入webpack

```js
let webpack=require("webpack");
```

2.插件里使用

```js
  new webpack.HotModuleReplacementPlugin()
```

3.在devServer里跟上参数

```js
 devServer:{
             hot:true,	//boolean 开启热模块replacement
             hotOnly:true	//热模块更新无生效,浏览器不会刷新
    },
```

41

28

### (无处理完毕)chrome报警告:

 DevTools failed to loadSourceMap: Could not load content

报错原因:

在其它浏览器可以访问实时页面，而chrome不行,建议使用chrome无痕模式前端开发

https://blog.csdn.net/qq_36413371/article/details/106684032

js执行结果是累加的.

# 5.ES6到ES5的转义

## babel-loader

将babel和webpack打通的工具而已  需要babelcore核心库

1.安装 babel核心库

```
npm install --save-dev babel-loader @babel/core
```

2.安装

```
npm install @babel/preset-env --save-dev
```

3.在webpack.config.js里新建module下的rule

```js
 {
                test:/\.js$/,   
                exclude:"/node_modules/",     //存放目录,排除不是es6的东西,不做语法转换的目录
                loader:"babel-loader",
                options:{
                    presets:[
                        "@babel/preset-env"
                    ]
                }
            },
```

4.运行

```
npm run bundle
```

可以看见let变成了var,但是还是有promise函数,没有完全转化完成,.因此需要另一个工具

## polyfill 

1.安装

```
npm i @babel/polyfill -D
```

2.在index.js里导入文件

```
import "@babel/polyfill"
```

3.编译

```
npm run bundle
```

缺点:文件太大了,所以可以按需翻译

4.按需翻译步骤

在webpack.config.js里新建module下的rule,在option里

```js
options:{
                    presets:[
                        [   //用到模块的转义
                            "@babel/preset-env",{   //写对应的参数
                                useBuiltIns:"usage"
                            }
                        ]
                    ]
                }
```

可以选择啥版本,第二个参数是目标的浏览器

```js
"@babel/preset-env",{   
                                useBuiltIns:"usage",
    				//最终写出来的项目想运行的浏览器及以上
                                targets:{
                                    chrome:"58",
                                    edge:"17",
                                    safari:"11.1"
                                }
                            }
```

## runtime实时翻译

1.安装

```
npm install @babel/plugin-transform-runtime -D
```

2.安装

```
npm install @babel/runtime -D
```

3.安装

```
npm install @babel/runtime-corejs2 -D
```

4.configjs里的loader为"babel-loader"后的options改为

```js
options:{
                    "plugins":[
                        [   
                            "@babel/plugin-transform-runtime",
                            {
                                "absoluteRuntime":false,
                                "corejs":2,
                                "helpers":true,
                                "regenerator":true,
                                "useESMoudules":false
                            }
                        ]
                    ]
                }
```

5.运行一下

```cmd
npm run bundle
```

结果:

会看见promise变成了

```js
_babel_runtime_corejs2_core_js_promise__WEBPACK_IMPORTED_MODULE_0___default.a
```

原来全局注入,现在避免全局变量污染

好处:

想要开发相应组件的时候,让我们顺利的使用原本的原型而不是转义插件

很少使用,主要就是为了兼容而已.

另一种

3.在webpack.config.js里新建module下的rule

```js
 {
                test:/\.js$/,   
                exclude:"/node_modules/",     
                loader:"babel-loader",
             
            },
```

4.创建.babelrc

```
{

 "plugins":[
                        [   
                            "@babel/plugin-transform-runtime",
                            {
                                "absoluteRuntime":false,
                                "corejs":2,
                                "helpers":true,
                                "regenerator":true,
                                "useESMoudules":false
                            }
                        ]
                    ]
   }
```



# 6.tree sharking

不使用的不打包

1.config.js里entry同级设置

```js
 optimization:{
        usedExports:true
    },
```

2.运行

```
npm run bundle
```

3.多了一行标注,开发模式下

```
/*! exports used: add, minus */
```

4.要改为生产模式下的话

```
    devtool:'cheap-module-source-map',
      mode:"production",
```

假如在index.js里面加多一行import "@babel/polyfill"

导入这种库导出的东西把他给忽略掉,没办法导出的,只能忽略

避免这种问题

1.在package.json里加入配置

```json
"sideEffects":false //有导出打包出来,没有导出就忽略掉
"sideEffects":[
    "@babel/polyfill",	//排除babel的polyfill
    "*.css"				//排除所有导入的css文件
] 
```



​    







