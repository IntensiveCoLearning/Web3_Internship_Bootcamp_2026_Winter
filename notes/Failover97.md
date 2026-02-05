---
timezone: UTC+8
---

# Failover97

**GitHub ID:** Failover97

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-02-05
<!-- DAILY_CHECKIN_2026-02-05_START -->
### 今天完成了常见的合约漏洞的最后一种类型

### 使用tx.origin

solidily提供了两种机制来识别调用方： msg.sender tx.origin

msg.sender 直接调用合约者的地址

tx.origin 完整调用链的最初的发送方

tx.origin phising liquidator—>Manager—>LendingProtocol

```jsx
pragma solidity ^0.8.0;

interface ILendingProtocol {//外部接口名称，约定以I开头 调用别人函数的接口
    function liquidate(address user) external;
}

contract LiquidationManager {
    address public authorizedLiquidator;
    ILendingProtocol public lendingProtocol;

    constructor(address _liquidator, address _lendingProtocol) {
        authorizedLiquidator = _liquidator;//授权清算员
        lendingProtocol = ILendingProtocol(_lendingProtocol);//借贷合约地址
    }

    // @audit tx.origin for authorization
    function performLiquidation(address _user) external {
        require(tx.origin == authorizedLiquidator, "Not authorized");
        lendingProtocol.liquidate(_user);
    }
}
```

问题出在合约检查的是最初的调用者，而不是这个合约的发起者。所以只要用户授权了恶意合约作为proxy,恶意合约调用了这个清算合约的话，就会产生很大的问题。

```jsx
contract PhishingWrapper {
    LiquidationManager public manager;
    address public targetUser;

    constructor(address _manager, address _targetUser) {
        manager = LiquidationManager(_manager);
        targetUser = _targetUser;
    }

    function bait() external {
        manager.performLiquidation(targetUser);
    }
}
如果授权清算人调用bait()函数，恶意合约调用清算合约后，授权清算人会产生法律风险
```
<!-- DAILY_CHECKIN_2026-02-05_END -->

# 2026-02-03
<!-- DAILY_CHECKIN_2026-02-03_START -->

```
NIST 网络安全框架 (CSF)
         ↓
    五大核心功能
         ↓
┌────────────────────────────────────────────┐
│ Identify → Protect → Detect → Respond → Recover │
│  识别       保护      检测      响应      恢复   │
└────────────────────────────────────────────┘
         ↓
   风险评估属于 Identify（识别）阶段
         ↓
   具体控制措施在 SP 800-53 的 RA 家族
```

**关键理解**：风险评估是所有安全工作的起点。你得先知道有什么风险，才能决定怎么保护。这就是为什么它在 Identify 阶段。

对 Web3 的启发：很多项目上线就被黑，就是因为跳过了风险评估，直接开干。

* * *

## 什么是风险？

### 官方定义

NIST SP 800-30 的定义：

> "Risk is a measure of the extent to which an entity is threatened by a potential circumstance or event, and typically is a function of: (i) the adverse impacts that would arise if the circumstance or event occurs; and (ii) the likelihood of occurrence."

翻译成人话：**风险 = 坏事发生的可能性 × 坏事造成的影响**

### 2.2 风险的三要素

```
        ┌─────────────┐
        │   威胁源     │  ← 谁/什么想搞你？
        │   Threat    │     (黑客、内鬼、自然灾害)
        │   Source    │
        └──────┬──────┘
               │
               ↓ 利用
        ┌─────────────┐
        │   漏洞       │  ← 你的弱点在哪？
        │Vulnerability│     (代码bug、配置错误、人员疏忽)
        └──────┬──────┘
               │
               ↓ 造成
        ┌─────────────┐
        │   影响       │  ← 出事了会怎样？
        │   Impact    │     (丢钱、数据泄露、名誉损失)
        └─────────────┘
```

**三者缺一不可**：

-   有威胁但没漏洞 → 攻不进来，风险低
    
-   有漏洞但没威胁 → 没人攻击，风险低
    
-   有威胁有漏洞但影响小 → 就算被攻击损失也不大
    

> Web3 案例理解：
> 
> -   威胁源：朝鲜黑客组织 Lazarus Group
>     
> -   漏洞：智能合约的重入漏洞
>     
> -   影响：协议 TVL 全部被盗（比如 $600M）
>     
> 
> 三个都具备 = 极高风险，这就是为什么 DeFi 审计这么重要

### 2.3 风险 vs 威胁 vs 漏洞（容易混淆！）

| 概念 | 定义 | Web3 例子 |
| --- | --- | --- |
| 威胁 Threat | 可能造成伤害的潜在原因 | 闪电贷攻击、私钥泄露、51%攻击 |
| 漏洞 Vulnerability | 可被利用的弱点 | 重入漏洞、整数溢出、权限控制缺失 |
| 风险 Risk | 威胁利用漏洞造成损失的可能性和程度 | "该合约存在高风险，因为存在重入漏洞且合约持有大量资金" |

* * *

## 三、NIST SP 800-53 风险评估控制家族详解

SP 800-53 把安全控制分成了 20 个"家族"，风险评估（RA）是其中之一。

### 3.1 RA 家族完整控制清单

| 编号 | 名称 | 重要程度 | 详细说明 |
| --- | --- | --- | --- |
| RA-1 | 政策与程序 | ⭐⭐⭐ | 组织必须制定书面的风险评估政策，明确目的、范围、角色职责、管理承诺、协调机制，并定期审查更新 |
| RA-2 | 安全分类 | ⭐⭐⭐⭐ | 对信息和系统按照机密性、完整性、可用性进行分类分级，确定保护优先级 |
| RA-3 | 风险评估 | ⭐⭐⭐⭐⭐ | 核心控制！执行风险评估的具体要求 |
| RA-5 | 漏洞监控与扫描 | ⭐⭐⭐⭐⭐ | 定期扫描漏洞、分析结果、修复问题 |
| RA-6 | 技术监控报告 | ⭐⭐ | 使用技术监控报告辅助风险评估 |
| RA-7 | 风险响应 | ⭐⭐⭐⭐ | 对识别出的风险采取应对措施 |
| RA-8 | 隐私影响评估 | ⭐⭐⭐ | 评估对个人隐私的影响 |
| RA-9 | 关键性分析 | ⭐⭐⭐⭐ | 识别关键系统组件及其功能 |
| RA-10 | 威胁猎捕 | ⭐⭐⭐ | 主动搜索潜伏的威胁 |

### 重点控制详解

RA-2 安全分类（Security Categorization）

**为什么重要**：你不可能保护所有东西到同一级别，资源有限，必须分优先级。

**分类依据**（CIA 三要素）：

| 要素 | 英文 | 含义 | 低/中/高影响示例 |
| --- | --- | --- | --- |
| 机密性 | Confidentiality | 信息不被未授权访问 | 低：公开信息 / 中：内部文档 / 高：私钥 |
| 完整性 | Integrity | 信息不被未授权篡改 | 低：宣传文案 / 中：用户数据 / 高：合约代码 |
| 可用性 | Availability | 信息需要时可访问 | 低：历史归档 / 中：官网 / 高：交易系统 |

**分类公式**：

```
系统安全分类 = max{机密性影响, 完整性影响, 可用性影响}
```

举例：一个系统机密性=中，完整性=高，可用性=中，那系统整体分类=高

> Web3 应用：
> 
> 对一个 DeFi 协议做分类：
> 
> | 资产 | 机密性 | 完整性 | 可用性 | 最终分类 |
> | --- | --- | --- | --- | --- |
> | 智能合约代码 | 低（开源） | 高 | 高 | 高 |
> | 管理员私钥 | 高 | 高 | 中 | 高 |
> | 前端界面 | 低 | 中 | 中 | 中 |
> | 用户交易历史 | 中 | 中 | 低 | 中 |
> | 协议文档 | 低 | 低 | 低 | 低 |

* * *

RA-3 风险评估（Risk Assessment）— 核心中的核心

