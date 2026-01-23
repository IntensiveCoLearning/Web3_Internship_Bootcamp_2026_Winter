---
timezone: UTC+8
---

# lorstyang

**GitHub ID:** lorstyang

**Telegram:** @lorstyang

## Self-introduction

ha

## Notes

<!-- Content_START -->
# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->
1.Ethereum Foundation Dev Growth 团队负责人 Austin 的英文分享会

2.本周例会学习
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->

1.参会 Dapp Workshop

2.参会南塘 DAO

3.继续优化一下留言板项目

4.solidity101
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



1.参会Uniswap 工作原理解析

2.参会休闲黑客松

3.完成留言板项目并部署[https://onchainmessageboard.vercel.app/](https://onchainmessageboard.vercel.app/)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->





1.参会公共物品资金分配

2.参会「Solidity Walk Through」

3.留言板项目实践
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->






# Remix

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/lorstyang/images/2026-01-19-1768834231707-image.png)

# 智能合约开发教程

## Dapp结构

-   前端界面 UI
    
    -   界面实现
        
-   智能合约 contract
    
    -   链上
        
-   检索器 indexer
    
    -   Dapp一般不会频繁的什么数据都去链上查，而是通过捕获智能合约释放的事件，把数据存下来，提供给前端展示
        
-   区块链和去中心化存储（Blockchain & Decentralized Storage）
    
    -   区块链用于存储智能合约的状态数据及交易记录
        
    -   去中心化存储如 **IPFS**（InterPlanetary File System）或 **Arweave**，用于存储大规模的非结构化数据（如图片、文档等），确保数据不易丢失和篡改。
        

## Dapp 的开发流程

-   需求分析
    
-   智能合约开发
    
-   前端开发
    
-   与区块链交互（读写区块链数据）
    
-   部署与上线
    

# 示例代码

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleStorage {
    // 这个变量用来存一个数字
    uint256 private myNumber;

    // 写入函数：把数字存进去 (要花 Gas)
    function store(uint256 _num) public {
        myNumber = _num;
    }

    // 读取函数：把数字读出来 (免费)
    function retrieve() public view returns (uint256) {
        return myNumber;
    }

    // 存进来的数加一
    function addOne() public  {
        myNumber += 1;
    }
}
```
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->










# Stuck Transactions（卡住的交易）

交易被广播了，但一直不被打包进区块

## 原因

-   Gas Price 太低
    
    -   给的 gas price < 当前网络接受的最低价
        
    -   矿工 / 验证者直接无视
        
-   Nonce 阻塞
    
    -   账户的交易必须 nonce 连续
        
    -   前一笔卡住 → 后面全卡
        

## 解决方案

-   加价替换（Replace）
    
    用相同的nonce\*\*，\*\*更高的 gas price 覆盖原交易
    
-   Cancel
    
    用相同的 nonce，给自己转 0 ETH，gas 拉高
    

# Gas Limits

为单次合约执行最多付多少计算费

👉如果Gas Limit 设太低，会导致：

-   执行到一半 Out of Gas
    
-   状态回滚
    
-   gas 费不退
    

一般多预留20%-30%的buffer

# Multisigs（多签钱包）

一个钱包需要“多个人同意”才能花钱

## 使用多签钱包的原因

| 场景 | 风险 |
| --- | --- |
| 私钥丢失 | 资产归零 |
| 单人权限 | 内鬼 / 失误 |
| 合约升级 | 权限滥用 |

## 核心价值

-   权限分散
    
-   抗单点失效
    
-   社会信任最小化
    

# L2s（Layer 2）

公链的扩展层，在不牺牲以太坊安全性的前提下，便宜 + 快

## 需要L2的原因

L1（以太坊主网）的问题：

-   慢
    
-   贵
    
-   吞吐低
    

## L2的核心思路

-   计算放到链下
    
-   结果提交到 L1
    

## 对比

| 维度 | L1 | L2 |
| --- | --- | --- |
| Gas | 贵 | 便宜 |
| TPS | 低 | 高 |
| 合约 | EVM | 基本兼容 |
| UX | 差 | 好 |

## 主流分类

### Optimistic Rollup

-   Arbitrum
    
-   Optimism
    

特点：

-   默认交易是对的
    
-   有挑战期（~7 天）
    

### ZK Rollup

-   zkSync
    
-   Starknet
    

特点：

-   数学证明
    
-   无挑战期
    
-   技术复杂
    

# Lending（借贷协议）

无需信任、超额抵押的链上银行

## 有什么用

用户可以：

-   存币 → 收利息
    
-   抵押 → 借币
    

代表项目：

-   Aave
    
-   Compound
    

## 核心机制

-   超额抵押
    
-   利率模型
    
    -   利率随资金利用率变化
        
    -   借得越多 → 越贵
        
-   清算（Liquidation）
    
    -   抵押率跌破阈值
        
    -   任何人可清算
        
    -   有奖励
        

## 风险

| 风险 | 说明 |
| --- | --- |
| 价格波动 | 强平 |
| Oracle | 价格被操纵 |
| 合约漏洞 | 直接归零 |

# 一个实际项目可能存在的流程

```
用户
 └─ L2 上发交易
       ↓
   Gas Limit 估算
       ↓
   交易 pending（可能 stuck）
       ↓
   合约是 Multisig owner
       ↓
   用户资产用于 Lending
