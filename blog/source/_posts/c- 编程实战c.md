---
title: (1)"编程实战-c语言+数据结构"
tags:
  - c
categories:
  - 笔记
  - c
toc: true
date: 2020-05-18 23:54:39
top:
cover:
---
# **c语言部分**
## 学生管理信息系统

###  对于涉及程序的分析

**学生管理系统**


写一个学生管理系统

**1.要干嘛?具备什么能力**

对于接下来的管理系统，需要具备那些能力

- c语言基础
- 会c语言结构体
- 数据结构的基础：链式结构
- 基本文件操作：打开，读写

### 基础认识
**首先我们先要了解程序是怎么运行的/执行?**

- 由上到下，依次执行
- 编译过程
- 运行过程

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519004516766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
	
**对于函数的声明使用**

`两种方式:`

1. 写`全部`的声明
如
	
  
```c
# include<stdio.h>
void print() //什么叫做空，就是功能在这个函数就完成了，不要返回值，在主函数或其他函数完成 
{
    printf("hello world!");
    
}


int main()
{
    print();

}
```
1. 写`部分`的声明
如
```c
# include<stdio.h>
void print(void );//什么叫做空，就是功能在这个函数就完成了，不要返回值，在主函数或其他函数完成
void print()
{
  
    printf("hello world!");   
}
int main()
{
    print();
}
```
	
>部分使用时:`类型 函数名(形参类型)`


>目的都是要程序先认识这个东西

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519005016659.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519005032303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)


"先死后活"

先设计死流程:就是至少要完成的操作
- 用户交互界面
- 按键交互界面
- 如何储存数据
-  …


还要具备分块思维:`就是分工要详细，各尽其职`，首先要知道干什么?

分块思维：
一部分控制一部分

### 开始基本的实现

①创建菜单，就是显示的菜单

    void systemMenu()
    {
        //遵循几个菜单就是几个函数
      printf("-----------[学生管理系统]------------\n");
      printf("\t\t0.退出系统\n");
        printf("\t\t1.插入信息\n");
        printf("\t\t2.浏览信息\n");
      printf("\t\t3.删除信息\n");
      printf("\t\t4.修改信息\n");
      printf("\t\t5.查找信息\n");
      printf("---------------------------------------");
      printf("请输入0~5:");
      //每一次交互，可以告诉别人程序是干嘛的
    }

>隐藏的文件:同步文件功能

>⭐注意在设计程序的过程中要考虑用户的交互效果，方便自己也方便他人

②交互:获取用户的输入信息

```c
void keyDown()
{
    int userkey;     //用户需要输入一个值
    struct student tempData; //定义一个student结构体别名，用于存储临时的信息
    scanf("%d",&userkey);//接收用户输入的值

    //根据用户输入的值，进入不同的功能
    switch (userkey)
    {
        case 0:
            printf("\t\t[退出系统]\n");
            //清屏操作，防止闪屏
            system("pause");
            system("cls");
            break;
        case 1:
            printf("\t\t[插入信息]\n");
            printf("请输入姓名，学号，年龄，电话，住址:");
            scanf("%s%s%d%s%s",tempData.name,tempData.num,&tempData.age,tempData.tel,tempData.addr);//单纯的随便的存入内存
            insertNodeByHead(list,tempData); //把先前的存入内存的数据进行操作，有结构化的，符合结构顺序，绑定
            saveInfoToFile("student.txt",list);
            break;
        case 2:
            printf("\t\t[浏览信息]\n");
            printList(list);
            break;
        case 3:
            printf("\t\t[删除信息]\n");
            printf("请输入要删除的学生姓名:");//以学生为关键字
            scanf("%s",tempData.name);
            deleteNodeByAppoinNum(list,tempData.name);
            saveInfoToFile("student.txt",list);
            break;
        case 4:
            printf("\t\t[修改信息]\n");
            printf("请输入要修改学生的学号:");
            scanf("%s",tempData.num);
            if(searchNodeByAppinNum(list,tempData.num) == NULL)
            {
                printf("未找到相关信息!\n");

            }
            else
            {   struct Node* curNode= searchNodeByAppinNum(list,tempData.num);

                printf("请输入新的姓名，学号，年龄，电话，地址");
                scanf("%s\t\t%s\t%d\t\t%s\t\%s\n",curNode->data.name,curNode->data.num,&curNode->data.age,
                      curNode->data.tel,curNode->data.addr);
               saveInfoToFile("student.txt",list);
            }
            break;
        case 5:
            printf("\t\t[查找信息]\n");
            printf("请输入查找的学号:");
            scanf("%s",tempData.num);
            if(searchNodeByAppinNum(list,tempData.num) == NULL)
            {
                printf("未找到相关信息!\n");

            }
            else
            {
                printNode(searchNodeByAppinNum(list,tempData.num));
            }
            break;

        default:      //优化一下，为意外情况做处理，为无法意料的问题做准备，可能用户输入数字不对
            printf("输入错误！，只能输入0~5之间的数字，请重新输入");
            break;

    }
}

```

    int main ()
    {
      while (1)
      {
      
        sytemMenu();
        keyDown();
        system("pause");
        system("cls");
        
      }
    }

>设置一下清屏

**⭐注意的地方**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519102532311.png)
## 简单的学生管理系统


一般出错 scanf_s是编译器的错误

>visual studio独有的问题，需要加上`宏定义`

` define _CRT_SECURE_NO_WARNING`

主要功能：
- **增**；插入学生信息
- **删**：删除学生信息
- **改**：修改学生信息
- **查**：查询学生信息

>采用自带的windows命令版的学生信息管理系统,`非图形界面`




### 数据结构部分详解
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519102956868.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

把结构体类型变量看成和普通变量一样就行

如 

    struct Node* student 

    变量类型      变量名

    如同
    int         a 

    变量类型     变量名

>只不过结构体数据类型的数据时`多维的`,不止一个

如 

`struct Node node1 = {1,NULL}` 

这里的 `{1,NULL}`就是其`多维`的数据

**链表，链表是什么?**

>结构体变量和结构体变量`连接`在一起的结构

指针的变成变量的方式，动态内存申请

**指针变为变量的两种方式**

1. 直接初始化
   
        struct Node* p1 = &node1;
        struct Node* p2 = &node2;

2. 使用`malloc()`动态内存申请
   
        struct NOde* p4 = (struct Node*)malloc(sizeof(struct Node));


>这两者都是向内存`申请空间`作为储存值的`变量`得过程

>这个把地址给一个变量名就是给`内存的某一个地址起一个名字`，方便操作


**如何表示链表?**

默认使用链表的第一个结点代表整个链表

>就如同学校的运动会，为了区分各个班级，会在每个班级的最前面`树立一个牌子`，用于`区分不同的班级`，这个牌子就相当于最前面的结点

有两种情况
1. 第一个结点不储存数据，就是用来标识链表:**`有表头`链表**
2. 第一个结点也储存数据，和其它的结点一样，但同样可以标识链表:

>所以创建链表，就是要用`第一个结点`表示整个链表，也就是创建一个`指针类型结构体变量`


#### **创建链表**

**主要操作**
1. 产生一个变量(结构体变量)
2. 初始化一个变量
```c
//1.链表是：结构体变量和结构体变量链接在一起
//2“指针第二种变为变量的方式--->动态内存申请
//3.用第一个结点表示链表
struct Node* createList()   //创建链表要有东西啊，这个东西就是data数据
{
    //有表头链表
    //无表头链表
    //先要创建结构体
    //1.申请一个变量
    struct Node* ListHeadNode = (struct Node*)malloc(sizeof(struct Node));
    //2.初始化一个结构体变量
    //newNode->data = data; //只有第一个节点，数据
    ListHeadNode->next = NULL;//没有下一个节点
    return ListHeadNode;
};
```
>相当于一个整体的定义链表

#### **创建结点**

```c
struct Node* createNode(struct student data)
{
    //1.产生结构体变量
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    //2. 初始化一个变量
    newNode->data = data; //第二个data表示传入的数据
    newNode->next = NULL;
    return newNode;
};
```
>创建结点实际是向结构体变量添加值

#### **插入数据**

