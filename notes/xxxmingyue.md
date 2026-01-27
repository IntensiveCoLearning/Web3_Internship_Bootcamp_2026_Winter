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
# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->
# 体验了 MyFirstZKVote

### 1\. 核心目标：我们要解决什么问题？

在普通的区块链投票中，大家都能看到「地址 A 投给了 选项 1」。 在 **ZK 投票**中，我们要实现：

1.  **资格验证**：证明我有权投票（我在名单里）。
    
2.  **匿名性**：没人知道具体是「谁」投了这一票（切断 钱包地址 <-> 投票选项 的联系）。
    
3.  **防重复**：虽然我是匿名的，但我不能投两次。
    

* * *

### 2\. 第一阶段：生成身份（入场券）

_对应文档中的：「生成身份秘密与承诺」_

在你投票之前，你需要在本地（你的浏览器里）制造一张「入场券」。

-   **秘密（identitySecret）：** 相当于你自己在纸上写了一个只有你知道的随机密码。**这个绝对不发给任何人，也不上链。**
    
-   **承诺（identityCommitment）：** 你把这个密码放进一个透明但锁住的保险箱里。大家都能看到保险箱（承诺），但打不开。
    
-   **注册：** 你把这个「保险箱」交给智能合约。合约把它挂在一棵巨大的「承诺树」（Merkle Tree）上。
    

> **此时的状态：** 链上有一堆保险箱，其中一个是你的，但没人知道具体哪一个是你的。

* * *

### 3\. 第二阶段：构造证明（带上面具）

_对应文档中的：「在本地构造 ZK 证明」_

当你要投票时，你不能直接用原来的钱包地址去喊“我投赞成”，因为那样就暴露了。你需要在此刻生成一个**零知识证明**。

你的浏览器会在本地悄悄做以下数学运算（生成 Proof）：

1.  **证明我在树上：** 你的浏览器拿出你的「秘密」，通过数学路径证明：_“看，这棵树上的第 X 个保险箱的钥匙在我手里。”_ —— 但**不透露**你打开的是第几个箱子。
    
2.  **生成防重标（Nullifier）：** 这是最天才的一步。系统用你的「秘密」和「这次选举的ID」算出一个唯一的**代号（Nullifier）**。
    
    -   只要你的秘密不变，选举ID不变，算出来的代号永远一样。
        
    -   如果你想投第二次，代号会重复，会被抓包。
        
    -   但是，别人无法通过这个代号反推出你的秘密。
        

* * *

### 4\. 第三阶段：提交与验证（投出选票）

_对应文档中的：「提交投票交易」与「链上验证」_

现在，你通过一个新的钱包地址（或者通过中继器）向合约发送交易。这笔交易包含三个东西：

1.  **Proof（证明）：** “我发誓我有资格，且没撒谎，数学可以作证。”
    
2.  **Nullifier（代号）：** “这是我这次投票的唯一流水号。”
    
3.  **Vote（选项）：** “我投赞成。”
    

**智能合约怎么做？** 合约是那个铁面无私的守门员，它执行两步检查：

1.  **验证 Proof：** 运行验证算法。如果 Proof 是真的，说明你确实在名单树上。
    
2.  **检查 Nullifier：** 查一下这个“代号”之前有没有出现过？
    
    -   如果没出现过 -> **计票有效**，并把这个代号记在黑名单上。
        
    -   如果出现过 -> **报错**，你已经投过票了。
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->

今天学习到了最重要的是如何做好投研？  
一个好的投研需要包含：技术背景、团队背景、代币经济学、宏观政策和叙事  
  
如何从有效的输入开始：  
\- 项目的白皮书  
\- 由点到面的去学习，从一个项目拆解出发、逐步拓展到其他项目，最终丰富自己的知识  
  
  
**一、 投研的核心框架 (Research Framework)**

演讲者将投研主要定义为针对**背后有项目主体（实体）的代币**（即“山寨币”）的研究，而非比特币（主流币）或Meme币（无逻辑）,。一个完整的投研框架应包含以下三个核心维度：

1\. 基本面与底层技术 (Fundamentals & Technology)

这是投研的基石，要求研究者必须进行大量的学习，理解项目的技术逻辑，而不是只看表面。

• **技术原理**：必须搞懂项目是做什么的以及它是如何实现的。例如，研究Layer 2时，需要理解什么是“乐观挑战（Optimistic Rollup）”，它与“零知识证明（ZK-Rollup）”的区别是什么，以及非对称加密等底层概念,。

