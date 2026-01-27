---
timezone: UTC+8
---

# evaristebernhardwiener@gmai.com

**GitHub ID:** evaristebernhard

**Telegram:** @henrycogito

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->
1.参与实习项目的对接
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->

1.作为运营对接休闲黑客松的bd需求

2.参加Hardman Vibe Coding

3.参加投研基础框架确立的课程
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->


1.参加休闲黑客松的运营

2.学习了很多dapp开发

3.参与公共物品资金分配的治理
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->



1.参加LXDAO周会

2.参加Web303组会
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->




1.参加Speedrun Basic Worshop

2.参加co-learning

3.参加Web3实习计划例会
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->





今天不学协议，专门把 **Remix 和 Hardhat** 当成“训练工具”来练，目标是：不是看懂概念，而是把常见流程跑熟、把常见坑踩一遍，最后能稳定产出一套可复用的开发模板。

### 1）Remix：把“写—编译—部署—调试”流程压到肌肉记忆

-   新建一个最小合约（Counter 级别），刻意覆盖：`constructor`、`require`、`event`、`public view` 读状态、`external` 写状态。
    
-   反复切换 Solidity 编译版本，确认：同一份代码在版本不一致时会出现哪些编译报错（比如语法/内置 overflow 检查差异），并记录“固定版本”是必须的。
    
-   在 Remix VM 部署后，分别用不同账户调用写函数，观察：
    
    -   state 变量变化是否符合预期
        
    -   event 是否正常 emit
        
    -   每次写操作都会生成交易并消耗 gas（读操作不产生交易）
        
-   今天特意把 Debugger 点开跑了一次交易，至少看懂：calldata、storage 改动、event log 在哪里出现。
    

**今日产出**：一个能稳定编译部署、带事件日志的最小合约 + 一次完整调试记录（知道去哪里看 state / log / tx detail）。

* * *

### 2）Hardhat：把“项目化开发”的骨架搭起来（不是只会跑命令）

今天重点不是写复杂业务，而是把 Hardhat 的“工程结构”跑通：

-   初始化项目后把目录结构对齐清楚：
    
    -   `contracts/` 放合约
        
    -   `scripts/` 放部署脚本
        
    -   `test/` 放测试用例
        
-   启动本地链（Hardhat Network），跑通一次“脚本化部署”，让流程可复制，而不是像 Remix 那样手动点。
    
-   配置并理解最基本的部署脚本结构：拿到合约工厂、deploy、wait、打印地址。
    
-   把 Remix 里那个最小合约搬进 Hardhat，验证：
    
    -   编译 OK
        
    -   部署 OK
        
    -   通过脚本调用写函数 OK（并能拿到交易回执）
        
-   顺手处理了 2 个典型问题：
    
    -   依赖/插件版本冲突导致的报错（知道从 package.json / hardhat.config.js 定位）
        
    -   网络配置/私钥环境变量的基本写法（至少能区分本地链和测试网）
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->






今天的学习主要围绕 Uniswap 的 AMM 数学模型和其在合约中的实现方式展开，目标是把“公式—机制—代码”三者对应起来，而不是停留在概念层。

首先在数学层面，重点理解了 Uniswap V2 的定价模型来源。从流动性池中两种资产的边际变化关系出发，得到价格变化满足 `dy/dx = -y/x`，由此自然推出恒定乘积关系 `x * y = k`。通过这一模型，进一步理解了交易过程中价格滑点产生的原因，以及为什么交易规模越大，对价格影响越明显。

在此基础上，学习并复盘了无常损失的计算思路，明确无常损失并不是“手续费扣走的钱”，而是由于价格变化导致 LP 资产结构被动调整，与单纯持币相比出现的价值差异。通过对比“持有资产”和“做 LP”的两种路径，理解了无常损失随价格偏离程度变化的趋势。

随后重点分析了 LP 的收益来源。明确 LP 的收益并非来自价格上涨，而是来自每次 swap 中收取的 0.3% 手续费。这部分手续费并不会单独结算，而是直接留在池子中，体现在储备增加和 LP Token 可兑换价值上，从机制上理解了“手续费如何补偿无常损失”。

在编程层面，阅读了 Uniswap V2 的核心合约逻辑，重点关注了 Pair 合约中的 `swap` 实现方式，理解了通过余额变化反推出真实输入量，以及通过常数乘积不变式（含手续费修正）进行最终校验的设计思路。

