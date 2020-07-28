---
title: ()"c-复习回顾"
tags:
  - null
categories:
  - null
toc: true
date: 2020-07-18 23:44:55
top:
cover:
---
<style>
.yellow{
	background:yellow
}

</style>





# 一.基础部分(1)

## 程序运行的机制

首先了解 `IDE`

一个软件，用于软件开发

1. **编辑**: 编写源程序`.c`
2. **编译**: 将`.c`程序翻译为目标文件`(.obj)`在计算机底层执行的
3. **链接**: 将**目标文件(.obj)+ 库文件**生成可执行的文件(项目名.exe)
4. **运行**: 执行`.exe`文件，得到运行的结果
	
	
	
![c语言编译，运行机制图](https://user-gold-cdn.xitu.io/2020/7/19/1736303b6c4a487f?w=810&h=516&f=png&s=42390)

	
如图是visual安装目录下的相关文件

	
![visul下的相关程序](https://user-gold-cdn.xitu.io/2020/7/19/17363045ffe59589?w=540&h=576&f=png&s=307894)
	
**什么是运行?**

- 运行生成`.exe`文件(二进制文件)
- 控制台可直接运行`.exe`文件

	
## c语言标准库

**<math.h>**

```c
c

# include <stdio.h>  
# include <math.h>
# include <stdlib.h>  //包含 system() 

int main(){
	double res = pow(2.0,3.0);
	printf("2+3=%f",res);
	// system("pause")   表示暂停函数 
	system("pause");  
	return 0;
}

```

### 小结

常见问题和解决方法

语法错误

缺少分号

符号的状态(英文输入法和中文输入法混淆)

## 变量快速入门

```c
# include <stdio.h>

int main(){
	
	int num =1; //整型
	double score = 2.3;// 小数
	char gender ='A'; //字符
	char name[] = "尚硅谷" ;//字符串
	printf("num=%d core=%.2f gender=%c name=%s",num,score,gender,name); 
	return 0;
}

```
注意 

- 单个字符是本质是`整型`,如 `A~Z,a~z`,基于acsll码的字符，用单引号`''`
- 字符串其实是字符串数组，如 ,`hello`，用双引号`""`括起来

### 变量的使用和注意事项


**变量的三要素**

    数据类型 变量名 变量值

变量要先声明后使用

    声明:申请户口
    使用:申请"户口"后使用是合法的，不然是"非法黑户口"

变量在同一作用域不可同名

![](https://user-gold-cdn.xitu.io/2020/7/19/1736315350b4d29d?w=1006&h=400&f=png&s=73215)

如上图是在[devc++c语言编辑器](https://sourceforge.net/projects/dev-cpp/)中运行的结果

也是一般的常见的错误 `xxxwas not declared in this scope`,表示变量在这个作用域内没有声明

**总结**

	1) 变量表示内存中的一个储存区域(不同的数据类型，占用的空间大小不同)
	2) 该区域有自己的名称和类型
	3) 变量先声明后使用
	4) 该数据可在同一类型范围变化
	5) 变量在同作用域不可重名

## 数据类型介绍

