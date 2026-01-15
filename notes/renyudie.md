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
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
今日任务：看视频，理解概念

### 一些工具：

Etherscan+metamsk: 钱包转账，计算gas fee

ENS: Ethereum name system,通过命名的方式映射address, 这样只需要记住名字就可以代替地址了

uniswap: 连接钱包，swap token,

DAI: 美元稳定币

open sea: NFT

### 新方向：AI与web3

![image.png](attachment:a6c54be2-14b6-4e09-9cd3-e389a1448d41:image.png)

![image.png](attachment:a94d7060-8f30-4abd-8a73-208d40aa05fd:image.png)

ENGINE：API框架，

![image.png](attachment:12822e1e-cd23-4ab5-906f-e9148a0a69e6:image.png)

项目的价值主张: 实现链上AI经济，帮助想使用AI agent 的用户更方便的调用。

multiple agent pipeline写smart contract：有什么相关的DB推荐吗
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->


今日任务：**安全 & 合规**

法币与稳定币的区别：

法币是“国家信用 + 强制流通”的货币；（CNY, ERU）

稳定币是“锚定法币价值 + 市场信任”的数字资产。（USDT, USDC）

产品经理：了解概念，设计，运营

关于安全（会议笔记）：

智能合约漏洞导致资产被盗

两种攻击模式：精准猎杀，

CEX(Bybit)被攻击事件

币安中文meme

关于合规（会议笔记）：法律，咨询为主

关于就业:

交易所的合规要看，看牌照问题，看是否有大陆业务。不作恶。

Web3 项目常常通过跨境分区的方式来规避境内的监管风险，通常为在境外开展敏感业务部分，境内主要负责技术研发。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->



今日任务：对基础概念的了解

### TVL

**TVL = 在某个 DeFi 协议或智能合约中锁定的所有资产的总价值**。

-   资产可以是加密货币、稳定币，甚至 NFT 的价值。
    
-   通常用美元计价（USD），方便比较不同协议规模。
    

公式可以简单表示为：

TVL=∑(用户锁定的每种资产数量×当前市场价格)

-   **DeFi 交易所（DEX）**：TVL 代表流动性池的总资产。
    
-   **借贷协议**：TVL 代表借贷市场中锁定的总抵押资产。
    
-   **Yield Farming/流动性挖矿**：投资者会看 TVL 来衡量潜在收益和竞争程度。
    

### IDO，IEO，ICO

它们都是项目发行代币的方式，但形式和平台不同。

| 特征 | ICO | IEO | IDO |
| --- | --- | --- | --- |
| 发行平台 | 官方 / 智能合约 | 中心化交易所 | 去中心化交易所 |
| 审查门槛 | 低 | 高（交易所筛选） | 中（社区/智能合约审查） |
| 投资者风险 | 高 | 中 | 高 |
| 参与方式 | 官网或合约 | 交易所账户 | DEX 钱包交易 |
| 价格波动 | 大 | 中 | 大，容易被“抢跑” |

质押：validator质押才有更高的收益，home staking

### ZK：Zero-Knowledge Proof（零知识证明）

ZK 在以太坊生态中是做什么的？

1.  Layer 2 扩容（最重要应用）
    

ZK Rollup

-   把大量交易在链下执行
    
-   生成一个 **ZK Proof**
    
-   只把「证明 + 最终状态」提交到以太坊主网
    

优点：

-   高吞吐
    
-   低 gas
    
-   强安全性（继承以太坊安全）
    

代表项目：

-   zkSync
    
-   StarkNet
    
-   Scroll
    
-   Polygon zkEVM
    

* * *

1.  隐私保护
    

-   隐私转账（金额、地址隐藏）
    
-   身份验证但不暴露身份信息
    

代表：

-   Zcash
    
-   Tornado Cash（技术层面）
    

* * *

1.  身份 / 合规（ZK + DID）
    

你可以证明：

-   “我已满 18 岁”
    
-   “我通过 KYC”
    
-   但不暴露身份证号、姓名
    

这在 **Web3 合规方向**非常关键。

* * *

1.  ZK + AI / ZK + 数据验证（新趋势）
    

-   证明 AI 模型是按规则运行的
    
-   证明某个数据计算结果是正确的
    
-   不公开原始数据
    

### 本日的问题思考：

### 资产自托管：如何提高安全性、降低管理私钥的复杂度？

### 没有中心化机构/税收时，公共基础设施谁来维护？如果有税收，又如何分配？

个人对于税收的理解：在一个组织内部，某些治理机构（如政府）从组织成员个体处收取费用，并用作治理成本。从这个角度上来讲，去中心化网络中的gas fee就可以看作是网络中的税收，它是由个体支付，并用于去中心化节点治理的费用。

然而，去中心化节点能否进行公共基础设施的维护？此处的公共基础设施指的又是什么？

### 没有审查且隐私很强时，如何界定并治理有害信息/黑产？

有害信息，黑产的界定：个人认为仍应尊重各国各地区法律法规，然而，这会引发新的疑问：在一个国际化网络中，不同地区的法律法规是有区别的，这一点如何平衡？DAO是否应该有主体的国籍属性？我认为这涉及到立法与治理的范畴，区块链技术虽然不是一个新技术，但作为一个被扩展到如今的web3行业的技术，大部分国家地区的法律体系在这一块是空白的。如果要界定并治理有害信息，还是要依赖于法律体系的健全和治理制度的完善。

在有法可依的前提下，匿名去中心化网络的治理则有赖于组织成员/各节点的投票、staking，在必要的条件下，执法机构也可以申请成为节点，这样就让执法机构通过参与去中心化网络本身的治理逻辑（即质押）的方式有效执法了。

### 去中心化协作下，如何实现公平，可信的分配？

去中心化的协作，分配原则有赖于智能合约的部署，只要智能合约是公平，可信的，那么这个去中心化协作大概率是公平的。建议开发一些降低智能合约理解难度的工具，让不懂代码的普通人也能更好的理解智能合约。
<!-- DAILY_CHECKIN_2026-01-13_END -->

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
