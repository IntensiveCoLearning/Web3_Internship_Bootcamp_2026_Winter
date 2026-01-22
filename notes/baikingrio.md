---
timezone: UTC+8
---

# Quinn

**GitHub ID:** baikingrio

**Telegram:** 

## Self-introduction

巩固加强Web3技术开发能力和拓展知识面。

## Notes

<!-- Content_START -->
# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->
## Uniswap V2 Flash Swap（闪电交换）

Flash Swap（也叫 Flash Loan in Uniswap V2 语境）是 Uniswap V2 中最强大且最具创新性的功能之一。它允许用户（通常是智能合约）先借出池子中的任意数量的代币，然后在同一笔交易内执行任意逻辑，最后归还借出的代币（或等值的另一种代币）+ 0.3% 费用。

-   本质：Uniswap V2 的所有 swap 实际上都是 Flash Swap！
    
-   普通 swap：用户先转入输入代币 → Pair 转出输出代币。
    
-   Flash Swap：Pair 先乐观地转出输出代币 → 调用回调 → 用户在回调中归还输入代币（或另一种代币）。
    
-   因为 Ethereum 交易是原子性的，如果回调中没有正确归还，整个交易会回滚（revert）。
    

这让用户可以实现零资本套利、清算、抵押品交换等复杂操作，而无需提前持有资金。

### 为什么叫 Flash Swap 而不是 Flash Loan？

-   Uniswap V2 官方文档称之为 Flash Swaps。
    
-   它本质上是“借一种代币 + 还另一种代币”的 swap 过程，而不是传统意义上的无抵押贷款。
    
-   但功能上等价于 flash loan，许多人直接叫它 Uniswap V2 的 flash loan。
    

### 核心源码分析：swap 函数中的 Flash Swap 实现

在 UniswapV2Pair.sol 的 swap 函数中：

```
function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external lock {
    // ... 检查 amountOut > 0 且不超过储备 ...

    // 乐观转出（先把要借的代币发给 to）
    if (amount0Out > 0) _safeTransfer(token0, to, amount0Out);
    if (amount1Out > 0) _safeTransfer(token1, to, amount1Out);

    // 如果传入了 data（非空），触发回调
    if (data.length > 0) {
        // 调用 to 地址的 uniswapV2Call 函数
        IUniswapV2Callee(to).uniswapV2Call(msg.sender, amount0Out, amount1Out, data);
    }

    // 计算实际收到的输入量（balance - (reserve - amountOut)）
    uint amount0In = balance0 > _reserve0 - amount0Out ? balance0 - (_reserve0 - amount0Out) : 0;
    uint amount1In = balance1 > _reserve1 - amount1Out ? balance1 - (_reserve1 - amount1Out) : 0;

    require(amount0In > 0 || amount1In > 0, 'INSUFFICIENT_INPUT_AMOUNT');

    // 验证 K 值（扣除 0.3% 费用后仍满足常量乘积）
    uint balance0Adjusted = balance0.mul(1000).sub(amount0In.mul(3));
    uint balance1Adjusted = balance1.mul(1000).sub(amount1In.mul(3));
    require(balance0Adjusted.mul(balance1Adjusted) >= uint(_reserve0).mul(_reserve1).mul(1000**2), 'K');

    _update(balance0, balance1, _reserve0, _reserve1);
    emit Swap(msg.sender, amount0In, amount1In, amount0Out, amount1Out, to);
}
```

关键流程：

1.  Pair 先把 amount0Out 或 amount1Out 转给 to（乐观转账）。
    
2.  如果 data 非空，调用 to 的 uniswapV2Call 回调函数。
    
3.  在回调中，用户可以：
    
    -   执行任意逻辑（套利、调用其他合约、清算等）。
        
    -   必须归还足够的代币（输入量 + 0.3% 费用）。
        
4.  回调结束后，Pair 检查实际收到的输入量是否足够（K 值验证）。
    
5.  如果不够 → 交易 revert，一切回滚。
    

### 回调接口：IUniswapV2Callee

用户合约需要实现这个接口：

```
interface IUniswapV2Callee {
    function uniswapV2Call(
        address sender,      // 调用 swap 的人（通常是路由或用户合约）
        uint amount0,        // 借出的 token0 数量
        uint amount1,        // 借出的 token1 数量
        bytes calldata data  // 自定义数据，可用于传递参数
    ) external;
}
```

