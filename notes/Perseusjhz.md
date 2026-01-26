---
timezone: UTC+8
---

# REPSEUS JIAO

**GitHub ID:** Perseusjhz

**Telegram:** @perseusbless4u

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->
hi
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->

Hi I’m coming
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->


### 1\. 从 Data Structure 到 Social Structure

过去一周我们聊了 **Stacks**、**Queues**、**Merkle Trees**，你可能会觉得这只是程序员的玩具。但本质上，它们在做同一件事：**消除人为干预的空间**。

-   **The Logic:** 当一个系统的规则写死在 $O(1)$ 的算法里，而不是写在员工手册里时，这个系统就从“信任人”变成了“信任数学”。
    
-   **Term:** **Deterministic System (确定性系统)**。无论谁来操作，输入 A，结果一定得是 B。
    

### 2\. 效率的代价：为什么我们还在忍受“慢”？

回想一下我们对比栈和队列的快慢。在区块链里，我们为了 **Decentralization**，故意选择了一条“慢”路径。

-   **Redundancy (冗余):** 全球几万个节点都在重复计算同一笔账。从工程角度看，这简直是极大的浪费；但从安全角度看，这是唯一的生存之道。
    
-   **Trade-off:** 就像排队（Queue），虽然慢，但它保证了 **Fairness (公平)**。
    

### 3\. The Power of "Verifiable" (可验证性的力量)

还记得前两天的 **Merkle Proof** 吗？

-   在 Web2 时代，如果银行说你账上没钱了，你得去求他们查账。
    
-   在 Web3 时代，你拿着一个 **Hash** 就能在链上自证清白。这种从“被动接受”到“主动验证”的转变，就是 **Data Sovereignty (数据主权)** 的真谛。
    

### 4\. 2026 年的生存法则：Think in Code

如果你关注最近的趋势，你会发现懂点数据结构不再是后端开发的专利。

-   **Smart Contract Auditors:** 专门找合约漏洞的人，年薪已经涨疯了。
    
-   **On-chain Data Analysts:** 拿着 SQL 和 Python 在链上追踪黑客和巨鲸的动向。
    
-   **The Insight:** 所有的资产都变成了 **Code**，所有的交易都变成了 **Data Flow**。
    

### 5\. 一周复盘 (The Weekly Loop)

-   **Day 1-2:** 搞懂了区块的 **Linked List** 结构和激励机制。
    
-   **Day 3-4:** 拆解了 **Consensus**（谁有权记账）和 **L2**（怎么跑得快）。
    
-   **Day 5-6:** 杀入底层，理解了 **Merkle Tree** 和 **Dual-Pointer**（双指针）的效率哲学。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->



### 1\. Merkle Tree: 区块链的“骨架”

每个 Block 里面可能有几千笔交易。如果要把这些交易全部打包成一个“指纹”（Hash），区块链并不是简单地把它们排成一排。

-   **Mechanism:** 它是两两配对，算出 Hash，再把结果两两配对，一直往上算，直到最后只剩下一个 Hash。
    
-   **Merkle Root (根哈希):** 最终生成的那个唯一值就是根哈希。它就像一个**全家福摘要**，只要底层任何一笔交易变了一个标点符号，顶端的根哈希就会天崩地裂。
    
-   **Pivot Term:** **Hierarchical Hashing**（层级哈希）。
    

### 2\. SPV 验证：为什么手机能跑区块链？

这是最神奇的地方。你的手机钱包并不需要下载几十 GB 的完整账本，它只下载 **Block Headers**（区块头）。

-   **Merkle Proof:** 如果有人转了 1 BTC 给你，你只需要向节点索要几段特定的 Hash（路径），配合你手里的 **Merkle Root** 算一下，就能确定这笔钱是不是真的。
    
-   **Efficiency:** 验证 1000 笔交易，你只需要对数级（$log\_2 N$）的操作，而不是傻傻地查 1000 遍。这就是 **Computational Scalability**。
    

### 3\. State Machine: 区块链不仅是账本，更是“状态”

很多人以为区块链只是存历史记录，其实它在维护一个**最新状态 (Global State)**。

-   **State Transition:** 想象一个账户：账户 A 有 10 元，账户 B 有 0 元。
    
-   **Transaction:** A 给 B 转了 5 元。
    
-   **New State:** 状态机更新为 A=5, B=5。
    
