---
timezone: UTC+8
---

# daicidemeigui

**GitHub ID:** daicidemeigui

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->
# Vibe Coding

## 流程

1.  **描述目标**：用户首先用通俗易懂的语言给出一个概要提示（例如，“创建一个Python函数，读取CSV文件并返回‘email’列”）。
    
2.  **AI生成代码**：AI助理会解读用户的请求并生成初始代码块或应用结构 。
    
3.  **执行和观察**：用户运行生成的代码，查看其是否按预期工作 。这一步至关重要，且与传统的代码审查不同；其主要的验证方式是执行，而非手动检查 。
    
4.  **提供反馈并优化**：如果输出不正确或发生错误，用户会提供新的反馈，通常是将错误信息粘贴回AI并请求修复（例如，“这没问题，但请为找不到文件的情况添加错误处理”）。这个循环会一直重复，直到功能完善为止。
    

## 现有工具-cursor

-   **核心功能**：Cursor是一款基于VSCode分支构建的AI驱动IDE，旨在将AI直接带入开发者的工作空间 。其关键功能包括：
    
    -   **代码库级上下文**：与简单的聊天助手不同，Cursor能够吸收并理解整个代码库，从而提供更具相关性的建议，并执行复杂的多文件编辑 。例如，它可以为整个项目生成一份README文件，这展示了其深度的上下文理解能力 。
        
    -   **行内编辑与聊天**：用户可以直接在编辑器中高亮一段代码，并用自然语言下达指令，如“让这段代码运行得更快”或“修复这个函数中的错误”，从而无需切换上下文 。
        
    -   **预测性补全（“Tab补全”）**：Cursor能够预测开发者的下一步编辑意图，并生成期望的代码，用户只需按一下键即可接受 。用户反馈称，这种体验如同“以思想的速度编码” 。
        
    -   **熟悉的用户体验**：作为VSCode的分支，它允许用户一键导入现有的扩展、主题和快捷键，确保了较低的学习曲线 。
        
-   **工作流分析**：Cursor通过使与AI的互动感觉像对话一样流畅和集成，从而促进了一种流动的“氛围”。AI不再是一个独立的顾问，而是一个无处不在的协作者 。这对于初学者和希望获得引导式、支持性体验的用户来说是理想的选择 。
    
-   **局限性与用户体验**：尽管功能强大，Cursor并非没有缺点。它可能占用较多系统资源，在处理大文件时会出现延迟 。在长时间不活动后，AI有时会“忘记”上下文，或将代码插入错误的位置 。此外，尽管其基于聊天的界面很直观，但仍需要用户对如何“引导”大语言模型有隐性的理解，这种技巧并非所有用户都能立即掌握 。
    

### **开发者角色转变与思考**

未来的开发者不会被取代，而是被增强和提升 。最有价值的技能将不再是编写样板代码的能力，而是：

-   在系统和架构层面思考。
    
-   向人类和AI清晰地阐述复杂需求。
    
-   批判性地评估和验证AI生成的工作。
    
-   设计和工程化人机协作过程本身（即上下文工程）。
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->

本周末比较忙，所以没有具体完成什么新的学习方面的东西，目前正在努力追赶Solidity101网站的基础学习计划，目前进度是第三章，争取下周能跟上进度早日完成：  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/daicidemeigui/images/2026-01-25-1769352218638-image.png)
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->


# 区块链概念学习

## 公链类型

### **一、主流通用型公链**

**1\. 比特币（Bitcoin）**

-   **定位**：首个区块链，主打价值存储与点对点支付。
    
-   **共识机制**：PoW（工作量证明）。
    
-   **特点**：高安全性、低吞吐量（7 TPS），脚本功能有限。
    

**2\. 以太坊（Ethereum）**

-   **定位**：智能合约平台，支持DeFi、NFT、DApp生态。
    
-   **共识机制**：原为PoW，2022年合并后转为PoS（权益证明）。
    
