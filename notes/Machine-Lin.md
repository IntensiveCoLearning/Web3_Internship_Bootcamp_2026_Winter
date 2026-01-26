---
timezone: UTC+8
---

# M·N·H

**GitHub ID:** Machine-Lin

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->
# 一、Web2 to Web3 Week 2 Day 2

ethers.js 从 JavaScript 脚本读取（read）和写入（write）以太坊智能合约。

## 1.注意事项

-   **私钥 绝不 硬编码到 .js 文件**
    
-   **使用 .env + dotenv 加载**
    
-   **.gitignore 必须包含 .env 和 node\_modules**
    
-   **每个项目生成全新 mnemonic / 私钥（不要复用）**
    
-   **generateWallet() → 只显示地址，私钥手动复制到 .env**
    

## 2.读取合约状态

-   **用 Etherscan 找合约地址 + ABI（eg：Somfor NFT ）**
    
-   **ABI 复制成数组，存到单独文件**
    
-   **只读 Contract 连接 (provider，不用 signer）**
    

码中注意点：

-   view / pure 函数不需要 gas、不发交易、直接返回结果
    
-   返回值通常是 BigNumber → 用 formatEther / formatUnits 转人类可读
    
-   ABI 必须完整（Etherscan Verified 合约直接复制最方便）
    

## 3.写入合约

-   切换到 signer（有私钥，能签名）
    
-   重新创建 Contract 实例，这次用 signer
    

```
const signer = getSigner(false);
const contract = new ethers.Contract(contractAddress, somforNFTABI, signer);

async function mintNFT() {
  const mintPrice = await contract.mintPrice();  // 先读价格

  console.log("Sending mint tx...");
  const tx = await contract.mint({ 
    value: mintPrice   // payable 函数必须带 value
  });

  console.log("Tx hash:", tx.hash);
  const receipt = await tx.wait();           // 等待上链
  console.log("Minted! Block:", receipt.blockNumber);
}
```

**注意：**

-   如果函数是 payable，必须传 `{ value: xxx }`
    
-   value 用 `ethers.utils.parseEther("0.01")` 或 BigNumber
    
-   非 payable 函数传 value 会 revert
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->

# 一、Web2 to Web3 Week 2 Day 1

由于本期几乎全程live coding，可能笔记不多。

JavaScript + ethers.js 与以太坊区块链交互。

## 1.ethers.js

ethers.js 是由 Richard Moore 开发的一套完整、轻量级的以太坊（EVM 兼容链）开发库，相比 web3.js 更轻量、模块化更强、API 设计更简洁，是目前以太坊前端 / Node.js 开发的主流选择。

项目初始化：

-   创建 Node 项目`npm init -y`
    
-   "type": "module"（允许使用 ESM import 语法，避免到处写 async function main()）
    
-   `npm install ethers dotenv --save`
    

2.Providers

Provider 是区块链的“只读接口”。

-   用 Infura / Alchemy 连接主网或测试网
    
-   .env 文件存储密钥（INFURA\_PROJECT\_ID）+ .gitignore 保护
    

## 3.读取区块链数据

-   获取最新区块号：provider.getBlockNumber()
    
-   ENS 解析（主网/测试网）：provider.resolveName("vitalik.eth")
    
-   查询余额：provider.getBalance(address)
    
-   单位转换：ethers.formatEther(wei)、ethers.parseEther("1.5")
    

## 4.Wallets & Signers（可签名）

**Wallet = 私钥 + 地址**

**Signer = 可以签名的 Provider + Wallet**

创建方式：

-   随机：ethers.Wallet.createRandom()
    
-   从助记词：ethers.HDNodeWallet.fromMnemonic(mnemonic)
    
-   连接 provider：wallet.connect(provider)
    

其他的几乎都是实操，结合其GitHub仓库，代码食用更佳，虽然代码方面有点整不明白，但还是加油。
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->


# 一、Web2 to Web3 Week 1 Day 5

## 1.Stuck Transactions/交易停滞

-   现象：交易发出后长时间 pending（卡在内存池）
    
-   原因：Gas Price 太低、Nonce 错误、Gas Limit 不够等
    
-   解决方案：
    
    1.  Speed Up（提高 Gas 重新提交）
        
    2.  Cancel（发送 0 ETH 同 nonce + 高 gas 的交易来取消）
        
    3.  实际演示 MetaMask 的 cancel / speedup 按钮（经常不灵敏或行为奇怪）
        

## 2.Mainnet vs L2

-   在以太坊主网做小额交易 → Gas 极贵（有时 80% 手续费）
    
-   桥接资产到 L2（eg: Optimism）
    
-   在 L2 上重复相同操作 → 几乎免费、秒确认
    

## 3.ENS使用与缺陷

-   有些Dapp由于多地址解析、过期风险，而不支持ENS
    
-   ENS + MetaMask 在 L2 上的兼容性问题
    

## 4.Multisig 钱包

使用 Gnosis Safe（现 Safe{Wallet}）创建多签钱包。

## 5.Lending 借贷协议实操

本期具体以实操为主，没啥笔记可以做的似乎。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->



# 一、整个NFT生命周期

## 1.在 OpenSea 上购买 NFT

-   [https://opensea.io](https://opensea.io)
    
-   连接 MetaMask，OpenSea 自动显示你的 ENS 头像和持有的 NFT（去中心化身份）
    
-   搜索便宜的 NFT（因为 Sanford ——穷人角色很穷），找到地板价 0.01 ETH 左右的项目
    
-   在 OpenSea 上“Buy Now”：
    
    -   点击 Buy → MetaMask 弹出签名交易（支付 ETH + gas）
        
    -   交易确认后，NFT 立刻出现在你的钱包和 OpenSea 个人页面
        

-   NFT 的所有权完全由链上记录决定，任何支持 ERC721 标准的平台都能看到
    

## 2.ERC20 vs ERC721 核心区别

| 项目 | ERC20（可互换代币，如 DAI、USDC） | ERC721（非同质化代币，如 NFT） |
| --- | --- | --- |
| 存储方式 | mapping(address => uint256) balance | mapping(uint256 => address) ownerOf |
| 每个代币是否唯一 | 完全相同，可分割 | 每一个 tokenId 都唯一，不可分割 |
| 转账逻辑 | balanceOf[msg.sender] -= amount balanceOf[to] += amount | 必须先检查 msg.sender 是否是 ownerOf[tokenId] 然后 ownerOf[tokenId] = to |
| 典型用途 | 货币、稳定币、治理代币 | 艺术品、域名、游戏道具 |

该表格借助AI总结。

Austin 现场写的一个最简 ERC721 合约：

```
contract SimpleNFT {
    uint256 public totalSupply = 100;   // 总量限制，制造稀缺性
    uint256 private currentId = 0;
    mapping(uint256 => address) public ownerOf;

    function mint() public {
        require(currentId < totalSupply, "Sold out");
        currentId++;
        ownerOf[currentId] = msg.sender;   // 直接记录所有者
    }
}
```

## 3.IPFS 与 NFT 元数据标准——个人没太明白

## 4.在测试网（Rinkeby）免费 Mint + 用 Etherscan 直接调用合约

-   切换 MetaMask 到 Rinkeby 测试网
    
-   用水龙头（faucet）领免费 ETH
    
-   找一个免费 mint 的测试项目
    
-   直接在 Etherscan 的 Contract → Write Contract 页面连接 MetaMask 调用 mint() 或 ownerMint()
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->




# 一、Web2 to Web3 WEEK1 DAY 3

用钱包真正交互dApp，主要做两件事——注册ENS域名 + 在DEX上把ETH换成DAI

## 1.转账ETH

MetaMask转账：输入地址、金额、设置gas。

gas，单位为Gwei；基础费由网络自动调整，小费决定优先级。

Austin故意把gas设低一点（91 vs 当前92 Gwei），但因网络波动仍快速确认。

## 2.ENS

ENS——Web3的域名系统（DNS）。

与传统DNS类比：DNS把域名映射到IP，ENS把human-readable名字映射到以太坊地址（0x…）。

## 3.连接钱包&ENS注册

连接MetaMask到ENS网站（只暴露地址，不暴露私钥）。

搜索可用名字，注册1年，缴纳费用。

\-commit-reveal防抢注（front-running）

-   第一步：commit（提交哈希），发送一笔交易记录意图（≈$10 gas）。
    
-   等待约60秒（防抢跑）。
    
-   第二步：reveal + register，真正铸造ENS（其实是铸造一个NFT，代表所有权），这步gas更高（≈$30–40）。 注册成功后，在ENS管理页面设置 **reverse record**（反向解析）：让地址解析回名字（必须从该地址签名，防冒充），费用≈$23。
    
-   测试：在MetaMask直接输入 sanfordstout.eth 发0.01 ETH给它，成功解析并转账。
    

## 4.DEX VS CEX

-   CEX：中心化订单簿，易被黑/冻结。
    
-   DEX（Uniswap）：AMM自动做市商，流动性池（ETH-DAI对），套利机器人保持价格平衡。
    

## 5.DAI

DAI是锚定$1的去中心化稳定币，避免ETH价格剧烈波动。

-   超额抵押贷款（MakerDAO）：锁$150 ETH借$100 DAI，若抵押率跌破阈值自动清算。
    
-   价格>1$ → 鼓励铸造更多DAI（增加供给压低价格）。
    
-   价格<1$ → 鼓励偿还/销毁DAI（减少供给抬高价格）。
    
-   极端情况：烧MKR（治理代币）回购DAI。
    

## 6.女巫攻击

没整明白。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->





# 一、Web2 to Web3 WEEK 1 day1 总结笔记

## 1.该系列的整体定位：

已经会写代码的**传统 Web2 开发者**（前端/后端皆可），想快速进入以太坊 / Web3 开发的一个**速成课程**。

## 2.课程安排：

-   第1周：成为 Web3 高级用户（钱包、链上交互、基础设施熟悉）
    
-   第2周：脚本编写、智能合约、Solidity 入门
    
-   第3周：实战 DApp 开发，使用 [SpeedRunEthereum.com](http://SpeedRunEthereum.com) 平台做挑战
    
-   第4周：自己独立做一个项目
    

## 3.整体介绍：

不做详细笔记，因为与实习手册内的内容几乎相近，不做赘述。

# 二、Web2 to Web3 WEEK 1 day2 总结笔记

## 1.钱包基础概念：

### Seed Phrase / Mnemonics（助记词 / 种子短语）

使用BIP-39标准，其是加密货币钱包助记词（种子短语）生成与使用的行业标准，能将复杂私钥转为易记单词组合，用于钱包备份与恢复，被绝大多数现代钱包支持。使用 2048 个单词的词表生成 12 词（或 24 词）短语。

-   **生成过程（了解）：**
    

从高熵随机数（entropy） → 哈希 → 助记词 → 派生 master seed → 通过派生路径（derivation path，如 m/44'/60'/0'/0/0）生成私钥。

-   **这 12 个词能重现整个钱包，也能清空它**
    
-   **同一助记词 + 同一路径 = 永远相同账户**
    

\>\`永不分享助记词，不要截图、不要发微信/邮件、不要存云端。丢失 = 永久丢失资金；泄露 = 立即被盗。\`

### Keypairs（密钥对）

-   Private Key（私钥）
    

——256 位随机数（2²⁵⁶ 种可能），从**助记词派生**而来，是“签名交易的唯一凭证”。

-   `公钥->地址`的过程：
    

私钥 → ECDSA 算法生成公钥 → Keccak-256 哈希 + 截取后 20 字节 → 0x 开头的地址

-   签名过程：
    

交易数据 + 私钥签名 → 任何人用公钥验证签名（不暴露私钥）

代码不大清楚，还要再看看。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->







# 上手Remix

这里记录一下这次体验的步骤

## 一、新建文件

1.  点击左侧第一个图标 **📂**。
    
2.  点击 `contracts` 文件夹。
    
3.  点击下方的`Create New File`。
    
4.  输入文件名：`Storage.sol`，回车。
    

## 二、复制粘贴

easy~与之前学的Solidity基本语法差不多，大概能看懂，但自己写肯定不得行。

## 三、编译代码

把代码转为机器能看懂的Bytecode。

1.  点击左侧第三个图标 **🔌 (Solidity Compiler)**。
    
2.  点击左上角蓝色的 **Compile Storage.sol** 按钮。
    
3.  左侧图标上出现了一个绿色的对钩 ✅。
    

## 四、部署与交互

1.点击左侧第四个图标 **🚀 (Deploy & Run)**。

2.部署面板设置

-   **Environment (环境)**: 选择 `Remix VM (Cancun)`。
    
    -   _解释_：这是一个在浏览器内存里跑的“假区块链”。速度极快，不需要真钱。
        
-   **Account (账户)**: Remix 送了你 15 个测试账号，每个里面有 100 ETH。
    
    -   _解释_：你可以随意切换账号，假装自己是不同的用户。
        
-   **Contract (合约)**: 确保这里选中的是 `SimpleStorage`。
    

3.点击部署

-   点击橙色的 **Deploy** 按钮。
    
-   **观察底部控制台**，会出现一个绿色打勾符号，说明部署成功。
    
-   **观察左侧下方**：`Deployed Contracts` 栏目下，会出现一个新的条目 `SimpleStorage`。
    

4\. 开始玩耍 (交互)

点击 `SimpleStorage` 左边的小箭头 `>` 展开它，你会看到两个按钮：

1.  **store (橙色按钮)**：
    
    -   在旁边的框里输入一个数字，比如 `888`。
        
    -   点击按钮。
        
    -   _解释_：**这是一个写操作。**你会看到控制台有日志滚动，这是模拟了一次“交易”。
        
2.  **retrieve (蓝色按钮)**：
    
    -   直接点击按钮。
        
    -   下方会显示：`0: uint256: 888`。
        
    -   _解释_：**这是一个读操作。**它瞬间就返回了结果。
        

### C. 开始玩耍 (交互)

点击 `SimpleStorage` 左边的小箭头 `>` 展开它，你会看到两个按钮：

1.  **store (橙色按钮)**：
    
    -   在旁边的框里输入一个数字，比如 `888`。
        
    -   点击按钮。
        
    -   _解释_：这是一个**写操作**。你会看到控制台有日志滚动，这是模拟了一次“交易”。
        
2.  **retrieve (蓝色按钮)**：
    
    -   直接点击按钮。
        
    -   下方会显示：`0: uint256: 888`。
        
    -   _解释_：这是一个**读操作**。它瞬间就返回了结果。
        

### C. 开始玩耍 (交互)

点击 `SimpleStorage` 左边的小箭头 `>` 展开它，你会看到两个按钮：

1.  **store (橙色按钮)**：
    
    -   在旁边的框里输入一个数字，比如 `888`。
        
    -   点击按钮。
        
    -   _解释_：这是一个**写操作**。你会看到控制台有日志滚动，这是模拟了一次“交易”。
        
2.  **retrieve (蓝色按钮)**：
    
    -   直接点击按钮。
        
    -   下方会显示：`0: uint256: 888`。
        
    -   _解释_：这是一个**读操作**。它瞬间就返回了结果。
        

![屏幕截图 2026-01-20 215919.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Machine-Lin/images/2026-01-20-1768918071935-_____2026-01-20_215919.png)
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->









# 一、Solidity函数

函数是Solidity 智能合约中执行具体逻辑操作的核心组成部分。

通过函数，可以实现对状态变量的**读取、修改，或执行特定业务逻辑**。

## 1.格式

```
function <函数名>(<参数列表>)
    <可见性>
    <状态可变性>
    <修饰符列表>
    <虚拟/重写关键字>
    returns (<返回值列表>)
{
    // 函数体
}
```

不解含义：

-   <参数列表>：传入函数的参数
    
-   <可见性修饰符>：如 `public`、`private`、`internal`、`external`
    
-   <状态可变性修饰符>：如 `view`、`pure`、`payable`
    
-   <函数修饰符>：如 `onlyOwner` 等自定义逻辑控制
    
-   **virtual/override：用于支持继承与函数重写**
    

## 2.函数可见性

函数可见性决定了函数在何种上下文中可以被调用。

```
contract VisibilityExample {
    // 仅当前合约可访问
    function privateFunc() private pure returns(uint256) { return 1; }
    // 当前合约和继承合约可访问
    function internalFunc() internal pure returns(uint256) { return 2; }
    // 所有人可访问
    function publicFunc() public pure returns(uint256) { return 3; }
    // 仅外部调用
    function externalFunc() external pure returns(uint256) { return 4; }
}
```

可以理解为一般编程语言的作用域。

## 3.函数状态修饰符

用于指明函数是否修改或读取合约状态。

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

注释需要注意看看。

## 4.函数参数和返回值

Solidity 支持多参数与多返回值，以及命名返回值。

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

## 5.修饰符

修饰符允许在函数执行前插入额外逻辑，常用于权限控制与前置检查。

```
contract ModifierExample {
    address public owner;
    bool public paused = false;

    constructor() {
        owner = msg.sender;
    }

    // 自定义修饰符
    modifier onlyOwner() {
        require(msg.sender == owner, "Not the owner");
        _;  // 继续执行被修饰的函数
    }

    modifier whenNotPaused() {
        require(!paused, "Contract is paused");
        _;
    }

    function togglePause() public onlyOwner {
        paused = !paused;
    }

    // 使用多个修饰符
    function criticalFunction() public onlyOwner whenNotPaused {
        // 函数逻辑
    }
}
```

## 6.**继承与函数重写**

Solidity 支持单继承与多继承，子合约可重写父合约中的函数。

```
// 基础合约
contract Animal {
    string public name;

    constructor(string memory _name) {
        name = _name;
    }

    function speak() public virtual returns(string memory) {
        return "Some sound";
    }
}

// 继承合约
contract Dog is Animal {
    constructor(string memory _name) Animal(_name) {}

    // 重写父类函数
    function speak() public pure override returns(string memory) {
        return "Woof!";
    }
}

// 多重继承
contract Pet is Animal {
    address public owner;

    constructor(string memory _name, address _owner) Animal(_name) {
        owner = _owner;
    }
}

contract Labrador is Dog, Pet {
    constructor(string memory _name, address _owner)
        Dog(_name)
        Pet(_name, _owner) {}
}
```

## 7.**接口与抽象合约**

接口与抽象合约用于定义规范与继承框架。

```
// 接口定义
interface IERC20 {
    function transfer(address to, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

// 抽象合约
abstract contract AbstractToken {
    string public name;

    // 没有函数体的抽象函数，必须被子类使用 override 关键词重载实现
    function totalSupply() public virtual returns (uint256);

    // 有函数体实现的抽象函数，子类可以不使用 override 关键词重载直接继承已有的实现，也可以选择使用 override 关键词重载实现
    function decimals() public view virtual returns (uint8) {
        return 18;
    }
}
```

## 8.**事件机制**

事件用于在链上记录重要状态变化，并可由外部监听器（如检索器或前端应用）捕捉。

```
contract EventExample {
    // 定义事件
    event Transfer(address indexed from, address indexed to, uint256 amount);
    event Approval(address indexed owner, address indexed spender, uint256 amount);

    mapping(address => uint256) public balances;

    function transfer(address to, uint256 amount) public {
        require(balances[msg.sender] >= amount, "Insufficient balance");

        balances[msg.sender] -= amount;
        balances[to] += amount;

        // 触发事件
        // 可以在区块链浏览器查找到当前事件记录
        emit Transfer(msg.sender, to, amount);
    }
}
```
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->











# 一、Solidity智能合约编程

Solidity 是一种 面向合约 的高级编程语言，专门用于在 以太坊虚拟机（EVM）上编写智能合约。

它具有 静态类型 、 支持继承 、 库 和 复杂的用户定义类型 等特性。

## 1.基础语法与开发范式

首先，每个 Solidity 文件必须以版本声明开始：

```
pragma solidity ^0.8.0;
```

其二，基本数据类型和复合数据类型基本与寻常编程语言大概一致：

基本数据类型：

| 类型 | 描述 | 示例 | 默认值 |
| --- | --- | --- | --- |
| uint8 | 8 位无符号整数 | 0 ~ 255 | 0 |
| uint16 | 16 位无符号整数 | 0 ~ 65535 | 0 |
| uint256 / uint | 256 位无符号整数 | 0 ~ (2^256 - 1) | 0 |
| int8 | 8 位有符号整数 | -128 - 127 | 0 |
| int256 / int | 256 位有符号整数 | -2^255 ~ (2^255 - 1) | 0 |
| address | 以太坊地址 | 0x…. | 0 |
| bytes1 ~ bytes32 | 固定长度字节数组 | bytes32 data = "Hello" | 0x00 |
| bytes | 动态字节数组 | bytes memory data = "Hello World" | "" |
| string | UTF-8 编码字符串 | string name = "Alice" | "" |

复合数据类型：

| 类型 | 语法 | 描述 | 示例 |
| --- | --- | --- | --- |
| 静态数组 | T[k] | 固定长度数组 | uint[5] numbers |
| 动态数组 | T[] | 可变长度数组 | uint[] memory list |
| 映射 | mapping(K => V) | 键值对存储 | mapping(address => uint256) balances |
| 结构体 | struct | 自定义数据结构 | struct Person { string name; uint age; } |
| 枚举 | enum | 枚举类型 | enum Status { Pending, Active, Inactive } |

其三，函数修饰符：

可见性修饰符表：

| 修饰符 | 可见范围 | 描述 |
| --- | --- | --- |
| public | 内部+外部 | 任何地方都可以调用 |
| external | 仅外部 | 只能合约外部调用 |
| internal | 内部+继承 | 当前的合约和子合于可以调用 |
| private | 仅内部 | 只有当前的合约可调用 |

状态修饰符表：

| 修饰符 | 状态读取 | 状态修改 | Gas 消耗 | 描述 |
| --- | --- | --- | --- | --- |
| pure | ❌ | ❌ | 低 | 不读取也不修改状态的函数 |
| view | ✅ | ❌ | 低 | 只读取状态，不修改状态 |
| payable | ✅ | ✅ | 正常 | 可以接收以太币的函数 |
| 无修饰符 | ✅ | ✅ | 正常 | 可以读取和修改状态 |

最后开发范式：

-   **状态机模式**
    

智能合约本质上是一个状态机，通过交易改变合约状态。

-   **事件驱动编程**
    

使用事件（Events）记录重要的状态变化，便于前端监听和日志记录。

-   **模块化设计**
    

通过继承和库（Library）实现代码复用和模块化。

## 2.合约结构

### （1）基本结构

① Solidity的单行注释符号——//

② pragma 关键字用于声明 Solidity 源代码所需的**编译器版本**，确保合约在兼容的编译器环境中正确编译。

③ contract 关键字用于定义一个智能合约。

格式为：contract 合约名 {...}

④ 一个智能合约的基本结构通常由 状态变量 、 构造函数 和 普通函数 。

eg：

```
pragma solidity ^0.8.0;

contract MyContract{
   //变量
   uint256 public myNumber;
   
   //构造函数
   constructor(){
       myNumber = 100;
   }

   //函数
   function setNumber(uint256 _number) public{
       myNumver=_number;
   }
}
```
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->












# 一、Dapp架构

Dapp 的架构主要由三个核心部分组成：

## 1.前端

-   **前端是Dapp与用户交互的界面，通常与传统Web应用一样由HTML、CSS和JavaScript构建。与传统Web应用不同的是，Dapp前端不会直接连接区块链网络，而是通过钱包注入Provider或第三方RPC节点与区块链交互：**
    
-   ——通过RPC节点对智能合约发起只读调用（eg：eth\_call），获取合约状态、事件日志等链上数据。
    
-   ——当需要修改状态的操作，由前端构造对智能合约的交易调用，交由钱包完成签名后，再通过RPC 节点广播到区块链网络并最终上链执行。
    
-   **前端需要集成区块链钱包（如 MetaMask）来进行身份验证和签署交易，确保用户的隐私和安全。**
    

## 2.智能合约

-   **智能合约是 Dapp 的核心，它定义了应用的业务逻辑，并部署在区块链上。**
    
-   **在以太坊平台上，智能合约通常使用 Solidity 编程语言编写，并通过 Ethereum Virtual Machine (EVM) 执行。**
    

## 3.**数据检索器**

-   **智能合约通常以** `Event` **形式释放日志事件。**
    
-   **Dapp 在前端进行数据展示时需要检索器内的数据**。
    

## other：

## **区块链和去中心化存储**

-   **区块链用于存储智能合约的状态数据及交易记录。**
    

去中心化存储如 **IPFS**（InterPlanetary File System）或 **Arweave**，用于存储大规模的非结构化数据（如图片、文档等），确保数据不易丢失和篡改。

-   **通过使用去中心化存储，Dapp 确保所有数据在多个节点上备份，保证数据的持久性和去中心化特性。**
    

# 二、Dapp开发流程

## 1.需求分析

-   确认Dapp的目标和功能
    
-   选择区块链平台（eg：以太坊、Solana、Polygon 等）
    
-   设计用户体验（UX）
    

## 2.智能合约开发

-   编写智能合约
    
-   编写测试用例
    
-   审计和优化
    

## **3.检索器开发**

其是获取链上数据的核心，负责捕获智能合约释放的事件并以合理的方式将其存入数据库的不同的表内部。

-   确定功能需要的数据内容
    
-   编写检索器程序
    
-   部署和运维
    

## 4.前端开发

-   选择前端框架
    
-   连接钱包
    
-   显示区块链数据
    
-   处理交易签名与确认
    

## 5.区块链交互

前端和智能合约通过 **Viem**（推荐）、**Ethers.js** 或 **Wagmi** 等现代化库进行交互。

-   读取数据
    
-   发送交易
    

## 6.部署与上线

-   部署智能合约
    
-   前端部署
    
-   发布和维护
    

# 三、RPC节点服务

## 1.RPC本质

-   **在 Web3 开发中，RPC（Remote Procedure Call，远程过程调用） 是连接前端应用与区块链网络的关键桥梁。**
    
-   **RPC 是一种通信协议，允许应用程序通过网络调用远程服务器上的函数或方法。**
    
-   **在区块链开发中，RPC 节点是运行区块链客户端软件的服务器，它们维护完整的区块链数据副本，并提供 API 接口供开发者查询链上数据、发送交易等操作。**
    

可以理解为：你是通过RPC再区块链网络上得到你所需要的东西。

## 2.RPC作用

### \-1.**读取链上数据**

-   查询账户余额、交易历史
    
-   读取智能合约的状态变量
    
-   获取区块信息、Gas 价格等
    

### **\-2.发送交易**

-   将用户签名的交易广播到区块链网络
    
-   查询交易状态和确认数
    
-   估算交易所需的 Gas 费用
    

### **\-3.事件监听**

-   监听智能合约事件（Events）
    
-   实时获取链上状态变化
    
-   支持 WebSocket 长连接推送
    

### **\-4.网络管理**

-   切换不同的区块链网络（主网、测试网）
    
-   获取网络信息和链 ID
    
-   管理节点连接状态
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->















# 一、节点间的链接&通信方式

## 1.节点发现——先加好友再扩散（基于UDP+Kademlia）

在一个以太坊节点刚启动时，其只知道少数几个引导节点（boot nodes）的地址。

这些引导节点是长期在线、对外公布地址的全节点。

1.1 Discovery 协议：Ping / Pong / FindNode / Neighbors

以太坊的节点发现协议跑在UDP上，只干两件事：

①确定对方在线——即发ping收pong

②要通讯录——即发送FindNode接受Neighbors or Nodes

如此反复，此即为Kademlia 风格的 DHT（分布式哈希表）发现机制。

1.2 Kademlia 的直观类比：快递分拨中心

发现阶段 = 用 UDP + Kademlia 找到一堆靠谱的邻居，把自己的“好友列表”填满。

## 2.建立安全连接——TCP+RLPx/devp2p

2.1 TCP

首先，TCP是可靠的、面向连接，速度慢；UDP是不可靠、不建立连接，速度快。

那么，节点之间一旦决定互相连接，就会通过 TCP 建立双向连接。

2.2 RLPx

RLPx = 在一条 TCP 线上开很多“分机”：同一根网线同时承载区块同步、交易广播、状态请求等多种子协议流量。

## 3.消息传播全网

3.1 Gossip（广播式传播）

Gossip 协议，某节点收到一笔新交易或新块，就会随机挑选未得到的邻居进行传播，邻居再与其他人进一步传播，进而使整个网络几乎所有节点都知道这条消息。

优点：

-   完全去中心化、自组织
    
-   以太坊能在几秒级别完成一笔交易或区块的全网传播
    

3.2 点对点请求-响应（拉取指定数据）

Gossip 适合传播“最新消息”，而请求-响应则是精准请求。

**两者结合在一起，才能既保证新消息快速散播，又保证任何节点都能随时补齐历史、追上最新状态。**

## 小结：

① 发现阶段（UDP + Kademlia）：

先通过引导节点 + Ping/Pong/FindNode，逐步认识一群“距离 ID 较近”的邻居，把自己的“通讯录”填满。

② 连接阶段（TCP + RLPx/devp2p）：

对选中的邻居建立加密的 TCP 连接，在同一条线路上多路复用区块同步、交易传播等子协议。

③ 传播阶段（Gossip + 请求/响应）：

新交易和区块通过 Gossip 像办公室八卦一样在全网扩散；缺失的历史数据则通过点对点请求-响应按需拉取。

# 二、全节点、轻节点、归档节点

## 1.全节点（Full Node）=当前状态 + 全部区块（但老状态会被“裁剪”）

-   全节点是运行一对执行客户端+共识客户端，从创世块起验证整条链，保留所有区块的区块头和区块体（也就是交易、收据等历史记录不会被删），只保留最近若干个区块的完整状态，但会修剪掉更老的状态的节点。
    
-   全节点最重要的特点是“自己验证，不求他人”，但不会保持所有的历史快照。
    

## 2.归档节点（Archive Node）=全节点 + 从创世到现在的“所有历史状态”

-   归档节点就是禁用修剪的全节点，并保存了从创世到现在的每一个区块高度下的完整状态。
    
-   但是其体量大、同步时间长、维护成本高。
    
-   所以可以理解为：归档节点 = “国家档案馆”，它保留着一切。
    

## 3.轻节点 / 轻客户端（Light Node / Light Client）=只存区块头，需要时向全节点要具体数据

-   轻节点是只保存区块头，不保存完整区块体和全量状态，需要时会像以上的两个节点请求具体数据，并用区块头的state root / Merkle 根 对收到的数据进行验证的节点。
    
-   其对于存储和带宽需求低，保留“可验证性”。
    
-   但是它不参与共识，即不能做验证者，对数据有一定依赖。
    
-   所以可以理解为：轻节点 = “只看目录 + 零信任校验”。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->


















# 一、以太坊节点&客户端

## 1.节点（node）：

任何一台运行**以太坊客户端软件**并连接到其他节点的电脑，就是一个以太坊节点。

## 2.客户端（client）：

是对以太坊协议的具体实现，会按协议规则验证数据、同步区块和状态，帮助网络保持安全。

**完整的以太坊节点**\=**执行客户端**（Execution Client，EL）+**共识客户端**（Consensus Client，CL）（+验证者客户端（Validator））

**（1）执行客户端——“干活的”（收集材料、执行流程、更新状态）**

执行客户端——主要负责链上的“业务逻辑和状态”：

-   监听网络上广播来的交易与区块
    
-   在 EVM（以太坊虚拟机） 中执行交易和智能合约
    
-   维护当前链的状态数据库
    
-   提供 JSON-RPC 接口（eth\_\*），给 dApp / 钱包 / 脚本调用
    
-   通过自己的 P2P 网络转发交易和区块头 / 区块体
    

**（2）共识客户端——“法官”（裁决判断）**

共识客户端——主要负责权益证明 PoS 共识：

-   维护信标链（Beacon Chain）视图和 fork choice 规则
    
-   在每个 slot 里选择谁来提议区块，并对区块进行投票
    
-   跟踪验证者集合、质押余额、惩罚（slashing）等共识状态
    
-   把来自执行客户端的结果当作“材料”，决定哪一个区块被接受到链上
    

**（3）验证者客户端——“陪审团代表”（支持并做出行动）**

验证者客户端——负责：

-   管理你的验证者密钥
    
-   在轮到你时提议新区块
    
-   在每个 slot 为看到的区块做 attestation（投票）
    

## 3.节点和客户端的关系

以太坊全节点=（在电脑上运行）1 个执行客户端 + 1 个共识客户端+（1个验证者客户端）

-   执行客户端负责“算交易、管状态、对外提供 RPC”
    
-   共识客户端负责“跑 PoS、选区块、投票达成共识”
    
-   它们通过 Engine API 的“专线电话”互相协作，整个节点再通过 P2P
    

简易示意图：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Machine-Lin/images/2026-01-15-1768480003092-image.png)
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->





















# 一、“全球可编程区块链”——以太坊

全球性（Global）+可编程（Programmable）+区块链（Blockchain）

## 1.区块链

第一天笔记以较为详细写明，此处不再赘述。

## 2.可编程

可编程是由\_智能合约（Smart Contracts） + 图灵完备虚拟机（EVM）\_所构成。

-   **智能合约**
    

智能合约是一段部署在区块链上的代码。

_我的理解是你可以看作是编程中的函数。_

-   **图灵完备虚拟机**
    

“图灵完备”——只要给足时间和资源，就能表达几乎任何可计算的逻辑。

以太坊的智能合约语言就是在图灵完备虚拟机上运行的，它可以说是一台**分布式**的**通用计算机**，可以跑**所有**的程序，而这些程序的结果会被写进一个**全网共享**的状态机里。

## 3.全球性

以太坊网络像一台**单一的**、**全球共享**的世界计算机——

-   其不属于任何人，而任何人都可以使用
    
-   其由全球节点组成
    
-   其强抗审查、强抗宕机，任何单一的节点离线，都不会使得整个网络停摆
    
-   任何人何时何地，只要能连上互联网就能部署自己的智能合约、调用别人部署的合约、把自己的资产、身份、应用逻辑绑在这个全球共享的状态机上
    

**_总的来说：以太坊就是建立在全球、去中心化、抗审查基础设施上的一台通用“世界计算机”，任何人都可以在上面写程序、跑逻辑、发资产。_**

# 二、dApps

## 1.dApp本质：

去中心化应用程序（dApps）是构建在去中心化网络上的应用，拼接了链上的智能合约（后端逻辑）和链下的前端页面/客户端（用户界面）。

**_也可以理解为它是web2和web3在技术上融合结合而成的一种应用程序，框架是web2技术，而运行逻辑和核心是web3。_**

-   dApp的后端代码即智能合约运行在以太坊主网或二层网络之上的EVM中，由全网节点共同执行和记录
    
-   dApp的前端可以用任意Web/App技术编写，可以放在传统服务器，也可以托管在IPFS、Arweave 等去中心化存储上
    

## 2.dApp核心技术特征

-   **去中心化**
    
-   **确定性**
    
-   **图灵完备**
    
-   **隔离性**
    

## 3.dApp开发的好处

1.**零停机时间**

无外界因素可以将其关机或下架。

2.**隐私/匿名性**（相对传统Web）

无实名注册、提交身份证明，但其实是透明伪匿名而不是“完全隐身”。

3.**抗审查**

无外界单一实体可以组织你传输或读取公开数据，且无法轻易封锁。

4.**数据完整性**

依托密码学和共识机制，，链上数据一旦写入就几乎不可篡改，恶意行为者无法事后伪造交易或随意修改历史状态。

5.**无需信任/可验证行为**

不与人性斗争，而是相信代码逻辑规则。

## 4.dApp开发的缺点

**1.维护困难——链上代码默认不可修改**

**2.性能开销大扩展难度大**

**3.网络拥堵，Gas成本上升，用户体验差**

**4.用户门槛高**
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->























# 一、以太坊起源

以太坊最初由 Vitalik Buterin（也就是 V 神）在 2013 年提出，并在 2015 年 7 月 30 日随“Frontier（边境）”阶段正式上线主网。

# 二、以太坊平台定位

以太坊（Ethereum）是一个去中心化、开源并且具备智能合约功能的公共区块链平台。

其核心组件是以太坊虚拟机（Ethereum Virtual Machine，EVM），它是一个图灵完备的虚拟机，能够执行智能合约代码，让区块链不再只是“记账”，而是**可以运行各类程序和应用。**

# 三、以太坊核心特性

相较于大多数其他加密货币或区块链技术，以太坊的特点包括但不限于以下几点：

-   **智能合约（Smart Contracts）**
    

智能合约是存储在区块链上的程序，由网络节点执行。“矿工（PoW）”时代完全过渡到“验证者（PoS）”时代。可以理解为智能合约使得在进行每一笔操作时是由分布式节点“自动执行”，并为此收取手续费，而且整个过程是公开可验证、不可随意篡改的。

-   **分布式应用程序（dApps）**
    

以太坊上的分布式应用程序（dApp）部署在区块链上，不依赖单一服务器，也没有传统意义上的 “后台可关机”。

只要以太坊网络和客户端软件还活着，dApp 就很难被单一主体关停。

意味着与现有的传统应用程序由单一主体所主控相反，某种程度上实现应用程序的自由运行。

-   **代币（Tokens）**
    

代币分为两种：

1.  可替代的（ERC-20）：例如游戏里的“金币”、治理代币、稳定币等；
    
2.  不可替代的（ERC-721 / ERC-1155）：对应独特资产，如艺术品、门票或游戏角色。
    

-   **权益证明（PoS）**
    

以太坊最初采用的是工作量证明（Proof-of-Work, PoW）机制，和比特币类似，由“矿工”通过算力竞争出块。

自 2022 年 9 月 15 日的 The Merge 升级起，以太坊完全改用权益证明（Proof-of-Stake, PoS），淘汰了挖矿机制，_能耗降低约 99.95%。_

PoW（工作量证明，历史上的以太坊共识）vs PoS（权益证明——以太坊当前使用的共识）

一句话总结，也是个人认为的好理解的——**PoW 用电力和硬件“押注诚实”，PoS 用锁定的 ETH 和削减机制“押注诚实”。**

-   **燃料费（Gas）**
    

“燃料”（Gas）是对计算与存储资源的计量单位，发送 ETH、转账代币、调用合约函数、部署合约，统统会消耗 Gas。

Gas费=基础费（base fee）+小费（priority fee）

只要你使用了服务，就需要消耗Gas，其中基础费会被销毁，小费奖励给出块验证者。

-   **Proto-Danksharding（EIP-4844）与 Dencun 升级**
    

Proto-Danksharding （原型丹克分片，EIP-4844）已在 2024 年 3 月 13 日的 Dencun 升级 中正式上线主网。

引入了新的“携带 blob 的交易类型” （blob-carrying transactions），为 Rollup 提供临时数据空间。这些blob数据只是临时保存，一段时间后就会被丢弃，降低存储压力，也减少数据传输成本。

-   **分片与 Danksharding**
    

“分片”本质上是把一个大系统拆成多个“分区”并行处理，从而提升吞吐量。

but新路线转向 Danksharding：它更像一种“数据分片（data-sharding）”，核心是引入更多 blob 数据空间，加上数据可用性采样（DAS），让节点只需要随机抽样检查一小部分数据，就能对整个大数据块“有把握”。

便于理解的一段话：**Proto-Danksharding 先给以太坊挂上“小挂车”，而完整 Danksharding 则是把挂车扩展成“高速货运列车”。**

-   **叔块（Uncle / Ommer Block）**
    

叔块在转为权益证明后已停用。

总览图：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Machine-Lin/images/2026-01-13-1768285242614-image.png)

# 四、Ether（ETH）的定义与系统职能

## 定义：

以太币（Ether，ETH）是以太坊区块链上的原生数字资产。其是可以点对点发送、无需银行等中介机构的数字货币，你可以在各大加密货币交易所买卖 ETH，它有真实且浮动的市场价格。

## 区别：

-   **比特币——“数字黄金”**
    
-   **ETH——“实用+资产”**
    

## 核心作用：

### 1.网络燃料（Gas）

这是 ETH 在协议层面最根本、最核心的作用。

在以太坊网络上，任何操作都会消耗链上计算与存储资源。为了防止垃圾交易或恶意程序无限消耗资源，这些操作都必须付费，这个费用就是我们常说的 Gas 费，并且必须用 ETH 支付。

* * *

**Gas费忽高忽低的原因：**

_1.网络拥堵（供需决定价格）_

在交易较少的时段（如部分时区的深夜、周末），Gas 会明显下降。

在DeFi 高峰、NFT 热点铸造、牛市情绪爆发时，大家都争着抢区块空间，Base Fee 和 Tip 都会上升，造成Gas上升。

_2.全球用户活跃时间差_

以太坊是 24 小时全球网络，美洲和欧洲工作时间段（例如美国东部时间早 8 点到下午 1 点），dApp / DeFi 交互往往更集中，Gas 通常偏高；反之亦然。

_3.交互复杂程度_

与复杂合约交互（例如多步的 Swap、Mint NFT、批量操作）会消耗更多 Gas 单位，费用自然更高。

### 2.参与网络共识与安全（Staking）

**在 PoS 机制下，ETH 在共识层扮演了三个关键角色：**

_1.质押（Staking）资产_

_2.获得奖励（Rewards）_

PoS验证者的收入=共识层奖励（Consensus Layer Rewards）+执行层奖励（Execution Layer Rewards）+（某些协议给的补贴）

共识层奖励（Consensus Layer Rewards）——来自 PoS 协议本身的 ETH 增发，对应你参与出块与投票的行为。

执行层奖励（Execution Layer Rewards）——来自用户支付的 优先费（tip） 和可能存在的 MEV （最大可提取价值），由打包区块的验证者获得，波动更大但有放大利润的空间。

（有些协议还会在你通过其平台质押时额外抽取服务费或提供二次激励（例如发放额外代币、返佣等），这属于协议层的设计。）

_3.提供经济安全（Security / “保证金”）_

_规则：_

行为诚实、在线率高 → 按规则拿奖励

作恶 → 面临 Slashing（削减） 和强制退出

（质押在系统中的 ETH 越多、越分散，攻击者要控制足够多权益发动攻击的成本就越高）——某种激励模式（maybe）

* * *

Slashing 是 PoS 系统里最严厉的惩罚，用来针对那些危害网络安全的行为，而不是单纯“偶尔掉线”，典型的Slashing 行为：对同一个 slot 双重提议区块、对同一高度同时给两个不同区块投票、提交 “环绕投票”（surround vote），试图篡改链的最终性。

* * *

**Slashing 流程：**

1.  _立即罚没一部分质押_
    
2.  _长达约 36 天的移除期_
    
3.  _相关性惩罚_
    
4.  _举报奖励_
    

发现别人作恶并提交证据的“举报者”，可获得一小部分被 Slash 的 ETH 作为奖励，进一步激励大家监督彼此

### 3.价值储存和交换媒介

**ETH 在更广泛的加密经济中扮演着价值载体的角色**：

-   _数字货币与投资资产_
    

ETH 长期稳居全球市值第二大的加密货币。

-   _DeFi 生态的抵押品与流动性基石_
    
-   _NFT 与数字经济的基石货币_
    

* * *

### NFT

NFT （Non-Fungible Token）是在区块链上具有唯一标识的加密代币，，每一枚 NFT 都代表某个特定、不可互换的资产，类似于数字世界的“所有权凭证”或“收藏证书”。（链上的记录清楚写明谁在什么时候铸造、谁买走了它、现在谁拥有）

常见NFT标准：

-   ERC-721
    

以太坊上最经典的 NFT 标准。

-   ERC-1155
    

* * *

### “同质化代币”（Fungible Token）

“同质化代币”指每一个单位完全等价、可互换，并且可以拆分成更小单位，不关心“具体这一枚是谁”，只关心数量的代币。

_其更适用于货币、计价单位、储值和高流动性资产的角色，而 NFT 更适合表达唯一性与所有权_
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

























# 一、区块链

## 1.区块链的本质

_区块链=区块+链_

区块：

-   **一个区块=交易记录等信息+之前的区块摘要（哈希）**
    
-   **每个区块只有有限的存储容量**
    
-   **每个区块会在一定时间内打包**
    

链：

-   **一条链只可以链接两个区块**
    
-   **只有一条链**
    
-   **区块按照顺序通过链依次串联**
    

严谨定义：_区块链是一种去中心化的分布式账本技术，用于在网络节点之间安全、透明且不可篡改地记录事务数据。每条链由一系列按照时间顺序相连的“区块”组成，每个区块内部包含了多笔交易数据及元数据，确保了数据记录的完整性与可追溯性。_

## 2.分布式网络

是什么？——简单来说就是，将相关资源分散在多个独立的节点，没有中心节点的网络架构。

正因如此，可以保证区块中的历史记录没有被篡改且可信。

## 3.区块链特性

-   **不可篡改（真正）**
    

无法改变历史信息，如果修改，你必须修改后面的全部区块。而区块链网络通常分布全球，一个人几乎不可能控制大部分节点。当被修改的节点不超过51%时，这个改代不会被认可。

-   **公开透明、匿名**
    

每个人都可以顺着区块和链找到历史上所有的记录来查看你的钱包余额，but没人知道这是你的钱包。

-   **快速交易**
    

只要你的交易记录被打包在区块链中，交易就自动完成。

-   **去中心化**
    

区块链网络遍布全球，每个节点都会存储一份相同的区块链数据，没有人能够控制全部节点，这便意味着这个区块链数据永久存在。

# 二、web2 vs web3.0 vs web3（认识：**Web3 ≠ Web 3.0**）

## 1.web2（也就是当前的互联网）

核心特征：

-   **中心化控制**
    

你的所有数据是存储在科技巨头的服务器之中。

-   **用户生产数据，但不拥有数据**
    

怎么理解不拥有数据？就是你将一个数据例如图片、文章上传至软件时，这些数据就存入了软件的服务器，你无法彻底将其取出。

-   **广告驱动来赚钱**
    

用户生产数据，可能有几个渠道来使用这些数据：基于你的数据分析你的画像来精准推送广告、将你的数据打包出售给其他数据公司、凭借你的数据来吸引更多用户进入软件。

个人理解：_在web2里，你在朋友圈发了图片，相当于上传到了微信的服务器（中心化控制），而微信有权对图片压缩优化，可以通过这张图片来分析你，提取你的爱好和情况，给你推送广告，赚取广告费而这与你无关（广告驱动来赚钱）。重要的是，微信有权把你的图片删除，并且封禁你的账号，至此你的图片是无法要回来的（用户生产数据，但不拥有数据）。_

## 2.web3.0（语义网）

核心特征：

-   **使用标准描述数据关系**
    
-   **信息按照标准格式组织，便于机器的理解**
    
-   **构建的是实体之间的语义关系网络**
    

个人理解：_相当于对于数据进行了标准、格式化的处理，而为的就是让使用者更好的利用机器来查找筛选数据，差不多是图书馆图书的标签利于管理员查找资料的概念。_

## 3.web3（去中心化互联网）

核心特征：

-   **数据主权属于用户**
    

用区块链来存储身份和资产。

-   **无需信任中介（去中心化）**
    

通过智能合约自动执行规则。

-   **核心组件**：
    

钱包——签名——>Dapp——调用——>智能合约——读写——>区块链

意味着什么？

-   **用户真正拥有数字资产，没有平台制约**
    
-   **金融服务无门槛，无需实体介质**
    
-   **应用可自由组合**
    
-   **内容永久存在，因为存储于分布式网络，它几乎实现永存**
    

个人理解：_你真正拥有了数据的主权，是平台服务你，完成你的需求，无论是物质还是心理，而不是现有的你服务于平台。但在公共环境里，没有所谓的制约，没有一个所谓的机构来筛选数据或者信息，便无法保证数据或信息的真实和可信性，是存在隐患的。_

## 4.web3和web3.0

**Web3 ≠ Web 3.0**

关键区别：

-   **web3.0不是区块链技术，而是传统互联网的数据组织升级；web3是区块链驱动的革命**
    
-   **web3.0主要不依赖ai，而是勇敢标准化数据格式实现**
    

## 5.技术栈对比

web2开发：

```
React + Node.js + MySQL
```

web3开发：

```
React + Ethers.js + Solidity + IPFS
```

web3.0开发：

```
Python + RDFLib + SPARQL
```

# 三、区块链的核心组成部分

## 1.一条区块链的运行方式

1.  **用户发起交易**：用户通过钱包应用发起转账、智能合约调用等操作
    
2.  **交易广播**：交易信息被广播到整个网络中的各个节点
    
3.  **节点验证**：网络中的矿工节点验证交易的合法性（余额是否足够、签名是否正确等）
    
4.  **打包成块**：通过共识机制（如工作量证明），矿工将验证过的交易打包成新的区块
    
5.  **链接上链**：新区块被添加到区块链上，更新全网的账本状态
    
6.  **奖励发放**：成功打包区块的矿工获得代币奖励和交易手续费
    

![ecosystem-BrsPk-pm.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Machine-Lin/images/2026-01-12-1768221498834-ecosystem-BrsPk-pm.jpg)

## 2.核心组成部分

区块链通过一条链来记录全部的信息，这条链存在于对于的去中心化网络。

去中心化网络，由无数节点提供服务来维持网络运行。

而节点通过计算验证交易获得代币奖励。

## 那么，什么是代币奖励机制？

# 四、区块链代币激励机制

## 1.概念

去中心化的网络由无数节点**提供服务**来维持网络运行，这些操作统称为挖矿。维持这些服务的人一般称之为矿工。矿工们维持网络运行需要奖励，就像你工作需要工资，**在区块链中对矿工的奖励一般是代币**，你经常听说的燃料费（Gas Fee）就是矿工们的“工资”。

网络节点服务提供商可以得到奖励，即节点可以得到奖励，不同网络服务提供商可以得到不同的代币奖励，比如：**比特币**。

## 2.比特币（BTC）

特征：

-   **比特币具备货币属性，按照白皮书设计，它并不会通胀**
    
-   **比特币的价格由供需决定，但因为没有监管和限制，容易受到热点新闻的影响出现大幅度波动，出现不稳定的价格**
    

特点：

-   **账户匿名、交易公开透明、不可篡改、完全在虚拟网络中，相比跨境转账结算等场景要方便许多**
    

在整个网络中，你的交易将通过一个随机生成的钱包地址完成，与这个钱包地址相关的交易等在网络上全部是公开透明的。匿名是因为别人不知道你跟这个钱包有关联。如果你在社交媒体发布过这个钱包地址，那么就可以将其跟你联系在一起。

-   **难以追踪和限制，所以经常被黑产利用，进行洗钱等违法犯罪活动**
    

于每打包一个区块需要约 10 分钟，因此也会影响交易的实时性。每个区块的存储数据也是有限的。不过，越来越多的新区块链技术正在优化和解决这些问题。

# 五、公链 vs 私链 vs 联盟链

按照去中心化程度从高到低排列：

1.  **公链——开放访问，安全，但速度慢**
    
2.  **联盟链——联合治理，安全与速度平衡**
    
3.  **私链——权限访问，高速，可控**
    

| 区块链类型 | 节点加入方式 | 数据可见性 | 管理模式 | 适合场景 |
| --- | --- | --- | --- | --- |
| 公链 | 任何人自由加入 | 所有人可见 | 去中心化（大家投票） | 加密货币、公共存证 |
| 联盟链 | 需联盟成员邀请/审批 | 仅联盟成员可见 | 多中心化（董事会决策） | 供应链、金融协作 |
| 私链 | 由老板严格审批 | 仅内部成员可见 | 中心化（老板说了算） | 企业内部管理、审计 |
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
