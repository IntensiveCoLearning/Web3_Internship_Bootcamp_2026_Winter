---
timezone: UTC+8
---

# crabread1004

**GitHub ID:** crabread1004

**Telegram:** 

## Self-introduction

学习中...0v0

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
学习智能合约部分时，和ai对话的疑问以及重点总结如下：

1\. 架构与省钱技巧 (Structure & Gas)

-   **可见性是省钱的关键**：
    
    -   对外接口优先用 external（参数直接读 calldata，省 Gas）。
        
    -   只有当合约内部也需要调用该函数时，才用 public。
        
-   **状态变量很贵**：
    
    -   能用 constant (常量) 或 immutable (不可变量) 就别用普通变量，因为它们不占 Storage 槽位，巨省 Gas。
        
    -   不需要链上读取的数据（如历史记录），用 event (事件) 抛出，前端能看到，但链上存储成本极低。
        
-   **构造函数 (Constructor)**：
    
    -   这是初始化的唯一机会，用于设置 owner 和 immutable 变量。
        

2\. 函数逻辑 (Logic)

-   **收款必须声明**：
    
    -   如果函数需要接收 ETH，**必须**加上 payable 关键词，否则交易会直接失败。
        
-   **读写分明**：
    
    -   view：只读，不改状态（链下调用免费）。
        
    -   pure：纯计算，不读也不改状态（链下调用免费）。
        
    -   _注意：如果合约内部调用 view/pure 函数，还是要耗 Gas 的。_
        
-   **修饰符 (Modifier)**：
    
    -   \_; 是一切的核心，它代表“原函数体代码插入的位置”。通常放在修饰符逻辑的最后。
        

3\. 安全防线 (Security - 💀生死攸关)

这是最容易导致项目归零的部分，请刻在脑子里：

-   **🛡️ 防重入 (Anti-Reentrancy) —— 黄金法则 CEI**
    
    -   **C (Checks)**：先检查条件 (require)。
        
    -   **E (Effects)**：再修改状态 (扣减余额、更新标记)。
        
    -   **I (Interactions)**：最后才进行交互 (转账 call、调用外部合约)。
        
    -   _口诀：先扣款，再转账！_
        
-   **🔐 访问控制 (Access Control)**
    
    -   **默认是公开的**：任何 public/external 函数如果没有加修饰符，任何人都能调用。
        
    -   **必加锁**：涉及提款、铸造 (mint)、暂停合约等敏感操作，必须加 onlyOwner 或权限验证。
        
-   **🔢 整数溢出 (Overflow)**
    
    -   **版本分水岭**：0.8.0。
        
    -   **\>= 0.8.0**：编译器自动检查，溢出会报错，安全。
        
    -   **< 0.8.0**：必须使用 SafeMath 库，否则 255 + 1 会变成 0。
        

* * *

### 自查清单

在提交代码前，请按此清单检查：

1.  **\[Gas\]** 只有外部调用的函数，写成 external 了吗？
    
2.  **\[Gas\]** 那些部署后不再修改的数值，加 constant 或 immutable 了吗？
    
3.  **\[安全\]** 所有转账 (transfer/call) 操作，是不是都放在了**修改余额之后**？(CEI模式)
    
4.  **\[安全\]** withdraw (提款) 函数是不是每个人都能调？有没有加 onlyOwner？
    
5.  **\[版本\]** pragma 版本是不是 ^0.8.0 以上？
    
6.  **\[逻辑\]** 接收 ETH 的函数加 payable 了吗？
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