**RA-3 的具体要求**（NIST 原文精简）：

组织必须：

1.  对系统和信息进行风险评估，考虑来自外部/内部的威胁造成的危害
    
2.  评估上述危害发生的**可能性（Likelihood）**
    
3.  评估一旦发生的**影响程度（Impact）**
    
4.  评估组织运营、资产和个人的风险
    
5.  记录风险评估结果，包括支持文档
    
6.  定期审查和更新风险评估（周期根据系统分类确定）
    
7.  在系统重大变更后重新评估
    
8.  将结果传达给相关人员
    

**完整的风险评估流程**（我根据 SP 800-30 整理的）：

```
┌─────────────────────────────────────────────────────────────────┐
│                    风险评估四阶段                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  │   准备      │ →  │   执行      │ →  │   沟通      │ →  │   维护      │
│  │  Prepare   │    │  Conduct   │    │Communicate│    │  Maintain  │
│  └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
│        │                  │                  │                  │
│        ↓                  ↓                  ↓                  ↓
│   - 确定目的          - 识别威胁源        - 编写报告        - 监控风险因素
│   - 确定范围          - 识别威胁事件      - 传达结果        - 更新评估
│   - 识别假设/约束     - 识别漏洞          - 分享信息        - 定期复查
│   - 识别信息来源      - 确定可能性                        
│   - 定义风险模型      - 确定影响程度                       
│                      - 确定风险等级                       
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**阶段一：准备（Prepare）详解**

| 任务 | 具体内容 | Web3 场景举例 |
| --- | --- | --- |
| 确定目的 | 为什么做这次评估？ | "上线前审计"、"年度安全复查"、"出事后复盘" |
| 确定范围 | 评估什么？ | 单个合约 / 整个协议 / 包含前端和后端 |
| 识别假设 | 有哪些前提条件？ | "假设用户会保管好自己的私钥" |
| 识别约束 | 有什么限制？ | 时间、预算、无法获取某些信息 |
| 信息来源 | 数据从哪来？ | 代码库、链上数据、历史攻击案例 |
| 风险模型 | 用什么方法论？ | STRIDE、DREAD、攻击树 |
<!-- DAILY_CHECKIN_2026-02-03_END -->

# 2026-02-01
<!-- DAILY_CHECKIN_2026-02-01_START -->


## 一、核心术语对照表

| 术语 | 中文理解 | 含义 |
| --- | --- | --- |
| Repository (Repo) | 仓库 | 项目的所有文件和历史记录的存储位置 |
| Clone | 克隆 | 把远程仓库完整复制到本地电脑 |
| Fork | 派生/复刻 | 把别人的仓库复制一份到你的 GitHub 账号下 |
| Commit | 提交 | 保存一次代码修改的快照（本地操作） |
| Push | 推送 | 把本地的 commit 上传到远程仓库 |
| Pull | 拉取 | 把远程仓库的最新代码下载到本地 |
| Branch | 分支 | 代码的平行版本，用于独立开发功能 |
| Merge | 合并 | 把一个分支的改动合并到另一个分支 |
| Pull Request (PR) | 拉取请求 | 请求把你的代码合并到主仓库 |
| Issue | 议题 | 用于讨论 bug、功能请求等 |
| Main/Master | 主分支 | 项目的主要代码分支 |
| Remote | 远程仓库 | 托管在 GitHub 上的仓库 |
| Local | 本地仓库 | 你电脑上的仓库副本 |

## 二、工作流程对比

### 个人项目流程

`修改代码 → Commit (保存) → Push (上传)`

### 团队协作流程

`1. Clone/Pull (获取最新代码) 2. 创建新分支 (Branch) 3. 修改代码 4. Commit (本地保存) 5. Push (推送到远程) 6. 发起 Pull Request 7. 代码审查 (Code Review) 8. 合并 (Merge)`

* * *

## 三、团队协作实战指南

### 场景 1：你是团队成员（有写权限）

### 第一次参与项目

bash

\`# 1. 克隆团队仓库到本地 git clone [https://github.com/团队名/项目名.git](https://github.com/团队名/项目名.git) cd 项目名

# 2\. 查看当前分支

git branch\`

### 日常开发流程

bash

\`# 1. 确保在主分支，拉取最新代码 git checkout main git pull origin main

# 2\. 创建新分支（命名规范：feature/功能名 或 fix/bug名）

git checkout -b feature/add-login

# 3\. 修改代码...

# 4\. 查看改动

git status

# 5\. 添加改动到暂存区

git add . # 添加所有文件

# 或

git add [文件名.py](http://文件名.py) # 只添加特定文件

# 6\. 提交改动（写清楚做了什么）

git commit -m "添加用户登录功能"

# 7\. 推送到远程

git push origin feature/add-login

# 8\. 在 GitHub 网页上发起 Pull Request

# 请求把你的分支合并到 main\`

### 其他常用命令

bash

\`# 切换分支 git checkout 分支名

# 查看提交历史

git log --oneline

# 撤销未提交的修改

git checkout -- 文件名

# 更新当前分支（同步主分支的最新改动）

git checkout main git pull origin main git checkout feature/add-login git merge main\`

* * *

### 场景 2：你是外部贡献者（没有写权限）

### 使用 Fork 工作流

bash

\`# 1. 在 GitHub 网页上点击 "Fork" 按钮

# 这会把项目复制到你的账号下

# 2\. 克隆你 Fork 的仓库

git clone [https://github.com/你的用户名/项目名.git](https://github.com/你的用户名/项目名.git) cd 项目名

# 3\. 添加原始仓库为上游（upstream）

git remote add upstream [https://github.com/原作者/项目名.git](https://github.com/原作者/项目名.git)

# 4\. 创建新分支并开发

git checkout -b fix/typo-in-readme

# 5\. 修改代码...

# 6\. 提交并推送到你的 Fork

git add . git commit -m "修复 README 中的拼写错误" git push origin fix/typo-in-readme

# 7\. 在 GitHub 上发起 Pull Request

# 从你的 Fork 请求合并到原始仓库

# 8\. 同步原始仓库的最新改动（定期执行）

git fetch upstream git checkout main git merge upstream/main git push origin main\`

## 四、解决冲突

### 什么是冲突？

当你和别人修改了同一个文件的同一行，Git 无法自动合并时会产生冲突。

### 解决步骤

bash

\`# 1. 拉取最新代码时出现冲突 git pull origin main

# 提示：CONFLICT (content): Merge conflict in [文件名.py](http://文件名.py)

# 2\. 打开冲突文件，会看到：

# <<<<<<< HEAD 你的代码

别人的代码

> > > > > > > branch-name

# 3\. 手动编辑，保留正确的代码，删除标记符

# 4\. 标记冲突已解决

git add [文件名.py](http://文件名.py)

# 5\. 完成合并

git commit -m "解决合并冲突"\`

* * *

## 五、常用 Git 命令速查

bash

\`# 配置用户信息（第一次使用） git config --global [user.name](http://user.name) "你的名字" git config --global [user.email](http://user.email) "你的邮箱"

# 查看状态

git status

# 查看改动

git diff

# 查看远程仓库

git remote -v

# 删除分支

git branch -d 分支名 # 本地删除 git push origin --delete 分支名 # 远程删除

# 暂存当前工作（临时切换分支）

git stash # 暂存 git stash pop # 恢复

# 回退到某个版本

git reset --hard commit-id # 危险操作！\`

* * *

## 六、团队协作流程图

`你的本地电脑 GitHub 远程仓库 ┌─────────────┐ ┌─────────────┐ │ │ git clone │ │ │ 工作目录 │ ◄──────────────│ 项目仓库 │ │ │ │ │ └─────────────┘ └─────────────┘ │ ▲ │ git add │ ▼ │ ┌─────────────┐ │ │ 暂存区 │ │ │ (Stage) │ │ └─────────────┘ │ │ │ │ git commit │ ▼ │ ┌─────────────┐ git push │ │ 本地仓库 │ ────────────────────┘ │ (Repository)│ └─────────────┘ ▲ │ git pull └────────────────────────────┘`

* * *

## 七、GitHub 网页操作

### 发起 Pull Request

1.  Push 代码后，访问 GitHub 仓库页面
    
2.  点击 "Compare & pull request" 绿色按钮
    
3.  填写 PR 标题和描述（说明做了什么改动）
    
4.  选择审查者（Reviewers）
    
5.  点击 "Create pull request"
    

### 代码审查

-   查看 "Files changed" 标签页
    
-   在代码行旁边添加评论
    
-   批准（Approve）或请求修改（Request changes）
    

### 合并 PR

-   确保所有检查通过（CI/CD）
    
-   点击 "Merge pull request"
    
-   选择合并方式：
    
    -   **Merge commit**：保留所有提交历史
        
    -   **Squash and merge**：合并为一个提交（推荐）
        
    -   **Rebase and merge**：线性历史
<!-- DAILY_CHECKIN_2026-02-01_END -->

# 2026-01-31
<!-- DAILY_CHECKIN_2026-01-31_START -->



今天在为了项目要设计几个agent；怎么调校不同的agent;agent之间用什么pattern；agent\\MCP server\\skills\\tools到底是什么关系 狂补这些知识

### 主流的Agent在做什么

**1、信息—→行动型Agent**

典型特征：非结构化信息，转化为结构化信息；只做提案，不做决策

典型例子

-   从一堆消息中判断哪些是“会议/任务/截止日期”
    
-   自动整理成 Todo / Calendar / CRM / Project task
    
-   给出“建议行动方案”，而不是直接拍脑袋执行
    

**2、流程自动化Agent( Workflow / Orchestration Agent )**

典型特征：用户目标很清楚；Agent 负责 **拆步骤 + 调工具 + 兜底**

典型例子

-   报名 / 填表 / 拉数据 / 批量操作
    
-   跨系统同步（Notion ↔ Calendar ↔ Slack）
    

3、决策辅助型Agent（**规则+推理混合**）

典型特征：输入复杂、帮助压缩决策空间

典型例子：

-   排优先级（邮件 / 需求 / 工单）
    
-   判断哪些事情“现在不该做”
    
-   给出 next-best-action
    

**4、探索/调研型AI**

## **5、Multi-Agent**

[https://www.youtube.com/shorts/hdZwwSF\_p5U?feature=share](https://www.youtube.com/shorts/hdZwwSF_p5U?feature=share)

**结构**：

用户 → Meta-Agent → 多个子 Agent → 汇总 → 输出

**关键点**

-   Meta-Agent 不干活，**只做调度和决策**
    
-   子 Agent 各自有明确职责（搜索、检索、分析）
    

**为什么会出现**

> 当一个 Agent 同时承担「找信息 + 判断 + 反驳 + 总结」时，质量会急剧下降。

### Design Patterns（设计模式）

1.  Sequential（顺序模式）
    

-   **工作流程**：查询 → 智能体1 → 智能体2 → ... → 智能体n → 输出
    
-   **特点**：
    
    -   线性处理流程
        
    -   每个智能体按顺序执行特定任务
        
    -   前一个智能体的输出作为下一个的输入
        
-   **适用场景**：需要多步骤处理的任务
    

2.  Router（路由模式）
    

-   **工作流程**：查询 → 根据需求路由到不同专门智能体
    
-   **包含的智能体**：
    
    -   Flight Agent（航班智能体）
        
    -   Travel Agent（旅行智能体）
        
    -   Hotel Agent（酒店智能体）
        
-   **特点**：
    
    -   根据查询类型智能分发
        
    -   不同智能体并行工作
        
    -   各自产生独立输出
        
-   **适用场景**：多类别任务分类处理
    

3.  Parallel（并行模式）
    

-   **工作流程**：查询 → Divisor（分发器）→ 多个智能体并行执行 → 输出
    
-   **包含的智能体**：
    
    -   Web Search（网络搜索）
        
    -   Database Search（数据库搜索）
        
-   **特点**：
    
    -   同时执行多个任务
        
    -   提高处理效率
        
    -   结果汇总输出
        
-   **适用场景**：需要从多个数据源获取信息
    

4.  Generator（生成器模式）
    

-   **工作流程**：查询 → 多个智能体循环协作 → 输出
    
-   **包含的智能体**：
    
    -   Coding（编码）
        
    -   Testing（测试）
        
    -   Debugging（调试）
        
-   **特点**：
    
    -   迭代式工作流程
        
    -   持续优化和改进
        
    -   循环反馈机制
        
-   **适用场景**：需要迭代优化的任务（如代码开发）
    

5.  Network（网络模式）
    

-   **工作流程**：查询 → Meta Agent（元智能体）→ 多个智能体相互通信 → 输出
    
-   **包含的智能体**：
    
    -   Coding（编码）
        
    -   Debugging（调试）
        
    -   其他协作智能体
        
-   **特点**：
    
    -   智能体之间可以相互通信
        
    -   动态协作网络
        
    -   复杂的交互关系
        
-   **适用场景**：需要多智能体深度协作的复杂任务
    

6.  Autonomous Agents（自主智能体模式）
    

-   **工作流程**：
    
    -   Agent 1 处理查询
        
    -   Agent 2 处理输出
        
    -   两者独立运作但相互协调
        
-   **特点**：
    
    -   高度自主性
        
    -   智能体独立决策
        
    -   松耦合协作
        
-   **适用场景**：需要独立决策和行动的场景
    

### 关键要点总结

1.  **选择合适的模式**：根据任务复杂度和需求选择对应的设计模式
    
2.  **MCP Server**：为智能体提供工具和数据支持
    
3.  **Memory（记忆）**：智能体需要记忆功能来保持上下文
    
4.  **协作机制**：多智能体系统的核心是协作和任务分配
    
5.  **可扩展性**：设计模式支持系统的灵活扩展
<!-- DAILY_CHECKIN_2026-01-31_END -->

# 2026-01-30
<!-- DAILY_CHECKIN_2026-01-30_START -->




最近几天在参加hackthon，笔记后面再补（免淘汰卡，滴）
<!-- DAILY_CHECKIN_2026-01-30_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->





### Unexpected Ether Transfer (Forcing Feeding)

一个合约被转ETH的时候，EVM会按固定优先级决定先调用receive（）还是fallback(),否则直接revert

![image.png](attachment:7cf058ed-e26a-46c9-8d07-edea0efdb0fe:image.png)

被攻击的案例

```jsx
pragma solidity ^0.8.13;

