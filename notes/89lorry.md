---
timezone: UTC-8
---

# Miao

**GitHub ID:** 89lorry

**Telegram:** @mkylorry

## Self-introduction

大家好，我是Miao，毕业于南京大学，做过区块链与信贷系统科研，对实战感兴趣，希望通过本次学习能获得实战经历与对Web3更系统的认知 :)

## Notes

<!-- Content_START -->
# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->
pte中
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->

学习法语
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->


今天做了一半tokenization 明天继续
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->



### 一、 传统链上投票

传统区块链投票利用区块链作为公共账本，解决了中心化电子投票的信任危机，但同时也引入了极端的隐私暴露问题。

### 1\. 优势

-   **去信任化与不可篡改：** 不再需要信任中心化的服务器管理员。一旦投票记录上链，基于区块链的经济假设，历史记录无法被回滚或修改。
    
-   **端到端可验证性 (End-to-End Verifiability)：** 任何人都可以审计代码（智能合约）和数据（区块记录）。从选票提交到计票结果，全流程公开透明，第三方可独立复算。
    
-   **自动执行：** 投票结果直接触发智能合约逻辑，消除了人为干预计票的可能。
    

### 2\. 风险

-   **地址与行为的强关联：** 每一票都是一笔公开交易。所有人都能看到哪个地址（`from`）调用了合约，以及具体的参数（投给了谁）。
    
-   **链下身份推导：** 如果您的钱包地址曾在交易所 KYC、社交媒体或线下活动中使用过，攻击者可以轻易将您的链上投票行为与您的**现实身份**绑定。
    
-   **缺乏抗胁迫性：** 由于投票记录公开可见，选民容易遭受贿赂或胁迫（因为胁迫者可以查验你是否按要求投票）。
    

* * *

### 二、 零知识证明在投票系统中的“职能分工”

在 ZKVote 系统中，零知识证明（特别是 zk-SNARK）充当了一个“黑盒”，它允许用户在不泄露具体数据的前提下证明数据的有效性。具体分工如下：

### 1\. 身份隐私（我是谁？）

-   **机制：** 割裂钱包地址与投票权的联系。
    
-   **实现：** 您在链上注册的是一个**身份承诺**（$IdentityCommitment = Hash(Secret)$）。投票时，您不需要证明“我是地址 0xABC...”，而是证明“我知道某个 Secret，其哈希值存在于合法的选民名单中”。
    
-   **结果：** 验证者知道您是合法选民之一，但不知道具体是哪一个。
    

### 2\. 资格验证（我能投吗？）

-   **机制：** **Merkle Tree 成员证明**。
    
-   **实现：** 所有合法选民的 `IdentityCommitment` 构成一棵 Merkle 树。您在本地生成证明时，输入您的 `Secret` 和对应的 `Merkle Path`。
    
-   **结果：** 电路证明了“我的身份承诺在 Merkle 树的根（Root）之下”，从而确立投票资格。
    

### 3\. 防止一人多投（我投过吗？）

-   **机制：** **Nullifier（无效符）**。
    
-   实现： 电路强制要求输出一个确定性的标记：
    
    $$Nullifier = Hash(IdentitySecret, ElectionID)$$
    
-   **结果：** \* 因为 $IdentitySecret$ 不变，同一个用户在同一个 $ElectionID$ 下生成的 Nullifier 永远相同。
    
    -   智能合约记录所有用过的 Nullifier。如果有人试图再次提交，Nullifier 会重复，合约直接拒绝。
        
    -   外界无法通过 Nullifier 反推 $IdentitySecret$。
        

### 4\. **zk-SNARK和zk-STARK**

-   **zk-SNARK（Zero-Knowledge Succinct Non-Interactive Argument of Knowledge）**
    
    -   _Succinct_：证明大小很小，验证开销与原始计算规模无关或几乎无关；
        
    -   _Non-Interactive_：证明是单向的，不需要多轮交互；
        
    -   已被广泛用于隐私交易、zk-Rollup 等场景。
        
-   **zk-STARK（Zero-Knowledge Scalable Transparent Argument of Knowledge）**
    
    -   不依赖可信初始化（trusted setup）；
        
    -   更适合大规模、复杂计算的场景，但证明大小通常更大。
        

