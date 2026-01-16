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
