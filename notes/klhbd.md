---
timezone: UTC+8
---

# klhbd

**GitHub ID:** klhbd

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
### 1\. 基础应用类：ENS & DEX

这两者是 Web3 世界中最常用的工具。

-   **ENS (Ethereum Name Service)** [**https://ens.domains/**](https://ens.domains/)
    
    -   **中文：** 以太坊域名服务。
        
    -   **作用：** 它把又长又难记的钱包地址（如 `0x123...456`）转换成人类可读的名称（如 `yourname.eth`）。
        
    -   **类比：** 就像互联网的 **DNS**（它把 IP 地址转成类似 `google.com` 的网址）。有了 ENS，转账时直接输入对方的 `.eth` 域名即可，不再担心输错地址。
        

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-15-1768468293211-image.png)

-   **DEX (Decentralized Exchange)**
    
    -   **中文：** 去中心化交易所（例如 Uniswap, PancakeSwap）。
        
    -   **作用：** 允许用户在不经过中介（如币安或银行）的情况下，直接通过智能合约进行数字资产兑换。
        
    -   **核心：** 资产始终在你自己的钱包里（非托管），通过算法（AMM）实现自动交易。
        

### 2\. 身份与资产类：Identity & Inventory

在去中心化网络中，如何定义“你”和“你的东西”？

-   **Identity (去中心化身份 / DID)**
    
    -   **核心：** 在 Web3 中，身份不再是由社交巨头（如腾讯、Google）掌握的账号，而是由你自己控制的加密私钥。
        
    -   **意义：** 你的链上行为、信用记录、持有的 NFT 共同构成了你的数字画像。它具有**主权性**（没人能封掉你的身份）和**互通性**（一个身份登录所有应用）。
        
-   **Inventory (资产清单 / 库存)**
    
    -   **核心：** 指你在特定应用（尤其是链游或元宇宙）中拥有的数字资产集合。
        
    -   **应用：** 例如在《Axie Infinity》中，你的 Inventory 包含你拥有的所有宠物、土地和道具。这些资产以 **NFT** 形式存在，即便游戏关服，这些资产依然记录在区块链上。
        

### 3\. 安全攻击类：Sybil (女巫攻击)

这是一个关于系统安全和公正性的重要概念。

-   **Sybil Attack (女巫攻击)→开小号**
    
    -   **定义：** 一个人或组织创建**大量伪造身份**（多个钱包地址），伪装成许多个不同的人，以此来操纵系统或获得不正当利益。
        
    -   **常见场景：**
        
        -   **空投猎人：** 一个人用几千个钱包去交互，试图领走大部分代币奖励。
            
            -   **撸空投（Airdrop Farming）：** 项目方本想给 10,000 个早期真实用户发奖励，结果被 1 个“猎人”开了 5,000 个小号领走了大半。这破坏了代币分发的公平性。
                
        -   **治理投票：** 一个人通过多个账号投票，制造“民意所向”的假象。
            
            -   **操纵治理投票（Governance Manipulation）：** 在 DAO（去中心化自治组织）中，如果是按“人头”投票，一个人控制大量账号就能左右投票结果。
                
    -   **防范：** 这就是为什么很多项目现在要求进行 **Gitcoin Passport** 验证或提供生物特征证明，目的是为了实现 **Sybil Resistance（抗女巫攻击）**。
        

### 项目方如何防御Sybil？（抗女巫攻击）

为了确保奖励发给“真人”而非“脚本”，项目方通常会使用以下手段：

-   **证明你是人（Proof of Personhood）**： 使用 Worldcoin（扫虹膜）、Gitcoin Passport（绑定 GitHub/Twitter 等社交账号）或 BrightID 进行真人验证。
    
-   **链上行为分析**： 如果 1000 个地址的资金都来源于同一个钱包，或者几乎在同一秒操作，会被判定为“关联账户”而取消资格。
    

增加成本：

-   **PoW (工作量证明)**： 创建新区块需要消耗电力成本，开小号没用，得有算力。
    
-   **PoS (权益证明)**： 必须质押代币（钱），开一万个空账号没有投票权。
    
-   **资产门槛（Token Gating）**： 只有持有特定 NFT 或一定数量代币的人才能参与，提高“开号”的经济门槛。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