```
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->












# ENS（Ethereum Name Service）

-   ENS 是以太坊上的**去中心化域名系统**，本质是个智能合约。
    
-   把“难记的钱包地址”变成“人能读的名字”
    
    -   0x7e3A…9f2C ❌
        
    -   `yang.eth` ✅
        
-   类似Email
    

## 原因

-   钱包地址太长、容易转错
    
-   一个地址只能“是地址”，没有身份感
    

## 作用

-   收款：别人直接给 `yang.eth` 转账
    
-   绑定信息：
    
    -   钱包地址
        
    -   Twitter / Github
        
    -   头像（NFT）
        
-   Web3 身份入口
    

# DEX（Decentralized Exchange）

去中心化交易所

-   Uniswap
    
-   Curve
    

## 核心机制

-   AMM（自动做市商）
    
-   用资金池定价，而不是挂单撮合
    

## 作用

-   去信任（不信交易所）
    
-   抗审查
    
-   资产自持
    

# Identity（去中心化身份）

“你是谁”不再由平台决定，而是你自己拥有

Identity 通常是由多个组件组合而成：

-   钱包地址（基础）
    
-   ENS（可读身份）
    
-   NFT / POAP（经历）
    
-   链上行为（信用、历史）
    

## 典型实现方式

-   DID（Decentralized ID）
    
-   Verifiable Credentials（可验证凭证）
    

# Inventory（资产/物品清单）

Inventory 在 Web3 里通常指：

-   Token（ERC20）
    
-   NFT（ERC721 / 1155）
    
-   SBT（不可转让身份资产）
    
-   游戏道具 / 凭证
    

## 与传统web2的区别

-   资产是用户直接持有
    
-   可跨应用使用
    
-   资产可组合性（Composability）
    

# Sybil（女巫攻击）

一个人伪装成“很多人”来薅羊毛或操纵系统

常见场景

-   空投刷号
    
-   DAO 投票作弊
    
-   治理攻击
    

是web3的痛点之一，原因：

-   创建地址几乎是 0 成本
    
-   没有 KYC 的前提下很难区分“真人”
    

## 解决手段（与ENS DEX Identity Inventory有关）

| 手段 | 依赖 |
| --- | --- |
| 空投门槛 | 历史交互 |
| 身份证明 | Identity / DID |
| 社交图谱 | ENS + 社交绑定 |
| 成本约束 | 质押 / Gas |
| SBT | Inventory（不可转让） |

# ERC20

同质化 Token 标准（Fungible Token）

EG

-   ETH（包装成 WETH）
    
-   USDT / USDC
    
-   治理 Token
    

## 关键接口

```solidity
balanceOf(address)
transfer(to, amount)
approve(spender, amount)
```

# ERC721

NFT 标准（Non-Fungible Token）

EG

-   图片 NFT
    
-   游戏道具
    
-   门票
    
-   ENS 域名（本质是 NFT）
    

## 关键接口

```solidity
ownerOf(tokenId)
safeTransferFrom(from, to, tokenId)
tokenURI(tokenId)
```

# ERC20 vs ERC721 的“设计哲学差异”

| 问题 | ERC20 | ERC721 |
| --- | --- | --- |
| 状态核心 | balance | ownership |
| 资产语义 | 价值 | 身份 / 属性 |
| 是否依赖 Metadata | 不强 | 非常强 |

-   ERC721 本身只管谁拥有哪个 tokenId，具体长什么样，依赖 Metadata
    

# IPFS（InterPlanetary File System）

去中心化文件系统

传统web2的文件放在单独的服务器里面，可能出现服务器关停，内容被替换等问题，这会破坏“链上不可篡改”的承诺

## IPFS 的工作方式

内容寻址，使用CID（Content ID，本质是个hash值）

```
ipfs://QmXk...abc
```

文件内容一变 → hash 就变

和传统的 URL（存在哪儿）不同，更侧重于存的是什么（hash），谁存不重要，只要有人存就能取

## IPFS ≠ 永久存储

IPFS 只负责寻址\*\*，\*\*不保证“有人一直帮你存”

解决方案

-   Pin 服务：
    
    -   Pinata
        
    -   Infura
        
-   或者 Filecoin / Arweave
    

# Metadata

通俗易懂的来说，Metadata = NFT 的说明书

## Metadata 是什么

对于 ERC721：

```solidity
tokenURI(tokenId) → 一个 URL
```

这个 URL 通常指向一个 JSON 文件

## 标准 NFT Metadata 示例

```json
{
  "name": "Cool Cat #123",
  "description": "A cool cat living on-chain",
  "image": "ipfs://Qm...xyz",
  "attributes": [
    { "trait_type": "Hat", "value": "Cap" },
    { "trait_type": "Mood", "value": "Chill" }
  ]
}
```

## Metadata 里通常放什么

| 字段 | 用途 |
| --- | --- |
| name | NFT 名字 |
| description | 描述 |
| image | 图片 / 视频 |
| attributes | 稀有度 / 属性 |
| animation_url | 动态内容 |
| external_url | 官网 |

# Metadata 在哪存？

常见的有三种方案

| 方案 | 去中心化 | 成本 | 灵活性 |
| --- | --- | --- | --- |
| 中心化服务器 | ❌ | 低 | 高 |
| IPFS | ⚠️ | 中 | 中 |
| 链上 | ✅ | 高 | 低 |

对于大多数项目而言Metadata JSON 在 IPFS，URI 写死在链上。

于是，一个完整NFT的结构是：

```
区块链（合约）
 └─ tokenId → tokenURI
                 ↓
           Metadata JSON（IPFS）
                 ↓
          image / media（IPFS）
