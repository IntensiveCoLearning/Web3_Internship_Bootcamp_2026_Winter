---
timezone: UTC+8
---

# xiaohajian-art

**GitHub ID:** xiaohajian-art

**Telegram:** Jane charlotte

## Self-introduction

我是数字媒体技术专业在读学生，设计与摄影基础，熟悉视觉表达与内容呈现。但在web3层面上我仍处于一张白纸，选择加入web3实习出于对GitHub上的观察和理解：Web3 通过代码和智能合约构建信任机制的逻辑，与 GitHub 所代表的开源在本质上高度相通——信任不来自身份，而来自可验证的规则与贡献。参与本次 Web3 实习计划，我的目标很明确： 零开始积累真实的项目经验，而不是停留在概念或文章层面的理解，并以此为起点，逐步建立知识体系提高能力，争取获得一份 web3 相关的实习机会

## Notes

<!-- Content_START -->
# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
## [**Day 3: ENS, DEX, Identity, Inventory, Sybil**](https://www.youtube.com/watch?v=wYSMNdIRoII&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3)

### [**ENS (以太坊域名服务)**](https://ens.domains/)

-   **核心作用**：将复杂的“0x...”钱包地址映射为人类可读的名字（如 sanfordstout.eth），便于记忆和转账。
    
-   **注册流程**：
    
    1.  链接钱包
        
    2.  填写域名（需无占用）
        
    3.  提交
        
    4.  等待注册完成
        
    5.  正式完成注册，这会在链上铸造一个 NFT
        
    6.  设置，钱包链接与域名绑定（域名和地址都可转账）
        

通过Zapper→搜索注册域名/地址钱包→看到该钱包记录（交易/nft/资金等）

**女巫攻击**：一个人拥有多个钱包，可以在社区投票  
**治理挑战**：这给 DAO 的“一人一票”治理带来了困难。  
目前的解决方案是通过分析链上行为（如是否参与过治理、是否有复杂的交互历史）来验证“真人程度”。

* * *

## [Web3 行业全局介绍 & 岗位总结](https://www.youtube.com/playlist?list=PLpInLW2dF2K-koNbtY5Z6pmIicb7TC603)

### **行业背景与发展历程**

-   **发展阶段：**
    
    -   **2009-2014 (极客与黄金时代):** 只有比特币，密码学和理想主义者的实验阶段，小圈子讨论
        
    -   **2015-2019 (造富与乱象并存):** 以太坊上线，智能合约出现，DeFi、交易所兴起，行业开始扩大
        
    -   **2020-至今 (行业爆发与正规化):** NFT、DeFi、GameFi 等应用层爆发，Layer2 解决技术问题
        
    -   **2024年以来:** 政治环境转变（如特朗普当选可能带来的支持），监管政策逐渐落地（如 ETF 获批），传统金融机构入场 (RWA, DePIN)，行业走向正规化
        

**2\. 行业市场与就业现状**

-   **市场规模：** 加密货币总市值约 3.1 万亿美元（约深交所一半），预计 2026 年 Web3 市场规模将达 694 亿美元
    
-   **人才需求：** 全球月活开发者仅约 2.5-3 万人，人效杠杆率极高
    
-   **结构性缺口：** 初级运营/开发者过剩，但懂合规、懂底层技术、有丰富经验的复合型人才极度短缺
    
-   **地域分布：** 北美仍是硬核技术中心；亚洲（香港、新加坡）和中东（迪拜）适合交易、资管和运营
    

**3\. 岗位概览与画像**

-   **岗位比例：** 技术岗占比最高 (~32%)，其次是项目管理/产品 (~27%)，然后是运营、市场、社区管理等
    
-   **薪资梯队：**
    
    -   **第一梯队:** 智能合约开发、合规法务、安全审计（起薪高，Senior 级别月薪可破 10 万）
        
    -   **第二梯队:** 核心研究员 (Research)、核心产研
        
    -   **第三梯队:** 运营与增长（起薪相对较低 8k-15k，但做得好奖金上限极高）
        
-   **人才偏好：** 偏好名校背景、大厂经验（但这几年逐渐转变为看重实际 Web3 项目经验）、有激情、学习能力强
    

**4\. 求职渠道与建议**

