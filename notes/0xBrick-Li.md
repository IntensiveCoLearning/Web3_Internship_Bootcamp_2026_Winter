---
timezone: UTC+8
---

# Tianyu Li

**GitHub ID:** 0xBrick-Li

**Telegram:** @Brick0v0

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
# To do list

-   推特持续关注行业名单 ✅
    
-   021以太坊学习第四章
    
-   阅读uniswap
    
-   阅读Web3实习手册 ✅
    
-   今天你Commit了吗？✅
    
-   制定职业规划，完成职业规划任务 ✅
    
-   Unphishable完成至少5章 ✅
    

# 职业规划与学习计划

**目标求职意向**

Web3后端远程实习  Go + Solidity + 以太坊 L2

**4周后达成目标：**

-   Go工程能力
    
-   Web3 实战经验
    
-   可展示项目
    

**Week 1  Go 工程基础**

-   gin 框架
    
-   REST API
    
-   Go完成简单的后端项目
    

**Week 2  Solidity + 链交互**

-   solidity中级
    
-   学会后端与链交互
    

**Week 3：Go Indexer**

-   go-ethereum
    
-   订阅区块 / event
    
-   ABI 解码
    

**Week 4：面试准备**

-   README 优化
    
-   简历打磨
    
-   面试准备
    

Web3知识量大，迭代快，多了嚼不烂，专注一个方向更容易成功。

学一门好，多了嚼不烂。 -----Jintol老师
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->



# To do list

-   推特发学习总结 ✅
    
-   021以太坊阅读 ✅
    
-   实习手册阅读 ✅
    
-   Unphishable至少5章
    
-   安装欺诈检测插件
    
-   今天你Commit了吗：Arbireum Rust Task4 ✅
    
-   Go 工程项
    
-   参会 ✅
    

# Note

### CREATE 合约地址不可预测性

1.  CREATE 地址计算公式  
    公式：address = keccak256(rlp(\[sender, nonce\]))\[12:\]  
    sender：部署合约的账户地址（EOA 或合约）  
    nonce：部署时账户当前的 nonce  
    rlp(…)：以太坊递归长度前缀编码  
    keccak256：哈希结果256 bits(32 bytes)  
    \[12:\]：取后 20 bytes 作为地址
    
2.  nonce 定义  
    链上 nonce：账户已执行交易数量，所有节点最终一致  
    pending nonce：节点根据链上 nonce + 本地 mempool 中的未确认交易数推算  
    每个节点不同，不参与共识，只是本地估算值
    
3.  内存池（Mempool）  
    每个节点本地缓存尚未打包进区块的交易  
    每个节点独立维护，节点间不一致  
    生命周期：发送交易 → 节点收到 → 加入 mempool → 广播 → 矿工打包 → 上链 → 从 mempool 删除
    

### CREATE 地址不可预测的核心原因

查询的 pending nonce ≠ 出块节点实际使用的 nonce

-   节点视图不一致：不同节点 mempool 中交易数量不同，不同节点看到的 pending nonce 不一致
    
-   交易替换：钱包自动加价重发交易
    
-   交易乱序传播：节点先收到后续 nonce 交易
    
-   区块重组（Reorg）：区块回滚导致 nonce 回退
    

### CREATE2 的可预测性

-   地址公式：keccak256(0xff ++ deployer ++ salt ++ keccak256(init\_code))
    
-   只依赖 deployer、salt、init\_code，不依赖 nonce 或 mempool
    
-   因此在任何节点、时间、环境下可稳定预测
    

### 总结

-   CREATE 不可预测性是分布式系统一致性问题，不是密码学问题
    
-   核心链路：nonce ∈ 分布式局部状态 → 局部视图不一致 → 工程不可预测
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->






# To do list

-   学习alloy ✅
    
-   以太坊回放 ✅
    
-   Web3实习手册提交PR ❌
    
-   Unphishable继续完成 ✅
    
-   学习Web3实习手册 ✅
    
-   021学习以太坊 ✅
    
-   推特继续关注账号 ✅
    

# Note

## EVM

由分布在全世界的“以太坊**全节点**（Full Nodes）”来执行，每一个全节点都会运行一个以太坊客户端软件。

-   **内置引擎**：客户端软件内部都集成了 EVM 的实现代码。
    
-   **人人参与，人人计算**：为了验证交易是否合法，全网每一个正在工作的全节点都会在自己的 CPU 上，通过自己本地的 EVM 副本，把代码跑一遍。
    

