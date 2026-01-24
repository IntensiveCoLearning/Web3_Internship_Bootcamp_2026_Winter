---
timezone: UTC+8
---

# ShawnX-F

**GitHub ID:** ShawnX-F

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->
1.  学习了Reactive Contracts的基础内容
    
2.  补充了一下预言机的基本知识
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

# 有关ERC-7962的思考

最近在训练营学习了 ERC-7962，越琢磨越有意思。结合我的理解，浅谈一下对ERC-7962的定位、严格模式和批量传输的粗浅看法。

首先，定位问题。 如果你把它看作 ERC-20 的替代品，那它确实面临很多困境。但如果看作“补充”，一切就很顺了。 现在的 ERC-20 就像我们的银行账户，透明、可查，适合日常大额流转；而 ERC-7962 更像是一次性的游戏点卡或者现金，主打“阅后即焚”。

基于这个定位，关于“严格模式”的争论其实很好选。 社区里有人想妥协，允许 KeyHash 复用，我觉得这有点顾此失彼。如果我们为了好用而牺牲掉“一次性”这个ERC-7962的核心理念，那它就变成了一个既没有 ERC-20 方便，又没有做到极致隐私的“四不像”。 既然是补充协议，就应该剑走偏锋，而不应本末倒置。

其次，被低估的“批量传输”。 在 UTXO 模型下，Batch Transfer 绝对不应该是可选项，而必须是标配。 除了省 Gas，更重要的是获客体验。想象一下这个组合拳：Layer 2 的低成本环境 + Paymaster 代付 + ERC-7962 的原生批量传输。这意味着项目方可以极其丝滑地向成千上万个临时账户分发资产。这种“无感且隐私”的体验，很可能是 L2 爆发的关键。

最后，别忘了 AI。 未来的链上交互，大头可能都是 AI Agent。机器不在乎助记词好不好记，它们只在乎结算效率和逻辑闭环。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


# Polymarket架构理解

Polymarket是一个基于区块链的预测市场平台，其核心数据模型围绕预测事件的结构化设计，旨在实现链上透明、可验证的预测交易。

### **1\. 事件（Event）与市场（Market）**

事件类似于一个“预测主题”或“话题”，比如“2024年美国大选结果”或“美联储利率调整”。市场是事件下的具体预测单元，通常是二元问题（如“Yes/No”），一个事件可以包含一个或多个互斥或相关的市场，目的是让用户对同一主题下的不同角度进行预测。

当一个事件包含多个互斥市场时，Polymarket引入了NegativeRisk契约，简单来说就是一个市场的NO可以转化为其他市场的YES。例如，美国大选五位候选人，A获胜（YES）和其他人未获胜（NO）是等价的。

### **2\. 条件 (Condition)、问题 (Question)、集合 (Collection) 与头寸 (Position/TokenId)**

条件是市场的链上“身份证”，通过哈希计算生成唯一的conditionId。

```
conditionId = keccak256(oracle, questionId, outcomeSlotCount)
```

-   `oracle`是预言机合约地址（Polymarket 目前使用 UMA Optimistic Oracle 预言机）。
    
-   `questionId`是问题的标识符（通常由问题内容等信息哈希得到，或UMA Oracle的辅助数据哈希）。
    
-   `outcomeSlotCount`是结果选项数量。对于二元市场，该值为2。
    

条件的作用是绑定市场问题与外部结果来源，是市场从创建到结算的链上锚点。

头寸指的是用户持有的某市场某结果的份额（又称 Outcome Share），表现为ERC-1155代币的形式。每种结果对应一个不同的 TokenId，用于区分 YES 和 NO 两种头寸。

在条件代币框架中，中间引入了集合（CollectionID）的概念，用于表示特定条件下某个结果集合。计算方法为：

```
collectionId = keccak256(parentCollectionId, conditionId, indexSet)
```

-   `parentCollectionId` 对于独立的条件通常为 `bytes32(0)`（Polymarket 所有市场都是独立条件，没有嵌套条件，因此 `parentCollectionId` 一律为 0）。
    
-   `indexSet` 是一个二进制位掩码，表示选取哪些结果槽位。对于二元市场，有两个可能的 indexSet：
    
    -   YES 头寸的 `indexSet = 1` (`0b01`，表示选取第一个结果槽)。
        
    -   NO 头寸的 `indexSet = 2` (`0b10`，表示选取第二个结果槽)。
        

集合本质上是条件的结果“分支”，用于区分不同结局（如胜出或失败）。