-   **特点**：图灵完备的EVM，生态最丰富，但Gas费较高。
    

**3\. BNB Chain（原币安智能链）**

-   **定位**：兼容EVM的高性能链，由币安推动。
    
-   **共识机制**：PoSA（权益证明权威）。
    
-   **特点**：低手续费、高速度，中心化程度较高。
    

**4\. Solana**

-   **定位**：高性能公链，主打高吞吐量。
    
-   **共识机制**：PoH（历史证明）结合PoS。
    
-   **特点**：理论TPS达6.5万，低成本，但曾出现网络中断问题。
    

**5\. Cardano**

-   **定位**：科研驱动的PoS公链，强调安全与合规。
    
-   **共识机制**：Ouroboros PoS。
    
-   **特点**：分层架构，注重形式化验证，发展节奏较慢。
    

**6\. Polkadot**

-   **定位**：跨链互操作协议，通过中继链连接多条平行链。
    
-   **共识机制**：NPoS（提名权益证明）。
    
-   **特点**：共享安全性，支持定制化区块链（Substrate框架）。
    

**7\. Avalanche**

-   **定位**：高可扩展性三层网络（P/C/X链）。
    
-   **共识机制**：Snow共识（类PoS）。
    
-   **特点**：亚秒级最终确认，支持自定义虚拟机。
    

**8\. Cosmos**

-   **定位**：区块链互联网，通过IBC协议实现链间通信。
    
-   **共识机制**：Tendermint PoS。
    
-   **特点**：模块化开发（Cosmos SDK），强调主权互操作。
    

## Layer

**1\. Layer 1（基础层/结算层）**

-   **定义**：区块链的底层基础协议，是最终的“真理之源”。
    
-   **核心职责**：
    
    -   **共识机制**：确保所有节点对交易记录达成一致（如PoW， PoS）。
        
    -   **数据可用性**：存储完整的交易历史和状态。
        
    -   **最终结算**：交易的最终确认和不可篡改。
        
-   **代表项目**：**比特币、以太坊、Solana、BNB Chain** 等所有独立的公链。
    
-   **特点**：
    
    -   **优点**：安全性最高、去中心化程度最高。
        
    -   **缺点**：可扩展性瓶颈（速度慢、费用高、吞吐量低）。
        

**2\. Layer 2（扩展层）**

-   **定义**：构建在Layer 1之上的“扩展方案”。它们将大部分交易处理从主链（L1）转移到链下（off-chain），只将最终结果或证明批量提交回L1进行结算和安全保障。
    
-   **核心目标**：**大幅提升交易速度、降低交易成本**，同时继承L1的安全性。
    
-   **核心技术**：
    
    -   **Rollups（汇总）**：当前主流技术，将所有交易“卷起来”打包，然后将一个数据包提交到L1。
        
        -   **ZK-Rollups**：使用零知识证明来验证交易的有效性，**安全性最高，速度最快**（如 **zkSync, StarkNet, Polygon zkEVM**）。
            
        -   **Optimistic Rollups**：默认交易是有效的，但设有“欺诈证明”挑战期，**兼容性更好**（如 **Arbitrum, Optimism, Base**）。
            
    -   **状态通道**：用户先在链上锁定资产，然后在链下进行多次快速交易，最后将最终状态结算回链上（如比特币的闪电网络）。
        
    -   **侧链**：拥有独立共识机制的链，通过桥与主链连接，但安全性独立于L1（如 **Polygon PoS** 早期被视为侧链）。
        
-   **特点**：**性能的飞跃**，通常TPS（每秒交易数）是L1的数十到数百倍，费用仅为L1的零头。
    

**3\. Layer 3（应用层/定制层）**

-   **定义**：构建在L2（或直接L1）之上的**特定应用或互操作框架**。它不是另一个通用的扩展层，而是为特定用例进行深度优化的执行环境。
    
-   **核心目标**：实现**极致的定制化、用户体验和跨链互操作性**。
    
