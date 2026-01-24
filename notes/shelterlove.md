---
timezone: UTC+8
---

# shelterlove

**GitHub ID:** shelterlove

**Telegram:** @shelterlove6

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->
今天的主要任务还是继续深度任务的学习和实践

1.  听了几个分享会，同学们都好有热情，能力和学习热情都狠棒，向大家学习
    
2.  听了LXDAO周会，感受下去中心治理的魅力，对LXDAO的好感又加深了
    
3.  终于完成了Reactive的任务，花了好久，明天整理一个学习流程发到X上
    
4.  任务中对Uniswap V2中函数的直接调用让我的理解更深刻了
    
5.  和yuxuan老哥线下面基了，离得这么近实在是太巧了，还有base厦门的同学欢迎找我玩
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

今天完成任务+听分享会，感觉没有太多时间做详细笔记了，跟huahua老师学习，简单记录一下，周六日复盘再整理下

1.  上午Austin的分享相当友好了，带大家过了一下SpeedRun，他真的很有激情，想他学习，比较关键的点是随机数的问题
    
2.  下午的colearning，印象最深的是在web3的世界中真的要提高防范意识，保护好自己
    
3.  晚上的分享会真是误闯天家了，印象最深的是，建立个人的知识库，前端开发Rainbow什么工具，还有hardhat框架，之前只了解了Foundry，回头还要看下hardhat，总之要学习的东西真的很多
    
4.  本来还想再研究下Reactive合约的，只能交给明天了，要学的真的太多了了了了了了了了
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


## 完成任务

1.  完成了几个技术向的任务
    
2.  colearning wachi老师的分享太赞了
    
3.  xiaohai老师的分享会真的让人收获满满，最后的鸡汤环节也很动人
    

## 笔记

### Dapp开发

1.  一个Dapp，核心是和链上的交互，但同样有链下的支撑，主要包括三个部分，智能合约本身，前端（负责网页，和钱包交互，发起交易等），后端（本地的数据库，快速的读取，提高用户体验）
    
2.  智能合约方面，solidity和Foundry和Hardhat，Foundry和Hardhat各有侧重，尽可能地都要了解
    
3.  前端最常用的框架事React/Next.js wagmi viem
    
4.  后端可能会涉及到SQL等数据库
    
5.  访问链上需要RPC，链上存储需要IPFS
    
6.  部署靠Vercel或者VPS
    
7.  感觉要学的有好多
    

### Reactive Contract

-   因为任务中有这个项目，就花了点时间了解了一下
    
-   简单来说，reactive contract提供了一种监控链上状态并自动触发合约的功能
    
-   比较核心的内容是双状态的设计，同一份RC代码，会在两个运行环境里运行，存储状态彼此独立，写的时候必须明确在哪里执行，读写哪一份状态
    
-   简单来说，Reactive Network接入外部世界，ReactVM处理事件决定是否要发生Callback
    
-   两个状态的区分通过判断特定地址上是否有代码， 有代码是RN，无代码是RV
    
-   assembly { size := extcodesize(0x0000000000000000000000000000000000fffFfF) } vm = size == 0
    
-   这种架构分层，一方面加速了并行处理，另一方面也更安全，让受限的环境做判断，边界更清晰
    
-   流程大概如下
    

1.  事件产生 emit event
    
2.  RN会看到匹配订阅的事件，subscribe() 设置过滤条件：链 ID、合约地址、event topics 等
    
3.  把事件穿给react，在RV上进行判断
    
4.  判断完后，发出一个callback请求给RN
    
5.  RN到链上发起交易，同时还有安全检查
    

-   第一步检查调用者地址符合回调代理地址
    
-   第二步RC部署者的地址是否等于合约里记录的授权身份
    

## 明日计划

1.  继续学习reactive的内容
    
2.  看看深度向任务哪些能做
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



## 笔记

Uniswap

V2 恒积做市商

liquidity 流动性；iso-price line 等价格线；price impact 价格影响；slippage 滑点；impermanent loss 无常损失；flash loan 闪电借贷；TWAP oracle 时间加权平均价格预言机

x\*y=k p=y/x 保持两个代币数量乘积恒定，比值就是兑换价格

k越大流动性越大，兑换时候带来的移动也会变少

因为交易带来的价格移动，会导致成交价格因为AMM曲线产生偏离被称为Price Impact，可根据流动性预测

Slippage滑点，取决于交易顺序或MEV，不可预测

流动性的添加减少要严格按照比例

无常损失，对于LP（流动性提供者），在价格变动时，本质上是相当于在用一种币沿曲线定投另一种币，价格变动结束时，如果在此时梭哈，“受益”会更高，损失的部分就被成为无常损失

当价格 3,000时，LP价值

6,000 + 2 \* 3,000 = 12,000

当价格 3,200时，LP价值

6,196.77 + 1.9365 \* 3,200

\= 12,393.5467 Nice!

但是，如果不做 LP

6,000 + 2 \* 3,200 = 12,400

TWAP时间加权平均价格预言机，定义了累积价格方便计算

price0CumulativeLast += reserve0 / reserve1 \* timeElapsed

TWAP = (v1 - v0) / (t1 - t0)

本质上是计算面积等价的矩形

V3 集中流动性做市商

LP提供的流动性贡献在一个特定的区间，可以让流动性更大

因为L可能在变化，所及swap中需要轮询

