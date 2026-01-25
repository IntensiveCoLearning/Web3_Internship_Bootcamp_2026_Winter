---
timezone: UTC+8
---

# liziye627-design

**GitHub ID:** liziye627-design

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
\# 2026-01-25 Rust入门学习

\## 📅 日期

2026年1月25日

\## 🎯 学习目标

\- 了解Rust语言基础概念

\- 掌握Rust的核心优势

\- 入门Rust的基本语法和特性

\## 🦀 Rust入门知识

\### 什么是Rust？

Rust是一门系统编程语言，专注于：

\- **内存安全**：无需垃圾回收器的内存管理

\- **并发安全**：编译时防止数据竞争

\- **性能卓越**：接近C/C++的运行速度，超级快的

\- **可靠性**：强类型系统和错误处理机制（这个还没了解到）

\### Rust的核心优势

\#### 1. 内存安全 🔒

\- **所有权系统**：每个值都有一个明确的所有者

\- **借用检查器**：编译时检查内存访问安全性

\- **生命周期**：自动管理内存，防止悬垂指针

\- **零成本抽象**：安全机制不影响运行时性能

\#### 2. 并发安全 ⚡

\- **无数据竞争**：编译时保证线程安全

\- **Send/Sync特质**：类型级别的并发安全保证

\- **Arc/Mutex**：安全的共享数据结构

\- **异步编程**：现代化的async/await支持

\#### 3. 性能优势 🚀

\- **零开销抽象**：高级特性编译为高效机器码

\- **内存布局控制**：精细的内存管理

\- **LLVM后端**：优秀的优化能力

\- **内联函数**：编译器自动优化函数调用

\#### 4. 生态系统 🌟

\- **Cargo**：强大的包管理和构建系统

