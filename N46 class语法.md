# 46 class语法

## 46.1 class语法详解

### 1.组合模式用class

**1.class里的所有方法定义在类的prototype属性上面**

2.ES5构造函数Point对应ES6的Point类的构造方法

```js
class Point{
    constructor(x,y){
        this.x=x;
        this.y=y;
    }
    
    toString(){
        console.log(this.x+this.y);
    }
}

let point=new Point(1,2);
toString in point 			//true
```

##### 语法糖：计算机语言中添加某种语法但是面对语言的功能没有影响，更方便程序员的使用。

##### 2.在class里添加新函数

1.

```js
 Point.prototype.sayx=function(){
            console.log(this.x);
        }
```

2.assign：target，source  返回target

```js
  class Point {

        }
       
        Object.assign(Point, {
            constructor(x, y) {		//不先在class里写constructor，则默认不要，这里的constructor没有用，但是不报错
                this.x = x;
                this.y = y;
            },

            toString() {
                console.log(this.x + this.y);
            }
        })

 		let point = new Point(1, 2);
  		"constructor" in Point		//true,但是其实是不可枚举的
```

*方法    in     构造函数* 	//检测可枚举属性

*实例   instanceof   构造函数*   //检测构造函数是谁

3.缺点：在class里定义的所有方法，看上去好像不可枚举的，

```js
Object.keys(Point.prototype)
["sayx"]
```

无论是可不可遍历，reflect.keys

### 对象属性遍历

ES6一共有五种方法可以遍历对象属性

1.for...in 

2.Object.keys(obj)

在12两个属性里面，在谷歌浏览器中对class的遍历得出的属性是不一样的

3.Object.getOwnPropertyNames(obj)

4.Object.getOwnPropertySymbols(obj)

5.Reflect.ownKeys(obj)

![image-20201206113313814](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201206113313814.png)

### 2.constructor

1.寄生构造模式不能用在class里，否则会创造最干净的class

```js
class foo{
    constructor(){
        return Object.create(null);
    }
}
foo();//VM1064:1 Uncaught TypeError: Class constructor foo cannot be invoked without 'new'
  //  at <anonymous>:1:1
```

2.原来class里不写constructor，在Object.assign里用constructor则没有用。

### 3.getter和setter

![image-20201206115207694](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201206115207694.png)

### 4.属性名  类名表达式实现

类的属性名可以采用表达式

```js
let methodName='getArea';
class Square{
    [methodName](){		//方法名getArea    Symbol值
        
    }
}
```

**5.表达式**

**6.注意点**

1.严格模式 	 ES6，类和模块的内部，默认严格模式

2.不存在提升，前方都会是临时死去

```js
new Foo();				//error
class Foo{};	
```

3.name属性 		函数名称

```js
class Foo{};
Foo.name 	//"Foo"
```

4.this的指向	默认指向类的实例，单独使用的话可能会报错

解决办法:箭头函数（**<u>普通函数是调用对象的this，箭头函数是创造的this</u>**，就是new出来的新对象

```js
class Obj{
    constructor(){
        this.getThis=()=>this;		//this永远是new出来的新对象！this就是Obj
    }
}

const myObj=new Obj();
myObj.getThis()===myObj;			//true
```

### 7.属性定义规范

实例属性定义在constructor里面

```js
class Point{
    constructor(){
        x=1;
    }
    toString(){
        //...
    }
}
```

第二个直接在类的最顶层定义就可以了

```js
class Point{
    x=1;
    toString(){
        //...
    }
}
```

## 46.2 class的静态方法和私有属性

### 静态方法:不会被实例继承，直接通过类的名称.属性或方法来调用

用static

```js
class Point{
    static bar(){
        this.baz();		//Point.baz()
    }
    static baz(){		
        console.log('hello')
    }
    
   baz(){
       console.log('world')
   }
}
```

ex

```js
class Point{
    static baz(){		
        console.log('hello')
    }
    
   baz(){
       console.log('world')
   }
}

Point.baz()		//hello
let point=new Point();
point.baz()		//world
```

1.父类的静态方法可以被子类继承。

2.静态方法也可以从super对象上调用的

3.super：对象的原型，只能用在 方法 里面

### 私有方法:外部不能访问，只能类的内部调用,可被继承

1.下划线命名区别（命名规范

```js
class Point{
   //公有
   baz(){		
        console.log('hello')
    }
   //私有，用下划线区别
   _baz(){
       return this.snaf=baz;
   }
}
```

2.Symbol的唯一性，将私有方法的名字命名为一个symbol值

```js
const bar=Symbol('bar');
const snaf=Symbol('snaf');

export default class myClass{
    //公有方法
    foo(baz){
        this[bar](baz);
    }
    
    //私有方法
    [bar](baz){
        return this[snaf]=baz;
    }
}
```

得不到的symbol值

3.用#符号 在属性名前用#表示

```js
class Point{
   #a;
   #b;
   constructor(a,b){
   		this.#a=a;
   		this.#b=b;
   }
	#sum(){
    return #a+#b;
	}
    printSum(){
        console.log(this.#sum());
    }
}
```

getter和setter也是

