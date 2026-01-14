---
timezone: UTC+0
---

# yishengxu777-sketch

**GitHub ID:** yishengxu777-sketch

**Telegram:** 

## Self-introduction

AI + Web3 believer + INFP

## Notes

<!-- Content_START -->
# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
![NotebookLM Mind Map.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yishengxu777-sketch/images/2026-01-14-1768415061600-NotebookLM_Mind_Map.png)

# Briefing on Web3 Infrastructure and Application Layers

## Executive Summary

This document synthesizes key insights into the foundational principles, architectural components, and practical applications of Web3 technologies. The central challenge addressed is the **Blockchain Oracle Problem**: how to securely connect isolated, deterministic blockchains with non-deterministic, real-world data. The solution lies in **Decentralized Oracle Networks (DONs)**, such as the industry-standard Chainlink, which enable the creation of **Hybrid Smart Contracts** that combine on-chain security with off-chain data and computation. This model is now fundamental to most major Web3 protocols.

For participants, security is paramount, shifting from institutional responsibility to individual diligence under the ethos of **"Don't trust, verify."** The absolute control and security of personal assets depend entirely on the offline, physical protection of private keys and mnemonic phrases. This individual responsibility is set against a backdrop of an evolving regulatory landscape, particularly in jurisdictions like China, which exhibit a policy of "comprehensive blocking of financial attributes, limited tolerance for technological innovation."

The core architecture of Ethereum distinguishes between user-controlled **Externally Owned Accounts (EOAs)**, which initiate all transactions, and code-driven **Contract Accounts (CAs)**. This system is the foundation for advanced applications like on-chain identity via the **Ethereum Name Service (ENS)** and **Decentralized Exchanges (DEXs)** like Uniswap. The evolution from Uniswap V2 to V3 highlights a key industry trend: a move towards greater capital efficiency through mechanisms like **concentrated liquidity**. While this advancement offers superior returns and lower slippage for traders, it demands more sophisticated, active management from liquidity providers.

\--------------------------------------------------------------------------------

## 1\. The Blockchain Oracle Problem: Bridging Digital and Physical Worlds

The core utility of blockchain technology is severely limited by its intrinsic design as a closed system. The "Oracle Problem" refers to the fundamental challenge of securely feeding external, off-chain data into the deterministic on-chain environment without compromising the system's core principles of decentralization and trustlessness.

### 1.1. Core Concepts: Blockchains and Smart Contracts

-   **Blockchain:** A decentralized, trustless source of truth. Its distributed ledger architecture ensures the system is transparent, permissionless, and censorship-resistant.
    
-   **Smart Contracts:** Decentralized and immutable protocols that run on the blockchain. They execute deterministically based on their code, with the principle that **"code itself is law,"** eliminating the need for intermediaries.
    

### 1.2. The Challenge of Isolation and Determinism

The primary strength of blockchains—their secure isolation—is also their greatest limitation. These closed systems can only interact with data already present on the chain (on-chain data). They have no native capability to access real-world information (off-chain data), such as weather data for insurance contracts, asset prices for DeFi protocols, or sports results for prediction markets.

The root of this limitation is **determinism**, a core blockchain principle requiring that a given input always produces the identical output. This ensures that thousands of nodes across the network can process a transaction and reach a consensus on the same resulting state. Real-world data from sources like APIs is inherently non-deterministic; data can change in milliseconds, leading to different nodes receiving slightly different inputs and breaking the consensus mechanism.

### 1.3. The Solution: Decentralized Oracle Networks (DONs)

An **oracle** is a service that acts as a bridge, fetching off-chain data and delivering it to smart contracts. However, a single, centralized oracle reintroduces the very problems blockchain was designed to solve: single points of trust and failure.

The robust solution is a **Decentralized Oracle Network (DON)**, which operates as follows:

1.  A smart contract requests data from the network.
    
2.  Multiple independent oracle nodes fetch the same data from various high-quality, real-world sources.
    
3.  The nodes validate the data among themselves.
    
