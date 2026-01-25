---
timezone: UTC+8
---

# Tianya

**GitHub ID:** biduone

**Telegram:** @Robomtaya

## Self-introduction

中年小伙，探索人生新旅程。萌新入社写过两年Java工作需要转了Web前端、后来数年又搞过App跨平台开发RN、Flutter，基于headlessCMS开发过单节点业务后台和写过工具脚本，算一个Node.js全栈吧，一直混迹toB的IM工具软件行业。\n
从2010年在校初识BTC->区块链技术破圈大🔥->NFT风靡，从远观逐渐靠近了解到Web3的路线。对链上的未来世界产生了浓厚的好奇感，对公开的不可篡改的真实世界充满希望⛓️，期待自己加入这场改变世界技术浪潮并贡献自己的力量。\n
个人喜欢自驾🚙游历山川河流🏔️、探访不同地域🏞️人民生活风情风貌，🦶徒步亲近大自然，特别是大西北。
从2010年在校初识BTC->区块链技术破圈大🔥->NFT风潮，从远观逐渐靠近了解到Web3的路线。对链上的未来世界产生的浓厚的好奇感，对公开的不可篡改的“历史”真实世界充满希望⛓️。
个人喜欢自驾🚙游历山川河流🏔️、探访不同地域🏞️人民生活风情风貌，🦶徒步亲近大自然，特别是大西北。
从2010年在校初识BTC->区块链技术破圈大🔥->NFT风潮，从远观逐渐靠近了解到Web3的路线。对链上的未来世界产生的浓厚的好奇感，对公开的不可篡改的“历史”真实世界充满希望。
个人生活中喜欢自驾旅游历山川河流、探访不同地域人民生活风情风貌，徒步亲近大自然，特别是大西北。

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
# Foundry

以命令行为主的Web3开发神器

## 下载安装

```
curl -L https://foundry.paradigm.xyz | bash
```

## 运行

```
foundryup
```

## 在foundry中进行检查

```
forge --version
```

## 启动服务

```
Anvil
```

## 以forge初始化项目

```
forge init my_web3_proj
```

## 部署合约-通过指定的rpc

```
forge create src/Counter.sol:Counter --rpc-url http://127.0.0.1:8545 --private-key<YOUR_PRIVATE_KEY> --broadcast
```
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->

# **Hardhat 3**

## 基础

-   **环境：**VScode+Node.js v22 + npm(pnpm)
    
-   **VSCode插件：**Solidity（语法显示）
    
-   **初始化项目**：npx hardhat --init
    
-   **运行测试：**npx hardhat test
    
-   **项目文件**
    
    -   hardhat.config.ts 主要配置文件，定义Solidity编译器版本、网络配置、插件和任务等
        
    -   contracts/目录 合约文件夹，也可以包含测试代码文件
        
    -   test/目录 包括TS和合约的测试代码文件，如.sol .t.sol
        
    -   ignition 包含Hardhat Ignition部署模块，描述如何部署您的合约
        
    -   scripts：包含自定义脚本，可自动化部分工作流程。脚本可完全访问Hardhat的运行时环境，并可使用插件、连接网络和部署合约。
        
-   **编译合约：** npx hardhat build
    

## 本地测试部署

-   **npx hardhat node** 以node.js启动一个本地8545端口的区块链服务
    
-   **编辑部署脚本：** scripts/deploy.js
    
-   **执行部署：** npx hardhat run scripts/deploy.js --network localhost
    