-   **典型形态**：
    
    -   **超定制化应用链**：例如，一个游戏可以构建自己的L3链，使用自己定义的Gas代币、隐私规则和治理模型，完全为该游戏优化。
        
    -   **跨链互操作枢纽**：专门用于连接不同L2或L1的通信层，让资产和数据在不同扩展方案间无缝流动。
        
    -   **开发者友好平台**：提供高度抽象的API和工具，让开发者无需关心底层（L1/L2）的复杂性，快速部署DApp。
        
-   **代表概念/项目**：
    
    -   **Arbitrum Orbit**、**zkSync Hyperchains**、**StarkNet Appchains**：这些是允许开发者在他们的L2上构建定制化L3链的框架。
        
    -   **Cosmos IBC**：虽然架构不同，但其“应用链”理念与L3的定制化思想高度契合。
        
-   **特点**：**可定制性极强，用户体验可以接近Web2应用**。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



# ERC20模板

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import "./IERC20.sol";

contract ERC20 is IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner, address indexed spender, uint256 value
    );

    uint256 public totalSupply;
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    string public name;
    string public symbol;
    uint8 public decimals;

    constructor(string memory _name, string memory _symbol, uint8 _decimals) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
    }

    function transfer(address recipient, uint256 amount)
        external
        returns (bool)
    {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount)
        external
        returns (bool)
    {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function _mint(address to, uint256 amount) internal {
        balanceOf[to] += amount;
        totalSupply += amount;
        emit Transfer(address(0), to, amount);
    }

    function _burn(address from, uint256 amount) internal {
        balanceOf[from] -= amount;
        totalSupply -= amount;
        emit Transfer(from, address(0), amount);
    }

    function mint(address to, uint256 amount) external {
        _mint(to, amount);
    }

    function burn(address from, uint256 amount) external {
        _burn(from, amount);
    }
}
```

# ERC20概念

ERC20 是 **以太坊区块链上创建和发行可互换代币** 的技术标准。它由一系列 **必须实现的函数和可选函数** 组成，确保所有基于此标准发行的代币都能在以太坊生态中（如钱包、去中心化交易所）无缝兼容和交互。

你可以把它理解为代币的 **“通用说明书”或“接口协议”**。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




# Solidity

## EVM组成部分

-   **Virtual ROM**：用于存放代码虚拟只读存储器，在ROM中的code(智能合约)不能更改（只读不写）。
    
-   **Machine state**：相当于RAM，一种易失性的存储器。这里的易失的是指当合约代码执行完毕（执行另一个合约代码）内存的变量会被清除。
    
-   **World state**：所有合约的状态变量存储的地方，这部分数据是永久存储在区块链上的数据。
    

![EVM.png](https://p6-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/97663290254d497fadd5f4e9d72130e6~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAgU2FsYnQ=:q75.awebp?rk3s=f64ab15b&x-expires=1769063525&x-signature=VoLKjsECUnV0KJIhhJxmKWGe2Ug%3D)

## 基础语法

### 数据结构

| 类别 | 类型 | 说明 | 示例 | 存储位置 |
| --- | --- | --- | --- | --- |
| 值类型 | 布尔型 | 真/假值 | bool isActive = true; | - |
|   | 整数 | 有/无符号整数 | uint256 count; int256 temp; | - |
|   | 地址 | 20字节地址 | address owner; | - |
|   | 字节 | 固定大小字节 | bytes32 hash; | - |
|   | 枚举 | 自定义枚举类型 | enum Status { Pending, Active } | - |
| 引用类型 | 动态数组 | 大小可变的数组 | uint256[] numbers; | storage/memory/calldata |
|   | 固定数组 | 大小固定的数组 | address[5] owners; | storage/memory/calldata |
|   | 结构体 | 自定义复合类型 | struct User { address addr; uint balance; } | storage/memory |
|   | 字节数组 | 动态字节 | bytes data; | storage/memory/calldata |
|   | 字符串 | UTF-8字符串 | string name = “Token”; | storage/memory/calldata |
| 映射类型 | 映射 | 键值对存储 | mapping(address => uint) balances; | 仅storage |
|   | 嵌套映射 | 多层映射 | mapping(address => mapping(uint => bool)) approvals; | 仅storage |
| 特殊类型 | 函数 | 函数类型 | function(uint) external returns(bool) callback; | - |
|   | 地址支付 | 可支付地址 | address payable receiver; | - |

### 存储位置区别

| 位置 | 适用类型 | 生命周期 | 是否可修改 | 适用场景 |
| --- | --- | --- | --- | --- |
| storage | 所有引用类型 | 永久 | 是 | 状态变量、局部引用存储变量 |
| memory | 所有引用类型 | 函数执行期间 | 是 | 函数内临时变量、函数参数 |
| calldata | 数组、结构体、字节 | 函数调用期间 | 否 | 外部函数参数（只读） |

### **可见修饰符**

| 修饰符 | 作用域 | 可否被继承 | Gas成本 | 使用场景 |
| --- | --- | --- | --- | --- |
| public | 内外均可访问 | ✓ | 较高 | 外部调用接口 |
| private | 仅当前合约 | ✗ | 低 | 内部工具函数 |
| internal | 当前及派生合约 | ✓ | 低 | 合约内部逻辑 |
| external | 仅外部调用 | ✓ | 最低 | 优化外部调用 |

### 状态修饰符

| 修饰符 | 可读状态 | 可写状态 | 可接收ETH | 使用场景 |
| --- | --- | --- | --- | --- |
| view | ✓ | ✗ | ✗ | 读取状态，不修改 |
| pure | ✗ | ✗ | ✗ | 纯计算，不读写状态 |
| payable | ✓ | ✓ | ✓ | 接收ETH转账 |

## 合约组成部分案例

```
// 1. 许可证标识
// SPDX-License-Identifier: MIT