```js
class Point{
   #a;
   #b;
   constructor(a,b){
   		this.#a=a;
   		this.#b=b;
   }
	get #sum(){
		
	}
}
```

## 46.3 构造函数的新属性

Class内部调用new.target,返回当前Class

只要不是new调用的，他会返回undefined。

### new.target 检测是否使用构造函数

```js
function Person(name){
	if(new.target!=='undefined'){
        this.name=name;
    }else{
        console.log('必须使用new命令生成实例');		//丢出error
    }
}
```

# 47 class继承

## 47.1 class的extends继承

一个对象的实例可以作为另一个对象的原型

##### ES6继承

```js
class A{
    constructor(){
        this.x=1;
        this.y=2;
        this.name='wan';
    }
    sayName(){
        console.log(this.name);
    }
}

class B extends A{
    constructor(){	
        super();	//super指向对象的原型，只能用在方法里面	A.prototype.constructor.call(this);
        this.age=18;
    }
    sayAge(){
        console.log(this.age);
    }
}
```

##### ES5继承

```js
function PersonF(){
    this.name='yin';
    this.x=1;
    this.y=2;
}

function PersonS(){
    this.age=18;
}

PersonsS.prototype=new PersonF();
PersonS.constructor=PersonS;

let yinshi=new PersonS();
```

![image-20201207111403468](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201207111403468.png)



#### 区别：

！ES5先有儿子再有老爸。先son实例，在新对象上添加原型方法

！ES6必须在子类里面用父类的构造函数创造出一个对象，子类在对象上添加子类的方法。

​		老爸先生一个，再儿子在老爸生的里面添加方法



***在ES5上本应该出现在父级的属性出现在了子集**，其他几乎是一模一样的。

1.class的extends继承的父类的自由属性都会变为子集的自由属性。

**无论继承多少个层级，最后都是会在实例上显示自由属性。**

2.Object.keys()  获取自由属性

![image-20201207111515321](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201207111515321.png)

3.class 实例的原型等于子集的原型，子集的原型等于父级的原型

​			实例.prototype=B ; B.prototype=A

​	ES5 	实例原型等于父级的原型

4.

![image-20201207112224347](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201207112224347.png)

#### ES6优点：

1.自由属性

2.不需要手动设置继承关系

3.是否遍历，手动定义的方法都为可便利，extends里不能遍历

4.方法是方法，属性是属性

#### class的prototype和_proto_属性的差别：

1.子类的_proto_属性，表示构造函数的继承，总是指向父类

```js
	son._proto_=Father
```

2.子类的prototype属性的_proto_属性,表示方法的继承，总是指向父类的prototype属性

```js
	son.prototyoe._proto_=father.prototype
```

#### Object.setPrototypeOf 	

先将方法继承，然后继承自由属性

#### class的实现原理

```js
Object.setPrototypeOf(B,A);		//将A的静态属性赋给B

//ex
class A{}
class B{}
//B的实例继承A的实例
Object.setPrototypeOf(B.prototype,A.prototype);
//B继承A的静态属性
Object.setPtototypeOf(B,A)

const b=new B();
```

## 47.2 super关键字详解

#### 1.super()

​	1.super指向对象的原型，只能用在方法里面

​	相当于

```js
A.prototype.constructor.call(this);
```

​	2.确实是B生出来的，但是先从老祖宗->爸爸->过一道再还你。所以this是儿子生出来的

​	3.不能在super方法之前使用this

#### 2.super使用：属于闭包。

​	不建议此种解法

```js
	    class A {
            constructor(name) {				//爸爸接收
                this.x = 1;
                this.y = 2;
                this.name = name;
            }
            sayName() {
                console.log(this.name);
            }
        }

        class B extends A {
            constructor(name,age) {			//传参
                super(name);				//给爸爸
                this.age =age;
            }
            sayAge() {
                console.log(this.age);
            }
        }
        let wanzhang = new B('wan',18);		//传参
```

#### 3.普通方法：super指向父类的原型对象；静态方法：super指向父类自己。

1. ​		**super作为对象时，在普通方法中指向父类的原型对象；静态方法中指向父类。**

```js
class A{
    p(){
        return 2;
    }
}
    
    class B extends A{
        constructor(){
            super();
            console.log(super.p())			//2
    }
}

let b=new B();
```

![image-20201207160230563](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201207160230563.png)

![image-20201207160310020](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201207160310020.png)

​	2. 3. 	蓝线重要！

​	super调用父类的方法时，内部的this指向当前的子类实例

​    在子类的静态方法中通过super调用父类的方法时,方法内部的this指向当前的子类而不是子类实例.

​	4.	赋值会变为子类实例的属性

​	5.	super要显性 函数还是对象使用

## 47.3 Mixin模式

1. 混合模式

![image-20201207161320823](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201207161320823.png)

# 48 Iterator 迭代器

## 48.1 Iterator的概念

迭代器，遍历，接口给的功能

Symbol.Iterator 迭代器的功能

遍历器是一种接口，为不同的数据结构提供统一的访问机制。本身是一个功能。

！返回一个迭代器对象

#### 作用

1.为各种数据结构提供一个统一、简便的访问接口

