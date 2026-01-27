---
timezone: UTC+8
---

# huanglaifa0-sys

**GitHub ID:** huanglaifa0-sys

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->
**传统预算系统 = 人治流程 + 事后审计**  
**Web3 预算系统 = 代码治理 + 过程即审计**

核心差异不在“链不链”，而在**控制权放在哪里**。

* * *

# 二、传统预算流程（标准形态）

我们先明确一个典型流程（不管是企业、基金会还是项目制）：

1.  **预算制定**
    
2.  **预算审批**
    
3.  **预算下达 / 拨付**
    
4.  **预算执行**
    
5.  **预算监控**
    
6.  **预算调整**
    
7.  **预算审计 / 复盘**
    

下面逐项翻译。

* * *

# 三、逐步翻译为 Web3 架构（核心部分）

* * *

## ① 预算制定 →【提案系统（Proposal Layer）】

### 传统做法

-   各部门提交预算表
    
-   财务/管理层汇总
    
-   多轮博弈、拍脑袋、经验判断
    

### Web3 翻译

**预算 = 链上提案（Budget Proposal）**

**实现方式：**

-   每个预算项目 = 一个 Proposal
    
-   内容包括：
    
    -   请求金额
        
    -   使用周期
        
    -   Milestone（里程碑）
        
    -   交付标准
        

**工具 / 机制：**

-   Snapshot（链下投票，低成本）
    
-   或链上 Proposal 合约
    

👉 **变化本质**：  
预算不再是“填表”，而是**公开论证的资源申请**。

* * *

## ② 预算审批 →【治理机制（Governance Layer）】

### 传统做法

-   管委会 / 领导审批
    
-   权力高度集中
    
-   结果不透明
    

### Web3 翻译

**预算审批 = 治理决策**

**可选治理模型：**

-   Token-weighted vote（资本治理）
    
-   Delegate vote（代议制）
    
-   Reputation / SBT（声誉治理）
    
-   多签委员会（小规模组织）
    

**关键变化：**

-   谁能投票 = 谁拥有治理权
    
-   审批逻辑 = 事先公开
    
-   投票结果 = 可验证
    

👉 **审批不再是“拍板”，而是“共识形成”**

* * *

## ③ 预算下达 / 拨付 →【金库合约（Treasury Layer）】

### 传统做法

-   财务打款
    
-   一次性 / 分批
    
-   人工控制节奏
    

### Web3 翻译

**预算拨付 = 智能合约执行**

**实现方式：**

-   DAO Treasury 合约
    
-   项目子钱包 / 合约账户
    

**高级玩法：**

-   按 Milestone 自动解锁
    
-   超时未完成 → 自动冻结
    
-   多签触发释放
    

👉 **钱不是“批出来的”，是“条件触发的”**

* * *

## ④ 预算执行 →【执行即状态变更（Execution Layer）】

### 传统做法

-   项目自己花钱
    
-   报销、对账
    
-   财务追着要材料
    

### Web3 翻译

**执行 = 链上状态变化**

表现为：

-   钱从 Treasury → 执行地址
    
-   合约记录剩余额度
    
-   所有转账可追溯
    

**关键差异：**

-   没有“执行黑箱”
    
-   没有“补材料”
    
-   没有“解释成本”
    

👉 **执行 = 自动留下证据**

* * *

## ⑤ 预算监控 →【实时可视化（Transparency Layer）】

### 传统做法

-   月报 / 季报
    
-   Excel + PPT
    
-   信息严重滞后
    

### Web3 翻译

**监控 = 链上实时状态**

实现：

-   Dashboard（Dune / 自建）
    
-   实时显示：
    
    -   已用 / 剩余
        
    -   资金流向
        
    -   项目进度
        

👉 **监控从“汇报”变成“查看”**

* * *

## ⑥ 预算调整 →【参数治理（Amendment Layer）】

### 传统做法

-   追加预算
    
-   特批流程
    
-   灰色空间大
    

### Web3 翻译

**预算调整 = 再次治理**

方式：

-   新 Proposal
    
-   修改合约参数
    
-   执行需再次共识
    

**重要意义：**

-   所有“例外”都会留下痕迹
    
-   没有无痕特权
    

👉 **例外被制度化，而不是人情化**

* * *

## ⑦ 预算审计 / 复盘 →【事后分析 + 可追溯账本】

### 传统做法

-   审计报告
    
-   高成本、低频率
    
-   很多问题无法纠正
    

### Web3 翻译

**审计 = 原生属性**

-   不可篡改账本
    
-   全流程可追溯
    
-   审计成本接近 0
    

还能进一步：

-   Retroactive Funding（事后奖励）
    
-   调整未来预算权重
    

👉 **审计不再是“追责工具”，而是“学习机制”**

* * *

# 四、完整对照表（压缩版）

| 传统预算 | Web3 架构 |
| --- | --- |
| 预算表 | Proposal |
| 审批会 | 治理投票 |
| 财务打款 | Treasury 合约 |
| 报销 | 链上转账 |
| 月报 | Dashboard |
| 特批 | 再治理 |
| 审计 | 原生可追溯 |

* * *

# 五、最重要的一句“翻译原则”（请记住）

> **传统预算控制“人”，  
> Web3 预算控制“规则”。**

* * *

# 六、结合你背景的关键提醒（非常重要）

以你关心的 **链下行为 × 链上确权** 来看：

-   Web3 预算不是取代财务
    
-   而是：
    
    -   给“公共价值”定价
        
    -   给“贡献”确权
        
    -   给“未来分配权”一个制度接口
        

这正是 DAO、Grant、RetroPGF 的核心。

* * *
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->


**Proof of Participation（PoP）参与凭证系统**

* * *

### 1\. 链下行为（Participation）

可以是任何**低风险、高频、非金融**的行为，例如：

-   参加一次线上 / 线下活动
    
-   完成一次社区任务（内容贡献、翻译、讨论）
    
-   连续签到 / 学习打卡
    
-   为某项目提供一次有效反馈
    
-   DAO 内部的“非治理型贡献”
    

**关键点：**

-   行为发生在链下（网站 / 社区 / 表单）
    
-   不要求用户理解区块链
    

* * *

### 2\. 链上确权（On-chain Proof）

当链下行为被确认后：

-   用户获得一个 **NFT / SBT（不可转让更佳）**
    
-   该凭证只表达三件事：
    
    1.  谁（地址）
        
    2.  在什么时候
        
    3.  参与过什么
        

⚠️ **不代表资产、不承诺收益、不涉及分红**

* * *

### 3\. 链下使用（Redemption / Utility）

链上凭证可以用于：

-   链下权益：
    
    -   活动优先报名
        
    -   折扣 / 周边 / 内容解锁
        
-   平台内权益：
    
    -   等级、声誉、身份标识
        
-   二次验证：
    
    -   “你是否参与过 X”
        
    -   “你是否是早期用户”
        

👉 本质是：**链上负责“不可伪造”，链下负责“怎么用”**

* * *
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->



今天参与了Dao治理的讨论：

| 阶段 | 对应项目 | 你解决的核心问题 |
| --- | --- | --- |
| 第一步 | Builder 工作坊 | 我是否真的适合？我在做什么？ |
| 第二步 | 项目展示夜 | 我做的东西是否有意义？是否能被理解？ |
| 第三步 | 模拟治理游戏 | 我如何在不确定性中做判断并长期存活？ |

**这是一条完整的学习—实践—判断闭环。**

* * *

## 二、从“教育产品”角度看，这三个项目为什么成立

### 1️⃣ Builder 工作坊：不是教技能，而是“去幻觉”

你非常准确地抓住了一点：

> 新人真正缺的不是信息，而是**第一次不被营销、不被叙事包裹的实践体验**

这个工作坊的真正价值不是：

-   学会 Solidity / 工具 / 框架  
    而是：
    
-   **意识到复杂性**
    
-   **意识到协作成本**
    
-   **意识到“我不知道的部分”**
    

这是极高质量的“负反馈教育”，极其稀缺。

**资金在这里应该花在：**

-   小规模（10–20 人）
    
-   高质量引导者
    
-   强制协作而非个人完成
    

* * *

### 2️⃣ 项目展示夜：不是 Demo Day，而是“表达校准器”

你说“权重较少”是非常正确的。

这个展示夜不应该是：

-   成果炫耀
    
-   投资路演
    

而应该是：

> **一次“我能否把不成熟的判断讲清楚”的校准**

它的核心价值有三点：

1.  逼迫参与者把混乱的实践压缩成结构
    
2.  让他们意识到“别人如何误解我”
    
3.  形成最初的表达肌肉
    

这一步**对新人的长期发展影响极大，但对外界几乎不可见**，这正是它的护城河。

* * *

### 3️⃣ 模拟治理游戏：你整个体系中“最核心、最稀缺、最不可抄袭”的部分

你对它的定位非常准确：

> 在黑暗森林中，获得判断力，而不是答案

这意味着它不是：

-   DAO 角色扮演
    
-   规则游戏
    
-   投票练习
    

而是：

> **在信息不完全、利益冲突、时间压力下，持续做选择并承担后果**

这是**职业投资、组织治理、链上生存的通用底层能力**。

**资金在这里的意义最大**，因为：

-   设计成本高
    
-   复用周期长
    
-   抄袭难度极高
    
-   用户粘性最强
    

* * *

## 四、资金配置的一个“专业建议模型”（供你参考）

在不讨论具体金额的前提下，**比例结构建议如下**：

-   **模拟治理游戏：40–50%**
    
    -   规则设计
        
    -   迭代
        
    -   长期运行
        
-   **Builder 工作坊：30–35%**
    
    -   场地
        
    -   引导者
        
    -   小规模多轮
        
-   **项目展示夜：15–20%**
    
    -   轻量
        
    -   稳定
        
    -   不追求规模
        

这不是因为展示夜不重要，而是：

> **它的边际成本最低，边际价值递减最快**
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->




今天参与了Dao治理的讨论：

| 阶段 | 对应项目 | 你解决的核心问题 |
| --- | --- | --- |
| 第一步 | Builder 工作坊 | 我是否真的适合？我在做什么？ |
| 第二步 | 项目展示夜 | 我做的东西是否有意义？是否能被理解？ |
| 第三步 | 模拟治理游戏 | 我如何在不确定性中做判断并长期存活？ |

**这是一条完整的学习—实践—判断闭环。**

* * *

## 二、从“教育产品”角度看，这三个项目为什么成立

### 1️⃣ Builder 工作坊：不是教技能，而是“去幻觉”

你非常准确地抓住了一点：

> 新人真正缺的不是信息，而是**第一次不被营销、不被叙事包裹的实践体验**

这个工作坊的真正价值不是：

-   学会 Solidity / 工具 / 框架  
    而是：
    
-   **意识到复杂性**
    
-   **意识到协作成本**
    
-   **意识到“我不知道的部分”**
    

这是极高质量的“负反馈教育”，极其稀缺。

**资金在这里应该花在：**

-   小规模（10–20 人）
    
-   高质量引导者
    
-   强制协作而非个人完成
    

* * *

### 2️⃣ 项目展示夜：不是 Demo Day，而是“表达校准器”

你说“权重较少”是非常正确的。

这个展示夜不应该是：

-   成果炫耀
    
-   投资路演
    

而应该是：

> **一次“我能否把不成熟的判断讲清楚”的校准**

它的核心价值有三点：

1.  逼迫参与者把混乱的实践压缩成结构
    
2.  让他们意识到“别人如何误解我”
    
3.  形成最初的表达肌肉
    

这一步**对新人的长期发展影响极大，但对外界几乎不可见**，这正是它的护城河。

* * *

### 3️⃣ 模拟治理游戏：你整个体系中“最核心、最稀缺、最不可抄袭”的部分

你对它的定位非常准确：

> 在黑暗森林中，获得判断力，而不是答案

这意味着它不是：

-   DAO 角色扮演
    
-   规则游戏
    
-   投票练习
    

而是：

> **在信息不完全、利益冲突、时间压力下，持续做选择并承担后果**

这是**职业投资、组织治理、链上生存的通用底层能力**。

**资金在这里的意义最大**，因为：

-   设计成本高
    
-   复用周期长
    
