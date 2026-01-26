---
timezone: UTC-5
---

# Keming Liu

**GitHub ID:** kmiliu

**Telegram:** @kmiliu

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
# 🧱 Scaffold-ETH & Solidity 实战 Notes（Week 2 Day 5）

> 核心目标：  
> **用 Scaffold-ETH 快速「看见 Solidity 在跑」**，用 UI + Burner Wallet 形成高频反馈循环，而不是一上来就 Hardhat + Tests。

* * *

## 一、为什么用 Scaffold-ETH（而不是一开始就 Hardhat）

### Hardhat 的特点

-   **Test-driven**：写测试 → 写合约 → 跑测试
    
-   非常严谨，但**学习曲线陡**
    
-   初学时很难「感受到合约在干嘛」
    

### Scaffold-ETH 的优势

-   **所见即所得**
    
-   自动生成：
    
    -   前端 UI
        
    -   Burner Wallet
        
    -   Faucet
        
    -   合约交互界面
        
-   **极快迭代循环**：
    
    ```
    改 Solidity → yarn deploy → UI 立刻更新
    ```
    

👉 结论：  
**学习 / 原型阶段 → Scaffold-ETH**  
**稳定版本 / 上链前 → Hardhat + Tests**

* * *

## 二、Scaffold-ETH 基本工作流（必背）

```
git clone scaffold-eth
cd scaffold-eth
yarn install       # 一次性，慢
yarn chain         # 启动本地 Hardhat 节点
yarn start         # 启动前端
yarn deploy        # 部署合约
```

### 核心概念

-   **yarn chain**：本地链
    
-   **yarn deploy**：部署合约（可 reset）
    
-   **yarn start**：React 前端
    
-   **Burner Wallet**：自动生成的钱包（incognito）
    

* * *

## 三、Solidity 基础（用 Scaffold-ETH 玩）

### 1️⃣ 全局变量（Globals）

```
block.timestamp
msg.sender
msg.value
tx.origin
```

示例：

```
function timey() public view returns (uint256) {
    return block.timestamp;
}
```

* * *

### 2️⃣ 基础类型（Primitives）

-   `bool`
    
-   `uint256`
    
-   `address`
    
-   `string`
    

示例：

```
bool public flag = true;

function toggle() public {
    flag = !flag;
}
```

* * *

### 3️⃣ Ether vs Wei（超重要）

-   Solidity **默认单位是 wei**
    
-   `1 ether = 10^18 wei`
    

```
uint256 public price = 0.001 ether;
```

UI 中要手动输入：

```
0.001 * 10^18
```

👉 Scaffold-ETH **强制你意识到单位换算**

* * *

## 四、Mapping（不能遍历！）

```
mapping(address => uint256) public count;

function inc() public {
    count[msg.sender] += 1;
}
```

特点：

-   默认值是 `0`
    
-   **不能遍历**
    
-   非常适合余额 / 计数器
    

* * *

## 五、Events（前端的生命线）

```
event PurposeSet(address sender, string purpose);

emit PurposeSet(msg.sender, purpose);
```

作用：

-   便宜（比 storage 便宜）
    
-   前端监听用
    
-   Scaffold-ETH UI 会自动显示
    

* * *

## 六、Ownership（不要自己写！）

❌ 自己写 `boss + modifier`

```
require(msg.sender == boss);
```

✅ 正确方式：**OpenZeppelin Ownable**

```
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyContract is Ownable {
    function onlyBoss() public onlyOwner {}
}
```

### Owner 初始化

-   默认：部署者（Hardhat 的 `f39...`）
    
-   解决方式：
    
    1.  deploy script 里 `transferOwnership`
        
    2.  或 constructor 中 `super.transferOwnership(addr)`
        

* * *

## 七、Payable / Receive / Withdraw（资金流）

### Receive（直接转账）

```
receive() external payable {}
```

如果没有：

-   ❌ 直接转 ETH 会失败
    

* * *

### Withdraw（⚠️正确写法）

❌ 错误（transfer，2300 gas）

```
payable(msg.sender).transfer(address(this).balance);
```

✅ 正确（call）

```
(bool ok,) = msg.sender.call{value: amount}("");
require(ok);
```

* * *

## 八、msg.sender vs tx.origin（面试必考）

### 场景

```
EOA → 合约 A → 合约 B
```

| 变量 | 在 B 中的值 |
| --- | --- |
| msg.sender | 合约 A |
| tx.origin | EOA |

### 限制 EOA

```
require(tx.origin == msg.sender);
```

⚠️ 实际生产中 **慎用 tx.origin**

* * *

## 九、合约与合约交互（Contract → Contract）

### Bank 合约（示例）

```
mapping(address => uint256) public balance;

function deposit() public payable {
    balance[msg.sender] += msg.value;
}

function withdraw() public {
    uint256 amt = balance[msg.sender];
    balance[msg.sender] = 0;
    (bool ok,) = msg.sender.call{value: amt}("");
    require(ok);
}
```

* * *

### 另一个合约调用 Bank

```
import "./Bank.sol";

Bank public bank;

constructor(address bankAddr) {
    bank = Bank(bankAddr);
}

function myBalance() public view returns (uint256) {
    return bank.balance(address(this));
}
```

* * *

## 十、Reentrancy（⚠️重点安全）

### ❌ 漏洞写法

```
function withdraw() public {
    (bool ok,) = msg.sender.call{value: balance[msg.sender]}("");
    balance[msg.sender] = 0;
}
```

### ✅ 正确顺序（CEI）

```
uint256 amt = balance[msg.sender];
balance[msg.sender] = 0;
(bool ok,) = msg.sender.call{value: amt}("");
require(ok);
```

* * *

### Reentrancy 攻击本质

-   外部合约在 `receive()` 中 **再次调用 withdraw**
    
-   状态尚未更新 → 无限提款
    

* * *

## 十一、学习路线建议（非常重要）

### 学习顺序

1.  Scaffold-ETH → 玩
    
2.  Solidity by Example
    
3.  攻击案例（Reentrancy、Overflow）
    
4.  Ethernaut（强烈推荐）
    

🔗 Ethernaut：

> OpenZeppelin 官方 Solidity 安全挑战
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->

## Scaffold-ETH + Solidity 深度实操笔记

* * *

## 一、今天在干什么（总体目标）

**核心目标：**

> 用 **Scaffold-ETH** 代替纯 Hardhat，进入一个  
> **“写 Solidity → 立刻部署 → 前端直接交互 → 快速验证想法”**  
> 的高频迭代开发模式。

**为什么不用一开始就写测试？**

-   Hardhat = Test Driven（慢但严谨）
    
-   Scaffold-ETH = Idea Driven（快、直观、适合学习 & 原型）
    
-   推荐流程：
    
    1.  Scaffold-ETH 中「乱试 + 理解机制」
        
    2.  确定逻辑后 → 再回 Hardhat 写测试
        

* * *

## 二、Scaffold-ETH 基本工作流（一定要记住）

### 1️⃣ 安装 & 启动流程

```
git clone https://github.com/scaffold-eth/scaffold-eth
cd scaffold-eth
yarn install      # 很慢，耐心
yarn chain        # 启动本地 Hardhat 节点
yarn start        # 启动前端
yarn deploy       # 部署合约
```

> ⚠️ 合约改了但没更新？  
> 👉 用：

```
yarn deploy --reset
```

* * *

### 2️⃣ Scaffold-ETH 的核心优势

-   自动前端 UI（函数、变量直接显示）
    
-   自动 Burner Wallet（无需 MetaMask）
    
-   合约热更新
    
-   Events 自动监听
    
-   极适合 **tinker / debug / learn**
    

* * *

## 三、Solidity 基础回顾（结合 Scaffold-ETH）

### 1️⃣ Globals（全局变量）

常用：

```
msg.sender
msg.value
block.timestamp
tx.origin
```

示例：

```
function time() public view returns (uint256) {
    return block.timestamp;
}
```

* * *

### 2️⃣ 基本数据类型

```
bool flag;
uint256 number;
address owner;
```

简单函数：

```
function toggle() public {
    flag = !flag;
}
```

* * *

### 3️⃣ Ether / Wei

-   Solidity 中**所有金额默认是 Wei**
    
-   推荐写法：
    

```
uint256 public price = 0.001 ether;
```

前端测试时要：

```
0.001 * 10^18
```

* * *

## 四、Scaffold-ETH Wallet 机制（非常重要）

### Burner Wallet

-   每个浏览器 / incognito 都是一个新账户
    
-   自动生成、自动管理
    
-   Faucet 一键给钱
    
-   极适合测试权限 / 攻击 / 多账户交互
    

* * *

## 五、Ownership & OpenZeppelin（重点）

### ❌ 不推荐自己写 owner

```
address boss;
modifier onlyBoss { ... }
```

### ✅ 推荐：继承 OpenZeppelin

```
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyContract is Ownable {
    function foo() public onlyOwner {}
}
```

* * *

### Ownership 转移的两种方式

方式 1：deploy script 中转移

```
await contract.transferOwnership(frontendAddress);
```

（多一次交易，gas 多）

方式 2（推荐）：constructor 中转移

```
constructor() {
    transferOwnership(0xYourAddress);
}
```

（**一次交易完成，gas 更低**）

* * *

## 六、Events & 前端监听

### Solidity

```
event SetPurpose(address sender, string purpose);

emit SetPurpose(msg.sender, purpose);
```

### Scaffold-ETH

-   前端自动监听
    
-   Events 直接显示在 UI
    
-   **比 storage 便宜，适合日志 & UI 状态**
    

* * *

## 七、Mapping & Token 简化实现

### 基础 Token 最小实现

```
mapping(address => uint256) public balances;

function transfer(address to, uint256 amount) public {
    balances[msg.sender] -= amount;
    balances[to] += amount;
}
```

-   Solidity ^0.8 自动防溢出
    
-   Mapping：
    
    -   ❌ 不能遍历
        
    -   ✅ 查余额极快
        

* * *

## 八、Receive / Fallback（送钱相关）

### 没有 receive → 不能直接收钱

```
receive() external payable {}
```

### 高级玩法：自动转发

```
receive() external payable {
    deposit();
}
```

👉 用户 **直接给合约转账 ≈ 调用 deposit()**

* * *

## 九、Withdraw：⚠️ 最容易写错的地方

### ❌ 错误写法（易被攻击）

```
msg.sender.call{value: balance}("");
balances[msg.sender] = 0;
```

### ✅ 正确写法（防重入）

```
uint256 amount = balances[msg.sender];
balances[msg.sender] = 0;
(bool ok, ) = msg.sender.call{value: amount}("");
require(ok);
```

📌 原则：

> **先更新状态 → 再外部调用**

* * *

## 十、Contract → Contract 交互（核心难点）

### 1️⃣ 部署顺序

1.  Bank 合约
    
2.  YourContract(bankAddress)
    

### 2️⃣ Solidity 绑定方式

```
Bank public bank;

constructor(address bankAddr) {
    bank = Bank(bankAddr);
}
```

### 3️⃣ 调用方式

```
bank.deposit{value: msg.value}();
```

* * *

## 十一、msg.sender vs tx.origin（必考点）

### 场景图

```
EOA → Middleware → Bank
```

| 变量 | 在 Bank 里是谁 |
| --- | --- |
| msg.sender | Middleware 合约 |
| tx.origin | 最初 EOA |

### 限制只允许 EOA

```
require(tx.origin == msg.sender);
```

⚠️ **生产环境慎用 tx.origin**（容易被钓鱼）

* * *

## 十二、重入攻击（Re-entrancy）完整演示

### 攻击原理

1.  Bank 调用 attacker.withdraw()
    
2.  attacker.receive() 中再次调用 withdraw()
    
3.  balance 还没清零 → 钱被反复拿
    

### 防御核心一句话

> **Checks → Effects → Interactions**

* * *

## 十三、学习路径建议（非常重要）

### 短期

-   Scaffold-ETH 疯狂 tinker
    
-   Solidity by Example
    
-   Ethernaut（OpenZeppelin）
    

### 中期

-   回 Hardhat
    
-   写测试
    
-   Audit mindset
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


# Uniswap Notes

## 一、Uniswap 的核心思想（一句话总览）

> **Uniswap 是一种基于 AMM（自动做市商）的去中心化交易协议，用数学规则而非订单簿来定价，通过套利机制让链上价格逼近市场价格。**

* * *

## 二、Uniswap V2：AMM 的“经典模型”

### 1️⃣ 定价机制：恒定乘积公式

-   **公式**：
    
    x⋅y=kx \\cdot y = kx⋅y=k
    
-   **价格定义**：
    
    p=yxp = \\frac{y}{x}p=xy​
    

👉 含义：

-   不管你怎么交易，池子里两种资产的乘积不变
    
-   价格由池子内资产比例**自动决定**
    
-   **没有人“报价”，只有数学**
    

* * *

### 2️⃣ 交易 & 套利机制

-   当池内价格 ≠ 市场真实价格
    
-   **套利者进场交易**
    
-   把价格“推”回合理区间
    

👉 关键理解：

-   套利者不是坏人
    
-   **他们是 AMM 正常运作的一部分**
    
-   Uniswap **靠套利来纠偏，而不是靠管理员**
    

* * *

### 3️⃣ 流动性提供（LP）与无常损失（IL）

-   LP 向池子提供两种资产
    
-   赚取：**交易手续费**
    
-   承担：**价格波动风险**
    

📌 无常损失本质：

> **相对于“什么都不做、直接持币”，LP 因价格变化而产生的相对损失**

-   是 AMM 机制的**结构性结果**
    
-   ❌ 无法消除
    
-   ✅ 只能用更高手续费、激励机制补偿
    

* * *

### 4️⃣ 闪电贷（Flash Loan）

-   特点：**先借 → 同一笔交易内还**
    
-   如果没还，整笔交易回滚
    

👉 意义：

-   极大提升资金效率
    
-   支持套利、清算、复杂组合操作
    
-   **也是 DeFi 组合性的代表机制**
    

* * *

### 5️⃣ TWAP（时间加权平均价格）

-   用多区块平均价格
    
-   作为**链上价格预言机**
    

👉 目的：

-   防止被单笔交易操纵
    
-   给其他协议一个“相对稳健”的价格参考
    

* * *

## 三、Uniswap V3：从“平均铺开”到“精准布防”

> **V3 的核心升级：集中流动性（Concentrated Liquidity）**

* * *

### 1️⃣ 流动性区间（Range）

-   LP 不再必须覆盖 (0,+∞)(0, +\\infty)(0,+∞)
    
-   可选择一个**价格区间**提供流动性
    

👉 结果：

-   💰 同样资金 → 更高资本效率
    
-   ⚠️ 价格跑出区间 → 不再赚手续费
    

* * *

