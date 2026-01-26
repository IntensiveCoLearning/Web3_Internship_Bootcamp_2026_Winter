---
timezone: UTC+8
---

# Emulisy

**GitHub ID:** Emulisy

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->
## 3\. 前端使用 ethers.js 的三大核心对象 ⭐⭐⭐

### 3.1 Provider（只读）

**作用：连接区块链，读取数据**

```
import { ethers } from "ethers"

const provider = new ethers.BrowserProvider(window.ethereum)
```

能干什么：

-   查余额
    
-   查区块
    
-   调用合约的 `view / pure` 方法
    

不能干什么：

-   ❌ 发交易
    
-   ❌ 改状态
    

* * *

### 3.2 Signer（签名者）

**作用：代表“用户身份”，可以发交易**

```
const signer = await provider.getSigner()
```

本质：

-   来自 MetaMask
    
-   背后是用户私钥（你永远拿不到）
    

能干什么：

-   转账
    
-   调用合约写方法
    
-   签名消息
    

* * *

### 3.3 Contract（合约 JS 映射）

**作用：把 Solidity 合约变成 JS 对象**

```
const contract = new ethers.Contract(
  contractAddress,
  abi,
  signer // 或 provider
)
```

-   用 `provider`：只能读
    
-   用 `signer`：能写
    

* * *

## 4\. 前端最常见的 5 个使用场景

* * *

## 4.1 连接 MetaMask（前端第一步）

```
await window.ethereum.request({
  method: "eth_requestAccounts"
})
```

⚠️ 必须用户手动触发（按钮）

* * *

## 4.2 获取用户地址 & 余额

```
const signer = await provider.getSigner()
const address = await signer.getAddress()

const balance = await provider.getBalance(address)
console.log(ethers.formatEther(balance))
```

👉 **典型“钱包信息展示”功能**

* * *

## 4.3 普通转账（不经过合约）

```
await signer.sendTransaction({
  to: "0xabc...",
  value: ethers.parseEther("0.01")
})
```

-   会弹 MetaMask
    
-   会产生交易
    
-   会进新区块
    

* * *

## 4.4 调用智能合约（前端最核心）

### 读合约（不花 gas）

```
const total = await contract.totalSupply()
```

### 写合约（要 gas）

```
const tx = await contract.transfer(to, amount)
await tx.wait()
```

👉 前端要处理：

-   pending 状态
    
-   成功 / 失败
    
-   loading UI
    

* * *

## 4.5 监听链上事件（实时 UI）

```
contract.on("Transfer", (from, to, value) => {
  console.log("Transfer happened")
})
```

常用于：

-   NFT mint 成功提示
    
-   DeFi 状态更新
    
-   DAO 投票变化
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

-   创建新节点的方法：
    
    -   `document.createElement(tag)` —— 用给定的标签创建一个元素节点，
        
    -   `document.createTextNode(value)` —— 创建一个文本节点（很少使用），
        
    -   `elem.cloneNode(deep)` —— 克隆元素，如果 `deep==true` 则与其后代一起克隆。
        
-   插入和移除节点的方法：
    
    -   `node.append(...nodes or strings)` —— 在 `node` 末尾插入，
        
    -   `node.prepend(...nodes or strings)` —— 在 `node` 开头插入，
        
    -   `node.before(...nodes or strings)` —— 在 `node` 之前插入，
        
    -   `node.after(...nodes or strings)` —— 在 `node` 之后插入，
        
    -   `node.replaceWith(...nodes or strings)` —— 替换 `node`。
        
    -   `node.remove()` —— 移除 `node`。
        
    
    文本字符串被“作为文本”插入。
    
-   这里还有“旧式”的方法：
    
    -   `parent.appendChild(node)`
        
    -   `parent.insertBefore(node, nextSibling)`
        
    -   `parent.removeChild(node)`
        
    -   `parent.replaceChild(newElem, node)`
        
    
    这些方法都返回 `node`。
    
-   在 `html` 中给定一些 HTML，`elem.insertAdjacentHTML(where, html)` 会根据 `where` 的值来插入它：
    
    -   `"beforebegin"` —— 将 `html` 插入到 `elem` 前面，
        
    -   `"afterbegin"` —— 将 `html` 插入到 `elem` 的开头，
        
    -   `"beforeend"` —— 将 `html` 插入到 `elem` 的末尾，
        
    -   `"afterend"` —— 将 `html` 插入到 `elem` 后面。
        

