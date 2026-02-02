---
timezone: UTC+8
---

# nounoupop77

**GitHub ID:** nounoupop77

**Telegram:** @nounoupo

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-02-02
<!-- DAILY_CHECKIN_2026-02-02_START -->
**1\. RC-1046 – 可扩展元数据的代币标准**

-   **核心定位**：**代币元数据的扩展标准**，支持动态更新代币的元数据（如名称、描述、图片），兼容 ERC-20/721/1155。
    
-   **解决痛点**：ERC-20/721 的元数据（如`tokenURI`）通常是固定的，无法动态修改；不同代币的元数据格式不统一。
    
-   **核心功能**：
    
    -   定义了标准化的元数据字段（名称、符号、图标、描述等）；
        
    -   支持元数据更新事件（`MetadataUpdate`），更新后自动通知钱包 / DApp；
        
    -   兼容现有代币标准，可直接集成。
        
-   **适用场景**：需要动态更新信息的代币，如 RWA 代币（实时更新资产价格）、会员代币（更新会员权益）。
    

2.  **ERC-6093 – 统一代币错误代码标准**
    

-   **核心定位**：**代币错误处理的标准化规范**，定义了通用的错误码和错误描述，兼容所有代币标准。
    
-   **解决痛点**：不同代币合约的错误提示五花八门（如 ERC-20 用`false`表示失败，ERC-721 用`revert("Owner query for nonexistent token")`），开发者调试和用户理解成本高。
    
-   **核心功能**：
    
    -   定义了 18 个通用错误码，如`TOKEN_NOT_EXISTS`（代币不存在）、`INSUFFICIENT_BALANCE`（余额不足）、`TRANSFER_TO_ZERO_ADDRESS`（转账到零地址）；
        
    -   要求合约用`revert`抛出标准化错误信息，而非返回`false`。
        
-   **适用场景**：所有代币合约的开发和调试，提升合约的可读性和兼容性。
    

3.  **ERC-1363 – 可支付型代币（转账 + 自动调用逻辑）**
    

-   **核心定位**：**专为支付场景设计的代币标准**，结合了 ERC-20、ERC-223、ERC-827 的优点，转账后自动触发接收方的业务逻辑。
    
-   **解决痛点**：ERC-20 转账和业务逻辑分离，需要多笔交易；ERC-223 兼容性差；ERC-827 功能复杂。
    
-   **核心功能**：
    
    -   支持`transferAndCall`/`approveAndCall`，转账同时调用接收方的`onTransferReceived`/`onApprovalReceived`回调；
        
    -   兼容 ERC-20 接口，无需修改现有钱包 / 交易所的集成逻辑；
        
    -   内置安全检查，防止恶意回调。
        
-   **适用场景**：去中心化支付、订阅服务、NFT Mint 支付（转账代币自动 Mint NFT）等场景。
    

4.  **ERC-3643（前身 T-REX）– 合规型代币标准（常用于 RWA）**
    

-   **核心定位**：**面向现实世界资产（RWA）的合规代币标准**，满足监管要求，支持 KYC/AML、权限管理、暂停交易等功能。
    
-   **解决痛点**：传统代币标准没有合规功能，现实世界资产（房地产、债券、股票）代币化后，无法满足各国监管的 KYC、反洗钱、限售等要求。
    
-   **核心功能**（合规核心特性）：
    
    -   **身份验证**：只有通过 KYC/AML 的用户才能持有 / 转账代币；
        
    -   **权限管理**：分管理员、发行方、用户等角色，支持冻结 / 解冻账户；
        
    -   **交易限制**：支持暂停交易、设置转账额度上限、限售期；
        
    -   **合规事件**：记录所有合规相关操作，便于监管审计。
        
-   **适用场景**：现实世界资产代币化（RWA），如房地产代币、债券代币、私募股权代币等。
<!-- DAILY_CHECKIN_2026-02-02_END -->

# 2026-02-01
<!-- DAILY_CHECKIN_2026-02-01_START -->

1.  ERC-165 – 接口检测标准
    

