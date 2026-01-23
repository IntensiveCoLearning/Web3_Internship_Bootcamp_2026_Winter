---
timezone: UTC+1
---

# yuhang

**GitHub ID:** yuhangf2333

**Telegram:** @yuhangf

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->
继续学习了简单的语法
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->

## 1\. 基础与变量 (Basics & Variables)

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract VariablesAndTypes {
    // --- 1. 状态变量 (State Variables) ---
    // 存储在链上 (Storage)，写入消耗 Gas
    uint256 public myUint = 123;
    bool public myBool = true;
    address public myAddress = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;
    
    // --- 2. 常量与不可变变量 (Gas Saving) ---
    // constant: 编译时确定，不占 Storage 插槽，极其省 Gas
    uint256 public constant MY_CONSTANT = 100;
    
    // immutable: 部署时确定 (在 constructor 中)，之后不可改，比状态变量省 Gas
    address public immutable i_owner;

    constructor() {
        i_owner = msg.sender; // 部署者地址
    }

    // --- 3. 局部变量 (Local Variables) ---
    function doSomething() public pure {
        // 仅在函数执行期间存在于内存中
        uint256 i = 456; 
        bool f = false;
        // 甚至更复杂的计算...
    }
}
```

## 2\. 复杂数据结构 (Data Structures)

枚举、结构体、数组和映射。

```
contract DataStructures {
    // --- 1. Enum (枚举) ---
    // 用于表示状态流程
    enum Status { Pending, Shipped, Completed }
    Status public currentStatus;

    // --- 2. Struct (结构体) ---
    // 自定义数据组合
    struct Todo {
        string text;
        bool completed;
    }
    // 结构体数组
    Todo[] public todos;

    // --- 3. Array (数组) ---
    uint[] public dynamicArray; // 动态长度
    uint[3] public fixedArray;  // 固定长度

    // --- 4. Mapping (映射) ---
    // Key => Value (最常用的存储方式)
    mapping(address => uint256) public balances;
    // 嵌套映射
    mapping(address => mapping(string => bool)) public userPermissions;

    function addData() public {
        // 修改 Enum
        currentStatus = Status.Shipped;

        // 添加到 Array
        dynamicArray.push(10);
        
        // 修改 Mapping
        balances[msg.sender] = 999;

        // 添加 Struct
        todos.push(Todo({text: "Learn Solidity", completed: false}));
    }
}
```

## 3\. 函数类型与数据存储位置 (Functions & Data Locations)

`storage` / `memory` / `calldata` 以及 `view` / `pure`。

```
contract FunctionsAndLocations {
    uint256 public x = 1; // 状态变量 (Storage)

    // --- 1. View vs Pure ---
    
    // View: 读取了状态变量 x，但没有修改它
    function addToX(uint256 y) public view returns (uint256) {
        return x + y;
    }

    // Pure: 既不读取也不修改状态变量，纯计算
    function addPure(uint256 i, uint256 j) public pure returns (uint256) {
        return i + j;
    }

    // --- 2. Data Locations (引用类型必须指定) ---
    
    // 函数参数推荐用 calldata (如果不需要修改)，比 memory 省 Gas
    function splitData(uint[] calldata _input) external {
        
        // Memory: 临时变量，可以修改，函数结束后销毁
        uint[] memory tempArray = new uint[](_input.length);
        tempArray[0] = _input[0];
        
        // Storage: 引用状态变量
        // 注意：这里无法直接在函数内定义 storage 变量指向新的空间
        // 通常用于引用已存在的 Struct/Array 进行修改
    }
}
```
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->


**1\. 核心概念**

• **什么是质押 (Staking)**：将资金（ETH 或代币）锁定在一个智能合约中，就像把钱放入一台自动售货机。这是一种“去信任化”（Trustless）的机制，用户不需要信任某个人，只需要信任代码规则,。

• **第三方托管 (Escrow)**：智能合约充当中间层。如果各方互不信任，合约可以作为锁仓机制，只有满足特定条件（如时间、金额）时资金才会移动。

**2\. 应用逻辑：状态机 (State Machine Logic)**

这个挑战的核心是构建一个具有不同状态的系统，根据**截止时间**和**资金阈值**进行状态切换。

• **存款阶段 (Deposit Phase)**：用户在截止时间前向合约质押 ETH。合约需要追踪个人余额（Mapping）和总余额。

• **执行逻辑 (Execution)**：

    ◦ **成功条件**：如果在截止时间前达到了目标金额（Threshold），调用 `execute()` 将资金发送到外部合约,。

    ◦ **失败条件**：如果超时且未达到目标金额，进入“撤资状态”，允许用户取回（Withdraw）自己的资金,。

• **非自动执行**：区块链上的状态变化不会自动发生（没有自动 Cron Job）。必须由外部账户（用户或脚本）发起交易来调用 `execute` 函数以触发状态变更,。为了确保执行，通常会设置激励机制或开源脚本供他人运行。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->



这一周主要在划水，准备期末考试复习

等下一周考完后，就轻松不少了
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->




学了下智能合约的相关内容  
智能合约通常基于 **Solidity** 编写，其主要组成包括以下几类：

1\. 状态数据（State Variables）

-   用于保存合约的长期数据
    
-   数据直接存储在区块链上，具有持久性
    
-   共同决定合约在任意时刻的状态
    

2\. 函数体系（Functions）

函数定义了合约的行为逻辑，按照用途和可见性可划分为：

-   **构造函数（Constructor）**  
    在合约部署时执行，用于初始化关键参数
    
-   **外部可调用函数**
    
    -   `public`：外部账户和其他合约均可调用
        
    -   `external`（若使用）：主要供外部调用，节省 gas
        
-   **内部逻辑函数**
    
    -   `internal`：当前合约及其继承合约可访问
        
    -   `private`：仅当前合约内部使用
        
-   **只读函数**
    
    -   `view`：可读取状态变量，但不修改链上状态
        
    -   `pure`：既不读取也不修改任何状态，纯计算逻辑
        

3\. 事件机制（Events）

-   用于在链上记录关键行为
    
-   不参与状态存储，但会写入日志（log）
    
-   方便前端应用监听，也便于索引器抓取数据
    
-   例如：NFT 转移时触发的 `Transfer` 事件
    

4\. 修饰符（Modifiers）

-   用于在函数执行前统一进行条件校验
    
-   常见用途包括：
    
    -   权限控制（如 onlyOwner）
        
    -   安全防护（如防止重入攻击）
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->





听了律师的讲解，了解了目前国内的一些情况
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->






## Day2

今天了解了 Web3 的一些基础概念

-   **去中心化的代价与优势** 比特币耗能，是 POW（算力竞争），而以太坊用的 POS 是靠质押。虽然机制不同，但核心都是为了达成共识，防止有人作恶。
    
-   **不可篡改** 交易广播出去后，被打包成区块，再通过哈希值连成链。虽然出块很快，但要做到真正的“不可篡改”，还是需要等待网络确认的，不是即时的。
    
-   **智能合约** 这东西本质就是链上的代码。任何人都能生成钱包、发起交互，不需要经过类似银行这种中介机构的批准。
    
-   **安全底线** 学到最后发现，入圈第一课其实是保管私钥。助记词衍生私钥，私钥生成地址。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->







Day1

打卡，阅读了学习以太坊第一章节，然后在b站上找到了区块链的公开课，再用视频理解了一下以太坊的相关内容。

参加了20:00的行业介绍，还是想从技术方面入手。

继续加油💪
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
