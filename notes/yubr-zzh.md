---
timezone: UTC+8
---

# yubr-zzh

**GitHub ID:** yubr-zzh

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
# 一、以太坊核心机制：从账户到执行的完整链路

以太坊不仅仅是一条区块链，更是一个全球共享的“分布式计算机”。它的核心价值在于通过代码实现无需信任的自动化规则（如智能合约），而这一切的背后依赖于三个关键机制：**账户系统、Gas 模型 和 以太坊虚拟机（EVM）**。

## **1、账户系统**

账户系统 包含由私钥控制的 **_外部账户（EOA）_** 和由智能合约代码控制的 **_合约账户（CA）_** 。

想象你第一次接触以太坊——你需要一个“数字钱包”来参与网络。这个钱包的核心是 外部账户（EOA） ，它由一对密钥（私钥和公钥）生成，就像银行账户的密码和账号。私钥 是你控制账户的“钥匙”，必须严格保密；公钥通过加密算法生成一个唯一的地址（如 0xAbc...123），你可以把它分享给朋友接收转账。

除了用户控制的 EOA，还有 合约账户（CA） 。它们不像 EOA 那样受私钥控制，而是**由代码驱动。**比如，你部署一个智能合约（如一个 NFT 市场），区块链会自动生成一个 CA 地址（如 0xDef...456）。这个账户不能主动发起交易，只能通过 EOA 触发——比如你点击“购买 NFT”按钮时，EOA 向 CA 发送交易，CA 的代码自动执行出货逻辑。

每个账户都包含四个关键字段：

• Nonce：防止重复交易的计数器（EOA 记录发送次数，CA 记录创建合约次数）。

• 余额：账户持有的 ETH 数量（单位为 Wei）。

• CodeHash：EOA 为空哈希，CA 存储合约字节码的哈希值。

• StorageRoot：记录账户数据的 Merkle 树根哈希（如 NFT 归属关系）。

## **2、Gas模型**

当用钱包使用自己的 EOA 发起一笔交易（比如转账或操作合约），需要支付“燃料费”——也就是 Gas。

Gas 费用 = **用多少 × 每单位多少钱**，就像你打车一样：

-   **Gas Limit（限额）**：你最多愿意“烧”多少燃料。 比如你觉得最多可能需要 15 万单位，就设置 150,000。
    
-   **Gas Price（单价）**：每单位燃料多少钱，用 Gwei 表示（1 Gwei = 0.000000001 ETH）。 网络越拥堵，价格越贵，就像打车高峰期加价。
    

所以，**总费用 = Gas Limit × Gas Price**。

**Gas 的存在有两个目的：**

-   **激励矿工/验证者**：你给得越多（Gas Price 越高），他们越愿意优先处理你的交易。
    
-   **防止资源滥用**：如果有人想让合约死循环，Gas 会用光，交易失败，系统不会被拖垮。
    

以前，Gas Price 全部给矿工。

**在 EIP-1559 升级后，Gas Price 被拆分了两部分：**

-   **基础费用（Base Fee）**：每个区块都会有，自动计算，直接销毁（直接消失）来帮助 ETH 通缩，毕竟物以稀为贵。
    
-   **小费（Tip）**：你额外加的钱，用来鼓励矿工优先处理你。
    

## 3、以太坊虚拟机（EVM）：代码的执行引擎

EVM（Ethereum Virtual Machine）是 以太坊的“大脑”，是**_专门用来运行智能合约的虚拟计算机。_**它运行在每个节点上，确保整个网络在处理代码时，结果都一致、可信任。

EVM 的核心特点：

• **图灵完备**：就像真正的电脑一样，EVM 可以执行各种逻辑，比如 if 判断、循环等。

• **全球同步**：每个矿工/节点都会自己执行一遍合约代码，保证结果一样。

• **隔离安全**：EVM 把合约“关”在一个小房间里运行，不允许它乱访问用户的数据和网络，保护隐私和安全。

# 二、一般流程与类比

## **1.一般流程**

1.  **用户通过 EOA 发起交易**（如调用合约与 **CA** 交互，或者转账与其他用户的 **EOA** 交互）。
    
2.  **交易附带 Gas 参数**，矿工 / 验证者选择打包。
    
3.  **EVM 执行合约代码**，修改存储（如更新 NFT 归属）。
    
4.  **Gas 费用按 Gas Limit × Gas Price 扣除**，保障资源合理使用。
    

## **2.相似类比**

-   **EOA = 个人钱包**：你用私钥“签名”来证明身份，可以主动发起任何交易
    
-   **CA = 智能保险箱**：按照预设程序自动运行，只有满足条件才执行操作（如：收到正确密码后自动转账）
    
-   **Gas = 计算资源费用**：
    
    -   Gas Limit：你愿意支付的最大计算量
        
    -   Gas Price：每单位计算的价格
        
    -   如果设置 1000 单位 ×10Gwei，实际用了 800 单位，退还 200 单位的费用
        
