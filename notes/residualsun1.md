---
timezone: UTC+8
---

# Guozheng Huang

**GitHub ID:** residualsun1

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->
## **1\. 对** `pure` **和** `view` **的细碎理解**

今天在对 `pure` 和 `view` 两个状态可变性修饰符进行辨析时，进一步理解了智能约合中的结构——用我自己的话来概括，就是包括「对状态变量保持敏感」、「什么时候该用 `pure` 或 `view`」，以及「函数参数列表何时填入内容，填入什么」。

首先是一段基础代码。

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
​
contract FunctionTypes{
    // 状态变量
    uint256 public number = 5;
​
    // 普通函数
    function add() external{
        number = number + 1;
  }
​
}
```

在《[理解 Solidity 语法的基本结构](https://guozheng.rbind.io/project/learn-solidity-1/)》的博文里，我已经详细写过了区别，这里想简要复现部分内容以整理知识。

结论先上，对状态变量的敏感有助于我快速判断什么时候该用 `pure` 或 `view` 可变性修饰符，以下面的代码为例。

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
​
contract FunctionTypes{
    // 状态变量
    uint256 public number = 5;
    
    // 普通函数
    function add() external pure{
        number = number + 1;
    }
​
    function add() external view{
        number = number + 1;
    }
}
```

从代码开头便可以清楚地知道状态变量是 `number`，数据存储结构是 `uint256`，因而两个普通函数的写法显然都是错误的——函数体中都直接读取和修改了状态变量 `number`，但 `pure` 既无法读取也无法修改，`view` 则是只能读取但无法修改。

如果要正确改写，可以如下所示。

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
​
contract FunctionTypes{
    // 状态变量
    uint256 public number = 5;
​
    // 普通函数
    function addPure(uint256 _number) external pure returns(uint256 new_number1){
        new_number1 = _number + 1;
    }
  
    function addView() external view returns(uint256 new_number2) {
        new_number2 = number + 1;
    }
}
```

这里可以阐述两点，其一是 `pure` 和 `view` 的具体非读取与读取功能的表现，其二是我过去一直不理解的——**函数的参数列表中啥时候该填入内容，又该填入什么**？

第一，可以看到含 `pure` 的函数在函数体中是完全没有涉及状态变量 `number`，而是给函数 `addPure()` 传递了一个参数 `_number`，并规定好数据类型是 `uint256`，接着再在函数体中创造了一个局部变量 `new_number1`，并赋值为 `_number + 1`，最后返回。

这里我们可以理解，如果要在函数体中加入新的参数，不涉及状态变量，可以在函数的参数列表里声明加入的参数名称（如`_number`），并指定其类型（如 `uint256`）。

第二，可以看到含 `view` 的函数在函数体里涉及了状态变量 `number`，但是并没有改变 `number` 本身，而是读取了 `number`，在此基础上创建了局部变量 `new_number2`，并赋值为 `number + 1`，最后返回。

这部分没有在函数的参数列表中加入新的参数，而是直接读取状态变量。

## **2\. 函数输出**

### **2.1 函数返回值**

Solidity 中的函数输出相关的关键词是 `returns` 和 `return`。

`returns` 跟在函数名之后，用于声明返回的变量类型与变量名。

`return` 则是指定具体的变量值。

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
​
contract ReturnDemo{
    
    function returnDemo() public pure returns(uint256, bool, uint256[3] memory){
        return(1, true, [uint256(1),2,3] );
    }
}
```

在 Remix IDE 中编译和部署上述代码后，查看左下角的 `returnDemo`，会显示如下信息：

```
0:uint256: 1
1:bool: true
2:uint256[3]: 1,2,3
```

总结起来就是通过 `returns` 的关键字来声明有多个返回值以及返回值类型的 `returnDemo()` 函数，然后在函数主体再通过 `return()` 来确定具体的返回值。uint256\[3\] 声明了长度为 3 而且类型为 `uint256` 的数组作为返回值，而由于 `[1,2,3]` 会被默认为 `uint8(3)`，因此 `[uint256(1),2,3]` 中的首个元素必须强制转换为 `uint256` 来声明该数组内的元素都是这种类型。另外，数组类型返回值默认必须用 `memory` 来进行修饰。

### **2.2 命名式返回**

在函数返回值一节中已说到 `returns` 可以声明返回的变量名，如果这么做，Solidity 会初始化这些变量，自动返回变量值而无需再使用 `return`。上一节的做法是只声明了变量类型，这节我们就同时声明变量名和变量类型，这样在函数主体中为几个变量赋值就可以了。

```
function returnNamed() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array){
    _number = 1;
    _bool = true;
    _array = [uint256(1),2,3];
}
```

在 Remix IDE 中编译和部署后运行返回结果如下：

```
0:uint256: _number 1
1:bool: _bool true
2:uint256[3]: _array 1,2,3
```

但在命名式返回中仍然可以使用 `return`。

```
// 命名式返回，依然支持return
function returnNamed2() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array){
    return(1, true, [uint256(1),2,5]);
}
```

### **2.3 解构式赋值**

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
​
contract ReturnDemo{
​
    function returnNamed() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array){
    _number = 1;
    _bool = true;
    _array = [uint256(1),2,3];
}
​
    function readReturn() public pure{
        uint256 _number;
        bool _bool;
        uint256[3] memory _array;
        (_number, _bool, _array) = returnNamed();
​
        // 读取部分返回值
        (, _bool, ) = returnNamed();
​
    }
}
```

其实解构式没有看懂，但是不想一直卡在一个地方，等后续碰到相关题目或者代码时再结合具体语境搞清楚……
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

Solidity 语法的变量类型包括**值类型、引用类型、映射类型和函数类型**，这次先来介绍其中的值类型。

值类型（Value Type）主要包括布尔型、整数型、定长字节数组型、地址型和枚举型。

在 Solidity 中运行这些值类型的语法时，同样需要完整的合约语法，包括协议声明、编译器版本和合约定义。

## **布尔型**

布尔型只有两种值：`true` 和 `false`

布尔型的运算符则有五种：

`!`：非，取相反的逻辑

`==`：等于，取相等的逻辑

`!=`：不等于，取否定的逻辑

`&&`：与，根据短路规则，当第一个条件是 `false` 时，不会再判断第二个条件

`||`：或，根据短路规则，当第一个条件是 `true` 时，不会再判断第二个条件。

这里直接手搓代码来进行说明：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
​
contract BoolExample {
    bool public _bool = true; // 为 _bool 变量赋值为 true
    bool public _bool1 = !_bool; //  !_bool 与 _bool 取反，为 false
    bool public _bool2 = _bool == _bool1; // 两个变量一个为 true，一个为 false，因此最后为 false
    bool public _bool3 = _bool != _bool1; // 两个变量一个为 true，一个为 false，因此最后为 true
    bool public _bool4 = _bool && _bool1; // 第一个变量为 true，第二个变量为 false，根据短路规则最后为 false
    bool public _bool5 = _bool || _bool1; // 第一个变量为 true，第二个变量为 false，根据短路规则最后为 true
}
```

来一个看起来抽象的：

```
bool a = 1-1==0 && 1%2==1;
```

`&&` 取 false，但是两边都为 true，最后结果便也为 true。

## **整数型**

整数型包括 `int` 和 `uint`，前者是有符号整数，可以存负数、0 和正数，后者是无符号整数，只能存 0 和正数。`int` 和 `uint` 后面的数字后缀则代表位数（bits）

如果只是写 `int` 或 `uint`，Solidity 会将它们默认为最大的类型——`int` 等同于 `int256`，`uint` 则等同于 `uint256`。那么从 8 到 256 的位数都代表什么呢？

| 类型 | 位数 | 最小值 | 最大值公式 | 最大值 (十进制) | 典型用途 |
| --- | --- | --- | --- | --- | --- |
| uint8 | 8 bits | 0 | $2^8 - 1$ | 255 | 极小的数 (如年龄、百分比) |
| uint16 | 16 bits | 0 | $2^{16} - 1$ | 65,535 | 小计数器 |
| ... | ... | ... | ... | ... | ... |
| uint256 | 256 bits | 0 | $2^{256} - 1$ | 很大很大 ($1.15 \times 10^{77}$) | 最常用，适合存代币余额 |

`int` 的范围则被切割为两半，一半给负数，一半给正数（包含 0）

| 类型 | 位数 | 最小值公式 | 最大值公式 | 范围示例 |
| --- | --- | --- | --- | --- |
| int8 | 8 bits | $-2^7$ | $2^7 - 1$ | -128 到 127 |
| int16 | 16 bits | $-2^{15}$ | $2^{15} - 1$ | -32,768 到 32,767 |
| ... | ... | ... | ... | ... |
| int256 | 256 bits | $-2^{255}$ | $2^{255} - 1$ | 极其巨大 |

整数型的分类很细致，这对于 Gas 优化和存储很有帮助。例如，虽然 `uint256` 最为常用，但是在某些情况下需要用到更小的类型，比如 `uint8`，这可以节省 Gas。这里可以通过两种情景进行对比：

NaN.  单独使用数值型。Solidity 的虚拟机按照 256 位来处理数据，这时候如果单独定义一个 `uint8`，EVM 还是要用 256 位来进行处理，甚至需要进行填充操作，这可能比直接定义 `uint256` 要消耗更多 Gas。
      
NaN.  在结构体（struct）中使用数值型。如果将多个较小的数值型放在一起，Solidity 会将它们打包进同一个存储槽（Storage Slot）中，而一个槽位 256 位，在这种情况下，如果我们要处理四个 `uint64`，将它们打包起来，便可以在原来需要占用四个槽位的情况下，转变为只占用一个槽位，由此节省大量 Gas。
      
      ```
      // 这里的变量会打包，节省大量 Gas
      struct MiniPack {
          uint64 a;
          uint64 b;
          uint64 c;
          uint64 d; 
          // 4个64位加起来正好256位，只占1个槽
      }
      ```
      

整数型在运算符上包括比较运算符和算术运算符两种。

（1）比较运算符

NaN.  功能：返回布尔值
      
NaN.  类型
      
      -   `<=`
          
      -   `<`
          
      -   `==`
          
      -   `!=`
          
      -   `>=`
          
      -   `>`
          

（2）算术运算符

NaN.  功能：返回计算后的具体数值
      
NaN.  类型
      
      -   `+`
          
      -   `-`
          
      -   `*`
          
      -   `/`
          
      -   `**`：幂运算
          
      -   `%`: 除法但取余数
          

继续手搓代码来说明。

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
​
contract NumberExample {
    // 设置两个初始值
    uint256 public _number = 10;
    uint256 public _number1 = 100;
​
    // 比较运算（注意，由于返回的值属于布尔值，因此数据存储结构要写明为 bool）
    bool public _number2 = _number == _number1; // 结果为 false
    bool public _number3 = _number != _number1; // 结果为 true
​
    // 算术运算
    uint256 public _number4 = _number * _number1; // 结果为 1000
    uint256 public _number5 = _number % _number1; // 结果为 10 
}
```

## **定长字节数组型**

字节数组实际分为定长和不定长两种。

定长字节数组属于值类型，数组长度声明以后不可修改，根据字节数组长度可以分为 `bytes1`、`bytes8` 和 `bytes32`。其中，定长字节数组最多可以存储 32 bytes 数据，也就是 `bytes32`。

`bytesX` 代表占用 X 个字节（bytes），一个字节（bytes）等于 8 位（bits），在 16 进制中则用 2 个字符表示，比如 `0xFF`。相应地，`bytes8` 在 16 进制里有 16 个字符，`bytes32` 在 16 进制中有 64 个字符。

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
​
contract NumberExample {
    // 固定长度的字节数组
    bytes32 public _byte32 = "MiniSolidity"; 
    bytes1 public _byte = _byte32[0]; 
}
```

在上面这段代码中，本是字符串的 `MiniSolidty` 被以字节的形式——bytes32——存储进变量 `_bytes32` 之中，运行之后会发现 `_bytes32` 的值显示为 `0x4d696e69536f6c69646974790000000000000000000000000000000000000000`，正好 64 个字符。

`_byte` 的值则是取 `bytes1` 这一字节，具体是取 `_byte32` 的第一个字节，因此写成 `_byte32[0]`，运行出来结果是 `0x4d`。

不定长字节则属于引用类型，其数组长度在声明之后可以改变，包括 `bytes` 等。

## **地址型**

地址型一般分为两类。

-   普通地址：存储一个 20 字节的值，这是以太坊地址的大小。
    