总结来说：一个Event可以包含多个Market，形成主题下的多维度预测；每个Market对应一个独立的Condition，作为其链上出生证明；Condition生成多个Collection（通常YES和NO两个），代表结果分支；每个Collection结合抵押品（USDC）生成对应的TokenId，即实际的Position头寸。

在NegativeRisk多市场事件中，关系更紧密：一个市场的NO头寸可以通过适配器合约转换为其他市场的YES头寸，实现跨市场流动性共享，避免资金碎片化。 整体上，这些实体从抽象（Event）到具体（Position），层层哈希计算，确保链上唯一性和可追溯性，形成一个高效的预测生态。

### **3\. Polymarket 的市场从创建到结算，关键的链上步骤和日志事件**

-   **市场创建 (Creation) 阶段– 登记问题**
    

由市场创建者调用 `ConditionalTokens.prepareCondition` 创建条件。

关键日志：`ConditionPreparation` 事件，包含 `conditionId`、`oracle`、`questionId`、`outcomeSlotCount` 等信息。证明市场问题已被正式注册并绑定预言机。它奠定后续所有操作的基础，没有这个日志，市场就无法合法存在或结算。

-   **交易阶段（包括初始流动性提供与拆分）**
    

关键日志：

**PositionSplit**：发生在初始流动性注入时，将USDC拆分为YES和NO头寸代币。日志包含 `conditionId`、`collateralToken`（应为 USDC 地址）、`parentCollectionId`（一般为 0）、`partition`（拆分出的 indexSets 列表，如 `[1,2]`）、以及 `amount`（拆分抵押品数量），证明资金已被锁定并生成可交易代币。

**OrderFilled**：发生在买卖双方的订单在链上部分或全部成交时，都会触发该事件，记录每笔交易细节，包括 `maker` 和 `taker` 地址（做市（挂单）方和吃单方地址）、`makerAssetId` 和 `takerAssetId`成交时双方各自支付的资产 ID（0为USDC，非0为TokenId）、成交量`makerAmountFilled` 和 `takerAmountFilled`和费用。交易总是 USDC 与 Outcome Token 之间。

-   **结算阶段**
    

预言机将把结果提交回 CTF 合约，调用 `reportPayouts(conditionId, payouts[])` 来公布各结果的兑付率。

关键日志：调用 `reportPayouts` 本身通常不会有特殊事件（或有 `ConditionResolution` 事件），但其效果是将相应 `conditionId` 下的头寸标记为可赎回。

从 `ConditionPreparation` 证明市场的存在和参数、`PositionSplit` 证明资金注入和代币铸造、`OrderFilled` 记录交易交换细节、直到 `reportPayouts` 确认结果以供赎回。这些事件串联起来，可以让我们基于链上数据重建出市场发生的一切。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



// 基础合约  
contract Animal {  
string public name;  
  
constructor(string memory \_name) {  
name = \_name;  
}  
  
function speak() public virtual returns(string memory) {  
return "Some sound";  
}  
}  
  
// 继承合约  
contract Dog is Animal {  
constructor(string memory \_name) Animal(\_name) {}  
  
// 重写父类函数  
function speak() public pure override returns(string memory) {  
return "Woof!";  
}  
}  
  
// 多重继承  
contract Pet is Animal {  
address public owner;  
  
constructor(string memory \_name, address \_owner) Animal(\_name) {  
owner = \_owner;  
}  
}  
  
contract Labrador is Dog, Pet {  
constructor(string memory \_name, address \_owner)  
Dog(\_name)  
Pet(\_name, \_owner) {}  
}

# 继承与函数重写**（Inheritance and Override）**

这段代码是一个 Solidity 继承示例，涵盖了**单继承、多重继承、构造函数传参**以及**函数重写**的核心逻辑。

### 1\. 基础合约 `Animal`：定义规范

这是继承树的顶端（基类）。

-   **状态变量** `name`：使用了 `public` 修饰符，Solidity 会自动为其生成一个同名的 getter 函数。
    
-   **构造函数** `constructor`：要求在部署时必须传入 `_name`。
    
-   `virtual` **关键字**：这是关键。在 Solidity 中，只有标记为 `virtual` 的函数才能在子类中被重写（Override）。
    

## 2\. 单继承 `Dog`：逻辑覆盖

`contract Dog is Animal` 表示 `Dog` 获取了 `Animal` 的所有能力。

-   **构造函数初始化**：`Animal(_name)`。因为父类 `Animal` 的构造函数带参数，子类必须显式调用它来完成初始化。
    
