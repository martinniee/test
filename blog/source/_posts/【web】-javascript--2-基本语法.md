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

# 基本语法
- ----
- --
- ----
注释
-    多行注释
   ```
   /*   */
   ```
   -  单行注释
   ```
   //
   ```
<!-- more    -->
   通过注释可以简单的调试代码
`   注意事项:`
  1. js中区分大小写
  2. js语句每条以分号结尾
 - 如果不写`分号`也可以，但是会加大系统资源，浏览器会帮你加，但是有可能加错位置，加对了还好说，加错了，就不容易找到了
 3. 可以使用`多个空格`和`多行换行`用于格式化结构，调整结构，便于读写

# 字面量和变量

-  ---
### `字面量：`
都是一些不变的量，如 1 2 3 4 5 ，12344，123456，553535
字面量可以直接使用，但是一般不会直接使用字面量
选择使用变量来替换  如  一个比较长的字面量 `126878394859494999`
这样记忆和再次书写都很麻烦，不如直接用一个变量转换
如  ` x  =   126878394859494999`;
那么此时，x 就代表这一长串数字，使用起来也很方便
- --

### `声明变量`:
js中用 `var`声明变量  如
```java
var   a; //声明的作用相当于上户口，没声明（没上户口）的变量，是黑户，会报错
a = 123; //给变量赋值 （把字面量123 转换给 变量 a）
```
`注意`;
声明和赋值可以同时进行
```java
var a = 123 ;
```
同一个变量只需要`声明`一次，后边再赋值可以不用带上`var` 
如
```java
var a = 123 ;
     a  = 456;
       a = 779;
重新声明一个变量 b  需要带上var 
var  b = 2222;
     b =3333;
```
- ---
### `标识符`
js 中所有的自主命名都可称为 `标识符`
如   变量名  ， 函数名 ， 属性名 
命名标识符规则
  1.由 字母， 数字，下划线（_），$组成
  2.标识符不能以数字开头
  
  如   ~~`var 1_223 =  2222;`~~
     
   3.标识符不能是关键字或保留字
   
   4.命名规范
- 首字母小写，每个单词开头大写，其余小写（`也就是驼峰命名法`）
- 命名规范不是必须的，但是最好遵守
        如
        
   ```java
         var  HeLloWoLd = 123;
    ```
        
        这样也可以，但是这样看起来，使用起来方便吗，不方便吧！！
        所以最好这样或者说一定要这样
        
  ```java
        var hellWorld = 123;
   ```