另外，还有类似的方法，`elem.insertAdjacentText` 和 `elem.insertAdjacentElement`，它们会插入文本字符串和元素，但很少使用。

-   要在页面加载完成之前将 HTML 附加到页面：
    
    -   `document.write(html)`
        
    
    页面加载完成后，这样的调用将会擦除文档。多见于旧脚本。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


# Hardhat vs Foundry

## 1\. Hardhat

-   JS/TS 生态的智能合约开发框架
    
-   核心命令：
    
    -   `hardhat compile` → 编译合约
        
    -   `hardhat test` → JS/TS 测试
        
    -   `hardhat node` → 本地测试链
        
    -   `hardhat run scripts/deploy.js` → 部署脚本
        
-   特点：
    
    -   适合前端/全栈开发者
        
    -   插件丰富（gas、coverage、verify 等）
        
    -   测试用 JS/TS 编写
        
-   用途：写、测试、部署合约；与前端 Dapp 联动
    

* * *

## 2\. Foundry

-   Rust 写的高性能 Solidity 开发工具链
    
-   核心工具：
    
    -   `forge` → 编译、测试、部署
        
    -   `cast` → CLI 交互、调用交易
        
    -   `anvil` → 本地链（类似 Hardhat node）
        
-   特点：
    
    -   测试用 Solidity 写（更接近合约本身）
        
    -   速度快，支持 fuzz 测试、trace、调试
        
    -   适合合约开发/安全/审计
        
-   用途：专业级合约开发、测试、部署
    

* * *

## 3\. 对比总结

| 方面 | Hardhat | Foundry |
| --- | --- | --- |
| 生态 | JS/TS | Solidity + CLI |
| 测试语言 | JS/TS | Solidity |
| 本地链 | hardhat node | anvil |
| 适合 | 前端/全栈 | 合约开发/审计 |
| 插件/扩展 | 丰富 | 核心工具强大，插件少 |
| 速度 | 中等 | 很快 |
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



# Web3 Frontend Tech Stack

## 1\. What is Web3 Frontend

-   Web3 frontend = **Web2 UI + Blockchain interaction**
    
-   Smart contracts are the **source of truth**
    
-   Frontend talks to blockchain via **wallet + RPC**
    
-   No direct control over blockchain state
    

* * *

## 2\. Frontend Basics

-   **Languages**: JavaScript / TypeScript
    
-   **Frameworks**: React, Next.js
    
-   **Styling**: Tailwind CSS, CSS Modules
    

* * *

## 3\. Wallet Integration

-   **Wallets**: MetaMask, WalletConnect
    
-   **Role**:
    
    -   Manage private keys
        
    -   Sign transactions
        
-   **Libraries**:
    
    -   ethers.js
        
    -   wagmi / viem
        

* * *

## 4\. Blockchain Interaction

-   **RPC Providers**:
    
    -   Infura, Alchemy, QuickNode
        
-   **Frontend can**:
    
    -   Read on-chain data
        
    -   Send signed transactions
        
    -   Listen to events
        

* * *

## 5\. Smart Contract Interaction

-   Required:
    
    -   Contract address
        
    -   ABI
        
-   Two types:
    
    -   **Read**: no gas, `call`
        
    -   **Write**: needs signature + gas, `transaction`
        

* * *

## 6\. Off-chain & Backend (Optional)

-   Used for:
    
    -   Data indexing (The Graph)
        
    -   Caching & performance
        
-   Rule:
    
    -   Backend is **not** the source of truth
        

* * *

## 7\. Typical Architecture

```
Frontend (React / Next)
   |
   | Wallet + ethers.js
   v
RPC Provider
   |
   v
Smart Contracts (Blockchain)
```

* * *

## 8\. Key Takeaways

-   Wallet signs, node executes, contract stores state
    
-   Frontend orchestrates user interaction
    
-   Decentralization depends on **where truth lives**
    

* * *

如果你愿意，我可以再帮你压成 **「面试速背版」** 或 **「学习路线版」**，哪个你更想要？
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




# FullStack Web3 Engineer Learning Path – Notes  

## Core Skill Areas  
————————————————

1.  **Blockchain Fundamentals**
    

