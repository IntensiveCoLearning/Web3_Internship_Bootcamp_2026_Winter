---
timezone: UTC+8
---

# Girasol

**GitHub ID:** ChainDora

**Telegram:** @LewisCAD

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-02-06
<!-- DAILY_CHECKIN_2026-02-06_START -->
## **升级合约**

升级合约是指在不改变合约地址的情况下，修改智能合约的逻辑和功能的过程。由于以太坊区块链的不可篡改性，已部署的合约代码本身是无法直接修改的。因此，“升级”并不是直接修改已部署的代码，而是通过一些设计模式和技术手段，使得合约能够以一种可控的方式更新其行为。

以下我将从几个方面详细解释升级合约：

**1\. 为什么需要升级合约？**

-   **Bug 修复：** 即使经过充分的测试，合约中仍可能存在未被发现的 bug。升级合约允许我们修复这些 bug，提高合约的安全性。
    
-   **功能增强：** 随着项目的发展，可能需要向合约添加新的功能。升级合约可以满足这些需求，保持合约的竞争力。
    
-   **性能优化：** 随着以太坊网络的发展和技术的进步，可能有更高效的合约实现方式。升级合约可以让我们采用这些新的技术，提高合约的性能。
    
-   **适应变化：** 业务需求或外部环境的变化可能需要修改合约的逻辑。升级合约可以使合约更好地适应这些变化。
    

**2\. 升级合约的常见方法**

目前，最常用的升级合约方法是使用**代理模式（Proxy Pattern）**。此外，还有一些其他方法，但相对较少使用。

-   代理模式（Proxy Pattern）：
    
    这是目前最主流的升级方式。它通过一个“代理合约”接收用户的请求，并将请求“委托”给另一个“实现合约”执行。通过更换代理合约指向的实现合约，就可以实现合约的升级。代理模式又可细分为以下几种：
    
    -   **透明代理（Transparent Proxy）：** 代理合约只负责转发调用，不包含任何业务逻辑。
        
    -   **UUPS（Universal Upgradeable Proxy Standard）：** 将升级逻辑放在实现合约中，减少了代理合约的复杂性，节省了 Gas 消耗。
        
    -   **Beacon Proxy：** 使用信标合约来存储实现合约的地址，可以更灵活地管理多个代理合约的升级。
        
-   **数据迁移（Data Migration）：** 部署一个新的合约，并将旧合约的数据迁移到新合约中。这种方法比较复杂，且会产生新的合约地址，不属于严格意义上的“升级”。
    
-   **自毁合约（Selfdestruct）：** 使用 `selfdestruct` 操作码销毁旧合约，然后部署一个新合约。这种方法会导致合约地址的改变，不推荐使用。
    

**3\. 代理模式的详细解释**

代理模式是升级合约的核心技术。其基本原理是使用 `delegatecall` 操作码。

-   `delegatecall`**：** 与普通的 `call` 操作码类似，都用于调用其他合约的函数。但 `delegatecall` 在 _调用合约的上下文中_ 执行被调用合约的代码，被调用合约可以访问 _调用合约的存储_。
    

通过 `delegatecall`，代理合约可以将所有调用委托给实现合约，但实际操作的是代理合约的存储。这样，即使更换了实现合约，数据仍然保存在代理合约中。

**代理模式的组成部分：**

-   **代理合约（Proxy Contract）：** 负责接收用户的请求，并将请求委托给实现合约执行。
    
-   **实现合约（Implementation Contract / Logic Contract）：** 包含实际的业务逻辑。可以被更新和替换。
<!-- DAILY_CHECKIN_2026-02-06_END -->

# 2026-02-05
<!-- DAILY_CHECKIN_2026-02-05_START -->

## **代理合约**

代理合约（Proxy Contract）是智能合约开发中一种重要的设计模式，主要用于解决智能合约一旦部署就无法修改的难题，实现合约的**可升级性**。简单来说，代理合约就像一个“中间人”，它接收用户的请求，然后将请求“委托”给另一个合约（称为“实现合约”或“逻辑合约”）来执行。通过更换代理合约指向的实现合约，就可以在不改变代理合约地址的情况下更新合约的逻辑。

下面我将从以下几个方面详细解释代理合约：

**1\. 为什么需要代理合约？**

以太坊上的智能合约一旦部署到区块链上，就无法直接修改其代码。这是区块链不可篡改性的一个重要特性。然而，在实际应用中，合约的逻辑可能需要修复 bug、添加新功能或进行其他更新。如果没有某种机制，我们就需要重新部署整个合约，这将导致：

-   **地址变更：** 新合约的地址与旧合约不同，所有依赖旧合约的应用和用户都需要进行相应的更新。
    
-   **状态丢失：** 新合约无法直接访问旧合约存储的数据，导致数据丢失。
    

代理合约模式就是为了解决这些问题而生的。

**2\. 代理合约的基本原理**

代理合约的核心是使用 `delegatecall` 操作码。`delegatecall` 与普通的 `call` 操作码类似，都用于调用其他合约的函数。但它们之间有一个关键的区别：

-   `call`**：** 在被调用合约的上下文中执行代码，被调用合约可以访问自己的存储。
    
-   `delegatecall`**：** 在 **_调用合约_**_的上下文中_ 执行被调用合约的代码，被调用合约可以访问 _调用合约的存储_。
    

利用 `delegatecall`，代理合约可以将所有调用“委托”给实现合约，但实际操作的是代理合约的存储。这样，即使更换了实现合约，数据仍然保存在代理合约中。

**3\. 代理合约的组成部分**

一个典型的代理合约模式包含以下两个主要部分：

-   代理合约（Proxy Contract）：
    
    负责接收用户的请求，并将请求委托给实现合约执行。它主要包含以下功能：
    
    -   存储实现合约的地址。
        
    -   使用 `fallback` 函数或 `receive` 函数接收所有调用。
        
    -   使用 `delegatecall` 将调用转发给实现合约。
        
    -   可能包含一些管理功能，例如设置管理员、升级合约等。
        
-   **实现合约（Implementation Contract / Logic Contract）：** 包含实际的业务逻辑。它可以被更新和替换。
    

### **4\. 代理合约的工作流程**

NaN.  用户调用代理合约的某个函数。
      
NaN.  代理合约的 `fallback` 函数或 `receive` 函数被触发。
      
NaN.  代理合约使用 `delegatecall` 将调用转发给当前指向的实现合约。
      
NaN.  实现合约在 _代理合约的上下文中_ 执行代码，操作的是代理合约的存储。
      
NaN.  实现合约执行完毕后，将结果返回给代理合约。
      
NaN.  代理合约将结果返回给用户。
      

**5\. 代理合约的类型**

常见的代理模式有以下几种：

-   **透明代理（Transparent Proxy）：** 需要两个合约：代理合约和实现合约。代理合约只负责转发调用，不包含任何业务逻辑。
    
-   **UUPS（Universal Upgradeable Proxy Standard）：** 将升级逻辑放在实现合约中，减少了代理合约的复杂性，节省了 Gas 消耗。
    
-   **Beacon Proxy：** 使用信标合约来存储实现合约的地址，可以更灵活地管理多个代理合约的升级。
    

**6\. 代理合约的优点**

-   **可升级性：** 允许在不改变合约地址的情况下更新合约的逻辑。
    
-   **数据持久性：** 升级后，数据仍然保存在代理合约中，不会丢失。
    

**7\. 代理合约的缺点**

-   **复杂性增加：** 引入了额外的合约和调用流程，增加了代码的复杂性。
    
-   **Gas 消耗略有增加：** 转发调用会带来一定的 Gas 开销。
    
-   **初始化复杂：** 需要仔细设计初始化流程，以确保代理合约和实现合约的状态正确设置。
    

**8\. 示例说明（简化版）**

假设我们有一个简单的计数器合约：

```
// 实现合约
contract CounterV1 {
    uint256 public count;
​
    function increment() public {
        count++;
    }
}
​
// 代理合约
contract Proxy {
    address public implementation;
​
    constructor(address _implementation) {
        implementation = _implementation;
    }
​
    fallback() external {
        assembly {
            calldatacopy(0, 0, calldatasize())
            let result := delegatecall(gas(), implementation, 0, calldatasize(), 0, 0)
            returndatacopy(0, 0, returndatasize())
            switch result
                case 0 { revert(0, returndatasize()) }
                default { return(0, returndatasize()) }
        }
    }
}
```

在这个例子中，`Proxy` 合约将所有调用委托给 `CounterV1` 合约。如果我们想要升级计数器的逻辑，只需要部署一个新的实现合约（例如 `CounterV2`），然后更新 `Proxy` 合约中 `implementation` 变量的值即可。

**总结**

代理合约是实现智能合约可升级性的重要手段。它通过 `delegatecall` 操作码将调用委托给实现合约，从而在不改变代理合约地址的情况下更新合约的逻辑。虽然代理合约增加了一定的复杂性，但在需要合约升级的场景中，它是一种非常有价值的设计模式。
<!-- DAILY_CHECKIN_2026-02-05_END -->

# 2026-02-04
<!-- DAILY_CHECKIN_2026-02-04_START -->


## **内联函数**

在Solidity中，"内联"这个概念通常与汇编（assembly）结合使用，称为“内联汇编”（inline assembly）。它指的是在Solidity代码中嵌入汇编代码块，而不是像其他一些语言那样，将一个函数标记为“内联”，指示编译器在调用处直接展开函数体。

因此，在Solidity中，没有像C++或C#那样的“内联函数”的概念。我们讨论的是“内联汇编”，它允许你直接编写EVM（以太坊虚拟机）的指令。

**什么是内联汇编？**