contract Vulnerable {
    receive() external payable {
        revert();
    }

    function somethingBad() external {
        require(address(this).balance > 0);
        // Do something bad
    }
}
```

**自毁**

Solidity 层的“收款控制”≠ EVM 层的“余额控制”

`SELFDESTRUCT` 是一种 EVM 级的强制转账机制，它可以在不执行任何 Solidity 接收逻辑的情况下改变合约余额，因此所有“基于余额的安全假设”都是不可靠的。

**Pre-calculated Deployment**

用 `CREATE` 部署合约时，合约地址 = 由部署者地址 + 当时的 nonce 决定的确定性结果，所以在合约真正部署之前，这个地址就已经是“已知的”

攻击者可以在部署完成之前向地址发送资金

**总结：**

上述结果表明，仅仅依靠与合约中以太币余额的精确比较是不可靠的。智能合约的业务逻辑必须考虑到，与其相关的实际余额可能高于内部会计记录的值。

**一般来说，不建议使用合约余额作为保障措施。**

![image.png](attachment:b48d7e8f-c37d-4076-b07c-19cb3856097e:image.png)

### Unprotected Swaps

```jsx
IUniswapRouterV2(ROUTER).swapExactTokensForTokens(
    toSwap,
    0,
    path,
    address(this),
    now
);
```

在此代码片段中，第二个参数 `<output_token>` amountOutMin `指定交易者预期收到的最小输出代币数量。当 <output_token>amountOutMin设置为 0时0`，无论输出数量多少，交易都可以继续进行，从而有效地容忍无限滑点.

**对于滑点的理解**

前端根据当前池子价格算出「预计能拿到多少 token」，用户设置“可接受滑点 %”，前端把它换算成amountOutMin，合约只保证：最终收到的数量 ≥ 这个最小值。

当amountOutMin = 0时，你放弃了对“最差成交结果”的任何保护，合约允许你用既定输入金额，换取任意少的输出数量（趋近于 0）,等价于“以无限差的成交价格完成交易”。

```jsx
ISwapRouter.ExactInputSingleParams memory params = ISwapRouter.ExactInputSingleParams({
 tokenIn: inToken,//换钱的token
 tokenOut: getTokenisedAddr(_outToken),//目标token
 fee: 3000,//Uniswap V3特有概念 3000=0.3% fee pool
 recipient: address(this),
 deadline: block.timestamp + 60,
 amountIn: _amount - swapFee,
 amountOutMinimum: 0 //最小接受的swaptoken
 });
