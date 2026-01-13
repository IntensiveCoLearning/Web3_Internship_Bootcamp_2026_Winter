---
timezone: UTC+8
---

# Free-riding

**GitHub ID:** Free-riding

**Telegram:** @have_try

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
## L1和L2的相关知识
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

# 一.transfer测试币

## 1.领水地址：

[https://cloud.google.com/application/web3/faucet/ethereum/sepolia](https://cloud.google.com/application/web3/faucet/ethereum/sepolia) (可以直接领0.05)

[https://sepolia-faucet.pk910.de/#/](https://sepolia-faucet.pk910.de/#/) (可以自己挖)

## 2.在MetaMask APP中操作

在Meta Mask中在**主页->发送->点击SepoliaETH->输入金额0.1eth->继续->输入接收地址->点击确认**

## 3.在ethscan中查看转账信息

ethscan地址：[https://etherscan.io/](https://etherscan.io/)

在右上角点击以太坊图标，切换成Sepolia Testnent(或者直接访问[https://sepolia.etherscan.io/](https://sepolia.etherscan.io/))

搜索自己转账的地址，可以看到自己交易的具体信息（Transaction Hash，Status，Block等）

# 二.ETH学习

## 2.1区块链

区块链是一个网络中共享的公共数据库。数据被写入区块后便不能修改，每个区块记录了交易信息和上一个区块的哈希，保证了数据的不可篡改。若你修改了某一个区块的数据，需要修改后续所有区块的信息，并且要达到51%最低限度，才会被整个网络认可。

## 2.WEB3.0

web1.0 只读

web2.0 可读可写

web3.0 可读可写可拥有

web3.0用区块链技术实现去中心化，可以让用户拥有自己的数字资产，而不是掌握在平台手中，具有抗审查性。以此出现的去中心化自治组织DAO，让治理集中在每个社区的成员手中。

## 2.3以太坊

以太币（ETH）是以太坊的原生加密货币，使用ETH来支付连上操作产生的gas费用。目前的gas计算为：基础费（base fee）+小费(priority fee)。基础费由系统根据网络的拥堵情况，交互复杂程度等来决定，小费可以让验证者更倾向于打包你的交易。

gas计价单位为wei（纪念V神的贡献）是以太坊网络中的最小计价单位，**1 ETH = 10¹⁸ Wei**

## 2.4POS 权益证明

2022 年 9 月 15 日，**The Merge之后，**以太坊全面转入POS证明。

现在的以太坊实际上是两层结构：

-   **执行层**：处理交易、智能合约（原主网）
    
-   **共识层**：管理验证者、确定区块顺序（信标链）
    

整体的生态如下：、

### **1\. 应用层（Application Layer）**

用户直接交互的应用和界面：

-   **DeFi 应用**：Uniswap（去中心化交易所）、Aave（借贷协议）、Compound（借贷协议）
    
-   **NFT 平台**：OpenSea、Foundation、SuperRare
    
-   **钱包应用**：MetaMask、Coinbase Wallet、Rainbow
    
-   **DAO 工具**：Snapshot、Aragon、Colony
    

### **2\. 协议层（Protocol Layer）**

以太坊的核心基础设施：

-   **共识层客户端**：Prysm、Lighthouse、Nimbus、Teku
    
-   **执行层客户端**：Geth、Nethermind、Erigon、Besu
    
-   **核心协议**：EVM、状态管理、Gas 机制
    

### **3\. 扩展层（Scaling Layer）**

提升性能和降低成本的解决方案：

-   **Layer 2 Rollups**：Arbitrum、Optimism、Polygon zkEVM、zkSync Era
    
-   **侧链**：Polygon PoS、xDAI（Gnosis Chain）
    
-   **状态通道**：Lightning Network for Ethereum
    

## 2.5验证者，质押，联合质押

验证者存入 32 个以太币以激活验证者软件，运行将交易正确打包到新区块中并检查其他验证者工作的软件，以此来获得奖励，同时若是双重提议区块，double vote，surround vote等危害共识机制的行为会触发slashing惩罚，损失甚至是罚没质押的eth.

验证者的收益构成:

共识层奖励：

o 为链投票（attestation）、正确提议区块、参与同步委员会（sync committee）等都会获得奖励；

o 奖励以 ETH 形式在共识层记账。

• 执行层奖励：

o 包括用户支付的优先费（tip）、可能存在的 MEV 收入等；

o

直接打到你设置的执行层收益地址（fee recipient）。

• 额外激励（由协议或第三方提供）：

o 某些质押协议、LSD/LRT （流动性质押）协议会额外发代币或奖励；

o 某些场景下，对“举报 slashable 行为”的举报者（whistleblower）还会给额外 ETH 奖励。

### 联合质押：

质押池：激活一组验证者密钥需要 32 个以太币，而质押池这种协作方式使拥有少量以太币的人能够满足这一条件，相当于大家一起凑到目标金额来质押获得奖励，有些池甚至仅需0.01eth便能参与，一些资金池使用智能合约运行，资金会存入一个合约中，该合约以去信任的方式管理和跟踪你的质押，并向你发放代表该价值的代币。 其他资金池不涉及智能合约，而是在链下促成。

风险：将所有节点操作委托给第三方可能会带来额外的风险和费用
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
