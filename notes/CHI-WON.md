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

## 完成情况：

1.  **短路原则**：回顾了一下Bool型的运算符，主要关注了**短路原则**。当逻辑与（&&）的第一个条件为false时，就不会再去判断第二个条件；当逻辑或（｜｜）的第一个条件为false时，就不会再去判断第二个条件。
    
2.  **数据位置和gas：**回顾了Solidity数据存储的三种类型：storage，memory和calldata。不同存储位置的gas成本不同。
    

-   storage:合约里的状态变量默认都是storage，存储在链上，消耗的gas多
    
-   memory：函数里的参数和临时变量一般用，存储在内存中，不上链。尤其是如果返回数据类型是变长的情况下，必须加memory修饰。如：string ,bytes,array和自定义结构
    
-   calldata：和memory类似，存储在内存，不上链。不同在于calldata变量不能修改。
    
-   整体gas消耗： storage >memory >calldata
    

3.  **数据位置和赋值规则：**在不同的存储类型相互赋值时，有时会产生独立的副本（修改新的变量不会影响原变量），有时会产生引用（修改新变量会影响原变量）
    

赋值本质上是创建引用指向本体，因此修改本体或是引用，变化会被同步。eg：合约里的storage赋值给本地（函数里的）

storage时，会创建引用，改变新变量会影响原变量。

-   👉 **storage ↔ storage**：产生「引用」
    
-   👉 **storage → memory**：产生「独立副本」
    
-   👉 **memory ↔ memory**：产生「独立副本」
    
-   👉 **calldata → memory**：产生「独立副本」
    
-   👉 **calldata → storage**：产生「拷贝写入」
    
-   🚫 **calldata 不能被引用修改**
    

4.  **全局变量：**全局范围工作的变量，都是预留关键字，在函数内可以不声明直接使用。
    

一些常用的全局变量：

-   `blockhash(uint blockNumber)`: (`bytes32`) 给定区块的哈希值 – 只适用于最近的256个区块, 不包含当前区块。
    
-   `block.coinbase`: (`address payable`) 当前区块矿工的地址
    
-   `block.gaslimit`: (`uint`) 当前区块的gaslimit
    
-   `block.number`: (`uint`) 当前区块的number
    
-   `block.timestamp`: (`uint`) 当前区块的时间戳，为unix纪元以来的秒
    
-   `gasleft()`: (`uint256`) 剩余 gas
    
-   `msg.data`: (`bytes calldata`) 完整call data
    
-   `msg.sender`: (`address payable`) 消息发送者 (当前 caller)
    
-   `msg.sig`: (`bytes4`) calldata的前四个字节 (function identifier)
    
-   `msg.value`: (`uint`) 当前交易发送的 `wei` 值
    
-   `block.blobbasefee`: (`uint`) 当前区块的blob基础费用。这是Cancun升级新增的全局变量。
    
-   `blobhash(uint index)`: (`bytes32`) 返回跟当前交易关联的第 `index` 个blob的版本化哈希（第一个字节为版本号，当前为`0x01`，后面接KZG承诺的SHA256哈希的最后31个字节）。若当前交易不包含blob，则返回空字节。这是Cancun升级新增的全局变量。
    

5.  **数组：**用来存储一组数据，分为固定和可变数组
    

-   固定长度数组：用T\[k\]的格式声，T是元素的类型，k是长度
    

```
uint[8] array1
btyes1[5] array2
address[100] arrays3
```

-   可变长度数组：声明时不指定长度，用T\[\]格式
    

```
uint[] array4;
bytes1[] array5;
address[] array6;
bytes array7;
```

6.  **Mapping**
    

```
mapping(uint => address)
```

主要回顾了一下mapping的规则：

-   keytype不能使用自定义的结构体，但是valuetype可以使用自定义的类型
    
-   映射的存储位置必须是`storage`
    
-   如果映射声明为`public`，那么Solidity会自动给你创建一个`getter`函数，可以通过`Key`来查询对应的`Value`
    

7.  **构造函数：**构造函数（`constructor`）是一种特殊的函数，每个合约可以定义一个，并在部署合约的时候自动运行一次。它可以用来初始化合约的一些参数
    
8.  **修饰器：**  
    修饰器（`modifier`）是`Solidity`特有的语法，类似于面向对象编程中的装饰器（`decorator`），声明函数拥有的特性，并减少代码冗余。它就像钢铁侠的智能盔甲，穿上它的函数会带有某些特定的行为。`modifier`的主要使用场景是运行函数前的检查，例如地址，变量，余额等。
    

```
// 定义modifier
modifier onlyOwner {
   require(msg.sender == owner); // 检查调用者是否为owner地址
   _; // 如果是的话，继续运行函数主体；否则报错并revert交易
}

function changeOwner(address _newOwner) external onlyOwner{
   owner = _newOwner; // 只有owner地址运行这个函数，并改变owner
}
```

9.  **事件：**`Solidity`中的事件（`event`）是`EVM`上日志的抽象，它具有两个特点：
    
    -   响应：应用程序（`ethers.js`）可以通过`RPC`接口订阅和监听这些事件，并在前端做响应。
        
    -   经济：事件是`EVM`上比较经济的存储数据的方式，每个大概消耗2,000 `gas`；相比之下，链上存储一个新变量至少需要20,000 `gas`。
        

**声明事件**

声明用event关键字开头，接事件名称，括号里时需要记录的变量类型和变量名

```
event Transfer(address indexed from, address indexed to, uint256 value);
```

最主要的是**EVM日志log:**evm用日志存储solidity事件，每条日志记录都包含主题`topics`和数据`data`两部分。主题长度不能超过4，第一个元素是事件的签名（哈希）。

10.  **继承：**继承是面向对象编程很重要的组成部分，可以显著减少重复代码。如果把合约看作是对象的话，`Solidity`也是面向对象的编程，也支持继承。
     
     ### **规则**
     
     -   `virtual`: 父合约中的函数，如果希望子合约重写，需要加上`virtual`关键字。
         
     -   `override`：子合约重写了父合约中的函数，需要加上`override`关键字。
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
