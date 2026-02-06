---
timezone: UTC+8
---

# Jimmy Wang

**GitHub ID:** Jorrrmungandr

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-02-06
<!-- DAILY_CHECKIN_2026-02-06_START -->
今天收到了 Consensus 共识大会的纪念品NFT，在Verse网站上免费领取了“马到成功”的NFT。但是在这个过程中我产生了疑问：为什么我领取 NFT 时没有花 gas fee，只是签了个名？通过AI的帮助我了解到：

-   ERC-2771 是一种以太坊标准，旨在实现元交易（Meta Transactions），允许用户在不持有 ETH 支付 Gas 费的情况下与智能合约交互
    

猜测是Consensus主办方和Verse进行合作，我们领取NFT的请求不会直接上链，而是在Verse的服务器中，等待网络不拥堵、gas fee 较低的时候上链，gas fee 从 Consensus 给 Verse 提供的预算里出。
<!-- DAILY_CHECKIN_2026-02-06_END -->

# 2026-02-05
<!-- DAILY_CHECKIN_2026-02-05_START -->

今天学习了技术向前端 Logic 老师的学习之路，总结了几点：  
1、多动手，参加迷你黑客松，写小项目demo  
2、多学点运营知识，对理解行业也很有帮助
<!-- DAILY_CHECKIN_2026-02-05_END -->

# 2026-02-04
<!-- DAILY_CHECKIN_2026-02-04_START -->


今天继续学习了Solidity by Example的Uniswap V4部分，相对于V3在数学层面上进行的提升，V4在底层进行了彻底的重构，引入了hooks的概念，开发者可以在交易的生命周期中插入合约代码。生命周期的关键时间点包括：

-   初始化池子之前/之后
    
-   LP 添加/移除流动性之前/之后
    
-   交易 发生之前/之后
    

此外，V2 和 V3 通常需要将 ETH 包装成 WETH 才能交易。V4 允许原生 ETH 直接参与交易，省去了包装/解包的 Gas 费
<!-- DAILY_CHECKIN_2026-02-04_END -->

# 2026-02-03
<!-- DAILY_CHECKIN_2026-02-03_START -->



今天继续学习了solidity by example，主要学习了Uniswap V3的合约。相比于V2，V3在底层数学模型上进行了升级，一句话概括：从全范围流动性转变为集中流动性

-   V2
    

遵循标准的 xy = k 公式。流动性均匀分布在整个价格区间内。但是绝大多数流动性从未被利用，导致资金利用率极低。

-   V3
    

允许 LP 显式选择一个特定的价格区间来提供流动性，在这个特定区间内，V3 依然遵循 xy = k 的曲线，但在区间之外，你的流动性完全不参与交易（即资金被移出）。结果是，同样数额的资金，在 V3 中可以提供比 V2 高出数千倍的“有效流动性”，从而大幅降低交易滑点。
<!-- DAILY_CHECKIN_2026-02-03_END -->

# 2026-02-02
<!-- DAILY_CHECKIN_2026-02-02_START -->




今天学习了Solidity by Example 的 DeFi 部分，Uniswap V2 是基于恒定乘积公式： xy=k 的 DEX。

-   通过 `swap` 实现代币兑换，`addLiquidity` 注入流动性并获得 LP Token。
    

-   利用代币的储备量比例定价
    
-   手续费在兑换前扣除，确保 k 值在交易后非减。
    
-   闪电贷 `optimistic transfers`，允许先借出代币，只要在同一笔交易内还清并支付手续费即可。
    

此外还了解到 ENS 的概念，它类似于 Web2 中的 DNS 服务，将难以记忆的复杂加密钱包地址（`0x123...`）转换为可读的域名。它本质上是 NFT，不是以太坊原生支持的概念，在实际使用中，钱包插件会将域名转换为 EOA，主要经过了 NameHash、查找注册表和解析域名三步。  
Q：为什么要分成注册和解析两步？  
A：ENS 是不断扩展的业务，最初只是为了存储账户地址，后来成为了标识个人身份的工具，可以存储 BTC 地址、telegram 账号信息、NFT 头像、个人简介等等。而区块链上的合约一旦部署就很难修改（尤其是数据结构）。如果所有数据都存在注册表里，每次想支持一种新币种或新功能，就需要重新部署整个 ENS 系统。
<!-- DAILY_CHECKIN_2026-02-02_END -->

