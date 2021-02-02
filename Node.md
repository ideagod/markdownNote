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