• **解决的问题**：理解项目的核心价值主张。

2\. 团队与背景 (Team & Background)

对项目背后的“人”和“钱”进行尽职调查。

• **团队履历**：创始团队的背景是什么？毕业于哪所大学？之前做过什么项目？是否有过不良记录（“割韭菜”）？

• **投资机构**：背后的投资机构（VC）是谁？如果有一线知名机构背书，通常是一个加分项。

3\. 代币经济模型 (Tokenomics) —— **核心筛选器**

演讲者强调，代币经济模型是最重要的过滤工具，可以以此筛掉市场上90%的不靠谱项目。

• **流通量与总量**：重点关注当前的流通量占总量的比例。

    ◦ **风险案例**：如果一个项目流通量仅占10%，且每月大量解锁（例如每月解锁10%），这意味着资金池必须每月增加相应比例的资金才能维持币价不跌。此类项目通常不值得投资,。

• **解锁机制**：必须警惕代币的释放时间表，巨大的抛压会导致价格崩盘。

4\. 宏观流动性与叙事 (Macro Liquidity & Narrative)

除了项目本身，必须判断市场环境。投研在“流动性充裕”且有“新叙事”时才最有效。

• **流动性判断**：理解经济周期和美联储动作（放水）。当流动性不足时（如当前阶段），山寨币市场缺乏资金溢出，投研效果会大打折扣,,。

• **叙事创新**：寻找新的技术或模式创新（如DeFi的出现、铭文的比特币脚本创新、图币互换ERC404等）。只有新叙事出现，才可能带动新的造富效应,。
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->


最近这段时间，不管是运营端还是技术端的深挖，体感上收获都挺大的。复盘了一下，大概分为这两个板块：

1\. 运营实战：

如何组织一场高质量的 AMA： 不只是拉个群开语音那么简单。从前期的嘉宾邀约、核心议题拆解，到预热文案的铺设，再到嘉宾组织和活动后复盘，这一套流程跑下来，我才明白“逻辑闭环”对一场活动有多重要。

玩转 Telegram 社区： 电报群运营水很深，重点在于自动化工具（Bot）的配置和社区氛围的破冰。如何在高频刷屏的信息流里保持核心内容的曝光，同时过滤掉那些烦人的垃圾广告，这块儿我现在算是有了一些心得。

Figma 海报速成：复制了Ethpanda的figma的海报。现在从网格布局、字体层级到色彩规范，已经能独立搞定一张审美在线的活动海报了。这种“设计不求人”的感觉确实爽。

2\. 技术钻研：DApp 架构与安全防线

技术这块儿，最近最大的触动来自 Wachi 老师：

DApp 框架的深度梳理： 听完 Wachi 老师对 DApp 框架的拆解，我最深刻的感受是：好的架构不是代码的堆砌，而是逻辑的解耦。他把前端交互、合约调用和后端索引的协作逻辑讲得非常透彻，把我之前很多模糊的认知点全给连成线了。

区块链安全漏洞初探： 顺着这个架构，我也自己上手梳理了一下目前链上最基础、也最致命的那些安全漏洞（像重入攻击、整数溢出、权限校验失效这些）。

下一步计划： 搞技术不能光看不练。我打算后续围绕区块链安全这个垂直方向，出几篇深度长文或者技术博客，把这些漏洞案例讲透。一方面是给自己做知识内化，另一方面也希望能够帮到大家。
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->



主要学习了代理模式，围绕上次的parity多签钱包的访问控制漏洞，具体学习了对应的dialing模式

## 主流的代理模式架构

目前 Web3 开发中，主要有三种流行的代理架构：

### A. 透明代理（Transparent Proxy Pattern）

这是早期 OpenZeppelin 推荐的标准模式。

-   **原理：** 代理合约通过判断 `msg.sender` 来分流请求。如果调用者是管理员（Admin），就执行升级操作；如果是普通用户，就 `delegatecall` 给逻辑合约。
    
-   **优点：** 彻底杜绝了“函数选择器冲突”（即用户不小心调到了管理员升级函数的风险）。
    
-   **缺点：** 每次调用都需要判断 `msg.sender`，**Gas 消耗较高**。
    

### B. 通用可升级代理（UUPS - EIP-1822）