4.  A consensus mechanism is used to agree upon a single, aggregated result.
    
5.  This verified data is then delivered on-chain for the smart contract to use.
    

This decentralized process guarantees that the data fed into the blockchain is accurate, reliable, and resistant to manipulation.

### 1.4. The Rise of Hybrid Smart Contracts

The combination of secure on-chain smart contract logic with reliable off-chain data provided by a DON creates a **Hybrid Smart Contract**. This architecture leverages the best of both worlds:

-   **On-Chain Component:** The core smart contract logic, providing decentralization, security, and transparency.
    
-   **Off-Chain Component:** The DON, providing access to real-world data and computational capabilities.
    

This hybrid model has become the standard for building practical, real-world Web3 applications, from DeFi to blockchain gaming. The industry-leading infrastructure for this is **Chainlink**, a modular, blockchain-agnostic DON integrated with numerous blockchains including Ethereum, Avalanche, Polygon, and zkSync.

## 2\. Security and Compliance in the Web3 Ecosystem

The shift to a decentralized paradigm necessitates a fundamental change in user mindset, prioritizing personal responsibility for security and an awareness of the complex legal landscape.

### 2.1. Guiding Philosophy and Personal Security

-   **Core Principle:** The Web3 ethos is **"Don't trust, verify."** Security is not a platform's responsibility but a fundamental skill required of every participant.
    
-   **Private Key Primacy:** The private key or mnemonic phrase represents the sole control over a user's assets. If lost or compromised, funds are irretrievable.
    
-   **Storage Best Practices:** It is imperative to avoid storing keys or phrases in screenshots, cloud services, or any internet-connected device. The recommended method is physical backup, such as writing them on paper and storing them in a secure location like a safe deposit box.
    

### 2.2. Common Threats and Mitigation

-   **Phishing Attacks:** Users must remain vigilant against private messages on platforms like Telegram and Discord, as well as fake website links. DApps should only be accessed through official channels.
    
-   **Impersonation:** Legitimate Web3 projects will never proactively ask for a user's mnemonic phrase or request a transfer of funds.
    
-   **Malicious Signatures:** Before confirming any transaction in a wallet, users must carefully verify the destination address and the scope of permissions (Allowance) being granted to prevent malicious smart contracts from draining assets.
    

### 2.3. The Regulatory Landscape

-   **China's Regulatory Stance:** The current regulatory framework in China is characterized by a policy of **"comprehensive blocking of financial attributes, limited tolerance for technological innovation."** This creates significant legal and criminal risks for those involved in projects with financial elements.
    
-   **Global Divergence:** Policies on cryptocurrencies vary significantly between nations, including the United States, Singapore, and Dubai.
    
-   **Trend Towards Compliance:** The Web3 industry is transitioning from "wild growth" to a more regulated environment. Key areas of focus include stablecoin legislation, asset compliance audits, Anti-Money Laundering (AML), and Know Your Customer (KYC) mechanisms.
    

## 3\. Ethereum's Foundational Architecture

Understanding the Ethereum account system and its security models is essential for interacting with the "world computer."

### 3.1. Account Types and Key Management

Ethereum features two distinct types of accounts:

| Account Type | Description | Key Features |
| Externally Owned Account (EOA) | Controlled by a user via a private key (e.g., a MetaMask wallet). | - Is the sole initiator of all on-chain transactions.<br>- Stores an ETH balance. |
| Contract Account (CA) | Controlled by the code deployed within it. | - Has no private key and is driven by code logic.<br>- Can store an ETH balance and complex state data (EVM bytecode).<br>- Can only act when called upon by an EOA or another CA. |

-   **Address Generation:** An Ethereum address (prefixed with `0x`) is derived from the last 20 bytes of a Keccak-256 hash of a public key.
    
-   **Mnemonic Phrase:** The 12 or 24-word phrase is the "master key" from which an infinite number of private keys can be derived. Securing the mnemonic phrase is the highest priority.
    