-   抄袭难度极高
    
-   用户粘性最强
    

* * *

## 四、资金配置的一个“专业建议模型”（供你参考）

在不讨论具体金额的前提下，**比例结构建议如下**：

-   **模拟治理游戏：40–50%**
    
    -   规则设计
        
    -   迭代
        
    -   长期运行
        
-   **Builder 工作坊：30–35%**
    
    -   场地
        
    -   引导者
        
    -   小规模多轮
        
-   **项目展示夜：15–20%**
    
    -   轻量
        
    -   稳定
        
    -   不追求规模
        

这不是因为展示夜不重要，而是：

> **它的边际成本最低，边际价值递减最快**
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->





# 参与、验证、真实

我之前一直把区块链技术当作验证真实参与的技术，想当然以为区块链技术就是用来解决避免不诚实造假的工具，它毕竟是一个新的信任范式，智能合约在我的理解好像也就是避免违约，需要强制执行。

当时今天突然想到，如果区块链真的需要发挥价值，必然不能链上空转，与链下的结合必然设计验证问题。但是这其中可造假的地方太多。

如果我们从根本上就假设我们无法百分百验证真实性，那我们为什么还要参与？

如果我们需要验证真实性，那验证权从何而来？

一个参与式系统，真正驱动展示与互动的，并不是“是否真实”，而是“我展示之后，会发生什么”。

或者，区块链带来的验证不是一种即时验证，不是当下就判断黑白，而是这个链通过一种不可篡改的记录，让参与者能够持续为自己的后续参与或者曾经的声明负责。通过不可篡改，其实是一种声誉担保，但同一个人会有很多地址，这怎么解决？
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->






## MVP 功能模块总览（开发视角）

```
模块 1：用户与账户
模块 2：行为积分与碳积分映射
模块 3：碳凭证生成（链下）
模块 4：权益兑换（模拟）
模块 5：后台管理与演示辅助
（可选）模块 6：链上留痕（Hash 上链）
```

* * *

## 三、逐模块开发任务清单（可直接分配）

下面每一项都已经是 **“开发任务粒度”**。

* * *

## 模块 1：用户与账户（基础必做）

### 1.1 用户注册 / 登录（极简）

**功能说明：**

-   用户可通过手机号 / 邮箱 / 微信 OpenID（任选其一）登录
    
-   不需要密码体系，支持验证码或 mock 登录
    

**开发任务：**

-   创建 User 表
    
    -   字段：user\_id、nickname、login\_type、created\_at
        
-   登录接口（POST /login）
    
-   获取当前用户信息（GET /me）
    

**验收标准：**

-   能在 Demo 中快速切换不同用户
    
-   页面展示“我的昵称 + 积分”
    

* * *

## 模块 2：行为积分与碳积分映射（核心叙事）

### 2.1 行为积分模拟

**功能说明：**

-   模拟“用户已有的环保行为积分”（对应蚂蚁森林）
    
-   行为类型固定 2–3 种即可
    

**示例行为：**

-   绿色出行 +50
    
-   节能减排 +30
    
-   低碳消费 +20
    

**开发任务：**

-   BehaviorRecord 表
    
    -   字段：id、user\_id、behavior\_type、points、created\_at
        
-   行为生成接口（POST /behavior/mock）
    
-   行为列表接口（GET /behavior/list）
    

* * *

### 2.2 碳积分映射规则（非常关键）

**功能说明：**

-   将“行为积分”映射为“碳积分（Carbon Points）”
    
-   使用**简单、可解释规则**
    

**示例规则：**

```
100 行为积分 = 1 碳积分
```

**开发任务：**

-   CarbonRule 表
    
    -   字段：rule\_id、ratio、description
        
-   计算碳积分服务函数
    
-   碳积分汇总接口（GET /carbon/balance）
    

**验收标准：**

-   能清楚解释“为什么是这个数”
    
-   规则可配置（不是写死）
    

* * *

## 模块 3：碳凭证生成（链下确权核心）

### 3.1 碳凭证生成

**功能说明：**

-   用户消耗一定碳积分，生成一张“碳参与凭证”
    
-   凭证不是资产，只是权益凭证
    

**开发任务：**

-   CarbonVoucher 表
    
    -   字段：
        
        -   voucher\_id
            
        -   user\_id
            
        -   carbon\_amount
            
        -   status（unused / redeemed）
            
        -   hash
            
        -   created\_at
            
-   凭证生成接口（POST /voucher/create）
    
-   凭证列表接口（GET /voucher/list）
    
-   凭证详情接口（GET /voucher/{id})
    

**技术要点：**

-   hash = hash(voucher\_id + user\_id + carbon\_amount + timestamp)
    

* * *

## 模块 4：权益兑换（To-C 闭环展示）

### 4.1 权益配置

**功能说明：**

-   预设 2–3 个权益（咖啡、数字挂件、IP 联名）
    
-   每个权益需要消耗一定碳积分
    

**开发任务：**

-   Benefit 表
    
    -   字段：benefit\_id、name、carbon\_cost、type、description
        
-   权益列表接口（GET /benefit/list）
    

* * *

### 4.2 兑换流程（模拟核销）

**功能说明：**

-   用户使用碳凭证兑换权益
    
-   生成一个“核销码”或“已兑换状态”
    

**开发任务：**

-   RedeemRecord 表
    
    -   字段：id、voucher\_id、benefit\_id、redeem\_code、created\_at
        
-   兑换接口（POST /benefit/redeem）
    
-   更新凭证状态为 redeemed
    

**验收标准：**

-   路演中可以点“兑换”
    
-   页面显示“兑换成功”
    

* * *

## 模块 5：后台管理与演示辅助（比赛神器）

### 5.1 管理后台（可以极简）

**功能说明：**

-   给评委看的“系统可信感”
    

**开发任务：**

-   查看所有用户
    
-   查看行为记录
    
-   查看已生成碳凭证
    
-   查看兑换记录
    

**技术实现建议：**

-   简单 Admin 页面
    
-   或直接 Swagger + 表格展示
    

* * *

## （可选）模块 6：链上留痕（加分项）

### 6.1 凭证 Hash 上链

**功能说明：**

-   仅将凭证 hash 写入链上
    
-   不涉及 Token、不转账
    

**开发任务：**

-   智能合约（存 hash）
    
-   上链接口（POST /voucher/anchor）
    
-   返回 tx hash
    
-   前端展示“已上链”
    

**推荐链：**

-   Polygon / Base
    

**验收标准：**

-   路演时能展示区块浏览器链接（截图也可）
    

* * *

## 四、开发优先级（给 PM / 你自己用）

**P0（必须）：**

-   模块 1、2、3、4
    

**P1（强烈建议）：**

-   模块 5
    

**P2（锦上添花）：**

-   模块 6
    

* * *

## 五、评委最在意但开发容易忽略的三点（请同步给开发）

1.  **字段命名要“像碳行业”**
    
    -   carbon\_amount、voucher、anchor\_hash
        
2.  **每一步都能“被解释”**
    
    -   不要魔法数字
        
3.  **Demo 流程必须顺**
    
    -   登录 → 行为 → 积分 → 凭证 → 兑换
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->








## X. 核心审计与可信性架构（Audit & Trust Architecture）

### X.1 设计目标与边界

本系统的审计目标并非重新定义碳核算科学标准，而是解决以下**工程与治理问题**：

1.  **真实性（Truthfulness）**
    
    确保被引用的绿色行为与碳积分，源自真实、已发生的链下行为。
    
2.  **唯一性（Uniqueness / No Double Use）**
    
    确保同一份减排行为或积分结果，在跨平台、跨商户场景中**只能被使用一次**。
    
3.  **可审计性（Auditability）**
    
    任何外部主体均可在不获取隐私数据的前提下，验证履约事实是否存在、是否合规。
    
4.  **反戏绿（Anti-Greenwashing）**
    
    限制商户或平台对减排效果的夸大性、模糊性或不可证伪的营销声明。
    

> 本系统不对“减排是否足够有效”“项目是否符合某一政策目标”作价值判断，该类问题属于政策与科学讨论范畴。

* * *

### X.2 审计对象的最小可信单元（MCU）

系统审计的最小单位定义为 **Carbon Redemption Proof（CRP）**，即“碳履约事实证明”。

每一份 CRP 对应一次明确的、不可拆分的积分使用与履约行为，包含以下字段：

-   行为引用摘要（Hash of Ant Forest Credit Usage）
    
-   商户唯一标识
    
-   抵消类型（如绿电、林业、其他合规类型）
    
-   抵消数量（kg CO₂e 或等价单位）
    
-   商户碳能力来源标识（如绿证批次 / CCER 项目 ID）
    
-   状态标识：`Pending / Confirmed / Invalid`
    
-   时间戳与区块高度
    

CRP 不包含用户身份信息、交易金额或商业敏感数据。

* * *

### X.3 链上状态机与唯一性约束

为防止重复使用与事后篡改，CRP 在链上以**状态机形式存在**，其状态只能按以下顺序单向变化：

```
Unused → Pending → Confirmed
                 → Invalid

```

### 关键约束条件

-   任一商户在任一时间窗口内：
    
    ```
    Σ Confirmed_CRP ≤ Total_Carbon_Capacity
    
    ```
    
-   智能合约层面强制执行额度守恒，不允许超发、回滚或事后补录。
    

该约束无需依赖人工审计，即可在系统层面防止“先使用、后补碳”的行为。

* * *

### X.4 链下数据与链上审计的连接方式

系统采用 **Hash Anchoring（哈希锚定）** 模式进行链下—链上对接：

-   原始业务数据（积分明细、财务台账、供应链文件）仅保存在链下系统
    
-   链上仅存储：
    
    -   数据摘要（Merkle Root / Hash）
        
    -   版本号
        
    -   时间戳
        

任何链下数据的修改都会导致链上哈希不一致，从而被外部审计者发现。

该模式在保证隐私与合规的同时，实现了**低成本、可复核的审计能力**。

* * *

### X.5 第三方与 Oracle 的低权限审计机制

系统不将第三方审计机构作为单一信任源，而是引入**低权限、多主体审计模型**：

-   第三方可对以下事项进行审计与签名：
    
    -   数据完整性（是否缺失）
        
    -   规则执行一致性（是否按公开规则运行）
        
    -   抽样真实性（随机抽查链下凭证）
        
-   第三方无权：
    
    -   修改 CRP 状态
        
    -   增减商户碳能力额度
        
    -   删除链上记录
        

不同第三方的审计意见可并存，系统不强制形成单一结论。

* * *

### X.6 防戏绿机制与声明边界控制

为防止夸大性或不可证伪的环保声明，系统引入以下硬性约束：

### 1\. 声明模板化（Claim Standardization）

商户对外披露的减排声明，必须引用具体 CRP，并使用系统预定义模板，不允许自由发挥。

### 2\. 影响上限（Impact Ceiling）

每类行为与商品设定最大可宣称的碳影响值，避免通过营销叙事无限放大真实贡献。

### 3\. 可反驳性（Falsifiability）

任何声明必须满足：

-   可追溯至具体 CRP
    
-   可按公开规则复算
    
-   可被第三方提出异议并记录在链
    

无法被证伪的声明，默认不被系统支持。

* * *

### X.7 信用模型总结

本系统的信用不来源于单一平台、单一审计机构或单一区块链节点，而来源于以下结构性约束的叠加：

-   行为真实性来自蚂蚁森林等高可信 Web2 系统
    
-   使用唯一性由链上状态机保证
    
-   审计可复核性由规则公开与哈希锚定保证
    
-   叙事边界由声明模板与影响上限约束
    

> 系统通过降低作恶收益、提高作恶成本，而非依赖权威背书，来建立长期可信性。

## X+1. 与 EU / 中国 ESG 披露标准的映射关系

### X+1.1 章节目的与适用范围

本章节旨在说明：

本系统在**不替代现有 ESG / 碳披露标准**的前提下，如何作为一层**技术性审计与数据可信基础设施**，支持企业与平台更低成本、更高一致性地完成相关披露义务。

本章节不构成任何法律意见或合规保证。

* * *

### X+1.2 与欧盟 ESG / 碳披露框架的映射

