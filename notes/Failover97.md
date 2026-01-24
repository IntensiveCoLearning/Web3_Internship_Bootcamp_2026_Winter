---
timezone: UTC+8
---

# Failover97

**GitHub ID:** Failover97

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->
### Oracle Manipulation Attacks(预言机操纵攻击）

漏洞：盲目依赖单一数据源信息

例一：闪电贷进行链上价格操作。通过大量买入（稀缺性）抬高单一货币价格，已虚高价格进行质押获利，大量以原价卖出归还闪电贷。

```jsx
攻击者可以对即将到账的资产 A 进行闪电贷，然后在相关的 Uniswap 资金池中，以大笔交易将资产 A 兑换成资产 B。
这项交易将提高资产 B 的价格（需求增加），并降低资产 A 的成本（供给增加）。
当资产 B 存入上述函数时，其价格仍然会被闪电贷推高。
因此，资产 B 使攻击者获得了不成比例的股份数量。
这些份额可以被提取，使攻击者从资产池中获得等量的 A 和 B 资产。
重复这个过程会耗尽所有脆弱资金池中的资金。
攻击者可以利用赎回股份所得的资金偿还闪电贷。
```

### Reentrancy

重入漏洞

**单功能重入**

```jsx
/漏洞合约，先转账在修改余额，攻击者通过回退函数不断提取余额
contract Vulnerable {
    mapping (address => uint) private balances;

    function withdraw() public {
    uint amount = balances[msg.sender];
    (bool success, ) = msg.sender.call.value(amount)("");
    require(success);
    balances[msg.sender] = 0;
    }
}
```

```jsx
/攻击者利用回退函数 不断提取合约余额
receive() external payable {
    // 此时 balances[攻击者] 还是 1 ETH（没有被清零）
    if (address(target).balance > 0) {  // Vulnerable 还有 9 ETH
        target.withdraw();  // 🔁 再次调用 withdraw！
    }
}
```

**Cross-Function Reentrancy 跨函数重入**

```jsx
contract Vulnerable {//用于内部记账，影响withdraw balances的状态
    mapping (address => uint) private balances;

    function transfer(address to, uint amount) public {
        if (balances[msg.sender] >= amount) {
        balances[to] += amount;
        balances[msg.sender] -= amount;
        }
    }

    function withdraw() public {
        uint amount = balances[msg.sender];
        (bool success, ) = msg.sender.call.value(amount)("");
        require(success);
        balances[msg.sender] = 0;
    }
}
```

跨函数重入的特点：

1、balance共享状态

2、防住一个函数的重入状态防不住另一个

3、重入窗口存在（balances\[msg.sender\]仍然在转账成功后修改）

在这个例子中，攻击者可以先调用该`withdraw`函数，并在收到价值转移请求后，再次进入合约，但这次仍然调用该`transfer`函数。由于`withdraw`攻击者通过该函数重新进入时，之前的调用尚未完成`transfer`，因此该函数的余额映射`msg.sender`尚未被重置为零。结果，攻击者除了可以提取的金额外，还可以转移其他资金
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->

今天完善了一下领英和web3 security governance的英文简历，就不在这里po了
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->


### **参数顺序错误**

solidity不支持命名参数

```jsx
# Python 支持命名参数
def transfer(to, amount, notify):
    pass

# 可以任意顺序，只要指定名字
transfer(amount=100, notify=True, to="0x123...")  ✅
transfer(notify=True, to="0x123...", amount=100)  ✅
```

所以solidity必须严格按照顺序传递参数

### Oracle Manipulation Attacks(预言机操纵攻击）

漏洞：盲目依赖单一数据源信息

例一：闪电贷进行链上价格操作。通过大量买入（稀缺性）抬高单一货币价格，已虚高价格进行质押获利，大量以原价卖出归还闪电贷。

```jsx
攻击者可以对即将到账的资产 A 进行闪电贷，然后在相关的 Uniswap 资金池中，以大笔交易将资产 A 兑换成资产 B。
这项交易将提高资产 B 的价格（需求增加），并降低资产 A 的成本（供给增加）。
当资产 B 存入上述函数时，其价格仍然会被闪电贷推高。
因此，资产 B 使攻击者获得了不成比例的股份数量。
这些份额可以被提取，使攻击者从资产池中获得等量的 A 和 B 资产。
重复这个过程会耗尽所有脆弱资金池中的资金。
攻击者可以利用赎回股份所得的资金偿还闪电贷。
```
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->



