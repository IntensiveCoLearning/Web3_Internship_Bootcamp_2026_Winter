---
timezone: UTC+8
---

# Calciux

**GitHub ID:** Calciux

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
## 021学习以太坊 C2
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

## 分享会 web3安全

**CeFi成为主要的黑客攻击靶机**：管理层私钥被盗、热钱包私钥被盗是主要的原因，暴露了显著的风险。

CeFi: centralized finance

**DeFi漏洞越来越深入**：但值得注意的是Balancer、Yearn这些已经安全运营数年的智能合约不断被挖掘出新的漏洞，以及Move智能合约漏洞明显增多，体现出攻击者利用AI技术挖掘智能合约漏洞的能力正在不断增强。

**国家级黑客的“业绩”巅峰**：2025年，朝鲜黑客组织（如Lazarus Group）盗取的资金继续高速增长

• 社会工程：通过伪装成Web3或AI公司的招聘人员，在LinkedIn等平台进行数月的关系维护，最终通过“技术面试”环节诱导目标员工下载带有后门的测试代码。

• 精准猎杀：他们将精力集中在少数拥有巨额资金储备的机构或个人身上。在2025年的所有机构级侵害事件中，朝鲜黑客贡献了超76%的金额。

• 洗钱工业化：资金在被盗后迅速进入一个高度自动化的洗钱链条，通常可以在45天内完成从链上混币到非法平台法币结算的全过程。

**地址投毒（Address Poisoning）是 Web3 中一种高频且隐蔽的钓鱼攻击，攻击者伪造与你真实地址“极其相似”的假地址vanity address，并通过小额转账把它“污染”进你的交易记录，诱导你在下一次转账时误复制该假地址，从而把资产直接转给攻击者。**

## 高频攻击类型

-   Private Key Theft
    
    -   Social engineering
        
    -   Malware / Clipboard hijack
        
    -   phishing / approval phising
        
    -   side channel attack
        
    -   Supply Chain Attack
        
-   **“Rug token” 指的是与 rug pull相关的代币：通常是开发者为了实施诈骗而创建的代币，最终会通过撤走流动性或抛售代币让投资者血本无归。**
    

Liquidity Rug / Insider Dump / Malicious Contract / Honey pot

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Calciux/images/2026-01-14-1768391768690-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Calciux/images/2026-01-14-1768392147725-image.png)

# 分享会 合规

1.大陆的负面清单 ICO公开融资\\虚拟货币交易所/挖矿

2.中心化的法律 vs 去中心化理念

3.

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Calciux/images/2026-01-14-1768395370636-image.png)

4.

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Calciux/images/2026-01-14-1768395775996-image.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->





## 学习以太坊 1-2

### 以太坊

-   Tokens
    
    -   ERC-20 fungible token的统一标准,使所有代币在钱包、交易所、DeFi 协议中可互操作
        
    -   ERC-721 non-fungible token 每个token彼此不相同,是独立不可替代的实体,相互不等价ERC-1155 同时包含FT/NFT/半同质化, 通过不同的标识来区分不同token的性质
        
-   Decun 升级
    
    -   Proto-Danksharding
        
    -   引入 blob: 临时的专门给Rollup存放交易数据的区域 主链不会保存完整的内容而是爆粗哪一个commitment, 能够验证
        
-   完整的danksharding
    
    -   blob变成了多个data shards, 每个shards只保存一部分数据
        
    -   Data Availability Sampling(DAS) 允许节点不下载整个区块而可以验证数据的真实性
        
    -   DAS利用Reed-Solomon ensure code(是一种maxium distance seperable code), 只要其中的部分数据就能恢复全部数据, 而大量节点的同时抽样验证保证了数据的生产者几乎不可能隐藏数据, 从而确保了只需要下载少量数据就可以验证数据是否被真实发布.
        

# 分享会

原文转成文字

钱包、RPC、节点、网络：如何链接

关键关系：

\- 钱包通常不直连全网：它连一个 RPC（访问节点的 API）

\- RPC 背后是节点（或节点集群）

\- 所以：RPC 往往是“中心化入口风险点”

1.钱包通常不直连全网：它连一个 RPC（访问节点的 API）

\- 钱包是什么：

钱包其实就是一个应用（App / 浏览器插件 / 网页），负责帮你管理私钥、发起交易、查询余额等。

它本身不等于节点，也不等于“连接全网”的那个东西。

\- 为什么“通常不直连全网”：

理论上，一个钱包可以自己运行一个完整节点，直接和区块链网络通信。

但那太重：需要同步区块、存储大量数据、保持在线。

大多数普通用户的手机/电脑不会这么做。

\- 它连的是谁：一个 RPC：