### 2️⃣ Tick 系统（价格离散化）

-   连续价格 → 离散 Tick
    
-   Tick 是流动性变化的“分界点”
    

👉 本质：

-   用工程方式管理“在哪些价格段有多少流动性”
    

* * *

### 3️⃣ 手续费与流动性聚合

-   多个 LP、多个区间
    
-   按：
    
    -   所在区间
        
    -   提供比例  
        分配手续费
        

📌 手续费档位（重要记忆点）：

-   稳定币：0.01% / 0.05%
    
-   主流资产：0.3%
    
-   高波动资产：1%
    

* * *

### 4️⃣ 数学优化：L 与 √P

-   用 **√P（平方根价格）** 表达价格
    
-   用 **L 表示流动性强度**
    

👉 意义：

-   减少计算复杂度
    
-   提升合约执行效率
    
-   **为大规模使用打基础**
    

* * *

## 四、Uniswap V4：走向“可编程 AMM”

> V4 不只是“更快”，而是 **更自由**

* * *

### 1️⃣ Hooks（钩子机制）

-   可在以下节点插入自定义逻辑：
    
    -   交易前 / 后
        
    -   添加 / 移除流动性时
        

* * *

### 2️⃣ 能做什么？

-   动态手续费
    
-   限价单
    
-   自动调仓
    
-   防 MEV / 抢跑逻辑
    
-   定制化流动性策略
    

👉 一句话总结：

> **AMM 从“固定规则” → “可插拔策略框架”**

* * *

## 五、常见问题（你笔记里的 Q&A 已经很好，我帮你提炼成“考点版”）

### 🔹 无常损失能避免吗？

-   ❌ 不能（机制固有）
    
-   ✅ 只能通过手续费与激励对冲
    

### 🔹 滑点 vs 价格影响

-   **价格影响**：池子深度决定的、可计算
    
-   **滑点**：实际成交偏差，包含不确定性
    

### 🔹 V3 区间怎么设？

-   高波动 → 宽区间 / 高费率
    
-   稳定币 → 窄区间 / 低费率
    

### 🔹 如何防抢跑？

-   Flashbots
    
-   合理滑点
    
-   V4 Hooks 定制保护逻辑
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->



# Solidity 智能合约开发入门

**主线**：**EVM 是运行环境 → Gas 决定成本 → Solidity 是控制成本和安全的语言 → ERC20 是最小可复用应用**

* * *

## Part 0：你在写的到底是什么？

### 0.1 智能合约的本质

-   智能合约 = **部署在链上的程序**
    
-   特点：
    
    -   不可篡改
        
    -   永久存在
        
    -   每一步执行都要付 Gas
        
-   结论：**Solidity 的第一目标不是“优雅”，而是：安全 + 省 Gas**
    

* * *

## Part 1：EVM —— 一切 Solidity 行为的底层原因

### 1.1 EVM 是什么？

-   EVM = 以太坊虚拟机
    
-   **栈式虚拟机（Stack-based VM）**
    
-   所有 Solidity 代码最终都会：Solidity → Bytecode → EVM 指令
    

理解 EVM = 理解为什么某些写法贵、某些危险

* * *

### 1.2 EVM 四大执行区（核心）

1️⃣ Stack（栈）

-   用于计算
    
-   只存临时操作数
    
-   极快、极便宜
    
-   例：`1 + 1`
    

2️⃣ Memory（内存）

-   函数执行期间的临时空间
    
-   不上链
    
-   Gas 成本中等
    

3️⃣ Calldata（调用数据）

-   外部传入参数
    
-   **只读**
    
-   Gas 成本低
    
-   典型：`external` 函数参数
    

4️⃣ Storage（链上存储）

-   **永久存储**
    
-   写一次 ≈ 20,000 gas
    
-   修改 ≈ 5,000 gas
    
-   **合约优化的核心战场**
    

📌 类比：

-   Storage = 硬盘
    
-   Memory / Calldata = 内存
    
-   Stack = CPU 寄存器
    

* * *

## Part 2：Gas 成本

### 2.1 Gas 决定一切

合约设计只围绕两件事：

1.  安全性
    
2.  Gas 成本
    

### 2.2 Storage 是最贵的

-   能不存就不存
    
-   能少改就少改
    
-   能算出来就别存
    

👉 **80% 的 Gas 优化 = Storage 优化**

* * *

## Part 3：Solidity 开发环境（落地工具）

### 3.1 Remix IDE

-   在线 IDE
    
-   自带：
    
    -   EVM
        
    -   私链
        
    -   Debug
        
-   适合学习 + 原型验证
    

### 3.2 Solidity 版本意识

-   教学基准：`0.8.20`
    
-   `^0.8.20` ≠ 任意版本
    
-   **读合约源码第一件事：看版本**
    

* * *

## Part 4：Solidity 类型系统（直接决定 Gas）

### 4.1 值类型（Value Types）

> **便宜、安全、首选**

-   `bool`
    
-   `uint / int`（位宽越小越省）
    
-   `address`
    
-   `bytes32`
    

核心原则：能用 uint / address，就不要 string

* * *

### 4.2 引用类型（Reference Types）

> **贵、慎用**

-   `string`
    
-   动态数组
    
-   `bytes`
    
-   `mapping`
    

### 4.3 Mapping 的地位

-   Solidity 的“账本结构”
    
-   常见用途：
    
    -   余额
        
    -   授权
        
    -   状态标记
        

* * *

## Part 5：ERC20 —— 第一个完整应用模型

### 5.1 为什么从 ERC20 学？

-   最小可用合约系统
    
-   覆盖：
    
    -   Storage
        
    -   权限
        
    -   事件
        
    -   安全设计
        

* * *

### 5.2 ERC20 核心链上状态

```
string name;
string symbol;
uint8 decimals;
uint256 totalSupply;
address owner;
mapping(address => uint256) balances;
mapping(address => mapping(address => uint256)) allowances;
```

* * *

### 5.3 两本账本 = ERC20 的灵魂

1️⃣ 余额账本  
2️⃣ 授权账本

* * *

## Part 6：构造函数（部署即初始化）

-   只执行一次
    
-   初始化所有 Storage
    
-   **部署阶段就要付 Gas**
    

结论：constructor 里写的每一行，都是永久成本

* * *

## Part 7：函数设计（权限 + 可见性）

### 7.1 可见性

-   `external`（最省 gas）
    
-   `public`
    
-   `internal`
    
-   `private`
    

### 7.2 状态可变性

-   `view`
    
-   `pure`
    

* * *

## Part 8：ERC20 核心方法逻辑

### 8.1 transfer（最重要）

执行顺序：

1.  校验
    
2.  更新余额
    
3.  发事件
    

### 8.2 approve

-   修改授权账本
    
-   发 Approval 事件
    
-   **DeFi 的入口**
    

* * *

## Part 9：事件（Event）—— 链上到链下的桥梁

### 9.1 事件是干嘛的？

-   不给链上用
    
-   给：
    
    -   钱包
        
    -   区块浏览器
        
    -   Bot
        
    -   DApp 后端
        

### 9.2 indexed 的意义

-   最多 3 个
    
-   用于链下高效过滤
    

* * *

## Part 10：Mint 与安全设计

### 核心模式

```
external（权限校验）
   ↓
internal（状态修改）
```

* * *

## Part 11：完整 Remix 流程

1.  编译
    
2.  部署（constructor 执行）
    
3.  调用（读 vs 写）
    

* * *

## Part 12：错误处理（Gas + 安全）

优先级建议：

1.  自定义 error
    
2.  revert
    
3.  require
    
4.  assert（极少用）
    

* * *

## Part 13：unchecked（高级优化）

-   0.8+ 默认防溢出
    
-   确定安全后才用
    

* * *

## Part 14：Modifier —— Solidity 的“规则抽象”

### 为什么重要？

-   权限统一
    
-   逻辑复用
    
-   减少 bug
    

* * *

## Part 15：认知纠偏 Q&A（安全与生态）

### Q1：助记词骗局是怎么实现的？

-   合约权限锁
    
-   或抢跑 Bot（Front-running）
    
-   **不是“漏洞”，是机制被利用**
    

* * *

### Q2：发币要几个合约？

-   一个 ERC20 = 一个合约
    
-   参数不可变（除非你写升级）
    

* * *

### Q3：EVM 有并发吗？

-   没有多线程
    
-   有 **重入风险**
    

* * *

### Q4：新标准是否兼容 ERC20？

-   不兼容
    
-   但生态共存
    

* * *

### Q5：还能用哪些成熟轮子？

-   OpenZeppelin
    
-   Proxy
    
-   Diamond（EIP-2535）
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->




# 以太坊中文分享

![NotebookLM Mind Map.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/kmiliu/images/2026-01-19-1768827456773-NotebookLM_Mind_Map.png)

## **Synchronous Composability for Based and Sequenced Rollups**

Research explores combining the low latency of sequenced rollups with the synchronous composability of based rollups.

• **Rollup Types**:

    ◦ **Based Rollups**: L1 determines transaction ordering; rollup blocks are L1 transactions.

    ◦ **Sequenced Rollups**: Offchain mechanisms (e.g., centralized sequencers) determine ordering, regularly committing history to L1.

• **Proposed Hybrid Design**: Uses three block types:

    ◦ **Regular sequenced blocks**: Frequent blocks requiring a sequencer certificate.

    ◦ **Slot-ending sequenced blocks**: Blocks that signal it is valid to build a based block on top of them during the current L1 slot.

    ◦ **Based blocks**: Can be built by anyone on top of slot-ending blocks to enable **synchronous composability** with L1 liquidity.

• **Timing Dynamics**: The sequencer plays a "timing game," releasing regular blocks with low latency and then a slot-ending block near the L1 slot's end to allow for based block inclusion.

• **Trade-offs and Challenges**:

    ◦ This design requires rollups to revert if the L1 reverts.

    ◦ Permissionlessness is not inherent because based blocks still require a sequencer certificate for the slot-ending block; a **forced-inclusion channel** on L1 is the suggested path to achieve it.

    ◦ Real-time proving and short "dead time" windows before L1 proposals present technical hurdles for block builders.

\--------------------------------------------------------------------------------

## **MegaETH EVM (mega-evm)**

The `mega-evm` is a specialized Ethereum Virtual Machine tailored for MegaETH, extending `revm` and `op-revm` for high performance.

• **Specs vs. Hardforks**: The codebase distinguishes between **Specs** (defining what the EVM does, e.g., `REX`) and **Hardforks** (defining when those behaviors are activated).

• **Key Feature Sets (Specs)**:

    ◦ **MINI\_REX**: Introduces a **multidimensional gas model** (independent tracking for compute, data, and KV updates), disables `SELFDESTRUCT`, and supports large contracts up to **512 KB**.

    ◦ **REX**: Refined storage gas economics with optimized formulas and a 39,000 storage gas baseline for all transactions.

    ◦ **REX2**: Inherits Rex1 behavior but restores `SELFDESTRUCT` using EIP-6780 semantics.

• **Parallel Execution**: Supports block environment access tracking to detect conflicts for parallel processing.

• **Command Line Tool**: The `mega-evme` binary allows for executing, debugging, and replaying EVM transactions directly from the CLI.

\--------------------------------------------------------------------------------

## **SALT: Small Authentication Large Trie**

**SALT** is an authenticated key-value store designed to eliminate random disk I/Os during state root updates.

• **Efficiency**: It maintains a low memory footprint (1 GB for up to 3 billion KV pairs) and can scale to tens of billions of items.

• **Architecture**:

    ◦ **Two-Tier Structure**: A static **4-level 256-ary main trie** (top tier) and **dynamic buckets** (second tier).

    ◦ **Buckets**: Use **Strongly History-Independent (SHI)** hash tables, which guarantee a canonical commitment regardless of insertion order.

• **Cryptographic Operations**:

    ◦ Uses **Inner Product Arguments (IPA)** with Pedersen commitments for incremental, homomorphic updates.

    ◦ Updating a key-value pair requires only a few elliptic curve multiplications (ECMul), leading to highly efficient state root updates.

• **Proofs**: Provides compact witnesses for both **membership** (key exists) and **non-membership** (key does not exist). Non-membership proofs use SHI invariants to prove a key cannot exist anywhere in its linear probe sequence.

## **MegaETH Stateless Validator**

This is a Rust implementation that allows nodes to verify blocks without maintaining the full blockchain state.

• **Mechanism**: Instead of a local state database, it uses **cryptographic witness data from SALT** to verify state transitions.

• **Key Characteristics**:

    ◦ **Partial Statelessness**: Contract bytecode is fetched on-demand via RPC and cached, significantly reducing the size of the witness.

    ◦ **Parallelism**: Validation workers operate independently on different blocks, allowing throughput to scale linearly with CPU cores.

    ◦ **Multi-Client Support**: Supports multiple execution engines, including a vanilla `revm` interpreter and one based on formal K semantics, to ensure state transition integrity.

• **Trust Model**: The validator acts as an **execution client** to verify the state transition function (STF). It does not verify the canonical chain itself; that task remains for a consensus client like `op-node`.

• **Coordination**: Uses a central **ValidatorDB** with ACID transactions to manage task queues, block data, witnesses, and cached contracts.

NotebookLM can be inaccurate; please double check its responses.
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->





## 1) 总结

分享的核心观点是：**2026 年的 AI 正在从“只会说”变成“能做事并能收钱”的 agent（智能体）**；而要让 agent 真正进入经济活动，需要两类基础设施：

-   **可信身份/可审计行为**（信任与治理问题）
    
-   **可编程支付/微支付与快速结算**（支付与商业模式问题）  
    Web3（区块链）被认为是同时满足这两类需求的候选方案。
    

* * *

## 2) 传统 LLM 为何“只能动嘴”？

Rick 把 2023 年的 LLM 定义为：**自回归 token 预测器**（输入→预测下一个 token 概率→采样→循环）。  
它的三大限制：

1.  **无状态（memory 缺失）**：对话结束就“忘记”，上下文窗口有限。
    
2.  **无工具（tooling 缺失）**：只能生成文本，不能执行、不能调 API、不能操作外部系统。
    
3.  **无身份（identity 缺失）**：不能代表主体签署/交易/管理资产，缺乏可追责性。
    

对应地，**agentic AI** 被描述为：

-   有私有记忆（向量数据库 + 检索增强）
    
-   有工具调用（functions/MCP，形成“思考–行动–观察”的 ReAct 循环）
    
-   有链上身份（ERC-8004）
    

* * *

## 3) AI 智能体的“标准三件套”（脑子 / 记忆 / 手脚）

他给了一个很清晰的组成结构：

-   **LLM = 脑子**：理解意图、制定计划、生成行动指令，但不直接执行。
    
