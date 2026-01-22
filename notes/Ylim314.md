---
timezone: UTC+8
---

# ChaplinX

**GitHub ID:** Ylim314

**Telegram:** @Tikey19

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->
# 1月23日学习日志：

今天的课程从两个极端切入：一边是《DApp 基础》里的第一性原理，另一边是《Aave V4》的硬核架构设计。作为工程学生，我试图找到连接这两者的红线——代码如何以去中心化的方式管理巨额资产。

### 1\. DApp 的工程隐喻：自动贩卖机

《DApp 基础》里提到一个极佳的比喻：

-   Web2 App = 小卖部：必须经过老板（中心化服务器）同意才能买东西，老板可以随时关门。
    
-   Web3 DApp = 广场上的自动贩卖机：逻辑写死在硬件（合约）里，只要投币（Gas + Value），机器必须吐出商品。没人能拔电源。
    
-   这解释了为什么 Smart Contract 必须开源且经过审计，因为一旦部署，由于 `Code is Law`，Bug 也会成为不可逆的规则。
    

### 2\. Aave V4：分布式系统的拓扑升级

从 V3 到 V4 的演进，本质上是一次分布式系统的架构重构，旨在解决 EVM 多链时代的流动性碎片化问题。

-   V3 问题 ：以太坊主网、Arbitrum、Optimism 上的 Aave 池子互不通气。你在 Arbitrum 有钱，在主网还是借不出来。
    
-   V4 方案 ：
    
    -   Unified Liquidity Layer (Hub)：一个统一的流动性账本，管理所有链的资产状态。
        
    -   Spokes：各条链上的借贷网关。
        
    -   技术美感：这实现了跨链的原子性。用户在 L2 存钱，无需复杂的跨桥操作，即可在 L1 享受信用额度。
        

### 3\. Solidity 硬核技巧：位图压缩

在 Aave 的技术文档中，我发现了一个极其优雅的 Gas 优化案例，值得我以后的合约开发借鉴：

-   场景：一个资产有 30+ 个配置项（LTV、清算阈值、激活状态等）。
    
-   错误写法：定义 30 个 `bool` 或 `uint8` 变量。这会导致几十次 `SLOAD` 操作，Gas 费爆炸。
    
-   Aave 写法：`ReserveConfigurationMap` (位图)。
    
    -   将所有配置压缩进一个 `uint256` 整数。
        
    -   Bit 0-15: LTV (抵押率)
        
    -   Bit 16-31: Liquidation Threshold
        
    -   Bit 56: Active Flag
        
-   应用：利用 位运算 (Bitwise Operations) 进行读取和修改。虽然代码可读性降低，但执行效率达到了极致。
    

### 4\. AI x DeFi：清算机器人的猎场

Aave 的文档反复提到 Liquidator (清算人)。

-   机制：协议无法自动清算坏账，必须依赖外部调用。
    
-   AI 机会：这是 AI Agent 的完美战场。
    
    -   我不需要手动操作，而是写一个 Python 脚本（Searcher），实时监控链上价格和用户健康因子 (Health Factor)。
        
    -   一旦 `HF < 1`，Agent 自动触发 `liquidationCall()`，瞬间赚取 5-10% 的清算奖金。
        
    -   Next Action：研究 Aave 的清算接口，尝试在测试网写一个简易的 Watchdog 脚本。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->

# 1月22日学习日志：

结合 Wachi 导师关于“全栈转型”的建议，和晚上 Tina/Lynn 导师对“休闲黑客松”的案例拆解，我意识到：黑客松是检验工程能力与产品思维的最佳练兵场。

### 1\. 黑客松的本质：高并发的软件工程管理

听完会议纪要，我发现运营一场黑客松的逻辑，和架构一个高可用的分布式系统惊人地相似：

-   Demo Day (2.1) = Mainnet Launch（主网已上线）：这是死线，所有功能必须在此刻可用。
    
-   筹备组 (30人) = Microservices（微服务架构）：
    
    -   小组长：API Gateway（流量网关），处理所有请求分发。
        
    -   BD组：Oracle（预言机），引入外部资源（嘉宾/赞助）。
        
    -   技术组：Backend/Infrastructure，确保基础设施（提交渠道、评审系统）不崩。
        