### 1\. EU CSRD（Corporate Sustainability Reporting Directive）

CSRD 要求企业对可持续发展信息进行**系统性、可审计披露**，核心特征包括：

-   数据可追溯
    
-   方法论一致
    
-   支持第三方审计
    
-   防止选择性披露
    

**映射关系如下：**

| CSRD 要求 | 本系统支持方式 |
| --- | --- |
| 数据来源可追溯 | CRP 记录引用明确的行为来源与时间戳 |
| 方法论一致性 | 碳积分与履约计算规则版本化并公开 |
| 可审计性 | Hash Anchoring + 外部可复核接口 |
| 防重复计算 | 链上唯一性状态机（No Double Use） |

> 本系统不生成 CSRD 报告本身，而为报告提供可验证的底层事实记录。

* * *

### 2\. ESRS（European Sustainability Reporting Standards）

ESRS 强调 **“披露的可验证性与可比性”**，尤其在环境（E）维度。

**相关条款对应：**

-   **ESRS E1（Climate Change）**
    
    -   要求披露减排措施、抵消使用情况及其边界
        
    -   本系统通过：
        
        -   明确区分“行为激励”与“碳抵消履约”
            
        -   提供可追溯、不可重复的履约证明
            
-   **ESRS 的反“误导性披露”原则**
    
    -   本系统通过声明模板与影响上限，技术性限制夸大表述
        

* * *

### 3\. EU 对 Greenwashing 的监管趋势

欧盟监管机构明确反对以下行为：

-   模糊的“碳中和”“气候友好”声明
    
-   无法核查的数据来源
    
-   同一减排被多次声明
    

**系统对应机制：**

-   禁止自由文本式环保声明
    
-   所有声明必须引用具体 CRP
    
-   声明本身具备可反驳性（Falsifiability）
    

* * *

### X+1.3 与中国 ESG / 碳信息披露框架的映射

### 1\. 中国企业 ESG 披露总体趋势

当前中国 ESG 披露呈现以下特征：

-   标准逐步统一（交易所、国资委、行业指引）
    
-   强调真实性与可核验性
    
-   避免将“激励性行为”直接等同为“碳资产”
    

本系统的定位与上述趋势保持一致。

* * *

### 2\. 与交易所 ESG 指引的关系（原则层面）

| 披露要求方向 | 本系统作用 |
| --- | --- |
| 真实披露环境行为 | 提供行为真实性的技术引用 |
| 避免重复计算 | 链上唯一性防重复声明 |
| 支持审计 | 提供链上不可篡改的履约事实层 |
| 控制夸大宣传 | 技术限制可宣称影响范围 |

系统不直接生成 ESG 披露文本，而是为披露提供**可核查的数据基础**。

* * *

### 3\. 与中国碳市场（CCER / 配额）边界区分

本系统**不将蚂蚁森林积分或用户行为积分定义为**：

-   法定碳配额
    
-   可交易 CCER
    
-   金融资产
    

而是明确定位为：

> “已发生绿色行为在特定激励场景中的一次性使用与履约记录”

该定位有助于避免与现行碳市场制度产生冲突。

* * *

### X+1.4 审计逻辑与监管友好性总结

从监管视角看，本系统具备以下特征：

1.  **不创造新的碳资产类别**
    
2.  **不改变现有核算或披露标准**
    
3.  **仅解决真实性、唯一性与可审计性问题**
    
4.  **允许多方审计意见并存，避免单点信任**
    

系统通过技术手段降低信息不对称与合规成本，而非替代监管判断。

* * *

### X+1.5 小结

本系统可以被理解为：

> 一层面向 ESG 与碳披露的“技术型公共记账与审计基础设施”，而非新的碳市场或新的标准制定者。

其价值在于提升披露可信度、降低重复核验成本，并为跨平台、跨主体的环境声明提供一致的技术基础。

## X+1. 与 EU / 中国 ESG 披露标准的映射关系（条款编号版）

### X+1.1 章节目的与适用范围

本章节说明本系统在**不替代现有 ESG / 碳披露标准**前提下，如何作为**技术型审计基础设施**，支持企业和平台更低成本、更高一致性地完成相关披露。

不构成法律意见或合规保证。

* * *

### X+1.2 与欧盟 ESG / 碳披露框架映射

### 1\. EU CSRD（Corporate Sustainability Reporting Directive, Directive (EU) 2022/2464）

-   **条款引用：**
    
    -   Art. 19a: 可持续发展信息披露义务
        
    -   Annex I, II: 披露内容范围、环境指标
        
    -   Art. 29a: 第三方核验要求
        

**系统支持映射**

| CSRD 要求 | 对应系统机制 |
| --- | --- |
| 数据来源可追溯 (Art.19a, Annex I) | CRP 记录积分使用来源与时间戳 |
| 方法论一致性 (Annex II) | 积分计算规则版本化并公开 |
| 可审计性 (Art.29a) | Hash Anchoring + 外部可复核接口 |
| 防重复计算 (Annex II, Art.19a) | 链上状态机（No Double Use） |

> 系统提供底层事实记录，不生成 CSRD 报告。

* * *

### 2\. ESRS（European Sustainability Reporting Standards, EFRAG 发布）

-   **条款引用：**
    
    -   ESRS E1, Par. 65–72: 气候变化减排披露
        
    -   ESRS E1-3: 抵消措施说明
        
    -   ESRS 2-13: 可验证性要求
        
    -   ESRS 2-15: 避免误导性声明
        

**映射机制**

-   CRP 明确行为激励与履约边界
    
-   链上记录不可重复、可审计
    
-   声明模板 + 影响上限控制“戏绿”风险
    

* * *

### 3\. EU Greenwashing 风险控制

-   禁止模糊“碳中和”或“气候友好”声明
    
-   防止重复宣称减排
    
-   系统机制：
    
    -   统一声明模板
        
    -   每次声明引用 CRP 哈希
        
    -   可反驳性设计
        

* * *

### X+1.3 与中国 ESG / 碳信息披露框架映射

### 1\. 中国企业 ESG 披露趋势

-   参考交易所上市公司 ESG 指引：
    
    -   上交所《上市公司 ESG 信息披露指南（2022）》条款 2.2–2.6
        
    -   深交所《上市公司 ESG 信息披露指引（2022）》条款 1.3–1.5
        
-   要求：
    
    -   数据真实性与可核验性
        
    -   防止重复或夸大披露
        

**映射机制**

| 披露方向 | 对应系统机制 |
| --- | --- |
| 真实披露环境行为 | CRP 引用积分来源与时间戳 |
| 避免重复计算 | 链上唯一性状态机 |
| 支持审计 | Hash Anchoring + 外部可复核接口 |
| 控制夸大宣传 | 声明模板 + 影响上限 |

* * *

### 2\. 中国碳市场（CCER / 配额）边界

-   系统明确：
    
    -   蚂蚁森林积分不作为法定碳配额
        
    -   不可交易 CCER
        
    -   不作为金融资产
        

> 明确定位为已发生绿色行为的一次性使用与履约记录。

* * *

### X+1.4 审计逻辑与监管友好性总结

-   不创造新的碳资产
    
-   不改变现有核算或披露标准
    
-   仅解决真实性、唯一性、可审计性
    
-   多方审计意见并存，避免单点信任
    

* * *

### X+1.5 小结

本系统可被理解为：

> 面向 ESG / 碳披露的技术型公共记账与审计基础设施，
> 
> 提供可核查、不可重复的底层数据，支持跨平台、跨主体环境声明。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->










# 与“数据确权”相关的关键研究议题

任何适合合规研究的数据资产，都要回答以下基础问题：

* * *

## 1\. 数据确权：权利定义与法律挂钩

-   数据上链前权属如何定义？
    
-   上链后数据如何被法律承认？
    
-   谁可以修改 / 批准 / 撤回？
    
-   是否存在“唯一性证明”？
    

> 核心目标：让链上数据可作为可执行权利与责任而非“不可归责的日志”。

* * *

## 2\. 数据可审计性与链—链下映射

-   链上数据如何与链下现实事件映射？
    
-   跨数据源（IoT / ERP / KYC / 政府系统）如何融合？
    
-   如何处理“真实世界与链上分叉”？
    

* * *

## 3\. 合规风险指标体系设计

-   什么是合法
    
-   什么是可疑
    
-   什么是违规
    

每一个指标必须是：

-   可计算
    
-   可解释
    
-   可追责
    

这与传统 KYC 体系非常不同。

* * *

## 4\. 隐私保护与合规之间的制度张力

学界与监管一致认为：

> 完全透明 ≠ 合规友好
> 
> 完全匿名 ≠ 合规接受

**研究关键**：如何在隐私保护与合规可审计之间找到平衡。

* * *

# 碳信用额度

Singapore is actively developing its carbon market to become a global hub for carbon trading. A key step in this direction was the country’s first-ever carbon credit auction, which attracted over S$1.3 billion (around $1 billion) in bids.

