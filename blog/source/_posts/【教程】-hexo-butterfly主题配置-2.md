---
title: (1)"hexo-butterfly主题配置"
tags:
  - hexo
categories:
  - 教程
toc: true
date: 2020-04-25 15:09:21
top:
cover: https://img-blog.csdnimg.cn/20200425155508238.png
---
## Page Front-matter(生成新的Page时的配置)

```md
---
title:
date:
type: (tags,link,categories這三個頁面需要配置)
comments: (是否需要顯示評論，默認true)
description:
top_img: (設置頂部圖)
mathjax:
katex:
aside:
---



```
## Post Front-matter(用于生成post文章的配置和某些状态设置)

```md
---
title:
date:
tags:
categories:
keywords:
description:
top_img: (除非特定需要，可以不寫)
comments  是否顯示評論(除非設置false,可以不寫)
cover:  縮略圖
toc:  是否顯示toc (除非特定文章設置，可以不寫)
toc_number: 是否顯示toc數字 (除非特定文章設置，可以不寫)
copyright: 是否顯示版權 (除非特定文章設置，可以不寫)
mathjax:
katex:
hide:
---

```

## 标签页
1. 打开命令行窗口
2. 切换到到博客`根目录` 输入指令 `hexo new page tags` (用于生成标签tags的Page)
3. 然后你会找到`source/tags/index.md`
4. 在这个路径下修改写下这个文件内容


```md
---
title: 標籤
date: 2018-01-05 00:00:00
type: "tags"
---



```
## 分类页 

1. 切换到hexo 博客`根目录`
2. 输入 `hexo new page categories`
3. 然后你会找到 `source/categories/index.md`
4. 修改写下

```md
---
title: 分類
date: 2018-01-05 00:00:00
type: "categories"
---

```

## 友情链接

**创建友情链接`页面`**

1. 切换到hexoboke`根目录`
2. 输入`hexo new page link`
3. 你会找到`source/link/index.md` 这个文件
4. 修改写入

```md
---
title: 友情鏈接
date: 2018-06-07 22:17:49
type: "link"
---


```

友情页面链接`添加`

再hexo博客 目录中`source/_data`路径中创建一个文件`link.yml`

```yml
class:
  class_name: 友情鏈接
  link_list:
    1:
      name: xxx
      link: https://blog.xxx.com
      avatar: https://cdn.xxxxx.top/avatar.png
      descr: xxxxxxx
    2:
      name: xxxxxx
      link: https://www.xxxxxxcn/
      avatar: https://xxxxx/avatar.png
      descr: xxxxxxx  

 class2:
   class_name: 鏈接無效
   link_list:
     1:
       name: 夢xxx
       link: https://blog.xxx.com
       avatar: https://xxxx/avatar.png
       descr: xxxx
     2:
       name: xx
       link: https://www.axxxx.cn/
       avatar: https://x
       descr: xx


```

友情鏈接界面設置
由 2.2.0 起，友情鏈接界面可以由用户自己自定義，只需要在友情鏈接的 md 檔設置就行，以普通的 Markdown 格式書寫


## 導航菜單

配置 `butterfly.yml`文件

```md
menu:
  Home: / || fa fa-home
  Archives: /archives/ || fa fa-archive
  Tags: /tags/ || fa fa-tags
  Categories: /categories/ || fa fa-folder-open
  Link: /link/ || fa fa-link
  About: /about/ || fa fa-heart
  List||fa fa-list:
    - Music || /music/ || fa fa-music
    - Movie || /movies/ || fa fa-film

```

**注意格式**

`一般链接：`

    显示名称: /路径/ || 图标名


`多级目录(sub-menu)的格式:`


    名称 ||icon:
      名称 || /路径/ || icon
      名称 || /路径/ || icon
              ...
        ...

**注意**：导航的`显示文字`可以修改，如可以写为中文 "标签"或英文"tags"

例如

