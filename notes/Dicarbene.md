---
timezone: UTC+8
---

# dicarbene

**GitHub ID:** Dicarbene

**Telegram:** @antidoses

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
\## 1. Foundry

\- 以 **Rust** 实现的以太坊开发工具链，主打：\*\*编译快、测试快、脚本部署强\*\*。

\- 核心组件：

\- **forge**：编译、测试、安装依赖、构建项目

\- **cast**：命令行与链交互（查数据/发交易/调用合约）

\- **anvil**：本地 EVM 节点（类似本地链）

\- **chisel**：Solidity REPL（可选）

\---

\## 2. 安装与环境

\- 安装`foundryup`（安装后可升级）

\- 常用环境变量（放 `.env`）：

\- `RPC_URL=...`

\- `PRIVATE_KEY=...`（仅用于测试/脚本，注意保密）

\- `ETHERSCAN_API_KEY=...`（验证合约可用）

\---

\## 3. 创建项目与目录结构

\- 创建：

\- `forge init my-project`

\- 常见目录：

\- `src/` 合约源码

\- `test/` 测试代码（Solidity 测试为主）

\- `script/` 部署/交互脚本（Solidity Script）

\- `lib/` 依赖`forge install` 拉取）

\- `foundry.toml` 配置文件

\---

\## 4. Forge 常用命令

\- 编译`forge build`

\- 运行测试`forge test`

\- 显示更详细日志`forge test -vvv`

\- 指定测试合约/函数：

\- `forge test --match-contract MyTest`

\- `forge test --match-test testTransfer`

\- Gas 报告`forge test --gas-report`

\- 格式化`forge fmt`

\- 静态分析/检查`forge lint`（视版本/插件而定）

\- 安装依赖：

\- `forge install openzeppelin/openzeppelin-contracts`

\- 更新`forge update`

\---

\## 5. 测试基础（Solidity 测试）

\### 5.1 Test 合约写法

\- 测试合约一般继承：

\- `import "forge-std/Test.sol";`

\- `contract MyTest is Test { ... }`

\- 测试函数命名：

\- `test...()` 会被自动识别为测试

\- `setUp()` 在每个 test 前运行

\### 5.2 常用断言

\- `assertEq(a, b)`

\- `assertTrue(x)`

\- `assertFalse(x)`

\- `assertGt / assertLt`

\### 5.3 作弊码（Cheatcodes）核心

Foundry 最大优势之一：在测试里“控制环境”。

常用（通过 `vm`）：

\- **改调用者/交易发起者**

\- `vm.prank(addr)`：下一次调用 `msg.sender=addr`

\- `vm.startPrank(addr)` / `vm.stopPrank()`：多次调用持续生效

\- **给地址打钱**

\- `vm.deal(addr, amount)`

\- **时间/区块**

\- `vm.warp(timestamp)`：改 `block.timestamp`

\- `vm.roll(blockNumber)`：改 `block.number`

\- **预期 revert**

\- `vm.expectRevert()` 或 `vm.expectRevert(bytes("msg"))`

\- `vm.expectRevert(Selector)`（适配自定义 error）

\- **事件**

\- `vm.expectEmit(...)`

\- **外部调用模拟**

\- `vm.mockCall(target, calldata, returndata)`

\- **读写存储**

\- `vm.load` / `vm.store`（调试/攻防场景常用）

\---

\## 6. Fuzz / Invariant 测试

\### 6.1 Fuzz（模糊测试）

\- 直接在 `test` 函数里加参数即可自动 fuzz：

\- `function testFoo(uint256 x) public { ... }`

\- 常配合：

\- `vm.assume(x > 0)` 限制输入范围

\- 优点：比手写用例覆盖更广，容易发现边界 bug。

\### 6.2 Invariant（不变量测试）

\- 目标：无论怎么调用，某个性质始终成立（如“总余额守恒”）。

\- 写法通常：

\- 一个 Handler 合约负责随机调用目标合约方法

