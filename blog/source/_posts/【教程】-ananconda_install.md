---
tags: 
   - 教程
categories: 
    - 教程
toc: true
cover: https://img-blog.csdnimg.cn/20200309214434662.png

---


# 安装python ananconda环境

1. **下载执行程序**

建议使用第三方源，下载稳定快速

<!-- more -->

**[点击下载ananconda](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)**

下载完后

2. **安装**

下载后的文件为`.exe`文件，双击该文件进入安装界面。 

  1、 依次点击Next –> I agree –> Next(在这个界面时博主建议个人电脑使用时选择Just me，公用电脑集体用户选择All user)进入选择安装目录界面。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200304183801371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

2、 在选择安装目录界面，默认安装路径为C盘。如果想更改安装路径，先在想要安装的目录下新建Anaconda的文件夹，然后选择该路径。（安装路径根据自己的实际情况安排，不建议安装在C盘，我的安装路径为`D:\Anaconda\`，如图2所示）。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200304183858559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

3、 然后点击Next进入到`Advanced Options`界面。其中有两个选项框，建议将第二个选项框（Add Anaconda to my PATH environment variable，默认为不选）选上。然后点击`Install`，等待安装完成点击Next –> Finish - >Skip即可（安装过程可能较长，10 ~ 15分钟，请耐心等待

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020030418400714.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200304184032364.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

**3.环境变量**

 1、  找到Anaconda的安装目录

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200304184145868.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

 2、复制上面文件夹的地址

3、找到该目录下Scripts文件夹复制其地址

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200304184303416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

 4、将上面复制的地址分别添加到环境目录下

**如何打开环境变量**

 > 我的电脑>右键选择属性>选择高级系统设置>选择“高级”下的“环境变量”>选择系统变量下的Path选项并双击打开>

 如果是windows10系统，点击新建，将两个路径分别添加进去

如果是windows7系统，将光标定位到最后一行，输入一个英文分号，粘贴一条路径，再写一个分号，再粘贴一条路径

5、分别点击确定关闭所有页面

6.验证环境变量是否配置成功

  1、开始菜单，输入cmd回车，打开终端

  2、输入python回车

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200304184544143.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

  3、出现python环境基本信息则证明安装成功!!  