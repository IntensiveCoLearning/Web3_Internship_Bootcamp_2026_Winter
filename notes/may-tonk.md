---
timezone: UTC+8
---

# may-tonk

**GitHub ID:** may-tonk

**Telegram:** @maytonk

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
# 一、Solidity 核心概念总览表（回温版）

1️⃣ 语言与语法层

| 模块 | 核心概念 | 关键点 | 回温关注点 |
| --- | --- | --- | --- |
| 合约结构 | pragma / contract | ^0.8.x 内置溢出检查 | 构造函数、可见性 |
| 变量 | 状态变量 / 局部变量 | storage 持久化 | gas 成本 |
| 数据类型 | uint / address / bool | 默认 uint256 | 显式类型 |
| 引用类型 | array / mapping / struct | mapping 不能遍历 | storage vs memory |
| 函数 | external / public | calldata 只能 external | ABI 设计 |
| 修饰符 | modifier | 执行顺序 _; | 权限与校验 |

2️⃣ 数据位置与内存模型

| 关键字 | 存储位置 | 是否可修改 | Gas 成本 | 常见错误 |
| --- | --- | --- | --- | --- |
| storage | 链上 | ✅ | 高 | 误以为是拷贝 |
| memory | 函数内 | ✅ | 中 | 生命周期误判 |
| calldata | 入参 | ❌ | 最低 | 用在 public |

3️⃣ 函数与执行控制

| 类型 | 说明 | 你需要记住的点 |
| --- | --- | --- |
| view | 读状态 | 不消耗 gas（call） |
| pure | 不读不写 | 常用于工具函数 |
| payable | 可收 ETH | 必须显式声明 |
| fallback | 无函数匹配 | ETH 接收兜底 |
| receive | 仅收 ETH | 无 calldata |

4️⃣ 调用上下文（msg / tx / block）

| 变量 | 含义 | 安全注意 |
| --- | --- | --- |
| msg.sender | 当前调用者 | 权限判断唯一可靠 |
| msg.value | 转入 ETH | payable |
| msg.data | 原始 calldata | 函数选择器 |
| tx.origin | 交易发起人 | ❌ 不可用于鉴权 |
| block.timestamp | 区块时间 | 可被微调 |

5️⃣ EVM 执行与 Gas

| 行为 | 结果 |
| --- | --- |
| revert | 状态回滚，gas 消耗 |
| require | 条件失败即回滚 |
| 外部调用 | 可能重入 |
| storage 写入 | gas 最贵 |

6️⃣ 常见安全问题总表

| 风险 | 本质 | 防御方式 |
| --- | --- | --- |
| 重入攻击 | 状态未锁 | CEI / ReentrancyGuard |
| 溢出 | 数值越界 | ^0.8.x 自动检查 |
| 授权钓鱼 | approve 滥用 | 限额 / revoke |
| 权限失控 | modifier 错误 | onlyOwner |
| 签名重放 | nonce 缺失 | nonce / domain |

7️⃣ 数字签名

| 步骤 | 说明 |
| --- | --- |
| hash | keccak256 |
| 前缀 | \x19Ethereum Signed Message |
| 恢复 | ecrecover |
| 用途 | permit / 白名单 |

## 复习的相关代码以上传本地的GitHub

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

// 从 Chainlink 导入价格数据接口
import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/shared/interfaces/AggregatorV3Interface.sol";

