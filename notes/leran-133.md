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
# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->
Solidity 智能合约基础语法笔记

一、基础结构

1\. 许可证声明

\`\`\`solidity

// SPDX-License-Identifier: MIT

· 作用：指定合约的开源许可证类型

· 必须项：所有Solidity文件的第一行都应该是SPDX许可证标识符

2\. 版本声明

\`\`\`solidity

pragma solidity ^0.8.26;

· pragma：编译指示指令

· ^0.8.26：兼容0.8.26及以上版本，但低于0.9.0

· 版本规则：

· ^0.8.26 = >=0.8.26 <0.9.0

· 0.8.26 = 必须是0.8.26版本

· >=0.8.0 <0.9.0 = 指定范围

二、合约定义

\`\`\`solidity

contract Counter {

// 状态变量和函数定义

}

· contract：声明智能合约的关键字

· Counter：合约名称（遵循帕斯卡命名法）

三、状态变量

\`\`\`solidity

uint256 public count;

\`\`\`

变量类型：

· uint256：无符号整数，256位（0 到 2²⁵⁶-1）

· 其他整数类型：

· uint8, uint16, uint32, ..., uint256（8的倍数）

· int8, int16, ..., int256（有符号整数）

· 访问权限：

· public：自动生成getter函数，可在合约内外访问

· private：仅当前合约内可访问

· internal：当前合约及继承合约可访问（默认）

变量默认值：

· uint：0

· bool：false

· address：0x000...（20字节零地址）

四、函数定义

1\. 视图函数（View Function）

\`\`\`solidity

function get() public view returns (uint256) {

return count;

}

\`\`\`

· 特点：

· view：承诺不修改状态（只读取）

· 不消耗gas（如果由外部账户调用）

· 有返回值：returns (uint256)

· 调用方式：可在链下调用，无需交易

2\. 状态修改函数

\`\`\`solidity

function inc() public {

count += 1;

}

· 特点：

· 没有view或pure修饰符

· 修改区块链状态

· 需要交易，消耗gas

· 操作符：+= 是 count = count + 1 的简写

3\. 带有条件检查的函数

\`\`\`solidity

function dec() public {

count -= 1;

}

· 潜在问题：当count = 0时，count -= 1会导致下溢（underflow）

· 解决方案（现代Solidity ≥0.8.0）：

\`\`\`solidity

function dec() public {

require(count > 0, "Counter: cannot decrement below zero");

count -= 1;

}

· 从Solidity 0.8.0开始，算术运算默认检查溢出/下溢

· require()：条件检查，失败则回滚交易

五、函数修饰符详解

1\. 可见性修饰符

· public：任意账户或合约可调用

· private：仅当前合约内部

· internal：当前合约及继承合约

· external：仅能从合约外部调用（this.func()形式可在内部调用）

2\. 状态修饰符

· view：承诺不修改状态

· pure：承诺不读取也不修改状态（只使用参数和局部变量）

· payable：函数可以接收ETH

3\. 示例组合

function get() public view returns (uint256) {

// public: 可外部调用

// view: 不修改状态

// returns: 指定返回值类型

}

六、完整改进版合约示例

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.26;

contract Counter {

// 状态变量

uint256 private \_count;

address public owner;

// 件：记录重要状态变化

event CountChanged(uint256 newCount, address changer);

// 构造函数

constructor() {

owner = msg.sender;

\_count = 0;

}

// 修饰器：仅所有者可调用

modifier onlyOwner() {

require(msg.sender == owner, "Not owner");

\_; // 继续执行函数体

}

// 获取当前值

function get() public view returns (uint256) {

return \_count;

}

// 增加计数

function inc() public {

\_count += 1;

emit CountChanged(\_count, msg.sender);

}

// 减少计数（带安全检查）

function dec() public {

require(\_count > 0, "Cannot decrement below zero");

\_count -= 1;

emit CountChanged(\_count, msg.sender);

}

// 重置计数（仅所有者）

function reset() public onlyOwner {

\_count = 0;

emit CountChanged(\_count, msg.sender);

}

}

\`\`\`

七、关键概念总结

1\. Gas消耗

· 存储操作 > 计算操作 > 读取操作

· view/pure函数在外部调用时不消耗gas

· 状态修改需要交易，消耗gas

2\. 交易与调用

· 交易（Transaction）：改变状态，需要签名，上链

· 调用（Call）：只读操作，立即返回，不上链
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->

以太坊智能合约开发完整指南

一、账户类型对比

对比维度 外部拥有账户 (EOA) 合约账户 (Contract Account)

地址来源 keccak256(pubKey)\[12:\] (公钥→地址) 创建时由 CREATE/CREATE2 计算

控制方式 私钥签名（用户、钱包） 合约代码（EVM 字节码）

状态字段 nonce、balance nonce、balance、codeHash、storageRoot

能否发起交易 ✅ 必须用私钥签名 ❌ 只能由 EOA 触发或合约内部调用

Gas费用支付 由账户本身 ETH 余额承担 由调用者支付

典型场景 钱包地址、热冷账户 ERC-20/721 Token、DeFi协议、DAO

二、Gas 机制

术语 含义 备注

Gas 执行1条EVM指令的抽象工作量单位 汇编级别价格表见 [evm.codes](http://evm.codes)

Gas Limit (Tx) 发送者愿为本笔交易消耗的Gas上限 防止死循环耗尽余额

Gas Used 实际执行指令花费的Gas总和 多退少不补

Base Fee 随区块动态调整的基础费用（EIP-1559） 全网销毁，抑制拍卖狂飙

Priority Fee / Tip 发送者给出以激励打包者的附加费 给矿工/验证者

Max Fee Per Gas maxFee = baseFee + priorityFee 上限 钱包通常自动估算

三、交易生命周期

1\. 签名构造

· 钱包收集字段：nonce, to, value, data, gasLimit, maxFeePerGas, priorityFeePerGas, chainId

· 使用私钥生成 v, r, s 签名 → 序列化 RLP

2\. 广播到 P2P 网络

· 交易进入本地 & 邻居节点的 mempool

· 节点根据 maxFeePerGas、gasLimit、nonce 做基本筛查

3\. 打包/提议区块

· 验证者（PoS）或矿工（PoW时代）挑选利润最高、合法顺序的交易

· 执行 EVM → 产生交易收据（status, gasUsed, logsBloom, logs\[\]）

4\. 区块传播与共识

· 区块头包含新 stateRoot、receiptsRoot

· 超2⁄3质押者签名后在共识层定案（PoS Finality ≈ 2 Epoch ≈ 64 slot ≈ ~12 min）

5\. 确认数 & Finality

· 客户端/前端常以 n ≥ 12 作"概率足够低"确认

· 完全终结在 PoS 下由 Finality Gadget（Casper FFG）给出

四、部署合约到测试网

1\. 测试网选择

名称 共识机制 状态 主要特点 适用场景

Sepolia PoS 活跃 长期支持，与主网最相似，稳定性高 最终部署前测试，生产环境模拟

Holesky PoS 活跃 专为验证者测试设计，大型网络规模 验证者节点测试，质押协议开发

2\. 领取 Sepolia 代币

· 获取地址：MetaMask 切换至 Sepolia 网络

· 申请测试币：通过水龙头（如 [https://sepolia-faucet.pk910.de/](https://sepolia-faucet.pk910.de/)）

· 注意事项：可能需要真人验证、关闭 VPN

3\. Remix 部署到 Sepolia

· 连接钱包：Environment → Injected Provider - MetaMask

· 编译合约：Solidity Compiler → Compile

· 部署合约：Deploy & Run Transactions → Deploy → MetaMask 确认

· 查看结果：Remix 终端显示交易哈希、合约地址

4\. Etherscan 验证

· 查看交易：通过交易哈希查询部署详情

· 查看合约：通过合约地址查看代码、余额、交互记录

· 查看事件：Events 标签页显示合约触发日志

五、区块链前端整合

1\. 交互流程概览

初始化连接 → 用户授权 → 合约实例化 → 函数调用 → 交易签名 → 广播交易 → 状态更新

2\. 关键技术栈

· 合约语言：Solidity

· 交互库：Web3.js / Ethers.js / Viem / Wagmi

· 钱包连接：RainbowKit, ConnectKit, WalletConnect

· 状态管理：React Context

· 错误处理：网络异常、用户拒绝、Gas不足

3\. 实例操作 - 关键代码

连接钱包

javascript

async function connectWallet() {

const accounts = await window.ethereum.request({

method: 'eth\_requestAccounts',

});

web3 = new Web3(window.ethereum);

account = accounts\[0\];

// 验证网络（Sepolia: 11155111）

const chainId = await web3.eth.getChainId();

if (chainId !== 11155111) {

// 网络错误处理

}

}

\`\`\`

合约实例化

\`\`\`javascript

function setContract() {

const address = document.getElementById('contractAddress').value.trim();

// 地址验证

if (!web3.utils.isAddress(address)) return;

// 创建合约实例

contract = new web3.eth.Contract(contractABI, address);

}

\`\`\`

写入操作（需Gas）

\`\`\`javascript

async function leaveMessage() {

const message = document.getElementById('messageInput').value.trim();

try {

const tx = await contract.methods.leaveMessage(message).send({

from: account,

});

console.log('交易哈希:', tx.transactionHash);

} catch (error) {

// 错误处理

}

}

\`\`\`

只读操作（免费）

\`\`\`javascript

async function queryMessages() {

const address = document.getElementById('queryAddress').value.trim();

try {

const count = await contract.methods.getMessageCount(address).call();

for (let i = 0; i < count; i++) {

const message = await contract.methods.getMessage(address, i).call();

// 处理消息

}

} catch (error) {

// 错误处理

}

}

六、高阶内容

1\. Gas 优化技巧

· 减少存储操作：Storage写入（20k gas）成本高，优先使用memory缓存

· 位压缩：将多个变量压缩到 uint256 节省空间

· 循环优化：缓存 array.length 到变量

· 函数可见性：external 比 public 更节省gas

2\. 合约安全

常见漏洞与防护：

· 重入攻击：先更新状态再转账，使用防重入锁

· 预言机操纵：使用 Chainlink 多源验证，TWAP算法

· 整数溢出：Solidity 0.8+ 自动检查，或使用 SafeMath

· 权限控制缺失：使用 onlyOwner / AccessControl 修饰符

· 前置交易/三明治攻击：设置滑点保护，使用防抢跑机制

3\. 智能合约审计

审计工具：

· Slither：静态分析工具，检测安全漏洞

· MythX：云平台安全分析服务

· Foundry：模糊测试框架

审计流程：

静态分析 → 动态测试 → 人工审查 → 报告生成

知名审计机构：

· 慢雾科技：国内领先，注重攻击复现

· OpenZeppelin：社区信赖度高，基础库作者

· ConsenSys Diligence：精通以太坊底层原理

4\. 开发协作规范

GitHub 工作流：

· 主分支 (main)：始终可部署，需测试审查

· 开发分支 (develop)：日常功能开发

· 功能分支 (feature/xxx)：新功能开发

· 修复分支 (fix/xxx)：bug修复

· 发布分支 (release/xxx)：发布准备

提交信息规范：

· 使用简洁明确的提交信息

· 遵循 Conventional Commits 规范

关键要点总结

1\. 账户区别：EOA由私钥控制，可发起交易；合约由代码控制，需被调用

2\. Gas机制：理解Base Fee、Priority Fee、Gas Limit的关系

3\. 测试网部署：Sepolia为主要测试环境，使用水龙头获取测试币

4\. 前端交互：Web3.js/Ethers.js连接钱包，通过ABI与合约交互

5\. 安全优先：注重Gas优化，防范常见安全漏洞，必要时进行审计

6\. 规范开发：遵循Git工作流，使用标准工具和最佳实践
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->


一、Dapp核心概念与架构

1\. 什么是Dapp

定义：Dapp是与传统集中式应用不同的全新应用模式，运行在区块链或分布式网络上。

核心特点：

· 去中心化：逻辑和数据不由单一实体控制，由多个参与者共同维护

· 透明性：所有交易和操作记录在区块链上公开可查

· 不可篡改性：一旦数据上链，无法被单方面修改或删除

· 自主运行：通过智能合约自动执行，无需人工干预

2\. Dapp核心架构组成

（1）前端（User Interface）

· 技术栈：HTML、CSS、JavaScript（React、Vue等框架）

· 交互方式：

· 通过钱包注入的Provider或第三方RPC节点与区块链交互

· 只读调用：通过eth\_call获取合约状态、事件日志等数据

· 状态修改：构造交易调用 → 钱包签名 → RPC节点广播上链

· 钱包集成：集成MetaMask等钱包进行身份验证和交易签名

（2）智能合约（Smart Contracts）

· 作用：定义应用业务逻辑，部署在区块链上执行

· 平台：以太坊等支持智能合约的区块链

· 编程语言：Solidity（以太坊主要语言）

· 执行环境：EVM（Ethereum Virtual Machine）

（3）数据检索器（Indexer）

· 功能：检索智能合约事件日志，写入传统数据库

· 应用场景：

· NFT项目展示用户持有的所有NFT

· 复杂查询需求（区块链原生查询功能有限）

· 技术实现：使用Ponder、Subgraph等框架，TypeScript编写

（4）区块链和去中心化存储

· 区块链：存储智能合约状态数据和交易记录

· 去中心化存储：

· IPFS/Arweave：存储图片、文档等非结构化数据

· 特点：多节点备份、数据持久性、抗审查

二、Dapp开发流程

1\. 需求分析与规划

关键步骤：

1\. 确定功能需求：转账、查询、投票等具体操作

2\. 选择区块链平台：

· 以太坊：生态成熟，开发者多

· Solana：高性能，低费用

· Polygon：以太坊侧链，扩容方案

3\. 设计用户体验：考虑区块链交互的特殊性

2\. 智能合约开发

开发流程：

\`\`\`

编写合约 → 单元测试 → 安全审计 → 优化部署

\`\`\`

安全注意事项：

· 防范重入攻击

· 避免整数溢出

· 权限控制设计

3\. 检索器开发

开发要点：

1\. 确定数据需求：分析前端所需数据结构

2\. 编写检索程序：使用TypeScript编写事件处理逻辑

3\. 部署运维：Docker容器化部署，数据库管理

4\. 前端开发

技术选型：

· 前端框架：React、Vue等

· 区块链交互库：

· Viem（推荐）：现代化，TypeScript支持好

· Ethers.js：成熟稳定

· Wagmi：React Hooks风格

核心功能实现：

· 钱包连接集成

· 区块链数据展示

· 交易签名与确认处理

5\. 部署与上线

部署步骤：

1\. 智能合约部署：

· 使用Hardhat或Foundry

· 先部署测试网（Sepolia、Holesky）

· 主网部署前充分测试

2\. 前端部署：

· 去中心化：IPFS

· 传统方式：Vercel、Netlify

3\. 发布维护：

· 收集用户反馈

· 定期更新修复

三、以太坊开发环境搭建

基础环境准备

1\. 安装Node.js四、开发注意事项

1\. 安全性

· 智能合约必须经过安全审计

· 使用成熟的开发框架和库

· 实施权限控制和访问限制

2\. 用户体验

· 简化钱包连接流程

· 提供交易状态反馈

· 优化Gas费用估计

3\. 成本控制

· 选择合适的数据存储方案

· 优化合约Gas消耗

· 考虑Layer2解决方案

4\. 测试策略

· 单元测试覆盖所有合约功能

· 集成测试模拟真实交互

· 测试网充分测试后再部署主网
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->



一、核心比喻

· Foundry：代码特种兵的战场

· Remix：设施齐全的“全能烹饪教室”

· 浏览器开发：零门槛，无需安装和配置环境

二、界面三大核心区域（驾驶舱）

区域 图标 功能 使用说明

左侧工具栏 前三个图标 核心操作区 只需关注前三个

中间代码区 - 写代码的地方 案板

底部控制台 黑框 系统反馈 看日志

左侧工具栏详解（前三个）

1\. 文件管理器 (第一排图标)

· 新建文件、管理文件夹

2\. Solidity 编译器 (第三排图标，S形插头)

· 检查代码错误

· 编译成机器语言

3\. 部署与运行 (第四排图标，以太坊Logo+播放键)

· 部署合约到链上

· 与合约交互

三、四步生命周期流程

第1步：新建文件 (Create)

1\. 点击文件管理器图标

2\. 打开 contracts 文件夹

3\. 点击小白纸图标 (Create New File)

4\. 输入文件名：Storage.sol

第2步：编写代码 (Code)

solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract SimpleStorage {

uint256 public myNumber; // 存储数字的变量

// 写入函数（花费Gas）

function store(uint256 \_num) public {

myNumber = \_num;

}

// 读取函数（免费）

function retrieve() public view returns (uint256) {

return myNumber;

}

第3步：编译代码 (Compile)

1\. 点击编译器图标

2\. 关键设置：

· Compiler：版本 ≥ 0.8.0（默认即可）

· Auto compile：✔️ 强烈建议勾选（实时检查错误）

3\. 点击蓝色 Compile Storage.sol 按钮

4\. 成功标志：左侧图标出现绿色对钩 ✅

第4步：部署与交互

A. 部署面板设置

设置项 选择 说明

Environment Remix VM (Cancun) 或 London 浏览器内的“假区块链”，速度快，无需真钱

Account 任意测试账户 Remix 提供15个账户，每个含100测试ETH

Contract SimpleStorage 确保选中

B. 部署

1\. 点击橙色 Deploy 按钮

2\. 成功标志：

· 控制台显示绿色文字

· 左侧出现 Deployed Contracts → SimpleStorage

C. 交互

展开 SimpleStorage，看到两个按钮：

1\. store (橙色按钮)

· 输入数字（如888）→ 点击

· 这是写操作，消耗Gas，控制台有交易日志

2\. retrieve (蓝色按钮)

· 直接点击 → 显示 0: uint256: 888

· 这是读操作，瞬间返回结果，免费

四、颜色密码（重要概念）

按钮颜色 含义 是否花费Gas 操作类型

橙色按钮 修改链上数据 是 写操作（交易）

蓝色按钮 读取链上数据 否 读操作（查询）

颜色密码总结

· 橙色 = 花钱办事：修改状态、转账等操作，需要支付Gas费

· 蓝色 = 免费查看：查询数据、获取信息，无需支付费用

· 底层逻辑：以太坊为维护网络，对改变状态的操作收费，查询不收费
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->




Layer2 核心理念

资产锁 L1，交易在 L2 执行，结果提交回 L1 裁决。目标是速度与成本优化，同时兼顾安全与去中心化。

演化主线

从链下计算开始，重点解决两个问题：计算正确性、数据可用性。

通道 → Plasma → Rollup，逐步强化安全性（证明 + 数据上链）。

主要方案

· 状态通道：链上开关与结算，链下高频交易。适合封闭场景，需持续在线。

· Plasma：子链运作，可退出主链，但机制复杂，数据可用性弱，已渐被 Rollup 替代。

· 侧链：独立运行，通过跨链桥连接。成本低但安全性不自带，需额外信任。

· Rollup：当前主流，链下执行，数据与证明提交链上。

Rollup 两种路线

· Optimistic Rollup：乐观假设，设挑战期作欺诈证明。生态成熟、兼容性好，但提现慢。

· ZK Rollup：链上提交有效性证明，确认快、安全性强，但实现复杂、成本较高。

· Validium/Volition：ZK 证明 + 数据可选链下（Validium）或由用户自选（Volition），性能更高但需信任数据可用性。

DAO 本质

基于智能合约与投票机制运行的组织，链上提案、投票、管理资金。透明且去中心化，但决策效率低，易受大户影响。

以太坊节点通信

节点通过 P2P 网络传播交易与区块，无中心服务器。同步步骤：区块头 → 区块数据 → 本地验证。

节点类型

· 轻节点：只存区块头，依赖证明验证，资源占用低，适合移动端。

· 全节点：保存全部区块数据并独立验证，平衡安全与成本。

· 归档节点：存完整历史数据与状态，成本最高，服务于浏览器、审计等专业需求。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->





过去几日总结，一、区块链基础概念

● 定义：区块链是一种去中心化的分布式账本技术，通过密码学链接区块，确保数据不可篡改、全程可追溯。

● 核心特征：

● 去中心化：无单一控制点，数据由网络节点共同维护。

● 透明性：所有交易公开可查（地址匿名，但交易记录透明）。

● 不可篡改性：一旦数据上链，几乎无法修改或删除。

● 关键技术：

● 共识机制：PoW（工作量证明，如比特币）、PoS（权益证明，如以太坊2.0）。

● 智能合约：自动执行的代码协议，是区块链应用的逻辑层（如发行代币、NFT交易）。

● 关键术语：钱包、哈希函数、公私钥、节点、dApp。

二、NFT（非同质化代币）

● 定义：NFT是基于区块链技术的唯一数字凭证，用于记录特定作品（如艺术品、音乐、游戏道具）的所有权。

● 核心特征：

● 唯一性：每个NFT拥有独一无二的Token ID。

● 不可分割性：不可像比特币那样分割（只能拥有完整的一个）。

● 可验证性：所有权和交易历史公开透明。

● 技术标准：

● ERC-721：最主流的NFT标准，每个代币都独一无二。

● ERC-1155：支持半同质化代币，适合游戏道具等场景。

● 核心价值：解决了数字内容的“稀缺性”和“所有权”问题，将数字资产化。

● 注意：购买NFT通常只获得该数字文件的所有权（Token），不等于获得了其版权或知识产权。

三、法律与合规

● 法律属性：NFT通常被认定为“虚拟财产”或“数字资产”，受法律保护，但不具备法定货币地位。

● 核心法律风险：

● 知识产权侵权：未经授权将他人作品铸造成NFT（盗版）。

● 金融化风险：炒作、价格操纵、承诺回报，可能涉嫌非法集资或证券违规。

● 洗钱风险：利用NFT交易进行资金清洗。

● 合规要点：

● 版权审查：发行前必须获得明确的授权（版权+改编权）。

● 禁止二级市场炒作：许多合规平台禁止或限制NFT的二次交易，以防止金融化。

● 用户告知：必须明确告知用户购买的是使用权还是所有权，以及相关风险。

● 关键原则：“NFT所有权 ≠ 作品版权”。

四、核心金句/要点回顾

● 区块链是信任的机器，NFT是数字所有权的凭证。

● NFT让数字世界第一次有了“稀缺”和“唯一”。

● 在NFT世界里，你买的是一张“数字证书”，而不是“版权证书”。

● 合规是NFT行业发展的生命线，防止金融化是关键红线。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->






今日学习，安全与合规，根据我国最新出台法律，对加密货币有严格限制，加密货币行业虽然先进且方便，但是充斥着不确定与危险性，空投项目，挖矿项目等等都被严格限制，我们作为技术人员尽量也不要参加相关项目的开发，哪怕是写代码也难逃法律责任，更不用说教唆人们参加或者自己参加了，我们应该提前预防了解哪些行为可能造成违法，因为我们有时候出于对钱财的渴望会相信一些东西，看似不违法但其实有很大风险，交易对手如果涉嫌洗钱和非法经营给我们转帐，那我们甚至有可能被卷入协助非法洗钱的罪名，虚拟货币兑换一定要对对方的背景信息，钱财来源进行审核，并且虚拟货币在我国不被法律承认，涉及虚拟货币的纠纷可能不被法院受理，我们要注意，合同可能无效，我们要尽量在合同签前多思考，不让自己利益受损，同时全球虚拟货币行业也在提出更多监管，正在让虚拟货币不断合规化，虚拟货币的风险被监管体系脱离传统金融体系，我认为虚拟货币虽然具有交易属性，但上层希望让其作为商品，而非主流交易工具。

之后，我们来讨论新型雇佣关系，1.区块链行业许多项目无法在国内注册公司，这时我们如果入职，我们将不受基本劳动法的保障，更多时候采用委托国内公司雇佣，总之关注社保和公积金结构，要能享受到社会保障服务，2.既然是虚拟货币公司，有的公司工资结构中会有虚拟货币，出金是最主要转换手段，在这之中我们还是要关注交易对手的资金来源，活动，以免陷入违法指控，可以与公司协商薪资结构，此外小心自发Token，这种代币不是主流，波动性和风险极大，项目结束后有可能失去价值，

最后，讨论常见的攻击方式和钓鱼盘

总之，这个行业的人技术性很高，骗人手段丰富，很可能通过文件下载植入木马病毒，伪装官方页面点入后植入病毒，盗取信息，我们需要做的就是，不随便信任加你的陌生人，不随便相信带你赚大钱的人，天下没有这等好事，不随便下载文件，可以通过下图来验证，不访问非官方网站，保护好自己的邮箱和钱包，密码定时更换防止泄漏

![IMG_4431.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/leran-133/images/2026-01-15-1768483712319-IMG_4431.png)
<!-- DAILY_CHECKIN_2026-01-15_END -->

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