2.使得数据结构成员能够按某种次序排列		set 先进先出

3.创造了一种for...of

#### 步骤

1.生成迭代器，在第一个之前(**0号位之前**)

2.指针位置，next方法，指向数据结构第一个成员

3.不断调用next方法

#### 仿写迭代器：模拟

```js
function makeIterator(arry){
    var nextIndex=0;	//默认下次指针指向的位置
    return {
        next(){
            //返回对象，下一次指针指向的位置是否超出数组长度
            if(nextIndex>=arry.length){
                return {
                    value:undefined,
                    done:true		//表示已经结束遍历
                }
            }else{
                return {
                    value:arry[nextIndex++],	//先访问再++
                    done:false
                }
            }
        }
    }
}

let It=makeIterator([1,2,3,4,5,6,7,8]);
It.next();
```

#### 原生

```js
let a=[1,2,3,4,5,6];
let fn=a[Symbol.Iterator]();
```

**遍历器与他遍历的数据结构实际上是分开的，因为只是把接口加到数据结构之上。**

## 48.2 默认的迭代器接口

set，map，array，String,TypedArray,arguments,NodeList

### 迭代器仿写

```js
class RangeIterator{
    constructor(start,end){
        this.value=start;
        this.end=end;   
    }
    [Symbol.iterator](){
        return this;
    }
    next(){
        var _value=this.value
        if(_value<this.end){	//左闭右开区间
            this.value++;
            return {
                value:_value,
                done:false
            }
        }else{
             return {
                value:undefined,
                done:true
            }
        }
    }
}

for(let i of new RangeIterator(1,9)){console.log(i)}
```

### 遍历器实现指针例子:对象是object

```js
function Obj(value){
    this.value=value;
    this.next=null;	//为null的都一定会传对象
}

Obj.prototype[Symbol.iterator]=function (){
    var iterator={next:next};	//next里面是一个函数
    var current=this;		
    function next(){
        if(current){
            value=current.value;
            current=current.next;
            return{done:false,value:value};
        }else{
            return {done:true};
        }
    }
    return iterator;
}

var one=new Obj(1);
var two=new Obj(2);
var three=new Obj(3);
one.next=two;
two.next=three;
for(var i of one){
    console.log(i);		//1,2,3
}
```

### 遍历器实现指针例子:对象是array

实现任意数组

```js
class obj{
    constructor(x,y,name){
        this.x=x;
        this.y=y;
        this.name=name
    }
    data(){
        return Object.entries(this);	//实例._proto_=constructor.prototype
    }
    [Symbol.iterator](){		//值是固定的 Symbol.iterator ，他只会找这个值，在for...of里
        const self=this.data();
        let index=0;
        return{
            next(){
                if(index<self.length){
                    return {
                        value:self[index++],//也可以改值截胡 value:[...self[index++],"aa"]
                       //[...[ this.start++],"adfas"]
                        done:false
                    };
                }else{
                    return{
                        value:undefined,
                        done:true
                    };
                }
            }
        }
    }
}

let o=new obj(2,8,12);
let it=o[Symbol.iterator]();
it.next();

for(let i of o){console.log(i)};
```

可遍历的for...of循环就是用Symbol.Iterator来遍历的



顶层截胡：

```js
Object.assign(Object.prototype,{
      data(){
        return Object.entries(this);	//实例._proto_=constructor.prototype
    }
    [Symbol.iterator](){		//值是固定的 Symbol.iterator ，他只会找这个值，在for...of里
        const self=this.data();
        let index=0;
        return{
            next(){
                if(index<self.length){
                    return {
                        value:self[index++],//也可以改值截胡 value:[...self[index++],"aa"]
                        done:false
                    };
                }else{
                    return{
                        value:undefined,
                        done:true
                    };
                }
            }
        }
    }
})
```

### 类数组迭代器接口设置方式

```js
let iterable={
    0:'a',
    1:'b',
    2:'c',
    length:3,
    [Symbol.iterator]:Array.prototype[Symbol.iterator]
};
for(let item of iterable){
    console.log(item);			//'a','b','c';
}
```

1.有length值

2.以数字为下标

## 48.3 Iterator的常用领域

### 1. 解构赋值  调用迭代器接口

对set结构进行解构赋值时，会默认调用set数据的Symbol.iterator方法

```js
let set=new Set().add('a').add('b').add('c');
let [x,y]=set;			//x='a',y='b'
let [first,...rest]=set	
```

### 2. 扩展运算符

```js
var str='hello';
[...str];
```

### 3. 字符串的Iterator接口

字符串迭代器接口

先变成数组，再对数组进行迭代

```js
var str=new String('hi');
[...str]
```

## 48.4 迭代器接口的其他方法

### 1. return

迭代器执行遍历结束后就会执行return.

只要报错了就会执行return方法提前终止遍历

### 2. throw 

终止遍历并执行throw

### 3.for...of 本质上调用接口产生的遍历器

```js
const arr=['red','green','blue'];

for(let v of arr){console.log(v)};

const obj={};
obj[Symbol.iterator]=arr[Symbol.iterator].bind(arr);

for(let v of obj){console.log(v)};
```

# 49 generator生成器

