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
