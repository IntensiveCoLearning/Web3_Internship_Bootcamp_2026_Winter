---
timezone: UTC+8
---

# huahua

**GitHub ID:** huahuahua1223

**Telegram:** @huahuahuahua1223

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->
# ERC-7984 vs ZEC vs DASH

链上默认**全透明**（余额、金额、交易关系都可追踪），因此出现了三种典型隐私路线：  
1）**合约级机密代币**（ERC-7984）  
2）**原生 ZK 隐私链资产**（ZEC）  
3）**混币增强隐私**（DASH）

## 1) ERC-7984：机密同质化代币标准

ERC-7984 目标是做一个“像 ERC-20 一样使用，但**余额/转账金额不公开**”的代币接口：  
它仍是账户模型，但把\*\*金额与余额变成密文句柄（ciphertext handles）\*\*存储与传递，从而避免公开泄漏。  
OpenZeppelin 的 Confidential Contracts 也明确：ERC7984 的余额与转账金额以密文 handle 表示。  
（工程落地常见组合：OpenZeppelin + Zama fhEVM/FHE 协处理器路线。）

## 2) ZEC（Zcash）：ZK shielded pool（最强隐私范式之一）

Zcash 的隐私核心是 **shielded pool（屏蔽池）**：  
在池内交易可隐藏**发送方/接收方/金额**（取决于交易类型），链上只验证“交易合法”，不暴露细节。  
其现代隐私池 **Orchard** 由 ZIP-224 标准化描述。

## 3) DASH：CoinJoin（混币扰动交易图谱）

Dash 的隐私不靠 ZK，而是 **CoinJoin 混币**：把多方输入/输出组合，让外部更难判断“谁付给了谁”。  
Dash 文档直接把该功能称为 CoinJoin（历史上称 PrivateSend）。

* * *

## 4) 表格对比（快速记忆）

| 维度 | ERC-7984（机密代币标准） | ZEC / Zcash（shielded pool） | DASH（CoinJoin） |
| --- | --- | --- | --- |
| 隐私隐藏什么 | 余额 & 金额（密文 handle） | 地址/金额（池内可隐藏） | 输入输出关联关系（混币） |
| 技术路线 | 合约 + 机密计算（常见：OZ + fhEVM/FHE） | zk-SNARK 系（Orchard） | CoinJoin（非托管混币） |
| DeFi 可组合性 | 目标就是“更像 ERC20 的隐私版”，更好集成 | 独立链资产，组合性依赖跨链/桥 | 偏“钱包侧隐私增强”，非 DeFi 标准 |
| 隐私强度依赖 | 取决于机密计算/实现与授权模型 | 依赖 shielded pool 使用规模（匿名集） | 依赖参与人数/轮次/金额分布 |

* * *

## 5) 一句话总结

-   **ERC-7984**：把“隐私”做成 **EVM 里的代币标准**（余额/金额不公开）。
    
-   **ZEC**：把“隐私”做到 **链级协议层**（shielded pool + ZK）。
    
-   **DASH**：把“隐私”做到 **交易图谱扰动**（CoinJoin 混币）。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

1、参加Speedrun Basic workshop、Co-Learning、第二周例会

2、准备例会分享内容
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


1、参加Co-Learning

2、参加Dapp Workshop

3、参加南塘 Dao 主题

4、学习pinata ipfs存储NFT图片元数据
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



1、学习scaffold-eth框架

2、参加co-learning

3、参加Uniswap工作原理解析

4、参加残酷共学 & 休闲黑客松案例拆解
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




1、参加线上co-learning

2、参加Web3 公共物品资金第一节课

3、参加Solidity Walk Through分享会

4、学习hardhat v3框架
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->





1、参加以太坊中文周会

2、参加线上Co-Learning

3、参加社区运营基础 & 活动策划与执行流程分享会
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->






1、观看并总结今晚的Key Hash Based Tokens: 从 ERC-721 到 ERC-7962分享会

2、学习foundry框架
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->







1、研究了智能合约中多重继承引发的钻石问题，使用C3线性化算法如何解决函数调用顺序的冲突，通过明确指定和避免使用super来避免问题

2、研究了使用GPU碰撞合约hash特定字符的地址，[manuelinfosec/eth-vanity-cuda: CUDA-accelerated Ethereum vanity address generator written in C++, capable of brute-forcing custom prefixes and suffixes for addresses using NVIDIA GPUs.](https://github.com/manuelinfosec/eth-vanity-cuda)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->









1、准备今晚分享会内容

2、参加线上co-learning

3、参加第一周例会
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->










1、学习MPC钱包，发一篇推文

2、参加 AI 及其基础概念分享会
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->











1.  学习本地部署remixd
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->












1.  进阶hardhat v2框架 [DAPP开发框架之hardhat进阶使用-CSDN博客](https://hua1223.blog.csdn.net/article/details/136885624)
    
2.  给web3实习手册提交了一个pr，在智能合约开发章节添加了一个完整的现代化 Dapp 示例项目，展示如何使用 Viem、Wagmi 和 RainbowKit 构建链上留言板应用。[https://github.com/ethpanda-org/Web3-Internship-Handbook/pull/30](https://github.com/ethpanda-org/Web3-Internship-Handbook/pull/30)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->














1.  参加了开营仪式，并观看回放
    
2.  参加了晚上的分享会，了解了web3行业全局介绍
    
3.  入门hardhat v2合约框架
    

-   编译合约`npx hardhat compile`
    
-   测试合约`npx hardhat test`
    
-   部署合约`npx hardhat ignition deploy ./ignition/modules/Lock.js`
    
-   连接钱包到hardhat network
    

启动hardhat所带的链，它将 公开一个 JSON-RPC 接口。

`npx hardhat node`

如果要针对hardhat network部署合约的话，还要在后面加上--network [localhost](http://localhost)

`npx hardhat ignition deploy ./ignition/modules/Lock.js --network localhost`

在MetaMask里添加该接口所对应的网络
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
