---
timezone: UTC+8
---

# yanzhuchen96-creator

**GitHub ID:** yanzhuchen96-creator

**Telegram:** @cydot0906

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
### **节点和客户端的关系？**

客户端是软件实现，节点是跑着软件（客户端）的“电脑”。有验证者在的节点就是一个“验证节点”。

执行客户端就像法院里的书记员 + 案卷系统，负责接收材料、执行具体流 程、更新“案卷状态”。  
共识客户端就像法官，根据书记员提供的案卷（执行结果），做出“这是不 是合法区块、最终选哪条链”的裁决。

**通过Engine API连接**  
验证者就像陪审团代表，按规则投票并在应该发言时站出来，拿质押的 ETH 当“保证金”。  
其实在现实里： • 执行层仍然主要通过 JSON-RPC 提供 eth\_\* 接口； • 共识层更多使用 REST / HTTP / gRPC 接口，规范定义在共识层规范中。

以太坊节点 = 一台电脑 + 两个必备客户端 + （可选）一个验证者客户端： • 执行客户端负责“算交易、管状态、对外提供 RPC”； • 共识客户端负责“跑 PoS、选区块、投票达成共识”； • 它们通过 Engine API 的“专线电话”互相协作，整个节点再通过 P2P。 网络和其他节点连接起来，共同维护以太坊这条“世界计算机”。

### 以太坊节点的“社交流程”

先找到人（UDP + Kademlia） → 再拉上加密专线（TCP + RLPx/devp2p） → 最后靠八卦（gossip）把消息传满全网（Gossip + 请求/响应）  
RLPx = 在一条 TCP 线上开很多“分机”：同一根网线同时承载区块同步、 交易广播、状态请求等多种子协议流量。 这也是为什么会说：“以太坊的 P2P 网络主要由 UDP 的 Node Discovery 协议 + TCP 上的 RLPx/devp2p 协议 组成”。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->


## 今日学习目标（周一 1.12）

1.  **读完 Web3 实习手册**[**「入门导读」**](https://web3intern.xyz/zh/blockchain-basic/)**部分，并写出自己的理解**
    

-   用 2–3 句话回答「什么是区块链、它解决了什么问题」。
    

区块链是一个由很多节点共同维护的**去中心化**账本，按时间顺序把交易打包成区块，串成链，并且记录一旦写入就很难被篡改。它主要解决的是在不完全信任对方、没有单一中心的情况下，大家还能对同一份账本达成一致。

-   列出自己觉得最重要的 3 个概念​。
    

Layer 2（对技术岗十分重要）| Gas （所有链上操作都要付费）| EVM（以太坊“核心引擎”）

**Web3 不等于 Web3.0 ！**

2.  **完成 021 学习以太坊第 1 章的第一次通读**
    

-   知道以太坊大概是干什么的、和比特币的核心区别（不需要记细节）。
    

以太坊更像是一台大家共享的“世界计算机”，能够通过执行智能合约来实现很多功能，有更完整的区块链生态；而比特币作为“数字黄金”，主要在金融相关的领域使用（例如储值和投资），在支付以外的应用生态（DeFi、NFT、游戏等）上，相比以太坊要少得多。

3.  **阅读拓展 & 辅助材料（optional)**
    

-   [Day 1: A Developer’s Guide to Building on Ethereum](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro
    
-   [Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)
    
-   [Uniswap V2 学习官方文档](https://docs.uniswap.org/contracts/v2/overview?utm_source=chatgpt.com)
    

4.  **Web3 行业全局介绍分享会重点**
    

-   竞争情况：热门岗位平均一个职位收到450份简历，初级人才过剩，精英极度短缺。70%的面试会考察AI工具使用能力和链上交互记录。
    
-   薪酬梯队：
    

第一梯队：智能合约开发和合规法务，senior月薪可破10万

第二梯队：研究员和核心产品

第三梯队：运营和增长，初级8000-1.5万，但做好增长的奖金和token分成可能超过开发

-   **如何求职：**
    

**进入Web3的三条主要路径**

1.  从Web2迁移：带着能力和资源从传统互联网转型，特别欢迎有海外工作经验、英文好、能独立交付项目的人才
    
2.  生态嵌入：在DAO里接任务、在开源社区贡献代码、写研究报告，积累贡献后项目方会主动联系
    
3.  建立链上声誉：在Twitter/Farcaster持续输出深度内容、参加Space活动，公域流量池是最好的社交简历
    

**对自己来说，短期更适合的路径可能是：1. 在开源社区贡献代码；2. 在Twitter持续输出深度内容。**

**打造链上简历很重要！！！**

**技术岗：**

1.  掌握Solidity（最基础）
    
2.  了解新语言如Rust、Move、Cairo、Funk
    
3.  具备安全意识和系统设计能力
    
4.  理解DeFi机制
    

**当前短期目标：学会Solidity和Rust**
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