### 3.2. Smart Contract Deployment and Address Calculation

-   **CREATE Opcode:** The traditional method for contract deployment. The resulting contract address is calculated using a hash of the sender's address and their transaction count (nonce). This makes the address difficult to predetermine.
    
-   **CREATE2 Opcode:** An advanced opcode that allows for pre-computation of a contract address. The address is calculated from a hash of a constant (`0xff`), the sender's address, a custom value (**salt**), and the contract's code. This enables "counterfactual instantiation," where an address is known and can be interacted with before the contract is deployed.
    

### 3.3. Advanced Security Mechanisms

-   **Multisig (Multi-signature):** A security model requiring M-of-N participants to approve a transaction.
    
    -   **Benefit:** Prevents a single point of failure or compromise from leading to a loss of funds.
        
    -   **Drawback:** Reduces operational efficiency and can cause critical delays in emergencies.
        
-   **Timelock:** A mechanism that enforces a mandatory waiting period for sensitive administrative actions, such as protocol upgrades.
    
    -   **Benefit:** Provides a "cool-down" period, giving users time to review changes and react to potential risks.
        
    -   **Drawback:** The forced delay can be a fatal flaw when an immediate fix is needed to counter an active exploit.
        

## 4\. On-Chain Identity and Decentralized Finance (DeFi)

Web3 applications demonstrate powerful interoperability, allowing user identity and assets to move seamlessly across the ecosystem.

### 4.1. On-Chain Identity: Ethereum Name Service (ENS)

ENS functions as the DNS of Web3, mapping complex hexadecimal addresses (e.g., `0xae9...`) to human-readable names (e.g., `sanfordstout.eth`).

-   **Commit-Reveal Registration:** To prevent front-running (where an attacker sees a pending registration and registers the name first), ENS uses a two-step process:
    
    1.  **Commit:** A user submits a hash of the desired name, hiding the name itself but proving intent.
        
    2.  **Reveal:** After a waiting period (e.g., one minute), the user reveals the name in a second transaction to complete the registration.
        
-   **Reverse Resolution:** This critical feature allows DApps to look up a user's address and display their corresponding ENS name, creating a portable identity that follows the user across the ecosystem.
    

### 4.2. Decentralized Exchanges (DEXs) and Stablecoins

-   **DEX vs. CEX:** Unlike Centralized Exchanges (CEX) like Coinbase, which operate on internal ledgers, DEXs like Uniswap are smart contracts running directly on the blockchain, enabling peer-to-peer token swaps without an intermediary.
    
-   **Interoperability:** When a user with an ENS name connects to Uniswap, their name (e.g., `sanfordstout.eth`) automatically appears, demonstrating how identity and applications are natively linked.
    
-   **DAI Stablecoin:** An example of a decentralized stablecoin that maintains its peg to the US dollar through over-collateralization. Users lock collateral (e.g., $100 of ETH) to borrow a smaller amount of DAI (e.g., $60). If the collateral's value drops, an automated liquidation process sells the collateral to ensure every DAI remains backed by sufficient assets.
    

### 4.3. Portable Inventory and Sybil Attacks

-   **User-Owned Inventory:** Tools like Zapper demonstrate that a user's assets and transaction history ("inventory") are tied to their wallet, not a platform. This inventory is automatically visible on any DApp the user connects to.
    
-   **Sybil Attack:** A significant challenge in permissionless systems where one individual can create thousands of fake accounts to illegitimately influence governance votes or claim airdrops. A common mitigation strategy is to use on-chain behavior scoring to differentiate "real" users from bots based on their transaction history and protocol interactions.
    

## 5\. The Evolution of AMMs: Uniswap V2 vs. V3

Uniswap V3 represents a significant paradigm shift in automated market maker (AMM) design, moving from a passive to an active liquidity provision model to dramatically increase capital efficiency.

### 5.1. Core Distinction: Concentrated Liquidity

