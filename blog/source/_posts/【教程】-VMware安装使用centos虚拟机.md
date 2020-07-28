---
title: "VMware安装使用centos虚拟机"
tags:
  - 教程
categories:
  - 教程
  - VMware安装centos7虚拟机
toc: true
date: 2020-06-23 14:49:07
top:
cover: https://img-blog.csdnimg.cn/20200626090932988.png
---
## VMware安装centos7虚拟机
**网络连接模式**

- 桥接模式
- NAT模式
- 仅主机模式
- 自定义模式

模式 | 描述
-- | --
桥接模式 | 
NAT模式 |
仅主机模式 |
自定义模式 |

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200622111858978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)


### 硬件的基本要求
- CPU : Pentium 或更高性能处理器
- 内存:对于x86,AMD64/Inter64 和Itanium2架构的主机，至少要要求 `512m`内存，对于IBM Power 系列，至少需要`1GB`内存
- 硬盘:至少`1GB`磁盘空间
- 显卡:VGA兼容显卡

>使用[迅雷](http://x.xunlei.com/)下载[iso镜像文件](http://mirrors.163.com/centos/7.6.1810/isos/x86_64/CentOS-7-x86_64-DVD-1810.iso
)



### 使用 `VMware `虚拟机安装centos 7
VMware版本 : `VMware Workstation 12 pro`

#### **step 1**

安装虚拟机软件VMware Workstation 

>这个安装比较简单

#### **step2**

创建`centos7` 虚拟机
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623125633692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623125710429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
**典型安装与自定义安装**

- **典型 :** VMware会将主流的配置应用在虚拟机上，队新手友好
- **自定义:** 可以有针对性的选择，把一些资源加强，把不需要的资源删除，避免浪费资源

选择【自定义】
点击【下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623125813305.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

**这里要注意兼容性，高版本的VMware创建的虚拟机到低版本的虚拟机使用会出现不兼容现象**

如 `VMware 12`创建的虚拟机复制到`VMware 10 `或更低版本，不兼容

如 `VMware 10 `或低版本到`VMware12`打开则不会出现兼容问题

点击【下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623125855767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
选择【稍后安装】
点击【下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623130146813.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
点击【下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623130309864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

- **虚拟机命名** : 在虚拟机多的情况下，容易找到
- **VMware储存位置** ： 默认是c盘，可以改其它盘

点击【下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623130518234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

处理器分配要和实际的需求有关，如果过程中`CPU`不够使用，是不可以再增加的，这里默认
点击【下一步】

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623132147628.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
内存也是要根据实际的需求分配的，![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623132014536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
此时电脑的内存是`8G`，所以分配`2G`
点击【下一步】

**网络连接**

- 桥接
- NAT:
- 仅主机

**桥接 :** 选择桥接，虚拟机和宿主机再网络上是平级的，相当于连接在同一交换机上

桥连接 ，linux可以和其它系统通信，但是可能会造成`ip冲突`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623133454497.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
**NAT** : 就是虚拟机要联网需要通过宿主机才能和外面联网

NAT , 网络转换方式，linux可以访问外网，不会造成`ip冲突`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623134001880.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

**仅主机** : 就是虚拟机直接和宿主机连接起来

你的linux是一个独立的主机，不能访问外网

这里选择【桥连接】
【下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623135136692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
其余两项默认即可

【下一步】
【下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623135300785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
【下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623135639114.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
磁盘容量暂时分配`20G` ，不要选择【立即分配所有磁盘】，这样会立即分配给centos，导致宿主机硬盘容量减少

这个`20G`表示，最多可用20G
【下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623135707698.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
磁盘名称，默认即可
【下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623135811196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
取消不需要的硬件
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020062313584913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623135919654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
然后点击【完成】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623140014420.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

- ----
#### **step3**

安装 `centOS 7 `

右击新建的虚拟机，选择【设置】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623140302514.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
选择使用`iso`映像文件，【启动时连接】一定要勾选上
，然后【确定】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623140600285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
开启虚拟机
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623140654839.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623144011923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
鼠标点击屏幕，待鼠标光标消失后，移动键盘的方向键，选择`第一项`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623140852877.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623141003937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
选择语言，这里使用中文
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623141115327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
**首先设置时间**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623141320350.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
**选择安装的软件**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623141432725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623141533672.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
**选择安装位置，可以进行磁盘划分**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623141632370.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623141702398.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623141833658.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
按同样方法设置下面两个
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623142223223.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
点击【完成】

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623142358261.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
点击【接收更改】

**设置主机名和网卡信息**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623142448493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
首先
- 打开网卡
- 查看是否能获取 `IP地址`
- 更改主机名
- 点击【完成】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623142824158.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
选择【开始安装】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623142910513.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
**设置root密码**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623143207961.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623143025364.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
**创建管理员用户**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623143222249.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623143141658.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
等待系统安装即可.....
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623144433379.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623144508896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

然后输入密码

进入图形界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200623144724552.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
参考教程 [VMware 安装 Centos7 超详细过程](https://www.runoob.com/w3cnote/vmware-install-centos7.html)