\- 测试合约写 `invariant_...()` 校验性质

\- 适合：DeFi、资金流、权限系统等复杂状态机。

\---

\## 7. Script（部署与链上交互）

\- 写在 `script/`，继承 `Script`：

\- `import "forge-std/Script.sol";`

\- 广播交易常用：

\- `vm.startBroadcast(privateKey)` / `vm.stopBroadcast()`

\- 或 CLI `--private-key` + `--broadcast`

\- 运行示例：

\- `forge script script/Deploy.s.sol:Deploy --rpc-url $RPC_URL --broadcast -vvv`

\---

\## 8. Cast（命令行交互）

常用：

\- 查区块号`cast block-number --rpc-url $RPC_URL`

\- 查余额`cast balance <addr> --rpc-url $RPC_URL`

\- 调用只读函数：

\- `cast call <contract> "balanceOf(address)(uint256)" <addr> --rpc-url $RPC_URL`

\- 发交易（写链）：

\- `cast send <contract> "transfer(address,uint256)" <to> <amt> --rpc-url $RPC_URL --private-key $PRIVATE_KEY`

\- 编码/解码工具：

\- `cast abi-encode "f(uint256)" 1`

\- `cast keccak "Transfer(address,address,uint256)"`

\---

\## 9. Anvil（本地链）

\- 启动`anvil`

\- 默认提供一组测试私钥/地址，适合本地快速验证：

\- 结合 `forge testforge script --rpc-url http://127.0.0.1:8545`
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

\## 1. 复习回顾（补课清单）

\- 数据位置`storage / memory / calldata`

\- 可见性`public / external / internal / private`

\- 函数属性`view / pure / payable`

\- 错误处理`require / revert / assert`，自定义 `error`

\- `event`：链上记录 + 前端监听

\- 数组/映射：删除（置零、swap&pop），映射不可遍历

\- `receive()` vs `fallback()`：触发条件

\- 转账：优先 `call{value: x}("")` 并检查返回值；避免 `tx.origin` 鉴权

\- 安全：重入（CEI、ReentrancyGuard）、权限控制、随机数可预测、delegatecall 存储布局风险

\## 2. 作业实践：Remix 跑 Solidity by Example（0.8.26 Basic）

\- 编译器选 `0.8.26`，环境用 `Remix VM (London)`

\- 每段代码做到：能编译 → 能部署 → 能调用验证结果

\- 重点观察：

\- 默认值（0/false/address(0)）

\- constructor 仅部署时执行

\- `view/pure` 不改状态

\- `payable` 调用需设置 `Value`

\- `require/revert` 失败报错与 gas 差异（自定义 error 更省）

\## 3. Solidity 挑战：Ethernaut

\- 通用解题流程：读源码 → 找通关条件 → 找可控入口（参数/value/fallback）→ 推演攻击 → 写攻击合约/脚本复现 → 写修复建议

\- 常见考点：访问控制、tx.origin、伪随机、重入、delegatecall/代理、自毁/强制转账
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


\## Ethereum Clients & Hardhat

\### 以太坊客户端

\- 客户端是运行以太坊节点的软件

\- 执行层客户端：Geth（Go）、Nethermind（C#）、Besu（Java）

\- 共识层客户端：Prysm、Lighthouse、Teku

\- 全节点验证所有交易和区块，轻节点仅下载区块头

\### 节点类型

\- 全节点：存储完整区块链数据，独立验证

\- 归档节点：保留所有历史状态，占用空间大

\- 轻节点：仅区块头，依赖全节点获取数据

\### JSON-RPC

\- 以太坊标准化API接口

\- 常用方法：eth\_blockNumber、eth\_getBalance、eth\_sendTransaction

\- 通过HTTP或WebSocket与节点交互

\### Hardhat框架

\- 主流Solidity开发框架，Node.js环境

\- 内置本地测试网络Hardhat Network

\- 支持console.log调试合约

