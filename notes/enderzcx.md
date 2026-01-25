---
timezone: UTC+8
---

# enderzcx

**GitHub ID:** enderzcx

**Telegram:** @enderzcx

## Self-introduction

努力将https://solidity-by-example.org/简体中文版本优化成纯小白能看得懂的知识！并且将没有翻译的部分也补全

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
# **Fork了Solidity-by-example 中文翻译**

后续会在这个学习仓库更新[https://github.com/enderzcx/Solidity-by-example\_Chinese-by-enderzcx/tree/main](https://github.com/enderzcx/Solidity-by-example_Chinese-by-enderzcx/tree/main)

**优化个人简历 & 职业规划**
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->


今日复习了Python 准备投测试实习生的简历
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->



# [**循环语句**](https://github.com/Web3-Club/Solidity-by-example_Chinese/blob/main/11%20%E5%BE%AA%E7%8E%AF%E8%AF%AD%E5%8F%A5.md#%E5%BE%AA%E7%8E%AF%E8%AF%AD%E5%8F%A5)

**这里直接复制代码编译会warning，将pure添加进函数状态栏即可（英文版是有pure的）**

**加入了**for和while语句的标准语法和执行顺序；加入了一些的注释。

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Loop {
    function loop() public pure{
        // for 循环
        /* for 语句的标准语法
          for (初始化; 条件; 每次循环后执行) {循环体}*/
        for (uint i = 0; i < 10; i++) {
            if (i == 3) {
                // 使用 continue 跳过当前迭代
                // 意思就是立即结束当前循环迭代，进入下一次迭代。
                continue;
            }
            if (i == 5) {
                // 使用 break 退出循环
                // 意思是立即终止整个循环，跳转到循环后面的代码。
                break;
            }
        }

        // while 循环
        /* 基本语法：
        // while (条件) {
        // 循环体
        // 更新条件（避免死循环）
        }*/   
        uint j; // 默认j = 0
        while (j < 10) {
            j++; // 更新条件 // 等价于 j = j + 1和 j += 1
        /*执行流程：

        检查 j < 10（0 < 10 → true）→ 进入循环体。
        执行 j++ → j 变为 1。
        重新检查 1 < 10 → true → 继续循环。
        重复直到 j = 10 时，10 < 10 为 false → 循环结束。*/ 
        }
    }
}
/* 执行顺序:
   for (uint i = 0; i < 3; i++) {
    // body
}
执行顺序是：
1️⃣ i = 0
2️⃣ 判断 i < 3 → true
3️⃣ 执行 body
4️⃣ 执行 i++
5️⃣ 判断 i < 3 → true
6️⃣ 执行 body
7️⃣ i++
8️⃣ 判断 i < 3 → false → 退出循环
*/
```

为什么不加pure就会waring？这里需要解释一下状态变量和参数的区别

1.  **状态变量 (**`number`**)**
    
    -   存储在\*\*合约存储（Storage）\*\*中，持久化保存。
        
    -   修改状态变量会消耗 **Gas** 并改变区块链状态。
        
    -   例子：`uint256 public number = 5;`（永久存储在合约中）
        
2.  **函数参数 (**`_number`**)**
    
    -   临时传入函数的值，存储在\*\*内存（Memory）\*\*中。
        
    -   不改变合约状态，仅在函数执行时存在。
        
    -   例子：`function addPure(uint256 _number)` 中的 `_number`
        

`pure`：只能处理**参数/局部变量**，不能碰状态变量

诶？这段代码确实没有碰状态变量啊，这是 Solidity 编译器在**教你写专业合约**：

> “我发现你这个函数很纯，你不标 `pure` 有点不严谨，我提醒你一下。”

# [**映射**](https://github.com/Web3-Club/Solidity-by-example_Chinese/blob/main/12%E6%98%A0%E5%B0%84.md#%E6%98%A0%E5%B0%84)

直接copy代码会报错，删除了一些乱码，加入了尽可能详细的注释。

当有两个合约的时候，记得点击这里切换合约，以免找不到你需要的函数。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-23-1769181512905-image.png)

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Mapping {
    // 地址到整数的映射
    mapping(address => uint) public myMap;
    // 基本语法：mapping(KeyType => ValueType) [visibility] mappingName;
    // 映射可以当成Python里的字典  这个映射的意思就是 映射名：myMap，只能接受地址类型的数据输入，输返回uint类型的数据
    // 在这里所有键默认返回  0（对 uint类型来说），输入的所有地址都会返回0
    // 映射数据永久存储在区块链上（storage）且不能用于 memory 或函数参数/返回值。，修改需消耗 Gas。
    // 映射的 Gas 成本（读取 ~200 Gas，写入 ~5000–20000 Gas）。
    /* 映射的使用
        // 赋值
        myMap[msg.sender] = 100;  // 将调用者地址的映射值设为 100

        // 查询
        uint balance = myMap[address(0x123)];  // 返回 0（如果未赋值）或之前存储的值

        // 修改
        myMap[msg.sender] += 50;  // 增加 50（需先检查是否足够，避免溢出）
    */
    function get(address _addr) public view returns (uint) {
        // 映射始终返回一个值。
        // 如果该值从未设置过，它将返回默认值。
        return myMap[_addr];
    }

    function set(address _addr, uint _i) public {
        // 更新此地址的值
        myMap[_addr] = _i;
    }

    function remove(address _addr) public {
        // 将该值重置为默认值。
        delete myMap[_addr];
    }
    }

    contract NestedMapping {
    // 嵌套映射（从地址到另一个映射）
    mapping(address => mapping(uint => bool)) public nested;
    // 嵌套映射最终返回的值类型为最内层(就是最右边)映射的valueType。
    // 该嵌套映射的赋值方法为nested[地址][无符号整数] = Ture/False
    
    function get(address _addr1, uint _i) public view returns (bool) {
        // 即使未初始化，也可以从嵌套映射中获取值
        return nested[_addr1][_i];
    }

    function set(address _addr1, uint _i, bool _boo) public {
        nested[_addr1][_i] = _boo; // 外部输入布尔型数据时，0 代表 False，1代表 Ture, 不要直接输入 Ture 或 False
    }

    function remove(address _addr1, uint _i) public {
        delete nested[_addr1][_i];
    }
}
```
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->





# **查看 Uniswap 工作原理解析 回放**

[Solidity by Example | 0.8.26](https://solidity-by-example.org/) Basic 部分**学习笔记：**

# [**第一个应用**](https://github.com/Web3-Club/Solidity-by-example_Chinese/blob/main/02%20%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%BA%94%E7%94%A8.md#%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%BA%94%E7%94%A8)

**在原有基础上加入了适合小白（比如我）的注释**

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Counter { // contract 是智能合约的起点，不可或缺
    uint public count; // 一个公共的 uint 类型变量 

    /* 变量用法：
    变量类型 可见性 变量名字 = 变量值
    变量类型：uint, string, int, bool, address 等
    可见性： public, private, internal, external。状态变量的默认可见性是 internal
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
    function 函数名(参数类型 参数名称, 参数类型2 参数名称2, ...) 可见性 函数状态 returns (返回值类型1, 返回值类型2, ...){
        return (返回值1, 返回值2, ...); // 多返回值时，必须用括号。
    // 或 return 单个返回值;          // 单返回值时，无需括号。
    // 如果函数不返回值，可省略 returns
    }

    tips：最简函数用法：function 函数名() 可见性 {}
    // 空括号 () 表示该函数不接受任何参数。   
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

重点解释一下参数：参数是函数在执行时**从外部接收的数据（我将在If/Else板块举例说明）**。

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

## [If / Else](https://solidity-by-example.org/if-else/)

该部分在简体中文版本缺失，以下为我手动翻译（AI）补充：

**Solidity 支持条件语句** `if`**、**`else if` **和** `else`**。**

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract IfElse {
    // 函数 foo：根据 x 的值返回不同结果
    function foo(uint256 x) public pure returns (uint256) {
        // 这里引入了外部参数 x ，并且规定了他的参数类型：uint256，此时 x 还没有具体数值。
        // 你可以理解为一句话：foo 这个函数需要别人调用时，给我一个 uint256 类型的数，我把它叫做 x
        if (x < 10) {          // 如果 x 小于 10 
            return 0;         // 返回 0
        } else if (x < 20) {   // 否则，如果 x 小于 20
            return 1;         // 返回 1
        } else {              // 其他情况
            return 2;         // 返回 2
        }
    }

    // 函数 ternary：示范三元运算符（if/else 的简写）
    function ternary(uint256 _x) public pure returns (uint256) {
        // 传统 if/else 写法：
        // if (_x < 10) {
        //     return 1;
        // }
        // return 2;

        // 三元运算符简写：
        // 语法：条件 ? 表达式1（true 时返回） : 表达式2（false 时返回）
        return _x < 10 ? 1 : 2;
    }
}
```

这是我们遇到第一个有参数的函数，现在介绍一下参数

**参数（parameter）**：  
是**函数定义时**写在括号里的变量，用来**接收调用函数时传进来的值**。

**实参（argument）**：  
是**你调用函数时实际输入的那个具体值**（比如 666）

点击橙色按钮

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-22-1769091319887-image.png)

部署合约后，点击出现的合约地址

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-22-1769091487400-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-22-1769091528660-image.png)

会出现可输入数据的框，当你输入完数据，点击

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-22-1769091622898-image.png)

蓝色按钮，这时x = 666，经过了函数运算后结果为2（注意：x还是等于666，2 是函数返回的结果，不是 x 的新值）

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-22-1769091647772-image.png)

会出现一行字，0 表示**返回值的索引**，uint256 表示**返回值的类型**，2 是函数根据**传入的参数 x（实参 666）计算后 return 的结果**。
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
