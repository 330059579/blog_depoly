---
title: Hexo搭建个人博客（三）
date: 2020-04-23 15:42:00
tags: Hexo
description: 
cover: http://120.27.226.80:8088/images/1586914697695.jpg
top_img: http://120.27.226.80:8088/images/1586914697695.jpg
categories: 工具使用
---

### 1.创建标签页

不管是hexo的那种主题，页面上一般都会有一个标签的导航栏，点击会展示我们再博客正文中添加的标签，例如之前Butterfly主题的页面上：

![](http://120.27.226.80:8088/images/1587628675640.png)

点击以后页面如下：

![](http://120.27.226.80:8088/images/1587628802646.png)

这是因为没有配置标签页面的原因，再博客根目录输入如下命令

```shell
hexo new page tags
```

![](http://120.27.226.80:8088/images/1587628991754.png)

再source/tags/目录下会出现index.md文件

![](http://120.27.226.80:8088/images/1587629457410.png)

修改文件内容如下

![](http://120.27.226.80:8088/images/1587629578664.png)

运行查看：页面正常显示

![](http://120.27.226.80:8088/images/1587630511269.png)

### 2.创建分类页

博客根目录下执行如下命令

```shell
hexo new page categories
```

你会找到 source/categories/index.md 這個文件，修改内容如下：

![](http://120.27.226.80:8088/images/1587632560521.png)

运行查看：

![](http://120.27.226.80:8088/images/1587632645485.png)

### 3.音乐

音乐页面使用了 hexo-tag-aplayer插件，这里简单说明一下使用方法

执行命令安装 hexo-tag-aplayer插件

```shell
npm install --save hexo-tag-aplayer
```

博客根目录下运行

```shell
hexo new page music
```

再source\music目录下生成了index.md

向其中写入如下内容

```
{% aplayer "她的睫毛" "周杰伦" "http://home.ustc.edu.cn/~mmmwhy/%d6%dc%bd%dc%c2%d7%20-%20%cb%fd%b5%c4%bd%de%c3%ab.mp3"  "http://home.ustc.edu.cn/~mmmwhy/jay.jpg"  %}
```

![](http://120.27.226.80:8088/images/1587632962186.png)

运行查看

![](http://120.27.226.80:8088/images/1587633446770.png)