\### 目录结构

\- contracts/：合约源码

\- scripts/：部署脚本

\- test/：测试文件

\- hardhat.config.js：配置文件

\### 核心命令

\- `npx hardhat compile`：编译合约

\- `npx hardhat test`：运行测试

\- `npx hardhat node`：启动本地节点

\- `npx hardhat run scripts/deploy.js`：部署合约

\### 测试编写

\- 使用Mocha测试框架和Chai断言库

\- ethers.js与合约交互

\- describe组织测试套件，it定义测试用例

\- beforeEach部署合约，确保测试隔离

\### 网络配置

\- 默认Hardhat Network（chainId: 31337）

\- 可配置Sepolia、Mainnet等网络

\- 需设置RPC URL和私钥

\### 实用插件

\- hardhat-etherscan：验证合约源码

\- hardhat-gas-reporter：gas消耗报告

\- solidity-coverage：测试覆盖率
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



\### 继承与多态

\- 使`is`关键字继承，支持多重继承

\- 继承顺序从"最基础"到"最派生"

\- virtual标记可被重写的函数，override标记重写函数

\- super调用父合约函数，遵循C3线性化规则

\### 接口与抽象合约

\- interface：定义函数签名，不实现逻辑，用于合约间交互

\- abstract：包含未实现函数的合约，不能直接部署

\- 接口函数必须声明为external

\### 可见性与访问控制

\- private变量子合约不可访问

\- internal变量子合约可访问

\- 通过modifier实现权限控制（如onlyOwner）

\### 支付相关

\- receive()：接收纯ETH转账，无calldata时触发

\- fallback()：调用不存在函数或有calldata时触发

\- 转账三种方式：transfer（2300gas，失败revert）、send（2300gas，返回bool）、call（推荐，可设gas）

\### 合约交互

\- 使用接口调用已知ABI的合约

\- call低级调用`address.call{value: amount}(abi.encodeWithSignature("func()"))`

\- delegatecall：在调用者上下文执行，storage布局必须一致

\### 工厂模式

\- 合约可部署其他合约

\- new关键字创建合约实例

\- create2可预计算部署地址，需提供salt值

\### 异常处理

\- require：条件检查，退还剩余gas

\- revert：主动回滚，配合自定义error更省gas

\- assert：检查不变量，用于内部错误

\- try/catch：处理外部调用和合约创建异常

\### 库（Library）

\- 无状态、不能接收ETH

\- 使`using A for B`语法附加到类型

\- internal函数嵌入合约，public函数需单独部署

\### ABI编码

\- abi.encode：标准编码，带填充

\- abi.encodePacked：紧凑编码，节省空间但可能碰撞

\- abi.decode：解码数据
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->




````markdown
# Day 8 学习笔记：智能合约开发与Solidity基础

## 核心内容
学习了Dapp架构、Solidity基础语法以及中文排版规范，掌握智能合约开发的基本框架和Solidity编程语言的核心概念。

## 主要知识点

### 一、Dapp架构与开发流程

#### Dapp核心架构
1. **前端**：HTML、CSS、JavaScript构建，通过钱包Provider或RPC节点与区块链交互
2. **智能合约**：Dapp核心，定义业务逻辑，部署在区块链上
3. **数据检索器**：检索智能合约Event，写入传统数据库供前端查询
4. **区块链与去中心化存储**：存储合约状态和大规模非结构化数据

#### Dapp开发流程
- **需求分析与规划**：确定功能、选择区块链平台、设计UX
- **智能合约开发**：编写合约、测试、审计优化
- **检索器开发**：捕获Event数据，写入数据库
- **前端开发**：与区块链交互，展示数据和处理交易

### 二、Solidity基础语法