# 2026-02-01
<!-- DAILY_CHECKIN_2026-02-01_START -->





今天开始了 speedrunethereum Challenge 0 Tokenization 的挑战，熟悉了以太坊开发的基础知识和环境配置。

使用 `openssl rand -hex 32` 生成一个 256 位的随机数作为私钥， `openssl ec` 配合 secp256k1 曲线从私钥推导出公钥。将公钥经过 `keccak256` 哈希运算，取后 20 字节（40位十六进制）加上 `0x` 前缀，得到最终的以太坊地址。这种不依赖 UI 工具的推导过程让我对 EOA 账户有了更本质的认识。
<!-- DAILY_CHECKIN_2026-02-01_END -->

# 2026-01-31
<!-- DAILY_CHECKIN_2026-01-31_START -->






今天继续深入学习并实践了foundry的知识，chisel 命令提供的交互式 REPL 工具，用于快速调试、验证 Solidity 代码片段。在终端输入 `chisel` 启动交互界面，直接输入 Solidity 语句（如 `uint256 a = 1;`），Chisel 会在命令行即时反馈结果。
<!-- DAILY_CHECKIN_2026-01-31_END -->

# 2026-01-30
<!-- DAILY_CHECKIN_2026-01-30_START -->







今天准备了周会分享的主题，介绍了我的foundry的一些理解，展示了foundry的一些基本用法

-   cast 可以用来实现很多功能，比如 code 子命令可以查看合约代码，tx 可以查看交易信息
    
-   chisel 可以创造一个 REPL 的交互式命令环境
<!-- DAILY_CHECKIN_2026-01-30_END -->

# 2026-01-29
<!-- DAILY_CHECKIN_2026-01-29_START -->








今天主要实践了一下Foundry，然后做了一下黑客松的项目。相比于 hardhat 采用 js 脚本部署合约的形式，foundry 主要利用了命令行进行链上的操作。

-   foundry 中最重要的三个概念：anvil、forge 和 cast
    
-   anvil 相当于游戏的服务器，它在本地启动一个虚拟的以太坊网络
    
-   forge 用于把智能合约部署到链上
    
-   cast 用于与合约互动
    
-   anvil 启动后，会生成若干个以太坊节点，以及一个上帝账户
    
-   forge create 部署合约之后，获得合约地址
    
-   使用 cast call 向合约地址发送交易，用 cast --to-dec 将十六进制输出转化为十进制
<!-- DAILY_CHECKIN_2026-01-29_END -->

# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->









今天和黑客松团队同学商量了一下产品设计，准备做一个拦截用户开合约并可以和用户辩论的产品。搓了一个浏览器插件demo，准备明天做一下交易的监听和拦截
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->










今天接了web3实习计划网站接入posthog的issue，调研了一下posthog，了解它是一个数据分析平台，支持埋点上报、用户行为录制等功能。然后继续实践了下hardhat，在命令行里deploy了demo合约，并写了ignition脚本和合约交互
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->











-   今天接了实习计划网站的两个issue并提了第一个PR，优化了即将到期任务的显示，降低同学们漏掉任务的概率（对我们p人很友好
    
-   在分享会中了解到了休闲黑客松的玩法和赛道
    
-   学习了hardhat教程并运行了demo，准备明天体验下console实时交互功能
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->












今天继续做了一些Ethernaut的挑战，然后在本地跑通了Web3学习计划网站，看了一下代码，准备后续提一些PR

-   区块链的存储是公开的，任何人都可以查看，哪怕存储是private的数据也可以查看，所以千万不要在合约中存储密码等敏感信息。private 的数据虽然无法通过合约代码访问，但是可以通过低级的存储读取方法（如 web3.eth.getStorageAt）获取
    

-   重入攻击是指攻击者利用合约的漏洞，在合约执行过程中多次调用某个函数，从而获取更多的利益。攻击者可以利用重入攻击来获取合约的控制权
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->













今天继续完成了几个Ethernaut的挑战，收听了同学组织的Web303播客。