```

这个是在Uniswap V3中的一个严重的风险

因为V3：

-   集中流动性
    
-   非连续价格区间
    
-   可以被推到极端tick
    

**漏洞利用场景**

前情提要：

**Uniswap 的核心公式：恒定乘积**

Uniswap 使用 **x × y = k** 公式（恒定乘积做市商模型）

x = 池中 A 代币数量

y = 池中 Y 代币数量

k = 常数（池子创建时确定）

**关键理解：池子里某个币越少，它就越贵**

以下示例说明了攻击者如何利用在两个代币 A 和 Y 之间交换抵押品的协议：

攻击者可以通过以下步骤利用协议漏洞：首先，使用 10 个 A 代币作为抵押品开设一个抵押债务头寸 (CDP)，并铸造 100 个 Y 代币。然后，攻击者利用协议缺乏滑点保护的漏洞，触发协议在 A/Y 池中执行人为操纵的互换操作，从而实现价格的无限波动。攻击者利用闪电贷和其他价格操纵手段，人为地抬高或压低 A 代币的价格，最终耗尽 A 代币池的流动性。结果，由于无法收回足够的抵押品，协议将背负大量坏账；而攻击者则出售铸造的 Y 代币获利，协议及其用户则承担损失。

这种攻击会对协议造成严重损害，直接给协议或其用户带来损失

**实际：质押并不会出现这种情况**

真实的DeFi协议如何解决?

方案1: 时间加权平均价格 (TWAP)

不用瞬时价格,而是用过去一段时间的平均价格

\`例如:

-   不看"现在这一秒" A=50Y
    
-   而看"过去1小时平均" A=10Y
    

攻击者用闪电贷瞬间拉高价格 → 但平均价格只变化一点点 → 攻击失败\`

代表项目: Uniswap V2/V3 的 TWAP 预言机

方案2: 多数据源聚合预言机

从多个地方获取价格,取中位数

\`Chainlink 预言机:

-   从 Binance 获取价格: 1A = 10Y
    
-   从 Coinbase 获取: 1A = 10.1Y
    
-   从 Uniswap 获取: 1A = 50Y (被操纵)
    
-   取中位数: 1A ≈ 10Y
    

攻击者只操纵了一个池子 → 中位数不受影响 → 攻击失败\`

代表项目: Chainlink, Band Protocol

方案3: 深度检查 + 滑点限制

设置交易的最大价格波动范围

\`协议规则: "如果价格变动超过5%,交易失败"

攻击者拉高价格50倍 → 协议拒绝执行 → 攻击失败\`

**缓解措施**

一个稳健的解决方案涉及一个两步交易流程，即提交-显示方案：

1.  提交阶段：用户提交对其交易详情（例如，资产对、金额和滑点容忍度）的加密承诺。这可以保护关键信息免受攻击者的侵害。
    
2.  揭示阶段：在提交阶段完成后，用户披露交易详情，交易执行。
    

这种机制使得攻击者更难预测和利用交易，从而缓解了基于夹层和滑点的攻击。

此外，协议还可以实施进一步的缓解措施来增强安全性。**动态滑点限制**可以根据市场状况和交易规模调整容忍度，从而降低波动市场中的脆弱性。使用**时间加权平均价格 (TWAP)数据有助于抵消短期价格操纵，确保更准确的定价。通过 MEV 保护的内存池等解决方案实现的私密交易**，可以在交易被打包到区块之前隐藏交易详情，防止攻击者提前锁定交易。最后，**流动性分析**可确保交易池保持充足的流动性，最大限度地减少大额交易对价格的影响，并降低操纵风险。
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->






今天复习一下transformer 准备选LLM+agent方向

-   当两个向量指向同一个方向时，点积为正
    
-   垂直方向时，点积为零
    
-   相反方向时，点积是负的
    

Embedding、Key、Query、Value、Output、Up-projection、Down-projection、Unembedding

**Embedding** 决定了每一个单词转换成什么向量矩阵，且embedding matrix是会改变的。

在chatgpt中每一个word的维度就高达12288 coordination(维），机器通过不断调整词向量的表示，找到一组嵌入使得空间中的方向含有特定的语义意义。

训练后的embedding层的参数是固定的，这意味着每个token都可以用唯一的向量表示，但这时候的向量不包含上下文，我们的目标就是让向量通过网络传递，使得每个向量都获得比单个词更加丰富和具体的含义。

每个网络都只能处理一定数量的上下文，这被我们称为context size

**Unembedding** 为单词表里的每一个单词都分配了一行。最后的向量通过softmax的处理会把每一行变为概率分布（softmax是为了满足概率分布和加起来为1的概念）
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->







### **Signature—related attacks**

攻击者利用签名验证机制的漏洞来实现未授权攻击。

**缺少验证攻击**

```jsx
function recover(uint8 v, bytes32 r, bytes32 s, bytes32 hash) external {
    address signer = ecrecover(hash, v, r, s);
    //Do more stuff with the hash
}
```

-   攻击者故意提交**无效的签名参数**（比如全是0的 v, r, s）
    
-   `ecrecover` 无法恢复出有效地址，返回 `address(0)`（零地址）
    
-   如果 `owner` 恰好也没有初始化或被设置为 `address(0)`
    
-   那么 `address(0) == address(0)` 验证通过！
    
-   攻击者成功盗取资金
    

解决方法

```jsx
require(signer != address(0), "invalid signature");//检查攻击者信息
```

用OpenZeppelin库是好的

**回放攻击**

```jsx
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";

contract OwnerAction {
    using ECDSA for bytes32;//toEthSignedMessageHash()：把 32-byte 的消息哈希转成以太坊钱包常见的“个人签名”格式
		//recover(signature)：从签名里恢复出签名者地址。
    address public owner;

    constructor() payable {
        owner = msg.sender;
    }

    function action(uint256 _param1, bytes32 _param2, bytes memory _sig) external {
        bytes32 hash = keccak256(abi.encodePacked(_param1, _param2));//第一次哈希，虽然用了abi.encodePacked（），但_param1,_param2是固定字节的所以不会产生
        bytes32 signedHash = hash.toEthSignedMessageHash();//第二次哈希加前缀
        address signer = signedHash.recover(_sig);

        require(signer == owner, "Invalid signature");

        // use `param1` and `param2` to perform authorized action
    }
}
```

应该多加一个\_nounce防止签名重放

**Cross-Chain Replay attack(跨链重放攻击）**

```jsx
pragma solidity ^0.8.17;

import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";

