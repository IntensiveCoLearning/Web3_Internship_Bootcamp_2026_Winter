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