// 2. 版本声明
pragma solidity ^0.8.0;

// 3. 导入
import "@openzeppelin/contracts/access/Ownable.sol";

// 4. 合约声明
contract MyToken is Ownable {
    
    // ============ 状态变量 ============
    string public name;
    uint256 public totalSupply;
    mapping(address => uint256) private balances;
    
    // ============ 事件 ============
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Mint(address indexed to, uint256 amount);
    
    // ============ 错误 ============
    error InsufficientBalance(uint256 available, uint256 required);
    
    // ============ 结构体/枚举 ============
    struct Holder {
        address wallet;
        uint256 since;
    }
    
    enum TokenState { Active, Paused, Burned }
    
    // ============ 修饰器 ============
    modifier whenActive() {
        require(state == TokenState.Active, "Token inactive");
        _;
    }
    
    // ============ 构造函数 ============
    constructor(string memory _name, uint256 _initialSupply) {
        name = _name;
        totalSupply = _initialSupply;
        balances[msg.sender] = _initialSupply;
    }
    
    // ============ 函数区 ============
    
    // 1. 外部函数
    function transfer(address to, uint256 amount) 
        external 
        whenActive 
        returns (bool) 
    {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        balances[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }
    
    // 2. 视图函数
    function balanceOf(address account) public view returns (uint256) {
        return balances[account];
    }
    
    // 3. 纯函数
    function calculateReward(uint256 amount, uint256 rate) 
        public 
        pure 
        returns (uint256) 
    {
        return amount * rate / 100;
    }
    
    // 4. 仅所有者函数
    function mint(address to, uint256 amount) public onlyOwner {
        totalSupply += amount;
        balances[to] += amount;
        emit Mint(to, amount);
    }
    
    // 5. payable函数
    function buyTokens() public payable {
        uint256 tokens = msg.value * 1000; // 假设汇率
        balances[msg.sender] += tokens;
    }
    
    // ============ 回退函数 ============
    receive() external payable {
        // 接收ETH
    }
    
    fallback() external {
        // 处理未知调用
    }
}
```

## 许可证标识

许可证标识是 Solidity 文件开头的特殊注释，用于声明智能合约的软件许可证。它告诉用户和其他开发者该合约代码的使用权限、复制、修改和分发规则，类似于开源项目的开源协议。

### 许可证类型

| 许可证 | 标识符 | 特点 | 使用场景 |
| --- | --- | --- | --- |
| MIT | MIT | 最宽松，允许任意使用 | 大多数开源项目 |
| GPL-3.0 | GPL-3.0-only | 强制开源，传染性 | 要求衍生作品也开源 |
| Apache-2.0 | Apache-2.0 | 专利保护条款 | 企业级项目 |
| Unlicense | Unlicense | 公共领域，无限制 | 完全放弃版权 |
| 商业许可证 | UNLICENSED | 保留所有权利 | 私有、商业项目 |
| BSD-3-Clause | BSD-3-Clause | 宽松，需保留声明 | 学术、研究项目 |
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->





## 一、Dapp 核心定义

去中心化应用（Dapp）是运行在区块链或分布式网络上的全新应用模式，核心特征为**去中心化**—— 应用逻辑和数据不由单一实体控制，由多个参与者共同维护，区别于传统集中式应用。开发需掌握去中心化技术栈、智能合约编程及前端与区块链的交互方式。

## 二、Dapp 架构组成

Dapp 架构包含四个核心部分，各组件分工明确、协同工作：

1.  **前端（User Interface）**：
    
    -   由 HTML、CSS、JavaScript（React、Vue 等框架）构建，是用户交互界面。
        
    -   不直接连接区块链，通过钱包注入的 Provider 或第三方 RPC 节点实现交互：只读操作（如查询合约状态）通过 RPC 节点调用 `eth_call`；写操作（如转账）由前端构造交易，经钱包签名后通过 RPC 节点广播上链。
        
    -   需集成 MetaMask 等区块链钱包，用于身份验证和交易签署，保障用户隐私与安全。
        
2.  **智能合约（Smart Contracts）**：
    
    -   Dapp 的核心，定义应用业务逻辑并部署在区块链上，通过自动化规则确保交易透明性与不可篡改性。
        
    -   以太坊平台中，常用 Solidity 语言编写，通过以太坊虚拟机（EVM）执行。
        
3.  **数据检索器（Indexer）**：
    
    -   负责捕获智能合约释放的 `Event` 日志（如 NFT 转移的 `Transfer` 事件），清洗后写入 PostgreSQL 等传统数据库。
        
    -   解决区块链原生数据查询限制（如 NFT 合约不支持按地址查询持仓），为前端提供高效数据检索支持。
        
4.  **区块链和去中心化存储（Blockchain & Decentralized Storage）**：
    
    -   区块链存储智能合约状态数据与交易记录；去中心化存储（IPFS、Arweave）存储图片、文档等大规模非结构化数据。
        
    -   通过多节点备份，确保数据持久性、防丢失与防篡改，契合去中心化特性。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->






# Remix IDEA

## 简单编写一个智能合约

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TestToken {
    string public name = "TEST";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    
    address public owner;
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    
    constructor(uint256 _initialSupply) {
        owner = msg.sender;
        totalSupply = _initialSupply * 10 ** decimals;
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }
   

    function transfer(address to, uint256 value) external returns (bool) {
        require(balanceOf[msg.sender] >= value, "Insufficient balance");
        
        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;
        
        emit Transfer(msg.sender, to, value);
        return true;
    }
    
    function approve(address spender, uint256 value) external returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }
    
    function transferFrom(address from, address to, uint256 value) external returns (bool) {
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Allowance exceeded");
        
        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;
        
        emit Transfer(from, to, value);
        return true;
    }
}
```

## 使用Remix部署

先编译和运行脚本

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/daicidemeigui/images/2026-01-18-1768735642704-image.png)