整体来看，今天的学习完成了从数学模型 → 经济机制 → 合约实现的完整串联，对 Uniswap V2 的工作原理有了更扎实的理解。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->







今日主要学习内容为 [ethereum.org](http://ethereum.org) 官方教程《Uniswap V2 Annotated Code》，对 Uniswap V2 合约结构和核心逻辑进行通读与理解。

学习过程中重点梳理了 Uniswap V2 的整体架构，明确了 Core（Factory、Pair）与 Periphery（Router）的职责划分。Pair 合约负责资产托管和核心逻辑执行，Router 主要作为用户交互入口，负责路径计算和多跳交易组织。

阅读了 Pair 合约中 swap 函数的完整流程，理解了其通过“先转出、再根据余额变化反推输入数量”的方式计算真实成交量，并在最后通过常数乘积不变式校验交易合法性。明确了 0.3% 手续费是通过 balance×1000 − amountIn×3 的形式融入不变式检查中完成的。

同时关注了合约中的安全设计，包括 lock 修饰符实现的防重入机制，以及 K 校验作为最终经济安全约束的作用。对 flash swap 的实现方式也有了更清晰的认识，即在 data 不为空时触发回调，只要最终满足不变式即可完成交易。

此外，阅读了 \_update 函数中关于价格累计（priceCumulative）的实现，理解其用于支持 TWAP 计算的设计思路，以及 Router 与 Library 中多跳 swap 和 pair 地址计算（CREATE2）的基本逻辑。

整体完成了对 Uniswap V2 核心交易流程和关键安全机制的初步理解。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->








### 1）今日学习内容概览

今天主要做了两件事：

-   跟着《Building Blockchain in Go - Part 1》把一个“最小可运行区块链”从零实现到能新增区块。
    
-   参考《Web3 实习手册：Smart Contract Development》梳理智能合约开发的学习路线与工程全链路（合约/前端/测试/部署）。
    

* * *

### 2）Go 手写区块链：我今天具体做了什么

我实现的目标不是“功能齐全”，而是把区块链最核心的两点做出来：**链式结构**与**哈希链接**。

**（1）Block 的最小结构**  
理解到一个区块至少需要：

-   `Timestamp`：时间戳
    
-   `Data`：交易或任意数据（Part 1 先简化成字符串）
    
-   `PrevBlockHash`：上一个区块的 hash
    
-   `Hash`：本区块 hash
    

**（2）Hash 的生成逻辑（核心）**  
今天最大的收获是：区块的 hash 本质上就是对 `PrevHash + Data + Timestamp` 做哈希，让每个区块“绑定”前一个区块，从而形成不可轻易篡改的链式结构。  
（后面如果改了旧区块内容，后续区块 hash 全都要变。）

**（3）Blockchain 最小实现**

-   先创建 Genesis Block（创世块）
    
-   `AddBlock(data)`：取链尾的 hash 作为 PrevHash，构造新区块并 append  
    这一步跑通后，我可以连续加多个区块，链结构是完整的。
    

* * *

### 3）智能合约开发：我今天梳理了哪些要点

看完 Web3 实习手册的“合约开发”部分，我把学习路线拆成了“能落地”的步骤：

**（1）合约开发不只是写 Solidity**  
一个完整 DApp 至少包含：

-   合约：状态 + 权限控制 + 事件（events）
    
-   前端：用钱包 provider / RPC 读链、发交易
    
-   测试：单测/集成测试（尤其是边界条件、权限、重入等）
    
-   部署：脚本化部署、多网络配置、环境变量管理
    

**（2）我今天重新建立的工程直觉**  
链上适合做“状态变更与验证”，但不适合做复杂查询；现实中很多列表/历史记录依赖事件 + 索引服务（indexer）去做读性能。这对后面做产品非常关键。

* * *

### 4）今日总结

-   手写区块链让我更清楚“区块链到底在保证什么”：用 hash 链接保证历史难以篡改。
    
-   合约开发路线让我更清楚“Web3 工程到底在做什么”：合约只是核心之一，测试/部署/前端交互同样重要。
    
-   今天整体偏“搭骨架 + 建认知”，后面需要继续补交易结构、持久化、共识/挖矿（PoW）等更完整模块。
    

* * *

### 5）明日计划

-   Go 链：加入 Transaction 结构（即使先简化），并尝试把区块序列持久化（boltDB/本地文件）。
    
-   合约：选一个简单合约（如 ERC20/Counter），用 Hardhat/Foundry 跑通：编译→测试→部署→前端调用 的最小闭环。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->









今天继续学 Uniswap V2，但这次重点不是“会用”，而是把“DEX 买入 → CEX 卖出”的套利模型推到一个可算的最优交易量，并理解它为什么能有闭式解、现实里又为什么经常不成立。

### 1）先把变量用文字说清楚

-   DEX（V2 池子）里有两种储备：base 储备 Rb、quote 储备 Rq
    
-   CEX（比如 Binance）给一个最优买价 P（你把 base 卖给它的价格）
    
-   V2 手续费用一个折扣因子 gamma 表示（常见 0.997），意思是你投入的 quote 只有 gamma 部分真正进入池子
    
-   CEX 手续费是 f，卖出后实际拿到的钱是 (1 - f) 倍
    

今天要解决的问题很明确：**在 DEX 买多少 base（记 y），拿去 CEX 卖，利润最大？**

* * *

### 2）为什么能出现“根号解析解”

闭式解能出来不是因为市场简单，而是因为做了两个很强的假设：

1.  CEX 只吃一档深度  
    也就是说你卖出去的 y 个 base 都按同一个价格 P 成交，P 不随数量变化。这样 CEX 那条腿的收益是线性的：卖多少就按比例收多少钱。
    
2.  DEX 是单一 Uniswap V2 常乘池  
    池子的交换关系是一个光滑的函数（可导），因此“利润 = 卖出回款 - 买入成本”也变成一个可导的一元函数。  
    于是就可以直接对 y 求导，令导数等于 0，解出最优点。这个过程中会出现平方项，所以最后会出现根号。
    

现实里一旦变成：多档 orderbook、V3 分 tick、跨池路由、同块竞争导致储备变化……这些假设就不成立，闭式解也就没那么好用了，只能数值法/模拟。

* * *

### 3）今天推到的关键关系（用文字/分式表达）

我今天把“为了在 V2 池子里买到 y 个 base，需要投入多少 quote（记 x）”推出来了：

-   结论是：x 和 y 的关系不是线性的，而是一个分式：  
    x(y) = (Rq _y) / (gamma_ (Rb - y))
    

直觉上也合理：

-   y 越接近 Rb（把池子快买空了），分母越小，成本会爆炸式上升
    
-   所以“看起来有价差”并不代表你能买很多，一买多滑点就把利润吃掉
    

* * *

### 4）利润函数怎么写（无公式版）

利润 = “CEX 卖出净回款” - “DEX 买入成本”

-   CEX 卖出 y 个 base：净回款 = (1 - f) _P_ y
    
-   DEX 买入 y 个 base：成本 = x(y) = (Rq _y) / (gamma_ (Rb - y))
    

所以利润就是上面两项相减。

然后对 y 做一阶最优条件（求导=0）可以解出最优买入量 y\*。最终得到的结果是：

-   y\* = Rb - sqrt( (Rb _Rq) / (gamma_ (1 - f) \* P) )
    

（这就是截图里那种“根号形式”的本体，只是它把符号换成了 base\_reserves/quote\_reserves/best\_bid 等。）

* * *

### 5）今天最大的收获：套利机会为什么“看起来有但赚不到”

我今天的理解是：在热门市场里，价格差通常很小，而 V2 的成本曲线对交易量非常敏感，所以“理论最优 y\*”会被压得很小，利润也会变得很薄。

另外现实还要加一个硬约束：

-   CEX 顶层深度有限（只能卖 Q\_top 那么多 base），所以真正能做的是 min(y\*, Q\_top)，再加上必须 y\*>0 才值得做。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->










今天主要学习的问题是：**在考虑滑点和手续费的情况下，如何在 Uniswap V2 买入并去 Binance 卖出，以实现单笔利润最大化？**

**1\. 市场环境假设**

-   **路径**：USDT (Quote) $\\to$ ETH (Base) \[Uniswap\] $\\to$ USDT \[Binance\]。
    
-   **CEX 假设**：只吃第一档深度（Level 1），认为回款是线性的。
    
-   **DEX 假设**：标准的 Constant Product Market Maker (CPMM)。
    

**2\. 核心变量**

| 符号 | 含义 | 备注 |
| $R_b$ | DEX Base 储备 (如 ETH) | 随交易减少 |
| $R_q$ | DEX Quote 储备 (如 USDT) | 随交易增加 |
| $\gamma$ | Uniswap 手续费因子 | 固定为 $0.997$ (0.3% fee) |
| $P$ | Binance 最佳买价 (Bid) | 假设为固定值 |
| $f$ | Binance Taker 费率 | 如 0.001 |
| $C_{gas}$ | 预估 Gas 费用 | Quote 本位 (新增变量) |
| $y$ | DEX 买入 Base 数量 | 待求的优化变量 |

* * *

### 二、推导过程 (Core Derivation)

1\. DEX 的成本函数

根据 $x \\cdot y = k$ 模型，买入 $y$ 个 Base 需要投入的 Quote 数量 $x(y)$ 为：

$$(R\_b - y)(R\_q + \\gamma x) = R\_b R\_q$$

整理得成本函数：

$$Cost\_{dex}(y) = x(y) = \\frac{R\_q \\cdot y}{\\gamma(R\_b - y)}$$

2\. 利润函数构建

总利润 = CEX回款 - DEX成本 - 固定Gas费

$$\\Pi(y) = (1-f)Py - \\frac{R\_q y}{\\gamma(R\_b - y)} - C\_{gas}$$

3\. 求解最优输入量 $y^\*$

对 $y$ 求导并令导数为 0（寻找驻点）：

$$\\frac{d\\Pi}{dy} = (1-f)P - \\frac{R\_q R\_b}{\\gamma(R\_b - y)^2} - 0 = 0$$

> **关键发现**：常数项 $C\_{gas}$ 的导数为 0。这意味着 **Gas 费的高低不改变最优下单量 $y^\*$ 的大小**，只改变是否开单的门槛。

解方程 $(R\_b - y)^2 = \\frac{R\_q R\_b}{\\gamma(1-f)P}$，得到理论最优解：

$$y^\* = R\_b - \\sqrt{\\frac{R\_q R\_b}{\\gamma (1-f)P}}$$

* * *

### 三、工程落地逻辑 (Implementation)

在实盘 Bot 中，不能直接盲目使用 $y^\*$，必须经过“截断”和“盈亏检查”。

Step 1: 理论计算

计算 $y^\*$。

-   若 $\\gamma(1-f)P \\le \\frac{R\_q}{R\_b}$（即 CEX 净价低于 DEX 边际价格），公式根号内无实数解或算出的 $y^\*$ 为负，直接 `Return`。
    

Step 2: 深度截断 (Clipping)

获取 CEX 的盘口量 $Q\_{top}$。

$$y\_{trade} = \\min(y^\*, Q\_{top})$$

(防止买太多导致在 CEX 砸穿第一档价格)

Step 3: 最终盈亏检查 (Gatekeeping)

这是 $C\_{gas}$ 发挥作用的地方。计算实际净利润：

$$\\Pi\_{net} = \[(1-f)P \\cdot y\_{trade}\] - \[\\text{DEX\\\_Cost}(y\_{trade})\] - C\_{gas}$$
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->











在前几天已经分别学习了 Uniswap V2 的 AMM 定价公式、Pair 合约中的 swap / mint / burn 逻辑后，今天主要把零散的点串起来，重点放在 **Router 在整个交易流程中的作用，以及一次 swap 从用户到 Pair 的完整路径**。

### 一、从“用户视角”重新理解 Uniswap V2

之前更多是从 Pair 合约出发看逻辑，但今天尝试从**用户发起一笔交易**开始理解整个流程。  
实际使用中，用户并不会直接调用 Pair，而是通过 Router 统一入口，例如：

```
swapExactTokensForTokens(
    uint amountIn,
    uint amountOutMin,
    address[] calldata path,
    address to,
    uint deadline
)
```

今天比较清楚的一点是：  
**Router 并不持有资金，也不决定价格，它只是负责“算路径 + 组织调用”**。

* * *

### 二、Router 中的路径与数量计算

在真正执行 swap 之前，Router 会根据 `path` 调用 `getAmountsOut`，逐跳计算每一跳的输出数量：

```
amounts[i + 1] = getAmountOut(
    amounts[i],
    reserveIn,
    reserveOut
);
```

这一步让我意识到：

-   多跳 swap 本质是多个 Pair 的串联
    
-   每一跳都会受到当前池子储备的影响
    
-   最终滑点是逐跳累积的结果
    

所以多跳路径并不是“免费优化”，而是在**流动性深度和路径长度之间做权衡**。

* * *

### 三、真正执行 swap 时发生了什么

在 `_swap` 中，Router 会按顺序调用每一个 Pair 的 `swap`，并把输出 token 直接转给下一个 Pair：

```
pair.swap(amount0Out, amount1Out, nextTo, new bytes(0));
```

这让我更直观地理解到：

-   Uniswap V2 并没有“全局状态”
    
-   每个 Pair 只关心自己的储备是否满足 K 约束
    
-   Router 只是负责把这些局部操作拼成一次完整交易
    

* * *

### 四、对设计边界的认识

通过今天的学习，也更清楚 Uniswap V2 的一些限制：

-   Router 提供的是**使用层面的安全性**，而不是价格保护
    
-   滑点、MEV、抢跑并不是 V2 能彻底解决的问题
    
-   这套设计在简洁性和可组合性上做了明显取舍
    

* * *

### 五、今日总结

-   Router 是“调度者”，Pair 才是核心执行者
    
-   多跳 swap 是多个独立 AMM 的组合
    
-   Uniswap V2 的设计非常克制，功能边界清晰
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->












### 一、Uniswap V2（继续学习）

在前两天理解了 Uniswap V2 的 swap 和 LP 机制后，今天主要补了一点 **Router 的作用**，搞清楚用户为什么几乎不直接和 Pair 合约交互。

Router 本质上负责：

-   计算最优路径
    
-   支持多跳 swap
    
-   在交易前做基本的参数校验（如最小接收数量）
    

```
function swapExactTokensForTokens(
    uint amountIn,
    uint amountOutMin,
    address[] calldata path,
    address to,
    uint deadline
)
```

今天主要理解了 `amountOutMin` 和 `deadline` 的作用，它们是**防止滑点和被抢跑**的基础手段，但并不能完全避免 MEV。

* * *

### 二、Web3 安全（继续）

在昨天的基础上，今天更关注 **DeFi 合约本身的攻击面**。

结合 AMM 场景，比较典型的风险包括：

-   重入或状态更新顺序问题
    
-   闪电贷配合价格操纵
    
-   依赖外部价格或瞬时状态的逻辑
    

虽然 Uniswap V2 本身比较安全，但**围绕 AMM 构建的协议**（如借贷、收益策略）更容易出问题。

同时也意识到，很多攻击并不是“发现一个 bug 就结束”，而是**组合多个设计缺陷**完成的。

* * *

### 三、AI 与 Web3 安全（今天新增）

今天额外接触了一点 **AI 在安全中的应用**，主要是概念层面：

-   攻击者可以用 AI 自动扫描合约中的潜在漏洞
    
-   利用 AI 更高效地寻找边界条件和异常路径
    
-   在社会工程中，用 AI 伪造更真实的身份和沟通内容
    

这让我感觉未来的攻击门槛可能会进一步降低，对防守方要求更高。

* * *

### 四、今日总结

-   Router 提供的是“安全下限”，而不是完全防护
    
-   DeFi 攻击更多来自组合风险，而非单点漏洞
    
-   AI 可能会放大 Web3 安全中的攻防不对称
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->













### 一、Uniswap V2（继续学习）

在前一天理解了 Uniswap V2 的 swap 基本流程后，今天继续学习了 **LP 机制和手续费在池子中的体现方式**。  
重点看了 `mint` 和 `burn` 的逻辑，理解 LP Token 本质上代表的是对池子中两种资产的**比例所有权**。

```
liquidity = Math.min(
    amount0 * totalSupply / reserve0,
    amount1 * totalSupply / reserve1
);
```

这段代码让我意识到，加池子并不是“随便存币就能赚手续费”，而是必须严格按当前储备比例，否则多出来的资产并不会提升 LP 份额。  
同时也理解了 Uniswap V2 的手续费并不会单独结算，而是通过储备的增长体现在 LP Token 的价值上。

* * *

### 二、Web3 安全（今日主要内容）

今天在 Web3 安全上花的时间更多，主要通过**真实攻击事件**来总结风险来源。

首先是 **CeFi 安全问题**。  
很多损失并不是合约漏洞，而是来自：

-   管理员私钥泄露
    
-   热钱包权限过大
    
-   运维账号被钓鱼或社工攻击
    

一旦中心化系统被突破，往往是直接的大额损失，恢复空间很小。

然后是 **DeFi 安全风险**。  
DeFi 表面上是“代码即规则”，但实际问题更多来自：

-   合约逻辑边界没考虑全面
    
-   新语言、新框架（如 Move）生态不成熟
    
-   老协议在新增功能后引入新漏洞
    

即使是运行多年的协议，也可能被重新挖出问题。

* * *

### 三、攻击方式的变化

比较明显的一个趋势是，攻击已经不只是“技术对抗”，而是**技术 + 社会工程**的结合，比如：

-   伪装成 Web3 / AI 公司进行长期钓鱼
    
-   针对高价值个人或机构精准下手
    
-   资金被盗后迅速进入自动化洗钱流程
    

这让我感觉 Web3 安全不仅是写安全合约的问题，还涉及人员、权限和流程设计。

* * *

### 四、今日总结

-   Uniswap V2 的设计在机制上很严谨，但安全依然依赖正确使用
    
-   CeFi 风险集中在“人”，DeFi 风险集中在“代码和复杂性”
    
-   安全更多是系统工程，而不是单点技术问题
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->














今天主要学习了 **Uniswap V2 的基础 AMM 机制**，重点放在 **swap 定价公式和 Pair 合约的核心逻辑**，没有深入到全部源码。

### 1\. AMM 定价公式（今天重点）

Uniswap V2 使用恒定乘积模型：

x⋅y=k

在实际合约中会扣除 0.3% 手续费，对应代码逻辑如下：

```
uint amountInWithFee = amountIn * 997;
uint amountOut = (amountInWithFee * reserveOut) /
    (reserveIn * 1000 + amountInWithFee);
```

今天主要理解了：

-   为什么要用 `997 / 1000`
    
-   滑点是由 `reserveIn` 和 `reserveOut` 决定的
    

* * *

### 2\. Pair 合约中 swap 的基本流程

简单看了 `UniswapV2Pair.swap`，理解了整体流程：

```
// 转出 token
_safeTransfer(tokenOut, to, amountOut);

// 通过余额反推用户实际转入的 amountIn
uint amountIn = balanceAfter > reserveBefore
    ? balanceAfter - reserveBefore
    : 0;

// 校验恒定乘积（含手续费）
require(balance0Adjusted * balance1Adjusted >= k, "K");
```

基本逻辑是**先转账，再通过余额变化判断输入量的**，而不是参数直接给 amountIn。

* * *

### 3\. 简单总结

-   Uniswap V2 的核心就是 `x*y=k + 手续费`
    
-   swap 的安全性主要靠最终的 K 校验保证
    
-   Pair 合约是真正持币和执行逻辑的地方
    

明天打算继续看 LP 的 mint / burn。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->















按照 Web3Career 冬季实习计划的要求，我已于 **2026 年 1 月 12 日** 将官方活动日程导入 **Google Calendar**，用于个人时间管理和后续活动提醒。  
该操作主要是为了更好地对齐项目整体安排，明确即将参与的活动时间，并提前规划个人学习节奏。

* * *

### 今日完成的学习内容

今日我实际完成了以下学习内容：

-   参与 Web3 相关的线上会议与分享
    
-   阅读并记录 ETH 中文周报的相关内容
    

在学习过程中，重点关注了以下主题：

-   Ethereum 及其 Layer 2（如 Arbitrum、Optimism）的最新进展
    
-   Web3 项目中的治理机制与激励设计案例
    
-   合约安全事件及相关历史风险分析
    
-   隐私项目与基础设施方向（如 Zcash、Starknet）
    
-   Vitalik 关于稳定币与去中心化发展的观点
    

* * *

### 今日个人收获（当日感受）

通过今天的学习与整理，我对 Web3 行业有了更加清晰的认知：

-   Web3 行业信息更新频繁，需要通过周报等方式进行持续、系统的跟踪
    
-   技术演进、治理决策和安全事件会共同影响项目的长期发展
    
-   在实习阶段，应尽早养成主动记录和总结的学习习惯，为后续深入参与打下基础
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