```c
//插入节点,系统录入信息
void insertNodeByHead(struct Node* ListHeadNode,struct student data)   //因为插入的是一个结构体的节点，所以类型是结构体类型
{
    //如何插入
    struct Node* newNode = createNode(data); //直接调用之前创建好的函数
    //先要判断是否为空
    //做链接操作
    newNode->next = ListHeadNode->next;
    ListHeadNode->next = newNode;
    //房子                  地址
    //关键是要表现出指向关系

}
```
**插入关键算法**

    newNode->next = ListHeadNode->next;
    ListHeadNode->next = newNode;

#### **打印链表**

```c
//打印链表
//1.输入:打印那个链表，指定链表也就是头节点
void printList(struct Node* ListHeadNode) //打印列表
{
    struct Node* pMove  = ListHeadNode->next; //第一个节点不放数据，需要移动到第二个节点
    printf("姓名\t\t学号\t\t年龄\t\t电话\t\t住址\n");
    while(pMove) //先要判断是否为空,不为空，则打印
    {
        printf("%s\t\t%s\t%d\t\t%s\t\%s\n",pMove->data.name,pMove->data.num,pMove->data.age,pMove->data.tel,pMove->data.addr);
        pMove = pMove->next;//表示pMove指向原来的下一个地方，就是往下走一个
        //怎样实现一直打印，使用while循环
    }
    printf("\n");

}
```
**要从第二个结点开始打印**
- 要判断有无数据
- 有，则进行打印操作

#### **删除数据**

```c
//删除
//1.输入:
//       ①要删除那个结点:指明头节点
//       ②要删除的值:根据什么删除，指定一个关键字
void deleteNodeByAppoinNum(struct Node* ListHeadNode, char *name)   //只能用数字找位置
{
    struct Node* posFrontNode = ListHeadNode;//表示从表头开始遍历
    struct Node* posNode = ListHeadNode->next;//posNode表示当前删除的位置,posrontNode表示当前位置的前一个
    if (posNode == NULL)
    {
        printf("无相关内容，无法删除！");
        return ;
    }
    else{
         while(strcmp(posNode->data.name,name))
        {   //主要的删除代码操作
            posFrontNode = posNode;
            posNode = posFrontNode->next;
            if(posNode == NULL)
            {
                 printf("无相关内容，无法删除！");
                 return ;
            }
        }
        posFrontNode->next = posNode->next;
        free(posNode);//释放删除的元素
    }

}
```
**输入:**
- 指定储存数据的链表
- 删除位置，无法直接获取要删除元素的位置信息，只能传递数值

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519110526812.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

**字符串比较strcmp**

遍历到的元素和要删除的元素关键字对比，因为是字符串所以用strcmp

如果是普通的数值，就直接比大小了


#### **查找**

```c
//1/输入:
//      查找的链表;指定头节点
//       查找的值
struct Node* searchNodeByAppinNum(struct Node* listHeadNode,char *num)
{
    struct Node* pMove = listHeadNode->next;
    if (pMove == NULL)
        return pMove;
    else
    {

        while (strcmp(pMove->data.num,num)) //把遍历的数值和要查找的数值对应，就找到了
        {

            pMove = pMove->next;
            if (pMove == NULL) //防错机制
                break;
        }
        return pMove; //返回这个位置的数值，也就是返回这个结点
    }

};
```



>这个数据，实际上是学生的数据，实例化了，所以当使用了学生的数据，就需要把之前抽象的数据改为学生的数据data

#### 插入

```c
//插入节点,系统录入信息
//1. 输入
//      要插入的链表:指定头节点
//      要插入的数据，指定数据
void insertNodeByHead(struct Node* ListHeadNode,struct student data)   //因为插入的是一个结构体的节点，所以类型是结构体类型
{
    //如何插入
    struct Node* newNode = createNode(data); //直接调用之前创建好的函数
    //先要判断是否为空
    //做链接操作
    newNode->next = ListHeadNode->next;
    ListHeadNode->next = newNode;
    //房子                  地址
    //关键是要表现出指向关系



}
```

#### 简单的文件操作

**读取文件**
>程序从外部读取文件数据到本程序中使用

1. 读取文件
2. 若文件不存在，创建并打开文件
3. 创建一结构体用于存储数据
4. 读数据(以文件的方式)


  

```c
//读
void readInfoFromFile(char *filename ,struct Node* listHeadNode)
{
    FILE *fp = fopen(filename,"r");//以读的方式打开指定文件
    //假如为文件为空/没有，则创建一个这个文件名的文件，以读的方式打开
    if (fp == NULL)
    {
        fp = fopen(filename,"w");
    }
    struct student tempData;
    //相当于从内存中读出数据
    while(fscanf(fp,"%s\t%s\t%d\t%s\t%s\n",tempData.name,tempData.num,&tempData.age,
                 tempData.tel,tempData.addr) !=EOF)
    {
      //把数据插入到程序需要的储存链表结构中
        insertNodeByHead(listHeadNode,tempData); //插入数据
        memset(&tempData,0,sizeof(tempData))     ;

    }
    fclose(fp); //表示将这文件关闭，保存的意思

}
```

主要操作

    FILE *文件对象 = fopen(文件名,"操作类型");//打开

    fclose() //关闭保存

#### 写入文件
```c
void saveInfoToFile(char *filename,struct Node* listHeadNode)
{
    FILE *fp = fopen(filename,"w"); // 以写的方式打开，表示存入，写入数据
    struct Node* pMove = listHeadNode->next;
    while(pMove)
    {
        fprintf(fp,"%s\t%s\t%d\t%s\t%s\n",pMove->data.name,pMove->data.num,pMove->data.age,
                 pMove->data.tel,pMove->data.addr);//相当于打印到文件

        pMove = pMove->next; //相当于循环的更新条件
    }
    fclose(fp);
}
```

>写/读无非就是文件操作的模式不同

#### 定义结构体

```c
//定义学生结构体
//整体来说是一个变量，用于储存数据的变量，
//代表数据域
struct student
{
    char name[20];
    char num[10];
    int age;
    char tel[20];
    char addr[20];

};
```
就是那个`data`数据域部分，只不过是一个多维的结构体变量类型
### 主界面部分

    
#### 插入

    case 1:
        printf("\t\t[插入信息]\n");
        printf("请输入姓名，学号，年龄，电话，住址:");
        scanf("%s%s%d%s%s",tempData.name,tempData.num,&tempData.age,tempData.tel,tempData.addr);//单纯的随便的存入内存
        insertNodeByHead(list,tempData); //把先前的存入内存的数据进行操作，有结构化的，符合结构顺序，绑定
        saveInfoToFile("student.txt",list);
        break;
#### 删除
    case 3:
        printf("\t\t[删除信息]\n");
        printf("请输入要删除的学生姓名:");//以学生为关键字
        scanf("%s",tempData.name);
        deleteNodeByAppoinNum(list,tempData.name);
        saveInfoToFile("student.txt",list);
        break;

#### 查找

    case 5:
      printf("\t\t[查找信息]\n");
      printf("请输入查找的学号:");
      scanf("%s",tempData.num);
      if(searchNodeByAppinNum(list,tempData.num) == NULL)
      {
          printf("未找到相关信息!\n");

      }
      else
      {
          printNode(searchNodeByAppinNum(list,tempData.num));
      }
      break;
#### 修改

     case 4:
            printf("\t\t[修改信息]\n");
            printf("请输入要修改学生的学号:");
            scanf("%s",tempData.num);
            if(searchNodeByAppinNum(list,tempData.num) == NULL)
            {
                printf("未找到相关信息!\n");

            }
            else
            {   struct Node* curNode= searchNodeByAppinNum(list,tempData.num);

                printf("请输入新的姓名，学号，年龄，电话，地址");
                scanf("%s\t\t%s\t%d\t\t%s\t\%s\n",curNode->data.name,curNode->data.num,&curNode->data.age,
                      curNode->data.tel,curNode->data.addr);
               saveInfoToFile("student.txt",list);
            }
            break;
#### 打印
    case 2:
            printf("\t\t[浏览信息]\n");
            printList(list);
            break;
⭐可能会报错

**把函数调用和初始化分成两个部分**

>c++可以使用全局的，但是c语言不能使用全局的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200519113820909.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
### 源代码

**main.c**

