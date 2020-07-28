---
title: ()"c语言知识回顾"
tags:
  - c
categories:
  - 笔记
  - c
toc: true
date: 2020-05-24 17:11:59
top:
cover:
---

# malloc()函数-库函数

```c
# include <stdio.h>  //printf,scanf,NULL

# include <stdlib.h> //malloc,free,rand,system

int main()
{
	int i,n;
	char * buffer; //要动态生成的char类型的变量
	printf("请输入字符串长度");
	
	scanf("%d",&i);  
	
	buffer = (char*)malloc(i+1); //(char*)表示强制类型格式转换
  // 如 要保存abcde 本来是申请5的内存，实际上是abcde\0
  //字符串最后用 "\0"结尾来标识，所以实际上是 要申请6的内存的空间
	
	if(buffer==NULL) exit(1); //判断是否分配成功
	//随机生成字符串
	
	for (n=0;n<i;n++)       
		buffer[n] = rand()%26+"a"; //使用rand
	buffer[i] = "\0"
	
	printf("随机生成的字符串为: %s\n",buffer);
	free(buffer);   //释放内存空间
	
	system("pause");
	return 0;
}	
	
	
	

```

运行结果：

输入字符串的长度：20

随机生成的字符串为：phqghumeaylnlfdxfirc