-   **函数重写** `override`：
    
    -   子类函数必须标记为 `override` 才能修改父类的 `virtual` 函数。
        
    -   **注意点**：父类 `speak` 没有 `pure` 修饰符（默认是会读取/写入状态的，虽然示例里没写），但子类改成了 `pure`。Solidity 允许这种“权限收紧”的操作（从读取状态变为不读取状态是安全的）。
        

## 3\. 多重继承与构造函数：`Pet` 与 `Labrador`

这里是代码最复杂的部分，涉及到了多重继承。

`Pet` 合约同样继承自 `Animal`，并引入了一个新的状态变量 `owner`。

`Labrador` 合约`contract Labrador is Dog, Pet`

1.  **继承顺序**：在 Solidity 中，继承顺序遵循 **“由基类到派生类”**。虽然这里 `Dog` 和 `Pet` 都继承自 `Animal`，但在 `Labrador` 的声明中，顺序会影响 `super` 的调用逻辑。虽然`Labrador` 的构造函数里没写`super` ，但其后的`Dog(_name) Pet(_name, _owner)` 实际上就是 `super` 机制在构造函数中的体现。
    
2.  **构造函数传参**：`Labrador` 同时初始化了两个父类的构造函数。有趣的是，由于 `Dog` 和 `Pet` 都依赖 `Animal`，`Animal` **的构造函数实际上只会被执行一次**。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




1.  RPC详解
    

全称 Remote Procedure Call（远程过程调用），连接前端应用与区块链网络的关键桥梁。RPC是一种通信协议，允许应用程序通过网络调用远程服务器上的函数或方法。在区块链开发中，RPC 节点是运行区块链客户端软件的服务器，它们维护完整的区块链数据副本，并提供 API 接口供开发者查询链上数据、发送交易等操作。在 Dapp 开发中，RPC 节点承担着以下关键职责：

-   读取链上数据
    

查询账户余额、交易历史，读取智能合约的状态变量，获取区块信息、Gas 价格等。

-   发送交易
    

将用户签名的交易广播到区块链网络，查询交易状态和确认数，估算交易所需的 Gas 费用。

-   事件监听
    

监听智能合约事件（Events），实时获取链上状态变化，支持 WebSocket 长连接推送。

-   网络管理
    

切换不同的区块链网络（主网、测试网），获取网络信息和链 ID，管理节点连接状态。

2.  合约结构详解
    

一个智能合约的基本结构通常由三部分组成：状态变量、构造函数和普通函数。

状态变量（State Variables）是指在合约中定义的、其值**永久**存储在区块链上的变量。它们用于记录合约的持久化数据，构成了合约的整体状态。当合约被**部署后**，这些变量将**被写入区块链**，并在合约的整个生命周期中保持可访问性和可追踪性。

函数（Functions）是智能合约中执行具体逻辑操作的核心组成部分。通过函数，可以实现对状态变量的读取、修改，或执行特定业务逻辑。

Solidity 函数的标准声明格式如下：

 function <函数名>(<参数列表>)  
<可见性>  
<状态可变性>  
<修饰符列表>  
<虚拟/重写关键字>  
returns (<返回值列表>)  
{  
// 函数体  
}

-   函数可见性（Function Visibility）决定了函数在何种上下文可以被调用。
    

| 修饰符 | 可见范围 | 描述 | 使用场景 |
| public | 内部 + 外部 | 任何地方都可以调用 | 对外提供的公共接口 |
| external | 仅外部 | 只能从合约外部调用 | 外部用户接口，gas 效率更高 |
| internal | 内部 + 继承 | 当前合约和子合约可调用 | 内部逻辑函数，需要被继承 |
| private | 仅内部 | 只有当前合约可调用 | 私有实现细节 |

-   函数状态修饰符（State Mutability Modifiers）用于指明函数是否修改或读取合约状态：
    

pure不读不改，view可读不可改，payable及无修饰符可读可改

```
contract StateModifiers {
    uint256 public count = 0;

    // view: 只读函数，不修改状态
    function getCount() public view returns(uint256) {
        return count;
    }

    // pure: 纯函数，不读取也不修改状态
    function add(uint256 a, uint256 b) public pure returns(uint256) {
        return a + b;
    }

    // payable: 可接收以太币
    function deposit() public payable {
        // msg.value 是发送的以太币数量
    }

    // 默认：可修改状态
    function increment() public {
        count++;
    }
}
```

Solidity 支持多参数与多返回值，以及命名返回值：

多个返回值（getPersonInfo）、命名返回值（calculate）、解构赋值与忽略返回值（callExample）

