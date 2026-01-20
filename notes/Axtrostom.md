---
timezone: UTC+8
---

# Axtrostom

**GitHub ID:** Axtrostom

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->
这周要完成的目标

联邦学习实验代码

基础和深度技术任务

solidity基础语法

rust刷刷力扣

今天保底把 solidity 基础语法看完

* * *

## Solidity

### Transient Storage 瞬态存储

> 必须确保 EVM 版本和虚拟机设置为 Cancun (坎昆升级后的版本)

-   Storage (存储): 数据存储在区块链上（永久，写入极贵，如硬盘）
    
-   Memory (内存): 数据在函数调用结束后清除（临时，便宜，如内存条）
    
-   Transient Storage (瞬态存储): 数据在【整笔交易】结束后清除（介于两者之间，跨函数保留，交易完销毁）
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 必须确保 EVM 版本和虚拟机设置为 Cancun (坎昆升级后的版本)

interface ITest {
    function val() external view returns (uint256);
    function test() external;
}

// 这是一个辅助合约，用来模拟“回调”场景
// 它的作用是：当有人调它，它就反过来去调调用者的 val() 函数
contract Callback {
    uint256 public val;

    fallback() external {
        // 反向调用 msg.sender 的 val() 函数
        val = ITest(msg.sender).val();
    }

    function test(address target) external {
        ITest(target).test();
    }
}

// 1. 传统 Storage 测试
contract TestStorage {
    uint256 public val;

    function test() public {
        val = 123; // 写 Storage (昂贵，消耗 20000+ Gas)
        bytes memory b = "";
        // 触发回调，Callback 会来读取 val
        msg.sender.call(b);
    }
}

// 2. 瞬态存储测试 (主角)
contract TestTransientStorage {
    // 定义一个槽位号
    bytes32 constant SLOT = 0;

    function test() public {
        // 使用汇编写入瞬态存储
        // tstore(key, value)
        assembly {
            tstore(SLOT, 321)
        }
        bytes memory b = "";
        // 触发回调
        msg.sender.call(b);
    }

    // Callback 合约会回调这个函数
    function val() public view returns (uint256 v) {
        // 使用汇编读取瞬态存储
        // tload(key)
        assembly {
            v := tload(SLOT)
        }
        // 关键点：虽然这是另一个函数调用 (val)，
        // 但因为还在同一笔交易里，所以 tload 能读到刚才 tstore 存进去的 321。
    }
}

// 3. 传统的防重入锁 (使用 Storage)
contract ReentrancyGuard {
    bool private locked; // 占用 Storage 槽位

    modifier lock() {
        require(!locked);
        locked = true;  // 写 Storage (贵)
        _;
        locked = false; // 写 Storage (贵)
    }

    // 这一套下来大约消耗 35313 gas
    function test() public lock {
        // 忽略调用错误
        bytes memory b = "";
        msg.sender.call(b);
    }
}