-   **Uniswap V2 (Passive Liquidity):** Liquidity providers (LPs) deposit assets that are distributed evenly across the entire price curve from `[0, ∞)`. This is simple to manage but highly capital inefficient, as most funds are unused at any given time.
    
-   **Uniswap V3 (Active, Concentrated Liquidity):** LPs can provide liquidity within specific, custom price ranges `[Pa, Pb]`. This concentrates capital where most trading occurs, dramatically increasing efficiency and fee generation for LPs.
    

### 5.2. Technical Implementation: Ticks and TickSpacing

V3 achieves concentrated liquidity by discretizing the price curve into granular price points called **Ticks**.

-   **Ticks:** Each tick corresponds to a specific price, calculated as `p(i) = 1.0001^i`, where `i` is the tick index. This allows price calculations to be handled efficiently with integer math.
    
-   **Liquidity Position:** An LP's position is defined by a lower tick and an upper tick, and their liquidity is only active for trading when the market price is between these two boundaries.
    
-   **TickSpacing:** To optimize performance, LPs cannot choose any tick as a boundary. They must select from ticks whose indices are divisible by a `tickSpacing` value (e.g., 60 for a 0.3% fee pool). This quantizes the available price ranges.
    

### 5.3. Impact on Ecosystem Participants

| Participant | Uniswap V2 Experience | Uniswap V3 Experience |
| Liquidity Provider (LP) | - Simple, "set-and-forget" experience.<br>- Low capital efficiency; fees are diluted across the entire price range.<br>- Constant exposure to impermanent loss. | - Requires active position management.<br>- Far higher fee earnings when price is within the selected range.<br>- If price moves out of range, fee earning stops and impermanent loss is fully realized. |
| Trader | - Liquidity exists at all price points.<br>- Generally higher slippage on trades. | - Extremely deep liquidity and low slippage near the current market price.<br>- Potential for sharp slippage increases if a large trade pushes the price across multiple liquidity ranges. |

### 5.4. Security and Oracle Improvements

-   **Mitigation of Sandwich Attacks:** V3 does not eliminate sandwich attacks but significantly raises their cost. The high concentration of liquidity means a much larger trade is required to create enough slippage for an attack to be profitable, rendering many smaller attacks economically unviable.
    
-   **Upgraded TWAP Oracle:** V3's Time-Weighted Average Price (TWAP) oracle is a major improvement over V2's.
    
    -   **On-Chain Observations:** The V3 contract natively stores an array of cumulative price and liquidity data, eliminating the need for external data snapshots.
        
    -   **Manipulation Resistance:** It uses a geometric mean price calculation, which is more resilient to single-block price manipulation attacks.
        
    -   **Cost Efficiency:** External protocols can query historical average prices in a single, low-cost call, making the V3 oracle a highly reliable and foundational tool for the broader DeFi ecosystem.
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

![e9a72981-2250-43da-9ec4-f45fb687bd33.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yishengxu777-sketch/images/2026-01-13-1768335171707-e9a72981-2250-43da-9ec4-f45fb687bd33.png)

  
1\. 关于以太坊架构

**Q: 为什么现在的全节点不默认保存所有历史状态？**

-   **A:** 这是一个存储效率与验证需求的平衡。**全节点**只需要保存“最新状态”即可验证新交易的合法性；只有**归档节点**才需要保存所有历史时刻的状态快照（用于查询“这一笔交易发生前的余额是多少”）。如果全节点都跑归档模式，普通人的电脑硬盘根本装不下（TB级别）。
    

2\. 关于 Uniswap V2 闪电贷

**Q: 代码中并没有** `flashLoan` **这个函数，它是如何实现的？**

-   **A:** 它是“寄生”在 `swap` 函数中的。
    
    1.  **乐观转账：** 用户请求输出代币，合约**先**把钱转给用户。
        
    2.  **回调检查：** 代码检查 `data` 参数是否有值。如果有，触发 `uniswapV2Call`（这给了用户操作资金的时间窗口）。
        
    3.  **最终审判：** 回调结束后，强制检查 $K$ 值（$x \\times y \\geq k$）。如果用户没有还钱（或没给够手续费），导致 $K$ 值下降，交易直接 Revert。
        

