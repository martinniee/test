---
title: "【web】-初识HMTL"
tags:
  - web
categories:
  - 笔记
  - 前端
toc: true
date: 2020-07-28 02:08:37
top:
cover: https://user-gold-cdn.xitu.io/2020/7/28/17393a7c0789be6d?w=482&h=267&f=png&s=61390
---

<style>

.yellow{
background:orange;


}

</style>

# 内容简介

**什么是HTML**?

了解html发展史


简介html

    - HTML 是用来描述网页的一种语言。
    - HTML 指的是超文本标记语言 (Hyper Text Markup Language)
    - HTML 不是一种编程语言，而是一种标记语言 (markup language)
    - 标记语言是一套标记标签 (markup tag)
    - HTML 使用标记标签来描述网页


html特点

    - html不需要编译
    - html是一个文本文件
    - html文件必须使用html或htm为后缀名
    - html对大小写不敏感 HTML 和 html是一样的


# HTML基础内容

- [html基本结构]()
- [html标签]()
- [html元素]()
- [html属性]()
- 注释

推荐`初学者`使用记事本工具，或者纯文本工具学习

![](https://img-blog.csdnimg.cn/20200205000854231.png)

**HTML标签属性**

    语法:

    <标签名  属性名1="属性值"  属性名2="属性值" ,...> ...</标签名>

如 

```
     <标签名  属性名1 = "属性值"  属性名2 = "属性值"  ....  > ....... </标签名 >
如    <div    class  = "box"     id     = "one"          > 我是div  </div   >


```


html的注释 是这个样子的 `< !-- 注释内容 -->`

```
例如     <!-- 我是一个html中的注释 -->
        /* 我是css中的注释*/
        //我是javascript中的注释 


```

## 文档段落

现在流行的html就是html5
`< iDOCTYPE>`这是一个声明，必须放在html文档第一行
注意 `<IDOCTYPE>不是html标签`

**网页编码设置**

    问题:
    当网页出现乱码时

    解决 :
    在<head></head>标签之间添加:

    <meta http-equiv= "Content-Type" contect="text/html;charse=utf-8" / >

    - Content-Type:表示文档类型
    - content : 告诉浏览器文件内容是 HTML
    - uft-8 :表示字符编码

### 文字和段落

**1. 标题**

    <h1></h1> ~ <h6></h6>


**2. 段落**

    <p>段落内容... </p>


**3. align对齐属性**

值 | 描述
-- | --
left| 左对齐
right | 右对齐
center |居中~
justify |对行进行伸展，这样每行都有相同的长度

<p align="justify">我是p</p>


**4. 换行**


    <br/>是自结束标签
    <pre> </pre>是保留预格式标签（就是你编辑的文本格式是怎样的，网页显示就是怎样的）
    &nbsp 算是一种空格转义符吧 ，表示空格
    <hr/>水平线标签

**5. 文字段落**

文字斜体`<li></li>,<em></em>`

下标 `<sub>>/sub>
`
上标`<sup></sup>`

下划线`<ins/>`

删除线 `<del></del>`


**6. 特殊符号**

属性 | 显示结果 |描述
-- | --|--
&it; | < | 小于号，或标记
&gt; | > ~
&reg; | ® | 已经注册
&copy; | © | 版权
%trade; | ® | 商标
&nbsp; | Space | 空格

##  列表标签

**列表分为**

    无序列表
    有序列表
    定义列表


**1. 无序列表**

```
<ul>   // 无序列表开始标签
 <li> 列表项 </li>
 <li> 列表项 </li>
 <li> 列表项 </li>
 ......
 </ul> //无序列表结束标签
 在body标签内创建

```

<span class="yellow">效果</span>
// 无序列表开始标签
<ul> 
 <li> 列表项 </li>
 <li> 列表项 </li>
 <li> 列表项 </li>
 ......
 </ul> //无序列表结束标签


 **无序列表type属性**

 值 | 描述
 -- | --
 disc | 圆点
 square | 正方形
 circle | 空心圆

**2. 有序列表**

```
<ul>  //有序列表开始标签
     <li>列表项</li>
     <li>列表项</li>
     <li>列表项</li>
     ....
     </ul>   //有序列表结束标签

```

**有序列表type属性**

值|	描述
-- | --
1|	表示从数字1~9
a	|表示从英文字母 a ~ z
A	|表示从大写英文字母从A ~ Z
i	|表示从罗马字母 开始
I	|表示从大写罗马字母开始

**如**

**3. 定义列表**

```
<dl>
  <dt>定义列表项</dt>
   <dd>定义列表项</dd>   //dt 和 dd是同级标签
   <dd>定义列表项</dd>
   <dd>定义列表项</dd>
   ...
   </dl>
    

```

## 图像标签

插入一个图片

    语法:

    <img src = "图片链接地址" alt = "图片描述"/>


**img属性**

   -  src 路径
   -  alt 图片描述
   -  height 设置 图片高度
   -  width 设置 图片宽度


### 相对路径



**相对路径则是html文件相对于文件的位置，有三种情况**

**1. 比如html文件在目标文件的下面**

![](https://img-blog.csdnimg.cn/20200205015401140.png)


**2. html文件在目标文件的上面**

![](https://img-blog.csdnimg.cn/20200205015440890.png?)

**3. html文件和文件在同级文件里头，比如，html文件和目标文件在c盘下，且路径同为C:\indec.html 和C:\object.png**

![](https://img-blog.csdnimg.cn/20200205015450884.png)
### 绝对路径

相对于在服务器，如把文件存在电脑中的某个磁盘中，如c盘，那么这个目标文件在c盘中就有一个路径，这个路径就是绝对路径，是固定的，储存器中的路径

alt 属性用于 

    - 网速慢
    - src链接属性错误，打不开图片
    - 浏览器禁用图像

>用户无法查看图像，alt可以代替图像显示在浏览器中内容，内容为描述图像内容的文字
用于盲人


## 超链接

    语法:

    <a href="链接地址"> 内容... </a>


上述代码表示 点击 “内容” 后 跳转到href指定的链接地址
href中的链接可以是 外部链接也可以是内部链接

<span class="yellow">如果将链接地址设为“ #”，则默认跳到当前页面顶部</span>

像这种没有标明具体链接地址的，就叫做空链接
`<a href="#"> 内容 </a>`

**a标签链接属性**

属性 | 描述
-- | --
href | 链接地址
target| 链接的目标窗口 self表示从当前页面打开链接 blank表示从一个新的页面打开链接 top parent
title| 链接提示文字
name | 链接命名

**如**

<a href="http://martinniee.top" title="点击跳转到martinniee.top站点" name="我的博客" target ="blank">点击我!!!</a>


title :鼠标悬浮在链接上，会显示相关链接信息

```
<a href="#" target="-self"> 内容 </a> <!--//在本页面打开链接 -->
<a href="#" target="-blank"> 内容 </a> <!-- //在新页面打开链接 -->

```

### 超链接锚功能

跳转到页面指定位置

**1. 在同一页面的情况下跳转**


```
   <a href="#锚名1"> 目录1</a>
   <a href="#锚名2"> 目录2</a>
             ......
             ..............
             ............
             ......
             ..............
             ............
   <a href="...." name="锚名1">内容</a>
   <a href="...." name="锚名2">内容</a> 

```

 通过点击“内容"用锚沉落在锚指定位置”

 ![](https://img-blog.csdnimg.cn/20200205021540731.png)

 **2. 在另一个页面跳转**

 ![](https://img-blog.csdnimg.cn/20200205021753890.png)

 意思是 点击 `"目录1"` 就会跳转到 `new.html`网页的地址中 `锚名 2`的位置

 ## 文本标签

    - <i></i> : 斜体
    - <b></b> : 加粗
    - <small></small> : 用于表示细则 一类的内容，文字大小较一般文字要小，如合同 中的小字，网站版权的声明等
    - <cite></cite> : 网页中所有加书名号的内容都可以用cite标签，表示参考内容
        比如书名，歌名 ，话剧名 ，电影名 例子
        
![](https://img-blog.csdnimg.cn/20200206192733677.png)

虽然看起来也是斜体，但是应用的场合不一样

    - <blockquote></blockquote> :  表示引用的内容，一个长的引用（块级引用）
    - <q></q> : 表示一个短的引用（行内引用）

千万不要因为某些标签使用效果一样就随便用，要注意在合适的场合使用正确的标签

    - <sup></sup> : 使用来设置一个上标
      - 有些时候网页中的名词需要多重解释的时候，这个名词上面就会有一个数字

![](https://img-blog.csdnimg.cn/20200206194432649.png)

**1使用来设置一个下标**

例子 水的化学式H<sub>2</sub>o

    - <del></del> :  使用del表示一个删除的内容
      - del标签中的内容会自动添加删除线
        例子 比如在购物的时候，经常会有价格的变化，如打折
        有 299 ->199 这样会把原来额价格删除掉
    - <ins></ins> : 表示插入的内容
      - Ins中的内容，会自动添加下划线


## 长度单位

**1.像素 px**

像素是我们在网页中使用最多的单位
一个像素就相当于屏幕中的一个小点

我们的屏幕是由这些小点组成的，但是这些像素点是不能直接看到的
不同显示器的像素的大小也是不同的，所谓像素，也就是画面清晰程度，显示效果越好，像素越小，画面清晰，反之像素越大

![](https://img-blog.csdnimg.cn/20200206203420204.png)

**2.百分比%**

也可以将单位设为百分比的形式，这样浏览器会根据父元素的样式来计算该值

**使用百分比的好处**

    当父元素的的属性值发生变化的时候，子元素会根据比例发生变化
    所以我们在创建自适应页面的时候经常使用%作为单位

![](https://img-blog.csdnimg.cn/20200206204915869.png)

**修改百分比后**

![](https://img-blog.csdnimg.cn/20200206204926165.png)

**3.em**

    em和百分比类似，他是相对于当前元素的字体大小计算的
    1em = 1font-size
    使用em时，当字体大小发生变化时，em也会发生变化
    当设置字体相关的样式时，会用到em

## rgb颜色

在css中可以直接使用颜色的单词来表示不同的颜色

    红色 red
    蓝色 blue
    黄色 yellow

**也可以使用RGB来表示不同颜色**


    所谓RGB就是通red green blue三原色的不同浓度来表示不同的颜色

    例子 rgb(红色浓度 绿色浓度 蓝色浓度)

    颜色 浓度需要一个0~255之间的值来表示，255表示最大，0表示没有
    浓度也可以采用一个百分数，需要0%~100%之间的数字，使用%百分比，最终会转化为0-255之间的数字
    0%表示0
    100%表示255

## 字体

网页中将字体分为三大类

    1) serif (衬线字体)
    2) sans-serif(非衬线字体)
    3) monospace(等宽字体）
    4) cursive(草书字体)
    5) fantacy(虚幻字体)

可以将字体设置为这些大的分类，浏览器会自动选定制定字体并应用样式

一般会将字体的大分类，指定为font-family中最后一个字体

css中为我们提供了一个样式font

使用这个样式可以同时设置字体相关样式
可以将字体样式的值，统一写在font样式中，不同的值之间用空格隔开

![](https://img-blog.csdnimg.cn/20200206211823493.png)

![](https://img-blog.csdnimg.cn/20200206212318463.png)

## 行高

可以通过`line-height`属性间接的设置行高

一个文本框有高度

**可以接受的值**

    直接接受一个大小
    可以指定一个百分数，会相对于字体大小计算行高
    传递一个数值，行高会设置为字体大小的相应倍数

![](https://img-blog.csdnimg.cn/20200206213148896.png)

## 文本样式

text-transform :设置文本的大小写

值 | 描述
-- |--
none | 默认值，该怎么显示就怎么显示
capitalize | 单词首字母大写，通过空格识别
uppercase | 所有字母大写
lowercase |所有字母小写

<span class="yellow">例子</span>

<p style=text-transform="none">i love more than u love me !</p>
<p style=text-transform:capitalize>i love more than u love me !</p>
<p style=text-transform:uppercase>i love more than u love me !</p>
<p style=text-transform:lowercase>i love more than u love me !</p>

![](https://img-blog.csdnimg.cn/20200206213713538.png)

letter-spacing :字符之间间距

word-spacing : 单词之间的间距

text-indent :用于设置首航缩进

        给一个正值 : 自动向右缩进指定像素
        给一个负值 : 向左~

        如 

        text-indent : -9999px;

## 表格


```
<table>   表格
<tr>  行
<td> 单元格


```

```
基本语法和结构

<table class="p">
   <tr>
	  <td>1</td>
	  <td>2</td>
	  <td>3</td>
	  <td>4</td>
  </tr>
  <tr>
	<td>5</td>
	<td>6</td>
	<td>7</td>
	<td>8</td>
  </tr>
</table>

```

<table class="p">
   <tr>
	  <td>1</td>
	  <td>2</td>
	  <td>3</td>
	  <td>4</td>
  </tr>
  <tr>
	<td>5</td>
	<td>6</td>
	<td>7</td>
	<td>8</td>
  </tr>
</table>

## 添加删除表格

**带表头的格子**

```
<table class="p" border="1">
		<tr>
			<th>1</th>
			<td>2</td>
			<td>3</td>
			<td>4</td>
		</tr>
		<tr>
			<td>5</td>
			<td>6</td>
			<td>7</td>
			<td>8</td>
		</tr>
</table>

```

![](https://img-blog.csdnimg.cn/2020020621460817.png)

**带标题的格子**

```
默认居中显示
<table class="p" border="1">
	<tr>
		<caption>number</caption>
		<td>1</td>
		<td>2</td>
		<td>3</td>
		<td>4</td>
	</tr>
	<tr>
		<td>5</td>
		<td>6</td>
		<td>7</td>
		<td>8</td>
	</tr>
</table>

```

![](https://img-blog.csdnimg.cn/20200206214734137.png)

表格化为三部分： 表头 ，主体， 脚注

    theard : 表格头（放标题之类）
    tbody : 表格主体（放数据主体）
    tfoot : 表格脚 （放表格的脚注）

表格属性 | 值  | 描述
-- |--|--
Width | Pixels,% | 规定表格的宽度
align | left,cneter,right | 表格相对周围元素的对齐方式
border|pixels |表格边框宽度
Bgcolor | rgb(x,x,x), #xxxxxx,Colorname |表格背景颜色
Cellpadding |Pixels,% | 单元编研，与其内容的空白
cellspacing | Pixels,%|单元格之间的空白
frame | 属性值  |规定外侧边框那些是可见的
rules |属性值 |规定内侧 ~

`width = “xxx%”` 算是可以设置伸展宽度，浏览器宽度变化

```
<table class="p" border="1" width="20%">
<tr>
	<caption>number</caption>
	<td>1</td>
	<td>2</td>
	<td>3</td>
	<td>4</td>
</tr>
<tr>
	<td>5</td>
	<td>6</td>
	<td>7</td>
	<td>8</td>
</tr>
</table>

```

![](https://img-blog.csdnimg.cn/20200206215353876.png)

**固定宽度大小**

```
<table class="p" border="1" width="40px">
<tr>
	<caption>number</caption>
	<td>1</td>
	<td>2</td>
	<td>3</td>
	<td>4</td>
</tr>
<tr>
	<td>5</td>
	<td>6</td>
	<td>7</td>
	<td>8</td>
</tr>
</table>

```

**设置外边框**

```
<table class="p" border="10">
<tr>
    <caption>number</caption>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    
</tr>
<tr>
    <caption>number</caption>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    
</tr>

</table>
```

![](https://img-blog.csdnimg.cn/20200206215558954.png)

**设置背景颜色**

```
<table Bgcolor="red">
<tr>
    <caption>number</caption>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    
</tr>
<tr>
    <caption>number</caption>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    
</tr>

</table>
```

![](https://img-blog.csdnimg.cn/20200206215645816.png)

**单元边沿与其内容之间的间距**

![](https://img-blog.csdnimg.cn/20200206215742386.png)

![](https://img-blog.csdnimg.cn/2020020621575187.png)

**单元格之间的空格**

![](https://img-blog.csdnimg.cn/20200206215815459.png)

**frame ：规定外侧边框那部分可见**

    void
    above
    below
    hsides
    lhs
    rhs
    vsides
    box
    border

![](https://img-blog.csdnimg.cn/20200206220033344.png)

![](https://img-blog.csdnimg.cn/20200206220048494.png)

![](https://img-blog.csdnimg.cn/20200206220104152.png)

![](https://img-blog.csdnimg.cn/20200206220112915.png)

**rules** : 规定内侧边框哪部分可见

    - none : 没有线条
    - groups :位于行组与列组之间的线条
    - rows : 位于行之间的线条
    - cols ： 位于列之间的线条
    - all : 位于行，列之间的线条

![](https://img-blog.csdnimg.cn/20200206220243941.png)


属性 | 值 |描述
-- | --| --
align |left,center,right,justify,char| `<thead>,<tbody>,<tfoot>`内容水平对齐
valign |top,middle,bottom,baseline| `<thead>,<tbody>,<tfoot>`内容垂直对齐


## **表格跨列、跨行**

跨列属性

![](https://img-blog.csdnimg.cn/20200206220453594.png)

**表格跨行**

![](https://img-blog.csdnimg.cn/20200206220525728.png)

不管是跨列还是跨行，注意删除是对应的列、行

<span class="yellow">例子</span>

<table>
<tr>
<td rowspan="2">1</td>
<td>2</td>
<td>3</td>
<td>4</td>

</tr>
<tr>
<!-- <td>5</td> -->
<td>6</td>
<td>7</td>
<td>8</td>

</tr>
</table>

<span class="yellow">例子</span>

<table>
<tr>
<td colspan="2" >1</td>
<!-- <td>2</td> -->
<td>3</td>
<td>4</td>

</tr>
<tr>
<td>5</td>
<td>6</td>
<td>7</td>
<td>8</td>

</tr>
</table>

>跨行，跨列，主要是让内容消失，看起来是那样

## 表单

表单介绍
主要内容

- 表单应用场景
- 表单结构
- 常用表单元素使用
- 表单交互属性

![](https://img-blog.csdnimg.cn/20200206221006285.png)

语法:

    <form action="">



    <form>

**input标签**

type属性值 | 描述
-- | --
text | 文字域
password | 密码域
file | 文件域
checkbox | 复选域
radio | 单选域
Button |按钮
Submit |提交按钮
Reset |重置按钮
Hidden|隐藏域
image |图像域

**`<input />`标签**

语法 :

    <input type="类型属性" name="名称"/>


**1. 文本域**


<input type="text" name="名称"/>

**2. 单选域**


<input type="radio" name="名称"/>

**3. 密码域**

<input type="password" name="名称"/>

**4. 文件域**

<input type="file" name="名称"/>

**5. 复选域**

<input type="checkbox" name="名称"/>

**6. 按钮**

<input type="Button" name="名称"/>

**7. 提交**

<input type="Submit" name="名称"/>

**8. 重置**

<input type="Reset" name="名称"/>

**9. 隐藏域**

<input type="Hidden" name="名称"/>

**10. 图像域**

<input type="image" name="名称"/>


<span class="yellow">注意： 表单本身不可见</span>

### form 元素的添加

标签 | 描述
-- | --
`<input> `| 表单输入标签
[`<select>`](#select)| 菜单和列表标签
`<option> `|菜单和列表项目标签
[`<textarea>`](#textarea) |文字域标签
`<optgroup>` |菜单和列表项目分组标签

例子

```
<form action="">
	<input type="text">
	<select name="" id=""></select>
	<option value=""></option>
	<textarea name="" id="" cols="30" rows="10"></textarea>
	<optgroup></optgroup>
</form>



<td><input type="password" name="paw" maxlength="6" size="25" placeholder="请输入姓名"></td>

```

<form action="">
	<input type="text">
	<select name="" id=""></select>
	<option value=""></option>
	<textarea name="" id="" cols="30" rows="10"></textarea>
	<optgroup></optgroup>
</form>

<td><input type="password" name="paw" maxlength="6" size="25" placeholder="请输入姓名"></td>


**单行文本域的属性**

属性 | 描述
-- | --
Name | 文字域的名称
Maxlength |  用户输入最大字符长度
Size|  指定文本框宽度，以字符为单位,文本框的缺省宽度是20个字符
Value| 指定文本框默认值
`placeholder` | 规定用户填写字段提醒


**1. input单选框**

```
<form action="">
	<input type="text">
	<select name="" id=""></select>
	<option value=""></option>
	<textarea name="" id="" cols="30" rows="10"></textarea>
	<optgroup></optgroup>
</form>


```


<form action="">
	<input type="text">
	<select name="" id=""></select>
	<option value=""></option>
	<textarea name="" id="" cols="30" rows="10"></textarea>
	<optgroup></optgroup>
</form>

> 如果要实现，在`多个选项中单选`，name值要相同

**2. 按钮**

图像提交按钮

```
<form action="">
      <input type="image" name="" src="">
</form>

```
<form action="">
      <input type="image" name="" src="">
</form>

**3. 隐藏域**
```
<form action="">
	<input type="hidden" name="" >
</form>
```

传递信息，用户不需要看到

#### select

```
<form action="">
    <select name="" id="">数字
	     <option value="">1</option>
	     <option value="">2</option>
	     <option value="">3</option>
     </select>
</form>

```

<form action="">
    <select name="" id="">数字
	     <option value="">1</option>
	     <option value="">2</option>
	     <option value="">3</option>
     </select>
</form>

`<select>`标签属性

属性 | 描述
-- | --
name | 设置下拉菜单和列表的名称
multiple| 设置可选择多个选项
size| 设置列表可见选项数目

### textarea

```
<form action="">
        <textarea name="" id="" cols="30" rows="10">
           djhbgejidhbgvwewghejkdhnvkedsgvkeh
        </textarea>
</form>

```
<form action="">
        <textarea name="" id="" cols="30" rows="10">
           djhbgejidhbgvwewghejkdhnvkedsgvkeh
        </textarea>
</form>

### php

`<form>`标签

属性 | 值 |描述
-- | -- | --
action | URL  |提交表单时，向何处发送表单数据
method| get,post|设置表单以和何种方式发送到指定页面
name|form_name | 表单名
target| _parent,_top|何处打开action URl

**post和get区别**

get 

    使用URL 传递参数
    一般用于信息的获取

post

    表单数据作为http请求的一部分
    一般用于修改服务器的资源


