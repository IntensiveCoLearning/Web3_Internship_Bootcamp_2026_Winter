---
timezone: UTC+8
---

# ChiWon

**GitHub ID:** CHI-WON

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->
# day13

\[x\]搭建了本地区块链节点

\[x\]完成了Cyfrin Updraft中Uniswap V2的前两节内容
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

# DAY12

\[x\] Gas优化

\[x\] 漏洞修复

## Gas优化

优化前的合约：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    
    constructor(string memory _name, string memory _symbol, uint8 _decimals, uint256 _initialSupply) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _initialSupply * 10 ** _decimals;
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }
    
    function transfer(address to, uint256 amount) public returns (bool) {
        require(balanceOf[msg.sender] >= amount, "Insufficient balance");
        
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
        
        emit Transfer(msg.sender, to, amount);
        return true;
    }
    
    function approve(address spender, uint256 amount) public returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }
    
    function transferFrom(address from, address to, uint256 amount) public returns (bool) {
        require(balanceOf[from] >= amount, "Insufficient balance");
        require(allowance[from][msg.sender] >= amount, "Allowance exceeded");
        
        balanceOf[from] -= amount;
        balanceOf[to] += amount;
        allowance[from][msg.sender] -= amount;
        
        emit Transfer(from, to, amount);
        return true;
    }
}
```

优化后的合约：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract OptimizedToken {
    // 使用 bytes32 替代 string 存储名称和符号（如果可能）
    bytes32 public immutable name;
    bytes32 public immutable symbol;
    uint8 public immutable decimals;
    uint256 public totalSupply;
    
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    
    // 使用 indexed 参数减少 gas
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    
    constructor(string memory _name, string memory _symbol, uint8 _decimals, uint256 _initialSupply) {
        // 转换为 bytes32 节省存储空间
        name = _toBytes32(_name);
        symbol = _toBytes32(_symbol);
        decimals = _decimals;
        
        uint256 supply = _initialSupply * 10 ** _decimals;
        totalSupply = supply;
        
        // 直接赋值而非计算
        balanceOf[msg.sender] = supply;
        
        // 使用 address(0) 的预定义值
        emit Transfer(address(0), msg.sender, supply);
    }
    
    function transfer(address to, uint256 amount) external returns (bool) {
        // 使用外部函数 (external) 减少 gas
        _transfer(msg.sender, to, amount);
        return true;
    }
    
    function approve(address spender, uint256 amount) external returns (bool) {
        // 避免 front-running 攻击的模式
        _approve(msg.sender, spender, amount);
        return true;
    }
    
    function transferFrom(address from, address to, uint256 amount) external returns (bool) {
        // 先检查授权，使用 unchecked 节省 gas
        uint256 currentAllowance = allowance[from][msg.sender];
        require(currentAllowance >= amount, "Allowance exceeded");
        
        // unchecked 块：我们知道减法不会下溢
        unchecked {
            allowance[from][msg.sender] = currentAllowance - amount;
        }
        
        _transfer(from, to, amount);
        return true;
    }
    
    // 内部函数，避免重复代码
    function _transfer(address from, address to, uint256 amount) internal {
        require(to != address(0), "Transfer to zero address");
        
        uint256 fromBalance = balanceOf[from];
        require(fromBalance >= amount, "Insufficient balance");
        
        // unchecked 块：余额检查确保不会下溢，加法使用 SafeMath 模式
        unchecked {
            balanceOf[from] = fromBalance - amount;
            balanceOf[to] += amount; // 假设不会溢出（总供应量限制）
        }
        
        emit Transfer(from, to, amount);
    }
    
    function _approve(address owner, address spender, uint256 amount) internal {
        require(spender != address(0), "Approve to zero address");
        
        allowance[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    
    // 批量转账，减少交易次数
    function batchTransfer(address[] calldata recipients, uint256[] calldata amounts) external {
        require(recipients.length == amounts.length, "Arrays length mismatch");
        
        uint256 total = 0;
        uint256 length = recipients.length;
        
        // 预先计算总数，一次性检查余额
        for (uint256 i = 0; i < length; ) {
            total += amounts[i];
            unchecked { i++; }
        }
        
        require(balanceOf[msg.sender] >= total, "Insufficient balance");
        
        // 执行转账
        for (uint256 i = 0; i < length; ) {
            address recipient = recipients[i];
            uint256 amount = amounts[i];
            
            if (amount > 0 && recipient != address(0)) {
                unchecked {
                    balanceOf[msg.sender] -= amount;
                    balanceOf[recipient] += amount;
                }
                emit Transfer(msg.sender, recipient, amount);
            }
            
            unchecked { i++; }
        }
    }
    
    // 辅助函数：将短字符串转换为 bytes32
    function _toBytes32(string memory str) private pure returns (bytes32) {
        bytes memory b = bytes(str);
        require(b.length <= 32, "String too long");
        
        bytes32 result;
        assembly {
            result := mload(add(str, 32))
        }
        return result;
    }
    
    // Gas优化：增加增量和减量函数
    function increaseAllowance(address spender, uint256 addedValue) external returns (bool) {
        _approve(msg.sender, spender, allowance[msg.sender][spender] + addedValue);
        return true;
    }
    
    function decreaseAllowance(address spender, uint256 subtractedValue) external returns (bool) {
        uint256 currentAllowance = allowance[msg.sender][spender];
        require(currentAllowance >= subtractedValue, "Decrease below zero");
        _approve(msg.sender, spender, currentAllowance - subtractedValue);
        return true;
    }
}
```

## 漏洞修复

漏洞合约：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