-   **向量库 + RAG = 记忆**：数据采集→embedding→存向量库→相似度检索→拼进 prompt，让 AI 从“金鱼”变成“能记事的大象”。
    
-   **工具调用（Functions/MCP）= 手脚**：LLM 触发函数调用，外部程序执行并把结果回传，形成闭环。
    

* * *

## 4) 为什么要 Web3：AI agent 当前的“三大基础设施缺口”

他认为现在 AI agent 面临一个“能力很强但像黑户”的处境，缺口有三类：

1.  **身份不可验证**：HTTP 世界缺少机制证明请求方是谁（人类/AI？哪个公司/哪个 agent？）
    
2.  **行为不可审计**：日志多在企业私有服务器，容易被修改/删除/伪造；一旦 AI 签错合同，责任追溯困难。
    
3.  **部署不可管控**：组织内部 AI “游击队式接入”，IT/治理体系跟不上，造成隐私与合规风险。
    

结论：区块链的“可验证、不可篡改、可编程”特性与上述需求匹配。

* * *

## 5) “机器经济”为什么需要链上支付（以及 X402）

他从支付角度给了 4 个“传统金融不适配 agent”原因：

-   **验证码/KYC**：传统注册与身份验证本身就是为阻止机器人；但 agent 恰恰需要顺畅接入。
    
-   **微支付成本**：信用卡/银行有固定费率与手续费，小额支付成本过高。
    
-   **结算速度**：传统结算 T+1/T+3，跨境更慢；agent 需要毫秒级、多 API 的即时结算。
    
-   **订阅模式错配**：SaaS 月订阅 vs agent 的高动态、突发负载需求。
    

### X402 的核心想法

-   利用 HTTP 的 **402 Payment Required**（历史上保留状态码）
    
-   当服务端返回 402 时，在响应头给出：支付地址/金额/代币/链信息
    
-   客户端（agent）自动签名支付→返回支付证明→服务端验证后继续提供服务
    
-   验证方式：
    
    -   **链上验证**（查交易哈希，较慢需确认）
        
    -   **签名承诺**（先承诺后结算，快但需要信任/约束机制）
        

并提到微支付的成本问题 → 可能用 **支付通道/批量结算** 来降低频繁上链的 gas 消耗。

* * *

## 6) ERC-8004：AI 的链上“护照”（身份 + 声誉 + 可验证运行）

ERC-8004 被描述成 **AI agent 的链上身份证标准**，核心设计：

### (1) 身份注册表（基于 ERC-721 NFT）

-   每个 agent 对应一个唯一 NFT（全局唯一、可验证、可转让、可编程权限）
    
-   元数据可包含：agent 类型、能力声明、模型哈希（用于验证）、owner 地址等。
    

### (2) 声誉系统（链上评分）

-   0–100 分评分体系（目前偏单维）
    
-   **防刷分机制**：只有与该 agent 有过链上真实交易/调用记录的地址才可评价；评价有成本（gas），权重可能与交易金额相关
    
-   **不可篡改**：上链后不可删除，且设计了 **时间衰减**（近期评价更重要，但不完全抹去历史）
    

### (3) 验证机制（“表里如一”的证明）

-   **TEE**（如 Intel SGX 等）产生证明
    
-   **ZKML**（零知识证明 AI 推理/模型使用）  
    他也承认这类技术仍在早期。
    

* * *

## 7) SpoonOS / SparkOS：把协议封装成“AI × Web3 操作系统”

观点是：协议虽好，但开发者直接对接太复杂、成本高，所以做一个框架把复杂度封装掉：

-   提供统一 LLM 接口与 agent 编排引擎（类似 LangGraph 思路）
    
-   记忆管理（短期上下文 + 长期向量记忆）
    
-   工具调用器
    
-   协议层：集成 ERC-8004（身份/声誉/证明）与 X402（支付网关）
    
-   钱包托管与私钥安全（强调“私钥不离开安全环境”，对外只暴露签名 API）  
    目标：让开发者像做 App 一样做 agent，不必自己造“钱包/签名/支付/身份”的轮子。
    

* * *

## 8) 现场 Q&A 的关键争议点（很值得写进你的总结里）

**Q1：量化交易 agent 需要链上身份吗？**  
A：如果只是自己玩，可以不需要；如果要做成“可募资、可验证策略”的产品，则需要身份与可审计能力来建立信任。

**Q2：Web3 TPS 真的能比 Web2 更大吗？**  
答得有点混：他强调的是“跨境/合规审查/中心化瓶颈/支付摩擦”带来的效率问题，而不是单纯 TPS 数字。  
（你如果发 X，可以写成：**争议点：性能 vs 信任/结算摩擦，二者不是同一指标**。）

**Q3：agent 升级后身份一致性怎么办？**  
回答偏开放：可以换新身份，也可以用某种方式更新（提到把调用信息/IPFS/证明暴露在链上）。  
（这其实是“版本管理 + 声誉继承”的设计难题。）

**Q4：声誉是给 agent 还是 developer？能否作恶后重开小号？**  
A：当前偏 **agent 维度**，确实存在“开发者重开号”漏洞，未来可能需要更完善机制。

**Q5：女巫攻击（刷分/抹黑）怎么防？**  
A：目前没有完美方案，只能提高攻击成本（调用成本/评价成本/权重机制等），协议仍早期。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->






# AI 及其基础概念

### 1\. 什么是 AI 智能体（Agent）？

不是 ChatGPT 那种“你问一句，它答一句”的东西。

而是**一个能长期存在、能记住事情、能用工具、能自己付钱的 AI**

类比一下：

-   ChatGPT = 临时工
    
-   AI Agent = 有身份证 + 银行卡 + 工作能力的“数字员工”
    

### 2\. 为什么光靠 AI 本身不够？

因为 AI 遇到的不是“算法问题”，而是**社会问题**：

| 问题 | 为什么难 |
| --- | --- |
| 身份 | 谁知道你是真 AI 还是假账号？ |
| 信任 | 你说你赚钱了，证据呢？ |
| 支付 | 信用卡不适合 0.001 美元这种交易 |

这就像：**你造出了一个机器人，但没给它身份证、银行账户和法律体系**

### 3\. Web3 在这里到底解决了什么？

不是“去中心化很酷”，而是非常现实的三件事：

（1）AI 怎么付钱？

**X402 协议**：

-   服务器：`402 Payment Required + 收款信息`
    
-   AI：自动签名 → 自动转账 → 继续请求
    

没注册、没登录、没人工介入

（2）AI 怎么“证明自己是谁”？

**ERC-8004 = AI 的链上身份证**

-   每个 AI = 一个 NFT
    
-   所有交易、行为都挂在这个身份下面
    
-   有声誉评分（不能白嫖刷分）
    

像大众点评，但**必须真花钱才能评价**

（3）开发者会不会被 Web3 劝退？

所以有 **Spoon OS**：

你可以理解为：**“给 AI 用的 Web3 操作系统 / SDK”**

它帮你：

-   管钱包
    
-   管身份
    
-   管支付
    
-   管安全
    

你只用写：_“我的 AI 要做什么”_

### 4\. Spoon OS 四层结构（一句话版）

```
输入（数据） → 核心（AI大脑） → 协议（身份+钱） → 输出（和世界交互）
```

如果你是开发者：

-   Spoon OS = 不用从零造区块链轮子
    
-   直接做 AI 产品
    

### 5\. 常见疑问（超级直白版）

**Q：我自己用 AI 量化，还需要链上身份吗？**  
不需要。  
但你要募资、对外提供服务，就必须“可验证”。

**Q：Web3 不是更慢吗？**  
对“跨境 + 小额 + 高频支付”，Web3 反而更快。

**Q：AI 亏钱 / 攻击合约，谁负责？**  
**开发者负责**。  
链上身份的意义是：**跑不了**。

# 领取 Sepolia 测试币

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/kmiliu/images/2026-01-17-1768663428128-image.png)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->








# Co-Learning总结

## 1) QL/BD 想提供 case studies：核心看什么？

**结论：最硬的“案例”不是你写了多少内容，而是你能证明“影响力 + 转化能力”。**

-   **影响力指标**：粉丝量（尤其万粉级别更容易被认可为 KOL/QL）
    
-   **合作逻辑**：品牌/项目方做合作，**优先找头部 KOL**（粉丝多 + 扩散快）
    
-   **“人品”在 Web3 里不一定是负资产**：项目方更在意曝光，“黑红也是红”，用户心理是“我能跑得比别人快，先吃一口”。
    

✅ 你做 BD / KOL 合作的 case study 可以这样写：

-   你如何做 **KOL 分层**（头部/中部/尾部）
    
-   你为什么选择某些头部 KOL（受众匹配、历史数据、互动率/转发率）
    
-   你合作后拿到什么结果（曝光、引流、注册、任务完成数）
    

* * *

## 2) Web3 运营 vs KOL（QL）到底差在哪？

**KOL=品牌宣传口；项目运营=“把本项目做活”。**

-   **KOL**：展示光鲜、打造个人品牌；也有人做爆料/黑料号
    
-   **项目运营**：围绕项目做增长、活动、社群、售后、用户沟通
    
-   本质区别：
    
    -   KOL 面向“市场/公众”
        
    -   项目运营面向“产品/社区的留存和转化”
        

* * *

## 3) 推特/X 做运营：为什么“难受”？你要有心理预期

他说的痛点很真实：

-   推特环境“恶臭”，为了流量**无所不用其极**
    
-   想做流量就得**高强度互动**：天天去蹭大号、抢热评、拼速度
    
-   要持续产出“抽象点子”吸引点赞转发评论 → **耗精力**
    
-   能力画像：要会“见人说人话”，说白了要会蹭（尤其在瓶颈期）
    

**例外情况**：如果你自己能稳定爆款，就不需要蹭，别人会来找你。

* * *

## 4) KOL 商业化：钱怎么来的？

-   “发薪资”通常只会给**顶级 KOL**
    
-   顶级 KOL 常见模式：**年费**（他说他了解到的某段行情是 30 万级别）
    
-   交易所（如 Binance/OKX）也会签一些顶级 KOL（不等于一定要“漂亮”，也有很多靠内容/影响力）
    

* * *

## 5) 运营入行怎么做？（对“运营方向同学怎么提供”很关键）

他给了一个非常实操的路径：

### A. 写研报/分析帖是有效入场方式之一

-   研报写得好，头部 KOL 可能会来私信你做兼职/合作
    
-   因为他们也需要内容供给来维持权重和流量
    

### B. 从 0 运营社群：最快的“增长方式”很现实

-   **买量/买粉**（他说是最快方式之一）
    
-   去“撸毛/任务网站”发布任务：关注/点赞/转发→给积分→TG/空投激励
    
-   也可以做曝光活动、联名增长（他举例：某活动是一次增长合作）
    

### C. 如果没人做任务：怎么排查？

-   先看是不是**任务太难/卡点多/条件苛刻**
    
-   多做用户沟通、降低门槛、调整节点（他举了黑客松任务太难导致延期）
    

* * *

## 6) 数据真相：Web3 自然流量很惨，别被“刷数据”骗

他提到的经验判断：

-   推特自然流量：一条推**200+ 已经算不错**，很多项目在 100 左右
    
-   很多项目方会刷（几万流量/很多点赞不一定真实）
    
-   观察：
    
    -   **流量上万**：点赞可能 <20，转发更多，评论最少
        
    -   **5万/10万级流量**：转发会变得非常大
        

👉 结论：Web3 更像“转发驱动传播”，不是点赞驱动。

* * *

## 7) 冷启动做活动/校园社群：怎么起？

-   **找“号召力强”的人背书**（老师/学生领袖都行），否则很难启动
    
-   找学院/社团/计算机协会等渠道宣传
    
-   国内环境敏感：建议以**兴趣小组**形式，不做太大，避免被约谈
    
-   学校可能担心“炒币/思想渗透”等风险（他举了被盘问半小时的例子）
    

* * *

## 8) 个人经历：他怎么把运营做起来的？（这是你最能复用的“案例模板”）

他的方法论很清晰：

-   观察 7 天推特后发现痛点：**信息碎片化 + 新手无法判断 KOL 是否靠谱**
    
-   做了一个“测评 KOL”的账号：
    
    -   告诉小白：这个 KOL 是否值得看、是什么类型
        
-   因为解决痛点 → 账号起飞（10万+爆帖很多）
    
-   但即便如此，也**很难形成经济正循环**
    
    -   建议：前期当副业，用主业钱补贴
        
    -   Web3 最快万粉路径：**做交易 + 晒单**（但这条路风险/不适合所有人）
        

* * *

## 9) 职业建议：技术/金融/运营怎么选？

他给的“交叉优势论”：

-   Web3 更像 **金融科技**
    
-   **懂技术 + 懂金融 = 王炸**，可以去量化、交易所、合约开发、产品等方向
    
-   只靠 AI 写代码能做个 demo/了解 dApp，但想当工程师不够；  
    做产品/运营则 AI 能替代很多工作
    

* * *

## 10) 其他碎点（也挺有用）

-   项目方做流量不一定为了真实用户，可能是为了“上所/找接盘”
    
-   内推 > 海投：多参加线下活动更容易认识交易所/项目方的人
    
-   城市活动密度：他认为深圳/广州/成都/上海多，北京相对少
    

# 一周例会总结

## Solidity

## 1) 可见性与函数/变量的“对谁开放”

### 1.1 `public`：公开可访问

-   **状态变量 public**：会自动生成一个 getter（外部可以读）。
    
-   **函数 public**：合约内外都能调。
    

### 1.2 `external`：只允许“外部调用”

-   只能被 **EOA（钱包地址）** 或 **其它合约** 调用。
    
-   **合约内部不能直接调用 external 函数**（除非用 `this.f()` 走外部调用，但那会更贵、也更麻烦）。
    

> 你的理解很到位：`external` 往往用于“高频外部调用”来省一些开销/字节码空间（尤其是带 `calldata` 参数时更明显）。

### 1.3 `internal` / `private`（你文里没展开，但建议你补上）

-   `internal`：合约内部 + 子合约可用（类似“受保护”）。
    
-   `private`：仅当前合约内部可用（子合约也不行）。
    

* * *

## 2) 数据类型与默认值（你已经抓到主干了）

### 2.1 常见类型

-   `bool`：`true/false`
    
-   `uint`：无符号整型（默认 `uint256`），也有 `uint8/uint16/...`
    
-   `int`：有符号整型（常用 `int256`）
    
-   `address`：20 字节地址（一般显示成 `0x...`）
    
-   `bytes32`：32 字节定长字节串（常用于哈希/标识）
    

### 2.2 `.min` / `.max`

-   `type(uint).max`、`type(int).min` 这种写法才是 Solidity 标准姿势（你写的“min/max像内置函数”这个方向是对的，只是语法是 `type(T).max`）。
    

