---
timezone: UTC+1
---

# renyudie

**GitHub ID:** renyudie

**Telegram:** 

## Self-introduction

数字技能与可持续社会转型方向背景，当前研究结合不同大语言模型自动生成智能合约。求职方向为 Web3 产品相关的实习或项目型岗位。

An academic background in Digital Skills for Sustainable Societal Transitions and is currently researching the automated generation of smart contracts using various LLMs. Seeking Web3 product-related internship or project-based opportunities.

## Notes

<!-- Content_START -->
# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->
## 学习过程中遇到的问题：

### 什么是水龙头（Faucet）？

Faucet（水龙头）：免费给你发测试币的服务

PoW Faucet 通过“轻量算力证明”（类似挖矿）防止滥领，而非真实挖矿

### 什么是RDF, OWL：

-   **RDF**：一种用「三元组」来描述事实和关系的数据表示标准
    
    RDF（Resource Description Framework）用一种非常简单的结构来表达世界：
    
    Subject – Predicate – Object（主语-谓语-宾语）
    
-   **OWL**：一种在 RDF 之上，用来定义“规则、约束和逻辑关系”的本体语言
    
    OWL（Web Ontology Language）可以定义：
    
    -   类（Class）：User、Asset、Protocol
        
    -   继承关系：ETH 是 CryptoAsset
        
    -   约束：
        
        -   一个地址只能属于一个用户
            
        -   Staking 是 Earn 的一种
            
    -   逻辑规则：
        
        -   如果 A 是 B，B 是 C，那么 A 是 C
            

### 以太坊生态下的所有活动的gas fee都用ETH结算吗？哪些活动会产生gas fee?

在以太坊生态下，绝大多数链上活动的 gas fee 都用 ETH 结算。但不同类型活动是否产生 gas fee 要分情况看。

**哪些活动会产生 gas fee？**

**1\. 普通 ETH 转账**

-   从一个地址转 ETH 到另一个地址
    
-   Gas fee = 基本费用（非常小）
    

**2\. 智能合约调用**

-   DeFi 协议交互：
    
    -   Swap / SwapExactTokens / AddLiquidity / RemoveLiquidity
        
    -   Borrow / Repay / Deposit / Withdraw
        
-   NFT 操作：
    
    -   Mint / Transfer / Burn / Approve
        
-   DAO 投票 / Proposal 创建
    
-   任何 ERC20/ERC721/ERC1155 合约函数调用
    

这些操作都会计算 gas，gas fee 随操作复杂度变化。

**3\. 部署智能合约**

-   部署新合约通常最贵
    
-   因为涉及：
    
    -   大量字节码存储
        
    -   初始化函数计算
        

**4\. 跨合约调用（Contract-to-Contract）**

-   一个合约调用另一个合约
    
-   复杂调用链增加 gas
    

* * *

**哪些操作不产生 gas fee？**

1.  **链下操作**
    
    -   查询余额、事件、区块信息
        
    -   Dune / Etherscan 数据查询
        
    -   这些不改变链状态，所以不消耗 gas
        
2.  **部分 Layer 2 / Rollup 内部操作**
    
    -   例如某些 rollup 内部转账可能暂时不收费
        
    -   最终结算到主网仍需支付 ETH
        
3.  **钱包内显示操作（只是界面交互）**
    
    -   比如 MetaMask 显示余额、生成签名消息，但不广播交易 → 无 gas
        

* * *

gas fee 影响因素

1.  **交易类型**
    
    -   普通 ETH 转账 < ERC20 转账 < NFT mint < 合约部署
        
2.  **网络拥堵**
    
    -   高峰期 gas price 高
        
    -   低峰期便宜
        
3.  **交易复杂度**
    
    -   调用函数越多，涉及的存储读写越多，gas 越高
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