# `**数据类型**`          
数据类型就是字面量的类型
共6中
 1. String  （字符串）
 2. Number  (数值)
 3. Boolean  (布尔类型)
 4. Null   (空对象类型)
 5. Undefined  (未定义类型）
 6. Object  (对象类型) 

上述 除了 `Object是`属于`引用`类型，其他5种都是属于`基本数据`类型

###### `1-字符串String`

 js 字符串要用 `''   ''`,  或者`'  '`括起来
 如
 ```java
 var  str = " hello";
 console.log(str);
 相当于
  
      console.log("hello");
 或者
 var  str  = 'hello'
 console.log('hello');
 相当于
       console.log('hello');
 这两个都是一样的
 要分出两种的原因是为了区分，防止嵌套
 ```
 引号用单引号`''`或者`""`都行
 单引号，双引号 混合使用是为了区分
 如
 ```java
 str = "  我说： ‘今天天气真好！！’    "     ;
 ```
 上面双引号`""`代表整体是个字符串，上面单引号`''`是具体说话语境中代表的引用
 在字符串中使用`\`作为转义字符
 可以使用  `\ "` 代表 `"`
 如
 ```java
 str  =  " 我说：\'' 今天天气真好 \''   '';
 ```
 还有其他一些转义字符的使用 如
 `\"` 代表 `"`
 `\'` 代表 `'`
 `\n`代表 换行
 `\\` 代表 `\`
 同理 `\\\\`代表`\\`
` 注意：`
```java
alert(   " str "  )  ;  // 加双引号或单引号代表是字符串
alert(    str     )   ;  // 不加单独的代表变量
```
- --
###### `2-Number`
js中所有数值都是number类型
包括`整数`和浮点数（`小数`）
```java
var  a = 123;
       a = 345;
       a =789;
       后面的覆盖前面的值
       也就是
       console.log(a);
       会输出
       789
```
`注意区分：`
```java
var  a= 123;
      b=  " 123 " ;
      console.log(a);   一个代表数值类型的123
      console.log(b);   一个代表字符串类型的123
```
例子
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112140503267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

如何区分是`字符串`类型还是 `数值`类型呢     
使用 `typeof`检查变量类型
如
```java
console.log(typeof a )		;
console.log(typeof  b)      ;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112141027682.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

使用数值类型如果超过了最大值会返回一个 `infinity`,表示`正无穷`
同理 `-infinity` 表示`负无穷`
`NaN`表示`not a number`是一个特殊的数字
用typeof 检查也会返回 Number
例子
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112144015362.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
###### `3-布尔值（字面量）`
布尔值只有两个
`true` 或`false`

###### `4-Null`
专门表示空对象
typeof null 会返回一个 `object`
例子
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112144519438.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
###### `5-Undefined`
undefined类型只有一个值，就是`undefined`（未定义）
(当我们声明一个变量，但是不给他赋值，此时的这个变量的值就是		`undefined`)
如
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112144949932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
- ---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112145259498.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
# `强制类型转换`
主要是转换为 String , Number ,Boolean 

### a. *将其他数据类型转换为 `String`类型*
 
 **`方法一`**
 - 使用 `toString()`方法
   如
   ```java
    var  a =123 ; (数值类型)
      a.toString();      调用toString() 方法
   ```
`   注意：`
   >
   >toString()方法是不会影响到原变量，是有返回值的
   >因此要想真正改变类型，再嵌套一个变量
如
```java
       var  a =123;
       var b = a.toString();  //此时a还不是字符串类型，把b 变成字符串类型
       如果不想要两个变量可以改为
       a  =  a .toString() ;
```
例子
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112150627282.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
例子2
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112150804941.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
`注意：`      
`Null`和`undefined`没有toString() 方法，如果继续使用的话会报错
只能使用string(） 函数

 **`方法二`**
 调用`string() 函数`
 所以对于
  null ---> string
  如
```java
var a = null;
    a= string(a) ; 			
```
对于
undefined --->string
如
```java
var a = undefined;
    a = string(a) ;
```
这里的`a`表示参数 
==使用string () 函数==
对于 Number ,Boolean 还是用 `tostring() `方法
但是对于 null ,undefined，不会调用`tostring () `方法      
会将null --->  "null"
     将undefined ---> “undefined”
### b. *将其他数据类型转换为 `number`类型*
```java
var  a =123;

console.log(  typeof a  ) ;
console.log(  a  );
```
 **`方法一`**
 - 使用`number() `函数
  如
  ```java
  var  a ="123 " ;
       a= Number(a);
```
`注意：`Number() 函数 	`N要大写`

例子
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112152924901.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
对于 xxxx ---> 数字
有以下几种情况
 1. 如果是纯数字字符串，可以直接转化为数字
   
 2. 如果字符串中有非数字，则返回值为NaN
     ```java
     var a = "123_34";
     a = Number(a) ;
     console.log(  a  );
     console.log(   typeof  a );
     ```
     ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112153825216.png)
 3. 如果字符串中有空格，则返回值为NaN
 ```java
    var a= "123   456";
    a = Number(a);
    console.log( a );
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112154118314.png)
 4.  如果是布尔型类型转数字，则true为1，false 为0
 ```java
  var  a = true;
  a = Number( a );
  console.log(a );
  ```
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112154300373.png)
   ```java
  var  a = true;
  a = Number( a );
  console.log(a );
  ```
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112154333445.png)
 5. null类型转数字，为0
 ```java
 var  a = null;
 a = Number(a);
 console.log(a ) ;
 ```
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112154522148.png)
 
 6. undefined类型转数字，则为NaN    
  ```java
 var  a = undefined;
 a = Number(a);
 console.log(a ) ;
 ```
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112154621589.png)
 **`方法二`**（专门用来对付字符串）
 ```java
 var a = "123px";
     方法 ： parseInt  ( )  函数  //将字符串中有效整数读取出来
             parseFloat () 函数    //将 .. .. . 有效小数读取出来
  ```
  如
  ```java
   var  a ;
    a = " 123px";
    a = parseInt(a ) ;
    console.log( a ) ;
    console.log( typeof a );
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112155425249.png)

对于读取整数`parseInt() 函数`，有几种情况
   1. a = " 123px";
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112155425249.png)
   2. a = " a123px";        
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112155641761.png)
   3. a = " 123a456";
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112155740277.png)
  总结：
  - 若第一位是数字，则取 是数字的有效值
  - 若第一位不是数字，则为NaN

  对于读取整数`parsefloat() 函数`，有几种情况
    1.  a = "123.456";
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112160206555.png)
    2. a = "a123.456";
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020011216031035.png)
3. a = "123.456.789";
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112160358861.png)
总结
- 如果第一位是数字则取是小数部分的有效小数
- 如果第一位不是数字则取NaN

如果对于非字符串类型，使用parseInt() 或 parseFloat() 
会将其变为字符串String类型，再进行变换
如
```java
var a  = true;
a  = parseInt(a ) ; //相当于 a = parseInt ( "true");
console.log( a );
console.log( typeof a);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200112160908943.png)
`返回非数字类型`

   




      	
      



 
 

 

  

