# C

浮点数不能准确存储

函数，面向对象变成，模块化

## 函数

1.函数是什么？

​	黑匣子

​	完成特定功能的独立代码块

​	double 

```c
void f (void); 函数声明，分号不能丢掉

int mian(){
    
}

void f(void){
    
}
```

2.形参和实参

​	个数相同，类型兼容

​	A如何调用B函数（递归  栈）

## 指针

### 1.概念

```c
#include<stdio.h>
/*
	指针 就是 地址，地址 就是 指针
	地址 就是内存单元的编号 
			从0开始的非负整数
			范围： 4g【0--4G-1】 
	指针变量 就是 存放地址的变量
	指针 就是一个编号（只是一个单元的值，变量 才是存放编号的一个单元 
	指针 和 指针变量 是两个不同的概念 ，我们叙述时会将 指针变量 简称为 指针 ，实际他们的 
*/ 

int main(void) {
	int * p; 	//int *标识p变量存放的是int类型变量的的地址  	*是地址！ 指针变量 ==地址变量 
				//所谓int * 类型就是存放int变量地址的类型 
	int i = 3;
	 
	p = &i;		//存放int变量地址的变量 = int i的地址  
				// int * ==  &int ,*p == i 
				//*p 代表的是以p的内容 为地址 的变量 
	/*
		p保存了i的地址，所以p指向i 
		p不是i，i不是p：修改p不影响i的值，修改i的值也不影响p 
		一个指针变量指向了某个普通变量，则
			*指针变量 完全等同于 普通变量 
			
		如果p 是指针变量，p存放了普通变量i的地址，
		则p指向普通变量
		*p 就完全等同于 i 
		
		*p  互相转换 i 
		
	*/
	 
//	p = i;	//error
	return 0;
}
```

### 2.常见错误解析

#### 2.1

```c
	int * p;			本身存储的是垃圾内容 
	int i = 5;
	
	*p = i;				//错误 
	printf("%d",*p);
```

#### 2.2

```c
	int *q;
	printf("%d",*q); 	q内部是垃圾值，则不能读写*q的内容 ，内存单元的控制权限没有分配给本程序 
```

指针最大的问题会导致内存泄露。

#### 2.3

​	多free(p) 

### 127 指针互换两个数字

```c
#include<stdio.h>
/*
	*p == &a
*/ 

//不能互换  只是修改了qp的值，没改变*p和*q的值 
void snap(int *p,int *q){
	int *t;	//数据类型要一样，要不然出错 
	/*
		没有互换ab的地址，只是互换了pq的值 
		没有语言可以做到将静态变量换地址 
	*/
	t = p;	
	p = q;
	q = t;
}

//可以互换 p是a的地址，*p就是a， 
void snap2(int *p,int *q){	//互换值  huhuan(&a,&b)
	int t;
	t = *p;
	*p = *q;
	*q = t;
} 

int main(void) {
	
	int a = 3;
	int b = 5; 
	
	snap3(&a,&b); //snap(a,b) error,
	
	return 0;
}
```



#### 	测试：

```c
#include<stdio.h>
/*
	*p == &a
*/

void change(int *i,int *j) {
	printf("%d  %d\n",i,j);
	int *t;
	t=i;	//改变的是指针变量初始值
	i=j;
	j=t;
	printf("%d  %d\n",i,j);
}
void test(){
	int *t;
	int i=4;
	t = &i;
	printf("%d  %d\n",t,*t);//t是i的内存地址 
}

int main(void) {
	int x=2;
	int y=3;
	change(&x,&y);
	printf("%d  %d\n",x,y);
	test(); 
	return 0;
}
```

​	int *p和 *p=5 两个 *的含义不同

​	p是一个已经定义好的指针变量

​	***p表示 以p的内容为地址的变量。 *p== i**

### 130 被调函数中改变主调函数相关变量的值

```c
change(int *p , int *q){	发送多个普通变量的地址既可
	*p = 3;
	*q = 4;
}

int main(void) {
	int a = 1;
	int b = 2;
	change(&a, &b);
	printf( "%d,%d",a,b);
	
	return 0;
}

------------------------
    3,4
------------------------
Process exited after 0.3227 seconds with return value 0
请
```

131 指针和数组

​	指针和一维数组

​	指针和二维数组

132 

​	一维数组：

- 一维数组名是个指针常量

- 名字存放的是一个变量的地址，a就是a[0]的地址  **&a[0] === a**

- ```
  int %d
  long int  %d
  char  %c
  float %f
  double %lf
  
  以16进制输出的话 ： %#
  ```

134 确定一个一维数组需要两个参数的原因

```c
#include<stdio.h>
/*
	数组没有特殊结束的标记 
*/

void printfArray(int *p, int len){		//数组元素的个数，数组名 
//	printf("%d\n",p);
	for(int i =0; i<len ; i++){
		printf("第%d次循环\n",i);
		printf("取到元素 %d\n",p[i]);		//取的是int *类型的p地址的第i个 int *p[i],取的是元素 
		printf("取到元素 %d\n",*(p+i));		//取的是以p的内容为地址的变量，元素 
		printf("取到地址 %d\n",(p+i));		//p是地址，p+i也是地址 
	}
}

int main(void) {
	int a[5] = {1,2,3,4,5};
	int b[6] = {-1,2,3,-4,5};
//	printf("%d\n",a);				//a是int *类型 
//	printf("%d\n",&a[0]); 

	printfArray(a, 5);
	
	return 0;
}

/*
第0次循环
取到元素 1
取到元素 1
取到地址 6487552
第1次循环
取到元素 2
取到元素 2
取到地址 6487556
第2次循环
取到元素 3
取到元素 3
取到地址 6487560
第3次循环
取到元素 4
取到元素 4
取到地址 6487564
第4次循环
取到元素 5
取到元素 5
取到地址 6487568

--------------------------------
Process exited after 0.3002 seconds with return value 0
请按任意键继续. . .
*/
```

2021-11-18

