---
timezone: UTC+8
---

# jacksoncheng-87

**GitHub ID:** jacksoncheng-87

**Telegram:** @jacksoncheng_87

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->
**gas 优化**

基本原理与计量单位：Gas 是 EVM 执行操作的单位。每条指令消耗固定的 gas。

优化目标是减少交易所需的总 gas，提高用户体验并降低成本。

常见优化技巧：

1.  减少存储操作（Storage Write）
    

读取存储第一次需 2100 gas（后续 100 gas），而内存读取仅 3 gas。推荐多次访问同一存储数据时，将其缓存到内存以减少 SLOAD 次数

每次写入 storage 的成本高达 20,000 gas；优先使用 memory。

优化写法（一次读，一次写）

function deposit() public payable {

uint256 current = balances\[msg.sender\];

balances\[msg.sender\] = current + msg.value;

}

2.使用位压缩（Bit Packing）

将多个变量压缩到一个 uint256 中以节省存储空间。

示例：

struct Packed {

uint128 a;

uint128 b;

}

3.循环优化

减少不必要的运算，如 array.length 缓存到变量中。

// 优化

uint256 len = arr.length;

for (uint i = 0; i < len; ++i) {

...

}

4.函数可见性选择 - external 比 public 更节省 gas，适用于仅被外部调用的函数

合约安全：

安全设计原则:最小权限原则（Least Privilege）;模块化结构便于审计;显式错误处理与事件记录

常见漏洞类型与防护

1.  重入攻击 Reentrancy
    

利用外部合约在 fallback 中重新调用原函数。历史上最著名的 The DAO 事件便因重入漏洞导致约 6000 万美元 ETH 被盗，最终造成以太坊社区分裂（形成 ETH/ETC 链）。

防护方法：先更新状态，再转账。

示例：

// ❌ 有漏洞

function withdraw() public {

require(balance\[msg.sender\] > 0);

(bool sent,) = [msg.sender.call](http://msg.sender.call){value: balance\[msg.sender\]}("");

require(sent);

balance\[msg.sender\] = 0;

}

// ✅ 修复后

function withdraw() public {

uint256 amount = balance\[msg.sender\];

balance\[msg.sender\] = 0;

(bool sent,) = [msg.sender.call](http://msg.sender.call){value: amount}("");

require(sent);

}

2.预言机操纵 Oracle Manipulation

依赖外部价格源的不可信更新。

解决方法：

-   使用 Chainlink 等权威价格源。
    
-   增加时序约束和多源验证。
    
-   使用 TWAP 等加权算法。
    

3.整数溢出/下溢

-   使用 unchecked {} 时需确保逻辑安全。
    
-   推荐使用Solidity 0.8+ 的内建溢出检查或 SafeMath。
    

4.权限控制缺失

-   所有管理函数应使用 onlyOwner 或 AccessControl 修饰符保护。
    

5.未初始化代理

-   基于代理模式的合约若未正确执行初始化函数，可能被任意人初始化并接管合约。
    
-   著名的例子包括 Harvest Finance 其在使用 Uniswap V3 做市策略的 Vault 合约中存在未初始化漏洞，如果被利用攻击者可销毁实现合约。该团队曾为此漏洞支付高额赏金修复。
    

6.前置交易/三明治攻击

-   攻击者在交易执行前后分别发送交易，以不利滑点或套利为目的。
    
-   例如 2025 年 3 月，一名用户在 Uniswap V3 的稳定币兑换中遭遇三明治攻击，约 21.5 万美元的 USDC 兑换几乎被抢跑，损失了 98% 的资金
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

Solidity Basic 笔记

| 主题 | 说明 | 关键代码示例 | 要点 |
| --- | --- | --- | --- |
| 1. Hello World | 简单合同，返回字符串。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract HelloWorld {
    function helloWorld() external pure returns (string memory) {
        return "Hello, World";
    }
} | 使用 pure 不改状态；pragma 指定版本。 |
| 2. First App | 计数器示例，状态变量交互。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Counter {
    uint256 public count;

    function increment() external {
        count += 1;
    }
} | public 自动 getter；external 外部调用。 |
| 3. Primitive Data Types | 基本类型：bool, uint/int, address, bytes。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Primitives {
    bool public boo = true;

    uint8 public u8 = 1;
    uint256 public u256 = 456;
    uint public u = 123; // uint is alias for uint256

    int8 public i8 = -1;
    int256 public i256 = 456;
    int public i = -123; // int is same as int256

    int public minInt = type(int).min;
    int public maxInt = type(int).max;

    address public addr = 0xCA35b7d915458EF540aDe6068dFe2F44E8fa733c;

    bytes1 public a = 0xb5; //  [10110101]
    bytes1 public b = 0x56; //  [01010110]
} | uint/int 有大小变体；默认值如 0 或 false。 |
| 4. Variables | 类型：状态（持久）、本地（临时）、全局（区块链 info）。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Variables {
    // State variables are stored on the blockchain.
    string public text = "Hello";
    uint256 public num = 123;

    function doSomething() public view {
        // Local variables are not saved to the blockchain.
        uint256 i = 456;
        // Here are some global variables
        uint256 timestamp = block.timestamp; // Current block timestamp
        address sender = msg.sender; // address of the caller
    }
} | 全局如 msg.sender、block.timestamp。 |
| 5. Constants | 不可改值，节省 gas。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Constants {
    // coding convention to uppercase constant variables
    address public constant MY_ADDRESS = 0x777788889999AaAAbBbbCcccddDdeeeEfFFfCcCc;
    uint public constant MY_UINT = 123;
} | 大写命名惯例；编译时替换。 |
| 6. Immutable | 部署时赋值，不可改。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Immutable {
    // coding convention to uppercase constant variables
    address public immutable MY_ADDRESS;
    uint public immutable MY_UINT;

    constructor(uint _myUint) {
        MY_ADDRESS = msg.sender;
        MY_UINT = _myUint;
    }
} | 构造函数赋值；类似常量节省 gas。 |
| 7. Reading and Writing to a State Variable | 状态变量 getter/setter。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract SimpleStorage {
    uint public num;

    function set(uint _num) public {
        num = _num;
    }

    function get() public view returns (uint) {
        return num;
    }
} | 修改需非-view 函数。 |
| 8. Ether and Wei | 单位转换：1 ether = 10^18 wei。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract EtherUnits {
    uint public oneWei = 1 wei;
    // 1 wei is equal to 1
    bool public isOneWei = 1 wei == 1;

    uint public oneEther = 1 ether;
    // 1 ether is equal to 10^18 wei
    bool public isOneEther = 1 ether == 10**18;
} | 使用后缀如 wei, ether；防溢出。 |
| 9. Gas and Gas Price | Gas 是计算单位；示例无限循环耗尽 gas。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Gas {
    uint public i = 0;

    // Using up all of the gas that you send causes your contract to fail.
    // State changes are undone.
    // Gas spent are not refunded.
    function forever() public {
        // Here we run a loop until all of the gas are spent
        // and the transaction fails
        while (true) {
            i += 1;
        }
    }
} | solidity<br>function forever() public {<br> while (true) {<br> i += 1;<br> }<br>}<br> |
| 10. If / Else | 条件语句，支持三元。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract IfElse {
    function foo(uint x) public pure returns (uint) {
        if (x < 10) {
            return 0;
        } else if (x < 20) {
            return 1;
        } else {
            return 2;
        }
    }

    function ternary(uint _x) public pure returns (uint) {
        return _x < 10 ? 1 : 2;
    }
} | 纯函数中使用。 |
| 11. For and While Loop | 循环，支持 break/continue。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Loop {
    function loop() public pure {
        // for loop
        for (uint i = 0; i < 10; i++) {
            if (i == 3) {
                // Skip to next iteration with continue
                continue;
            }
            if (i == 5) {
                // Exit loop with break
                break;
            }
        }

        // while loop
        uint j;
        while (j < 10) {
            j++;
        }
    }
} | 循环耗 gas，小心无限循环。 |
| 12. Mapping | 键值对，默认值 0/false。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Mapping {
    // Mapping from address to uint
    mapping(address => uint) public myMap;

    function set(address _addr, uint _i) public {
        // Update the value at this address
        myMap[_addr] = _i;
    }

    function get(address _addr) public view returns (uint) {
        // Mapping always returns a value.
        // If the value was never set, it will return the default value.
        return myMap[_addr];
    }

    function remove(address _addr) public {
        // Reset the value to the default value.
        delete myMap[_addr];
    }
}

contract NestedMapping {
    // Nested mapping (mapping from address to another mapping)
    mapping(address => mapping(uint => bool)) public nested;

    function set(address _addr1, uint _i, bool _boo) public {
        nested[_addr1][_i] = _boo;
    }

    function get(address _addr1, uint _i) public view returns (bool) {
        return nested[_addr1][_i];
    }

    function remove(address _addr1, uint _i) public {
        delete nested[_addr1][_i];
    }
} | 支持嵌套；delete 重置默认。 |
| 13. Array | 动态/固定数组，push/pop。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Array {
    // Several ways to initialize an array
    uint[] public arr;
    uint[] public arr2 = [1, 2, 3];
    // Fixed sized array, all elements initialize to 0
    uint[10] public myFixedSizeArr;

    function get(uint i) public view returns (uint) {
        return arr[i];
    }

    // Solidity can return the entire array.
    // But this function should be avoided for
    // arrays that can grow indefinitely in length.
    function getArr() public view returns (uint[] memory) {
        return arr;
    }

    function push(uint i) public {
        // Append to array
        // This will increase the array length by 1.
        arr.push(i);
    }

    function pop() public {
        // Remove last element from array
        // This will decrease the array length by 1
        arr.pop();
    }

    function getLength() public view returns (uint) {
        return arr.length;
    }

    function remove(uint index) public {
        // Delete does not change the array length.
        // It resets the value at index to it's default value,
        // in this case 0
        delete arr[index];
    }

    function examples() external {
        // create array in memory, only fixed size can be created
        uint[] memory a = new uint[](5);
    }
} | delete 不变长度，只重置值；内存数组固定。 |
| 14. Enum | 定义状态，索引从 0。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Enum {
    // Enum representing shipping status
    enum Status {
        Pending,
        Shipped,
        Accepted,
        Rejected,
        Canceled
    }

    // Default value is the first element listed in
    // definition of the type, in this case "Pending"
    Status public status;

    // Returns uint
    // Pending  - 0
    // Shipped  - 1
    // Accepted - 2
    // Rejected - 3
    // Canceled - 4
    function get() public view returns (Status) {
        return status;
    }

    // Update status by passing uint into input
    function set(Status _status) public {
        status = _status;
    }

    // You can update to a specific enum like this
    function cancel() public {
        status = Status.Canceled;
    }

    // delete resets the enum to its first value, 0
    function reset() public {
        delete status;
    }
} | delete 重置为 0。 |
| 15. User Defined Value Types | 自定义类型，基于现有。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

type UFixed256x18 is uint256;

library FixedMath {
    uint constant multiplier = 10**18;

    function mul(UFixed256x18 a, UFixed256x18 b) internal pure returns (UFixed256x18) {
        return UFixed256x18.wrap(UFixed256x18.unwrap(a) * UFixed256x18.unwrap(b));
    }

    function floor(UFixed256x18 a) internal pure returns (uint256) {
        return UFixed256x18.unwrap(a) / multiplier;
    }

    function toUFixed256x18(uint256 a) internal pure returns (UFixed256x18) {
        return UFixed256x18.wrap(a * multiplier);
    }
}

contract UserDefinedValueTypes {
    UFixed256x18 public a = FixedMath.toUFixed256x18(5);
    UFixed256x18 public b = FixedMath.toUFixed256x18(2);

    function multiply() public view returns (uint256) {
        return FixedMath.floor(FixedMath.mul(a, b));
    }
} | 需要库支持操作。 |
| 16. Structs | 数据分组。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract TodoList {
    struct TodoItem {
        string text;
        bool completed;
    }

    // An array of 'TodoItem' structs
    TodoItem[] public todos;

    function create(string memory _text) public {
        // 3 ways to initialize a struct
        // - calling it like a function
        todos.push(TodoItem(_text, false));

        // key value mapping
        todos.push(TodoItem({text: _text, completed: false}));

        // initialize an empty struct and then update it
        TodoItem memory todo;
        todo.text = _text;
        // todo.completed initialized to false

        todos.push(todo);
    }

    // Solidity automatically created a getter for 'todos' so
    // you don't actually need this function.
    function get(uint _index) public view returns (string memory text, bool completed) {
        TodoItem storage todo = todos[_index];
        return (todo.text, todo.completed);
    }

    // update text
    function update(uint _index, string memory _text) public {
        TodoItem storage todo = todos[_index];
        todo.text = _text;
    }

    // update completed
    function toggleCompleted(uint _index) public {
        TodoItem storage todo = todos[_index];
        todo.completed = !todo.completed;
    }
} | 多初始化方式；storage/memory。 |
| 17. Data Locations | storage (持久), memory (临时), calldata (只读)。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract DataLocations {
    uint[] public arr;
    mapping(uint => address) map;
    struct MyStruct {
        uint foo;
    }
    mapping(uint => MyStruct) myStructs;

    function f() public {
        // call _f with state variables
        _f(arr, map, myStructs[1]);

        // get a struct from a mapping
        MyStruct storage myStruct = myStructs[1];
        // create a struct in memory
        MyStruct memory myMemStruct = MyStruct(0);
    }

    function _f(
        uint[] storage _arr,
        mapping(uint => address) storage _map,
        MyStruct storage _myStruct
    ) internal {
        // do something with storage variables
    }

    // You can return memory variables
    function g(uint[] memory _arr) public returns (uint[] memory) {
        // do something with memory array
    }

    function h(uint[] calldata _arr) public {
        // do something with calldata array
    }
} | calldata 节省 gas。 |
| 18. Transient Storage | 交易内存储，assembly 使用。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract TransientStorage {
    bytes32 constant SLOT = keccak256("some_slot");

    function set(uint256 value) public {
        assembly {
            tstore(SLOT, value)
        }
    }

    function get() public view returns (uint256 ret) {
        assembly {
            ret := tload(SLOT)
        }
    }
} | 仅交易有效，节省 gas。 |
| 19. Function | 函数参数/返回，多值支持。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Function {
    // Functions can return multiple values.
    function returnMany() public pure returns (uint, bool, uint) {
        return (1, true, 2);
    }

    // Return values can be named.
    function named() public pure returns (uint x, bool b, uint y) {
        return (1, true, 2);
    }

    // Return values can be assigned to their name.
    // In this case the return statement can be omitted.
    function assigned() public pure returns (uint x, bool b, uint y) {
        x = 1;
        b = true;
        y = 2;
    }

    // Use destructuring assignment when calling another
    // function that returns multiple values.
    function destructuringAssignments() public pure {
        (uint i, bool boo, uint j) = returnMany();

        // Values can be left out.
        (uint x, , uint y) = (4, 5, 6);
    }

    // Cannot use map for either input or output

    // Can use array for input
    function arrayInput(uint[] memory _arr) public {}

    // Can use array for output
    uint[] public arr;

    function arrayOutput() public view returns (uint[] memory) {
        return arr;
    }
} | 命名返回可省 return；支持数组输入/输出。 |
| 20. View and Pure Functions | view 读状态，pure 纯计算。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract ViewAndPure {
    uint256 public x = 1;

    // Promise not to modify the state.
    function addToX(uint256 y) public view returns (uint256) {
        return x + y;
    }

    // Promise not to modify or read from the state.
    function add(uint256 i, uint256 j) public pure returns (uint256) {
        return i + j;
    }
} | 调用不耗 gas（除非内部调用）。 |
| 21. Error | 错误处理：require/revert/assert，自定义 error。 | // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Error {
    function testRequire(uint _i) public pure {
        // Require should be used to validate conditions such as:
        // - inputs
        // - conditions before execution
        // - return values from calls to other functions
        require(_i > 10, "Input must be greater than 10");
    }

    function testRevert(uint _i) public pure {
        // Revert is useful when the condition to check is complex.
        // This code does the exact same thing as the above require
        if (_i <= 10) {
            revert("Input must be greater than 10");
        }
    }

    uint public num;

    function testAssert() public view {
        // Assert should only be used to test for internal errors,
        // and to check invariants.

        // Here we assert that num is always equal to 0
        // since it is impossible to update the value of num
        assert(num == 0);
    }

    // custom error
    error InsufficientBalance(uint balance, uint withdrawAmount);

    function testCustomError(uint _withdrawAmount) public view {
        if (_withdrawAmount > address(this).balance) {
            revert InsufficientBalance({
                balance: address(this).balance,
                withdrawAmount: _withdrawAmount
            });
        }
    }
} | require 输入验证；assert 内部；自定义节省 gas。 |

Solidity 的基础部分聚焦于智能合约的核心构建块，从简单"Hello World"示例入手，逐步引入数据类型（Primitives 如 uint/int、address）、变量管理（状态/本地/全局、Constants/Immutable）、Ether 单位与 Gas 优化；控制结构包括 If/Else、循环（For/While），数据结构有 Mapping（键值对）、Array（动态/固定）、Enum（状态定义）、Structs（自定义分组）和自定义 Value Types；函数方面，强调 View/Pure（不改状态）、多返回值、数据位置（Storage/Memory/Calldata）和 Transient Storage（交易临时），错误处理使用 Require/Revert/Assert 和自定义 Error，提高合约安全性。

整体，这些概念为构建可靠的 Ethereum 合约奠基，强调 Gas 效率和状态管理，避免常见 pitfalls 如无限循环或溢出。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


补充昨天内容：

**6.引用类型**

-   数组 array：是Solidity常用的一种变量类型，用来存储一组数据（整数，字节，地址等等）。数组分为固定长度数组和可变长度数组两种： （1）：固定长度数组：在声明时指定数组的长度。用T\[k\]的格式声明，其中T是元素的类型，k是长度；
    

（2）：可变长度数组（动态数组）：在声明时不指定数组的长度。用T\[\]的格式声明，其中T是元素的类型；

-   bytes比较特殊，是数组，但是不用加\[\]。另外，不能用byte\[\]声明单字节数组，可以使用bytes或bytes1\[\]。bytes 比 bytes1\[\] 省gas。
    
-   创建数组的规则：
    

（1）：对于memory修饰的动态数组，可以用new操作符来创建，但是必须声明长度，并且声明后长度不能改变；

（2)数组字面常数(Array Literals)是写作表达式形式的数组，用方括号包着来初始化array的一种方式，并且里面每一个元素的type是以第一个元素为准的，例如\[1,2,3\]里面所有的元素都是uint8类型，因为在Solidity中，如果一个值没有指定type的话，会根据上下文推断出元素的类型，默认就是最小单位的type，这里默认最小单位类型是uint8。而\[uint(1),2,3\]里面的元素都是uint类型，因为第一个元素指定了是uint类型了，里面每一个元素的type都以第一个元素为准。 下面的例子中，如果没有对传入 g() 函数的数组进行 uint 转换，是会报错的;