• 核心定位：辅助型标准，不是代币标准，但几乎所有主流代币标准（ERC-721/1155/223）都依赖它。

• 解决痛点：合约之间交互时，无法判断对方是否实现了自己需要的接口，容易调用失败。

• 核心功能：定义了一个标准函数，让合约可以声明自己支持的接口。

• 适用场景：所有需要跨合约交互的场景，是以太坊合约的「接口身份证」。

2.  ERC-223 – ERC-20 改进，防止代币误转丢失
    
    • 核心定位：ERC-20的升级版，解决ERC-20代币转合约丢失的问题。
    
    • 解决痛点：ERC-20的transfer函数没有回调机制，用户误将代币转到不支持接收的合约地址，代币会永久锁定。
    
    • 核心功能：
    
    ◦ 重写transfer函数，增加bytes data参数；
    
    ◦ 转账到合约地址时，会触发合约的tokenFallback回调函数；
    
    ◦ 若合约没有实现tokenFallback，转账会直接失败，避免代币丢失。
    
    • 适用场景：需要更高安全性的代币发行，替代ERC-20。与 ERC-20 不兼容，主流平台对于ERC-20更支持维护，由此普及度不如ERC-20。
    
3.  ERC-827 – ERC-20 扩展，支持授权调用第三方合约
    
    • 核心定位：ERC-20的功能扩展版，允许代币转账/授权的同时，调用第三方合约的函数。
    
    • 解决痛点：ERC-20需要「授权→调用合约」两步操作，ERC-827将其合并为一步，提升效率。
    
    • 核心功能：在ERC-20基础上新增3个函数：
    

function transferAndCall(address to, uint256 value, bytes calldata data) external returns (bool);

function approveAndCall(address spender, uint256 value, bytes calldata data) external returns (bool);

function transferFromAndCall(address from, address to, uint256 value, bytes calldata data) external returns (bool);

需要代币转账触发业务逻辑的场景（如去中心化交易所、支付系统）。
<!-- DAILY_CHECKIN_2026-02-01_END -->

# 2026-01-31
<!-- DAILY_CHECKIN_2026-01-31_START -->




學習一些erc標準，可在openzeppplin調用

1.  **ERC-20 – 可替代代币标准**
    

-   **核心定位**：以太坊**同质化代币（FT）** 的基础标准，所有稳定币（USDT/USDC）、平台币（BNB/ETH）都基于此。
    
-   **解决痛点**：早期代币没有统一接口，不同代币的转账、查询逻辑不兼容，钱包 / DApp 无法通用。
    
-   **核心功能**：定义了代币的发行、转账、授权三大核心能力，必须实现的 6 个强制接口：
    
    solidity
    
    ```
    function totalSupply() external view returns (uint256); // 总发行量
    function balanceOf(address account) external view returns (uint256); // 账户余额
    function transfer(address recipient, uint256 amount) external returns (bool); // 直接转账
    function approve(address spender, uint256 amount) external returns (bool); // 授权第三方花费
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool); // 授权转账
    function allowance(address owner, address spender) external view returns (uint256); // 查询授权额度
    ```
    
-   **适用场景**：稳定币、平台币、项目代币等**同质化资产**。
    
-   **缺点**：转账到合约地址时，若合约没有代币接收逻辑，代币会直接丢失（后续被 ERC-223 改进）。
    

**2\. ERC-721 – NFT 标准**

-   **核心定位**：以太坊**非同质化代币（NFT）** 的基础标准，每个代币独一无二，不可分割。
    
-   **解决痛点**：数字藏品、虚拟资产没有唯一标识的标准，无法证明所有权和稀缺性。
    
-   **核心功能**：为每个代币分配唯一`tokenId`，记录所有权，核心接口：
    
    solidity
    
    ```
    function balanceOf(address owner) external view returns (uint256); // 持有NFT数量
    function ownerOf(uint256 tokenId) external view returns (address); // 查询tokenId的所有者
    function safeTransferFrom(address from, address to, uint256 tokenId) external; // 安全转账（检查接收方是否支持NFT）
    function transferFrom(address from, address to, uint256 tokenId) external; // 普通转账
    function approve(address to, uint256 tokenId) external; // 授权单个NFT
    ```
    