```
 // 多个返回值
function getPersonInfo() public pure returns(string memory name, uint256 age) {
    name = "Alice";
    age = 25;
}

// 命名返回值
function calculate(uint256 a, uint256 b) public pure returns(uint256 sum, uint256 product) {
    sum = a + b;
    product = a * b;
    // 自动返回命名变量
}

// 调用带多返回值的函数
function callExample() public pure {
    (string memory name, uint256 age) = getPersonInfo();
    // 或者忽略某些返回值
    (, uint256 productOnly) = calculate(5, 3);
}
```

修饰符（Function Modifiers）允许在函数执行前插入额外逻辑，常用于权限控制与前置检查：

核心机制占位符\_，它告诉编译器“现在去执行被修饰的函数体”，如果条件不满足（如require失败），代码会直接报错回滚，\_;之后的内容包括函数体不会被执行。

-   onlyOwner（权限闸门）确保只有合约的部署者可以调用某些功能，应用于保护提现、修改重要参数等敏感操作；
    
-   whenNotPaused（开关控制器）实现“熔断机制”，如果发现合约有漏洞或受到攻击，管理员可以一键关停重要功能；
    
-   togglePaused（修改状态）
    

function togglePause() public onlyOwner {  
paused = !paused;  
}  
这里使用了onlyOwner，意味着只有管理员可以切换Paused的状态；

-   criticalFunction（多重叠加）
    

function criticalFunction() public onlyOwner whenNotPaused {  
// 逻辑  
}  
从左到右顺序执行修饰符。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->





# 1\. 合约部署的成本核算

### Gas 消耗量

-   基础交易成本（Base transaction cost），任意一笔交易都有的固定成本 21，000 Gas；
    
-   合约创建开销（CREATE 成本），使用CREATE 指令创建新合约，约需32，000 Gas；
    
-   字节码存储成本（Code deposit），部署时 runtime bytecode 写入链上代码存储，约 200 Gas/一字节 runtime 代码；
    
-   构造函数执行（Constructor 执行逻辑），constructor（...）里做的所有运算、条件判断、事件等，都会按实际执行的指令消耗 Gas。其中，复杂计算、for循环、创建数组等，Gas 会直线上升；
    
-   初始存储写入（Storage 初始化），部署时第一次把某个存储槽从0→非0，每个槽大约 20，000 Gas，如：构造函数里把很多mapping、数组、配置参数一次性写入storage。
    

总结：代码越长、构造逻辑越复杂、初始化写入的状态越多，Gas Used 越高。通过**减少字节码体积 + 减少部署时的 storage 写入 + 精简 constructor**，降低 Gas Used。

### 2\. 合约逻辑的修改与升级模式

因智能合约部署后，代码无法直接修改，故只可通过“部署新的东西 + 通过某种方式把‘流量/状态’指过去”的逻辑实现在函数中

A. 重新部署新合约——只适用无状态/状态少/状态可离线迁移的小合约

做法：写好新合约，部署新地址→前端/ SDK /其他合约改成指向新地址→通过脚本把旧合约的重要数据（余额、配置等）迁移到新合约（往往需要用户参与或批处理脚本）

B. 代理模式（Proxy Pattern）——行业主流

核心思想：把”地址 + 存储“锁死，把”逻辑“换着用。

a. 基本结构

Proxy（代理合约）——**存档案 + 收客人**：

地址固定：用户只和它交互；存储状态：所有状态变量都写在这里；内部只做一件事：把delegatecall转发到当前“实现合约”。

Implementation/Logic合约——**外包业务团队**：

里面是真正的业务代码；不直接被用户调用（正常情况下不会对外暴露接口）；被认为是可丢弃的——升级就是换一个新实现。

b. 主流形态（两种形态都遵循ERC-1967里定义的固定存储槽，避免和业务数据冲突）

-   Transparent Proxy Pattern（OpenZeppelin TransparentUpgradeable Proxy 使用这一套）：
    

代理合约负责转发，管理员有单独的管理入口，用户调用时不会意外触发管理函数。

-   UUPS（Universal Upgradeable Proxy Standard，基于 EIP-1822 + ERC-1967）：
    

升级逻辑写在现实合约内（upgradeTo/upgradeToAndCall）；Proxy本身很薄，只保存实现地址和必要元数据；更节省 Gas，也很灵活，目前更受偏爱。

c. 升级过程

-   部署 v1 实现合约（LogicV1）→部署Proxy，指向 LogicV1，初始化状态→运行后若发现需要升级：
    
-   部署 LogicV2（保证存储布局兼容）；
    
-   由 Admin（往往是多签 + Timelock）调用升级函数，把实现地址改成LogicV2；用户继续用原来的 Proxy 地址交互，但底层逻辑已经是V2了。
    

