---
timezone: UTC+8
---

# Han Schurer

**GitHub ID:** hanschurer

**Telegram:** @hymn626

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
### 以太坊核心概念

-   以太坊定位：基于区块链的去中心化计算平台，区别于比特币的纯数字货币属性，支持智能合约与去中心化应用（DApp）开发。
    
-   关键特性：
    
    -   智能合约：运行在以太坊区块链上的自动化合约，无需第三方干预即可执行约定逻辑。
        
    -   以太币（ETH）：以太坊网络的原生代币，用于支付 Gas 费、转账等。
        
    -   Gas 机制：保障以太坊网络安全和高效运行的计费方式，防止恶意合约无限消耗资源。
        

### 2.2 技术架构（视频 / 文档重点拆解部分）

| 层级 | 核心功能 | 关键技术 |
| --- | --- | --- |
| 应用层 | DApp、钱包、区块链浏览器 | 智能合约开发（Solidity） |
| 合约层 | 智能合约执行环境 | EVM（以太坊虚拟机） |
| 网络层 | 节点通信、数据同步 | P2P 网络、RLP 编码 |
| 数据层 | 区块存储、账户管理 | 默克尔树、账户模型（外部账户 / 合约账户） |

### 实操相关

1.  以太坊节点部署步骤：
    
    bash
    
    运行
    
    ```
    # 示例：安装geth客户端（以Ubuntu为例）
    sudo add-apt-repository -y ppa:ethereum/ethereum
    sudo apt-get update
    sudo apt-get install ethereum
    # 启动测试网节点
    geth --goerli
    ```
    
2.  智能合约编写与部署要点：
    
    -   开发工具：Remix、Truffle、Hardhat 等。
        
    -   部署流程：编写合约→编译→部署到测试网 / 主网→验证合约。
        

## 重点总结

1.  以太坊的核心价值在于提供了去中心化的计算平台，通过 EVM 和智能合约实现了丰富的去中心化应用落地。
    
2.  Gas 机制是理解以太坊运行成本和资源调度的关键，开发和使用 DApp 需重点关注 Gas 优化。
    
3.  以太坊的账户模型、P2P 网络、EVM 等技术组件共同构成了其完整的生态体系，是学习和开发的核心基础。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

今天学习了下整体的流程 以及一些工具的下载安装
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