```

# 常见问题

## Metadata 可被项目方改

-   URI 指向中心化服务器
    
-   项目方可以换图
    

👉 正确做法：

-   IPFS + immutable URI
    

## IPFS 文件没人 Pin

-   几个月后图片加载不出来
    

👉 正确做法：

-   多 Pin + 付费存储
    

## ERC721 滥用

-   本质是 ERC20 的东西却用 NFT
    

👉 判断标准：

-   是否需要“唯一性”？
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->














## 安全

### 常见风险

-   钓鱼攻击
    
    -   伪造邮件/网站
        
    -   社交平台钓鱼
        
    -   假冒客服/好友
        
-   木马
    
    -   伪装软件
        
    -   剪切板劫持
        
    -   浏览器插件
        
    -   远程控制
        
-   社工
    
-   供应链/第三方依赖攻击
    
-   地址污染与扫描木马
    
-   传统隐私与账号安全风险
    

### 2025安全情况

1200+ 起，35+ 亿刀，特点: 精准猎杀，广撒网

-   EG
    
    -   bybit，攻击多签服务提供商
        
    -   UXLINK，AI深伪（deepfake）+社工
        
    -   Balancer，计算精度与合约漏洞，比较经典的技术方面的问题，还有大数溢出等
        

### 攻击方式演变与新技术骗局

-   EIP7702 与新型钓鱼
    
-   软件供应链攻击
    
-   deepfake + 社工
    
-   AI 智能合约漏洞挖掘
    

## 合规

### **Web3 行业法律环境概览**

-   禁止 ICO（首次代币发行）；
    
-   禁止虚拟货币交易所运营与提供撮合服务；
    
-   明确虚拟货币不具有法偿性，不得作为支付工具；
    
-   禁止境外交易平台向中国境内居民提供服务；
    
-   打击以虚拟货币为载体的非法集资、传销、洗钱、赌博等活动。
    

### 一些关注点

-   涉及合约与期货
    
-   换汇
    
-   项目合规风险
    
-   劳工合同问题
    

[https://web3intern.xyz/zh/security/](https://web3intern.xyz/zh/security/)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
















# 创建钱包以及测试网转账

[https://sepolia.etherscan.io/tx/0x33c92ea26d4603816cc29b34793c042d4f2c8ddc0ac1f998604b4656f37eda59](https://sepolia.etherscan.io/tx/0x33c92ea26d4603816cc29b34793c042d4f2c8ddc0ac1f998604b4656f37eda59)

# 铸造NFT

[https://sepolia.etherscan.io/tx/0x4088b1550b659f7e0d2b6a7bb5d3a93834ab88c781e41535f379d5a72695b096](https://sepolia.etherscan.io/tx/0x4088b1550b659f7e0d2b6a7bb5d3a93834ab88c781e41535f379d5a72695b096)

# 参加Web3 运行原理分享会

-   钱包 私钥 个人主权
    
-   交易与签名
    
-   区块链网络运行
    
    -   [https://txcity.io/v/eth-btc](https://txcity.io/v/eth-btc)
        
    -   [https://beaconcha.in/block/24226416](https://beaconcha.in/block/24226416)
        
    -   [https://etherscan.io/](https://etherscan.io/)
        
-   智能合约
    
    -   [https://nft.myfirst.io/](https://nft.myfirst.io/)
        
-   协议升级
    
    -   [https://ethereum-magicians.org/](https://ethereum-magicians.org/)
        
    -   [https://ethernodes.org/](https://ethernodes.org/)
        
-   参考
    
    -   b站bv号 BV1NjBKBnEcu，**Vitalik Devconnect 2025 演讲**
        
        good for
        
        -   支付 金融应用（双花问题）
            
        -   ENS
            
        -   强有力的参与保证（eg.抗审查）
            
        -   不存在证明
            
        
        not good for
        
        -   隐私
            
        -   Extreme scalability（eg. 以太坊上运行LLM代价很大）
            
        -   低延迟（去中心化特性）
            
        -   现实世界信息
            

# 白皮书

[https://ethereum.org/zh/whitepaper/](https://ethereum.org/zh/whitepaper/)

读了一部分白皮书

回顾BTC的概念以及历史，提及其存在的一些缺陷

-   建立新区块链要付出更多的代价
    
-   脚本功能有限
    
-   元币协议存在扩展性差的缺陷
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

















# **区块链**

**定义**：区块链是一种**分布式、去中心化的公共数字账本**。它由按时间顺序排列并链接在一起的“区块”组成，每个区块记录着多笔交易或数据。  
**核心特征**：

-   **去中心化**：数据不存储在单一中心服务器，而是由网络中的多个节点共同维护和验证。
    
-   **不可篡改**：一旦数据被记录并添加到链上，就极难被修改或删除。这是通过密码学哈希和共识机制实现的。
    
-   **透明可追溯**：所有交易记录对网络参与者公开（在公有链上），并且可以追溯到创世区块。
    
-   **共识机制**：网络节点通过特定规则（如工作量证明PoW、权益证明PoS等）对交易有效性达成一致，无需信任中间方。
    

## **优势**

-   去中心化与抗审查
    
-   用户数据主权与所有权
    
-   透明度与可验证性
    
-   金融包容性与开放访问
    
-   创新商业模式与创作者经济: DeFi NFT Play-to-Earn
    
-   可编程资产与自动执行
    

## **挑战**

-   可扩展性瓶颈
    
-   用户体验复杂
    
-   监管与法律的不确定性
    
-   安全风险: 智能合约漏洞 私钥丢失/被盗 钓鱼诈骗和桥接攻击
    
-   中心化压力与悖论
    
-   环境与能源消耗
    
-   “炒作”与现实应用的差距
    

# 以太坊

以太坊是一个去中心化的区块链网络和软件开发平台，由加密货币以太币 (ETH) 提供支持。

这里涵盖了数千种加密货币以及跨多个领域（去中心化金融、非同质化代币、游戏、去中心化社交媒体和稳定币等）的应用程序。

## 以太币

以太币 (ETH) 是以太坊网络的原生加密货币。它用于支付使用网络及其之上构建的应用程序的交易费用（称为“gas”），通过质押来保障网络安全，并作为数字货币用于支付和投资——不受任何中心化实体、组织或政府的控制。ETH 是市值最大的加密货币之一。

## 智能合约

-   **定义**：智能合约是存储在区块链上的一段**可编程代码**。它像一份自动执行的数字合同，当预先设定的条件被满足时，代码就会自动、不可逆地执行。
    
-   **关键点**：它**不由任何单一方控制**，而是在以太坊网络上所有节点共同验证后执行，结果被永久记录在链上，确保了公正与透明
    

## EVM

-   **作用**：EVM是一个运行在以太坊网络所有节点上的虚拟环境。它为智能合约的执行提供隔离和安全保障，确保无论在哪里运行，相同输入都会得到完全相同的结果。
    
-   **燃料机制**：在EVM上执行操作（尤其是写入操作）需要支付 **“Gas费”** 。Gas费以原生代币 **ETH** 支付。这既是为了补偿矿工/验证者的计算资源，也是为了防止网络被无限循环或垃圾计算攻击。
    

## 共识机制

2022年9月，以太坊完成了从 **“工作量证明”** 到 **“权益证明”** 共识机制的史诗级升级。

-   **之前（PoW）**：依赖“矿工”通过算力竞争来出块，耗能巨大。
    
-   **之后（PoS）**：依赖“验证者”通过质押ETH来获得出块权，能耗降低了约99.95%。这是实现其未来可扩展性蓝图的关键第一步。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
