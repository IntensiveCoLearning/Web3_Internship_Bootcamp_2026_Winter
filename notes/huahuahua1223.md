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
# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->
1、参加co-learning、休闲黑客松openday，Web3 数据及分析基础
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->

1、学习ERC标准，发推

2、参加co-learning，Web3 公共物品资金分配第二节课

3、参加筹划的【学员 Space】黑客松选题圆桌：怎么选题做得完、讲得清、好展示[https://x.com/hujny218119/status/2015433592193868257?s=20](https://x.com/hujny218119/status/2015433592193868257?s=20)
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->


1、学习SpoonOS

2、参加以太坊中文周会，co-learning，vibe-coding + 黑客松经验分享，投研基础框架建立
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->



# Uniswap The Compact：用“可信承诺”解决跨链非原子性

## 1) 它要解决的问题：跨链天然“不原子”

跨链操作是**异步环境**：A 链执行成功 ≠ B 链一定同步成功，所以很难做到像单链那样“一步到位、全成全败”的 **atomic transaction**。  
The Compact 的定位就是：给跨链 swap / action 一个通用的**可信承诺框架**，让你可以在多链环境里“先锁钱→再完成条件→再释放/领取”。

* * *

## 2) 核心概念：Resource Lock（资源锁）

你可以把 The Compact 理解成一个 **ownerless 的锁仓合约**（基于 ERC-6909），把“可花的钱”先锁进去，变成可复用的资源锁。

每个资源锁有 4 个关键属性：

-   **Underlying token**：锁的是什么资产（ERC20 或原生币）
    
-   **Allocator**：分配器（防止重复花 / 双花）
    
-   **Scope**：作用域（单链 or 多链）
    
-   **Reset period**：强制退出周期（兜底取回资产）
    

* * *

## 3) “Compact” 是什么？

**Compact = Sponsor（赞助者）签署/注册的一份协议**：  
允许别人（claimant）在满足条件时，从资源锁里把资产“领走”。

它用 **EIP-712 typed data** 来做签名与验证，并支持多种 payload（例如单链、批量、多锁等）。

* * *

## 4) 一条典型跨链流程（理解它怎么“补原子性”）

Uniswap 官方给的典型跨链例子大概是：

1.  **Deposit → Lock**：Sponsor 把资产存入 The Compact，生成资源锁
    
2.  **Commit（Create a Compact）**：Sponsor 创建/签署 compact（写清：要换什么、条件是什么、过期时间等）
    
3.  **Fill / Execute（跨链执行动作）**：执行者在目标链完成指定动作（swap / call / message 等）
    
4.  **Claim**：满足条件后，执行者拿“证明”去领取锁里的资产
    
5.  **Fallback（Reset period）**：如果长期没完成，Sponsor 可按 reset 机制撤回资金（降低资金永久卡死风险）
    

> 关键点：它不是把跨链变成“真原子”，而是用 **“先锁定资金 + 条件满足后才能领取”** 来获得接近原子的经济结果（credible commitment）。

* * *

## 5) 模块化组件（记住 4 个角色）

官方文档把系统拆成可组合模块：

-   **Allocator**：分配资源、防双花（你可以做纯链上或混合链下版本）
    
-   **Arbiter / Tribunal**：判断“条件是否满足”（例如跨链证明是否有效）
    
-   **Court**：结算引擎（协调 claim / settlement）
    
-   **Messenger**：给智能合约钱包等场景提供兜底验证路径（容错能力）
    

* * *

## 6) 表格对比：它和常见跨链方案差在哪？

| 方案 | 核心机制 | 原子性怎么处理 | 风险点 |
| --- | --- | --- | --- |
| The Compact | 资源锁 + 条件领取（可信承诺） | 不追求“同步原子”，用锁仓确保“完成才领钱” | 条件/仲裁器设计要强；reset 参数要合理 |
| 传统桥（Lock & Mint） | 锁/铸 + 消息验证 | 多数是异步，靠桥安全性 | 桥被攻击风险高 |
| 意图（Intents）系统 | 用户表达“我想要结果”，由 solver 执行 | 依赖结算和担保机制 | solver 信用/担保设计复杂 |

> 简单理解：**Compact 更像“跨链结算层的积木”**，让 intents / solver 能做“可验证结算”。

* * *

## 7) 一句话总结

**The Compact = 一个开源的跨链“可信承诺”合约系统：**  
用 **Resource Lock（先锁钱）+ Compact（签条件）+ Arbiter/Allocator（防双花+验条件）**，在跨链异步环境里做出“近似原子”的交换与执行体验。
<!-- DAILY_CHECKIN_2026-01-25_END -->

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