```c

#include "singleList.h"

//要干什么？
//0.登录界面设计
//2.菜单设计
//3.按键交互
//4.
//选择数据结构
//数组：结构体数组
//链式结构

//系统菜单
struct Node* list = NULL; //初始化
void systemMenu()
{
    //遵循几个菜单就是几个函数
	printf("-----------[学生管理系统]------------\n");
	printf("\t\t0.退出系统\n");
    printf("\t\t1.插入信息\n");
    printf("\t\t2.浏览信息\n");
	printf("\t\t3.删除信息\n");
	printf("\t\t4.修改信息\n");
	printf("\t\t5.查找信息\n");
	printf("---------------------------------------");
	printf("请输入0~5:");
	//每一次交互，可以告诉别人程序是干嘛的
}
//按键交互,通过选择数字进入不同的操作
void keyDown()
{
    int userkey;     //用户需要输入一个值
    struct student tempData; //定义一个student结构体别名，用于存储临时的信息
    scanf("%d",&userkey);//接收用户输入的值

    //根据用户输入的值，进入不同的功能
    switch (userkey)
    {
        case 0:
            printf("\t\t[退出系统]\n");
            //清屏操作，防止闪屏
            system("pause");
            system("cls");
            break;
        case 1:
            printf("\t\t[插入信息]\n");
            printf("请输入姓名，学号，年龄，电话，住址:");
            scanf("%s%s%d%s%s",tempData.name,tempData.num,&tempData.age,tempData.tel,tempData.addr);//单纯的随便的存入内存
            insertNodeByHead(list,tempData); //把先前的存入内存的数据进行操作，有结构化的，符合结构顺序，绑定
            saveInfoToFile("student.txt",list);
            break;
        case 2:
            printf("\t\t[浏览信息]\n");
            printList(list);
            break;
        case 3:
            printf("\t\t[删除信息]\n");
            printf("请输入要删除的学生姓名:");//以学生为关键字
            scanf("%s",tempData.name);
            deleteNodeByAppoinNum(list,tempData.name);
            saveInfoToFile("student.txt",list);
            break;
        case 4:
            printf("\t\t[修改信息]\n");
            printf("请输入要修改学生的学号:");
            scanf("%s",tempData.num);
            if(searchNodeByAppinNum(list,tempData.num) == NULL)
            {
                printf("未找到相关信息!\n");

            }
            else
            {   struct Node* curNode= searchNodeByAppinNum(list,tempData.num);

                printf("请输入新的姓名，学号，年龄，电话，地址");
                scanf("%s\t\t%s\t%d\t\t%s\t\%s\n",curNode->data.name,curNode->data.num,&curNode->data.age,
                      curNode->data.tel,curNode->data.addr);
               saveInfoToFile("student.txt",list);
            }
            break;
        case 5:
            printf("\t\t[查找信息]\n");
            printf("请输入查找的学号:");
            scanf("%s",tempData.num);
            if(searchNodeByAppinNum(list,tempData.num) == NULL)
            {
                printf("未找到相关信息!\n");

            }
            else
            {
                printNode(searchNodeByAppinNum(list,tempData.num));
            }
            break;

        default:      //优化一下，为意外情况做处理，为无法意料的问题做准备，可能用户输入数字不对
            printf("输入错误！，只能输入0~5之间的数字，请重新输入");
            break;

    }
}
//入口函数
int main()
{

 //就相当于给一个普通的变量赋值，只不过这个值不止一个
   //始插入数据操作

    list = createList(); //调用
    readInfoFromFile("student.txt",list);//先读取文件

   // deleteNodeByAppoin(list,2);
    //printList(list);

    //struct Node node1 = {1,NULL};
    //struct Node node2 = {1,NULL};
    //struct Node node3 = {1,NULL};
     //指针变为变量
     /*就是一段内存，比如说a，实际上在内存中有一个固定的位置就是a，这个位置对应一个地址，
      这个地址也就是指针，也就是说只要有内存就有地址，但是不一定有变量，变量是当要有使用某一个内存的需要的时候，
      才会声明，就相当于，告诉系统，我要使用这块地，给我使用权限，当当决定使用之后，就会分配一个名字，这个时候，
      这个地址这个地方的内存就是一个变量，变量名为分配的名字
      总结一下：地址是一直有的，但是变量不一定一开始有
     */
    //1.通过变量的地址初始化
   /* struct Node* p1 = &node1;
    struct Node* p2 = &node2;
    struct Node* p3 = &node2;
    */

    // 指针指向运算符   ->
   // p1->next = p1 ;
    //p2->next = p3 ;

    //2.通过动态内存申请
    //struct Node* p4 = (struct Node*)malloc(sizeof(struct NdoeNOde);
    //p4->data  =4;
    //p4->next =NULL;

    //p3->next  =p4;


    //地址地址就是表示某一个房子在哪
    //node1.next 相当于一个信封，里面放着某个房子地址，是一个容器，等式右边就是内容，表示房子的地址
    //所以可以理解为，等式左边为容器，存放地址，右边就是地址




    while(1)  //程序一开始就进入系统界面，让用户输入值选择
    {
       systemMenu(); //进入系统界面
       keyDown();//等待用户输入值
       system("pause"); //清屏
       system("cls");//清屏

    }
    return 0;
}


```

**数据结构单链表`singleList`**

```c
#ifndef SINGLELIST_H_INCLUDED
#define SINGLELIST_H_INCLUDED



#endif // SINGLELIST_H_INCLUDED


#include <stdio.h>
#include <stdlib.h>
# include <string.h> //因为删除使用姓名字符串作为关键字，所以比较需要使用到字符串string
/*
****************************
*********写数据结构**********
****************************
*/

//定义学生结构体
//整体来说是一个变量，用于储存数据的变量，
//代表数据域
struct student
{
    char name[20];
    char num[10];
    int age;
    char tel[20];
    char addr[20];

};

//写数据结构
//死方法
//1.抽象单一个体，切分
//2.初始化，描述最初状态--->初始化变量
//3.插入，删除
//4.打印遍历

//>也就是要知道需要使用那些操作


//要理解结构体,把结构体当作一个大的变量 -->变量就是一段内存
//定义一个链表，的单个结点
struct Node
{
    struct student data ;
    struct Node* next;

};
//1.链表是：结构体变量和结构体变量链接在一起
//2“指针第二种变为变量的方式--->动态内存申请
//3.用第一个结点表示链表
struct Node* createList()   //创建链表要有东西啊，这个东西就是data数据
{
    //有表头链表
    //无表头链表
    //先要创建结构体
    //1.申请一个变量
    struct Node* ListHeadNode = (struct Node*)malloc(sizeof(struct Node));
    //2.初始化一个结构体变量
    //newNode->data = data; //只有第一个节点，数据
    ListHeadNode->next = NULL;//没有下一个节点
    return ListHeadNode;
};
struct Node* createNode(struct student data)
{
    //1.产生结构体变量
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    //2. 初始化一个变量
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
};


//插入节点,系统录入信息
void insertNodeByHead(struct Node* ListHeadNode,struct student data)   //因为插入的是一个结构体的节点，所以类型是结构体类型
{
    //如何插入
    struct Node* newNode = createNode(data); //直接调用之前创建好的函数
    //先要判断是否为空
    //做链接操作
    newNode->next = ListHeadNode->next;
    ListHeadNode->next = newNode;
    //房子                  地址
    //关键是要表现出指向关系



}
//删除
void deleteNodeByAppoinNum(struct Node* ListHeadNode, char *name)   //只能用数字找位置
{
    struct Node* posFrontNode = ListHeadNode;//表示从表头开始遍历
    struct Node* posNode = ListHeadNode->next;//posNode表示当前删除的位置,posrontNode表示当前位置的前一个
    if (posNode == NULL)
    {
        printf("无相关内容，无法删除！");
        return ;
    }
    else{
         while(strcmp(posNode->data.name,name))
        {   //主要的删除代码操作
            posFrontNode = posNode;
            posNode = posFrontNode->next;
            if(posNode == NULL)
            {
                 printf("无相关内容，无法删除！");
                 return ;
            }
        }
        posFrontNode->next = posNode->next;
        free(posNode);//释放删除的元素
    }

}
//查询操作
struct Node* searchNodeByAppinNum(struct Node* listHeadNode,char *num)
{
    struct Node* pMove = listHeadNode->next;
    if (pMove == NULL)
        return pMove;
    else
    {

        while (strcmp(pMove->data.num,num)) //把遍历的数值和要查找的数值对应，就找到了
        {

            pMove = pMove->next;
            if (pMove == NULL) //防错机制
                break;
        }
        return pMove; //返回这个位置的数值，也就是返回这个结点
    }

};
void printNode(struct Node* curNode )
{

    printf("姓名\t\t学号\t\t年龄\t\t电话\t\t住址\n");
    printf("%s\t\t%s\t%d\t\t%s\t\%s\n",curNode->data.name,curNode->data.num,curNode->data.age,curNode->data.tel,curNode->data.addr);
}
//打印链表
void printList(struct Node* ListHeadNode) //打印列表
{
    struct Node* pMove  = ListHeadNode->next; //第一个节点不放数据，需要移动到第二个节点
    printf("姓名\t\t学号\t\t年龄\t\t电话\t\t住址\n");
    while(pMove) //先要判断是否为空,不为空，则打印
    {
        printf("%s\t\t%s\t%d\t\t%s\t\%s\n",pMove->data.name,pMove->data.num,pMove->data.age,pMove->data.tel,pMove->data.addr);
        pMove = pMove->next;//表示pMove指向原来的下一个地方，就是往下走一个
        //怎样实现一直打印，使用while循环
    }
    printf("\n");

}

//文件的操作
//读
void readInfoFromFile(char *filename ,struct Node* listHeadNode)
{
    FILE *fp = fopen(filename,"r");
    if (fp == NULL)
    {
        fp = fopen(filename,"w");
    }
    struct student tempData;
    while(fscanf(fp,"%s\t%s\t%d\t%s\t%s\n",tempData.name,tempData.num,&tempData.age,
                 tempData.tel,tempData.addr) !=EOF)
    {

        insertNodeByHead(listHeadNode,tempData); //插入数据
        memset(&tempData,0,sizeof(tempData))     ;

    }
    fclose(fp); //表示将这文件关闭，保存的意思

}
//写
void saveInfoToFile(char *filename,struct Node* listHeadNode)
{
    FILE *fp = fopen(filename,"w"); // 以写的方式打开，表示存入，写入数据
    struct Node* pMove = listHeadNode->next;
    while(pMove)
    {
        fprintf(fp,"%s\t%s\t%d\t%s\t%s\n",pMove->data.name,pMove->data.num,pMove->data.age,
                 pMove->data.tel,pMove->data.addr);//相当于打印到文件

        pMove = pMove->next; //相当于循环的更新条件
    }
    fclose(fp);
}
```
## 数值转换

