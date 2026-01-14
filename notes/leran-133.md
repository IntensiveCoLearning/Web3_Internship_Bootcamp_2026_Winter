---
timezone: UTC+8
---

# leran-133

**GitHub ID:** leran-133

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
1.3 共识机制对比

机制类型 代表项目 特点 能耗

PoW 比特币 算力竞争，安全性高 高

PoS 以太坊2.0 持币质押，效率提升 低

DPoS EOS 委托投票，速度快 低

PBFT Hyperledger 节点投票，企业适用 中等

二、智能合约深入理解

2.1 智能合约定义

· 自动执行的数字协议：当预设条件满足时自动执行

· 运行在区块链上的代码：部署后无法修改（除非有升级机制）

· 状态可验证：所有执行结果公开可查

2.2 合约生命周期

\`\`\`

开发 → 编译 → 部署 → 调用 → 终止/升级

↓ ↓ ↓

Solidity 上链 Gas支付

代码 transaction

\`\`\`

2.3 基础Solidity语法示例

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract SimpleStorage {

// 状态变量

uint256 private storedData;

// 事件

event ValueChanged(uint256 newValue);

// 构造函数

constructor(uint256 initialValue) {

storedData = initialValue;

}

// 设置函数

function set(uint256 x) public {

storedData = x;

emit ValueChanged(x); // 触发事件

}

// 获取函数（view表示不修改状态）

function get() public view returns (uint256) {

return storedData;

}

}

\`\`\`

2.4 Gas费用理解

· 每笔交易都需要支付Gas

· Gas费用 = Gas用量 × Gas单价

· 优化技巧：

· 减少链上存储

· 使用合适的数据类型

· 避免无限循环

三、Web3生态系统

3.1 Web3架构层次

\`\`\`

┌─────────────────┐

│ DApp层 │ ← 用户交互界面

├─────────────────┤

│ 合约层 │ ← 智能合约逻辑

├─────────────────┤

│ 协议层 │ ← 区块链网络

├─────────────────┤

│ 网络层 │ ← P2P通信

└─────────────────┘

\`\`\`

3.2 常用开发工具

· 开发框架：Hardhat, Truffle, Foundry

· 测试网：Goerli, Sepolia, Mumbai

· 钱包：MetaMask, WalletConnect

· API服务：Infura, Alchemy, QuickNode

· 前端集成：Web3.js, Ethers.js, Wagmi

3.3 交互流程示例

\`\`\`javascript

// 使用ethers.js与合约交互

const { ethers } = require("ethers");

async function interactWithContract() {

// 1. 连接网络

const provider = new ethers.providers.JsonRpcProvider(

"[https://eth-mainnet.alchemyapi.io/v2/YOUR\_KEY](https://eth-mainnet.alchemyapi.io/v2/YOUR_KEY)"

);

// 2. 连接钱包

const signer = new ethers.Wallet("PRIVATE\_KEY", provider);

// 3. 合约实例

const contractABI = \[...\]; // 合约ABI

const contractAddress = "0x...";

const contract = new ethers.Contract(

contractAddress,

contractABI,

signer

);

// 4. 调用合约

const value = await contract.get();

console.log("当前值:", value.toString());

}

\`\`\`

四、实践项目：简易投票合约

4.1 合约设计

\`\`\`solidity

contract SimpleVoting {

struct Candidate {

uint id;

string name;

uint voteCount;

}

mapping(uint => Candidate) public candidates;

mapping(address => bool) public voters;

uint public candidatesCount;

event Voted(uint candidateId, address voter);

constructor(string\[\] memory candidateNames) {

for(uint i = 0; i < candidateNames.length; i++) {

addCandidate(candidateNames\[i\]);

}

}

function addCandidate(string memory name) private {

candidatesCount++;

candidates\[candidatesCount\] = Candidate(

candidatesCount,

name,

0

);

}

function vote(uint candidateId) public {

require(!voters\[msg.sender\], "已投票");

require(candidateId > 0 && candidateId <= candidatesCount, "候选人无效");

voters\[msg.sender\] = true;

candidates\[candidateId\].voteCount++;

emit Voted(candidateId, msg.sender);

}

}

\`\`\`

4.2 测试要点

1\. 功能测试：投票、查询结果

2\. 边界测试：重复投票、无效候选人

3\. Gas估算：不同操作的Gas消耗

五、常见问题与解决方案

问题1：合约部署失败

· 可能原因：Gas不足、构造函数错误、网络拥堵

· 解决方案：增加Gas上限、检查构造函数参数、选择非高峰时段

问题2：交易pending时间过长

· 解决方案：提高Gas价格、使用加速服务、更换RPC节点

问题3：前端无法连接钱包

· 检查步骤：

1\. MetaMask是否安装并解锁

2\. 是否连接正确网络

3\. 网站是否使用HTTPS区块结构：

┌─────────────────┐

│ 区块头(Header) │

│ - 前区块哈希 │

│ - 时间戳 │

│ - Merkle根 │

│ - 难度目标 │

│ - Nonce值 │

├─────────────────┤

│ 交易数据(Body) │

│ - 交易列表 │

│ - 状态变化 │

└─────────────────┘
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

\* 区块链 (Blockchain): 一个公开、无法篡改的“数字账本”，记录了所有交易历史。

\* Web3: 强调“用户拥有数据”的下一代互联网，区块链是它的核心技术基础。

\* NFT (Non-Fungible Token): 一种“独一无二”的数字资产，可以代表艺术品、头像、游戏道具等。

详细笔记：

一、 区块链：

想象一下，有一个由成千上万台电脑共同维护的巨大 Excel 表格（账本）。

1\. 它是什么？

\* 它是一个分布式数据库。数据不存放在某个公司的服务器里（比如腾讯或阿里的服务器），而是同步给网络上的所有参与者。

\* 区块 (Block): 就像账本里的一页纸，记录了一段时间内的交易。

\* 链 (Chain): 每一页（区块）都通过密码学技术链接起来。如果想改第一页的数据，第二页、第三页……后面所有的页都会失效。这保证了数据的不可篡改性。

2\. 关键特征：

\* 去中心化 (Decentralization): 没有中心化的机构（如银行或支付宝）来控制一切。大家共同维护规则，彼此信任代码和算法，而不是信任某个“大老板”。

\* 透明与匿名: 所有的交易记录都是公开的（透明），但交易背后的人是用一串字符（地址）来标识的（匿名）。

3\. 如何运作？

\* 共识机制: 大家怎么决定谁记的账是对的？主要有两种方式：

\* 工作量证明 (PoW): 比拼算力（“挖矿”），像比特币。

\* 权益证明 (PoS): 比拼谁持有的币多、谁更愿意维护网络，像以太坊。

\* 智能合约 (Smart Contract): 存放在区块链上的小程序。它像一个自动售货机，只要满足预设条件（比如“如果A转账给B”），它就会自动执行对应操作（比如“自动把NFT发给A”），不需要第三方介入。

二、 Web3：属于你的互联网

Web1 是“只读”（看报纸），Web2 是“读写”（发微博，但数据归平台），Web3 则是“拥有”（读写+拥有所有权）。

1\. 核心理念:

\* 数据所有权归还用户: 在 Web3 里，你的数字身份、资产（比如你在游戏里买的皮肤）真正属于你，而不是某个平台的数据库里的一行记录。平台倒闭了，你的资产还在链上。

\* 去中心化应用 (DApp): 运行在区块链上的应用程序，比如去中心化交易所、链游等。

2\. 基础设施:

\* 钱包 (Wallet): 这是你进入 Web3 的“钥匙”和“身份证”。它不只存钱，还存你的资产、证明你的身份。最常用的是 MetaMask（小狐狸钱包）。

\* 注意: 钱包的核心是助记词（12或24个单词）。它等于你的全部身家，绝不能泄露，也不能丢失，丢了就永远找不回资产了。

三、 NFT：数字世界的“唯一凭证”

NFT 就是“非同质化代币”。要理解它，先理解“同质化”。

1\. 同质化 vs. 非同质化

\* 同质化 (Fungible): 比如你钱包里的 100 元人民币，和我的 100 元人民币可以互换，它们的价值是相同的。比特币、以太坊也是同质化的，每一个单位都一样。

\* 非同质化 (Non-Fungible): 每一个都是独一无二的，不可互换。比如你的一张独一无二的画作，不能和我的另一张画作直接互换。

2\. NFT 的特性:

\* 唯一性: 每个 NFT 都有一个唯一的 ID，证明它是“这个”。

\* 不可分割: 比特币可以分 0.1 个，但 NFT 通常是一个整体（你不能买半张画）。

\* 确权与溯源: 买了 NFT，就等于在区块链上拥有了这件数字资产的“所有权凭证”。你可以证明你是它的主人，并且能查到它从诞生到现在每一次交易的记录。

3\. NFT 能用来做什么？

\* 数字艺术: 画作、GIF 动图。

\* 头像 (PFP): 像无聊猿（BAYC）这样的头像，既是社交身份，也可能有社区权益。

\* 游戏资产: 游戏里的角色、装备、土地。你在 A 游戏里买的装备，理论上可以带到 B 游戏里用（如果支持）。

\* 门票/会员卡: 参加某个活动的凭证，或某个俱乐部的会员资格。

\* 现实资产映射: 将房产、汽车等现实世界的资产数字化上链。

总结与关系图

你可以这样理解它们的关系：

\* 区块链 是土壤（底层技术，提供信任）。

\* Web3 是森林（基于土壤生长出的生态，即下一代互联网）。

\* NFT 是森林里的一棵特色树（一种重要的应用，代表数字所有权）。
<!-- DAILY_CHECKIN_2026-01-13_END -->
<!-- Content_END -->
