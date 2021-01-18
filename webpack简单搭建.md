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