-   SOP 执行 = Smart Contract Logic：一旦部署（宣发发出），规则不可更改，必须按预设逻辑自动执行。
    

### 2\. 我的定位：Tech Support 还是 DevRel？

会议提到招募“技术组”负责解答技术问题和初审代码。这让我对“工程师”的定义有了新的理解：

-   传统视角：我只会写代码。
    
-   黑客松视角：这其实是 DevRel 的雏形。
    
-   价值点：作为大三工程学生，如果我能去审核别人的代码，甚至帮非技术背景的团队 Debug，这比自己写一个 Hello World 合约更有含金量。通过阅读几十个项目的源码，能极快提升我的审计直觉。
    

### 3\. AI 赋能黑客松运营 (My AI Edge)

如果我加入筹备组，我不想只做人力电池，我想引入 AI 提效：

-   AI 评审初筛：利用 GPT 写一个脚本，自动读取 GitHub 提交的 Readme 和代码结构，生成初步的“完整性报告”，辅助评委筛选。
    
-   Q&A Bot：把黑客松规则喂给 Dify 或 Coze，做一个 TG 答疑机器人，7x24 小时回答“怎么提交？”“截止时间是多少？”等重复性问题。
    

### 4\. 行动计划 (Action Items)

-   **报名**：申请加入 **技术组** 或 **协调者** 。理由是：既懂 Solidity 能审代码，又懂 AI 能做自动化工具。
    
-   **备战**：虽然我是筹备组，但我也可以利用这个机会观察优秀的 Demo 是如何切中痛点的，为我下一次自己参赛做“竞品分析”。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->


### 1月21日学习日志：

**核心：** 用代码定义资产规则，用运营定义社区共识。

1\. Solidity 工程进阶 (Hard Skill)

重读文档 0.8.24，挖掘出两个对 **AI Agent** 极重要的 Gas 优化点：

-   **Calldata 替代 Memory**：
    
    -   _原理_：`calldata` 是只读区域，无需数据拷贝。
        
    -   _应用_：AI 提交庞大 Proof 数据时，强制使用 `calldata`，Gas 费可降 30%+。
        
-   **Events 作为 AI 触发器**：
    
    -   _原理_：AI 无法高频轮询链上状态。
        
    -   _应用_：合约必须抛出 Event（如 `TradeExecuted`），让链下 Python 脚本通过监听日志实现毫秒级响应。
        

2\. 社区运营的工程化 (Soft Skill)

将 Luna 导师的运营课解构为合约逻辑：

-   **权限管理**：TG 群的 Admin 设置 = `AccessControl.sol` 的链下版。
    
-   **运营 Bot**：现在的 Rule-based 机器人（如 Rose）是未来 **LLM-Based Agent** 的雏形。
    
    -   _思路_：用 GPT-4 接入 TG Bot，做一个能读白皮书的自动客服。
        
-   **活动宣发**：AMA 流程 = **主网部署**（海报是前端，流程表是测试用例）。
    

3\. 明日计划

-   尝试写一个含 `AccessControl` 的合约，模拟 TG 群组权限。
    
-   实测 `calldata` 的省钱效果。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->



**1月20日学习日志：**

第二周的学习正式开始，今天的重头戏是 DataDance 的 Alex 导师带来的关于 ERC-7962 的深度架构分享。这不仅是对 ERC-721 的改进，更是一次对以太坊底层账户模型的“降维打击”与重构。

### 一、 架构层面的范式转移

1\. 传统模型 (ERC-721/20) 的痛点

在传统的 Solidity 开发中，我们习惯了 mapping(tokenId => address)。这种设计将资产强绑定在 Address上。

-   隐私裸奔：只要知道地址，就能查到该地址下的所有资产。
    
-   授权风险：转移资产必须先 `Approve`，导致了无数的钓鱼攻击。
    

2\. ERC-7962 的 UTXO 化尝试

Alex 提出的 ERC-7962 核心在于将所有权映射改为 mapping(tokenId => bytes32 keyHash)。

-   所有权解耦：资产不再属于某个“钱包”，而是属于某个“公钥哈希”。谁拥有这把钥匙，谁就能花费这个资产。
    
