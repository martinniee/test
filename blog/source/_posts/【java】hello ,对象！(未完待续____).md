---
title: java对象【updating】
tags: java基础
catagories: 
    - 笔记
    - java

archives: 
toc: true
top: 
cover: https://img-blog.csdnimg.cn/20200319011128478.png

---


# 创建对象
---
---
##      创建一个对象包括  `对象的声明` 和 为 对象 `分配变量` 两个步骤 
## 1.对象的声明  
  -  一般格式：
   ```
        类名        对象名字 ;
   ```
  例如 ：     
  ```javascript
Lader   lader ; //注意`L` 和`l`分别代表的含义,Lader 代表`类名`，lader代表`对象名`
  ```
 ## 2.为声明的对象分配变量 
使用`new` 运算符 和 `类`的构造方法 为 声明的对象 分配变量 即创建对象  （为什么给对象分配变量叫创建对象呢？前面有说到，对象是类的变量，类是 创造 对象的模板，当用对象 创造一个 实例（也就是给`对象实例变量`） ），如果此时类中没有`自定义构造方法，系统会调用`默认`的构造方法 ，默认的构造方法是`无参`的，且方法体中没有`语句` 
```javascript
# 人 // 是一个抽象的类，相当于模板，（我们肯定会举例子 ，举具体的例子，用来表现人
    //暂且认为，所有举的例子的集合称为作对象，但还是模糊的，但是又没有具体指明对吧，所以要具体指定一个例子）
```
```javascript
# 张三 // 就相当于 实例化 类 的 对象 （也就是之前的“所有举的例子”中的一个，肯定不止一个人吧，
     //也就是属于所有举的例子中的一个），因为这样的例子不止一个，可能是 李四，王五，因此这个名字就
    //是变化的，所以是变量，给对象分配变量，就是给一个具体的例子，那么这个“人”就清晰了
```
   也就是 对于` “创建对象” `这四个字来说，必须是创造一个具体的例子（创建实例化变量），才叫创建了对象
前面声明的意思，就是向程序说 ：“我先报告一声，我待会可能会用这个了类创建一个对象，但是具体创建不创建，看具体的业务需求
   ### 例子1
```javascript

//人物类

class XiyoujiRenwu {
      float height,weight;
      String head ,ear;

      void speak (String s) {
          System.out.prinln("s");
      }
}
//主类

public class Example {
     public static void main (String args[]) {
         XiyoujiRenwu  zhubajie;              //声明对象
         zhubajie = new  XiyoujiRenwu( ) ;    //为对象分配变量（使用new和默认构造方法）
     }
}
```
###  例子2
```javascript
class Point {
    int x,y;
    Point ( in a, in b) {
        x = a;
        y = b;
    }
}

//主类

public class Example {
     public static void main (String args[]) {
         Point p1, p2;             // 声明对象p1 和 p2
         p1 = new Poin(10,10);     //为对象p1分配变量（使用new 和类中的构造方法）
         p2 = new Poin(23，35);    //为对象p2分配变量（使用new 和类中的构造方法）
     }
}
```
 !!!注意：
 如果类中定义了`一个 `或者`多个`构造方法。则`不提供默认无参数`的构造方法，上述例子2提供了构造方法（相当于自定义的构造方法），因此下面的写法是非法的
 ```javascript
