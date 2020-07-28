---
title: (1)"编程实战-python"
tags:
  - python
categories:
  - 笔记
  - python
toc: true
date: 2020-06-02 13:12:40
top:
cover:
---

#  **调用百度API ---小小翻译器**
## 小小翻译器


## 程序设计思路
[百度翻译申请](http://api.fanyi.baidu.com/api/trans/product/index)

[详细文档说明](http://api.fanyi.baidu.com/api/trans/product/apidoc)

按照百度翻译文档要求，当请求`URL`网页成功后，提交获取`json`数据，再将json数据翻译即可


## 关键技术
### **urllib 库简介  url lib**

`python`标准库，网页访问模块，像用户访问本地文件一样读取网页内容


URL模块 | 介绍 
-- | --
`urllib.request` | 用于打开/读取URL
`urllib.error` | 包含请求的错误，可以try异常处理
`urllib.parse` | 包含解析url方法
`urllib.robotparser` | 用来解析`robots.text` 文本文件，提供单独的`RobotFileParser`类，通过该类的`can_fetch()`测试爬虫是否可以下载一个页面

### urllib库基本使用

用`urllib.request` 和`urllib.pase` 模块说明

**1. 获取网页信息**

    urlopen(url[,data[,proxies]]) 

关键字参数 | 介绍
-- | --
`urlopen()` | 返回`response`对象，可以操控这个对象远程获取网页数据
`url` | 远程数据的路径，一般是网址
data | 表示以`post`方式提交到url的数据,提交数据包括`post`/`get`
proxies | 设置代理

Response对象方法 | 描述
-- | -- 
read()/readline()/readlines()/fileno()/close() | 和文件对象和使用一样
info() | 返回`httplib.HTTPMessage`对象，表远程服务区返回`头信息`
getcode() | `HTTP`状态码，200表示请求成功，404表示网址未找到
geturl() | 返回请求的url

**如何查看网页的编码方式**

- 1.浏览器查看源码
- 找到`head`标签的`chareset`，即可查看

<span style="background:yellow">值得注意的是 </span>

    openurl(url)  # url不一定是要字符串网址参数，还可以是Response对象，因此可以这样

    response = urllib.request("https://www.baidu.com")
    res = urlopen(response)
    html = res.read() # 读取内容
    html = hmtl.decode('utf-8') # 解码得以查看
    print(html)  # 打印内容


**2. 获取服务器响应**

**3 .向服务器发送数据**

**4. 使用User-agent隐藏身份**


## 正式的程序设计
### 设计界面
### 使用百度翻译API

使用百度翻译需要向[翻译请求服务](http://api.fanyi.baidu.com/api/trans/vip/translate)请求get/post参数，获取服务

请求参数 | 类型 | 必填参数 | 描述| 备注
-- |-- |-- |-- |--
q | TEXT |Y | 请求翻译query | UTF-8编码
from | TEXT | Y | 翻译的源语言 | 语言列表（可设为auto）
to | TEXT | Y | 目标翻译语言 |  语言列表（可设为auto）
appid | INT | Y | AAPID | 可在管理控制台查看
salt | INT |  Y | 随机数 | 
sign | TEXT | Y| 签名 | appid + q + salt + 密钥的md5值

为安全 `sign`是用`md5`算法生成的字符串，长度`32`位，小写英文字符，单词请求长度控制6000字节内(中文2000个)

**签名生成的方法**

1. 请求appid,query(utf-8编码)，随机数salt，密钥，按照`appid+q+salt+密钥`拼接的字符串1
2. 对字符串1做`md5`，得`32`位sign


<span style="background:yellow">注意</span>

1. 先将文本转utf-8编码
2. 发送`HTTP`请求之前，对字段做`URL encode`
3. 生成签名拼字符串，q无需 `URL encode` ，生成签名之后，发送`HTTP`前，需要对`翻译文本q`做`URL encode`

**【例子】**

如，将apple翻译位中文

    q = apple
    from = en
    to = zh
    appid = 2015063000000001
    salt = 1435660288
    平台密钥 : 12345678

    生成签名sign ： 
    (1) 拼接字符串1
    appid = 2015063000000001 + apple + 1435660288 + 12345678
    得到字符串1 ： 2015063000000001apple143566028812345678
    (2) 计算得到签名sign : 
    # 在计算md之前，字符串1必须为utf-8编码
    sign = md5(2015063000000001apple143566028812345678)
    sign = f89f9594663708c605f3d736d01d2d4

通过`python`的`hashlib`的`hashlib.md5()` 可实现签名计算


如下 

    import hashlib
    m = '2015063000000001apple143566028812345678'
    m = m.encode() # 在hashing之前，文本要decode()
    m_MD5 = hashlib.md5(m)
    sign=m_MD5.hexdigest()
    print('m=',m)  # 原
    print("sign=",sign) # 现

    # 打印
    m= b'2015063000000001apple143566028812345678'
    sign = f89f9594663708c1605f3d736d01d2d4

得到签名后，请求`URL`
完整请求:

`http://api.fanyi.baidu.com/api/trans/vip/translate?q=apple&from=en&to=zh&appid=2015063000000001&salt=1435660288&sign=f89f9594663708c1605f3d736d01d2d4`


**【例子-采用urllib.request.urlopen()的`data`参数向服务器发送参数】**

```py
from tkinter import *
from urllib import request
from urllib import parse
import json
import hashlib
#  ------------------------

# 主要是完成翻译功能，传入源文本，返回目标文本(中文)
def translate_Word(): 
    # 百度翻译api URL
    URL = 'http://api.fanyi.baidu.com/api/trans/vip/translate'
    # 请输入原文本
    en_str = input("请输入要翻译的内容")
    # 创建字典，用于向服务发送data
    # Form_Data=['from':'en','to':'zh','q':'en_str',"appid":'2015063000000001','salt':'1435660288']
    Form_data = {}       #定义空字典
    Form_data['from'] = 'en'
    Form_data['to'] = 'zh'
    Form_Data['q'] = en_str  #要翻译的文字
    Form_Data['appid'] = '2015063000000001'  # 申请APP ID 
    Form_Data['salt'] = '1435660288'    
    Key = "12345678"  # 平台分配的密钥
    # 拼接
    m = Form_Data['appid'] + en_str + Form_Data['salt'] + Key
    m_MD5 = hashlib.md5(m.encode("utf-8"))  # hashing之前转utf-8
    # 得sign
    Form_Data['sign'] = m_MD5.hexdigest()  # 用这一步才得sign
    # 得 data
    data = parse.urlencode(Form_Data).encode('utf-8')  # 使用urlencode()转为标准格式
    # 请求
    response = request.urlopen(URl,data)
    html = response.read().decode('utf-8')  # 读取信息并解码
    translate_results = json.load(html)  # 使用json,得到Json数据
    # 打印json数据
    print(translate_results)
    # 翻译
    translate_results['trans_result'][0]['dist'] # 找到翻译的结果
    print("翻译结果是%s"% translate_results)
    # 返回翻译结果
    return translate_results
# 翻译按钮事件
def leftClick(event):
    en_str=Entry1.get()         # 文本域.get()可以获取内容
    print(en_str)
    vTEXT = translate_Word(en_str)  # 调用函数，翻译
    Entry2.config(Entry2,text=xTEXT)  # 修改文本框的内容为翻译结果
    s.set("")
    Entry2.insert(0,vTEXT)  
# 清空按钮事件
def leftClick2(event):
    s.set("")
    Entry2.insert(0,"")  


# 打印结果

请输入要翻译的内容: I am a teacher
翻译的结果是: 我是老师

# 此时得到的json数据

{'from':'en','to':'zh','trans_result':[{'dst':'我是老师。','src':'I am a teacher'}]}

```

`说明`

    insert ( index, s )

    # 向文本框中插入值，index：插入位置，s：插入值

     Entry2.config(Entry2,text=xTEXT)  # 修改文本框的内容为翻译结果
     # label.config(label,text="") # 用于修改文本内容


[json-菜鸟教程](https://www.runoob.com/json/json-tutorial.html)

[json-百度百科](https://baike.baidu.com/item/JSON/2462549)


**源代码(`可用`)**

```python
from tkinter import *
from urllib import request
from urllib import parse
import json
import hashlib
import random


def translate_Word(en_str):
    URL = 'http://api.fanyi.baidu.com/api/trans/vip/translate'
    # en_str=input("请输入要翻译的内容:")
    # 创建Form_Data字典，存储向服务器发送的Data
    # Form_Data={'from':'en','to':'zh','query':en_str,'transtype':'hash'}
    Form_Data = {}
    Form_Data['from'] = 'en'
    Form_Data['to'] = 'zh'
    Form_Data['q'] = en_str  # 要翻译数据
    # Form_Data['transtype'] = 'hash'
    Form_Data['appid'] = '20151113000005349'  # 申请的APP ID
    Form_Data['salt'] = str(random.randint(32768, 65536))  # 随机数
    Key = "osubCEzlGjzvw8qdQc41"  # 平台分配的密钥
    m = Form_Data['appid'] + en_str + Form_Data['salt'] + Key
    m_MD5 = hashlib.md5(m.encode('utf8'))
    Form_Data['sign'] = m_MD5.hexdigest()

    data = parse.urlencode(Form_Data).encode('utf-8')  # 使用urlencode方法转换标准格式
    response = request.urlopen(URL, data)  # 传递Request对象和转换完格式的数据
    html = response.read().decode('utf-8')  # 读取信息并解码
    translate_results = json.loads(html)  # 使用JSON
    print(translate_results)  # 打印出JSON数据
    translate_results = translate_results['trans_result'][0]['dst']  # 找到翻译结果
    print("翻译的结果是：%s" % translate_results)  # 打印翻译信息
    return translate_results


def leftClick(event):  # 翻译按钮事件函数
    # print( "x轴坐标:", event.x)
    # print( "y轴坐标:", event.y)
    en_str = Entry1.get()  # 获取要翻译的内容
    print(en_str)
    vText = translate_Word(en_str)
    # Entry2.config(Entry2,text=vText)   		        #修改提示标签文字
    s.set("")
    Entry2.insert(0, vText)


def leftClick2(event):  # 清空按钮事件函数
    s.set("")
    # Entry2.config(Entry2,text=vText)   		        #修改提示标签文字
    Entry2.insert(0, "")


if __name__ == "__main__":
    root = Tk()
    root.title("单词翻译器")
    root['width'] = 250;
    root['height'] = 130
    Label(root, text='输入要翻译的内容：', width=15).place(x=1, y=1)  # 绝对坐标（1，1）
    Entry1 = Entry(root, width=20)
    Entry1.place(x=110, y=1)  # 绝对坐标（110，1）
    Label(root, text='翻译的结果：', width=18).place(x=1, y=20)  # 绝对坐标（1，20）
    s = StringVar()  # 一个StringVar()对象
    s.set("")
    Entry2 = Entry(root, width=20, textvariable=s)
    Entry2.place(x=110, y=20)  # 绝对坐标（110，20）

    Button1 = Button(root, text='翻译', width=8)
    Button1.place(x=40, y=80)  # 绝对坐标（40，80）
    Button2 = Button(root, text='清空', width=8)
    Button2.place(x=110, y=80)  # 绝对坐标（110，80）
    # 给Label绑定鼠标监听事件
    Button1.bind("<Button-1>", leftClick)  # 翻译按钮
    Button2.bind("<Button-1>", leftClick2)  # 清空按钮
    root.mainloop()

```

# **爬虫应用---校园网搜索引擎**


## 校园网引擎功能分析

## 设计
校园搜索引擎步骤

1. 爬虫爬网站，得全网页链接

爬虫是一个嗅着url(链接)之虫,遍千山万水之网页，获取有益内容

**虫子前进方式**
- 广度优先搜索(BFS): 一圈一圈往内
- 深度优先搜索(DFS): 先进完一个，再进另一个

2. 得网页源代码，解析要想内容(新闻，标题，作者信息)
3. 把内容做成`词条索引`，一般采用倒排表索引
   - 正排表索引 :先逛文档，再内容(从文档中找相关内容)`效率低`
   - 倒排表索引 :先相关内容，再锁定文档(直接找到相关内容，再锁定文档) `效率高`

>如，要想找名字为`张三`的人，正排表就是，先找有人的房子，再从这个房子中找`张三`的人，有，则把这个房子标记

>倒排表，就是先把所有`张三`的人找出来,每个张三对应不同的房间，这样就可以把所有有`张三`的人的房间找出来

>最终目的，为了找出有`张三`人的房间

4. 搜索，按搜索词在词条索引中查询
   

**本系统由4个模块组成**

- 信息采集模块:用爬虫实现队校园网的信息获取
- 索引模块:对爬取后的信息如，新闻标题，内容，和作者分词，建立`倒排词表`
- 网页排名模块:TF/IDF是一种统计方法，用于评估一个词语对于一个文件的重要程度
- 用户搜索界面模块；负责关键字的输入以及搜索结果的返回






## 关键技术

### 中文分词
### 安装和使用jieba
### 为jieba 添加自定义词典
### 文本分类关键字提取
### deque


## 程序设计步骤

### 爬取代码 ( search_engine_build-2)

```python
import sys
from collections import deque
import urllib
from urllib import request
import re
from bs4 import BeautifulSoup
import lxml
import sqlite3
import jieba

##safelock=input('你确定要重新构建约5000篇文档的词库吗？(y/n)')
##if safelock!='y':
##    sys.exit('终止。')

url='http://www.zut.edu.cn/index/xwdt.htm'  #'http://www.zut.edu.cn'#入口

unvisited=deque()#待爬取链接的列表，使用广度优先搜索
visited=set()    #已访问的链接集合
unvisited.append(url)
#unvisited.append('http://www.zut.edu.cn/index/xwdt.htm')
conn=sqlite3.connect('viewsdu.db')
c=conn.cursor()
#在create table之前先drop table是因为我之前测试的时候已经建过table了，所以再次运行代码的时候得把旧的table删了重新建
#c.execute('drop table doc')
#c.execute('create table doc (id int primary key,link text)')
#c.execute('drop table word')
#c.execute('create table word (term varchar(25) primary key,list text)')
conn.commit()
conn.close()

print('***************开始！***************************************************')
cnt=0
print('开始。。。。。 ' )
while unvisited:
    url=unvisited.popleft()
    visited.add(url)
    cnt+=1
    print('开始抓取第',cnt,'个链接：',url)

    #爬取网页内容
    try:
        response=request.urlopen(url)
        content=response.read().decode('utf-8')
        
    except:
        continue

    #寻找下一个可爬的链接，因为搜索范围是网站内，所以对链接有格式要求，这个格式要求根据具体情况而定

    #解析网页内容,可能有几种情况,这个也是根据这个网站网页的具体情况写的
    soup=BeautifulSoup(content,'lxml')
    all_a=soup.find_all('a',{'class':"c67214"})   #本页面所有的新闻链接<a>
    for a in all_a:
        #print(a.attrs['href'])
        x=a.attrs['href']           #网址
        if re.match(r'http.+',x):   #排除是http开头，而不是http://www.zut.edu.cn网址
            if not re.match(r'http\:\/\/www\.zut\.edu\.cn\/.+',x):
                continue
        if re.match(r'\/info\/.+',x):       #"/info/1046/20314.htm"
            x='http://www.zut.edu.cn'+x
        elif re.match(r'info/.+',x) :       #"info/1046/20314.htm"
            x='http://www.zut.edu.cn/'+x 
        elif re.match(r'\.\.\/info/.+',x):  #"../info/1046/20314.htm" 
            x='http://www.zut.edu.cn'+x[2:]
        elif re.match(r'\.\.\/\.\.\/info/.+',x):  #"../../info/1046/20314.htm" 
            x='http://www.zut.edu.cn'+x[5:]
        #print(x)
        if (x not in visited) and (x not in unvisited):            
                unvisited.append(x)
                
    a=soup.find('a',{'class':"Next"})   #下一页<a>
    if a!=None:
        x=a.attrs['href']           #网址
        if re.match(r'xwdt\/.+',x):
            x='http://www.zut.edu.cn/index/'+x
        else:
            x='http://www.zut.edu.cn/index/xwdt/'+x
        if (x not in visited) and (x not in unvisited):            
           unvisited.append(x)    
    
    title=soup.title
    article=soup.find('div',class_='c67215_content',id='vsb_newscontent')
    author=soup.find('span',class_="authorstyle67215")  #作者
    time=soup.find('span',class_="timestyle67215")
    if title==None and article==None and author==None:
        print('无内容的页面。')
        continue

    elif article==None and author==None:
        print('只有标题。')
        title=title.text
        title=''.join(title.split())
        article=''
        author=''

    elif article==None:
        print('有标题有作者，缺失内容')
        title=title.text
        title=''.join(title.split())
        article=''
        author=author.get_text("",strip=True)
        author=''.join(author.split())

    elif author==None:
        print('有标题有内容，缺失作者')
        title=title.text
        title=''.join(title.split())
        article=article.get_text("",strip=True)
        article=''.join(article.split())
        author=''
    else:
        title=title.text
        title=''.join(title.split())
        article=article.get_text("",strip=True)
        article=''.join(article.split())
        author=author.get_text("",strip=True)
        author=''.join(author.split())

    print('网页标题：',title)

    #提取出的网页内容存在title,article,author三个字符串里，对它们进行中文分词
    seggen=jieba.cut_for_search(title)
    seglist=list(seggen)
    seggen=jieba.cut_for_search(article)
    seglist+=list(seggen)
    seggen=jieba.cut_for_search(author)
    seglist+=list(seggen)

    #数据存储
    conn=sqlite3.connect("viewsdu.db")
    c=conn.cursor()
    #c.execute('insert into doc values(?,?)',(cnt,url))

    #对每个分出的词语建立词表
    for word in seglist:
        #print(word)
        #检验看看这个词语是否已存在于数据库
        #c.execute('select list from word where term=?',(word,))
        result=c.fetchall()
        #如果不存在
        if len(result)==0:
            docliststr=str(cnt)
            #c.execute('insert into word values(?,?)',(word,docliststr))
        #如果已存在
        else:
            docliststr=result[0][0]#得到字符串
            docliststr+=' '+str(cnt)
            c.execute('update word set list=? where term=?',(docliststr,word))

    conn.commit()
    conn.close()
print('词表建立完毕=======================================================')

```

# **爬虫应用-爬取百度图片**

<iframe src="//player.bilibili.com/player.html?aid=667662157&bvid=BV1Ra4y1t7BZ&cid=176201304&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

[视频资源参考-b站](https://www.bilibili.com/video/BV1Ra4y1t7BZ?t=1107)
## 功能介绍

## 设计思路

## 关键技术

### 图片下载到本地



使用`urlretrieve()`函数，下载对应图片到本地

1. 使用`request.urlretrieve()`函数
   
        from urllib import request
        request.urlretrieve("http://www.zzti.edu.cn_mediafile/index/2017/06/24/lqjdyc7vq5.jpg","aaa.jpg")

   上述 把 `lqjdyc7vq5.jpg`下载到本地 命名为 `aaa.jpg`


2. 使用python文件操作 `write( )`写入文件
   
        from urllib import request
        import urllib 
        url='http://www.zzti.edu.cn/_mediafile/index/2017/06/24/lqjdyc7vq5.jpg'          #给url赋值
        urll=urllib.request.Request(url)    # Request()函数将url添加到头部，模拟浏览器访问
        page=urllib.request.urlopen(urll).read()     # 将url页面的源代码保存成字符串
        # open().write() 方法原始有效
    
        open('C:\\aaa.jpg','wb').write(page)         # 写入aaa.jpg文件


**格式**

1. 导入 urllib.request模块
2. 导入urllib库
3. 赋值`url地址`链接给url 变量
4. 用request模块的`Requset(url)` 函数请求`源代码` 返回给 urll
5. 用`requset.urlopen(urll).read()`将源代码保存`字符串` page
6. `写入`到指定本地地址 
   
    使用 `open('本地地址url').write(page)`

### **爬取指定网页图片**
使用urllib库模拟浏览器访问网站行为，由给定的`url`的到对应的源代码`html`，源代码以字符串的形式返回

然后用正则表达式`re`库，匹配源代码的中的与图片有关的链接字符，返回列表，循环列表，依次下载图片

urllib在python2.x与python3.x差距很大，以下python3.x为例

## 一个简单的爬取图片的程序
```python
import urllib.request      # 导入 request模块
import re # 引入正则表达式库

def getHtmlCode(url): #该方法传入url,返回对应url的源代码
	headers={
		'User-Agent': 'Mozilla/5.0(Linux;android 6.0;Nexus 5 Build/MRA58N) APPLEWebkit/537.36(KHTML, like Gecko) Chrome/56.0.2924.87 Mobile Safari/537.36'
	}       #设置一个浏览器请求对象
	urll=urllib.request.Request(url,headers=headers)
	# 添加头部，模拟浏览器行为
	page=urllib.request.urlopen(url).read()  #将网页源代码保存为字符串
	page=page.decode('UTF-8')      #字符串转码
	return page
	
def getImg(page):       #传入hml源码，截取img标签，保存图片到本机
	imgList=re.findall(r'(http:[^\s]*?(jpg|png|gif))"',page)
	x=0
	for imgUrl in imgList:        #如果符合img的链接格式，则循环下载
		try:
			print('正在下载: %s'%imgUrl[0])      #储存在列表中
			# urlretrieve(url,local)方法         根据图片的url将图片下载到本机
			urllib.request.urlretrieve(imgUrl[0],'E:/img/%d.jpg'%x) 
			 #第一个参数是，要保存的图片在imglist中的位置
			#第二个参数是要保存在本机的位置
			x+=1
		except:
			continue
if __name__=='__main__':
	url='http://blog.csdn.net/qq_32166627/article/details/60345731'   #指定网址页面
	page=getHtmlCode(url)     #根据url得到网页源代码
	getImg(page)         #根据源代码下载图片
	
```

对于`findall (正则表达式,代表页面源代码str) `函数

在字符串中按正则表达式截取字符串,`findall`返回一列表，列表元素是元组，元组第一元素是 : `图片url`,第二个元素 : `url扩展名`

如 

`[('http://avatar.csdn.net/4/E/B/1_11_3216627.jpg'),'jpg']`    
![]()
[('图片url地址'), '图片格式扩展名']

上述找图片是用`re`正则表达式，要正确使用正则表达式

正则表达式难以掌握，引入 `BeautifulSoup`库 (第三方库)

根据标签名称对网页内容进行截取

[具体见BeafulSoup中文档](http://beautifulsoup.readthedocs.io/zh_CN/latest/)

### **BeautifulSoup库**

`keyword` :处理 HTML/XML函数库，快速抓取网页

BeautifulSoup库是python处理HTML/XML的函数库

是python内置的网页分析工具，快速转换  抓取的网页    ，产生DOM树，尽量和原文档内容相同，满足用户的查找数据的需求

`keyword`: 入为 `Unicode`,出为 `UTF-8`

提供简单方法 ，转换为DOM树，自动 转 文档为`Unicode`码，输入为 `UTF-8`码 ，BS可找所有链接 `<a><a/>`,或所有class =xxx 的a链接，或匹配url  的 a链接

**BeautifulSoup安装***

使用`pip`

	pip3 install beautifulSoup4

>推荐项目使用`BeautifulSoup4(bs4)` ,需导入 `import bs4`

**基本使用**

	from bs4 import BeautifulSoup
	# 可是HTML内容字符串，本例是列表，需转为字符串
	doc=['<html><head><title> The story of Monkey </title></head>',
		 '<body><p id="firstpara", align="center">This is one paragraph</p>,'<p id="secondpara",align="center">This is two paragraph></p>,'</html>']
	soup=BeautifulSoup(''.join(doc),"html.parser")    # 提供字符串信息， ''.join(doc) 将其合并为字符串
	
	print(soup.prettify() )


​	
1.使用BeautifulSoup之前要导入库 `bs4`

	from bs4 import BeautifulSoup

2.创建BeautifulSoup对象

	soup=BeautifulSoup(html) 

3.还要用本地文件夹HTML创建对象

	soup=BeautifulSoup(open("index.html') , "html.paser")          #提供本地文件HTML

>上述是将本地的`index.html`打开，创建soup对象

也可以使用 `URL`获取HTML文件

	from urllib import request
	response=request.urlopen("http://www.baidu.com")          #直接用url打开某个网址的代码
	html=response.read()       #读取源代码
	html=html.decode("UTF-8")     #相当手动输出时进行解码为UTF-8格式，否则会乱码
	soup=BeautifulSoup(html,"html.paser")          #远程网站上的HTML文件

>程序最后格式化为`BeautifulSoup`对象的内容
	print(soup.prettify() )

运行结果

	<html>
		<head>
			<title> The story of Monkey</title>
		</head>
		<body>
			<p align="center" id="firstpara">This is one paragraph</p>
			<p align="center" id="secondpara">This is two paragraph</p>
		</body>
	</html>

>以上时`格式化`打印 BeatifulSoup对象的(DOM树)的内容

BeautifulSoup  转 HTML 为 复杂树结构，每一节点为 `python对象`

**对象归为4类**
- Tag
- NavigableString
- BeautifulSoup
- Comment

1.**tag :( html中标签)**

查找`第一个`符合要求的标签(默认)
如 

	print(soup.title)# 打印 <title> The story of Monkey</title>

验证类型

	print(type(soup.title))  # 输出 : <class 'bs4.element.Tag>


​	
对于tag,两重要属性  `name` , `attrs`

	print(soup.name)       # 输出 : [document]           
	print(soup.head.name) #  输出 :head

soup.name很特殊，为 `[document]`，其余标签的name为其标签名

>soup相当于 `html对象` ，所以是 [document] 和javascript类似

输出p标签的`所有属性`,得到`字典`

	print(soup.p.attrs)  #输出  :  {'id','firstpara','align','center'}

单个输出,如获取id

	print(soup.p'[id'])    #输出 : firstpara

还可以用`get()`方法传入属性的名称

	print(soup.p.get('id')) #输出 ； firstpara

修改(就是赋值)

	soup.p['class'] = "newclass"
删除

	del soup.p['class']


**NavigableString对象**

获取标签文字 : `.string`

	soup.title.string

**BeautifulSoup对象**
其表示文档的全部内容，大部分时候当成 Tag对象,下面代码分别获取类型，名称，属性

	print(type(soup))     #输出 :   <class 'bs4.BeautifulSoup'>
	print(soup.name)   #输出 :  [document]
	print(soup.attrs)    #输出空字典 {}

>同样把BeautifulSoup当成文档的html对象理解即可

**Comment对象**

注释是特殊的NavigableString对象 ，不包括注释符号，若处理不当会造成麻烦


### **使用BeatifulSoup库解析HTML文档树**
1. **遍历文档树**
	

⭐`.content` 和 `.children` 获取`直接子节点`

Tag 的`.content`属性可将Tag以`列表list`方式输出

    print(soup.body.contents)
    
    # 输出
    
    [<p align="center" id="firstpara">This is one paragraph</p>,
      <p align="center" id="secondpara">This is two paragraph</p>]

>Tag.content 表示其`子节点`

此时输出为list，可以输出某一个元素

    print(soup.body.contents[0])   #  表示输出这个子节点的第一个子节点
    
    # 输出
    
    <p align="center" id="firstpara">This is one paragraph</p>
    # 只输出标签

`.children`返回的是 `list生成器对象`，用户可以通过遍历获取所有`子结点`

    for child in soup.body.children:
      print(child)
    # 输出
    <p align="center" id="firstpara">This is one paragraph</p>
    <p align="center" id="firstpara">This is two paragraph</p>

⭐ `.descendants` 获取`所有`的`子孙结点`
      `/descendants`和`.children`一样可以对所有的`Tag`子孙结点循环

    for child in soup.descendants:
      print(child)

>所有结点打印出来，先是最外层的html,然后head,依次剥离

⭐结点内容
	
如一标签没有子标签/只有唯一子标签 ，`.string`返回最里边标签的内容

如Tag包含多标签，`.string`输出 `None`

		print(soup.title.string)        # 输出 : The story of Monkey ，即<title>标签内容
		print(soup.body.string)     #<body>包含多标签，输出: None

⭐父节点

`.parent` 属性用于获取父节点
	
  	p=soup.title
	print(p.parent.name)   # 输出 : Head
	
	
	
	  
2. **搜索文档树**
	

⭐`find_all(name,attrs,recursive,text,**kwargs)`
	
搜索当前Tag所有子节点，并判断是否符合过滤条件
	
参数
	
- `name` : 可以查找所有名字 为 name 的标签
	
			print(soup.find_all('p'))          #输出所有的<p>标签
			# 输出

>注意这个 `name`是 find_all()参数

  如name使用正则表达式，则BeautifulSoup使用re 的 `match()`匹配，如 

      for tag in soup.find_all(re.compile("^h")):       表示匹配以h开头的标签
    		# 输出
    		html head


​		
- `atrrs参数` :  按tag属性值检索，需要列出属性值和属性名,采用`字典`的形式
  
			soup.find_all(   'p',        atrrs={'id':"firstpara"}    )
			或
			soup.find_all(     'p',        {'id':"firstpara}     )
		
>当然也可以采用html关键字形式 

    	soup.find_all('p',{id="firstpara"})

- `recursive参数` : 调用 soup.find_all()方法，BeautifulSoup会检索当前Tag的所有子节点，如只想搜索直接子节点，设置 `recursive = Flase`

- `text参数` :  通过 text搜文档字符串内容
		
      print(soup.find_all(text=re.compile("paragraph")))    # re.compile正则表达式
      # 输出
      ['This is one paragraph', 'This is two paragraph']    #以列表的形式

re.compile("parapraph")表示含`paragraph`的字符串匹配
		
- `limit` : find_all返回搜索的结果，用limit可以限制返回的数量，当搜索数量达到limit则停止搜索
	
		print(soup.find_all("p", limit=1))
		# 输出
		[<p align="center" id="firstpara"> This is one paragraph</p>]
		# 本来有两个<p>标签，因为限制为1个，所以返回1个
	

⭐ `find(name,atrrs,recursive,text)`

与 find_all不同的是，返回第一个结果

3. **用Css选择器筛选元素**

`soup.select() `:返回列表
	
   - 通过标签名查找
	

    soup.select('title')    # 选取 <title>元素

   - 类名class查找

    soup.select('.firstpara') #  选取class是 firstpara的元素
    soup.select_one('.firstpara') # 选取class是 firstpara的第一个元素

   - id 查找


		  soup.selcet('#firstpara') # 选取id是firstpara的元素


以上返回列表形式，可以遍历输出 ，用` get_text() `/`text`获取内容

		soup=BeautifulSoup(hmtl,'html.parser')    #创建当前文档的对象
		print(type (soup.select('div')))  
		print(soup.select('div')[0].get_text())  #输出首个div的内容
		for title in soup.select('title')     
			print(title.text)                                # 输出所有div的内容

**小练习**

```python
	from bs4 import BeautifulSoup
	from urllib import request
	import urllib
	#import urllib
	def getHtmlCode(url):                 # url是目标网页的网址
		# 设置浏览器代理
		headers={
		'User-Agent':'Mozilla/5.0(windows NT 6.1; WOW64; rv:31.0) Gecko/20100101 Firefox/31.0'
		}
		# Request函数将 url添加到头部，模拟浏览器行为，相当于和服务器说: "我是xxx浏览器请求内容" ，不是 py爬取请求的内容，防止反爬虫
		urll=urllib.request.Request(url,headers=headers)   # 相当于得到一个网页对象
		#
		page=urllib.request.urlopen(urll).read()      # 获取这个网页的源代码,并保存为字符串
		page=page.decode('UTF-8') #输出时将字符转为 UTF-8模式，防止乱码
		return page # 返回源代码
		
	def getImg(page,localPath):          #  参数 page:源代码 localPath : 要保存的本地地址
		soup=BeautifulSoup(page,'html.parser')    #按照html格式解析页面
		imgList=soup.find_all('img')                          #返回包含所有img标签的链接的列表
		x=0
		for imgUrl in imgList:             #列表循环
			print('正在下载:%s'%imgUrl.get('src'))  
			# urlretrieve(url,local) 根据图片的url将图片保存到本地
			urllib.request.urlretrieve(imgUrl.get('src'),localPath+'%d.jpg'%x)
			x+=1
	    
	            
if __name__=='__main__':
  url='https://martinniee.gitee.io/2020/05/04/%E7%9B%B8%E5%86%8C/'
  localPath='E:/img/'
  page=getHtmlCode(url)
  getImg(page,localPath)
	
```

>可见BeautifulSoup比正则表达式更方便找到所有的`img标签`






​	

​	


​			
​	
​		

### **request库使用**
`requests`和`urllib`库有类似，用法相同，`requests`比`urllib`使用更简单些

1. **requests库安装** 

        pip3 install requests

>注意是`requests`,有`s`

安装测试python是否导入模块成功

        import requests
没报错则成功
	
[详情请看官方文档](http://cn.python-request.org/zh_CN/lastest/)

2. **发送请求**
   

首先导入模块`requests`

    >>> import requests
    获取一个网页
    >>> r=requests.get('http://www.zut.edu.cn')
>用`requests.get()`方法获取网页请求对象`r`

>之后可以使用`r`的各种方法和函数

关于 **HTTP的请求**
- POST
- PUT
- DELETE
- HEAD
- OPTIONS

       #同样实现
    
        >>> r=requests.head('http://www.zut.edu.cn')
        >>> r=requests.post('http://www.zut.edu.cn')
	
3. **在url中传递参数**
	

有时需要在URL中传递参数，如`wd` :搜索词；`rn`:搜索结果数量,使用`requests`更简单

    >>>  playload={'wd':'夏敏捷','rn','100'}
    >>>  r=requests.get('http://www.baidu.com/s',params=playload)
    >>> print(r.url)
    #输出
    http://www.baidu.com/s?wd=%E5%A4%8F%E6%95%8F%E6%8D%B7&rn=100

>上面的乱码`wd`就是对搜索结果的URL转码形式

 POST参数请求

        requests.post('http:/www/itwhy.org/wp-comments-post.php',data={'comment':'测试POST'})      #POST参数
                
    	# 格式requests.post(url,data={})      

4. **获取响应内容**
		
		>>> r=requests.get('http://www.baidu,com')
		>>> r.text

**结果如下**
```html
<!DOCTYPE html>
<!--STATUS OK-->
<html>
<head>
    <meta http-equiv=content-type content=text/html;charset=utf-8>
    <meta http-equiv=X-UA-Compatible content=IE=Edge>
    <meta content=always name=referrer>
    <link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css>
    <title>ç¾åº¦ä¸ä¸ï¼ä½ å°±ç¥é</title></head>
<body link=#0000cc>
<div id=wrapper>
    <div id=head>
        <div class=head_wrapper>
            <div class=s_form>
                <div class=s_form_wrapper>
                    <div id=lg><img hidefocus=true src=//www.baidu.com/img/bd_logo1.png width=270 height=129></div>
                    <form id=form name=f action=//www.baidu.com/s class=fm><input type=hidden name=bdorz_come value=1>
                        <input type=hidden name=ie value=utf-8> <input type=hidden name=f value=8> <input type=hidden
                                                                                                          name=rsv_bp
                                                                                                          value=1>
                        <input type=hidden name=rsv_idx value=1> <input type=hidden name=tn value=baidu><span
                                class="bg s_ipt_wr"><input id=kw name=wd class=s_ipt value maxlength=255
                                                           autocomplete=off autofocus></span><span
                                class="bg s_btn_wr"><input type=submit id=su value=ç¾åº¦ä¸ä¸ class="bg s_btn"></span>
                    </form>
                </div>
            </div>
            <div id=u1><a href=http://news.baidu.com name=tj_trnews class=mnav>æ°é»</a> <a href=http://www.hao123.com
                                                                                             name=tj_trhao123
                                                                                             class=mnav>hao123</a> <a
                    href=http://map.baidu.com name=tj_trmap class=mnav>å°å¾</a> <a href=http://v.baidu.com
                                                                                     name=tj_trvideo
                                                                                     class=mnav>è§é¢</a> <a
                    href=http://tieba.baidu.com name=tj_trtieba class=mnav>è´´å§</a>
                <noscript><a
                        href=http://www.baidu.com/bdorz/login.gif?login&amp;tpl=mn&amp;u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1
                        name=tj_login class=lb>ç»å½</a></noscript>
                <script>
                    document.write('<a href="http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u='+ encodeURIComponent(window.location.href+ (window.location.search === "" ? "?" : "&")+ "bdorz_come=1")+ '" name="tj_login" class="lb">ç»å½</a>');
                </script>
                <a href=//www.baidu.com/more/ name=tj_briicon class=bri style="display: block;">æ´å¤äº§å</a></div>
        </div>
    </div>
    <div id=ftCon>
        <div id=ftConw><p id=lh><a href=http://home.baidu.com>å³äºç¾åº¦</a> <a href=http://ir.baidu.com>About
            Baidu</a></p>
            <p id=cp>&copy;2017&nbsp;Baidu&nbsp;<a href=http://www.baidu.com/duty/>ä½¿ç¨ç¾åº¦åå¿è¯»</a>&nbsp; <a
                    href=http://jianyi.baidu.com/ class=cp-feedback>æè§åé¦</a>&nbsp;äº¬ICPè¯030173å·&nbsp; <img
                    src=//www.baidu.com/img/gs.gif></p></div>
    </div>
</div>
</body>
</html>

```

>requests返回Response对象，储存服务器响应内容

>用户可用`r.text`获取网页内容,这个text指的是`网页的一些信息，头部，服务器响应的内容`

**还可以通过`r.content`获取页面内容**
	
		>>> r.content

>`r.content`以字节的方式显示，所以在IDLE中以`b`开头

		>>> r.encoding      #可使用r.encoding获取网页编码
		 'UTF-8'

返回

<p style="background:yellow">也就是说获取页面源代码的方式</p>

1.⭐

	        r=requests.get(url)
	
			print(r.content)
			print(r.text)
			都是获取这个网页的html页面源代码

>就是整个html文本`text`

**如 用`requests`库获取html源代码**

```html
import requests
import urllib
from urllib import request
def getHtmlCode(url):
    #   111：模拟浏览器
    #设立请求头部，防止被反爬虫而进制访问
    headers={
        'User-Agent': 'Mozilla/5.0(Linux;android 6.0;Nexus 5 Build/MRA58N) APPLEWebkit/537.36(KHTML, like Gecko) Chrome/56.0.2924.87 Mobile Safari/537.36'

    }
    # Request函数将url添加到头部，模拟浏览器访问
    urlone=urllib.request.Request(url,headers=headers)
    #   222:正式获取html源代码
    #将url指向的html页面源代码保存成字符串
    page=urllib.request.urlopen(urlone).read()  #以Unicode编码方式读取
    page=page.decode('UTF-8') #以UTF-8编码方式转码，防止页面乱码
    return page       #返回最后的html源代码

p=getHtmlCode("http://www.baidu.com")
print(p)
```
**⭐还可以通过`urllib`库** 

		urlre=urllib.request.Request
		text=urllib.request.urlopen(urlre)read()
		text=text.decode('UTF-8')
		return text

获得`html`页面源代码

>当发送请求，requests会根据HTTP头部获取网页编码，用户可以修改这个`requests`编码形式

		>>> r=requests.get('http://www.zhidaow.com')
		>>> r.encoding
		'utf-8'
		>>> r.encoding='ISO-8859-1'

>可以通过修改encoding内容改变编码，获取网页内容

>像这样就修改了网页内容


1. ⭐JSON

如用`json`，要引入新模块`json`和`simplejson` ,`requests`有内置的函数`r.json`，以查询IP的API为例
    

		>>>r=requests.get('http:ip.taopao.com/service/getIpInfo.php?ip=202.169.32.7')
		>>> r,json()
		{'data':{'region-id':'410000',;country':'XX','city_id':'410100','area':'','country':'中国','country_id':'CN','isp':'教育网','area_id':'','city':'郑州','ip':'202.169.32.7','region':'河南','isp_id':'100027','country_id':'xx'},'code':0}
		>>> r,json()['data']['country']
			'中国'

6. **网页状态码**
       

使用`r.status_code`检查网页状态码
	
		>>> r=requests.get('http://www.mengtiankong.com')
		>>> r.status_code
		200
		>>> r=requests.get('http://www.mengtiankong.com/123123/')
		>>> r.status_code
		404

>网页的状态，对应有一个编码 404/403/200…

>200正常。404不能正常打开

7. **响应头部内容**

`r.headers`获取头部内容
	
		>>>r=requests.get('http://www.zhidaow.com')
		>>> r.headers
		{
			'content-encoding':'gzip',
			'transfer-encoding':'chunked',
			'conten-type':'text/html;charset=utf-8';
			…


​		
		}

>可以看到以`字典`的形式返回所有内容


8. **设置超时时间**

`timeout`,一但超过这个时间，还没获取响应内容则提示错误
	
		>>> requests.get('http://github.com',timeout=0.001)
		Treceback(most recent call last):
		
		.
		.
		.
		#格式
		requests.get(url,timeout=)


9. **代理访问**

采集为避免封`IP`，使用代理，requests有相应的属性`proxies`
	
		import requests
		proxies={
			"http": "http://10.10.1.10:3128".
			"http": "http://10.10.1.10:1080"
			
		# 如果代理需要账户密码，则这样
		proxies={
			"http":"http://user:pass@10.10.1.10:3128/",
		}

10. **请求头内容**

用`r.requests.headers`获取

		>>> r.requests.headers
		{'Accept-Encoding': 'identify,deflate,compress,gzip','Accept':'*/*','User-Agent':'python-requests/1.2.3CPython/2.7.3Windows/XP'}

>模拟浏览器行为

11. **自定义请求头部**

伪装请求头部，用于隐藏自己
	
- --


下面用`requests`库替换`urllib`库，用`open().write()`方法替换`urllib.request.urlretrieve(url,localpath)`方法下载图片

```python
# 使用requests,bs4 下载所有图片

import os
import requests
from bs4 import BeautifulSoup


# *************导入相关的库************

# 获取网页源代码
def getHtmlCode(url):
    # 设置头部请求
    headers = {
        'User-Agent': 'Mozilla/5.0(windows NT 6.1; WOW64; rv:31.0) Gecko/20100101 Firefox/31.0'
    }
    # 通过头部请求和url返回网页对象r
    r = requests.get(url, headers=headers)
    # 输出设置为utf-8编码
    r.encoding = 'UTF-8'  # 指定网页解析的编码模式
    # 获取url页面源代码字符串文本page
    page = r.text
    # 返回page
    return page


# 1.设头部
# 2. 创对象
# 3.定编码
# 4.获页面
# 5.返页面

# 获取图片函数
def getImg(page, localPath):  # 改方法传入html源代码，截取img标签，将图片保存
    # 先建文件夹
    if not os.path.exists(localPath):
        os.mkdir(localPath)
    # 创建一个网页文档解析对象soup
    soup = BeautifulSoup(page, 'html.parser')
    # 返回所有的img标签
    imgList = soup.find_all('img')
    # 设初值
    x = 0
    # 循环下载
    for imgUrl in imgList:
        # 设置异常处理try ,except
        try:
            print('正在下载: %s' % imgUrl.get('src'))
            # 分情况讨论img 地址的格式
            # 若不是 http://开头 (非绝对路径)，则需要加上"http://"
            if "http://" not in imgUrl.get('src'):
                m = 'http://www.zut.edu.cn/' + imgUrl.get('src')
                print("正在下载:%s" % m)  # 提示下载进度
                # 得到某图片绝对路径
                ir = requests.get('http://www.zut.edu.cn/' + imgUrl.get('src'))
            else:
                ir = requests.get('http://ww.zut.edu.cn/')  # 直接得到图片的绝对路径
            # 用write()方法写入本地文件
            open(localPath + '%d.jpg' % x, 'wb').write(ir.content)
        # 格式 open(本地路径+".jpg", 'wb' ).write(图片网址)
            x +=1
        except:
            continue


# 爬虫开启语句
if __name__ == '__main__':
    url = 'http://www.zut.edu.cn/'  # 要爬取的网页地址
    localPath = 'E:/img/'  # 存入本地的路径
    page = getHtmlCode(url)  # page : 要爬取网页的源代码文档
    getImg(page, localPath)  # 获取图片

```


## **设计步骤**

### **分析网页源代码和网页结构**

### **设计代码**
python爬虫搜索百度图片下载图片的代码如下

**源代码** `可行代码`
```python
import requests #首先导入库
import  re
#设置默认配置
MaxSearchPage = 20 # 收索页数
CurrentPage = 0 # 当前正在搜索的页数
DefaultPath = "pictures" # 默认储存位置
NeedSave = 0 # 是否需要储存
#图片链接正则和下一页的链接正则
def imageFiler(content): # 通过正则获取当前页面的图片地址数组
          return re.findall('"objURL":"(.*?)"',content,re.S)
def nextSource(content): # 通过正则获取下一页的网址
          next = re.findall('<div id="page">.*<a href="(.*?)" class="n">',content,re.S)[0]
          print("---------" + "http://image.baidu.com" + next)
          return next
#爬虫主体
def spidler(source):
          content = requests.get(source).text  # 通过链接获取内容
          imageArr = imageFiler(content) # 获取图片数组
          global CurrentPage
          print("Current page:" + str(CurrentPage) + "**********************************")
          for imageUrl in imageArr:
              print(imageUrl)
              global  NeedSave
              if NeedSave:  			# 如果需要保存图片则下载图片，否则不下载图片
                 global DefaultPath
                 try:
                      # 下载图片并设置超时时间,如果图片地址错误就不继续等待了
                      picture = requests.get(imageUrl,timeout=10)
                 except:
                      print("Download image error! errorUrl:" + imageUrl)
                      continue
                 # 创建图片保存的路径
                 imageUrl = imageUrl.replace('/','').replace(':','').replace('?','')
                 pictureSavePath = DefaultPath + imageUrl
                 fp = open(pictureSavePath,'wb') # 以写入二进制的方式打开文件
                 fp.write(picture.content)
                 fp.close()
          global MaxSearchPage
          if CurrentPage <= MaxSearchPage:    #继续下一页爬取
               if nextSource(content):
                   CurrentPage += 1
                   # 爬取完毕后通过下一页地址继续爬取
                   spidler("http://image.baidu.com" + nextSource(content))
#爬虫的开启方法
def  beginSearch(page=1,save=0,savePath="E:/pictures/"):
          # (page:爬取页数,save:是否储存,savePath:默认储存路径)
          global MaxSearchPage,NeedSave,DefaultPath
          MaxSearchPage = page
          NeedSave = save					#是否保存，值0不保存，1保存
          DefaultPath = savePath				#图片保存的位置
          key = input("Please input you want search：")
          StartSource = "http://image.baidu.com/search/flip?tn=baiduimage&ie=utf-8&word=" + str(key) + "&ct=201326592&v=flip" # 分析链接可以得到,替换其`word`值后面的数据来搜索关键词
          spidler(StartSource)
#调用开启的方法就可以通过关键词搜索图片了
beginSearch(page=5,save=1)			# page=5是下载前5页，save=1保存图片
```
**测试代码**
```python
import request     #导入库
import re             # 正则表达式相关库

#设置默认配置MaxSearchPage=20    #最大搜索页数
CurrentPage=0                   #当前正在搜索的页数
DefaultPath='pictures'     #默认储存位置
NeedSave=0                    #是否储存

#设置图片链接的正则，和<下一页>的正则
def  imageFiler(content):          #通过正则获取图片链接地址的数组
	return re.findall('"objURL":"(.*?)"',content,re.S)        # (正则，)
def nextSource(content):            #通过正则获取<下一页>的网址
	next = re.findall('<div id='page'>.*<a href="(.*?)" class="n">',content,re.S)[0]       
	print("-------------"+"http://baidu.com"+next)
	return next
#爬虫主体
def spidler(source):        #爬虫函数
	content=request.get(source).text    #通过链接获取内容
	imageArr=imageFiler(content)         #获取图片数组
	global CurrentPage   #全局变量
	print("Current page:"+str(CurrentPage)+"*************")   
	for imageUrl in imageArr:
		print(imageUrl)
		global NeedSave
		if NeedSave:                 #如果需要需要保存图片则下载图片
			global DefaultPath
			try:
				#下载图片并设置超时时间，如图片地址错误，不等待
				picture=request.get(imageUrl,timeout=10)
			except:
				# 如果出错则输入下载图片地址错误
				print("Download image error! errorUrl:" +imageUrl )
				continue 
			# 创建图片保存路径
			imageUrl=imageUrl.replace('/','').replace(':','').replace('?','')
			pictureSavePath=DefaultPath+imageUrl #如"C:/image"+"/img001"
			fp=open(picutureSavePath,'wb') #以写入二进制文件的方式打开文件
			fp=write(picture.content) #向这个路径添加图片
			fp=close()
		global MaxSearchPage
		if CurrentPage<=MaxSearchPage:
			if nextSource(content):
				CurrentPage+=1
				# 爬取完毕，通过<下一页>的地址继续爬取
				spidler("http://image.baidu.com"+nextSource(content))
#爬虫的开启方法
def beginSearch(page=1,save=0,savePath="pictures/"):
	#page :爬取页数，save:是否储存，savePath:默认储存路径
	global MaxSearchPage,NeedSave,DefaultPath
	MaxSearchPage=page
	NeedSave=save     #是否保存，保存为1 ，不保存为0
	DefaultPath=savePath    # 图片保存的位置
	key=input("please inout tou want to search: ")
	StartSource="http://image.baidu.com/search/flip?tn=baiduiimage&ie=utf-8&word="+str(key)+"&ct=201326592&v=flip"
	#分析，替换word值后面的关键字即可实现搜索关键字
	spidler(StartSource)
	#调用开启的即可通过关键字搜索图片
	beiginSearch(page=5,save=1)   # page 下载前5页，save保存图片		
```

## **使用BeautifulSoup和关键字爬取百度图片**
[来源](https://blog.csdn.net/lingyunxianhe/article/details/103958594)

【例子】`可行代码`
```python
import re
import requests
from urllib import error
from bs4 import BeautifulSoup
import os

num = 0
numPicture = 0
file = ''
List = []


def Find(url):
    global List
    print('正在检测图片总数，请稍等.....')
    t = 0
    i = 1
    s = 0
    while t < 100:
        Url = url + str(t)
        print(Url)
        try:
            Result = requests.get(Url, timeout=7) #超时设置-136        #返回Url指向的那个网页的html对象
        except BaseException:            #当超时说明这个图片加载不出来，则跳跃图片
            t = t + 60 #向后滑动60个图-一页60个图
            continue
        else:
            result = Result.text
            pic_url = re.findall('"objURL":"(.*?)",', result, re.S)  # 先利用正则表达式找到图片url-147
            s += len(pic_url)
            # print(s)
            if len(pic_url) == 0:
                break
            else:
                List.append(pic_url)
                t = t + 60


    print(s)
    return s


def recommend(url):
    Re = []
    try:
        html = requests.get(url)
    except error.HTTPError as e:
        return
    else:
        html.encoding = 'utf-8'
        bsObj = BeautifulSoup(html.text, 'html.parser')
        # print(bsObj)
        div = bsObj.find('div', id='topRS') #定位
        if div is not None:
            listA = div.findAll('a')#定位a标签
            for i in listA:
                if i is not None:
                    Re.append(i.get_text())#取出标签内的推荐标题放入列表中
        return Re


def dowmloadPicture(html, keyword):
    global num
    # t =0
    pic_url = re.findall('"objURL":"(.*?)",', html, re.S)  # 先利用正则表达式找到图片url
    # print(pic_url)
    print('找到关键词:' + keyword + '的图片，即将开始下载图片...')
    for each in pic_url:
        print('正在下载第' + str(num + 1) + '张图片，图片地址:' + str(each))
        try:
            if each is not None:
                pic = requests.get(each, timeout=7)
            else:
                continue
        except BaseException:
            print('错误，当前图片无法下载')
            continue
        else:
            string = file + r'\\' + str(num) + '.jpg'    #  如    E:\picture\1.jpg
            fp = open(string, 'wb')     #打开文件操作
            fp.write(pic.content)#-125-content是二进制形式（text打印的是str形式）
            fp.close()
            num += 1
        if num >= numPicture: #控制下载的图片数
            return


if __name__ == '__main__':  # 主函数入口
    word = input("请输入搜索关键词(可以是人名，地名等): ")
    # add = 'http://image.baidu.com/search/flip?tn=baiduimage&ie=utf-8&word=%E5%BC%A0%E5%A4%A9%E7%88%B1&pn=120'
    url = 'http://image.baidu.com/search/flip?tn=baiduimage&ie=utf-8&word=' + word + '&pn='
    tot = Find(url)
    Recommend = recommend(url)  # 记录相关推荐
    print('经过检测%s类图片共有%d张' % (word, tot))
    numPicture = int(input('请输入想要下载的图片数量 '))
    print("请输入一个存储图片的本地磁盘:")
    print("格式为: C:\或E:\或F:\或D:\\")
    localPath=input()
    filename = input('请建立一个存储图片的文件夹，输入文件夹名称即可: ')
    # print("格式为:")
    file=localPath+filename     #合并为一个完整的路径 如 : E:\picture
    y = os.path.exists(file)    #判断文件是否存在，1为存在
    if y == 1:                  #若存在文件夹，则重新输入
        print('该文件已存在，请重新输入')
        file = input('请建立一个存储图片的文件夹,输入文件夹名称即可:  ')
        os.mkdir(file)
    else:
        os.mkdir(file)         #若不存在同样输入
    t = 0
    tmp = url
    while t < numPicture:
        try:
            url = tmp + str(t)
            result = requests.get(url, timeout=10) #超时设置
            # print(url)
        except error.HTTPError as e:
            print('网络错误，请调整网络后重试')
            t = t + 60
        else:
            dowmloadPicture(result.text, word)
            t = t + 60

    print('当前搜索结束，感谢使用')
    print('猜你喜欢')
    for re in Recommend:
        print(re, end='  ')


```

`version2`

```python
import re
import requests
from urllib import error
from bs4 import BeautifulSoup
import os

num = 0
numPicture = 0
file = ''
List = []


def Find(url):
    global List
    print('正在检测图片总数，请稍等.....')
    t = 0
    i = 1
    s = 0
    while t < 100:
        Url = url + str(t)
        # print(Url)
        try:
            Result = requests.get(Url, timeout=7) #超时设置-136        #返回Url指向的那个网页的html对象
        except BaseException:            #当超时说明这个程序
            t = t + 60 #向后滑动60个图-一页60个图
            continue
        else:
            result = Result.text
            pic_url = re.findall('"objURL":"(.*?)",', result, re.S)  # 先利用正则表达式找到图片url-147
            s += len(pic_url)
            # print(s)
            if len(pic_url) == 0:
                break
            else:
                List.append(pic_url)
                t = t + 60


    print(s)
    return s


def recommend(url):
    Re = []
    try:
        html = requests.get(url)
    except error.HTTPError as e:
        return
    else:
        html.encoding = 'utf-8'
        bsObj = BeautifulSoup(html.text, 'html.parser')
        # print(bsObj)
        div = bsObj.find('div', id='topRS') #定位
        if div is not None:
            listA = div.findAll('a')#定位a标签
            for i in listA:
                if i is not None:
                    Re.append(i.get_text())#取出标签内的推荐标题放入列表中
        return Re


def dowmloadPicture(html, keyword):
    global num
    # t =0
    pic_url = re.findall('"objURL":"(.*?)",', html, re.S)  # 先利用正则表达式找到图片url
    # print(pic_url)
    print('找到关键词:' + keyword + '的图片，即将开始下载图片...')
    for each in pic_url:
        print('正在下载第' + str(num + 1) + '张图片，图片地址:' + str(each))
        try:
            if each is not None:
                pic = requests.get(each, timeout=7)
            else:
                continue
        except BaseException:
            print('错误，当前图片无法下载')
            continue
        else:
            string = file + r'\\' + str(num) + '.jpg'    #  如    E:\picture\1.jpg
            fp = open(string, 'wb')     #打开文件操作
            fp.write(pic.content)#-125-content是二进制形式（text打印的是str形式）
            fp.close()
            num += 1
        if num >= numPicture: #控制下载的图片数
            return


if __name__ == '__main__':  # 主函数入口
    word = input("请输入搜索关键词(可以是人名，地名等): ")
    # add = 'http://image.baidu.com/search/flip?tn=baiduimage&ie=utf-8&word=%E5%BC%A0%E5%A4%A9%E7%88%B1&pn=120'
    url = 'http://image.baidu.com/search/flip?tn=baiduimage&ie=utf-8&word=' + word + '&pn='
    tot = Find(url)
    Recommend = recommend(url)  # 记录相关推荐
    print('经过检测%s类图片共有%d张' % (word, tot))
    numPicture = int(input('请输入想要下载的图片数量 '))
    print("请输入一个存储图片的本地磁盘:")
    print("格式为: C:\或E:\或F:\或D:\\")
    localPath=input()
    filename = input('请建立一个存储图片的文件夹，输入文件夹名称即可')
    # print("格式为:")
    file=localPath+filename     #合并为一个完整的路径 如 : E:\picture
    y = os.path.exists(file)
    if y == 1:
        print('该文件已存在，请重新输入')
        file = input('请建立一个存储图片的文件夹，)输入文件夹名称即可')
        os.mkdir(file)
    else:
        os.mkdir(file)
    t = 0
    tmp = url
    while t < numPicture:
        try:
            url = tmp + str(t)
            result = requests.get(url, timeout=10) #超时设置
            # print(url)
        except error.HTTPError as e:
            print('网络错误，请调整网络后重试')
            t = t + 60
        else:
            dowmloadPicture(result.text, word)
            t = t + 60

    print('当前搜索结束，感谢使用')
    print('猜你喜欢')
    for re in Recommend:
        print(re, end='  ')

```
**个人完成**

```python
# 爬取妹子图
# python基础用法
# 互联网的访问 ,访问方式

# 第三方库     Beautifulsoup
import requests
from bs4 import BeautifulSoup


def downLoadPicture():
    # 先找网站
    word = input("输入你想找的:")
    url = "https://www.ivsky.com/search.php?q=" + str(word)

    # 获取网站返回值(html)
    headers={
        'User-Agent': 'Mozilla/5.0(Linux;android 6.0;Nexus 5 Build/MRA58N) APPLEWebkit/537.36(KHTML, like Gecko) Chrome/56.0.2924.87 Mobile Safari/537.36'
    }
    res = requests.get(url,headers=headers)  # <Response [200]>
    # 解析返回内容
    soup = BeautifulSoup(res.text, "html.parser")
    # 寻找目标标签    div class_=il_img
    divs = soup.find_all("div", class_="il_img")
    list1 = []
    for div in divs:
        img = div.find_all("img")
        img = img[0]  # print(img["src"])   图片的url

        list1.append(img["src"])
    # 打开文件
    n = 0
    # 再次发出请求，访问图片网址
    for x in list1:
        res = requests.get(x)  # print(res.content) 图片的二进制表示
        n += 1
        print("正在下载第" + str(n) + "张图片-------------------------")
        with open(str(n) + ".jpg", "wb+") as file:  # 默认存入 python文件同目录
            file.write(res.content)
            print("第%d张图片下载完成" % n)


downLoadPicture()

```
> 参考教程

[通过关键词爬取百度图片——Python爬虫](https://blog.csdn.net/weixin_42453746/article/details/87724348?utm_medium=distribute.pc_relevant.none-task-blog-baidujs-4)

# itchat应用-微信机器人

### 安装 itchat

[官方技术文档](http://itchat.readthedocs.io/zh/latest/)

### **itchat微信登录**

运行代码，**显示二维码**，扫码登录发送`"hello,fielhelper"` 给 文件传输助手

		import itchat      #导入itchat库
		itchat.auto_login()  #登录微信
		# 发送消息，发送目标是文件传输助手
		itchat.send('Hello,filehelper',toUserName='filehelper')
	
		# 格式   :   itchat.send(  发送的消息  ，              )

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200608161635616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
### itchat消息类型

		import itchat #加载导入itchat库
		# 注册消息响应事件: 一个事件用于消息响应，消息类型为 itchat.content.TEXT ,即文本类型
		@itchat.msg_register(itchat.content.TEXT)
		def text_reply(msg):      #一个函数用于消息回复
			return msg['Text]     #返回这样的消息文本
		itchat.auto_login()       #登录微信
		itchat.run()              # 绑定消息响应事件后，让itchat运行起来

**itchat.content**包含所有的消息类型参数

参数名 | 类型 | Text值
-- | -- | --


**比如需要储存发送给自己的`附件`**

		@itchat.msg_register(ATTACHMENT)    #attachment
		def download_files(msg):   #下载文件函数
			msg['Text](msg['FileName']) #相当于调用Text()方法，参数为要下载的文件名


​		
msg字典的`Text`键是个下载方法，可以下载文件

打印消息响应事件的msg`，是一个字典

`可行代码`

```python

import itchat
from itchat.content import *    #导入全部的消息类型 (因为说过itchat.content为包含所有的消息类型参数)

# 处理文本类消息，如 文本，位置，名片，通知，分享
@itchat.msg_register([TEXT,MAP,NOTE,SHARING])
def text_reply(msg):    #回复消息函数
	# 因为微信中的用户和群聊是通过ID区分的，msg['fromUserName] 就是消息发送者的ID
	# 将消息类型和文本返回
	itchat.send('%s:%s'%(msg['Type'],msg['Text']),msg['FromUserName'])
	# 相当于 itchat.send(文本，发送者)

	# 处理多媒体消息,如 图片，录音，文件，视频
@itchat.msg_register([PICTURE,RECORDING,ATTACHMENT,VIDEO])
def dowmload_file(msg):
	# 前面说过msg['Text]是一个下载文件的函数,传入文件名，下载
	msg['Text'](msg['FileName']) #msg['Text'](    )
	# 下载好，再发送
	return '@%s@%s' %({'Picture':'img','Video':'vid'}.get(msg['Type'],'fil'),msg['FileName'])
	#相当于
	( 文件.get(msg['Type']) , 发送者   )

	# 处理好友添加请求,收到好友邀请，自动添加
@itchat.msg_register(FRIENDS)     #这个消息响应事件处理的是好友添加请求的消息
def add_friend(msg):     #定义一个添加好友的函数
	itchat.add_friend(**msg['Text'])    #该操作自动将新好友的消息录入，不需要重载入通讯录
	# 加完好友给好友打招呼
	itchat.send_msg('Nice to meet you!',msg['RecommendInfo']['UserName'])

# 处理群聊消息，注册时，增isGroup用于判定是群聊消息
@itchat.msg_register(TEXT,isGroupChat=True)
def text_reply(msg):   #消息回复，和一般消息回复一样
	if msg['isAt']:
		itchat.send(u'@%s\u2005I received: %s' % (msg['ActualNickName'],msg['Content']),msg['FromUserName'])
		# 如 itchat.send(信息，发送者)

# 在 auto_login()提供True ,即hotReload=True，一定时间内，可以保持登录状态
itchat.auto_login(True)
itchat.run()       #运行itchat
```

PICTURE,RECORDING,ATTACHMENT,VIDEO这几类的`msg`字典的`Text`键存放了下载消息内容的函数

要下载那个文件，就指定一个参数 : `msg['FileName']`

默认的 `@itchat.msg_register(TEXT)` 表示和好友聊天

加上参数 `isGroupChat=True`则为群聊

>群消息增加三个键

- `isAt` : 判断是否@本号自己

- `ActualNickName`: 实际的NickName

- `Content` : 信息内容

**通过下列程序测试**

```python
import itchat #导入库
from itchat.content import TEXT 
# -----------------------------

#设置消息响应事件
@itchat.msg_register(TEXT,isGroupChat=True)       #表示为群聊消息
def text_reply(msg):  # 消息回复函数
	if(msg.isAt):    #判断是否有人@自己
	# 如果有人@自己，就发消息告诉对方已收到
		itchat.send_msg("我已经收到了来自{0}的消息,实际内容为{1}".format(msg['ActualNickName'],msg['Text']),toUserName=msg['FromUserName'])

		# 如 itchat.send_msg(消息.格式化,信息的发送者为被@者)
	print(msg.isAt)       #输出True或Flase
	print(msg.actualNickName)
	print(msg.text)
itchat.auto_login()   #登录
itchat.run()    #运行
```

## itchat回复消息

**提供5种**

- send()
- send_msg()
- send_file()
- send_img()
- send_video()
  
### Python调用图灵机器人实现简单的人机交互

>相关链接  
[脚本之家系列](https://www.jb51.net/article/162277.htm)



**【微信聊天机器人】**

`可行代码`

```py
###################### 完整代码##############################
# 加载库
import requests
import json
import itchat
from itchat.content import *
itchat.auto_login()
# 调用图灵机器人的api，采用爬虫的原理，根据聊天消息返回回复内容
def tuling(info):
  appkey = "e5ccc9c7c8834ec3b08940e290ff1559"
  url = "http://www.tuling123.com/openapi/api?key=%s&info=%s"%(appkey,info)
  req = requests.get(url)
  content = req.text
  data = json.loads(content)
  answer = data['text']
  return answer

# 对于群聊信息，定义获取想要针对某个群进行机器人回复的群ID函数
def group_id(name):
  df = itchat.search_chatrooms(name=name)
  print(df)
  print(df[0]['UserName'])
  return df[0]['UserName']

# 注册文本消息，绑定到text_reply处理函数
# text_reply msg_files可以处理好友之间的聊天回复
@itchat.msg_register([TEXT,MAP,CARD,NOTE,SHARING])
def text_reply(msg):
  itchat.send('%s' % tuling(msg['Text']),msg['FromUserName'])
@itchat.msg_register([PICTURE, RECORDING, ATTACHMENT, VIDEO])
def download_files(msg):
  msg['Text'](msg['FileName'])
  return '@%s@%s' % ({'Picture': 'img', 'Video': 'vid'}.get(msg['Type'], 'fil'), msg['FileName'])

# 现在微信加了好多群，并不想对所有的群都进行设置微信机器人，只针对想要设置的群进行微信机器人，可进行如下设置
@itchat.msg_register(TEXT, isGroupChat=True)
def group_text_reply(msg):
  # 当然如果只想针对@你的人才回复，可以设置if msg['isAt']:
  item = group_id(u'夏家') # 根据自己的需求设置
  print('夏家group_id',item)
  print(msg['ToUserName'])
  if msg['FromUserName'] == item:                 #'夏家'群
    itchat.send(u'%s' % tuling(msg['Text']), msg['FromUserName'])
  if msg['isAt']:                                  #@你的人
    itchat.send(u'%s' % tuling(msg['Text']), msg['FromUserName'])
itchat.run()

```
# **词云实战---爬取豆瓣影评生成词云**

## 设计思路
**本程序分为三个过程**

- 抓取网页数据
- 清理数据
- 用词云进行展示


**抓取网页数据**

用`python爬虫`抓 豆瓣最新上映的电影网页

[正在上映-郑州](https://movie.douban.com/cinema/nowplaying/zhengzhou)

如下图所示

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200609145550772.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

html解析电影的`ID`和`电影名`，获取某一个`ID`计课得到该电影的`影评网址页面`

`https://movie.douban.com/subject/1292063/comments?status=P`

如这种形式，即为美丽人生的影评网址，**1292063**,即为`ID`,可以指定开始号，来获取更多的影评

如

`https://movie.douban.com/subject/1292063/comments?status=P?start=40&limit=20`

>表示获取从第40条开始的20个影评

**清理数据**

通常将影评数据放入`eachCommentList`列表，把`eachCommentList`成字符串`comment`，去除`comment`中的 `"也"`,`"太"`等虚词

**用词云展示**


## 关键技术
### 安装 `wordcloud`词云库

    pip install wordcloud

>可以在windows命令行输入指令

[下载不了解决](https://blog.csdn.net/DCclient/article/details/89818315)

[wordcloud-WHL安装包下载地址](https://www.lfd.uci.edu/~gohlke/pythonlibs/#wordcloud)

[pip最常用命令](https://www.runoob.com/w3cnote/python-pip-install-usage.html)



直接执行

    easy_install -i https://mirrors.aliyun.com/pypi/simple pip
即可    
### 使用wordcloud

**基本用法**

所有参数

参数名 | 描述 
-- | --
font_path | 展现字体，设置字体格式， `字体路径+扩展名` 如 `font_path='黑体.ttf'`


方法

方法名 | 描述
-- | --

**应用举例**

```python
# 导入wordcloud模块和matplotlib模块         mat plot lib
from wordcloud import WordCloud,ImageColorGenerator,STOPWORDS    # stop wprds
import matplotlib.pyplot as plt
import numpy as np
from PIL import Image
# --------------------------------
# 读取一个txt文件，注意修改编码格式
text=open('text.txt','r',encoding='utf-8').read()

#读取一个用于覆盖的图片
bg_pic=np.array(Image.open("alice1.png"))
# 设置词云样式
wc=WordCloud(
    background_color='white', #用于背景颜色，默认为黑色
    mask=bg_pic,
    font_path='simhei.ttf',  #设置字体集
    max_words=2000,
    max_font_size=150,     #最大单词的字体大小
    random_state=30,scale=1.5
)

wc.generate_from_text(text)      #用这个函数根据text文本生成词云
imgage_colors=ImageColorGenerator(bg_pic)  #用这个函数生成词云图片
#显示图片
plt.imshow(wc)     #显示词云图片
plt.axis('off')
plt.show()
print('display success!')      # 提示显示成功
wc.to_file('text2.jpg')          #保存图片


```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200609173117989.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

>注意`wordcloud`的下载

Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0;


   User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36 Edge/18.18362



**设置停用词**


## 程序设计步骤

1. 抓取网页数据
2. 数据清洗
3. 用词云进行显示
## 抓取网页数据
网页的访问，使用python的`urllib`库

**主要代码**

		# 1.导入库
		from urllib import request
		# 2. 请求url地址
		response = request.urlopen('http://movie.douban.com/mowplaying/hangzhou/')  
		# 3.获取源代码
		html_data = response.read().decode('utf-8')

html_data是字符串类型变量，存放的是url地址的源代码
使用`print(html_data)` ，即可看到网页的源代码

然后使用python中的`beautifulsoup`库，对源代码解析

安装 
	
	pip install Beautifulsoup

Beautifulsoup的使用格式 : 

	Beautifulsoup(html,"html.parser")  

第一个参数html : 表示某要解析的网页的源代码

第二个参数html.parser : 表示要解析的是html类型的网页，指定一个解析器

使用`find_all(  )` 解析

[正在上映-郑州](https://movie.douban.com/cinema/nowplaying/zhengzhou)
打开这个网页，检查源代码，按 `ctrl + F `，输入 `nowplaying `  ，找到 如下图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200613153251544.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
这个就是想要的数据，包含，电影名，评分，获赞数，演员表等信息

这个标签是 `div  id="nowplaying`下面的 `li class="list-item"`标签的内容

所以代码为

```python		 
# 导入库
from bs4 import BeautifulSoup
import urllib
from urllib import request

# 请求网页对象，获取源代码html_data
# Request函数将url添加到头部，模拟浏览器访问
headers = {
'User-Agent': 'Mozilla/5.0(Linux;android 6.0;Nexus 5 Build/MRA58N) APPLEWebkit/537.36(KHTML, like Gecko) Chrome/56.0.2924.87 Mobile Safari/537.36'
}
url = "https://movie.douban.com/cinema/nowplaying/zhengzhou/"
res = urllib.request.Request(url, headers=headers)
html_data = urllib.request.urlopen(res).read().decode('utf-8')
print(html_data)
# 解析网页
soup = BeautifulSoup(html_data, "html.parser")
#  寻找目标标签 <div id = "nowplaying">
nowplaying_movie = soup.find_all('div', id="nowplaying")
# nowplaying_movie_list = nowplaying_movie[0].find_all('li', class_="list-item")
# nowplaying_movie_list = nowplaying_movie
print(nowplaying_movie)
# print(nowplaying_movie_list)

```
其中请求的源代码html_data的格式

		res  = urllib.request.Request(url, headers=headers)
		html_data =  urllib.request.urlopen(res).read().decode('utf-8')
		# 即 
		请求后的对象 = urllib.reqeust.Request(  url   ,   headers= headers)
		# 用urllib.request库的Request模块请求
		页面源代码 = urllib.request.urlopen(res).read().decode('utf-8')
		# 用urllib.request库的urlopen模块打开网站url，打开之后用read()解析网页内容，用		decode('utf-8')转编码为正确格式，防止乱码，之后返回html_data源代码

`nowlplaying_movie_list`是包含所有电影的信息的列表

可以打印一下试试 

	print(nowplaying_movie_list)

```html
[ < li
class ="list-item" data-actors="罗伯托·贝尼尼 / 尼可莱塔·布拉斯基 / 乔治·坎塔里尼" 
data-category="nowplaying" data-director="罗伯托·贝尼尼"
data-duration="116分钟"
data-enough="True" data-region="意大利"
data-release="1997"
data-score="9.5" 
data-showed="True" 
data-star="50" 
data-subject="1292063"
data-title="美丽人生"
data-votecount="971249"
id="1292063" >

< ul


class ="" >

< li


class ="poster" >

< a
data - psource = "poster"
href = "https://movie.douban.com/subject/1292063/?from=playing_poster"
target = "_blank" >
< img
alt = "美丽人生"


class ="" rel="nofollow" src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2578474613.jpg" / >

< / a >
< / li >
< li


class ="stitle" >

< a


class ="" data-psource="title" href="https://movie.douban.com/subject/1292063/?from=playing_poster" target="_blank" title="美丽人生" >


美丽人生
< / a >
< / li >
< li


class ="srating" >

< span


class ="rating-star allstar50" > < / span >

< span


class ="subject-rate" > 9.5 < / span >

< / li >
< li


class ="sbtn" >

< a


class ="ticket-btn" href="https://movie.douban.com/ticket/redirect/?movie_id=1292063" target="_blank" >


选座购票
< / a >
< / li >
< / ul >
< / li >, < li


class ="list-item" data-actors="威尔·史密斯 / 汤姆·赫兰德 / 拉什达·琼斯" data-category="nowplaying" data-director="尼克·布鲁诺 特洛伊·奎安" data-duration="102分钟" data-enough="True" data-region="美国" data-release="2019" data-score="7.5" data-showed="True" data-star="40" data-subject="27000084" data-title="变身特工" data-votecount="47838" id="27000084" >

< ul


class ="" >

< li


class ="poster" >

< a
data - psource = "poster"
href = "https://movie.douban.com/subject/27000084/?from=playing_poster"
target = "_blank" >
< img
alt = "变身特工"


class ="" rel="nofollow" src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2577340942.jpg" / >

< / a >
< / li >
< li


class ="stitle" >

< a


class ="" data-psource="title" href="https://movie.douban.com/subject/27000084/?from=playing_poster" target="_blank" title="变身特工" >


变身特工
< / a >
< / li >
< li


class ="srating" >

< span


class ="rating-star allstar40" > < / span >

< span


class ="subject-rate" > 7.5 < / span >

< / li >
< li


class ="sbtn" >

< a


class ="ticket-btn" href="https://movie.douban.com/ticket/redirect/?movie_id=27000084" target="_blank" >


选座购票
< / a >
< / li >
< / ul >
< / li >, < li


class ="list-item" data-actors="迈克尔·佩纳 / 丽兹·卡潘 / 伊瑟尔·布罗萨德" data-category="nowplaying" data-director="本·扬" data-duration="93分钟(中国大陆)" data-enough="True" data-region="美国" data-release="2018" data-score="5.6" data-showed="True" data-star="30" data-subject="26871938" data-title="灭绝" data-votecount="13807" id="26871938" >

< ul


class ="" >

< li


class ="poster" >

< a
data - psource = "poster"
href = "https://movie.douban.com/subject/26871938/?from=playing_poster"
target = "_blank" >
< img
alt = "灭绝"


class ="" rel="nofollow" src="https://img1.doubanio.com/view/photo/s_ratio_poster/public/p2579512247.jpg" / >

< / a >
< / li >
< li


class ="stitle" >

< a


class ="" data-psource="title" href="https://movie.douban.com/subject/26871938/?from=playing_poster" target="_blank" title="灭绝" >


灭绝
< / a >
< / li >
< li


class ="srating" >

< span


class ="rating-star allstar30" > < / span >

< span


class ="subject-rate" > 5.6 < / span >

< / li >
< li


class ="sbtn" >

< a


class ="ticket-btn" href="https://movie.douban.com/ticket/redirect/?movie_id=26871938" target="_blank" >


选座购票
< / a >
< / li >
< / ul >
< / li >, < li


class ="list-item" data-actors="石川由依 / 茅原实里 / 远藤绫" data-category="nowplaying" data-director="藤田春香" data-duration="91分钟" data-enough="True" data-region="日本" data-release="2019" data-score="7.5" data-showed="True" data-star="40" data-subject="33424345" data-title="紫罗兰永恒花园外传：永远与自动手记人偶" data-votecount="31004" id="33424345" >

< ul


class ="" >

< li


class ="poster" >

< a
data - psource = "poster"
href = "https://movie.douban.com/subject/33424345/?from=playing_poster"
target = "_blank" >
< img
alt = "紫罗兰永恒花园外传：永远与自动手记人偶"


class ="" rel="nofollow" src="https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2578722076.jpg" / >

< / a >
< / li >
< li


class ="stitle" >

< a


class ="" data-psource="title" href="https://movie.douban.com/subject/33424345/?from=playing_poster" target="_blank" title="紫罗兰永恒花园外传：永远与自动手记人偶" >


紫罗兰永恒花园...
< / a >
< / li >
< li


class ="srating" >

< span


class ="rating-star allstar40" > < / span >

< span


class ="subject-rate" > 7.5 < / span >

< / li >
< li


class ="sbtn" >

< a


class ="ticket-btn" href="https://movie.douban.com/ticket/redirect/?movie_id=33424345" target="_blank" >


选座购票
< / a >
< / li >
< / ul >
< / li >, < li


class ="list-item" data-actors="尼娅·朗 / 约翰·考伯特 / 苏菲·奈丽丝" data-category="nowplaying" data-director="约翰内斯·罗伯茨" data-duration="89分钟" data-enough="True" data-region="英国" data-release="2019" data-score="5.1" data-showed="True" data-star="25" data-subject="27186353" data-title="鲨海逃生" data-votecount="10068" id="27186353" >

< ul


class ="" >

< li


class ="poster" >

< a
data - psource = "poster"
href = "https://movie.douban.com/subject/27186353/?from=playing_poster"
target = "_blank" >
< img
alt = "鲨海逃生"


class ="" rel="nofollow" src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2578721161.jpg" / >

< / a >
< / li >
< li


class ="stitle" >

< a


class ="" data-psource="title" href="https://movie.douban.com/subject/27186353/?from=playing_poster" target="_blank" title="鲨海逃生" >


鲨海逃生
< / a >
< / li >
< li


class ="srating" >

< span


class ="rating-star allstar25" > < / span >

< span


class ="subject-rate" > 5.1 < / span >

< / li >
< li


class ="sbtn" >

< a


class ="ticket-btn" href="https://movie.douban.com/ticket/redirect/?movie_id=27186353" target="_blank" >


选座购票
< / a >
< / li >
< / ul >
< / li >]

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200614133907884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200614133959280.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
如图所示，`nowplaying_movie_list`存放所有电影的信息的列表

从中可知 `id`为电影的id，`img`标签的alt属性存放电影的名字

再打开短评，需要获取`id`,所以需要解析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200614134804383.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

经过分析可以知道，`nowplaying_movie_list`列表中每一个元素是某个电影的信息的`li`标签，要找标签中的属性的值
```py
nowplaying_list = []    #
for item in nowplaying_movie_list:
    nowplaying_dict = {}      # 以字典的形式储存每一步电影的id和电影名
    nowplaying_dict['id'] = item['data-subject']     # item为单个电影的信息，从中找data-subject属性，放入字典，作为id键值
    for tag_img_item in item.find_all('img'):  #找img标签
        nowplaying_dict['name'] = tag_img_item['alt']      # 中的alt属性的值，作为字典的name
        nowplaying_list.append(nowplaying_dict)  存入列表中
```
打印`nowplaying_list`

```py
[{'id': '1292063', 'name': '美丽人生'},
{'id': '27000084', 'name': '变身特工'},
{'id': '26871938', 'name': '灭绝'},
{'id': '33424345', 'name': '紫罗兰永恒花园外传：永远与自动手记人偶'},
{'id': '27186353', 'name': '鲨海逃生'}]
```
>获取一个电影的id和name，作为字典，获取所有的字典作为列表


**接下来对电影的`短评`分析**

[短频网址](https://movie.douban.com/subject/1292063/comments?status=P)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200614140037643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

`https://movie.douban.com/subject/26861685/comments?start=0&limit=20` 

**其中**

-  `26861685` 为美丽人生电影的`ID`
- `start = 0 ` 表示第一条评论
- `limit=20` 表示一次最多显示`20`条
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200614140827507.png)
查看页面源代码，可以知道，电影的短评在`div`为 `comment`的标签下,以`美丽人生`为例

如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200614140224171.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)


因此对这个标签进行解析

```py
# 对影评解析
reurl='https://movie.douban.com/subject/' + nowplaying_list[0]['id'] + '/comments' + '?' + 'start=0' +'&limit=20'
resp=urllib.request.Request(reurl,headers=headers)
html=urllib.request.urlopen(resp).read().decode('utf-8')
soup=BeautifulSoup(html,'html.parser')
comment_div_lists=soup.find_all('div',class_='comment')     #找class=comment的div标签

```
> comment_div_lists存放的是所有的class='commnet'的div标签

根据上图继续分析，短评在`<p class=""></p>` 中 的`span`标签内

继续解析

```py
eachCommentList=[]
for item in comment_div_lists:
    b=item.find('p').find('span')    #获取p标签内的短评
    if b.string is not None:        # b.string中的string表示b标签的内容
        eachCommentList.append(b.string)
```
`标签.string`表示标签的文本

打印 `eachCommentList`  的内容，即短评

```py
['我以为如此智慧的一个人，在那几声枪响过后，必定是会走出来，继续对他的公主说早安的...',
'如果谎言可以这样美丽，我也情愿生活在谎言之中',
'人生，就是喜剧，就是一场1000分的游戏。千万别哭，不然那个迈着正步去赴死的犹太人会笑话你的。\r\n \r\n', 
'看了这个不要看《辛德勒名单》或者看了《辛德勒名单》不要看这个。时间：2005',
'喜剧的开始，悲剧的结束，\r\n满满的亲情，无限的父爱', 
'绝对难忘的一部片子----影片结尾美国大兵的那句 hei! boy! 叫人已经凉透了的心又感到了温暖。', 
'父爱构筑了一场温馨的战争，没有在孩子心里留下任何阴影。故事分裂成两部分，前半是圭多对公主的追逐，用智慧战胜了强大的求婚者，抱得美人归；后半是战争风雨飘摇时对亲情的忠诚，在迈开大正步赴死，努力保护孩子时，父爱如山，界已成神.',
'即使是悲惨世界,也要大大的笑着.', '很久以前看过的，永远难忘片中父亲的鬼脸', '不敢相信男主死了，总以为他能活过来', '对我说：“早上好，公主”吧', '三刷了。记得上学时跟父母吵过，中国的父母经常想让小孩相信这世界的丑陋，而国外的父母即便身在地狱也要让孩子相信在天堂，让他们开心的活着，你们为什么要让我的童年的这么痛苦。哈哈，大概就是受这剧影响吧。', '看过很多遍，我会背很多台词：我想和你做爱，我见你的第一眼就想和你连续做三次爱。如果你违反了三条规定中的任何一条，你的得分就会被扣光：一、如果你哭，二、如果你想要见妈妈，三、如果你饿了，想要吃点心！想都别想！第一句爱情性感，第二句父爱如山 \r\n最爱的还是那句：早安，公主！', '这是父亲所作的牺牲，这是父亲赐我的恩典。★★★★☆', '世界上最美丽的谎言与微笑。Surprise her, forever!', '从天而降的钥匙打开了她的心房，却打不开奴役的枷锁；\n骑马逃离婚姻的羁绊，却逃不出法西斯的束缚。\n现实残酷，他就用温柔回敬现实。\n战争凶猛，他就用父爱打赢这场战争。\n做一块蛋糕，他就融化了她的心窝；\n铺一张地毯，她就走进了他的生活。\n一句广播，一首音乐，让妻子重新看见希望。\n一个善意的谎言，一个属于父子之间的游戏，给孩子一个没有阴影的童年。\n我只是一个普通人，你却让我做了公主。\n我只是想要一个玩具坦克，你却让我坐上了真坦克。\n他默默无闻，但他又无所不能。\n纵然身在地狱，他也能将它变成天堂。\n以生命为代价，给家人一个美丽人生。', '关于父爱的伟大电影。以非凡的想象力和诙谐幽默演绎一场不堪回首的历史惨剧，那怦动的热情和对人生充满希望的美丽震撼人心。“为了看到阳光，我们来到世上。为了成为阳光，我们存于世上。”在Guido身上，你看不到那些痛苦、隐忍、挣扎和艰难。这位一直用荒谬的态度对待人生的荒谬、以达观的态度对', '前后如同两部电影。前一半有多欢脱，后一半就有多苍凉。我特别在意那位执着与于猜谜的军官，善与恶，竟然最终抵不过一道谜题，这也是一种人生的况味吧。', '该给 1000 分的电影', '最温柔的谎言。']

```
` b.string`就是`span.string`表示获取span标签的内容

## 进行数据清洗

数据清洗是去除与数据分析无关的信息

将列表中数据放入`字符串`中

```py
# 数据清洗

comments=''
for k in range(len(eachCommentList)):
    comments=comments + (str(eachCommentList[k])).strip()

```

`strip()` ?

打印`comments`如下

```py
我以为如此智慧的一个人，在那几声枪响过后，必定是会走出来，继续对他的公主说早安的...如果谎言可以这样美丽，我也情愿生活在谎言之中喜剧的开始，悲剧的结束，
满满的亲情，无限的父爱绝对难忘的一部片子----影片结尾美国大兵的那句 hei! boy! 叫人已经凉透了的心又感到了温暖。父爱构筑了一场温馨的战争，没有在孩子心里留下任何阴影。故事分裂成两部分，前半是圭多对公主的追逐，用智慧战胜了强大的求婚者，抱得美人归；后半是战争风雨飘摇时对亲情的忠诚，在迈开大正步赴死，努力保护孩子时，父爱如山，界已成神.即使是悲惨世界,也要大大的笑着.很久以前看过的，永远难忘片中父亲的鬼脸不敢相信男主死了，总以为他能活过来对我说：“早上好，公主”吧三刷了。记得上学时跟父母吵过，中国的父母经常想让小孩相信这世界的丑陋，而国外的父母即便身在地狱也要让孩子相信在天堂，让他们开心的活着，你们为什么要让我的童年的这么痛苦。哈哈，大概就是受这剧影响吧。看过很多遍，我会背很多台词：我想和你做爱，我见你的第一眼就想和你连续做三次爱。如果你违反了三条规定中的任何一条，你的得分就会被扣光：一、如果你哭，二、如果你想要见妈妈，三、如果你饿了，想要吃点心！想都别想！第一句爱情性感，第二句父爱如山 
最爱的还是那句：早安，公主！世界上最美丽的谎言与微笑。Surprise her, forever!从天而降的钥匙打开了她的心房，却打不开奴役的枷锁；
骑马逃离婚姻的羁绊，却逃不出法西斯的束缚。
现实残酷，他就用温柔回敬现实。
战争凶猛，他就用父爱打赢这场战争。
做一块蛋糕，他就融化了她的心窝；
铺一张地毯，她就走进了他的生活。
一句广播，一首音乐，让妻子重新看见希望。
一个善意的谎言，一个属于父子之间的游戏，给孩子一个没有阴影的童年。
我只是一个普通人，你却让我做了公主。
我只是想要一个玩具坦克，你却让我坐上了真坦克。
他默默无闻，但他又无所不能。
纵然身在地狱，他也能将它变成天堂。
以生命为代价，给家人一个美丽人生。关于父爱的伟大电影。以非凡的想象力和诙谐幽默演绎一场不堪回首的历史惨剧，那怦动的热情和对人生充满希望的美丽震撼人心。“为了看到阳光，我们来到世上。为了成为阳光，我们存于世上。”在Guido身上，你看不到那些痛苦、隐忍、挣扎和艰难。这位一直用荒谬的态度对待人生的荒谬、以达观的态度对该给 1000 分的电影最温柔的谎言。美丽的灵魂成就美丽的人生我爸爸是四核的超人后来他成了王子还有传说。早安！公主！
```
会发现，还有标点符号，要去除标点符号

使用`re`正则表达式来去除

```py
# 继续去除标点符号
import re
pattern=re.compile(r'[\u4e00-\u9fa5]+')
filterdata=re.findall(pattern, comments)
cleaned_comments=''.join(filterdata)
```
>这段的方法和用法不熟悉

继续打印 

```py
我以为如此智慧的一个人在那几声枪响过后必定是会走出来继续对他的公主说早安的人生就是喜剧就是一场分的游戏千万别哭不然那个迈着正步去赴死的犹太人会笑话你的如果谎言可以这样美丽我也情愿生活在谎言之中喜剧的开始悲剧的结束满满的亲情无限的父爱看了这个不要看辛德勒名单或者看了辛德勒名单不要看这个时间绝对难忘的一部片子影片结尾美国大兵的那句叫人已经凉透了的心又感到了温暖父爱构筑了一场温馨的战争没有在孩子心里留下任何阴影故事分裂成两部分前半是圭多对公主的追逐用智慧战胜了强大的求婚者抱得美人归后半是战争风雨飘摇时对亲情的忠诚在迈开大正步赴死努力保护孩子时父爱如山界已成神即使是悲惨世界也要大大的笑着很久以前看过的永远难忘片中父亲的鬼脸不敢相信男主死了总以为他能活过来对我说早上好公主吧三刷了记得上学时跟父母吵过中国的父母经常想让小孩相信这世界的丑陋而国外的父母即便身在地狱也要让孩子相信在天堂让他们开心的活着你们为什么要让我的童年的这么痛苦哈哈大概就是受这剧影响吧看过很多遍我会背很多台词我想和你做爱我见你的第一眼就想和你连续做三次爱如果你违反了三条规定中的任何一条你的得分就会被扣光一如果你哭二如果你想要见妈妈三如果你饿了想要吃点心想都别想第一句爱情性感第二句父爱如山最爱的还是那句早安公主这是父亲所作的牺牲这是父亲赐我的恩典世界上最美丽的谎言与微笑从天而降的钥匙打开了她的心房却打不开奴役的枷锁骑马逃离婚姻的羁绊却逃不出法西斯的束缚现实残酷他就用温柔回敬现实战争凶猛他就用父爱打赢这场战争做一块蛋糕他就融化了她的心窝铺一张地毯她就走进了他的生活一句广播一首音乐让妻子重新看见希望一个善意的谎言一个属于父子之间的游戏给孩子一个没有阴影的童年我只是一个普通人你却让我做了公主我只是想要一个玩具坦克你却让我坐上了真坦克他默默无闻但他又无所不能纵然身在地狱他也能将它变成天堂以生命为代价给家人一个美丽人生关于父爱的伟大电影以非凡的想象力和诙谐幽默演绎一场不堪回首的历史惨剧那怦动的热情和对人生充满希望的美丽震撼人心为了看到阳光我们来到世上为了成为阳光我们存于世上在身上你看不到那些痛苦隐忍挣扎和艰难这位一直用荒谬的态度对待人生的荒谬以达观的态度对前后如同两部电影前一半有多欢脱后一半就有多苍凉我特别在意那位执着与于猜谜的军官善与恶竟然最终抵不过一道谜题这也是一种人生的况味吧该给分的电影最温柔的谎言
```

像这样就是把`标点符号`去除了的结果

因为要统计词频，所以要进行分词操作

**分词**

使用`jieba`库，如果没有，可以`pip install jieba`

中文分词如下

```py
# 中文分词

import jieba.analyse
# 使用jieba 分词 进行中午分词
result=jieba.analyse.textrank(cleaned_comments,topK=50,withWeight=True)
keywords=dict()
for i in result:
    keywords[i[0]]=i[1]
```

print(keywords)

```py
删除停用词前 {'美丽': 1.0, '父爱': 0.8830777456454413, '谎言': 0.7786033548519086, '孩子': 0.7168316732302272, '人生': 0.649244921094173, '战争': 0.529481153617465, '父母': 0.48543462568908613, '父亲': 0.4799638561702853, '想要': 0.43529316706932325, '公主': 0.42029415433150763, '难忘': 0.3915035260881966, '生活': 0.3832842192583704, '天堂': 0.365104031781331, '赴死': 0.36264754503661806, '亲情': 0.3328760837734129, '阴影': 0.3070161959663855, '没有': 0.304165317901892, '骑马': 0.30348806349071006, '现实': 0.28731475734353146, '爱情': 0.2859044857200335, '性感': 0.28362084833835344, '我会': 0.28268177024462215, '逃不出': 0.27117870221955154, '片子': 0.2694383584662726, '影片': 0.2687014448013604, '坦克': 0.2644919045699723, '奴役': 0.2614365678967113, '留下': 0.26096209729542225, '羁绊': 0.2578515439645892, '枷锁': 0.25576659745186503, '逃离': 0.25411073287633207, '喜剧': 0.2535910077032035, '婚姻': 0.252870119059615, '吵过': 0.24924502642983754, '点心': 0.245465595182439, '想都别想': 0.24473832485495478, '成为': 0.2446760335462939, '历史': 0.24248549647599624, '看过': 0.2409085687080437, '隐忍': 0.23913525582456638, '违反': 0.23906629006591645, '扣光': 0.23906629006591645, '生命': 0.23890999603124521, '看不到': 0.23801413755656795, '规定': 0.2373088953878489, '得分': 0.2373088953878489, '挣扎': 0.2367659255924238, '灵魂': 0.23471319779481464, '成就': 0.23393348089443175, '追逐': 0.23240924504269359}

```

从这个打印结果，可以看出已经进行词频统计了

，但是数据还有虚词 `太`,`也`,还要去除

本程序把停用词放入`stopwords.txt`中，将数据于停用词对比

```py
# 删除停用词代码

keywords={x:keywords[x] for x in keywords if x not in stopwords}
print("删除停用词后",keywords)

```

打印结果
```py

```

## 用词云展示
```py
# 用词云展示-----------------
import matplotlib.pyplot as plt
import matplotlib
matplotlib.rcParams['figure.figsize']=(10.0,5.0)
from wordcloud import WordCloud       # 词云包
# 指定字体类型，字体大小，字体颜色
wordcloud=WordCloud(font_path="simhei.ttf",background_color="white",
max_font_size=80,stopwords=stopwords)
word_frequence=keywords
myword=wordcloud.fit_word(word_frequence)
plt.imshow(myword)      #展示词云
plt.axis("off")
plt.show()
```

其中`simhei.ttf`用来指定字体，可以百度搜索下载使用，然后放入程序的`根目录`


**完整的程序代码**

```py

 
```


#  娱乐游戏---人物拼图

## 关键技术

### 复制和粘贴图像区域

`crop( )` 可以从一张图片裁剪指定区域

```py
from PIL import Image
im=Image.open("E:\\4.jpg")    # 用Image.open()打开图片
box=(100, 100, 400, 400)      # 坐标
region=im.crop(box)          # 根据图片和坐标裁剪

```
box=(左，上，右，下) 表示四元组

PIL指定坐标系左上角为`(0,0)`   ,可以旋转，使用`paste()`方法，将该区域放上去

```py
region=region.transpose(Image.ROTATE_180)     #逆时针旋转180°
im.paste(region,box) #      (原图片，坐标)
```

### 调整尺寸和旋转
`resize( )` 用于调整图片，参数为元组，用于指定新的图像的大小

		out=im.resize((128,128))

`rotate( )` 用于旋转图像,逆时针

		out=im.rotate(45)

### 转换成灰度图像
对于彩色图像，无论格式 : `PNG`,`BMP`,`JPG`,在 PIL中使用的`Image`模块的`open()`打开，返回图像对象都是`RGB`

而对于灰色图像，返回`L`

彩色图像格式之间的转换通过`open()` 和`save()` 函数解决，简单说，打开图片,`PIL`会将图像解码为**三通道**的`RGB`模式，用户经过修之后，保存，即可得到格式后的图像

对于灰度也可以采用以上方法，只不过，最后的图像对象是`L`

Image模块`convert()`函数，用于不同模式图像转换
```py
im.convert(mode)
im.convert('p' , **options)
im.convert(mode, matrix)
```
>convert()函数有三种形式定义

使用不同参数，将图像转化为新的模式，`PIL`中有9中不同的模式 `1,L,P,RGB,RGBA,CMYK,YCbCr,I,F`产生新图像作为返回值

如 
```py
from PIl importImage    #或直接import Image
im=Image.open('a.jpg')     #打开图片返回图片对象 im
im1=im.convert("L")        # 将图片转为灰度
```
>你想变成什么就 `convert(什么)`

`L`模式灰图像，每像素`8bit`,0为`黑`，255为`白`，其他数字表不同程度灰度，在`PIL`，`RGB`转`L`的公式过程

$L=R*299/1000+G*587/1000+B*114/1000$

**打开图片转灰度方法**
1. 打开图片 `im=Image.open(''a.jpg)`
2. 转换 `im1=im.convert("L")`


如要转黑白(二值)，非黑即白,即模式`"1"`

```py
from PIL import Image
im=Image.open('a.jpg')
im1=im.convert("1")
```
这样就将彩色图片转为了`黑白`

### 对像素操作

`getpixel(x, y)`获指定像素色，如像为多通道，返回一`元组`,此法慢

如需处理大数据，用python的`load()`或`getdata()`

`putpixel(xy, color)`可改变单像素点颜色

```py
img=Image.open("smallimg.png")    #打开图片
img.getpixel((4,4))              # 获取像素色
img.putpixel((4,4),(255,0,0))    # 改单像素点为红色
img.save("img1.png","png")
```
>`getpixel(4,4)`表示获取这像素点，`(255,0,0)`将像素点色变为`红色`


## 程序设计步骤

### 图片切割

使用`PIL库`的`Image模块`的`crop()方法`指定裁剪区域

区域使用四元组形式`(左， 上， 右， 下)`

```py
from PIL import Image
img=Image.open('4.jpg')  #打开图片
box=(100,100,400,400)
# 切割图片
region=omg.crop(box)
# 保存切割后的图片
region.save('crop.jpg')  #命名为crop图片
```

本游戏切割为`3x3`图片块，为更方便的，可以使用`splitimage(src, rownumn, colnum, dstpath)` 函数，实现指定`src `图片切割为`rownumxcolnum`数量小图片块

**【人物拼图】**

**具体代码实现(`可行`)**

```py
import os
from PIL import Image


def splitimage(src, rownum, colnum, dstpath):
    img = Image.open(src)
    w, h = img.size
    if rownum <= h and colnum <= w:
        print('Original image info: %sx%s, %s, %s' % (w, h, img.format, img.mode))
        print('开始处理图片切割, 请稍候...')

        s = os.path.split(src)  # 分割源文件名和路径
        if dstpath == '':
            dstpath = s[0]
            print(dstpath)
        fn = s[1].split('.')
        basename = fn[0]
        ext = fn[-1]

        num = 0
        rowheight = h // rownum
        colwidth = w // colnum
        for r in range(rownum):
            for c in range(colnum):
                box = (c * colwidth, r * rowheight, (c + 1) * colwidth, (r + 1) * rowheight)
                img.crop(box).save(os.path.join(dstpath, basename + '_' + str(num) + '.' + ext))
                num = num + 1

        print('图片切割完毕，共生成 %s 张小图片。' % num)
    else:
        print('不合法的行列切割参数！')


src = input('请输入图片文件路径：')
# src = "c:\woman.png"
if os.path.isfile(src):
    dstpath = input('请输入图片输出目录（不输入路径则表示使用源图片所在目录）：')

    if (dstpath == '') or os.path.exists(dstpath):
        row = int(input('请输入切割行数：'))
        col = int(input('请输入切割列数：'))
        if row > 0 and col > 0:
            splitimage(src, row, col, dstpath)
        else:
            print('无效的行列切割参数！')
    else:
        print('图片输出目录 %s 不存在！' % dstpath)
else:
    print('图片文件 %s 不存在！' % src)

```
**上机测试代码`未成功`**
```py
import os
from PIL import Image
def splitimgae(src,rownum,colnum,dstpath):  # 切割图片函数
    img=Image.open(src) #打开
    w, h=img.size    #获取图片尺寸
    if rownum<=h and colnum<=w:
        print('原始图片信息是: %sx%s, %s, %s'%(w,h,img.format,img.mode))  #打印原始信息
        print('开始处理图片切割，请稍后...')
        s=os.path.split(src)    # 判断这个路径是否存在
        if dstpath=='': # 没有输入路径
            dstpath=s[0]    # 使用源文件目录s[0]
        fn=s[1].split('.')  # s[1] 是原图片文件名
        basename=fn[0]  # 主文件名
        ext=fn[-1] # 扩展名
        num=0
        rowheight=h # rownum
        colwidth=w  # colnum
        for r in range(rownum):
            for c in range(colnum):
                box=(c*colwidth,r*rowheight,(c+1)*colwidth,(r+1)*rowheight)
                img.crop(box).save(os.path.join(dstpath,basename+'_'+str(num)+'.'+ext))
                num=num+1
        print('图片切割完毕,共生成%s张图片'%num)
    else:
        print('不合法的行列切割参数')

# ------------------------------------------------------
src=input("请输入文件路径:")   # 如 C:\woman.png      原图片
if os.path.isfile(src):
    dstpath=input('请输入图片的输出目录(不输入路径表示使用原图片所在路径)')
    if (dstpath=='') or os.path.exists((dstpath)):  #如果存在或不存在
        row=int(input("请输入切割行数:"))
        col=int(input("请输入切割列数:"))
        if row>0 and col>0:
            splitimgae(src,row,col,dstpath) #调用切割图片函数
        else:
            print("无效的行列切割参数")
    else:
        print("图片输出目录%s不存在"%dstpath)
else:
    print("图片文件%s不存在"%src)

```

运行结果
```ppy
请输入文件路径:>? E:\test\4.jpg
请输入图片的输出目录(不输入路径表示使用原图片所在路径)>? 
请输入切割行数:>? 4
请输入切割列数:>? 4
原始图片信息是: 1200x1601, JPEG, RGB
开始处理图片切割，请稍后...
开始打印了:
rownum:4,colnum:4
图片切割完毕,共生成16张图片	
```
`出现效果错误`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200616140720985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

>图片没有每个成功
>
`错误分析`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200616140422970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)
>因为代码错误，导致图片没有分割

`部分代码分析`


	 print("fn：")
     print(fn)

打印

	fn：
	['4', 'jpg']

说明`split(".")`表示把`.`切割出去，后把`文件名`和`后缀名`作为一个列表
- -- 

	print("s：")
    print(s)

打印

	s：
	('E:\\test', '4.jpg')

说明 ` s=os.path.split(src)  `用于把`路径`和`文件名.后缀名`切割为元组

`s[0]` : 路径   

`s[1]`: 文件名.后缀名
- ---

	fn=s[1].split('.')  # s[1] 是原图片文件名.后缀名      
    basename=fn[0]  # 主文件名
    ext=fn[-1] # 扩展名
	print("ext:")
    print(ext)

打印
		

		ext:
		jpg
`fn[-1]`就表示`['4', 'jpg']` 中的倒数第一个元素，即`jpg`



### **游戏逻辑实现**
**1    . 定义常量加载图片**

```py
from tkinter import *
from tkinter.messagebox import *
import random
# ------------------------
# 定义常量
WIDTH=312
HEIGHT=450
# 图像块的边长
IMAGE_WIDTH=WIDTH//3    # 整除3
IMAGE_HEIGHT=HEIGHT//3
# 游戏的行列数
ROWS=3
COLS=3
# 移动步数
steps=0
# 保存所有图像块的列表
board=[[0,1,2],
       [3,4,5],
       [6,7,8]]
# 创建窗口
root=Tk("拼图2020")   # 创建窗口
root.title("拼图")
# 载入外部实现生成的图像块
Pics=[]
for i in range(9):
    filename="woman_"+str(i)+".png" #
    Pics.append(PhotoImage(file=filename))
```

**2   . 图像块类**

每一图像块是`Square`对象，有`draw`功能，将拼图绘制到`canvas`上，`orderID`是每一个拼图对应的编号
```py
class Square:
    def __init__(self,orderID):     # 初始化，分配对象
        self.orderID=orderID
    def draw(self,canvas,board_pos):
        img=Pics[self.orderID]  # 传入图片到img
        canvas.create_image(board_pos,image=img)    # 画图片到画布
    
```

**3	. 初始化游戏**

`random.shuffle(board)` 打乱二维列表只能按行，所以要按一维列表打乱，根据编号生成对应的图像块

>跟图片块`不同`的编号，代表其显示的位置坐标，达到打乱的目的

```py
def init_board():
    # 打乱图像块
    L=List(range(9))    # L列表中的[01,2,3,4,5,6,7,8]
    random.shuffle(L)   # 表示打乱L列表中的顺序
    # 填充拼图版
    for i in range(ROWS):
        for j in range(COLS):
            ix=i*ROWS+j
            orderID=L[idx]
            if orderID is 8:    # 8号拼图块不显示，所以存为None
                board[i][j]=None
            else:
                board[i][j]=Square(orderID)


```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200616112930926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

> 因为对`二维列表`排序是`对行`排序，所以是要把二维列表看成一维列表排序，才有可能对每个元素排序

**4	. 绘制游戏界面各元素**

```py
def drawBoard(canvas):
    # 画黑框
    canvas.create_polygon((0,0,WIDTH,0,WIDTH,HEIGHT,0,HEIGHT),width=1,outline='Black')
    # 画所有图像
    for i in range(ROWS):
        for j in range(COLS):
            if board[i][j] is not None:
                board[i][j].draw(canvas,(IMAGE_WIDTH*(j+0.5)),IMAGE_HEIGHT*(i+0.5))     # 绘制
                
```

>`canvas`的使用？

5	. 鼠标使用

点击空位置，不移动，如点击有图像位置，则和空位置对换
```py
def mouseclick(pos):
    global steps
    # 将单击位置，换为棋盘坐标
    r=int(pos.y//IMAGE_HEIGHT)  # y坐标
    c=int(pos.x//IMAGE_WIDTH)   # x坐标
    if r<3 and c<3: # 表示单机位置在图片块内
        if board[r][c] is None  # 单机为空位置，则不移动
            return 
        else:
        # 一次检查单击位置的上左下右是否右空位置，有则移动当前图片块
            current_square=board[r][c]
            if r-1>0 and board[r-1][c] is None: # 判断上面
                board[r][c]=None
                board[r-1][c]=current_square
                steps+=1
                # 完成一次调换，步数加一
            elif c+1<=2 and board[r][c+1] is None:  # 判断右面
                board[r][c]=None
                board[r][c+1]=current_square
                steps+=1
            elif r + 1 > 0 and board[r+1][c ] is None:  # 判断下面
                board[r][c] = None
                board[r+1][c] = current_square
                steps += 1
            elif c - 1> 0 and board[r][c - 1] is None:  # 判断左面
                board[r][c] = None
                board[r][c - 1] = current_square
                steps += 1
            # print(board) 
            label1["text"] = "步数:" + str(steps)
            cv.delete('all')    # 清除画布上的内容
            drawBoard(cv)
    if win():
        showinfo(title="恭喜",message="你成功了!")
    

```
>要知道如何进行换位置的算法，搞清楚`row`,`col`，一个是行，一个是列，如果是行变化，即`纵坐标变换`
>同样的如是列变换，即`横坐标变换`

7	.输赢的判断

即判断图片的编号是否是有序的，有序证明拼好了，即赢了，否则，输了

赢了，返回`True` ; 输了返回 `Flase`

```py
def win():
    for i in range(ROWS):
        for j in range(COLS):
            if board[i][j] is not None and board[i][j].orderID!=i*ROWS+j:   # 如果顺序不一致，则返回Flase，即输了
                return False
            
    return True #   否则赢了
```


8	.重置游戏
```py
def play_game():
    global steps
    steps=0
    init_board()
    
```

9	.重新开始按钮单击事件

```py
def callBack2():
    print("重新开始")
    play_game()
    cv.delete('all')    # 清除画布上的内容
    drawBoard(cv)
```


10	.**主程序**

```py
# 设置窗口
cv=Canvas(root,bg='green', width=WIDTH,height=HEIGHT)
b1=Button(root,text="重新开始",command=callBack2(),width=20)   #点击这个按钮，就重新开始
label1=Label(root,text="步数:"+str(steps),fg="red",width=20)   # 用于实时显示走过的步数
label1.pack()
cv.bind("<Button-1",mouseclick())
cv.pack()
b1.pack()
play_game()
drawBoard(cv)
root.mainloop()


```

**最后运行效果**
### **总代码(可行)**
**总代码**
```py
from tkinter import *
from tkinter.messagebox import *
import random


root = Tk('拼图2017')
root.title(" 拼图--夏敏捷2017-10-5")
# 载入外部图像 
Pics = []
for i in range(9):
   filename="woman_"+str(i)+".png"
   Pics.append(PhotoImage(file=filename))
# 定义常量
# 画布的尺寸
WIDTH = 312 
HEIGHT = 450 
# 图像块的边长 
IMAGE_WIDTH = WIDTH // 3
IMAGE_HEIGHT = HEIGHT // 3 

# 棋盘行列数
ROWS = 3
COLS = 3 
# 移动步数
steps = 0 
#  保存所有图像块的列表
board = [[0, 1, 2],
         [3, 4, 5],
         [6, 7, 8]]

# 图像块类
class Square:  
    def __init__(self, orderID):
        self.orderID = orderID 
    def draw(self, canvas, board_pos):
        img = Pics[self.orderID]
        canvas.create_image(board_pos, image=img) 
# 初始化拼图板
def init_board(): 
    # 打乱图像块坐标
    L=list(range(8))
    L.append(None)      
    random.shuffle(L)
    # 填充拼图板 
    for i in range(ROWS): 
      for j in range(COLS):
           idx = i * ROWS + j 
           orderID = L[idx]
           if orderID is None:
               board[i][j] = None
           else:
               board[i][j] = Square(orderID) 
# 重置游戏 
def play_game():
    global steps 
    steps = 0
    init_board()
 
# 绘制游戏界面各元素
def drawBoard(canvas):
    # 画黑框 
    canvas.create_polygon((0, 0, WIDTH, 0, WIDTH, HEIGHT, 0, HEIGHT),width=1,outline='Black',fill='green')
    # 画图像块 
    # 代码写在这里
    for i in range(ROWS):
         for j in range(COLS):
             if board[i][j] is not None:
                  board[i][j].draw(canvas, (IMAGE_WIDTH*(j+0.5),IMAGE_HEIGHT*(i+0.5) )) 
def mouseclick(pos):
    global steps 
    # 将点击位置换算成拼图板上的坐标
    r = int(pos.y // IMAGE_HEIGHT)
    c = int(pos.x // IMAGE_WIDTH)
    print(r,c)
    if r < 3 and c < 3:   # 点击位置在拼图板内才移动图片
        if board[r][c] is None: # 点到空位置上什么也不移动
            return
        else: 
            # 依次检查当前图像块的上,下,左,右是否有空位置，如果有就移动当前图像块
            current_square = board[r][c] 
            if r - 1 >= 0 and board[r-1][c] is None:     # 判断上面
                board[r][c] = None 
                board[r-1][c] = current_square
                steps += 1 
            elif c + 1 <= 2 and board[r][c+1] is None:    # 判断右面
                board[r][c] = None 
                board[r][c+1] = current_square
                steps += 1 
            elif r + 1 <= 2 and board[r+1][c] is None:    # 判断下面
                board[r][c] = None 
                board[r+1][c] = current_square
                steps += 1 
            elif c - 1 >= 0 and board[r][c-1] is None:    # 判断左面
                board[r][c] = None 
                board[r][c-1] = current_square
                steps += 1
            #print(board)
            label1["text"]=str(steps)
            cv.delete('all')  #清除canvas画布上的内容
            drawBoard(cv)
    if win():
       showinfo(title="恭喜",message="你成功了！")
            
def win():
   for i in range(ROWS):
         for j in range(COLS):
            if board[i][j] is not None  and   board[i][j].orderID!=i * ROWS + j:
                return False
   return True
            
def callBack2():
   print("重新开始")
   play_game()
   cv.delete('all')  #清除canvas画布上的内容
   drawBoard(cv)


# 设置窗口
cv = Canvas(root, bg = 'white', width =WIDTH, height = HEIGHT)
b1=Button(root,text="重新开始",command=callBack2,width=20)
label1=Label(root,text="0" ,fg="red",width=20)
label1.pack()
cv.bind("<Button-1>", mouseclick)

cv.pack()
b1.pack()
play_game()
drawBoard(cv)
root.mainloop()









```

# 基于pgame游戏设计
[PyCharm安装Pygame方法](https://www.cnblogs.com/LHJL8023/p/8441016.html)

### **pygame模块**

pygame软件包中模块

模块名 | 功能 | 示例
-- | -- | -- 
`pygame.display` | 访问显示设备 | 
`pygame.draw` | 绘制形状，线，点|
`pygame.event`| 管理事件| 
`pygame.font` | 使用字体|
`pygame.image`| 加载和储存图片|
`pygame.key`| 读取键盘按键|
`pygame.mixer`| 声音|
`pygame.mouse`| 鼠标|
`pygame.music`| 播放音频|
`pygame` | Python模块，专为电子游戏设计|
`pygame.rect` | 管理矩形区域|
`pygame.sprite`| 操作移动图像|
`pygame.surface`| 管理图像和屏幕|
`pygame.time`| 管理时间和帧信息| 
pygame.transform | 缩放和移动图像|
pygame.cursors| 加载光标|
pygame.joystick| 使用游戏手柄或类似的东西|
pygame.overlay|高级视频叠加|
pygame.sndarry|操作声音数据|
pygame.surfarry| 管理点阵图像数据|
pygame.movie| 播放视频|
pygame.cdrom | 访问光驱|

>`红色`标记的是常用模块

- ---


导入`pygame`模块

	import pygame.sys,time,random
	from pygame.locals import  *

第一行是调用主要的模块

第二行是调用`pygame.locals`下的所有模块

测试字体是否载入
	
	if pygame.font is None:
		print("字体模块不可用!")
		pygame.quit()		#如果没有则退出



## pygame使用

### pygame开发游戏流程
- 创建窗口(基础)
- 创建图像(基础)
- 处理事件(核心)
- 循环(核心)
- 更新游戏状态(核心)
	-  退出循环

> 游戏就是图像在变换，达到想象的效果

![一个图片，python开发游戏的主要流程]()

**【例子，使用Pygame开发显示hello world 的游戏窗口】**
```py
import pygame   # 导入pygame库
from pygame.locals import *     # 从pygame.locals模块导入所有
import sys  # 导入系统库

# ------------------------
def hello_world():
    pygame.init()   # 必须初始化pygame相关的文件吧
    # 设置窗口      (690,480)表示窗口像素，元组
    pygame.display.set_mode((680,480))      # 表示设置窗口
    pygame.display.set_caption("hello World")   # 表示设置窗口标题
    # 无限循环，游戏阶段
    while True:
        # 处理事件，退出条件
        for event in pygame.event.get():
            if event.type==QUIT:    # 接收窗口关闭事件，一一匹配
                pygame.quit()   # 游戏退出
                sys.exit()  # 窗口退出
        # 将 surface对象绘制到屏幕上，即图像
        pygame.display.update() # 屏幕更新启动的意思
# -------------------------------
# 主程序
if __name__=="__main__":
    hello_world()   # 调用函数
                
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617144456389.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

>任何pygame游戏在游戏的无限循环之前，要执行`pygame.init()` 表示初始化所有的模块(可能会需要一些时间)

**pygame游戏中的无限循环中的操作**

- 处理事件 ： 鼠标，`键盘`，关闭窗口
- 更新游戏状态：位置变换，数量的变化
- 屏幕上绘制图像：新的图像

>基本上游戏就是遵循这个样的步骤

上例中的处理事件仅仅是 `关闭窗口`，即`pygame.quit()`退出游戏

### pygame图像绘制

**1. pygame图像绘制**

`surface对象`


Pygame使用`surface对象`加载绘制图像

`surface.pygame.display.set_mode()` 返回surface对象

**2. pygame图形绘制**

`pygame.draw`模块

函数名 | 作用 | 特殊说明
-- | -- | --
`rect() `| 绘制矩形 
polygon() | 绘制多边形(>=3) 
`circle()` | 圆形
ellipse() | 椭圆
arc() | 圆弧 
`line()`   | 直线
lines() | 一系列直线
aaline() | 平滑直线
aalines() | 一系列平滑直线

**详细函数使用**

函数名 | 使用 | 特殊说明
-- | -- |--
`.rect()` | 格式`pygame.draw.rect(surface.color,width-0)` | 用 `.rect`绘制矩形在`surface`表面，除surface 和 color ，rect接受矩形坐标和线宽参数，线宽0省略，则填充
`.polygon()` | ` pygame.draw.polygon(surface,color,pointlist,width=0) ` | 用法和rect类似，只不过polygon会接收一系列点的坐标的`列表`，用于确定多边形
`.circle()` |` pygame.draw.circle(surface,color,pos,radius,width=0)`   | 接收参数 圆形坐标pos ，圆半径 radius
`.ellipse()` | `pygame.draw.ellipse(surface,color,rect,width=0`  | 可以看成压瘪的圆，可以用矩形包起来,参数rect就是外接矩形
`.arc()` | `pygame.draw.arc(surface,color,rect,start_angle,stop_angle,width=0)` | start_angle和stop_angle分别为开始角度和结束角度
`.line()` | `pygame.draw.line(surface.color,start_pos,end_pos,width=0)` | 
`.lines()` | `pygame.draw.lines(surface,color,closed,pointlist,width=1)` | `closed`是布尔型变量，表示是否需要多画一条线，是闭合


 


### pygame鼠标，键盘事件处理

Pygame常用事件 | 产生途径| 参数
-- | -- | --
`QUIT` | 用户按下`关闭`按钮 | none
ACTIVEEVENT | pygame被激活/被隐藏 | gain ,state
KEYDOWN | 按键按下 | unicode,key,mod
KEYUP | 按键放开 | key,mod
MOUSEMOTION | 鼠标移动 | pos,rel,buttons
MOUSEBUTTONDOWN | 鼠标按下 | pos,button
MOUSEBUTTONUP | 鼠标松开 | pos,button

**1.  pygame键盘事件处理**

`pygame.event.get()` 获得所有事件，若`event.type=KEYDOWN` 表示键盘按下，再看按的是哪个键类型用`event.key` 

也可以使用`pygame.key.get_pressed()` 获取所有被按下的键值，返回`元组`，元组索引就是键值

如

    pressed_keys=pygame.key.get_pressed() # 先获取所有的键值，返回元组
    if pressed_keys[K_SPACE]:
        print("空格键被按下")
        fire() 发射子弹

>抽象和具体

key下的模块 | 描述
-- | --
key.get_focused() | 返回当前Pygame游戏窗口是否被激活
`key.get_pressed()` | 获取所有按下的键值 `(是当前按下的键的键值)`
`key.get_mods()` | 按下的组合键 (Alt,Ctrl,Shift)
key.set_mods() | 模拟按下组合键效果 ? 

**【例子-开发用户控制tank移动游戏，结合按键知识，通过方向键控制坦克运动，并增加游戏背景】**
```py
# 1 .导入库
import pygame # 游戏模块
import sys # 系统
import os #
from pygame.locals import *

# 2 . 定义函数 control_tank()  / play_tank()

# 主要是按键交互操控
def control_tank(event):  # 控制tank运动函数
    speed=[x,y]=[0,0]  # 相对坐标
    speed_offset=1 #   速度

    # 当按键方向键按下计算坐标
    if event.type==pygame.KEYDOWN:     #表示事件是键盘按下
    # 左键
        if event.key==pygame.K_LEFT:
            speed[0]-=speed_offset
    # 右键
        if event.key==pygame.K_RIGHT:
            speed[0]=speed_offset
    # 上键
        if event.key==pygame.K_UP:
            speed[1]-=speed_offset
    # 下键
        if event.key==pygame.K_DOWN:
            speed[1]=speed_offset
    # 当方向键被释放，则不移动
    if event.type==pygame.KEYUP:
        speed=[0, 0]   # [x,y]
    return speed   # 这里返回速度是作为矢量看待的，x,y轴方向，主要方向，然后用tank_rect.move(cur_speed)指定方向移动

#  相当于开始游戏的意思，设置初始化/窗口/背景/图像
def play_tank():
    pygame.init() # 必须

    # 设置窗口
    window_size=Rect(0,0,600,400) # 窗口大小
    speed=[1,1] # 坦克运行偏移量 [水平，垂直]，值越大，移动越快
    color_black=(255,255,255)  # 窗口背景白色
    screen=pygame.display.set_mode(window_size.size) # 设置窗口模式
    pygame.display.set_caption('用方向键控制坦克移动')
    tank_image=pygame.image.load(r'F:\school-pratice-documents\python\python代码练习\py实战\tankU.bmp')   # 加载tank图片

    # 加载窗口背景
    back_image=pygame.image.load('back_image.jpg') # 先加载
    tank_rect=tank_image.get_rect() #获取坦克的图片的区域形状

    while True:
        # 退出事件处理
        for event in pygame.event.get():    #获取所有的事件序列
            if event.type==pygame.QUIT:  # 如果这个事件的类型是quit，则执行退出操作
                pygame.quit() #游戏界面退出
                sys.exit()  # 系统退出
        # 是tank移动,speed控制速度
        cur_speed=control_tank(event) # 参数是事件，具体就是按下哪个键
        # rect.clamp()可以限制移动范围在窗口内，不用多余的设置判断
        tank_rect=tank_rect.move(cur_speed).clamp(window_size) #
        screen.blit(back_image,(0,0)) #放置图像到srceen
        screen.blit(tank_image,(tank_rect)) # 放置tank到screen
        pygame.display.update() # 表示更新，应用改变的内容


# 3 . 主函数
if __name__=='__main__':
    play_tank() # 调用函数
```

报错

    Couldn't open \tankU.bmp

[pygame无法打开bmp文件的解决](https://blog.csdn.net/weixin_39449570/article/details/78436705)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200619212924238.gif)

**流程简介**

- 导入库
- 初始化`pygame.init()`
- 定义东西
    - 创建窗口
    - 设置窗口标题
    - 游戏循环
    - 调用函数，触发事件/键盘/鼠标
    - 绘制图像（添加图形到窗口）
    - 更新状态(`pygame.display.update()`),显示状态

> 先定义，后使用

**2. 鼠标事件处理**

`pygame.mouse`函数

函数名 |  描述  | 特殊说明
-- | --|--
`.get_pressed()` | 返回`元组`类型的按键情况，分左，中，右，按下为`True` |
`.get_rel()` | 返回`元组`类型的相对偏移量 (x方向,y方向) 
`.get_pos() `| 返回当前鼠标的位置 | 如`x,y =pygame.mouse.get_pos()`
.set_visible() | 设置鼠标光标是否可见
.get_focused() | 鼠标在`Pygame`窗口有效返回`True`
.set_cursor() | 设置鼠标哦的默认样式
.get_cursor() | 返回(获取)鼠标的光标的样式



### pygame字体的使用
### **pygame声音播放**

**1. Sound对象**

`pygame.mixer.sound()` 接收文件名/文件对象，必须是`wav`/`ogg`类型音频文件

    hello_sound=pygame.mixer.sound("hello_sound.wav")  # 创建Sound对象
    hello_sound.play() # 播放一次

> 先要创建对象，才能使用方法

    play(loop,maxtime) # loop 重复次数，重复次数，不是播放次数,取1为2次，取-1为无限次，maxtime时多少毫秒后结束

    play() 默认播放一次，调用成功，返回channel对象，否则返回None

**总结**

方法 | 描述
-- | -- 
`pygame.mixer.sound()`| 创建Sound对象
`SoundObject.play(loop,maxtime) `| 用于音频的设置重复次数

**⭐2. music对象**

`pygame.mixer.music`模块


支持播放`mp3`/`ogg`格式音乐

方法|描述
--|--
`pygame.mixer.music` | 控制背景音乐播放
`pygame.mixer.music.load()`| 用于加载音乐
`pygame.mixer.music.play()` | 用于播放,`stop()`停止播放

如

    # 加载背景音乐
    pygame.mixer.music.load('hello.mp3')
    pygame.mixer.music.set_volume(music_volume/100.0)
    # 循环播放
    pygame.mixer.music.play(-1,30.0) # 表示循环无限次，从30.0秒开始播放

可以在游戏退出事件中加入`stop()`用于停止播放

>❗❗❗背景音乐相对唯一，所以不用特意指定对象，就是这个程序的背景音乐`music对象`

**pygame.mixer.music对象**

music对象的方法 | 描述 |特别说明
-- | --  | --
`.load()` | 加载音乐文件 | 
`.play(loop=0,start=0.0)` | 播放音乐 | loops为循环次数，-1为不停，5为5+1=6次，start为从那一秒开始播放，0为从头播放
`.rewind()` | 重新播放
`.stop()` | 停止播放
`.pause()`|暂停播放
`.unpause()`| 恢复播放
`.set_volume(value)`|设置音量 | value取值`0.0~1.0`
`.get_pos()`| 获取当前播放多次长时间 | 格式 ： `pygame.mixer.music.get_pos()` ： return time 

### **Pygame的精灵的使用**

**1. 精灵:**

可以被认为是一个个小图片的帧序列(如人物的行走)，可以在屏幕上移动，可以和其它图片交互，精灵图像和=可以使用，`Pygame`绘制形状函数绘制,也可以是图像文件

**sprite类成员**

用`pygame.sprite.Sprite` 实现精灵类，Sprite的数据成员和方法

1. `self.image`

负责显示什么图形

    self.image=pygame.Surface([x,y]) # 该精灵使x x y大小的矩形
    self.image.fill([color]) # 负责对image调色

    如 
    self.image=pygame.Surface([x,y])
    self.image.fil([255,0,0]) # 表示对这个x x y区域填充红色

2. `self.rect` 

负责在哪里显示

    一般
    self.rect=self.image.get_rect() 获取image矩形大小
    再给这个矩形设置显示位置,一般用self.rect.topleft用于确定左上角显示位置，以这个为参照，当然其它方位也可以

> `self.rect.方位`   ,这个方位可以是 `top`,`bottom`,`left`,`right`
> 也就是说 self.image用于创建精灵，而self.rect用于确定位置

3. `self.update()`

    负责使精灵生效

4. `Sprite.add()` 

    添加精灵到`groups` 组

5. `Sprite.remove()` 

    从精灵组中移除
6. `Sprite.kill()` 

    移除所有的精灵

7. `Sprite.alive`()

    判断某精灵是否属于精灵组groups

**⭐2. 建立精灵**

**【例子-建立tank精灵类】**

```py
import pygame,sys   # 导入库
pygame.init() # 必须初始化
class Tank(pygame.sprite.Sprite):      # 用Sprite实现一个精灵Tank
    def __init__(self,filename,initial_position):    # (某一个对象实例,图像文件名，初始位置)
        pygame.sprite.Sprite.__init__(self) # 实际是Sprite.__init__(self) 用到的是Sprite,初始化一个精灵对象实例
        self.image=pygame.image.load(filename) # 表示加载显示这个精灵的图像
        self.rect=self.image.get_rect() # 创建精灵后，用get_rect()获取矩形区域
        # self.rect.position=initial_position 确定显示的位置
        # 如
        self.rect.bottom=initial_position # 
# -------------------------        
screen=pygame.display.set_mode([640,480]) # 创建游戏窗口
screen.display.set_caption('坦克')  #用窗口对象设置窗口标题
fi='tankU.jpg'
b=Tank(fi,[150,100])  # 调用 Tank类的初始化参数 (文件名，初始位置)
while True:
    for event in pygame.event.get(): # pygame.event.get()表示获取所有的事件
        if event.type==pygame.QUIT:
            sys.quit
        screen.blit(b.image,b.rect)  # blit表示往屏幕添加图像 ，screen的方法blit()，参数(精灵的图像，精灵图像的位置)
        pygame.display.update()     # 使精灵行为生效

```

**总结:**
```py
import pygame,sys
pygame.init() #初始化模块

1. 定义精灵类 
class Tank(pygame.sprite.Sprite)；
   (1). 初始化
   def __init__(self,filename,initial_position):
       - 用精灵初始化类的对象
       pygame.sprite.Sprite.__init__(self) # 固定写法
       - 创建精灵 
       self.image=pygame.image.load('xx.png')
       -获取rect大小
       self.rect=self.image.get_rect()
       -确定显示位置
       self.rect.显示的位置方位=initial_position
 # ---------------------------------   
2. 创建窗口
screen=pygame.display.set_mode([x,y])
screen.display.set_caption('title')
screen.fill([,,,]) # 填充颜色
b=Tank('tank.png',[150,100]) # 初始化类的对象b,self即b
# 游戏主循环中
3. 放置精灵到窗口
while True:
    # 游戏退出事件判断
    for event in pygame.event.get():
        if event.type ==pygame.QUIT:  # 注意这里的写法
            sys.exit()
    screen.blit(b.image,b.rect)  # 表示添加(图像，区域形状)
    pygame.display.update()  # 因为游戏循环，导致数据的改变，需要更新数据以应用

    
```

>这个精灵的作用就是图像动画处理，让动画显示更自然，`本质还是个图像`

>注意位置尺寸都是`列表[]`类型

**【例子-使用`精灵图片序列`建立`动画效果`的人物行走精灵】**

在游戏动画中，人物的行走是基本动画，在精灵中不断切换人物行走的图片，达到动画效果

>类似于photoshop中的`gif`的制作原理，首先要有原始的图片帧

```py

```
如果只是单纯的设置动画，那就像`gif`不断的重复，如果不想这样，可以设置时间，根据时间间隔一张一张播放，将帧速率`ticks`传递给`Sprite`的`update()`函数，这样就可按帧播放

**3. 建立精灵组**

精灵组为`容器`，用于管理精灵

**4. 创建精灵组**

    group = pygame.sprite.Group()  # 比如这个组为tank_group
    goup.add(sprote_one) # 用组的对象.add()添加精灵
    # 精灵组也有update() 和 draw()
    group.update()
    group.draw()

>pygame还提供精灵于精灵之间的冲突检测，碰撞检测，这些在之后的飞机大战游戏中会用到

**5. 精灵与精灵之间的碰撞检测**

1. ⭐**两个精灵之间的矩形检测**

    只有两精灵，一对一冲突，使用`pygame.sprite.Sprite()`传递两精灵，每精灵继承`pygame.sprite.Sprite`

        # Mysptite()是之前创建的精灵类
        sprite_1 = MySprite("sprite_1.png",200,200,1)
        sprite_2 = MySprite("sprite_2.png",50,50,1)
        # 总共创建两个精灵
        result = pygame.sprite.collode_rect(sprite_1,sprite_2) # 用 pygame.sprite的 collide_rect()判断是否碰撞，返回布尔型 ，True为碰撞
        if result:
            print("两精灵碰撞上了)

2. 两精灵圆检测

`pygame.sprite.colide_circle()`基于精灵半径检测，用户可指定/直接测出半径

3. 两精灵之间像素遮罩检测

更精确检测 `pygame.sprite.collide_mask()`

5. 精灵和组的矩形碰撞检测

`pygame.sprite.spritecollide(sprite,sprite_group,bool)` ， 一一把组的精灵和组外的精灵检测，当bool为` True` ，则删除组中冲突的精灵，为`Flase`时，则不会删除

`pygame.sprite.spritecollideany()` 判断精灵组和单个精灵冲突时，返回`bool`型

5. 精灵组之间的碰撞检测

`pygame.sprite.roupcollide()`,检测两组冲突，返回`字典`，键值对

**总结**

碰撞类型 | 调用方法函数
-- | --
一对一矩形 | `pygame.sprite.collide_rect(one,two)`
一对一圆形 | `pygame.sprite.collide_circle(one,two)`
一对一像素检测 | `pygame.sprite.collide_mask(one,two)`
精灵和组矩形| `pygame.sprite.spritecollide(one,group,bool)`
精灵组矩形 | `pygame.sprite.groupcollide(group1,goup2)`





## 基于pygame的贪吃蛇游戏


## **基于pygame设计飞机大战游戏**

### **游戏角色**

**【例子-游戏角色设计】**

这里使用pythonm[面向对象](https://cn.bing.com/search?q=%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1&qs=n&form=QBRE&sp=-1&pq=mian%27xiang%E5%AF%B9%E8%B1%A1&sc=8-12&sk=&cvid=04F9356FA5044E459616197FA1146A66)
设计 `玩家类`,`子弹类`,`敌机类`

继承`pygame`和`sprite`,实现动画效果，和检测碰撞

```py
# 基于pygame的飞机大战游戏
#     游戏角色

# 玩家飞机，敌方飞机，子弹
# 设计类 ： 玩家类 player; 敌方飞机类enemy ; 子弹类 bullet
# ----------------------------------------------------
import pygame
from pygame.locals import *
from sys import exit    # 用于系统退出
import random

# 初始化数据

SCREEN_WIDTH=480    # 屏幕宽度
SCREEN_HEIGHT=800   # 屏幕高度
TYPE_SMALL=1
TYPE_MIDDLE=2
TYPE_BIG=3
# ---------------------
# 设计类

# 子弹类
class Bullet(pygame.sprite.Sprite):     # 继承sprite精灵类
    def __init__(self,bullet_img,init_pos):     # 初始化函数 （子类的图片，初始位置）
        pygame.sprite.Sprite.__init__(self)     # 用精灵初始化子弹
        # 开始创建对象实例
        self.image=bullet_img  #子弹图片
        self.rect=self.image.get_rect()     # 显示位置
        self.rect.midbottom=init_pos       # rect. midbottom表示形状的中下方
        self.speed=10   # 表示偏移量
    def move(self): # 移动方法
        self.rect.top-=self.speed   # 减速
# 玩家类
class Player(pygame.sprite.Sprite):     # 同样继承精灵类
    def __init__(self,plane_img,player_rect,init_pos):  # (飞机图片，形状，初始位置)
        pygame.sprite.Sprite.__init__(self)
        self.image=plane_img
        for i in range(len(player_rect)):
            self.image.append(plane_img.subsurface(player_rect[i]).convert_alpha())
        self.rect=player_rect[0]    # 初始化图片所在的矩形
        self.rect.topleft=init_pos  # 初始化矩形的左上角坐标
        self.speed=8    # 初始化玩家的速度,这里是确定的值
        self.bullets=pygame.sprite.Group()  # 玩家飞机所发射的子弹的集合
        self.img_index=0    # 玩家精灵图片的索引
        self.is_hit=False   # 玩家是否被击中
    # --------------------------
    # 一些动作事件的处理
    # 射击
    def shoot(self,bullet_img):
        bullet=Bullet(bullet_img,self.rect.midtop) # 调用子弹类，初始化
        self.bullets.add(bullet)    # 把子弹添加到玩家的飞机
    # 加速(往上移动)
    def moveUp(self):
        if self.rect.top<=0:    # 如果此时是在框外，则变到框内
            self.rect.top=0
        else:
            self.rect.top-=self.speed   # 不是的话，就往上移动
    # 减速(往下移动)
    def moveDown(self):
        if self.rect.top>=SCREEN_HEIGHT-self.rect.height:   # 如果飞机跑到屏幕下面，则移动到屏幕内
            self.rect.top=SCREEN_HEIGHT-self.rect.height
        else:
            self.rect.top+=self.speed   # 表示往下方向移动一个偏移量
    # 左移动
    def moveLeft(self):
        if self.rect.left<=0:   # 表示跑到左边，嵌入屏幕,则转正
            self.rect.left=0
        else:
            self.rect.left-=self.speed  # 表示往左方向移动一个偏移量
    # 右移动
    def moveRight(self):
        if self.rect.left>=SCREEN_WIDTH-self.rect.width:    # 坐标计算方法不一样和moveLeft有所区别
            self.rect.left=SCREEN_WIDTH-self.rect.width
        else:
            self.rect.left+=self.speed
    # 注意这里的moveRight的算法有不同，因为坐标计算的原因
# -------------------------
# 敌机类
class Enemy(pygame.sprite.Sprite):
    def __init__(self,enemy_img,enemy_down_imgs,init_pos):  # (敌机飞机图片，敌机击毁的效果图片，初始位置)
        pygame.sprite.Sprite.__init__(self)
        self.image=enemy_img
        self.rect=self.image.get_rect()     # 表示用get_rect()方法获取图像的区域
        self.rect.topleft=init_pos          # 表示初始位置，左上角出现
        self.down_imgs=enemy_down_imgs
        self.speed=2    # 敌机的偏移量
        self.down_index=0   # 敌机被击毁的索引
    def move(self):
        self.rect.top+=self.speed   # 表示top往下降，即从上往下出发
```

### 游戏界面的显示
在游戏中使用到的图片，图像需要分割开

在游戏中，`显示`的元素在pygame中都是`surface`，那么就可以用`subsurface()`方法生成一个大的图片，再在大图片中生成小图片
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200617162613552.png?)

**【载入飞机图片】**

```py
# 载入飞机图片

plane_img=pygame.image.load('shoot.png')    # 这个地方注意设置路径
# 选择飞机在大图中的位置，使用subsurface()生成subsurface，初始化飞机开始的位置
player_rect=pygame.Rect(0,99,102,126)
player1=plane_img.subsurface(player_rect)   # 获取飞机图片;用飞机的图片生成一个区域，且是飞机的区域
player_pos=[200,600]    # 使用两点定坐标法,左，上
screen.blit(player1,player_pos) # 根据图片和位置，生成屏幕的图像

# 初始化游戏后，载入相关文件

# 初始化游戏
pygame.init()
screen=pygame.display.set_mode((SCREEN_WIDTH,SCREEN_HEIGHT))    # 创建窗口  ；再次注意的是，尺寸为元组类型
pygame.display.set_caption('飞机大战')
# ----------------------------------
# 载入音乐
bullet_sound=pygame.mixer.Sound('')     # 填音乐文件的路径
enemy1_down_sound=pygame.mixer.Sound('')    # 填路径
game_over_sound=pygame.mixer.Sound('')

bullet_sound.set_volume(0.3)    # 设置音量
enemy1_down_sound.set_volume(0.3)
game_over_sound.set_volume(0.3)

pygame.mixer.music.load('')     # 加载全场音乐
pygame.mixer.music.play(-1, 0.0) # 全场音乐的设置
pygame.mixer.music.set_volume(0.25)
# ---------------------------
# 设置相关图片；背景图，飞机，子弹图
background=pygame.image.load('').convert    # 载入背景图
game_over=pygame.image.load('')     # 载入游戏结束的时候显示的图
filename=''
plane_img=pygame.image.load(filename)   # 载入飞机，子弹相关图片

# --------------------
# 设置玩家参数
player_rect=[]  # 表示玩家的区域
player_rect.append(pygame.Rect(0,99,102,126))   # 玩家精灵图片区域

player_rect.append(pygame.Rect(165,360,102,126))
player_rect.append(pygame.Rect(165,234,102,126))
player_rect.append(pygame.Rect(330,624,102,126))    # 玩家爆炸精灵图片区域
player_rect.append(pygame.Rect(330,498,102,126))
player_rect.append(pygame.Rect(432,624,102,126))
player_pos=[200,600]
player=Player(plane_img,player_rect,player_pos)     # 根据(玩家图片，玩家区域，玩家位置)创建玩家

# 定义子弹对象使用surface相关参数
bullet_rect=pygame.Rect(1004,987,9,21)
bullet_img=plane_img.subsurface(bullet_rect)    # subsurface是用大图片基础上生成小图片子弹图片

# 定义敌机
enemy_rect=pygame.Rect(534,612,57,21)
enemy_img=plane_img.subsurface(enemy_rect)  # subsurface(某图像的区域)
enemy_down_imgs=[]  # 敌机被击毁时的图片
enemy_down_imgs.append(plane_img.subsurface(pygame.Rect(267,347,57,43)))
enemy_down_imgs.append(plane_img.subsurface(pygame.Rect(873,697,57,43)))
enemy_down_imgs.append(plane_img.subsurface(pygame.Rect(267,296,57,43)))
enemy_down_imgs.append(plane_img.subsurface(pygame.Rect(930,697,57,43)))
enemies1=pygame.sprite.Group()      # 储存敌人的飞机
enemies_down=pygame.sprite.Group()      # 储存被击毁的飞机，用于渲染击毁精灵动画
shoot_frequency=0   # 射击频率
player_frequency=0
player_down_index=16
score=0     # 分数，表示敌机击毁的个数
clock=pygame.time.Clock()   # 游戏闹钟？
running=True    # 是否运行

```

### **游戏逻辑实现**

下面进入游戏的主循环，主要操作有

- 处理键盘输入事件，处理游戏交互
- 处理子弹
- 处理敌机
- 得分显示

**处理键盘输入事件**

```py
key_pressed=pygame.key.get_pressed()    # 先获取所有的pygame的键盘事件
# 若玩家被击中，则无效
# 键盘上，控制飞机的移动
if not player.is_hit:
    if key_pressed[K_w] or key_pressed[K_UP]:   # 如果是键盘的 "w" 键或者，向上箭头键，则往上移动
        player.moveUp() # 执行相关的方法
    if key_pressed[K_s] or key_pressed[K_DOWN]:
        player.moveDown()
    if key_pressed[K_a] or key_pressed[K_LEFT]:
        player.moveLeft()
    if key_pressed[K_d] or key_pressed[K_RIGHT]:
        player.moveRight()
    
```
> 键盘响应事件，就是，设定键盘按键响应事件 `key_pressed` 表示键盘按键事件的集合，具体的事件`[]`加参数，然后再设定事件后执行对应的方法即可

**处理子弹**

```py
# 处理子弹
if not player.is_hit:   # 如果没有被击中，则发射子弹
    if shoot_frequency%15==0:   # 射击的频率要达到一定标准，才发出音效，和图片
        bullet_sound.play()
        player.shoot(bullet_img)
    shoot_frequency+=1
    if shoot_frequency>=15:
        shoot_frequency=0   # 如果射击频率大于15，则

# 移动已发射过的子弹，若超出窗口则删除
for bullet in player.bullets:
    bullet.move()   # 以固定的速度移动子弹
    if bullet.rect.bottom<0:    # 一旦移出屏后删除子弹
        player.bullets.remove(bullet)   # 删除子弹

```

**敌机处理**

在界面的上方生成，一定速度下移

- 生成敌机，控制生成的频率
- 移动敌机
- 敌机与玩家飞机碰撞效果
- 移出屏幕后删除敌机
- 敌机被子弹击中后的效果

**得分显示**

界面固定位置显示得分

```py
score_font=pygame.font.Font(None,36)
score_text=score_font.render(str(score),True,(128,128,128)) 
text_rect=score_text.get_rect() # 获取文本的区域，选择位置坐标
text_rect.toleft=[10,10]
screen.blit(score_text,text_rect)   # 屏幕显示分数块
```
**游戏主循环完整代码**

**【简单的飞机大战游戏】**

`上机测试代码`

```py
# 基于pygame的飞机大战游戏
#     游戏角色

# 玩家飞机，敌方飞机，子弹
# 设计类 ： 玩家类 player; 敌方飞机类enemy ; 子弹类 bullet
# ----------------------------------------------------
import pygame
from pygame.locals import *
from sys import exit
import random

# 初始化数据

SCREEN_WIDTH = 480  # 屏幕宽度
SCREEN_HEIGHT = 800  # 屏幕高度
TYPE_SMALL = 1
TYPE_MIDDLE = 2
TYPE_BIG = 3


# ---------------------
# 设计类

# 子弹类
class Bullet(pygame.sprite.Sprite):  # 继承sprite精灵类
    def __init__(self, bullet_img, init_pos):  # 初始化函数 （子类的图片，初始位置）
        pygame.sprite.Sprite.__init__(self)  # 用精灵初始化子弹
        # 开始创建对象实例
        self.image = bullet_img  # 子弹图片
        self.rect = self.image.get_rect()  # 显示位置
        self.rect.midbottom = init_pos  # rect. midbottom表示形状的中下方
        self.speed = 10  # 表示偏移量

    def move(self):  # 移动方法
        self.rect.top -= self.speed  # 减速


# 玩家类
class Player(pygame.sprite.Sprite):  # 同样继承精灵类
    def __init__(self, plane_img, player_rect, init_pos):  # (飞机图片，形状，初始位置)
        pygame.sprite.Sprite.__init__(self)
        self.image = []
        for i in range(len(player_rect)):
            self.image.append(plane_img.subsurface(player_rect[i]).convert_alpha())
        self.rect = player_rect[0]  # 初始化图片所在的矩形
        self.rect.topleft = init_pos  # 初始化矩形的左上角坐标
        self.speed = 8  # 初始化玩家的速度,这里是确定的值
        self.bullets = pygame.sprite.Group()  # 玩家飞机所发射的子弹的集合
        self.img_index = 0  # 玩家精灵图片的索引
        self.is_hit = False  # 玩家是否被击中

    # --------------------------
    # 一些动作事件的处理
    # 射击
    def shoot(self, bullet_img):
        bullet = Bullet(bullet_img, self.rect.midtop)  # 调用子弹类，初始化
        self.bullets.add(bullet)  # 把子弹添加到玩家的飞机

    # 加速(往上移动)
    def moveUp(self):
        if self.rect.top <= 0:  # 如果此时是在框外，则变到框内
            self.rect.top = 0
        else:
            self.rect.top -= self.speed  # 不是的话，就往上移动

    # 减速(往下移动)
    def moveDown(self):
        if self.rect.top >= SCREEN_HEIGHT - self.rect.height:  # 如果飞机跑到屏幕下面，则移动到屏幕内
            self.rect.top = SCREEN_HEIGHT - self.rect.height
        else:
            self.rect.top += self.speed  # 表示往下方向移动一个偏移量

    # 左移动
    def moveLeft(self):
        if self.rect.left <= 0:  # 表示跑到左边，嵌入屏幕,则转正
            self.rect.left = 0
        else:
            self.rect.left -= self.speed  # 表示往左方向移动一个偏移量

    # 右移动
    def moveRight(self):
        if self.rect.left >= SCREEN_WIDTH - self.rect.width:  # 坐标计算方法不一样和moveLeft有所区别
            self.rect.left = SCREEN_WIDTH - self.rect.width
        else:
            self.rect.left += self.speed
    # 注意这里的moveRight的算法有不同，因为坐标计算的原因


# -------------------------
# 敌机类
class Enemy(pygame.sprite.Sprite):
    def __init__(self, enemy_img, enemy_down_imgs, init_pos):  # (敌机飞机图片，敌机击毁的效果图片，初始位置)
        pygame.sprite.Sprite.__init__(self)
        self.image = enemy_img
        self.rect = self.image.get_rect()  # 表示用get_rect()方法获取图像的区域
        self.rect.topleft = init_pos  # 表示初始位置，左上角出现
        self.down_imgs = enemy_down_imgs
        self.speed = 2  # 敌机的偏移量
        self.down_index = 0  # 敌机被击毁的索引

    def move(self):
        self.rect.top += self.speed  # 表示top往下降，即从上往下出发


# ---------------------------------------------------------
# 游戏界面的显示

# 初始化游戏后，载入相关文件

# 初始化游戏
pygame.init()
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))  # 创建窗口  ；再次注意的是，尺寸为元组类型
pygame.display.set_caption('飞机大战')
# ----------------------------------
# 载入音乐
bullet_sound = pygame.mixer.Sound('resources/sound/bullet.wav')  # 填音乐文件的路径
enemy1_down_sound = pygame.mixer.Sound('resources/sound/enemy1_down.wav')  # 填路径
game_over_sound = pygame.mixer.Sound('resources/sound/game_over.wav')

bullet_sound.set_volume(0.3)  # 设置音量
enemy1_down_sound.set_volume(0.3)
game_over_sound.set_volume(0.3)

pygame.mixer.music.load('resources/sound/game_over.wav')  # 加载全场音乐
pygame.mixer.music.play(-1, 0.0)  # 全场音乐的设置
pygame.mixer.music.set_volume(0.25)
# ---------------------------
# 设置相关图片；背景图，飞机，子弹图
background = pygame.image.load('resources/image/background.png').convert()  # 载入背景图
game_over = pygame.image.load('resources/image/gameover.png')  # 载入游戏结束的时候显示的图
filename = 'resources/image/shoot.png'
plane_img = pygame.image.load(filename)  # 载入飞机，子弹相关图片

# --------------------
# 设置玩家参数
player_rect = []  # 表示玩家的区域
player_rect.append(pygame.Rect(0, 99, 102, 126))  # 玩家精灵图片区域
player_rect.append(pygame.Rect(165, 360, 102, 126))
player_rect.append(pygame.Rect(165, 234, 102, 126))  # 玩家爆炸精灵图片区域
player_rect.append(pygame.Rect(330, 624, 102, 126))
player_rect.append(pygame.Rect(330, 498, 102, 126))
player_rect.append(pygame.Rect(432, 624, 102, 126))
player_pos = [200, 600]
player = Player(plane_img, player_rect, player_pos)  # 根据(玩家图片，玩家区域，玩家位置)创建玩家

# 定义子弹对象使用surface相关参数
bullet_rect = pygame.Rect(1004, 987, 9, 21)
bullet_img = plane_img.subsurface(bullet_rect)  # subsurface是用大图片基础上生成小图片子弹图片

# 定义敌机
enemy1_rect = pygame.Rect(534, 612, 57, 43)
enemy1_img = plane_img.subsurface(enemy1_rect)  # subsurface(某图像的区域)
enemy1_down_imgs = []  # 敌机被击毁时的图片
enemy1_down_imgs.append(plane_img.subsurface(pygame.Rect(267, 347, 57, 43)))
enemy1_down_imgs.append(plane_img.subsurface(pygame.Rect(873, 697, 57, 43)))
enemy1_down_imgs.append(plane_img.subsurface(pygame.Rect(267, 296, 57, 43)))
enemy1_down_imgs.append(plane_img.subsurface(pygame.Rect(930, 697, 57, 43)))
enemies1 = pygame.sprite.Group()  # 储存敌人的飞机
enemies_down = pygame.sprite.Group()  # 储存被击毁的飞机，用于渲染击毁精灵动画

# 射击频率初始化
shoot_frequency = 0  # 射击频率
enemy_frequency = 0
player_down_index = 16
score = 0  # 分数，表示敌机击毁的个数
clock = pygame.time.Clock()  # 游戏闹钟？
running = True  # 是否运行
# 载入飞机图片

# plane_img = pygame.image.load('resources/image/shoot.png')  # 这个地方注意设置路径
# # 选择飞机在大图中的位置，使用subsurface()生成subsurface，初始化飞机开始的位置
# player_rect = pygame.Rect(0, 99, 102, 126)
# player1 = plane_img.subsurface(player_rect)  # 获取飞机图片;用飞机的图片生成一个区域，且是飞机的区域
# player_pos = [200, 600]  # 使用两点定坐标法,左，上
# screen.blit(player1, player_pos)  # 根据图片和位置，生成屏幕的图像

# ---------------------------------------
# ---------------------------------------
# key_pressed=pygame.key.get_pressed()    # 先获取所有的pygame的键盘事件
# # 若玩家被击中，则无效
# # 键盘上，控制飞机的移动
# if not player.is_hit:
#     if key_pressed[K_w] or key_pressed[K_UP]:   # 如果是键盘的 "w" 键或者，向上箭头键，则往上移动
#         player.moveUp() # 执行相关的方法
#     if key_pressed[K_s] or key_pressed[K_DOWN]:
#         player.moveDown()
#     if key_pressed[K_a] or key_pressed[K_LEFT]:
#         player.moveLeft()
#     if key_pressed[K_d] or key_pressed[K_RIGHT]:
#         player.moveRight()
#
# # 键盘的交互，就是，通过事件的触发获取，一个信息，然后，判断，后执行相关事件
#
#
# # --------------------------------
# # 处理子弹
# if not player.is_hit:   # 如果没有被击中，则发射子弹
#     if shoot_frequency%15==0:   # 射击的频率要达到一定标准，才发出音效，和图片
#         bullet_sound.play()
#         player.shoot(bullet_img)
#     shoot_frequency+=1
#     if shoot_frequency>=15:
#         shoot_frequency=0   # 如果射击频率大于15，则
#
# # 移动已发射过的子弹，若超出窗口则删除
# for bullet in player.bullets:
#     bullet.move()   # 以固定的速度移动子弹
#     if bullet.rect.bottom<0:    # 一旦移出屏后删除子弹
#         player.bullets.remove(bullet)   # 删除子弹
# # ----------------------
# score_font=pygame.font.Font(None,36)
# score_text=score_font.render(str(score),True,(128,128,128))
# text_rect=score_text.get_rect() # 获取文本的区域，选择位置坐标
# text_rect.toleft=[10,10]
# screen.blit(score_text,text_rect)   # 屏幕显示分数块

# --------------------------------------------
# 游戏主循环代码
running=True
while running:
    clock.tick(60)  # 控制游戏最大的帧律60
    if not player.is_hit:  # 如果没有被击中，就正常工作
        if shoot_frequency % 15 == 0:
            bullet_sound.play()
            player.shoot(bullet_img)  # 玩家射出子弹
        shoot_frequency += 1
        if shoot_frequency >= 15:
            shoot_frequency = 0

    # 生成的敌机
    if enemy_frequency % 50 == 0:
        enemy1_pos = [random.randint(0, SCREEN_WIDTH - enemy1_rect.width), 0]
        enemy1=Enemy(enemy1_img, enemy1_down_imgs, enemy1_pos)    # 创建一个敌机
        enemies1.add(enemy1)
    enemy_frequency += 1
    if enemy_frequency>=100:
        enemy_frequency = 0
    # 移动子弹
    for bullet in player.bullets:
        bullet.move()
        if bullet.rect.bottom < 0:  # 如果子弹消失于屏幕界面，则删除子弹图像
            player.bullets.remove(bullet)
    # 移动敌机
    for enemy in enemies1:
        enemy.move()
        # 判断玩家是否被击中
        if pygame.sprite.collide_circle(enemy,player):# 玩家和敌机碰撞后的效果处理
            enemies_down.add(enemy)
            enemies1.remove(enemy)
            player.is_hit=True
            game_over_sound.play()  # 则游戏失败
            break
        if enemy.rect.top > SCREEN_HEIGHT: # 表示飞机从下面，离开屏幕
            enemies1.remove(enemy)
# 敌机被子弹击中的效果
# 将被击中的敌机，添加到击毁敌机的Group中，用于渲染
    enemies1_down = pygame.sprite.groupcollide(enemies1,player.bullets,1,1)   # 敌机碰撞后，作为一个击毁对象
    for enemy_down in enemies1_down:    # 添加到组group
        enemies_down.add(enemy_down)

    # 绘制背景
    screen.fill(0)
    screen.blit(background, (0, 0))

# 绘制玩家飞机
    if not player.is_hit:
        screen.blit(player.image[player.img_index],player.rect)
        # 更换图片索引使飞机有动画效果
        player.img_index=shoot_frequency // 8
    else:
        player.img_index=player_down_index //  8
        screen.blit(player.image[player.img_index],player.rect)
        player_down_index+=1
        if player_down_index>47:    # 被击毁数量大于47的时候，就失败
            running=False
    # 绘制击毁动画
    for enemy_down in enemies_down:
        if enemy_down.down_index==0:   #
            enemy1_down_sound.play()    #
        if enemy_down.down_index>7: # 击落超7
            enemies_down.remove(enemy_down) #   移出
            score+=1000
            continue
        screen.blit(enemy_down.down_imgs[enemy_down.down_index//2],enemy_down.rect)  #是某一个击毁的敌机
        enemy_down.down_index+=1
    # 绘制子弹和敌机
    player.bullets.draw(screen)
    enemies1.draw(screen)
    # 绘制得分
    score_font=pygame.font.Font(None,36)
    score_text=score_font.render(str(score),True,(128,128,128))
    text_rect=score_text.get_rect()
    text_rect.topleft=[10,10]
    screen.blit(score_text,text_rect)
    # 更新屏幕
    pygame.display.update()
    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            pygame.quit()
            exit()
    # 监听键盘事件
    key_pressed=pygame.key.get_pressed()    # 键盘事件
    # 若玩家被击中，则无效
    # 键盘上，控制飞机的移动
    if not player.is_hit:
        if key_pressed[K_w] or key_pressed[K_UP]:   # 如果是键盘的 "w" 键或者，向上箭头键，则往上移动
            player.moveUp() # 执行相关的方法
        if key_pressed[K_s] or key_pressed[K_DOWN]:
            player.moveDown()
        if key_pressed[K_a] or key_pressed[K_LEFT]:
            player.moveLeft()
        if key_pressed[K_d] or key_pressed[K_RIGHT]:
            player.moveRight()

font=pygame.font.Font(None,48)
text=font.render('Score:'+str(score),True,(255,0,0))
text_rect=text.get_rect()
text_rect.centerx=screen.get_rect().centerx
text_rect.centery=screen.get_rect().centerx+24
screen.blit(game_over,(0,0))
screen.blit(text,text_rect)
while 1:
    # 退出判断
    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            pygame.quit()
            exit()
    pygame.display.update()     # 更新

```

> 出现问题解答 : [https://stackoverflow.com/questions/50730233/typeerror-argument-1-must-be-pygame-surface-not-builtin-function-or-method](https://stackoverflow.com/questions/50730233/typeerror-argument-1-must-be-pygame-surface-not-builtin-function-or-method)

![](https://img-blog.csdnimg.cn/20200619115055677.png)

`报错`

❗一时半会解决不了

- 变量写错
- 方法用错
- 少了，漏了
- python格式不对
- ---


`源代码`

```py
import pygame
from sys import exit
from pygame.locals import *
# from gameRole import *
import random

# 定义类
SCREEN_WIDTH = 480
SCREEN_HEIGHT = 800

TYPE_SMALL = 1
TYPE_MIDDLE = 2
TYPE_BIG = 3


# 子弹类
class Bullet(pygame.sprite.Sprite):
    def __init__(self, bullet_img, init_pos):
        pygame.sprite.Sprite.__init__(self)
        self.image = bullet_img
        self.rect = self.image.get_rect()
        self.rect.midbottom = init_pos
        self.speed = 10

    def move(self):
        self.rect.top -= self.speed


# 玩家类
class Player(pygame.sprite.Sprite):
    def __init__(self, plane_img, player_rect, init_pos):
        pygame.sprite.Sprite.__init__(self)
        self.image = []  # 用来存储玩家对象精灵图片的列表
        for i in range(len(player_rect)):
            self.image.append(plane_img.subsurface(player_rect[i]).convert_alpha())
        self.rect = player_rect[0]  # 初始化图片所在的矩形
        self.rect.topleft = init_pos  # 初始化矩形的左上角坐标
        self.speed = 8  # 初始化玩家速度，这里是一个确定的值
        self.bullets = pygame.sprite.Group()  # 玩家飞机所发射的子弹的集合
        self.img_index = 0  # 玩家精灵图片索引
        self.is_hit = False  # 玩家是否被击中

    def shoot(self, bullet_img):
        bullet = Bullet(bullet_img, self.rect.midtop)
        self.bullets.add(bullet)

    def moveUp(self):
        if self.rect.top <= 0:
            self.rect.top = 0
        else:
            self.rect.top -= self.speed

    def moveDown(self):
        if self.rect.top >= SCREEN_HEIGHT - self.rect.height:
            self.rect.top = SCREEN_HEIGHT - self.rect.height
        else:
            self.rect.top += self.speed

    def moveLeft(self):
        if self.rect.left <= 0:
            self.rect.left = 0
        else:
            self.rect.left -= self.speed

    def moveRight(self):
        if self.rect.left >= SCREEN_WIDTH - self.rect.width:
            self.rect.left = SCREEN_WIDTH - self.rect.width
        else:
            self.rect.left += self.speed


# 敌人类
class Enemy(pygame.sprite.Sprite):
    def __init__(self, enemy_img, enemy_down_imgs, init_pos):
        pygame.sprite.Sprite.__init__(self)
        self.image = enemy_img
        self.rect = self.image.get_rect()
        self.rect.topleft = init_pos
        self.down_imgs = enemy_down_imgs
        self.speed = 2
        self.down_index = 0

    def move(self):
        self.rect.top += self.speed


# 初始化游戏
pygame.init()
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption('飞机大战')

# 载入游戏音乐
bullet_sound = pygame.mixer.Sound('resources/sound/bullet.wav')
enemy1_down_sound = pygame.mixer.Sound('resources/sound/enemy1_down.wav')
game_over_sound = pygame.mixer.Sound('resources/sound/game_over.wav')
bullet_sound.set_volume(0.3)
enemy1_down_sound.set_volume(0.3)
game_over_sound.set_volume(0.3)
pygame.mixer.music.load('resources/sound/game_music.wav')
pygame.mixer.music.play(-1, 0.0)
pygame.mixer.music.set_volume(0.25)

# 载入背景图
background = pygame.image.load('resources/image/background.png').convert()
game_over = pygame.image.load('resources/image/gameover.png')

filename = 'resources/image/shoot.png'
plane_img = pygame.image.load(filename)

# 设置玩家相关参数
player_rect = []
player_rect.append(pygame.Rect(0, 99, 102, 126))  # 玩家精灵图片区域
player_rect.append(pygame.Rect(165, 360, 102, 126))
player_rect.append(pygame.Rect(165, 234, 102, 126))  # 玩家爆炸精灵图片区域
player_rect.append(pygame.Rect(330, 624, 102, 126))
player_rect.append(pygame.Rect(330, 498, 102, 126))
player_rect.append(pygame.Rect(432, 624, 102, 126))
player_pos = [200, 600]
player = Player(plane_img, player_rect, player_pos)

# 定义子弹对象使用的surface相关参数
bullet_rect = pygame.Rect(1004, 987, 9, 21)
bullet_img = plane_img.subsurface(bullet_rect)

# 定义敌机对象使用的surface相关参数
enemy1_rect = pygame.Rect(534, 612, 57, 43)
enemy1_img = plane_img.subsurface(enemy1_rect)
enemy1_down_imgs = []
enemy1_down_imgs.append(plane_img.subsurface(pygame.Rect(267, 347, 57, 43)))
enemy1_down_imgs.append(plane_img.subsurface(pygame.Rect(873, 697, 57, 43)))
enemy1_down_imgs.append(plane_img.subsurface(pygame.Rect(267, 296, 57, 43)))
enemy1_down_imgs.append(plane_img.subsurface(pygame.Rect(930, 697, 57, 43)))

enemies1 = pygame.sprite.Group()

# 存储被击毁的飞机，用来渲染击毁精灵动画
enemies_down = pygame.sprite.Group()

shoot_frequency = 0
enemy_frequency = 0

player_down_index = 16

score = 0

clock = pygame.time.Clock()

running = True

while running:
    # 控制游戏最大帧率为60
    clock.tick(60)

    # 控制发射子弹频率,并发射子弹
    if not player.is_hit:
        if shoot_frequency % 15 == 0:
            bullet_sound.play()
            player.shoot(bullet_img)
        shoot_frequency += 1
        if shoot_frequency >= 15:
            shoot_frequency = 0

    # 生成敌机
    if enemy_frequency % 50 == 0:
        enemy1_pos = [random.randint(0, SCREEN_WIDTH - enemy1_rect.width), 0]
        enemy1 = Enemy(enemy1_img, enemy1_down_imgs, enemy1_pos)
        enemies1.add(enemy1)
    enemy_frequency += 1
    if enemy_frequency >= 100:
        enemy_frequency = 0

    # 移动子弹，若超出窗口范围则删除
    for bullet in player.bullets:
        bullet.move()
        if bullet.rect.bottom < 0:
            player.bullets.remove(bullet)

    # 移动敌机，若超出窗口范围则删除
    for enemy in enemies1:
        enemy.move()
        # 判断玩家是否被击中
        if pygame.sprite.collide_circle(enemy, player):
            enemies_down.add(enemy)
            enemies1.remove(enemy)
            player.is_hit = True
            game_over_sound.play()
            break
        if enemy.rect.top > SCREEN_HEIGHT:
            enemies1.remove(enemy)

    # 将被击中的敌机对象添加到击毁敌机Group中，用来渲染击毁动画
    enemies1_down = pygame.sprite.groupcollide(enemies1, player.bullets, 1, 1)
    for enemy_down in enemies1_down:
        enemies_down.add(enemy_down)

    # 绘制背景
    screen.fill(0)
    screen.blit(background, (0, 0))

    # 绘制玩家飞机
    if not player.is_hit:
        screen.blit(player.image[player.img_index], player.rect)
        # 更换图片索引使飞机有动画效果
        player.img_index = shoot_frequency // 8
    else:
        player.img_index = player_down_index // 8
        screen.blit(player.image[player.img_index], player.rect)
        player_down_index += 1
        if player_down_index > 47:
            running = False

    # 绘制击毁动画
    for enemy_down in enemies_down:
        if enemy_down.down_index == 0:
            enemy1_down_sound.play()
        if enemy_down.down_index > 7:
            enemies_down.remove(enemy_down)
            score += 1000
            continue
        screen.blit(enemy_down.down_imgs[enemy_down.down_index // 2], enemy_down.rect)
        enemy_down.down_index += 1

    # 绘制子弹和敌机
    player.bullets.draw(screen)
    enemies1.draw(screen)

    # 绘制得分
    score_font = pygame.font.Font(None, 36)
    score_text = score_font.render(str(score), True, (128, 128, 128))
    text_rect = score_text.get_rect()
    text_rect.topleft = [10, 10]
    screen.blit(score_text, text_rect)

    # 更新屏幕
    pygame.display.update()

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            exit()

    # 监听键盘事件
    key_pressed = pygame.key.get_pressed()
    # 若玩家被击中，则无效
    if not player.is_hit:
        if key_pressed[K_w] or key_pressed[K_UP]:
            player.moveUp()
        if key_pressed[K_s] or key_pressed[K_DOWN]:
            player.moveDown()
        if key_pressed[K_a] or key_pressed[K_LEFT]:
            player.moveLeft()
        if key_pressed[K_d] or key_pressed[K_RIGHT]:
            player.moveRight()

font = pygame.font.Font(None, 48)
text = font.render('Score: ' + str(score), True, (255, 0, 0))
text_rect = text.get_rect()
text_rect.centerx = screen.get_rect().centerx
text_rect.centery = screen.get_rect().centery + 24
screen.blit(game_over, (0, 0))
screen.blit(text, text_rect)

while 1:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            exit()
    pygame.display.update()


```

### **游戏总结**


**初始化游戏**

    pygame.init()  # 必须在游戏循环之前设置
    screen=pygame.display.set_mode( (屏幕宽，屏幕高) ) # 使用 pygame.display 模块
    pygame.dsiplay.set_caption('飞机大战')  # 设置游戏标题

> pygame.display 中的 `set_mode` | `set_caption`方法

![在这里插入图片描述](https://img-blog.csdnimg.cn/202006191218554.png?)

**媒体载入** 

`pygame.mixer`包

    # 声音
    pygame.mixer.Sound('filepath')  # 音乐可以为.wav格式

    # 音量
    pygame.set_volume(value)  # value 如 0.3

    # 音乐
    pagame.mixer.music.load('filepath')    # 可以为 .wav格式

> 都是`pygame.mixer包`的方法

**图像载入**

`pygame.image`包

    # 图像/背景图
    pygame.image.load('filepath')


**pygame中的`图像/屏幕/区域`**

`pygame.rect`包 ， `pygame.surface`包

    使用图像对象.image.get_rect() 方法获取矩形区域


**pygame精灵**

`pygame.sprite`包

    pygame.sprite (操作移动图像)
    pygame.sprite.Sprite # 用Sprite创建精灵