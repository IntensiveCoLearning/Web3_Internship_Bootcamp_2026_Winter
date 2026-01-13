---
timezone: UTC+8
---

# yuyan0h

**GitHub ID:** yuyan0h

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
\# Web3 实习手册学习笔记

\## 📘 入门导读

\- Web3 实习手册的初步引导部分

\- 旨在帮助实习生快速进入 Web3 领域的学习和实践

\## 🧱 区块链基础概念

\- 区块链的核心原理与数据结构

\- 去中心化、分布式账本、共识机制等基本概念

\- 区块、哈希、加密算法等基础技术要素

\## 🌐 以太坊概览

\- 以太坊作为智能合约平台的基本介绍

\- EVM（以太坊虚拟机）与智能合约

\- 以太坊生态系统与常用工具（如 MetaMask、Remix 等）

\## 🧭 行业赛道全览

\- Web3 行业的主要赛道概览

\- 可能包括 DeFi、NFT、DAO、GameFi、SocialFi、Layer2、跨链等

\- 各个赛道的基本特征与发展现状

\## 💼 Web3 工作方式

\- Web3 团队的组织结构与协作方式

\- 远程办公、分布式团队、DAO 治理等新型工作模式

\- 常用工具与平台（Discord、Telegram、Notion、Snapshot 等）
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

# Done list

1.  创建第一个钱包
    
2.  实习手册chap1、2阅读
    
3.  参与线上分享会
    
4.  规划四周流程和学习内容
    

# Note：规划流程

### **🗓️ Web3 全栈 4 周冲刺学习规划**

**Week 1: 合约底层与 Solidity 深度进阶**

**核心目标：** 彻底搞懂合约如何在以太坊上运行，写出健壮的合约逻辑。

-   **重点学习：**
    
    -   **Solidity 高阶：** 继承、接口（Interface）、抽象合约、库（Library）、错误处理（Revert vs Require）。
        
    -   **数据结构优化：** 理解 Storage vs Memory vs Calldata 对 Gas 的影响。
        
    -   **标准协议：** 深入研读 **ERC-20** 和 **ERC-721** 代码（OpenZeppelin 库）。
        
-   **实战任务：**
    
    -   在 Remix 中手写一个带“铸造上限”和“管理员权限”的 ERC-20 代币。
        
    -   通关 _CryptoZombies_ 前 4 章，确保对函数权限修饰符了如指掌。
        
-   **本周产出：** 部署并验证（Etherscan）一个属于你自己的代币合约。
    

**Week 2: 工程化开发环境 (Foundry/Hardhat)**

**核心目标：** 告别网页 IDE，进入专业开发流程。

-   **重点学习：**
    
    -   **Foundry 入门（推荐）：** 安装 forge, cast, anvil。学习用 Solidity 写测试用例（比 JS 效率更高）。
        
    -   **Hardhat 复选：** 如果你 JS 基础好，学习脚本化部署（Scripts）和 Ethers.js 插件。
        
    -   **环境变量：** 学习使用 .env 管理私钥和 API Key（安全意识第一步）。
        
-   **实战任务：**
    
    -   使用 Foundry 初始化项目，编写测试用例达到 90% 以上的路径覆盖。
        
    -   学习如何 Fork 主网环境进行本地调试。
        
-   **本周产出：** 一个拥有完整测试套件的 GitHub 仓库。
    

**Week 3: 前端交互与 Web3 胶水层**

**核心目标：** 让合约“动起来”，实现网页与钱包的连接。

-   **重点学习：**
    
    -   **React/Next.js 基础：** Web3 前端的主流框架。
        
    -   **Wagmi & Viem：** 目前最现代的 Web3 Hooks 库，用于连接钱包、读取合约数据、发送交易。
        
    -   **ConnectKit/RainbowKit：** 快速集成“连接钱包”按钮。
        
-   **实战任务：**
    
    -   搭建一个简单的 Next.js 页面。
        
    -   实现功能：点击按钮连接 Metamask -> 显示当前账户余额 -> 调用 Week 1 写的合约函数（如 Mint）。
        
-   **本周产出：** 一个能实时读取链上数据并触发钱包签名的前端 Demo。
    

**Week 4: 综合实战与作品集交付**

**核心目标：** 独立完成一个闭环项目，模拟真实岗位需求。

-   **项目建议（二选一）：**
    
    -   **Mini NFT Marketplace：** 用户可以上传图片（使用 IPFS/Pinata）并 Mint 成 NFT，支持在页面上查看。
        
    -   **Simple DeFi Vault：** 一个简单的质押池，用户存入 ERC-20 代币，根据时间获取收益（Reward）。
        
-   **核心任务：**
    
    -   **集成：** 合约代码 + 部署脚本 + 前端 UI。
        
    -   **部署：** 合约部署至 Sepolia 测试网，前端部署至 Vercel。
        
    -   **文档：** 编写 [README.md](http://README.md)，包含项目截图、合约地址和运行步骤。
        
-   **本周成就：** 在简历上填入第一个**生产级 Web3 项目链接**。
    

* * *
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