成功后会在这里看到部署的合约

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/daicidemeigui/images/2026-01-18-1768735699840-image.png)

最后进行测试合约功能，使用transfer进行转账

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/daicidemeigui/images/2026-01-18-1768735755858-image.png)

然后再用balance查看接收代币的地址有多少数量

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/daicidemeigui/images/2026-01-18-1768735789248-image.png)
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->







# 以太坊

## ERC

ERC 代表 Ethereum Request for Comments，即“以太坊征求意见”。它是以太坊开发者的协议提案，属于以太坊智能合约开发人员使用的技术文档，旨在为以太坊生态系统中的特定用户组提供方法、创新、研究和特征的规范。这些文档通常由开发人员创建，并包含有关协议规范和合约说明的信息。简单类比一下，就相当于我们使用的HTTP协议，由大家一起遵守的一个规则，构建出现在的庞大的互联网应用。

### 常见ERC

**1\. ERC-20 (同质化代币标准)**

-   **特点**: 代币互换性强，价值相等，可分割。
    
-   **应用**: 大多数加密货币、DeFi代币、治理代币、游戏内货币。
    
-   **功能**: 定义了代币的名称、符号、总量、精度等。 
    

**2\. ERC-721 (非同质化代币标准/NFT)**

-   **特点**: 每个Token独一无二，不可分割，如数字艺术品。
    