### Web3合规性要求与常见法律风险

CN基调是 **“全面封堵金融属性，有限容忍技术创新”**

**代币发行与交易行为的法律风险—禁止ICO（Initial Coin Offering，首次代币发行）**

-   风险点：在中国，**所有面向公众的代币发行融资活动均被认定为非法金融活动**
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-14-1768403222409-image.png)

**赌博、传销、洗钱等刑事风险**

-   Web3 项目的设计与经济激励模型直接影响风险等级
    

**场外交易中的洗钱与非法经营风险**

-   **虚拟币作为支付工具进行场外交易**
    
    -   跨境资金流转链条（rmb→usdt、eth等）→可能涉及非法经营外汇业务
        
    -   虚拟货币兑换可能涉及非法资金
        
-   **民商事争议**
    
    -   ·涉及虚拟货币的买卖或委托投资合同，在国内很可能被认定为无效，产生纠纷时难以获得法律保护。
        

### **全球主要监管趋势**

全球正在建立更清晰的监管框架，主要趋势包括：

1.  **反洗钱（AML）核心规则**：金融行动特别工作组（FATF）推行的 **“旅行规则”** ，要求虚拟资产服务商在转账时收集和传递交易双方信息。
    
2.  **稳定币成为监管重点**：因规模巨大，为防止系统性风险（如Terra UST崩盘），欧盟、美国、香港等地都在出台严格规则，要求稳定币必须有足额资产储备。
    
3.  **主要地区监管框架**：
    
    -   **美国**：由SEC（将多数代币视为证券）和CFTC（将比特币、以太坊视为商品）等多部门共同监管。
        
    -   **欧盟**：已实施全面的 **《加密资产市场法规》（MiCA）** ，对服务商发牌并对稳定币有严格要求。
        
    -   **香港**：已推行虚拟资产服务提供商（VASP）牌照制度，并对零售投资者开放交易。
        

**Web3从业者入职与工作风险防范**

-   **关注实际运营模式和经济效果**
    
-   **以最严格的相关法规为基准，关注监管动态与司法判例，注意区域合规**
    

为自己的安全负责：不点！不签！不装！不转！

1.  用网页和软件通过在官方渠道使用和下载（关闭不常用插件）
    
2.  不随意点不了解的链接
    
3.  不泄露私钥、助记词等信息
    
4.  只用苹果原生键盘，关闭软件读写剪贴板、录音权限
    
5.  准备多个小额钱包，用于交互，如果不确定钱包是否有问题，换新钱包。
    
6.  转账前务必核对**全部**地址（前 6 位和后 4 位已经过时了，有黑客做到了仅中间几位不同，防止剪贴板被劫持 ）
    
7.  了解清楚每一步授权，转100个代币不要授权无上限，及时取消不必要的授权。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->



**以太坊（Ethereum）概览**

来源：[https://web3intern.xyz/zh/overview-of-ethereum/](https://web3intern.xyz/zh/overview-of-ethereum/)

**简介**：一个**开源的去中心化区块链平台**，通过其原生加密货币以太币（Ether，简称 ETH）提供去中心化的以太虚拟机（EVM）来处理点对点合约。

**原生代币**：以太币（ETH），全球市值排名第二的加密货币，仅次于比特币。自 The Merge 后，ETH 呈现轻微通缩趋势，年通胀率约为 0.2%-0.8%。

💡燃料费（Gas Fee)：区块链操作中支付给网络服务提供商的手续费

**提出人**：维塔利克·布特林（Vitalik Buterin）

**提出时间**：2013-2014

**目标**：构建“下一代加密货币与去中心化应用平台”，成为全球范围内的“世界计算机”。

**核心创新**：智能合约（Smart Contracts）→数字货币载体、构建生态系统的基础设施【去中心化应用（Dapps）、去中心化金融（DeFi）、非同质化代币（NFT）等】

💡智能合约

储存在区块链上的可执行代码

满足预设条件下自动执行操作，无需人工干预

-   比特币是区块链 1.0 的象征（专注于货币属性）
    
-   以太坊的定位是“区块链 2.0”的代表（通过智能合约和可编程性推动岗区块链技术向更广泛的应用场景拓展）
    

Ethereum VS Bitcoin

