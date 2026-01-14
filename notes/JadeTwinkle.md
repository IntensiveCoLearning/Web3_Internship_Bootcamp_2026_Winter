---
timezone: UTC+8
---

# JadeTwinkle

**GitHub ID:** JadeTwinkle

**Telegram:** @JadeOWO

## Self-introduction

The eternity of pain often does not lie in choosing hardship, but in rejecting the choice itself.

## Notes

<!-- Content_START -->
# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
今日目标：完成Web3 实习手册[「安全与合规」](https://web3intern.xyz/zh/security/)部分
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

# 1\. 核心认知：

## 账户体系的本质不再是 Web2 的“账号密码”模式，而是“钥匙与锁”的主权模式

**EOA (外部拥有账户) vs. 合约账户：**

-   EOA（如 MetaMask 生成的）：由私钥控制，没有代码。它是所有链上行动的“扳机”，只有 EOA 能主动发起交易并支付 Gas
    
-   合约账户（如 Uniswap 资金池）：由代码控制，无私钥。它像自动售货机，必须被 EOA“踢一脚”（调用）才会执行逻辑，无法主动发起顶层交易
    
-   运营启示：做产品设计时，要明白用户（EOA）是起点，合约是后端逻辑
    

**助记词 (Mnemonic) 的真相**：

本质：那 12 个单词本质上是一个巨大的随机数，是我的Master Key（主钥匙）

层级关系：助记词 -> 私钥 -> 公钥 -> 地址。通过标准路径，一套助记词可以确定性地衍生出无数个子账户

安全铁律：助记词丢失 = 资产永久丢失。没有“忘记密码”，没有客服

# 2\. 实操技能：Web3 原生工具流

**钱包 (MetaMask) 的误区：**

-   密码 vs. 助记词：MetaMask 的登录密码只是为了锁住当前设备（本地加密），丢了可以重置；但助记词丢了，神仙难救
    
-   环境隔离：开发/测试用的钱包绝对不能和存大额资产的钱包混用
    

**Etherscan (区块浏览器)：**

去伪存真：钱包 UI 可能会骗人（延迟或 Bug），但 Etherscan 是“公正的第三方”

关键数据：作为运营，必须学会看 Status (是否成功)、From/To (资金流向)、Gas Price (当前成本)

**Nonce (交易计数器)：**

防重放：交易必须按顺序排队。每个账户都有一个 Nonce 值（0, 1, 2…），必须按顺序执行

运营场景：用户投诉“交易卡住了”，通常是因为前一笔交易（低 Nonce）没确认，导致后面所有交易都在排队

# 3\. 安全观：黑暗森林生存法则

**托管 (Custodial) vs. 非托管 (Non-custodial)：**

-   托管（如交易所）：私钥在平台手里，资产只是数据库里的一行数字。风险是平台跑路
    
-   非托管（如 MetaMask）：**Not your keys, not your coins**。自己掌握私钥，拥有资产的完全主权，但也承担所有安全责任
    

# 💡 我的心路历程：Day 2 复盘

我今天接触了大量新概念：从 Gas 机制的计算公式，到 RPC 的中心化风险，再到区块的 Finality 时间

坦白说，**笔记记了很多，但脑子还没完全转过来**

现在的状态更像是把拼图碎片都收集齐了，但还在琢磨怎么把它们拼成一张完整的图

这种“知识未完全内化”的感觉让我意识到，Web3 的门槛确实存在，不能只靠死记硬背，得靠实操来验证
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



今天是我通过“**实操 + 理论 + 行业视野**”的结合，我完成了从创建钱包到理解以太坊底层逻辑的闭环

感觉这些可以帮助我对未来的职业路径有一个比较清晰的认知！

## **1\. 完成了 Web3 的“Hello World”全流程，打破了对区块链的陌生感**

-   钱包创建： 使用 MetaMask 创建了第一个 EOA（外部拥有账户）钱包，深刻理解了“助记词 = 私钥 = 资产”的安全铁律。
    
-   链上交互：
    

在测试网领取了 Testnet Token（领水）

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/JadeTwinkle/images/2026-01-12-1768219559014-image.png)

完成了第一笔转账，体验了去中心化点对点支付

-   NFT 铸造： 亲手制作并 Mint 了人生第一个 NFT。不仅是上传一张图，而是理解了将文件“资产化”并获得链上唯一 ID 的过程
    

## **2\. 结合实操，通读了**[**《从零到一学习以太坊》**](https://github.com/XiaoHai67890/021Ethereum.git)**的核心章节，建立了一些基本概念**

-   以太坊 vs 比特币： 比特币是“数字黄金”（计算器），以太坊是“全球可编程计算机”（智能手机操作系统）
    
-   智能合约（Smart Contract）： 本质是“数字世界的自动售货机”。输入条件（Gas + 指令） -> 自动执行 -> 产出结果，无需中介
    
-   ETH 的三重属性：
    

燃料（Gas）： 驱动网络运行的手续费

保证金（Staking）： PoS 机制下的安全抵押品

货币： 价值流转的载体

### 关键技术点

-   账户模型：
    

EOA（外部账户）： 由私钥控制，发交易（我用的 MetaMask）

-   合约账户： 由代码控制，被动执行（我交互的 NFT 合约）
    
-   Gas 机制（EIP-1559）： 费用 = Base Fee (被销毁) + Priority Fee (给验证者的小费)。这解释了为什么链上拥堵时 Gas 会飙升
    

### 扩容方案

-   Layer 2 (L2)： 像高架桥，分流主网拥堵
    
-   坎昆升级 (Dencun)： 引入 EIP-4844 (Blob)，为 L2 提供了便宜的“外挂存储”，大幅降低了 L2 手续费
    

## 3\. 下午参加了Web3中文周会，拓展了行业视野

通过周会了解了当前（2025-2026）的市场脉搏

-   技术进展： 以太坊完成 Fusca 升级，Blob 容量提升；L2 战局激烈（OPBNB, Arbitrum 持续进化）
    
-   安全警示： 关注到 Two Bits 等项目的被盗事件，再次印证了“不随意授权、保护私钥”的重要性
    
-   市场趋势：
    

2026 展望： 被定义为“布局年”，行业从野蛮生长转向合规与整合

RWA (真实世界资产)： 传统金融资产上链是下一轮增长引擎

## 4.下一步计划

继续深入[**《从零到一学习以太坊》**](https://github.com/XiaoHai67890/021Ethereum.git)的共识与生态部分

整理一份针对 Web3 产品岗的简历 MVP

复盘和深入理解Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
