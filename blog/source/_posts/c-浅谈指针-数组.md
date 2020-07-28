---
title: 浅谈指针/数组/malloc()函数
tags:
  - c
categories:
  - 笔记
  - c
toc: true
date: 2020-03-20 16:54:38
top:
cover: https://img-blog.csdnimg.cn/20200320190622563.png
---

# 指针与数组
程序  = 数据的储存 + 数据与数据之间的操作 + 可以被执行的编程语言


指针是重点

   **定义** : 指针是地址---内存单元的编号
从0 开始的非负整数
范围 0 ---FFFFFFFF

也等于 F [0~4G-1]

计算机内部cpu和内存的原理


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320174503176.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)


指针就是地址，地址就是指针

指针 ! = 指针变量

指针变量，专门存放内存地址的变量

指针的本质式一个操作受限的非负数整数

分类

区别   int *p 和 *p 
```c
int *p 表示 int * 类型的变量p

*P表示指针变量p指向的那个变量
比如p指向的那个变量是 I ，那么* p = i
```

数组  int a[5]

对于数组地址

记住这个公式

`*(a+ i) = a[i]`

简单证明？

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320175643696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

关键点就在于，规定了`数组名`代表数组首地址

所以只要知道首地址，就可以获取所有的位置的元素

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320180243208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

关键在于找到`地址`，而这个地址所在的`变量名`就是地址所在位置的一个别称/简称，所以从某种意义上`变量名`就是这个变量所在地址的内容


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320180807159.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

P此时存放的是一个未知的地址，但这个地址未必存放一个具体可观的值，
有可能没有值，是一个垃圾数字

```c
int * p  // 表示p 是一个int * 类型的变量,只能存储int 类型的指针
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320182219865.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032018245199.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320182523708.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)


```c
# include <stdio.h>

int main(void) {

  double arry[3] = {1,2,1,3,1,4};
  double *p ;
  p = &arry[0];
  printf("%p\n",p);
  p = &arry[1];
  printf("%p\n",p);
  return 0;
}

// print results
0060FEE0
0060FEE8
```

*数组的1,2,3..代表位置，如果1代表当前位置，那么2代表下一个位置，每一个位置是`1Byte`,所以下一个地址是隔了8个子节，同理3代表`下下`个位置，那么隔了`16`个子节*

无论指针指向的变量占几个字节
指针变量都只占4个子节

结构体

https://www.bilibili.com/video/av6159200?p=10




# 引用型

当函数没有返回值的时候如何通过函数修改实参的值

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320181540549.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

通过找到该**变量的指针地址**来修改实参的值

结构体
动态内存分配


**模块一**

线性结构

连续储存[  数组  ]

离散储存[  链表  ]


# malloc()函数

https://www.bilibili.com/video/av6159200?p=9

```c
sizeof(  int )  
```

malloc( )函数是向系统申请空闲空间的使用权

sizeof 是你想使用怎么样的空间类型 int 型4个字节?

变量是什么类型 指针类型的* 

几个 ？  len 表示申请的个数，如果是5个的话，就是20个子节

这也是为什么可以扩展分配空间的原因

```c
int * pArr = (int * ) malloc(sizeof(  int ) * len )

sizeof(  int )   //表示向系统申请空间

mallco() 只返回第一个子节地址


```

但是这只是单纯的20个子节，没有表现出是什么数据类型

( int  * ) 表示malloc() 返回地址是int 类型还是double类型

malloc() 函数默认返回   void *   类型，是无意义的

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320185849307.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)