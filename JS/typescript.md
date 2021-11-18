# typescript

他是编译源,所以鼠标放上去就有提示.更适合企业化的开发

机器语言,汇编语言(助记符,高级语言

### 1.安装

1.用管理员身份运行cmd

2.输入指令

```cmd
npm install -g typescript
```

3.查看版本号

```cmd
tsc
```

### 2.使用

1.在vsCode里面用终端的打开,打开后路径已经设置好了

2.若没有设置,则cd 路径即可

### 3.编译

在终端上写 **tsc  文件名.ts** 即可.他会自动编译成js文件.

如果编译的是js,则可以在终端上写 **node  文件名.js** 即可.

## ①注意事项

1.必须使用管理员身份运行

2.第一次终端运行vscode的时候报错

![image-20201222180706805](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201222180706805.png)

原因是因为没有使用权限;

解决方法:

1. 以管理员身份运行PowerShell

2.  执行：get-ExecutionPolicy，回复Restricted，表示状态是禁止的

3. 执行：set-ExecutionPolicy RemoteSigned

4. 选择Y

   **注意：一定要以管理员的身份运行PowerShell，不是cmd窗口！是windows里的另一个脚本环境**

![image-20201222181259892](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201222181259892.png)

# 1.基础变量声明

```ts
let [变量]:[类型]=值;

function sayName(name:string):any{	//any 为返回值类型	
    
}
```

1.如果没有定义好类型的话,就强行根据基础数型来赋类型.锁死类型后不能更改.

2.undefined是任何一个类型的子类型,所以都能undefined.

# 2.数据类型

### 1.元祖类型Tunple

```ts
let a:[string,number,number,object,boolean];	//初始值的类型
```

不能多不能少也不能变.

### 2.枚举类型enum

如果看不懂新语法则编译一下,看看到js里面是什么

ts

```ts
enum color {x,y,z}
```

不加编号的话就从第一个往下走,如果加了的话则是0,第二个值,第二个值+1;

js

```js
(function (color) {
    color[color["x"] = 0] = "x";
    color[color["y"] = 1] = "y";
    color[color["z"] = 2] = "z";
})(color || (color = {}));

console.log(color);	//{ '0': 'x', '1': 'y', '2': 'z', x: 0, y: 1, z: 2 }	值-名-值
```

```js
{x:1}	//[object Object]
```

### 3.任意类型any

这些值可能来自于**动态的内容**，比如来自**用户输入或第三方代码库**。 这种情况下，我们**不**希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。

1.创建变量不赋值和赋undefined都是any数据类型.你可以不赋值或者赋值任意类型.

```typescript
let a:any=null;
a=1;
```

2.any用的多的都是方法放回值

```ts
function sayName(name:string):any{	//any 为返回值类型	
}
```

### 4.无类型void

定义好后只能往里面传入undefined和null,其他都不行.

但是undefined和null又不仅仅是放在void里.

undefined和null都是任何类型的子类型

### 5.Never类型

1.never表示永远不存在值的类型.意思是根本不用返回值,定义就是抛出错误的,所以用never.

```ts
function sayName(name:string):never{	//一般用在函数返回值上.
    throw new Error(name);				//要么是循环,要么报错.
}
```

2.never算得上是任何类型的子类型

### 6.类型断言

只是告诉编译器一定是这个类型.其一是“尖括号”语法：

```ts
let s: any = 123;
let len: number = (<string>s).length
```

另一个为`as`语法：

```ts
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

### 7.数组

 第一种，可以在元素类型后面接上`[]`，表示由此类型元素组成的一个数组：

```js
let list: number[] = [1, 2, 3];
```

第二种方式是使用数组泛型，`Array<元素类型>`：

```js
let list: Array<number> = [1, 2, 3];
```



### 8.

# 3.接口

接口就是类型命名和代码或第三方代码的定义契约.

接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法。

```ts
function sayName(name:{name:string}){
    
}
sayName({name:'1'})
```

接口实现:

```ts
interface nameCheck{
    name:string
}

function sayName(name:nameCheck){
    
}
sayName({name:'1'})
```

函数的参数必定是接口需要的参数

传多个参的话

```ts
interface nameCheck{
    name:string
}

function sayName(name:nameCheck,...args){
    
}
sayName({name:'1'},{name:'2'})
```

## 3.1接口特性

### 1.提供可选属性

```ts
interface nameCheck{
    name?:string			//可选参数(打在冒号前)
}

function sayName(name:nameCheck,...args){
    
}
sayName({name:'1'},{name:'2'})
```

#### 优点

1.可以对可能存在的属性进行定义.

2.捕获引用不存在属性时的错误.

### 2.只读属性

```ts
interface nameCheck{
   readonly name:string			//只读属性
}

function sayName(name:nameCheck){
    
}
sayName({name:'1'})
```

### 3.属性检查

```ts
interface nameCheck{
    age:18,
   [property:string]:number		//属性检查,可以从两个参变成无数个参
}

function sayName(name:nameCheck){
    
}
sayName({age:"18"，name:152542})
```

## 3.2接口设计

```ts
interface fnCheck{
    (s:string,n:number):string;
}

let fn:fnCheck=function(a,b){
    return a+b;
}

fn("1",2);
```

### 1.索引类型

```ts

```

数字索引的返回值必须是字符串索引返回值类型的子类型

### 2.只读

### 3.类class类型

类描述了所创建的对象共同的属性和方法。

类的检查都是检查属性和方法,对于其他的没有太大的基本要求.

```ts
interface classCheck{
    clockDate:Date;
    getDate():Date;
}

class clock implements classCheck{		//implements 接口
    clockDate:Date;
    getDate(){
        return new Date();
    }
}

```

### 4.继承接口

```ts
interface Shape{
    color:string;
}

interface Square extends Shape{
    sideLength:number;
}

ler square:Square={
    color:64,		//报错,因为Coloir的值必须是string
    sideLength:18
}
```

也可以拓展很多个:

```ts
interface Shape{
    color:string;
}

interface PenStroke{
    penWidth:number
}

interface Square extends Shape,PenStroke{
    sideLength:number;
}
```

### 5.混合类型

```ts
interface Counter{
    (start:number):string;	//有一个函数,参数是start,值是number参数,返回string

	interval:number	//一个值是数字的属性
    
    reset():void;	//没有返回值的函数
}

function getCounter():Counter{
    let counter=<Counter>function(start:number){
        return "123456";
    }
    counter.interval=123;
    counter.rest=function(){};
    return counter;
}

let c=getCounter();
c(10);
c.reset();
c.intercal=5.0;
```

### 6.接口继承类

```ts
class Control{
    private state:any;
}
interface SelectableControl extends Control{
    select():void;
}

class Button extends Control implements SelectableControl{
    selet(){}
}

class Image implements SelectableControl{
    select(){};
}
```

control和interface很相似

# 4 公共,私有与受保护的修饰符

### 1.私有private

私有**:只能内部进行this调用**.实例和类都不能直接调用

```ts
class Animal{
    public name:string;
    public constructor(theName:string){
        this.name=theName;
    }
    private move(dis:number){
        console.log(`${this.name}移动了${dis}m`)
    }
    private age:18;
    public sayAge(){
        console.log(this.age);
        this.move(1314);
    }
}

let obj= new Animal("熊猫");
obj.move(1314);
```

想两个对象赋值能实现的话:

1.公用的话同名就行

2.私有的 都不行.

### 2.保护修饰符 protected

在构造函数里写protected,你就没办法写new,但是可以继承下去.

### 3.只读修饰符 readonly

只能在创造或申明时被初始化

```ts
class Octopus{
    readonly name:string;
}
```

### 4.静态属性 static

不能通过this来调用.

### 5.抽象类 abstract

定义抽象类和抽象类的方法

```ts
abstract class Animal{		//抽象类作为基类
    public name:string;
    public constructor(theName:string){
        this.name=theName;
    }
    public sayName():void{		//函数实现缺失或未立即出现在声明之后
        console.log(this.name);
    }
    abstract sayAge():void;	//不能有具体实现,因为是标记的
}

class Person extends Animal{
    constructor(){
        super("熊猫");
    }
    sayAge(){
        console.log(18);
    }
    sayHello(){				//新创造的不能用,但是可以存在
        console.log("hello world");
    }
}
     
let obj1:Animal;
obj1=new Person();
console.log(obj1);		//Person { name: '熊猫' }
```

## 4.1高级技巧

### 1.构造函数 

声明一个类的时候实际上声明了很多东西

```ts
let 变量:class类		//变量必须是class类的实例
let 变量:typeof 类		//变量必须是类本身
类才能使用静态方法.
```

### 2.把类当做接口使用

因为类可以创建出类型,所以可以允许使用接口的地方使用类

```ts
class Point{
    x:number;
    y:number;
}

interface Point3d extends Point{
    z:number;
}

let point3ds:Point3d={x:1,y:2,z:3};
```

# 5 函数类型

## 5.1 函数类型定义

变量想要定义的话:

```ts
let myAddFn:(x:number,y:number)=>number;

myAddFn=function(x,y){
    return x+y;
}

//连着一块写也行,但是容易误解
let myAddFn:(x:number,y:number)=>number=(x,y)=>(x+y);
										//←变量声明的部分	//右边则是对函数进行具体设置
```

1.包含两部分:参数类型和返回值类型

2.要进行个数和类型匹配就够了,名字可以不一样.

3.无返回值的话一定要强行规定void

4.推断类型:一边指定类型另外一边没有指定的话,就会推论.

## 5.2 可选参数和默认参数

1.严格模式下不可以用默认参数

2.js参数不严格.js函数体内的具体代码不一定和函数相关.ts多一个少一个都不行,否则报错

3.可选 也是参数后打问号.

​			1.可选参数必须跟在其他参数后面(和默认参数差不多)

​			2.可选参数就是输出undefined

```ts
function buildName(firstName:number,secondName?:number){
    console.log(firstName,secondName);
}
buildName(1);		//1 undefined
```

## 5.3 ts的函数

### 1.剩余参数

```ts
function buildName(firstName:number,...restName:string[]){
    return firstName+" "+restName.join(" ");
}
let employee=buildName(1,"234","343","43e");	
```

### 2.this

this的指向和es无区别

1.普通函数:调用该函数的对象

2.箭头函数:生成或定义时函数所在的环境对象

### 3.重载

## 5.4泛型

泛指的类型

```ts
function identity(arg:any):any{
    return arg;
}
```



```ts
function fn(x:any):any{
    return x+" ";
}
fn("2");
```

### 解决方案

<字母>  类型,但是不代表任何具体的类型

```ts
function fn<T>(x:T):T{
    return x+" ";	//报错了 不能把string类型分配给类型T
    //约束返回类型,必须属于无办法预估的
    return x;
}
fn("2");
```

### 常见用法

```ts
let output=indetity<string>("myString");	//运行函数时手动跟上类型
let output=indetity("myString");			//编译器推断(更常用)
```

## 5.5使用泛型

```ts
function fn<T>(x:T):T{
    console.log(x.length);	//bug
    return x;
}
```

组合,可以约束传进来项目的类型

```ts
function fn<T>(arg:Array<T>):T{
    console.log(x.length);		//接收类型是T和参数arg,arg是数组项目类型是T的数组
    return x[0];
}
fn([1]);
```

```ts
function fn<T>(X:Array<T>):T
```

### 泛型函数

```ts
function 函数名 <泛型>(参数要求):返回值类型
```

### 泛型接口

```ts
interface Ifn{
    <T>(arg:T):T;
}
funciton identity<T>(arg:T):T{
    return arg;
}
let myIdentity:Ifn=identity;

//↑与↓下效果完全一致
let myIdentity:<U>(arg:U)=>U=function identity<T>(arg:T):T{
    return arg;
}
```

```ts
class fn<T>{
    zeroValue:T;
    add:(x:T,y:T)=>T
}
let myGen=new Gen<number>();
```

# 6.枚举

## 6.1枚举类型

名值对的双向映射

```ts
enum obj{
    a,
    b,
    c
}
```

#### 初始化器

1.要在枚举里给某一个值设定初始化器的时候,这个数字作为下个成员的基准值,所以不允许通过函数来实现

2.如果没用初始化器,则从0开始.

```ts
getValue(){
    return 123;
}
enum obj{
    a=0,
    b=1,
    c=getValue()			//不报错
}				
```

#### 字符串枚举

1.如果用字符串的话,每一个成员都使用字符串变量

2.数字实行双向映射,字符串不是

```ts
enum obj{
    a="a",
    b=2,
    c="c"
}					//{ '2': 'b', a: 'a', b: 2, c: 'c' }
```

3.只能第一个是空着的,默认的话是数字和string映射,以此基础上再命名

#### 异构枚举

混合字符串和数字成员,不建议

#### 计算成员

1.第一个没有初始化器,则定义为0

 2.带括号的枚举表达式:返回值是常量,但是初始化没法用函数的返回值

3.不能为NaN或Infinity

#### 联合枚举

1.字面量枚举成员,不带初始值的常量枚举成员

2.把枚举类型用在类的限制上

```ts
enum ShapeKind{
    circle,
    square
}

interface circle{
    kind:ShapeKind.circle;		//这个值被定死了
    radius:number;				//值的类型
}

interface square{
    kind:ShapeKind.square;
    idoLength:number;
}

let c:circle{
    kind:ShapeKind.Square;	//~~~~~~~~~~~~~~~~error
    radius:100;
}
```

3.枚举类型本身变成每个枚举成员的联合

```ts
enum E{
    Foo,
    Bar
}

function f(x:E){
    if(x!==E.Foo||x!==E.Bar){//要么是E.Foo要么就是E.Bar,逻辑浪费,因为定义时已经限制了他是枚举类型E了
        //error	此条件将始终返回 "true"，因为类型 "E.Foo" 和 "E.Bar" 没有重叠。				
    }
}
```

```ts
enum E{x,y,z}		//x=0,'0'=x		名值对,值名队
function f(obj:{x:number}){
    return obj.x;
}
f(E);	//完全没有问题的,E里面就是有一个值为数字的x属性
```

#### 反向映射

正向映射(name->value)和反向映射(value->name)

**!不会为字符串枚举成员生成反向映射**

#### const 常量枚举

只能属性的访问,而且在编译后被删的渣都不剩.

只是方便程序在编译时方便

```js
const enum obj{
    a,b
}
console.log(obj)//error. "const" 枚举仅可在属性、索引访问表达式、导入声明的右侧、导出分配或类型查询中使用
```

常量枚举在js里会有注释:

ts↓

```ts
const enum obj{
    a,b					//产生了双向映射
}
let objs=[obj.a,obj.b];
console.log(objs);
```

js↓,深拷贝,反向debug会有用

```js
var objs = [0 /* a */, 1 /* b */];		//会有注释
console.log(objs);
```

#### 外部枚举

没有初始化的就会当成常量成员

```ts
declare enum obj{
    a,
    b=1,
    c=2,
}
console.log(obj.a)		//在js内则为空,就是空着.但是假如是正常的枚举的话则输出0
```

# 7类型推论

## 7.1类型推论

编译系统自动加上类型,就是类型推论.在值的赋予过程中没有赋值类型,则编译系统会辅助推论.

```ts
let x=3;
x="s";		//error,基础类型是number
```

#### 最佳通用类型

```ts
let x=[0,1,null];		//类型是x:number[],null和undefined是所有类型的子类型
x.push("1")			//error,类型“string”的参数不能赋给类型“number”的参数。
```

#### 上下文类型推论

事件,函数,参数

1.根据上文

```ts
window.onmousedown=function(mouseEvent){	//通过window.onmousedown判断右边函数表达式的类型
    console.log(mouseEvent.button);
}
```

2.根据下文

```ts
window.onmousedown=function(mouseEvent:any){	
    console.log(mouseEvent.button);
}
```

总结:

1.有上文无下文,根据上文推断下文.有上下文,根据下文设置的为主.

2.保证类型通用,都兼容,

## 7.2类型兼容

### 名义兼容:不怎么使用.

### 结构性兼容

#### 结构子类型(函数参数比较)

**函数参数可以少不能多**

基于结构子类型,使用其成员来描述类型的方式.类型是否兼容看成员是否使用此类型的方式.

1.如果直接赋值的话就会报错:

```ts
interface named{
    name:string
}
let x:named;
x={name:"helloworld",age:18}//error,对象文字可以只指定已知属性，并且“age”不在类型“named”中。
```

2.但是曲线救国的话,多一个也可以赋值.

```ts
interface named{
    name:string
}
let x:named;
let y={name:"helloworld",age:18};
x=y;	//不报错
console.log(x);		//{ name: 'helloworld', age: 18 }
```

对两个变量进行匹配,首先看接口,拿值和接口比.接口和接口比不一样的.

3.但是少一个就会报错:

```ts
interface named{			//需要的
    name:string,
    age:number,
    sex:string
}
let x:named;			
let y={name:"helloworld",age:18};		//实际给的
x=y;	                             //error!,只能大的赋值给小,小不能赋值给大的.
//类型 "{ name: string; age: number; }" 中缺少属性 "sex"，但类型 "named" 中需要该属性。ts(2741)
console.log(x);
```

**实际给的一定要大于需要的,否则会报错.**

**函数参数可以少不能多**

#### 函数类型比较

**返回值:可以多不可以少**.   对象也是可多不可少

源函数的返回值类型必须是目标函数返回值类型的子类型

```ts
let x=()=>({name:'Alice'});
let y=()=>({name:'Alice',location:'Seattle'});

x=y;		//OK		x为源函数,y为目标函数
y=x;		//error
```

#### 可选参数及剩余参数

可选参数和必选参数可以互换

#### 枚举类型的比较

1.枚举类型与数字类型兼容

2.不同枚举类型之间是不兼容的

```ts
enum Status{Ready,Waiting};
enum Color{Red,Blue,Green};

let s=Status.Ready;
s=3;//没问题   枚举类型与数字类型兼容
s=Color.Green;		//Error 不能将类型“Color.Green”分配给类型“Status”。不同枚举类型之间是不兼容的
```

#### 类的类型的比较

静态属性,私有属性,受保护成员

1.静态成员和构造函数不在比较范围内

2.如果包含私有成员,源类型必须包含同一个类的私有成员

#### 泛型的类型兼容性

1.只要接口里没用到泛型,则可以兼容,

```ts
interface Empty<T>{}
let x:Empty<string>;
let y:Empty<number>;
x=y;					//不报错
```

2.只要接口里用到了泛型,则不能兼容

```ts
interface Empty<T>{
    data:T;
}
let x:Empty<string>;
let y:Empty<number>;
x=y;					//错误,因为类型不同
```

在激活泛型的一瞬间,假如不一样的话就是不同的

3.写了一个泛型的话,泛型都是相等的(不激活).最初状态:全部为any.

#### 子类型与赋值的理论辨析

类型兼容性是由赋值兼容性来控制的,即使在implements和extends语句也不例外.

根据结构来看的:包括量和名**,能多不能少,能少不能多!重点**

# 9 ts高级

## 9.1交叉类型

主要是创建新函数

&  交叉,取并集

并集,留的是名

多种类型叠加成为一种类型,包含所需所有类型的特性

```ts
function extend<T,U>(first:T,second:U):T&U{	//T & U
	let result=<T&U>{};
	for(let id in first){
	(<any>result)[id]=(<any>first)[id];
	}
	for(let id in second){
	if(!result.hasOwnProperty(id)){
		(<any>result)[id]=(<any>first)[id];
		}
	}
	return result;
}
class Person{
	constructor(public name:string){}
}
interface Loggable{
	log():void;
}
class ConsoleLogger implements Loggable{
	log(){
		//...
	}
}

var jim=extend(new Person("Jin"),new ConsoleLogger());
ver n=jim.name;
jim.log();
```

## 9.2 联合类型

|  联合,和或很像

1.基本数据类型

```ts
function padLeft(value:string ,padding:sting|number)
```

2.如果里是对象,则里面则是取交叉集:(需要的是共有的成员)

```ts
interface Bird{
    fly();
    layEggs();
}
interface Fish{
    swim();
    layEggs();
}
function getSmallPet():Fish|Bird{
    return{
        layEggs(){},
        swim(){},
        fly(){}
    };
}

let pet=getSmallPet();
pet.layEggs();		//ok
pet.swim();			//error 类型“Bird | Fish”上不存在属性“swim”。类型“Bird”上不存在属性“swim”。
```

## 9.3 类型保护与区分类型

### 1.类型断言

实际上有的,可以使用类型断言的方式来强行指定类型来绕过排查机制.

```ts
if(<Fish>pet).swim{
    (<Fish>pet.swim());
}
```

### 2.用户自定义的类型保护

pet is Fish		类型谓词,定义了属性名或参数名是某一种类型

```ts
interface Bird{
    fly();
    layEggs();
}
interface Fish{
    swim();
    layEggs();
}
function getSmallPet():Fish|Bird{
    return{
        layEggs(){},
        swim(){},
        fly(){}
    };
}

let pet=getSmallPet();
function isFish(oet:Fish|Bird):pet is Fish{
    return(<Fish>pet).swim !==undefined;
}
if(isFish(pet)){
    pet.swim();
}else{
    pet.fly();
}
```

### 3.instanceof类型保护

分析出来属于哪个构造函数,来进行细化

### 4.null,undefined 严格空值检查

1.严格空值检查:不会自动包含null或undefined.

```ts
tsc demo.ts --strictNullChecks
```

2.可以手动设置

```ts
let x:Name|undefined|null;
```

没有严格空值检查的时候,系统自动在后面加undefined和null

3.手动设置的话undefined和null会有相等问题:严格空值检查内undefined和null不同.

按照js的语义,ts会把null和undefined区别对待.  string|null,  string|undefined,  stirng|undefined|null是不同的类型

### 5.可以为null类型的可选参数

没有严格空值检查时:

```ts
function test(x:number,y?:string):void{
    
}
test(1,undefined);
test(1,null);
//都可以使函数正确运行,兼容
```

但是严格空值检查的话,就不能使用null. 只能undefined.

```ts
function test(x:number,y?:string):void{
    
}
test(1,null);	//error
/*
demo1.ts:4:8 - error TS2345: Argument of type 'null' is not assignable to parameter of type 'string | undefined'.

4 test(1,null);
         ~~~~


Found 1 error.
*/
```

### 6.如果需要null的类型保护

1.自己写入null

2.短运算符进行实现

### 7.类型断言手动去除null或undefined.

!后缀

```ts
function fixed(name:string|null):string{
    function postfix(epithet:string){
        return name!.charAr(0)+'.the'+epithet;//ok
    }			//使用感叹号手动去除null和undefined
    name=name||"Bob";
    return postfix("great");
}
```

ex:

```ts
function test(y?:string|null){
    function demo(x:string|undefined):void{	//string不兼容undefined   x:string
        
    }
    return demo(undefined);
}
```

### 8.类型别名

```ts
type 新类型名称=原始类型名称;
```

创建一个新 名字来引用那个类型. 就用起来方便

```ts
type Name=string;
function test(x:Name){}
```



```ts
type container<T>={value:T};
type Tree<T>={					//无线嵌套
	value:T;
	left:Tree<T>;
	right:Tree<T>;
}
type Yikes=Array<Yikes>			
```

# 10 高级类型

## 10.1 字符串字面量类型

#### 1.type可以创造类型

```ts
type userName="female"|"male";
let name:userName=""			//值会被限制的非常死,所以可以进行限制.
```

#### 2.字符串字面量类型还可以区分函数重载

```ts
function createElement(tagName:"img"):HTMLImageElement;
function createElement(tagName:"input"):HTMLInputElement;

function createElement(tagName:string):Element{
    return;
}
```

#### 3.数字字面量类型

```ts
function test(x:1|2|3|4):void{}
```

#### 4.可辨识联合

加入一个string

```ts
interface Square{
    kind:"Square";
    size:number;
}
interface Circle{
    kind:"Circle";
    radius:number;
}
type Shape=Square|Circle;		//看的是kind		//可以多,下面函数没有变的情况下是不会报错的
												//1.严格空值检查,2.加never

function area(s:Shape){
    switch(s.kind){				//根据kind来判断 ( 联合取的是交集
            case "Square":return s.size*s.size;
            case "Circle":return s.radius*2*Math.PI;
    }
}
```

2.加never,永远不会返回值的类型.意味着传入其他任何类型都会报错

```ts
type Shape=Square|Circle|Triangle;

function assertNever(x:never):never{
    throw new Error("unexpected object:"+x);
}
function area(s:Shape){
    switch(s.kind){				//根据kind来判断 ( 联合取的是交集
            case "Square":return s.size*s.size;
            case "Circle":return s.radius*2*Math.PI;
        	default:return asserNever(s);// error here if there are missing cases
    }
}
```

#### 5.多态的this类型检查

##### 1.链式操作:

jquery.   :在大的构造函数中间,每一次最后都要return一个this

除非要返回值 否则返回对象本身.

要保证传进来的参数是保证基础要求的

##### 2.继承

保证类扩展,能进行链式操作

#### 6.索引类型Index types

索引是number的值,是索引是string的值的子类型

返回的对象时o对象里的子类.

##### keyof 索引类型查询操作符.  得到的是属性名的类型

目的:查询某个对象的属性名称.用在类型系统里面

```ts
interface Person{
    name:string,
     age:number
}
let person:Person={
    name:'mary',
    age:35
}
let personProps:keyof Person;	//'name'|'age'
```

##### 索引访问操作符

```ts
T[k]		//约束不了,要求返回类型是对象,没法约束的
```

用处

```ts
function getProperty<T,K extends keyof T>(o:T,name:K):T[k]{
    return o[name];
}
```

keyof 和T[K]字符串索引签名进行交互

```ts
interface Map<T>{
    [key:string]:T;
}
let keys:keyof Map<number>		//string
let value:Map<number>['foo']	//number
```

#### 7.映射类型

想立即将大量的参数从必须变成所选

可以另每个属性变成readonly类型或可选的:

```ts
type Readonly_<T>={
    readonly[P in keyof T]:T[P];
}
type Partial_<T>={
    [P in keyof T]?:T[P];
}
type PersonPartial=Partial<Person>;
type ReadonlyPerson=Readonly<Person>;
```

这样就可以在任意情况下改成只读版本的.

查找的话:就是按F12;基本库;

通用版本:按照一定的方式转化字段,这就是keyof和索引访问类型要做的事情

```ts
type Nullable_<T>={[p in keyof T]:T[P]|null}
type Partial_<T>={[p in keyof T]?:T[P]}
```

##### 映射类型的基本讲解:proxy

例子:T[P]被包装在Proxy<T>类里

```ts
type Proxy<T>={				//3.proxy可以接受n多种类型了
    get():T;
	set(value:T):void;
} 
type Proxify<T>={		//1.传入对象
    [P in keyof T]:Proxy<T[P]>;		//首先遍历,然后proxy往里面传入了T[P]	//2.遍历对象,并把对象所有的值作为proxy的一个输入				
}
function proxify<T>(o:T):Proxify<T>{		//得出若干个新的类
    //...
    return ;
}
let props={}
let proxyProps=proxify(props);
```

拆包:

```ts
function unproxify<T>(t:Proxify<T>):T{
    let result={} as T;
    for(const k in t){
        result[k]=t[k].get();
    }
    return result;
}
let originalProps=unproxify(proxyProps);
```

#### 8.预定义

```ts
Exclude<T,U>		T中提出可以赋值个U的类型(差集)
Extract<T,U>		提取可以赋给U的类型(交集)
NonNullable<T>		从T剔除null和undefined(差集)
ReturnType<T>		获取函数返回值的类型:InstanceType<T>,获取实例类型
```

# 11 模块

## 11.1 模块系统

##### 1.内部模块namespace					2.外部模块  模块



#### 2.模块的使用

1.模块在自身作用域里执行, 而不是全局作用域.

2.在方法的ts内有输出,在使用的ts内有引用

3.单模块单文件储存,每一个模块都有输出接口

4.调用一个模块用import进行调用	import   from	//es6

```ts
import {add,math} from "./math"       //引用
```

```ts
//math.ts
function add(...args:number[]):number{
    let sum:number=0;
    sum=args.reduce(function(pre,cur):number{
        return pre+cur;
    },sum);
    return sum;
}
export {add};       //输出

//file.ts
import {add} from "./math"       //引用
add(12,23,4,5,6);
```

#### 3.模块的导出

```ts
export {add};       //输出
```

1.可以分屏

2.上下切分

3.在module下创建多个ts,只有主demo在外面

```ts
//主demo
import {userMsg}from "./module/file"
console.log(userMsg);

//math.ts
function add(...args:number[]):number{
    let sum:number=0;
    sum=args.reduce(function(pre,cur):number{
        return pre+cur;
    },sum);
    return sum;
}
export {add};       //输出

//file.ts
import {add} from "./math"       //引用
import{userMsg} from "./head"
add(12,23,4,5,6);
let person:userMsg;
person={                    //强制性,多一个不行少一个也不行.
    name:'mary',
    age:18,
    sex:"female"
}
export {person as userMsg};		//!改变名字!让主demo的名字更加清晰

//head.ts
interface userMsg{
    name:string;
    age:number;
    sex:string;
}
export {userMsg};
```

4.可以同时实现导入和导出:

```ts
export {foo,bar} from 'my_module'

//可以理解为
import {foo,bar} from 'my_module'
export {foo,bar} 
```

5.模块导出的东西不允许变的

6.模块重新导出

```ts
export * from 'my_module'	//合并成一个对象倒出去 Object.XXX
```

```ts
import * as add from 'my_module'//将所有的方法作为add对象,将导入的模块系统放入一个对象的方法下.导入的是数据则是方法下的一个属性,导入的是函数则是对象下的方法.
```

#### 4.默认导出

```ts
export default function(){	//预先设定好导出的什么,默认输出只有一个
    console.log('hello')
}
//import写的对不对不知道
```

#### 5.模块代码,模块系统

```ts
tsc --model common.js demo1.ts
```

# 15 tsconfig

支持glob通配符的有

```text
*		匹配0或多个字符
?		匹配任意一个字符(不包括目录分隔符)
**/		递归匹配任意子目录	
```

```json
//tsconfjg.json
{       
    "files":[                   //制定的特殊文件有最高的优先级
        "./module/math.ts",
        "./module/file.ts",  
        "/module/file.ts"       //不能
    ],
    "include": [
        "./module/*e.ts"
    ],
    "exclude": [
        "./module/f*.ts"
    ]
}
```

outDir文件夹里一定不 编译