contract OwnerAction {
    using ECDSA for bytes32;

    address public owner;
    mapping(bytes32 => bool) public seenSignatures;

    constructor() payable {
        owner = msg.sender;
    }

    function action(uint256 _param1, bytes32 _param2, uint256 _nonce, uint256 _chainId, bytes memory _sig) external {
        require(_chainId == block.chainid, "Invalid chain ID");

        bytes32 hash = keccak256(abi.encodePacked(_param1, _param2, _nonce, _chainId));
        require(!seenSignatures[hash], "Signature has been used");

        bytes32 signedHash = hash.toEthSignedMessageHash();
        address signer = signedHash.recover(_sig);
        require(signer == owner, "Invalid signature");

        seenSignatures[hash] = true;

        // use `param1` and `param2` to perform authorized action
    }
}
```

为了防止一个签名在不同链上被使用，所以加了一个签名中链ID的验证

**签名可塑性**

关于ECDSA签名的特殊性

![image.png](attachment:71eb4836-148a-4f74-8a36-124d60ab130d:image.png)

由于圆锥曲线的对称性，签名可以被修改但不会失效，OpenZeppelin的ECDSA库包含代码防止签名可塑性攻击

```jsx
if (uint256(s) > 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF5D576E7357A4501DDFE92F46681B20A0) {
    return (address(0), RecoverError.InvalidSignatureS);
}
```

**Frontrunning**

另一个常见问题是抢先交易。攻击者可以监控内存池中某些系统（例如那些奖励第三方执行恶意载荷的系统）中使用的 ECDSA 签名交易。根据签名有效载荷中的信息，攻击者可以抢先执行原始交易、操纵特定参数并利用系统漏洞。 这是合约验证签名的漏洞

```jsx
bytes32 hash = keccak256(abi.encodePacked(_param2, _nonce, _chainId));//签名对transfer的完整信息没有保护
```

**紧凑型签名**

![image.png](attachment:5eeb976e-ff90-40b4-95c6-131e6ea4008e:image.png)

两种格式可以互相转换
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->








### Oracle Manipulation Attacks(预言机操纵攻击）

漏洞：盲目依赖单一数据源信息

例一：闪电贷进行链上价格操作。通过大量买入（稀缺性）抬高单一货币价格，已虚高价格进行质押获利，大量以原价卖出归还闪电贷。

```jsx
攻击者可以对即将到账的资产 A 进行闪电贷，然后在相关的 Uniswap 资金池中，以大笔交易将资产 A 兑换成资产 B。
这项交易将提高资产 B 的价格（需求增加），并降低资产 A 的成本（供给增加）。
当资产 B 存入上述函数时，其价格仍然会被闪电贷推高。
因此，资产 B 使攻击者获得了不成比例的股份数量。
这些份额可以被提取，使攻击者从资产池中获得等量的 A 和 B 资产。
重复这个过程会耗尽所有脆弱资金池中的资金。
攻击者可以利用赎回股份所得的资金偿还闪电贷。
```

### Reentrancy

重入漏洞

**单功能重入**

```jsx
/漏洞合约，先转账在修改余额，攻击者通过回退函数不断提取余额
contract Vulnerable {
    mapping (address => uint) private balances;

    function withdraw() public {
    uint amount = balances[msg.sender];
    (bool success, ) = msg.sender.call.value(amount)("");
    require(success);
    balances[msg.sender] = 0;
    }
}
```

```jsx
/攻击者利用回退函数 不断提取合约余额
receive() external payable {
    // 此时 balances[攻击者] 还是 1 ETH（没有被清零）
    if (address(target).balance > 0) {  // Vulnerable 还有 9 ETH
        target.withdraw();  // 🔁 再次调用 withdraw！
    }
}
```

**Cross-Function Reentrancy 跨函数重入**

```jsx
contract Vulnerable {//用于内部记账，影响withdraw balances的状态
    mapping (address => uint) private balances;

    function transfer(address to, uint amount) public {
        if (balances[msg.sender] >= amount) {
        balances[to] += amount;
        balances[msg.sender] -= amount;
        }
    }

    function withdraw() public {
        uint amount = balances[msg.sender];
        (bool success, ) = msg.sender.call.value(amount)("");
        require(success);
        balances[msg.sender] = 0;
    }
}
```

跨函数重入的特点：

1、balance共享状态

2、防住一个函数的重入状态防不住另一个

3、重入窗口存在（balances\[msg.sender\]仍然在转账成功后修改）

在这个例子中，攻击者可以先调用该`withdraw`函数，并在收到价值转移请求后，再次进入合约，但这次仍然调用该`transfer`函数。由于`withdraw`攻击者通过该函数重新进入时，之前的调用尚未完成`transfer`，因此该函数的余额映射`msg.sender`尚未被重置为零。结果，攻击者除了可以提取的金额外，还可以转移其他资金
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->









今天完善了一下领英和web3 security governance的英文简历，就不在这里po了
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->










### **参数顺序错误**

solidity不支持命名参数

```jsx
# Python 支持命名参数
def transfer(to, amount, notify):
    pass

