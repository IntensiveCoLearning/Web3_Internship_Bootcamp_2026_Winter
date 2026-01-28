---
timezone: UTC+8
---

# tokyoexplorer

**GitHub ID:** tokyoexplorer

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->
### 变量初始值

-   `boolean`: `false`
    
-   `string`: `""`
    
-   `int`: `0`
    
-   `uint`: `0`
    
-   `enum`: 枚举中的第一个元素
    
-   `address`: `0x0000000000000000000000000000000000000000` (或 `address(0)`)
    
-   `function`
    
    -   `internal`: 空白函数
        
    -   `external`: 空白函数
        
-   映射`mapping`: 所有元素都为其默认值的`mapping`
    
-   结构体`struct`: 所有成员设为其默认值的结构体
    
-   数组`array`
    
    -   动态数组: `[]`
        
    -   静态数组（定长）: 所有成员设为其默认值的静态数组
        

### `delete`**操作符**

`delete a`会让变量`a`的值变为初始值。

### 常数

Solidity中和常量相关的两个关键字，`constant`（常量）和`immutable`（不变量）。状态变量声明这两个关键字之后，不能在初始化后更改数值。这样做的好处是提升合约的安全性并节省`gas`。

另外，只有**数值变量**可以声明`constant`和`immutable`；`string`和`bytes`可以声明为`constant`，但不能为`immutable`。

### **constant**

`constant`变量必须在声明的时候初始化，之后再也不能改变。尝试改变的话，编译不通过。

```
// constant变量必须在声明的时候初始化，之后不能改变
uint256 constant CONSTANT_NUM = 10;
string constant CONSTANT_STRING = "0xAA";
bytes constant CONSTANT_BYTES = "WTF";
address constant CONSTANT_ADDRESS = 0x0000000000000000000000000000000000000000;
```

### **immutable**