### Rag

RAG（检索增强生成）—Retrieval-Augmented Generation

**解决问题：**一、知识有截止日期 二、产生幻觉

**技术流程：**

-   索引阶段（离线准备）：chunks(将知识库切成小块+Embedding Model(存储于向量库）——>上下文构建
    
-   检索阶段（实时查询）：在向量库进行相似度搜索
    
-   增强阶段（上下文构建）：检索到文档片段（context)+用户问题=prompt
    
-   生成阶段（LLM输出）
    

**技术要点**

chunk size: 大—>引入噪音 小—→丢失上下文

embedding model:平衡语义理解能力和推理速度

vector retrieval: hybrid search+稠密向量检索/稀疏检索提升效果

### MCP（Model context Protocol)

**目的：**在MCP之前,每个工具都需要单独集成,开发者要为每个API写专门的适配代码。MCP提供了统一的协议规范,就像USB标准让各种设备都能即插即用。

**技术架构**

MCP采用client-server架构。AI系统作为MCP client,各种工具/数据源作为MCP server。协议定义了三个核心概念:

-   **Resources**(资源):AI可以读取的数据源,如文件、数据库记录、API响应
    
-   **Prompts**(提示模板):预定义的操作模板,降低AI使用工具的难度
    
-   **Tools**(工具):AI可以主动调用的函数,实现具体操作
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->




## 工作原理

当你拥有一个 ERC-7962 代币时，所有权记录的是你的公钥哈希，而不是你的地址。要转移代币，你需要提供：

-   你的完整公钥（65字节的 secp256k1 未压缩公钥）
    
-   用对应私钥签名的 ECDSA 签名（对授权转移协议的私钥签名）
    
-   目标密钥哈希（接收方）
    
-   签名截止时间
    

合约会验证 `keccak256(公钥) = 当前所有者的keyHash`，并验证签名是否由该公钥对应的私钥生成。验证通过后，所有权转移到新的 keyHash。

## 两大优势

**隐私保护**：链上只存储密钥哈希，不存储地址。你可以为每个代币使用不同的密钥对，大大降低交易之间的关联性。

**Gas 费分离**：任何人都可以提交转账交易（只要有有效签名），这意味着第三方可以代付 Gas 费，而不会获得代币控制权。这对批量交易和 Gas 赞助场景非常有用。

## ERC-KeyHash20 的特殊设计

ERC-KeyHash20（同质化代币）采用了类似比特币 UTXO 的模型。转账时，原始余额清零，转账金额发送到接收方的 keyHash，**剩余余额**必须发送到一个新的 keyHash（`leftKeyHash`）。这强制执行密钥轮换，进一步增强隐私性和不可关联性。

## 安全机制

该标准使用 EIP-712 结构化签名防止消息篡改，使用每个 keyHash 的 nonce 防止重放攻击，并设置签名截止时间限制签名有效期。这些机制共同确保只有真正的所有者才能操作代币。

这个标准特别适合需要匿名性的收藏品、私密交易或去中心化身份应用场景。

阅读材料

GITHUB:[https://github.com/ethereum/ERCs/blob/master/ERCS/erc-7962.md](https://github.com/ethereum/ERCs/blob/master/ERCS/erc-7962.md)

magician：[https://ethereum-magicians.org/t/erc-7962-key-hash-based-tokens/24422](https://ethereum-magicians.org/t/erc-7962-key-hash-based-tokens/24422)

怎么理解？把安全问题放到grok里面，攻击

判断一个协议：把web3的一些特性的设计初衷来判断，协议的设计会不会符合透明性、去中心化的特征。解决问题之间是如何权衡的。

**NFT**是基于ERC-721的一个实践，在现实世界中通常表达为一个藏品。——性质在区块链上有ownership的记录，溯源透明。

[ERC-7962协议](https://www.notion.so/ERC-7962-2ec7fcc97cdf80beaa27dcbfde89260b?pvs=21)

## ERC-7962

**核心概念**

1.  以 keccak256(公钥) 而非地址作为资产所有权标识
    
2.  所有权与交易发起地址解耦，不在链上存储地址
    
3.  覆盖ERC-20 （稳定币）与ERC-721（NFT）两类资产模型
    
4.  用户只需签名表达意图，无需持币、无需理解 Gas、无需管理地址隐私，也能完成资产操作
    

**解决关键问题**

1.  隐私问题:避免地址可追踪，降低身份与资产的关联性
    
2.  UX问题:用户只需签名表达意图，无需持币、无需理解Gas、无需管理地址隐私，也能完成资产操作。——-降低用户的操作困难
    
3.  扩展性问题:天然支持批量交易、Relayer、账户抽象场景
    

**技术要点** 所有权由 keyHash=keccak256(publicKey)表示转账时提交:

-   公钥
    
-   EIP-712签名
    
-   nonce + deadline (防重放)
    
-   通过ECDSA验证签名人与keyHash 的一致性
    

**ERC-KeyHash20 (FT)**

-   keyHash balance余额模型
    
-   转账采用 UTXO风格拆分
    
-   转出部分toKeyHash
    
-   剩余部分 leftKeyHash
    
-   强制key轮换，增强不可关联性
    

**Gas 与交易模型**

-   任何地址都可提交交易
    
-   所有权只由签名控制
    
-   天然支持: Gas Sponsorship（代付Gas费）——交易的发起者和token的owner是分开的，可以由任何一个relayer发起交易 批量转账 Relayer / Paymaster
    

**ERC-7962的设计初衷：**

-   希望通过ZK能证明身份的同时，不暴露隐私。
    
-   ERC-7962在ERC-721的基础上应用ZK原理保证了向第三方的证明其归属，但不暴露真实地址
    

**优势总结**

-   更强隐私(地址不上链)
    
-   更佳用户体验(免ETH/Gas代付)
    
-   更高灵活性(批量、代付、抽象账户)
    
-   与 ERC-5564 (StealthAddress)理念一致
    

**局限与代价**

-   Gas成本高于传统ERC-20/ERC-721!
    
-   公钥在 calldata中暴露(需key轮换)!
    
-   与现有ERC标准不向后兼容
    
-   对密钥管理要求更高
    

**安全设计**

-   EIP-712结构化签名，防参数 篡改
    
-   per-keyHash nonce，防重放 攻击（防止relayer作恶，double-check）
    
-   deadline控制签名有效期
    
-   强制low-S/v校验，防签名可塑性
    

**适用场景**

-   隐私资产/匿名
    
-   NFTWeb3新手友好
    
-   钱包企业级批量分发、空投Account Abstraction /Paymaster 体系'
    

### Zero Knowledge Proof

我能证明我知道某个秘密/满足某个条件，但我不需要把秘密本身告诉你。

例如：大素数分解、哈希

## 知识补充

1、Relayer = “代你把交易广播上链、并帮你先垫 gas 的中间人/转发者”。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->





### FrontRunning

区块链网络上的每笔交易在执行前都会在内存池中经历一段可见期。这种透明性使得网络参与者能够在交易被打包进区块之前查看并对其做出响应。攻击者可以利用这种信息泄露来影响交易的执行过程。

**直觉解释**：利用交易在Mempool中的可见期，在之前、之中和之后插手，改变结果，从而达到牟利的目的。

**置换攻击**

```jsx
pragma solidity ^0.8.17;

contract GuessTheNumberChallenge {
    bytes32 challenge;

    constructor(bytes32 _challenge) payable {
        require(msg.value == 1 ether);
        challenge = _challenge;
    }

    function isComplete() public view returns (bool) {
        return address(this).balance == 0;
    }

    function guess(uint256 number) public payable {
        require(msg.value == 1 ether, "Submission fee required");
        uint256 balance = address(this).balance;
        require(balance != 0, "Game has ended");

        bytes32 userChallenge = keccak256(abi.encode(number));
        if (userChallenge == challenge) {
            (bool success, ) = msg.sender.call{value: balance}("");
            require(success, "Transfer failed");
        }
    }
```

合约逻辑：

猜数字游戏，猜到数字的人可以提取合约中所有的余额

该系统存在一个根本性的设计缺陷。当用户准备提交猜测时，他们会调用一个`guess`函数并将猜测的数字作为参数传递。除非采取特殊预防措施，否则该交易会在被挖矿并打包到区块之前就出现在内存池中。这种可见性使得任何观察者都有机会复制交易数据，其中包括`guess`函数签名及其参数。

通用抢先者可以复制此交易数据，并在分叉网络上模拟该交易（分叉链居然可以clone一个合约的所有状态包括状态参数的值？？？）。如果交易盈利，他们可以重新提交复制的有效载荷，并支付高于原始用户的 gas 价格。这种在 gas 价格拍卖中出价更高的行为，增加了他们的交易在原始用户交易之前被挖矿的概率。

这种情况反映了抢先交易的一个关键方面。一旦复制的交易数据被提交并被挖矿，原始交易在智能合约系统的上下文中就变得无关紧要了。

**插入攻击**

滑点窃取攻击或者叫夹心饼干（Sandwich Attack)攻击。

![image.png](attachment:9e1bc133-3f66-498a-ba4e-fde34d7850e7:image.png)

假设你发起交易（进入mempool)

所有人都能看到：你要买什么、买多少、滑点是多少（可接受的差价范围）

攻击操作：

攻击者抢先买币—>推高价格至滑点可接受的最高价——>你买币——>攻击者趁机卖出——→\*\*结果：\*\*攻击者赚到了你接受的差价

危机直觉：只要用户交易公开、价格即时操纵、只要存在滑点就有被夹空间

**抑制攻击（block stuffing)**

攻击目的：让某些关键交易晚进区块，来达到操纵市场的目的：比如预言机更新、清算、开奖、最后一笔抢购。

手段：攻击者用高优先级费用+高gas消耗占满区块容量，让别人的交易连续几个区块都进不去，从而改变结果或者制造系统性风险。

EIP-1559缓解风险——把“持续填满区块”的成本抬高

```jsx
// EIP-1559 之前
交易费用 = gasUsed × gasPrice
// gasPrice 全部给矿工

// 区块容量固定
区块 gas limit = 15,000,000 gas
```

成本：const attackCost = (blockGasLimit × attackGasPrice) / 1e9;

攻击成本有限；如果攻击者本身就是矿工就可以免费填充自己的区块

EIP-1599的新的费用结构

```jsx
// EIP-1559 之后
交易费用 = (baseFee + priorityFee) × gasUsed

// baseFee：基础费，被销毁 🔥
// priorityFee：小费，给验证者
// Base Fee 调整机制

if (上一个区块使用量 > 目标容量) {
    baseFee 增加 12.5%
} else if (上一个区块使用量 < 目标容量) {
    baseFee 减少 12.5%
}

// 弹性区块
目标容量 = 15,000,000 gas
最大容量 = 30,000,000 gas (2倍弹性)
```

![image.png](attachment:6b9ffd4e-c1e6-4482-befc-dcf695d3a2f5:image.png)

**恶意攻击**

拒绝服务攻击

```jsx
pragma solidity ^0.8.17;

contract DelayedWithdrawal {
    address beneficiary;
    uint256 delay;
    uint256 lastDeposit;

    constructor(uint256 _delay) {
        beneficiary = msg.sender;
        lastDeposit = block.timestamp;
        delay = _delay;
    }

    modifier checkDelay() {
        require(block.timestamp >= lastDeposit + delay, "Keep waiting");
        _;
    }

    function deposit() public payable {
        require(msg.value != 0);
        lastDeposit = block.timestamp;
    }

    function withdraw() public checkDelay {
        (bool success, ) = beneficiary.call{value: address(this).balance}("");
        require(success, "Transfer failed");
    }
}
```

延迟提现合约：受益人必须在lastDeposit时间戳+delay的时间间隔才能实现取现。然而攻击者可以通过不断的更新lastDeposit，导致受益人无法提现。

**毒气恶意攻击（Infficient Gas Griefing)**