-   **渠道：** Web3 垂直招聘平台 ([SmartDeer](https://m.smartdeer.com/zh), [Web3.career](http://Web3.career) 等)、社交网络 (Twitter/X)、隐形渠道 (链上简历、GitHub 代码提交记录、DAO 治理记录)。
    
-   **进入路径：**
    
    1.  **Web2 迁移:** 带着资源和方法论（大厂经验、海外背景）直接进入
        
    2.  **Permissionless Work (无许可工作):** 参与开源社区贡献代码、写投研报告、参与 DAO 治理，积累声誉后被项目方发掘
        
    3.  **建立链上声誉:** 在社交媒体持续输出有价值内容
        
-   **面试技巧：** 展示链上交互记录和作品集比传统简历更重要；掌握新语言 (Rust, Move 等)；善用 AI 工具但要有自己的框架
    
-   **避坑指南：** 警惕“三无”团队（无代码更新、无知名风投、匿名），多查 GitHub 提交频率和团队背景
    

* * *

## [Web3运行原理](https://www.youtube.com/playlist?list=PLpInLW2dF2K-koNbtY5Z6pmIicb7TC603)

1.  **钱包、私钥与个人主权**
    

-   **私钥 (Private Key)：**一个随机生成的长字符串，类似于银行卡的密码，是控制资产的唯一凭证
    
-   **助记词 (Seed Phrase)：** 由私钥转化为一组单词（12个）  
    **私钥/助记词一旦丢失或泄露，资产将无法找回，也无法重置**
    
-   **地址 (Address)：** 公开给他人用于收款的，由私钥推导生成的公钥进一步处理得到
    
-   **个人主权：** 相比传统银行需要身份验证和审批，Web3 允许任何人无需许可地创建钱包和管理资产，真正赋予了用户对资产的完全掌控权
    

**2\. 交易与签名 (Transaction & Signature)**

-   **交易 (TX)：** 包含发送方、接收方、金额、Data（执行的操作）以及 Gas 费等信息
    
-   **数字签名：** 钱包使用私钥对交易信息进行签名  
    网络中的其他节点可以使用对应的公钥验证该签名确实由私钥持有者发出，且未被篡改，从而在不暴露私钥的情况下证明身份
    
-   **Gas 费：**激励矿工/验证者维护网络运行，操作越复杂（如智能合约交互）消耗的 Gas 越多，也有防止垃圾交易阻塞网络作用
    

**3\. 区块链网络运行机制**

-   **生命周期：** 交易提交 -> 签名 -> 广播到网络 (RPC) -> 验证者排序打包 -> 生成区块 -> 全网共识 -> 上链
    
-   **区块确认 (Finality)：** 区块链是链式结构，新的区块会指向前一个区块的哈希值  
    为了防止篡改，交易上链后通常需要等待一段时间（如以太坊约 12-13 分钟）才能被视为“最终确认” (Finalized)
    
-   **共识机制：**
    
    -   **POW (工作量证明)：** 比特币使用。算力“解题”竞争记账权，耗能高
        
    -   **POS (权益证明)：** 以太坊使用。质押代币（32 ETH）成为验证者，被随机选中记账，作恶会被没收押金 (Slashing)
        

**4\. 智能合约 (Smart Contract)**

-   **本质：** 存储在区块链上的可执行代码
    
-   Code is Law**：** 部署后不可篡改，规则公开透明，自动执行
    
-   Permissionless**：** 消除中介，极大降低了信任成本
    

**5\. 协议升级与去中心化**

-   **EIP (Ethereum Improvement Proposals)：** 以太坊通过社区提案和讨论来决定升级方向，而非由单一个人或公司决定
    
-   **客户端多样性：** 节点运行多种不同的软件客户端，避免因单一软件 Bug 导致全网瘫痪，增强了网络的抗风险能力
    

**6\. Web3 的核心特性与思考**

-   **去中心化：** 无需许可，抗审查
    
-   **透明性：** 代码开源，数据可查
    
-   **安全性挑战：** 私钥管理是最大的门槛；智能合约漏洞可能导致资金损失
    
-   **社会学意义：** Web3 不仅仅是技术，更是权力的重新分配，涉及经济学、博弈论和社会治理
    

* * *

### 用测试币在MetaMask走了遍流程

-   获取 Sepolia 测试币的水龙头：[https://sepolia-faucet.pk910.de/](https://sepolia-faucet.pk910.de/)
    
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
        
    -   [Web3 | Google Cloud](https://cloud.google.com/web3)
        

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xiaohajian-art/images/2026-01-14-1768379772259-image.png)

### [unphishable.i](http://unphishable.io)安全支付挑战

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xiaohajian-art/images/2026-01-14-1768378969358-image.png)

* * *

在 [My First NFT](https://nft.myfirst.io/) 创建自动的第一个 NFT

![nft-1768359023972.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xiaohajian-art/images/2026-01-14-1768379961197-nft-1768359023972.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

| 类型 | 比特币（bitcoin） | 以太坊(Ethereum) |
| --- | --- | --- |
| 发行 | 总量定2100万，每四年减半一次 | 无上限，但链上活动复杂，烧掉的燃料（ggs）就越多 |
| 机制 | 工作证明（pow）通过算力解决复杂难题获得新区块的权力和奖励劣势：拼算力、耗电大、成本高、效率低 | 权益证明（pos）质押（32ETH）-抽选-奖罚机制【好：奖励（增发 ETH + Gas 小费）。坏 (Slashing)：作恶者质押的 ETH 被销毁】优势：能耗降低99.95%，经济安全，效率快 |
| 编程 | 非图灵完备（Script），极度安全 | 图灵完备(Solidity)，能写复杂逻辑，但有风险 |
| 可拓展性 | 脚本能力有限，链上为主 | 原生支持智能合约与EVM |
| 经济模型 | 抗通胀 | 供应灵活，通过 EIP-1559 等机制可能呈现通缩趋势 |

* * *

**账户的两种形态（EOA vs CA）**

外部账户（EOA ）  
控制者：私钥

-   **有私秘**：你自己保管助记词。
    
-   **主动发起**：只有EOA才能发起第一笔交易（扣动扳机）。
    

合约账户（CA）  
**控制者**：**代码**（智能合约）

-   **无私钥**：它受代码逻辑控制，没有人拥有它的私钥。
    
-   **执行逻辑**：它不能自己动，必须由EOA或其他合约发送交易才会运行。
    

EVM（Ethereum Virtual Machine）是 **_以太坊的“大脑_”**，是专门用来**_运行智能合约的虚拟计算机_**。它运行在每个节点上，确保整个网络在处理代码时，**_结果都一致、可信任_**。EVM依靠代码解决了信任问题

**以太坊生态**

1.  **layer1（L1**）  
    以太坊主网拥有独立共识机制（pos）和分布式节点网络，提供最高安全性和最终结算
    
2.  **layer2（L2）**  
    建立在L1负责执行和计算，L2将交易打包压缩发回L1验证存档，主要降低Gas费
    
    **Optimistic Rollup**：仅存在争议时验证  
    **ZK Rollup**：零知识证明验证交易，无需链上争议
    
3.  **侧链（Sidechains）**：独立运行的链，通过桥接与主网交
    
4.  **应用层（Application Layer）：用户直接交互应用和界面**
    
5.  **协议层（Protocol Layer）:核心协议**：EVM、状态管理、Gas 机制
    
6.  **扩展层（scaling Layer）**:提升性能和降低成本. 如L2，侧链
    

### ETH三大核心作用

1.  **燃料费（Gas）**  
    发送ETH，转账代币，调用合约会消耗燃料，Gas=基础费（base fee)+小费（priority fee）,基础费会被销毁  
    Gas价格受网络波动影响、时间、合约复杂程度
    
2.  **权益证明（Pos）**  
    ETH 是验证者（Validator）必须质押的资产（存款为32 ETH）。它是网络安全的经济根基  
    收益：为链投票等和打包区块所得Gas  
    惩罚（Slashing）：质押全部没收，部分没收，相关性惩罚，举报奖励
    
3.  **储存价值**  
    DeFi 协议中信任度最高、流动性最好的抵押  
    NFT数字艺术品和链上资产（ERC-721非同质化，ERC-1155同质化）
    

* * *

1.  [**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)
    

### 设置MetaMask钱包

在浏览器上里官方渠道下载安装**MetaMask**插件（小狐狸钱包）

-   **创建账户**：
    
    -   **本地密码**：锁住当前设备上的应用，丢了没关系，只要有助记词就能找回。
        
    -   **助记词（种子短语/助记词）**：一个12个单词的助记词，**绝对不能泄露**、**不能截图**、**不能联网存储**。
        

助记词 → 派生出私钥（64位16进制字符串） → 私钥签名交易 → 生成公钥/地址。

一个助记词可以确定性生成无数个账户

**托管 vs 非托管钱包**

-   交易所（如Coinbase）是托管的：你不掌握密钥
    
-   MetaMask是非托管的：你自己完全掌控
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



## 一.区块链的本质与特性

定义：区块链是一种去中心化的安全系统，核心在于解决“信任”问题，需要依赖进来。

1.  **不可篡改**  
    历史记录环环相扣（哈希链接），改一处需后续后续所有区块，成本极高。
    
2.  **公开透明**  
    所有权可查账本（公链），信息在网络上透明。
    
3.  **匿名性**  
    只认钱包地址（随机生成），不暴露真实身份。
    
4.  **快速交易**  
    无论身处何地，交易记录储备上链即完成结算。
    

### **二.区块链的类别**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xiaohajian-art/images/2026-01-12-1768226332241-image.png)

### **三.**代币激励:燃料费（Gas Fee）是发起交易、转账、铸造 NFT 等所需支付的费用，即矿工的“工资”

## **四.区块链生态运转**

1.  **用户发起交易**  
    用户通过钱包应用发起转账、智能合约调用等操作
    
2.  **交易广播**  
    交易信息被广播到整个网络中的各个节点
    
3.  **节点验证**  
    网络中的矿工合法交易的节点验证性（余额是否足够、签名是否正确等）
    
4.  **配额成块**  
    通过共识机制（如工作量证明），矿工将验证过的交易配额成新的块
    
5.  **链接上链**  
    新区块被添加到区块链上，更新全网的账户本状态
    
6.  **奖励奖励**  
    成功分配区块的矿工获得代币奖励和交易手续费
    

## **五.去中心化的优势与挑战**

### 优势

1.  **信任最小化**  
    无需依赖中心化第三方，交易和数据由共识算法和加密证明保障
    
2.  **抗审查与高弹性**  
    数据分布在多个节点，单点故障或审查攻击难以完全阻塞网络，提升了系统的安全性和可用性。
    
3.  **用户自主管理**  
    用户通过私钥掌控资产与数据，平台无法随意更改或冻结账户，赋予个人更高的隐私权限和聊天权限。
    
4.  **开放创新生态**  
    区块链与智能合约构建了去中心化应用（Dapps）平台，任何开发者都可以在此基础上创新并获得代币激励，促进了大众化的技术和商业模式。
    

### 挑战

1.  **可扩展性瓶颈：**  
    公链在节点众多时共识效率低下，吞吐量和延迟问题突出，目前各大项目正通过分片、Layer 2 方案等技术进行优化。
    
2.  **安全与治理难题**  
    区块链的不可篡改特性虽能保证数据安全，但代码漏洞或治理失衡（如 DAO 中投票权集中）也可能导致严重损失。
    
3.  **用户体验与成本**  
    完全去中心化的系统往往对普通用户不够友好，如私钥管理复杂、交易手续费浮动大，需要在易用性与去中心化程度之间权衡。
    
4.  **法律与合规风险**  
    去中心化和匿名特性与现行法规存在冲突，跨境监管和合规要求尚未完善，给项目方和用户带来不确定性。
    

* * *

[**Day 1: A Developer's Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro

-   **第1周 成为超级用户**  
    学习使用MetaMask钱包、理解Gas（燃料费）、交易、DApps（去中心化应用）、ENS、DEX（去中心化交易所）、以及各类NFT标准（ERC20、ERC721）。重点是先学会“使用”这个网络。
    
-   **第 2 周 学习脚本**  
    深入的技术细节，学习如何编写脚本与智能合约、使用 Providers 和 Signers、Ethers.js、Hardhat 测试框架等。
    
-   **第3周 Speedrun Ethereum**  
    实战周。[通过SpeedrunEthereum.org](http://通过SpeedrunEthereum.org)构建具体项目，如NFT、质押应用（Stake App）、代币机、骰子游戏、抵押签名钱包（Multisig）等。
    
-   **第4周 自由构建**  
    开放式构建，学员尝试开发自己的项目，导师提供支持。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