-   **服务控制台：** npx hardhat console --network [localhost](http://localhost)
    
-   **控制台交互：**以js进行交互如：await contract.getBalance()
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->


# **零知识证明**

## **区块链投票**

-   **公开透明**：所有投票交易记录在链上，任何人可以查询与复算；
    
-   **不可篡改**：区块一旦确认，历史记录在经济假设下难以回滚；
    
-   **自动执行**：智能合约按照预先写死的逻辑完成计票与结果发布。
    

## **基础**

### **Prover / Verifier 模型**

零知识证明通常用两个角色来描述：

-   **证明者（Prover）**：掌握某个秘密信息，希望向他人证明自己「确实知道」这个秘密；
    
-   **验证者（Verifier）**：希望确认某个断言是真的，但不想（或不允许）知道该秘密本身。
    

### **Statement / Witness：你在“证明”什么？**

我们把「要被证明的陈述」称为 **语句（statement）**，把证明者掌握的秘密称为 **见证（witness）**。

-   你的私有身份秘密（如某个随机生成的 `identitySecret`）；
    
-   证明你是选民集合成员所需的 Merkle 路径；
    
-   你选择的具体投票选项等私密数据
    

### **零知识证明必须满足的三个性质**

-   完备性保证「合法选民的合法投票不会被无故拒绝」；
    
-   可靠性保证「非选民或者试图一人多投的人无法通过验证」；
    
-   零知识性保证「验证者无法从证明中推断出你是谁、投了什么票」
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->



# 共学1月22日

Dapp架构总览(from. wachi)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/biduone/images/2026-01-22-1769073534074-image.png)

# DappWorkshop

## ⼯具链混合栈HybridStack

Hardhat Ignition+Foundry+Next.js+Wagmi混合架构

**Hardhat（TypeScript）**

灵活插件⽣态、脚本管理、集成测试

**Hardhat⼯程⼯具箱**

-   HardhatNetwork本地游戏服务器
    

这就好⽐你⾃⼰的私服。⼀切都在本地发⽣，快速且免费。

-   Scripts⾃动机器⼈
    

编写JS代码让机器⼈去部署合约。不再⼿动敲命令。

-   Console聊天窗口
    

⽤JavaScript与区块链实时对话。读写数据就像聊天⼀样简单。

**Foundry(Rust)**

-   毫秒级编译、Solidity单测、模糊测试（Fuzzing test）
    

**工具箱**

-   Anvil（铁砧）
    
    游戏服务器本地启动⼀条区块链。它开着，世界存在；它关了，世界消失。
    
-   Forge（锻造）
    

造物主⼯具⽤来编写和放置合约（⾃动售货机）。

-   Cast（施法）
    

游戏⼿柄⽤来和合约互动（查询余额、按下按钮）。

**现代前端架构：Next.js+Wagmi**

-   ReactComponents + RainbowKit/Tailwind
    
-   Wagmiv2Hooks + TanStackQuery(Caching)
    
-   Viem(JSON-RPCInterface)
    
-   Ethereum|Sepolia
    

Q：解决SSR（服务端）与LocalStorage（客户端）的状态同步

# 南塘DAO

议事规则

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/biduone/images/2026-01-22-1769087633950-image.png)

南塘豆“解耦代币”与内部经济循环模式试探
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->





# 共学1月21日

wachi老师聊未来的发展方向，我本人原来也写前端和一些Node.js（typescript），算是很契合typescript全栈方向。

# Uniswap

## V2 基础原理

-   恒定乘积公式：x \* y = k，价格 p = y / x
    
-   交易与套利：通过价格偏差实现套利，推动价格向市场价靠拢
    
-   流动性提供与无常损失：LP 面临价格波动导致的资产价值相对损失
    
-   闪电贷机制：支持先借后还，提升资金效率
    
-   时间加权平均价格（TWAP） ：链上价格预言机机制
    

## V3 集中流动性

-   流动性区间：LP 可在自定义价格区间内提供流动性，提升资金效率
    
-   Tick 系统：价格离散化，通过 Tick 管理流动性变化点
    
-   流动性聚合与手续费分配：多个 LP流动性聚合，按区间和比例分配手续费
    
-   L 与 P 公式：简化价格与流动性计算，优化合约效率
    

## V4 介绍

-   钩子机制：允许在交易与流动性操作中插入自定义逻辑
    