### 模板源代码

**consts.h**
```c
 /* consts.h  */
 #include<string.h>
 #include<malloc.h> /* malloc()等 */
 #include<limits.h> /* INT_MAX等 */
 #include<stdio.h> /* EOF(=^Z或F6),NULL */
 #include<stdlib.h> /* atoi() */
 #include<io.h> /* eof() */
 #include<math.h> /* floor(),ceil(),abs() */
 #include<process.h> /* exit() */
 /* 函数结果状态代码 */
 #define TRUE 1
 #define FALSE 0
 #define OK 1
 #define ERROR -1
 #define INFEASIBLE -1
```

**linkqueue.c**
```c
LinkQueue  *LQueueCreateEmpty()/*创建空链队,返回头指针*/
{ 	
    LinkQueue *plq = (LinkQueue *)malloc(sizeof(LinkQueue));
    if (plq != NULL)
        plq->front = plq->rear = NULL;
    else
	{
		printf("内存不足!! \n");
		return NULL;
	}	
    return plq;
}

 
int  LQueueIsEmpty( LinkQueue *plq )/*判断链接表示队列是否为空队列*/
{
    return (plq->front == NULL);
}



void  LQueueEnQueue( LinkQueue *plq, DataType x)/*入链队*/
{ 
    LQNode *p = (LQNode *)malloc(sizeof(LQNode));
    if ( p == NULL  )
        printf("内存分配失败!\n");
    else 
	{ 
        p->info = x;
        p->next = NULL;
        if (plq->front == NULL)/*原来为空队*/
            plq->front = p;
        else
            plq->rear->next = p;
        plq->rear = p;
    }
}


int  LQueueDeQueue( LinkQueue *plq,DataType * x)/*出链队*/
{ 
    LQNode *p;
    if( plq->front == NULL )
	{
		printf( "队列空!!\n " );
		return ERROR;
	}
    else
	{ 
        p = plq->front;
		*x=p->info;
        plq->front = plq->front->next;
        free(p);
		return OK;
    }
}
DataType  LQueueFront(LinkQueue *plq )/* 在非空队列中求队头元素 */
{ 
    return (plq->front->info);
}

```

**linkqueue.h**
```c
typedef struct LQNode/* 链队结点结构 */
{ 		
    DataType    info;
    struct LQNode *next;
}LQNode;

typedef struct/* 链接队列类型定义 */
{		
    struct LQNode  *front;  		/* 头指针 */
    struct LQNode  *rear;  		/* 尾指针 */
}LinkQueue;
 
```

**linkstack.c**
```c
LinkStack* LStackInit()/*初始化链栈*/ 
{
	LinkStack *h; 
    h=(LinkStack*)malloc(sizeof(LinkStack));
    h->data=1; 
    h->next=NULL;
    return h; 
}

int LStackIsEmpty(LinkStack *ls)/*判别空栈*/
{
	return (ls->next?FALSE:TRUE);
}

DataType LStackGetTop(LinkStack *ls)/*取栈顶元素*/
{
   if(!ls)
   {
     printf("\n栈是空的!");
     return ERROR;
   }
  return ls->data;
}

LinkStack *LStackPush(LinkStack *ls,DataType x)/*入栈*/
{
   LinkStack *p;
   p=(LinkStack *)malloc(sizeof(LinkStack));/*分配空间*/ 
   p->data=x;         /*设置新结点的值*/
   p->next=ls;       /*将新元素插入栈中*/
   ls=p;             /*将新元素设为栈顶*/
   return ls;
}

LinkStack *LStackPop(LinkStack *ls,DataType *e)/*出栈*/
{
   LinkStack *p; 
   if(!ls)/*判断是否为空栈*/ 
   {
	  printf("\n链栈是空的!");
	  return NULL;
   }  
   p=ls;  /*指向被删除的栈顶*/
   *e=p->data;/*返回栈顶元素*/ 
   ls=ls->next; /*修改栈顶指针*/        
   free(p);
   return ls;
}
```

**linkstack.h**
```c
typedef struct node
{
	DataType data;          /*数据域*/
    struct node * next;     /*指针域*/
}LinkStack;                 /*链栈结点类型*/
```

**main.c**

