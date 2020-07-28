---
tags: 
    - c

categories:
    - 笔记 
    - c
date: 2020-3-2 15:38:00
toc: true
cover: https://img-blog.csdnimg.cn/20200309214024258.png
---

# 指针
## 什么是指针
### 指针的声明
```java
type  *var-name 
```
例如
```java
int *ip  //整型
double *dp  //double类型
float *fp  // 浮点型
char *ch  // 字符型
```
不同数据类型之间的唯一差异是 ： 
<!-- more -->
指针指向的`变量 `或 `常量`的数据类型不同

**注意：**

在声明或者定义指针变量的时候
比如
```
int  * p ；
 //仅仅是代表定义一个变量p，类型是 int * （int表示是一个整型变量，*而且是一个仅仅能放地址的变量）
 
一个变量的存放的内容是一个地址，不就是一个指针变量嘛
上述仅仅是定义一个变量而已
 没有对这个变量操作
 
```
### 指针的使用
指针变量也是一个变量，这个变量存放的值是另一个变量的地址，就如同一般的变量或者常量一样

在使用指针之前需要特别说明这个变量是指针变量，毕竟很特殊相对于一般的变量

使用指针会频繁经历这样的几个过程，1定义一个指针变量，2把变量的地址给这个指针变量，3访问这个指针变量指向的变量的值，4通过`一元运算符 *`，来返回指定地址的值

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228180330703.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
**注意：**  `*ip`就是代表`ip`存放的地址指向的变量

这里ip指向的变量是var 

即   *ip <<==>>var

所以  
```
printf("ip指向变量的值是d%"，*ip) ;
等价于
printf("ip指向变量的值是d%"，var) ;
```

### c中的空指针null
在变量声明的时候，如果没有明确的地址，那么可以赋一个null，为指针赋值null只一个良好的编程习惯

空指针null,是一个定义在标准库中值为0的常量，请看下列的程序

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228180631279.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

在大多数的操作系统上，程序不允许访问地址为 0 的内存，因为该内存是操作系统保留的。然而，内存地址 0 有特别重要的意义，它表明该指针不指向一个可访问的内存位置。但按照惯例，如果指针包含空值（零值），则假定它不指向任何东西。
如需检查一个空指针，您可以使用 if 语句，如下所示：
```java
if(ptr)     /* 如果 p 非空，则完成 */
if(!ptr)    /* 如果 p 为空，则完成 */

```
**注意：** 

>当在`定义`指针p的时候，*p  应该当作一个整体，就是一个变量而已，只不过这个变量是指针变量


## 指针的算数运算
c指针指的是一个用数值表示的地址，而c指针变量指的是一个一个值为地址的变量
这种运算其实是一种位置的移动
往前移动
往后移动

因此可以对c指针进行数值运算 ++ ,--, +， -
假设 **ptr**是一个指向地址 1000的整型指针，是一个32位的整数
进行下列运算
```java
ptr++
```
表示ptr往前移动了一次，每一次是4个子节，所以就是
结果是运行完后，ptr指向1004

因为ptr每运行一次，都指向下一个整数的位置，32位即4个子节
这个运算不会影响到内存中实际值

如果ptr指向的是一个1000的字符，一个字符类型占 1个子节，所以下个字符指向的字符位置是 1001

示例图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302152437508.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
## 指针的数组
在我们讲解指针数组的概念之前，先让我们来看一个实例，它用到了一个由 3 个整数组成的数组：
```java
实例
#include <stdio.h>
 
const int MAX = 3;
 
int main ()
{
   int  var[] = {10, 100, 200};
   int i;
 
   for (i = 0; i < MAX; i++)
   {
      printf("Value of var[%d] = %d\n", i, var[i] );
   }
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```java
Value of var[0] = 10
Value of var[1] = 100
Value of var[2] = 200
```
可能有一种情况，我们想要让数组存储指向 int 或 char 或其他数据类型的指针。下面是一个指向整数的指针数组的声明：
```java
int *ptr[MAX];
```
在这里，把 ptr 声明为一个数组，由 MAX 个整数指针组成。因此，ptr 中的每个元素，都是一个指向 int 值的指针。下面的实例用到了三个整数，它们将存储在一个指针数组中，如下所示：
```java
实例
#include <stdio.h>
 
const int MAX = 3;
 
int main ()
{
   int  var[] = {10, 100, 200};
   int i, *ptr[MAX];
 
   for ( i = 0; i < MAX; i++)
   {
      ptr[i] = &var[i]; /* 赋值为整数的地址 */
   }
   for ( i = 0; i < MAX; i++)
   {
      printf("Value of var[%d] = %d\n", i, *ptr[i] );
   }
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```java
Value of var[0] = 10
Value of var[1] = 100
Value of var[2] = 200
```
您也可以用一个指向字符的指针数组来存储一个字符串列表，如下：
实例
```java
#include <stdio.h>
 
const int MAX = 4;
 
int main ()
{
   const char *names[] = {
                   "Zara Ali",
                   "Hina Ali",
                   "Nuha Ali",
                   "Sara Ali",
   };
   int i = 0;
 
   for ( i = 0; i < MAX; i++)
   {
      printf("Value of names[%d] = %s\n", i, names[i] );
   }
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```java
Value of names[0] = Zara Ali
Value of names[1] = Hina Ali
Value of names[2] = Nuha Ali
Value of names[3] = Sara Ali
```
## 指向指针的指针
指向指针的指针是一种多级间接寻址的形式，或者说是一个指针链。通常，一个指针包含一个变量的地址。当我们定义一个指向指针的指针时，第一个指针包含了第二个指针的地址，第二个指针指向包含实际值的位置。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302152821424.png)

一个指向指针的指针变量必须如下声明，即在变量名前放置两个星号。例如，下面声明了一个指向 int 类型指针的指针：
int **var;
当一个目标值被一个指针间接指向到另一个指针时，访问这个值需要使用两个星号运算符，如下面实例所示：
```java
#include <stdio.h>
 
int main ()
{
   int  var;
   int  *ptr;
   int  **pptr;

   var = 3000;

   /* 获取 var 的地址 */
   ptr = &var;

   /* 使用运算符 & 获取 ptr 的地址 */
   pptr = &ptr;

   /* 使用 pptr 获取值 */
   printf("Value of var = %d\n", var );
   printf("Value available at *ptr = %d\n", *ptr );
   printf("Value available at **pptr = %d\n", **pptr);

   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```java
Value of var = 3000
Value available at *ptr = 3000
Value available at **pptr = 3000
```
## 相关链接
- ----
[指针的算数运算](https://www.runoob.com/cprogramming/c-pointer-arithmetic.html
)
[指针的数组](https://www.runoob.com/cprogramming/c-array-of-pointers.html
)
[指向指针的指针](https://www.runoob.com/cprogramming/c-array-of-pointers.html
)
[传递指针给函数](https://www.runoob.com/cprogramming/c-passing-pointers-to-functions.html
)
[从函数返回指针](https://www.runoob.com/cprogramming/c-return-pointer-from-functions.html
)