内联汇编允许开发者在Solidity代码中直接插入汇编代码。这些汇编代码会在编译时被直接嵌入到合约的字节码中。这提供了一种更底层、更精细地控制EVM执行的方式，常用于优化Gas消耗或实现Solidity本身难以表达的功能。

**内联汇编的语法**

内联汇编使用`assembly { ... }`块来定义。在这个块中，你可以使用Yul语言，这是一种为EVM设计的中间语言，比原始的EVM操作码更易读写。

**内联汇编的用途**

NaN.  **Gas 优化：** 有时，使用内联汇编可以比使用Solidity代码更有效地利用Gas。例如，直接操作存储或内存可以避免Solidity编译器引入的一些额外开销。
      
NaN.  **访问底层功能：** 有些EVM指令Solidity没有直接的语法支持。这时，就需要使用内联汇编来访问这些底层功能，例如访问区块哈希、调用其他合约的低级函数等。
      
NaN.  **实现复杂逻辑：** 对于一些复杂的位操作或算术运算，使用内联汇编可能更清晰、更高效。
      

**内联汇编的示例**

以下是一个简单的示例，演示如何使用内联汇编获取合约的余额：

```
pragma solidity ^0.8.0;
​
contract Example {
    function getBalance() public view returns (uint256) {
        uint256 balance;
        assembly {
            balance := balance(address()) // 使用汇编指令balance
        }
        return balance;
    }
}
```

在这个例子中，`balance(address())`是一个汇编指令，用于获取指定地址的余额。通过内联汇编，我们可以直接使用这个指令，而不需要通过Solidity的其他方式来间接实现。

**内联汇编的注意事项**

NaN.  **复杂性：** 编写汇编代码需要对EVM的内部工作原理有深入的了解，这增加了代码的复杂性和维护难度。
      
NaN.  **安全性：** 内联汇编绕过了Solidity的类型检查和安全机制，如果使用不当可能会引入安全漏洞。因此，使用内联汇编需要格外小心。
      
NaN.  **可移植性：** 内联汇编是针对EVM的，如果将来以太坊虚拟机发生变化，可能需要修改这些汇编代码。
      
NaN.  **调试难度：** 调试内联汇编代码比调试Solidity代码更困难。
      

**与“内联函数”的区别**

在C++等语言中，“内联函数”是一种编译器优化技术，它建议编译器在调用函数的地方直接展开函数体，以减少函数调用的开销。这是一种编译时的优化。

而Solidity的“内联汇编”是在Solidity代码中直接嵌入汇编代码，这是在代码编写阶段就进行的，并且直接操作EVM指令。它们是完全不同的概念。

**总结**

在Solidity中，没有传统的“内联函数”概念。我们所说的“内联”指的是“内联汇编”，它允许开发者在Solidity代码中嵌入汇编代码，以实现Gas优化、访问底层功能或实现复杂逻辑。使用内联汇编需要谨慎，因为它会增加代码的复杂性和潜在的安全风险。只有在必要时，并且对EVM有深入了解的情况下才应该使用。

### **汇编的一些差异**

我们需要区分Solidity的`return`和汇编的返回机制。

**1\. Solidity 的** `return`

Solidity的`return`关键字用于从一个Solidity函数中返回值。当Solidity编译器遇到`return`语句时，它会生成相应的EVM字节码，将返回值复制到调用者可以访问的位置。

**2\. 内联汇编中的返回机制**

在内联汇编中，你直接操作EVM的指令。EVM使用栈来传递参数和返回值。当一个函数（包括通过`delegatecall`调用的合约代码）执行完毕后，它会将返回值压入栈顶。调用者可以从栈顶获取这些返回值。

因此，在内联汇编中，你不需要使用类似`return`的关键字。你只需要确保将需要返回的数据压入栈顶即可

我们来看看这个函数

```
function _getImplementation() internal view returns (address impl) {
    bytes32 slot = IMPLEMENTATION_SLOT;
    assembly {
        impl := sload(slot)
    }
}
```

-   一般的函数中我们需要返回对应的数据类型，但在这个函数中我们还制定了对应的名字
    
-   `impl` 是函数的返回值，因为它在函数签名中被声明为 `returns (address impl)`。
    
-   在内联汇编块中，`impl := sload(slot)` 将存储槽位中的值加载到 `impl` 变量中。
    

由于 `impl` 是函数的返回值，当函数执行完毕时，`impl` 的值会自动作为返回值返回给调用者。因此，不需要显式地使用 `return` 关键字。

如果你尝试在函数内部显式地使用 `return` 关键字，例如：

```
        复制代码function _getImplementation() internal view returns (address impl) {
    bytes32 slot = IMPLEMENTATION_SLOT;
    assembly {
        impl := sload(slot)
    }
    return impl; // 这是不必要的，也是不允许的
}
```

这会导致编译错误，因为 `return` 关键字不能用于内联汇编块中。正确的方式是直接赋值给返回变量，如前所示。

### **内联汇编常用的方法**

NaN.  `sload` 和 `sstore`
      

-   `sload(slot)`: 从存储槽位加载数据。
    
-   `sstore(slot, value)`: 将数据存储到指定的存储槽位。
    

```
assembly {
    let slot := 0x12345678
    let value := sload(slot)
}    
```

2.  `mload` 和 `mstore`
    

-   `mload(pos)`: 从内存位置加载数据。
    
-   `mstore(pos, value)`: 将数据存储到指定的内存位置。
    

```
assembly {
    let pos := 0x1000
    let value := mload(pos)
}    
```

3.  `calldataload` 和 `calldatacopy`
    

-   `calldataload(pos)`: 从调用数据加载数据。
    
-   `calldatacopy(memPos, calldataPos, length)`: 从调用数据复制数据到内存。
    

```
assembly {
    let memPos := 0x2000
    let calldataPos := 0x40
    let length := 0x20
    calldatacopy(memPos, calldataPos, length)
}
```

4.  `keccak256`
    

-   `keccak256(value)`: 计算输入值的Keccak-256哈希。
    

```
assembly {
    let value := 0x12345678
    let hash := keccak256(value)
}
```
<!-- DAILY_CHECKIN_2026-02-04_END -->

# 2026-02-03
<!-- DAILY_CHECKIN_2026-02-03_START -->



**ZK-Rollups (Zero-Knowledge Rollups)**

-   **核心理念：** “有效性证明” (你必须用数学方法立刻证明自己是好人)。
    
-   **代表项目：** zkSync Era, Polygon zkEVM, Starknet。
    
-   **产品启示：** 安全性更高，提款到L1几乎瞬时完成，长期看成本更低。但技术复杂，与EVM的兼容性（zkEVM）仍在发展中。
    

**默克尔树在两种Rollup中的不同作用**

这是理解两种Rollup本质区别的关键。虽然都用默克尔树，但用法和目的完全不同。

| 特性 | Optimistic Rollup 中的默克尔树 | ZK-Rollup 中的默克尔树 |
| --- | --- | --- |
| 主要作用 | 作为欺诈证明的数据结构，用于发起挑战。 | 作为零知识证明的输入，用于证明状态转换。 |
| 核心机制 | 防御性工具：用于在挑战期内，向L1证明某笔交易是无效的。 | 证明性工具：用于生成一个数学证明，直接向L1证明整个批次的状态转换都是有效的。 |
| 验证方式 | 任何人都可以通过默克尔证明来发起挑战，L1合约会重新执行有争议的交易来裁决。 | 主链智能合约通过验证零知识证明来确认有效性，无需重新执行交易。 |
| 最终确认 | 需等待挑战期结束（约7天）。 | 证明被验证后，立即确认。 |
<!-- DAILY_CHECKIN_2026-02-03_END -->

# 2026-02-02
<!-- DAILY_CHECKIN_2026-02-02_START -->




## **IPFS NFT**

**什么是 IPFS？**

IPFS (InterPlanetary File System) 是一个**去中心化的分布式存储网络**，旨在取代传统的 HTTP 协议，构建一个更加开放、安全和高效的网络。 你可以把它想象成一个全球范围内的、点对点的文件共享系统，但它不仅仅是文件共享，更是一个内容分发网络 (CDN)。

### **IPFS 的核心概念：**

NaN.  **内容寻址 (Content Addressing):**
      
      -   **传统的位置寻址 (Location Addressing):** HTTP 使用 URL 来定位资源，URL 指定了服务器的地址和文件名。 如果服务器宕机或文件被移动，URL 将失效。
          
      -   **IPFS 的内容寻址:** IPFS 使用内容的哈希值 (Content Identifier, CID) 来唯一标识文件。 CID 是基于文件内容的加密哈希值。 这意味着如果文件内容发生任何变化，CID 也会随之改变。
          
      -   **优势:** 只要文件内容不变，就可以通过 CID 永久访问文件，无需关心文件存储在哪个服务器上。
          
NaN.  **CID (Content Identifier):**
      
      -   CID 是 IPFS 中用于标识文件的唯一哈希值。 它由多部分组成，包括哈希算法、哈希长度和哈希值本身。
          
      -   示例: `QmZm5Uzj2VbW7Kj93HtgEeqLjhQ9ZnWtXnypQf7oYsyN5u`
          
      -   CID 可以确保文件的完整性和唯一性。
          
NaN.  **分布式哈希表 (DHT):**
      
      -   DHT 是一个分布式数据库，用于存储 IPFS 网络中所有文件的 CID 及其存储位置。
          
      -   当你想访问一个文件时，IPFS 会先在 DHT 中查找该文件的 CID，找到存储该文件的节点，然后从该节点下载文件。
          
      -   DHT 使得 IPFS 能够高效地定位和检索文件。
          