(3)如果创建的是动态数组，你需要一个一个元素的赋值。

-   数组成员
    
    length: 数组有一个包含元素数量的length成员，memory数组的长度在创建后是固定的。
    
    push(): 动态数组拥有push()成员，可以在数组最后添加一个0元素，并返回该元素的引用。
    
    push(x): 动态数组拥有push(x)成员，可以在数组最后添加一个x元素。
    
    pop(): 动态数组拥有pop()成员，可以移除数组最后一个元素。
    
-   结构体 struct
    
    Solidity支持通过构造结构体的形式定义新的类型。结构体中的元素可以是原始类型，也可以是引用类型；结构体可以作为数组或映射的元素。创建结构体的方法：
    
    // 结构体
    
    struct Student{
    
    uint256 id;
    
    uint256 score;
    
    }
    
    Student student; // 初始一个student结构体
    
-   给结构体赋值
    
    方法1:在函数中创建一个storage的struct引用；
    

方法2:直接引用状态变量的struct；

方法3:构造函数式；

方法4:key value;

**7.映射类型**

-   映射Mapping:在映射中，人们可以通过键（Key）来查询对应的值（Value）
    

声明映射的格式为mapping(\_KeyType => _ValueType)，其中_ KeyType和\_ValueType分别是Key和Value的变量类型;

-   映射的规则
    
    规则1：映射的\_KeyType只能选择Solidity内置的值类型，比如uint，address等，不能用自定义的结构体。而\_ValueType可以使用自定义的类型;
    

规则2：映射的存储位置必须是storage，因此可以用于合约的状态变量，函数中的storage变量和library函数的参数（见例子）。不能用于public函数的参数或返回结果中，因为mapping记录的是一种关系 (key - value pair);

规则3：如果映射声明为public，那么Solidity会自动给你创建一个getter函数，可以通过Key来查询对应的Value;

规则4：给映射新增的键值对的语法为\_Var\[\_Key\] = _Value，其中_Var是映射变量名，\_Key和\_Value对应新增的键值对。

-   映射的原理
    
    原理1: 映射不储存任何键（Key）的资讯，也没有length的资讯;
    
    原理2: 对于映射使用keccak256(h(key) . slot)计算存取value的位置。感兴趣的可以去阅读 WTF Solidity 内部规则: 映射存储布局;
    
    原理3: 因为Ethereum会定义所有未使用的空间为0，所以未赋值（Value）的键（Key）初始值都是各个type的默认值，如uint的默认值是0。
    

**8.变量初始值**

-   类型初始值
    
    boolean: false
    
    string: ""
    
    int: 0
    
    uint: 0
    
    enum: 枚举中的第一个元素
    
    address: 0x0000000000000000000000000000000000000000 (或 address(0))
    
    function
    
    internal: 空白函数
    
    external: 空白函数
    
-   引用类型初始值
    

映射mapping: 所有元素都为其默认值的mappin

结构体struct: 所有成员设为其默认值的结构体

数组array

动态数组: \[\]

静态数组（定长）: 所有成员设为其默认值的静态数组

-   delete操作符:delete a会让变量a的值变为初始值。
    

**9.常数**

Solidity中和常量相关的两个关键字constant（常量）和immutable（不变量）。状态变量声明这两个关键字之后，不能在初始化后更改数值。这样做的好处是提升合约的安全性并节省gas;另外，只有数值变量可以声明constant和immutable；string和bytes可以声明为constant，但不能为immutable。

-   constant:constant变量必须在声明的时候初始化，之后再也不能改变。尝试改变的话，编译不通过。
    