在 uniswapV2Call 中必须：

-   计算需要归还的金额：amountBorrowed \* 1000 / 997（因为 0.3% 费用，实际要多还一点）。
    
-   转回足够的 token0 或 token1 给 Pair。
    
-   或者转回另一种代币（相当于 swap 方向相反）。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->

## UniswapV2的协议费用

V2 的协议费用（Protocol Fee）是一种可选机制，设计目标是从每笔交易的 0.3% 交易费中抽取 1/6（约 16.67%），即 0.05% 归协议所有（剩余 0.25% 全部给流动性提供者 LP）。

### 1、协议费用的开关与接收地址

-   由 **UniswapV2Factory** 合约控制。
    
-   关键变量：
    
    -   feeTo：协议费用接收地址（默认是 address(0)，即关闭状态）。
        
    -   feeToSetter：只有这个地址可以调用 setFeeTo(address) 来开启/修改接收地址。
        
-   当 feeTo != address(0) 时，协议费用机制开启（feeOn = true）。
    

### 2、为什么不直接在 swap 时收取？

-   直接在每笔 swap 中扣除 0.05% 会显著增加 gas 成本（因为swap 是最频繁的操作）。
    
-   Uniswap V2 的巧妙设计：**把协议费用“延迟”到 mint / burn 时收取**，通过铸造额外的 LP Token 给 feeTo 来实现。
    
-   这样 swap 操作保持低 gas，而费用在流动性添加/移除（相对不频繁）时一次性结算。
    

### 3、核心计算逻辑：\_mintFee 函数

协议费用在 Pair 合约的 mint() 和 burn() 函数中调用 \_mintFee() 来处理。

核心代码（简化版）：

```
function _mintFee(uint112 _reserve0, uint112 _reserve1) private returns (bool feeOn) {
    address feeTo = IUniswapV2Factory(factory).feeTo();
    feeOn = feeTo != address(0);
    uint _kLast = kLast;  // 上次更新时的 k 值（reserve0 * reserve1）

    if (feeOn) {
        if (_kLast != 0) {
            uint rootK = Math.sqrt(uint(_reserve0).mul(_reserve1));          // 当前 √k
            uint rootKLast = Math.sqrt(_kLast);                               // 上次 √k
            if (rootK > rootKLast) {                                          // 有费用累积（k 增长了）
                uint numerator = totalSupply.mul(rootK.sub(rootKLast));
                uint denominator = rootK.mul(5).add(rootKLast);               // 关键：×5 + 上次
                uint liquidity = numerator / denominator;                     // 要额外铸造的 LP 数量
                if (liquidity > 0) _mint(feeTo, liquidity);                   // 铸造给 feeTo
            }
        }
    } else if (_kLast != 0) {
        kLast = 0;  // 关闭时清零
    }
}
```

-   **kLast**：记录上一次 mint/burn/sync 时（即流动性事件）的 k 值（reserve0 × reserve1）。
    
-   swap 发生时 k 会增长（因为 0.3% 费用被加到储备中），但 kLast 不变。
    
-   当有人再次 mint 或 burn 时，比较当前 √k 和 √kLast 的增长。
    
-   额外铸造的 LP 数量公式：
    

```
liquidity = totalSupply × (√k - √kLast) / (5 × √k + √kLast)
```

这个公式确保铸造的 LP 份额正好对应于累积费用的 1/6（即协议拿走累积费用的 1/6，LP 们分 5/6）。

### 4、数学原理（为什么 ×5 + √kLast）

-   假设费用累积导致 k 增长，增长部分全部来自 0.3% 交易费。
    
-   协议想拿走 1/6 的费用增长 → 相当于让协议拥有增长部分流动性的 1/6。
    
-   通过铸造新 LP Token 给 feeTo，稀释现有 LP，但精确控制稀释比例为 1/6。
    
-   推导后正好得到上述公式：分母是 5×当前√k + 上次√k，相当于把增长的 1/6 分配给协议，5/6 留给 LP。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->


## Swap 与价格预言机