NaN.  **BitSwap 协议:**
      
      -   BitSwap 是一种点对点的文件交换协议，用于在 IPFS 网络中高效地分发文件。
          
      -   当一个节点需要下载一个文件时，它会向其他节点发送一个 "wantlist"，列出它需要的文件 CID。
          
      -   其他节点如果拥有这些文件，就会将它们发送给请求节点。
          
      -   BitSwap 协议使用激励机制，鼓励节点共享文件，从而提高网络的效率。
          
NaN.  **IPFS 节点:**
      
      -   IPFS 网络由许多节点组成，每个节点都可以存储和提供文件。
          
      -   你可以运行自己的 IPFS 节点，也可以使用公共的 IPFS 节点。
          
      -   IPFS 节点之间通过 P2P 协议进行通信。
          
NaN.  **IPFS 网关 (Gateway):**
      
      -   IPFS 网关是一个 HTTP 服务器，允许你通过 HTTP 协议访问 IPFS 上的文件。
          
      -   例如，你可以使用 `https://ipfs.io/ipfs/<CID>` 来访问 IPFS 上的文件。
          
      -   IPFS 网关使得即使没有安装 IPFS 客户端的用户也可以访问 IPFS 上的文件。
          

**IPFS 的工作原理：**

NaN.  **添加文件到 IPFS:** 当你将一个文件添加到 IPFS 时，IPFS 会将文件分割成多个小的块，并为每个块计算一个 CID。
      
NaN.  **构建 Merkle DAG:** IPFS 将这些块组织成一个 Merkle DAG (有向无环图)。 Merkle DAG 的根节点就是整个文件的 CID。
      
NaN.  **分发文件块:** IPFS 将这些文件块分发到网络中的多个节点上。
      
NaN.  **查找文件:** 当你需要访问一个文件时，IPFS 会首先在 DHT 中查找该文件的 CID。
      
NaN.  **下载文件块:** 找到存储该文件的节点后，IPFS 会从这些节点下载文件块，并将它们重新组装成完整的文件。
      

**IPFS 的优势：**

-   **去中心化:** 没有单点故障，更加可靠和安全。
    
-   **内容寻址:** 确保文件的完整性和永久性。
    
-   **高效:** BitSwap 协议使得文件分发更加高效。
    
-   **版本控制:** IPFS 可以轻松地跟踪文件的版本历史。
    
-   **节省带宽:** 避免重复存储相同的文件。
    

**IPFS 的应用场景：**

-   **NFT 元数据存储:** 确保 NFT 元数据的长期可用性和防篡改。
    
-   **网站托管:** 托管去中心化的网站。
    
-   **文件共享:** 安全地共享文件。
    
-   **版本控制:** 构建去中心化的版本控制系统。
    
-   **数据备份:** 备份重要数据。
    
-   **内容分发网络 (CDN):** 构建去中心化的 CDN。
    

**如何使用 IPFS：**

NaN.  **安装 IPFS 客户端:** 下载并安装 IPFS Desktop 或 IPFS CLI。
      
NaN.  **初始化 IPFS:** 运行 `ipfs init` 命令初始化 IPFS。
      
NaN.  **添加文件到 IPFS:** 运行 `ipfs add <filename>` 命令将文件添加到 IPFS。 该命令会返回文件的 CID。
      
NaN.  **访问 IPFS 上的文件:** 可以使用 `ipfs cat <CID>` 命令查看文件内容，也可以使用 IPFS 网关通过 HTTP 协议访问文件。
      

**示例：**

NaN.  **添加一个名为** `hello.txt` **的文件到 IPFS:**
      
      ```
      ipfs add hello.txt
      added QmZm5Uzj2VbW7Kj93HtgEeqLjhQ9ZnWtXnypQf7oYsyN5u hello.txt
      ```
      
NaN.  **使用 IPFS 网关访问该文件:**
      
      ```
      https://ipfs.io/ipfs/QmZm5Uzj2VbW7Kj93HtgEeqLjhQ9ZnWtXnypQf7oYsyN5u
      ```
      

**IPFS Pinning Services:**

-   为了确保你的文件在 IPFS 网络中长期可用，你需要将文件 "Pin" 到至少一个 IPFS 节点上。 Pinning 意味着告诉 IPFS 节点存储该文件，即使该节点不再需要它。
    
-   你可以运行自己的 IPFS 节点并 Pin 文件，也可以使用 IPFS Pinning 服务，例如：
    
    -   **Pinata:** 一个流行的 IPFS Pinning 服务，提供易于使用的 API 和 UI。
        
    -   **Infura:** 提供 IPFS 和以太坊基础设施服务。
        
    -   **Web3.Storage:** 由 Protocol Labs (IPFS 的创建者) 提供的免费 IPFS Pinning 服务，专注于存储 Web3 内容。
        

**总结：**

IPFS 是一个非常有前景的去中心化存储网络，可以用于各种应用场景。 作为一名区块链开发者，了解 IPFS 的原理和使用方法是非常重要的。 尤其是在 NFT 领域，IPFS 已经成为存储 NFT 元数据的标准解决方案。 通过学习和使用 IPFS，你可以构建更加开放、安全和高效的去中心化应用。

### **IPFS与NFT相关**

发布 NFT 与 IPFS 的相关内容，包括：

NaN.  **NFT 发布流程中 IPFS 的作用**
      
NaN.  **如何准备 NFT 元数据并在 IPFS 上存储**
      
NaN.  **智能合约与 IPFS 的集成**
      
NaN.  **选择合适的 IPFS Pinning 服务**
      
NaN.  **优化 NFT 的 IPFS 存储**
      
NaN.  **常见问题及解决方案**
      

**1\. NFT 发布流程中 IPFS 的作用**

在 NFT 的发布流程中，IPFS 主要用于存储 NFT 的**元数据 (metadata)**。 元数据描述了 NFT 的属性，例如名称、描述、图像/视频 URL、创作者信息等。

发布 NFT 的大致流程如下：

-   **准备 NFT 资源:** 创建 NFT 代表的数字资产，例如图像、音乐、视频或 3D 模型。
    
-   **创建 NFT 元数据:** 创建一个 JSON 文件，描述 NFT 的属性，并包含指向 NFT 资源的 IPFS CID 或 URL。
    
-   **将 NFT 元数据上传到 IPFS:** 使用 IPFS 客户端或 Pinning 服务将元数据 JSON 文件上传到 IPFS。
    
-   **部署 ERC721 合约:** 部署一个实现了 ERC721 标准的智能合约。
    
-   **铸造 (Mint) NFT:** 调用智能合约的 `mint` 函数，将 NFT 铸造到指定的地址。 在铸造过程中，你需要提供 NFT 元数据的 IPFS CID。
    
-   **在市场上架 NFT:** 将 NFT 在 NFT 交易市场上架，例如 OpenSea、Rarible 或 LooksRare。
    

**2\. 如何准备 NFT 元数据并在 IPFS 上存储**

NFT 元数据通常以 JSON 格式存储，并遵循一定的规范。 以下是一个 NFT 元数据的示例：

```
{
  "name": "My Awesome NFT",
  "description": "This is a unique and amazing NFT.",
  "image": "ipfs://QmYBNKkhP57x38wWjqp4jFn8Y4jXeqcp7xe7wW2w4M6L44/my-nft.png",
  "attributes": [
    {
      "trait_type": "Background",
      "value": "Blue"
    },
    {
      "trait_type": "Character",
      "value": "Robot"
    }
  ],
  "external_url": "https://example.com/my-nft"
}
```

-   `name`**:** NFT 的名称。
    
-   `description`**:** NFT 的描述。
    
-   `image`**:** 指向 NFT 图像的 IPFS CID 或 URL。 推荐使用 IPFS CID，以确保图像的长期可用性。
    
-   `attributes`**:** NFT 的属性列表。 每个属性包含 `trait_type` (属性类型) 和 `value` (属性值)。
    
-   `external_url`**:** 指向包含 NFT 更多信息的外部网站的 URL。
    

**存储 NFT 元数据到 IPFS 的步骤：**

NaN.  **选择 IPFS 客户端或 Pinning 服务:** 可以选择使用 IPFS Desktop 客户端、命令行工具，或者使用 Pinata、Infura、Web3.Storage 等 IPFS Pinning 服务。
      
NaN.  **创建 NFT 元数据 JSON 文件:** 将 NFT 元数据保存为 JSON 文件，例如 `metadata.json`。
      
NaN.  **将元数据上传到 IPFS:**
      
      -   **使用 IPFS Desktop:** 将 `metadata.json` 文件拖放到 IPFS Desktop 客户端中，它会自动将文件添加到 IPFS 并返回 CID。
          
      -   **使用命令行工具:** 运行 `ipfs add metadata.json` 命令，它会返回文件的 CID。
          
      -   **使用 Pinning 服务:** 按照 Pinning 服务的文档上传 `metadata.json` 文件，并获取 CID。
          
NaN.  **确保 pinning:** 确保上传的元数据被 pinning，以便长期可用。
      

**3\. 智能合约与 IPFS 的集成**

ERC721 合约需要一个 `tokenURI(uint256 tokenId)` 函数，用于返回指定 NFT 的元数据 URI。 这个 URI 通常是一个指向 IPFS 上的元数据 JSON 文件的 URL。

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
​
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
​
contract MyNFT is ERC721 {
    using Counters for Counters.Counter;
    Counters.Counter private _tokenIds;
​
    string private _baseURI; // 用于存储 IPFS 网关前缀 + 基本路径
​
    constructor(string memory baseURI) ERC721("MyNFT", "MNFT") {
        _baseURI = baseURI;
    }
​
    function setBaseURI(string memory baseURI) public {
        _baseURI = baseURI;
    }
​
    function _baseURI() internal view virtual override returns (string memory) {
        return _baseURI;
    }
​
    function mintNFT(address recipient) public returns (uint256) {
        _tokenIds.increment();
        uint256 newItemId = _tokenIds.current();
        _mint(recipient, newItemId);
        return newItemId;
    }
​
    function tokenURI(uint256 tokenId) public view virtual override returns (string memory) {
        require(_exists(tokenId), "ERC721Metadata: URI query for nonexistent token");
​
        // 构造完整的 token URI
        string memory base = _baseURI();
        return string(abi.encodePacked(base, Strings.toString(tokenId), ".json"));
    }
}
```

**代码解释:**

-   `_baseURI`**:** 存储 IPFS 网关前缀和基本路径，例如 `ipfs://YOUR_CID/` 或 `https://YOUR_GATEWAY.com/metadata/`。
    
