---
timezone: UTC+8
---

# Max

**GitHub ID:** Linwentao-tech

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->
Uniswap V3 的核心是集中流动性。LP 不再把资金均匀铺在 0 到无穷的价格区间，而是只在自己选择的价格区间内提供流动性，这显著提高了资本效率。价格被离散为 tick，LP 通过 tickLower 和 tickUpper 定义自己的做市区间，不同费率对应不同的 tick 间隔，用精度换取 gas 成本。每一份 LP 头寸以 NFT 的形式存在，里面包含交易对、费率、价格区间和流动性数量，因此 LP 不再是池子的可互换份额，而是带有明确策略参数的独立头寸。手续费不会自动复投，需要手动 collect，窄区间意味着更高资金利用率和更高风险，价格一旦超出区间，头寸就会变成单边资产。整体上，V3 把 LP 从被动提供流动性变成了主动做市，更接近在特定价格区间内跑一套明确的做市策略。
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->

今天看了入门级 Web2 to Web3 中的 Week 3, Day 3 - token vendor：[**Speed Run Ethereum: Challenge 2 - Token Vendor，**](https://www.youtube.com/watch?v=OvBrInI5VbM&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=13)主要是 ERC-20 的资产模型以及合约之间是如何协作的。ERC-20 代币并不实际存放在钱包中，而是记录在代币合约内部的映射结构里，钱包只是对应地址的控制权。这也决定了合约无法在被动接收代币时自动执行逻辑。

代币买入流程相对简单，用户向 Vendor 合约发送 ETH，合约根据固定价格直接将 token 转给用户。卖出流程则必须通过 Approve 机制完成，用户需要先在代币合约中授权 Vendor 使用一定数量的 token，再由 Vendor 主动调用 transferFrom 将 token 转入合约并返还 ETH。这一设计虽然增加了步骤，但明确划分了资产控制与执行权限。

同时，强调了部署和编排的重要性。除了部署 ERC-20 和 Vendor 合约之外，还需要为 Vendor 注入初始 token 流动性，并在测试阶段移交合约的所有权，整个系统才能完整运转。理解这些流程后，ERC-20 合约的可组合性以及 DeFi 协议的运行方式会更加清晰。
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->


今天补了一下youtube的Solidity Walk Through，老师先讲了一下evm的底层架构的stack和storage概念，stack通常用来执行opcode，比如算数或者比较和跳转条件，类似汇编语言。其次是storage，是合约的永久状态，保存的数据永久存在链上。

然后老师讲了一下数值类型，有bool，uint，address ，string，array还有struct等等，然后老师写了一个简单的erc20 token的合约，虽然对我来说不是很新的内容了，然后老师展示了如何提交合约到链上进行部署，然后进行了erc20的合约交互，比如说转账。

总体来说是一些很基础的概念 。
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->





今天看了 `Uniswap/interface`，了解了它作为一个产品级前端工程是怎么被组织和发布的。整个repo通过 `apps / packages / config` 把页面层和通用能力层彻底拆开，链交互、业务计算和组件复用都下沉到 packages 中，应用本身只负责组合和展示。同时这个 repo 是对外发布的稳定版本镜像，而不是日常开发现场，配合打 tag 和 IPFS 发布，前端被当成一个可验证的静态产物来看待。整体给我的感受是，当 Web3 前端复杂到一定程度后，真正决定可维护性的不是 React 写法，而是工程结构、复用边界以及发布方式。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->






前半周系统复习了 Web2 到 Web3 的开发流程，重新梳理了 provider、signer、wallet 的职责划分，以及使用 ethers.js 与合约交互和通过 Hardhat 进行测试的整体结构。结合 Staking App 的实践，将用户操作、合约状态变化与前端读写串联成完整流程，对链上与链下职责边界有了更清晰的认识。后半周集中学习 Uniswap V2，从 Core 与 Periphery 的设计出发，再次理解 Factory、Pair、Router 各自承担的功能，以及协议在自动做市中的运行逻辑。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->







重新梳理了一遍 Uniswap V2 的整体结构，把 Core、Periphery 和 SDK 这三层的职责看得更清楚了。V2 Core 的设计非常收敛，只做最底层的 AMM 逻辑：Factory 负责创建和记录 Pair，Pair 本身既是流动性池也是 LP Token，核心状态只有 reserve，以及围绕 mint / burn / swap 的 `x * y = k` 约束。Pair 不关心谁在调用，只校验最终状态是否满足不变量，这让它天然适合被套利、清算或 flash swap 等场景复用。

Periphery 主要解决“怎么用”的问题。用户基本只和 Router 交互，由它来处理路径拆分、滑点校验、token 转账以及 ETH/WETH 的细节。Router 本身几乎不存状态，更像一个执行层，把复杂交互封装起来，但真正的定价和结算仍然发生在 Pair 中。

SDK 则把链上的这套逻辑原样搬到链下，使用同一套公式提前计算交易结果和 price impact，让前端在发交易前就能预期行为，避免无意义的 revert。整体看下来，Uniswap V2 的重点不在算法复杂度，而在分层边界：Core 稳定且极简，Periphery 可替换，链上和链下逻辑保持一致，这种结构对理解 DeFi 协议和清算执行流程很有参考价值。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->








今天略微看了一下入门级 Web2 to Web3 中的 Week 2 Day 1-5 内容。通过区分 Provider、Signer 和 Wallet，我更清楚地理解了链上读写在本质上的不同：读取状态只是访问节点数据，而写入状态必须由私钥签名并参与共识，这也直接决定了成本和风险。使用 ethers.js 调用合约时，逐步建立起对交易生命周期的认知，明白链上交互并不是即时返回结果，而是一个需要提交、等待确认的过程。Hardhat 和以太坊客户端的介绍，让我对合约开发环境有了更整体的认识，合约可以在本地链中部署、调试和测试，大部分问题应当在本地被发现，而不是依赖测试网反复试错。后续通过 Solidity 测试和 scaffold-eth，把合约放入一个完整应用中观察，进一步理解到合约并不是孤立存在的代码，而是需要和前端、部署流程以及交互逻辑共同构成一个完整系统。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->









### 第五章学习笔记：EVM 与 Gas 机制

这一章主要是在补执行层的直觉。之前一直在写 Solidity，但其实合约真正跑的不是 Solidity，而是被编译成字节码之后，在 EVM 里按 opcode 一条一条执行。所有节点都会重复这套执行过程，所以执行必须是确定性的，这也是为什么 EVM 设计得这么克制。

Gas 的概念在这里终于变得具体了。Gas 本身不是钱，而是计算和存储资源的计量单位。每一条 opcode 都有固定的 Gas 成本，尤其是和 storage 相关的操作，代价明显更高。也就是说，Gas 高不高，很多时候不是“网络贵”，而是你写的逻辑在做什么操作。

以前对 Gas 优化的理解比较模糊，看完这一章之后感觉更偏工程问题了：  
少写 storage、能用 memory 就别落 storage、减少不必要的外部调用，本质上都是在减少高成本 opcode 的使用，而不是在“抠参数”。

还有一点之前没太认真想过：Gas 其实也是安全设计的一部分。Gas 上限 + 用完即失败但不退款，直接限制了死循环和恶意消耗资源的可能性。如果没有这套机制，图灵完备合约在公链上几乎不可行。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->










今天做了一下链上留言板合约这个项目，对我来说是很基础的任务，主要是把以太坊里一些基础但容易被忽略的点重新走了一遍。通过在 Remix 编写并部署合约到 Sepolia，再配合 Etherscan 查看交易和事件，明确了合约部署本质上就是一笔交易，构造函数的状态写入和事件都会在部署阶段一次性完成。留言通过 `mapping(address => string[])` 存储，每次调用都会真实修改链上状态，而事件更多是为了链下读取和追踪，而不是合约内部逻辑使用。整个过程中也再次对 EOA 与合约账户的分工、Gas 的实际消耗位置，以及交易从发起到被确认的流程有了更清晰的整体感知。这类简单合约本身不复杂，但作为基础机制的复盘很有价值。因为今天一直在花时间做自己的项目所以计划明天看一下youtube视频。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->












今天更多是在把之前学过的东西重新对齐和压实。

通过重新实现一个最小化的 ERC-721 合约，我把 NFT 的边界想得更清楚了：NFT 本质上只是链上的所有权记录，合约关心的是 tokenId 的唯一性、地址归属，以及 tokenURI 指向的元数据。图片、音频这些内容本身并不在 NFT 里，只是被引用。配合 IPFS 的内容寻址机制，这种所有权在链上、内容在链下的分工就很清晰了。

在身份和 DeFi 这部分，通过 ENS 和 Uniswap 的实际使用，我开始真正用“协议结构”而不是应用界面去理解它们。ENS 是一个链上 NFT，用来把可读名字绑定到地址；Uniswap 不是换币工具，而是一套 AMM 合约体系，Factory 负责创建池子，Pair 持有流动性并执行 swap，Router 帮用户处理路径和滑点。普通用户几乎只和 Router 交互，直接调用 Pair 更多是工程场景，比如套利、MEV 或清算。

在基础层面，Gas、Nonce 以及交易的加速和取消，让我更清楚交易在 mempool 里的真实行为逻辑。在 Optimism 上的低手续费体验，也让我直观理解了 Layer2 对以太坊扩展性的价值。通过 Safe 多签，我意识到合约本身也可以作为“账户”与其他协议交互，这是可组合性很重要的一点。

最后，合规和安全的内容让我对现实边界保持清醒：技术开发本身问题不大，但一旦涉及发币、交易或资金承诺，风险会迅速放大。整体来看，这一周最大的变化不是多学了多少工具，而是开始用机制和约束的角度去理解 Web3，这对之后做 DeFi 和合约开发会更有用。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->













虽然之前已经接触并使用过 NFT，但通过这次重新实现一个最小化的 ERC-721 合约，我对 NFT 的核心结构和设计边界进行了更系统的梳理。

在实现过程中，我再次确认 NFT 的本质并不是图片或内容本身，而是一条链上的所有权记录。合约真正负责的是tokenId的唯一性、NFT 与钱包地址之间的归属关系，以及通过tokenURL关联到链下的元数据。图片、音频等内容只是被引用的外部资源，而不是 NFT 的核心。

此外，完整走了一遍合约部署、mint NFT 以及在区块浏览器中验证资产归属的流程，也再次加深了我对 Web3 特性的理解：资产直接绑定在地址上，而不是依赖某一个平台或应用存在。这种特性，在简单 NFT 这个场景下被体现得非常直接。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->














这几天的学习让我对 Web3 的理解更上了一层。

一开始重新理解了区块链本身：它不是某种神奇的数据结构，而是一个去中心化的网络系统。节点可以失效，但系统仍然运作。真正解决的问题不是“存数据”，而是在不互信的情况下如何协作，而智能合约本质上就是一个可验证的 escrow。

在 Day 3，通过创建新钱包、注册 ENS、使用 Uniswap，我第一次真正感受到“链上身份”。ENS 不只是好记的名字，而是一个链上 NFT，把人类可读的身份和地址绑定，并能在 Uniswap、Zapper 等不同协议中直接被识别，身份不再属于某个 App。

同时我也更清楚地理解了 Uniswap：  
它不是订单簿，而是基于x\*y=k 的 AMM。价格来自流动性池的储备比例。  
Factory 负责创建 Pair，Pair 保存流动性并执行 swap，Router 负责帮用户处理路径、数量和滑点。普通用户几乎一定通过 Router 交互，而直接和 Pair 交互更多出现在套利、MEV 或清算合约中。

Day 4 让我厘清了 NFT 的本质：  
NFT 不是图片，而是所有权记录。ERC-721 合约只记录 tokenId 属于谁，图片和元数据要么在 IPFS，要么直接在链上生成。IPFS 的内容寻址解释了不可篡改，而链上生成型 NFT增强了可组合性。

Day 5 更偏“实战体验”。我理解了 Gas、Nonce，以及 MetaMask 的 speed up / cancel 本质是用相同 nonce + 更高 gas 覆盖旧交易。在 Optimism 上体验到极低手续费，也让我直观理解了 Layer2 对以太坊扩展性的意义。Safe 多签则让我看到，合约本身也可以像“用户”一样与协议交互，这是 Web3 可组合性的关键。

同时，合规部分也让我对现实边界更清醒：  
在中国语境下，技术开发可以，金融行为风险极高。发币、交易、返利、资金盘都有明确前车之鉴，长期能活下来的项目一定是合规优先。

总结下来：  
最大的变化不是学了多少工具，而是开始用“协议和机制”的视角理解 Web3，而不是把它当成一堆应用在用。这一点对后面做 DeFi 和合约开发非常关键。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->















### **Day 3 — ENS、身份与 Uniswap**

这一节通过创建新钱包账户、注册 ENS 和在 Uniswap 上换币，让我第一次真正理解了 Web3 的“身份”和“资产跟随性”。ENS 不只是一个好记的名字，而是一个链上 NFT，它把人类可读的身份（如 sanfordstout.eth）和钱包地址绑定在一起，并且在 Uniswap、Zapper 等不同应用中自动被识别。用 Uniswap 把 ETH 换成 DAI 时，我也理解了去中心化交易所并不是订单簿，而是由智能合约和流动性池自动定价运行的，这让我对 DeFi 的运作方式有了更直观的认识。

* * *

### **Day 4 — NFT、IPFS 与链上数据**

这一节通过 OpenSea、Rarible 和直接在 Etherscan 上铸造 NFT，我理解了 NFT 的本质是“所有权记录”，而不是图片本身。ERC-721 合约只是记录了哪个 tokenId 属于哪个地址，图片和元数据要么存储在 IPFS，要么直接存在链上。IPFS 的内容寻址机制让我明白了为什么好的 NFT 是不可篡改的，而链上 NFT（如用 SVG 生成）则进一步增强了可组合性，让其他智能合约也能读取这些数据来构建游戏或应用。

* * *

### **Day 5 — Gas、Layer2 与多签钱包**

这一节让我体验了成为“以太坊超级用户”的真实感觉，包括交易卡住、Nonce、加速和取消交易的底层原理。我理解了为什么 Gas 出价太低会让交易卡在 mempool，以及 MetaMask 的 speed up 和 cancel 实际上是在用相同 nonce 发更高 Gas 的交易覆盖旧交易。同时在 Optimism 上体验到极低的手续费，也让我理解了 Layer2 对以太坊扩展性的意义。最后通过 Safe 多签钱包，我看到了比单一私钥更安全的资产管理方式，也理解了智能合约可以像“用户”一样去调用 Uniswap 等协议，这正是 Web3 可组合性的体现。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
















## 1\. Web3 合规

中国对加密的态度很明确：  
**技术可以，金融不行。**

写区块链、做开发没问题，但一旦碰到 **发币、交易、融资、返利**，就可能触发刑事风险，比如非法集资、传销、洗钱、开设赌场等。

真实案例已经很多：

-   EOS 赌博平台 → 开设赌场罪
    
-   USDT 换汇 → 非法经营
    
-   矿机资金盘 → 非法吸存
    
-   PlusToken → 传销
    
-   用币跨境转钱 → 洗钱
    

从业建议：  
看项目白皮书和 Token 模型有没有**承诺收益、拉人头、回购托底**；  
工资如果是「RMB + Token」要特别小心；  
出金别碰来源不清的币。

海外也在加强监管（MiCA、SEC、FATF），方向是：**合规才能活**。

* * *

## 2\. 以太坊基础

以太坊只有两种账户：

-   **EOA**：有私钥，能发交易（像银行卡）
    
-   **合约**：有代码，被调用才执行（像自动售货机）
    

合约通常用 **Proxy** 升级：  
用户调代理，代理用 `delegatecall` 跑逻辑，换逻辑就能升级。

ERC20、NFT 本质都是合约里的**记账表**，没有“币”，只有数据。

* * *

## 3\. 安全

链上常见问题：  
重入、权限、外部调用、预言机、Gas DoS。  
用 CEI、ReentrancyGuard、AccessControl、SafeERC20、TWAP/Chainlink。

现实中更危险的是：  
假面试、假空投、剪贴板劫持、社工诈骗。  
**硬件钱包 + 核对地址**比任何技术都重要。

开发用 Foundry / Hardhat，  
安全靠测试 + 人工审计 + 源码验证。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

















在day1的视频里比较抓住我眼球的是，视频首先介绍了blockchain的概念.What is blockchain from high level? It’s a system set up without any central control ad thousands of people from the world can communicate p2p. And of course it has many issues like gas and wallet issues. It’s kinda like linked list but its more about the network where all the nodes are participating together. If any of nodes got knocked out the whole network still continues working.

其次介绍了blockchain中trust的概念，然后引用到了escrow这个term。首先提出了一个问题。How to to make a transaction for two parties if they don’t trust or don’t like each other. So in web3 the concept of escrow is executed by smart contract like a middle layer.

最后又介绍了一点ethereum app的一些例子。  
在day2的视频里还是介绍了一些基础知识，像metamask wallet setup,Menemonics的安全问题还有如何在testnet上面进行转账。  
  
Uniswap v2本质上是基于公式x\*y=k的一种链上自动做市商机制，和传统的做市商提供流动性不同，没有订单簿。每一个交易pair有两种资产分别是reserve0,reserve1,价格永远是price=reserve0/reserve1​。本质上一共有三个重要的合约，factory,pair,router.  
Factory只做一件事情就是负责创建部署并且记录Pair.  
function getPair(address tokenA, address tokenB) external view returns (address);

function createPair(address tokenA, address tokenB) external returns (address);  
Factory不参与交易，不存Liquidity，不计算价格。

第二个合约是Pair，负责保存真实流动性，执行swap更新价格和铸造Lp token。Pair会计算拿走多少，检查x\*y是否满足k，更新reserves。

第三个合约是Router，算作一个用户和Uniswap之间的交易操作层，可以用来提供流动性或者移除流动性，就是所谓的加池子减池子，他只把用户的意图拆解成一连串Pair的操作，比如swapExactTokensForTokens(amountIn, amountOutMin, \[A, B, C\])被调用的时候Router会自动找到Pair(A,B) and Pair(B,C),然后调用getReserves进行计算报价然后调用Pair.swap()。

不过一般用户都是和Router交互而不是和Pair，因为如果直接调用Pair.swap你需要自己计算复杂的数据比如amount0Out.amount1Out,自己计算slippage，保证x\*y不被破坏，自己计算路径，一旦有一个数据出错就会revert。而这些复杂的工作都会被Router做，比如我要swapExactTokensForTokens(100, 95, \[ETH, USDC, DAI\], ...)，Router会自动帮我找池子然后计算getAmountsOut，读取reserves，检查滑点。所以Router对于普通的用户极为友好。如果想要完全取得控制权，像金额，路径，gas的话需要与Pair直接交互，一般用于MEV bot或者套利合约，清算引擎等。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



















今天系统地学习了《Web3 实习手册》的「入门导读」部分，并通读了《精通以太坊》第 1 和第 2 章。虽然我之前就学习过区块链的底层原理，但当时更多是碎片化的了解。这次是比较沉下心来重新梳理了一遍，不仅把旧知识重新夯实，也补充了不少之前忽略的细节比如共识层和执行层机制。  
明天计划对 Uniswap V2 的源码重新做一次更深入、逐行级别的阅读，加深对 AMM 与链上流动性机制的理解。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