-   **Insight:** 所有的 **Smart Contracts** 运行本质上就是在大规模地修改这些“状态”。以太坊甚至用了一种更复杂的 **Merkle Patricia Tree** 来管这些状态，让查询速度起飞。
    

### 4\. Memory vs Disk: 性能的真正瓶颈

昨天的笔记提到栈和队列谁快，在区块链节点里，**IO (输入输出)** 才是真正的杀手。

-   **State Triples:** 节点要频繁地从硬盘（SSD）里读取账户余额。
    
-   **Cache Strategy:** 聪明的开发者会利用 **LRU Cache** (最近最少使用缓存) 把热点账户存在内存里，这跟我们昨天聊的“栈的缓存友好性”逻辑是一通百通的。
    

### 5\. 今日脑暴：如果哈希被破解了怎么办？

既然区块链全靠 Hash 撑着，那如果 **Quantum Computing**（量子计算）成熟了，能瞬间算出 Hash 碰撞怎么办？

-   **Post-Quantum Cryptography:** 现在的研究方向是把现有的算法换成“量子抗性”的。
    
-   **My Take:** 别担心，如果量子计算真的强到能秒破 SHA-256，那全世界的银行系统、军事密码会先于区块链崩溃。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->




### 1\. AI + Blockchain: 硅基生命的“契约”

这是 2026 年最性感的叙事。

-   **Trust for AI:** 在这个 Deepfake（深度伪造）横行的时代，区块链成了内容的“真伪证”。通过在链上给视频或图片打上 **On-chain Watermark**，我们才能确定这段话确实是某人说的。
    
-   **AI Agents with Wallets:** 未来的 AI 助手不仅会帮你写代码，它还会拥有自己的 **Smart Contract Wallet**。它会自己去链上购买算力、支付 API 费用，甚至在 DAO 里参与投票。
    

### 2\. RWA: 现实资产的“数字化重生”

**RWA (Real-World Assets)** 正在把华尔街搬上链。

-   **Tokenization (代币化):** 现在的房地产、国债、甚至名画，都被拆解成了链上的 Token。
    
-   **Fractional Ownership (碎片化所有权):** 你不需要买下整栋楼，通过持有代表该房产 0.01% 的 Token，你就能享受租金分红。
    
-   **Liquidity (流动性):** 以前卖房子要几个月，现在在链上，资产的买卖就像买卖股票一样快。
    

### 3\. The "Invisible" Blockchain: 消失的技术名词

最好的技术，就是让你**感觉不到它的存在**。

-   **Account Abstraction (账户抽象):** 你不再需要手写那该死的 12 个助记词。现在的钱包支持邮件找回、指纹支付，**Gas Fee** 也可以用任何代币（甚至是人民币数字版）支付。
    
-   **Chain Abstraction (链抽象):** 用户不需要关心自己在用以太坊、Solana 还是 Layer 2。界面上只有一个“发送”按钮，底层逻辑会自动帮你完成跨链。
    

### 4\. Modular Blockchains: 像拼电脑一样拼公链

以前的链是“大一统”的（Monolithic），现在我们进入了 **Modular (模块化)** 时代：

-   **Specialization (专业化):** 有的层专门负责**执行 (Execution)**，跑得飞快；有的层专门负责**存数据 (Data Availability)**，又便宜又稳。
    
-   **Low Cost:** 这种分工让交易手续费降到了几分钱甚至更低。
    

### 5\. 今日终章思考：你是参与者，还是旁观者？

学了五天，最大的收获不应该是几个名词，而是这种 **Ownership (所有权)** 的思维：

-   Web1 给了我们信息。
    
-   Web2 给了我们交互，但拿走了我们的隐私。
    
-   **Web3 给了我们选择权。**
    

虽然目前还有法律监管（Regulation）和黑客攻击的阴影，但方向已经不可逆了。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->





### 1\. The Trilemma: 不可能三角的生死局

这是每个区块链开发者做梦都会梦到的三角形。你没法同时把这三个角顶到头：

-   **Decentralization (去中心化)**：节点要够多。
    
-   **Security (安全性)**：账本要够硬。
    
-   **Scalability (扩展性)**：速度要够快。
    
-   **现状：** Bitcoin 和 Ethereum 为了前两个，牺牲了速度。每秒处理几笔交易（TPS），在双十一这种高并发面前就是个弟弟。
    

