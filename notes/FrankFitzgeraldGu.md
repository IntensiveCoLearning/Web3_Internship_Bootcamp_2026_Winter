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