-   `tokenURI`**:** 拼接 `_baseURI` 和 `tokenId`，生成完整的元数据 URI。 例如，如果 `_baseURI` 是 `ipfs://YOUR_CID/`，`tokenId` 是 1，则 `tokenURI` 返回 `ipfs://YOUR_CID/1.json`。
    

**部署合约时，需要将** `_baseURI` **设置为正确的 IPFS CID 前缀。 例如，如果你将所有元数据文件存储在 IPFS 上的** `YOUR_CID` **目录下，则可以将** `_baseURI` **设置为** `ipfs://YOUR_CID/` **或** `https://YOUR_GATEWAY.com/metadata/`**。**

**4\. 选择合适的 IPFS Pinning 服务**

选择 IPFS Pinning 服务时，需要考虑以下因素：

-   **价格:** 不同的 Pinning 服务提供不同的定价方案。
    
-   **可靠性:** 选择具有高可用性和良好声誉的 Pinning 服务。
    
-   **存储容量:** 确保 Pinning 服务提供足够的存储容量来存储你的 NFT 元数据。
    
-   **带宽:** 确保 Pinning 服务提供足够的带宽来支持 NFT 用户的访问。
    
-   **API:** 选择提供易于使用的 API 的 Pinning 服务，以便你可以自动化 NFT 发布流程。
    
-   **额外功能：** 一些 Pinning 服务提供额外的功能，例如文件管理、分析和 CDN 集成。
    

一些流行的 IPFS Pinning 服务包括：

-   **Pinata:** 易于使用，提供免费套餐和付费套餐。
    
-   **Infura:** 提供 IPFS 和以太坊基础设施服务，适合大型项目。
    
-   **Web3.Storage:** 由 Protocol Labs 提供的免费 IPFS Pinning 服务，专注于存储 Web3 内容。
    
-   **Filebase:** 提供多云存储解决方案，包括 IPFS 支持。
    

**5\. 优化 NFT 的 IPFS 存储**

-   **使用内容寻址:** 使用 IPFS CID 代替传统的 URL，以确保 NFT 元数据的长期可用性和防篡改。
    
-   **选择合适的 IPFS 网关:** 可以使用公共 IPFS 网关，也可以搭建自己的 IPFS 网关。 如果使用公共 IPFS 网关，请选择可靠且速度快的网关。
    
-   **优化图像大小:** 优化 NFT 图像的大小，以减少存储成本和提高加载速度。
    
-   **使用 IPFS Companion 浏览器扩展:** IPFS Companion 可以将浏览器配置为使用本地 IPFS 节点或公共 IPFS 网关来访问 IPFS 上的文件。
    

**6\. 常见问题及解决方案**

-   **NFT 元数据无法加载:**
    
    -   确保 NFT 元数据的 IPFS CID 正确。
        
    -   检查 IPFS 网关是否可用。
        
    -   检查 IPFS pinning 服务是否正常工作。
        
-   **IPFS CID 泄露了真实 IP 地址:**
    
    -   使用 VPN 或 Tor 等工具隐藏真实 IP 地址。
        
    -   使用 IPFS Pinning 服务，避免直接运行 IPFS 节点。
        
-   **NFT 图像加载速度慢:**
    
    -   优化图像大小。
        
    -   选择速度快的 IPFS 网关。
        
    -   使用 CDN 加速 IPFS 文件的访问。
        

通过以上步骤，可以成功地将 NFT 发布到 IPFS 上，并确保 NFT 元数据的长期可用性和防篡改。
<!-- DAILY_CHECKIN_2026-02-02_END -->

# 2026-02-01
<!-- DAILY_CHECKIN_2026-02-01_START -->





## **Twitter Space 线上活动策划总结**

### **（一）全流程框架：准备 - 执行 - 复盘**

**1\. 准备阶段（核心环节）**

-   **信息确认**：明确活动主题（需贴合 Web3 运营方向，如 “Web3 社群获客技巧”“Twitter Space 活动流量转化” 等）、活动时间（需考虑目标用户活跃时段）、核心目标（如吸引新用户、提升品牌认知、收集用户反馈等）。
    
-   **嘉宾邀请**：采用多渠道邀请方式，包括自我推荐、内部邀请、行业 KOL 合作等，确保嘉宾与主题匹配度。
    
-   **物料制作**：基于统一模板设计活动海报，突出主题、时间、嘉宾、参与方式等关键信息，提升视觉辨识度。
    
-   **多平台宣发**：在 Twitter、社群、相关 Web3 平台（如 Discord、Telegram）同步发布活动信息，提前预热引流，扩大活动覆盖范围。
    

**2\. 执行阶段**

-   **流程把控**：按照预设流程推进活动，包括开场介绍（主题、嘉宾、流程）、嘉宾分享、互动问答（用户提问、嘉宾解答）、结尾总结等环节，确保节奏顺畅。
    
-   **互动引导**：实时关注评论区动态，引导用户积极提问、参与讨论，维持活动热度，避免冷场。
    

**3\. 复盘阶段**

-   **数据复盘**：重点分析核心数据，如活动浏览量、观看时长、互动率（提问数、评论数）、新增关注数等，评估活动效果。
    
-   **问题总结**：梳理执行过程中存在的问题（如宣发流量不足、嘉宾衔接不顺畅、用户互动度低等），提出优化方案，为后续活动积累经验。
<!-- DAILY_CHECKIN_2026-02-01_END -->

# 2026-01-31
<!-- DAILY_CHECKIN_2026-01-31_START -->






## **一、Uniswap V2 核心内容总结**

### **（一）核心机制：恒基做市商**

1.  **定价公式**：遵循核心等式 **XY=K**（X、Y 分别代表交易对中两种资产的储备量，K 为恒定常数），交易价格由两种资产的储备比例决定，而非传统订单簿模式。
    
2.  **关键概念补充**
    
    -   **套利机制**：当 Uniswap V2 的交易价格与外部市场价格出现偏差时，套利者会通过低买高卖操作修正价格，最终使平台价格趋近市场公允价，同时维持 XY=K 的平衡。
        
    -   **手续费修正公式**：交易时会收取一定比例手续费（默认 0.3%），手续费会注入资产储备池，实际公式调整为 **(X+ΔX)(Y-ΔY)=K**（ΔX 为买入资产的数量，ΔY 为卖出资产的数量，手续费部分计入 X 或 Y 的储备），确保 K 值不会因交易直接减少。
        

### **（二）技术与功能细节**

1.  **代码核心函数**：未明确具体函数名，但重点围绕 “资产储备更新”“价格计算”“手续费分配” 三大模块，确保交易执行时 XY=K 规则的严格遵循，以及资金流向的透明可追溯。
    
2.  **闪电借贷协议支持**：兼容闪电借贷功能，允许用户在同一区块链交易内借入资产、完成操作（如套利、清算）并归还，无需提前抵押，依托智能合约自动确保资金安全闭环。
    
3.  **链上价格预言机**：利用储备池内资产的实时储备比例（X/Y）作为价格数据源，为其他 DeFi 协议（如借贷、衍生品）提供低成本、去中心化的价格参考，但存在 “受大额交易短期影响” 的局限性。
    

### **（三）流动性相关风险与缓解**

1.  **无偿损失（Impermanent Loss）**
    
    -   **定义**：当交易对中两种资产的市场价格发生大幅波动时，流动性提供者（LP）持有的资产价值，相比不提供流动性、仅持有资产的情况出现亏损，且价格波动越大，无偿损失越严重。
        
    -   **特点**：Uniswap V2 中无偿损失难以完全避免，属于 CPMM 机制下的固有风险。
        
2.  **缓解方式**：通过提高交易手续费比例（部分场景可自定义手续费），让 LP 获得更多手续费收益，对冲部分无偿损失带来的影响，但无法从根本上消除风险。
<!-- DAILY_CHECKIN_2026-01-31_END -->

# 2026-01-29
<!-- DAILY_CHECKIN_2026-01-29_START -->







## **数据分析总结**

### **（一）区块链数据结构基础**

1.  **链上数据核心类型**：明确区块链数据的基础构成，为后续分析奠定数据认知基础。
    
2.  **交易生命周期解析**：讲解交易从发起、验证到上链的完整流程，帮助理解数据产生的逻辑与节点。
    

### **（二）Web3 独特数据指标**

1.  **关键指标：总锁仓量（TVL）**
    
    -   定义：衡量某一 DeFi 协议中用户存入的资产总价值，是评估协议规模与市场认可度的核心指标。
        
    -   与传统指标差异：区别于传统金融领域的资产规模统计逻辑，TVL 直接反映链上用户实际参与度，受市场行情、协议功能吸引力等因素影响更直接。
        
2.  **指标分析重点**：强调需结合业务场景理解指标含义，避免单一依赖数据数值判断项目价值。
    

### **（三）链上数据处理与产品**

1.  **ETL 工作流**：讲解数据抽取（Extract）、转换（Transform）、加载（Load）的完整流程，这是链上数据从原始状态转化为可分析数据的关键环节。
    
2.  **链上数据产品形态**：介绍基于处理后数据开发的各类产品，如数据仪表盘、行情分析工具等，展示数据的实际应用场景。
    