```c
/*进制转换的队列实现*/
#include "consts.h"
typedef int DataType;/*链队元素类型为整型*/
#include "linkstack.h"
#include "linkstack.c"
#include "linkqueue.h"
#include "linkqueue.c"
LinkStack * IntConverDToB(int t, LinkStack *s)/*十进制到二进制整数部分转换函数*/
{
	while(t != 0)
	{
		s = LStackPush(s,t % 2);
		t /= 2;
	}
	return s;
}
void DecConverDToB(float f,LinkQueue *l)/* 十进制到二进制小数部分转换函数*/
{
	int i = 0;
	while(i <= 6 && f != 0) 
	{
		f = f * 2;
		if(f >= 1)
		{
			f -= 1;
			LQueueEnQueue(l,1);				
		}
		else 
			LQueueEnQueue(l,0);
		i ++;
	}
}
void GetRadixPoint(const char * chs, int *pos,int *len) /* 获取小数点位置以及串的长度 */ 
{
	int i = 0;
	int flag = FALSE;
	while('\0' != chs[i])
	{
		if(chs[i] == '.')
		{
			*pos = i;
			flag = TRUE;
		}
		i ++;
	}
	if(flag)
		*len = i;
	else
	{
		*pos = -1;
		*len = i;
	}
}
int StringSplit(const char * chs, char * chs1,char *chs2) /* 拆分字符串chs，分成整数部分chs1和小数部分chs2 */
{
	int pos = 0, len = 0;
	int i = 0;
	int k = 0;
	GetRadixPoint(chs, &pos, &len);
	if(pos != -1)
	{
		for(i = 0; i < pos; i ++) 
			chs1[i] = chs[i];		
		chs1[i] = '\0';
		for(i = pos + 1; i < len; i ++) 
			chs2[k ++] = chs[i];	
		chs2[k] = '\0';
	}
	else
		return ERROR;
	return OK;
}


int IntConverBToD(char * chs, LinkStack *s)/* 二进制到十进制整数部分转换函数*/
{
	int i = 0;
	int sum = 0;
	int k = 1;
	int temp = 0; 
	int tt=0;/*临时输出栈元素使用*/ 
	while('\0' != chs[i])
	{
		s = LStackPush(s,chs[i] - '0');
		i++;
	}
	i = 0;
	while(!LStackIsEmpty(s))
	{
		temp=LStackGetTop(s); 
		s = LStackPop(s,&tt);
		if(temp != 1 && temp != 0)
			return -ERROR;
		if(0 == i)
			sum += temp;
		else
		{
			k *= 2;
			sum += temp * k;
		}
		i++;	
	}
	return sum;
}
float DecConverBToD(char * chs ,LinkQueue *l)/* 二进制到十进制小数部分转换函数*/
{
	int i = 0;
	float sum = 0;
	float k = 1;
	int temp = 0;
	while('\0' != chs[i])
	{
		LQueueEnQueue(l,chs[i] - '0');
		i ++;	
	}
	while(!LQueueIsEmpty(l))
	{
		LQueueDeQueue(l,&temp);
		if(temp != 1 && temp != 0)
			return -ERROR;
		k /= 2;
		sum += temp * k;
		i++;
	}
	return sum;
}
int main(int argc,char* argv[])
{
	int menu;
	int k;
	float temp;
	float f;
	LinkQueue *l;
	LinkStack *s = NULL;
	char chs[100];
	char chs1[100];
	char chs2[100];
	DataType e;
	float num;
	int tt=0;/*输出栈元素时使用*/ 
	printf("           欢迎使用进制转换软件！       \n"); 
	while(TRUE)
	{
		l = LQueueCreateEmpty();
		s = (LinkStack *)malloc(sizeof(LinkStack));
		s->data=1;
		s->next=NULL;	
		printf("*****************************************\n");
		printf("**    1、10-2进制小数转换              **\n");
		printf("**    2、2-10进制小数转换              **\n");
		printf("**    3、退出                          **\n");
		printf("*****************************************\n");
		scanf("%d",&menu);
		switch(menu)
		{
			case 1:
  				getchar();
  				printf("请输入需要转换的数字：\n");
  				scanf("%f",&temp);
  				if(temp > 1.0 && temp != (int)temp) 
				{/* 如果输入的不是一个整数并且大于1 */ 
					s = IntConverDToB((int)temp,s);  
					DecConverDToB(temp - (int)temp,l);
					printf("转化后的二进制小数为：",temp);
					while(!LStackIsEmpty(s))
					{
						printf("%d",LStackGetTop(s)); /* 输出整数部分 */ 
						s=LStackPop(s,&tt);
	  				}
					printf(".",temp);
  					while(!LQueueIsEmpty(l)) /* 输出小数部分 */ 
  					{
  						LQueueDeQueue(l,&e);
						printf("%d",e);  
  					}
  					printf("\n"); 		  	
				}
				else
				{
					if(temp == (int)temp) /* 如果输入的是一个整数 */ 
					{
						printf("%d转化后的二进制小数为：",(int)temp);
						s = IntConverDToB((int)temp,s);  
		  				while(!LStackIsEmpty(s)) /* 输出整数部分 */ 
		  				{
							printf("%d",LStackGetTop(s)); 
							s=LStackPop(s,&tt);
	  					}
	  					printf(".0\n"); 
					}
					else /* 如果输入的是一个小于1的小数 */ 
					{
						printf("----------\n");
						printf("转化后的二进制小数为：",temp);
						DecConverDToB(temp,l);
						printf("0.",temp);
  						while(!LQueueIsEmpty(l))  /* 输出小数部分 */ 
  						{
  							LQueueDeQueue(l,&e);
							printf("%d",e);  
  						}
  						printf("\n"); 
  						getchar();
					}
				}
				break; 
		case 2: 
  			getchar();
  			printf("请输入需要转换的二进制数字：\n");
			gets(chs);		
			k = StringSplit(chs,chs1,chs2);	
			if(k != -1)
			{
				num = IntConverBToD(chs1,s);
	  			f = DecConverBToD(chs2,l);
	  			if(-1 != num && f != -1)
					printf("转化后的十进制形式为：%f\n",(float)num + f);
	   			else
					printf("输入格式错误\n");
			}
			else 
			{
				num = IntConverBToD(chs,s);
				if(-1 != num)
					printf("转化后的十进制形式为：%f\n",(float)num);
	   			else
					printf("输入格式错误\n");
			}
  			
  			break;
		case 3: /* 退出 */
  			return 0;
		default: /* 输入不满足要求，提示输入错误 */
			printf("输入错误，请重新输入！\n");
			continue;	
		}
	}
	return 0;
}

```



### 上机代码
**consts.h**
```c
 /* consts.h  */
 #include<string.h>
 #include<malloc.h> /* malloc()等 */
 #include<limits.h> /* INT_MAX等 */
 #include<stdio.h> /* EOF(=^Z或F6),NULL */
 #include<stdlib.h> /* atoi() */
 #include<io.h> /* eof() */
 #include<math.h> /* floor(),ceil(),abs() */
 #include<process.h> /* exit() */
 /* 函数结果状态代码 */
 #define TRUE 1
 #define FALSE 0
 #define OK 1
 #define ERROR -1
 #define INFEASIBLE -1
```

**linkqueue.c**
```c
LinkQueue  *LQueueCreateEmpty()/*创建空链队,返回头指针*/
{ 	
    LinkQueue *plq = (LinkQueue *)malloc(sizeof(LinkQueue));
    if (plq != NULL)
        plq->front = plq->rear = NULL;
    else
	{
		printf("内存不足!! \n");
		return NULL;
	}	
    return plq;
}

 
int  LQueueIsEmpty( LinkQueue *plq )/*判断链接表示队列是否为空队列*/
{
    return (plq->front == NULL);
}



void  LQueueEnQueue( LinkQueue *plq, DataType x)/*入链队*/
{ 
    LQNode *p = (LQNode *)malloc(sizeof(LQNode));
    if ( p == NULL  )
        printf("内存分配失败!\n");
    else 
	{ 
        p->info = x;
        p->next = NULL;
        if (plq->front == NULL)/*原来为空队*/
            plq->front = p;
        else
            plq->rear->next = p;
        plq->rear = p;
    }
}


int  LQueueDeQueue( LinkQueue *plq,DataType * x)/*出链队*/
{ 
    LQNode *p;
    if( plq->front == NULL )
	{
		printf( "队列空!!\n " );
		return ERROR;
	}
    else
	{ 
        p = plq->front;
		*x=p->info;
        plq->front = plq->front->next;
        free(p);
		return OK;
    }
}
DataType  LQueueFront(LinkQueue *plq )/* 在非空队列中求队头元素 */
{ 
    return (plq->front->info);
}

```

**linkqueue.h**
```c
typedef struct LQNode/* 链队结点结构 */
{ 		
    DataType    info;
    struct LQNode *next;
}LQNode;

typedef struct/* 链接队列类型定义 */
{		
    struct LQNode  *front;  		/* 头指针 */
    struct LQNode  *rear;  		/* 尾指针 */
}LinkQueue;
 
```

**linkstack.c**
```c
LinkStack* LStackInit()/*初始化链栈*/ 
{
	LinkStack *h; 
    h=(LinkStack*)malloc(sizeof(LinkStack));
    h->data=1; 
    h->next=NULL;
    return h; 
}

int LStackIsEmpty(LinkStack *ls)/*判别空栈*/
{
	return (ls->next?FALSE:TRUE);
}

DataType LStackGetTop(LinkStack *ls)/*取栈顶元素*/
{
   if(!ls)
   {
     printf("\n栈是空的!");
     return ERROR;
   }
  return ls->data;
}

LinkStack *LStackPush(LinkStack *ls,DataType x)/*入栈*/
{
   LinkStack *p;
   p=(LinkStack *)malloc(sizeof(LinkStack));/*分配空间*/ 
   p->data=x;         /*设置新结点的值*/
   p->next=ls;       /*将新元素插入栈中*/
   ls=p;             /*将新元素设为栈顶*/
   return ls;
}

LinkStack *LStackPop(LinkStack *ls,DataType *e)/*出栈*/
{
   LinkStack *p; 
   if(!ls)/*判断是否为空栈*/ 
   {
	  printf("\n链栈是空的!");
	  return NULL;
   }  
   p=ls;  /*指向被删除的栈顶*/
   *e=p->data;/*返回栈顶元素*/ 
   ls=ls->next; /*修改栈顶指针*/        
   free(p);
   return ls;
}
```