-   更高的灵活性：支持更复杂的交易对与流动性策略
    

## 生态与资源

-   孵化器、黑客松、审计补贴：支持开发者项目
    
-   基金会资助与教育计划：提供资金、技术、市场等多方面支持
    
-   社区与活动：全球线上线下活动，持续推动DeFi 教育与发展
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->







# Solidity分享

-   EVM是栈执行虚拟机，任务以栈方式执行先进后出，256为块
    
-   选择适合大小的**数据类型可节省gas**(如uint8 uint128)
    
-   引用类型的需要的gas更多（如 string\[\]数组，mapping)
    
-   function的external比public更省gas，external可通过外部和this.xxxFunction()调用
    
-   unchecked{} 在require保证安全的前提下，跳过溢出检查 节省Gas
    
-   require报错会更消耗gas相比revert
    

## ERC-20代币合约

-   Transfer、Approval、mint
    

## ERC-721NFT合约

## 去学习Proxy合约升级

## 去学习uniswap v3代码，aave代码

-   理解它的安全设计
    
-   理解它的存储设计
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->











# 智能合约开发实操

## RemixIDE

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/biduone/images/2026-01-19-1768815539497-image.png)

## EVM相关知识

-   EVM 的栈是 256 位的，**默认uint256最省gas**
    
-   uint类型默认值为0，不同**类型转换会消耗gas**
    
-   **external** 比 **public** 更节省 gas
    
-   减少存储操作（Storage Write）能省gas
    

## 运营管理

### tg管理

拉群、数据分析（加群曲线、发言活跃曲线）、加管理机器人（注意山寨高仿机器人）

### 活动管理

准备阶段

-   邀请：主题、时间、涉及话题内容
    
-   材料： Twitter Space链接、zoom链接、海报(主题、时间、内容)
    

宣发阶段

-   至少提前2-3预热
    
-   当天二次预热
    
-   Twitter Space\\Zoom开始前5分钟，再进行提示参与
    

执行阶段

-   开场：亮品牌、嘉宾IP介绍、活动背景
    
-   问答：N
    
-   互动：N
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->













# 零知识证明(ZK)

在不暴露具体细节的情况下，能够向第三方证明数据的某些特征。  

## ERC-7962

### 核心概念

-   以KeyHash（公钥哈希）而非地址 作为资产所有权标识
    
-   所有权与交易发起地址解耦，不在链上存储地址
    
-   覆盖 ERC-20与 ERC-721 两类资产模型
    
-   用户只需签名表达意图，无需持帀、无需理解 Gas、无需管理地址隐私，也能完成资产操作
    

### 解决的关键问题

-   隐私问题：避免地址可追踪，降低身份与资产的关联性
    
-   UX 问题：用户只需签名表达意图，无需持币、无需理解 Gas、无需管理地址隐私，也能完成资产操作。
    
-   扩展性问题：天然支持批量交易、Relayer、账户抽象场景
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->














# 开发环境

-   基于VSCode+hardhat搭建开发环境，学习solidity结构、 状态变量， 函数， 函数修饰器
    
-   尝试学习“Introduction to Smart Contracts” 和“Solidity by Example”，
    

## **Solidity**状态变量（**State Variables**）

-   public：会自动生成一个同名的 getter 函数，允许外部读取。
    
-   internal：只能在合约内部和继承合约中访问。
    
-   private：只能在当前合约内部访问。
    

## Solidity函数

### 可见性：

-   public：可以从合约内部、继承合约或外部调用。
    
-   external：只能从外部调用，不能从合约内部直接调用（但可以通过this.functionName() 间接调用）。通常用于对外接口函数。
    
-   internal：只能从合约内部或继承合约中调用。
    
-   private：只能从当前合约内部调用。
    

### 状态可变性：

-   pure：不读取也不修改合约状态的函数。
    
-   view：读取合约状态但不修改状态的函数。
    
