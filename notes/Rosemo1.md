---
timezone: UTC+8
---

# Rosemo1

**GitHub ID:** Rosemo1

**Telegram:** rosemo

## Self-introduction

cooking at solidity

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
完成任务：  
读完了XiaoHai老师的《021学习以太坊》前三章。以下是个人总结的一些比较重要的概括性笔记。

###   
第一章：认识以太坊

1.  起源与发展：2013 年 Vitalik 提出，2015 年主网上线，2022 年完成 PoW 到 PoS 的 “合并” 升级。
    
2.  核心定位：可编程公共区块链，以 EVM 支持智能合约，是 DeFi、NFT 等生态的核心载体。
    
3.  ETH 职能：网络燃料、PoS 质押保证金、价值储存与交换媒介。
    
4.  生态特点：与比特币差异显著，生态涵盖 DeFi、NFT、DAO，通过 L2 Rollup 扩容。
    

* * *

### 第二章：网络结构与节点类型

1.  节点构成：合并后由执行客户端和共识客户端组成，通过 Engine API 协同。
    
2.  节点类型：全节点、归档节点以及轻节点。
    
3.  通信与同步：通过 UDP+Kademlia 发现节点，TCP+RLPx 建立加密连接，Gossip 协议传播数据；同步模式分全同步、快照同步、轻同步。
    
4.  搭建要点：选节点类型与环境，安装 EL+CL 客户端，配置同步模式与 RPC，可通过云服务器或本地设备部署。
    

* * *

### 第三章：账户类型与结构

1.  账户类型：EOA、合约账户。
    
2.  地址生成规则：CREATE 模式由部署者地址 + nonce 计算，CREATE2 模式可通过部署者地址 + salt+init\_code 哈希预计算地址，支持可预测部署。
    
3.  核心差异：EOA 依赖私钥，合约账户依赖代码；EOA 主动交易，合约账户被动执行。
    
4.  核心机制：两者可互相调用，ERC-20/721 代币本质是合约账户的记账系统，合约部署后代码默认不可篡改。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

Task1：创建钱包并进行测试币转账

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Rosemo1/images/2026-01-12-1768230265244-image.png)

交易id：0xefd8ba3bfa5b7cae25f9c37ef42356e6da34805f8b42906576b801339d12cd74  
  
Task2：google canlendar

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Rosemo1/images/2026-01-12-1768230599039-image.png)

  
Task3：中文周会  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Rosemo1/images/2026-01-12-1768231585322-image.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
