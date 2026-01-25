---
timezone: UTC+8
---

# ShihaoZhou-NEU

**GitHub ID:** ShihaoZhou-NEU

**Telegram:** @Ox221Eight

## Self-introduction

Web3 实习计划 2025 冬季实习生
全栈开发 -> 进入Web3

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
# 学习计划

## Solidity学习

-   ⭕️在 remix 中运行 Solidity by Example | 0.8.26 Basic 部分的代码
    
    -   [**\[GitHub repository\]**](https://github.com/ShihaoZhou-NEU/Solidity_Learning)
        

## 零知识证明基础：证明什么、向谁证明？

\### 1. Prover / Verifier 模型

零知识证明通常用两个角色来描述：

\- **证明者（Prover）**：掌握某个秘密信息，希望向他人证明自己「确实知道」这个秘密；

\- **验证者（Verifier）**：希望确认某个断言是真的，但不想（或不允许）知道该秘密本身。

\### 2. Statement / Witness：你在“证明”什么？

我们把「要被证明的陈述」称为 **语句（statement）**，把证明者掌握的秘密称为 **见证（witness）**。

在投票场景中，一个典型的语句会是：“我确实是这场投票中被授权的选民之一，并且我投出的票是一个合法选项。”

而见证则包括：

\- 你的私有身份秘密（如某个随机生成的 `identitySecret`）；

\- 证明你是选民集合成员所需的 Merkle 路径；

\- 你选择的具体投票选项等私密数据

\### 3. 零知识证明必须满足的三个性质

形式上，一个零知识证明系统需要同时满足三个核心性质：

1\. **完备性（Completeness）**

如果语句是真的，并且证明者与验证者都按照协议诚实地执行，那么验证者最终一定会被说服。

2\. **可靠性（Soundness）**

如果语句是假的，那么任何试图伪造证明的「作弊」证明者，都几乎不可能骗过诚实的验证者。

3\. **零知识性（Zero-Knowledge）**

验证者在整个证明过程中，除了“语句是真的”这一事实之外\*\*不会获得任何额外信息\*\*。

换句话说，验证者无法从证明内容中，还原出证明者所掌握的秘密。

在投票系统中：

\- 完备性保证「合法选民的合法投票不会被无故拒绝」；

\- 可靠性保证「非选民或者试图一人多投的人无法通过验证」；

\- 零知识性保证「验证者无法从证明中推断出你是谁、投了什么票」。

\### 交互式 vs 非交互式、zk-SNARK 与 zk-STARK

\- **zk-SNARK（Zero-Knowledge Succinct Non-Interactive Argument of Knowledge）**

\- _Succinct_：证明大小很小，验证开销与原始计算规模无关或几乎无关；

\- _Non-Interactive_：证明是单向的，不需要多轮交互；

\- 已被广泛用于隐私交易、zk-Rollup 等场景。

\- **zk-STARK（Zero-Knowledge Scalable Transparent Argument of Knowledge）**

\- 不依赖可信初始化（trusted setup）；

\- 更适合大规模、复杂计算的场景，但证明大小通常更大。

\## Error

| 特性 | `require` | `revert` | `assert` |

| :----------- | :----------------- | :--------------------- | :--------------------- |

| **主要用途** | 输入验证和前置条件 | 复杂的条件检查 | 内部一致性和不变量 |

| **Gas 处理** | 退回剩余 Gas | 退回剩余 Gas | **不退回** Gas |

| **错误消息** | 支持字符串 | 支持字符串或自定义错误 | 无错误消息 |

| **使用时机** | 函数开始时 | 代码任何位置 | 函数结束时或不变量检查 |

| **失败含义** | 外部输入错误 | 执行条件不满足 | **合约存在 bug** |

\## 什么是事件（Event）？

事件是智能合约存储在区块链上的日志，可以被外部监听但无法被合约直接读取。它们就像是合约发出的"广播通知"。

\- **Gas 成本低**：比存储数据便宜得多

\- **外部可监听**：DApp 前端可以实时监听

\- **历史可查询**：可以查询过去的事件

\- **数据不可变**：一旦记录，无法修改

\### `indexed` 参数

\- 最多可以有 **3 个** `indexed` 参数

\- `indexed` 参数可以被高效地过滤/搜索

\- 非 `indexed` 参数存储在日志数据中，查询较慢

\`\`\`solidity

// indexed vs 非 indexed 的区别

event OrderPlaced(

address indexed buyer, // 可高效过滤：所有 buyer = X 的订单

uint256 orderId, // 不可高效过滤

uint256 amount, // 不可高效过滤

uint256 indexed timestamp // 可高效过滤：特定时间段的订单

);

\`\`\`

\### 最佳实践

\`\`\`Solidity

// 遵循 ERC 标准

contract ERC20Compliant {

// ERC20 标准事件

event Transfer(address indexed from, address indexed to, uint256 value);

event Approval(address indexed owner, address indexed spender, uint256 value);

// 可选：额外的自定义事件

event TokensMinted(address indexed to, uint256 amount);

event TokensBurned(address indexed from, uint256 amount);

}

\`\`\`

\## 接口遵循标准规范（ERC标准）

接口是实现和遵循ERC标准的关键：

\`\`\`solidity

// ERC20接口

interface IERC20 {

// 必需函数

function totalSupply() external view returns (uint256);

function balanceOf(address account) external view returns (uint256);

function transfer(address to, uint256 amount) external returns (bool);

function allowance(address owner, address spender) external view returns (uint256);

function approve(address spender, uint256 amount) external returns (bool);

function transferFrom(address from, address to, uint256 amount) external returns (bool);

// 事件

event Transfer(address indexed from, address indexed to, uint256 value);

event Approval(address indexed owner, address indexed spender, uint256 value);

}

// ERC721接口（NFT标准）

interface IERC721 {

function balanceOf(address owner) external view returns (uint256 balance);

function ownerOf(uint256 tokenId) external view returns (address owner);

function safeTransferFrom(address from, address to, uint256 tokenId) external;

function transferFrom(address from, address to, uint256 tokenId) external;

function approve(address to, uint256 tokenId) external;

function getApproved(uint256 tokenId) external view returns (address operator);

function setApprovalForAll(address operator, bool approved) external;

function isApprovedForAll(address owner, address operator) external view returns (bool);

}

\`\`\`
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->



# 学习计划

✅️体验 My first zk vote

## Solidity学习

-   ⭕️在 remix 中运行 Solidity by Example | 0.8.26 Basic 部分的代码
    
-   ✅️完成挑战 Challenge #0 - Tokenization
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->





# 学习计划

## Solidity学习

-   ⭕️在 remix 中运行 Solidity by Example | 0.8.26 Basic 部分的代码
    
    -   [**\[GitHub repository\]**](https://github.com/ShihaoZhou-NEU/Solidity_Learning)
        
-   ⭕️SpeedRun
    
-   ✅️Ethernaut
    

## dApp 回顾

-   ✅️Colearning
    
-   ✅️dApp 分享
    

**Foundry 与 Hardhat 对比**：Foundry 适合高级开发者，支持 Solidity 原生测试；Hardhat 部署流程更友好，拥有更多 TypeScript 测试支持。目前两者均可选择，建议结合 AI 工具进行开发。

部署网络：优先选择 Layer2 网络（如 Arbitrum、Base），部署成本仅需几美分，远低于以太坊主网（曾高达 1500 美元）。

学习路径：先完成 Speedrun Ethereum 挑战，掌握核心概念；再研究 Uniswap、OpenZeppelin 等现有协议的智能合约代码，理解实际应用逻辑。

实践操作：使用 Scaffold-Eth 框架 + AI 工具快速搭建项目（如在 Base 网络部署代币并发送给好友，或在 Uniswap 创建流动性池），通过实践深化理解。

行业动态：关注 Crypto Twitter，跟踪 ERC-804、X402 等支付相关协议趋势，了解 AI 代理在链上经济中的应用。

人工智能与链上经济融合：AI 将加速项目开发，催生 “智能代理经济”，更多高效应用将被快速落地。

机构与国家层面 adoption：谷歌、微软等企业及部分国家可能会采用去中心化区块链作为第三方中介，实现资产上链与合规交易。

技术升级：量子抗性将成为重要研发方向，以应对量子计算对现有加密算法（如 ECDSA）的冲击。

数据主权：铁打的数据，流水的网络

智能合约（裁判）

​ 自动执行，谁也赖不掉

区块链（账本）

​ 公开透明

Token（燃料）

Hardhat Ignition + Foundry + Next.js + Wagmi 混合架构

Hardhat：灵活插件生态，脚本管理，集成测试

​ Hardhat Keystore：存储私钥

Foundry：毫秒级编译，Solidity 单测，模糊测试（Fuzzing）

混合栈：本地极速编译 + 生产级部署流程

前端架构：Next.js + Wagmi
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->







# 学习计划

1.  ✅️[Gas 优化](https://github.com/ShihaoZhou-NEU/Solidity_Learning/tree/main/Homework)
    
2.  ✅️[合约安全](https://github.com/ShihaoZhou-NEU/Solidity_Learning/tree/main/Homework)
    
3.  021教材
    
    -   第三章
        

| 安全问题 | 核心攻击方式 | 主要风险 | 核心防范方案 |
| --- | --- | --- | --- |
| 预言机操纵 | 操控价格等外部数据源输入 | 清算、套利造成资金损失 | 使用去中心化预言机（如Chainlink），结合多源验证与TWAP |
| 整数溢出/下溢 | 数学运算超出类型范围，绕开检查 | 代币增发、余额错误等逻辑漏洞 | 使用Solidity 0.8+内置检查；unchecked块需严谨审查 |
| 权限控制缺失 | 调用未受保护的关键函数 | 合约被恶意接管或破坏 | 为函数添加onlyOwner等修饰符；使用成熟的权限控制库 |
| 未初始化代理 | 直接调用未初始化的可升级合约 | 攻击者成为管理员，接管合约 | 在构造函数或初始化函数中锁定初始化状态；使用透明代理/UUPS标准 |
| 前置交易/三明治攻击 | 通过调整Gas等手段，抢在目标交易前后下单 | 用户交易滑点增大，资金被套利 | 设置更保守的滑点；使用抗MEV策略（如承诺-揭示方案） |
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->








# 学习计划

## 021学习以太坊

-   ⭕️第3章
    

## Solidity学习

-   ⭕️在 remix 中运行 Solidity by Example | 0.8.26 Basic 部分的代码
    
-   \[\[GitHub repository\]\]([https://github.com/ShihaoZhou-NEU/Solidity\_Learning](https://github.com/ShihaoZhou-NEU/Solidity_Learning))
    

## 三种主要的数据位置对比

| 关键字 | 存储位置 | 生命周期 | 成本 | 主要用途 |
| --- | --- | --- | --- | --- |
| storage | 区块链状态（永久存储） | 永久，直到被修改 | 极高 | 存储合约的状态变量（如你代码中的 arr）。 |
| memory | 函数调用的临时内存 | 仅在函数调用期间 | 中等 | 用于函数内的临时变量、函数参数和返回值。 |
| calldata | 交易调用数据（只读） | 仅在函数调用期间 | 最低 | 用于不可变的函数外部（external）调用参数。 |

### 函数中的 memory 使用

在函数 `function getArr() public view returns(uint256[] memory)` 中，`memory` 明确了两件事：

NaN.  **返回值的位置**：它告诉编译器，函数返回的 `uint256[]` 数组**不是一个指向永久存储的引用**，而是一个**全新的、独立的副本**，这个副本存放在临时内存中，准备返回给调用者。
      
NaN.  **数据复制行为**：当执行 `return arr;` 时，实际发生的是将 `storage` 中的状态数组 `arr` **完整地复制一份**到 `memory` 中，然后返回这个内存中的副本。
      

### **为什么这里必须用** `memory`**？**

因为函数的返回值需要被外部（如另一个合约或你的前端）接收和使用。外部调用者无法直接访问你的合约的 `storage`（这是每个合约私有且昂贵的永久存储）。因此，必须通过 `memory` 创建一个临时的、独立的副本传递出去。如果尝试将返回类型写成 `uint256[] storage`，编译器会报错。

```
 // 创建一个放在 memory 的数组，这个数组一定要是固定长度的
 uint256[] memory a = new uint256[](5);
 ​
 uint256[][] memory b = new uint256[][](2);
 // 禁止的行为：以下操作在 memory 数组中都是不允许的，会导致编译错误
 // tempArr.push(30);     // 错误：memory数组不能使用push
 // tempArr.pop();        // 错误：memory数组不能使用pop
 // tempArr.length = 5;   // 错误：memory数组长度不可变
 ​
```

## assert 的使用

`assert` 是 **Solidity 的断言检查函数**，用于**验证内部逻辑**。当条件不满足时，它会**消耗全部交易 Gas 并回滚所有状态更改**，表明合约存在应修复的程序错误。

### 🛠 基本语法与效果

solidity

```
 assert(bool condition);
```

-   **条件为** `true`：程序正常执行。
    
-   **条件为** `false`：交易会**立刻终止**，触发“Panic”错误（错误码 `0x01`），所有状态变更被撤销，并且**已消耗的 Gas 不予退还**。
    

### 🔍 `assert` 与 `require` 的核心区别

在 Solidity 中，`assert` 常与另一个检查函数 `require` 对比，理解它们的差异至关重要：

| 特性 | assert | require |
| --- | --- | --- |
| 设计目的 | 验证内部不变性（Invariants），捕捉程序 BUG。 | 验证输入和外部条件，确保操作 前提有效。 |
| Gas 处理 | 消耗所有剩余 Gas，绝不退还。 | 退还所有剩余 Gas（EIP-3298 后）。 |
| 错误标识 | 触发 Panic 异常（如 0x01）。 | 触发 Error 异常，可附带自定义错误信息。 |
| 使用场景 | 检查绝不应该发生的、违反合约内部逻辑的情况。 | 检查用户输入、合约状态、调用权限等。 |

### 📝 何时使用 `assert`（最佳实践）

`assert` 应用于那些**理论上永远不应该失败**的条件。如果失败了，意味着智能合约代码本身存在需要修复的错误。

**典型场景包括：**

NaN.  **不变量检查**：例如，在资金转账后，合约的总余额应保持不变。
      
      ```
       uint256 initialBalance = address(this).balance;
       // ... 一些转账操作 ...
       assert(address(this).balance == initialBalance); // 检查守恒
      ```
      
      > 在 Solidity 中，`this` 是一个**指向当前合约实例的引用**。`address(this).balance` 这行代码的意思是：**“获取当前合约地址上的以太币（ETH）余额。”**
      > 
      > 我们可以分两步来理解：
      > 
      > NaN.  `this`：关键字 `this` 代表**当前正在执行的这份智能合约本身**，它是一个合约类型的对象。
      >       
      > NaN.  `address(this)`：这是将合约类型显式转换为 `address` 类型。在以太坊中，**每个部署到链上的合约都有一个独一无二的地址**，就像你的银行账户号一样。这个转换让我们能获取到合约的地址。
      >       
      > NaN.  `.balance`：这是 `address` 类型的成员属性，表示**该地址所持有的以太币（ETH）数量**，单位是 **wei**（1 ETH = 10^18 wei）。
      >       
      
NaN.  **运算后验证**：在数学运算后（尤其是可能溢出的旧代码中），验证结果在合理范围内。
      
      ```
       uint256 result = a + b;
       assert(result >= a); // 检查加法未溢出（Solidity 0.8+ 已内置溢出检查）
      ```
      
NaN.  **访问数组后**：验证索引在访问后仍处于边界内（尽管通常在访问前用 `require` 检查）。
      
      ```
       assert(index < array.length); // 确认索引绝对有效
      ```
      
NaN.  **开关状态验证**：确保一个应永远为 `true` 或 `false` 的状态变量没有异常变化。
      
      ```
       assert(isLocked == false); // 此时合约绝不应该被锁定
      ```
      

### 🎯 总结与建议

简单来说：

-   **用** `require` **来保护合约免受“外部错误”**（如无效的用户输入）。
    
-   **用** `assert` **来发现并杜绝“内部错误”**（即代码逻辑的 BUG）。
    

由于 `assert` 失败时代价高昂（不退款），且意味着合约存在根本性逻辑问题，因此在实际生产代码中，`assert` 的使用应非常谨慎，且通常远少于 `require`。

## `assembly` 是什么？

`assembly { ... }` 在 Solidity 中称为**内联汇编**。它允许你在智能合约中直接编写 **EVM 的底层操作指令**，从而绕过 Solidity 的高级语法和安全性检查，以追求极致的控制和效率。

| 特性 | 高级 Solidity 代码 | 内联汇编 (assembly) |
| --- | --- | --- |
| 抽象层级 | 高级，人类可读 | 低级，接近机器指令 |
| 安全性 | 有自动的类型、溢出等安全检查 | 无安全网，开发者全权负责 |
| Gas 效率 | 较好，但编译器可能添加额外操作 | 通常更高，可手动优化掉冗余操作 |
| 主要用途 | 99% 的常规合约逻辑 | 极致的 Gas 优化、复杂位操作、实现 Solidity 无法表达的特定操作 |

### `:=` 是什么？

在 `assembly` 块内部，`:=` 是**汇编语言的赋值操作符**。

-   **作用**：它将右侧表达式的计算结果**直接赋值**给左侧的变量或栈槽。
    
-   **关键区别**：它**不进行任何类型转换或溢出检查**，赋值完全由你编写的指令决定。
    

**类比理解：**

-   Solidity 中的 `=` 就像**秘书帮你处理文件**：会检查格式、登记备案。
    
-   汇编中的 `:=` 就像**你自己直接把文件塞进档案柜**：速度快，但放错地方或放错了文件也没人提醒你。
    

## 如何使用“用户定义值类型”（User Defined Value Type, UDVT）

**增强代码的类型安全性和可读性**，同时**不牺牲任何 Gas 效率**。

它通过对比“使用 UDVT”和“不使用 UDVT”的两种实现，生动地展示了其核心价值。

### 🎯 核心教学目的

NaN.  **防止低级错误**：利用编译器在**编译阶段**就杜绝 `Duration`（时长）和 `Timestamp`（时间戳）这两种虽然底层都是 `uint64`、但语义完全不同的值被意外混淆使用。
      
NaN.  **语义化类型**：让代码“自文档化”。看到 `Duration` 类型，立刻明白它代表一个时间段，而不是一个普通的数字。
      
NaN.  **零成本抽象**：在保持底层高效位操作（汇编）的同时，提供高级别的类型安全。
      

### 🔍 UDVT 到底是什么？有什么用？

**UDVT** 是 Solidity 0.8.8 引入的特性。它允许你为现有的基本值类型（如 `uint256`, `address`）创建一个**具有新名称的别名**。这个新类型在运行时与基础类型完全一致（无额外Gas开销），但在编译时被视为**独立的类型**，无法直接混用。

**核心用途：用编译器强制区分语义。**

| 场景（无 UDVT） | 问题 | 使用 UDVT 的解决方案 |
| --- | --- | --- |
| function setLock(uint64 duration, uint64 timestamp) | 容易传参顺序错误：setLock(timestamp, duration)，编译通过，逻辑错误。 | function setLock(Duration duration, Timestamp timestamp)。传参顺序错误会导致编译失败。 |
| uint64 balance; 和 uint64 tokenId; | 都是 uint64，可能误赋值：balance = tokenId;。 | 定义为 type Balance is uint64; 和 type TokenId is uint64;。误赋值会编译失败。 |
| address userAddr 和 address contractAddr | 都是 address，可能把用户地址误传给需要合约地址的函数。 | 定义为 type UserAddress is address; 和 type ContractAddress is address;。 |

### 用`wrap`对 UDVT 赋值

当你写下 `type Duration is uint64;` 这行代码时，Solidity 编译器会自动为 `Duration` 类型生成以下两个函数：

| 自动生成的函数 | 作用 | 类比 |
| --- | --- | --- |
| Duration.wrap(uint64 value) pure returns (Duration) | “包装”：将一个原始的 uint64 值升级（转换）为安全的 Duration 类型。 | 把散装咖啡豆 封装 进一个贴有“Duration”标签的罐子。 |
| Duration.unwrap(Duration duration) pure returns (uint64) | “解包”：将一个 Duration 类型降级（转换）回其底层的原始 uint64 值。 | 从“Duration”罐子里 倒出 原始的咖啡豆。 |

### 总结

用UDVT是为了防止人为编程犯错

分类存储到不同的 libraries，让整个项目更有结构

使用 import 和 using 来调用

## Data Locations

### 🍳 厨房比喻：理解三大数据位置

| 数据位置 | 比喻 | 特点 | 类比 |
| --- | --- | --- | --- |
| storage | 冰箱里的永久食材 | 保存在区块链上，永久存在，但存取都很贵 | 从冰箱拿/放食材要花更多时间和力气 |
| memory | 砧板上的临时食材 | 只在做菜时存在，用完就丢，便宜快速 | 切菜、处理食材的工作台 |
| calldata | 外卖送来的包装食材 | 别人送来给你用的，只能看不能改，最便宜 | 密封的外卖包装，你只能打开吃，不能重新烹饪 |

### 💡 什么时候用什么？快速指南

| 你的需求 | 应该用 | 例子 |
| --- | --- | --- |
| 修改状态变量 | storage | myArray[0] = newValue;（永久改变） |
| 临时处理数据后丢弃 | memory | 计算中间结果、格式化数据 |
| 函数参数（特别是外部函数） | calldata | function process(string calldata name) |
| 返回值 | memory | returns (string memory) |

## Transient Storage

### Transient Storage vs Regular Storage：核心区别

| 特性 | 常规 Storage | Transient Storage |
| --- | --- | --- |
| 持久性 | 永久保存，直到被更改 | 仅存续于当前交易，交易结束自动归零 |
| Gas成本 | 写入 (SSTORE) 极昂贵，读取 (SLOAD) 较贵 | 写入 (TSTORE) 便宜约10倍，读取 (TLOAD) 便宜 |
| 设计目的 | 存储合约的永久状态 | 存储交易内的临时中间状态 |
| 底层指令 | SSTORE, SLOAD | TSTORE, TLOAD |
| Solidity 关键字 | 无（默认） | transient |

NaN.  **自动清零机制**：
      
      -   交易成功结束 → transient storage **自动归零**
          
      -   交易回滚（revert） → transient storage **也会归零**
          
      -   下一个交易开始时，所有 transient storage 都是初始零值
          
NaN.  **Gas 节省对比**：
      
      ```
       // 永久存储：首次写入 20,000+ Gas，后续修改 5,000 Gas
       bool permanentLock;
       permanentLock = true; // 昂贵
       permanentLock = false; // 仍需要 5,000 Gas
       ​
       // 瞬态存储：每次写入约 100 Gas（估算）
       transient bool transientLock;
       transientLock = true;  // ~100 Gas
       transientLock = false; // ~100 Gas（但通常不需要，交易结束自动清零）
      ```
      

### **兼容性与支持**：

-   **需要编译器 0.8.24+**
    
-   **需要 EVM 支持** `TSTORE`**/**`TLOAD`（Cancun 升级后）
    
-   支持的网络：以太坊主网、OP Stack 链（OP Mainnet, Base）、Arbitrum、Polygon PoS 等
    

### 结论

`transient storage` 是智能合约开发的重大优化，特别适合**重入锁、跨函数临时数据、复杂交易中间状态**等场景。它让“临时但需跨函数共享”的数据存储变得既安全又廉价，是编写高效合约的新利器。

## Remix At address

在开发过程中，可以重复拿出之前部署的合约

### 🆚 “Deploy” 与 “At Address” 对比

| 功能 | Deploy (部署) | At Address (连接地址) |
| --- | --- | --- |
| 作用 | 将新合约创建到区块链上。 | 连接一个已存在的合约。 |
| 结果 | 生成新的合约地址。 | 不产生新地址，加载现有合约。 |
| 交易 | 需要发送一笔部署交易，消耗Gas。 | 通常不立即产生交易（仅当调用写函数时才需要）。 |
| 目的 | 首次发布合约。 | 与你自己或他人已部署的合约进行交互 |

### 📝 如何操作（以连接一个已部署合约为例）

NaN.  **获取合约地址和ABI**：你需要知道目标合约的地址，并且在Remix的文件资源管理器中拥有**该合约的源代码**（用于自动生成ABI）或一个独立的ABI文件。
      
NaN.  **编译合约**：在Remix中打开并编译该合约的源代码（确保编译器版本等设置匹配）。这一步是为了让Remix知道如何与合约对话（即获得ABI）。
      
NaN.  **输入地址并连接**：
      
      -   在“部署与运行”面板，找到“At Address”按钮。
          
      -   在旁边的输入框中**粘贴目标合约地址**。
          
      -   点击 **“At Address”** 按钮。
          
NaN.  **开始交互**：如果地址和ABI正确，该合约实例会出现在“Deployed Contracts”（已部署合约）区域。你可以像操作自己部署的合约一样，查看其状态变量和调用函数。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->










# 学习计划

## Web3 实习手册

-   ⭕️智能合约开发
    

## 021学习以太坊

-   ⭕️第3章
    
-   ⭕️第4章
    

## 拓展 & 辅助理解材料

1.  ⭕️Ethereum 官网 Overview 基础内容学习
    
2.  Web2 to Web3
    

-   ⭕️Week1-Day 5: Stuck Transactions, Gas Limits, Multisigs, L2s, Lending…
    
-   ⭕️Week2-Day 1: Scripting and Smart Contracts! Providers, Signers, and Wallets
    
-   ⭕️Week2-Day 2: Reading and Writing to Contracts with Ethers.js
    

## Solidity学习

-   ⭕️在 remix 中运行 Solidity by Example | 0.8.26 Basic 部分的代码
    
-   [**\[GitHub repository\]**](https://github.com/ShihaoZhou-NEU/Solidity_Learning)
    

从最小到最大排列：

| 单位 | Wei 值 | 换算关系 | 用途场景 |
| --- | --- | --- | --- |
| Wei | 1 Wei | 最小单位 | 底层计算 |
| Kwei (Babbage) | 10³ Wei | 1 Kwei = 1,000 Wei | - |
| Mwei (Lovelace) | 10⁶ Wei | 1 Mwei = 1,000,000 Wei | - |
| Gwei (Shannon) | 10⁹ Wei | 1 Gwei = 1,000,000,000 Wei | Gas 费用 |
| Microether (Szabo) | 10¹² Wei | 1 Szabo = 1,000,000,000,000 Wei | - |
| Milliether (Finney) | 10¹⁵ Wei | 1 Finney = 0.001 ETH | - |
| Ether (ETH) | 10¹⁸ Wei | 1 ETH = 1,000,000,000,000,000,000 Wei | 交易计价 |
| Kether | 10²¹ Wei | 1,000 ETH | - |

EVM: 机制要弄明白

是Stack运行

Storage：存储成本，gas的关键，优化的关键

Call data:

定长字节，byte32

值类型

\- bool

\- uint

\- int

\- address

\- byte

引用类型

\- string

\- 数组 uint\[\]

\- bytes 动态字节数组

映射

\- mapping(address => uint256)

搜索材料

1\. memory 什么用

2\. external 与 public 什么区别

3\. view：只读

4\. pure：既不修改也不读取

5\. 事件里面的 indexed 是什么

6\. unchecked

7\. emit

8\. 为什么 approve 要 returns bool

9\. modify

10\. onlyOwner

11\. uniswap and AAVE 合约标准可以看看

12\. OpenZeppelin 合约模版

13\. 可升级合约，加proxy，使得合约可以升级，

14\. 钻石架构

\# 公共物品

非排他性的，非竞争性

solidity，hardhat

Web3中的阳光和空气

开源代码

协议安全

开发者工具/文档

公地悲剧：搭便车问题：

\- 每个人都免费使用，理性的选择是等待别人付费

\- 结果：市场失灵，高使用 ！= 高收入

机制一：委员会制度

\- 优点：简单快速，决策效率高

\- 缺点：权力集中，偏好固化与偏见，有限的知识

机制二：二次方自助（QF），人数 > 金额

\- 配捐池的资金分配有贡献者的数量决定，放大了社区的共识

机制三：回溯性资助（RetroPGF）先创造价值，后获得奖励

\- 传统模式：为“承诺”付费，风险：可能不发芽

\- 回溯模式：为“结果”付费，优势：价值已证明
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->












# 学习计划

## Web3 实习手册

-   ⭕️智能合约开发
    

## 021学习以太坊

-   ⭕️第3章
    
-   ⭕️第4章
    

## 拓展 & 辅助理解材料

1.  ⭕️Ethereum 官网 Overview 基础内容学习
    
2.  Web2 to Web3
    

-   ⭕️Week1-Day 5: Stuck Transactions, Gas Limits, Multisigs, L2s, Lending…
    
-   ⭕️Week2-Day 1: Scripting and Smart Contracts! Providers, Signers, and Wallets
    
-   ⭕️Week2-Day 2: Reading and Writing to Contracts with Ethers.js
    

## Solidity学习

-   ⭕️在 remix 中运行 Solidity by Example | 0.8.26 Basic 部分的代码
    
-   \[\[GitHub repository\]\]([https://github.com/ShihaoZhou-NEU/Solidity\_Learning](https://github.com/ShihaoZhou-NEU/Solidity_Learning))
    
    -   ✅️3 Lessons
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->















# 学习计划

## Web3 实习手册「行业知识」部分

-   ⭕️智能合约开发
    

## 021学习以太坊

-   ✅️第2章
    
-   ✅️一 ~ 四
    
-   ✅️五 ~ 十三
    
-   ⭕️第3章
    

## 拓展 & 辅助理解材料

1.  ⭕️Ethereum 官网 Overview 基础内容学习
    
2.  Web2 to Web3
    

-   ✅️Day 1 ~ Day 3
    
-   ✅️Day 4: NFTs!!! ERC20 vs ERC721, IPFS, Metadata
    
-   ⭕️Day 5: Stuck Transactions, Gas Limits, Multisigs, L2s, Lending…
    

## Solidity学习

-   ✅️Remix 使用教程
    

## 五、归档节点在数据查询中的优势

归档节点 = “国家档案馆 + 时间机器”：

全节点只知道“现在是什么样”，归档节点还能瞬间回答“历史上任意一个时刻是什么样”。

Archive Node = 全节点 + 所有历史状态快照

\- 和全节点一样，保存所有区块和交易数据

\- 额外再保存每一个区块高度的完整状态（账户余额、合约存储、状态树等），从创世块开始

普通的全节点一般只保留最近约128个区块的状态

\### 1. 即使查询任意历史状态（无需重放）

相当于直接读盘操作，就是快

\### 2. 支持按区块高度查询的高级RPC

有些 JSON-RPC 方法支持 “blockNumber 参数”

\- eth\_getBalance

\- eth\_getCode

\- eth\_getStorageAt

\- eth\_call（对历史区块执行只读调用）

\### 3. 历史分析、审计与治理的基础设施

\- 区块浏览器、数据服务商

\- 市场与链上行为分析

\- 合规与审计

\- 开发者调试与回归测试

\### 4. 代价

存储需求：十几TB，Erigon可以做到2-3TB

首次同步需要从创世开始，要数周到数月

硬件要用 NVMe SSD、较大内存和稳定高带宽

\## 六、合并后执行与共识客户端的区别

EL + CL +Engine API

执行客户端管“算账 + 状态”，共识客户端管“选块 + 投票”。

\### 1. EL：EVM + 状态 + RPC

1\. 处理交易

1\. 接收转账、部署合约、调用合约函数等交易

2\. 对交易做基本校验（签名、nonce、余额、gas等）

2\. 执行EVM操作

1\. 在EVM中逐条执行交易

2\. 计算 gas 消耗、抛出异常、更新 storage 等

3\. 维护状态与数据库

1\. 维护当前链的世界状态

2\. 负责把这些状态写入本地数据库

4\. 提供 JSON-RPC API

1\. 提供 `eth_*、net_*、web3_*` 等标准 RPC 方法，是 dApp、钱包、脚本接入以太坊的主要入口。

5\. 执行层 P2P 网络

1\. 通过 devp2p/RLPx 网络协议与其他EL交换区块、交易等数据

6\. 配合集成 Engine API

可以把执行客户端想成：\*\*“世界计算机”的\*\* \*\*CPU +\*\* \*\*内存\*\* \*\*+\*\* \*\*本地数据库\*\* \*\*+ RPC\*\* **网关**

\### 2. CL：PoS + Beacon + Finality

处理 PoS 逻辑与链头选择

1\. 运行 PoS 共识，提议/验证新区块

2\. 追踪链头与最终性

3\. 管理验证者，给予奖励 / 惩罚

4\. 共识层 P2P 网络

1\. 与执行层不一样的 P2P 网络

5\. Beacon API / 共识 RPC

1\. 有自己 HTTP/REST/gRPC 接口，可以查询 Beacon 状态、验证者信息、slashing等

\*\*一群“裁判\*\* \*\*+\*\* **选举委员会”**：负责安排谁出块、谁投票、票数到哪条链头上，什么时候给“最终盖章”

\### 3. EL + CL 之间沟通

!\[image-20260118111053507\](D:\\Coding\_Workspace\\Co-Learning\\2026\_Web3\_Winter\_Internship\\021笔记\\021 Ethereum Note 第二章.assets\\image-20260118111053507.png)

\### 4. 开发者记住

调用 `eht_*` / 部署合约 / 查状态 -> EL

质押 32 ETH 做验证者 / 关心 attestation、Finality、slashing -> CL +EL

为了模块化、易维护、多客户端升级、独立升级、安全边界

\## 七、EL与CL协同配合

执行层和共识层各自有独立的 P2P 网络，但\*\*它们之间的桥\*\*只剩下这一条Engine API，本质上就是几组方法：

\- engine\_newPayload

\- engine\_forkchoiceUpdated

\- engine\_getPayload 等。

执行客户端负责“算结果、管状态”，共识客户端负责“选哪条结果被全网认可”，两者通过 Engine API 三大动作 newPayload / forkchoiceUpdated / getPayload 互相配合 —— 这就是 Merge 之后以太坊节点内部的日常节奏。

\## 八、节点同步的目标与意义

要同步的内容是：

1\. 区块链数据（每个区块及其头信息）

2\. 状态数据（每个账户余额、合约存储、代码等）

\### 1. FullSync

从创世区块开始，下载所有区块，验证所有

归档节点的初始化

\### 2. SnapSync 快照同步

全节点的同步

节点首先下载最新区块头链，然后请求一个可信状态快照

\### 3. LightSync

\## 九、节点间交换的数据类型

\- 交易信息

\- 区块数据

\- 加密哈希

\- 账户信息

\- 公钥

\- 智能合约代码和执行数据

\- 网络状态

\- 日志、事件

知识补充

种子生成私钥，私钥生成公钥，公钥生成地址

\## 十、节点数量与去中心化保障

哪怕所有“官方 RPC / 大公司节点”都宕机，普通人也能自己拉起节点接入网络，继续验证和广播交易。

星星之火可以燎原

\> 目前公开探测到的以太坊节点，大致在 1–2 万个量级（取决于统计口径），活跃验证者接近 100 万个。这些节点和验证者分布在全球多个国家，运行着由不同团队、不同语言实现的多种客户端，既有家庭宽带的自建节点，也有云服务托管节点。即便某个国家、某家云厂商、某个客户端实现出现问题，其他地区、其他云、其他客户端仍然可以继续出块和验证。正是这种在 **软件实现、地理位置、基础设施和经济主体上的高度分散**，让以太坊在现实世界中尽可能接近“无需许可、抗审查、可自我验证”的去中心化网络。

\## 十一、Gossip

1\. 局部随机转发

2\. 邻居继续 局部随机转发

\- 指数级扩散

3\. 副作用：一定会有重复消息

\- 缓存最近见过的消息ID

\- 设置传播 TTL / 跳数限制

\> Gossip 协议相当于以太坊的“去中心化广播系统”： 它让每个节点只需和少数邻居聊天，就能在几秒内把新交易、新区块及 PoS 投票散播到整个网络，从而在没有中心服务器的前提下，实现高可用、高容错、可扩展的全网信息同步。

\## 十二、个人节点的搭建与开发配置

!\[image-20260118123148070\](D:\\Coding\_Workspace\\Co-Learning\\2026\_Web3\_Winter\_Internship\\021笔记\\021 Ethereum Note 第二章.assets\\image-20260118123148070.png)

开发用全节点：执行层用 Snap Sync，共识层开 checkpoint sync，就够了

安全提醒：不要在公网裸暴露未鉴权的 RPC，尤其是有 personal, admin 等危险 API 的配置；最安全做法是只在内网/[localhost](http://localhost) 暴露，或者放在反向代理后面做认证和限流。

!\[image-20260118123819607\](D:\\Coding\_Workspace\\Co-Learning\\2026\_Web3\_Winter\_Internship\\021笔记\\021 Ethereum Note 第二章.assets\\image-20260118123819607.png)

\## 十三、不跑节点的数据分析

速记表：

不跑节点搞数据分析的几种路线：

1\. SQL 平台：Dune / Flipside

\- 优点：零运维，直接写 SQL + 画图

\- 适合：分析师、研究员、想快速产出 Dashboard

2\. BigQuery 公共数据集

\- 优点：企业级数仓，能跟自家业务数据 Join

\- 适合：已经有 GCP / BI 体系的团队

3\. 区块浏览器 API（Etherscan 等）

\- 优点：脚本接入简单，免费额度够学习与小项目

\- 适合：Python / JS 轻量分析、监控脚本

4\. Web3 数据 API / Indexing 服务

\- 优点：拿到的是“已经整理好的业务视图”

\- 适合：小团队做产品、需要多链、但不想运维

5\. 混合方案：历史用 BigQuery/Dune，实时用托管 RPC

\- 优点：接近“自己有归档节点”的能力，但不自己跑机器

\- 适合：认真做数据产品，却又不想从硬件和节点运维起家的团队

# ERC-7962

!\[image-20260118202056472\](D:\\Coding\_Workspace\\Co-Learning\\2026\_Web3\_Winter\_Internship\\课程笔记\\6. ERC-7962.assets\\image-20260118202056472.png)

`keyHash = keccak256(publicKey)`

更强隐私（地址不上链）

更佳用户体验（免ETH / Gas 代付）

更高灵活性（批量、代付、抽象账户）

与 ERC-5564（Stealth Address）理念一致
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->


















# 学习计划

## Web3 实习手册「行业知识」部分

-   ⭕️智能合约开发
    

## 021学习以太坊

-   ⭕️第2章
    
-   ✅️一、以太坊节点与客户端
    
-   ✅️二、节点间的连接与通信方式
    
-   ✅️三、全节点、轻节点、归档节点
    
-   ✅️四、开发人员运行全节点的必要性
    

## 拓展 & 辅助理解材料

1.  ⭕️Ethereum 官网 Overview 基础内容学习
    
2.  Web2 to Web3
    

-   ✅️Day 1 ~ Day 3
    
-   ✅️Day 4: NFTs!!! ERC20 vs ERC721, IPFS, Metadata
    
-   ⭕️Day 5: Stuck Transactions, Gas Limits, Multisigs, L2s, Lending…
    

## Solidity学习

-   ⭕️Remix 使用教程
    

# 第二章

## 一、以太坊节点与客户端

Node: 运行客户端 + 连接到其他节点

Client：有开源软件，按照协议规则验证数据、同步区块和状态、帮助网络保持安全

The Merge之后，由：

\- 执行客户端（Execution Client， EL）

\- 共识客户端（Consensus Client， CL）

\- 如果要做验证者还需要跑，验证者客户端（Validator）：作为共识客户端的“插件”

\### 执行客户端：

负责链上“业务逻辑和状态”

\- 监听网络上广播来的交易与区块

\- 在EVM中执行交易和智能合约

\- 维护当前链的状态数据库（账户余额、合约存储等）

\- 提供JSON-RPC接口（eth\_\*），给dApp、钱包、脚本调用

\- 通过自己的P2P网络转发交易和区块头、区块体

e.g.

\- Geth（go-ethereum）

\- Nethermind

\- Besu

\- Erigon

\- Reth

\### 共识客户端

负责PoS共识

\- 维护Beacon Chain视图和 Fork Choice 规则（LMD-GHOST等）

\- 在每个slot选择谁来提议区块，并对区块进行投票（attestation）

\- 跟踪验证者集合、质押余额、惩罚等共识

\- 审核EL的结果

e.g.

\- Lighthouse

\- Prysm

\- Teku

\- Nimbus

\- Lodestar

\### 验证者客户端

管理验证者密钥

轮到自己时候，提议新的区块

在每个slot为看到的区块投票 attestation

\### Engine API

一个专门给EL <-> CL用的JSON-RPC 接口

以太坊节点 = 一台电脑 + 两个必备客户端 + （可选）一个验证者客户端：

• 执行客户端负责“算交易、管状态、对外提供 RPC”；

• 共识客户端负责“跑 PoS、选区块、投票达成共识”；

• 它们通过 Engine API 的“专线电话”互相协作，整个节点再通过 P2P。

!\[image-20260117223928674\](D:\\Coding\_Workspace\\Co-Learning\\2026\_Web3\_Winter\_Internship\\021笔记\\021 Ethereum Note 第二章.assets\\image-20260117223928674.png)

\## 二、节点间的连接与通信方式

节点新进网络，先找 boot nodes，这些 nodes 长期在线，对外公布地址的节点

\### Discovery 协议

先给引导节点 Ping - Pong

再送 FindNode 请求，对方回复 Neighbors 或 Nodes

这就是 Kademlia 风格的 DHT（分布式哈希表）发现机制

UDP + Kademlia 发现 Nodes

\### 建立安全连接

TCP建立安全稳定，可重传的连接

以太坊在 TCP 上跑的是 devp2p 协议栈，其底层加密与多路复用由 RLPx 负责：

1\. 通过握手交换密钥，建立加密会话（防窃听、防篡改）

2\. 在这条加密的 TCP 通道上，复用多种自协议 （capabilities），比如：

\- eth/66，eth/68：同步区块、传播交易（执行层）

\- les：为轻客户端提供服务

以太坊的 P2P 网络主要由 UDP 的 Node Discovery 协议 + TCP 上的 RLPx/devp2p 协议 组成

\#### 如何将区块传播全网

两种方式：

1\. Gossip（广播式传播）

2\. 点对点请求-响应（拉取指定数据）

\#### Gossip

一传十，十传百，核裂变

优点：

\- 不需要中心服务器，完全去中心化，自组织

\- 鲁棒性好，不会手局部故障影响

\- 几秒钟的级别传播全网

在以太坊中：

\- 执行层：通过devp2p 上的eth/\* 子协议，gossip交易和传播区块

\- 共识层：有自己的 P2P 网络和 gossip 协议，用来广播 Beacon block、attestation等共识信息

\#### 请求-响应，按需拉取历史区块、状态

Gossip主要是传播 最新消息，请求-响应 可以发起精确请求。

\## 三、全节点、轻节点、归档节点

**全节点 = 当前状态 + 全部区块（但老状态会被“裁剪”）；**

**归档节点 = 全节点 + 从创世到现在的“所有历史状态”；**

**轻节点 = 只存区块头，需要时向全节点要具体数据。**

以太坊在 PoS 后专门为轻客户端设计了 同步委员会（sync committee）协议，让轻客户端只需验证 512 个验证者对区块头的签名，就能安全地追踪链头，进一步降低资源消耗。

\> **关键点：**

\>

\> 轻节点 = “只看目录 + 零信任校验”：

\>

\> 不存全书，只存区块头和必要证明，但依然可以做到“不完全信任别人、

\>

\> 自己核对关键部分”。

!\[image-20260117230548458\](D:\\Coding\_Workspace\\Co-Learning\\2026\_Web3\_Winter\_Internship\\021笔记\\021 Ethereum Note 第二章.assets\\image-20260117230548458.png)

\## 四、开发人员运行全节点的必要性

RPC = Remote Procedure Call，远程过程调用

简单来说，就是一个程序（或客户端）可以像调用本地函数/方法一样，向远端服务器节点（或服务）发出请求，让对方执行某个过程（procedure／方法／函数），并返回结果。客户端自己并不关心这个过程实际运行在哪台机器或在什么网络之中。

可以调用这个 RPC 来

\- 查询区块、交易、账户余额、智能合约状态等数据

\- 提交新的事务到网络

\- 调用智能合约的方法

\- 监测交易确认状态等

怪不得说这 RPC 都用同一个节点就会有问题，这问题真是大大的，去中心化又变成中心化了。

**谁在给你的 dApp 喂数据，谁就在“半控”你的安全和用户。 自己跑全节点 = 不求人 + 更安全 + 更隐私 + 更去中心化。**

\### 1. 不泄露用户隐私：地址 & IP

默认使用公共RPC的钱包，会让提供商把 IP 和 链上地址 关联起来

所以使用自己的RPC会更安全，更隐私

\### 2. 不被卡脖子：抵御 RPC 层面的“软审查”

运行自己的节点是获得 抗审查性、可用性和主权（sovereignty） 重要一步。

别人可以限流，封锁IP，可以被拒绝广播，服务商g我就g

又变成了中心化

\### 3. 不要信任，要验证：自己验证每一笔交易和区块

\### 4. 参与以太坊

\### 5. 为dApp和内部系统提供稳定的“数据底座”

\- 自定义 RPC 接口：根据业务需要配置限流、日志、鉴权、白名单等

\- 更低延迟

\- 可以在节点上挂：

\- 索引服务

\- 内部风控 / 数据仓库流水

\- 各部门使用的分析面板

自托管工具和服务

\### 6. 归档节点支撑分析、审计和合规

\### 7. 升级和分叉选择权

协议升级

\### 8. 配置自由

选用不同客户端

\- 性能需求

\- 语言栈

\- 企业特性

\- 自己控制日志留存、监控指标、访问控制策略、满足合规或内部审计要求

\- 为共识、执行层做特殊配置（比如接入本地 MEV 流，或对接专用L2、跨链组件）

\> 开发者和机构运行全节点，是为了拿回三件东西：

\>

\> ① 自己验证、自主管理的安全与隐私；

\>

\> ② 抗审查、抗单点故障的基础设施韧性；

\>

\> ③ 在以太坊协议和应用层上的话语权与自主权。 这既是为自己负责，也是为整个以太坊网络“增肌”。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->




















# 学习计划

## 学习内容：

Web3 实习手册「行业知识」部分

-   ✅️区块链岗位全景图
    
-   ⭕️智能合约开发
    
-   021 学习以太坊第 1 章
    
-   ✅️六、以太坊 dApps 的概念与应用
    
-   ✅️七、以太坊的去中心化实现机制
    
-   ✅️八、网络结构的开放性与参与机制
    
-   ✅️九、行业应用案例：金融、游戏与社交
    
-   ✅️十、生态系统创新：DeFi、NFT 与 DAO
    
-   ✅️十一、社区与开发生态扮演的角色
    
-   ⭕️021学习以太坊第2章
    

拓展 & 辅助理解材料

1.  ⭕️Ethereum 官网 Overview 基础内容学习
    

-   ✅️Day 1 ~ Day 3
    
-   ✅️Day 4: NFTs!!! ERC20 vs ERC721, IPFS, Metadata
    
-   ⭕️Day 5: Stuck Transactions, Gas Limits, Multisigs, L2s, Lending…
    

\## 什么是dApp

去中心化应用程序，顾名思义，在去中心化网络上的应用

1\. 链上的智能合约（后端逻辑）

2\. 链下的前端页面、客户端（用户界面）

​ 智能合约：一组开放的，透明的，永远在线的API

后端运行在L1 或 L2上的EVM

前端就是传统Web2的技术，可以在传统的服务器上，也可以托管在IPFS、Arweave等去中心化的存储

\### 核心特征

1\. 去中心化：无法被单一机构控制，不存在关停和删库

2\. 确定性：同一个合约在不同的节点上执行，给同样的输入，输出永远一样

3\. 图灵完备

4\. 隔离性（沙盒）：

\- 单个合约逻辑挂了，只会让它自己的调用失败（回退），不会影响整个Ethereum net

\- 全网节点只是共同重放并验证同一套字节码

\### 优势何在

1\. 零停机时间

\- 网在合约在

2\. 隐私、匿名性（相对Web2）

\- 与dApp交互，只需要一个公钥私钥对，不需要实名注册

\- 链上是透明伪匿名

3\. 抗审查

\- 很难封杀一个dAPP

4\. 数据完整性

\- 依托密码学和共识机制，链上数据一旦写入就几乎不可篡改

\- 恶意行为无法事后伪造交易或随意修改历史状态

5\. 无需信任、可验证行为

\- 智能合约可以公开审计，执行路径可重放

\- 用户不用相信运营方，可直接相信代码

\### 可以做什么呢？

\- 金融DeFi

\- 资产与内容

\- DAO

\- 基础设施

\- 其他实验性应用

\- 去中心化社交

\- 链上存证

\- 供应链追踪

\- 链上订阅与会员系统

用智能合约做后端，用以太坊做数据库和结算层的应用程序

\### 缺点

\#### 维护困难

\- 上链后不能修改

\- 即使有漏洞也不行

\- 预先设计好升级代理模式（proxy、UUPS）

\- 部署合约+迁移数据、资产

\- 上线前要\*\*严谨\*\*设计与审计

\#### 性能开销与扩展难度

\- L1 上每个全节点都要执行所有交易，有性能上限

\- 扩容主要靠Rollup +Dencun（EIP-4844）升级，把绝大部分的用户交易搬到L2（Arbitrum，Optimism、Base等），L2 TPS已经可以达到每秒数千甚至上万，而L1更像结算与数据可用层

\#### 网络拥堵与Gas成本

\- 需要再合约设计的时候想到Gas友好（减少无谓的存储和循环，也就是时间和空间复杂度）

\- 前端提示用户Gas情况，必要时支持在多条L2之间切换

\#### 用户门槛高

\- 钱包都搞不明白的大有人在

\- 体验也会不好，如果私钥泄露

\#### 有“再中心化”倾向

\## 去中心化实现机制

1\. 共识与出块（技术、经济层）

PoS，32ETH质押，表现好就给利息，表现不好就罚钱

Pectra 升级之后，可以质押不止32，可以到2048ETH

2\. 节点与网络（技术层）

任何人都可以运行全节点或轻节点，DevP2P

\- 发现层（Discovery Stack）基于UDP，用于发现和加入新节点

\- DevP2P通信层 基于TCP，负责在已连接节点间传递区块、交易等实际数据

3\. 客户端多样性（实现层）

多客户端

\- 执行层客户端（EL）：Geth、Nethermind、Besu、Erigon、Reth 等；

\- 共识层客户端（CL）：Lighthouse、Prysm、Teku、Nimbus、Lodestar 等。

不同团队在不同语言和代码库下实现同一份协议规格。

4\. MEV 与 PBS （市场结构层）

\> 最大可提取价值：这是验证者最复杂也最可观的潜在收入来源。简而言之，MEV是验证者通过调整、插入或审查区块内交易顺序所能获得的额外利润（例如，套利交易、清算等）。专业验证者通过运行MEV-Boost等中继服务，可以捕获这部分价值，其收益有时甚至超过前两者之和。

会推高专业化、集中化的矿工、验证者。为缓解，以太坊引入了 Proposer-Builder-Separation （PBS，提议者-建构者分离）

5\. Rollup中心路线与数据去中心化（扩容、职责分离）

提出L2，Dencun升级，引入blob（临时数据块）

做到：

\- L1 更小，更简单，更去中心化

\- L2 追求的是执行扩容和多样化的应用场景

\- 未来像 Danksharding、PeerDAS 等方案则继续提升数据可用性、而不强迫每个节点处理所有细节

6\. 治理（社会层）

不是1票1coin的链上代币治理，核心协议演进通过：

1\. 开放的 EIP 流程（任何人都可以提交和改动提案）

2\. 定期的 AllCoreDevs / ACDE 等开发者会议

3\. 开发者、节点运营者、应用和用户之间的公开讨论

最终是否升级，由 客户端开发团队实现 + 节点运营者选择是否升级软件 共同决定。 这是一套彻底的 “粗共识+退出权” 模式

7\. 多个机制叠加出来的 去中心化

\- 共识层

\- 网络层

\- 实现层

\- 扩容路径

\- 治理层

\### 节点越多越好？

也不是人人都能参与，全节点的运行要成本：存储空间，带宽，电力，32ETH

为了人又多，跑得又快，把不同的工作分层，\*\*Rollup为中心 + 数据分片（Danksharding 、 PeerDAS）\*\*

1\. L1，就是主链，区块链本身

2\. L2 盖在主链上的加速层：

\- 扩展层，目标是在不牺牲 L1 安全的前提下，让交易更快，更便宜

\- 常见的L2类型

\- Rollups（汇总链）

\- 把大量的交易打包，然后把压缩后的数据和证明提交给L1

\- 分为

\- Optimistic Rollup（乐观打包）：先假定交易有效，有争议再提交欺诈证明

\- ZK Rollup（零知识证明）：用零知识证明直接向L1证明一大批交易是正确的

\- 状态通道（State Channels）

\- 双方把一部分状态锁在合约里，然后再链下反复互相更新，最后只把最终结果结算回链上，适合高频、双边交互

\- 侧链（Side Chains）

\- 独立链，有自己的共识和安全假设，通过跨链桥与以太坊互通，例如早期的 Polygon PoS等

\- 更像是“兼容以太坊的另一条链”。而不是严格意义上的L2

\> 以太坊的网络结构 = 理论上对任何人开放（permissionless），现实中受限于硬件和带宽，但通过 Layer 2 + 数据分片等方案，把“更多人参与”和“系统还能跑得动”两件事尽量同时做到。

\## 跨链，Layer2 扩展

乐观：默认交易正确，只有在有人质疑时才通过“欺诈证明（fraud proof）”回滚。

zk：

\## NFT

\### 基础标准

ERC（\*\*Ethereum Request for Comments\*\*，以太坊意见征求稿）就像以太坊生态的\*\*技术规范说明书\*\*。它定义了开发者应遵循的统一规则，以确保不同应用之间能顺畅交互。

\- ERC721：最经典的NFT标准，每个token都有唯一的ID，对应一个独一无二的资产

\- ERC1155：混合标准，可以在同一合约中同时管理同质化和非同质化资产，适合游戏里批量铸造道具、门票等（例如“1000张同一场演唱会门票 + 若干 VIP 特别票 ）

物理+ 数字NFT

\### ERC-6551

给NFT一个自己的账户

ERC-6551（Token-Bound Accounts）

为每个NFT绑定一个智能合约账户，让NFT本身可以：

\- 持有其他代币或NFT

\- 主动与合约交互（比如：签到，挂单，装备道具）

\- 记录自己的行动与“人生轨迹”

与AI agent很像？

\### NFT 角色管理： ERC-7432

给NFT引入了角色系统：

\- 持有人可以给某个地址授予与该NFT关联的特定角色

\- 角色会自动在某个时间戳过期，并可以携带自定义数据（如，可以帮我领取空投一次，在活动期间拥有管理员权限等）

把NFT从“纯所有权” 扩展成一套 可编程权限系统， 适合：

\- 游戏里的装备授权、租赁

\- 虚拟土地的使用权、管理权分离

\- DAO成员的权限管理

Nouns 确实挺有趣

Remix IDE

Hardhat，Truffle，Foundry （测试，本地链模拟，脚本化）

Infura、Alchemy、QuickNode 这样的基础设施提供商，负责维护高可用的节点和 RPC 服务，让 dApp 不必自己运维节点。

Scaffold-ETH、Wagmi、第三方 SDK 等，帮开发者快速搭建前端、集成钱包和合约调用；
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->























# 学习计划

## 学习内容：

1.  Web3 实习手册「入门导读」部分
    

-   ✅️Web3 工作方式
    
-   ✅️行业赛道全览
    
-   ✅️安全与合规
    
-   ✅️区块链岗位全景图
    
-   ⭕️智能合约开发
    
-   021 学习以太坊第 1 章
    
-   ⭕️六、以太坊 dApps 的概念与应用
    
-   ⭕️七、以太坊的去中心化实现机制
    
-   ⭕️八、网络结构的开放性与参与机制
    
-   ⭕️九、行业应用案例：金融、游戏与社交
    
-   ⭕️十、生态系统创新：DeFi、NFT 与 DAO
    
-   ⭕️十一、社区与开发生态扮演的角色
    
-   ⭕️021学习以太坊第2章
    

## 拓展 & 辅助理解材料

1.  ⭕️Ethereum 官网 Overview 基础内容学习
    

-   ✅️Day 1: A Developer’s Guide to Building on Ethereum - Intro
    
-   ✅️Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs
    
-   ✅️Day 3: ENS, DEX, Identity, Inventory, Sybil
    
-   ⭕️✅️Day 4: NFTs!!! ERC20 vs ERC721, IPFS, Metadata
    

## 非同质化代币

当然可以截图，下载，但是在链上，这个NFT不真正属于你

ERC（\*\*Ethereum Request for Comments\*\*，以太坊意见征求稿）就像以太坊生态的\*\*技术规范说明书\*\*。它定义了开发者应遵循的统一规则，以确保不同应用之间能顺畅交互。

\> 可以把ERC理解为智能手机的“接口标准”：比如Type-C接口有统一的尺寸、电压和协议标准，这样任何厂家生产的充电线和充电器都能通用。ERC标准也是如此，它让不同的代币、数字藏品（NFT）能在同一个钱包、交易所里被识别和管理。

| 标准 | 全称/俗称 | 核心功能与特点 | 典型应用场景 |

| :----------- | :------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |

| **ERC-20** | **可互换代币标准** | 定义了\*\*同质化代币\*\*的通用接口。所有代币单位完全相同、可互相替代。 | 各类功能性代币、治理代币（如UNI）、稳定币（如USDC）。 |

| **ERC-721** | **非同质化代币标准** | 定义了\*\*非同质化代币\*\*的接口。每个代币都独一无二，拥有独立ID和元数据。 | 加密艺术品（如CryptoPunks）、单个游戏道具、独一无二的数字藏品。 |

| **ERC-1155** | **多代币标准** | **一个合约可同时管理多种代币**（包括同质化和非同质化）。批量操作效率极高。 | 游戏资产平台（一套合约管理所有武器、皮肤、货币）、包含多种类型资产的合集。 |

| 标准 | 主要目的与贡献 |

| :----------- | :----------------------------------------------------------- |

| **ERC-4337** | 实现\*\*账户抽象\*\*，让钱包使用像Web2一样便捷（如社交恢复、免助记词）。 |

| **ERC-4844** | 为以太坊引入\*\*Proto-Danksharding\*\*，大幅降低Layer2数据可用性成本。 |

| **ERC-918** | 最早的\*\*工作量证明挖矿代币标准\*\*，为后来的公平发行模式提供灵感。 |
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

























# **学习计划**

1.  学习内容：
    
    1.  Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分
        
        -   **✅️Web3 工作方式**
            
        -   **✅️行业赛道全览**
            
        -   **✅️安全与合规**
            
    2.  [021 学习以太坊第 1 章](https://github.com/XiaoHai67890/021Ethereum/blob/main/%E3%80%8A021%E5%AD%A6%E4%B9%A0%E4%BB%A5%E5%A4%AA%E5%9D%8A%E3%80%8B%E5%BC%80%E6%BA%90%E6%95%99%E6%9D%90.pdf)
        
        -   **⭕️六、以太坊 dApps 的概念与应用**
            
        -   **⭕️七、以太坊的去中心化实现机制**
            
        -   **⭕️八、网络结构的开放性与参与机制**
            
        -   **⭕️九、行业应用案例：金融、游戏与社交**
            
        -   **⭕️十、生态系统创新：DeFi、NFT 与 DAO**
            
        -   **⭕️十一、社区与开发生态扮演的角色**
            
    3.  **⭕️**021学习以太坊第2章
        
2.  拓展 & 辅助理解材料
    
    1.  **⭕️**Ethereum 官网 [Overview](https://ethereum.org/learn/) 基础内容学习
        
    2.  ✅️[**Day 1: A Developer’s Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro
        
    3.  ✅️[**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)
        
    4.  **✅️**[**Day 3: ENS, DEX, Identity, Inventory, Sybil**](https://www.youtube.com/watch?v=wYSMNdIRoII&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3)
        

\## Web3 行业法律环境概览

\> 自 2017 年以来，中国监管部门已陆续发布多项针对虚拟货币的限制政策，主要包括：

\>

\> - 禁止 ICO（首次代币发行）；

\> - 禁止虚拟货币交易所运营与提供撮合服务；

\> - 明确虚拟货币不具有法偿性，不得作为支付工具；

\> - 禁止境外交易平台向中国境内居民提供服务；

\> - 打击以虚拟货币为载体的非法集资、传销、洗钱、赌博等活动。

\>

\> 当前主流监管基调为“全面封堵金融属性，有限容忍技术创新”。虽然 Web3 作为技术栈本身不被禁止，但一旦触及 Token 发行、融资、交易、矿池运营等高风险场景，便极易受到行政乃至刑事打击。

1\. 代币发行与交易

2\. 赌博、传销、洗钱等刑事等刑事风险

3\. 场外交易中的洗钱与非法经营

4\. 民商事争议

\## 全球监管背景

1\. FATF（Financial Action Task Force）金融行动特别工作组

2\. 美国监管框架

1\. SEC 证券交易委员会

\- 将大部分代币视为证券进行监管

2\. CFTC 商品期货交易委员会

\- 监管加密货币期货和衍生品

\- 将比特币和以太坊视为商品

3\. OCC 货币监理署

\- 允许银行提供加密货币托管服务

3\. 欧盟监管

1\. MiCA （加密资产市场法规）

4\. 香港

1\. 虚拟资产服务提供商（VASP）制度

\- 2023.06生效，要求加密货币交易所获得许可证

2\. 稳定币监管

\- 计划2024-2025推出稳定币监管框架

\## OKR写法与最佳实践

1\. **目标设定原则**

\- **聚焦关键目标**：每季度设定 3-5 个目标，避免分散精力。

\- **野心与可行平衡**：目标应“令人不适但可达”，理想完成率在 60%-70%（Google 标准）。

\- **结果导向**：聚焦成果而非产出（例：“提升客户留存率” 而非 “发送 10 份调研”）。

2\. 关键结果（KR）设计

\- 递进式量化：避免二进制（完成/未完成），采用里程碑式量化：

\- 比如

\- 目标：提升产品国际化能力

KR1：完成 3 个语言版本开发（40%）

KR2：用户测试通过率 ≥90%（70%）

KR3：新市场用户增长 25%（100%）

\- SMART标准：确保KR可衡量、有时限

3\. **评分与复盘**

\- **评分规则**：5 分（卓越贡献）→ 1 分（完成度 ＜ 50%）；

\- **复盘要点**：分析低分 KR 原因（目标过高/资源不足），高分则反思挑战性是否不足。

_表：OKR 评分标准参考_

| **得分** | **完成度** | **意义** |

| :------- | :--------- | :------------- |

| 1.0 | 100%+ | 目标过于简单 |

| 0.7 | 70% | 理想挑战水平 |

| 0.3 | 30% | 需重新规划策略 |

4\. **智能合约团队 OKR 示例**

**Objective** : 提升协议安全性至机构级标准

**Key Results** :

\- 通过 Certora 形式化验证（主网部署前）

\- 漏洞赏金计划覆盖率 100%（Immunefi 平台）

\- 平均审计发现项 ≤2 个（对比 OpenZeppelin 基准）

5\. **万能汇报结构：**

\- 向上汇报：

\- 公式：进展 + 卡点 + 建议 + 需支持（例："目前完成 80%，遇到 XX 问题，我建议尝试 A 方案，需要您协调技术组确认可行性"）。

\- 跨部门协作：

\- 提前了解对方 KPI（如市场部关注转化率），用共同利益点推动合作（例："这次优化可能帮你们提升 XX 指标"）。

【项目 xxx 进度】

✅ 已完成：A 模块上线（比计划提前 1 天）

🚧 进行中：B 测试（完成 70%，明天下班前交付）

⚠️ 卡点：C 接口权限未获批（已联系 `@李四`，最晚周四答复）

🔜 下一步：周四同步测试报告（初稿已发附件）

\- **埋点式复盘**

\- 建立\*\*个人 SOP 库\*\*（如：用 Notion 存"活动策划检查清单"），尽量详细记录每一个步骤

\- 对工作留痕

\- 交接文件包：含操作指南 / 联系方式 / 踩坑记录

\- **沟通案例**

\- 需求沟通模板

\- 当同事说：“这个功能尽快做出来！”

\- ❌ 回答：“现在排期满了” → 制造对立

\- ✅ 结构化回应：

\- 理解需求：您需要解决 X 问题，对么？（确认真实诉求）

\- 给选项：

\- Option A：简化版周五交付（缺 Y 功能）

\- Option B：完整版下周三交付

\- 建议：如果急用推荐 A，我能协调资源加急

\- 冲突化解话术

\- 当被甩锅时：

\- ❌ “这明明是你的责任！”

\- ✅ “从流程看我这部分周三已完成（附邮件截图），目前卡在 QA 环节，咱们一起找测试负责人碰下？”

\- 要点：用事实代替情绪，拉对方成“战友”而非对手

\- 向上沟通技巧

\- 接收模糊任务时：您希望优先保证速度还是质量？如果有参考案例更好~

\- 领导意见矛盾时：上次您提到重点控成本，这次营销预算超标是否意味着策略调整？求确认方向
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->



























# 学习计划

1.  学习内容：
    
    1.  Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分
        
        -   **⭕️Web3 工作方式**
            
        -   **✅️行业赛道全览**
            
    2.  [021 学习以太坊第 1 章](https://github.com/XiaoHai67890/021Ethereum/blob/main/%E3%80%8A021%E5%AD%A6%E4%B9%A0%E4%BB%A5%E5%A4%AA%E5%9D%8A%E3%80%8B%E5%BC%80%E6%BA%90%E6%95%99%E6%9D%90.pdf)
        
        -   **⭕️六、以太坊 dApps 的概念与应用**
            
        -   **⭕️七、以太坊的去中心化实现机制**
            
        -   **⭕️八、网络结构的开放性与参与机制**
            
        -   **⭕️九、行业应用案例：金融、游戏与社交**
            
        -   **⭕️十、生态系统创新：DeFi、NFT 与 DAO**
            
        -   **⭕️十一、社区与开发生态扮演的角色**
            
2.  拓展 & 辅助理解材料
    
    1.  Ethereum 官网 [Overview](https://ethereum.org/learn/) 基础内容学习
        
    2.  ✅️[**Day 1: A Developer’s Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro
        
    3.  ✅️[**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)
        

## DeFi

Decentralized Finance（去中心化金融），不依赖传统银行，实现借贷，交易，支付

### 1\. Uniswap （DEX去中心化交易所）

恒定乘积公式x\*y=k

-   恒定乘积公式就是 **x \* y = k**，其中 **k** 是一个常数。
    
    10 \* 5000 = 50,000（这就是 k 的值）
    
    当你在这个池子里进行交易时，这个公式始终要保持成立。也就是说，交易后，ETH 和 USDC 的数量会发生变化，但它们相乘的结果 **k** 必须始终是 50,000（忽略手续费的情况下）。
    
    举个例子：
    
    > 如果你想用 1 个 ETH 兑换 USDC，流动性池中的 ETH 数量就会增多，而 USDC 的数量就会减少。
    > 
    > 假设交易后，池中的 ETH 变成了 11 个，那么为了保持公式成立，USDC 数量需要调整到 4545.45 个（50,000 / 11 = 4545.45）。
    > 
    > **具体兑换价格计算**：
    > 
    > -   交易前：10 ETH，5000 USDC
    >     
    > -   交易后：11 ETH，4545.45 USDC
    >     
    > -   你用 1 ETH 兑换到了 5000 − 4545.45 = 454.55 USDC（不考虑手续费的情况）
    >     
    > -   **兑换价格 = 454.55 USDC / 1 ETH = 454.55 USDC per ETH**（低于交易前现货价 500，体现了价格冲击）
    >     
    > 
    > 这样，Uniswap 就能自动算出兑换的价格，而且随着池子中资产比例的变化，价格也会自动调整。
    

流动性池和流动性提供者（LP）

赚取手续费

举个例子：

> 假设你存入了 1 ETH 和 5000 USDC 到一个 ETH/USDC 的流动性池。如果别人用 1 ETH 交换 USDC，那么这个交易会产生 0.003 ETH 的手续费（0.3%），实际到手约 453.31 USDC（有效输入 0.997 ETH）。
> 
> **具体收益计算**：
> 
> -   手续费是以输入代币计价（这里是 ETH），直接留在池子里分给 LP
>     
> -   交易手续费：1 ETH × 0.3% = 0.003 ETH，按照 500 USDC / ETH 的价格，手续费价值 1.5 USDC
>     
> -   如果你占池子 10% 的流动性份额，你获得的手续费收入：0.003 ETH × 10% = 0.0003 ETH
>     
> 
> 当然，实际情况会因为池子大小、交易量、市场波动等因素而不同，这里只是一个简化的例子。

### 2\. Compound：去中心化借贷协议

Compound 是一个去中心化的借贷平台，允许用户借入或借出加密资产。

**举个例子**

> 小明手上有 5 个 ETH，但他需要 2000 美元现金。他可以把这 5 个 ETH 抵押到 Compound，系统会根据市场情况发放相应的 USDC（比如 2000 美元等值的 USDC）。通过这种方式，小明可以获得所需的资金，而不必卖掉自己的 ETH。
> 
> **重要：关于价格变化的影响**
> 
> -   小明需要偿还的 USDC 数量是固定的（2000 USDC + 利息），不会因为 ETH 价格上涨而改变
>     
> -   如果 ETH 价格上涨，小明其实赚了：他用价值更高的 ETH 作为抵押，但偿还的 USDC 数量不变
>     
> -   如果 ETH 价格下跌过多，可能会触发强制清算，系统会自动卖出部分 ETH 来偿还贷款
>     
> 
> **具体例子**：
> 
> -   抵押时：ETH 价格 2000，抵押 5 ETH（价值 10,000），借出 2000 USDC
>     
> -   价格上涨后：ETH 价格 3000，抵押物价值 15,000，但仍只需偿还 2000 USDC + 利息
>     
> -   这样小明既获得了流动性，又享受了 ETH 价格上涨的收益
>     
> 
> 通过这种机制，Compound 不仅为借款人提供流动性，也为存款人提供利息回报。整个过程都由智能合约自动执行，极大地提高了效率和透明度。

### 3\. MakerDAO（现已更名为Sky）：稳定币系统

MakerDAO 是一个去中心化的借贷协议，它允许用户通过超额抵押资产来生成 **DAI**，一种与美元挂钩的稳定币。DAI 的价值稳定在 1 美元左右，因此可以作为一个避险工具或支付手段。MakerDAO 通过智能合约管理抵押资产，并利用 **稳定费率** 来调整系统中 DAI 的供需关系。

**举个例子**

> 假设小明持有很多 ETH，但他担心市场波动，想换成一个更加稳定的资产。他可以将 1 个 ETH（价值约 3500 美元）抵押到 MakerDAO，并生成 2000 个 DAI（1 DAI ≈ 1 美元）。这 2000 个 DAI 可以用于消费、投资或存款。当小明希望取回 ETH 时，他只需要偿还 2000 个 DAI 和相关的稳定费，系统将退还给他 1 个 ETH。
> 
> Sky（原 MakerDAO）的这种超额抵押和去中心化的稳定币生成机制，不仅为用户提供了资金的灵活性，还为加密市场提供了一个相对稳定的货币单位。

## NFT

### 1\. 数字资产的唯一性和所有权

### 2\. 智能合约：自动化的所有权转移和交易

智能合约是一种自执行的协议，意味着合约中的条款在满足特定条件时会自动执行，而无需第三方中介。这些合约不仅能确保交易的安全性和自动化，还能赋予 NFT 一些特别的功能。

## DAO

### 1\. NounsDAO

每日生成一个头像拍卖，然后积累

### 2\. LXDAO

### 3\. **ConstitutionDAO**

一起参加拍卖一个文物

## MEME

MEME 原意为“迷因”，即网络文化中的梗、表情包、搞笑段子等。在加密圈，MEME 通常指那些以网络文化为基础的代币，比如 DOGE（狗狗币）、PEPE（青蛙币）、SHIB（柴犬币）等。

MEME 币的特点通常是“有趣、搞怪、社区驱动”，它们往往缺乏明确的技术价值，但通过强大的社区共识和文化认同获得价值。2024-2025 年，MEME 币市场出现了新的特点：**公平发射**（Fair Launch）、**社区自治**和**实用性整合**成为新趋势。

## 交叉创新领域

1.  DeFi + NFT
    
2.  DAO + MEME
    
3.  AI + DeFi
    
4.  Web3 + 乡建
    
    > 2024 年 8 月，在 **SeeDAO** 的持续推动与 **LXDAO** 的孵化支持下，一群年轻人相聚在安徽省阜阳市南塘兴农合作社，成立了中国首个致力于将 Web3 落地于乡村的组织——南塘 DAO 。 南塘 DAO 秉持“向乡土求共同体之 DAO，向加密寻世界性之道”的核心理念，旨在促进乡建与 Web3 领域的深度交流与融合，推动双方互学互鉴。
    

## 对于求职者

1.  **专业化发展**：选择 1-2 个领域深入研究
    
2.  **技术栈匹配**：根据目标岗位学习相应技术
    
3.  **项目经验**：参与开源项目或 Hackathon
    
4.  **行业动态**：持续关注最新发展和监管变化
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->





























# 2060112

准备学习：

1.  学习内容：
    
    1.  Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分
        
    2.  021 学习以太坊第 1 章
        
2.  拓展 & 辅助理解材料
    
    1.  Ethereum 官网 [Overview](https://ethereum.org/learn/) 基础内容学习
        
    2.  [**Day 1: A Developer’s Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro
        
    3.  [**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)
        

完成任务：

1.  测试币转账 ✅️
    
2.  Mint 自己的NFT ✅️
    
3.  查看回放 ✅️
    

# 区块链基础

一个链表，首尾相连

每一个区块里有->有存储限制，有生成速率（可以通过难度系数调整）：

1\. 交易list

2\. 前一个区块的哈希值（整个区块的内容 convert to 哈希Hash）

\### 区块链特性

1\. 不可篡改（其实是难篡改，修改创世区块，整条链都要改）

2\. 公开透明，但匿名（一旦钱包地址与本人关联就不匿名了）

3\. 快速交易，只要被记账就完成了交易，跨境支付场景

\### 分布式记账，每个节点都有完整的，相同的账本

1\. 去中心化，没人能控制所有节点（很难）

2\. 不可篡改（如果51%以上节点不诚实就可以篡改）

\### BTC，ETH代币

只有一种凭空铸造的方法 -> 区块奖励（打包记账的矿工会获得代币）

\### 矿工就是维护节点的人

1\. **用户发起交易**：用户通过钱包应用发起转账、智能合约调用等操作

2\. **交易广播**：交易信息被广播到整个网络中的各个节点

3\. **节点验证**：网络中的矿工节点验证交易的合法性（余额是否足够、签名是否正确等）

4\. **打包成块**：通过共识机制（如工作量证明），矿工将验证过的交易打包成新的区块

5\. **链接上链**：新区块被添加到区块链上，更新全网的账本状态

6\. **奖励发放**：成功打包区块的矿工获得代币奖励和交易手续费

\---

\### 公链，联盟链，私链

1\. #### 公链（公共公园）

1\. **成为节点的方法**：

\- **无需申请**：任何人只要带着工具（比如手机、电脑）就能加入公园，成为维护者（节点）。

\- **自由进出**：你可以随时离开或回来，没人会拦你。

2\. **共同管理数据的模式**：

\- **所有人可见**：公园里的所有活动（比如谁修剪了草坪、谁清理了垃圾）都会被公开记录在公告栏上，所有人都能看到。

\- **去中心化决策**：如果公园需要修路，大家会投票决定（共识机制），不需要某个领导拍板。

\- **缺点**：因为人太多，决策效率低（交易确认慢），维护成本高（比如电费、人力）。

2\. #### 联盟链（Consortium Blockchain) 多公司联合的董事会

1\. **成为节点的方法**：

\- **需要邀请或申请**：如果你想加入董事会，必须得到其中一家公司的认可（比如你是某家银行的合作伙伴）。

\- **权限分级**：董事会成员可能分为两类：

\- **决策者**（联盟核心成员）：比如银行 A、银行 B，可以修改数据库规则。

\- **观察者**（联盟普通成员）：比如物流公司，只能查看数据但不能修改。

2\. **共同管理数据的模式**：

\- **半公开数据**：数据库里的信息（比如客户信用评分）只有董事会成员能看到，外人无法访问。

\- **联合决策**：如果要修改数据库规则（比如增加新字段），需要董事会成员投票通过。

\- **优点**：效率比公链高（因为成员少），隐私比公链好（数据不对外公开），但不如私链灵活（需要多方协调）。

3\. #### 私链（私人俱乐部）

1\. **成为节点的方法**：

\- **严格审批**：想加入俱乐部？必须经过老板批准（比如交会员费、填写申请表）。

\- **固定成员**：一旦成为会员，你的权限由老板决定（比如能否查看账本、能否修改规则）。

2\. **共同管理数据的模式**：

\- **数据完全私有**：账本只对会员开放，外人无法查看（比如你的消费记录只有你和老板知道）。

\- **老板说了算**：如果俱乐部要改规则（比如涨价），老板可以直接决定，不需要投票。

\- **优点**：效率极高（因为只有少数人参与），隐私极强（数据不对外公开），但缺乏公链的透明性。

| 区块链类型 | 节点加入方式 | 数据可见性 | 管理模式 | 适合场景 |

| :--------- | :------------------ | :------------- | :--------------------- | :----------------- |

| **公链** | 任何人自由加入 | 所有人可见 | 去中心化（大家投票） | 加密货币、公共存证 |

| **联盟链** | 需联盟成员邀请/审批 | 仅联盟成员可见 | 多中心化（董事会决策） | 供应链、金融协作 |

| **私链** | 由老板严格审批 | 仅内部成员可见 | 中心化（老板说了算） | 企业内部管理、审计 |

\---

\### Web3，Web 3.0， Web2

1\. #### Web2（当前互联网）

1\. 中心化控制

2\. 内容生产者，但不拥有数据

3\. 广告驱动，平台佣金

2\. #### Web3.0（语义网）

1\. **语义标记**：使用 RDF、OWL 等标准描述数据关系

2\. **结构化数据**：信息按照标准格式组织，便于机器理解

3\. **知识图谱**：构建实体间的语义关系网络

4\. **典型技术**：本体工程、语义查询语言（SPARQL）、链接数据

5\. **关键区别**：

\- ❌ **不是区块链技术**，而是传统互联网的数据组织升级

\- ❌ **主要不依赖 AI**，而是通过标准化数据格式实现

\- ✅ 与 Web3 可结合（语义标记 + 区块链存储）

3\. #### Web3

1\. **数据主权归用户**：用区块链存储身份和资产

2\. **无需信任中介**：智能合约自动执行规则

3\. **核心**

1\. 钱包-->\[签名\]-->Dapp-->\[调用\]-->智能合约-->\[读写\]-->区块

4\. #### 对比矩阵

| 维度 | Web2 | Web 3.0 | Web3 |

| :----------- | :-------------: | :-----------: | :-----------------: |

| **控制权** | 平台垄断 | 部分开放 | 用户自治 |

| **数据存储** | 中心服务器 | 混合存储 | 区块链 / IPFS |

| **支付系统** | 信用卡 / 支付宝 | 集成支付 | 加密货币 |

| **典型技术** | JavaScript | RDF / OWL | 智能合约 |

| **代表企业** | 腾讯 / 阿里 | W3C / DBpedia | Uniswap / ConsenSys |

5\. #### 常见错误

1\. Web3 ！= Web3.0

\- Web3 是区块链驱动的革命

\- Web3.0 是语义网技术驱动的数据组织升级

2\. Web3 不是万能

\- 优势：金融，产权，隐私场景

\- 劣势：高频交流

6\. #### 我的技术栈需求

React + Ether.js + Solidity + IPFS

# 以太坊概览

### 以太坊介绍

以太坊（Ethereum）被称为“区块链 2.0”，它不仅是一种加密货币（以太币 ETH），更是一台支持智能合约的“全球共享计算机”

2022 从 Pow 到 Pos

核心创新：智能合约

比特币是区块链 1.0 的象征（专注于货币属性）

以太坊通过智能合约和可编程性，目标是成为全球范围内的“世界计算机”，图灵完备性

### Ethereum VS Bitcoin

| 维度 | 比特币（Bitcoin） | 以太坊（Ethereum） |
| --- | --- | --- |
| 目标与定位 | 去中心化的数字货币，强调安全、稳定和稀缺性（总量 2100 万枚） | 去中心化平台，支持智能合约和 Dapps，定位为“区块链 2.0” |
| 编程能力 | 脚本语言有限，仅支持简单的交易验证逻辑 | 图灵完备的编程语言（如 Solidity），可开发复杂智能合约 |
| 共识机制 | 工作量证明（PoW），矿工通过算力竞争记账权 | 从 PoW 转向权益证明（PoS），通过 The Merge 实现能源效率优化 |
| 交易速度 | 每 10 分钟生成一个区块，交易确认较慢 | 区块时间约 12 秒，交易确认更快，适合高频应用 |
| 经济模型 | 总量固定，强调抗通胀属性 | 供应灵活，通过 EIP-1559 等机制可能呈现通缩趋势 |

NaN.  以太坊1.0
      
      -   通过计算机算力“挖矿”，争夺打包交易的权利
          
      -   消耗电力
          
      -   成功打包就可以获得ETH奖励
          
      -   算Nonce值
          
NaN.  以太坊2.0
      
      -   2020.12
          
          -   信标链（Beacon Chain）专门运行PoS共识机制
              
              -   以太坊继续使用PoW挖矿
                  
              -   信标链独立运行PoS验证
                  
              -   两条链并行存在，互不干扰
                  
      -   2022.09
          
          -   The Merge完成 0915
              
          -   以太坊主网“抛弃”PoW挖矿引擎（猜测：51%一起改变）
              
          -   将共识机制“插接”到信标链的PoS系统
              
      -   合并后
          
          -   执行层：处理交易、智能合约（主网）
              
          -   共识层：管理验证者、确定区块顺序（信标链）
              
          
          **验证者如何工作**：
          
          -   **准入门槛**：质押 32 ETH 成为验证者
              
          -   **工作方式**：系统随机选择验证者来提议和验证区块
              
          -   **奖励机制**：验证者获得新发行的 ETH + 交易费用
              
          -   **惩罚机制**：作恶者质押的 ETH 被销毁（Slashing）
              
          
          **相比 PoW 的优势**：
          
          -   **能耗降低 99.95%**：无需大量电力和硬件
              
          -   **经济安全性**：攻击成本约需控制全网 67% 的质押 ETH（价值数百亿美元）
              
          -   **最终确定性**：区块确认更快、更可靠
              
      -   未来升级路线图
          
          -   升级目标是：**让更多人用得起、用得快、用得安全**
              
          -   **分片技术演进——从执行分片到数据分片**
              
          -   **EIP-4844（Cancun 升级）——“省钱神器”**
              
          -   **ZK-Rollup 技术——“批量验证，一步到位”**
              
          -   **其他重要升级方向**
              
              -   **EIP-1559 成果**：已实现基础费用机制，但 Gas 费仍受网络拥堵影响较大
                  
              -   **Verkle 树技术**：优化状态存储结构，减少节点同步所需的数据量
                  
              -   **执行环境优化**：提升 EVM 性能，支持更复杂的智能合约应用
                  

# 第一章

## EVM（Ethereum Virtual Machine）

以太虚拟机，图灵完备

> **图灵完备（Turing Completeness）** 是计算机科学中的一个核心概念，用于描述一个系统（如编程语言、指令集或自动机）是否拥有与**通用图灵机（Universal Turing Machine）** 相同的计算能力。简单来说，如果一个系统是图灵完备的，那么**理论上它可以解决任何可计算的问题**（只要提供足够的时间和内存）。
> 
> 一个图灵完备的系统通常需要具备以下能力：
> 
> -   **条件分支**（如 `if` 语句）
>     
> -   **循环或递归**（避免无限循环需由程序员控制）
>     
> -   **可读写的数据存储**（如变量、内存）
>     

能执行智能合约代码，让区块链不再只是“记账

### L2，要看看L2是什么

NaN.  **Layer 1（L1）**
      
      -   **以太坊主网**：核心区块链，负责最终安全性与共识。
          
      -   **EVM**：以太坊虚拟机，执行智能合约代码。
          
      -   **账户系统**：外部账户（EOA）与合约账户（CA）共同构成网络基础。
          
NaN.  **Layer 2（L2）**
      
      -   **Rollup**：通过将交易批量处理后提交至 L1，降低 Gas 费。
          
          -   **Optimistic Rollup**：假设交易合法，仅在争议时验证。
              
          -   **ZK Rollup**：通过零知识证明验证交易，无需链上争议。
              
NaN.  **侧链（Sidechains）**：独立运行的链，通过桥接与主网交互。
      
NaN.  **以太坊生态分层架构**
      

ERC-721 or ERC-1155

ERC-20

* * *

## 智能合约

可以把智能合约想象成自动贩卖机：

你投入硬币（支付 Gas），选择饮料（调用合约函数），机器就自动出货（执

行转账或业务逻辑），无需店员干预。

每一笔操作都由分布式节点“自动执行”，并为此收取手续费，而且整个过

程是公开可验证、不可随意篡改的。

* * *

## 分布式app (dAPPs)

以太坊上的分布式应用程序（dApp）部署在区块链上，不依赖单一服务器，

也没有传统意义上的 “后台可关机”。只要以太坊网络和客户端软件还活着，dApp

就很难被单一主体关停。

* * *

## 代币（Tokens）

智能合约可以创造代币供分布式应用程序使用，这些代币可以是：

-   可替代的（ERC-20）：例如游戏里的“金币”、治理代币、稳定币等；
    
-   不可替代的（ERC-721 / ERC-1155）：对应独特资产，如艺术品、门票或
    

游戏角色。

* * *

## PoS vs PoW

**PoW**（工作量证明，历史上的以太坊共识）

-   **机制原理：** 节点（矿工）投入算力“挖矿”，竞争解出复杂数学难题，以获得打包新区块的权利和区块奖励。
    
-   **安全性来源：** 依赖高昂的硬件与电力成本——攻击者要想“51% 攻击”，就必须付出极其巨大的算力和电费。
    

**PoS** （权益证明）

-   质押32ETH成为验证者，每个时隙由协议随机选出提议区块的验证者，并由其他验证者投票确认。质押越多、表现越稳定，长期看被选中的频率越高
    
-   安全机制：恶意行为，罚质押的ETH
    

简单总结就是：**PoW 用电力和硬件“押注诚实”，PoS用锁定的ETH和削减机制“押注诚实”**

* * *

## GAS Fee

链上任何行为都会消耗Gas

EIP-1559之后，Gas 费由“基础费（base fee）+小费（priority fee）”组成，其中基础费会被销毁，小费奖励给出块验证者。

### 销毁基础费的三大核心目的

| 设计目标 | 具体机制与效果 |
| --- | --- |
| 1. 建立可信的“锚定价格”，防止矿工/验证者操纵手续费 | 基础费由协议算法直接计算和销毁，矿工/验证者无法获得。这消除了他们通过人为制造网络拥堵来推高手续费的动机，使手续费更加公平。 |
| 2. 改善用户体验，实现手续费可预测性 | 基础费根据前一区块的拥堵情况（利用率是否超过50%）算法化调整（每个区块最多±12.5%），避免了旧模式下的价格剧烈波动。用户无需再猜测“最优出价”。 |
| 3. 重塑ETH经济模型，为其创造价值捕获与通缩压力 | 销毁基础费相当于将从网络活动中产生的价值（手续费）从ETH流通总量中永久移除，为ETH引入了通缩属性。网络越活跃，销毁越多，这使ETH从单纯的“燃料”转变为具有内在价值积累的资产。 |

* * *

## Protp-Danksharding (EIP-4844) 与 Dencun升级

-   加入了新的“携带blob的交易类型”，为Rollup提供临时数据空间
    
-   这些blob只在共识层保存一段时间后被丢弃，不进入永久状态，从而极大减轻L1存储压力
    
-   实际效果是：L2 发布数据的成本大幅下降，很多 Rollup 的交易费用下降了 90% 甚至接近 99%
    

**比喻：**

想象你开着车（普通以太坊交易），旁边挂着一个“小挂车” （blob 数据）：

-   你把大批货物（Rollup 的批量交易数据）先装到挂车上；
    
-   车跑完该跑的路，过一段时间挂车会自动脱落回收，不再长期占用空间；
    
-   主车（L1 状态）保持轻量，而 L2 的数据传输成本大幅降低。
    

当前网络对 blob 数量采用保守限制：

-   Dencun 上线初期每个区块最多约 6 个 blob，目标平均 3 个；随着PeerDAS 等技术部署，预计可逐步提升到每块十几个乃至几十个 blob。
    

* * *

## 分片与Danksharding

“分片”本质上是把一个大系统拆成多个“分区”并行处理，从而提升吞吐量。以太坊早期路线图曾设想通过“64 条分片链”来扩容。

Proto-Danksharding 先给以太坊挂上“小挂车”，而完整Danksharding 则是把挂车扩展成“高速货运列车”，专门为 L2 输送数据。

* * *

## 叔块

PoW时期的，当同时挖出2个区块，主链选择一个，另一个叫Uncle block（会获得安慰奖）

The Merge 之后，以太坊转为 PoS，共识流程完全不同，不再产生新的叔块；相关字段在区块头中基本被置零，仅为兼容历史数据和工具而保留。

* * *

## Ether的定义

以太币（Ether，ETH）是以太坊区块链上的**原生数字资产**

ETH三大核心作用

NaN.  Gas fee
      
      -   EIP-1559后
          
      -   实际支付费用≈（Base Fee+ Priority Fee ） \* Gas Used
          
NaN.  参与网络共识与安全（staking）
      
      NaN.  质押资产ETH
            
      NaN.  获得奖励
            
      NaN.  提供经济安全
            
      NaN.  奖惩机制
            
      
      我想知道底层代码，什么算法来维持这个奖惩机制
      
NaN.  价值存储和交换媒介
      
      NaN.  数字资产
            
      NaN.  DeFi生态的抵押品与流动性基石
            
      NaN.  NFT与数字经济的基石货币
            

## 以太坊：全球 可编程 区块链

## 以太坊 与 比特币
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