```md
menu:
  首頁: / || fa fa-home
  時間軸: /archives/ || fa fa-archive
  標籤: /tags/ || fa fa-tags
  分類: /categories/ || fa fa-folder-open
  清單||fa fa-heartbeat:
    - 音樂 || /music/ || fa fa-music
    - 照片 || /Gallery/ || fa fa-picture-o
    - 電影 || /movies/ || fa fa-film
  友鏈: /link/ || fa fa-link
  關於: /about/ || fa fa-heart

```
![显示图片](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-doc-menu.png)


# 代码

## 代碼高亮主題

Butterfly 支持了 Material Theme 全部 5 種代碼高亮樣式：

- default
- darker
- pale night
- light
- ocean

配置 `butterfly.yml`
```md
highlight_theme: light

# 可修改 为 light 或其它高亮风格

```

default

![](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-doc-code-default.png)



darker


![](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-doc-code-darker.png)

pale night

![](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-doc-code-pale-night.png)

light

![](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-doc-code-light.png)

ocean

![](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-doc-highlight-ocean.png)

## 代碼複製

配置 `butterfly.yml`

```yml
highlight_copy: true

```

![](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-doc-code-copy.png)

## 代碼框展開 / 關閉

在默認情況下，代碼框自動展開，可設置是否所有代碼框都關閉狀態，點擊 > 可展開代碼

配置 `butterfly.yml`

```yml
highlight_shrink: true #代碼框不展開，需點擊 '>' 打開

```

```yml
highlight_shrink: true

```
显示效果

![](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-doc-highlight-shrink-true.png)


`highlight_shrink: false`

![](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-doc-highlight-shrink-false.png)

`highlight_shrink: none`

![](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-docs-highlight-shirk-none.png)

## 代碼換行

在默认情况下，hexo-highlight 在编译的时候不会实现代码自动换行。如果你不希望在代码块的区域里有横向滚动条的话，那么你可以考虑开启这个功能。

`配置 butterfly.yml`

```yml
code_word_wrap: true

```

```yml
highlight:
  enable: true
  line_number: false
  auto_detect: false
  tab_replace:


```


然后找到你站点的 Hexo 配置文件`_config.yml`，将 line_number 改成` false`:

>设置 code_word_wrap 之前:

![](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-doc-code-word-wrap-before.png)

>设置 code_word_wrap 之后:

![](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-doc-code-word-wrap-after.png)

未完待续...

# 参考：

[jerryc.me](https://jerryc.me/posts/4aa8abbe/#%E4%BB%A3%E7%A2%BC%E6%8F%9B%E8%A1%8C)

[butterfly主题设置](https://www.lucfzy.com/2020/02/butterfly-theme/#%E5%8F%8B%E6%83%85%E9%93%BE%E6%8E%A5%E7%95%8C%E9%9D%A2%E8%AE%BE%E7%BD%AE)

**butterfly主题**

[butterfly主题基础设置](https://www.cnblogs.com/milovetingting/p/12159100.html)

[主题优化](https://www.larscheng.com/butterfly/)

[butterfly的创作者的教程](https://jerryc.me/posts/31391d01/#%E5%AD%97%E6%95%B8%E7%B5%B1%E8%A8%88%E5%92%8C%E9%96%B2%E8%AE%80%E6%99%82%E9%95%B7-%E7%B6%B2%E7%AB%99%E5%BA%95%E9%83%A8-%E6%96%87%E7%AB%A0%E5%86%85)

[butterfly发布页面的配置](https://jerryc.me/posts/4aa8abbe/#%E5%B0%8E%E8%88%AA%E8%8F%9C%E5%96%AE)

[主题页面设置-网页顶置-友链-音乐-相册](https://ciweigg2.github.io/2019/07/04/hexo-theme-butterfly-zhu-ti-an-zhuang-ya/)

[butterfly友链设置](https://www.larscheng.com/butterfly/)