### 2.3 默认值（重要！）

-   `bool` 默认 `false`
    
-   `uint/int` 默认 `0`
    
-   `address` 默认是 `address(0)`（也就是 `0x000...000`，不是 `0x1e16`）
    
-   `bytes32` 默认 `0x00...00`
    

* * *

## 3) 状态变量 vs 局部变量：链上“状态”是什么

### 3.1 状态变量（State Variables）

-   **存储在链上**（storage），合约存在就一直在。
    
-   修改它会产生交易、消耗 gas、且不可逆（除非再发交易改回去）。
    

### 3.2 局部变量（Local Variables）

-   函数执行过程中临时存在，函数结束就没了（memory/stack）。
    
-   不上链（除非你把结果写回状态变量）。
    

* * *

## 4) `pure` / `view` / 无修饰：你总结的判断法非常好

你已经总结出一个非常实用的决策顺序（我给它写成“口诀版”）：

> **先看写不写状态：写 → 什么都别加（或按需要加 payable 等）**  
> **不写再看读不读：读 → view；不读 → pure**

-   `pure`：**不读、不写** 状态变量（只用参数/局部变量）
    
-   `view`：**只读不写** 状态变量
    
-   无 `view/pure`：可能读也可能写，**只要写状态就必须不用 view/pure**
    

* * *

## 5) Gas：你抓到了“为什么有些场景免费”的关键差别

你对“读常量为什么还算 gas”这点，其实已经接近本质了：

-   **链下读取（read call）**：比如你在 Remix 点 “call” 的读取，或前端用 `eth_call` —— 不上链，所以不扣你 gas。
    
-   **链上执行（transaction）**：如果你发交易调用写入函数，函数内部顺手读了某个值 —— 这是链上执行的一部分，会计入 gas（只是读比写便宜很多）。
    

### `constant` / `immutable`

-   `constant`：编译期常量，通常更省（值直接内联到字节码里）。
    
-   `immutable`：部署时确定一次，之后不变（常用于 owner、关键地址）。
    

* * *

## 6) 多合约文件部署：Remix 的坑点

-   同一个文件写多个 `contract`，Remix 默认部署下拉框里选中的那个；你发现“只部署了第一个”其实就是没切 `CONTRACT` 下拉框。
    

* * *

## 7) 条件与循环

### 7.1 if / else if / else + 三元

-   `return _x < 10 ? 1 : 2;` 非常常见（简洁）。
    

### 7.2 循环的链上注意事项

-   `for/while` 都能用，但**链上每一次迭代都要 gas**。
    
-   尽量避免不受控循环（尤其是 `while(true)` 这种），容易直接 OOG（out of gas）让交易失败。
    

* * *

## 8) 错误处理三件套：`require` / `revert` / `assert`

你这段理解非常清楚，我帮你压缩成“对外 vs 对内”的区分：

### 8.1 `require(condition, "msg")`

-   用于 **校验外部输入 / 前置条件**（不满足就回滚）
    
-   常见：权限、参数范围、状态检查
    

### 8.2 `revert("msg")` / 自定义 error

-   `revert` 更像你说的：先 `if (bad) revert...`
    
-   **自定义错误**（更省 gas、更规范）：
    
    ```
    error MyError(address caller, uint i);
    ```
    
    然后 `revert MyError(msg.sender, _i);`
    

### 8.3 `assert(condition)`

-   用于 **内部不变量**（理论上永远应该成立）
    
-   触发了通常说明：你代码逻辑/合约状态出了 bug
    
-   也因此它更“严厉”（历史上会消耗更多剩余 gas；你记住“只用于内部逻辑”就够了）。
    

* * *

## 9) `modifier`：可复用的“规则模板”（你说的很准）

### 9.1 核心机制

-   `modifier XXX { ...; _; ... }`
    
-   `_` 是占位符：把被修饰函数的主体“插入”到这里执行。
    

### 9.2 典型模式

-   暂停开关 `whenNotPaused`
    
-   权限 `onlyOwner`
    
-   参数约束 `cap(_x)`
    
-   “三明治”结构：前置逻辑 → `_` → 后置逻辑
    

* * *

## 10) `constructor` 与 Ownable 权限控制

### 10.1 constructor

-   **只在部署时执行一次**
    
-   常用于设置 `owner = msg.sender`、初始化关键参数
    

### 10.2 Ownable（你写的很典型）

-   `onlyOwner` + `setOwner(newOwner)`
    
-   关键注意：新 owner 不允许是 `address(0)`，否则合约会“失主”锁死。
    

* * *

## 11) 函数返回值：多返回 + 解构赋值

### 11.1 多返回

-   `returns (uint, bool)`
    
-   可以命名：`returns (uint x, bool b)`
    

### 11.2 解构

```
(uint x, bool b) = returnMany();
(, bool b) = returnMany(); // 忽略第一个
```

* * *

## 12) 数组：动态 vs 定长，storage vs memory（这一章你学得很关键）

### 12.1 动态数组（常见）

```
uint[] public nums = [1,2,3];
nums.push(4);
nums.pop();
```

### 12.2 定长数组

```
uint[3] public numsFixed = [4,5,6];
```

### 12.3 `delete` 的效果

-   `delete nums[i]` **不会缩短数组**，只是把该位置重置为默认值（`0`）。
    

### 12.4 memory 里创建数组

你最后的理解是正确的：

```
uint;
```

-   `memory` 里不能 `push/pop` 改长度（因为长度固定了）
    
-   但可以 `a[i] = ...` 赋值
    

### 12.5 返回整个数组

-   返回动态数组通常要写：
    

```
function returnArray() external view returns (uint[] memory) {
    return nums;
}
```

* * *

## Hong Kong's Digital Finance Experiment: Can Blockchain IPOs Become the Next Trend?

## A. 文章主线：链上 IPO 为什么“现在”重新变得严肃

**作者不直接给结论“能不能成”，而是用三个层面拆解：宏观环境 → 技术现实 → 商业模型 → 香港窗口。**

### A1. 为什么是现在（3 个宏观前提 + 1 个隐藏变量）

1.  **链上原生货币（稳定币）规模到达阈值**
    

-   逻辑：稳定币已经形成 7x24 的清算/结算网络 → 才可能承载更复杂的资本市场行为（IPO）。
    
-   对比打法：拿链上稳定币年度结算量对比 Visa 交易量，强调“清算网络已成型”。
    

2.  **监管从“粗暴打击”转向“分类治理”**
    

-   逻辑：不再是“全部不准”，而是“哪些资产/哪些行为/哪些主体怎么管”逐步清晰 → 给合规试点留空间。
    
-   文章举例：美国稳定币法案、欧盟 MiCA（作为“监管框架趋于明确”的信号）。
    

3.  **传统金融机构战略入场**
    

-   逻辑：如果只是创业公司搞，是边缘实验；当交易所/传统巨头把它当“核心战略”，就意味着它可能进入主流基础设施路线图。
    
-   文章举例：NASDAQ 向 SEC 申请支持 tokenized securities 的规则变更（作为“交易所级别”的信号）。
    

4.  **隐藏变量：AI Agents 正在成为“经济主体”**（作者认为这是最根本驱动）
    

-   核心判断：未来大量交易不是人类做的，而是 Agent 自动做的，且呈现**高频小额**特征。
    
-   所以需要：可编程、可审计、可结算、可自动化的金融基础设施（作者把这归结为 Web3 的“必然性”）。
    

* * *

## B. 技术可行性拆解：链上 IPO 的“四大支柱”

作者把完整链上 IPO 平台抽象成 4 层（你可以当成系统架构图背下来）：

1.  **Blockchain Layer（共识账本层）**
    

-   作用：单一可信真相源（ownership / trades）
    
-   关键词：透明、不可篡改、可追溯
    

2.  **Smart Contract Layer（业务逻辑层）**
    

-   作用：发行、分配、合规规则自动化（比如股权/分红/锁定期/KYC白名单等）
    
-   关键词：可编程金融、自动执行
    

3.  **Stablecoin Settlement Layer（稳定币结算层）**
    

-   作用：资产与货币原子结算（instant/atomic settlement）
    
-   关键词：T+0、24/7、跨境
    

4.  **Oracle Interface（现实世界接口层）**
    

-   作用：把链外信息“可信喂给链上”（KYC 状态、价格、监管黑名单等）
    
-   关键词：桥接、数据可信性
    

* * *

## C. 作者对“四大支柱”的批判：每一层都带着“代价”

这部分是文章的亮点：它不是科普，而是在讲 trade-off（取舍）。

### C1. 链上账本层：区块链不可能三角

-   观点：去中心化 / 性能 / 成本 三者不可同时最优
    
-   结论：选公链 → 透明但贵；选联盟链 → 便宜快但“去中心化价值”被削弱
    
-   记忆法：**“底层没有完美解，只有政治+工程取舍。”**
    

### C2. 智能合约层：从 “Code is Law” 到 “Tyranny of Code”

-   观点：金融世界充满例外与人为失误；合约一旦部署太“刚性”，Bug 就可能灾难性不可逆
    
-   因此：现实系统必然引入 **可升级合约 + 紧急治理机制**
    
-   反问：一旦有后门/治理，**谁来控制升级权？** → 治理复杂度反而上升
    

### C3. 稳定币层：地缘政治风险（美元稳定币=美元权力延伸）

-   观点：若依赖 USD stablecoin，相当于把链上金融“血液”交给少数发行方 + 美国监管
    
-   风险：单一主权货币依赖 → 地缘政治与监管外溢风险
    
-   这块你可以写成一句：**“结算层看似技术问题，实则货币主权问题。”**
    

### C4. Oracle 层：系统最容易被忽略的中心化点

-   观点：链上再去中心化，只要数据入口中心化，系统可信度就被入口决定
    
-   记忆法：**“最强的链，也怕一个 API。”**
    

* * *

## D. 作者的关键推论：链上 IPO 的本质其实是“让 AI Agents 合法上岗”

作者把 AI Agent 的演进写成三件事（你可以直接当成论文背景用）：

1.  **记忆问题**：LLM 天生无状态 → Agent 用向量数据库/私有记忆补齐
    
2.  **工具问题**：LLM 只会生成文本 → Agent 通过 Function Calling / MCP 调工具执行任务
    
3.  **身份问题**：LLM 不能代表法律主体签署/持有资产 → 区块链提供可验证身份与资产容器
    

然后推导出三个“传统互联网/金融的缺陷”：

-   **身份欺诈风险**：HTTP 无法证明请求背后的“主体是谁”（人/机器人/哪个公司）
    
-   **行为不可审计**：传统系统缺乏公开、不可篡改、可验证的行动日志
    
-   **交易量结构不匹配**：未来大量微支付/高频小额交易，传统系统成本结构扛不住
    

* * *

## E. 市场与商业模型：链上 IPO 为什么可能更便宜更快

文章给的对比框架你可以直接背成面试回答：

-   时间：传统 6–12 个月 → 链上 2–4 周
    
-   承销/费用：3–7% → 0.5–2%
    
-   交易：T+2 → 原子结算
    
-   覆盖：国内为主 → 全球零售可参与
    
-   流动性：交易所开盘时间 → 7x24
    

### E1. 商业模式升级：EaaS（Ecosystem as a Service）

作者说链上 IPO 平台不是“发一次就完”，而是把资产接入 DeFi 生态，持续赚多条收入：

-   发行费、上币/挂牌费、流动性/桥、数据分析、合规托管、跨链服务等
    

一句话总结：**“从一次性 IPO 服务商 → 资产全生命周期的生态编排者。”**

* * *

## F. 香港视角：为什么香港可能是试点中心

作者给了香港的结构性优势（你可以整理成申请/面试的“地区论点”）：

-   监管：SFC 有证券监管经验且更务实（可做沙盒/试点）
    
-   金融中心：人才、机构、基础设施、国际连接
    
-   桥梁位置：连接中国内地资本与海外市场
    
-   时区：夹在欧美之间，天然“7x24 覆盖”节点
    

并给出 **三阶段路线图**（你无需记细节，记“分阶段推进”就行）：

-   先试点、再扩容、再做全球数字资产中心的“港口/枢纽”
    

* * *

# 账号运营 · 学习笔记（起号 & 长期运营）

> 核心目标：  
> **不是“做内容”，而是“建立一个能被平台识别、被用户记住、被机会方信任的账号”。**

* * *

## 一、账号运营的底层逻辑（比技巧重要）

### 1️⃣ 平台视角（你在和谁博弈）

平台本质只关心三件事：

-   ❓ **你是谁**（账号标签是否清晰）
    
-   ⏱️ **你能不能留住人**（完读率 / 停留时长）
    
-   🔁 **你是否稳定输出**（是否值得长期分发）
    

👉 所以平台不是“讨厌新号”，而是**讨厌“不确定的新号”**。

* * *

### 2️⃣ 起号的本质不是爆，而是「被正确分类」

很多人起号失败不是内容差，而是：

-   一会儿 Web3
    
-   一会儿 AI
    
-   一会儿生活感悟
    
-   一会儿学习记录
    

➡️ **平台无法给你贴标签 = 不推**

> 起号期最重要的不是粉丝，而是：  
> **让算法“确信你是干嘛的”**

* * *

## 二、起号期三件必须先想清楚的事

### ① 我是谁（账号定位一句话）

不是「我想发什么」，而是：

> **“这个账号，长期解决什么人，什么阶段，什么问题？”**

🔹 错误示例：

-   Web3 学习记录
    
-   技术分享账号
    

🔹 正确示例（可被算法理解）：

-   Web3 新人避坑与入门路径
    
-   AI / Web3 交叉领域的结构化学习者
    
-   用「外行能懂的话」讲 Web3 机制的人
    

👉 你现在最适合的是：  
**「高认知密度的学习型账号」**，不是娱乐型，也不是纯教程号。

* * *

### ② 我为谁写（目标读者必须具体）

平台喜欢 **“服务单一人群”** 的账号。

比如：

-   Web3 新手（0–3 个月）
    
-   想转 Web3 的 Web2 / 理工背景学生
    
-   对区块链感兴趣但被术语劝退的人
    

⚠️ **千万不要写给“所有人”**  
那等于写给算法一个巨大的不确定性。

* * *

### ③ 我凭什么（信任来源）

新号没有粉丝，但可以有「可信理由」：

-   系统学习笔记（不是碎片观点）
    
-   连续输出的学习轨迹
    
-   真实踩坑 + 反思
    
-   把复杂内容讲清楚的能力
    

👉 **不是“我懂很多”，而是“我在认真走这条路”**

* * *

## 三、内容结构：新号最稳妥的 4 种内容类型

### 1️⃣ 学习拆解型（你现在最强）

格式模板：