## 49.1 generator的概念

1. yield 状态，每一个状态都是yield语句
2. *跟在function后
3. 生成器函数是普通函数

```js
 function* gen(){    //写在func后面，生成器
            yield "hello";		//状态一次只会输出一个
            yield "world";
            yield "wan";
     		//return 'new';
        }

        //普通函数 生成器函数是普通函数
        let obj=gen();
		obj.next()
		//{value: "world", done: false}
		obj.next()
		//{value: "wan", done: false}
		obj.next()
		//{value: undefined, done: true}			//{value: new, done: true}
```

1.无需手动定义迭代器的格式

2.return里的值不是遍历的环节，return是结束的标石

3.return过后所有yield语句都结束遍历

## 49.2 yield表达式

1.运行方法，遇到yield表达式就暂停后面的操作，将yield后面表达式的值作为返回对象的value属性值

2.调用next方法，往下执行，直到遇到yield

3.没有遇到yield则找return

4.没有return则undefined，并且done为true

```js
function getValue(){
    return 5;
} 
function* gen(){  
            yield getValue();	//5
            yield "world";
            yield "wan";
        }

```

yield是惰性求值

#### yield使用规范

1.只能用在生成器里面

2.表达式用在另一个yield表达式中，必须放在圆括号里面

3.yield作为参数和赋值

#### yield 使用场景

1.生成器赋值给Symbol。iterator属性，从而使得该对象具有Iterator接口

```js
var myiterable={};
myIterable[Symbol.iterator]=function* (){
    yield 1;
    yield 2;
};
[...myIterable]			//[1,2,3]
```

2.generator返回一个遍历器对象，也有Symbol.iterator属性

```js
function* gen(){
    
}
var g=gen();
g[Symbol.iterator]()===g	//true
```

**3.next方法可以带参数，作为上一个yield表达式的返回值**

```js
function* f(){
    for(var i=0;true;i++){
        var reset=yield i;
        if(reset){i=-1;}
    }
}

var g=f();
g.next()
//{value: 0, done: false}
g.next()
//{value: 1, done: false}
g.next(true)
//{value: 0, done: false}
```

4.

```js
function* foo(x){
    var y=2*(yield(x+1));
    var z=yield(y/3);
    return (x+y+z);
}
var a=foo(5);
a.next();
//{value: 6, done: false}
a.next();
//{value: NaN, done: false}
a.next();
//{value: NaN, done: true}

var b=foo(5);
b.next()
//{value: 6, done: false}
b.next(12)
//{value: 8, done: false}
b.next(13)
//{value: 42, done: true}
```

5.

```js
 function* c(){
           let a= yield "a";
            console.log(a);
            let b = yield "b";
            console.log(b+a);
        }
        let df=c();

df.next(2)
//{value: "a", done: false}
df.next(2)
//Iterator.html:64 2
//{value: "b", done: false}
df.next(2)
//Iterator.html:66 4
//{value: undefined, done: true}
df.next(2)
//{value: undefined, done: true}
```

总结：意思是运行到yield，然后输出yield，下一个next语句是yield的返回值，意思就是“yield ”总的语句返回一个.next的参数。然后再执行

## 49.3 for...of循环与生成器

1.for...of

2.拓展运算符

3.解构赋值

4.array.from方法

## 49.4 函数的报错处理

#### 1.try catch用法

```js
try{
    
}catch(err){
    window.open("https://www.baidu.com/s?wd="+err,"blank");
}
```

#### 2.throw

主动报错出来

```js
try{
    throw "并不存在"
}catch(err){
    
}
```

#### 3.finally	

无论trycatch如何，都会执行finally里的代码

```js
try{
    
}catch(err){
    window.open("https://www.baidu.com/s?wd="+err,"blank");
}
finally{

}
```

err是object ,包含了很多方法，错误信息的名称是什么，错误信息的内容是什么。

#### 4.catch的error值

![image-20201215150056866](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201215150056866.png)

# 50 generator高阶

## 50.1 throw

笔记没有记全

```js
function* gen(){
    try{
        yield 1;
    }catch(err){
        console.log(err)
    }
}
let o=gen();
try{
    o.throw("hello")
    o.throw("world")
}catch(err){
    console.log(err);
}
```

1.要激活

！throw先是内部try catch写完，然后才是外部。有多少个catch就截胡多少个throw直到用光了

```js
function* gen(){
    while(true){//死循环
        	  try{
        yield 1;
    }catch(err){
        console.log("in"+err)			//1//2
    }
    }
  
}
let o=gen();
try{
    o.next();		//有了都是内部捕获，没有都是外部捕获		
    o.throw("hello")
    o.throw("world")
}catch(err){
    console.log("out"+err);
}

//in hello
//in world
```

2.函数体内出错，往外部传

```js
function* gen(){
    while(true){
        	  try{
        yield 1;
    }catch(err){
        if(err==="a") throw'b';			//1
        console.log("in"+err)
    }
    }
  
}
let o=gen();
try{
    o.next();		
    o.throw("a")					//直接不用执行了
    o.throw("world")
}catch(err){
    console.log("out"+err);				//2
}

//out b
```

