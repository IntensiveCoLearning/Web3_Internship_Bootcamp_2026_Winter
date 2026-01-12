---
timezone: UTC+8
---

# Twooweeks

**GitHub ID:** Twooweeks

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->
-   什么是base费：每单位 Gas 的“协议强制最低价：由协议根据网络拥堵程度自动计算出来，所有交易都必须至少支付这部分费用；它不奖励给打包者/验证者，而是直接被销毁
    
-   为什么要摧毁base费，有三个原因：1) 防止验证者操纵“基础费”，让费用市场更干净，验证者只拿tip,激励单一，博弈更少；2）2) 锁定 ETH 的“货币属性”：只能用 ETH 付网络使用费，eth网络会一直消耗eth。3) 经济层面：用销毁对冲发行，降低通胀甚至形成通缩；
    
-   base费若给验证者博弈场景，验证者 A：故意少打包交易 → 当前区块偏空
    

让下一个区块 baseFee 降然后：在后续区块中多打包交易，从 baseFee 中获取更多收益

或者反过来：人为制造拥堵 → 推高 baseFee → 自己吃 baseFee 收益

-   什么是blob数据：简单理解主要是 Rollup 的批量交易数据/压缩后的 L2 数据，是一个**临时保险柜。**
    
-   **叔节点的奖励：挖矿奖励是每个区块3个以太币，叔节点的奖励如下：**
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Twooweeks/images/2026-01-12-1768228753545-image.png)

只有**两个**叔节点能得到奖励，且叔节点是可以和爷爷节点同一个级别的，但由于叔节点的奖励是由**后代决定**的，也还是容易造成不公平分配。后代隔离的叔父倍数最多是**6倍**。叔父和后代要有共同的祖先(unclde reward)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Twooweeks/images/2026-01-12-1768228848240-image.png)

-   \- 叔父区块得不到gas费
    
    \- 包含叔父区块，但并不执行叔父区块里面的交易，叔父区块和父区块的交易会产生冲突
    
    \- 包含叔父区块，要检查区块是否是合法区块，是符合难度时挖出来的
    
    \- 叔父区块屁股的区块是得不到奖励的，要是有奖励的话会使分叉攻击的风险下降(就算不是最长链也能得到一点奖励)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
