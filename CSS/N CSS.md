# 插件

## 同归于尽的绑定

```js
  function death(ele1,ele2){
            ele1.addEventListener("DOMNodeRemoved",function(){
                ele2.remove();
            })
        }

death(ele1,ele2);
```

1.querySelect 

2.addEventListener

3.vw,vh以屏幕为

## 清除标签

```js
  .clearfix::after{
        content:"";	//必备
        display: block;	
        clear: both;
    }
```



# N CSS

## 选择器优先级

![image-20201218143623026](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201218143623026.png)



## 浮动

1.浮动本身为了环绕文字设计，浮动不会遮盖文字

2.若第一个元素浮动后 后面的元素会上移

3.但是若前面有元素且在文档流内，浮动则不会覆盖此元素，此时的浮动则是就地向上浮动。

4.浮动后 后一个元素尾随着前一个元素的空隙。若单行无空隙则在前一个元素的下方

5.给图片浮动，图片则飘到左边，文字环绕图片。图片本身不在平面上。（在哪一行，图片不能超出所在行框

6.被设置为浮动元素会自动变成块元素



## BFC 块级格式化上下文

除非父元素形成BFC，否则子元素的外边距不会导致父元素的外边距变大。（没框则会合并，一起往下

意思是：父元素形成BFC，子元素会撑开父元素的高度

形成BFC：

​	  1.float元素。

　　2.position不是static，relative。

　　3.display为inline-block, table-cell, table-caption，flex和inline-flex。

　　4.overflow不为visiable。

## 伪元素

一定要有content样式来激活为元素，哪怕是空着的“ ”也要

content样式值种类:

1. none 不会产生伪类元素

2. normal 效果同上

3. 文本

4. url()

```css
a[href="*baidu"]::before{
    content:url("tupianlujing")
}

.text::before{
    content:"$";
    color:red;
    margin-right:5px;
}
```

装饰永远是装饰，内容永远是内容

思维导图，选择器

emmet

## 盒模型

#### <u>content-box</u>

width 只有content

定死内容玩边框

#### <u>border-box</u>

width 有padding，border，content 

定死边框，靠内容撑起来的：内容多变长，内容少变少，实际占据多少。



# html Dom element对象

### appendChild（boolean）

boolean是否克隆其属性。

#### element.innerHTML

设置或返回元素的内容。

#### tagName

返回元素的标签名

#### textContent

返回节点及其后代的文本内容

# 样式

## 动画 animation

### 触发式动画 transition

1.元素默认动画第一帧和最后一帧 只有两个帧数。

2.给触发式动画的动画时间	transition-duration：0.5s   过渡动画时间

例子：

```css
input:checked~div{
    height:500px;
}
.animation{
    	width:300px;
    	height:300px;
    	background-color:red;
    	margin:50px auto
            0;
    	transition-duration：0.5s ;
    	transition-delay:3s;
}
.animation:hover{
    	height:500px;
}
```

1. transition-duration：0.5s   过渡动画时间

2. transition-delay:1s               过渡动画的延迟效果                       单程和双程效果

