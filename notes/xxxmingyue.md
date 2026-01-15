---
timezone: UTC+8
---

# xxxmingyue

**GitHub ID:** xxxmingyue

**Telegram:** @reidz123

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
# 阅读《学习以太坊》

### 以太坊的发展

这里不过多介绍了

### 以太坊的核心

以太坊的核心是EVM，也就是以太坊虚拟机，个人理解是以太坊通过以太坊虚拟机，实现将智能合约以区块链的方式存放在每一个节点中进行计算，并确保其真实性和唯一性。

相对于比特币只是单纯的记录电子货币的流动，以太坊的伟大之处在于将智能合约代码也作为区块链需要记录的对象进行流动、计算

> Q：有一个疑问，以太坊的货币ETH是怎么被挖出来的，BTC是通过POW机制被挖出来的，以太坊是通过什么机制实现的奖励？ A：以太坊前期也是POW，当然矿工可以获得对应的ETH作为激励，后续转换给POS后，那么被选中的节点同样按照事先定义的激励方式获得其ETH激励。看了后续：包括：共识层激励、执行层激励和额外激励

### 关键词

**智能合约**：

运行在区块链上的自动化程序代码。它就像一份“自动执行的合同”，一旦满足了预设的条件，代码就会自动执行，无需第三方（如律师或银行）介入。

**ERC-20**：

以太坊最强大的功能之一是发行资产。为了方便流通，社区制定了统一的接口标准（ERC）。 ERC-20是用于发行**同质化代币**的标准。这意味着每一个代币都是完全一样的，可以互换。

**ERC-721/ERC-1155标准**

**ERC-721**： 定义： 每一个代币都是独一无二的，有独立的编号（Token ID），不可互换。 应用： 数字艺术品（如 BAYC 无聊猿）、独特的ENS域名。

**ERC-1155**： 定义： 一个更加灵活的多代币标准。它允许在一个智能合约中同时包含同质化（ERC-20）和非同质化（ERC-721）代币，并且支持批量转账，极大节省 Gas 费。

**GAS**：

在以太坊网络上进行任何操作（转账、执行智能合约）所需要支付的**手续费**。

GAS的计算：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-15-1768484810817-image.png)

你可以把它理解为：

> **总油费 = 你需要的油量（升） × （官方今日油价 + 你愿意多给加油员的小费）**

(1) **Gas Limit (燃料限制/用量)** —— “车程有多远”

**含义：** 这笔交易**复杂程度**的量化指标。操作越复杂，代码行数越多，需要的计算资源（Gas）就越多。 **谁决定的？** 由**智能合约的代码逻辑**决定，通常是固定的（或者有个大致范围）。

常见操作的“gas Limit”参考表：

| 操作类型 | 大致 Gas 用量 (Gas Limit) | 复杂度 |
| --- | --- | --- |
| 普通转账 (ETH) | 21,000 (固定值) | 最简单，就像去邻居家串门 |
| 代币转账 (USDT) | 45,000 ~ 65,000 | 稍微复杂，要改写账本数据 |
| NFT 转账 | 85,000+ | 中等 |
| Uniswap 交易 (Swap) | 150,000 ~ 250,000+ | 复杂，涉及多个合约调用 |
| 复杂的 DeFi 操作 | 500,000+ | 非常复杂，就像长途跨省旅行 |

(2) **Base Fee (基础费)** —— “官方指导油价”

-   **含义：** 网络根据当前的拥堵情况，自动算出的**最低单价**。
    
-   **特点：**
    
    -   **必须支付：** 如果你出的钱少于这个价，网络根本不理你。
        
    -   **被销毁 (Burned)：** 这部分钱**不给矿工**，而是直接从网络中“销毁”消失了（这是 ETH 通缩的主要原因）。
        
    -   **动态调整：** 用的人多，下一块区块的 Base Fee 就涨；用的人少，就跌。单位通常是 **Gwei** (1 Gwei = 0.000000001 ETH)。
        

(3) **Priority Fee (优先费/小费)** —— “给加油员的加急费”

-   **含义：** 你额外付给验证者（以前叫矿工）的钱，为了让他们**优先打包**你的交易。
    
-   **场景：**
    
    -   **平时：** 给一点点（比如 0.05 Gwei）意思一下就行。
        
    -   **抢购 NFT/土狗币时：** 大家都在排队，谁给的小费高，验证者就先处理谁的交易（这就是所谓的 Gas War）。
        

