---
timezone: UTC+8
---

# Yuchen528

**GitHub ID:** Yuchen528

**Telegram:** 

## Self-introduction

Yuchen

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
听了《为什么是Web3? ——个体拥抱AI Agent的确定性选择》直播
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

嘟嘟嘟嘟啦啦啦啦啦啊

今天干嘛了呢，听了直播，没看别的了2333
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


今天看了一点访谈~

（hhhhh我发现我会自己给自己记假账哈哈哈哈哈
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



学习了社区运营指南

看了一点合规安全
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




智能合约开发

一、Dapp去中心化应用

1.  Dapp架构
    

-   前端 User Interface: 通过前端注入的Provider或第三方RPC节点与区块链交互，前端需要集成区块链钱包
    
-   智能合约 Smart Contracts：通常用Solidity编程语言编写，通过EVM执行
    
-   数据检索器 Indexer：智能合约通常以Event形式释放日志事件（如NFT转移的Transfer），数据检索器会检索这些数据并将其写入PostgreSQL等数据库；Dapp在前端进行数据展示时需要检索器内的数据。
    
-   区块链和去中心化存储 Blockchain & Decentralized Storage：区块链用于储存智能合约的状态数据及交易记录（eg. 去中心化存储IPFS（InterPlanetary File System）或Arweave）。通过去中心化备份。
    

2.  开发流程
    

-   需求分析：确当功能；选择区块链平台；设计用户体验(UX)
    
-   智能合约开发：\*\*Dapp的核心。\*\*编写智能合约(Solidity)；编写测试用例；审计和优化
    
-   检索器开发：\*\*获取链上数据的核心。\*\*确定功能需要的数据内容；编写检索器程序(主流:ponder/subgraph.用的都是TypeScript)；部署和运维(一般用Docker)
    
-   前端开发：选择前端框架(React/Vue)通过JavaScript与智能合约进行交互；连接钱包；显示区块链数据；处理交易签名与确认
    
-   与区块链交互：前端和智能合约通过**Viem**等现代化库进行交互。读取数据；发送交易
    
-   部署与上线：部署智能合约(Hardhat/Foundry)到测试网或者主网；前端部署(去中心化平台IPFS或传统Web)；发布和维护
    

二、以太坊开发环境搭建

1.  基础环境：Node.js（建议用 nvm 管理）;npm 或 yarn;Git
    
2.  以太坊本地开发连接
    

-   Foundry
    
-   Hardhat
    

3.  以太坊钱包和前端交互
    
4.  其他
    

