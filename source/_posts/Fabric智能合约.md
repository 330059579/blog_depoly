---

title: Fabric智能合约
date: 2020-01-19 14:29:15
tags: 
  - 区块链
  - Fabric
description: 
cover: http://120.27.226.80:8088/images/1579415531031.jpg
top_img: http://120.27.226.80:8088/images/1579415531031.jpg
categories: 区块链
---

## **1.什么是智能合约**

目前区块链网络中存在的最有影响力的两大公链，是比特币和以太坊。其中，比特币区块链可以视为区块链1.0版本的代表，区块加载的账本信息比较单一，集中于认证新币产生以及旧币在账户间转移，并无其他更多功能。而以太坊可以视为是2.0版本的区块链，不在仅仅局限于数字货币，二十打造一种能用于各种领域的生态，其核心工具就是所谓的“智能合约”。

智能合约本质上一套软件程序，是基于区块链的，并且会在区块链检测到某些特定数据条件下时会触发。

 在Hyperledgre Fabric中，智能合约又被称为链码，链码有如下的执行过程。

1.链码将会被一个授权的成员安装并实例化到一台Peer节点服务上。

2.普通的业务人员可以使用SDK客户端与Peer节点进行交互，调用智能合约的方法。

3.智能合约在事务中运行，如果一旦被验证且被验证的结果集被发送到Orderer节点，那么其运行结果的变化将会被共享到Fabric网络中的所有Peer节点，从而改变World State。

## **2.智能合约的编写**

 **2.1智能合约接口**

每个智能合约都必须实现Chaincode接口，该接口方法被调用来响应接收的事务，例如当智能合约接收instantiate时，将调用Init方法。

**2.2 go语言编写智能合约**

首先在本地准备好go语言的安装环境

具体事例如下：

![](http://120.27.226.80:8088/images/1579415648262.png)
接下来要继续实现智能合约的初始化方法

![](http://120.27.226.80:8088/images/1579415700050.png)
这里的Init方法是ChaincodeStubInterface的实现，实例化链码时会调用这个方法。

![](http://120.27.226.80:8088/images/1579415726864.png)
Invoke方法会根据传入的参数选择调用具体哪一个方法，这里分别实现了四个方法，具体的实现如下：

![](http://120.27.226.80:8088/images/1579415771941.png)
查询账户余额：

![](http://120.27.226.80:8088/images/1579415820171.png)以下略去1000字。。。。。

**3.链码安装**

链码安装需要先安装好Fabric的环境，然后启动节点的服务

**3.1链码安装**

先进入docker

```shell
docker exec -it cli bash 
```



执行命令如下：

```shell
peer chaincode install -n mycc -v 1.0 -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02
```

**3.2链码实例化**

先设置一下CA证书的环境变量

```shell
ORDERER_CA=/opt/gopath/src/

github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
```
执行命令实例化链码
```shell
peer chaincode instantiate -o orderer.example.com:7050 --tls true --cafile $ORDERER_CA -C mychannel -n mycc -v 1.0 -c '{"Args":["init"]}' -P "OR	('Org1MSP.member','Org2MSP.member')
```



**3.3测试是否安装成功**

查询初始账户：

```shell
peer chaincode query -C mychannel -n mycc -c '{"Args":["find","GenesisAccount"]}'
```

![](http://120.27.226.80:8088/images/1579415873921.png)

创建账户：账户名为A1

```shell
peer chaincode invoke -o orderer.example.com:7050  --tls true --cafile $ORDERER_CA -C mychannel -n mycc -c '{"Args":["create","A1"]}'
```

![](http://120.27.226.80:8088/images/1579416105516.png)

查询账户A1

```shell
peer chaincode query -C mychannel -n mycc -c '{"Args":["find","A1"]}'
```

![](http://120.27.226.80:8088/images/1579416146649.png)


