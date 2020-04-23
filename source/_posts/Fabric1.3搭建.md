---
title: Fabric1.3搭建
date: 2020-01-19 14:00:49
tags: 
  - 区块链
  - Fabric
description: 
cover: http://120.27.226.80:8088/images/1579414020964.jpg
top_img: http://120.27.226.80:8088/images/1579414020964.jpg
categories: 区块链
---

### 安装依赖环境

**1.1安装go语言环境（版本需要 1.10.x）**

```shell
wget https://storage.googleapis.com/golang/go1.10.2.linux-amd64.tar.gz

tar -C /usr/local -xzf go1.10.2.linux-amd64.tar.gz
```

编辑当前用户的环境变量

```shell
vi /etc/profile
```

添加以下内容

```shell
export GOROOT=/usr/local/go 

export GOPATH=$HOME/go 

export PATH=$PATH:$HOME/go/bin
```

编辑保存并退出vi后，记得使这些环境变量生效

```shell
source /etc/profile
```

![](http://120.27.226.80:8088/images/1579414189811.png)


把go的目录GOPATH设置为当前用户的文件夹下，所以记得创建go文件夹

```shell
cd ~
mkdir go
```

**1.2 安装CURL**

```shell
sudo apt install curl
```

**1.3安装Docker（版本要求17.06.2-ce 或更高）**

```shell
sudo apt install docker.io
docker --version
```

![](http://120.27.226.80:8088/images/1579414342432.png)


**1.4安装Docker Compose**

```shell
sudo apt install docker-compose
```

查看版本

```shell
docker-compose --version
```

![](http://120.27.226.80:8088/images/1579414459997.png)


**1.5安装Python**

```shell
sudo apt-get install python
python --version
```

![](http://120.27.226.80:8088/images/1579414546786.png)
**1.6安装nvm**

```shell
sudo apt update

curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.10/install.sh | bash

export NVM_DIR="$HOME/.nvm"

[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" 
```

**1.7安装Node**

```shell
nvm install v8.11.1
```

查看node版本

```shell
node -v
```

![](http://120.27.226.80:8088/images/1579414634476.png)



### 安装fabric1.3

**2.1下载官方fabric-samples工程**

```shell
mkdir -p ~/go/src/github.com/hyperledger/

cd ~/go/src/github.com/hyperledger/

git clone https://github.com/hyperledger/fabric-samples.git
```

![](http://120.27.226.80:8088/images/1579414753964.png)


**2.2安装项目依赖镜像**

```shell
curl -sSL https://raw.githubusercontent.com/hyperledger/fabric/master/scripts/bootstrap.sh | bash -s 1.3.0
```

![](http://120.27.226.80:8088/images/1579414804210.png)**2.3启动demo**

```shell
cd fabric-samples/first-network

./byfn.sh generate

./byfn.sh up
```

![](http://120.27.226.80:8088/images/1579414874795.png)
