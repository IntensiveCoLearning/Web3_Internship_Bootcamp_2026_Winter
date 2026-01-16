---
timezone: UTC+8
---

# yeqiu

**GitHub ID:** yeqiu260

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->
# Web3 & AI 深度技术总结 (Day 5 )

## Part 1: AI 基础 - 思维范式转换

### 1\. 机器学习 (Machine Learning) 的本质

-   **范式转换 (Paradigm Shift)**：
    
    -   **传统编程**：`Data + Rules (人工编写) = Output`。试图用代码穷举规则（如定义猫的解剖特征）。
        
    -   **机器学习**：`Data + Output (标签) = Rules (模型)`。通过统计学寻找函数 f(x)，让机器从数据中反推规律（如像素排列模式）。
        
-   **神经元模型**：
    
    -   模拟生物神经元：`输入(x) -> 加权求和(w·x) -> 激活 -> 输出(y)`。
        
    -   **权重 (w)**：代表不同输入因素的重要性（如“学霸模型”中“考试”的权重极高）。
        

* * *

## Part 2: 以太坊底层架构 - EVM 与 账户模型 (Day 5)

### 1\. 比特币 vs 以太坊

| 特性 | 比特币 (Bitcoin) | 以太坊 (Ethereum) | | 本质 | 分布式计算器 | 世界计算机 (World Computer) | | 完备性 | 非图灵完备 (无循环) | 图灵完备 (支持循环/递归) | | 状态 | UTXO (未花费输出) | Account Model (账户模型) |

### 2\. Gas 机制与停机问题

-   **目的**：解决 **停机问题 (Halting Problem)**。防止死循环代码占用全网计算资源。
    
-   **机制**：每一步操作（Opcode）都有标价。Gas 耗尽即抛出异常，状态回滚。
    
-   **安全反模式 (Anti-Pattern)**：
    
    -   **DoS with Block Gas Limit**：避免在数组 (`Array`) 中使用无限增长的 `for` 循环。一旦遍历所需的 Gas 超过区块上限，合约将永久锁死。
        
    -   **解决方案**：采用 **Pull over Push** 模式（让用户自己领取，而不是群发）。
        

### 3\. EVM 存储金字塔 (成本分级)

开发者必须理解数据存在哪里的成本差异（由低到高）：

1.  **Stack (栈)**：免费但极小 (1024 depth)。用于局部变量。
    
2.  **Memory (内存)**：便宜但易失。用于临时计算。
    
3.  **Storage (存储)** 💾：**极贵且持久化**。对应区块链上的永久状态 (`SSTORE` 指令)。
    
    -   **优化法则**：**Minimize Storage Writes**。先在 Memory 中计算，最后只写入一次 Storage。
        

### 4\. 账户模型 (Account Model)

-   **EOA (外部账户)**：由**私钥**控制。是所有交易的**主动发起者**。
    
-   **CA (合约账户)**：由**代码**控制。无私钥，**被动执行**。必须被 EOA 或其他合约调用才能运行。
    

* * *

## Part 3: 分布式共识 - 信任的成本

### 1\. 核心难题

-   **拜占庭将军问题**：如何在互不信任的网络中达成一致？
    
-   **女巫攻击 (Sybil Attack)**：防止恶意节点伪造百万个身份控制网络。
    
-   **解决方案**：让“投票”付出昂贵的代价。
    

### 2\. PoW vs. PoS 深度对比

| 维度 | Proof of Work (BTC) | Proof of Stake (ETH) | | 稀缺资源 | 外部物理资源 (电力/硬件) | 内部经济资源 (质押代币) | | 门槛 | 购买矿机 | 质押 32 ETH | | 攻击成本 | 51% 算力 (极其昂贵) | 购买大量代币 (推高币价，自毁资产) | | 最终性 | 概率性 (6个区块) | 确定性 (Finality) (2/3 投票确认) | | 作恶惩罚 | 仅损失电费 (隐性) | Slashing 罚没 (显性，扣除本金) |