钱包一般会配置一个“RPC 地址”（比如 [https://mainnet.infura.io/…、https://rpc.ankr.com/eth](https://mainnet.infura.io/...%E3%80%81https://rpc.ankr.com/eth) 等）。

这个 RPC 就是一个对外暴露的接口服务器，提供一组 API，比如：

\- eth\_getBalance：查余额

\- eth\_sendRawTransaction：广播交易

\- eth\_call：读合约状态

\- 钱包所有的“查链上数据 / 发交易”动作，本质上都是在 调用这个 RPC 提供的 API，而不是直接对整条链说话。

2.「RPC 背后是节点（或节点集群）」- RPC 本身不“存区块链”，它只是入口：

RPC 服务器只是对外暴露 HTTP/WebSocket 接口，它本身不是区块链，它要把请求转给真正在跑区块数据的节点。

\- 背后是一个或多个节点：

\- 有的服务商直接用一个全节点作为后台。

\- 更常见的是：节点集群，例如很多台以太坊节点做负载均衡、容错、地域分布等。

\- RPC 服务器负责：

\- 接收钱包的请求

\- 按 JSON-RPC 标准解析

\- 转发给后端节点

\- 收到节点返回后再返回给钱包

\- 从逻辑视角看：

\- 钱包看到的是一个“RPC URL”

\- RPC 实际上是一个门面（gateway）

\- 真正跟区块链网络打交道的是节点 / 节点集群

3.「所以：RPC 往往是“中心化入口风险点”」这句话是在点出一个安全与信任结构上的关键问题：- 入口是“单点”或“少数点”：

虽然区块链网络本身是去中心化的，但大部分用户的钱包，默认都使用少数几个大型 RPC 提供方（例如 Infura、Alchemy、各大公链官方 RPC 等）。

这意味着：

\- 你表面上在用“去中心化网络”

\- 实际你是通过一个中心化服务的入口去访问它

\- 为什么是风险点：

\- 审查风险：

RPC 可以选择：不转发某些交易 / 不返回某些数据（比如屏蔽某些地址、某些合约）。

\- 篡改或误导风险：

虽然篡改区块链数据很难，但在展示层面，RPC 可以返回“错误或不完整的信息”，例如：

\- 少给你一些历史交易

\- 返回延迟的数据

\- 隐私风险：

你的所有查询、交易广播，都会经过它，RPC 可以记录：

\- 你什么时候查

\- 查了什么地址

\- 发了什么交易

\- 可用性风险：

RPC 挂了（被攻击、被关停、区域性屏蔽），你这边钱包就“感觉链挂了”，但其实链本身还在正常运转。

\- “中心化入口”这四个字的含义：

\- 链本身是分布式的

\- 但你接入链的方式却高度集中（1–2 个 RPC 服务商）

\- 所以入口变成一个“结构性单点”，这就是“中心化入口风险点”

## Web3 关键特性

去中心化

\- 钱包创建：高度去中心化（人人可生成钥匙）

\- 交易广播：可能去中心化（RPC 入口、前端入口）

\- 网络运行：节点越分散越安全

\- 客户端越多样越稳（减少单点软件风险）

无许可

\- 任何人都可以读/写网络（写入需要 gas）

抗审查

\- 节点全球分布，入口可替换（钱包/前端/RPC 都可换）

开放开源

\- 客户端开源，交易记录公开可查询（透明可审计）

隐私（现状与演进）

\- 现状：公开账本 + 伪匿名（地址不等于身份）

\- 风险：可通过关联分析追踪行为图谱

\- 演进：隐私方案仍在发展（例如 ZK，隐私链/隐私层等）
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->









# Web3实习手册C1

### 去中心化?

1.  Permission model: Permissionless; No privileged participants; Replaceability
    
2.  [BFT](https://bitcoin.org/bitcoin.pdf): PoW 50%; PoS 33%
    

## 区块链的运行

-   **用户发起交易**：用户通过钱包应用发起转账、智能合约调用等操作
    
-   **交易广播**：交易信息被广播到整个网络中的各个节点
    
    -   gossip protocol: 验证交易是否有效,放入mempool; inventory 避免重复传播
        
-   **节点验证**：网络中的矿工节点验证交易的合法性（余额是否足够、签名是否正确等）
    
-   **打包成块**：通过共识机制（如工作量证明），矿工将验证过的交易打包成新的区块
    
-   共识:谁有权力写入下一个区块; 写入的内容是否被全网接受
    

PoW: 寻找满足条件的N. 安全性来源于target的阈值极小, 需要大量暴力搜索

```
f(D)>SHA256(SHA256(H-(T||TX||D||N)))
```

PoS: Stake 竞争被选中记账的概率, 质押越多概率越大(balance)

```
SHA256(SHA256(prevBlock, A, t))<=balance(A)*m
```

-   **链接上链**：新区块被添加到区块链上，更新全网的账本状态
    

**奖励发放**：成功打包区块的矿工获得代币奖励和交易手续费

# Jan12 分享会

RWA**现实世界中存在的、有价值的资产**，通过 **tokenization（代币化）** 的方式被映射到区块链上，形成可在链上流通、拆分、抵押、交易的数字化资产。

DePIN资源上链

Rust\\合规\\Research analyst AI usage & On-chain Reputation

DAO/开源社区/投研社区

**Solidity：**  
Web3 最广泛和通用的开发语言，易上手，建议新手要学习一下，理解智能合约的编程思维逻辑。

**趋势：**  
Rust 的应用越来越多，很多链和自创语言基于 Rust 开发。

**学习和关注 Web3 新语言：**  
Move、Cairo、Func 等。

**合约安全意识；系统设计；对 DeFi / L2 的理解**

ai工具: 智能合约审计\\自动化
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
