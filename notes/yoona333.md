---
timezone: UTC+8
---

# yoona333

**GitHub ID:** yoona333

**Telegram:** @yoona1020

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
1.  看币圈资讯、深潮、区块律动等平台，了解 Web3 行业动态
    
2.  参与线上 Co-Learning、AI 及其基础概念分享会
    
3.  学习以太坊协议
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

1.  √ 学习 AI 原生开发工作流
    
2.  √ 看币圈资讯、深潮、区块律动等平台，了解 Web3 行业动态
    
3.  √ 参与线上 Co-Learning、Web3 安全、合规分享会
    
4.  √ 学习 以太坊协议
    

**下文所说的nftdemo文件，地址在这里哦** [https://github.com/yoona333/NFTdemo](https://github.com/yoona333/NFTdemo)

前提：

使用Metamask里面的Sepolia测试网络。（如果Sepolia测试网络里面没有币，可以去[https://faucets.chain.link/](https://faucets.chain.link/) 这个网站里领取，每隔12小时领取0.25SepoliaETH。）  
[大家可以看我的这篇 CSDN。](https://blog.csdn.net/yoona333/article/details/138653349?spm=1001.2014.3001.5501)

**如何领取SepoliaETH币方法**：

**打开**[**https://faucets.chain.link/网页。**](https://faucets.chain.link/%E7%BD%91%E9%A1%B5%E3%80%82)

1、点击连接钱包

2、点击“我同意”、连接小狐狸。​

你会发现，这里已经连接上了，轻轻触碰这个图标，会像图中所示。你也可以点开这个图标，会显示已连接到该网站。​编辑​

3、这个地址是metamask里面账户的地址、点击0.25 test ETH​

4、点击登录 login GitHub，没有GitHub账号，请注册一个。在“确认您是真人”的前面方框那 里 点击一下，就会出现勾勾。，再点击send request。 ​

5、等这个蓝色线到达最后，再点击close。再去你的 metamask 账户上查看是否有增加Sepolia测试币。就可以领取啦。（每12小时可以领取一次哟）​编辑​

**一、将合约代码导入remix 中（记得环境选择这个哟**

**Remix**    [**https://remix.ethereum.org/**](https://remix.ethereum.org/)

​编辑）

**这里有两种方法，第一种可能有些同学使用不了，可以试试第二种。**

**第一种方法：**

1、首先进入remix中，在首页有个home，点击GitHub。就会出现一个框框。​编辑​

就是这样的框框，这里面输入这一串网址，你可以点开来看一下，是GitHub上的代码，把它导入到remix中去。[https://github.com/ConsenSys/Tokens/blob/fdf687c69d998266a95f15216b1955a4965a0a6d/contracts/eip20/EIP20.sol](https://github.com/ConsenSys/Tokens/blob/fdf687c69d998266a95f15216b1955a4965a0a6d/contracts/eip20/EIP20.sol) ​编辑​

导入之后，会有这些，点开看**EIP20.sol文件**，会发现报红，因为这个文件中还导入了**EIP20Interface.sol**文件，所以我们接下来导入此文件。 ​编辑​

2、导入EIP20Interface.sol文件，同样的方法，在home里面，点击GitHub，就会出现框框，输入

[https://github.com/Consensys/Tokens/blob/fdf687c69d998266a95f15216b1955a4965a0a6d/contracts/eip20/EIP20Interface.sol](https://github.com/Consensys/Tokens/blob/fdf687c69d998266a95f15216b1955a4965a0a6d/contracts/eip20/EIP20Interface.sol)

就可以发现多了一个代码，记得点开这两个sol文件，它会自动多出来json文件。编译版本记得改。 ​

**第二种方法：直接自己在remix里面新建一个文件夹叫class（自己随便取名）里面再自己手动创建两个文件（注意后缀名为.sol），分别对应刚刚两个导入的文件名称，再点进去那两个链接，讲里面的合约内容分别复制到刚刚创建的文件夹里面去。效果也是一样的。**

**二、导入好了代码之后，我们还要导入一个文件夹，这里面就是关于NFT的文件。**

点击这个图标，代表的是上传文件夹。

​

**三、打开这个**[**https://www.pinata.cloud/**](https://www.pinata.cloud/) **网页。进入这个网页需要Vpn，科学上网。**

Pinata（用户可以在Pinata上注册账户，创建存储桶，并上传图片等文件。上传完成后，Pinata会生成一个唯一的CID（内容标识符），用于标识文件在IPFS网络中的位置，并生成一个可永久访问的链接，用户可以将此链接分享给他人，使他们能够访问在IPFS网络上存储的内容

1、进入之后，点击Get created。 ​

2、如果没有帐户，就选择Create one，创建一个账户。​

3、填写好自己的信息之后，点击Sign up。注意自己的邮箱，会发送信息让你确认。最好在电脑上登录自己的邮箱。如果注册后，登不上去，可以刷新页面，再登录试试。​

4、登录之后是这样的。然后我们再点击这个upload，上传一张照片。

5、可以自己给它取名字，为了方便好记。然后点开这个图片，这个上面就是图片的网址。

**四、打开remix，在remix中找到刚刚导入的nftdemo文件夹里面的 nft-metadata.json。将双引号里面的网址改成刚刚图片的网址，记得保存哈。现在在自己的此电脑里找到netdemo文件夹，找到里面的nft-metadata.json，看下有没有改成照片的网址，如果没变，请手动改。**

**五、再次点开pinata的网页，然后点击upload，将刚刚的nft-metadata.json文件也上传上去。**

**六、接下来，我们开始部署合约了。打开remix，找到nftdemo下的MyNFT.sol文件。**

**七、点击deploy部署，小狐狸就会自动跳出来，然后点击确认。注意看这个gas值，如果太高的话，就尽量白天部署，因为我们和国外是有时差的，这样会便宜很多。**

**八、点击确认之后，稍等一会，桌面右下角会弹出消息提示，然后左下角箭头这里，出来了部署的合约，我们把这个合约展开。**

\*\*九、找到这个mintNFT方法，然后根据图片上的提示，将参数填入进去。\*\*​

这个是我上传的json文件的网址（url）

**十、点击交易之后，又会跳出metamask，然后也是要注意消耗的燃料费用。**

成功之后会有消息提示。

\*\*十一、接下来，打开小狐狸，找到这个收藏品，点击导入NFT。\*\*​

点击导入之后，会出现这个，然后看着图片，将信息填好之后，点击导入。

**十二、导入之后，会发现收藏品里面多了东西。**

\*\*十三、点开这个NFT，然后进去之后，再点击三个点点，再选择在Opensea上查看。\*\*​

最后的效果就是这样子哦，我也不是一次就出来的，换了好几张图片，操作了很多遍才成功哒。

\*\*最后希望我的笔记可以帮助到大家！\*\*​
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->




1.  √ 学习UI UX Pro Max的使用
    
2.  √ 学习 AI 原生开发工作流
    
3.  √ 看币圈资讯、深潮、区块律动等平台，了解 Web3 行业动态
    
4.  √ 参与线上 Co-Learning、Web3 运行原理
    

整体的 UI 设计还可以，布局、配色有一定的提升，在 [SKILL.md](http://SKILL.md) 文件中，也会有怎么操作，去写提示词。后续在项目中，可以用这个进行整体的风格统一优化

下面是我使用的提示词：

请为我生成一个吵架陪练器网站

产品类型：consumer web tool

行业语境：communication / conflict resolution / social

风格关键词：modern, clean, minimal, friendly, playful accents

技术栈：React + Tailwind

产品定位： 把“想怼但不想翻车”的对话，变成三套可直接发送的回应：强硬赢面 / 体面收场 / 搞笑反杀。

目标用户：

-   20-35 岁高频社交沟通人群（同事/对象/客户/家人/群聊）
    
-   容易在冲突中“要么憋着要么爆炸”的用户
    

输出语言规则：

-   默认中文输出。
    
-   如果用户输入主要为英文，则输出英文（自动判断）
    

核心价值主张（页面必须体现）：

-   30 秒生成：可复制、可直接发
    
-   降温优先：即使强硬也不升级矛盾
    
-   风险提示：告诉你“发出去会发生什么”
    
-   可替换变量：让你把话说得更像“你”
    

功能：

即时生成：输入完点按钮，2秒内出结果

一键复制：每条回应旁边有复制按钮，点完显示"已复制"

快捷操作：回车发送、Ctrl+1/2/3快速复制对应模式

示例提示：输入框hover显示吵架例句，降低使用门槛

整体的 UI 设计还可以，布局、配色有一定的提升，在 [SKILL.md](http://SKILL.md) 文件中，也会有怎么操作，去写提示词。后续在黄金河上，可以用这个进行整体的风格统一优化

下面是我使用的提示词：

请为我生成一个吵架陪练器网站

产品类型：consumer web tool

行业语境：communication / conflict resolution / social

风格关键词：modern, clean, minimal, friendly, playful accents

技术栈：React + Tailwind

产品定位： 把“想怼但不想翻车”的对话，变成三套可直接发送的回应：强硬赢面 / 体面收场 / 搞笑反杀。

目标用户：

20-35 岁高频社交沟通人群（同事/对象/客户/家人/群聊）

容易在冲突中“要么憋着要么爆炸”的用户

输出语言规则：

默认中文输出。

如果用户输入主要为英文，则输出英文（自动判断）

核心价值主张（页面必须体现）：

30 秒生成：可复制、可直接发

降温优先：即使强硬也不升级矛盾

风险提示：告诉你“发出去会发生什么”

可替换变量：让你把话说得更像“你”

功能：

即时生成：输入完点按钮，2秒内出结果

一键复制：每条回应旁边有复制按钮，点完显示"已复制"

快捷操作：回车发送、Ctrl+1/2/3快速复制对应模式

示例提示：输入框hover显示吵架例句，降低使用门槛

![img_v3_02ts_9f0248c5-c1c3-4998-80fd-ca9c2c8bd50g.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yoona333/images/2026-01-13-1768318153897-img_v3_02ts_9f0248c5-c1c3-4998-80fd-ca9c2c8bd50g.jpg)![img_v3_02ts_77c08a7b-2cc8-44f4-a3e2-b6481921720g.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yoona333/images/2026-01-13-1768318229677-img_v3_02ts_77c08a7b-2cc8-44f4-a3e2-b6481921720g.jpg)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->






1.  √ 学习巩固 Uniswap v2 源码
    
2.  × 撰写有一定信息含量的学习总结（发布 Twitter 或者个人博客）
    
3.  √ 阅读 Web3 行业新闻，深潮，区块律动，星球日报等平台
    
4.  √ 参加以太坊中文周会、线上一小时Co-Learning、Web3 行业全局介绍 & 岗位概览
    

一、项目结构 & 合约关系

Uniswap V2 项目通常拆分为两个仓库：

`uniswap-v2-core`：核心合约

`uniswap-v2-periphery`：外围设计，用户交互方便用的合约，比如 router

**主要合约：**

Uniswap V2 的核心合约主要有三个：

`UniswapV2Factory`

工厂合约，用来创建和管理交易对（Pair）合约。管理所有流动性池的 "工厂"

关键点：

-   部署新交易对（如 USDC/ETH）。
    
-   存储所有已部署的交易对地址。
    
-   提供查询接口（如 getPair() 返回特定 token 对应的 Pair 地址）。
    

`UniswapV2Pair`

每个交易对都会有一个独立的 Pair 合约，用于真正存储资金、计算定价和执行 swap、add/remove liquidity。每个池子一个 Pair，所有交易都发生在 Pair 合约里。

关键点：

-   保留了 token0 和 token1 的储备量。
    
-   实现核心恒定乘积公式：x \* y = k。
    
-   实现 swap()、mint()（增加流动性）、burn()（移除流动性）等方法。
    

`UniswapV2Router02`

是一个提供更友好的交互接口的合约，让用户或 DApp 更方便地：添加/移除流动性、进行 token 之间的 swap。是 DApp/前端最常直接交互的合约。

关键点：

-   内部帮你调用 Pair 的 swap/mint/burn。
    
-   会计算 slippage、最小金额、路径等，简化操作。
    

**典型的流程解释：**

以用户通过 Router 添加流动性为例：

**解释：**

用户只和 Router 交互，不直接调用 Pair 或 Factory。

Router 内部先去 Factory 查询 Pair 地址，然后去 Pair 完成交互。

如果 Pair 不存在，先调用 Factory 创建。

Pair 是实际存储 token 和发放 LP Token 的地方。

**假设场景**

你想在 Uniswap V2 上添加一个 USDC/ETH 的流动性池，也就是让别人可以方便地在 USDC 和 ETH 之间交易。

✅第 1 步：你发起请求

你在 DApp/前端（网页）点了「添加流动性」按钮。这个操作其实是要调用链上的一个合约方法。

✅ 第 2 步：前端调用 Router

前端帮你调用合约 UniswapV2Router02.addLiquidity(...)。Router 是 Uniswap 官方写好的合约，帮忙处理各种计算（比如最小价格、滑点、路径等），让用户不需要自己和底层合约打交道。

✅ 第 3 步：Router 去找池子（Pair 合约）

Router 想添加流动性，必须知道这个池子的合约地址在哪里。所以 Router 会先问 UniswapV2Factory.getPair(tokenA, tokenB)：

“你有没有 USDC 和 ETH 的池子？”Factory 是一个工厂合约，里面只管理所有 Pair（池子）的地址。

✅ 第 4 步：如果池子还没创建

如果这是第一个想要创建 USDC/ETH 池子的人，那 Factory 会返回一个空地址（意思是没有）。Router 就需要先让用户调用 UniswapV2Factory.createPair(tokenA, tokenB)，去链上创建一个新的池子（Pair 合约）。

✅ 第 5 步：拿到 Pair 地址后，Router 把用户的 token 转进去

用户想放多少 USDC 和多少 ETH，前端把这些信息告诉 Router。Router 调用 transferFrom 等方式，把这些 token 转到 Pair 合约里。

✅ 第 6 步：Router 调用 Pair 的 mint() 函数

mint() 是 Pair 合约的方法，用来给用户铸造 LP Token（流动性凭证）。LP Token 的作用：以后当你想撤回资金时，就用 LP Token 去 Pair 合约里 burn，取回原来的 token + 一部分手续费收益。

✅ 第 7 步：用户得到 LP Token

添加流动性完成，你拿到 LP Token。别人就可以在这个池子里自由交换 USDC ↔ ETH，你也能从手续费里获得收益。

**来个比喻方便理解**

`Factory` = 房地产开发商，建楼（创建池子）

`Pair` = 楼本身（池子），里面放钱、交易

`Router` = 中介公司，帮忙跑流程、算价钱、带你找楼

**简单总结**

Router：帮你计算、帮你调用，不存钱

Factory：只管理/创建池子

Pair：池子本体，真正存钱，负责 swap、mint、burn

**二、创建合约实例**

```
// SPDX-License-Identifier: BSD-3-Clause-Clear

pragma solidity ^0.8.0;

import "@uniswap/v2-periphery/contracts/interfaces/IUniswapV2Router02.sol";

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";contract UniswapV2Integration {

    address private constant ROUTER = 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D;

    IUniswapV2Router02 public uniswapV2Router;

    constructor() {

        uniswapV2Router = IUniswapV2Router02(ROUTER);

    }

}
```

**三、增加流动性：成为流动性提供者**

`addLiquidity()` 函数允许你为交易对提供流动性，并从每笔交易中赚取费用。

```
function addLiquidity(

    address tokenA,

    address tokenB,

    uint amountADesired,

    uint amountBDesired,

    uint amountAMin,

    uint amountBMin,

    address to,

    uint deadline

) external returns (uint amountA, uint amountB, uint liquidity);
```

**参数分解**

-   tokenA/tokenB：要配对的代币的合约地址
    
-   amountADesired/amountBDesired：你期望的存款金额
    
-   amountAMin/amountBMin：最小金额（滑点保护）
    
-   to：接收 LP 代币的地址
    
-   deadline：交易过期时间戳
    

**实际实现**

```
function addLiquidityToPool(

    address tokenA,

    address tokenB,

    uint256 amountA,

    uint256 amountB

) external {

    // Approve router to spend tokens

    // 授权路由器消费代币

    IERC20(tokenA).approve(address(uniswapV2Router), amountA);

    IERC20(tokenB).approve(address(uniswapV2Router), amountB);

    // Add liquidity with 5% slippage tolerance

    // 增加流动性，滑点容忍度为 5%

    uniswapV2Router.addLiquidity(

        tokenA,

        tokenB,

        amountA,

        amountB,

        amountA * 95 / 100,  // 5% slippage protection

        amountB * 95 / 100,  // 5% slippage protection

        msg.sender,          // LP tokens to caller

        // LP 代币给调用者

        block.timestamp + 300 // 5 minute deadline

                              // 5 分钟截止时间

    );

}
```

**内部机制：addLiquidity() 如何工作**

交易对创建检查

```
if (IUniswapV2Factory(factory).getPair(tokenA, tokenB) == address(0)) {

    IUniswapV2Factory(factory).createPair(tokenA, tokenB);

}
```

**基于储备金的计算**

对于现有的交易对，该函数维护价格比率：

```
amountBOptimal = amountADesired.mul(reserveB) / reserveA;

if (amountBOptimal <= amountBDesired) {

    (amountA, amountB) = (amountADesired, amountBOptimal);

} else {

    amountAOptimal = amountBDesired.mul(reserveA) / reserveB;

    (amountA, amountB) = (amountAOptimal, amountBDesired);

}
```

**CREATE2 地址计算**

Uniswap V2 使用确定性地址来提高 gas 效率：

```
pair = address(uint(keccak256(abi.encodePacked(

    hex'ff',

    factory,

    keccak256(abi.encodePacked(token0, token1)),

    hex'96e8ac4277198ff8b6f785478aa9a39f403cb768dd02cbee326c3e7da348845f'

))));
```

**移除流动性：退出你的仓位**

removeLiquidity() 函数允许你通过销毁 LP 代币来提取你的代币：

```
function removeLiquidity(

    address tokenA,

    address tokenB,

    uint liquidity,

    uint amountAMin,

    uint amountBMin,

    address to,

    uint deadline

) external returns (uint amountA, uint amountB);
```

**实现示例**

```
function removeLiquidityFromPool(

    address tokenA,

    address tokenB,

    uint256 liquidityAmount

) external {

    address pair = IUniswapV2Factory(factory).getPair(tokenA, tokenB);

    // Approve router to spend LP tokens

    // 授权路由器消费 LP 代币

    IERC20(pair).approve(address(uniswapV2Router), liquidityAmount);

    // Remove liquidity

    // 移除流动性

    uniswapV2Router.removeLiquidity(

        tokenA,

        tokenB,

        liquidityAmount,

        0, // Accept any amount of tokenA

           // 接受任何数量的 tokenA

        0, // Accept any amount of tokenB

           // 接受任何数量的 tokenB

        msg.sender,

        block.timestamp + 300

    );

}
```

**代币互换：DeFi 交易的核心**

swapExactTokensForTokens() 函数能够实现精确的代币兑换：

```
function swapExactTokensForTokens(

    uint amountIn,

    uint amountOutMin,

    address[] calldata path,

    address to,

    uint deadline

) external returns (uint[] memory amounts);
```

**高级 Swap 实现**

```
function swapTokens(

    address tokenIn,

    address tokenOut,

    uint256 amountIn,

    uint256 slippagePercent

) external {

    // Approve router

    // 授权路由器

    IERC20(tokenIn).approve(address(uniswapV2Router), amountIn);

    // Calculate minimum output with slippage

    // 通过滑点计算最小输出

    address[] memory path = new address[](2);

    path[0] = tokenIn;

    path[1] = tokenOut;

    uint[] memory amountsOut = uniswapV2Router.getAmountsOut(amountIn, path);

    uint amountOutMin = amountsOut[1] * (100 - slippagePercent) / 100;

    // Execute swap

    // 执行兑换

    uniswapV2Router.swapExactTokensForTokens(

        amountIn,

        amountOutMin,

        path,

        msg.sender,

        block.timestamp + 300

    );

}
```

**多跳兑换**

对于没有直接交易对的代币，Uniswap 通过中间代币进行路由：

```
function multiHopSwap(uint256 amountIn) external {

    address[] memory path = new address[](3);

    path[0] = USDC_ADDRESS;  // Start with USDC

                               // 从 USDC 开始

    path[1] = WETH_ADDRESS;  // Route through WETH

                               // 通过 WETH 路由

    path[2] = DAI_ADDRESS;   // End with DAI

                               // 以 DAI 结束

    IERC20(USDC_ADDRESS).approve(address(uniswapV2Router), amountIn);

    uniswapV2Router.swapExactTokensForTokens(

        amountIn,

        0, // Calculate proper minimum in production

           // 在生产中计算适当的最小值

        path,

        msg.sender,

        block.timestamp + 300

    );

}
```

**四、完整的集成示例**

这是一个全面的合约，演示了所有主要的 Uniswap V2 交互：

```
// SPDX-License-Identifier: BSD-3-Clause-Clear

pragma solidity ^0.8.0;

import "@uniswap/v2-periphery/contracts/interfaces/IUniswapV2Router02.sol";

import "@uniswap/v2-core/contracts/interfaces/IUniswapV2Factory.sol";

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

import "@uniswap/v2-core/contracts/interfaces/IUniswapV2Pair.sol";

contract UniswapV2Manager {

    IUniswapV2Router02 public immutable uniswapV2Router;

    IUniswapV2Factory public immutable uniswapV2Factory;

    constructor() {

        uniswapV2Router = IUniswapV2Router02(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);

        uniswapV2Factory = IUniswapV2Factory(0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6f);

    }

    function addLiquidity(

        address tokenA,

        address tokenB,

        uint256 amountA,

        uint256 amountB,

        uint256 slippage

    ) external returns (uint256, uint256, uint256) {

        IERC20(tokenA).transferFrom(msg.sender, address(this), amountA);

        IERC20(tokenB).transferFrom(msg.sender, address(this), amountB);

        IERC20(tokenA).approve(address(uniswapV2Router), amountA);

        IERC20(tokenB).approve(address(uniswapV2Router), amountB);

        uint256 amountAMin = amountA * (100 - slippage) / 100;

        uint256 amountBMin = amountB * (100 - slippage) / 100;

        return uniswapV2Router.addLiquidity(

            tokenA,

            tokenB,

            amountA,

            amountB,

            amountAMin,

            amountBMin,

            msg.sender,

            block.timestamp + 300

        );

    }

    function removeLiquidity(

        address tokenA,

        address tokenB,

        uint256 liquidity,

        uint256 slippage

    ) external returns (uint256, uint256) {

        address pair = uniswapV2Factory.getPair(tokenA, tokenB);

        IERC20(pair).transferFrom(msg.sender, address(this), liquidity);

        IERC20(pair).approve(address(uniswapV2Router), liquidity);

        // Get current reserves to calculate minimums

        // 获取当前储备金以计算最小值

        (uint256 reserveA, uint256 reserveB,) = IUniswapV2Pair(pair).getReserves();

        uint256 totalSupply = IERC20(pair).totalSupply();

        uint256 amountAMin = (reserveA  liquidity / totalSupply)  (100 - slippage) / 100;

        uint256 amountBMin = (reserveB  liquidity / totalSupply)  (100 - slippage) / 100;

        return uniswapV2Router.removeLiquidity(

            tokenA,

            tokenB,

            liquidity,

            amountAMin,

            amountBMin,

            msg.sender,

            block.timestamp + 300

        );

    }

    function swapExactTokensForTokens(

        address tokenIn,

        address tokenOut,

        uint256 amountIn,

        uint256 slippage

    ) external returns (uint256[] memory) {

        IERC20(tokenIn).transferFrom(msg.sender, address(this), amountIn);

        IERC20(tokenIn).approve(address(uniswapV2Router), amountIn);

        address[] memory path = new address[](2);

        path[0] = tokenIn;

        path[1] = tokenOut;

        uint256[] memory amountsOut = uniswapV2Router.getAmountsOut(amountIn, path);

        uint256 amountOutMin = amountsOut[1] * (100 - slippage) / 100;

        return uniswapV2Router.swapExactTokensForTokens(

            amountIn,

            amountOutMin,

            path,

            msg.sender,

            block.timestamp + 300

        );

    }

}
```
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
