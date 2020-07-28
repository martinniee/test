---
tags:
  - python
categories: 
  - 教程
toc: true
cover: https://img-blog.csdnimg.cn/20200309214323829.png


---
### 安装Jupyter Notebook
在终端中输入

```java
conda install jupyter notebook
```

<!-- more -->

回车，待安装完毕。
  安装好以后，就可以在Web端进行交互输入代码、运行代码，添加文本、图片等等。

### 修改主目录文件夹
Jupyter Notebook默认的主目录是“我的文档”或“用户”，在Jupyter Notebook中也会显示“我的文档”或“用户”中其他无关文件，影响文件的查找。因此有必要修改默认的主目录。

1. 在终端输入

```java

jupyter notebook --generate-config

```

回车，下方显示配置文档所在位置。如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200304115708410.png)

2. 打开此文件夹，右键点击`jupyter_notebook_config.py`文件，使用打开方式为记事本打开。
在记事本中查找（按Ctrl+F）`NotebookApp.notebook_dir `所在位置，将后边的文件夹修改为你喜欢的文件夹位置，并把前面的#号去掉，改为非注释语句。如下图所示：![在这里插入图片描述](https://img-blog.csdnimg.cn/20200304115843801.png)


3. 此时重新打开notebook即可进入设置好的目录
如果这一步打开的目录还是默认的目录，那接下来按照以下步骤操作：
   在开始菜单中右击notebook图标——更多——打开文件所在位置，在打开的文件夹中右击notebook快捷方式——属性——快捷方式，在目标栏把最后的`“%userprofile%”`去掉。

最后重新启动notebook即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200304115858716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTk0ODQy,size_16,color_FFFFFF,t_70)

**可以在Web端或者资源管理器中，进行文件夹和文件的移动、删除等操作。
右侧点击“upload”可以上传加载本地文件。
右侧点击“new”——“Folder”即可新建文件夹。
右侧点击“new”——“Python 3”即可新建一个ipynb文件。**

### 可以安装一个代码补齐插件

1 .在命令行中激活代码补全环境(注: 如果是使用默认环境，则不用激活)

2 .安装 **nbextension** 

```
pip install jupyter_nbextension_configurator
```
```
jupyter  nbextensions_configurator enable --user
```
3 . 启动 jupyter ，在弹出的页面里，可以看到Nbextension标签页，在这个页面，勾选Hinterland即启动了代码自动补齐，如果页面中没有Hinterland，或者让不全
在命令行中执行指令

```
jupyter contrib nbextension install --user --skip_running-check
```
再次重启jupyter,Nbextension标签页中的数据全部出现
