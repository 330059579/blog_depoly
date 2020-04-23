---
title: Hadoop搭建
date: 2020-01-08 16:18:35
tags: 
  - 大数据
  - Hadoop
description: 
cover: http://120.27.226.80:8088/images/1578471753684.jpg
top_img: http://120.27.226.80:8088/images/1578471753684.jpg
categories: 大数据
---

**软件版本**

ubuntu16.04

安装jdk1.8

hadoop-2.6.4

**1.准备服务器环境**

将一台linux服务器安装好jdk1.8后克隆成三份

将三台主机名分别命名为 Master，hadoop2 ，hadoop3 

```shell
vim /etc/hostname
```

重启系统使配置生效

修改Host文件，三台服务器添加配置如下

```shell
vim /etc/hosts

192.168.111.139  Master   
192.168.111.136  hadoop2 
192.168.111.137  hadoop3 
```

**2.配置Master对hadoop2和hadoop3的免密登录**

一般情况下ubuntu系统是不允许root用户远程登录的，为了以后操作方便，我们要配置允许root的远程登录

```shell
apt-get install ssh
vim /etc/ssh/sshd_config
```

找到PermitRootLogin without-password 修改为PermitRootLogin yes

![](http://120.27.226.80:8088/images/1578471931837.png)

继续执行如下命令：

```shell
/etc/init.d/ssh restart
```

现在可以直接使用xShell远程登录root用户

配置免密登录

每台机器执行如下命令，然后回车

```shell
ssh-keygen -t rsa
```

![](http://120.27.226.80:8088/images/1578472085943.jpg)

生成的公钥私钥都保存在～/.ssh下

在master上将公钥放入authorized_keys，命令如下：

```shell
cat ～/.ssh/id_rsa.pub > ~/.ssh/authorized_keys
```

将master上的authorized_keys放到其它机器上

```shell
scp ~/.ssh/authorized_keys hadoop2:~/.ssh
scp ~/.ssh/authorized_keys hadoop3:~/.ssh
```

测试是否成功

![](http://120.27.226.80:8088/images/1578472194593.png)

**3.安装Hadoop**

新建目录：

```shell
mkdir ~/apps
```

上传hadoop-2.6.4.tar.gz到此目录，并解压

![](http://120.27.226.80:8088/images/1578472246560.png)

修改/root/apps/hadoop-2.6.4/etc/hadoop 路径下的xml配置文件：

 主要有5个文件要修改：

**hadoop-env.sh**

将JAVA_HOME设置为本地的java安装目录

![](http://120.27.226.80:8088/images/1578472318754.png)

**core-site.xml**

添加配置如下

```xml
<property>
   <name>fs.defaultFS</name>
   <value>hdfs://Master:9000</value>
</property>

<property>
   <name>hadoop.tmp.dir</name>
   <value>/home/hadoop/hdpdata</value>
</property>
```



**hdfs-site.xml**

添加配置如下：

```xml
<property>
   <name>dfs.replication</name>
   <value>2</value>
</property>
```



**mapred-site.xml**

添加配置如下：

```xml
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
</property>
```



**yarn-site.xml**

添加配置如下：

```xml
<property>
   <name>yarn.resourcemanager.hostname</name>
   <value>Master</value>
</property>

<!-- reducer获取数据的方式 -->

<property>
   <name>yarn.nodemanager.aux-services</name>
   <value>mapreduce_shuffle</value>
</property>
```

**slaves**　

![](http://120.27.226.80:8088/images/1578472477519.png)

配置完成后：将apps目录发送待其他两台服务器：

```shell
scp -r apps.tar hadoop2:~
scp -r apps.tar hadoop3:~
```

**4.启动项目**

进入hadoop的安装目录的bin目录下

执行命令

```shell
./hadoop namenode -format
```

(每修改一次配置文件都要重新格式化)

执行启动命令

进入sbin目录下：

```shell
./start-all.sh
```

**5.检查是否启动**

Master服务器输入命令

```shell
jps
```

![](http://120.27.226.80:8088/images/1578472576321.png)

hadoop2服务器下：

![](http://120.27.226.80:8088/images/1578472631294.png)

可以看到线程都已经启动

登录后台：

关闭防火墙

```shell
ufw disable
```

访问下面两个地址

http://192.168.111.139:8088/cluster

![](http://120.27.226.80:8088/images/1578472687810.png)



http://192.168.111.139:50070/dfshealth.html#tab-overview

![](http://120.27.226.80:8088/images/1578472701983.png)

项目搭建完成