1）记录每个流动性变化的价格点位和变化值

2）计算推到下一个点位消耗的 x或 y

3）根据变化值修改流动性，重复 2）

点位是离散的tick，避免gasFee过高

计算LP的Fee，取决于该区间的Fee在LP加入后增加的单位量乘上LP提供的流动性

## 明日任务

1.  继续Uniswap的学习
    
2.  完成深度技术中能看懂的任务
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




## 笔记

**remix实操**

运行了Transient Storage代码，该part旨在介绍一种介于memory 和 storage间的存储方式，在交易过程中类似storage，交易结束就没有了，成本更低，代码内包含两个案例，共计6个合约

案例1 展示tstore的存储能力

通过callback函数分别调用TestStorage和TestTransientStorage ，

在调用val()两个合约都传回了数值，分别为123和321，但合约内部的val，TestTransientStorage仍然为默认值，没有被存下来

案例2 展示tstore用来防止重入攻击，MaliciousCallback中先直接调用test，再通过回调触发fallback()再调用test，重入攻击

常规的方法是通过一个storage变量实现modifier

bool private locked;

require(!locked); locked = true; \_; locked = false;

使用tstore成本更低

bytes32 constant SLOT = 0; assembly { if tload(SLOT) { revert(0, 0) } tstore(SLOT, 1) } \_; assembly { tstore(SLOT, 0) }

在不加locked情况下，增加一个计数器统计重访了多少次，可以看到虽然只运行了一次，但实际上访问了30次

加上lock后，防止了重访

对比两种方式的gas消耗，大幅减少

tstore：27240

storage：4906

## 计划

1.  最近还要写论文，准备年终组会，明天上午写论文
    
2.  下午把深度技术的几个任务尝试一下
    
3.  继续看solidity by example
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->





## bytes 格式介绍

### **1) 基本概念**

-   `bytes`：动态字节数组。
    
-   `bytesN`：定长字节类型。
    

常见用法示例：

-   `bytes32`：哈希（`keccak256`）、Merkle Root、ID、固定长度 key。
    
-   `bytes4`：函数选择器（function selector）。
    
-   `bytes1`：单字节标记、位运算场景。
    

补充：

-   `bytes` 常用作 `abi.encode(...)` 的结果，也常见于 `calldata` 输入参数。
    
-   `string` 也是可变长度，约定为 UTF-8 编码的字节序列。
    

### **2) 转换规则**

-   `bytesN` 之间可转换；常用 `bytes4`（从 hash（一般是 `bytes32`）截取）作为 selector。
    
-   大转小：删除右侧字节。
    
-   小转大：在右侧补零。
    
-   `bytes` 是数组，可单独访问某些字节。
    

### **3) 从** `bytes` **解释为具体类型（**`address` **/** `uint256` **/** `bytes32` **等）**

常见有三种做法：

1.  **标准做法：**`abi.decode`
    
    -   不能解码 `abi.encodePacked(...)` 的结果（packed 编码没有边界信息，很多情况下不可逆）。
        
2.  `assembly`**（内联汇编）**
    

`mload` 是 EVM 的一条**内存读取指令**（memory load）。在 Solidity 的 `assembly` 里，用它从 **memory** 的某个地址一次性读出 **32 字节（256 bit）**的数据。

**基本定义**

-   语法（Yul/assembly）：
    

```
assembly {
    let x := mload(ptr)
}
```

-   含义：从内存地址 `ptr` 开始，读取 **32 字节**，返回一个 256 位整数（`uint256` 语义的 word）。
    

EVM 的内存按字节寻址，但 `mload` 总是“按 word（32 字节）”读。

原因：EVM 的基本计算单位是 256-bit word：栈操作、算术、哈希、ABI 编码等几乎都围绕 32 字节对齐。

`mload` **读到的是什么：与 Solidity memory 布局的关系**

Solidity 中很多动态类型在 memory 的布局是：

-   `bytes memory b` / `string memory s`
    
    -   `b` 指向一段内存
        
    -   **前 32 字节**是长度 `len`
        
    -   接下来才是数据内容（从 `b + 32` 开始）
        

因此常见写法：

```
bytes32 out;
assembly {
    out := mload(add(b, 32)) // 读 b 的数据区前 32 字节
}
```

这里：

-   `b` 指向“长度字段”的指针
    
-   `add(b, 32)` 跳过 length，指向数据起始处
    

**读不足 32 字节会怎样？**

`mload` 永远读 32 字节；如果你实际上只关心其中一部分（比如 `address` 20 字节），就需要位移截取：

```
address a;
assembly {
    let word := mload(add(b, 32)) // 读 32 字节
    a := shr(96, word)            // 右移 12 字节，取高 20 字节作为 address
}
```

说明：

-   `address` 是 20 字节（160 bit）
    
-   256 - 160 = 96，所以 `shr(96, word)` 常用来把 32 字节里对应部分对齐到低位
    

> 注意：截取“高 20 字节”还是“低 20 字节”取决于你数据在内存里的对齐方式；在解析 bytes/string 的场景中，上述写法是最常见且通常正确的。

3.  **手工拼装（较复杂）**
    

**从** `bytes` **的第 0~19 字节拼出** `address`**（20 字节）**