-   immutable:immutable变量可以在声明时或构造函数中初始化，因此更加灵活。在Solidity v0.8.21以后，immutable变量不需要显式初始化，未显式初始化的immutable变量将使用数值类型的初始值（见 8. 变量初始值）。反之，则需要显式初始化。 若immutable变量既在声明时初始化，又在constructor中初始化，会使用constructor初始化的值。
    

**10.控制流**

-   与其他语言类似：if-else；for循环；while循环；do-while循环；三元运算符；另外还有continue（立即进入下一个循环）和break（跳出当前循环）关键字可以使用。
    
-   用Solidity实现插入排序
    

与python不同在于：// 插入排序 正确版

function insertionSort(uint\[\] memory a) public pure returns(uint\[\] memory) {

// note that uint can not take negative value

for (uint i = 1;i < a.length;i++){

uint temp = a\[i\];

uint j=i;

while( (j >= 1) && (temp < a\[j-1\])){

a\[j\] = a\[j-1\];

j--;

}

a\[j\] = temp;

}

return(a);

}

**11.构造函数和修饰器**

-   构造函数（constructor）：是一种特殊的函数，每个合约可以定义一个，并在部署合约的时候自动运行一次。它可以用来初始化合约的一些参数；
    
-   注意：构造函数在不同的Solidity版本中的语法并不一致，在Solidity 0.4.22之前，构造函数不使用 constructor 而是使用与合约名同名的函数作为构造函数而使用，由于这种旧写法容易使开发者在书写时发生疏漏（例如合约名叫 Parents，构造函数名写成 parents），使得构造函数变成普通函数，引发漏洞，所以0.4.22版本及之后，采用了全新的 constructor 写法。
    
-   修饰器（modifier）：是Solidity特有的语法，类似于面向对象编程中的装饰器（decorator），声明函数拥有的特性，并减少代码冗余。它就像钢铁侠的智能盔甲，穿上它的函数会带有某些特定的行为。modifier的主要使用场景是运行函数前的检查，例如地址，变量，余额等。
    
-   OpenZeppelin的Ownable标准实现
    

**12.事件（用转账ERC20代币为例）**

-   事件：Solidity中的事件（event）是EVM上日志的抽象，它具有两个特点：
    
-   响应：应用程序（ethers.js）可以通过RPC接口订阅和监听这些事件，并在前端做响应。
    
-   经济：事件是EVM上比较经济的存储数据的方式，每个大概消耗2,000 gas；相比之下，链上存储一个新变量至少需要20,000 gas
    
-   声明事件：事件的声明由event关键字开头，接着是事件名称，括号里面写好事件需要记录的变量类型和变量名；
    
-   释放事件：我们可以在函数里释放事件。在下面的例子中，每次用\_transfer()函数进行转账操作的时候，都会释放Transfer事件，并记录相应的变量；
    
-   EVM日志 ：以太坊虚拟机（EVM）用日志Log来存储Solidity事件，每条日志记录都包含主题topics和数据data两部分；
    
-   主题 topics：日志的第一部分是主题数组，用于描述事件，长度不能超过4。它的第一个元素是事件的签名（哈希），除了事件哈希，主题还可以包含至多3个indexed参数，也就是Transfer事件中的from和to；
    
-   数据 data：事件中不带 indexed的参数会被存储在 data 部分中，可以理解为事件的“值”。data 部分的变量不能被直接检索，但可以存储任意大小的数据。因此一般 data 部分可以用来存储复杂的数据结构，例如数组和字符串等等，因为这些数据超过了256比特，即使存储在事件的 topics 部分中，也是以哈希的方式存储。另外，data 部分的变量在存储上消耗的gas相比于 topics 更少。
    

**13：继承**

-   继承：是面向对象编程很重要的组成部分，可以显著减少重复代码。如果把合约看作是对象的话，Solidity也是面向对象的编程，也支持继承；
    
-   规则： virtual: 父合约中的函数，如果希望子合约重写，需要加上virtual关键字；
    

override：子合约重写了父合约中的函数，需要加上override关键字；

注意：用override修饰public变量，会重写与变量同名的getter函数。

-   简单继承：
    
-   多重继承：
    
    Solidity的合约可以继承多个合约。规则：
    
    继承时要按辈分最高到最低的顺序排。比如我们写一个Erzi合约，继承Yeye合约和Baba合约，那么就要写成contract Erzi is Yeye, Baba，而不能写成contract Erzi is Baba, Yeye，不然就会报错；
    
    如果某一个函数在多个继承的合约里都存在，比如例子中的hip()和pop()，在子合约里必须重写，不然会报错；
    
    重写在多个父合约中都重名的函数时，override关键字后面要加上所有父合约名字，例如override(Yeye, Baba)。
    
-   修饰器的继承
    
    Solidity中的修饰器（Modifier）同样可以继承，用法与函数继承类似，在相应的地方加virtual和override关键字即可；
    
-   构造函数的继承
    
    子合约有两种方法继承父合约的构造函数。举个简单的例子，父合约A里面有一个状态变量a，并由构造函数的参数来确定：
    

在继承时声明父构造函数的参数，例如：contract B is A(1)；

在子合约的构造函数中声明构造函数的参数。

-   调用父合约的函数
    

子合约有两种方式调用父合约的函数，直接调用和利用super关键字。

（1）直接调用：子合约可以直接用父合约名.函数名()的方式来调用父合约函数

（2）super关键字：子合约可以利用super.函数名()来调用最近的父合约函数。Solidity继承关系按声明时从右到左的顺序是：contract Erzi is Yeye, Baba，那么Baba是最近的父合约，super.pop()将调用Baba.pop()而不是Yeye.pop()。

-   钻石继承：在面向对象编程中，钻石继承（菱形继承）指一个派生类同时有两个或两个以上的基类。
    

**14.抽象合约和接口(用ERC721的接口合约为例)**

-   抽象合约
    
    如果一个智能合约里至少有一个未实现的函数，即某个函数缺少主体{}中的内容，则必须将该合约标为abstract，不然编译会报错；另外，未实现的函数需要加virtual，以便子合约重写。
    
-   接口类似于抽象合约，但它不实现任何功能。接口的规则：
    

不能包含状态变量

不能包含构造函数

不能继承除接口外的其他合约

所有函数都必须是external且不能有函数体

继承接口的非抽象合约必须实现接口定义的所有功能

虽然接口不实现任何功能，但它非常重要。接口是智能合约的骨架，定义了合约的功能以及如何触发它们：如果智能合约实现了某种接口（比如ERC20或ERC721），其他Dapps和智能合约就知道如何与它交互。因为接口提供了两个重要的信息：