这种攻击中，攻击者可能只提供足够的gas来确保顶层函数成功执行，同时确保外部调用因gas耗尽而失败。由于[63/64规则](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-150.md)，顶层合约可以完成其函数调用，而顶层函数不检查外部调用是否成功，从而导致合约目的最终不能执行。

常见毒气攻击合约：此类问题在执行**通用调用**的智能合约中尤为常见，例如中继器和多重签名钱包。

```jsx
contract Relayer {
    // 记录哪些数据已经执行过（防止重放攻击）
    mapping (bytes => bool) executed;
    
    // 目标合约地址
    address target;
    
    function forward(bytes memory _data) public {
        // 步骤 1：检查这个数据是否已经执行过
        require(!executed[_data], "Replay protection");
        
        // 步骤 2：（代码中省略）验证签名
        // ... signature validation ...
        
        // ❌ 步骤 3：标记为已执行（危险！先改状态）
        executed[_data] = true;
        
        // ❌ 步骤 4：外部调用（没检查返回值！）
        target.call(abi.encodeWithSignature("execute(bytes)", _data));
        //     ↑
        //   这就是外部调用！
    }
}
```
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->






## Exposed Data

区块链看似匿名的特性可能会给用户带来虚假的安全感。只要链上拥有足够的数据，用户的匿名性就很容易被破解。个人身份信息（PII）

