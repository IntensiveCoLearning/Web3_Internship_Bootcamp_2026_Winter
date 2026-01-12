---
timezone: UTC+8
---

# Quinn

**GitHub ID:** baikingrio

**Telegram:** 

## Self-introduction

巩固加强Web3技术开发能力和拓展知识面。

## Notes

<!-- Content_START -->
# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->
![WeChat509bc988ca05c9df2227c761fde707a1.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/baikingrio/images/2026-01-12-1768183404462-WeChat509bc988ca05c9df2227c761fde707a1.jpg)

今天的计划是详细阅读UniswapV2文档，在V1版本上进行了很大的变化。

| 核心特性 | 技术要点说明 | 主要意义/改进 |
| --- | --- | --- |
| 任意 ERC-20 ↔ ERC-20 | 不再强制通过 ETH 中转，可直接创建任意两种 ERC-20 的交易对 | 极大提升灵活性，减少滑点和 gas 消耗 |
| 定价公式 | x × y = k（恒定乘积做市商） | 简单、数学上永不破产（但存在无常损失） |
| 交易手续费 | 0.3%（全部给 LP），可选开启 0.05% 协议费（LP 得 0.25%） | 协议费开关写死在合约里，基本不可改 |
| 闪电兑换（Flash Swap） | 先拿走想要的 token，同一交易内必须归还（可还另一种 token 等值） | 开启套利、清算、抵押品拍卖等高级用法 |
| TWAP 价格预言机 | 每个区块开始时记录当时池子价格，累加成 cumulative price（uint112） | 抗短期操纵，为链上其他协议提供相对安全价格 |
| 合约结构 | Factory + Pair 双层结构，核心合约不可升级，极简设计 | 经典且安全，被最多项目直接复制 |
| 重要安全设计 | - 强制检查 balance 防 donation attack - 烧毁 1000 份最小流动性 - sync()/skim() 逃生通道 | 修复了 V1 很多工程上的危险边界情况 |
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