> 今天我学了 X  
> → 最容易误解的是 Y  
> → 一句话讲清 Z  
> → 给新手的 1 个建议

📌 例子：

-   Solidity 中 require / revert / assert 到底怎么用
    
-   为什么 stablecoin 是链上金融的“血液”
    

* * *

### 2️⃣ 避坑总结型（平台很爱）

平台对「帮人少走弯路」极其友好。

模板：

> 我刚学 X，差点被 Y 坑  
> → 错误理解是什么  
> → 正确理解是什么  
> → 如果你是新手，请记住这一点

* * *

### 3️⃣ 结构化输出型（建立“专业感”）

比如：

-   一图讲清：链上 IPO 的四大技术支柱
    
-   三点看懂：为什么 AI Agent 需要 Web3
    

这种内容：

-   不靠情绪
    
-   不靠热点
    
-   靠 **“认知密度”**
    

* * *

### 4️⃣ 轻个人化（但不能太生活）

不是日常碎碎念，而是：

-   学到一半突然顿悟
    
-   对一个概念的“从误解到理解”
    
-   对行业的理性观察
    

👉 **你不是生活博主，是“有主线的人”**

* * *

## 四、起号期的节奏（非常重要）

### 起号前 7–14 天原则

-   ❌ 不追热点
    
-   ❌ 不乱换方向
    
-   ❌ 不一天一个领域
    

### 推荐节奏

-   1 天 1 条（或 2 天 1 条）
    
-   连续 10–20 条 **同一方向内容**
    
-   让平台形成稳定画像
    

> **宁可慢一点，也不要杂**

* * *

## 五、账号“权重”的真实来源（去魅）

❌ 不是：

-   点赞
    
-   关注
    
-   爆一条
    

✅ 而是：

-   同类型内容的 **稳定完读**
    
-   连续内容之间的 **主题一致性**
    
-   用户是否愿意点进你主页
    

一句话总结：

> **平台要的是“可预测的优质供给者”，不是“一次性爆款制造机”。**

* * *

## 六、你现在最适合的账号打法（给你定制）

结合你目前：

-   Web3 + AI + 技术学习
    
-   有系统笔记能力
    
-   不适合做情绪型内容
    

👉 **你的最优解是：**

### 「高密度学习输出 + 稳定人设 + 不急着爆」

你可以直接用这个定位句：

> **“一个认真拆 Web3 和 AI 底层逻辑的学习型账号，帮新手少踩坑。”**
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->









# Web3 实习手册[「安全与合规」](https://web3intern.xyz/zh/security/)

## 1）一句话总览：Web3 在国内的“红线”是什么？

在中国大陆环境里，监管基调可以理解为：

-   **技术本身不一定被禁止**（区块链/密码学/分布式系统）
    
-   但只要你做的事情**带上“金融属性”**（发币、融资、交易、撮合、矿池、出入金），就非常容易踩到行政甚至刑事风险
    
-   **很多风险不是“你是坏人”才会发生**，而是：你参与的环节、你接触的钱、你帮助了谁，都会被算进来
    

## 2）最核心的法律风险（你要优先记住的 5 类）

### A. 发币/融资（ICO/IEO/IDO/“积分/凭证/治理Token”换个名字也一样）

只要它具备：

-   **融资功能**（面向公众募钱）
    
-   **可流通/可交易**（能卖、能换、能变现）
    

就可能被认定为**非法金融活动**，严重时可能涉及**非法吸收公众存款**等罪名。  
⚠️ 技术岗也不一定安全：**代币模型、空投逻辑、合约部署**都可能被视为“共同行为”。

### B. 赌博/资金盘/传销（最容易“产品形态合法、结构违法”）

高危结构长这样：

-   “充值—抽奖/随机收益—提现/转卖” → 容易被认定赌博（尤其有提现闭环）
    
-   “邀请返利、多级推广、团队计酬” → 容易触发传销风险（链游/NFT/DAO/挖矿平台都常见）
    

### C. 场外交易/出入金（洗钱、帮信、非法经营外汇）

你以为你在“换点 U”，实际上可能变成：

-   **地下换汇链条的一环**（规避外汇监管）
    
-   对手资金是涉诈涉赌涉黑 → 你收款就可能导致**银行卡冻结**、被要求退赔，甚至被牵连
    

### D. 民事纠纷：合同大概率不被支持（“亏了找法院”很难）

虚拟币交易、委托代投、币币买卖等纠纷，在当前政策态度下：

-   **合同可能被认定无效**
    
-   很多损失只能“风险自担”
    

### E. 入职劳动关系风险（境外主体、远程协作、社保缺失）

Web3 项目常见：

-   境外注册 + 远程办公
    
-   国内可能没有合法用工主体 → **劳动合同/社保公积金**难保障
    
-   即便有合同，也可能因主体资格问题变“白纸黑字”  
    解决思路之一：部分公司会用 **EOR（名义雇主）**模式，但仍需仔细审查。
    

## 3）全球监管趋势：对你意味着什么？

文中提到的大方向你可以这样理解：

-   **合规会越来越像传统金融**：KYC、AML、客户尽调、交易监控都会变成常规配置
    
-   国际上在推进统一标准（例如 FATF），其中最关键的是 **Travel Rule（旅行规则）**：转账要收集并传输发送方/接收方信息（尤其大额），这对 DeFi/DEX 是挑战
    
-   稳定币会被重点监管：因为它已经是 DeFi 的“基础设施”，一出事会系统性冲击（文中举了 Terra UST 崩盘作为典型）
    

对从业者的实际影响：

-   门槛提高、成本增加，但长期看会让行业更“可持续”
    
-   合规好的项目更容易和机构合作、更容易活得久
    

## 4）“学生/实习生最实用”的避雷清单（直接照做）

### 面试/实习（最常见被黑入口）

-   **只用官方会议工具**（Zoom/Teams/腾讯会议等），拒绝安装“内部面试软件”“专用视频软件”
    
-   对方让你下载 zip/rar“面试助手”“资料包” → 直接当高危
    
-   必要时：先用**虚拟机**测试、核对文件哈希/签名（你不懂也没关系，记住“不要在主电脑装”）
    

### 空投/奖学金/福利（骗新人最快）

-   任何要求你**连接钱包、签名验证、输入助记词/私钥**的网页：**99% 是骗局**
    
-   空投只用测试钱包，主钱包尽量冷存储
    

### 群聊/私聊（社工）

-   “管理员/学长/HR”私聊发链接要你操作 → **多渠道核实**
    
-   “紧急转账/帮忙测试/填表连接钱包” → 默认拒绝
    

### 软件/插件

-   只从官网/官方商店下载
    
-   浏览器插件控制在极少数（钱包、密码管理器、广告屏蔽足够）
    
-   转账前核对地址**前几位 + 后几位**，防剪贴板劫持
    

### 账号安全（最容易被忽视、后果最大）

-   重要账号开 2FA（尽量不用短信，优先验证器/硬件密钥）
    
-   每个平台独立强密码 + 密码管理器（1Password/Bitwarden）
    
-   **邮箱和手机号的安全优先级最高**（一旦被拿下，几乎全盘失守）
    

### 钱包安全

-   助记词/私钥：**只能离线保存**（不截图、不网盘、不发人）
    