目前**最推荐、最主流**的模式（现在的 OpenZeppelin 默认采用 UUPS）。

-   **原理：** 代理合约被极度精简，**升级逻辑被移到了“逻辑合约”内部**。代理只负责转发。
    
-   **优点：** 代理合约非常轻量，用户每次交互的 **Gas 费用更低**。
    
-   **风险（类似 Parity 攻击点）：** 如果新升级的逻辑合约里**忘了写升级函数**，那么这个合约就永远锁死了，无法再升级。因此，UUPS 要求逻辑合约必须继承特定的可升级接口。
    

### C. 信标代理（Beacon Proxy）

适用于需要同时管理成百上千个相同逻辑合约的场景（例如一个平台为每个用户生成一个独立的金库）。

-   **原理：** 用户不直接存储逻辑合约地址，而是指向一个“信标（Beacon）”合约。信标合约里存着逻辑合约地址。
    
-   **优点：** 想要同时升级所有用户的金库时，只需要更新一次信标合约里的地址，所有代理就会自动读取新代码。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->




一次性将之前创作的链上安全的文章都更新上去了

今天Secret同学的分享对我来说很有感悟：

如果做一个工作流：结合X的搜索、tg特定群组以及招聘网站，做一个实时抓取的岗位分析应该可行

今天听了各个同学的分享会，给我最大的感悟是：web3不仅是defi。持续地投入一些大家可能不太那么看得到的基础设施，也挺好的
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->





今天纠结的点在于，到底是尽量多学Ethpanda上面的内容，还是专精于一个方向

  
主要学习了基于Dune的链上数据分析：

包括Dune的基本使用、如何使用Dune完成一次简单的链上分析、以及复现历史的真实攻击事件等等。

比如追踪某个黑客攻击的代币流向。

感谢伟大的AI！

除此之外，就是和Web2有关的内容，今天是忙绿的一天。

```
SELECT 
    contract_address, -- 代币合约地址（如 USDT 的地址）
    value,            -- 原始金额
    evt_tx_hash,      -- 交易哈希
    block_time
FROM erc20_ethereum.evt_Transfer
WHERE "to" = 0xHackerAddress... -- 替换为实际黑客地址
ORDER BY block_time DESC
```

明天一定要出一个对应的Dune的文章
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->






# 学习运营相关知识

Telegram 如何运营？  
包括：

1.  创建群组
    
2.  管理群组
    
3.  bot设置
    
4.  话题设置
    

Figma 如何使用？

-   基本操作
    
-   完成 Ethpanda 的Figma实战作业
    

# 技术向

重新复习了一下Solidity的基本知识  
完成 Web3 实习计划《智能合约开发》的阅读
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->







# 阅读web3实习计划《智能合约开发》

整体架构：前端、智能合约、去中心化存储、数据检索器

前端：  
完全的链下架构，负责于智能合约、RPC等进行交互，常见的框架包括viem、web3.js等

智能合约开发框架：  
包括boundary和hardhat，一个是rust编写的一个是js编写的，个人倾向于使用hardhat

去中心化存储：  
IPFS：内容寻址的分布式文件系统，常配合 Filecoin 实现持久化存储；  
Arweave：提供“永久存储”服务，一次付费永久保存；

数据检索器：  
也就是event事件，我觉得实习日记上写得就很好，相当于智能合约没有办法去查询这个地址下有多少nft，必须得通过数据检索，去记录每次event事件，梳理出应该会有多少个NFT

基本的solidity数据结构：

基本数据类型：  
值类型：uint, int, bool, address, bytes, string 等；  
引用类型：array, struct, mapping。

函数定义：  
可见性：public, external, internal, private；  
状态可变性：view, pure, nonpayable, payable；  
修饰器（Modifiers）：用于复用校验逻辑（如 onlyOwner）。

面向对象特性：  
继承（Inheritance）：支持多继承，使用 is 关键字；  
接口（Interface）与抽象合约（Abstract Contract）：用于解耦和标准化；  
库（Library）：部署一次，多个合约复用（如 SafeMath）。

事件（Events）：  
使用 event 关键字定义；  
通过 emit 触发；  
是链下索引和监听的核心机制。

# 运营相关