**确定性（Deterministic）**： EVM **最核心**的特质。同样的初始状态 + 同样的输入指令，在全世界任何一台电脑的 EVM 上运行，结果百分之百相同。

### EVM如何实现全网同步？（核心难点）

**关键机制：通过“共识机制”与“状态根（State Root）”同步。**

1.  **打包与广播**： 某个验证者（Validator）会将一批交易打包成一个区块。在打包过程中，它会在本地 EVM 里运行这些交易，计算出执行后的“新世界状态”。
    
2.  **生成“摘要”**： 这个验证者并不会把整个巨大的数据库发给别人，而是将所有账户的新余额、新合约数据通过一种叫 **Merkle Patricia Tree** 的算法压缩成一个很短的哈希值，叫做 **State Root（状态根）**，放在区块头里广播出去。
    
3.  **全网验证（重放交易）**： 其他节点收到这个区块后，不会盲目相信。它们会把区块里的交易拿出来，在**自己本地**的 EVM 里按顺序再跑一遍（重放）。
    
4.  **比对结果**： 如果其他节点跑完后，算出的本地“状态根”与区块头里的“状态根”**完全一致**，说明这笔账没问题，大家就同步更新本地的数据库；如果不一致，说明发送者造假，该区块会被全网拒绝。
    

### 区块链扩展性难题（The Scalability Trilemma）

既然每一台节点都要重新运行一遍交易来验证，这意味着以太坊的计算速度取决于**其中单台最慢节点的计算能力**

解决方案：执行与结算的分离（Modular Blockchain），在 L2 执行，在 L1 结算。

-   Optimistic Rollups（乐观汇总，如 Optimism, Arbitrum）  
    默认交易都正 确，只有在有人质疑时才通过“欺诈证明（fraud proof）”回滚
    
-   ZK-Rollups（零知识证明汇总，如 ZKsync, Starknet）
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->








# To do list

## Tasks

-   创建钱包，并尝试测试币转账 ✅
    
-   完成 [Unphishable](https://unphishable.io/) 钓鱼攻防挑战 ✅ 已完成初级部分
    
-   在 [My First NFT](https://nft.myfirst.io/) mint 第一个 NFT ✅
    
-   线上 Co-Learning ✅
    
-   阅读Web3实习手册入门导读部分+总结笔记 ✅
    
-   以太坊中文周会 ❌
    

# Note

Web3实习手册-入门导读  

**Web2 vs Web 3.0 vs Web3**

-   **Web2**（当前主流互联网）：平台拥有数据，用户贡献内容
    
-   **Web 3.0**（语义网）：让机器更好“理解”数据（语义层升级） 是传统互联网的数据组织升级
    
-   **Web3**（去中心化互联网）：用户真正拥有数据与资产（权属层升级）
    

Web3挑战：扩展性、安全治理、用户体验、合规  

### 零知识证明

1\. 算术化 (Arithmetication) 将计算机程序或逻辑断言“翻译”成数学语言。

-   拍平 (Flattening)：将复杂的代码分解成简单的算式。
    
-   R1CS (Rank-1 Constraint System)：将算式转化成一组向量约束，程序变成互相关联的方程。
    

2\. 多项式转换 (Polynomial Mapping)

证明者不会直接发送成千上万个方程式的解，而是将这些约束编码进**多项式**中。

**原理**：如果两个多项式在某个随机点上的取值相同，那么它们在所有点上几乎肯定完全相同。这使得证明者只需要证明“我的多项式在某一点是正确的”，就能代表整个逻辑是正确的。

3\. 非交互式（Non-Interactive）

-   **原理**：证明者不再等验证者给随机数，而是利用多项式的哈希值（Hash）自己生成一个“伪随机数”作为挑战。
    
-   **结果**：证明者生成一个极其简短的“证明文件”，直接发到链上。任何人（包括以太坊智能合约）随时看到这个文件，都能独立验证其真实性，且不需要与证明者进行任何沟通。
    

## 会议：Web3行业介绍

2025年新增岗位：6.6万个，增长47%

结构性缺口严重：初级运营供过于求，懂Rust语言的工程师和合规人才稀缺  
AI影响：**70%面试**考察AI工具使用能力

链上简历比传统pdf简历更重要，HR会重点看链上的经历， **GitHub代码记录**、DAO投票记录、Mirror文章

## 目标和计划

1.  坚持每月至少有20天Commit代码，多提交PR
    
2.  了解DAO的运行模式，加入DAO
    
3.  了解如何Mirror文章
    
4.  除了Vibe Coding之外，掌握AI在其他工作场景的应用（会议记录、推文、群聊管理）
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
