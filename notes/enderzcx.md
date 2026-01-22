---
timezone: UTC+8
---

# enderzcx

**GitHub ID:** enderzcx

**Telegram:** @enderzcx

## Self-introduction

小白一枚，求带

## Notes

<!-- Content_START -->
# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->
# **查看 Uniswap 工作原理解析 回放**

[Solidity by Example | 0.8.26](https://solidity-by-example.org/) Basic 部分**学习笔记：**

**在原有基础上加入了适合小白（比如我）的注释**

# [**第一个应用**](https://github.com/Web3-Club/Solidity-by-example_Chinese/blob/main/02%20%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%BA%94%E7%94%A8.md#%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%BA%94%E7%94%A8)

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Counter { // contract 是智能合约的起点，不可或缺
    uint public count; // 一个公共的 uint 类型变量 

    /* 变量用法：
    变量类型 可见性 变量名字 = 变量值
    变量类型：uint, string, int, bool, address 等
    可见性： public, private, internal, external
    */

    // 函数获取当前计数
    function get() public view returns (uint) {
        return count;
    }
    /*
    function 是声明函数时的固定用法要编写函数，就需要以 function 关键字开头。(来自Solidity 101):
    function <function name>([parameter types[, ...]]) {internal|external|public|private} [pure|view|payable] [virtual|override] [<modifiers>]
[returns (<return types>)]{ <function body> } 方括号中的是可写可不写的关键字

    我自己总结的用法：
    function 函数名(变量类型，变量名称) 可见性 函数状态 returns (返回值的类型）{
        return 你需要将返回的值赋值到的变量名
    }

    tips：最简函数用法：function 函数名() 可见性 {}
       
    注意 1：合约中定义的函数需要明确指定可见性，它们没有默认值
    注意 2：public|private|internal 也可用于修饰状态变量(定义可参考WTF Solidity 第5讲的相关内容)。public变量会自动生成同名的getter函数，用于查询数值。未标明可见性类型的状态变量，默认为internal。
    */
    // 函数将计数增加 1
    function inc() public {
        count += 1;
    }

    // 函数将计数减少 1
    function dec() public {
        // 如果计数为 0，则此函数将失败
        count -= 1;
    }
}
```

# [**基本数据类型**](https://github.com/Web3-Club/Solidity-by-example_Chinese/blob/main/03%20%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B.md#%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)

代码好像出了点错，我搬运了一下

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Primitives {
    bool public boo = true;

    /*
    uint 代表无符号整型，即非负整数
    不同大小的 uint 可用
        uint8 范围从 0 到 2 ** 8 - 1
        uint16 范围从 0 到 2 ** 16 - 1
        ...
        uint256 范围从 0 到 2 ** 256 - 1
    */
    uint8 public u8 = 1;
    uint256 public u256 = 456;
    uint256 public u = 123; // uint 是 uint256 的别名

    /*
    int 类型允许负数。
    类似于 uint，不同范围的 int 可用，从 int8 到 int256
    
    int256 范围从 -2 ** 255 到 2 ** 255 - 1 
    int128 范围从 -2 ** 127 到 2 ** 127 - 1
    */
    int8 public i8 = -1;
    int256 public i256 = 456;
    int256 public i = -123; // int 和 int256 相同

    // int 的最小值和最大值
    int256 public minInt = type(int256).min;
    int256 public maxInt = type(int256).max;

    address public addr = 0xCA35b7d915458EF540aDe6068dFe2F44E8fa733c;

    /*
    在 Solidity 中，byte 数据类型表示一系列字节。 
    Solidity 有两种字节类型：

     - 固定大小的字节数组
     - 动态大小的字节数组。
     
     在 Solidity 中，字节数组的术语代表字节的动态数组
     这是 byte[] 的简写
    */
    bytes1 a = 0xb5; //  [10110101]
    bytes1 b = 0x56; //  [01010110]

    // 默认值
    // 未赋值的变量有一个默认值
    bool public defaultBoo; // false
    uint256 public defaultUint; // 0
    int256 public defaultInt; // 0
    address public defaultAddr; // 0x0000000000000000000000000000000000000000
}
```

# [**变量**](https://github.com/Web3-Club/Solidity-by-example_Chinese/blob/main/04%20%E5%8F%98%E9%87%8F.md)

这一章直接COPY代码编译会Warning（REMIX Windows桌面版）  
以下是我修改后不会报错的版本：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Variables {
    string public text = "Hello";
    uint public num = 123;

    function doSomething() public view returns (uint,uint, address) {
        uint i = 456;
        uint timestamp = block.timestamp;
        address sender = msg.sender;
        return (i,timestamp, sender);
    }
}
```

## **扩充**

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Variables {
    // Local存在函数内存中 调用的时候才有
    // state 在函数之外声明 存储在区块链上
    // blockchain 存在链上 需要消耗GAS 
    // global是默认的全局变量,提供有关区块链的信息
    string public text = "Hello";  //存在区块链上
    uint public num = 123;

    function doSomething() public view returns (uint,uint,address){

        uint i = 456;   // 内存变量 在调用这个函数的时候才会使用此变量
 

        uint times = block.timestamp;  // 当前区块的时间戳,全局变量
        address sender = msg.sender;   
        return (i,times,sender);
    }
}
```

# [**常量**](https://github.com/Web3-Club/Solidity-by-example_Chinese/blob/main/05%20%E5%B8%B8%E9%87%8F.md)

更正一下这里的解释方便小白（就是我）理解：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Constants {
// 编码惯例将常量变量大写
// "Constants" 是这个 "contract" 的名字
address public constant MY_ADDRESS = 0x777788889999AaAAbBbbCcccddDdeeeEfFFfCcCc;
uint public constant MY_UINT = 123;
// "constant" 才表示这个变量是一个常量 用法是：常量类型 可见性 constant 常量名字 = 常量值
// 需要与一般的变量用法区分开，普通变量用法：变量类型 可见性 变量名字 = 变量值
// "="和"变量值" 可以不写，默认变量值为0
}
```

# [**不可变**](https://github.com/Web3-Club/Solidity-by-example_Chinese/blob/main/06%20%E4%B8%8D%E5%8F%AF%E5%8F%98.md#%E6%89%A9%E5%85%85)

中的 [构造函数（constructor）](https://docs.soliditylang.org/zh/v0.8.17/contracts.html#constructor)404了

这里贴上AI的解释：

### **1.** `constructor` **是什么？**

-   **构造函数**：合约**部署时自动运行一次**的特殊函数，用于初始化状态变量。
    
-   **特点**：
    
    -   每个合约**只能有一个** `constructor`。
        
    -   不能被显式调用（仅在部署时触发）。
        
    -   通常用于设置**初始状态**（如所有者地址、参数等）。
        

* * *

### **2\. 参数** `uint _myUint`

-   部署合约时，**必须传入一个** `uint` **类型的参数**（如 `_myUint = 100`）。
    
-   示例部署调用（Remix IDE）：
    
    ```
    SOLIDITY
    ```
    
    ```solidity
    // 部署时输入：100（作为 _myUint 的值）
    ```
    

* * *

### **3\. 函数体逻辑**

1.  `MY_ADDRESS = msg.sender`
    
    -   将**部署者的地址**（`msg.sender`）永久存储在状态变量 `MY_ADDRESS` 中。
        
    -   例如：如果 `0x123...` 部署了合约，`MY_ADDRESS` 将永远是 `0x123...`。
        
2.  `MY_UINT = _myUint`
    
    -   将传入的 `_myUint` 值存储到状态变量 `MY_UINT`。
        
    -   例如：部署时传入 `100`，则 `MY_UINT = 100`。
        

* * *

### **4\. 完整示例**

假设合约定义如下：

```
SOLIDITY
```

```solidity
contract MyContract {
    address public MY_ADDRESS;  // 存储部署者地址
    uint public MY_UINT;       // 存储传入的 uint 值

    constructor(uint _myUint) {
        MY_ADDRESS = msg.sender;
        MY_UINT = _myUint;
    }
}
```

-   **部署后**：
    
    -   调用 `MY_ADDRESS()` 返回部署者地址。
        
    -   调用 `MY_UINT()` 返回部署时传入的 `_myUint` 值。
        

* * *

### **5\. 为什么用** `constructor`**？**

-   **安全性**：确保关键参数（如所有者地址）在部署时正确设置，且**不可篡改**。
    
-   **气体效率**：避免后续用 `setter` 函数额外消耗 gas。
    
-   **不可变性**：结合 `immutable` 关键字（如 `address public immutable OWNER;`），可以节省 gas。
    

# [**以太币和Wei**](https://github.com/Web3-Club/Solidity-by-example_Chinese/blob/main/08%20%E4%BB%A5%E5%A4%AA%E5%B8%81%E5%92%8CWei.md#%E4%BB%A5%E5%A4%AA%E5%B8%81%E5%92%8Cwei)

**可能代码翻译者没删除一些东西，优化了一下，我还加入了一些解释：**

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract EtherUnits {
uint public oneWei = 1 wei;
// 1 Wei 等于 1 (记住他)
bool public isOneWei = 1 wei == 1; // == 是相等比较运算符，用于判断两个值是否相等。它返回一个布尔值（true 或 false）。

uint public oneEther = 1 ether;
// 1 以太币等于10^18 Wei
bool public isOneEther = 1 ether == 1e18;
}
// wei 和 ether 是 Solidity 的内置单位：可以直接在代码中使用（如 1 wei、0.5 ether）

```

# [**Gas费**](https://github.com/Web3-Club/Solidity-by-example_Chinese/blob/main/09%20Gas%E8%B4%B9.md#gas%E8%B4%B9)

**加入了对while语句和其他解释：**

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Gas {
uint public i = 0;

  // 使用完你发送的所有 gas 会导致你的交易失败。
  // 状态更改被撤销。
  // 花费的 gas 不会被退还。
  function forever() public {
      // 在这里，我们运行一个循环直到花费所有的 gas
      // 交易失败
      while (true) { // while 循环：只要条件为 true，就重复执行代码块。
          i += 1; // 循环体
      } // 每次循环都会消耗 gas（约 20,000 gas/次，因 i += 1 的存储操作。用户调用此函数会损失所有支付的 gas（例如，设置 gasLimit=1,000,000，则全部消耗）。
  } // 状态回滚：交易失败后，所有状态更改撤销（i 不会保留修改后的值）。
}
```
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->













收看「Web3 公共物品资金分配第一节课」回放

我也想参与！我也想当坏人！
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->














**查看「Key Hash Based Tokens: 从 ERC-721 到 ERC-7962」回放**  
**后面学习完智能合约我要试试ERC7962在代码层面和一般的有什么不一样，从代码层面是如何实现的**  
Solidity学习路线（**021学习以太坊**）：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-20-1768886692829-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-20-1768886705747-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-20-1768897672853-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-20-1768897692149-image.png)
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->

















**体验 My first zk vote**  
**成功把一个智能合约部署在测试网上并且发送了一段信息 Remix有点难用 不知道是我的网络问题还是什么**  
[**3.2 审计标准流程**](https://web3intern.xyz/zh/smart-contract-development/#_3-2-%E5%AE%A1%E8%AE%A1%E6%A0%87%E5%87%86%E6%B5%81%E7%A8%8B)

Solidity 审计流程

1.  **静态分析（Static Analysis）**：使用工具如 Slither、Mythril 扫描代码缺陷
    
2.  **动态测试（Fuzzing/Property Testing）**：模拟攻击行为和极限条件
    
3.  **人工审查（Manual Review）**：由资深审计员检查业务逻辑漏洞
    
4.  **审计报告生成（Audit Report）**：明确发现的问题及修复建议
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->




















Web3 实习手册[「智能合约开发」](https://web3intern.xyz/zh/smart-contract-development/)部分  
学习中文排版规范：[https://github.com/sparanoid/chinese-copywriting-guidelines](https://github.com/sparanoid/chinese-copywriting-guidelines)  
学习AI 及其基础概念
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->






















已完成 [Unphishable](https://unphishable.io/) 钓鱼攻防挑战  
Trezor的域名到底是什么 试了很多都不对。。。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->























《021学习以太坊》: 比特币更像是“价值锚”， 以太坊更像是“应用发动机” 已学习到32页
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
























学习了Web3 实习手册[「安全与合规」](https://web3intern.xyz/zh/security/)部分  
开发环境并熟悉：  
[Remix IDE](https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.30+commit.73712a01.js)

[Remix 使用教程](https://www.notion.so/2bc1a400ef948075ab7ec8b8259c7746?pvs=21)  
同时开始在B站上学习智能合约编程[https://www.bilibili.com/video/BV1Ca411n7ta/?spm\_id\_from=333.1007.top\_right\_bar\_window\_default\_collection.content.click&vd\_source=6be132124d2a630c6fcea0ba05bfbbf9](https://www.bilibili.com/video/BV1Ca411n7ta/?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click&vd_source=6be132124d2a630c6fcea0ba05bfbbf9)
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

























这两天系统性学习了**区块链基础概念、以太坊概览、行业赛道全览、Web3 工作方式**，果然比我之前东学一点西看一些有效率有条理多了。  
**收纳了很多富有见解的笔记：**

| Web3 行业全局介绍 & 岗位概览How to DeFi: 入门篇 | Web3入门计划总结系列之 一 |
| --- | --- |
| DeFi 进阶指南 | Web3入门计划总结系列之 二 |
| Web3 运行原理 |   |
| Vibe Coding 方法论：不会编程的人如何用 AI 写出能跑的代码 |   |
| 写给00后：为什么我们不能错过 Web3 |   |

**一些工具和网站:**

| 官方文档 | 研究报告 | 中文资源： |
| --- | --- | --- |
| Ethereum.org - 以太坊官方文档 | Messari - 区块链项目深度分析 | ETHPanda - 以太坊中文社区 |
| Uniswap Docs - Uniswap 协议文档 | The Block - 行业数据和趋势 | LXDAO |
| OpenSea Learn - NFT 入门指南 | Delphi Digital - 加密市场研究 |   |

其他工具和网站

|   | 找工作 |
| --- | --- |
| 用AI设计网站：Stitch - Design with AI | DeJob |
|   | The Ethereum Season of Internships - 英文，以太坊实习季 |
<!-- DAILY_CHECKIN_2026-01-14_END -->
<!-- Content_END -->
