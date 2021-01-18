# CSS

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

## 随机数

从min到max的随机数

```js
     function random(min,max){
            return Math.random()*(max-min)+min;
        }
```



# Object

### 1.深拷贝

```javascript
function clone(obj){
    var newObj=null;
    if(typeof obj=='object'&&obj!==null){
        
        newObj=obj instanceof Array?[]:{};
        for(var v in obj){
            newObj[v]=clone(obj[v]);
        }
    }else{
        newObj=obj;
    }
    	return newObj;
}
```

### 2.将对象转换为一维数组 

> 将一个对象变为二重数组后再存到一重数组内！能方便调用

> {foo:'bar',baz:43}=> [["foo","bar"],["baz",42]]=>["foo","bar","baz",42]

> Object.fromEntries()  将二维数组变成对象：↑逆操作

```js
 function entries(obj = 0) {			//传入需封装的对象
            if(obj===0){return 0;}
            let arr = [];
            obj = Object.entries(obj);
            for (const [attr, value] of obj) {
                arr[attr] = value;
            }
            return arr;
        }

let o=entries(obj);						//use
```

### 3.属性是否在对象里

```js
function judgePro(obj,pro){
    if(pro in obj){
        	if(obj.hasOwnProperty(pro)){{
                 console.log(`${pro}对象在属性里,自有的`);
            }else{
                console.log(`${pro}对象在属性里,继承的`);
            }}else{
                console.log(`${pro}对象不在属性里`);
            }                      
    }
}
```

##### ***对象.hasOwnProperty(prop)***  判断属性在哪个对象，是否是一级或者继承

*in*  检测可枚举属性

*instanceof*   检测构造函数是谁

### 4.查找一个元素里是否包含另一个元素的某一个值

```js
  function find(){
    let obj={
           sun: { value: ["晴天","晴"], path: "./weatherImg/sun.png" },
           rain: { value: ["小雨", "阵雨", "大雨", "中雨"], path: "./weatherImg/rain.png" },
           cloud: { value: ["多云", "少云", "云"], path: "./weatherImg/cloud.png" }
       }
    let obj1={value:"晴天"};
    var arr=Object.values(obj).find(item=>item.value.includes(obj1.value)).path;               
// Object.values(obj).find(item => item.value.includes(weatherObj.weather)).path;
    return arr;
    }
```

## 防抖和节流

### 防抖

```js
let input=document.querySelector("input");
let timer=null;
input.oninput=function(){						//oninput 事件在用户输入时触发
    clearTimeout(timer);
    timer=setTimeout(()=>{
        console.log(this.value);
    },1000)
}
```

### 节流

```js
let count=0;
(function(){
   let flag=true;
btn.onclick=function(){
    if(!flag)	return;
        flag=false;
    setTimeout(()=>{
        flag=true;
    },1000);
    
    console.log(++count);
} 
})()
```



# window

### 1.获取url的对象，window，history

```js
function getSearchMsg(){
    if(location.search.length>0){
        let qs=location.search;
        let items=qs.split("&");
        let arg={};
        
        for(let i=0;i<items.length;i++){
            let tepArr=items[i].split("=");
            arg[window.decodeURIComponent(tepArry[0])]=window.decodeURIComponent(tepArry[1]);
        }
    }
    return arg;
}
```

> 获取url的值并存为对象

### 2.自有网页内嵌一个网页搜索

```js
window.open("https://www.shiguangkey.com/video/1346?videoId=96821&classId=6509&playback=1")
```

### 3.解码

```js
window.decodeURI
```

### 4.location对象

```js
location
```

### 5.history对象

```js
history.go(-1)	//上一页
history.go(1)	//前进一页
history.go(2)	//前进2页
```

> 需要是当前网页

# 兼容性

### 1.事件监听兼容性封装方法

```js
let EventUtil={
	addHandle:function(ele,type,handle){
        if(ele.addEventListener){
            ele.addEventListener(type,handle)
        }else if(ele.attachEvent){
            ele.attachEvent("on"+type,handle)
        }else{
            ele["on"+type]=handle;
        }
    },
    removeHandle:function(ele,type,handle){
        if(ele.removeEventListener){
            ele.removeEventListener(type,handle)
        }else if(ele.removeEvent){
            ele.removeEvent("on"+type,handle)
        }else{
            ele["on"+type]=handle;
        }
    }
}


function fn(){
    console.log("a");
}
EventUtil.addHandle(demo,"click",fn);
```

# 手撕代码

### 1.手写call方法

```js
 Function.prototype.call4=function(thisArg){
      //先判断是不是空的，是的话指向window，不是的话采用Object方法，将thisArg传进去      
     thisArg= thisArg==null?Window:Object(thisArg);
      // log(thisArg); //调用的对象
      //然后将剩下的使用[...rest]将它变为数组，然后返回一个新对象（slice复制
           let arrArg=[...thisArg].slice(1);
     
           let symbolFn=Symbol('fn');
           thisArg[symbolFn]=this;	//方法  调用对象.方法
           let result=thisArg[symbolFn](...arrArg);		//调用对象.方法（参数）
           delete thisArg[symbolFn];
           return result;
        }
```