-   `tx.origin` 是一个全局变量，它指向**发起当前交易的最初始EOA**，无论调用栈有多深。攻击者可以利用 `tx.origin` 来欺骗合约，使其认为攻击者是合约的拥有者
    
-   `msg.sender` 表示当前调用智能合约函数的**直接发送者**。和 `tx.origin` 不同，它是动态的，如果用户直接调用，它是用户钱包地址；如果合约A调用合约B，在合约B中 `msg.sender` 为合约A的地址，但 `tx.origin` 仍然为用户地址。
    
-   `delegatecall` 是低级调用，可以调用另一个合约的代码，它允许合约 A 执行合约 B 的代码，但使用 A 的存储上下文。换句话说，合约 B 的逻辑会影响合约 A 的状态。攻击者可以利用 `delegatecall` 来调用合约的代码，从而获取合约的所有权
    
-   \`selfdestruct\` 用于销毁合约并将其所有余额发送给指定的目标地址。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->














今天完成了3个Ethernaut上的挑战并听了周会同学们的分享，对智能合约有了进一步的了解  

-   receive函数在合约接收ETH时调用；fallback函数在调用不存在的函数时执行
    

-   老版本solidity中合约的构造函数为合约同名的函数，新版本使用constructor关键字
    
-   链上随机数是可以被预测的，block.number等区块信息是公开的
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->















今天在xiaohai老师的分享会上学习了foundry和hardhat的用法，并自己跑了demo尝试  

-   foundry是由rust编写的，性能好，提供了强大的命令行工具，比如 anvil 提供本地的以太坊测试网节点，模拟真实区块链环境；forge 用于编译、测试和部署智能合约；cast 用于调用智能合约与链上交互  
    
-   hardhat是由js编写的，更灵活，技术栈生态更广，也提供了类似的npx脚本。与foundry不同的是，需要用js代码去部署合约和获取合约地址，但调试时用console.log十分方便
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->
















今天在solidity-by-example网站上体验了一下demo，学习了智能合约的事件系统的用法，了解智能合约的内存模型，主要包括`storage` 和 `memory` 。`storage` 用于永久存储状态变量（如区块链上的数据），访问慢成本高；而 `memory` 是临时存储，用于函数执行过程中的数据（如函数参数），访问快成本低，函数结束即清空。类似于其他语言中堆内存和栈内存的区别。  
此外，对web3-career-build网站提出了若干issue，加入了dev小组，准备这两天本地部署并提几个pr
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->

















今天进一步学习了智能合约，体验了实习手册中给的leader board demo

-   `constructor()` 为合约构造函数，仅在合约部署的那一刻执行一次
    

-   全局变量 `msg.sender` 是 Solidity 中最常用的全局变量，代表当前调用合约函数的钱包地址。
    
-   事件机制，使用 `event` 定义， `emit` 触发。前端页面（如 React/Vue）可以监听这些事件，当有人留言时自动刷新页面。
    

-   通过钱包或浏览器直接调用 `view` 函数时，完全不消耗 Gas。单数如果一个“写操作”函数在执行过程中调用了 `view` 函数，它仍然会作为整体交易的一部分消耗 Gas。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->


















今天学习了 Solidity 的基本用法和 Remix IDE 的基本操作  
1、Solidity 中常见的数据类型和函数修饰符  
2、仿照 demo 写了个简单的数据存储合约  
3、在 Remix 中编译并部署这个合约，并尝试了存储和取数这两个基本操作  
  
读取链上状态数据而不修改的函数，在节点本地执行（不上链），因此不消耗 Gas（但在测试网中还是需要消耗）
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->



















今天使用gemini3 pro体验了一下vibe coding，做了一个简单的nft minter  
铸造nft主要分为两步：

1、发起交易 :

唤起钱包，用户点击“确认”后，交易被广播到内存池，此时你会得到一个 `hash` (交易哈希)，但这不代表成功，只代表交易“已发送”

2、监听 & 上链:

前端拿到 `hash` 后，挂起监听。直到矿工把这笔交易打包进区块，才算真正铸造成功。  

（注意，evm不懂小数，要将ETH转化为Wei）
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->




















今天参加了LXDAO的周会, 了解了LXDAO的使命愿景以及工作模式, 并学习了他们写的My First ZKVote教程(和My First NFT类似, 这种边学习边实践的模式我非常喜欢! 建议多来点)

传统链上投票的痛点 - 所有人都能看到“谁投了谁”, 缺乏隐私性

ZK投票 - 确保投票有效可验证, 又是匿名的

在 ZK 中有两个主要角色：

Prover (证明者/你)： 拥有秘密（如私钥、投票意向）, 想证明自己合法。

Verifier (验证者/智能合约)： 负责检查证明是否通过，但不知道你的秘密。

ZK投票的过程是:

注册： 本地生成秘密 $\\rightarrow$ 提交身份承诺上链 $\\rightarrow$ 加入 Merkle Tree。

准备： 从链上获取最新的 Merkle Root 和你的 Merkle Path。

生成证明： 在浏览器本地运行电路。

\- 输入： 你的秘密、Merkle 路径、投票选项。

\- 输出： 一个 ZK Proof 和 Nullifier。

提交与验证 ： 将 Proof 提交给智能合约。

\- 合约验证 Proof 有效 $\\rightarrow$ 检查 Nullifier 未使用 $\\rightarrow$ 投票成功！
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->





















今天主要实践了一下Wagmi框架

Wagmi 相当于 Web3 的 React Query，将“连接钱包”、“监听账户变化”、“错误处理”等逻辑封装成了方便调用的hooks，比如 useReadContract 类似于 useQuery，用于读取合约数据，并且同样具有缓存功能；还有 useWriteContract，类似 useMutation，需要用户手动触发签名交易。

常用的API还有 useConnect、useDisconnect、useAccount、useBalance等。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->






















今天主要学习了NFT相关的知识

NFT 不是一张图片，而是一个\*\*智能合约（Smart Contract）\*\*，记录了

_​_\*Token ID\*\*​ （唯一的编号）

_​_\*Owner Address\*\*​（所有者地址）

_​_\*Metadata URI\*\*​（元数据链接，通常是一个指向 IPFS 或服务器的 URL）

一个NFT合约需要满足代币标准ERC-721，其中最核心的是两个状态变量：

1\. `_owners`​: `mapping(uint256 => address)`

\* 输入 Token ID，返回持有者的钱包地址。

2\. `_balances`\*: `mapping(address => uint256)`

\* 输入钱包地址，返回该地址持有的 NFT 数量。

一个NFT合约还需要实现代币标准ERC-721规定的所有方法，比如：

\* balanceOf(owner): 查询某人有多少个 NFT。

\* ownerOf(tokenId): 查询某个 ID 的主人是谁。

\* safeTransferFrom(from, to, tokenId): 安全转账。

\* approve(to, tokenId): 授权别人（如 OpenSea）操作你的这一个 NFT。

\* setApprovalForAll(operator, approved): 授权别人操作你所有的 NFT（钓鱼攻防挑战中提到）。

\* tokenURI(tokenId): 获取某个 ID 对应 NFT 的元数据链接。

后续还有新的ERC-1155对 NFT 进行了拓展，单个合约可以管理多种类型的代币（\*\*包括同质化和非同质化\*\*）。支持批量转账，极大节省 Gas 费。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->























计划：

1、继续学习ethereum基本概念  
2、用wagmi实现转账等基本操作
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
























计划：  
1、体验一下wagmi的基本功能

2、做几关钓鱼攻防（连接钱包、授权、交易等操作）

打卡：

1、体验了连接钱包的功能，学习了useConnect hook  
2、了解了DApp和钱包的一些交互，包括签名、授权、转账等，了解不同授权协议之间的区别，比如Uniswap的Permit3协议
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->


























## 学习内容

1.  在水龙头领取测试币并转账给其他同学
    
2.  完成了NFT铸造任务
    
3.  参加了ETH中文周会、Co-Learning和行业介绍会
    

## 学习感悟

今天主要学习了钱包的基本使用，包括浏览器钱包插件、网络的切换以及发送/接收代币等功能

在My First NFT网站中实践NFT铸造时，由于我是web2开发人员，比较关注网络请求的链路，我发现web3应用中，链上的请求无法在浏览器的console中查看到。问了wachi老师之后才知道，链上的网络交互都是由钱包插件完成，而浏览器页面只需要给插件传递一个json即可，然后钱包会发送rpc请求给链上节点
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