#### 1. 合约结构
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyContract {
    // 状态变量
    uint256 public myNumber;
    string public myString;
    
    // 事件
    event NumberUpdated(uint256 newValue);
    
    // 构造函数
    constructor(uint256 initialValue) {
        myNumber = initialValue;
    }
    
    // 函数
    function setNumber(uint256 newValue) public {
        myNumber = newValue;
        emit NumberUpdated(newValue);
    }
    
    // 读取函数
    function getNumber() public view returns (uint256) {
        return myNumber;
    }
}
```

#### 2. 数据类型
- **值类型**：uint256、int256、bool、address、bytes32等
- **引用类型**：array、struct、mapping
- **特殊类型**：enum、bytes、string

#### 3. 函数
- **可见性**：public、private、internal、external
- **状态修改器**：view、pure、payable
- **修饰符**：自定义逻辑，如onlyOwner

#### 4. 合约交互
- 合约调用：通过地址调用其他合约
- 事件：记录合约状态变化
- 继承：合约间代码复用

### 三、中文排版规范总结

#### 1. 空格使用
- 中英文之间加空格：`在 LeanCloud 上` ✅
- 中文与数字之间加空格：`花了 5000 元` ✅
- 数字与单位之间加空格：`10 Gbps` ✅
- 度數/百分比与数字之间不加空格：`90°`、`15%` ✅
- 全形標點與其他字符之間不加空格：`iPhone，好開心！` ✅

#### 2. 标点符号
- 不重复使用标点：`戰勝了巴西隊！` ✅（而非`戰勝了巴西隊！！！`）
- 使用全形中文標點：`嗨！你知道嘛？` ✅（而非`嗨!你知道嘛?`）

#### 3. 全形和半形
- 数字使用半形：`1000 元` ✅（而非`１０００ 元`）
- 英文整句使用半形标点：`「Stay hungry, stay foolish.」` ✅

#### 4. 专有名词
- 大小写正确：`GitHub` ✅（而非`github`或`Github`）

## 学习心得

1. **Dapp架构理解**：Dapp与传统应用的核心区别在于去中心化，前端不直接连接区块链，而是通过钱包或RPC节点交互，智能合约作为核心业务逻辑层。

2. **Solidity基础掌握**：Solidity语法与JavaScript有相似之处，但也有独特特性，如状态变量、可见性修饰符、事件等，需要深入理解其运行机制。

3. **中文排版规范重要性**：规范的中文排版能提升内容可读性和专业性，在智能合约注释、文档编写中尤为重要。

4. **开发流程认知**：智能合约开发需要完整的流程，从需求分析到部署上线，每个环节都需要严格把控，特别是安全审计环节。

## 待解决的问题

- 如何编写更安全的智能合约，避免常见漏洞？
- 如何优化智能合约的Gas使用？
- 如何设计高效的数据检索器？
- 如何将中文排版规范应用到智能合约开发文档中？

## 学习计划

- 继续学习Solidity高级特性
- 实践编写简单的智能合约
- 学习智能合约安全审计
- 深入理解Dapp完整开发流程
````
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->





[https://x.com/palytoxin5/status/2012876581187067984?s=20](https://x.com/palytoxin5/status/2012876581187067984?s=20)

今天发了推文，还听了分享会：**分享会 - Key Hash Based Tokens: 从 ERC-721 到 ERC-7962**

7天内系统学习了Web3与以太坊核心知识：区块链基础、Web3概念、钱包安全、合规要求、NFT/IPFS，以及Remix智能合约开发。收获：理解去中心化特性、用户主权价值，掌握钱包原理、Uniswap AMM机制、NFT标准，熟悉Remix开发流程。 Web3前景广阔需持续学习！
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->






# 学习总结（Day1-6）

### Day1：区块链基础与Web3概念

-   **区块链定义**：分布式账本技术，通过密码学保证数据不可篡改
    
-   **核心特点**：去中心化、透明性、不可篡改性、安全性
    
-   **区块链类型**：公链、私链、联盟链
    
-   **Web3 vs Web2**：Web3强调去中心化、用户主权、价值互联，区别于Web2的平台中心化
    
-   **以太坊架构**：基于区块链的智能合约平台，支持去中心化应用（DApp）开发
    

### Day2：以太坊开发与钱包安全

-   **钱包工作原理**：管理密钥对，签名交易，访问区块链
    
-   **钱包类型**：热钱包（在线，如MetaMask）、冷钱包（离线，如硬件钱包）
    
-   **安全基础**：助记词（12/24词）→ 种子 → 私钥 → 公钥 → 地址
    
-   **以太坊账户**：外部账户（EOA）和合约账户
    
-   **Uniswap V2**：去中心化交易所，基于自动做市商（AMM）机制
    

### Day3：安全合规与ENS

-   **合规要求**：中国禁止ICO、虚拟货币交易所运营，明确虚拟货币不具法偿性
    
-   **法律风险**：代币发行、赌博、传销、洗钱等风险
    
-   **全球监管**：欧盟MiCA、新加坡新规、美国稳定币法案
    
-   **ENS**：以太坊名称服务，将复杂地址转换为易记域名
    
-   **DEX机制**：恒定乘积公式x\*y=k确保流动性池平衡
    

### Day4：NFT与IPFS

-   **NFT定义**：非同质化代币，每个代币具有唯一性
    
-   **标准对比**：ERC20（同质化代币）vs ERC721（NFT）
    
-   **应用场景**：数字艺术品、游戏道具、身份凭证、虚拟地产
    
-   **IPFS**：去中心化存储网络，用于存储NFT媒体文件
    
-   **Metadata**：NFT描述信息，包含名称、属性、图片链接等
    

### Day5：Remix IDE开发环境

-   **Remix特点**：浏览器基智能合约开发环境，无需安装
    
-   **开发流程**：创建合约→编译→部署→测试
    
-   **核心功能**：调试（断点、单步执行）、测试（Solidity测试文件）、插件扩展
    
-   **示例合约**：SimpleStorage合约的编写、编译、部署与测试
    

## 学习心得

-   **知识体系**：Web3是一个多层次的技术体系，从基础概念到开发实践需要系统学习
    
-   **安全优先**：区块链领域安全风险较高，需时刻关注合规要求和技术安全
    
-   **实践重要性**：通过Remix等工具进行实践操作，加深对概念的理解
    
-   **生态多样性**：以太坊生态包含DeFi、NFT、DAO等多种应用场景，需全面了解
    

## 整体收获

通过6天的系统学习，对Web3与以太坊生态有了全面的认识，掌握了核心概念和基础开发技能。Web3作为下一代互联网的重要方向，具有广阔的发展前景，但也面临技术、安全、合规等多方面挑战。后续需持续学习，关注技术发展和行业动态，逐步深入理解和应用Web3技术。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->







````markdown
# Day 5 学习笔记：Remix IDE开发环境

## Remix IDE简介
基于浏览器的以太坊智能合约开发环境，特点：
- 无需安装，浏览器直接使用
- 支持完整开发流程：编写→编译→部署→测试

## 基础操作

### 访问与界面
- 官方地址：remix.ethereum.org
- 界面核心区域：文件浏览器、编辑器、编译面板、部署面板、控制台

### 开发流程示例

#### 1. 创建与编写合约
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleStorage {
    uint256 private _value;
    event ValueChanged(uint256 newValue);
    
    function store(uint256 value) public {
        _value = value;
        emit ValueChanged(value);
    }
    
    function retrieve() public view returns (uint256) {
        return _value;
    }
}
```

