---
timezone: UTC+8
---

# NingLin Liu

**GitHub ID:** ninglinLiu

**Telegram:** @duanqiangushifei

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
今天Web3运营学习经验总结（2026年1月13日）

作为Web3 builder，我的运营重点围绕AI+Web3项目（如AI狼人杀竞技场带NFT角色），今天的学习主要从社交平台增长、社区构建和变现路径入手。基于我们讨论的策略和AI产品建议，以下是关键经验提炼，强调实战性和低成本执行。核心逻辑：Web3运营不是烧钱，而是通过价值互动构建忠实社区，转化成可持续收入。1. 粉丝增长起步：从小基数（40粉丝）到1k的实战框架

-   第一周重点是高强度互动而非盲目发帖。优化Profile突出项目（如Bio加“Building AI Werewolf Arena with NFT Roles”），每天发1-2帖分享进度/价值（如项目更新或AI tips）。
    

回复大V策略：选AI/Web3领袖如@VitalikButerin、@AndrewYNg

，提供深度见解（e.g., 链接你的NFT项目补充他们的区块链/AI帖），目标吸他们的观众。

-   构建连接：DM同niche小号聊帖（e.g., “爱你的AI NFT想法，跟我的狼人杀项目类似，分享经验？”）；参与Spaces讨论Web3热点；跑抽奖帖（如“评论AI Web3想法，抽NFT角色”）增互动。
    
-   监控调整：用X Analytics追踪印象高帖，复制模式。经验：复合增长靠一致（周增50-100起步），Premium账号已解锁工具，无需升级。
    

2\. 跨平台扩展：LinkedIn作为B2B补充

-   LinkedIn适合Web3专业连接，优化Profile/Headline强调NFT/AI专长。每天1-2帖混价值分享和项目demo，互动评论大V帖建DM关系。
    
-   经验：Web3运营需多平台联动（X引流到LinkedIn），焦点thought leadership而非病毒。垂直niche（如Web3游戏）能快速到1k粉丝。
    

3\. AI产品运营启发：从验证到变现的Web3应用

-   卖空气（Smoke Test）：建假落地页测试NFT角色需求，别先写代码。适用于你的项目—先测用户是否点“Buy NFT Role”。
    
-   找吐槽用户：在Reddit/X挖掘Web3痛点（如NFT工具难用），用AI工具（如n8n雷达）自动化商机监控，转成运营机会。
    
-   转向Agentic Workflow：Web3产品别做聊天套壳，卖“结果”（e.g., AI自动生成NFT游戏角色），建护城河。
    
-   高价定价&垂直切入：按价值收（如省企业成本的10%），专注不性感行业（如游戏物流）。经验：看行动非口头，问用户过去为Web3问题付多少钱。
    
-   本地化隐私&ROI量化：B2B Web3卖安全感（本地AI跑NFT数据），产品介绍用数字（e.g., “快3倍生成NFT”）。
    
-   渠道验证优先：确保CAC < LTV，别假设好项目自带流量。先找Web3社区（如Discord）低成本获客。
    

整体经验反思

-   最大坑避免：Web3运营别伪创新，先验证需求/渠道。AI化一切（如用Claude分析评论加速调研），专注Outcome而非技术。
    
-   变现路径：到1k粉丝后，用X Subscriptions/Affiliate推NFT；1万后，品牌deal或付费社区。你的AI狼人杀是钩子—分享进度建忠诚。
    
-   行动建议：今天发第一帖总结这些（如我生成的推文模板），鼓励评论互动。坚持1-2小时/天，一周见效。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

## 今日 Web3 防钓鱼总结日志（D-Log）

**日期**：2026.1.12  
**链/环境**：Ethereum Sepolia（演示）→ 迁移到主网时风险极高  
**主题**：Permit / Approvals / Punycode 同形域名钓鱼

* * *

### 1) 今日学到的三个核心结论

**① 签名不是“无害确认”，它可能是“离线授权”。**  
Permit（EIP-2612）允许用签名直接授权 `spender`，攻击者拿到签名后可在链上 `permit → transferFrom`，用户不需要再发起交易或二次确认。

**② Approve 弹窗里最危险的不是 Gas，而是“Spend Limit”。**  
看到超大额度（常见为 `115792089…`，即 `2^256-1`）= “无限授权”。这不是质押，不是存入，而是把 USDC 的支出权交给对方合约。

**③ 域名长得像 ≠ 真域名。**  
Punycode / 同形字符可以做“视觉欺骗”，例如把某个字母替换成长得一样的 Unicode 字符，最终域名底层会变成 `xn--...`。肉眼看不出，但 DNS 完全不同。

* * *

### 2) 今日见到的典型钓鱼剧本（复盘）

**剧本 A：空投诱导 + Permit 签名**

-   页面看似“领取 UNI / 资格验证”
    
-   实际请求签名 Permit：`spender` 是攻击者，`value` 常为 MAX，`deadline` 设很久
    
-   结果：攻击者可在有效期内随时花光你钱包里的 USDC
    

**剧本 B：高收益质押诱导 + Approve 无限授权**

-   “USDC 质押池，日收益 50%”这类文案
    
-   钱包弹窗提示：approve spend limit
    
-   Spend limit 出现超大数（MAX）
    
-   结果：一旦主网同套路，授权后资金可被直接转走
    

**剧本 C：Punycode 假官网 + 复制 UI**

-   假域名视觉上与真官网几乎一致
    
-   用户误以为在官方页面操作
    
-   下一步永远是：Sign / Permit / Approve
    

* * *

### 3) 今日形成的“3 秒拒绝规则”（随手可用）

只要出现 **Sign / Approve / Permit**，立刻扫这 4 项：

-   **额度**：是不是 MAX / 超大数？（是 → 拒绝）
    
-   **对象**：spender/contract 地址你是否明确认识？（不认识 → 拒绝）
    
-   **动机**：是不是“空投/高收益/补贴/资格验证”？（是 → 拒绝）
    
-   **入口**：是不是邮件/Telegram/广告链接点进去的？（是 → 拒绝）
    

> 任何一条命中：**Reject + 关页面**。

* * *

### 4) 今日行动清单

-   主网钱包与交互钱包分离：交互钱包只放小额
    
-   只用书签/手输官方域名，不点外链
    
-   遇到 `xn--` 域名直接拉黑
    
-   将“授权额度默认改为本次用量”（能改就改）
    
-   每周一次检查授权：清掉不认识的 spender（approve 设 0 或 revoke）
    

* * *

### 5) 一句话心法（写给未来的自己）

> **交易是“我现在给你钱”，Permit/Approve 是“我允许你随时拿走钱”。**  
> **看名字没用，看合约地址才有用。**
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