3.throw方法被捕获后，会附带执行下一条yield表达式。也就是说会附带执行一次next方法。

```js
function* gen(){
    while(true){
      	  	  try{
     			   yield 1;		//1.1
   			 }catch(err){
      			  console.log("in"+err)
   		 }
        yield 1;				//1.2
        yield 1;
    }
  
}
let o=gen();
try{
    o.next();		
    o.throw("a")					//1				
    o.throw("world")				//2.1
    o.throw("hello")
}catch(err){
    console.log("out"+err);			//2.2
}
//in a
//out world
```

4.throw命令与g.throw方法是相互无影响，所以是无关的。

5.内部无catch，内部报错会被外部暴露，然后内部函数终止运行

## 50.2 return

generator还有一个return方法。返回给定特定的值，终结遍历器的遍历

#### 1.return 

```js
function* gen(){
  	  yield 1;
      yield 2;
      yield 3;
}

let g=gen();
g.next();				//{value:1,done:false}
g.return('foo');		//foo
```

#### 2.try...finally

trycatch后一定有finally。无论如何都要执行

```js
function* gen(){
  	  yield 1;
    try{
        yield 2;
        yield 3;
    }finally{
         yield 4;
       	 yield 5;
    }
     yield 6;
}

var g=gen();
g.next()	  	//{value: 1, done: false}
g.next()		//{value: 2, done: false}
g.return(7)		//{value: 4, done: false}
g.next()		//{value: 5, done: false}
g.next()		//{value: 7, done: true}
```

#### 3.return/throw/next

都是让generator恢复执行。

什么方法就是让yield表达式替换成什么语句

想要中断

## 50.3 yield*表达式

#### yield*对于yield的遍历对象完成遍历

```js
function* gen(){
    yield* ['a','b','c'];
}
gen().next();		//{value: "a", done: false}
```

```js
function* gen(){
    yield "hello";				//hello
    yield* "hello";				//h
}
```

## 50.4 generator其他特性

1.

```js
let obj={
    *myGen(){
        ...
    }
};
```

#### 2.generator 的this

没有通过new却是类继承

generator不能用new

箭头函数的this是定义了就在了

函数的this指的是调用该函数的对象。

#### 3.把一个生成器函数改造成构造函数

```js
function* gen(){
    this.a=1;
    yield this.b=2;		//this是独立的
    yield this.c=3;
}

function F(){
    return gen.call(gen.prototype);			//返回的是生成器对象
}
var f=new F();

f.next()	//{value: 2, done: false}
f.next()	//{value: 3, done: false}
f.next()	//{value: undefined, done: true}
f.a	//1
f.b	//2
f.c	//3
```

#### 4.Tick Tock

生成器函数实现

```js
var clock=function* (){
    while(true){
        console.log('Tick');
        yield;
        console.log('Tock');
        yield;
    }
};
clock().next()
```



# 53 什么是异步

## 53.1 什么是异步

一个任务被人分成两段：文件的读取上

向后台服务器发送一个请求

for循环就是个同步

#### 1. 回调函数callback

![image-20201208150532333](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201208150532333.png)



进程依赖于程序，线程存在同个内存地址

操作系统只管进程

#### 2. generator 函数协程

1.数据交换

2.错误机制

## 53.2 Trunk函数  没学

## 53.3 Promise对象

```js
const promise=new Promise(function (resolve,reject){
    if(成功){
        resolve(value);
    }else{
        reject(error);
    }
});

promise.then(function fn_resolve(value){
    //success
},function fn_reject(error){
    //failure
} );
```

1. resolve 从未完成变成成功

2. reject    从未完成变为失败

3. pading代表未完成，进行中

4. then 只要上面条件做完，输出了状态，无论是成功还是失败都调用then

5. 要么成功，要么失败

6. 可以写两个then，但是要么都是resolve被激活，要么都是reject被激活

    

#### Promise流程图

![image-20201208153534454](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201208153534454.png)

#### 状态嵌套的概念

```js
const p1=new Promise((resolve,reject)=>{
 	setTimeout(()=>{												//顺序
        console.log("p1被激活");									//1
        reject(new Error("error"));								//2
    },3000)   
})

const p2=new Promise((resolve,reject)=>{
 	setTimeout(()=>{
        reject(p1);		//p1先返回结果才可以执行p2
    },1000)   
})

p2.then(
    value=>{console.log(value)},	
    err=>{console.log(err);console.log("p2激活")}						//3
)
```

![image-20201208155938589](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201208155938589.png)

# 54 Promise方法 可再看

## 54.1 then方法

1.then方法返回的是新的Promise实例

2.会将返回结果作为参数。

3.把一个状态作为另一个状态的输入：p1抛啥p2就是啥↓

```js
const p1=new Promise((resolve,reject)=>{
 	setTimeout(()=>{												//顺序
        console.log("p1被激活");									//1
        reject("p1被激活");							
    },3000)   ++++++++++++++++
})

const p2=new Promise((resolve,reject)=>{
 	setTimeout(()=>{
        resolve(p1);		//p1先返回结果才可以执行p2
    },1000)   
})

p2.then(
    value=>{console.log(value)},	
    err=>{console.log("2");return 'aa';}										//2
).then(
     value=>{console.log("resolve"+value);},									//3
	    err=>{console.log("reject"+value);}				
)
```

