---
title: Fabric升级智能合约
date: 2020-01-19 15:33:21
tags: 
  - 区块链
  - Fabric
description: 
cover: http://120.27.226.80:8088/images/1579419493842.jpg
top_img: http://120.27.226.80:8088/images/1579419493842.jpg
categories: 区块链
---

fabric 允许管理员调用升级命令，将在某个Channel上已经实例化的智能合约升级到一个新的版本

链码一般在首次安装时，会确定链码的名称和版本，例如：

```shell
peer chaincode install -l java -n mycc -p /opt/gopath/src/github.com/hyperledger/fabric/tuanzhang/chaincode/java -v 1.0
```

此处链码为java编写，需要加上-l java参数（go不用）  这里定义了链码的名称为mycc  版本为1.0

版本1的链码如下：

![](http://120.27.226.80:8088/images/1579419781375.png)
![](http://120.27.226.80:8088/images/1579419805971.png)
版本1.0初始化时，先初始化A,B两个值，均为10

![](http://120.27.226.80:8088/images/1579419889862.png)
执行一次转账 A向B转5块钱

![](http://120.27.226.80:8088/images/1579419919240.png)


现在区块链上的数据是A = 5, B=15

升级合约，对合约稍作修改：

防止升级合约时，将数据再次初始化

![](http://120.27.226.80:8088/images/1579419978261.png)
添加一个测试方法：

![](http://120.27.226.80:8088/images/1579420088437.png)将新的链码上传到链码存放的指定目录

首先将新的链码安装到节点上，这里要保证链码的名字同之前一致，版本修改为2.0

```shell
peer chaincode install -l java -n mycc -p /opt/gopath/src/github.com/hyperledger/fabric/tuanzhang/chaincode/java -v 2.0
```

![](http://120.27.226.80:8088/images/1579420123076.png)


执行升级命令：

```shell
peer chaincode upgrade -o orderer.example.com:7050 -C testupdate -n mycc -l java -c '{"Args":["init","upgrade"]}' -P "OR('Org1MSP.peer','Org2MSP.peer')" -v 2.0
```

![](http://120.27.226.80:8088/images/1579420224473.png)链码升级成功。

测试一些新加入的方法：

```shell
peer chaincode invoke -C testupdate-n mycc-c '{"Args":["testupgrade"]}'
```

![](http://120.27.226.80:8088/images/1579420262194.png)
查询A，B的余额

![](http://120.27.226.80:8088/images/1579420287639.png)


![](http://120.27.226.80:8088/images/1579420330958.png)
A,B的余额没有被重新初始化