-   类似比特币：这本质上是在以太坊的 Account 模型上，通过智能合约实现了一套类 UTXO的逻辑。
    

### 二、 隐私与合规的工程博弈

结合会议中 Alex 与学员的 Q&A，我对 ERC-7962 的隐私特性有了更准确的工程认知：

-   它是“Output Privacy”而非“完全匿名”：
    
    它不同于 Tornado Cash 的混币逻辑。ERC-7962 的隐私性体现在\*\*“持有即隐身”\*\*。
    
    -   _Mint 阶段_：资产生成时，链上只记录一个 Hash。没人知道这个 Hash 背后是谁。
        
    -   _Transfer 阶段_：只有当用户出示公钥并签名消费时，链上才会通过 `ecrecover` 验证身份。
        
    -   _工程隐喻_：这就像是给每个 Token 发了一个“一次性保险箱”。只有开箱那一刻，大家才知道钥匙在你手里。
        

### 三、 对 AI Agent 的特殊意义

作为关注 AI 方向的开发者，我敏锐地发现 ERC-7962 是 AI Agent 资产交互的绝佳载体：

1.  免 Gas 交互 ：
    
    ERC-7962 天然支持元交易。AI Agent 可以只生成签名，而由专门的 Paymaster 合约代付 Gas。这意味着我们可以在链上部署成千上万个“贫穷”的 Agent，它们不需要持有 ETH 也能操作资产。
    
2.  一次性密钥：
    
    Slides 中提到的 Key Rotation功能，允许 AI Agent 为每一次任务生成临时的 Session Key。
    
    -   _场景_：Agent A 完成任务 -> 获得 Token -> 任务结束 -> Agent A 销毁 Key A，将资产“发送”给 Key B（无需暴露 Agent A 的主钱包地址）。这极大地提升了自动化系统的安全性。
        
3.  消除 Approve 授权风险：
    
    由于所有权是基于签名的，转移 Token 不需要 Approve 合约。AI 在执行高频交易时，不再面临“授权给恶意 DEX 后被清空钱包”的风险。
    

### 四、 关键技术细节备忘

-   验证逻辑：不是存储 `address`，而是验证 `keccak256(abi.encodePacked(publicKey)) == storedKeyHash`。
    
-   安全防范：为了防止重放攻击，ERC-7962 在签名结构中引入了 `per-keyHash nonce`。这与我们周一学的 EOA Nonce 机制异曲同工，但在应用层实现了更细粒度的控制。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->





\# 1月18日工程日志：EVM 架构视角与战术预演 (Week 1 Day 7)

\## 第一周复盘

第一周结束，我完成了对 Web3 \*\*底层基础设施、安全边界、合规逻辑\*\* 的完整闭环构建。

作为工程方向的 Builder，今天的核心任务是为下周的 Solidity 开发建立\*\*“EVM 架构观”\*\*。

\## 核心思考：原子性与可组合性

在预习下周的 DeFi 协议逻辑时，我深刻理解了为什么以太坊被称为 "Infinite Machine"：

1\. \*\*原子性 (Atomicity)\*\*：

Web2 的数据库事务需要复杂的锁机制，而在 EVM 中，一笔交易要么全部成功，要么完全回滚。

\* \*工程启示\*：这让 \*\*Flashloan（闪电贷）\*\* 成为可能——借款、套利、还款在一个区块内完成，无风险套利的本质就是利用了 EVM 的原子性。

2\. \*\*可组合性 (Money Legos)\*\*：

Aave 的存款凭证 (aToken) 可以被 Uniswap 交易，又可以作为 MakerDAO 的抵押品。

\* \*AI 结合点\*：AI Agent 不需要像人类一样去跟每个银行谈接口，它只需要调用合约的标准 ABI。\*\*智能合约是为 AI 诞生的原生 API\*\*。

\## 下周战术部署 (Week 2 Preview)

下周进入 Solidity 深水区，我将不再关注基础语法，而是聚焦于\*\*底层存储与 Gas 优化\*\*：

\* \*\*Storage Layout\*\*：深入理解 Slot 0, Slot 1 的存储机制，这是手写汇编优化 Gas 的前提。