Uniswap V2 的核心功能之一是 Swap（代币交换），允许用户在流动性池中交换两种 ERC-20 代币，同时维护常量乘积公式（x \* y = k）。Swap 通过 Pair 合约实现，涉及费用计算、储备更新和事件触发。

另一个重要特性是 价格预言机（Oracle），Uniswap V2 内置 TWAP（时间加权平均价格）机制，用于提供可靠的价格数据，防止短期操纵。预言机基于累积价格计算平均价格，常用于 DeFi 协议（如借贷平台）获取喂价，这些机制在 UniswapV2Pair.sol 合约中实现。

### Swap

-   Swap 不是直接匹配买卖订单，而是从池中取出一种代币，存入另一种。
    
-   使用常量乘积公式确保价格自动调整：交换后，储备量乘积保持不变（忽略费用）。
    
-   费用：0.3%（997/1000），其中 0.25% 奖励 LP，0.05% 可选给协议（通过 Factory 的 feeTo）。
    
-   滑点：大额交易会导致价格偏差，用户需设置容忍度。
    
-   支持闪电交换（Flash Swap）：先取出代币，后支付（通过回调）。
    

在 UniswapV2Pair.sol 中，核心函数是：

```
function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external lock {
    require(amount0Out > 0 || amount1Out > 0, 'UniswapV2: INSUFFICIENT_OUTPUT_AMOUNT');
    (uint112 _reserve0, uint112 _reserve1,) = getReserves(); // 获取当前储备
    require(amount0Out < _reserve0 && amount1Out < _reserve1, 'UniswapV2: INSUFFICIENT_LIQUIDITY');

    uint balance0;
    uint balance1;
    { // 作用域避免栈溢出
        address _token0 = token0;
        address _token1 = token1;
        require(to != _token0 && to != _token1, 'UniswapV2: INVALID_TO');
        if (amount0Out > 0) _safeTransfer(_token0, to, amount0Out); // 先转出输出代币（乐观转账）
        if (amount1Out > 0) _safeTransfer(_token1, to, amount1Out);
        if (data.length > 0) IUniswapV2Callee(to).uniswapV2Call(msg.sender, amount0Out, amount1Out, data); // 回调，支持闪电贷
        balance0 = IERC20(_token0).balanceOf(address(this));
        balance1 = IERC20(_token1).balanceOf(address(this));
    }
    uint amount0In = balance0 > _reserve0 - amount0Out ? balance0 - (_reserve0 - amount0Out) : 0;
    uint amount1In = balance1 > _reserve1 - amount1Out ? balance1 - (_reserve1 - amount1Out) : 0;
    require(amount0In > 0 || amount1In > 0, 'UniswapV2: INSUFFICIENT_INPUT_AMOUNT');
    
    { // 验证费用后常量乘积
        uint balance0Adjusted = balance0.mul(1000).sub(amount0In.mul(3)); // 0.3% 费 (3/1000)
        uint balance1Adjusted = balance1.mul(1000).sub(amount1In.mul(3));
        require(balance0Adjusted.mul(balance1Adjusted) >= uint(_reserve0).mul(_reserve1).mul(1000**2), 'UniswapV2: K');
    }

    _update(balance0, balance1, _reserve0, _reserve1); // 更新储备和累积价格
    emit Swap(msg.sender, amount0In, amount1In, amount0Out, amount1Out, to);
}
```

-   **关键步骤**：
    
    1.  检查输出量 > 0 且不超过储备。
        
    2.  乐观转出输出代币（支持闪电交换）。
        
    3.  如果有 data，调用回调（uniswapV2Call），允许用户在同一交易中还款。
        
    4.  计算输入量（balance - reserve + out）。
        
    5.  验证调整后余额满足 k 值（扣除 0.3% 费）。
        
    6.  更新储备（\_update 函数，同时更新预言机）。
        
    7.  触发 Swap 事件。
        
-   **锁修饰符**：使用 ReentrancyGuard 防止重入攻击。
    
-   **闪电交换示例**：用户可先借出代币，执行套利，然后在回调中归还 + 费用。
    

### 价格预言机机制

-   Uniswap V2 提供 TWAP 预言机，抵抗短期价格操纵（因为累积价格基于时间加权）。
    
-   每当储备更新（mint/burn/swap/sync）时，计算累积价格。
    
