---
tags: 
  - hexo 
categories: 
    - 教程
toc: true
cover: https://img-blog.csdnimg.cn/20200309213814185.png
---

说到个人博客，我更倾心于GitHub Page方式的个人静态博客，虽然每次需要自己基于Markdown文档生成HTML页面，但是这种方式一是免费，二是可以完全自定义博客且木有广告链接，想用起来极为干净舒适！
<!--more-->

奈何由于国外的GitHub Page访问总是莫名龟速且不稳定，幸好我们有了国内对应的第一个开源代码托管平台码云：码云（https://gitee.com/），因而可以在国内搭建访问与SEO检索都优于GitHub的个人网站。

由于自己刚接触个人网站不久，而且上周才勉强搭建起自己的个人小站。虽然网上有很多详细的教程，但是大多教程彼此借鉴严重，很多步骤只是标准化的回答，导致自己在实际搭建时遇到了不少看似不大，却很严重的“大坑”。作为记录整理，趁热打铁来梳理一下安装过程中遇到的“坑”，也希望可以为其他选择使用Gitee+Hexo搭建个人博客的亲们提供帮助。
# 第一步
 ==配置环境==
   1. **下载node.js  （下载node.js后会有一个npm）**

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020021221004221.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )

  `简单来说，我们要使用npm下载hexo`
       [点击下载](https://nodejs.org/en/)
       ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212205251554.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )

  2. **下载git (git 主要是为了后期部署到远端使用)**
       
       [点击下载](https://git-scm.com/)

       下载好后，直接点击运行gitbash
       然后会出现一个命令行窗口
     

# 第二步
下载hexo 
==step1==
在下载hexo 之前，先验证 `node.js` |  `npm` | 是否有下载好
  打开 `gitbash `在命令行窗口执行以下代码

  ```
   node -v   // 查看是否下载好了node.js可以查看到版本
   npm -v   //查看npm是否下载好
  ```
  
  如图

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212210838612.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

  step2

  正式下载hexo ，执行以下代码
  ```
  npm install -g  cnpm --registry=https://registry.npm.taobao.org
  ```
  这一步是用淘宝的镜像源文件先下载`cnpm`，然后用cnpm下载hexo,这样下载hexo速度更快,为什么不使用npm 下载呢，npm服务器在国外，`访问非常慢`
然后执行以下代码
```
cnpm install -g  hexo-cli
```
（下载hexo 的速度.....也是不尽人意，慢慢等吧！！）
ok,下载好之后
检测一下吧
```
hexo  -v  //查看hexo版本来验证是否下载好
```
如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212211830438.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )

- ----
在初始化之前我们要找一个安装文件的根目录，比如`C盘 `,`D盘` `F盘`都行 

比如我这里在F盘 下面安装好了 ，先建立一个文件，文件名比如叫 blog
在命令行执行以下代码
```
cd /F/    // 这个是切换到F盘的命令
pwd        //这个是查看是否在F盘的命令
mkdir blog    //这个是在 F盘建立一个blog名文件
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/202002122131315.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
`注意`:这个新建的blog目录就是整个博客的`根目录`，如果后期有什么问题，直接把这个目录删除掉。重来就行

初始化博客
```
hexo init    //执行这段代码后，会自动下载一个默认主题和默认文章
```
如果出现初始化不成功可能是没有安装包管理依赖

```
npm install
```

新建一篇文章（当初始化后）
```
hexo n  "xxxxx"
```
xxxx的内容写的是要建立的这篇文章的文件名
比如 ` hexo    init     "这是我的第一篇文章"`
这个文章会保存在根目录的`source/_post/`文件中
如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212214512465.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
然后向这个文件用`markdown格式`写一些内容
`hexo 三剑客`
- hexo  clean   //清空已有的hexo 网站文件
- hexo generator (也可以简写为 hexo  g)    //启动网页文件和新css样式来生成新的文件
- hexo sever (简写hexo  s)    //启动本地服务 ，可以在localhost:4000端口查看本地生成的文件预览（这个就是在正式发布到远端的一种检查）

执行 
```
hexo  clean
hexo  g  
hexo  s
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212215246602.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212215407543.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )

这样本地预览就完成了，如果有什么问题，直接把那个根目录删除。重来就可以
- - --
- ---
# 第三步
部署到 github  /   gitee 
这里使用gitee  ，github国内访问慢
打开gitee官网
[点击打开](https://gitee.com/)
先    ： 注册或者登陆
完了之后进入主页
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212221306987.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212221403357.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
这个公钥的作用就相当于本地和gitee的一个通关证，以便于下次传到gitee时，不用输入密码什么的

> 注意一定要添加个人公钥，第五步的那个添加个人公钥，这个是有写权限的公钥,
不是第一个页面的只读公钥
如果不这么做，那么就部署不到服务器gitee

如何添加公钥，点击
[如何获取个人公钥](https://gitee.com/help/articles/4181#article-header0)

启动gitee  page 这页面就发布文件的地方
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212222818551.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)


## 配置文件

使用编辑器打开对应的博客根目录
这里使用的是vscode编辑器

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212222115390.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
点击`根目录下的config.yml`文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212222156783.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
注意这个地方格外注意的是



```java
#URL
##If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://martinniee.gitee.io/
root: /
```
上述说明中提到可以自定义名称，只需要在root字段修改即可，然而这里有两个容易出问题的地方：

你的URL并不是你所在仓库的地址，而应该是你启动仓库的Gitee Page服务后分配给你的`网站静态域名`，以我个人为例，仓库地址为：https://gitee.com/用户名/仓库名（我新建的网站名称与Gitee账号同名），而网站URL应为“服务--Gitee Page”启动/更新后显示的网站地址：https://仓库名.gitee.io
你的网站目录当然可以和账户不同名，但是那样就需要按照文档说明修改root字段，自己当初定义的名称不同，结果导致域名莫名无法解析，总是无法正确访问网页，因此干脆像GitHub Page一样强制要求使用账号同名新建网站仓库，这样还获得了以账号名为特征的独有域名，一举两得！
##  坑1
```java
#Deployment
##Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@gitee.com:用户名/仓库名.git
  branch: master
```
注意每个`:`后面都要有一个空格
>Git部署目录不是仓库地址！

这里的`repo:  xxxxx`
xxxx是生成仓库后的`克隆/下载`ssh或者https路径
而不是这个仓库的路径 https://gitee.com/用户名/仓库名
所以
  如果使用`ssh`方式
- 应该是  git@gitee.com:用户名/仓库名.git

使用`https`方式
- 应该是  https://gitee.com/用户名/仓库名.git

所以git部署目录后面都要加git

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212223457364.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
## 坑2
url地址不是创建仓库的地址，而是在gitee  page 页面`启动`后生成的最后用来访问个人博客的网址

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200212224139400.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

也就是

`https://仓库名.gitee.io/`格式
如果使用 ssh
**就是**   
```java
type: git
repo: ssh地址
```

如果是使用https形式
**就是**
```java
type: https
repo: https地址
```
完成之后，保存
在命令行窗口切换到根目录
执行
```
hexo clean
hexo g
hexo d     //部署到gitee远端
```
....
.....
你会发现部署失败
我要部署到远端之前还要下载git插件
执行
```
cnpm  install --save hexo-deployer-git
```
安装好之后再执行上述操作
执行 hexo  d之后
第一次要验证ssh

执行
输入邮箱和用户名

再次执行
`hexo  d`
好了
到浏览器输入静态网站网址吧



# **相关链接/教程**

[]()

 
 [gitee hexo博客部署](https://blog.csdn.net/u012937032/article/details/91458026)

 [gitee  公钥如何设置](https://gitee.com/help/articles/4181#article-header0)
  
 [将本地文件部署到gitee](https://www.jianshu.com/p/ac8bf89a21f9)

**[简书markdown语法的使用](https://www.jianshu.com/p/65ab196bef04)**

 [github主题？](https://github.com/Molunerfinn/hexo-theme-melody)
 [超实用博客编写框架](https://hexo.io/docs/)

##  **博客实例**

[butterfly博客主题实例](https://lemonadorable.gitee.io/music/)

##  **技术博客**
[莫提](http://xuewei.world/
)

[知乎-计算机技术博客](https://www.zhihu.com/question/27471510)

[阮一峰的日志](http://www.ruanyifeng.com/blog/)

[博客园-it技术博客](https://www.cnblogs.com/wushuaishuai/p/7738313.html)

[知乎教程-hexo-github建立个人博客](https://zhuanlan.zhihu.com/p/26625249)