![8d7a7689dc588aae61b183775e15005e.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-13-1768289782678-8d7a7689dc588aae61b183775e15005e.png)

-   以太坊→灵活性→支持更多应用场景，DeFi（借贷、交易）、NFT（数字艺术品）、DAO（去中心化自治组织）等
    
-   比特币（Bitcoin）→作为“数字黄金”的存储价值
    

**以太坊（Ethereum）定位与演进**

**定位：** 基于区块链的去中心化“全球计算机”。

**核心逻辑：** 通过改变底层**共识机制**（节能安全）和引入**分片/L2技术**（提速降费）来实现大规模应用。

**升级路线**：升级路线图以 **“The Merge”** 为分水岭，从**工作量证明（PoW）** 彻底转向**权益证明（PoS）**，并确立了以**Layer 2扩展**为核心的未来路径。

### 三大关键发展阶段

### 1\. 以太坊 1.0：PoW 挖矿时代 （已成为历史）

-   **机制：** 类似比特币，靠算力“挖矿”，争夺打包交易的权利，成功打包区块的矿工获得新生成的ETH奖励。
    
-   **痛点：** 能耗高（全球以太坊矿工的电力消耗相当于小国家用电量）、速度慢（每秒处理30笔交易（TPS））、费用贵。
    

💡PoW（Proof of Work）：工作量证明

核心逻辑：用“努力”换取“信任”

区块链网络（如比特币）用来达成共识的一种机制，通过“比拼算力”来决定谁有权记账并获得奖励。

💡TPS（Transactions Per Second）：每秒事务处理量

衡量一个系统处理能力和速度的核心指标

💡不可能三角（Trilemma）：去中心化、安全性、可拓展性（高TPS）

以太坊的策略是通过 **Layer 2** 来提升 TPS，同时保证 **Layer 1** 的去中心化和安全。

2.  The Merge（合并）：开启 PoS 时代 （2022年完成）
    

-   **核心动作：** 以太坊主网的执行层全新的**信标链（Beacon Chain，共识层）** 合并，**彻底关闭PoW挖矿**。
    
-   **新架构**：
    
    -   **执行层**：处理交易、智能合约（原主网）
        
    -   **共识层**：管理验证者、确定区块顺序（信标链）
        

💡PoS（Proof of Stake）：权益证明

核心逻辑：用“资产”换取”信任“

![039f222e2d141b8bee0161d37456ffa6.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-13-1768289851296-039f222e2d141b8bee0161d37456ffa6.png)

### 3\. 未来升级方向：

**重点方向**

-   **分片技术演进——从执行分片到数据分片（**全面分片预计 2025-2026 年 启动，重点是 Proto-Danksharding）
    
    -   专注于**数据分片**，配合 Layer 2 实现扩容
        
    -   数据可用性分片、配合L2、EIP-4844 先行
        
    -   **预期效果：**
        
        -   **Layer 2 成本进一步降低 90%+**
            
        -   **以太坊主网专注于做“结算层”和“数据可用性层”**
            
-   **EIP-4844（Cancun 升级）——“省钱神器”（已于 2024 年 3 月 13 日上线主网）**
    
    -   解决方案：引入专门的“Blob 交易”类型，数据存储成本大幅降低
        
    -   技术细节：Blob 数据会在一定时间后自动删除，不会永久占用主网存储
        
    -   实际效果：
        
        -   L2 交易费用降低 **70% - 90%**
            
        -   Arbitrum、Optimism 等主流 L2 费用降至几美分
            

💡**EIP-4844**（ **Proto-Danksharding**）

核心创新：引入“Blob”（Binary Large Objects，二进制大对象）交易→以太坊区块的一个“外挂行李舱”，只存不算、定期自动清理

目标：让 Layer 2 (L2) 的手续费暴跌

-   **ZK-Rollup 技术——“批量验证，一步到位”**
    
    -   批量处理
        
    -   零知识证明：生成一个简洁的“正确性证明”
        
    -   主网验证
        
    -   **优势：速度快、成本低、安全性高**
        
    -   代表项目：zkSync Era、Polygon zkEVM、Scroll
        

💡**ZK-Rollup 技术：**Zero-Knowledge Rollup（零知识证明汇总）

本质：用“数学的确定性”代替了“人为的监督”