#### 2. 编译合约
- 切换编译面板，选择对应Solidity版本
- 点击"Compile"按钮，查看ABI、Bytecode等结果

#### 3. 部署合约
- 切换部署面板，选择"JavaScript VM"模拟环境
- 选择账户，点击"Deploy"按钮
- 部署成功后显示在"Deployed Contracts"区域

#### 4. 交互测试
- 调用`retrieve()`查看初始值（0）
- 调用`store(42)`更新值
- 再次调用`retrieve()`验证更新

## 关键功能
- **调试**：支持断点、单步执行、变量状态查看
- **测试**：支持Solidity测试文件编写与执行
- **插件**：可安装安全检查、代码格式化等插件扩展功能

## 学习心得
Remix IDE作为浏览器基开发环境，无需安装即可快速上手，适合初学者。其完整的开发流程支持大幅降低了智能合约开发的入门门槛。

- 使用固定Solidity版本，避免浮动版本号
- 测试环境用模拟账户，勿用真实资金
- 主网部署前充分测试
- 注意Gas费用设置
````
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->








# Day 3 学习笔记：安全合规与ENS

## Web3安全与合规

**核心法律风险**要特别注意：

-   代币发行与交易：不管叫"积分"“凭证"还是"治理Token”，只要具备融资功能或可流通性，就可能构成非法金融行为
    
