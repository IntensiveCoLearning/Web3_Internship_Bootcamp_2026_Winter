---
timezone: UTC+8
---

# QingQiuGeek

**GitHub ID:** QingQiuGeek

**Telegram:** 

## Self-introduction

河南科技大学软件工程专业，大四，已有两段Java后端实习，计划转web3。对web3、ai感兴趣，掌握JavaWeb、docker、linux、数据库等常见后端开发技能以及react、js、ts、next.js基础前端知识，有独立开发全栈web项目的经验，对ai agent、mcp也有一定了解。

## Notes

<!-- Content_START -->
# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->
## EIP

Ethereum Improvement Proposal（以太坊改进提案），EIP 是向以太坊社区提出新功能、流程或标准的正式文档。任何重大变更（如协议升级、新操作码、费用机制）都必须先提交 EIP。

![](image.png)

## ERC

Ethereum Request for Comments（以太坊征求意见稿），ERC 是针对智能合约和 DApp 开发者制定的应用层标准，确保不同项目之间的互操作性，是 EIP 的的一个子类，每个 ERC 都是 EIP，但不是每个 EIP 都是 ERC。

![](image-5.png)

### ERC-20 万链之基

ERC-20 是所有代币的“通用语言”，它定义了一套**所有代币都必须遵循的规则**，就像所有手机都必须有充电口一样。这使得任何支持 ERC-20 的交易所、钱包或 DApp 都能识别和处理任何 ERC-20 代币，**极大地促进了生态的互联互通。**

核心接口：

![](image-2.png)

### ERC-721 数字资产的确权标准

如果说 ERC-20 是“钱”，那么 ERC-721 就是“房产证”或“收藏品证书”。它为每一个代币（NFT）赋予一个独一无二的 ID，确保其不可复制、不可分割、不可替代。这使得它成为数字艺术、游戏道具、虚拟土地等独特资产的理想载体。

核心接口：

![](image-3.png)

### ERC-1155 半同质化资产的终极方案

ERC-1155 是 ERC-20 和 ERC-721 的“超级融合体”。它允许**在一个智能合约中同时管理多种类型的资产**，既可以是同质化的代币（如游戏金币），也可以是非同质化的 NFT（如稀有装备）。这极大地简化了开发流程，降低了 Gas 费用，是游戏和复杂应用的首选标准。

![](image-4.png)

### ERC-2612 签名授权机制

在 DeFi 世界里，用户每次转账或授权代币，都需要支付 Gas 费并进行一次链上交易。ERC-2612 引入“签名授权”机制，通过在合约中添加一个 permit 函数，允许用户在链下生成一个签名，然后在链上提交这个签名和相关参数，合约会验证签名的有效性，从而完成授权操作，即可在链上多次使用，极大地节省了 Gas 费用和操作步骤，提升了用户体验。但签名授权也意味着用户需要对签名内容有充分信任，一旦签名被恶意利用，可能导致资产损失。

### ERC-777 自动触发的 Hook

ERC-777 是 ERC-20 的升级版，它引入了“Hook”（钩子）机制。这意味着在代币转账或接收时，合约可以**自动触发一些预设的代码逻辑**，比如自动将部分代币转入保险库，或者在接收代币时自动执行某些操作。

**如果一个合约在接收代币时触发了另一个合约的函数，而这个函数又试图再次调用原合约**，就可能造成“重入攻击”，导致资产被盗，这个“钩子函数” 却成了黑客重入攻击的温床，它的落幕告诉我们：在 Web3，安全和可控永远优先于优雅。

### ERC-4626 收益金库的统一规范

比如你有多个银行账户，但每个银行的存取款流程、ATM 机、手机 App 都完全不同。每次操作都要重新学习，非常麻烦。ERC-4626 就是为了解决这个问题而生的，它为所有 DeFi 收益金库提供了一个统一的接口标准。无论是哪个协议的金库，用户都可以用同样的方式存入、取出、查看收益，极大地简化了操作流程，提高了互操作性。

核心接口：

-   deposit：用户向金库存入“底层资产”如 USDC、ETH，金库会按当前汇率，返回给用户等值的“份额代币”。就像把现金存进银行，银行给一张存单。
    
-   withdraw：用户用“份额代币”去金库按当前汇率兑换回“底层资 产”。就像拿着存单去银行取现金。举例：你对兑换所说：“你要 100 个金币”。兑换所会告诉你：“好的，你需要交回 95 张兑换券。” 你交回 95 张券，拿到 100 个金币。
    
-   redeem：用户用“份额代币”去金库“赎回”底层资产。这是 withdraw 的反向操作，逻辑相同。举例：你对兑换所说：“我要把这 100 张兑换券兑成金币”。兑换所会告诉你：“好的，你将得到 105 个金币。” 你交回 100 张券，拿到 105 个金币。
    
-   mint：用户输入“份额代币”，向金库“铸造”新的份额。这在某些特定场景下（如批量操作）会更高效。
    

redeem 和 withdraw 看起来有些像，但是前者是兑掉多少存单，后者是兑换多少资产！

由于金库合约可能被闪电贷攻击，因此需要\*\*结合时间加权平均价格（TWAP）\*\*等机制来防范价格操纵。

闪电贷攻击流程：

-   第一步：借入巨资 – 攻击者通过闪电贷瞬间借入大量的资产作为操纵市场的“弹药”。
    
-   第二步：扭曲汇率 – 在极短时间内用这笔巨资在 DEX 进行大额交易，人为拉高或压低目标资产的价格。
    
-   第三步：套利收割 – 利用被操纵的虚假高价，向金库存入资产以获取更多份额，或在低价时赎回，完成套利并在\*\*同一笔交易（以太坊出块时间是 12 秒）\*\*中归还闪电贷。如果没有按时归还闪电贷，则整个交易原子性回滚，债务从未发生，但消耗的 gas 费不退还。
    

整个过程依赖的就是**金库合约对瞬时价格的“信任”**，时间加权平均价格的核心思想是**不相信任何一个瞬间的价格，而是通过计算资产在一段较长时间窗口内的平均价格，来平滑掉由闪电贷等操作引起的瞬时剧烈波动。**

时间加权平均：系统会记录每个区块的时间戳和对应的价格，将每个价格乘以其持续的时间长度，再将所有加权值相加后除以总时长，得到最终价格。公式：TWAP = Σ(价格\_i × 时间间隔\_i) / 总时间间隔

这意味着攻击者若想成功操纵 TWAP 价格，必须在一段较长的时间内维持异常价格，这需要很大的资金成本，使得攻击在经济上几乎不可行。

### ERC-4337 账户、私钥的抽象

在传统 Web3 中，**私钥即账户**，用户必须管理一个私钥，私钥一旦丢失或被盗，资产就无法找回。而 ERC-4337 把“账户”和“私钥”这两个概念分离开，让用户**不再需要直接管理私钥**。用户看到的是一个“账户”，它拥有资产、可以执行交易，但这个账户的底层逻辑是由一个智能合约（而不是一个私钥）来控制的。这个智能合约可以被编程，实现各种高级功能，比如：

-   社交恢复：用户丢失了“登录凭证”，可以通过预先设置的“好友”或“机构”来帮助恢复账户，就像忘记密码时可以通过邮箱或手机找回一样。
    
-   Gas 代付：用户可以授权一个“Gas 代付者”（比如一个 DApp 或一个朋友）来为自己的交易支付 Gas 费。
    
-   多签：一个账户的交易需要多个“签名者”同意才能执行，非常适合公司、DAO 或家庭共同管理资产的场景，极大地提升了安全性。
    

### ERC-2981 版税机制给创作者的持续收益

ERC-2981 为 NFT 引入了**版税机制**。核心函数是 royaltyInfo 函数，让 NFT 合约能够返回创作者的地址和版税比例，这使得 NFT 市场平台可以自动计算并支付版税，无需创作者手动干预。当一个 NFT 在二级市场被转售时，创作者可以自动获得一定比例的收益。这为创作者提供了一个可持续的收入来源，激励他们创作更多优质内容。

### ERC-6551 NFT 独立的合约地址

ERC-6551 让每个 NFT 都拥有一个独立的合约地址，这个地址可以像普通钱包一样持有资产、执行交易。这使得 NFT 不再只是一个“图片”，而是一个可以拥有“装备”、“积分”、“凭证”的“角色”，极大地拓展了 NFT 的应用场景。

