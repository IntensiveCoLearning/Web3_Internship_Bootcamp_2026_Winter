---
timezone: UTC+11
---

# Max

**GitHub ID:** Linwentao-tech

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
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