-   外部合约可查询平均价格：(priceCumulativeCurrent - priceCumulativeLast) / (timeElapsed)。
    
-   价格单位：token1/token0 或 token0/token1，使用固定点数（UQ112x112）表示。
    
-   变量
    

```
uint public price0CumulativeLast;
uint public price1CumulativeLast;
uint32 public blockTimestampLast;
```

-   \_update 函数（在 mint/burn/swap/sync 中调用）
    

```
function _update(uint balance0, uint balance1, uint112 _reserve0, uint112 _reserve1) private {
    require(balance0 <= uint112(-1) && balance1 <= uint112(-1), 'UniswapV2: OVERFLOW');
    uint32 blockTimestamp = uint32(block.timestamp % 2**32); // 防溢出
    uint32 timeElapsed = blockTimestamp - blockTimestampLast;
    
    if (timeElapsed > 0 && _reserve0 != 0 && _reserve1 != 0) {
        // 累积价格：price0 = reserve1 / reserve0 (UQ112x112 编码)
        price0CumulativeLast += uint(UQ112x112.encode(_reserve1).uqdiv(_reserve0)) * timeElapsed;
        price1CumulativeLast += uint(UQ112x112.encode(_reserve0).uqdiv(_reserve1)) * timeElapsed;
    }
    reserve0 = uint112(balance0);
    reserve1 = uint112(balance1);
    blockTimestampLast = blockTimestamp;
    emit Sync(reserve0, reserve1);
}
```

-   getReserves：返回储备和时间戳，用于外部计算 TWAP。
    
-   UQ112x112 库：用于精确价格计算（112 位整数 + 112 位小数）。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->



## UniswapV2Pair.sol - 交易对合约

### 主要作用

每个交易对就是一个独立的 ERC-20 LP 代币合约 + AMM 池，同时实现：

-   流动性提供/移除（mint/burn）
    
-   代币交换（swap）
    
-   价格预言机（price oracle）
    
-   手续费累积
    

### 继承与接口

继承：

-   UniswapV2ERC20（基本的 ERC-20 实现，代表 LP 份额） 实现接口：IUniswapV2Pair
    

### 核心状态变量

```
uint public constant MINIMUM_LIQUIDITY = 10**3;  // 永久锁定的最小流动性（防止精度丢失）

address public factory;          // 所属工厂地址
address public token0;           // 排序后的较小地址代币
address public token1;           // 排序后的较大地址代币

uint112 private reserve0;        // token0 储备量（uint112 节省 gas）
uint112 private reserve1;        // token1 储备量
uint32  private blockTimestampLast; // 上次更新时间戳

uint public price0CumulativeLast;     // token0 累计价格（用于 TWAP）
uint public price1CumulativeLast;     // token1 累计价格

uint public kLast;                    // 上次同步时的 k = reserve0 * reserve1
```

### 关键事件

```
event Mint(address indexed sender, uint amount0, uint amount1);
event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
event Swap(...);
event Sync(uint112 reserve0, uint112 reserve1);
```

### 核心函数

1.  **initialize(address _token0, address_ token1)**
    
    -   只能由工厂调用一次
        
    -   设置 token0/token1
        
2.  **getReserves()** → (uint112 _reserve0, uint112_ reserve1, uint32 \_blockTimestampLast)
    
    -   返回当前储备量（视图函数）
        
3.  **mint(address to) → uint liquidity**
    
    -   提供流动性，铸造 LP 代币
        
    -   计算新增流动性比例（基于当前储备）
        
    -   第一次添加流动性时，会永久锁定 MINIMUM\_LIQUIDITY（0.0001%）给 address(0)
        
    -   更新 kLast（用于判断是否需要手续费）
        
4.  **burn(address to) → (uint amount0, uint amount1)**
    
    -   移除流动性，销毁 LP 代币，返还两种代币
        
5.  **swap(uint amount0Out, uint amount1Out, address to, bytes calldata data)**
    
    -   交换核心函数！
        
    -   要求：至少有一个 amountOut > 0
        
    -   计算输入量（amountIn = balance - reserve + fee）
        
    -   验证 k 增加（考虑 0.3% 手续费）
        
    -   转移输出代币给 to
        
    -   如果 data 不为空，调用 to 的 uniswapV2Call（闪电贷回调）
        
    -   最后更新储备 + 累计价格
        
