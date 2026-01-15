---
timezone: UTC+8
---

# cheng-zhaofeng

**GitHub ID:** cheng-zhaofeng

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
# 第一章 认识以太坊

## 一、以太坊的起源与发展

_以太坊早期的官方路线图，主要将网络开发划分为四个阶段:边境(Frontier， 也有“前锋”的意思)、家园(Homestead)、都会(Metropolis)和宁静(Serenity)。 其中，2015 年 7 月 30 日主网上线属于 Frontier 阶段，2016 年的 Homestead 升级标志着网络趋于稳定，Metropolis 又细分为 Byzantium 和 Constantinople 等多次升级;宁静(Serenity)阶段则通过信标链与 2022 年的“合并”(The Merge)等升级，实现了以太坊从工作量证明(PoW)向权益证明(PoS)的核心 过渡，之后又通过 Dencun(2024)、Pectra(2025)等升级继续演进，以接近其 更高扩展性和可持续性的长期愿景。_

以太坊分为四个阶段，逐次升级

2022年从POW升级成POS

_以太坊使用名为“以太”(Ether，缩写为 ETH)的加密货币作为其内部交 易的“燃料”(Gas)，用于支付交易费用和计算服务;在以太坊的 Rollup-为主 路线图下，ETH 既是资产，又是整个 L2 生态的结算与数据可用性“底层油料”。_

## 二、以太坊平台定位与核心特性

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NmIwZWY3MzFiZjk5YzY1MjAxM2NkN2E0M2ViZGE5OWRfRHlUcXY4ZVBMeDZ6bE1CQ2NDbHJVbG5yY2Jna2Nha21fVG9rZW46T05zMGJTc0pYb01uOGt4c2JDVGNjWjdWbk1mXzE3Njg0OTE2MzQ6MTc2ODQ5NTIzNF9WNA)

这是一张很牛逼的图，直接解释了不同的语言/层级/链之间的关系

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OTAxNWE0MTNmNGMzMjY5MmIyMmY3MmJhOWEyOTY0YTZfVVFuS25QcVAxS2VnOHpSd01UUjQ5dHNBWkcwOHI2YWpfVG9rZW46UWs2WGJsSmoyb0JpS2R4bkhoVWM3d0ZPbkpnXzE3Njg0OTE2MzQ6MTc2ODQ5NTIzNF9WNA)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NjA1NzkxOTkzOTkxNzM5YzMyZDZjMzY5ZmNlMDhiNTBfNW9NMENBMmtoejk3bTdlNktiN3pUR2hBUUduMnJlT2FfVG9rZW46WGJBWmJoek9Xb0JJNm54UFZTSmNTWlJxbnpoXzE3Njg0OTE2MzQ6MTc2ODQ5NTIzNF9WNA)

## 三、**Ether**(**ETH**)的定义与系统职能

**ETH** 三大核心作用

1.  网络燃料(Gas):支付交易和计算费用
    
    1.    如下action需要支付gas fee
        
    2.  转账一笔 ETH;
        
    3.  执行一个智能合约;
        
    4.  铸造一个 NFT;
        
    5.  与DeFi应用交互(Swap、借贷、质押等);
        

在 PoS 时代，用户支付的 Gas 费主要流向:

-   基础费(**Base Fee**):由协议自动计算，随网络拥堵程度上下浮动;这部 分 被直接销毁(**burn**)，不进入任何人的口袋;
    

我理解这一部分本质上奖励给质押者，用来cover通胀