-   payable address：比普通地址多出 `transfer` 和 `send`，用于接收转账。
    

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
​
contract NumberExample {
    // 普通地址
    address public _address = 0x7A58c0Be72BE218B41C608b7Fe7C5bB630736C71;
    address payable public _address1 = payable(_address); // payable address，可以转账、查余额
    // 地址类型的成员
    uint256 public balance = _address1.balance; // balance of address
}
```

第一段合约代码仅仅定义了基础的地址类型变量，并命名为 `_address` 和赋值，但是无法对其进行转账。第二段合约代码则通过 `payable()` 将普通的 `_address` 转换为可以收款的类型，新变量 `_address1` 不仅存有同样的地址，还可以接收转账。Solidity 为了安全，默认将地址看作不能接收转账的，如果需要往某个地址转账，则需要转化为 `payable` 类型。 最后一段合约代码中，`.balance` 是所有 `address` 类型都自带的属性，这段代码会到以太坊的状态树中查询该地址当前持有的 ETH 数量（单位为 Wei），查询到的余额数值会被存入 `balance` 变量之中。类型则也被定义好为 `uint256`。这段代码会在合约部署时就会执行并定格，换句话说，`balance` 变量存的是合约部署时该地址的余额，而非实时的。

## **枚举型**

`enum` 可以给 `uint` 分配名称，从 0 开始，以使程序易于阅读和维护。

```
// 用enum将uint 0， 1， 2表示为Buy, Hold, Sell
enum ActionSet { Buy, Hold, Sell }
// 创建enum变量 action
ActionSet action = ActionSet.Buy;
// enum可以和uint显式的转换
function enumToUint() external view returns(uint){
    return uint(action);
}
```
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


## **1\. Solidity 语法中的基本结构**

在 Solidity 代码中，都必须先指定和声明 Solidity 的编译器版本。

```
pragma solidity ^0.8.26;
```

接下来的具体内容以实例来理解 Solidity 语法中的基本结构，以下面三段 Solidity 语法为例。

```
contract HelloWorld {
    string public greet = "Hello World!";
}
contract Counter {
    uint256 public count;
​
    // Function to get the current count
    function get() public view returns (uint256) {
        return count;
    }
​
    // Function to increment count by 1
    function inc() public {
        count += 1;
    }
​
    // Function to decrement count by 1
    function dec() public {
        // This function will fail if count = 0
        count -= 1;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
​
contract MyContract {
    // 状态变量
    uint256 public myNumber;
​
    // 构造函数
    constructor() {
        myNumber = 100;
    }
​
    // 函数
    function setNumber(uint256 _number) public {
        myNumber = _number;
    }
}
```

### **1.1 合约定义结构**

可以看到三段代码的共通处是都含有 `contract XXX {}` 的结构——**合约定义**（Contract Definition），是智能合约中的基本构件单元，类似于 Java 或者 C++ 中的类（Class）。

-   `contract`：向编译器声明这是一个合约模块。
    
-   `XXX`：合约的名称，由开发者来定义，通常用大驼峰命名法——首字母大写——来书写，比如上面例子中的 `HelloWorld` 和 `Counter`。
    
-   `{}`: 合约体，里面是合约的具体内容，主要包括**状态变量**和**函数**。
    
    -   **状态变量**可以被简单理解为「数据」，相当于合约的「属性」/「肉体」，主要存数据。 如同文档中所定义的：
        
    
    > 状态变量是指在合约中定义的、其值永久存储在区块链上的变量。它们用于记录合约的持久化数据，构成了合约的整体状态。当合约被部署后，这些变量将被写入区块链，并在合约的整个生命周期中保持可访问性和可追踪性。
    
    -   **函数**则可以被简单理解为「逻辑」/「行为」，或者说是操作数据的方法。函数是一个统称，可以分出构造函数、普通函数。**构造函数** `constructor` 比较特殊，是初始化逻辑，只在合约部署时执行一次，通常用于给状态变量赋予初始值；**普通函数** `function` 可以被理解为具体的执行或业务逻辑，随时都可以执行，用于日常的交互、数据读取和修改等。
        

前面讲到了 `contract xxx {}` 这样的基本结构，下面继续对其中的要素进行理解，比如其中的数据存储结构（含状态变量）——可在此[回顾](https://guozheng.rbind.io/project/compile-and-deploy-message-contract/#1%e6%95%b0%e6%8d%ae%e5%ad%98%e5%82%a8%e7%bb%93%e6%9e%84%e9%83%a8%e5%88%86)、构造函数和普通函数。

### **1.2 构造函数结构**

构造函数的基本结构如下：

```
constructor(参数列表) {
    // 初始化逻辑
}
```

`constructor` 是关键字也是名字，没有自定义的名称，`(参数列表)` 内用接收部署时传入的初始参数。另外，构造函数不需要写可见性，因为默认只能在部署时由系统调用，而且由于其任务是初始化，所以不存在返回结果。

### **1.3 普通函数结构**

普通函数结构我们已经[看过](https://guozheng.rbind.io/project/smart-contract-development/#solidity-%e6%99%ba%e8%83%bd%e5%90%88%e7%ba%a6%e7%bc%96%e7%a8%8b)了，但是没有与智能合约中的其他部分一起进行梳理，在此再次书写一次，以便对比和理解。

```
function 函数名(参数列表) 可见性 状态可变性 修饰符 返回类型 {
    // 函数体
}
```

-   `function` 固定开头，声明要定义一个函数；
    
-   `函数名` 部分可以自定义，且通常采用小驼峰命名法——第一个单词首字母小写，从第二个单词开始每个单词的首字母大写；
    
-   `(参数列表)` 内的内容是函数接收的参数，可以为空；
    
-   `可见性` 包括 `public`（谁都可以调用参数）、`private`（只有合约自己内部可以调用参数）、`internal`（自己和继承的子合约可以调用参数） 和 `external`（只有外部能调用参数）四种；
    
-   `状态可变性` 部分详细可见[此处](https://guozheng.rbind.io/project/compile-and-deploy-message-contract/#4%e6%a0%b8%e5%bf%83%e5%8a%9f%e8%83%bd%e5%87%bd%e6%95%b0);
    
-   `返回类型` 是函数返回的结果，如果有——写成 `returns` 而非 `return`——需要告知其返回的数据类型。
    
-   `修饰符` 有点复杂，它允许在函数执行前插入额外逻辑，常用于权限控制与前置检查。
    

理解函数的基本结构，对于理解新的代码很有帮助，还是以一段计数代码为例。

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;
​
contract Counter {
    uint256 public count;
​
    // Function to get the current count
    function get() public view returns (uint256) {
        return count;
    }
​
    // Function to increment count by 1
    function inc() public {
        count += 1;
    }
​
    // Function to decrement count by 1
    function dec() public {
        // This function will fail if count = 0
        count -= 1;
    }
}
```

在上面这段代码中，合约名称直接命名为计数器 `Counter`，合约中的第一行内容即状态变量 `count`，它会被永久存储在区块链上，其数据存储结构是 `uint256`，也就是说 `count` 的类型已经确定为 256 位无符号整数。`public` 表示变量是公开的，外部用户可以直接通过调用 `count()` 来查看值，不需要额外写 `get()` 函数——虽然示例里为了演示还是写了一个。

之后的三段代码都是普通函数，根据加减进行了不同的命名，并基于此在函数体中进行了不同的具体描述。

## **2\. 南塘道分享会**

今晚的南塘 DAO 分享会让我想起了过去在西埠村做田野调查的经历。

或许可以理解为，南塘 DAO 在做的事情是将 Web3 技术和线下的乡村建设与治理结合起来，又或者说是 DAO 理念的一次实践。如同第二位分享者说的，在传统公司模式中，老板决策，全员沉默执行，高效但压抑；不过在南塘 DAO 早期，是人人有权，但没有共识，很凌乱。基于这种情况，引入了「罗伯特议事规则」，似乎在这一规则体系之下，议事不批判人，只完善事，规则首先保护的是提出不成熟想法的人。——「当每个人都感到安全，智慧才会真正开始流动」。换句话说，提出想法的人无法得到有效的保护，感受不到安全，是许多会议失败的原因。感到安全时，智慧才会流动。

在议事之中，我们一开始就不需要诸葛亮，而是通过抚平信息差，让我们这些受过相似教育、底层代码相似的年轻人的个性和差异成为了资源，发挥三个臭皮匠的作用。

第三位分享者富章来自台湾，是上一期的学员，负责议事规则实践，提到了书籍《可操作的民主：罗伯特议事规则下乡群全纪录》。

他还提出，每个人的想法是否可以保留在区块链中？一群人的共识。以某种智能合约的方式来运作，而这个规则是非常程序化的。希望开会不仅人主持，进一步由 ai 主持，由区块链、智能合约执行？

第一位分享者小白说到大家的 Web3 实践如何与官方的沟通：只谈区块链的技术——记事簿，但不涉及法币。

据闻南塘道已经有了一个实践：

> 我们第二期艺术共创营已经过半了，这次的主题是南塘人物志，挖掘的是每一位村民最深刻的故事，并用画作和NFT保存下来。预计2月2日我们会进行线上展览，如果感兴趣的可以持续关注喔

为什么要用去中心化的理念来实践？区块链的技术可以让大家看到自己的贡献？而且这种贡献会一直存在且无法被篡改

基于文档、照片，形成 NFT？

将村民的故事、村庄历史变成区块链上永久的、不可篡改的 NFT，可以成为在西埠村的尝试吗？
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



# 开发我的第一个 Web3 Vibe-Coding Demo 项目

## **1\. 今日想法**

黑客松即将到来，我决定今天上手结合 AI，试试能不能开发一个 DApp——严格来说或许只是一个 Demo 吧。同时，今天在 Co-Learning 上又听到 wachi 老师分享了自己关于 Web3 的学习理念，我最为在意的内容是**对概念的学习吸收**。具体来说，其实技术向的学习也离不开概念的理解，许多概念越发熟悉，在写代码和做项目时也会越发得心应手。

此外，还有十分重要的一点是「做中学」，一味地学习概念会让人感到迷失，但是在实践和应用中发现和理解知识会更生动、形象、深刻。总的来说「学习概念或知识」和「实践」两者都不可偏废，是相辅相成的关系——这点我在 Web3 中的感受十分深刻。

例如，今天 wachi 老师说到如何给 AI 提做黑客松的需求时，举到以「ERC-8004」为例子的技术点让 AI 帮忙进行开发，这无疑使得 AI 的协助更加具体，而不会天马行空地给出不着边际的方案。同时，我也才发现自己对 ERC 系列都不了解，不知道 ERC 都是些什么，但此次听了 wachi 老师的分享并找 AI 协助开发 Dapp 时，我却对 ERC 的印象深刻了许多，远非看实习手册或听分享会时困顿、迷糊的状态。

我的第一份提示词如下：

> 我是一个 Web3 新人，需要参加一场 Web3 的休闲黑客松，目的做出一个产品。但我目前只是学习了一周，了解 Web3 的基本概念（如区块链、去中心化、Gas 等），跟着文档在 Remix IDE 上编译和部署过关于链上留言的智能合约代码（我的记录在[这里](https://guozheng.rbind.io/project/compile-and-deploy-message-contract/)）,所以我感觉我的情况可能不一定能做得了复杂的产品。你可以基于我的情况进行评估，然后为我提供 3-5 个可行的方案吗？相关背景是以 ERC8004 为技术点，制作产品的时间在一周以内，技术性可以不用太强，让普通人也可以感受到这个产品的作用即可。

我和 Claude Opus 讨论了一阵子，决定基于昨天的「[链上留言](https://guozheng.rbind.io/project/compile-and-deploy-message-contract/)」智能合约代码，做一个在区块链上对 AI Agent 进行评分的网页。

这里再插播一段：如果对区块链的知识不够了解，即便有再强大的 AI，也不知应该提出怎样的 idea 来开发产品。

下面让我和 AI 动手一起开发一个极其简单的 Dapp 吧！这里再插播一句：也是经过这一次动手实践，我还对原来看到的概念有了更深的了解，也解决了一些过去遗留的问题。

## **2\. 构建 AI Agent 信誉评分器**

我们先简单回顾一下 [Dapp 的开发流程](https://guozheng.rbind.io/project/smart-contract-development/#dapp-%e5%bc%80%e5%8f%91%e6%b5%81%e7%a8%8b)：

需求分析 → 智能合约开发 → 检索器开发 → 前端开发 → 与区块链交互 → 部署与上线

### **2.1 需求分析**

我们的需求是**搭建一个简单的网页**，在这个网页上，用户可以对特定的 AI Agent ——用一串数字 ID 来指代——进行 1-5 星级 的评分，同时留下自己的评论。同时，由于属于 Web3 的技术范畴，因而所有数据都会存储在区块链上，且不可篡改。

### **2.2 智能合约开发**

明确需求后，先在 Remix IDE 中编写好相关智能合约代码。

具体做法是在 `contracts` 文件夹中创建新文件，我命名为 `AgentReputation.sol`，代码如下：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
​
contract AgentReputation {
    
    // 定义一个评价的结构：包含 评分人、分数、评论内容
    struct Review {
        address reviewer; // 谁打的分
        uint8 score;      // 分数 (1-5)
        string comment;   // 具体的评价
        uint256 timestamp; // 时间戳
    }
​
    // 建立一个映射：通过 Agent ID (数字) 就能查到它所有的 评价列表
    mapping(uint256 => Review[]) public agentReviews;
​
    // 一个事件：当有人打分成功时，通知区块链外的世界
    event NewReview(uint256 indexed agentId, address indexed reviewer, uint8 score);
​
    // --- 功能函数 ---
​
    // 1. 打分函数
    // agentId: 给哪个AI打分, score: 几颗星, comment: 评价内容
    function rateAgent(uint256 _agentId, uint8 _score, string memory _comment) public {
        // 检查：分数必须在 1 到 5 之间
        require(_score >= 1 && _score <= 5, "Score must be between 1 and 5");
​
        // 创建一个新的评价
        Review memory newReview = Review({
            reviewer: msg.sender, // msg.sender 就是调用这个函数的人（钱包地址）
            score: _score,
            comment: _comment,
            timestamp: block.timestamp
        });
​
        // 把它加入到列表中
        agentReviews[_agentId].push(newReview);
​
        // 发出通知
        emit NewReview(_agentId, msg.sender, _score);
    }
​
    // 2. 查询某个 Agent 有多少条评价
    function getReviewCount(uint256 _agentId) public view returns (uint256) {
        return agentReviews[_agentId].length;
    }
​
    // 3. 获取特定的某一条评价 (因为数组太长不能一次取完，通常需要分页或者按索引取)
    function getReview(uint256 _agentId, uint256 _index) public view returns (address, uint8, string memory, uint256) {
        Review memory review = agentReviews[_agentId][_index];
        return (review.reviewer, review.score, review.comment, review.timestamp);
    }
    
    // 4. 获取某个 Agent 的所有评价（注意：如果评价太多，这个函数可能会因为Gas超限而失败，但做Demo没问题）
    function getAllReviews(uint256 _agentId) public view returns (Review[] mmory) {
        return agentReviews[_agentId];
    }
}
```

### **2.3 智能合约代码编译与部署**

智能合约代码编写好即可进行编译，编译的过程很简单，只需在 `Solidity Compiler` 部分（Remix IDE 界面最左侧的第三个图案）点击 `Compile AgentReputation.sol` 按钮即可，如果成功图标会出现绿色的对钩。

编译成功后即可到 `Deploy & Run Transactions` 部分（Remix IDE 界面最左侧的第四个图案）进行部署，然后在部署环境中选择 `Injected Provider - MetaMask`，这时候在浏览器安装好的 MetaMask 插件会弹出窗口——好了，这里我遇到了两个问题，第一个昨天已经遭遇但未能解决，但最终都在 AI 的协助下解决了，我认为很值得单独记录。

（1）让 MetaMask 的网络切换为 Sepolia 测试网

我在 Remix IDE 的部署界面中没能成功连接 Sepolia 测试网，而是默认为以太网主网。

解决方案有两步：

-   第一步是点击插件右上角的第二个图标，然后进入 `设置 → 高级` 部分，然后将 `显示测试网络` 一栏打开，接着就可以在插件的主页面处将网络选择为 Sepolia；
    
-   第二步是点击插件右上角的第一个图标，然后点击显示网络名称，进入 `管理网络`，拉到下方的 `显示测试网络`，找到 Sepolia 并点击。
    

完成以上两个步骤后，可以确保 Remix IDE 连接的是 Sepolia 测试网。我在[第一次编译和部署「链上留言」](https://guozheng.rbind.io/project/compile-and-deploy-message-contract/)时就碰到这个问题，但是没有解决，导致无法正常部署——毕竟部署是要 Gas 费的，但是作为新人的我在主网显然没有以太币，但测试网中的测试币还是有一些的。

（2）我在自己的 Google 上直接搜索 Remix，但没注意自己进入的 Remix 网站竟然不是官方的，而是 [https://remix.polkadot.io/](https://remix.polkadot.io/) ——Polkadot 版本的 Remix——它虽然也可以编译 Solidity，但是对测试网的兼容性有问题，无法正常部署，切换回官方网站 [https://remix.ethereum.org](https://remix.ethereum.org/) 就成功部署了。

将以上两步都完成以后，我终于成功将智能合约代码部署到了 Sepolia 测试网的区块链上——控制台下出现绿色的对勾，显示合约成功部署，接下来就可以进入前端开发阶段，将部署的合约变成真正的网页应用。

但在此前，我们需要获取合约地址和 ABI，前者在部署成功以后 Remix 左下角的 `Deployed Contracts` 列表中复制，一般 `0x` 开头；后者可以回到 `Solidity Compiler` 部分，在最下方有一个 ABI 的复制选项，点击复制即可获取。

### **2.4 前端开发**

我创建了一个新的文件夹 `AgentReputation`，创建 `index.html` 和 `app.js` 文件，之前拿到的合约地址和 ABI 就填入 `app.js` 文件中。

`index.html` 和 `app.js` 的代码太多，就不在此书写，可以到我的[仓库](https://github.com/residualsun1/AgentReputation)中查看。

将合约地址和 ABI 都填入 `app.js` 文件之中，由于此次开发没有后端内容\[^nobackend\]，直接打开 `index.html` 文件就可以看到网页。

\[^nobackend\]: 严格来说，智能合约本身也算是一种「后端」，但通常我们说的“后端”指的是传统的服务器程序（如 Node.js, Python Flask 等）。在这个项目中，我们完全依赖区块链来存储和验证数据，所以没有写传统的后端代码。不过我觉得也可以想一想，是否可以加入传统的后端内容？如果加入了传统的后端内容可以做成什么样？

不过由于没有将这些本地文件部署到云端——点击 `index.html` 文件后，地址栏显示`D://...`，因此在网页中点击「连接钱包」会由于浏览器的安全限制而要求安装 MetaMask，不允许访问其插件。

### **2.5 部署网站**

要将项目部署到云端，我的常规做法是将源代码文件托管到 GitHub，之后到 Netlify 中进行部署，网站可见：[https://agentreputation.netlify.app/](https://agentreputation.netlify.app/)

不过部署好网站后，「连接钱包」仍然是显示失败，向 Gemini 3 Pro（High）求助，得到的回应是大概率是因为浏览器 「混合内容」(Mixed Content) 的安全拦截或者 Ethers.js 版本加载失败，可以在网站进行检查，查看控制台的情况。

原来是因为 `cdn.ethers.io` 加载失败。

```
<script src="https://cdn.ethers.io/lib/ethers-5.7.umd.min.js"></script>
```

更换一个新的 CDN，然后将文件推送到仓库中重新部署，再重新进入网站就可以连接钱包了。

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/ethers/5.7.2/ethers.umd.min.js"></script>
```

一切搞定以后，我们就可以使用这个简单的 Dapp 了，它的具体用法是：先连接自己的钱包，然后在 Agent ID 输入框中输入某个数字，比如 `123`——它是你指代的某个 AI，下面给它点星星，接着还可以输入具体的评论内容，比如我输入的 ID 是 `888`，然后给了 5 星，还评论了「Claude Opus 4.5 的表现超乎我的想象！」，最后点击「提交评价」，会发现钱包弹出窗口，需要我们确认交易，并消耗一点测试币，点击确认即可，网页不久便会弹出成功上链的说明。

![01-21-6.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/residualsun1/images/2026-01-21-1769010745816-01-21-6.png)

现在我们到网页最底部的「查看历史评价」，输入 `888`，然后点击「查询」，即可看到之前留下的评论。

![01-21-8.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/residualsun1/images/2026-01-21-1769010758777-01-21-8.png)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




## 1\. 理解「链上留言」的合约代码

以下是一段关于「链上留言」的智能合约代码。

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0; // 指定 Solidity 编译器版本为 0.8.0

contract MessageBoard {
    
    // （1）数据存储结构
    // 保存所有人的留言记录
    mapping(address => string[]) public messages; 

    // （2）事件定义
    // 留言事件，便于检索器和区块链浏览器追踪
    event NewMessage(address indexed sender, string message);  

    // （3）构造函数
    // 在部署时留言一条欢迎词
    constructor() {
        string memory initMsg = "Hello ETH Pandas";
        messages[msg.sender].push(initMsg);
        emit NewMessage(msg.sender, initMsg);
    } 

    // （4）核心功能函数
    // 发送一条留言
    function leaveMessage(string memory _msg) public {
        messages[msg.sender].push(_msg);     // 添加到发言记录
        emit NewMessage(msg.sender, _msg);   // 发出事件
    }

    // 查询某人第 n 条留言（从 0 开始）
    function getMessage(address user, uint256 index) public view returns (string memory) {
        return messages[user][index];
    }

    // 查询某人一共发了多少条
    function getMessageCount(address user) public view returns (uint256) {
        return messages[user].length;
    }
}
```

对于以上代码，我们可以拆分开来进行理解。此前，让我们回顾一下——一个智能合约的基本结构通常由三部分组成：**状态变量、构造函数和函数**。

### （1）数据存储结构部分

```markdown
// 保存所有人的留言记录
mapping(address => string[]) public messages; 
```

`mapping` 是映射类型`mapping(address => string[])` 是数据存储结构`messages` 是状态变量——永久存储在区块链上。数据存储结构中的 `address => string[]` 将用户钱包地址映射到一个字符串数组， `address` 是用户的钱包地址，也就是以太坊地址 `string[]` 则是用户的留言，且由于是数组形式，因此可以有多条。外部的 `public` 则是指自动生成一个查询函数，外部可以据此读取数据。  
  
在此我们需要区分一下「数据存储结构」与「状态变量」之间的关系。「数据存储结构」指的是数据的组织方式，「状态变量」是变量本身。状态变量通过数据存储结构来定义其自身的类型。以 `messages` 和 `mapping(address => string[])` 之间为例 `message` 呈现出来的正是 `mapping(address => string[])` 所表达字符串组信息，比如 `“这是第一条留言”, “这是第二条留言”`。

````markdown
以下都是数据存储结构
mapping(address => string[])   // 映射类型
uint256                        // 无符号整数
string                         // 字符串
address                        // 地址类型
string[]                       // 字符串数组
struct User { ... }            // 结构体类型
```
````

```markdown
contract Example {
    // 状态变量名: messages
    // 数据类型: mapping(address => string[])
    mapping(address => string[]) public messages;
    
    // 状态变量名: totalSupply
    // 数据类型: uint256
    uint256 public totalSupply;
    
    // 状态变量名: owner
    // 数据类型: address
    address public owner;
}
```

### （2）事件定义部分

```markdown
// 留言事件，便于检索器和区块链浏览器追踪
event NewMessage(address indexed sender, string message);  
```

看到这部分有一个 `event`，指的是事件。事件用于记录区块链上发生的重要操作，里面的 `indexed` 可以使之后的 `sender` 作为索引以进行快速检索。所以整个事件定义部分可以让 Dapp 的前端监听该事件以实时更新界面，区块链浏览器可以追踪谁发了什么留言，而且比直接读取存储便宜。事件定义部分就相当于区块链日志（logs）。

### （3）构造函数部分

```markdown
// 在部署时留言一条欢迎词
constructor() {
    string memory initMsg = "Hello ETH Pandas";
    messages[msg.sender].push(initMsg);
    emit NewMessage(msg.sender, initMsg);
} 
```

构造函数旨在合约部署时执行一次 `msg.sender` 是部署合约的人的地址，`emit` 用于触发事件通知，综合来看，这里的构造函数可以自动为部署合约的人留下一条欢迎消息「Hello ETH Pandas」。

### （4）核心功能函数

````markdown
// 发送一条留言
function leaveMessage(string memory _msg) 
    public 
{
    messages[msg.sender].push(_msg);     // 添加到发言记录
    emit NewMessage(msg.sender, _msg);   // 发出事件
}

// 查询某人第 n 条留言（从 0 开始）
function getMessage(address user, uint256 index)
    public 
    view 
    returns 
    (string memory) 
{
    return messages[user][index];
}

// 查询某人一共发了多少条
function getMessageCount(address user)
    public
    view
    returns
    (uint256)
{
    return messages[user].length;
}
```

让我们再回顾以下 Solidity 中函数的标准声明格式。

```Solidity
function <函数名>(<参数列表>)
    <可见性>
    <状态可变性>
    <修饰符列表>
    <虚拟/重写关键字>
    returns (<返回值列表>)
{
    // 函数体
}
````

好，接下来则需要区分一下 Solidity 函数的类型，我们主要从「状态可变性修饰符」来进行分类，主要有四种，分别是无可变性修饰符`view`、 `pure` 和 `payable`。

| 修饰符 | 是否读取状态 | 是否修改状态 | 是否接收 ETH | Gas 费用 |
| --- | --- | --- | --- | --- |
| 无修饰符（普通） | ✅ | ✅ | ❌ | 需要 |
| view | ✅ | ❌ | ❌ | 免费* |
| pure | ❌ | ❌ | ❌ | 免费* |
| payable | ✅ | ✅ | ✅ | 需要 |

\*免费是指本地调用时；如果通过交易调用仍需 Gas

注意：智能合约不一定必须含有普通函数，从语法角度来看，一个合约只包含 `view` 或 `pure` 函数也是完全合法的。但从实用角度来看，只有 `view` 或 `pure` 函数的合约由于无法改变状态，因而没有实际价值。大多数有意义的合约都会有至少一个可以修改状态的函数。

补充完相关知识后，可以对合约中具体的三个函数进行解释。

```
function leaveMessage(string memory _msg) 
    public 
{
    messages[msg.sender].push(_msg);     // 添加到发言记录
    emit NewMessage(msg.sender, _msg);   // 发出事件
}
```

这个函数可以发送事件通知前端，`public` 意味着任何人都可以调用，`string memory` 意为参数临时存储在内存中，而非永久存储（便宜）——相对低，`storage` 则意为永久保存在区块链上（昂贵），`.push()` 是将新留言添加到该用户的留言数组末尾。

```
function getMessage(address user, uint256 index)
    public 
    view 
    returns 
    (string memory) 
{
    return messages[user][index];
}
```

这个函数可用于查询指定留言，通过索引查询某用户的第 n 条留言，从 0 开始。里面的 `view` 意味着只读函数，不修改状态也不消耗 Gas。

```
function getMessageCount(address user)
    public
    view
    returns
    (uint256)
{
    return messages[user].length;
}
```

这个函数可以查询用户总共发送了多少留言，配合上面的函数可以遍历所有留言。

## **2\. 理解编译与部署信息**

在 Remix IDE 中创建文件 `messageboard.sol` 中，将「链上留言」合约代码粘贴到其中，之后进行编译和部署，可以查看以下消息。

-   `status` 指代交易状态码，1 = 成功，0 = 失败。此处为 1 表示合约已经成功部署并且执行。
    
-   `transaction hash` 指代交易哈希，它是我们发起的这笔交易在区块链上的唯一 ID，可以在区块链浏览器中查询这笔交易的所有细节。
    
-   `block hash` 指代区块哈希，是这一区块的唯一标识，`block number` 指这笔交易被打包进第 6 个区块——一个区块可以包含多笔交易。
    
-   `contract address` 指代合约地址，是这份合约在区块链上的永久地址，后面调用合约函数都需要用到该地址，是**最重要的信息之一**，如同合约的身份证。
    
-   `from` 后的字符是部署该合约的账户地址，我们在 Remix IDE 中进行测试部署的第一个账户，也是构造函数中 `msg.sender` 的值。
    
-   `to` 之后的字符则是表示调用了合约的构造函数——部署合约时总是调用构造函数。
    
-   `transaction cost` 是指部署的总费用，包括数据存储、计算等所有开销，是实际支付的 gas
    
-   `execution cost` 是执行费用，指实际执行智能合约逻辑的成本，`transaction cost` - `execution cost` 就是数据存储的开销。
    
-   `output` 输出的大量字节码是合约的编译后字节码，EVM 实际执行的机器码，也是 Solidity 代码编译之后的二进制表示。
    
-   由于构造函数没有参数，而且也没有返回值，因此 `decode input` 输入为空，`decode output` 输出为空。
    
-   `logs` 指事件日志，证明了此次构造函数成功执行，其中，`event: "NewMessage"` 触发了在构造函数中定义的 `NewMessage` 事件，`arg[0]` 指代部署者地址，`agr[1]` 指代留言内容。总的来说，这是构造函数里 `emit NewMessage(msg.sender, initMsg)` 的结果。
    
-   `raw logs` 指原始日志。
    

## **3\. 测试网与领取 Sepoia 代币**

\> 以太坊测试网（Ethereum Testnets）是用于开发、测试和部署智能合约的网络环境，它们模拟主网功能但使用无价值的测试代币，让开发者可以安全地进行实验而无需承担真实的经济成本。

第一周便听到测试网这个概念，同时也领取了 SepETH，但一直不清楚是什么。特别是 Sepolia，原来它和 Holesky 都是测试网的一种，两者的共识机制都是 Pos，前者特点是长期支持的主要测试网，与主网最相似，稳定性高，一般用于最终部署前测试，生产环境模拟，Dapp 集成测试。

在将合约正式部署到 Sepoia 测试网前，让我们先领取一些测试币。关于测试币的领取，第一周在一个网站中已经领取过，并且学员们也会相互赠送一些，这次到 [https://sepolia-faucet.pk910.de/](https://sepolia-faucet.pk910.de/) 中领取。

该网站也叫「水龙头」，领取测试币的过程则被称作「领水」，在该网站中输入自己的 Sepoia 测试地址，然后 start minting，水龙头就可以启动 Pow（Proof of Work）来开始挖矿，获得 SepETH。背后的运行规则是浏览器持续运行挖矿脚本，这需要消耗 CPU 算力，同时 SepETH 也会不断增加，等到一定量以后停止挖矿，便会获得相应量的测试币。一般来说，0.05 - 0.1 SpeETH 可以部署 1 - 2 个合约，0.2 - 0.5 SepETH 可以进行多次部署和交互，0.5 - 1 ETH 则可以用于长期开发了。

根据我看到的信息，至少要挖到 0.05 SepETH 才可以领取，单次最多则只能领取 2.5。

最后我领取了 0.503 个 SepETH。

## **4\. 将合约部署到 Speoia 测试网**

纯粹按照手册的指示，选择 `Injected Provider - MetaMask`，我无法成功部署。

在 Remix IDE 里的部署部分连接钱包，我最后在 Enviroment 中的浏览器拓展选择 Sepolia 测试网，之后编译和部署，倒也能跑出反馈。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->





\## Dapp

\* 去中心化应用

\* 运行在区块链或分布式网络上，而非传统服务器

\* 去中心化技术栈 + 智能合约编程 + 前端与区块链的交互方式

Dapp 与传统应用的比较：

对传统应用来说，相当于去银行办业务，到前台（前端）填写表单，由柜员（后端服务器）处理我们的请求，这个过程是我们与柜员直接沟通，然后完成操作。但在 Dapp 中主要是通过代理人办理区块链业务，到前台（前端网页）填写表单，然后私人代理（钱包，如 MetaMask）会验证我们的身份，帮我们签字。邮递员（RPC 节点）负责传递信息，最后由公证处（区块链）来进行最终的执行和记录。

\## Dapp 架构

-   前端
    
    -   _Dapp 与_用户**交互的界面**（由 HTML、CSS、Javascript、React、Rust 构建）
        
    -   Dapp 前端不直接连接区块链网络，通过「钱包注入的 provider」或「第三方 RPC 节点」与区块链交互
        
    -   需要区块链钱包（如 MetaMask）进行身份验证和签署交易，确保隐私和安全
        
    -   _通过 RPC 节点对智能合约发起只读调用，_**获取合约状态、事件日志等链上数据**（通俗来说，「只读」可以被理解为想查看账户余额，前端通过 RPC 节点，然后直接读取区块链数据，最后现实结果）
        
    -   对需要修改状态的操作，由前端构造对智能合约的交易调用，交由钱包完成签名后，再通过 RPC 节点广播到区块链网络并最终上链执行（这里可以简单理解为想转账，然后前端构造交易，钱包随之弹出确认窗口，RPC 节点再广播到区块链，矿工进行打包执行，状态发生改变）
        

{{% notice warning "补充"%}}

RPC：一种通信协议，允许应用程序通过网络调用远程服务器上的函数或方法。可以被理解为区块链网络的「入口」，帮助发送请求和接收数据。

{{% /notice %}}

-   智能合约
    
    -   Dapp 的核心，定义应用的业务逻辑
        
    -   部署在区块链上
        
    -   由 Solidity 编写，通过 EVM 执行
        
-   数据检索器
    
    -   智能合约以 `Event` 的形式释放日志事件，数据检索器检索这些数据并写入 PostgreSOL 等传统数据库中（这部分还是感到困惑）
        
-   区块链和去中心化存储
    
    -   区块链存储智能合约的状态数据及交易记录
        
    -   去中心化存储包括 IPFS 或 Arweave，存储大规模非结构化数据，如图片、视频、文档等
        
    -   去中心化存储使得所有数据在多个节点上备份，确保数据持久性和去中心化特性
        

\## Dapp 开发流程

（1）需求分析

-   功能需求：转账？查询余额？创建投票？
    
-   区块链平台选择：在哪个平台构建 Dapp？以太坊？Solana？Polygon？
    
-   用户体验设计：Dapp 界面设计和交互流程
    

（2）智能合约开发

-   编写智能合约：使用 Solidity 或其他相关语言编写合约，确保满足（1）需求分析
    
-   编写测试用例：为智能合约编写单元测试，确保合约逻辑正确
    
-   审计和优化：对合约进行安全审计，确保合约安全性，避免常见漏洞——重入攻击、整数溢出等
    

（3）检索器开发

-   确定功能需要的数据内容：前端使用的数据大部分来自检索器
    
-   编写检索器程序：主流检索器框架（如 ponder 和 subgraph）都用了 TypeScript 语言作为检索器的程序编写语言
    
-   部署和运维：编写程序完成后用 Docker 部署到云服务器中
    

（4）前端开发

-   选择前端框架：可用现代前端框架如 React、Vue 来构建 UI，前端会通过 JavaScript 与智能合约进行交互
    
-   连接钱包：通过 MetaMask 等 Web3 钱包，用户可以连接到 Dapp，并授权其与智能合约交互
    
-   显示区块链数据：前端需要从区块链和检索器内获取数据——如数据余额、交易记录，通过用户界面展示
    
-   处理交易签名与确认：用户发起交易时，前端与钱包交互，获取用户签名并将交易发送到区块链
    

（5）与区块链交互

-   读取数据：前端通过智能合约的公开函数读取区块链上的状态数据——余额、合约信息
    
-   发送交易：当用户发起交易时，前端需要通过钱包签署交易并发送到区块链，执行合约中的功能（如转账）
    

（6）部署与上线

-   部署智能合约：使用 Hardhat 或 Foundry 等工具将智能合约部署到测试网（如 Sepolia）或主网
    
-   前端部署：将前端应用部署到去中心化平台（如 IPFS）或传统的 Web 服务（如 Vercel）
    
-   发布和维护：将 Dapp 上线，收集用户反馈并定期更新合约和前端
    

\## RPC 简述

RPC（Remote Procedure Call），意为「远程过程调用」，是\*\*连接前端应用与区块链网络的关键桥梁\*\*。RPC 是理解 Dapp（去中心应用）和传统应用之间差别很重要的一个概念，我认为主要体现在两者的交互方式上。

Dapp 和传统应用都有前端。传统应用没有 RPC，数据和资产属于平台，前端通过调用后端 API 来获取数据和执行操作，「转账」时告诉银行服务器个人信息和密码，银行服务器验证过后完成操作；Dapp 中，数据和资产都属于私人，前端通过个人钱包和 RPC 与区块链进行交互，「转账」时需要个人的钱包签名，然后再由 RPC 广播到全网，全网的验证者验证信息无误后执行操作。

RPC 功能

-   **读取链上数据**。包括查询账户余额、交易历史，读取智能合约的状态变量，获取区块信息、Gas 价格等。
    
-   **发送交易**。将用户签名的交易广播到区块网络，查询交易状态和确认数，估算交易所需的 Gas 费用。
    
-   **事件监听**。监听智能合约事件（Events），实时获取链上状态变化
    
-   **网络管理**。切换不同的区块链网络（主网、测试网）等
    

\## Solidity 智能合约编程

Solidity 是面向合约的高级编程语言，专门用于在以太坊虚拟机（EVM）上编写智能合约。

-   每个 Solidity 文件都必须以版本声明开始：
    

\`\`\`solidity

pragma solidity ^0.8.0;

\`\`\`

-   `//` 是 solidity 文件中的单行注释符号
    
-   `pragma` 关键字用于声明 solidity 源代码所需的编译器版本，确保合约在兼容的编译器环境中正确编译
    
-   `contract` 关键字用于定义一个智能合约，其语法格式为 `contract 合约名 { ... }`
    
-   函数是 Solidity 智能合约中执行具体逻辑操作的核心组成部分。
    
-   通过函数，可以实现对状态变量的读取、修改，或执行特定业务逻辑
    
-   一个智能合约的基本结构通常由三部分组成：状态变量、构造函数、普通函数
    

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract MyContract {

// 状态变量

uint256 public myNumber;

// 构造函数

constructor() {

myNumber = 100;

}

// 函数

function setNumber(uint256 \_number) public {

myNumber = \_number;

}

}

\`\`\`

-   Solidity 中函数的标准声明格式如下
    

\`\`\`solidity

function <函数名>(<参数列表>)

<可见性>

<状态可变性>

<修饰符列表>

<虚拟/重写关键字>

returns (<返回值列表>)

{

// 函数体

}

\`\`\`

{{% notice info "说明" %}}

-   <函数名>：函数的名称；
    
-   <参数列表>：传入函数的参数；
    
-   <可见性修饰符>：如 public、private、internal、external；
    
-   <状态可变性修饰符>：如 view、pure、payable；
    
-   <函数修饰符>：如 onlyOwner 等自定义逻辑控制；
    
-   virtual/override：用于支持继承与函数重写；
    
-   returns：定义返回值及其类型。
    

{{% /notice %}}

-   状态变量是指在合约中定义的、其值永久存储在区块链上的变量。它们用于记录合约的持久化数据，构成合约的整体状态。当合约被部署后，这些变量将被写入区块链，并在合约的整个生命周期中保持可访问性和可追踪性。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->






## **补充梳理：图灵完备与智能合约**

当朋友问起我什么是「以太坊」（Ethereum）时，我总会习惯性地通过其与「比特币」的对比来进行说明。但需要指出的是，这里的比特币并非简单地指「代币」（Token）——一种在去中心化的网络中用于给予维持网络运行的节点/网络服务提供商/「矿工」 的奖励/Gas Fee，如比特币、以太币等。

这里说到的「比特币」可以简单地说为比特币平台，即最早的去中心化分布式网络区块链平台。在此基础上，便可以较为简单快速地指出「以太坊」是一个去中心化、开源并且具备**智能合约**功能的公共区块链平台。

「比特币」和「以太坊」都是去中心化、开源的区块链平台，这意味着「以太坊」本身就继承了区块链技术所有的基本属性：**去中心化**、**不可篡改**、**安全透明**。不同在于后者基于**图灵完备**加入**智能合约**，使得原来智能记账的区块链可以运行各类程序和应用，而比特币平台上脚本语言是刻意被设计得非常有限——功能以转账和简单条件控制为主。

我们之前没有具体展开**图灵完备**（Turing-complete）和**智能合约**（Smart Contracts）的概念。

-   **图灵完备**的特性体现在一个计算机系统（编程语言或机器）能够计算任何「图灵机」能计算的问题。具体来说，**只要给这个系统足够的时间和内存，它便能执行任何逻辑，解决任何可以被计算的问题**。它需要能支持**逻辑判断**功能——如果发生 A，则执行 B，还有**循环**功能——重复执行某段代码，比如 for 循环，while 循环，或者直到满足某个条件才停止。
    

打个形象的比喻：图灵完备相当于一部电脑或手机，可以给它安装任何 app，它可以运算复杂的数学难题，运行游戏，只有有人写得出代码，它就能跑。但非图灵完备就像一部老式计算机，擅长加减乘除，十分稳定，但是无法在上面运行《愤怒的小鸟》这样复杂的游戏，也没办法写一个复杂的循环程序。

以太坊图灵完备的来源是其内置的虚拟机 EVM（以太坊虚拟机），EVM 带来的图灵完备使得以太坊不仅可以像比特币那样转账，还可以编写任意复杂的逻辑——**智能合约**。也正是基于智能合约，以太坊得以构建出 Dapps 应用、DeFi（事实上 DeFi 是 Dapps 的一种具体类型）、NFT 和 DAO。如同我在《[Web3 行业赛道全览](https://guozheng.rbind.io/project/web3-career-overview/)》中说到的，从以比特币为代表的区块链 1.0 迈向以以太坊为象征的区块链 2.0，区块链早已发展为一种涵盖金融、艺术、组织治理和社区文化的多元生态系统。

-   **智能合约**就是存储在区块链上，并可由节点执行的**程序或代码**。这意味着预设条件满足后，智能合约就可以自动执行相关逻辑完成任务进程，无需人工审批。
    

|   | 类比 | 与智能合约的关系 | 备注 |
| --- | --- | --- | --- |
| 智能合约 | 安卓/iOS 系统里的底层代码库/API | 1. 没有智能合约，就不存在后面提到的所有概念。2. 是一段自动执行的代码，规定了“如果发生A，就执行B”。3. 是无情的执行者，不带感情，只认代码逻辑。 | 编程代码、逻辑脚本。 连接以太坊/区块链网络与 Dapps、DeFi 这些应用层的桥梁 |
| Dapps | APP（应用程序） | 整体与后端的关系1. DApp = 前端界面 (网页/手机App) + 后端 (智能合约)。2. 智能合约通常是 DApp 的“后端服务器”。你在网页上点击“借款”，前端界面会把你这个请求发送给区块链上的智能合约去执行。3. 例子：Uniswap 是一个 DApp（你看得到的网站），它背后是一组运行在以太坊上的智能合约（你看不到的逻辑）。 | 使用了智能合约的软件——一种统称，包括 Uniswap |
| DeFi | 手机里的“金融类 App”文件夹 | 特定领域的应用集合1. DeFi 是 DApps 的一个子集，专门指那些处理金融业务（借贷、交易、保险、理财）的应用。2. 智能合约在这里充当了“银行家”和“柜员”的角色。3. 逻辑：传统金融靠银行经理审批贷款；DeFi 靠智能合约代码判断你的抵押物够不够，够了就自动放款。 | 金融向的 Dapps |
| NFT | 游戏里的“限量版皮肤”或“数字房产证” | 通过合约定义的一种数字资产标准1. NFT 本质上就是一个智能合约（通常遵循 ERC-721 或 ERC-1155 标准）。2. 这个合约里有一个账本，记录着：“ID号为 #8888 的图片属于小明”。3. 智能合约定义了 NFT 的属性（不可分割、独一无二）和流转规则（如何转账、版税如何自动支付给创作者）。 | 智能合约生成的独特商品 |
| DAO | 没有CEO的公司，或者小区的业主委员会，但规则是自动执行的 | 管理规则的代码化1. DAO 是一个组织，而智能合约是这个组织的“宪法”和“财务章程”。2. 传统公司：CEO 签字批准报销。 DAO：智能合约规定，“只要超过 51% 的成员投票同意，国库里的钱就自动转给某个项目”。3. 智能合约保证了治理结果的强制执行，没人能赖账。 | 用智能合约来管理的人群/公司 |

缺乏虚拟机、图灵完备和智能合约的比特币网络显然没有以太坊这么丰富的功能。事实上，比特币的脚本语言的图灵不完备是有意为之的设定，它不支持循环语句，也就不能写出一个“重复执行直到……”的死循环，但这反而确保了作为「数字黄金」的比特币的安全和稳定。试想一下，如果有人在比特币网络上发送一笔交易，而这笔交易包含了一个死循环代码，全网所有的矿工会在验证这笔交易时卡死，导致网络瘫痪。那么允许复杂逻辑和循环的以太坊又是如何防止死循环的呢？这便涉及以太坊的 Pos 共识机制，即在以太坊上运行每一行代码、每一步计算都需要消耗一定数量的 Gas，而且发起交易的人还必须预先支付一笔 ETH 作为 Gas，在这种情况下，一旦有人写了死循环，代码会开始运行，但 Gas 会消耗殆尽，而一旦 Gas 用完，矿工会强制停止执行代码，回滚状态并没收付出的 ETH。我们可以概括说，**图灵完备使得以太坊从原来作为单纯的「去中心化记账本」的比特币网络变成了可以运行各种程序的灵活的「操作系统」，而 Pos 共识机制则是防止该系统死机的「保险丝」**。

从中也可以看出，比特币以牺牲灵活性换取安全性和稳定性，专注于其**货币、价值存储和转移（转账）的价值**，强调**资产属性**。

由此，我们反推以太坊的价值——全球性的世界计算机，强调的是应用属性。这里的「全球性」不只是在世界各地都有节点，如同我们在《[如何理解区块链](https://guozheng.rbind.io/project/blockchain/#%e5%8c%ba%e5%9d%97%e9%93%be%e7%bd%91%e7%bb%9c)》中介绍的那样。事实上，除了遍布世界各地，还要求以太坊像一台**单一的、全球共享的**世界计算机那样。这意味着以太坊计算机不属于任何人，但任何人无论身处哪个国家，只要能连接上互联网，就可以部署自己的智能合约、调用别人的智能合约，并把自己的资产、身份、应用逻辑绑在这个全球共享的状态机上，同时它强抗审查、抗宕机，任何单一节点离线也不会让整个网络停摆。在这份世界计算机内记录着所有账户的余额、所有智能合约的存储变量、内部状态，还有所有已发生的交易与事件。在这些基础上还衍生出 DeFi、NFT 和 DAO。

## **01-18 分享会：Key Hash Based Tokens：从 ERC-721 到 ERC-7962**

-   今晚分享会的主讲人本人正是 ERC 的作者 Alex.Tian。
    

-   NFT 就是基于 ERC-721 的一个实践，在现实世界里通常表达为一个藏品——类似于文物、字画。
    
-   商业组织或者机构在发展社区的过程中，用户的标识在 Web3 世界里往往会以某个 ETH 地址体现。组织会部署 ERC-721 智能合约，通过 mint 给予用户身份。
    
-   ERC-7962 出现的现实需求背景：会员身份属于隐私数据，但是如果把 ZK 应用其中，就能够在不暴露隐私的情况下进行数据交易，在现实社会中非常有意义。
    
-   ERC-7962 的原理涉及「零知识证明」。
    

-   零知识证明：在不暴露具体细节的情况下，能够向第三方证明数据的某些特性。
    
    -   提供 proof（x，f），向第三方证明 x + y = z，但是proof（x，y）无法反推出 x 和 y 的具体值。比如，有两个整数值，第三方关注其和或者乘积是某个值，但是不能暴露具体值。
        
    -   关于「零知识证明」，可以参考李永乐的讲解《[神奇的零知识证明：既能保守秘密，又让别人相信你](%E3%80%90%E7%A5%9E%E5%A5%87%E7%9A%84%E9%9B%B6%E7%9F%A5%E8%AF%86%E8%AF%81%E6%98%8E%EF%BC%9A%E6%97%A2%E8%83%BD%E4%BF%9D%E5%AE%88%E7%A7%98%E5%AF%86%EF%BC%8C%E5%8F%88%E8%AE%A9%E5%88%AB%E4%BA%BA%E4%BF%A1%E4%BD%A0%E3%80%91%20https://www.bilibili.com/video/BV1iacBebE1E/?share_source=copy_web&vd_source=3f89400ae195feff9dd7d805682d7814)》。
        

-   ERC-7962 是在 ERC-721 的基础上应用 ZK 原理保证了向第三方证明其归属，但是不暴露其真实地址。（现实场景需求：会员身份）（例子：星巴克---机场）
    

-   ERC-7962 以 KeyHash （公钥哈希）而非地址作为资产所有权标识。谁发起都可以，这意味着所有权与交易发起地址解耦。用户只需要签名表达意图，无需持币、无需理解 Gas、无需管理地址隐私，也能完成资产操作——好处是什么呢？Web2.0 从用户可能难以理解 Web3 中的运行，比如需要单独的钱包应用，管理自己的公钥私钥，但我们希望能把他们带进来。
    

-   Web3 的 Gas 费用在现实中或许难以被外行人接收，ERC-7962 下的交易模型中的 relayer/paymaster 可以代付，但是代付 Gas 费在现实中不容易实现。
    

-   Alex.Tian 认为 AI 的表述有时比自己想的更加形象，推荐使用。似乎在其开发 ERC-7962 时，就与 AI 进行沟通。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->







## **1\. LXDAO 周会**

今天早上参加了 LXDAO 的周会，大致了解一下社区的运作模式和每周一会的内容。就社区的运作模式来说，和在实习手册中看到的描述相近，主要是由成员来提出提案，大家进行讨论和反馈，确认对社区有帮助便会支持一起做出来。在LXDAO 的 [notion](https://www.notion.so/lxdao/LXDAO-Dashboard-253dceffe40b80efadf0dab89a1e33a9) 上可以见到其 Dashboard，根据不同主题分出了许多板块，包括教育、资金可持续性还有研究与机制创新等，里面还清晰地罗列着 Not started、In progress、Pending 和 Done 等状态的项目任务。接任务的方式有两种，一种是在周会现场举手加入，然后联系发起人；另一种是在路线图中找到任务并以详细计划进行申请。

下面是一些我在此次周会上听到的提点。说起来，这种方式还是我在昨晚的例会上从学员 Swen 的分享中学习的——这种方式其实也有点像 ETHPanda 的中文周会分享的形式，我觉得很有意思。

-   LXDAO：可持续性、开源项目、无限循环、公共物品
    

-   周会上，Bruce 老师基于一份提案提出可以使用依托 AI 的设计网站 [Stitch](https://stitch.withgoogle.com/)
    
-   在 LXDAO 中，如何发起分配和激励？可以发起提案，然后看是否通过，FairSharing 用于公平分配
    
-   开源产品如何商业化（Bruce 老师的提案）？一种最简单的方式-GitHub上的捐赠。我可以提什么点子？什么样的工作流？
    
-   Marcus 老师（mrzzz011）正在进行两个项目，可以报名，现场几位发言加入 LXDAO 的学员马上举手加入项目
    
-   一个由 Bruce 老师发起的项目 Research on Public Goods history ，想了解一下
    
-   期待一下 Marcus 老师和 wachi 老师做的 Web3 游戏「良心杀」
    
-   在 Bruce 老师看来，以后编程可能不再是核心，以及长期使用 AI 和不使用 AI 的人在未来会有很大差别
    
-   当下的知识学习甚至不再需要机械式的阅读学习，善于利用 AI 更加重要。竞争力在于如何使用 AI，解决问题的思路？能力？
    
-   英语对于 Web3 的重要性：Web3 在国内做英语远程比较合适
    

## **2\. LXDAO 论坛**

我申请加入 LXDAO，并在论坛中创建了账号，发现上面有许多有意思的话题，除了一些社区的提案，还有一些关于去中心化、AI、开源 话题的探讨。Bruce 老师发起了一个「[关于 AI 对 开源项目 的可持续性的冲击、挑战和应对方法的一些思考](https://forum.lxdao.io/t/ai/3385)」的话题，讲到 AI 的 web coding 对开源项目本身以及开源项目的商业化方案\[^1\]都带来了冲击，并提出自己认可可能的解决方案，简单来说是给 AI 使用的开源项目进行加权，基于代码付出一定的 token 费用，类似于以太坊的 Gas Fee，手续费。我想这或许也算是用 Web3 的智慧来对 Web2.0 或者 AI 进行建设？但总的来说，LXDAO 应该不是只讨论 Web3 的平台，同时也关注着开源、AI 本身——这意味着关注的话题其实超越 Web3，是更广阔的，甚至不同内容相互交互。我喜欢这样的导向和氛围，希望以后有更多类似的话题。

\[^1\]: 事实上，今天 Bruce 老师还讲到了社区中关于开源项目商业化的方案项目，他说到目前最容易想到的一个方案是在开源项目中的捐赠。

## **3\. 与室友的交流**

最近在学习 Web3，为了检验自己的学习成果，我时不时会向室友进行介绍。室友本科念的会计学，接触区块链的概念比我更早，我不知是否缘由于此，他似乎比我对相关知识更加敏感，又或者说他总能想到一些问题。

-   第一次向室友介绍区块链的原理时，我说到由于每个区块上都有上一个区块的哈希值和本区块的哈希值，而哈希值是唯一的，一旦交易信息遭到篡改便会变动，每个节点上的网络服务提供商都会知晓，进而引起连锁反应，因而确保了交易的安全，不过这里安全的代价却是速度、效率上的缓慢。室友据此问我，**既然交易记录是要经过网络服务商的算力来进行算法打包，然后再形成哈希值，那么交易方如何确保自己的交易信息在还没形成哈希值前没有被篡改**？
    
-   和室友说到 Web3 「可以做什么」时，我想到起基于以太坊这样的区块链 2.0 去中心化平台的出现，Web3 从最初的主要用于加密账本的管理走向了**去中心化金融体系的形成和运作**、**DAO 社区组织的出现与治理**以及 **NFT** 这一系列内容形成的**体系**。去中心化金融体系这部分关联 Uniswap 等，但我只记得具体例子在 Bruce 回答学员提问 Web3 噱头很大，是只在金融有很大价值，还是真的有其他的价值时提过：在现有的 Web2.0 有着许多审查和种种限制下，带来一些提供额外、关键的数字设施，解决 Web2.0 无法解决的事情，比如做一些 Web3 的通讯软件，让有需要的人逐渐迁移过去，以及**解决一些信任和资金问题，例如应用需要支付，可以选择使用美元、法币，但这样可能让亚非拉地区的人很难使用服务，这种情况下 Web3 便很有价值。因为这些人由于信用问题很难创建相关银行账号，但使用 Web3 可以很容易使用到 USDC**。
    
-   提及 Web3 易涉嫌违法犯罪，我举出最简单的例子，即用人民币购买虚拟币后再兑换成美元，这会使得人民币的流通没有经过国家的银行，而「出金」则是由于难以确保用代币换来的人民币是否涉及违法犯罪的资金源而风险重重，舍友便问我，那如果是用代币换成美元，再用美元走国家的银行体系兑换成人民币呢？我就没有想过这样的问题，或者说如此发散。
    
-   最后和舍友说到 DAO 组织具有让大众共同参与到传统上只能由少数人决定的重大事项中，而且社区的运行是让大家共同提出意见，共同建设，没有领导，没有复杂的公司结构，是去中心化的。舍友便提到古希腊时期的城邦民主——是否会出现「陶片放逐」的情况？我想了一下回答了他，但不知自己的思考是否合理：第一，在我了解的内容范围中，DAO 组织有许多具体的类型，例如在国内的 LXDAO 关注开源、AI、公共项目等，国外有 Nouns DAO、ConstitutionDAO，而这些 DAO 具体关注的事情又不一样，所以具体的 DAO 应该分类型，因而有着形形色色的 DAO，而不同类型的 DAO 规模应当都不大，因而不会出现过于分散的民主；第二，能进入 DAO 的人都需要有基本的条件，例如基本的互联网技术知识、对 DAO 的相关理念认同、对技术价值有一定的认识等等，这些决定了 DAO 的成员应该不会像雅典城邦的公民那样出现「暴民政治」。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->








今晚的 Co-learning 分享感觉简直是「八仙过海，各显神通」，无论是运营向还是技术向的同学都很有自己的想法，听完以后我只感到震惊。好吧，有种前两天好不容易才建立的「心理防线」又被击溃的感觉，但在分享会上我感觉学习到的内容远远不止是 Web3 领域，还有大家是怎么理解和处理知识的，相比而言，我觉得后者远比前者重要。

## **Howard**

我来晚了一些，没能听完 Howard 的完整分享，但是在 Howard 的分享里听出他很有激情，开了一个好头。

## **Vincent**

Vincent 借助 Figma 思维导图软件来根据实习手册来形成自己对 Web3 的理解。

讲的内容简洁，但是很精炼，也很清晰。

## **Swen**

Swen 自述经济学出身，辞职了一份很稳定的工作，追求古朴简约。他的叙述很平静，感觉很稳。

Swen 的记录似乎也很有意思，在看了相关的采访视频后，以一个一个提点的方式来分享自己的感想，有点像是以太坊中文周会的形式。

我觉得这种形式也很有意思，相对于我过去喜欢「复现」学习内容的笔记记录方式，这种形式带来了一种新的想象：分散的、更突出和强调个人的所思所想。

## **周星星**

周星星分享的是自己的心路，走心局——按照自己的节奏来走吧，走下去。

## **Max**

Max 分享的是技术向的内容，分享内容里代码扑面而来，看起来十分硬核。主要介绍了 Uniswap。在 GitHub 里，Uniswap 有两个仓库，一个面向用户。

在 Max 的代码分享中，看起来 Web3 技术向对于区块链中的交易原理理解要求会比较高。

Max 对代码的解释很熟练、很具体也很稳，在ETHPanda 说到这些是技术进阶的内容。不过我基本是一头雾水，佩服 Max。

## **奕铭**

奕铭讲的内容是运营向，分享的是学习的过程，关于如何输入和输出知识。这给我的启示还是要进行费曼学习法，但其实费曼学习法不一定要面向他人，也可以面向 AI，或者说以能让别人理解为目标来做笔记。

我可以试着将一周的内容总结一下，然后再到 X 上分布。

舍弃完美主义，选择迭代思维。

## **Karen**

Karen 是中文系的研究生，分享的内容是自己起号的经验，例如先观察 DAN KOE 的推特文章特点——标题党，配图吸引人；还要厚脸皮，例如可以多点引用别人的内容、做自己的水军。

当然，还可以在 X 上表演，将自己推销出去。

我感觉 Karen 将自己的中文系写作水平在 X 的营销中发挥出来了，但是她本身也有着很强的观察能力，例如刷了两个多月的推特，从兴趣入手，观察、总结别人在写什么，怎么写，之后再输出。

Karen 还分享了具体的写作方法，如果要高效整理自己的思考，可以从写是什么，到为什么，再到怎么做；而如果要把文章写得有故事性，可以从起因、经过、结果等简单因素出发。

## **小白**

小白分享 solidity 语法的学习，看起来 solidity 和 python 相近。

小白最后说的找到适合自己做的很有意思，做某件事情如果无法乐在其中，可以考虑换一个赛道。例如如果只是为了流量而做流量，那也是很难走远。小白说得很朴素，但不知为什么就很打动我，能感觉到他的实诚。当然，小白自身的生活习惯也很惊人，早上五点起床跑个五公里，然后七点开始学习，中午睡个午觉后，下午一点多学习到五点，然后吃晚饭……当然，我不会模仿小白，毕竟每个人的习惯、方式和节奏都不一样。看到大家都那么优秀，我内心很是动摇，但小白的分享却又让我感到想明白自己的赛道，或者说做自己乐意和愿意坚持下去的事情很重要。

## huahua

这才第一周，huahua 直接做了一个 MPC 钱包的 demo，并且部署好了在网站上，我直接被震惊。

## **Alex Fan**

Alex Fan 具有法律的背景，从法律的角度来跟大家介绍对 DAO 的理解。

## **Mike**

Mike 分享的是 AI Agent 和 Web3 的关系。

## **Leon**

Leon 也是通过思维导图的方式来分享自己对 Web3 的学习。如果只是读笔记，其实还是很机械的学习方式，但是通过思维导图的打开和关闭过程，可以以此来进行检测自己。

学习不要只是局限在笔记里。可以扩展开来。做笔记的目标不是仅仅复现，而是能做到吸收知识。

## **ziv ziv**

由于时间比较晚了，Ziv Ziv 便主要基于自己的想法对 Bruce 的问题进行了思考了尝试性的回应。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->









## **1\. 今日思绪 - 有些气馁但也会坚持**

刚好今天学习的内容是对 Web3 行业进行一览，我想也可以梳理一下今天的感受：学习 Web3 不一定就意味着必须进入其中的技术领域，甚至不一定必须进入 Web3 这一行。Co-learning 时几位助教和老师提出的建议很有建设性，运营和技术都大可试一试。好吧，事实上，我所想的是自己基于他者的建议而产生的「偏见」：

我也不知道是不是自己不够坚定，或者不够勇敢坚韧。基于这 4 天的自我观察和状态感受，我认为自己恐怕很难进入 Web3 行业，不论是因为自身的技术栈很匮乏——Web3 的技术岗要求比较高，如果是纯粹小白，很难凭借一次实习计划就找到相关岗位，需要不断深入该领域，还是 Web3 的安全和合规问题让我感觉自己难以处理，又或者是在几天高密度的学习中我发现有些力不从心，当真慢于许多学员的节奏——许多时候看着大家兴致勃勃地在 X、小红书或者群聊中输出运营帖子、学习心得，以及讨论各种问题，我都感觉自己难以介入，也……似乎没有太多介入的欲望。

但也正是这么几天的学习，我能感受到 ETHPanda 和 LXDAO 发起这次活动的诚意，致力于让外行内行都能全面了解 Web3 行业，从基本的概念、理念、行业状态、安全问题、合规问题、落地实践等等等等……我更一步感受到 Bruce 老师说的他们这些 Web3 中的 Regen 希望能给致力于探索 Web3 的可持续发展，包括相关项目的落地，以及为未来的数字世界探索新的机制。我相信来到此次实习计划，只要踏踏实实地学习绝对不会空手而归。不知道 ETHPanda 和 LXDAO 的期望是什么？如果说希望找到能参与 Web3 建设的人才，我感觉作为入选的 1/500 会不会有些浪费这些资源？唉。

不过我还是那句话，我不会因为自己不够坚定 Web3 而在此次实习计划中退缩。这种坚定在我看来其实指向对于学习的高度热衷、热情（有许多问题可提，频繁地参与讨论），而且确实能表现出一些不同（比如能主动贡献些学习输出或者 pr），或者在社群中能积极地活动（和学员或老师们有来有回地互动）。但我发现我好像做不到以上这些，我好像有些像中小学生一样，阅读、记录、以及产生一些微小的想法，只是做完这些都感觉花费了许多时间和精力。我会从头走到尾，善始善终，哪怕没能进入 Web3，也会开心于学习到了一些很棒的理念。

比如在今天的 Co-Learning 上，助教建议大家运营和技术方向都可以试试。以运营为例，**先做着**，可以在 X 或者小红书上起号，分享自己每天在 Web3 上做的笔记，学了什么，听了什么，以自己的话总结出来。如果想获得足够的流量，还可以在帖子的最后贴上实习计划的 tag。以后去求职 Web3 的运营岗也好，又或者是其他领域的运营岗，个人在相关平台上的粉丝量都可以作为最基本的反映指标。

我思考了一下，刚加入实习计划时自己只有钻研技术岗的想法，但现在开始改变了想法，其实每种方式都可以试一试。有时候重要的不是眼前具体的某一个对象，而是这件事情背后可以产生怎样的思维。我在此次 Web3 实习计划尝试学习运营，如何不算探索其他领域时要做的？想一想过去社会学、人类学专业的学生投递较多的产品经历、用户研究等岗位，这些不也与运营有关吗？总之，多试一试，总归没错。

此外，wachi 老师关于工作和知识学习的分享对我的触动也不小。

在工作方面，wachi 老师本是 Web2.0 技术背景出身，学生时代还是保送到上交大的计算机专业，后来转入了 Web3，跟着兴趣一直走到今天。对 wachi 老师，似乎不分前端或者后端，更多时候称呼自己为开发者——自己是足够优秀的，哪里需要自己，自己都可以去 handle，有需要的就去学，从不将自己限制在某个位置，也带着致力于把事情解决好的理念。这让我想到的有些模糊，但与我未来的规划有关，我想过自己可能很难走进 Web3 的行业，但或许 Web2.0 还是可能的，那么我是否也可以将自己视为拥有更多可能的人？学好 Web3 就不可以进入 Web2.0 了吗？学不好 Web3 就不可以进入 Web2.0 了吗？这些不重要，重要的是学了 Web3 也很开心，同时不会妨碍我以后要走向 Web2.0。另外，有时候不要预设太多未来工作的可能和状态，因为这大多数时候都是动态的——人与人之间的相处状态是变化着的，包括我们与任职单位中上级的关系，以及个人所在团队的氛围等等，具体来说，当我们老是忧虑于自己能不能满足他者的期望时，也需要知道不同的标准有所不同，ta 可能会给予你更多容错率，但 TA 却无法容忍而不愿意给予你成长时间，如此等等。从这一点也延伸出找工作其实也是双向选择的过程，我们需要有一定的配得感。不过还有一点很关键——**先做事，后做人**。

关于知识学习，每天群聊都有海量 Web3 信息涌入，实习计划提供的资料也太多太多，每天也还有各方面的知识分享，应该怎么处理它们？怎么取舍？怎么提炼？其实这可以被理解为上路开车，刚开始会觉得很紧张，哪里都是重要信息，但成为老司机后便信手拈来了。总的来说，持续地让这些概念向我们奔涌而来，每天都在看，然后感受它们的联系，到后来自然就明白了，知道之后要怎么查询，怎么理解，形成自己的记忆和理解网络。我很期待，实习计划结束以后我对 Web3 的感受会是怎样的呢？

##   
2\. **DeFi: Decentralized Finance，去中心化金融**

我们昨天已经知道，Web3 天然具备金融属性，具体来说，区块链技术早期也被认为是一种**去中心化**的**分布式**记账方式，它催生出了维系区块链网络运行和维持的虚拟代币——比特币。而比特币作为区块链 1.0 的代表专注于其货币属性和价值，因其稀缺性（上限为 2100 万枚）、稳定性-安全性（全球账本）和高自由流动性（跨时间跨区域）而成为了重要的支付手段。

基于上述，我们可以讲区块链技术总结为一种加密货币支付系统。但实际上区块链技术不止于此，它启示已经发展为一种涵盖**（1）金融、（2）艺术、（3）组织治理和（4）社区文化**的多元生态系统，具体可分为 **（1）DeFi、（2）NFT、（3）DAO 和 （4）MEME**。

DeFi 是一种基于区块链技术建立的**金融体系**，致力于提供不依赖传统银行或金融中介的服务，包括借贷、交易和支付，实现人人都可以自由参与、无审批、无信任中介机构的目的。当然，既然前面说到这是一种体系，那么应当是包含多种内容，具体来说包含 **Uniswap**、**Compound** 和 **MakerDAO（Sky）**。

### **2.1 Uniswap**

-   Uniswap 是去中心化交易所（DEX）的代表
    
-   流动性池是 Uniswap 交易的核心——很多用户共同提供的资金池。
    
-   不再需要通过一个中心化的交易所来等待配对和匹配订单，只需加入流动性池，Uniswap 即可根据池子资金自动帮助完成交易。
    
-   中心化交易所：币安、火币
    

| 特点 | 去中心化 | 中心化 |
| --- | --- | --- |
| 交易不需要中介，快速方便 | 直接用自己的钱包进行交易，不需要像中心化交易所那样注册账户、上传身份证等繁琐步骤。 | 需要通过一个平台来进行配对和交易，往往存在人工审核、系统延迟等问题，交易可能比较慢。 |
| 24/7 全天候交易 | 无论身在何处，任何时候都能进行交易。没有交易时间的限制。 | 通常会受到全球市场交易时间、服务器维护等因素的影响。尤其是某些交易所还可能进行系统升级或停机。 |
| 大额交易更便捷 | 不需要等待层层审批，特别是对于一些大额资金的交易，完全依赖智能合约和流动性池来完成。所有交易都直接在链上进行，快速而高效。 | 大额资金的交易通常需要通过银行、金融机构等审批环节，可能还要经过反洗钱审查等复杂的程序，时间较长。 |

## **2.2 Compuond：去中心化借贷协议**

-   去中心化的借贷平台
    
-   允许借入或借出加密资产
    
-   利用**智能合约**和**去中心化金融（DeFi）**理念彻底改变传统借贷模式
    
-   存入资产可获得相应c代币（cToken），可依次代币赚取利息
    
    -   将资产 ETH 存入 Compound 便能获得 cETH 代币
        

### **2.3 MakerDAO（更名 Sky）：稳定币系统**

-   去中心化的借贷协议，更名为 Sky
    
-   用户可据此**超额抵押资产来生成 DAI**
    
    -   DAI 是一种与美元挂钩的稳定币，价值稳定在 1 美元左右，因此可以作为避险工具或支付手段
        
    -   具体：将 ETH 这类加密货币资产抵押到系统，通过智能合约生成与美元挂钩的 DAI 稳定币，但需要超额抵押资产——2 美元价值的资产抵押才可以生成 1美元的 DAI
        
-   DAI 稳定币已升级为 USDS（Sky Dollar），MKR 治理代币升级为 SKY
    

## **3\. NFT: 非同质化代币**

NFT 的全称是 **Non-Fungible Token**，中文直译是「**非同质化代币**」。这名称看起来有些让人费解，但拆开来看，Non-Fungible 是指非同质化的、不可替代的，Token 则是指代币或凭证，又或者说是区块链上的一个记录单位。可以先将 NFT 简单理解为独一无二、完全不同，难以用来平等交换的代币，而其对应的 NT 即同质化代币则是可以被任意交换的代币，像钱一样。

传统上网络中的图片、视频和音频等由于可以很简易地被复制、保存和传播，因而所有权很模糊——你手中的数据文件和我手中的数据文件没有什么差异，它们是同质化的，可代替的，但依托于区块链技术——本质是一种去中心化、公开透明的账本，因此所有交易和所有权信息都被记录在上面——的 NFT 相当于给这些数据文件以唯一的凭证，**每个 NFT 都代表一个唯一的数字资产**，且在区块链上有不可篡改的记录，它使得数据文件具备了唯一性、稀缺性和明确的所有权，从而可以被买卖和收藏。

### **3.1 特性**

-   独一无二，不可分割，无法等价交换。
    
-   不可篡改，公开透明。NFT 基于区块链技术，任何人都可以查看且不可篡改。
    
-   万物皆可 NFT。NFT 可以在虚拟世界代表任意非钱币的实体。
    
-   完全控制权。NFT 所有者对其拥有永久、完全的控制权，不可被剥夺。不过大部分数字藏品并不算真正的 NFT，因为不在公链上，还可以被平台控制。
    
-   确权成本低。验证某人是否拥有某个 NFT 的成本非常低，可快速确认版权归属。
    
-   无限想象空间。NFT 的具体功能和玩法由参与者定义。
    

### **3.2 在未来世界的应用**

|   | 现状 | 未来 |
| --- | --- | --- |
| 游戏 | 游戏里的虚拟资产仅限于当前游戏，无法在游戏之间流通，且受限于游戏平台规则，所以玩家很难进行自由买卖。另外游戏账号也并非被你拥有，一旦被封号你将失去所有游戏资产。 | 结合区块链和 NFT 技术，开发者可以设计出具有唯一性的游戏虚拟资产，且完全归属于个人，例如账号、武器、工具、房屋、宠物等，玩家可以自由的进行游戏资产的交易和流通。 |
| 版权确认 | 被盗版之后，很难确权，需要准备大量文件和记录。导致小规模盗版事件频发也无可奈何。 | NFT 并不能解决盗版问题，但是相比传统的盗版艰难的反侵权诉讼而言，仅需要几秒即可确定你是否拥有版权。也非常容易确认谁最先创造了这个物品。未来正规商业公司在采用素材时，非常容易确认谁是所有者，提升商业合作效率降低风险和盗版事情的发生。 |
| 买卖房产 | 买卖房产需要聘请律师，通过政府信用背书的房产证等资料来确认房产所有者，甚至需要由政府档案处管理资料。这要付出时间、金钱、和一个可信任的第三方才能完成交易。 | 如果未来 NFT 得到了法律认可，代表了你的房产，那么只需要几分钟转移 NFT 即可完成房产所有权交割。 |
| 艺术品收藏 | 艺术品通常为实物，需要妥善保管避免被盗和损坏。展示时需要较大成本来运输和做安保工作。 | 艺术品为虚拟物品，基于 NFT 技术确保艺术品的安全性。在钱包安全的情况下，无需担心艺术品被盗火者损坏。在元宇宙中可以任意展示你的藏品或者举办艺术展。想要提前体验一下元宇宙 NFT 艺术展？可以访问 https://oncyber.io/6529om |
| 交换物品 | 交易和交换的信任成本很高，往往需要引入第三方可信平台作为担保执行。需要一定的时间和金钱成本。 | 通过区块链和 NFT 技术，可以很轻松的查看到过去的交易记录来确定物品的来源和真实性。通过智能合约等技术，可以直接实现交易和交换。减少诈骗、交易纠纷和信任成本。 |
| 通行证 | 不同的系统的门票、会员卡、员工卡、通行证不互通无法相互使用，每个系统需要自己准备系统门票和会员卡等。 | 所有系统门票、会员卡、员工卡、通行证均通过 NFT 技术实现。每个人的身份与其钱包绑定，不同的系统可以非常快速准确的识别你的身份，并为你提供定制化的服务。 |

由于 NFT 所代表的是虚拟网络的物品，比较抽象。因此目前比较常见的火爆的 NFT 项目是一些艺术作品，包括头像、画作、3D 艺术品、照片等等，也开始逐步出现更多 NFT 的玩法和项目，比如社区通行证、游戏道具、门票等。

在了解了 NFT 的含义后，我终于可以到 [My First NFT](https://nft.myfirst.io/) mint（铸造）自己的第一个 NFT 了。这一项任务在第一天就有了，许多学员第一天就完成了，我直到今天才开始——发现需要会创建钱包、获取助记词、转换网络等的知识。还记得第一天自己感叹看不懂「mint」、「挖矿」这些术语，现在也都逐一了解，还完成了任务。虽然进度可能还是很慢，不过继续坚持下去吧！

![my-mfnft.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/residualsun1/images/2026-01-15-1768486425824-my-mfnft.png)

## **4\. DAO: 去中心化组织**

DAO 的全称是 Decentralized Autonomous Organization，意为去中心化组织，它不依赖传统的公司架构，不需要老板/管理层，而是一群志同道合的人通过**智能合约**和**社区投票**来做决策，所有事情都由持有代币的**社区成员共同治理**，公开透明——没有什么事情可以瞒着大家，尤其是利益分配相关的问题。

这次实习计划的共创方 LXDAO 则是话语社区比较活跃的 DAO，之前提到的 Bruce 老师是 LXDAO 的发起人。

### **4.1 Nouns DAO**

Nouns DAO 可以被视为艺术和区块链的结合体，每天都会生成一个独一无二的 NFT——一个卡通小人头像。这些头像会通过拍卖出售，所有的收益则会被收入 DAO 的金库。有意思的是，持有 Nouns NFT 的人除了是收藏者，还可以参与组织的决策——例如大家一起投票决定将这些资金用于做什么事情，完全**依靠社区的力量**。

### **4.2 LXDAO**

LXDAO 则是一个特别专注 Web3 公共物品的项目，通过支持开源项目、资助创新项目，LXDAO 推动 Web3 作为一种**更自由、更去中心化**的互联网形态的理念实际落地。在 LXDAO 中，大家根据彼此的兴趣特长来承担和完成不同的任务，完成任务后得到相应的奖励，这也是根据区块链和智能合约实现。总的来说，LXDAO 中的成员通过各自的贡献来建设一个去中心化的未来。

### **4.3 ConstitutionDAO**

ConstitutionDAO 打算集合人们的力量，齐心协力买下一份《美国宪法》的原稿——目标是让普通人有机会共同参与购买，而不是由传统的博物馆或私人收藏家垄断。虽然最终没能成功竞标宪法副本，但让《美国宪法》被拍卖了出来，这展现了 DAO 的潜力：**让大众共同参与到传统上只能由少数人决定的重大事项之中**。可以说，ConstitutionDAO 的尝试为 DAO 模式开辟了新天地，也为未来的去中心化集体行动提供了宝贵经验，并让人们对 DAO 产生了新的认识：DAO 不再只是「虚拟货币」或「小众项目」的代名词，甚至有可能成为打破传统精英控制、让大众参与重大历史事件的全新方式。

##   
5\. **MEME: 模因币**

我没想到 MEME 也会和 Web3 有关。

**MEME**

-   在网络文化中指「迷因」，梗、表情包和搞笑段子等
    
-   在加密圈中，指以网络文化为基础的代币
    
    -   DOGE（狗狗币）、PEPE（青蛙币）、SHIB（柴犬币）
        
-   MEME 币
    
    -   特点：有趣，搞怪、社区驱动；**极高投机性和波动性**（不建议碰）
        
    -   缺乏明确技术价值，却通过强大的社区共识和文化认同获得价值
        

## **6\. 夜间分享会：AI 及其基础概念**

AI 与 Web3 的结合—— AI 赚钱？

传统 LLM 没有记忆，没有工具，只能生成一些文本，不能调用 API。Agentic AI ，代理式 AI，可以推理，也有记忆，还可以使用工具，自主执行任务。

今晚的分享会我在犯困（我的问题(;´༎ຶД༎ຶ\`)）……但下面这张图让我印象很深刻。可能是因为我这段时间在接触 AI Agent，也听说过 LLM、RAG和 API，但不知道它们之间存在关系，比如共同构成 AI Agent？这种接触过局部知识而后在某个时刻看到它们再作为整体的一部分而出现时，好像会感觉对它们的认识变得更具体、系统、深刻了？

![01-15-2.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/residualsun1/images/2026-01-15-1768486537376-01-15-2.png)
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->










## 1\. 今日心路

我的进度好像慢了一点，但今晚还有关于安全会的分享，今天就从这一章开始。

在了解「安全与合规」这一部分以前，还是需要先说明 Web3 虽然颇有争议，但随着对其认识的深入，其本身倡议的价值理念或许还是值得思考一下……我还是坚持自己的立场，反对以 Degen 这投机炒币派，而支持探索未来数字世界新机制和 Web3 可持续发展与落地的 Regen。

但今晚听完邓律师的分享后，突然觉得 Web3 行业离我更远了。但是我还是会坚持把实习计划本身完成。

## **2\. Web3 的核心法律风险**

相关核心法律风险可以分为四个方面，我按照自己的熟悉感来重新排列了一次：

### **2.1 「虚拟货币兑换」或涉及场外交易中的非法经营与洗钱风险**

**虚拟货币在场外交易环节常被用作规避监管的「地下换汇」工具**。我们前两天在认识区块链和以太坊的过程中认识了代币——代币有许多种，目前我认识的就有比特币和以太币两种。境内人可以使用人民币购买这些虚拟货币，然后再通过链上或者境外平台来兑换美元、欧元等外币，从而形成了**跨境资金流转链条**。该过程**不仅绕过外汇监管，还使得虚拟货币成为规避外汇管制的「桥梁资产」**。根据我国《外汇管理条例》和刑法，未经许可反复组织撮合人民币与外币的虚拟币交易，可能构成非法经营外汇业务。

> 写到这里会让我想到虚拟货币是否只受区块链网络中的一群人和他们构成的交易市场「管控」？类似人民币、美元、欧元这样一些货币是国家认可的，可以受到国家管控，但虚拟货币却不符合许多国家的法律。在这种情况下就想起 Bruce 留下的一个问题：去中心化协作下，如何实现公平、可信的分配?我进而还想知道，感觉虚拟货币的存在，由于其去中心化的特性，因而会与现行货币和金融秩序相冲突，至少在公开、法制环境下，虚拟货币无法和现行货币共存。

此外，**如果交易方或资金来源与电信诈骗、赌博、毒品交易等犯罪活动相关，参与者极可能被控洗钱罪掩饰隐瞒犯罪所得罪，甚至在不知情的情况下被卷入“协助洗钱”的刑事链条**。因此，在参与虚拟币兑换时必须谨慎，对交易对手的信息、背景、资金来源进行审核，避免成为非法资金链条中的一环。

### **2.2 代币发行与交易行为的法律风险**

国内法律禁止任何单位或个人通过 ICO、IEO、IDO 来进行融资，不论代币是否命名为积分、凭证或治理 token，只要具备融资功能或可流通性，便可能构成非法金融行为。

值得注意的是，技术人员如果参与**代币模型设计、空投逻辑配置、合约部署**等环节，也会被视为「共同行为人」，无法以「只是写代码」免责。

### **2.3 赌博、传销、洗钱**

Web3 项目的设计与经济激励模型直接影响风险等级。

部分链游存在“充值—抽奖—提现”的闭环结构，玩家投入法币或 USDT 购买盲盒、转盘机会，随机获得稀有 NFT 或 Token，后续可在平台提现或转售。这种“下注—随机收益—兑现”的机制容易被认定为开设赌场罪。

NFT 项目、DAO 社群、挖矿平台中常见的“邀请返利”“算力挂靠”“多级推广”模式，一旦涉及团队计酬、多层级返佣，可能触犯组织、领导传销活动罪。

### **2.4 民商事争议**

虚拟货币买卖、委托他人投资在 Web3 中是常见的交易形式，但我国目前的政策态度和司法实践对此并不认可，认定为无效，风险需要自担。

## **3\. Web3 的入职法律风险**

### **3.1 新型雇佣关系形态**

-   分布式办公模式
    
    -   普遍采用境外注册方式，并在境外设立控股实体
        
    -   通过 Telegram、Zoom 等方式进行线上面试与协作
        
    -   Web3 项目方在国内无注册公司，不具备用工主体资格，因而**无法签订有效《劳动合同》，也无法缴纳五险一金**。一旦发生薪资争议或因公受伤，难以根据《劳动合同法》享受合法保障。
        

> 我国社会保障体系与诸多生活事项紧密挂钩，包括落户、购房、贷款、子女教育、婚育福利等，缺失社保公积金的雇佣结构将直接影响个人生活。即使签署了书面合同，也可能因主体资格缺失被认定为无效，成为“白纸黑字”。

工作方式、生活方式的取舍：灵活用工还是保障？

### **3.2 薪酬结构**

-   「人民币 + Token」或「全 USDT」模式
    
-   《劳动法》：工资应以法定货币（即人民币）支付，不得以实物或虚拟币等形式代替。
    
    -   如果薪酬中 Token 或 USDT 部分**被认定为工资支付**，可能导致支付行为无效。如果该部分**未被明确纳入工资范围**，则会拉低员工的平均工资基数，影响其社保缴纳、经济补偿计算等，间接损害劳动者合法权益。
        

### **3.3 虚拟货币出金与合规风险**

-   **出金**：将虚拟货币兑换为人民币来满足日常支出的行为
    

-   出金主流方式：C2C 交易，包括场内挂单、场外担当保交易、OTC 交易群
    
-   出金风险：
    
    -   若交易对手使用涉赌、涉诈资金购币，收款方可能因接收非法资金而遭遇银行卡冻结，甚至要退赔全部涉案金额
        
    -   参与高价出 U 交易、协助他人洗钱、绕开监管等行为，还可能构成「帮信罪」或「掩饰、隐瞒犯罪所得罪」
        
-   建议
    
    -   与公司协商薪资发放方式，若日常需要法币支出，保留一部分法币收入
        
    -   通过可信渠道出金，避免灰色交易
        
    -   定期留存相关资金合法来源记录，以证清白
        
    

### **3.4 项目合法性审查**

-   入职前必须审查 Web3 项目是否合法
    
-   Web3 整体项目若涉嫌非法金融活动也会牵连员工受到调查
    
-   查阅项目白皮书、Token 分发机制、收益模型、是否面向中国大陆用户、是否含有返利结构、投资承诺等
    

## **4\. Web3 项目风险**

-   开设赌场罪、赌博罪
    
-   非法经营罪
    
    -   涉及通过虚拟货币作为媒介工具实现人民币和外币的兑换
        
    -   利用虚拟货币进行外汇买卖的非法经营活动
        
    -   利用虚拟货币进行支付结算的非法经营活动
        
-   非法吸收公众存款罪
    
    -   诱使公众支付资金并提供“挖矿”服务
        
    -   直接吸收公众手中所拥的虚拟货币
        
-   组织、领导传销活动罪
    
-   洗钱罪
    
-   虚拟货币风险
    
    -   国内整体监管对虚拟货币为禁止态度
        
-   代币发行风险
    
    -   法规规定，ICO（首次代币发行）是未经批准非法公开融资的行为
        
    -   代币不具有货币属性
        
    -   各类代币交易所的兑换、买卖、定价和信息中介服务被禁止
        
-   挖矿风险
    
    -   虚拟货币挖矿行为被法律定义为高能耗、高排放和低贡献的淘汰类产业
        
    -   虚拟货币相关全部业务活动均属于非法金融活动
        

## **5\. 网络安全风险与攻击方式**

### **5.1 钓鱼攻击**

-   攻击者通过**伪造官方网站、社交账号、邮件、短信等，诱导受害者点击恶意链接，输入敏感信息（助记词、私钥、账号密码），或下载恶意软件**。
    
    -   伪造邮件/网站
        
        -   仿冒知名企业、学校等的邮箱或网站发送面试通知、账号异常、奖学金通知等诱导点击钓鱼链接
            
    -   社交平台钓鱼
        
        -   在微信群、qq群、telegram等社群中冒充官方人员、HR、学长学姐发布虚假招聘、空投、福利活动
            
    -   假冒好友/客服
        
        -   通过盗号或伪造头像等方式冒充信任的人，诱导转账或泄露信息
            

### **5.2 恶意软件/木马**

-   伪装成面试/学习软件，诱导下载安装
    
-   剪贴板劫持
    
    -   木马常驻后台，监控剪贴板内容，检测到钱包地址即自动替换为攻击者地址
        
-   浏览器扩展/插件后门
    
    -   伪装成热门插件，窃取浏览器中的敏感数据
        
-   远程控制
    
    -   木马获取系统权限后，可以远程操控电脑，窃取文件、录频和键盘记录
        

### **5.3 社交工程攻击**

冒充信任的人通过社交平台、邮件、电话等方式进行诱导

### **5.4 供应链/第三方依赖攻击**

-   恶意浏览器插件/扩展-在商店平台中得到上传，诱导用户安装
    
-   开源库后门-攻击者在开源库、依赖包中植入恶意代码
    
-   官方渠道被劫持-正规商店的软件也可能被感染
    

### **5.5 地址污染与扫描木马**

-   剪贴板劫持
    
    -   木马监控剪贴板，检测到钱包地址即自动替换为攻击者地址
        
-   输入框监听
    
    -   恶意软件监听浏览器或输入法，实时获取输入内容
        

### **5.6 传统隐私与账号风险**

-   弱密码及其复用：多个平台使用相同秘密
    
-   邮箱/SIM 卡劫持：通过运营商漏洞等手段劫持邮箱或手机号，重置所有账号密码
    
-   双因素认证（2FA）缺失
    

## **6\. Co-Learning 讨论**

wachi：

Web3 对技术的要求：

1、写文档——AI也能看懂的

2、写demo

3、能经营好开发者关系——把开发者社群建设起来

## **7\. 夜晚分享会**

### **7.1 Web 安全**

2025 年 Web3 领域发生了超过 1200 起较严重的安全事件，总损失金额超过 35 亿美元

-   精准猎杀
    
-   广撒网
    

Web3 天生具有金融属性。

不要随便点进链接，别人的链接可能总有风险，要有独立验证的习惯。这是 UXLINK 的案例教训。

EIP-7702 带来了新技术，但骗局也更新了，没有天上掉馅饼。

AI-Coding 时代，许多代码能力委托给了 AI。这个过程很容易引入恶意的代码或插件——这是一种软件供应链攻击。Trust Wallet 浏览器钱包 2.68.0 版本就被植入了恶意后门，波及大量用户，损失七百万美元。这里的启示是在开发过程中，一定要进行人工review，不要全程委托给 AI 或 IDE。

建议「不点、不签、不装、不转」

安装安全插件，警惕钓鱼链接

妥善保管私钥/助记词，冷热钱包分离

GoPlus 有相关网站可以检测风险。

### **7.2 Web3 合规**

入职 Web3 咨询相关律师十分重要……

曼昆（深圳）律师事务所的邓小宇律师进行了今晚的分享，最后的 Q&A 环节涉及了许多付费内容，期间律师似乎有些为难，但是看在 ETHPanda 的面子上还是讲了。

中国内地三个负面清单：ICO公开融资、虚拟货币交易所、虚拟货币挖矿

公安沟通方式——南方一般温和一些，北方的可能强硬一些。例如除新疆、内蒙的公安，其他地区的公安一般会提前让准备好证明材料，证明自己不知资金上流的来源，还有商量余地

随着国内 Web3 用户合规意识的增强，海外发起入金攻击。

背调清楚是否拿到了牌照——如果只是拿到美国MSB牌照还不够；

是否向大陆展业？

不用翻墙的情况下能否登录上交易所？

在大陆是否能下载这些交易所的app？

**合规风险点**：Web3 行业处处都是风险，哪怕只是玩玩钱包，没去去中心化的交易所都有风险。具体到工作开展，工作赚到钱出入金都有风险。额外受到莫名其妙的钱也要尽快向当局反映。

合规和风控意识是动态的，待得越久才越能意识到哪里有问题。

不过大多数人都是简简单单地玩，简简单单地赚到一些钱。

总的来说都是人心。

出金的合规方式——很难。真能找到合规的方式，这也相当于一个难得的项目了。

邓律举了好多法务实例，突然觉得法律知识对以后的社会生活是多么重要……
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->











今日思绪：或许是因为早上将最后一份课程论文完成，我好像多出了不少干劲，中午睡了个午觉，下午全程学习 Web3。我的进度应该算是比较慢了，实习手册的阅读进度还停留在「入门导读」部分，今天一个下午的时间都用于理解「以太坊」，包括以太坊的具体含义、内容、结构、核心机制、发展演变以及价值观。但我感受到了一些专注和享受的感觉，可能有一点像是慢慢吸取知识的感觉，至少我真的开始对以太坊有了感性的认识。可能知识的积累真的需要一个过程，这才第二天。

群里的小伙伴们都很活跃，求币、提问题、相互答疑、分享自己整理的资料和输出创作的内容，这些似乎在慢慢影响我——我也想多一些参与到大家的互动之中，但无奈于自己还有很多知识不懂，但昨晚给一个忘记保存笔记的小伙伴分享了用 Typora 记录和迁移笔记的建议，今天下午为一个小伙伴解答了如何查看测试币交易记录的问题。

不过，给我触动最大的是今晚群里大家对共学积分的讨论。有的小伙伴惊叹于第二天已经有人达到了 300 分以上，一位小伙伴说好些任务时间期限是二十多天，应当在后期写才有意义——刷分便失去了学习的意义。来 Web3 不适合带着学生思维，主要还是强调主动贡献优质项目，遇到喜欢的主动研究，即使找不到工作也有收益。前面这些我都能理解，也十分认同，但他还说了一段我听不懂的：「太多十倍的机会了。百倍不好遇到，十倍真的几乎每天都有」。

## **1\. 以太坊是？**

### **1.1 以太坊的内容**

-   开源的去中心化**区块链平台**
    
-   基于区块链的**去中心化计算平台**
    
-   原生代币是以太币
    
-   构建去中心化应用（Dapps）、去中心化金融（DeFi）、非同质化代币（NFT）等生态系统的**基础设施**
    
-   比特币之后最重要的**区块链项目**之一：比特币-区块链 1.0 的象征（专注货币属性）；以太坊-区块链 2.0 的代表
    
-   基于智能合约——存储在区块链的可执行代码，能在满足预设条件时自动执行操作
    

### **1.2 以太坊的结构**

**1.2.1 生态架构**

**Layer 1（主网）+Layer 2（二层扩展解决方案）+侧链（Sidechains）**

-   **Layer 1（L1）**
    
    -   **以太坊主网**：核心区块链，负责最终安全性与共识
        
    -   **EVM**：以太坊虚拟机，**执行智能合约代码**
        
    -   **账户系统**：外部账户（EOA）与合约账户（CA）共同构成网路基础
        
-   **Layer 2（L2）**
    
    -   **Rollup**：通过将交易批量处理后提交到 L1，以降低 Gas 费
        
        -   **Optimistic Rollup**：假设交易合法，仅在争议时验证。
            
        -   **ZK Rollup**：通过零知识证明验证交易，无需链上争议。
            
-   **侧链**：独立运行的链，通过桥梁与主网交互
    

**1.2.2 生态层次**

-   **扩展层**
    
    -   提升性能和降低成本的解决方案：
        
        -   **Layer 2 Rollups**：Arbitrum、Optimism、Polygon zkEVM、zkSync Era
            
        -   **侧链**：Polygon PoS、xDAI（Gnosis Chain）
            
        -   **状态通道**：Lightning Network for Ethereum 应用层
            
-   **应用层**
    
    -   用户直接交互的应用和界面
        
        -   **DeFi 应用**：Uniswap（去中心化交易所）、Aave（借贷协议）、Compound（借贷协议）
            
        -   **NFT 平台**：OpenSea、Foundation、SuperRare
            
        -   **钱包应用**：MetaMask、Coinbase Wallet、Rainbow
            
        -   **DAO 工具**：Snapshot、Aragon、Colony
            
-   **协议层**
    
    -   以太坊的核心基础设施
        
        -   **共识层客户端**：Prysm、Lighthouse、Nimbus、Teku
            
        -   **执行层客户端**：Geth、Nethermind、Erigon、Besu
            
        -   **核心协议**：EVM、状态管理、Gas 机制
            

### **1.3 以太坊的核心机制**

**账户系统 + Gas 模型 + 以太坊虚拟机（EVM）**

-   **账户系统：数字身份**
    
    -   私钥控制的**外部账户（EOA）**\+ 智能合约代码控制的**合约账户（CA）**
        
    -   用户使用外部账户（EOA）与区块链中的其他用户的外部账户（EOA），或者与合约账户（CA）进行交互
        
    -   外部账户由一对密钥生成，这对密钥包括私钥和公钥
        
        -   私钥：我们控制账户的钥匙，必须严格保密
            
        -   公钥：通过加密算法生成一个唯一的地址，可拿来分享给朋友接收转账
            
    -   合约账户不由私钥控制，而由代码驱动
        
        -   部署智能合约（如 NTF 市场）后，区块链自动生成一个 CA 地址
            
        -   合约账户无法主动发起交易，只能通过外部账户触发
            
    -   账户关键词
        
        -   **Nonce**：防止重复交易的计数器（EOA 记录发送次数，CA 记录创建合约次数）。
            
        -   **余额**：账户持有的 ETH 数量（单位为 Wei）。
            
        -   **CodeHash**：EOA 为空哈希，CA 存储合约字节码的哈希值。
            
        -   **StorageRoot**：记录账户数据的 Merkle 树根哈希（如 NFT 归属关系）。
            
-   **Gas 模型**
    
    -   使用 EOA 发起交易，都需要支付 Gas 「燃料费」\[^gas fee\]
        
    -   Gas Fee = Gas Limit × Gas Price
        
        -   Gas Limit 是我们最多愿意「烧」的燃料
            
        -   Gas Price 是每单位燃料的钱，用 Gwei 表示，1 Gwei = 0.000000001 ETH。如果网络越拥堵，Gas Price 越高。
            
    -   Gas 存在的目的
        
        -   激励矿工/验证者，给的 Gas Price 越多，越愿意优先处理
            
        -   防止资源滥用，如果有人想让合约死循环，Gas 会用光，交易失败，系统不会被拖垮
            
    -   现在 Gas Price 随着 EIP-1559 的升级分成了两部分，包括基础费用和小费
        
-   **以太坊虚拟机**
    
    -   代码的执行引擎
        
    -   用于运行智能合约的虚拟计算机，是以太坊的大脑
        
    -   运行在每一个节点中，确保整个网络在处理代码时结果都一致，可信任
        
    -   特点
        
        -   如真正的电脑一眼，可以执行各种逻辑（如 if 判断、循环）
            
        -   全球同步，每个节点/矿工都会自己执行一遍合约代码，保证结果一致
            
        -   隔离安全，合约在特定空间运行，不会被允许随意访问用户的数据和网络
            

\[^gas fee\]: 我们在第一天的学习中知道，所谓的 Gas 燃料费就是网络服务商（矿工）挖矿后的奖励/工资。

流程：通过 EOA 发起交易 → 交易附带 GAS 参数，矿工/验证者选择打包 → EVM 执行合约代码 → Gas 费用按照 Gas Limit × Gas Price 扣除

## **2\. 以太坊的发展**

### **2.1 共识机制：以太坊 1.0（PoW 阶段） → 以太坊 2.0 （PoS 阶段）**

我对共识机制的理解是**如何决定由谁来打包交易**

1.  **PoW：工作量证明**
    

2.  **运行原理**：区块链是去中心化的，交易信息广播道各节点并被验证后，节点上的网络服务提供商（矿工）一般还要通过**计算机算力**来获得打包交易权利，成功打包区块的矿工会获得相应的代币奖励（比特币或以太币等）。
    

3.  **特点**：算力能耗大，每秒只能处理 30 笔交易（TPS），速度慢
    

1.  **PoS：权益证明**
    

2.  **运行原理**：质押 32 个以太币成为验证者，之后系统随机选择验证者来提议和验证区块，验证者将获得新发行的以太币和交易费用，作恶者质押的 32 个以太币会被销毁——这不正是基于人性的设置吗？从 PoW 到 PoS，代表挖矿者成为持有经济体系的利益，一旦作恶个人的利益首当其冲受到影响。
    

3.  **特点**：能耗降低 99.95%，无需大量算力和硬件，安全度高（PoS 机制决定要篡改账本则需要付出全网 67% 的质押以太币成本）
    

4.  **背景**：以太坊团队创建信标链以运行 PoS，初期，以太坊主网仍是以 PoW 挖矿，两链独立并行，后来进行 The Merge，共识机制插接到信标链的 PoS 系统。以太坊的实际结构也分为两层：1、执行层（相当于原主网，处理交易、智能合约）2、共识层（相当于信标链，管理验证者、确定区块顺序）
    

**Layer 2 → EIP-4844**

Layer 2 一般在链下批量处理大量交易，然后定期将交易数据打包到以太坊主网。但 L2 提交数据使用常规交易，成本高昂，引入了专门的 Blob 交易类型，数据存储成本大大降低。

**EIP-4844 → ZK-Rollup**

ZK-Rollup 在线下一次性处理数百笔交易，生成一个简洁「正确性证明」，以太坊只需验证该证明，无需重新执行所有交易。

**数据分片 + Layer 2**

## **3\. 以太坊价值观**

-   **去中心化治理**
    
    -   没有单一的控制者或权威机构
        
    -   社区通过公开讨论和 EIP（以太坊改进提案）机制共同决策
        
    -   验证者遍布全球，防止权力集中
        
-   **无需许可与开放性**
    
    -   任何人都可以使用、开发、部署智能合约
        
    -   开源代码，透明可审计
        
    -   无身份、地域、财富限制的参与门槛
        
-   **抗审查性**
    
    -   交易和智能合约不受政府或机构干预
        
    -   通过分布式验证确保网络弹性
        
    -   支持言论自由和经济自由
        
-   **密码朋克精神**
    
    -   代码即法律：用算法和数学构建信任
        
    -   密码学保护隐私和自主权
        
    -   技术驱动的社会变革，而非政治手段
        
-   **公共物品导向**
    
    -   优先考虑生态系统整体利益
        
    -   支持开源项目和基础设施建设
        
    -   通过各种资助计划推动创新
        
-   **可持续发展理念**
    
    -   The Merge 体现了对环境责任的承诺
        
    -   长期主义思维，注重技术的可持续演进
        
    -   平衡创新速度与网络稳定性
        

## **4\. 以太坊与比特币的比较**

如前所述，比特币是区块链 1.0 的象征，而以太坊被定位为区块链 2.0 的代表，如果要比较区块链 1.0 和 区块链 2.0，可以具体从比特币和以太坊之间的比较开始。

|   | 比特币 | 以太坊 |
| --- | --- | --- |
| 性质、目标、定位 | 区块链 1.0，去中心化数字货币，强调作为「数字黄金」的存储价值，总量 2100 万 | 区块链 2.0，去中心化平台，支持智能合约 |
| 共识机制 | PoW，工作量证明，算力成本 | PoS，权益证明 |
| 交易速度 | 每 10 分钟生成一个区块，交易确认慢 | 区块时间 12 秒，交易确认更快 |
| 技术栈 | 脚本语言有限，仅支持简单的交易验证逻辑 | 有完备的编程语言，如 Solidity，可开发复杂智能合约 |
| 经济模型 | 总量固定，强调抗通胀属性 | 供应灵活，通过 EIP-1559 等机制可能呈现通缩趋势 |

# **Bruce 老师讲解 Web3 运行原理**

Bruce 老师今晚对 Web3 做了全面的基本介绍，信息量很大，但感觉挺多信息可以和这两天在实习手册上阅读到的信息对应上，有一定的熟悉度。但我想我可能还是需要观看回放来做一个系统和全面的重新梳理，今晚就先写下自己印象最为深刻的——主要是「私钥的重要性」和「Web3 的价值」

不过在此之前，可以从技术的层面来一句话概括：**Web3 就是用私钥签名证明你是谁，用共识网络保证账本可信，用智能合约让规则自动执行**。对了，Bruce 老师还分享了一个很有意思的[网站](https://txcity.io/v/eth-btc)，里面以动画的形式来呈现区块链去中心化分布式的运行过程。

私钥很安全，是不可重置的密码，丢了就永远丢了，只能换钱包。区块链不认识我们是谁，只认识谁能签名。大部分资产被盗取，不是链被攻破，而是私钥、助记词泄露。一般来说，私钥都很长也很复杂，因此往往会借助助记词来帮助记忆。

wachi 助教补充了一段信息：

> 助记词是钱包，私钥是钱包里个夹层 助记词可以推出很多私钥 私钥推不出助记词

借由 Web3 中私钥的特性，可以联系到 Web3 中的社会学视角\[^perspective\]——私钥象征一种个人主权，无需第三方许可即可拥有自己的账户、账号，Web3 追求的是一种去中心化治理和共识的形成，探讨权力的重新分配，关乎于数字资产与权利自我控制、无许可、抗审查、公开可信。关于这一点，有一位同学向 Bruce 老师提问，问的问题也可以说是我这个纯粹小白好奇的，即 Web3 到底想做些什么呢，好像听起来难免有噱头的感觉，要颠覆 Web2.0 吗？或者说就是在金融那一领域比较火，是要炒币？

\[^perspective\]: 经济学视角：激励设计保证安全与可持续（Gas、质押、惩罚）；密码学视角：签名、哈希、ZK提供可信基础

针对这一问题，Bruce 老师的回答是 Web3 并不追求颠覆 Web2.0，它更想追求的是提供一个构建未来数字世界的重要基础，比如带来一些以严苛的审查机制和权力垄断特点出名 Web2.0 所缺乏的新机制，解决相关信任问题。比如在亚非拉一些地区的人们无法使用某些货币完成支付，如何让他们可以用到 Web3 的技术完成支付？还有我们的群聊之前由于进行技术讨论也会被封禁，Web3 也是想探索一种可以不被完全控制的方式。

不过需要指出的是，当下 Web3 领域确实分出不同的人，一拨人被称为 Degen，他们投机、炒币、搞噱头发币，而另一拨人则被称为 Regen，他们考虑的是 Web3 的可持续性，到底可以让 Web3 的理念和技术如何落地，做出实际的事情——「选择做什么样的人，选择和什么样的人待在一起」。

一些Tips：

-   开源的 Web3 一般不被认为是作恶的。
    

-   关于钱包的安全建议：
    

热钱包：日常高频使用的存放少量资产，不同钱包使用不同的助记词创建

冷钱包：用于储蓄和转账，减少交互和使用

周期性第创建新钱包进行资产转移

很多资深人士的做法是将资产放入硬件钱包，每次需要对外交互的时候，都重新创建一个新钱包，用完即废
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->












# **1\. 知识学习**

## **1.1 区块链与区块链网络**

### **1.1.1 区块链**

区块链就是由「区块」和「链」来组成的一种结构——「区块」内包含交易记录等信息和**过去的区块摘要（哈希）**，一条「链」按照顺序，只可以连接两个「区块」。

区块链的核心逻辑是「链」，但为了更好地对其进行理解，我们需要先讲讲什么是哈希（Hash）。

哈希，简单来说就是**将任意长度的数据，通过算法变成一串固定长度的代码**，这段代码可以被理解为区块的唯一标识。每个区块都同时拥有两种哈希值，其一是「自身的哈希值」，其二是自身连接着的「上一个区块的哈希值」。那么这有什么作用呢？这里就很好体现了区块链「**不可篡改**」的特性：每个区块里的数据一旦确定就会生成一个「自身的哈希值」，假设对区块 X 中的交易记录进行篡改，区块 X 中「自身的哈希值」（0111）就会发生剧变，同时，由于区块 X 中还标示着上一个区块 Y「自身的哈希值」（0110），区块 X 和区块 Y 之间的连接会因此而断裂。

![01-12-1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/residualsun1/images/2026-01-12-1768226089087-01-12-1.png)

除了「不可篡改」外，区块链还有两个特性。一个是「**公开透明与匿名**」——公开透明在于区块链上的信息，每个人都可以顺着区块与链找到历史上的记录进而查看钱包余额，匿名在在与不知道这是谁的钱包；另一个则是「**快速交易**」——无论金额地点，只要交易记录被打包于区块链中即可自动完成交易。

由以上这些「**不可篡改**」、「**公开透明与匿名**」和「**快速交易**」三个特点，可以将区块链总结为一种**去中心化**的**分布式**数据库或记账账本。

### **1.1.2 区块链网络**

![01-12-2.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/residualsun1/images/2026-01-12-1768226111168-01-12-2.png)

我自己没能看懂这幅图，还是求助了 Gemini。图中的黑色圆圈是节点，也是网络服务提供商（矿工）——其实也可以理解为一部管理账本的电脑，黑色的连接线把各个节点连接起来，但其中没有处于中心的对象，所有节点都彼此连接成了一张网，即便其中一个节点故障了，也不影响其他节点运行。剩下的紫色与黄色链条则是前面说到的区块链——每个节点/网络服务提供商都有着一样的区块链。换句话说，所有人都在共同维护一样的账本。

在 Gemini 辅助的基础上，我区分了「区块链」和「区块链网络」。「区块链」是「区块」与「链」，本质上相当于存储着一定信息——包括前面说的交易记录、哈希值——的文件，放在上图中则是黄色与紫色链条的部分，但「区块链网络」则是囊括了黑色节点、连接线还有区块链的整体。打一个不恰当的比喻，「区块链网络」就像一群连接起来的电脑，它们共同运行和管理同样的账本——也就是一个个有着同样信息的区块链。所有节点都盯着这些区块链，而由于区块链的信息一样，一旦局部发生改变就会牵一发而动全身。

### **1.1.3 实际运行**

用户通过钱包应用发起转账、智能合约调用 → 交易信息被广播到整个网络中的各个节点 → 节点中的「矿工」验证交易合法性 → 矿工将验证过的交易打包成新区块 → 新区块被添加到区块链上并更新全网账本 → 成功打包区块的「矿工」获得代币奖励

## **1.2 比特币**

理解了「区块链网络」的概念后，就方便理解什么是比特币了。简而言之，在我看来，比特币或许可以被理解为**一种在「奖励」的基础上形成的独特虚拟货币**。

**首先是关于其来源**。如前所述，在「区块链网络」之中，所有的节点都会管理同样的区块链，但这背后的问题是为什么一个陌生人要帮助另一个陌生人记账呢。比特币由此应运而生，作为一种代币奖励来激励网络服务商提供服务，相应地，网络服务提供商被称作「矿工」，提供服务也就是管理区块链的过程就是「挖矿」了。

**其次是其货币属性**。比特币的设计决定了其数量有限，最多只能达到 2100 万个，因而被视为「数字黄金」，同时区块链的不可篡改性使得人们对比特币的信任度更高，同时比特币可以被自由转账，便可被当作资产。

## **1.3 区块链类型**

**公链**

-   人人都可以加入节点成为维护者
    
-   所有数据信息都公开可见
    
-   决策**去中心化**
    
-   由于人太多，决策效率较慢（交易确认）
    

**联盟链**

-   加入节点需要得到多位联盟成员中的一位的认可，不同的联盟对数据管理的权限不同
    
-   数据信息仅联盟内部成员可见
    
-   决策**多中心化**
    
-   相比于公链，联盟链在数据管理上的总体特征表现为效率更高、隐私性更强
    

**私链**

-   加入节点需要得到一位决策者的同意
    
-   数据信息只有内部人员可见
    
-   决策**中心化**，决策者权限最高
    
-   效率和隐私最高，但最为缺乏透明性
    

## **1.4 区分 Web2.0, Web3.0 与 Web3**

|   | Web2.0 | Web3.0 | Web3 |
| --- | --- | --- | --- |
| 数据控制权/主权 | 中心化；平台垄断 | 部分开放 | 去中心化；用户所有 |
| 数据存储 | 中心服务器 | 混合存储 | 区块链/IFPS |
| 技术栈 | JavaScript; React | RDF; OWL | Solidity; Rust; React |
| 支付媒介 | 信用卡/支付宝/微信 | 集成支付 | 加密货币 |

## **1.5 去中心化的优势与挑战**

优势：

-   无需依赖**中心化第三方**，共识算法和加密证明降低信任成本
    
-   数据分布存储在多个节点，单点故障或攻击难以完全破坏网络
    
-   用户高度掌控资产与数据，平台难以干涉，拥有更高隐私权和所有权
    

挑战：

-   去中心化与匿名特性和先行法规存在冲突
    
-   用户使用体验与去中心化程度之间需要平衡
    
-   公链在节点众多时共识效率较低，延迟问题突出
    

# **2\. 个人心路**

或许正是因为每个人的背景不一样，有些人早已涉足过 Web3，第一天分布的「创建钱包，并尝试测试币转账」的任务就有学员在做了，他们说着我看不懂的行话，例如「mint」、「钱包地址」、「挖币」……这让我感到有些焦虑。但我想这种对未知的恐惧也是一种锻炼，越是未知，越是要保持冷静，想清楚自己的定位，自己究竟要学什么，怎么学——既然真的是完全小白，就做到小白能做到的，将基础打好即可，做到自己能做的。一步一步来，不要妄自菲薄，也不要好高骛远。

不过说实话，我有些讨厌「炒币」这类事情，不知道是不是有的学员就奔着这一点来呢？我更喜欢 Bruce 说的「开源」、「建设」，很期待 Bruce 要分享的关于 Web3 的全貌，而不是所谓狭隘的比特币。

* * *

一天下来，发现许多其他行业的学员都想加入 Web3，大家都很期待能在这里转行，找到一份实习或者工作。我觉得对自己的定位还是不够清晰，我i似乎多少受到大家的影响，又或者说忧虑起两年后的就业，但这样一想就会患得患失。与其如此，其实回归一开始的潜心学习技术，学多少是多少才是王道吧……唉，适度的焦虑是合适的，但过度焦虑就感觉迷失了方向。

具体来说，我应该明确自己最终的目的不是非找到 Web3 的工作或者必须进入 Web3 行业，而是带着一颗纯粹的心去探索、了解，能做多少是多少——对待做事，态度应该是我们必须全力以赴，同时又不抱任何希望。不管做什么事，都要把它当作是全世界最重要的一件事，但同时又知道这件事根本无关紧要。

能学到东西，就很棒啦，加油，坚持下去！
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