```
function readAddressManual(bytes memory b, uint256 offset)
    internal
    pure
    returns (address out)
{
    require(b.length >= offset + 20, "too short");

    uint256 x;
    for (uint256 i = 0; i < 20; i++) {
        x = (x << 8) | uint8(b[offset + i]); // 逐字节左移拼装（大端）
    }

    out = address(uint160(x));
}
```

* * *

## 几个语法小 tips

1.  `view` 代表只读；`pure` 不读也不改，但不代表不能运算（可以用临时变量、入参、常量做计算）。
    

```
function doSomething() public view {
    // Local variables are not saved to the blockchain.
    uint256 i = 456;

    // pure 函数做不了下面的
    uint256 timestamp = block.timestamp; // Current block timestamp
    address sender = msg.sender;         // address of the caller
}
```

2.  `immutable` 可以在构造函数中赋值，不同部署的合约可以不同；`constant` 是常量，一直一样。
    
3.  状态变量的读取可以不经过发送交易：读取一些变量时不需要和钱包交互。
    
4.  单位：`1 ether = 10e18 wei = 10e9 gwei`。Solidity 中的 `1` 代表 `1 wei`，可以使用单位：`1 wei`、`1 gwei`、`1 ether`。
    
5.  Gas 的两个上限：`gas limit`（交易级）和 `block gas limit`（区块级）。为了防止无限消耗 gas，前者自己设置，后者是区块网络定义。
    
6.  逻辑结构示例：
    

```
if (condition) {
    // ...
} else if (otherCondition) {
    // ...
} else {
    // ...
}

for (uint256 i = 0; i < 10; i++) {
    // ...
}

while (i < 10) {
    i++;
}
```

7.  `mapping` 可以嵌套：
    

```
mapping(address => mapping(uint256 => bool)) public nested;

// 访问
nested[_addr][_i];

// 删除
delete nested[_addr1][_i];
```

8.  `array` 可以在新建的时候固定长度，也可以是动态长度：
    

```
uint256[] public arr;
uint256[] public arr2 = [1, 2, 3];
uint256[10] public myFixedSizeArr;

// 创建 memory array 只能固定长度
uint256[] memory a = new uint256[](5);

// push / pop / delete
// arr.push(i) 新增元素 i
// arr.pop()  删除最后一个元素
// delete arr[index] 删除指定索引
```

9.  `enum`（useful to model choice and keep track of state），可以声明在合约外：
    

```
enum Status {
    Pending,
    Shipped,
    Accepted,
    Rejected,
    Canceled
}

Status public status;
status = Status.Pending;
```

10.  user defined value type：有点难，之后再详细了解。
     
11.  `struct` 结构体：
     

```
struct Todo {
    string text;
    bool completed;
}

Todo[] public todos;

todos.push(Todo(_text, false));
```

12.  三个变量类别：
     

-   `storage`：会存储在链上的数据
    
-   `memory`：可读可写，临时内存
    
-   `calldata`：仅读，一般是外部调用时随交易携带的输入数据；尽量不要拷贝，`calldata[i]` 是直接取值，不会拷贝
    

13.  Cancun 升级后新增 **transient storage**
     

介于 `memory` 和 `storage` 之间：像 `storage` 一样是 key→value（32 字节槽），但只在**同一笔交易（transaction）内**有效，交易结束会自动清零。

-   写入：`assembly { tstore(SLOT, 321) }`
    
    -   `SLOT` 是 `bytes32` 或 `uint256`，是存储槽，不会和 storage 冲突
        
-   读取：`assembly { v := tload(SLOT) }`
    

只要在同一笔交易，都能读到；案例给了一个防止重入攻击的方式，比 `storage` 更便宜。

常规实现：

```
bool private locked;

modifier lock() {
    require(!locked);
    locked = true;
    _;
    locked = false;
}
```

`tstore` 实现：

```
bytes32 constant SLOT = 0;

modifier lock() {
    assembly {
        if tload(SLOT) { revert(0, 0) }
        tstore(SLOT, 1)
    }
    _;
    assembly {
        tstore(SLOT, 0)
    }
}
```

14.  `error`：`require` / `revert` / `assert`
     

```
require(_i > 10, "Input must be greater than 10");

if (_i <= 10) {
    revert("Input must be greater than 10");
}

assert(_i > 10);
```

推荐使用 `if + revert + 自定义错误`：

```
error InsufficientBalance();
error InsufficientBalance(uint256 available, uint256 required);

if (bal < amount) {
    revert InsufficientBalance(bal, amount); // 触发带参数的错误
}
```

15.  `modifier` 修饰符：方便条件判断
     

```
modifier onlyOwner() {
    require(msg.sender == owner, "Not owner");
    _;
}

function changeOwner(address _newOwner)
    public
    onlyOwner
    validAddress(_newOwner)
{
    owner = _newOwner;
}
```
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->






## 今日计划完成情况

1.  完成speed run etheruem challenge 0
    
2.  学习ERC 7962
    

## 笔记

### challenge 0

今天画了好长时间研究speed run etheruem challenge 0，最主要的问题是铸造NFT后不显示，排查过后发现是连接不上IPFS的服务，想了很多办法，最后利用pinata提前传好metadata，铸造的时候随机给一个uri，成功解决，详细的操作明天再整理一下

### ERC 7962

使用公钥的哈希来代表代币归属，把授权和发起交易解耦

链上只记录地址的哈希