### **3\. ERC-1155 – 多代币标准（FT+NFT）**

-   **核心定位**：**兼容 FT 和 NFT 的复合型代币标准**，一个合约可以发行多种代币，大幅节省 Gas 成本。
    
-   **解决痛点**：ERC-20 和 ERC-721 需要分别部署合约，发行多种资产时成本高、管理复杂；游戏中既有同质化金币，又有非同质化装备，需要两套标准。
    
-   **核心功能**：
    
    -   用同一个合约管理**多种代币**，每种代币用`id`区分；
        
    -   支持**批量转账**（`safeBatchTransferFrom`），一次转多种代币，降低 Gas；
        
    -   单一代币`id`的供应量为 1 时就是 NFT，供应量 > 1 时就是 FT。
        

ERC-1155 提供 `safeBatchTransferFrom` 接口，**一笔交易就能批量转账多种资产。**支持 FT 和 NFT 的查询、转账

在RWA上有很廣的適用場景
<!-- DAILY_CHECKIN_2026-01-31_END -->

# 2026-01-30
<!-- DAILY_CHECKIN_2026-01-30_START -->





今天系統性學習了合約安全

**安全设计原则**

-   最小权限原则（Least Privilege）
    
-   模块化结构便于审计
    
-   显式错误处理与事件记录
    

以下是常見的漏洞類型和防護：

-   重入攻击 Reentrancy 是利用外部合约在 fallback 中重新调用原函数。其防护方法：先更新状态，再转账。-->先改内部状态，再做外部交互。 、当用`msg.sender.call{value: ...}("")`给地址转账时，分两种情况：
    
    1.  如果接收方是**普通钱包地址**：直接转 ETH，执行结束，回到原合约继续运行；
        
    2.  如果接收方是**恶意合约地址**：会触发该恶意合约的`receive()`或`fallback()`回退函数（这两个是合约接收 ETH 时的默认执行函数），**此时原合约的执行会被「暂停」，先去执行恶意合约的回退函数，等回退函数执行完，才会回到原合约继续跑剩下的代码**。
        

重入攻击就是利用这个「执行暂停」的特性，薅合约的羊毛。

-   **预言机操纵 Oracle Manipulation：**依赖外部价格源的不可信更新。
    

解决方法：

-   使用 Chainlink 等权威价格源。
    
-   增加时序约束和多源验证。
    
-   使用 TWAP 等加权算法。
    

-   **整数溢出/下溢**
    

解決方法：

-   使用 `unchecked {}` 时需确保逻辑安全。
    
-   推荐使用Solidity 0.8+ 的内建溢出检查或 `SafeMath`
    

-   **权限控制缺失**
    

解決方法：所有管理函数应使用 `onlyOwner` 或 `AccessControl` 修饰符保护。

-   **未初始化代理：**基于代理模式的合约若未正确执行初始化函数，可能被任意人初始化并接管合约。
    

解決方法：可使用openzeppelin的`Initializable`库做**单次初始化保护**

-   **前置交易/三明治攻击**
    

**解決方法：**合约层**强制设置滑点容忍度**（minOut/maxIn）
<!-- DAILY_CHECKIN_2026-01-30_END -->

# 2026-01-29
<!-- DAILY_CHECKIN_2026-01-29_START -->






系統地學習了一下gas費。

Gas 是 EVM 执行操作的单位。每条指令消耗固定的 gas。

常見的減少gas費方法：

-   多次访问同一存储数据时，将其缓存到内存以减少 SLOAD 次数，优先使用 `memory。`尽量少碰链上存储（Storage），能放内存（Memory）的操作全放内存。意思是，先讀入内存，再進行加減變化。
    
-   使用位压缩（Bit Packing） ： 将多个变量压缩到一个 `uint256` 中以节省存储空间。
    