**linkstack.h**
```c
typedef struct node
{
	DataType data;          /*数据域*/
    struct node * next;     /*指针域*/
}LinkStack;                 /*链栈结点类型*/
```
- ----
**main.c**

```c
# include "consts.h"   //封装的函数库 
typedef int DataType;
# include "linkstack.h" //储存结构实现
# include "linkstack.c"  //功能是实现需要
# include "linkqueue.h" 
# include "linkqueue.c"

//4.十进制转二进制，十进制整数部分的转换
//输入:		要转换的数t
//			使用到的栈s 
LinkStack *IntConverDToB(int t,LinkStack* s )
{
	while(t!=0)
	{
		s =LStackPush(s,t%2); //1.栈，2.要入的数 
		t/=2; //思路清晰 
	}
	return s;
	
}
//5.十进制转二进制小数，十进制小数部分 
//输入 :	要转换的小数部分f
//			用到的队 l
void DecConverDToB(float f,LinkQueue *l)
{
	int i =0;
	while(i<6 && f!=0)  //i<6精度6，F!=0,表示有数 
	{
		f =f*2; //乘2取整 
		if(f>=1) //如何x2后的数大于1，说明整数部分为1，也就是这个高位的二进制 
		{
			f -= 1; //表示减1后的下一个数 
			LQueueEnQueue(l,1); //进队 ,注意第一个参数是l,要插入元素的队 
			
		}
		else
			LQueueEnQueue(l,0);
		i++; 
		
	}
	
}

//1.获取小数点位置，以及串的长度函数 
//输入：chs:传入的数制字符串 
//		pos:位置
//		len:长度 
void GetRadixPoint(const char* chs,int* pos,int* len )
{
	int i = 0;
	int flag = FALSE ; 
	while('\0'!=chs[i])  //当遍历的字符不结尾'\0'时执行语句 
	{
		if(chs[i] == '.')//判断当前字符为.
		{
			*pos = i; //--->就相当于把获取到的小数点的位置，传给实参的pos，直接传递 
			flag = TRUE ;
		 } 
		 i++;
		 
	}
	if(flag)  //flag表示是否有小数点?,有为1，没有为0 
		*len = i;
	else
	{
		*pos = -1;
		*len = i;//继续返回字符串的长度，也就是整数部分字符串的长度 
	} 
}

//2，定义函数StringSplit(),字符串分割函数 
//输入   要拆分的字符串chs
//		 分成整数部分 : chs1
//	     小数部分；  chs2 
int StringSplit(const char* chs,char *chs1,char* chs2) 
{
	int pos = 0 ,len = 0 ; //定义位置和长度
	int i = 0 ;
	int k = 0 ;
	GetRadixPoint(chs,&pos,&len); //调用函数
	if(pos!=-1)  //表示有小数点的情况下
	{
		for(i = 0;i<pos;i++)	
			chs1[i] = chs[i] ;//截取整数字符给   整数数组 
		chs1[i] = '\0';//最后一个字符加为'\0' 结尾标识
		//截取小数部分
		for(i= pos+1;i<len;i++)  //小数点之后的字符为小数部分
			chs2[k++]  = chs[i];//重新定义一个变量k,用来表示小数部分的数组
		chs2[k] = '\0' ;
	} 
	else
		return ERROR;  //没有小数点，表示失败 
	return OK ;
}

/*
如何求出二进制数的某一位的数值和对应的权值相乘， 累加求和得到对应十进制? 
采用字符串数组储存用户需要转换的二进制数，使用StringSplit()函数将其拆分，整数部分的转换使用栈实现 
*/ 

//3.二进制转为十进制，整数部分的转换 
int IntConverBToD(char* chs,LinkStack*s)   
{
	int i =0;
	int sum = 0;
	int k =1;
	int temp=0;
	int tt=0;         //临时输出栈元素使用 
	while('\0'!=chs[i]) //当不为字符串末尾时候，执行 
	{
		s =LStackPush(s,chs[i]-'0'); //先将整数部分高位放入栈 ,chs[i] -'0' :表示将数组的数值变为字符串形式 
		i++; 
	}
	i = 0 ;  //相当于一个预处理工作 
	while(!LStackIsEmpty(s)) //栈非空输出 
	{
		temp = LStackGetTop(s) ;//取栈顶元素
		s= LStackPop(s,&tt) ;//表示改变结构的取栈顶
		if(temp!=1 && temp!=0) //表示不是二进制形式
			return ERROR; 
		if(0==i)  //设立这一步表示 之前的进栈操作成功，则可以进行接下来的操作 
			sum+=temp;
		else
		{
			k*=2;
			sum+=temp*k; //二进制的权值是:1  2 4 8 16 32 64 128 256 ... 因此有规律 
		}
		i++; 
	}
	return sum ;//这个sum的数值就是对应的整数部分的十进制。也就是加权求和的最后结果 
	
}


//这个十进制小数部分转二进制的算法，逻辑上结束 : 是最后得到的f，小数位，也就是.后面的数都为0 ，则结束

float DecConverBToD(char * chs ,LinkQueue *l)/* 二进制到十进制小数部分转换函数*/
{
	int i = 0;
	float sum = 0;
	float k = 1;
	int temp = 0;
	while('\0' != chs[i])
	{
		LQueueEnQueue(l,chs[i] - '0');
		i ++;	
	}
	while(!LQueueIsEmpty(l))
	{
		LQueueDeQueue(l,&temp);
		if(temp != 1 && temp != 0)
			return -ERROR;
		k /= 2;
		sum += temp * k;
		i++;
	}
	return sum;
}

int main(int argc,char* artgv[])
{
	int menu; //定义菜单
	int k;
	float temp;
	float f;
	LinkQueue* l; //定义队
	LinkStack* s = NULL;
	//先定义数组，各种：整个，整数，小数 
	char chs[100] ;
	char chs1[100];
	char chs2[100];
	DataType e;
	float num;
	int tt=0;     //输出栈元素临时使用 
	printf("欢迎使用进制转换软件! \n");
	while(TRUE)
	{
		l = LQueueCreateEmpty() ; //创建链队
		s = (LinkStack*) malloc(sizeof(LinkStack)); //动态申请内存,链栈 
		s->data = 1;//赋初值吗
		s->next = NULL; 
		printf("*********************************\n"); 
		printf("**       1.十-二进制小数转换   **\n");
		printf("**       2.二-十进制小数转换   **\n");
		printf("**       3.退出                **\n");
		printf("***********************************");
		scanf("%d",&menu); //录入选择的数字
		switch(menu) 
		{
			case 1:
				getchar();
				printf("请输入要转换的数字:\n");
				scanf("%f",&temp);
				if(temp>1.0 && temp!=(int)temp)  //temp!=(int)temp表示是 有小数部分，为真小数
				{
					//如果输入的不是整数，而是小数，且大于1，如 : 1.2324 
					s = IntConverDToB((int)temp,s); //十进制转二进制整数部分
					DecConverDToB(temp-(int)temp,l)  ;//十进制转二进制小数部分转换
					printf("转换后的二进制小数:",temp) ;//通过了指针直接改变了temp的值
					while(!LinkStackIsEmpty(s))  
					{
						printf("%d",LStackGetTop(s)); //输出整数部分
						s = LStackPop(s,&tt) ;
					}
					printf(".",temp); //输出一个小数点
					while(!LQueueIsEmpty(l))    //输出小数部分
					{
						LQueueDeQueue(l,&e)	; 
						printf("%d",e);
					} 
					printf("\n"); //回车换行一下 
				}
				else
				{
					if(temp==(int)temp)     //如果输入的是整数 如 ； 12 
					{
						printf("%d转化后的二进制小数为: ",(int)temp);
						s =IntConverDToB((int)temp,s);
						while(!LStackIsEmpty(s))    //输出整数部分
						{
							printf("%d",LStackGetTop(s))	;//取栈顶
							s = LStackPop(s,&tt) ;
						}
						printf(".0\n");
					 } 
					 else       //如果输入的一个小于1的小数 ,如: 0.12 
					 {
					 	printf("----------\n");
					 	printf("转换后的二进制小数为:",temp);
					 	DecConverDToB(temp,l); 
					 	printf("0.",temp);
					 	while(!LQueueIsEmpty(l))  //输出小数部分
						{
							LQueueDeQueue(l,&e);
							printf("%d",e);
					    } 
					    printf("\n");
					    getchar();
						
					 }
				}
				break;
			case 2:
				getchar();
				printf("请输入需要转化的二进制数:\n");
				gets(chs);
				k =StringSplit(chs,chs1,chs2);
				if(k!=-1)      //
				{
					num = IntConverBToD(chs1,s); //转换整数部分 
					f= DecConverBToD(chs2,l) ;//转换小数部分
					if(-1!=num && f!=-1) 
						printf("转化后的十进制形式为:%f\n",(float)num);
					else
						printf("输入格式错误");
				}
				break;	
			case 3 : //退出 
				return 0;
				break;
			default :
				printf("输入错误,请重新输入!\n");
				continue;
				  
		}
	}
	return 0;
}
```
**简洁压缩版**