// 定义一个名为 fundyou 的库（library）
// 库里面封装了一些与价格转换相关的函数
contract fundme{
    uint256 public minnal = 1e18;
    address[] public funders;
    AggregatorV3Interface public datafeed;

    constructor(address datafeedadd){
        datafeed = AggregatorV3Interface(datafeedadd);//进行相关数据的转换
    }

    function fund() public payable{
        require(getconversion(msg.value)>=minnal,"send last 1 ETH");
        funders.push(msg.sender);

    }
    // 获取 ETH / USD 价格
    function getprice() public view returns (uint256) {
        // Chainlink ETH/USD 预言机地址（Sepolia 测试网）
        AggregatorV3Interface pricefeed = datafeed /*AggregatorV3Interface(
            0x694AA1769357215DE4FAC081bf1f309aDC325306)*/;

        // 获取最新一轮价格数据
        // latestRoundData() 返回五个值，我们只需要第二个 price
        (, int256 price, , , ) = pricefeed.latestRoundData();

        // Chainlink 返回的 ETH/USD 价格精度是 8 位小数（即 1e8）
        // 这里乘上 1e10，把它转换成 1e18 的形式（和以太币精度保持一致）
        return uint256(price * 1e10);
    }

    // 把 ETH 数量转换成等值的 USD
    function getconversion(uint256 ethAmount) public view returns (uint256) {
        uint256 ethprice = getprice(); // 获取当前 1 ETH 的美元价格
        // 计算：ETH 数量 × ETH 价格 / 1e18（因为 ETH 的单位是 Wei）
        uint256 ethusd = (ethprice * ethAmount) / 1e18;
        return ethusd; // 返回等值的 USD 金额
    }

    // 获取 Chainlink Aggregator 的版本号
    function getversion() public view returns(uint) {
        AggregatorV3Interface pricefeed = datafeed/*AggregatorV3Interface(
            0x694AA1769357215DE4FAC081bf1f309aDC325306)相关sepolia的接口*/;
        return pricefeed.version();
    }
    

    //外部人员进行直接转账，没有调用合约是触发的一个条件
    receive() external payable {
        fund();
     }

     fallback() external payable{
        fund();
     }

}
```

具体所以代码查看[my\_web3\_study/contracts/fundme.sol at master · may-tonk/my\_web3\_study --- my\_web3\_study/contracts/fundme.sol at master · may-tonk/my\_web3\_study](https://github.com/may-tonk/my_web3_study/blob/master/contracts/fundme.sol)
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

# **order book(订单薄)和AMM(**自动做市商)(采用ChatGPT纠正总结)

## 一、什么是订单簿（Order Book）

**订单簿**是交易所用来实时展示市场买卖意愿的核心工具，本质上反映的是：

> 市场中“谁愿意用什么价格，买 / 卖 多少资产”。

无论是 **Web2 股票市场** 还是 **Web3 加密交易所（如 Binance、OKX、Uniswap v4 的链下撮合部分）**，订单簿的逻辑是相通的。

* * *

## 二、订单簿的三大核心组成

### 1️⃣ 买入订单（Buy Orders / Bids）

-   表示买方**愿意出的价格** + **想买的数量**
    
-   本质是“需求侧”
    
-   价格越高，越靠近成交
    

### 2️⃣ 卖出订单（Sell Orders / Asks）

-   表示卖方**愿意卖的价格** + **卖出的数量**
    
-   本质是“供给侧”
    
-   价格越低，越容易成交
    

### 3️⃣ 成交历史（Order / Trade History）

-   已经发生的真实交易记录
    
-   用来观察：
    
    -   最近成交价
        
    -   成交量
        
    -   市场活跃度
        

* * *

## 三、订单簿的“核心位置”：Top of the Book

**订单簿顶部 = 市场最关键的信息**

-   **最高买价（Highest Bid）**
    
-   **最低卖价（Lowest Ask）**
    

这两者之间的差值叫：

> **点差（Spread）**

📌 含义：

-   点差越小 → 市场越活跃、流动性越好
    
-   点差越大 → 流动性差、滑点风险高（Web3 非常重要）
    

* * *

## 四、订单簿 + K线的配合意义

订单簿通常会配合 **K 线图（Candlestick Chart）** 使用：

-   **K线**：反映“已经发生了什么”（结果）
    
-   **订单簿**：反映“现在市场想做什么”（意图）
    

👉 在 Web3 交易中：

-   K 线 = 历史行为
    
-   订单簿 = 即时博弈状态
    

* * *

## 五、订单簿能帮交易者做什么（实战价值）

### 1️⃣ 判断是谁在推动市场

-   大量小单 → 更像散户
    
-   集中大额订单 → 更像机构 / 做市商 / 巨鲸
    

在加密市场中：

-   很多时候是 **做市商 + 机器人** 在主导
    

* * *

### 2️⃣ 发现短期方向信号（订单失衡）

**订单失衡（Order Imbalance）**：

-   买单明显多于卖单 → 买压大 → 短期偏多
    
-   卖单明显多于买单 → 卖压大 → 短期偏空
    

⚠️ 注意（Web3 特别重要）：

-   订单是可以**随时撤单的**
    
-   存在“假墙 / 钓鱼单”
    

* * *

### 3️⃣ 判断支撑位与阻力位

这是订单簿非常实用的一点：

-   某个价格有**大量买单堆积**
    
    -   → 潜在**支撑位**
        
-   某个价格有**大量卖单堆积**
    
    -   → 潜在**阻力位**
        

在 Web3 中你会常听到：

-   **Buy Wall（买墙）**
    
-   **Sell Wall（卖墙）**
    

## 一、AMM 的核心组成（对应“订单簿的三大组成”）

### 1️⃣ 流动性池（Liquidity Pool）

-   由**两种（或多种）资产**构成，例如 ETH / USDC
    
-   本质不是“买单或卖单”，而是**可随时被交易的资金库存**
    
-   池中资产的数量比例，直接决定当前价格
    

👉 对应订单簿中的：

-   买单 + 卖单的**集合体**
    
-   但不区分“谁在买、谁在卖”
    

* * *

### 2️⃣ 定价函数（Pricing Function）

-   最经典的是恒定函数：`x * y = k`
    
-   价格不是报价，而是：
    
    > **“在当前池子状态下，再多买 1 个，会导致什么变化”**
    
-   每一笔交易都会改变池子状态，也就改变价格
    

👉 对应订单簿中的：

-   不同价格档位的挂单结构
    
-   但在 AMM 中，价格是**连续函数**，而不是离散档位
    

* * *

### 3️⃣ 链上成交记录（On-chain Swap History）

-   每一笔 swap 都是链上交易
    
-   可以看到：
    
    -   成交价格
        
    -   成交数量
        
    -   时间戳
        
    -   交易者地址
        

👉 对应订单簿中的：

-   成交历史（Trade History）
    
-   区别在于：**AMM 的成交历史是完全可验证的**
    

* * *

## 二、AMM 的“核心位置”（对应 Top of the Book）

在 AMM 中，**不存在最高买价 / 最低卖价**，但存在一个等价概念：

### 当前池子隐含价格（Spot Price）

-   由池中两种资产的数量比例决定
    
-   是：
    
    > “在极小交易量下的理论成交价格”
    

📌 含义：

-   池子越大 → 单笔交易对价格影响越小 → 滑点越低
    
-   池子越小 → 价格对交易极其敏感 → 滑点风险高
    

👉 对应订单簿中的：

-   点差（Spread）
    
-   但 AMM 中的“点差”体现为 **滑点曲线的陡峭程度**
    

* * *

## 三、AMM + K 线的配合意义（和订单簿类似，但逻辑不同）

-   **K 线**：反映“已经发生过的交换结果”
    
-   **AMM 池子状态**：反映“当前价格是如何被资金结构支撑的”
    

👉 在 Web3 交易中：

-   K 线 = 历史交换结果
    
-   AMM = 资金结构驱动下的价格函数
    

换句话说：

> 订单簿看的是**人怎么想**  
> AMM 看的是**钱怎么放**

## order book 和 AMM 对比：

| 对比维度 | Order Book（订单簿模式） | AMM（自动做市商模式） |
| --- | --- | --- |
| 主要使用场景 | 中心化交易所（CEX） | 去中心化交易所（DEX） |
| 典型代表 | Binance、OKX、Bybit、Coinbase | Uniswap、Curve、Balancer、PancakeSwap |
| 撮合方式 | 买卖双方挂单撮合 | 通过数学公式自动定价 |
| 是否需要做市商 | 需要（人工或量化做市） | 不需要传统做市商 |
| 流动性来源 | 做市商 + 交易者挂单 | 流动性提供者（LP） |
| 定价机制 | 市场竞价形成价格 | 数学公式（如 x*y=k） |
| 是否有订单簿 | 有 | 没有传统订单簿 |
| 价格发现效率 | 高（深度好时） | 中等（依赖池子规模） |
| 流动性深度 | 与做市规模强相关 | 与资金池规模强相关 |
| 滑点表现 | 深度好时滑点低 | 大额交易滑点明显 |
| 高频 / 量化友好度 | 非常友好 | 不友好 |
| 小币种支持能力 | 上线门槛高 | 上线门槛极低 |
| 抗审查能力 | 弱（中心化） | 强（链上合约） |
| 透明度 | 内部撮合，不完全透明 | 完全链上透明 |
| 资金托管 | 交易所托管 | 用户自托管 |
| 技术复杂度 | 高（撮合系统） | 高（智能合约 + 数学） |
| Web3 原生程度 | 偏 Web2 架构 | Web3 原生 |

## order book总结：

首先是即时供需关系。买单和卖单的数量分布，直接反映了市场中买方与卖方谁更积极。如果某一侧的挂单明显更厚，说明该方向的意愿更强，但这只是“意愿”，不等于一定会成交。

其次是短期价格压力方向。当买单在当前价格下方密集堆积，而卖单相对稀疏时，价格向下的阻力会更大，短期更容易被托住；反之，如果上方卖单密集、买单稀薄，价格向上会面临明显阻力。这更多适用于短周期和高频视角。

第三是流动性与滑点风险。订单簿越深、挂单越连续，说明流动性越好，大额成交对价格的冲击越小；如果订单稀疏、价格档位断层明显，说明流动性差，容易出现较大的滑点。这一点在 Web3 小币种或新上线交易对中尤为重要。

第四是支撑位与阻力位的潜在位置。在某个价格附近，如果出现持续存在的大量买单，往往被视为潜在支撑位；如果某个价格附近长期堆积大量卖单，则可能形成阻力位。但需要注意，这些挂单随时可以撤销，因此只能作为参考，而不是确定性的结论。

第五是市场参与者行为特征。大量、整齐、规律的挂单，往往来自做市商或量化程序；零散、情绪化的挂单，更像散户行为。在加密市场中，订单簿的“形态”有时比数量本身更有信息价值。

第六是短期情绪与博弈意图。买卖单的快速增减、某一价位反复挂单又撤单，可能反映出试探市场、诱导情绪或制造假支撑/阻力的行为。这在 Web3 市场中非常常见，需要结合成交记录一起观察。

第七是成交可能性与执行成本。通过观察当前最优买价和最优卖价，可以判断使用市价单是否会立即成交，以及大致会在什么价格区间内完成成交，这对于策略执行和风控非常关键。

总体而言，订单簿告诉你的不是“未来一定会涨还是跌”，而是：现在这一刻，市场上各方愿意用什么价格参与博弈，以及这种博弈是否有明显失衡。在 Web3 学习和交易中，订单簿更适合用于理解短期结构、流动性和风险，而不是单独用来做中长期判断。

## AMM总结：

AMM 的核心不在“挂单”，而在**资金池 + 定价函数**。市场中不再有买单和卖单，而是由两种资产组成的流动性池，用户直接与池子交易。池中资产的数量比例，决定当前价格；每一笔交易都会改变池子状态，从而推动价格变化。

AMM 的价格不是人为报价，而是由数学函数计算得出，最典型的是 `x * y = k`。这意味着价格是连续变化的，不存在订单簿那样的离散价位结构。交易规模越大、池子越小，对价格的冲击就越明显，这种冲击体现为滑点。

在 AMM 中，没有“最高买价 / 最低卖价”，但存在一个等价概念，即**池子的隐含现价（Spot Price）**。池子越深，价格越稳定，滑点越低；池子越浅，价格对交易越敏感，风险越高。这在效果上，相当于订单簿中的点差和深度。

AMM 和 K 线的关系是：K 线反映已经发生的交换结果，而 AMM 池子状态反映当前价格是如何被资金结构支撑的。订单簿关注的是“人愿意怎么出价”，AMM 关注的是“钱是怎么放的
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


## 今天分享solidity复盘和最新学习的进展(已上传在本人自己的GitHub)和在学习过程中关于区块的一些疑惑(下面有解决）

-   **复习solidity内容(ERC20)**
    

1.  主要检查关于代币铸造和payable进一步的了解[my\_web3\_study/contracts/\_ERC20.sol at master · may-tonk/my\_web3\_study](https://github.com/may-tonk/my_web3_study/blob/master/contracts/_ERC20.sol)
    
2.  学习白名单[my\_web3\_study/contracts/\_DucthAuvtion.sol at master · may-tonk/my\_web3\_study](https://github.com/may-tonk/my_web3_study/blob/master/contracts/_DucthAuvtion.sol)
    

-   **如何识别钓鱼授权**
    

1.  **授权（approve）** = 允许某合约未来可以操作你代币
    
2.  **钓鱼授权** = 授权对象是恶意合约，一旦签署，对方可随时转走资产
    
3.  **防范**
    
4.  **核对合约地址和官方文档**
    
5.  **不要无限授权**，尽量选择“最小额度”
    
6.  **只操作自己主动访问的可信网站**
    
7.  **使用钱包内授权管理检查并撤销可疑授权**
    
8.  **分钱包管理**：主钱包、DApp交互钱包分开
    

-   **内容治理方式**
    

1.  **规则层**：平台政策、奖励/惩罚、透明流程
    
2.  **社区层**：投票、举报、仲裁、DAO 决策
    
3.  **技术层**：智能合约执行规则、链上存证、AI 审核
    

-   **在去中心化协作下怎么实现公平可信分配机制**
    

在去中心化协作中，核心特点是：

1.  **没有中心化管理者**
    
    -   所有参与者平等参与协作和治理。
        
2.  **多方贡献难量化**
    
    -   不同成员贡献类型不同：代码、内容、创意、审核、推广等。
        

### **设计原则**

1.  **透明性（Transparency）**
    
    -   分配规则、计算公式、操作记录全链上公开。
        
2.  **去中心化（Decentralization）**
    
    -   分配权不依赖单一机构，由智能合约或社区投票决定。
        
3.  **贡献导向（Contribution-Oriented）**
    
    -   奖励与实际贡献挂钩，避免“一刀切”。
        
4.  **可验证性（Verifiability）**
    
    -   每个参与者可独立验证分配结果，防止操纵。
        

-   **抗操纵性（Sybil-Resistance）**
    

1.  如何防止作弊、刷量、虚报贡献？
    
2.  **分配需要透明、可验证**
    
3.  每个人都能看到规则和结果，确保公平。
    
4.  **贡献量化**：先确定“谁贡献了什么”，量化成可计算指标
    
5.  **规则透明**：所有分配公式和权重上链
    
6.  **去中心化执行**：智能合约自动分配，无需信任第三方
    
7.  **社区参与**：投票或仲裁处理难以量化或争议内容
    
8.  **防作弊设计**：身份绑定、质押惩罚、多维度验证
    

## **关于Dapp的定义**

![屏幕截图 2026-01-13 213317.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/may-tonk/images/2026-01-13-1768311248576-_____2026-01-13_213317.png)

相关链接[dapps 技术入门 |](https://ethereum.org/developers/docs/dapps/) [ethereum.org](http://ethereum.org) [— Technical introduction to dapps |](https://ethereum.org/developers/docs/dapps/) [ethereum.org](http://ethereum.org)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




# 今日学习内容：

## **_探索web3深度技术学习有关的内容并对相关部分进行总结_**

**一.DApp 如何建立在以太坊之上**？**DApp ≠ 完全去中心化的 App真正去中心化的只有：资产所有权 + 核心业务逻辑**

换句话说：

-   ❌ 前端不一定去中心化
    
-   ❌ 网站不一定去中心化
    
-   ✅ **合约一定要去中心化**
    
-   ✅ **用户私钥一定要在用户手里**
    

* * *

**二.DApp 的标准三层架构**

## 第一层：前端

### 1️⃣ 前端在 DApp 中的角色

前端本质是一个 **“区块链操作界面”**：

-   展示数据
    
-   收集用户输入
    
-   触发链上交易
    
-   调用合约的只读函数
    

📌 **前端本身不可信，也不需要可信**

* * *

### 2️⃣ 技术栈

常见技术：

-   React / Next.js / Vue
    
-   ethers.js / viem / web3.js
    
-   Wagmi + RainbowKit（现代 Web3 常用）
    

* * *

### 3️⃣ 前端能做什么 / 不能做什么

✅ 能做的：

-   调用 `view / pure` 函数（不花 Gas）
    
-   构造交易参数
    
-   请求用户签名
    
-   监听事件（Event）
    

❌ 不能做的：

-   **不能修改链上状态**
    
-   **不能绕过合约规则**
    
-   **不能代替用户签名**
    
-   **前端只是“遥控器”，不是“执行者”。**
    

* * *

### 4️⃣ 一个关键认知

> **任何人都可以写一个新的前端，直接与你的合约交互**

这意味着：

合约才是“真正的产品”前端只是“皮肤”

## **_完成web3实习手册的阅读并且给出相关的总结和思考_**

1.  \*\*_总结：_\*\*以太坊不仅是加密货币（ETH），而是支持智能合约的“全球共享计算机”，实现去中心化应用（如DeFi、NFT、DAO），代码即规则，无需中介。
    
2.  共识机制演进PoW阶段早期依赖矿工算力，TPS低（约30），能耗高。PoS转向：2020年信标链启动，2022年9月The Merge完成，主网从PoW切换PoS。验证者质押32 ETH，区块时间约12秒，能耗降99.95%。未来升级：EIP-4844（2024年上线，降低L2成本70%-90%）；数据分片（2025-2026年启动，提升扩展性）；其他如EIP-1559（费用机制）、Verkle树等
    
3.  EVM 生态系统架构：应用层（DeFi如Uniswap、Aave；NFT如OpenSea；钱包如MetaMask）；协议层（EVM、共识/执行客户端）；扩展层（L2、侧链）。
    

4.**_相关思考_**\_：\_以太坊的战略意义：作为Web3基础，以太坊通过PoS和L2解决了比特币的局限（如扩展性），但仍面临竞争（如Solana的高TPS）。在2026年，随着数据分片落地，以太坊可能进一步巩固“开发者首选”地位，推动更多企业级应用（如供应链、身份验证）。

1.  生态演化：EVM兼容性形成壁垒，许多公链（如BSC、Polygon）借此分流，但也分散流动性。思考：是否会出现“多链并存”还是“以太坊主导”的格局？L2的碎片化可能增加用户学习曲线，但Rollup-as-a-Service（如Optimism的Superchain）正简化这一问题
    

## **_阅读021学习以太坊并且给出相应心得总结_**

1.  以太坊的定义与定位：以太坊是一个全球可编程区块链，不是单纯的虚拟货币系统。它的目标是让任何人可以在链上运行自定义程序（智能合约），实现去中心化应用（DApp）。
    
2.  以太坊采用区块链技术保证不可篡改、公开透明、去中心化的特性。
    
3.  创始背景与发展简史：以太坊由 Vitalik Buterin 发起，目的在突破比特币单一支付系统的限制，引入图灵完备的智能合约执行能力。相比比特币只支持“记账/转账”，以太坊是一个可以运行任意程序的世界计算机。
    
4.  ETH和BTC核心区别：特币的核心区别以太坊的核心不只是价值转移，还包括应用逻辑执行（合约执行）。以太坊使用账户模型，不同于比特币采用的 UTXO 模型。以太坊有内置编程功能，而比特币脚本较为受限。
    
5.  以太币 (ETH) 的角色：ETH 是以太坊网络中的原生单位，用于支付交易费（Gas）、担保执行资源。ETH 在网络中扮演：用户和合约执行 Gas 支付单位系统内最基本价值载体未来 PoS 网络中质押与奖励的基础资产当你调用合约或发起交易时，你实际上是在用 ETH 购买网络的计算和存储资源
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
