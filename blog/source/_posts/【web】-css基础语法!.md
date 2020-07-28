---
title: 【web】-css基础
tags: 
  - web
categories: 
  - 笔记
  - 前端
date: 2020-2-21 22:19:00
toc: true
cover: https://img-blog.csdnimg.cn/20200309213908902.png
---
<style>
	/* span高亮 */
	.yellow{
		background:yellow;
		font-weight:bold;
	}
	.one{

		color:red
	}

	.two{
		color:green
	}

	.three{
		color:blue
	}

	.fOne{
		text-decoration:underline;
	}
	.fTwo{
		text-decoration:line-through;
	}
	.fThree{
		text-decoration:overline;
	}
	.fFour{
		text-decoration:blink;
	}
	.fFive{
		text-decoration:none
	}
	/* css层叠 */
	.cssCover{
		color:red;
	}
	.cssCover{color:blue;}
	
</style>

# 1css基础语法!

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200108182417204.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
 

**选择器** : 对什么?

**声明** : 做什么?

```
【选择器】 : 需要改变样式的html元素，如，一些标签

p , h1~h6 ,div ,span

【属性】(property):  

  希望设置样式属性(style attribute)


【声明】 :

    属性 + 属性值

  如 

  {color ：red ; font-size:blod}
  {属性 : 属性值 ;属性 :属性值}

  Ø 不同的声明之间用 ; 隔开，主要是为了区分

      selector {`property`: value}
     如
      h1 {`color`:red; `font-size`:14px;}

      p {text-align:center; color:red;}

    • p标签默认里面内容是文本，所以上面的设置表示
    为设置
    • h1标签内的文字设置颜色为红色
    • 字体大小为14个像素


p {
  text-align: center;
  color: black;
  font-family: arial;
}

body {
      color: #000;
      background: #fff;
      margin: 0;
      padding: 0;
      font-family: Georgia, Palatino, serif;
  }

```

 
##  	颜色的不同写法和形式

```

	除了英文单词 red，我们还可以使用十六进制的颜色值#ff0000：
	
  	p { color: #ff0000; }
	为了节约字节，我们可以使用 CSS 的缩写形式：  	p { color: #f00; }

  	我们还可以通过两种方法使用 RGB 值：
	
	p { color: rgb(255,0,0); }
	p { color: rgb(100%,0%,0%); }
	
  > 	请注意:当使用 RGB 百分比时，即使当值为 0 时也要写百分比符号。但是在其他的情况下就不需要这么做了。比如说，当尺寸为 0 像素时，0 之后不需要使用 px 单位，因为 0 就是 0，无论单位是什么。
	
```

## css引用方式

- 行内(内联)
- 内部(嵌入)
- 外部
- 导入

**1. 行内样式(内联样式)**

```html
	写在  html 标签内
	
	<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
	<title>Document</title>
	----------------------
	<style type="text/css" >
	/* css样式写在这里...*/
	</style>
      ----------------------
	</head>
	
	
<p  style = "color : red ;">  文本内容 </p>
		
提示：什么是开始标签，什么是结束标签
通常一般标签是双标签，就是两个<><>，组成
如：

<p></p>
<h1></h1>
<span></span>
<div></div>
		


```
**2. 内部样式(嵌入样式)**

```html
	<head>
	<style type="text/css">
	 /*css样式写在这里*/
	
	</style>
	
	</head>
	
	
	注:对于高版本的浏览器，会忽视style标签中的注释，去解析
	
	
	

```

**3. 外部样式**

```
链入外部样式
就是写在 xxx.css 文件里面



```
**4. 导入式**

```

@import  " xxx.css "


@import "外部文件地址"

url ( 外部文件地址)



```

**css样式引入总结**

类别 | 引入方法 | 位置  |加载
-- | --| --| --
行内样式 |  开始标签`<style>`内 |  html文件内 | 同时
内部样式 | `<head>`中的`<style>`内 | html文件内 | 同时
链入外部样式 | `<head>`的`<link>`引入| css样式文件和html分离| 页面加载时，同时加载css样式
导入式`@import `| 样式代码最开始处|css样式和html文件分离 |读取html文件后加载

**使用链入外部样式的好处**

- css和html分离
- 多个文件可使用同css文件
- 多文件引入同css文件，css只需下载一次

## **css注释**

	
	html文件里面的注释： <!--注释语句-->
	css文件里面的注释： /*注释语句*/
	