\* \*\*Memory vs Calldata\*\*：彻底搞懂数据拷贝成本，写出 Gas Efficient 的代码。

\* \*\*DelegateCall\*\*：掌握代理合约（Proxy）模式，理解合约的可升级性架构。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->






1月17日工程日志： ## 🎯 核心议题 \*\*From Scripting to Engineering.\*\* 放弃 JS/TS 驱动的 Hardhat 框架，全面迁移至以太坊基金会及 Paradigm 强推的 \*\*Foundry\*\* 工具链。 \*\*目标：\*\* 建立一套支持 Fuzzing（模糊测试）、Gas Snapshot（Gas 快照对比）及 Mainnet Forking（主网分叉模拟）的工业级开发环境。 --- ## 🛠️ 1. 技术选型：Why Foundry Over Hardhat? 1. \*\*测试反馈环 (Feedback Loop)\*\*：Foundry 基于 Rust 编写的 EVM，编译速度比 Hardhat (Node.js) 快 20-50 倍。在跑大型协议的集成测试时，这几分钟的差距决定了开发体验。 2. \*\*上下文统一 (Context Switching)\*\*：Hardhat 强迫开发者用 JS/TS 写测试逻辑（异步操作、BigNumber 处理极其痛苦）。Foundry 允许 \*\*Solidity-Native Testing\*\*，直接用 Solidity 写测试，测试代码本身就是合约逻辑的一部分。 3. \*\*高级测试原语\*\*：原生支持 \*\*Property-Based Testing (Fuzzing)\*\* 和 \*\*Invariant Testing\*\*，这是传统 Unit Test 无法覆盖的安全盲区。 --- ## ⚡ 2. 工程环境配置 (Infrastructure) ### 2.1 基础组件安装 跳过基础 curl 安装，重点配置 \`foundry.toml\` 以适配不同网络环境与优化器设置。 \`\`\`toml # foundry.toml 核心配置 \[profile.default\] src = 'src' out = 'out' libs = \['lib'\] op`imizer = tru`optimizer\_runs = 20000 #`针对生产环境的高频调用优`fs\_permissions = \[{ access = "read", path = "./"}\] # 允`读取本地文件，用于复杂脚`

### 2.2 依赖管理 (Dependency Management)

使用 Git Submodules 替代 npm/yarn，确保合约库的源码级可溯源性。

Bash

```
# 安装 OpenZeppelin 和 Solmate (Gas 优化狂魔必备)
forge install OpenZeppelin/openzeppelin-contracts
forge install transmissions11/solmate 
```

* * *

## 🔬 3. 核心组件深度实战 (Deep Dive)

### 3.1 Forge: 模糊测试与 Gas 审计

这是 Foundry 的杀手锏。我不只写单元测试，我写不变量测试。

实战：编写一个 Fuzzing Test Case

不同于 Hardhat 手动构造边缘数据，Foundry 会自动生成随机数轰炸合约接口。

Solidity

```
// test/Vault.t.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

import "forge-std/Test.sol";
import "../src/Vault.sol";

contract VaultTest is Test {
    Vault public vault;

    function setUp() public {
        vault = new Vault();
    }

    // Fuzzing Test: Foundry 会自动填充 amount 参数进行数千次调用
    // 验证逻辑：无论存多少钱，取出的金额不应超过存款（假设无收益）
    function testFuzz_Withdraw(uint256 amount) public {
        // 限制输入范围，避免溢出测试干扰业务逻辑
        vm.assume(amount > 0 && amount < 1000 ether);
        
        vault.deposit{value: amount}();
        uint256 balanceBefore = address(this).balance;
        
        vault.withdraw(amount);
        
        assertEq(address(this).balance, balanceBefore + amount);
    }
}
```

**运行 Gas 报告：**

Bash

```
forge test --gas-report
# 输出详细的 Gas 消耗表，精确到每个函数的平均/最大/最小消耗
```

### 3.2 Cast: 命令行级 RPC 交互

`Cast` 不仅仅是工具，它是对 **EVM 数据结构** 的直接操作。

-   **ABI 解码**：`cast 4byte-decode [Calldata]` —— 逆向分析黑客交易时的利器。
    
