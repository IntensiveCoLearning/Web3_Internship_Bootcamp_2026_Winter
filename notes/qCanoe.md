---
timezone: UTC+8
---

# CaNoe

**GitHub ID:** qCanoe

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
### **1.13** [**021 学习以太坊第 1 章**](https://github.com/XiaoHai67890/021Ethereum/blob/main/%E3%80%8A021%E5%AD%A6%E4%B9%A0%E4%BB%A5%E5%A4%AA%E5%9D%8A%E3%80%8B%E5%BC%80%E6%BA%90%E6%95%99%E6%9D%90.pdf)

**一、学习目标与章节定位**

本章旨在从宏观层面理解以太坊的**起源、平台定位、核心机制与应用边界**，为后续关于节点、账户、智能合约与 EVM 的技术性内容建立统一认知框架。其重点不在“如何开发”，而在“以太坊是什么、能做什么、为何重要”。

**二、以太坊的起源与发展脉络**

以太坊由 Vitalik Buterin 于 2013 年提出，源于其对比特币脚本能力受限的反思。比特币更偏向安全、稳健的价值转移系统，而以太坊从一开始就被设想为一个**可运行通用程序的去中心化平台**。 以太坊主网于 2015 年上线（Frontier 阶段），随后经历 Homestead、Metropolis（Byzantium / Constantinople）等升级，并在 2022 年 The Merge 中完成从 PoW 向 PoS 的根本性转型。2024 年后的 Dencun、EIP-4844（Proto-Danksharding）标志着其发展重点转向 Rollup 支撑与数据可用性扩展，体现出“长期演进而非一次性设计”的工程哲学。

**三、以太坊的平台定位与核心特性**

以太坊是一个**去中心化、开源、支持智能合约的公共区块链平台**。其核心在于以太坊虚拟机（EVM），这是一个图灵完备的执行环境，使区块链从“分布式账本”升级为“分布式状态机”。 在该架构下，所有账户余额、合约状态与执行结果都被记录在一个全网共享的状态中，并通过共识机制确保一致性。这使得以太坊不仅能够转移资产，还能承载复杂逻辑和跨合约组合。

**四、ETH（以太币）的定义与系统职能**

ETH 是以太坊的原生资产，其设计目标并非单一的“货币”，而是兼具**功能性与资产性**。 首先，ETH 是网络运行的燃料，所有交易和合约执行都必须以 ETH 支付 Gas，用以计量计算和存储资源。EIP-1559 之后，Gas 费用由 Base Fee（销毁）与 Priority Fee（奖励验证者）构成，强化了 ETH 与网络使用度之间的内在联系。 其次，在 PoS 体系中，ETH 是质押资产，验证者通过锁定 ETH 参与共识并获得奖励，同时承担被 Slashing 的风险，从而为系统安全提供经济约束。 再次，ETH 也是 DeFi、NFT 与 Web3 生态中最重要的抵押资产和结算单位，构成整个应用层经济活动的“底层货币”。

**五、智能合约与可编程性**

智能合约是部署在区块链上的确定性程序，由全网节点在 EVM 中执行。它以“代码即规则”的方式替代传统的中心化执行者，使得协议行为无需依赖信任。 由于 EVM 是图灵完备的，以太坊能够表达几乎所有可计算逻辑，这一特性使其区别于比特币等以转账为核心的区块链系统，也奠定了 DeFi、DAO、NFT 等应用得以出现的技术基础。

**六、全球可编程区块链与“世界计算机”**

“全球可编程区块链”这一称谓可拆解为三层含义： 其一，区块链提供去中心化、不可篡改与公开可验证的信任基础； 其二，可编程性来自智能合约与 EVM，使逻辑不再受限于简单脚本； 其三，全球性意味着任何人只要接入网络，就可以在同一个共享状态机上部署和调用程序。 因此，以太坊常被描述为“世界计算机”：它不属于任何单一实体，但所有人都可以在其上执行逻辑并达成共识。

**七、以太坊与比特币的异同**

比特币与以太坊在去中心化、共识驱动和数字资产属性上高度相似，但在目标定位上存在根本差异。 比特币强调稀缺性、抗通胀与长期价值储存，协议设计极度克制；以太坊则强调通用性和应用承载能力，愿意通过升级换取功能与扩展性。这种差异并非优劣之分，而是服务于不同的系统目标。

**八、dApp 的概念与应用形态**

去中心化应用（dApp）通常由**链上智能合约（后端）与链下前端界面**构成。合约作为公开、不可篡改的逻辑层，前端则负责用户交互。 dApp 的优势在于抗审查、零停机和可验证性，但代价是性能受限、合约升级困难以及较高的设计与安全要求。当前 DeFi、NFT、DAO、链游等生态，正是这种架构在不同领域的具体体现。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

## 1.12 Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分

## **一、区块链基础**

> [https://web3intern.xyz/zh/blockchain-basic/](https://web3intern.xyz/zh/blockchain-basic/)

-   去中心化、分布式账本
    
-   区块、链式结构、不可篡改
    
-   哈希函数、Merkle Tree
    
-   共识机制（PoW / PoS / DPoS）
    
-   区块确认、最终性（Finality）
    
-   公链 / 联盟链 / 私链
    
-   激励机制与节点（矿工 / 验证者）
    
-   经济安全性（Crypto-economic Security）
    
-   Web3 与 Web2 的区别
    
-   信任最小化、抗审查
    
-   可扩展性与性能瓶颈（不可能三角）
    
-   链上透明性 vs 隐私保护
    

* * *

## **二、以太坊概览**

> [https://web3intern.xyz/zh/overview-of-ethereum/](https://web3intern.xyz/zh/overview-of-ethereum/)

-   支持智能合约的公链
    
-   EVM（以太坊虚拟机）
    
-   Solidity 合约语言
    
-   ETH 与 Gas 机制
    
-   账户模型（EOA / 合约账户）
    
-   DApp 开发生态
    
-   DeFi / NFT / DAO 基础设施
    
-   PoW → PoS（The Merge）
    
-   质押（Staking）与验证者
    
-   可组合性（Composable / Money Lego）
    
-   Layer2 扩展需求
    
-   Rollup（Optimistic / ZK）
    

* * *

## **三、Web3 行业认知**

> [https://web3intern.xyz/zh/industry-knowledge/](https://web3intern.xyz/zh/industry-knowledge/)

-   技术岗 vs 非技术岗
    
-   智能合约开发
    
-   前端 / 后端 / 协议工程
    
-   产品、运营、社区
    
-   研究员、分析师、BD
    
-   DAO 与去中心化组织
    
-   Token 经济模型（Tokenomics）
    
-   基础设施赛道（L2、跨链、预言机）
    
-   钱包、节点、RPC 服务
    
-   开源与链上透明文化
    
-   快速迭代、高风险高波动
    
-   熊市 / 牛市周期影响
    

* * *

## **四、Web3 远程工作**

> [https://web3intern.xyz/zh/remote-work-guide/](https://web3intern.xyz/zh/remote-work-guide/)

-   全球化、远程优先
    
-   异步沟通为主
    
-   工具导向（Discord / GitHub / Notion）
    
-   文档驱动协作
    
-   公共讨论、透明决策
    
-   结果导向、弱层级
    
-   多时区协同
    
-   社区即入口
    
-   贡献即简历（PR / 提案 / 内容）
    
-   Token / 股权 / Grant 激励
    
-   自驱能力与自我管理
    
-   英语与跨文化沟通能力
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