-   **应用**: 数字艺术、收藏品、PFP头像、游戏道具、虚拟土地。
    
-   **功能**: 保证所有权、可追溯性，实现资产的唯一性。 
    

**3\. ERC-1155 (多代币标准)**

-   **特点**: 结合了ERC-20和ERC-721的优点，一个合约可管理同质化和非同质化代币。
    
-   **应用**: 区块链游戏（同时管理道具和货币）、元宇宙资产。
    
-   **功能**: 优化批量操作，降低成本，提高效率。
    

## EIP

EIP 即以太坊改进提案，是以太坊社区为区块链提出升级建议或新功能的一种方式。它类似于一个蓝图，详细说明了变更的内容、原因以及实施方法。这些提案旨在让所有利益相关方就变更的内容和原因达成共识，从而推动以太坊的持续发展并提升适应性。

### EIP类别

1.  **核心**：聚焦于以太坊协议的变更，如交易处理方式的更新或硬分叉等重大升级。
    
2.  **网络**：涉及以太坊客户端的通信方式，确保其顺畅运作。
    
3.  **界面**：重点关注用户和应用如何与以太坊进行交互。
    
4.  **ERC（以太坊征求意见提案）**：主要关注与代币等相关的标准。例如ERC-20。
    

### EIP提案案例

-   **EIP-1559**：改变了交易费用机制，使费用更加可预测，并通过销毁部分费用来减少以太币的流通量。
    
-   **ERC-721**：为 NFT（非同质化代币）设定了标准，NFT 是独一无二的数字资产，如艺术品或收藏品。
    
-   **EIP-4844**：也称为 proto-danksharding，该提案通过引入携带 Blob 的交易，进一步提升了以太坊的可扩展性。这一变更支持以更低的成本处理大量数据，为未来的 danksharding 升级奠定了基础。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->








# 安全与合规

## 国内相关公告/文件

### **《关于防范比特币风险的通知》**

由中国人民银行等五部委在2013年发布，**首次明确**比特币属性为“特定的虚拟商品”，**不具有**与货币等同的法律地位，不能且不应作为货币在市场流通，并要求金融机构和支付机构不得提供比特币相关服务。

### **《关于防范代币发行融资风险的公告》**

由中国人民银行等七部委在2017年发布，将**首次代币发行**（ICO）定性为“未经批准非法公开融资的行为”，**全面叫停**各类代币发行融资活动。

### **《关于进一步防范和处置虚拟货币交易炒作风险的通知》**

