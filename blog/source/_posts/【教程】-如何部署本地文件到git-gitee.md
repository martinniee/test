---
title: 如何部署本地文件到git/gitee
date: 2020-02-17 02:25:22
tags: 
    - git
categories: 
    - 教程
toc: true
cover: https://img-blog.csdnimg.cn/20200309214721187.png
---


# 先以gitee为例
将`本地文件`部署到`代码托管所平台` gitee
## 1安装git
## 2 配置
1. 打开gitbash终端
输入以下指令
```
git config --global user.name "your name"
git congif --global user.email "email@example.com"
```
这里的name 可以是注册gitee的用户名
email也是注册gitee绑定的对应邮箱
<!-- more -->
2. 查看git信息
输入
```
git config --list
```
### 配置gitee公钥
1. 获取公钥
在gitbash窗口输入
```
ssh-keyen -t rsa -C "xxxxx"
```
`xxxx`内填邮箱地址
三次回车，就可以形成sshkey
[具体可以看gitee官方获取sshkey](https://gitee.com/help/articles/4191#article-header0)

2. 回到gitee个人页面添加shhkey

## 上传项目到gitee仓库
1. 首先在gitee上创建一个用来传输本地文件比如叫`project`，并且管理文件的仓库
2. 在电脑本地磁盘建立一个和这个仓库同名的文件夹`project`用来本地传输到远端仓库
3. 本地进入`project`这个文件夹目录鼠标右键打开`gitbash`输入
```
git init 
```
*目的是把这个目录设置为gi可以管的目录*

4. 在之前建立的gitee仓库`project`项目的页面复制 `https`形式的链接

如图

![图片](https://raw.githubusercontent.com/martinniee/picgoimg/master/img/20200217021028.png)
在本地的gitee仓库同名文件目录`project`打开`gitbash`
输入
```
git remote add origin
```
5. 回车

*相当于添加一个远程控制*

6. 继续输入
```
git pull origin master
```
将gitee仓库代码pull到本地文件中

7. 然后将需要上传到gitee仓库的本地文件放到那个与gitee仓库同名的本地文件`project`中
就可以实现部署到gitee
8. 使用
```
git add .
或者
git add 文件名 （将文件保存到缓存区）
```
执行
```
git commit -m "新添加的文件的描述"
```
为新增添的文件添加描述
执行
```
git push origin master 
```
将本地文件部署到远程仓库

9. 回到gitee 刷新一下那个`project`仓库页面就行