-   Remix IDE（网页版，适合快速测试合约）：[**https://remix.ethereum.org**](https://remix.ethereum.org)
    
-   OpenZeppelin 合约库：`npm install @openzeppelin/contracts`
    
-   [**Chainlink 测试环境**](https://docs.chain.link/resources/link-token-contracts) - 预言机集成指南
    

三、PRC\*\*（Remote Procedure Call，远程过程调用）\*\*

1.  What?
    

-   一种通信协议。允许 应用 通过网络调用 远程服务器上的 函数。
    
-   区块链中PRC节点是 运行区块链客户端软件的 服务器。维护完整的区块链数据副本，提供API供开发者查询链上数据、发起交易等。-----ATM
    

2.  在Web3开发中的应用
    

-   读取链上数据
    
-   发送交易
    
-   监听事件：监听Events；实时获取链上状态变化；支持WebSocket长连接推送
    
-   网络管理：切换不同的区块链网络(主网、测试网)；获取网络信息和链 ID；管理节点连接状态
    

3.  以太坊使用 **JSON-RPC 2.0** 协议作为标准的 RPC 通信格式。所有请求和响应都是 JSON 格式，通过 HTTP 或 WebSocket 传输。
    
4.  主流RPC
    
5.  How获取（Alchemy）—>使用—>配置
    
6.  RPC使用最佳实践
    

-   保护API Key
    
-   重试机制；设置合理的超时时间；处理常见的错误码（如 `429` 速率限制、`503` 服务不可用）
    
-   速率限制管理
    
-   多节点备份
    
-   监控和日志
    
-   本地节点开发
    

四、Solidity智能合约编程(((((学到这儿脑子有点疼先算了哈哈哈

Solidity 是一种面向合约的高级编程语言，专门用于在以太坊虚拟机（EVM）上编写智能合约。它具有静态类型、支持继承、库和复杂的用户定义类型等特性。

1.  基础语法与开发范式
    

pragma solidity ^0.8.0; ---------版本声明

数据类型：bool;
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->






岗位概览

1.  技术岗
    

-   前端工程师：HTML, CSS, JavaScript, React, Vue, Viem
    
-   后端工程师：Node.js, Python, Go, Viem, MySQL, PostgreSQL
    
-   智能合约工程师：Solidity
    

2.  非技术岗
    

-   产品与运营
    
-   社区管理
    
-   研究分析：SPSS, Python, 链上数据分析工具（Glassnode、Token Terminal）
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->







行业黑话：

1.  基础类
    

-   DYOR
    
-   FOMO
    
-   FUD
    

2.  技术类
    

-   L1/L2
    
-   EVM: Ethereum Virtual Machine，以太坊虚拟机，运行智能合约的核心
    
-   Smart Contract
    
-   Bridge: 跨链桥，用于在多条链之间转移资产
    
-   Oracle: 预言机，用于向链上提供链下数据
    
-   Fork: 分叉，指复制已有代码的项目，或区块链协议分裂
    
-   Mint: 铸造，生成新的 NFT 或代币
    
-   Burn
    
-   Rugpull: 抽地毯，项目方卷款跑路（尤其在 DeFi 项目）
    

3.  投资类：
    

-   Pump: 拉盘，代币价格快速上涨
    
-   Dump: 砸盘，代币价格快速下跌
    
-   HODL: 原为"Hold"打错，后来变成文化，意为坚定持币不卖
    
-   Bagholder: "接盘侠"，高位买入亏损后长期持币的人
    
-   Alpha: 内部消息/潜在机会，表示未公开但价值潜力大的信息
    
-   Tokenomics: 代币经济学，研究代币发行、分配、用途等机制
    

4.  文化类
    

-   gm/gn
    
-   Anon: 匿名者
    
-   KOL: Key Opinion Leader，意见领袖，影响力人物
    
-   CT: Crypto Twitter，加密行业活跃的信息与讨论来源
    
-   DAO: 去中心化自治组织，社区驱动型的组织管理形式
    
-   NFT PFP: NFT 头像项目，如 CryptoPunks、BAYC，PFP = Profile Picture
    

5.  开发类
    

-   Hardhat / Foundry: 常用的智能合约开发框架
    
-   RPC: Remote Procedure Call，链上节点访问接口
    
-   Gas
    
-   Etherscan: 区块浏览器，用于查看合约、交易、地址信息
    
-   Viem / Ethers.js / Wagmi: Web3 前端交互库
    
-   ABI: Application Binary Interface，合约调用所需的接口信息
    
-   Mainnet / Testnet: 主网 / 测试网，分别对应正式和测试环境
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->








Web3工作方式

1.OKR写法与最佳实践：目标（Objectives）全团队公开可见，关键结果（Key Results）量化可追踪

2.远程会议：明确需求、决策留痕。决策事项 + 行动项明确 **DRI**（直接责任人）+ **Deadline**（精确到时区）

3.软技能：文档能力，AI能力、媒体工具

-   **Web3 常用工具：**
    
    -   CoinMarketCap 全球最值得信赖的加密货币数据、洞见和社区来源：[**https://coinmarketcap.com/**](https://coinmarketcap.com/)
        
    -   CoinGecko 是世界上最大的独立加密货币数据聚合器：[**https://www.coingecko.com/**](https://www.coingecko.com/)
        
    -   DefiLlama 是 DeFi（去中心化金融）领域最大的 TVL 聚合器：[**https://defillama.com/**](https://defillama.com/)
        
    -   RootData 是一个 Web3 资产数据平台：[**https://www.rootdata.com/zh/Projects?influenceSort=2**](https://www.rootdata.com/zh/Projects?influenceSort=2)
        
    -   媒体：
        
        -   律动 BlockBeats：[**http://theblockbeats.info/**](http://theblockbeats.info/)
            
        -   PANews：[**https://www.panewslab.com/zh**](https://www.panewslab.com/zh)
            
    -   免费发推抽奖工具：[**https://apidance.pro/twitter\_giveaway**](https://apidance.pro/twitter_giveaway)
        
    -   查看链上持仓：[**https://pummmm.com**](https://pummmm.com)
        
    -   查看持币地址、筹码分布：[**https://bubblemaps.io/**](https://bubblemaps.io/)
        
    -   即将启动的加密项目 IDO、IEO、ICO 列表：[**https://www.coincarp.com/zh/upcoming-ido/**](https://www.coincarp.com/zh/upcoming-ido/)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->









今天学习了以太坊核从账户到执行的完整链路，下载了钱包~

行业赛道：

1.  DeFi
    
2.  NFT
    
3.  DAO
    
4.  MEME
    

交叉创新领域

1.  DAO+MEME
    
2.  AI+DeFi
    
3.  社区代币分发创新
    
4.  WEB3+乡建
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->










-   Bitcoin & Ethereum
    

| 维度 | 比特币（Bitcoin） | 以太坊（Ethereum） |
| --- | --- | --- |
| 目标与定位 | 去中心化的数字货币，强调安全、稳定和稀缺性（总量 2100 万枚） | 去中心化平台，支持智能合约和 Dapps，定位为“区块链 2.0” |
| 编程能力 | 脚本语言有限，仅支持简单的交易验证逻辑 | 图灵完备的编程语言（如 Solidity），可开发复杂智能合约 |
| 共识机制 | 工作量证明（PoW），矿工通过算力竞争记账权 | 从 PoW 转向权益证明（PoS），通过 The Merge 实现能源效率优化 |
| 交易速度 | 每 10 分钟生成一个区块，交易确认较慢 | 区块时间约 12 秒，交易确认更快，适合高频应用 |
| 经济模型 | 总量固定，强调抗通胀属性 | 供应灵活，通过 EIP-1559 等机制可能呈现通缩趋势 |

PoW --The Merge–> PoS

**PoW:** 耗能高，拓展性差

-   矿工通过计算机算力“挖矿”，争夺打包交易的权利。
    
-   矿工需要消耗大量电力资源，竞争激烈。
    
-   成功打包区块的矿工会获得新生成的 ETH 奖励。
    

**PoS:**

验证者如何工作：

-   准入门槛：质押 32 ETH 成为验证者
    
-   工作方式：系统随机选择验证者来提议和验证区块
    
-   奖励机制：验证者获得新发行的 ETH + 交易费用
    
-   惩罚机制：作恶者质押的 ETH 被销毁（Slashing）
    

**相比 PoW 的优势**：

-   能耗降低 99.95%：无需大量电力和硬件
    
-   经济安全性：攻击成本约需控制全网 67% 的质押 ETH（价值数百亿美元）
    
-   最终确定性：区块确认更快、更可靠
    

**future**

1.  分片技术演进——从执行分片到数据分片
    
2.  EIP-4844（Cancun 升级）——“省钱神器”：Layer2链下处理，打包提交，主网验证
    
3.  ZK-Rollup 技术——“批量验证，一步到位”
    

**以太坊生态**

**1.Layer 1**

-   以太坊主网：核心区块链，负责最终安全性与共识。
    
-   EVM：以太坊虚拟机，执行智能合约代码。
    
-   账户系统：外部账户（EOA）与合约账户（CA）共同构成网络基础。
    

**2.Layer 2**

-   Rollup：通过将交易批量处理后提交至 L1，降低 Gas 费。
    
    -   Optimistic Rollup：假设交易合法，仅在争议时验证。
        
    -   ZK Rollup：通过零知识证明验证交易，无需链上争议。
        

**3.Sidechains(侧链)**：独立运行的链，通过桥接与主网交互。

**4.以太坊生态分层架构**

-   **应用层Application Layer** 用户直接交互的应用和界面：
    
-   _DeFi 应用：Uniswap（去中心化交易所）、Aave（借贷协议）、Compound（借贷协议）_
    
-   _NFT 平台：OpenSea、Foundation、SuperRare_
    
-   _钱包应用：MetaMask、Coinbase Wallet、Rainbow_
    
-   _DAO 工具：Snapshot、Aragon、Colony_
    
-   **协议层Protocol Layer** 以太坊的核心基础设施
    
-   _共识层客户端：Prysm、Lighthouse、Nimbus、Teku_
    
-   _执行层客户端：Geth、Nethermind、Erigon、Besu_
    
-   _核心协议：EVM、状态管理、Gas 机制_
    
-   **扩展曾Scaling Layer** 提升性能和降低成本的解决方案
    
-   _Layer 2 Rollups：Arbitrum、Optimism、Polygon zkEVM、zkSync Era_
    
-   _侧链：Polygon PoS、xDAI（Gnosis Chain）_
    
-   _状态通道：Lightning Network for Ethereum_
    

价值观：区中心化、无需许可与开放性、抗审查、密码朋克精神、公共物品导向、可持续发展理念

* * *

* * *

* * *

tips: 很迟的听了开营仪式hhh然后记录了一下emmmm
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->












-   Bitcoin & Ethereum
    

| 维度 | 比特币（Bitcoin） | 以太坊（Ethereum） |
| --- | --- | --- |
| 目标与定位 | 去中心化的数字货币，强调安全、稳定和稀缺性（总量 2100 万枚） | 去中心化平台，支持智能合约和 Dapps，定位为“区块链 2.0” |
| 编程能力 | 脚本语言有限，仅支持简单的交易验证逻辑 | 图灵完备的编程语言（如 Solidity），可开发复杂智能合约 |
| 共识机制 | 工作量证明（PoW），矿工通过算力竞争记账权 | 从 PoW 转向权益证明（PoS），通过 The Merge 实现能源效率优化 |
| 交易速度 | 每 10 分钟生成一个区块，交易确认较慢 | 区块时间约 12 秒，交易确认更快，适合高频应用 |
| 经济模型 | 总量固定，强调抗通胀属性 | 供应灵活，通过 EIP-1559 等机制可能呈现通缩趋势 |

PoW --The Merge–> PoS

**PoW:** 耗能高，拓展性差

-   矿工通过计算机算力“挖矿”，争夺打包交易的权利。
    
-   矿工需要消耗大量电力资源，竞争激烈。
    
-   成功打包区块的矿工会获得新生成的 ETH 奖励。
    

**PoS:**

验证者如何工作：

-   准入门槛：质押 32 ETH 成为验证者
    
-   工作方式：系统随机选择验证者来提议和验证区块
    
-   奖励机制：验证者获得新发行的 ETH + 交易费用
    
-   惩罚机制：作恶者质押的 ETH 被销毁（Slashing）
    

**相比 PoW 的优势**：

-   能耗降低 99.95%：无需大量电力和硬件
    
-   经济安全性：攻击成本约需控制全网 67% 的质押 ETH（价值数百亿美元）
    
-   最终确定性：区块确认更快、更可靠
    

**future**

1.  分片技术演进——从执行分片到数据分片
    
2.  EIP-4844（Cancun 升级）——“省钱神器”：Layer2链下处理，打包提交，主网验证
    
3.  ZK-Rollup 技术——“批量验证，一步到位”
    

**以太坊生态**

**1.Layer 1**

-   以太坊主网：核心区块链，负责最终安全性与共识。
    
-   EVM：以太坊虚拟机，执行智能合约代码。
    
-   账户系统：外部账户（EOA）与合约账户（CA）共同构成网络基础。
    

**2.Layer 2**

-   Rollup：通过将交易批量处理后提交至 L1，降低 Gas 费。
    
    -   Optimistic Rollup：假设交易合法，仅在争议时验证。
        
    -   ZK Rollup：通过零知识证明验证交易，无需链上争议。
        

**3.Sidechains(侧链)**：独立运行的链，通过桥接与主网交互。

**4.以太坊生态分层架构**

-   **应用层Application Layer** 用户直接交互的应用和界面：
    

-   _DeFi 应用：Uniswap（去中心化交易所）、Aave（借贷协议）、Compound（借贷协议）_
    
-   _NFT 平台：OpenSea、Foundation、SuperRare_
    
-   _钱包应用：MetaMask、Coinbase Wallet、Rainbow_
    
-   _DAO 工具：Snapshot、Aragon、Colony_
    

-   **协议层Protocol Layer** 以太坊的核心基础设施
    

-   _共识层客户端：Prysm、Lighthouse、Nimbus、Teku_
    
-   _执行层客户端：Geth、Nethermind、Erigon、Besu_
    
-   _核心协议：EVM、状态管理、Gas 机制_
    

-   **扩展曾Scaling Layer** 提升性能和降低成本的解决方案
    

-   _Layer 2 Rollups：Arbitrum、Optimism、Polygon zkEVM、zkSync Era_
    
-   _侧链：Polygon PoS、xDAI（Gnosis Chain）_
    
-   _状态通道：Lightning Network for Ethereum_
    

价值观：区中心化、无需许可与开放性、抗审查、密码朋克精神、公共物品导向、可持续发展理念

* * *

* * *

* * *

tips: 今天在实习没能完整的听完视频，只能先自己卡看文档学习sadQAQ
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->













区块链：时间顺序相连的区块

-   区块：区块头（参与hash计算:verson, pre-block-hash, merkle-root, timestamp, difficulty/targrt, nonce）+区块体（transcations)
    
-   特性：不可篡改，公开透明且匿名，快速交易
    

去中心化的网络

-   优点：信任最小化；康审查与高弹性；用户自主管理；开放创新生态(Dapps)
    
-   挑战：可拓展性瓶颈；安全与治理难题(DAO投票权集中)；用户体验与成本；合规
    

公链&联盟链&私链

以太坊Ehereum & ETH

-   Etherum is a blockchain; Ether is its token.
    
-   Etherum引入smart contract, NFT(Non-fungible Token)是其中之一
    
-   NFT features and values: unique, indivision; inmutable and transparent(可变且透明); everything can be NFT; Full control; Low cost on confirming the right; Infinite imagination
    

DYOR
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