-   **存储槽读取**：`cast storage [Address] [Slot]` —— 绕过 `private` 变量限制，直接读取合约底层 Storage Layout。
    

**实战：读取合约的 Private 变量（验证链上无隐私）**

Bash

```
# 假设 Slot 0 存储了管理员地址
cast storage 0x...ContractAddress... 0 --rpc-url $SEPOLIA_RPC
```

### 3.3 Chisel: Solidity REPL

在写复杂算法（如位运算、汇编优化）时，不需要部署合约，直接在终端验证逻辑。

Bash

```
$ chisel
➜ uint256 a = 0xff;
➜ uint256 b = 0x01;
➜ a & b
Type: uint256
├ Hex: 0x0000000000000000000000000000000000000000000000000000000000000001
└ Decimal: 1
```

* * *

今天的环境搭建引发了我对 AI 辅助审计 的思考：

传统的审计依赖人工阅读代码，而 Foundry 提供了完美的 Trace 功能 。

未来的 AI Agent 审计工具，极有可能是基于 forge test 的输出日志进行训练的。Agent 不再只是“看代码”，而是“看执行路径”，结合 Fuzzing 自动生成的边缘 Case，实现自动化的漏洞挖掘。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->







**1月16日学习日志：**

第一周的实习即将结束，今天的周会和复盘让我对“Web3工程师”这个职业有了全新的定义。如果说在学校是学习如何解题，那么这周的实战则是学习如何定义问题和构建规则。以下是本周的四维工程重构：

**1\. 身份认知的重构：Permissionless Work**

-   在周会的交流中，我深刻体会到Web3最大的红利不是币价涨跌，而是\*\*“无需许可的工作权”\*\*。以前我认为找工作依赖于简历和面试，但通过提交文档PR和链上交互，我意识到：GitHub的Commit记录和Etherscan的Tx Hash才是工程师最硬的通行证。
    
-   对于AI Agent而言，这种“无需许可”更是生存土壤。Agent不需要通过图灵测试成为“人”才能工作，只要它能签名交易、支付Gas，它就是链上的“一等公民”。
    

**2\. 技术视野的重构：确定性 vs 概率性**

-   **复盘串联**：
    
    -   Day 2原理：理解了`Unfinalized`状态和RPC机制。
        
    -   Day 3安全：见识了黑客如何利用重入和前端篡改（Bybit案例）。
        
    -   Day 4实操：亲自Mint NFT体验了Gas费波动。
        
-   我发现Web3的本质是“确定性机器”（Code is Law）**，而**AI的本质是“概率性模型”。 这一周的学习让我明确了未来的工程方向：如何让概率性的AI Agent安全地运行在确定性的智能合约之上？答案可能在于\*\*“限制权限”（如Session Keys）和“链上验证”\*\*（zkML）。
    

**3\. 风险意识的重构：代码之外的红线**

-   邓小宇律师的合规课和GoPlus的安全课像两把悬顶之剑。
    
-   在写代码前，我现在会下意识地问两个问题：
    
    1.  **安全层**：这个函数的授权逻辑（Approval）是否会被滥用？
        
    2.  法律层：这个业务逻辑（如开仓公式）是否构成了“开设赌场”？
        
        这周最大的收获是明白了技术中立”不是免死金牌，合规设计必须前置到架构阶段。
        

**4\. 行动力的重构：从Read到Write**

-   **本周产出**：
    
    -   完成常用钱包（MetaMask）的开发环境配置。
        
    -   完成Sepolia测试网的资金流转与NFT铸造。
        
    -   贡献了第一个开源文档PR（关于交易确定性）。
        
    -   通过了Unphishable的安全攻防挑战。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->








**1月15日学习日志：**

经过前三天关于原理、安全与合规的高密度输入，今天我利用空档期对“AI x Crypto”这一核心赛道进行了底层逻辑的梳理，并为明天的分享会做了最后准备。

**1\. 技术视角的深度复盘（Deep Dive）**

-   重新审视了昨天Mint NFT时调用的ERC-721合约源码。
    