任何地址都能提交transfer并支付gas，但交易是否有效取决于签名和nonce deadline
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->







## 学习任务完成情况

1.  021学习以太坊
    

## 笔记

**漏洞与防御**

1.  重入攻击 合约如果先进行外部调用再更新状态，攻击者可以通过回调反复进入合约提取资产 防范： CEI模式，先做参数和权限检查，再更新内部状态，最后做外部调用 使用ReentrancyGuard的nonReetrant修饰符防止执行过程中被递归重入 采用Pull-Payment提款模式，不再函数里主动打钱，而是记录可提取余额，通过单独提现函数领取代币，再架构上隔离
    

2.  由于opcode 的 gas 成本被上调，transfer() / send() 固定 2300 gas 津贴变得不再可靠，更推荐使用低级调用模式，对批量退款或退款等使用Pull-Payment模式，把“给用户打钱”改为“用户来领钱”
    

3.  整数的溢出或下溢，使用大于0.8的版本
    

4.  权限的管理，要使用Ownable等来管理角色，操作多签，时间锁等
    

5.  对低级调用，要仔细检查返回值，所有低级调用都应该显式检查success
    

6.  预言机操控，使用多节点预言机，做平均价格，价差限制，延迟窗口等，尽量避免使用单一数据
    

7.  Dos和Gas，在合约中大量调用外部服务或转账，如果某一地址刻意revert，可能导致整个函数执行失败，造成拒绝服务，在合约设计中，如果依赖“固定gas”可能会在协议升级后不可用
    

8.  业务逻辑缺陷，进行详尽的测试，使用模糊测试和不变式测试
    

9.  随机数不安全
    

10.  可升级/代理合约风险，
     

11.  对delegatecall的目标要十分小心
     

12.  使用safaERC20
     

**合约公开与审计**

1.  合约公开性 字节码公开 交易历史公开 状态变量半公开
    

2.  可审计 源码验证
    

3.  审计方式 人工 自动化可以识别已知漏洞和可疑模式Slither，Mythril 社区审计
    

**合约升级**

因为代码一旦被部署就无法直接修改，因此一旦遇到bug或要更新，相对比较困难

1.  直接替换，部署新合约，将前端指向新地址，迁移数据，在简单情况下可以使用，真正项目不会这么做
    

2.  代理模式，主流，有一套标准的方案，包含两部分：Proxy，代理合约，负责和用户交互，将调用转发到实现合约 Implementation：真正的业务代码，不直接被调用
    

3.  Transparent Proxy Pattern ；UUPS （ Universal Upgradeable Proxy Standard，基 于 EIP-1822 + ERC-1967），升级逻辑写在实现合约里
    

4.  小Tips：检查存储布局兼容； 限制构造函数用法，改用 initializer； 升级权限一定要： 多签（Gnosis Safe 等）； 时间锁 Timelock（例如 24h / 48h）；协议通常会配合治理投票
    

5.  数据与逻辑分离 / 模块化：不同逻辑的合约，存储，放在不同的合约，由此推出了EIP2535
    

**EVM机制**

1\. EVM = 状态机 + CPU

-   状态机：通过旧状态和交易，利用规则计算新状态。
    

-   EVM 作用：
    

-   执行字节码
    

-   维护栈、内存、存储
    

-   扣除 Gas
    

-   生成新状态
    

2\. EVM 组件

-   全局状态（World State）：记录账户信息、余额、合约状态等。
    

-   字节码 & 操作码：EVM 指令集，如 ADD、SSTORE、CALL。
    

-   栈（Stack）：存储操作数，进行算术运算和条件判断。
    

-   内存（Memory）：临时字节数组，存放中间数据。
    

-   存储（Storage）：每个合约独有的永久存储，操作最贵。
    

-   执行上下文：包括 msg.sender、gas 等。
    

-   Gas 计费系统：为每个操作收费，防止滥用。
    

3\. 智能合约的执行流程

1.  编写 & 编译：Solidity 编写合约，生成字节码、ABI、metadata。
    

2.  部署：通过交易上传字节码到链上，EVM 执行初始化并存储合约代码。
    

3.  调用：用户/合约通过交易调用合约，EVM 执行字节码。
    

4.  状态更新 & 共识：执行后更新状态，节点一致性验证。
    

4\. 为什么需要 Gas？

-   防止滥用：限制无限循环和恶意脚本。
    

-   资源按需付费：确保复杂操作和存储写入付费。
    

-   优化交易优先级：验证者根据 Gas 价格优先打包交易。
    

5\. EVM 的硬约束

-   确定性（Deterministic）：合约结果不依赖外部信息，如时间和 HTTP 请求。
    

-   沙盒隔离（Sandboxed Runtime）：不能直接访问外部系统，只能通过链上数据和合约交互。
    

6\. EVM 不仅限于以太坊主网

-   EVM 兼容链：如 Arbitrum、Optimism、Avalanche 等，支持跨链部署。
    

-   L2 方案：通过 Rollup 等方式扩展性能。
    

7\. EVM 重要性

-   统一执行环境：保证全网节点一致执行。
    

-   支持智能合约和 DApps：自动执行合约逻辑，推动 DeFi、NFT 等应用。
    

-   隔离性 & 安全性：提供沙箱环境，防止合约间互相干扰。
    

-   去中心化共识：保证全网状态一致性。
    