_实战 Tips：_

直接用OpenZeppelin Upgrades + Hardhat/Foundry 插件（不要手写proxy），可以检查存储布局是否兼容和限制构造函数用法，改用initializer，且升级权限一定要挂在多签（Gnosis Safe等），再套一层 Timelock（例如24h/48h），大协议通常会配合治理投票一起使用，避免项目方临时改逻辑。

C. 数据与逻辑分离/模块化（State-Logic Separation）

EIP-2535 Diamond Standard：

允许一个“钻石合约”挂接多个逻辑Facet，通过函数选择器路由到不同模块，做出超大升级系统，常见于复杂 DeFi/NFT/GameFi 协议。

D. 可插拔模块（Pluggable Modules）

主合约保存一个“模块列表”，不同功能拆到不同模块合约内，通过注册/反注册方式做“插件化升级”。

_Tips：_

-   一般会配合接口白名单 + 角色权限，否则“模块热插拔”也可能成为攻击入口；
    
-   复杂协议常把“风控逻辑”、“清算逻辑”、“费率模型”做成独立模块，以实现在线调参（更换利率模型模块）和按市场环境更换风控（如上线新的抵押品策略）。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->






**智能合约编译产物**

1.字节码Bytecode

定义：智能合约的核心，是EVM可以理解和执行的低级机器码。

solidity编译器实际会生成两段主要字节码：

创建字节码Creation Bytecode——包含构造函数逻辑，用于部署时执行，最终负责把运行代码写入链上；

运行时字节码Runtime Bytecode——部署完成后真正储存在合约地址上的代码，后续调用合约时执行。

2.应用二进制接口Application Binary Interface（ABI）

定义：JSON（Javascript Object Notion）格式文件，描述智能合约的公共接口，包含合约中的所有公共函数和事件的详细信息——名称、参数类型、返回值类型以及可见性（public、external）。

作用是外部交互和数据编码/解码。

3.合约元数据Contract Metadata

定义：JSON文件，包含合约的额外信息——编译器版本和设置、源代码路径或IPFS/Swarm等去中心化存储的引用、ABI、NatSpec文档（如果写了）。

solidity默认生成，并默认把它的IPFS哈希通过CBOR编码附加在运行时代码的尾部。

作用：有助于验证以部署合约的来源和编译过程，增加透明度；区块链浏览器也依赖metadata来做源码验证和自动生成交互界面。

4.源映射/Source Map/Storage Layout/ AST（抽象语法树）Abstract Syntax Tree

AST定义：源代码的结构化表示，编译器在编译过程中生成，用于分析和优化代码。

作用：主要用于编译器内部工作，或者在开发工具中进行静态分析、代码审计等高级用途。solidity编译器支持通过StandardJSON接口输出AST，静态分析工具（如Slither等）利用它来定位漏洞。

源映射定义：编译器生成两类源映射——AST节点对应的源码区间和字节码指令—源代码区间的映射。

作用：用于调试与审计

5.Assembly（EVM Assembly）与Yul IR

EVM Assembly定义：比字节码高级一点的中间表示（仍然是低级代码），包含EVM 操作码（opcode）和操作数，更具可读性。

作用：分析Assembly代码来深入理解合约执行逻辑、优化Gas消耗和进行安全审计。solidity编译器通过—asm等选项输出这部分内容。

Yul IR定义：Yul是solidity官方提供的中间语言，作为“IR—based codegen”的核心，用于在后端（EVM、eWASM等）之间共享优化逻辑。

作用：

-   编译器在内部会先把solidity源代码翻译成Yul，再从Yul生成最终字节码；
    
-   对Yul做优化可以同时优化所有后端目标；
    
-   可以手动写Yul（或通过inline assembly）来做极端优化场景。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->







# Uniswap

### 1\. 工作原理

基于恒定乘积公式，通过智能合约系统实现。每个uniswap智能合约管理着一个流动性池，该池由两个ERC-20代币的储备组成。

2\. 恒定乘积公式

x\*y=k，x和y为池子中的两个ERC-20的储备金额，k为初始池子x、y的乘积，为不变值。但实际上，uniswap对每笔交易都会收取0.30%的手续费，并添加到储备金k中，这相当于向流动性提供者（LP）支付收益，当LP销毁自己的池代币以提取其应得的储备金份额时，即可获得收益。

### 3\. 流动性提供者（LP）

LP把两种ERC20代币按当前市场比例一起存入pool，获得LP Token（也叫流动性代币或LP份额），帮别人提供“可交易的深度”，别人来swap时，用pool中的币完成交易，并支付0.30%的手续费，按比例分给所有LP。