notion共学笔记：[https://www.notion.so/1-23-2f1408fac1a68072b437de593158822b?source=copy\_link](https://www.notion.so/1-23-2f1408fac1a68072b437de593158822b?source=copy_link)

完成gas优化任务时和Gemini对话过程重点总结：

### 1\. 核心原则

-   **Storage (存储) 是最贵的**：尽量减少对链上状态变量的读写。
    
-   **Memory (内存) 是便宜的**：尽量在内存中处理完逻辑，最后一次性写入 Storage。
    
-   **Calldata 是最省的**：对于外部调用，参数尽量放在 calldata 中。
    

### 2\. 优化技巧清单

### A. 存储优化

-   **读操作 (SLOAD)**：不要在循环中反复读取 Storage 变量。
    
    -   _做法_：在函数开始时将 Storage 变量赋值给 Memory 变量（缓存）。
        
-   **写操作 (SSTORE)**：这是 Gas 杀手。
    
    -   _做法_：**绝对避免在循环中修改 Storage 变量**。在循环中计算中间结果（存在 Memory 变量中），循环结束后再更新 Storage。
        

### B. 数据结构

-   **原理**：EVM 的 Slot 为 32 字节 (256 bit)。
    
-   **做法**：
    
    -   使用更小的类型（如 uint128, uint64）代替 uint256。
        
    -   将这些小变量**相邻排列**，使其总和 <= 256 bit。
        
    -   _示例_：两个 uint128 挨在一起只占 1 个 Slot；如果中间夹了一个 uint256，就会占 3 个 Slot。
        

### C. 循环黑科技

1.  **缓存 Length**：
    
    -   错误：for (uint i=0; i < arr.length; i++) (每次都读长度)
        
    -   正确：uint len = arr.length; for (uint i=0; i < len; ++i)
        
2.  **前置递增 (++i)**：
    
    -   ++i 比 i++ 少 5 gas（不需要返回旧值）。
        
3.  **Unchecked (Solidity 0.8+)**：
    
    -   循环变量 i 通常不会溢出（因为受限于数组长度）。
        
    -   使用 unchecked { ++i; } 包裹递增操作，省去溢出检查的 Gas。
        

### D. 函数属性

-   **External vs Public**：
    
    -   如果函数只被外部调用（UI或合约），**必须用 external**。
        
    -   external 函数直接从 calldata 读取参数（无法修改，便宜）。
        
    -   public 函数会将参数复制到 memory（贵，尤其是大数组）。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


# 1.22共学笔记

notion链接和笔记更新：[https://www.notion.so/1-22-2f0408fac1a68098898bde133667c47a?source=copy\_link](https://www.notion.so/1-22-2f0408fac1a68098898bde133667c47a?source=copy_link)

1.完成remix教程课后任务

\-应该怎样看到retrive后的结果，在个人钱包里面吗

2.小海老师分享会

3.智能合约开发

4.南塘dao分享会

\-群友笔记整理

-   罗伯特议事规则，全名是罗伯特议事规则（Robert’s Rules of Order），是一套用于会议与集体决策的“标准流程规则”。
    
-   罗伯特议事规则 = 一群人不吵架、有效开会、按规则做决定的方法说明书
    
-   它解决什么问题？ 当一个组织里有很多人时，常见问题是： 谁先说？ 什么时候该投票？ 少数服从多数，但如何保护少数？ 情绪化争吵 vs 理性决策怎么区分？
    
-   罗伯特议事规则就是为“多人协商 + 民主决策”设计的操作系统
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



图片较多，notion链接如下

[https://www.notion.so/1-21-2ea408fac1a68010a19efcb11f69a02f?source=copy\_link](https://www.notion.so/1-21-2ea408fac1a68010a19efcb11f69a02f?source=copy_link)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->




1.观看并总结分享会

2.继续研究remix中...
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->





熟悉remix中！......
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->






今日任务：

1.021 学习以太坊第 3 章

2.油管视频D4D5（感谢群友翻译！
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->







今天有两场考试，休息一天~
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->








# 1.14共学笔记

非学分任务：

1.  Web3 实习手册[「安全与合规」](https://web3intern.xyz/zh/security/)部分
    
2.  021 学习以太坊第 2&3 章
    
3.  拓展 & 辅助理解材料
    
    1.  [**Day 3: ENS, DEX, Identity, Inventory, Sybil**](https://www.youtube.com/watch?v=wYSMNdIRoII&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3)
        
    2.  [**Day 4: NFTs!!! ERC20 vs ERC721, IPFS, Metadata**](https://www.youtube.com/watch?v=NOdrEpnoCiM&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=4)
        
    3.  [**Day 5: Stuck Transactions, Gas Limits, Multisigs, L2s, Lending...**](https://www.youtube.com/watch?v=11QTT6BK5j0&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=5)
        

学分任务：

1.分享会：合规与安全
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->









# 1.13共学笔记

1.12补充：

web3行业赛道：

1.DeFi（去中心化金融）

2.NFT（数字资产）

3.DAO（去中心化自治组织）

[4.MEME](http://4.MEME) 币（社区自治与实用）

非学分任务：

1.  Ethereum 官网 [Overview](https://ethereum.org/learn/) 基础内容学习
    
    1.  与许多传统货币不同，**以太币可能会随时间推移变得越来越稀缺**。每次有人使用以太坊时，都会销毁一小部分以太币，使其永久退出流通。在交易繁忙的日子里，被销毁的以太币数量会超过新铸造的数量，使得以太币具有通缩特性，其价值会随时间推移而增长。以太坊的使用量越大，被销毁的以太币就越多。
        
    2.  比特币通过矿工来保障网络安全。这些高性能计算机相互竞争，致力于破解复杂的数学难题，胜出者有权将下一笔交易区块添加至区块链，并获得比特币作为奖励。——以太坊：诚实的验证者可获得以太币奖励，而任何不诚实的验证者则会损失部分质押。
        
2.  [**Day 1: A Developer’s Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro
    
3.  [**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs、**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)
    
4.  分享会自用笔记：[https://www.notion.so/1-13-web3-2e7408fac1a680f3ba9ff5c27a86a3a6?source=copy\_link](https://www.notion.so/1-13-web3-2e7408fac1a680f3ba9ff5c27a86a3a6?source=copy_link)
    
5.  **crptoria - Ep.141 2026展望上 - 加密货币的本质：货币控制权从国家回归个人**
    

学分任务：

1.  在 My First NFT mint 第一个 NFT
    
    1.1 测试币不足解决方案：
    
    挖矿： [https://sepolia-faucet.pk910.de/#/](https://sepolia-faucet.pk910.de/#/)
    
    钱包使用和领水网站：[如何完成“创建钱包，并尝试测试币转账”？](https://www.notion.so/2e54867ff3e180b5bc84e33a7e2f9835?pvs=21)（感谢群友！）
    
    1.2 Gas Fee查看：[https://sepolia.etherscan.io/gastracker](https://sepolia.etherscan.io/gastracker)
    
2.  完成 [Unphishable](https://unphishable.io/) 钓鱼攻防挑战2章
    
    2.1 一些工具
    
    你可以用 [https://openchain.xyz/signatures](https://openchain.xyz/signatures) 来解码函数选择器
    
    你可以用 [https://calldata.swiss-knife.xyz/decoder](https://calldata.swiss-knife.xyz/decoder) 解码完整的通话数据
    
    2.2码一些自己难以识别的
    
    **Seaport Zero Order NFT 钓鱼**
    
    **地址前缀/后缀钓鱼**
    
    **Uniswap V3 多呼叫钓鱼**
    
    **安全代理合同攻击**
    
    **谷歌伪造钓鱼（注意邮件标题！）**
    
3.  确立岗位目标意向，提供四周学习规划
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->













1.  学习内容：
    
    1.  Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分——已总结至notion
        
    2.  [021 学习以太坊第 1 章](https://github.com/XiaoHai67890/021Ethereum/blob/main/%E3%80%8A021%E5%AD%A6%E4%B9%A0%E4%BB%A5%E5%A4%AA%E5%9D%8A%E3%80%8B%E5%BC%80%E6%BA%90%E6%95%99%E6%9D%90.pdf)
        
2.  拓展 & 辅助理解材料——layer2拓展（英文阅读速度有些慢）
    
3.  工具安装——完成了除tg注册外工具安装学习
    

**4.分享会：行业介绍&全局概览**

含完整图片链接：[https://www.notion.so/2e6408fac1a680ddb6d8f898643c25da?source=copy\_link](https://www.notion.so/2e6408fac1a680ddb6d8f898643c25da?source=copy_link)

1.历史回溯：（09-14）BTC1.0——（15-20）以太坊上主网，币安逐步合规——（20-24）概念爆发——

2.就业情况：

-   重点增长区域：北美——技术，中东（迪拜）/香港/东南亚（新加坡）——交易所
    

![image.png](attachment:7a3efc21-052b-4b4d-83cf-51e626443726:image.png)

-   求职渠道
    

![image.png](attachment:4cbe132d-5d18-44b8-b975-dcbb3ff44b3d:image.png)

3.职业与薪资

-   用人特征
    

![image.png](attachment:65458b36-f6e5-47a7-acbd-5c222e19a690:image.png)

-   职位比例
    

![image.png](attachment:2862fb6f-3add-4158-bd9d-460cd506153a:image.png)

-   薪资分布
    

![image.png](attachment:4e00c153-9e46-4102-90e4-96962cfb21a4:image.png)

技术/合规：最底层

运营：方差较大，根据对团队贡献

4.实战指南

-   入门方向
    

![image.png](attachment:34111f47-7a56-4506-b5ea-e857726c46fd:image.png)

-   能力要求
    

![image.png](attachment:1841abe8-4f07-49e0-91da-ff42907b90d0:image.png)

-   求职建议
    

![image.png](attachment:c70d6569-0739-4f76-a95e-c77057e92c76:image.png)

短期内ai无法代替细节工作，对人要求更高须有更清晰的概念（需要ai做什么，ai能做什么，怎样使用ai）

Q&A

1.  web3合规岗：针对港新欧等；需学习相关法案具体情况&开放的牌照申请制度
    
2.  先进大厂是否有帮助：时效性；偏向在大厂已经有经验的情况；已对细分方向有了解，可直接进入；也可先在大厂积累经验
    
3.  安全合规审计相关岗位：大公司才会有较多岗位，但实习机会丰富，可关注官方渠道
    
4.  劳动合同保障：因项目和公司而异，大项目更合规，但也存在初期项目无法签订正式合同/发法币，需自己注意权益保障
    
5.  增长是一个概念，很多岗位都会涉及；BD带来点对点的增长；产品：学习有哪些好的增长机制eg.pdd发红包；运营：积累项目资源等
    
6.  规避项目风险：看官网信息，是否有投资方，是否匿名；看团队成员的背景风评；早期尽量去大项目更有保障
    
7.  房地产传统行业转web3
    
8.  新人进入投研和数据分析需要掌握的技能/行业对其画像/发展方向：分析能力，了解项目/机制/周期；能做深度的行业研究，可先尝试写报告：历史，现在发展程度etc；投研是每个人都要做的事；用人方偏向有经验，有个人池塘；投研虽重要但并不会招很多人，只会数分不会其他可能竞争力偏弱
    
9.  链上CV很重要！
    
10.  传统律所转型：可考虑靠相关证书，反洗钱etc
     
11.  如何展示成果：在社媒分享
     
12.  BD路径
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
