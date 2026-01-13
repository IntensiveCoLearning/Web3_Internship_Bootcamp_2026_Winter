---
timezone: UTC+8
---

# ChiWon

**GitHub ID:** CHI-WON

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
# **DAY3**

## TASK:

回顾Solidity基础语法
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

# DAY2

## TASK:学习Hardhat3-Tutorial

### 什么是HardHat3

Hardhat 3 是一个面向以太坊开发者的智能合约开发环境，它帮助开发、编译、测试、调试和部署 Solidity 合约，并支持编写脚本与测试工程。它扩展了之前的 Hardhat 生态，引入更现代化的工具链和默认配置

### Setting up the environment

-   安装Node.js v22或更高版本
    
-   安装包管理器：npm pnpm或yarn
    
-   选择一个编译器并安装Hardhat插件
    

### Creating a new Hardhat project

1.  **create an empty directory and initialize a Hardhat project**
    

```
mkdir hardhat-example
cd hardhat-example
npx hardhat --init
```

2.  **verify**
    

```
npx hardhat --help
```

### Write and Compile a contract

```
pragma solidity ^0.8.28;

contract Counter {
  uint public x;

  function inc() public { x++; }
}
```

compile:

```
npx hardhat build
```

or

```
npx hardhat compile
```

### Test

1.  **Solidity Test**
    

写.t.sol的Solidity测试文件，在EVM内部直接运行逻辑

```
npx hardhat test
```

仅运行Solidity：

```
npx hardhat test solidity
```

2.  **Javascript/Typescript Test**
    

在Node.js运行环境中进行，更贴近客户端调用合约的模式

运行Typescript测试：

```
npx hardhat test nodejs
```

### Ignition

```
npx hardhat ignition deploy ignition/modules/Counter.ts
```
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



# **DAY1**

## **今日任务：**

-   学习创建钱包、完成测试网转账
    
-   铸造第一个NFT
    

### 钱包知识：

1.  是钱包：
    

区块链上的钱包与传统钱包不同，区块链上的钱包是对持有资产的证明，而真正的资产永远存储在链上，钱包是管理私钥的工具。

2.  公钥->私钥->地址：
    

-   私钥是随机生成的，管理资产的所有权
    
-   公钥饰根据私钥推导出来的，用于签名验证
    
-   地址是公钥再哈希出来的，对外展示和收款用到
    

3.  助记词：
    

-   钱包一般是一组英文单词
    
-   是私钥的一种可读表达
    
-   可以用来恢复钱包
    

4.  钱包的功能：
    

-   生成私钥和保管私钥
    
-   对交易进行签名
    
-   把签名后的交易上传的区块链网络
    

### 钱包的类型：

1.  按私钥的控制权划分：
    

-   托管钱包：交易所的钱包属于托管钱包，私钥由第三方的交易所保管，使用的门槛较低，忘记密码时可以找回，但实际的资产控制权在交易所手上。
    
-   非托管钱包：私钥由个人保管，自己对资产有绝对的控制，但一旦丢失私钥就丢失了所有资产
    

2.  按存在形式划分：
    

-   软件钱包：运行在手机和电脑上的钱包统称软件钱包
    
-   硬件钱包：私钥存储在离线的物理设备上
    

### 转账交易：

1.  一个钱包可以创建多个账户
    

2.  对链的选择：
    

-   钱包中可以显示在不同区块链上的资产，在进行转账时要选对对应的区块链
    
-   在练习是要使用测试网，防止转移了真实的资产
    

3.  转账的记录可以在区块链浏览器上查看
    

### NFT:

1.  什么是NFT
    

NFT的全称是Non-Fungible Token，即非同质化代币，本质是一种链上“唯一标识+所有权记录”。

-   非同质化：每个NFT都不完全等于另一个
    
-   链上确权：所有权、转移记录都记录在区块链上，无法随意篡改
    
-   元数据指向：NFT本身通常不存图片，而是指向一个资源，如IPFS、中心化服务器
    

2.  技术标准：
    

-   ERC-721:每个tokenid唯一
    
-   ERC-1155:半同质化，常用于游戏资产、门票
    

3.  NFT包含数据：
    

-   tokenid：唯一编号
    
-   owner：持有人
    
-   metadata:指向json文件
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