2.  **计算案例**
    

假设你想在 Uniswap 上把 ETH 换成 USDT。

-   **场景数据：**
    
    -   **操作复杂度 (Gas Limit)：** 假设需要 **200,000** 单位 Gas。
        
    -   **当前网络拥堵 (Base Fee)：** **20 Gwei**。
        
    -   **你想快点成交 (Priority Fee)：** 给 **2 Gwei** 小费。
        
-   计算过程：
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-15-1768484836687-image.png)

-   换算成 ETH：
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-15-1768484851273-image.png)

-   所以，这笔交易你要付 **0.0044 ETH** 的手续费。
    

**Dencun**：

以太坊在 2024 年 3 月完成的一次重大硬分叉升级的名称。 名字由来： 它是两个升级词的组合：Deneb（共识层升级）+ Cancun（执行层升级）。大家习惯简称为“坎昆升级”。 主要目的： 大幅降低 Layer 2（二层网络，如 Arbitrum, Optimism）的交易费用。此次升级的核心内容就是引入了下面的 EIP-4844。

**EIP-4844升级**：

Dencun 升级中最重要的技术提案，也叫 Proto-Danksharding。 引入的新概念：Blob（二进制大对象）。 通俗比喻：给卡车挂一个廉价的临时货箱。 以前： 二层网络（L2）要把交易数据存回以太坊主网，必须占用昂贵的“车内座位”（Call Data），导致路费很贵。 现在 (EIP-4844)： 以太坊给这些数据提供了一个外挂的“临时货箱”（Blob）。这个货箱空间大、价格极其便宜，而且数据过段时间会自动删除（不永久占用主网空间）。 结果： 使用 L2 网络转账的手续费从几美元降到了几美分甚至更低。

**分片与 Danksharding**

分片 (Sharding)： 这是以太坊早期的终极扩容愿景。原本的想法是将以太坊区块链像切蛋糕一样切成 64 个小链（分片），让它们并行处理交易。但这个方案技术难度极高，后来被放弃了，转向了 Danksharding。

Danksharding： 这是以太坊当前的终极扩容路线图。它不再追求把区块链切成小块去执行交易，而是专注于让以太坊成为一个巨大的、廉价的数据存储层，专门服务于 Layer 2（Rollups）。

两者关系： EIP-4844 是第一步（被称为 Proto-Danksharding，即“原初版”）。

Danksharding 是未来的完全体，它将进一步扩大 Blob 的数量，实现极高的吞吐量。

升级代理模式（Proxy Patterns）

区块链的特性是“不可篡改”，这意味着代码一旦上链就写死在石头上了。如果你发现了一个 Bug，或者想加个新功能，你是没法直接修改原合约的。

“部署新合约 + 迁移数据” 是最笨的办法（就像因为手机软件要更新，让你扔掉旧手机买个新的，还得把通讯录一个个手抄过去），成本极高且用户体验极差。

所以，升级代理模式（Proxy Patterns） 应运而生。它的核心思想是：“肉体（数据）”和“灵魂（逻辑）”分离。

升级代理模式包括：

-   透明模式：升级合约由proxy决定，而不是逻辑合约决定。
    
-   UUPS模式：升级合约写在逻辑合约中，通过upgradeTo实现
    

# 共学

如何在尽可能合规的角度，带来业务的增长，不管是技术岗还是非技术岗，公司的目的都是为了更好的增长，围绕这个目标，

技术人员需要实现稳定、可靠的代码，实现稳定的产品、得出可靠的数据分析结论等等

非技术人员在社群运营、活动运营等等方面，设计有效的激励方式、活跃用户生态。

强大的模仿能力： 比如从X上看到一个优秀的帖子，去分析，为什么他的内容这么好？我如何能够模仿，甚至去超越。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

# 完成区块链完全-访问控制漏洞的撰写

