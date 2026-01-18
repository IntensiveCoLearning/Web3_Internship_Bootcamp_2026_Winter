---
timezone: UTC+8
---

# Plato333

**GitHub ID:** Plato333

**Telegram:** @Plato

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->
核心时间序列指标（按日/小时）

-   日活跃地址 (DAU)（交易/转账任何一次计一次）
    

```
SELECT
  DATE(block_timestamp) AS day,
  COUNT(DISTINCT from_address) AS dau
FROM `<PROJECT>.<DATASET>.transactions`
WHERE block_timestamp BETWEEN '2025-01-01' AND '2025-06-30'
  AND (to_address = '<TOKEN_CONTRACT>' OR from_address = '<TOKEN_CONTRACT>' OR EXISTS(...) )
GROUP BY day
ORDER BY day;
```

-   日交易量（token 级别，归一化 decimals）
    

```
SELECT
  DATE(block_timestamp) AS day,
  SUM(value / POW(10, decimals)) AS token_volume
FROM `<PROJECT>.<DATASET>.token_transfers`
WHERE token_address = '<TOKEN_CONTRACT>'
  AND block_timestamp BETWEEN '2025-01-01' AND '2025-06-30'
GROUP BY day
ORDER BY day;
```

-   持币分布（Top N holders 占比、Gini）
    

```
-- Top holders占比
SELECT
  holder_address,
  balance / total_supply AS share
FROM `<PROJECT>.<DATASET>.token_holders_snapshot`
WHERE token_address = '<TOKEN_CONTRACT>'
ORDER BY share DESC
LIMIT 20;
```

（Gini 可用 Python/pandas 计算）

流动性 & 池子指标

-   LP 总深度（USDT/WETH 计价）：
    

```
-- 需要 pair reserves表或通过 DEX subgraph 获取 reserves
SELECT
  DATE(block_timestamp) as day,
  AVG(reserve_usd) as avg_liquidity_usd
FROM `<PROJECT>.<DATASET>.pair_reserves`
WHERE pair_address = '<PAIR_ADDR>'
  AND block_timestamp BETWEEN ...
GROUP BY day
ORDER BY day;
```

资金流向 / 大户行为

-   大额转账监测（>=阈值）
    

```
SELECT block_timestamp, from_address, to_address, value/POW(10,decimals) as amt
FROM `<PROJECT>.<DATASET>.token_transfers`
WHERE token_address = '<TOKEN_CONTRACT>'
  AND value/POW(10,decimals) >= 10000 -- 大额阈值
ORDER BY block_timestamp DESC
LIMIT 200;
```

异常检测（短期 spike）

-   计算 z-score 或 rolling median + IQR 来识别 volume/DAU spike（用 Python/pandas）
    

Rug/风险信号（需要额外表）

-   查看 LP token 转移给非锁地址或销毁：
    

```
SELECT *
FROM `<PROJECT>.<DATASET>.token_transfers`
WHERE token_address = '<LP_TOKEN_ADDRESS>'
  AND to_address NOT IN ('<KNOWN_LOCK_CONTRACTS>', ...)
ORDER BY block_timestamp DESC;
```

说明：Dune/BigQuery 的表名会有差别，但思路一致。Dune 用户通常用 `erc20.events.Transfer`、`transactions`、`uniswapv2.pair_day_data` 等表修改成对应的平台即可。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->

**Etherscan**

-   **Approve**：某地址在代币合约上执行了 ERC20/BEP20 的 `approve(spender, amount)` 操作，意思是该地址允许某个 `spender`（通常是 DEX Router、合约或某个地址）代表它花指定数量的代币。大量 `Approve` 出现通常说明很多人/钱包在准备交易（例如准备在 DEX 上把这个代币卖出/买入，或是某些工具在批量授权）。
    
-   **Transfer / Token Transfer**：代币实际移动（转账、买入/卖出会产生 transfer log）。
    

-   **From / To**：谁发起、谁是目标（注意 Approve 的 To 一般是合约地址或 spender）。在你的截图 `To` 显示是合约地址（0x1646...87777），说明这些批准事件与这个合约有关。
    
-   **Txn Hash（交易哈希）**：点击它能看到交易的完整详情（方法名、输入参数、logs、gas、调用者地址、decoded input）。这是你要点开的第一步。
    
-   **Age / Block**：时间与区块，判断动作发生的节奏（短时间内大量操作是高频活动/群体行为的信号）。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->


阅读xiaohai老师写的手册
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->



备战期末，鸽一天hh
<!-- DAILY_CHECKIN_2026-01-14_END -->
<!-- Content_END -->