-   **EVM = 全球共享计算机**：所有节点同时运行相同的程序，确保结果一致且可信
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

# 一、以太坊生态概览

以太坊的生态系统由多层架构组成，包括 **_L1（主网）、L2（二层扩展解决方案）、侧链（Sidechains）_** 等，共同支持高吞吐量和低费用的交易处理。

以太坊的几个架构如图：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-14-1768397731495-image.png)

而以太坊生态又可分为如下几个层次：

**1、应用层**

是用户直接交互的应用界面：

**·**DeFi 应用：Uniswap（去中心化交易所）、Aave（借贷协议）、Compound（借贷协议）

• NFT 平台：OpenSea、Foundation、SuperRare

• 钱包应用：MetaMask、Coinbase Wallet、Rainbow

• DAO 工具：Snapshot、Aragon、Colony

**2、协议层**

以太坊的核心基础设施：

• 共识层客户端：Prysm、Lighthouse、Nimbus、Teku

• 执行层客户端：Geth、Nethermind、Erigon、Besu

• 核心协议：EVM、状态管理、Gas 机制

**3、扩展层**

提升性能和降低成本的解决方案：

• Layer 2 Rollups：Arbitrum、Optimism、Polygon zkEVM、zkSync Era

• 侧链：Polygon PoS、xDAI（Gnosis Chain）

• 状态通道：Lightning Network for Ethereum

# 二、以太坊文化与价值观

## 核心价值观主要有如下几点：

1、去中心化治理

没有单一的控制者或权威机构，验证者遍布全球

2、无需许可与开放性

任何人都可以使用、开发、部署智能合约，没有地域、身份和财富的限制

3、抗审查性

4、密码朋克精神 也就是“Code is law" :用算法和数学构建信任

5、公共物品导向：

优先考虑生态系统整体利益

支持开源项目和基础设施建设

通过各种资助计划推动创新

6、可持续发展理念

长期主义思维，注重技术的可持续演进

平衡创新速度与网络稳定性
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


# 一、文档阅读--以太坊

1.  **以太坊的介绍：**
    

以太坊（Ethereum）是一个开源的去中心化区块链平台，通过其原生加密货币以太币（Ether，简称 ETH）提供去中心化的以太虚拟机（EVM）来处理点对点合约。

以太坊的核心创新在于 智能合约（Smart Contracts） 。智能合约是存储在区块链上的可执行代码，能够在满足预设条件时自动执行操作，无需人工干预。这一特性使得以太坊不仅是数字货币的载体，更是构建去中心化应用（Dapps）、去中心化金融（DeFi）、非同质化代币（NFT）等生态系统的基础设施。

以太坊的定位是“区块链 2.0”的代表。以太坊通过**智能合约**和**可编程性**，推动区块链技术向更广泛的应用场景扩展。

**2、以太坊与Bitcoin的区别**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-13-1768308889369-image.png)

**3、以太坊的发展历程**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-13-1768309130988-image.png)

# 二、分享会--Web3运行原理

1、助记词、私钥和公钥

助记词：一个可以对多个私钥

私钥：一串字符串，具有隐私性，需要注意私钥安全

![微信图片_20260113211003.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-13-1768309943898-_____20260113211003.jpg)![微信图片_20260113211010.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-13-1768309915828-_____20260113211010.jpg)

2、交易和签名

数字签名

gas fee ：去中心化网络的激励代币机制；可类比“油费”

交易的完整周期（附上img）：从交易到出块：

![微信图片_20260113211027.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-13-1768309994011-_____20260113211027.jpg)

3、共识机制

目的：让全球节点共同认同一个账本

Pow:工作量证明 --- BTC

算力竞争写帐

Pos：权益证明 -- ETH

质押＋随机选人写帐（更节能)

4、几个概念的关系

钱包通常不连全网，而是连接一个RPC(一个提供节点的API)

5、智能合约

本质： = 区块链账本里的“可执行代码”