-   payable：可以接收以太币的函数。
    
-   默认（无修饰符）：可以修改合约状态的函数。
    

从 0.6.0 起， Solidity 对 receive() 和 fallback() 这两个特殊函数做了更清晰的语义区分：

-   receive()：专门处理“只带 ETH、不带 calldata”的转账；
    
-   fallback()：处理“调用不存在的函数”或合约没有 receive() 时的 ETH 转账。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->
















# 智能合约与代码机制

## 基础概念

-   它是部署在区块链上的**一段程序代码**（functions） **\+ 一份持久状态**（state）的组合。 只要输入**条件满足**，就会**按事先写好的规则自动执行**。
    
-   合约一旦部署，字节码本身是不可直接修改。
    
-   如需“升级合约”，常用**代理（Proxy）模式、UUPS**等可升级标准：通过一个固定地址的代理合约持有状态，把逻辑合约替换掉。
    
-   其他计算机程序可以做的事情，智能合约在“链上逻辑 + 状态机 + Gas限制”的约束下基本上都可以做。
    
-   **多重签名**需要 M 个可能的可接受签名中的 N 个签名才能执行（其中 N ≤ M，并且 M > 1）。 普遍使用 `N = 3, M = 5` 和 `N = 4, M = 7`。 4/7 多重签名需要七个可能的有效签名中的四个。 这意味着即使失去了三个签名，资金仍然可以收回。 （**DAO组织治理**）
    
-   智能合约**最大可达 24 KB**
    

## Solidity

-   Solidity是为Ethereum Virtual Machine（EVM）专门设计的合约导向语言，它能直接**编译为EVM字节码**，天然适配以太坊架构，能够高效**操作存储、Gas计算、事件**等链上资源。官方也明确把它定义为“为 EVM 开发智能合约的高阶、**静态类型**、**面向对象**语言。
    

-   使用Hardhat、Foundry 在日常开发、测试、fork 主网模拟
    
-   学习“Introduction to Smart Contracts” 和“Solidity by Example”
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->

















# 以太坊网络

## 节点：

-   **节点=执行端+共识端 \[+验证者端\]**
    
-   \*\*执行端：(EVM + 状态 + RPC)\*\*广播&监听交易与区域、执行交易与合约、维护状态库等。Geth、Nethermind、Besu、Erigon、Reth
    
-   \*\*共识端：\*\*负责权益证明 PoS 共识，维护信标链、投票&决定上链、跟踪验证者、余额、惩罚等共识状态。Lighthouse、Prysm、Teku、Nimbus、Lodestar
    
-   **验证者：** 提议新区块、投票、管理验证乾密钥(类陪审团代表)(在**PoS下，想质押ETH参与出块获取奖励时**)
    
-   **Engine API ：**“专线电话”互相协作，整个节点再通过 P2P。 网络和其他节点连接起来，共同维护以太坊这条“世界计算机”
    

![image.png](attachment:37438c70-5c98-4239-b552-80b22e42ea6a:image.png)

## 节点通信

-   发现阶段（UDP + Kademlia） ：引导节点 + Ping/Pong/FindNode，认识“近距离ID”邻居，将“通讯录”填满。
    
-   连接阶段（TCP + RLPx/devp2p）：与邻居加密TCP连接，一条线路多路复用区块同步、 交易传播等子协议。
    
-   传播阶段（Gossip + 请求/响应） ：新交易和区块通过 Gossip 与邻居广播
    

## 节点类型

-   全节点: 当前状态 + 全部区块（但老状态会被“裁剪”）L
    
-   归档节点: 全节点 + 从创世到现在的“所有历史状态” XL
    
-   轻节点: 只存区块头，需要时向全节点要具体数据。M
    

![image.png](attachment:596d3367-c8b3-4c74-9747-e43e2f06fbe8:image.png)

## 客户端区别与配合

The Merge 之后：