* * *

### 三、 ZKVote 系统完整流程（基于 zk-SNARK）

这是一个从“注册”到“审计”的闭环流程，核心在于将计算转移到**链下（浏览器）**，将验证留在**链上**。

### Phase 1: 身份注册 (Identity Registration)

1.  **本地生成：** 用户在浏览器生成随机的 $IdentitySecret$。
    
2.  **计算承诺：** 计算 $IdentityCommitment = Hash(IdentitySecret)$。
    
3.  **上链存储：** 用户发起交易，将 `IdentityCommitment` 发送给智能合约。
    
4.  **构建集合：** 合约将新的承诺插入 Merkle 树，并更新 Merkle Root。
    

### Phase 2: 生成证明 (Off-chain Proving)

_此步骤完全在用户浏览器本地完成，不涉及网络传输敏感数据。_

1.  **准备私有输入 (Witness)：** $IdentitySecret$、投票选项 (Vote)、Merkle Path。
    
2.  **准备公开输入 (Public Inputs)：** 当前的 Merkle Root、选举 ID ($ElectionID$)。
    
3.  **运行电路：** 浏览器运行 zk-SNARK 证明算法。
    
4.  **产出结果：** 生成 **Proof**（一串加密数据）和 **Nullifier**。
    

### Phase 3: 合约验证 (On-chain Verification)

1.  **提交交易：** 用户（或中继者）调用合约：`vote(Proof, Nullifier, Root)`。
    
2.  **数学验证：** 合约内的 `Verifier` 函数验证 Proof 是否由合法的 Key 生成，且满足电路约束。
    
3.  **防重检查：** 合约检查 `mapping(nullifier => bool)`，确认该 Nullifier 未被使用。
    
4.  **状态更新：** \* 标记 Nullifier 为 `true`（已使用）。
    
    -   增加对应选项的计票（如果是隐秘计票，则累加同态密文）。
        

### Phase 4: 公共审计 (Public Audit)

任何第三方观察者都可以查询区块浏览器：

-   **看到：** 有一笔交易提交了有效的 Proof，且使用了合法的 Merkle Root。
    
-   **确信：** 计票逻辑被正确执行（因为 Proof 验证通过）。
    
-   **不知道：** 具体的投票者是谁（匿名），以及（在某些设计中）具体的投票内容。
    

* * *

### 四、 工程实践中的关键假设与安全边界

在实际部署像 My First ZKVote 这样的系统时，必须注意以下工程细节：

### 1\. 关键假设

-   **可信设置 (Trusted Setup)：** zk-SNARK（如 Groth16 算法）通常需要一次初始化的仪式来生成 Proving Key 和 Verification Key。必须假设这个仪式的参与者中至少有一个是诚实的，且生成的“有毒废料（Toxic Waste）”已被销毁，否则伪造证明是可能的。
    
-   **客户端计算能力：** 生成零知识证明是计算密集型任务。假设用户的设备（浏览器环境）有足够的性能在数秒内完成计算，否则会导致用户体验极差。
    

### 2\. 安全边界与局限

-   **交易发起者的隐私（Relayer 问题）：**
    
    -   虽然 ZK 隐藏了“Merkle 树中的哪一个成员”投了票，但\*\*发起链上交易的钱包地址（Gas Payer）\*\*依然是公开的。
        
    -   如果用户使用注册身份时的同一个钱包发送投票交易，网络分析仍可能通过 IP 或资金来源关联身份。
        
    -   **解决方案：** 引入 **Relayer（中继网络）**，用户将 Proof 签名后发给中继者，由中继者代付 Gas 上链，彻底切断网络层关联。
        
-   **前端安全：** 所有的秘密（Secret）都在浏览器端生成和存储。如果前端网页被挂马或被恶意篡改，用户的 $IdentitySecret$ 可能会直接泄露，ZK 算法本身无法防御此类攻击。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->




今天去tesla试驾，本来对电车无感的，但是自动驾驶真的很厉害，完全不用动，以后会考虑。晚上喂了小浣熊几个葡萄。明天：

