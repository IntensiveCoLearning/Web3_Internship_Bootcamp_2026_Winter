---
timezone: UTC+8
---

# 朱宇轩

**GitHub ID:** Yuxuancangsu

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->
CoinFlip 关卡笔记

一、关卡目标

连续猜对 10 次抛硬币结果

本质不是运气，而是利用伪随机数漏洞。

二、合约核心代码解析

复制代码

Solidity

uint256 public consecutiveWins;

uint256 lastHash;

uint256 FACTOR =

57896044618658097711785492504343953926634992332820282019728792003956564819968;

关键逻辑：

复制代码

Solidity

uint256 blockValue = uint256(blockhash(block.number - 1));

if (lastHash == blockValue) {

revert();

}

lastHash = blockValue;

uint256 coinFlip = blockValue / FACTOR;

bool side = coinFlip == 1 ? true : false;

三、随机数是如何“被预测”的？

1️⃣ “随机数”来源

复制代码

blockhash(block.number - 1)

特性

说明

是公开数据

链上所有合约都能读取

是确定值

每个节点结果一致

不是随机

攻击者可提前计算

这属于：

链上可预测随机数漏洞

2️⃣ 为什么除以 FACTOR？

FACTOR = 2²⁵⁵

因为：

复制代码

blockhash 是 256 位数

/ 2²⁵⁵ 只可能得到 0 或 1

等价于：

复制代码

取最高位 bit

3️⃣ 关键漏洞点

攻击者在同一笔交易中可以做：

复制代码

Solidity

uint256 blockValue = uint256(blockhash(block.number - 1));

uint256 coinFlip = blockValue / FACTOR;

bool guess = coinFlip == 1;

然后把 guess 传入 flip()，100% 正确。

四、防止“连续点击”的机制

复制代码

Solidity

if (lastHash == blockValue) revert();

含义：

同一块内不能调用两次

因为同一区块的 blockhash(block.number-1) 不变。

所以攻击必须是：

每个新区块调用一次

五、攻击合约逻辑

复制代码

Solidity

function attack() public {

uint256 blockValue = uint256(blockhash(block.number - 1));

require(blockValue != lastHash, "Wait next block");

lastHash = blockValue;

uint256 coinFlip = blockValue / FACTOR;

bool guess = (coinFlip == 1);

target.flip(guess);

}

六、通关流程总结

步骤

操作

1

获取关卡实例地址

2

部署攻击合约

3

每个区块调用一次 attack()

4

连续成功 10 次

5

提交关卡

七、失败原因对照表

现象

原因

revert

同区块调用两次

连胜清零

有一次猜错（可能手动调用）

一直是 0

攻击合约没成功调用目标

八、本关考察的安全知识

这是区块链安全里的经典漏洞类型：

❌ 不安全随机数来源

block.timestamp

block.number

blockhash

msg.sender

这些都能被操控或预测。

✅ 安全做法

真实项目需要：

方法

说明

Chainlink VRF

可验证随机数

Commit-Reveal

两阶段防预测

Off-chain randomness

预言机提供

九、本关核心一句话总结

区块变量不是随机数，只要算法可复制，攻击者就能 100% 预测结果。
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->

### 1\. 学习笔记