-   Blockchain structure: blocks, transactions, consensus
    
-   Ethereum and EVM basics
    
-   Tokens: ERC20, ERC721, ERC1155
    
-   Gas, transaction fees, block confirmation
    
-   Wallets and accounts (EOA vs contract accounts)
    

2.  **Smart Contracts**
    

-   Solidity basics: contracts, state, functions, modifiers, events
    
-   Contract deployment and testing
    
-   Security practices: reentrancy, access control, overflow
    
-   Interacting with contracts via JS libraries
    

3.  **Frontend Development (Web3)**
    

-   Modern frameworks: React, Vue
    
-   Web3 libraries: Ethers.js, Viem, Wagmi, Web3.js
    
-   Wallet integration: MetaMask, Coinbase Wallet
    
-   Handling transactions and signatures
    
-   Reading chain data, listening to events
    

4.  **Backend / Indexing**
    

-   Indexers for on-chain data (The Graph, Ponder)
    
-   Database integration (PostgreSQL, MongoDB)
    
-   Server-side handling for notifications, analytics
    
-   RPC nodes usage and management (Alchemy, Infura, QuickNode)
    

5.  **Fullstack Integration**
    

-   Connect frontend → wallet → smart contract → indexer → frontend
    
-   Handle read-only calls vs state-changing transactions
    
-   Error handling and UX for slow/blockchain transactions
    
-   Testing end-to-end Dapp behavior
    

6.  **Deployment & DevOps**
    

-   Smart contract deployment: Hardhat, Foundry
    
-   Frontend deployment: IPFS, Arweave, Vercel
    
-   CI/CD for Dapps
    
-   Environment management: .env files, private key security
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->





# DApp Development Workflow

## Requirement Analysis

\- Clarify the problem the DApp aims to solve

\- Determine whether blockchain is necessary

\- Decide on the target chain (e.g. Ethereum, Polygon)

\- Define core features and user interactions

## Smart Contract Design

\- Design contract logic and data structures

\- Define state variables, functions, and access control

\- Consider security, gas efficiency, and upgradeability

\- Choose the programming language (usually Solidity)

## Smart Contract Development

\- Write smart contracts

\- Follow best practices (checks-effects-interactions pattern)

\- Add events for frontend listening

\- Write unit tests for contract logic

## Smart Contract Testing

\- Test contracts using local or test networks

\- Cover normal cases and edge cases

\- Perform security testing to avoid common vulnerabilities

\- Fix bugs and optimize gas usage

## Contract Deployment

\- Deploy contracts to a testnet

\- Verify contract code if required

\- Deploy to mainnet after sufficient testing

\- Record contract addresses and ABIs

## Frontend Development

\- Build user interface (Web, Mobile, etc.)

\- Integrate wallet connection (e.g. MetaMask)

\- Use libraries like ethers.js or web3.js

\- Handle transaction states and user feedback

## Frontend and Contract Integration

\- Connect frontend to deployed smart contracts

\- Call read/write functions

\- Listen to contract events

\- Manage errors and network changes

## DApp Testing

\- End-to-end testing with real wallets

\- Test user flows and transaction lifecycle

\- Ensure correct behavior under network delays or failures

## Deployment and Maintenance

\- Deploy frontend to hosting services

\- Monitor contract usage and errors

\- Update frontend as needed

\- Maintain and upgrade contracts if supported
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->






## 一、Web3 合规 (Compliance)

1.  核心监管概念
    
    -   KYC (Know Your Customer/了解你的客户): 验证用户身份的过程，通常需要上传护照、身份证和面部识别。这是交易所和法币出入金通道的标配。
        
    -   AML (Anti-Money Laundering/反洗钱): 监控和防止非法资金流动的法规。链上分析工具（如 Chainalysis）被用来追踪黑客资金或制裁地址。
        
    -   CTF (Counter-Terrorist Financing/反恐融资): 防止资金流向恐怖主义活动的措施。
        
2.  代币分类与证券法 (以美国 SEC 为例)
    
    -   豪威测试 (Howey Test): 判断一种加密资产是否属于“证券”的核心标准。如果满足以下条件，通常被视为证券：
        
        1.  金钱投资。
            
        2.  投资于共同事业。
            
        3.  期待获利。
            
        4.  获利主要来自于他人的努力。
            
    -   证券型代币 (Security Token): 受到严格证券法监管，发行需注册。
        
    -   功能型代币 (Utility Token): 主要用于网络内部使用（如 Gas 费），监管相对宽松，但界限日益模糊。
        