### 3\. Slashing 与 运维陷阱

-   **无利害关系 (Nothing-at-Stake)**：PoS 引入 Slashing 机制，解决节点在所有分叉上乱投票的问题。
    
-   **高可用陷阱 (The HA Trap)**：
    
    -   **错误操作**：为了防止掉线，在两台服务器（如阿里云+AWS）上同时配置同一个验证者私钥。
        
    -   **后果**：导致 **双重签名 (Double Signing)**。协议将其判定为恶意攻击，触发 Slashing，资金被没收。
        
    -   **原则**：在 Web3 验证节点运维中，**宁可掉线 (Liveness Failure)，绝不双签 (Safety Failure)**。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->


# Web3 & AI 学习笔记 Day 4：智能的本质

## 1\. 人工智能的分级：专才 vs 通才

我们明确了目前 AI 所处的阶段以及未来的方向。

### 弱人工智能 (ANI - Artificial Narrow Intelligence)

-   **现状**：目前所有的 AI（包括 ChatGPT, AlphaGo, Siri）都属于此类。
    
-   **特征**：**“超级偏科生”**。在特定领域（如下棋、翻译、写代码）能力超强，甚至超越人类。
    
-   **局限**：缺乏通用性。无法处理未见过的新环境（如扫地机器人上马路），没有自我意识。
    

### 强人工智能 (AGI - Artificial General Intelligence)

-   **愿景**：科幻电影中的 AI（如《终结者》的天网）。
    
-   **特征**：**“全能通才”**。具备跨领域学习能力（Transfer Learning）、常识推理、因果判断和自我意识。
    
-   **核心差距**：目前的 AI 缺乏 **“世界模型” (World Model)**，即对物理世界规律（如重力、物体体积）的直觉理解。
    

## 2\. 图灵测试与常识陷阱

我们通过 **威诺格拉德模式 (Winograd Schema)** 了解了测试 AI 智能的边界。

-   **案例**：“奖杯放不进手提箱，因为**它**太大了。” vs “……因为**它**太小了。”
    
-   **结论**：人类依靠**物理常识**（容器 vs 物体）能瞬间判断“它”指代什么；而 AI 如果没有经过特定训练，很难理解这种因果关系。这是区分“计算”与“理解”的关键。
    

## 3\. 机器学习的本质思维 (Machine Learning)

这是今天最重要的计算机科学思维转换（Paradigm Shift）。

### 传统编程 (Traditional Programming)

-   **公式**：`数据 + 规则 (人工编写) = 答案`
    
-   **思维**：**教机器“怎么做”**。
    
-   **缺陷**：就像试图用“百科全书”定义猫。规则永远写不完，无法覆盖所有边缘情况（Edge Cases）。
    

### 机器学习 (Machine Learning)

-   **公式**：`数据 + 答案 (标签) = 规则 (模型)`
    
-   **思维**：**给机器看“是什么”**。
    
-   **本质**：寻找一个函数 f(x)。程序员不再手写 f(x) 的逻辑，而是提供输入 x 和输出 y，让计算机通过统计学方法反推 f(x)。
    
-   **案例**：**猫的识别**。不是定义“三角形耳朵”，而是投喂 1 万张猫的照片，让机器自己发现像素排列的规律。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->





# Web3 学习笔记 Day 3：中国环境下的生存指南

### Web3 实习手册[「安全与合规」](https://web3intern.xyz/zh/security/)部分

### 021 学习以太坊第 3&4 章

## 1\. 法律红线与合规认知

-   **定性**：虚拟货币被定义为\*\*“特定的虚拟商品”\*\*。
    
-   **红线**：
    
    -   **个人持有**：不违法，自担风险。
        
    -   **商业运营**：在境内开设交易所、ICO 发行、提供中介撮合均属**非法金融活动**。
        
    -   **司法保护缺失**：相关投资合同往往被认定无效，因炒币产生的债务纠纷或亏损，法律通常不予保护。
        