-   **循环优化 :** 减少不必要的运算，如 `array.length` 缓存到变量中。// ❌ 非优化
    
    for (uint256 i = 0; i < arr.length; i++) {
    
    ...
    
    }
    
    // ✅ 优化
    
    uint256 len = arr.length;
    
    for (uint i = 0; i < len; ++i) {
    
    ...
    
    }
    
-   `external` 比 `public` 更节省 gas，适用于仅被外部调用的函数。
<!-- DAILY_CHECKIN_2026-01-29_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->







今天依舊是運營部分的學習内容。

在社群再次進行了宣發，得到了一些回饋，也進行了一些解答，吸引了一些注意。（創造互動更能引起路人的注意）

舉行了會議，在嘉賓未能及時出席時主動分享吸引別人焦點，不要讓場子冷下來。

宣發要有針對性
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->








今天处理了运营方面的任务

用figma制作海报，想吸引人的文案，联系嘉宾以及多平台宣发，初步熟悉了运营工作的其中一个环节！
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->









今天在和虫虫忙比赛，完全没时间学习，不过也是在踏浪者记的会议上，学到了一些关于黑客松的技巧。

-   注意时间分配
    
-   优先确定主题明确方向
    
-   按比重分配任务:前端，后端，智能合约
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->










数组分为固定长度数组和可变长度数组两种：

```
// 固定长度 Array
uint[8] array1;
bytes1[5] array2;
address[100] array3;
```

可变长度数组（动态数组）：

```
// 可变长度 Array
uint[] array4;
bytes1[] array5;
address[] array6;
bytes array7;（不用[],且使用bytes或bytes1[]。bytes 比 bytes1[] 省gas。）
```

对于`memory`修饰的`动态数组`，可以用`new`操作符来创建，但是必须声明长度，并且声明后长度不能改变。例子：

```
// memory动态数组
uint[] memory array8 = new uint[](5);
bytes memory array9 = new bytes(9);
```

-   `length`: 数组有一个包含元素数量的`length`成员，`memory`数组的长度在创建后是固定的。
    
-   `push()`: `动态数组`拥有`push()`成员，可以在数组最后添加一个`0`元素，并返回该元素的引用。
    
-   `push(x)`: `动态数组`拥有`push(x)`成员，可以在数组最后添加一个`x`元素。
    
-   `pop()`: `动态数组`拥有`pop()`成员，可以移除数组最后一个元素。
    

// 结构体

struct Student{

uint256 id;

uint256 score;

}

Student student; // 初始一个student结构体
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->











-   第 1 行是注释，说明代码所使用的软件许可（license），这里使用的是 MIT 许可。如果不写许可，编译时会出现警告（warning），但程序仍可运行。
    

Solidity 注释以“//”开头，后面跟注释内容，注释不会被程序执行。

// SPDX-License-Identifier: MIT

-   常用类型：
    

1.  **值类型(Value Type)** ：包括布尔型，整数型等等，这类变量赋值时候直接传递数值。（短路規則：判斷了一個變量后不判斷另一個變量，存在於and和or中）
    
2.  **引用类型(Reference Type)** ：包括数组和结构体，这类变量占空间大，赋值时候直接传递地址（类似指针）。
    
3.  **映射类型(Mapping Type)** : Solidity中存储键值对的数据结构，可以理解为哈希表
    

-   修改链上状态：
    
    1.  写入状态变量。
        
    2.  释放事件。
        
    3.  创建其他合约。
        
    4.  使用 `selfdestruct`.
        
    5.  通过调用发送以太币。
        
    6.  调用任何未标记 `view` 或 `pure` 的函数。
        
    7.  使用低级调用（low-level calls）。
        
    8.  使用包含某些操作码的内联汇编。
        
-   函數：結構為function <function name>(\[parameter types\[, ...\]\]) {internal|external|public|private} \[pure|view|payable\] \[virtual|override\] \[<modifiers>\]
    

\[returns (<return types>)\]{ <function body> }

`{internal|external|public|private}`：函数可见性说明符，共有4种。

-   `public`：内部和外部均可见。
    
-   `private`：只能从本合约内部访问，继承的合约也不能使用。
    