-   LP Token的获得：
    

创建池子时，LP Token=√（x\*y）（x，y为LP提供的代币）；增加流动性：LP Token=添加的流动性价值/当前池子的总流动性价值\*当前LP Token总供应量。注，添加时，两种代币的比例应以当前市场比例为准。LP Token就像池子的股票，是ERC20标准，可转账、抵押、质押、出售。

-   LP Token的销毁：
    

把LP Token发回给Pair合约（调用burn函数），系统按比例返回对应数量的两种代币+累积的手续费，LP Token被销毁，总供应量减少。

-   LP的收益及无常损失：
    

LP收益=交易量\*0.30%\*份额比例 - 无常损失

无常损失（Impermanent Loss）≈2\* √（r/（r+1））－1,其中r =新价格/旧价格

总结来说，当交易量大且提取时波动小，手续费>无常损失，则赚钱；当交易量小且提取时单边波动大，手续费<无常损失，则亏钱。结合套利动机，价格来回震荡且最终提取时比例相差不大时，最赚钱。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->








# 智能合约

### 1\. 形式

代码+持久状态：代码functions+数据state的组合，驻留在以太坊的地址上，由交易触发后执行。智能合约可以部署在L1或L2上，是**链上的基本执行单元**。

### 2\. Denkun升级

升级前，任何合约在任何时间调用SELFDESTRUST（opcode 0xff）都可以：

-   把合约全部余额转给指定地址
    
-   删除合约的代码（code）
    
-   删除合约的全部存储（storage）
    
-   把 nonce 重置为 0
    
-   获得gas refund
    

升级后，**只有在“创建合约的同一笔交易中”调用时才会真正移除代码**。换言之，只能在诞生之时才可以自毁。

| 场景 | 是否同一交易内创建 | SELFDESTRUCT效果（Dencun后） | 备注 |
| 立即 SELFDESTRUCT | 是 | 完整自毁：转余额 + 删除 code + 删除 storage + nonce 重置为0 | 行为不变，常用于临时合约、create2 模式 |
| 已部署的旧合约后续调用 SELFDESTRUCT | 否 | 只转余额（相当于 SENDALL），不删除 code、不删除 storage、不改 nonce | 最常见的变化 |
| 向自己 SELFDESTRUCT（target = self） | 无论是否同一 tx | 余额被销毁（burn），但 code 和 storage 保留（如果非同一 tx） | 之前也可能 burn，现在明确 |

###  3. 智能合约特性

-   可预测性：相同输入，相同输出
    
-   公开记录和可审计性：执行过程和状态变更，都会记录在链上，包括合约部署调用、交易、事件日志、资产转移等。所以DeFi、NFT市场分析、合规审计等领域，会在合约公开数据上做指数和风控模型。
    
-   “隐私”与假名制：无需暴露实名就可以参与网络，但链上留下的一切行为都是公开可追踪的，隐私需要额外设计，如使用专门的隐私协议（如混币、隐私池、零知识证明等）或采取更强的隐私设计。
    
-   条款可见与可审查：合约源码开源，工具（如Slither、Mythril、Foundry + formal verification）框架可直自动化发现一些安全问题，大型协议通常会公布多轮审计报告。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->









# 国内相关法律最新研究

1.  2026年1月1日施行修改后的《民事案件案由规定》，专门增加了第一级案由“数据、网络虚拟财产纠纷”，并且根据金杜律师事务所的调研结果——最高法研究室发表的署名文章“《民事案件案由规定》（2025年）的理解与适用”进一步明确了将虚拟货币、数字藏品（NFT）与网络游戏装备一同纳入网络虚拟财产的范畴。这意味着，若遇到加密货币相关的民事争议，不必再面对“案由不对，无法立案”的尴尬窘境。
    
2.  此外，2026年1月8日上海二中院与中国人大法学院联合主办的刑事判例研讨会上，围绕“涉虚拟货币犯罪案件的适法统一”问题，司法机关释放了以下信号：如果行为不具备经营行为特征，仅属于个人持币、炒币，一般不认定为非法经营罪。此次表态可以在“精准打击危害金融犯罪行为和避免普通个人投资行为被过度刑事化”上，做出了重要的指示。
    
3.  关于虚拟货币买卖，我认为使用人民币购买虚拟货币’时，如遇到支付人民币，而对方不支付相应虚拟货币的情况（非价格未谈拢而是诈骗情形），应积极采取措施维护自己的合法权利（如报警等），并不是所有的涉虚拟货币的行为都不受法律保护。
    