·在EVM(虚拟机）里运行

·交易出发执行->改变链上状态

·写进区块链 ：具备难以篡改、难追溯的特性

社会学意义：

“code is law”

·规则可验证、按代码执行

·部署后难修改

·减少中介和摩擦

6、区块链协议升级

·以太坊如何改规则(附上img）：

·节点客户端多样性： 节点软件不止一种——执行层客户端+共识层客户端（因而不怕单一软件出现bug）

7、Web3关键特性回顾

·去中心化

钱包创建（人人可生成钥匙）

交易广播

网络运行（节点越分散越稳定）

客户端越多越稳

无许可、抗审查、开放开源、隐私

**总的来说，Web3就是，用私钥签名来证明你是谁，用共识网络保证账本可信，用智能合约让规则自动执行。**

![微信图片_20260113211044.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-13-1768310022051-_____20260113211044.jpg)

8、分享会末尾的几点思考

·在网络的运行中，当它逐渐与现实世界的一些运行模式相关联时，比如一些税收、公共基础设施等，这些的话又该如何定义、或者进行资产分配？

·由于网络的抗审查，它对所有人都是抗审查的，对于一些黑产也是抗审查的，那么应该如何去避免这些问题，从而让整个网络的运行更加安全些？
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



## 一、区块链入门导读知识

### · 区块链主要由两个核心组成部分：

1、去中心化的网络和区块链

区块链将会有一条链来记录全部的信息，这条链将存在对应的去中心化网络中。 而去中心化的网络，将由无数节点提供服 务来维持网络运行。节点通过计算验证交易获得代币奖励。

![decentralize-CO_v5XlA.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-12-1768219594828-decentralize-CO_v5XlA.jpg)

2、维持网络运行的代币激励

网络节点服务提供商（以下简称为网络服务提供商）可以得到奖励。这些提供商也叫做“矿工”，他们负责一些“挖矿”工作—— 由无数节点服务来维持网络运行。由于他们对网络运行的贡献，不同的网络服务提供商可以得到不同的代币奖励，比如：比特币。而除了比特币，还有其他的代币激励机制，比如BTH、SOL、MATIC等。

### ·区块链的运行

区块链生态系统的运行包含以下几个关键步骤：

1\. 用户发起交易：用户通过钱包应用发起转账、智能合约调用等操作

2\. 交易广播：交易信息被广播到整个网络中的各个节点

3\. 节点验证：网络中的矿工节点验证交易的合法性（余额是否足够、签名是否正确等）

4\. 打包成块：通过共识机制（如工作量证明），矿工将验证过的交易打包成新的区块

5\. 链接上链：新区块被添加到区块链上，更新全网的账本状态

6\. 奖励发放：成功打包区块的矿工获得代币奖励和交易手续费

### ·区块链的分类

主要分为三种：

公链：像一个完全开放的公园，任何人都可以自由进入、散步、拍照、甚至参与公园的维护（比如修剪草坪、清理垃圾）。公园里没有管理员，所有规则由大家共同制定。

联盟链：像一个由多家公司（比如银行、物流公司）组成的 董事会，他们共同管理一个共享数据库（比如客户信用信息）。只有这些公司才能参与决策，外人不能随便加入。

私链：像一个私人俱乐部，只有会员才能进入。俱乐部的老板（比如 CEO）完全控制规则，比如谁可以加入、谁能查看账本（会员消费记录）。

![different-chains-BJzFHow9.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-12-1768220917577-different-chains-BJzFHow9.jpg)

### · [**Web3 vs Web 3.0 vs Web2 的范式革命**](https://web3intern.xyz/zh/blockchain-basic/#%E5%9B%9B%E3%80%81web3-vs-web-3-0-vs-web2-%E7%9A%84%E8%8C%83%E5%BC%8F%E9%9D%A9%E5%91%BD)

**Web2(当前主流互联网）:**

**核心特征：**

-   **中心化控制**：数据存储在科技巨头的服务器（如 Google、Facebook）
    
-   **用户角色**：内容生产者，但不拥有数据
    
-   **商业模式**：广告驱动，平台抽取佣金
    
-   **典型应用**：微信、抖音、亚马逊
    

**Web3.0(语义网）：**

**核心特征：**

• 语义标记：使用 RDF、OWL 等标准描述数据关系

• 结构化数据：信息按照标准格式组织，便于机器理解

• 知识图谱：构建实体间的语义关系网络

• 典型技术：本体工程、语义查询语言（SPARQL）、链接数据

**关键区别：**

• ❌ 不是区块链技术，而是传统互联网的数据组织升级

• ❌ 主要不依赖 AI，而是通过标准化数据格式实现

• ✅ 与 Web3 可结合（语义标记 + 区块链存储）

**Web3(去中心化互联网）：**

**核心特征：**

• 数据主权归用户：用区块链存储身份和资产

• 无需信任中介：智能合约自动执行规则

• 核心组件： 钱包 ->Dapp->智能合约->区块链

**核心创新：**

• 真正拥有数字资产：你的 NFT 头像、游戏道具真正属于你，平台无法删除或收回

• 金融服务无门槛：无需银行卡，用手机钱包就能借贷、理财、交易

• 应用可自由组合：一个 DeFi 协议的流动性可以被其他应用直接调用，就像搭积木

• 内容永不消失：文章、图片存储在分布式网络，不会因为平台关闭而丢失

## 二、分享会——Web3 行业全局介绍 & 岗位概览

Emilly老师主要介绍了Web3的发展历程（09年至今，共历经四个阶段），Web3 的工作模式，当前的用人

岗位、行业生态，以及Web3的入职建议。

![屏幕截图 2026-01-12 202503.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-12-1768222177168-_____2026-01-12_202503.png)![屏幕截图 2026-01-12 202803.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-12-1768222233113-_____2026-01-12_202803.png)![屏幕截图 2026-01-12 203038.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-12-1768222309007-_____2026-01-12_203038.png)![屏幕截图 2026-01-12 203442.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yubr-zzh/images/2026-01-12-1768222325471-_____2026-01-12_203442.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