## 2\. 资金安全核心：防冻卡 (Anti-Frozen) ❄️

在中国进行 OTC/C2C（法币与加密货币兑换）时，最大的风险不是黑客，而是收到涉嫌诈骗或网赌的\*\*“黑钱”\*\*，导致银行卡被司法冻结。

### 实战防冻三步走 🛡️

Step 1: 严格筛选商家 (KYC)

-   **避坑**：绝不贪图\*\*“最高收购价”\*\*（高价往往是洗钱者的诱饵）。
    
-   **看信誉**：认准交易所的 **“神盾” / “Pro” / “大宗”** 标识商家（缴纳了高额保证金）。
    
-   **看时长（核心）**：注册时间必须 **\> 1~2 年**。能长期存活的商家，资金来源通常经过了风控考验。
    

Step 2: 物理隔离 (专卡专用) 💳

-   **禁区**：绝对不要使用 **工资卡、房贷卡、社保卡** 或存有全家积蓄的主卡。
    
-   **策略**：专门办理一张 **二类卡** 或使用闲置卡。万一被冻结，不会导致生活瘫痪（房贷断供等）。
    

Step 3: 交易红线 (非实名必拒)

-   **原则**：交易所显示的实名信息，必须与银行转账人的姓名 **100% 一致**。
    
-   **警报**：如果商家在聊天中要求\*\*“用朋友/家人的卡代付”**，请**立即拒绝、取消订单并举报\*\*。这极大概率是黑钱。
    

## 3\. 环境安全：软件供应链陷阱

-   **风险源**：由于无法访问 Google Play，国内搜索引擎（如百度）搜索“MetaMask”或交易所名称时，前几条结果常为黑客购买的**假官网广告**。
    
-   **后果**：下载后的 App 看起来一样，但内置后门，资金充入即被盗。
    
-   **对策**：必须建立“官方验证意识”（后续课程将深入讲解如何通过 Github 或 官方推特 验证软件真伪）。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->







# Web3 学习笔记 Day 2：成为高级用户

## 1\. 核心概念：钱包的本质

-   **非存储机制**：钱包（如 MetaMask）不存储代币，代币记录在区块链（公共账本）上。
    
-   **钥匙的作用**：钱包存储的是**私钥**（Private Key），它是控制链上资产所有权的唯一凭证。
    

## 2\. 密钥分层与安全 (HD Wallet)

-   **助记词 (Seed Phrase)**：
    
    -   **地位**：树根 (Root)。它是生成所有账户的母体。
        
    -   **风险**：丢失助记词 = 丢失该钱包下**所有**账户的资产。
        
-   **私钥 (Private Key)**：
    
    -   **地位**：树枝 (Branch)。对应单个特定账户。
        
    -   **风险**：丢失私钥 = 仅丢失该特定账户的资产。
        
-   **冷存储原则**：助记词必须物理备份（纸笔/钢板），**绝不触网**（截图/云盘/微信均视为泄露）。
    

## 3\. 公钥、地址与隐私

-   **单向函数**：`私钥 ➡️ 公钥 ➡️ 地址`。此过程不可逆，由地址无法推算出私钥。
    
-   **隐私模型**：区块链是透明的。一旦地址与真实身份（或交易所KYC账户）关联，所有历史交易均可被审计。
    
-   **资产隔离**：
    
    -   不要在“空投交互钱包”与“主资产钱包”之间直接转账。
        
    -   利用中心化交易所 (CEX) 作为中转站来切断链上追踪链路。
        

## 4\. 链上实操环境

-   **测试网 (Sepolia)**：模拟主网环境，使用无价值的测试币作为 Gas。
    
-   **水龙头 (Faucet)**：获取测试网 ETH 的渠道。
    
-   **区块链浏览器 (Etherscan)**：
    
    -   遵循 “Don’t Trust, Verify” 原则。
        
    -   通过 Tx Hash 查询交易状态、区块高度和 Gas 消耗。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->










# Web3 初学者：从零到一的链上实操笔记

