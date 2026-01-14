---
timezone: UTC+8
---

# somewhere1

**GitHub ID:** somewhere1

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
# 1.基本的节点概念：

### 1\. Merge 之后的节点组成

Merge 之后，一个完整以太坊节点不再是单一程序，而是由多个独立进程组成：

-   **执行客户端（Execution Client, EL）**
    
-   **共识客户端（Consensus Client, CL）**
    
-   **验证者客户端（Validator）**
    

> 完整节点 = EL + CL  
> 验证节点 = EL + CL + Validator

* * *

## 二、执行客户端（Execution Client, EL）

### 1\. 核心职责

-   接收并验证交易（签名、nonce、余额、gas）
    
-   在 **EVM** 中执行交易与智能合约
    
-   维护世界状态（账户余额、storage、state trie）
    
-   维护执行层数据库（LevelDB / MDBX 等）
    
-   提供 **JSON-RPC 接口（eth\_\***）
    
-   运行执行层 P2P 网络（devp2p / RLPx）
    
-   通过 **Engine API** 与共识客户端协作
    

### 2\. 对外接口

-   dApp / 钱包 / 脚本 → `eth_*` RPC → 执行客户端
    

### 3\. 常见实现

-   Geth
    
-   Nethermind
    
-   Besu
    
-   Erigon
    
-   Reth
    

* * *

## 三、共识客户端（Consensus Client, CL）

### 1\. 核心职责

-   运行 PoS 共识（Beacon Chain）
    
-   维护 slot / epoch 时间节奏
    
-   选择区块提议者（proposer）
    
-   处理验证者投票（attestation）
    
-   执行 fork choice（LMD-GHOST）
    
-   维护最终性（Casper FFG）
    
-   管理验证者集合、质押余额、奖励与惩罚
    
-   运行共识层 P2P 网络（Beacon blocks、attestations、slashings）
    

### 2\. 不做的事情

-   **不执行交易**
    
-   **不维护 EVM 状态**
    
-   **不直接给 dApp 提供 eth RPC**
    

### 3\. 常见实现

-   Lighthouse
    
-   Prysm
    
-   Teku
    
-   Nimbus
    
-   Lodestar
    

* * *

## 四、验证者客户端（Validator）

### 1\. 作用

-   管理验证者私钥
    
-   在轮到自己时：
    
    -   提议新区块
        
    -   对区块进行 attestation（投票）
        
-   参与奖励 / 惩罚机制（slashing）
    

### 2\. 实现方式

-   有的 CL 内置 validator 子进程
    
-   有的项目将 validator 拆成独立进程
    

### 3\. 关键点

-   验证者 **必须绑定某个共识客户端**
    
-   不直接与执行层通信（经由 CL 间接）
    

* * *

## 五、Engine API（EL ↔ CL）

### 1\. 定义

-   Merge 后引入的 **内部 JSON-RPC 接口**
    
-   专门用于执行客户端与共识客户端之间通信
    

### 2\. 核心交互流程

-   CL 发送区块骨架（block header + tx list）
    
-   EL：
    
    -   执行交易
        
    -   计算状态变更
        
    -   返回 execution payload（state root 等）
        
-   CL：
    
    -   验证 payload 合法性
        
    -   决定是否接受该区块
        

### 3\. 特点

-   **只用于 EL ↔ CL**
    
-   不对 dApp / 用户开放
    
-   是 Merge 架构的核心耦合点
    

* * *

## 六、节点间的网络通信

### 总体流程

1.  节点发现（UDP）
    
2.  建立安全连接（TCP + 加密）
    
3.  数据传播（Gossip + 请求/响应）
    

* * *

### 1\. 节点发现（Discovery）

-   基于 **UDP**
    
-   使用 **Kademlia DHT**
    
-   协议：discv4 / discv5
    
-   消息类型：
    
    -   Ping / Pong
        
    -   FindNode
        
    -   Neighbors / Nodes
        

目的：

> 找到一组“ID 距离接近”的活跃邻居节点

* * *

### 2\. 建立连接（TCP + RLPx）

-   使用 TCP 建立稳定连接
    
-   在 TCP 之上运行：
    
    -   **devp2p 协议栈**
        
    -   **RLPx 加密与多路复用**
        

RLPx 功能：

-   握手协商密钥
    
-   建立加密会话
    
-   在一条 TCP 上复用多个子协议（capabilities）
    

执行层常见子协议：

-   eth/66, eth/68
    
-   les（轻客户端）
    

* * *

### 3\. 消息传播机制

3.1 Gossip（广播）

-   用于：
    
    -   新交易
        
    -   新区块
        
    -   共识消息（Beacon block、attestation）
        
-   特点：
    
    -   去中心化
        
    -   低延迟
        
    -   高鲁棒性
        

3.2 请求-响应（Request / Response）

-   用于：
    
    -   同步历史区块
        
    -   拉取区块体、receipts、状态
        
-   精确点对点请求
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

# 继续学习以太坊基础知识

1.**目标定位: Ethereum** “全球可编程计算机”：可运行智能合约dApps 的通用平台

2.**共识机制:** 全面采用**PoS**

**3.可扩展性与应用:** 原生支持智能合约与 EVM，已经发展出DeFi、NFT、DAO、游戏、公链 L2 等完整应用生态

**4.代码 / 协议 复杂度:** 协议和执行环境更复杂，升级频繁（London、Merge、Dencun 等），既带来功能，也带来实现和安全上的挑战

5.**代币标准:** 在主网原生支持 ERC-20、ERC-721、ERC-1155 等标准，形成了可组合的代币与 NFT生态系统

6.**扩容方案:** 走 Rollup-**为中心的路线图**：L2 Rollups 负责执行，L1 负责结算与数据可用性；2024 年的 **Dencun 升级（EIP-4844）** 引入 Blob，显著降低 L2 数据成本，为未来 **Danksharding（数据分片）** 打基础

7\. **长期目的:** 成为 Web3 的基础设施：一个全球可编程结算层 + 数据可用性层，为各种 dApps、Rollups 和数字经济提供底层支持
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->


# 学习以太坊知识：

1.区块链是一个大的记账本

2.  EVM是Ethereum重要的一部分，可以让ETH这条区块链不只是一个记账本，还是可以运行solidity写的智能合约
    
3.  Ethereum上的的代币有ETH,ERC-20,ERC-721,ERC-1155.
    
4.  Ethereum采用PoS, 任何想要执行合约的人都需要支付Gas费用。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