p1 = new Point ();    //为什么是非法的，这里想使用默认不带参数的构造方法，而已经提供了自定义的构造方法
                       // Point (int a ,in b)，则不提供不带参数的默认构造方法   
 ```
 ## 3.对象内存模型
 使用前面的例子来说明：
	`1) 声明对象时的内存模型`
	当用XiyoujiRenwu类声明一个变量zhubajie ，即对象zhubajie ，如例子中
	    XiyoujiRenwu  zhubjie;
	内存模型模型如图
	![ ](https://img-blog.csdnimg.cn/20200101114033756.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
    
	
声明对象变量zhubajie 后，zhubajie 内存里面还没有任何数据（没有实例化具体值嘛），称这个时候的zhubajie 是一个`空对象`，空对象不能使用，必须再进行`分配变量`操作，即为对象分配实体

`2)为对象分配变量后的内存模型`
new 运算符和构造方法进行运算时要做两件事，如，系统见到
new  XiyoujiRenwu ( ) ;
会做两件事:

 （1）找到 XiyoujiRenwu 这个类所在的位置，然后为 height,weight,head,ear各变量`分配内存`，即成员变量分配内存
再`执行构造方法`中的语句，如果成员变量声明时，没有赋初值，所使用的构造方法也没有对成员变量进行初始化和操作，那么
            - 对于整型的成员变量，默认初值为 0
            -  对于浮点型，默认初始值为 0.0，
            -  对于boolean型，默认为false 
            -  对于引用型，默认为 null
 
  (2) 当new 运算符给所有成员变量分配内存了之后，将`计算`出一个称为`引用`的值，（包含代表这些成员变量的内存位置，及相关重要信息），即表达式 new XiyoujiRenwu(  ) 是 一个值，如果把这个值/引用赋值给zhubajie；
zhubajie  =  new  XiyoujiRenwu (  ) ;
那么 Java 系统分配的height ,weight ,ear的内存单元将由zhubajie 操作管理，称这些变量属于zhubajie 实体，即这些变量属于zhubajie 的
`所谓创建对象，指的是，为对象分配一个变量，获得引用，确保这些变量可以被对象引用 ，确保这些变量可以被对象来操作管理
`
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020010111452225.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
对对象分配变量后，内存模型，由之前的变成上图，箭头表示  对象可以操控的属于它的变量
`3） 创建多个不同的对象`
一个类通过new 运算符可以创建多个不同的对象，这些对象的变量将被分配不同的空间
在的上述例子中可以再创建一个对象
```javascript
       sunwukong   =  new  XiyoujiRenwu( );
```
当`创建对象zhubajie` 时 ，成员变量height ,weight,head,ear,被分配内存空间，并返回一个引用值给zhubajie 
当`创建对象sunwukong`时，成员变量height ,weight,head,ear,被分配内存空间，并返回一个引用值给sunwukong
 
zhubajie变量占据内存空间和sunwukong占据的内存空间是`不同`的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200101114929872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_9,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200101114947864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_9,color_FFFFFF,t_70)
>注：对象的引用存在栈中，对象的实体（分配给对象的变量）存在堆中．不要求读者
熟悉栈和堆·栈(Stack）与堆（heap）都是Java用来在RAM中存放数据的地方．Java自动管理栈和堆，程序员不能直接地设置栈或堆，栈的优势是，存取速度比堆要快，
缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性．
堆的优势是，可以动态地分配内存
大小，生存期也不必事先告诉编译器，Java的垃圾收集器会自动收走这些不再使用的数据·
，但缺点是，由于要在运行时动态分配内存，存取速度较慢


# 使用对象
New  + 类的构造方法
等于一个引用值，这个值给对象用
（把类中的成员变量信息和方法提供给先建的对象）
使用对象
抽象的目的是产生类，
而类的目的是创建具有属性和行为的对象
对象不仅可以操作自己的变量改变状态，也可以调用类中的方法产生一定的行为
 
使用运算符  "` . ` " ,对象可以实现对自己`变量的访问`和`方法的调用`（说话有主语）
比如：
```javascript
              我  要   吃饭
```
 
就是   
```javascript
             我        .    吃饭 
             对象      .     方法
```
注意：常见的是`调用方法`，其实也可以`访问变量`的
 
##### 1.对象操作自己的变量（体现对象的属性）
访问格式
```javascript
             对象   .  变量；
 ```
##### 2.对象调用类中的方法（体现对象的行为）
调用格式
```javascript
              对象  .  方法