由中国人民银行等十部门在2021年发布，此次整改是针对加密市场进行全面的监管升级，有以下几个重要的手段：

-   明确“虚拟货币相关业务活动属于非法金融活动”。
    
-   境外虚拟货币交易所通过互联网向中国境内居民提供服务同样属于非法。
    
-   为相关非法金融活动提供技术支持、营销宣传、支付结算等服务均被禁止。
    

## 常见诈骗、攻击手段

### **技术攻击**

| 重放攻击 | 在一条链上广播的合法签名交易，被恶意复制到另一条链上重复执行。 |

| 智能合约漏洞 | 合约代码存在安全缺陷，如重入、整数溢出、权限校验错误等。 |

| 女巫攻击 | 攻击者伪造大量虚假节点或身份，破坏网络的共识机制或治理投票。 |

| 前端攻击/网络钓鱼 | 黑客伪造或入侵项目官网、钱包界面，诱导用户输入私钥或授权恶意交易。 |

### **金融诈骗**

| 钓鱼与虚假客服 | 在社交媒体、群聊中冒充官方客服，以“帮助解决问题”为名索要私钥或助记词。 |

| 貔貅盘 | 智能合约被编写为只允许买、不允许卖，或仅允许项目方出售，投资者无法套现。 |

| 伪造空投 | 发送虚假空投代币到用户地址，诱使用户与恶意智能合约交互，从而盗取资产。 |

## 防范手段

-   新人最好做到**不点！不签！不装！不转！**
    
-   保护好自己的私钥，永远不要对外公开、泄露自己的私钥，最好也不要截图保存，而是自己线下记录或使用硬件钱包
    
-   安装相关安全插件，例如GoPlus等，使用插件对每一次交互进行安全扫描，防止恶意签名
    
-   在进行合约交互的时候，可以先查看合约是否审计通过，或使用相关审查工具对合约进行代码扫描
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->









# 以太坊

## 以太坊的应用场景

### **去中心化金融 (DeFi)**

去中心化金融是金融产品和服务的统称，任何可以使用以太坊（具有互联网连接）的人都可以访问这些产品和服务。 有了去中心化金融，市场始终开放，没有集中管理机构可以阻止付款或拒绝访问任何内容。

### **非同质化代币 (NFT)**

每个非同质化代币都是**独一无二的**。 每个非同质化代币都有不同的属性（非同质化），并且可证明其稀缺性。 这一点不同于以太币和 USDC 等其他基于以太坊的代币，在这类代币中，每个代币都是相同的且具有相同的属性（“同质化”）。 你不会关心钱包中的具体某一张钞票（或以太币），因为它们完全相同且具有同样的价值。 然而，你\_需要\_关心你拥有具体哪一个非同质化代币，因为它们都有区分于其他非同质化代币的独特属性（“非同质化”）。

### **去中心化自治组织 (DAO)**

去中心化自治组织让全世界志同道合之士开展合作，而无需信赖一位宅心仁厚的领导来管理资金和运营。 没有可以随意花钱的首席执行官，也没有能够做假账的首席财务官。 取而代之的是，融入代码的基于区块链的规则规定组织如何运作以及资金如何使用。

### **去中心化应用程序 (dapp)**

以太坊应用程序可能感觉起来和普通应用程序没什么两样，但实际上它们有一些特殊之处。一旦应用程序发布到以太坊区块链上，它就无法停止运行。这是因为以太坊网络是去中心化的，分布在全球数千台计算机上。没有人能够让运行在以太坊上的应用程序宕机，因为没有可以攻击的单一服务器。以太坊也是中立的，因此世界上任何地方的任何人都可以使用它，甚至可以连接到它并在此基础上进行修改。

## 共识机制

### POW

PoW 是一种通过计算能力竞争的共识机制。矿工使用高性能硬件解决复杂的数学问题，第一个成功解题的矿工将获得验证区块的权利，并获取区块奖励。

