---
timezone: UTC+8
---

# FrankFitzgeraldGu

**GitHub ID:** FrankFitzgeraldGu

**Telegram:** @F Frank

## Self-introduction

M.S. (CityU)

## Notes

<!-- Content_START -->
# 2026-01-29
<!-- DAILY_CHECKIN_2026-01-29_START -->
账户抽象是 Kite AI 实现智能体自治支付的技术引擎。它将钱包从单纯的签名工具转变为具备逻辑判断能力的智能合约 。  

### **ERC-4337 与智能合约钱包**

ERC-4337 引入了 `UserOperations` 和 `Bundlers` 的概念，允许开发者构建无需人类手动签名的自动化交互流 。  

-   **Gas 赞助（Paymasters）**：允许第三方（如 dApp 开发者或智能体调度平台）代付 Gas 费用。这意味着智能体无需预先持有网络原生代币即可启动，大大降低了冷启动门槛 。  
    
-   **交易批处理**：智能体可以将“授权代币”和“执行交易”封装在一个操作中，减少了链上交互次数，提升了效率并节省了成本 。  
    

### **EIP-7702：EOA 账户的智能升级**

EIP-7702 是对账户抽象的进一步革新，它允许传统的外部拥有账户（EOA，即普通私钥控制的钱包）在单次交易中临时“穿上”智能合约的外衣 。 对于智能体开发者而言，EIP-7702 的重要性在于：  

1.  **原址升级**：用户无需迁移资产到新的合约钱包，即可让现有的钱包支持智能特权 。  
    
2.  **委托执行（Delegation Designator）**：通过在账户代码中设置 `0xef0100` 前缀并指向特定的代理合约，EOA 可以临时具备批量转账、自动续费等功能 。  
    
3.  **高性能与低成本**：相比 ERC-4337 需要复杂的链外 Bundler 设施，EIP-7702 是协议级别的原生支持，执行开销更小 。    
      
      
      
    **开发流程与核心 API 调用**
    
    开发者在构建如“AI 购物助手”或“自动订阅管理” Agent 时，可以遵循以下流程：
    
    1.  **环境配置**： 安装 SDK：`npm install gokite-aa-sdk` 。  
        
    2.  **初始化 SDK**： 配置 RPC 和 Bundler 地址，连接到 Kite 测试网 。  
        
    3.  **创建智能体金库（Agent Vault）**： 为智能体部署一个可升级的合约账户，用于存储特定任务的资金 。  
        
    4.  **定义预授权规则（Spending Rules）**： 设定支出限额、有效期和目标白名单。
        

通过这种方式，智能体在执行“寻找并购买 NFT”的任务时，系统会自动验证其目标地址是否在白名单中，以及单笔金额是否超限。这不仅保护了用户的本金，也为智能体提供了独立行动的合法资产边界 。  

## **协议互操作性：x402 与 Model Context Protocol (MCP)**

Kite AI 并不是孤立的协议，它积极兼容现有的 AI 与支付标准，以实现真正的“万物互联”。

### **x402 协议：智能体商务的标准语言**

x402 是由 Coinbase 等机构推动的、专为智能体原生支付设计的协议标准 。它实现了：  

-   **链与币种无关性**：支持智能体在不同底层链和不同稳定币（如 USDC, PYUSD）之间进行结算 。  
    
-   **意图驱动的授权**：智能体不再发送简单的“转账指令”，而是发送“支付意图（Payment Intents）”。这些意图包含任务描述、资金托管条件和执行证明 。  
    

### **与 MCP 及 A2A 的集成**

-   **Anthropic MCP (Model Context Protocol)**：Kite 作为 MCP 的支付插件，允许 AI 模型在调用外部工具或数据库时实时结算费用 。  
    
-   **Google A2A**：通过与 Google 的智能体支付框架兼容，Kite 能够触达 Web2 领域庞大的 AI 服务生态系统 。    
    智能体协作生态：从微支付到信誉网络
    
    当多个智能体在网络中协作时，Kite 的信誉机制（Reputation Mechanism）成为了信任的“粘合剂”。
    
    ### 案例分析：多智能体结算网络
    
    在“智能体 A 提供数据服务，智能体 B 自动发起微支付”的场景中，Kite 的架构优势得以充分体现 ：
    
    1.  **建立状态通道**：两智能体在链下建立高速交互通道 。
        
    2.  **流式结算**：B 根据 A 提供的每条高质量回复，签名发送分毫级别的支付更新（如 $0.0001 / message） 。
        
    3.  **信誉累积**：所有的交易记录都会贡献给智能体的全局信誉分。表现良好的智能体可以获得更低的抵押要求或更高的信用额度 。
        
    
    ### LXDAO 社区参考项目：FairSharing 与 zkVote
    
    -   **FairSharing**：该项目提供了一种去中心化的贡献记录与验证机制。在智能体场景下，可以利用 FairSharing 的逻辑来验证智能体的工作量是否真实，从而触发合约自动发放奖励 。
        
    -   **zkVote**：利用零知识证明技术，允许智能体在不泄露其主人身份的情况下参与治理投票，确保了决策的隐私性与自治性 。
<!-- DAILY_CHECKIN_2026-01-29_END -->

# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->

**Kite AI 的技术架构、账户抽象、以及如何通过 x402 等协议构建自治商业网络**  
**智能体经济的崛起与基础设施错配分析**

在传统的 Web2 架构中，AI 系统的运行高度依赖于中心化的身份验证（如用户名/密码、SSO）和繁琐的 API 密钥管理 。这种模式对于需要进行高频次、跨服务协作的自治智能体而言是不可持续的。当一个智能体代表其主人执行复杂的购物任务或管理跨平台的订阅服务时，它面临的首要挑战是如何在没有人类实时干预的情况下证明其授权合法性，并安全地调拨资金 。

### **人类中心化架构与智能体自治需求的冲突**

当前的基础设施在凭证管理、身份验证和授权机制上存在根本性的局限性。人类使用的身份验证工具（如生物识别、二次验证）无法直接由代码执行，而传统的 OAuth 授权模式通常是基于会话的、一次性的，缺乏对智能体在运行时的动态权限控制 。    
**Kite AI 技术架构：专为智能体设计的 Layer-1 区块链**

Kite AI 并非简单的通用型区块链，而是一个针对智能体交易模式深度优化的专用 Layer-1 网络 。其架构设计的核心目标是解决智能体在执行任务时面临的信任、速度和成本问题。  
**核心设计支柱与性能指标**

Kite 网络采用了名为 SPACE 的框架来构建其技术底座。该框架涵盖了稳定币原生性（Stablecoin-native）、可编程约束（Programmable constraints）、智能体优先认证（Agent-first authentication）、合规性（Compliance）以及高效经济模型（Economics） 。  

### **专用支付巷道（Dedicated Payment Lane）**

在传统的以太坊等网络中，DeFi 协议的流动性挖矿或热门 NFT 的铸造往往会导致网络拥堵，使得 Gas 费用激增。对于依赖高频调用 API 的 AI 智能体而言，这种不确定性是致命的 。Kite AI 引入了“专用支付巷道”概念，为智能体交互提供保障带宽，确保支付指令不会受到无关网络活动的干扰，从而维持稳定的结算成本和低延迟体验 。  
**身份架构的三层防御体系：根权限、智能体与会话**

为了在赋予智能体自主权的同时防止灾难性的资金流失，Kite 引入了业界首创的基于 BIP-32 层次推导的身份管理模型 。这种模型将权力解耦，实现了安全性的“梯度管理”。  
**身份层级与权限边界**

1.  **用户层（根权限 / Root Authority）**：由人类用户持有的主钱包，作为信任的终极锚点。用户在此层定义全局策略（如每个智能体每天限额 $100），并存储核心私钥 。  
    
2.  **智能体层（委托权限 / Delegated Authority）**：每个自主程序拥有独立的加密衍生地址。智能体可以持有少量资金并在限定的白名单服务内进行操作，其身份与用户主钱包通过密码学手段绑定，但私钥不直接暴露给服务商 。  
    
3.  **会话层（临时权限 / Ephemeral Authority）**：针对特定的短期任务生成的随机密钥，任务结束或超时即作废。这种设计确保了即便特定的会话被攻破，攻击者也无法触及智能体层或用户层的核心资产 。    
    
    | 身份层级 | 身份类型 | 生命周期 | 安全影响范围 |
    | User (Root) | 永久 / 核心私钥 | 长期存在 | 拥有所有资产的最终处置权 |
    | Agent (Derived) | 确定性派生地址 | 与智能体存续期一致 | 仅限于分配给该智能体的资金与信誉 |
    | Session (Ephemeral) | 临时随机密钥 | 单次交互或短期窗口 | 仅限于当前会话的操作，泄露后自动失效 |
    
    这种多层架构不仅是一个安全图纸，更是一种责任清晰的“收据追踪”机制。系统可以清晰地溯源：用户批准了什么，智能体尝试执行了什么，以及最终结算了什么 。
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->