-   赌博、传销、洗钱：链游的"充值—抽奖—提现"容易被认定为开设赌场罪，“邀请返利”"多级推广"可能触犯传销活动罪
    
-   场外交易中的洗钱风险：虚拟货币常被用作规避监管的"地下换汇"工具，参与者可能被控洗钱罪
    

技术人员参与代币模型设计、空投逻辑配置、合约部署等环节，也会被视为"共同行为人"，不能以"只是写代码"为由免责。这个真的要注意！

## 全球监管趋势

全球加密货币合规化趋势越来越明显。欧盟的MiCA正在落地，新加坡出台新规，美国也出台了《稳定币法案》。更健全的监管框架能提供更清晰的指导，降低风险，增强用户信心。

## ENS和DEX

今天还学了ENS（以太坊名称服务），可以把复杂的以太坊地址换成好记的域名，比如"alice.eth"。这样转账就不用记那一长串地址了，方便很多。

DEX（去中心化交易所）像Uniswap，用自动做市商（AMM）机制，不需要订单簿就能交易。核心是恒定乘积公式x \* y = k，保证流动性池平衡。

## 身份和Sybil攻击

Web3中的身份管理很重要，但也要防范Sybil攻击（一个人创建多个虚假身份）。一些项目用"邀请返利""算力挂靠"来激励，但容易被滥用。

## 学习心得

今天的学习让我意识到Web3不是法外之地，合规性要求很严格。技术人员不能只关注技术，还要了解法律风险。"只是写代码"不能作为免责理由，参与代币发行等环节也可能承担法律责任。

**安全清单**：

-   了解所在国家的加密货币监管政策
    
-   不参与ICO、IEO、IDO等融资活动
    
-   警惕"充值—抽奖—提现"的链游模式
    
-   不参与多级推广、邀请返利
    
-   场外交易要谨慎，了解交易对手背景
    
-   妥善保管助记词，不分享给任何人
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->









## Day 2 学习笔记：以太坊开发与钱包安全

### 以太坊开发入门

以太坊是一个开源的、基于区块链的分布式计算平台，支持智能合约的部署和执行。技术架构包括共识层（负责网络共识和安全性）、执行层（负责交易处理和智能合约执行）和客户端（连接到以太坊网络的软件节点）。

智能合约是存储在区块链上的程序，当满足预设条件时自动执行。工作流程是：开发者用Solidity编写合约 → 编译成字节码 → 部署到区块链 → 用户通过交易调用合约函数 → 合约执行并更新状态。

**关键概念**：

-   **账户**：分为外部账户（EOA）和合约账户
    
-   **交易**：从外部账户发送的数据包
    
-   **Gas**：执行交易的计算资源计量单位
    
-   **状态**：以太坊的全局状态，包括所有账户和合约的状态
    

### 钱包工作原理

重要概念是：钱包不"存储"加密货币，而是存储用于访问区块链上资产的私钥。资产实际上记录在区块链上。

钱包分为热钱包（连接互联网的软件钱包，如MetaMask）和冷钱包（离线存储的硬件钱包，如Ledger、Trezor）。