3\. 关于 `sync` 和 `skim`

**Q: 既然** `x * y = k` **自动管理余额，为什么还需要这两个手动函数？**

-   **A:** 因为以太坊上有两种转账方式：
    
    1.  **正常 Swap：** 合约知道余额变了。
        
    2.  **强制打币（误操作或空投）：** 有人直接把 USDT 转到了 Uniswap 合约地址，没有经过 swap 函数。这时，合约记录的 `reserve` 变量和链上真实的 `balanceOf` 就不一致了。
        
    
    -   `sync`：把记录更新，以此为准（做慈善，把多出来的币加进池子）。
        
    -   `skim`：把多出来的币提走（保护机制，防止代币卡死）。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->


非常感谢能够参加这个实习计划，也再次表达对创始人Bruce和各位老师付出的时间精力表示感谢！非常认可大家提到的愿景，先生大义！

# Web3 入门学习笔记（Day 1）

## 一、Web2 与 Web3 的核心区别

| 维度 | Web2 | Web3 |
| 数据所有权 | 平台所有 | 用户所有 |
| 身份体系 | 平台账号 | 钱包地址 |
| 信任机制 | 信任机构/平台 | 信任代码 |
| 价值流转 | 间接、平台控制 | 原生、点对点 |

## 二、区块链核心概念

### 1\. 什么是区块链

区块链是一种 **去中心化的分布式账本技术**，由按时间顺序连接的区块组成，每个区块包含交易数据和元数据，并通过哈希值与前一区块相连，形成一条不可篡改的链。

### 2\. 区块链的关键特性

·        **不可篡改性**：修改历史数据需重算所有后续区块，几乎不可能

·        **公开透明**：交易记录对所有人可验证

·        **匿名性**：地址公开但身份未知

·        **跨地域高效性**：不受地理限制，跨境转账效率高

### 3\. 分布式网络与去中心化

·        全球大量节点共同记账

·        每个节点保存完整账本

·        无单点控制，数据永久存在

·        **51% 攻击阈值**是安全性的核心来源

## 三、区块链的类型

·        **公链**：完全开放（如 Bitcoin、Ethereum）

·        **私链**：组织内部使用

·        **联盟链**：多机构共同维护（如银行系统）

公链去中心化程度最高，但性能与效率存在权衡。

## 四、去中心化的优势与挑战

### 优势

·        抗审查

·        数据主权归用户

·        高透明度

·        全球无许可访问

·        可组合性与互操作性

### 挑战

·        可扩展性问题

·        用户体验复杂

·        监管合规难

·        安全风险

·        能源消耗（特定共识机制）

## 五、以太坊（Ethereum）

### 1\. 以太坊是什么

·        **世界计算机**：不仅记录交易，还能运行代码

·        **去中心化计算平台**：全球节点共同维护，无法被关闭或审查

### 2\. 核心资产：ETH

·        **货币属性**：支付与价值存储

·        **燃料属性（Gas）**：执行交易和合约的手续费

### 3\. 核心概念

·        **钱包（Wallet）**

o   管理私钥，而非真正“存资产”

o   私钥即资产控制权

·        **智能合约（Smart Contract）**

o   链上可执行代码

o   “如果 A，则自动执行 B”

o   部署后不可篡改，公开透明

·        **DApp（去中心化应用）**

o   前端类似普通 App

o   后端逻辑运行在区块链上

o   覆盖 DeFi、NFT、GameFi 等领域

### 4\. Web3 的愿景

·        用户真正拥有资产与数据

·        任何人可无许可参与与构建

## 六、Gas Fee 与智能合约

·        去中心化网络中，每次操作都需要 **Gas Fee**

·        Gas 是网络运行与安全的保障

·        掌握 **Solidity**：

o   编写智能合约

o   定义链上规则

o   自动执行逻辑
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