### 2\. Layer 2: 给主链建个“高架桥”

为了解决慢和贵，天才们想了个招：既然主链（Layer 1）太挤，那咱们在上面盖一层 **Layer 2**。

-   **Off-chain Execution:** 咱们一万笔小额交易先在 L2 算清楚，最后只把一个最终的结算证明传回主链。
    
-   **Rollups:** 这是目前最火的技术。它把一堆交易“打包”成一个包发给主链。
    
    -   **Optimistic Rollups:** 默认大家都诚实，除非有人举报。
        
    -   **ZK-Rollups (Zero-Knowledge):** 利用高深的**零知识证明**，直接用数学证明包里的数据是对的。这是目前公认的终极方案。
        

### 3\. Wallet Security: 别把钥匙弄丢了

在 Web3，你就是自己的银行，这也意味着**你也是自己的保安**。

-   **Private Key vs Seed Phrase:** 私钥是钥匙，助记词（那12个单词）是母钥匙。**永远不要联网存储它们。**
    
-   **Hot vs Cold Wallets:** \* **Hot Wallet:** 联网的（比如手机 App），方便但容易被黑。
    
    -   **Cold Wallet:** 硬件钱包（像个 U 盘），不联网，资产最安全。
        
-   **The "Fat Finger" Risk:** 链上交易不可逆。转错地址了？不好意思，上帝也帮不了你。
    

### 4\. Cross-chain Bridges: 孤岛间的摆渡船

现在公链太多了（ETH, Solana, BSC...），它们之间互不相通。

-   **Cross-chain Bridge:** 通过“锁定 A 链资产，在 B 链生成映射资产”的方式实现跨链。
    
-   **The Risk:** 桥是目前黑客的“提款机”。因为桥上锁着巨额资产，一旦代码有 Bug，瞬间就会被搬空。
    

### 5\. 今日思考：Web2.5 是不是更好的路？

学到这儿你会发现，纯粹的去中心化代价太大了。

-   为什么我们要忍受高昂的 **Gas Fee**？
    
-   为什么非要自己保管私钥？
    
-   **My Take:** 真正的未来可能是 **Web2.5**。底层数据用区块链存，确保 **Ownership**；上层交互用中心化服务器优化体验。就像你买房子，房产证是在政府备案的（链上），但你平时进门刷脸（中心化体验）就行。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->






第四天呢
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->







### 1\. Consensus Mechanism: 怎么让全球节点“统一口径”？

在 **Decentralized**（去中心化）世界里，没有 CEO 下命令，节点之间必须有一套玩得转的规则。目前主流就这两大派系：

-   **PoW (Proof of Work) - 勤劳致富派**
    
    -   **Logic:** 典型代表是 Bitcoin。节点（Miners）通过拼算力、费电去解题。谁先算出来，谁就有 **Right to Write**（记账权）。
        
    -   **Insight:** 这种方式极度安全，但确实太费电了，而且 **Throughput**（吞吐量）比较低，每秒处理不了几笔交易。
        
-   **PoS (Proof of Stake) - 持股分红派**
    
    -   **Logic:** 以太坊 2.0 就在用这个。不再拼算力，而是拼你存了多少钱（Stake）。你押的币越多，被选为验证人的概率就越大。
        
    -   **Insight:** 环保、高效，但容易被吐槽“富者恒富”，有 **Centralization** 的风险。
        

### 2\. Smart Contract: 自动化的“法官”

如果说比特币是“电子金币”，那以太坊这种链就是“自动法律”。

-   **Logic:** **Smart Contract**（智能合约）是一段写死在链上的代码。它的逻辑是 `If X happens, then execute Y`。
    
-   **Trustless Execution:** 比如搞个去中心化保险，一旦延误险的数据输入进来，合约自动打钱，不需要理赔员审核。这就是 **Code is Law**（代码即法律）。
    

### 3\. Web3 的“积木”：Composable DeFi

搞清楚了共识和合约，就能理解为什么区块链能搞金融了。

-   **Money Legos:** 这是 Web3 特有的词。因为代码开源且标准统一，你可以把借贷协议、交易协议像乐高一样拼起来，创造出复杂的金融产品。
    
-   **Liquidity Pool:** 以前换汇得找银行，现在通过 **Liquidity Pool**（流动性池），代码自动根据算法帮你换钱，效率高得多。
    