**main.c**
```c
//进制转换的队列实现

 #include<stdio.h> /* EOF(=^Z或F6),NULL */
 #include<stdlib.h> //库 

 // 函数结果状态代码 
 #define TRUE 1
 #define FALSE 0
 #define OK 1
 #define ERROR -1

typedef int DataType;/*链队元素类型为整型*/

#include "linkstack.c"

#include "linkqueue.c"

//----------------------------------------- 
 
LinkStack * IntConverDToB(int t, LinkStack *s)/*十进制到二进制整数部分转换函数*/
{
	while(t != 0)
	{
		s = LStackPush(s,t % 2);
		t /= 2;
	}
	return s;
}

//1.十进制转二进制，十进制整数部分的转换
//输入:		要转换的数t
//			使用到的栈s 
void DecConverDToB(float f,LinkQueue *l)/* 十进制到二进制小数部分转换函数*/
{
	int i = 0;
	while(i <= 6 && f != 0) 
	{
		f = f * 2;
		if(f >= 1)
		{
			f -= 1;
			LQueueEnQueue(l,1);				
		}
		else 
			LQueueEnQueue(l,0);
		i ++;
	}
}
//2.获取小数点位置，以及串的长度函数 
//输入：chs:传入的数制字符串 
//		pos:位置
//		len:长度 
void GetRadixPoint(const char * chs, int *pos,int *len) /* 获取小数点位置以及串的长度 */ 
{
	int i = 0;
	int flag = FALSE;
	while('\0' != chs[i]) //当遍历的字符不结尾'\0'时执行语句 
	{
		if(chs[i] == '.')//判断当前字符为.说明整数部分结束了 
		{
			*pos = i; //--->就相当于把获取到的小数点的位置，传给实参的pos，直接传递 
			flag = TRUE;
		}
		i ++;
	}
	if(flag) //flag表示是否有小数点?,有为1，没有为0
		*len = i;
	else
	{
		*pos = -1;
		*len = i; //继续返回字符串的长度，也就是整数部分字符串的长度 
	}
}
//3.定义函数StringSplit(),字符串分割函数 
//输入   要拆分的字符串chs
//		 分成整数部分 : chs1
//	     小数部分；  chs2 
int StringSplit(const char * chs, char * chs1,char *chs2) /* 拆分字符串chs，分成整数部分chs1和小数部分chs2 */
{
	int pos = 0, len = 0;//定义位置和长度
	int i = 0;
	int k = 0;
	GetRadixPoint(chs, &pos, &len);//调用函数
	if(pos != -1)//表示有小数点的情况下
	{
		for(i = 0; i < pos; i ++) 
			chs1[i] = chs[i];		//截取整数字符给   整数数组 
		chs1[i] = '\0';//最后一个字符加为'\0' 结尾标识
		//截取小数部分
		for(i = pos + 1; i < len; i ++)  //小数点之后的字符为小数部分
			chs2[k ++] = chs[i];	//重新定义一个变量k,用来表示小数部分的数组
		chs2[k] = '\0';
	}
	else
		return ERROR;//没有小数点，表示失败 
	return OK;
}


/*
如何求出二进制数的某一位的数值和对应的权值相乘， 累加求和得到对应十进制? 
采用字符串数组储存用户需要转换的二进制数，使用StringSplit()函数将其拆分，整数部分的转换使用栈实现 
*/ 

//4.二进制转为十进制，整数部分的转换 
int IntConverBToD(char * chs, LinkStack *s)/* 二进制到十进制整数部分转换函数*/
{
	int i = 0;
	int sum = 0;
	int k = 1;
	int temp = 0; 
	int tt=0;/*临时输出栈元素使用*/ 
	while('\0' != chs[i])//当不为字符串末尾时候，执行 
	{
		s = LStackPush(s,chs[i] - '0');//先将整数部分高位放入栈 ,chs[i] -'0' :表示将数组的数值变为字符串形式 
		i++;
	}
	i = 0; //相当于一个预处理工作 
	while(!LStackIsEmpty(s))//栈非空输出 
	{
		temp=LStackGetTop(s); //取栈顶元素
		s = LStackPop(s,&tt);//表示改变结构的取栈顶
		if(temp != 1 && temp != 0)//表示不是二进制形式
			return -ERROR;
		if(0 == i)//设立这一步表示 之前的进栈操作成功，则可以进行接下来的操作 
			sum += temp;
		else
		{
			k *= 2;
			sum += temp * k;//二进制的权值是:1  2 4 8 16 32 64 128 256 ... 因此有规律 
		}
		i++;	
	}
	return sum;//这个sum的数值就是对应的整数部分的十进制。也就是加权求和的最后结果 
}
//5. 
//这个十进制小数部分转二进制的算法，逻辑上结束 : 是最后得到的f，小数位，也就是.后面的数都为0 ，则结束
//输入   
// 要转化的十进制小数的数组  : chs
//  链队用于储存值，完成这个功能  : LinkQueue 
float DecConverBToD(char * chs ,LinkQueue *l)/* 二进制到十进制小数部分转换函数*/
{
	int i = 0;         
	float sum = 0;
	float k = 1;
	int temp = 0;
	while('\0' != chs[i])  //同样读取整个小数部分的字符串 
	{
		LQueueEnQueue(l,chs[i] - '0');//然后依次存储到队 
		i ++;	
	}
	while(!LQueueIsEmpty(l))  //当非空的时候，然后依次取出，完成了逻辑层面的功能实现的
	{
		LQueueDeQueue(l,&temp);
		if(temp != 1 && temp != 0)
			return -ERROR;
		k /= 2;
		sum += temp * k;
		i++;
	}
	return sum;
}
int main(int argc,char* argv[])
{
	int menu,k;//定义菜单
	float temp,f;
	LinkQueue *l;//定义队
	LinkStack *s = NULL;
	//先定义数组，各种：整个，整数，小数 
	char chs[100];     //定义数字用于存放不同部分的数制字符，如存放整数部分 ，小数部分... 
	char chs1[100];
	char chs2[100];
	DataType e;
	float num;
	int tt=0;/*输出栈元素时使用*/ 
	printf("           欢迎使用进制转换软件！       \n"); 
	while(TRUE)
	{
		l = LQueueCreateEmpty();//创建链队
		s = (LinkStack *)malloc(sizeof(LinkStack));//动态申请内存,链栈 
		s->data=1;//赋初值吗
		s->next=NULL;	
		printf("—————进制转换—————————— \n");
		printf("\t1:  十进制转二进制\n"); 
		printf("\t2:  二进制转十进制\n");
		printf("\t3:  退出程序 \n");
		printf("-----------------------------------------\n");  
		printf("\n");  
		printf("请输入选择的功能对应的数字编号：");                                   
		scanf("%d",&menu);//录入选择的数字
		
		switch(menu)
		{
			case 1:
  				getchar();
  				printf("请输入十进制数：\n");
  				scanf("%f",&temp);
  				if(temp > 1.0 && temp != (int)temp)  //temp!=(int)temp表示是 有小数部分，为真小数
				{
					//如果输入的不是整数，而是小数，且大于1，如 : 1.2324 
					s = IntConverDToB((int)temp,s);  //十进制转二进制整数部分
					DecConverDToB(temp - (int)temp,l);//十进制转二进制小数部分转换
					printf("转化后的二进制小数为：",temp);//通过指针直接改变了temp的值
					while(!LStackIsEmpty(s))
					{
						printf("%d",LStackGetTop(s)); /* 输出整数部分 */ 
						s=LStackPop(s,&tt);
	  				}
					printf(".",temp);//输出一个小数点
  					while(!LQueueIsEmpty(l)) /* 输出小数部分 */ 
  					{
  						LQueueDeQueue(l,&e);
						printf("%d",e);  
  					}
  					printf("\n"); 		 //回车换行一下  	
				}
				else
				{
					if(temp == (int)temp) //如果输入的是整数 如 ； 12 
					{
						printf("%d转化后的二进制小数为：",(int)temp);
						s = IntConverDToB((int)temp,s);  
		  				while(!LStackIsEmpty(s)) //输出整数部分
		  				{
							printf("%d",LStackGetTop(s)); //取栈顶
							s=LStackPop(s,&tt);
	  					}
	  					printf(".0\n"); 
					}
					else   //如果输入的一个小于1的小数 ,如: 0.12 
					{
						printf("----------\n");
						printf("转化后的二进制小数为：",temp);
						DecConverDToB(temp,l);
						printf("0.",temp);
  						while(!LQueueIsEmpty(l))  //输出小数部分
  						{
  							LQueueDeQueue(l,&e);
							printf("%d",e);  
  						}
  						printf("\n"); 
  						getchar();
					}
				}
				break; 
		case 2: 
  			getchar();
  			printf("请输入二进制数字：\n");
			gets(chs);		
			k = StringSplit(chs,chs1,chs2);	
			if(k != -1)
			{
				num = IntConverBToD(chs1,s);//转换整数部分 
	  			f = DecConverBToD(chs2,l);//转换小数部分
	  			if(-1 != num && f != -1)
					printf("转化后的十进制形式为：%f\n",(float)num + f);
	   			else
					printf("输入格式错误\n");
			}
			else 
			{
				num = IntConverBToD(chs,s);
				if(-1 != num)
					printf("转化后的十进制形式为：%f\n",(float)num);
	   			else
					printf("输入格式错误\n");
			}
  			
  			break;
		case 3:  //退出
  			return 0;
		default: /* 输入不满足要求，提示输入错误 */
			printf("输入错误，请重新输入！\n");
			continue;	
		}
	}
	return 0;
}

```