4. value err只能执行一个，代码算完了就是执行下面的resolve

```js
const p1=new Promise((resolve,reject)=>{
 	setTimeout(()=>{												//顺序
        console.log("p1被激活");									//1 p1被激活
        resolve("p1被激活");							
    },3000)   
})

const p2=new Promise((resolve,reject)=>{
 	setTimeout(()=>{
        resolve(p1);		//p1先返回结果才可以执行p2
    },1000)   
})

p2.then(	
    value=>{console.log(value)},								//2 p1被激活
    err=>{console.log("2");return 'aa';}									
).then(
     value=>{console.log("resolve"+value);},		//3 resolve undefined
	    err=>{console.log("reject"+value);}				
)
```

**5.与逻辑**     then的执行结果

++=+

-+=-

+-=-

--=-

6.但是第二个then都是成功的，因为第一个then返回成功了，所以就往成功里面传。

7.上面的then要返回一个具体值才能到下方去

```js
const p1=new Promise((resolve,reject)=>{
 	setTimeout(()=>{												
        console.log("p1被激活");								
        resolve("p1被激活");							
    },3000)   
})

const p2=new Promise((resolve,reject)=>{
 	setTimeout(()=>{
        resolve(p1);		
    },1000)   
})

p2.then(	
    value=>{console.log(value);
           return new Promise((resolve,reject)=>{
                	setTimeout(()=>{
     			   	resolve("9257");		
   				 },1000)   
           }).then(
               value=>value
           )},								
    err=>{console.log("2");return 'aa';}									
).then(
     value=>{console.log("resolve"+value);},								
	    err=>{console.log("reject"+value);}				
)
```

返回的:1.数据,2.状态

逻辑,类似0伪代码

```js
obj.promise.then=function(){
   switch(this.['[[PromiseStatus]]']) {
       case"resolved":
           arguments[0].call(null,this.['[[PromiseValue]]']);
           break;
       case "rejected":
           arguments[0].call(null,this.['[[PromiseValue]]']);
           break;
   }
}
```

描述符[[]]

## 54.2 catch方法

Promise.prototype.catch 方法是.then(null,rejected)或.then(undefined,rejected)的别名.(不期望他输正确)

resolve等后面无意义.

promise对象的错误具有冒泡性质,会一直传递直到错误为止,但是会被catch语句捕获

### promise的内部错误

referenceError	引用错误.

**不会传递到外层代码,即没有任何反应.**

```js
someAsync = ()=>{	//箭头函数的定义方式
    return new Promise((resolve,reject)=>{
      resolve(x+2);							//bug了,但是假如输出000000,就没有爆出来
    });
}

someAsync().then{
    ()=>{
        console.log("321123");
    },
    ()=>{									//像是一个catch
        console.log("000000");				//000000
    }
}
```

## 54.3finally方法

1.不管Promise最后的状态,都会执行finally.

2.会返回原来的值,与状态无关,不依赖于Promise.

3.意思就是总结,收尾工作.

### 54.4.1  promise.all

静态方法,不需要继承下去

```js
const p=Promise.all([p1,p2,p3])
```

新的promise,串联对象. 

1.所有变成fulfilled,p1p2p3**返回值**会组成一个数组,传递给p的回调函数

2.只要有一个是rejected,p的状态渐变reject,**第一个reject的实例返回值**会传递给p的回调函数

应用:

```js
const promises=[2,3,5,7,11].map(funciton(id){
                                return getJSON('/post/'+id+'.json');
                                });
Promise.all(promise).then(fucniton(posts){
                          
                          }).catch((reason)=>{
    
})
```

1.检测数据的完整性

### 54.4.2  promise.race

```js
const p=Promise.race([p1,p2,p3]);
```

与方法

## 54.5 resolve与reject

### .resolve方法

```js
Promise.resolve('foo');
//等价于
new Promise(resolve=>resolve('foo'))
```

1.参数是一个Promise实例,那么Promise.resolve将不做任何修改,原封不动返回这个实例

2.参数是一个thena对象

3.参数不是具有then方法的对象,或根本不是对象

```js
const p=Promise.resolve('hello');

p.then(function(s){
    console.log(s)
});
//hello
```

生成一个新的Promise对象的实例p

4.是在本轮事件循环的结束时执行,而不是在下一轮"事件循环"开始

### .reject方法

会原封不动的作为reject的输出,变成后续方法参数.

thenable对象原封不动的输出

# 55 Proxy 代理器

## 55.1 proxy概念

用它来代理某些操作。

```js
var proxy=new Proxy(target,handle);		//拦截的东西,操作
```

