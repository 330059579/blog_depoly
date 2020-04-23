---
title: Spark伪分布式搭建
date: 2020-01-08 16:42:48
tags: 
  - 大数据
  - Spark
description: 
cover: http://120.27.226.80:8088/images/1578473041221.jpg
top_img: http://120.27.226.80:8088/images/1578473041221.jpg
categories: 大数据
---

1.安装hadoop

安装Spark前，服务器上要有可与运行的hadoop

2.根据hadoop版本上传相应的sparkjar包,并解压

![](http://120.27.226.80:8088/images/1578473125167.png)



3.进入spark-2.4.3-bin-hadoop2.6/conf 目录

修改两个文件名

```shell
cp spark-env.sh.template spark-env.sh

cp slaves.template slaves
```

修改spark-env.sh内容

在文件最后添加

```shell
 export JAVA_HOME=jdk路径
 export SPARK_MASTER_HOST=master
 export SPARK_MASTER_PORT=7077
```

修改slaves目录

将localhost换为master

![](http://120.27.226.80:8088/images/1578473183959.png)

进入spark-2.4.3-bin-hadoop2.6/sbin

```shell
./start-all.sh
```

![](http://120.27.226.80:8088/images/1578473239581.png)

![](http://120.27.226.80:8088/images/1578473266703.png)

运行成功

浏览器查看后台

http://192.168.111.140:8080/

![](http://120.27.226.80:8088/images/1578473326296.png)