![](https://img-blog.csdnimg.cn/20200108190425786.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200108192411345.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )




![在这里插入图片描述](https://img-blog.csdnimg.cn/20200108194731616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )


# css优先级

		行内样式优先于内部样式
		内部样式优先于导入样式

    		行内 > 内部 > 外部

		注意：
			
			链入外部样式css和内部css的优先级取决于所处位置的先后，遵循 就近原则，最后定义的优先级高

**情况1.**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200108201403171.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
chrome浏览器显示效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200108201458267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
同时使用 `链入外部样式，内部样式，行内样式`

很显然，效果显示的是`行内样式`设置的的样式`黄色`

- --


**情况2.**


 注意此时`内部样式`和`链入外部样式`的位置先后顺序
 此时`内部样式`在`链入外部样式`的下面，也就是更靠近body标签

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200108202109629.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
chrome浏览器显示效果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200108202143443.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
同时使用 `链入外部样式，内部样式`
很显然，显示的是`内部style样式`的显示效果`蓝色`

**情况3.**


同样把两种不同样式方法互换位置
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020010820291766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
chrome浏览器显示效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200108202952433.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70  )

		<link  rel="stylesheet"  href="test.css">

的意思是，`test.css`中设置样式的标签元素是当前引入这个链接文件的html文件里面对应的标签元素,这里的标签元素就是 h1,也就是为html文件中的h1标签设置样式

此时显示的颜色是红色，说明采用了`链入外部样式` `test.css`的样式`红色`

也就是对于`内部样式`和`链入外部样式`对比来说，谁更靠近body标签，或者最近，谁优先级最高，简称`就近原则`，也可以理解为后面写的样式`覆盖`了前面写的样式

# css权值和优先级

`! important`

可以调整样式的优先级
添加在样式规则之后，中间用空格隔开

![](https://img-blog.csdnimg.cn/20200209173115681.png)

**css优先级总结**

	- !imporant声明高
    - css使用方法优先级

		行内> 内部样式>外部样式

	注意：link链入外部样式的优先级和style内部样式优先级，取决于先后顺序

	- 样式表中的优先级
	id选择器> class选择器> 标签选择器> 通配符选择器

权值相同 |	权值不同
-- | --
就近原则|	使用权值高的 


也就是，如果一个样式使用了`!important`那么这个样式的优先级自动为最高	

**选择器权值**


	Ø 标签选择器 : 1
	Ø 类选择器 : 10
	Ø ID选择器 : 100
	Ø 通配符选择器 : 0
	Ø 行内样式 : 1000


![](https://img-blog.csdnimg.cn/20200209174358617.png)



# 选择器

1. [ID选择器](#id-选择器)
2. [标签选择器](#标签选择器)
3. [类选择器](#类选择器)
4. [群组选择器](#群组选择器)
5. [全局选择器](#全局选择器)
6. [伪类选择器](#伪类选择器)

不常用

1. [属性选择器](#属性选择器)
2. [子元素伪类选择器](#子元素伪类选择器)
3. [兄弟选择器](#兄弟元素选择器)
4. [否定类](#否定类)
## 标签选择器 

```
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8>
		<meta name="viewport" content="width=device-width，initial-scale-1.0">
		<meta http-equiv="X-UA-Compatible" content="ie=edge">
		<title>Document</title>
		<style>
		h1{
			color:blue;
		}
		#p1{
			background: red;
		}
		#p2{
			background:blue;
		}
		#p3{
			background: orange;
		}
		</style>
	</head>
	<body>
		<h1 >你好!，我是h1标签</h1>
		<p id="p1”>我是p1</p>
		<p id="p2”>我是p2</p>
		<p id="p3”>我是p3</p>
	</body>
</html>

```

## ID 选择器

```
# + 要设置样式的标签 {


}

如：
#  h1 { 
background  : red ;
}

> 注意:id选择器标志是#,每个id是唯一的，就像身份证号码一样

p,h1,h2,h3,h4{font-size: 30px;}
/*P标签样式*/
p{
	color:blue;
	font-famiLy:"隶书";
}

h1{color:red;}


```

【案例】


![](https://img-blog.csdnimg.cn/20200108210857368.png)

效果

![](https://img-blog.csdnimg.cn/20200108210921365.png)

## 类选择器



![](https://img-blog.csdnimg.cn/20200108211508470.png)


```

格式 :

. + 要设置样式的标签/元素的class属性{


}

如 
<html>
	<head>
		<style>
		.one {

			color:red
		}

		.two {
			color:green

		}

		.three {
			color:blue
		}
		</style>
	</head>
	<body>
		<p class="one">我是one</p>
		<span class="two">我是two</span>
		<div class="three">我是div</div>
	</body>
</html>
```


<p class="one">我是one</p>
<span class="two">我是two</span>
<div class="three">我是div</div>

【案例】


![](https://img-blog.csdnimg.cn/20200108212010951.png)

与id选择器不同的是，可以为`多个不同`的标签设置同一个class属性

【案例】

![](https://img-blog.csdnimg.cn/20200108212425563.png)

可以为同一个元素设置多个不同的标签属性

【案例】

![](https://img-blog.csdnimg.cn/20200108212905829.png?)

		虽然是不同的class属性值，但是都是为一个元素设置的样式
		分别是
			• 设置背景颜色为红色
			• 设置文本居中
			• 设置字体颜色为白色
		注意：class 多个属性之间要用空格隔开

## 群组选择器

```
群组的意思就是集体统一设置样式

p,h1,h2,h3,h4{font-size: 30px;}
/*P标签样式*/
p{
	color:blue;
	font-famiLy:"隶书";
}

h1{color:red;}


不同选择器之间用,号隔开

```

## 全局选择器

		所有标签设置样式

		*{

			color :blue
		….
		}

## 后代选择器

		<em>cSS层叠样式</em>
		<p><em>cSS</ em>层叠样式</p>
		<p><em>css</em>层叠样式</p>

		使用后代选择器，选择器之间用 空格隔开

		p em {font-size:40px;}

		此时的p 和 em 是一种包含与被包含的关系，称为父代与子代的关系

		<p>
			<em>
				<span>
					<a>
					</a>
				<span>
			</em>

		</p>

		越被包含在内部越是子代
		<a> </a>是最子代
		而越在外面越是父代
		如<p></p>是最父代

比如想为p下面的span设置样式可以

		p  span {

		}

		如果当前没有其它span，也可以直接

		span {

		}
		如果想为p下面的a设置样式可以
		p  span  a{

		}

		越详细越准确
		如果此时p下面只有一个a，那么可以使用


		p  a{


		}
		因为只有一个a,不用怀疑其指向性，肯定是这个 p下面的 a

## html文档结构

![](https://user-gold-cdn.xitu.io/2020/7/26/1738b45c52969238?w=1058&h=436&f=png&s=115871)

按照上面的结构，越在上面越是父代，或者祖先代，越在下面越是子代/后代

## 伪类选择器


	- 定义选择器特殊状态下的样式
	- 无法用标签，id，class以及其它属性完成的

链接伪类  |  说明
-- | --
: link | 未访问的链接状态
: visited | 已访问的链接状态
: hover | 鼠标悬浮的链接状态
: active | 激活的链接状态

优先级： `link>visited>hover>active`

也就是说要按照这个顺序写，暂时不需要显示效果，可以空着

		• 如：暂时不需要visited效果，需要link ，hover,active显示效果

		那么在写样式的时候,按照这个顺序
		1 link
		2
		3 hover
		4 active

切不可顺序错误

**伪类** 

`: hover`和 `: active`

	1  : hover 用于鼠标经过某个元素
	2  : active 用于元素被激活时(即按下鼠标之后，松开鼠标之前)

【案例】

![](https://img-blog.csdnimg.cn/20200108221601464.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200108221435677.gif)

当鼠标滑入表面的时候变成蓝色

**说明:**

     1. a:hover必须置于a:link和a:visited之后，才有效
     2. a:active 必须置于a:hover之后，才有效
     3.伪类名称对大小写不敏感

## 不常用的选择器

### 属性选择器

	• 作用： 可以根据元素中的属性或者属性值来选取指定的元素
	• 语法： 标签 [属性名] 选取含有指定属性的元素


【案例】

![](https://img-blog.csdnimg.cn/20200209024323817.png)

为某个标签 的 title属性以某个数字/字母`结尾`或 ` 开头`的属性值设置属性

`[属性名$ = "属性值"]` // 相当于定位符合这个标准的标签 ，以xxx结尾

`[属性名^ = "属性值"]`  // 以xx开头


![](https://img-blog.csdnimg.cn/20200209025026887.png)

为 标签的属性 title 以 `包含`数字或` 字母`的属性值设置属性


`[属性名* = "属性值"]`


![](https://img-blog.csdnimg.cn/20200209025946170.png)

### 子元素伪类选择器

fisrt-child


<span style="font-size:16px;font-weight:bold">1 表示选择某父元素的第一个某标签</span>

		<body>   
			<p>我是一个p 元素</p>
			<p>我是一个p 元素</p>
			<p>我是一个p 元素</p>
			<p>我是一个p 元素</p>
			<div>
				<p>我是div中的p元素</p>
			</div>
		</body>


![](https://img-blog.csdnimg.cn/20200209031135371.png)

<span style="background:yellow">表示选取某父元素含有p标签且这个标签是作为父元素的第一个子标签的标签</span>

![](https://img-blog.csdnimg.cn/202002090320007.png)

<span style="font-size:16px;font-weight:bold">2 指定父元素下的某位子元素</span>


`nth-chlid()`


![](https://img-blog.csdnimg.cn/20200209032450606.png)

<span style="font-size:16px;font-weight:bold">3 选定父元素的子元素的序号位奇数的 或者序号位偶数的那个所在的元素</span>

![](https://img-blog.csdnimg.cn/20200209033212354.png)

		First-of-type

		Last-of-type

		nth-of-type

是在`当前类型中排列`(相当于限制了类型)，忽略了其它类型

和`first-child` 非常类似，只不过child是在`所有的子元素中排列`


这种作用时锁定要表达元素的`相对位置`，而没有指定某一个元素，
只要是排在第一个就是显示红色，具有普遍适用性

### 兄弟元素选择器

为`h3` 标签后面的`p 元素`设置样式，背景颜色为红色

<span style="font-size:16px;font-weight:bold">1后一个兄弟元素选择器</span>


	• 作用: 可以选中一个元素后面紧挨着的指定的兄弟元素
	• 语法： 前一个 -, 　后一个 +
	例子

	
![](https://img-blog.csdnimg.cn/20200209164102794.png)

<span style="font-size:16px;font-weight:bold">2 选中后边所有的兄弟元素</span>


	• 语法 ：  " ~  "后边所有
	• 例子


![](https://img-blog.csdnimg.cn/20200209164547632.png)

### 否定类


	• 为所有p 标签设置背景颜色为黄色，除了class = hello 的p标签
	否定伪类
	• 作用： 可以从已经选出的元素中剔除某些元素
	• 语法：
		标签:not(选择器)
		表示 除了标签的选择不设置样式，其它设置，
	• 例子

<span style="background:yellow">相当于一个反选</span>


![](https://img-blog.csdnimg.cn/20200209165307424.png)


# 文字样式及排列

**文字样式属性**

- 字体 : [font-family](#font-family)
- 文字大小 : [font-size](#font-size)
- 文字颜色: [font-color](#font-color)
- 文字粗细: [font-weight](#font-weight)
- 文字样式: [font-style](#font-style)
- 文字对齐样式
  - [text-align](#text-align)
  - [vertical-align](#vertical-align)
- 文字标注: [text-decoration](#decoration)

## font-family

**定义元素内的文字以什么字体显示**

	语法：font-famliy:[字体1]，[字体2]，....

**说明**

		• 含空格字体名和中文，用英文引号(“)括起来
		• 多个字体，用英文逗号”,”隔开
		• 引号嵌套，外使用双引号" "，内使用单引号' ' 

![](https://img-blog.csdnimg.cn/20200209182722326.png)

**font-family字体属性**

	font-family属性值 : 具体字体名 ,字体集

**字体集 :**
	
	► Serif
	► Sans-serif
	► Monospace
	► Cursive
	► Fantasy


![](https://img-blog.csdnimg.cn/20200209183022564.png)

## font-size

**`font-size`字体大小
定义元素内的文字大小**

	语法：
	font-size : 绝对单位/相对单位

**1. 绝对单位**

属性值 | 说明
-- | --
in | 英寸 Inch ,1英寸 = 2.54cm
cm |一厘米 = 0.394英寸
mm |一毫米 = 0.1厘米
pt | 磅，印刷的点数,72磅  =1英寸
pc | Pica ,1pc = 12pt


**2. 相对单位**

属性值 | css2缩放系数1.2
-- |--
xx-small | 9px
x-small | 11px
small | 13px
medium | 16px
large | 19px
x-large | 23px
xx-large | 28px

**font-size 文字大小**

	► px : 像素
	► em / % 

> 绝对大小，不能随浏览器分辨率或父元素的大小改变而改变

## font-color

字体颜色

![](https://img-blog.csdnimg.cn/20200209183607521.png)

## font-weight

**为元素内文字设置粗细**

	语法 : 

	font-weight : normal | bold | bolder |lighter | 100~900

>默认值 : normal 

400等同于 normal ,`700`等同于`bold`

## font-style

**为元素内文字设置样式**

	font-style : normal |italic | oblique

## 字体对齐样式

### **text-align**

text-align : 设置元素内的文本`水平`对齐方式

	语法:

	text-align : left |right | center | justify

>注 : 该属性对`块级元素`设置有效(块级元素占整行，所以可以左右动，才有左右之分)

<span class="yellow">块级元素和内联元素</span>

**div就是一个典型的`块级元素`**

	• 所谓块级元素就是会独占一行，无论他的内容由多少，都会独占一行
	• 如 p , h1~h6 
	• div标签没有任何意义，就是一个纯粹的块元素
	• div主要是用来页面布局的

**span标签是一个典型的内联元素（行内元素）**

所谓行内元素就是只是占据自身大小，不会占用一行
常见的内联元素


	• a , img .iframe,span
	• span没有任何语义，span专门用来选中文本/文字，然后为文字设置样式

>块元素主要用来页面布局
>
>内联元素用来选中文字设置样式
a 元素可以包含任意元素，除了它本身

###  **vertical-align**

**vertical-align  : 设置元素内容`垂直`方式**


	语法 :

	vertical-align : baseline | sub | super | top | text-top| middle | bottom | text-tottom  |长度|百分比

	如 
	vertical-align:middle


### 文字基线

![](https://img-blog.csdnimg.cn/2020020918551345.png)


![](https://img-blog.csdnimg.cn/20200209185519788.png)

>单行文字或者元素垂直居中，只需要把`line-height设置为父级元素高度就可以
`


**设置元素中文本行高**

	语法:

	line-height : 长度值 | 百分比


### 文字样式和其它样式

字体属性 | 说明
-- | --
word-spacing | 设置元素内单词间距
letter-spacing | 设置元素内字母间距
text-transform | 设置元素内文版大小写 `capitalize | uppercase | lowercase | none`
text-decoration | 设置元素内文本装饰 `underline | overline | line-through | blink |none`

## **text-decoration示例**

	文字下划线
	文字中中划线


<p class="fOne">我是下划线underline</p>

<p class="fTwo">我是中划线line-through</p>

<p class="fThree">我是上划线overline</p>

<p class="fFour">我是blink</p>

<p class="fFive">我啥也没有none</p>



# css继承和层叠

## 继承

简单来说，就是`子元素`可以`继承父元素`设置的样式或者**一些**属性

![](https://img-blog.csdnimg.cn/20200211212243893.png)

**继承的好处都有啥?**

	1) 父元素设置样式，子元素可以继承部分的属性
	2) 减少css代码

## 层叠

就是可以为一个标签元素定义多个样式

- 如果冲突的情况下
- 多个样式可以层叠为一个


```
<style>
		p {
			color:red;
		}
		p {
			font-size:14px;
		}
</style>
<body>
		<p>  我是p标签 <span>我是span标签</span>  </p>
</body>
```

效果

<p style="color:red;font-size:14px">  我是p标签 <span>我是span标签</span>  </p>

像这样的，设置的都是span，而且属性不冲突，一个是设置字体颜色，一个是字体大小,这样最后span的样式也就是应用了设置的样式

<span class="yellow">注意,并不是所有的属性都是可以继承的，比如像border这些元素 背景相关的的样式，，定位相关的样式不能继承</span>

由于css代码阅读顺序也是由上到下，所以同时设置同一个元素同一个样式，会应用写在后面的那一个

```c
<head>
	<style>
		h1{color:red;}
		h1{color:blue;}

	</style>
</head>

<body>
	<h1	 class="cssCover">我是css继承和层叠</h1>
</body>
```
测试效果


<h1 class="cssCover">我是css继承和层叠</h1>

会应用 `h1{
color:blue;
}`
的样式，也可以理解为，后面设置的样式把前面设置的样式`覆盖`了

# css盒子模型，布局

- [width属性](#width属性)
- [盒子模型简介](#盒子模型)
- [height属性](#height属性)
- [border边框属性](#border属性)
- [padding内边距属性](#padding填充属性)
- [margin外边距属性](#margin外边框属性)
- [display属性](#display属性)

## width属性

宽度: 

	width : 长度值 | 百分值 | auto

最大宽度:

	max-width: 长度值 | 百分比 | auto

最小宽度:

	min-width: 长度值 | 百分比 | auto

>宽度的表示方式有`三种` 长度 百分比 auto
>
>百分比的方式适用于`自适应布局`

## 盒子模型

![](https://img-blog.csdnimg.cn/20200209234331950.png)

![](https://img-blog.csdnimg.cn/20200209234354127.png)

![](https://img-blog.csdnimg.cn/20200209234401619.png)

![](https://img-blog.csdnimg.cn/20200209234410205.png)

## height属性

高度:

	height: 高度值 | 百分值 | auto

最大高度：

	max-height:高度值 | 百分值 | auto

最小高度:

	min-height:高度值 | 百分值 | auto

> 设置块级元素和替换元素的内容高度


**那些元素可以设置width和height**

块级元素

	<p> , <div> , <h1>~<h6> , <ul> ,<li>,<ol>
	<dl> , <dt> ,<dd>。。。。

替换元素

	浏览器根据标签的元素与属性判断显示内容
	<img> , <input> , <textarea>

## border属性

- 边框宽度 `border- width`
- 边框颜色 `border-color`
- 边框样式 `border-style`

**设置元素边框宽度**

	border-width : thin | medium | thick | 长度值

![](https://img-blog.csdnimg.cn/2020020923505688.png)

**border-style属性值**

值 | 描述
-- |--
none | 定义无边框，默认值
hidden | 与 "none" 同，除非在表格元素中解决冲突
dotted | 定义点状边框，大多浏览器显示实线
dashed | 定义虚线，大多浏览器显示实线
solid | 定义实线
double | ~双线
groove |定义3D凹槽边框
ridge | 定义3D垄状边框
insert | ~insert边框
outset | ~outset边框
inherit | 规定从父元素继承边框样式

**边框属性**

缩写:

	border: [宽度] [样式] [颜色]
	如
	border:4px solid red

不同方向:

	border-top : [宽度] [样式] [颜色]
	border-left: [宽度] [样式] [颜色]
	bordr-right: [宽度] [样式] [颜色]
	border-bottom: [宽度] [样式] [颜色]

## padding填充属性

![](https://img-blog.csdnimg.cn/20200209235259815.png?,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

**内边距属性**


设置元素的内容和边框之间的距离，4个方向

	- padding-top :长度 | 百分比
	- padding-right:长度 | 百分比
	- padding-bottom:长度 | 百分比
	- padding-left:长度 | 百分比

> 值不能为`负数`
> 
> 理解为内容不能超出框

**内边距的缩写**

![](https://img-blog.csdnimg.cn/20200209235642189.png?k,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

> 注意，如果是三个值，是 `上 左右 下`
>
> 如果是四个值，是`上 右 下 左`

## margin外边框属性

设置元素与元素之间的距离 ，4个方向

	- margin-top : 长度值 | 百分比 | auto
	- margin=right: 长度值 | 百分比 | auto
	- margin-bottom: 长度值 | 百分比 | auto 
	- margin-left: 长度值 | 百分比 | auto

> 值可以为`负数`
> 
> 理解为元素之间可以相交



<span class="yellow">和 padding类似但是要注意的是margin 值可以为负数</span>

默认情况，html`块级元素`存在外边距

	body , h1~h6 ,p ,div

声明margin属性，`取消(覆盖)默认的margin`外边矩样式

	body,h1,h2,h3,h4,h5,h6,p{
		margin:0;
	}

**外边距margin的缩写和padding一样**

![](https://img-blog.csdnimg.cn/20200209235932337.png)

> 注意，如果是三个值，是 `上 左右 下`
>
> 如果是四个值，是`上 右 下 左`

![](https://img-blog.csdnimg.cn/20200209235957154.png)


<>注意:<>

![](https://img-blog.csdnimg.cn/20200210000049792.png)

有时可能会出现设置样式之后发现没有效果，就有可能是这种原因
还要注意的是

![](https://img-blog.csdnimg.cn/2020021000165390.png?)

如果设置左右 为 `auto`，则左右边距将平分

![](https://img-blog.csdnimg.cn/20200210003013238.png)

![](https://img-blog.csdnimg.cn/20200210003013238.png?)

>也容易理解，水平容易充满，而垂直不易充满，`所以是左右水平方向`

## 盒子模型计算

**总的来说**

	盒子总的宽度 = 左右外边框 + 左右外边距 + 左右内边距
	盒子总的宽度 = 上下外边框 + 上下外边距 + 上下内边距


## display属性

**HTML元素分类**

块级元素，独占一行

	<p> , <div> , <h1>~<h6> ,<ul> ,<li> , <dl> ,<dt> ,<dd>。。。

行内元素(内联元素),一行显示

	<span> , <a> ,<em>...
 

**display属性**

	inline:
		元素将显示为内联元素，元素前后没有换行符
	block
		元素将显示为块级元素，元素前后带换行符
>可以理解为，行内，块级，独占一行和行内显示`由换行符决定`

	1．相应内联元素及使用display : inline设置成内联元素的元素
		width和height属性无效。
	水平方向: margin-left / margin-right / padding-left/ padding-right有效
	垂直方向: margin-top/margin-bottom/padding-top/ padding-bottom【无效】

	2. 块级元素及使用display : block设置成块级元素的
	元素width/height/margin/padding属性都生效

> 行内元素，宽高受限，垂直方向受限


也就是内联元素无法设置宽高
水平方向 margin和padding可以用，因为内联元素活动范围是在一行内
垂直方向不可以用

块级元素设置为block都可以用，因为本身就是这个属性


**inline-block :**

	行内元素呈现 inline，具有block相应特性

**none:**

	此元素不会显示

`inline-block就是活动范围在一行内（前后没有换行符），但是又能设置宽高`
`
使用display: none;` 的元素，会隐藏该元素，并且不占据页面空间

`visibility` 可以用来设置元素的隐藏和显示状态，
可选值

- visible 默认值 元素会默认在页面显示
- hidden 元素会隐藏不显示，但是和`display:none`不同的是，这样设置会占据页面位置


子元素默认存在父元素的内容区，理论子元素最大等于父元素内容区

如果超出父元素内容，就是溢出

父元素默认将溢出的内容，显示在父元素外

**通过`overflow`设置父元素溢出内容处理**

- **visible** 默认值 元素会默认在页面显示
- **hidden** 元素会隐藏不显示，但是和`display:none`不同的是，这样设置会占据页面位置
也就是这个位置依旧保持
- **scroll**: 父元素增减滚动条，无论是否溢出，都会显示滚动条
- **auto**: 根据需求添加滚动条
    - 需要水平就水平
    - 需要垂直就垂直
    - 都不要就不加


![](https://img-blog.csdnimg.cn/20200210005848765.png)

**overflow: hidden;**


![](https://user-gold-cdn.xitu.io/2020/7/27/1738f587c474c402?w=876&h=503&f=png&s=40893)

**overflow: scroll;**

![](https://user-gold-cdn.xitu.io/2020/7/27/1738f5f046e08b00?w=409&h=519&f=png&s=42281)

**overflow:auto:**

会根据需求加滑动块
注意这个块是`父元素`加上的

# css浮动介绍

**float中的4个参数**

	- float :left ---> 左浮动
	- float :right---> 右浮动
	- float :none---> 不浮动
	- float : inherit--->继承浮动


属性的`默认float属性是none`，也就是没有设置浮动
为了理解浮动，用例子表现较好

![](https://img-blog.csdnimg.cn/20200211214315785.png)

保持默认样式

![](https://img-blog.csdnimg.cn/20200211214542609.png)

设置`左浮动`，按照标签代码先后，从左到右

div本来是块级元素，应该是独占一行的，但是设置了浮动后，变成了内联元素的特性，占据在一行里面
这是为什么呢，好像设置了浮动可以改变元素原来的属性

**我们来试一试内联元素设置浮动会怎样?**

![](https://img-blog.csdnimg.cn/20200211215232310.png)

我们会发现原本只能在一行显示的，只能改变宽度的内联元素，设置浮动后似乎可以设置高度，然后还能独占一行，好像变成了块级元素
所以浮动可以改变元素原来的属性是无疑的

## 实现文字环绕

![](https://img-blog.csdnimg.cn/20200211220937721.png?)

当给div设置浮动时

![](https://img-blog.csdnimg.cn/20200211221155950.png)

出现了文字环绕图片的效果
这是因为
当元素没有设置浮动属性时，处于`标准文本流`，就是这个属性标签规定属性是什么就是什么，比如内联元素的属性就是不能独占一行，不能设置高度，块级元素就是独占一行，所以当设置了浮动后，这些原有的的属性就变了，就不在标准文本流当中
这里 当块级元素设置了浮动后，脱离正常文本流，但是占据原来的空间，本来时独占一行的，现在不是独占一行，这样在一行内，其它的文字就可以同占据一行，但是还是会占据原来的文本空间，所以造成文字环绕图片的效果，图片也是块级元素
浮动元素不会盖住文字，因为最初浮动就是为了解决文字环绕问题的

上述的效果解释就是 

> 设置浮动后，理解为`div` "浮动起来",所以文本往上 "挤" ，但是`div的位置还是占用`，所以呈现上述效果

## 浮动的原因及其副作用分析

**css中的定位机制**

	1. 标准流(普通流)
	2. 定位
	3. 浮动

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
<style>
.box1{
	width: 500px;
	border:1px solid #000;
	height: auto;
	background: rgb(189, 189, 219);
}
.test{
	width: 200px;
	height: 100px;
	background: rgb(30, 219, 163);
	border: 1px solid #fff;
}

</style>
</head>
<body>
	<div class="box1">
		<div class="test">1</div>
		<div class="test">2</div>
		<div class="test">3</div>
		<div class="test">4</div>

	</div>
</body>
</html>
```
![](https://user-gold-cdn.xitu.io/2020/7/27/1738f74eb33f0e68?w=746&h=616&f=png&s=15519)

**div设置浮动后**

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
<style>
.box1{
	width: auto;
	border:1px solid #000;
	height: auto;
	/* 父元素的高度设置auto，表示大小由子元素填充 */
	background: rgb(189, 189, 219);
	
}
.test{
	width: 200px;
	height: 100px;
	background: rgb(30, 219, 163);
	border: 1px solid #fff;
	/* 当设置浮动 */
	float: left;
}

</style>
</head>
<body>
	<div class="box1">
		<div class="test">1</div>
		<div class="test">2</div>
		<div class="test">3</div>
		<div class="test">4</div>
box1
	</div>
</body>
</html>
```

![](https://user-gold-cdn.xitu.io/2020/7/27/1738f7cf4c0e3164?w=991&h=150&f=png&s=8405)

块元素默认在`文档流`中垂直排列，所以三个div自上到下排列

如果浮动元素上边一个块元素，那么浮动元素不会超过块元素

浮动元素不会超过身边的兄弟元素，最多一边齐


```
<style>

.box1{
	width: 20px;
	height: 20px;
	background: #000;
}
.box2{
	width: 30px;
	height: 30px;
	background: rgb(178, 243, 162);
}
</style>
</head>
<body>
		
		<div class="box1">1</div>
  <div class="box2">2</div>

</body>
```

**此时，都不设置浮动，黑色块在绿色块上面**


![](https://user-gold-cdn.xitu.io/2020/7/27/1738f8f6fd835761?w=382&h=252&f=png&s=3195)

**如果黑色块设置浮动**
```
<style>

.box1{
	width: 20px;
	height: 20px;
	background: #000;
	float:left;
}
.box2{
	width: 30px;
	height: 30px;
	background: rgb(178, 243, 162);
}
</style>
</head>
<body>
		
		<div class="box1">1</div>
		<div class="box2">2</div>

</body>
```

![](https://user-gold-cdn.xitu.io/2020/7/27/1738f9110b5cf3e9?w=254&h=130&f=png&s=1614)

则 绿色块 "跑到"上面

此时黑色块像`悬浮`在绿色上空一样

	- 当元素设置浮动后，会完全脱离文档流
	- 块级元素设置浮动后，高度和宽度被其内容撑开，但是这样的话，会改变页面结构造成页面高度的塌陷问题，这个问题还是比较重要的

## 解决因浮动导致父元素塌陷问题

## BFC 

开启BFC，根据w3c标准，页面中有一个默认的属性叫` Block Formatting Context`，简称BFC ，默认是关闭的
当元素设置了BFC后，元素会以下特性

- 父元素`垂直外边距`不会和子元素重叠
- 开启BFC的元素不会被子元素覆盖
- 开启BFC的元素`可以包含浮动子元素`


**开启BFC**

	1) 设置元素浮动
	使用这种方法会导致父元素宽度缺失，也会导致上边元素上移，不能解决问问题

	2) 设置元素【绝对定位】
	3) 设置元素为inline-block。也会导致宽度丢失问题，不推荐使用
	4) ==推荐方式==
		将元素的overflow属性设置为非visible

		设置元素overflow属性为hidden，是副作用最小的开启BFC的方法

		开启BFC的方式有很多，ie中没有BFC，但是有中有一个hasLayout属性和BFC类似
		使用zoom属性，设置zoom:1
		也可以解决问题
		zoom表示放大的意思，后面的系数，表示元素放大的倍数
		1表示放大一倍，·zoom只能ie中使用

**浮动解决办法**

		- 手动给父元素添加高度
		- 通过clear清除内部，外部浮动
		- 给父元素添加overflow属性结合zoom:1
		- 给父元素添加浮动

<span class="yellow">给父元素手动添加高度的方法，用起来解决问题还是不彻底，可以使用，但是不会建议用</span>

**clear属性**

	- clear : none
	- clear :left
	- clear :right
	- clear : both