```js
let person={
    age:18,
    name:"赖赖",
    sex:"男"
}
let proxy=new Proxy(person,{	//target==person,
    get(target,property,prox){	//传进来的对象,属性名称,代理本身prox==proxy(自行操作本身)
        if(property==="age"){
            alert("你确定要知道我的年纪嘛?");
            return `我${target[property]}了`;
        }else{
            return target[property];
        }
    },
    set(target,property,value,prox){	//value==属性值
        if(property==="look"){			//都可以进行拦截
            let len=target[property];
            let toggle=value.split("帅哥").jion("");
            if(toggle.length<len){
                target[property]=value;
            }else{
                console.log("error!!");
            }
        }
    },
   	apply(target,obj,args){
        console.log(obj);		//person is not function
    }
})

console.log(proxy);	//Proxy {age: 18, name: "赖赖", sex: "男"}			输出的里面已经是新对象了
console.log(proxy.age);		//我18了
console.log(person.age);	//18
```

1.是创造出一个新对象(深复制)

2.只能使用代理后的对象,代理后的与原先的已经不一样了,操作的话只能当且仅当新生成的一个类

3.proxy实例也可以作为其他对象的原型对象

## 55.2 proxy拦截操作的方法(可再看一遍)

#### 1.get(target,property,prox)

1.一定要为各种情况定义返回值,不同属性的基本特征

2.链式操作

#### 2.set (target,property,value,prox)

1.都要设置操作,要有反馈

2.严格模式下set如果没有返回true则会报错

#### 3.apply(target,object,args)

1.target  **目标对象得是函数**

2.object 目标对象的上下文对象,(需要手动设置的,this)

3.args 目标对象的参数数组

#### 4.has方法

拦截hasProperty操作.是否存在的操作,对for in 操作不起作用	返回boolean

has(target,propKey)拦截prop.

```js
var handler={
    has(target,key){
        if(key[0]==='-'){
            return false;
        }
        return key in target;
    }
};
var target={_prop:'foo',prop:'foo'};
var proxy=new Proxy(target ,handler);
'_prop' in proxy;	//false
```

#### 5.construct 方法

拦截new命令

接收两个参数:

1.target:目标对象

2.args:构造函数的参数对象

​			construct方法返回的必须是一个对象,否则会报错.

```js
construct(tar,args,newTar){
    console.log(newTar);
    return {};
}
```

#### 6.deleteProperty方法

拦截delete操作.返回boolean值 迭代方法就是true

拦截

#### 7.getOwnPropertyDescriptor 方法

拦截Object.getOwnPropertyDescriptor(),返回一个属性描述对象或者undefined.

用#符号定义自由属性

静态属性,只允许构造函数访问

自由属性,只允许this内部

#### 8.getPrototypeof 拦截对象原型

返回是对象

拦截以下操作

```js
Object.prototype._proto_
Object.prototype.isPrototypeOf()
Object.getPrototypeOf()
Reflect.getPrototypeOf()
instanceof
```

#### 9.definedPrototype()

返回的是对象

#### 10.isExtensible()方法

返回值与原始方法相同

#### 11.ownKeys()

拦截对象自身属性的读取操作

拦截以下操作

```js
Object.getOwnPropertyNames()
Object.getOwnPropertySymbols()
Object.keys()
for...in循环
```

返回不报错,目标对象自身包含不可配置的属性,该属性必须被ownKeys方法返回,否则报错.

#### 12. Proxy.revocable()方法  

不可被继承的,静态方法

#### 13.setPrototypeof 拦截对象原型

### 55.3 proxy的this问题

不做任何拦截的情况下,也无法保证与目标对象的行为一致.

指向实例本身而不是原始对象

# 56 Reflect

## 56.1 Reflect的静态方法

通过构造函数引用某些方法来实现其功能

1.第一个要大写

2.不能用new,和math很像,提供一个静态api接口 简单的方式来调用功能.

完全独立的,通用性函数引用.	

### 1.Reflect.get(target,name,receiver)

返回target对象的name属性

改变了一个this的指向问题.

```js
let obj={
    name:'mary',
    age:18,
    sayNAme(){
        console.log(this.name)
    },
    get getAge(){
        return this.age;
    }
}

let receiver={
    name:'yingshi',
    age:998
}
let result1=Reflect.get(obj,'getAge');	//18
let result=Reflect.get(obj,'getAge',receiver);	//998
									//将this指向成第三个值
```

完全将功能和数据独立开.让数据成为数据,方法成为方法.

### 2.Reflect.defineProperty(target,propertyKey,attributes)

可能被废除														对象	,												,value

可以拦截操作

### 3.Reflect.set(target,name,value,receiver) 

receiver 也是改变this的指向

设置target对象的name属性等于value

```js
var myObject={
    foo:4,
    set bar(value){
        return this.foo=value;	//返回的是value	返回值的本身
    },
};
var myReceiverObject={
    foo:0,
};
Reflect.set(myObject,'bar',3,myReceiverObject);
myObject.foo;//4
myReceiverObject.foo;//1
```

Proxy和Reflect联合使用,Reflect.set会触发Proxy.defineProperty拦截.

```js
let hander={
    set(target,key,value,receiver){
        console.log('set');
        Reflect.set(target,key,value,receiver);
    },
    defineProperty(target,key,attribute){
        cosnole.log('defineProperty');
        Reflect.defineProperty(target,key,attribute);
    }
};

let obj=new Proxy(p,handler);
obj.a='A';
//set
//defineProperty

console.log(obj.a)	//A
```

### 4.Reflect.has(obj,name)

