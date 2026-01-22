---
timezone: UTC+8
---

# LI LEYA

**GitHub ID:** Leya-LI

**Telegram:** @AlinaLeya

## Self-introduction

大家好，我是Alina。最初是因为课程和身边圈子接触到web3，后来去跑了TOKEN2049和SFF展会，心态慢慢从“好奇看看”变成了想真正扎根在这个行业。 希望通过这次实习，一边系统理解Web3，一边在社区持续输出！

## Notes

<!-- Content_START -->
# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->
【Web3 实习计划（冬季）：AI 及其基础概念】 [https://www.bilibili.com/video/BV1axkwBaEvC/?share\_source=copy\_web&vd\_source=39c8c4bb14001aa820309560be10ccdd](https://www.bilibili.com/video/BV1axkwBaEvC/?share_source=copy_web&vd_source=39c8c4bb14001aa820309560be10ccdd)  
  
这份视频由 Spoon OS 的 Rick 老师主讲，详细探讨了 AI Agent（智能体）与 Web3 技术的深度融合。

以下是视频内容的详细总结：

### 1\. 分享者分享部分：AI 与 Web3 的融合逻辑

Rick 老师从 AI 的演进、当前局限性以及 Web3 如何赋能 AI 三个维度进行了系统分享：

-   **AI 的演进与局限**：
    
    -   **大模型（LLM）的本质**：是一个基于概率的 Token 预测器，虽然像“学霸”一样博学，但由于无状态（不记事）、无工具调用能力、无身份，目前更像是一个高级辅助工具而非独立的执行者。
        
    -   **AI Agent 的崛起**：2026 年的 AI 不再仅是“嘴替”，而是进化为拥有记忆、能调用工具、具备自主身份的“打工人”，能够自主思考并执行任务。
        
-   **AI 经济的“黑户”困境**：
    
    -   当前 AI 在互联网环境下难以像人类一样参与经济活动，面临身份不可验证（谁在发请求？）、行为不可审计（决策逻辑不透明）、支付结算难（缺乏银行账户且微支付成本高）等问题。
        
-   **Web3 的核心解决方案**：
    
    -   **ERC-8004（身份标准）**：为每个 AI 提供一张链上“护照”。基于 NFT 形式，身份全球唯一、可验证且所有权可转移。
        
    -   **ERC-402（支付标准）**：原生微支付协议。支持毫秒级、极低金额（如 0.001 美元）的链上结算，无需 KYC 即可让机器自主完成交易支付。
        
    -   **可验证审计**：利用区块链不可篡改的特性记录 AI 的决策哈希，确保护照和行为记录真实可信，满足未来严格的合规审计需求。
        
-   **Spoon OS 框架**：
    
    -   定位为 AI Agent 的“安卓系统”，通过四层架构（输入、核心、协议、输出）封装复杂的钱包、签名和合规逻辑，让开发者能快速构建具备 Web3 能力的智能体。
        

* * *

### 2\. Q&A 互动部分

Q&A 环节针对 AI 决策责任、技术效率等实战问题进行了深度解答：

-   **AI 交易的必要性**：有学员质疑是否直接托管账户给量化机器人即可。老师回应，Web3 解决的是“可验证性”和“信任”问题，只有具备链上身份，外部资金才能在无需信任项目方的前提下参与 AI 管理的基金。
    
-   **Web3 的效率优势**：针对 Web3 效率是否低于 Web2 的提问。老师解释，对于跨国机器间的高频微额支付，传统金融体系存在严苛审计和高额手续费，而 Web3 通过 Layer 2 等技术能实现极速、低成本结算，更适合机器经济。
    
-   **Agent 身份的一致性**：关于 Agent 升级后身份是否变化。老师表示，目前协议支持通过 NFT 映射关系维持身份延续，也可以根据具体业务逻辑选择通过新合同替代旧身份，取决于开发者的设计。
    
-   **防范女巫攻击**：针对恶意评价的问题，老师指出 ERC-8004 设有信誉系统，用户只有在真正与该 Agent 产生链上交易（支付了成本）后才有权评价，极大地提高了刷分抹黑的门槛。
    
-   **决策责任归属**：如果 AI 造成经济损失谁负责？老师强调，通过链上身份可溯源到其部署者。虽然算法能辅助决策，但最终的法律责任仍落在链上所有权（所有者地址）身上。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->

[https://www.youtube.com/watch?v=11QTT6BK5j0&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=6](https://www.youtube.com/watch?v=11QTT6BK5j0&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=6)

This video focuses on advanced Web3 user skills, covering troubleshooting transactions, utilizing Layer 2 (L2) networks, and managing assets with decentralized financial tools.

### **Transaction Management & Troubleshooting**

-   **Stuck Transactions**: If a transaction is stuck with a low gas fee, you can use the **"Speed Up"** button in MetaMask to resubmit it with a higher bid \[[12:15](http://www.youtube.com/watch?v=11QTT6BK5j0&t=735)\].
    
-   **Canceling Transactions**: To cancel a pending transaction, you send a new transaction to yourself for 0 ETH using the **same Nonce** but a higher gas fee \[[19:12](http://www.youtube.com/watch?v=11QTT6BK5j0&t=1152)\].
    
-   **Resetting MetaMask**: If the wallet becomes confused with incorrect nonces or chain IDs, using the **"Reset Account"** feature clears the local activity history without affecting your funds \[[15:57](http://www.youtube.com/watch?v=11QTT6BK5j0&t=957)\].
    
-   **The Nonce**: This is a sequential transaction counter for your address \[[10:13](http://www.youtube.com/watch?v=11QTT6BK5j0&t=613)\]. Transactions must be mined in order; for example, transaction #6 must finish before #7 can start \[[10:26](http://www.youtube.com/watch?v=11QTT6BK5j0&t=626)\].
    

### **L2 Applications and Platforms**

The video highlights the speed and cost advantages of Layer 2 networks, specifically using **Optimism**, which settles to Ethereum mainnet for security but offers much cheaper transactions \[[31:29](http://www.youtube.com/watch?v=11QTT6BK5j0&t=1889)\].

| Website Name | Purpose / Use Case |
| Optimism Bridge | Used to transfer ETH from the Ethereum mainnet (L1) to the Optimism network (L2) [30:21]. |
| Aave | A lending and borrowing platform where you can provide assets as collateral to take out loans [38:11]. |
| Matcha | A decentralized exchange (DEX) aggregator that finds the cheapest paths for swapping tokens across multiple platforms [41:28]. |
| Uniswap | A popular DEX for swapping tokens; it is also used via Layer 2 to demonstrate faster, cheaper swaps [45:04]. |
| Quixotic | An NFT marketplace specifically built on the Optimism network for buying and selling digital assets [36:26]. |
| Safe (formerly Gnosis Safe) | A multi-signature (multisig) smart contract wallet that requires multiple people to approve a transaction before it can execute [01:00:34]. |

### **Advanced Concepts**

-   **Account Abstraction & Multisigs**: The **Safe** allows for "M-of-N" security (e.g., 2 out of 4 signers), protecting funds even if one individual's seed phrase is leaked \[[01:03:50](http://www.youtube.com/watch?v=11QTT6BK5j0&t=3830)\].
    
-   **WalletConnect**: A protocol that allows you to connect a mobile wallet (like Rainbow) or a multisig safe to a desktop browser application by scanning a QR code or pasting a link \[[01:12:08](http://www.youtube.com/watch?v=11QTT6BK5j0&t=4328)\].
    
-   **ERC-20 Approval Pattern**: Unlike sending ETH, sending tokens (like DAI) requires a two-step process: first, an **"Approve"** transaction to let the contract spend your tokens, followed by the actual swap or transfer \[[43:05](http://www.youtube.com/watch?v=11QTT6BK5j0&t=2585)\].
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->


This video from the **Web2 to Web3** curriculum series focuses on **Non-Fungible Tokens (NFTs)**, explaining their fundamental concepts, technical standards, and how users interact with them in the Ethereum ecosystem. **👩‍💻 Web2 to Web3 🚀 - Week 1 Day 4 - 🎫 NFTs!!! ERC20 vs ERC721, IPFS, Metadata** [**https://www.youtube.com/watch?v=NOdrEpnoCiM&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=5**](https://www.youtube.com/watch?v=NOdrEpnoCiM&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=5)

### **Core Concepts of NFTs**

-   **Definition**: NFTs represent digital ownership with on-chain provenance, allowing artists to mint unique collections that can be verified as authentic \[[03:44](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=224)\].
    
-   **Digital Scarcity**: Unlike fungible tokens (like ETH), NFTs have a capped supply specified in a smart contract, ensuring only a certain amount can ever be minted \[[05:30](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=330)\].
    
-   **Digital Identity & Inventory**: Your inventory and identity follow you across different Web3 services (e.g., from an ENS registration to a marketplace like OpenSea) \[[01:43](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=103)\].
    

### **NFT Technical Standards**

-   **ERC-20 vs. ERC-721**:
    
    -   **ERC-20 (Fungible)**: Tracked as a balance in a contract; sending a token means subtracting from one balance and adding to another \[[50:26](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=3026)\].
        
    -   **ERC-721 (Non-Fungible)**: Tracked by owner per individual asset ID; each token is a unique object with its own owner \[[50:26](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=3026)\].
        
-   **Metadata & Storage**:
    
    -   **Off-Chain (IPFS)**: Most NFTs store their metadata and images on the InterPlanetary File System (IPFS) to be "content-addressable," meaning the address changes if the content changes \[[43:21](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=2601)\].
        
    -   **On-Chain (SVG)**: Some NFTs (like "Loogies") render their artwork (SVG) directly from the smart contract, making them more composable and truly decentralized, though more expensive to mint \[[56:59](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=3419)\].
        

### **Marketplaces & Minting**

-   **OpenSea**: A major marketplace where users can buy, sell, and auction NFTs; it serves as a decentralized inventory that follows your wallet \[[01:50](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=110)\].
    
-   **The Minting Process**: "Minting" is the act of creating the token directly from a smart contract \[[19:12](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=1152)\]. This can happen on a project's website or directly through a block explorer like Etherscan \[[28:06](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=1686)\].
    
-   **Trust & Verification**: Verified contracts on Etherscan allow users to read the code and understand the rules (like total supply) before interacting \[[30:10](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=1810)\].
    

### **Utility and Creative Empowerment**

-   **Utility**: Beyond art, NFTs can provide "token gating" (access to chat rooms or voting rights) \[[09:15](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=555)\].
    
-   **Artist Empowerment**: NFTs allow creators to connect directly with their audience, monetize "super fans," and build in permanent royalties that are automatically paid out on secondary sales \[[08:36](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=516)\].
    

### **Key Technical Demos**

-   **IPFS Interaction**: Using a hash to retrieve content from the peer-to-peer storage network \[[37:01](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=2221)\].
    
-   **Interacting with Contracts**: Using Etherscan's "Read" and "Write" functions to check balances or mint tokens without a frontend website \[[36:17](http://www.youtube.com/watch?v=NOdrEpnoCiM&t=2177)\].
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->



In this video, "Web2 to Web3 - Week 1, Day 3," Austin Griffith and his team demonstrate how to use an Ethereum wallet to interact with various Web3 services, emphasizing how your identity and inventory follow you across the decentralized web.

### **1\. Identity & Naming (ENS)**

-   **The Problem:** Standard Ethereum addresses are long hex strings (e.g., `0xae94...`) that are difficult to remember and share \[[03:01](http://www.youtube.com/watch?v=wYSMNdIRoII&t=181)\].
    
-   **The Solution:** The **Ethereum Name Service (ENS)** maps these addresses to human-readable names like `sanfordstout.eth` \[[06:23](http://www.youtube.com/watch?v=wYSMNdIRoII&t=383)\].
    
-   **Registration Process:**
    
    -   **Two-Step Transaction:** Registration uses a "Commit-Reveal" scheme to prevent others from stealing your name during the process. You first "commit" to the name, wait one minute, and then "reveal" and pay to complete the registration \[[15:30](http://www.youtube.com/watch?v=wYSMNdIRoII&t=930)\].
        
    -   **Primary Names:** Setting a "Primary ENS Name" (reverse record) allows apps to display your name instead of your hex address when you connect your wallet \[[23:11](http://www.youtube.com/watch?v=wYSMNdIRoII&t=1391)\].
        
-   **ENS as an NFT:** Every registered ENS name is actually an NFT owned by the wallet address \[[21:17](http://www.youtube.com/watch?v=wYSMNdIRoII&t=1277)\].
    

### **2\. Decentralized Exchanges (DEX)**

-   **CEX vs. DEX:** Centralized exchanges (like Coinbase) are custodial and rely on an internal database. Decentralized exchanges (like Uniswap) are **non-custodial smart contracts** that allow peer-to-peer trading without a middleman \[[30:37](http://www.youtube.com/watch?v=wYSMNdIRoII&t=1837)\].
    
-   **The "Vending Machine" Concept:** Uniswap acts like an unstoppable vending machine where you put one token in (e.g., ETH) and get another out (e.g., DAI) based on the current ratio in the liquidity pool \[[32:16](http://www.youtube.com/watch?v=wYSMNdIRoII&t=1936)\], \[[48:20](http://www.youtube.com/watch?v=wYSMNdIRoII&t=2900)\].
    
-   **Liquidity Providers:** Anyone can provide tokens to these pools to earn trading fees, meaning the exchange is run by the community rather than a single entity \[[33:12](http://www.youtube.com/watch?v=wYSMNdIRoII&t=1992)\].
    

### **3\. Stablecoins (DAI)**

-   **Managing Volatility:** Holding ETH is risky due to price fluctuations. Stablecoins like **DAI** are pegged to $1 to provide a better user experience for payments \[[37:36](http://www.youtube.com/watch?v=wYSMNdIRoII&t=2256)\].
    
-   **Mechanics:** DAI is maintained through **over-collateralized loans**. Users lock up ETH in a vault to mint DAI. If the value of the ETH drops too low, the vault is liquidated to ensure the DAI remains backed by value \[[40:09](http://www.youtube.com/watch?v=wYSMNdIRoII&t=2409)\].
    

### **4\. Portability & Privacy**

-   **Universal Inventory:** Because your tokens and NFTs are tied to your address on the blockchain, any Web3 app you visit (like [Zapper.fi](http://Zapper.fi)) can instantly see your entire history and inventory once you connect or search for your name \[[35:52](http://www.youtube.com/watch?v=wYSMNdIRoII&t=2152)\].
    
-   **Sybil Resistance:** Since anyone can create infinite addresses (identities) for free, Web3 developers use "reputation scores" (e.g., voting history, account age) to determine if an account belongs to a real human or a bot \[[46:37](http://www.youtube.com/watch?v=wYSMNdIRoII&t=2797)\].
    

**Video URL:** [https://www.youtube.com/watch?v=wYSMNdIRoII](https://www.youtube.com/watch?v=wYSMNdIRoII)

**Sybil attack**

In the context of the video and Web3, a **Sybil attack** refers to a situation where one person or entity creates multiple fake identities (or Ethereum addresses) to gain unfair influence or subvert a system \[[37:09](http://www.youtube.com/watch?v=wYSMNdIRoII&t=2229)\].

Here are the key points about Sybil from the video:

-   **The Problem:** Because creating a new Ethereum address is free and instant, anyone can generate thousands of accounts. This makes it very difficult to implement "one person, one vote" systems, as a single human can pretend to be many people \[[36:54](http://www.youtube.com/watch?v=wYSMNdIRoII&t=2214)\].
    
-   **Civil Resistance:** This is an unsolved problem in Web3. Developers spend a lot of time figuring out how to prevent bots or bad actors from overwhelming decentralized systems with fake identities \[[37:15](http://www.youtube.com/watch?v=wYSMNdIRoII&t=2235)\].
    
-   **Reputation Scoring:** To combat Sybil attacks, projects often use "humanity scoring." They look at an account's history to see if it has done "human-like things," such as voting in governance (Snapshot), signing multi-sig messages, or having long-term activity, rather than just acting like a bot \[[46:37](http://www.youtube.com/watch?v=wYSMNdIRoII&t=2797)\].
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->




### **1\. Getting Started with MetaMask**

-   **Official Installation:** Go to [**metamask.io**](http://metamask.io) and install the Chrome extension \[[01:21](http://www.youtube.com/watch?v=_GjPeRLCREA&t=81)\]. Be extremely cautious of phishing sites with similar URLs \[[01:52](http://www.youtube.com/watch?v=_GjPeRLCREA&t=112)\].
    
-   **The Fox Icon:** The animated 3D fox in MetaMask acts as a subtle security feature that is difficult for simple phishing sites to replicate \[[02:15](http://www.youtube.com/watch?v=_GjPeRLCREA&t=135)\].
    
-   **Local Password vs. Seed Phrase:** You will set a password, but this is only for **local encryption** on your device \[[03:30](http://www.youtube.com/watch?v=_GjPeRLCREA&t=210)\]. If you lose this password, you can still recover your wallet using your seed phrase \[[03:36](http://www.youtube.com/watch?v=_GjPeRLCREA&t=216)\].
    

### **2\. The 12-Word Seed Phrase (Mnemonic)**

-   **Your Master Key:** This 12-word phrase is the master key to your entire wallet \[[02:44](http://www.youtube.com/watch?v=_GjPeRLCREA&t=164)\]. Anyone with these words can empty your account \[[05:59](http://www.youtube.com/watch?v=_GjPeRLCREA&t=359)\].
    
-   **Safe Storage Practices:**
    
    -   **Do:** Write it on paper and store it in a secure physical location (e.g., a "sock drawer" or safe) \[[06:11](http://www.youtube.com/watch?v=_GjPeRLCREA&t=371)\].
        
    -   **Do:** Use a reputable password manager (e.g., 1Password) \[[07:03](http://www.youtube.com/watch?v=_GjPeRLCREA&t=423)\].
        
    -   **Don't:** Take screenshots, store it in your phone's notes, or send it via messaging apps like WhatsApp \[[06:39](http://www.youtube.com/watch?v=_GjPeRLCREA&t=399)\].
        
-   **Brute Force Immunity:** While mathematically possible to guess a phrase, the entropy is so high that it would take a trillion computers guessing a trillion times per second the entire lifetime of the universe to find a specific account \[[48:32](http://www.youtube.com/watch?v=_GjPeRLCREA&t=2912)\].
    

### **3\. Understanding Key Pairs & Addresses**

-   **Private Keys:** Derived from your seed phrase, this is a 64-character hex string that signs transactions \[[37:54](http://www.youtube.com/watch?v=_GjPeRLCREA&t=2274)\].
    
-   **Public Address:** Derived from the private key; this is what you share with others to receive funds \[[16:03](http://www.youtube.com/watch?v=_GjPeRLCREA&t=963)\].
    
-   **Deterministic Nature:** One seed phrase can generate an infinite number of accounts (Account 1, Account 2, etc.). These will always be the same every time you restore that specific seed phrase \[[30:59](http://www.youtube.com/watch?v=_GjPeRLCREA&t=1859)\].
    

### **4\. Transactions and Block Explorers**

-   **Disinterested Third Parties:** Use **Etherscan** to verify transactions independently of your wallet software \[[23:39](http://www.youtube.com/watch?v=_GjPeRLCREA&t=1419)\].
    
-   **No "Undo" Button:** Once a transaction is mined on the blockchain, it cannot be reversed. There is no customer support to call \[[01:06:47](http://www.youtube.com/watch?v=_GjPeRLCREA&t=4007)\].
    
-   **Gas Fees:** You pay "gas" to have your transaction processed. On Mainnet, this is real money; on Testnets (like Kovan or Goerli), it is free \[[18:26](http://www.youtube.com/watch?v=_GjPeRLCREA&t=1106)\].
    

### **5\. Custodial vs. Non-Custodial**

-   **Custodial (Exchanges):** Services like Coinbase own the keys and manage your balance in their database. You are reliant on their security \[[01:04:15](http://www.youtube.com/watch?v=_GjPeRLCREA&t=3855)\].
    
-   **Non-Custodial (MetaMask/Hardware):** You own the keys. "Not your keys, not your coins" \[[01:04:36](http://www.youtube.com/watch?v=_GjPeRLCREA&t=3876)\]. This offers more control but places 100% of the security responsibility on you \[[01:07:05](http://www.youtube.com/watch?v=_GjPeRLCREA&t=4025)\].
    

### **6\. Advanced Security Options**

-   **Hardware Wallets:** Physical devices (like Ledger or Trezor) that keep private keys offline and air-gapped \[[01:12:35](http://www.youtube.com/watch?v=_GjPeRLCREA&t=4355)\].
    
-   **Smart Contract Wallets (Multi-sig):** Wallets like Gnosis Safe or Argent that require multiple signatures (e.g., 2-of-3) to move funds, providing a "2FA" feel for large sums of money \[[01:14:29](http://www.youtube.com/watch?v=_GjPeRLCREA&t=4469)\].
    

[https://www.youtube.com/watch?v=\_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->





**Web2 to Web3 - Day 1 Intro Notes**

-   **Course Structure:**
    
    -   **Week 1 (Power User):** Wallets, Gas, dApps, NFTs, ERC20/721.
        
    -   **Week 2 (Scripting):** Providers, Signers, Ethers.js, Hardhat.
        
    -   **Week 3 (Speedrun):** Building apps like Staking, Token Vendors, and Dice Games.
        
    -   **Week 4:** Open-ended build phase.
        
-   **Key Concepts:**
    
    -   **Trustless Escrow:** Smart contracts allow parties (Alice & Bob) to trade without trusting each other.
        
    -   **Incentives:** Essential for decentralized systems (e.g., paying users to trigger “cron jobs”).
        
    -   **Unstoppable Code:** Once deployed, smart contracts cannot be stopped or censored.
        
-   **Success Paths:**
    
    1.  Get hired by a Web3 organization.
        
    2.  Build and launch your own product.
        
    3.  Become a Smart Contract Auditor (Level 3).
        

[https://www.youtube.com/watch?v=zuJ-elbo88E](https://www.youtube.com/watch?v=zuJ-elbo88E)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