-   我在MetaMask点一下就能Mint。但如果是一个AI Agent呢？它无法点击UI。这让我意识到账户抽象（ERC-4337）对于AI赛道的必要性——Agent需要拥有可编程的“智能钱包”，通过Paymaster代付Gas，并通过Session Key实现特定权限的自动化交互，而不是把私钥明文写在Python脚本里裸奔。
    

**2\. 赛道调研：去中心化算力与可验证推理**

-   结合行业概览中提到的“AI融合”趋势，查阅了Render与Gensyn的技术白皮书。
    
-   Web3赋能AI的痛点不在于“模型训练”（大厂已经做得很好），而在于去中心化推理（Inference）”。目前的工程难点是如何证明链下的模型计算结果是诚实的（Verifiable Inference）。这可能需要引入zkML（零知识机器学习）或OpML（乐观机器学习）技术栈。这是我未来想在简历上体现的技术差异化竞争点。
    

**3\. 合规与安全的再消化**

-   回顾邓律师的合规课，我检查了自己正在构思的“AI自动交易Bot”逻辑。
    
-   原计划是做一个“根据推特情绪自动冲土狗”的Bot，但现在意识到必须加上貔貅盘检测”（调用GoPlus API）和“滑点保护”。同时，必须确保Bot的策略是基于公开市场数据，而不是任何形式的内幕操控，以符合“金融衍生品”而非“赌博”的合规定义。
    

**4\. 分享会筹备**

-   整理了关于“基础设施单点风险对AI Agent影响”的发言稿。
    
-   在明天的复盘会上，直接抛出RPC节点对自动化程序的致命影响这个技术观点
    

今晚开始配置本地开发环境（Foundry），不再依赖网页版的Remix，为下周的硬核代码开发做准备。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->









**1月14日学习日志：**

今天完成了从理论学习到链上实操的转换，通过钱包配置、合约交互、开源贡献及安全攻防四个维度的实战，对昨日布老师讲解的底层原理进行了验证。以下是核心工程复盘：

**1\. 账户模型与RPC的交互验证**

-   **操作复盘**：配置MetaMask连接Sepolia测试网，通过Faucet领取测试币并完成转账。
    
-   **工程思考**：在点击“Send”的瞬间，直观理解了“交易组装 -> 私钥签名 -> RPC广播”的全流程。作为工程师，我意识到MetaMask其实就是一个管理私钥并连接RPC节点的客户端。对于未来的AI Agent开发，若要实现自动化交易，本质上就是用代码（如Ethers.js）替代MetaMask的UI层，通过脚本管理Nonce和Gas Price来实现交易上链。
    

**2\. 智能合约的确定性与状态流转**

-   **操作复盘**：在MyFirstNFT完成铸造，Tx Hash生成后，区块浏览器显示状态为`Unfinalized`。
    
-   **工程思考**：验证了以太坊PoS机制下的Finality延迟。这纠正了Web2的即时响应思维——在Web3中，数据库（链上状态）的写入是概率性的（存在重组风险），直到Epoch结束。这对DApp前端的状态管理（Loading/Success提示）和后端监听逻辑提出了更高的鲁棒性要求。
    

**3\. 开源协作的Permissionless体验（完成文档PR）**

-   **操作复盘**：向官方实习手册仓库提交了关于“交易确定性”的补充说明PR。
    
-   **工程思考**：这是我第一次践行“Permissionless Work”。不需要面试，不需要许可，直接Fork仓库修改代码/文档并提交。这种基于GitHub协作的工作流（Workflow）是Web3去中心化组织的基石。这也坚定了我在后续四周通过PR积累“链上简历”的决心。
    

**4\. 授权机制的安全隐患**

-   **操作复盘**：通过了钓鱼邮件识别和Token Approval挑战。
    
-   **工程思考**：深刻理解了`SetApprovalForAll`的危险性。一旦授权无限额度，合约即可随意划转用户资产。结合AI方向，我认为未来的Agent钱包必须引入“交易模拟（Transaction Simulation）”层，在签名之前预执行交易，分析是否存在过度授权或恶意逻辑，这可能是AI安全赛道的一个重要切入点。
    

**5\. 职业路径的战略锚定**