**数据保护合规性**

对于将PII存储在链上时违反例如(GDPR)欧盟的《通用数据保护条例》的要求。

除了PII之外IP地址和电子邮件等信息存储在链上也会造成巨大风险。

## FrontRunning（今天太累了关于抢跑还有一些内容没学完，明天补充 ）

区块链网络上的每笔交易在执行前都会在内存池中经历一段可见期。这种透明性使得网络参与者能够在交易被打包进区块之前查看并对其做出响应。攻击者可以利用这种信息泄露来影响交易的执行过程。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->







# JS的特点

在 JavaScript 里，某些操作（比如**读取文件、网络请求、数据库查询、区块链交互**等）需要**一定的时间**才能完成。JavaScript 采用**异步编程（异步的意思指的是好几个耗时线程可以并行操作，不会被block)**来避免阻塞主线程。`await` 让代码 **按顺序执行**，避免出现未完成的任务导致错误。 默认会把 **I/O 操作（如数据库、网络请求、区块链调用）** 设为异步

### 什么是合约工厂？

合约工厂（Contract Factory）**是用于部署新智能合约的对象**。在 Hardhat + Ethers.js 中，你不能直接创建合约实例，而是需要先获取合约工厂，然后再部署合约。

**这和现实中的“工厂”很类似**：