### **（四）常用 Web3 分析工具**

-   **代表工具：Etherscan**：作为以太坊区块链浏览器，可查询交易记录、地址余额、智能合约代码等基础数据，是入门级常用工具。
    
-   **工具选择逻辑**：根据分析需求（如基础数据查询、深度数据分析、跨链数据对比等）选择适配工具，无需追求 “全工具掌握”，聚焦核心需求即可。
<!-- DAILY_CHECKIN_2026-01-29_END -->

# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->








# **Plasma 总结**

## 一、Plasma 的基本概念

-   **提出时间**：2017 年，由 Vitalik Buterin 与 Joseph Poon 提出。
    
-   **目标**：解决以太坊主网 TPS（约 15–20 笔交易/秒）的瓶颈。
    
-   **核心思想**：
    
    -   在链下（子链）执行交易，提升速度与成本效率。
        
    -   定期将 **Merkle 树状态根** 提交到以太坊主网（根链），以继承部分安全性。
        
    -   通过 **欺诈证明（Fraud Proofs）** 来仲裁争议。
        

## 二、工作机制

1.  **资金锁定**
    
    -   用户将资产锁定在以太坊主网智能合约中。
        
    -   合约作为桥梁，允许资产在 Plasma 子链与主网之间转移。
        
2.  **交易处理**
    
    -   用户将交易提交给 **Operator（运营商）**。
        
    -   Operator 验证签名与余额 → 排序 → 打包成区块。
        
3.  **状态承诺**
    
    -   Operator 将区块状态构建为 Merkle 树。
        
    -   定期将 Merkle 根上传至主网，作为链下账本的“承诺”。
        
4.  **欺诈证明与挑战期**
    
    -   用户可在 **7 天左右的挑战期** 内提交欺诈证明，质疑非法交易。
        
    -   若证明成立，相关交易作废，用户可安全退出。
        

## 三、扩展性：嵌套链

-   Plasma 支持 **无限层级的嵌套子链**。
    
-   每个子链都可以将状态根提交到父链，而父链再提交到主链。
    
-   理论上可形成庞大的扩展网络，分流主网带宽。
    

## 四、优势

-   **高吞吐量**：链下执行交易，速度远超主网。
    
-   **低成本**：减少主网交易验证负担。
    
-   **安全性继承**：通过状态根和欺诈证明，部分继承以太坊安全性。
    

## 五、局限与问题

-   **数据可用性不足**：用户需依赖 Operator 提供数据，若数据丢失则无法验证。
    
-   **提现等待期长**：挑战期导致用户体验差。
    
-   **大规模退出风险**：若 Operator 恶意或宕机，用户集体退出会造成主网拥堵。
    
-   **开发复杂度高**：实现 Plasma 子链需要额外的技术与维护成本。
    

## 六、历史与现状

-   **早期应用**：Polygon（前 Matic）最初基于 Plasma 技术。
    
-   **逐渐被替代**：随着 **Optimistic Rollup** 与 **ZK Rollup** 的成熟，Plasma 因数据可用性与用户体验问题逐渐淡出主流。
    
-   **意义**：Plasma 是 Layer2 演进的重要阶段，为后续 Rollup 技术奠定了思想基础。
    

## 七、对比总结

| 技术方案 | 安全性来源 | 数据可用性 | 用户体验 | 代表项目 |
| --- | --- | --- | --- | --- |
| 侧链 | 独立共识 | 独立维护 | 快速但风险高 | xDai 等 |
| Plasma | 主网欺诈证明 | 不足 | 提现慢 | Polygon（早期） |
| Rollup | 主网验证（Optimistic/ZK） | 完全继承 | 快速、安全 | Arbitrum, zkSync |
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->









# 侧链 (Sidechain) 笔记

## 1\. 背景与动机

-   **为什么出现侧链？**
    
    -   状态通道 (State Channel) 在处理复杂交易时有局限性。
        
    -   为了突破这些限制，侧链方案逐渐受到重视。
        

## 2\. 定义与基本原理

-   **侧链是什么？**
    
    -   相对独立的区块链，通常采用与主链类似的架构（如以太坊）。
        
    -   方便主链项目迁移到侧链。
        
-   **资产跨链机制**
    
    -   在主链智能合约中锁定一定数量资产。
        
    -   在侧链上铸造等量资产，实现 **原子交换 (atomic swaps)**。
        
    -   用户可将资产存入侧链，在侧链上进行交易，再转回主链。
        

## 3\. 性能优化

-   **定制化设计**：缓解主链交易负载。
    
-   **共识机制选择**：
    
    -   **PoS (权益证明)**：提升区块生成速度，增强可扩展性。
        
        -   例：Polygon 将区块时间缩短至 2 秒。
            
    -   **PoA (权威证明)**：少量授权超级节点，性能极高。
        
        -   例：Ronin 用于游戏资产交易。
            

## 4\. 安全性与争议

-   **是否属于 Layer2？**
    
    -   有争议：侧链受主链影响，但不继承主链安全性。
        
-   **安全依赖点**：
    
    -   侧链自身的共识节点。
        
    -   主链与侧链之间的跨链桥。
        
-   **最薄弱环节原则**：
    
    -   系统安全取决于最脆弱的部分。
        
    -   若侧链或跨链桥出问题，可能危及整个生态系统。
        

## 5\. 案例

-   **Polygon (PoS)**：区块时间缩短至 2 秒。
    
-   **Ronin (PoA)**：适用于游戏内 NFT 资产交易。
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->










## ptimistic Rollup 核心机制

-   **基本原理**：假设大多数参与者是可信的。
    
-   **角色分工**：
    
    -   **验证者（Validators）**：收集、排序并验证交易。
        
    -   **挑战者（Challengers）**：负责检查验证者提交的数据是否正确。
        
-   **数据上传**：定期向以太坊主网上传两类数据：
    
    -   **状态根（State Root）**：快速验证 Layer2 账本完整性。
        
    -   **压缩交易数据**：包含用户签名等细节，主网仅公开展示，不直接验证安全与验证
        

-   **Merkle 树结构**：保存 Layer2 的“迷你账本”，记录所有账户余额。
    
-   **验证方式**：
    
    -   若信任验证者，状态根即可快速验证账本是否被篡改。
        
    -   若不信任，可独立验证交易并与账本交叉比对。
        
-   **欺诈证明（Fraud Proofs）**：
    
    -   挑战者可在 Layer1 提交欺诈证明。
        
    -   验证成功后：惩罚不诚实验证者、奖励挑战者，并对相关交易进行回滚和重新验证。
        
-   **安全保障**：只需一个诚实的挑战者即可确保整个 Layer2 的安全。
    

## 优缺点与权衡

-   **优点**：
    
    -   逻辑简单，易于理解。
        
    -   与 EVM 高度兼容，便于实现。
        
-   **缺点**：
    
    -   **退出等待期较长**：通常需要 7–14 天，给挑战者留出提交欺诈证明的时间。
        
    -   用户体验上可能显得繁琐。
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->











# ZK Vote 学习笔记

## 1\. 投票方式对比

| 方式 | 优点 | 缺点 |
| --- | --- | --- |
| 传统链上投票 | 公开透明、不可篡改、智能合约自动计票、端到端可验证 | 隐私缺失，投票记录与钱包地址永久绑定，容易关联真实身份 |
| ZK 投票 | 可验证且匿名，保护隐私，防止重复投票 | 技术复杂度高，Proof 生成耗时，需本地计算 |

## 2\. 零知识证明 基础

-   **核心矛盾**：既要保证投票可验证，又要保护投票者匿名。
    
-   **模型**：
    
    -   Prover（证明者）
        
    -   Verifier（验证者）
        
    -   Statement（语句）
        
    -   Witness（见证）
        
-   **三大性质**：
    
    -   完备性：合法投票不会被拒绝
        
    -   可靠性：无法作弊
        
    -   零知识性：不泄露身份与投票内容
        

## 3\. ZK 投票流程

1.  本地生成身份秘密 `identitySecret` 与承诺 `identityCommitment`
    
2.  加入提案，将承诺写入选民集合的 Merkle 树
    
3.  本地生成零知识证明（约 2–5 秒）
    
4.  提交投票交易，合约验证 proof 与 nullifier
    
5.  链上记录结果，可审计但不泄露身份与选票
    

## 4\. 技术细节

-   **zk-SNARK**：常用证明系统（Groth16、PLONK、Halo2）
    
-   **Merkle 树**：存储选民集合
    
-   **Nullifier**：防止重复投票
    
-   **电路约束**：定义投票逻辑
    
-   **Setup 阶段**：生成证明/验证密钥
    
-   **Proof 验证**：合约验证 proof 与 nullifier
    

## 5\. 工程实践与常见的问题

-   Proof 生成较慢，需本地计算
    
-   交易 pending：可能因网络拥堵或 Gas 不足
    
-   Proof 失败：可能因身份丢失、数据不同步或设备性能不足
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->












## **ERC721笔记**

