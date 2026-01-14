---
timezone: UTC+8
---

# ShawnX-F

**GitHub ID:** ShawnX-F

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
4\. 账户类型与结构

1) EOA = 由“私钥”直接控制的钱包账户，能发交易、收资金、调合约，但自己_不带代码逻辑_。控制私钥的人 = 控制这个账户的一切。

主要风险：私钥/助记词丢失或泄露。

_注：_

a. nonce（交易计数器）对于EOA来说，表示本账户已经发生过几笔交易。一笔交易的nonce必须等于账户的nonce，打包成功后nonce+1，同一个nonce只允许执行一次，用以**防止重放攻击&保证交易有序**。

b. 一个交易在内存池还没被打包前，使用相同nonce+更高的Gas Fee可以替换掉旧交易。收款地址填自己，金额可0或极小值，可以“取消”原交易；收款地址不变，金额改变，可以“加速”原交易。目前很多钱包都有内置的“Speed up”和“Cancel”功能，可以一键替换。

2) 合约账户

可以持有ETH/token，内部存code（不可改）&storage（随调用改动），在执行中给其他地址转账、调用其他合约地址、使用CREATE/CREATE2创建新合约。

CREATE：地址 = f(sender, nonce) → 不可预测、不可重用

CREATE2：地址 = f(sender, salt, bytecode) → 可预测、可跨链统一、可重部署_（危险）_

主要风险：代码漏洞/逻辑错误/权限设计不当

3) ERC20/ERC721代币与合约账户的关系

地址中显示的“拥有”某个ERC20/ERC721代币，并不是地址里有代币，而是**对应的代币合约的storage里记录了某个地址拥有多少代币或者某个tokenID的owner是谁。**
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

（书接上文）

4\. 以太坊的去中心化实现机制

1) 共识与出块（技术/经济层）：采用PoS共识，任何人只要抵押在32ETH及以上时，即可成为验证者（validator），参与区块提议和投票。每个slot，区块会在随机选择的提议者（proposer）中产出，同时数百验证者在同一时隙对链头进行投票（attestation），通过Gasper共识（Gasper-FFG提供终局性+LMD-GHOST提供链头选择）完成安全性与活性。

a. Slot是以太坊信标链（Beacon Chain / Consensus Layer）的时间基本单位，整个网络的时间被均匀切割成一个个 12秒的 slot。

b. Gasper = Casper-FFG 套在 LMD-GHOST 上面

-   日常运行时：验证者先用 LMD-GHOST 选链头 → 提议新块 → 其他人用 LMD-GHOST 投票支持。
    
-   同时，所有 attestation 里还携带 Casper-FFG 的投票（针对两个 checkpoint 之间）。
    
-   Casper-FFG 周期性地看这些投票，决定哪些块可以 justified/finalized。
    
-   结果：链快速生长（LMD-GHOST 保证活性） + 历史块被逐步锁定（Casper-FFG 保证安全性）。
    

| 组件 | 全称 | 主要职责 | 提供的主要属性 | 典型表现形式 |
| Casper-FFG | Casper Friendly Finality Gadget | 提供“终局性”（finality） | 安全性（Safety） | 把某些 checkpoint 升级为 finalized |
| LMD-GHOST | Latest Message Driven - GHOST | 提供“链头选择”（fork-choice） | 活性（Liveness） | 决定当前链头（head），链持续生长 |

2) 节点与网络（技术层）

以太坊执行层使用DevP2P协议维护点对点网络：

-   发现层（Discovery stack）基于UDP，用来发现和加入新节点；
    
-   DevP2P通信层基于TCP，负责在已连接节点间传递区块、交易等实际数据。
    

3) 以太坊的客户端多样性（实现层）

-   执行层客户端EL（Execution Client）：Geth、Nethermind、Besu、Erigon、Reth 等；
    
-   共识层客户端CL（Consensus Client）：Lighthouse、Prysm、Teku、Nimbus、Lodestar 等。
    

[clientdiversity.org](http://clientdiversity.org) 这类站点会定期占比过高的客户端，提醒质押者「换个少数派客户端」。目标是避免「一个客户端一旦出 bug，全网一起躺平」的单点风险。

4) MEV与PBS（市场结构层）

为缓解最大可提取价值MEV，引入了PBS（提议者-建构者分离）思路，主流的实现是协议外的MEV-boost中继系统：

可以理解为“区块拍卖中介”，从 Builder 收集高 MEV 区块，挑出给验证者报酬最高的那个 header，让验证者签名后广播完整区块，从而让所有验证者（不管大小）都能分到 MEV 大蛋糕，但引入了中心化与信任风险，未来会被协议ePBS 取代。