![](https://user-gold-cdn.xitu.io/2020/7/19/173631f69edfb4c9?w=1492&h=1288&f=png&s=137156)


**注意 **
- c语言中没有`string`类型数据类型,只有字符串数组

### 整型

**如何确定某设备下的int的字节数？**

```c
# include <stdio.h>

int main(){
	
	int num = -2.147483648;
	printf("int的字节数=%d",sizeof(int)); //使用sizeof()函数可以的到数据的字节数 
	
}

// print
-----------------
int的字节数=4
--------------------------------
Process exited after 0.2498 seconds with return value 0
请按任意键继续. . .

```

默认 int 类型就是 sign int (有符号)

还有 unsigned int 

#### 整型注意事项

```c
# include <stdio.h>
int main(){ 
	long long  num1 = 12147483647;
	// 如果输出的是long ,格式为 %ld  ，输出 : -737418241 
	// 如果输出 long long 类型  格式为 %lld ,输出 :  
	printf("%lld",num1); 
	getchar();
}
```

![](https://user-gold-cdn.xitu.io/2020/7/19/17363433a2e369ab?w=550&h=298&f=png&s=15820)

![xxx](https://user-gold-cdn.xitu.io/2020/7/19/17363408f8467889?w=6&h=5&f=png&s=27403)

- int 3 在内存中占 `4`个字节

## 浮点型

类型:| 储存大小: |值范围： |精度
-- |--|--|--
float 单精度 | 4字节|1.2E-38 ~ 3.4E+38|6位小数
double双精度|8字节|2.3E-308~ 1.7E+308|15位小数

默认是`double`类型，如果要表示`float`类型，则加`f`

如 
```c
double i = 3.14159; //默认表示double类型的数据
double i = 3.14159f; //这才表示float类型


```

**【案例】**

```c
# include <stdio.h>

int main(){ 
	float d1 = 1.1; //实际上的double类型，从double到 float截断
	float d2 = 1.1f; //才是float 类型 
	double d3 = 1.3; // ok
	
	double d4 = 5.12;
	double d5 = .512; //等价0.512
	double d6 = 5.12e2;//等价 5.12*(10^2) 
	double d7 = 5.12e-2; //等价 5.12*(10^-2) 
	printf("float d1=%f\nfloat d2=%f\ndouble d3=%f\ndouble d4=%f\ndouble d5=%f\ndouble d6=%f\ndouble d7=%f",d1,d2,d3,d4,d5,d6,d7);
	getchar();
	return 0;
}

// print --------------------
	float d1=1.100000
	float d2=1.100000
	double d3=1.300000
	double d4=5.120000
	double d5=0.512000
	double d6=512.000000
	double d7=0.051200

// 如 float d1 = 1.895633754
// 则 float只会保留小数点后6位，也就是  1.895633 ,显然精度下降
// 可以格式指定位数 ，如 %.10f : 表示保留小数点后10位，但是要看这个类型最多实际多少位 

 

```

## 字符型

c语言中 `char数组`表示字符串

数组为`构造类型`，非基本类型

- 字符常量是单引号 ' ' 括起来的 ，如 char a = 'a' , char b ='0'
- c 语言中允许转义字符 "\ " 将其后的字符转为特殊字符型常量  如char c3 = '\n' ,表示换行符
- c 中的char 本质是整数 ，对应ascll码，输出会按照ascll码字符
- char 是可以运算的，因为相当于整数，对应ascll码

```c
# include <stdio.h>

int main(){
	char a1 = 'a';
	char b2 = 'b';
	char c3 = 97;  // 97是整数，当以%c格式输出时，会以ascll码对应的字符输出 
	printf("a1=%c b2=%c c3=%c",a1,b2,c3); 
	getchar();
	return 0;
}

// 打印

// a1=a b2=b c3=a



```

字符型储存到计算机中，需要讲字符对应的码值(整数)找出来

**储存** ： 字符'a' --->码值(97) --->二进制(1100001) --->储存

**读取**：二进制(1100001) --->码值(97) ---> 字符'a' ---> 读取显示

## 布尔类型

```c
# include <stdio.h>
// 宏定义  
# define BOOL int 
# define TRUE 1
# define FALSE 0

/*
 c 语言中 0表示假 ，1表示真 
*/


int main()
{
	int isPass = 1;
	if(isPass){   
		printf("通过考试!");
	}
	
	// 可以使用宏定义来完成 
	// 定义一个bool变量
	BOOL isOk = TRUE; // 等价isOk = 1 
	if(isOk) {
		printf("ok成立");
	}
	
	getchar();
	return 0;
}

// 打印

//   通过考试!ok成立
 

```

## 数据类型自动转换

![](https://user-gold-cdn.xitu.io/2020/7/19/173635ad8df33321?w=752&h=449&f=png&s=26641)

反正是向着，精度高的转换，最高的是`long double`

## 强制类型转换

因为自动是从高精度 --->低精度

所以从低精度--->到高精度是一个"刻意"的过程，要强制转换 

但是精度可能会损失

```c
# include<stdio.h>
/*
 变量 = (数据类型)表达式 ; 
 如 
 double d1;
 int num;
 d1 = (int)num;//表示把double类型的num强制转换为int类型
 d1 = (int)3.24*4+4*6;
*/


# include <stdio.h>

int main(){
	double d1 = 3.13626673753646524;
	float num = d1;
	printf("num=%f",num);
	return 0;
}

```

## c指针快速入门

```c
c
# include<stdio.h>

/*

1. int* 表示变量为指针类型 
2. ptr  表示变量名
3. ptr 指向int类型的变量的地址

 指针变量本身也有地址 
*/

# include<stdio.h>
int main(){
	int num = 1;
	int* ptr=&num; // 表示将num变量的地址存入ptr指针变量中
	// 如果要输出一个变量的地址，使用%p
	// &num 表示取出 num变量的地址
	printf("num的地址=%p\nnum的值=%d",&num,num);
	printf("\nptr指针变量的地址=%p",&ptr);
	printf("\nptr的值=%p",ptr);
	printf("\nptr指向的变量num的值=%d",*ptr); //*指针变量名 表示这个指针变量指向的变量的值 
//	getchar();
	return 0;
	
	
}

//// 打印
num的地址=000000000062FE1C
num的值=1
ptr指针变量的地址=000000000062FE10
ptr的值=000000000062FE1C
ptr指向的变量num的值=1

```
![](https://user-gold-cdn.xitu.io/2020/7/19/173636e2a1dfbeb6?w=595&h=429&f=png&s=19070)

### 指针应用案例

写一个程序，获取一个int变量num的地址，并显示到终端

将num的地址赋给指针ptr,并通过ptr修改num的值

画出案例的内存布局示意图

```c
# include<stdio.h>


int main(){
	
	
	int num=88;
	printf("num没有修改前的值=%d\n",num);
	int*ptr = &num;
	*ptr =99; //修改num的值 
  //*ptr表示ptr指针指向的那个变量的值即num
	printf("int类型变量num的地址=%p\n",&num);
	printf("num的修改后的值=%d",num);
	return 0; 
}

// 打印

num没有修改前的值=88
int类型变量num的地址=000000000062FE14
num的修改后的值=99

```

<span style="backgound:red">指针变量的类型和这个指针指向的变量的类型相同</span>

基本数据类型都有对应的指针类型

## 值传递和地址传递

c语言中的传递参数

- 值传递(pass by value)
- 地址传递(pass by point )，传递指针也叫地址传递

**默认的传递值的数据类型 :**
- 基本数据类型
    - 整型
    - 浮点
    - 字符
- 结构体
- 共用体

**默认的传递地址的类型**
- 指针
- 数组

其实本质都是"值"传递，只不过值的意义不一样

### 练习

```c


# include<stdio.h>

int main(){
	
	/*
	使用不同的变量保存不同的数据 
	
	*/
	char name[10] = "张三";
	short age =23;
	float score =78.5f;
	char gender = 'M'; //man男  
	char hobby[20] = "足球,篮球";
	
	printf("姓名\t年龄\t成绩\t性别\t爱好\n");
	printf("%s\t%d\t%.2f\t%c\t%s",name,age,score,gender,hobby);
	return 0;
}

// 打印

/*
姓名    年龄    成绩    性别    爱好
张三    23      78.50   M      足球,篮球


*/ 

****

```

```c
# include<stdio.h>

// 小小计算器 


int main(){
	
//	分析
// 1.定义两个变量
// 2.根据要求输入结果即可
	int n1 =0;
	int n2 = 5;
	int sum = n1+n2;
	int sub = n1-n2;
	int mul = n1*n2;
	int div = n1/n2;
	int mod = n1%n2;
	printf("%d+%d=%d",n1,n2,sum);
	return 0;
}

// 打印

//0+5=5 


```

## 常量

- 常量是固定值，程序运行过程，无法改变，又称`字面量`(看到的怎样，就是怎样)
- 常量可以是任何`基本数据类型`
- 常量的值定义后不能进行修改


### 整型常量

```c
# include<stdio.h>


int main(){
	
	int n1 = 0213; // 8进制对应的十进制139 
	int n2  = 0x4b;// 16进制对应十进制75 
//	验证
	printf("n1=%d n2=%d",n1,n2); //这个%d 的 d代表十进制的格式，即decimal
	return 0;
}

// 打印

//n1=139 n2=75

```

### 浮点型

    整数部分 + 小数点 + 指数部分
    3.14 // double类型
    314E-2 //科学计数法
    3.14f //float类型

浮点型可以用 小数形式 / 指数形式表示

```c
#include<stdio.h>

int main(){
  int n1 = 0123; //8进制 对应的10进制的83
  int n2 = 0x4b; // 16进制对应十进制的75
  char c1 = 'a';
  char c2 = '\t';  //"\t"是字符型常量，本质是整型，对应ascll码
  char str[120] = "北京hello";
  char str2[100] = "hello\ 
  world" ; //等价 "hello world"

  // \ 表示下面的字符是要换行的字符，是一个整体，仅仅是为编译器标识，不是字符内容
  // \ 和字符内容的上半部分之间不能有空格space

  //验证
  printf("n1=%d\nn2=%d",n1,n2);

  return 0;

}

//打印
n1=83
n2=75

```

## define定义常量

define 是预处理器

define定义常量的形式

    #define 常量名  常量值
    如 
    #define PI    3.14159

【案例】

```c
# include<stdio.h>

#define PI 3.14

int main(){
	double area; //圆面积 
	double r =1.2; //圆半径 
	area = PI*r*r;
	printf("圆的面积=%.4f",area);
	return 0;
}

// 打印

圆的面积=4.5216
```


## const关键字

```c
# include<stdio.h>

//#define PI 3.14
const double PI =3.14; 
/*
1. const 是关键字，定义好的，后边表示定义常量
2. PI是常量，不可修改 
3. const定义常量时，需要加分号  ";" 

*/

int main(){
	// PI可以修改嘛? ---> 答案：不能，因为PI被定义为define常量 
	double area; //圆面积 
	double r =1.2; //圆半径 
	area = PI*r*r;
	printf("圆的面积=%.4f",area);
	return 0;
}

// 打印

圆的面积=4.5216

```

- const 定义常量时，带类型，define不带类型
- const`只是`在编译运行的时起作用
- define是预编译时就起作用
- define只是简单的替换，无类型检查，简单的字符替换，会导致`边界效应`
- const 可以调试，define不能，引预编译已经替换，调式时没有
- const不能重复定义(如同定义普通变量)
- define可以通过通过`undefine`取消定义
- define可结合`#ifdef` ,`#ifndef` ,`#endif`，使代码灵活，可以用define启动，调试

【案例】

```c
#include<stdio.h>

#define A 1
#define B A+3
#define C A/B*3

int main(){
  /*
  分析：
    #define就是简单的替换
    C 本质是 A/A+3*3 = 1/1+3*3 = 1+9 =10
          也即  1/A+3*3
          也即  A/ B *3
    也就是说运算不用加括号，直接组合形成一个表达式，计算表达式即可


  */
  return 0
}

```
```c
# include<stdio.h>

const double PI =3.14; //不能重复定义
#define PI 3.14 // 定义PI常量 
#undef PI //取消PI 
#define DEBUG //定义DEBUG常量

int main(){
#ifdef DEBUG
	printf("ok,调试信息");
#endif	
#ifndef DEBUG
	printf("hello,另外信息");
#endif

// 如果定义过DEBUG，则 执行 ifdef，否则执行ifndef
//通过这个可以调试信息 
	return 0;
}

// 打印

 
```

## 运算符

**运算符是一种特殊的符号，表示数据的运算，赋值，比较等**

- 算数运算符 (+-*/%)
- 赋值运算符(=+=-=)
- 关系运算符(> ,>=,<,<=,==,=)
- 逻辑运算符(&&与，|| 或,!非)
- 位运算符(&按与，|按或^按位异或，~按位取反)
- 三元运算符(表达式?表达式1:表达式2)

### 算数运算符

```c
# include<stdio.h>


int main(){
	double d1 = 10/4; 
	// 处理流程分析
	/*
		10/4->2.5,因为是整数/整数，所以->2.又因为是double类型，所以
		->2.000000 (6位小数) 
		
		如果希望保留小数，则参与运算的数有一个为小数
		10.0/4 或者 10/4.0 
	
	*/ 
	int res1 =10%3; //求10/3得余数
	int res2 = -10%3; //? 
	// 计算机中的取mod公式 
	// a%b = a-a/b*b 
	// 即 -10%3 = -10-(-10)/3*3 = -10-(-3)*3 = -10+9 =-1
	
	int i =10;
	printf("i改变前的值=%d\n",i); 
	int j =i++;
	// j = i++ 等价
	// j =i ;
	// i = i+1;  
	// 即 print i  = 11 , j = 10; 
	printf("d1=%f\nres1=%d\nres2=%d\n",d1,res1,res2) ;
	printf("i改变后的值=%d\nj=%d",i,j);
	return 0;
}

// 打印
/*
	i改变前的值=10
	d1=2.000000
	res1=1
	res2=-1
	i改变后的值=11
j=10

*/

```
![](https://user-gold-cdn.xitu.io/2020/7/19/17365080e92db277?w=709&h=615&f=png&s=42499)

**注意**

- 对于`/`除号，整数和小数是有区别的，整数之间做除法，保留整数，舍弃小数部分，如`int x = 10/3 ，结果是 3 而不是3.33...`
- 取模运算的本质
    - `a%b = a-a/b*b`
- 当自增作为独立运算，`++i`还是`i++`是一样的

#### 练习

【假如还有97放假，问：xx星期零xx天】

```c
# include<stdio.h>


int main(){
	 // 分析
	 // 思路
	 /*
	 	1. 天数 days
		2. 星期 week
		3. 剩余天数 leftdays 
		4. 使用 %,/ 
	 
	 */ 
	int days = 97;
	int week = days / 7;
	int leftdays = days % 7;
	printf("有%d星期零%d天",week,leftdays);
	return 0;
}

// 打印
/*
有13星期零6天
*/



```

【定义变量保存华氏温度，华氏温度转摄氏公式:5/9*(华氏温度-100),求编写程序，根据华氏温度求对应的摄氏温度】

```c
# include<stdio.h>


int main(){
	 // 分析
	 // 思路
	 /*
	 1. 华氏温度 huaShi 
	 2. 摄氏温度sheShi 
	 3. 使用公式转换 
	 */ 
	double huaShi = 146.7;
	double sheShi = 5.0/9*(huaShi-100);
	printf("华氏温度:%f对应的设置摄氏温度:%f",huaShi,sheShi);
}

// 打印
华氏温度:146.700000对应的设置摄氏温度:25.944444

```

### 关系运算符

比如 

    a = 2
    b = 3
    a >= b 
    printf("a>=b=%d",a>=b);

看这个`a>=b`的表达式的真值是真还是假，"真"用非0表示，"假"用0表示，一般的真值用1表示

那么上述的打印结果就是 0


```c

# include<stdio.h>

int main(){
	int a = 2 ;
	int b = 3 ;
	printf("a>b=%d",a>b);
	
	return 0;
}

// 打印

a>b=0 

```

### 逻辑运算符

a = 3 ,b = -4 :| .| .
-- | --|--
&& | 与运算，同真为真 | a && b == 0
`||` | 或运算，有真为真 | a || b ==1
! | 反| !a = 0

**短路**

- && 称为`短路逻辑与` ，当前面的表达式为假，则不用判断下一个表达式的真值

- || 称为`短路逻辑或`，当前面表达式为真，则直接为真

```c
# include<stdio.h>

int main(){
	int score = 100;
	int res =  score > 99; //100>99
	if (res){
		printf("hellw world\n");
		
	}
	if (!res){
		printf("hello,tom\n");
	}
	return 0;
}

// 打印

hellw world

```

#### 练习

```c
# include<stdio.h>

int main(){
	int x = 1;
	int y = 1;
	if(x++==2 && ++y==2){ // 此时 ++y不执行，被短路了 
		x= 7;
	}
	printf("x=%d,y=%d",x,y); // x 脱离 x++的范围，所以+1 后 = 2 ，y还是1
	return 0;
}

// 打印

x=2,y=1


```

```c
# include<stdio.h>

int main(){
	int x = 1;
	int y = 1;
	if(x++==1 ||++y==1){ // 此时 ++y不执行，被短路了 
		x= 7;
	}
	printf("x=%d,y=%d",x,y); // 执行了 x = 7，所以x=7 
	return 0;
}

// 打印

x=7,y=1

```

逻辑运算符是用于`连接多个条件`的，最后的表达式有真值，0或1

> 需注意逻辑运算的`短路现象`

### 三元运算符

    条件表达式? 表达式1:表达式2;

条件表达式为真，则结果为表达式1

条件表达式为假，则结果为表达式2

【案例】

```c
# include<stdio.h>

int main(){
	int a = 10;
	int b = 99;
	int res = a>b?a++:b--; //a>b为假，则结果为b--,先res =b ,后b-- 
	
	printf("\na=%d b=%d res=%d",a,b,res);// 此时的b = 99-1 =98 ,res= 99 
	return 0;
}

// 打印

a=10 b=98 res=99

```

```c
# include<stdio.h>

int main(){
	int a = 10;
	int b = 99;
	int max = a>b?a:b;
	printf("a,b两数的最大值为%d",max);
	return 0;
}

// 打印

a,b两数的最大值为99

```

## 标识符命名

**c语言标识符**

	各种变量，函数命名使用的字符序列
	凡是可以自己起名字的都叫标识符
	
**标识符命名规则**

- 26英文字母(c语言中区分大小写),0~9,_或$组成
- 数字不可以开头
- 不能使用关键字或保留字，但可以包含
- 标识符不能包含空格


      hello √
      hello12√
      1hello x
      h-b x
      x h x 
      h$s √
      int x 
      stu_name √


**规范**

- 所有的宏定义，枚举常数，常量全部用大写字母，用下划线_ 分隔字母
如 `const double TAX_RATE = 0.08 `
- 定义变量要初始化，不然程序会异常退出

## 键盘输入语句

编程中，须接收外界用户的值，使用键盘输入语句

    1)include<stdio.h>
    2)使用scanf()函数
    3)使用适当的格式接收参数

【案例：从控制台接收用户的信息，姓名，年龄，薪水，性别】

```c
# include<stdio.h>

int main(){
	//使用字符数组接收名字
	char name[10] ="";
	int age = 0;
	double sal =0.0;
	char gender=' ';
	printf("请输入名字:");
	scanf("%s",name); //表示接收一个字符串，放到name中,name数组名本身是地址 
	printf("请输入年龄:") ;
	scanf("%d",&age); // 将输入的值放入变量的地址位置，所以要取地址&
	printf("请输入薪水:") ;
	scanf("%f",&sal);
	printf("请输入你的性别m/f:");
	getchar();// 接收回车符
	scanf("%c",&gender);
	
	// 输出信息
	printf("\nname %s age %d sal %.2f gender %c",name,age,sal,gender);
	getchar();
	getchar();
	return 0;
}

/*

*/

// 打印


```

getchar的机制分析

```c
#include <stdio.h>
int main(){
	char a,b;
	a=getchar();
	b=getchar();
	printf("a=%c\nb=%c",a,b);
	return 0;
}

// 打印

x
a=x
b=    

```

![](https://user-gold-cdn.xitu.io/2020/7/19/1736552ddf042985?w=976&h=561&f=png&s=82674)

也就是第一次按下回车键后，既是`排在前面`的字符从缓冲区释放，因为回车符号`\n`表示确定又是字符，所以`\n`回车字符进入缓冲区，下次就是`\n`字符"等待"从缓冲区释放

`gechar()`每次只能接收一个字符，如果回车前输入多个字符，则释放此时缓冲区的第一个字符，剩下的进入缓冲区等待释放，当遇到`getchar()`，此时不用回车"等待释放"的第一个字符已经进入getchar()中

有顺序的哦！

> 准确来说，当遇到`getchar()`才会释放，不然是等待释放状态
> 参考 https://www.cnblogs.com/snow1234/p/5463005.html

## 运算符综合练习

【定义变量保存秒数，打印输出xx小时xx分钟xx秒】

```c
# include<stdio.h>

int main(){
	//思路
	/*
	1. 定义变量保存秒数 second
	2.         保存小时 hour
	3.         保存 分钟 minute
	4.         保存剩余leftSecond 
	*/ 
	int second = 984567; // 秒 
	int hour = second /3600; //转换为小时 
	int min =second %3600 / 60;
	int leftSecond =second % 60;
	printf("%d秒合 %d小时 %d分钟 %d秒",second,hour ,min,leftSecond);
	getchar();
	return 0;
}

// 打印
984567秒合 273小时 29分钟 27秒


```

【实现对三个整数进行排序，输出按从小到大顺序】

```c
# include<stdio.h>

int main(){
	//思路
	/*
	
	*/ 
	int n1 =10;
	int n2 =8;
	int n3 =5;
	int temp =0; //用于交换的临时变量 
	printf("最初顺序 n1=%d n2=%d n3=%d\n",n1,n2,n3);
	// n1和 n2比较，如果n1 > n2 则交换 
	if(n1>n2){
		temp = n1;
		n1 = n2;
		n2 =temp;
	}
	// n2 和 n3比较，如果n2 > n3 则交换 
	printf("第1次处理后 n1=%d n2=%d n3=%d\n",n1,n2,n3);
	if(n2>n3){
	temp = n2;
	n2 = n3;
	n3 =temp;
	}
	
	printf("第2次处理后 n1=%d n2=%d n3=%d\n",n1,n2,n3);
	// n1和 n2比较，如果n1 > n2 则交换 
	if(n1>n2){
		temp = n1;
		n1 = n2;
		n2 =temp;
	}
	// n2 和 n3比较，如果n2 > n3 则交换 
	printf("第3次处理后 n1=%d n2=%d n3=%d\n",n1,n2,n3);
	getchar();
	return 0;
}

// 打印
最初顺序 n1=10 n2=8 n3=5
第1次处理后 n1=8 n2=10 n3=5
第2次处理后 n1=8 n2=5 n3=10
第3次处理后 n1=5 n2=8 n3=10

```

## 四种进制规则

## 位运算底层机制

计算机的底层的数是用`补码`表示的

## 程序流程控制

### 顺序控制 

顺序控制

	程序从上到下逐步的执行，没有判断和跳转
	
### 分支控制
#### if-else

**单分支**
	
```c
# include<stdio.h>

int main(){
	int age = 0;
	printf("请输入年龄:");
	scanf("%d",&age);
	
	if(age > 18){
		printf("你已经成年了!");
	}
    
     printf("你还未成年!!!");
	getchar(); //接收回车 
	getchar(); //暂停控制台 
	return 0;
}

// 打印

```



**双分支**

```c
# include<stdio.h>
/*
双分支

如果....
否则....
if...
else...
if和else是相联系的 

*/ 
int main(){
	int age = 0;
	printf("请输入年龄:");
	scanf("%d",&age);
	
	if(age > 18){
		printf("你已经成年了!");
	}else{
		printf("你还未成年!");
	}
	
	
	getchar(); //接收回车 
	getchar(); //暂停控制台 
	return 0;
}

// 打印

```


**多分支**

```c
# include<stdio.h>

int main(){
	/*
	思路
	定义变量保存成绩double 
	判断条件有多个，所以使用多分支处理
	 
	*/
	double score = 0;
	printf("请输入期末成绩:");
	scanf("%f",score);
	
	if(score==100){
		printf("奖一辆BMW");
	}else if(score>80 && score<99){
		printf("奖励iphone7plus");
	}else if (score >60 && score <80){
		printf("奖励一台ipad");
	}else{
		printf("什么奖励都没有");
	}
	getchar();
	getchar();
	return 0;
}


```

**嵌套分支**

#### switch

```c
# include<stdio.h>

int main(){
	char c1=' ';
	printf("请输入一个字符(a,b,c)：");
	scanf("%c",&c1);
	
	switch(c1){
		case 'a':
			printf("今天星期一，猴子穿新衣");
			break;
		case 'b':
			printf("今天星期二，猴子当小二");
			break;
		case 'c':
			printf("今天星期三，猴子去搬砖");
			break;
		default:
			printf("没有匹配到任何值");
	}
	return 0;
}

```

> switch 语句用于，判断的条件的格式和类型一致的问题中

判断的表达式必须为`整型常量字符串`
- 整型
- 字符

遇到break就退出分支


### 循环控制

#### **for**

```c
# include<stdio.h>

int main(){
	/*
	要求： 打印1~100之间所有的是9的倍数的个数及总和
	
	分析: 
	定义变量count 保存个数
	定义变量sum 保存总和 
	*/	
	int i = 0;
	int count =0;
	int sum = 0;
	for(i=0;i<=100;i++){
		//判断i是否是9的倍数
		if(i%9==0) {
			count++; //统计个数 
			sum+=i;//求和 
		}
	}
	printf("sum=%d\ncount=%d",sum,count) ;
//	getchar();
	return 0;
}

```

#### while

```c
# include<stdio.h>

int main(){
	/*
	要求： 
	
	分析: 
	*/	
	int i = 0;
	while(i <= 5 ){ //循环条件成立则执行语句 
		printf("\n你好");
		i++;
	}
	return 0;
}

```

```c
# include<stdio.h>

int main(){
	/*
	要求： 
		输出 1~100所有能被3整除的数 
	分析: 
	*/	
	int i = 0;
	int max = 100;
	while(i <= max){
		if(i%3==0){
			printf("%d\n",i);
		}
		i++;
	}
	return 0;
}

```

```c
# include<stdio.h>
# include<string.h>
int main(){
	/*
	要求： 
		不断的输入姓名，直到输入"exit" 为止 
	分析: 
	*/	
	char name[10] ="";
	
	while(strcmp(name,"exit")!=0){
		printf("\n请输入名字:");
		scanf("%s",name);
		printf("\n请输入的名字是:%s",name);
	}
	return 0;
}

/*
	<string.h>中有一个方法strcmp用于字符串比较 
	int strcmp(const char* str1,const char* str2)
	把str1指向的字符串和str2指向的字符串比较，返回0表示相等，非0则不相等
	strcmp 即 string compare字符比较 
*/


```

#### do while

先执行后判断 


## 经典案例

![](https://user-gold-cdn.xitu.io/2020/7/19/17366762af12d9f8?w=422&h=82&f=png&s=19486)


【经典案例-打印空心金字塔】

```c

# include<stdio.h>
# include<string.h>
int main(){
	/*
	要求： 
		打印空心金字塔 
	分析: 
		化繁为简，先死后活 
		1.打印矩形
			int i j;
			for(i=1;i<=5;i++){ //控制行 
				for(j=1;j<=5;j++){ //控制列 
					print("*");
				}
			}
			return 0;	
		2.打印半个金字塔
			int i ,j;
			for(i=1;i<=5;i++){ //控制行 
				for(j=1;j<=i;j++){ //控制列 
					printf("*");
				}
				printf("\n");
			}	
			return 0;
		3.打印整个金字塔
			int i,j,k;
			for(i=1;i<=5;i++) {
				for(k=1;k<=5-i;k++){
					printf(" ");
				}
				for(j=1;j<=2*i-1;j++){
					printf("*");
				}
				printf("\n");
		 	}
			
			return 0;
			
		4.打印空心金字塔
			int i,j,k,level = 20;
				for(i=1;i<=level;i++) { //i控制层数 
					for(k=1;k<=level-i;k++){// k控制 空格何时输出 
						printf(" ");
					}
					for(j=1;j<=2*i-1;j++){ //J控制列数 (每行的*数)
						if(j==1 || j==2*i-1 || i==level){
						printf("*");
						}else{
						printf(" ");
					}
					
					printf("\n");
				}
			
			}
				
				return 0;
		5.打印空心菱形 
	*/	
	int i,j,k,level = 20;
	for(i=1;i<=level;i++) { //i控制层数 
		for(k=1;k<=level-i;k++){// k控制 空格何时输出 
			printf(" ");
		}
		for(j=1;j<=2*i-1;j++){ //J控制列数 (每行的*数)
			if(j==1 || j==2*i-1 || i==level){
				printf("*");
			}else{
				printf(" ");
			}
			
			
		}
		printf("\n");
 	}
	
	return 0;
}

//打印

/*
1 

	*****
	*****
	*****
	*****
	*****
2

	*
	**
	***
	****
	*****
	
3

	   *
 	  ***
     *****
    *******
   ********* 
 
 4
 		           *
                  * *
                 *   *
                *     *
               *       *
              *         *
             *           *
            *             *
           *               *
          *                 *
         *                   *
        *                     *
       *                       *
      *                         *
     *                           *
    *                             *
   *                               *
  *                                 *
 *                                   *
***************************************



*/ 

```

**学习的思想**
- 由简单到复杂
- 先死后活(具体到抽象)


## break跳转语句

遇到break语句会退出循环

【案例-账户密码登录验证模拟】

```c
# include<stdio.h>
# include<string.h>

int main(){
	
	
	/*
	 	要求：实现登录验证，有三次机会，如果用户名为"张无忌" ，密码为"888" 则登录成功
		 	  否则 提示还有几次机会，使用for循环完成
			   
		分析:
			定义变量，保存登录的机会的个数 chance
			定义两个字符数组，用于存放用户名和密码
			使用for + break 如果登录成功，则退出循环(不用验证了嘛) 
	*/
	int chance = 3;
	int logCount = chance; //logCount为登录次数 
	char name[10] = "";
	char pwd[10] = "";
	int i ;
	for(i=1;i<=logCount;i++){
		printf("请输入用户名:");
		scanf("%s",name);
		printf("请输入密码:");
		scanf("%s",pwd);
		
		//判断字符串相同 使用strcmp 
		if(strcmp(name,"张无忌") == 0 && strcmp(pwd,"888") == 0 ){
			printf("登录成功!");
			break;
		}else{
			//机会次数减少
			chance--;
			printf("你还有%d次登录机会!!!\n",chance); 
		}
	}
	getchar();
	return 0;
}

```

### continue

结束本次循环，不是退出循环，而是直接跳转到下次循环

`只能配合循环语句`使用，不能单独和switch / if 使用 

`break可以单独配合switch / if 使用`

## goto
c语言中无条件跳转到指定行

一般不建议使用
```c
# include<stdio.h>
# include<string.h>
/*
语法
  goto destination_keyword;

  ...
  ...
  destination_keyword:
  ...
  ...

*/
int main(){
	printf("\n开始"); 
	goto label1;
	printf("aaa\n");
	printf("***");
	 label1:
	printf("bbb\n");
	printf("***");
	getchar();
	return 0;
}
/*
打印 ： 

	
	开始bbb
  ***
 
*/
```


# 二.基础部分(2)


## 枚举语法入门

介绍 :枚举类型是c语言中的一种`构造类型`

功能: 使数据更简洁，易读 ，`对于只有几个限定的数据`，可以使用枚举

枚举是一组常量的集合，包含一组**有限**的**特定**的数据，特定(形式结构上，要统一)

语法 

	enum 枚举名 {枚举元素1,枚举元素2,...};

	注意格式 
	enum  ;
	当成一个变量

```c
#include <stdio.h>
int main(){
	enum DAY
	{	MON=1,TUE=2,WED=3,THU=4,FRI=5,SAT=6,SUN=7
	}; //这里的DAY就是枚举类型，包含7个枚举元素
	enum DAY day; //enum DAY是枚举类型 ,day是枚举变量
	day=WED; //给枚举变量 day赋值 ，值就是某个枚举元素
	printf("%d",day); // 每一个枚举元素对应一个值
	getchar() ;
	return 0;

}
//打印

3 

```

**思想:** 变量的思想，便于维护，管理

		按道理，可以直接
		DAY{1,2,3,4,5,6};
		但是这样的话，就写"死"了

### 枚举

【案例】

```c
#include <stdio.h>


//枚举类型的定义
enum DAY{
	MON=1,TUE,WED,THU,FRI,SAT,SUN //如果没有赋值，则按顺序赋值
	 
}day; 
/*
	1. 表示定义一个枚举类型，也就是enum DAY
	2. 同时定义了一个枚举类型的变量 day
	
	如同 int i ; 
*/
	
int main(){


		for(day=MON;day<=SUN;day++){
			printf("枚举元素:%d\n",day); //1,2,3,4,5,6,7
		}
		getchar();
		return 0;	

	

}
//打印


/*语法：
 
    enum 枚举类型名{} 枚举变量名 ； 
		

*/

```

### 枚举类型使用注意事项

【案例】

```c
#include <stdio.h>


//1. 枚举类型的定义形式 (定义枚举类型同时定义枚举变量) 
enum DAY{
	MON=1,TUE,WED,THU,FRI,SAT,SUN //如果没有赋值，则按顺序赋值
	 
}day; 
/*
	1. 表示定义一个枚举类型，也就是enum DAY
	2. 同时定义了一个枚举类型的变量 day
	
	如同 int i ; 
*/
// -----------------------------------------	

// 2. 定义枚举类型形式 (先定义枚举类型后定义枚举变量) 

enum DAY
	{	MON=1,TUE=2,WED=3,THU=4,FRI=5,SAT=6,SUN=7
	}; //这里的DAY就是枚举类型，包含7个枚举元素
	
enum DAY day; //enum DAY是枚举类型 ,day是枚举变量
// ------------------------------------------------------ 

// 3. 定义枚举类型形式 (省略枚举类型名，直接定义枚举变量) 

enum {
	MON 1，TUE,WED,THU,FRI,SAT,SUN
	 
}day;  //这样使用枚举，该枚举类型只能使用一次，因为没有定义枚举类型 

```

```c
# include<stdio.h>	

// 可以将整数转换为对应的枚举值 


int main(){
	
	//定义枚举类型 
	enum SEASONS{
		SPRING =1,SUMMER,AUTUMN,WINTER	
	};
	// 定义枚举变量 
	enum SEASONS season;
	int n =4;
	season =  (enum SEASONS)n ;
	printf("season = %d",season);
	getchar();
	
	return 0;
}
/*
	本质，枚举类型可以当作常量的集合 ，变量的使用，使之更为灵活
            不能将整数赋值给 枚举变量，但是可以转化
        
	 
*/
printf("season = %d\t%d\t%d\t%d", season,SUMMER,AUTUMN,WINTER);
// 打印
season = 4      2       3       4

// ----------------------------------------------------------------------
可知，当从SPRING = 1，开始，以后的值自动赋值，按照逻辑的顺序连续加

如果是从中间赋值的话整体也是按逻辑的

但是如果中间某个值跳跃很大，则后面按照逻辑加

如 

# include<stdio.h>

int main() {

	//定义枚举类型 
	enum SEASONS {
		SPRING , SUMMER= 1, AUTUMN= 12, WINTER
	};
	// SUMMER= 1,后面AUTUMN快速变为 12，则WINTER按照12的顺序往后加，而SPRING按照SUMMER的逻辑顺序减

	// 定义枚举变量 
	enum SEASONS season;
	int n = 4;
	season = (enum SEASONS)n;
	printf("season = %d\t\nSPRING= %d\t\nSUMMER= %d\t\nAUTUMN=%d\t\nWINTER=%d", season,SPRING,SUMMER,AUTUMN,WINTER);
	// season = 4      2       3       4
	getchar();

	return 0;
}

// 打印

season = 4
SPRING= 0
SUMMER= 1
AUTUMN=12
WINTER=13


```
```c
# include<stdio.h>

int main() {

	//定义枚举类型 
	enum SEASONS {
		SPRING , SUMMER= 1, AUTUMN, WINTER
	};
	// 定义枚举变量 
	enum SEASONS season;
	int n = 4;
	season = (enum SEASONS)n;
	printf("season = %d\t\nSPRING= %d\t\nSUMMER= %d\t\nAUTUMN=%d\t\nWINTER=%d", season,SPRING,SUMMER,AUTUMN,WINTER);
	// season = 4      2       3       4
	getchar();

	return 0;
}

// 打印

season = 4
SPRING = 0
SUMMER = 1
AUTUMN = 2
WINTER = 3

按照逻辑顺序加减
```

**总结 :**

		1. 不赋初值，按逻辑从0到最大赋值
		2. 中间赋值，则前后按逻辑，加/减赋值
## 函数

### 函数的基本定义

**基本语法**

		返回类型 函数名 (形参列表){
			执行语句 ... // 执行语句这块是函数体
			....
			 return 返回值 ;// 返回值可选
		}

		1. 形参列表:表示函数的输入
		2. 函数中的函数体，是此函数要完成的功能语句
		3. 函数，可有返回值，也可无返回值,无返回值的类型 为 void

**一个完整的函数定义和调用**
```c
返回类型  函数名  (形参列表){
执行语句 …//函数体
return 返回值 ; //可选
}


函数名 (实参列表); // 调用函数


// 一般来说函数的定义写在前面,也就是要用函数，必先定义函数

// 如果是主函数的使用，可以定义放到后面，但是在主函数main()之前要定义

如 
// ----------------开始----------------------

# include<stdio.h>

void printStar(); //这个是函数的声明，算是简化的没有函数体的函数定义

void main() {
	printStar(); //调用函数
}


//函数的定义放到最后
void printStar() {
	printf("********************");
}


// 打印

********************

// ----------------结束------------------------------

```

**思考：**

	为什么要使用函数?
	
		使用函数的原因是和传统的，一般的方法比较而来的，一般，传统的方法，使用代码冗余，结构复杂，不易阅读


	>函数就相当于一个"灵活"的工具，想用就
	>函数主要是减轻代码量，使代码结构清晰

### 头文件简述

实际开发，需要很多不同的文件，如果把开发的的文件总体称为一个项目`project` ,头文件的使用，就是避免整个项目的`代码冗余`，如**文件a**可以使用**文件b**中定义的函数


**头文件** 
-  程序员自行编写的
- c标准库自带的

头文件是**扩展名为.h**的文件，包含`c函数的声明和宏定义`，可以被多个源文件使用

c程序用要使用头文件，用 `预处理指令# include<头文件名.h>`引用

#include<> 是文件包含命令 ，用于引入对应的头文件(.h),也是c预处理指令的一种

建议把所有的常量，宏，系统全局变量，函数原型写在头文件中，需要时可以随时引用

![](https://user-gold-cdn.xitu.io/2020/7/24/173816faaad6380d?w=1235&h=696&f=png&s=540518)


【案例】

```c

// demo.c

# include <stdio.h>

//引入需要的头文件

# include "myfun.h"

int main() {

	//使用myCal完成计算
	int n1 = 10;
	int n2 = 50;
	char oper = '+';
	double res = 0.0;

	//调用myfun.c中定义的函数
	res = myCal(n1, n2, oper);
	printf("\nres = %.2f", res);
	getchar();
	return 0;
}

```

```c
//myfun.c


//实现(定义) 
# include<stdio.h>


int myCal(int n1, int n2, char oper) {
	//定义一个变量res ，保存运算的结果 

	double res = 0.0;

	switch (oper) {
	case '+':
		res = n1 + n2; //做加法运算
		break;
	case '-':
		res = n1 - n2; //减法运算
		break;
	case '*':
		res = n1 * n2; //乘法运算
		break;
	case '/':
		res = n1 / n2; //除法运算
		break;
	default:
		printf("你的运算符有错！！！");
	}
	printf("\n%d %c %d = %.2f\n", n1, oper, n2, res);
	return res;
}


```

```c
// myfun.h
//声明

int myCal(int n1, int n2, char oper); //也相当于一个变量 #pragma once

```

### 头文件使用注意事项

1. 引用头文件，相当于赋值头文件的内容
2. 源文件名可以不可头文件一样，但是为了好管理，一般源文件和头文件名相同


3. `# include`
   
	- `# include<> ` :  引用系统头文件，如果这个头文件是系统头文件，那么两种方式都可以，这种效率高
	-  `# include " "` ：用户编写的头文件，且只能是用户编写的头文件,引用时先找程序的头文件，如果没有则找系统类库头文件


4. `# include`命令只能包含一个头文件，多个头文件须多个`# include`
5. 同一头文件如果多次引入，和一次引入效果一样，(头文件的防重复定义机制)
6. 在一个被包含的文件(.c)中可以包含另一个文件头文件(.h)
7. 头文件只能包含变量和函数的声明，不能包含定义(引起重复定义错误)

### 函数调用机制

```c
# include <stdio.h>
/*

	说明:
	1. 函数名 : test
	2.返回值 :无
	3. 功能 : 传入一个数,+1

*/


void test(int n) { //形参 要定义
	int n2 = n + 1;
	printf("n2=%d",n2);
}

void main() {
	int num = 6;
	test(num);
	getchar();
}
// 打印

n2 = 7


```

![](https://user-gold-cdn.xitu.io/2020/7/24/1738178e1fa022d6?w=1423&h=615&f=png&s=596931)


**函数调用的规则**

	1. 调用执行函数时，会开辟一个独立的空间(栈)
   
	2. 每一个栈是相互独立的
   
	3. 当函数执行完毕，会返回到调用函数的位置，继续执行


	4. 如果函数有返回值，将返回值作为调用函数的语句的值，如有赋值，则赋值给变量


	5. 当函数返回后，对应的栈空间也就销毁了


![](https://user-gold-cdn.xitu.io/2020/7/25/173841fa216b6542?w=884&h=432&f=png&s=108155)

### 函数递归调用

```c
# include <stdio.h>

/*函数的递归调用*/

void test(int n){
	if(n>2){
		test(n-1);
	}
	printf("\nn=%d",n);
}

void main(){
	test(4);
} 

/*
	一旦函数调用执行完，则返回到函数调用位置，继续执行下一句 
		
		
*/ 

//打印

n=2
n=3
n=4 

```

![](https://user-gold-cdn.xitu.io/2020/7/25/1738422d7d127c65?w=1165&h=526&f=png&s=317936)

![](https://user-gold-cdn.xitu.io/2020/7/25/1738427ac8457254?w=1587&h=858&f=png&s=1125152)

分析的递归的调用过程可以联系

内存中 的 函数的 开辟栈 和销毁栈的过程

<span class="yellow">重点就是 ，当调用的函数语句执行完，会回到调用时的语句，继续执行下面的语句子</span>

### 函数的递归练习

**斐波拉契数**

请使用递归的方式，求斐波拉契数1,1,2,3,5,8,13,21,34,55...， 给你一整数n，求它的斐波拉契数是多少?

		这个整数n指的是，这个斐波拉契数的最大的数，要求整个斐波拉契数列

```c
# include <stdio.h>

/*
	题目： 求斐波拉契数列，用递归的方式
	斐波拉契数列 1,1,2,3,5,8,13,21...给一整数 n ，求斐波拉契数列 是多少
	
	
	
	分析：
		如果 n =1 ,n =2 时，返回1 
		如果n 从3开始，则对应的斐波拉契数 是前两项之和 
*/
int fbn(int n ) {
	if(n==1||n==2){
		return 1;
	}else{
		return fbn(n-1) + fbn(n-2);
	}
}



int main(){
	
	getchar();
	return 0;
}


```

找到相对结构(相似结构)->当为一种变量结构
	
- 递归结束条件
- 递归结构

**可以画树形图分析**


### 函数注意事项

基本数据类型是 `值传递`

### 函数作用域 scope

**局部变量**

 作用域仅限于函数内部

```c
# include <stdio.h>

void sayHello(){
	char name[]= "tom"; 
	printf("hello %s \n",name); //name就是局部变量，作用域仅在sayHello函数中 
}

int main(){
	sayHello();
	printf("name=%s",name );
	getchar();
	return 0;
}

//如果在main函数(即 sayHello函数外)使用没有定义的name变量

//报错 
[Error] 'name' undeclared (first use in this function)

指的是main函数范围中的name未声明 


/*
	1. 函数内的变量称为 局部变量
	 
	2. 函数形参会当作函数的局部变量
	
	3. 函数外的变量称为全局变量
	 
	4. )当函数的全局变量和局部变量同名时，优先使用局部变量 (就近原则
	
	5. 在一个代码块中的作用域在代码块中,如 if / for 代码块 

	6. 变量可以被其它文件引用，只需在要引用的文件中 # include<xxx.h> 头文件即可，头文件只能引用一次(防止重定义) 
	
*/

```
<span class="yellow">
当函数的全局变量和局部变量同名时，优先使用局部变量 (就近原则</span>

<span class="yellow">这个全局，局部是相对的概念</span>

### 变量初始化注意事项

- **局部变量** :系统不会默认初始化，要先初始化才能使用

- **全局变量** :系统默认初始化为0
	
数据类型  | 默认初始化值
-- | --
int | 0
char | '\0'
float | 0.0
double | 0.0
pointer|NULL

正确初始化变量，防止因使用系统垃圾只，导致程序的异常终止

```c
# include<stdio.h>

int a;
float f;
double d1;
void main() {
	printf("\na=%d f=%f d1=%f",a,f,d1);
		getchar();
}

//打印

a = 0 f = 0.000000 d1 = 0.000000

```

此时的这个"全局"，到底是相对还是绝对呢?

```c
#include<stdio.h>

int a;
int b;

void test() {
	int c; // 报错
	printf("c=%d", c);
}

void main() {
	int d; // 报错
	printf("a=%d b=%d d=%d", a, b,d);
	getchar();
}

//打印


警告	C6001	使用未初始化的内存“c”。		

警告	C6001	使用未初始化的内存“d”。		

错误	C4700	使用了未初始化的局部变量“c”	

错误	C4700	使用了未初始化的局部变量“d”

// visual studio 2019中演示后的报错

// 所以，这个全局应该是这个文件来说的全局，所有函数的最外层

```

### 作用域的注意事项

**全局变量**(Global variable)保存在 内存的`全局储存区`中，占用静态储存单元

**局部变量**(Local variable)保存在`栈`中，函数调用时，`动态`的为变量分配储存单元，作用域仅限制函数内

c语言规定，只能从小作用域向大的作用域"寻找"变量，不能反过来，使用较小得作用域的变量

被 `{}`包裹的代码，也有独立的作用域



![](https://user-gold-cdn.xitu.io/2020/7/25/17384499111809c5?w=698&h=410&f=png&s=133235)

		中央(宪法) 和 地方 (地方法)
		
		地方的法律可以引用宪法

		但是中央的宪法不能引用地方法律

【案例】

```c
# include<stdio.h>

void fun() {
	int n = 20; //局部变量
	printf("fun1 n :%d\n", n);
}

void fun2(int n) {
	printf("fun2 n :%d\n", n);
}

void fun3(int n) {
	printf("fun3 n :%d\n", n);
}
void main() {
	int n = 30; //局部变量
	fun();
	fun2(n);
	fun3(n);

	//代码块由 {} 包围
	{
		int n = 40; //局部变量
		printf("block n :%d\n",n);
	}
	printf("main n : %d\n", n);
	getchar();

}

//打印

fun1 n : 20
fun2 n : 30
fun3 n : 30
block n : 40
main n : 30

```

### static 关键字

**局部变量使用 static修饰**

    1. 局部变量被 static修饰后，称为静态局部变量
    2. 对应静态局部变量未赋初值，编译器自动初始化为0
    3. 静态局部变量储存在内存静态储存区(全局性质),只会初始化一次,即使函数返回了，也不用像普通的局部变量销毁,而是不变

```c
# include<stdio.h>

int main(){
	
	static int n ; //n就是静态局部变量，默认初始化为0
	printf("%d",n); // 打印 0 
	getchar();
	return 0;
}




```


**全局变量使用static**

一个工程内(多个文件)

**普通全局变量** 整个工程可用，使用 `extern关键字`外部声明后使用，"共享的"，因此其它文件不能定义同名变量(防编译器重定义)
`(动态的->可以串门，到其它文件)`

**静态全局变量**仅在当前文件有效，其它文件不能访问使用，可以在其它文件定义同名变量 `(静态->"不动的，呆在当前文件内")`

定义不需要个其它文件共享的变量后，给变量加上static可以防止程序的耦合

![](https://user-gold-cdn.xitu.io/2020/7/25/173845c8ca27a96a?w=1613&h=371&f=png&s=292631)

![](https://user-gold-cdn.xitu.io/2020/7/25/173845e3567524e2?w=1120&h=633&f=png&s=472513)

```c

# include<stdio.h>


void fn_static(void){
	static int n = 10 ;//赋初值，静态局部变量
	printf("static n = %d",n) ;
	n++;
	printf("static n=%d",n);
}

int main(){
	printf("\n第一次调用\n");
	fn_static();
	printf("\n第二次调用\n");
	fn_static();
	return 0;

}

//打印

第一次调用
static n = 10static n=11
第二次调用
static n = 11static n=12 


/*
	静态变量只会初始化一次，"赖着不走" ，在全局区 

	普通变量在栈区，函数使用完就销毁 
*/

```

### 静态函数

`静态函数`之只能在当前文件使用


### 常用系统函数

函数名 | 描述 | 案例 
-- | -- |--
strlen(char *str) | 计算字符串 str的长度,直到空字符结束，但不包括空结束字符 |str = "hello" ;strlen(str) ;// 5
strcpy(char *目标，cahr *来源)| 把src指向的字符复制到dest| strcpy(char *c1,char *c2)
strcat() | 把src所指向的字符追加到dest所指向的字符串结尾 | strcat(char *c1,char *c2)

【案例】

```c


# include<stdio.h>
#include<time.h>

void test() {
	int i = 0;
	int j = 0;
	for (i = 1; i < 100; i++) {
		for (j = 1; j < 100; j++) {
			printf("i*j=%d\n", i * j);
		}
	}
}

void main() {

	time_t start_t, end_t;
	// 时间时结构体类型，因为包含多条数据
	double diff_t; //存放时间差
	printf("程序启动...\n");
	time(&start_t); //初始化得到当前时间

	test(); //就是一个函数，得到这个函数的执行消耗的时间

	//得到执行 test()后的时间

	time(&end_t);// 得到当前时间

	diff_t = difftime(end_t, start_t); //时间差，按 end_T - start_t

	printf("test()函数执行消耗的时间时 : %f", diff_t);
	getchar();
}

//打印

test()函数执行消耗的时间时 : 9.000000
```

### 常用数学函数


函数名 | 描述
-- |  --
double exp(double x ) |返回 e的x幂
double log(double x)   |返回x的自然对数 (基数为 e的对数)
double pow(double x,double y)|返回x的y次幂
double sqrt(double x)|返回x的平方根
double fabs(double x ) |返回x的绝对值

### 基本数据类型和字符串转换

```c
# include<stdio.h>


/*
	sprintf() ,把结果放入字符串
	sprintf(目标字符串,字符串) 
*/

// 【案例-基本数据类型转字符串类型】 
int main(){
	
	char str1[20]; //字符串数组，即字符串 
	char str2[20];
	char str3[20];
	
	int a = 20984 , b = 48090;
	double d =14.309948;
	sprintf(str1,"%d %d",a,b); //把输出的字符串"%d %d" 放入str1字符数组中
	sprintf(str2,"%.2f",d); 
	sprintf(str3,"%8.2f",d); //%8.2表示 共8位数，其中小数位占2位
	/* 对于   14.31来说 ，小数位2位，共5位，如果按照%8.2，
	则整数位5位，实际整数位2位，则有3位是没有的，没有的用空格代替 
	即
	str3=   14.31 (实际打印结果)
	
	str3=12314.31  (对比的)
	
	可见有三位是 空着的 
	*/
	printf("str1=%s\n\nstr2=%s\nstr3=%s\n",str1,str2,str3); 
	return 0;
}
// 打印

str1=20984 48090

str2=14.31
str3=   14.31 

```

```c
# include<stdio.h>
#include<stdlib.h> 


// 【案例-基本数据类型转字符串类型】 
int main(){
	
	char str[20]="123456";
	char str2[20]="12.67423";
	char str3[3]="ab";
	char str4[20]="111";
	char str5[20] = "hello";
	
	
	//1. atoi(str)将str转为整数
	int num1 = atoi(str); 
	short s1 =atoi(str4);
	
	//2 .stof(str2) 将str2转为 小数 
 	double d = atof(str2);
 	char c =str3[0]; //表示获取 str3字符串第一个元素 
 	int e = atoi(str5);
	printf("num1=%d\ns1=%d\nd=%f\nc=%c\n\ne=%d",num1,s1,d,c,e); 
	return 0;
}
// 打印

num1=123456
s1=111
d=12.674230
c=a

e=0
// 注意：
/*

	将char转为基本数据类型时，要确保可以转成有效数据，如
	字符串的内容是整数或小数
	如：  "123" 可以转为 整数 123 
	但是 "hello"  不能转为一个整数 
	
	如果格式不正确，默认转为0 ,如 e=0 

*/ 

```


#### 函数练习


```c
# include<stdio.h>
# include<string.h>
void printStar(int totalLevel){
	/*
	要求： 
		打印空心金字塔 
	分析: 
		化繁为简，先死后活 
		1.打印矩形
			int i j;
			for(i=1;i<=5;i++){ //控制行 
				for(j=1;j<=5;j++){ //控制列 
					print("*");
				}
			}
			return 0;	
		2.打印半个金字塔
			int i ,j;
			for(i=1;i<=5;i++){ //控制行 
				for(j=1;j<=i;j++){ //控制列 
					printf("*");
				}
				printf("\n");
			}	
			return 0;
		3.打印整个金字塔
			int i,j,k;
			for(i=1;i<=5;i++) {
				for(k=1;k<=5-i;k++){
					printf(" ");
				}
				for(j=1;j<=2*i-1;j++){
					printf("*");
				}
				printf("\n");
		 	}
			
			return 0;
			
		4.打印空心金字塔
			int i,j,k,level = 20;
				for(i=1;i<=level;i++) { //i控制层数 
					for(k=1;k<=level-i;k++){// k控制 空格何时输出 
						printf(" ");
					}
					for(j=1;j<=2*i-1;j++){ //J控制列数 (每行的*数)
						if(j==1 || j==2*i-1 || i==level){
						printf("*");
						}else{
						printf(" ");
					}
					
					printf("\n");
				}
			
			}
				
				return 0;
		5.打印空心菱形 
	*/	
	int i,j,k,level = totalLevel;
	for(i=1;i<=level;i++) { //i控制层数 
		for(k=1;k<=level-i;k++){// k控制 空格何时输出 
			printf(" ");
		}
		for(j=1;j<=2*i-1;j++){ //J控制列数 (每行的*数)
			if(j==1 || j==2*i-1 || i==level){
				printf("*");
			}else{
				printf(" ");
			}
			
			
		}
		printf("\n");
 	}
	
}
int main(){
	int totalLevel = 0;
	printf("请输入层数:"); 
	scanf("%d",&totalLevel);
	printStar(totalLevel);
	return 0;
}

```

<span class="yellow">把功能封装为一个函数，考虑输入，输出</span>

### 预处理指令

`预处理就是预先处理机制`

**具体要求:**

开发一个C语言程序，让它暂停5秒以后再输出内容"hello，尚硅谷!~"，并且要求跨平台，在Windows和Linux下都能运行，如何处理

**提示**

		1)Windows平台下的暂停函数的原型是void Sleep(DWORD dwMilliseconds)，参数的单位是“毫秒”，位于<windows.h>头文件。
		2) Linux平台下暂停函数的原型是unsigned int sleep (unsigned int seconds)，参数的单位是“秒”，位于<unistd.h>头文件
		3)#if、#elif、endif 就是预处理命令，它们都是在编译之前由预处理程序来执行的。

【案例】

```c
# include<stdio.h>
#if_WIN32 //如果是windows平台就执行 #include<windows.h>
#include<windows.h>
#elif_linux_ //否则如果是linux平台，就执行#include<unistd.h>
#include<unistd.h>
#endif
int main(){
	//不同平台下调用不同的函数
	#if_WIN32 //识别windows平台
	Sleep(5000); //毫秒
	#elif_linux_ //识别是 linux平台
	sleep(5);// 秒
	#endif
	puts("hello,world"); //输出
	getchar();
	return 0;
}

```
### 宏定义介绍

```c
# include<stdio.h>


/*
	【预处理命令】 
*/
#define M (n*n+3*n)
int main(){

	int sum , n;
	printf("input a number:");
	scanf("%d",&n);
	sum = 3*M+4*M+5*M;
	printf("sum=%d\n",sum);
	getchar();
	
	
	return 0;
}

```

	- 如果宏名用 ""引号括起来，则不会被宏替换

	- 宏定义可以被嵌套替换，层层展开即可

	- 习惯宏名为大写，也可以小写，一般大写(规范)

	- 宏定义是字符串替换，由预处理器处理

	- typedef 是编译器处理的

## 数组入门

【案例】

```c
# include<stdio.h>
# include<string.h>

int main() {
	/*
	一个养鸡场有6只鸡，它们的体重分别是3kg，5kg，1kg，
3．4kg，2kg，50kg。请问这六只鸡的总体重是多少？平
均体重是多少？请你编一个程序?
	*/
	// 1. 定义数组
	double hens[6] ;
	
	//2. 初始化数组元素 
	hens[0] =3;
	hens[1] =5;
	hens[2] =1 ;
	hens[3] =3.4 ;
	hens[4] =2 ;
	hens[5] =50 ;
	
	double totalWeight = 0.0;
	double avgWeight = 0.0;
	int arrlen;
	// 3. 遍历数组
	
	// 如何得出数组的大小
	arrlen = sizeof(hens)  / sizeof(double) ;
	// 这个大小指的是   固定单位字节的元素的 个数 
	int i; 
	for(i=0;i<arrlen;i++) {
		totalWeight += hens[i];
	}
	avgWeight= totalWeight / 6;
	
	printf("总体重= %f\n平均体重=%f",totalWeight,avgWeight);
	return 0;

}
//打印

//总体重= 64.400000
//平均体重=10.733333

```


**获取数组的大小?**

	· sizeof(数组名) / sizeof(double)

	· 数组总大小 / 单个数组元素的大小(数组的类型)


以字节为单位

<span class="yellow">数组默认传递地址</span>


![](https://user-gold-cdn.xitu.io/2020/7/25/173852f7d2172c58?w=388&h=94&f=png&s=16825)

### 数组的定义

		数据类型 数组名 [数组大小] ；

对于 数组 `a[4];`

		- 数组名 代表 该数组的首地址 ，即 a[0]的地址

		- 数组的各个元素是 连续分布的 ，假如 
		a[0]地址 0x1122 ,a[1]地址 = a[0]地址 + int字节数(4) = 0x1122 + 4 = 4 0x1126

		a[2] 的地址 = a[1]的地址 + int 字节数 = 0x1124+4 = 4 0x1128

		- 访问数组元素
			
			数组名[下标]   如三个元素 则是  a[2] ，即 a[0],a[1],a[2]三个


<span class="yellow">注意，数组是从0开始的，a[0] 代表第一个元素</span>

当定义数组的时候，`[]`内的数表示数组元素的个数，而不是代表数组下标的意思

**如**

		char a[3] ; //表示定义一个元素个数为3的a字符数组

		/*
		数组最大可用下标为 a[2]
		即 a[0] ,a[1] ,a[2]三个

		//千万不要写成a[3]，这样会数组越界
		*/
		a[2] = "hello"; //当使用数组时，此时的[ ]的数字，代表下标


	
	
**初始化数组的三种方式**

	- int arr2[3] = {10,20,30}; //带{}赋值
  
	- int arr3[] = {10,20,30} ;// 可以省略数组大小，系统会自动判断数组的元素的个数

	- int arr4[3] ;//先定义
		
	  arr4[3] = {10,20,30} ;// 再赋值

### 数组使用注意事项和细节

1. 数组是多个<span class="yellow">同类型</span>的数据的组合，声明/定义后，不能动态变化
2. 数组创建后，`全局变量默认赋初值0`，非全局数组为赋垃圾值
3. 数组的下标是从0开始的，指定下标范围使用数组，防止数组越界，如三个元素的数组，定义a[2],如果使用a[3]则越界
4. c语言的数组属于`构造类型`,是`引用传递`，当把数组传递个函数时，函数操作会改变数组

![](https://user-gold-cdn.xitu.io/2020/7/26/1738ae62f1aa7a69?w=800&h=591&f=png&s=86058)

【案例】

```c
# include<stdio.h>

void f1(int arr[]){
	printf("\nf1函数中的arr地址=%p",arr);
	arr[0] = arr[0] + 1;
	
}

int main(){
	
	int arr[3] = {3,4,5};
	
	int i ;
	printf("\nmain函数中的arr地址= %p",arr);
	
	
	f1(arr);
	for(i=0;i<3;i++){
		printf("\n\narr[%d]=%d",i,arr[i]);
		
	}
	return 0;
}
//打印

main函数中的arr地址= 000000000062FE10
f1函数中的arr地址=000000000062FE10
// 可见，main函数中的arr和f1函数中的arr是同一个arr，即地址传递 
arr[0]=4

arr[1]=4

arr[2]=5 




```

![](https://user-gold-cdn.xitu.io/2020/7/25/173853e23b9c887d?w=377&h=473&f=png&s=92204)

`数组的的数组名 = 数组首元素的地址`

### 字符数组和字符串

顾名思义

用于存放字符的数组

		char a[10];// 一维字符数组长度为10
		char b[5][10];// 二维字符数组
		char c[20] = {'c','p','r','o','g','r','a','m'} ;//给部分数组元素赋值

		字符数组其实就是一系列的字符的集合，即字符串(String),  c语言中没有专门的类型 string，用字符数组当字符串


字符数组以` \0`结尾

**字符数组和字符串** 

	► c语言中，字符串实际使用null(\0)终止一维字符数组
	
	► \0 是ascll中第0个字符，用null表示，c语言中仅作为字符串结束表示，不能显示，也不是控制字符，输入无效果
	
	► char str[3] = {'a','b','c'} ;
	输出什么?  
	
		输出 str = abc烫烫烫烫蘌{?

str[3] 表示这个数组的元素个数，可以存放`三个元素`，不是下标定义的时候的[数字] ,数字表示数组元素的个数

		若给某字符数组赋值，若赋给元素个数小于数组长度，则默认加\0 ,若元素个数等于数组长度，则不加 \0 

		char str[] ={'t','o','m'}; //这个是字符数组
		输出什么? 
		
			str = tom烫烫烫烫蘌{


![](https://user-gold-cdn.xitu.io/2020/7/25/1738546884db0bae?w=752&h=246&f=png&s=114929)

【案例】

```c
# include<stdio.h>

int main(){
	//c是一维数组，给部分元素赋值
	char c[20] = {'t','o','m'};
	char b[2] = {'j','a','c'};
	char greeting[] = "Hello";
	printf("%s\n%s\n%s",c,b,greeting);
	return 0;
}

//  取出单个字符  : 数组名[下标] 

//打印
tom
ja //因为数组长度为2
Hello 

```

```c
# include<stdio.h>
# include<string.h>

int main() {
	char str1[] = { "I am happy" }; // 默认后面加 \0 
	char str2[] = "i am happy"; // 省略 {} ,后面默认加 \0 
	char str3[] = { 'i',' ','a','m',' ','h','a','p','p','y' }; // 字符数组后面不会加\0，可能会乱码 
	char str4[5] = {'C','h','i','n','a'}; // 字符数组后面不会加 \0,可能会乱码 
	char* pStr = "hello"; // ok
	char str[3] = {'a','b','c'} ; //str[3]  ,四个位置(\0占一个) ，剩下3个给a.b,c 
	printf("\nstr1=%s", str1);
	printf("\nstr2=%s", str2);//ok 
	printf("\nstr3=%s", str3);//乱码 
	printf("\nstr4=%s", str4);//乱码 
	printf("\nstr = %s",str); // str = abc 
	return 0;

}
//打印

str1=I am happy
str2=i am happy
str3=i am happy烫烫烫烫烫i am happy
str4=China烫烫烫烫烫蘨 am happy烫烫烫烫烫i am happy
str = abc烫烫烫烫蘌{?

```

	char greeting[] = "Hello"

	这种方式，由于没有指定大小，所以根据实际的元素确定，同时自动的加上"\0"结尾标志，所以不用担心乱码的问题


**用字符数组存放字符串**

	char str[] = "hello" : 直接赋值整个字符串
	char str[] = {'h','e','l','l','o'} " 赋值单个字符集合
	
	char * pstr = "hello";

![](https://user-gold-cdn.xitu.io/2020/7/25/173859f70ec1d4f2?w=1266&h=442&f=png&s=235243)


**字符串表示形式**

	使用 "字符指针变量" 和 "字符数组" 两种方法表示字符串
	
	 字符数组 : 若干元素组成，每个元素存放一个字符

	 字符指针变量:  指针变量存放的是字符串/字符数组的首地址，不是将字符串放入字符指针变量中

**对于字符数组的赋值，只能单个元素赋值**

```c
	char str[14];
	str = "Hello tom"; //错误，不能整体赋值

	//实际上赋值字符串给数组名，其实就是把一个数给一个常量，这是错的
	str[0] = 'i';//可以
	

```

对于**字符指针变量**，采用下列方式是可以的


	char * a ;
	a = "hello tom" ; //字符串本质是地址，把一个字符串赋值给一个指针变量，实际是把这个字符串所在的内存的首地址赋值给变量，这样就确定字符串的位置

**字符数组存放字符串**


		1   char str[] = "hello tom " ;  //赋值字符串用""
		2   char str2[] = {'h','e'} ;// 用{}括起来的单字符元素形式


![](https://user-gold-cdn.xitu.io/2020/7/25/17385ac07267f2af?w=1106&h=587&f=png&s=408217)


【案例】

```c
# include<stdio.h>

int main(){
	
	char* pStr= "hello tom";
	
	char* a= "yes"; //把yes的地址给一个指针
	printf("当a = yes 时\na本身的地址 = %p a指向的地址=%p",&a,a);
	printf("\na= %s",a);
	a = "hello tom";
	printf("\n当a = hello tom  时\na本身的地址 = %p a指向的地址=%p",&a,a);
	printf("\na= %s",a);
}

//  取出单个字符  : 数组名[下标] 

//打印
当a = yes 时
a本身的地址 = 000000000062FE10 a指向的地址=000000000040400A
a= yes
当a = hello tom  时
a本身的地址 = 000000000062FE10 a指向的地址=0000000000404000
a= hello tom

// 可见a的字符不同，指向的地址也不同 


```

### 字符串数组注意细节

```c
# include<stdio.h>
# include<string.h>

int main(){
	char str1[12] = "hello";
	char str2[12] = "World";
	char str3[12] ;
	int len;
	// 复制str1到 str3
	strcpy(str3,str1) ; //此时str3= "hello"
	printf("\nstrcpy(str3,str1):%s",str3);
	
	// 连接str1到str2
	strcat(str1,str2);//此时str1 = "helloWorld"
	printf("\nstrcat(str1,str2): %s",str1);
	
	//连接后 str1的总长度 
	len = strlen(str1) ;
	printf("\nstrlen(str1): %d",len);
	
	return 0;

}
//打印

strcpy(str3,str1):hello
strcat(str1,str2): helloWorld
strlen(str1): 10

```

**字符数组注意细节**

	1. 程序中 依靠 \0 判断字符串是否结束，而不是靠数组的长度，【字符串】的长度是实际的输入的字符内容中字符的个数，所以不加 \0 
	
	2. 字符数组的长度是指的，加上 \0后的能判断数组结束的标志的那个长度，内存中显示的，所以字符数组的长度是要加上 \0的
	
	3. 数组长度是大于当前数组的实际的字符串长度的
	
	4. 系统对字符串常量也加 \0结束符，如 "C program" 9个字符，但是内存中占10个字节，最后一个字节\0是系统自动加上的(通过 sizeof()可以测试)
	
	5. 定义字符数组，若给定字符的长度小于数组长度，默认剩余的空间全部填入 \0
	如 char str[6] = "ab" str的内存布局就是 [a][b][\0][\0][\0][\0]

```c
# include<stdio.h>
# include<string.h>

int main() {
	char str1[] = { "I am happy" }; // 默认后面加 \0 
	char str2[] = "i am happy"; // 省略 {} ,后面默认加 \0 
	char str3[] = { 'i','a','m','h',' ','a','p','p','y' }; // 字符数组后面不会加\0，坑你会乱码 
	char str4[5] = { 'C','h','i','n','a' }; // 字符数组后面不会加 \0,可能会乱码 
	char* pStr = "hello"; // ok

	printf("\nstr1=%S", str1);
	printf("\nstr2=%S", str2);//ok 
	printf("\nstr3=%S", str3);//乱码 
	printf("\nstr4=%S", str4);//乱码 

	return 0;

}
//打印



```

序号 | 函数&目的 | 说明
-- | -- |--
1 | `strcpy(s1,s2)` ; |  复制字符串s2到s1(可覆盖)
2 | `strcat(s1,s2)` ;  | 连接字符串s2到s1的末尾
3 |` strlen(s1)` ; |返回字符串s1的长度
4 | `strcmp(s1,s2)`;| 比较字符串，如果s1和s2字符串相同，则返回0，如s1 < s2 ,返回小于0，如s1>s2 ，返回大于0  [`前面减后面`]
5 | strchr(s1,ch) ; |返回指针，指向字符串 s1中字符 ch第一次出现的位置
6 | strstr(s1,s2) ; |返回指针，指向字符串s1中字符串s2第一次出现的位置

## 冒泡排序

排序

**内部排序**  : 将所需要处理的所有数据全部加载到`内部储存器(内存)`中进行排序

**外部排序法** : 数据量过大，无法加载全部到内存，需要外部内存进行排序

**冒泡排序的过程演示 (从小到大)**

```c
原始数组 {3,9,,-1,10,-2}

第一轮

1) 3,9,-1,10,-2
2) 3,-1,9,10.-2
3) 3,-1,9,10,-2
4) 3,-1,9,-2,【10】 //第一大的数移到最后
//每一次对比一次就记录过程

第二轮

1) -1,3,9,-2,10
2) -1,3,9,-2,10
3) -1,3,-2,【9】,10 //第2大的数移动到适当位置，排序结束

第三轮

1) -1,3,-2,9,10
2）-1,-2,【3】,9,10 ////第3大的数移动到适当位置，排序结束

第四轮

1) -2,-【1】,3,9,10 //第四大的数移动到适当位置，排序结束
```

【冒泡排序代码实现】

```c
# include<stdio.h>
// 冒泡排序代码实现
void main() {
int arr[] = { 3,9,-1,10,-2,4,7,3,73,7,3 }; //待排序序列
因为每轮排序基本一样，可以使用for循环套用
//第一轮排序
(1) 3,9,-1,10,-2
(2) 3,-1,9,10,-2
(3) 3,-1,9,10,-2
(4) 3,-1,9,-2,10
比较4次
int j;
int t; //临时变量
int i; 
int arrlen = sizeof(arr) / sizeof(int); // 5
for (i = 0; i < arrlen; i++) {
	for (j = 0; j < arrlen-1 -i; j++) {
	//如果前面的数大于后面的数，交换
	if (arr[j] > arr[j + 1]) {
	t = arr[j];
	arr[j] = arr[j + 1];
	arr[j + 1] = t;
	}
}
//输入看第一轮的排序后的情况
printf("排序后的结果:\n");
for (j = 0; j < arrlen; j++) {
	printf("%d ", arr[j]); // 3 -1 9 -2 10
	printf("\n");
}
// -----------------------
////第二轮排序
//for (j = 0; j < 3; j++) {
////如果前面的数大于后面的数，交换
//if (arr[j] > arr[j + 1]) {
//t = arr[j];
//arr[j] = arr[j + 1];
//arr[j + 1] = t;
//}
//}
////输入看第2轮的排序后的情况
//for (j = 0; j < 5; j++) {
//printf("%d ", arr[j]); // 3 -1 9 -2 10
//}
//printf("\n");
//// -----------------------
////第3轮排序
//for (j = 0; j < 2; j++) {
////如果前面的数大于后面的数，交换
//if (arr[j] > arr[j + 1]) {
//t = arr[j];
//arr[j] = arr[j + 1];
//arr[j + 1] = t;
//}
//}
////输入看第3轮的排序后的情况
//for (j = 0; j < 5; j++) {
//printf("%d ", arr[j]); // 3 -1 9 -2 10
//}
//printf("\n");
////------------------------------
////第4轮排序
//for (j = 0; j < 1; j++) {
////如果前面的数大于后面的数，交换
//if (arr[j] > arr[j + 1]) {
//t = arr[j];
//arr[j] = arr[j + 1];
//arr[j + 1] = t;
//}
//}
////输入看第4轮的排序后的情况
//for (j = 0; j < 5; j++) {
//printf("%d ", arr[j]); // 3 -1 9 -2 10
//}
//printf("\n");
getchar();
// 打印 
3 -1 9 -2 10
-1 3 -2 9 10
-1 -2 3 9 10
-2 -1 3 9 10

```

封装为函数后的算法

```c
# include<stdio.h>
// 冒泡排序代码实现
// 封装为函数
void bubbleSort(int arr[],int arrlen) {
	int j;
	int t; //临时变量
	int i;
	for (i = 0; i < arrlen; i++) {
	for (j = 0; j < arrlen - 1 - i; j++) {
	//如果前面的数大于后面的数，交换
	if (arr[j] > arr[j + 1]) {
	t = arr[j];
	arr[j] = arr[j + 1];
	arr[j + 1] = t;
	}
	}
	}
	//输入看第一轮的排序后的情况
	printf("排序后的结果:\n");
	for (j = 0; j < arrlen; j++) {
	printf("%d ", arr[j]); // 3 -1 9 -2 10
	}
	printf("\n");
}
// ------------------
void main() {
int j;
int arr[] = { 3,9,-1,10,-2,4,7,3,73,7,3 }; //待排序序列
/*
因为每轮排序基本一样，可以使用for循环套用
*/
int arrlen = sizeof(arr) / sizeof(int); // 5
bubbleSort(arr,arrlen);
getchar();
}
// 打印 
/*
3 -1 9 -2 10
-1 3 -2 9 10
-1 -2 3 9 10
-2 -1 3 9 10
*/

```

## 顺序查找和二分查找

```c
# include<stdio.h>
int seqSearch(int arr[], int arrlen, int val) {
int i;
for (i = 0; i < arrlen; i++) {
if (arr[i] == val) {
return i;
}
else {
return -1; //没有找到
}
}
}
void main() {
/*
有一个数列 {23,1,34,89,101}
猜数游戏，从键盘任意输入一个数，判断数列是否包含该数【顺序查找】
找到就提示找到，并给出下标值，否则提示没有找到
*/
int arr[] = { 23,1,34,89,101 };
int arrlen = sizeof(arr) / sizeof(int);
int index = seqSearch(arr, arrlen, -101);
if (index != -1) { //找到，返回-1表示没有找到
printf("找到下标为%d", index);
}
else {
printf("没有找到"); // 没有找到
}
getchar();
}



```

[二分查找待续
]()

## 二维数组

	Ø 若对部分元素赋值，其余元素赋值为0
	Ø 若对全部的元素赋值，第一维长度可不写(系统自动计算)
	Ø二维数组可看成一维数组的嵌套


## 断点调试介绍

	Ø 断点调试可以在某一行设置断点，程序运行到断点，会停住，一步一步调试，可以看个各变量的值，如，出错，则可以显示错误

	Ø 可以找bug

	Ø 断点调试是程序员必备的技能，也能帮助程序员理解程序的过程

**快捷键**

快捷键 | 描述
-- | --
f5 | 开始调试，执行到下一个断点
f11 | `逐句`执行代码，会进入函数
f10 |` 逐过程`执行代码，遇到函数，不会进入函数体
shift + f5 | 终止调试
	
![](https://user-gold-cdn.xitu.io/2020/7/25/17385cae0ed655a3?w=728&h=584&f=png&s=41453)

![](https://user-gold-cdn.xitu.io/2020/7/25/17385cb5bd792cfb?w=538&h=532&f=png&s=49721)

![](https://user-gold-cdn.xitu.io/2020/7/25/17385cc8bbaf3966?w=858&h=620&f=png&s=67101)

![](https://user-gold-cdn.xitu.io/2020/7/25/17385ccdc410df14?w=414&h=916&f=png&s=56676)

```c
	
	# include<stdio.h>
	// 【案例,查看变量的情况】
	void main() {
	int sum = 0;
	int i = 0;
	for (i = 0; i < 10; i++) {
	sum +=i;
	printf("\n i =%d", i);
	printf("\n sum=%d", sum);
	}
	printf("退出for循环了~~~");
	getchar();
	}
	


```

## 指针

### 指针算数运算

若 有一个指针 ptr

指针的算数运算，实际上指的是，<span class="yellow">以ptr指向的变量( *ptr)的类型的所占的字节为单位</span>，的`位移`

如 *ptr 所指向的变量是 int a ，所占字节就是4 Byte,

		ptr +1 ,就是ptr指针往+1的方向(往后移动4个字节)，即下一个变量  / 又或者说，ptr储存的地址 + 1个int 类型 单位的字节(4字节)

		同样 ptr -1 ，就是ptr指针指向的位置，往前移动4个字节的长度


【案例】

```c
# include<stdio.h>

int main() {
	int var[] = { 10,100,200 };
	int * ptr;
	
	ptr = var; //将 var的首地址赋值给 ptr
	printf("ptr初始指向的变量的值 =%d\nptr储存的地址=%p\n\n", *ptr, &ptr);
	ptr += 2;// 将 ptr储存的地址 + 2个int类型的字节(8字节)
	printf("var[2]=%d \nvar[2]的地址=%p \nptr储存的地址=%p \nptr指向的地址的值=%d", var[2], &var[2], ptr, *ptr);

	getchar();
	return 0;
}

//打印

ptr初始指向的变量的值 =10
ptr储存的地址=00B9FBC4

var[2]=200
var[2]的地址=00B9FBD8
ptr储存的地址=00B9FBD8
ptr指向的地址的值=200



// 很明显，开始的ptr指向的是首元素地址,即10,现在指向了200，即加了2个字节
```

### 指针数组

<span class="yellow">就是一个数组 的元素存放的是某一个类型数据的地址</span>

![](https://user-gold-cdn.xitu.io/2020/7/25/17385ed08a367227?w=1087&h=333&f=png&s=305920)

<span class="yellow">把一个字符串赋值给指针 ，本质是把字符串的地址(首地址)赋值给指针</span>

本质因为，字符串传递是 地址传递

【案例】

```c
# include<stdio.h>
void main() {
char* books[] = {
"三国演义",
"西游记",
"红楼梦",
"水浒传"
};
// 遍历
int i, len = 4;
for (i = 0; i < len; i++) {
printf("\nbook[%d] 指向的字符串是=%s", i, books[i]); // 不能是*books[i]哦!
}
getchar();
}
// 打印
book[0] 指向的字符串是 = 三国演义
book[1] 指向的字符串是 = 西游记
book[2] 指向的字符串是 = 红楼梦
book[3] 指向的字符串是 = 水浒传





```

### 多重指针数组

```c
# include<stdio.h>
int main() {
	int var;
	int* ptr;// - 级指针
	int** pptr; //2级指针
	int*** ppptr; // 三级指针
	var  = 3000;
	ptr = &var; // var变量的地址赋给ptr
	pptr = &ptr;//表示将ptr 存放的地址,赋给pptr
	ppptr = &pptr; //表示将pptr存放的地址, 赋给peptr
	printf("var的地址=%p var= %d \n", &var, var);// 0x11333000
	printf("ptr的本身的地址=%p ptr存放的地址=%p *ptr = % d\n", &ptr, ptr, *ptr );
	printf("pptr本身地址= %p pptr存放的地址=%p **pptr = %d\n", &pptr, pptr, **pptr);
	printf("pptr本身地址= %p ppt存放的地址=%p ***pptr = % d\n", &ppptr, ppptr, ***ppptr);
	getchar();
	return 0;	
}
// 打印
var的地址 = 0057F9D0 var = 3000
ptr的本身的地址 = 0057F9C4 ptr存放的地址 = 0057F9D0 * ptr = 3000
pptr本身地址 = 0057F9B8 pptr存放的地址 = 0057F9C4 * *pptr = 3000
pptr本身地址 = 0057F9AC ppt存放的地址 = 0057F9B8 * **pptr = 3000

```

### 指针的传递

		指针--->目的为 "定位" 数据
		变量--->目的是 "定位" 数据

		① : 先考虑思维的流畅性
		②: 再考虑代码的合理性

### 返回指针的函数

c语言 ，中一个函数的返回值类型是  指针类型 ，这样的函数称为 指针函数


【案例】

```c
# include<stdio.h>
# include<string.h>

char *strlong(char *str1,char *str2){
	printf("\nstr1的长度=%d\nstr2的长度=%d",strlen(str1),strlen(str2));
	if(strlen(str1) >= strlen(str2)){
		return str1;  
	}else{
		return str2;
	}
}

int main(){
	char str1[30] ,str2[30],*str ;//str是指针类型,指向一个字符串
	printf("\n请输入第1个字符串:");
	gets(str1);
	printf("\n请输入第2个字符串:");
	gets(str2);
	str = stelong(str1,str2);
	printf("\nLonger string is :%s\n",str);
	getchar();
	return 0;

	//使用指针函数的关键是，这个返回值需要是地址，需要有这种需求
	
	// 这里的例子是字符串，字符串本身就是地址

	//本质，变量是一个 "容器" ， 重点是里面的 值，这个值需要容器储存

	字符串是地址传递，实际就是把这个地址值，进行传递

}

```

**指针函数注意事项**

		1. 用指针做为函数返回值，函数运行结束，会销毁它内的所有局部数据
            - 局部变量
            - 局部数组
            - 形式参数
		2. 函数运行结束，会销毁所有的局部数据，销毁不是真正得销毁，而是放弃所有的使用权限，后面还是可以使用这个数据的


```c
# include<stdio.h>

int *func(){
	int n =100; //局部变量，在func返回时，会销毁
	return &n ; //报错
}

int main(){
	int *p = func(); //返回指针(地址)
	int n ;
	printf("okook~~~~"); //
	n = *p;
	printf("\nValue = %d",n); //思考，是否能输出100?
	getchar();
	return 0;
}

//报错
C4172	返回局部变量或临时变量的地址: n	Project1	


````

<span class="yellow">我依然在你心，可你的心中没有我的位置
</span>


c语言不支持调用函数时，返回局部变量的地址，若确有需求，可以使用statci定义局部变量为static变量

![](https://user-gold-cdn.xitu.io/2020/7/25/173860d8a41cff14?w=927&h=460&f=png&s=53551)

![](https://user-gold-cdn.xitu.io/2020/7/25/1738614957f5917f?w=983&h=488&f=png&s=54857)


### 函数指针内存分布

1. 函数内存中占连续的区域，函数名在表达式中有时也会转换为该函数内存区域的首地址，和<span class="yellow">数组名类似</span>

2. 把函数的首地址赋值给指针变量，使指针变量指向函数所在内存区域，然后，通过这个指针可以找到并调用函数，<span style="color:red;font-size:16px">这个指针就是函数指针</span>

**函数指针(指向函数的指针)**

		returnType (*pointerName)(param list);

		1) returnType //函数返回值类型
		2) pointerName //指针名称
		3） param list //函数参数列表
		4） 参数列表可以同时给出类型和名称，也可以省略类型，直接写名称
		5) {}优先级大于 *，{}一定要写
	
【案例】

```c
int (*pmax)(int x,int y) = max ;
printf("Input two numbers:");

scanf("%d %d",&x,&y);

// (*pamx)(x,y)通过函数指针调用函数 max
maxVal= (*pmax)(x,y);
printf("Max value:%d pmax=%p pmax本身的地址 = %p\n",maxVal,pmax,&pmax);
getchar();
getchar();
return 0;


```

### 回调函数

回调函数就是基于，[函数指针](#函数指针内存分布),把一个函数当作另一个函数的参数，如果说要当成参数的话，这个函数就有返回值，实际上，就是为了获取函数的值，作为新的函数参数输入

**【案例-使用回调函数，给一个整型数组int arr[10] 赋值10个随机值】**

```c
# include<stdio.h>
# include<stdlib.h>
/*
用函数指针,实现会函数的调用，返回两个整数的最大值
*/
/*
*/
/*
回调函数

1. int (*f)(void) ：表示接收一个函数，这个函数，返回int 类型，没有形参的函数
2. f 是函数指针，它可以接收一个函数，这个函数的返回值是 int 类型，无参数的函数，即getNextRadomvalue
3. f 在这里被initArry调用，充当回调函数的角色(形参)
4. 需要传入函数的地址(先找到这个函数)，再使用
5. int (*f) (void) : 表示找到符合这样函数特征(返回值int ，无参数),且地址为(*f)的函数
*/


void initArry(int* arry, int arrySize, int(*f)(void)) {
	int i;
	//循环10
	for (i = 0; i < arrySize; i++) {
		arry[i] = f();
	/*
	通过函数指针调用 getNextRandomValue函数

	这里的  arry[i] = f();就相当于 调用

	arry[i] = getNextRandomValue();

	(*f)() 这样也可以
	*/



	
	}
}



// 获取随机值
int getNextRandomValue(void) {
	return rand();// rand()系统函数，生成随机数
}


int main() {
	int myarry[10]; //定义数组和 int 
	/*
	1. 调用 inintarry函数
	2. 传入一个函数名为getNextRandomValue的地址，需要用函数指针接收
	*/
	initArry(myarry, 10, getNextRandomValue);//这里的函数名作为函数的地址
	int i;
	for (i = 0; i < 10; i++) {
		printf("%d\n", myarry[i]);
	}
	printf("\n");
	getchar();
	getchar();
	return 0;
}
// 打印
41
18467
6334
26500
19169
15724
11478
29358
26962
24464

// 为什么要采用 指针传递？

指针运算快速，减少内存开销

假如b函数定义在a函数之后，也可以使用b函数，而不会出错


```

### 空指针

```c
# include<stdio.h>

void main(){
	int *p =NULL; //p赋值空指针
	int num = 34;
	p = &num ;
	printf("p= %d",p);
	getchar();
}

//合理使用空指针给指针变量初始化是一个良好的编程习惯，防止获取系统垃圾值
```

		NULL指针是定义在标准库<stdio.h>中的值为0的常量 #define NULL 0 


## 动态内存分配

**c语言，不同数据在内存中的分配**

	Ø 全局变量--内存中的静态储存区(比较稳定)

	Ø 非静态的局部变量--内存的动态储存区(局部->随时可以被销毁),stack栈内存

	Ø 临时的数据--建立动态内存分配区，需要随时分配(更加随意了)，不需要则能及时释放--heap 堆

因为是临时，所以呢，不需要正式的声明/定义啊(反正就是用了就释放的，如同临时的身份证明，不需要正式的像身份证申请一样)，所以不能通过正式声明/定义的数组名/变量名引用，<span class="yellow">只能使用指针</span>

**动态内存分配的相关函数**

在头文件中 `#include<stdlib.h>`中声明了4个有关于内存分配的函数


**函数原型` void * malloc (unsigned int size) // memory allocation`**

[malloc用法](https://www.runoob.com/cprogramming/c-function-malloc.html)

		这个函数，用于动态的在内存的堆区，开辟一个大小为size的空间，形参size为无符号的整型，这个函数同时返回一个值，这个值是这个开辟的内存空间的首地址，也就是这个函数是指针型函数

		侧重 【总共】 开辟空间

<span class="yellow">这个函数是指针型函数</span>

**函数原型 `void *calloc (unsigned n ,unsigned size) `**

[malloc用法](https://www.runoob.com/cprogramming/c-function-calloc.html)

		同样动态开辟空间，只不过是开辟n个大小为 size(字节为单位)的大于一般开辟空间的大小的空间  (足以保存一个数组)

		用 calloc(）函数可为一维数组开辟动态空间，n个数组元素，每个数组元素 长度size

		同样，返回此空间的起始位置的地址，分配不成功，返回NULL

		如 p = calloc(50,4) ;// 表示开辟50个长度为4字节的临时空间，起始地址赋值给p

**函数原型` void free (void *p) `**

		在最近一个开辟动态空间且把初始地址赋值给 p的前提下，使用 free(p)  ,释放这个空间，使得可被重新利用/其它变量使用
		
		free()函数无返回值 
		free(p)  ;// 释放p指针变量指向的动态分配的空间

**函数原型  `void *realloc (void *p, unsigned int size)`**

		重新分配通过 malloc() / calloc()函数 获取的动态内存空间，主要是改变大小size,但是p指向不变
		
		分配失败返回 NULL
		
		如 realloc(p,50) ; //将p 指针所指向的分配的动态空间，重复的设置大小为 50 字节 (总共的)


![](https://user-gold-cdn.xitu.io/2020/7/26/17386cd20e887255?w=1000&h=338&f=png&s=39774)


**动态内存分配基本原则**


		1) 避免分配大量的小空间内存块，分配堆的内存有系统开销
		2) 仅在需要时分配内存。用完堆内存，就要及时释放。遵循谁分配，谁释放原则，否则会出现【内存泄露】
		3) 在释放内存之前，确定不会覆盖堆已经有的内存地址，否则程序会出现【内存泄露】，循环分配内存时，更加需要注意

## 结构体

**对于一个猫来说**

		提取所有可以描述的信息(数据)，或者说特征
		⬇
		Cat结构体数据类型
		列出这些数据特征
		⬇
		变量1，变量2，...

		再如
		提取人的特征，可以被描述的数据
		⬇
		Person结构体
		列出可表示特征的数据变量
		⬇
		变量1，变量2...

【案例】

```c
	
	# include<stdio.h>
	void main() {
	/*
	分析
	张老太养了两只猫猫:- -只名字叫小白，今年3岁，白色。还有一只叫小花今年100岁花色。
	请编写一个程序,当用户输入小猫的名字时,就显示该猫的名字,年龄,颜色。
	如果用户输入的小猫名错误,则显示张老太没有这只猫猫。
	猫?
	1. 变量
	2. 使用结构体完成
	*/
	struct Cat { // 结构体名 Cat ,自己编写的
	char* name;
	int age;
	char* color;
	};
	// 使用Cat创建对应的变量
	struct Cat cat1; //cat1 是 struct Cat类型的变量
	struct Cat cat2; //cat2 是 struct Cat类型的变量
	// 给cat1赋值
	cat1.name = "小白";
	cat1.age = 3;
	cat1.color = "白色";
	// 给cat2赋值
	cat2.name = "小花";
	cat2.age = 100;
	cat2.color = "花色";
	//输入猫的信息
	printf("\n第一只猫 name=%s age=%d ,color=%s\n第一只猫 name=%s age=%d ,color=%s\n", cat1.name, cat1.age, cat1.color, cat2.name, cat2.age, cat2.color);
	getchar();
	}
	//打印
	第一只猫 name = 小白 age = 3, color = 白色
	第一只猫 name = 小花 age = 100, color = 花色

```


### 结构体内存布局

		1) 结构体类型是一种自定义的数据类型
		2) 结构体变量代表一个具体的变量
			int num1 ;  // int 数据类型，而num1则是一个具体的变量
			struct Cat cat1;  //同样,struct Cat是一个数据类型，则cat1是具体的变量cat1

			//"类型"表示作了限制



![](https://user-gold-cdn.xitu.io/2020/7/26/17386e6b10b3d5b1?w=1048&h=356&f=png&s=45636)


### 结构体成员

**格式**

```c
// 声明结构体

struct 结构体名 {
	成员列表;
	...
}; //注意不要忘了 ; 

如 
struct Student{
	char *name;
	int age;
	int num;
	float score; 
}

// 在创建结构体变量后，要给成员赋初值，否则，可能会导致程序异常终止

// 不同的结构体变量的成员是独立的，互不影响

// 下面是一个完整的结构体定义，声明，调用全过程

// ---------------开始-----------------

# include<stdio.h>

void main(){
	struct Cat{
		char *name;
	    int age;
		char *color;//因为字符串是地址传递，所以用指针类型接收
	};

	//创建结构体变量
	struct Cat cat1; //定义结构体变量 cat1
	//如果此时 没有给结构体的成员变量赋初值，则可能会指向内存中的垃圾值，导致程序出错
	printf("\n名字=%s 年龄=%d 颜色=%s",cat1.name,cat1.age,cat1.color);
	/*
	
	结构体变量的访问 :

	 结构体变量名 . 结构体成员变量
	 如
	 cat1.name

	 */
	getchar();
}

```

### 结构体变定义形式

		1. 先定义结构体，再创建结构体变量
   
		如
		struct Stu{
			char *name;
			int num;
			int age;
			char *group;
			float score;
		};

		struct Stu stu1,stu2;

		2. 定义结构体同时定义结构体变量

		struct Stu{
			char *name;
			int num;
			int age;
			char *group;
			float score;
		}stu1,stu2;

		3. 定义匿名结构体(无结构体名)

		struct{
			char *name;
			int num;
			int age;
			char *group;
			float score;
		}stu1 ,stu2;

		成员的获取和赋值

		struct {
			char *name;
			int num ;
			int age;
			char *group;
			float score;
		}stu1={"贾宝玉"m11,18,'8',90.50},stu2={...}


【案例】

```c
# include<stdio.h>
void main() {
// 定义结构体同时赋值
struct Student {
char* name;
int num;
int age;
char group;
float score;
}stu1 = { "贾宝玉",11,18,'B',90 }, stu2 = { "林黛玉",12,16,'A',100 };
/*
1. 先创建变量同时赋值
struct Student stu3 = { "林黛玉",12,16,'A',100 };
*/
/*
struct Student stu4 ;
stu4 = {"smith",12,16,'A',100};
这样先创建变量，后整体赋值是不行的
只能一个一个的赋值
*/
printf("\n stu1.name=%s", stu1.name);
printf("\n stu2.name=%s", stu2.name);
getchar();
}
// 打印
stu1.name = 贾宝玉
stu2.name = 林黛玉

```

```c
 
#pragma warning(disable:4996)
#include<stdio.h>
/*
编写一个Dog结构体,包含nafne(char[10])、 age(int). weight(double)属性
编写一个say函数,返回字符串,方法返回信息中包含所有成员值。
在main方法中,创建Dog结构体变量,调用say函数,将调用结果打印输出。
*/
//定义结构体
struct Dog {
char* name;
int age;
double weight;
};
// say函数，返回字符串，信息包含所有成员值
char* say(struct Dog dog) {
//将信息放入字符串(字符数组)
static char info[50];// 这个是局部变量，应用于函数返回
sprintf(info,"name=%s age =%d weight=%.2f", dog.name, dog.age, dog.weight);
return info;
}
void main() {
struct Dog dog;
char* info = NULL;
dog.name = "小黄";
dog.age = 1;
dog.weight = 3.4;
info = say(dog); //结构体变量默认是值传递
printf("小狗的信息是\n%s", info);
getchar();
}
小狗的信息是
name = 小黄 age = 1 weight = 3.40






```


异常处理['sprintf':This function or varrable may be unsafe Consider using sprintf_s instead ,To disable deprecation ,use _CRT_SECURE_NO_WARNNING,See online help for details](https://docs.microsoft.com/zh-cn/cpp/error-messages/compiler-warnings/compiler-warning-level-3-c4996?f1url=https%3A%2F%2Fmsdn.microsoft.com%2Fquery%2Fdev16.query%3FappId%3DDev16IDEF1%26l%3DZH-CN%26k%3Dk(C4996)%26rd%3Dtrue%26f%3D255%26MSPPError%3D-2147217396&view=vs-2019)

解决

			所有头文件加 
	#pragma warning(disable:4996)

##共用体

		1. 共用体(Union)  属于构造类型,可包含多种类型不同成员，可结构体类似
		2. 定义格式
			union 共用体名 {				成员列表
			};
		3. 结构体会占用不同的地方，共用体的【所有】成员占用一段内存，修改其中一个，会影响其余的成员

【案例】

```c

// 1. 先定义
union data{
	int n;
	char ch;
	double f;

};
//后创建
union data a,b,c;

//2. 定义时，创建
union data{
	int n;
	char ch;
	double f;

}a,b,c;

//3. 匿名
union {
	int n;
	char ch;
	double f;

}a,b,c;

// 例子

// ---------------------开始----------------

#include<stdio.h>

union data{ //data就是一个共用体，包含3个成员，共用数据空间，以该空间占用内存最大的成员的空间为主
	int n; //最大的是int 类型变量
	char ch;
	short m;
};

void main(){
	union data a; //创建
	printf("%d , %d\n",sizeof(a),sizeof(union data));

	getchar();
}
// 打印

4 , 4

// 那么这个共用体的大小，就是以int类型为准

// --------------------------------
#include<stdio.h>

union data{ //data就是一个共用体，包含3个成员，共用数据空间，以该空间占用内存最大的成员的空间为主
	// int n; //最大的是int 类型变量
	char ch;
	short m;
};

void main(){
	union data a; //创建
	printf("%d , %d\n",sizeof(a),sizeof(union data));

	getchar();
}

//打印

2 , 2
// 这是就以 short类型为主
```

![](https://user-gold-cdn.xitu.io/2020/7/26/17389f5e43801d2c?w=1225&h=606&f=png&s=67708)


**大的 n 会影响全部数据，而小的 ch , m会影响部分数据** (<span class="yellow">这点容易理解</span>)

【共用体案例】

```c
//# include <cstdio>
# include<stdio.h>

#define TOTAL 1

struct Person {
	char name[20];//人员总数
	int num;
	char sex;
	char profession;
	union { //匿名共用体，只用一次
		float score;
		char course[20];
	}sc;// sc是一个共用体
	//union MYUNION sc;
};

//union MYUNION {
//	float score;
//	char cource[20];
//
//};

void main() {
	int i;
	struct Person person[TOTAL];
	//输入人员信息
	for (i = 0; i < TOTAL; i++ ){
		printf("Input info:");
		printf("\nname,num,sex,profession:\n");
		scanf("%s %d %c %c", person[i].name, &(person[i].num), &(person[i].sex), &(person[i].profession));
		if (person[i].profession == 's') { //如果是学生
			printf("请输入该学生成绩:");
			scanf("%f", &person[i].sc.score);
		}
		else { //如果是老师
			printf("请输入老师课程:");
			scanf("%s", person[i].sc.course);
		}
		fflush(stdin); //刷新
	}
	// 输出人员信息
	printf("\n name\t\tNum\t\tSex\t\tProfession\t\tScore\t\tCourse\n");
	for (i = 0; i < TOTAL; i++  ) {
		if (person[i].profession == 's') { //如果是学生
			printf("%s\t\t%d\t\t%c\t\t%c\t\t\t%f\t\t%s\n", person[i].name, person[i].num, person[i].sex, person[i].profession, person[i].sc.score, person[i].sc.course[i]);
		}
		else { //如果是老师
			printf("%s\t\t%d\t\t%c\t\t%d\t\t%s\n", person[i].name, person[i].num, person[i].sex, person[i].profession, person[i].sc.course[i]);
		}
	}
	getchar();
}


```


## 文件

文件时数据源(保存数据的地方)的一种，比如大家经常使用的word文档，txt文件，excel文件，都是文件，文件时最主要的作用就是**保存数据**，它可以保存一张图片，视频

![](https://user-gold-cdn.xitu.io/2020/7/26/1738ab34995da9ce?w=675&h=489&f=png&s=45663)

### 文件基本介绍


**标准文件**

标准文件 | 文件指针 | 设备
标准文件 | stdin | 键盘
标准输出| stdout | 屏幕
标准错误 | stderr | 您的屏幕

**文件指针**是访问文件的方式





函数| 说明
-- | --
getchar() | `int getchar(void)` 函数从屏幕读取下一个可用的字符，返回为整数，同一时间只能读取一个单一的字符，可以循环使用，获取多个字符
putchar()| `int putchar(int c)` 输出字符到屏幕，并返回相同的字符，同一时间只能读取一个单一的字符，可以循环使用，获取多个字符

【案例】

```c
# include<stdio.h>
int main(){
	itn c ;
	printf("Enter a Value : ");
	c = getchar();//读取一个char，返回一个int  (即单个字符)

	printf("\n You entered is : ");
	putchar(c) ;//可以在屏幕上显示
	printf("\n");
	getchar(); //暂停屏幕
	return 0;
}
```

```c
# include<stdio.h>
int main() {
	char str[100]; //字符串数组
	printf("输入字符串:");
	gets(str);
	printf("\n你刚刚输入的字符串是:");
	puts(str);
	getchar();
	return 0;
}
//打印

输入字符串:martin
你刚刚输入的字符串是 : martin

```

函数| 说明
-- | --
scanf(const char *format,...) |.
printf(const char *format,...) |.


### fopen,fclose


**打开文件**

fopen (file open)

函数调用原型

<span style="color:#6581ff;font-weight:bold">FILE *fopen(const char *filename,const char *mode);</span>

		1) filename是字符串，用来命名文件，访问模式，是可选的
		2) 如果需要处理二进制文件(视频，图片)，使用以下的模式

		- "rb" 
		- "wb"
		- "ab"
		- "rb+"
		- "r+b"
		- "wb+"
		- "w+b"
		- "ab+"
		- "a+b"

模式 |描述
r |  打开【已有】文件，允许读取read
w | 打开文件，若没有，则新建，若有，则重写原文件内容
a | append,打开文件，追加，若不存在，则新建后追加
r+ | 等价 r/m,打开文本文件，允许读取
w+ | xxx
a+ | xxx

<span class="yellow">如果打开的是二进制文件(图片，视频)打开形式要加 b(+b) binary</span>

**关闭文件**

函数原型

<span style="color:#6581ff;font-weight:bold">int fclose(FILE *fp)</span>

		1) 如，成功关闭，则返回0，若关闭发生错误，返回EOF,这个函数，实际是要清空缓冲区的数据，关闭文件，释放该文件的所有内存，EOF是定义在stdio.h中的常量
		2) c标准库提供各种函数按【字符串】或【固定字符长度字符串】读取文件

### 写文件

函数 | 说明
-- | --
<span style="color:#6581ff;font-weight:bold">int fputc(int c ,FILE *fp);</span> |把字符串 `c`写入fp 指向的输出流，成功，返回写入的字符，失败，返回EOF
<span style="color:#6581ff;font-weight:bold">int fputs(const char *s,FILE *fp);</span> |把字符串 s写入 fp 指向的输出流，成功，则返回非负值，错误，则返回 EOF,也可以使用 `int fprintf(FILE *fp,const char *format,...)将字符串写入文件`

```c
# include<stdio.h>
void main() {
	// 创建文件指针
	FILE * fp = NULL;
	//打开该文件
	fp = fopen("E:/test100.txt", "w+");
	//将内容写入到文件中
	fprintf(fp, "\n");
	fputs("\n", fp);
	//关闭文件!!!
	fclose(fp);
	printf("创建文件.写入信息完成!!!");
	getchar();
}


```

<span class="yellow">注意用  w+ / w 格式打开，且输入的语句没有，则会导致之后打开的文件，内容清空</span>

### 读取文件

从文件读取单个字符的函数

<span style="color:#6581ff;font-weight:bold">int fgetc(FILE *fp);</span>

从流中读取一个字符串

<span style="color:#6581ff;font-weight:bold">char *fgets(char *buf,int n,FILE *fp);</span>

		1) 说明:函数fgets()从fp所指向的输入流中读取n-1个字符。它会把读取的字符
		串复制到缓冲区buf，并在最后追加一个null字符来终止字符串。
		如果这个函数在读取最后一个字符之前就遇到一个换行符'\n'或文件的末尾EOF，则只会返回读取到的字符，包括换行符。

		2) 也可以使用 int fscanf(FILE *fp,const char *format,..) 函数来从文件中读取字符串，
		但是在遇到第一个空格字符时，它会停止读取。

```c
# include<stdio.h>

void main(){
	// 创建一个文件指针
	FILE *fp = NULL;
	// 定义一个缓冲区
	char buff[1024];

	//打开文件
	fp = fopen("d://test200.txt",'r');
	//方法1
	fscanf(fp,"%s",buff);
	//输出
	printf("%s\n",buff);

	//方法2
	读取整个文件
	 // 循环读取fp指向的文件内容，如果读取NULL ,就结束
	 while(fgets(buff,1024,fp)!=NULL){
		 printf("%s",buff);
	 }
	 //关闭文件

	 fclose(fp);
}
```

# 项目

## 家庭收支管理软件

模拟基于文本界面的<家庭收支记账软件>

**主要设计知识点**

	- 局部变量和基本数据类型
	- 循环语句
	- 分支语句
	- 简单的屏幕输出格式控制

### 需求说明

能记录家庭的收入，支出，并能打印出收支明细表

项目采用分级菜单的方式

		-------家庭收支记账软件-------
				1 收支明细
				2 登记收入
				3 登记支出
				4 退 出

				请选择(1~4):


**登记收入界面及操作**

			-------家庭收支记账软件-------
							1 收支明细
							2 登记收入
							3 登记支出
							4 退 出

							请选择(1~4):
				本次收入金额 : 1000
				本次收入说明 : 劳务费

**收支明细界面**

		-------家庭收支记账软件-------
						1 收支明细
						2 登记收入
						3 登记支出
						4 退 出

						请选择(1~4):
		--------当前收支明细记录--------
		收支	收支金额		账户金额		说明
		收入	1000	    11000	    劳务费
		支出	800		    10200	    物业费

		- 明细表格对齐，可以使用\t制表符

**退出界面**

		-------家庭收支记账软件-------
				1 收支明细
				2 登记收入
				3 登记支出
				4 退 出

				请选择(1~4):
		确认是否退出(y/n) : _

【基础版本】

```c

	# include<stdio.h>
	# include<string.h>
	void main() {
	/*
	完成1:显示菜单
	分析 
	 用do while 显示菜单 ，用户输入4表示退出
	*/
	/*
	 完成2 : 登记收入
	 分析
	 1. 需要字符串变量储存收入的数据
	*/
	/*
	完成4 :确认用户退出信息提示
	*/
	char key = ' '; //表示用户输入的菜单选项
	int loop = 1; //控制是否退出菜单 ,0表示退出
	char details[3000] = ""; //用于所有的存入收入数据
	char note[20] = ""; //  对收入/支出的说明
	char temp[100] = "";//用于临时储存
	double money = 0;
	char choice = ' '; //用于退出的选择选项
	int flag = 0;// 有至少一笔收入或支出
	double balance = 1000.0; //账户余额
	do {
		printf("\n--------------家庭收支记账软件----------");
		printf("\n-------------1. 收支明细----------");
		printf("\n-------------2. 登记收入----------");
		printf("\n-------------3. 登记支出----------");
		printf("\n-------------4. 退出----------");
		printf("\n 请选择(1~4):");
		scanf("%c", &key);
		getchar();//过滤回车
		switch (key) {
			case '1':
				if (flag) { //当有收入/支出时，才显示表头
				printf("\n收支\t收支金额\t账户金额\t说明%s",details); //显示收支明细
				}
				else {
				printf("当前没有任何的收支明细...来一笔吧！");
				}
				break;
			case '2':
				printf("\n请输入本次收入金额:");
				scanf("%lf", &money);
				getchar();//过滤回车
				balance += money;//更新余额
				printf("\n请输入本次收入说明:");
				scanf("%s", note);
				getchar();//过滤回车
				sprintf(temp, "\n收入\t%.2f\t\t%.2f\t\t%s", money, balance, note);
				//将信息拼接到details ,因为要显示多条收支信息
				strcat(details, temp);
				flag = 1;
				break;
			case '3':
				printf("请输入支出金额:");
				scanf("%lf", &money);
				getchar();
				printf("\n请输入本次支出说明:");
				scanf("%s", note);
				getchar();
				// 判断是否支出大于余额
				if (money<=balance) { //如果余额足
				balance -= money;
				}
				else{//如果余额不足
				printf("余额不足，请充值吧!");
				break;
				}
				sprintf(temp, "\n支出\t%.2f\t\t%.2f\t\t%s", money, balance, note);
				//将信息拼接到details ,因为要显示多条收支信息
				strcat(details, temp);
				flag = 1;
				break;
			case '4':
				printf("确定要退出吗?\n请输入y/n:");
					//这个do while循环主要判断输入的字符是否是 y / n
				do
					{
					scanf("%c", &choice);
					getchar();//过滤回车\n
					if (choice == 'y' || choice == 'n') { //choice用于储存确认的选项
					break;
					}
					printf("\n你的输入有误!\n请重新输入:");
				} while (loop); //当输入的是 y / n 是才退出循环
				// 这个if 是退出的判断
				if (choice == 'y') { //如果直接输入的是 y,则退出程序
				loop = 0;
				}
				printf("已经退出菜单!");
				break;
		default:
			printf("输入选项错误!");
			}
	} while (loop!=0); //当 loop ! = 0,即非退出状态时，执行循环
	getchar();
	}
	





```

**项目改进**

		1) 用户输入4退出时，提示 "你确定要退出吗?(y/n)" 必须输入正确的y/n,否则循环输入指令，直到正确的输入y/n
		2) 当没有任何收支明细的时候，提示"当前没有任何收支明细...来一笔吧!!!"
		3) 支出时，判断余额是否足够，并给出相应的提示
		4) 改进使用结构体完成


![](https://user-gold-cdn.xitu.io/2020/7/26/1738a3207e9f2a9c?w=904&h=430&f=png&s=136386)

使用函数和结构体改进

使用结构完成

使用函数提高项目的模块化


```c
#include<stdio.h>
#include<string.h>
// 定义结构体
struct MyFamilyAccount{
	int flag;//标志
	char details[100]  ; //明细
	double balance; //余额
};
//定义相关的局部变量
char key = ' '; //表示用户输入的菜单选项
int loop = 1; //控制是否退出菜单 ,0表示退出
char note[20] = ""; //  对收入/支出的说明
char temp[100] = "";//用于临时储存
double money = 0;
char choice = ' '; //用于退出的选择选项
//使用函数显示明细
void showDetails(struct MyFamilyAccount* myFamilyAccount) {
	if ((*myFamilyAccount).flag) { //当有收入/支出时，才显示表头
	printf("\n%s", (*myFamilyAccount).details); //显示收支明细
	}
	else {
	printf("当前没有任何的收支明细...来一笔吧！");
	}
}
//使用函数完成收入
	void income(struct MyFamilyAccount* myFamilyAccount) {
		printf("\n请输入本次收入金额:");
		scanf("%lf", &money);
		getchar();//过滤回车
		(*myFamilyAccount).balance += money;//更新余额
		printf("\n请输入本次收入说明:");
		scanf("%s", note);
		getchar();//过滤回车
		sprintf(temp, "\n收入\t%.2f\t\t%.2f\t\t%s", money, (*myFamilyAccount).balance, note);
		//将信息拼接到details ,因为要显示多条收支信息
		strcat((*myFamilyAccount).details, temp);
		(*myFamilyAccount).flag = 1;
}
//使用函数完成支出
void pay(struct MyFamilyAccount *myFamilyAccount){
	printf("请输入支出金额:");
	scanf("%lf", &money);
	getchar();
	// 判断是否支出大于余额
	if (money <= (*myFamilyAccount).balance) { //如果余额足
	(*myFamilyAccount).balance -= money;
	printf("\n请输入本次支出说明:");
	scanf("%s", note);
	getchar();
	sprintf(temp, "\n支出\t%.2f\t\t%.2f\t\t%s", money, (*myFamilyAccount).balance, note);
	//将信息拼接到details ,因为要显示多条收支信息
	strcat((*myFamilyAccount).details, temp);
	(*myFamilyAccount).flag = 1;
	}
	else {//如果余额不足
	printf("余额不足，请充值吧!");
	}
}
//退出程序功能
void myExit() {
	printf("确定要退出吗?(y/n):");
	//这个do while循环主要判断输入的字符是否是 y / n
	do{
		scanf("%c", &choice);
		getchar();//过滤回车\n
		//choice用于储存确认的选项
		if (choice == 'y' || choice == 'n') { //确认输入的是 y / n
		break; //表示首先输入的是 y / n正确选项
		}else {
		printf("\n你的输入有误!\n请重新输入:"); //否则输入非法字符
		}
	} while (1); //当输入的是 y / n 是才退出循环
	// 这个if 是退出的判断
	if (choice == 'y') {
	loop = 0; //表示退出
	}
	printf("已经退出菜单!");
}
//包装菜单的显示
// 专门显示菜单
/*
1. 输入 ；结构体 * myFamilyAccount
*/
void mainMenu(struct MyFamilyAccount * myFamilyAccount) {
	do {
	printf("\n--------------家庭收支记账软件----------");
	printf("\n\t\t\t1. 收支明细");
	printf("\n\t\t\t2. 登记收入");
	printf("\n\t\t\t3. 登记支出");
	printf("\n\t\t\t4. 退出");
	printf("\n请选择(1~4):");
	scanf("%c", &key);
	getchar();//过滤回车
	switch (key) {
		case '1':
			showDetails(myFamilyAccount);
			break;
		case '2':
			income(myFamilyAccount);
			break;
		case '3':
			pay(myFamilyAccount);
			break;
		case '4':
			myExit();
			break;
			default:
			printf("输入选项错误!");
			}
	} while (loop != 0); //当 loop ! = 0,即非退出状态时，执行循环
}
void main() {
	//定义结构体变量
	struct MyFamilyAccount  myFamilyAccount; 
	//初始化
	myFamilyAccount.flag = 0;
	myFamilyAccount.balance = 1000.0;
	//因为details是字符型常量，不能改变值，所以使用拷贝的方式赋值
	memset(myFamilyAccount.details, 3000, 0);//meset  清0
	strcpy(myFamilyAccount.details, "\n收支\t收支金额\t账户金额\t说明"); //拷贝details
	//调用mainMenu显示菜单
	mainMenu(&myFamilyAccount); //调用菜单
	getchar();
}


```

## 客户信息管理系统

### 结构体

**菜单**

功能说明

		用户打开软件，可以看到主菜单，输入5退出软件

思路

		在customerManagment.c文件中，编写函数mainMenu，显示菜单的函数，在main函数用调用

代码实现

```c
# include<stdio.h>
#include<string.h>
//定义结构体
struct Customer {
	int id;
	int age;
	char gender[10];
	char name[10];
	char phone[16];
	char email[20];
};
//输出显示某个customer客户的信息
//void getCustomerInfo(struct Customer *customer)
//{
//printf("\n%d\t%s\t%c\t%d\t%s\t%s", *(customer).id, *(customer).name, *(customer).gender, * (customer).age, * (customer).phone, *(Customer).email);
//};
//
//定义全局变量
int loop = 1; //是否控制主菜单的变量
int  key = 0 ; //储存菜单输入的选项
//显示主菜单
void mainMenu() {
	do{
		printf("\n--------------客户信息管理软件-------------");
		printf("\n               1.添加用户");
		printf("\n               2.修改用户");
		printf("\n               3.删除用户");
		printf("\n               4.显示客户列表");
		printf("\n               5.退出菜单");
		printf("\n 请输入1~5:");
		scanf("%d", &key);
		getchar();//过滤\n
		switch (key) {
			case 1:
				printf("添加用户：");
				break;
			case 2:
				printf("修改用户：");
				break;
			case 3:
				printf("删除用户：");
				break;
			case 4:
				printf("显示客户列表：");
				break;
			case 5:
				loop = 0; //表示退出
				break;
			default:
				printf("\n您输入有误，请重新输入:");
		}
	} while (loop);
	printf("你已经退出主菜单!");
}
//项目功能实现
/*
思路分析:
*/
// 主函数
void main() {
//struct Customer customer1;
mainMenu();//调用菜单函数
getchar();
}

```

### 显示用户信息

		------------------客户表---------------
		编号    姓名    性别   年龄    电话    邮箱
		1       张三    男     30      010-56253823    abc@mail.com
		2       李四    女     30      010-56253823    lisi@mail,com
		3       王芳    女     26      010-56253825    wagnfang@mial.com


		> 思路:
			1) 因为需要将多个客户保存，所以需要使用结构体数组Customer
			2) 编写 函数 listCustomer 显示客户信息
			3) 主菜单调用 listCustomer函数


```c
# include<stdio.h>
#include<string.h>
//定义结构体
struct Customer {
	int id;
	int age;
	char gender[10];
	char name[10];
	char phone[16];
	char email[20];
};
//变量的定义
//定义全局变量(全局变量不用赋初值，自动赋值)
int loop = 1; //是否控制主菜单的变量
int  key = 0; //储存菜单输入的选项
int customerNum = 1; //表示当前有多少个客户
//客户结构体数组
struct Customer customers[100]; //最多100个客户
//输出显示某个customer客户的信息
void getCustomerInfo(struct Customer *customer) // customer应该是一个结构体指针变量
{
	printf("\n%d\t%s\t%s\t%d\t%s\t%s", (*customer).id,(*customer).name, (*customer).gender, (*customer).age, (*customer).phone, (*customer).email);
};
//显示客户信息列表
void listCustomers() {
	int i = 0;
	printf("\n------------------客户列表---------------");
	printf("\n编号\t姓名\t性别\t年龄\t电话\t邮箱");
	for (i = 0; i < customerNum;i++) {
	getCustomerInfo(&customers[i]);
	}
}
//显示主菜单
void mainMenu() {
	do{
		printf("\n--------------客户信息管理软件-------------");
		printf("\n               1.添加用户");
		printf("\n               2.修改用户");
		printf("\n               3.删除用户");
		printf("\n               4.显示客户列表");
		printf("\n               5.退出菜单");
		printf("\n 请输入1~5:");
		scanf("%d", &key);
		getchar();//过滤\n
		switch (key) {
			case 1:
			printf("添加用户：");
			break;
		case 2:
			printf("修改用户：");
			break;
		case 3:
			printf("删除用户：");
			break;
		case 4:
			printf("显示客户列表：");
			listCustomers();
			break;
		case 5:
			loop = 0; //表示退出
			break;
			default:
			printf("\n您输入有误，请重新输入:");
			}
} while (loop);
printf("你已经退出主菜单!");
}
//项目功能实现
/*
思路分析:
*/
// 主函数
void main() {
	//测试，添加客户信息,初始化一个客户 (人为的添加)
	customers[0].id = 1;
	customers[0].age = 10;
	//字符串要拷贝进去,因为是常量
	strcpy(customers[0].gender, "男");
	strcpy(customers[0].email, "1954924950@qq.com");
	strcpy(customers[0].name, "张三");
	strcpy(customers[0].phone, "110");
	{
};
mainMenu();//调用菜单函数
getchar();
}

```


### 添加用户信息

		--------添加客户----------
		姓名:张三
		性别:男
		年龄:30
		电话:010-56253825
		邮箱:zhagnsan@abc.com
		------添加完成------------




```c
# include<stdio.h>
#include<string.h>
//定义结构体
struct Customer {
	int id;
	int age;
	char gender[10];
	char name[10];
	char phone[16];
	char email[20];
};
//变量的定义
//定义全局变量(全局变量不用赋初值，自动赋值)
int loop = 1; //是否控制主菜单的变量
int  key = 0; //储存菜单输入的选项
int customerNum ; //表示统计当前有多少个客户
//客户结构体数组
struct Customer customers[100]; //储存客户的数组，最多100个客户
//添加用户
void addCustomer() {
	customers[customerNum].id = customerNum + 1;//按照编号自增规则 ,这个地方的 + 1是进入这个数组的范围
	//添加用户实际上就是添加Customer结构体，也就是添加 id
	printf("--------------添加用户--------------");
	printf("\n姓名 :");
	scanf("%s",customers[customerNum].name);
	getchar();//过滤\n
	printf("\n性别 :");
	scanf("%s", customers[customerNum].gender);
	getchar();//过滤\n
	printf("\n年龄 :");
	scanf("%d", &customers[customerNum].age);
	getchar();//过滤\n
	printf("\n电话 :");
	scanf("%s", customers[customerNum].phone);
	getchar();//过滤\n
	printf("\n邮箱 :");
	scanf("%s", customers[customerNum].email);
	printf("--------------添加用户--------------");
	customerNum++;//表示用户数+1
}
//输出显示某个customer客户的信息
void getCustomerInfo(struct Customer *customer) // customer应该是一个结构体指针变量
{
	printf("\n%d\t%s\t%s\t%d\t%s\t%s", (*customer).id,(*customer).name, (*customer).gender, (*customer).age, (*customer).phone, (*customer).email);
};
//显示客户信息列表
void listCustomers() {
	int i = 0;
	printf("\n------------------客户列表---------------");
	printf("\n编号\t姓名\t性别\t年龄\t电话\t邮箱");
	for (i = 0; i < customerNum;i++) {
	getCustomerInfo(&customers[i]);
}
}
//显示主菜单
void mainMenu() {
	do{
		printf("\n--------------客户信息管理软件-------------");
		printf("\n               1.添加用户");
		printf("\n               2.修改用户");
		printf("\n               3.删除用户");
		printf("\n               4.显示客户列表");
		printf("\n               5.退出菜单");
		printf("\n 请输入1~5:");
		scanf("%d", &key);
		getchar();//过滤\n
		switch (key) {
			case 1:
				printf("添加用户：");
				addCustomer();
				break;
			case 2:
				printf("修改用户：");
				break;
			case 3:
				printf("删除用户：");
				break;
			case 4:
				printf("显示客户列表：");
				listCustomers();
				break;
			case 5:
				loop = 0; //表示退出
				break;
				default:
				printf("\n您输入有误，请重新输入:");
		}
} while (loop);
printf("你已经退出主菜单!");
}
//项目功能实现
/*
思路分析:
*/
// 主函数
void main() {
	//测试，添加客户信息,初始化一个客户 (人为的添加)
	//customers[0].id = 1;
	//customers[0].age = 10;
	////字符串要拷贝进去,因为是常量
	//strcpy(customers[0].gender, "男");
	//strcpy(customers[0].email, "1954924950@qq.com");
	//strcpy(customers[0].name, "张三");
	//strcpy(customers[0].phone, "110");
	mainMenu();//调用菜单函数
	getchar();
}

```

### 删除用户信息

		-----------删除客户-----------
		请选择待删除的客户编号(-1退出) :1
		确认是否删除(y/n): y
		----------删除完成-----------



<span class="yellow">需要考虑用户输入用户的编号的合理性</span >

```c
#include<stdio.h>
#include<string.h>
//定义结构体
struct Customer {
    int id;
    int age;
    char gender[10];
    char name[10];
    char phone[16];
    char email[20];
};
//变量的定义
//定义全局变量(全局变量不用赋初值，自动赋值)
int loop = 1; //是否控制主菜单的变量
int  key = 0; //储存菜单输入的选项
int customerNum; //表示统计当前有多少个客户
int flag = 0; //0表示没用进行删除/修改/添加操作，即没有用户
char choice; //用于接收退出的选择(y/n)
//客户结构体数组
struct Customer customers[100]; //储存客户的数组，最多100个客户
//添加用户
void addCustomer() {
    customers[customerNum].id = customerNum + 1;//按照编号自增规则 ,这个地方的 + 1是进入这个数组的范围
    //添加用户实际上就是添加Customer结构体，也就是添加 id
    printf("--------------添加用户--------------");
    printf("\n姓名 :");
    scanf("%s", customers[customerNum].name);
    getchar();//过滤\n
    printf("\n性别 :");
    scanf("%s", customers[customerNum].gender);
    getchar();//过滤\n
    printf("\n年龄 :");
    scanf("%d", &customers[customerNum].age);
    getchar();//过滤\n
    printf("\n电话 :");
    scanf("%s", customers[customerNum].phone);
    getchar();//过滤\n
    printf("\n邮箱 :");
    scanf("%s", customers[customerNum].email);
    printf("--------------添加用户--------------");
    customerNum++;//表示用户数+1
}
//主功能函数
//输出显示某个customer客户的信息
void getCustomerInfo(struct Customer* customer) // customer应该是一个结构体指针变量
{
    printf("\n%d\t%s\t%s\t%d\t%s\t%s", (*customer).id, (*customer).name, (*customer).gender, (*customer).age, (*customer).phone, (*customer).email);
};
//显示客户信息列表
void listCustomers() {
    int i = 0;
    printf("\n------------------客户列表---------------");
    printf("\n编号\t姓名\t性别\t年龄\t电话\t邮箱");
    for (i = 0; i < customerNum; i++) {
        getCustomerInfo(&customers[i]);
    }
    printf("\n------------------客户列表完成---------------");

}
//判断用户要删除的用户的编码是否存在
// 用户的id 和实际的内存中的Index索引不是一一对应的,id可以变化，index的顺序是不变的
// 所以通过用户的index判断是否可以被删除
/*
1. 先输入用户的id
2. 获得对应的index索引
3. 判断这个索引的值id是否存在
 有 : 返回index
 没有: 返回-1
*/
int findIndex(int id) { //用户输入的删除的id
    int index = -1;
    int i;
    for (i = 0; i < customerNum; i++) {
        if (customers[i].id == id) { //如果找到
        // 
            index = i;
        }
    }
    return index; //index 为 ! 1表示存在
}
//删除
/*
1. 返回int ，1:说明删除成功
2. 0 :说明删除失败
3. 接收的是要删除的客户的id
*/
int  delCustomer(int id) {
    int index = findIndex(id); //先确定id对应的用户是否存在
    int i;
    if (index == -1) { //则不存在
        return 0; //表示失败
    }
    else {
        //说明存在,删除就是把用户的id移除
        //1.从Customer数组 index+1 这个位置 开始，整体前移
        for (i = index + 1; i < customerNum; i++) {
            customers[i - 1] = customers[i];
        }
        //2. customerNum -1
        customerNum--;
        return 1; //表示删除成功
    }
}
//显示删除用户的界面
void delView() {
    int index;
    int delId = 0; //这个id是用户要删除的用户的id,就是一个数值而已
    char choice = ' ';
    printf("\n---------删除用户---------");
    printf("\n请输入待删除的用户的id(-1  退出):");
    scanf("%d", &delId);
    getchar();
    index = findIndex(delId);
    if (delId == -1) { //用户id正常是 >0的
        printf("\n----------你放弃删除了!----------");
        return;
    }
    printf("\n是否确认删除?(y/n):");
    scanf("%c", &choice);
    getchar();
    if (choice == 'y') {
        if (!delCustomer(delId)) { //表示删除失败。id不存在
            printf("\n-----------你要删除的用户id不存在!!!-----------");
            return;
        }
        else {
            printf("\n你已成功删除用户 %s !!!", customers[index].name);
        }
    }
}
//显示主菜单
void mainMenu() {
    do {
        printf("\n--------------客户信息管理软件-------------");
        printf("\n               1.添加用户");
        printf("\n               2.修改用户");
        printf("\n               3.删除用户");
        printf("\n               4.显示客户列表");
        printf("\n               5.退出菜单");
        printf("\n 请输入1~5:");
        scanf("%d", &key);
        getchar();//过滤\n
        switch (key) {
        case 1:
            addCustomer();
            flag = 1;
            break;
        case 2:
            break;
        case 3:
            delView();
            flag = 1;
            break;
        case 4:
            listCustomers();
            break;
        case 5:
            do
            {
                printf("\n确认是否退出(y/n)");
                scanf("%c", &choice);
                getchar();
            } while (choice != 'y' && choice != 'n'); //如果输入的不是并且不是n,则输入错误
            //也就是只能输入y / n，其它的就是非法
            if (choice == 'y') {
                loop = 0; //表示退出
            }
            break;
        default:
            printf("\n您输入有误，请重新输入:");
        }
    } while (1);
    printf("你已经退出主菜单!");
}
//项目功能实现
/*
思路分析:
*/
// 主函数
void main() {
    //测试，添加客户信息,初始化一个客户 (人为的添加)
     //customers[0].id = 1;
     //customers[0].age = 10;
     ////字符串要拷贝进去,因为是常量
     //strcpy(customers[0].gender, "男");
     //strcpy(customers[0].email, "1954924950@qq.com");
     //strcpy(customers[0].name, "张三");
     //strcpy(customers[0].phone, "110");
    mainMenu();//调用菜单函数
    getchar();
}

```

### 修改用户信息(待编写)

