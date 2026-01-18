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
# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->
## 二、节点间的连接与通信方式

可以把以太坊节点的“社交流程”拆成三步：

先找到人 → 再拉上加密专线 → 最后靠八卦（gossip）把消息传满全网。

RLPx = 在一条 TCP 线上开很多“分机”：同一根网线同时承载区块同步、 交易广播、状态请求等多种子协议流量

这也是为什么会说：“以太坊的 P2P 网络主要由 UDP 的 Node Discovery 协议 + TCP 上的 RLPx/devp2p 协议 组成”。

Gossip模式：随机告诉几个不知道的邻居完成传输

• 实际测量中，以太坊能在几秒钟级别完成一笔交易 / 区块的全网传播。

这种模式就像：

**Gossip = 大家在茶水间互相聊最新八卦；**

**请求-响应 = 你专门去档案室翻某一天的旧报纸。**

**两者结合在一起，才能既保证新消息快速散播，又保证任何节点都能随时补 齐历史、追上最新状态。**

## 三、全节点、轻节点、归档节点的区别

先来做个一句话总结：

全节点 = 当前状态 + 全部区块（但老状态会被“裁剪”）；

归档节点 = 全节点 + 从创世到现在的“所有历史状态”；

轻节点 = 只存区块头，需要时向全节点要具体数据。

### 全节点（Full Node）

运行一对执行客户端 + 共识客户端，从创世块起验证整条链。

保存 所有区块的区块头和区块体。

只保留最近若干个区块的完整状态

### 归档节点（Archive Node）

禁用修剪（pruning）的全节点。

巨额开销

### 轻节点 / 轻客户端（Light Node / Light Client）

只保存 区块头（block headers），不保存完整区块体和全量状态；
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->

# 第二章 网路结构与节点类型

## 一、以太坊节点与客户端软件

-   节点（node）：任何一台运行以太坊客户端软件并连接到其他节点的电脑
    
-   客户端（client）：是对以太坊协议的具体实现，会按协议规则验证数据、同 步区块和状态，帮助网络保持安全。
    

客户端分三种：

-   执行客户端：主要负责链上的业务逻辑和状态
    
-   共识客户端：主要负责权益证明 PoS 共识
    
-   验证者客户端：在轮到你时提议新区块，在每个 slot 为看到的区块做 attestation（投票）
    

客户端之间的通信：

Engine API —— 一个专门给 EL ↔ CL 用的 JSON-RPC 接口。

小结（可以直接背下来）

以太坊节点 = 一台电脑 + 两个必备客户端 + （可选）一个验证者客户端：

• 执行客户端负责“算交易、管状态、对外提供 RPC”；

• 共识客户端负责“跑 PoS、选区块、投票达成共识”；

• 它们通过 Engine API 的“专线电话”互相协作，整个节点再通过 P2P。。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NDRiMTgwMDVkOGFjN2ZmYjI4ZThhNWYyNzdhZDlkNTBfa1Zieno1YU5TOEdXeEZaeWhHVFhNdTM2WWlyaXlmZHlfVG9rZW46RmM2WmJPSm9Mb1VxWVN4VkpmOWM0UnBibjBnXzE3Njg3MjM0MTY6MTc2ODcyNzAxNl9WNA)
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->






## 十、生态系统创新：DeFi、NFT 与 DAO

分成三块：

1.  Defi：金融操作系统
    
2.  NFT：数字资产与身份
    
3.  DAO：组织与治理层
    

创意型 DAO：

比如Nouns DAO 通过每天拍卖一个 NFT，让拍卖所得直接进入 DAO 金 库，再由持有者投票决定怎么花钱（赞助活动、做衍生产品、支持公共 物品等）。

这种模式把“IP 创作 + 社区治理 + 金库管理”统一在一起，是 NFT + DAO 的一个代表性创新。

我对DAO的理解是一个帮会，而不像一家公司，DAO组织不以盈利为目标，更像是一个社群，给大家一个统一的目标和议题，大家一起通力协作去解决一些治理问题。比如大学的每一个社团都可以理解为一个DAO，大学的流浪猫救助组织，也可以当成一个流浪猫救助DAO。DAO是松散绑定形式，以兴趣为导向，但不排除有公司形式的DAO组织（比如协议DAO）

## 十一、社区与开发生态扮演的角色

社区=“大脑 + 声音”，负责共识、价值观、方向感；

开发生态=“手和工具”，负责把这些想法变成真正跑在主网和 L2 上的 代码与应用。

### 社区之中

1.1 去中心化治理与共识形成

• 核心开发者会议（AllCoreDevs，含 ACD/ACDC 等电话会）

• 以太坊魔术师（Fellowship of Ethereum Magicians，FEM）

提案一般先在FEM公开论坛上进行长时间的讨论与打磨，随后进入AllCoreDevs中进行公开决策，所有流程全部公开透明

1.2 多元化：谁都能参与、谁都能发声