其他方向：

-   **EIP-1559 成果**：已实现基础费用机制，但 Gas 费仍受网络拥堵影响较大
    
-   **Verkle 树技术**：优化状态存储结构，减少节点同步所需的数据量
    
-   **执行环境优化**：提升 EVM 性能，支持更复杂的智能合约应用
    

💡以太坊升级命名惯例

![4b0d74fbd373027e00c6ef06bfee4a51.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-13-1768289927223-4b0d74fbd373027e00c6ef06bfee4a51.png)

以太坊生态概览

以太坊生态系统：

**Layer 1（L1）：以太坊主网、EVM（以太坊虚拟机，Ethereum Virtual Machine）、账户系统**

**Layer 2（L2）：**

-   Rollup
    
    -   Optimistic Rollup（假设交易合法）
        
    -   ZK Rollup（零知识证明汇总）
        

**侧链（Sidechains）：独立运行的链**

**以太坊生态分层架构**

![bed0fa16b136104059c9e4fc992ebec0.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-13-1768290067830-bed0fa16b136104059c9e4fc992ebec0.png)

详情：[https://web3intern.xyz/zh/overview-of-ethereum/#四、以太坊生态概览-l1、l2、sidechains-等](https://web3intern.xyz/zh/overview-of-ethereum/#四、以太坊生态概览-l1、l2、sidechains-等)

💡可参考：[https://youtu.be/35FF4PP58\_A?si=5lOSsWkuyftsC9Ca](https://youtu.be/35FF4PP58_A?si=5lOSsWkuyftsC9Ca)

### 1\. Layer 1（底层网络）：地基与规则

Layer 1 指的是区块链的**基础层**（主网）。它是一个独立运行的系统，负责处理所有的安全、共识和交易记录。

-   **核心任务：** 确保安全性和去中心化。它就像一个国家的法律和中央银行。
    
-   **例子：** **比特币 (Bitcoin)**、**以太坊 (Ethereum)**、**Solana**。
    
-   **瓶颈：** 由于每一笔交易都要经过全网所有节点的验证，所以速度慢（TPS 低），一旦人用多了，就像高峰期的独木桥，又堵又贵。
    

### 2\. Layer 2（二层网络）：扩容与效率

Layer 2 是构建在 Layer 1 **之上**的辅助协议。它的存在是为了分担主网的压力。

-   **核心任务：** 负责“干苦力”。它把成千上万笔小交易拿到链下处理，处理完后再把汇总结果发回 Layer 1 记录。
    
-   **例子：** \* 以太坊的 **Arbitrum**、**Optimism**、**Base**、**zkSync**。
    
    -   比特币的 **Lightning Network (闪电网络)**。
        
-   **优势：** 极快、极便宜。它继承了 Layer 1 的安全性，但拥有极高的 TPS。
    

**以太坊文化与价值观**

一个由 **“密码朋克精神”驱动，以“去中心化”为核心，通过“无需许可”的开放协作，共同建设“抗审查”的数字公共基础设施”** 的独特生态。 详情：[https://web3intern.xyz/zh/overview-of-ethereum/#五、以太坊文化与价值观](https://web3intern.xyz/zh/overview-of-ethereum/#五、以太坊文化与价值观)

**以太坊核心机制**

三个关键机制：**账户系统、Gas 模型 和 以太坊虚拟机（EVM）**

**账户系统**

-   由私钥控制的 **外部账户（EOA，Externally Owned Account）**
    
    -   公钥通过加密算法生成一个唯一的地址→分享接收转账
        
    -   私钥→控制账户（人类可读备份形式：**助记词（Mnemonic Phrase）**
        
-   由智能合约代码控制的 **合约账户（CA，Contract Account）**
    

**Gas模型：交易燃料费**

-   **总费用 = Gas Limit × Gas Price**
    
    -   **Gas Limit（限额）：**最多愿意“烧”多少
        
    -   **Gas Price（单价）**：每单位燃料多少钱，用 Gwei 表示（1 Gwei = 0.000000001 ETH）。高峰期加价。
        

💡在 EIP-1559 升级后，Gas Price 被拆分为两部分：

**基础费用（Base Fee）：**自动计算，直接销毁帮助ETH通缩。 **小费（Tip）**：额外加的钱，用来鼓励矿工优先处理。

-   **存在目的**：激励矿工/验证者、防止资源滥用
    

**以太坊虚拟机**（EVM，Ethereum Virtual Machine）：专用于运行智能合约的虚拟计算机，运行在每个节点上，确保整个网络处理代码时，**结果一致、可信任**。

核心特点：图灵完备、全球同步、隔离安全

**一般流程**

![06720d21826526da0b51ffd71a9bdd09.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-13-1768291115758-06720d21826526da0b51ffd71a9bdd09.png)

区块链行业赛道

DeFi（Decentralized Finance，去中心化金融）

DeFi 的目标是建立一个不依赖传统中介（银行等）的自由金融体系。

-   去中心化交易所（DEX，**Decentralized Exchange**）
    
    -   Uniswap：DEX代表
        
        -   核心创新：引入“自动做市商”（AMM）模式，通过“恒定乘积公式” 实现代币定价，随着池子中资产比例的变化，兑换价格也会自动调整。
            
        -   **恒定乘积公式：x \* y = k，x** 和 **y** 代表流动性池中的两种加密货币，比如 ETH 和 USDC；**k** 是一个常数，必须始终是50,000（忽略手续费情况下）
            
        -   交易核心：
            
        -   **流动性池 (Liquidity Pool)：** 由很多用户共同提供的资金池，**用于交易中的兑换**。它是去中心化交易的“燃料仓”。不再是“人等循环”，而是“人对池交易”。池子越大，交易时的价格波动（滑点）就越小。
            
        -   **流动性提供者 (LP)：** 相当于池子的“小股东”。通过存入等值的两种资产，换取池子的份额，并以此赚取分红（手续费）。
            
        -   DEX VS CEX（Centralized Exchange）
            
            ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-13-1768292099487-image.png)
-   Compound：去中心化借贷协议
    
    -   超额抵押借贷。用户存入资产获得利息（cToken），也可以抵押资产借出另一种代币，但如果抵押物价值下跌会触发“强制清算”。
        
-   MakerDAO（现已更名为 Sky）：稳定币系统
    
    -   通过超额抵押（如抵押 ETH）生成与美元挂钩的稳定币 **DAI (现为 USDS)**。
        

NFT：数字所有权革命

NFT 解决了数字资产“易复制、所有权模糊”的问题，本质是数字资产的唯一性和所有权。

-   **本质：** 利用区块链确权，让图片、艺术品、游戏道具真正属于用户。
    
-   **智能合约赋能：** 自动执行所有权转移，甚至可以自动给创作者发放转售版税。
    
-   **典型项目：** CryptoPunks（头像鼻祖）、OpenSea（主流交易市场，目前全球最大的 NFT 交易平台）。
    

### DAO：去中心化自治组织（组织变革）Decentralized Autonomous Organization

DAO 是一种通过代码和代币持有者投票，而非传统层级结构管理的组织形式。

-   **特点：** 社区驱动、透明化、无国界。
    
-   **案例：** Nouns DAO（艺术治理）、LXDAO（公共物品建设）、ConstitutionDAO（曾试图众筹购买美国宪法副本）。
    

### MEME：文化与投资热潮

MEME 币基于互联网文化和社区情绪，而非传统财务模型。

-   **代表：** DOGE（鼻祖）、PEPE（社区驱动）。
    
-   **提示：** 波动剧烈，新手参与需高度警惕风险。
    

### 五、 2025 年新兴趋势（重点关注）

1.  **Intent-Based（意图驱动）：** 用户只需说出“目标”（如：用最少的钱买到币），系统自动寻找最优路径。
    
2.  **账户抽象 (AA) 与智能钱包：** 像 Web2 一样用社交账号恢复钱包，支持免 Gas 费交易。
    
3.  **模块化区块链：** 将执行、共识、数据可用性等功能分离（如 Celestia），提高性能和定制性。
    
4.  **AI + Web3：** 包括去中心化算力（Render）、AI 代理执行脚本以及 AI 驱动的自动化交易。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




**区块链**

**一、定义**：去中心化的分布式账本技术，用于在网络节点之间安全、透明且不可篡改地记录事务数据。数据按照时间顺序打包成“区块”并链接城“链”。

☞**区块**：区块链的基本组成单位，包含一组交易数据，通过哈希链形成一个不可篡改的链式结构。每个区块由区块头（元数据，用于标识和验证区块链中的区块）和区块体（包含该区块内所有的交易记录，记录账户间的交易信息）组成。

☞**链**：通过密码学哈希函数将多个数据区块按时间顺序连接形成的链式数据结构。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-12-1768202129257-image.png)

**二、关键特性：**

1.  不可篡改：无法修改历史信息，一旦某个区块被修改，后续所有区块的哈希值都会发生变化。
    
2.  公开透明、匿名：交易信息公开透明、用户身份匿名。
    
3.  快速交易：交易不受金额、时间等限制，只要交易记录被打包在区块链中，交易就能自动完成。
    
4.  去中心化：区块链分布在全球，每个节点都储存一份相同的区块数据→**如何实现可信：分布式网络记账模式→区块链实现真正的不可篡改**
    

_△ 本质上是一个创造信任的技术，由大家共同维护、不可篡改、历史记录可查的分布式数据库，降低了信任成本。_

**三、核心组成部分**

-   **去中心化的网络和区块链：**
    

1.  区块链核心是记录全量信息的 “链”
    
2.  链存储于去中心化网络（无数节点组成）
    
3.  节点验证交易，获取代币奖励
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-12-1768202388661-image.png)