-   \*\*执行客户端是“世界计算机的大脑皮层（算逻辑）”\*\*负责“算结果、管状态”，**共识客户端是“集体神经系统（达成一致）”负责“选哪条结果被全网认可”，两者通过 Engine API 绑在一起**，才构成一台真正的以太坊节点。通过 Engine API 三大动作 newPayload / forkchoiceUpdated / getPayload互相配合
    

## Gossip协议

它是一种点对点（P2P）的通信机制，模仿信息在社交网络中传播的方式。在以太坊中，Gossip 协议用于在节点之间高效地传播信息，如交易、区块和状态更新。

# 账户与结构

## **EOA**（**Externally Owned Account**，外部拥有账户）

-   **address**(地址）+ 助记词(**Seed Phrase**\=私钥签名) + **nonce**（交易计数器）+ **balance**（余额）+ code(EVM 字节码) / storage（合约状态）
    
-   既“私钥”直接控制的钱包账户，能发交易、收资金、调合约，但自己不带代码逻辑。控制私钥的人 = 控制这个账户的一切。
    

## CA （\*\*Contract Account，\*\*合约账户）

-   没有私钥，它的行为完全由账户里的代码（EVM bytecode）决定；
    
-   有自己的地址和 **ETH** 余额，也可以持有 ERC-20 / NFT；
    
-   只能在被 EOA 或其他合约调用时执行
    

链上的每一笔交易，起点仍然是某个 EOA 的签名； 合约账户 再“聪明”，也只是被调用时执行，而不是自己起头发交易。 账户抽象和 EIP-7702 做的事，是让“谁来签、怎么付费、怎么组合操作”更灵活，而 不是改变这一条物理定律。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->


















# Web3攻防安全

### 社会工程类诈骗

-   助记词恢复诈骗
    
-   空投诈骗
    
-   假冒代币空投
    
-   假冒质押合约
    
-   TransferFrom 零转账诈骗
    
-   Telegram 假冒安全验证诈骗
    
-   DeFi 代理安全更新
    

### 钓鱼欺骗

-   USDC/T授权钓鱼
    
-   Punycode 钓鱼攻击 & 近似域名字符欺骗
    
-   剪贴板钓鱼
    
-   函数选择器钓鱼
    
-   交易模拟欺骗-时间差交易攻击
    
-   Google 邮件伪造钓鱼-使用 DKIM 重放攻击
    
-   Token 精度钓鱼攻击
    

# Web3合规

不得在大陆展开业务

不得以大陆网络直接访问

不得用大陆身份注册

保持对行业合规知识持续更新

出入金来源追踪，避免诈骗资金关联
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->



















# **应用&赛道**

## 去中心化金融：DeFi（Decentralized Finance）

-   **Uniswap：** 去中心化交易所（DEX）的代表，自动交易员
    
-   **Compound：**一个去中心化的借贷平台
    
-   **MakerDAO(Sky)：** 是一个去中心化的借贷协议，它允许用户通过超额抵押资产来生成 DAI，一种与美元挂钩的稳定币
    

## NFT

-   **NFT本质：**数字资产的唯一性和所有权
    
-   **智能合约：**自动化的所有权转移和交易
    

### 智能合约

存储在区块链上的程序，由网络节点执行

### 代币（**Tokens**）

智能合约可以创造代币供分布式应用程序使用，例如ERC-20、ERC-721 等

### Pow 与PoS

PoW“挖矿”工作量证明，PoS代币(权益)质押证明

### **dApps**

特征：去中心化、输出确定性、图灵完备、隔离性（EVM沙盒）

应用场景：

-   DeFi(DEX、借贷协议、稳定币、衍生品、保险)，
    
-   资产与内容：（NFT市场、链游、虚拟土地、音乐、视频版税）
    
-   组织与治理（DAO） ： 链上投票、协议治理、公共物品资助（例如 Gitcoin）；
    
-   基础设施类 dApp： 钱包、域名系统（ENS）、身份、预言机、L2 Rollups本身等；
    
-   其他实验性应用： 去中心化社交、链上存证
    

dApp部署在区块链上，不依赖单一服务器，没有传统上的”后台”，很难被单一主体关停。

dApp 后端代码 （智能合约）运行在以太坊主网或其 Rollup （二层网络）之上的 EVM 中，由全网节点共同执行和记录；

## 开发生态系统

**IDE**: RemixIDE

**Solidity三件套：**Hardhat、Truffle、Foundry

**前端集成工具：**Scaffold-ETH、Wagmi

**节点与基础设施服务商：**Infura、Alchemy、QuickNode等

### MetaMask知识

MetaMask密码是只在本设备本次安装有效，私钥和助记词都是存在本地的

助记词 与私钥是 1 以 N的，**绝对不可泄露。**
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->






















## 区块链基础概念

**一、区域链**

**定义**：一个**去中心化的分布式**账本技术，通过将数据打包成按时间顺序连接的“区块”(与《数据结构》的链表非常相似)，在网络节点间安全、透明且不可篡改地记录事务。

### 1\. 特点

-   **不可篡改**：链式结构，修改一个区块导致其哈希值变化，后继区块都必须修改否则链表会断开 。
    
-   **去中心化与高容错**：数据由全球多节点存储运维不会因单点故障丢失。单一实体无法控制整个网络，除非能控制超过51%的节点，否则任何改动都不会被网络认可。
    
-   **公开透明且匿名**：链上所有记录公开的，用户以钱包地址进行交互，地址是匿名的。
    
-   **快速交易:** 打包上链即完成，无中介，跨国汇款高效；
    

* * *

### 二、比特币BTC

**节点矿工奖励**：运维网络的节点（矿工）因提供服务而获得比特币作为奖励，如验证交易。 **货币属性**：比特币总量有限、可自由转账，使其具备了加密货币的属性。其价格由市场供需决定，因此波动较大。

* * *

### 三、区块链的核心组成部分

**去中心化的网络和区块链:**一条链来记录全部的信息，对应的去中心化网络中由无数节点提供

**维持网络运行的代币激励：**维持网络运行需要奖励，燃料费（Gas Fee）就是矿工们的“工资”

### 区链运行逻辑

六个步骤：1用户发起交易 →2 交易广播到网络 → 3节点验证交易 → 4矿工打包成新区块 → 5新区块链接上链 → 6矿工获得奖励。

* * *

### 四、公链-vs-私链-vs-联盟链

去中心化程度：**公链 > 联盟链 > 私链**，主要差异在于 “访问权限” 与 “治理模式”。

**公链**：无需申请、自由进出、所有人可见、去中心化决策(投票)、人太多决策效率低、维护成本高

**联盟链：**需要邀请或申请、权限分级(决策者、观察者）、半公开数据、联合决策、效率比公链高（成员少），隐私较好好（数据不公开），不如私链灵活

**私链：**成员严格审批、固定成员、数据完全私有、老板说了算。。。

* * *

### 五、Web2 Web 3.0 Web3

**Web2**

中心化控制、用户生产内容但不拥有数据；广告驱动商业模式；

**Web 3.0**

区别于Web3技术；通过 RDF/OWL 标准实现准描述数据关系，便于机器理解；核心是 数据组织形式升级变化

**Web3**

区块链驱动；数据主权归用户（用区块链存身份 / 资产）；智能合约替代中介；核心是 “去中心化”；内容永不消失、但不适用于高频社交应用。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/biduone/images/2026-01-12-1768232593683-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/biduone/images/2026-01-12-1768232623854-image.png)

**三者对比**

![image 1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/biduone/images/2026-01-12-1768232636347-image_1.png)

**技术栈**

**Web2 开发**：

`React + Node.js + MySQL`

**Web3 开发**：

`React + Ethers.js(Viem.js) + Solidity + IPFS`

**Web 3.0 开发**：

`Python + RDFLib + SPARQL`

* * *

### 五、去中心化的优势与挑战

### 1\. 优势

-   **降低信任成本、抗审查与高韧性、用户主权、开放创新**
    

### 2\. 挑战

-   **可扩展性瓶颈、安全与治理难题、用户体验复杂、法律与合规风险**
    

## 以太坊概览

### 一、基本核心信息

**定义定位**：开源去中心化区块链平台（“区块链2.0”代表），目标是“**世界计算机**”，支持智能合约，由维塔利克·布特林2013-2014年提出，2014年启动。

**核心创新**：智能合约（链上可执行代码(EVM)，满足条件自动执行，无需人工干预）。

**原生代币ETH**：支付链上费用叫燃料费（Gas Fee），The Merge后呈轻微通缩（年通胀0.2%-0.8%），市值次于BTC。

* * *

### 二、与比特币核心差异

| 对比维度 | 比特币 | 以太坊 |
| --- | --- | --- |
| 目标 | 去中心化数字货币（数字黄金） | 去中心化应用平台 |
| 编程能力 | 仅支持简单交易验证 | 图灵完备（可开发复杂合约） |
| 共识机制 | PoW（算力挖矿） | PoS（质押32 ETH成为验证者） |
| 交易速度 | 10分钟/区块，确认慢 | 12秒/区块，确认快 |
| 经济模型 | 总量固定2100万枚 | 供应灵活，可能通缩 |

* * *

### 三、技术演进与升级

**以太坊1.0（PoW）**：矿工挖矿记账，问题是能耗高、扩展性差（TPS30）。

**The Merge（2022.9）**：主网与信标链合并，切换为PoS，架构分执行层（处理交易/合约）和共识层（管理验证者），“关闭”了 PoW 挖矿，能耗降99.95%。

**Pos机制 准入门槛：**质押 32 ETH

**工作方式**：系统随机选择验证者

**奖励机制**：新发行的 ETH + 交易费

**惩罚机制**：质押的 ETH 被销毁（Slashing？？）

**核心升级方向**：

-   分片技术（2025-2026）：进一步降本；
    
-   EIP-4844（2024.3上线）：L2费用降70%-90%；
    
-   ZK-Rollup：链下批量处理交易，提效降本。
    
-   其他：EIP-1559 成果、Verkle 树技术、执行环境优化(提升 EVM 性能)
    

**发展里程碑图**

[https://web3intern.xyz/assets/ethereum-roadmap\_01-Bi2f1MRr.jpg](https://web3intern.xyz/assets/ethereum-roadmap_01-Bi2f1MRr.jpg)

![](https://web3intern.xyz/assets/ethereum-roadmap_01-Bi2f1MRr.jpg)

* * *

### 四、生态架构

1.  **层级**：L1（主网+EVM+账户系统）、L2（Rollup为主）、侧链（Polygon PoS等）。
    
2.  **应用层(核心应用)**：
    
    -   DeFi：Uniswap、Aave；
        
    -   NFT：OpenSea；
        
    -   钱包：MetaMask；
        
    -   DAO工具：Snapshot。
        
3.  **协议层**
    
    EVM、状态管理、Gas 机制
    
4.  **扩展层**
    

* * *

### 五、核心价值观

去中心化治理、无需许可开放、抗审查、密码朋克精神（代码即法律）、可持续发展。

### 六、核心机制

1.  **账户系统**：EOA（用户钱包，私钥控制）、CA（合约账户，代码控制）。
    
2.  **Gas模型**：总费用=Gas Limit（最大燃料）×Gas Price（单价），EIP-1559拆分基础费用（销毁）和小费。
    
3.  **EVM**：执行智能合约的虚拟计算机，图灵完备、全球同步、隔离安全。
    

* * *

* * *

* * *

**未完待续。。。看麻了，学麻了**
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