-   `external`：只能从合约外部访问（但内部可以通过 `this.f()` 来调用，`f`是函数名）。
    
-   `internal`: 只能从合约内部访问，继承的合约可以用。
    

`[pure|view|payable]`：决定函数权限/功能的关键字，其中，pure不能讀取也不能寫入鏈上狀態；view能讀取但不能寫入狀態變量

如果计算不改变链上状态，就可以不用付 `gas`。包含 `pure` 和 `view` 关键字的函数是不改写链上状态的，因此用户直接调用它们是不需要付 gas 的（注意，合约中非 `pure`/`view` 函数调用 `pure`/`view` 函数时需要付gas）

-   `return`和`returns`。它们的区别在于：
    

1.  `returns`：跟在函数名后面，用于声明返回的变量类型及变量名。
    
2.  `return`：用于函数主体中，返回指定的变量。
    

// 返回多个变量

function returnMultiple() public pure returns(uint256, bool, uint256\[3\] memory){

return(1, true, \[uint256(1),2,5\]);

}

// 命名式返回

function returnNamed() public pure returns(uint256 _number, bool_ bool, uint256\[3\] memory \_array){

\_number = 2;

\_bool = false;

\_array = \[uint256(3),2,1\];

}

//結構式返回

uint256 \_number;

bool \_bool;

uint256\[3\] memory \_array;

(\_number, _bool,_ array) = returnNamed();/(, \_bool2, ) = returnNamed();
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->












今天學習了零知識證明。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/nounoupop77/images/2026-01-19-1768838798042-image.png)

-   完备性保证「合法选民的合法投票不会被无故拒绝」；
    
-   可靠性保证「非选民或者试图一人多投的人无法通过验证」；
    
-   零知识性保证「验证者无法从证明中推断出你是谁、投了什么票」。
    

自動生成identitySecret，由此計算identity commitment身份承諾-->所有`identityCommitment` 被组织成一棵 **Merkle 树**，其根节点 `root` 存储在投票合约中。选民在**本地**保存自己的 `identitySecret` 和对应的 Merkle 路径

某个合法 `identitySecret` 与当前投票 ID `electionId` 计算得到-->nullifier(唯一)

在零知識證明中：区块浏览器只会看到 nullifierHash/voteCommitment/proof，看不到具体选项或真实身份，因此无法把这次投票与你的钱包地址绑定。

1.  从你的输入中收集**公开输入（public inputs）**：
    
    -   如投票所属的 `electionId`；
        
    -   当前选民集合的 Merkle 根 `root` 等。
        
2.  从本地或钱包中收集**私有输入（witness）**：
    
    -   你的身份秘密 `identitySecret`；
        
    -   对应的 Merkle 路径；
        
    -   你选择的投票选项 `vote`。
        

![ZK 投票端到端流程示意](https://zkvote.0xtmp.xyz/images/zk-flow-v2.svg)

重新生成的流程图（v2）：避免 SVG 字符转义导致的图裂。

1.  **生成身份秘密与承诺**
    
    -   前端在本地生成 `identitySecret`；
        
    -   计算 `identityCommitment` 并发送给后台/注册合约；
        
    -   等待该承诺被加入选民 Merkle 树。
        
2.  **获取最新的选民集合信息**
    
    -   前端从合约读取当前的 Merkle 根 `root`；
        
    -   根据你的 `identityCommitment` 计算并存储对应的 Merkle 路径。
        
3.  **在本地构造 ZK 证明**
    
    -   你在界面中选择「同意 / 反对」等选项；
        
    -   浏览器端把以下数据输入到证明电路：
        
        -   私有输入：`identitySecret`、Merkle 路径、`vote`；
            
        -   公开输入：`root`、`electionId`；
            
    -   调用 zk 库，使用 `Proving Key` 生成证明 `proof`，同时计算 `nullifier`。
        
4.  **提交投票交易**
    
    -   前端将 `proof`、`publicInputs`（包含 `root`、`nullifier`、投票选项等必要字段）打包，调用投票合约；
        
    -   你的钱包会弹出签名与发送交易的确认页面，但交易数据中不包含你的身份秘密。
        
5.  **链上验证与计票**
    
    -   合约调用 Verifier 合约验证 `proof` 是否有效；
        
    -   检查 `nullifier` 是否已被使用；
        
    -   如果验证通过且 `nullifier` 未出现过，则记录这张票并标记 `nullifier` 为已使用。
        
6.  **查看结果与审计**
    
    -   任何人都可以在链上查看：
        
        -   每一次投票调用时提供的 `publicInputs`；
            
        -   以及合约验证通过的事实；
            
    -   但没有人可以从这些数据中还原出：
        
        -   具体是哪一个 `identitySecret` 参与了某次投票；
            
        -   某个真实世界身份在这场投票中选择了什么。
            

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/nounoupop77/images/2026-01-19-1768840038673-image.png)
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->