### 4\. DAO: 没有老板的公司

既然有了共识机制和智能合约，大家就可以尝试一种新的组织方式——**DAO**（去中心化自治组织）。

-   **Governance:** 公司决策不再由几个高管拍板，而是持有 **Governance Tokens**（治理代币）的成员共同投票。
    
-   **Transparency:** 每一笔钱的去向、每一条提案的执行全在链上，没法暗箱操作。
    

### 5\. 今日脑暴：去中心化的边界在哪？

学到这儿，得跳出来想个问题：**是不是所有东西都值得上链？**

-   **Storage Cost:** 链上存数据极贵，存个高清电影能让你破产。
    
-   **UX Barrier:** 记不住 **Private Key**（私钥）资产就清零，这对普通用户太残酷。
    
-   **My Take:** 区块链解决的是 **Value Transfer**（价值传输）和 **Trust**（信任）的问题，而不是为了替代中心化服务器
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->








既然前两天把底层逻辑和交易流程都理顺了，第三天咱们得聊聊最“硬核”也最让新人头大的部分——**Consensus Mechanism（共识机制）**，顺便看看基于这些共识，现实中到底长出了什么样的应用。

说白了，前两天聊的是“账本长啥样”，今天聊的是“**谁有权写账本，以及写错了怎么办**”。

* * *

### 1\. 共识机制：怎么让全球几万台机器“统一口径”？

在去中心化世界里，没有 CEO 下命令，节点之间必须有一套玩得转的规则。目前主流就这两大派系：

-   **PoW (Proof of Work) - 勤劳致富派**
    
    -   **Logic:** 典型代表是 Bitcoin。节点（矿工）通过拼算力、费电去解题。谁先算出来，谁就有 **Right to Write**（记账权）。
        
    -   **Pros/Cons:** 极度安全，但太费电了，而且 **Throughput**（吞吐量）比较低。
        
-   **PoS (Proof of Stake) - 持股分红派**
    
    -   **Logic:** 以太坊 2.0 就在用这个。不再拼算力，而是拼你存了多少钱（Stake）。你押的币越多，被选为记账员的概率就越大。
        
    -   **Pros/Cons:** 环保、高效，但容易被吐槽“富者恒富”，有 **Centralization** 的风险。
        

### 2\. 区块链的“积木”：Composable DeFi

搞清楚了共识，就能理解为什么区块链能搞金融了。

-   **Money Legos:** 这是 Web3 特有的词。因为代码是开源且逻辑统一的，你可以把一个借贷协议（比如 Aave）和一个交易协议（比如 Uniswap）像乐高一样拼起来。
    
-   **Liquidity (流动性):** 以前换汇得找银行，现在通过 **Liquidity Pool**（流动性池），代码自动根据供需帮你换钱，这就是去中心化金融的核心。
    

### 3\. NFT 与 Metadata：不只是小图片

昨天的笔记提到过 IPFS，今天咱们拆解一下 NFT 的本质：

-   **Unique ID:** 每一份 NFT 在链上都有一个独一无二的 **Token ID**。
    
-   **Metadata (元数据):** 这个 ID 指向的是一个 JSON 文件，里面记着这个 NFT 的名字、属性，以及指向 IPFS 图片的链接。
    
-   **Key Point:** 你买的不是那张图，而是那张图在链上的**所有权证书**。
    

### 4\. DAO：没有老板的公司

既然有了共识机制和智能合约，大家就可以尝试一种新的组织方式——**DAO (Decentralized Autonomous Organization)**。

-   **Governance (治理):** 公司的决策不再由几个高管拍板，而是持有 **Governance Tokens**（治理代币）的成员共同投票。
    
-   **Transparency:** 每一笔支出的去向、每一条提案的执行，全在链上写着，没法暗箱操作。
    

### 5\. 今日脑暴：去中心化的边界在哪？

学到这儿，你得跳出来想一个问题：**是不是所有东西都值得上链？**

-   **Storage Cost:** 链上存储极贵，存个高清电影能让你破产。
    
-   **Performance:** 如果追求极致的社交体验（比如刷抖音），去中心化网络目前的延迟根本受不了。
    
-   **My Take:** 区块链解决的是 **Value Transfer**（价值传输）和 **Trust**（信任）的问题，而不是单纯为了替代中心化服务器。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->









