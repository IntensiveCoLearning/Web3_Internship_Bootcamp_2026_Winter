---
timezone: UTC−8
---

# chiahao-dev

**GitHub ID:** chiahao-dev

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生
base 加拿大 一年半Java后端工程师，半年网络工程师，性格是infp。最早接触到行业是曾经臭名昭著的luna币事件，后面发现web3已经开始慢慢地改变世界，加拿大的咖啡馆里都有了比特币的自助结账机。我在读完中本聪比特币的白皮书后，开始对web3逐渐感兴趣，希望转行到web3行业，并且能小小地推动整个行业和世界的发展。方向是Defi相关的开发，已经学完了Solidity基础，接下来就是All in Web3!
base 加拿大 一年半Java后端工程师，半年网络工程师，性格是infp。最早接触到行业是曾经臭名昭著的luna币事件，后面发现web3已经开始慢慢地改变世界，加拿大的咖啡馆里都有了比特币的自助结账机。我在读完中本聪比特币的白皮书后，开始对web3逐渐感兴趣，希望转行到web3行业，并且能小小地推动整个行业和世界的发展。方向是Defi相关的开发，已经学完了Solidity基础，接下来就是All in Web3!

## Notes

<!-- Content_START -->
# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->
第三周第一天打卡

今天租好了房子，明天开始进行内容
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->


Day13  
  
今天提交“良心杀”完整投票方案，在深圳找了一天房子。下周开始再次全力投入到学习中
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->



Day12

今天需要完成的任务：

1\. 查看「南塘 Dao 主题」回放 √

2\. 参与 LXDAO 周会 260124 √

3\. 「公共物品资金分配游戏」任务 - 提案投票 √

4\. 撰写学习总结，并发布社媒平台 √  
  
Review：今天一整天都在飞机上，终于从加拿大回国了。实在没有多余精力学习新内容，水一天
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->




Day12  
  
今天飞回国，打卡一下 XD
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->





### Day11  
  
Day11

今天需要完成的任务：

1\. Figma 安装或在线创建账号 √

2\. 在 remix 中运行 Solidity by Example | 0.8.26 Basic 部分的代码 √

3\. 挑战：Crowdfunding √
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->






### Day10

今天需要完成的任务：

1\. 挑战：Challenge #1 - Decentralized Staking App （中文版链接）√

2\. 完成 设置开发环境并熟悉 Remix IDE √

3\. 完成 通过 Ethernaut 前 3 关 √

4\. 学习Uniswap V2 代码 V2 Core 未完成

Review: 今天效率不高，学习了有关Uniswap 的相关内容，截至学习web3相关内容到现在，这部分是最难的，明天需要多花一些时间消化。入门技术对我来说还是很简单的，但是深度技术又有一点困难，看来还是需要继续沉淀技术。Xiaohai老师提到了reactive，明天也研究一下
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->







### Day9

今天需要完成的任务：

1.挑战：Challenge #1 - Decentralized Staking App （中文版链接）完成了一半，明天早起完成

2.体验 My first zk vote (1 hour) 已完成√
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->








### Day8

今天需要完成的任务：

1\. 挑战：Challenge #0 - Tokenization （中文版链接）√