使用githubpages搭建了个人博客：\[xxxmingyue的个人博客\]([http://xxxmingyue.github.io](http://xxxmingyue.github.io))

目前只搭建了框架，未来打算将Ethpanda相关的学习内容更新到个人博客上

**可查看下面的文章**

[访问控制漏洞](https://xxxmingyue.github.io/#/notes/%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6%E6%BC%8F%E6%B4%9E)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


# 线上共学讨论

1.  **什么是DAO？**  
    DAO（Decentralized Autonomous Organization，去中心化自治组织）是一种基于区块链的组织形式，通过智能合约自动执行规则，由社区成员共同治理，无需中心化权威。
    
2.  **DAO的治理和其他方式有什么不同？**  
    DAO采用去中心化、透明、基于代币投票的治理机制，决策权分布于所有持币成员，而非集中于管理层或董事会。其规则写入代码（智能合约），一旦部署难以篡改，强调程序正义和社区共识。
    
3.  **如何实现DAO的良好运行？**
    

-   设计清晰、公平的治理机制（如投票权重、提案流程）
    
-   建立激励机制以鼓励参与和贡献
    
-   保证技术安全（如智能合约审计）
    
-   维护社区活跃度与信任
    
-   逐步演进治理结构，避免僵化
    

4.  **Layer 1 和 Layer 2 的区别**
    

-   **Layer 1（L1）**：指底层区块链主网（如以太坊、比特币），负责核心共识、安全性和数据最终性。
    
-   **Layer 2（L2）**：构建在L1之上的扩展方案（如Rollups、状态通道），用于提升交易速度、降低费用，将大部分计算移出主链，再将结果提交回L1以保障安全性。  
    简言之：L1是“主干道”，L2是“高速辅路”，用于缓解拥堵。
    

# Unshipable

## Exclusive Token Airdrop

实际上是你发起一笔交易，把你的sepoliaETH发送攻击者

## USDT Approval Phishing Demo

实际是授权恶意合约对账户的无限额的USDT提取权限
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



\# 学习Unphishable：

\## Web3 Wallet Setup Guide

包括如何安metamask钱包，设置助记词、链接sepolia测试网络，获取对应代币

\[Wallet Setup Guide\]([https://unphishable.io/challenges/wallet-setup-guide?lang=en](https://unphishable.io/challenges/wallet-setup-guide?lang=en))

**步骤**：

按照教程，完成小狐狸钱包安装配置-领取测试币，连接Dapp，确认完成

![Snipaste_2026-01-12_11-21-31.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-12-1768214784033-Snipaste_2026-01-12_11-21-31.png)

\## Wallet Recovery Assistant

1\. 提交问题说明

![Snipaste_2026-01-12_11-23-10.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-12-1768214825335-Snipaste_2026-01-12_11-23-10.png)

2\. 拒绝泄漏助记词，结束对话

![Snipaste_2026-01-12_11-23-43 1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-12-1768214843790-Snipaste_2026-01-12_11-23-43_1.png)

重点，禁止泄漏助记词给任何人

\## USDC Permit Phishing Demo

进入该关卡后，选择授权，会弹出恶意授权，无限制代币使用权限，选择拒接（取消），完成该关卡。

1\. 点击发起授权签名：

![Pasted image 20260112113949.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-12-1768214897864-Pasted_image_20260112113949.png)

2\. 弹出授权，选择取消

![Pasted image 20260112114040.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-12-1768214912445-Pasted_image_20260112114040.png)

\# 铸造自己的第一个NFT

访问：\[My First NFT | A non-profit instructional project for Web3 newbies. Get a FREE NFT while learning about Web3, underlying values of NFT, and security principles.\]([https://nft.myfirst.io/](https://nft.myfirst.io/))

按照要求注册MetaMask钱包，连接钱包，选择你的NFT头像，点击铸造

![Snipaste_2026-01-12_15-34-45.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-12-1768214950631-Snipaste_2026-01-12_15-34-45.png)

确认交易后，会支付sepolia的测试网eth的gas费用，然后铸造成功，可在metamask的收藏品中查看：

![Snipaste_2026-01-12_15-35-23.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-12-1768214968018-Snipaste_2026-01-12_15-35-23.png)

\# hello Web3

完成metamask钱包创建，可参考\[Wallet Setup Guide\]([https://unphishable.io/challenges/wallet-setup-guide?lang=en](https://unphishable.io/challenges/wallet-setup-guide?lang=en))

领取sepoliaEth测试币

向同学完成转账：

![Snipaste_2026-01-12_15-58-21.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-12-1768214989324-Snipaste_2026-01-12_15-58-21.png)

etherscan可查看该笔转账：

![Pasted image 20260112183758.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-12-1768215005909-Pasted_image_20260112183758.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