# 今天我的感悟是关于，它是如何跑起来的：

### 1\. 交易的生命周期：从 Wallet 到 Blockchain

别把区块链想成一个网页，它更像是一个不停跳动的**状态机**。

-   **Initiate (发起)：** 你在 Wallet（比如 MetaMask）点下发送，这本质上是用你的 **Private Key**（私钥）做了一个数字签名。这个签名就是告诉全网：我有权动这笔钱。
    
-   **Broadcast (广播)：** 交易信息会飞向全网的节点。这时候交易还没入账，它待在一个叫 **Mempool**（内存池）的地方排队。
    
-   **Verification & Packing (验证与打包)：** 矿工从池子里挑出交易（通常谁给的 **Gas Fee** 高先挑谁），确认你的余额够不够、签名对不对。
    
-   **Finality (确认)：** 一旦区块被挖出来并连接到主链，这笔账就封死了。
    

### 2\. 智能合约：自动化的“法官”

如果说比特币是“电子金币”，那以太坊这种链就是“电子法律”。

-   **Smart Contract** 是一段写死在链上的代码。它的逻辑是 `If X happens, then execute Y`。
    
-   **Trustless Execution：** 比如搞个去中心化保险，一旦航班延误的信息输入进来，合约自动打钱，不需要理赔员审核，谁也赖不掉。这就是所谓的 **Code is Law**（代码即法律）。
    

### 3\. Web3 的工具箱：DApp 与 IPFS

Web3 看着高大上，其实拆开了看，架构跟 Web2 完全不同：

-   **DApp (Decentralized App)：** 你的前端可能还是 React 写的，但后端不再是中心化数据库，而是直接调用 **Smart Contracts**。
    
-   **Identity (身份)：** 以前是手机号登录，现在你的 **Wallet Address** 就是你的唯一通行证。
    
-   **IPFS (Storage)：** 区块链存数据很贵，所以大文件（比如 NFT 的图片）通常存在 IPFS 这种分布式存储网络里。
    

### 4\. 现实的骨感：去中心化的代偿 (The Trilemma)

昨天的笔记提到了挑战，今天得深挖一下这个著名的 **Impossible Trinity**（不可能三角）：

-   **Scalability (扩展性)**、**Security (安全性)**、**Decentralization (去中心化)**，这三者目前很难兼得。
    
-   为了安全和去中心化，以太坊这种公链就得牺牲速度（每秒处理交易量 TPS 比较低）。
    
-   所以现在大家都在搞 **Layer 2** 或者分片技术，试图在不破坏安全性的前提下，让链跑得快一点。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->










### 1\. 到底什么是区块链？(The "Immutable" Ledger)

很多人把区块链想复杂了，其实它就是一个**全网共享的账本**。

-   **它的物理结构：** 想象一个带锁的日记本，每一页（Block）都记满了账。每页纸最后都有一个根据这页内容生成的“指纹”（Hash）。
    
-   **为什么改不了：** 下一页的开头会写上上一页的指纹。如果你偷偷改了第一页的一个数字，它的指纹就变了，导致后面每一页的指纹都对不上。这种 **Chronological order**（时间序）让作恶成本变得极高。
    
-   **记账的人：** 以前只有银行能记账（Centralized），现在全球成千上万台机器都在同步这个账本。这就是 **Decentralization**。
    

### 2\. 激励机制：凭什么让人家帮你记账？

没人会白干活。比特币（BTC）其实就是发给这些记账员（Miners）的**工资**。

-   **Mining：** 矿工们通过消耗电力和算力去解一道数学难题，谁先解出来，谁就有权打包这个区块，并拿到系统派发的 **Block Reward**。
    
-   **Gas Fee：** 除了系统奖励，咱们转账给矿工的小费也叫燃料费。
    

### 3\. 三种链：花园、董事会和私人俱乐部

别被这些名词绕晕了，其实就是个**权限控制**的问题：

-   **公链 (Public Chain)：** 像公共公园，谁都能进。比如以太坊，完全去中心化，但也因为人太多，有时候很拥堵。
    
-   **联盟链 (Consortium Chain)：** 几个巨头（比如五大行）坐在一起开董事会。外人进不来，但在他们内部是共享的。
    
-   **私链 (Private Chain)：** 企业内部的私人领地，其实更像是一个高效的分布式数据库。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