(1)合约里每个函数的bytes4选择器，以及函数签名函数名(每个参数类型）。

(2)接口id（更多信息见EIP165）

另外，接口与合约ABI（Application Binary Interface）等价，可以相互转换：编译接口可以得到合约的ABI，利用abi-to-sol工具，也可以将ABI json文件转换为接口sol文件。

-   IERC721包含3个事件，其中Transfer和Approval事件在ERC20中也有。
    

Transfer事件：在转账时被释放，记录代币的发出地址from，接收地址to和tokenId。

Approval事件：在授权时被释放，记录授权地址owner，被授权地址approved和tokenId。

ApprovalForAll事件：在批量授权时被释放，记录批量授权的发出地址owner，被授权地址operator和授权与否的approved。

-   IERC721函数
    

balanceOf：返回某地址的NFT持有量balance。

ownerOf：返回某tokenId的主人owner。

transferFrom：普通转账，参数为转出地址from，接收地址to和tokenId。

safeTransferFrom：安全转账（如果接收方是合约地址，会要求实现ERC721Receiver接口）。参数为转出地址from，接收地址to和tokenId。

approve：授权另一个地址使用你的NFT。参数为被授权地址approve和tokenId。

getApproved：查询tokenId被批准给了哪个地址。

setApprovalForAll：将自己持有的该系列NFT批量授权给某个地址operator。

isApprovedForAll：查询某地址的NFT是否批量授权给了另一个operator地址。

safeTransferFrom：安全转账的重载函数，参数里面包含了data。

-   什么时候使用接口:
    

如果我们知道一个合约实现了IERC721接口，我们不需要知道它具体代码实现，就可以与它交互。

**15.异常**

-   error：是solidity 0.8.4版本新加的内容，方便且高效（省gas）地向用户解释操作失败的原因，同时还可以在抛出异常的同时携带参数，帮助开发者更好地调试。人们可以在contract之外定义异常；
    
-   require命令：是solidity 0.8版本之前抛出异常的常用方法，目前很多主流合约仍然还在使用它。它很好用，唯一的缺点就是gas随着描述异常的字符串长度增加，比error命令要高。使用方法：require(检查条件，"异常的描述")，当检查条件不成立的时候，就会抛出异常；
    
-   Assert：一般用于程序员写程序debug，因为它不能解释抛出异常的原因（比require少个字符串）。它的用法很简单，assert(检查条件），当检查条件不成立的时候，就会抛出异常。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



**soildity的深入学习**

**1.HelloWeb3(三行代码)**

-   Solidity 是一种用于编写以太坊虚拟机（EVM）智能合约的编程语言；Remix 是以太坊官方推荐的智能合约集成开发环境（IDE）；
    

注：Solidity 语句以分号（;）结尾

// SPDX-License-Identifier: MIT //第 1 行是注释，说明代码所使用的软件许可（license）这里指MIT许可，Solidity 注释以“//”开头，后面跟注释内容，注释不会被程序执行。

pragma solidity ^0.8.21; //第 2 行声明源文件所使用的 Solidity 版本，这行代码表示源文件将不允许小于 0.8.21 版本或大于等于 0.9.0 的编译器编译

contract HelloWeb3{

string public \_string = "Hello Web3!"; //第3-4 行是合约部分。第 3 行创建合约（contract），并声明合约名为 HelloWeb3。第 4 行是合约内容，声明了一个 string（字符串）变量 \_string，并赋值为 "Hello Web3!"。

}

-   编译（按住CTRL+S即可）并部署代码：
    

点击 Deploy（黄色按钮），即可部署我们编写的合约，部署成功后，在下方会看到名为 HelloWeb3 的合约。点击 \_string，即可看到 "Hello Web3!"。

**2.值类型**

-   _值类型_(Value Type) ：包括布尔型，整数型等等，这类变量赋值时候直接传递数值。
    

1\. 布尔型（bool）：二值变量，取值为 true 或 false；

2\. 整型：整型是 Solidity 中的整数，最常用的包括：

int public \_int = -1; // 整数，包括负数

uint public \_uint = 1; // 无符号整数

uint256 public \_number = 20220330; // 256位无符号整数

3.地址（address）：分成两种：

普通地址（address）: 存储一个 20 字节的值（以太坊地址的大小）。

payable address: 比普通地址多了 transfer 和 send 两个成员方法，用于接收转账；

4\. 定长字节数组：

属于值类型，数组长度在声明之后不能改变。根据字节数组的长度分为 bytes1, bytes8, bytes32 等类型。定长字节数组最多存储 32 bytes 数据，即bytes32；

5\. 枚举 enum（冷门了解）

-   _引用类型_(Reference Type) ：包括数组和结构体，这类变量占空间大，赋值时候直接传递地址（类似指针）。
    
-   _映射类型_(Mapping Type) : Solidity中存储键值对的数据结构，可以理解为哈希表
    

**3.函数**

solidity中的函数的形式：

function <function name>(\[parameter types\[, ...\]\]) {internal|external|public|private} \[pure|view|payable\] \[virtual|override\] \[<modifiers>\]

\[returns (<return types>)\]{ <function body> }

(注：(方括号中的是可写可不 写的关键字))

-   function：声明函数时的固定用法。要编写函数，就需要以 function 关键字开头；
    
-   <function name>：函数名；
    
-   (\[parameter types\[, ...\]\])：圆括号内写入函数的参数，即输入到函数的变量类型和名称；
    
-   {internal|external|public|private}：函数可见性说明符，共有4种：
    

public：内部和外部均可见；

private：只能从本合约内部访问，继承的合约也不能使用；

external：只能从合约外部访问（但内部可以通过 this.f() 来调用，f是函数名）；

internal: 只能从合约内部访问，继承的合约可以用。

tips：合约中定义的函数需要明确指定可见性，它们没有默认值；public|private|internal 也可用于修饰状态变量(定义可参考WTF Solidity 第5讲的相关内容)。public变量会自动生成同名的getter函数，用于查询数值。未标明可见性类型的状态变量，默认为internal。

-   \[pure|view|payable\]：决定函数权限/功能的关键字。payable（可支付的）很好理解，带着它的函数，运行的时候可以给合约转入 ETH。pure 和 view 的介绍见下一节；
    
-   \[virtual|override\]: 方法是否可以被重写，或者是否是重写方法。virtual用在父合约上，标识的方法可以被子合约重写。override用在自合约上，表名方法重写了父合约的方法；
    
-   <modifiers>: 自定义的修饰器，可以有0个或多个修饰器；
    
-   \[returns ()\]：函数返回的变量类型和名称；
    
-   <function body>: 函数体。
    

pure和view的区别：solidity 引入这两个关键字主要是因为 以太坊交易需要支付气费（gas fee）。合约的状态变量存储在链上，gas fee 很贵，如果计算不改变链上状态，就可以不用付 gas。包含 pure 和 view 关键字的函数是不改写链上状态的，因此用户直接调用它们是不需要付 gas 的（注意，合约中非 pure/view 函数调用 pure/view 函数时需要付gas）。

总结：`view`函数可以读取状态变量，但不能改写；`pure` 函数既不能读取也不能改写状态变量，这是其他语言中没有出现过的。

**4.函数输出**

-   返回值：return 和 returns；
    

returns：跟在函数名后面，用于声明返回的变量类型及变量名；

return：用于函数主体中，返回指定的变量。

-   命名式返回
    

我们可以在 returns 中标明返回变量的名称。Solidity 会初始化这些变量，并且自动返回这些变量的值，无需使用 return。

eg：// 命名式返回

function returnNamed() public pure returns(uint256 _number, bool_ bool, uint256\[3\] memory \_array){

\_number = 2;

\_bool = false;

\_array = \[uint256(3),2,1\];

}

当然命名式返回，依然支持return

-   解构式赋值
    

读取所有返回值：声明变量，然后将要赋值的变量用,隔开，按顺序排列；

读取部分返回值：声明要读取的返回值对应的变量，不读取的留空。

**5.变量数据存储和作用域**

引用类型(Reference Type) ：包括数组（array）和结构体（struct），由于这类变量比较复杂，占用存储空间大，我们在使用时必须要声明数据存储的位置。

-   数据位置：storage：合约里的状态变量默认都是storage，存储在链上，类似计算机的硬盘，**消耗gas多；**
    
    memory：函数里的参数和临时变量一般用memory，存储在内存中，不上链。尤其是如果返回数据类型是变长的情况下，必须加memory修饰，例如：string, bytes, array和自定义结构；
    
    calldata：和memory类似，存储在内存中，不上链。与memory的不同点在于calldata变量不能修改（immutable），一般用于函数的参数。
    

**memory 和calldata**均是临时存在内存里，**消耗gas少**，整体消耗gas从多到少依次为：storage > memory > calldata

-   数据位置和赋值规则：
    

赋值本质上是创建引用指向本体，因此修改本体或者是引用，变化可以被同步：

storage（合约的状态变量）赋值给本地storage（函数里的）时候，会创建引用，改变新变量会影响原变量；

memory赋值给memory，会创建引用，改变新变量会影响原变量。

其他情况下，赋值创建的是本体的副本，即对二者之一的修改，并不会同步到另一方。这有时会涉及到开发中的问题，比如从storage中读取数据，赋值给memory，然后修改memory的数据，但如果没有将memory的数据赋值回storage，那么storage的数据是不会改变的。

-   变量的作用域：
    

1\. 状态变量：状态变量是数据存储在链上的变量，所有合约内函数都可以访问，gas消耗高。状态变量在合约内、函数外声明；并且可以在函数里更改状态变量的值；

2\. 局部变量：局部变量是仅在函数执行过程中有效的变量，函数退出后，变量无效。局部变量的数据存储在内存里，不上链，gas低。局部变量在函数内声明；

3\. 全局变量：全局变量是全局范围工作的变量，都是solidity预留关键字。他们可以在函数内不声明直接使用；

注：常见的全局变量：msg.sender代表请求发起地址；

block.number代表当前区块高度；

msg.dat代表请求数据；

blockhash(uint blockNumber): (bytes32) 给定区块的哈希值 – 只适用于最近的256个区块, 不包含当前区块。

block.coinbase: (address payable) 当前区块矿工的地址

block.gaslimit: (uint) 当前区块的gaslimit

block.number: (uint) 当前区块的number

block.timestamp: (uint) 当前区块的时间戳，为unix纪元以来的秒

gasleft(): (uint256) 剩余 gas

msg.data:(bytes calldata) 完整call data

msg.sender: (address payable) 消息发送者 (当前 caller)

msg.sig: (bytes4) calldata的前四个字节 (function identifier)

msg.value: (uint) 当前交易发送的 wei 值

block.blobbasefee: (uint) 当前区块的blob基础费用。这是Cancun升级新增的全局变量。

blobhash(uint index): (bytes32) 返回跟当前交易关联的第 index 个blob的版本化哈希（第一个字节为版本号，当前为0x01，后面接KZG承诺的SHA256哈希的最后31个字节）。若当前交易不包含blob，则返回空字节。这是Cancun升级新增的全局变量。

4\. 全局变量-以太单位与时间单位:

-   以太单位
    

Solidity中不存在小数点，以0代替为小数点，来确保交易的精确度，并且防止精度的损失，利用以太单位可以避免误算的问题，方便程序员在合约中处理货币交易。

wei: 1

gwei: 1e9 = 1000000000

ether: 1e18 = 1000000000000000000

-   时间单位
    

可以在合约中规定一个操作必须在一周内完成，或者某个事件在一个月后发生。这样就能让合约的执行可以更加精确，不会因为技术上的误差而影响合约的结果。因此，时间单位在Solidity中是一个重要的概念，有助于提高合约的可读性和可维护性。

seconds: 1

minutes: 60 seconds = 60

hours: 60 minutes = 3600

days: 24 hours = 86400

weeks: 7 days = 604800
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




课上笔记

## **一、EVM存储架构**

**四大存储区：**

-   **Storage**：永久存储，Gas消耗最高
    
-   **Memory**：临时存储，Gas消耗低
    
-   **Stack**：运算栈
    
-   **Code**：只读输入
    

**优化策略：**

-   用uint8/uint16替代uint256节省空间
    
-   临时变量用Memory不用Storage
    
-   减少Storage读写次数
    

## **二、ERC-20代币合约**

**账本系统设计：**

-   balances映射：记录每个地址余额
    
-   allowances映射：记录授权额度
    

**必备功能实现：**

-   **transfer()**：转账 = require验证 + 余额扣减/增加 + event广播
    
-   **approve()**：授权他人操作代币
    
-   **balanceOf()**：查询余额（view函数）
    
-   **mint()**：铸造新币（internal限制）
    

**关键技术点：**

-   **require**：条件验证，失败自动回滚
    
-   **msg.sender**：获取调用者地址
    
-   **event**：链上事件通知
    
-   **函数修饰符**：
    
-   external/internal：控制调用权限
    
-   view/pure：控制状态修改
    

Remix基础学习部分：

**Remix在线编辑器**：其中左侧的第四个 debugg图标 可以快速的让我们找到代码错误点

在 Remix 的交互面板里，按钮的颜色代表了极重要的含义，计费逻辑：蓝色随便点，橙色要花钱，红色是打钱。

| 按钮颜色 | 含义 | 专业术语 | 是否花钱(Gas)? | 是否需要确认? |
| --- | --- | --- | --- | --- |
| 蓝色 | 查 (只读) | view / pure | 免费 | 瞬间返回，不用等 |
| 橙色 | 改 (写入) | Transaction | 付费 | 需要矿工打包，需要等 |
| 红色 | 付 (转账) | payable | 付费+转账 | 既执行代码，又接收 ETH |

记住常见的误区：

-   重要的代码要复制保存到本地电脑，或者使用 Remix 里的 "Connect to [Localhost](http://Localhost)" 功能（进阶），避免刷新网址代码没了；
    
-   回到 Compiler 页面，看看有没有红色的错误提示。修好 bug 才能部署；
    
-   在 Environment 里，把 Remix VM 切换成 Injected Provider - MetaMask。这时 Remix 就会连接你浏览器里的小狐狸钱包，用你钱包里的真（测试）币来部署。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->





**智能合约开发**

**1\. DApp 整体架构与开发全流程**

**DApp 核心四层架构**

-   **前端（UI/UX）**：React/Vue + Wagmi/Viem，负责钱包连接、展示数据、触发交易
    
-   **智能合约**：Solidity，部署在 EVM 兼容链上，核心业务逻辑 + 状态存储
    
-   **Indexer / 数据检索器**：The Graph Subgraph 或 Ponder，监听 Event → 结构化存入 Postgres/ClickHouse 等，便于前端复杂查询
    
-   **区块链 + 去中心化存储**：EVM 链（状态 + 交易） + IPFS/Arweave（NFT 元数据、图片、大文件）
    

**典型 DApp 开发完整流程**（6 大阶段）

1.  需求分析与规划 → 选链（Ethereum L1 / L2 / Alt-L1）、核心功能、Gas 敏感度
    
2.  智能合约开发 → 编写 + Hardhat/Foundry 测试 + OpenZeppelin 继承 + 安全审计
    
3.  Indexer 开发 → 事件解析 + 数据库建模（Ponder / Subgraph）
    
4.  前端开发 → 钱包集成（Wagmi） + 合约交互（Viem） + UI 状态管理
    
5.  与链交互 & 测试 → 测试网全流程走通
    
6.  部署上线 → 合约验证（Etherscan/Sourcify） + 前端 Vercel/IPFS + 监控
    

**2\. 开发环境搭建（2025-2026 推荐）**

**基础工具**

-   Node.js LTS（用 nvm 管理）
    
-   yarn / pnpm（推荐 pnpm 更快）
    
-   Git
    

**两大主流开发框架对比**

**推荐钱包**：MetaMask（开发必备）  
**推荐前端库**（2025主流）：Wagmi + Viem（取代 ethers.js + Web3.js）

**3\. RPC 节点服务（最容易踩坑的部分）**

**RPC 作用总结**

-   读：eth\_call、eth\_getBalance、eth\_getLogs
    
-   写：eth\_sendTransaction、eth\_estimateGas
    
-   订阅：eth\_subscribe（新区块、日志）
    

**2025-2026 主流 RPC 服务商对比**

**最佳实践**

-   永远不要把 API Key 写死在前端代码
    
-   配置多 Provider failover（Viem 支持）
    
-   生产环境开启重试 + 指数退避
    
-   监控请求量，避免超限被封
    

**4\. Solidity 核心知识点（重点）**

**当前推荐版本**：pragma solidity ^0.8.20;（内置溢出检查）

**常用数据类型**

-   uint256 / uint（别名 uint256）
    
-   address / address payable
    
-   mapping(address => uint256)
    
-   struct + mapping 组合
    
-   bytes32（哈希、固定长度）、string、bytes
    

**函数可见性 & 状态修饰符**

-   public / external / internal / private
    
-   view / pure（不改状态，可省 Gas）
    
-   payable（可收 ETH）
    

**事件 & 索引**

event MessageLeft(address indexed user, string message, uint256 timestamp);

**安全编码规范（必须记住）**

1.  **Checks-Effects-Interactions**（CEI）模式
    
2.  使用 OpenZeppelin 的 ReentrancyGuard
    
3.  继承 Ownable / AccessControl
    
4.  Solidity ≥0.8 自动防整数溢出
    
5.  避免 tx.origin
    
6.  外部调用前更新状态
    

**5\. 入门实战项目：链上留言板（Remix 实现）**

**部署验证步骤**

1.  Remix → 编译
    
2.  环境选 Injected Provider - MetaMask
    
3.  切换到 Sepolia 测试网
    
4.  部署 → 确认交易
    
5.  复制合约地址 → Etherscan 验证源码
    

**6\. 测试网 & 部署必备知识**

**主流测试网（2025-2026）**

-   Sepolia（最活跃，Alchemy/Infura 都支持）
    
-   Holesky（ staking 测试）
    

**领取测试币**

-   Sepolia Faucet：[https://sepolia-faucet.pk910.de/](https://sepolia-faucet.pk910.de/) 或 Alchemy
    
-   Holesky：[https://holesky-faucet.pk910.de/](https://holesky-faucet.pk910.de/)
    

**合约验证**

-   Remix 部署后直接在 Etherscan 验证
    
-   Hardhat/Foundry 使用 forge verify-contract / hardhat verify
    

**7\. 前端与合约交互（Viem + Wagmi 写法示例）**

**8\. 高阶进阶方向**

-   **Gas 优化**：打包存储、用 uint128 代替 uint256、事件代替 storage、避免循环写 storage
    
-   **安全审计**：Slither / Mythril / Aderyn / 4naly3er / 手动审计
    
-   **代理模式**：透明代理 / UUPS（OpenZeppelin 可升级合约）
    
-   **Layer 2**：Arbitrum / Optimism / Base / zkSync / Scroll
    
-   **EIP 相关**：EIP-4337（账户抽象）、EIP-3074、EIP-7702
    
-   **工具链**：Foundry + Huff（极致 Gas）、ApeWorX、Brownie（Python）
    

**学习建议**

1.  先用 Remix 快速上手 3-5 个小合约
    
2.  迁移到 Hardhat 或 Foundry 做完整项目
    
3.  完成 1 个完整 DApp（前端 + 合约 + Indexer）
    
4.  读 OpenZeppelin 源码 + 做 Damn Vulnerable DeFi / Ethernaut
    
5.  参与 Code4rena / Sherlock 审计竞赛
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->







**一周总结**

这一周从零摸索Web3，区块链本质是一台停不下来的全球共享电脑，用代码和激励让互不信任的人可靠协作，从平台许可转向私钥即一切。ENS成了链上永久身份证，DEX无需KYC直接换币，NFT的链上存储带来真正的永久性和可组合性，而L2和多签工具把Gas贵、卡顿、踩坑的真实痛苦降到可接受范围。节点自己跑才最信任、抗审查，合约账户代码写死基本不可改，代币NFT不过是合约里的记账表。安全底线是助记词绝不截图云存，转账核对地址，钓鱼和红线（ICO、返利、场外）一碰就翻车。总之，Web3把控制权交给用户，但代价是自己全责——贵、慢、麻烦，却也自由、震撼、值得。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->








学习别人的X笔记

一、AI相关技术名词（核心从“会聊天”→“能赚钱的打工人”）

1、大语言模型 (LLM) → AI的“超级大脑 / 项目经理” 懂需求、定计划，但自己不执行，靠工具落地。

2、自回归TOKEN预测器 → “接龙式说话机器” 早期AI本质，只能逐词猜下一句，无法真正“做事”。

3、Agentic AI（智能体AI） → “有手有脚有身份证的AI打工人” 2026主流形态：有记忆（向量DB）、能调用工具（Function Call）、有链上身份（ERC8004）、能自主赚钱。

4、向量数据库 + Embedding → AI的“永久记忆本 + 条形码系统” 把文字/知识转数字向量 → 快速检索、不忘事，解决金鱼记忆。

5、Function Call → AI的“手脚指令接口” 让AI不是发文字，而是发可执行命令（查价、交易、发邮件）。

6、ReAct结构 → “想→做→看→调整”的循环工作流 让AI像人一样迭代完成复杂任务（典型套利流程）。

7、ZKML + TEE → “保密证明 + 硬件保险箱” 证明AI按规则执行（没作恶、没泄密），但不暴露模型/数据核心。

8、API → “软件间服务员 / 沟通桥梁” AI干活全靠各种API（数据、交易、计算），是“说到做到”的物理通道。

二、Web3相关技术名词（给AI提供“身份、钱包、自动执行”）

1 区块链 → “不可篡改的公共账本” 给AI决策、交易、身份上链 → 信任、可审计。

2 以太坊 → Web3的“主舞台 / 民政局” ERC8004等AI身份标准的主要部署链。

3 智能合约 → “无人值守的自动售货机” 条件满足自动执行（套利触发、支付等）。

4 NFT / ERC721 → “独一无二的数字身份证模板” ERC8004基于此，给每个AI发唯一、不可伪造身份。

5 Gas费 → “链上办事邮费” 防止刷分、女巫攻击，也构成AI链上操作的真实成本。

6 钱包 + 私钥 → AI的“数字银行卡 + 家门钥匙” 存资产、签名交易，Sparkos帮忙安全托管。

7 稳定币 → “价格不波动的链上美元” AI小额、高频交易的首选货币（结算量已超Visa）。

8 DeFi → “无中介自动金融超市” AI最主要赚钱场景（套利、借贷、流动性提供）。

9 IPFS / 跨链桥 / TPS → “分布式硬盘 + 跨国通道 + 高速公路车道数” 存代码/数据不丢、资产跨链流动、高并发支持亿级AI交易。

三、协议/框架相关（2025-2026年AI+Web3基础设施关键件）

1 ERC8004 → AI的“链上身份证 + 信誉档案 + 验证登记”标准 基于NFT（唯一性）+ 链上声誉 + 验证证明，让AI从黑户变合法公民。

2 X402 → “HTTP自带收款功能”的AI原生支付协议 复活HTTP 402状态码，支持超小额（0.0001美元）、免登录、自动扣费，完美适配AI高频微支付。

3 Sparkos (Spoon OS) → “AI+Web3的安卓开发系统” 把ERC8004、X402、私钥、支付通道打包成SDK，两天做出有身份、会付钱的AI。

4 三层协议架构 → 底层支付（X402）→ 中层身份（ERC8004）→ 顶层易用封装 完整解决AI“谁、怎么付钱、怎么好用”。

5 智能体编排引擎 + 支付通道/批量结算 → “项目经理 + 包月公交卡” 多AI协作、降低高频小额Gas成本。

四、其他高频对比概念

• 女巫攻击 → 多开小号刷分/刷差评 → ERC8004用Gas+真实交易权重防刷。

• KYC vs Web3钱包 → 传统要人脸身份证，AI过不了 → 链上地址即身份，无需KYC。

• 无状态 / 上下文窗口 → 早期AI“健忘症” → 向量DB彻底解决。

• 套利 → 跨平台/跨链价差赚钱 → AI实时监控+自动执行的经典场景。

• RPA vs Agentic AI → 固定剧本机器人 vs 能思考决策赚钱的智能体。

• SaaS订阅 vs X402按次付费 → 健身房年卡（浪费） vs 按次扣费（更适配AI忙闲不均）。

一句话总结2026年AI+Web3趋势： LLM + Agentic框架 + 向量记忆 + Function Call/ReAct → 会思考的AI + ERC8004（身份+信誉） + X402（原生微支付） + Sparkos（一站式开发） + DeFi（赚钱场景） → 能自主赚钱、有信誉、可大规模协作的链上AI经济体。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->









补充课程笔记

Day 1：课程导论 + 区块链高层次认知

核心主题：Web2 → Web3 心态转变 + 区块链本质

关键概念与金句

• 区块链 = 无法停止的全球巨型计算机（P2P 网络 + 共识规则，抗审查、抗单点故障）

• 参与门槛：只需私钥（无需许可、无 KYC）

• 智能合约 ≈ 自动售货机 / 托管机器人：在互不信任环境中执行“If A then B”

• 激励对齐：写规则 + 奖励，让任何人自发执行

• “代码即法律”：主网部署后任何人（包括自己）无法修改/停止

• 网络冗余：全节点存储全量合约与状态

• 主流应用：DeFi（LP 收益）、DAO（治理投票）、NFT（收藏 + Token Gated）、On-chain SVG NFT

• 缺点：贵、慢、异步、助记词丢失 = 永久丢失资金

• 未来潜力：协调机制可扩展到国家级别（博弈论 + 惩罚背叛）

用**代码 + 激励构建互不信任者的协作机器**，这就是区块链的超级力量。

Day 2：钱包设置与安全（超级用户入门）

核心操作流程（MetaMask 初始化）

1 [metamask.io](http://metamask.io) 下载官方扩展（警惕钓鱼！）

2 创建新钱包 → 设置本地密码（仅本地加密，可重置）

3 备份 12 词助记词（Secret Recovery Phrase）

◦ 手写纸质 / 密码管理器（中额资产） / 多签+Guardians（大额）

◦ 绝对禁止：截图、发聊天、存云端、GitHub

4 验证备份（按顺序点击单词）

5 切换测试网（当前推荐：Sepolia / Holesky），领测试 ETH（水龙头）

6 测试恢复：删钱包 → 用助记词恢复（必须亲测！）

7 用 Etherscan 验证交易（中立第三方，查 Tx Hash）

关键认知

• 助记词 → 私钥 → 地址（确定性派生，无限账户）

• 2²⁵⁶ 安全性 ≈ 不可能暴力破解

• 交易不可逆（打包后无客服），Mempool 内可 Speed Up / 取消（同 Nonce 高 Gas 覆盖）

**助记词 = 你的全部主权，安全存储 + 测试恢复，才算真正入门。**

Day 3：建立链上身份 + 基础交互（ENS + DEX + 资产可移植性）

核心主题 身份（Identity）与资产（Inventory）在 Web3 的可移植性，一切由智能合约驱动

主要操作

1 MetaMask 新账户（同一助记词无限派生）

2 接收测试 ETH → [app.ens.domains](http://app.ens.domains) 注册 ENS（Commit → 等待 → Reveal/Register）

◦ ENS = NFT（所有权可转移）

◦ 设置 Primary ENS Name（反向解析）

3 Uniswap 交换 ETH → DAI（AMM 流动性池）

4 [Zapper.fi](http://Zapper.fi) 输入 ENS 查看全资产（无需连接钱包）

关键对比

• DNS vs ENS：人类可读名字解析

• CEX（订单簿） vs DEX（AMM + 池）

• DAI（超额抵押） vs 算法稳定币（UST 死亡螺旋）

• Sybil Attack → Proof of Personhood / Gitcoin Passport

**ENS 建立持久身份 → 资产与身份在所有 dApp 间无缝跟随。**

Day 4：NFT 日（非同质化 + 存储 + 可组合性）

核心主题 NFT = 链上唯一所有权证明，存储方式决定信任与可组合性

主要体验

1 ENS 本身是 ERC-721 NFT

2 OpenSea 查看/浏览 NFT（身份跟随）

3 铸造示例：MetaAvatar（完全 On-chain SVG）

4 Etherscan 直接调用测试合约铸造（Rinkeby/Sepolia）

5 IPFS 元数据查看（内容寻址：哈希不变 → 防篡改）

6 完全链上 NFT 示例（Loogies）：tokenURI 返回 base64 SVG

关键认知

• NFT 本质：账本记录 ownerOf\[tokenId\] = address

• 内容寻址防篡改

• 可组合性：链上数据可被其他合约直接读用

• Royalties：合约强制二次销售分账艺术家

一句话总结 不同存储决定了 NFT 的永久性与创新边界，去垃圾堆里挖掘可组合性吧。

Day 5：超级用户进阶（失败、卡顿、高阶工具、L2、多签）

核心主题 真实使用的痛苦 + 解决办法 + 安全/效率工具

重要演示

• 主网 ↔ Optimism 桥接 + L2 低 Gas 体验

• Gnosis Safe 多签（2-of-4）：提案 → 多方链下签名 → 上链执行

• WalletConnect + Safe 可组合性（Safe 如普通钱包操作 Uniswap）

• DeFi 示例：Aave 超额抵押借贷（L2 更实用）

 **超级用户 = 经历大量失败 → 掌握 Gas/Nonce/L2/多签 → 把痛苦降到最低，继续构建。**

Week 1 整体收获一句话提炼 从安全钱包 + 身份（ENS） → 交互（DEX/NFT） → 高阶工具（L2/多签），经历真实痛点，成为能自如航行以太坊的超级用户，为 Week 2+ 的脚本/构建打下坚实基础。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->










# **网络结构与节点类型**

一、以太坊节点与客户端软件

节点（node） 与 客户端（client）

一个“完整的以太坊节点”：两个核心客户端组成：执行客户端（Execution Client，EL）以及共识客户端（Consensus Client，CL）

再挂了一个作为共识客户端的插件：验证者客户端（Validator）

1.主要负责链上的“业务逻辑和状态” ：执行客户端（Execution Client）

常见的执行客户端包括： Geth（go-ethereum） 、Nethermind、 Besu 、Erigon 、Reth

2\. 主要负责权益证明 PoS 共识：共识客户端（Consensus Client）

常见的共识客户端包括： Lighthouse 、Prysm、 Teku、 Nimbus、 Lodestar

3\. 验证者客户端（Validator）：在 PoS 模式下，想质押 ETH、参与出块并获取奖励就得再运行这个

二.节点内部如何构建

合并后的节点结构：

你的应用 / dApp / 脚本

-   使用 Web3 库（如 web3.js、ethers.js，或 Rust/Go/Java SDK）
    
-   通过 JSON-RPC 调用执行客户端的 eth\_\* 接口（读状态、发交易等）
    

以太坊节点（Ethereum Node）

-   里面跑着一套 执行客户端（EL）
    
-   再配一套 共识客户端（CL）
    
-   想当验证者的话，再加上 validator
    

P2P 网络（p2p）

-   执行客户端和共识客户端各自都有 P2P 网络，用来和其他节点同步区块、交易和共识消息。
    

tips：执行层仍然主要通过 JSON-RPC 提供 eth\_\* 接口； 更多使用 REST / HTTP / gRPC 接口，规范定义在共识层规范中。

三：Engine API —— 一个专门给 EL ↔ CL 用的 JSON-RPC 接口，合并后执行客户端和共识客户端两者之间必须通过一个标准化接口通信。

小结：以太坊节点 = 一台电脑 + 两个必备客户端 +（ 可选）一个验证者客户端：

1.  执行客户端负责“算交易、管状态、对外提供 RPC”；
    
2.  共识客户端负责“跑 PoS、选区块、投票达成共识”；
    
3.  它们通过 Engine API 的“专线电话”互相协作，整个节点再通过 P2P。
    

网络和其他节点连接起来，共同维护以太坊这条“世界计算机”。

二、节点间的连接与通信方式

1\. 节点发现（基于 UDP + Kademlia）：发现阶段 = 用 UDP + Kademlia 找到一堆靠谱的邻居，把自己的“好友列表”填满；

2\. 建立安全连接（TCP + RLPx / devp2p）：以太坊的 P2P 网络主要由 UDP 的 Node Discovery 协议 + TCP 上的 RLPx/devp2p 协议 组成；

3.传播消息（Gossip + 请求/响应）：① Gossip（广播式传播） ② 点对点请求-响应（拉取指定数据）

两者结合在一起，才能既保证新消息快速散播，又保证任何节点都能随时补齐历史、追上最新状态。

tips：1.Gossip 是节点之间传播关键信息的主力通道，主要承担： 执行层：传播交易、区块等数据； 共识层：传播 Beacon 区块、attestations（ 投票）、slashings 等 PoS 共识消息；

2.解决了什么问题：高可用 + 容错：节点挂了也不怕；可扩展到上万节点；尽量“快而不炸”：缓存 + TTL 过滤重复消息；支持共识与同步：不仅是交易，还有投票；

3.总结一句话：Gossip 协议相当于以太坊的“去中心化广播系统”： 它让每个节点只需和少数邻居聊天，就能在几秒内把新交易、新区块及 PoS 投票散播到整个网络，从而在没有中心服务器的前提下，实现高可用、高容错、可扩展的全网信息同步。

三、全节点、轻节点、归档节点的区别

1.  全节点 = 当前状态 + 全部区块（但老状态会被“裁剪”）；
    
2.  归档节点 = 全节点 + 从创世到现在的“所有历史状态”；
    

轻节点 = 只存区块头，需要时向全节点要具体数据。

| 类型 | 存储什么数据 | 资源消耗 | 典型用途 | 信任 / 安全属性 |
| --- | --- | --- | --- | --- |
| 全节点 | 全部区块 + 最近若干块的完整状态（老 状态会被修剪） | 中等（数百 GB 级磁盘 + 稳定带宽） | 自 托 管 RPC 节点、参与网络、做验证者的基础 | 自己验证所有交易和区块，最信任最抗审查 |
| 信任 / 安全属性 | 全节点数据 + 从创世到现在的所有历史状态快照 | 极高（TB 级磁盘） | 区块浏览器、链上分析、历史调试与回测、节点服务商 | 同全节点，但还能随时回答“任意高度的任意状态” |
| 轻节点 | 区块头 + 必要的轻客户端数据，不存完整区块和状态 | 很低（适合手机 / 嵌入式） | 轻钱包、浏览器插件、跨链桥验证、IoT 设备 | 通过状态根 + Merkle 证明验证数据，安全性接近全节点，但数据依赖全节点提供 |

四、开发者或机构运行全节点的必要性

引入:RPC（Remote Procedure Call）远程过程调用:

在区块链生态中间，区块链节点提供的一种接口，供应用程序（比如钱包、dApp、区块浏览器等）远程调用；节点（RPC 节点）或 RPC 服务商就提供这些接口和 endpoint。应用端只需要知道这个 endpoint 地址，以及支持的 RPC 方法，就能与区块链交互，而不需要每个应用都自己运行完整节点。

1.不泄露用户隐私：不用把地址 & IP 交给第三方；

2\. 不被卡脖子：抵御 RPC 层面的“软审查”；

3\. “不要信任，要验证”：自己验证每一笔交易和区块；

4\. 帮以太坊“撑腰”：去中心化和客户端多样性；

5\. 为 dApp 和内部系统提供稳定的“数据底座”；

6\. 归档节点支撑分析、审计和合规；

7.升级与分叉选择权：用脚“投票”的能力；

8\. 配置自由：客户端选型、参数调优、合规要求

总结：开发者和机构运行全节点，是为了拿回三件东西：

① 自己验证、自主管理的安全与隐私；

② 抗审查、抗单点故障的基础设施韧性；

③ 在以太坊协议和应用层上的话语权与自主权。 这既是为自己负责，也是为整个以太坊网络“增肌”。

五、归档节点在数据查询中的优势

在以太坊里，归档节点（Archive Node）= 全节点 + 所有历史状态快照：

-   和全节点一样，它保存所有区块和交易数据；
    
-   额外再保存每一个区块高度的完整状态（ 账户余额、合约存储、状态树等），从创世块一直到当前最新块。
    

类比：归档节点 = “国家档案馆 + 时间机器”；全节点只知道“现在是什么样”，归档节点还能瞬间回答“历史上任意一个时刻是什么样”。

六、合并后执行与共识客户端的区别

执行客户端管“算账 + 状态”，共识客户端管“选块 + 投票”。

七、执行与共识客户端的协同配合

1\. 一个节点 = 执行客户端 + 共识客户端 + Engine API；

2\. 当本地是出块者：CL 找 EL“要一块菜”；

3\. 当别人出块：CL 收块 → 让 EL 复算 → 再决定投不投票；

4\. 同步阶段

八、节点同步的目标与意义

节点要同步的内容主要包括两部分： ① 区块链数据（每个区块及其头信息） ② 状态数据（每个账户余额、合约存储、代码等）

九、节点间交换的数据类型

交易信息；区块数据；加密哈希；账户信息；公钥和私钥；智能合约代码和执行数据；网络状态；日志/事件

十、网络节点数量与去中心化保障机制

现在公开可见的以太坊节点数量大概在 1～2 万这个量级，活跃验证者接近 100 万个； 节点和验证者在客户端实现、地理位置、网络类型和经济主体上都高度分散，这就是它实际“去中心化”的底气。

十一、个人节点的搭建与开发环境配置

先选节点类型 → 选硬件&部署环境 → 装好执行/共识客户端 → 同步 → 开 RPC/ETL，用起来。

# **账户类型与结构**

补充：CREATE（ 传统创建）：公式是keccak256( RLP(\[s ender, nonce\]) ) 的后 20 字节；

CREATE2（EIP-1014 引入）：地址在部署前可以被精确预计算：keccak256(0xff ++ deployer ++ salt ++ keccak256(init\_code)) 的后 20 字节（0xff 是定界前缀）；

作 用 差 异 要点：CREATE2 的最大价值 是可 预 测 地 址 （deterministic deployment）与跨链/跨环境一致性。

一、EOA 的定义与控制方式

EOA = 由“私钥”直接控制的钱包账户，能发交易、收资金、调合约，但自己不带代码逻辑。控制私钥的人 = 控制这个账户的一切。

| 属 性 | 内 容 |
| --- | --- |
| 私钥与公钥 | EOA 是由一个密码学 key pair（ 私钥 + 公钥）控制的。私钥用于签名，公钥经过哈希等计算生成地址。 |
| 地址 | 公钥派生出地址。以太坊地址通常是 20字节 /40个十六进制字符 + "0x" 前缀。 |
| Nonce（交易计数器） | 跟踪这个 EOA曾经发送过多少交易，用来防止重放攻击。 |
| Balance（余额） | EOA 可以持有 ETH或代币余额。区块链状态里记录这个账户在世界状态对（World State）中的余额。 |
| Code / Storage | 对于 EOA，code部分是空的（没有智能合约代码）。也没有“storage root”（ 存储状态），这些是合约账户（Contract Account）才有的。 |

二、合约账户的概念与创建流程

合约账户 = “带代码的账户”，不靠私钥，而是靠部署在链上的 EVM 字节码决定它能做什么； 它是通过 “创建合约交易 + 部署字节码 + CREATE / CREATE2 地址规则” 被创建出来的。

三、以太坊地址“0x”开头的由来

以太坊地址本质是一串 20 字节（160 bit）的二进制数据，写成字符串时通常用十六进制展示，于是按照编程界传统加了个前缀 0x，用来标明：“注意，这是 hex 不是普通数字。”

四、EOA 与合约账户的控制方式对比

EOA =“由私钥直接控制的人类钱包”； 合约账户 =“由代码控制的自动售货机”。

| 区别维度 | EOA (Externally Owned Account，外部拥有账户） | 合约账户 （Contract Account / Smart Contract Account） |
| --- | --- | --- |
| 控制主体 | 私钥 / 助记词 持有者控制账户；谁拿到私钥，谁就能发交易。 | 由部署在账户里的代码控制行为；没有私钥，规则写死在智能合约逻辑中。 |
| 行为触发方式 | 可以主动发起交易：转账、调用合约，只要签名 + 有足够 gas。 | 不能主动发起外部交易；只能在被 EOA 或其他合约调用时执行（内部 message call）。 |
| 是否存在私钥 | 有私钥 + 公钥，对应地址；“控制权 = 私钥所有权”。 | 没有私钥；任何“权限控制”都由合约内部的 require(msg.sender == …)、角色管理、多签等代码实现。 |
| 功能 灵活性 | 自身功能很简单：签名 → 发交易；要实现复杂逻辑一般需要配合合约。 | 逻辑高度可编程：可实现多签、时间锁、访问控制、代币发行、治理、金库、机器人逻辑等。 |
| 风险与信任点 | 主要风险是私钥管理：丢失=永久失去控制；泄露=资产被直接转走。 | 主要风险在于代码漏洞 / 逻辑错误 / 权限设计不当；一旦部署，代码通常不可改，只能通过预先设计的升级机制（proxy等）调整。 |
| 创建成本与方式 | 生成 EOA 只是本地生成一对 key pair，不需要上链、不花 gas。 | 部署合约账户必须发一笔“创建合约交易”，把 bytecode 写入链上，需要支付不少 gas。 |
| 行为/权限的可修改性 | EOA 自身没有代码，行为模式基本固定（签名+发交易）；想要更复杂控制（多签等）通常借助合约钱包。 | 合约代码一旦部署一般不可变；只能通过合约内预留的升级逻辑、代理模式或 selfdestruct 等机制间接“修改”行为；storage 状态可以通过函数调用改变。 |

五、可主动发起交易的账户类型

能够主动发起交易的账户只有 EOA（外部拥有账户，Externally Owned Account），这是因为： EOA由私钥控制，用户通过签名发送交易请求并广播至网络，因此它们能够主动发起交易——包括转账、调用智能合约等行为。根据以太坊当前协议设计，每一笔链上 transaction 的起点，依然必须是一个由私钥签名的账户，也就是 EOA。 合约账户（Contract Account）由智能合约代码控制，没有私钥，无法主动创建交易。它们只能在收到 EOA发出的调用时被动执行逻辑，因此不能主动发起交易——这一点在今天依然成立：合约可以执行 call / delegatecall / create 等内部消息，但不能凭空生成新的顶层交易（top-level transaction）。

六、合约账户的余额与状态存储

以太坊中的每个账户（无论是 EOA 还是合约账户）在“世界状态”（ world state）里，都被建模为一个四元组：

σ\[a\] = (nonce, balance,0 storageRoot, codeHash);

从协议内部看，EOA 和合约账户在结构上是同一种账户对象，只是对 EOA 来说 codeHash / storageRoot 这两个字段是“空的 / 没意义”；你用“只有合约账户才有存储、才真正使用 storageRoot”来讲是完全可以的，只要在脚注提醒一下“规范里字段也挂在 EOA 上，但不真正使用”就够了。

七、EOA 与合约账户的互相调用机制;

1\. 从 EOA 发起调用到合约账户;

2\. 合约账户之间的调用（合约互调用链路）;

3\. 合约账户无法主动发起 EOA 级交易;

tips：以太坊合约账户交互的核心在于：用户（EOA）通过签名交易或“读”请求，将调用数据发送到合约地址，EVM 按合约字节码执行逻辑并更新存储或返回数据。整个过程可在不同层面完成，包括区块浏览器的“写合约”界面、钱包插件（如 MetaMask）的内置调用、使用前端库（Web3.js/Ethers.js）在网页或脚本中直接调用，以及借助 Truffle/Hardhat 等框架在控制台或脚本中管理合约。无论哪种方式，都需要合约地址与 ABI（ 或合约接口），并通过 JSON-RPC 与节点通信。

八、MetaMask 钱包对 EOA 的管理

1\. 私钥与助记词:MetaMask 不会保存你的 SRP，任何“客服”向你要助记词，都是诈骗;

2\. EOA 的生成与切换;

3\. 交易签名流程（MetaMask 如何用 EOA 发交易）;

4\. 支持智能账户拓展（Account Abstraction）:EIP-7702 把“EOA 能不能执行合约逻辑”这件事，放进了协议层，用一种可授权、可撤销、带 nonce 防重放的新交易类型来做；钱包（包括 MetaMask）可以在此基础上继续构建更上层的智能账户体验;

5\. 备份与安全防诈骗（与现在官方建议对齐）

小结 : MetaMask 这类钱包管理 EOA 的核心依然是：本地生成 / 加密保存

SRP → 本地派生私钥 / 地址 → 本地签名 → 通过 RPC 广播。

账户抽象（ERC-4337）+ EIP-7702 只是把“这个 EOA 能做什么”变得更灵活，而不是改变“私钥归谁所有”这件底层事实。

对用户来说，重点没变：记好助记词、管好私钥、看清每一次签名弹窗， 其余的“智能账户”“批量交易”“gas 赞助”“7702 高速通道”，都只是你之上的“外挂”和“外骨骼”。

九、ERC20 / ERC721 代币与合约账户的关系

1\. 代币就是智能合约存储的“记账系统” :ERC-20 / ERC-721 代币本质上都是某个合约账户里的“记账表 + 规则”;

2\. 代币操作就变成了合约调用:“转代币” 本质上是 “调用代币合约的一段代码 + 改一行存储”；

3\. 合约账户是逻辑执行与数据存储中心：本质仍然是“一个合约账户 + 一棵状态树”；

4\. 用户持有代币，是合约里那份“账”：在那个代币合约的 storage 里，有一条记录写着：这个地址的余额是多少 / 这个 tokenId 的 owner 是谁。

十、合约部署后的不可篡改性与销毁

合约代码默认仍然是“不可修改的”，但“删除”这件事在 Dencun 升级（2024-03-13）之后已经基本被废掉，只在极少数同交易场景下还算真正删除。日常开发应视为：合约一旦部署，就几乎不能改、也不能删，只能通过代理等模式“曲线升级”。

1\. 代码不可更改：合约部署后，字节码存储在指定地址，默认为不可变。 而且在 Dencun/EIP-6780 生效后更“硬核”；

2\. SELFDESTRUCT：可选的 SELFDESTRUCT：如果合约中实现了 SELFDESTRUCT（或 DELEGATECALL+SELFDESTRUCT 等组合），它可以在执行时销毁自身代码和状态，也可能将 ETH 转移到指定地址；

3\. 地址可重部署：自销毁后，通过 CREATE2 机制，有可能在相同地址重新部署新合约（metamorphic contract），但必须在初始部署时设计并允许这种行为；

4\. 代理合约模式（Proxy Pattern）：大多数“可升级合约”采用代理模式——主合约（proxy）永不变，只是可指向不同的实现合约地址，通过 delegatecall 调用新的逻辑，实现功能升级，同时保留原始地址和状态。

现在的“正确姿势”是：地址不变 → 代理合约；逻辑变更 → 换实现合约地址。不要再用 SELFDESTRUCT 做升级。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->











## **安全与合规**

一、合规不是形式，是底线

文章开头就讲了一个很现实的问题：在 Web3 这个看起来很 “去中心化、去规则” 的世界里，并不是说你做什么都没关系。事实上，如果触及到代币发行、交易这些具有金融属性的行为，中国法律已经明确设定了很多红线。比如：

• ICO、IEO、IDO 都属于禁止的融资方式；

• 虚拟货币在国内不被承认为支付工具；

• 场外交易可能被视为非法经营外汇或洗钱行为。

二、法律风险比你想象的更具体、更实际

手册里把风险分得很清楚——不仅是大公司的事，也和你每天接触的链游、DAO、空投等玩法有关，比如：

• 链游设计的经济模型可能被认定为赌博；

• 带返利或多层级推广的 NFT/DAO，很可能被指为传销；

• 场外交易常和洗钱、非法经营挂钩。

三、入职 Web3 要注意“非传统劳动关系”

Web3 的工作方式很特别：

• 可能是境外注册的公司；

• 面试和日常协作线上化；

• 很可能没有书面劳动合同、没有五险一金。

一方面这给了更多灵活性，特别适合远程工作者；另一方面，没有传统劳动保障，一旦发生争议，普通实习生或开发者真的很难保护自己的权益。所以入职前要自己评估风险，不要只看“高薪 + token 奖励”。

四、网络安全部分

后半部分（占了很大篇幅）讲的是常见网络攻击类型以及如何防范：

• 钓鱼攻击：假网站、假招聘、假空投网页；

• 恶意软件/木马：伪装成有用工具却能植入后门；

• 社交工程：冒充熟人/官方账号拉你上钩；

• 剪贴板劫持：复制钱包地址瞬间被替换。

文章里有些实用的建议：

✔️ 钱包助记词绝对不要截图和上传

✔️ 转账前核对地址前六后四

✔️ 安装软件/插件要核对官方来源

✔️ 使用 2FA 和密码管理器等安全工具。

五、整体感悟

文章最后的一句话挺打动我——这个行业虽然有活力，但同样存在太多作恶方式。你技术再牛、商业模式再创新，如果没有基本的安全和合规意识，一切都可能毁于细节。很多人以为只要合约没 bug 就安全了，殊不知很多损失不是因为合约漏洞，而是因为助记词泄露/钓鱼网站被骗/权限没收回这些更“低级”的问题。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->














**认识以太坊（What is Ethereum）**

-   **一：平台定位**：Ethereum是一个去中心化、开源并且具备智能合约功能的公共区块链平台。通过其原生加密货币以太币（Ether，简称 ETH）提供去中心化的以太虚拟机（EVM）来处理点对点合约。由V神提出，目标是构建“下一代加密货币与去中心化应用平台”是“区块链 2.0”的代表，目标是成为全球范围内的“世界计算机” ，为开发者提供构建复杂应用的工具和环境；
    

tips：以太币（ETH）是以太坊的原生代币。

-   **二：核心特性**：1.智能合约（Smart Contracts），存储在区块链上的可执行代码，能够在满足预设条件时自动执行操作，无需人工干预；
    

2.分布式应用程序（dApps），以太坊上的分布式应用程序（dApp）部署在区块链上，不依赖单一服务器，也没有传统意义上的“ 后台可关机”；

3.代币（Tokens），智能合约可以创造代币供分布式应用程序使用，有可替代的（ERC-20）和不可替代的（ERC-721 / ERC- 1155）；

4.权益证明（PoS）与早期的工作量证明（PoW），以太坊完全改用权益证明（Proof-of-Stake, PoS），淘汰了挖矿机制，降低能耗，PoW 用电力和硬件“押注诚实”，PoS 用锁定的 ETH 和削减机制“押注诚实”；

5.燃料费（Gas），Gas 费由“基础费”（base fee）+小费（priority fee）”组成，其中基础费会被销毁，小费奖励给出块验证者；

6.Proto-Danksharding（EIP-4844）与 Dencun 升级，Proto-Danksharding（ 原型丹克分片，EIP-4844）已在Dencun 升级 中正式上线主网，作为以太坊迈向完整 Danksharding 的中间升级，为 Rollup 提供临时数据空间，极大减轻 L1 的存储压力；

7.分片与 Danksharding，以太坊最新设计转向 Danksharding，Proto-Danksharding 先给以太坊挂上“小挂车”，而完整 Danksharding 则是把挂车扩展成“高速货运列车”，专门为 L2 输送数据；

8.叔块（Uncle / Ommer Block），叔块在转为权益证明后已停用；

-   **三：Ether（ETH）的定义与系统职能：**
    

作为以太坊上区块链上的原生数字资产，ETH 的设计初衷更偏向“实用 +资产”；

ETH 三大核心作用：1. 在协议层面最根本、最核心的作用：网络燃料（Gas）：支付交易和计算费用；

2.参与网络共识与安全（Staking）；

3.价值储存和交换媒介；

-   **四：以太坊 “全球可编程区块链”** ：全球性（Global）、可编程（Programmable） 和 区块链（Blockchain）；
    

1\. 区块链（Blockchain）：继承了区块链技术的所有基本属性；

2.可编程（Programmable）：引入了更强的智能合约 + 图灵完备虚拟机（EVM）；

3.全球性（Global）：以太坊维护的是一个全球共享的状态机。

tips：智能合约：是一段部署在区块链上的代码。

图灵完备（Turing-complete）：本质是 只要给足时间和资源，就能表达几乎任何可计算的逻辑，以太坊的智能合约语言（Solidity、Vyper 等）在 EVM 上运行，开发者可以在以太坊上构建几乎无限种类的去中心化应用（dApps）。

-   **五：以太坊与比特币的异同对比**（见昨天笔记）
    
-   **六：以太坊 dApps 的概念与应用**：
    

1.  去中心化应用程序（dApp）是链上的智能合约（后端逻辑）和链下的前端页面 / 客户端（用户界面）拼接在一起，构建在去中心化网络上的应用
    
2.  dApp 的几个核心技术特征：去中心化、 确定性、图灵完备、隔离性（沙盒）
    
3.  dApp 开发的好处：① 零停机时间；② 相对传统 Web具有隐私 / 匿名性；③ 抗审查；④ 数据完整性；⑤ 无需信任 / 可验证行为
    
4.  dApp的应用（覆盖场景）：金融（DeFi）；资产与内容（NFT & 游戏 / 内容）；组织与治理（DAO）；基础设施类 dApp；其他的：去中心化社交、链上存证、供应链追踪、链上订阅与会员系统等。
    

理解dApp：**“用智能合约做后端，用以太坊做数据库和结算层”的应用程序。**

5.dApp 开发的缺点 / 代价：维护困难；性能开销与扩展难度‘；网络拥堵与 Gas 成本；用户体验门槛高；“再中心化”倾向。

-   **七：以太坊的去中心化实现机制：**
    

1\. 共识与出块（技术 / 经济层）：全面采用 PoS 共识；区块通过 Gasper 共识（Casper-FFG 提供终局性 + LMDGHOST

提供链头选择）完成安全性与活性；验证者作恶会被削减（slashing），大规模长期离线，还会触发 inactivity leak；

2\. 节点与网络（技术层）：以太坊执行层使用 DevP2P 协议维护点对点网络：发现层（Discovery Stack） 基于 UDP，用来发现和加入新节点；DevP2P 通信层 基于 TCP，负责在已连接节点间传递区块、交易等实际数据。

3\. 客户端多样性（实现层）：“多客户端（multi-client）”：执行层客户端（EL）和共识层客户端（CL）；

4\. MEV 与 PBS（市场结构层）：引入了 ProposerBuilderSeparation（PBS，提议者-建构者分离） 的思路；

5\. 「Rollup-中心」路线与数据去中心化（扩容 / 职责分离）：提出了「Rollup-centric Ethereum Roadmap」：L1 主要做高度去中心化的 结算层 + 数据可用性层，把大规模执行吞吐交给各种 L2 Rollup；

6\. 治理（社会层）：没有「1 票 1 COIN」式的链上代币治理；

7\. 多个机制叠加出来的「去中心化」。

tips：什么是 P2P 网络：简单把它想象成一群“好友”节点互相连线聊天、转发消息，而不是所有人都连到一个中心服务器。

-   **八：网络结构的开放性与参与机制：**
    

1.  节点越来越多带来的问题
    
2.  共识效率
    
3.  以太坊需要把不同的工作分层做
    

-   **九：行业应用案例：金融、游戏与社交：**
    

金融（DeFi / 稳定币 / RWA）、NFT & 游戏、社交 & 内容 & DAO。 很多应用已经迁到各类 L2，但结算和安全大多仍依托以太坊生态。

1.  去中心化金融（DeFi、稳定币、RWA）：是以太坊最成熟、体量最大的应用方向之一；
    
2.  游戏、虚拟世界与 NFT；
    
3.  社交、内容创作与 DAO / 公共物品：基本都是以“协议 + L2”的形态。
    

-   **十：生态系统创新：DeFi、NFT 与 DAO：**
    

1\. 去中心化金融（DeFi）：重塑全球金融“操作系统”；

2\. 非同质化代币（NFT）：从“所有权证明”到“链上身份与权限系统”；

3\. 去中心化自治组织（DAO）：把“组织结构图”写进代码。

-   **十一：社区与开发生态扮演的角色：**
    

1\. 社区之中

1.1 去中心化治理与共识形成；

1.2 多元化：谁都能参与、谁都能发声；

1.3 文化与价值观：开源、透明、“无限花园”；

2\. 开发生态系统

2.1 工具与基础设施：把开发门槛打下来；

2.2 教育与培训：把新人源源不断“拉进来”；

2.3 生态扩展与协议创新：把想法变成升级和产品。

-   **十二：总结：**
    

以太坊之所以能在十多年里不断演化，不只是因为有一套聪明的协议设计，更因为它有一个开放、多元、有价值观约束的社区，和一整套 自下而上的开发者生态：社区负责方向和共识，开发者负责把这些共识写成代码，基金会和各类组织则在中间“管理而非控制”，尽量让这座“无限花园”自己长出来。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->















1\. 区块链到底是什么

区块链是一种去中心化的分布式账本技术，用于在网络节点之间安全、透明且不可篡改地记录事务数据。每条链由一系列按照时间顺序相连的“区块”组成，每个区块内部包含了多笔交易数据及元数据，确保了数据记录的完整性与可追溯性。

个人理解：区块链 = 全世界很多人一起记一本永远改不了的公开账本，用密码学+经济奖励保证大家不敢/不能作弊。

4个最核心特性：

1.  去中心化 → 没有“老板服务器”
    
2.  不可篡改 → 历史几乎永久（改动代价极高）
    
3.  公开透明、匿名 → 所有交易都看得见，但可以匿名
    
4.  快速交易 →无论金额和地方，只要你的交易记录被打包在区块链中，交易就自动完成。相比传统的跨国汇款非常快速便捷。
    

2\. 三种区块链对比

| 项目 | 公链（比特币/以太坊） | 联盟链 | 私链 |
| --- | --- | --- | --- |
| 谁能加入 | 任何人 | 得被邀请/审核 | 老板/公司说了算 |
| 数据谁能看 | 全世界都能看 | 只有联盟成员 | 只有公司内部 |
| 去中心化程度 | ★★★★★ | ★★★ | ★ |
| 速度 | 比较慢 | 快 | 最快 |
| 成本 | 贵（Gas费） | 便宜 | 几乎免费 |
| 典型代表 | BTC、ETH、Solana、TON | Hyperledger Fabric | 企业内部测试链 |
| 主要用途 | 加密货币、DeFi、NFT、DAO | 企业间协作（供应链、金融） | 公司内部数据存证 |

我们平时说的“玩Web3、炒币、撸空投、发NFT” → 几乎100%都在公链上，尤其是以太坊生态（目前最成熟、项目最多）。

3\. 比特币 vs 以太坊

| 维度 | 比特币 (BTC) | 以太坊 (ETH) |
| --- | --- | --- |
| 出生时间 | 2009 | 2015 |
| 主要功能 | 数字黄金 + 价值储存 | 数字黄金 + 可编程世界计算机 |
| 是否支持智能合约 | 不支持 | 支持（这就是最大区别！） |
| 目前共识机制 | 曾经PoW，现在仍在PoW | 已转为PoS（权益证明，省电） |
| 最大特点 | 最安全、最去中心化 | 生态最丰富（DeFi、NFT、GameFi、DAO…） |
| 小白比喻 | 数字保险柜/黄金 | 可以编程的全球超级大电脑 |

比特币是证明了“去中心化数字货币”可行  
以太坊是把区块链从“只能转账”升级成“能跑程序”

4\. Web2 vs Web3.0 Vs Web3

| 维度 | Web2（现在互联网） | Web3.0（语义网） | Web 3（去中心化互联网） |
| --- | --- | --- | --- |
| 控制权 | 平台（腾讯、字节、Meta…） | 部分开放 | 用户自己（私钥→资产） |
| 数据存储 | 中心服务器 | 混合存储 | 区块链 / IPFS |
| 支付系统 | 信用卡 / 支付宝 | 集成支付 | 加密货币 |
| 典型技术 | JavaScript | RDF / OWL | 智能合约 |
| 代表产品 | 微信、抖音、淘宝、小红书 | Uniswap、OpenSea、ENS、Aave、Lens | Uniswap / ConsenSys |

技术栈对比：web2开发： React + Node.js + MySQL

web3开发：React + Ethers.js + Solidity + IPFS

Web 3.0 开发：Python + RDFLib + SPARQL

五、去中心化的优势与挑战

1\. 优势

信任最小化；抗审查与高弹性；用户自主管理；开放创新生态

2.挑战

可扩展性瓶颈；安全与治理难题；用户体验与成本；法律与合规风险
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