// ⚠️ 含有多个安全漏洞的拍卖合约
contract VulnerableAuction {
    address public owner;
    address public highestBidder;
    uint256 public highestBid;
    uint256 public auctionEndTime;
    bool public auctionEnded;
    
    mapping(address => uint256) public pendingReturns;
    
    event NewHighestBid(address bidder, uint256 amount);
    event AuctionEnded(address winner, uint256 amount);
    
    constructor(uint256 _biddingTime) {
        owner = msg.sender;
        auctionEndTime = block.timestamp + _biddingTime;
    }
    
    // 漏洞1：重入漏洞
    function bid() external payable {
        require(block.timestamp <= auctionEndTime, "Auction ended");
        require(msg.value > highestBid, "Bid too low");
        
        // 退款时可能触发重入攻击
        if (highestBidder != address(0)) {
            pendingReturns[highestBidder] += highestBid;
        }
        
        highestBidder = msg.sender;
        highestBid = msg.value;
        
        emit NewHighestBid(msg.sender, msg.value);
    }
    
    // 漏洞2：不安全的提款函数
    function withdraw() external {
        uint256 amount = pendingReturns[msg.sender];
        if (amount > 0) {
            // 重入攻击点：状态更新在转账之后
            pendingReturns[msg.sender] = 0;
            
            (bool success, ) = msg.sender.call{value: amount}("");
            require(success, "Transfer failed");
        }
    }
    
    // 漏洞3：权限控制不严
    function endAuction() external {
        require(block.timestamp >= auctionEndTime, "Auction not ended");
        require(!auctionEnded, "Auction already ended");
        
        auctionEnded = true;
        
        // 直接转账，没有使用pull模式
        (bool success, ) = owner.call{value: highestBid}("");
        require(success, "Transfer failed");
        
        emit AuctionEnded(highestBidder, highestBid);
    }
    
    // 漏洞4：时间戳依赖
    function extendAuction() external {
        // 矿工可能操纵时间戳
        if (block.timestamp >= auctionEndTime - 1 hours) {
            auctionEndTime += 1 hours;
        }
    }
    
    // 漏洞5：整数溢出（旧版本）
    function addBid(uint256 amount) external {
        // 旧版本Solidity中可能溢出
        pendingReturns[msg.sender] += amount;
    }
    
    // 漏洞6：缺少输入验证
    function setNewOwner(address newOwner) external {
        // 缺少权限检查！
        owner = newOwner;
    }
    
    // 漏洞7：不安全的随机数生成
    function selectRandomWinner() external view returns (address) {
        // 可被矿工操纵
        uint256 random = uint256(keccak256(abi.encodePacked(block.timestamp, blockhash(block.number - 1))));
        return highestBidder; // 伪随机选择
    }
}
```

修复合约：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

/**
 * @title 安全拍卖合约
 * @dev 修复了所有已知漏洞的安全版本
 * @notice 采用防重入、权限控制、安全数学等最佳实践
 */
contract SecureAuction {
    using SafeMath for uint256;
    
    address public owner;
    address public highestBidder;
    uint256 public highestBid;
    uint256 public auctionEndTime;
    bool public auctionEnded;
    
    // 使用更安全的数据结构
    struct Bid {
        address bidder;
        uint256 amount;
        uint256 timestamp;
        bool refunded;
    }
    
    Bid[] public bids;
    mapping(address => uint256) public pendingReturns;
    
    // 重入防护
    bool private locked;
    
    // 自定义错误（更省gas）
    error AuctionAlreadyEnded();
    error AuctionNotEnded();
    error BidTooLow(uint256 currentHighest);
    error Unauthorized();
    error ReentrancyGuard();
    error TransferFailed();
    error InvalidAddress();
    
    event NewHighestBid(address indexed bidder, uint256 amount);
    event AuctionEnded(address indexed winner, uint256 amount);
    event Withdrawal(address indexed recipient, uint256 amount);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    
    // 修饰器：防重入
    modifier nonReentrant() {
        if (locked) revert ReentrancyGuard();
        locked = true;
        _;
        locked = false;
    }
    
    // 修饰器：仅所有者
    modifier onlyOwner() {
        if (msg.sender != owner) revert Unauthorized();
        _;
    }
    
    // 修饰器：拍卖进行中
    modifier auctionActive() {
        if (block.timestamp > auctionEndTime) revert AuctionAlreadyEnded();
        if (auctionEnded) revert AuctionAlreadyEnded();
        _;
    }
    
    // 修饰器：拍卖已结束
    modifier auctionEndedOnly() {
        if (block.timestamp <= auctionEndTime) revert AuctionNotEnded();
        if (auctionEnded) revert AuctionAlreadyEnded();
        _;
    }
    
    constructor(uint256 _biddingTime) {
        if (_biddingTime == 0) revert("Bidding time must be > 0");
        
        owner = msg.sender;
        auctionEndTime = block.timestamp + _biddingTime;
    }
    
    /**
     * @dev 安全出价函数
     * @notice 采用Checks-Effects-Interactions模式防止重入
     */
    function bid() external payable auctionActive nonReentrant {
        if (msg.value <= highestBid) {
            revert BidTooLow(highestBid);
        }
        
        // Checks: 参数验证
        require(msg.sender != address(0), "Invalid address");
        
        // Effects: 先更新状态
        address previousBidder = highestBidder;
        uint256 previousBid = highestBid;
        
        highestBidder = msg.sender;
        highestBid = msg.value;
        
        // 记录出价历史
        bids.push(Bid({
            bidder: msg.sender,
            amount: msg.value,
            timestamp: block.timestamp,
            refunded: false
        }));
        
        // 记录退款（使用SafeMath）
        if (previousBidder != address(0)) {
            pendingReturns[previousBidder] = pendingReturns[previousBidder].add(previousBid);
        }
        
        emit NewHighestBid(msg.sender, msg.value);
        
        // Interactions: 最后处理外部调用
        // 这里没有立即转账，使用pull模式
    }
    
    /**
     * @dev 安全提款函数
     * @notice 采用pull模式，避免push模式的危险
     */
    function withdraw() external nonReentrant {
        uint256 amount = pendingReturns[msg.sender];
        
        if (amount == 0) {
            revert("No funds to withdraw");
        }
        
        // Effects: 先更新状态
        pendingReturns[msg.sender] = 0;
        
        // Interactions: 后执行转账
        _safeTransferETH(msg.sender, amount);
        
        emit Withdrawal(msg.sender, amount);
    }
    
    /**
     * @dev 结束拍卖（仅所有者）
     * @notice 将最高出价转给所有者，采用pull模式
     */
    function endAuction() external onlyOwner auctionEndedOnly nonReentrant {
        // 更新状态
        auctionEnded = true;
        
        emit AuctionEnded(highestBidder, highestBid);
        
        // 不立即转账，让所有者自己提取
        pendingReturns[owner] = pendingReturns[owner].add(highestBid);
    }
    
    /**
     * @dev 所有者提取拍卖所得
     */
    function withdrawProceeds() external onlyOwner nonReentrant {
        require(auctionEnded, "Auction not ended");
        
        uint256 amount = pendingReturns[owner];
        require(amount > 0, "No proceeds to withdraw");
        
        pendingReturns[owner] = 0;
        _safeTransferETH(owner, amount);
    }
    
    /**
     * @dev 延长拍卖时间（带限制）
     * @param additionalTime 延长时间（秒）
     */
    function extendAuction(uint256 additionalTime) external onlyOwner auctionActive {
        require(additionalTime <= 1 days, "Cannot extend more than 1 day");
        require(additionalTime > 0, "Additional time must be positive");
        
        // 最大拍卖时间限制
        uint256 maxAuctionTime = auctionEndTime + 7 days;
        uint256 newEndTime = auctionEndTime.add(additionalTime);
        
        if (newEndTime > maxAuctionTime) {
            revert("Auction cannot exceed maximum duration");
        }
        
        auctionEndTime = newEndTime;
    }
    
    /**
     * @dev 安全的所有权转移
     * @param newOwner 新的所有者地址
     */
    function transferOwnership(address newOwner) external onlyOwner {
        if (newOwner == address(0)) revert InvalidAddress();
        
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }
    
    /**
     * @dev 紧急停止（Circuit Breaker模式）
     * @notice 仅在紧急情况下使用
     */
    function emergencyStop() external onlyOwner {
        auctionEnded = true;
        // 可以添加更多紧急逻辑
    }
    
    /**
     * @dev 获取合约ETH余额
     */
    function getBalance() external view returns (uint256) {
        return address(this).balance;
    }
    
    /**
     * @dev 获取出价数量
     */
    function getBidCount() external view returns (uint256) {
        return bids.length;
    }
    
    /**
     * @dev 安全ETH转账
     * @param to 收款地址
     * @param value 转账金额
     */
    function _safeTransferETH(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}("");
        if (!success) revert TransferFailed();
    }
    
    /**
     * @dev 接收ETH（如果需要）
     */
    receive() external payable {
        revert("Direct ETH transfers not allowed");
    }
    
    /**
     * @dev 回退函数
     */
    fallback() external payable {
        revert("Function not found");
    }
}

/**
 * @title SafeMath库
 * @dev 安全的数学运算，防止溢出和下溢
 */
library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }
    
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
    }
    
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) return 0;
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }
    
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }
}
```
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