所以，尽管目前国内对虚拟货币的监管还处于高压态势，但我们web3从业者和相关人员，应该坚守自己的良知，做到“识别恶、不作恶、不助恶”，根据目前释放的信号，我们的合法权益在时间的见证下，也会得到更好的保障。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->










6\. 账户类型与结构

-   EOA = 由“私钥”直接控制的钱包账户，能发交易、收资金、调合约，但自己\_不带代码逻辑\_。控制私钥的人 = 控制这个账户的一切。
    

主要风险：私钥/助记词丢失或泄露。

_注：_

a. nonce（交易计数器）对于EOA来说，表示本账户已经发生过几笔交易。一笔交易的nonce必须等于账户的nonce，打包成功后nonce+1，同一个nonce只允许执行一次，用以**防止重放攻击&保证交易有序**。

b. **一个交易在内存池还没被打包前，使用相同nonce+更高的Gas Fee可以替换掉旧交易**。收款地址填自己，金额可0或极小值，可以“取消”原交易；收款地址不变，金额改变，可以“加速”原交易。目前很多钱包都有内置的“Speed up”和“Cancel”功能，可以一键替换。

-   合约账户
    

可以持有ETH/token，内部存code（不可改）&storage（随调用改动），在执行中给其他地址转账、调用其他合约地址、使用CREATE/CREATE2创建新合约。

CREATE：地址 = f(sender, nonce) → 不可预测、不可重用

CREATE2：地址 = f(sender, salt, bytecode) → 可预测、可跨链统一、可重部署（存在风险）

主要风险：代码漏洞/逻辑错误/权限设计不当

-   ERC20/ERC721代币与合约账户的关系
    

地址中显示的“拥有”某个ERC20/ERC721代币，并不是地址里有代币，而是**对应的代币合约的storage里记录了某个地址拥有多少代币或者某个tokenID的owner是谁。**
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->












（书接上文）

4\. 以太坊的去中心化实现机制

1) 共识与出块（技术/经济层）：采用PoS共识，任何人只要抵押在32ETH及以上时，即可成为验证者（validator），参与区块提议和投票。每个slot，区块会在随机选择的提议者（proposer）中产出，同时数百验证者在同一时隙对链头进行投票（attestation），通过Gasper共识（Gasper-FFG提供终局性+LMD-GHOST提供链头选择）完成安全性与活性。

a. Slot是以太坊信标链（Beacon Chain / Consensus Layer）的时间基本单位，整个网络的时间被均匀切割成一个个 12秒的 slot。

b. Gasper = Casper-FFG 套在 LMD-GHOST 上面

-   日常运行时：验证者先用 LMD-GHOST 选链头 → 提议新块 → 其他人用 LMD-GHOST 投票支持。
    
-   同时，所有 attestation 里还携带 Casper-FFG 的投票（针对两个 checkpoint 之间）。
    
-   Casper-FFG 周期性地看这些投票，决定哪些块可以 justified/finalized。
    
-   结果：链快速生长（LMD-GHOST 保证活性） + 历史块被逐步锁定（Casper-FFG 保证安全性）。
    

| 组件 | 全称 | 主要职责 | 提供的主要属性 | 典型表现形式 |
| Casper-FFG | Casper Friendly Finality Gadget | 提供“终局性”（finality） | 安全性（Safety） | 把某些 checkpoint 升级为 finalized |
| LMD-GHOST | Latest Message Driven - GHOST | 提供“链头选择”（fork-choice） | 活性（Liveness） | 决定当前链头（head），链持续生长 |

2) 节点与网络（技术层）

以太坊执行层使用DevP2P协议维护点对点网络：

-   发现层（Discovery stack）基于UDP，用来发现和加入新节点；
    
-   DevP2P通信层基于TCP，负责在已连接节点间传递区块、交易等实际数据。
    

3) 以太坊的客户端多样性（实现层）

-   执行层客户端EL（Execution Client）：Geth、Nethermind、Besu、Erigon、Reth 等；
    
-   共识层客户端CL（Consensus Client）：Lighthouse、Prysm、Teku、Nimbus、Lodestar 等。
    

[clientdiversity.org](http://clientdiversity.org) 这类站点会定期占比过高的客户端，提醒质押者「换个少数派客户端」。目标是避免「一个客户端一旦出 bug，全网一起躺平」的单点风险。

4) MEV与PBS（市场结构层）

为缓解最大可提取价值MEV，引入了PBS（提议者-建构者分离）思路，主流的实现是协议外的MEV-boost中继系统：

