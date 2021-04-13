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