-   **合约文件 (**`DiceMarket.sol`**)** 就像**蓝图**。
    
-   **合约工厂 (**`getContractFactory("DiceMarket")`**)** 就像一个**工厂**，可以根据**蓝图**生产多个合约实例。
    
-   **部署后的合约 (**`DiceMarket.deploy(...)`**)** 就像**最终生产出来的产品**。
    

# 关键字

### async

-   申明一个异步函数，返回值始终是Promise
    
-   如果函数内部返回的是普通值，JavaScript会自动封装成Promise.resolve(返回值）
    

```
async function hello() {
    return "Hello, world!";
}

hello().then(console.log); // 输出: Hello, world!由于关键字的作用，所以需要用.then()处理
```

### await

该关键字只能在async function内部使用，

它会暂停函数的执行，直到Promise解析（resolve),然后返回Promise解析之后的值

```
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function run() {
    console.log("开始");
    await delay(2000); // 等待 2 秒
    console.log("2 秒后执行");
}

run();
```

代码里的语法：

new Promise((resolve,reject)=>{.....}

resolve是一个函数，表示成功时候调用

reject也是一个函数，表示失败时候调用

setTimeout(resolve,ms)

setTimeout是JavaScript的定时器函数，作用是等待一段时间后执行某个操作

setTimeout(resolve,ms)代表等待ms毫秒后执行resolve()

### console

在 JavaScript 中，`console` 是一个**全局对象**，主要用于**打印调试信息**，帮助开发者查看代码的执行情况。最常用的方法是 `console.log()`，但 `console` 还有很多其他功能。

```
console.log("Hello, JavaScript!");
```

console的其他用法

![](https://cdn.nlark.com/yuque/0/2025/png/34360919/1741086687786-5e0d8cd5-cb6c-401b-aa9d-4a4a85756fe5.png)

# 变量声明和数据类型

### 变量声明的三种方式

```
// let - 推荐使用,可以重新赋值
let age = 25;
age = 26; // 允许

// const - 用于常量,不能重新赋值
const PI = 3.14;
// PI = 3.15; // 这会报错

// var - 旧版本使用,现在不推荐
var name = "John";
```

### 基本数据类型

```
// 数字 (Number)
let count = 42;        // 整数
let price = 99.99;     // 小数
let infinity = Infinity;//特殊用法，表示无穷大
let notANumber = NaN;//not a number

// 字符串 (String)
let firstName = "Alice";
let greeting = 'Hello';
let message = `Hello ${firstName}`; // 模板字符串

// 布尔值 (Boolean)
let isActive = true;
let isLoggedIn = false;

// 空值
let empty = null;
let undefined;

// 数组 (Array);❤这里的数组跟python里面的列表特别像
let colors = ['red', 'green', 'blue'];
let numbers = [1, 2, 3, 4, 5];

// 对象 (Object) 对象非常像C语言里面的结构体
let person = {
    name: "Alice",
    age: 25,
    isStudent: true
};
```

### 类型转换

```
// 转换为数字
let strNumber = "123";
let num1 = Number(strNumber);    // 123
let num2 = parseInt(strNumber);  // 123
let num3 = parseFloat("123.45"); // 123.45  
let bigNum=BigInt("123455677766");//bigint专门用于大整数

// 转换为字符串
let number = 123;
let str1 = String(number);      // "123"
let str2 = number.toString();   // "123"

// 转换为布尔值
let bool1 = Boolean(1);         // true
let bool2 = Boolean("");        // false
let bool3 = Boolean("hello");   // true
```

NaN

not a number；通常用于用户的输入校验

```
// 1. 验证用户输入是否为有效数字
function validateUserInput(input) {
    let number = parseFloat(input);
    if (isNaN(number)) {
        return "请输入有效的数字";
    }
    return "输入有效";
}

// 2. 数学计算错误检查
function divideNumbers(a, b) {
    let result = a / b;
    if (isNaN(result)) {
        return "计算出错";
    }
    return result;
}
```

注意校验NaN不需要用==号，可以直接用isNaN

### Hoisting

function关键字定义的函数会被完整的提升，可以在定义前调用

```
hello(); // 输出: Hello, world!

function hello() {
    console.log("Hello, world!");
}
/*其实就相当于*/
function hello() { // 函数声明被提升
    console.log("Hello, world!");
}

hello(); // 调用
```

### 箭头函数

箭头函数的语法

**const 函数名 = (参数) => { 函数体 }**

```
const add = (a, b) => {
    return a + b;
};
console.log(add(2, 3)); // 输出: 5
```

**隐式返回**

如果函数体只有一行return语句，可以省略{}和return

```
const multiply = (a, b) => a * b;
console.log(multiply(3, 4)); // 输出: 12
```

**参数规则**

![](https://cdn.nlark.com/yuque/0/2025/png/34360919/1740129457676-5a38cb23-b7a6-4133-b9da-904012844f42.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->








**unchecked:**

避免solidity 0.8.0开始的编译器自动对合约做数学安全检查，消耗gas.(高频函数非常在意gas)

避免使用unchecked的场景：

1.  输入来自用户
    
2.  条件依赖外部合约
    
3.  没有显式边界检查
    
4.  金额/索引/balance操作
    

## 危险调用

### **abi.encodePacked()**

功能：把几段字符拼接在一起，不加分隔符

比较安全的方法，拼接固定字节

-   `uint256` 固定 32 字节
    
-   `address` 固定 20 字节
    
-   `bytes32` 固定 32 字节
    

abi.encodePacked+动态类型 +hash/授权/校验 =！！！风险

### Ambiguous evaluation order

```jsx
function foo() public pure returns(uint){
uint x=5;
return x*x++;//could be 25 or 30
}
```

vulnerability mitigation:storing intermidiate results in temporary variables.

### Approval Vulnerabilities

**无限审批漏洞（权限失控）**

```jsx
approve(spender,type(uint256).max)
```

特点：授权额度无限、授权时间永久、不绑定具体交易\\业务条件

攻击路径：

1、用户授权无限审批（合约A）

2、合约A：

-   被黑
    
-   有后门
    
-   owner作恶
    

3、攻击者获得合约控制权

4、调用

```jsx
token.transferFrom(
	user,
	attacker,
	token.balanceOf(user)
	);
```

**审批抢先**

攻击者在受害者修改授权额度之前，提前花完原有的授权额度。（ERC20）

例子：

1.  通过此`approve x`功能，用户允许智能合约系统转移
    
    其 ERC20 代币。
    
2.  后来，他们选择修改授权金额，`approve` y 并因此发送了另一份请求。
    
3.  与此同时，在用户提供的交易被包含之前，攻击者启动了从用户钱包中`transferFrom x`删除代币的功能。
    
4.  如果攻击者的交易先被处理，然后用户的新`approve y`交易才被处理，那么恶意行为者就可以转移额外的代币。
    
5.  未经授权的转账总额将达到`x+y`代币级别。
    

```jsx
//allowances状态变量
mapping(address=>mapping(address=>uint256) private_allowances;
_allowances[owner][spender]=授权额度还剩下多少 
```

```jsx
//ERC20授权模型
approve(spender,value);//设置授权额度
transferFrom(owner,to,x);//在额度内转账
```

**Recommended Best Practices for Approval and Allowance Management**

针对 **ERC-20** 授权更新可能引发的抢跑风险，一个直接的缓解措施是优化 allowance 管理方式。相比通过approve以覆盖式写入新授权值，更推荐采用**增量调整**的模式，例如使用 OpenZeppelin SafeERC20提供的 safeIncreaseAllowance 与 safeDecreaseAllowance 来按差值更新授权额度，从而降低在“修改授权额度”场景下出现竞态条件的概率。

另一方面，“approval phishing（审批钓鱼）”虽不属于合约代码层面的漏洞，但在前端遭受内容注入或被篡改时，攻击者可将授权目标从预期合约地址替换为其控制地址，诱导用户完成恶意授权。为降低风险，开发者应遵循最小权限原则，仅授权业务所需的最小额度，并在必要时采用更严格的交互流程（例如两步操作、限额或延时机制）以减少单次恶意行为的影响；用户则应养成定期检查与撤销不再使用授权的习惯，可借助 [revoke.cash](http://revoke.cash) 等工具进行授权清理。

# Remix

### contract按键显示规则

**变量**

Public状态变量，自动形成蓝色按钮变成getter

**函数**

View/Pure函数，自动变成蓝色按钮，只读

External/Public函数，橙色按钮（会改变状态）

Payable函数，红色按钮（可以接受ETH）

![image.png](attachment:5a65c35f-3909-407a-9257-37db4007463b:image.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->









![image.png](attachment:5211f265-6361-475e-ae51-59654d33eee4:image.png)

![image.png](attachment:5c11b668-535e-4415-b6b6-8f19c1688dd6:image.png)

![image.png](attachment:324ff638-f490-47ae-9054-3bf3a6d3c66d:image.png)

## grammar

**四种变量可见性函数**

| 可见性 | 允许调用位置 | 常见用途 |
| --- | --- | --- |
| public | 合约内部+外部+子合约 | 默认接口+对外方法 |
| external | 外部调用 only | 对外API，节省gas |
| internal | 合约内部+自合约 | 内部逻辑 |
| private | 仅当前合约 | 封装细节 |

**modifier**:修饰符，给函数创建前置条件

```jsx
modifier onlyOwner(){
require(msg.sender == owner, "not owner");
_;//占位符，若符合条件在这里执行函数
}
```

**constructor:**构造函数，用于初始化设置（合约部署时自动执行一次）

```jsx
constructor(){
	owner=msg.sender;
	}
```

**receive()**:收钱函数;

```jsx
receive() external payable{} //专门从外部接受Eth
```

**calldata:** 外部调用的不可修改的函数参数；（省gas)

```jsx
function transferMany(address[] calldata users)//通常是external的函数参数，或者是复杂大的参数
```

**keccak256:**任意长度的bytes类型，变成32位的hash串

```jsx
bytes32 h = keccak256(data)
```

**require**(条件，“错误信息”）；

if true —→ 继续执行后续代码

if false —→ 立即停止，报错并且回滚所有操作 ❌

**emit:用来出发event的关键字，往区块链日志里写一条事件记录。**

```jsx
event Transfer(address from, address to, uint256 value);
function send(address to, uint256 amount) public{
balances[msg.sender]-=amount;
balances[to]+=amount;

emit Transfer(msg.sender,to,amount);
}
```

设计目的：

1、给前端/dApp用

2、给链下系统用 后端可以监听链上行为

3、比存storage便宜很多（省gas)

**call: low-level call**

```jsx
(bool ok,)=users[i].call{value:reward)(" ");//给用户地址i转reward wei数量的ehter 并且读取是否成功数据
address.call(bytes) //发起一次底层call,返回true或者false的数据

（bool ok,) = users[i].call{value: reward}(" ");
require(ok,"ETH transfer failed");
```

## 危险调用

**abi.encodePacked()**

功能：把几段字符拼接在一起，不加分隔符

比较安全的方法，拼接固定字节

-   `uint256` 固定 32 字节
    
-   `address` 固定 20 字节
    
-   `bytes32` 固定 32 字节
    

abi.encodePacked+动态类型 +hash/授权/校验 =！！！风险

## 英语单词

revert:回滚
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