// 4. 瞬态存储防重入锁 (使用 Transient Storage)
contract ReentrancyGuardTransient {
    bytes32 constant SLOT = 0;

    modifier lock() {
        assembly {
            // tload 检查是否上锁
            if tload(SLOT) { revert(0, 0) }
            // tstore 上锁 (非常便宜)
            tstore(SLOT, 1)
        }
        _;
        assembly {
            // 解锁
            tstore(SLOT, 0)
        }
    }

    // 这一套下来只要 21887 gas (省了约 1.4万 Gas!)
    function test() external lock {
        // 忽略调用错误
        bytes memory b = "";
        msg.sender.call(b);
    }
}
```

```Solidity
interface ITest {
    function val() external view returns (uint256);
    function test() external;
}
```

上面这段代码是定义接口的语法

在 Solidity 中，接口用来定义**两个合约之间如何对话**。

**命名习惯：** 接口通常以大写 `I` 开头（如 `IERC20`, `ITest`），方便识别。

### 接口需要被实现吗？

**理论上：** 接口只是一个空壳（规范），如果想让代码真正跑起来，必须有某个具体的合约去**实现**里面的逻辑。

**写法上：** 你不需要显式地写 `contract A is ITest`。

在 Solidity 中，接口的使用遵循 **“鸭子类型 (Duck Typing)”** 的逻辑：

> “如果它走起路来像鸭子（函数名一样），叫起来像鸭子（参数和返回值一样），那它就是鸭子（实现了接口）。”

只要目标合约里**包含了接口所要求的函数**（名字、参数、返回值都对得上），接口就可以调用它，**完全不需要**目标合约显式地声明 `is ITest`。

调用接口，或者说调用合约的语法如下

```Solidity
ITest(target).test();
```

其中

-   `target` **(地址):** 这是一个 `address` 类型的变量。 它代表了目标合约在区块链上的“家庭住址”（例如 `0x5B38Da6a701c568545dCfcB03FcB875f56beddC4`）。
    
-   _问题：_ 光有一个地址，编译器并不知道这个地址里住了谁，也不知道它有什么功能。
    
-   `ITest(...)` **(包装/转换):** 这步叫 **“类型转换” (Type Casting)**。 它的意思是：**“编译器听令！把** `target` **这个地址，当成一个** `ITest` **类型的合约来看待。”**
    
-   一旦你套上了 `ITest` 的壳，编译器就知道：“哦，原来这个地址里有一个叫 `test()` 的函数啊。”
    
-   `.test()` **(调用):** 这才是真正的**执行**。 向那个地址发起一笔交易（或调用），执行其中的 `test` 函数。
    

```Solidity
fallback() external {
    val = ITest(msg.sender).val();
}
```

这是 Solidity 中的 **回退函数 (Fallback Function)**。

`fallback()` 是一个特殊的函数，它没有名字，也没有参数。它会在以下两种情况下被**自动触发**：

1.  **别人瞎调你的函数时 (Function not found):** 如果有其他合约（或者人）调用了你的合约，但是**函数名字写错了**，或者**根本不存在**这个函数，EVM 不会直接报错，而是会把这个请求转给 `fallback()` 处理。
    
2.  **别人给你发空数据时:** 如果有人发起了一笔交易，通过 `call` 或者是直接发数据，但没有指定任何具体的函数签名（就像你代码里的 `msg.sender.call(b)`），也会触发它。
    

在 Solidity 中，`msg.sender` 是一个**动态变量**，它永远代表**“上一手调用我的人（或合约）”**。

```Solidity
// 1. 传统 Storage 测试
contract TestStorage {
    uint256 public val;

    function test() public {
        val = 123; // 写 Storage (昂贵，消耗 20000+ Gas)
        bytes memory b = "";
        // 触发回调，Callback 会来读取 val
        msg.sender.call(b);
    }
}
```

如果跨合约传递 memory 类型的参数，需要额外开辟一片内存空间进行值拷贝

  **跨合约传递 Memory 变量的本质：**

1.  **不是引用传递**：指针发不过去。
    
2.  **是值拷贝**：数据会被“序列化”传输。
    
3.  **双重成本**：
    
    1.  发送方：需要 Gas 进行 ABI 编码。
        
    2.  接收方：如果声明为 `memory`，需要 **开辟新内存 + 再次拷贝**。
        
4.  **优化建议**：接收方的函数参数尽量写成 `calldata`，避免接收时的“二次拷贝”和内存开销。
    

内存计费的最小单位是 **32 字节 (32 Bytes)**。

```Solidity
// 2. 瞬态存储测试 (主角)
contract TestTransientStorage {
    // 定义一个槽位号
    bytes32 constant SLOT = 0;

    function test() public {
        // 使用汇编写入瞬态存储
        // tstore(key, value)
        assembly {
            tstore(SLOT, 321)
        }
        bytes memory b = "";
        // 触发回调
        msg.sender.call(b);
    }

    // Callback 合约会回调这个函数
    function val() public view returns (uint256 v) {
        // 使用汇编读取瞬态存储
        // tload(key)
        assembly {
            v := tload(SLOT)
        }
        // 关键点：虽然这是另一个函数调用 (val)，
        // 但因为还在同一笔交易里，所以 tload 能读到刚才 tstore 存进去的 321。
    }
}
```

然后这个代码

我草，有点看不懂了

一下塞一堆没见过的东西

**1\.** `SLOT` **必须是 state 变量吗？写成** `tstore(0, 321)` **一样吗？**

**答案：不需要是 state 变量。写成** `0` **是一模一样的。**

在你的代码里，`SLOT` 其实定义的是 `constant`（常量）：

Solidity

```Plain
bytes32 constant SLOT = 0;
```

这意味着在编译的时候，编译器会把代码里所有的 `SLOT` 直接替换成数字 `0`。

**为什么大家喜欢用** `SLOT` **而不是直接写** `0`**？** 这纯粹是为了**代码可读性**和**防止冲突**。

-   如果你直接写 `tstore(0, 321)`，过两个月你再看代码：“这个 0 是啥意思？是初始值？还是第 0 个位置？”
    
-   如果你写 `tstore(REENTRANCY_GUARD_SLOT, 321)`，你就知道：“哦，这个位置是用来存放防重入锁的。”
    

**底层的本质：**`tstore` 函数只在乎第一个参数是一个 **32字节的数字（位置编号）**。它不关心这个数字是来自常量、变量，还是你手写的一个 `0`。

* * *

**2\.** `assembly` **是什么？是必要的吗？**

`assembly` **是 Solidity 的“后门”，或者说是“手动挡模式”。**

-   **是什么：** 它允许你在 Solidity 代码中直接写 **Yul 语言**（一种贴近机器码的低级语言）。你可以直接指挥以太坊虚拟机（EVM）去执行最底层的指令（Opcode）。
    
-   **是必要的吗？** **在这个特定的例子（瞬态存储）中，是的，它是绝对必要的。**
    

**为什么？** 因为 Solidity 语言的更新速度往往比以太坊网络（EVM）的更新速度要慢半拍，或者为了安全起见更加保守。

-   **EVM 说：** “兄弟们，坎昆升级（Cancun）完成了！我现在支持瞬态存储了，我有两个新指令：`TSTORE` 和 `TLOAD`！”
    
-   **Solidity 编译器说：** “呃，我还没有给这两个新指令设计专门的语法（比如 `transient uint x;` 这种写法还没完全定型或普及）。”
    
-   **开发者说：** “但我现在就想用！怎么办？”
    
-   **解决办法：** 使用 `assembly`。通过汇编代码，直接调用底层的 `tstore` 和 `tload` 指令，绕过 Solidity 高级语法的限制。
    

**总结：** 当你想用最新的 EVM 特性，或者想极致优化 Gas（省去 Solidity 自动生成的安全检查代码）时，`assembly` 就是必要的。

* * *

**3\.** `tstore(key, value)` **是什么？**

它是 **Transient Store（瞬态存储写入）** 的缩写。

你可以把它理解为 EVM 底层的一个操作指令。

-   **Key (槽位):** 你要把数据存在哪个柜子里？（比如第 0 号柜子）。
    
-   **Value (值):** 你要存什么东西？（比如数字 321）。
    

它确实是在底层创建了一个 **Key-Value 键值对**。

* * *

**4.** **为什么不直接用 Memory Mapping？**

既然是“临时存储”，Memory 不也是临时的吗？为什么要搞个新东西？这里有三个巨大的区别：

**理由一：Solidity 根本不支持 Memory Mapping**

在 Solidity 中，`mapping` 类型**只能**存在于 `storage` 中。 你**不能**写 `mapping(uint => uint) memory myMap;`。这是语言层面的限制。如果你想在 Memory 里实现类似 Mapping 的功能，你需要自己写极其复杂的哈希算法，不仅代码难写，而且 Gas 费巨贵。

**理由二：生命周期不同（最关键点！）**

这是最容易混淆的地方，请仔细看：

-   **Memory (内存) = 函数级生命周期**
    
    -   你在 `test()` 函数里申请了一块 Memory。
        
    -   当你调用 `msg.sender.call(b)` 离开当前函数，去执行别人的代码，或者哪怕是别人回调你的 `val()` 函数时。
        
    -   **之前的 Memory 对** `val()` **是不可见的！** `val()` 处于一个新的“执行栈帧 (Stack Frame)”中，它有自己独立的一块 Memory。
        
    -   _比喻：Memory 是每个人手里的草稿纸。_`test` _写在自己纸上，_`val` _根本看不到_ `test` _的纸。_
        
-   **Transient Storage (瞬态存储) = 交易级生命周期**
    
    -   它是挂在当前合约实例上的一个**全局暂存区**。
        
    -   只要这笔交易还没结束，无论函数怎么跳来跳去（`test` -> `Callback` -> `val`），只要是在同一个合约地址下，大家都能看到这个区域。
        
    -   _比喻：Transient Storage 是贴在墙上的白板。_`test` _写上去，_`val` _进屋也能看到。交易结束，白板擦除。_
        

**理由三：Gas 成本**

就算你能用 Memory 模拟出跨函数共享（通过疯狂传参），当数据量大或者调用层级深时，不断地 Copy Memory 会非常贵。而 `tstore` 就像访问一个全局变量一样便宜。

**tstore就是创建了一个临时的 state 类型的 Mapping**

然后这个函数

```Solidity
    function val() public view returns (uint256 v) {
        // 使用汇编读取瞬态存储
        // tload(key)
        assembly {
            v := tload(SLOT)
        }
```

读取 直接使用 tloade(key) 就可以了

tloade tstore 必须要在 `assembly { ... }` 里面使用

目前 Solidity (v0.8.26) 还没有把 `tload` 做成一个像 `address(this).balance` 那样的内置函数。EVM 认识它，但 Solidity 编译器不认识它。所以你必须用 `assembly` 告诉编译器：“别管语法了，直接把这行机器码塞进去”。

然后在 assembly 括号中要使用 Yul 语言，

-   **区别：** 在 `assembly { ... }` 区块里（也就是 Yul 语言中），**赋值必须用** `:=`。
    
-   **作用：** 把右边运算的结果，塞进左边的变量里。
    

看一下函数定义 `function val() ... returns (uint256 v)`。

这个 `v` 是 Solidity 定义的**返回值变量**。

Yul 汇编代码可以直接访问外部 Solidity 定义的局部变量。

当这行代码执行完，`v` 就变成了 `321`。函数结束时，就会把 `v` 返回给调用者。

所以就不需要 定义然后显示的写 return 了

**防重入锁 (Reentrancy Guard)** 是智能合约安全中**最重要、最基础**的防御机制之一。

简单来说，它就是给合约的函数装一把**“门锁”：“在我还没办完这次业务之前，任何人都别想再进来，包括我自己。”**

简单来说，防重入锁就是用于保证合约原子性的工具

```Solidity
// 3. 传统的防重入锁 (使用 Storage)
contract ReentrancyGuard {
    bool private locked; // 占用 Storage 槽位

    modifier lock() {
        require(!locked);
        locked = true;  // 写 Storage (贵)
        _;
        locked = false; // 写 Storage (贵)
    }

    // 这一套下来大约消耗 35313 gas
    function test() public lock {
        // 忽略调用错误
        bytes memory b = "";
        msg.sender.call(b);
    }
}
```

`_;` 是什么？

这是 Solidity 中 `modifier` **(修饰器)** 独有的语法符号，你可以把它叫作 **“占位符”** 或者 **“下划线”**。

它的含义是：**“请把被修饰函数的代码，粘贴到这里来执行。”**

在你的代码中，`_;` 起到了 **“三明治夹心”** 的作用，它决定了原本的 `test()` 函数代码在什么时候运行。

`revert(0, 0)` 是 Solidity 内联汇编 (Yul) 中**最节省 Gas 的报错方式**。

它的意思是：**“立即停止交易，回滚所有操作，并且不给任何理由（不返回错误信息）。”**

我们来像拆解炸弹一样拆解这句指令：

**1.** **语法拆解：**`revert(p, s)`

在汇编（Assembly）中，`revert` 接受两个参数：

-   **第一个参数** `p` **(offset):** 错误信息在内存中的**起始位置**。
    
-   **第二个参数** `s` **(size):** 错误信息的**数据长度**。
    

**2\.** `(0, 0)` **代表什么？**

-   **起始位置 0:** 从内存的第 0 个字节开始读。
    
-   **长度 0:** 读取 0 个字节。
    

> 再无话可说，请速速动手

### Function

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Function {
    // 【多返回值】
    // 函数可以一次性返回多个值（不需要像 Java 那样封装成对象）
    function returnMany() public pure returns (uint256, bool, uint256) {
        return (1, true, 2);
    }

    // 【命名返回值】
    // 返回值可以在 returns 里直接起名字 (x, b, y)
    function named() public pure returns (uint256 x, bool b, uint256 y) {
        return (1, true, 2);
    }

    // 【隐式返回】
    // 既然返回值已经命名了，我们可以直接给这些名字赋值。
    // 在这种情况下，可以省略 'return' 语句，Solidity 会自动返回这些变量的当前值。
    function assigned() public pure returns (uint256 x, bool b, uint256 y) {
        x = 1;
        b = true;
        y = 2;
        // 这里的 return 被省略了，但效果和上面一样
    }

    // 【解构赋值】
    // 当调用另一个返回多值的函数时，我们可以用元组 (tuple) 来接住这些值。
    function destructuringAssignments()
        public
        pure
        returns (uint256, bool, uint256, uint256, uint256)
    {
        // 调用 returnMany，把它返回的三个值分别赋给 i, b, j
        (uint256 i, bool b, uint256 j) = returnMany();

        // 【值可以被省略】
        // 如果你只想要第 1 和第 3 个值，不想要第 2 个，可以空着不写
        // (4, 5, 6) -> 4 给 x, 5 被丢弃, 6 给 y
        (uint256 x, , uint256 y) = (4, 5, 6);

        return (i, b, j, x, y);
    }

    // 【类型限制】
    // 不能使用 mapping (映射) 作为输入参数或输出结果
    
    // 可以使用 array (数组) 作为输入
    function arrayInput(uint256[] memory _arr) public {}

    // 可以使用 array (数组) 作为输出
    uint256[] public arr;
    function arrayOutput() public view returns (uint256[] memory) {
        return arr;
    }
}

// 调用函数时的【键值对传参】
contract XYZ {
    function someFuncWithManyInputs(
        uint256 x,
        uint256 y,
        uint256 z,
        address a,
        bool b,
        string memory c
    ) public pure returns (uint256) {}

    // 方式 1：常规调用
    // 必须严格按照定义的顺序传参
    function callFunc() external pure returns (uint256) {
        return someFuncWithManyInputs(1, 2, 3, address(0), true, "c");
    }

    // 方式 2：键值对调用 (Key-Value Inputs)
    // 只要名字对得上，顺序乱了也没关系，可读性更强
    function callFuncWithKeyValue() external pure returns (uint256) {
        return someFuncWithManyInputs({
            a: address(0),
            b: true,
            c: "c",
            x: 1,
            y: 2,
            z: 3
        });
    }
}
```

感觉没什么好说的

### View and Pure Functions

```JavaScript
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract ViewAndPure {
    uint256 public x = 1; // 这是一个状态变量（写在区块链上的）

    // 【View 函数】
    // 承诺不修改状态 (Promise not to modify the state)
    // 它可以“看”到 x，但不能“改” x
    function addToX(uint256 y) public view returns (uint256) {
        return x + y; // 读取了状态变量 x
    }

    // 【Pure 函数】
    // 承诺不修改也不读取状态 (Promise not to modify or read from the state)
    // 它完全不理会 x，只关心传入的参数 i 和 j
    function add(uint256 i, uint256 j) public pure returns (uint256) {
        return i + j; // 既没读 x，也没改 x
    }
}
```

-   Getter 函数（读取函数）可以被声明为 view 或 pure。
    
-   View 函数声明：承诺不会【修改】任何状态。
    
-   Pure 函数声明：承诺既不会【修改】也不会【读取】任何状态变量。
    

### Error

一个错误将撤销交易期间对状态所做的所有更改。

可以通过调用 `require` 、 `revert` 或 `assert` 抛出错误。

-   `require` 用于在执行前验证输入和条件。
    
-   `revert` 类似于 require。以下代码提供了详细信息
    
-   `assert` 用于检查永远不应为假的代码。断言失败返回错误信息 `Panic(0x01)`（Assertion failed），**没有**文字描述
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Error {
    // 1. 测试 require
    function testRequire(uint256 _i) public pure {
        // Require 用来做“守门员”，验证：
        // - 输入参数
        // - 执行前的条件
        // - 调用其他函数的返回值
        // 语法：require(条件, "错误提示信息");
        require(_i > 10, "Input must be greater than 10");
    }

    // 2. 测试 revert
    function testRevert(uint256 _i) public pure {
        // Revert 适用于判断逻辑非常复杂的场景（例如有多层 if-else 嵌套）
        // 下面这段代码的作用和上面 testRequire 完全一样
        if (_i <= 10) {
            // 语法：revert("错误提示信息");
            revert("Input must be greater than 10");
        }
    }

    uint256 public num;
    
    // 3. 测试 assert
    function testAssert() public view {
        // Assert 应该仅用于测试内部错误和检查不变量 (Invariants)。
        // 这里的 num 永远是 0，因为合约里没有修改 num 的函数。
        // 如果这里报错，说明 EVM 出问题了或者代码逻辑有严重的 Bug。
        assert(num == 0);
    }

    // 4. 自定义错误 (Custom Error) - 省钱利器
    // 定义一个错误类型，可以带参数
    error InsufficientBalance(uint256 balance, uint256 withdrawAmount);

    function testCustomError(uint256 _withdrawAmount) public view {
        uint256 bal = address(this).balance;
        // 如果余额不足
        if (bal < _withdrawAmount) {
            // 抛出自定义错误
            // 这种方式比 require(..., "Long String") 便宜很多
            revert InsufficientBalance({
                balance: bal,
                withdrawAmount: _withdrawAmount
            });
        }
    }
}
```

这是一个示例

```Solidity
contract Account {
    uint256 public balance;
    // MAX_UINT 是 uint256 能存的最大数字
    uint256 public constant MAX_UINT = 2 ** 256 - 1;

    function deposit(uint256 _amount) public {
        uint256 oldBalance = balance;
        uint256 newBalance = balance + _amount;

        // 【前置检查】防止溢出 (Overflow)
        // 这里的逻辑是：如果加法溢出，结果会变小。
        // (注：Solidity 0.8.0 之后自带溢出检查，这行代码主要是演示逻辑)
        require(newBalance >= oldBalance, "Overflow");

        balance = newBalance;

        // 【后置检查】不变量检查
        // 存款后，新余额一定要大于等于旧余额，这是绝对真理。
        // 如果这行报错，说明上面的加法逻辑有严重漏洞。
        assert(balance >= oldBalance);
    }

    function withdraw(uint256 _amount) public {
        uint256 oldBalance = balance;

        // 【前置检查】防止下溢 (Underflow) 和透支
        require(balance >= _amount, "Underflow");

        // 这里展示了 if + revert 的写法，效果同上
        if (balance < _amount) {
            revert("Underflow");
        }

        balance -= _amount;

        // 【后置检查】
        // 取款后，余额肯定变少了。
        assert(balance <= oldBalance);
    }
}
```

### Function Modifier

修饰符是可以运行在函数调用之前和/或之后的代码。

修饰符可用于：

-   Restrict access 限制访问
    
-   Validate inputs 验证输入
    
-   Guard against reentrancy hack 防范重入攻击
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract FunctionModifier {
    // 我们将使用这些变量来演示修饰器是如何工作的
    address public owner;
    uint256 public x = 10;
    bool public locked;

    constructor() {
        // 将部署合约的人（交易发送者）设置为合约的 owner
        owner = msg.sender;
    }

    // 【修饰器 1：权限控制】
    // 检查调用者是否是合约的 owner
    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        // 【关键点】：下划线 _ 是修饰器特有的符号
        // 它告诉 Solidity：“请把被修饰函数的代码粘贴到这里来执行”
        _;
    }

    // 【修饰器 2：参数校验】
    // 修饰器也可以接收参数。这里检查传入的地址不是 0 地址。
    modifier validAddress(address _addr) {
        require(_addr != address(0), "Not valid address");
        _;
    }

    // 使用修饰器的函数
    // 这里同时使用了两个修饰器：
    // 1. 先执行 onlyOwner 检查身份
    // 2. 再执行 validAddress 检查参数
    // 3. 都通过了，才执行 changeOwner 的大括号里的代码
    function changeOwner(address _newOwner)
        public
        onlyOwner
        validAddress(_newOwner)
    {
        owner = _newOwner;
    }

    // 【修饰器 3：防重入锁】
    // 修饰器可以在函数执行【前】和【后】都运行代码。
    // 这个修饰器防止函数在仍在执行时被再次调用（防重入）。
    modifier noReentrancy() {
        // --- 函数执行前 (Before) ---
        require(!locked, "No reentrancy");
        locked = true;
        
        // --- 插入原本的函数逻辑 ---
        _;
        
        // --- 函数执行后 (After) ---
        locked = false;
    }

    // 这是一个会触发递归调用的函数，用来测试锁是否生效
    function decrement(uint256 i) public noReentrancy {
        x -= i;
        if (i > 1) {
            // 这里尝试再次调用自己
            // 如果没有 noReentrancy，这会成功递归
            // 但因为有了锁，这里会报错 revert，因为 locked 还是 true
            decrement(i - 1);
        }
    }
}
```

前面也基本上看了，没什么好说的，过

### Events

`Events` 允许记录到以太坊区块链。events 的一些用例包括：

-   监听事件并更新用户界面
    
-   一种廉价的存储形式
    

-   主要是为了**通知前端**更新 UI，或者作为**历史数据存档**（因为比 Storage 便宜太多）。
    
-   `indexed`**：** 相当于数据库的索引，最多 3 个，用来加速链下查询。
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Event {
    // 【定义事件】
    // 声明一个叫 Log 的事件。
    // indexed 关键字：最多可以有 3 个参数被标记为 indexed。
    // 作用：标记为 indexed 的参数，允许你在链外进行“搜索”和“过滤”（比如：只查关于某个特定用户的日志）。
    event Log(address indexed sender, string message);

    // 声明另一个没有参数的事件
    event AnotherLog();

    function test() public {
        // 【触发事件】
        // 使用 emit 关键字来发送事件。
        // 这相当于在区块链上打印了一张“收据”。
        emit Log(msg.sender, "Hello World!");

        // 再发一次
        emit Log(msg.sender, "Hello EVM!");

        // 触发那个没参数的事件
        emit AnotherLog();
    }
}
```

**"A cheap form of storage"**。

-   **State Storage (变量):** 比如 `uint256 public x = 10;`。
    
    -   存在哪里？以太坊的“状态树 (State Trie)”里。
        
    -   特点：所有节点都要实时维护它，合约可以随时读取它。
        
    -   **价格：** 极贵（每 32 字节 20,000 Gas）。
        
-   **Events (日志):** 比如 `emit Log(...)`。
    
    -   存在哪里？交易的\*\*“收据树 (Receipt Trie)”\*\*里。
        
    -   特点：它只是作为历史记录存在区块里，**合约自己读不到它**（一旦发出，泼水难收）。
        
    -   **价格：** 极便宜（每字节只需 8 Gas，外加基础费）。
        

**智能合约无法读取自己（或别人）发出的事件！**

```Solidity
function tryToReadLog() public {
    emit Log(msg.sender, "Hello");
    // ❌ 下面这行代码是不存在的！Solidity 没有这种语法！
    // string memory m = readLog(msg.sender); 
}
```

-   **Events 是单向的：** 从链上 -> 发射到 -> 链下。
    
-   如果你需要在合约内部使用数据，必须用 **Storage** 存起来。
    
-   如果你只需要告诉前端“我完事了”，或者为了将来数据分析（比如统计由于历史交易量），用 **Events**。
    

### Events Advanced

在 Solidity 中，事件是一种强大的工具，可以实现各种高级功能和架构。事件的一些高级使用案例包括：

-   事件过滤和监控以实现实时更新和分析
    
-   事件日志分析和解码，用于数据提取和处理
    
-   去中心化应用（dApps）的事件驱动架构
    
-   事件订阅，用于实时通知和更新
    

`EventDrivenArchitecture` 合约展示了一种事件驱动架构，其中事件用于协调和触发流程的不同阶段，例如发起和确认转账。

`EventSubscription` 合约展示了如何实现事件订阅，允许外部合约或客户端在事件发出时订阅并接收实时更新。它还展示了如何处理事件订阅和管理订阅生命周期。

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.14;

// --- 1. 事件驱动架构示例 ---
contract EventDrivenArchitecture {
    // 定义两个事件，分别对应流程的两个阶段
    event TransferInitiated(
        address indexed from, address indexed to, uint256 value
    );
    event TransferConfirmed(
        address indexed from, address indexed to, uint256 value
    );

    // 用于记录转账确认状态的 Mapping
    mapping(bytes32 => bool) public transferConfirmations;

    // 第一步：发起转账
    function initiateTransfer(address to, uint256 value) public {
        // 发出“发起”事件，后端或索引器监听到后会进行记录
        emit TransferInitiated(msg.sender, to, value);
        // ... (此处省略具体的发起逻辑，如冻结资金)
    }

    // 第二步：确认转账
    function confirmTransfer(bytes32 transferId) public {
        require(
            !transferConfirmations[transferId], "Transfer already confirmed"
        );
        transferConfirmations[transferId] = true;
        // 发出“确认”事件，标志流程结束
        emit TransferConfirmed(msg.sender, address(this), 0);
        // ... (此处省略确认逻辑，如实际划转资金)
    }
}

// --- 2. 事件订阅与实时更新示例 ---

// 定义一个接口，订阅者合约必须实现这个接口才能接收通知
interface IEventSubscriber {
    function handleTransferEvent(address from, address to, uint256 value)
        external;
}

contract EventSubscription {
    event LogTransfer(address indexed from, address indexed to, uint256 value);
    
    // 记录谁订阅了 (防止重复订阅)
    mapping(address => bool) public subscribers;
    // 订阅者列表 (用于遍历通知)
    address[] public subscriberList;

    // 订阅函数
    function subscribe() public {
        require(!subscribers[msg.sender], "Already subscribed");
        subscribers[msg.sender] = true;
        subscriberList.push(msg.sender); // 加入列表
    }

    // 取消订阅函数
    function unsubscribe() public {
        require(subscribers[msg.sender], "Not subscribed");
        subscribers[msg.sender] = false;
        
        // 【关键技巧】从数组中删除元素的低 Gas 方法：Swap and Pop
        for (uint256 i = 0; i < subscriberList.length; i++) {
            if (subscriberList[i] == msg.sender) {
                // 1. 把最后一个元素移到当前要删除的位置
                subscriberList[i] = subscriberList[subscriberList.length - 1];
                // 2. 删除最后一个元素（缩短数组长度）
                subscriberList.pop();
                break;
            }
        }
    }

    // 触发转账并通知所有订阅者
    function transfer(address to, uint256 value) public {
        // 1. 发出常规日志
        emit LogTransfer(msg.sender, to, value);
        
        // 2. 链上广播：遍历列表，主动调用订阅者合约的函数
        for (uint256 i = 0; i < subscriberList.length; i++) {
            // 强制转换地址为接口类型，并调用处理函数
            IEventSubscriber(subscriberList[i]).handleTransferEvent(
                msg.sender, to, value
            );
        }
    }
}
```

-   为启用高效的过滤和搜索，应索引正确的事件参数。地址通常应被索引，而金额一般不应被索引。
    
-   避免冗余事件，不要发出已被底层库或合约覆盖的事件。
    
-   事件不能在 `view` 或 `pure` 函数中使用，因为它们通过存储日志来改变区块链状态。
    
-   注意发出事件相关的 gas 成本，尤其是在索引参数时，因为它会影响合约的整体 gas 消耗。
    

### Constructor

构造函数（ `constructor` ）是在合约创建时执行的可选函数。

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 基类合约 X (Base contract X)
contract X {
    string public name;

    // 构造函数：需要传入一个 _name 参数
    constructor(string memory _name) {
        name = _name;
    }
}

// 基类合约 Y (Base contract Y)
contract Y {
    string public text;

    // 构造函数：需要传入一个 _text 参数
    constructor(string memory _text) {
        text = _text;
    }
}

// 【方法 1：静态传参】
// 有两种初始化父合约的方法。
// 第一种：在继承列表（inheritance list）中直接把参数写死。
// B 继承自 X 和 Y，并且在定义时就给出了参数。
contract B is X("Input to X"), Y("Input to Y") {}

// 【方法 2：动态传参】
contract C is X, Y {
    // 第二种：在子合约的构造函数中传递参数，
    // 写法类似于函数修饰器 (modifier)。
    // 这里 C 的构造函数接收参数，然后把它们“透传”给 X 和 Y。
    constructor(string memory _name, string memory _text) X(_name) Y(_text) {}
}

// 【关于执行顺序的重要规则】
// 父合约构造函数总是按照“继承列表”中的声明顺序被调用。
// 无论你在子合约构造函数里怎么写（先写 X 还是先写 Y），都不影响这个顺序。

// 这里的继承顺序是：is X, Y
// 所以构造函数调用顺序是：
// 1. X
// 2. Y
// 3. D
contract D is X, Y {
    // 即使这里先写了 X(...) 再写 Y(...)，顺序依然由 `is X, Y` 决定
    constructor() X("X was called") Y("Y was called") {}
}

// 这里的继承顺序依然是：is X, Y
// 所以构造函数调用顺序依然是：
// 1. X
// 2. Y
// 3. E
contract E is X, Y {
    // 关键点：哪怕你在这里先写了 Y(...) 后写 X(...)，
    // 真实的执行顺序依然是先 X 后 Y！
    constructor() Y("Y was called") X("X was called") {}
}
```

### Inheritance 继承

Solidity 支持多重继承。合约可以通过使用 `is` 关键字来继承其他合约。

被子合约重写的函数必须声明为 `virtual` 。

重写父函数的函数必须使用 `override` 关键字。

继承顺序很重要。

你必须按照从“最基础”到“最派生”的顺序列出父合约。

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

/* 继承关系图 (Graph of inheritance)
    A
   / \
  B   C
 / \ /
F  D,E
*/

// 基类合约 A
contract A {
    // 【关键点 1】virtual
    // 如果一个函数将来打算被子合约修改（重写），必须标记为 virtual。
    function foo() public pure virtual returns (string memory) {
        return "A";
    }
}

// 合约使用 'is' 关键字来继承其他合约
contract B is A {
    // 【关键点 2】override
    // 重写父类 A 的 foo 函数。必须加上 override 关键字。
    function foo() public pure virtual override returns (string memory) {
        return "B";
    }
}

contract C is A {
    // 重写 A 的 foo 函数
    function foo() public pure virtual override returns (string memory) {
        return "C";
    }
}

// 【关键点 3】多重继承 (Multiple Inheritance)
// 合约可以继承多个父合约。
// 当同一个函数在多个父合约中都被定义时，父合约的搜索顺序是：
// 从右向左 (Right to Left)，并且是深度优先 (Depth-first) 的。

contract D is B, C {
    // D.foo() 会返回 "C"
    // 因为在继承列表 `is B, C` 中，C 在最右边 (Right-most)。
    // 所以 C 的 foo() 会覆盖 B 的 foo()。
    
    // override(B, C) 表示：我要重写的这个函数，在 B 和 C 里都有份。
    function foo() public pure override(B, C) returns (string memory) {
        // super.foo() 会调用继承链上的“下一个”合约，这里就是 C
        return super.foo();
    }
}

contract E is C, B {
    // E.foo() 会返回 "B"
    // 因为在继承列表 `is C, B` 中，B 在最右边。
    function foo() public pure override(C, B) returns (string memory) {
        return super.foo();
    }
}

// 【关键点 4】继承顺序 (Inheritance Order)
// 继承列表必须按照从“最基类 (Base)”到“最派生类 (Derived)”的顺序排列。
// A 是爷爷，B 是爸爸。你必须先写 A，再写 B。
// 如果写成 `contract F is B, A`，编译器会报错。
contract F is A, B {
    function foo() public pure override(A, B) returns (string memory) {
        return super.foo();
    }
```

**有** `virtual` **标签的函数，被继承时是必须要 override 的吗？不是必须的。** 只要父类的这个函数有“函数体”（即大括号 `{ ... }` 里面有代码），子类就可以选择不重写。

-   _只有一种情况是必须重写的：_ 如果父类函数**没有函数体**（分号结尾），那么子类**必须**实现它（除非子类也是抽象合约）。
    

**如果不 override 能否直接使用父类的同名函数？完全可以，而且是自动拥有的。** 子合约会自动继承父类的逻辑，就像这行代码是写在子合约里一样。

### Shadowing Inherited State Variables 继承状态变量的遮蔽

与函数不同，状态变量不能通过在子合约中重新声明来覆盖。

简单来说，它的核心规则是：**你不能在子合约里再定义一个和父合约同名的变量，你应该直接修改父合约的那个变量。**

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract A {
    string public name = "Contract A";

    function getName() public view returns (string memory) {
        return name;
    }
}

// 【错误示范】
// Solidity 0.6 版本之后，禁止遮蔽（Shadowing）状态变量。
// 下面的代码无法通过编译。
// contract B is A {
//     // 错误：父合约 A 已经有一个叫 'name' 的变量了。
//     // 你不能在这里重新声明一个 string public name。
//     string public name = "Contract B";
// }

contract C is A {
    // 【正确示范】
    // 这是覆盖继承状态变量的正确方式。
    // 不要重新声明，而是通过构造函数去修改它的值。
    constructor() {
        // 直接给父类已经定义好的 name 赋值
        name = "Contract C";
    }

    // 调用 C.getName() 将返回 "Contract C"
}
```

### Calling Parent Contracts 调用父合约

父合约可以直接调用，或使用关键字 `super` 。

使用关键字 `super` ，所有直接父合约都会被调用。

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

/* 继承树结构 (Graph of inheritance)
    A
   / \
  B   C
   \ /
    D
*/

contract A {
    // 这是一个事件。你可以从函数中发出事件，它们会被记录在交易日志中。
    // 在本例中，这对于追踪函数调用路径非常有用。
    event Log(string message);

    function foo() public virtual {
        emit Log("A.foo called");
    }

    function bar() public virtual {
        emit Log("A.bar called");
    }
}

contract B is A {
    function foo() public virtual override {
        emit Log("B.foo called");
        // 【方式 1：直接调用】指名道姓地调用 A 的 foo
        A.foo();
    }

    function bar() public virtual override {
        emit Log("B.bar called");
        // 【方式 2：Super 调用】调用继承链上的“上一级”
        super.bar();
    }
}

contract C is A {
    function foo() public virtual override {
        emit Log("C.foo called");
        // 【方式 1：直接调用】指名道姓地调用 A 的 foo
        A.foo();
    }

    function bar() public virtual override {
        emit Log("C.bar called");
        // 【方式 2：Super 调用】调用继承链上的“上一级”
        super.bar();
    }
}

// D 继承了 B 和 C (注意顺序：is B, C)
contract D is B, C {
    // 尝试操作：
    // - 调用 D.foo 并检查交易日志。
    //   虽然 D 继承了 A, B 和 C，但它只调用了 C，然后是 A。
    // - 调用 D.bar 并检查交易日志。
    //   D 调用了 C，然后 C 的 super 调到了 B，最后 B 的 super 调到了 A。
    //   虽然 super 被调用了两次（在 B 和 C 中），但 A 只被调用了一次。

    function foo() public override(B, C) {
        // 这里的 super 指向 C (因为 C 在继承列表的最右边)
        super.foo();
    }

    function bar() public override(B, C) {
        // 这里的 super 指向 C
        super.bar();
    }
}
```

子类能直接调用 爷类 的接口

比如说上面的代码中的 D 能直接使用 A 的接口，只需要写 A.foo() 即可

### Visibility 可见性

这个前面基本上也看过了

**函数的可见性可以是：**

-   `public` **(公开)**：任何合约、任何账号（EOA）都可以调用。
    
-   `private` **(私有)**：**最严格**。只有**定义该函数**的合约内部可以调用（子合约也不行）。
    
-   `internal` **(内部)**：只有合约**内部**以及**继承该合约的子合约**可以调用。
    
-   `external` **(外部)**：只有**其他合约**和**账号**可以调用（合约内部不能直接调用，必须用特殊手段）。
    

**状态变量的可见性：**

-   可以是 `public`, `private`, 或 `internal`。
    
-   **注意：** 状态变量**不能**定义为 `external`。
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Base {
    // 【Private / 私有】
    // 权限：仅限本合约内部。
    // 限制：继承了这个合约的子合约（Child）也无法调用它。
    function privateFunc() private pure returns (string memory) {
        return "private function called";
    }

    // 本合约内部可以调用 private 函数
    function testPrivateFunc() public pure returns (string memory) {
        return privateFunc();
    }

    // 【Internal / 内部】
    // 权限：本合约内部 + 子合约内部。
    // 类似于 Java/C++ 中的 protected。
    function internalFunc() internal pure returns (string memory) {
        return "internal function called";
    }

    function testInternalFunc() public pure virtual returns (string memory) {
        return internalFunc();
    }

    // 【Public / 公开】
    // 权限：所有人。内部、子合约、外部账号、外部合约都能调。
    // 如果是状态变量设为 public，Solidity 会自动为你生成一个 getter 函数。
    function publicFunc() public pure returns (string memory) {
        return "public function called";
    }

    // 【External / 外部】
    // 权限：仅限外部调用。
    // 场景：通常用于只给用户调用的接口，省 Gas（参数直接从 calldata 读，不复制到 memory）。
    function externalFunc() external pure returns (string memory) {
        return "external function called";
    }

    // ❌ 编译错误演示：
    // 这是一个 public 函数（属于内部调用的一种），它不能直接调用 external 函数。
    // function testExternalFunc() public pure returns (string memory) {
    //     return externalFunc(); // 报错！
    //     // 修正方法：使用 this.externalFunc() 发起一个外部调用（极费 Gas，不推荐）
    // }

    // 【状态变量】
    string private privateVar = "my private variable";
    string internal internalVar = "my internal variable";
    string public publicVar = "my public variable";

    // ❌ 错误：状态变量不能是 external
    // string external externalVar = "my external variable";
}

contract Child is Base {
    // ❌ 错误演示：
    // 子合约无法访问父合约的 private 函数或变量
    // function testPrivateFunc() public pure returns (string memory) {
    //     return privateFunc();
    // }

    // ✅ 正确：
    // 子合约可以访问父合约的 internal 函数
    function testInternalFunc() public pure override returns (string memory) {
        return internalFunc();
    }
}
```

误区 1：`private` 的数据是保密的吗？

**绝对不是！** 在区块链上，`private` 只是**代码层面的访问限制**。

-   **代码限制：** 别的合约写代码调不到你的 `private` 变量。
    
-   **数据公开：** 黑客、普通用户通过区块链浏览器、或者直接读取节点的 Storage 槽位，**可以轻而易举地读出** `private` **变量的值**。
    
-   **警示：** 永远不要用 `private` 变量存密码、私钥！
    

误区 2：`public` 和 `external` 怎么选？

-   如果你的函数**只会被外部**（用户或别的合约）调用，自己合约内部不调，**请务必用** `external`。
    
-   **原因：** 处理大数组参数时，`external` 直接从 Calldata（原始数据区）读取，而 `public` 需要把数据从 Calldata 复制到 Memory（内存区）。`external` **更省 Gas。**
    

误区 3：状态变量默认是什么？

如果你写 `uint256 x;` 没加修饰符：

-   **状态变量** 默认为 `internal`。
    
-   **函数** 没有默认值，必须显式声明可见性（Solidity 0.5.0 之后的强制要求）。
    

### Interface 接口

-   不能包含任何函数实现（只有函数名、参数、返回值，没有大括号 `{...}` 里的逻辑）。
    
-   可以继承其他的接口（接口也能有父接口）。
    
-   所有声明的函数必须是 `external` 的（因为接口本来就是给外部用的）。
    
-   不能声明构造函数（因为它只是个外壳，不需要部署初始化）。
    
-   不能声明状态变量（它不存数据，只定义行为）。
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 1. 目标合约：这是真正干活的合约
contract Counter {
    uint256 public count;

    function increment() external {
        count += 1;
    }
}

// 2. 接口定义：这是 Counter 合约的“说明书”
// 我们不需要知道 increment 里面具体是怎么写的（是 +=1 还是 +=10），
// 我们只需要知道它叫什么名字，怎么调用。
interface ICounter {
    // 对应 Counter 中的 public count 变量
    // (Public 变量会自动生成一个同名的 getter 函数)
    function count() external view returns (uint256);

    // 对应 Counter 中的 increment 函数
    function increment() external;
}

// 3. 调用者合约
contract MyContract {
    // 传入目标合约的地址 _counter
    function incrementCounter(address _counter) external {
        // 【关键语法】: ICounter(_counter)
        // 意思是：把这个地址 (_counter) 当作 ICounter 接口来看待。
        // 然后调用它的 increment() 按钮。
        ICounter(_counter).increment();
    }

    function getCount(address _counter) external view returns (uint256) {
        // 同样，把地址包装成接口，调用 count() 读取数据
        return ICounter(_counter).count();
    }
}

// Uniswap 工厂合约的接口
// 作用：通过两个代币地址，找到它们对应的交易对(Pair)地址
interface UniswapV2Factory {
    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);
}

// Uniswap 交易对合约的接口
// 作用：查询这个池子里有多少币（流动性储备）
interface UniswapV2Pair {
    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

contract UniswapExample {
    // 这些是 Ethereum 主网上真实的合约地址
    address private factory = 0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6f;
    address private dai = 0x6B175474E89094C44Da98b954EedeAC495271d0F;
    address private weth = 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2;

    function getTokenReserves() external view returns (uint256, uint256) {
        // 第一步：问 Factory，“DAI 和 WETH 的交易池地址在哪？”
        // 语法：接口名(合约地址).函数名(参数)
        address pair = UniswapV2Factory(factory).getPair(dai, weth);

        // 第二步：问 Pair 合约，“你池子里现在有多少 DAI 和 WETH？”
        // 语法：(返回值1, 返回值2, 忽略的值) = 接口调用()
        (uint256 reserve0, uint256 reserve1,) = UniswapV2Pair(pair).getReserves();
        // 注意上面那个逗号 ","，表示我们不需要第三个返回值 (时间戳)，直接丢弃
        return (reserve0, reserve1);
    }
}
```

### Payable

声明为 `payable` 的函数和地址可以接收 `ether` 进入合约。

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Payable {
    // 【可支付地址】
    // 只有标记为 address payable 的变量，才能使用 .transfer() 或 .send() 方法给它转账
    address payable public owner;

    // 【可支付构造函数】
    // 加上 payable，意味着你在部署合约的那一刻，就可以往合约里存入一笔初始资金
    constructor() payable {
        // msg.sender 是部署者，我们需要把它强制转换为 payable 类型存起来
        owner = payable(msg.sender);
    }

    // 【存款函数】
    // 这个函数可以接收以太币 (Ether)。
    // 调用此函数时，附带发送一些 Ether，合约的余额会自动更新。
    // 函数体是空的 {}，但这没关系，Solidity 引擎会自动处理记账。
    function deposit() public payable {}

    // 【非支付函数】
    // 如果你调用这个函数的同时尝试发送 Ether，交易会直接失败 (Revert)。
    // 因为它没有 payable 标签。
    function notPayable() public {}

    // 【提款函数】
    // 将合约里所有的 Ether 取出来发给 owner
    function withdraw() public {
        // 获取当前合约里的余额 (address(this).balance)
        uint256 amount = address(this).balance;

        // 发送 Ether 给 owner
        // 这是目前推荐的转账写法：call{value: ...}("")
        (bool success,) = owner.call{value: amount}("");
        require(success, "Failed to send Ether");
    }

    // 【转账函数】
    // 从合约转钱给指定的某个地址
    function transfer(address payable _to, uint256 _amount) public {
        // 注意：参数 _to 必须声明为 payable，否则不能给它转账
        (bool success,) = _to.call{value: _amount}("");
        require(success, "Failed to send Ether");
    }
}
```

### Sending Ether: Transfer, Send, Call

You can send Ether to other contracts by

-   `transfer` (2300 gas, throws error)
    
-   `send` (2300 gas, returns bool)
    
-   `call` (forward all gas or set gas, returns bool)
    

**How to send Ether? (如何发送？)** 你可以通过以下方式发送 ETH 给其他合约：

1.  `transfer`: 限制 **2300 gas**，失败时直接**抛出异常 (revert)**。（已过时，不推荐）
    
2.  `send`: 限制 **2300 gas**，失败时**返回** `false`（不会自动 revert）。（已过时，不推荐）
    
3.  `call`: **转发剩余所有 gas**（或者你可以指定 gas），失败时**返回** `false`。（**当前推荐的标准做法**）
    

**How to receive Ether? (如何接收？)** 一个合约想要接收 ETH，必须至少包含以下函数之一：

-   `receive() external payable`
    
-   `fallback() external payable`
    

**逻辑判断：** 如果 `msg.data` 为空，优先调用 `receive()`；否则（或者 `receive` 不存在时），调用 `fallback()`。

**Which method should you use? (应该用哪个？)**

-   **自 2019 年 12 月起，推荐使用** `call` **与重入保护机制结合使用。**
    
-   **通过重入保护机制来防范重入攻击。**
    
    -   **在进行其他合约调用之前进行所有状态变更**
        
    -   使用 `nonReentrant` 重入保护修饰符。
        

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 引入 Solady 库（这是一个追求极致 Gas 优化的库，比 OpenZeppelin 更省钱）
import {SafeTransferLib} from "@solady/utils/SafeTransferLib.sol";

// =====================
// 1. 接收 ETH 的合约
// =====================
contract ReceiveEther {
    /*
    接收逻辑图：
                 发送 Ether
                    |
              msg.data 为空?
                 /      \
               是        否
              /           \
        receive()存在?   fallback()
           /     \
          是      否
         /         \
    receive()    fallback()
    */

    // 情况 A: 纯转账，msg.data 为空
    receive() external payable {}

    // 情况 B: msg.data 不为空，或者 receive 不存在
    fallback() external payable {}

    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
}

// =====================
// 2. 发送 ETH 的合约
// =====================
contract SendEther {
    // 【流派 1: Transfer (不推荐)】
    // 特点：自带 2300 gas 限制。
    // 缺点：如果目标合约的 receive 函数逻辑稍微复杂一点（消耗超过 2300 gas），转账就会失败。
    // 这种“写死 Gas”的做法在以太坊升级后很容易出问题。
    function sendViaTransfer(address payable _to) public payable {
        _to.transfer(msg.value);
    }

    // 【流派 2: Send (不推荐)】
    // 特点：自带 2300 gas 限制，返回布尔值。
    // 缺点：你需要手动处理失败情况 (require)，且同样受 Gas 限制困扰。
    function sendViaSend(address payable _to) public payable {
        bool sent = _to.send(msg.value);
        require(sent, "Failed to send Ether");
    }

    // 【流派 3: Call (官方推荐)】
    // 特点：转发所有剩余 Gas，返回布尔值和数据。
    // 优点：最灵活，兼容性最好（特别是像 Gnosis Safe 这种智能合约钱包，往往需要消耗更多 Gas 才能收钱）。
    // 风险：因为转发了所有 Gas，对方可能会利用剩余 Gas 回头攻击你（重入攻击）。所以必须配合“重入锁”使用。
    function sendViaCall(address payable _to) public payable {
        // 语法：地址.call{value: 金额}("")
        (bool sent, bytes memory data) = _to.call{value: msg.value}("");
        require(sent, "Failed to send Ether");
    }

    // 【流派 4: Solady (高手专用)】
    // 使用 Solady 的 SafeTransferLib
    // 优点：比原生的 .call 写法还要省 Gas（约 0.36%），且代码更简洁。
    // 它在底层用汇编 (Yul) 处理了返回值检查。
    function sendViaSolady(address payable _to) public payable {
        // 如果失败，会自动 Revert 并抛出 ETHTransferFailed 错误
        SafeTransferLib.safeTransferETH(_to, msg.value);
    }
}
```

### Fallback 回退函数

`fallback` **是一个特殊的函数，它会在以下两种情况被执行：**

1.  **调用了不存在的函数：** 别人发来一个指令，但合约里没有匹配的函数名。
    
2.  **直接发 ETH 且无法由** `receive` **处理：**
    
    1.  别人直接转账，但合约里没写 `receive()` 函数；
        
    2.  **或者** 别人转账的同时还带了数据 (`msg.data` 不为空)。
        

**Gas 限制：** 当使用 `transfer` 或 `send` 调用 `fallback` 时，它只有 **2300 gas** 的限制（这非常少，只能做最简单的日志记录，连写入状态变量都不够）。

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Fallback {
    event Log(string func, uint256 gas);

    // 【Fallback 函数】
    // 必须声明为 external。
    // 如果想让它能接收以太币，必须加 payable。
    fallback() external payable {
        // 这里的 gasleft() 可以让我们看到还剩多少 Gas。
        // - 如果是用 transfer/send 调用的：这里剩余 Gas 很少（约 2300）。
        // - 如果是用 call 调用的：这里剩余 Gas 很多（几乎全部）。
        emit Log("fallback", gasleft());
    }

    // 【Receive 函数】
    // 专门用于接收纯 ETH 转账（msg.data 为空时触发）。
    receive() external payable {
        emit Log("receive", gasleft());
    }

    // 辅助函数：查看合约余额
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
}

contract SendToFallback {
    // 使用 transfer 发送 ETH
    function transferToFallback(address payable _to) public payable {
        // 【关键点】
        // transfer 自带 2300 gas 限制。
        // 如果目标合约的 fallback/receive 逻辑太复杂（比如写入了 Storage），
        // 消耗超过了 2300，这笔交易会直接失败 (Revert)。
        _to.transfer(msg.value);
    }

    // 使用 call 发送 ETH
    function callFallback(address payable _to) public payable {
        // 【关键点】
        // call 会转发所有剩余的 Gas。
        // 这是推荐的发送方式，因为它允许目标合约执行复杂的逻辑。
        (bool sent,) = _to.call{value: msg.value}("");
        require(sent, "Failed to send Ether");
    }
}
```

### Call 调用

`call` **是一个用于与其他智能合约交互的低级函数。**

-   **推荐场景：** 当你仅仅是想发送 ETH（触发对方的 `fallback` 或 `receive` 函数）时，这是推荐的方法。
    
-   **不推荐场景：** 这通常**不是**调用现有（已知）函数的推荐方式。
    

**为什么不推荐使用低级 call 来调用函数？**

1.  **Reverts are not bubbled up (异常不冒泡):** 如果目标合约报错了（revert），调用者合约**不会**自动报错停止，它只会收到一个 `false` 的返回值。你需要手动写代码去检查这个失败。
    
2.  **Type checks are bypassed (跳过类型检查):** 编译器不会帮你检查参数类型是否匹配（比如你传了 `uint` 给 `string`，编译时不报错，运行时才出错）。
    
3.  **Function existence checks are omitted (省略函数存在性检查):** 如果你调用的函数根本不存在，编译器也不管，依然会发送交易，通常会触发对方的 `fallback`。
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Receiver {
    event Received(address caller, uint256 amount, string message);

    // 如果有人瞎调函数（函数名对不上），或者只发钱不带数据，就进这里
    fallback() external payable {
        emit Received(msg.sender, msg.value, "Fallback was called");
    }

    // 这是一个正常的业务函数
    function foo(string memory _message, uint256 _x)
        public
        payable
        returns (uint256)
    {
        emit Received(msg.sender, msg.value, _message);
        return _x + 1;
    }
}

contract Caller {
    event Response(bool success, bytes data);

    // 假设 Caller 没有 Receiver 的源代码（也就无法使用接口 Interface），
    // 但我们知道 Receiver 的地址，以及我们要调用的函数名叫 "foo"。
    function testCallFoo(address payable _addr) public payable {
        // 【核心语法】
        // 1. {value: ..., gas: ...} : 自定义发送的 ETH 金额和 Gas 限制
        // 2. abi.encodeWithSignature(...) : 将函数名和参数手动打包成二进制数据
        (bool success, bytes memory data) = _addr.call{
            value: msg.value,
            gas: 5000
        }(abi.encodeWithSignature("foo(string,uint256)", "call foo", 123));
        
        // 注意：abi.encodeWithSignature 对空格非常敏感！
        // "foo(string, uint256)" ❌ (逗号后有空格，会导致计算出的 Hash 不对)
        // "foo(string,uint256)" ✅ (必须紧凑)

        emit Response(success, data);
    }

    // 演示调用一个不存在的函数
    function testCallDoesNotExist(address payable _addr) public payable {
        // 试图调用 "doesNotExist()"
        (bool success, bytes memory data) = _addr.call{value: msg.value}(
            abi.encodeWithSignature("doesNotExist()")
        );

        // 结果：
        // success 会是 true (只要对方没 revert)
        // 对方合约会触发 fallback 函数
        emit Response(success, data);
    }
}
```

### Delegatecall 委托调用

`delegatecall` 是一个类似于 `call` 的低级函数。

当合约 A 对合约 B 执行 `delegatecall` 时，**B 的代码**会被执行，但是是在 **A 的上下文**中执行的。 这意味着：

1.  修改的是 **A 的 Storage (存储)**。
    
2.  `msg.sender` 依然是 **触发 A 的那个原始调用者**（而不是 A）。
    
3.  `msg.value` 依然是 **原始交易发送的金额**。
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 注意：先部署这个合约 (逻辑合约 / 老师)
contract B {
    // 注意：这里的存储布局必须和合约 A 一模一样！
    uint256 public num;
    address public sender;
    uint256 public value;

    function setVars(uint256 _num) public payable {
        // 当 A delegatecall 这里时：
        // 1. num 修改的是 A 的 num
        num = _num;
        // 2. sender 是谁？是调用 A 的那个人 (user)，而不是 A
        sender = msg.sender;
        // 3. value 是多少？是 user 发给 A 的钱
        value = msg.value;
    }
}

// 这是一个代理合约 (Proxy / 学生)
contract A {
    // 存储布局必须和 B 保持一致 (Slot 0, Slot 1, Slot 2)
    uint256 public num;
    address public sender;
    uint256 public value;

    function setVars(address _contract, uint256 _num) public payable {
        // 【核心代码】
        // A 调用 B 的代码，但修改的是 A 自己的状态
        (bool success, bytes memory data) = _contract.delegatecall(
            abi.encodeWithSignature("setVars(uint256)", _num)
        );
        // 如果成功，此时 A.num 变了，A.sender 变了，但 B 的状态纹丝不动。
    }
}
```

代理调用会产生额外 gas 费用

**地址访问成本 (Address Access Cost - EIP-2929):**

-   **冷加载 (Cold Load):** 如果这是你在本笔交易中第一次访问逻辑合约的地址，需要花费约 **2600 Gas**。
    
-   **热加载 (Warm Load):** 如果这个地址在交易中已经被访问过了（或者在预热列表中），只需要 **100 Gas**。
    
-   _注：在代理模式（Proxy Pattern）中，通常每次交易只调用一次逻辑合约，所以通常是冷加载的 2600 Gas。_
    

**内存扩展与数据拷贝 (Memory Expansion & Copying):**

-   你需要把参数从 Calldata 或 Memory 复制一份传给 `delegatecall`。
    
-   如果有返回值，还需要把返回值复制回来。
    
-   这部分取决于你的参数数据有多大（通常几十到几百 Gas）。
    

### Function Selector

当调用一个函数时， `calldata` 的前 4 个字节指定要调用的函数。

这 4 个字节称为函数选择器。

例如，下面这段代码。它使用 `call` 在地址 `addr` 的合约上执行 `transfer` 。

```Solidity
addr.call(abi.encodeWithSignature("transfer(address,uint256)", 0xSomeAddress, 123))
```

从 `abi.encodeWithSignature(....)` 返回的前 4 个字节是函数选择器。

或许可以在代码中预计算并内联函数选择器，从而节省一点 gas？

**优化写法 (省 Gas):** 我们已经知道 `transfer` 的选择器是 `0xa9059cbb`，直接写死它。

直接 `abi.encodeWithSelector` 传入函数的选择器

```Solidity
addr.call(abi.encodeWithSelector(0xa9059cbb, recipient, amount));
```

就是说，encodeWithSelector会把函数名，比如说"transfer(address,uint256)"，直接转化为一个4字节的识别码，用于找到对应的函数

当你向以太坊网络发送一笔交易去调用合约函数时，你发送的数据（Input Data / Calldata）其实是一串长长的十六进制编码。

**结构如下：**`[ 4字节: 函数选择器 ]` + `[ N字节: 参数编码 ]`

### Calling Other Contract 高层级调用 (High-level Call

合约可以通过两种方式调用其他合约。

最简单的方法是直接调用，例如 `A.foo(x, y, z)` 。

另一种调用其他合约的方法是使用低级别的 `call` 。这种方法不推荐使用。

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 1. 被调用方 (Callee)
// 这是我们要去调用的目标合约
contract Callee {
    uint256 public x;
    uint256 public value;

    // 普通函数：修改 x 并返回 x
    function setX(uint256 _x) public returns (uint256) {
        x = _x;
        return x;
    }

    // 可支付函数：既修改 x，又接收 ETH
    function setXandSendEther(uint256 _x)
        public
        payable
        returns (uint256, uint256)
    {
        x = _x;
        value = msg.value; // 记录收到了多少钱
        return (x, value);
    }
}

// 2. 调用方 (Caller)
// 这个合约负责去“戳”上面的合约
contract Caller {
    
    // 【场景 A：直接传入合约类型】
    // 参数 _callee 的类型直接就是合约 Callee
    function setX(Callee _callee, uint256 _x) public {
        // 编译器知道 _callee 里有个函数叫 setX，直接调就行
        uint256 x = _callee.setX(_x);
    }

    // 【场景 B：传入地址，自己转换】（最常用）
    // 通常我们在前端只知道目标合约的地址 (address 0x...)
    function setXFromAddress(address _addr, uint256 _x) public {
        // 关键步骤：强制类型转换
        // 把“光秃秃”的地址 _addr，包装成 Callee 类型的合约实例
        Callee callee = Callee(_addr);
        
        // 现在可以调用了
        callee.setX(_x);
    }

    // 【场景 C：调用函数并附带 ETH】
    // 类似于低级调用的 {value: ...}，高级调用也可以这样写
    function setXandSendEther(Callee _callee, uint256 _x) public payable {
        // 语法：合约对象.函数名{value: 金额}(参数)
        // 这里把 Caller 收到的钱 (msg.value) 转发给了 Callee
        (uint256 x, uint256 value) = 
            _callee.setXandSendEther{value: msg.value}(_x);
    }
}
```

### Contract that Creates other Contracts 通过合约创建合约（**工厂模式**）

通过 `new` 关键字，其他合约可以创建合约。自 0.8.0 版本起， `new` 关键字通过指定 `salt` 选项支持 `create2` 功能。

CREATE 和 CREATE2 的开销差别非常小，通常可以忽略不计

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

//要创建的合约，这是我们要生产的“车”。
contract Car {
    address public owner;
    string public model;
    address public carAddr;

    // 构造函数是 payable 的，意味着你在创建这辆车时，可以顺便塞钱进去
    constructor(address _owner, string memory _model) payable {
        owner = _owner;
        model = _model;
        carAddr = address(this);
    }
}

//工厂
contract CarFactory {
    Car[] public cars; // 用于记录所有生产出来的车的地址

    // 【方式 1: 普通创建 (CREATE)】
    // 使用传统的 `new` 关键字。
    // 新合约地址 = hash(工厂地址, 工厂的 nonce)
    // 这种方式生成的地址难以预测（因为 nonce 会变）。
    function create(address _owner, string memory _model) public {
        Car car = new Car(_owner, _model);
        //car 的类型是 Car ， address(car) 获得相应的 地址
        cars.push(car);
    }

    // 【方式 2: 带资进组】
    // 在创建合约的同时，给新合约转入 ETH。
    // 语法：(new ContractName){value: 金额}(构造函数参数)
    function createAndSendEther(address _owner, string memory _model)
        public
        payable
    {
        // 注意：Car 的构造函数必须是 payable 的，这里才不会报错
        Car car = (new Car){value: msg.value}(_owner, _model);
        cars.push(car);
    }

    // 【方式 3: 确定性创建 (CREATE2)】
    // 使用 `salt` 选项。
    // 新合约地址 = hash(0xFF, 工厂地址, salt, 新合约字节码)
    // 只要 salt 不变，代码不变，生成的地址永远是固定的！
    // 这叫 "Counterfactual instantiation" (反事实实例化)。
    function create2(address _owner, string memory _model, bytes32 _salt)
        public
    {
        // 语法：(new ContractName){salt: 盐值}(构造函数参数)
        Car car = (new Car){salt: _salt}(_owner, _model);
        cars.push(car);
    }

    // 【方式 4: 全家桶 (带钱 + 带 salt)】
    function create2AndSendEther(
        address _owner,
        string memory _model,
        bytes32 _salt
    ) public payable {
        Car car = (new Car){value: msg.value, salt: _salt}(_owner, _model);
        cars.push(car);
    }

    // 辅助函数：读取某辆车的信息
    function getCar(uint256 _index)
        public
        view
        returns (
            address owner,
            string memory model,
            address carAddr,
            uint256 balance
        )
    {
        Car car = cars[_index];
        return (car.owner(), car.model(), car.carAddr(), address(car).balance);
    }
}
```

### try / catch

`try / catch` 只能捕获来自外部函数调用和合约创建的错误。

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 用于演示 try / catch 的外部合约
contract Foo {
    address public owner;

    constructor(address _owner) {
        // 情况 A: 如果 owner 是 0地址，抛出带字符串的错误 (Error)
        require(_owner != address(0), "invalid address");
        // 情况 B: 如果 owner 是 0x...01，抛出 Panic 错误 (assert 产生)
        assert(_owner != 0x0000000000000000000000000000000000000001);
        owner = _owner;
    }

    function myFunc(uint256 x) public pure returns (string memory) {
        // 如果 x 为 0，抛出错误
        require(x != 0, "require failed");
        return "my func was called";
    }
}

contract Bar {
    event Log(string message);
    event LogBytes(bytes data);

    Foo public foo;

    constructor() {
        // 这里创建一个 Foo 实例，用于下面的外部调用演示
        foo = new Foo(msg.sender);
    }

    // 【示例 1：捕获外部函数调用的错误】
    // 输入 0 => 触发 require 报错 => 进入 catch => 输出 Log("external call failed")
    // 输入 1 => 正常运行 => 进入 try 代码块 => 输出 Log("my func was called")
    function tryCatchExternalCall(uint256 _i) public {
        // 语法：try 外部调用 returns (返回值) { ...无报错执行 } catch { ...报错时执行 }
        // 注意：这里用的是 this.foo 或者直接 foo (因为 foo 是外部合约变量)，这属于外部调用
        try foo.myFunc(_i) returns (string memory result) {
            emit Log(result);
        } catch {
            // 这种写法是通用的 catch，不管什么错误都进这里
            emit Log("external call failed");
        }
    }

    // 【示例 2：捕获合约创建（new）时的错误】
    // 输入 address(0) => 触发 require => 进入 catch Error => 输出 "invalid address"
    // 输入 address(1) => 触发 assert  => 进入 catch (bytes) => 输出 原始字节数据
    // 输入 address(2) => 成功创建 => 进入 try 代码块 => 输出 "Foo created"
    function tryCatchNewContract(address _owner) public {
        // 语法：try new 合约(...) returns (实例变量) { ... }
        try new Foo(_owner) returns (Foo foo) {
            // 创建成功，在这里可以使用新创建的 foo 变量
            emit Log("Foo created");
        } catch Error(string memory reason) {
            // 【特定捕获 1】
            // 专门捕获 require(..., "reason") 或 revert("reason") 抛出的错误
            emit Log(reason);
        } catch (bytes memory reason) {
            // 【特定捕获 2】
            // 这是一个“兜底”捕获。
            // 它可以捕获 assert 失败 (Panic)、除以零、或者没有错误信息的 revert。
            emit LogBytes(reason);
        }
    }
}
```

语法：

```Solidity
try 外部调用 returns (返回值) { 
    ...无报错执行 
} catch {
    ...报错时执行 
 }
```

错误类型

| 错误类型 | 触发代码示例 | 对应的 Catch 块 |
| 标准 Error | require(false, "No!"); | catch Error(string memory reason) |
| Panic | assert(false); 或 1/0; | catch Panic(uint256 code) |
| Custom Error | revert MyError(); | catch (bytes memory data) (兜底) |
| Empty Revert | revert(); | catch (bytes memory data) (兜底) |

### Import

Local (本地引用)

这是我们的文件夹结构（假设这两个文件在同一个文件夹下）：

```Solidity
├── Import.sol
└── Foo.sol
```

**文件名：Foo.sol (被导入的文件)**

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 1. 定义一个结构体
struct Point {
    uint256 x;
    uint256 y;
}

// 2. 定义一个自定义错误
error Unauthorized(address caller);

// 3. 定义一个“自由函数”(Free Function)
// 注意：这个函数是在合约外面定义的，不属于任何合约，属于全局作用域
function add(uint256 x, uint256 y) pure returns (uint256) {
    return x + y;
}

// 4. 定义一个合约
contract Foo {
    string public name = "Foo";
}
```

**文件名：Import.sol (执行导入的文件)**

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 【方式 1: 全局导入】
// 导入当前目录下的 Foo.sol 文件中的所有内容
import "./Foo.sol";

// 【方式 2:以此为准的具名导入 (推荐)】
// 只导入你需要的部分，还可以重命名 (Alias)
// 语法: import { 原名 as 新名, 原名 } from "路径";
import {Unauthorized, add as func, Point} from "./Foo.sol";

contract Import {
    // 初始化 Foo 合约
    Foo public foo = new Foo();

    // 测试获取 Foo 的名字
    function getFooName() public view returns (string memory) {
        return foo.name();
    }
}
```

External (外部引用)

你也可以通过直接复制 GitHub 的 URL 来导入文件（通常用于 Remix 编辑器）。

```Solidity
// 语法示例：import "https://github.com/owner/repo/blob/branch/path/to/Contract.sol";

// 示例：从 OpenZeppelin 的代码库导入 ECDSA 库 (release-v4.5 分支)
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v4.5/contracts/utils/cryptography/ECDSA.sol";
```

### 深度解析：三种导入姿势

在 Solidity 开发中，掌握正确的 `import` 姿势非常重要，这关系到代码的整洁度和编译效率。

A. 全局导入 (Global Import)

> `import "./Foo.sol";`

-   **含义：** 把 `Foo.sol` 里所有能导出的东西（合约、函数、结构体、错误）全拿过来。
    
-   **缺点：** 污染命名空间。如果 `Foo.sol` 里有个 `struct Point`，而你的文件里也有个 `struct Point`，就会冲突报错。
    
-   **适用场景：** 快速原型开发，或者文件内容很少时。
    

B. 具名导入 (Named Import) —— **推荐做法**

> `import {Unauthorized, add as func, Point} from "./Foo.sol";`

这是 Solidity 0.7.0 之后引入的现代化写法，模仿了 JS 的 ES6 语法。

-   **精确控制：** 只引入你需要的组件。
    
-   **起别名 (Aliasing)：** 使用 `as` 关键字。
    
    -   例如：`add as func`。在 `Import.sol` 里，你就不叫它 `add` 了，改叫 `func`。
        
    -   **作用：** 完美解决命名冲突。如果两个文件都有 `add` 函数，你可以分别重命名为 `addA` 和 `addB`。
        

C. 外部导入 (External Import)

> `import "https://github.com/..."`

-   **Remix 专属：** 这种直接写网址的方式，主要是在 **Remix IDE** 网页版里使用。Remix 会自动去下载这个文件。
    
-   **本地开发 (Hardhat/Foundry)：** 在本地开发时，我们通常不写网址，而是使用 `npm` 安装库，然后这样导入：
    

```Solidity
// 本地开发标准写法
import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";
```

### 补充知识点：自由函数 (Free Functions)

在 `Foo.sol` 中，你看到了一个奇怪的现象：

```Solidity
function add(uint256 x, uint256 y) pure returns (uint256) { ... }
```

这个函数**没有被包裹在** `contract { ... }` **里面**。

-   这叫 **Top-level Function (顶级函数)** 或 **Free Function**。
    
-   **特点：** 它们不需要实例化合约就可以使用，类似其他语言的静态工具函数。
    
-   **用途：** 常用于数学计算库、辅助工具库。
    

### Library

**核心定义：**

-   库和合约（Contract）很像，但有两大限制：
    
    -   **不能声明任何状态变量**（State Variable）。
        
    -   **不能接收 ETH**。
        

**部署规则（面试考点）：**

-   **内嵌 (Embedded):** 如果库里**所有**函数都是 `internal`（内部的），那么库的代码会直接被**复制粘贴**到调用它的合约里。部署时不需要单独部署库。
    
-   **链接 (Linked):** 如果库里有 `public` 或 `external` 函数，库必须先单独部署。然后，主合约在部署时需要通过“链接”的方式找到这个库的地址。
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

library Math {
    // 【关键点 1】 internal
    // 因为是 internal，这部分代码编译时会直接嵌入到 TestMath 合约中。
    // 不会产生外部调用开销。
    function sqrt(uint256 y) internal pure returns (uint256 z) {
        if (y > 3) {
            z = y;
            uint256 x = y / 2 + 1;
            // 巴比伦法求平方根（牛顿迭代法）
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
        // 默认为 0
    }
}

contract TestMath {
    function testSquareRoot(uint256 x) public pure returns (uint256) {
        // 调用方式：库名.函数名(参数)
        return Math.sqrt(x);
    }
}

// 这是一个用于删除数组元素并重新整理数组的库
library Array {
    // 【关键点 2】 storage 指针
    // 第一个参数是 `uint256[] storage arr`。
    // 这意味着这个函数会直接修改调用者合约里的状态变量（那个真实的数组）。
    function remove(uint256[] storage arr, uint256 index) public {
        require(arr.length > 0, "Can't remove from empty array");
        
        // 【核心算法：Swap and Pop】
        // 目标：删除 index 位置的元素，且节省 Gas。
        // 做法：把数组“最后一个元素”搬过来，覆盖掉“index 位置的元素”，然后把尾巴删掉。
        // 代价：数组的顺序会被打乱（无序删除）。
        
        arr[index] = arr[arr.length - 1]; // 1. 搬运尾部元素覆盖目标
        arr.pop();                        // 2. 删掉尾部
    }
}

contract TestArray {
    // 【关键点 3】 Using 语法糖
    // 意思是：把 Array 库里的功能，应用到 uint256[] 类型的数据上。
    // 效果：原本需要写 Array.remove(arr, 1)，现在可以写 arr.remove(1)。
    // 类似于给 uint256[] 类型“扩展”了新方法。
    using Array for uint256[];

    uint256[] public arr;

    function testArrayRemove() public {
        // 初始化数组 [0, 1, 2]
        for (uint256 i = 0; i < 3; i++) {
            arr.push(i);
        }

        // 调用库函数删除索引为 1 的元素 (也就是数字 1)
        // 过程演示：
        // 初始: [0, 1, 2]
        // 目标: 删除 index 1
        // 搬运: 把末尾的 2 搬到 index 1 -> [0, 2, 2]
        // Pop : 删掉末尾 -> [0, 2]
        arr.remove(1);

        assert(arr.length == 2);
        assert(arr[0] == 0);
        assert(arr[1] == 2); // 注意：原本最后的元素跑到中间来了
    }
}
```

**为什么用库？**

-   **复用代码：** 不要重复造轮子（比如 `SafeMath` 或字符串处理工具）。
    
-   **节省 Gas（针对 Internal）：** 如果代码复用率高，internal 库可以避免多次部署相同逻辑。
    
-   **节省合约大小（针对 Public）：** 传统的合约最大限制是 24KB。如果你把大段逻辑拆分到 public 库里单独部署，主合约只存一个链接地址，就可以突破大小限制。
    

如果 `Array` 库里的函数写的是 `public`，情况就完全变了。这时候它是一个独立的“实体”。

1.  部署流程（必须分两步走）
    

你不能直接部署主合约。流程如下：

1.  **先部署库：** 把 `Array` 库部署到链上，假设获得地址 `0xAA...AA`。
    
2.  **再部署主合约：** 部署 `TestArray`。
    

2.  怎么定位？（关键概念：链接 Linking）
    

这就是你问的核心：**主合约怎么知道库在** `0xAA...AA`**？**

这是在**部署前**由编译器和部署工具（如 Remix, Hardhat）完成的，这个过程叫 **链接（Linking）**。

**具体步骤如下：**

-   **Step A: 编译期（挖坑）** 当你编译 `TestArray` 时，编译器不知道库的地址。它会在生成的字节码里留一个 **“占位符”（Placeholder）**。
    
-   生成的字节码看起来像这样： `60806040...CALL...$Array_Library_Path$...`_(中间那串奇怪的字符就是留给库地址的坑位)_
    
-   **Step B: 链接期（填坑）** 在部署 `TestArray` **之前**，部署工具（Remix/Hardhat）会问你（或者自动查找）：“嘿，Array 库部署在哪里了？”
    
-   你告诉它：`0xAA...AA`。
    
-   工具会把字节码里的 `$Array_Library_Path$` 替换成 `0xAA...AA`。
    
-   **Step C: 部署期（上链）** 将这就“填好坑”的完整字节码部署到链上。此时，`TestArray` 就在代码里永远记住了 `Array` 在 `0xAA...AA`。
    

对于 **Public Library（需要单独部署和链接的库）**，你不能像调用普通合约那样（比如 `ICallee(地址).func()`）直接在代码里写地址调用。

你需要在 **“部署阶段”** 告诉你的合约：“嘿，那个库在链上的地址是 `0x...`”。这个过程叫 **链接 (Linking)**。

以下是具体的操作步骤：

1.  **代码层面：** 你照常写代码，假装这个库就在你本地。
    
2.  **编译层面：** 编译器会生成带有“填空题”（占位符）的字节码。
    
3.  **部署层面（关键）：** 你必须在部署脚本里，把那个已经在链上的库地址，**填入**这个“空”里。
    

第一步：代码怎么写？

假设链上已经有一个 `MathLib`，你需要在你的代码里 **拥有它的源代码（或者接口定义）**。

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 1. 你必须引入库的定义（源文件）
// 即使库已经在链上了，编译器也需要知道它里面有哪些函数
import "./MathLib.sol"; 

contract MyContract {
    function test() public {
        // 2. 直接用库的名字调用
        // 注意：千万不要写成 MathLib(0x地址).add(1, 2) —— 这是错的！
        // 就直接写 MathLib.add(1, 2);
        MathLib.add(1, 2);
    }
}
```

第二步：怎么把地址告诉合约？（怎么链接）

这取决于你用什么工具开发。我们分 **Remix** 和 **Hardhat** 两种情况讲。

3.  如果你用 Remix
    

Remix 比较智能，它通常会在部署面板自动处理，但如果是已经存在的链上库，操作如下：

1.  确保 `MathLib.sol` 在你的文件列表里。
    
2.  编译 `MyContract`。
    
3.  在 **Deploy & Run** 界面，找到 **Deploy** 按钮上面一点点，通常会有一个生成的配置文件（在旧版 Remix 中可能需要手动部署库；在新版中，如果检测到库）：
    
    1.  **情况 A（你自己部署）：** 你先部署 `MathLib`，拿到地址。然后在部署 `MyContract` 时，Remix 会自动检测到你刚才部署的库，并自动链接。
        
    2.  **情况 B（链接到现有地址）：** 这在 Remix 网页版里操作比较隐蔽。通常最简单的办法是：创建一个文件 `address_file.json` 告诉 Remix 库的地址（比较麻烦）。
        
    3.  **暴力解法：** 在 Remix 里，大部分人会选择**重新部署一遍库**（因为库通常很小，重部署成本低），然后 Remix 会自动链接新部署的这个。
        

2.  如果你用 Hardhat (推荐，最常用)
    

这是最标准的工程做法。你在写部署脚本（Deploy Script）时，显式地填入地址。

假设链上的 `MathLib` 地址是 `0x5FbDB2315678...`。

**部署脚本 (**`scripts/deploy.js`**)：**

```JavaScript
const hre = require("hardhat");

async function main() {
  // 1. 假设这是链上那个已有的库地址const libraryAddress = "0x5FbDB2315678..."; 

  // 2. 获取你的合约工厂const MyContract = await hre.ethers.getContractFactory("MyContract", {
    // 【关键步骤】：在这里配置 librarieslibraries: {
      // 库的名字 : 库的地址MathLib: libraryAddress,
    },
  });

  // 3. 部署合约// Hardhat 会自动把 libraryAddress 填入 MyContract 的字节码占位符里const myContract = await MyContract.deploy();
  await myContract.waitForDeployment();

  console.log("MyContract deployed to:", myContract.target);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

当你想要调用一个链上已有的 Public Library：

1.  **代码里：** 不需要写地址。只管 `import` 源代码，然后 `LibName.func()`。
    
2.  **部署时：** 必须使用 **Link（链接）** 操作。
    
    1.  **Hardhat:** 在 `getContractFactory` 的参数里传入 `libraries: { LibName: "0x..." }`。
        
    2.  **Remix:** 最好是让 Remix 帮你重新部署库并自动链接，或者使用脚本注入地址。
        

### Gas Saving Techniques

-   **Replacing** `memory` **with** `calldata`: 将参数的存储位置从内存改为 `calldata`。
    
-   **Loading state variable to memory**: 将状态变量读取到本地内存（栈）中处理。
    
-   **Replace for loop** `i++` **with** `++i`: 循环中用 `++i` 代替 `i++`。
    
-   **Caching array elements**: 缓存数组元素（和长度）。
    
-   **Short circuit**: 利用逻辑运算的“短路”特性。
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// Gas 高尔夫（极限优化）
contract GasGolf {
    // 初始状态 Gas 消耗参考：
    // start - 50908 gas (完全没优化)
    // use calldata - 49163 gas (使用 calldata)
    // load state variables to memory - 48952 gas (缓存状态变量)
    // short circuit - 48634 gas (短路运算)
    // loop increments - 48244 gas (循环自增优化)
    // cache array length - 48209 gas (缓存数组长度)
    // load array elements to memory - 48047 gas (缓存数组元素)
    // uncheck i overflow/underflow - 47309 gas (不检查 i 溢出 - 最终形态)

    uint256 public total;

    // 【未优化版本】
    // function sumIfEvenAndLessThan99(uint[] memory nums) external {
    //     for (uint i = 0; i < nums.length; i += 1) { // 每次都读 nums.length
    //         bool isEven = nums[i] % 2 == 0;
    //         bool isLessThan99 = nums[i] < 99;
    //         if (isEven && isLessThan99) { // 两个状态都读写，没有有效利用短路
    //             total += nums[i]; // 每次都多次读写 Storage 变量 total
    //         }
    //     }
    // }

    // 【Gas 优化终极版】
    // 输入示例: [1, 2, 3, 4, 5, 100]
    function sumIfEvenAndLessThan99(uint256[] calldata nums) external {
        // 优化 1: 将 storage 变量 total 读取到本地 stack 变量 _total
        // 避免在循环中反复读写昂贵的 storage
        uint256 _total = total;
        
        // 优化 2: 缓存数组长度，避免每次循环都去读取 nums.length
        uint256 len = nums.length;

        // 循环头部留空，把 ++i 放到最后
        for (uint256 i = 0; i < len;) {
            // 优化 3: 把当前元素取出来存到 stack 变量，避免多次访问数组索引
            uint256 num = nums[i];

            // 优化 4: 短路运算
            // 如果 num 是奇数，num < 99 就不会被执行
            if (num % 2 == 0 && num < 99) {
                _total += num; // 修改的是本地变量
            }

            // 优化 5: Unchecked 代码块
            // 省略了 i 自增时的溢出检查 (因为 i 不可能超过数组长度)
            // 优化 6: 使用前缀自增 ++i (比 i++ 稍微省一点点)
            unchecked {
                ++i;
            }
        }

        // 循环结束，只写回一次 storage
        total = _total;
    }
}
```

还有一些没看

但是感觉这些应该够现在用的了

后面再有需要的再补吧
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->


## Solidity基础语法

### 基础数据类型

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Primitives {

    bool public boo = true;

    uint8 public u8 = 1;
    uint256 public u256 = 456;
    uint public u = 123; // uint 是 uint256 的别名 (即默认就是 256 位)

    int8 public i8 = -1;
    int256 public i256 = 456;
    int public i = -123; // int 与 int256 相同

    // int 的最小值和最大值
    int256 public minInt = type(int256).min;
    int256 public maxInt = type(int256).max;

    address public addr = 0xCA35b7d915458EF540aDe6068dFe2F44E8fa733c;

    /*
    在 Solidity 中，byte 数据类型代表字节序列。
    Solidity 提供两种类型的字节数组：

     - 定长字节数组 (fixed-sized byte arrays)
     - 动态字节数组 (dynamically-sized byte arrays)
     
     Solidity 中的术语 bytes (带s) 代表动态字节数组。
     它是 byte[] 的简写。
    */
    bytes1 a = 0xb5; //  [10110101]
    bytes1 b = 0x56; //  [01010110]

    // 默认值
    // 未赋值的变量会自动拥有一个默认值
    bool public defaultBoo; // 默认值为 false
    uint256 public defaultUint; // 默认值为 0
    int256 public defaultInt; // 默认值为 0
    address public defaultAddr; // 默认值为 0x0000000000000000000000000000000000000000
}
```

只有整数，没有小数

编辑器会要求先声明版权，一般使用下面这个，意思是谁都能用，出事不管

```Solidity
// SPDX-License-Identifier: MIT
```

之后要声明 solidity 的版本

-   `pragma solidity ^0.8.0;`允许 0.8.x 系列的所有版本
    
-   `pragma solidity 0.8.19;`**锁死版本**，必须是 0.8.19
    
-   `pragma solidity >=0.7.0 <0.9.0;`范围更广，跨越大，意思是要版本在 0.7.0 以上， 0.9.0 以下
    

Gas 费用是根据你在本次交易中触碰到的**最大内存位置（Max Offset）** 来计算的。

-   如果你曾经用到了第 128KB 的位置，系统就记住了“这个用户用了 128KB”。
    
-   **你不需要为已经买过的空间再次付费**。
    

但是，solidity 中无法释放内存，

**看看你的场景在 Solidity 里通过** `new` **关键字是如何实际发生的：**

1.  **第一步：** 申请 128KB 数组 `Array A`。
    
    1.  内存指针从 `0` 移到了 `128KB`。
        
    2.  **费用：** 支付 128KB 的 Gas。
        
2.  **第二步：** 你“释放”了 `Array A` (比如 `delete a` 或者离开了函数作用域)。
    
    1.  **残酷的现实：** 内存指针**依然停留在 128KB**。Solidity 不会把指针拨回去，它只是忘掉了变量 `a` 而已。
        
3.  **第三步：** 申请 64KB 数组 `Array B`。
    
    1.  Solidity 会从指针当前的位置（128KB）开始往后分配。
        
    2.  新的指针位置 = `128KB + 64KB` = **192KB**。
        
    3.  **费用：** 你不仅没有“免费”，反而因为不仅扩容了（从 128 到 192），而且这部分扩容还是处于**更昂贵的二次方计费区间**，所以 Gas 费更贵了！
        

但是可以通过移动空闲内存指针的方法重复利用空间

```Solidity
function reuseMemory() public pure {
    // 1. 正常申请内存
    uint[] memory a = new uint[](100); // 指针移到了后面
    
    // ... 用完了 a ...

    // 2. 【关键】手动把“空闲内存指针”拨回去！
    // 0x40 是 Solidity 记录“下一个空闲内存位置在哪里”的特殊地址
    assembly {
        // 强制告诉 EVM：现在的空闲位置是初始位置 (比如 0x80)，忽略刚才申请的 a
        mstore(0x40, 0x80) 
    }

    // 3. 再次申请内存
    uint[] memory b = new uint[](50); 
    // 这次 b 会直接覆盖之前 a 的位置。
    // 因为最大内存位置没有超过之前的上限，所以这一步是【免费】的（指扩展成本为0）。
}
```

### 定义变量

Solidity 中变量分为以下三种

-   Local 在函数内部声明，不存储在链上，便宜
    
-   State 在函数外部声明，存储在链上，贵
    
-   Global 提供有关区块链的信息，其实就是通过接口获取一些全局的信息
    

```Solidity
contract Variables {
    // 【状态变量 (State Variables)】
    // 状态变量存储在区块链上。
    // (特点：写入需要消耗较高的 Gas，数据永久存在，直到合约销毁)
    string public text = "Hello";
    uint256 public num = 123;

    function doSomething() public view {
        // 【局部变量 (Local Variables)】
        // 局部变量不会保存到区块链上。
        // (特点：只在函数执行的瞬间存在，函数跑完就销毁，不占链上空间，很便宜)
        uint256 i = 456;

        // 【全局变量 (Global Variables)】
        // 这里列出了一些全局变量
        // (特点：这是 Solidity 提供的“内置信息”，你可以直接读取当前链的状态)
        
        uint256 timestamp = block.timestamp; // 当前区块的时间戳 (即：这个区块是几点几分被挖出来的)
        address sender = msg.sender; // 调用者的地址 (即：是谁发起了这笔交易/谁点了按钮)
    }
}
```

常量，硬编码，不占用内存

```Solidity
contract Constants {
    // 常量全用大写
    address public constant MY_ADDRESS =
        0x777788889999AaAAbBbbCcccddDdeeeEfFFfCcCc;
    uint256 public constant MY_UINT = 123;
}
```

不可变变量

```Solidity
contract Immutable {
    // 编码规范：常量（和不可变量）通常使用全大写字母命名
    // 这样做是为了让读者一眼就能看出：这个变量是不能被修改的
    address public immutable MY_ADDRESS;
    uint256 public immutable MY_UINT;

    // 构造函数：合约部署时执行
    constructor(uint256 _myUint) {
        // 在这里进行赋值。
        // 注意：这是不可变量 (immutable) 唯一一次可以被赋值的机会！
        MY_ADDRESS = msg.sender;
        MY_UINT = _myUint;
    }
}
```

一般不可变变量都是 state 的，在部署合约的同时记录一些信息用

在合约部署的那一瞬间，EVM 会把它的值直接 **“硬编码” (Hardcode)** 进合约的**代码逻辑 (Bytecode)** 里。

读写状态变量

```Solidity
contract SimpleStorage {
    // 【状态变量】
    // 用于存储一个数字。它会永久保存在区块链上。
    uint256 public num;

    // 【写操作】
    // 你必须发送一笔“交易” (Transaction) 才能修改状态变量。
    // 这意味着：需要消耗 Gas，且需要等待矿工打包确认。
    function set(uint256 _num) public {
        num = _num;
    }

    // 【读操作】
    // 你可以在“不发送交易”的情况下读取状态变量。
    // 这意味着：免费，且立刻返回结果。
    function get() public view returns (uint256) {
        return num;
    }
}
```

1 `ether` = 10^18 `wei`

**Giga-wei** = 10^9 wei

### 死循环太烧了

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=N2VjYmM2ZGIzMzUzMmZlMDIxNjBhMjkxODAwOTExOWRfaWZKaEs3ZUJxOEttQzJGMFlCZnhsNnJnUjNleWNCd3BfVG9rZW46QUd6T2Jhclpxb3NaVlJ4VXpsaWNuU2VRbnF5XzE3Njg4Mjc4OTg6MTc2ODgzMTQ5OF9WNA)

### If else 不用看

```Solidity
contract IfElse {
    function foo(uint256 x) public pure returns (uint256) {
        if (x < 10) {
            return 0;
        } else if (x < 20) {
            return 1;
        } else {
            return 2;
        }
    }

    function ternary(uint256 _x) public pure returns (uint256) {
        return _x < 10 ? 1 : 2;
    }
}
```

### 然后是 for 和 while，基本上跟 cpp 一样

```Solidity
contract Loop {
    function loop() public {
        // for loop
        for (uint256 i = 0; i < 10; i++) {
            if (i == 3) {
                continue;
            }
            if (i == 5) {
                break;
            }
        }
        uint256 j;
        while (j < 10) {
            j++;
        }
    }
}
```

### 然后是 struct

结构体可以在合约外部声明，并在另一个合约中导入。

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Todos {
    struct Todo {
        string text;      
        bool completed;   
    }

    // 【结构体数组】
    // 这是一个列表，里面存的全是 Todo 类型的数据
    Todo[] public todos;

    function create(string calldata _text) public {
        // 【3种初始化结构体的方法】
        
        // 方法 1: 像函数调用一样 (推荐用于字段少的结构体)
        todos.push(Todo(_text, false));

        // 方法 2: 键值对映射 (Key-value mapping) —— 【最推荐】
        todos.push(Todo({text: _text, completed: false}));

        // 方法 3: 先初始化一个空的，再手动赋值
        Todo memory todo;
        todo.text = _text;
        // todo.completed 默认就是 false，所以不写也行
        todos.push(todo); // 最后把它推入数组（拷贝进 Storage）
    }

    // Solidity 会自动为 public 的数组生成读取函数，
    // 所以其实你不需要手写下面这个 get 函数。
    // 但这个函数展示了如何手动解构返回数据。
    function get(uint256 _index)
        public
        view
        returns (string memory text, bool completed)
    {
        // 从数组里取出第 _index 个任务
        // 注意：这里用了 storage，但只是为了读取
        Todo storage todo = todos[_index];
        return (todo.text, todo.completed);
    }

    // 【修改任务内容】
    function updateText(uint256 _index, string calldata _text) public {
        // 关键点：【storage】关键字！
        // 这意味着 `todo` 是一个指向链上数据的“指针”。
        // 修改 `todo.text` 会直接修改区块链上的原始数据。
        Todo storage todo = todos[_index];
        todo.text = _text;
    }

    // 【切换完成状态】
    function toggleCompleted(uint256 _index) public {
        Todo storage todo = todos[_index];
        // 取反：如果是 true 变 false，如果是 false 变 true
        todo.completed = !todo.completed;
    }
}
```

关于这个 get 函数

```Solidity
    function get(uint256 _index)
        public
        view
        returns (string memory text, bool completed){
        Todo storage todo = todos[_index];
        return (todo.text, todo.completed);
    }
```

View 意思是说这个函数是 只读 的，同时意味着不会产生太多开销（为什么要显示写明 view？）

> 除了“只读”，写明 `view` 还有两个核心作用：
> 
> 1.  **安全锁 (Compiler Safety)：** 这是一个给编译器的承诺。如果你在 `view` 函数里不小心写了修改数据的代码（比如 `i += 1`），**编译器会直接报错**，阻止你编译通过。它防止了程序员犯傻。
>     
> 2.  **前端交互 (ABI Interface)：** 这一点至关重要。当 Remix、MetaMask 或前端代码（Web3.js）看到 `view` 标签时，它们就知道：**“这个操作不需要消耗 Gas，不需要弹出小狐狸钱包让用户签名，直接在本地节点读取即可。”** 如果不写 `view`，前端可能会把它当成一笔交易，去收用户的 Gas 费。
>     

`Todo storage todo = todos[_index];`这一句c

Solidity 有些时候要写明变量的存储位置

这一句差不多意思是定义了一个存储在区链上的变量的索引

类型 是 Todo storage ， 变量名是 todo

然后函数返回值中的`string memory text`

意思是 返回值是临时存放在 内存中 的，而不是存放在 链上 的

为什么 \_index completed 这些变量没写？

因为对于`uint`, `int`, `bool`, `address`, `bytes1`~`bytes32`这些数据类型，数据量很小，长度固定，它们默认通过**值拷贝**的方式传递，通常直接放在 EVM 的**栈 (Stack)** 上。因为太简单了，编译器不需要你操心它们的位置。**所以不需要（也不能）写关键字。**

后面这个函数

```Solidity
    // 【修改任务内容】
    function updateText(uint256 _index, string calldata _text) public {
        Todo storage todo = todos[_index];
        todo.text = _text;
    }
```

使用了 `calldata` 关键字，意思是这个变量是直接引用了【交易的原始输入数据】，而不是把它复制到内存里。它是只读的，且最省 Gas。

然后可以调用别的 sol 文件中的数据结构

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;
// This is saved 'StructDeclaration.sol'

struct Todo {
    string text;
    bool completed;
}
```

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import "./StructDeclaration.sol";

contract Todos {
    // An array of 'Todo' structs
    Todo[] public todos;
}
```

### 自定数据类型，或者说给数据类型起别名

然后可以自定义数据类型，防止张冠李戴，也可以实现底层位操作节省Gas，直接看下面这个代码吧

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// Code copied from optimism
// https://github.com/ethereum-optimism/optimism/blob/develop/packages/contracts-bedrock/src/dispute/lib/LibUDT.sol

type Duration is uint64;

type Timestamp is uint64;

type Clock is uint128;

library LibClock {
    function wrap(Duration _duration, Timestamp _timestamp)
        internal
        pure
        returns (Clock clock_)
    {
        assembly {
            // data | Duration | Timestamp
            // bit  | 0 ... 63 | 64 ... 127
            clock_ := or(shl(0x40, _duration), _timestamp)
        }
    }

    function duration(Clock _clock)
        internal
        pure
        returns (Duration duration_)
    {
        assembly {
            duration_ := shr(0x40, _clock)
        }
    }

    function timestamp(Clock _clock)
        internal
        pure
        returns (Timestamp timestamp_)
    {
        assembly {
            timestamp_ := shr(0xC0, shl(0xC0, _clock))
        }
    }
}

// Clock library without user defined value type
library LibClockBasic {
    function wrap(uint64 _duration, uint64 _timestamp)
        internal
        pure
        returns (uint128 clock)
    {
        assembly {
            clock := or(shl(0x40, _duration), _timestamp)
        }
    }
}

contract Examples {
    function example_no_uvdt() external {
        // Without UDVT
        uint128 clock;
        uint64 d = 1;
        uint64 t = uint64(block.timestamp);
        clock = LibClockBasic.wrap(d, t);
        // Oops! wrong order of inputs but still compiles
        clock = LibClockBasic.wrap(t, d);
    }

    function example_uvdt() external {
        // Turn value type into user defined value type
        Duration d = Duration.wrap(1);
        Timestamp t = Timestamp.wrap(uint64(block.timestamp));
        // Turn user defined value type back into primitive value type
        uint64 d_u64 = Duration.unwrap(d);
        uint64 t_u54 = Timestamp.unwrap(t);

        // LibClock example
        Clock clock = Clock.wrap(0);
        clock = LibClock.wrap(d, t);
        // Oops! wrong order of inputs
        // This will not compile
        // clock = LibClock.wrap(t, d);
    }
}
```

### Mapping，不可迭代

简单来说就是键值对

```Solidity
contract Mapping {
    // 【定义映射】
    // 语法：mapping(KeyType => ValueType)
    // 这里是建立一个从 "地址 (address)" 到 "数字 (uint)" 的映射
    // 典型场景：存储每个用户的代币余额
    mapping(address => uint256) public myMap;

    function get(address _addr) public view returns (uint256) {
        return myMap[_addr];
    }
    function set(address _addr, uint256 _i) public {
        // Mapping 永远会返回一个值。
        // 如果这个键 (key) 从来没有被设置过，它会返回该类型的【默认值】。
        // 对于 uint 来说，默认值是 0。
        myMap[_addr] = _i;
    }

    function remove(address _addr) public {
        // 将该地址对应的值重置为默认值 (0)。
        // 注意：它不是从内存中“移除”这个键，而是把值清零。
        delete myMap[_addr];
    }
}

contract NestedMapping {
    // 【嵌套映射 (Nested Mapping)】
    // 映射套映射 (从 address 映射到 另一个映射)
    // 结构：地址 -> (ID -> 是否完成)
    // 典型场景：检查某个用户(address) 是否完成了 第N项任务(uint)
    mapping(address => mapping(uint256 => bool)) public nested;

    function get(address _addr1, uint256 _i) public view returns (bool) {
        // 你可以从嵌套映射中获取值
        // 即使它没有被初始化，也会返回 false (bool 的默认值)
        return nested[_addr1][_i];
    }

    function set(address _addr1, uint256 _i, bool _boo) public {
        // 写入嵌套映射：像二维数组一样操作
        nested[_addr1][_i] = _boo;
    }

    function remove(address _addr1, uint256 _i) public {
        // 重置嵌套映射中的某一个具体的值
        delete nested[_addr1][_i];
    }
```

### Array

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Array {
    // 【初始化数组的几种方式】
    // 动态数组：长度不固定，可以随时增加或减少
    uint256[] public arr;
    // 初始化时这就赋值：包含 1, 2, 3 的动态数组
    uint256[] public arr2 = [1, 2, 3];
    
    // 定长数组：长度固定为 10，不能 push 或 pop
    // 初始化时所有元素默认都是 0
    uint256[10] public myFixedSizeArr;

    function get(uint256 i) public view returns (uint256) {
        return arr[i];
    }

    // Solidity 可以返回整个数组。
    // 【警告】如果数组长度可能无限增长，应避免使用此函数。
    // 因为如果数组太大，返回它会消耗过多 Gas，甚至导致交易失败。
    function getArr() public view returns (uint256[] memory) {
        return arr;
    }

    function push(uint256 i) public {
        // 将 i 添加到数组末尾，数组长度 +1
        arr.push(i);
    }

    function pop() public {
        // 移除数组最后一个元素，数组长度 -1
        arr.pop();
    }

    function getLength() public view returns (uint256) {
        return arr.length;
    }

    function remove(uint256 index) public {
        // 【注意】delete 关键字并不会改变数组长度！
        // 它只是将 index 位置的值重置为默认值。
        // 在这里，arr[index] 会变成 0，但数组长度不变。
        delete arr[index];
    }

    function examples() external pure {
        // 【内存数组】
        // 在内存中创建数组，必须指定长度（这里是5）。
        // 内存中不能创建变长数组（不能 push/pop）。
        uint256[] memory a = new uint256[](5);
        // 更新内存数组的值
        a[1] = 123;
    }
}
```

Solidity 四种可见性修饰符（`public`, `private`, `internal`, `external`），用来定义函数的 【调用权限】（即：谁有资格调用这个函数）

  **public (公开的)**：

-   **权限**：**所有人**都可以调用。
    
-   **范围**：合约**内部**可以调，合约**外部**（用户钱包、其他合约）也可以调。
    
-   _注意：变量也可以设为 public，系统会自动生成 getter 函数。_
    

  **private (私有的)**：

-   **权限**：**只有当前合约自己**可以调用。
    
-   **范围**：最严格的权限。连**继承**了这个合约的子合约**都不能**调用。
    

  **internal (内部的)**：

-   **权限**：**自己和“孩子们”**可以调用。
    
-   **范围**：当前合约可以调，**继承**了该合约的子合约也可以调。
    
-   _注意：这是状态变量的默认可见性。_
    

  **external (外部的)**：

-   **权限**：**只有外部**可以调用。
    
-   **范围**：合约**内部不能**直接调用（除非用 `this.func()` 强制调用，但这很费钱）。
    
-   _优势：当参数是大型数组时，external 比 public 更省 Gas（因为直接读 calldata 不用拷贝到 memory）。_
    

三种状态修饰符（`write`, `view`, `pure`），用来定义函数的 **【区块链状态读写权限】（即：能否读取或修改链上的数据）**

  **write (默认/无关键字)**：

-   _（注：Solidity 代码中没有_ `write` _这个关键字，不写 view 或 pure 就是默认状态）_
    
-   **权限**：**可读 + 可写**。
    
-   **代价**：执行时需要发送交易，**消耗 Gas**。
    
-   _场景：转账、修改余额、更新变量。_
    

  **view (查看)**：

-   **权限**：**只读 + 不可写**。
    
-   **代价**：外部调用时**免费**（本地查询）；内部被其他 write 函数调用时消耗 Gas。
    
-   _场景：查询余额_ `balanceOf`_、查询状态。_
    

  **pure (纯净)**：

-   **权限**：**不读 + 不可写**。
    
-   **代价**：外部调用时**免费**。
    
-   _场景：数学计算库（如_ `1+1`_）、工具函数，与链上状态完全无关。_
    

然后为了解决 Array 不能删除的问题，有以下两个解决方式

一个是将后面半截的数组左移

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract ArrayRemoveByShifting {
    // [1, 2, 3] -- remove(1) --> [1, 3, 3] --> [1, 3]
    // [1, 2, 3, 4, 5, 6] -- remove(2) --> [1, 2, 4, 5, 6, 6] --> [1, 2, 4, 5, 6]
    // [1, 2, 3, 4, 5, 6] -- remove(0) --> [2, 3, 4, 5, 6, 6] --> [2, 3, 4, 5, 6]
    // [1] -- remove(0) --> [1] --> []

    uint256[] public arr;

    function remove(uint256 _index) public {
        require(_index < arr.length, "index out of bound");

        for (uint256 i = _index; i < arr.length - 1; i++) {
            arr[i] = arr[i + 1];
        }
        arr.pop();
    }

    function test() external {
        arr = [1, 2, 3, 4, 5];
        remove(2);
        // [1, 2, 4, 5]
        assert(arr[0] == 1);
        assert(arr[1] == 2);
        assert(arr[2] == 4);
        assert(arr[3] == 5);
        assert(arr.length == 4);

        arr = [1];
        remove(0);
        // []
        assert(arr.length == 0);
    }
}
```

一个是将最后一个元素移到删除的位置

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract ArrayReplaceFromEnd {
    uint256[] public arr;

    // Deleting an element creates a gap in the array.
    // One trick to keep the array compact is to
    // move the last element into the place to delete.
    function remove(uint256 index) public {
        // Move the last element into the place to delete
        arr[index] = arr[arr.length - 1];
        // Remove the last element
        arr.pop();
    }

    function test() public {
        arr = [1, 2, 3, 4];

        remove(1);
        // [1, 4, 3]
        assert(arr.length == 3);
        assert(arr[0] == 1);
        assert(arr[1] == 4);
        assert(arr[2] == 3);

        remove(2);
        // [1, 4]
        assert(arr.length == 2);
        assert(arr[0] == 1);
        assert(arr[1] == 4);
    }
}
```

### Enum

```Solidity
contract Enum {
    // 【定义枚举】
    enum Status {
        Pending,  // 待处理 (对应 0)
        Shipped,  // 已发货 (对应 1)
        Accepted, // 已签收 (对应 2)
        Rejected, // 已拒收 (对应 3)
        Canceled  // 已取消 (对应 4)
    }

    // 【状态变量】
    // 默认值是该类型定义中的第一个元素。
    // 在这个例子中，status 初始值就是 "Pending" (也就是 0)
    Status public status;

    // 【读取枚举】
    // 虽然返回类型写的是 Status，但在底层和 Remix 的输出中，
    // 它返回的是 uint (无符号整数)。
    // Pending  - 0
    // Shipped  - 1
    // Accepted - 2
    // Rejected - 3
    // Canceled - 4
    function get() public view returns (Status) {
        return status;
    }

    // 【更新状态】
    // 你可以传入一个 uint (比如 1)，它会自动转换成对应的 Enum (Shipped)
    // 或者直接传入 Status 类型
    function set(Status _status) public {
        status = _status;
    }

    // 【更新为特定值】
    // 你可以像这样直接引用枚举里的某个选项
    function cancel() public {
        status = Status.Canceled; // 把状态改成 4
    }

    // 【重置】
    // delete 会把变量重置为它的默认值
    // 对于枚举来说，默认值就是第一个元素 (索引为0的元素)，即 Pending
    function reset() public {
        delete status;
    }
```

就像 Struct 一样，Enum 也可以定义在单独的文件里，方便多个合约复用。

## Data Locations - Storage, Memory and Calldata

```Solidity
contract DataLocations {
    // 变量被声明为 storage、memory 或 calldata，以显式指定数据的存储位置。
    // storage  - 变量是状态变量（存储在区块链上，就像硬盘）
    // memory   - 变量位于内存中，仅在函数调用期间存在（就像 RAM）
    // calldata - 包含函数参数的特殊数据位置（只读，且非常省钱）

    uint256[] public arr;
    mapping(uint256 => address) map;
    
    struct MyStruct {
        uint256 foo;
    }
    
    mapping(uint256 => MyStruct) myStructs;

    function f() public {
        // 【核心演示 1：传递 Storage 指针】
        // 使用状态变量调用 _f 函数。
        // 注意：这里传进去的是“引用”（指针），不是拷贝。
        _f(arr, map, myStructs[1]);

        // 【核心演示 2：Storage 引用】
        // 从映射中获取一个结构体。
        // 关键点：使用了 'storage' 关键字。
        // 这意味着 myStruct 只是一个指针，指向链上数据库里的那个数据。
        // 修改 myStruct.foo 会直接修改区块链上的数据！
        MyStruct storage myStruct = myStructs[1];

        // 【核心演示 3：Memory 副本】
        // 在内存中创建一个结构体。
        // 关键点：使用了 'memory' 关键字。
        // 这意味着 myMemStruct 只是一个临时的变量。
        // 修改 myMemStruct.foo 不会影响任何链上数据，函数结束它就消失了。
        MyStruct memory myMemStruct = MyStruct(0);
    }

    function _f(
        // 这里接收的参数是 storage 类型
        // 这意味着这个函数可以直接修改传入的原始状态变量
        uint256[] storage _arr,
        mapping(uint256 => address) storage _map,
        MyStruct storage _myStruct
    ) internal {
        // 对 storage 变量做一些操作
        _arr.push(1); // 会直接改变外面的 arr 数组
        _myStruct.foo = 123; // 会直接改变外面的 myStructs 映射
    }

    // 你可以返回 memory 变量
    // memory 适用于函数内部的临时计算，或者作为返回值
    function g(uint256[] memory _arr) public returns (uint256[] memory) {
        // 对 memory 数组做一些操作
    }

    // calldata 专门用于外部函数的输入参数
    // 它是只读的，不可修改，但比 memory 更省 Gas
    function h(uint256[] calldata _arr) external {
        // 对 calldata 数组做一些操作
    }
}
```

前面看差不多了

然后该补充的直接看代码里面的注释吧

`storage` - 变量是状态变量（存储在区块链上） `Memory` - 变量位于内存中，并在函数被调用时存在 `calldata` - 特殊数据位置，包含函数参数
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->



1.  ### 基础交易与 Gas 费机制 (Basic Transactions & Gas)
    

-   **代币传输机制：** 视频开始演示了从一个账户发送 DAI（稳定币）到另一个账户。Austin 强调了一个概念：**代币并不存储在浏览器或钱包里**，钱包只是在读取链上的账本。发送代币实际上是向 DAI 的智能合约发送指令，让其更新余额。
    
-   **故意制造“堵塞”交易：**
    
    -   他们尝试发送一笔交易，但故意将 **Gas Price (Max Fee)** 设置得非常低（例如 10 Gwei，而当时市场价约 30 Gwei）。
        
    -   **结果：** 交易进入了 **Mempool（内存池）**，但矿工因为费用太低而不打包它，导致交易处于 "Pending"（挂起）状态。
        
-   **Nonce（交易序号）的重要性：**
    
    -   他们解释了 **Nonce** 的概念：这是为了防止重放攻击的计数器。
        
    -   如果你有一笔 Nonce 为 6 的交易卡住了，那么 Nonce 为 7、8 的后续交易都会排队等待，无法执行。必须先解决 Nonce 6 的问题。
        

### 解决“卡住”的交易 (Fixing Stuck Transactions)

视频演示了三种处理卡顿交易的方法：

1.  **加速 (Speed Up)：** 使用 MetaMask 的加速按钮，实质上是发送一笔内容相同但 Gas 费更高的新交易来覆盖旧交易。
    
2.  **手动取消 (Manual Cancel)：**
    
    1.  这是一种高级技巧。原理是向**自己的地址**发送 **0 ETH**，但手动将 **Nonce** 设置为与那笔卡住的交易**相同**（例如 Nonce 6），并支付正常的 Gas 费。
        
    2.  一旦这笔“空交易”被打包，原来的交易就会被覆盖并失效（Dropped）。
        
    3.  _演示插曲：_ MetaMask 的界面在手动设置 Nonce 时出现了一些 UI 故障，这也是视频想要展示的真实 Web3 开发体验——工具并不完美。
        
3.  **重置账户 (Reset Account)：** Austin 展示了在 MetaMask 设置中“重置账户”。这不会丢失资金，只是清除了本地的交易历史记录。这通常用于解决 MetaMask 状态与链上状态不一致导致的 UI 卡死问题。
    

### 直接与合约交互 (Direct Contract Interaction)

-   除了使用钱包界面，他们演示了如何直接在 **Etherscan** 上操作。
    
-   **操作：** 找到 DAI 的智能合约页面，使用 "Write Contract" 功能直接调用 `transfer` 函数。
    
-   **技术细节：** 这里展示了开发者需要注意的精度问题（Decimals）。在合约层面发送 5 个 DAI，需要输入 `5000000000000000000`（18个零），而不是简单的 "5"。
    

### Layer 2 与 跨链 (Layer 2 & Bridging)

-   **跨链桥 (Bridging)：** 演示了将 ETH 从以太坊主网（L1）跨链到 **Optimism**（L2）。
    
    -   体验了跨链的等待时间（Grace Period），资金从 L1 消失但尚未到达 L2 的“恐慌时刻”。
        
-   **L2 的优势：** 资金到达 Optimism 后，他们展示了交易速度极快且 Gas 费极低（几美分 vs 主网的几十美元）。
    
-   **NFT 铸造：** 在 Optimism 上铸造了一个 "Optimistic Loogie" NFT，并随后在 Quixotic（Optimism 上的 NFT 市场）上挂单出售。
    

### DeFi 操作与授权模式 (DeFi & Approve Pattern)

-   **Uniswap/Matcha 交易：** 演示了代币兑换（Swap）。
    
-   **Approve（授权）机制：** 视频重点讲解了 ERC-20 代币的一个重要模式。当你想用合约（如 Uniswap）处理你的代币（如 DAI）时，你必须先发送一笔 **Approve** 交易，授权合约动用你的资金，然后再发送第二笔交易进行 **Swap**。
    
-   **借贷 (Aave)：** 他们尝试在 Aave 上抵押 ETH 借出 DAI。
    
    -   _教训：_ 在主网操作 DeFi 非常昂贵。为了借出一小笔钱，可能需要支付极高的 Gas 费（视频中光是抵押就要 20-30 美元手续费），这说明了为什么 L2 对普通用户如此重要。
        

### 多签钱包 (Multisig / Gnosis Safe)

视频花了很大篇幅介绍了 **Gnosis Safe**（现名 Safe）：

-   **为什么需要多签：** 为了安全。单私钥钱包（助记词）一旦丢失或泄露，资金全无。多签钱包要求多个地址（如 2/3 或 2/4）共同签名才能执行交易。
    
-   **创建过程：** 他们在 Optimism 上创建了一个 2-of-4 的多签钱包（意味着 4 个拥有者中任意 2 个人签名即可转账）。
    
-   **操作演示：**
    
    -   向多签钱包转入资金。
        
    -   发起一笔转出资金的提案（Proposal）。
        
    -   Austin 签名（第一签）。
        
    -   Etta 或 Carlos 签名（第二签）并执行交易。
        
-   **WalletConnect 集成：** 展示了如何通过 WalletConnect 将多签钱包连接到 Uniswap。这意味着他们可以作为一个“组织”在 Uniswap 上进行交易，交易请求会发回 Gnosis Safe 界面等待多方签名。
    

### 总结的核心理念

-   **Web3 的用户体验 (UX) 仍有待提高：** 视频中多次出现 MetaMask 报错、ENS 在 L2 上无法解析、交易卡顿等情况。Austin 鼓励开发者要有耐心，并致力于解决这些问题。
    
-   **安全性：** 强调了助记词管理的风险，推崇使用多签钱包来管理重要资产或团队资金。
    
-   **实操出真知：** 鼓励开发者去主网和 L2 上实际消耗一点 Gas，去体验被“卡住”的感觉，这样才能真正理解底层逻辑。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->




感觉前两天干别的活有点懈怠了

今天开始继续加油吧

先整理一下上周有没有什么没完成的内容

### 1.12-13 入门导读 & 初步理解

1.  学习内容：
    
    1.  Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分✅
        
    2.  [021 学习以太坊第 1 章](https://github.com/XiaoHai67890/021Ethereum/blob/main/%E3%80%8A021%E5%AD%A6%E4%B9%A0%E4%BB%A5%E5%A4%AA%E5%9D%8A%E3%80%8B%E5%BC%80%E6%BA%90%E6%95%99%E6%9D%90.pdf)✅
        
2.  拓展 & 辅助理解材料
    
    1.  Ethereum 官网 [Overview](https://ethereum.org/learn/) 基础内容学习✅
        
    2.  [**Day 1: A Developer’s Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro✅
        
    3.  [**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)**✅**
        
3.  工具安装
    
    1.  工具安装指南：[https://web3intern.xyz/zh/remote-work-guide/✅](https://web3intern.xyz/zh/remote-work-guide/%E2%9C%85)
        

### 1.14-15 安全 & 合规

1.  学习内容：
    
    1.  Web3 实习手册[「安全与合规」](https://web3intern.xyz/zh/security/)部分✅
        
    2.  021 学习以太坊第 2&3 章✅
        
2.  拓展 & 辅助理解材料
    
    1.  [**Day 3: ENS, DEX, Identity, Inventory, Sybil**](https://www.youtube.com/watch?v=wYSMNdIRoII&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3)**⭕**
        
    2.  [**Day 4: NFTs!!! ERC20 vs ERC721, IPFS, Metadata**](https://www.youtube.com/watch?v=NOdrEpnoCiM&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=4)**⭕**
        
    3.  [**Day 5: Stuck Transactions, Gas Limits, Multisigs, L2s, Lending…**](https://www.youtube.com/watch?v=11QTT6BK5j0&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=5)**⭕**
        
3.  工具安装
    
    1.  MetaMask / [Rabby](https://x.com/Rabby_io) 钱包安装 & 创建，并熟悉✅
        

### 1.16 实践、回顾与成果展示

1.  复习回顾相关章节，完成未完成课程
    
2.  行业探索
    
    1.  知名开发者 & Builder X 关注清单
        
3.  实践 & 下周准备
    
    1.  开发环境并熟悉：[Remix IDE](https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.30+commit.73712a01.js)**⭕**
        
        1.  [Remix 使用教程](https://www.notion.so/2bc1a400ef948075ab7ec8b8259c7746?pvs=21)
            
4.  参加周五晚例会，分享本周感悟与学习笔记
    

### 1.12-13 入门导读 & 初步理解

1.  学习内容：
    
    1.  Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分✅
        
    2.  [021 学习以太坊第 1&2 章](https://github.com/XiaoHai67890/021Ethereum/blob/main/%E3%80%8A021%E5%AD%A6%E4%B9%A0%E4%BB%A5%E5%A4%AA%E5%9D%8A%E3%80%8B%E5%BC%80%E6%BA%90%E6%95%99%E6%9D%90.pdf)✅
        
2.  拓展 & 辅助理解材料
    
    1.  [**Day 1: A Developer’s Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro✅
        
    2.  [**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)**✅**
        
    3.  [Uniswap V2 学习官方文档](https://docs.uniswap.org/contracts/v2/overview?utm_source=chatgpt.com)✅
        

### 1.14-15 安全 & 合规

1.  学习内容：
    
    1.  Web3 实习手册[「安全与合规」](https://web3intern.xyz/zh/security/)部分✅
        
    2.  021 学习以太坊第 3&4 章✅
        
2.  拓展 & 辅助理解材料
    
    1.  [**Day 3: ENS, DEX, Identity, Inventory, Sybil**](https://www.youtube.com/watch?v=wYSMNdIRoII&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3)**⭕**
        
    2.  [**Day 4: NFTs!!! ERC20 vs ERC721, IPFS, Metadata**](https://www.youtube.com/watch?v=NOdrEpnoCiM&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=4)**⭕**
        
    3.  [**Day 5: Stuck Transactions, Gas Limits, Multisigs, L2s, Lending…**](https://www.youtube.com/watch?v=11QTT6BK5j0&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=5)**⭕**
        

### 1.16 实践、回顾与成果展示

1.  复习回顾相关章节，完成未完成课程
    
2.  行业探索
    
    1.  知名开发者 & Builder X 关注清单
        
3.  实践与挑战 - Simple NFT
    
    1.  视频指导：入门级 Web2 to Web3 中的 Week 3, Day 1 - simple NFT✅
        
        -   **Link:** [**Speed Run Ethereum: Challenge 0 - Simple NFT**](https://www.youtube.com/watch?v=ej_RgaO1Pr0&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=11)**✅**
            
    2.  挑战：[Challenge #0 - Tokenization](https://speedrunethereum.com/challenge/tokenization) **（**[**中文版链接**](https://www.notion.so/Challenge-0-2afbbd63be8780dbbd56cb01490da15e?pvs=21)**）⭕**
        
4.  有余力、学习进度比较快的同学可尝试 Bounty 内容：[Challenge #5](https://speedrunethereum.com/challenge/over-collateralized-lending)
    
5.  参加周五晚例会，分享本周感悟与学习笔记
    

这块剩余没干完的活有

-   [**Day 3: ENS, DEX, Identity, Inventory, Sybil**](https://www.youtube.com/watch?v=wYSMNdIRoII&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3)**⭕**
    
-   [**Day 4: NFTs!!! ERC20 vs ERC721, IPFS, Metadata**](https://www.youtube.com/watch?v=NOdrEpnoCiM&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=4)**⭕**
    
-   [**Day 5: Stuck Transactions, Gas Limits, Multisigs, L2s, Lending…**](https://www.youtube.com/watch?v=11QTT6BK5j0&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=5)**⭕**
    
-   开发环境并熟悉：[Remix IDE](https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.30+commit.73712a01.js)**⭕**
    
-   [Challenge #0 - Tokenization](https://speedrunethereum.com/challenge/tokenization) **（**[**中文版链接**](https://www.notion.so/Challenge-0-2afbbd63be8780dbbd56cb01490da15e?pvs=21)**）⭕**
    

Rust基础的语法算是学完了，后面这段时间就持续刷题进行巩固吧

今天先把这些东西补上吧

之后干什么？

1.  系统学一下 solidity ，做到能读懂，能使用 ai 辅助完成变成即可
    
2.  每天至少一道算法题
    
3.  该把论文想办法弄出来了
    

* * *

## [**Day 3: ENS, DEX, Identity, Inventory, Sybil**](https://www.youtube.com/watch?v=wYSMNdIRoII&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3)

### ENS 域名注册 (Ethereum Name Service)https://ens.domains/

这是视频的核心演示部分，展示了如何将难记的十六进制地址转化为易读的名称（如 `sanfordstout.eth`）。

本质是 NFT

-   **类比 DNS**：ENS 之于以太坊地址，就像 DNS 之于 IP 地址。
    
-   **注册流程（两步交易）**：
    
    -   **Commit（提交）**：为了防止抢跑（Front-running），用户首先向智能合约提交一个哈希（Commitment）。这一步不包含具体的域名明文，旨在锁定请求。
        
    -   **Wait（等待）**：通常等待一分钟，确保 Commit 交易被打包。
        
    -   **Reveal（揭示）**：发送第二笔交易揭示域名并正式注册。Austin 解释了这是一种通过“提交-揭示”方案（Commit-Reveal Scheme）来防止恶意观察者在看到注册请求后提高 Gas 费抢注域名的博弈机制。
        
    
    > 为什么需要使用两步交易的方式? 如果不采用两步机制，会发生 **抢跑攻击（Front-running）**：
    > 
    > 1.  你发送交易：“我要注册 `secretproject.eth`”。
    >     
    > 2.  恶意机器人在内存池看到这笔交易，发现这个名字很有价值。
    >     
    > 3.  机器人立刻发送一笔 Gas 费更高的交易：“我要注册 `secretproject.eth`”。
    >     
    > 4.  矿工优先打包机器人的交易，你注册失败，域名被抢注。
    >     
    > 
    > 整个流程简单来说就是先把域名的承诺放到链上，之后再揭示承诺的信息，注册这个域名
    > 
    > 那么存在一个问题，怎么通过承诺来判断一个域名是否被注册过?还是说只在第二步中验证域名是否被注册过？
    > 
    > 答案是不检查，要是被注册过了就白花第一步的 Gas 了
    
-   **反向解析 (Reverse Record)**：演示了第三笔交易，将地址反向指向域名。这样当 DApp 读取该地址时，能自动显示为 `sanfordstout.eth`。
    

**ENS 架构简图**

  ENS 主要由三个核心合约组成：

1.  **Registry (注册表)：** 核心合约，记录所有域名及其拥有者，以及该域名对应的 Resolver 地址。
    
2.  **Registrar (注册器)：** 负责按照特定规则（例如每年付费、竞拍等）向用户分配域名的合约。
    
3.  **Resolver (解析器)：** 真正干活的合约。当钱包问“`sanford.eth` 的地址是多少？”时，Registry 会指引它去问 Resolver，Resolver 返回具体的地址。
    

**ENS是由谁运营？**

-   **开发团队（ENS Labs）：** `ens.domains` 这个网站本身，以及 ENS 核心智能合约的开发工作，主要由 **ENS Labs**（前身为 True Names Ltd）负责。这是一家 DAO 非营利性质的组织，致力于开发和维护 ENS 标准。
    

**ENS 的域名与地址映射表存放在哪里？所有的全节点上吗？**

-   **是的，存放在以太坊区块链的“状态（State）”中，因此每一个同步了最新状态的以太坊全节点（Full Node）都保存着这份完整的映射表。**
    

> 域名下面还可以有子域名，比如说这样
> 
> **主域名 (NFT):** `company.eth` -> 绑定到公司冷钱包/多签钱包。
> 
> **子域名 1:** `pay.company.eth` -> 绑定到财务收币地址。
> 
> **子域名 2:** `dev.company.eth` -> 绑定到开发者的地址。
> 
> **子域名 3:** `01.company.eth` -> 绑定到 1 号机器人的地址。

### Web3 身份与库存 (Identity & Inventory)

-   **身份的可移植性**：视频强调了 Web3 身份的特点——你的身份（Identity）和资产库存（Inventory）是跟随钱包的，而不是存储在某个特定应用的服务器上。
    
-   **实例演示**：
    
    -   登录 **ENS App** 时，应用识别出了用户。
        
    -   登录 **Uniswap** 时，右上角自动显示了 `sanfordstout.eth`。
        
    -   使用 **Zapper.fi** 时，无需连接钱包，仅输入域名即可查看该“角色”在链上的所有资产（ETH 和 DAI）。
        

### Uniswap 与去中心化交易 (DEX)

-   **DEX vs CEX**：
    
    -   **CEX（中心化交易所）**：基于订单簿（Order Book）和中心化数据库，类似传统银行或股票交易。
        
    -   **DEX（去中心化交易所）**：基于智能合约储备池（Liquidity Pools）。Austin 将其比作一个“自动售货机”，里面存有两种资产（如 ETH 和 DAI），并通过算法（自动做市商 AMM）根据供需自动调节价格。
        
-   **实操**：演示了将部分 ETH 兑换为稳定币 DAI。Austin 提到这不仅是一次简单的转账，而是与持有巨大资产储备的智能合约进行交互。
    

### 稳定币 (Stablecoins) 与女巫抵抗 (Sybil Resistance)

-   **DAI 的机制**：简要（且通俗地）解释了 DAI 作为一个去中心化稳定币，是如何通过超额抵押（Over-collateralized loans）、铸造/销毁机制以及 MakerDAO 的治理代币来维持与美元挂钩的。
    
-   **女巫攻击 (Sybil Attack)**：视频最后反思了刚才的操作——他们轻松创建了一个看似真人的“Sanford Stout”身份。这引出了 Web3 中的一个核心难题：如何区分通过算法生成的虚假账户和真实人类（Civil Resistance/Sybil Resistance）。
    

## [**Day 4: NFTs!!! ERC20 vs ERC721, IPFS, Metadata**](https://www.youtube.com/watch?v=NOdrEpnoCiM&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=4)

### 市场浏览与属性 (Marketplace & Properties)

-   **平台体验**：展示了如何使用 OpenSea 浏览 NFT。
    
-   **属性与稀缺性**：解释了 NFT 的属性（Traits/Properties），例如某个头像的背景色或配饰。稀缺性是由智能合约中设定的总量（Total Supply）和属性的分布决定的。
    
-   **实用性 (Utility)**：NFT 不仅仅是图片，还可以作为门票（Token Gating）、投票权或游戏道具（如视频中提到的赛马、斗鸡游戏）。
    

### 铸造 NFT 的两种方式 (Minting Methods)

视频演示了两种截然不同的铸造体验：

-   **方式一：通过项目官网铸造 (On-Chain NFT)**
    
    -   他们访问了一个名为 "MetaAvatar" 的项目网站。
        
    -   **On-Chain（全链上）**：这组 NFT 特别之处在于它的图像（SVG代码）是直接存储在以太坊智能合约里的，而不是存储在外部服务器。
        
    -   **互操作性测试**：铸造完成后，他们尝试在 OpenSea、Zapper、Rarible 等不同平台查看该 NFT。Austin 指出，某些平台（如 Rarible）没有及时显示或审查了某些内容，这体现了**Web2 平台（前端）的审查性**与**Web3 协议（后端）的抗审查性**之间的差异——只要链上确认了，你就拥有它，无论前端是否显示。
        
-   **方式二：通过 Etherscan 直接交互 (Direct Contract Interaction)**
    
    -   Carlos 部署了一个简单的测试网（Rinkeby）NFT 合约。
        
    -   **Power User 操作**：他们没有使用任何前端网页，而是直接在 **Etherscan** 的 "Write Contract" 页面，连接钱包并调用 `mint` 函数。
        
    -   这展示了只要合约开源且经过验证，用户可以直接绕过项目方的前端与智能合约交互。
        

### NFT 的存储层：IPFS vs. HTTP

这是视频中最硬核的技术部分，Austin 详细拆解了 NFT 到底是什么。

-   **NFT 的本质**：在 ERC-721 标准下，你拥有的其实是一个 `TokenID`（例如 #1）。
    
-   **元数据 (Metadata)**：智能合约中有一个 `tokenURI` 函数，它指向描述这个 NFT 的文件（JSON格式，包含名称、描述、图片链接）。
    
-   **IPFS (星际文件系统)**：
    
    -   视频演示了如何通过 Etherscan 读取 `tokenURI`，获取一个 IPFS 哈希值。
        
    -   **内容寻址 (Content Addressing)**：Austin 演示了如果修改文件内容（Buffalo -> Bison），哈希值就会彻底改变。这保证了 **不可篡改性**。
        
    -   **中心化风险 (Rug Pull)**：Austin 警告说，如果 NFT 的 `tokenURI` 指向的是一个普通的 HTTP 链接（如 `aws.amazon.com/my-nft.png`），项目方随时可以把图片换成其他东西或者直接删库跑路。只有存储在 IPFS 或全链上的 NFT 才是相对安全的。
        
    
    > **什么是 IPFS？**
    > 
    > **IPFS (InterPlanetary File System，星际文件系统)** 是一个点对点（P2P）的分布式文件存储和传输协议。它的目标是取代 HTTP，构建一个更快、更安全、更开放的 Web。
    > 
    > **最核心的区别：**
    > 
    > -   **HTTP (Web2):** 基于**位置**寻址 (Location-based)。
    >     
    >     -   _“去阿里云深圳机房的 X 服务器找_ `photo.jpg`_。”_
    >         
    >     -   缺点：如果服务器挂了，或者文件被删了，你就找不到了（404）。
    >         
    > -   **IPFS (Web3):** 基于**内容**寻址 (Content-based)。
    >     
    >     -   _“我要找哈希值为_ `QmAbC...` _的那张照片，我不关心它在谁的电脑里，谁有就给我传一份。”_
    >         
    >     
    > 
    > **IPFS 的原理（简单版）**
    > 
    > 1.  **内容寻址 (Content Addressing) —— 视频中提到的 "Cid"**
    >     
    > 
    > 这是 IPFS 的灵魂。 当你把一张图片上传到 IPFS，系统会对文件内容进行哈希计算（SHA-256），生成一个唯一的指纹，叫 **CID (Content ID)**。
    > 
    > 2.  **默克尔有向无环图 (Merkle DAG)**
    >     
    >     1.    IPFS 不会把大文件直接存进去，而是像拼图一样：
    >         
    >     2.    **切片：** 把大文件（比如一部电影）切成每块 256KB 的小碎片。
    >         
    >     3.    **哈希：** 给每个碎片算一个哈希。
    >         
    >     4.    **树状结构：** 用一个根哈希（Root Hash）把这些碎片连起来。 这使得 IPFS 可以去重（大家都有同样的文件头，只存一份即可）和并行传输。
    >         
    > 3.  **分布式哈希表 (DHT) —— 怎么找到文件？**
    >     
    >     1.    当你告诉网络“我要下载 `QmAbC...`”时：
    >         
    >     2.  你的电脑会询问附近的节点：“你知道谁有 `QmAbC...` 吗？”
    >         
    >     3.  网络通过 DHT（类似一个去中心化的电话簿）快速定位到拥有这个文件碎片的节点（可能是你的邻居，也可能是地球另一端的人）。
    >         
    >     4.  **点对点传输：** 你的电脑同时从几十个节点下载不同的碎片，拼成完整文件。
    >         
    > 4.  **inning (固定) —— 关键的“坑”**
    >     
    >     1.    视频里提到了一点：**IPFS 不是魔法硬盘。** IPFS 节点（比如你自己的电脑）有垃圾回收机制。如果你下载了一个文件，过段时间不看，缓存会被自动清理。
    >         
    >     2.  **问题：** 如果你是某个冷门 NFT 的唯一拥有者，且你关机了，别人就无法通过 IPFS 下载这个 NFT 的图片了（虽然链上的哈希还在）。
    >         
    >     3.  **解决：** 你需要 **"Pin"**（大头针钉住）这个文件。这通常由 Infura、Pinata 这样的服务商来做（类似 Web3 的云盘），他们承诺永远保持服务器开机并“钉住”你的文件，确保它永远在线。
    >         
    

### 代码层面的理解 (Solidity Logic)

Austin 展示了极简版的 Solidity 代码，对比了同质化代币（Fungible）与非同质化代币（Non-Fungible）的区别：

-   **Fungible (如 ERC-20)**：使用 `mapping(address => uint256) balance`。主要记录“**谁有多少个**”。转账只是在一个大账本上加减数字。
    
-   **Non-Fungible (如 ERC-721)**：使用 `mapping(uint256 => address) owner`。主要记录“**这个 ID 归谁**”。转账是将特定 ID 的拥有者修改为新地址。
    
-   **全链上 SVG**：展示了如何在 Solidity 中用代码拼接字符串来生成 SVG 图像（如 `<circle cx="..." />`），使得图像数据完全存在区块链上。
    

### 总结

-   **转账的本质**：并没有文件在网线中传输，所有人只是在同步更新同一个去中心化的账本（状态机）。
    
-   **版税 (Royalties)**：简要提及开发者可以在合约中写入版税逻辑，让创作者在二级市场交易中持续获利。
    
-   **核心观念**：从“用户”转变为“开发者”，学会查看合约源码、理解元数据存储位置（On-chain vs IPFS vs Centralized Server）是判断 NFT 价值和安全性的关键。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->






## Unphishable 钓鱼攻防挑战

第一章测试是安装小狐狸

连接钱包

连接测试网络

领取测试币

之前已经做过了，基本上没什么问题

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZGM0NmUyMDg4Y2IwOTEyYjc2OWVlODRmOTYxMWViMWJfbGljdTFxcWIzQmEzNnBOaVVUcTJaTDVBTE1aMmpkTlBfVG9rZW46SEgyMmJvTHZQbzg1cG54eFo5OGNSeThqbktiXzE3Njg1NjYwNDA6MTc2ODU2OTY0MF9WNA)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MjA2M2I5NmZjNzg5YWU5YzU2N2Y3Y2IyYjY5N2ZjMTFfYU1QUWR0RzNsQXhGeE5yYVBuM0N4WjRwc3pOb2tBR3JfVG9rZW46SUZFZ2JHdms4b1R1UDZ4UXNJdmMxQjRtbk5iXzE3Njg1NjYwNDA6MTc2ODU2OTY0MF9WNA)

然后第二个实验是防钓鱼测试

钱包这个东西不是丢了就丢了，怎么还能找回的） 也算是顺利通过

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDk5OThkYjUyNTYwMWFmNGJiMmMyZDNjYTgzMGUxYTJfY2NIaElNMHpRZEtCcEI1TFZjd1prUW1paHA0bHh6ZktfVG9rZW46RTF1bWJJSXdmb2VaRlF4UFQ1VmNIM2pRbjRlXzE3Njg1NjYwNDA6MTc2ODU2OTY0MF9WNA)

然后第三个实验是请求比我身份证号都长的支出上限

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTY5ZDQzODE5MDJiYzliMTc5ZTgwY2ZjYmQzNDlhZTJfWkVVWmcxSFdQaXJJaHlJdnMzRUJJYWw2V1M4T3FqWjdfVG9rZW46UVdiUmJoSjVnb1ZaOTB4cVVFWWNzdTIxbk1iXzE3Njg1NjYwNDA6MTc2ODU2OTY0MF9WNA)

取消后直接完成了

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NTU4NWVjZjY5ZjUxMDY2MTdjOTA1MGE5YjkxM2E5NGVfcHRTSTJ2UWE1NGhnUmt4VW5qc3RwNGhzeGRVNnJBRHZfVG9rZW46T1RjUGJ1czRwb3JEbXF4bnJkRWM2TjJSbjllXzE3Njg1NjYwNDA6MTc2ODU2OTY0MF9WNA)

然后是第四个测试，怎么领取空投还要我自己花钱的

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=Zjg2NDQ1YzkyZTRkNDQ0ZWYyMzU5NDg0MjRjNzFjZDJfT3lmNVRidGgzaW1EaDhXT2lkVjh4RkttNUhqUmhEdmpfVG9rZW46V3hMN2JteG92b0RLZFl4WlJkV2NTc2dibk1oXzE3Njg1NjYwNDA6MTc2ODU2OTY0MF9WNA)

也是通过了

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MTlkNjFlM2IyNjgwMDRkMTQ1MzUyOTFkNTY1MDUzOGJfc0xlYnJLZXZpRXdCamFNcXRnY0ZTc0hFMkNlY0YwR3ZfVG9rZW46R01ETWJlYUs4b0tQbkh4aE9EUWNNRURSbmhmXzE3Njg1NjYwNDA6MTc2ODU2OTY0MF9WNA)

感觉总之就是不要花钱）

第五个也通过了

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YzAxNGE2ZWVmZWRiMDFkMjdmZjFhMzFhNDU3MTA1ZWVfUVNlUmJyZ241eFYybFN3dnpmZU9JZjZQMUxaTDdzRlpfVG9rZW46TTNvMmJFQmIxb2RJVXB4RVg4aWNsNHlabkJlXzE3Njg1NjYwNDA6MTc2ODU2OTY0MF9WNA)

[app.un1swap.org](http://app.un1swap.org)

[app.uniswap.org](http://app.uniswap.org)

查了一下交易信息的格式

```YAML
Transaction Hash:   0x5d2a... (唯一的交易ID)
Status:             Success (成功)
Block:              18234567
Timestamp:          1 min ago
Transaction Action: Swap 1,000 USDT for 0.5 ETH (明确的操作描述)
From:               0xYourWalletAddress (你的钱包地址)
Interacted With (To): 0xUniswapRouterAddress (Uniswap 官方路由合约，且通常有绿勾认证)

ERC-20 Tokens Transferred: 
From 0xYourWallet... To 0xUniswapPool... For 1,000 USDT (Tether USD)
From 0xUniswapPool... To 0xYourWallet... For 0.5 WETH (Wrapped Ether)

Value:              0 ETH (因为你转的是代币，不是 ETH 本身)
Transaction Fee:    0.002 ETH (Gas 费)
```

钓鱼信息中

```YAML
Transaction Hash:
0x1b23f927c47a6cb68fb7207a42c600a34726c583082301d2b7626d5447a5e363
Status:
Success 20 Block Confirmations
Block:
3504400
Timestamp:
5 mins ago (Mar-17-2025 02:34:36 AM UTC)
Transaction Action:
Call Function by 0x4276BF06...a1B7D03Ce on 0x4d264c6A...41F757Fe5
From:
0x4276BF06Aebd8A1F3D89a1fd5BF84bFa1B7D03Ce
Interacted With (To):
0x4d264c6Ab8933633cd8B052dE243Fa41F757Fe5
ERC-20 Tokens Transferred:
From 0x4276BF06...a1B7D03Ce To 0xE1a9d5C7...615506D4
For 8,888 ERC-20: app.un1swap.org (UNI)
Value:
0 ETH
Transaction Fee:
0.0000001689464305 ETH
Gas Price:
0.003237514 Gwei (0.000000000003237514 ETH)
```

ERC-20 Tokens Transferred:

From 0x4276BF06...a1B7D03Ce To 0xE1a9d5C7...615506D4

For 8,888 ERC-20: [app.un1swap.org](http://app.un1swap.org) (UNI)

这部分本应该是代币名称的，被改成钓鱼链接了

然后下一个测试也是要无上限的转账
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->







### 一、 宏观背景与合规基本逻辑

1\. 合规 vs. 风控

-   **企业端**：主要强调“合规”（Compliance）。
    
-   **个人端**：本质是“风控”（Risk Control）。
    
-   **核心矛盾**：法律具有**滞后性**，而Web3行业具有激进性和创新性。在大陆语境下，由于政策收紧，合规空间狭窄。
    
-   **律师的作用**：将滞后的法律规定与新兴业务形态进行梳理和匹配，通过规范业务动作来规避法律红线。
    

2\. 地域差异（大陆 vs. 香港）

-   **法无禁止即自由**：在香港等地，如果没有明确法律禁止（如早期的找换店），业务可以开展；一旦立法（如稳定币条例），业务需收缩或持牌。
    
-   **大陆环境**：强调刑事合规，监管更为严厉，往往缺乏明确的“Web3法律”，而是套用现有的刑法罪名。
    

* * *

### 二、 具体业务场景的风险与案例

1\. 发币与ICO（非法集资风险）

-   **现状**：单纯发币（如Meme币）本身不一定构成犯罪，但**“公开融资”**是红线。
    
-   **负面清单**：大陆严厉打击利用发币进行公开融资的行为（ICO）。
    
-   **案例**：
    
    -   **畜牧业类比**：“认养一头牛”模式若涉及传销机制和公开融资，同样违规。
        
    -   **Mimi Coin案例**：某项目仿照Meme Coin玩法，社区喊单后撤回流动性，即便请了国内顶尖刑辩律师，最终仍被认定为**诈骗罪**。
        

2\. 交易所与永续合约（开设赌场风险）

-   **办案逻辑**：
    
    -   办案人员不懂技术，通常问：“听过94、924公告吗？”若听过仍从业，被视为有主观犯罪故意。
        
    -   **定性**：永续合约被视为“压大小”的赌博，交易所被定性为**开设赌场**，技术人员（即使主张技术中立）会被视为共犯（提供技术支持）。
        
-   **辩护难点**：虽然律师主张合约是金融活动（风险控制权在用户手中）而非赌博（买定离手），但在大陆语境下，常出现**“逐利性执法”**（通过打击交易所实现地方财政创收）。
    
-   **合规出路**：
    
    -   **形式合规**：完善法律文本。
        
    -   **深度合规**：这就涉及到“中国特色”的合规，即与地方建立良好关系。
        

3\. 挖矿业务（政策波动与转型）

-   **演变**：从全面禁止到部分地方政府默许回流。
    
-   **转型路径**：将挖矿包装为**AI算力超算中心**。与政府签协议，甚至获取政策福利。算力可合规外贸卖给海外，至于海外买家是用来挖矿还是跑AI，则属于服务范畴，风险隔离。
    

4\. 去中心化业务（Polymarket类）

-   **定性**：虽然美国可能将其视为金融衍生品，但在大陆法律体系下，如果华人开设此类预测市场，必被定性为**赌博**。
    
-   **建议**：彻底出海，不要在大陆硬刚司法认定。
    

* * *

### 三、 资金与账户风险（OTC、出入金、U商）

1\. 个人出入金（风险极高）

-   **数据**：出金涉案比例在律所咨询中占比约7%。
    
-   **现状**：
    
    -   **冻卡风险**：核心是收到赃款（电信诈骗、赌博资金）。
        
    -   **警方态度**：部分地区（内蒙、新疆）仍倾向跨省抓人；其他地区若能证明“善意取得”（不知是赃款、价格合理、留存证据），可协商解冻。
        
    -   **税务风险**：通过香港合规交易所出金回国，已被税务局盯上，要求**申报税务**。
        

2\. 新型风险：入金攻击（洗钱新手段）

-   **背景**：海外资金盘洗钱成本高、通道少。
    
-   **手法**：洗钱团伙找普通人（如正规职业者），要求普通人付5万现金，团伙向其指定账户打入10万赃款。
    
-   **后果**：账户因收赃款被冻结甚至立案。
    
-   **建议**：若账户莫名收到大额资金，需警惕并考虑主动报案留底；交易必须核实对手方身份（KYC）。
    

3\. U商（USDT交易商）

-   **结论**：在大陆**做不了**，要做只能去海外。
    
-   **终极形态**：加密支付（Crypto Payment）。
    
-   **B端风险**：即便做B端生意，也难免被“车队”（海外洗钱团伙/地下钱庄）盯上，混入黑钱导致账户连坐冻结。
    

4\. U卡（加密信用卡）风险

-   **挂靠公司风险**：发卡方将用户挂靠为公司员工，一旦公司被查，资金冻结。
    
-   **“私池”风险**：发卡方是个人组局，资金在其控制的多签钱包或个人地址中。若合伙人闹翻、被抓或跑路，用户资金归零。
    

* * *

### 四、 项目方与KOL的衍生风险

1\. 项目方经营风险

-   **“配增公司”/技术侦查**：大陆存在与公安合作的技术公司，专门分析链上数据、卧底项目群、固定证据，举报后从罚没款中**拿返点**。
    
-   **常见罪名**：
    
    -   **非法吸收公众存款/集资诈骗**：针对公开募资。
        
    -   **非法经营罪**：口袋罪，涉及法币与虚拟币兑换（对敲）。
        
    -   **组织、领导传销活动罪**：
        
        -   误区：认为只有两级、不直接分润就没事。
            
        -   现实：司法实践直接穿透，实质重于形式。
            

2\. KOL（关键意见领袖）风险

-   **案例**：
    
    -   某资讯平台因给海外交易所（被定性为赌博网站）打广告，员工被抓，定罪\*\*“非法利用信息网络罪”\*\*。
        
    -   个人KOL接交易所推广，拿返佣（Rebate），被定性为**开设赌场罪的共犯**。
        
-   **求职建议**：入职Web3公司若涉及合约、期权板块，风险较高。
    

* * *

### 五、 总结与建议

1.  **出海是核心**：在大陆做Web3合规极其困难，建议业务和人员主体出海。
    
2.  **不要抱侥幸心理**：形式上的合规（如层级设计、技术中立说辞）在大陆穿透式执法面前往往无效。
    
3.  **深度合规**：如果必须在大陆展业，除了法律文本合规，还需要建立“中国特色”的深度合规（如与地方政府、监管的良好沟通与合作）。
    
4.  **警惕新型犯罪**：如入金攻击、配增公司的钓鱼执法等。
    

**结束语**：合规不仅是针对Web3，任何行业在大陆进行非法募资或违规金融活动都会受到打击。在当前环境下，保护好自己（人身安全与资产安全）是第一要务。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->








## w1d3

-   **入门技术&深度技术任务**
    
    -   [**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)
        
    -   021 以太坊 第四章
        
-   **Rust基础**
    
    -   10 [Generic Types, Traits, and Lifetimes](https://rust-book.cs.brown.edu/ch10-00-generics.html#generic-types-traits-and-lifetimes)
        
-   **杂项**
    
    -   改卷子
        
    -   整理企业项目方案
        

有点慢了，感觉走远了

总之加油干吧还能怎么办，实在不行明天开始争取能把上午的时间变成学习的

看着后面几天的任务好像不多，等到周五六日猛猛补进度吧

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OTBlNGEwODFkNTY1N2Q3NDZhMTFiNTJhNzgyNDM4OTFfU2NtN1A5bkF6ZXR2YmY2eFN0VGUybmo3djFXRFhKWDhfVG9rZW46VFBrb2JFcWtsb0dMT3p4VHRpMGNHTG5FbmJhXzE3NjgzNjc4Mjk6MTc2ODM3MTQyOV9WNA)

* * *

-   [https://layer2.myfirst.io/#1.3-layer2](https://layer2.myfirst.io/#1.3-layer2)
    

不知道什么时候打开的网页，后面看一下

## Rust 基础[Generic Types, Traits, and Lifetimes](https://rust-book.cs.brown.edu/ch10-00-generics.html#generic-types-traits-and-lifetimes)

### Generic Types 泛型

在函数体中使用参数时，必须声明该参数。 参数签名中应包含参数名称，以便编译器知道该名称的含义。

定义泛型函数，我们将类型名称声明放在尖括号内。 在函数名和参数列表之间 `<>` ，如下所示：

```Rust
fn largest<T>(list: &[T]) -> &T {
```

我们把这个定义理解为：函数 `largest` 在某种类型上是泛型的 `T` 此函数有一个名为 `list` 的参数，它是一个类型为 `T` 的值的切片。 `largest` 函数将返回对相同类型 `T` 的值的引用。

**用泛型定义结构体**

```Rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let integer = Point { x: 5, y: 10 };
    let float = Point { x: 1.0, y: 4.0 };
}
```

```Rust
struct Point<T, U> {
    x: T,
    y: U,
}

fn main() {
    let both_integer = Point { x: 5, y: 10 };
    let both_float = Point { x: 1.0, y: 4.0 };
    let integer_and_float = Point { x: 5, y: 4.0 };
}
```

用泛型定义枚举

```Rust
enum Option<T> {
    Some(T),
    None,
}
```

```Rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

**定义相关方法**

```Rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {//函数名与字段名相同，是因为一般情况下字段名是私有的，需要通过方法来访问
        &self.x
    }
}

fn main() {
    let p = Point { x: 5, y: 10 };

    println!("p.x = {}", p.x());
}
```

为特定类型f32实现方法

```Rust
impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}
```

你不能同时实现同名的特定方法_和_通用方法。例如，如果你为所有类型 `T` 实现了一个通用的 `distance_from_origin` 方法，又为 `f32` 实现了一个特定的 `distance_from_origin ，那么编译器会拒绝你的程序：Rust 不知道在调用` `Point<f32>::distance_from_origin` \` 时应该使用哪个实现。

**如何在同一个方法中，混合使用“结构体定义的泛型”和“方法自带的泛型”**

```Rust
struct Point<X1, Y1> {
    x: X1,
    y: Y1,
}

impl<X1, Y1> Point<X1, Y1> {
    fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 5, y: 10.4 };
    let p2 = Point { x: "Hello", y: 'c' };

    let p3 = p1.mixup(p2);

    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
}
```

**Rust 的泛型没有任何运行时性能损耗**

下面这个代码无法通过编译

```Rust
fn print_slice<T>(v: &[T]) {
  for x in v {
    println!("{x}");
  }
}
fn main() {
  print_slice(&[1, 2, 3]);
}
```

Rust 的编译器非常严谨。虽然你说 `T` 可以是任何类型，但并非所有类型都能用 `{}` 格式来打印。

需要给 `T` 加一个约束，告诉编译器：“我只接受那些**实现了打印功能**的类型”。

```Rust
use std::fmt::Display; // 引入 Display 特征

// 关键点：<T: Display>
// 这句话的意思是：T 可以是任何类型，但前提是它必须能被打印
fn print_slice<T: Display>(v: &[T]) {
    for x in v {
        println!("{x}");
    }
}

fn main() {
    print_slice(&[1, 2, 3]);
}
```

### Trait

Trait 定义了一种“共同的行为”或“能力”

类似接口，需要给出一些需要实现的功能

例如，假设我们有多个结构体，分别保存各种类型和数量的文本：

-   一个 `NewsArticle` 结构体，用于保存特定位置的新闻报道；
    
-   一个 `SocialPost` 结构体，最多可以包含 280 个字符，以及指示它是新帖子、转发帖子还是对另一个帖子的回复的元数据。
    

我们希望创建一个名为 `aggregator` 的媒体聚合库 crate，它能够显示可能存储在 `NewsArticle` 或 `SocialPost` 实例中的数据摘要。为此，我们需要每种类型的摘要，并且将通过在实例上调用 `summarize` 方法来请求该摘要。清单 10-12 展示了一个公开的 `Summary` 特性的定义，该特性表达了这种行为。

```Rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

**在类型上实现特性**

```Rust
pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct SocialPost {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub repost: bool,
}

impl Summary for SocialPost {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```

在一个类型上实现特性与实现常规方法类似。不同之处在于，在 `impl` 后面，我们放置要实现的特性名称，然后使用 `for` 关键字，接着指定要为其实现特性的类型名称。

需要 trait 或者 类型 在当前 crate 才可以为其实现 trait

**默认实现**

```Rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}
```

后续可以选择保留，也可以直接覆盖

```Rust
pub trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}
```

也可以这样写，直接调用别的 trait

**将 Trait 作为参数**

```Rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

上面的代码意思是 item 需要是实现了 Summary trait 的类型

**Trait Bound 约束**

```Rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

跟刚才的代码功能上一样，不过刚才的代码使用了语法糖，更简单一些，这个是展开的写法

这种写法比较适用于更复杂的情况，例如，我们可以有两个实现 `Summary` 的参数。使用 `impl Trait` 语法看起来像这样：

```Rust
pub fn notify(item1: &impl Summary, item2: &impl Summary) {
```

如果我们希望强制两个参数具有相同的类型，我们必须使用特征约束，如下所示：

```Rust
pub fn notify<T: Summary>(item1: &T, item2: &T) {
```

我们也可以指定多个特质约束。

```Rust
pub fn notify(item: &(impl Summary + Display)) {
```

```Rust
pub fn notify<T: Summary + Display>(item: &T) {
```

但是如果面对更复杂的情况，会导致函数签名过长，可以使用 where

```Rust
fn some_function<T: Display + Clone, U: Clone + Debug>(t: &T, u: &U) -> i32 {

fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{
```

可以在返回位置使用 `impl Trait` 语法来返回一个实现某个特征的类型的值，如下所示：

```Rust
fn returns_summarizable() -> impl Summary {
```

只能在使用单个返回类型时使用 `impl Trait` 。例如，以下代码返回一个 `NewsArticle` 或一个 `SocialPost` ，并且将返回类型指定为 `impl Summary` ，这样将无法工作

```Rust
fn returns_summarizable(switch: bool) -> impl Summary {
    if switch {
        NewsArticle {}
    } else {
        SocialPost {}
    }
}
```

即使你用了 `impl Trait` 隐藏了具体类型，但在编译时，**该函数的所有分支返回的具体类型必须完全一样。**

**使用 trait bound 条件性的实现方法**

```Rust
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

impl<T: Display + PartialOrd> Pair<T> {//给只有实现了 Display + PartialOrd trait 的 T 实现的功能
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```

### 生命周期

-   一种泛型
    
-   生命周期确保引用在我们需要它们有效的时间内始终有效
    

生命周期的主要目的是防止悬垂引用

```Rust
fn main() {
    let r;
    {
        let x = 5;
        r = &x;
    }
    println!("r: {}", r);
}
```

简单来说就是指针指向的内存**原本是有效的**，但是后来被释放（free/drop）了，而指针还在

```Rust
fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() { x } else { y }
}
```

编译器在检查 `longest` 函数时，是一个 **局部检查 (Local Analysis)**。它只盯着这个函数看，看不到外部是谁在调用它（看不到 `main` 函数里的花括号）。

如果函数返回 `x`，那么返回值的寿命 = `x` 的寿命。

如果函数返回 `y`，那么返回值的寿命 = `y` 的寿命。

现在问题来了：既然不知道返回谁，那编译器应该要求返回值的寿命必须多长才安全呢？它陷入了逻辑死循环。

既然编译器猜不到，我们就手动写下“生死契约”。

**契约内容 (**`'a`**)**：我们告诉编译器：“别管具体是 `x` 还是 `y`，反正**返回值的寿命**肯定不会比 **输入参数里寿命最短的那个** 更长。”

**生命周期注解语法**

  生命周期注解不会改变任何引用的存活时间

  生命周期注解的语法略有不同：生命周期参数的名称必须以撇号（ `'` ）开头，通常全部小写且非常简短，类似于泛型类型。大多数人使用 `'a` 作为第一个生命周期注解的名称。我们将生命周期参数注解放在引用的 `&` 之后，用空格将注解与引用的类型分开

  仅有一个生命周期标注本身并没有太多意义，因为这些标注的目的是告诉 Rust 多个引用的泛型生命周期参数之间是如何相互关联的

**函数签名中的生命周期注解**

```Rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

感觉这部分没看太明天，但是基本上知道怎么用了

为什么没看明白？

没搞懂这个生命周期是干什么用的，似乎只能给编译器提供信息

好像该报错的地方还会报错

**结构体定义中的生命周期注解**

```Rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().unwrap();
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
```

我们在结构体名称后面的尖括号中声明泛型生命周期参数的名称，以便在结构体定义的主体中使用生命周期参数。这个注解意味着 `ImportantExcerpt` 的实例不能比它在其 `part` 字段中持有的引用存活得更久

**生命周期推断**

```Rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

在 Rust 的早期版本（1.0 之前），这段代码是无法编译的，因为每个引用都需要显式的生命周期

在编写了大量 Rust 代码后，Rust 团队发现 Rust 程序员在特定情况下会反复输入相同的生命周期注解。这些情况是可预测的，并遵循几种确定性的模式。开发人员将这些模式编程到编译器的代码中，以便借用检查器可以在这些情况下推断生命周期，而无需显式注解。

当结构体本身带有生命周期参数时，如何为它实现方法

```Rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }
}
```

特殊生命周期 `'static`

表示受影响的引用可以持续整个程序的运行时间

* * *

卷子改不完了😭😭😭😭😭😭😭😭😭
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->










## w1d2

今天先保底把昨天没干完的事情干完吧

-   **入门技术&深度技术任务**
    
    -   021 学习以太坊第 3 章
        
    -   [**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)
        
    -   [Uniswap V2 学习官方文档](https://docs.uniswap.org/contracts/v2/overview?utm_source=chatgpt.com)
        
-   **Rust基础**
    
    -   第八章 common collection
        
    -   第九章 error handling
        
-   **杂项**
    
    -   把小登试卷er图那题改完吧
        

先这样吧，至少今天要把这些事给干完

* * *

## **Rust基础**

### **String**

**str和 String的区别**

-   str 是字符串切片，是一串长度可变的字节序列，是要访问的一块数据本身
    
-   &str 胖指针，起始位置+长度
    
-   String 是 围绕 Vec<u8> 封装的结构体，封装了一些列
    

**构造**

```Rust
println!("Hello, world!");

let mut s = String::new();
let data = "initial contents";
let s = data.to_string();
// The method also works on a literal directly:
//实现了display trait的类型都可以使用to_string方法
let s = "initial contents".to_string();
let s = String::from("initial contents");
```

**连接字符串**

```Rust
let mut s = String::from("foo");
s.push_str("bar");//使用push_str方法追加字符串

let mut s1 = String::from("foo");
let s2 = "bar";
s1.push_str(s2);//使用push_str方法追加字符串，本质上s2没有被移动，对s2的数据进行了一次“字节拷贝”
println!("s2 is {s2}");//s2仍然有效

let mut s = String::from("lo");
s.push('l');//使用push方法追加单个字符

let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // s1被移动，无法再使用
//s2仍然有效，因为它是通过引用传递的

let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");
let s = format!("{s1}-{s2}-{s3}");
//使用format!宏来连接字符串，format!宏不会获取任何参数的所有权
```

**迭代字符串的方法**

字符串不支持索引访问，因为字符串是UTF-8编码的，某些字符可能占用多个字节

不支持下面的操作

```Rust
let s1 = String::from("hi");
let h = s1[0];
```

获取第N个字节

```Rust
let s1 = String::from("hello");
// 1. 先转成字节切片，再索引
let h_byte = s1.as_bytes()[0]; 
println!("{}", h_byte); // 输出 104 (这是 'h' 的 ASCII 码)
println!("{}", h_byte as char); // 强转回 char，输出 'h'
```

获取第N个Unicode字符

```Rust
let s1 = String::from("你好World");
// 1. 获取迭代器 -> 取第0个 -> 解包(因为可能越界返回None)
let c = s1.chars().nth(0).unwrap(); 
println!("{}", c); // 输出 "你"
```

直接迭代

```Rust
for c in "Зд".chars() {
    println!("{c}");
}
```

逐字节

```Rust
for b in "Зд".bytes() {
    println!("{b}");
}
```

### Hash Map

> 用太久dict说哈希表想不起来是什么，其实就是字典

基础操作

```Rust
    use std::collections::HashMap;//导入HashMap类型

    let mut scores = HashMap::new();//创建一个空的HashMap

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);//向HashMap中插入键值对

    let team_name = String::from("Blue");
    let score = scores.get(&team_name).copied().unwrap_or(0);//通过键获取值

    for (key, value) in &scores {//遍历HashMap中的键值对
        println!("{key}: {value}");
    }
```

在通过调用 `insert` 将变量移入哈希映射后，我们无法再使用这些变量，所有权转移到哈希表

解决方法可以直接使用clone或者引用，但是用引用的话需要考虑生命周期的问题

更新

直接 insert ，相同的键会被覆盖

使用 entry 可以只在没有键值的时候插入

```Rust
    scores.entry(String::from("Yellow")).or_insert(50);
    scores.entry(String::from("Blue")).or_insert(50);
```

`Entry` 上的 `or_insert` 方法被定义为：如果该键存在，则返回对应 `Entry` 键的值的可变引用；如果不存在，则将参数作为此键的新值插入，并返回对新值的可变引用。

基于旧值更新

```Rust
    let text = "hello world wonderful world";

    let mut map = HashMap::new();

    for word in text.split_whitespace() {
        let count = map.entry(word).or_insert(0);
        *count += 1;
    }
```

hashmap的所有权最小粒度是hashmap，不是键值对

```Rust
fn reverse(v: &mut Vec<String>) {
    let n = v.len();
    for i in 0 .. n / 2 {
        let p1 = &mut v[i] as *mut String;
        let p2 = &mut v[n - i - 1] as *mut String;
        unsafe { std::ptr::swap_nonoverlapping(p1, p2, 1); }
    }
}
```

### 错误处理

Rust 认为错误不可避免，因此强制要求开发者在**编译阶段**就显式处理潜在的错误，以确保程序的健壮

Rust 将错误明确分为两类：

-   **可恢复错误**（Recoverable）：例如文件未找到，通常只需报告问题或重试。
    
-   **不可恢复错误**（Unrecoverable）：通常是 Bug（如数组越界），需要立即终止程序。
    

与大多数语言使用“异常”（Exceptions）不同，Rust 使用特定的工具来处理这两类错误：

-   用 `panic!` **宏** 处理不可恢复错误。
    
-   用 `Result<T, E>` **类型** 处理可恢复错误。
    

用 Result 的一个例子

```Rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {error:?}"),
    };
}
```

还能进一步细分处理

```Rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {e:?}"),
            },
            _ => {
                panic!("Problem opening the file: {error:?}");
            }
        },
    };
}
```

更进一步，不使用 match 的写法

```Rust
    let greeting_file = File::open("hello.txt").unwrap_or_else(|error| {
        if error.kind() == ErrorKind::NotFound {
            File::create("hello.txt").unwrap_or_else(|error| {
                panic!("Problem creating the file: {error:?}");
            })
        } else {
            panic!("Problem opening the file: {error:?}");
        }
    });
```

使用 unwrap 的例子

```Rust
let greeting_file = File::open("hello.txt").unwrap();
```

`unwrap()` 的本质就是：“如果不成功，就 panic”。

就相当于不处理失败的 result

**Expect**

```Rust
let greeting_file = File::open("hello.txt").expect("hello.txt should be included in this project");
```

expect相当于加了个错误信息的 unwrap ，失败时 panic 并且输出错误信息

Propagating Errors 错误传递

当一个函数的实现调用了一个可能失败的函数时，与其在函数内部处理错误，不如将错误返回给调用代码，由它来决定如何处理

```Rust
fn read_username_from_file() -> Result<String, io::Error> {
    let username_file_result = File::open("hello.txt");
    let mut username_file = match username_file_result {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut username = String::new();
    match username_file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e),
    }
}
```

下面是更简单的写法

```Rust
fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}
```

再缩

```Rust
fn read_username_from_file() -> Result<String, io::Error> {
    let mut username = String::new();
    File::open("hello.txt")?.read_to_string(&mut username)?;
    Ok(username)
}
```

再缩

```Rust
fn read_username_from_file() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt")
}
```

我们只能在返回 Result、Option 或实现 FromResidual 的其他类型的函数中使用 ? 操作符。

`?` 是 Rust 中用于“错误传播” (Error Propagation) 的语法糖：如果这个操作成功了，就把里面的值给我；如果出错了，立马把错误扔给调用我的函数，我也不干了

`File::open("hello.txt")?.read_to_string(&mut username)?`

在 open 返回 err 的时候就不会执行 read\_to\_string 了

```Rust
fn read_username_from_file() -> Option<String> {
  let mut username_file = File::open("hello.txt")?;
  let mut username = String::new();
  username_file.read_to_string(&mut username)?;
  Some(username)
}
```

这个代码不能通过编译，因为 ？ 会直接返回一个 result 类型，但是函数的输出是 option

修改的话可以将 option 换为 result

To panic! or Not to panic!

-   在原型设计阶段，当你还没准备好处理错误时，\` `unwrap` 和 `expect` 方法非常有用
    
-   当您有一些逻辑确保 `Result` 值为 `Ok` 时，调用 `expect` 也是合适的，但这些逻辑编译器无法理解。
    

* * *

## 021学习以太坊 第三章

### 创建合约的两个操作码

  **CREATE**

  合约地址 = keccak256( RLP(\[s ender, nonce\]) ) 的后 20 字节

  **CREATE2**

  合约地址 = keccak256(0xff + deployer + salt + keccak256(init\_code)) 的后 20 字节

CREATE2 的最大价值是可预测地址，相同输入在任意 EVM 链上会生成相同地 址

### 一、EOA 的定义与控制方式

以太坊有两种账户

-   EOA：由公私钥对控制，可以发起交易，简单来说就是发起交易或者调用合约的人
    
    -   有私钥，不可存代码，只能签名，只能交易
        
-   合约账户：用于存储被调用的合约，可以通过访问合约账户的地址执行存储的合约
    
    -   没有私钥，有地址，有余额，可持有NFT
        

一个账户有以下字段：

| 字段 | EOA (你的钱包) | 合约账户 (如 Uniswap) |
| nonce | 交易次数 | 创建过的子合约数 |
| balance | 你的 ETH 余额 | 合约持有的 ETH 余额 |
| storageRoot | 空 | 指向合约的数据存储树 |
| codeHash | 空哈希 | 指向合约的代码逻辑 |

**助记词**

-   BIP-39 定义了助记词，便于记忆
    
-   由助记词可以生成**无数个**私钥。
    
-   一组助记词能生成无数个私钥，那具体生成哪一个呢？这就要靠**派生路径**来指定。
    

$$\\text{助记词} \\xrightarrow{\\text{加盐(可选)}} \\mathbf{\\text{种子 (Seed)}} \\xrightarrow{\\text{派生路径}} \\mathbf{\\text{特定的私钥}}$$

### 二、合约账户的概念与创建流程

**合约账户**

-   有地址，有余额
    
-   有 code（合约逻辑）和 storage（状态存储），可以读写状态、转账、 调用其他合约
    
-   不能主动发起交易
    

**创建合约账户流程**

1.  编写智能合约
    
2.  编译成 EVM 字节码
    
    1.  初始化部署在链上
        
    2.  被调用
        

**如何在 Solidity 合约代码中，去调用另一个合约或给别人转账**

**方法一：使用低级接口** `.call`

```Solidity
(bool success, bytes memory data) = _targetAddress.call{value: _amount, gas: _gas}(_payload);
```

  参数

    **目标地址 (**`_targetAddress`**)**:

-   你要转账给谁？或者你要调用哪个合约？
    
-   类型：`address` 或 `address payable`。
    

    **附带金额 (**`value`**)** _(可选)_:

-   你要转多少 ETH 过去？
    
-   参数写法：`{value: 1 ether}` 或 `{value: msg.value}`。
    
-   注意：单位是 Wei。如果不写，默认为 0。
    

    **Gas 限制 (**`gas`**)** _(可选)_:

-   你限制这步操作最多消耗多少 Gas？
    
-   参数写法：`{gas: 5000}`。
    
-   建议：通常**不建议**手动指定，除非有特殊安全需求。默认会把剩余所有 Gas 传过去。
    

    **调用数据 (**`_payload`**)**:

-   **如果是纯转账**：传空字符 `""`。
    
-   **如果要调用函数**：需要使用 `abi.encodeWithSignature(...)` 对函数名和参数进行编码。
    

```Solidity
function sendMoney(address payable _to) public payable {
    // 接口：call
    // 参数：value (金额), data (空)
    (bool success, ) = _to.call{value: msg.value}("");
    
    require(success, "Transfer failed."); // 必须检查返回值
}
```

**方法二：使用接口 Interface**

```Solidity
interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
}
```

```Solidity
function callByInterface(address _tokenAddress, address _to, uint256 _amount) public {
    // 1. 实例化接口
    IERC20 token = IERC20(_tokenAddress);
    
    // 2. 直接调用 (编译器会自动处理参数编码和 call 接口)
    bool success = token.transfer(_to, _amount);
    
    require(success, "Token transfer failed");
}
```

算了，先不看 solidity 了，等之后系统的学一下吧

合约账户的能力与限制

**核心能力**

-   **管钱管数据**：既能持有 ETH/Token，又能存储复杂数据（Storage）。
    
-   **执行逻辑**：是 DEX、NFT、DAO 的载体，代码定义规则。
    
-   **链上交互**：在运行过程中，可以转账、调用别的合约，甚至生出新合约（CREATE/CREATE2）。
    

**核心限制**

-   **被动执行**：**无私钥**。它不能主动“动”起来，必须由外部（EOA）或其他合约“推一把”才能运行。
    
-   **代码固化**：部署后**代码不可改**（Code Immutable），只能修改内部数据（除非用代理模式）。
    
-   **部署昂贵**：因为占用了链上的永久存储空间，创建合约的 Gas 费很高。
    

### 三、以太坊地址“0x”开头的由来

以太坊地址本质是一串 20 字节（160 bit）的二进制数据，写成字符串时通 常用十六进制展示，于是按照编程界传统加了个前缀 0x

### 四、EOA 与合约账户的控制方式对比

| 维度 | EOA (外部账户) | CA (合约账户) |
| 控制核心 | 私钥 (Private Key) | 代码 (Code Logic) |
| 能否主动 | 能 (一切交易的起点) | 不能 (只能被动响应调用) |
| 创建成本 | 免费 (本地生成密钥对) | 昂贵 (需支付 Gas 存贮代码上链) |
| 权限管理 | 只有“拥有/未拥有”私钥两种状态 | 高度灵活 (多签、角色、白名单) |
| 主要风险 | 私钥丢失/泄露 | 代码漏洞 (Bug) / 逻辑错误 |
| 可变性 | 行为固定 (只能签名发交易) | 代码不可变 (但可经由代理升级) |
| 识别方式 | eth_getCode 返回空 | eth_getCode 返回字节码 |

由于 EOA 存在“单点故障”（私钥丢了就全完了），而合约账户可以编程，因此衍生出了两种增强安全性的机制：

**A. 多签钱包 (Multisig Wallet)**

-   **本质**：用智能合约模拟“多把钥匙开一把锁”。
    
-   **机制**：**M-of-N**。例如 3 人管理，至少 2 人签名同意才能转账。
    
-   **优势**：
    
    -   **防单点故障**：丢了一个私钥，钱还在。
        
    -   **适合团队**：DAO 国库、公司资产管理 (如 Gnosis Safe)。
        

**B. 时间锁 (Timelock)**

-   **本质**：给操作加一个“冷静期”或“公示期”。
    
-   **机制**：**提案 -> 等待延迟(Delay) -> 执行**。
    
    -   例如：管理员想修改合约参数，提交后必须等 48 小时才能生效。
        
-   **优势**：
    
    -   **防跑路/恶意篡改**：社区发现管理员作恶，可以在 48 小时内卖币退出。
        
    -   **增加透明度**：所有重大变更不仅公开，而且有预告。
        
-   **代价**：牺牲了紧急响应速度（修 Bug 也要等）。
    

### 六、合约账户的余额与状态存储

无论哪种账户，在状态树中都存储为：

$$Account = (nonce, balance, storageRoot, codeHash)$$

### 七、EOA 与合约账户的互相调用机制

### 九、ERC20 / ERC721 代币与合约账户的关系

转代币” 本质上是 “调用代币合约的一段代码 + 改一行存储

> 感觉这一章结构好像有点乱，很多重复的东西在来回讲

* * *

## Uniswap V2 学习官方文档

分为核心层和外围层

-   核心层 [https://github.com/Uniswap/v2-core](https://github.com/Uniswap/v2-core)
    
-   外围层 [https://github.com/Uniswap/v2-periphery](https://github.com/Uniswap/v2-periphery)
    

### Ecosystem Participants

**主要参与角色**

-   **流动性提供者 (LPs)**：提供资金（卖方/做市商）。
    
-   **交易者 (Traders)**：进行兑换（买方）。
    
-   **开发者 (Developers)**：构建工具和入口（基础设施）。
    

**核心机制**：

-   LPs 把代币存入池子 -> 交易者来兑换并支付 **0.30%** 的手续费 -> 这些手续费全部分给 LPs 作为奖励。
    
-   这个机制激励了更多人提供流动性，流动性越好，交易滑点越低，吸引更多交易者，产生更多手续费，从而吸引更多 LPs……
    

**A. 流动性提供者 (Liquidity Providers / LPs)**

1.  **被动型 LPs (Passive LPs)**：普通持币者，存池子里吃利息
    
2.  **专业型 LPs (Professional LPs)**：专业的做市商团队。他们使用复杂的策略和自定义工具来监控各个 DeFi 协议的收益率，并在不同项目间搬砖。
    
3.  **代币项目方 (Token Projects)**：新发币的项目方（比如发了一个新 Token "ABC"），为了让大家能买卖 "ABC"，项目方自己会充当 LP，创建一个 "ABC/ETH" 的交易对。这样一来，Uniswap 就成了这个代币的“交易所”。
    
4.  **DeFi 先驱 (DeFi Pioneers)**：探索高阶玩法的人。比如把 LP Token（你在 Uniswap 存钱后拿到的凭证）再拿去抵押借贷，实现“流动性挖矿”或“乐高组合”。
    

**B. 交易者 (Traders)**

1.  **投机者 (Speculators)**：普通的买卖交易者。比如看涨 ETH，就用 USDT 换 ETH。
    
2.  **套利机器人 (Arbitrage Bots)**：低买高卖的自动脚本，虽然它们是为了赚钱，但在客观上帮助 Uniswap 的价格**回归市场公允价格**，保持了市场的有效性。
    
3.  **DAPP 用户**：为了使用某个应用而买币。
    
4.  **智能合约 (Smart Contracts)**：比如“DEX 聚合器”（如 1inch），它是一个合约，它会自动计算发现 Uniswap 的价格最好，于是它的合约代码会自动调用 Uniswap 的合约进行交易。
    

**C. 开发者 (Developers)**

就是各种各样围绕 Uniswap 做开发的人

### Smart contracts

架构采用 核心 + 外围 的涉及

Core（核心层）**“极简主义 (Brutalist)”**。

-   代码极少，逻辑极简。容易审计，安全性越高。这是因为 Core 掌管着所有资金，绝对不能出错。
    
-   因为太简单，所以对人类很不友好（User-unfriendly），直接调用它会很麻烦且容易出错。
    
-   **包含组件**：
    
    -   **Singleton Factory (单例工厂)**：只有一个。它的任务只有两个：
        
        -   创建并索引所有的 Pair 合约（每个交易对一个）
            
            1.  每一个 Pair 合约就是一个独立的“资金池”
                
        -   控制是否开启协议收费。
            
    -   **Pairs (交易对)**：
        
        -   **做市商 (AMM)**：实际执行 `x * y = k` 逻辑的地方。
            
        -   **存钱罐**：记录池子里有多少币。
            
        -   **价格预言机**：提供历史价格数据。
            

Periphery（外围层）“用户友好”

-   它是 Core 的“管家”或“界面”。它没有特权（Permissionless），只是为了方便大家使用而写的一套工具合约。
    
-   Periphery 哪怕有 Bug，通常也不会导致 Core 里的资金直接被盗（因为资金在 Pair 里，不在 Router 里），最多是用户在使用 Router 过程中出问题。
    
-   **包含组件**：
    
    -   **Library (库)**：提供各种计算公式（比如算价格、算地址），主要是为了复用代码。
        
    -   **Router (路由)**：这是前端（Web UI）和钱包直接打交道的对象。
        
        -   **路径路由**：支持 `ETH -> A -> B -> C` 这种多跳交易。
            
        -   **ETH 支持**：把 ETH 处理成 WETH（后面会细讲）。
            
        -   **辅助功能**：比如支持用签名来移除流动性（Meta-transactions）。
            

三大关键设计决策 (Design Decisions)

Sending Tokens (代币发送机制)

-   **传统做法**：通常我们调用合约是 `approve` -> `transferFrom`。即：我授权给你，你把钱从我这拉走。
    
-   **Uniswap V2 的做法**：**“先转钱，再调用”**。
    
    -   **流程**：
        
        -   用户先把代币转账（Transfer）给 Pair 合约。
            
        -   用户调用 Pair 的 `swap` 或 `mint` 函数。
            
        -   Pair 检查：`当前余额 - 记录的余额 = 用户打进来的钱`。
            
    -   **为什么这么做？** 文档提到这部分原因在白皮书里，简单说是为了**解耦**和**安全性**，同时也支持了 **Flash Swap（闪电贷）**（这是唯一例外，可以先借钱再还）。
        

WETH (包装 ETH)

-   **V1 vs V2**：
    
    -   V1 的池子必须是 ETH ↔ ERC20。
        
    -   V2 的池子是 ERC20 ↔ ERC20。
        
-   **问题**：ETH 是原生货币，不符合 ERC-20 标准（它没有 `approve`, `transferFrom` 接口）。
    
-   **解决**：V2 Core 完全不支持 ETH。
    
    -   如果想创建 ETH 交易对，必须先把 ETH 包装成 **WETH (Wrapped ETH)**，它就是一个标准的 ERC-20 代币。
        
    -   这样 Core 的代码就不用写两套逻辑（一套处理 ETH，一套处理 ERC20），代码更干净。
        
    -   **用户感知**：用户在前端依然是用 ETH，是因为 **Router (Periphery)** 帮用户做了“ETH 转 WETH”的动作。
        

Minimum Liquidity (最小流动性销毁)

-   **机制**：当第一个人给一个新池子添加流动性时，会有 **1000 wei** (极微小的金额) 的 LP Token 被永久**销毁（Burn）**。
    
-   **目的**：**防数学攻击**。
    
    -   如果不销毁，攻击者可以通过操控初始流动性，构造出极其极端的价格或数量（比如把 totalSupply 弄得非常小），从而利用舍入误差（Rounding Errors）窃取后续用户的资金。
        
    
    > 在 Solidity（以太坊编程语言）中，**除法是向下取整的**。
    > 
    > -   `3 / 2 = 1.5` -> 在 Solidity 中结果是 **1**。
    >     
    > -   `1 / 2 = 0.5` -> 在 Solidity 中结果是 **0**。
    >     
    > 
    > 如果算出来的结果不到 1，就会变成 0。如果你存了钱，但系统计算你应该得到 `0.99` 个凭证，系统就会给你 **0** 个凭证。你的钱进去了，凭证没拿到，钱就归池子里其他人了。
    > 
    > 攻击者利用 `TotalSupply = 1`，通过捐赠操控分母，让后续用户的 `(存款 * 1) / 储备量` 计算结果小于 1，利用 Solidity 的舍入为 0 特性，让用户“存了钱却拿不到凭证”。
    > 
    > 强行把 `TotalSupply` 拉大 1000 倍。攻击者想操控比例，成本也随之扩大 1000 倍甚至更多，导致攻击在经济上不划算。
    
    -   销毁这 1000 wei 相当于提高了攻击门槛（成本），让这种数学攻击变得极其昂贵且不可行。
        
    -   对于正常用户，这 1000 wei 的价值几乎为零，可以忽略不计。
        

### Glossary

-   **utomated market maker(自动做市商)**：自动做市商是以太坊上的一个智能合约，它持有链上流动性储备。用户可以根据自动做市商公式设定的价格，与这些储备进行交易。
    
-   **Constant product formula(恒定乘积公式)**：Uniswap 使用的自动做市算法。参见 x\*y=k。
    
-   **ERC20(ERC20)**：ERC20 代币是以太坊上的同质化代币。Uniswap 支持所有标准的 ERC20 实现。
    
-   **Factory(工厂合约)**：一个智能合约，用于为任意 ERC20/ERC20 交易对部署一个唯一的智能合约。
    
-   **Pair(交易对合约)**：由 Uniswap V2 工厂合约部署的智能合约，用于实现两种 ERC20 代币之间的交易。
    
-   **Pool(资金池)**：交易对内的流动性是汇集了所有流动性提供者资金的集合。
    
-   **Liquidity provider / LP(流动性提供者 / LP)**：流动性提供者是指将等值的两种 ERC20 代币存入交易对流动性池中的人。流动性提供者承担价格风险，并获得费用作为补偿。
    
-   **Mid price(中间价)**：在特定时刻用户买入和卖出代币价格的中间值。在 Uniswap 中，这是两种 ERC20 代币储备量的比率。
    
-   **Price impact(价格影响)**：中间价与一笔交易实际成交价格之间的差额（通常由交易规模引起）。
    
-   **Slippage(滑点)**：交易对的价格在交易“提交时”与“实际执行时”之间的变动量（通常由市场波动或抢跑引起）。
    
-   **Core(核心合约)**：Uniswap 赖以生存的基础智能合约。升级核心合约的新版本需要进行流动性迁移。
    
-   **Periphery(外围合约)**：有用但非 Uniswap 存在所必需的外部智能合约。部署新的外围合约无需迁移流动性。
    
-   **Flash swap(闪电兑换)**：一种允许在支付代币费用之前就可以先使用所购买代币的交易模式。
    
-   **x _y = k(x_ y = k)**：恒定乘积公式。
    
-   **Invariant(不变量/恒定值)**：恒定乘积公式中的 "k" 值。
    

### Swaps

当提取（购买）任一代币时，必须存入（卖出）一定比例的另一种代币，以维持常数不变。

在最基础的层面上，Uniswap V2 中的所有兑换都发生在一个函数中，该函数被恰当地命名为 `swap`：

```Solidity
function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data);
```

**Uniswap V2 模式 (Transfer + Swap)**

1.  **用户**：直接把 100 USDT **推（Push / Transfer）** 到 Pair 合约地址。
    
2.  **用户**：调用 Pair 合约的 `swap` 函数。
    
3.  **Pair**：看一眼自己兜里现在的钱，再查一下账本上原来的钱，算出差额：
    
4.  `当前余额 (Balance)` - `账本储备 (Reserve)` = `用户刚才打进来的钱 (Input)`。
    
5.  **Pair**：根据这个差额，算出该给你多少目标代币，然后发给你。
    

如果在非原子操作中，先转币给交易对再调用 swap 是不安全的，因为发送的代币容易被套利者截获

### Pools 资金池

-   **冷启动**：池子刚创建时是空的（余额为 0）。
    
-   **定价权**：**第一个**存入流动性的人（Initial LP），他存入的 Token A 和 Token B 的**比例**，直接决定了池子的**初始价格**。
    
-   **风险约束**：如果第一个人乱定价（比如存入比例偏离市场价），套利者（Arbitrageurs）会立刻进场搬砖，把便宜的币买走，导致第一个人亏损。
    
    -   _结论：市场博弈迫使 LP 必须按公允价格存币。_
        

**Pool tokens (资金池代币 / 流动性代币)**

  **LP Token = 存款收据**：当你存入流动性，你会收到 `Liquidity Tokens`。

-   **首次铸造公式**：数量 = sqrt(x \* y)。
    
-   **后续存入**：必须按当前池子的价格比例存入。
    

  **收益来源**：每笔交易收取的 **0.3% 手续费**，会按比例分给所有 LP。

-   注意：手续费不会直接打到你钱包，而是加到池子里，让你的 LP Token 变得更值钱。
    

  **退出机制**：把 LP Token 发回给合约（**Burn/销毁**），合约把对应的本金 + 赚到的手续费退给你。

### Flash Swaps

-   **什么是 Flash Swap？** 它允许你从 Uniswap 的池子里**预先提取**任意数量的代币（甚至把池子借空），拿去干任何你想干的事（比如去别的平台卖掉）。
    
-   **唯一的条件**： 这一切必须在同一个交易（同一区块）内完成。在交易结束的那一刹那，你必须：
    
    -   **方案 A**：用另一种代币支付刚才借走的代币（相当于买）。
        
    -   **方案 B**：把借走的代币原本奉还，并支付少量手续费（相当于借）。
        
-   **如果不还钱？** 整个交易会直接 **Revert（回滚）**，就像这一切从未发生过一样。
    

**场景 A：无本金套利 (Capital Free Arbitrage)**

-   **痛点**：以前发现差价（Uniswap 便宜，Oasis 贵），你得自己先有本金买下便宜的，再去卖。没钱就只能看着机会溜走。
    
-   **Flash Swap 解法**：
    
    -   **借**：从 Uniswap 借出 1 ETH（此时还没付钱）。
        
    -   **卖**：去 Oasis 把这 1 ETH 卖成 220 DAI。
        
    -   **还**：回到 Uniswap，按照当时的价格（比如 200 DAI）还债。
        
    -   **赚**：剩下的 20 DAI 就是你的利润，你实际上是**空手套白狼**（除了 Gas 费）。
        
-   **意义**：实现了**套利的民主化**，任何人只要有技术（代码），不需要有资本，就能抹平市场差价。
    

**场景 B：瞬时杠杆 (Instant Leverage)**

-   **痛点**：以前想在 MakerDAO 做 2 倍杠杆，需要“抵押 -> 借贷 -> 买币 -> 再抵押 -> 再借贷...”循环多次，操作繁琐且 Gas 费极高。
    
-   **Flash Swap 解法**：
    
    -   **借**：直接从 Uniswap 借出你最终想要的总 ETH 数额。
        
    -   **存**：把这些 ETH 一次性存入 MakerDAO。
        
    -   **铸**：生成 DAI。
        
    -   **还**：用生成的 DAI 偿还 Uniswap 的闪电贷。
        
-   **意义**：把复杂的多步操作压缩进一个原子交易中，省钱、省心。
    

* * *

歇逼了，今天就先学到这了

-   **入门技术&深度技术任务**
    
    -   021 学习以太坊第 3 章✅
        
    -   [**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)**⭕**
        
    -   [Uniswap V2 学习官方文档](https://docs.uniswap.org/contracts/v2/overview?utm_source=chatgpt.com)✅
        
-   **Rust基础**
    
    -   第八章 common collection✅
        
    -   第九章 error handling ✅
        
-   **杂项**
    
    -   把小登试卷er图那题改完吧⭕
        

要学会跟自己和解

明明今天好像一直都在学，但是怎么感觉进度那么慢？

嗷，好像是下午打牌去了

唉，唉唉

明天再加把劲吧

试卷再不改老登估计要催了

想一下

明天周三，后天周四

明天要把企业项目的方案整理一下，组会给老登看

然后还有论文的代码实验，拖到周五六日干吧

明天改卷子，明天必须要改卷子了

今天就先这样吧，明天再努力

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NjM5MWQwOGExNmRkYzE5ZTAxNzVlZWRmM2ZmYTllM2JfSGJxaHM0U0lRVXlha0NjSUVXbkNwSEkyQllyZEcxdmNfVG9rZW46UkVCTWJDUk91bzNxNzR4MHhENGNXelpwbkpoXzE3NjgzMTgxMDg6MTc2ODMyMTcwOF9WNA)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->












（叽里咕噜写了半天不小心点到白框外面的内容之后全部内容没有了，后续干脆直接把飞书文档上的学习记录复制粘贴过来吧）

总之，先给接下来的一段时间设定一个总体的目标吧，平常也有记录学习笔记的习惯，干脆一起记录吧，反正差不多也都算是沾点边的东西

## 四周目标

1.  完成入门技术学习任务
    
2.  完成深度技术学习任务
    
3.  完成论文实验（随缘吧，回家前做完就行）
    
4.  完成rust基础语法学习（争取第三周之前完成）
    
5.  尽量做一个reth二开的项目（或者说至少开始做）
    

## w1d1

### todo

-   **入门技术&深度技术任务**
    
    -   Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分
        
    -   Web3 实习手册[「安全与合规」](https://web3intern.xyz/zh/security/)部分
        
    -   021 学习以太坊第 1-3 章
        
    -   Ethereum 官网 [Overview](https://ethereum.org/learn/) 基础内容学习
        
    -   [**Day 1: A Developer’s Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro
        
    -   [**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)
        
    -   工具安装指南：[https://web3intern.xyz/zh/remote-work-guide/](https://web3intern.xyz/zh/remote-work-guide/)
        
    
    > 昨天装完了，这个不管了
    
    -   [Uniswap V2 学习官方文档](https://docs.uniswap.org/contracts/v2/overview?utm_source=chatgpt.com)
        
-   **Rust基础**
    
    -   第八章 common collection
        
    -   第九章 error handling
        
-   **杂项**
    
    -   把小登试卷er图那题改完吧
        

* * *

### Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分

大部分好像是之前已经了解到的内容

web3.0好像没怎么听说过，好像是给数据集打标签一样

最后看到作者好像还是熟人

**尝试“《我的第一个 NFT》：**[**https://nft.myfirst.io/”**](https://nft.myfirst.io/”)

-   捯饬了半天一直提示401，看后续的教程说需要**Gas Fee，**以为是因为钱包里面没钱导致认证不了等等
    

> 之后看到群里才知道不是我这边的问题

-   然后在币安买了 0.0003 个 eth ，本来想把币安的地址直接导入到 metamask ，但是找半天没有找到
    
-   之后想着直接把 0.0003 转到 metamask 的钱包，被告知“不足0.003无法转账”
    
-   索性又买了点，然后转账到 metamask 钱包
    
-   网站修复后发现需要的是 SepoliaETH ，糖丸了
    
-   然后又扒拉了会任务文档找到水龙头流了会，拿到测试币成功铸造了
    
-   然后顺便在群里找到别人的地址转账完成了另外一个任务
    

**以太坊结构**

Layer 1

-   以太坊主网：核心区块链，负责最终安全性与共识。
    
-   EVM：以太坊虚拟机，执行智能合约代码。
    
-   账户系统：外部账户（EOA）与合约账户（CA）共同构成网络基础。
    

Layer 2

-   Rollup：通过将交易批量处理后提交至 L1，降低 Gas 费。
    
    -   Optimistic Rollup：假设交易合法，仅在争议时验证。
        
    -   ZK Rollup：通过零知识证明验证交易，无需链上争议。
        

> **L1 是负责“提供”共识机制和安全性的层（不做改变）。L2 是负责“剥离”计算工作，利用 L1 的共识进行最终确认的层。**
> 
> L2 并不是让 L1 的共识机制本身变快了（L1 该慢还是慢），而是**通过减少 L1 需要处理的工作量**，变相实现了效率的提升。
> 
> 这块之前一直没弄的特别细，虽然知道 L2 是干什么的，但是两个层的区别一直模模糊糊

### 021 学习以太坊第 1-3 章

我天，这个量好大

不过大部分好像都是已经了解的内容，跳着看吧

  第一章：认识以太坊

-   智能合约
    
-   dApp
    
-   Tokens
    
-   PoS
    
-   Gas
    
-   Proto-Danksharding（EIP-4844）与 Dencun 升级
    
    -       主要引入了 blob
        
    -       什么是 blob？临时数据结构，存储在执行层
        
    -       简单来说，就是一些用完了之后，共识节点可以直接删除的一些数据
        
-   分片与 Danksharding
    

  分片有两种方案，旧的分片方案是将以太坊分为64个链

  新的分片方案是在主链上挂载一个**超级巨大的数据，**存储更多的blob

  但是这样还存在一个问题，共识节点需要存放更多，Danksharding 存在是为了解决这个问题

-   叔块（Uncle / Ommer Block）
    

**gas费计算**

实际支付费用 ≈ (Base Fee + Priority Fee) × Gas Used

-   Base Fee：协议根据区块拥堵情况自动调整，并在执行后被销毁；
    
-   Priority Fee：用户自愿支付的小费，用于激励验证者优先打包；
    
-   钱包里看到的 maxFeePerGas / maxPriorityFeePerGas 是你愿意支 付的上限，实际扣费不会超过这两个上限组合对应的值。
    

第二章 网络结构与节点类型

**以太坊中节点通信**

1.  节点**通过 UDP 运行 Kademlia DHT 协议**来发现邻居
    
2.  建立 tcp 链接
    
3.  新交易通过 Gossip 协议扩散
    

**Kademlia**

-   每个分拨中心有一个“编号”（节点 ID）；
    
-   每个中心都记着“在不同距离区间内，离我最近的几个中心”；
    
-   当要把快递寄往一个目标编号时，会一步步转发给“离目标更近”的中 心，越转越接近收件人；
    
-   最终快递就被精准送到目的地。
    

> 感觉跟路由的那个算法差不多

**Gossip**

1.  当节点收到一笔新交易，验证通过后，随机选择一部分邻居发送交易哈希。
    
2.  邻居如果没有这笔交易，就会请求下载完整交易数据，然后再转发给它的邻居。
    

**归档节点**

就是超级全节点，全节点会定时清除太早的区块信息，归档节点不会

**开发者或机构运行全节点的必要性**

-   避免将用户 IP 和钱包地址暴露给第三方 RPC 服务商
    
-   摆脱公共节点的限流、封锁或宕机风险，确保业务在任何时候都能自主广播交易。
    
-   本地独立校验每一笔区块和交易的真实性
    
-   增强网络健壮性，增加独立节点和多样化客户端，防止单点故障
    
-   获得毫秒级的本地读写延迟和自定义 RPC 接口
    
-   支持搭建归档节点查询全量历史状态，满足合规审计、量化回测及复杂数据分析需求。
    
-   在网络升级或发生硬分叉时拥有自主选择权（用脚投票），而不必被动跟随服务商的路线。
    
-   可完全根据业务和合规要求，自由定制客户端类型、同步策略、日志留存及访问控制权限。
    

**执行客户端**

-   处理&检验交易，执行evm操作
    
-   维护世界状态
    
-   提供 RPC 接口
    
-   与其他节点交换数据
    

**共识客户端**

-   共识，出块
    
-   追踪链头与最终性，就是处理分叉的情况
    
-   管理验证者集与奖励 / 惩罚
    
-   维护 p2p 网络
    
-   Beacon API / 共识 RPC
    

**“出块 / 同步”流程**

1.  节点被选择为 proposer，执行客户端生成 execution payload
    
2.  执行客户端执行交易，返回完整 payload
    
3.  共识客户端将 execution payload 打包放入 block ，作为提议的区块，然后广播
    
4.  其他节点的公式客户端接收并验证区块，包括
    
    1.  验证 PoS 签名、slot/epoch、历史链等共识规则；
        
    2.  每个节点的执行客户端再次执行交易，验证合法性
        
    3.  所有通过则该块被正式发布，开始下一轮循环
        

调用 eth\_\* / 部署合约 / 查状态？

→ 找的是 执行客户端（EVM + JSON-RPC）。

质押 32 ETH 做验证者 / 关心 attestation、finality、slashing？

→ 这是 共识客户端 + 验证者客户端 的工作。

> 就先把前两章看完吧，剩下的明天再看，感觉好多东西都忘记了，打算再想办法补一下之前看的区块链课程的视频

### Rust

vector

基础用法

```Rust
fn main() {
    let v1: Vec<i32> = Vec::new();  // 创建一个空的动态数组,Vec必须指定类型
    let v2: Vec<i32> = vec![1, 2, 3, 4, 5];
    let mut v3 = Vec::new(); // 可变动态数组,类型由编译器推断,后续添加元素时确定类型
    v3.push(10); // 添加元素

    let third: &i32 = &v2[2]; // 使用索引访问元素,返回引用,如果索引越界会panic
    let third_opt: Option<&i32> = v2.get(2); // 使用get方法访问元素,返回Option类型,越界返回None。用 get 方法默认是不可变引用
    
}
```

```Rust
    let mut v = vec![100, 32, 57];
    for i in &mut v {
        *i += 50;
    }
```

下面这个代码是错的

```Rust
fn main() {
  let v = vec![String::from("Hello ")];
  let mut s = v[0];
  s.push_str("world");
  println!("{s}");
}
```

其中`let mut s = v[0];` 错误

不能通过索引（Indexing）把值从集合中“搬走”（Move out）。

但是 i32 可以这样写，因为 i32 是直接拷贝

使用迭代器遍历vec

```Rust
    let mut v: Vec<i32> = vec![1, 2];
    let mut iter: Iter<'_, i32> = v.iter();
    let n1: &i32 = iter.next().unwrap();
    let n2: &i32 = iter.next().unwrap();
    let end: Option<&i32> = iter.next();
```

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MmI1MTY5NmU4ZjU3ZWQxMzNjNGRjNTRhOTA3YzQ5ZTJfam94MjdRakhGZ2swcjVYSTFrS0dsbmJ0YWJqN0JYZUVfVG9rZW46S05FSWJ4bmZ5b1AxQlB4ZUFpY2NFV2dNbkdjXzE3NjgyMzEyMDg6MTc2ODIzNDgwOF9WNA)

next() 方法的逻辑是：先返回当前元素的引用，然后将迭代器内部的指针向后移动一位

类似cpp中的i++

使用索引遍历动态数组

```Rust
use std::ops::Range; 
let mut v: Vec<i32>        = vec![1, 2];
let mut iter: Range<usize> = 0 .. v.len();
let i1: usize              = iter.next().unwrap();
let n1: &i32               = &v[i1];
```

在 vec 里面镶一个 enum 可以让 vec 中存不同类型的元素

```Rust
    enum SpreadsheetCell {
        Int(i32),
        Float(f64),
        Text(String),
    }

    let row = vec![
        SpreadsheetCell::Int(3),
        SpreadsheetCell::Text(String::from("blue")),
        SpreadsheetCell::Float(10.12),
    ];
```

```Rust
fn main() {
  let mut v: Vec<i32> = vec![1, 2, 3];
  let mut v2: Vec<&mut i32> = Vec::new();
  for i in &mut v {
    v2.push(i);
  }
  *v2[0] = 5;
  let a = *v2[0];
  let b = v[0];
  println!("{a} {b}");
}
```

v2 存放 v 每个元素的引用，读写权在 v2

`*v2[0] = 5;` 修改 v\[0\]

`let a = *v2[0]; let b = v[0];`两句因为是 i32 数据类型，直接复制，换别的数据类型会 move 导致报错，然后v2在后续没有使用，v 重新获得读写权

## 杂项

rust是对的，冲！

web3招聘平台

-   SmartDeer
    
-   Web3.carrer
    
-   Remote3
    
-   CryptoJobsList
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YTRhNmU4YTZlZGRlMWE3Zjk1OGUyM2JlYmVkNDIyZDlfT1RyM3E4aWRqMm1rTVNRSThhNWw5OU5jdUp6Rnk5M2hfVG9rZW46WHhsRGJnbG95bzBOTHp4OVdtamM4OGhyblhnXzE3NjgyMzEyMDg6MTc2ODIzNDgwOF9WNA)

如何进入web3

-   从web2迁移
    
    -   英文！
        
    -   独立交付项目成果
        
-   从生态侧接入
    
    -   DAO 中接任务
        
    -   开源社区贡献代码
        

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MWY2ZjJiN2Q0MzIwZDZlYWE5OWI2YjBkMzhjMDY3ZGZfczZtMmY2U0pLbkJLZTNBVWl2VWxzY1BZQks5anBzYzFfVG9rZW46TXY0emJZZ21mbzdMQWN4bXlscWNIYURlbkZnXzE3NjgyMzEyMDg6MTc2ODIzNDgwOF9WNA)

Rust是对的！！！！！！

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MDgwZjRiODZlMDA4OGY4OTg2MjY5ZGU2YWQ2NWI4MjBfWHVHVUpMMGpWdEtvYXJ5UTlFSU1EUzNSM25TcEpuTHVfVG9rZW46THI4c2J0Vkdqb2Z1QWx4QW9SMWN1U3lXbnRkXzE3NjgyMzEyMDg6MTc2ODIzNDgwOF9WNA)

## 今日总结

-   **入门技术&深度技术任务**
    
    -   Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分✅
        
    -   Web3 实习手册[「安全与合规」](https://web3intern.xyz/zh/security/)部分✅
        
    -   021 学习以太坊第 1-3 章⭕
        
    
    > 太多了，有点高估自己了，只看完前两章，明天再努力吧
    
    -   Ethereum 官网 [Overview](https://ethereum.org/learn/) 基础内容学习✅
        
    -   [**Day 1: A Developer's Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro✅
        
    -   [**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)**⭕**
        
    
    > 学不完了
    
    -   工具安装指南：[https://web3intern.xyz/zh/remote-work-guide/✅](https://web3intern.xyz/zh/remote-work-guide/✅)
        
    
    > 昨天装完了，这个不管了
    
    -   [Uniswap V2 学习官方文档](https://docs.uniswap.org/contracts/v2/overview?utm_source=chatgpt.com)⭕
        
    
    > 相信明天的自己吧还是
    
-   **Rust基础**
    
    -   第八章 common collection⭕
        
    -   第九章 error handling ⭕
        
    
    > 😭😭😭rust，我的rust
    
-   **杂项**
    
    -   把小登试卷er图那题改完吧
        
    
    > 明天再干
    

感觉今天大概是开会占的时间有点太多了）

唉，明天再加油吧

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDJlMDQ5NDgxYTE3ZTA2MjYxOGEwMzY0MDU4OWQzZjNfZWp5OVZmV2NRMWtJODQ5R3lGWVAwenlvTUFSb3pxOElfVG9rZW46WWhKRmI4QlU1b0xaelV4aFlMWmNMNDhYbnBoXzE3NjgyMzEyMDg6MTc2ODIzNDgwOF9WNA)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