-   **维持网络运行的代币激励（incentives）**：
    

1.  **网络运行基础**：去中心化网络依赖全球节点（矿工）提供服务以维持运行，这一过程称为“挖矿”。
    
2.  **矿工与奖励**：矿工通过维护网络安全和处理交易获得奖励，形式为网络代币（如ETH、BTC）。类似工作获取工资。
    
3.  **燃料费（Gas Fee）**：用户使用网络进行交易、转账或铸造NFT时需支付的费用，即矿工的主要收入来源。
    
4.  **获取代币**：若没有相应代币，需通过交易所用法币或其他代币兑换，才能使用该区块链服务。
    

☞比特币（BTC）→节点代币奖励；具有货币属性（有限供应、可自由转账），成为加密货币

**定义**：一种去中心化的数字货币，允许用户在无需通过中央银行或第三方支付处理器的情况下进行点对点的电子现金交易，价格由供需决定。

**特点**：有限供应、具有货币属性、账户匿名、交易公开透明、不可篡改、完全在虚拟网络中

**风险**：难以追踪和限制，常被黑产利用；打包区块时间影响交易实时性；区块储存数据有限

**底层原理**：去中心化的点对点支付网络

**记账方式**：全球公共记账本

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-12-1768203552540-image.png)

**获取方式：**