[Gas\_Optimization\_Learning\_](Gas_Optimization_Learning_Notes.md)[Notes.md](http://Notes.md) 是本项目的核心文档，包含：

•Gas 基础概念与机制

•核心优化技巧详解

•代码案例对比

•Gas 节省效果量化分析

•最佳实践建议

### 2\. 合约代码

TokenUnoptimized.sol - 未优化版本

包含以下常见问题：

•多次读取 storage 变量

•使用 public 而非 external

•循环中重复读取 array.length

•未使用位压缩

•未缓存 storage 变量到 memory

TokenOptimized.sol - 优化版本

应用了以下优化技巧：

•✅ 缓存 storage 变量到 memory

•✅ 使用位压缩（Bit Packing）

•✅ 使用 external 替代 public

•✅ 使用 calldata 替代 memory

•✅ 循环优化（缓存 length，使用 ++i）

•✅ 使用 unchecked 块

•✅ 使用 constant 关键字

•✅ 短路求值优化

### 3\. Gas 优化效果

| 操作 | 未优化 Gas | 优化后 Gas | 节省 | 改善率 |
| 单笔转账 | ~51,000 | ~46,000 | ~5,000 | ~10% |
| 批量转账(5个) | ~180,000 | ~120,000 | ~60,000 | ~33% |
| 授权 | ~46,000 | ~44,000 | ~2,000 | ~4% |
| 授权转账 | ~55,000 | ~49,000 | ~6,000 | ~11% |

## 如何使用

### 方法 1：使用 Remix IDE（推荐初学者）

1.访问 [Remix IDE](https://remix.ethereum.org/)

2.创建新文件并复制 TokenUnoptimized.sol 和 TokenOptimized.sol

3.编译合约（Solidity 版本 ^0.8.0）

4.在 JavaScript VM 环境下部署

5.调用函数并查看 Gas 消耗

### 方法 2：使用 Foundry（推荐专业开发者）

Bash

\# 安装 Foundry curl -L [https://foundry.paradigm.xyz](https://foundry.paradigm.xyz) | bash foundryup # 初始化项目 forge init gas-optimization-project cd gas-optimization-project # 将合约文件复制到 src 目录 cp TokenUnoptimized.sol src/ cp TokenOptimized.sol src/ # 将测试文件复制到 test 目录 cp GasComparisonTest.sol test/ # 运行测试并生成 Gas 报告 forge test --gas-report # 查看详细输出 forge test --gas-report -vvv

### 方法 3：使用 Hardhat

Bash

\# 初始化项目 npm init -y npm install --save-dev hardhat npm install --save-dev hardhat-gas-reporter # 配置 hardhat.config.js # 添加 gasReporter 配置 # 运行测试 npx hardhat test
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->


# 📘 CoinFlip 关卡笔记

## 一、关卡目标

> 连续猜对 10 次抛硬币结果

本质不是运气，而是**利用伪随机数漏洞**。

* * *

## 二、合约核心代码解析

```
uint256 public consecutiveWins;
uint256 lastHash;

uint256 FACTOR =
57896044618658097711785492504343953926634992332820282019728792003956564819968;
```

### 关键逻辑：

```
uint256 blockValue = uint256(blockhash(block.number - 1));

if (lastHash == blockValue) {
    revert();
}

lastHash = blockValue;

uint256 coinFlip = blockValue / FACTOR;
bool side = coinFlip == 1 ? true : false;
```

* * *

## 三、随机数是如何“被预测”的？

### 1️⃣ “随机数”来源

```
blockhash(block.number - 1)
```

| 特性 | 说明 |
| --- | --- |
| 是公开数据 | 链上所有合约都能读取 |
| 是确定值 | 每个节点结果一致 |
| 不是随机 | 攻击者可提前计算 |

这属于：

> **链上可预测随机数漏洞**

* * *

### 2️⃣ 为什么除以 FACTOR？

FACTOR = 2²⁵⁵

因为：

```
blockhash 是 256 位数
/ 2²⁵⁵ 只可能得到 0 或 1
```

等价于：

```
取最高位 bit
```

* * *

### 3️⃣ 关键漏洞点

攻击者在**同一笔交易**中可以做：

```
uint256 blockValue = uint256(blockhash(block.number - 1));
uint256 coinFlip = blockValue / FACTOR;
bool guess = coinFlip == 1;
```

然后把 `guess` 传入 `flip()`，**100% 正确**。

* * *

## 四、防止“连续点击”的机制

```
if (lastHash == blockValue) revert();
```

含义：

> 同一块内不能调用两次

因为同一区块的 `blockhash(block.number-1)` 不变。

所以攻击必须是：

> **每个新区块调用一次**

* * *

## 五、攻击合约逻辑

```
function attack() public {
    uint256 blockValue = uint256(blockhash(block.number - 1));

    require(blockValue != lastHash, "Wait next block");
    lastHash = blockValue;

    uint256 coinFlip = blockValue / FACTOR;
    bool guess = (coinFlip == 1);

    target.flip(guess);
}
```

* * *

## 六、通关流程总结

| 步骤 | 操作 |
| --- | --- |
| 1 | 获取关卡实例地址 |
| 2 | 部署攻击合约 |
| 3 | 每个区块调用一次 attack() |
| 4 | 连续成功 10 次 |
| 5 | 提交关卡 |

* * *

## 七、失败原因对照表

| 现象 | 原因 |
| --- | --- |
| revert | 同区块调用两次 |
| 连胜清零 | 有一次猜错（可能手动调用） |
| 一直是 0 | 攻击合约没成功调用目标 |

* * *

## 八、本关考察的安全知识

这是区块链安全里的经典漏洞类型：

### ❌ 不安全随机数来源

-   `block.timestamp`
    
-   `block.number`
    
-   `blockhash`
    
-   `msg.sender`
    

这些都能被操控或预测。

* * *

### ✅ 安全做法

真实项目需要：

| 方法 | 说明 |
| --- | --- |
| Chainlink VRF | 可验证随机数 |
| Commit-Reveal | 两阶段防预测 |
| Off-chain randomness | 预言机提供 |

* * *

## 九、本关核心一句话总结

> **区块变量不是随机数，只要算法可复制，攻击者就能 100% 预测结果。**
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->




# Ethernaut · Fallback（极简笔记）

## 目标

1.  成为 `owner`
    
2.  合约余额 = 0
    

* * *

## 关键漏洞

```
receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
}
```

👉 **无 calldata 转账 + 已贡献过 = 直接当 owner**

* * *

## 核心概念

-   `receive()`
    
    -   只在 **data 为空 + 转 ETH** 时触发
        
-   `contributions`
    
    -   只要 **\> 0** 就够（1 wei 即可）
        
-   `owner`
    
    -   在 `receive()` 中被错误修改
        

* * *

## 攻击步骤（4 步）

### 1️⃣ contribute（ABI 交互）

```
contribute()
value = 1 wei   （< 0.001 ether）
```

### 2️⃣ 纯转账（ABI 之外）

```
to    = 合约地址
value = 1 wei
data  = 空
```

➡️ 触发 `receive()`，抢 owner

### 3️⃣ 验证

```
owner() == 我的地址
```

### 4️⃣ 提现

```
withdraw()
```

➡️ 合约余额 = 0

* * *

## 必踩雷区（记住这 3 个）

❌ 转账 value = 0  
❌ CALLDATA 不是空  
❌ 两步用的不是同一个地址

* * *

## 单位速记

```
1 ether = 10^18 wei
0.001 ether = 10^15 wei
```

* * *

## 一句话总结

> 先贡献 1 wei，  
> 再无 calldata 转 1 wei，  
> 抢 owner，提余额。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->





# DApp (去中心化应用) 学习笔记

## 1\. DApp 基础概念

DApp (Decentralized Application) 是一种运行在去中心化网络（如区块链）上的应用程序。与传统的中心化应用不同，DApp 的后端逻辑由智能合约驱动，而不是由中心化服务器控制。这使得 DApp 具有更高的透明度、安全性和抗审查性。

### 1.1 DApp 的核心特征

•开源 (Open Source): DApp 的源代码通常是公开的，任何人都可以审查、使用和修改。

•去中心化 (Decentralized): 数据和状态存储在公共区块链上，由网络中的多个节点共同维护。

•加密安全 (Cryptographically Secure): 所有数据都通过加密技术进行保护，确保其完整性和安全性。

•代币化系统 (Tokenized System): DApp 通常使用加密代币作为其内部经济系统的激励机制。

### 1.2 DApp 与传统应用的对比

| 特性 | 传统应用 | DApp |
| 数据存储 | 中心化服务器 | 分布式区块链 |
| 控制权 | 公司或机构 | 用户自主控制 |
| 透明度 | 代码不公开 | 开源透明 |
| 单点故障 | 存在 | 不存在 |
| 用户数据 | 需提供个人信息 | 可匿名使用钱包 |

## 2\. DApp 技术架构

DApp 的技术架构通常分为三层：

1.区块链层 (Blockchain Layer): 这是 DApp 的基础，提供了去中心化的数据存储和计算能力。常见的区块链平台包括以太坊 (Ethereum)、币安智能链 (BSC)、Solana 等。

2.智能合约层 (Smart Contract Layer): 这是 DApp 的核心，包含了应用的业务逻辑。智能合约是用 Solidity、Vyper 或 Rust 等语言编写的，并部署在区块链上。

3.前端层 (Frontend Layer): 这是用户与 DApp 交互的界面。前端通常使用 React、Vue.js 等框架开发，并通过 Web3.js 或 Ethers.js 等库与智能合约进行交互。

## 3\. DApp 开发技术栈

### 3.1 智能合约开发

•编程语言:

•Solidity: 最流行的智能合约语言，用于以太坊及兼容链。

•Vyper: Python 风格的智能合约语言。

•Rust: 用于 Solana、Polkadot 等高性能区块链。

•开发框架:

•Hardhat: 现代化的以太坊开发框架，功能强大。

•Truffle: 老牌开发框架，生态成熟。

•Foundry: 基于 Rust 的快速开发工具。

### 3.2 前端开发

•前端框架: React, Vue.js, Angular

•Web3 库:

•Ethers.js: 轻量级、现代化的以太坊库，推荐新项目使用。

•Web3.js: 传统的以太坊库，社区庞大。

•钱包集成: MetaMask, WalletConnect, Trust Wallet

### 3.3 后端与数据服务 (可选)

•节点服务: Infura, Alchemy, QuickNode

•数据索引: The Graph

•去中心化存储: IPFS, Arweave, Filecoin

## 4\. DApp 开发流程

1.需求分析与设计: 明确 DApp 的功能和目标用户。

2.智能合约开发: 编写、测试和部署智能合约。

3.前端开发: 开发用户界面，并与智能合约进行集成。

4.测试与审计: 在测试网上进行全面测试，并进行专业的安全审计。

5.部署与发布: 将智能合约部署到主网，并发布前端应用。

6.运营与维护: 监控 DApp 的运行状态，并根据社区反馈进行迭代。

## 5\. DApp 安全最佳实践

### 5.1 智能合约安全

•代码审计: 聘请专业的安全公司进行代码审计。

•使用经过验证的库: 如 OpenZeppelin 提供的安全合约库。

•遵循安全模式:

•Checks-Effects-Interactions: 先检查条件，再更新状态，最后与外部合约交互。

•Reentrancy Guard: 防止重入攻击。

•Pull Payment: 让用户主动提取资金，而不是合约主动推送。

•Gas 优化: 优化代码以降低交易成本。

### 5.2 前端安全

•输入验证: 验证所有用户输入，防止恶意攻击。

•交易确认: 向用户清晰地展示交易详情。

•不存储私钥: 永远不要在前端存储用户的私钥。

## 6\. 总结

DApp 作为 Web3 时代的核心应用形态，正在深刻地改变着互联网的格局。虽然 DApp 开发面临着性能、用户体验和安全等多方面的挑战，但其去中心化、透明和用户自主的特性，为构建一个更加开放、公平和可信的数字世界提供了可能。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->






### **Uniswap 综合研究笔记**

**核心摘要:** Uniswap 是以太坊区块链上最大的去中心化交易所（DEX）协议。它不依赖传统的订单簿模式，而是开创性地使用了**自动做市商（AMM）**模型，允许用户直接通过智能合约与流动性池进行点对点交易。其原生治理代币是 **UNI**。

**一、核心概念与运作机制**

1.  **去中心化交易所 (DEX):**
    
    -   与中心化交易所（如币安、Coinbase）不同，DEX 不持有用户资金或私钥。
        
    -   所有交易都在区块链上通过智能合约直接执行，用户始终保持对其资产的完全控制。
        
    -   这大大降低了因交易所被黑客攻击或内部问题导致资金损失的风险。
        
2.  **自动做市商 (AMM) 与流动性池:**
    
    -   **核心创新:** Uniswap 的核心是 AMM 模型，它取代了传统交易所的“订单簿”（Order Book）。
        
    -   **流动性池 (Liquidity Pool):**
        
        -   每个交易对（例如 ETH/DAI）都有一个对应的流动性池，池中锁定了这两种代币。
            
        -   任何人都可以成为**流动性提供者 (Liquidity Provider, LP)**，通过向池中按特定比例（通常是 50/50 的价值）存入两种代币来提供流动性。
            
    -   **定价机制:**
        
        -   池中代币的价格由一个恒定乘积公式 `x * y = k` 决定。
            
        -   `x` 和 `y` 分别代表池中两种代币的数量。
            
        -   `k` 是一个恒定值。当用户进行交易时（例如用 `x` 换 `y`），池中的 `x` 数量增加，`y` 数量减少，为了维持 `k` 的恒定，`y` 的价格会相对于 `x` 上升。
            
        -   **结果:** 交易量越大，对价格的影响（即**滑点**）就越大。
            
3.  **流动性提供者 (LP) 与收益:**
    
    -   LP 向池中注入资金后，会收到代表其份额的 **LP 代币**。
        
    -   当池中发生交易时，交易者需要支付一笔手续费（例如 0.3%）。
        
    -   这些手续费会按比例分配给所有 LP，作为他们提供流动性的回报。
        
    -   LP 可以随时销毁他们的 LP 代币，取回他们在池中的资产份额及累积的费用。
        
4.  **无常损失 (Impermanent Loss):**
    
    -   这是 LP 面临的主要风险。
        
    -   当池中代币的市场价格发生剧烈波动时，LP 从池中撤出资金时，其资产的总价值可能会低于他们最初简单持有这些代币的价值。
        
    -   这种损失是“无常的”，因为如果代币价格恢复到初始比例，损失就会消失。但如果 LP 在价格变动后撤出资金，损失就会变成永久性的。
        

**二、UNI 治理代币**

-   **发行:** UNI 代币于 2020 年 9 月推出，并向早期协议用户（包括交易者和 LP）进行了大规模的**空投 (Airdrop)**。
    
-   **功能:**
    
    -   **治理:** UNI 持有者可以对协议的未来发展进行投票，例如费用结构调整、新功能添加、资金库的使用等。
        
    -   **去中心化所有权:** UNI 的分发旨在将协议的控制权交给社区用户，实现真正的去中心化治理。
        

**三、主要版本迭代**

-   **Uniswap v1 (2018年11月):**
    
    -   最初版本，只支持 ETH 与单一 ERC-20 代币之间的兑换。
        
    -   验证了 AMM 模型的可行性。
        
-   **Uniswap v2 (2020年5月):**
    
    -   **重大改进:** 引入了任意 ERC-20 代币之间的直接兑换（不再需要 ETH 作为中介）。
        
    -   引入了**价格预言机 (Price Oracles)**，为其他 DeFi 应用提供了可靠的价格数据。
        
    -   引入了**闪电兑换 (Flash Swaps)**，允许用户在单次交易中“借出”池中资产，执行任意操作后立即归还，无需抵押。
        
-   **Uniswap v3 (2021年5月):**
    
    -   **核心创新 - 集中流动性 (Concentrated Liquidity):**
        
        -   允许 LP 将其资金集中在特定的价格区间内，而不是在从零到无穷大的整个价格曲线上提供流动性。
            
        -   **优势:** 资本效率大幅提高。LP 可以在更窄的价格范围内赚取更多的交易费。
            
        -   **劣势:** 管理更复杂，无常损失的风险也更集中。
            
    -   **多级费率:** 为不同波动性的交易对提供了多种手续费等级（例如 0.05%, 0.30%, 1.00%），让 LP 可以根据风险选择合适的费率。
        

**四、总结与展望**

Uniswap 不仅仅是一个交易平台，它更是去中心化金融（DeFi）领域的基石之一。它的 AMM 模型被无数项目效仿和改进，其开源和去中心化治理的理念也深刻影响了整个加密行业。随着以太坊生态系统的不断扩展和 Layer 2 解决方案的成熟，Uniswap 预计将继续在提升交易效率、降低成本和探索新金融模式方面发挥领导作用。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->







今天主要解决了以太坊中第一个链接不到IPFS的bug，然后跟随Elon老师学习了solidity语言的语法知识，主要学习了如何写一份属于自己的稳定币
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->








今天主要练习了以太坊第0个challenge，这个项目我从去年10月份开始就做但苦于没有社区去答疑，现在我遇到很多朋友能够一起解决这个IPFS失效的问题。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->









今天主要学习了ZK（零知识证明）：在不泄密任何有价值的信息来证明有用的信息从某种角度保证了隐私性和和可扩展性。从“开锁不泄密”说起：零知识的核心逻辑想象你有个带密码锁的日记本，想向朋友证明你知道密码，但绝不能说出来——直接开锁怕朋友记住密码，怎么办？零知识证明的操作堪称“心机”：让朋友写一张带特殊记号的纸条（比如画个歪歪扭扭的小太阳），放进日记本的锁抽屉；朋友锁好抽屉后，你背过身（不让他看操作），用密码打开抽屉，取出那张纸条。朋友看到你拿出了只有他能认出的纸条，立刻相信你知道密码——但他既没听到密码，也没看到你按了哪几个数字。这就是零知识证明的精髓：用一个“和秘密强关联的结果”反向证明秘密存在，却不泄露秘密本身。你证明了“我知道”，但没说“我知道什么”。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->











## Remix 是什么？

Remix 是一个零门槛、基于浏览器的智能合约集成开发环境（IDE）。它无需安装和配置，是 Web3 新手入门的首选工具。

## 核心功能区

作为新手，你只需要关注三个主要区域：

1.文件管理器 (File Explorers)：用于创建和管理你的 .sol 合约文件。

2.Solidity 编译器 (Solidity Compiler)：用于检查代码语法错误，并将 Solidity 代码编译成以太坊虚拟机（EVM）可以理解的字节码。

3.部署与运行 (Deploy & Run)：用于将编译好的合约部署到区块链（模拟或真实），并与其进行交互。

## 理解关键的按钮颜色

在 Remix 中，交互按钮的颜色直接反映了操作的性质和成本，理解这一点至关重要：

| 颜色 | 含义 | 链上行为 | Gas 费用 |
| 蓝色 | 查 (Read) | 只读数据，不修改状态 | 免费 |
| 橙色 | 改 (Write) | 写入数据，会改变链上状态 | 付费 |
| 红色 | 付 (Payable) | 写入数据且附带以太币转账 | 付费 |

## 基本开发流程

1.编写：在文件管理器中创建一个新文件（例如 Storage.sol），并编写你的智能合约代码。

2.编译：切换到编译器页面，确保已勾选 “Auto compile”，然后点击编译按钮，检查是否出现绿色对勾。

3.部署：切换到部署与运行页面，选择 Remix VM（一个快速的内存模拟区块链），点击 “Deploy”。

4.交互：在下方 “Deployed Contracts” 区域找到你的合约，使用不同颜色的按钮与其进行交互，测试功能。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->












Web3 新人学习方法论笔记这份笔记总结了“Web3牛马新人学习效果最大化方法论”PPT的核心观点，旨在帮助 Web3 初学者构建高效的学习闭环。核心理念：输入、思考、总结、输出、反馈学习的本质是建立一个完整的闭环，而不仅仅是知识的输入。为了最大化学习效果，关键在于跑通 输入 → 思考 → 总结 → 输出 → 反馈 的全过程。很多初学者停留在“有掌控感，但非真实掌握”的阶段，其根本原因在于缺少高质量、针对性的反馈。如何构建高效学习闭环？1. 构建知识框架与掌控感在学习初期，通过以下方式快速构建对新知识的自我框架：•梳理逻辑链条：使用思维导图等工具理清知识点之间的内在联系。•明确兴趣板块：找到自己最感兴趣的方向，进行深耕。•发散思维：不局限于单一信息源，进行关联性思考。•拥抱 AI 协作：利用 AI 工具辅助理解和整理，提高效率。2. 解决反馈缺失问题高质量的反馈是检验学习成果和驱动进步的关键。通过在公开渠道“输出”来主动获取反馈：•利用社交平台：在推特、小红书、朋友圈等平台分享你的学习笔记、思考和项目。这不仅能扩大反馈面，还能让你获得针对性的建议。•重视内容营销：在 Web3 领域，营销的影响力不亚于技术本身。通过持续输出，可以建立个人品牌，将内容转化为链接、资源，最终形成复利效应。3. 解决输出效率问题“先做成一坨屎，再去改”——执行力是第一位的。不要追求完美，先完成再优化。可以借助 AI 大幅提升输出效率：•AI 辅助创作：让 AI 帮助你进行笔记总结、绘制思维导图、甚至生成符合特定平台调性的初稿，能将工作量减少高达 90%。•培养个人风格 AI：通过给你偏好的内容风格“喂”给 AI，让它学习并模仿，从而高效产出符合你要求的高质量内容。总结1.总结复盘：结合思维导图和 AI 工具，对所学知识进行系统性复盘。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->













今天参加co-learning，我学到技术好与坏并不是入职web3的唯一标准，积极性和靠谱也关键，踏踏实实学习，认认真真做事应该就能参加到web3这个伟大的行业
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->














今天听了wwwach老师经历感慨万千总结一句话就是要做认为正确的事情要follow自己的内心并且坚定不移地去执行
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->















今天参加Co-learning主要学习了DAO的治理：独裁与民主不是关键，关键要有纠错的能力，参加了晚上Web3的构建主要学习了钱包和公私钥和助记词之间的关系，要保护好自己的钱包
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->
















今天我主要实践了建立自己的钱包，并且实施了第一次转账，主要学习了钱包的应用并且第一次在Binance上购买了以太坊，在晚上的课程主要了解了技术岗方面的知识和求职方法，要通过这一个月的时间来提升的技术能力。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