```
##### 3.体现封装
  当对象调用方法，方法中出现的`成员变量`指的是分配给`该对象`的变量，在讲类的时候，说过，类中的方法可以操作成员变量，对象调用方法
下面的例子：
其中主类main 方法中使用XiyoujiRenwu创建`两个对象` zhubajie,sunwukong 
```javascript

 class   XiyoujiRenwu {
      float height ,weight;
      String head,ear  ;
      void  speak ( String s )  {
            head =  "歪着头 " ;
            System.out.println(s)  ;
      }
 }
 
 public  class  Example  {
    public  static void  main ( String args[] ) {
        XiyoujiRenwu zhubajie ,sunwukong  ;  //声明两个对象
        zhubajie   =   new  XiyoujiRenwu  (  )  ;   // 为对象分配变量
        sunwukong   =   new  XiyoujiRenwu  (  )  ;   // 为对象分配变量
             //到这一步才算创建好了对象
      zhubajie . height =  1.80f;     //对象给自己的变量赋值
      zhubajie .head=  "大头";   
      zhubajie . ear=  "一双大耳朵";   

    
      sunwukong . height =  1.62f;     //对象给自己的变量赋值
      sunwukong . weight =  1000f;   
      sunwukong .head=  "秀发飘飘";   

      System.out.println("zhubajie 的 身高"  + zhubajie.height);
      System.out.println("zhubajie 的 头"  + zhubajie.head);
	  System.out.println("sunwukong的 重量"  + sunwukong . weight);
      System.out.println("sunwukong 的 头"  + sunwukong .head);

      zhubajie.speak   ("俺老猪想娶媳妇"); //zhubajie对象调用方法
      System.out.println("zhubajie 的 现在的头"  + zhubajie.head);
      sunwukong.speak   ("老孙我1000斤重，我想骗猪八戒背我");  //zhubajie对象调用方法
      System.out.println("sunwugong 的 现在的头"  + sunwukong.head);
    }
    
}
 ```
 显示效果
 ![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMjAvMDEvMDEvRVdlQkxNdE5LNGdIVmJ6LnBuZw?x-oss-process=image/format,png)
>实际上呢，`new  XiyoujiRenwu （ ) `已经是引用值了，只不过没有赋给一个具体的对象，暂且叫new XiyoujiRenwu 为匿名对象，匿名对象当然可以用`.`来访问自己的变量
>但需要`注意`的是 ；
>下面是两个不同的匿名对象访问自己的变量weight(一个将weight值改为`100`,一个将weight值改为`200`)
```javascript
new XiyoujiRenwu ().weight = 100;
new XiyoujiRenwu ().weigth = 200;
```
>而下面的对象shaSeng是在访问自己的weigth变量,即修改自己的weight，将weight由`100`修改为`200`  :
```javascript
XiyoujiRenwu  shaSeng  =new  XiyoujiRenwu() ; 
shaSeng.weight  =100; //先将自己的weight变量改为 100
sheSeng.weight  =200; //再将自己的weight变量改为 200
//自始至终都是再一个内存空间修改
```
在内存存储示意图：
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMjAvMDEvMDEvYTJSbDNBU1E3SWRQdG5wLnBuZw?x-oss-process=image/format,png =300x300)
`写程序应该尽量避免使用匿名对象去访问自己的变量，以免引起混乱`
 
 # 对象的引用与实体 
` 1. 避免使用空对象`
- 空对象不能使用
    即不能使用空对象取调用方法产生行为。
    假如程序中使用了空对象，程序运行的时候会出现这样的问题
    提示：`NullPointException`
    
    由于对象可以`动态的`分配实体，所以java编译器不对对象做检查的，所以写程序时要注意
    
`2. 重要结论`
 
- 一个类声明的两个对象如果由相同的引用值，那么这两个对象具有完全相同的变量（）

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMjAvMDEvMDEvZ25Nek5XSTFUWWVRakRzLnBuZw?x-oss-process=image/format,png =550x250)
在java中，对于同一个类的；两个对象`zhubajie` 和` shaSeng`，允许如下赋值
```javascript
      zhubajie = shaSeng;
```
这样的话，`zhubajie` 里面存放的时`shaSeng`的值，即`shaSeng`的引用，因此`zhubajie` 拥有的变量和`shaSeng`一样了
就是`zhubajie 是 shaSeng` ，`shaSeng也是shaSeng`
如图;

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMjAvMDEvMDEvWHNwRmZpVkRTdlpRUFJHLnBuZw?x-oss-process=image/format,png =550x250)
`3. 垃圾收集机制`
- 一个类声明的`两个对象`如果具有`相同`的引用，那么这两个对象具有完全相同的`实体`
- 所谓`垃圾回收机制`，就是机制周期的检测某个`实体`已经`不被任何`对象使用，如果发现这样的实体，就是释放实体占有的内存

```javascript
      Point p1 = new Point (15,5) ;
      Point p2  =new  Point (30,23) ;
