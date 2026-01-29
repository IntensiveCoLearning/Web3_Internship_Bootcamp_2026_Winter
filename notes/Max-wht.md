---
timezone: UTC+8
---

# Max

**GitHub ID:** Max-wht

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-29
<!-- DAILY_CHECKIN_2026-01-29_START -->
头脑风暴中。多人协作很吃交流啊
<!-- DAILY_CHECKIN_2026-01-29_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->

大概做一个Polymarket做生息的DeFi吧。首先搞明白Polymarket的对外接口，然后想想price oracle怎么弄，差不多了。前端应该就3mins的事情(对Agent来说。。)
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->


**time:** 2026:: 01/26 -> 02/01

**Description:** 我打算做一个Solidity Contests的汇总网站，类似于 [DailyWarden](https://www.dailywarden.com/) 。列出当前正在进行的Competitive Audit和即将到来的Audit的同时，我打算让AI综合对应项目的SLOC和对应推特帖子的数量和讨论度，给出一个全面评价，主要涉及的方面有 1.难度 2.类型 3.参与热度 \[我觉得这是一个创新点，目前市场上是没有同类型的产品的\]。这个项目的目的是为了方便合约安全审计工程师合理安排比赛时间和任务，节约时间，成为所有安全审计人员的必备工具。
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->



### **\[S-1\] Use BytesMask for more efficient storage**

**Description:** `BytesMasking` is a technique to pack multiple values into a single storage slot (usually taking uint256 -> 32 bytes == 256 bits) to save gas, instead of using separate storage slots for each variable (such as struct)

💹Example in Panoptic

```js
PACKING RULES FOR A MARKETSTATE:
From the LSB to the MSB:
(0) borrowIndex          80 bits : Global borrow index in WAD (starts at 1e18). 2**80 = 1.75 years at 800% interest
(1) marketEpoch          32 bits : Last interaction epoch for that market (1 epoch = block.timestamp/4)
(2) rateAtTarget         38 bits : The rateAtTarget value in WAD (2**38 = 800% interest rate)
(3) unrealizedInterest   106 bits : Accumulated unrealized interest that hasn't been distributed (max deposit is 2**104)
Total                    256 bits  : Total bits used by a MarketState.
```

The `MarketState` packs 4 values into 1 storage slot:

|<--0-79-->|<--80-111-->|<--112-149-->|<--150-155-->|

| 80 Bits | 32 Bits | 32 Bits | 106 Bits |

💹How to use this powerful skill?

Example: The `MarketState` packs 4 values into 1 storage slot:

|<--0-79-->|<--80-111-->|<--112-149-->|<--150-155-->|

| 80 Bits | 32 Bits | 38 Bits | 106 Bits |

First, define the mask we need. In this case, rateAtTarget is required

```js
TARGET_RATE_MASK = ((1 << 38) - 1) << 112;
// creates: 111...111(38 ones) at position 112-149
// 0x...3FFFFFFFFF000000000000000000000000000
```

Then, we use Yul to load specific storage

-   Write Value by mask
    

```js
MarketState self,
uint40 newRate

assembly{
    //clear bits 112-149
    let cleared := and(self, not(TARGET_RATE_MASK));
    // ...000..000 (38 zeros in position)

    //2. Mask the input to ensure it fits 38 bits
    // uint40 -> we have to ignore the top 2 bits
    // 0011 1111 1111 ....  1111 1111
    let safeRate := and(newRate, 0x3FFFFFFFFF);


    let result := or(cleared,shl(112, safeRate));
}

```

-   Read Value by mask
    

```js
MarketState self

assembly{
    // push->[xxx ...|         RATE          |]
    //               0011 1111 .... 1111 1111
    //               &&&& &&&& &&&& &&&& &&&&
    let result := and(shr(112,self), 0x3FFFFFFF);
}

```

  

**Benefits:**

1.  Gas Savings: SSTORE (~20k gas) vs multiple SSTORE
    
2.  Atomic Update: All values update together
    
3.  Cache Efficiency: Reading multiple values costs less
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->




**推特写了个文章**

[https://x.com/0xMax2557/status/2014938927061471348](https://x.com/0xMax2557/status/2014938927061471348)

### \[B-4\]JIT Attack

**Description:** `JIT`指`Just In Time`，攻击者在某个关键状态即将生效前或刚生效时，瞬时介入（进入 / 退出 / 修改状态），从而获得不成比例的收益或特权，且几乎不承担对应风险或成本。本质是收益与风险的不对称。下面我举几个例子：

**AMM:** 在DEX的语境下，JIT常常指beforeswap添加大量流动性，afterswap移除流动性，从中赚取Swap Fee的同时不用承担风险。攻击者对交易进行排序获得优势。

**FLASH LOAN:** 在借贷的语境下，同上。在一个闪电贷前提供资金，赚取手续费而不承担风险。

总而言之，造成JIT的原因在于合约没有对于流动性做监控。如何防止JIT呢？

**TimeLock:** 对于新加入的流动性，需要Lock一段时间才能参与收益的分配，类似Euler。

**Before Distribution:** 将受益的分配前置。在Uniswap V4的Hook中可以设置 beforeAddliquidity，让攻击者的流动性在添加的时候，旧流动性都能分配给LP持有者。在swap进行的时候，所有的累积收益已经分配完，新流动性无法套利。(Uniswap V3没有Hook，所以很被JIT攻击)

**Linear Vesting:** 让收益的分配不只看"钱"，而是用时间来加权，这样可以保证JIT攻击加入的流动性的权重是很低的，无法套利。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->






今天坐了一天高铁，准备明天开始赶进度，好多分享会都错过了。这几天实在忙，今天就讲一讲最近在审计的VII-Finance。这是一个Uniswap生态的衍生Defi，用来将Uniswap V3和V4的token接入Euler借贷系统，通过将NFT转化成ERC6909 (ERC20的组合)，解决Uniswap V3和V4 用来代表LP的NFT不能被传统借贷Defi接收的问题。Euler的`Etherium Vault Connection`架构允许了LP Tokne作为抵押，借贷任何Euler体系中的代币。

为什么Euler借贷不兼容ERC721，也就是非同质化代币。我认为主要的原因在于相比于ERC20，ERC721是非同质化的，如果计算ERC20 Vault一个address的代币"价值" (比如用美元为标准的价值) 的时间复杂度是O(1)，那么ERC721就是O(n)，二者的差别很大。

在区块链的视角下，时间复杂度越高意味着Gas Fee越多。达到一个上线就会造成DOS。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->







### \[T-2\] 1/21/2026

这三天一直在做`VII-Finance`的audit，一共差不多花了八小时，现在应该是进行到了1/2，还剩几个合约没有review。这算我第一次的审计，很激动的，希望我的Finding都能通过。但是这几天还要加急"复习"数字电路，其实这学期这个课我一节没听，有些难！这两件事叠加在一起导致实习计划的很多colearning和分享会没听到。。。23号回家重新看录播吧
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->








### \[B-3\] ISO-4217

ISO 4217 is the international standard defining three-letter alphabetic and three-digit numeric codes for representing currencies, funds, and precious metals, used globally in banking, trade, and finance to avoid ambiguity (e.g., USD for US Dollar, EUR for Euro). It provides clarity by linking countries (first two letters, often from [ISO 3166](https://www.google.com/search?q=ISO+3166&oq=ISO+4217&gs_lcrp=EgZjaHJvbWUqBwgAEAAYgAQyBwgAEAAYgAQyBwgBEAAYgAQyBwgCEAAYgAQyBwgDEAAYgAQyBwgEEAAYgAQyBwgFEAAYgAQyBwgGEAAYgAQyBwgHEAAYgAQyBwgIEAAYgAQyBwgJEAAYgATSAQc3MzJqMGo3qAIAsAIA&sourceid=chrome&ie=UTF-8&mstk=AUtExfB1DwnqVHHfWw032QSB61ys9eh0hnigvB9s_iOFVn2ar_kZXd8aLrIkYLd7Khmko-tn25KR7sLmbTCwm0xoCc671fEBfRv3PA0CNQ4bWEWTlJ_hKRBT8_LHbxnDec4NxOkRoiUqQXonByOe4UkxZJlPxArzbObyrPaQXsZ18XT-2dY&csui=3&ved=2ahUKEwia1qfkvJmSAxU-ho4IHXyLAxcQgK4QegQIARAB)) with their specific currency (third letter), and also details minor currency units and gold (XAU) or silver (XAG).

这是个国际货币代号。Euler中使用这个来代表法定货币单位。比如USD

```
0x0000000000000000000000000000000000000348 address
```

在开发中不能把这个address当作一个ERC20来用。(刚刚看一个Defi源码，把这个当作erc20使了)
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->









### \[ETH Panda周会\]

快讯

-   以太坊基础设施基本完成，需要开始走向去中性化世界。是否意味着下一波Dapp风潮
    
-   BNB Chain升级，finalize小于1s
    
-   市场正在进入一个由政策信号主导的新阶段
    
-   黑客攻击增加
    
-   starknet只有市值没有交易量
    
-   avalanche的c-chain活跃性达到历史新高
    

以太坊新EIP

下次升级主要feature: BAL/ePBS

-   EIP7778
    
-   EIP7708 ETH转账会发送log
    
-   EIP7843 SLOTNUM Opcode 通过block时间戳计算slot。
    
-   EIP8024 加入SWAPn和DUPn Opcode
    

Tempo链稳定币支付

稳定币绕过中间商(银行，汇率差)直接交付，在跨国交易上有优势

-   实时结算
    
-   低成本
    
-   可追踪
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->










### \[N-4\] Just jotting down some thoughts in 1/18/2026

昨天找了一个competitve audit参加。过程并没有我想象中的顺利，昨天前前后后花了三个小时，整个audit的进度推进并不大，整个codebase知识密度太大了，很多我都不了解，不过我知道自己正在正确的成长，一周之后我一定可以交出一份令我满意的report。

想想我为什么会加入到Web3这个圈子，很直白的原因是互联网大厂太卷，准备实习不比高考轻松，还不如考研，我也不喜欢考研，我不喜欢考试。二五年五月我记得很清楚，学长们都在准备大厂面试，整个工作室都弥漫着焦虑、紧张和迷茫，虽然我不需要实习，那时我才大一，但是这种氛围让我窒息，也是这个时候了解到了web3，当时我通过媒体了解到的消息，Web3看来这是一个远离压力充满希望的桃花源。

那时我的Web3学习之路启程了，我一直保持一个学习者的角色到现在。

我没有大家那么"社会化"，周五的例会大家都侃侃而谈，谈论Web3的治理、传统金融、讨论工作… 但是这些方面我都很不清楚。我觉得我是个"局外人"，觉得我还只是个"小孩"，而你们都是"大人"。很难要求一个十九岁的年轻人知道这么多社会上的东西，就算知道了也没有体会过。我也只出于技术的热爱所以加入Web3，而Web3之于金融，Web3之于政府，甚至Web3之于人类历史，我都觉得很懵。我只觉得这是一片技术的蓝海，我能从开源的代码中学习到行业的所有知识，我能知道市面上所有DeFi的源码和技术细节，我能自由探索没有限制，这是我热爱Web3而不是Web2的原因，还有一部分是因为Web3有像ETHPanda和LXDAO这样充满活力和热爱的社区，我能感受到归属感，真的很赞。

这半年的"旅程"很充实，就像人生的锚点，回忆会一直清晰，感受会愈发强烈。我会一直前进，无论多少困难都不会使我低头，我有这样的自信"still I rise."，未来我不会是一个无名小辈，与君共勉。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->












### **\[UNIV3-3\] Swap In UniswapV3Pool.sol**

**Discription:** 通过`swap-router-contract__UniswapV3Router::exactInputSingle`用户可以与`v3-core`进行交互。提供四种方式，exactInput,exactInputSingle,exactOutput,exactOutputSingle

✅ Exersice

```js
// SPDX-License-Identifier: MIT
pragma solidity 0.8.24;

import {Test, console2} from "forge-std/Test.sol";
import {IERC20} from "../../../src/interfaces/IERC20.sol";
import {IWETH} from "../../../src/interfaces/IWETH.sol";
import {ISwapRouter} from "../../../src/interfaces/uniswap-v3/ISwapRouter.sol";
import {DAI, WETH, WBTC, UNISWAP_V3_SWAP_ROUTER_02} from "../../../src/Constants.sol";

contract UniswapV3SwapTest is Test {
    IWETH private weth = IWETH(WETH);
    IERC20 private dai = IERC20(DAI);
    IERC20 private wbtc = IERC20(WBTC);
    ISwapRouter private router = ISwapRouter(UNISWAP_V3_SWAP_ROUTER_02);
    uint24 private constant POOL_FEE = 3000;

    function setUp() public {
        deal(DAI, address(this), 1000 * 1e18);
        dai.approve(address(router), type(uint256).max);
    }

    // Exercise 1
    // - Swap 1000 DAI for WETH on DAI/WETH pool with 0.3% fee
    // - Send WETH from Uniswap V3 to this contract
    function test_exactInputSingle() public {
        uint256 wethBefore = weth.balanceOf(address(this));

        // Write your code here
        // Call router.exactInputSingle
        uint256 amountOut = router.exactInputSingle(
            ISwapRouter.ExactInputSingleParams({
                tokenIn: DAI,
                tokenOut: WETH,
                fee: POOL_FEE,
                recipient: address(this),
                amountIn: 1000 * 1e18,
                amountOutMinimum: 1,
                sqrtPriceLimitX96: 0
            })
        );

        uint256 wethAfter = weth.balanceOf(address(this));

        console2.log("WETH amount out %e", amountOut);
        assertGt(amountOut, 0);
        assertEq(wethAfter - wethBefore, amountOut);
    }

    // Exercise 2
    // Swap 1000 DAI for WETH and then WETH to WBTC
    // - Swap DAI to WETH on pool with 0.3% fee
    // - Swap WETH to WBTC on pool with 0.3% fee
    // - Send WBTC from Uniswap V3 to this contract
    // NOTE: WBTC has 8 decimals
    function test_exactInput() public {
        // Write your code here
        // Call router.exactInput
        bytes memory path = abi.encodePacked(
            DAI,
            uint24(3000),
            WETH,
            uint24(3000),
            WBTC
        );
        uint256 amountOut = router.exactInput(
            ISwapRouter.ExactInputParams({
                path: path,
                recipient: address(this),
                amountIn: 1000 * 1e18,
                amountOutMinimum: 1
            })
        );

        console2.log("WBTC amount out %e", amountOut);
        assertGt(amountOut, 0);
        assertEq(wbtc.balanceOf(address(this)), amountOut);
    }

    // Exercise 3
    // - Swap maximum of 1000 DAI to obtain exactly 0.1 WETH from DAI/WETH pool with 0.3% fee
    // - Send WETH from Uniswap V3 to this contract
    function test_exactOutputSingle() public {
        uint256 wethBefore = weth.balanceOf(address(this));

        // Write your code here
        // Call router.exactOutputSingle
        uint256 amountIn = router.exactOutputSingle(
            ISwapRouter.ExactOutputSingleParams({
                tokenIn: DAI,
                tokenOut: WETH,
                fee: POOL_FEE,
                recipient: address(this),
                amountOut: 0.1 * 1e18,
                amountInMaximum: 1000 * 1e18,
                sqrtPriceLimitX96: 0
            })
        );

        uint256 wethAfter = weth.balanceOf(address(this));

        console2.log("DAI amount in %e", amountIn);
        assertLe(amountIn, 1000 * 1e18);
        assertEq(wethAfter - wethBefore, 0.1 * 1e18);
    }

    // Exercise 4
    // Swap maximum of 1000 DAI to obtain exactly 0.01 WBTC
    // - Swap DAI to WETH on pool with 0.3% fee
    // - Swap WETH to WBTC on pool with 0.3% fee
    // - Send WBTC from Uniswap V3 to this contract
    // NOTE: WBTC has 8 decimals
    function test_exactOutput() public {
        // Write your code here
        // Call router.exactOutput
        bytes memory path = abi.encodePacked(
            WBTC,
            uint24(3000),
            WETH,
            uint24(3000),
            DAI
        );
        uint256 amountIn = router.exactOutput(
            ISwapRouter.ExactOutputParams({
                path: path,
                recipient: address(this),
                amountOut: 0.01 * 1e8,
                amountInMaximum: 1000 * 1e18
            })
        );

        console2.log("DAI amount in %e", amountIn);
        assertLe(amountIn, 1000 * 1e18);
        assertEq(wbtc.balanceOf(address(this)), 0.01 * 1e8);
    }
}
```
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->













### **\[UNIV-3\] Math In UniswapV3**

**Discription:** 在 uniswapv3 中，代币池中 x，y 或者说 tokne0 和 token1 的数量不能直白地如 uniswapv2 那样"xy = L^2"表示出来。相反，uniswapv3 通过追踪 price 和 liquidity 来计算代币的数量

YX=P_XY_​=_P_

XY=L2_XY_\=_L_2

LP=X_P_​_L_​=_X_

LP=Y_LP_​=_Y_

现在我们已经知道如何用过 price 和 liquidity 来表示 X 和 Y 了。现在还有一个问题就是在 uniswap V3 中，X 代表着 real resource 和 virtual resource 志和，如何计算 real resource？

l:lower h:higher

x=LPl−LPh_x_\=_Pl_​​_L_​−_Ph_​​_L_​

y=LPh−LPl_y_\=_LPh_​​−_LPl_​​
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->














### **\[UNI-V3-2\] Spot Price**

**Discription:** 在 UniswapV3 中如何计算 SpotPrice 现货价格？

✅SLOT0 in Pair

```js
struct Slot0 {
        // the current price
        uint160 sqrtPriceX96;
        // the current tick
        int24 tick;
        // the most-recently updated index of the observations array
        uint16 observationIndex;
        // the current maximum number of observations that are being stored
        uint16 observationCardinality;
        // the next maximum number of observations to store, triggered in observations.write
        uint16 observationCardinalityNext;
        // the current protocol fee as a percentage of the swap fee taken on withdrawal
        // represented as an integer denominator (1/x)%
        uint8 feeProtocol;
        // whether the pool is locked
        bool unlocked;
    }
```

注意到 Slot0 中有两个参数`sqrtPriceX96`和`tick`。 任意知道这两个变量的其中一个就可以计算出 Price。

✅Calculate Price By Tick

**Discription:** 用 Tick 表示 Price 也叫离散价格模型。在 Uniswap V3 中，tick 是一个 整数，价格(token1 / token0)按固定比例离散化：

price(tick)=1.0001 tickprice(_tick_)=1.0001_tick_

其中

-   priceprice 表示 token1 / token0
    
-   tick∈Z_tick_∈Z
    

每一个 tick 约等于`0.01%`的价格变化

假设我们已经知道了 WETH/USDT[^weth/usdt\_pool](https://etherscan.io/address/0x4e68ccd3e89f51c3074ca5072bbac773960dfa36#readContract)池的 tick

通过 etherscan 可以看到如下信息

```txt
The 0th storage slot in the pool stores many values, and is exposed as a single method to save gas when accessed externally.

 sqrtPriceX96 uint160, tick int24, observationIndex uint16, observationCardinality uint16, observationCardinalityNext uint16, feeProtocol uint8, unlocked bool

[ slot0 method Response ]
  sqrtPriceX96   uint160 :  4586418891309846984317130
  tick   int24 :  -195150
  observationIndex   uint16 :  55
  observationCardinality   uint16 :  150
  observationCardinalityNext   uint16 :  150
  feeProtocol   uint8 :  102
  unlocked   bool :  true
```

```python
# Weth = 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
# USDT = 0xdAC17F958D2ee523a2206206994597C13D831ec7
# Weth is token0 and USDT is token1
tick = -194624

# p repercent token0 in terms of token1 (1 weth = p usdt)
#      Raw_Amount_USDT(token1)
# p = ----------------------
#      Raw_Amount_WETH(token0)

p = 1.0001 ** tick

decimal_usdt = 6
decimal_weth = 18

price = p * (10 ** (decial_weth - decimal_usdt))

```

换一个例子，假设我们在 Weth/UDSC 池子里

```python

# Weth = 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
# USDC = 0xA0...
# USDC is token0 and WETH is token1
tick = 194609

# p repercent token0 in terms of token1 (1 udsc = p weth)
#      Raw_Amount_WETH(token1)
# p = ----------------------
#      Raw_Amount_USDC(token0)

p = 1.0001 ** tick

decimal_weth = 18
decimal_usdc = 6

#          raw_amount_token1          decimal_token0(1e6)
# price = ----------------------- X --------------------------
#          raw_amount_token0          decimal_token1(1e18)
price = p * (10 ** (decial_usdc - decimal_weth))

price2 = 1/price
```

price(tick)=1.0001 tickprice(_tick_)=1.0001_tick_

-   tick < 0 : 代表着 token1 更少更稀有
    
-   tick > 0 : 代表着 token0 更少更稀有
    

✅Calculate Price By SqrtPriceX96

**Discription:** 在 Uniswap V3 中，池子的核心价格状态不是直接存 price，而是存：

sqrtPriceX96=P1P0⋅296sqrtPriceX96=_P_0​_P_1​​​⋅296

其中：

现在有 WETH / USDT pool

```python

sqrt_q_96  = 4586418891309846984317130
Q96 = 2 ** 96

p = (sqrt_q_96/Q96) ** 2

price = p / 1e6 * 1e18

```

另一个需要知道的是`tick`和`sqrt_q_96`的相互转化

💹SqrtQ96 To Tick

P=1.0001tick=(sqrtPriceX96Q96)2_P_\=1.0001tick=(_Q_96sqrtPriceX96​)2

tick=2⋅log⁡ ⁣(sqrtPriceX96Q96)log⁡(1.0001)tick=log(1.0001)2⋅log(_Q_96sqrtPriceX96​)​

```python
Q96 = 2 ** 96
sqrt_p_x96 = 1386025840740905446350612632896904
tick = 195402

t = 2 * math.log(sqrt_p_x96 / Q96) / math.log(1.0001)
```

结果发现整数位都是一样的，最后取整数位 tick 相同

✅Exersice

### **\[UNIV-3\] Math In UniswapV3**

**Discription:** 在 uniswapv3 中，代币池中 x，y 或者说 tokne0 和 token1 的数量不能直白地如 uniswapv2 那样"xy = L^2"表示出来。相反，uniswapv3 通过追踪 price 和 liquidity 来计算代币的数量

YX=P_XY_​=_P_

XY=L2_XY_\=_L_2

LP=X_P_​_L_​=_X_

LP=Y_LP_​=_Y_

现在我们已经知道如何用过 price 和 liquidity 来表示 X 和 Y 了。现在还有一个问题就是在 uniswap V3 中，X 代表着 real resource 和 virtual resource 志和，如何计算 real resource？

l:lower h:higher

x=LPl−LPh_x_\=_Pl_​​_L_​−_Ph_​​_L_​

y=LPh−LPl_y_\=_LPh_​​−_LPl_​​
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
















### **\[UNIV3-1\] Introduction of Uniswap V3**

**Discription:** 对于 UniswapV2，所有的流动性都集中在一个 Pair 中，AMM 方程如下

$$  
P\_{X/Y} = \\frac{Y}{X}  
$$

  

只有当 L^2 足够大，这个 Pair 才可以说够“坚固”，不然很容易被恶意操控。既然如此，Uniswap V3 的出发点就是能否直接通过很小的资金来代表很大的流动性。于是 Uniswap V3 的核心`Concentrated Liquidity`就诞生了，其目的就是允许一对 Pair 可以在指定价格区间进行 swap，换句话说就是将流动性集中在指定价格区间。只有在指定价格区间的流动性才是真流动性`real reserves`,非区间的流动性都是虚拟流动性`virtual reserves`

**Diff between V2 and V3:** 在V2中，我们通过X和Y的存量来计算流动性和Price，但是在V3中我们通过流动性和Price来计算X和Y。Active Liqituity用ERC712来表示，而不是ERC20。Swap Fee也存在区别，V2是固定0.3%，但是V3有四种不同的计费规则。TWAP的计算也会有区别

![uniswapV3OverView.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Max-wht/images/2026-01-13-1768322676813-uniswapV3OverView.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

















### **\[N-2\] Phased Plan from 1/12 to 2/8 in 2026**

**Discription:** 我的目标岗位是合约审计。这个岗位门槛真的很高，而且是和合约开发高度耦合的。一个优秀的审计员无疑也是一个优秀的开发者，二者需要的知识储备，技术栈都是相似的。

这四周我需要学习市面上主流Defi的源码，学习里面优秀的设计模式，以及其他Dapp如何接入这些Defi protocal

-   uniswap V3
    
-   uniswap V4
    
-   Aave V4
    
-   GMX
    

一周学一个吧，前两周允许我进度慢一点，还在期末考试中。除此之外继续跟进实习计划的任务，目标是保持在排行榜的前面，也希望能在实习结束接到一个开发的实习。目前我Solidity的代码量还是太少了，还是没能做到随心所欲的地步，需要一些开发来逐渐精通。

这一段时间应该是参加不了Competitve Audit了，不过这些比赛我今后长期活跃在其中。还有黑客松，我也会经常参加。最近还觉得需要在社交媒体中建立自己的权威，比如在推特上，这个方向我也会关注，不过不是现在这四周的重心。

我能感觉到现在是我人生的一个小小新阶段的起步。未来会专注于合约审计和开发并行学习，我觉得自己还是需要开发的经验作为积累，二者并行的学习模式可能是最适合现阶段的我的。

### **\[UNIV2-3\] TWAP (time weight everage price) in UniswapV2**

**Discription:** 在使用 Uniswap 这种链上 Oracle 最为 price 来源的时候，很容易(100%)会受到攻击，原因就在于 Uniswap 的价格太好操控了，任何一个人做 FlashLoan 就可以让价格波动很大。由此 Uniswap 提供`TWAP`(time weight everage price)来防止价格波动。注意，TWAP 价格和现货价格是两个东西。

**Math:**

-   **Spot Price 现货价格(AMM)**
    
    -   Token X 以 Token Y 计价的现货价格:
        
        P\_X/Y=YX\_P\_\__X_/_Y_\=\_XY\_​
        
-   **TWAP 价格**
    
    -   Token X 在时间区间 i 到 k 的时间加权平均价格
        
        TWAPX(Tk,Tn)=∑i=kn−1ΔTi PiTn−TkTWAP\_X\_​(\_Tk\_​,\_Tn\_​)=\_Tn\_​−_Tk\_​\_i_\=_k_∑_n_−1​Δ\_Ti\_​\_Pi\_​​
        

💹 \_update in pair

```js
function _update(uint balance0, uint balance1, uint112 _reserve0, uint112 _reserve1) private {
        require(balance0 <= uint112(-1) && balance1 <= uint112(-1), 'UniswapV2: OVERFLOW');
        uint32 blockTimestamp = uint32(block.timestamp % 2 ** 32);
        uint32 timeElapsed = blockTimestamp - blockTimestampLast; // overflow is desired
        if (timeElapsed > 0 && _reserve0 != 0 && _reserve1 != 0) {
            // * never overflows, and + overflow is desired
            price0CumulativeLast += uint(UQ112x112.encode(_reserve1).uqdiv(_reserve0)) * timeElapsed;
            price1CumulativeLast += uint(UQ112x112.encode(_reserve0).uqdiv(_reserve1)) * timeElapsed;
        }
        reserve0 = uint112(balance0);
        reserve1 = uint112(balance1);
        blockTimestampLast = blockTimestamp;
        emit Sync(reserve0, reserve1);
    }
```

💹 How to use TWAP in your dapp

```js
// SPDX-License-Identifier: MIT
pragma solidity >=0.4 <0.9;

import {IUniswapV2Pair} from "../../../src/interfaces/uniswap-v2/IUniswapV2Pair.sol";
import {FixedPoint} from "../../../src/uniswap-v2/FixedPoint.sol";

// Modified from https://github.com/Uniswap/v2-periphery/blob/master/contracts/examples/ExampleOracleSimple.sol
// Do not use this contract in production
contract UniswapV2Twap {
    using FixedPoint for *;

    // Minimum wait time in seconds before the function update can be called again
    // TWAP of time > MIN_WAIT
    uint256 private constant MIN_WAIT = 300;

    IUniswapV2Pair public immutable pair;
    address public immutable token0;
    address public immutable token1;

    // Cumulative prices are uq112x112 price * seconds
    uint256 public price0CumulativeLast;
    uint256 public price1CumulativeLast;
    // Last timestamp the cumulative prices were updated
    uint32 public updatedAt;

    // TWAP of token0 and token1
    // range: [0, 2**112 - 1]
    // resolution: 1 / 2**112
    // TWAP of token0 in terms of token1
    FixedPoint.uq112x112 public price0Avg;
    // TWAP of token1 in terms of token0
    FixedPoint.uq112x112 public price1Avg;

    // Exercise 1
    constructor(address _pair) {
        // 1. Set pair contract from constructor input
        pair = IUniswapV2Pair(_pair);
        // 2. Set token0 and token1 from pair contract
        token0 = pair.token0();
        token1 = pair.token1();
        // 3. Store price0CumulativeLast and price1CumulativeLast from pair contract
        price0CumulativeLast = pair.price0CumulativeLast();
        price1CumulativeLast = pair.price1CumulativeLast();
        // 4. Call pair.getReserve to get last timestamp the reserves were updated
        (, , updatedAt) = pair.getReserves();
        //    and store it into the state variable updatedAt
    }

    // Exercise 2
    // Calculates cumulative prices up to current timestamp
    //@note 这个函数计算并返回截止到当前时间戳的累积价格，用于后续计算时间加权平均价格。
    function _getCurrentCumulativePrices()
        internal
        view
        returns (uint256 price0Cumulative, uint256 price1Cumulative)
    {
        // 1. Get latest cumulative prices from the pair contract
        price0Cumulative = pair.price0CumulativeLast();
        price1Cumulative = pair.price1CumulativeLast();
        // If current block timestamp > last timestamp reserves were updated,
        // calculate cumulative prices until current time.
        // Otherwise return latest cumulative prices retrieved from the pair contract.

        // 2. Get reserves and last timestamp the reserves were updated from
        //    the pair contract
        (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast) = pair
            .getReserves();

        // 3. Cast block.timestamp to uint32, and update the timestamp of the last update
        uint32 blockTimestamp = uint32(block.timestamp);
        if (blockTimestampLast != blockTimestamp) {
            // 4. Calculate elapsed time
            uint32 dt = blockTimestamp - blockTimestampLast;

            // Addition overflow is desired
            unchecked {
                // 5. Add spot price * elapsed time to cumulative prices.
                //    - Use FixedPoint.fraction to calculate spot price.
                //    - FixedPoint.fraction returns UQ112x112, so cast it into uint256.
                //    - Multiply spot price by time elapsed
                price0Cumulative +=
                    uint256(FixedPoint.fraction(reserve1, reserve0)._x) *
                    dt;
                price1Cumulative +=
                    uint256(FixedPoint.fraction(reserve0, reserve1)._x) *
                    dt;
            }
        }
    }

    // Exercise 3
    // Updates cumulative prices
    function update() external {
        // 1. Cast block.timestamp to uint32
        uint32 blockTimestamp = uint32(block.timestamp);
        // 2. Calculate elapsed time since last time cumulative prices were
        //    updated in this contract
        uint32 dt = blockTimestamp - updatedAt;
        // 3. Require time elapsed >= MIN_WAIT
        require(dt >= MIN_WAIT, "InsufficientTimeElapsed");

        // 4. Call the internal function _getCurrentCumulativePrices to get
        //    current cumulative prices
        (
            uint256 price0Cumulative,
            uint256 price1Cumulative
        ) = _getCurrentCumulativePrices();

        // Overflow is desired, casting never truncates
        // https://docs.uniswap.org/contracts/v2/guides/smart-contract-integration/building-an-oracle
        // Subtracting between two cumulative price values will result in
        // a number that fits within the range of uint256 as long as the
        // observations are made for periods of max 2^32 seconds, or ~136 years
        unchecked {
            // 5. Calculate TWAP price0Avg and price1Avg
            //    - TWAP = (current cumulative price - last cumulative price) / dt
            //    - Cast TWAP into uint224 and then into FixedPoint.uq112x112
            price0Avg = FixedPoint.uq112x112(
                uint224(price0Cumulative - price0CumulativeLast) / dt
            );
            price1Avg = FixedPoint.uq112x112(
                uint224(price1Cumulative - price1CumulativeLast) / dt
            );
        }

        // 6. Update state variables price0CumulativeLast, price1CumulativeLast and updatedAt
        price0CumulativeLast = price0Cumulative;
        price1CumulativeLast = price1Cumulative;
        updatedAt = blockTimestamp;
    }

    // Exercise 4
    // Returns the amount out corresponding to the amount in for a given token
    function consult(
        address tokenIn,
        uint256 amountIn
    ) external view returns (uint256 amountOut) {
        // 1. Require tokenIn is either token0 or token1
        require(tokenIn == token0 || tokenIn == token1, "InvalidToken");
        // 2. Calculate amountOut
        //    - amountOut = TWAP of tokenIn * amountIn
        //    - Use FixePoint.mul to multiply TWAP of tokenIn with amountIn
        //    - FixedPoint.mul returns uq144x112, use FixedPoint.decode144 to return uint144
        if (tokenIn == token0) {
            // Example
            //   token0 = WETH
            //   token1 = USDC
            //   price0Avg = avg price of WETH in terms of USDC = 2000 USDC / 1 WETH
            //   tokenIn = WETH
            //   amountIn = 2
            //   amountOut = price0Avg * amountIn = 4000 USDC
            amountOut = FixedPoint.mul(price0Avg, amountIn).decode144();
        } else {
            amountOut = FixedPoint.mul(price1Avg, amountIn).decode144();
        }
    }
}
```
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




















### **\[B-1\] Etherscan::Transaction**

**Description** 对于ETH来说，区分交易的类别是很重要的。

**\[Transaction type classification\]**

-   **🛜Type0 \[Legacy Transaction\]**
    
    最早的以太坊交易形式，使用 **单一 Gas Price**，没有 Base Fee / Priority Fee 的概念，手续费 = `Gas Used × Gas Price`，现在仍然**兼容**，但不推荐使用。这也是很多链下服务出bug的一个点，协议太老了，不适配新协议。下面的表格着重看gasPrice和gasLimit
    
    | 字段 | 说明 |
    | --- | --- |
    | gasPrice | 固定 gas 单价 |
    | gasLimit | Gas 上限 |
    | nonce | 交易序号 |
    | to | 接收地址 |
    | value | ETH 数量 |
    | data | 合约数据 |
    
-   🛜 **Type1 \[Access List Transaction (EIP-2930)\]**
    
    核心概念是AccessList，这个会直接声明要访问的数据
    
    ```
    [
      {
        address: 0xContractA,
        storageKeys: [slot1, slot2, ...]
      },
      {
        address: 0xContractB,
        storageKeys: [...]
      }
    ]
    ​
    ```
    
-   🛜 **Type 2：EIP-1559 Transaction (主流)**
    
    引入 **Base Fee（销毁）**，引入 **Priority Fee（矿工小费）**，自动退还多余 Gas
    
    ```
    effectiveGasPrice =
    min(
      maxFeePerGas
      baseFee + maxPriorityFeePerGas
    )
    ```
    
    | 字段 | 含义 |
    | --- | --- |
    | maxFeePerGas | 你愿意支付的最高 Gas |
    | maxPriorityFeePerGas | 给矿工的小费 |
    | baseFee | 网络自动决定 |
    
    相当于原来的`gasPrice`被拆分成了`maxFeePerGas`和`maxPrioityFeePerGas`，实际的gas Fee
    
-   🛜 **Type 3：Blob Transaction（EIP-4844 / Proto-Danksharding）**
    
    2024年引入，面向layer2，数据放在blob中，隔一段时间主网会删除Blob
    
    Rollup（如 Arbitrum、Optimism）提交数据，数据放在 **Blob** 中，而不是 calldata，极低的数据成本，不直接参与 EVM 执行，专为扩容设计。给 Rollup（如 Arbitrum、Optimism）提交数据，数据放在 **Blob** 中，而不是 calldata。极低的数据成本，不直接参与 EVM 执行，专为扩容设计
    

# **\[Uniswap Introduction\]**

## **\[Uniswap V2\]**

### **\[UNIV2-1\] 为什么需要两个 codebase？**

**Discription:** uniswap V2 有两个仓库，`v2-core`和`v2-periphery`。区分二者的重点在于面向对象的不同。v2-core 是核心，里面包含了 pool 的创建，token swap 逻辑，其中的 function 普通用户是用不上的。v2-periphery 专门用用来与用户交互。

### **\[UNIV2-2\] Swap Token in V2**

**Discription:** `v2-periphery`提供接口给用户 swapTokens，分别是`UniswapV2RouterV2::swapExactTokensForTokens()`以及`UniswapV2RouterV2::swapTokensForExactTokens()` .

-   swapExactTokenForToken() 目的是通过 exactInput -> 计算出 calculated output，然后交易。 举个例子，我有 1WETH，我要用这 1WETH 去兑换 DAI，在这个场景下我不知道我能兑换多少 DAI，但是我会提供指定的 WETH。由于兑换出的 DAI 是未知数，uniswapv2 提供了滑点保护(slip protection)用于抵抗 MEV,说人话就是我(用户)可以指定一个最低兑换 DAI 的数量，如果小于这个数就放弃。
    
-   swapTokenForExactToken() 目的是通过 exactOutput -> 计算出 calculated input，然后交易。
    

​

​

<summary>💹Swap?TokenFor?Token</summary>

```
function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external virtual override ensure(deadline) returns (uint[] memory amounts){
        ...
​
        _swap(amounts, path, to);
    }
​
function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external virtual override ensure(deadline) returns (uint[] memory amounts){
        ...
​
        _swap(amounts, path, to);
    }
```

**Inspect:** 通过上面两个 function 我们发现，交易并不是 1 对 1 对，而是 1->1->1..->1,我们可以传\[WETH, USDC, DAI\]。这样最终结果还是 WETH -> DAI。出现这种场景是因为没有 WETH/DAI 的 pool，所以只能“绕远路”。 最后我们观察`_swap(...)`这个函数，入参的 path 和 to 都是有的，amounts 是哪里来的？ 答案我省略了 hhh，这个 amounts 其实就是 path\[i\]->path\[i+1\]的钱(正向，也就是 exactTokenforToken) ||path\[i\]<-path\[i+1\]的钱(逆向，也就是 TokenforExactToken),这其中的钱 uniswap 会计算好。

</details> </details>

​

<summary>💹\_swap in Router</summary>

```
function _swap(uint[] memory amounts, address[] memory path, address _to) internal virtual {
        for (uint i; i < path.length - 1; i++) {
            // index i ->path[i]::path[i+1]
            // input = path[i], output = path[i + 1]
            (address input, address output) = (path[i], path[i + 1]);
            // in Uniswap Pair, the smaller address of token will be regarded as token0
            (address token0, ) = UniswapV2Library.sortTokens(input, output);
            // token0 is the smaller address of the two tokens
            uint amountOut = amounts[i + 1];
            // if input is the smaller one, then inputOutcom e is 0; if input != the smaller one, thus the logic is token1 swap token0
            (uint amount0Out, uint amount1Out) = input == token0 ? (uint(0), amountOut) : (amountOut, uint(0));
            // next pair or receiver address
            address to = i < path.length - 2 ? UniswapV2Library.pairFor(factory, output, path[i + 2]) : _to;
            IUniswapV2Pair(UniswapV2Library.pairFor(factory, input, output)).swap(
                amount0Out,
                amount1Out,
                to,
                new bytes(0)
            );
        }
    }
```

**Inspect:** `UniswapV2RouterV2::_swap`这个循环调用 v2-core 的 swap 函数，这也是为什么说`periphery`面向用户，核心的状态改变都在`v22-core`中

</details> </details>

​

​

<summary>💹swap in Pair</summary>

```
function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external lock {
        require(amount0Out > 0 || amount1Out > 0, 'UniswapV2: INSUFFICIENT_OUTPUT_AMOUNT');
​
        //_reserve0 = X0, _reserve1 = Y0
        (uint112 _reserve0, uint112 _reserve1, ) = getReserves(); // gas savings
        //amount0Out = token0 output
        //amount1Out = token1 output
        require(amount0Out < _reserve0 && amount1Out < _reserve1, 'UniswapV2: INSUFFICIENT_LIQUIDITY');
​
        uint balance0;
        uint balance1;
        //scope for _token{0,1}, avoids `stack too deep` errors
        {
            address _token0 = token0;
            address _token1 = token1;
            require(to != _token0 && to != _token1, 'UniswapV2: INVALID_TO');
            if (amount0Out > 0) _safeTransfer(_token0, to, amount0Out); // optimistically transfer tokens
            if (amount1Out > 0) _safeTransfer(_token1, to, amount1Out); // optimistically transfer tokens
            if (data.length > 0) IUniswapV2Callee(to).uniswapV2Call(msg.sender, amount0Out, amount1Out, data);
            balance0 = IERC20(_token0).balanceOf(address(this));
            balance1 = IERC20(_token1).balanceOf(address(this));
        }
        //      In         after  >   before  -  transfer  ?  after   - (   before - transfer  ) : 0
        //`after > before - transfer` means the token0Pool received tokens so the input is not 0
        uint amount0In = balance0 > _reserve0 - amount0Out ? balance0 - (_reserve0 - amount0Out) : 0;
        uint amount1In = balance1 > _reserve1 - amount1Out ? balance1 - (_reserve1 - amount1Out) : 0;
        require(amount0In > 0 || amount1In > 0, 'UniswapV2: INSUFFICIENT_INPUT_AMOUNT');
        {
            // scope for reserve{0,1}Adjusted, avoids stack too deep errors
            uint balance0Adjusted = balance0.mul(1000).sub(amount0In.mul(3));
            uint balance1Adjusted = balance1.mul(1000).sub(amount1In.mul(3));
            require(
                // Invariant x*y=L^2
                balance0Adjusted.mul(balance1Adjusted) >= uint(_reserve0).mul(_reserve1).mul(1000 ** 2),
                'UniswapV2: K'
            );
        }
​
        _update(balance0, balance1, _reserve0, _reserve1);
        emit Swap(msg.sender, amount0In, amount1In, amount0Out, amount1Out, to);
    }
```

**Inspect:** 在一个 Pair 的合约实例中，两个 token 需要按照 address 的地址排序，排序成 token0 和 token1。入参的 amount0Out 和 amount1Out 有一个是 0，函数的逻辑会计算 amount1In 和 amount0In。在 swap 的过程中会收取 0.3%的手续费

</details> </details>

由此,swap 的逻辑简单梳理了一遍
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