作为一名成员，你可以参与生态建设的方式：

• 撰写和评审 EIP / ERC 标准

• 在 Devcon、Devconnect、各地的 Ethereum 社区会议上分享经验；

• 参加 ETHGlobal 等黑客松，在 36 小时里做出一个 MVP，很多知名 项目（例如一些 DeFi、NFT 协议）都是从这里起步；

• 在本地社区组织 meetup、读书会、工作坊，帮助新人进入 Web3 世界。

这种“任何角色都能贡献”的多元性，让以太坊在设计协议时，能听到来自 钱包、L2、DeFi、NFT、游戏、监管等不同视角的真实需求。

### 开发生态系统

2.1 工具与基础设施：把开发门槛打下来

**• 开发框架与 IDE**

  Remix IDE：浏览器里就能写、编译、部署合约，非常适合新手和教 学场景；

  Hardhat、Truffle、Foundry：用来写测试、做本地链模拟、脚本化 部署，已经成为 Solidity 开发的“三件套”。

**• 节点与基础设施服务**

像 Infura、Alchemy、QuickNode 这样的基础设施提供商，负责维 护高可用的节点和 RPC 服务，让 dApp 不必自己运维节点。

**• 脚手架与前端集成工具**

Scaffold-ETH、Wagmi、第三方 SDK 等，帮开发者快速搭建前端、 集成钱包和合约调用；

2.2 教育与培训：把新人源源不断“拉进来”

-   官方与社区文档 / 教程：[ethereum.org](http://ethereum.org)
    
-   Devcon / Devconnect / 本地黑客松
    

其中Devcon是以太坊官方的黑客松，更专业和盛大，Devconnect和本地黑客松更适合新人练手startup，更针对性

-   ETHGlobal 等黑客松平台
    

这是学习的最佳平台
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->







## 八、网络结构的开放性与参与机制

permissionless：任何人都可以运行节点、发送交易、部署合约，网络本身不设“准入门槛” 或白名单

### 1\. 节点的性能要求

存储，带宽和计算性能

• 全节点：4 核 CPU、16 GB 内存、1 TB NVMe SSD、至少 20–25 Mbps 稳定带宽，且尽量不要有流量封顶；

• 归档节点：动辄 4–10 TB SSD、64 GB 内存以上，只适合专业服务商或 基础设施项目来跑。

### 2\. 共识效率的四个重要指标

1.  吞吐量
    
2.  延时
    
3.  资源消耗
    
4.  容错能力
    

总结：节点太多太少都不好，需要一个trade off

### 3\. 以太坊技术路线选择

明确的：现在的官方路线已经演变成更明确的 “Rollup 为中心 + 数 据分片（Danksharding/PeerDAS）”

Layer1：负责共识、安全、最终结算和数据存储

Layer2：

常见的Layer2有3种类型，Rollup，State Channels，sidechains

Rollup又分为Optimistic和ZK Rollup

我对ZK的理解是，牺牲了极小部分效率，提升了很多安全性，因为ZK基于红黑树状结构，本身是平衡树，效率是O(logn)量级

因此出现了 Rollup、Proto-Danksharding / Danksharding、PeerDAS 等一系列扩容方案，专门用来在不牺牲去中心化的前提下，把性能拉上去。

给一个可以背的句子（面试的时候可以直接使用）：

以太坊的网络结构 = 理论上对任何人开放（permissionless），现实中受限 于硬件和带宽，但通过 Layer 2 + 数据分片等方案，把“更多人参与”和 “系统还能跑得动”两件事尽量同时做到。

## 九、行业应用案例：金融、游戏与社交

### 1\. 去中心化金融（DeFi、稳定币、RWA）

-   Uniswap(dex/自动做市 AMM)
    
-   Aave（自动借贷协议）
    
-   MakerDAO/Sky + DAI稳定币（抵押ETH发行稳定币）
    
-   Lido、EigenLayer 质押/再质押协议
    
-   RWA（Real-World Assets，现实世界资产上链）
    

### 2\. 游戏、虚拟世界与 NFT

-   NFT 赛道最早就是在以太坊上被玩“爆”的。
    
-   CryptoKitties（加密猫）
    
-   Decentraland（虚拟土地 / 元宇宙）
    
-   The Sandbox（开放世界 / 创作者经济）
    
-   Axie Infinity、Illuvium、The Sandbox 等链游生态
    
-   OpenSea、Blur、Magic Eden 等 NFT 市场，OpenSea 长期是以太坊 NFT 交易量的重要一极。
    

### 3\. 社交、内容创作与 DAO / 公共物品

-   Lens Protocol / Lens Chain
    
-   Farcaster
    
-   [friend.tech](http://friend.tech) 等 SocialFi 应用
    
-   协议治理 DAO：Uniswap DAO、Aave DAO、MakerDAO
    
-   Gitcoin Grants 等公共物品资助平台
<!-- DAILY_CHECKIN_2026-01-16_END -->

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