8\. EVM 兼容性与创新

-   跨链兼容性：EVM 兼容链极大降低开发成本，支持多链部署。
    

-   创新扩展能力：支持 Layer-2 Rollups、ZK EVM、Gas 优化等。
    

9\. EVM 局限性

-   Gas 费用高：网络拥堵时用户体验差，推动向 L2 转移。
    

-   性能限制（TPS）：每个节点需处理所有交易，扩容需求大。
    

-   升级兼容性压力：改变字节码格式、opcode 时需要考虑现有合约兼容。
    

-   存储效率问题：存储开销大，面临状态膨胀问题。
    

## 明日计划

1.  重点看看合约相关内容
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->








## 学习任务完成情况

1.  阅读021学习以太坊第四章前半部分
    
2.  Web3安全回放
    
3.  周会
    

## 笔记

**1.合约编译产物（Compile Artifacts）**

**Bytecode（字节码）**

-   **Creation Bytecode**：部署时执行（含 constructor）
    
-   **Runtime Bytecode**：部署后链上真正执行的代码
    
-   **Metadata Hash**：CBOR 编码，附在 runtime 末尾（含源码/编译器信息）
    
-   **本质**：EVM 可执行程序（上链内容）
    

**ABI（Application Binary Interface）**

-   **内容**：函数 / 事件 / 参数 / 返回值 / 可见性
    
-   **用途**：
    
    -   链下交互（前端 / 脚本 / 钱包）
        
    -   调用编码 & 返回值解码
        
-   **形式**：JSON（artifacts / out）
    

**Metadata（合约元数据）**

-   **包含**：ABI、编译器版本、源码哈希、NatSpec
    
-   **用途**：源码验证（Etherscan / Sourcify）
    

**高级调试产物**

-   **AST**：源码结构树（静态分析 / 审计）
    
-   **Source Map**：字节码 ↔ 源码行
    
-   **Assembly / Yul IR**：底层执行与优化分析
    

**部署流程**

1.  本地编译 → bytecode + ABI + metadata
    
2.  部署 → 合约地址
    
3.  源码验证 → 浏览器自动展示 ABI
    

**2.智能合约核心组成**

1\. 状态变量（State Variables）

-   永久存储（storage）
    
-   public / internal / private
    
-   **constant / immutable**：省 gas
    

2\. 函数（Functions）

-   可见性：public / external / internal / private
    
-   状态性：pure / view / payable / 默认
    
-   特殊函数：
    
    -   `receive()`：纯 ETH 转账
        
    -   `fallback()`：未知调用 / 无 receive
        

3\. 事件（Events）

-   存在 **logs**（非 storage）
    
-   可监听、低成本
    
-   `indexed`（最多 3 个）便于检索
    

4\. 构造函数（Constructor）

-   仅部署时执行
    
-   初始化状态
    

5\. 修饰符（Modifiers）

-   复用逻辑
    
-   访问控制（如 onlyOwner）
    

6\. Struct / Enum / Custom Errors

-   **struct / enum**：建模业务结构
    
-   **custom error**（≥0.8.4）：
    
    -   比 require(string) 省 gas
        
    -   支持结构化参数
        

**3.部署GAS**

1\. Gas

-   21,000：基础交易
    
-   ~32,000：CREATE
    
-   ~200 gas / byte：runtime code 存储
    
-   constructor 执行逻辑
    
-   storage 初始化：≈20,000 gas / slot（最贵）
    

**结论**：

-   代码越大、初始化越多 → 越贵
    
-   常见部署：**几十万 ~ 几百万 gas**
    

2\. Gas 价格（EIP-1559）

-   **Total Fee = GasUsed × (BaseFee + PriorityFee)**
    
-   Base Fee：协议决定，**直接 burn**
    
-   Priority Fee：给验证者的小费
    
-   参数：
    
    -   maxFeePerGas
        
    -   maxPriorityFeePerGas
        

3\. 影响因素

-   合约复杂度 / 字节码大小
    
-   网络拥堵（Base Fee）
    
-   Priority Fee 设置
    
-   **L1 vs L2**
    
    -   L2（Arbitrum / Optimism / Base）≈ 便宜一个数量级
        
    -   EIP-4844（blob）后更便宜
        

4.控制成本 Checklist

-   本地 / 测试网先部署看 gasUsed
    
-   部署前查 gas tracker
    
-   精简 constructor & storage 写入
    
-   开启 solc optimizer
    
-   优先在 L2 部署
    

### Web3安全

1.  **精准猎杀模式 (Precision Hunting)**
    

-   **目标锁定：** 头部中心化交易所、高TVL（总锁定价值）的DeFi协议等高价值目标。
    

-   **手法特征：** 攻击呈现长期、精密、高投入的特点。例如，在Bybit事件中，攻击者通过**多签钱包前端劫持与篡改**等复杂手段实施渗透，最终实现巨额资产窃取。
    

2.  **广撒网攻击模式 (Broadcasting Attacks)**
    

-   **目标锁定：** 广泛的普通Web3用户群体。
    

-   **手法特征：** 以低成本大量散布钓鱼链接、虚假空投、欺诈性项目（特别是**Rug Pull**）等，利用用户贪心或疏忽心理，积少成多骗取资产。这是每位用户日常最可能遭遇的风险。
    

3.  **AI驱动的社会工程学攻击**
    

