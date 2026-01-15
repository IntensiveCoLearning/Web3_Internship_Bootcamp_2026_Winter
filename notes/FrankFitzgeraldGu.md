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