5) Rollup中心路线与数据去中心化（扩容/职责分离）

L1：更小、更稳，负责安全、数据共识；L2：执行负载、创新

6) 治理（社会层）

“客户端开发团队实现 + 节点运营者选择是否升级软件”共同决定是否升级。

5\. 网络结果与节点类型

1) 客户端类型：

-   干活的EL：监听交易与区块、在EVM中执行交易和智能合约、维护当前链的状态数据库（账户余额、合约储存等）、提供JSON-RPC接口（eth\_\*)、通过自己的P2P网络转发交易和区块头/区块体；
    
-   决策的CL：维护信标链视图和fork choice规则（LMD-GHOST等），每个slot中做出选择（区块提议）、并对区块做出投票，跟踪共识状态（验证者集合、质押余额、惩罚等），根据EL提供的结果，决定上链的区块；
    
-   陪审的Validator：管理验证者密钥，提议新区块，（区块）投票。用于质押、参与出块并获得奖励。
    

以太坊节点=EL+CL+Validator（验证者节点可选），CL需要Engine API来向EL发号施令。行客户端（EVM + JSON-RPC）——调用 eth\_\* / 部署合约 / 查状态执；共识客户端 + 验证者客户端——质押 32 ETH 做验证者 / 关心 attestation、finality、slashing。

2) 节点间的连接与通信方式

先找到人 → 再拉上加密专线 → 最后靠八卦（gossip）把消息传满全网。（Kademlia风格的DHT发现机制）

A. 节点发现（UDP+Kademlia）

a. 新节点给引导节点ping，引导节点回pong，确认引导节点在线；

b. 发送Findnode，请求，对方回复Neighbors（discv4）或者Nodes（discv5），里面是其他活跃节点的联系方式；

c. 对活跃节点重复以上操作，直到达到预设上限。

B. 建立安全连接（TCP+RLPx/devp2p）

a. TCP搭好稳定管道；

b. RLPx在一个TCP里多路复用多种协议：以太坊在 TCP 之上跑的是 devp2p 协议栈，其底层加密与多路复用由RLPx 负责。

C. Gossip+请求/响应传播“八卦”

Gossip用于传播新交易喝区块，请求/响应用于按需拉取缺失的历史区块、状态。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->


_Tips_：各位uu们可线下编辑好，再粘贴到打开界面；如果电脑网页端打卡界面编辑时，一定要注意，鼠标不要！点击！界面以外的位置！编辑中的内容不会自动保存，会消失！（心痛教训）

真的非常开心能参加本次的web3实习计划，之前虽也参加过一两期共学营，但一直觉得掌握的不到位，有很多根上的东西不太明白。希望通过本次学习，能藉由循序渐进的学习路径和查漏补缺，全面理解web3和以太坊。

**前期笔记以查漏补缺为主**

1\. Gas Fee

EIP-1559之前，Gas Fee = Gas Used \* Gas Price

EIP-1559之后，Gas Fee ≈ （Base Fee + Priority Fee）\*Gas Used

其中，钱包中maxFeePerGas/maxPriorityFeePerGas是用户愿意支付的上限，实际扣费不会超过这个上限。

2\. dApp核心技术特征（为什么要用以太坊跑应用的底层逻辑）

1) 去中心化

2) 确定性（智能合约）

3) 图灵完备以及

4) 隔离性（沙盒）：dApp的执行被隔离在EVM里，单个合约逻辑的失败只会导致其自身的调用失败（回退），而不会使整个以太坊网络宕机；全网节点只是共同重放并验证同一套字节码，把终极信任从人/机构转移到了“确定性计算”本身，这是区块链能做到“极致去信任”的最关键密码学社会学机制之一。

3\. dApp开发的缺点/代价

1) 维护困难：合约部署后，默认无法更改

2) 性能开销与扩展难度：追求性能和价格，优先考虑L2；但最终安全性上还是要落在L1

3) 网络拥堵和Gas费问题：即便有L2，但部分高价值操作，如大额清算和跨链桥结算等，还是会回到L1；开发者应通过减少无谓存储/循环和前端Gas提醒，必要时支持多条L2切换来提升用户体验。

4) 用户门槛高：还是基于web3自身的普及率问题，需要时间，精进和推广。

5) “再中心化”倾向：为了降低门槛，有些产品会有在服务端代管私钥，把前端放在CDN上和链下逻辑链上结果的操作。

（未完待续）
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