NaN.  对于实例化的差异，
      
      ```
      IERC20 public token;
      ERC721 public nft;
      ```
      
      -   **ERC20 接口：** ERC20 标准非常简单，包含 transfer、approve、`transferFrom` 等基本方法。大多数情况下，我们只需要这些标准方法，因此直接用 IERC20 接口就足够了。接口（interface）只定义函数签名，不包含具体实现，这样可以与任何符合 ERC20 标准的合约进行交互，而不关心其内部实现细节。
          
      -   **ERC721 实现：** 对于 ERC721 来说，虽然也有对应的接口（如 IERC721），但在实际开发中，我们往往需要调用一些额外的功能（例如 `tokenURI`、metadata 扩展函数等），这些功能通常是在`OpenZeppelin` 提供的 ERC721 实现中定义的。
          
          -   如果你只需要最基本的 NFT 功能（比如`transferFrom、ownerOf`等），使用 IERC721 接口也是可以的。
              
          -   但在很多案例中，开发者倾向于直接使用 ERC721 实现，因为这样可以直接访问例如`tokenURI`这样的扩展方法，而不必额外编写代码去处理。
              
          -   ERC20合约中\_totalSupply是对应铸造给所有代币的合约
              
          -   对于ERC20的授权代币和对应的铸造代币
              
      
      总结来说，使用 IERC20 接口是因为 ERC20 的功能非常标准且简单，而对 ERC721 而言，直接使用具体实现（如 `OpenZeppelin`的 ERC721 合约）能让你利用更多附加功能和扩展（比如元数据处理），从而更好地满足 NFT 市场等应用的需求。
      

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
​
contract NFTmarket  {
// 引入对应的NFT合约
    ERC721 public nft;
    // 引入对应的ERC20合约
    IERC20 public token;
    // 创造对应NFT信息，储存对应的上架是否和价格
    // 其中对应的tokenID为key，对应的上架状态和价格为value
    // 其中的value为一个结构体，储存对应的上架状态和价格
    struct NFTinfo{
        bool isOnsale;
        uint256 price;
    }
    // 创造一个mapping，储存对应的NFT信息
    mapping(uint256 => NFTinfo) public nftinfo;
    // 创造一个mapping，储存对应的NFT的主人
    mapping(uint256 => address) public nftowner;
    // 创建对应上架的事件,记录对应的NFT的ID和价格
    event Onsale(uint256 indexed tokenID,uint256 price);
    // 创建对应下架的事件,记录对应的NFT的ID
    event Offsale(uint256 indexed tokenID);
    // 创建对应购买的事件,NFT的卖家和买家，以及对应的价格
    event Buy(address indexed seller,address indexed buyer,uint256 price);
    // 创建对应的构造函数，引入对应的NFT合约和ERC20合约
    constructor(address _nft,address _token){
        // 引入对应的NFT合约
        nft = ERC721(_nft);
        // 引入对应的ERC20合约
        token = IERC20(_token);
    }
    // 创建对应上架的方法，其中传入的参数因为对应的NFT的ID和价格
    function onsale(uint256 tokenID,uint256 price)external{
        // 首先判断对应的NFT是否存在
        require(nft._exists(tokenID),"NFT does not exist");
        // 然后判断对应的NFT是否已经被上架
        require(!nftinfo[tokenID].isOnsale,"NFT is already on sale"); 
        // 然后判断对应的NFT是否是对应的主人
        require(nft.ownerOf(tokenID) == msg.sender,"You are not the owner"); 
        // 然后判断对应的价格是否大于0
        require(price > 0,"Price must be greater than 0");
// 将合约转移至市场合约
        nft.transferFrom(msg.sender, address(this), tokenID);
        // 然后将对应的NFT的信息储存到对应的mapping中
        nftinfo[tokenID] = NFTinfo(true,price);
        // 设置对应的NFT的主人
        nftowner[tokenID] = msg.sender;
        // 然后发出对应的事件
        emit Onsale(tokenID,price);
    }
    // 创建对应下架的方法，其中传入的参数为对应的NFT的ID
    function offsale(uint256 tokenID)external{
        // 首先判断对应的NFT是否存在D),"NFT does not exist");
        // 然后判断对应的NFT是否已经被上架
        require(nftinfo[tokenID].isOnsale,"NFT is not on sale");
        // 然后判断对应的NFT是否是对应的主人
        require(nft.ownerOf(tokenID) == msg.sender,"You are not the owner");
        // 然后将NFT转移至对应的主人
        nft.transferFrom(address(this), msg.sender, tokenID);
        // 然后将对应的NFT的信息储存到对应的mapping中
        nftinfo[tokenID] = NFTinfo(false,0);
        require(nft._exists(tokenI  
        // 然后将对应的NFT的主人设置为0
        nftowner[tokenID] = address(0);、
        // 然后发出对应的事件
        emit Offsale(tokenID);
    }
    // 创建对应购买的方法，其中传入的参数为对应的NFT的ID
    function buy(uint256 tokenID)external{
        // 首先判断对应的NFT是否存在
        require(nft._exists(tokenID),"NFT does not exist"); 
        // 然后判断对应的NFT是否已经被上架
        require(nftinfo[tokenID].isOnsale,"NFT is not on sale"); 
        // 然后判断对应的NFT是否是对应的主人
        require(nft.ownerOf(tokenID) != msg.sender,"You are the owner"); 
        // 然后判断对应的价格是否大于0
        require(nftinfo[tokenID].price > 0,"Price must be greater than 0");
        // 然后判断对应的授权的代币是否足够
        require(token.allowance(msg.sender, address(this)) >= nftinfo[tokenID].price,"You do not have enough allowance");
        // 先将代币转移给给卖家
        token.transferFrom(msg.sender, nftowner[tokenID], nftinfo[tokenID].price);
        // 然后将NFT转移给对应的买家
        // 此时市场合约是对应的NFT的主人
        nft.transferFrom(address(this), msg.sender, tokenID);
//        // 然后将对应的NFT的信息储存到对应的mapping中
        nftinfo[tokenID] = NFTinfo(false,0);
        // 然后将对应的NFT的主人设置为对应的买家
        nftowner[tokenID] = msg.sender;
        // 然后发出对应的事件
        emit Buy(nftowner[tokenID],msg.sender,nftinfo[tokenID].price);
​
    }
​
}
```

### **1\. 关于 NFT 上架时为何选择转移 NFT 而不是仅仅授权**

**使用授权（Approval）方案：**

-   如果采用 ERC721 的授权方法（例如调用 `setApprovalForAll` 或 `approve`），NFT 实际上依然保留在卖家钱包中，而市场合约只是获得了操作该 NFT 的权限。
    
-   这种方式要求在 NFT 交易达成时，市场合约再通过调用 `transferFrom` 将 NFT 从卖家转移给买家。
    

**直接转移 NFT 的好处：**

-   **托管（Escrow）机制：** 将 NFT 转移到市场合约中后，市场就持有该 NFT。这确保了在 NFT 上架期间，卖家无法把 NFT 转给其他人，也避免了卖家撤销授权的风险。买家在交易时更有信心，因为 NFT 已经由合约托管，确保交易过程的顺利进行。
    
-   **简化逻辑：** 一旦 NFT 被转移到合约，后续交易（买家购买或卖家取消上架）只需要在合约内部操作 NFT，而不需要担心授权撤销或外部干预的问题。
    
-   **安全性：** 托管的方式防止了卖家在 NFT 上架后再私下转移 NFT，从而导致交易无法完成的情况。
    

**总结：** 虽然通过授权也能让市场合约操作 NFT，但直接转移 NFT 到合约中（即采用托管方式）能更好地保证交易双方的安全性和交易的确定性。这是很多 NFT 市场设计中常见的一种模式。

* * *

### **2\. 关于购买 NFT 时为何检查买家的代币授权而非仅检测余额**

**ERC20 代币转账机制：**

-   ERC20 标准中，转账函数 `transfer` 是直接从调用者余额中扣除；而 `transferFrom` 则需要先由用户通过 `approve` 授权给调用者一定额度的代币，才能由调用者从用户账户中扣除对应金额。
    
-   当市场合约调用 `token.transferFrom(msg.sender, seller, price)` 时，除了确保买家余额足够外，还必须确保买家已通过 `approve` 授权市场合约能够扣除至少 `price` 数量的代币。
    

**为什么不能只检测余额：**

-   **授权机制是必需的：** 即使买家的余额足够，如果买家没有调用 `approve` 给予市场合约足够的支出额度，`transferFrom` 操作也会失败。ERC20 设计的核心就在于分离“余额”和“授权额度”，从而确保用户明确许可第三方可以支配他们账户中的资金。
    
-   **安全性与控制：** 通过检查 `allowance`（授权额度），合约可以提前拒绝交易，提示买家先执行授权操作。这样可以避免因余额充足但未授权而导致交易失败的情况，同时让用户更清楚地了解他们已经同意让合约扣款的具体额度。
    

**总结：** 检测买家的代币授权额度是 ERC20 标准操作流程中的必要步骤，因为只有授权了相应额度，市场合约才能安全调用 `transferFrom` 执行转账操作。仅检测余额无法保证合约有权限支配买家的代币，因此授权检查是必不可少的。

* * *

总体来说，这两种设计选择都出于安全和交易流程可控的考虑：

-   **托管 NFT** 保证了 NFT 在上架期间不会被其他途径转走。
    
-   **授权检查** 确保市场合约在扣款时有足够的权限，从而使交易流程更加顺畅和安全。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->













-   figma对于平行元素只是部分元素不相同的部分只需要先将其中的一个元素建立好，部分的内容再做修改
    
-   平行图标的使用可以统一对应的大小，间距等
    
-   除了状态栏之外，下方通常会设置一个对应的标题栏大约是44px
    
-   双击对应的图标再加上对应的点就可以画出对应的不规则的图形
    
-   选中对应的元素，如底部的导航栏，在对应的position中选中对应的约束在下部分就可以让元素吸附于底部的边框
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->














-   figma实际操作过程中一些状态栏和对应的导航栏始终没有变化，我们直接从原生拿过来锁死即可，锁死的时候，在右侧选择对应的元素
    
-   按住对应的ctrl键加上对应的上下左右可以快速的调节对应的大小
    
-   标尺的使用，两边各预留16px的宽度（非必须）
    
-   选中多个元素（或图标）可以统一对多个元素的尺寸，同时可以将多个元素设置于一排之中
    
-   蒙版对应的操作可以将溢出的内容进行对应的切割
    
-   对于画面的阴影可以通过设置对应的负数值达到对应的效果
    
-   渐变色同样也可以在对应的属性栏中去设置
    
-   部分元素只需要使用对应的边框加入即可达成对应的效果
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->















## 字体的设置

> figma的字体会遵循前端字体的设计逻辑

-   字重等元素，都需要选中对应的字体才可以把设置，如中文选择对应的alibb这种
    
-   行高的设置使用默认就好，不可以使用对应的行高不可以低于对应的行高
    
-   layout的样式，可以对文字内容的排版设置对应的限制（如固定的宽度，固定的高高宽）
    
-   同时可以选择对应的对齐方式，其中有些需要选择对应的
    

## 图片和蒙版

> 导入图片其实也就相当于导入对应的矩形，设置基本逻辑保持一致

### 导入图片的几种方式

1.  直接复制粘贴，再调节对应的尺寸至合适即可
    
2.  选中对应的容器之后在导入（粘贴），图片会根据对应的容器的大小进行对应的缩放和裁切
    
    -   同时注意需要填充进对应的图像模型，如果填入对应的section的话只会放置在section的的居中位置
        
3.  对应的图片有时也需要通过不同的裁切等等操作，选择对应的图片然后再使用crop操作
    

### 蒙版的使用

需要将原来的素材和当前进行一个重叠，需要将对应展示的轮廓叠入对应的位置，然后选择`use as mask` 即可，同时我们在调整对应的位置时，按住shift或者选择对应的模块即可
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->
















右侧即为**属性栏**为详细的一些调整其中有对应的design，prototype，也就是对应的静态设计和原型模式，一般我们会先设计出对应的静态网页，如一些钱包界面，转账，出块这种，然后通过对应的图标和连线使整个过程可以串联起来，同时上方还有基本的演示按钮，如果设计出原型即可使用对应的功能

![b3f380b27ecd20b7230c37f2e966d564.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ChainDora/images/2026-01-20-1768924120711-b3f380b27ecd20b7230c37f2e966d564.png)

-   使用alt键的同时悬浮至其他元素会显示出对应的距离
    

## 图层

-   可以同时选中后多个图形后右击选择对应的group selection，同时对多个图形进行对应的操作
    
-   如果要选择对应的图形直接使用长按对应的ctrl就好悬浮鼠标就好
    
-   同时有时也会有元素的重复，同级的元素该改变对应的顺序即可
    
-   如果几个对应的元素使用对应的group分组，如果去调节对应的组的大小对应内部的元素会变得
    
-   同时也可以修改对应的名称
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->

















# figma基础学习

## 优势

外界：figma的设计基于云端，带来了协作，共享等优势

-   通过远端的服务器运行程序和储存数据
    

产品力：自动布局 聚合相关的开发模式，官方社区等等

> figma没事对应的中文版本

## 软件界面

![2de22d74089eccbf1985650464ce2f26.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ChainDora/images/2026-01-19-1768831382141-2de22d74089eccbf1985650464ce2f26.png)

总体分为三个部分，左侧为对应的的元素，例如这张图中左侧不同的页面选中不同的元素，当我们选中对应的元素的时候，即可在右侧进行基本的编辑

中间部分为**画布**，同时当开始时使用f选择对应的电脑比例，手机都是可以进行修改的，，下方为基本的工具栏，基本工具，图标，图形的，基本工具栏的右侧为对应的不同的模式，分别为草稿，设计以及对应的开发者模式

![ec7601ae2b3af545c9b16a608e1220bf.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ChainDora/images/2026-01-19-1768831407588-ec7601ae2b3af545c9b16a608e1220bf.png)

-   其中的scale（缩放）功能用于选中对应的图标
    
-   其中的section（分区）用于将一些模块进行整合
    
-   其中图形在使用的过程中，结合shift键会有不一样的功能
    

总体分为三个部分，左侧为对应的的元素，例如这张图中左侧不同的页面选中不同的元素，当我们选中对应的元素的时候，即可在右侧进行基本的编辑

中间部分为**画布**，同时当开始时使用f选择对应的电脑比例，手机都是可以进行修改的，，下方为基本的工具栏，基本工具，图标，图形的，基本工具栏的右侧为对应的不同的模式，分别为草稿，设计以及对应的开发者模式

-   其中的scale（缩放）功能用于选中对应的图标
    
-   其中的section（分区）用于将一些模块进行整合
    
-   其中图形在使用的过程中，结合shift键会有不一样的功能
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->


















## Web3 行业简历撰写指导总结

### 1\. 简历理念

-   **去中心化思维**：简历应像区块链一样透明、可追溯、可量化。
    
-   **价值来源**：链上贡献、社区参与度、可验证成果。
    
-   **三大核心**：价值（持续产出）、稀缺性（独特技能组合）、贴合度（与岗位需求匹配）。
    

### 2\. 推荐简历结构

-   **个人信息**：Telegram、Twitter、GitHub、Discord、邮箱、电话等。
    
-   **学历**：相关课程与知识体系可重点突出。
    
-   **经历**：精选 3 段以内，采用 **STAR 模型**（情境-任务-行动-结果）。
    
-   **技能栈 & 工具**：列出 Web3 技能并附项目链接。
    
-   **社区与 DAO 角色**：展示参与角色及投票/提案证明。
    
-   **奖项**：黑客松等官方背书奖项。
    

### 3\. 三板斧核心观点

-   **链上战绩 > 宏大叙事**：避免空泛描述，强调量化成果与证据。
    
-   **参与度曲线 > 从属关系**：展示持续贡献和成长路径，而非仅公司头衔。
    
-   **技能—结果双链路**：技能与实战成果结合更具说服力。
    

### 4\. 写作要领

-   **定位公式**：目标岗位 + 细分赛道 + 最大卖点。
    
-   **关键贡献**：动作 + 量化结果 + 引证，控制在 2 行以内。
    
-   **STAR 模型**：突出个人贡献与量化结果。
    
-   **技能 & 工具**：技能旁边挂作品链接。
    
-   **社区角色排序**：Core Contributor > Lead > Contributor > Bounty Hunter。
    

### 5\. 量化指标参考

-   **运营向**：社区成员增长、活动参与率、内容传播量。
    
-   **用户增长**：钱包数、交互交易量、活跃用户数。
    
-   **技术向**：合约部署次数、审计结果、代码提交量。
    
-   **注意事项**：简历控制在 1–2 页，关键贡献前置，统一格式。
    

### 6\. 模板与工具

-   使用在线简历生成器（如 Canva 模板）。
    
-   保持简洁，突出岗位相关技能与经验。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->



















## Web3 求职全流程要点总结

### 1\. 招聘平台选择黄金法则

-   **及时**：岗位仍在开放（避免过期信息）。
    
-   **真实**：岗位确实存在，不是虚假编造。
    
-   **准确**：岗位描述、薪资、地点、面试流程与实际情况匹配。
    

### 2\. 推荐的优质招聘平台

-   [**Web3Career.build**](http://Web3Career.build)：官方实习计划平台，适合系统参与。
    
-   [**Web3.career**](http://Web3.career)：全球性 Web3 招聘平台，职位更新快、覆盖广。
    
-   **DeJob**：专注 Web3 与远程招聘，国内项目更新快。
    
-   **SmartDeer**：新兴平台，注重人才质量与项目可靠性。
    

### 3\. 其他信息渠道

-   **LXDAO**：社区 DAO，提供招聘与 Bounty 岗位。
    
-   **ETHPanda**：华语区与全球以太坊生态桥梁，定期发布机会。
    
-   **官方社区**：Discord / Telegram。
    
-   **Twitter(X)**：关注 KOL、创始人、招聘专家。
    
-   **LinkedIn**：成熟公司与转型部门常用。
    
-   **行业媒体与研究报告**：CoinDesk、The Block、Messari、Nansen。
    
-   **线下活动 / AMA**：会议、Meetup、线上问答。
    

### 4\. 项目方可靠性评估（满分 120 分）

-   12 项指标：愿景、声誉、创始人背景、上市情况、办公地点、领英可检索性、市值排名、创始年份、合规性、价值观、投资机构支持、社区运营情况。
    
-   **及格线：72 分**，低于需谨慎。
    

### 5\. 常见招聘骗局特征

-   要求先付费（培训费/押金/代币）。
    
-   要求安装不明软件或插件。
    
-   公司信息无法核验。
    
-   岗位描述宽泛且承诺高回报。
    
-   要求通过非官方渠道沟通。
    

**自我保护清单**：核验主体、拒绝付费招聘、只用官方渠道、不运行未知程序、双重确认。

### 6\. Web3 职业发展路径

-   **运营向**：
    
    -   大使计划（门槛低，经验积累）。
        
    -   实习生（远程为主，待遇更高）。
        
    -   正式职工：社区经理、市场经理、BD、产品运营、内容创作。
        
-   **技术向**：
    
    -   岗位：核心开发者、智能合约工程师、Dapp 后端、审计师、密码学工程师、DevOps。
        
    -   积累经验方式：参与开源项目、黑客松、构建个人项目、学习课程、阅读白皮书。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->




















# Web3 合规与法律风险

-   **中国监管态度**：全面禁止金融属性（ICO、交易所、支付工具），有限容忍技术创新。
    
-   **代币发行风险**：ICO/IEO/IDO 等均属非法融资，技术人员参与也可能被追责。
    
-   **赌博/传销/洗钱风险**：链游、NFT、DAO 等若涉及抽奖、返利、跨境换汇，可能触犯刑法。
    
-   **民商事争议**：虚拟货币交易合同常被认定无效，投资纠纷风险自担。
    

## 全球监管趋势

-   **欧盟 MiCA**：首个全面加密货币监管框架，禁止算法稳定币。
    
-   **美国**：SEC、CFTC 等将多数代币视为证券，重点监管交易所与 DeFi。
    
-   **新加坡/香港**：采取监管沙盒或许可制度，允许创新但要求严格 KYC/AML。
    
-   **FATF Travel Rule**：要求 VASP 在转账时收集并传输用户信息。
    
-   **稳定币监管**：防范系统性风险、货币政策冲击，确保储备资产支持。
    

## Web3 入职风险

-   **雇佣关系**：境外注册公司常无法签订有效劳动合同，缺乏社保公积金保障。
    
-   **薪酬结构**：常见“人民币+Token”或“全 USDT”，可能导致工资支付无效或权益受损。
    
-   **虚拟货币出金风险**：C2C 出金易卷入涉赌涉诈资金，可能触发“帮信罪”。
    
-   **项目合法性审查**：需核查白皮书、Token 分发机制、返利结构等，避免被牵连。
    

## 常见刑事风险与案例

-   **开设赌场罪**：链游带提现功能易被认定为赌博平台。
    
-   **非法经营罪**：虚拟货币换汇、支付结算可能构成非法经营。
    
-   **非法吸收公众存款罪**：挖矿返利、代币投资模式常触发。
    
-   **传销罪**：层级返利、发展下线的链游或平台可能涉嫌传销。
    
-   **洗钱罪**：虚拟货币跨境兑换常被用于掩饰犯罪资金。
    

## 网络安全风险与防护

-   **钓鱼攻击**：伪造面试软件、奖学金空投、假客服。
    
-   **木马/恶意软件**：剪贴板劫持、浏览器插件后门、远程控制。
    
-   **社交工程**：冒充 HR/好友诱导转账或泄露信息。
    
-   **供应链攻击**：恶意插件、开源库后门。
    
-   **账号安全**：弱密码、邮箱/SIM 劫持、缺乏 2FA。
    
-   **防护清单**：只用官方会议工具、谨慎安装软件、启用 2FA、冷钱包存储、定期检查授权。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->





















## Web3 社区运营指南要点

### 一、社区运营核心职责

-   **日常内容与社群维护**：定期更新社媒内容，维持活跃度；管理秩序、答疑、打击垃圾信息；设计话题引导讨论。
    
-   **内容发布与互动引导**：活动预热公告、AMA 宣传、Twitter Space 提醒；推送品牌相关内容，引导讨论与转发。
    
-   **活动策划与组织**：线上（Twitter Space、课程、黑客松）、线下（Meetup、Workshop）；涵盖策划、执行、复盘全流程。
    
-   **对外合作与社区联动**：联合 AMA、跨社区活动；与 KOL、媒体保持合作，扩大曝光。
    

### 二、常用工具与平台

-   **社交媒体渠道**：Twitter/X（主阵地）、微信公众号、Medium/Substack。
    
-   **社群沟通平台**：Discord（分频道管理）、Telegram（轻量群聊）、微信群（中国市场常用）。
    
-   **内容创作工具**：Notion、ChatGPT、Figma/Canva、Tally/Typeform。
    
-   **数据与行业调研工具**：Etherscan、Dune Analytics、CoinGecko/CMC、Token Terminal。
    

### 三、任务案例模板

-   **线下活动策划模板**：涵盖策划（主题、目标、预算、合规）、筹备（嘉宾邀约、宣传设计、物资准备）、执行（场地布置、签到、协调）、复盘（数据分析、报告总结）。
    

### 四、关键实践要点

-   活动需明确目标与受众，量化预期效果。
    
-   宣传节奏要分阶段推进，多渠道联动。
    
-   嘉宾邀约与物料设计需提前确认。
    
-   活动后及时产出总结内容并进行数据分析。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->






















## Web3 四大核心领域

-   **DeFi（去中心化金融）**
    
    -   代表案例：Uniswap（自动做市商 AMM）、Compound（去中心化借贷）、MakerDAO/Sky（稳定币系统）。
        
    -   特点：无需中介，智能合约自动执行，流动性池与手续费分配机制。
        
    -   风险点：价格波动可能触发清算。
        
-   **NFT（非同质化代币）**
    
    -   本质：数字资产的唯一性与所有权。
        
    -   技术：区块链记录 + 智能合约自动化交易与版税。
        
    -   案例：CryptoPunks（先锋收藏品）、OpenSea（最大交易平台）。
        
-   **DAO（去中心化自治组织）**
    
    -   社区治理，无需传统公司架构。
        
    -   案例：Nouns DAO（NFT艺术社区）、LXDAO（支持 Web3 公共物品）、ConstitutionDAO（竞拍美国宪法原稿）。
        
    -   优势：公开透明、社区驱动；劣势：隐私与资金安全挑战。
        
-   **MEME 币**
    
    -   特点：社区驱动、文化属性强，投机性高。
        
    -   案例：DOGE（狗狗币）、PEPE（青蛙币）、SHIB（柴犬币）。
        
    -   风险提醒：高度波动，依赖情绪与名人效应，需控制仓位。
        

## 交叉创新

-   **DeFi + NFT**：NFT 抵押借贷、碎片化流动性（如 BendDAO、Sudoswap）。
    
-   **DAO + MEME**：社区文化与治理结合（如 ShibaSwap、FriendTech）。
    
-   **AI + DeFi**：智能化金融服务（Yearn Finance V3、Gauntlet、Chaos Labs）。
    
-   **Web3 + 乡建**：南塘 DAO 探索乡村自治与区块链结合。
    

## 未来趋势

1.  **Intent-Based 交易**：意图驱动的自动化交易（UniswapX、1inch Fusion、CoW Protocol）。
    
2.  **账户抽象与智能钱包**：ERC-4337 标准，支持 Gas 代付、社交恢复（Safe、Argent）。
    
3.  **模块化区块链**：分层架构提升扩展性（Celestia、Polygon CDK、OP Stack）。
    
4.  **AI + Web3 融合**：去中心化 AI 训练、预测市场、AI 创作（[Fetch.ai](http://Fetch.ai)、SingularityNET）。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->























## 以太坊学习要点

### 1\. 基本介绍

-   以太坊是“区块链 2.0”，不仅是加密货币 ETH，更是支持智能合约的全球共享计算机。
    
-   由 Vitalik Buterin 在 2013–2014 年提出，2014 年正式启动。
    
-   ETH 是全球市值第二的加密货币，仅次于比特币。
    

### 2\. 核心创新

-   **智能合约**：自动执行代码逻辑，无需人工干预。
    
-   支持 DeFi、NFT、DAO 等应用生态。
    
-   交易需支付 **Gas 费**，类似燃料费用。
    

### 3\. 与比特币的差异

| 维度 | 比特币 | 以太坊 |
| --- | --- | --- |
| 定位 | 数字货币，强调稀缺性 | 去中心化平台，支持智能合约 |
| 编程能力 | 脚本有限 | 图灵完备语言（Solidity） |
| 共识机制 | PoW 挖矿 | 已转向 PoS（The Merge） |
| 区块速度 | 10 分钟 | 约 12 秒 |
| 经济模型 | 总量固定 | 灵活，可能通缩 |

### 4\. 演进与升级

-   **以太坊 1.0**：PoW 挖矿，能耗高，TPS 低。
    
-   **The Merge (2022)**：转向 PoS，能耗降低 99.95%。
    
-   **未来路线图**：
    
    -   **分片技术**：数据分片 + Layer 2，预计 2025–2026 启动。
        
    -   **EIP-4844 (2024 已上线)**：Blob 交易降低 L2 成本。
        
    -   **ZK-Rollup**：零知识证明提升效率与安全。
        
    -   其他优化：Verkle 树、EVM 性能提升。
        

### 5\. 生态系统

-   **L1 主网**：安全与共识。
    
-   **L2 扩展**：Rollup（Arbitrum、Optimism、zkSync Era 等）。
    
-   **侧链**：Polygon PoS、xDAI。
    
-   **应用层**：Uniswap、Aave、OpenSea、MetaMask 等。
    
-   **协议层**：EVM、Gas 机制、客户端软件。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->
























## 区块链基础概念

### 一、区块链是什么

-   去中心化的分布式账本技术，记录不可篡改、透明的交易数据
    
-   区块由交易信息 + 上一区块哈希组成，按时间顺序串联
    
-   特性：不可篡改、公开透明但匿名、交易快速
    

### 二、比特币与激励机制

-   节点提供服务可获得奖励（如比特币）
    
-   比特币具备货币属性：有限供应、可自由转账
    
-   矿工通过验证交易、打包区块获得代币奖励和手续费（Gas Fee）
    

### 三、区块链核心组成

-   **去中心化网络**：全球分布的节点共同维护账本
    
-   **区块链本身**：记录所有交易信息
    
-   运行流程：用户发起交易 → 广播 → 节点验证 → 打包成块 → 上链 → 奖励发放
    

### 四、公链 vs 联盟链 vs 私链

-   **公链**：完全开放，任何人可参与，透明但效率低（如比特币、以太坊）
    
-   **联盟链**：由多方机构共同管理，效率较高，隐私较好（如银行联盟）
    
-   **私链**：由单一机构控制，效率高、隐私强，但缺乏透明性（企业内部应用）
    

### 五、Web2 / Web 3.0 / Web3 对比

-   **Web2**：中心化平台，用户生产内容但不拥有数据
    
-   **Web 3.0**：语义网，强调数据结构化与语义关系
    
-   **Web3**：去中心化互联网，用户掌握数据与资产，智能合约自动执行
    
-   对比矩阵：
    
    -   控制权：Web2 平台垄断 → Web 3.0 部分开放 → Web3 用户自治
        
    -   数据存储：中心服务器 → 混合存储 → 区块链/IPFS
        
    -   支付系统：信用卡/支付宝 → 集成支付 → 加密货币
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