-   简明法语教程两集
    
-   web3两集，做nft
    
-   准备accenture终面
    
-   pte core了解一下大概，时间合适报名
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->





Web3运行原理

-   私钥：一个随机生成的字符串，由其派生出公钥和钱包地址（如以太坊地址为公钥末20字节加“0x”前缀）
    
-   助记词：通常为12或24个单词序列，用于派生和管理多个私钥，便于备份。
    
-   交易生命周期：从签名到上链 交易包含核心三要素：操作内容（如转账）、手续费（Gas费）以及防重放的序号（Nonce）。
    
-   数字签名确保交易真实性与不可伪造：钱包用私钥对交易信息签名，网络可通过公钥验证签名真伪。 Gas费是网络运行的“燃料”与安全机制：由Gas价格（单位成本）和Gas用量（操作复杂度）共同决定，用于防止垃圾交易、激励维护者，并在网络拥堵时形成价格竞争。 交易广播后需经排序、打包与验证：验证者节点（如以太坊需质押32个ETH）将交易按Gas价格优先级打包进区块（约12秒一个），并投票验证其有效性。
    
-   区块链的信任基础：共识、不可篡改与智能合约
    
-   区块链的不可篡改性需要时间来实现：新区块通过哈希值指向前一区块形成链条，初期并不安全。以太坊约需12-13分钟达到“最终确定”状态，此时篡改可能性几乎为零。
    
-   共识机制： 1. 工作量证明（PoW）：节点通过算力竞争打包权。 2. 权益证明（PoS）：节点通过质押资产获得随机打包权，作弊将被罚没。
    
-   智能合约是存储在链上的可执行代码：在虚拟机中运行，逻辑公开、不可篡改且可追溯，实现了“代码即法律”，将信任从人/机构转移至程序。 智能合约能显著降低交易信任成本：以自动交换合约为例，可无需中介（如律师、法院）自动完成资产互换，减少摩擦。
    
-   节点与验证者的分散性增强网络韧性：以太坊约有1.37万个节点（地理上美国、德国、中国较多）和约90.7万个验证者，数量越多，网络抗攻击和中断能力越强。
    
-   Web3的核心特性围绕去中心化构建： 无需许可，创建钱包和参与无需审核，但需支付Gas费防垃圾信息。 抗审查：交易难以被阻止，节点和钱包可替换。 开放开源，代码与交易记录完全公开。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->






-   用Etherscan验证交易在链上是否真实发生：
    

查以太坊主网 `https://etherscan.io/`

查Sepolia testnet  `https://sepolia.etherscan.io/`

-   做了unphihable的beginner部分，下面这道题还挺好玩的，学了punycode，解码网址`https://www.punycoder.com/`
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/89lorry/images/2026-01-14-1768370020555-image.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->







### 1.12

-   三年前就注册过metamask但是是为了写论文，没有实际操作过。今天尝试用测试币在两个账户间转账，用的faucet是google cloud web3，got 0.05 Sepolia ETH. in chinese 活水
    
-   转的是0.013 ETH，扣的是0.01305468 ETH，多出来的就是 Gas，模拟真实以太坊。去向，Base Fee直接销毁，Priority Fee给出块的验证者，所以刚刚那 0.000055 ETH一部分被永久销毁，一部分给了 Sepolia 验证节点。 MetaMask 赚钱不是从Gas里抽，它赚钱来自：内置 swap 的 0.875% fee（换汇），内置 bridge（跨链），某些聚合路由。普通转账它完全是零收入。
    
-   有用的笔记：ISyaxinLiu
    
-   教程：[https://humdrum-volleyball-4a2.notion.site/2e54867ff3e180b5bc84e33a7e2f9835](https://humdrum-volleyball-4a2.notion.site/2e54867ff3e180b5bc84e33a7e2f9835)
    
-   用了半年google cloud才知道还有个web3 testnet： [https://cloud.google.com/application/web3/protocols](https://cloud.google.com/application/web3/protocols)
    

![截屏2026-01-12 12.10.40.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/89lorry/images/2026-01-12-1768257676926-__2026-01-12_12.10.40.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
