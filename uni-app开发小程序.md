

## 0.uni-app运行微信小程序项目

```
运行-小程序-微信
```

如果工具的服务端口关闭，则去微信-设置-安全设置打开

![image-20210407121229528](C:\Users\breo\AppData\Roaming\Typora\typora-user-images\image-20210407121229528.png)

运行后uni-app的项目会自动转为wxss，贼牛

## 1.上手

1.1通过HbuilderX 可视化界面

1.2创建uni-app，可以直接在运行中 运行到小程序模拟器

2.在HBuilderX中顶部菜单依次点击 "发行" => "小程序-百度"，输入小程序名称和appid点击发行即可在 `/unpackage/dist/build/mp-baidu` 生成百度小程序项目代码。

##### 3.除了hbuilderx，在vue-cli中也可以创建项目

1.全局安装vue-cli

2.创建uni-app 	有分正式版和alpha版

```
vue create -p dcloudio/uni-preset-vue my-project
```

3.[运行、发布uni-app](https://uniapp.dcloud.io/quickstart-cli?id=运行、发布uni-app)

```shell
npm run dev:%PLATFORM%
npm run build:%PLATFORM%
```

## 2.目录结构

### 1.目录及文件

```

┌─uniCloud              云空间目录，阿里云为uniCloud-aliyun,腾讯云为uniCloud-tcb（详见uniCloud）
│─components            符合vue组件规范的uni-app组件目录
│  └─comp-a.vue         可复用的a组件
├─hybrid                App端存放本地html文件的目录，详见
├─platforms             存放各平台专用页面的目录，详见
├─pages                 业务页面文件存放的目录
│  ├─index
│  │  └─index.vue       index页面
│  └─list
│     └─list.vue        list页面
├─static                存放应用引用的本地静态资源（如图片、视频等）的目录，注意：静态资源只能存放于此
├─uni_modules           存放[uni_module](/uni_modules)规范的插件。
├─wxcomponents          存放小程序组件的目录，详见
├─main.js               Vue初始化入口文件
├─App.vue               应用配置，用来配置App全局样式以及监听 应用生命周期
├─manifest.json         配置应用名称、appid、logo、版本等打包信息，详见
└─pages.json            配置页面路由、导航条、选项卡等页面类信息，详见
```

1.1static能被完整引用，别的按需。而且static不会被编译（js文件，不能将ES6放进去。

1.2 css等要放在common下面。

### 2.静态资源的引入

1.图片

```html
<!-- 绝对路径，/static指根目录下的static目录，在cli项目中/static指src目录下的static目录 -->
<image class="logo" src="/static/logo.png"></image>
<image class="logo" src="@/static/logo.png"></image>
<!-- 相对路径 -->
<image class="logo" src="../../static/logo.png"></image>
```

2.js

 绝对路径，@指向项目根目录，在cli项目中@指向src目录

```js
// 绝对路径，@指向项目根目录，在cli项目中@指向src目录
import add from '@/common/add.js'
// 相对路径
import add from '../../common/add.js'
```

### 3.路由

`uni-app`页面路由为框架统一管理，开发者需要在[pages.json](https://uniapp.dcloud.io/collocation/pages?id=pages)里配置每个路由页面的路径及页面样式。类似小程序在app.json中配置页面路由一样。所以 `uni-app` 的路由用法与 `Vue Router` 不同，

3.1 路由跳转

`uni-app` 有两种页面路由跳转方式：使用[navigator](https://uniapp.dcloud.io/component/navigator)组件跳转、调用[API](https://uniapp.dcloud.io/api/router)跳转。

3.2 页面栈

框架以栈的形式管理当前所有页面， 当发生路由切换的时候，页面栈的表现如下：

| 路由方式   | 页面栈表现                        | 触发时机                                                     |
| ---------- | --------------------------------- | ------------------------------------------------------------ |
| 初始化     | 新页面入栈                        | uni-app 打开的第一个页面                                     |
| 打开新页面 | 新页面入栈                        | 调用 API  [uni.navigateTo](https://uniapp.dcloud.io/api/router?id=navigateto) 、使用组件  [](https://uniapp.dcloud.io/component/navigator?id=navigator)uni.navigateTo |
| 页面重定向 | 当前页面出栈，新页面入栈          | 调用 API  [uni.redirectTo](https://uniapp.dcloud.io/api/router?id=redirectto) 、使用组件 [](https://uniapp.dcloud.io/component/navigator?id=navigator) |
| 页面返回   | 页面不断出栈，直到目标返回页      | 调用 API  [uni.navigateBack](https://uniapp.dcloud.io/api/router?id=navigateback)  、使用组件 [](https://uniapp.dcloud.io/component/navigator?id=navigator) 、用户按左上角返回按钮、安卓用户点击物理back按键 |
| Tab 切换   | 页面全部出栈，只留下新的 Tab 页面 | 调用 API  [uni.switchTab](https://uniapp.dcloud.io/api/router?id=switchtab) 、使用组件 [](https://uniapp.dcloud.io/component/navigator?id=navigator) 、用户切换 Tab |
| 重加载     | 页面全部出栈，只留下新的页面      | 调用 API  [uni.reLaunch](https://uniapp.dcloud.io/api/router?id=relaunch) 、使用组件  [](https://uniapp.dcloud.io/component/navigator?id=navigator) |

都是

```html
<navigator open-type=""/>
```

### 4.跳转界面

#### uni.navigateTo(object）

保留当前页面，跳转到应用内的某个页面，使用`uni.navigateBack`可以返回到原页面。

```js
//跳到另外的小程序
	toApplet = 'wx4e38f992ec83f1b9'			
			if(toApplet){
					uni.navigateToMiniProgram({
						appId: toApplet,
						path: path || '',
						success(res) {
							// 打开成功
						}
					})		
	
//跳到另外的界面
	urls = '/pages/coupon/couponCenter'			
			if(urls){
					uni.navigateTo({
					    url: urls
					});	
```

4.2传递参数

```js
//在起始页面跳转到test.vue页面并传递参数
uni.navigateTo({
    url: 'test?id=1&name=uniapp'
});

// 在test.vue页面接受参数
export default {
    onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
        console.log(option.id); //打印出上个页面传递的参数。
        console.log(option.name); //打印出上个页面传递的参数。
    }
}
```

另外参数中出现空格等特殊字符时需要对参数进行编码，如下为使用`encodeURIComponent`对参数进行编码的示例。

```html
<navigator :url="'/pages/test/test?item='+ encodeURIComponent(JSON.stringify(item))"></navigator>
```

```js
// 在test.vue页面接受参数
onLoad: function (option) {
    const item = JSON.parse(decodeURIComponent(option.item));
}
```

#### uni.redirectTo(object)

关闭当前页面，跳转到应用内的某个页面。

```js
uni.redirectTo({
    url: 'test?id=1'
});
```

#### uni.navigateBack(object)

用 navigateTo 跳转时，会加入堆栈，除非使用redirectTo

```js
// 此处是A页面
uni.navigateTo({
    url: 'B?id=1'
});

// 此处是B页面
uni.navigateTo({
    url: 'C?id=1'
});

// 在C页面内 navigateBack，将返回A页面
uni.navigateBack({
    delta: 2		//返回页面数，返多回主页
});
```

#### 

## 3.样式

### 1样式导入

```css
   @import "../../common/uni.css";
```

### 2内联样式

### 3.block标签

```html
<template>
    <view>
        <template v-if="test">
            <view>test 为 true 时显示</view>
        </template>
        <template v-else>
            <view>test 为 false 时显示</view>
        </template>
    </view>
</template>

block
<template>
    <view>
        <block v-for="(item,index) in list" :key="index">
            <view>{{item}} - {{index}}</view>
        </block>
    </view>
</template>

更推荐template
```

### 4.在 `pages.json` 对应页面的 style -> usingComponents 引入组件：

```json
{
    "pages": [{
        "path": "index/index",
        "style": {
            // #ifdef APP-PLUS || H5 || MP-WEIXIN || MP-QQ
            "usingComponents": {
                "custom": "/wxcomponents/custom/index"
            },
            // #endif
            // #ifdef MP-BAIDU
            "usingComponents": {
                "custom": "/swancomponents/custom/index"
            },
            // #endif
            // #ifdef MP-ALIPAY
            "usingComponents": {
                "custom": "/mycomponents/custom/index"
            },
            // #endif
            "navigationBarTitleText": "uni-app"
        }
    }]
}
```

然后在界面中使用

```html
<!-- 页面模板 (index.vue) -->
<view>
    <!-- 在页面中对自定义组件进行引用 -->
    <custom name="uni-app"></custom>
</view>
```

## 4.API

uni.getRecorderManager()

获取全局唯一的录音管理器recorderManager。

可以先看权限。

https://uniapp.dcloud.io/api/media/record-manager

```html
<template>
    <view>
        <button @tap="startRecord">开始录音</button>
        <button @tap="endRecord">停止录音</button>
        <button @tap="playVoice">播放录音</button>
    </view>
</template>
```

```js
const recorderManager = uni.getRecorderManager();			//获取录音
const innerAudioContext = uni.createInnerAudioContext();

innerAudioContext.autoplay = true;	

export default {
    data() {
        return {
            text: 'uni-app',
            voicePath: ''		//声音路径
        }
    },
    onLoad() {	//都在onload上
        let self = this;
        recorderManager.onStop(function (res) {
            console.log('recorder stop' + JSON.stringify(res));
            self.voicePath = res.tempFilePath;
        });
    },
    methods: {	//tap的点击事件在方法里面
        startRecord() {					//开始录音
            console.log('开始录音');
            recorderManager.start();
        },
        endRecord() {					//录音结束
            console.log('录音结束');
            recorderManager.stop();
        },
        playVoice() {
            console.log('播放录音');

            if (this.voicePath) {
                innerAudioContext.src = this.voicePath;
                innerAudioContext.play();
            }
        }
    }
}
```

2021-04-12

查了很久uni-app有没有长按触发事件，只看见longpress，但是longpress也不需要长按。最后看见了bindtap事件（微信小程序的）

而且longpress和longtab一样，更建议用longpress，html5和小程序端只对longpress有响应，longtab是支付宝的

在想是否可以用户点击后直接开始录制，不选择长按，但是用户情况下应该都是要长摁。uni-app可以搭载微信小程序吗

A：搜索了uniapp自定义长按事件，搜到了touchstart

```html
bindlongtap='touchdown'  bindtouchend="touchup" 
```

```
<view   @touchmove="handletouchmove" @touchstart="handletouchstart" @touchend="handletouchend" >  
</view>  

             handletouchstart(e) {  
                this.timeOutEvent = setTimeout(() => {  
                    this.onLongPress(e)  
                }, 1000); //这里设置定时器，定义长按1000毫秒触发长按事件，时间可以自己改，  
                return false;  
            },  
            handletouchend() {  
                clearTimeout(this.time); //清除定时器    
                if (this.time != 0) {  
                    //处理点击时间  
                }  
                return false;  
            },  
            handletouchmove() {  
                clearTimeout(this.time); //清除定时器    
                this.time = 0;  
            },  
           onLongPress(e) {  
                    // 处理长按事件  
           }  
```

```
手指开始触摸元素。

element.touchstart(options: Object): Promise<void>

options 字段定义如下：

字段	类型	必填	默认值	说明
touches	array	是	-	触摸事件，当前停留在屏幕中的触摸点信息的数组
changedTouches	array	是	-	触摸事件，当前变化的触摸点信息的数组
element.touchmove

手指触摸元素后移动。

element.touchmove(options: Object): Promise<void>
```

2021-07-01

# uni应用

## 1.配置

### 1.1环境配置

manifest.json 

1.含有uni-app应用标识，每一个uni应用都会有一个唯一标识

2.小程序-开发-开发管理-服务器域名-request和uploadFile合法域名的配置（https://xxx.xx.com)

### 1.2代码配置

3.在main.js内可让console.log无效，需要调试的话将代码注释。

```js
if (uni.getSystemInfoSync().platform !== "devtools") {
  console.log = () => {}
}
```

全局方法，针对发布上去的才会起作用。

3.静态图片放到static里面，component也放在根目录内，这样才能解析到。

4.utils里面为封装的一些方法

req.js则是封装好的请求方法，要搭配../config/apiroot.js，

其中的

```js
const req = (method, data = {}, opt) => {
	opt = opt || {};
	data.method = method
	opt.method = opt.method || 'POST';
	opt.header = opt.header || {
		'content-type': 'aaa/b-ccc-dddd-urlencoded'
	};
```

这个是要和后台一起定义好的，不是一成不变的.

其中，../config/apiroot.js将config复制进去，然后修改KEY_INFOS里面的内容

```js
const KEY_INFOS = {
	Development: {
		applet: {
			'kAppKey': 'aaa1',		//后台定义
			'kAppSecret': 'bbb1'	//密钥同上
		},
		url: {
			'ApiUrl':'https://testapi.baidu.com/projectApi/api/rest/router',
            //request域名+接口
		}
	},
	Production: {
		applet: {
			'kAppKey': 'applet_ababababababababababsssss',
			'kAppSecret': 'qwerrttyyuiasdfjkl'
		},
		url: {
			'ApiUrl': 'https://openapi.relax.baidu.cn/relaxedCloud/api/rest/router',
		}
	}
}
```

## 2.上传

微信开发者工具-上传-填写版本上传，注意要等运行后上传。

## 3.

- filter过滤器，管道符。

- 在data里设置了对象{}，内有键值对的形式存储，在方法内需要定义this.userData.recordId=' '，否则是没有值的，猜测是复杂数据类型类型。
- 接口返回值错误时首先看Network内是否传进值了再说。
- 要做出错处理
- hover-class="recordBtnClick" 添加阴影，互动