`immutable`变量可以在声明时或构造函数中初始化，因此更加灵活。在`Solidity v0.8.21`以后，`immutable`变量不需要显式初始化，未显式初始化的`immutable`变量将使用数值类型的初始值（见 [8\. 变量初始值](https://github.com/AmazingAng/WTF-Solidity/blob/main/08_InitialValue/readme.md#%E5%8F%98%E9%87%8F%E5%88%9D%E5%A7%8B%E5%80%BC)）。反之，则需要显式初始化。 若`immutable`变量既在声明时初始化，又在constructor中初始化，会使用constructor初始化的值。

```
// immutable变量可以在constructor里初始化，之后不能改变
uint256 public immutable IMMUTABLE_NUM = 9999999999;
// 在`Solidity v8.0.21`以后,下列变量数值暂为初始值
address public immutable IMMUTABLE_ADDRESS; 
uint256 public immutable IMMUTABLE_BLOCK;
uint256 public immutable IMMUTABLE_TEST;
```

你可以使用全局变量例如`address(this)`，`block.number` 或者自定义的函数给`immutable`变量初始化。在下面这个例子，我们利用了`test()`函数给`IMMUTABLE_TEST`初始化为`9`：

```
// 利用constructor初始化immutable变量，因此可以利用
constructor(){
    IMMUTABLE_ADDRESS = address(this);
    IMMUTABLE_NUM = 1118;
    IMMUTABLE_TEST = test();
}

function test() public pure returns(uint256){
    uint256 what = 9;
    return(what);
}
```

### **控制流**

-   **if-else**
    

```
function ifElseTest(uint256 _number) public pure returns(bool){
    if(_number == 0){
        return(true);
    }else{
        return(false);
    }
}
```

-   **for循环**
    

```
function forLoopTest() public pure returns(uint256){
    uint sum = 0;
    for(uint i = 0; i < 10; i++){
        sum += i;
    }
    return(sum);
}
```

-   **while循环**
    

```
function whileTest() public pure returns(uint256){
    uint sum = 0;
    uint i = 0;
    while(i < 10){
        sum += i;
        i++;
    }
    return(sum);
}
```

-   **do-while循环**
    

```
function doWhileTest() public pure returns(uint256){
    uint sum = 0;
    uint i = 0;
    do{
        sum += i;
        i++;
    }while(i < 10);
    return(sum);
}
```

-   **三元运算符**
    

三元运算符是`Solidity`中唯一一个接受三个操作数的运算符，规则`条件? 条件为真的表达式:条件为假的表达式`。此运算符经常用作`if`语句的快捷方式。

```
// 三元运算符 ternary/conditional operator
function ternaryTest(uint256 x, uint256 y) public pure returns(uint256){
    // return the max of x and y
    return x >= y ? x: y; 
}
```

另外还有`continue`（立即进入下一个循环）和`break`（跳出当前循环）关键字可以使用。
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->

### array、struct

-   固定长度数组：在声明时指定数组的长度。用`T[k]`的格式声明，其中`T`是元素的类型，`k`是长度
    
-   可变长度数组（动态数组）：在声明时不指定数组的长度。用`T[]`的格式声明，其中`T`是元素的类型，例如
    

```
// 固定长度 Array
uint[8] array1;
bytes1[5] array2;
address[100] array3;

// 可变长度 Array
uint[] array4;
bytes1[] array5;
address[] array6;
bytes array7;
```

-   对于`memory`修饰的`动态数组`，可以用`new`操作符来创建，但是必须声明长度，并且声明后长度不能改变。例子：
    

```
// memory动态数组
uint[] memory array8 = new uint[](5);
bytes memory array9 = new bytes(9);
```

-   如果创建的是动态数组，你需要一个一个元素的赋值。
    

```
uint[] memory x = new uint[](3);
x[0] = 1;
x[1] = 3;
x[2] = 4;
```

-   `length`: 数组有一个包含元素数量的`length`成员，`memory`数组的长度在创建后是固定的。
    
-   `push()`: `动态数组`拥有`push()`成员，可以在数组最后添加一个`0`元素，并返回该元素的引用。
    
-   `push(x)`: `动态数组`拥有`push(x)`成员，可以在数组最后添加一个`x`元素。
    
-   `pop()`: `动态数组`拥有`pop()`成员，可以移除数组最后一个元素。
    

```
// 结构体
struct Student{
    uint256 id;
    uint256 score; 
}

Student student; // 初始一个student结构体

//  给结构体赋值
// 方法1:在函数中创建一个storage的struct引用
function initStudent1() external{
    Student storage _student = student; // assign a copy of student
    _student.id = 11;
    _student.score = 100;
}
// 方法2:直接引用状态变量的struct
function initStudent2() external{
    student.id = 1;
    student.score = 80;
}
// 方法3:构造函数式
function initStudent3() external {
    student = Student(3, 90);
}
// 方法4:key value
function initStudent4() external {
    student = Student({id: 4, score: 60});
}
```

### 映射类型：mapping

声明映射的格式为`mapping(_KeyType => _ValueType)`，其中`_KeyType`和`_ValueType`分别是`Key`和`Value`的变量类型。例子：

```
mapping(uint => address) public idToAddress; // id映射到地址
mapping(address => address) public swapPair; // 币对的映射，地址到地址
```

**映射的规则**

-   **规则1**：映射的`_KeyType`只能选择Solidity内置的值类型，比如`uint`，`address`等，不能用自定义的结构体。而`_ValueType`可以使用自定义的类型。下面这个例子会报错，因为`_KeyType`使用了我们自定义的结构体：
    

```
// 我们定义一个结构体 Struct
struct Student{
    uint256 id;
    uint256 score; 
}
mapping(Student => uint) public testVar;
```

-   **规则2**：映射的存储位置必须是`storage`，因此可以用于合约的状态变量，函数中的`storage`变量和library函数的参数。不能用于`public`函数的参数或返回结果中，因为`mapping`记录的是一种关系 (key - value pair)。
    
-   **规则3**：如果映射声明为`public`，那么Solidity会自动给你创建一个`getter`函数，可以通过`Key`来查询对应的`Value`。
    
-   **规则4**：给映射新增的键值对的语法为`_Var[_Key] = _Value`，其中`_Var`是映射变量名，`_Key`和`_Value`对应新增的键值对。例子：
    

```
function writeMap (uint _Key, address _Value) public{
    idToAddress[_Key] = _Value;
}
```

**映射的原理**

-   **原理1**: 映射不储存任何键（`Key`）的资讯，也没有length的资讯。
    
-   **原理2**: 对于映射使用`keccak256(h(key) . slot)`计算存取value的位置。感兴趣的可以去阅读 [WTF Solidity 内部规则: 映射存储布局](https://github.com/WTFAcademy/WTF-Solidity-Internals/tree/master/tutorials/02_MappingStorage)
    
-   **原理3**: 因为Ethereum会定义所有未使用的空间为0，所以未赋值（`Value`）的键（`Key`）初始值都是各个type的默认值，如uint的默认值是0。
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->


今日在WTF上重新学习solidity，这里的教程比 [Solidity by Example](https://solidity-by-example.org/) 更好懂一点，目前完成了solidity101前五关。
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->



**Transient Storage:** Data stored in transient storage is cleared out after transaction.

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// Make sure EVM version and VM set to Cancun

// Storage - data is stored on the blockchain
// Memory - data is cleared out after a function call
// Transient storage - data is cleared out after a transaction

interface ITest {
    function val() external view returns (uint256);
    function test() external;
}

// Contract for testing TestStorage and TestTransientStorage
// Shows the difference between normal storage and transient storage
contract Callback {
    uint256 public val;

    fallback() external {
        val = ITest(msg.sender).val();
    }

    function test(address target) external {
        ITest(target).test();
    }
}

contract TestStorage {
    uint256 public val;

    function test() public {
        val = 123;
        bytes memory b = "";
        msg.sender.call(b);
    }
}

contract TestTransientStorage {
    bytes32 constant SLOT = 0;

    function test() public {
        assembly {
            tstore(SLOT, 321)
        }
        bytes memory b = "";
        msg.sender.call(b);
    }

    function val() public view returns (uint256 v) {
        assembly {
            v := tload(SLOT)
        }
    }
}

// Contract for testing reentrancy protection
contract MaliciousCallback {
    uint256 public count = 0;

    // Try to reenter the target contract multiple times
    fallback() external {
        ITest(msg.sender).test();
    }

    // Test function to initiate reentrance attack
    function attack(address _target) external {
        // First call to test()
        ITest(_target).test();
    }
}

contract ReentrancyGuard {
    bool private locked;

    modifier lock() {
        require(!locked);
        locked = true;
        _;
        locked = false;
    }

    // 27587 gas
    function test() public lock {
        // Ignore call error
        bytes memory b = "";
        msg.sender.call(b);
    }
}

contract ReentrancyGuardTransient {
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

    // 4909 gas
    function test() external lock {
        // Ignore call error
        bytes memory b = "";
        msg.sender.call(b);
    }
}
```
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->




structs

You can define your own type by creating a .`struct`

They are useful for grouping together related data.

Structs can be declared outside of a contract and imported in another contract.

```
contract Todos {
    struct Todo {
        string text;
        bool completed;
    }

    // An array of 'Todo' structs
    Todo[] public todos;

    function create(string calldata _text) public {
        // 3 ways to initialize a struct
        // - calling it like a function
        //todos.push(Todo(_text, false));

        // key value mapping
        todos.push(Todo({text: _text, completed: false}));

        // initialize an empty struct and then update it
        // Todo memory todo;
        // todo.text = _text;
        // todo.completed initialized to false
        // todos.push(todo);
    }

    // Solidity automatically creates a getter for 'todos' so
    // you don't actually need this function.
    function get(uint256 _index)
        public
        view
        returns (string memory text, bool completed)
    {
        Todo storage todo = todos[_index];
        return (todo.text, todo.completed);
    }

    // update text
    function updateText(uint256 _index, string calldata _text) public {
        Todo storage todo = todos[_index];
        todo.text = _text;
    }

    // update completed
    function toggleCompleted(uint256 _index) public {
        Todo storage todo = todos[_index];
        todo.completed = !todo.completed;
    }
}
```
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->





**mapping**：mapping(keyType => valueType)

The **keyType** can be any built-in value type, bytes, string, or any contract. **ValueType** can be any type including another mapping or an array.

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Mapping {
    // Mapping from address to uint
    mapping(address => uint256) public myMap;

    function get(address _addr) public view returns (uint256) {
        // Mapping always returns a value.
        // If the value was never set, it will return the default value.
        return myMap[_addr];
    }

    function set(address _addr, uint256 _i) public {
        // Update the value at this address
        myMap[_addr] = _i;
    }

    function remove(address _addr) public {
        // Reset the value to the default value.
        delete myMap[_addr];
    }
}
```

**Array:** An array can have a compile-time fixed size or a dynamic size.

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Array {
    // Several ways to initialize an array
    uint256[] public arr;
    uint256[] public arr2 = [1, 2, 3];
    // Fixed sized array, all elements initialize to 0
    uint256[10] public myFixedSizeArr;

    function setArr(uint256[] calldata _arr) external {
    // 逐个写入 storage
    for (uint256 i = 0; i < _arr.length; i++) {
        arr.push(_arr[i]);
        }
    }

    function get(uint256 i) public view returns (uint256) {
        return arr[i];
    }

    // Solidity can return the entire array.
    // But this function should be avoided for
    // arrays that can grow indefinitely in length.
    function getArr() public view returns (uint256[] memory) {
        return arr;
    }

    function push(uint256 i) public {
        // Append to array
        // This will increase the array length by 1.
        arr.push(i);
    }

    function pop() public {
        // Remove last element from array
        // This will decrease the array length by 1
        arr.pop();
    }

    function getLength() public view returns (uint256) {
        return arr.length;
    }

    function remove(uint256 index) public {
        // Delete does not change the array length.
        // It resets the value at index to it's default value,
        // in this case 0
        delete arr[index];
    }

    function examples() external pure {
        // create array in memory, only fixed size can be created
        uint256[] memory a = new uint256[](5);

        // create a nested array in memory
        // b = [[1, 2, 3], [4, 5, 6]]
        uint256[][] memory b = new uint256[][](2);
        for (uint256 i = 0; i < b.length; i++) {
            b[i] = new uint256[](3);
        }
        b[0][0] = 1;
        b[0][1] = 2;
        b[0][2] = 3;
        b[1][0] = 4;
        b[1][1] = 5;
        b[1][2] = 6;
    }
}

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
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->






使用view读取状态变量无需花费gas fee

交易使用ether付费，一ether等于10^18wei

1.  Gas
    
    ### How much `ether` do you need to pay for a transaction?
    
    You pay `gas spent * gas price` amount of `ether`, where
    
    -   `gas` is a unit of computation
        
    -   `gas spent` is the total amount of `gas` used in a transaction
        
    -   `gas price` is how much `ether` you are willing to pay per `gas`
        
2.  Solidity supports conditional statements `if`, `else if` and `else`.
    

```
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
        // if (_x < 10) {
        //     return 1;
        // }
        // return 2;

        // shorthand way to write if / else statement
        // the "?" operator is called the ternary operator
        return _x < 10 ? 1 : 2;
    }
}
```

1.  `for`, `while`, and `do while`
    

```
contract Loop {
    function loop() public pure {
        // for loop
        for (uint256 i = 0; i < 10; i++) {
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
        uint256 j;
        while (j < 10) {
            j++;
        }
    }
}
```

1.  mapping
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->







### Solidity 基础语法

1.  原始数据类型
    
    -   boolean：布尔值
        
    -   uint256：无符号整型
        
    -   int256：有符号整型
        
    -   address：地址
        
    -   bytes32：定长字节
        
2.  引用类型
    
    -   string：字符串
        
    -   bytes：动态字节
        
    -   uint\[\]：数组
        
    -   mapping：映射
        
    -   struct：结构体
        
3.  Variables
    
    -   **local**
        
        -   declared inside a function
            
        -   not stored on the blockchain
            
    -   **state**
        
        -   declared outside a function
            
        -   stored on the blockchain
            
    -   **global** (provides information about the blockchain)
        
4.  Constants: variables that cannot be modified.
    
5.  Immutable: Values of immutable variables can be set inside the constructor but cannot be modified afterwards.
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->








今天按照draken老师的教程一步步进行了remix的设置开发环境和熟悉~

![daf12249-a599-4b10-9e74-d4633af2c037.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/tokyoexplorer/images/2026-01-19-1768814261433-daf12249-a599-4b10-9e74-d4633af2c037.png)
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->









\# 2026-01-16

<!-- DAILY\_CHECKIN\_2026-01-16\_START -->

**L1**\-底层公链(比特币/以太坊等基础区块链)

**L2**\-二层扩容方案(在L1上构建的高性能扩展层)

**_ZK_**零知识证明(证明你知道秘密但不透露秘密的技术)0P-乐观验证(先假设交易有效，有争议再验证的机制)

\## **DeFi领域**

**TVL**\-总锁定价值(协议中锁定的加密资产总价值)

**APY** - 年化收益率(考虑复利后的年度回报率)

**IL**\-无常损失(提供流动性时因价格波动造成的暂时损失)

**MEV**\-矿工可提取价值(验证者通过排序交易获取的额外收益)

\## **治理与DA0**

**DA0**\- 去中心化自治组织(代码即章程的区块链组织)\*\*POAP\*\*- 出席证明协议(参与活动的数字纪念徽章)\*\*SBT\*\* - 灵魂绑定代币(不可转让的身份/信誉凭证)

\## **交易与市场**

**MM** -市场做市商(提供流动性的专业交易者)

**0TC** - 场外交易(交易所外的私下大额交易)

**FOM0** - 错失恐惧症(害怕错过行情而追高买入)

**DY0R** -自己做好研究(投资前务必独立调研的忠告)

\## **技术概念**

**EVM** -以太坊虚拟机(执行智能合约的运行环境)\*\*IPFS\*\* -星际文件系统(去中心化的存储协议)

**ZK-SNARK** -简洁非交互式零知识证明

**ZK-STARK** -透明零知识证明(无需可信设置的ZK技术)

\## **网络状态**

**P2E** -边玩边赚(游戏与金融结合的模式)

**IYKYK** -懂的都懂(圈内人心照不宣的默契)

**NGMI** -注定失败(对糟糕决策的讽刺性评价)

**WAGMI** -我们都会成功(社区互相鼓励的口号)

\# **新兴概念**

**L3** -应用专用链(为特定应用定制的三层架构)\*\*AA\*\* -账户抽象(让钱包像智能合约一样可编程)

**LSDFi** -LSD衍生金融(基于流动性质押代币的DeFi生态)

**_RWA_**\-现实世界资产(传统资产上链的代币化)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->










### 第二章 以太坊网络结构与节点类型

一、以太坊节点与客户端软件

节点(node):任何一台运行以太坊客户端软件并连接到其他节点的电脑, 就是一个以太坊节点。

客户端(client):是对以太坊协议的具体实现,会按协议规则验证数据、同 步区块和状态,帮助网络保持安全。

-   执行客户端(Execution Client,EL)
    
-   共识客户端(Consensus Client,CL)
    
-   验证者客户端(Validator)
    

1.  执行客户端
    
    -   监听网络上广播来的交易与区块
        
    -   在 EVM(以太坊虚拟机) 中执行交易和智能合约
        
    -   维护当前链的状态数据库(账户余额、合约存储等)
        
    -   提供 JSON-RPC 接口(eth\_\*),给 dApp / 钱包 / 脚本调用
        
    -   通过自己的 P2P 网络转发交易和区块头 / 区块体
        
    -   常见的执行客户端：• Geth(go-ethereum) • Nethermind • Besu • Erigon • Reth
        
2.  共识客户端：主要负责**权益证明 PoS 共识**
    
    -   维护信标链(Beacon Chain)视图和 fork choice 规则(LMD-GHOST 等)
        
    -   在每个 slot 里选择谁来提议区块,并对区块进行投票(attestation)
        
    -   跟踪验证者集合、质押余额、惩罚(slashing)等共识状态
        
    -   把来自执行客户端的结果当作“材料”,决定哪一个区块被接受到链上
        
3.  验证者客户端(Validator)
    
    -   管理你的验证者密钥
        
    -   在轮到你时提议新区块
        
    -   在每个 slot 为看到的区块做 attestation(投票)
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->











### 以太坊的特点

**1\. 智能合约(Smart Contracts)** 智能合约是存储在区块链上的程序，由网络节点执行。现在以太坊已从早期的“矿工(PoW)”时代完全过渡到“验证者(PoS)”时代,这些验证者负责打包并执行合约。

**2\. 分布式应用程序(dApps)** 以太坊上的分布式应用程序(dApp)部署在区块链上,不依赖单一服务器, 也没有传统意义上的 “后台可关机”。只要以太坊网络和客户端软件还活着,dApp 就很难被单一主体关停。 比喻: 分布式应用就像一间“永不打烊的 24 小时自动商店”。 它运行在无数台电脑上,就算某些电脑坏了,其它电脑依然可以继续“营业”;没有哪一个人能按下总电源开关让整个应用消失。

**3\. 代币(Tokens)** 智能合约可以创造代币供分布式应用程序使用,这些代币可以是:

-   可替代的(ERC-20):例如游戏里的“金币”、治理代币、稳定币等;
    
-   不可替代的(ERC-721 / ERC-1155):对应独特资产,如艺术品、门票或 游戏角色。
    

**4\. 权益证明(PoS)与早期的工作量证明(PoW)**

**5\. 燃料费(Gas)** “燃料”(Gas)是对计算与存储资源的计量单位,发送 ETH、转账代币、 调用合约函数、部署合约,统统会消耗 Gas。EIP-1559 之后,Gas 费由“基础费 (base fee)+小费(priority fee)”组成,其中基础费会被销毁,小费奖励给出块验证者。

**6\. Proto-Danksharding(EIP-4844)与 Dencun 升级** Proto-Danksharding(原型丹克分片,EIP-4844)已在 2024 年 3 月 13 日的 Dencun 升级 中正式上线主网。它是以太坊迈向完整 Danksharding 的中间升级: • 引入了新的“携带 blob 的交易类型” (blob-carrying transactions),为 Rollup 提供临时数据空间;• 这些 blob 数据只在共识层保存一段时间后被丢弃,不进入永久状态, 从而极大减轻 L1 的存储压力; • 实际效果是:L2 发布数据的成本大幅下降,很多 Rollup 的交易费用下 降了 90% 甚至接近 99%。 比喻: 想象你开着车(普通以太坊交易),旁边挂着一个“小挂车” (blob 数据): • 你把大批货物(Rollup 的批量交易数据)先装到挂车上; • 车跑完该跑的路,过一段时间挂车会自动脱落回收,不再长期占用空间; • 主车(L1 状态)保持轻量,而 L2 的数据传输成本大幅降低。 当前网络对 blob 数量采用保守限制: • Dencun 上线初期每个区块最多约 6 个 blob,目标平均 3 个;随着 PeerDAS 等技术部署,预计可逐步提升到每块十几个乃至几十个 blob。

**7\. 分片与 Danksharding 基础概念**: “分片”本质上是把一个大系统拆成多个“分区”并行处理,从而提升吞吐量。以太坊早期路线图曾设想通过“64 条分片链”来扩容。 在以太坊中的最新设计: • 新路线不再强调“多条执行分片链”,而是转向 Danksharding:它更 像一种“数据分片(data-sharding)”,核心是引入更多 blob 数据空间, 加上数据可用性采样(DAS),让节点只需要随机抽样检查一小部分数 据,就能对整个大数据块“有把握”。 • Proto-Danksharding 是 迈 向 完 整 Danksharding 的 第 一 步 ; 完 整 Danksharding 的目标,是在保持去中心化前提下,将每个区块可用的数据空间扩展到几十甚至上百个 blob,具体数值(如 64、128、256 等) 仍在研究和参数调优中。 可以粗略理解为:Proto-Danksharding 先给以太坊挂上“小挂车”,而完整 Danksharding 则是把挂车扩展成“高速货运列车”,专门为 L2 输送数据。

**8\. 叔块(Uncle / Ommer Block)** 叔块在转为权益证明后已停用。补充一点背景,方便读者理解这一概念的历 史位置: • 在 PoW 时期,以太坊的出块是概率性的,有时会出现几乎同时挖出两 个有效区块的情况。主链只会选择其中一个,另一个则成为“叔块” (或 称 ommer),为减少因网络延迟对中小矿工的不公平,以太坊会对被包 含的叔块给予部分奖励。 • The Merge 之后,以太坊转为 PoS,共识流程完全不同,不再产生新的 叔块;相关字段在区块头中基本被置零,仅为兼容历史数据和工具而保 留。 比喻: 想象两个演员同时冲向领奖台,但只有一个能上台领奖;另一个虽然表现不 差,只是运气稍差没被选中。主办方为了鼓励大家,给没上台的那位发了个“安 慰奖”以示认可——叔块在 PoW 时代扮演的就是这种“安慰奖”角色。 在今天的以太坊 PoS 里,这个奖项已经不再颁发,但它仍然是理解以太坊 早期设计的重要一环。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->












### 以太坊概览

1.  以太坊的介绍：以太坊（Ethereum）是一个开源的去中心化区块链平台，通过其原生加密货币以太币（Ether，简称 ETH）提供去中心化的以太虚拟机（EVM）来处理点对点合约。
    
    -   核心创新：**智能合约**。智能合约是存储在区块链上的可执行代码，能够在满足预设条件时自动执行操作，无需人工干预。
        
    -   构建去中心化应用（Dapps）、去中心化金融（DeFi）、非同质化代币（NFT）等生态系统的基础设施。
        
    -   其目标是成为全球范围内的“世界计算机”。
        
    -   以太币（ETH）是以太坊的原生代币，在以太坊上进行的任何操作或交易，都需要用ETH来支付费用（gas fee）。
        
2.  Ethereum 与 Bitcoin 的差异
    
3.  以太坊的演化
    
    -   PoW到PoS
        
    -   新架构：执行层（原主网）和共识层（信标链）
        

| 维度 | 比特币（Bitcoin） | 以太坊（Ethereum） |
| --- | --- | --- |
| 目标与定位 | 去中心化的数字货币，强调安全、稳定和稀缺性（总量 2100 万枚） | 去中心化平台，支持智能合约和 Dapps，定位为“区块链 2.0” |
| 编程能力 | 脚本语言有限，仅支持简单的交易验证逻辑 | 图灵完备的编程语言（如 Solidity），可开发复杂智能合约 |
| 共识机制 | 工作量证明（PoW），矿工通过算力竞争记账权 | 从 PoW 转向权益证明（PoS），通过 The Merge 实现能源效率优化 |
| 交易速度 | 每 10 分钟生成一个区块，交易确认较慢 | 区块时间约 12 秒，交易确认更快，适合高频应用 |
| 经济模型 | 总量固定，强调抗通胀属性 | 供应灵活，通过 EIP-1559 等机制可能呈现通缩趋势 |

![以太坊升级路线图](https://web3intern.xyz/assets/ethereum-roadmap_01-Bi2f1MRr.jpg)

1.  以太坊生态概览：L1（主网）、L2（二层扩展解决方案）、侧链（Sidechains）
    
2.  以太坊核心机制：**账户系统、Gas 模型 和 以太坊虚拟机（EVM）**
    
    -   账户系统 包含由私钥控制的 外部账户（EOA） 和由智能合约代码控制的 合约账户（CA） 。
        
        -   **Nonce**：防止重复交易的计数器（EOA 记录发送次数，CA 记录创建合约次数）。
            
        -   **余额**：账户持有的 ETH 数量（单位为 Wei）。
            
        -   **CodeHash**：EOA 为空哈希，CA 存储合约字节码的哈希值。
            
        -   **StorageRoot**：记录账户数据的 Merkle 树根哈希（如 NFT 归属关系）。
            
    -   总费用Gas Fee = Gas Limit × Gas Price
        
    -   以太坊虚拟机（EVM）：代码的执行引擎
        
        -   **图灵完备**：就像真正的电脑一样，EVM 可以执行各种逻辑，比如 if 判断、循环等。
            
        -   **全球同步**：每个矿工/节点都会自己执行一遍合约代码，保证结果一样。
            
        -   **隔离安全**：EVM 把合约“关”在一个小房间里运行，不允许它乱访问用户的数据和网络，保护隐私和安全。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->













## Web3实习手册——入门导读

### 区块链基础概念

1.  区块链的概念：区块链是一种去中心化的分布式账本技术，用于在网络节点之间安全、透明且不可篡改地记录事务数据。每条链由一系列按照时间顺序相连的“区块”组成，每个区块内部包含了多笔交易数据及元数据，确保了数据记录的完整性与可追溯性。
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/tokyoexplorer/images/2026-01-12-1768210489065-image.png)
2.  区块链特性：
    
    -   不可篡改
        
    -   公开透明、匿名
        
    -   快速交易
        
3.  一个区块链网络中有非常多的节点（矿机）来记账，每个节点都会记录完整的、相同的区块链信息，通过分布式网络确保了去中心化和不可篡改。
    
4.  通过设计代币奖励机制（比特币），为网络服务提供商提供奖励。
    
5.  区块链生态系统的运行包含以下几个关键步骤：
    
    1.  用户发起交易：用户通过钱包应用发起转账、智能合约调用等操作
        
    2.  交易广播：交易信息被广播到整个网络中的各个节点
        
    3.  节点验证：网络中的矿工节点验证交易的合法性（余额是否足够、签名是否正确等）
        
    4.  打包成块：通过共识机制（如工作量证明），矿工将验证过的交易打包成新的区块
        
    5.  链接上链：新区块被添加到区块链上，更新全网的账本状态
        
    6.  奖励发放：成功打包区块的矿工获得代币奖励和交易手续费
        
6.  公链 vs 私链 vs 联盟链
    
    ![公链、联盟链、私链对比图](https://web3intern.xyz/assets/different-chains-BJzFHow9.jpg)
    
    | 区块链类型 | 节点加入方式 | 数据可见性 | 管理模式 | 适合场景 |
    | --- | --- | --- | --- | --- |
    | 公链 | 任何人自由加入 | 所有人可见 | 去中心化（大家投票） | 加密货币、公共存证 |
    | 联盟链 | 需联盟成员邀请/审批 | 仅联盟成员可见 | 多中心化（董事会决策） | 供应链、金融协作 |
    | 私链 | 由老板严格审批 | 仅内部成员可见 | 中心化（老板说了算） | 企业内部管理、审计 |
    
7.  去中心化的优势与挑战
    
    -   优势：信任最小化、抗审查与高弹性、用户自主管理、开放创新生态、
        
    -   挑战：可扩展性瓶颈、安全与治理难题、用户体验与成本、法律与合规风险
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