-   优先费 **/** 小费(**Priority Fee / Tip**):由用户设置，作为“加急费”支付 给出块验证者，用来激励他们优先打包你的交易。
    

我理解Layer2就是黑心中介，赚差价，打包10次，给每个人省钱

实际支付费用 ≈ (Base Fee + Priority Fee) × Gas Used

## 四、以太坊为何被誉“全球可编程区块链”

以太坊则引入了更强大的抽象：智能合约（Smart Contracts） + 图灵完备 虚拟机（EVM）

dApp开发的缺点：

1.  维护困难：代码不可修改，只能通过预设的升级代码模式或部署新合约 + 迁移数据/资产
    
2.  性能开销与扩展难度：优先考虑部署到L2
    
3.  网络拥堵与 Gas 成本
    
4.  用户体验门槛高
    
5.  “再中心化”倾向
    

## 七、以太坊的去中心化实现机制

### 1\. 共识与出块（技术 / 经济层）

质押超过32个ETH自动成为验证者，区块在每个slot中由随机选出的提议者产出，同时有数百个验证者在同一时间对链头进行投票（attestation）通过Gasper 共识（Casper-FFG 提供终局性 + LMDGHOST 提供链头选择）完成安全性与活性

如果验证者作恶（例如双签、提交无效区块），会被削减（slashing），直接损 失部分质押；若大规模长期离线，还会触发 inactivity leak，使这些离线验证者 的有效质押被慢慢稀释，确保链最终仍能恢复终局性。

Pectra 升级之后要特别说明的一点是： EIP-7251 并不是把「最低质押门槛」从 32 ETH 提高到 2048 ETH，而是把 每个验证者的有效余额上限从 32 ETH 提升到了 2048 ETH——也就是说，你依 然可以用 32 ETH 开一个验证者，只是大户可以选择把更多质押合并到同一个 验证者里赚取利息，不必拆成很多个 32 ETH 的小验证者。

### 2\. 节点与网络（技术层）

任何人都可以在家里、办公室或云端运行全节点或轻节点。以太坊执行层使 用 DevP2P 协议维护点对点网络：

• 发现层（Discovery Stack） 基于 UDP，用来发现和加入新节点；

• DevP2P 通信层 基于 TCP，负责在已连接节点间传递区块、交易等实 际数据

共识层方面，引入了面向轻客户端的 同步委员会（Sync Committee）：每 ~27 小时会随机挑选 512 个验证者组成一组，他们连续为区块头签名，让轻客户端只通过验证少量签名就能跟随链头，而不需要下载和执行完整区块，从而让手机、 浏览器插件甚至 IoT 设备都能以低成本参与。

### 3\. 客户端多样性（实现层）

以太坊的一个很有意思的文化是 “多客户端（multi-client）”：

• 执行层客户端（EL）：Geth、Nethermind、Besu、Erigon、Reth 等；

• 共识层客户端（CL）：Lighthouse、Prysm、Teku、Nimbus、Lodestar 等。

### 4\. MEV 与 PBS（市场结构层）

### 5\. 「Rollup-中心」路线与数据去中心化（扩容 / 职责分离）

### 6\. 治理（社会层）

### 7\. 多个机制叠加出来的「去中心化」

### 8\. 做个补充：什么是 P2P 网络？
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

学习了安全分享会

# 1\. 安全事故规模

去年有1200起安全事故，资产规模达到35亿美元

每一起安全事故平均损失300万美元（只要成功发动一起攻击，就能平均获得2000万RMB的奖励？这个诱惑真的有点大，感觉不如去做黑客了。小声）

做单一攻击目标的好处：不容易被发现

粉尘攻击：投毒，模仿历史转账地址，大金额不多

# 2\. 安全事故形式

主要安全事故有如下形式

1.  私钥泄露
    
2.  智能合约泄露
    
3.  钓鱼攻击
    

每一种都有很多技术手段

黑客用了越来越多的高级渗透形式

早已经渗透到了核心管理人员的硬件设备之中，像定时炸弹

还有像deep fake这样的手段，伪造成你认识的人
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


学习了web3的运行原理

# 1.钱包，私钥，和个人主权

推动关系是

一对多派生（1 -> N） 一对一绑定(1：1) 一对一，计算后截取尾段

1.助记词-------------------------------> 2.私钥 ---------------------------------> 3.公钥 -----------------------------------> 4.钱包地址

前面能推出后面，后面推不出前面

# 2\. 交易与签名

交易 = 你要做的事 + 手续费 + 防重放序号（Nounce）

防重放序号用于唯一确认一笔交易
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



学习了以太坊的发展，层次划分，共识机制

岗位全景图

学习了Layer2的成因：本质是一场拼好饭模式，为用户省gas fee，但是等待拼车需要时间，分类：opt，zk

技术岗分类：

1.  基础设施开发：最硬核
    
2.  智能合约开发
    
3.  前端，全栈
    
4.  Zk工程师，算法
    
5.  安全审计
    
6.  协议开发
    
7.  交易所和项目
    
8.  白帽子
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