6.  **skim(address to)**
    
    -   把多余的代币（因直接转入而非通过 mint）转给 to（通常是攻击者或清理用）
        
7.  **sync()**
    
    -   强制同步储备量 = 当前余额（用于修复储备不一致的情况）
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->




## 了解UniswapV2合约的代币交换机制

在 Uniswap V2 中，交换是通过Pair合约执行的。每次交换都会改变Pair中两个代币的储备余额，同时保持恒定乘积公式x\*y=k。

当用户想要用代币 A 交换代币 B 时：

1.  用户将代币 A 发送到Pair合约
    
2.  Pair合约计算可以接收的代币 B 的数量（基于当前的储备比率和 0.30% 的费用）
    
3.  Pair合约将代币 B 发送给用户
    
4.  储备余额更新，但 `k` 值增加（由于费用）
    

交换价格由以下因素决定：

1.  当前储备比率 - 配对中两种代币的当前余额
    
2.  交换规模 - 相对于储备量的交换大小
    
3.  费用 - 每笔交易收取 0.30% 的费用
    

较大的交换相对于储备量会产生更多的滑点，因为价格沿着恒定乘积曲线移动。

如果两个代币之间没有直接配对，可以通过中间代币进行多路径交换。Router 合约自动处理这些路径，找到最佳路由。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->





## 阅读Uniswap V2工厂合约代码

Uniswap V2 的工厂合约（UniswapV2Factory.sol）是 Uniswap 协议的核心组件之一，用于创建和管理流动性池对（Pair）。它本质上是一个“工厂”，负责标准化地部署交易对合约，确保每个 token 对只有一个唯一的流动性池，从而避免流动性碎片化。代码很简洁高效，只有不到 50 行，但缺体现了 Uniswap 的创新设计。

首先，合约实现了 IUniswapV2Factory 接口，定义了几个关键状态变量：feeTo（手续费接收地址，默认没有设置）和 feeToSetter（设置 feeTo 的权限持有者，由构造函数初始化）。有一个 mapping getPair，用于存储 token0 和 token1 之间的 pair 地址，支持双向查询；还有 allPairs 数组，记录所有创建的 pair，便于外部查询总数（通过 allPairsLength 函数）。

核心函数是 createPair，它允许任何人创建新的 token 对。函数先检查输入：tokenA 和 tokenB 不能相同，不能为零地址，且 pair 不存在。然后，通过比较地址大小排序 token0 和 token1，确保唯一性。部署使用 create2 操作码，这是一个关键创新点：利用 bytecode（UniswapV2Pair 的创建代码）和 salt（keccak256 编码的 token0+token1）来确定性地生成 pair 地址。这种方式让 pair 地址可预测，避免了传统 create 的随机性，有助于链上交互和安全性。部署后，调用 pair 的 initialize 初始化 token0 和 token1，更新 mapping 和 allPairs，并发出 PairCreated 事件，记录 pair 的创建顺序。

权限控制方面，setFeeTo 和 setFeeToSetter 函数确保只有 feeToSetter 能修改 feeTo（用于协议手续费）和转移 setter 权限。这体现了去中心化治理的初步形式：初始 setter 可以是部署者，后来可转移给 DAO 等。feeTo 如果设置，手续费的 1/6 会流向它，否则默认关闭协议费。

总结一下，这个合约做到了最小化信任假设，任何人可创建 pair，无需许可，促进开放 DeFi 生态。create2 的使用提升了效率和可组合性，例如允许 flash loan 等高级应用。相比 V1，V2 Factory 更灵活，支持任意 ERC20 对，而非只 ETH。整体上，它是 AMM 模型的基石，推动了流动性提供和价格发现的自动化。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->






Uniswap V2 的核心由两个存储库组成：core 和 periphery。核心合约为所有与 Uniswap 交互的参与方提供基本的安全保障。外围合约与一个或多个核心合约交互，但其本身并非核心合约的一部分。

## 核心合约

