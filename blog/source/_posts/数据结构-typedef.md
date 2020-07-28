---
title: 4>typedef的使用
tags:
  - 数据结构
categories:
  - 笔记
  - 数据结构
toc: true
date: 2020-03-14 14:49:01
top:
cover: https://img-blog.csdnimg.cn/20200309212551381.png
---


# typedef的作用

typedef简单来说，就是可以替换变量的数据类型的一个方法?

比如 

```c
//使用一般方法定义一个变量
int age ;

//使用typedef后
typedef int good;

//那么int 类型的数据的定义可以变成
good age ; 
//也就是good age 等价于 int age 

```

使用typedef的关键就在于，原来的数据类型名比较长，才需要可以等价的使用另一个比较短的名字

所以typedef常见于结构体数据类型的使用过程中

# typedef的用法

**`typedef`第一种常规用法**

```c
# include <stdio.h>

struct Student {

  int age ;
  char name;
};

int main(void){
   
  struct Student st ;
  //如果还是像之前一样使用Struct student会显得很麻烦
  //能不能一个单词解决呢 
  st.age = 20 ;
  printf(" %d ", st.age) ;
  return 0;
}

// print results
 20
Process returned 0 (0x0)   execution time : 0.922 s
Press any key to continue.

```
>使用typedef之后的效果

```c
# include <stdio.h>
//使用typedef自定义数据类型
typedef struct Student good;
//表示struct Studen 等价于 good 
struct Student {

  int age ;
  char  name;
};

int main(void){

  good st ; //注意此时数据类型换成了 good
//good st 等价于 struct Student st 
  st.age = 20 ;
  printf("%d ", st.age) ;

  return 0;
}

// print results 
20
Process returned 0 (0x0)   execution time : 0.288 s
Press any key to continue.

```

经过测试 struct Student 换成 good 也可以成功编译运行

>当然使用typedef换`普通`的数据类型，像int char ...也可以

**`typedef` 第二种用法**


使用在结构体语句中

```c
# include <stdio.h>

typedef struct Student {

  int age ;
  char name;
}ST ; 
//可以在结构体定义过程中顺便定义一个别名 ST 
//ST 等价于 struct Student

int main(void){
   
  ST st ;
  st.age = 20 ;
  printf(" %d ", st.age) ;
  return 0;
}

// print results
 20
Process returned 0 (0x0)   execution time : 2.894 s
Press any key to continue.
//一样正常输出
```
**`typedef` 第三种用法**

也可以为结构体指针数据类型指定别名

```c
# include <stdio.h>

typedef struct Student {

  int age ;
  char name;
}* PST ;  // PST等价于struct Student *

```

```c
# include <stdio.h>

typedef struct Student {

  int age ;
  char name;
}* PST ,STU ;  // PST等价于struct Student *
//STU等价于 struct Student

int main(void){

  PST ps ;
  STU st ;
  st.age = 18 ;
  ps = &st ;
  printf("%d\n",ps->age) ;
  printf("%d",st.age) ;
  return 0;
}


//print results
18
18
Process returned 0 (0x0)   execution time : 0.944 s
Press any key to continue.
```
>当然在使用typedef定义了一个数据类型的别名，同时还是可以使用原来的数据类型的名字

```c
PST ps  ; or struct Student ps 可以任选一个使用;
```

# typedef使用的注意事项


注意 `c语言`中变量命名不能是`大写`

要注意指针变量p和指向的变量st

如果是指针变量p ，赋值是 ： p->age

如果是 指向的变量st ，赋值是 :  st.age

