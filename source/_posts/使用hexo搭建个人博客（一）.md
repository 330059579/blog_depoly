---
title: 使用hexo搭建个人博客（一）
date: 2020-04-14 11:44:33
tags: Hexo
description: 
cover: http://120.27.226.80:8088/images/1586844828426.jpg
top_img: http://120.27.226.80:8088/images/1586844828426.jpg
categories: 工具使用
---

**1.环境准备**

安装前要保证机器上要安装好 nodejs 和 git

**2.下载Hexo**

在电脑上新建一个文件夹用作博客的根目录

![](http://120.27.226.80:8088/images/1586842342980.png)

npm 是一个包管理工具，因此我们可以通过 npm 工具下载 Hexo 框架

执行命令

```shell
npm install -g hexo
```

查看hexo是否安装成功

```shell
hexo -v
```

![](http://120.27.226.80:8088/images/1586842785535.png)

**3.运行Hexo**

```shell
hexo init //命令可以将该文件夹初始化为 Hexo 根目录
```

![](http://120.27.226.80:8088/images/1586843171087.png)

博客根目录下已经初始化了博客的目录

![](http://120.27.226.80:8088/images/1586843265259.png)

```shell
npm install //命令可以自动安装依赖列表中列出的所有模块
```

![](http://120.27.226.80:8088/images/1586843330002.png)

本地运行：

```shell
hexo s
```

![](http://120.27.226.80:8088/images/1586843468997.png)

访问 http://localhost:4000/  页面如下：

![](http://120.27.226.80:8088/images/1586843613616.png)

**4.Hexo 远端部署：**

首先准备一个github账号，并且创建一个仓库，仓库的名字必须为：github账户名 + github.io

例如：

![](http://120.27.226.80:8088/images/1586843969754.png)

执行命令安装发布插件

```shell
npm install hexo-deployer-git --save
```



修改配置文件

hexo的基本配置都在 _config.yml 文件中，打开配置文件，找到deploy相关配置

![](http://120.27.226.80:8088/images/1586844387552.png)

修改如下：

![](http://120.27.226.80:8088/images/1586844504906.png)



git bash 中输入命令 `hexo g` 生成静态文件，输入命令 `hexo d` 部署到 Github

发布成功：

![](http://120.27.226.80:8088/images/1586845214320.png)

此时，打开 [https://330059579.github.io](https://username.github.io/) 就可以看见已经部署好的网站了，大功告成！

![](http://120.27.226.80:8088/images/1586844720526.png)