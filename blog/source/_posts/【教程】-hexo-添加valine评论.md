---
title: ()"hexo-添加valine评论"
tags:
  - hexo
categories:
  - 教程
toc: true
date: 2020-04-25 22:48:12
top:
link:
cover: https://img-blog.csdnimg.cn/20200425231358362.png
---


# Valine简介

Hexo的评论系统有很多，常见的有以下几个

- 多说
- 网易云跟帖
- 畅言
- 来必力（LiveRe）
- Disqus
- Hypercomments
- valine


首先多说和网易云已经倒下了，其次畅言需要备案，Disqus，Hypercomments和LiveRe都是国外的，加载速度贼慢，甚至有被墙的可能，寻觅了很久之后，从Material主题换成next主题之后，终于找到了一个好用的评论系统，那就是 valine

我使用的是Next 5.1.3版本，本身就已经集成了valine，因此正常情况下是按照官方文档走就可以了，5分钟开启你的评论系统~

# 开启Valine

首先要先注册`Leancloud`

我的评论系统是放在Leancloud上面的

[**点我注册**](https://leancloud.cn/)

注册需要创建一个`应用`,名字无所谓，然后进入 **应用>设置>应用key**

获取`appid`和`appkey`  

如图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020042523071175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

之后在`主题`配置文件下面填写valine的`appid`和`appkey`即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200425230938276.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

>注意把默认的`false`改为`true`哈！！！


**`最后`**

最后！记得在**Leancloud** -> **设置** -> **安全中心** -> **Web 安全域名 **把你的`域名`加进去

刷新一下~ 是不是就能看到评论框了？

**结尾彩蛋**
如果能正常使用，那么恭喜大家。