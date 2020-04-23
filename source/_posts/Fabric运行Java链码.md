---
title: Fabric运行Java链码
date: 2020-01-19 15:01:44
tags: 
  - 区块链
  - Fabric
description: 
cover: http://120.27.226.80:8088/images/1579418373359.jpg
top_img: http://120.27.226.80:8088/images/1579418373359.jpg
categories: 区块链
---

注意：java链码需要fabric1.3的运行环境

**1.首先创建一个自己的fabric工程**

```shell
cd ~/go/src/github.com/hyperledger/fabric-samples

mkdir tuanzhang
```

![](http://120.27.226.80:8088/images/1579418507430.png)


**2.进入tuanzhang目录**

```shell
cd tuanzhang
```

**3.将fabric的二进制文件上传到tuzhang目录下，并解压**

![](http://120.27.226.80:8088/images/1579418551172.png)

**4.再将first-network工程中的configtx.yaml文件和crypto-config文件同样拷贝到tuanzhang目录下**

用于生成创世区块和证书文件等

![](http://120.27.226.80:8088/images/1579418619844.png)


**5.再新建一个明为channel-artifacts的文件夹。**

```shell
mkdir channel-artifacts
```



**6.生成证书文件 执行命令如下：**

```shell
./bin/cryptogen generate --config=./crypto-config.yaml
```

完成之后，会生成crypto-config/文件夹，其中包含ordererOrganizations和peerOrganizations两个目录

![](http://120.27.226.80:8088/images/1579418675630.png)

**7.接下来使用bin文件夹中的cinfigtxgen工具执行configtx.yml文件**，在此之前需要现在/etc/profile文件中配置如下环境命令：

```shell
export FABRIC_CFG_PATH=$PWD
```

Orderer排序服务启动时需要用到创世区块，所以先要生成创世区块：

执行命令：

```shell
./bin/configtxgen -profile TwoOrgsOrdererGenesis -outputBlock ./channel-artifacts/genesis.block
```

![](http://120.27.226.80:8088/images/1579418766522.png)


**8.生成Peer节点启动后创建Channel的配置文件**

执行命令如下：

```shell
./bin/configtxgen -profile TwoOrgsOrdererGenesis -outputCreateChannelTx ./channel-artifacts/mychannel.tx -channelID mychannel
```

该命令生成了一个channelID为mychannel的tx文件，通过该文件，peer可以执行channel的创建工作。

**9.编写节点配置文件**

配置文件内容略

![](http://120.27.226.80:8088/images/1579418809019.png)
**10.创建chaincode目录**

![](http://120.27.226.80:8088/images/1579418842881.png)由于我们要运行java链码，我们可以去fabric-samples工程的chaincode/chaincode_example02目录将java链码拷贝到tuanzhang/chaincode下

```shell
cp -r java/ ../../tuanzhang/chaincode/
```

tuanzhang工程中链码目录如下所示

![](http://120.27.226.80:8088/images/1579418887203.png)从目录中可以看到，java链码是一个用gradle搭建的工程

线下打开这个工程，（需要搭建好gradle环境），对链码稍微做一些修改：

![](http://120.27.226.80:8088/images/1579418924870.png)


方法内容如下：

![](http://120.27.226.80:8088/images/1579418959103.png)**用修该过后的链码替换掉原来的链码**

**11.启动节点**

```shell
docker-compose -f docker-orderer.yaml up -d

docker-compose -f docker-peer.yaml up -d

docker-compose -f docker-peer1.yaml up -d
```



进入docker

```shell
docker exec -it cli bash
```



创建channel

```shell
peer channel create -o orderer.example.com:7050 -c mychannel -f ./channel-artifacts/mychannel.tx
```



节点加入通道

```shell
peer channel join -b mychannel.block
```



安装链码

```shell
peer chaincode install -l java -n mychannel -p /opt/gopath/src/github.com/hyperledger/fabric/tuanzhang/chaincode/java -v 1.0
```



注意：链码路径要用绝对路径

![](http://120.27.226.80:8088/images/1579419059714.png)


链码实例化：（线上不需要单独安装gradle或者jdk环境，因为javaenv镜像中已经集成了需要的环境）

```shell
peer chaincode instantiate -o orderer.example.com:7050 -C mychannel -n mychannel -l java -c '{"Args":["init","A","10","B","10"]}' -P "OR ('Org1MSP.member','Org2MSP.member')" -v 1.0
```

![](http://120.27.226.80:8088/images/1579419104220.png)实例化时间略长

测试：

```shell
peer chaincode query -C mychannel -n mychannel -c '{"Args":["test"]}'
```

![](http://120.27.226.80:8088/images/1579419168433.png)
