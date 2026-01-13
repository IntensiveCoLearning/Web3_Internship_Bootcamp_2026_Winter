---
timezone: UTC+8
---

# vstral

**GitHub ID:** vstralcn

**Telegram:** vstral

## Self-introduction

我对区块链的发展非常感兴趣，希望能与大家一起学习！

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
\# 以太坊诞生背景

2013年，BTC称为去中心货币网络的先驱，但其功能主要限于转账，内建脚本语言不具备图灵完备，难以支持多样化逻辑

于是\*\*Vitalik Buterin\*\*提出建立一个无需信任的计算平台，诞生了ETH，被定义为

**一个具备状态、逻辑、可执行代码的去中心化通用平台**

思维跃迁：

\- 比特币 = 去中心化的货币系统

\- 以太坊 = 去中心化的逻辑系统（世界计算机）

\# 以太坊的核心组成

| 模块 | 描述 |

| ------------------ | ---------------------- |

| 🧠 EVM（以太坊虚拟机） | 合约字节码的运行环境，负责逻辑执行 |

| 📜 智能合约 | 可部署在链上，自动执行、可被调用的代码体 |

| 🌐 状态树（State Trie） | 存储所有账户、变量与状态快照 |

| ⛽ Gas 机制 | 控制计算资源消耗、防止滥用的经济模型 |

| 🔐 共识机制 | 保障账本一致性（从 PoW 过渡至 PoS） |

!\[image.png\]([https://vstral-1300032599.cos.ap-chengdu.myqcloud.com/blog/20260112001915.png](https://vstral-1300032599.cos.ap-chengdu.myqcloud.com/blog/20260112001915.png))

\# EVM执行引擎

[https://github.com/chaseSpace/learn\_blockchain/blob/main/ethereum\_execute\_contract.md](https://github.com/chaseSpace/learn_blockchain/blob/main/ethereum_execute_contract.md)

[https://learnblockchain.cn/article/16784?column\_id=134](https://learnblockchain.cn/article/16784?column_id=134)

\## 1. EVM

EVM是一个基于栈的、大端序的虚拟机，类似于JVM，EVM也是一个在真实计算机之上设计并创建出来的支持一套自定义指令集的计算机，它还包括一个栈和两个存储域，（memory和storage）

EVM的设计之初就是嵌入ETH客户端，即EVM运行在以太坊系统之上，EVM的作用是运行智能合约，合约是在外部账户通过一笔交易创建的，合约字节码会附在交易的data中。同样，交易也可以通过携带data的方式与合约进行各种类型的交互，例如调用、销毁合约等。

\## 2. 合约字节码

合约字节码由一系列操作符（也叫指令）构成，每个操作符都可以编码成一个字节面量，除了PUSHn。EVM指令集支持多个PUSH指令，如PUSH1、PUSH2等，后面的数字指的是入栈的数据字节大小。PUSH1就是入栈1字节数据，PUSHn由于携带了数据，所以该指令占用空间不定`PUSH1 0x00`占用2字节`PUSH2 0X0010`占用3字节等）

\## 3. 合约构造函数

合约在创建成功后，构造函数将从中剔除、即构造函数不会出现在已部署的合约中

\## 4. Call Data

是在调用合约时附在交易的data字段的信息，通常包含一个4字节的方法标识，方法标识的构造方式：

keccak256("somefunc(uint)uint")\[:4\]

即函数签名的keccak256哈希后的前4字节。

\## 5. 程序计数器

计数器与栈、Memory和Storage协作，共同完成合约字节码的执行工作

计数器的本质是一个部署后的操作码序列的offset，也就是执行指针，计数器执行的位置就是下一步执行指令的位置

\## 6. 执行环境

EVM在开始执行合约字节码的时候，回味这个合约创建一个临时且独立的上下文环境，具体来说就是创建几个单独的内存区：

\- 代码区：静态只读区，存储合约字节码。可通过CODESIZE、CODECOPY指令读取，其他合约代码可通过EXTCODESIZE和EXTCODECOPY指令读取

\- 栈：是一个32字节元素、容量为1024的数组空间，用来存放EVM指令需要的参数和返回的空间。指令对栈元素的存取只能从栈顶开始。通常有如下指令：

PUSH1、DUP1、SWAP1、POP

\- Memory：是一个单字节元素的数组空间，用来存放合约执行过程中的瞬态数据。Memory空间通过字节offset来访问。通常MLOAD、MSTORE、MSTORE8会操作memory（M前缀）

\- Storage：不同于前两个结构，storage用于存放持久化数据的一个map结构，key和valude都是uint256类型，通常指令SLOAD、SSTORE操作Storage（S前缀）

\- calldata：交易发生时附带的数据，静态只读区。例如合约创建时，calldata的内容是构造器代码。通常CALLDATALOAD, CALLDATASIZE, CALLDATACOPY可读取（CALLDATA前缀）

\- return data：存放合约的返回值，通过RETURN、REVERT修改，指`RETURNDATASIZE, RETURNDATACOPY`读取；
<!-- DAILY_CHECKIN_2026-01-13_END -->
<!-- Content_END -->