# DAY11

周始，观废寝忘食刷榜、寻到offer者甚多，顿觉无力，浑噩踱步，不知所向

\[x\] Javascript

虽然之前有跟教程完整过了一遍Hardhat开发流程，但真到自己独自开发时才觉还有很多知识点需要补充，刚写完两个合约，但是不知道怎么写测试，查阅一会还得从最基本的Javascript语法开始看起。

今日已完成：简单的Javascript语法
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->




# DAY10

\[x\]Uniswap

\[x\]一个Chat APP的合约编写

## uniswap

1.  **学习 Uniswap V2 前需要掌握的知识背景**
    
2.  **Uniswap V​​2 的核心机制（AMM、恒定乘积公式等）**
    
3.  **Uniswap V2 的核心合约结构**
    
4.  **关键合约代码逐行解释（以核心的 Pair 合约为例）**
    
5.  **如何与 Uniswap V2 交互（简要开发示例）**
    

* * *

## 一、学习 Uniswap V2 前需掌握的知识背景

### 1\. Solidity 基础

-   合约结构、状态变量、函数、修饰器（modifier）
    
-   `mapping`、`address`、`uint256` 等基本类型
    
-   `require`、`revert` 错误处理
    
-   `call`、`transfer`、`delegatecall`（尤其是 `call` 调用外部合约）
    

### 2\. ERC20 标准

-   `transfer`, `transferFrom`, `approve`, `balanceOf`, `totalSupply`
    
-   授权机制（allowance）和重入风险（虽然 V2 通过先转账后回调规避了部分问题）
    

### 3\. 以太坊交易与 Gas 模型

-   外部账户（EOA） vs 合约账户
    
-   交易调用 vs 合约内部调用
    
-   Gas 成本意识（例如避免在循环中操作）
    

### 4\. 基础密码学与安全常识

-   整数溢出（Solidity 0.8+ 已内置检查）
    
-   重入攻击（Reentrancy）——V2 使用“Checks-Effects-Interactions”模式防御
    
-   前端运行（Front-running）与 MEV
    

* * *

## 二、Uniswap V2 核心机制

### 1\. 自动做市商（AMM）

-   不依赖订单簿，而是通过算法提供流动性
    
-   用户与“池子”（Liquidity Pool）交易，而不是与其他用户
    

### 2\. 恒定乘积公式（x \* y = k）

-   假设池中有两种代币 A 和 B，数量分别为 x 和 y
    
-   交易前后满足：x \* y ≥ k（实际因手续费略大于 k）
    
-   价格由池中比例决定：price = y / x（B 相对于 A 的价格）
    

### 3\. 流动性提供者（LP）

-   提供两种代币，获得 LP Token（代表份额）
    
-   交易费（0.3%）按比例分配给 LP
    

### 4\. 闪电贷（Flash Swap）

-   允许先取走资产，只要在同一笔交易结束前归还 + 手续费即可
    
-   V2 通过 `swap` 函数 + 回调实现
    

* * *

## 三、Uniswap V2 核心合约结构

Uniswap V2 主要由三个合约组成：

| 合约 | 功能 |
| --- | --- |
| Factory | 创建 Pair（交易对）合约，记录所有 Pair 地址 |
| Pair | 实际的交易/流动性池逻辑（每个交易对一个实例） |
| Router | 用户友好的入口，封装复杂逻辑（如添加流动性、多跳交易） |

> 注意：**Pair 合约是核心**，所有交易、流动性操作都在这里发生。

* * *

## 四、Pair 合约代码逐行解析（简化版）