-   **新型武器：** **Deepfake（深度伪造）** 技术在2025年爆发式增长，被用于伪造项目方、KOL甚至熟人身份。
    

-   **攻击案例：** 如Uxlink事件中，攻击者通过伪造管理员身份，诱导用户点击恶意链接或安装恶意软件，实现账户渗透。
    

4.  **技术升级伴生的新型风险**
    

-   **典型案例：** 以太坊**EIP-7702** 是一项旨在赋予普通账户智能合约能力的重大升级。然而，黑客迅速利用其“合约能力”特性，设计出新型钓鱼陷阱（如伪装授权交易），诱骗用户转账，导致升级初期链上恶意交易占比畸高。
    

### **防御指南**

-   “**不点**（未知链接）、**不签**（不明授权）、**不装**（未经验证的软件）、**不转**（向未核实的地址转账）”。
    

-   始终坚持从**官方渠道**下载应用；对任何交易授权（签名）保持警惕，**拒绝盲签**；转账前**反复核对合约/钱包地址**。
    

**项目端：开发者安全生命周期**

-   **核心意识：** 建立覆盖项目**全生命周期的安全管理**体系。
    

-   **关键阶段：**
    

-   **开发期：** 将安全规范（如代码审计）嵌入开发流程。
    

-   **运营期：** 加强链上链下的实时监控与异常检测。
    

-   **应急期：** 提前建立并演练**安全事件应急响应**机制，确保在遭遇攻击时能快速止损与恢复。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->









## 今日学习任务完成情况

1.  021学习以太坊第三章
    

## 笔记

以太坊里有两种账户EOA和合约账户，EOA全称externally owned account，有私钥，能主动发动交易，合约地址无私钥，只能被调用，有代码和存储

每个账户都有固定字段：address，nonce（操作计数器，避免重放攻击），balance（以wei计数，1ETH=10e18wei），codeHash（EVM字节码的哈希，用来标识和校验代码，EOA为空），storageRoot（Merkle-Patricia Trie）的根哈希，指向账户的存储树，EOA为空），因为account四元组会被编码进状态trie

**EOA**

1.  交易通过私钥签名发起，被节点通过公钥验证，通过助记词派生（一套助记词能派生出对应钱包里的所有EOA地址），nonce防止攻击者把已经签过的交易反复执行
    
2.  BIP39：生成一个随机的种子，使用哈希算法生成一组助记词12/24或更多
    
3.  BIP44旨在通过一个种子回复不同币种的钱包，定义了一种路径格式m / purpose' / coin\_type' / account' / change / address\_index
    
4.  EIP-55：标准化地址，生成地址是基于哈希决定哪些大写哪些小写，这样可以通过检查EIP-55格式确定其是否是正确的以太坊地址
    

**合约账户**

1.  合约被编写，编译器将其编译成EVM字节码，包括两部分init code（初始化合约的代码）和runtime code（合约真正运行时的代码） 一笔创建合约的交易，to字段为空，data字段写入init code，发起者可以说EOA，也可以是链上的工厂合约
    
2.  EVM执行init code，最终新的合约账户出现在状态树中，拥有一个新地址，一段runtime bytecode和初始存储状态
    
3.  创建合约地址有create和create2，create由创建者地址和创建者nonce决定，create2合约地址由创建者地址+salt（开发者定义的32字节的）+init\_code决定，可以被精确计算（和nonce无关），最大的价值是提供了可预测地址，跨链和跨环境具有一致性
    
4.  以太坊地址本质上时一串20字节（160bit）的二级制数据，正好是40个16进制，为了表示后面的内容是16进制（0-9、a-f），所以常常以0x开头，于是总共是42个字符
    
5.  ERC-4337 EIP-7702：Account Abstraction，账户抽象，在一定程度上模糊了EOA和合约账户的界限，让钱包更加智能灵活。ERC-4337包括三个机制：User Operations用户操作，Bundler：打包者，Paymaster“支付者，可以让钱包使用者不用自己支付gas（项目方支付）
    
6.  合约升级早期存在删除合约重新create2部署，现在已经不能删除，调用selfdestruct只会转走余额，不会删除代码和nonce等，除非是在创建合约的同一笔交易中把他selfdestruct，目前的合约升级要依靠代理合约
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->










## 今日任务完成情况

1.  学习实习手册合规与网络安全
    

## 笔记

### Web3实习手册——合规与网络安全

在Web3领域的各种活动中，从个人角度来说，任何行为前可以考虑两个问题：我又没有做出违法行为；我的行为会不会损害自己的利益

**合规**

1.  核心法律风险  
    代币发行，命令禁止  
    一些项目机制触碰了赌博（盲盒，转盘）、传销（邀请，推广）等  
    虚拟货币的场外交易可能涉及非法经营外汇、协助洗钱  
    个人商家的交易没有法律保障，委托资产等都有可能触碰法律
    
2.  全球监管发展  
    整体上越来越多的监管条例法案都在出台，有利于产业合规化发展  
    FATF，反洗钱反恐怖融资的标准制定者，提出Travel Rule，要求虚拟资产服务商收集发送方和接收方信息（适用于超过1000美元/欧元）  
    美国去年出台了稳定币法案，稳定币是DeFi的基础，如果出现问题会引发系统性问题，对传统金融体系产生冲击，尤其是跨境支付  
    对稳定币的关键要求：i. 发行商需持有等值资产 ii.面临更严格的监管 iii.算法稳定币被禁止  
    整体更严格，跨国协调，创新友好，合规成本增加，准入门槛提高，长期是利好行业发展的
    