今天參加運營分享會。

-   知道了如何在telegram搭建和運營社群。通過對話題進行管理，分類來增加社群活躍度，對數據進行分析為社群製造吸引人的話題，認識到了機器人@MissRose\_bot，通過/help指令可以看到ross的功能
    
-   在twitter space策劃活動並執行。籌備話題，群聊。準備嘉賓，設計物料，海報，文案，進行推廣：提前2-3天宣發預熱。現場執行和活動復盤。（策劃>宣發>執行>復盤）
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->














今天在分享會上學習了標準erc7962。

該標準定義了接口erc20和erc721還提供了更强的隐私保护和交易灵活性。痛擊了傳統erc20和erc721容易被追蹤和gas費高昂等痛點

用 keyHash 替代地址，实现隐私保护

支持 Gas 赞助和批量操作，解決gas費高的問題
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->















今天有點忙碌所以都是斷斷續續在加入會議，不過也零零碎碎地學到了一些知識！

下午的co-learning，聼助教分享了一些運營經驗，雖然本身并沒有往運營發展的打斷但還是受益匪淺！感覺見了些市面哈哈哈

晚上的分享會，看到了一些人分享solidity，一些人分享怎麽起號，收穫了好多！也感嘆怎麽大家一邊有著工作一邊還這麽强。。。平時多多向大家學習才行。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
















今天參加了關於安全和合規的分享會，已經開營3天了，對于每天都有分享會可以聽這件事，我感到非常充實。

了解了web3存在的風險，比如釣魚攻擊，謹記不點不簽不裝不轉，最好不要以電子的形式保存私鑰或者助記詞

向善良的同學要到了合規會議的轉錄，計畫晚點再好好消化鄧律的內容。web3的合規確實是很多新人接觸web3的一個痛點，這次的會議帶給我很大啟發，感謝實習計劃這個平台為合規這個課題，給我們找資源和行業人士，為我們解答。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

















-   今天在平臺上mint了一個nft，很有趣地感受到了nft和錢包之間的關聯，每一步都需要錢包的確認。雖説在之前已經mint過nft，也在平臺上上架已經購買過，但還是感嘆mint一個nft這個平臺的簡單通俗易懂。
    
-   參加了今晚的分享會，在懵懵懂懂的情況下，發現有同學做了會議紀要并且無私地發出，非常感動，想到了web3的很多知識都是開源公開的，由衷感謝這種慷慨的行爲，在web3，至少知識不是私有化。
    
-   會議也大量討論了web3運行的底層邏輯，幫助了我更好瞭解了web3的運作。在此我加深了原本對web3的瞭解，比如錢包，簽名，solidity等等。我還弄懂了原本不是很清晰的gas費，以及gas費高低對交易造成的影響
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->


















今天學習用小狐狸錢包了，給同學發送了測試的sepolia幣，實行了一次交易！

參加了今晚Emily的分享會，得到了關於Web3行業趨勢、就業市場現狀、職位畫像、薪酬體系以及求職實戰指南的信息，也漸漸對未來的發展有了較爲清晰的方向。

還在「從零到一學習以太坊」這本教材裡學習了平時在web3看到的一些專有名詞如DAO,RAW背後的含義，看完感覺更加熟悉web3了！
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
