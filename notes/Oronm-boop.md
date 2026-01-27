---
timezone: UTC+8
---

# Self

**GitHub ID:** Oronm-boop

**Telegram:** @javabeanss

## Self-introduction

冬训营技术向学员，web2转web3 ing~

## Notes

<!-- Content_START -->
# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->
今天将前端与合约进行集成
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->

今天完成休闲黑客松的需求分析、UI设计
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->


打卡
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->



打卡
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->




打卡
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->





今天准备把黑客松项目部署上线
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->






今天学习anchor框架
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->








打卡
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->









做了一个solana前后端交互小demo：[Create Next App](https://solanacounter-iota.vercel.app/)
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->











今天学习web3.js
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->












学习质押项目
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->













今天学习了solana的**Pinocchio框架。**  
**Pinocchio vs Anchor：从 Anchor 开发者视角看 Pinocchio 代码**

本文档旨在帮助习惯了 Anchor 框架的开发者理解 Pinocchio 代码（以 `blueshift_vault` 为例）。

**核心区别**：Anchor 帮你自动处理了所有底层繁琐的工作（"魔法"），而在 Pinocchio 中，为了极致的性能和零依赖，所有事情都必须**手动完成**。

* * *

## **1\. 标准库与 Panic 处理**

### **Anchor**

默认开启 `std`，帮你处理内存分配和 Panic 捕获。你基本不需要关心程序挂了会怎么样，框架会返回错误码。

### **Pinocchio**

**代码对应**：

```rust
#![no_std] 
// ...
nostd_panic_handler!(); 
```

-   `#![no_std]`：必须禁用标准库，因为我们没有任何运行时支持。
    
-   `nostd_panic_handler!()`：必须手动定义“程序崩溃时该怎么办”。它告诉机器：如果代码出错了（Panic），请打印错误位置并立即停止运行 (`abort`)。
    

* * *

## **2\. 程序的入口点与 ID**

### **Anchor**

使用宏 `declare_id!("...");` 和 `#[program]` 属性宏自动处理路由。

### **Pinocchio**

**代码对应**：

```rust
entrypoint!(process_instruction);
pub const ID: Pubkey = [...];

fn process_instruction(...) { ... }
```

-   `entrypoint!`：手动声明入口。
    
-   `process_instruction`：Solana Runtime 会把所有原始数据（Program ID, Accounts 数组, Instruction Data 字节数组）塞给这个函数。你必须亲自处理这些原始数据。
    

* * *

## **3\. 指令路由 (Discriminator)**

### **Anchor**

自动根据函数名生成 Discriminator，自动匹配并路由到对应的函数（如 `fn deposit(...)`）。

### **Pinocchio**

**代码对应**：

```rust
match instruction_data.split_first() {
    Some((Deposit::DISCRIMINATOR, data)) => Deposit::try_from((data, accounts))?.process(),
    Some((Withdraw::DISCRIMINATOR, _)) => Withdraw::try_from(accounts)?.process(),
    _ => Err(ProgramError::InvalidInstructionData),
}
```

-   **手动路由**：你必须自己切分 `instruction_data` 的第一个字节（通常定为 Discriminator），然后写 `match` 语句决定调用哪个逻辑。
    

* * *

## **4\. 账户验证 (硬核部分)**

这是区别最大的地方。Anchor 的核心魔法 `#[derive(Accounts)]` 在这里变成了纯手动的 `TryFrom` 实现。

### **Anchor 写法**

```rust
#[derive(Accounts)]
pub struct Deposit<'info> {
    #[account(mut, signer)]
    pub owner: Signer<'info>,
    #[account(mut, seeds = [b"vault", owner.key.as_ref()], bump)]
    pub vault: Account<'info, Vault>, // 自动检查 PDA、Owner、数据结构等
    pub system_program: Program<'info, System>,
}
```

### **Pinocchio 写法**

**代码对应**：`DepositAccounts::try_from`

1.  **解析数组**：你拿到的是一个扁平的 `&[AccountInfo]` 数组，必须自己按顺序解构：
    
    ```rust
    let [owner, vault, _] = accounts else { ... }; // 必须清楚客户端传账户的顺序！
    ```
    
2.  **手动检查 Signer**：
    
    ```rust
    if !owner.is_signer() { ... } // 对应 Anchor 的 signer 约束
    ```
    
3.  **手动检查 Owner**：
    
    ```rust
    if vault.owner() != &pinocchio_system::ID { ... } // 对应 Anchor 的 owner 检查
    ```
    
4.  **手动计算并检查 PDA**：
    
    ```rust
    let (vault_key, _) = find_program_address(&[b"vault", owner.key()], &crate::ID);
    if vault.key().ne(&vault_key) { ... } // 必须手动对比地址
    ```
    
    > **注意**：Anchor 帮你自动计算 PDA 并验证传入是否正确。在这里，你必须自己算一遍，然后跟传入的账号对比。如果不对比，黑客就可以传入一个假账号来骗过程序。
    

* * *

## **5\. 参数反序列化**

### **Anchor**

使用 Borsh 自动序列化/反序列化，参数直接出现在函数签名里 `fn deposit(ctx, amount: u64)`。

### **Pinocchio**

**代码对应**：

```rust
fn try_from(data: &'a [u8]) -> Result<Self, Self::Error> {
    // ...
    let amount = u64::from_le_bytes(data.try_into().unwrap());
    // ...
}
```

-   **字节操作**：为了省 Compute Unit，通常直接读字节（如 `from_le_bytes`）。不做复杂的序列化，零拷贝。
    

* * *

## **6\. 业务逻辑与 CPI**

### **Anchor**

封装了 `CpiContext`，调用其他程序像调用本地函数一样简单：

```rust
anchor_lang::system_program::transfer(cpi_ctx, amount)
```

### **Pinocchio**

**代码对应**：

```rust
Transfer {
    from: self.accounts.owner,
    to: self.accounts.vault,
    lamports: self.instruction_data.amount,
}
.invoke()?;
```

-   **Invoke**：你得构建类似底层的指令结构体，然后调用 `.invoke()`。
    
-   **PDA 签名**：如果是 PDA 签名，得用 `.invoke_signed(&signers)`，并且必须手动构建 `seeds` 数组。
    

* * *

## **总结**

| 概念 | Anchor | Pinocchio |
| --- | --- | --- |
| 开销 | 大（包含大量检查代码，二进制体积大） | 极小（只包含你写的代码，二进制体积极小） |
| 账户验证 | #[derive(Accounts)] 声明式 | 手动 if 判断 过程式 |
| 数据解析 | Borsh 自动 | 字节操作 手动 (Zero Copy) |
| 安全性 | 框架帮你兜底，不容易出错 | 全靠你自己（漏写一个 check 就可能有漏洞） |

Pinocchio 虽然写起来繁琐，但它赋予了你对每一个字节、每一个指令的完全控制权。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->














打卡
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->















今天了解一下solana公链，学习rust语言
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
















### **一、 2025年 Web3 安全概况与趋势**

-   **安全数据**：GoPlus 内部记录了约 1200 起安全事件（B 端项目方与 C 端用户各占约一半），造成的总经济损失超过 35 亿美元 4。  
      
    
-   **两大主要攻击趋势**：
    

1.  **精准猎杀 (Precision Hunting)**：针对交易所（如 Bybit）、高 TVL 协议进行长期布局和精密攻击，常涉及国家级黑客（如朝鲜黑客 Lazarus，2025年盗取约 20 亿美元） 5。  
      
    
2.  **广撒网 (Broadcasting)**：针对普通用户，通过钓鱼网站、Rug Pull（跑路盘）、貔貅盘等方式进行大规模攻击 6。  
      
    

### **二、 典型攻击案例解析**

会议重点分析了 2025 年发生的几起具有代表性的安全事件：

-   **Bybit 事件（损失 15 亿美元）**：黑客渗透了多签服务提供商 Safe 的前端开发人员，篡改了前端界面。管理人员在进行资金调拨时，界面显示正常但背后交易已被恶意篡改，最终导致巨额损失 7。  
      
    
-   **Sonne Finance（损失 2.23 亿美元）**：智能合约漏洞被利用。由于建立了较好的应急响应机制，成功冻结并恢复了部分资金，实际损失控制在 6000 万美元左右 8。  
      
    
-   **Balancer**：老牌协议被黑客利用 AI 技术挖掘出深层漏洞 9。  
      
    
-   **UXLINK 及其他社工攻击**：黑客（多为朝鲜黑客）利用 Deepfake 技术伪造合作伙伴或投资人，通过 Zoom 等视频会议诱导受害者下载带毒软件或点击恶意链接 10。  
      
    

### **三、 新技术带来的新风险**

-   **AI 与社会工程学结合**：利用 AI 深度伪造（Deepfake）视频和声音，导致数字身份信任体系崩塌。黑客伪装成面试官、投资人进行诈骗 11。  
      
    
-   **以太坊 7702 升级风险**：该技术赋予链上地址智能合约能力。黑客利用此特性设计新型钓鱼，诱导用户签署恶意的 delegate（委托），从而控制用户地址自动转走资产，甚至利用被控地址进行二次钓鱼 12。  
      
    
-   **软件供应链攻击 (AI Coding)**：在 AI 辅助编程时代，黑客可能通过 AI 生成的代码或开发插件植入后门（例如 Trust Wallet 浏览器插件曾被植入恶意代码） 13。  
      
    
-   **地址投毒 (Address Poisoning)**：黑客向用户发送小额代币，生成与用户常用转账对象首尾相同但中间不同的伪造地址，诱导用户在复制历史记录时转错账 14。  
      
    

### **四、 安全防范建议**

**1\. C 端用户建议：安全“四步大法”**

-   **不点**：不轻易点击不明链接（私信、会议链接等），养成交叉验证习惯 15。  
      
    
-   **不签**：拒绝盲签，必须看懂钱包弹出的签名内容（金额、交互对象、操作类型）再确认 16。  
      
    
-   **不装**：不随意安装软件，务必从官方渠道下载应用，警惕搜索引擎广告位的假冒 App 17。  
      
    
-   **不转**：转账前仔细核对完整地址，不轻信高收益理财或虚假矿池 18。  
      
    
-   **其他**：做好冷热钱包分离，妥善保管私钥/助记词 19。  
      
    

**2\. B 端开发者/项目方建议**

-   **全生命周期管理**：开发阶段引入安全库（如 OpenZeppelin），上线前进行审计 20。  
      
    
-   **警惕供应链攻击**：在使用 AI 辅助写代码或安装插件时需人工确认，防止恶意代码植入 21。  
      
    

**应急响应机制**：提前建立完善的应急响应流程，以便在攻击发生时能及时冻结资金止损 22。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


















今日学习go-eth链下交互，学习使用foundry框架
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




















学习内容:

1.下载metamask，领水，互转sepolia测试代币

2.观看每周简报WeeklyBriefing，学习到了以太坊的一些前沿资讯，行业发展情况

3.观看Web3 行业全局介绍，总结如下：

传统pdf简历不适用web3，海投简历效果差，更看重链上履历，热门赛道有RWA、Depin、AI+web3**，**初级人才过剩**，**缺高级复合型人才，solidity是基础，还可以掌握一下rust、move等语言，提升自己的竞争力，选一个链深耕，比如 ehterum 或者 solana，web3是一个国际化的领域，需要练好英语口语，经营自己的X，参与Space、Snapshot投票、GitHub绿格子（代码提交记录）提升自己的影响力，应届生最好去大公司，最大程度上避免踩坑，web3工作一般不发社保，小公司一般发U，找工作时需要仔细甄别公司的资质和安全性。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