3.  全球主要监管框架
    
    -   欧盟 (MiCA 法案): 世界上第一个全面的加密资产监管框架，涵盖稳定币发行商和服务提供商（CASP）。
        
    -   香港 (VASP 牌照): 虚拟资产服务提供商发牌制度，允许散户在持牌交易所交易大盘币（BTC/ETH）。
        
    -   美国: 监管分散，SEC（证券交易委员会）管证券，CFTC（商品期货交易委员会）管商品。目前执法主要靠起诉（Regulation by Enforcement）。
        
4.  稳定币合规
    
    -   储备金证明: 发行商需证明有 1:1 的法币或等价资产支持。
        
    -   审计与透明度: 定期发布由第三方会计事务所出具的鉴证报告。
        
5.  DAO 的法律结构
    
    -   纯链上组织: 法律实体地位不明，成员可能承担无限连带责任。
        
    -   法律包装 (Legal Wrapper):
        
        -   UNA (非法人非营利协会): 常用于 DAO。
            
        -   LLC (有限责任公司): 如怀俄明州 DAO LLC，为成员提供责任保护。
            
        -   基金会结构: 常见于开曼群岛或瑞士，作为 DAO 的现实世界代理人。
            

## 二、Web3 安全 (Security)

1.  智能合约常见漏洞 (Smart Contract Vulnerabilities)
    
    A. 重入攻击 (Reentrancy)
    
    -   原理: 攻击者合约在被调用转账时，回调原合约的函数，在原合约更新余额之前再次提取资金。
        
    -   防御: 使用“检查-生效-交互”模式 (Checks-Effects-Interactions)，或使用重入锁 (ReentrancyGuard)。
        
    
    B. 整数溢出/下溢 (Overflow/Underflow)
    
    -   原理: 数字超过变量存储上限归零，或减过头变成最大值。
        
    -   防御: Solidity 0.8.0 版本以后编译器自动检查，此前版本需用 SafeMath 库。
        
    
    C. 访问控制缺失 (Access Control)
    
    -   原理: 关键函数（如铸币、提款、修改所有权）没有加 `onlyOwner` 等修饰符，导致任何人都能调用。
        
    -   防御: 严格检查每个函数的可见性和修饰符。
        
    
    D. 预言机操纵 (Oracle Manipulation) / 闪电贷攻击
    
    -   原理: 攻击者利用闪电贷借入巨款，瞬间砸盘或拉升去中心化交易所(DEX)的价格，导致依赖该价格的借贷协议发生错误清算或坏账。
        
    -   防御: 使用时间加权平均价格 (TWAP) 或去中心化预言机 (如 Chainlink)，不依赖单一 DEX 的现货价格。
        
    
    E. 抢跑 (Front-running) / MEV
    
    -   原理: 机器人在内存池 (Mempool) 看到你的交易，通过提高 Gas 费抢先打包，从而从你的滑点中获利。
        