```
内存模如图所示：

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMjAvMDEvMDEvdkkza3dPVkhjR05YdTRpLnBuZw?x-oss-process=image/format,png =550x250)
如果在程序中使用如下赋值语句
```javav script
           p1  = p2;
```
即把 p2 的引用给了 p1 ，因此` p1 ,p2 `的 引用一样了，虽然源程序中p1,p2是两个名字，但是在程序系统看来，都是 `0x999`,系统将`取消`l之前分配给p1 的变量（如果这些变量没有被其他对象使用的话）

#### 例子
```javascript
class Point {
     int  x,y;
     void setXY(int  m,int  n) {
        x = m;
        y  =n;
     }
}

public class Exam {
  public static void main(String args[])  {
       Point p1, p2;
       p1 = new Point () ;   //把相关变量的引用给 p1
       p2 =new Point () ;    //把相关变量的引用给 p2
       System.out.println("p1的引用是 ："+p1);
       System.out.println("p2的引用是 ："+p2);
       p1.setXY(1111,2222);
       p2.setXY(-100,-200);
       System.out.println("p1的x,y坐标"+p1.x+","+p1.y;
       System.out.println("p2的x,y坐标 ："p2.x+","+p2.y);
       p1  = p2 ;
       System.out.println("将p2的引用赋给p1后"+p1);
       System.out.println("p1的引用 ："+p1);
       System.out.println("p2的引用 ："+p2);
       System.out.println("p1的x,y坐标"+p1.x+","+p1.y;
       System.out.println("p2的x,y坐标 ："p2.x+","+p2.y);

    }
}      
```
运行效果

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMjAvMDEvMDMvWENSYjREb1FOdkFzSXA1LnBuZw?x-oss-process=image/format,png =550x250)
p1的坐标和引用都变成了p2的坐标和引用，说明他们现在是一样的变量

# 类与程序的基本结构
一个java应用程序(也称一个工程)，有若干个类组成，这些类可以在一个源文件里头，也可以分布在其它源文件里头
如图：

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMjAvMDEvMDMvb2JLNDNFdXRYZXIyNkdKLnBuZw?x-oss-process=image/format,png =550x250)         


  - 将程序所需要的源文件保存放在同一个目录里面，不同的源文件分别通过编译，生成java程序所需要的字节码文件
  - 运行主类
  使用解释器`(java.exe)` 运行一个java应用程序时，java虚拟机将java应用程序所需要的`字节码`文件加载到内存中，然后由`java虚拟机`解释执行,，因此，可以`单独`
j执行java程序所需要的其他源文件，并将所得的字节码文件和主类字节码文件放在同一个目录，`又一种方式`，就是`先写好`java应用程序需要的源文件，包括含有主类的源文件，放在同一个目录下，`先编译`含有`主类`的源文件，这样java系统自动编译主类需要的`其他`源文件

java程序以类为“`基本单位`”，一个java程序由若干个类组成，一个java程序可以将要使用的类单独放在不同的源文件里面，也可以将使用的类放在一个源文件里面，一个源文件的类可以被多个java程序使用，这样将了`类合理的分隔开`，有利于维护，
当需要修改某个类的时候，只需要找到所在的源文件，进行修改，而不需要修改其他类的源文件，利于维护。从软件设计角度，java语言中的类是可`复用的`，编写一定可复用的代码是软件设计需要考虑到的

#### 例子
下面例子，由三个源文件组成，每个源文件包含一个类（需要打开三个记事本分别编辑，保存）
`Rect.java`
```javascript
     public class Rect {
        double width;//矩形宽
        double height;//矩形高
        double getArea() {
          double area = width*height;
          return area;
        }
    }
```
`Lader.java`
```javascript
     public class Lader {
        double above;    //梯形上底
        double bottom;   //梯形下底
        double height;   //梯形高
        double getArea() {
          return (above+bottom)*height/2;
        }
    }
```
`Exam.java`
```javascript
     public class Lader {
       public static void main String args[]) {
                 Rect rectangle = new Rect();
                 rectangle.width = 109.87;
                 tectangle.height = 25.18;
                 double area = tectanglr.getArea() ;
                 System.out.println("矩形的面积 ："+area);
                 Lader lader = new Lader() ;
                 lader.above = 10.798;
                 lader.bottom = 156.65;
                 lader.height = 18.12;
                 area = Lader.getArea() ;
                 System.out.println("梯形的面积 ："+area);
                  
     }
}
```
假设上述三个源文件保存在`C:\test`中，在命令行窗口中输入这个路径，`编译`Exam源文件
```javascript
C:\test>  javac Exam.java
```
编译过程中，java系统会自动编译Lader.java和Rect.java，因为应用程序要使用到他们编译生成的字节码文件，编译完成之后，在`C:\test`目录下面会看到`3`个字节码文件,分别是`Exam.class`,`Lader.class`,`Rect.class`

>Lader和Rect就是`可复用代码`，应用程序主类只需要知道让他们计算面积就可以，而不必知道其中的算法

# 参数传值
方法中最重要的部分之一就是方法的`参数`，参数属于`局部变量`，当对象调用方法时，会给参数开辟内存空间，并要求向参数传递值，
方法调用时，`参数变量必须有具体的值`

### 传值机制
 
java中所有的参数都是`“传值“`的，方法中参数的值实际上是`调用者指定值`的一种`拷贝`
如：
向一个`int  型 参数x` 传递一个` int 类型的值`，比如是10，那么这个10其实是调用者传递值的一个`拷贝`，因此改变`参数的值`不会影响`向参数传递值`的那个`值`，如果把参数值由10改为20，那么调用者向参数传递的那个值还是10
就如同生活中`原件`和`复印件`的关系，复印件的改变不会改变原件
一般传递参数值的值的调用者和参数`不在一个类`里面

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMjAvMDEvMDMveW54TkJKMW1zNGdEd1FvLnBuZw?x-oss-process=image/format,png)
### 基本数据类型参数的传值
对于`基本数据类型`的参数，向该`参数传递的值`的级别`不能高于`该`参数`的级别
如：
不可以向int 类型参数传递一个float类型的值，但可以向double 类型的参数传递一个float 型的值
也就是要明白 `传值的一方` 和 `接收值  的参数`一方

#### 例子
`Exam.java`
```javascript
      class Computer{
          int add(int x, int y){
              return x + y;
          }
     }
     //***********************************************
     public class Exam {
        public static void main(String args[]) {
           Computer com = new Computer();
           int  m =100;
           int  n =200;
           int  result = com.add(m,n); //将参数 m,n分别传递给x,y
           System.out.println(result);
           result  =com.add(120+m,n*10+8) ;  //将参数120+m,n*10+8,分别传递给x,y
           System.out.println(result);
        }
 }
```



`注意:`这里的`Exam.java`类是把主类和其他类放在一个源文件里头

运行效果：

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMjAvMDEvMDMvUnBIU1pMc3lsNE9vRHVXLnBuZw?x-oss-process=image/format,png)
```javascript
  result  =com.add(120+m,n*10+8) ; 
```
//这个地方的意思是  com 对象调用属于它的add方法，然后再返回以一个int 类型的值，可以理解为，这里调用add方法返回一个int 类型的值 只不过属于com对象的，从实际功能上面可以省略com（只不过要表明这个方法被谁使用而已），所以这个地方不能按语法翻译为 `com调用自己的add方法返回一个值`，而是调用`com.add`方法返回一个值com.add是一个整体，重在于这里是个·方法返回一个值 ·（心里面出现的语句要想到这里是一个方法）要不然当结构变多时，容易乱,比如：
```javascript
    result = com.user.jack.add() //重点在于add() 的返回值
```
`但是语法上，真正写代码不可以省略`

### 引用类型参数传值