今天主要把几件“入门但很关键”的事情跑通了：一边在 Remix 里把 Solidity 的基本语法和交互流程练熟，一边把 SpeedRunEthereum 的 Challenge #0（Tokenization）从本地链一路推进到 Sepolia。整体感觉确实不难，但非常适合打基础——尤其是“合约怎么部署、怎么交互、交易为什么会失败、链上状态怎么验证”这些核心概念，今天算是都变得很具体了。

* * *

## 1）Remix：前三个练习跑通（HelloWorld / Counter / Primitives）

今天在 Remix 里完成了 Solidity by Example（Solidity 0.8.26 Basic）的前三个练习，流程也基本固定下来：  
**新建 .sol 文件 → 选 0.8.26 编译器 → 编译 → Remix VM (Cancun) 部署 → 在 Deployed Contracts 里点按钮验证。**  
这一套跑顺以后，做任何小实验都很快。

### HelloWorld：public 变量 = 自动 getter

HelloWorld 合约里就一个：

-   `string public greet = "Hello World!"`
    

部署后直接点 `greet` 就能读到值。今天最大的体会是：  
**public 状态变量会自动生成 getter**，所以不写函数也能读取，这对理解“合约接口”和“ABI 展示的东西”很有帮助。

### Counter：0.8+ 的整数安全检查真的会救命

Counter 合约里有：

-   `uint256 public count`
    
-   `get()` 只读
    
-   `inc()` / `dec()` 改状态
    

验证过程很直观：`inc` 一路加，`dec` 一路减。真正让我记住的是：  
当 `count == 0` 再点 `dec()` 会直接 revert。  
这一下把“Solidity 0.8+ 自动做溢出/下溢检查”这个知识点从概念变成了体验：  
**不是你手动判断，而是编译器层面就帮你守住了底线。**

### Primitives：默认值规则 + bytes 的小坑

Primitives 主要是把常见类型和默认值跑一遍：

-   bool 默认 false
    
-   uint/int 默认 0
    
-   address 默认全 0 地址
    
-   `type(int256).min/max` 能读到边界值（很长的十进制）
    

今天也踩了一个小坑：  
`bytes1 a`、`bytes1 b` 如果没写 `public`，Remix 就不会显示读取按钮；想在界面直接查，就要写 `bytes1 public a` 或自己写 getter。  
这让我更清楚：**Remix 里看到的按钮，其实就是 ABI 能调用的东西。**

* * *

## 2）SpeedRunEthereum Challenge #0（Tokenization）：从本地链到 Sepolia

今天把 SpeedRunEthereum 的 Tokenization 关卡完整跑通了一次，感觉它最大的价值是：  
让“本地玩具”变成“公网可见”，并且整个链上交互闭环真的跑起来。

### 环境准备：工具链对齐就能顺

Mac（Apple Silicon）下主要就是把 Node/Yarn 版本处理好：  
Node >= 20 没问题，Yarn 最后用 Corepack 启用 v4 搞定。工具链对齐后，后面基本一路顺。

### 本地链启动：第一次对 gas 的理解更真实

按教程开三个终端：

-   一个跑本地链
    
-   一个部署合约
    
-   一个跑前端
    

页面上用 Burner Wallet（临时钱包）交互时，我第一次在“本地链”也遇到“余额不足发不了交易”的问题。  
这一下让我明白：**gas 不是主网才有，本地链同样要遵守交易规则**。  
解决方法很简单：用页面 Faucet 给钱包打点测试币，然后 mint/transfer 就顺畅了。

### Mint & Transfer：所有权不是“说我有”，是链上可验证

我 mint 了几个 NFT，然后转给隐身窗口里的另一个临时钱包，再去 Debug Contracts 里查 `ownerOf(tokenId)`。  
看到 owner 真的变了那一下挺有感觉：  
**NFT 不是图片，核心是合约里可查询、可验证的所有权状态。**

### 部署到 Sepolia：真正跨出本地

后面把 hardhat 切到 sepolia：

-   生成部署账户
    
-   领测试网 ETH 做 gas
    
-   执行部署
    
-   配置自动回写到前端
    

最后还跑了 verify（提示已验证/跳过）。  
这一段让我更清楚：一旦上测试网，合约就开始变成“任何人都能查、能追踪”的公开资产，开发的心态也会更偏向真实项目。

* * *

## 3）今天的收获（为什么觉得轻松但很值）

今天这些作业难度确实偏入门，但它们覆盖了 Web3 开发最核心的几条主线：

-   **Remix 操作熟练**：版本、编译、部署、交互、看返回值
    
-   **ABI/可见性直觉更清楚**：public 才会在 Remix 里变成“按钮”
    
-   **0.8+ 安全检查是真机制**：下溢直接 revert，不是课本概念
    
-   **Gas 是硬门槛**：没钱就没交易，本地链也一样
    
-   **所有权是链上状态**：`ownerOf()` 一查就能验，转账结果立刻可证
    