我们以 [UniswapV2Pair.sol](https://github.com/Uniswap/v2-core/blob/master/contracts/UniswapV2Pair.sol) 为例，逐段讲解。

> 注：以下为简化+注释版本，便于理解。实际代码更严谨。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity =0.5.16;

import './interfaces/IUniswapV2Pair.sol';
import './UniswapV2ERC20.sol'; // LP Token 逻辑
import './libraries/Math.sol';
import './libraries/UQ112x112.sol';

contract UniswapV2Pair is IUniswapV2Pair, UniswapV2ERC20 {
    using SafeMath for uint256;
    using UQ112x112 for uint224;

    string public constant name = 'Uniswap V2';
    string public constant symbol = 'UNI-V2';

    uint112 private reserve0;           // 代币0的储备量
    uint112 private reserve1;           // 代币1的储备量
    uint32  private blockTimestampLast; // 上次更新时间（用于价格累积）

    uint256 public price0CumulativeLast; // 价格累积器（用于 TWAP）
    uint256 public price1CumulativeLast;

    uint256 public kLast; // 用于计算 LP 提现时的费用（fee-on-transfer）

    address public factory; // 工厂地址
    address public token0;  // 代币0地址（较小地址）
    address public token1;  // 代币1地址（较大地址）

    // 初始化 Pair（只能调用一次）
    function initialize(address _token0, address _token1) external {
        require(msg.sender == factory, 'UniswapV2: FORBIDDEN'); // 只能由工厂初始化
        token0 = _token0;
        token1 = _token1;
    }
```

### 关键点解释：

-   `reserve0` / `reserve1`：池中两种代币的数量（112位足够大）
    
-   `blockTimestampLast`：用于时间加权平均价格（TWAP）
    
-   `initialize`：由 Factory 调用，设定 token0/token1（按地址大小排序）
    

* * *

### Mint（添加流动性）

```solidity
function mint(address to) external lock returns (uint256 liquidity) {
    uint256 _reserve0 = reserve0;
    uint256 _reserve1 = reserve1;
    uint256 balance0 = IERC20(token0).balanceOf(address(this));
    uint256 balance1 = IERC20(token1).balanceOf(address(this));

    // 计算新存入的代币量
    uint256 amount0 = balance0.sub(_reserve0);
    uint256 amount1 = balance1.sub(_reserve1);

    bool feeOn = _mintFee(_reserve0, _reserve1); // 是否开启协议费（默认关闭）
    uint256 _totalSupply = totalSupply;

    if (_totalSupply == 0) {
        // 首次添加流动性：LP = sqrt(amount0 * amount1)
        liquidity = Math.sqrt(amount0.mul(amount1));
    } else {
        // 后续添加：按比例 mint LP
        liquidity = Math.min(
            amount0.mul(_totalSupply) / _reserve0,
            amount1.mul(_totalSupply) / _reserve1
        );
    }

    require(liquidity > 0, 'UniswapV2: INSUFFICIENT_LIQUIDITY_MINTED');

    _mint(to, liquidity); // 给用户 mint LP token

    _update(balance0, balance1, _reserve0, _reserve1); // 更新储备 & 价格累积
    emit Mint(msg.sender, amount0, amount1);
}
```

重点：

-   **首次添加流动性**：必须按当前市场价格提供，否则会被套利
    
-   **LP 数量计算**：确保新 LP 与已有 LP 比例一致
    
-   `_update`：更新 reserve 和价格累积器
    

* * *

### Swap（交易）

```solidity
function swap(
    uint256 amount0Out,
    uint256 amount1Out,
    address to,
    bytes calldata data
) external lock {
    require(amount0Out > 0 || amount1Out > 0, 'UniswapV2: INSUFFICIENT_OUTPUT_AMOUNT');
    (uint256 balance0, uint256 balance1) = getReserves();
    require(amount0Out < balance0 && amount1Out < balance1, 'UniswapV2: INSUFFICIENT_LIQUIDITY');

    uint256 balance0Adjusted = balance0.sub(amount0Out).mul(1000).add(amount0In.mul(3));
    uint256 balance1Adjusted = balance1.sub(amount1Out).mul(1000).add(amount1In.mul(3));

    require(balance0Adjusted.mul(balance1Adjusted) >= uint256(balance0).mul(balance1).mul(1000**2), 
           'UniswapV2: K');

    // 转出代币给用户
    if (amount0Out > 0) _safeTransfer(token0, to, amount0Out);
    if (amount1Out > 0) _safeTransfer(token1, to, amount1Out);

    // 如果有回调数据，说明可能是闪电贷
    if (data.length > 0) IUniswapV2Callee(to).uniswapV2Call(msg.sender, amount0Out, amount1Out, data);

    // 更新储备
    balance0 = IERC20(token0).balanceOf(address(this));
    balance1 = IERC20(token1).balanceOf(address(this));
    _update(balance0, balance1, reserve0, reserve1);
}
```

> ⚠️ 上面是简化逻辑，实际代码会先判断 `amount0In` / `amount1In`（通过余额变化推断）

重点：

-   **恒定乘积检查**：确保 `x' * y' >= x * y`（含 0.3% 手续费）
    
-   **闪电贷支持**：如果 `data.length > 0`，调用接收方的 `uniswapV2Call`
    
-   **安全转账**：使用 `_safeTransfer`（内部用 `call` + 返回值检查）
    

* * *

### `_update` 函数（维护价格累积）

```solidity
function _update(
    uint256 balance0,
    uint256 balance1,
    uint256 _reserve0,
    uint256 _reserve1
) private {
    require(balance0 <= uint112.max && balance1 <= uint112.max, 'UniswapV2: OVERFLOW');
    uint32 timeElapsed = uint32(block.timestamp) - blockTimestampLast;
    if (timeElapsed > 0 && _reserve0 != 0 && _reserve1 != 0) {
        // 累积价格：price0 = reserve1 / reserve0
        price0CumulativeLast += uint256(UQ112x112.encode(_reserve1).uqdiv(_reserve0)) * timeElapsed;
        price1CumulativeLast += uint256(UQ112x112.encode(_reserve0).uqdiv(_reserve1)) * timeElapsed;
    }
    reserve0 = uint112(balance0);
    reserve1 = uint112(balance1);
    blockTimestampLast = uint32(block.timestamp);
}
```

-   使用 **UQ112x112 定点数** 高精度计算价格
    
-   用于后续计算 **时间加权平均价格（TWAP）**
    

* * *

## 五、如何与 Uniswap V2 交互（开发示例）

### 添加流动性（通过 Router）

```solidity
// 用户先 approve Router
IERC20(tokenA).approve(address(router), amountA);
IERC20(tokenB).approve(address(router), amountB);

// 调用 Router
router.addLiquidity(
    tokenA,
    tokenB,
    amountA,
    amountB,
    minAmountA, // 滑点保护
    minAmountB,
    msg.sender,
    block.timestamp + 600
);
```

### 交易（Swap）

```solidity
// 用 tokenA 换 tokenB
path = [tokenA, tokenB];
router.swapExactTokensForTokens(
    amountIn,
    amountOutMin,
    path,
    to,
    deadline
);
```

* * *

## 六、延伸学习建议

1.  **阅读官方代码**：
    
    -   [v2-core](https://github.com/Uniswap/v2-core)（Pair + Factory）
        
    -   [v2-periphery](https://github.com/Uniswap/v2-periphery)（Router）
        
2.  **动手实验**：
    
    -   在本地 Hardhat + Foundry 部署 V2
        
    -   模拟添加流动性、swap、闪电贷
        
3.  **安全审计视角**：
    
    -   为什么 V2 不怕重入？（先转账后代币回调）
        
    -   如何防止闪电贷滥用？
        
4.  **进阶：对比 V3**
    
    -   V3 引入集中流动性、tick、多个费率等级
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->





# DAY9

古法笔记：

![IMG_1431.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/CHI-WON/images/2026-01-20-1768908389604-IMG_1431.png)![IMG_1432.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/CHI-WON/images/2026-01-20-1768908457424-IMG_1432.png)![IMG_1433.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/CHI-WON/images/2026-01-20-1768908481031-IMG_1433.png)
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->






# DAY8

\[\]frontend

\[\]zk
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->







# DAY7

看完了ERC-7962分享会，我发现自己对底层的加密原理和签名技术还是一知半解，于是又回去补习了一下关于非对称加密和签名的知识，以及现在广泛使用的ERC-20和ERC-721协议。

## 非对称加密：

引用wiki百科**：公开密钥密码学**（英语：**Public-key cryptography**）也称**非对称式密码学**（英语：**Asymmetric cryptography**）是[密码学](https://zh.wikipedia.org/wiki/%E5%AF%86%E7%A2%BC%E5%AD%B8)的一种[算法](https://zh.wikipedia.org/wiki/%E6%BC%94%E7%AE%97%E6%B3%95)，它需要两个[密钥](https://zh.wikipedia.org/wiki/%E5%AF%86%E9%92%A5)，一个是公开密钥，另一个是私有密钥；公钥用作加密，私钥则用作解密。使用公钥把[明文](https://zh.wikipedia.org/wiki/%E6%98%8E%E6%96%87)加密后所得的[密文](https://zh.wikipedia.org/wiki/%E5%AF%86%E6%96%87)，只能用相对应的[私钥](https://zh.wikipedia.org/wiki/%E7%A7%81%E9%92%A5)才能解密并得到原本的明文，最初用来加密的公钥不能用作解密。由于加密和解密需要两个不同的密钥，故被称为非对称加密；不同于加密和解密都使用同一个密钥的[对称加密](https://zh.wikipedia.org/wiki/%E5%AF%B9%E7%A7%B0%E5%8A%A0%E5%AF%86)。公钥可以公开，可任意向外发布；私钥不可以公开，必须由用户自行严格秘密保管，绝不透过任何途径向任何人提供，也不会透露给被信任的要通信的另一方。

基于公开密钥加密的特性，它还能提供[数字签名](https://zh.wikipedia.org/wiki/%E6%95%B8%E4%BD%8D%E7%B0%BD%E7%AB%A0)的功能，使电子文件可以得到如同在纸本文件上亲笔签署的效果。

## 数字签名：

如果某一用户使用他的私钥加密明文，任何人都可以用该用户的公钥解密密文；由于私钥只由该用户自己持有，故可以肯定该文件必定出自于该用户；公众可以验证该用户发布的数据或文件是否完整、中途有否曾被篡改，接收者可信赖这些数据、文件确实来自于该用户，这被称作[数字签名](https://zh.wikipedia.org/wiki/%E6%95%B8%E4%BD%8D%E7%B0%BD%E7%AB%A0)，大部分国家已经立法承认数字签名拥有等同传统亲笔[签名](https://zh.wikipedia.org/wiki/%E7%AD%BE%E5%90%8D)的法律效力。公钥可以透过[数字证书认证机构](https://zh.wikipedia.org/wiki/%E6%95%B0%E5%AD%97%E8%AF%81%E4%B9%A6%E8%AE%A4%E8%AF%81%E6%9C%BA%E6%9E%84)签授的[电子证书](https://zh.wikipedia.org/wiki/%E9%9B%BB%E5%AD%90%E8%AD%89%E6%9B%B8)形式公布，接收者透过[信任链](https://zh.wikipedia.org/wiki/%E4%BF%A1%E4%BB%BB%E9%8F%88)形成一套完整的[公开密钥基础建设](https://zh.wikipedia.org/wiki/%E5%85%AC%E9%96%8B%E9%87%91%E9%91%B0%E5%9F%BA%E7%A4%8E%E5%BB%BA%E8%A8%AD)。例如，从网上下载的安装程序，大部分都带有程序制作者的数字签名，可以证明该程序的确是该作者（公司）发布的而不是第三方伪造的且未被篡改过（身份认证/验证）。而现时的[网上银行](https://zh.wikipedia.org/wiki/%E7%BD%91%E4%B8%8A%E9%93%B6%E8%A1%8C)或[购物网站](https://zh.wikipedia.org/wiki/%E8%B3%BC%E7%89%A9%E7%B6%B2%E7%AB%99)都会使用[HTTPS](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%AE%89%E5%85%A8%E5%8D%8F%E8%AE%AE)，避免沟通过程中的信息泄露。

## ERC-20:

ERC-20 是以太坊区块链上的一种技术标准，用于定义代币如何运行。ERC 代表“Ethereum Request for Comments”，而 20 是提案的编号。作为一种通用协议，ERC-20 允许开发者轻松创建和发行兼容的代币。ERC-20 的设计初衷是解决代币开发和交易中的碎片化问题，通过制定标准化接口，任何符合 ERC-20 规则的代币都可以在以太坊网络中无缝交互。例如，许多知名的加密货币，如 USDT 和 DAI，都基于 ERC-20 标准。

### 工作原理：

ERC-20 通过定义一套标准化接口，确保代币能够在以太坊网络中的钱包、交易所和去中心化应用（DApps）之间无缝交互。这些接口包括总供应量（totalSupply），用于定义代币的最大供应数量；余额查询（balanceOf），允许用户查看某个地址持有的代币数量；转账功能（transfer），用于在地址之间直接转移代币；以及授权与批准机制（approve/allowance），使用户可以授权第三方使用其代币，例如在去中心化交易所进行交易。通过这些规则，ERC-20 标准为开发者和用户提供了一种简洁、高效的方式来创建和管理代币，使其能够轻松融入以太坊生态系统。

## ERC-721:

### NFT:

非同质化代币（NFT）让数字资产能够在公共区块链上被拥有、交易和组合。而 ERC-721 标准正是这场变革的核心——它定义了如何在钱包、交易市场和应用程序中创建、转移和查找独一无二的代币。

### ERC-721:

ERC-721 是以太坊上一种用于非同质化代币的接口标准。这类代币的独特性体现在其 ID，与可 1:1 兑换的同质化代币（如 ERC-20）不同，NFT 无法互相替代。该标准规定了所有符合规范的合约必须实现的核心函数和事件，从而确保钱包和交易市场能够提供一致的支持。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->









# DAY6

\[\] 前端

\[\]Dapp

## 什么是前端

**前端就是用户看到和直接操作的那一层。**  
网页、按钮、输入框、列表、弹窗，这些都属于前端。它负责把“程序能做什么”变成“人能点、能看、能用的东西”。

一句话：**前端 = 人和系统之间的操作界面。**

## 在一个 DApp 里，前端的作用

用最直白的话说，DApp 的前端主要干这几件事：

1.  **展示信息**  
    把链上的数据（余额、NFT、交易记录、价格等）显示给用户看。
    
2.  **收集用户操作**  
    让用户点按钮、填数字、选资产，比如“转账”“兑换”“铸造”。
    
3.  **连接钱包**  
    引导用户连接钱包，获取地址，发起签名或交易。
    
4.  **调用智能合约**  
    把用户的操作转成对合约的调用请求，并把结果返回给用户。
    
5.  **反馈状态**  
    告诉用户：
    
    -   钱包是否已连接
        
    -   交易是否已发送
        
6.  是否成功 / 失败 / 等待确认
    

## wagmi

**wagmi** 是一个面向前端的 **React Hooks 库**，专门用来和以太坊生态交互。它把「连接钱包、读写合约、监听链上事件」这些原本又碎又烦的事情，抽象成一组**可组合、可声明式使用的 hooks**。

### React hooks

**React Hooks** 是 React 在 **16.8** 引入的一套机制，用来**在函数组件中使用状态和生命周期能力**。在 Hooks 出现之前，这些能力只属于 class 组件。

### React

**React** 是一个用于构建用户界面的前端 JavaScript 库，核心目标只有一件事：**用可预测的方式，把数据变成界面**。它由 Facebook（现 Meta）提出并维护，如今是 Web 前端的基础设施之一

### 什么是 viem

viem 是一个用来和区块链打交道的前端/后端 JavaScript 库。\*\*它帮你用代码去读链上数据、发交易、调智能合约。

## rainbowkit

**RainbowKit** 是一个开源的 **React 库（JavaScript UI 工具包）**，用于在基于 Web3 的前端应用（dApp）中快速、安全且可定制地集成用户的钱包连接功能。换句话说，它帮助开发者在网页上让用户连接他们的加密钱包（比如 MetaMask、WalletConnect、Coinbase Wallet 等）并进行交互，而不需要自己从头实现这一切界面与逻辑。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->












# DAY5

## 学习内容：

# Uniswap-Learning

## How Uniswap Works

### Definition

Uniswp V2是一个“不用订单簿、只靠资金池定价”的自动兑换系统。

用户不是和“另一个人”交易，而是直接和一个合约里的资金池交易。

​

### Role

1.  资金池（Pair合约）
    
2.  每一个交易对都有一个合约，例：ETH/USDC
    
3.  池子里同时存着两种货币
    
4.  池子由普通用户（流动性提供者）提供资金
    
5.  交易者（Trader）
    
6.  想用A换B的人
    
7.  把A打进池子
    
8.  从池子里拿走B
    
9.  流动性提供者（Liquid Provider）
    
10.  往池子里同时存入两种代币
     
11.  赚取交易手续费（0.3%）
     

### Why Price comes

Uniswap没有人报价。价格由一个关键的公式决定：

x \* y = k

-   x: 池子里Token A的数量
    
-   y: Token B的数量
    
-   k: 一个常数（交易前后必须保持不变）
    

这叫做**恒定乘积做市模型（Constant Product AMM）.**

**​**

### **Where liquidity comes**

​

LP提供流动性：

-   必须按当前池子的比例
    
-   同时存入两种Token
    
-   合约会分配LP Token作为凭证
    

​

LP Token:

LP Token代表Provider占整个池子的份额，将来可以用它取回本金和取回手续费收入

​

Gas：每笔交易收 **0.3%**

-   **不会给 Uniswap 官方**
    
-   **全部进入池子**
    
-   按 LP Token 占比分给流动性提供者
    

​

​
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->














# DAY4

对foundry有了一个基本的认识，Foundry不是一个工具而是一套工具链，包括了forge, cast, anvil, chisel。Foundry通过rust语言编写，实现了一个非常快的EVM，测试、脚本和部署不需要再像Hardhat那样繁琐，一切都可以在Solidity语言中开发编写。Foundry中最重要的、最灵魂的就是Cheatcodes.

## Cheatcodes

**Cheatcodes 是 Foundry 在测试环境中，给 EVM 插上的“调试接口（debug interface）”，**它们 **不是 Solidity 语法，**也**不是 EVM opcode，**而是 **forge 在执行测试时，拦截特定合约调用并执行宿主逻辑。**

拦截特定合约调用并执行宿主逻辑：

在一个没有Foundry的世界里，EVM的规则是：

-   Solidity 编译成字节码
    
-   EVM 执行 `CALL`
    
-   跳到目标地址
    
-   执行目标合约的字节码
    
-   返回结果
    

所有的逻辑都发生在EVM内部。而在Foundry中存在vm。vm是一个固定地址的合约接口，这个地址是 Foundry 预留的（常量地址），它的 ABI 是已知的。

在forge执行测试时，执行引擎不是geth，也不睡真实的节点，而是Foundry自己实现的EVM，这个EVM在执行call指令时，会检查目的地址是不是cheatcodes地址，calldata是否匹配某个已知cheatcodes，如果不是，就正常进入合约字节码执行，如果是，就不进入EVM合约执行，直接在宿主程序rust中执行对应逻辑，这就是拦截特定合约应用。

宿主逻辑：

宿主指的是：

-   运行 forge 的进程
    
-   用 Rust 写的测试运行器
    
-   在 EVM 之外
    

Foundry和Hardhat在不同的阶段用各自的差异：

-   Foundry：偏地层、偏协议和合约工程，有极强的测试能力
    
-   Hardhat：偏应用、偏前端协作、插件生态成熟
    

除了对Foundry有一个基本的了解之外，在AI及其基础概念的分享会后，对AI Agent的基础也做了一些简单的了解。

## What is AI Agent

An AI agent is a digital worker that can understand instructions and take actions in order to complete tasks.

## Five AI Agent Components

1.  LLM
    
2.  Prompting
    
3.  Memory
    
4.  External knowledge
    
5.  Tools
    

## What is API

Definition of API: Application Programming Interface

## Two Types Of Request

1.  Get request
    
2.  Post request
    

## Two Types Of Agent:

1.  Conversational agent
    
2.  automated agent
    

## Real World Examples

1.  personal assistant
    
2.  copilots
    
3.  lead generation and appointment setting agents
    
4.  research agent
    

[https://youtu.be/w0H1-b044KY?si=3I8lRFocdl8Y5WKF](https://youtu.be/w0H1-b044KY?si=3I8lRFocdl8Y5WKF)
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->















# **DAY3**

## TASK:

回顾Solidity基础语法

## 完成情况：

1.  **短路原则**：回顾了一下Bool型的运算符，主要关注了**短路原则**。当逻辑与（&&）的第一个条件为false时，就不会再去判断第二个条件；当逻辑或（｜｜）的第一个条件为false时，就不会再去判断第二个条件。
    
2.  **数据位置和gas：**回顾了Solidity数据存储的三种类型：storage，memory和calldata。不同存储位置的gas成本不同。
    

-   storage:合约里的状态变量默认都是storage，存储在链上，消耗的gas多
    
-   memory：函数里的参数和临时变量一般用，存储在内存中，不上链。尤其是如果返回数据类型是变长的情况下，必须加memory修饰。如：string ,bytes,array和自定义结构
    
-   calldata：和memory类似，存储在内存，不上链。不同在于calldata变量不能修改。
    
-   整体gas消耗： storage >memory >calldata
    

3.  **数据位置和赋值规则：**在不同的存储类型相互赋值时，有时会产生独立的副本（修改新的变量不会影响原变量），有时会产生引用（修改新变量会影响原变量）
    

赋值本质上是创建引用指向本体，因此修改本体或是引用，变化会被同步。eg：合约里的storage赋值给本地（函数里的）

storage时，会创建引用，改变新变量会影响原变量。

-   👉 **storage ↔ storage**：产生「引用」
    
-   👉 **storage → memory**：产生「独立副本」
    
-   👉 **memory ↔ memory**：产生「独立副本」
    
-   👉 **calldata → memory**：产生「独立副本」
    
-   👉 **calldata → storage**：产生「拷贝写入」
    
-   🚫 **calldata 不能被引用修改**
    

4.  **全局变量：**全局范围工作的变量，都是预留关键字，在函数内可以不声明直接使用。
    

一些常用的全局变量：

-   `blockhash(uint blockNumber)`: (`bytes32`) 给定区块的哈希值 – 只适用于最近的256个区块, 不包含当前区块。
    
-   `block.coinbase`: (`address payable`) 当前区块矿工的地址
    
-   `block.gaslimit`: (`uint`) 当前区块的gaslimit
    
-   `block.number`: (`uint`) 当前区块的number
    
-   `block.timestamp`: (`uint`) 当前区块的时间戳，为unix纪元以来的秒
    
-   `gasleft()`: (`uint256`) 剩余 gas
    
-   `msg.data`: (`bytes calldata`) 完整call data
    
-   `msg.sender`: (`address payable`) 消息发送者 (当前 caller)
    
-   `msg.sig`: (`bytes4`) calldata的前四个字节 (function identifier)
    
-   `msg.value`: (`uint`) 当前交易发送的 `wei` 值
    
-   `block.blobbasefee`: (`uint`) 当前区块的blob基础费用。这是Cancun升级新增的全局变量。
    
-   `blobhash(uint index)`: (`bytes32`) 返回跟当前交易关联的第 `index` 个blob的版本化哈希（第一个字节为版本号，当前为`0x01`，后面接KZG承诺的SHA256哈希的最后31个字节）。若当前交易不包含blob，则返回空字节。这是Cancun升级新增的全局变量。
    

5.  **数组：**用来存储一组数据，分为固定和可变数组
    

-   固定长度数组：用T\[k\]的格式声，T是元素的类型，k是长度
    

```
uint[8] array1
btyes1[5] array2
address[100] arrays3
```

-   可变长度数组：声明时不指定长度，用T\[\]格式
    

```
uint[] array4;
bytes1[] array5;
address[] array6;
bytes array7;
```

6.  **Mapping**
    

```
mapping(uint => address)
```

主要回顾了一下mapping的规则：

-   keytype不能使用自定义的结构体，但是valuetype可以使用自定义的类型
    
-   映射的存储位置必须是`storage`
    
-   如果映射声明为`public`，那么Solidity会自动给你创建一个`getter`函数，可以通过`Key`来查询对应的`Value`
    

7.  **构造函数：**构造函数（`constructor`）是一种特殊的函数，每个合约可以定义一个，并在部署合约的时候自动运行一次。它可以用来初始化合约的一些参数
    
8.  **修饰器：**  
    修饰器（`modifier`）是`Solidity`特有的语法，类似于面向对象编程中的装饰器（`decorator`），声明函数拥有的特性，并减少代码冗余。它就像钢铁侠的智能盔甲，穿上它的函数会带有某些特定的行为。`modifier`的主要使用场景是运行函数前的检查，例如地址，变量，余额等。
    

```
// 定义modifier
modifier onlyOwner {
   require(msg.sender == owner); // 检查调用者是否为owner地址
   _; // 如果是的话，继续运行函数主体；否则报错并revert交易
}

function changeOwner(address _newOwner) external onlyOwner{
   owner = _newOwner; // 只有owner地址运行这个函数，并改变owner
}
```

9.  **事件：**`Solidity`中的事件（`event`）是`EVM`上日志的抽象，它具有两个特点：
    
    -   响应：应用程序（`ethers.js`）可以通过`RPC`接口订阅和监听这些事件，并在前端做响应。
        
    -   经济：事件是`EVM`上比较经济的存储数据的方式，每个大概消耗2,000 `gas`；相比之下，链上存储一个新变量至少需要20,000 `gas`。
        

**声明事件**

声明用event关键字开头，接事件名称，括号里时需要记录的变量类型和变量名

```
event Transfer(address indexed from, address indexed to, uint256 value);
```

最主要的是**EVM日志log:**evm用日志存储solidity事件，每条日志记录都包含主题`topics`和数据`data`两部分。主题长度不能超过4，第一个元素是事件的签名（哈希）。

10.  **继承：**继承是面向对象编程很重要的组成部分，可以显著减少重复代码。如果把合约看作是对象的话，`Solidity`也是面向对象的编程，也支持继承。
     
     ### **规则**
     
     -   `virtual`: 父合约中的函数，如果希望子合约重写，需要加上`virtual`关键字。
         
     -   `override`：子合约重写了父合约中的函数，需要加上`override`关键字。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

















# DAY2

## TASK:学习Hardhat3-Tutorial

### 什么是HardHat3

Hardhat 3 是一个面向以太坊开发者的智能合约开发环境，它帮助开发、编译、测试、调试和部署 Solidity 合约，并支持编写脚本与测试工程。它扩展了之前的 Hardhat 生态，引入更现代化的工具链和默认配置

### Setting up the environment

-   安装Node.js v22或更高版本
    
-   安装包管理器：npm pnpm或yarn
    
-   选择一个编译器并安装Hardhat插件
    

### Creating a new Hardhat project

1.  **create an empty directory and initialize a Hardhat project**
    

```
mkdir hardhat-example
cd hardhat-example
npx hardhat --init
```

2.  **verify**
    

```
npx hardhat --help
```

### Write and Compile a contract

```
pragma solidity ^0.8.28;

contract Counter {
  uint public x;

  function inc() public { x++; }
}
```

compile:

```
npx hardhat build
```

or

```
npx hardhat compile
```

### Test

1.  **Solidity Test**
    

写.t.sol的Solidity测试文件，在EVM内部直接运行逻辑

```
npx hardhat test
```

仅运行Solidity：

```
npx hardhat test solidity
```

2.  **Javascript/Typescript Test**
    

在Node.js运行环境中进行，更贴近客户端调用合约的模式

运行Typescript测试：

```
npx hardhat test nodejs
```

### Ignition

```
npx hardhat ignition deploy ignition/modules/Counter.ts
```
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



















# **DAY1**

## **今日任务：**

-   学习创建钱包、完成测试网转账
    
-   铸造第一个NFT
    

### 钱包知识：

1.  是钱包：
    

区块链上的钱包与传统钱包不同，区块链上的钱包是对持有资产的证明，而真正的资产永远存储在链上，钱包是管理私钥的工具。

2.  公钥->私钥->地址：
    

-   私钥是随机生成的，管理资产的所有权
    
-   公钥饰根据私钥推导出来的，用于签名验证
    
-   地址是公钥再哈希出来的，对外展示和收款用到
    

3.  助记词：
    

-   钱包一般是一组英文单词
    
-   是私钥的一种可读表达
    
-   可以用来恢复钱包
    

4.  钱包的功能：
    

-   生成私钥和保管私钥
    
-   对交易进行签名
    
-   把签名后的交易上传的区块链网络
    

### 钱包的类型：

1.  按私钥的控制权划分：
    

-   托管钱包：交易所的钱包属于托管钱包，私钥由第三方的交易所保管，使用的门槛较低，忘记密码时可以找回，但实际的资产控制权在交易所手上。
    
-   非托管钱包：私钥由个人保管，自己对资产有绝对的控制，但一旦丢失私钥就丢失了所有资产
    

2.  按存在形式划分：
    

-   软件钱包：运行在手机和电脑上的钱包统称软件钱包
    
-   硬件钱包：私钥存储在离线的物理设备上
    

### 转账交易：

1.  一个钱包可以创建多个账户
    

2.  对链的选择：
    

-   钱包中可以显示在不同区块链上的资产，在进行转账时要选对对应的区块链
    
-   在练习是要使用测试网，防止转移了真实的资产
    

3.  转账的记录可以在区块链浏览器上查看
    

### NFT:

1.  什么是NFT
    

NFT的全称是Non-Fungible Token，即非同质化代币，本质是一种链上“唯一标识+所有权记录”。

-   非同质化：每个NFT都不完全等于另一个
    
-   链上确权：所有权、转移记录都记录在区块链上，无法随意篡改
    
-   元数据指向：NFT本身通常不存图片，而是指向一个资源，如IPFS、中心化服务器
    

2.  技术标准：
    

-   ERC-721:每个tokenid唯一
    
-   ERC-1155:半同质化，常用于游戏资产、门票
    

3.  NFT包含数据：
    

-   tokenid：唯一编号
    
-   owner：持有人
    
-   metadata:指向json文件
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