核心合约由一个单例工厂和许多交易对组成，工厂负责创建和索引这些交易对。这些交易对非常简洁，这样做的理由很简单：一是更容易理解，二是不容易出错，功能也更优雅。这种设计最大的优点或许在于，系统的许多所需属性可以直接在代码中声明，从而最大限度地减少了出错的可能性。然而，缺点在于核心合约和交易对合约对用户不太友好。事实上，在大多数情况下，不建议直接与这些交易对进行交互。相反，更推荐使用外围合约调用交易对。

## 工厂合约

工厂合约负责创建交易对合约。它的主要任务是为每个唯一的代币对创建一个唯一交易对合约。它还包含启用协议收费的逻辑。

## 交易对合约

交易对有两个主要用途：一是作为自动做市商，二是跟踪资金池代币余额。它们还会公开可用于构建去中心化价格预言机的报价数据。

## 外围合约

外围系统是由一系列智能合约组成的集合，它负责安全高效地与 Uniswap V2 核心、工厂合约交互。

## 库合约

库合约提供了各种便捷的函数，用于获取数据和定价。

## 路由合约

路由合约充分使用库合约，支持前端调用进行交易、流动性管理等功能。它支持多交易对交易，比如从x代币到y代币再到z代币，还提供一处流动性的功能。通常Dapp前端中会经常调用路由合约的功能。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->







Uniswap 是一个基于恒定乘积公式的自动化流动性协议，它通过以太坊区块链上不可升级的智能合约系统实现。Uniswap 无需可信中介机构，优先考虑去中心化、抗审查性和安全性。Uniswap 是开源软件，采用 GPL 许可协议。  
每个 Uniswap 智能合约（称为 pair 交易对）管理一个流动性池，它包含两种 ERC-20 代币的储备。  
  
任何人都可以通过向池中存入等值的底层代币成为资金池的流动性提供者（LP），并收到资金池代币作为回报。这些池代币代表 LP 在总储备中的份额，并且可以随时赎回相应比例的底层资产。

配对作为自动化做市商（AMM）运行，只要维持"恒定乘积"公式，就可以随时用一种代币交换另一种代币。该公式表示为 \`x \* y = k\`，确保配对储备余额（\`x\` 和 \`y\`）的乘积（\`k\`）在交易期间保持不变。因此，相对于储备量较大的交易，其执行价格会按指数级恶化。  
Uniswap 对每笔交易收取 0.30% 的手续费，该费用会添加到储备中。这意味着每次交易都会使\`k\`值增大，这相当于给流动性提供者（LP）带来收益。LP 在销毁自己的池代币以提取其应得的储备金份额时，即可实现收益。未来，该费用可能会降至 0.25%，剩余的 0.05% 作为协议的固定费用。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->








![WeChat509bc988ca05c9df2227c761fde707a1.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/baikingrio/images/2026-01-12-1768183404462-WeChat509bc988ca05c9df2227c761fde707a1.jpg)

今天的计划是详细阅读UniswapV2文档，在V1版本上进行了很大的变化。

| 核心特性 | 技术要点说明 | 主要意义/改进 |
| --- | --- | --- |
| 任意 ERC-20 ↔ ERC-20 | 不再强制通过 ETH 中转，可直接创建任意两种 ERC-20 的交易对 | 极大提升灵活性，减少滑点和 gas 消耗 |
| 定价公式 | x × y = k（恒定乘积做市商） | 简单、数学上永不破产（但存在无常损失） |
| 交易手续费 | 0.3%（全部给 LP），可选开启 0.05% 协议费（LP 得 0.25%） | 协议费开关写死在合约里，基本不可改 |
| 闪电兑换（Flash Swap） | 先拿走想要的 token，同一交易内必须归还（可还另一种 token 等值） | 开启套利、清算、抵押品拍卖等高级用法 |
| TWAP 价格预言机 | 每个区块开始时记录当时池子价格，累加成 cumulative price（uint112） | 抗短期操纵，为链上其他协议提供相对安全价格 |
| 合约结构 | Factory + Pair 双层结构，核心合约不可升级，极简设计 | 经典且安全，被最多项目直接复制 |
| 重要安全设计 | - 强制检查 balance 防 donation attack - 烧毁 1000 份最小流动性 - sync()/skim() 逃生通道 | 修复了 V1 很多工程上的危险边界情况 |
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