## 阶段零：知识储备与理论学习(长期任务）

**0\. 理论先行与文档研读**

-   **核心课程**：阅读 Web3 实习手册「入门导读」部分，重点完成 **《021 学习以太坊》第 1 & 2 章** 的研读。
    
-   **拓展 & 辅助理解材料**：
    
    -   Day 1: A Developer's Guide to Building on Ethereum - Intro（以太坊开发入门指南）
        
    -   Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs（成为高级用户：理解钱包、助记词与密钥对）
        
-   **协议进阶**：阅读 **Uniswap V2 官方文档**，建立对去中心化交易（AMM）的初步认知。
    

## 阶段一：身份创建与环境配置

### 1\. 🦊 安装 MetaMask 钱包

-   **操作**：在浏览器扩展商店下载 MetaMask。
    
-   **核心点**：生成并手动抄写 **助记词（Seed Phrase）**。
    
-   **安全警示**：助记词是找回钱包的唯一途径，绝不向任何人泄露，绝不保存在联网设备中。
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yeqiu260/images/2026-01-12-1768204422774-image.png)

* * *

## 📅 阶段二：获取资源（测试网）

### 2\. 🚰 领取测试币 (Faucet)

-   **网络切换**：将网络从“以太坊主网”切换至 **Sepolia 测试网**。
    
-   **领取过程**：访问 Sepolia 水龙头网站，粘贴地址并领取测试用的 ETH。
    
-   **目的**：测试币没有实际价值，专门用于在实验中支付 **Gas Fee（手续费）**。
    
-   水龙头领取网站：
    
    -   [Faucet Trade](https://faucet.trade/sepolia-eth-faucet)
        
    -   [Google Cloud](https://cloud.google.com/application/web3/faucet/ethereum/sepolia)
        
    -   [PK910挖矿式](https://sepolia-faucet.pk910.de/)
        
    -   [Infura](https://www.infura.io/faucet/sepolia)
        
    -   [Polygon](https://faucet.polygon.technology/)
        
    -   [Binance Smart Chain](https://www.bnbchain.org/en/testnet-faucet)
        
    -   [Chainlink](https://faucets.chain.link/)
        
    -   [Alchemy](https://www.alchemy.com/faucets/ethereum-sepolia)
        
    -   [QuickNode](https://faucet.quicknode.com/ethereum/sepolia)
        

* * *

## 阶段三：链上交互实操

### 3\. 转账与 Gas 原理理解

-   **发起交易**：向另一个地址发送一笔测试币。
    
-   **关键参数**：
    
    -   **Gas Price**：你愿意为每单位“燃料”支付的单价（单位：Gwei）。
        
    -   **Nonce**：你的地址发出的交易序号。
        
-   **实验发现**：当交易卡住时，可以通过提高 Gas Price 发送相同 **Nonce** 的交易来“加速”它。
    

* * *

## 阶段四：数据审计与区块探索

### 4\. 使用区块链浏览器 (Etherscan)

-   **查询方式**：将钱包地址粘贴到 [Sepolia Etherscan](https://sepolia.etherscan.io/)。
    
-   **数据解读**：
    
    -   **Transaction Hash (TxID)**：每笔交易唯一的身份证号。
        
    -   **Block Number**：交易被打包进的区块高度。
        
    -   **链的特性**：观察发现 Nonce 14 和 Nonce 15 之间区块号差距很大，说明全球每秒都有大量交易在进行。
        

> ![6ce02faa01d4eccbcce9a732149513ad.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yeqiu260/images/2026-01-12-1768204518011-6ce02faa01d4eccbcce9a732149513ad.png)

![95e05e1d1469ca77df8c6a2bb6e2e4f8.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yeqiu260/images/2026-01-12-1768204532437-95e05e1d1469ca77df8c6a2bb6e2e4f8.png)![f2c82a8acf379e4d3075e8ca642a4087.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yeqiu260/images/2026-01-12-1768204542243-f2c82a8acf379e4d3075e8ca642a4087.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