按功能分为非托管钱包（用户完全控制私钥）和托管钱包（第三方托管私钥）。核心原则是"Not your keys, not your coins"

### 助记词与密钥对

助记词是一组12-24个英文单词，用于生成私钥的种子。遵循BIP-39标准，生成过程包括：生成熵 → 计算校验和 → 组合熵和校验和 → 转换为助记词 → 生成种子 → 派生密钥。

**安全管理**是重中之重： 正确做法：写在纸上，制作多个备份，分散存储，使用保险箱 错误做法：截图、拍照、存储在电脑或云盘、告诉任何人

密钥对由公钥和私钥组成，基于非对称加密。私钥必须保密，用于签名交易；公钥可以公开，用于验证签名和生成地址。以太坊地址从公钥的Keccak-256哈希派生，是20字节的十六进制字符串。

### 账户体系与权限控制

以太坊有两种账户类型：

-   **外部账户（EOA）**：由私钥控制，可以发起交易，没有关联代码
    
-   **合约账户**：由合约代码控制，不能主动发起交易，包含可执行代码
    

权限控制机制包括：

-   **私钥签名**：交易必须用私钥签名
    
-   **多重签名**：需要多个私钥签名才能执行（如3-of-5多签钱包）
    
-   **智能合约权限**：通过合约代码实现复杂权限控制
    

### Uniswap V2协议

Uniswap V2是一个去中心化交易协议，采用自动做市商（AMM）机制。核心是恒定乘积公式x \* y = k，确保流动性池的平衡。协议包括Factory（创建交易对）、Router（路由交易）和Pair（管理流动性池）等核心合约。

### 学习心得

通过学习，对以太坊开发和钱包安全有了更深的理解。钱包安全是Web3使用的基础，必须高度重视。"Not your keys, not your coins"是Web3的核心原则。硬件钱包提供了最高的安全性，值得投资。多签钱包适合团队和重要资产管理。定期备份和测试恢复流程是必要的。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->










## Day 1 学习笔记：区块链基础与Web3概念

### 一、区块链核心概念

区块链的基本原理是一个去中心化的分布式账本技术。简单来说，它是一个公开的、不可篡改的数字账本，由一系列按时间顺序连接的"区块"组成。每个区块包含多笔交易数据和元数据，通过哈希值与前一个区块相连，形成一条链。

**关键特性**：

-   **不可篡改**：每个区块包含上一个区块的哈希值，修改历史数据需重新计算所有后续哈希，几乎不可能
    
-   **公开透明**：所有交易记录公开可查，任何人都可验证
    
-   **匿名性**：通过随机钱包地址交易，公开但身份未知
    
-   **快速交易**：交易一旦打包即完成，无论金额或地理位置，比传统跨境汇款便捷
    

### 二、分布式网络与去中心化

区块链的核心是分布式网络。网络中有大量节点（矿机）记账，每个节点存储完整相同的区块链数据，实现真正去中心化。节点全球分布，无人能控制全部节点，数据永久存在。若修改部分节点数据，只要修改节点不超过51%，改动无效。这就是区块链安全的原因。

### 三、区块链类型

区块链分三种主要类型：

-   **公链**：完全开放，任何人可参与，如比特币、以太坊
    
-   **私链**：仅特定组织开放，如企业内部系统
    
-   **联盟链**：多个组织共同管理，如银行间结算系统
    

每种类型适用场景不同，公链去中心化最高但速度慢，私链速度快但中心化高。

### 四、去中心化的优势与挑战

**优势**包括抗审查性、数据主权、透明度、互操作性和全球访问。

**挑战**也很明显：可扩展性有限、用户体验复杂、监管合规困难、安全风险、能源消耗高等。

### 学习心得

今天的学习让我深刻理解区块链的核心价值在于去中心化和不可篡改。Web3不仅是技术变革，更是社会和经济模式的创新，理解区块链类型有助于选择合适的应用场景。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