**linkstack.c**
```c
//数据结构的定义 
typedef struct node
{
	DataType data;          /*数据域*/
    struct node * next;     /*指针域*/
}LinkStack; 
//-----------------------------------
LinkStack* LStackInit()/*初始化链栈*/ 
{
	LinkStack *h; 
    h=(LinkStack*)malloc(sizeof(LinkStack));
    h->data=1; 
    h->next=NULL;
    return h; 
}

int LStackIsEmpty(LinkStack *ls)/*判别空栈*/
{
	return (ls->next?FALSE:TRUE);
}

DataType LStackGetTop(LinkStack *ls)/*取栈顶元素*/
{
   if(!ls)
   {
     printf("\n栈是空的!");
     return ERROR;
   }
  return ls->data;
}

LinkStack *LStackPush(LinkStack *ls,DataType x)/*入栈*/
{
   LinkStack *p;
   p=(LinkStack *)malloc(sizeof(LinkStack));/*分配空间*/ 
   p->data=x;         /*设置新结点的值*/
   p->next=ls;       /*将新元素插入栈中*/
   ls=p;             /*将新元素设为栈顶*/
   return ls;
}

LinkStack *LStackPop(LinkStack *ls,DataType *e)/*出栈*/
{
   LinkStack *p; 
   if(!ls)/*判断是否为空栈*/ 
   {
	  printf("\n链栈是空的!");
	  return NULL;
   }  
   p=ls;  /*指向被删除的栈顶*/
   *e=p->data;/*返回栈顶元素*/ 
   ls=ls->next; /*修改栈顶指针*/        
   free(p);
   return ls;
}
```
**linqueue.c**
```c
//数据结构的定义 
typedef struct LQNode/* 链队结点结构 */
{ 		
    DataType    info;
    struct LQNode *next;
}LQNode;

typedef struct/* 链接队列类型定义 */
{		
    struct LQNode  *front;  		/* 头指针 */
    struct LQNode  *rear;  		/* 尾指针 */
}LinkQueue;
//----------------------------------------
LinkQueue  *LQueueCreateEmpty()/*创建空链队,返回头指针*/
{ 	
    LinkQueue *plq = (LinkQueue *)malloc(sizeof(LinkQueue));
    if (plq != NULL)
        plq->front = plq->rear = NULL;
    else
	{
		printf("内存不足!! \n");
		return NULL;
	}	
    return plq;
}

 
int  LQueueIsEmpty( LinkQueue *plq )/*判断链接表示队列是否为空队列*/
{
    return (plq->front == NULL);
}



void  LQueueEnQueue( LinkQueue *plq, DataType x)/*入链队*/
{ 
    LQNode *p = (LQNode *)malloc(sizeof(LQNode));
    if ( p == NULL  )
        printf("内存分配失败!\n");
    else 
	{ 
        p->info = x;
        p->next = NULL;
        if (plq->front == NULL)/*原来为空队*/
            plq->front = p;
        else
            plq->rear->next = p;
        plq->rear = p;
    }
}


int  LQueueDeQueue( LinkQueue *plq,DataType * x)/*出链队*/
{ 
    LQNode *p;
    if( plq->front == NULL )
	{
		printf( "队列空!!\n " );
		return ERROR;
	}
    else
	{ 
        p = plq->front;
		*x=p->info;
        plq->front = plq->front->next;
        free(p);
		return OK;
    }
}
DataType  LQueueFront(LinkQueue *plq )/* 在非空队列中求队头元素 */
{ 
    return (plq->front->info);
}

```
## 括号匹配

### 问题: 
**设某算数表达式中有圆括号(),方括号[],花括号{},编写一算法，判断括号是否匹配**

### 设计要求

1. 程序对输入的表达式能提供适当的提示，表达式包含圆括号，方括号，花括号
2. 允许使用混合的四则运算(+,-,*,/)，和包含变量的表达式
3. 只验证表达式中括号是否匹配，并给出验证结果

### 设计数据结构

括号匹配这种问题，肯定要用到`栈`

储存结构使用栈，用于判断括号是否匹配

### 分析与实现

**判断功能函数**

    int IsCorrect(char *str) //传递字符串


    
         for(i=0,str[i]!=='\0',i++)  //结束条件，可以设定一个标识符 #用于判断表达式结束
         {
            string k;
            if(k=="(" || k=="[" || k=="{") //如果是(,[,{其中一个，则入栈k
              StackPush(list,k); //相当于储存进去了

            switch (str[i])  //接受输入的字符 
              case ")" :  //如果输入的是 "("，把它入栈
                if(StackGetTop() !==str[i])
                  flag = FLASE;
                else
                  flag = TRUE;
                  continue;
                break;
              case "}" :
                if(StackGetTop() !==str[i])
                  flag = FLASE;
                else
                  flag = TRUE;
                  continue;
                break;
              case "]" :
                if(StackGetTop() !==str[i])
                  flag = FLASE;
                else
                  flag = TRUE;
                  continue;
                break;
              default:
                break;
         }
         if(IsEmpty(&st) && flag) 
          return TRUE;
        else
          return FLASE; 




    int main(int argc,char*argv[])  // 参数是什么意思?
     
      printf("输入带三种括号的四则表达式:\n");
      //设计数组用于暂时储存值，作为字符串的形式
      //先要把这个表达式转为字符形式
      //一开始输入的的时候就作为字符输入
      //输入的表达式
      char str ; //定义一个字符串储存结构
      str = (char*)malloc(100*sizeof(char)) ;//动态申请内存
      scanf("%s",str);//输入表达式放入这个储存结构
      status = IsCorrect(str); //调用判断函数
      if(status)
        printf("括号匹配成功");
      else
        printf("括号匹配失败");
      //一步一步先把单个字符输出，存入一个变量，用于暂时储存输入的字符
      //然后只把字符串中的括号((,[,{)输入栈
      //怎么判断，设计一个用于判断是否匹配的函数，将其封装
      //用一个循环，遍历，选择
   


