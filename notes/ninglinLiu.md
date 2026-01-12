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