ERC-6551 的核心是“Token Bound Accounts”，它通过一个**注册表来管理 NFT 与合约地址的绑定关系**。每个 NFT 都可以绑定一个独立的合约地址，这个地址可以持有 ERC-20、ERC-721、ERC-1155 等资产，也可以执行任意的智能合约逻辑，使得 NFT 真正“活”了起来。

### ERC-3643 RWA 的合规之门(原 T-REX)

ERC-3643（原 T-REX）是为现实世界资产（RWA）设计的**合规化证券代币标准**。核心功能是“KYC 验证、转账白名单/黑名单、监管冻结”，确保代币的发行和交易符合监管要求，使得机构投资者可以放心地将资金投入 RWA 项目，推动了 Web3 与传统金融的融合。

### ERC-4907 可租赁 NFT 的基石

ERC-4907 是对 ERC-721 的扩展，它引入了“使用者（User）”角色，允许 NFT 的所有权与使用权分离。这一机制使得 NFT 可以被租赁或借用，而无需转移所有权，从而极大地提升了资产的流动性和实用性。

ERC-4907 通过两个关键属性实现租赁功能：

-   user：代表被授权使用 NFT 的地址。
    
-   expires：一个时间戳，表示 user 角色的授权何时自动失效。
    

### ERC-1271 智能合约钱包的签名验证标准

ERC-1271 定义了一个标准化的函数 isValidSignature(bytes32 \_messageHash, bytes \_signature) ，智能合约钱包通过实现此函数来验证签名。当 DApp 需要验证一个智能合约钱包的签名时，它会首先检查该地址是否实现了 ERC-1271 接口。如果实现则调用 isValidSignature 函数进行验证，验证有效则返回一个预定义的值 0x1626ba7e；如果没有实现 1271 接口，则退回到传统的 ecrecover 方法。
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->


````markdown

Hardhat 和 Foundry 是当前最主流的两个智能合约开发框架,都致力于简化从编码、测试到部署的完整开发流程。

Hardhat 基于 JavaScript/TypeScript 生态，用 js 或 ts 写测试，依赖 nodejs 环境，需要配置驱动 hardhat.config.js，将 Solidity 开发无缝融入 JavaScript 生态，特别适合全栈开发者（前端+合约）。。

Foundry 基于 Rust（性能相对 nodejs 快很多），可用 Solidity 写测试，测试即合约，消除语言边界，并且约定优于配置：目录结构即配置（src/ → 合约, test/ → 测试）。

## Foundry

```
┌─────────────────────────────────────────────────────┐
│           Foundry 四大核心组件（Rust 实现）          │
├───────────┬───────────┬───────────┬─────────────────┤
│  forge    │   cast    │  anvil    │    chisel       │
│ (编译/测试)│ (链交互)  │ (本地链)  │  (REPL 调试)    │
└─────┬─────┴─────┬─────┴─────┬─────┴────────┬────────┘
      │           │           │              │
      ▼           ▼           ▼              ▼
  编译合约    读写链状态   提供测试环境   快速验证逻辑
  运行测试    发送交易     分叉主网       实时计算
  部署合约    解码事件     模拟攻击        调试 Gas
```

### forge

test/ 目录下的 .t.sol 文件本质是继承 Test.sol 的合约，控制台执行 forge test 命令会：

- 编译所有合约（含测试合约）
- 部署测试合约到本地 EVM
- 调用以 test 开头的函数（如 testIncrement）进行测试
- 捕获断言失败/Gas 消耗/覆盖率

同时还有模糊测试内置，函数参数带 (uint256 x) 会自动运行 256 次随机输入，无需额外配置。

```c
test/Counter.t.sol

pragma solidity ^0.8.20;
import "forge-std/Test.sol";
import "../src/Counter.sol";

// 测试合约本质是普通合约 + 特殊断言库
contract CounterTest is Test {  // Test 来自 forge-std
    function testFuzz_Increment(uint256 start) public {
        Counter c = new Counter();
        c.setNumber(start);
        c.increment();
        assertEq(c.number(), start + 1); // 断言在 EVM 中执行
    }
}
```

### cast

cast 无需写脚本就可以和任意 EVM 链交互

```shell
# 查询指定地址在以太坊主网上的 ETH 余额（单位：wei）
cast balance 0x... --rpc-url https://eth.llamarpc.com
# 调用某个合约地址的只读函数（view/pure），无需支付 Gas，不改变链上状态
cast call 0x... "name()" --rpc-url sepolia
# 广播交易到区块链，调用可修改状态的函数（如转账），需支付 Gas，--private-key $PK是发送方私钥（极度敏感！）
cast send 0x... "transfer(address,uint256)" 0x... 100 --private-key $PK --rpc-url sepolia
```

cast 本质是 命令行版的 Web3.js，但用 Rust 实现，速度极快且无 JS 依赖。

### anvil

快速启动本地以太坊节点

- anvil --fork-url https://eth.llamarpc.com 瞬间复制主网状态，测试真实合约交互
- 启动即提供 10 个带 10,000 ETH 的测试账户（私钥固定，方便调试）
- 测试中用 vm.warp(timestamp) 操控区块时间（无需重启节点）

### chisel

REPL（Read-Eval-Print Loop）环境，实时验证 Solidity 逻辑

```shell
chisel
>> uint256 x = 100
>> x * 2
200
>> address(this).balance  # 查看当前合约余额
0
```

## hardhat

```
┌──────────────────────────────────────────────────────────────┐
│          Hardhat 核心架构（Node.js + 插件系统）               │
├──────────────┬──────────────┬──────────────┬─────────────────┤
│  Hardhat     │ Hardhat      │  插件系统     │  开发者工具链    │
│  Core        │  Network     │ (Plugins)    │ (VS Code etc.)  │
│ (任务调度)    │ (本地EVM)     │              │                 │
└──────┬───────┴──────┬───────┴──────┬───────┴────────┬────────┘
       │              │              │                │
       ▼              ▼              ▼                ▼
   编译合约      模拟区块链     扩展功能：        深度集成：
   运行测试      捕获堆栈       部署/验证/        调试器/测试覆盖率
   执行脚本      时间旅行       类型生成          实时错误提示
```

### Hardhat 测试（TypeScript）

foundry 使用 solidity 编写测试，而 hardhat 使用 js/ts 编写测试，如：

```js
it('should transfer tokens', async () => {
	await token.transfer(user.address, amount);
	expect(await token.balanceOf(user.address)).to.equal(amount);

	// 复杂逻辑示例：循环测试 100 次不同金额
	for (let i = 0; i < 100; i++) {
		await token.transfer(user.address, ethers.utils.parseEther(i.toString()));
		// ... 验证逻辑
	}
});
```

### hardhat.config.ts 配置中心

```js
import { HardhatUserConfig } from 'hardhat/config';
import '@nomicfoundation/hardhat-toolbox';

const config: HardhatUserConfig = {
	solidity: {
		version: '0.8.20',
		settings: {
			optimizer: {
				enabled: true,
				runs: 200, // Windows 编译速度优化关键参数
			},
		},
	},
	networks: {
		// Windows 路径注意事项：使用正斜杠避免转义问题
		hardhat: {
			chainId: 1337,
			// 预设账户（Windows 开发常用）
			accounts: [
				{
					privateKey:
						'0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80',
					balance: '10000000000000000000000', // 10,000 ETH
				},
			],
		},
		sepolia: {
			url: process.env.SEPOLIA_RPC_URL || '',
			accounts: [process.env.PRIVATE_KEY || ''],
		},
	},
	// Windows 路径安全：输出目录使用正斜杠
	paths: {
		sources: './contracts',
		tests: './test',
		cache: './cache',
		artifacts: './artifacts', // 生成的 ABI/字节码存放处
	},
};

export default config;
```

```
# 1. Core 编译合约（触发 solidity 任务）
npx hardhat compile

# 2. Network 启动本地链（自动触发）
npx hardhat test  # 内部自动启动 hardhat network

# 3. 插件提供能力
- hardhat-ethers: 部署合约
- typechain: 生成 TS 类型
- chai-matchers: 断言事件
- hardhat-gas-reporter: 测试时显示 Gas 消耗
- hardhat-deploy: 部署脚本管理 + 多网络配置


```
````
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->