挖矿：运行特定软件解决复杂数学问题，从而验证交易并获得比特币奖励

交易：比特币交易平台进行交易

接受支付：可作为支付方式

_△ 区块链像一个“全民共享的公共账本”，它的运行依赖一套“大家一起记账，干活有奖”的激励系统。_

![deepseek_mermaid_20260112_9c841f.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-12-1768202837121-deepseek_mermaid_20260112_9c841f.png)

**四、区块链运行模式**

⭐一条区块链的运行模式：用户发起交易→交易广播→节点验证→打包成块→链接上联→奖励发放

![ecosystem-BrsPk-pm.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-12-1768204174088-ecosystem-BrsPk-pm.jpg)

四、公链 VS 私链 VS 联盟链（根据访问权限与治理模式分类）

按去中心化程度从高到低：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-12-1768204242896-image.png)

来源：[**4\. 总结对比**](https://web3intern.xyz/zh/blockchain-basic/#_4-%E6%80%BB%E7%BB%93%E5%AF%B9%E6%AF%94)

|   | 公链（公共公园）public chain | 联盟链（多公司联合董事会）consortium chain | 私链（私人俱乐部）private blockchain |
| --- | --- | --- | --- |
| 成为节点的方法 | 无需申请，自由进出 | 需要邀请或申请权限分级（决策者、观察者） | 数据完全私有老板说了算 |
| 共同管理数据的模式 | 所有人可见去中心化决策 | 半公开数据联合决策 | 数据完全私有老板说了算 |
| 优点 | 创造了一种全新的、无需中间人的信任与协作范式 | 效率比公链高（因为成员少）隐私比公链好（数据不对外公开），但不如私链灵活（需要多方协调） | 效率极高（因为只有少数人参与）隐私极强（数据不对外公开） |
| 缺点 | 决策效率低（交易确认慢）维护成本高（比如电费、人力） | 去中心化程度有限（多中心化、存在共谋风险）治理决策复杂（治理效率、准入与退出机制复杂）安全悖论（攻击面集中、依赖成员安全）激励不足（缺乏原生代币经济模型） | 缺乏公链的透明性 |

五、web3 vs web3.0 vs web2

来源：[https://web3intern.xyz/zh/blockchain-basic/#%E5%9B%9B%E3%80%81web3-vs-web-3-0-vs-web2-%E7%9A%84%E8%8C%83%E5%BC%8F%E9%9D%A9%E5%91%BD](https://web3intern.xyz/zh/blockchain-basic/#%E5%9B%9B%E3%80%81web3-vs-web-3-0-vs-web2-%E7%9A%84%E8%8C%83%E5%BC%8F%E9%9D%A9%E5%91%BD)

**⭐web3：**

-   **区块链驱动的革命**
    
-   优势：金融、产权、隐私场景
    
-   劣势：不适合高频社交（微博等）
    
-   渐进式过渡：web2→web2.5→web3
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-12-1768205296646-image.png)

web3核心组件：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/klhbd/images/2026-01-12-1768205318128-image.png)