参考：[活动前期策划](https://docs.google.com/presentation/d/19Ga8uaswI1ntTR_JrdPyVzc9kUyPebgbsFbQJa1x2-0/edit?usp=sharing)

如何组织一场Twitter space：  
标准SOP：

1.  确定基本信息
    
    1.  主题
        
    2.  问题库
        
    3.  时间
        
    4.  主持人
        
    5.  拟邀嘉宾
        
2.  邀请嘉宾
    
    1.  标准文案
        
        1.  主题、时间、话题内容
            
        2.  xx 老师好！我们计划在 \[时间\] 举办关于 \[主题\] 的 Twitter Space，想邀请你参加～  
            涉及的话题有：1...2...3...
            
    2.  收集嘉宾信息
        
        1.  name、twitter handle、title、headshot
            
3.  宣发
    
    1.  包括宣发文案、海报、链接
        
    2.  宣发渠道
        
        1.  tg、x、等社媒社群
            
    3.  提前3天预热
        
    4.  当天需要第二波预热
        
4.  准备
    
    1.  提前3天创建群聊、包含嘉宾和工作人员
        
    2.  提前1天同步对应的问题和链接
        
5.  上线
    
    1.  活动前5分钟，再次确认space 链接
        
    2.  开场
        
        1.  活动主题介绍
            
        2.  嘉宾介绍
            
        3.  嘉宾问答
            
        4.  观众提问
            
6.  复盘
    
    1.  实时在线人数
        
    2.  宣发推文数据
        
    3.  节奏情况等
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->








# 从 ERC-721 到 ERC-7962

这是我让大模型解析文档得到的：

### **1\. 核心概念与数据结构**

ERC-7962 的实现核心在于将代币的所有权绑定到公钥的哈希值（`keyHash`），而不是传统的以太坊地址。

-   **Key (公钥)**: 用户需持有 65 字节的未压缩 secp256k1 公钥（以 `0x04` 开头）。在交易时，公钥作为参数 `key` 传入以证明所有权。
    
-   **Key Hash (密钥哈希)**: 合约中存储的所有者标识是 `keyHash`，即 `keccak256(key)`。这使得链上不直接存储用户的地址。
    
-   **状态映射 (Mappings)**:
    
    -   **ERC-KeyHash721**: 维护 `tokenId` 到 `keyHash` 的映射。
        
    -   **ERC-KeyHash20**: 维护 `keyHash` 到 `balance`（余额）的映射。
        
    -   **Nonce**: 必须维护 `mapping(bytes32 => uint256) _keyNonces`，记录每个 `keyHash` 的 nonce 值以防止重放攻击。
        

### **2\. 签名验证机制 (Signature Verification)**

这是 ERC-7962 实现中最关键的步骤。由于所有权与 `keyHash` 绑定，但交易是由以太坊地址签名的，验证过程如下：

3.  **验证 KeyHash**: 检查传入的明文公钥 `key` 的哈希值 `keccak256(key)` 是否等于当前代币所有者的 `keyHash`。
    
4.  **构建 EIP-712 消息**: 根据 EIP-712 标准构建结构化数据哈希（digest）。
    
5.  **恢复签名者**: 使用 `ecrecover(digest, signature)` 恢复签名者的以太坊地址。
    
6.  **匹配地址**: 验证恢复出的签名者地址是否由传入的公钥 `key` 派生而来。具体算法是取公钥去除前缀 `0x04` 后的部分进行 keccak256 哈希，取最后 20 字节。
    
7.  **检查过期时间**: 验证 `block.timestamp` 是否小于等于 `deadline`。
    

### **8\. 具体接口实现逻辑**

**A. ERC-KeyHash721 (NFT) 的** `transfer` **实现**

该函数用于转移 NFT 所有权。

-   **输入参数**: `tokenId`, `toKeyHash` (新所有者), `key` (当前所有者公钥), `signature`, `deadline`。
    
-   **执行流程**:
    
    1.  验证 `fromKeyHash`（当前所有者）非零且代币存在。
        
    2.  执行上述的“签名验证机制”，确保操作者拥有对应私钥。
        
    3.  验证 `toKeyHash` 非零。
        
    4.  更新状态：将 `tokenId` 的所有者修改为 `toKeyHash`。
        
    5.  增加 `fromKeyHash` 的 nonce 值。
        
    6.  触发 `KeyHashTransfer721` 事件。
        

**B. ERC-KeyHash20 (同质化代币) 的** `transfer` **实现**

该实现采用了类似比特币 UTXO 的模型来处理部分转账，以增强隐私。

-   **输入参数**: `fromKeyHash`, `toKeyHash`, `amount`, `key`, `signature`, `deadline`, `leftKeyHash` (剩余资金接收地址)。
    
-   **执行流程**:
    
    1.  检查 `fromKeyHash` 余额是否充足。
        
    2.  执行“签名验证机制”。
        
    3.  **严格模式检查**: 强制要求 `leftKeyHash`（找零地址）**不等于** `toKeyHash` 且 **不等于** `fromKeyHash`。这是为了强制密钥轮换，切断资金链路。
        
    4.  更新状态：
        
        -   `balanceOf[fromKeyHash]` 清零（或扣除全额）。
            
        -   `balanceOf[toKeyHash]` 增加 `amount`。
            
        -   `balanceOf[leftKeyHash]` 增加剩余金额 (`original_balance - amount`)。
            
    5.  增加 `fromKeyHash` 的 nonce 值。
        
    6.  触发 `KeyHashTransfer20` 事件。
        

### **7\. 安全性与隐私设计**

-   **Gas 费分离**: 由于交易通过签名授权，任何地址（Relayer）都可以提交交易并支付 Gas 费，实现了所有权与 Gas 支付的分离。
    
-   **防重放**: 实现必须使用针对每个 `keyHash` 的 nonce 机制，类似于 EIP-2612。
    
-   **防篡改**: 必须使用 EIP-712 结构化数据签名，包含所有关键参数（如 `leftKeyHash`, `amount`, `tokenId` 等）。
    
-   **隐私限制**: 虽然链上存储的是哈希，但在交易发生时 `key` 会在 calldata 中公开。因此，建议用户在每次接收代币（`toKeyHash`）或找零（`leftKeyHash`）时使用新的密钥对，以最大限度减少可链接性。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->









# LXDAO周会

1 阅读了一遍web3黑话  
[WTFAcademy/WTF-gm: Web3黑话汇总（黑宝书）](https://github.com/WTFAcademy/WTF-gm)

2.  了解到了LXDAO注意做的事情，下周打算参与LXDAO
    

公平物品的可持续这个课题很好，我个人理解的是如何保证公告物品的可持续除了需要本身开源精神以外，还需要通过一定的激励

## 招聘平台分析

后续打算从产品的角度，详细地区做一个竞品分析，包括从求职者的角度，从企业主的角度，梳理对应的功能点等。+加入todolist

### [Web3.Career](http://Web3.Career)

[Web3.career](http://Web3.career)  
**产品模式**  
纯粹的流量分发平台、页面看起来像传统的web2，主要提供详细的薪资、JD要求、公司要求数据，他的产品核心不是功能点，而是在于他的大而全和SEO。

![Pasted image 20260117114704.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-17-1768661472760-Pasted_image_20260117114704.png)

**Web2 vs Web3**：  
更偏于传统Web2.5

**流量入口**  
SEO  
基本上，去搜索：“Web3 jobs”、几乎都在第一名

![Pasted image 20260117115114.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-17-1768661484864-Pasted_image_20260117115114.png)

**盈利模式**  
流量变现  
包括：

1.  **企业付费发帖**：标准的流量生意。因为我流量大，所以你必须来我这发。
    
2.  **置顶/加急费**：类似传统工作网站的竞价排名逻辑。
    

![Pasted image 20260117115410.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-17-1768661501191-Pasted_image_20260117115410.png)

### Dejob

社群服务型  
[Dejob](https://www.qianwen.com/chat/a7346cbad2ef4d12963553c83683231a)([http://t.me/DeJob\_official\_group](http://t.me/DeJob_official_group))

**产品模式 (Mode)**  
“私域流量+猎头服务”混合体。

它的描述：“人才库、链上工作台、DID”。这说明它的愿景是做 Web3 的 Link3 或 LinkedIn，但现状（看它依赖 TG 和表单）其实更像是一个数字化猎头/中介。它通过人工或半自动化的方式在撮合。

**Web2 vs Web3**  
运营上很 Web3，产品上目前偏 Web2。  
Web3 特征：重度依赖 Telegram (TG) 频道。在官网上可连接个人钱包。  
Web2 特征：还在用“填表单”这种传统方式收集需求

![Pasted image 20260117115802.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-17-1768661519323-Pasted_image_20260117115802.png)

**流量入口**  
Telegram 私域流量 + 合作伙伴互推  
它不像 [Web3.career](http://Web3.career) 靠 Google，它靠的是在各个币圈群里的活跃度、以及和项目方的合作（BD驱动）

**盈利方式**  
未在官网上找到提供企业认证发布岗位需要付费，倾向于是在tg上接受投放广告收费

### Cryptojoblist

[Cryptocurrency Jobs and Blockchain Jobs (75 new in Jan 2026)](https://cryptojobslist.com/)

**产品模式**

1.  也是垂直的招聘板
    
2.  提供企业直接搜索对应的人才
    

![Pasted image 20260117120604.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-17-1768661536489-Pasted_image_20260117120604.png)

它是这个圈子的“老字号”，有一定的社区信任度。它不仅仅发职位，还做很多采访、公司介绍，试图建立“雇主品牌”。

**Web2 vs Web3**  
Web 2

**流量入口**  
品牌直接访问 + Newsletter

**盈利模式**  
阶梯式付费  
付费发布广告帖子

![Pasted image 20260117120229.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-17-1768661549264-Pasted_image_20260117120229.png)

### AbetterWeb3

用爱发电型/社区共建  
[abetterweb3](https://abetterweb3.notion.site/)

**产品模式**  
信息聚合。  
用 Notion 做网站，说明它极低成本，且极度透明。它更像是一个“避坑指南”+“职位汇总”。它的核心价值是真实。

**Web2 vs Web3**  
精神上 Web3 (开源、去中心化)  
技术上 依赖 Web2 (使用Notion)

**流量入口**  
Twitter (X) + 社群、社媒推荐。

**盈利模式**  
目前可能是非盈利

### CryptocurrencyJobs

[Cryptocurrency Jobs and Blockchain Jobs - Cryptocurrency Jobs](https://cryptocurrencyjobs.co/)

**产品模式**  
精选型招聘板

**Web2 vs Web3**  
Web 2

**流量入口**  
SEO + 邮件订阅

**盈利模式**  
一次性付费 ($299)

![Pasted image 20260117121331.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/xxxmingyue/images/2026-01-17-1768661560725-Pasted_image_20260117121331.png)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->










# **学习《安全和合规》部分**

于自己而言，最重要的几点：

1.  不参与任何向国内提供服务的项目
    
2.  国家不承认虚拟货币的资金属性，所以不管是虚拟货币作为工资支付，还是用USDT参与交易，及时遭遇骗局，也很可能不受法律支持
    
3.  在参与法定货币与虚拟货币交换时，需要仔细核对交易对手方，避免参与到洗钱等其他恶意行为
    

安全上：  
不点击默认链接、转账确认对方地址、参加Unshipable课程  
不乱安装插件，真实的支付环境和测试的环境应该做好隔离

# **区块链安全学习路径&参考**

-   慢雾安全官网：[SlowMist Team](https://github.com/slowmist)
    
    -   可以了解到相关的攻击手法复现等
        
-   慢雾安全《Web3安全手册》
    

# **共学**

如何做一个自己的账号？

今天感触最大的就是学到了Echo老师说的，如何运营好自己的一个账号。  
老师有说，他在观看x的推特的时候，发现知识特别碎片化，他就做了一个工作，去整理，哪些是真正有效的信息，并且通过这样的方式获得了大量的关注。

这给了我一个启发，可以围绕链上分析、或者数据分析。

给大家带来全面的项目解析，和拆解，帮助大家能够快速的了解一个项目他的生态怎么样？等等

# **周会分享**

学到了最重要的一点，目标导向，设置超出自己能力120%的目标，能够到，但是需要稍微努力，感觉能极大的激发自己的兴趣

在这种情况下，本周计划：

-   完成一次区块链攻击的原理和链上追踪分享
    
-   完成一个Dune的技术博客
    

参考karen的:[X 上的 星星狗Karen：“在X上，如何站着把号起了？” / X](https://x.com/karenyuan03/status/2012000821555122331?s=20)

1.  确定你要写什么样的选题？
    
2.  搜集10位在这个选题方向知名的KOL
    
3.  分析他们的标题、内容、发文时间端、形式
    
4.  个人觉得前期最重要的不是流量，而是稳定，稳定的输出
    
5.  尽量不使用AI，锻炼自己的思考能力
<!-- DAILY_CHECKIN_2026-01-16_END -->

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
