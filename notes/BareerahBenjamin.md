---
timezone: UTC+8
---

# BareerahBenjamin

**GitHub ID:** BareerahBenjamin

**Telegram:** @bareerahmoooo

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
先打个卡，下午学了一点Rust和Solidity，一会整理一下Dapp部分的笔记
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

今天早上完成了一些通用任务（转账、钓鱼攻防、创建NFT）

1\. 区块链关键词：去中心化，分布式账本，**安全透明**，**匿名**（别人不知道钱包和谁关联）且**不可篡改**，区块按时间顺序链接，每个区块包含完整且可追溯的数据，**快速交易**。

2\. 区块：交易记录、区块摘要（哈希值），有限存储容量，在一定时间内打包

3\. 分布式网络实现了区块链的**去中心化**和**不可篡改**

4\. 比特币的优点：可充当货币，不会通胀，同时包含区块链的特点；缺点：难以追踪和限制、打包区块太慢、每个区块存储数据有限。

5\. 区块链的核心组成部分：去中心化的网络和区块链、维持网络运行的代币激励

6\. 区块链按访问权限与治理模式的分类（去中心化程度由强到弱）：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/BareerahBenjamin/images/2026-01-12-1768212448075-image.png)

7\. 网络维度发展对比

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/BareerahBenjamin/images/2026-01-12-1768212466873-image.png)

8\. 去中心化的优劣

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/BareerahBenjamin/images/2026-01-12-1768212483376-image.png)

9\. 以太坊介绍重点：

\- Vitalik Buterin在2013年提出，2014年正式启动

\- 核心创新：智能合约

\- 目标：成为全球范围内的世界计算机

\- 原生代币：以太币ETH

\- 区块链上的操作都需要支付手续费——燃料费（Gas Fee）

10\. 以太坊共识机制发展：

主链PoW（工作量证明）【以太坊1.0】→ 创建信标链（权益证明PoS）→ 2022.9 The Merge【以太坊2.0】，此时有两层结构：执行层（L1）和共识层（L2）

11\. 以太坊生态：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/BareerahBenjamin/images/2026-01-12-1768212528218-image.png)

12\. 以太坊核心机制：\*\*账户系统\*\*、\*\*Gas 模型\*\* 和 **以太坊虚拟机（EVM）**

每个账户都包含四个关键字段：

\- **Nonce**：防止重复交易的计数器（EOA 记录发送次数，CA 记录创建合约次数）。

\- **余额**：账户持有的 ETH 数量（单位为 Wei）。

\- **CodeHash**：EOA 为空哈希，CA 存储合约字节码的哈希值。

\- **StorageRoot**：记录账户数据的 Merkle 树根哈希（如 NFT 归属关系）。

Gas 费用 = **Gas Limit × Gas Price**（EIP-1559升级后被拆分为Base fee和Tip）

EVM核心特点：图灵完备、全球同步、隔离安全
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