\- [**crates.io**](http://crates.io)：丰富的第三方库生态

\- **WebAssembly**：优秀的WASM支持

\- **嵌入式开发**：适合IoT和嵌入式系统

\### Rust出生的技术背景

\#### 发展历程

\- **2006年**：Graydon Hoare开始开发

\- **2010年**：Mozilla接手项目

\- **2015年**：1.0版本正式发布

\- **2021年**：2.0版本，async/await稳定

\#### 设计哲学

\- **安全性优先**：防止常见的编程错误

\- **性能导向**：不牺牲性能的安全性

\- **实用性**：解决实际工程问题

\- **现代语言特性**：函数式编程支持

\### 最入门的基础知识

\#### 1. 基本语法

\`\`\`rust

// 变量绑定（不可变）

let x = 5;

// 可变变量

let mut y = 10;

y = 15;

// 常量

const MAX\_POINTS: u32 = 100\_000;

\`\`\`

\#### 2. 数据类型

\`\`\`rust

// 标量类型

let integer: i32 = 42;

let float: f64 = 3.14;

let boolean: bool = true;

let character: char = '🦀';

// 复合类型

let tuple: (i32, f64) = (500, 6.4);

let array: \[i32; 5\] = \[1, 2, 3, 4, 5\];

\`\`\`

\#### 3. 函数

\`\`\`rust

fn main() {

println!("Hello, Rust!");

}

fn add(a: i32, b: i32) -> i32 {

a + b // 表达式，无需return

}

\`\`\`

\#### 4. 所有权概念

\`\`\`rust

let s1 = String::from("hello");

let s2 = s1; // s1的所有权转移到s2

// println!("{}", s1); // 编译错误！s1不再有效

fn takes\_ownership(some\_string: String) {

// some\_string在函数结束时被释放

}

\`\`\`

\#### 5. 错误处理

\`\`\`rust

// Result类型处理可能失败的操作

match File::open("test.txt") {

Ok(file) => println!("文件打开成功"),

Err(error) => println!("打开失败: {:?}", error),

}

// 使用?操作符简化错误处理

fn read\_file\_content() -> Result<String, io::Error> {

let mut content = String::new();

File::open("test.txt")?.read\_to\_string(&mut content)?;

Ok(content)

}
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->

今日学习目标 (Learning Goals)

\### 核心目标

1\. **Solidity进阶语法**: 掌握继承 (Inheritance)、接口 (Interfaces)、库 (Libraries)。

2\. **标准协议实现**: 深入理解并实现 **ERC20** 代币标准。

3\. **合约安全基础**: 学习防范 **重入攻击 (Reentrancy)** 和 **整数溢出**。

4\. **工程化实践**: 编写可复用、模块化的智能合约代码。

\---

\## 📖 核心知识点详解

\### 1. 面向对象特性 (OOP Features)

\#### 继承 (Inheritance)

Solidity 支持多重继承，使用 `is` 关键字。

\- \*`virtual`\*\*: 父合约中允许被重写的函数。

\- \*`override`\*\*: 子合约中重写父合约函数。

\- \*`super`\*\*: 调用父合约的逻辑。

\`\`\`solidity

contract Ownable {

address public owner;

constructor() { owner = msg.sender; }

}

// Token 继承自 Ownable

contract Token is Ownable {

function burn() public {

require(msg.sender == owner, "Only owner"); // 使用继承的变量

}

}

\`\`\`

\#### 接口 (Interfaces)

接口是合约之间交互的标准。

\- 只有函数声明，没有实现。

\- 函数必须是 `external`。

\- 不能包含状态变量或构造函数。

\#### 库 (Libraries)

用于提取通用逻辑（如数学运算），减少 Gas 消耗。

\- 使用 `using Lib for type` 语法。

\- 常用库`SafeMath` (Solidity 0.8+ 已内置溢出检查，但了解其原理很重要)。

\### 2. ERC20 代币标准

ERC20 是以太坊上最通用的代币标准，必须实现以下核心接口：

| 函数 | 说明 |

|------|------|

| `totalSupply()` | 代币总供应量 |

| `balanceOf(account)` | 查询余额 |

| `transfer(to, amount)` | 转账 |

| `approve(spender, amount)` | 授权转账 |

| `transferFrom(from, to, amount)` | 代理转账（需先授权） |

\### 3. 合约安全基础

\- **Checks-Effects-Interactions 模式**:

1\. **Check**: 检查条件 (require)。

2\. **Effect**: 更新状态变量。

3\. **Interaction**: 与其他合约交互 (转账等)。

_这能有效防止重入攻击。_

\---

\## 💻 实战练习: AdvancedToken

编写一个包含安全机制和权限控制的 ERC20 代币合约。

\### 合约代码结构

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import "./IERC20.sol"; // 标准接口

contract AdvancedToken is IERC20 {

// 状态变量

mapping(address => uint256) private \_balances;

mapping(address => mapping(address => uint256)) private \_allowances;

uint256 private \_totalSupply;

string public name = "Advanced Learning Token";

string public symbol = "ALT";

uint8 public decimals = 18;

address public owner;

bool private \_paused; // 紧急暂停开关

// 事件

event Paused();

event Unpaused();

// 修饰符

modifier onlyOwner() {

require(msg.sender == owner, "Not owner");

\_;

}

modifier whenNotPaused() {

require(!\_paused, "Contract paused");

\_;

}

constructor(uint256 initialSupply) {

owner = msg.sender;

\_mint(msg.sender, initialSupply);

}

// 铸造函数 (内部)

function \_mint(address account, uint256 amount) internal {

\_totalSupply += amount;

\_balances\[account\] += amount;

emit Transfer(address(0), account, amount);

}

// 转账实现 (带暂停检查)

function transfer(address to, uint256 amount) public override whenNotPaused returns (bool) {

require(\_balances\[msg.sender\] >= amount, "Insufficient balance");

\_balances\[msg.sender\] -= amount;

\_balances\[to\] += amount;

emit Transfer(msg.sender, to, amount);

return true;

}

// ... 其他ERC20函数实现 (approve, transferFrom 等)

// 管理功能

function pause() public onlyOwner { \_paused = true; emit Paused(); }

function unpause() public onlyOwner { \_paused = false; emit Unpaused(); }

}

\`\`\`

\---

\## 📝 学习心得与遇到的问题

\### 遇到的挑战

1\. **接口与抽象合约的区别**:

\- 接口更纯粹，只能定义行为；抽象合约可以包含部分实现。

\- 在跨合约调用时，接口非常有用（如调用 Uniswap）。

2\. \*`transferFrom` 的理解\*\*:

\- 需要理解 `approve` 和 `allowance` 的机制。这是 DeFi 协议扣款的基础。

\### 关键收获

\- 理解了为什么 Solidity 0.8.0 之后不再强制需要 SafeMath（内置了溢出检查）。

\- 掌握了 Solidity 的继承顺序（C3 Linearization）在多重继承中的重要性。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->


\## 1. 核心知识点梳理

\### 1.1 函数可见性 (Function Visibility)

Solidity 中有四种可见性修饰符，决定了函数可以被谁调用。这是合约安全的基础。

| 修饰符 | 说明 | 适用场景 | Gas 消耗 |

| :--- | :--- | :--- | :--- |

| `public` | 内部外部均可调用 | 任何需要公开的函数 | 中 |

| `private` | 仅当前合约内部可见 | 隐藏底层逻辑，继承合约不可见 | 较低 |

| `external` | 仅外部调用 (可被 `this.f()` 内部调用) | 接收大数据量的数组参数时更省 Gas | 低 |

| `internal` | 仅内部及继承合约可见 | 复用逻辑，作为父类函数 | 最低 |

\### 1.2 状态可变性 (State Mutability)

区分函数是否读取或修改链上数据。

\- `view`: **只读**。不修改状态变量，不触发事件。（例如：查询余额）

\- `pure`: **纯函数**。既不读取也不修改状态。（例如：数学计算 `1+1`）

\- **Default (无修饰)**: 可读可写。

\### 1.3 引用类型 (Reference Types)

不同于值类型（uint, bool），引用类型通常较复杂，需要处理存储位置（Storage vs Memory）。

\#### A. 数组 (Arrays)

\- **固定长度**: `uint[5] arr;` (Gas 较低，长度不可变)

\- **动态长度**: `uint[] arr;` (主要使用，支持 `push()` 和 `pop()`)

\#### B. 结构体 (Structs)

用于自定义复杂数据类型，类似 C 语言的 struct 或 JS 的 Object。

\`\`\`solidity

struct Student {

uintid;

string name;

uint score;

}

\`\`\`

\#### C. 映射 (Mapping)

Solidity 中最常用的存储结构，类似哈希表（Hash Map）。

\- 语法: `mapping(KeyType => ValueType) public varName;`

\- **特点**: 无法遍历（除非配合数组使用），默认所有 Key 对应的 Value 都是初始值（如 0）。

\---

\## 2. 实战演练：简易学生管理系统

为了串联上述知识点，我编写了一个 `ClassRoom` 合约，实现了学生信息的录入、查询和管理。

\### 2.1 代码实现

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20;

contract ClassRoom {

// --- State Variables ---

// 定义学生结构体

struct Student {

uint256 id;

string name;

uint256 score;

bool isRegistered; // 用于判断是否存在

}

// 老师的地址（只有老师能录入成绩）

address public teacher;

// 1. 使用 Mapping 存储：通过 ID 快速查找学生

mapping(uint256 => Student) public students;

// 2. 使用动态数组：为了能够遍历所有学生的 ID

uint256\[\] public studentIds;

// --- Constructor ---

constructor() {

// 部署合约的人成为老师

teacher = msg.sender;

}

// --- Modifiers ---

// 自定义修饰符：权限控制

modifier onlyTeacher() {

require(msg.sender == teacher, "Only teacher can perform this action");

\_; // 继续执行函数体

}

// --- Functions ---

/\*\*

\* @dev 录入新学生信息 (写操作)

\* 使用了 storage 引用更新状态

\*/

function addStudent(uint256 _id, string memory_ name, uint256 \_score) public onlyTeacher {

// 检查是否已存在

require(!students\[\_id\].isRegistered, "Student already exists");

// 写入 Mapping

students\[\_id\] = Student(\_id, _name,_ score, true);

// 写入 Array

studentIds.push(\_id);

}

/\*\*

\* @dev 获取学生姓名 (View 函数)

\* 读取状态，不消耗 Gas (如果是外部调用)

\*/

function getStudentName(uint256 \_id) public view returns (string memory) {

require(students\[\_id\].isRegistered, "Student not found");

// 从 storage 读取

return students\[\_id\].name;

}

/\*\*

\* @dev 获取班级总人数

\*/

function getClassSize() public view returns (uint256) {

return studentIds.length;

}

/\*\*

\* @dev 纯计算演示 (Pure 函数)

\* 不涉及任何链上数据

\*/

function calculateBonus(uint256 \_score) public pure returns (uint256) {

return \_score + 10;

}

}

\`\`\`

\---

\## 3. 学习心得与总结 (Summary)

1\. **Storage vs Memory**:

在函数参数中使用 `string` 或 `struct` 时，必须显式声明 `memory`（临时存储）或 `calldata`。这是初学时最容易报错的地方。

2\. **Mapping 的局限性**:

Mapping 非常高效，但不能直接通过 `length` 获取长度，也不能直接遍历。因此在实战中（如上面的 `ClassRoom` 合约），通常需要配合一个数组 `studentIds` 来辅助记录 Key，从而实现遍历功能。

3\. **权限控制**:

结合 `msg.sender` 和 `require` 进行权限控制是 Web3 开发的核心思维。相比 Web2 的 Session/Cookie，这种基于地址的鉴权更加原生和直接。

\---

\## 4. 下一步计划

\- 深入学习 **Events (事件)**，了解前端如何监听合约状态变化。

\- 学习 **Inheritance (继承)** 和 **Interface (接口)**，为编写 ERC-20 代币做准备。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->



Course Note: WTF Academy Solidity 101

\> \[!info\] 课程信息

\> - **来源**: \[WTF Academy\]([https://www.wtf.academy/course/solidity101](https://www.wtf.academy/course/solidity101))

\> - **进度**: Chapter 1-3

\> - **状态**: 进行中

\## 1. Hello Web3

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.4;

contract HelloWeb3 {

string public \_string = "Hello Web3!";

}

\`\`\`

\- **License**: 必须写 `SPDX-License-Identifier`。（这里如果你不写的话，虽然可以编译，但是会报错）

\- **Pragma**: 指定编译器版本。

按住 CTRL +S 进行对应的编译

\## 2. 值类型 (Value Types)

\- **Boolean**: `bool public _bool = true;`

\- **Integer**: `uint` (unsigned, 正整数), `int` (signed, 包含负数)。常用 `uint256`。

\- **Address**: `address` (20字节), `address payable` (可以转账 ETH)。

Solidity 中的整数类型包括有符号整数和无符号整数。它可以存储高达 256 位的整数或数据单元

\- **枚举**（最多256个）：在solidity中的用户自定义数据类型，主要用于给常量赋予名称，从而保持易于阅读

\-使用enum（不是类型还是枚举成员）：用一种固定的名称常量表示有限状态

之后这里联想到我们之前设计的一种状态机的思维，利用我们之前的状态机思维，我们这里使用require（），当满足了require的条件之后，才能再执行的后续的操作，不然会在这里陷入阻塞状态

\## 3. 函数 (Functions)

\- 结构: `function <name>(<params>) <visibility> <stateMutability> returns (<return types>)`

\- **Visibility**:

\- `public`: 内部外部均可调用。

\- `private`: 只能本合约调用。

\- `external`: 只能外部调用 (Gas 略少)。

\- `internal`: 只能本合约及子合约调用。

\- **State Mutability**:

\- `pure`: 不读不写链上状态。

\- `view`: 只读不写链上状态。

\- `payable`: 可以接收 ETH。

\- (默认): 可读可写。

\## 4. 练习记录

\- \[x\] 编写一个 `add` 函数，实现两个 uint 相加。

\- \[x\] 尝试定义一个 `address payable` 变量。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->




会议纪要: Web3 社区运营与活动策划实战 SOP

\> \[!info\] 会议档案

\> - **主题**: 社区运营基础 & 活动策划全流程

\> - **日期**: 2026-01-19

\> - **关联**: \[\[Web3-实习手册\]\]

\> - **状态**: ✅ 已整理 (体系化优化版)

\## 📖 1. 核心理念：Web3 运营的 "3C" 模型

Web3 运营不仅是聊天和发推，而是围绕共识 (Consensus) 构建生态。

\- **Connect (连接)**: 不仅是连接人与人，更是连接 **Wallet to Wallet**。

\- **Contribute (贡献)**: 社区不是单向输出，必须设计机制让用户去 Build (如翻译、写教程、修 Bug)。

\- **Consensus (共识)**: 最终目标是让 Holder 变成 Believer。

\---

\## 🛠️ 2. 活动策划全流程 SOP (标准作业程序)

\> 💡 **SOP 核心逻辑**: 筹备(40%) -> 宣发(30%) -> 执行(20%) -> 复盘(10%)

\### 📅 Phase 1: 策划与定调 (T-7)

**1\. 灵魂三问 (The Why)**

\- **目的**: 是为了拉新 (New Wallets)？促活 (Active Users)？还是品牌宣发？

\- **受众**:

\- _Degen_: 关注收益、赔率、Alpha。

\- _Builder/Dev_: 关注技术栈、黑客松、Grant。

\- _Newbie_: 关注教程、入门、防骗。

\- **形式**: AMA (Twitter Space/Discord Stage) vs Workshop (Zoom/腾讯会议) vs 线下 Meetup。

**2\. 激励设计 (The Hook)**

\- **常规**: USDT/Token 空投 (最直接但容易招来羊毛党)。

\- **Web3 原生**:

\- **POAP/OAT**: 链上出席证明 (Galxe/Link3)。

\- **Whitelist (白名单)**: 早期参与资格。

\- **Role (身份组)**: Discord 特殊 OG 身份。

\### 📣 Phase 2: 宣发节奏 (T-3 to T-0)

| 时间节点 | 动作 (Action) | 渠道 (Channel) | 关键物料 |

| :--- | :--- | :--- | :--- |

| **T-3** | **官宣 (Announcement)** | Twitter, Discord 公告 | 海报 (含时间/嘉宾/Link3链接) |

| **T-2** | **预热 (Warm-up)** | 合作社区转发, TG群发 | 文字话术 + 抽奖活动 (RT+Like) |

| **T-1** | **提醒 (Reminder)** | Discord Event 提醒 | 倒计时海报, 收集问题 (AMA预埋) |

| **T-0** | **开场前 15min** | 所有社群 "Starting Soon" | 直播间链接 (Link) |

\### 🎬 Phase 3: 现场执行 (The Show)

**Run-of-Show (流程控制表)**

1\. **Warm-up (5min)**:

\- 播放背景音乐 (Lofi)。

\- 主持人暖场，测试麦克风，提醒签到链接 (Galxe)。

2\. **Intro (5min)**:

\- 介绍背景、嘉宾 Title（一定要准确，Web3 很看重 Title）。

3\. **Panel/Sharing (40min)**:

\- 核心内容输出。主持人负责 **控时** 和 **拉回话题**。

4\. **Q&A (10min)**:

\- **技巧**: 优先回答预先收集的高质量问题，再开放现场举手 (Hand raise)。

\- **防 FUD**: 遇到攻击性问题，委婉跳过或官方口径回答，不要争吵。

5\. **Wrap-up (5min)**:

\- 总结金句。

\- **CTA (Call to Action)**: "关注推特"、"加入 Discord"、"去填表领奖励"。

\### 🔄 Phase 4: 复盘与沉淀 (Post-Event)

\- **内容沉淀**:

\- **录音/录屏**: 上传 YouTube/Spotify。

\- **Recap**: 将核心观点整理成 Thread (推文串) 或 Mirror 文章。

\- **数据清洗**:

\- 剔除明显的 Bot 地址 (如刚创建无交互的钱包)。

\- 分析 **CAC (获客成本)** = 总奖金 / 真实新增人数。

\---

\## ❓ 3. 常见痛点与解决方案 (Troubleshooting)

\### Q1: 社区全是“羊毛党”和机器人 (Bot) 怎么办？

\- **解决方案**: **设置门槛 (Gating)**。

\- 使用 [**Guild.xyz**](http://Guild.xyz) 设置持仓门槛 (如持有 0.01 ETH 或特定 NFT 才能进群/领奖)。

\- 奖励设计倾向于 **SBT (不可转移代币)** 或 **积分**，而非直接发 USDT。

\- 增加\*\*交互难度\*\*: 必须在 Discord 发言升到 Level 3，或完成链上交互任务。

\### Q2: 活动没人来 (Cold Start) 怎么办？

\- **解决方案**: **BD (商务拓展) 互推**。

\- 找体量相当的项目方 (Co-host) 联合举办。

\- 去热门 Discord 社区申请 "AMA 互换"。

\- 利用 **Galxe/QuestN** 等任务平台的流量池（但要注意转化率）。

\### Q3: 遇到 FUD (Fear, Uncertainty, Doubt) 怎么办？

\- **场景**: 有人在群里喊 "项目方跑路了"、"币价暴跌"。

\- **SOP**:

1\. **Fact Check**: 确认是否属实。

2\. **透明沟通**: 如果是误会，通过官方公告澄清。

3\. **隔离噪音**: 引导到专门的 "建议反馈区"，不要让负面情绪在主 Chat 蔓延。

4\. **铁拳**: 对恶意造谣、刷屏广告者，直接 Ban。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->





# 每日学习日志 - 2026‑01‑18

## 学习信息

**日期**: 2026‑01‑18  
**星期**: 星期日  
**学习时长**: 3小时  
**学习地点**: 家

## 学习内容

### 今日目标

-   通读比特币白皮书（英文版至少 1 遍；中文对照查疑点）
    
-   Solidity 入门：能读懂一个最小合约结构并手写 Counter + event
    
-   建立可复用的“白皮书阅读输出模板”（费曼法）
    

### 学习模块

**主要模块**: \[\[区块链基础\]\]

**具体内容**:

1.  比特币白皮书（主线通读）
    
    -   按章节通读（Abstract -> Conclusion）
        
    -   每章输出 3 句话（问题/方案/为什么可行）
        
    -   标注 3 个关键术语并写自己的定义
        
2.  Solidity 入门（最小闭环）
    
    -   读懂合约骨架：state variable / function / event / constructor / modifier
        
    -   读懂数据位置：storage / memory / calldata（先理解差异，不追底层实现）
        
    -   手写 Counter 合约（count + inc + event）
        
3.  Foundry / OpenZeppelin（了解工具链与复用组件）
    
    -   了解 Foundry：forge/anvil/cast 各自做什么
        
    -   了解 OpenZeppelin：Ownable / ReentrancyGuard / ERC20/721 的定位
        

### 资源使用

-   比特币白皮书
    
-   Solidity 文档
    
-   Foundry 文档
    
-   OpenZeppelin 文档
    

## 学习笔记

### 重点概念

**概念1**: 比特币想解决的问题是什么？

-   定义: 解决数字货币“无需可信第三方”情况下的支付问题，尤其是防止双花（double spending）。
    
-   特征: 利用点对点网络 + 工作量证明机制来构建去中心化账本。
    
-   理解程度:
    

**概念2**: Solidity 合约的最小心智模型

-   中心化系统: 服务器控制状态与计算，由平台单点决策。
    
-   去中心化系统: 合约逻辑和状态记录在区块链上，全网共同验证。
    
-   区别: 合约只能被交易/调用驱动，没有外部主动行为或定时执行。
    

* * *

### 白皮书输出区（按章节/或先写主线）

**白皮书主线（5 句话版本）**

1.  当前的电子支付依赖中心化机构，存在信任成本和效率瓶颈。
    
2.  提出一种点对点电子现金系统，用于直接在线支付，无需第三方信任。
    
3.  利用工作量证明的分布式时间戳链来记录交易并防止双花。
    
4.  去中心化网络的最长链规则保证了全网对有效交易历史的一致性。
    
5.  这样的设计搭配激励机制使得系统对攻击具有鲁棒性，并激励参与者维护网络。
    

**关键术语（至少 3 个）**

-   **Proof‑of‑Work（PoW）**：通过计算证明工作量的机制，用于在分布式网络中建立一致的时间戳链和安全共识。
    
-   **Double‑spending（双花）**：同一个金额被重复花费的问题，比特币设计需确保不会发生。
    
-   **Longest chain（最长链/最大累计工作量）**：全网选择含有最多工作量证明的链作为有效账本的规则。
    

* * *

### 代码示例

```
// 最小 Counter 合约示例（带 event）
pragma solidity ^0.8.0;

contract Counter {
    uint256 public count;             // 状态变量：计数器
    event CountIncremented(uint256);  // 事件：每次增加触发

    constructor() {
        count = 0;                    // 合约初始化：计数器初始为 0
    }

    function increment() public {
        count += 1;                  // 计数器加 1
        emit CountIncremented(count);// 触发事件
    }
}
```

* * *

### 遇到的问题

问题1

**问题描述**: PoW 如何同时保证去中心化与安全？  
**解决方案**: 理解 PoW 提供了计算难度，使得伪造历史链很难；长期工作量最大链被多数节点采纳。  
**参考资料**: `resources/refs/2026‑01‑18/bitcoin‑whitepaper‑zh_cn.md`  
**状态**: ✅ 已解决

问题2

**问题描述**: Solidity 合约各种数据位置（storage/memory/calldata）在调用中差异是什么？  
**解决方案**: 阅读官方文档并做例子对比（比如 storage 与 memory 在函数中的生命周期不同）。  
**参考资料**: `resources/refs/2026‑01‑18/solidity‑structure‑of‑a‑contract.html`  
**状态**: ✅ 已解决

* * *

## 练习完成情况

### 练习1: 白皮书费曼输出

-   完成
    
-   理解
    
-   需要复习
    

### 练习2: 手写 Counter 合约

-   完成
    
-   理解
    
-   需要复习
    

* * *

## 学习成果

### 掌握的知识点

-   白皮书主线（双花 → PoW 时间戳链 → 最长链）
    
-   Solidity 合约结构（状态/函数/事件）
    
-   Foundry 工具链基本命令（forge/anvil/cast）
    

### 完成的任务

-   将今日离线资料落盘到 `resources/refs/2026‑01‑18/`
    
-   完成白皮书通读与输出
    
-   完成 Counter 合约编写
    

### 学习进度更新

-   今日学习时长: 3小时
    
-   累计学习时长: 8小时
    
-   进度更新: 12%
    

* * *

## 反思与总结

### 今日收获

1.  比特币白皮书核心逻辑彻底理解：问题 → 设计 → 安全假设。
    
2.  Solidity 最小合约结构可以手写 + 能解释每一部分。
    
3.  构建了白皮书通读模板，可复用到其它白皮书学习。
    

### 难点与困惑

1.  PoW 最长链细节需要结合 math 深入理解。
    
2.  白皮书部分密码学术语需查资料辅助。
    
3.  激励机制与经济模型暂未完全掌握。
    

### 学习方法调整

-   做得好的: 注重输出而非抄写，实践代码加深记忆。
    
-   需要改进: 白皮书细节拆解可对照章节更精细。
    
-   调整方案: 明日继续二刷白皮书分章细读并写概念图。
    

* * *

## 明日计划

### 明日目标

-   用 Foundry 跑通 `forge init` + `forge test`
    
-   白皮书第二遍：重点 SPV / Incentives / Calculations
    
-   Solidity 数据位置（storage/memory/calldata）做 2 个小例子
    

### 学习重点

-   模块: Foundry 实操 + 白皮书二刷
    
-   内容: forge/anvil/cast；SPV/激励/安全假设
    
-   预计时长: 2小时
    

### 准备工作

-   复习今日内容
    
-   预习明日内容
    
-   准备学习材料
    

* * *

## 附加内容

### 灵感与想法

-   把“白皮书输出模板”固化：每章 3 句话 + 术语表 + 反例/攻击者视角。
    
-   Solidity 学习最好配合测试（Foundry）建立反馈回路。
    

### 相关链接

-   `resources/refs/2026‑01‑18/bitcoin‑whitepaper‑en.pdf`
    
-   `resources/refs/2026‑01‑18/bitcoin‑whitepaper‑zh_cn‑notes.md`
    

### 待办事项

-   把白皮书主线 5 句写满
    
-   Counter 合约写完并逐行解释
    
-   记录 3 个不懂的问题（明天优先解决）
    

* * *

## 标签

#每日学习 #学习日志 #2026‑01‑18 #Web3

* * *

_创建时间: 2026‑01‑18_  
_最后更新: 2026‑01‑18_
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->







# 每日学习日志 - 2026-01-17

## 学习信息

**日期**: 2026-01-17  
**星期**: 星期六  
**学习时长**: 2小时

## 学习内容

### 今日目标

-   掌握区块、交易、共识机制等基础术语
    
-   理解区块链的分类和应用场景；理解以太坊的核心概念和设计理念
    
-   学习 Solidity 核心概念（Foundry, Solidity, OpenZeppelin）
    

### 学习模块

**主要模块**: \[\[01-区块链基础\]\]

**具体内容**:

1.  区块链基础概念
    
    -   理解区块链定义
        
    -   掌握核心特征
        
    -   了解应用场景
        

模块一笔记：基础术语（区块 / 交易 / 共识机制）

**区块（Block）**

-   定义（要点）：区块是“交易的批处理容器”，并且包含前一个区块的哈希/引用，从而把区块串成链；历史中任意区块的数据一旦被改动，会导致后续区块引用失效，从而很容易被全网发现并拒绝。
    
-   在以太坊里，区块大致可以理解为：每个 slot里由随机选出的 validator 提议一个区块，区块里包含执行层的交易（execution payload），其他 validator 会重放交易并投票确认其有效。
    
-   看区块结构时常见字段：
    
-   `parent_hash / parent_root`：父区块引用（把链连起来）
    
-   `state_root`：执行完本区块交易后得到的“全局状态”的根哈希
    
-   `transactions` / `transactions_root`：交易列表或其根哈希（Merkle/Trie root）
    
-   `timestamp`：区块时间
    
-   `gas_limit` / `gas_used`：本区块可用/实际消耗的计算资源
    

**交易（Transaction）**

-   定义（以太坊）：交易是由账户发起并被加密签名的“状态变更指令”。它不是“转账”这么窄——也可以是调用合约、部署合约等。（以太坊官方文档）
    
-   以太坊交易中最常见/最关键字段：
    
    -   `from`：发送者地址（EOA；合约账户不能主动发起交易）
        
    -   `to`：接收者地址（EOA 转账 / 合约调用；部署合约时可能为空/特殊）
        
    -   `signature`：签名，用来证明发送者授权了这笔交易
        
    -   `nonce`：发送者账户的交易序号（防重放，并保证同一账户交易顺序）
        
    -   `value`：转移的 ETH 数量（单位通常是 wei）
        
    -   `input data`：可选数据（合约调用参数/部署字节码等）
        
    -   `gasLimit`：最多允许消耗的 gas 单位
        
    -   `maxPriorityFeePerGas`：给 validator 的小费上限（tip）
        
    -   `maxFeePerGas`：你愿意为每单位 gas 支付的最高总价（包含 base fee + tip）
        
-   手续费（EIP-1559 核心概念）：每个区块都有 `base fee`（由网络根据拥堵动态调整，并被 burn）；交易还可以额外给 `priority fee` 作为打包激励。（EIP-1559 / 以太坊官方文档）
    
-   生命周期（简化）：签名并广播 → 进入 mempool → 被 proposer 选中打包进区块 → 其他节点重放验证 → 区块逐步 justified / finalized（最终性增强）。
    

**共识机制（Consensus）**

-   定义：共识机制解决“没有中心管理员的情况下，全网如何对同一份交易历史（哪一个区块序列）达成一致”。
    
-   PoW（工作量证明，Bitcoin 代表）：通过消耗算力把交易打包进区块，节点接受累计工作量最多（最长/最重）的链作为权威历史；这是为了解决双花并在开放网络里同步账本。
    
-   PoS（权益证明，Ethereum 代表）：validator 通过质押 ETH 参与出块与投票；作恶可能被罚没（slash）。以太坊把时间切成 slot（12 秒）和 epoch（32 个 slot），每个 slot 选出 proposer 出块，同时委员会投票；finality 通过 checkpoint 投票达到“经济上不可逆”。
    

**补充术语（快速）**

-   Hash（哈希）：把数据映射成固定长度摘要；改一点点输入输出就会大变。
    
-   Merkle Tree / Root：把交易哈希组织成树/根，用于证明“某笔交易在某个区块里”。
    
-   Mempool：未被打包进区块的待确认交易池。
    
-   Fork（分叉）：短期内出现两个候选链头；共识的 fork-choice 规则会让全网最终收敛到同一条链。
    
-   Double Spend（双花）：同一资产被花两次；共识要防止。
    

* * *

费曼学习法总结区

1.  我用一句话解释
    

-   区块是：交易数据的集合，构成了区块链。
    
-   交易是：账户发起并签名的、改变区块链状态的指令。
    
-   共识机制是：解决全网如何达成一致以验证交易历史的规则。
    

2.  我能举一个生活类比
    

-   区块像：银行账单，每一张记录了一笔交易。
    
-   交易像：银行账户的资金转移或变更。
    
-   共识像：银行为了确认交易的合法性，大家必须共同验证并同意。
    

3.  我最容易混淆的点
    

-   混淆点 1：PoS 和 PoW 的不同（原因：两个机制都是为了确认区块，理解差异有点混乱）
    
-   混淆点 2：区块链的分类（原因：区块链种类很多，涉及的技术栈也很广泛）
    

4.  我还没搞懂的问题
    

-   问题 1：PoS 的 validator 选举与权益关系？
    
-   问题 2：以太坊的 gas 是如何计算的，和其他区块链有何不同？
    

* * *

## 练习完成情况

### 练习1: 区块链术语理解

-   完成
    
-   理解
    
-   需要复习
    

### 练习2: Solidity 基础语法练习

-   完成
    
-   理解
    
-   需要复习
    

* * *

## 明日计划

### 明日目标

-   理解区块链的分类及应用场景
    
-   掌握以太坊核心设计与机制
    
-   学习 Solidity 开发环境设置与基础语法
    

### 学习重点

-   模块: \[\[01-区块链基础\]\]
    
-   内容: 区块链的分类与应用场景，Solidity 开发基础
    
-   预计时长: 2-3小时
    

### 准备工作

-   复习今日内容
    
-   预习明日内容
    
-   准备 Solidity 学习环境
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->









终于补齐了一些基础的认知，终于能够和大家一起聊一些话题了，但是我得赶紧制定一个小计划借助AI的力量（学无止境）  
我的 Web3Career Bootcamp 双线学习计划（5 周｜技术 + 运营并行）

我现在的目标很明确：**技术+运营都要能上手，但技术达到“能做出作品、能解释清楚、能继续自学”的入门水平就够了**。  
之后根据AI辅助自身，制定对应的计划如下：  
第 1 周：Web3 基础认知 + 工具入门（先把“会用”打通）

### 我这一周的目标

-   技术：把 Web3 的基础概念搭起来，并且**能完成一次真实链上交互**
    
-   运营：融入 Bootcamp 社群，知道怎么提问、怎么交流、怎么输出
    

### 我这周要做的事

**技术线（3h/天）**

-   装好 MetaMask，创建钱包，备份助记词（这一步不能敷衍）
    
-   领测试币，做一笔测试网交易，搞清楚交易/签名/Gas 是怎么回事
    
-   跑通 **MyFirstNFT**：亲手 mint 一枚测试 NFT（这就是我的“第一次链上作品”）
    
-   有余力再玩 **MyFirstLayer2**，先感受 L2 的交互体验差异
    

**运营线（1.5h/天）**

-   同样完成 MyFirstNFT（运营也必须会基础链上动作，不然没法沟通）
    
-   看 Bootcamp 的《Web3 实习手册》入门章节，建立术语感
    
-   进群报到，自我介绍，开始参与答疑流程
    
-   每天在群里输出 1–2 条学习心得（哪怕短，关键是持续）
    

### 这周我怎么验收（必须可检验）

-   技术：我能在区块浏览器上找到我 mint NFT 的交易记录，并能解释：
    
    -   交易从发起到上链大概发生了什么
        
    -   Gas 是什么、钱包签名在干嘛
        
-   运营：我能用自己的话跟别人解释“Web3/区块链/NFT”是啥，并且已经在社群里完成基础互动（提问/回答/分享）
    

* * *

# 第 2 周：智能合约入门开发 + 内容产出（开始“做出东西 + 讲清楚”）

### 我这一周的目标

-   技术：写出并部署第一个 Solidity 合约，走完完整流程
    
-   运营：产出并发布一篇 Web3 主题内容，让别人能看懂、能反馈
    

### 我这周要做的事

**技术线**

-   学 Solidity 基础语法：变量、函数、mapping、event
    
-   用 Remix 或 Hardhat 把一个合约写出来并部署到测试网（比如 Sepolia）
    
-   Etherscan 验证合约代码（让它可公开、可验证）
    
-   做一个小扩展：计数器 / 简化 ERC20 / 简单存储合约
    
-   写基础测试（Hardhat 或 Foundry 二选一），至少覆盖核心函数
    
-   顺便建立安全意识：知道重入、溢出、权限控制这些风险长什么样
    
-   Rust 部分只做“感知”：装 Rust 工具链，看一篇 Solana/Substrate 教程，知道差异即可（不强求深入）
    

**运营线**

-   选一个主题写短文（比如：DAO 是什么、新人怎么参与治理）
    
-   营内请教拿反馈 -> 修改 -> 发布（Mirror/公众号/营内论坛都行）
    
-   同步写一段“摘要 + 推广帖”（训练社交传播表达）
    
-   每天继续打卡输出，保持曝光和节奏
    

### 这周验收标准

-   技术：合约部署成功 + 浏览器可查 + 我能交互调用 + 测试能跑通
    
-   运营：文章发布 + 内容准确、结构清晰 + 拿到至少一轮反馈互动
    

* * *

# 第 3 周：合约项目实战 + 社区活动策划（进入“作品集模式”）

### 我这一周的目标

-   技术：独立完成一个更像样的小项目（合约 + 测试 + 部署），做成作品集
    
-   运营：主导策划一场真实活动，把运营技能落地
    

### 我这周要做的事

**技术线**

-   选一个项目做到底：众筹 / 投票 / Token Vendor / 简易 DEX 思路都行
    
-   测试要覆盖正常流程 + 异常流程 + 简单攻击场景（安全意识从这开始）
    
-   代码上 GitHub，README 写清楚别人怎么跑
    
-   有余力做个超简陋前端，让别人能“连接钱包 + 点按钮交互”
    
-   跨链部署一次（Polygon/BSC 测试网），对比体验差异
    
-   Rust 进阶可选：跟 Anchor 做一个计数器 demo（能跑起来就算赢）
    

**运营线**

-   选活动形式：AMA / Twitter Space / Workshop
    
-   输出完整活动方案：主题、目标人群、流程、分工、宣传节奏、应急预案
    
-   对接嘉宾/讲者，做海报/预热文案，提前 3–5 天预热
    
-   报名表/活动群/流程清单（Notion + 表单工具）
    
-   这周末或下周初执行（如果安排在第4周执行，本周必须筹备到位）
    

### 这周验收标准

-   技术：项目可运行 + 测试覆盖充分 + 合约部署成功 + 文档可复现
    
-   运营：活动方案完整且推进到可执行状态（宣传已发、流程已排、分工明确）
    

* * *

# 第 4 周：安全/优化 + 活动执行复盘 + 求职对接（把成果变成“可投递材料”）

### 我这一周的目标

-   技术：把项目做“更像真实项目”，补安全、补文档、补表达
    
-   运营：活动落地并复盘，开始准备简历和岗位对接
    

### 我这周要做的事

**技术线**

-   用 Slither 跑静态分析，按警告逐条修（别逃避）
    
-   找人 code review（导师/同学），把可读性和结构拉起来
    
-   写完整文档：部署方式、交互方式、功能说明、注意事项
    
-   准备一个 Demo（2–3 分钟讲清楚项目解决什么、怎么用、亮点在哪）
    
-   做一次 Gas/性能优化尝试（知道怎么思考成本）
    

**运营线**

-   把活动执行到位：主持、节奏、互动、记录、应急
    
-   活动后发回顾帖 + 1–2 天内完成复盘报告（数据 + 问题 + 改进）
    
-   更新简历与作品集：把链上链接、GitHub、活动材料都整理出来
    
-   开始投递/对接岗位：筛岗位清单、模拟面试、真实投递
    

### 这周验收标准

-   技术：项目质量明显提升（安全警告消掉、文档齐、能 Demo）
    
-   运营：活动办完 + 复盘报告像样 + 简历作品集可直接投递
    

* * *

# 第 5 周（延伸）：复盘巩固 + 长期计划（从学员切换成“自驱型”）

### 我这一周的目标

-   技术：查漏补缺，把薄弱点补上，形成自己的技能矩阵
    
-   运营：把所有成果整理成可公开展示的作品集，持续建立个人品牌
    

### 我这周要做的事

**技术线**

-   做一份“知识点清单”，每项标掌握程度
    
-   弱项专项补：Rust/L2/前端/安全/算法（按我未来方向选重点）
    
-   贡献一次开源 / 做一个小工具（哪怕很小，但要真实产出）
    

**运营线**

-   整理作品集：文章链接、活动方案+海报+复盘、链上证明、GitHub
    
-   写一篇总结：“我如何从 0 开始进入 Web3”（让自己沉淀，也让别人认识我）
    
-   继续推进求职，跟进投递与人脉连接（Twitter/LinkedIn/社群）
    

### 这周验收标准

-   我能清楚说出自己的强项/弱项，以及未来 3–6 个月怎么补
    
-   我有一份能公开展示的作品集（不是“我学过”，而是“我做过”）
    

今天晚上听了下同学的分享感觉同学们都太厉害了，从运营叫我们起号（还是原来的老几套，制造营销唬头，一个普通人如何快速达到一个大进步，共情一下之后马上让他们呢yy，让他们觉得自己能够达到这个程度，之后给他们设置情景，还是成为了爽文）  
然后是技术向的同学的分享，因为我的solidity目前还没有完全学完，所以大部分东西还是有点云里雾里但是还是可以继续认真往下学习这些东西，也是终于结束期末考试了，web3正式开始学习，我会证明自己的，我超级厉害的
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->










## 区块链到底是什么：区块、链、交易、状态

我们先从最直观的开始：**区块链，本质上是一套公开账本**。

### 区块（Block）

你可以把区块理解成“账本的一页”。  
这一页里写着一堆交易——谁给谁转了多少钱、谁调用了哪个合约、合约状态变成了什么。

而最关键的是：这页账本里还会写一个东西：**上一页的指纹**（上一页的哈希）。

### 链（Chain）

于是区块跟区块就串起来了。  
因为每一页都引用上一页的指纹，所以你想偷偷把历史改掉，就会发生一件事：

你改了旧的那一页，指纹变了，下一页引用就对不上了，再往后每一页都会对不上。  
这就是我们常说的“难以篡改”。

这里我想强调一个更准确的说法：  
区块链不是“绝对不能改”，而是“**改的成本非常高**”。  
高到你几乎无法在现实世界里对抗整个网络。

### 交易（Transaction）与状态（State）

到这里再往下走一层：交易其实不是“转账”这么简单。  
**交易的本质是：对系统状态发起一次改变。**

你可以把区块链想成一个巨大的状态机：

-   状态：当前世界的快照（余额、合约存储、NFT 归属、借贷仓位）
    
-   交易：输入指令
    
-   执行：状态发生转移
    

所以你一旦理解“区块链是状态机”，你会发现：  
智能合约、DeFi、NFT，本质上都只是“状态变化”而已。

* * *

## 2\. 两种账本模型：UTXO vs Account

接下来我们要解决一个容易被忽略的问题：  
同样是账本，为什么比特币和以太坊的“记账方式”不一样？

### UTXO（比特币典型）

UTXO 你可以理解成：**你手里拿着一张张“未花掉的收据”**。  
你要花钱，就把收据交出去，然后系统给你找零，生成新的收据。

在这种模型里，“余额”不是一个数字字段。  
你的余额是你手里所有 UTXO 加起来的总和。

它的好处是：结构很清晰、安全模型很稳定。  
但它不太擅长表达复杂的状态（这也是为什么“智能合约平台”更喜欢另一种模型）。

### Account（以太坊典型）

Account 模型更像我们熟悉的银行账户：  
你有一个账户，里面就有余额，合约也有自己的存储状态。

所以以太坊能更自然地做智能合约，是因为它天生就是“账户 + 全局状态”这种结构。

这一点你记住就行：

-   UTXO：像收据的集合
    
-   Account：像账户余额表 + 合约状态表
    

* * *

## 3\. 密码学最小集：为什么我能“验证”，而不是“相信”

到这里，你会发现区块链的可信，不是说“大家都很诚实”。  
恰恰相反，是因为系统假设大家可能不诚实，所以它用数学让你能验证。

### Hash（哈希）

哈希你可以理解成：数据的“指纹”。  
任何数据只要变一点点，指纹就完全不同。

所以：

-   区块引用上一块的哈希 → 构成链
    
-   数据有没有被改 → 一比对哈希就知道
    

### Merkle Tree（默克尔树）

如果区块里有几千笔交易，我怎么证明“某一笔交易真的在这个区块里”？  
难道我要把整个区块下载一遍？

Merkle Tree 解决的是这个问题：  
它让你只用很小的一段“证明路径”，就能验证某笔交易在不在区块里。

这也是轻节点能存在的原因。  
也就是说：区块链不仅要可信，还要**可扩展地验证**。

### 数字签名（ECDSA / EdDSA）

签名解决的问题更关键：  
**你怎么证明这笔交易是你发的？**

在 Web3 里，“身份”不是靠用户名密码，而是靠私钥签名：

-   私钥：你保管，决定控制权
    
-   公钥：别人用来验证你
    
-   签名：证明“是你授权的、内容没被改、你不能抵赖”
    

这就是为什么私钥丢了就是丢了——  
不是平台不帮你找，而是“证明你是你”的能力消失了。

* * *

## 4\. 共识：为什么全网会认同同一本账？

到这里你会发现：  
哈希、签名解决的是“我怎么验证”，  
但还有一个更难的问题：**大家为什么会达成一致？**

这就是共识机制在做的事。

共识的核心目标很简单：  
让一群互不信任的节点，对“哪份历史是真的”达成一致。

### PoW（工作量证明）

PoW 的逻辑是：  
谁付出更多计算成本，谁更有概率出块。

它把安全建立在“现实成本”上：电力、算力、硬件。  
你要想改历史，就得重做大量工作，而且还要超过全网算力。

### PoS（权益证明）

PoS 的逻辑是：  
谁质押更多资产、并遵守规则，谁获得出块资格。

它把安全建立在“经济成本”上：  
你作恶就会被罚（slashing），你的质押资产会被惩罚。

所以 PoS 不是“更文明”，它只是把安全方式从“烧电”变成了“押钱+罚钱”。

### 威胁模型：双花、长程攻击、女巫攻击

你会听到几个典型攻击名词，它们其实都在问同一个问题：  
**攻击者如何伪造历史并让大家相信？**

-   双花：同一笔钱花两次
    
-   女巫攻击：伪造很多身份影响共识（所以共识必须绑定成本）
    
-   长程攻击：PoS 特别要注意历史很久以后被伪造（所以需要检查点/最终性）
    

* * *

## 5\. 经济激励：为什么需要手续费、为什么需要质押/惩罚？

最后一层，是很多人以为“只是钱”，但其实它是系统安全的一部分。

### 为什么要手续费？

因为区块空间是有限资源：

-   没有手续费 → 垃圾交易刷爆网络
    
-   有手续费 → 资源被定价，拥堵时自动筛选交易
    

你可以把手续费理解为：  
不是平台抽成，而是“网络资源价格”。

### 为什么要质押/惩罚？

因为共识不是靠道德约束的，它要靠理性约束。

质押的本质是：  
把作恶成本内生化，让你“作恶就亏钱”。  
惩罚的本质是：  
让作恶变成负期望，系统才能长期稳定。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->











# 什么是web3？

**先明白一个简单的概念：web3是**可读可写可拥有，向我们之前很熟悉的NFT（游戏道具，数字艺术）就是web3  
然后我们来明确一些概念性的东西：  
区块链（Blockchain）：  
三个关键词：账本｜多人维护｜不可篡改  

![Blockchain vs Traditional Databases: What's the Difference and Why ...](https://miro.medium.com/1%2Axjm5BPNgscY3qMhnaqwiUA.png)

所以我们首先会看到一个非常重要的点：  
**对于 Web3 来说，它的“信任”并不是来自某一个机构，而是来自数学与共识。**

放到区块链层面，这意味着什么？  
意味着一旦数据被写入区块链，在现实条件下它几乎是无法被篡改的，它会长期存在、持续可验证。这很有意思，你可能会想那我拥有的数字资产岂不是永远是我的了。

但这里需要注意的是：  
**区块链并不是“数据永远只属于你”，而是“只要你掌握私钥，数据对应的资产和权利就由你控制”。**  
没有银行、没有平台可以单方面替你更改这件事。

在这个基础之上，我们再引出一个更关键的概念：**去中心化**。

在传统金融体系中，我们通常会说它是中心化的——  
因为信任的中心，往往集中在银行、交易所或者清算机构手中。  
谁记账、谁结算、谁有最终解释权，基本都是明确的。

那 Web3 呢？  
Web3 并不是把“中心”换成了另一个公司，而是**尽量让“中心”消失**。  
记账不再由某一家机构完成，而是由一整套公开的规则和分布在全球的节点共同维护。

换句话说：  
在 Web2 和传统金融中，我们信任的是“某个组织”；  
而在 Web3 中，我们尝试信任的是“系统本身”。

所以说去**中心化 ≠ 没有中心**，而是没有“**特权中心**”  
在这里，很多人会对 Web3 产生一个误解：  
既然它强调去中心化，那是不是意味着“没有中心、没有规则、没有约束”？实际上恰恰相反。  
Web3 不是没有规则，而是**规则被写得更死了**。

在传统金融和 Web2 体系中，规则往往是“可以被解释的”。  
比如：

1.  银行可以因为合规原因冻结账户
    
2.  平台可以修改用户协议
    
3.  交易可以被回滚或撤销
    

这些规则并不是不存在，而是**掌握在少数中心化机构手中**。

在说回web3：  
但这里就会出现一个更有意思的问题。

你会发现，在 Web3 里，**规则反而“看不见”了**。  
它们不再写在公告里、不再出现在用户协议中，也很少有人会真正逐行去读代码。

那问题来了：  
**当规则不在明面上时，资本还能不能操作？**

**很遗憾的是，资本依旧可以操作，但是资本不再是像国内的A股市场控制规则，而是通过影响环境来发挥作用（比如说：大量持仓，影响流动性，做市，叙事影响市场行为），这意味着资本也需要服从规则了。**

所以我们需要认识到：

**“去中心化”并没有消除掉有大量资金的资本的优势，而是消除了所谓的特权**  
总而言之：web3就是一个相对公平的平台，数据几乎无法更改，资本也无法拥有绝对的特权，这在一定程度上提高了你作为个体户的要求，在去中性化市场中，你获得了“**选择**”也拥抱了**“风险”**  

## web3将参与者分为了几类人？（这一部分为AI总结）  

## 第一类：规则的使用者 → **普通用户 / 散户**

**典型角色**

-   普通 DeFi 用户
    
-   NFT 买家
    
-   钱包用户
    

他们：

-   通过前端使用协议
    
-   很少研究底层机制
    
-   更关注“能不能用、赚不赚钱”
    

在行业里，这一类人通常被称为：

> **Retail / Users**

他们是生态的基础流量，  
但也是**最依赖他人判断的一群人**。

* * *

## 第二类：规则的理解者 → **研究员 / 成熟交易员**

**典型角色**

-   DeFi Researcher
    
-   Protocol Analyst
    
-   有体系的交易员
    

他们：

-   研究协议机制
    
-   理解 Token 经济
    
-   能识别系统性风险
    

他们未必写代码，  
但已经具备：

-   独立判断协议安全性的能力
    
-   理解收益来源的能力
    

在 Web3 行业中，  
**这是从“用户”跨入“参与者”的分水岭。**

* * *

## 第三类：规则的设计者 → **开发者 / 协议创始人**

**典型角色**

-   Smart Contract Developer
    
-   Protocol Architect
    
-   Core Contributor
    

他们：

-   编写智能合约
    
-   设计激励与约束
    
-   决定系统运行方式
    

在 Web3 里：

> **代码 = 产品 + 规则 + 法律**

这类人，  
实际上掌握着系统最深层的影响力。

* * *

## 第四类：规则的博弈者 → **专业交易员 / 做市商 / 套利者**

**典型角色**

-   Professional Trader
    
-   Market Maker
    
-   Arbitrageur
    

他们：

-   对机制极度敏感
    
-   利用流动性、时间差、结构差
    
-   在规则允许范围内压榨效率
    

他们并不“破坏系统”，  
但会逼迫系统暴露弱点。

**很多协议的设计是否成熟，  
就是被这一类人“交易出来的”。**

* * *

## 第五类：规则的资本化者 → **投资人 / 基金 / VC**

**典型角色**

-   Crypto VC
    
-   DAO Treasury
    
-   Strategic Investor
    

他们：

-   提供早期资本
    
-   参与治理
    
-   影响长期方向
    

他们的影响力来自：

-   资金体量
    
-   持仓结构
    
-   网络资源
    

去中心化并没有消灭资本，  
只是让资本的影响路径**更加透明**。

* * *

## 第六类：规则的叙事者 → **内容创作者 / 社区建设者**

**典型角色**

-   KOL / Research Writer
    
-   Community Manager
    
-   DAO Organizer
    

他们：

-   不直接改代码
    
-   不一定交易
    
-   但影响共识形成
    

在 Web3 中：

> **共识本身，就是一种生产要素**

## 然后回到自身：我本身是一个传统web2的开发出身（默认精通一门工程语言），那么想要快速转型到web3的话，我们需要做什么?  
  
**第一：web3的素养（区块链基础原理）**  

1.  区块链是什么：区块、链、状态、交易、账本模型（UTXO vs Account）
    
2.  **密码学最小集**：Hash、Merkle Tree、数字签名（ECDSA/EdDSA 的“使用与含义”）
    
3.  **共识机制**：PoW / PoS 的目标、威胁模型（双花、长程攻击、女巫攻击）
    
4.  **经济激励**：为什么需要手续费、为什么需要质押/惩罚  
    上述问题，我会在明天的笔记中完成对应的学习  
    

最后针对晚上老师讲的东西做一个简单的复盘：  
你要在web3中充当一个什么样的角色，你要怎末从450人中成为那个唯一的人：

针对技术向来说，我目前还不太会Solidity，之后老师推荐我们学习Rust，  
然后基于我的TS基础之下，我决定：  
主线先用 Solidity 把“协议研发”打通；Rust 作为第二技能同步起步，但用“可控的投入”逐步拉起来。  
基于gpt的学习项目模块化推荐：  
主线要学到什么（按能力模块拆）  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/liziye627-design/images/2026-01-12-1768224743220-image.png)

**模块 A：Solidity 语言与合约思维**

-   基础语法：类型、函数、事件、错误、modifier
    
-   核心结构：mapping/array/struct、继承与接口、库
    
-   权限体系：Ownable、Role、Timelock、Pausable
    
-   关键思维：**状态机**（你写的不是“流程”，是“状态 + 转移”）
    

**模块 B：EVM 执行与 Gas（拉开水平差距的地方）**

-   storage/memory/calldata 的区别
    
-   storage layout（slot、mapping/array 的布局）
    
-   delegatecall / call / staticcall 的语义差异
    
-   gas 成本为何不同（什么写法更省）
    

**模块 C：工程化（能不能上线的分水岭）**

-   Foundry：单测、fuzz、fork 主网测试
    
-   部署脚本、环境管理、可复现运行
    
-   失败路径测试（revert、边界、权限、极端输入）
    

**模块 D：DeFi 基元（协议开发必须懂“钱如何流”）**

-   AMM：swap、LP、手续费、滑点
    
-   借贷：抵押、利率、清算
    
-   Oracle：TWAP / 外部预言机思想、操纵窗口
    
-   Tokenomics：奖励释放、激励副作用（短期 TVL vs 长期安全）
    

> 你会发现：语言只是入口；真正深度来自 **EVM + 工程化 + 机制**。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/liziye627-design/images/2026-01-12-1768224633733-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/liziye627-design/images/2026-01-12-1768225052829-image.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
