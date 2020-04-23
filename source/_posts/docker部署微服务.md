---
title: docker部署微服务
date: 2020-01-06 16:10:29
tags: 
  - Java
  - docker
description: 
cover: https://pic2.zhimg.com/80/v2-12be1f41447fed73f7c7ced78b30ed5f_hd.jpg
top_img: https://pic2.zhimg.com/80/v2-12be1f41447fed73f7c7ced78b30ed5f_hd.jpg
categories: 运维
---

**1.docker架构**
![](http://120.27.226.80:8088/images/1.png)

分成docker客户端，宿主机端和仓库

宿主机就是安装docker的物理机器，用户可以通过docker客户端提供的命令 通过daemon守护线程对宿主机进行操作。仓库：类似于maven仓库，存放经过docker包装过后的各种软件

**2.首先部署好docker环境**

略。。

**3.配置镜像加速器**

进入docker目录， cd /etc/docker/

![](http://120.27.226.80:8088/images/2.png)

创建文件daemon.json，输入如下内容并保存

```json
{"registry-mirrors":["https://m9r2r2uj.mirror.aliyuncs.com"]}
```

![](http://120.27.226.80:8088/images/3.png)

重启docker服务

```shell
service docker  restart
```

**4.运行docker 容器**

首先安装java8镜像

输入命令：

```shell
docker pull java:8
```

![](http://120.27.226.80:8088/images/4.png)

查看本地镜像命令：

```shell
docker images
```

运行容器：即将刚才下载的镜像软件，放到容器中运行

命令：docker run 。。。。

```shell
docker run -p 91:80 nginx
```

docker run 新建并运行容器，容器里的镜像由自己指定，这个指定为nginx， 如果本地没有，会自动下载。

-p 端口映射，将宿主机的91端口映射到容器的80端口

![](http://120.27.226.80:8088/images/7.png)

docker ps  列出正在运行的容器：

**5.构建自己的镜像**

本机创建一个目录，进入目录创建一个Dokcerfile文件

![](http://120.27.226.80:8088/images/8.png)

编辑Dockerfile文件，输入内容如下：

```dockerfile
FROM nginx

RUN echo '<h1>This is Tuanzhang Nginx</h1>' > /usr/share/nginx/html/index.html
```

![](http://120.27.226.80:8088/images/9.png)

FROM指定了基于哪一个镜像来构建，我们不是从头开始做，这里 指定了本地nginx作为基础镜像，RUN 指定了构建时容器中运行的命令，这里是将<h1>This is Tuanzhang Nginx</h1>这句话输入到了/usr/share/nginx/html/index.html文件中

在目录下运行docker命令：

```shell
docker build -t nginx:tuanzhang  .
```

参数-t 制定镜像名字

通过docker images 命令可以看到我们自己的镜像 已经打包

![](http://120.27.226.80:8088/images/10.png)

```shell
docker run -d -p 94:80 nginx:tz
```

浏览器中访问如下：

![](http://120.27.226.80:8088/images/11.png)

**5.用docker部署web 应用**

本地准备一个简单的springboot工程

提供一个测试接口：

![](http://120.27.226.80:8088/images/12.png)

1.将工程的可执行jar包上传到服务器：

![](http://120.27.226.80:8088/images/13.png)

2.创建一个Dockerfile文件,内容如下：

![](http://120.27.226.80:8088/images/14.png)

From java:8                            指定基于哪个镜像

VOLUME /temp                      挂载了一个共享的目录

ADD testdocker.jar /app.jar    把当前文件夹下的工程jar包 改名为app.jar放到容器中

EXPOSE 8081                       暴漏端口

ENTRYPOINT ["java","-jar","/app.jar"]           容器启动后要执行的命令

3.编译打包

docker build -t testdocker:0.0.1  .

![](http://120.27.226.80:8088/images/15.png)

查看本地docker镜像

![](http://120.27.226.80:8088/images/16.png)

4.运行

docker run -d -p 8081:8081 testdocker:0.0.1

访问服务器上的项目：

![](http://120.27.226.80:8088/images/17.png)