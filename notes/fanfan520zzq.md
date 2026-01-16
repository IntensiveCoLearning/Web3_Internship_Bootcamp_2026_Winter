---
timezone: UTC+8
---

# fanfan520zzq

**GitHub ID:** fanfan520zzq

**Telegram:** @fannism

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->
```ini
1.16 今天明天都有事

一个节点是如何组成的？ 一台电脑+two clients + PoS

two clients : consensus client + execution client
两个client之间用Engine API 连接
核心组件包括库文件：web3.js , web3.py

执行客户端： 交易执行，EVM操作，状态管理
共识客户端： PoS共识机制，区块提议，验证与最终性

节点间的通讯方式：P2P+gossip
gossip:八卦：人传人扩散传播
工作模式：1.选择：从节点列表中选择几个通信 2.传播：pull，push，push-pull（最常用） 3，更新merge：如果是新信息，继续传播
gossip问题：走神的节点会错过信息

节点与节点之间建立通信：1，udp联通，kademlia（kademlia分布式哈希表协议，XOR距离） 2，tcp稳定联通，RLPx（在一个tcp中运行多个协议） 3.goppip传播

full node : 执行客户端+共识客户端，从创世（可信检查点）开始验证整条chain，用 snap/fast sync补齐再自我验证 ，
archive node : 禁用pruning的全节点。体量很大
light node ： 只保存区块头， 不参与共识 ， 不能做验证者 

RPC: Remote Procedure Call 远程过程调用 ， client 向节点中的服务挂钩并返回结果，client不关心自己的req运行在哪里，如何运行，只要结果。 etherumscan就是一种RPC
```
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->

```ini
solidity stduy;
uint -> 256 bit
set(),get() ： modify or retrieve variable 
public: use variable in other contract
address : 160 bit
了解了一下椭圆曲线加密（ECC），私钥d，公钥Q=kP,P是基点，k是私钥，Q是公钥
加密与解密，报文M，随机数r保密，C1=rP,C2=M+rQ
解密：M=C2-dC1
交易可以看作是从一个帐户发送到另一个帐户的消息 交易包含：一个二进制数据（合约负载），eth
交易没有接收者或者接收者位null，交易会创建一个new contract ， 这种合约的有效负载被认为是EVM字节码并执行
为创建一个合约：您不需要发送实际的合约代码，而使发送能够产生合约代码的代码（不太懂？创造了一个临时函数？）

EVM的三个数据区域：
1. 内存（Memory）：临时存储区域，合约执行期间使用，调用结束后清空。
2. 存储（Storage）：256bit 持久化存储区域，合约的状态变量保存在这里，直到合约被销毁。
3. 堆栈（Stack）：用于存储临时变量和函数调用的返回值，大小有限制（1024个元素）。

EVM不基于寄存器架构，而是基于堆栈架构，这意味着所有操作都是通过堆栈进行的，而不是通过寄存器。
栈的深度限制为1024个元素（每个元素长度为一个字 256位），这意味着在执行复杂的计算时需要小心，以避免栈溢出错误。
对栈的访问仅在顶部，拷贝最顶端16个元素的一个到栈顶，交换栈顶元素和下面16个元素中的一个；其他操作只能取顶上两个，运算后，把结果压在栈顶，无法访问指定深度的元素。
EVM指令集：精简
库：可以放在一个合约的存储中，并且可以被其他合约调用和使用，从而实现代码的重用和模块化设计。

while (voters[to].delegate != address(0)) {
    to = voters[to].delegate;
    require(to != msg.sender, "Found loop in delegation.");
} //链式循环 address(0) 终止

AI:AI agent : LLM+memory+tool
问题：身份欺诈；不可审计；影子AI
M2M微支付
AI协议栈：应用框架层；身份层；交易层
ERC-8004: identity registry ; reputation registry ; validation
合规：持续监控；不可篡改的认证；记录存证
SpoonOS: AI operating system ; AI container ; AI marketplace
```
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->


期末周终于考完了。。。这两天慢慢投入到学习web3及一些别的我感兴趣的方向

今天再看一下ETH概论类的，明天开始学习语法，先从solidty学吧

ETH:协议层 + 应用层 (Protocol Layer + Application Layer)

智能合约：图灵完备，可编程，自动执行

运行合约需要gas 计价单位：Wei

智能合约和dAPP: 智能合约是运行在链上的代码；

dAPP:智能合约+网页前端

抗审查：高权力无法干涉，无法关停

代币的核心认证是基于合约地址

DEX交易：verified;问号或感叹号的token

链上数据分析：学SQL，DUNE；RootDATA

好奇心，坚持基本功（对我来说，一定要坚持）

稳定币：USDT/USDC(centralized) DAI(decentralized)

经济模型漏洞？可套利？

签名：digital signature 证明yourself

私钥：个人保密认证 公钥：身份公认信息

发送，转账：就是签名

问题：每一笔交易都伴随着ECDSA,TPS增加，签名会越来越慢，硬件未来？？？
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->



1.13 期末周倒数day2 忙

基本完成了unpinshable的beginner部分

个人总结 钓鱼手法：1，更改address（手段：小规模更改值，利用前端特性（修改copyboard））

2，使用非法domain，但是伪装成看似官方的

应对措施 ： 严格比对地址

弹出钱包的交易确认界面时，认真确认交易内容

时刻确认是否是官方domain

善用etherscan（上面可以看到block中所有的交易）
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




## **以太坊特性**

-   不同于数字黄金，具有高度可编程性
    
-   每个人都可以在链上部署自己的智能合约（需要一定成本）
    

## **以太坊的存在形式**

-   **空间上**：存在于节点构成的分布式网络中
    
-   **时间上**：存在于按时间顺序连接的区块（block chain）中
    
-   信任机制：质押32 ETH的验证节点负责打包区块，并获得奖励
    

## **疑问记录**

### **疑问1**

打包了什么？该时间片段内全部的账单情况？所有的合约部署？

### **疑问2**

硬件层面是怎样进行的？

## **L1与L2架构**

-   **L1**：像一条主河流，承载关键信息（以太坊主网）
    
-   **L2**：像河边的基建，目的是更好地使用河流（扩容解决方案）
    
-   目前主流开发似乎都在L2上进行
    

## **Rollup扩容方案**

-   将客户端信息打包处理后再提交到L1
    
-   **ZK（零知识证明）**：一种保证信息可信性的方案
    
-   L2本身也是区块链
    

## **疑问3**

现在的开发是在ZK环节加速？如何加速？使用更好的硬件架构？在什么上跑的？未来会有ASIC吧

自我解答：问题1：打包了 交易，合约部署，合约状态变更，gas，reward

问题2：运行在节点网，一个节点一般是一个服务器|大计算机，网络类型是p2p

问题3：ZK优化，提高效率：GPU加速，FPGA开发

我个人对FPGA方向很感兴趣，以后可以试试？
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