相当于name in obj里面的in运算符

```js
var myObject={
    foo:1,
};
//旧语法
'foo' in myObject	//true
//新语法
Reflect.has(myObject,'foo')	//true
```

第一个参数不是对象的话会报错

### 5.Reflect.deleteProperty(obj,name)

Reflect.deleteProperty等同于delete obj[name], 用于删除对象属性.

```js
var myObject={
    foo:'bar'
};
//旧语法
delete myObj.foo;
//新语法
Reflect.deleteProperty(myObject,'foo');
```

1.删除成功或者属性不存在返回true , 删除失败属性或依然存在返回false

2.设置configurable 是否删除,则会报错

### 6.Reflect.construct(target,args)

```js
function fn(name){
    this.name=name;
}
Reflect.construct(fn,['mary']);	//至少是类数组对象  arg其实是...arg,所以要[args]
```

### 7.Reflect.getPrototypeOf(obj)

读取对象的_proto_属性

```js
const myObj=new FancyThing();
//旧
Object.getPrototype(myObj)===FancyThing.prototype;
//新
Reflect.getPrototype(myObj)===FancyThing.prototype;
```

2.如果参数不是对象,Object会将它变为对象,Reflect会报错.

### 8.Reflect.setPrototypeOf(obj,newProto)

```js
Reflect.setPrototypeOf(1,{})
```

不可拓展,会报错

处理bug

### 9.Reflect.apply(func,thisArg,args)

Reflect.apply方法等同于Function.prototype.apply.call(func,thisArg,args),用于绑定this对象后执行给定函数

```js
const ages=[11,33,23,46,67,45];

//旧写法
const youngest=Math.min.apply(Math,ages);
//新写法
const yongest=Reflect.apply(Math.min,Math,ages);
```

简化操作

### 10.Reflect.getOwnPropertyDescriptor(target,propertyKey)

1.基本等同于Object.getOwnPropertyDescriptor,用于得到指定属性的描述对象

2.如果第一个参数不是对象,object 不会报错,Reflect会报错

```js
var myObject={};
Object.defineProperty(myObject,'hidden',{
    value:true,
    enumerable:false,
})

//旧
var theDescriptor=Object.getOwnPropertyDescriptor(myObject,'hidden');
//新
var theDescriptor=Reflect.getOwnPropertyDescriptor(myObject,'hidden');
```

### 11.Relect.isExtensible(target)

1.对应Object.isExtensible,返回一个boolean ,表示当前对象是否拓展

```js
const myObject={};
//旧
Object.isExtensible(myObject);
//新
Reflect.isExtensible(myObject);
```

2.如果参数不是对象,Object.isExtensible会返回false,因为非对象本来就是不可扩展的,而Reflect.isExtensible会报错

```js
//旧
Object.isExtensible(1);		//false
//新
Reflect.isExtensible(1);	//报错
```

### 12.Reflect.proventExtensions(target)

1.Reflect.proventExtensions对应Object.proventExtensions

2.让对象变为不可拓展,返回boolean,表示是否操作成功

```js
const myObject={};
//旧
Object.proventExtensions(myObject);			//Object{}
//新
Reflect.proventExtensions(myObject);		//true
```

3.如果参数不是对象,Object.preventExtensions在ES5环境报错,在ES6返回传入参数,Reflect.proventExtensions会报错

```js
Object.proventExtensions(1);			//error			es5
Object.proventExtensions(1);			//1				es6
Reflect.proventExtensions(1);			//error			新语法
```

### 13.Reflect.ownKeys(target)

1.返回对象所有属性, 基本等同于Object.getOwnPropertyNames与Object.getOwnPropertySymbols之和.

2.如果第一个参数不是对象会报错

```js
var myObject={
    foo:1,
    bar:2,
    [Symbol.for('baz')]:3,
    [Symbol.for('bing')]:4
};
//旧语法
Object.getOwnPropertyNames(myObject);		//[ 'foo', 'bar' ]
Object.getOwnPropertySymbols(myObject);		//[ Symbol(baz), Symbol(bing) ]
//新语法
Reflect.ownKeys(myObject);					//[ 'foo', 'bar', Symbol(baz), Symbol(bing) ]
```

## 56.2 Reflect核心总结

好处:

1.将Object一些明显属于内部的方法,放到Reflect上.

未来的新方法将只部署在Reflect上,会一步步的迁移

2.修改了object的返回结果

3.让Object操作变成函数行为(之前的都是命令式的)

4.让Reflect和proxy进行组合.(05:58)

5.让方法变得更加易懂了.

## 56.3 观察者设计模式示例

### 例子

```js
const queueObservers=new Set();
const observe=fn => queueObservers.add(fn);
const observable =obj =>new Proxy(obj,{set});       //建立第三方

function set(target,key,value,receiver){
    const result=Reflect.set(target,key,value,receiver);        //会在设置值的同时调用一系列的函数
    queueObservers.forEach((item)=>item());
    return result;
}

const person=observable({           //对第三方进行操作,从而拦截下来
    name:"mary",
    age:18
});

function print(){
    console.log(`${person.name}${person.age}`);
}

observe(print);

person.name="tom"
```