# 可以任意顺序，只要指定名字
transfer(amount=100, notify=True, to="0x123...")  ✅
transfer(notify=True, to="0x123...", amount=100)  ✅
```

所以solidity必须严格按照顺序传递参数

### Oracle Manipulation Attacks(预言机操纵攻击）

漏洞：盲目依赖单一数据源信息

例一：闪电贷进行链上价格操作。通过大量买入（稀缺性）抬高单一货币价格，已虚高价格进行质押获利，大量以原价卖出归还闪电贷。

```jsx
攻击者可以对即将到账的资产 A 进行闪电贷，然后在相关的 Uniswap 资金池中，以大笔交易将资产 A 兑换成资产 B。
这项交易将提高资产 B 的价格（需求增加），并降低资产 A 的成本（供给增加）。
当资产 B 存入上述函数时，其价格仍然会被闪电贷推高。
因此，资产 B 使攻击者获得了不成比例的股份数量。
这些份额可以被提取，使攻击者从资产池中获得等量的 A 和 B 资产。
重复这个过程会耗尽所有脆弱资金池中的资金。
攻击者可以利用赎回股份所得的资金偿还闪电贷。
```
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->











### Rag

RAG（检索增强生成）—Retrieval-Augmented Generation

**解决问题：**一、知识有截止日期 二、产生幻觉

**技术流程：**

-   索引阶段（离线准备）：chunks(将知识库切成小块+Embedding Model(存储于向量库）——>上下文构建
    
-   检索阶段（实时查询）：在向量库进行相似度搜索
    
-   增强阶段（上下文构建）：检索到文档片段（context)+用户问题=prompt
    
-   生成阶段（LLM输出）
    

**技术要点**

chunk size: 大—>引入噪音 小—→丢失上下文

embedding model:平衡语义理解能力和推理速度

vector retrieval: hybrid search+稠密向量检索/稀疏检索提升效果

### MCP（Model context Protocol)

**目的：**在MCP之前,每个工具都需要单独集成,开发者要为每个API写专门的适配代码。MCP提供了统一的协议规范,就像USB标准让各种设备都能即插即用。

**技术架构**

MCP采用client-server架构。AI系统作为MCP client,各种工具/数据源作为MCP server。协议定义了三个核心概念:

-   **Resources**(资源):AI可以读取的数据源,如文件、数据库记录、API响应
    
-   **Prompts**(提示模板):预定义的操作模板,降低AI使用工具的难度
    
-   **Tools**(工具):AI可以主动调用的函数,实现具体操作
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->












## 工作原理

当你拥有一个 ERC-7962 代币时，所有权记录的是你的公钥哈希，而不是你的地址。要转移代币，你需要提供：

-   你的完整公钥（65字节的 secp256k1 未压缩公钥）
    
-   用对应私钥签名的 ECDSA 签名（对授权转移协议的私钥签名）
    
-   目标密钥哈希（接收方）
    
-   签名截止时间
    

合约会验证 `keccak256(公钥) = 当前所有者的keyHash`，并验证签名是否由该公钥对应的私钥生成。验证通过后，所有权转移到新的 keyHash。

## 两大优势

**隐私保护**：链上只存储密钥哈希，不存储地址。你可以为每个代币使用不同的密钥对，大大降低交易之间的关联性。

**Gas 费分离**：任何人都可以提交转账交易（只要有有效签名），这意味着第三方可以代付 Gas 费，而不会获得代币控制权。这对批量交易和 Gas 赞助场景非常有用。

## ERC-KeyHash20 的特殊设计

ERC-KeyHash20（同质化代币）采用了类似比特币 UTXO 的模型。转账时，原始余额清零，转账金额发送到接收方的 keyHash，**剩余余额**必须发送到一个新的 keyHash（`leftKeyHash`）。这强制执行密钥轮换，进一步增强隐私性和不可关联性。

## 安全机制

该标准使用 EIP-712 结构化签名防止消息篡改，使用每个 keyHash 的 nonce 防止重放攻击，并设置签名截止时间限制签名有效期。这些机制共同确保只有真正的所有者才能操作代币。

这个标准特别适合需要匿名性的收藏品、私密交易或去中心化身份应用场景。

阅读材料

GITHUB:[https://github.com/ethereum/ERCs/blob/master/ERCS/erc-7962.md](https://github.com/ethereum/ERCs/blob/master/ERCS/erc-7962.md)

magician：[https://ethereum-magicians.org/t/erc-7962-key-hash-based-tokens/24422](https://ethereum-magicians.org/t/erc-7962-key-hash-based-tokens/24422)

怎么理解？把安全问题放到grok里面，攻击

判断一个协议：把web3的一些特性的设计初衷来判断，协议的设计会不会符合透明性、去中心化的特征。解决问题之间是如何权衡的。

**NFT**是基于ERC-721的一个实践，在现实世界中通常表达为一个藏品。——性质在区块链上有ownership的记录，溯源透明。

[ERC-7962协议](https://www.notion.so/ERC-7962-2ec7fcc97cdf80beaa27dcbfde89260b?pvs=21)

## ERC-7962

**核心概念**

1.  以 keccak256(公钥) 而非地址作为资产所有权标识
    
2.  所有权与交易发起地址解耦，不在链上存储地址
    
3.  覆盖ERC-20 （稳定币）与ERC-721（NFT）两类资产模型
    
4.  用户只需签名表达意图，无需持币、无需理解 Gas、无需管理地址隐私，也能完成资产操作
    

**解决关键问题**

1.  隐私问题:避免地址可追踪，降低身份与资产的关联性
    
2.  UX问题:用户只需签名表达意图，无需持币、无需理解Gas、无需管理地址隐私，也能完成资产操作。——-降低用户的操作困难
    
3.  扩展性问题:天然支持批量交易、Relayer、账户抽象场景
    

**技术要点** 所有权由 keyHash=keccak256(publicKey)表示转账时提交:

-   公钥
    
-   EIP-712签名
    
-   nonce + deadline (防重放)
    
-   通过ECDSA验证签名人与keyHash 的一致性
    

**ERC-KeyHash20 (FT)**

-   keyHash balance余额模型
    
-   转账采用 UTXO风格拆分
    
-   转出部分toKeyHash
    
-   剩余部分 leftKeyHash
    
-   强制key轮换，增强不可关联性
    

**Gas 与交易模型**

-   任何地址都可提交交易
    
-   所有权只由签名控制
    
-   天然支持: Gas Sponsorship（代付Gas费）——交易的发起者和token的owner是分开的，可以由任何一个relayer发起交易 批量转账 Relayer / Paymaster
    

**ERC-7962的设计初衷：**

-   希望通过ZK能证明身份的同时，不暴露隐私。
    
-   ERC-7962在ERC-721的基础上应用ZK原理保证了向第三方的证明其归属，但不暴露真实地址
    

**优势总结**

-   更强隐私(地址不上链)
    
-   更佳用户体验(免ETH/Gas代付)
    
-   更高灵活性(批量、代付、抽象账户)
    
-   与 ERC-5564 (StealthAddress)理念一致
    

**局限与代价**

-   Gas成本高于传统ERC-20/ERC-721!
    
-   公钥在 calldata中暴露(需key轮换)!
    
-   与现有ERC标准不向后兼容
    
-   对密钥管理要求更高
    

**安全设计**

-   EIP-712结构化签名，防参数 篡改
    
-   per-keyHash nonce，防重放 攻击（防止relayer作恶，double-check）
    
-   deadline控制签名有效期
    
-   强制low-S/v校验，防签名可塑性
    

**适用场景**

-   隐私资产/匿名
    
-   NFTWeb3新手友好
    
-   钱包企业级批量分发、空投Account Abstraction /Paymaster 体系'
    

### Zero Knowledge Proof

我能证明我知道某个秘密/满足某个条件，但我不需要把秘密本身告诉你。

例如：大素数分解、哈希

## 知识补充

1、Relayer = “代你把交易广播上链、并帮你先垫 gas 的中间人/转发者”。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->













### FrontRunning

区块链网络上的每笔交易在执行前都会在内存池中经历一段可见期。这种透明性使得网络参与者能够在交易被打包进区块之前查看并对其做出响应。攻击者可以利用这种信息泄露来影响交易的执行过程。

**直觉解释**：利用交易在Mempool中的可见期，在之前、之中和之后插手，改变结果，从而达到牟利的目的。

**置换攻击**

```jsx
pragma solidity ^0.8.17;