* * *

[以太坊节点、客户端](https://ethereum.org/zh/developers/docs/nodes-and-clients/#execution-clients)

“节点”是指任何以太坊客户端软件的实例，它连接到其他也运行以太坊软件的计算机，形成一个网络。 **一个节点需要运行两种客户端软件：共识客户端和执行客户端**。

-   执行客户端（也称为执行引擎、EL 客户端或旧称“以太坊 1”客户端）侦听网络中广播的新交易，并在 EVM 中执行，并保存所有当前以太坊数据的最新状态和数据库。
    
-   共识客户端（也称为信标节点、CL 客户端或旧称“以太坊 2”客户端）**实现权益证明共识算法**，**使网络能够根据来自执行客户端的经验证数据达成一致**。 此外还有名为“验证者”的第三种软件，它们可被添加到共识客户端中，使节点能参与保护网络安全。节点分布可以看 [Etherscan 节点地图](https://etherscan.io/nodetracker)
    
    [以太坊节点](https://ethernodes.org/)
    

## 节点类型

客户端可以运行三种类型的节点：轻节点、全节点和归档节点。

### 轻节点

轻节点只下载区块头，不会下载每个区块。这些区块头包含区块内容的摘要信息。轻节点会向全节点请求其所需的任何其他信息。然后轻节点可以根据区块头中的状态根独自验证收到的数据。

轻节点可以让用户加入以太坊网络，无需运行全节点所需的功能强大的硬件或高带宽。**轻节点不参与共识（不能成为矿工/验证者）**，但可以访问功能和安全保障和全节点相同的以太坊区块链。

### 全节点

**全节点对区块链进行逐块验证**，包括下载和验证每个块的块体和状态数据。有些全节点从创世区块开始，验证区块链整个历史中的每一个区块（完全同步）。有些全节点则从更近期的区块开始验证（比如 Geth 的**快照同步**），而且它们信任这些区块是有效的。

但是全节点并不保存全部区块链数据，而是只保存相对较新的数据，那么又如何逐块验证呢？

全节点会同步历史数据。如上所说，如果从创世开始验证，那么**全节点会从创世区块开始**，逐块下载、执行并验证每一笔交易，从头构建完整的状态树，这是最彻底、最慢的方式，但能获得 100% 自验证的信任。

如果从近期区块开始验证，则会从某个最近的区块开始，逐块下载、执行并验证每一笔交易，这种方式显然更快，但是要 100%信任逐块下载的初始区块。

在逐块验证后，会进行**修剪**。区块链的“状态”是指所有账户余额、合约代码和存储变量的**当前值**。当执行交易时，会产生大量**中间状态**。修剪机制会安全地删除那些不再被最新区块引用的旧状态数据，只保留从创世状态推导出最新状态所必需的“状态树”路径上的关键节点。

### 归档节点

归档节点从创世块开始验证每个区块的全节点，存储全节点中保存的所有内容不删除任何下载的数据，并建立历史状态存档。

以归档以外的任何方式同步客户端将导致区块链数据被修剪。**这意味着不存在包含所有历史状态的归档，但全节点能够在需要时构建它们。**

## 信标链

信标链（Beacon Chain）是以太坊 2.0 的核心升级，它不处理日常交易，而是作为整个网络的“中央调度员”，负责管理验证者、分配共识任务，并协调未来的分片链。

## 同步模式

### 完全同步

完全同步会下载所有区块（包括区块头和区块体），并通过执行自创世块以来的每个区块，以增量方式重新生成区块链的状态。

### 快速同步

与完全同步一样，快速同步会下载所有区块（包括区块头、交易和收据）。 不过，快速同步并不重新处理历史交易，而是依赖**收据**，直至到达最近的头部时，再切换到导入和处理区块，以提供一个完整的节点。

收据（Receipt）是交易被成功打包进区块并执行完毕后生成的“官方凭证”，它记录了交易的交易哈希、执行结果、状态变化、消耗 Gas、状态跟等信息，是快速同步机制中信任历史交易的关键依据。

### 快照同步

快照同步从一个最近的已知为真实区块链一部分的受信任检查点开始。节点会定期保存检查点，同时删除早于某个时间的数据。这些快照被用于在需要时重新生成状态数据，而不是永久储存它。

### 轻量同步

轻客户端模式下载所有区块头和区块数据，并对其中一些进行随机验证。 仅从可信的检查点同步链的头部。

## 共识层同步模式

### 乐观同步

乐观同步是一种合并后同步策略，专为选择加入和向后兼容而设计，允许执行节点通过已确立的方法进行同步。

执行引擎可以**在不进行完全验证的情况下乐观地导入信标区块**，找到最新区块头，然后使用上述方法开始同步链。接着在执行客户端更新之后通知共识客户端信标链中交易的有效性。

### 检查点同步

检查点同步也称为**弱主观性同步”**，它基于弱主观性假设，从最近的弱主观性检查点而不是从创世块同步信标链。检查点同步可大幅加快初始同步速度，意味着节点会连接到远程服务，以下载最近的最终确定状态并从该点继续验证数据。**提供数据的第三方会受到信任，因此要谨慎选择。**

## 执行客户端

-   Go Ethereum：简称 Geth，Go 语言实现，以太坊协议的原始实现之一。 目前，它是使用最为广泛的客户端，拥有最大的用户群，为用户和开发者提供各种工具。
    
-   Erigon：Go 实现，以前称为 Turbo‐Geth，最初是 Go Ethereum 的一个分叉，注重速度和磁盘空间效率。
    
-   Nethermind：C#、.NET 实现。
    
-   Besu：Java 实现，企业级以太坊客户端，面向公共网络和许可网络。 它运行所有以太坊主网功能（从追踪到 GraphQL），可进行大范围监控。
    
-   Reth：Rust 实现，以太坊全节点。
    
-   EthereumJS：TypeScript 实现。
    

## 共识客户端

-   Lighthous：Rust
    
-   Grandine：Rust
    
-   Lodestar：TypeScript
    
-   Nimbus：Nim
    
-   Teku：Java
    
-   Prysm： Go
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->






## 从账本到状态机

我们通常用“分布式账本”的类比来描述像比特币这样的区块链，它使用密码学的基本工具来实现去中心化的货币。而以太坊也有自己的本土加密货币并同样遵循着**分布式账本规则**，但同时它也支持更强大的功能“智能合约”，这也是以太坊可编程的关键，因此以太坊除了是一个分布式账本之外，还是一个状态机器，可以根据预定义的规则（智能合约）在不同区块之间更改。

![alt text](image.png)

## EVM 如何工作

**EVM 运行在以太坊的每个节点上**，当部署一个智能合约时，合约代码会被编译成字节码，然后通过网络广播到所有节点。EVM 在每个节点上执行这些字节码，确保所有节点对交易和合约状态达成共识。

和 Java 的 JVM 不同，EVM 不是一个需要单独下载的独立软件，而是**嵌入在以太坊客户端中的运行时环境**。开发者可以通过以下方式获取 EVM 环境：

-   安装以太坊客户端 ：如 ethereumjs-vm（js 实现）、evnone（c++实现）、revm（rust 实现） 或 Py-EVM（python 实现），这些客户端都内置了 EVM。
    
-   使用开发框架 ：如 Hardhat 或 Truffle，它们在本地启动一个模拟的 EVM 环境，方便开发和测试。
    
-   在线 IDE ：如 Remix IDE，无需本地安装，直接通过浏览器连接到远程 EVM 实例。
    

比如用 Solidity 编写的合约，.sol 编译为 EVM 字节码（十六进制操作码）和 ABI 应用二进制接口。每个操作码执行会消耗燃料，具体看[EVM 操作码](https://ethereum.org/zh/developers/docs/evm/opcodes/)
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->








打卡
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->










## 自动做市商

自动做市商 Automated Market Maker，简称**AMM**，它是一种**部署在区块链上的智能合约协议**，通过数学算法（定价曲线），允许用户在无需传统订单簿和特定交易对手的情况下，直接与流动性池进行代币兑换，为代币提供流动性。

可以把 AMM 想象成一个永不停歇的“自动报价机”：它根据预设的数学公式（如 x \* y = k）实时计算两种资产间的兑换价格。**用户存入资产构成“资金池”**，交易者直接与这个池子交互，用 A 资产换取 B 资产，整个过程由代码自动执行，无需等待买家或卖家匹配。

用户存入资产就是 Liquidity ProVider，即**流动性提供者**，简称 LP，他们不靠报价差赚钱，而是**通过向交易池存入资金来赚取手续费**。

**AMM 是一个统称**，它包含多种数学模型，比如 uniswap V2 的恒定乘积 x\*y=k，稳定币的恒定总和 x+y=k。

## 无常损失

无常损失（Impermanent Loss, IL）是 DeFi 中 LP 面临的核心风险。它并非实际亏损，而是指将资产存入流动性池后，相较于直接持有这些资产，可能获得的价值的损失。

假设此时 ETH 价格为 $1000 /ETH，一个 LP 向 Uniswap V2 的 ETH/USDC 池存入价值 1000 美元的 ETH 和 1000 美元的 USDC，那么就是存入了 1 ETH 和 1000 U，恒定乘积 k = 1 \* 1000 = 1000，假设他存入的资金占据池子 50%的份额。

当 ETH 翻倍至 $2000，套利者发现池内 ETH 价格低于市场价，于是用市场价 $2000 的 USDC 不断从池中买入便宜的 ETH。设新的 ETH 数量为 x，USDC 数量为 y，则新价格: $2000 = y / x => y = 2000x，联立 x\*y=1000，得出池子新状态 0.7071 ETH / 1414.21 USDC。

此时 LP 的资产是：

-   新 ETH 数量: 0.7071 \* 50% ≈ 0.3536 ETH
    
-   新 USDC 数量: 1414.21 \* 50% ≈ 707.11 USDC
    
-   当前价值: (0.3536 \* 2000)+707.11=707.20 + 707.11=$1414.31
    

如果 LP 没有存入 Uniswap 提供流动性，而是直接持有 1 ETH 和 1000 USDC，则当前会有$3000，无常损失 = 直接持有价值 - LP 价值 = 3000−1414.31 = $1585.69，损失率 = (1585.69/3000) \* 100% ≈ 52.86%

简单来说就是套利者从交易池子中买低价的 ETH，再以正常价格卖出获利，导致 LP 资产损失.

## 从 UniSwap V1 到 V4

币安 / OKX 是中心化交易所 CEX，而 Uniswap 是去中心化交易 DEX，Uniswap 基于智能合约自动运行，无需注册，资产自托管，就像一个自动售货机，交易者可以自由进行代币兑换。

### UniSwap V1

**奠定了恒积做市商 x\*y=k 的基础**，缺点是所有代币只能跟 ETH 配对，买 A 代币得先换成 ETH。

### UniSwap V2

V2 增加了**ERC-20 自由配对**，ERC-20 是以太坊上创建和发行代币的技术标准，规定了代币转账、查询余额等基本功能。它就像代币的“通用接口”，**任何两个符合 ERC-20 标准的代币都可以直接创建一个交易对池子，无需许可，让代币直接对换，不再强制经过 ETH**。

此时 UniSwap V2 的 LP 的资产均匀分布在整个价格区间 （从 0 到无穷大），还无法像 V3 那样选择特定的价格区间，可以说是大水漫灌，**价格控制被动，价格完全由市场交易和公式决定**！

V2 还引加入了**闪电贷 (Flash Loan)**。闪电贷是一种无需抵押、但必须在同一笔区块链交易内归还的贷款。核心是利用区块链交易的“原子性”（要么全部成功，要么全部回滚）。如果借款人在交易结束前未能连本带利归还，整个交易无效，贷款从未发生。

### UniSwap V3

在 V2 中，LP 提供的流动性会均匀分布在整个价格曲线（0 到无穷大）上，但大部分流动性可能永远用不到，而 V3 **允许 LP 自定义价格区间**，实现精准滴灌，这样可以让资金集中在交易活跃区，在自定义的价格区间内提供集中流动性。

比如如果我认为 ETH 价格会在 1800-2200 美元之间波动，那么我就可以只在这个区间投入资金，资金不会被均匀分布在整个价格曲线，定位更精准，流动性更高。

### UniSwap V4

保留了 V3 的集中流动性核心，从“自动化做市商”向“可编程流动性平台 ”的范式转变. V4 新增了 **Hooks**，允许开发者在流动性池生命周期的关键节点（如初始化、添加/移除流动性、交易前后）**注入自定义的智能合约逻辑**，将 Uniswap 从一个固定的交易协议，转变为一个可组合的开发者平台。

> 在 V3 及更早版本中，**每个流动性池都是一个独立的智能合约**，创建和交互成本高昂。V4 采用了**单例设计，将所有池子集中在一个合约中管理**。 配合闪电记账（Flash Accounting）机制，交易过程中代币的转移被简化为合约内部的余额记录，**仅在交易最终结算时进行一次外部转账**。这使得创建新池的成本降低，跨池交易的 Gas 费用也大幅减少。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->












昨天学习了solidity基础语法，因为有java底子，而且solidity没有java那些web框架，所以学起来很快。另外solidity through walk 分享课上，老师说solidity写合约，一般一个合约几百行代码，代码量不大，部署上链就ok了，所以但看solidity合约开发岗位，需求量并不多，更多的是安全合规和漏洞审查以及全栈开发，全栈用nodejs+ts\\js，前后端通用，技术栈一样。今天做了以下事情：

1、休闲黑客松案例拆解

2、参加Dapp Workshop

3、参加南塘 Dao 主题

4、参加uniswap工作原理解析
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->














\## 概念

在 Solidity 中，\*\*状态（state）\*\*指合约中所有存储在区块链上的变量（即状态变量，如 uint balance;、address owner;等）

solidity 有一些俗成的约定，会使用下划线\\\_区分状态变量和局部变量或者函数是否应该被外部调用，比如构造函数内部；

\`\`\`c  
contract MyContract {  
uint256 private \_value;

constructor(uint256 value) {  
\_value = value; // 将构造函数参数赋值给状态变量  
}  
}  
\`\`\`

\## 数据类型

1\. 值类型

\- bool：true | false

\- int、uint：整数类型，后面可以加数字，步长是 8，从 int8、uint8 到 int256、uint256，省略数字则默认 256。

\- address：地址类型，仅能存储 20 字节的地址。

\- address payable：可接收 ETH，有 .transfer() 和 .send() 方法

\`\`\`c  
address addr = 0xAbC…;  
address payable payableAddr = payable(addr); // 转换为 payable

addr.balance 该地址的ETH余额，单位是wei

addr.code 合约字节码（bytes memory），若为 EOA 则为空

addr.code.length 判断是否为合约（>0 表示是合约）

addr.codehash 合约代码的 keccak256 哈希（bytes32）  
\`\`\`

\- bytes：定长字节数组，从 bytes1 到 bytes32，支持索引，有长度

\- enum：枚举类型，按定义顺序从 0 开始自动分配整数值。

\`\`\`c  
enum Status { Pending, Approved, Rejected }  
// Pending → 0  
// Approved → 1  
// Rejected → 2  
\`\`\`

2\. 引用类型

\*\*引用类型使用时都需要加上 storage、memory 或 calldata。\*\*

\> storage：持久化存储在区块链上（合约状态变量）  
\> memory：临时存储在内存，只在函数执行期间存在，函数结束即销毁。适合临时数据，gas 消耗较低。（函数内部使用）  
\> calldata：只读的调用数据，通常用于外部函数参数，从交易数据中读取，不分配新内存，节省 gas。但构造函数参数不能使用 calldata

\- Array：数组，分为定长数组和动态数组，比如 int\[5\]和 int\[\]

\- struct：结构体

\`\`\`c  
struct Person {  
string name;  
uint age;  
}  
\`\`\`

\- mapping：映射，也就是哈希 map，仅能声明为 storge，只能声明在合约顶部，不能作为局部变量或函数出入参。key 只能是值类型，value 无限制，key 不存在时默认返回值是 0。mapping 没有长度，无法遍历，也不能作为函数参数传递。

\- string：动态长度的字符串，底层是 bytes，不支持索引，但是可以通过 bytes(string)转成 bytes 类型

\## 函数

\### 构造函数

在合约部署时只执行一次，用于初始化状态变量（如设置所有者、初始参数等）。

特点：

\- 只能有一个构造函数

\- 不能有返回值

\- 不能被外部调用

\- 如果没有显示定义则有一个空的默认构造函数

从 Solidity 0.7.0 起，不再使用 function ContractName() 的方式定义构造函数，必须使用 constructor 关键字，如：

\`\`\`c  
pragma solidity ^0.8.0;

contract MyToken {  
address public owner;  
string public name;

// 构造函数  
constructor(string memory \_name) {  
owner = msg.sender; // 部署者设为所有者  
name = \_name; // 初始化代币名称  
}  
}  
\`\`\`

\### 普通函数

\`\`\`c  
function transfer(address to, uint amount) public {

//require，一般用于外部输入的条件检查，如果不满足condition条件则：  
//1. 立刻终止当前函数执行  
//2. 回滚所有状态更改  
//3. 退还剩余gas，已消耗的不退  
//4. 最终返回一条错误信息  
require(condition, “可选的错误消息”);

//如果assert失败说明代码存在bug  
//1. 回滚不退gas  
//2. 抛出操作码  
assert(condition)

//立即回滚 + 退还未用 gas + 返回错误消息  
if(…){  
revert(“msg”);  
}  
}  
\`\`\`

\### view 修饰的函数

不修改状态，可被外部免费调用（不消耗 gas）

\`\`\`c  
function balanceOf(address user) public view returns (uint) {  
return balances\[user\];  
}  
\`\`\`

\### pure 修饰的函数

既不读也不写状态，完全依赖参数计算

\`\`\`c  
function add(uint a, uint b) public pure returns (uint) {  
return a + b;  
}  
\`\`\`

\### fallback 函数

当合约收到无 calldata 的调用或调用了不存在的函数时触发

\`\`\`c  
fallback() external payable {  
// 可接收 ETH  
}  
\`\`\`

\### receive 函数

仅当合约通过普通转账（非函数调用）收到 ETH 时触发

\`\`\`c  
receive() external payable {  
// 必须标记为 payable  
}  
\`\`\`

\### modifier(修饰器)

用于复用校验逻辑（如权限控制）

\`\`\`c  
modifier onlyOwner() {  
require(msg.sender == owner, “Not owner”);  
\_;  
}

function withdraw() public onlyOwner {  
payable(msg.sender).transfer(address(this).balance);  
}  
\`\`\`

\## 关键字

\- external：函数可见性（visibility）修饰符之一，主要用于声明\*\*这个函数只能从合约外部调用\*\*，比如通过交易、其他合约调用或者 this.functionName()，不能直接 functionName()调用，更省 gas！

Solidity 函数调用有两种方式：

内部调用（jump）：像普通函数调用，参数用内存指针传递。  
外部调用（EVM message call）：通过 calldata 传递数据。

public 函数必须支持两种调用方式，所以编译器会强制把参数（尤其是数组、string、bytes）拷贝到 memory（即使是从外部调用也拷贝），这会产生 gas 开销。

external 函数只支持外部调用，编译器知道参数只来自 calldata，所以可以直接读取 calldata 中的数据，不需要拷贝 → 节省 gas。

\- public：可以直接 functionName()调用

\## 一些全局变量

\- msg：一个内置的全局结构体（struct），由以太坊虚拟机（EVM）在每次外部调用时自动提供，无需声明或初始化，在任何函数中都可以直接访问，它包含了当前函数调用的上下文信息。

!\[alt text\](image.png)

\- block：提供当前区块的信息

!\[alt text\](image-1.png)

\## ABI 接口

Application Binary Interface，应用二进制接口。相当于接口说明书，java 的 swagger 文档，json 格式，包含了：

\- 这个合约有哪些可调用的函数？  
\- 每个函数的名字、参数类型、返回值类型是什么？  
\- 还有哪些事件（Events）、错误（Errors）可以被触发或抛出？

\### 为什么需要 ABI？

智能合约部署到链上后，EVM（以太坊虚拟机）只认识字节码（bytecode），是一串十六进制字符串（人类完全看不懂），ABI 解决了这个问题，可以\*\*编码\*\*把人类可读的函数调用（如 transfer(address to, uint amount)）转成 EVM 能懂的二进制数据，可以\*\*解码\*\*把合约返回的二进制数据转回人类可读的结果。

\`\`\`json  
\[  
{  
“inputs”: \[  
{ “internalType”: “address”, “name”: “to”, “type”: “address” },  
{ “internalType”: “uint256”, “name”: “amount”, “type”: “uint256” }  
\],  
“name”: “transfer”,  
“outputs”: \[{ “internalType”: “bool”, “name”: “”, “type”: “bool” }\],  
“stateMutability”: “nonpayable”,  
“type”: “function”  
},  
{  
“anonymous”: false,  
“inputs”: \[  
{  
“indexed”: true,  
“internalType”: “address”,  
“name”: “from”,  
“type”: “address”  
},  
{  
“indexed”: true,  
“internalType”: “address”,  
“name”: “to”,  
“type”: “address”  
},  
{  
“indexed”: false,  
“internalType”: “uint256”,  
“name”: “value”,  
“type”: “uint256”  
}  
\],  
“name”: “Transfer”,  
“type”: “event”  
}  
\]  
\`\`\`

\-–

\### 补充说明

\- \`storage\`：持久化存储（合约状态）  
\- \`memory\`：临时内存（函数内使用）  
\- \`calldata\`：只读外部调用参数（节省 gas）  
\- \*\*\`string\` 和 \`bytes\`\*\* 虽然行为类似数组，但因用途特殊，通常单独归类。  
\- \*\*\`address payable\`\*\* 是 \`address\` 的子类型，拥有 \`.transfer()\` 和 \`.send()\` 方法；可通过 \`payable(addr)\` 转换。

\-–

\> ⚠️ 注意：\`mapping\` 不能作为函数参数、返回值或局部变量（只能作为状态变量）。

\`\`\`c  
pragma solidity ^0.8.0;

contract DataTypes {  
bool public isActive = true;  
uint256 public count = 100;  
address public owner = msg.sender;  
bytes32 public hash = keccak256(“hello”);

enum Status { Pending, Approved, Rejected }  
Status public status = Status.Pending;

struct User {  
string name;  
uint age;  
}

User\[\] public users; // 动态数组  
mapping(address => uint) public balances;

function addUser(string memory \_name, uint \_age) public {  
users.push(User(\_name, \_age));  
}  
}  
\`\`\`
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->
















## 概述 🎯

本课程围绕社群运营的基础技巧及活动策划展开，以 Telegram 社群作为主要示范平台，详解如何快速搭建并管理一个社群，如何利用数据面板监控社群活跃度和成员行为，结合实用的机器人工具提升管理效率。

随后课程讲解了线上活动策划的基本流程，通过一个推特空间的案例，引导学员掌握从准备、执行到复盘的全面操作流程，便于将所学应用于实际工作或兴趣社群。

### 核心知识点总结 ⏰

### 00:00-02:45 社群运营与活动策划课程介绍

本课程分为两部分：一是以 Telegram 社群为例，教大家如何快速搭建社群、利用数据面板和机器人工具进行高效管理；二是介绍活动策划和执行流程，涵盖线上分享活动的案例，指导学员如何策划并实施线上互动。

### 02:45-09:20 搭建基础 Telegram 社群

详细讲解了社群的建立操作，包括选择公开或私密群组类型，设置头像、名称、描述等基本信息。强调管理员权限的合理分配，防止权限滥用；首次启用话题（topic）分区功能，有助于实现内容分类管理，增强社群讨论的针对性和效率。同时提示避免 topic 过多导致混乱，对不活跃话题及时清理。

### 09:20-15:30 利用数据面板监控社群表现

教学员如何通过 Telegram 内置的数据面板监控社群成员数、活跃成员、消息数量及增长趋势。说明如何根据数据调整运营策略，同时介绍语言分布及活跃时间段分析，帮助确定最佳发帖时机，从而提高信息触达率和互动效果。

### 15:30-23:30 机器人管理工具（@MissRose\_bot）使用指南

介绍使用 Rose 机器人实现社群管理的步骤。验证机器人身份为官方账号，避免被假冒机器人骚扰或钓鱼。讲解基础指令如“start”、“help”，“blocklist”等的使用方法，设置屏蔽敏感词，维护社群环境，机器人权限和管理范围的合理配置也十分重要。

### 23:30-28:00 社群运营中的安全与风险意识

提醒学员警惕被非邀请入群及虚假机器人的困扰，强调活跃的真实社群比单纯的成员数更重要。提及社群刷粉和机器人账号现象，提示运营需兼顾成员质量和活跃度，避免社群沦为“僵尸粉”聚集地。

### 28:00-38:50 活动策划的三大阶段与推特空间案例解析

介绍活动策划包括准备、执行和复盘三大阶段。以 Twitter 空间为例，强调明确主题及目标人群的重要性，制定响亮标题，准备问题库供嘉宾参考。

阐述邀请嘉宾时要明确活动主题、时间和主要议题，确保沟通有效。宣传材料需要包含核心信息：活动主题、时间、嘉宾信息和参与链接。

### 38:50-46:30 执行阶段流程及注意事项

开始时明确项目品牌、主持人与嘉宾介绍，确保观众了解活动目的。发挥问题库的作用，灵活应对现场问答。观众提问环节可选。提醒要实时掌控活动数据，确保活动效果达标。

### 46:30-54:00 活动复盘与数据分析

强调活动后对浏览量、实时在线人数和总进入数进行数据回顾，指出实时在线人数更能反映活动效果。教导学员辨别“买粉”等人为数据膨胀的现象，结合数据和实际感受总结活动优劣，优化下一次活动策略。

### 54:00-01:08:30 活动实操安排与常见问题答疑

介绍实操表单填写要求和团队组队建议（三人左右为宜），活动日期安排及审核流程。讲解技术支持与推特账号使用注意事项，强调内容与 Web3 主题相关性。分享学员询问关于社群假聊机器人、Discord 对比等常见问题，提醒合理评估社群活跃度与质量。最后概述课程重点，鼓励学员积极实践，深化理解。

## 推理结构 📊

### 1.确定活动目标：

明确主题和目标受众 → 制定主题名称 → 设计问题库 → 邀请合适嘉宾。

### 2.活动准备：

收集嘉宾信息（名称、头像、社交账号、所属机构） → 制作海报及宣传文案 → 选择并设置推特空间时间。

### 3.活动执行：

上线时展示品牌和介绍主持及嘉宾 → 使用问题库进行讨论 → 预留观众提问环节。

### 4.活动复盘：

收集数据（浏览量、实时在线人数） → 分析数据真实性 → 结合体验总结优缺点 → 调整策略，为下次活动做准备。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->


















# **Key Hash Based Tokens: 从 ERC-721 到 ERC-7962**

## 动机与背景

传统 NFT（ERC-721）的所有权与地址绑定，所有权与转移记录完全公开。这种透明性符合区块链精神，但在涉及“身份”或“会员资格”的业务场景中会产生隐私冲突。例如：星巴克会员希望在机场贵宾室以“会员身份”获得服务，但星巴克无法将完整会员名单直接提供给机场，也无法公开地址与身份信息。因此产生了“验证资格但不暴露身份”的需求。现有 ERC-721 在隐私与链上身份方面难以满足这种场景，促成了 ERC-7962 的设计。

## 核心理念

ERC-7962 的核心是在链上确认资产归属，同时避免暴露持有者的真实地址。协议将所有权表示从“地址”替换为“公钥哈希（kHash）”，链上不出现真实地址与可追踪路径。同时将“所有权”与“交易发起者”解耦，可由第三方代发交易。配合零知识证明用于完成“证明用户具有资格”但不暴露真实标识的过程，形成隐私友好的链上身份表达模式！

## 技术实现特点

-   所有权标识替换 owner 字段不再存储地址，而是公钥哈希，减少可关联性。
    
-   UTXO 风格的密钥一次性使用 转移后生成新密钥，避免交易链路被追踪，增强隐私。
    
-   无 Approve 机制 Approve 会在链上暴露公钥或权限授予意图，与目标相悖，因此被移除。
    
-   交易发起者与所有权解耦 用户无需钱包和 Gas，可由商家或服务方代付交易并提交链上操作。
    

这一机制本质上改善了 Web2 用户接入 Web3 的体验，也为企业级分发、批量空投、会员体系提供可行入口。

## 应用场景

协议主要面向“隐私 + 资格验证”的业务，例如企业会员体系。以前面的星巴克示例：星巴克将会员身份铸为 ERC-7962，用户无需管理地址或 Gas，通过签名表达意图即可；到机场贵宾室时，用户仅需出示证明自己“拥有会员资格”，无须暴露地址、交易记录或完整会员名单。Gas 可由星巴克或机场承担，符合商业激励逻辑。由此实现了链上隐私身份验证、降低 Web3 使用门槛，并可用于 C 端用户无感参与的 Web3 场景。

## 优势与局限

协议优势包括：隐私保护、不可关联性、支持批量处理、支持代付 Gas、对 Web2 新手友好、适用于企业级场景。局限在于 Gas 成本高于传统 ERC、向后兼容较弱、公钥管理要求更高、密钥暴露风险需要轮换机制规避。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->




















\[铸造网站\]([https://nft.myfirst.io/](https://nft.myfirst.io/))

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-18-1768724065411-image.png)

**领取水龙头**

水龙头是在测试网使用的货币，没有交易价值，铸造 NFT 的前提是钱包要有 Sepolia 水龙头，因为要支付 gas 费。我的 Sepolia 是在 ETH。

[https://sepolia-faucet.pk910.de/](https://sepolia-faucet.pk910.de/)

[https://sepoliafaucet.org/](https://sepoliafaucet.org/)

# **铸造 NFT**

在\[铸造网站\]([https://nft.myfirst.io/)搭配好自己的](https://nft.myfirst.io/\)%E6%90%AD%E9%85%8D%E5%A5%BD%E8%87%AA%E5%B7%B1%E7%9A%84) NFT 头像后链接钱包，选择 Sepolia 网络并支付，如果没有 Sepolia 测试水龙头将无法铸造。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-18-1768724110572-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-18-1768724152805-image.png)

# **使用 EtherScan 查看交易内容**

364 是铸造的 Token ID，\*\*\*\*Mint\*\*\*\*意为铸造

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-18-1768724186137-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-18-1768724220896-image.png)

下图可以看到 Token ID、合约地址、该 NFT 所属地址也就是我的钱包地址

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-18-1768724266689-image.png)

如果要查看 NFT 具体内容，可以点击 Input Data 的 view in decoder，然后就可以看到 NFT 的图片链接了

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-18-1768724283475-image.png)

\- Transaction Fee: 该笔交易最终花费的 ETH

\- Gas Price: gas 费价格，单位是 Gwei，好比油价，8 元/升，8 元就是 gas 价格，升就是 ETH

# **钱包导入 NFT**

打开 metaMask 钱包，选择 Sepolia 网络，输入铸造 NFT 合约地址，注意不是自己的钱包地址！是 NFT 合约地址！收藏品 ID 是 Token ID，即 364

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-18-1768724366229-image.png)

导入后如果 NFT 头像没有加载出来是正常状况，可能是 IPFS 问题，可以尝试刷新。

# **IPFS**

InterPlanetary File System 星际文件系统， 是一种去中心化的文件存储和传输协议，专为 Web3 和区块链生态设计，用来解决传统 HTTP 协议在 NFT、元宇宙等场景下的局限性。上传的文件会被分发保存到多个节点，一个节点的保存失效不影响其访问。

## **IPFS 地址长什么样？**

在铸造 NFT 时，把图片上传到 IPFS：

\- 图片内容 → 生成哈希：QmT8Tk2sVjKx7U9zZzZzZzZzZzZzZzZzZzZzZzZzZzZz

\- 元数据 JSON 也上传 IPFS：QmY9Yk3sWjLx8U0aAaAaAaAaAaAaAaAaAaAaAaAaAaAa

\- 铸造时传入：

\`\`\`json

{

“name”: “MyFirstNFT #364”,

“image”: “ipfs://QmT8Tk2sVjKx7U9zZzZzZzZzZzZzZzZzZzZzZzZzZzZz”

}

\`\`\`

\- 原生格式：ipfs://QmXxxx…（这是标准写法，Web3 应用推荐使用）

\- 通过网关访问（浏览器兼容）：

[https://ipfs.io/ipfs/QmXxxx](https://ipfs.io/ipfs/QmXxxx)…

[https://gateway.pinata.cloud/ipfs/QmXxxx](https://gateway.pinata.cloud/ipfs/QmXxxx)…

[https://cloudflare-ipfs.com/ipfs/QmXxxx](https://cloudflare-ipfs.com/ipfs/QmXxxx)…

IPFS 是内容寻址，如果两个人上传了同一个文件，那么他们共享同一个哈希地址，指向同一份数据。

如果我本地删除了这个相同的文件，不影响另一个人，因为文件被多个节点分布式保存。

## **如何上传 IPFS**

可以使用免费的 IPFS 网站，上传文件后即可获得 IPFS 链接

\[Pinata\]([https://app.nft.storage](https://app.nft.storage))

\[[nft.storage](http://nft.storage)\]([https://app.pinata.cloud](https://app.pinata.cloud))

# EIP

Ethereum Improvement Proposal（以太坊改进提案），EIP 是向以太坊社区提出新功能、流程或标准的正式文档。任何重大变更（如协议升级、新操作码、费用机制）都必须先提交 EIP。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-18-1768724576889-image.png)

ERC

Ethereum Request for Comments（以太坊征求意见稿），ERC 是针对智能合约和 DApp 开发者制定的应用层标准，确保不同项目之间的互操作性。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-18-1768724558926-image.png)
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->






















\## Node.js 是什么？  
  
Node.js = V8 引擎 + 事件驱动 I/O + 模块系统  
  
\- V8 引擎：Google Chrome 的 JavaScript 引擎（用 C++ 编写），可将 JS 通过 JIT 及时编译为机器码高速执行，同时拥有自动垃圾回收 GC  
\- 事件循环（Event Loop）：非阻塞 I/O 模型，适合高并发场景（如 Web API、实时聊天）  
  
!\[\](image.png)  
  
说到这里不得不提到 JS，JS 在设计之初是浏览器脚本语言，只能在浏览器中运行（因为浏览器有 JS 运行所需的 V8 引擎），NodeJS（内嵌 V8 引擎） 的出现让 JS 脱离了浏览器，让 JS 有了\*\*后端运行时\*\*，成为通用编程语言，也就是可以作为后端开发语言！  
  
\## 什么是 V8 引擎  
  
V8 是 Google 开发的开源 JavaScript 和 WebAssembly 引擎，用 C++ 编写，负责将 JS 代码编译为机器码并执行  
  
\## NodeJS 和 JS 区别  
  
!\[\](image-1.png)  
  
\## NodeJS 后端框架  
  
!\[\](image-2.png)  
  
\## JS 模块系统  
  
JavaScript 有两种主流模块系统：  
  
\- CommonJS（Node.js 传统标准）  
语法：require() / module.exports  
特点：  
同步加载（适合服务器）  
运行时动态加载  
  
\`\`\`js  
// app.js (导入)  
const { add } = require('./math');  
console.log(add(2, 3)); // 5  
  
// math.js (导出)  
const add = (a, b) => a + b;  
module.exports = { add };  
\`\`\`  
  
\- ES Modules（ECMAScript 标准，现代方案）  
语法：import / export  
特点：  
静态分析（编译时确定依赖）  
支持 tree-shaking（打包优化）  
浏览器原生支持  
  
\`\`\`js  
// app.js (导入)  
import multiply, { add } from './math.js';  
console.log(add(2, 3)); // 5  
console.log(multiply(2, 3)); // 6  
  
// math.js (导出)  
export const add = (a, b) => a + b;  
export default function multiply(a, b) {  
return a \* b;  
}  
\`\`\`  
  
⚠️ 混合使用陷阱：  
  
CommonJS 不能直接 import ESM，需动态 import()  
  
ESM 不能直接 require CommonJS，需 createRequire()  
  
🔑 Node.js 中如何支持两者？  
  
\- .js 文件：默认 CommonJS  
\- .mjs 文件 或 package.json 中 "type": "module" → 启用 ESM  
  
\## NodeJS 历史包袱  
  
\### CommonJS  
  
Node.js 诞生时，ES Modules（import/export）尚未标准化，于是采用社区方案 CommonJS（require/module.exports），导致后续 ESM 完善后在同一项目中 CommonJS 与 ESM 混用发生冲突。  
  
\### 回调地狱  
  
早期 Node.js 依赖回调函数处理异步 I/O，嵌套过深导致代码难以维护,后来引入 Promise 和 async/await（Koa 框架率先采用），但旧代码仍大量存在。  
  
\`\`\`js  
fs.readFile('file1.txt', 'utf8', (err, data1) => {  
if (err) throw err;  
fs.readFile('file2.txt', 'utf8', (err, data2) => {  
if (err) throw err;  
fs.writeFile('output.txt', data1 + data2, (err) => {  
if (err) throw err;  
console.log('Done!');  
});  
});  
});  
\`\`\`  
  
\### 安全模型弱（无沙箱）  
  
Node.js 默认无权限控制，任何 JS 代码可读写文件、访问网络。  
  
\`\`\`js  
// 恶意 npm 包可能恶意操作系统重要文件  
require('fs').writeFileSync('/etc/passwd', 'hacked');  
require('child\_process').exec('rm -rf /');  
\`\`\`  
  
\### npm 依赖地狱  
  
node\_modules 嵌套过深，依赖包重  
  
\## NodeJS 项目结构  
  
\`\`\`  
my-node-app/  
├── src/ # 源代码目录（核心！）  
│ ├── controllers/ # 路由处理器（业务逻辑）  
│ ├── routes/ # API 路由定义  
│ ├── models/ # 数据模型（如 Mongoose Schema）  
│ ├── middleware/ # 中间件（认证、日志、错误处理）  
│ ├── services/ # 业务服务层（解耦逻辑）  
│ ├── utils/ # 工具函数（日期格式化、加密等）  
│ └── app.js # Express/Koa 应用实例  
│  
├── config/ # 配置文件（数据库、环境变量）  
│ └── db.js # 数据库连接配置  
│  
├── tests/ # 测试文件（Jest, Mocha）  
│ ├── unit/ # 单元测试  
│ └── integration/ # 集成测试  
│  
├── public/ # 静态资源（HTML/CSS/图片，可选）  
├── uploads/ # 用户上传文件（如头像）  
│  
├── .env # 环境变量（API\_KEY, DB\_URL）  
├── .gitignore # 忽略 node\_modules 等  
├── package.json # 项目依赖 + 脚本命令  
├── package-lock.json # 依赖锁定文件  
└── server.js # 服务器入口文件（启动 HTTP 服务）  
\`\`\`  
  
\## NodeJS 替代品  
  
!\[\](image-3.png)  
  
\*\*Bun 同时兼容 NestJS、Koa、Express、Fastify 框架，而 Deno 不兼容\*\*
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->























# 以太坊**分叉**

我们知道智能合约特点就是不可篡改+自动执行，那么部署过的合约，如果真的有漏洞，如何弥补呢？有几个方法：

1.  硬分叉：分叉会改变**整个区块链的状态**，不是只改一个合约。区块链分叉后，之后的交易都打包区块在新的链上，此方法需要区块链的验证者配合执行。硬分叉是**极端措施**，仅用于**重大安全事件**（如 The DAO 黑客事件），**不会为单个合约漏洞发起分叉**（成本太高、破坏去中心化原则），分叉后通常形成**两条链**（如 ETH / ETC），**原链是否“有效”取决于社区选择。**
    

如以太坊每次升级都是硬分叉，[https://eips.ethereum.orq/EIPS/eip-7607](https://eips.ethereum.orq/EIPS/eip-7607)

以太坊EIP：社区讨论->形成文档->升级（每年1-2次）

2.  部署合约时采用代理模式，也就是用户和代理进行交互，代理再和区块链交互，当要升级合约时，只需要让代理指向新的合约即可。
    
3.  紧急暂停，在合约中预设 `pause()` 函数，发现漏洞时立即暂停所有功能，争取时间部署修复方案（如通过代理升级）。
    
4.  资金迁移，部署新合约，提供 `migrate()` 函数让用户主动转移资产。
    
5.  链下补偿，项目方用自有资金赔偿用户损失，**不修改链上状态**，但维护声誉（如 Wormhole 跨链桥被盗后团队垫付 $3.25 亿）。
    

# Web3社会学

技术层面：用私钥签名证明你是谁，用共识网络保证账本可信，用智能合约让规则自动执行。

技术之外：权力的重新分配、数字资产与权利自我控制、无许可、抗审查、公开可信、去中间商。

每个区块可以有多个哈希交易，验证者一定是节点，节点不一定都是验证者，只有质押了区块链原生货币的节点才是验证者，这就是POS。

# 合规性

## ICO（首次代币发行）

法律定性：若面向不特定公众公开募资 → 涉嫌 非法吸收公众存款罪 / 集资诈骗罪，中国大陆已基本禁止公开ICO。

✅ 纯技术发币（无融资）→ 一般不违法

❌ 借发币名义公开募资 → 高风险

## 永续合约（Perpetual Contract）

公安常定性为“赌博软件”。

**法律误区：**

赌博 = “买定离手 + 结果随机”。但实际上合约买定后可以不离手，主动性在自己，不在赌场，性质不一样。

永续合约 = “用户全程可控 + 价格由市场决定” → 属金融衍生品

**风险点：**

1.  技术人员、运营、客服均可能被列为共犯
    
2.  “技术中立”辩护在司法实践中常被驳回
    

## 挖矿业务

政策环境：中国大陆禁止境内挖矿，但允许算力出口。

**合规路径：**

1.  通过“超算中心”形式包装
    
2.  结合外贸模式，将算力服务售予海外客户
    
3.  避免直接持有或交易虚拟资产
    

## 出入金与U卡

出入金就是法币和虚拟货币转换

**U卡（数字货币支付卡）风险：**

非正规发卡方可能绑定虚假“员工”身份，导致账户被冻结，资金易被黑灰产利用，遭遇“资金攻击”。

**合规建议：**

1.  选择持牌/正规U卡服务商
    
2.  主动报税、保留交易记录
    
3.  遭遇异常及时报案，形成“风险隔离”
    

## KOL与推广行为

典型案例：为**海外**永续合约交易所推广 → 被控 “非法利用信息网络罪”

**风险逻辑：**

1.  即使未直接参与运营，推广行为本身可能构成“帮助犯”
    
2.  返佣模式加剧责任认定
    

**合规要点：**

1.  避免推广涉赌、无牌金融产品
    
2.  建立内容审核机制
    
3.  优先选择已与地方监管建立沟通的合规平台合作
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
























# **Web3 行业全局介绍 & 岗位概览**

## 发展规模

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768484798780-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485033688-image.png)

## 求职岗位

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485051000-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485156432-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485182238-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485292440-image.png)

## 求职面试

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485424613-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485505401-image.png)

# Web3运行原理

助记词和私钥一对多，私钥可以自己生成。

交易就是要做的事+gas费+防重放序号nonce，而钱包则要组装交易内容并私钥签名，然后广播到区块链网络。

## 交易流程

-   当发起一笔交易后，这笔交易首先被发送给区块链其中一个节点A，在进入节点A前，先验证交易的格式、签名、nonce 等基本合法性，通过**验证**后，这笔交易会被**暂存**到节点A的内存池中，这个池子用来暂存已验证合法但尚未被打包的交易的池子。
    
-   然后节点A立刻广播到邻居节点，然后才在自己的池中按照 Gas 费用高低**排序**，矿工/验证者会优先选择价格更高的交易进行打包，打包成新区块添加到自己的区块链中（每个节点都有自己的区块链，一个个区块首尾相连形成的链）。
    
-   邻居节点收到交易后也会先进行合法性校验，通过后再转发给它们的邻居，不断重复上一步骤，如此病毒式传播，直到该区块链大部分节点都记录这笔交易，此时就可以认为这笔交易在链上达成了共识，被认可。
    

但并非所有节点都会接收或保留这笔交易，有些节点可能因 Gas 费过低而拒绝加入自己的内存池、可能因内存池已满而丢弃低优先级交易。

POW：工作量证明，一种验证交易的方式，验证者/矿工需要投入大量算力来解决复杂数学题，验证成功后才能获得奖励（挖矿）。这种方式能耗很高。

POS：权益证明，一种更环保的验证方式，验证者/矿工需要先质押一定数量的代币，才有资格去验证交易。验证成功后会获得奖励，相比 PoW 更节能。

智能合约：虚拟机中设定好的代码程序，不可篡改且自动执行
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->


























今天参与了以太坊中文周会和web3行业全局介绍&岗位概览分享

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-12-1768223118971-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-12-1768223106935-image.png)

## 发展史

-   最初由 HTTP 的发明者提出，是指一个集成的通信框架，互联网数据可以**跨越各个应用和系统**实现机器可读。Web 3.0 通常也被称作为“语义网”
    
-   2014 年，以太坊创始人重新定义了 Web3 概念，指一种区块链技术，可以\*\*基于“无须信任的交互系统”\*\*在“各方之间实现创新的交互模式”。
    

### Web1.0

-   时间：1994-2004
    
-   特点：Web 1.0 主要是静态的 HTML 网页，用户之间很少交互，有聊天转账功能但是无法保证安全。
    

实际在 1968 年，一个名为“ARPANET”的美国政府项目就已经启动了 Web 1.0。ARPANET 最初是由军方承包商和大学教授组成的一个用于交换数据小型网络。Web 1.0 时代中最具创新力的企业就是**必胜客**。他们在 1995 年开发了一个订购披萨的网页，消费者可以在页面中下单，等到披萨送到后再付现金。

### Web2.0

-   时间：2004 - 至今
    
-   特点：动态网页，用户之间互动性显著提升，用户使用互联网时必须**授权中心化的第三方平台管理大量数据**。因此这些中心化的实体在数据和内容权限方面被赋予了巨大的权力和影响力。
    

### Web3.0

-   时间：2008 - 至今
    
-   特点：**基于数学加密证明，而非信任第三方数据中心平台**，即去中心化。
    

2008 年中本聪提出了点对点的数字货币（比特币）和区块链概念。

智能合约出现后，去中心化互联网模式才逐渐流行，虚拟货币实现了点对点交易，智能合约不是传统的法律约束，而是可编程协议，通过编程实现约定的逻辑，好比商品贩卖机，规定输入硬币，选择商品后，规定输出商品。

## Web3 核心要素

-   区块链
    

安全性和去中心化水平都极高的网络，可以在一个**共享账本**中储存数据、交换价值并记录交易活动，而且账本**不受任何中心化**实体控制。区块链是 Web3 的基石

-   加密资产
    

基于区块链构建的数字货币或代币，类似现实的货币

-   智能合约、去中心化应用
    

智能合约是区块链上不可篡改的程序，利用“如果 x 是真实的，则执行 y”的代码逻辑自动执行交易。通过智能合约可以构建去中心化应用 dAPP，给用户使用。

-   预言机
    

智能合约要充分实现其潜力，就必须能够与区块链网络以外的数据和系统交互。预言机能**将区块链连接至真实世界中的数据和系统**，并提供关键的基础架构，打造一个具有互操作性且统一的 Web3 生态。

-   代币 Token
    

代币不是货币，和 BTC 这种加密货币不一样，\*\*代币构建在已有区块链之上的数字资产\*\*，比如用人民币买了游戏币、藏品，那么人民币就可理解为已有区块链之上的加密货币，游戏币、藏品就是代币，不能当钱花（除非有人愿意买）  
非同质化代币（Non-Fungible Token 简写 NFT ），像比特币这种原生的加密货币，每一单位都一样，可以互换（1 BTC = 1 BTC），是同质化，“非同质化”则是每个 NFT 都独一无二，不能互相替代，比如某个网站域名者签名球衣，可能全世界仅此一份。

-   Gas 费
    

因为区块链每笔交易都在链上进行加密等计算最终上块，肯定要消耗计算机资源，那么收点过路费也不为过吧

## 举例

假设某季节中的降雨量必须超过 20 英寸，才能保障农作物不欠收。一个农民希望买保险来对冲恶劣天气造成的不利影响。正常情况下他必须要经过漫长的理赔流程，并且要依靠**中心化的保险机构**来核实降雨量。但是如果在 Web 的世界，一切都是事先编程约定好的合约，保险流程会非常高效迅速，并基于简单的二元逻辑判断农民是否应获得理赔。物联网传感器在当季记录的降雨量是否低于 20 英寸？如果是，则立即支付赔偿金给农民。如果否，则不支付赔偿金给农民。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