-   **从** [**localhost**](http://localhost) **到 Sepolia 是关键跨越**：一上链就公开可见，开始像真实产品
    

总体来说，今天完成得比较轻松。
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->



## 从 Web3 到 AI Agent，再到 ICLR：

一方面，我在 Web3 实习计划中系统学习了区块链运行机制、安全、合规、社区与工具；另一方面，今晚 ICLR 的录取结果出来，我意识到自己正在研究的问题，和这一周学到的内容，其实在讨论**同一件事**。

那就是：

> **技术系统在规模化之后，究竟是走向多样性，还是被新的“隐形中心”重新塑形？**

### 一、Web3 给我的第一个冲击：去中心化不是自然结果

在学习以太坊运行原理、安全与合规的过程中，我第一次非常清晰地意识到：  
**去中心化并不是“默认状态”，而是一种需要不断被维护的结构。**

钱包、交易、共识、RPC、节点分布，这些设计初衷都是为了避免单点控制，但现实中依然会出现：

-   RPC 服务商的集中
    
-   工具链的路径依赖
    
-   用户在安全意识与便利性之间的妥协
    

Web3 的很多问题，本质上并不是“技术能不能做到”，而是**当人类追求效率时，系统会自然向哪里收敛**。

### 二、AI Agent 让我意识到：工具正在变成“行动主体”

在 AI × Web3 的分享中，我开始真正理解 Agentic AI 的意义。  
当 AI 拥有记忆、工具调用能力和链上身份后，它就不再只是“帮我写代码的助手”，而是：

-   可以代表某个目标持续行动
    
-   可以调用资源、花钱、做决策
    
-   甚至可以在没有人类实时介入的情况下运行
    

这一步非常关键，因为它意味着：  
**AI 不只是工具，而是开始参与“生态演化本身”。**

而一旦 AI 成为行动主体，我们就不得不面对：

-   谁为它的行为负责
    
-   它的决策依据是否可审计
    
-   它的偏好会如何影响系统结构
    

### 三、ICLR 这篇工作：我终于看清了“偏好是如何形成的”

正是在这样的背景下，我参与的这篇 ICLR 工作给了我一个非常清晰的答案。

我们研究的不是“模型准不准”，而是一个更长期的问题：  
**当越来越多开发者依赖 AI 编程助手时，技术生态会被推向哪里？**

结果非常直观，也非常残酷：

-   主流语言和框架，在 AI 辅助下成功率极高
    
-   小众语言、实验性技术，即使设计优秀，也更容易失败
    

这并不是因为它们“不好”，而是因为：  
**AI 的能力本身，继承了历史数据的不均衡分布。**

我们把这种现象称为一种 **“AI 引入的马太效应”**：

> 强者不是因为最好而胜出，而是因为 AI 更熟悉它。

### 四、当我把这三件事放在一起，出现了一个完整图景

把 Web3、AI Agent 和这篇 ICLR 工作放在一起看，我突然意识到一个让我既兴奋又警惕的事实：

-   Web3 试图通过协议设计抵抗中心化
    
-   AI Agent 正在成为新的高频决策者
    
-   而 AI 的“熟悉度”正在无意中决定技术生态的胜负
    

换句话说：  
**我们可能正在从“平台中心化”，走向“工具链中心化”，再走向“模型偏好中心化”。**

这不是某个公司或个人的阴谋，而是一个由效率、数据分布和生产力共同推动的结构性结果。

### 五、为什么这件事对未来非常重要

如果不正视这个问题，长期来看可能会出现几个后果：

-   新语言、新范式、新协议更难获得真实采用
    
-   开发者选择技术时，优先考虑“AI 是否友好”，而非技术本身
    
-   多样性下降，系统在黑天鹅面前反而更脆弱
    

这让我重新思考 Web3 中反复被提到的那些词：  
**去中心化、抗审查、多样性、长期鲁棒性。**

也让我意识到，未来真正重要的工作，可能不是“再做一个更强的模型”，而是：

-   让 AI 不再只放大既有优势
    
-   让新范式在 AI 时代仍有生存空间
    
-   让生态演化不完全被历史数据锁死
    

### 六、这一周给我的一个核心认知转变

如果用一句话总结我这一周的认知变化，那就是：

> **AI 不只是提高生产力的工具，它正在成为技术生态的“选择器”。**

而 Web3、AI Agent、隐私计算、治理与合规，恰恰都是在尝试回答同一个问题：  
**当系统规模足够大、行动足够自动化，我们如何仍然保留选择的自由？**

这也是为什么，今晚看到 ICLR 的结果时，我会如此激动——  
不是因为一个录取本身，而是因为我开始确信：  
**我正在研究的，是一个会长期存在、且值得被认真讨论的问题。**
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->




这一周参加 Web3 实习计划的学习，对我来说是一次从“概念理解”走向“系统认知”的过程，也让我逐渐意识到：Web3 并不是某一门技术或某一个岗位，而是一套高度现实、对个人负责能力要求极高的系统。

在技术层面，我重新夯实了区块链的底层认知。从比特币与以太坊的运行原理出发，理解了交易从签名、广播、打包到最终确认的完整生命周期，也更清楚了共识机制、Gas、分叉与最终性这些概念在真实网络中的意义。这让我意识到，Web3 的“信任”并不来自机构，而是来自密码学与共识设计，但代价是：安全责任被完全交还给个人。

随后在 Solidity 与 Remix 的学习中，我第一次把“链上规则”当作真正会产生后果的代码来理解。智能合约不是抽象逻辑，而是一旦部署就难以回滚的公开规则，任何一个设计疏忽，都可能直接演变为安全事故或资产损失。这让我对“代码即法律”有了更具体、也更敬畏的认识。

在 AI × Web3 的分享中，我最大的收获并不是某个新协议，而是认知上的转变。我开始理解传统 LLM 的边界，以及为什么 Agentic AI 必须引入记忆、工具和身份。当 AI 开始“替人行动”，身份、审计、支付与责任就不再是附加问题，而是基础设施。Web3 在这里的价值，并不是情怀叙事，而是提供了可验证的身份与结算方式，帮助 AI 从“黑箱工具”走向“可被信任的行动主体”。

安全与合规的内容给了我很强的现实冲击。无论是 Web3 攻击的工业化趋势，还是大陆清晰存在的负面清单，都让我意识到：技术并不能成为挡箭牌，很多风险并非来自代码，而是来自对边界的误判。尤其是出入金、授权签名和商业模式设计，这些看似“非技术”的问题，反而更容易直接触碰红线。

在运营与社区相关的分享中，我对 Web3 的流量生态也有了更清醒的认识。自然流量很小、刷量很普遍，很多指标并不等于真实增长。运营与 BD 的核心并不是“包装”，而是对赛道结构、用户分层和合作逻辑的理解，以及是否能持续产出可验证的案例。这让我对“案例”“作品集”这些词有了更现实的理解。

工具层面，我开始学习使用 Figma，不再把它当作单纯的设计工具，而是用来表达产品逻辑和用户路径的方式。好的界面不仅是好看，更是在关键操作（如授权、确认、交易）中帮助用户理解风险、做出正确决策。我逐渐意识到，设计也是 Web3 安全与产品可信度的一部分。

回顾这一周，我最大的变化并不是掌握了多少知识点，而是心态的调整：从“如何入行”转向“如何长期存在”。Web3 是一个高自由、高机会，但同样高风险、高责任的系统。想走得远，必须先理解规则、敬畏风险、建立可验证的能力，而不是只追逐短期机会。
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->





今天的学习内容集中在 **Figma 的基础使用与设计协作逻辑** 上。相比最初把 Figma 当成一个“画 UI 的工具”，这次学习让我开始意识到，它更像是一个**把产品想法可视化、并让团队对齐认知的协作平台**。

在具体操作上，我熟悉了 Frame、Auto Layout、组件（Component）和样式（Color / Text Style）的基本用法。这些功能本身并不复杂，但真正用起来之后，我才理解它们存在的意义：不是为了“画得快”，而是为了**降低后期修改成本**。例如通过组件和样式统一管理按钮、文字和颜色，当需求发生变化时，只需要在一个地方修改，就可以同步影响所有相关页面。

Auto Layout 给我的感受尤其明显。它强迫我在设计时就考虑内容的排列规则、间距和自适应行为，而不是单纯把元素“摆好看”。这让我第一次意识到，好的设计并不是静态截图，而是**能随着内容变化而合理伸缩的结构**，这和前端开发中的布局思维非常接近。

在协作层面，Figma 的价值更直观。多人同时编辑、评论区直接标注、历史版本可回溯，这些功能让我理解到为什么它会成为产品、设计和开发之间的“公共语言”。相比在文档里反复解释界面逻辑，一张结构清晰的设计稿本身就能减少大量沟通成本。

结合之前对 Web3 产品的学习，我也开始从“功能表达”的角度看设计。比如在涉及钱包授权、交易确认等关键流程时，界面本身其实承担了风险提示和用户教育的角色。如果设计表达不清晰，即使合约逻辑是安全的，也可能诱导用户做出错误操作。这让我意识到，**设计也是安全的一部分**。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->






今天的学习重点放在 **Remix IDE** 上，这是我第一次在不搭建本地环境的情况下，完整跑通一次“写合约 → 编译 → 部署 → 交互”的链上开发流程。相比单纯学 Solidity 语法，Remix 更像是把抽象概念落到实际操作中的一座“桥”。

一开始我对 Remix 的理解只是“网页版编辑器”，但真正用下来才发现，它几乎覆盖了智能合约开发的最小闭环：代码编写、编译版本控制、部署网络选择、账户切换、函数调用和交易记录查看。对初学者来说，这种“所见即所得”的反馈非常重要。

在编写合约时，我明显感受到 Remix 带来的一个变化：**每一次操作都更有“链上成本意识”**。比如在点击部署或调用写函数时，会立刻弹出 gas 消耗、交易详情和返回结果，让我清楚地知道哪些函数只是读取状态（不花 gas），哪些函数会真正修改链上状态（需要交易确认）。这比只在书本里理解 gas 概念要直观得多。

在编译阶段，我开始真正注意 `pragma solidity` 的版本声明。不同编译器版本对语法和安全特性支持不同，Remix 会明确提示当前合约是否使用了不兼容或过期的写法，这让我意识到**版本选择本身就是安全的一部分**，而不是随便写一个数字。

部署合约时，通过 Remix 的 JavaScript VM、测试网和 injected provider（连接钱包）几种模式，我第一次明确区分了“本地模拟环境”和“真实链上环境”的差别。特别是在连接钱包后，部署合约本身就是一笔真实交易，需要签名、消耗 gas，这让我更清楚地理解了：**部署不是上传代码，而是一次状态写入行为**。

在与合约交互的过程中，我对“函数调用”有了更具体的感受。读函数可以即时返回结果，而写函数则需要等待交易确认，并在链上留下记录。通过 Remix 面板查看返回值、交易 hash 和日志，让我第一次把“函数调用”和“区块浏览器里的交易”对应了起来。

整体下来，我发现 Remix 最大的价值不在于功能有多复杂，而在于它帮助我建立了一个正确的心智模型：  
**智能合约不是运行在服务器上的程序，而是被所有节点重复执行、并且每一步都需要付出成本的规则集合。**

今天的学习也让我意识到，在 Web3 开发中，“能写合约”和“理解合约在链上如何被执行”是两件不同的事。Remix 正好处在这两者之间，既不会被复杂工程配置劝退，又足够真实，能让我提前感知链上开发的约束和风险。

接下来，我计划继续用 Remix 多写几个小合约，重点关注权限控制、事件和状态变化，并逐步培养对 gas 成本和执行路径的直觉。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->







今天情绪有点悬着。

23 号本来被当成 ICLR 可能出结果的时间点，但也不确定会不会延期到 26 号。现在的状态更像是“知道快要揭晓，但不知道具体哪一天”，反而比明确的 deadline 更消耗心力。

这篇投的是 LLM 语言生成方向，属于我相对熟一点、也投入比较多精力的一条线。初始分数还可以，至少不算是“早早出局”的那种，但也没到让人完全安心的程度。现在更多是一种被动等待的状态，没法再做什么实质性补救。

我给自己设了一个现实的分叉点：

如果这次没中，明天就要立刻转头写 ICML 的摘要，重新整理贡献点和叙事角度，把注意力拉回到“下一次机会”。科研这件事就是这样，情绪可以有，但时间不会等你消化完情绪再往前走。

在等结果的间隙，我反而在想科研和 Web3 这两条线的关系。

以前我会把科研理解成一条相对封闭的轨道：论文、会议、审稿、影响因子；而 Web3 更像是另一套体系：产品、机制、社区、博弈。但最近越来越觉得，它们在“底层精神”上并不完全割裂。

科研追求的是可验证的创新：你的方法有没有新意，结论能不能被复现；

Web3 在理想状态下追求的，其实也是可验证的信任：代码是不是你说的那样，规则是否公开，行为是否可审计。

区别在于表达形式不同：

科研用论文、实验和引用构建声誉；

Web3 用合约、链上行为和历史记录构建声誉。

但两者都在试图回答一个问题：

凭什么相信你？

这也让我重新审视自己在 Web3 实习里学到的那些东西——身份、声誉、可验证输出、permissionless work——它们并不是和科研对立的，而是提供了另一种“把能力和贡献暴露出来”的路径。

不管 ICLR 结果如何，至少我越来越确定一点：

我不太想把自己锁死在单一叙事里。科研可以继续做，但我也希望理解技术走出论文之后，如何在更开放的系统里产生影响。

今晚大概就是这样。

等结果，随时准备转向，也尽量让等待本身不要变成空耗。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->








今天开始系统学习 Rust，最大的感受是：**Rust 不是在帮你写得更快，而是在强迫你一开始就写对**。它并不追求语法上的“好写”，而是通过编译期约束，把大量原本应该在运行期才暴露的问题，提前拦截在代码阶段。

从定位上看，Rust 是一门**系统级语言**，目标是在接近 C/C++ 性能的前提下，尽量避免内存安全问题。这一点让我立刻理解了它为什么会被大量用在区块链客户端、加密库、底层协议和安全敏感场景中。

Rust 给我带来的第一个冲击是**所有权（Ownership）模型**。在 Rust 里，每一块内存都有唯一的所有者，生命周期清晰；当所有者离开作用域，资源会被自动释放。这种设计从语言层面避免了悬垂指针和重复释放的问题，但也迫使我在写代码时真正思考：**这个数据是谁的？什么时候该被释放？是否允许被共享？**

与所有权紧密相关的是**借用（Borrowing）与引用（Reference）**。Rust 用非常严格的规则限制“同时可变与不可变访问”的情况：要么多个只读引用，要么一个可写引用，但不能同时存在。这一开始非常反直觉，但理解之后我意识到，这其实是在用编译器替我做并发安全和数据一致性的检查。

另一个明显的不同是 Rust 对 **错误处理** 的态度。它没有传统意义上的异常机制，而是通过 `Result` 和 `Option` 类型，强迫开发者显式处理失败情况。代码因此变得更啰嗦，但也更诚实——任何可能失败的地方，都必须在类型层面被承认。这让我联想到区块链和安全系统中的设计哲学：**失败不是意外，而是常态的一部分**。

在并发与安全方面，Rust 的设计让我感到“很克制”。语言本身就对数据竞争非常敏感，很多并发错误在编译期就无法通过。这也是为什么 Rust 经常被称为“Fearless Concurrency”——不是因为并发简单，而是因为不安全的并发根本写不出来。

从工程角度看，Rust 的工具链也给我留下了不错的印象。Cargo 把依赖管理、构建、测试统一在一个工具中，减少了环境不一致带来的问题。这种“默认正确”的工程体验，其实和 Rust 的语言哲学是一致的。

把 Rust 放回 Web3 语境里看，我开始理解它在生态中的角色：Solidity 更像是在定义**链上的规则与资产逻辑**，而 Rust 则更偏向**链下或底层系统的安全实现**，例如区块链客户端、共识模块、ZK/加密库、跨链组件等。两者解决的是不同层级的问题，但都高度强调“错误成本极高”。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->









### Solidity 学习总结

今天的学习重点放在 Solidity 语言本身，以及它在 Web3 中扮演的角色。Solidity 并不是一门通用编程语言，而是一种**为智能合约而生的语言**，主要运行在以太坊虚拟机（EVM）及其兼容链上。它的语法看起来像 JavaScript 或 C++，但底层语义完全不同，因为代码一旦部署就会成为链上规则的一部分。

与传统后端程序最大的不同在于：Solidity 写的不是“可以随时改的服务逻辑”，而是**不可篡改的执行规则**。合约部署后，代码本身就等同于法律，这也让我在学习语法时开始更关注安全性和边界条件，而不仅是功能是否实现。

在语法层面，Solidity 是一门**静态类型语言**。每个变量在编译期就必须确定类型，比如 `uint`、`address`、`bool`、`string` 等。合约中的状态变量会被永久写入链上存储，这意味着每一次写操作都会消耗 Gas，也意味着设计数据结构时必须非常克制。

合约本身可以理解为一个特殊的“类”，由状态变量和函数组成。函数定义了合约允许外界做什么，访问控制（如 `public`、`private`、`internal`）以及修饰符（modifier）决定了谁可以调用这些行为。这一点让我意识到，Solidity 中的“权限设计”几乎就是合约安全的第一道防线。

在数据结构上，`mapping` 和 `struct` 是最常用、也最关键的组合。它们承担了链上存储复杂关系的任务，比如地址到账户信息的映射、订单或 NFT 的结构化数据等。相比传统数据库，这些结构简单但成本高，因此更考验对业务核心数据的抽象能力。

事件（Event）是我之前容易忽略的一部分。事件本身不影响链上状态，但会被写入日志，方便链下系统（前端、索引器、分析工具）监听。这让我理解到，很多“用户感知”的状态变化，其实是通过事件而不是直接读链上存储完成的。

在开发流程上，Solidity 代码需要先被编译成 EVM 字节码，再通过交易部署到链上。部署本身就是一次不可逆操作，因此测试和验证远比在 Web2 开发中重要。哪怕是一个看似很小的逻辑错误，也可能在真实资金介入后被无限放大。

随着学习的深入，我逐渐意识到，Solidity 的难点并不在语法本身，而在于**它把代码、经济激励和安全假设绑在了一起**。合约既是程序，也是资产的管理者，更是信任的一部分。这也是为什么学习 Solidity 必须同时理解 Gas 成本、攻击模型和用户行为。

目前我给自己规划的学习路径是：先通过 Remix 写最小可用合约，熟悉部署和调用流程；再逐步引入权限控制、事件与复杂数据结构；随后学习常见安全问题与测试方法，最后通过完整小项目把这些能力串起来。

今天最大的收获是一个认知转变：  
**Solidity 不是“把逻辑写上链”，而是“把不可撤销的规则写上链”。**  
这也意味着，在 Web3 世界里，写代码本身就是在参与治理和风险分配
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->










今晚这场分享让我第一次比较系统地理解了「社区运营」到底在做什么。之前我对社区的理解偏向于“拉群 + 发公告 + 偶尔搞活动”，但听完之后才意识到，真正成熟的社区运营更像是一套**持续运转、可监控、可复盘的系统**。

在社区搭建层面，讲师强调的并不是“人数越多越好”，而是**结构设计**。包括社区是 public 还是 private、是否需要设置不同权限、管理员如何分工，这些并不是形式问题，而是直接关系到后期的信息秩序与风险控制。比如 topic 的数量并非越多越好，如果活跃度不足，反而会稀释讨论、降低参与感，需要根据社区阶段动态调整。

让我印象比较深的是对「数据面板」的强调。成员增长、发言人数、消息量、用户来源、语言分布、活跃时间段，这些指标并不是为了“好看”，而是用来指导下一步动作的依据。例如什么时候发布重要信息、是否需要多语言支持、是否要引入新的管理员，都是可以从数据中反推出来的。这让我意识到，社区运营不是凭感觉，而是**用数据不断修正判断**。

在活动策划部分，分享以 Twitter Space 为例，把活动拆成了一个完整闭环：准备、执行、复盘。准备阶段最重要的不是“快”，而是清晰——主题是否聚焦、目标听众是谁、问题库是否提前准备好、嘉宾邀请是否把讨论范围说清楚。宣发也不是临时喊人，而是需要提前几天预热，并在活动当天再次提醒，确保信息被真正看到。

执行阶段更考验运营的现场能力。主持人不仅要控节奏，还要兼顾项目方和嘉宾的背景介绍、讨论深度以及整体氛围。一场顺畅的 Space 背后，其实有大量“看不见”的准备工作。

复盘阶段是我之前最容易忽略的部分。分享中特别提到，不要被一些表面数据误导，比如 Space 的累计参与人数很容易被刷，真正有参考价值的是**实时在线人数**，甚至可以通过头像是否被折叠来判断水分。这一点让我意识到，运营的复盘能力，决定了下一次活动是否真的会变好。

整体听下来，我最大的收获并不是某一个具体技巧，而是认知上的转变：  
**社区和活动不是“热闹就够了”，而是每一步都应该有目标、有执行、有验证、有复盘。**  
运营能力的本质，不是话术或临场发挥，而是能否把一次活动、一次社区搭建，沉淀成下一次可以复用和优化的 SOP。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->











Privacy-Focused Browsers注重隐私的浏览器: Brave, Firefox (with Enhanced Tracking Protection),

DuckDuckGo browser often have stronger default protections.

Browser Extensions: Content Blockers (e.g., uBlock Origin): Block requests to known tracking domains/scripts using filter lists.   Behavioral Blockers (e.g., Privacy Badger): Learn which domains seem to be tracking you across sites and block them.

Other: VPNs (mask IP address), Tor Browser (anonymizes traffic)

The Core Privacy Challenge with Data

Personal Data: Information relating to an identifiable individual.      Sensitive Data: Subset of personal data needing extra protection (e.g., health, beliefs,finances).

Goal: Enable useful analysis (research, statistics) on datasets containing personal/sensitive info.      Risk: Analysis or data release might inadvertently reveal information about specific individuals.

Two Main Strategies: i. Anonymization: Modify the data itself to remove identifying links.  ii. Formal Privacy: Add constraints/noise to the analysis process to protect individuals mathematically

Data Anonymization: The k-Anonymity Approach

Quasi-Identifiers (QIs)准标识符: Attributes that are not unique on their own but can identify individuals when combined (e.g., ZIP Code邮政编码, Birth Date, Gender).单独看不具有唯一性，但与其他信息结合时可以识别个人的属性

k-Anonymity Goal: Ensure that for any combination of QIs in the dataset, there are at least k individuals sharing that combination. Makes individuals "hide in a crowd" of size k .确保在数据集中，对于任何一组准标识符的组合，至少有k个个体共享该组合。这使得个人在大小为k的“群体”中“隐藏”，从而保护个人隐私。Most based on generalization and suppression

Techniques: Generalization泛化: Replace specific values with broader ones (Age 28 -> 20-29 ).用宽泛的值代替具体的值             

Suppression抑制: Hide specific values ( \* ) 隐藏特定的值，例如用星号代替

Limitations of k-Anonymity

Homogeneity Attack同质性: In the example above, if you know someone lives in 90210 and is 30-39, you know they have HBP高血压, because everyone in that group does. kanonymity is met, but privacy is breached for the sensitive attribute!

Background Knowledge Attack: Attacker might know extra info (e.g.,"My neighbor Bob is in the dataset and is ~35"). This can help narrow possibilities even in a kanonymous dataset.

Curse of Dimensionality维度的诅咒: Hard to achieve k-anonymity with many QIs without

excessive generalization/suppression, destroying data utility.

(Advanced: l-diversity, t-closeness try to address homogeneity but have own

issues).

Formal Privacy: Differential Privacy (DP)

Different Philosophy: Instead of modifying the data structure, DP defines a mathematical property of the analysis algorithm or query mechanism.差分隐私不通过修改数据结构来保护隐私，而是定义了分析算法或查询机制的数学属性

Core Promise: The output of a DP analysis is statistically indistinguishable whether any particular individual's data was included in the input dataset or not.

Mechanism: Carefully calibrated random noise is added to the results of computations (counts, sums, averages, ML model weights, etc.).

Privacy Parameter (ε, epsilon): Controls the level of privacy. Lower ε = more noise,stronger privacy; Higher ε = less noise, weaker privacy

Why is Differential Privacy Often Considered Stronger?

Resilience to Background Knowledge: The privacy guarantee holds regardless of what external information an attacker possesses.

Protection Against Diverse Attacks: Inherently protects against linkage attacks, homogeneity attacks, differencing attacks, etc.Quantifiable Privacy Loss: Epsilon (ε) provides a measure of privacy loss.可量化的隐私损失Composition: DP guarantees compose predictably – you can track the total privacy loss (ε) across multiple queries (the "privacy budget").

Focus: Protects individuals while allowing aggregate statistical analysis.

Anonymization (like k-anonymity) modifies data to obscure individuals but has known vulnerabilities (homogeneity, background knowledge). Differential Privacy adds noise to analysis results, providing a formal, mathematical guarantee against inferring individual information. DP is robust against background knowledge and allows quantifiable privacy loss (ε) and composition. The choice involves a privacy-utility trade-off. DP often requires careful implementation to maintain usefulness.
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->












1.上午参加lxdao的周会，学习了很多区块链治理的知识

2.学习了mpc和zkp，概念总结如下：

The Need for Privacy-Preserving Computation

Problem: Often, valuable insights can be gained by combining data from different sources (e.g., hospitals, companies, individuals).     Constraint: This data is often sensitive and cannot be shared directly due to privacy regulations, business confidentiality, or personal concerns.    Goal: Enable computation on combined data without revealing the private inputs.      Key Technologies: Secure Multi-Party Computation (MPC) and Zero-Knowledge Proofs (ZKP).

What are Zero-Knowledge Proofs (ZKP)?

Goal: A Prover wants to convince a Verifier that a statement is true, without revealing why

it's true or any secret information related to the statement.

Example: Alice (Prover) wants to prove to Bob (Verifier) that she knows the solution to a puzzle, without showing Bob the solution itself.

Core Idea: Use a protocol involving challenges and responses that demonstrate knowledge or property adherence probabilistically or definitively, without leaking the core secret.

How do ZKPs work? (Conceptual Process)

Commitment/Claim: The Prover makes a claim or commits to possessing certain

knowledge (the "witness").证明者（Prover）提出一个声明或承诺拥有某种知识（即“证据”）

Challenge-Response (often iterative迭代): The Verifier issues challenges based on the claim. The Prover provides responses based on their secret knowledge (witness) and the challenge. Crucially, the responses don't reveal the witness itself.

Verification: The Verifier checks if the Prover's responses are consistent with the claim according to the protocol rules. If they are (after enough rounds, if iterative), the Verifier is convinced.

Key Properties of ZKPs

Completeness: If the Prover's statement is true (and they have the witness), they can always convince an honest Verifier.

Soundness: If the Prover's statement is false (or they lack the witness), they have only a negligible chance of fooling an honest Verifier.

Zero-Knowledge: The Verifier learns nothing beyond the fact that the statement is true. The proof reveals no information about the secret witness itself.

What is Secure Multi-Party Computation (MPC)?多个参与方在不泄露各自输入数据的情况下共同计算某个函数

Goal: Compute a function on multiple parties' private data without them revealing their data to each other.   Example: Alice has X , Bob has Y . They want to compute f(X, Y) (e.g., X + Y , Avg(X,Y) ) without Alice learning Y or Bob learning X .

Core Idea: Transform the data and the computation so no single party sees another's raw input.

How does MPC work? (Conceptual Process)

Input Sharing/Transformation: Parties process their private inputs using techniques like: ①Secret Sharing: Input is split into "shares", distributed among parties. No single share reveals the input. ②Homomorphic Encryption: Input is encrypted. Computations can be done directly on the encrypted data.Key Point: Raw private data is never directly exchanged

Secure Computation: Parties jointly perform calculations on the transformed (shared or encrypted) data following a specific protocol.  The protocol ensures calculations are correct without revealing intermediate values linked to original inputs.

Output Reconstruction: Parties combine their results (or decrypt the final encrypted result) to get the final output of f(X, Y) .  Ideally, only the final result is learned.

MPC: Key Goal Achieved

Input Privacy: Throughout the process (sharing, computation, reconstruction), no party learns the raw private inputs of others.

Correctness: The protocol ensures the final output is the same as if the computation were done centrally with all raw data (assuming no cheating/failures, which protocols also address).

Example Application: Combining MPC & ZKP: Scenario应用示例

Why combine them? MPC protects inputs during computation, but assumes inputs are correctly formed or honestly provided. ZKP can verify properties of inputs before they enter the MPC, without revealing the inputs.

Scenario场景: Multiple hospitals want to compute average patient recovery time (MPC) but need to ensure each hospital:

\* Only includes valid patient records (e.g., times are positive numbers).

\* Submits data consistent with a prior commitment (e.g., number of patients).

MPC: Considerations & Limitations

Input Privacy vs. Output Privacy: MPC primarily protects inputs. The output itself is revealed and might leak information. Example: If only two parties compute an average, knowing the average and your own input reveals the other party's input.虽然输入数据被保护，但计算结果是公开的。

Mitigation Strategies缓解策略: Use techniques like Differential Privacy on the output.  Agree only on aggregated/binned results instead of precise values.  Ensure enough participants to dilute information leakage.

ZKP: Considerations & Limitations

Proof Scope证明范围: A ZKP proves exactly the statement formulated. It doesn't inherently prove related things not explicitly included in the statement.  Example: Proving your list contains valid entries doesn't prove you included all relevant entries (omission is hard to prove directly).

Mitigation Strategies: Careful protocol design: Define precisely what needs proving.

External/Procedural Controls: Use pre-commitments (e.g., publicly declare dataset size before proving properties)

General Considerations (Both MPC & ZKP) 局限性①Complexity: Implementing and running these protocols can be complex.②Performance: Can be computationally intensive and require significant communication overhead compared to traditional methods. Performance depends heavily on the specific protocol choice and scale.③Assumptions: Different protocols rely on different cryptographic assumptions (e.g., hardness of factoring, discrete logarithms)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->













**今天参加了echo老师的colearing会议**

## 1) 我今天对 “QL / KOL / BD / 运营” 的理解更现实了

今天讨论一上来就围绕“QLBD 怎么提供 case studies、运营方向怎么提供作品”展开。我的第一感受是：这类岗位的“案例”并不是写得多漂亮，而是**你是否理解流量结构、目标用户、合作对象分层，以及你能不能把动作拆解成可复用的方法论**。

Echo 提到做 BD/合作，第一步不是乱找人，而是先熟悉赛道的 **头部/中部/尾部账号结构**，并且实际合作往往优先找头部 KOL。  
这让我意识到：很多“案例不足”的问题，本质是**对行业地图不熟**，不知道该用什么标准筛人、选渠道。

## 2) Web3 流量生态：不体面，但要看懂规则

分享里对推特生态的描述非常直白：为了流量需要高频互动、想“抽象点子”、甚至带有一定“蹭流量”的技巧；同时推特环境“恶臭”，会很耗精力，运营要具备“见人说人话”的能力。  
我把这段理解为：**流量工作本质是注意力竞争**，你可以不喜欢，但需要知道它怎么运作。

另一个点也挺颠覆：对于部分项目方而言，“黑红也是红”，甚至不太担心某些 KOL 的“人品”会带来反作用，因为很多人抱着“我能跑得比别人快”的投机心态。  
这让我更清楚 Web3 的一部分用户心理：**风险偏好高、追热点、重短期机会**。

## 3) 数据层面的现实：自然流量很小，刷量很普遍

我觉得最“扎心但有用”的信息是关于转化/流量的真实量级：

推特自然流量单条能到 **200+ 已经算很好**，很多项目在 **100 左右**；而外面看到的“几万点赞几万浏览”很多不可信，存在刷量。  
这让我反思：以后评估项目运营效果，不能只看“热闹指标”，要看**自然触达、互动结构（转发/点赞/评论比例）以及是否有真实增长动作**。

## 4) 从零做社区/增长：方法很“工程化”，但也有合规/舆情边界

Echo 提到从零运营社群的“最快方式”之一，是去找能发布任务的平台，让用户完成关注/点赞/转发等任务换积分或空投，这是 Web3 常见增长方式（撸毛群体）。  
同时，如果任务没人做，要反过来检查**任务是否过难、卡点在哪里，并多和用户沟通**。  
我学到的是：增长不是“拍脑袋”，而是**机制设计 + 反馈迭代**。

另外一个我觉得很实际的提醒：在国内高校场景做相关活动要谨慎，不建议做太大，容易敏感、甚至被约谈；以“兴趣小组”形式会更可持续。

## 5) 入行与职业建议：先定位，再用“可验证输出”积累机会

关于入行方向，Echo 的建议是先想清楚自己偏金融还是偏技术；如果两者都懂，可以考虑量化/交易所相关方向。  
他也强调：学校教的与一线需求差距大，建议多参加黑客松、和生态从业者沟通，带着目的问能力要求。 对于“如何把兴趣做成正循环”，他给出很现实的路径：前期当副业，用主业现金流补贴；即使能写出爆款内容也未必能立刻形成经济正循环。  
这点对我来说是“预期管理”：不要把短期涨粉等同于长期可持续。

ps:晚上的学员分享实在是太强了 大为震撼
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->















**今晚参加了Rick老师关于ai及基础概念的分享会**

## 1\. 今天我对 AI 的“第一层认知”更具体了

这场分享让我把“AI 很神秘”的感觉拆开了：传统大模型并不是魔法，而是一个**自回归的 token 预测器**——输入文本，算下一个 token 的概率，采样并拼接，循环直到生成完整回答。  
所以它很像一个“话痨学霸”：解释问题特别强，但让它**订机票、干活、跑流程**就不行。这个类比让我一下子听懂了“为什么 LLM 会说，但做不到”。

## 2\. 传统 LLM 的 3 个限制，以及 Agentic AI 怎么突破

我把今天最核心的对比记成一句话：  
**LLM = 只有脑子；Agentic AI = 脑子 + 记忆 + 手脚 + 身份。**

-   **限制 1：无状态/不记得**  
    以前对话窗口一换就“失忆”，上下文窗口也有限。  
    **突破方式：私有记忆（向量数据库 + RAG）**  
    流程是：采集数据 → embedding 向量化 → 存进向量库（Pinecone/Weaviate/Milvus 等）→ 提问时相似度检索 → 拼到 prompt 再回答。  
    我对这段的理解是：让 AI 从“金鱼”升级成“能翻资料的大象”。
    
-   **限制 2：没有工具/只能输出文本**  
    **突破方式：工具调用（Function Calling/MCP）**  
    AI 不直接“做事”，而是输出结构化的调用请求，程序去执行函数/API，再把结果回传给 AI。  
    这形成一个很清晰的循环：**思考 → 行动 → 观察 → 再思考（ReAct）**。  
    这部分让我意识到：所谓“能干活的 AI”，关键不在于模型多聪明，而在于它能否被接上工具链。
    
-   **限制 3：没有身份/不能代表谁行动**  
    这也是 Rick 说的“黑户问题”：AI 再强，没有身份就无法承担责任、签署协议、管理资产。  
    **突破方式：链上身份（ERC-8004）**（后面详述）
    

## 3\. 我开始理解：为什么 AI 走向“可用”会遇到信任与治理问题

Rick 提了一个让我印象很深的判断：AI 的普及不是技术线性增长，而会被“责任与治理”卡住。因为当 AI 开始替人签合同、花钱、做决策时，“出事谁负责”就变成硬问题。  
他提到的三个“AI Agent 互联网缺口”，我理解为都是**信任基础设施缺失**：

1.  **身份不可验证**：服务端无法判断请求来自谁（人/AI/哪家公司 AI）
    
2.  **行为不可审计**：日志在企业本地服务器，理论上可改可删，无法第三方验证
    
3.  **部署不可管控**：大量“影子 AI”在组织内游击运行，权限与数据访问不可控
    

我自己的反思是：我们以前总把 AI 当工具，但当它成为“行动主体”，就必须像对待员工或外包一样，有身份、有权限边界、有审计。

## 4\. Web3 在这里不是情怀，而是提供两样“刚需组件”：身份证 + 银行账户

今天最有冲击的一句话是：  
**Web3 能给 AI Agent 两样东西：不可伪造的身份证 + 可编程的银行账户。**  
我把它理解成：区块链擅长解决“在不互信环境里，如何证明你是谁、你做过什么、钱怎么结算”。

### 4.1 X402：把支付嵌进 HTTP 的“原生支付协议”

X402 的思路非常直觉：HTTP 里的 **402 Payment Required** 以前只是保留状态码，现在把它用起来——  
服务器返回 402 时，在响应头里给出**收款地址、金额、代币类型**，客户端钱包自动签名支付，再把支付证明带回去完成请求。  
我听完的总结是：它解决了 AI 付费的四个痛点：

-   不需要注册登录/信用卡/KYC
    
-   支持**微支付**（0.001 美元级别）
    
-   结算快，适合机器毫秒级交互
    
-   24/7 运转，更像“机器经济”的支付方式
    

同时也讨论到现实问题：链上每笔交易都有 gas 成本，所以会有**通道/批量结算/净额结算**等工程优化（类似“最后算总账”而不是每次都结算）。

### 4.2 ERC-8004：AI 的链上护照（身份 + 声誉 + 可验证）

ERC-8004 被描述成“AI 的链上身份证”。核心设计点我记了三块：

-   **身份：用 ERC-721（NFT）做唯一 ID**  
    每个 Agent 铸造一个唯一 token，链上可查、可转让（所有权可变更），也可加入能力声明、模型哈希、owner 等信息。
    
-   **声誉：链上评分 + 防刷机制**  
    只有与该 Agent 有真实链上交易/付费关系的地址才可评价（提高女巫攻击成本），评分不可篡改，还引入**时间衰减**（近期评价权重更高，允许“改过自新”但不完全洗白）。
    
-   **验证：TEE / ZKML（仍早期）**  
    用可信执行环境或零知识证明来证明 Agent “表里如一”，在不泄露模型/数据的情况下证明它确实运行过某模型并得到输出。
    

我个人觉得这套设计最重要的不是“酷”，而是它试图回答两个最现实的问题：  
**你是谁？你值不值得被信任？**

## 5\. Spoon OS / 框架视角：把复杂协议封装成开发者可用的 SDK

分享后半段讲 Spoon OS 的定位：把身份、支付、钱包管理、签名等底层复杂度封装起来，让开发者像写 App 一样开发 Agent，而不是自己“造 CPU + 写操作系统”。  
我把它理解为：

-   上层开发者专注业务逻辑、工具编排、策略；
    
-   底层框架提供钱包托管/私钥安全、接入 ERC-8004、接入 X402，以及必要的可验证记录。
    

## 6\. Q&A 对我最有价值的点：什么时候需要“Agent 身份”？

有同学问“量化交易把账户托管给 AI 不就行了吗，为什么还要 AI 身份？”  
Rick 的回答让我更清楚：

-   **自己玩的小工具**：不一定需要身份体系
    
-   **要做大、要让别人把钱交给你 Agent 管**：就必须让别人能验证你的 Agent、策略、历史记录与责任归属  
    本质是：身份与声誉是为了解决**可验证信任**，不是为了仪式感。
    

另外还有两个问题我也记下来了：

-   Web3 体系与 Web2 体系不是完全割裂，差别更多在**支付方式、审计与可验证性**
    
-   声誉体系目前仍可能被“开发者换号重来”绕过（声誉对 agent 而非 developer），这说明很多标准还在早期，需要继续迭代
    

## 7\. 今日复盘：我学到的不是“AI 黑科技”，而是“AI 走向可用的条件”

今天我更确定：

-   LLM 的强项是理解与生成；
    
-   Agentic AI 的关键是记忆与工具；
    
-   当 AI 进入“行动与交易”，就必须面对身份、审计、支付与责任。  
    Web3 在这个叙事里不是锦上添花，而是提供一套“可验证的信任与结算基础设施”。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
















今天连续参加了三场会议，分别是 Co-learning 讨论、Web3 安全主题分享以及 Web3 合规主题分享。整体感受是：**今天的内容比技术更“现实”**，让我开始真正思考在 Web3 中“如何活得久”，而不仅是“如何入行”。

## 一、Co-learning：自由的另一面是责任

在 Co-learning 中，讨论从钱包安全切入，但核心并不是工具本身，而是**人的风险意识**。让我印象最深的一点是：**即使私钥没有泄露，只要授权签名出现问题，资产依然可能被转走**。这打破了我之前“只要保管好私钥就安全”的简单认知。

同时也提到，代币的身份并不是名称，而是合约地址。同名代币、伪造价格显示在 Web3 中几乎没有门槛，这让我意识到链上世界并不会主动“帮你过滤风险”，一切判断都必须由自己完成。

在职业方向的讨论中，我逐渐意识到 Web3 并不存在所谓“轻松岗位”。无论是运营、BD 还是 DevRel，都要求对行业有持续、深入的理解，并且能输出**可被验证的真实经历**。相比传统简历，Web3 更看重你**实际做过什么、思考过什么**。

## 二、Web3 安全分享：攻击正在“工业化”

第二场安全分享用大量数据让我对行业风险有了更直观的认知。2025 年 Web3 发生了 1200 多起较严重的安全事件，损失超过 35 亿美元。攻击方式呈现出两种并行趋势：一类是针对项目方和核心人员的“精准猎杀”，另一类是面向普通用户的“广撒网式攻击”。

让我印象深刻的是，攻击早已不局限于代码漏洞，而是更多地结合了钓鱼、社工、AI 生成内容和自动化工具。攻击目标也逐渐从“系统漏洞”转向“人本身的判断失误”。

这让我意识到，Web3 安全并不是一个“学会某个工具就能解决”的问题，而是一种**长期习惯和系统性防御意识**。很多安全事故，并不是因为技术水平不够，而是因为忽视了最基础的风险判断。

## 三、Web3 合规分享：技术不能成为挡箭牌

第三场由刑事辩护律师主讲的合规分享，让我第一次从法律视角审视 Web3。律师明确提到，中国大陆对 Web3 存在清晰的负面清单，包括 ICO 融资、虚拟币交易所（尤其是永续合约）以及挖矿行为。

其中让我警觉的是“出入金风险”。大量案例并非主动违法，而是因为收到了来路不明的资金，甚至出现了所谓的“镀金攻击”——被动收赃也可能导致账户冻结。这让我意识到，在 Web3 中，“不知道”并不能成为免责理由。

此外，很多项目风险并不源于技术，而是来自商业模式设计，例如返佣机制、积分体系或法币对敲路径。一旦设计不当，就可能直接触碰刑事风险。

## 四、今日反思

今天三场会议从不同角度不断强化一个结论：  
**Web3 是一个高自由、高机会，同时也是高风险、高责任的系统。**

去中心化并不意味着没有规则，而是意味着规则更隐蔽、后果更直接、责任更个人化。无论是作为普通用户、从业者还是未来的项目参与者，都必须具备对安全和合规的基本认知。

对我来说，今天最大的收获不是某个具体知识点，而是心态上的转变：

想在 Web3 长期发展，必须先学会保护自己、理解边界，再谈机会和创新。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

















今天复习一下之前学过的区块链基础知识  
学习比特币白皮书《比特币:一种点对点的电子现金系统》和《021学习以太坊》开源教材  
**总结了一下术语和概念：**

•Hash: Map data of arbitrary size to data of a fixed sizeSecurity properties:1 Collision resistant(No

H has been proven collision-free)2Hiding:commit(msg):=(H(key|msg), H(key))Given com, infeasible

find msg3Puzzle-friendly(no solving strategy better than trying random values of x

•Blockchain: key idea: build data structures with hash pointers Useful trick: public

key=an identity verify(pk, msg, sig)==true double-spending attack Bitcoin entities

emulate a public trusted ledger 特点 pseudonymous(ECDSA) decentralized peer-to-2

•Centralized: Controlled by a single entity (如bank’s database)优Efficiency, clear control

劣 Single point of failure, censorship risk, potential for unilateral control/abuse.

•Decentralized: Control and data are distributed among multiple participants. Nosingle

entity has full control.优 Increased resilience, censorship resistance, transparency

(potentially).劣 Coordination challenges, potentially slower performance.

• Consensus Mechanisms: The Engine of Agreement

Goal: Allow independent nodes to propose and agree on the next valid block/update

to the shared ledger, ensuring everyone eventually has the same, consistent view.

Key Examples:

Proof-of-Work (PoW): Nodes (miners) compete to solve a computationally hard puzzle.

The winner proposes the next block.

Principle: Making block creation costly deters attackers. Requires significant energy.

Proof-of-Stake (PoS): Nodes (validators) are chosen to propose blocks based on the

amount of cryptocurrency they “stake” (lock up) as collateral.节点（验证者）是根据他们

作为抵押品“质押”（锁定）的加密货币数量来选择提出区块的。Randomly elect a leader in

every time step (based on their stakes)

Principle: Economic incentivvalidators lose stake if they cheat. More energy-efficient.

• Maintaining Consistency: Forks & Resolution

What is a Fork? 分叉 When two or more valid blocks are proposed at roughly the same

time, creating temporary diverging versions of the history.

解决分叉目的 To ensure the entire network eventually agrees on a single history.

•Is forking allowed in bitcoin blockchain? Is the consensus mechanism used by bitcoin

blockchain a deterministic or probabilistic one? Briefly explain 是否允许分叉比特币区

块链用的共识机制是确定性的还是概率性的: Yes, in two forms:Temporary forks:

Occur when miners create blocks simultaneously; resolved by the "longest chain

rule."Protocol forks:Hard forks (e.g.,Bitcoin Cash):Non-backward-compatible splits.Soft

forks (eg.SegWit):Backwardcompatible [upgrades.Is](http://upgrades.Is) Bitcoin consensus deterministic or

probabilisti?Probabilistic(Proof-of-Work):Transactions gain finality through confirmtion

(e.g., 6 blocks).Finality strengthens over time but remains statistical, not absolute.

•Nakamoto Consensus Resolution (PoW): The Longest Chain Rule

Nodes always try to extend the chain with the most cumulative proof-of-work (typically

the longest one). Over time, one chain attracts more work and outpaces others. Shorter,

abandoned forks become “orphaned”.

Result: Eventual Consistency the network converges

•Real-World Consideration: Scalability 可扩展性-A system’s ability to handle increasing

amounts of work (more users 或 transactions).

Challenge in Blockchains: Processing a high volume of transactions quickly can be

difficult. 瓶颈示例 Bottlenecks Examples: ①Block Generation Time: A fixed interval

limits how often new transactions are confirmed (e.g., Bitcoin ≈ 10 mins).②Block Size

Limit: Restricts how many transactions fit into each block.③Network Latency: Time for

blocks/transactions to spread across the network.瓶颈的影响 Impact: Limits throughput

(transactions per second). Improving scalability is a major area of blockchain research

(e.g., Layer 2, PoS variations, Sharding).

•BGP: 2f nodes can deadlocked between the nonfaulty (f nodes) and faulty (f nodes)

结论 If nf replies always contain more replies from honest nodes than Byzantine nodes,

we safe!• We set n==3f+1• We can always obtain nf,2f+1 votes. Then we have at least

f+1 votes from honest nodes§one more than number of potential faulty nodes

•Beyond Simple Ledgers 超越简单账本: Smart Contracts

Concept: Selfexecuting contracts with the terms of the agreement directly written into

code. They run on a blockchain network. Functionality: Automate processes

when predefined conditions are met. Enforce rules transparently.

Examples: Decentralized finance (DeFi), supply chain tracking, automated verification

checks. Execution Cost 执行成本(e.g., “Gas” in Ethereum): Users pay fees to

execute transactions or smart contract functions. Compensates node operators for

computational resources. Prevents network spam and resource exhaustion.

•Smart Contracts code execution 过程: • Deployed contracts (code) are triggered by

transactions• Transactions specify a TO contract address it sends to• Record on the

blockchain• EVM executes it with the deployed code• Code can:• Send ETH to other

contracts / individuals• Read / Write storage• Call (i.e., start execution in) other

contracts •Executed by all nodes in the Ethereum network•Ask any node to learn the

updated state of the EVM

•GHOST Protocal(Eth): if we increase block mining rate,More forks in a short time period,

which makes longest chain hard to find!区别 Orphan blocks in Bitcoin do not contribute

to longest chain rulebased consensus.But in GHOST, orphan blocks(uncles) are counted

when determinig heaviest subtree.Incentivizing honest but unlucky works

• Can we easily scale up the transaction confirmation rate in the bitcoin mining network

by enlarging the maximum block size? 通过增大区块的最大大小，我们是否可以轻松地

提高比特币挖矿网络中的交易确认率？: No. Increasing block size to scale Bitcoin’s

transaction rate faces critical trade-offs:1.Centralization Risk: Larger blocks require more

bandwidth/storage, pushing smaller nodes/miners out of the network.2.Propagation

Delays: Bigger blocks take longer to broadcast, increasing orphaned blocks and weakening

security.3.Temporary Relief Only: Even with larger blocks, Bitcoin’s 10-minute block interval

inherently limits throughput. Scalability solutions like SegWit, Lightning Network, or layer-2 protocols are preferred over

block size increases.

•Criticism of Selfish Mining: 1.While selfish miners may win more blocks than expected,

overall revenue could still be lower because of high orphan rateIf more blocks are

orphaned, less blocks receive block rewards Difficulty adjustment will resolve this

somewhat, original analysis doesnot fully take difficulty adjustment into account2.High

sunk costs Needs to maintain selfish mining for weeks before becomes profitable

•Gas system: • We cannot tell whether or not a deployed code will run infinitely• Waste

computing resources• Might damage system robustness•Ethereum solution: charge fee

per computational step (gas)• Special gas fees also apply to storage operations

晚上听了bruce老师的web3 运行原理

今天的课程系统性地讲解了以太坊与 Web3 的整体运行原理，让我第一次从“钱包 → 交易 → 区块 → 共识 → 网络参与者”的完整链路理解区块链，而不仅是停留在“转账或炒币”的表层认知。

### 1\. 钱包与私钥：真正的“资产主权”

课程一开始强调了钱包的本质：**私钥即身份，钱包即账户**。任何人都可以无需许可地创建钱包地址，这意味着资产完全由个人掌控，而不是由银行或平台代管。但这也让我意识到，去中心化并不等于“更安全”，一旦私钥或助记词泄露，资产将无法追回。因此 Web3 世界中，**安全意识本身就是门槛**。

### 2\. 交易不是转账那么简单

通过实际演示我理解了，一笔交易不仅仅是“给谁多少钱”，而是由**操作内容、Gas 费用和防重放的 nonce**共同组成。钱包的作用是帮助用户组装交易、用私钥签名并广播到网络，而真正决定交易能否被打包的，是 Gas 费用与网络拥堵情况。

### 3\. 数字签名与信任来源的改变

区块链中的信任并不依赖账号或机构，而依赖密码学。私钥可以对交易信息进行签名，任何人都能验证交易确实由该地址发出，却无法伪造。这让我意识到，Web3 的信任基础从“相信某个人或公司”，转变为“相信数学和共识机制”。

### 4\. 交易生命周期与区块确认

交易在签名后会进入交易池排队，由验证者挑选并打包进区块。以太坊平均每 12 秒出一个区块，但交易并非立刻不可篡改，仍需等待约 12 分钟进入 Finalized 状态。这一点对实际开发和支付场景非常关键，也让我理解了为什么“交易成功 ≠ 已经安全确认”。

### 5\. 共识机制与网络安全

以太坊目前采用 POS 共识机制，通过质押 32 ETH 成为验证者并参与投票。作恶将被罚没押金，从经济层面约束行为。虽然 POS 相比 POW 更节能，但也存在质押集中化的风险，这让我意识到**去中心化并不是一个状态，而是一种需要持续维护的过程**。

### 6\. RPC 与现实中的中心化问题

钱包并不直接连接区块链，而是通过 RPC 节点服务商。这是目前 Web3 中相对中心化的一环，也可能成为审查或单点故障的来源。好在以太坊支持多个 RPC 和自建节点，这让我理解到去中心化是“可替换性”的体现，而不是完全消灭中心。

### 7\. 智能合约：Code is Law

智能合约本质是部署在区块链上的可执行代码，规则公开、自动执行、难以篡改。通过 NFT Mint 合约示例，我直观理解了“代码即规则”的含义，也意识到合约安全与开源审计的重要性。

### 8\. 总结反思

这节课让我认识到，Web3 并不仅是技术革新，更是一种**信任结构、权力分配和协作方式的重构**。它带来了无需许可、抗审查和全球协作的可能性，但也把安全责任完全交还给个人。未来 Web3 是否能大规模落地，很大程度取决于：**如何在去中心化与用户友好之间取得平衡**。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



















**Web3 实习营第一天**  
参加了三个线上会议

**一是以太坊中文周会**

**1）技术进展**

Fusaka 升级：2025/12/03 成功激活，实现 PeerDAS 数据可用性采样，blob 容量上限提升至 21。

L2 生态：zkSync 计划于 Q1（2 月）进行 Atlas 升级；Arbitrum 因 Robinhood 代币化股票业务而活跃；StarkNet 近期经历约 18 分钟宕机。

**2）市场动态**

预测市场：Polymarket 与 Calibration 等平台受到关注。

永续 DEX 竞争：HyperLiquid 面临 Lighter、Aster 挑战，市场份额下滑。

**3）趋势展望**

稳定币：USDT、USDC 及人民币稳定币方向持续发展。

ETF 与机构化：大型机构/基础设施方参与度提升。

DeFi：衍生品市场工具持续升级。

RWA 代币化：2025 年增长显著，美债代币化规模扩张，2026 年预计持续增长。

**二是Co-Learning 共学答疑，助教老师回答了很多课程相关的问题**

**三是职业行业介绍会**  
**1）行业变化：从草莽到合规、从散户叙事到机构化**

机构入场与合规推进正在重塑行业底色，Web3 与主流金融/实体结合更紧密。

热点与长期赛道：RWA、DePIN、AI × Web3 等方向值得持续跟踪。

**2）就业市场：回暖但两极分化**

招聘在回升，但初级岗位竞争激烈，复合型/高阶人才稀缺。

企业更看重可验证的交付记录：项目、开源贡献、研究产出、链上行为等。

AI 工具使用能力正在成为默认要求，能显著提高个人效率与竞争力。

**3）岗位概览：结构 × 薪酬梯队 × 能力要求**

结构：技术岗占比最高，其次是产品/项目管理；运营增长、合规、审计安全、投研等构成关键拼图。

薪酬梯队：第一梯队（合约开发/安全审计/合规法务）起薪与上限更高；第二梯队（投研/核心产品）天花板高；第三梯队（运营增长）波动大但优秀增长可获得更高激励。

能力侧重点：开发重 Solidity 基础与安全意识（Rust/Move/Cairo 等加分）；运营增长重英语、Tokenomics 与增长闭环；产品/投研重底层理解与研究分析产出。

**4）求职方法论：你是谁不如你做过什么**

Web2 迁移：带着成熟的方法论与项目经验切入。

Permissionless Work：DAO 任务、开源贡献、研报/内容输出形成作品集。

链上声誉：GitHub、治理参与、公开作品与社区参与共同构成“可验证履历”。

**我今天明确的 3 个关键点**

1.行业正在走向合规与机构化，岗位更偏好复合能力。

2.Web3 更看重作品与可验证记录（交付与产出）而非纸面信息。

3.AI 将进一步挤压“只会一点点”的初级岗位，必须尽快形成独立交付能力。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
