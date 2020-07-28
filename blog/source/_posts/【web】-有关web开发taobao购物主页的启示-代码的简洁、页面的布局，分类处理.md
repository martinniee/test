---
title: 有关web实战开发/设计思路，技巧，心得
tags: 
	- web
categories: 
	- 笔记
	- 前端
	- 项目实战
toc: true
date: 2020-04-21 151:52:38
top: 
cover: https://img-blog.csdnimg.cn/20200309213908902.png
---


# 页面的结构分析


<iframe src="//player.bilibili.com/player.html?aid=56277133&bvid=BV18441137Sg&cid=133876455&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width=886 height=550> </iframe>


### 页面的布局
1. #### ==分析布局==
     1.划分依据
          - 自上到下，从左至右，尽量放到一行中
   
2. #### ==划分页面区块==
    1. 区分的依据
     --内容
     --颜色
     --间距
     --框架
   2. 划分区块要简洁，概括，合理，富有逻辑
3. #### ==合理的分类==
     1. 使用`id class`选择器，要根据这些选择器的属性使用，id用于`特殊`的元素，`唯一性`比较强的元素，class使用要随意一些
     2. 分类，
        1. 因为相同而分为`同一类`
          2. 因为不同而`区分为一类`
4. #### ==命名==
    1. 合理，简洁，概括，知名见意

5. #### ==样式文件的分类==
    1. css样式的分类，根据之前的结构划分，和区块划分，进行后续的样式分类
    2. 每一个css样式都要达到使用范围广，高度统一，区分依据合理，便于后期维护和修改
          1. 一般要有`reset.css`重置样式，消除因为元素自带的默认属性样式，在分析页面的结构和布局的时候就要考虑的
             1. 如 一些元素的margin ,padding 
             2. 一些特殊元素的样式 ，
-- 如 ul li 的 list-style 要设置为none 
--a  的默认下划线样式 text-decoration设置为 none
--还有body自带的margin ,padding
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209194047737.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
          3. `index`，页面基本样式


6. #### ==对于一些实战开发知识点的回顾和新的知识的了解==
   1. `line-height`属性
        line-height 的表示方式 ,有大致5种
        1. line-height : normal
        2. line-height : 1.5 (数字形式)
        3. line-height : 200% （百分比的形式）
        4. line-height : 16px (像素的形式)
        5. line-height : 5em （em 的形式）
    2. 区分不同方式的区别和用法
     --（`1`） (1.5数字形式）
-  例子
```javascript
<head>
<style>
   body{
      line-height:1.5;
      font-size: 14px;
   }
   h3{
       font-size:20px;
   }
   p{
    font-size:20px;
   }
   
</style>
<head/>

<body>
 <div>
  <h3>我是h3中元素</h3>
  <p >我是p元素中的标签<p/>
 <div/>
</body>
```
- 对于使用`line-height : 1.5(数字形式的)`，这个元素，此时是body
那么这个元素就影响它的子元素的行高，也就是**子元素的行高 = 父元素元素的 line-height 属性值  x 子元素的 字体大小**
`计算结果应该是 1.5 x 20 =30`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209160107662.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
     --(`2`) % (百分比形式)
父元素设置line-height : 200% ，它的~~子元素的行高就不是父元素行高 x 子元素字体大小了~~，而是**这个父元素的line-height属性值 x 父元素字体大小，也就是子元素继承父元素的最终行高**
- 如图

```javascript
<head>
<style>
   body{
      line-height:200%;
      font-size: 40px;
   }
   h3{
       font-size:20px;
   }
   p{
    font-size:20px;
   }
   
</style>
<head/>

<body>
 <div>
  <h3>我是h3中元素</h3>
  <p >我是p元素中的标签<p/>
 <div/>
</body>
```
`计算结果应该是 200% x 40 = 80`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209161218247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

- - --
`字体样式`
在body内统一设置字体样式
字体的表示方法
```java
  直接使用中文               font-family:"宋体;
  使用unicode编码         	font-family: '\5b8b\4f53';
  使用英文名                 font-family: SinSun;
  ```
  使用unicode编码的方式可以在在任何语言环境下都可以使用
  

`预定义样式`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209195109980.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

样式模块化
@ 规则
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209195707683.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
@ media 移动端开发 媒体查询

@ font-face  自定义字体
        
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209200744779.png)
小图标  显示 收藏夹， 网页tap页 
这个图标其实是一个图片，但是不是普通的图片格式
`.ico`格式
这个小图标文件要放到网站根目录中
使用代码  `<  link  rei=" icon "  href="favicon.ico">`


`<   base href=" ">`     可以统一设置基础路径
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020020920185537.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
`li `标签 使用条件，当看页面结构的时候，感觉是换汤不换药的样式，风格，或者结构，就可以使用
li搭配  ul 使用  如
```java
<ul>
   <li></li>
   <li></li>
   <li></li>
   <li></li>
<ul/>
```
# 头部信息样式和自定义图标
`图标的使用`
1. iconfont(阿里巴巴图标网站)
2. 下载代码
3. 使用
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209204810401.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209205226587.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
`疑问`
某个元素内的行内元素换行导致之间有空格，空格大小的与这个元素内的的行内元素字体大小有关
例如
```java
<ul>
	<li>
	    <a>你好</a>
	    <a>好啊</a>
	</li>
	<li>
	   <span>|</span>
	    <span>></span>
	</li>
</ul>
```
此时	第二个li 标签中的 span  与 span之间有空格，这个空格与父级元素 li 的字体大小有关，但是li又没有设置文本，但是可以把子元素的a 的文本当作自己的文本，当把li的字体大小设置为0时，span之间的空格就没有了，但是 a 的字体文本大小也为 0 这时可以设置a 的字体大小，总体来所。并不冲突

# 头部广告结构和样式
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209224613945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209225012773.png)
comment语法快捷书写


怪异盒模型（ie盒模型）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200209231342336.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