A [carbon credit](https://carboncredits.com/the-ultimate-guide-to-understanding-carbon-credits/) is a certificate representing one tonne of carbon dioxide (CO2) removed from or prevented from entering the atmosphere. Companies and countries can buy these credits to offset their greenhouse gas emissions.

To support this, Singapore introduced a [carbon tax](https://carboncredits.com/carney-scraps-carbon-tax-can-canada-reduce-emissions-without-it/) in 2019. This tax encourages companies to lower their emissions by making pollution more expensive. The country also aims to be a global hub for carbon trading. It’s attracting investments and partnerships from various regions

# **Singapore’s $1 Billion Carbon Credit Auction**

In September 2024, Singapore made headlines with its first-ever [carbon credit](https://carboncredits.com/singapores-carbon-credit-market-surging-at-21-cagr/) tender. The government aimed to buy at least 500,000 nature-based carbon credits, which would offset the same amount of CO2 emissions.

Nature-based credits come from projects that restore forests, protect ecosystems, or promote sustainable agriculture.

The tender attracted significant interest, with 17 submissions totaling over S$1.3 billion, about US$1 billion. The highest bid came from [Trafigura](https://carboncredits.com/trafigura-bets-big-600m-on-carbon-credits-market-revival/), a global commodities trading company, at nearly S$300 million.

Other major bidders included Mercuria Asia Resources, DNZ ClimateTech (S$200,000), Temasek-backed GenZero (S$27.5 million), Shell (S$34 million), and PetroChina (S$21.8 million).

These bids show the growing demand for carbon credits as a tool to fight climate change. Globally, demand for these credits could grow 100x by 2050, per [McKinsey & Company](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.facebook.com%2Fphoto.php%3Ffbid%3D10159598511058953%26id%3D21987463952%26set%3Da.10150662656883953%26locale%3Dth_TH&psig=AOvVaw37X3qY7hom8ZqfqXhH2s7i&ust=1743702236369000&source=images&cd=vfe&opi=89978449&ved=0CBcQjhxqFwoTCKDYk-jzuYwDFQAAAAAdAAAAABAE) estimates. Companies and governments view carbon trading as a method to offset emissions. It also helps fund environmental projects.

**$5.6 Billion and Counting: Building a Carbon Trading Hub**

By 2026, analysts predict that carbon markets will be growing quickly. More firms are integrating [carbon credit](https://carboncredits.com/the-ultimate-guide-to-understanding-carbon-credits/) strategies into their business models. Some generate credits directly. Others build markets or invest in credits. This article highlights the top public companies that stand out in the carbon credit space.

**Tesla: Leading Carbon Credit Revenue**

Since 2017, Tesla has earned more than $10.4 billion from selling carbon credits. That revenue stream is crucial for the company’s finances. It matters more as competition in the EV market grows and profit margins shrink.

**Carbon Streaming Corporation: A New Model for Credits**

Carbon Streaming is listed on Canadian and U.S. markets under tickers such as NETZ and OFSTF. As carbon markets grow, the model could expand. More credits might come from [forest protection](https://carboncredits.com/forest-finance-hits-record-growth-in-2025-investment-doubles-for-nature-based-climate-action/), clean energy, or [carbon capture](https://carboncredits.com/what-is-carbon-capture-and-storage-your-ultimate-guide-to-ccs-technology/) programs. These would then boost its balance sheet.

# **ntercontinental Exchange: Exchange Infrastructure for Carbon Markets洲际交易所：碳市场交易基础设施**

Intercontinental Exchange (ICE) is a financial markets company known for running major exchanges. ICE supports carbon markets by providing the infrastructure for trading carbon allowances and credits. This includes platforms for compliance markets like the European Union Emissions Trading System ([EU ETS](https://carboncredits.com/eu-bets-on-carbon-credits-bold-2040-climate-target-adds-global-twist/)) and other regional cap‑and‑trade programs.洲际交易所（ICE）是一家以运营大型交易所而闻名的金融市场公司。ICE 通过提供碳排放配额和信用交易的基础设施来支持碳市场。这包括欧盟排放交易体系（ [EU ETS](https://carboncredits.com/eu-bets-on-carbon-credits-bold-2040-climate-target-adds-global-twist/) ）等合规市场平台以及其他区域性碳排放交易计划。

Carbon credits and emissions allowances traded on ICE help companies meet regulated limits. By offering transparent pricing and reliable settlement, ICE reduces barriers for institutional participation. As carbon pricing systems expand globally, the need for strong trading infrastructure grows, too.在洲际交易所 (ICE) 交易的碳信用额和排放配额有助于企业达到监管限制。ICE 提供透明的定价和可靠的结算，从而降低了机构参与的门槛。随着碳定价体系在全球范围内的扩展，对强大的交易基础设施的需求也日益增长。

# **Xpansiv: Leading Carbon and Environmental Commodities ExchangeXpansiv：领先的碳和环境商品交易所**

[Xpansiv](https://carboncredits.com/from-tokyo-to-new-york-xpansiv-strengthens-global-role-in-climate-data-and-carbon-market-innovation/) is a technology company that operates one of the world’s largest [carbon credit exchanges](https://carboncredits.com/xpansiv-and-krx-collaborate-on-korean-carbon-credit-market-launch/) for voluntary environmental commodities. Its platform, the Carbon Business Line (CBL), is used by companies trading voluntary carbon credits and other climate‑linked assets.[Xpansiv](https://carboncredits.com/from-tokyo-to-new-york-xpansiv-strengthens-global-role-in-climate-data-and-carbon-market-innovation/) 是一家科技公司，运营着全球最大的自愿性环境商品[碳信用交易平台](https://carboncredits.com/xpansiv-and-krx-collaborate-on-korean-carbon-credit-market-launch/)之一。其平台 Carbon Business Line (CBL)供交易自愿性碳信用和其他气候相关资产的公司使用。

Xpansiv’s system has handled more than 250 million metric tons of carbon dioxide equivalent (CO₂e) transactions since 2020. In 2025, weekly trading volumes often exceeded 300,000 tons, with most credits coming from [nature‑based projects](https://carboncredits.com/a-recap-of-the-voluntary-carbon-market-quality-over-quantity/) like forestry and land restoration.自 2020 年以来，Xpansiv 的系统已处理超过 2.5 亿公吨二氧化碳当量 (CO₂e) 交易。到 2025 年，每周交易量通常超过 30 万吨，其中大部分碳信用额来自林业和土地恢复等[基于自然的项目](https://carboncredits.com/a-recap-of-the-voluntary-carbon-market-quality-over-quantity/) 。

Xpansiv has also expanded its listings to include removal‑only credits and [CORSIA](https://carboncredits.com/xpansiv-to-launch-new-carbon-credit-contract-to-support-corsia-compliance/)‑compliant aviation credits. Its new partnership with the Korea Exchange (KRX) seeks to create a carbon credit trading market in Asia. This will connect KRX to Xpansiv’s global platform. This could increase liquidity and price discovery in new regions.Xpansiv 还扩大了其碳信用额度列表，新增了仅可移除的碳信用额度和符合 [CORSIA](https://carboncredits.com/xpansiv-to-launch-new-carbon-credit-contract-to-support-corsia-compliance/) 标准的航空碳信用额度。其与韩国交易所 (KRX) 的新合作旨在亚洲打造碳信用交易市场。此举将 KRX 与 Xpansiv 的全球平台连接起来，有望提升新地区的流动性和价格发现能力。

# **Drax Group: From Power Generation to Carbon Removals德拉克斯集团：从发电到碳清除**

[Drax Group](https://www.drax.com/about-us/) plc is a British power generation company listed on the London Stock Exchange. In recent years, Drax has expanded into [carbon removal projects](https://carboncredits.com/big-tech-firms-microsoft-msft-and-alphabet-googl-lead-in-durable-carbon-removal-investments-exceeding-10-billion/), including bioenergy with carbon capture and storage ([BECCS](https://carboncredits.com/microsoft-invests-in-louisiana-based-c2xs-beccs-plant-to-capture-3-6-million-tons-of-co%e2%82%82/)).高级点数已不足，请购买加量包以继续使用 AI 翻译服务。立即增购[Drax Group](https://www.drax.com/about-us/) plc 是一家在伦敦证券交易所上市的英国发电公司。近年来，Drax 已将业务拓展至[碳去除项目](https://carboncredits.com/big-tech-firms-microsoft-msft-and-alphabet-googl-lead-in-durable-carbon-removal-investments-exceeding-10-billion/) ，包括生物能源碳捕获与封存（ [BECCS](https://carboncredits.com/microsoft-invests-in-louisiana-based-c2xs-beccs-plant-to-capture-3-6-million-tons-of-co%e2%82%82/) ）项目。

Drax has a carbon removals deal. They will provide 25,000 metric tons of CO2 removals using BECCS credits. The price starts at $350 per ton. These credits represent permanent carbon storage rather than simple emissions reductions.Drax 公司达成了一项碳移除协议。他们将使用生物能源碳捕获与封存（BECCS）信用额度移除 25,000 公吨二氧化碳。价格从每吨 350 美元起。这些信用额度代表的是永久性碳储存，而非简单的减排。

# **Incentivizing Gigaton-Scale Carbon Dioxide Removal via a Climate-Positive Blockchain**

[Jonathan Bachman](https://arxiv.org/search/cs?searchtype=author&query=Bachman,+J), [Sujit Chakravorti](https://arxiv.org/search/cs?searchtype=author&query=Chakravorti,+S), [Shantanu Rane](https://arxiv.org/search/cs?searchtype=author&query=Rane,+S), [Krishnan Thyagarajan](https://arxiv.org/search/cs?searchtype=author&query=Thyagarajan,+K)

> A new crypto token is proposed as an incentive mechanism to remove CO2 from the atmosphere permanently at gigaton scale. The token facilitates CO2 removal (CDR) by providing financial incentives to those that are removing CO2 and an opportunity to provide additional financial resources for CDR by the public. The new token will be native to a blockchain that uses a Proof-of-Useful-Work (PoUW) consensus mechanism. The useful work will be conducted by direct air carbon capture and storage (DACCS) facilities that will compete with each other based on the amount of CO2 captured and permanently stored. In terms of energy consumption, we require that the entire process, comprising DACCS technology and all blockchain operations, be climate positive while accounting for life cycle analysis of equipment used. We describe the underlying reward mechanism coupled with a verification mechanism for CDR. In addition, we consider security features to limit attacks and fraudulent activity. Finally, we outline a roadmap of features that are necessary to fully implement and deploy such a system, but are beyond the current scope of this article.
> 
> 本文提出了一种新的加密代币，旨在激励人们以千兆吨级的规模永久性地从大气中去除二氧化碳。该代币通过向二氧化碳去除者提供经济激励，并允许公众为二氧化碳去除提供额外的资金，从而促进二氧化碳去除（CDR）的开展。这种新代币将原生于一个采用有用工作证明（PoUW）共识机制的区块链上。有用工作将由直接空气碳捕获与封存（DACCS）设施完成，这些设施将根据捕获并永久封存的二氧化碳量相互竞争。在能源消耗方面，我们要求整个过程（包括 DACCS 技术和所有区块链操作）在考虑所用设备的生命周期分析的同时，实现气候正效益。本文描述了底层奖励机制以及与之配套的二氧化碳去除验证机制。此外，我们还考虑了限制攻击和欺诈活动的安全特性。最后，我们概述了全面实施和部署此类系统所需的功能路线图，但这些功能超出了本文的讨论范围。

| Comments: | 20 pages, 3 figures  20页，3幅图 |
| --- | --- |
| Subjects:  主题： | Cryptography and Security (cs.CR); Computers and Society (cs.CY) |
| Cite as: | arXiv:2308.02653 [cs.CR] |
|   | (or arXiv:2308.02653v1 [cs.CR] for this version) |
|   | https://doi.org/10.48550/arXiv.2308.02653Focus to learn more |

# **《Incentivizing Gigaton-Scale Carbon Dioxide Removal via a Climate-Positive Blockchain》**

## Main Thesis (English)

The paper proposes a **climate-positive blockchain system** that incentivizes **gigaton-scale carbon dioxide removal (CDR)** by directly coupling blockchain consensus with **real, verifiable physical work**, specifically **Direct Air Carbon Capture and Storage (DACCS)**.

Its central idea is to replace wasteful Proof-of-Work with a **Proof-of-Useful-Work (PoUW)** mechanism, where:

-   The “work” required to create new blocks and mint tokens is the **actual capture and permanent storage of atmospheric CO₂**.
    
-   DACCS facilities compete in a decentralized system, and the probability of receiving newly minted tokens is proportional to the **verified amount of CO₂ they remove**.
    
-   All participating facilities contribute positively to climate outcomes, even if they do not win a given round.
    

The blockchain is designed to be **net climate-positive**, accounting for:

-   Life-cycle emissions of DACCS facilities,
    
-   Energy consumption of blockchain operations,
    
-   Manufacturing and operational emissions of sensors and infrastructure.
    

To ensure trust without centralized authorities, the authors propose:

-   **Tamper-resistant sensor telemetry**,
    
-   **Cryptographic signatures and timestamps**,
    
-   **Zero-knowledge proofs** to verify CO₂ removal claims without revealing sensitive operational data.
    

Unlike traditional carbon credits:

-   The token does **not represent a fixed amount of CO₂**,
    
-   Its value is **market-determined**, similar to a cryptocurrency,
    
-   It can function both as an **incentive for carbon removal** and potentially as a **general medium of exchange**.
    

Overall, the paper argues that a properly designed climate-positive blockchain could mobilize capital, technology, and incentives at a scale necessary to meet global climate targets—while avoiding the moral hazard and opacity of existing carbon offset markets. 2308.02653v1

* * *

## 核心观点（中文）

这篇论文提出了一种\*\*“气候正效应区块链”（climate-positive blockchain）**的整体框架，核心目标是通过区块链机制**激励千亿吨级（gigaton-scale）的二氧化碳移除（CDR）**，重点采用**直接空气捕集与封存（DACCS）\*\*技术。

论文的核心思想可以概括为一句话：

> 让区块链“挖矿”本身成为真实、可验证、对气候有正贡献的物理工作。

具体而言，作者提出了一种 **Proof-of-Useful-Work（PoUW，有用工作量证明）** 机制，其关键特征包括：

1.  **区块链共识直接绑定现实世界的碳移除行为**
    
    -   新区块和代币的产生，取决于 DACCS 设施在现实世界中**实际捕集并永久封存的 CO₂ 数量**。
        
    -   捕集量越大，获得奖励的概率越高。
        
2.  **与传统 PoW 的根本不同**
    
    -   比特币 PoW 消耗能源却产生排放；
        
    -   PoUW 使用能源来“移除碳”，且即便未中奖的参与者，其工作也并非浪费，而是对气候有正贡献。
        
3.  **系统要求“净气候正效应”**
    
    -   不仅计算 CO₂ 捕集量，还严格纳入：
        
        -   DACCS 全生命周期排放（LCA），
            
        -   区块链运行能耗，
            
        -   设备与传感器制造排放。
            
    -   只有在“捕集 > 排放”时，才具备参与资格。
        
4.  **去中心化、可验证、抗造假**
    
    -   使用防篡改传感器实时采集数据；
        
    -   通过数字签名与时间戳确保真实性；
        
    -   借助零知识证明（ZKP），在不暴露商业机密的情况下验证“碳移除是否真实发生”。
        
5.  **区别于传统碳信用（Carbon Credits）**
    
    -   代币不等价于“1 吨 CO₂”；
        
    -   代币价格由市场决定，而非行政定价；
        
    -   可作为激励工具，甚至发展为通用支付媒介。
        

论文认为，现有碳信用机制存在**道德风险、不透明、激励不足**等结构性问题，而将区块链共识与真实物理减排行为直接绑定，可能为全球气候治理提供一种**规模化、可审计、去中心化的新范式**。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->












# 为什么现有合规设计对“防庞氏”几乎无效？

我们直接拆穿。

### 现在主流合规在做什么？

-   KYC / AML
    
-   证券属性认定
    
-   信息披露
    
-   投资者适当性
    

### 但庞氏依然成立的原因是：

-   **这些合规都发生在“交易层”**
    
-   而庞氏发生在\*\*“收益生成层”\*\*
    

> 只要收益逻辑仍然是：
> 
> **代币价格上涨 ← 新资金进入 ← 流动性刺激 ← 叙事驱动**
> 
> 那你即便 100% 合规，也只是“合规的庞氏”

# 真正决定“非庞氏”的 5 个底层合规设计（核心）

下面这五个，是**目前最有研究价值、且对市场真正有指导意义的合规设计维度**。

我会明确告诉你：

**哪一个是“决定性”的，哪几个是“辅助性”的。**

* * *

## ①【决定性】收益外生性合规（Exogenous Revenue Compliance）

### 核心判断问题

> 这个数字资产的收益，是否必须来自链外？

### 链外收益包括什么？

-   法币支付
    
-   实体服务收费
    
-   企业付费
    
-   政府 / 机构采购
    
-   合规金融机构对价
    

### 合规设计重点

-   收益来源必须可审计
    
-   收益进入链上的路径明确
    
-   不允许“自我支付”或“循环返还”
    

### 为什么这是“底层护城河”？

> 只要收益是外生的，就不可能是纯庞氏。

* * *

## ②【极其关键】收益—角色绑定合规（Role-bound Revenue）

这是你前面提到“社会角色识别”的直接升级版。

### 合规问题不是“谁是你”

而是：

> 你在系统中“做了什么”，你才能拿什么钱

### 典型庞氏结构

-   持币 ≈ 收益权
    
-   不参与生产也能拿钱
    

### 非庞氏设计

-   收益与：
    
    -   提供流动性
        
    -   提供算力
        
    -   提供数据
        
    -   提供风控
        
    -   提供真实服务
        
        **强绑定**
        

### 链上可验证性

-   行为可度量
    
-   行为可重复
    
-   行为不可伪造
    

* * *

## ③【结构性】收益离链合规（Off-chain Settlement Requirement）

这是**非常少被讨论、但极其重要的一条**。

### 研究假设

> 如果所有收益都留在链上，系统必然内卷并庞氏化。

### 合规设计方向

-   强制一部分收益：
    
    -   以法币结算
        
    -   或用于链外支付
        
    -   或缴税
        
    -   或用于真实成本
        

### 意义

-   让“赚钱”变成一件**有摩擦、有成本、有监管足迹**的事
    
-   直接压制庞氏增长曲线
    

* * *

## ④【治理性】反通胀与反激励滥用合规

### 庞氏常见手段

-   高 APY
    
-   代币补贴
    
-   流动性挖矿
    

### 合规研究重点

-   激励是否有“退出机制”
    
-   是否必须随真实收入下降
    
-   是否设有**激励衰减规则**
    

### 判断标准

> 如果激励不依赖真实收入，而依赖“市场情绪”，那它只是延迟爆炸。

* * *

## ⑤【系统性】可失败、可清算合规（Failure-aware Design）

这是**市场长期忽视、监管极其关心的点**。

### 非庞氏系统的特征

-   能失败
    
-   能关停
    
-   能清算
    
-   不拖累整个系统
    

### 合规要求

-   风险隔离
    
-   明确清算顺序
    
-   用户预期管理
    

* * *

# **代币经济研究**

我相信在未来各种代币不会真的空转、结合现在的dao组织实践、赋能各种线下经济活动、比如文旅、品牌营销等各种真实经济活动、代币本身就是传统生产要素（劳动、资本）等的链上证明。这样可追踪、可审计且能下链下创作稳定现金流的是真的有价值的代币。从而衍生出的数字资产是真的有价值的资产类别。但这其中的具体研究应该有很多方向，目前确定的研究方向是碳数据/茅台资产的现实赋能

## 碳数据：

### 1️⃣ 现实世界的问题是什么？

-   碳减排**不可见**
    
-   数据分散
    
-   核证成本极高
    
-   信任完全依赖中心机构
    

### 2️⃣ 数字资产真正赋能的“正确路径”

不是“把碳指标做成 Token”，而是：

> 把“可核证减排行为”变成可组合、可清算、可审计的数据资产

### 关键赋能点（研究重点）

-   MRV（监测、报告、核证）自动化
    
-   减排行为 → 数据 → 可交易权利
    
-   碳资产直接参与金融定价（抵押、融资）
    

### 3️⃣ 什么时候它不是庞氏？

-   收益来自：
    
    -   企业合规需求
        
    -   政策约束
        
    -   真实减排成本
        
-   而不是：
    
    -   投机买卖
        
    -   “碳概念叙事”
        

📌 **核心判断句（你可以直接写进研究里）**

> 碳数字资产的价值不是来源于“稀缺”，而是来源于“被强制需要”。

## 茅台：

### 1️⃣ 茅台的现实资产属性是什么？

-   强品牌溢价
    
-   稳定需求
    
-   稀缺供给
    
-   价格长期上涨预期
    

这正是**所有“资产上链”最危险的条件组合**。

* * *

### 2️⃣ 什么样的“茅台数字资产”是失败的？

❌ 把一瓶酒 Token 化

❌ NFT 表示一箱酒

❌ 二级市场自由炒作

这三点几乎**100% 走向庞氏**。

* * *

### 3️⃣ 那什么样的设计才可能“赋能现实”？（关键）

真正有意义的方向是：

> 不是把“酒”资产化，而是把“流通权、配额权、履约权”数据化

### 可能的真实赋能场景

-   防伪与溯源（减少假货）
    
-   配额管理（经销商体系透明化）
    
-   预售与库存金融（降低渠道融资成本）
    
-   税务与监管透明
    

注意：

> 价值来自“治理效率提升”，而不是价格上涨。

> 90% 的现实资产“不应该”被做成可自由交易的数字资产。

真正适合的，只有那些：

-   有强监管需求
    
-   有长期真实需求
    
-   有复杂治理结构
    
-   且现有制度成本极高的资产
    

👉 碳数据是

👉 医疗数据是

👉 供应链履约数据是

👉 茅台“本体”不是，但“渠道治理数据”可能是

明天先做碳资产的研究
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->













# 链上语言翻译为监管语言

## 一、怎么把“链上行为”翻译成“可监管语言”

### 先说结论（非常重要）

> 监管不是在监管“技术”， 而是在监管“可被制度识别的行为后果”。

所以翻译的核心不是：

-   “这个合约怎么写的”
    
-   “用了什么跨链桥”
    

而是三步映射：

> 技术行为 → 行为模式 → 制度风险语义

* * *

### 监管语言的最小结构（你必须内化）

任何一个合规判断，最低必须包含四个要素：

1.  **行为主体**：谁（地址 / 地址集）
    
2.  **行为事实**：做了什么（链上可验证）
    
3.  **风险语义**：触犯哪类监管关注
    
4.  **判断依据**：为什么不是偶发或正常行为
    

如果缺任何一项，**监管就不能采信**。

* * *

## 二、你需要掌握的知识与实践（不是泛泛而谈）

### （一）链上“事实层”知识（可验证）

这是**证据来源**，不是结论。

你必须熟悉：

-   交易频率、金额、时间分布
    
-   资产类型（稳定币 / 治理币 / NFT）
    
-   行为连续性（是否拆分、聚合）
    
-   跨链、跨协议路径
    

📌 **实践要求**：

你要能对任何一个地址，说出一句话：

> “这个地址在过去 X 天内，主要通过 A → B → C 路径转移资产。”

* * *

### （二）行为模式识别（结构化能力）

监管不关心“某一笔交易”，而关心**模式**。

常见可监管模式包括：

-   结构化拆分（structuring）
    
-   快速中转（layering）
    
-   非经济性行为（无合理收益逻辑）
    
-   关联地址协同
    

📌 **实践要求**：

你能把 100 笔交易，压缩成 **3–5 个行为特征指标**。

* * *

### （三）风险语义知识（制度层）

你至少要熟悉这些**不是技术的概念**：

-   AML / CFT
    
-   资金来源不明
    
-   规避审查（circumvention）
    
-   报告义务触发点
    
-   异常 vs 可疑（abnormal ≠ suspicious）
    

📌 **关键认知**：

监管判断的是**风险概率**，不是“定罪”。

* * *

## 三、标准示例：X → Y → Z 的完整合规表达

下面给你一个**完全合规可用的真实级别示例**。

* * *

### 示例 1｜典型“结构化洗钱风险”

> 在 X 条件下，触发 Y 风险类型，依据是 Z 行为模式

### X（触发条件）

-   某地址在 **72 小时内**
    
-   接收来自 **5 个以上新地址**
    
-   每笔金额均略低于 **10,000 USDT**
    
-   接收后 **15 分钟内全部转出**
    

### Y（风险类型）

👉 **结构化资金拆分风险（Structuring Risk）**

（规避交易监测与报告阈值）

### Z（行为模式依据）

-   金额高度接近阈值但未超过
    
-   多来源、短周期、高一致性
    
-   不符合正常投资或支付逻辑
    
-   转出路径集中，非消费型行为
    

### 合规表达（监管可用版本）

> “该地址呈现明显的结构化资金接收与快速中转特征，行为模式与规避交易监测阈值高度一致，构成中高等级反洗钱风险。”

⚠️ 注意：

**没有说“犯罪”，但风险已经成立。**

* * *

### 示例 2｜跨链规避风险（更高级）

### X（触发条件）

-   地址在 **单日内**
    
-   通过跨链桥将资产转移至 **3 个不同链**
    
-   每条路径金额接近一致
    
-   跨链后进入混合器或新协议
    

### Y（风险类型）

👉 **监管规避与资金混淆风险**

### Z（行为模式依据）

-   跨链行为缺乏经济必要性
    
-   多链拆分降低可追溯性
    
-   后续进入隐匿性协议
    
-   与正常 DeFi 套利路径不符
    

### 合规语言版本

> “该地址通过多链并行跨链行为显著降低资金可追溯性，且未体现合理经济目的，符合监管规避型资金流动特征。”

# 一、先点破关键：你为什么会“不知道”

你“不知道”的原因不是你不懂链上，而是：

> 你还在用“技术是否可追踪”的视角，而监管问的是“这种可追踪性是否足以支撑制度行动”。

这是两个完全不同的判断标准。

监管不问：

-   能不能追踪（技术问题）
    

监管问：

-   **追踪结果是否稳定、可解释、可复核、可用于决策**
    

* * *

# 二、L3 思考方式：不是“是不是匿名”，而是“四个条件”

一个合规专家在脑中会立刻拆成四个维度：

> 链上可追踪性是否“有效”，取决于是否同时满足以下四点：

* * *

## ① 行为是否具备**持续性**

-   单笔交易 → 几乎没有监管价值
    
-   **可重复、可聚类的行为 → 有监管价值**
    

👉 **连续性 = 可建模性**

* * *

## ② 行为是否具备**经济语义**

-   是否能解释为：投资 / 支付 / 套利 / 清算
    
-   还是：
    
    -   无收益
        
    -   无策略
        
    -   只有“转移本身”
        

👉 **没有经济逻辑 = 高风险候选**

* * *

## ③ 行为是否具备**跨地址关联性**

-   单一地址匿名性强
    
-   **地址集行为一致性 → 匿名性下降**
    

👉 监管几乎从不“看一个地址”，而是看**地址网络**

* * *

## ④ 行为是否能与**链下制度锚点连接**

例如：

-   交易所出入口
    
-   稳定币发行方
    
-   合规服务商
    

👉 **一旦连接链下锚点，匿名性急剧下降**

* * *

# 三、什么时候“有效”，什么时候“显著削弱”

* * *

> 链上地址的可追踪性在以下情况下对监管是有效的：
> 
> -   当地址行为具有时间上的连续性和模式稳定性；
>     
> -   当资金流动具备可识别的经济逻辑或明显缺乏经济合理性；
>     
> -   当地址之间存在可聚类的行为关联；
>     
> -   以及当链上路径能够连接至交易所、稳定币发行方等链下制度锚点时。
>     
> 
> 在这些条件下，链上行为可以被结构化、复核，并用于风险评估和监管决策。
> 
> 相反，当行为高度碎片化、随机化，且通过混合器、隐私协议或频繁跨链刻意破坏行为连续性时，链上可追踪性会被**显著削弱**，但这本身也可能构成一种监管规避风险信号。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->















| 周数区间 | 核心目标 | 核心输出 | | 第 1–2 周 | 坚实理解“链上地址与可审计性” | 5–7 页简报 | | 第 3–6 周 | 构建链上数据与行为分析基础能力 | 3 个分析报告 | | 第 7–10 周 | 对接合规与风险语义建模 | 风险指标体系与模型 | | 第 11–12 周 | 综合实践：完整链上合规方案设计 | 10–15 页方案 |

⸻

阶段一（第 **1–2 周）**

目标：扎实理解“链上地址非匿名 **& 可审计性是什么”**

主题 **A — 区块链基础与链上数据结构**

**必读与学习链接（入门清晰）**

• **Ethereum 官方文档：Accounts & Transactions**

[https://ethereum.org/en/developers/docs/accounts/](https://ethereum.org/en/developers/docs/accounts/)

• **区块链 101（简明解释）**

[https://www.blockchain.com/learning-portal/what-is-blockchain](https://www.blockchain.com/learning-portal/what-is-blockchain)

**实践任务**

1\. 在区块浏览器中查 10 个不同地址（交易所、合约、钱包）

2\. 判断地址类型：

• 是否为合约

• 是否为中心化交易所地址

• 是否为多签 / 智能合约管理

**知识笔记结构（建议撰写 5–7 页）**

• 键：什么是非匿名？

• 链上地址为何可审计？

• 地址行为如何被分类？

• 区块链与传统审计系统对比

**验证成果**

• 提交简报：

“链上地址为什么不是匿名？并举例说明如何在链上追踪行为。”

⸻

阶段二（第 **3–6 周）**

目标：掌握链上行为结构化与数据解析核心方法

⸻

第 **3–4 周主题｜链上数据解析与行为图谱构建**

技能点

• Transaction / Event / Log 结构

• Token Transfer 图谱

• 合约调用链

必学链接

• **Etherscan 教程（了解交易细节）**

[https://docs.etherscan.io/](https://docs.etherscan.io/)

• **ERC-20 / ERC-721 标准说明（技术规范）**

[https://ethereum.org/en/developers/docs/standards/tokens/](https://ethereum.org/en/developers/docs/standards/tokens/)

实践任务

1\. 对某 DeFi 协议

• 提取 20–30 笔交易

• 分解调用栈

2\. 绘制资金流向图（可用 Excel / Visio / Figma）

输出成果

• **行为分析报告 A**：

对某地址或协议资金流与功能调用图谱

⸻

第 **5–6 周主题｜链上风险标签与地址画像**

技能点

• 识别风险行为（如：合约交互异常、跨链桥异常）

• 地址画像方法

• 使用开源分析工具

推荐工具与链接

• **Dune Analytics（链上数据分析平台）**

[https://dune.com/](https://dune.com/)

• **Graph Protocol 基础**

[https://thegraph.com/docs/](https://thegraph.com/docs/)

实践任务

1\. 用 Dune 拉取某资产交易频率、资金移动

2\. 根据行为模式贴标签（如：高频转账、跨链活动）

输出成果

• **行为标签体系表**

• **行为分群与风险分析简报**

⸻

阶段三（第 **7–10 周）**

目标：与合规语义建模结合，形成“可解释风险模型”

⸻

第 **7–8 周主题｜合规语义与国家监管逻辑**

必学链接

• **FATF 虚拟资产监管指引**

[https://www.fatf-gafi.org/en/topics/virtual-assets.html](https://www.fatf-gafi.org/en/topics/virtual-assets.html)

• **OECD CARF 框架（加密资产报告）**

[https://www.oecd.org/tax/exchange-of-tax-information/crypto-asset-reporting-framework/](https://www.oecd.org/tax/exchange-of-tax-information/crypto-asset-reporting-framework/)

核心理解

• AML / CFT 基本架构

• 数据可报告事件定义

• 可审计风险指标（如：频率、规模、异常模式）

实践任务

• 根据上阶段行为标签

• 提炼符合 FATF 的风险指示器

• 形成风险语义表

输出成果

• **链上风险指标体系表**

• 指标、定义、触发条件、说明

⸻

第 **9–10 周主题｜风险模型与合规决策逻辑**

技能点

• 规则分层设计（高、中、低风险）

• 规则可编程化思考

• 模型解释性撰写

实践任务

1\. 设计一个“规则引擎”逻辑图

• 输入：行为特征

• 规则：风险条件

• 输出：风险等级／说明

2\. 手工模拟至少 10 个链上行为案例验证规则

输出成果

• **风险识别规则体系图**

• **案例验证表**

⸻

阶段四（第 **11–12 周）**

目标：完成“链上合规方案”综合设计与演示

⸻

第 **11–12 周｜系统化方案与演示文稿**

核心内容框架

1\. 背景与问题陈述

2\. 链上可审计性说明

3\. 行为结构化与风险标签

4\. 风险指数体系与规则

5\. 案例验证与规则解释

6\. 结论与政策建议

实践任务

• 撰写完整方案（10–15 页）

• 制作 12–15 页 PPT（可用于汇报）

验证成果

• 版本 _1_ 方案稿

• 沿用案例集 的逐条解释

• 政策沟通版 PPT

⸻

每周任务清单示例（具体可复制）

周 **1（示例）**

• 阅读：Ethereum 账户与交易

• 实践：查 10 个地址

• 笔记：写页说明“非匿名何以成为合规基础”

• 提交成果：1–2 页简报
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->

















主题：数据要素、链上数字资产与合规护城河的底层逻辑

**一、全球监管正在发生的根本变化**

近年来，各国在加密资产、稳定币、链上金融与数据治理上的立法密集出现，看似路径分化，但方向高度一致：

-   加密资产不再被视为“技术实验”
    
-   数据不再只是信息，而是生产要素
    
-   链上活动正在被纳入金融、税务、数据三重治理体系
    

MiCA、GENIUS Act、STABLE Act、CARF、DAC8、数据安全法、GDPR 等法规，本质上并不是为了“限制区块链”，而是为了把链上行为纳入国家可理解、可治理的制度框架。

**二、为什么数据要素成为关键变量**

**1\. 数据要素解决的不是技术问题，而是制度问题**

传统金融体系具备三项能力：

-   可解释（发生了什么）
    
-   可审计（是否合规）
    
-   可归责（谁负责）
    

而早期链上资产：

-   地址匿名
    
-   行为碎片化
    
-   权责难以界定
    

数据要素制度的出现，本质上是为了解决“如何治理非传统资产”的问题，这与链上资产面临的挑战高度同构。

**2\. 链上数字资产 = 数据资产的金融化形态**

从监管视角看：

-   钱包地址是行为数据容器
    
-   交易记录是可审计数据
    
-   NFT 是数字内容资产
    
-   RWA 是现实资产的数据映射
    

因此，链上资产是否“合规”，并不取决于是否上链，而取决于：

是否具备清晰的数据来源、使用权、责任边界与风险表达。

**三、为什么各国数据要素监管诉求不同**

**表面差异：法规不同**

**根本原因：国家治理模型不同**

钱包地址是行为数据容器

-   交易记录是可审计数据
    
-   NFT 是数字内容资产
    
-   RWA 是现实资产的数据映射
    

因此，链上资产是否“合规”，并不取决于是否上链，而取决于：

是否具备清晰的数据来源、使用权、责任边界与风险表达。

**三、为什么各国数据要素监管诉求不同**

**表面差异：法规不同**

**根本原因：国家治理模型不同**

差异并非分歧，而是以下因素的自然结果：

1.  经济结构不同（平台主导 vs 国家协调）
    
2.  法律传统不同（普通法 vs 成文法）
    
3.  风险排序不同（创新 / 权利 / 安全 / 稳定）
    
4.  国际竞争角色不同（规则输出者 vs 平台输出者）
    

因此不存在“全球统一合规路径”，只存在“可适配的合规模型”。

**四、税务合规与数据合规正在合流**

一个明确趋势是：

-   链上交易数据 = 税务数据
    
-   地址行为 = 潜在收入行为
    
-   跨链 ≠ 逃避申报
    

CARF、DAC8 等机制，正在把加密资产正式纳入：

-   跨境税务信息交换
    
-   国家税基保护体系
    

税务监管的目标并非“全面征税”，而是：

交易可见、收益可计算、责任可追溯。

**五、已经成型的合规工作方向**

在数据要素背景下，链上资产合规正在形成清晰路径：

1.  链上数据资产化  
    
    -   地址画像
        
    -   行为评分
        
    -   风险标签
        

3.  RWA 合规映射  
    
    -   法律权属 ≠ 链上凭证
        
    -   数据真实性与一致性成为核心
        

5.  可审计 DeFi / CeDeFi  
    
    -   白名单
        
    -   风险参数
        
    -   可报告机制
        

7.  合规稳定币  
    
    -   储备数据
        
    -   赎回数据
        
    -   用户行为数据
        

**六、唯一具备“必然性”的合规护城河**

经过剥离政策周期、地域差异与技术路线后，可以得出一个非常确定的判断：

真正不可绕过的合规护城河，是把链上行为转化为国家体系可理解、可审计、可归责的数据结构的能力。

这包括五个核心能力：

1.  行为结构化
    
2.  风险语义化
    
3.  权责映射
    
4.  可审计历史
    
5.  规则可编程
    

牌照、单点工具、单一隐私技术都不是护城河，合规数据基础设施才是。

**延伸学习与权威链接**

**国际与税务**

-   OECD CARF  
    [https://www.oecd.org/tax/exchange-of-tax-information/crypto-asset-reporting-framework/](https://www.oecd.org/tax/exchange-of-tax-information/crypto-asset-reporting-framework/)
    
-   欧盟 DAC8  
    [https://taxation-customs.ec.europa.eu/taxation-1/tax-cooperation-and-control/administrative-cooperation-taxation/dac8\_en](https://taxation-customs.ec.europa.eu/taxation-1/tax-cooperation-and-control/administrative-cooperation-taxation/dac8_en)
    

**加密资产与金融监管**

-   EU MiCA 官方页面  
    [https://www.esma.europa.eu/digital-finance/mica](https://www.esma.europa.eu/digital-finance/mica)
    
-   美国稳定币立法进展（Congress）  
    [https://www.congress.gov/](https://www.congress.gov/)
    
-   FATF 虚拟资产指引  
    [https://www.fatf-gafi.org/en/topics/virtual-assets.html](https://www.fatf-gafi.org/en/topics/virtual-assets.html)
    

**数据要素与数据治理**

-   GDPR 官方文本  
    [https://gdpr.eu/](https://gdpr.eu/)
    
-   EU Data Act  
    [https://digital-strategy.ec.europa.eu/en/policies/data-act](https://digital-strategy.ec.europa.eu/en/policies/data-act)
    
-   中国数据安全法（英文参考）  
    [https://digichina.stanford.edu/work/translation-data-security-law-of-the-peoples-republic-of-china/](https://digichina.stanford.edu/work/translation-data-security-law-of-the-peoples-republic-of-china/)
    

|   |   |
|   |   |
|   |   |
|   |   |
|   |   |
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->


















一、欧盟 **— MiCA（Markets in Crypto-Assets Regulation）**

主要内容与关键条款

**覆盖范围**

• 统一监管欧盟境内所有加密资产市场，包括公开发行的加密资产、交易服务机构和稳定币（资产参考代币、电子货币代币）。

**核心要求**

• **许可制度**：加密资产服务提供商（CASP）必须获得统一的欧盟级许可，允许跨境运营。

• **信息透明与风险披露**：发行人必须披露业务与风险信息，以提高市场透明度。

• **消费者与投资者保护**：规范营销、运营行为，设定稳定币储备与赎回规则。

• **监管技术标准**：欧盟监管机构发布一系列技术细则（Level 2/3 Measures）细化执行细则。

出台逻辑与政策诉求

• **构建单一市场**：解决各成员国规则不一致导致的合规碎片化，实现跨境监管一致性。

• **金融稳定性**：防止市场风险传导、提升投资者保护（例如对于稳定币的储备与赎回机制提出明确要求）。

• **创新与竞争**：在建立规则前提下，保持欧盟在数字金融领域的战略地位。

主要原因

欧盟认为缺乏统一监管会影响市场秩序，并可能带来系统性金融风险，因此以全面法规替代散碎监管体系。

⸻

二、美国 **— GENIUS Act 及 STABLE Act 等**

**GENIUS Act（U.S. Stablecoin Regulation）**

**关键条款**

• **稳定币法律地位明确**：将稳定币纳入联邦监管体系，明确其作为支付工具的规则与要求。

• **全储备要求**：发行方必须保持1:1的储备资产（美元或美债等高质量资产）。

• **消费者保护与AML/KYC**：引入反洗钱、消费者保护机制，要求发行方建立内部控制系统。

**STABLE Act / 其他法案（进程与方向）**

• STABLE Act 侧重稳定币透明度与问责机制（例如储备透明与报告义务）。

• 美国其他相关立法提案也在讨论加密资产市场结构、监管分工（SEC vs CFTC）等问题，但整体流程在国会推进中仍有变动。

出台逻辑与政策诉求

• **金融主权保护**：避免稳定币生态对美元金融体系与货币主权造成冲击。

• **系统性风险管理**：防止稳定币在金融危机时出现兑付风险或大规模挤兑。

• **投资者与消费者保护**：填补现有监管空白，减少诈骗、市场滥用风险。

主要原因

美国长期监管碎片化（联邦与州多层监管体系）导致不确定性，推动国会制定统一联邦性规则以提升市场稳定性与国际竞争力。

⸻

三、新加坡 **— MAS 主导框架及支付服务法规**

核心法律与框架

• **《支付服务法》 Payment Services Act (PSA) 2019** 是监管基础，涵盖数字支付令牌服务（包括加密资产交易等）。

• **稳定币监管框架**：新加坡金管局（MAS）制定稳定币法规，要求发行方满足储备、稳定性与赎回规则。

主要监管要素

• **许可制度**：所有数字支付令牌服务与稳定币发行需获 MAS 授权。

• **FATF 标准接轨**：严格执行 KYC/AML/CFT 规则，含跨境交易Travel Rule。

• **安全与运营标准**：包括网络安全、资产隔离、客户保护等要求。

出台逻辑与政策诉求

• **安全可控创新环境**：在保护金融稳定与消费者安全的同时，维持新加坡作为金融科技枢纽的吸引力。

• **国际合作与标准一致性**：与 FATF 等国际标准保持一致，提高跨境互认性。

主要原因

新加坡强调在开放创新与全球监管标准之间找到平衡，同时避免过去因高风险产品监管不足带来的市场冲击。

⸻

四、香港 **& 其他亚洲市场**

香港《稳定币条例》

• 引入稳定币发行许可制度，要求在港设立实体、充足资本与储备隔离（1:1 现金等价值资产）。

日本、韩国、澳大利亚等

• 日本与韩国推进银行资本要求、稳定币监管框架与用户保护法规。

• 澳大利亚与瑞士等已将 MiCA 经验借鉴至本地框架。

出台逻辑与诉求

• **吸引国际资本与创新生态**（香港、迪拜等积极引入规则）。

• **结合区域金融市场特点制定本地规则** 与国际标准接轨。

⸻

五、国际标准框架与趋势

**FATF / FSB 标准**

• 大多数国家按\*\*金融行动特别工作组（FATF）\*\*要求执行 Travel Rule、反洗钱与反恐怖融资标准。

• **金融稳定委员会（FSB） Crypto Framework** 提供高层次监管原则，推动跨境合作与一致性。

全球趋势

• **从宽松试验到全面监管**：过去“沙盒+创新试点”逐步转向全面许可制度与风险管理法规。

• **稳定币成为监管核心**：各国集中在稳定币储备、赎回与风险隔离机制建立明确标准。

| 地区/国家 | 代表法规 | 关键条款核心 | 核心诉求 | 推动原因 |
| 欧盟 | MiCA | CASP 许可、稳定币储备与披露、投资者保护 | 统一市场、风险控制 | 市场一体化、金融稳定 |
| 美国 | GENIUS Act / STABLE Act | 稳定币联邦监管、全储备要求、消费者保护 | 明确联邦规则、系统性风险管理 | 监管碎片化需统一 |
| 新加坡 | PSA + 稳定币框架 | 许可制度、FATF 接轨、运营安全 | 创新与安全平衡 | 打造国际金融中心 |
| 香港 | 稳定币条例 | 本地许可、资本 & 储备要求 | 吸引创新、规范市场 | 成为数字金融中心 |
| 其他国家 | 各自框架 | 资本要求、用户保护等 | 风险控制与市场发展 | 各自政策与金融体系需求 |

下面从\*\*“税务合规”**与**“数字资产 / 数据要素”**两个维度，对当前主要国家和国际层面的**核心条例、监管逻辑与制度诉求\*\*进行系统性梳理，便于你直接用于合规分析、白皮书或对外分享材料。

⸻

一、涉及加密资产 **/ 数字资产的税务合规条例**

（一）国际层面：统一税务信息交换框架

**1\. OECD — CARF（Crypto-Asset Reporting Framework）**

**适用范围**

• 加密货币

• 稳定币

• 部分 NFT（具备支付或投资属性）

• DeFi 中可识别的中介服务

**关键条款**

• 要求加密资产服务提供商（CASP）：

• 识别用户税务居民身份

• 记录并上报交易、余额、转移信息

• 跨境自动信息交换（类似 CRS）

**立法逻辑与诉求**

• 防止通过加密资产进行**跨境逃税**

• 将加密资产纳入全球税收透明体系

• 与传统金融税务规则“同责同标”

**本质判断**

CARF 的目标不是限制交易，而是“让税务看得见”。

⸻

（二）欧盟

**1\. DAC8（第八版《行政合作指令》）**

**定位**

• 欧盟版 CARF

• 与 MiCA 并行实施

**关键要求**

• CASP 向成员国税务机关报告：

• 用户身份

• 交易金额

• 加密资产类型

• 成员国之间自动交换数据

**政策逻辑**

• 防止欧盟内部税基流失

• 配合 MiCA，形成“金融 + 税务”闭环监管

⸻

（三）美国

**1\. IRS 加密资产税务规则**

**核心原则**

• 加密资产被认定为 **“财产（Property）”**，而非货币

**税务触发场景**

• 买卖（资本利得税）

• 挖矿 / Staking（所得税）

• 空投、奖励、利息类收入

**2\. Infrastructure Investment and Jobs Act（2021）**

**关键条款**

• 扩大 “Broker” 定义

• 要求部分加密服务方提交 1099 表

**监管诉求**

• 填补税收漏洞

• 强制建立可追溯的申报链条

⸻

（四）新加坡 **/ 香港（代表性金融中心）**

**新加坡**

• **不对资本利得征税**

• 若被认定为“商业交易行为”，需缴纳所得税

• GST（增值税）已豁免大多数数字支付代币

**香港**

• 区分：

• 投资性持有（一般不征税）

• 经营性活动（需缴利得税）

• 近年加强对虚拟资产平台税务披露要求

**共同逻辑**

税务合规 ≠ 全面征税，而是“是否构成经营与收入”。

⸻

二、数字资产 **/ 数据要素的核心条例体系**

这里的“数字资产”不仅指加密货币，也包括**数据、算法、算力、数字内容等新型生产要素**。

⸻

（一）欧盟：数字经济的制度化代表

**1\. GDPR（通用数据保护条例）**

**核心定义**

• 个人数据属于“受保护的经济资源”

**关键要求**

• 数据可携带权

• 使用合法性与最小化原则

• 数据处理需有法律基础

**逻辑**

• 将数据确认为“可被权利化管理的资产”

⸻

**2\. Data Act & Data Governance Act（DGA）**

**重点突破**

• 明确：

• 数据所有权 ≠ 数据使用权

• 鼓励数据共享与交易市场

**适用对象**

• 工业数据

• IoT 数据

• 平台生成数据

**政策诉求**

• 激活数据要素流通

• 防止平台垄断数据红利

⸻

**3\. DLT Pilot Regime**

• 允许使用区块链进行：

• 证券发行

• 清结算

• 为“链上资产证券化”提供合规实验空间

⸻

（二）中国（数据要素制度化最激进）

**1\. 《数据安全法》《个人信息保护法》**

• 明确数据的**国家安全属性**

• 建立数据分类分级制度

**2\. 数据要素市场化政策**

• 数据确权

• 数据定价

• 数据交易所（北数所、深数所）

**制度逻辑**

将数据正式纳入生产要素，与土地、资本、劳动力并列。

⸻

（三）美国：偏市场化、弱确权

• 无统一“数据法”

• 通过：

• 行业法（HIPAA、GLBA）

• 州法（CCPA）

• 数据更像“合同项”，而非独立资产

⸻

三、加密资产 **× 税务 × 数据的交叉监管趋势**

**1\. 三重合规正在合流**

| 维度 | 核心问题 |
| 金融合规 | 是否洗钱、是否非法融资 |
| 税务合规 | 是否申报、是否跨境逃税 |
| 数据合规 | 用户数据如何收集、存储、共享 |

**2\. 对机构的实际影响**

• KYC 数据 = 税务数据 + 数据合规对象

• 链上地址 ≠ 匿名资产，而是“准实名金融账户”

• 跨链、DeFi 操作正在被纳入“可报告范围”

⸻

四、一句话总结（可直接引用）

全球监管正在将加密资产从“技术实验”重新定义为**可征税、可监管、可确权的数字经济资产**；

税务合规解决的是“国家收益权”，

数据要素条例解决的是“生产要素配置权”，

二者共同指向的是——**加密与数字资产全面进入主流制度体系**。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->





















### Web3 合规与安全：我们到底在“管”什么？

* * *

### 一、从一个最关键的问题开始：谁在控制资产？

在传统金融里，我们习惯这样理解控制权：

-   银行控制账户系统
    
-   用户通过身份和密码使用
    
-   出问题可以冻结、回滚、追责
    

但在 Web3 世界，控制权的逻辑是完全不同的。

**这里有一个核心事实：**

> 区块链系统并不认识“你是谁”，
> 
> 它只认识——**谁能完成加密签名**。

换句话说：

-   **谁掌握私钥，谁就控制资产**
    
-   法律身份、公司身份、自然人身份，本身不自动产生控制权
    

这是理解一切合规问题的起点。

* * *

### 二、账户并不等于“人”，而是一种权限结构

在 Web3 中，账户主要分为两类：

**第一类：个人控制账户**

-   类似一个“只认密码、不认身份”的账户
    
-   可以主动发起操作
    
-   一旦私钥泄露，资产不可逆转
    

**第二类：合约控制账户**

-   没有任何人能直接“操作”
    
-   一切行为由预先写好的规则自动执行
    
-   规则写错，结果照样发生
    

这意味着一个非常重要的合规现实：

> 在 Web3 中，
> 
> **责任主体、控制主体、技术执行主体，经常不是同一个。**

* * *

### 三、为什么说 Web3 不是“匿名”，而是“假匿名”？

> Web3 是身份可隐藏，但行为完全公开的系统。

-   每一笔交易、每一次资产转移，永久记录
    
-   所有合约规则对外公开
    
-   不可篡改，但可以被持续分析
    

这也是为什么：

-   区块链**并不天然反监管**
    
-   相反，它非常适合做**事后审计与穿透式分析**
    

监管真正面对的挑战不是“看不见”，而是：

-   **如何把链上行为和现实世界的主体连接起来**
    

* * *

### 四、安全在 Web3 中，更多是“结构问题”，而不是“技术水平问题”

在传统系统中，我们谈安全，往往谈的是：

-   防火墙
    
-   权限管理
    
-   系统漏洞
    

而在 Web3 中，更关键的是三个结构性设计：

### 1️⃣ 权限分散（多签）

-   重要操作不由一个人决定
    
-   多方共同确认
    
-   可追溯、可审计
    

### 2️⃣ 延迟执行（时间锁）

-   关键操作不会立刻生效
    
-   给市场和参与者反应时间
    
-   把“程序正义”写进系统
    

### 3️⃣ 规则透明（合约公开）

-   所有规则事前可见
    
-   事后不可抵赖
    
-   风险前置，而不是事后解释
    

这些机制，本质上是在回答监管最关心的三个问题：

-   有没有内部人作恶空间？
    
-   决策过程是否可证明？
    
-   风险是否可被提前发现？
    

* * *

### 五、技术并不能解决所有合规问题

这里必须非常清醒地说一句：

> 不存在“写得足够好就一定合规的智能合约”。

原因很简单：

-   合规是社会规则
    
-   技术只能执行规则，不能判断合理性
    
-   风险的定义，随政策、司法和现实环境变化
    

所以我们看到的现实是：

-   技术在降低某些风险
    
-   同时也在引入新的风险形态
    

这并不是失败，而是**新系统必然经历的阶段**。

* * *

### 六、对监管与合规的一个现实判断

如果用一句话总结 Web3 合规的技术底色，那就是：

> Web3 并不是在消灭监管，
> 
> 而是在改变监管介入的“位置”和“方式”。

-   从事前审批 → 更多事后审计
    
-   从中心控制 → 对关键节点监管
    
-   从“相信人” → 更多依赖结构和证据
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->























# 从零到一学习以太坊

相比于昨天的实习手册，今天多了很多详细的东西，确定了之后我想学习的重点主要是在nft和dao方向。

这两者对于我来说都是一体的，最终都需要链接真实世界才能真正产生价值锚定或者价值增值。

另外今天参加co-learning的时候信号不好但是也听到了一些大家关于dao治理的讨论。所以当时写了一点笔记。

大家在讨论DAO 治理问题，在地铁上网络不好断断续续的。凭借我听到短暂的讨论，我还是同意小海老师说的能活下来的就是好的。没有什么组织是完美的。

我赞同无为而治的原因并非是信任优秀的组织者和领导者，而是不相信能有任何领导者能为组织提供更加民主的未来。况且在不同的组织文化中生长出来的民主也是不同的。 有时候摧毁进步的可能不是保守的势力，很有可能就是完美的乌托邦理想，不要让最好成为更好的敌人。 虽然我对如何治理组织没有任何概念，但我相信这是一个有机的过程，尽管人类有各种弱点，但是勇气与爱会带我们找到方向～

今天的web3运行原理内容也很多需要慢慢消化
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

























# **实习手册学习笔记**

## 一、区块链定义

本质上就是一种账本技术，只是这种账本是通过技术将原来金融体系下的中心化账本技术变为分布式账本技术。传统金融范式下所以的信息交互、审计、保荐等也许在分布式账本技术推广以后所有的信任建立方式会有相应的变革。本质上是陌生人交往中的信任建立方式改变。

陌生人信任方式不仅仅涉及金融领域、还有方方面面的各类经济形态、只是金融太依赖于信任了。其他之后再展开不然就偏题了。

当然区块链想要达到的理想世界是理想的技术方式，其中的安全性和信任感来自于排除不诚实节点的技术。但是如何真的能够保护诚实节点，让不诚实节点受到处罚是一个动态的过程，涉及到激励等等，但具体的技术太复杂看不懂。这里可能需要补充博弈论的知识，在我看来还是涉及到很多社会科学学科的成果能够帮助技术的发展。

## 二、比特币

比特币是我了解各类数字货币中更保护自己货币属性的币种。听说现在比特币的交易费越来越低，对于矿工的奖励降低可能导致矿工出走，许多人可能会想要在链上加更多应用让其中的贡献者有利可图，但这可能会损失比特币货币属性的核心地位或者损失安全性？后续如何需要补足笔记……

数字鸿沟、钓鱼攻击

我目前还没有一个固定的信息源能够获得这方面的信息。比如助记词的设计方式，理论上来说助记词的设计好像是2的256次方，理论上是无法被破解的，但是如果设计比较简单，可能破解的概率就是在2的32次方内进行，但是我不理解为什么助记词的设计会有这样的区别，如何能够让自己助记词更难被破解。这样的类似的信息很重要但我完全不知道。钓鱼攻击好可怕。

### [**扩展阅读**](https://web3intern.xyz/zh/security/#%E6%89%A9%E5%B1%95%E9%98%85%E8%AF%BB)

1.  区块链安全红手册（项目方）：[\*\*](https://www.slowmist.com/redhandbook/)[https://www.slowmist.com/redhandbook/\*\*](https://www.slowmist.com/redhandbook/**)
    
2.  区块链黑暗森林自救手册：[\*\*](https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook/blob/main/README_CN.md)[https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook/blob/main/README\_CN.md\*\*](https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook/blob/main/README_CN.md**)
    
3.  [Revoke.cash](http://Revoke.cash) 授权管理：[\*\*](https://revoke.cash/)[https://revoke.cash/\*\*](https://revoke.cash/**)
    
4.  Scam Sniffer 钓鱼检测：[\*\*](https://www.scamsniffer.io/)[https://www.scamsniffer.io/\*\*](https://www.scamsniffer.io/**)
    
5.  1Password 密码管理器：[\*\*](https://1password.com/)[https://1password.com/\*\*](https://1password.com/**)
    
6.  Bitwarden 密码管理器：[\*\*](https://bitwarden.com/)[https://bitwarden.com/\*\*](https://bitwarden.com/**)
    
7.  SlowMist Hacked 区块链被黑档案库，大量的案例索引：[\*\*](https://hacked.slowmist.io/)[https://hacked.slowmist.io/\*\*](https://hacked.slowmist.io/**)
    
8.  反钓鱼攻防学习平台：[\*\*](https://unphishable.io/)[https://unphishable.io/\*\*](https://unphishable.io/**)
    

通常我们会安装一些浏览器扩展或者插件来增强浏览体验。实际上浏览器插件具备的权限比较高，可以修改网页代码替换、篡改正常的合约，从而实现钓鱼攻击。

**应对方式：**

-   新开一个 Chrome Profile 仅仅安装 MetaMask 插件，不安装其他插件。需要使用钱包交互的时候，使用这个 Chrome Profile 进行操作。更多关于 Chrome Profile 的用法，请参考：[Share Chrome with others](https://support.google.com/chrome/answer/2364824)
    

分散资产、硬件钱包、冷钱包、资金管理

推荐 [区块链黑暗森林自救手册](https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook)、[Web3 生存指南之防骗反诈安全手册V1.0](https://e7qjl676i8.feishu.cn/docs/doccn2rvEMHefBYKvyTVRGwe7Pf#)。

### [**防护清单（学生版）**](https://web3intern.xyz/zh/security/#_3-%E9%98%B2%E6%8A%A4%E6%B8%85%E5%8D%95-%E5%AD%A6%E7%94%9F%E7%89%88)

1.  **面试/实习相关**
    
    -   只用官方 Zoom/Teams/腾讯会议等公开工具，拒绝安装任何“专用面试软件”
        
    -   核实面试邀请邮箱、域名、联系人，多渠道确认
        
    -   遇到“提前安装软件”“下载资料包”等要求，务必警惕，先查官网/Google/知乎/Reddit 等是否有安全警告
        
2.  **奖学金/空投/福利活动**
    
    -   任何要求“连接钱包”“签名验证”“输入助记词/私钥”的网页，99% 是骗局
        
    -   空投、福利只用专门测试钱包，主钱包冷存储，绝不轻易暴露
        
    -   收到“缴费”“奖学金调整”类消息时，先拨打官网热线复核；校园通知留意域名是否官方主域。
        
3.  **社交平台/群聊**
    
    -   不轻信群内“官方人员”“管理员”“学长学姐”的私聊和链接
        
    -   遇到“帮忙测试”“转发链接”“紧急转账”等请求，务必多方核实
        
4.  **软件/插件安装**
    
    -   所有软件、插件只从官网、官方应用商店下载
        
    -   安装前先查口碑、用户数、是否有安全警告
        
    -   浏览器插件控制在 3 个以内，且只用官方钱包、密码管理器、广告屏蔽器
        
    -   面试／竞赛前索要安装包时，要求公开 MD5 与官网对比；核查数字签名颁发者和时间戳。
        
    -   开启系统防火墙、浏览器插件权限管理；对摄像头与麦克风按需授权。
        
5.  **账号与密码安全**
    
    -   重要账户启用 2FA（短信易被劫持，优选谷歌验证或硬件密钥）；不要在同一浏览器里保存助记词与日常 Cookie。
        
    -   使用密码管理器（如 1Password、Bitwarden），每个平台设置独立强密码
        
    -   邮箱、手机号安全重于一切，定期更换密码，防止被劫持
        
6.  **钱包与转账**
    
    -   转账前务必核对地址前 6 位和后 4 位，防止剪贴板被劫持
        
    -   助记词/私钥只离线保存，绝不截图、上传网盘或发给任何人
        
    -   定期检查钱包授权，及时取消不必要的授权（可用 [Revoke.cash](http://Revoke.cash)、Etherscan 等工具）
        

**重要**

遇到疑似攻击第一时间断网、截屏、拍照、记录时间点并寻求校内信息中心或专业社群帮助。

## 三、公链、联盟链和私链

想象中这三种链是未来链上资产真的发展起来后人与人之间交往的媒介。本质上人与人之间的经济活动传统上的个体经济是受限于生产力，等生产力提高以后、公司制的出现是绝大部分个人参与经济活动更加高效的方式，因为交往的范围更大更复杂、集中式管理的团体更能在经济活动竞争中获得胜出。但是当AI出现以后个体的生产力和灵活度获得提高，并且更多垂直的市场需求出现，导致个人未来可能就是个人公司或者同时服务很多经济体、是一个放射性的经济交往方式。

从这个角度看私链最后就是一个个结构点产生的比较稳定的利益或者协作团体、联盟链则是这些私链协作的空间、公链是一个公共资源、公共利益实现的地方，希望比特币能够一直保护好自己的初衷

能够做社区治理的人真的很厉害，崇拜，从前逛公园的时候从未想过管理公园的人对大家有什么想象

可能即使是在区块链技术中，中心化的组织或决策可能仍然是比较常见的形式，不过未来中心化组织中的领导者需要具备的领导力和现在管理学中的领导力可能有很大的差异，虽然现在的也没学过，猜测

web3.0和web3这种区别的核心是是否有智能合约吗？智能合约确实是陌生人交往中新任建立很重要的技术。

## 四、我的第一个nft

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/huanglaifa0-sys/images/2026-01-12-1768227707631-image.png)

可爱

## 五、密码朋克

这个太感兴趣了，想要深入研究一下，之后详细些

### [**扩展阅读**](https://web3intern.xyz/zh/overview-of-ethereum/#%E6%89%A9%E5%B1%95%E9%98%85%E8%AF%BB)

-   [**MyFirstLayer2**](https://layer2.myfirst.io/)：使用图文、动画的方式，帮助 Web3 的用户更好、更安全地理解 Layer 2 是什么
    
-   [**以太坊密码朋克宣言**](https://hackmd.io/@pcaversaccio/the-ethereum-cypherpunk-manifesto)：深入了解以太坊的文化内核和价值观体系
    

## 六、Dao治理

等待研究

## 七、[**全球监管背景与趋势**](https://web3intern.xyz/zh/security/#_2-%E5%85%A8%E7%90%83%E7%9B%91%E7%AE%A1%E8%83%8C%E6%99%AF%E4%B8%8E%E8%B6%8B%E5%8A%BF)

合规条例、各地的、等待研究
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