3.  入职风险  
    雇佣关系和国内的社会保障体系不相符  
    薪酬的支付方式，虚拟货币出入金的风险  
    项目本身的合规性，有可能会在不清晰的情况下触碰法律
    

**安全**

1.  常见攻击方式  
    钓鱼攻击，伪造信息诱导点击恶意连接，输入敏感信息或下载恶意软件  
    恶意软件或木马，控制电脑行为窃取敏感数据，剪贴板劫持，将地址替换  
    社会工程学，人是最大的不确定性  
    第三方依赖攻击，在开源软件或正规软件中安装（替换）后门  
    弱密码等传统风险
    
2.  防护清单  
    只用官方软件  
    任何要求连接钱包，签名的活动，不要相信  
    主钱包冷储存，活动用测试钱包  
    不轻信他人的连接等，熟人也不能轻易相信  
    账号和密码安全，启用2FA  
    转帐前检查地址前6和后4位  
    遭受攻击第一时间断网截屏，记录并寻求专业帮助
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->











## 学习计划完成情况

1.  021学习以太坊教材第一章
    
2.  以太坊官方学习指南
    
3.  实操任务
    

## 笔记

### 021学习以太坊

1.  以太坊核心特性  
    智能合约：以太坊上运行的程序，自动执行，运行需要消耗eth，公开透明  
    dAPPs：不依赖单一服务器的应用程序  
    代币：NFT就是一种不可替代的代币  
    权益证明：质押门票32ETH成为验证者，恶意行为会触发slashing，典型行为包括，提供双重区块，投票双重区块，环绕投票，会立即罚没一部分质押并进入约36天的移除期  
    Gas：运行的燃料，代码越复杂，燃料越高  
    Proto-Danksharding：在Dencun升级中上线主网，引入了新的blob-carring transactions，为rollup提供临时数据空间，因为会被丢弃，减轻了L1的存储 压力和成本  
    Danksharding：目标，将区块数据空间扩展到更多的blob  
    叔块：Pow时期的概念，为了给予未被选中的矿工补偿
    
2.  dAPPs的概念与应用  
    dAPPs包括两部分，放在链式的智能合约负责运算，放在链下（传统服务器或去中心化储存）的前端用来和用户交互  
    核心特点：去中心化、确定性、图灵完备、隔离性（不会影响网络上的其他运算）  
    优点：零停机时间、匿名、抗审查、不可篡改、天然无需验证（代码写的很清楚）  
    缺点：维护（因为一旦部署就无法修改，只能靠事先设计好的升级模式），性能和扩展困难，网络拥堵会提高gas消耗，用户门槛导致在一些开发过程中出现再中心化（二者的平衡很重要）
    
3.  去中心化机制  
    共识与出块：在slot（时隙）中，随机选择出proposer产出区块，数百个同时进行attestation，Pectra升级后将余额的上限提高到了2048ETH  
    节点与网络：任何人都可以运行节点，给予DevP2P协议运行点对点网络，轻客户端能够只验证少量签名参与  
    多客户端：包括执行层和共识层客户端，社区鼓励客户端均衡发展  
    PBS：避免了价值将验证者推向高度专业化寡头的压力  
    Roll-up：分层执行，L1做结算和数据可用性 L2做大规模吞吐  
    治理：开放透明民主的共治
    
4.  应用相关  
    DeFi：连接“钱”，自动做市商和去中心化交易所，去中心化借贷、稳定币、跨链和rollup的扩展  
    NFT：连接“物”，收藏品、角色化  
    社交和DAO：连接“人”，治理dAPPs，参与公共事务，构建新型社交方式
    

### 以太坊官网

1.  测试
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/shelterlove/images/2026-01-13-1768275119888-image.png)
2.  overviwe更像是一个窗口，以太坊官网有非常多优质的信息和学习资源，未来要多多学习
    

### 思考与回答（感谢GPT老师）

Q：Blob存的什么数据，为什么可以丢弃？  
执行层不需要blob的内容，本质上只是用来验证，在共识节点被下载，一定时间后可删除

Q：除了参与验证，节点应该还参与智能合约的计算？  
合约的操作会在全节点上执行和验证，不质押也可以参与验证，但不会参与共识和出块

Q：NFT的价值来自什么

本质上来自共识和文化，协议定义的稀缺性，流动性和投机往往贡献了价格的波动，所有权和版权方面也存在一些空白

### 以太坊周会回放

周会的内容非常丰富，我标记下周会里一些自己感兴趣的内容吧

1.  V神提出行业需要更好的去中心化稳定币，三大难题：i优于美元价格锚定 ii 攻击成本要大于市值 iii解决稳定币与质押收益、CDP机制间的竞争
    
2.  X将开发智能资产标签，内置资产标记，未来可能会内置solana
    
3.  25年牛市没有到来，没有大量新一代入局，26年可能会进入资源整合的时代
    
4.  ETH的L1协议发展很好，zk EVM发展迅速，基建在发展，但应用不足，清晰法案通过，未来会进入后泡沫时代
    
5.  预测市场继续发展，未来巨无霸的一体化应用可能会受到追捧；永续合约值得关注，竞争非常激烈，没有明显护城河
    