contract GuessTheNumberChallenge {
    bytes32 challenge;

    constructor(bytes32 _challenge) payable {
        require(msg.value == 1 ether);
        challenge = _challenge;
    }

    function isComplete() public view returns (bool) {
        return address(this).balance == 0;
    }

    function guess(uint256 number) public payable {
        require(msg.value == 1 ether, "Submission fee required");
        uint256 balance = address(this).balance;
        require(balance != 0, "Game has ended");

        bytes32 userChallenge = keccak256(abi.encode(number));
        if (userChallenge == challenge) {
            (bool success, ) = msg.sender.call{value: balance}("");
            require(success, "Transfer failed");
        }
    }
```

合约逻辑：

猜数字游戏，猜到数字的人可以提取合约中所有的余额

该系统存在一个根本性的设计缺陷。当用户准备提交猜测时，他们会调用一个`guess`函数并将猜测的数字作为参数传递。除非采取特殊预防措施，否则该交易会在被挖矿并打包到区块之前就出现在内存池中。这种可见性使得任何观察者都有机会复制交易数据，其中包括`guess`函数签名及其参数。

通用抢先者可以复制此交易数据，并在分叉网络上模拟该交易（分叉链居然可以clone一个合约的所有状态包括状态参数的值？？？）。如果交易盈利，他们可以重新提交复制的有效载荷，并支付高于原始用户的 gas 价格。这种在 gas 价格拍卖中出价更高的行为，增加了他们的交易在原始用户交易之前被挖矿的概率。

这种情况反映了抢先交易的一个关键方面。一旦复制的交易数据被提交并被挖矿，原始交易在智能合约系统的上下文中就变得无关紧要了。

**插入攻击**

滑点窃取攻击或者叫夹心饼干（Sandwich Attack)攻击。

![image.png](attachment:9e1bc133-3f66-498a-ba4e-fde34d7850e7:image.png)

假设你发起交易（进入mempool)

所有人都能看到：你要买什么、买多少、滑点是多少（可接受的差价范围）

攻击操作：

攻击者抢先买币—>推高价格至滑点可接受的最高价——>你买币——>攻击者趁机卖出——→\*\*结果：\*\*攻击者赚到了你接受的差价

危机直觉：只要用户交易公开、价格即时操纵、只要存在滑点就有被夹空间

**抑制攻击（block stuffing)**

攻击目的：让某些关键交易晚进区块，来达到操纵市场的目的：比如预言机更新、清算、开奖、最后一笔抢购。

手段：攻击者用高优先级费用+高gas消耗占满区块容量，让别人的交易连续几个区块都进不去，从而改变结果或者制造系统性风险。

EIP-1559缓解风险——把“持续填满区块”的成本抬高

```jsx
// EIP-1559 之前
交易费用 = gasUsed × gasPrice
// gasPrice 全部给矿工

// 区块容量固定
区块 gas limit = 15,000,000 gas
```

成本：const attackCost = (blockGasLimit × attackGasPrice) / 1e9;

攻击成本有限；如果攻击者本身就是矿工就可以免费填充自己的区块

EIP-1599的新的费用结构

```jsx
// EIP-1559 之后
交易费用 = (baseFee + priorityFee) × gasUsed

// baseFee：基础费，被销毁 🔥
// priorityFee：小费，给验证者
// Base Fee 调整机制

if (上一个区块使用量 > 目标容量) {
    baseFee 增加 12.5%
} else if (上一个区块使用量 < 目标容量) {
    baseFee 减少 12.5%
}

// 弹性区块
目标容量 = 15,000,000 gas
最大容量 = 30,000,000 gas (2倍弹性)
```

![image.png](attachment:6b9ffd4e-c1e6-4482-befc-dcf695d3a2f5:image.png)

**恶意攻击**

拒绝服务攻击

```jsx
pragma solidity ^0.8.17;

contract DelayedWithdrawal {
    address beneficiary;
    uint256 delay;
    uint256 lastDeposit;

    constructor(uint256 _delay) {
        beneficiary = msg.sender;
        lastDeposit = block.timestamp;
        delay = _delay;
    }

    modifier checkDelay() {
        require(block.timestamp >= lastDeposit + delay, "Keep waiting");
        _;
    }

    function deposit() public payable {
        require(msg.value != 0);
        lastDeposit = block.timestamp;
    }

    function withdraw() public checkDelay {
        (bool success, ) = beneficiary.call{value: address(this).balance}("");
        require(success, "Transfer failed");
    }
}
```

延迟提现合约：受益人必须在lastDeposit时间戳+delay的时间间隔才能实现取现。然而攻击者可以通过不断的更新lastDeposit，导致受益人无法提现。

**毒气恶意攻击（Infficient Gas Griefing)**

这种攻击中，攻击者可能只提供足够的gas来确保顶层函数成功执行，同时确保外部调用因gas耗尽而失败。由于[63/64规则](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-150.md)，顶层合约可以完成其函数调用，而顶层函数不检查外部调用是否成功，从而导致合约目的最终不能执行。

常见毒气攻击合约：此类问题在执行**通用调用**的智能合约中尤为常见，例如中继器和多重签名钱包。

```jsx
contract Relayer {
    // 记录哪些数据已经执行过（防止重放攻击）
    mapping (bytes => bool) executed;
    
    // 目标合约地址
    address target;
    
    function forward(bytes memory _data) public {
        // 步骤 1：检查这个数据是否已经执行过
        require(!executed[_data], "Replay protection");
        
        // 步骤 2：（代码中省略）验证签名
        // ... signature validation ...
        
        // ❌ 步骤 3：标记为已执行（危险！先改状态）
        executed[_data] = true;
        
        // ❌ 步骤 4：外部调用（没检查返回值！）
        target.call(abi.encodeWithSignature("execute(bytes)", _data));
        //     ↑
        //   这就是外部调用！
    }
}
```
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->














## Exposed Data

区块链看似匿名的特性可能会给用户带来虚假的安全感。只要链上拥有足够的数据，用户的匿名性就很容易被破解。个人身份信息（PII）

**数据保护合规性**

对于将PII存储在链上时违反例如(GDPR)欧盟的《通用数据保护条例》的要求。

除了PII之外IP地址和电子邮件等信息存储在链上也会造成巨大风险。

## FrontRunning（今天太累了关于抢跑还有一些内容没学完，明天补充 ）

区块链网络上的每笔交易在执行前都会在内存池中经历一段可见期。这种透明性使得网络参与者能够在交易被打包进区块之前查看并对其做出响应。攻击者可以利用这种信息泄露来影响交易的执行过程。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->















# JS的特点

在 JavaScript 里，某些操作（比如**读取文件、网络请求、数据库查询、区块链交互**等）需要**一定的时间**才能完成。JavaScript 采用**异步编程（异步的意思指的是好几个耗时线程可以并行操作，不会被block)**来避免阻塞主线程。`await` 让代码 **按顺序执行**，避免出现未完成的任务导致错误。 默认会把 **I/O 操作（如数据库、网络请求、区块链调用）** 设为异步

### 什么是合约工厂？

合约工厂（Contract Factory）**是用于部署新智能合约的对象**。在 Hardhat + Ethers.js 中，你不能直接创建合约实例，而是需要先获取合约工厂，然后再部署合约。

**这和现实中的“工厂”很类似**：

-   **合约文件 (**`DiceMarket.sol`**)** 就像**蓝图**。
    
-   **合约工厂 (**`getContractFactory("DiceMarket")`**)** 就像一个**工厂**，可以根据**蓝图**生产多个合约实例。
    
-   **部署后的合约 (**`DiceMarket.deploy(...)`**)** 就像**最终生产出来的产品**。
    

# 关键字

### async

-   申明一个异步函数，返回值始终是Promise
    
-   如果函数内部返回的是普通值，JavaScript会自动封装成Promise.resolve(返回值）
    

```
async function hello() {
    return "Hello, world!";
}

hello().then(console.log); // 输出: Hello, world!由于关键字的作用，所以需要用.then()处理
```

### await

该关键字只能在async function内部使用，

它会暂停函数的执行，直到Promise解析（resolve),然后返回Promise解析之后的值

```
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function run() {
    console.log("开始");
    await delay(2000); // 等待 2 秒
    console.log("2 秒后执行");
}

run();
```

代码里的语法：

new Promise((resolve,reject)=>{.....}

resolve是一个函数，表示成功时候调用

reject也是一个函数，表示失败时候调用

setTimeout(resolve,ms)

setTimeout是JavaScript的定时器函数，作用是等待一段时间后执行某个操作

setTimeout(resolve,ms)代表等待ms毫秒后执行resolve()

### console

在 JavaScript 中，`console` 是一个**全局对象**，主要用于**打印调试信息**，帮助开发者查看代码的执行情况。最常用的方法是 `console.log()`，但 `console` 还有很多其他功能。

```
console.log("Hello, JavaScript!");
```

console的其他用法

![](https://cdn.nlark.com/yuque/0/2025/png/34360919/1741086687786-5e0d8cd5-cb6c-401b-aa9d-4a4a85756fe5.png)

# 变量声明和数据类型

### 变量声明的三种方式

```
// let - 推荐使用,可以重新赋值
let age = 25;
age = 26; // 允许

// const - 用于常量,不能重新赋值
const PI = 3.14;
// PI = 3.15; // 这会报错

// var - 旧版本使用,现在不推荐
var name = "John";
```

### 基本数据类型

```
// 数字 (Number)
let count = 42;        // 整数
let price = 99.99;     // 小数
let infinity = Infinity;//特殊用法，表示无穷大
let notANumber = NaN;//not a number

// 字符串 (String)
let firstName = "Alice";
let greeting = 'Hello';
let message = `Hello ${firstName}`; // 模板字符串

// 布尔值 (Boolean)
let isActive = true;
let isLoggedIn = false;

// 空值
let empty = null;
let undefined;

// 数组 (Array);❤这里的数组跟python里面的列表特别像
let colors = ['red', 'green', 'blue'];
let numbers = [1, 2, 3, 4, 5];

// 对象 (Object) 对象非常像C语言里面的结构体
let person = {
    name: "Alice",
    age: 25,
    isStudent: true
};
```

### 类型转换

```
// 转换为数字
let strNumber = "123";
let num1 = Number(strNumber);    // 123
let num2 = parseInt(strNumber);  // 123
let num3 = parseFloat("123.45"); // 123.45  
let bigNum=BigInt("123455677766");//bigint专门用于大整数

// 转换为字符串
let number = 123;
let str1 = String(number);      // "123"
let str2 = number.toString();   // "123"

// 转换为布尔值
let bool1 = Boolean(1);         // true
let bool2 = Boolean("");        // false
let bool3 = Boolean("hello");   // true
```

NaN

not a number；通常用于用户的输入校验

```
// 1. 验证用户输入是否为有效数字
function validateUserInput(input) {
    let number = parseFloat(input);
    if (isNaN(number)) {
        return "请输入有效的数字";
    }
    return "输入有效";
}

// 2. 数学计算错误检查
function divideNumbers(a, b) {
    let result = a / b;
    if (isNaN(result)) {
        return "计算出错";
    }
    return result;
}
```

注意校验NaN不需要用==号，可以直接用isNaN

### Hoisting

function关键字定义的函数会被完整的提升，可以在定义前调用

```
hello(); // 输出: Hello, world!

function hello() {
    console.log("Hello, world!");
}
/*其实就相当于*/
function hello() { // 函数声明被提升
    console.log("Hello, world!");
}

hello(); // 调用
```

### 箭头函数

箭头函数的语法

**const 函数名 = (参数) => { 函数体 }**

```
const add = (a, b) => {
    return a + b;
};
console.log(add(2, 3)); // 输出: 5
```

**隐式返回**

如果函数体只有一行return语句，可以省略{}和return

```
const multiply = (a, b) => a * b;
console.log(multiply(3, 4)); // 输出: 12
```

**参数规则**

![](https://cdn.nlark.com/yuque/0/2025/png/34360919/1740129457676-5a38cb23-b7a6-4133-b9da-904012844f42.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
















**unchecked:**

避免solidity 0.8.0开始的编译器自动对合约做数学安全检查，消耗gas.(高频函数非常在意gas)

避免使用unchecked的场景：

1.  输入来自用户
    
2.  条件依赖外部合约
    
3.  没有显式边界检查
    
4.  金额/索引/balance操作
    

## 危险调用

### **abi.encodePacked()**

功能：把几段字符拼接在一起，不加分隔符

比较安全的方法，拼接固定字节

-   `uint256` 固定 32 字节
    
-   `address` 固定 20 字节
    
-   `bytes32` 固定 32 字节
    

abi.encodePacked+动态类型 +hash/授权/校验 =！！！风险

### Ambiguous evaluation order

```jsx
function foo() public pure returns(uint){
uint x=5;
return x*x++;//could be 25 or 30
}
```

vulnerability mitigation:storing intermidiate results in temporary variables.

### Approval Vulnerabilities

**无限审批漏洞（权限失控）**

```jsx
approve(spender,type(uint256).max)
```

特点：授权额度无限、授权时间永久、不绑定具体交易\\业务条件

攻击路径：

1、用户授权无限审批（合约A）

2、合约A：

-   被黑
    
-   有后门
    
-   owner作恶
    

3、攻击者获得合约控制权

4、调用

```jsx
token.transferFrom(
	user,
	attacker,
	token.balanceOf(user)
	);
```

**审批抢先**

攻击者在受害者修改授权额度之前，提前花完原有的授权额度。（ERC20）

例子：

1.  通过此`approve x`功能，用户允许智能合约系统转移
    
    其 ERC20 代币。
    
2.  后来，他们选择修改授权金额，`approve` y 并因此发送了另一份请求。
    
3.  与此同时，在用户提供的交易被包含之前，攻击者启动了从用户钱包中`transferFrom x`删除代币的功能。
    
4.  如果攻击者的交易先被处理，然后用户的新`approve y`交易才被处理，那么恶意行为者就可以转移额外的代币。
    
5.  未经授权的转账总额将达到`x+y`代币级别。
    

```jsx
//allowances状态变量
mapping(address=>mapping(address=>uint256) private_allowances;
_allowances[owner][spender]=授权额度还剩下多少 
```

```jsx
//ERC20授权模型
approve(spender,value);//设置授权额度
transferFrom(owner,to,x);//在额度内转账
```

**Recommended Best Practices for Approval and Allowance Management**

针对 **ERC-20** 授权更新可能引发的抢跑风险，一个直接的缓解措施是优化 allowance 管理方式。相比通过approve以覆盖式写入新授权值，更推荐采用**增量调整**的模式，例如使用 OpenZeppelin SafeERC20提供的 safeIncreaseAllowance 与 safeDecreaseAllowance 来按差值更新授权额度，从而降低在“修改授权额度”场景下出现竞态条件的概率。

另一方面，“approval phishing（审批钓鱼）”虽不属于合约代码层面的漏洞，但在前端遭受内容注入或被篡改时，攻击者可将授权目标从预期合约地址替换为其控制地址，诱导用户完成恶意授权。为降低风险，开发者应遵循最小权限原则，仅授权业务所需的最小额度，并在必要时采用更严格的交互流程（例如两步操作、限额或延时机制）以减少单次恶意行为的影响；用户则应养成定期检查与撤销不再使用授权的习惯，可借助 [revoke.cash](http://revoke.cash) 等工具进行授权清理。

# Remix

### contract按键显示规则

**变量**

Public状态变量，自动形成蓝色按钮变成getter

**函数**

View/Pure函数，自动变成蓝色按钮，只读

External/Public函数，橙色按钮（会改变状态）

Payable函数，红色按钮（可以接受ETH）

![image.png](attachment:5a65c35f-3909-407a-9257-37db4007463b:image.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

















![image.png](attachment:5211f265-6361-475e-ae51-59654d33eee4:image.png)

![image.png](attachment:5c11b668-535e-4415-b6b6-8f19c1688dd6:image.png)

![image.png](attachment:324ff638-f490-47ae-9054-3bf3a6d3c66d:image.png)

## grammar

**四种变量可见性函数**

| 可见性 | 允许调用位置 | 常见用途 |
| --- | --- | --- |
| public | 合约内部+外部+子合约 | 默认接口+对外方法 |
| external | 外部调用 only | 对外API，节省gas |
| internal | 合约内部+自合约 | 内部逻辑 |
| private | 仅当前合约 | 封装细节 |

**modifier**:修饰符，给函数创建前置条件

```jsx
modifier onlyOwner(){
require(msg.sender == owner, "not owner");
_;//占位符，若符合条件在这里执行函数
}
```

**constructor:**构造函数，用于初始化设置（合约部署时自动执行一次）

```jsx
constructor(){
	owner=msg.sender;
	}
```

**receive()**:收钱函数;

```jsx
receive() external payable{} //专门从外部接受Eth
```

**calldata:** 外部调用的不可修改的函数参数；（省gas)

```jsx
function transferMany(address[] calldata users)//通常是external的函数参数，或者是复杂大的参数
```

**keccak256:**任意长度的bytes类型，变成32位的hash串

```jsx
bytes32 h = keccak256(data)
```

**require**(条件，“错误信息”）；

if true —→ 继续执行后续代码

if false —→ 立即停止，报错并且回滚所有操作 ❌

**emit:用来出发event的关键字，往区块链日志里写一条事件记录。**

```jsx
event Transfer(address from, address to, uint256 value);
function send(address to, uint256 amount) public{
balances[msg.sender]-=amount;
balances[to]+=amount;

emit Transfer(msg.sender,to,amount);
}
```

设计目的：

1、给前端/dApp用

2、给链下系统用 后端可以监听链上行为

3、比存storage便宜很多（省gas)

**call: low-level call**

```jsx
(bool ok,)=users[i].call{value:reward)(" ");//给用户地址i转reward wei数量的ehter 并且读取是否成功数据
address.call(bytes) //发起一次底层call,返回true或者false的数据

（bool ok,) = users[i].call{value: reward}(" ");
require(ok,"ETH transfer failed");
```

## 危险调用

**abi.encodePacked()**

功能：把几段字符拼接在一起，不加分隔符

比较安全的方法，拼接固定字节

-   `uint256` 固定 32 字节
    
-   `address` 固定 20 字节
    
-   `bytes32` 固定 32 字节
    

abi.encodePacked+动态类型 +hash/授权/校验 =！！！风险

## 英语单词

revert:回滚
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
