---
tags: 
	- web
categories: 
	- 笔记
	- 前端
date: 2020-2-21 22:19:00
toc: true
cover: https://img-blog.csdnimg.cn/20200309214611617.png
---


javascript最初用来网页前端验证 
`ECMAscript`和`javascript标准` 本质是一样的

##### javascript常用输出语句
1.
```javascript
alert("     ")   //控制浏览器弹出警告框
```
<!-- more -->
例子
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112105212176.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
2.
```javacsript
 document.write("     ")    //向body内添加内容，输出内容  
```
例子
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112105451345.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
3.
```javascript
 console.log("    ")  //向控制台输出内容
```
例子

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112110443981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112110223195.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112110324783.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 )
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020011211034871.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70 ) 

## 1-js 编写位置
#### `方式1 `
可以将js代码写到标签的`onclick`属性中，这一部分有关内容和css很像哈，相当于css中把样式写到`行内元素`,同样的道理，这样写具有耦合性，不利于后期修改，`不建议`大量这样写

例子：![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112111253247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112111434574.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
#### `方式2`
可以将代码写到`href`属性中
```javascript
<a href=" javascript  :  语句   "  > </a>
如
<a href=" javascript  :  alert('你好');  "> </a>
<a href=" javascript  :  document.write('学习使我快乐') "></a>
<a href=" javacsript  :  console.log('我是日志控制台')"</a>

```
`注意:`这里引号的使用，后面会再讲到
例子：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112112221923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112112228250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
#### `方式3`
写到`script`内部标签中,如同css中写到`内部样式`一样
那么这个内部样式和外部链接的优先级也是由两者引入先后关系决定的
例子：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112121331389.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112121340739.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
`互换位置后`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112121615341.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)


- ---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112121843754.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
#### `方式4`
写到`外部文件`里头
将代码写到一个专门的后缀名为 `.js`的script文件里面，就像css样式的时候，专门放到一个后缀为`.css`文件里面一样，好处也差不多,方便维护，修改
```javascript
<script>
 
</script>
```
js代码是按照由`上到下`的顺序读写的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112122247151.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112122334333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
`互换位置后`
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020011212245059.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112122533468.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
##### `读者最好自行练习各种情况都写一遍`

#### `小结`
## javascript基本`输出`语句

 - alert("    ");
 - document,write("     ");
 - console.log("   ");
 
 ## javascript编写位置
 - 1--onclick标签内部
 ```
 <button onclick = " alert("我是警告提示框");"></button>
 ```
 - 2--href标签链接 
 ```
 <a href=" javascript :  alert(" 我是警告提示");">/<a>
 ```
 - 3--`script`标签内，相当于内部js
 ```
 <script>
       alert("我是提示警告框");
</script>
```
- 4--js外部文件
`这是一个index.js`文件
```
alert("我是警告框");
document.write("我是内容输出");
console.log("我是控制台日志");
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112122732486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020011212273486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