6.  ETH和solana在分化，ETH和资产发行更紧密，solana侧重去中心化交易所
    
7.  Tempo链，面向稳定币支付用的Layer1，在协议层为支付设计，内置换币付费机制，为交易专门预留空间，开发商依然是EVM
    
8.  ERC4804，定义如何把类似网页的URL翻译成EVM的call message，方便用户直接访问链上内容，不用通过web2的代理服务
    

### 明日计划

1.  继续完成任务赚积分
    
2.  阅读学习材料
    
3.  看Web2toWeb3的课程
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->













## 个人介绍

在正式开始笔记前先做个简单的自我介绍，不知道会不会有人看到这里，我是shelter，base厦门，算是一名海洋生物学家（自封的，其实还在读研），去过南海，去过西北太平洋，采集过浮游植物也迫害过浮游动物，吐的昏天黑地过也看过大洋上的银河，做过化学分析也写过遥感算法，没什么太深的计算机背景。偶然的机会了解到区块链和web3，被他的理念所吸引，给我看不到希望的科研生活打开了另一扇窗口（天坑专业孩子哭啊，当初真该无脑CS的），希望能拓宽自己世界的边界，看到更多的可能性。个人平时爱好不多，喜欢看书，看电影和动漫，偶尔徒徒步，喜欢卡尔维诺、茨威格、鲁迅和汪曾祺，欢迎大家与我交流，在厦门的小伙伴也可以跟我面基。

## 学习计划完成情况

1.  学习Web3实习手册入门导读部分
    
2.  完成工具的安装
    
3.  参加Co-learning共学
    
4.  参加分享会
    

## Web3实习手册入门导读笔记

### 区块链基础知识

1.  区块像区块链世界的建筑材料，每隔一段时间就会被打包出来一个放在链上，打包者就是矿工，打包后要经过所有节点（51%）的验证通过才会被放进链上，这也就保证了不可篡改性  
    一个区块包含两部分内容：block header和block body，header里是一些基本信息，body里是具体的交易
    
2.  代币和去中心化网络中是区块链的核心，代币作为激励提供给矿工，保证了网络的运转
    
3.  区块链有公链、联盟链和私链之分，公链像无政府，联盟链像欧盟，私链像私人服务器
    
4.  Web3.0居然不是Web3，Web3.0强调信息智能化和语义的连接，我的理解就是让网页的互联走向信息的互联，让互联网更智能，Web3是让网页走向去中心化，让互联网更公开透明，能够承载价值
    

### 以太坊概览

1.  以太坊最关键的创新在于其可以编程，在这个基础上衍生出了一系列DeFi、NFT、DAO等新生的事物
    
2.  以太坊一直在升级，关键一次是从PoW向Pof的合并，现在以太坊内部有两层  
    i. 执行层CL：原先的主网，用来处理交易和合约运算  
    ii.共识层EL：负责对区块进行投票等操作
    
3.  以太坊还在升级，重点在提升速度和安全性，同时降低成本，主要有三个方向  
    i. 数据分片，配合Layer2提供存储  
    ii. Layer2在链下处理交易，将交易打包到主网  
    iii. ZK，零知识证明，只需验证证明
    
4.  核心机制  
    i. 账户（或者说是地址？），包括外部账户EOA（相当于钱包）和合约账户CA（相当于合约本身）  
    ii. Gas fee=Gas Limit\*Gas Price，limit只是上限，多余的会退回，目前的gas一部分会burn掉，另一部分会作为小费会用来鼓励矿工  
    iii. EVM，运行合约的虚拟计算机，全球统一
    

### 行业概览

1.  DeFi，去中心化的金融，由公式支配的透明金融世界
    
2.  NFT，数字资产，对所有权的保障
    
3.  DAO，自治组织，个人看来是理想的社会形式
    
4.  MEME，个人不感兴趣，感觉是情绪化体现
    
5.  交叉和新趋势：个人感觉AI和Web3的交叉很有吸引力，Web3赋能乡村建设很有实际意义和启发
    

### 工作相关

1.  软件部分需要逐渐上手，一些经验Tips需要在未来的实操中逐渐体会，未来可以经常回头看看
    

## 思考和回答（实际上的GPT老师答的）

Q：挖矿和验证具体是什么机制

记账很简单，但决定谁写下来的能进入账本的过程就是挖矿，也就是PoW，要不断调整一个可变数，对区块头做哈希，找到一个哈希满足要求，本质上就靠暴力计算，但验证这个哈希满不满足要求很容易  
验证这个名词对应很多个概念

i.验证交易是否合理，签名对不对，余额够不够

ii.验证这个区块对不对，能不能被放在链上，要检查parent hash，检查它是否通过共识（这部分就分成PoW和PoS）

iii.在PoS中，实际上是质押验证，先抵押一笔钱，然后随机选一组验证者负责propose新区块，其他验证者做attest，捣乱的会被没收质押的财产

Q：私链难道不违背去中心化的思想  
在一些情况下可能会违背一些叙事，但关键看应用场景，在一些情况下完全的去中心化会带来危险，私链适用于这些情景

## 明日计划

1.  完成实操的一些任务
    
2.  学习xiaohai老师的教材
    
3.  补一下周会的知识
    
4.  阅读拓展辅助材料
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