-   定期检查授权并撤销不必要授权（文中提到 [Revoke.cash](http://Revoke.cash)、Etherscan 等）
    

## 5）典型骗局的“识别信号”

你看到这些关键词，就把它当高风险：

-   “提现/出金/稳赚/保本/返利/邀请奖励/团队收益”
    
-   “安装我们内部软件/下载面试助手/打开文档填写宏”
    
-   “连接钱包签名验证身份”
    
-   “USDT 工资/Token 工资为主、法币很少或没有”
    
-   “对中国大陆用户不明确、注册地不透明、主体不清晰”
    

## 6）如果你怀疑自己中招了：第一时间做什么？

-   **立刻断网**
    
-   截图/拍照/记录时间点和你做过的操作
    
-   尽快联系校内信息中心或懂安全的人求助
    
-   能换的密码尽快换（优先邮箱/交易所/社交账号）
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->










# Co-learning

## 运营

參與項目的過程，所做的事情都可以進行分析描述，當作案例分析這樣寫進履歷

发推建议用英文，數據會好一點

推薦surf ai

沙丘dune

# Web3 安全主题分享会

資源：余弦老师的web3的黑暗森林

分享人：Adam @GoPlusSecurity

## Web3 安全与风险趋势：谁在偷走你的加密资产？

1.  不容乐观的Web3 2025 安全数据
    
2.  典型案例与攻击手法解密
    
3.  AI 时代下的安全新挑战
    
4.  GoPlus 安全建议
    

## 2025年安全态势总览：损失超35亿美元

-   安全事件数量 > 1200起
    
-   总损失金额 >35亿美元
    
-   攻击趋势 精准猎杀＋广撒网
    

2025年Web3领域共发生了超过1200起较严重的安全事件，总损失金额超过35亿美元。

值得注意的是，攻击趋势呈现出“精准猎杀”和“广撒网”并行的特点。

2025年We3领域安全事件频发，攻击手段呈现多样化和复杂化趋势，对行业构成严重威胁。

## 精准猎杀：2025损失金额Top事件

2月21日，Bybit 還政击，损失约15亿美元，原因系 Safe 多笠服务器湿滲透纂改交易指令。后续处理：与 FBI 等执法机构合作，冻结超 4000 万美元被盗资产，平台已通过补充储备金，确保用户资产未受损失。

-   5月22日，Cetus Protocol道攻击，在 #Sui 上损失约2.23亿美元，原因系Move智能合约漏调，$CETUS 价格跌超16%。后续处理：Sui 链迅速协同并冻结了 1.62亿美元被盗资产，社区投票通过了 100% 赔付方案，并获得 Sui基金会3000万美元紧急信贷支持。
    
-   11月03日，Balancer 遭攻击，多链损失约1.28亿美元，系 Balancer V2 智能合约漏洞。后续处理：Berachain 协调验证者执行紧急硬分叉并冻结攻击者地址；白帽操作员已归还1280万美元资金，StakeWise 利用合约管理机制追回约 2000万美元资产。
    
-   6月18日，伊朗交易所 Nobitex 退攻击，多链损失约9000万美元。后续处理：该事件具有地缘政治色彩，攻击者将资金转入无法使用的羞辱性地址，旨在造成瘫痪而非直接获利。
    
-   4月1日，UPCX 遭攻击，损失约7000万美元，原因系管理员私钥泄露导致ProxyAdmin合约遷非法升级。后续处理：转移剩余代币并联合执法部门追踪被盗资产。
    
-   1月23日，Phemex遭攻击，多链损失约6910万美元，原因系热钱包被盗。后续处理：发布储备证明 （PoR）证实冷钱包安全；平台通过自有资金对受损用户进行分阶段全额赔付。
    
-   8月14日，土耳其交易所 BtcTurk遭攻击，损失约5400万美元，原因系热钱包被盗，这是该交易所热钱包在14个月内第二次被盗。后续处理：启动 240万美元赏金计划寻求线索。
    
-   12月20日，高净值投资者地址 （0xcB8078）遺"地址投毒 （Address Poisoning）”钓鱼攻击，损失 4999万美元 USDT、后续处理：受害者发出链上消息，提议支付 100 万美元作为白帽赏金以换取98% 资金退回，但目前该攻击者已将资金兑换为 ETH 并通过 Tornado Cash 混币。
    
-   2月24日，Infini遷攻击，损失约4950万英元，原因系管理员权限管理漏洞。后绥处理：创始人公开致歉并设立专项赔偿基金，承诺在 2026年前完成对所有受影响用户的100%賠付。
    
-   7月20日，印度交易所 CoinDCX 遭攻击，损失约4420万奠元，原因系热钱包被盗。后续处理：指定用户补偿计划并追踪被盗资产。
    
-   7月9日，GMX還攻击，损失约4050 万美元，原因系智能合约漏泅。后续处理：攻击者在48小时内接受了项目方提出的10%（约500万美元）白帽赏金协议，并归还了约4000万美元的被盗资产。
    
-   12月2日，韩国交易所Upbit 遭攻击，在 #Solana 上损失约3000万美元，原因系朝鲜黑客组织渗透攻击。后续处理：与全球交易平台协作对黑客关联地址进行实时黑名单标记及资金追踪拦馥。
    

CeFi成为主要的黑客取款机：2025年单体损失金额超3000万美元的攻击事件12起，其中CeFi占了7起，管理员私钥被盗、热钱包私钥被盗是最主要的原因，暴露了显著的风险。

DeFi漏洞越来越深入：DeFi领域的安全性在2025年表现出相比往年更好的抗压性，合约漏洞导致的损失却远低于TVL的增长率。但值得注意的是 Balancer、Yearn 这类已安全运营数年的智能合约不断被挖掘出新的漏洞，以及Move智能合约漏洞明显增多，体现出攻击者利用 AI技术挖掘智能合约漏洞的能力正在不断增强。

国家级黑客的“业绩”巅峰：2025年，朝鲜黑客组织（如Lazarus Group）盗取的资金继续高速增长，在2025年至少盗取了20.2亿美元的加密资产，较2024年增长了51%，创下朝鲜通过网络攻击获取资产的历史新高。朝鲜黑客的攻击模式在2025年完成了深度进化：

-   社会工程：通过伪装成web3或AI公司的招聘人员，在LinkedIn等平台进行数月的关系维护，最终通过“技术面试“环节诱导目标员工下载带有后门的测试代码。
    
-   精准猎杀：他们将精力集中在少数拥有巨额资金储备的机构或个人身上。在2025年的所有机构级侵害事件中，朝鲜黑客贡献了超76%的金额。
    
-   洗钱工业化：资金在被盗后迅速进入一个高度自动化的洗钱链条，通常可以在45天内完成从链上混币到非法平台法币结算的全过程。
    

## 广撒网：高频攻击与欺诈类型

### 私钥窃取 Private Key Theft

-   发生次数：>300起
    
-   损失金额：＞18亿美元
    
-   针对性狩猎，基于病毒木马、社工和供应链渗透。
    

### 钓鱼攻击 Phishing Attacks

-   发生次数：＞400起
    
-   损失金额：>7.5亿美元
    
-   AI生成钓鱼，全平台化，签名陷阱。
    

### Rug Token Token Scam

-   发生次数：约300起损失金额：6亿美元
    
-   碎片化撤池，隐藏后门，职业化与团伙化。
    

与此同时，“广撒网”式的攻击也在持续泛滥。私钥窃取、钓鱼攻击 和 Rug Token 是最高发的三种类型。

特别是随着 AI技术的普及，钓鱼攻击变得更加难以辨别，而 Rug Token 也呈现出职业化和团伙化的趋势。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/kmiliu/images/2026-01-14-1768391330435-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/kmiliu/images/2026-01-14-1768392491102-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/kmiliu/images/2026-01-14-1768392931680-image.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->













# [Ethereum](ethereum.org)

## 1\. What is Ethereum?

**Ethereum** is a **decentralized blockchain network and software development platform**, powered by its native cryptocurrency **ether (ETH)**.

Unlike Bitcoin, which focuses on peer-to-peer digital cash, Ethereum supports **smart contracts** — open-source programs that run automatically on the blockchain.

**Key idea**: **Ethereum = programmable, global, permissionless infrastructure**

## 2\. Smart Contracts

**Smart contracts** are:

-   Open-source programs
    
-   Deployed on Ethereum
    
-   Run **24/7**, globally
    
-   Cannot be altered once deployed
    
-   Do not rely on banks, companies, or governments
    

They allow anyone to build **decentralized applications (dapps)** that are:

-   Trustless
    
-   Censorship resistant
    
-   Automatically enforced by code
    

## 3\. The Ethereum Network

The **Ethereum network** is made up of thousands of independent computers called **nodes**.

Each node:

-   Stores the entire state of Ethereum
    
-   Verifies transactions and smart contracts
    
-   Operates independently around the world
    

### Key advantages of the Ethereum network:

1\. Censorship resistance

No bank, company, or government can block transactions or freeze accounts.

2\. High security

All nodes store and verify the same data. Any invalid change is rejected by the network.

3\. Durability and reliability

Ethereum has no single point of failure and maintains continuous uptime even during disruptions.

## 4\. Ether (ETH)

**Ether (ETH)** is the native cryptocurrency of Ethereum.

ETH is used to:

1.  **Pay gas fees** (transaction fees)
    
2.  **Reward validators**
    
3.  **Secure the network through staking**
    

### ETH supply mechanics:

-   New ETH is issued as validator rewards
    
-   A portion of ETH is **burned** with every transaction
    
-   On high-usage days, more ETH is burned than created
    
-   This can make ETH **deflationary**
    

## 5\. Gas Fees

Every transaction on Ethereum requires a **gas fee**, paid in ETH.

Gas fees:

-   Prevent spam
    
-   Compensate validators
    
-   Reflect network demand
    

High fees indicate strong usage and security demand.

## 6\. Proof of Stake (PoS)

Ethereum originally used **Proof of Work**, but transitioned to **Proof of Stake** in 2022.

In Proof of Stake:

-   **Validators** lock up ETH as a security deposit (staking)
    
-   Validators propose and attest to blocks
    
-   Honest behavior earns rewards
    
-   Dishonest behavior results in penalties (slashing)
    

This upgrade reduced Ethereum’s energy consumption by **over 99%**.

## 7\. How Transactions Work (Simplified Flow)

Example: Sending stablecoins on Ethereum

1.  A wallet signs and broadcasts a transaction
    
2.  The transaction enters the **mempool**
    
3.  A validator selects the transaction
    
4.  The transaction is included in a block
    
5.  Smart contracts update balances
    
6.  Validators attest to the block’s validity
    

All state changes are synchronized across all nodes.

## 8\. Layer 2 (L2) Networks

**Layer 2 (L2) networks** run on top of Ethereum to improve scalability.

They:

-   Reduce transaction costs
    
-   Increase transaction speed
    
-   Inherit Ethereum’s security
    

Examples:

-   Optimism
    
-   Arbitrum
    
-   zkSync
    
-   Base
    

L2s process millions of transactions while settling on Ethereum.

## 9\. What is Ethereum Used For?

Ethereum enables use cases that were previously impossible:

### Consumers

-   Access financial services without banks
    
-   Own digital assets (NFTs)
    
-   Sign in using wallets instead of passwords
    
-   Participate in decentralized communities
    

### Developers & Businesses

-   Deploy tamper-proof smart contracts
    
-   Launch global applications instantly
    
-   Build composable financial products
    

### Governments & NGOs

-   Transparent aid distribution
    
-   Digital identity systems
    
-   Public infrastructure for registries and voting
    

## 10\. Ethereum vs Bitcoin

| Feature | Bitcoin | Ethereum |
| --- | --- | --- |
| Purpose | Digital money | Programmable infrastructure |
| Smart contracts | Limited | Fully programmable |
| Consensus | Proof of Work | Proof of Stake |
| Energy use | High | Low |
| Supply | Fixed (21M BTC) | Dynamic (issuance + burn) |

**Bitcoin** focuses on value storage.  
**Ethereum** focuses on building applications.

## 11\. Governance & Development

Ethereum has **no central authority**.

The network is maintained by:

-   Developers
    
-   Node operators
    
-   Validators
    
-   Community members
    

Changes are proposed through **Ethereum Improvement Proposals (EIPs)** and adopted only through community consensus.

## 12\. Ethereum Roadmap (2025)

Ethereum follows a **shared vision**, not a fixed roadmap.

Recent upgrade:

-   **Pectra (May 2025)**
    
    -   Improved smart wallet functionality
        
    -   Better Layer 2 compatibility
        
    -   Increased staking flexibility
        

Ongoing priorities:

-   Scalability
    
-   Usability
    
-   Security
    
-   Decentralization
    

# [**Web3**](https://ethereum.org/web3/)

## 1\. What is Web3?

**Web3** is a vision for a new version of the internet that embraces **decentralization**, where the web is **built, operated, and owned by its users**, rather than controlled by centralized corporations.

Core idea: **Web3 puts power in the hands of individuals instead of large technology companies.**

Web3 uses:

-   **Blockchains**
    
-   **Cryptocurrencies**
    
-   **NFTs**
    

to enable **ownership**, **permissionless participation**, and **trustless systems**.

## 2\. The Evolution of the Web

### Web 1.0: Read-Only (1990–2004)

-   Coincides with the early World Wide Web
    
-   Developed by **Tim Berners-Lee** at CERN
    
-   Focused on **static websites**
    
-   Users consumed information but rarely produced content
    
-   Little to no user interaction
    

Architecture:

-   **Client-server architecture**
    
-   Websites owned by companies
    
-   Often called the **read-only web**
    

### Web 2.0: Read-Write (2004–now)

-   Emerged with social media platforms
    
-   Users could **create and share content**
    
-   Enabled user-to-user interaction
    
-   Introduced **advertising-driven revenue models**
    

Key characteristics:

-   Platforms host user-generated content
    
-   A small number of companies control:
    
    -   User data
        
    -   Monetization
        
    -   Platform rules
        

Main issue: Users create value but **do not own their data or content**.

### Web 3.0: Read-Write-Own

-   Term coined by **Gavin Wood** in 2014
    
-   A response to excessive trust in centralized platforms
    

Architecture:

-   **Decentralized node architecture**
    

Key shift:

-   Web1 = Read
    
-   Web2 = Read-Write
    
-   **Web3 = Read-Write-Own**
    

## 3\. Core Ideas of Web3

### 1\. Web3 is decentralized

-   Ownership and control are distributed among builders and users
    
-   No single centralized authority
    

### 2\. Web3 is permissionless

-   Anyone can participate
    
-   No gatekeepers or exclusions
    

### 3\. Web3 has native payments

-   Uses **cryptocurrencies** instead of banks or payment processors
    
-   Enables direct peer-to-peer transactions
    

### 4\. Web3 is trustless

-   Relies on **incentives and economic mechanisms**
    
-   Removes the need for trusted third parties
    

## 4\. Why is Web3 Important?

### Ownership

Web3 enables **direct ownership** of digital assets.

Example:

-   In Web2 games, in-game items are tied to an account
    
-   If the platform bans or shuts down the account, assets are lost
    

In Web3:

-   Ownership is enforced via **non-fungible tokens (NFTs)**
    
-   Ownership cannot be revoked by platform creators
    
-   Assets can be sold or traded on open markets
    

### Censorship Resistance

In Web2:

-   Platforms can change rules, ban creators, or remove content
    
-   Creators lose reputation and audience if they leave a platform
    

In Web3:

-   Data and reputation live **on the blockchain**
    
-   Users can move their identity and reputation between platforms
    
-   **Censorship resistance is a native feature**
    

### Decentralized Autonomous Organizations (DAOs)

**DAOs** enable collective ownership and governance of platforms.

Technical definition:

-   DAOs are **smart contracts**
    
-   They automate decentralized decision-making over shared resources (tokens)
    

Key features:

-   Tokens act like shares
    
-   Token holders vote on proposals
    
-   Voting outcomes are executed automatically by code
    

Note:

-   Many Web3 communities call themselves DAOs
    
-   Levels of decentralization and automation vary
    

### Identity

Problems in Web2:

-   Separate accounts for each platform
    
-   Reliance on social logins
    
-   Platforms control access and personal data
    

Web3 solution:

-   **Ethereum address** as a universal login
    
-   **Ethereum Name Service (ENS)** for human-readable identities
    

Benefits:

-   Single login across platforms
    
-   Censorship-resistant
    
-   Anonymous
    
-   User-controlled
    

### Native Payments

Web2 payments:

-   Depend on banks and payment processors
    
-   Exclude people without bank access
    

Web3 payments:

-   Use tokens like **ETH**
    
-   Enable direct, browser-native payments
    
-   Require no trusted third party
    

## 5\. Web3 Limitations

### Accessibility

-   Transaction fees can be expensive
    
-   Limits adoption in developing regions
    
-   Addressed through:
    
    -   Ethereum roadmap
        
    -   **Layer 2 scaling solutions**
        

### User Experience

-   High technical barrier to entry
    
-   Users must understand:
    
    -   Wallet security
        
    -   Private keys
        
    -   Complex interfaces
        
-   Wallet providers are improving UX, but challenges remain
    

### Education

-   Web3 requires new **mental models**
    
-   Similar to early Web1 education efforts
    
-   Broad educational initiatives are necessary for mass adoption
    

### Centralized Infrastructure

-   Web3 still depends on centralized tools:
    
    -   GitHub
        
    -   Twitter
        
    -   Discord
        
-   Decentralized alternatives are emerging but immature
    

## 6\. A Decentralized Future

-   Web3 is still young and evolving
    
-   Recent progress includes:
    
    -   Increased interest in cryptocurrency
        
    -   Improvements in Layer 2 scaling
        
    -   Experiments in governance
        
    -   Advances in digital identity
        

Key idea: Web3 is not complete — it is actively being built.

## 7\. How Can I Get Involved?

-   Get a wallet
    
-   Find a community
    
-   Explore Web3 applications
    
-   Join a DAO
    
-   Build on Web3
    

# Web3 运行原理

## 钱包私钥和个人主权

-   私钥Private Key: 终极签名印章，不可重制，区块链只认“签名”
    
-   助记词Seed Phrase: 私钥的备份
    
-   地址： 公钥截取后的字符串
    

助记词一对多个私钥，助记词通过cryptology到private key，private key也通过运算生成公钥，但是公钥无法算出私钥

### 私钥的安全性建议

**私钥很安全——除非你自己泄漏了**

-   ﻿﻿私钥是不可重置的密码（丢了就是永远丢），只能换钱包
    
-   ﻿﻿区块链不认识你是谁，只认识“谁能签名"
    
-   ﻿﻿大部分资产被盗，不是链被攻破，而是私钥/助记词泄露或授权被钓鱼
    

**安全原则（强制记住）**

-   ﻿﻿助记词/私钥：不截图、不网盘、不发人、不复制到不可信环境
    
-   ﻿﻿剪贴板是高风险区（很多恶意软件会读剪贴板）
    
-   ﻿﻿任何人一旦拿到私钥/助记词= 拿到资产控制权
    

### 私钥的社会学意义：个人主权的起点、

任何人都可以随机创建私钥一立刻拥有一个无需许可的账户与资产权利

• 对比传统金融：开银行账户需要身份、审批、地域、制度

**背后原因：**

-   ﻿﻿这是密码学提供的“可验证的个人主权”
    
-   ﻿﻿你不需要别人“给你账号”，你自己就能生成“权利入口”
    

## 交易和签名

### 交易是什么

**交易（Transaction or tx）= 你要做的事＋手续费＋防重放序号**

-   ﻿﻿你要做的事：比如转账、调用合约 mint、投票等
    
-   ﻿﻿手续费（Gas Fee）：为网络资源付费（并提供激励）
    
-   ﻿﻿防重放序号（Nonce）：避免同一笔交易被重复执行
    

**钱包在做什么：**

-   ﻿﻿组装交易内容
    
-   ﻿﻿用私钥签名（生成数字签名，证明是你授权）
    
-   ﻿﻿广播到区块链网络
    

### Gas Fee 是什么？

**Gas Fee 的三层作用：**

-   ﻿「防垃圾交易淹没网络（提高作恶成本）
    
-   ﻿﻿激励打包者/验证者提供算力与带宽
    
-   ﻿﻿形成经济闭环：资源使用者付费 一网络可持续运转
    

**经济学视角：**

-   ﻿﻿Gas 是去中心化网络的激励机制之一
    
-   ﻿﻿没有激励，网络很难长期稳定运行
    

测试网: [https://sepolia.etherscan.io/](https://sepolia.etherscan.io/)

主网: [https://etherscan.io/](https://etherscan.io/)

## 区块链网络运行

### 从一笔交易到出块

交易的完整生命周期：[https://txcity.io/v/eth-btc](https://txcity.io/v/eth-btc)

-   Wallet（签名）
    
-   RRC/Node（传播）
    
-   Mempool（排队）
    
-   Builder/Validator (#kit) [https://beaconcha.in/block/24224597#votes](https://beaconcha.in/block/24224597#votes)
    
-   ﻿﻿Block (IA) [https://beaconcha.in/block/24224597](https://beaconcha.in/block/24224597)
    
-   ﻿﻿Explorer (PJ#) [https://etherscan.io/](https://etherscan.io/)
    

### 为什么“不可篡改需要时间”

**为什么要等确认？**

-   ﻿﻿区块像账本新一页，会引用上一页指纹（hash）
    
-   ﻿﻿区块越往后叠，历史被推翻的成本越高
    
-   ﻿﻿PoS 下存在“最终确认 （finality）”：通常需要约12-13分钟达到更稳定的最终状态
    

**实操：查看 Block 信息**

[https://etherscan.io/block/0xb36a9426b3646ddf3d64213-](https://etherscan.io/block/0xb36a9426b3646ddf3d64213-)

bcc65f854db7b7ba1c9995ffc62

### 共识机制：PoW vs PoS

**挑战：匿名且互不信任的全球节点，如何认可同一本账？**

**PoW（工作量证明）BTC**

-   ﻿﻿算力竞争写账（耗能）
    
-   ﻿﻿像"解一道很难但答案很好验的题”：谁先解出谁记账，全网快速验算
    

**PoS（权益证明）ETH**

-   ﻿﻿质押＋随机选人写账（更节能）
    
-   ﻿﻿像“押保证金参加抽签当记账员”：押得多/信誉好概率更高；作恶会被罚没押金
    

### 钱包、RPC、节点、网络：如何链接

**关键关系：**

-   ﻿﻿钱包通常不直连全网：它连一个 RPC（访问节点的 API）
    
-   ﻿﻿RPC 背后是节点（或节点集群）  
    所以：RPC 往往是“中心化入口风险点”
    

**实操：看钱包的 RPC设置**

-   ﻿﻿打开钱包网络设置查看 RPC URL/ Chain ID
    
-   ﻿﻿用 Chainlist 查看/添加网络：[https://chainlist.org/](https://chainlist.org/)
    

## 智能合约

### 智能合约的本质

**智能合约 = 区块链账本里的“可执行代码”。**

-   ﻿﻿在EVM（虚拟机）里运行
    
-   ﻿﻿交易触发执行一改变链上状态
    
-   ﻿﻿写进区块链：具备"难以篡改、可追溯”的特性
    

**实操：查看** [**https://nft.myfirst.io/**](https://nft.myfirst.io/) **的合约**

### 智能合约的社会学意义

**Code is law:**

-   ﻿﻿规则可验证、按代码执行：任何人都能检查规则与执行历史
    
-   ﻿﻿部署后难以更改：减少“临时改规则”的空间（除非使用升级机制）
    
-   ﻿﻿减少中介与摩擦：把“信任人/机构”压缩为“信任代码＋共识＋审计/治理”
    

**例子（交换的最小模型）：**

-   ﻿﻿A把 NFT 存入合约->B转账进合约->合约自动完成交换（按写死的规则执行）
    
-   ﻿﻿传统世界往往要信任中介、律师、法院与流程；智能合约试图把一部分信任成本变成可验证的程序规则。
    

**实操: Mint** [**https://nft.myfirst.io/**](https://nft.myfirst.io/) **的 NFT**

## 区块链协议升级

### 以太坊如何“改规则”

EIP 的基本路径：

-   ﻿﻿先讨论（Magicians 论坛）一再形成文档（EIPs）一再进入升级（每年1-2次）
    
-   ﻿﻿每次升级都是一次硬分叉 [https://eips.ethereum.org/EIPS/eip-7607](https://eips.ethereum.org/EIPS/eip-7607)
    
-   ﻿﻿关键点：不是某公司拍板，是社区协作推进
    

相关入口：

-   ﻿EIP FU#: [https://eips.ethereum.org/](https://eips.ethereum.org/)
    
-   ﻿﻿EIP ittz: [https://ethereum-magicians.org/](https://ethereum-magicians.org/)
    

### 节点客户端多样性

节点软件不止一种：执行层客户端＋共识层客户端

-   ﻿﻿多样性越高：越不怕单一软件 bug / 分叉风险
    
-   ﻿﻿去中心化不仅是“节点多”，还包括“实现多样”
    
-   ﻿﻿以太坊自上线期，从未有过宕机
    

实操：看主网节点/验证者分布数据

-   ﻿﻿节点软件占比：[https://ethernodes.org/](https://ethernodes.org/)
    
-   ﻿﻿Rated Network Explorer 查看 Validators（验证者）
    
-   Etherscan Node Tracker（节点服务器）[https://etherscan.io/nodetracker](https://etherscan.io/nodetracker)
    

## Web3 关键特性回顾

### Web3 关键特性

去中心化

-   ﻿﻿钱包创建：高度去中心化（人人可生成钥匙）
    
-   ﻿﻿交易广播：可能中心化 RPC入口、前端入口）
    
-   ﻿﻿网络运行：节点越分散越安全
    
-   ﻿﻿客户端越多样越稳（减少单点软件风险）
    

其他特性

-   ﻿﻿无许可：任何人都可以读/写网络（写入需要 gas）
    
-   ﻿﻿抗审查：节点全球分布、入口可替换（钱包/前端/RPC都可换）
    
-   开放开源：客户端开源、交易记录公开可查询（透明可审计）
    
-   ﻿隐私（现状与演进）
    
-   ﻿﻿现状：公开账本＋伪匿名（地址不等于身份）
    
-   ﻿风险：可通过关联分析追踪行为图谱
    
-   ﻿﻿演进：隐私方案仍在发展（例如ZK、隐私链/隐私层等
    

### 总结

**Web3 是跨学科的领域**

-   社会学：法中心化治理与共识形成
    
-   经济学：激励设计保证安全与可持续（Gas、质押、惩罚）
    
-   密码学：签名、哈希、ZK 提供可信基础
    

**技术层面一句话：**

-   Web3就是：用私钥签名证明你是谁，用共识网络保证账本可信，用智能合约让规则自动执行。
    

**技术之外：**

-   权力的重新分配：数字资产与权利自我控制、无许可、抗审查、公开可信、去中间商
    
-   这些是构建未来数字世界的重要基础（也是LXDAO人ETHPanda 关注的方向）
    

### 留给大家的思考（新的挑战）

-   ﻿﻿资产自托管：如何提高安全性、降低管理私钥的复杂度？
    
-   ﻿﻿没有中心化机构/税收时，公共基础设施谁来维护？  
    如果有税收，又如何分配？
    
-   ﻿﻿没有审查且隐私很强时，如何界定并治理有害信息/黑产？
    
-   ﻿﻿去中心化协作下，如何实现公平、可信的分配？
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->
















# Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分

## 核心

### Web3 到底在解决什么？

在没有一个中心老板/平台的情况下，大家怎么共同记账、共同执行规则、共同拥有资产？

### Web3 的三个重点

1.  **账本**：区块链（把发生过的事记下来）
    
2.  **规则**：智能合约（把“如果…就…”写成自动执行的代码）
    
3.  **身份+资产控制权**：钱包（私钥=你对资产的终极控制权）
    

## 区块链

### 区块链是什么？

一群互不信任的人，用同一本公开账本记录交易，而且谁都很难改历史。

### 为什么“很难改历史”？

想改历史 = 需要控制共识的多数（PoW 是多数算力 / PoS 是多数质押）并持续产出更长的链。

### 为什么愿意辛苦记账？

因为**记账有钱拿**：区块奖励 + 手续费  
手续费在以太坊里就是 **Gas**（付钱给网络让别人帮你跑代码/确认交易）

## 公链/私链/联盟链

### **公链（公园）**

任何人能来当维护者；数据基本公开；规则靠共识

-   优点：最开放、最抗审查
    
-   缺点：慢、贵、协调难
    

### **联盟链（董事会）**

只有被允许的组织能当节点；数据半公开；规则要多方同意

-   优点：更高效、更隐私
    
-   缺点：仍然是“少数组织共同控制”
    

### **私链（公司内网）**

老板说了算；数据内部可见；规则中心化

-   优点：极快
    
-   缺点：不“去中心化”，更像传统数据库
    

### 总结

去中心化程度：公链 > 联盟链 > 私链  
效率/控制：私链 > 联盟链 > 公链

## Web2 / Web3.0 / Web3

### **Web2**

发内容，但**平台拥有数据和规则**（租房）

### **Web3.0（语义网）**

让机器更懂数据关系（知识图谱那套），**跟区块链不是一回事**

### **Web3**

用私钥控制身份与资产，规则由合约执行（自己买地盖房）

## 以太坊

### 以太坊是什么？

**比特币主要是“钱的系统”，以太坊是“可以跑程序的系统”。**  
所以以太坊更像“全球共享计算机”。

### 三个核心零件

1.  **账户（Account）**
    

-   **EOA（你的钱包）**：你能主动发交易（靠私钥签名）
    
-   **CA（合约账户）**：合约本身，不能主动动，只能被你触发
    

点一次“Swap” 本质是：你的 EOA 发交易 → 调用某个 CA → 合约代码执行

2.  **Gas（计算费）**
    

你在以太坊上做任何事都要付 Gas，因为：

-   全网节点都要执行/验证你的操作
    
-   Gas 是“防滥用+激励执行”的机制
    

公式：**总费用 ≈ 用了多少 Gas × 每单位 Gas 多贵**

EIP-1559 后：

-   **Base fee**（基础费）会被销毁（通缩叙事）
    
-   **Tip**（小费）给打包你交易的人（让他更愿意优先处理）
    

3.  **EVM（执行引擎）**
    

智能合约的代码在 EVM 里跑。  
重点是：**每个节点都要跑一遍同样的代码，才能保证结果一致**

### The Merge

以前以太坊像比特币一样靠挖矿（PoW），后来改成质押（PoS）。  
**The Merge（2022/09/15）= 把以太坊从 PoW 换成 PoS。**  
你记住它带来的三个影响就够了：

-   能耗大幅下降
    
-   安全靠“质押”+惩罚（作恶会被 slash）
    
-   为扩容路线（L2）做铺垫
    

### 以太坊为什么还“贵”？所以 L2 为什么重要？

以太坊 L1 贵是因为它要保持全球同步安全，吞吐有限。  
解决策略是：

-   L1 做“最终安全/结算层”
    
-   **L2（Rollup）做大部分交易，链下批量处理，链上做最终盖章。**
    
-   L2 把结果批量提交回 L1
    

## 行业赛道

### DeFi

核心：把“交易、借贷、稳定币”这些金融功能，用合约自动化。

**Uniswap（DEX）**

-   池子里两种币的比例变化 ⇒ 价格自动变化
    
-   你换得越多 ⇒ 对池子冲击越大 ⇒ **滑点越大**
    

**Compound（借贷）**

-   借钱必须抵押更多（超额抵押）
    
-   价格跌 ⇒ 抵押不足 ⇒ 触发清算  
    **风险核心：波动 + 清算**
    

**MakerDAO/Sky（稳定币）**

-   抵押资产生成稳定币
    
-   抵押不足会被清算  
    **风险核心：抵押物价格崩 + 系统参数**
    

### NFT

NFT 的重点不是 JPEG，而是：

-   **链上可验证的“唯一所有权凭证”**
    
-   合约可以写版税、转售规则等
    

### DAO

DAO = 一套把组织治理写在链上的机制  
强项：透明、全球协作  
弱点：治理被大户绑架、信息太公开导致策略劣势（ConstitutionDAO 那类）

### MEME

**MEME 的价值主要来自情绪和叙事，不来自基本面。**  
要看的是：筹码集中度、叙事、流动性、退出路径。  
最该做的是：**控制仓位，别幻想“自己能逃顶”。**

## Web3 工作方式

### Web3 的现实：远程 + 异步

不是“酷”，是“必须”。  
所以你要把自己变成“异步协作可靠的人”。

### “靠谱”的 3 个动作

1.  **接任务时三件套确认**  
    交付物是什么 / 截止时间（含时区）/ 成功标准是什么
    
2.  **所有决策留痕**  
    会议后发总结：结论 + Action items + DRI + Deadline
    
3.  **不给模糊话**  
    不说“尽快”，改成“xx 前完成 + 需要你给我 yy”
    

### OKR

-   O：要达成什么结果（不要写动作），提升合约安全性
    
-   KR：用数字描述“成功长啥样”，审计问题 ≤ X；上 bounty；形式化验证完成
    

# 021 学习以太坊[第 1 章](https://lxdao.notion.site/What-is-Ethereum-2a8dceffe40b8028932acee0f9dae9ee)

## 核心

1.  \*\*以太坊是什么？\*\*一台“全球共享的可编程计算机”（区块链 + EVM + 智能合约）。
    
2.  \*\*ETH 是什么？\*\*以太坊系统的“原生资产”，既是 **Gas 燃料**，也是 **PoS 质押保证金**，还是 DeFi/NFT 的核心抵押与计价资产。
    
3.  \*\*以太坊和比特币最大差别？\*\*比特币更像“硬钱/数字黄金”，以太坊更像“可编程应用平台”。
    

## 历史线

### 为什么会有以太坊？

-   Vitalik 在比特币社区里发现：BTC 的脚本太受限，**不适合写复杂应用**
    
-   于是提出：做一个更通用的去中心化应用平台（DApp 平台）
    

### 关键里程碑

-   **2013**：白皮书提出以太坊设想
    
-   **2014**：项目启动 + 众筹（用 BTC 购买 ETH）
    
-   **2015-07-30**：以太坊主网（Frontier）上线
    
-   **2016**：Homestead（更稳定）
    
-   **Metropolis**：多次升级（Byzantium / Constantinople 等）
    
-   **2022-09-15**：The Merge：PoW → PoS（共识大转向）
    
-   **2024-03-13**：Dencun（EIP-4844 / blob，L2 成本大降）
    
-   **Pectra（你文里写 2025）**：涉及验证者有效余额上限等（**这类升级年份/细节建议你后续再核对一次**）
    

以太坊从“PoW 记账链”进化成“PoS + Rollup 为中心的结算/数据层”。

## 以太坊是什么平台？

### 定义

以太坊 = 去中心化、开源的公共区块链平台，它的特别之处是支持智能合约（可编程）。

### 3 个核心组件

1.  **EVM**：以太坊虚拟机——“所有节点都按同样规则执行代码”的执行环境
    
2.  **智能合约**：部署在链上的程序——“if…then…”自动执行
    
3.  **Gas / ETH**：运行和写入链状态的成本计量与支付体系（Gas 用 ETH 支付）
    

**链 = 账本；EVM = CPU；合约 = 程序；Gas = 电费；ETH = 电费货币 + 保证金。**

## 以太坊的核心特性

### 智能合约（Smart Contract）

-   本质：链上程序，节点执行，结果写入全网共享状态
    
-   类比：自动贩卖机（投币=付 gas，按键=调用函数，出货=状态改变）
    

### dApp（去中心化应用）

-   结构：**链上合约（后端逻辑）+ 链下前端（UI）**
    
-   关键特性：不依赖单一服务器，合约逻辑很难被单点关停
    
-   类比：24 小时自动商店（店员不是人，是网络共识+代码）
    

### 代币（Token）

-   ERC-20：同质化（每一单位都一样）
    
-   ERC-721 / 1155：NFT（非同质化/半同质化）
    
-   你要会讲：**代币=合约发行的“可编程资产”，是 dApp 的经济层**
    

### 共识：PoW → PoS（理解差异就够）

-   PoW：用电力/算力押注诚实
    
-   PoS：用质押 ETH + slashing（削减）押注诚实
    
-   The Merge 后：以太坊进入 PoS 时代
    

### Gas（费用结构：你要会算逻辑）

-   Gas 是“计算/存储消耗”的计量单位
    
-   EIP-1559 后：费用大体可理解为  
    **实际费用 ≈ GasUsed × (BaseFee + Tip)**
    
    -   BaseFee：被销毁（burn）
        
    -   Tip：给出块者/验证者激励
        

## 扩容主线：Rollup + EIP-4844

### 扩容逻辑

-   L1 不追求无限 TPS（否则会牺牲去中心化）
    
-   官方主线是 **Rollup-centric**：
    
    -   L2 负责执行大量交易
        
    -   L1 负责安全、结算、数据可用性（DA）
        

### Proto-Danksharding / EIP-4844（Dencun）

-   核心：给 Rollup 提供更便宜的数据发布通道（blob）
    
-   blob：临时数据，不进入永久状态（减少 L1 长期存储压力）
    
-   结果：L2 数据成本显著下降 → 用户手续费下降
    

**以前 L2 把大货物硬塞进 L1 的永久仓库；EIP-4844 给了“临时货运车厢”，过段时间自动清理。**

### Danksharding（方向感）

-   现在重点不是“多条执行分片链”，而是“数据分片/数据可用性扩容”
    
-   目标：未来每块承载更多 blob（吞吐更大），但仍保去中心化
    

## ETH 是什么？（必须能讲清楚的“三位一体”）

### ETH 的 3 个核心角色

1.  **Gas 燃料**：支付所有链上执行/存储费用
    
2.  **安全保证金**：PoS 质押，作恶会被 slash
    
3.  **金融基石资产**：DeFi 抵押、流动性、NFT 计价等
    

**ETH 既是网络资源的计价单位，也是共识安全的抵押品，还是生态内最核心的货币资产。**

## 以太坊 vs 比特币

-   **定位**：BTC = 硬钱/数字黄金；ETH = 可编程平台的原生资产
    
-   **可编程性**：BTC 脚本受限；ETH 有 EVM + 合约
    
-   **共识**：BTC PoW；ETH PoS（Merge 后）
    
-   **扩容**：BTC 偏 Lightning；ETH 偏 Rollup + DA 扩容（4844 / Danksharding）
    

## dApp

### 优点（为什么要用链）

-   不需要信任运营方（规则写在合约里，可审计可验证）
    
-   抗审查、抗单点关停
    
-   数据不可随意篡改（可追溯）
    

### 代价（Web3 的真实成本）

-   合约难升级（出 bug 代价大）
    
-   L1 贵/慢（所以要 L2）
    
-   用户门槛高（钱包、私钥、gas、L1/L2）
    
-   为了 UX 很多项目会“再中心化”（托管、CDN、后端算逻辑）
    

## 去中心化是怎么来的？

1.  **共识层**：PoS + slashing/inactivity leak（经济激励约束）
    
2.  **网络层**：P2P 节点广播（没有中心服务器）
    
3.  **实现层**：多客户端（避免单点 bug 毁全网）
    
4.  **治理层**：EIP + 公开讨论 + 节点选择是否升级（粗共识+退出权）
    

**去中心化不是一个开关，是多层机制叠出来的“没有单点控制”。**

## “无限制参与”

-   理念上：permissionless（无需许可）
    
-   现实上：硬件/带宽/存储有门槛
    
-   解决思路：分层（L1 安全结算 + L2 执行扩容 + DA 扩容）
    

## 成功应用赛道

1.  **DeFi / 稳定币 / 质押衍生品 / RWA**
    
2.  **NFT / 游戏 / 虚拟世界（大量在 L2）**
    
3.  **社交 & DAO & 公共物品资助（Gitcoin 等）**
    

# 胡老师小课堂

## 一、技术岗（Technical Roles）

### 1️⃣ 前端 / 后端工程师（Web3 技术入口首选）

**入门门槛**

-   Web2 前端/后端基础即可（React / Node / Python / Go）
    
-   **不要求**一开始就会 Solidity
    
-   会对接链上 SDK、钱包 API 是加分项
    

**转型周期**

-   Web2 有基础：**1–3 个月**
    
-   零基础：**6 个月起**
    

**薪资区间（远程为主）**

-   初级：15k–25k RMB / 月
    
-   熟练：30k–50k+
    
-   海外项目可用 USDT / USD 结算
    

**天花板**

-   高级工程师 / Tech Lead
    
-   可横跳协议、钱包、基础设施
    
-   天花板明显**高于运营**
    

**压力 & 流动性**

-   技术压力中高
    
-   流动性中等（牛市需求暴涨，熊市裁员）
    

📌 他明确说过：  
👉 **“不要一上来就冲智能合约开发”**

### 2️⃣ 智能合约工程师（Solidity）

**入门门槛**

-   Solidity + EVM + 安全意识
    
-   项目经验非常重要（不是写教程）
    

**转型周期**

-   非技术转：**基本不现实**
    
-   技术转：**6–12 个月**
    

**薪资区间**

-   初级：25k–40k
    
-   成熟：50k–80k+
    

**天花板**

-   高，但**岗位极少**
    
-   安全事故一次可能直接“职业事故”
    

**压力 & 流动性**

-   压力极高
    
-   流动性低（岗位少，要求极高）
    

📌 他的原话逻辑是：  
👉 **“现在冲合约开发，性价比很低”**

### 3️⃣ ZK / 协议 / 密码学工程师（顶级难度）

**入门门槛**

-   数学 + 密码学 + 系统底层
    
-   极强技术背景
    

**转型周期**

-   **1–2 年起**
    

**薪资区间**

-   60k–100k+
    
-   顶级岗位美元计价
    

**天花板**

-   极高（协议核心、联合创始人级）
    

**压力 & 流动性**

-   技术压力极高
    
-   流动性极低（但供需极不平衡）
    

📌 他说得很清楚：  
👉 **“没兴趣别碰”**

## 二、非技术岗（Non-Technical Roles）——他重点推荐

### 4️⃣ 社区运营 / 社媒运营（最容易入行）

**入门门槛**

-   会用 Telegram / Discord / Twitter
    
-   会写文案、维护社群、做 AMA
    
-   **不要求技术背景**
    

**转型周期**

-   **1 个月内可入行**
    

**薪资区间**

-   初级：8k–15k
    
-   熟练：15k–25k
    
-   多项目可叠加
    

**天花板**

-   中等
    
-   可转增长 / BD / 市场负责人
    

**压力 & 流动性**

-   压力中等
    
-   流动性极高（岗位多）
    

👉 **这是“最低摩擦入口”**

### 5️⃣ 大使 / Mod（低级运营）

**入门门槛**

-   在线时间
    
-   社群活跃
    
-   会英文更好
    

**转型周期**

-   **0–2 周**
    

**薪资区间**

-   兼职：2k–6k
    
-   有转正空间
    

**天花板**

-   本身很低
    
-   **作用是跳板**
    

**压力 & 流动性**

-   压力低
    
-   流动性极高
    

📌 关键点：  
👉 **很多机会只给熟人**

### 6️⃣ 增长运营 / 市场（Growth / Marketing）

**入门门槛**

-   数据意识（DAU / UV / 转化）
    
-   活动策划 / 投流 / KOL 合作
    

**转型周期**

-   从社区运营转：**3–6 个月**
    

**薪资区间**

-   20k–40k
    
-   成熟后上不封顶（和结果绑定）
    

**天花板**

-   很高
    
-   可做到市场负责人
    

**压力 & 流动性**

-   压力高（对结果负责）
    
-   流动性中高
    

### 7️⃣ BD / 商务（特别强调）

**入门门槛**

-   极强社交能力
    
-   能带资源
    
-   抗压
    

**转型周期**

-   有资源：**1–2 个月**
    
-   无资源：很难
    

**薪资区间**

-   底薪 + 提成
    
-   **头部 BD 月入可到百万 RMB（极少数）**
    

**天花板**

-   极高（交易量 = 钱）
    

**压力 & 流动性**

-   **压力极大**
    
-   流动性极大（KPI 残酷）
    

📌 这是**高风险高回报岗**

## 三、Web3 入行的「最短路径模型」

### 核心知识点

**Web3 不走“投简历—HR—面试”那条主路**

### 具体拆解

-   主流路径是：
    
    ```
    社群 / 大使 / 朋友介绍
          ↓
       兼职 / 远程
          ↓
       正职 / 多项目
    ```
    
-   **第一份 Web3 工作 ≠ 正式岗位**
    
-   身份比 title 重要
    
-   “混进去”是合法策略，不是投机
    

📌 可迁移性：适用于 DAO / 初创 / 海外远程 / 创业团队

## 四、MVP 思维（他反复强调的底层逻辑）

### 核心知识点

不要补短板，只放大长板

### 具体做法

-   不等“学完”
    
-   先做：
    
    -   能交付的
        
    -   能被用的
        
    -   能被验证的
        

### 举例

-   不会合约？→ 做前端
    
-   不会技术？→ 做社区
    
-   不会增长？→ 先做执行
    

## 三、社交与资源

### Web3 社交的「真实规则」

### 核心知识点

Web3 是熟人社会

### 具体规则

-   机会优先给：
    
    -   认识的
        
    -   见过面的
        
    -   聊过项目的
        
-   不是给“最厉害的简历”
    

### 操作级知识

-   跑会要做的事：
    
    -   拍合照
        
    -   记备注
        
    -   打标签
        
-   建个人资源库：
    
    -   项目方
        
    -   交易所
        
    -   KOL
        
    -   投资人
        

### 跑会 / 黑客松的正确用法

### 核心知识点

跑会不是学习，是建联

### 隐性经验

-   不要一个人去（能量消耗巨大）
    
-   有搭子效率翻倍
    
-   技术人一定要：
    
    -   组队
        
    -   留联系方式
        
    -   后续跟进
        

# Web3 行业全据介绍 & 岗位概览

## 历史回溯

### 第一阶段

2009-2014, BTC 主导的1.0时代，草莽时期

### 第二阶段

2015（以太坊上主网）-2020, ETH主导的ICO+ 同期崛起的第二代交易所（币安），逐步正规。

### 第三阶段

2020-2024，Web3 概念爆发（NFT+DeFi+Gamefi、去中心化钱包Metamask、OpenSea 等去中心化产品，解决区块链的扩容问题的L2。应用更多，正规军大规模入场。

### 第四阶段

2024-至今，宏观合规、ETF上市、RWA（资产上链）、DePIN（物理设施）、Al+Web3。推动更多资本巨头入场。

**世界上人造杠杆率最高的项目之一**

## 从大基建到合规化

2026年市场规模约69.4亿美元，年复合增长率（CAGR）为49.8%。

### 开发者数量

-   全球月活开发者：约2.5-3万人
    
-   连续两年基本持平
    

### 招聘情况

2025 年招聘反弹 47%（新增约6.6万个岗位）

### 重点增长区域

-   北美： 适合从事技术开发类工作
    
-   中东（阿联酋、沙特）：适合做项目的商务拓展和市场类工作
    
-   香港/东南亚：适合做交易所、资产管理、运营等工作
    
-   拉美（支付、稳定币）行业重心转移：
    
-   从单纯的 DeFi投机转向 RWA （现实资产）、DePIN（硬件设施）和 AI融合。
    

## 岗位数据与分布

### 平均录取比：

1个岗位-vs 450份简历（450:1）

### 职能分化：

**红海（Red Ocean）：** 初级 Solidity 开发、初级社区运营（Mod）。

\*\*蓝海（Blue Ocean）：\*\*资深 Rust 工程师、合规官、Research Analyst（复合型）。

**初级人才过剩，精英极度短缺**

### 隐形门槛：

70% 的面试会考察 AI 工具使用能力和链上交互记录 （On-chain Reputation）

## 渠道分散化

### 失效渠道：

传统招聘网站（Indeed,Boss直聘）覆盖率低。

### 核心渠道：

-   ﻿﻿Web3 垂直平台：SmartDeer, [Web3.career](http://Web3.career), Remote3, CryptoJobsList。
    
-   ﻿﻿社交网络：Twitter （X）， Web3媒体，Farcaster （Warpcast）， Discord 官方公告区，Linkedin及公司官网  
    (Notion)
    
-   隐形渠道：GitHub Contribution （代码贡献即简历），DAO 治理提案。
    

## 职业画像与薪酬体系

### 用人特征

**Web3的团队特征：**

1.  ﻿﻿团队规模不大，团队都是高精尖人才
    
2.  ﻿﻿业务模式主要面向海外用户；
    
3.  ﻿﻿公司主体以海外注册为主，部分团队在内地有办公室
    

**Web3的创始人背景：**

1.  ﻿﻿创始团队背景非常优秀
    
2.  ﻿﻿创始人多为传统互联网圈前辈或95后海外名校crypto native年轻人
    
3.  ﻿﻿非常熟悉现有的互联网的团队模式和业务模式
    
4.  ﻿﻿创始人有激情，崇尚新技术
    

**Web3的人才选择偏好：**

1.  top院校毕业
    
2.  ﻿﻿人选有互联网的行业背景
    
3.  具有高潜特征，学习能力强，有意愿
    
4.  区块链的行业背景不是必选项
    

**Web3的工作环境：**

1.  ﻿﻿团队国际化合作，团队成员分布全球
    
2.  ﻿﻿英文会是常用的工作语言
    
3.  ﻿﻿团队合作Remote是常态，不强求坐班
    

## 如何进入web3

你是谁不如**你做了什么**重要

### 从 Web2 带能力迁移

web2 头部公司技术/产品/增长

有海外/英文工作经验，能独立交付项目成果

HR / 财务 / 法务/ 运营支持

全球雇佣 /薪酬 /合规 / 牌照

### 从生态侧切入 Permissionless Work

DAO

开源社区

投资研究社区

### 建立链上声誉

Twitter (X) / Farcaster

输出研报、参与 Space 讨论，让Founder在公域流量池里看到你

## 不同岗位的能力要求

### 开发岗

Solidity: web3最广泛通用的开发语言，易上手，建议新手要学习一下，理解智能合约的编程思维逻辑.

趋势：Rust 的应用越来越多，很多链和自创语言基于 Rust 开发学习和关注Web3 新语言：Move, Cario, Func 等.

合约安全意识：系统设计：对DeFi/L2的理解

### 运营/市场岗

偏好做过社区，海外媒体运营如Twitter, Facebook, discord等背景人选：

英文口语能力：懂 token economics

链上活跃，做过转化闭环，了解Web2和Web3获客方式。

### 产品/投研岗

偏好社区和做过积分系统背景的产品经理

计算机金融专业毕业产品人选有优势

## 求职秘籍：不仅是简历

### 01 ‘打造“链上CV"

贡献开源代码（GitHub）、参与项目治理（Snapshot 投票）、累积链上徽章。

### 02 熟用 AI工具

展示如何利用AI提升智能合约审计或自动化运营的效率。

### 03 掌握新语言

除 Solidity 外，Rust 已成为Layer 2 和 Solana 生态的必选项，关注 Move/Cairo 等新语言。

### 04 避坑指南

如何识别“Rug”项目；警惕匿名、无风投支持、无代码更新的“三无”团队。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
