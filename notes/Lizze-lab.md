---
timezone: UTC+9
---

# Lizze-lab

**GitHub ID:** Lizze-lab

**Telegram:** 

## Self-introduction

Hi 大家好，我是 Litchi 🍵。目前计算机专业澳本在读。我是一名 Web3 小白，对去中心化架构很感兴趣，希望在这四周里能和大家一起硬核共学，和志同道合的小伙伴们交朋友，请多多指教～

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
# 以太坊概览学

**1.13**

* * *

### 一、 以太坊的介绍

-   **本质：** 一个开源的去中心化区块链平台，不仅是货币，更是**图灵完备**的计算环境。
    
-   **核心创新——智能合约：** 存储在链上的自动执行代码，实现“代码即法律”，无需中介干预。
    
-   **原生代币（ETH）：** \* **用途：** 支付燃料费（Gas Fee）、网络质押、价值存储。
    
    -   **状态：** 全球市值第二，自 _The Merge_ 后进入轻微通缩模型（年通胀率约 0.2%-0.8%）。
        

* * *

### 二、 Ethereum 与 Bitcoin 的差异

| 维度 | 比特币 (Bitcoin) | 以太坊 (Ethereum) |
| 定位 | 数字黄金（价值存储） | 世界计算机（应用平台） |
| 功能 | 简单转账脚本 | 复杂的智能合约与 Dapps |
| 共识机制 | PoW（算力挖矿） | PoS（权益质押） |
| 性能 | 区块时间 10 分钟 | 区块时间约 12 秒 |

* * *

### 三、 以太坊的定位与演进

以太坊经历了从“高能耗”向“高性能”的范式转移：

1.  **PoW 阶段：** 早期依赖矿机算力，存在能耗高、扩展性差（30 TPS）的问题。
    
2.  **The Merge（合并）：** 2022 年 9 月，执行层与信标链合并，正式转为 **PoS 机制**。
    
    -   **能耗：** 骤降 **99.95%**。
        
    -   **机制：** 质押 32 ETH 成为验证者，作恶将被罚没（Slashing）。
        
3.  **未来路线图：** \* **EIP-4844（坎昆升级）：** 引入 **Blob** 交易，使 Layer 2 费用降低 90%。
    
    -   **数据分片：** 2025-2026 年重点，目标实现主网作为“数据可用性层”支撑大规模扩容。
        

* * *

### 四、 以太坊生态概览：L1、L2、Sidechains 等

-   **Layer 1 (L1)：** 以太坊主网，底层安全与结算的基石。
    
-   **Layer 2 (L2)：** 扩容主力（如 Arbitrum, Optimism, zkSync）。
    
    -   **Rollup 原理：** 链下批量处理，链上打包验证。
        
    -   **ZK-Rollup：** 通过“零知识证明”实现高性能验证，被视为终极扩容方案。
        
-   **应用层：** 涵盖 DeFi（金融）、NFT（艺术）、DAO（治理）等。
    

* * *

### 五、 以太坊文化与价值观

以太坊的灵魂源于**密码朋克运动**，核心信念包括：

1.  **去中心化治理：** 社区驱动（EIP 机制），防止权力集中。
    
2.  **无需许可：** 任何人、任何时间、任何地点均可平等接入。
    
3.  **抗审查性：** 代码运行不受第三方干预。
    
4.  **公共物品：** 强调开源文化，注重生态系统的长期可持续性。
    

* * *

### 六、 以太坊核心机制：从账户到执行的完整链路

1.  **账户系统：**
    
    -   **EOA（外部账户）：** 私钥控制，用户交互入口。
        
    -   **CA（合约账户）：** 代码控制，实现自动化逻辑。
        
2.  **Gas 模型：**
    
    -   **逻辑：** $总费用 = Gas Limit \\times Gas Price$。
        
    -   **EIP-1559：** 基础费（Base Fee）直接**销毁**，小费（Tip）给验证者。
        
3.  **EVM（以太坊虚拟机）：** 全球统一的执行引擎，隔离运行合约，确保全网计算结果一致。
    

* * *

### 七、 一般流程与类比

-   **交互流程：** 用户 (EOA) 发起签名 → 支付 Gas → 触发合约 (CA) → EVM 执行 → 状态更新。
    
-   **形象类比：** \* **EOA** = 个人银行卡。
    
    -   **CA** = 自动售货机（投币/满足条件即出货）。
        
    -   **Gas** = 汽车汽油。
        
    -   **EVM** = 能够运行所有软件的通用处理器。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

**Web3**

1.12

**区块链基础概念**

今天学习了Web3实习手册里的区块链的基础概念

**一、区块链的特性：**

不可篡改、公开透明、匿名、快速交易

![blockchian-2ApLDSdO.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Lizze-lab/images/2026-01-12-1768207901683-blockchian-2ApLDSdO.jpg)

如何确保交易记录没被篡改切可信----使用分布式网络，节点记账（矿机）

特性：

1.**去中心化**

区块链网络通常分布在全球，每个节点都将会存储一份相同的区块链数据。没有人能够控制全部的节点，因此这份区块链数据将会一直存在。

2.不可篡改

**二、比特币**

是给 提供去中心化和分布式网络供应商的代币奖励！

![reward-ClDPYnzC.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Lizze-lab/images/2026-01-12-1768207929366-reward-ClDPYnzC.jpg)

益处：具备区块链同等特性、价格由供需决定

缺点：

难以追踪和限制、易被黑产利用-洗钱。

打包区块花费10min，影响交易实时醒

**三、区块链核心组成部分**

**1.去中心化的网络和区块链**

![decentralize-CO_v5XlA.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Lizze-lab/images/2026-01-12-1768207937261-decentralize-CO_v5XlA.jpg)

2.维持网络运行的代币激励

挖矿：节点维持网络运行的操作系统

矿工：维持这些服务的人

矿工的“工资”：代币（Gas Fee）

区块链如何运行？

用户发起交易->交易广播（交易信息广播到各个节点）->节点验证（矿工检验交易合法性、检查余额、签名）->打包成块->链接上链->奖励发放（矿工获得代币奖励和交易手续费）

![区块链技术和功能.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Lizze-lab/images/2026-01-12-1768207947499-________.jpg)

**四、**[**公链 vs 私链 vs 联盟链**](https://web3intern.xyz/zh/blockchain-basic/#%E5%9B%9B%E3%80%81%E5%85%AC%E9%93%BE-vs-%E7%A7%81%E9%93%BE-vs-%E8%81%94%E7%9B%9F%E9%93%BE)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Lizze-lab/images/2026-01-12-1768207967186-image.png)

**五、**[**Web3 vs Web 3.0 vs Web2 的范式革命**](https://web3intern.xyz/zh/blockchain-basic/#%E5%9B%9B%E3%80%81web3-vs-web-3-0-vs-web2-%E7%9A%84%E8%8C%83%E5%BC%8F%E9%9D%A9%E5%91%BD)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Lizze-lab/images/2026-01-12-1768207979209-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Lizze-lab/images/2026-01-12-1768207989993-image.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
