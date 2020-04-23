---
title: 使用hexo搭建个人博客（二）
date: 2020-04-14 14:18:06
tags: Hexo
description: 
cover: http://120.27.226.80:8088/images/1586914697695.jpg
top_img: http://120.27.226.80:8088/images/1586914697695.jpg
categories: 工具使用
---

**为博客更换主题**

​	hexo默认的主题丑的一笔，简直不能忍，网上很多关于切换Next的主题教程，感兴趣可以自己看一下。github上有很多的开源的主题，这里我们选一个页面酷炫的主题Butterfly，文档地址如下：https://jerryc.me/posts/21cfbf15/，具体功能可以感兴趣可以自己仔细看，这里直说主要步骤：

**1.下载主题**

在博客根目录中，执行如下命令

```shell
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly
```

主题下载完成，存储到themes目录下：

![](http://120.27.226.80:8088/images/1586846531873.png)

修改站點配置文件_config.yml，把主題改為 Butterfly

![](http://120.27.226.80:8088/images/1586846619104.png)

Butterfly还需要下载stylus渲染器，否则页面无法渲染

```shell
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

**2.配置平滑升级**

为了主题的平滑升级，Butterfly 使用了 data files 特性。推荐把主题配置文件config.yml 复制到 Hexo 工作目录下的 source/_data/butterfly.yml（注意：这里是主题的配置文件不是hexo的配置文件）

![](http://120.27.226.80:8088/images/1586847619205.png)

如果 _data目录不存在，则创建一个。此时butterfly.yml会替换config.yml 中的配置，若主题有更新，直接执行git pull 就可以平滑升级Butterfly。

运行 `hexo s` 打开浏览器显示如下：

![](http://120.27.226.80:8088/images/1586847703222.png)

还有很多关于页面显示的配置，有兴趣可以自己根据文档研究，这里不再重述



**3.为Hexo 添加萌萌的看板娘**

之前看到别人的博客上下角会出现一个看板娘，研究了一下，发现并不难搞，直接将代码加载到页面中就好，于是很开心的打算给自己的博客也做一个，结果打开butterfly的页面文件直接傻眼：

![](http://120.27.226.80:8088/images/1586848056675.png)

![](http://120.27.226.80:8088/images/1586848096023.png)

？？？

由于我不是专业前端，看不懂这是用啥语言写的，直接懵逼

不过还好我经验丰富，经过我仔细观察找到了这个文件

..\themes\Butterfly\source\js\main.js

猜测所有的页面都会加载这个文件，于是。。

打开main.js，将如下代码复制进去：

```js
 //注意：live2d_path参数应使用绝对路径
const live2d_path = "https://cdn.jsdelivr.net/gh/stevenjoezhang/live2d-widget/";
//const live2d_path = "/live2d-widget/";

//封装异步加载资源的方法
function loadExternalResource(url, type) {
	return new Promise((resolve, reject) => {
		let tag;

		if (type === "css") {
			tag = document.createElement("link");
			tag.rel = "stylesheet";
			tag.href = url;
		}
		else if (type === "js") {
			tag = document.createElement("script");
			tag.src = url;
		}
		if (tag) {
			tag.onload = () => resolve(url);
			tag.onerror = () => reject(url);
			document.head.appendChild(tag);
		}
	});
}

//加载waifu.css live2d.min.js waifu-tips.js
Promise.all([
	loadExternalResource(live2d_path + "waifu.css", "css"),
	loadExternalResource(live2d_path + "live2d.min.js", "js"),
	loadExternalResource(live2d_path + "waifu-tips.js", "js")
]).then(() => {
	initWidget(live2d_path + "waifu-tips.json", "https://live2d.fghrsh.net/api");
});
//initWidget第一个参数为waifu-tips.json的路径，第二个参数为api地址
//api后端可自行搭建，参考https://github.com/fghrsh/live2d_api
//初始化看板娘会自动加载指定目录下的waifu-tips.json

console.log(`
  く__,.ヘヽ.        /  ,ー､ 〉
           ＼ ', !-─‐-i  /  /´
           ／｀ｰ'       L/／｀ヽ､
         /   ／,   /|   ,   ,       ',
       ｲ   / /-‐/  ｉ  L_ ﾊ ヽ!   i
        ﾚ ﾍ 7ｲ｀ﾄ   ﾚ'ｧ-ﾄ､!ハ|   |
          !,/7 '0'     ´0iソ|    |
          |.从"    _     ,,,, / |./    |
          ﾚ'| i＞.､,,__  _,.イ /   .i   |
            ﾚ'| | / k_７_/ﾚ'ヽ,  ﾊ.  |
              | |/i 〈|/   i  ,.ﾍ |  i  |
             .|/ /  ｉ：    ﾍ!    ＼  |
              kヽ>､ﾊ    _,.ﾍ､    /､!
              !'〈//｀Ｔ´', ＼ ｀'7'ｰr'
              ﾚ'ヽL__|___i,___,ンﾚ|ノ
                  ﾄ-,/  |___./
                  'ｰ'    !_,.:
`);
```

重新运行项目：

![](http://120.27.226.80:8088/images/1586848774444.png)

左下角出现，快把萌萌的看板娘抱回家~~~~