## POS

PoS 是一种通过资产持有权达成共识的机制。验证者根据其持有代币的数量和时间获得验证区块的资格，无需进行高耗能的计算。验证者持有的代币越多，获得验证权的概率越高。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->










# 区块链基础概念

## 区块链是什么

简单来说，就是一个公共账本，大家都可以在上面进行记录，并且这个记录会被永久写入到账本中，并且所有人都可以回顾到这个记录；用专业的来讲，区块链是一种基于**密码学原理构建**的、按时间顺序将数据区块以**链式结构**组合的、由分布式网络节点**共同维护的、不可篡改和不可伪造**的分布式账本。

## 区块链的特性

-   **去中心化 (Decentralization)**: 没有中央控制点，数据分布在网络中的所有节点（计算机）上，每个节点都拥有账本副本，也就是人人都可以参与成为区块链中的一个贡献节点。
    
-   **不可篡改性 (Immutability)**: 一旦数据（交易）被打包成区块并加入链中，就极难被修改或删除，但是存在一种叫51%攻击，也就是当一个团队拥有51%的算力，那他就可以将整个区块链按照他们所想的方向进行串改并续写。
    
-   **透明性与可追溯性 (Transparency & Traceability)**: 交易记录对参与者公开（部分数据可加密），所有节点都能验证，因为所有人都可以一起维护这个账本，也能基于链式结构进行回溯。
    
-   **匿名性 (Anonymity)**: 交易信息公开，但用户的身份可以通过加密方式隐藏，只是一串Hash，保护了隐私，但是由于和现实金融世界存在了交互，个人认为还是可以基于某些特定技术和数据分析找到交易背后的人是谁。
    

## 一个区块链怎么运作  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/daicidemeigui/images/2026-01-12-1768228552124-image.png)

# 以太坊

## 以太坊简介

以太坊（Ethereum）是一个开源的去中心化区块链平台，通过其原生加密货币以太币（Ether，简称 ETH）提供去中心化的以太虚拟机（EVM）来处理点对点合约。简单来说，以太坊就由两个部分组成，一个是以太币，一个是以太虚拟机，然后我们可以在虚拟机上进行应用开发，和我们在日常操作系统，如linux，windwos一样，他提供了一个应用平台。

## 以太坊组成

-   **以太币（**Ethereum**）：**在以太坊上执行任何操作（转账、运行智能合约）都需要支付少量以太币作为 **“ Gas 费”**。这笔费用付给维护网络的计算机（矿工/验证者），防止网络被垃圾请求堵塞，这也是区块链运作起来的主要方式，向验证者提供奖励
    
-   **以太坊虚拟机（**EVM（Ethereum Virtual Machine）**）：**是 **以太坊的“大脑”**，是专门用来**运行智能合约的虚拟计算机**。它运行在每个节点上，确保整个网络在处理代码时，**结果都一致、可信任**。
    

## **智能合约**

智能合约是存储在区块链上的数字合约，在满足预定条款和条件时自动执行。他通常用于自动执行协议，以便所有参与者都可以立即确定结果，而无需任何中介机构参与，也无任何时间损失。它们还可以自动化工作流，在满足预定条件时触发下一个动作。

## 智能合约应用场景

-   **DeFi**：自动借贷、去中心化交易所。例如，你存款到一个借贷合约中，它会自动将你的资金借出，并把利息按秒计算分给你。
    
-   **NFT**：数字艺术品和收藏品的所有权管理和交易，其核心就是一个定义了所有权创建和转移规则的智能合约。
    
-   **游戏**：游戏道具真正归玩家所有，可以在链上市场自由交易，规则由合约保证。
    
-   **供应链**：商品从生产到配送，每一步信息上链，到达某个节点后，款项自动支付给上一环节。
    
-   **DAO**：去中心化自治组织的所有章程和财务规则都由智能合约管理，投票通过后自动执行。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