-   **规划落地**：确立了“智能合约架构师（AI方向）”的目标，制定了以底层原理、RWA合规标准、安全攻防为核心的T型学习路线。不再盲目追求热点，而是专注于基础设施与应用层的结合点。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->










**1月13日学习日志**

今天的布老师主讲的Web3 运行原理分享会给出了大量具体的网络实运行数据，结合讲座内容与我的最近学习背景，对以太坊底层架构的几个关键风险点进行了如下梳理：

**1\. RPC 层的中心化单点风险**

钱包并不直接连接区块链网络，而是通过 RPC 接口连接节点集群。如果 RPC 服务商拦截签名或拒绝转发，交易将无法上链。我们在开发中常习惯使用 Infura 等默认 RPC，但这构成了严重的单点故障。对于 Autonomous Agent（自主智能体）而言，若依赖单一 RPC 节点，一旦遭遇审查或服务宕机，Agent 将丧失链上交互能力。未来的 Agent 架构必须引入去中心化 RPC 网络或多节点负载均衡机制，以确保“永不掉线”。

**2\. 客户端多样性的数据警示**

目前执行层客户端中，Geth 的占比高达 49%。社区倾向于运行占比较低的客户端软件以降低风险。虽然以太坊十多年未宕机，但单一客户端占比过半意味着系统面临巨大的共识 Bug 风险。一旦 Geth 出现问题，网络分叉将不可避免。作为开发者，在后续配置节点时应避免盲从主流，尝试配置 Nethermind 或 Besu 等小众客户端是维护网络健壮性的必要举措。

**3\. 交易最终性的时间窗口**

虽然每 12 秒产出一个区块，但区块状态从 Unfinalized 变为 Finalized 大约需要 12-13 分钟。在 Mint NFT 的演示中，交易成功上链后区块状态仍是不确定的。这纠正了一个常见的开发误区——即看到 Transaction Hash 生成就认为业务完成。在设计涉及资金流转的后端逻辑（尤其是高频交易 Agent）时，必须设置足够的区块确认数，不能仅依赖即时状态，以防范区块重组带来的回滚风险。

**4\. 信任机制的代码化转换**

智能合约将对人、法律的信任转换为对代码、共识的信任。以 MyFirst NFT 为例，合约代码强制规定每个地址只能 Mint 一个，这一规则公开透明且不可篡改。这种“Code is Law”的特性是 Agent 经济的基础。相比于依赖法律合同，AI Agent 更适合在一个规则确定、逻辑可验证的链上环境中运行，智能合约即是 Agent 之间交互的最高协议。

明日将深入调研 EIP 提案流程及 ERC-4337 账户抽象文档，寻找解决私钥管理问题的最优解。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->











**1月12日学习日志：**

今天是Web3实习计划第一周正式开课的第一天。结束了昨晚的热身环节，今天我按照Week 1的学习路径，开始了第一阶段关于区块链与以太坊基础原理的梳理，同时参加了以太坊中文周会。

虽然作为相关专业的学生，我对基础概念已经比较熟悉，但今天的周会内容还是给我提供了很多新的技术视角。特别是关于以太坊PDS（提议者-构建者分离）升级的讨论，让我印象深刻。会议提到BPO2升级生效后，Blob容量虽然得到了8倍的扩容，利好Layer2的成本降低，但实验数据也表明Blob数量的增加会显著延长网络传播的延迟。这从工程角度看是一个非常经典的“不可能三角”权衡问题。对于我们开发者而言，这意味着在设计对时效性敏感的应用（比如某些链上博弈或高频交互的Agent）时，不能单纯依赖L2的高吞吐量，必须重新评估确认延迟带来的潜在风险。

周会中关于矿企Riot转型AI数据中心的消息，也印证了我对“算力流动”的判断。算力资源正在从单纯的维护账本安全向AI推理与训练倾斜。这种基础设施层面的变动，对于我感兴趣的Autonomous Agents方向来说是一个重要的信号，未来链上协议与AI算力的结合可能会比我们想象的更紧密。

作为第一周的第一天，我今天也重新回顾了以太坊开发者指南的入门部分。接下来的几天，我计划在快速完成Solidity基础语法复习的同时，重点去研究一下EIP提案的原始文档，不仅要知其然，还要知其所以然。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