DApp（ Decentralized Application，去中心化应用）是基于区块链技术构建的应用程序，其核心特点是**去中心化**和**开源**。与传统应用不同，DApp的后端代码运行在去中心化的区块链网络上，而不是集中式服务器上。

六、去中心化的优势与挑战

| 优势 | 挑战 |
| --- | --- |
| 信任最小化 | 可扩展性瓶颈（公链吞吐量和延迟问题突出） |
| 抗审查与高弹性 | 安全域治理难题（代码漏洞或治理失衡可能导致严重损失） |
| 用户自主管理 | 用户体验与成本（易用性相对不高） |
| 开放创新生态 | 法律与合规风险 |

**“Web2 到 Web3：以太坊开发指南” (Web2 to Web3: A Developer's Guide to Building on Ethereum)**

来源：[**Day 1: A Developer's Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro

### 📚 课程结构概览

整个课程设计为期四周，循序渐进地引导开发者：

-   **第一周：成为超级用户 (Power User)** 学习使用 MetaMask 钱包、理解 Gas 费、交易、ENS、去中心化交易所 (DEX) 交易、NFT（ERC-20 和 ERC-721 标准）等基础操作 \[[00:21](http://www.youtube.com/watch?v=zuJ-elbo88E&t=21)\]。
    
-   **第二周：脚本与工具** 学习如何编写与智能合约交互的脚本、理解 Provider 和 Signer、学习 Ethers.js 以及使用 Hardhat 进行合约测试 \[[00:42](http://www.youtube.com/watch?v=zuJ-elbo88E&t=42)\]。
    
-   **第三周：以太坊速通 (Speedrun Ethereum)** 通过实战构建项目，包括 NFT 项目、质押应用 (Staking App)、代币贩卖机 (Token Vendor) 和骰子游戏等，并深入了解多签钱包 (Multisig) 和 SVG NFT \[[01:03](http://www.youtube.com/watch?v=zuJ-elbo88E&t=63)\]。
    
-   **第四周：自由创作与导师指导** 开发者可以根据自己的想法构建项目，并获得导师的指导 \[[01:14](http://www.youtube.com/watch?v=zuJ-elbo88E&t=74)\]。
    

### 🚀 职业发展路径

完成课程后，开发者有三个层次的成功目标：

1.  **Level 1：** 在 Web3 领域找到工作 \[[18:44](http://www.youtube.com/watch?v=zuJ-elbo88E&t=1124)\]。
    
2.  **Level 2：** 创立自己的项目并寻找用户与收入 \[[19:05](http://www.youtube.com/watch?v=zuJ-elbo88E&t=1145)\]。
    
3.  **Level 3：** 成为智能合约审计师，专注于代码安全和漏洞分析 \[[19:22](http://www.youtube.com/watch?v=zuJ-elbo88E&t=1162)\]。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