可以理解为“区块拍卖中介”，从 Builder 收集高 MEV 区块，挑出给验证者报酬最高的那个 header，让验证者签名后广播完整区块，从而让所有验证者（不管大小）都能分到 MEV 大蛋糕，但引入了中心化与信任风险，未来会被协议ePBS 取代。

5) Rollup中心路线与数据去中心化（扩容/职责分离）

L1：更小、更稳，负责安全、数据共识；L2：执行负载、创新

6) 治理（社会层）

“客户端开发团队实现 + 节点运营者选择是否升级软件”共同决定是否升级。

5\. 网络结果与节点类型

1) 客户端类型：

-   干活的EL：监听交易与区块、在EVM中执行交易和智能合约、维护当前链的状态数据库（账户余额、合约储存等）、提供JSON-RPC接口（eth\_\*)、通过自己的P2P网络转发交易和区块头/区块体；
    
-   决策的CL：维护信标链视图和fork choice规则（LMD-GHOST等），每个slot中做出选择（区块提议）、并对区块做出投票，跟踪共识状态（验证者集合、质押余额、惩罚等），根据EL提供的结果，决定上链的区块；
    
-   陪审的Validator：管理验证者密钥，提议新区块，（区块）投票。用于质押、参与出块并获得奖励。
    

以太坊节点=EL+CL+Validator（验证者节点可选），CL需要Engine API来向EL发号施令。行客户端（EVM + JSON-RPC）——调用 eth\_\* / 部署合约 / 查状态执；共识客户端 + 验证者客户端——质押 32 ETH 做验证者 / 关心 attestation、finality、slashing。

2) 节点间的连接与通信方式

先找到人 → 再拉上加密专线 → 最后靠八卦（gossip）把消息传满全网。（Kademlia风格的DHT发现机制）

A. 节点发现（UDP+Kademlia）

a. 新节点给引导节点ping，引导节点回pong，确认引导节点在线；

b. 发送Findnode，请求，对方回复Neighbors（discv4）或者Nodes（discv5），里面是其他活跃节点的联系方式；

c. 对活跃节点重复以上操作，直到达到预设上限。

B. 建立安全连接（TCP+RLPx/devp2p）

a. TCP搭好稳定管道；

b. RLPx在一个TCP里多路复用多种协议：以太坊在 TCP 之上跑的是 devp2p 协议栈，其底层加密与多路复用由RLPx 负责。

C. Gossip+请求/响应传播“八卦”

Gossip用于传播新交易喝区块，请求/响应用于按需拉取缺失的历史区块、状态。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->













_Tips_：各位uu们可线下编辑好，再粘贴到打开界面；如果电脑网页端打卡界面编辑时，一定要注意，鼠标不要！点击！界面以外的位置！编辑中的内容不会自动保存，会消失！（心痛教训）

真的非常开心能参加本次的web3实习计划，之前虽也参加过一两期共学营，但一直觉得掌握的不到位，有很多根上的东西不太明白。希望通过本次学习，能藉由循序渐进的学习路径和查漏补缺，全面理解web3和以太坊。

**前期笔记以查漏补缺为主**

1\. Gas Fee

EIP-1559之前，Gas Fee = Gas Used \* Gas Price

EIP-1559之后，Gas Fee ≈ （Base Fee + Priority Fee）\*Gas Used

其中，钱包中maxFeePerGas/maxPriorityFeePerGas是用户愿意支付的上限，实际扣费不会超过这个上限。

2\. dApp核心技术特征（为什么要用以太坊跑应用的底层逻辑）

1) 去中心化

2) 确定性（智能合约）

3) 图灵完备以及

4) 隔离性（沙盒）：dApp的执行被隔离在EVM里，单个合约逻辑的失败只会导致其自身的调用失败（回退），而不会使整个以太坊网络宕机；全网节点只是共同重放并验证同一套字节码，把终极信任从人/机构转移到了“确定性计算”本身，这是区块链能做到“极致去信任”的最关键密码学社会学机制之一。

3\. dApp开发的缺点/代价

1) 维护困难：合约部署后，默认无法更改

2) 性能开销与扩展难度：追求性能和价格，优先考虑L2；但最终安全性上还是要落在L1

3) 网络拥堵和Gas费问题：即便有L2，但部分高价值操作，如大额清算和跨链桥结算等，还是会回到L1；开发者应通过减少无谓存储/循环和前端Gas提醒，必要时支持多条L2切换来提升用户体验。

4) 用户门槛高：还是基于web3自身的普及率问题，需要时间，精进和推广。

5) “再中心化”倾向：为了降低门槛，有些产品会有在服务端代管私钥，把前端放在CDN上和链下逻辑链上结果的操作。

（未完待续）
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