已完成，部署地址：[https://challenge-tokenization-difpjh93h-0xchiahaos-projects.vercel.app/](https://challenge-tokenization-difpjh93h-0xchiahaos-projects.vercel.app/)

2\. 021 学习以太坊第 5 章 完成√

3\. 学习中文排版规范：[https://github.com/sparanoid/chinese-copywriting-guidelines](https://github.com/sparanoid/chinese-copywriting-guidelines) 完成√  

1\. EVM：以太坊的“心脏”与“大脑” EVM（以太坊虚拟机）不仅仅是一个运行代码的环境，它是一个确定性的、栈式的状态机。

• 架构本质：EVM 是一个全网共享的“CPU + 状态机”。它读取旧状态（State）和交易（Transaction），通过执行智能合约的字节码（Bytecode），计算出新状态。所有全节点运行完全相同的 EVM 规则，确保了共识的一致性。

• 内部组件：EVM 内部包含\*\*栈（Stack）\*\*用于运算、\*\*内存（Memory）用于临时数据处理、以及最昂贵的存储（Storage）\*\*用于持久化状态。此外，它通过执行上下文（如 msg.sender）来管理权限和资金流向。

• 执行流程：合约源码（Solidity）被编译为字节码。当交易触发合约时，EVM 采用“逐条执行”模式，即 Fetch-Decode-Execute 循环。它将字节码解析为操作码（OpCode，如 ADD, SSTORE），一步步修改栈、内存或存储。这种逐条执行机制是精确计费和防止 DoS 攻击的基础。

2\. Gas 机制：安全与经济的博弈 Gas 不仅是计费单位，更是以太坊的防御系统。

• 设计目的：Gas 的核心作用是防止无限循环（图灵停机问题）和资源滥用（DoS 攻击）。通过给每个 OpCode 定价（如简单的 ADD 便宜，涉及磁盘 IO 的 SSTORE 昂贵），网络迫使攻击者付出巨大的经济成本。

• EIP-1559 变革：London 升级引入了新的费率模型。费用被拆分为 Base Fee（基础费） 和 Priority Fee（优先费/小费）。Base Fee 根据区块拥堵程度动态调整并被直接销毁（Burn），这让 ETH 具有了通缩属性；而 Priority Fee 则是真正付给验证者的激励。

• 异常处理：当交易执行中 Gas 耗尽（Out of Gas），EVM 会触发异常，回滚所有状态更改（原子性），但不退还已消耗的 Gas。这是为了防止攻击者免费消耗全网算力。

3\. 合约优化：开发者必修课 章节最后强调了基于 Gas 模型的代码优化策略：

• 存储是瓶颈：写 Storage（SSTORE）是链上最贵的操作。优化原则是“读多写少”，尽量使用 memory 或 calldata 进行中间计算，最后一次性写入 Storage。

• 临时存储新特性：Dencun 升级引入了 transient storage (EIP-1153)，允许在单笔交易内存储临时状态，比写入 Storage 更省 Gas，适合重入锁等场景

Q27：请深入剖析 EIP-1559 机制下，maxFeePerGas、maxPriorityFeePerGas 和 Base Fee 三者的动态关系。如果用户设置的 Max Fee 低于当前 Base Fee，交易会发生什么？

A27: “在 EIP-1559 模型下，用户实际支付的单价计算公式是：实际单价 = Min(maxFeePerGas, BaseFee + maxPriorityFeePerGas)。

1\. 三者关系：Base Fee 是协议根据上一区块拥堵情况自动计算的“底价”，这部分会被直接销毁。Priority Fee 是给验证者的小费。Max Fee 是用户愿意支付的绝对上限。

2\. 异常情况：如果用户设置的 maxFeePerGas 低于当前的 Base Fee，这笔交易不仅不会被打包，甚至大概率不会进入内存池（Mempool）。因为 Base Fee 是协议强制要求的最低门槛，连销毁的部分都付不起，验证者无法将其纳入区块，否则该区块会被网络拒绝

Q28: 在 EVM 中，如果一个智能合约在执行了一系列状态修改（如转账、修改变量）后，触发了 Out of Gas 异常或显式调用了 revert，链上状态和 Gas 费用会发生什么变化？请从底层机制解释原因。

A28: “这涉及 EVM 的原子性设计。

1\. 状态回滚：一旦触发 Out of Gas 或 revert，EVM 会强制中断执行，并回滚当前交易上下文中所做的所有状态修改（包括内部调用的子合约修改）。这就好比这笔交易从未修改过账本，保证了数据库的一致性。

2\. Gas 结算差异：

◦ 如果是 Out of Gas：所有预支付的 Gas 都会被消耗殆尽，不予退还。这是为了防止攻击者利用失败交易发起 DoS 攻击，免费消耗全网算力。

◦ 如果是显式 revert：通常会退还剩余未使用的 Gas，仅扣除执行到 revert 指令前消耗的部分（但在较旧的 EVM 版本或特定 OpCode 下行为可能不同，现代 Solidity 开发主要关注剩余 Gas 退还）。

3\. 链上记录：虽然状态回滚了，但这笔交易本身（Hash、Nonce 等）依然会被打包进区块，只是其状态标记为‘失败’。”

Q29: 在 Solidity 开发中，为什么说 Storage 是 Gas 杀手？在 Dencun 升级（EIP-4844/EIP-1153）后，针对“重入锁”或“临时计算”这类场景，你有哪些新的优化思路？

A29:

“Storage 昂贵是因为它需要操作底层的 Merkle Patricia Trie 并进行磁盘 I/O，这是一个永久性的全网状态变更，因此 SSTORE 指令定价极高。

针对优化思路，除了传统的‘用 calldata 代替 memory’和‘缓存 Storage 到 Stack’外，Dencun 升级带来了质变：

1\. 瞬态存储（Transient Storage, EIP-1153）：这是最新的优化利器。对于重入锁（Reentrancy Guard）这种只在单笔交易内有效、交易结束后无需保留的状态，以前我们被迫使用昂贵的 Storage。现在可以使用 TSTORE 和 TLOAD 操作瞬态存储。它的数据随交易结束而丢弃，不占用永久状态，Gas 成本远低于 Storage，是实现低成本重入保护的最佳选择。

2\. Blob 数据（EIP-4844）：虽然主要服务于 L2，但对于需要临时发布大量数据供链下验证但不需要合约读取的场景，Blob 也是一种极致的 Gas 优化方案。”
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->









### Day7

自动化市场制造（AMM）与恒定乘积公式

Uniswap V2 的核心设计是一个恒定乘积公式：x × y = k

x 和 y 是池子两个代币的余额（比如 USDC 和 ETH）。

k 是一个常数（不变数）。

在 Uniswap V2 中：

每个交易对都有一个流动性池（Liquidity Pool），由两种代币组成。

任何用户都可以成为 流动性提供者（LP），通过向池子存入等价值的两种代币来供给流动性。

LP 会收到代表其份额的 LP 代币（Pool Tokens），可以随时兑换回原始资产 + 交易手续费收益。

💡 LP 赚什么？

每笔交易协议收取 0.30% 的手续费，这笔费直接加入到池中，从而增加了 k 值，LP 按其占比分享手续费收益。  

Q24: Uniswap 和中心化交易所的区别是什么？

A24:

Uniswap 的交易不同于中心化交易所：

没有买卖订单簿，用户直接和池子交互。

当你提交一个交换（比如 ETH ➡ USDC）：

你将输入的 ETH 转入池子。

池子改变两个资产的余额，按恒定乘积公式调整价格。

合约向你发放相应数量的输出代币。

价格变化来自流动性池自动重新调整资产比率，并且不可能让余额乘积减少。  

Q25: 流动性提供者（LP）每笔交易协议收取 0.30% 的手续费，这0.30% 的手续费是给了LP，还是LP支付给uniswap？

A25： Uniswap V2 的 0.30% 交易手续费，100% 是支付给流动性提供者（LP）的，LP 不需要把这笔钱“交给 Uniswap”。  

Q26: 为什么每笔交易都会增加k，这和之前描述的k是一个不变的量是否冲突？

A26: 不冲突。

x · y = k 在 Uniswap V2 中是“单笔交易内的约束不变量”，

而 手续费使得每一笔交易结束后的 k 变大。

恒定乘积公式的真实含义是：

在一次 swap 执行完成时，新的储备必须满足：(x\_after) · (y\_after) ≥ (x\_before) · (y\_before)

单一交易过程中，k 是一个下界不变量（cannot decrease）,也就是说 k 不允许变小，允许变大，永远不会变小

在 Uniswap V2 中，x·y=k 是 swap 的不减约束而非时间常数；

手续费使得池子储备在每次交易后增加，因此 k 会随交易累计而单调上升。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->










### Day6  

今天尝试阅读Uniswap V2 学习官方文档，这一周作息日夜颠倒，感觉很难深入思考。上次反应过后，主办方很贴心地将直播和重播设置为同组任务，在此感谢ethpanda和lxdao人性化的措施！今天调整一下作息，早点休息了。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->











### Day5

Q23: 以太坊代理合约的升级过程是什么样的？过程是什么？

A23:

① 部署 v1 实现合约（LogicV1）；

② 部署 Proxy，指向 LogicV1，初始化状态；

③ 运行一段时间后，发现需要升级：

部署 LogicV2（保证存储布局兼容）；

由 Admin （往往是多签 + Timelock）调用升级函数，把实现地址改成LogicV2；

用户继续用原来的 Proxy 地址交互，但底层逻辑已经是 V2 了。

Review: 今天完成了021学习以太坊3&4章内容的学习，看了同学们分享的内容，收获很多，尤其是针对学习方法的：

1.每天首先列出自己的详细目标，然后逐一完成，这样我觉得会提高效率。

2.使用思维导图，将知识点列出来。

明天计划看完Day1-5的视频，然后学习Uniswap V2 学习官方文档。如果还有时间的话，尝试完成实践与挑战 - Simple NFT。同时Draken老师分享了两个链接：[https://hackmd.io/@wongssh/SyIUoNqdxx#%E7%8E%B0%E4%BB%A3%E5%90%88%E7%BA%A6%E7%89%B9%E6%80%A7](https://hackmd.io/@wongssh/SyIUoNqdxx#%E7%8E%B0%E4%BB%A3%E5%90%88%E7%BA%A6%E7%89%B9%E6%80%A7) [https://blog.wssh.dev/](https://blog.wssh.dev/) ，看了一眼蛮有兴趣的，明天阅读一下。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->












### Day4

Q19：合约账户都有什么能力，会有什么限制？

A19：

能力：

持有 ETH / Token；

内部存 code & storage，可以实现任意复杂逻辑：DEX、NFT、DAO 等；

可以在执行中给别的地址转账、调用其他合约、甚至使用 CREATE / CREATE2 再创建新合约。

限制：

没有私钥，因此不能像 EOA 一样“签名并主动发起外部交易”；

所有行为都必须由外部交易触发（EOA 调用它，或另一个合约在执行中调用它）。

部署合约必须付较高 gas：因为需要把代码永久存储在链上，这在 EVM 里是昂贵操作。

部署后的代码一般不可变（除非自己写了代理 / 升级模式），只有storage 可以随调用改变。  

Q20: 以太坊中每个账户都会有的信息是哪些？

A20:以太坊中的每个账户（无论是 EOA 还是合约账户）在“世界状态” （world state）里，都被建模为一个四元组：

σ\[a\] = (nonce, balance, storageRoot, codeHash)

nonce：

对 EOA：记录该地址已经发送过多少笔交易；

对 合约账户：记录该合约地址“创建了多少个合约”（通过CREATE / CREATE2）。

balance

账户持有的 ETH 数量（单位为 wei，1 ETH = 10¹⁸ wei）。

EOA 和合约账户都可以持有余额，并且都能收款 / 付款（只不过合约“付款”的逻辑由代码控制）。

storageRoot

这是一个 Merkle-Patricia Trie （或将来 Verkle Trie）根哈希，指向该账户的“存储树”（account storage trie）。

只有合约账户才真正使用这个字段：用来保存合约的状态变量（mapping、uint、address 等）的键值对；

对 EOA，规范上这个字段也存在，但值是“空存储根” （默认值）。

codeHash

合约账户：EVM 字节码的哈希 —— 用来标识和校验合约代码；

EOA：没有代码，规范上 codeHash 是空字符串的哈希（相当于“没有代码”的占位标记）。  

Q21: 以太坊里都有什么类型的账户？：EOA是什么？和合约账户有什么区别？

A21:

以太坊里只有两类账户：

① EOA：

• 有私钥

• 能“主动发起交易”

• 没有合约代码

② 合约账户（Contract Account）：

• 无私钥，由部署时的代码“锁死行为”

• 不能主动发起交易，只能“被调用”后按合约逻辑执行

• 有代码和存储（storage）  

Q22:EOA是什么？和合约账户有什么区别？

A22: EOA（Externally Owned Account，外部拥有账户）

由公私钥对控制，用户（人 / 机构）持有私钥；

可以发起交易（转账、调用合约）、持有 ETH 和各种代币；

账号本身没有代码，不会自动执行任何逻辑。  
  
Review：今天状态回暖，虽然看得不多，但是内容理解较为深刻。在今天的co-learning里，老师提到了保持输出，这是一个很重要的点，之前几天的学习中我都只是在输入，从明天开始，在review中，输出自己的理解和内容，并且保持下去。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->













### Day3

Q10: 目前以太坊 L1 实际吞吐大约在 15–30 TPS 左右，不会无限提升，否则会伤害去中心化。为什么提升吞吐速度会伤害去中心化？

A10: 以太坊的设计目标是：普通人也能在家跑全节点。L1 吞吐量 ≈ 网络中“最弱节点”能承受的上限。从逻辑上来讲：区块更大 → 节点需要更快下载 → 低带宽节点跟不上 → 节点退出 → 节点数量下降 → 去中心化下降

Q11: 常见 L2 类型都有哪些？

A11: Rollups（汇总链） 把大量交易打包，然后把压缩后的数据和证明提交到 L1；

状态通道（State Channels）双方把一部分状态锁进合约里，然后在链下反复互相更新，最后只把最终结果结算回链上；适合高频、双边交互。

侧链（Sidechains）独立链，有自己的共识和安全假设，通过跨链桥与以太坊互通，例如早期的 Polygon PoS 等；更像是“兼容以太坊的另一条链”，而不是严格意义上的 L2。

Q12: 以太坊的网络结构是什么样的？到底能不能“无限制参与”？

A12: 以太坊的网络结构 = 理论上对任何人开放（permissionless），现实中受限于硬件和带宽，但通过 Layer 2 + 数据分片等方案，把“更多人参与”和“系统还能跑得动”两件事尽量同时做到

Q13: 以太坊P2P网络主要是由什么组成的？

A13: 以太坊的 P2P 网络主要由 UDP 的 Node Discovery 协议 + TCP 上的 RLPx/devp2p 协议 组成

Q14: 如何在一个拥有成千上万节点的网络中，把新交易和新块尽快传遍？

A14: 以太坊用的是两种模式：① Gossip（广播式传播）② 点对点请求-响应（拉取指定数据）

Q15: 以太坊节点由什么构成？它们是如何配合的？

A15: 以太坊节点 = 执行客户端 + 共识客户端 + 它们之间的 Engine API。

一个典型“出块 / 同步”流程大致是这样：

共识客户端收到新区块 / 或被选中提议新区块

执行客户端执行交易，返回执行结果

共识客户端把执行结果“装进” Beacon 区块

其他节点的共识客户端接收区块 → 交给本地执行客户端复验

总结：执行客户端负责“这块里的交易是否合法？最终状态是什么？”

共识客户端负责“我们全网要不要接受这块当下一步？哪条链是正统？谁作恶要被 slash？”

差异：• 调用 eth\_\* / 部署合约 / 查状态？

→ 找的是 执行客户端（EVM + JSON-RPC）。

• 质押 32 ETH 做验证者 / 关心 attestation、finality、slashing？

→ 这是 共识客户端 + 验证者客户端 的工作。

• 为什么要拆两块？

→ 为了模块化、易维护、多客户端实现、独立升级，以及更清晰的安

全边界：执行层可以专心把 EVM 和状态做到极致性能，共识层可以独立演进PoS、PBS 等协议设计。

一句话做结尾：

The Merge 之后：执行客户端是“世界计算机的大脑皮层（算逻辑）”，共识客户端是“集体神经系统（达成一致）”，两者通过 Engine API 绑在一起，才构成一台真正的以太坊节点。

Q16: Merge之后，以太坊节点内部是如何配合的？

A16: 执行客户端负责“算结果、管状态”，共识客户端负责“选哪条结果被全网认可”，两者通过 Engine API 三大动作 newPayload / forkchoiceUpdated / getPayload 互相配合 —— 这就是 Merge 之后以太坊节点内部的日常节奏。

Q17: 节点同步的内容都有哪些？都有哪几种同步方式？

A17: 节点要同步的内容主要包括两部分：

① 区块链数据（每个区块及其头信息）

② 状态数据（每个账户余额、合约存储、代码等）

同步模式，以太坊支持三种主要的同步方式：1. FullSync（全同步）2. SnapSync（快照同步）3. LightSync（轻节点同步）

Q18: Gossip 协议是什么？

A18: Gossip 协议相当于以太坊的“去中心化广播系统”： 它让每个节点只需和少数邻居聊天，就能在几秒内把新交易、新区块及 PoS 投票散播到整个网络，从而在没有中心服务器的前提下，实现高可用、高容错、可扩展的全网信息同步。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->














# Day2

Q4: 讲一下Solidity中都有哪些函数修饰符？

A4:

可见性修饰符表

修饰符 可见范围 描述 使用场景

public 内部 + 外部 任何地方都可以调用 对外提供的公共接口

external 仅外部 只能从合约外部调用 外部用户接口，gas 效率更高

internal 内部 + 继承 当前合约和子合约可调用 内部逻辑函数，需要被继承

private 仅内部 只有当前合约可调用 私有实现细节

状态修饰符表

修饰符 状态读取 状态修改 Gas 消耗 描述

pure ❌ ❌ 低 不读取也不修改状态的函数

view ✅ ❌ 低 只读取状态，不修改状态

payable ✅ ✅ 正常 可以接收以太币的函数

无修饰符 ✅ ✅ 正常 可以读取和修改状态  

Q5: 讲一下前端与合约交互的过程？

A5:

1.初始化连接：前端检测并连接 Web3 提供者

2.用户授权：请求用户授权访问钱包账户

3.合约实例化：使用 ABI 和合约地址创建合约实例

4.函数调用：通过合约实例调用智能合约函数

5.交易签名：钱包对交易进行数字签名

6.广播交易：将签名交易发送到区块链网络

7.状态更新：获取交易结果并更新前端界面  

Q6:讲一下你是如何对Gas进行优化的？(讲一下常见的Gas优化技巧)

A6:

1.减少存储操作（Storage Write）

2.使用位压缩（Bit Packing）

3.循环优化

4.函数可见性选择 - external 比 public 更节省 gas，适用于仅被外部调用的函数。

Q7: 关于任务时间评估，你一般的经验是什么？

A7:比如你觉得一个任务需要 3 天，那么要根据你的熟悉程度来调整。

如果你对这个技术栈非常熟练，那么乘以 1.25，大概 4 天。如果以前做过类似的东西，乘以 1.5，大概 4.5 天。如果完全没接触过，那就乘以 2，需要 6 天。更重要的是，要学会拆分任务。不要说“做一个网站”这么大而空的描述，而是要列出“设计数据库结构”“搭建前端框架”“实现用户登录”这样的具体步骤。然后在总时间外再加 10%-15% 的缓冲时间。最后强烈建议在开始做之前先跟 Mentor 同步一下你的计划，让有经验的人帮你校验一下是否合理。经过你 Mentor 的建议，如果做出来达不到预期其实 Mentor 也有锅，也不会怎么责怪你。

(Bruce老师分享的经验，让我感触颇深。之前上一段工作中，经理让我自己来评估任务周期，当时没有做过类似的事情，完全没有头绪。最后草草计划了一个月完成的任务，硬生生拖了三个月以上。经理在这个过程中，也时常向我抱怨我没有即时和他沟通项目进展。Bruce老师分享的这部分内容，既有合理的沟通解决方式，又有可以量化的标准，好评)  

Q8: Gas费是如何计算的？

A8:

Gas 费的两个公式（旧机制 vs 现机制）

① EIP-1559 之前（旧版）: Gas Fee = Gas Used × Gas Price

• Gas Used：交易实际消耗的 Gas 单位数；

• Gas Price：用户愿意为每单位 Gas 支付的价格（如 20 Gwei）。

② EIP-1559 之后（当前主网机制）: 实际支付费用 ≈ (Base Fee + Priority Fee) × Gas Used

• Base Fee：协议根据区块拥堵情况自动调整，并在执行后被销毁；

• Priority Fee：用户自愿支付的小费，用于激励验证者优先打包；

• 钱包里看到的 maxFeePerGas / maxPriorityFeePerGas 是你愿意支

付的上限，实际扣费不会超过这两个上限组合对应的值。  

Q9: 为什么每天 Gas 费忽高忽低？

A9: ① 网络拥堵（供需决定价格）② 全球用户活跃时间差 ③ 交互复杂程度

行业前辈经验总结：

“小项目-社区-开源-Hackathon”渐进式成长路径

“预热”学钱包签名、EVM 基础；

“小项目”自己做个 Mint NFT 或 DEX 数据可视化；

“社区”参加 DAO 共学／小型 Hackathon，先当 Member 再做 Contributor；

“升级”给开源钱包或交易所提 PR。

把目标缩小、快速闭环，比一次冲大型 Hackathon 更容易坚持。

LXDAO “残酷共学”、“休闲黑客松”、高校区块链社团 Mini‑Hack、开源仓库 good‑first‑issue 都是低门槛入口。完成一个小闭环后再挑战大型 Hackathon，会轻松许多。  

今天完成内容：

1.阅读完Web3 实习手册的所有内容

2.学习完021 LEARN ETHEREUM 第一章

Review: 在群友的提醒下，才发现深度技术里的课程安排，稍微有点落后进度，明天补回来！
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->















\# Web3 Day 1

\## 1. 阅读 Web3 实习手册 & 笔记整理

\### 求职相关技术栈

\#### 前端

\> 当前前端 Web3 开发中，\*\*Ethers.js / Web3.js 使用频率正在下降\*\*，主流逐渐转向 **Viem**

\- HTML5

\- CSS3

\- JavaScript (ES6+)

\- React / Vue

\- TypeScript

\- Next.js

\- Ethers.js / Web3.js / **Viem**

\#### 后端

\- Node.js / Go / Python

\- Viem / Web3.js / Ethers.js

\- RESTful API / GraphQL

\- MySQL / PostgreSQL

\- Docker / Kubernetes

\#### 智能合约

\- Solidity

\- Remix

\- Foundry / Hardhat

\- Phalcon / Tenderly

\- Yul

\---

\### 求职平台推荐

\#### 中文平台

\- **DeJob**：中文，大量高频 Web3 工作机会

\- **SmartDeer**：中文，综合性招聘平台

\- **电鸭社区**：中文，远程工作者社区

\#### 英文平台

\- **Web3Career**：专业的 Web3 招聘平台

\- **CryptoJobs**：区块链行业招聘信息

\- **The Ethereum Season of Internships**：以太坊实习季

\- **Ethereum Job Board**：ETH 生态旗舰项目招聘板

\---

\## 2. 智能合约开发模拟面试题

\### Q1. 智能合约开发遵循的范式？

**A1：**

\- **状态机模式**

智能合约本质上是一个状态机，通过交易推动状态迁移。

\- **事件驱动编程**

使用 `Event` 记录关键状态变化，便于前端监听和日志索引。

\- **模块化设计**

通过继承（Inheritance）和库（Library）实现代码复用和解耦。

\---

\### Q2. 常见攻击手段、原理与防护措施？

\#### 1️⃣ 重入攻击（Reentrancy Attack）

**攻击原理：**

1\. 合约向外部地址或合约发送 ETH`call`）

2\. 外部合约在回调中再次调用原合约函数

3\. 原合约尚未更新状态

4\. 攻击者反复进入函数，实现多次提款

**典型代码特征：**

\> ❌ 先转账，后更新状态

**防护措施：**

\- **CEI 模式（Checks → Effects → Interactions）**

\> ✅ 先更新状态，后进行外部调用

\- **重入锁（Reentrancy Guard）**

\- 使用 `bool locked` 作为互斥锁

\- 保证同一时间只能进入一次受保护函数

\---

\#### 2️⃣ 访问控制缺失（Access Control）

**攻击原理：**

\- 攻击者直接调用缺乏权限检查的敏感函数

**防护措施：**

\- 引入显式权限校验（如 `onlyOwner`）

\- 限制关键函数的调用者

**示例：**

\- `deposit()`：所有用户可调用

\- `withdraw()`：仅合约所有者可调用

\---

\#### 3️⃣ 整数溢出（Integer Overflow）

**背景：**

\- Solidity `v0.8.0` 之前默认不检查溢出

\- 数值超出范围会发生 **wrap around**

**攻击原理：**

\- 将变量推到 `2^256 - 1`

\- 再次递增 → 数值回绕到 `0`

\- 绕过条件检查，触发无限奖励逻辑

**防护措施：**

\- 使用 Solidity `>= 0.8.0`

\- 设置业务逻辑上的上限约束

\---

\### Q3. 交易生命周期（Transaction Lifecycle）

\#### 1️⃣ 签名与构造

\- 钱包收集字段：

\- `nonce`

\- `to`

\- `value`

\- `data`

\- `gasLimit`

\- `maxFeePerGas`

\- `priorityFeePerGas`

\- `chainId`

\- 使用私钥生成 `v, r, s`

\- RLP 序列化

\#### 2️⃣ 广播到 P2P 网络

\- 进入本地及邻居节点的 mempool

\- 节点按费用、nonce、gas 规则筛选

\#### 3️⃣ 打包区块

\- 验证者（PoS）选择高收益交易

\- 执行 EVM

\- 生成交易收据（Receipt）

\#### 4️⃣ 共识与传播

\- 区块包含新的 `stateRootreceiptsRoot`

\- PoS 下超过 2/3 验证者签名后 Finalize

\> ≈ 2 Epoch ≈ 64 Slot ≈ ~12 分钟

\#### 5️⃣ 确认数与终局性

\- 前端通常认为 `n ≥ 12` 确认足够安全

\- 最终由 Casper FFG 给出终局性（Finality）

\---

\## 3. Web3 实习计划（冬季）开营仪式回顾

（略）

\---

\## 4. 实践记录

\### 转账记录

\- Etherscan（Sepolia）：

[https://sepolia.etherscan.io/tx/0x60acd9c2ac47e204b3a5ea4fc7b269dbceda81cc24b182d30fc373dd10f73a02](https://sepolia.etherscan.io/tx/0x60acd9c2ac47e204b3a5ea4fc7b269dbceda81cc24b182d30fc373dd10f73a02)

\### NFT Mint

\- 完成 NFT Mint 体验

\---（哈哈哈）

![mfnft.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/chiahao-dev/images/2026-01-12-1768223172894-mfnft.png)

  
  
\## Review

今天是开营第一天，由于时差原因，实际学习内容不多。

计划明天重新规划时间安排，并逐步适应时差。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