3. tansition-timing-function:ease 缓进缓出(默认；linear 线性;  **cubic-bezier（）（网站**；

4. transition-property:heigth/width/all;                过渡动画属性

5. transtion:     综合样式

   ex：transition:width 3s linear,background-color 3s linear;

   

```
1.：hover	鼠标悬浮
2.：checkd	选项选中
3.：active 	输入框点击	
```

### 主动式动画 animation

1.不管用户鼠标键盘，刷新一瞬间就执行。

2.当且仅当关联了才能有动画↓

```css
.animation{ 
    animation-name:onepiece;			主动：动画名称
    animation-duration:5s;				主动：动画时长
    animation-delay:5s;					动画延迟效果（延迟几秒开始			可以写负值的
    animation-timing-function:linear;
    animation-iteration-count:3/infinite;		动画播放次数
    animation-direction:normal/alternate/reverse/alternate-reverse;		是否逆向播放
    animation-fill-mode:forwards/backwards;		默认最后还是第一个；	
}

div:hover{				
 	animation-play-state：paused;		漫画暂停
}

@keyframes opepiece{					可以有多个帧数。
    0%{
        transform:skew(45deg);
        height:300px;
    }
    50%{
        height：500px;		
    }
    100%{
        height:600px;					空的话就是默认状态
    }
}
```

## animation圆环进度条

```html
<div id="progressBar">
      <!-- 进度条 -->
      <div>
        <span></span>
      </div>
      <!-- 五个圆 -->
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      <span></span>
 </div>
```

```css
	/* 元素大小宽度 */
#progressBar{
            width: 80%;
            height: 50px;
            position: relative;
            margin: 50px 0 0 100px;
        }
	/*  条纹边框，底色 */
        #progressBar div{
            width: 100%;
            height: 10px;
            position: absolute;
            top:50%;
            left: 0;
            margin-top:-20px;
            background: #ccc;
        }
	/* 进度条颜色 */
        #progressBar div span{
            position: absolute;
            display: inline-block;
            background: green;
            height: 10px;
            width: 100%;
            -webkit-animation:bgLoad 5.5s linear;
        }
			/* 每动到每个原点停止 停顿的时候 */
        @-webkit-keyframes bgLoad{
            0%{
                width: 0%;
            }
            18.18%,27.27%{
                width:25%;
            }
            45.45%,54.54%{
                width: 50%;
            }
            72.72%,81.81%{
                width: 75%;
            }
            100%{
                width:100%;
            }
        }	
		/* 圆球大小颜色 */
        #progressBar>span{
            position: absolute;
            top:0;
            margin-top: -10px;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: #ccc;
            margin-left: -20px;
            color:#fff;
        }
/* 圆球变色 */
        @-webkit-keyframes circleLoad_1{
            0%,66.66%{
                background: #ccc;
            }
            100%{
                background:green;
            }
        }
        @-webkit-keyframes circleLoad_2{
            0%,83.34%{
                background: #ccc;
            }
            100%{
                background:green;
            }
        }
        @-webkit-keyframes circleLoad_3{
            0%,88.88%{
                background: #ccc;
            }
            100%{
                background:green;
            }
        }
        @-webkit-keyframes circleLoad_4{
            0%,91.67%{
                background: #ccc;
            }
            100%{
                background:green;
            }
        }
	/* 圆球位置 */
        #progressBar span:nth-child(2){
            left: 0%;background:green;
        }
        #progressBar span:nth-child(3){
            left: 25%;background:green;
            -webkit-animation:circleLoad_1 1.5s ease-in;
        }
        #progressBar span:nth-child(4){
            left: 50%;background:green;
            -webkit-animation:circleLoad_2 3s ease-in;
        }
        #progressBar span:nth-child(5){
            left: 75%;background:green;
            -webkit-animation:circleLoad_3 4.5s ease-in;
        }
        #progressBar span:nth-child(6){
            left: 100%;background:green;
            -webkit-animation:circleLoad_4 6s ease-in;
        }
```

可以看到，其实对于动画本身是很简单的，一看就明白了，**主要就是动画持续时间的计算**，由于这个动画效果只执行一次，所以其实也可以用动画延迟时间来保证各个动画在指定的时间点开始执行，但是对于循环或者多次动画效果，延迟很不灵活，所以这里还是用持续时间的长短来控制动画的执行时间。

这个订单进度条，我是设置了走一段用时1秒，然后每到一个圆点就停顿0.5秒，而这0.5秒就是相对应的圆点的动画持续执行时间。但是**再次强调这个是单次动画**，如果想实现循环动画，还是得做调整的，必须让所有动画的持续执行时间是一样的，不然循环起来就错乱的。而时间的改动也会影响动画关键帧的改动，**下面对第一小段和第二个圆的动画时间讲解一下**：

首先，细长条的动画持续时间通过计算：

4小段x1秒 + 中间3个圆点 x 0.5秒 = 5.5秒

接下来就是计算细长条动画关键帧的时间分配，设每一份0.5秒，那么共总就是11份，每小段得2份，每个圆点得1份，用100%除以11，可得每份大约是9.09%，接下来就是分配时间了，这个就简单了，不多说。

接着，当细长条完成第一小段的动画效果到达第二个圆点时，会停顿0.5秒，而这0.5秒就是第二个圆点的动画时间，所以第二个圆点的动画持续时间就是：

等待细长条执行完1小段 x１秒＋自身的动画完成时间ｘ０.５秒＝１.５秒

同样的方法计算每一份的时间然后进行分配。**同理，其他动画效果相似，就不再赘述了。**

### 点击触发圆环进度条

想弄成点击后再触发动画，使用了vue组件，然后

```html
		<style>
			/* 元素大小宽度 */
			.down {}
			/*  条纹边框，底色 */
			.down div {}
			/* 圆球大小颜色 */
			.down>span {}
			
			/* 圆球位置 */
			.down span:nth-child(2) {
				left: 0%;
			}
			
			.down span:nth-child(3) {
				left: 25%;
			}
			
			.down span:nth-child(4) {
				left: 50%;
			}
			
			.down span:nth-child(5) {
				left: 75%;
			}
			
			.down span:nth-child(6) {
				left: 100%;
			}
			/* ------------- */
			/* 元素大小宽度 */
			.progress {}

			/*  条纹边框，底色 */
			.progress div {}

			/* 进度条颜色 */
			.progress div span {}

			/* 每动到每个原点停止 停顿的时候 */
			@-webkit-keyframes bgLoad {
				0%,
				16.66% {
					width: 0%;
				}
				24.99%,
				41.65% {
					width: 25%;
				}

				49.98%,
				66.64% {
					width: 50%;
				}

				74.97%,
				91.63% {
					width: 75%;
				}
				100% {
					width: 100%;
				}
			}

			/* 圆球大小颜色 */
			.progress>span {}

			/* 圆球位置 */
			.progress span:nth-child(2) {
				left: 0%;
				background: green;
				-webkit-animation: circleLoad_0 2s ease-in;
			}

			.progress span:nth-child(3) {
				left: 25%;
				background: green;
				-webkit-animation: circleLoad_1 3s ease-in;
			}

			.progress span:nth-child(4) {
				left: 50%;
				background: green;
				-webkit-animation: circleLoad_2 6s ease-in;
			}

			.progress span:nth-child(5) {
				left: 75%;
				background: green;
				-webkit-animation: circleLoad_3 9s ease-in;
			}

			.progress span:nth-child(6) {
				left: 100%;
				background:green;
				-webkit-animation: circleLoad_4 12s ease-in;
			}

			/* 圆球变色 */
			@-webkit-keyframes circleLoad_0 {
				0% {
					background: #ccc;
				}

				100% {
					background: green;
				}
			}

			@-webkit-keyframes circleLoad_1 {

				0%,
				83.88% {
					background: #ccc;
				}

				100% {
					background: green;
				}
			}

			@-webkit-keyframes circleLoad_2 {

				0%,
				88.88% {
					background: #ccc;
				}

				100% {
					background: green;
				}
			}

			@-webkit-keyframes circleLoad_3 {

				0%,
				88.88% {
					background: #ccc;
				}

				100% {
					background: green;
				}
			}

			@-webkit-keyframes circleLoad_4 {

				0%,
				91.67% {
					background: #ccc;
				}

				100% {
					background: green;
				}
			}
		</style>
	</head>
	<body>
		<div id="progressBar">

			<!-- 进度条 -->
			<div v-bind:class="{progress:progress,down:down}">
				<div>
					<span></span>
				</div>
				<!-- 五个圆 -->
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
			</div>
			<button @click="showProgress">click</button>
		</div>

	</body>

	<script>
		new Vue({
			el: '#progressBar',
			data: {
				down:true,
				progress:false
			},
			methods:{
				showProgress(){
					this.down=false
					this.progress=true
					// this.down=!this.down
				}
			}
		})
	</script>
```

首先设置了v-bind绑定两个不同的class在一个标签里，然后data数据在里面，通过每次点击后改变data数据的值去展示不同的class类。

其实感觉和v-if是否显示很像，都是为false时不显示，true时就显示。

查询来源：runoob。

其实有看见transition来控制是否发生动画，感觉优化的时候要根据transition看。

## 变化样式 transform

```css
.trans{											《参数
    transform:rotate(45deg);				旋转
    transform:translate(50px,40px);			位移 水平偏移，垂直偏移     沿着元素本身的x轴和y轴来的
    transform:scale(0.8);					缩放，沿着中心点，放的是倍数	负数的话就是旋转180°
    transform:skew(45deg);					倾斜	角度值
       transform:skewX(45deg);	
       transform:skewY(45deg);
    
    transform-origin:50% 50%;默认		    	变化原点（可以px（在元素的哪里 ex：0,0的话就是左上角
    
}
```

1.不会对兄弟元素等其他元素产生影响。宽高值不会变（只是视觉变化了而已

2.设置变化元素后层级变高了。

3.可以组合使用，但是顺序不同导致结论不同。

```css
.trans{		
     transform:rotate(45deg) translate(50px,40px) skew(45deg);	
}
```

## 优化

其实margin，left，transform都可以实现，但是看怎么样能性能最优化

transform最优化，更少的资源完成更多的事情

1.canvas

2.css控制元素位置



1.空间复杂度：占据内存量

2.时间复杂度：占据CPU计算量



## 响应式

@media 查询屏幕宽度

min-width 最小宽度

max-width 最大宽度

```css
.info{
        width:400px;
    }
@media all and (min-width:100px) and (max-width:500px){			用and连接
    .info{
        width:400px;
    }
}
```

1.断点的话	1440    1336   1024   768   600px，市场调研，网站写几个断点合适

2.引入：内联样式

​				外部样式

```css
<link rel="stylesheet" href="a.css" media="all and (min-width:800px)">宽度大于八百像素就生效
```

缺点：1.工作量很大，

​			2.加载时间延长

## viewport 视口

```css
<meta name="viewport" content="width=decive-width,initial-scale=1.0，max-scale=1.0">
									设备宽度		初始缩放				最大缩放

user-scalable=no	不允许缩放
```

# Canvas 动画

```js
  canCon.clearRect();		//清除
  canCon.beginPath();		//抬笔
  canCon.fillStyle="red",	//选择颜色
  canCon.arc(233,233,66,0,Math.PI*2);		//画圆
  canCon.fill();   //落笔
```

​    background-image: linear-gradient(to bottom, #f5b461 0%, #9ad3bc 100%);











## canvas

1.没有 src 和 alt，只有两个属性**——** width,height

2.需要结束标签，无则bug

3.开始空白，找渲染上下文后绘制（getContext() 获取渲染上下文和绘画功能，接受一个参数上下文类型

```js
var canvas = document.getElementById('tutorial');
var ctx = canvas.getContext('2d');
```

DOM对象后getContext()

4.检查支持性

```js
var canvas = document.getElementById("testCanvas");
if(canvas.getContext){
	var ctx = canvas.getContext('2d')
}else{
	//不支持替换
}
```

### 5.绘制矩形(左上开始)

[`fillRect(x, y, width, height)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/fillRect)

绘制一个填充的矩形

[`strokeRect(x, y, width, height)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/strokeRect)

绘制一个矩形的边框

[`clearRect(x, y, width, height)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/clearRect)

清除指定矩形区域，让清除部分完全透明

### 6.绘制路径

```
beginPath()
```

新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。

```
closePath()
```

闭合路径之后图形绘制命令又重新指向到上下文中。

```
stroke()
```

通过线条来绘制图形轮廓。

```
fill()
```

通过填充路径的内容区域生成实心的图形。

beginPath(),moveTo(x,y),moveTo(),fill()  此时就不需要closePath()

### 7.移动笔触

moveTo（x,y)

### 8.线   lineTo(x,y)

```html
<html>
 <head>
  <script type="application/javascript">
    function draw() {
   var canvas = document.getElementById('canvas');
  if (canvas.getContext) {
    var ctx = canvas.getContext('2d');

    // 描边三角形
  ctx.beginPath();
  ctx.moveTo(125, 125);
  ctx.lineTo(125, 45);
  ctx.lineTo(45, 125);
  ctx.closePath();
  ctx.stroke();
  }
}
  </script>
 </head>
 <body onload="draw();">
   <canvas id="canvas" width="300px" height="300px"></canvas>
 </body>
</html>

```

lineTo()需要closePath()

### 9.圆弧

[`arc(x, y, radius, startAngle, endAngle, anticlockwise)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/arc)

画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。

[`arcTo(x1, y1, x2, y2, radius)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/arcTo)

根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点。

10.Path2D

```js
 var ctx = canvas.getContext('2d');

    var rectangle = new Path2D();
    rectangle.rect(50, 50, 500, 500);
  ctx.stroke(rectangle);
```

11.colors 

[`fillStyle = color`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/fillStyle)

设置图形的填充颜色。

[`strokeStyle = color`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/strokeStyle)

设置图形轮廓的颜色。