2.  钱包与用户安全
    
    A. 私钥与助记词管理
    
    -   铁律: 助记词一旦触网（截图、发微信、存网盘）即视为泄露。
        
    -   冷钱包 (Hardware Wallet): 私钥永不触网（如 Ledger, Trezor）。
        
    -   热钱包: 浏览器插件或手机 App，仅用于存放小额资金。
        
    
    B. 授权签名 (Approve/Permit) 风险
    
    -   风险: 许多 DApp 要求“无限授权” (Unlimited Allowance)。如果 DApp 合约被黑，用户钱包里的对应代币会被转空。
        
    -   防御: 定期使用 [Revoke.cash](http://Revoke.cash) 等工具取消不用的授权；仅授权实际交易金额。
        
    
    C. 钓鱼攻击 (Phishing)
    
    -   手法: 假冒的空投网站、假冒的 NFT Mint 页面、高仿推特账号。
        
    -   识别: 检查域名拼写，不点击不明链接，不轻易签署 "SetApprovalForAll" 或不明 Hash 的签名。
        
3.  开发与运营安全流程
    
    -   审计 (Audit): 任何主网上线的代码必须经过至少一家（最好两家）专业安全公司（如 CertiK, OpenZeppelin, SlowMist）的审计。
        
    -   多重签名 (Multi-sig): 团队管理的金库或合约权限，必须使用多签钱包（如 Gnosis Safe），避免单人作恶或私钥丢失。
        
    -   漏洞赏金 (Bug Bounty): 在 Immunefi 等平台发布赏金计划，奖励发现漏洞的白帽子。
        
    -   时间锁 (Timelock): 关键参数修改需经过时间锁延迟（如 24-48 小时），给社区反应时间。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->







# SOLIDITY BASIC SYNTAX NOTES

1.  FILE STRUCTURE
    

* * *

Every Solidity file should start with a license identifier and the pragma version.

```
// SPDX-License-Identifier: MIT pragma solidity ^0.8.20;
```

2.  CONTRACT STRUCTURE
    

* * *

A contract is similar to a ‘class’ in object-oriented languages.

contract MyContract { // State variables (stored on the blockchain) uint256 public myNumber;

```
// Constructor (runs once upon deployment)
constructor(uint256 _initialNumber) {
    myNumber = _initialNumber;
}
```

}

3.  DATA TYPES
    

* * *

A. Value Types

-   boolean: bool public isActive = true;
    
-   uint: Unsigned integer (positive only). ‘uint256’ is the standard.
    
-   int: Signed integer (positive and negative).
    
-   address: Holds a 20-byte Ethereum address. address public owner; address payable public recipient; (can receive Ether)
    

B. Reference Types

-   string: string public name = “Solidity”;
    
-   struct: Custom defined types. struct User { uint id; string name; }
    
-   mapping: Key-value pairs (hash tables). mapping(address => uint) public balances;
    
-   Arrays: uint\[\] public dynamicArray; uint\[5\] public fixedArray;
    

4.  FUNCTIONS
    

* * *

A. Syntax function functionName(uint _param) public view returns (uint) { return_ param + myNumber; }

B. Visibility Modifiers

-   public: Can be called internally and externally.
    
-   private: Only accessible within the current contract.
    
-   internal: Accessible within the contract and derived contracts.
    
-   external: Can only be called from outside the contract.
    

C. State Modifiers

-   view: Promises not to modify the state (reads data only).
    
-   pure: Promises not to modify or read from the state (calculates logic only).
    
-   payable: Allows the function to receive Ether.
    

5.  DATA LOCATIONS
    

* * *

Where data is stored during execution:

-   storage: Persistent on-chain storage (state variables).
    
-   memory: Temporary storage (exists only during function execution).
    
-   calldata: Non-modifiable temporary storage for function arguments.
    

6.  CONTROL STRUCTURES
    

* * *

Solidity uses standard C-style syntax.

```
// If-Else if (x > 10) { // do something } else { // do something else }
```

```
// Loops for (uint i = 0; i < 10; i++) { // loop logic }
```

```
while (x < 5) { // loop logic }
```

7.  ERROR HANDLING
    

* * *

-   require(condition, “message”): Reverts if condition is false. Used for inputs.
    
-   revert(“message”): Aborts execution and reverts changes.
    
-   assert(condition): Used for internal invariants (serious bugs).
    

8.  EVENTS
    

* * *

Events allow logging to the blockchain, which apps can listen for.

event Transfer(address indexed from, address indexed to, uint amount);

function sendMoney(address _to, uint_ amount) public { emit Transfer(msg.sender, _to,_ amount); }

9.  GLOBAL VARIABLES
    

* * *

-   msg.sender: Address of the person calling the function.
    
-   msg.value: Amount of Wei sent with the call.
    
-   block.timestamp: Current block timestamp.
    
-   block.number: Current block number.
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->









# Smart Contract Development – Concise Notes  

## Dapp Architecture

A Dapp is different from a traditional centralized app. Logic and data are not controlled by a single server but distributed across the blockchain.

Main components:

Frontend

-   Built with HTML, CSS, JavaScript (React, Vue)
    
-   Does NOT connect directly to blockchain
    
-   Uses wallet-injected provider or RPC node
    
-   Reads data via contract calls
    
-   Sends transactions via wallet signing (MetaMask)
    

Smart Contracts

-   Core business logic of the Dapp
    
-   Deployed on blockchain
    
-   Immutable and transparent
    
-   Written in Solidity on Ethereum
    
-   Executed by the EVM
    

Indexer (Data Retriever)

-   Listens to smart contract events
    
-   Processes and stores data in traditional databases
    
-   Solves inefficient on-chain data queries
    
-   Common for NFTs, DeFi analytics, dashboards
    

Blockchain & Decentralized Storage

-   Blockchain stores state and transaction history
    
-   IPFS / Arweave store large files (images, metadata)
    
-   Ensures decentralization and data persistence
    

##   
2\. Dapp Development Workflow  

1.  Requirement Planning
    

-   Define features (transfer, staking, voting)
    
-   Choose blockchain (Ethereum, Polygon, Solana)
    
-   Design user experience
    

2.  Smart Contract Development
    

-   Write Solidity contracts
    
-   Write unit tests
    
-   Perform security audits
    
-   Prevent common vulnerabilities
    

3.  Indexer Development
    

-   Decide what data frontend needs
    
-   Parse events from contracts
    
-   Store processed data in databases
    
-   Often written in TypeScript
    

4.  Frontend Development
    

-   Build UI with modern frameworks
    
-   Connect wallet (MetaMask)
    
-   Display on-chain and indexed data
    
-   Handle transaction confirmation
    

5.  Deployment
    

-   Deploy contracts to testnet or mainnet
    
-   Deploy frontend to IPFS or web hosting
    
-   Monitor, update, and maintain
    

————————————————  
3\. Ethereum Development Environment  
————————————————

Basic tools:

-   Node.js
    
-   npm or yarn
    
-   Git
    

Local development frameworks:

Foundry

-   Very fast, Rust-based
    
-   Includes tools for testing, deploying, local node
    
-   Suitable for advanced users
    

Hardhat

-   Most popular Ethereum dev framework
    
-   Beginner-friendly
    
-   Strong plugin ecosystem
    
-   Common in production projects
    

Wallet & Frontend Tools

-   MetaMask for signing transactions
    
-   Viem and Wagmi for frontend-chain interaction
    
-   OpenZeppelin for secure contract libraries
    
-   Remix for quick browser testing
    

##   
4\. RPC Nodes  

What is RPC

-   RPC nodes are blockchain servers
    
-   They expose APIs for reading data and sending transactions
    
-   Frontends rely on RPC nodes to talk to the blockchain
    

What RPC does

-   Read balances, blocks, contract state
    
-   Broadcast signed transactions
    
-   Listen to smart contract events
    
-   Switch networks and query chain info
    

JSON-RPC

-   Standard Ethereum communication protocol
    
-   Uses methods like getBalance, call, sendTransaction, getLogs
    

RPC Providers

-   Alchemy: enterprise-grade, production use
    
-   Infura: popular for development
    
-   QuickNode: high performance
    
-   Public nodes: free but unstable
    
-   Ankr: multi-chain support
    

##   
5\. RPC Best Practices  

-   Never hardcode private keys or API keys
    
-   Use environment variables
    
-   Handle RPC failures and retries
    
-   Be aware of rate limits
    
-   Use multiple RPC endpoints as fallback
    
-   Prefer local nodes for development
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->










# From Wallet Transaction to Block Confirmation: Complete Flow

## 1\. Transaction Creation (Wallet Side)

1\. **User initiates a transaction in the wallet**

\- Example: Alice wants to send 1 ETH to Bob

\- Wallet generates a transaction object:

```
{

“from”: “AliceAddress”,

“to”: “BobAddress”,

“value”: 1 ETH,

“nonce”: 5,

“gasLimit”: 21000,

“gasPrice”: 50 Gwei

}
```

\- `nonce`: sequence number to prevent replay attacks

\- `gas`: transaction fee to compensate miners/validators

2\. **Wallet signs the transaction with the private key**

\- Ensures the transaction is authorized by Alice

\- Anyone can verify the signature but cannot forge it

## 2\. Transaction Broadcasting

1\. Wallet broadcasts the signed transaction to the blockchain network (P2P network)

2\. Nodes receive the transaction and place it in the **mempool** (pending transaction pool)

\- Transactions in mempool are waiting to be included in a block

3\. At this stage, the transaction is **not yet part of any block**

## 3\. Transaction Validation (Nodes / Miners / Validators)

Before packaging into a block, transactions are validated:

1\. **Signature verification** → confirms the transaction is from Alice

2\. **Sufficient balance** → Alice has enough ETH + gas

3\. **Nonce correctness** → prevents duplicate transactions

Only valid transactions are eligible to be included in a block

## 4\. Block Packaging

1\. Miner/validator selects transactions from the mempool

\- Usually prioritizes transactions with higher gas fees

2\. Creates a new block:

\- Contains a list of transactions

\- Includes **previous block hash**

\- Constructs a block header:

```
Block N+1
```

```
- PreviousHash: hash(Block N)
```

```
- MerkleRoot: hash(all transactions)
```

```
- Timestamp: 1673779200
```

```
- Nonce: ??? (used in PoW)
```

3\. Essentially, the block organizes transactions into a structured format ready for consensus

## 5\. Consensus Phase

Different blockchains use different consensus mechanisms:

-   a. Proof of Work (PoW, e.g., Bitcoin)
    

\- Miner performs **work computation**

\- Tries different `Nonce` values to find a hash below the target

\- Successfully mined block is broadcast to the network

-   b. Proof of Stake (PoS, e.g., Ethereum 2.0)
    

\- Validator is selected as block proposer

\- Verifies transaction validity

\- Signs the block and broadcasts it

\- Other validators vote to finalize the block

**Core idea: global agreement on which block is accepted**

## 6\. Block Confirmation (On-chain)

1\. Nodes receive the new block and re-verify transactions and block integrity

2\. Updates local ledger/state

3\. Block is appended to the blockchain

\- Links to previous block hash

\- Transactions in this block are now confirmed

4\. Wallet can check transaction status:

\- “Pending” → “Confirmed after 1/2/3 blocks”

\- Confirmed transactions are immutable

\-–

## 7\. Summary Flow Diagram

Wallet creates & signs transaction

↓

Broadcast to network (mempool)

↓

Nodes/Miners validate transactions

↓

Transactions packaged into new block

↓

Consensus mechanism confirms the block

↓

Block appended to blockchain, transaction complete
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->













# **Ethereum Basics**

## **1\. What Ethereum Is**

Ethereum is a **decentralized open-source blockchain platform** that extends blockchain beyond simple currency. It supports smart contracts—self-executing code stored on the blockchain—enabling developers to build decentralized applications (dApps), decentralized finance (DeFi) protocols, NFTs, DAOs and other Web3 services. It has its own native cryptocurrency called **Ether (ETH)**, which is required to pay network fees and secure the blockchain.

Ethereum aims to be a **global shared computing platform**, often described as “Blockchain 2.0,” where any developer can deploy programmable logic on a trustless network.

## **2\. Ethereum vs Bitcoin**

Although both are built on blockchain technology, Ethereum and Bitcoin differ significantly:

-   **Purpose**
    
    -   _Bitcoin_: Designed primarily as decentralized digital money and store of value.
        
    -   _Ethereum_: Built as a programmable platform for decentralized applications and smart contracts.
        
-   **Programming Capability**
    
    -   Bitcoin has limited scripting capabilities.
        
    -   Ethereum is Turing-complete, meaning it supports complex logic through languages like **Solidity**.
        
-   **Consensus Mechanism**
    
    -   Bitcoin uses Proof of Work (PoW).
        
    -   Ethereum transitioned to **Proof of Stake (PoS)** with **The Merge** to improve energy efficiency and decentralization.
        
-   **Speed & Usage**
    
    -   Ethereum’s faster block times (~12 s) and flexible execution environment make it more suitable for frequent interactions and decentralized services.
        

## **3\. Ethereum’s Evolution**

### **a. The Merge**

Ethereum started as a PoW chain but moved to PoS through an upgrade called **The Merge**. This improved energy efficiency dramatically and shifted block validation responsibility from miners to **validators** who stake ETH in the protocol.

### **b. Beyond The Merge**

Ethereum continues evolving with:

-   **Layer 2 solutions** (Rollups like Optimistic and ZK-Rollups) that increase throughput and reduce network fees.
    
-   **Future data sharding and other protocol upgrades** aimed at scaling without compromising security.
    

## **4\. Ethereum Architecture & Layers**

### **a. Layer 1 (Mainnet)**

-   **Execution Layer**: Where transactions and smart contracts run.
    
-   **Consensus Layer**: Handles block ordering and validator selection under PoS.
    
-   **EVM (Ethereum Virtual Machine)**: A runtime environment that executes smart contract bytecode consistently across all nodes.
    

### **b. Layer 2 (Scaling Solutions)**

Layer 2s process many transactions off-chain and post compressed data back to Layer 1, reducing fees and increasing throughput:

-   **Optimistic Rollups**: Assume validity, verify only when challenged.
    
-   **ZK (Zero-Knowledge) Rollups**: Use proofs to validate correctness.
    

### **c. Sidechains**

Independent chains that can interoperate with Ethereum but have their own security assumptions.

## **5\. Ethereum’s Core Mechanisms**

### **a. Accounts**

Ethereum differentiates two account types:

-   **Externally Owned Accounts (EOAs)**: Controlled by private keys (users).
    
-   **Contract Accounts (CAs)**: Governed by smart contract code.
    

Each account holds:

-   a balance (in ETH);
    
-   a **nonce** (to prevent replay attacks);
    
-   code hash (for contract accounts);
    
-   storage root hash (state of contract storage).
    

### **b. Gas Model**

Every action on Ethereum costs **Gas**, paid in ETH, to prevent abuse and compensate validators:

-   **Gas Limit**: Max units a user will spend.
    
-   **Gas Price**: Fee per unit (often specified in Gwei).
    

Under **EIP-1559**, fees split into:

-   **Base Fee** (burned),
    
-   **Tip** (incentive to validators).
    

* * *

### **c. EVM (Ethereum Virtual Machine)**

The EVM is a decentralized “world computer” that executes smart contract instructions deterministically across all nodes. It’s Turing-complete and ensures all nodes reach the same result from the same inputs.

* * *

## **6\. Ethereum Ecosystem Components**

### **a. Application Layer**

Tools and applications used by end users:

-   Decentralized exchanges (e.g., Uniswap),
    
-   Lending protocols (e.g., Aave, Compound),
    
-   NFT marketplaces,
    
-   Wallets (MetaMask, Rainbow),
    
-   DAO tools (Snapshot, Aragon).
    

### **b. Protocol Layer**

Infrastructure that supports the network:

-   Consensus clients (Prysm, Lighthouse),
    
-   Execution clients (Geth, Erigon),
    
-   EVM and state management.
    

### **c. Scaling Layer**

Solutions improving throughput:

-   Rollups,
    
-   Sidechains.
    

## **7\. Ethereum Culture & Values**

Ethereum’s community and philosophy are shaped by the **cypherpunk ethos**, emphasizing:

-   **Decentralization**: No single entity controls the network.
    
-   **Open participation**: Anyone can use, develop, or deploy on Ethereum.
    
-   **Censorship resistance**: Transactions cannot be arbitrarily blocked.
    
-   **Public goods**: Prioritizing infrastructure and open innovation.
    
-   **Long-term sustainability**: Balancing advancement with stability.
    

## **8\. Typical Lifecycle of an Ethereum Interaction**

1.  **User** initiates an action (transaction or smart contract call).
    
2.  **EOA** signs and broadcasts the transaction.
    
3.  **Validators** include it in a block.
    
4.  **EVM** executes the contract logic.
    
5.  **Gas** fees are consumed and base fee is burned; tip goes to the validator.
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->














# Blockchain Basics

## 1\. What is Blockchain?

Blockchain is a decentralized, distributed ledger technology used to record information securely, transparently, and immutably across multiple participants. It consists of a series of chronologically linked “blocks,” each containing transaction data and the hash of the previous block, ensuring the integrity of the chain.

### Key Features of Blockchain

\- **Immutable**: Once data is written to the chain, it is nearly impossible to alter  
\- **transparen**t: Transactions on the chain are visible to everyone  
\- **Decentralized**: No central authority; the network is maintained by multiple nodes  
\- **Fast Settlement**: Cross-border transactions can be confirmed quickly without banks

Nodes collaborate through a distributed network to maintain the ledger, improving the system’s resistance to interference and trustworthiness.

## 2\. Web3 vs Web2

\- **Web2**: User data and content are controlled by centralized companies  
\- **Web3**: Built on blockchain, emphasizing decentralization and giving users more control and privacy

Core principles of Web3:  
\- Decentralization  
\- Open protocols  
\- User-owned data  
\- Trustworthy cross-platform interactions
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
