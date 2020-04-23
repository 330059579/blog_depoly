---
title: 使用hexo搭建个人博客（四）
date: 2020-04-23 17:20:19
tags: Hexo
description: 
cover: http://120.27.226.80:8088/images/1586914697695.jpg
top_img: http://120.27.226.80:8088/images/1586914697695.jpg
categories: 工具使用
---

###  一、创建文章

博客根目录下输入

```shell
hexo new 文章标题
```

当输入命令后，就会在 source/_post 文件夹下创建一个文件，命名为：文章标题.md

![](http://120.27.226.80:8088/images/1587634037443.png)

![](http://120.27.226.80:8088/images/1587634215832.png)

这个文件就是将要发布到网站上的原始文件，用于记录文章内容

下面，我们将要在这个文件中写下我们的第一篇博客

### 二、编写文章（基于 Markdown）

博客的内容采用Markdown语法来编写，这里简单介绍一下Markdown的语法

注：再编写Markdown是推荐使用 Markdown 编辑器 —— Typora

下载地址：https://www.typora.io/，有兴趣的朋友可以下载来试试

#### （1）标题

```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

**Typora 快捷键：**

Ctrl+1：一级标题

Ctrl+2：二级标题

Ctrl+3：三级标题

Ctrl+4：四级标题

Ctrl+5：五级标题

Ctrl+6 ：六级标题

Ctrl+0：段落

#### （2）粗体、斜体、删除线和下划线

```
*斜体*
**粗体**
***加粗斜体***
~~删除线~~
```

**Typora 快捷键：**

Ctrl+I：斜体

Ctrl+B：粗体

Ctrl+U：下划线

Alt+Shift+5：删除线



#### （3）引用块

 ```
> 文字引用
 ```

**Typora 快捷键：** Ctrl+Shift+Q

#### （4）代码块

```
`行内代码`

​```
多行代码
多行代码
​```
```

**Typora 快捷键：**

行内代码：Ctrl+Shift+`

多行代码：Ctrl+Shift+K

#### （5）公式块

```
$$
数学公式
$$
```

**Typora 快捷键：** Ctrl+Shift+M

#### （6）分割线

```
方法一：---

方法二：+++

方法三：***
```

#### （7）列表

```
1. 有序列表项

* 无序列表项

+ 无序列表项

- 无序列表项
```

**Typora 快捷键：**

有序列表项：Ctrl+Shift+[

无序列表项：Ctrl+Shift+]

#### （8）表格

```
表头1|表头2
-|-|-
内容11|内容12
内容21|内容22
```

**Typora 快捷键：** Ctrl+T

#### （9）超链接

```
方法一：[链接文字](链接地址 "链接描述")
例如：[示例链接](https://www.example.com/ "示例链接")

方法二：<链接地址>
例如：<https://www.example.com/>
```

**Typora快捷键：** Ctrl+K

#### （10）图片

```
![图片文字](图片地址 "图片描述")
例如：![示例图片](https://www.example.com/example.PNG "示例图片")
```

Typora快捷键： Ctrl+Shift+I

说明：在 Hexo中 插入图片时，请按照以下的步骤进行设置

将 站点配置文件 中的 post_asset_folder 选项的值设置为 true

在站点文件夹中打开 git bash，输入命令 npm install hexo-asset-image --save 安装插件

这样，当使用 hexo new title 创建文章时，将同时在 source/_post 文件夹中生成一个与 title 同名的文件夹，我们只需将图片放进此文件夹中，然后在文章中通过 Markdown 语法进行引用即可

例如，在资源文件夹（就是那个与 title 同名的文件夹）中添加图片 example.PNG，则可以在对应的文章中使用语句 ![示例图片](title/example.PNG "示例图片") 添加图片


