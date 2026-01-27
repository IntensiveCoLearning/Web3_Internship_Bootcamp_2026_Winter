---
timezone: UTC+8
---

# Zhibo-Shang

**GitHub ID:** Zhibo-Shang

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->
Today I focused on how Ethereum’s Proof of Stake system works in practice, from a system and engineering perspective.

First, I reviewed Ethereum’s two-layer architecture:

-   The Consensus Layer, which handles validator selection, slot and epoch timing, voting, and finality
    
-   The Execution Layer, which handles transaction execution, state updates, gas accounting, and smart contracts
    

I then walked through the full lifecycle of a block:

1.  A proposer creates a block during its assigned slot by packing transactions
    
2.  The block is broadcast to the network
    
3.  Other validators verify the block’s transactions and state changes
    
4.  Validators send signed attestations
    
5.  These attestations are aggregated and used for fork choice and finality
    

Next, I studied the core operations of a validator, including:

-   Producing block proposals
    
-   Verifying execution results and state roots
    
-   Signing and broadcasting attestations
    
-   Avoiding slashing conditions like double signing
    

I also learned about the real software setup for running a validator node:

-   A consensus client (such as Prysm or Lighthouse)
    
-   An execution client (such as Geth or Nethermind)
    
-   Communication between them through the Engine API
    

Finally, I reviewed specific slashing scenarios, including:

-   Signing two different blocks in the same slot
    
-   Voting on conflicting checkpoints
    

These actions are automatically detected and penalized by the protocol.

Today’s focus was less on theory and more on understanding Proof of Stake as a live distributed system, including message flow, verification logic, and node responsibilities.
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->

Today I went deeper into how Ethereum’s Proof of Stake system actually runs step by step. I learned that time on Ethereum is split into short periods called slots, which last about 12 seconds. A group of slots makes up an epoch. In each slot, one validator is chosen to create a new block.

Other validators don’t just say “yes” or “no” — they send signed messages called attestations to confirm the block and the current chain. These signatures can be combined together using special cryptography, so the network can handle thousands of validators without slowing down too much.

I also learned how Ethereum decides which chain to follow when there are different versions. It uses a rule that follows the chain with the most recent validator support instead of just the longest chain. This helps the network stay in sync even if messages arrive late.

For final security, Ethereum uses a system where validators agree on checkpoints every so often. Once enough validators agree, those blocks become final and are almost impossible to change without losing a lot of staked ETH.

Overall, today helped me see how Ethereum mixes timing, cryptography, and incentives to keep everything running smoothly and securely — not just by trust, but by smart design.
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->


Today I continued exploring Ethereum’s consensus layer by looking more closely at how validators are organized and coordinated across the network. I learned about validator committees and how groups of validators are assigned to specific shards and time slots to improve efficiency and security. This design limits the influence of any single participant and helps the network scale while maintaining decentralization.

I also studied how block proposals are timed using slots and how each epoch contains multiple slots where different validators have opportunities to propose or attest to blocks. Understanding this scheduling system clarified how Ethereum maintains a steady block production rhythm without relying on competitive mining.

Another important concept I learned was the idea of fork choice rules, particularly how the network determines the “head” of the blockchain when multiple possible chains exist. Validators follow specific rules to select the most valid chain based on accumulated attestations, which helps resolve temporary network disagreements.

Overall, today’s session helped me see how Ethereum’s Proof of Stake system is not just about staking ETH, but about coordinated validator behavior, structured timing, and smart consensus rules that keep the blockchain consistent, scalable, and secure.
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->



Today I focused on understanding validators and how proof of stake works in Ethereum. I learned that validators replace miners and are responsible for proposing blocks and voting on them. To become a validator, a user must stake ETH, which acts as a form of security deposit. Validators are randomly selected to propose blocks, and other validators attest to whether the block is valid. If a validator behaves honestly, it earns rewards, but if it behaves incorrectly or stays offline for too long, it can lose part of its stake through penalties or slashing. I also learned that consensus works in rounds and epochs, and that finality is achieved when enough validators agree across multiple rounds. This system encourages good behavior through economic incentives instead of raw computing power. Overall, today’s study helped me understand that Ethereum’s security depends not only on code and cryptography, but also on carefully designed incentives that guide validator behavior and keep the network reliable.
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->




### 1\. Uniswap V2: Core AMM Mechanism

Uniswap V2 is built on the **constant product formula**:

x⋅y=kx \\cdot y = kx⋅y=k

where `x` and `y` are token reserves and `k` remains constant.

-   **Price** is determined by the ratio p=y/xp = y/xp=y/x.
    
-   **Trades** change token reserves, which moves the price.
    
-   **Arbitrage traders** exploit price differences between Uniswap and external markets, helping prices converge to market value.
    

**Liquidity Providers (LPs)** deposit token pairs and earn trading fees, but they face **impermanent loss**, which occurs when token prices change relative to simply holding them. This loss cannot be fully avoided and is usually offset by fee income

Uniswap基础原理

.

Other important mechanisms:

-   **Flash loans**: borrow assets without collateral as long as they are repaid within the same transaction.
    
-   **TWAP (Time-Weighted Average Price)**: provides a manipulation-resistant on-chain price oracle.
    

* * *

### 2\. Uniswap V3: Concentrated Liquidity

Uniswap V3 introduces **capital efficiency improvements** by allowing LPs to provide liquidity only within specific **price ranges**.

Key innovations:

-   **Liquidity ranges**: LPs earn fees only when price stays within their chosen range.
    
-   **Tick system**: discretizes price space to manage liquidity changes efficiently.
    
-   **Liquidity aggregation**: multiple LP positions combine seamlessly.
    
-   **New math model** using liquidity LLL and P\\sqrt{P}P​, reducing computation costs and improving precision.
    

This design increases potential returns but requires more active management and risk control.

* * *

### 3\. Uniswap V4: Flexibility via Hooks

Uniswap V4 introduces **Hooks**, which allow developers to insert custom logic at key points such as:

-   Before/after swaps
    
-   Adding/removing liquidity
    

This enables advanced features like **dynamic fees, limit orders, MEV protection, and custom liquidity strategies**, significantly increasing protocol flexibility

Uniswap基础原理

.

* * *

### 4\. Risks, Fees, and Design Considerations

-   **Impermanent loss** is inherent to AMMs.
    
-   **Price impact** is deterministic and related to pool depth, while **slippage** is unpredictable.
    
-   Fee tiers in V3 should match asset volatility:
    
    -   Stablecoins: 0.01%–0.05%
        
    -   Major assets: 0.3%
        
    -   Volatile assets: 1%
        

MEV risks such as **front-running** can be mitigated using private transaction relays or hook-based logic in V4.

* * *

### 5\. Ecosystem & Developer Support

Uniswap supports developers through:

-   Grants and audit subsidies
    
-   Hackathons and incubators
    
-   Educational and community programs
    

There is no official mobile app, but Uniswap can be accessed via web interfaces and third-party wallets.

* * *

### Key Takeaway

Uniswap evolves from a **simple AMM (V2)** to **capital-efficient liquidity (V3)** and finally to a **highly programmable DeFi infrastructure (V4)**. Understanding its mechanics is essential for DeFi traders, LPs, and protocol developers.

If you want, I can also:

-   Rewrite this as **lecture notes**
    
-   Make a **1-page cheat sheet**
    
-   Simplify it for **non-technical beginners**
    
-   Or turn it into an **English learning diary** (like you often do)
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->





Today I continued learning about Ethereum by focusing on how blocks are created, how transaction fees work, and how a transaction becomes truly safe on the chain. I learned that a block proposer selects transactions from its mempool, usually choosing ones with higher fees first, and puts them into a block that includes a header, transactions, and execution results. Before a block is accepted, the execution client replays all transactions to make sure the results are correct, while the consensus client checks whether the block follows the rules of the proof-of-stake system. I also studied the EIP-1559 fee model and understood that each block has a base fee that goes up when the network is busy and goes down when it is quiet, and that users can add a priority fee as a tip for the proposer. The base fee is burned, and only the tip becomes a reward, and the block gas limit means that not all pending transactions can fit into one block. I learned that a transaction is first included in a block, then gains confirmations as more blocks are added, and finally becomes finalized after enough validators agree, which makes it extremely hard to reverse. This helped me understand why wallets and block explorers show different states like pending, confirmed, and finalized, and overall I realized that Ethereum uses multiple layers of checks and incentives to keep the network secure and stable.
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->






Today I focused on how users and applications actually interact with Ethereum nodes, especially through JSON-RPC, and how a transaction moves through the network from start to finish.

First, I learned more about JSON-RPC.  
JSON-RPC is a simple request–response protocol. A user or application sends a request in JSON format to an Ethereum node, and the node replies with a result or an error.

Some common JSON-RPC methods include:

-   `eth_blockNumber`, which returns the latest block number
    
-   `eth_getBalance`, which checks the balance of an address
    
-   `eth_getTransactionByHash`, which queries a specific transaction
    
-   `eth_call`, which simulates a smart contract call without changing the blockchain
    
-   `eth_sendRawTransaction`, which sends a signed transaction to the network
    

I noticed that most values returned by JSON-RPC are in hex format, and numbers are often very large, so they need special handling in applications.

Then I studied the full lifecycle of a transaction.

A transaction usually starts when a user signs it locally with a private key. The signed transaction is then sent to an execution client through JSON-RPC. The execution client checks basic things such as:

-   the account nonce
    
-   whether the balance is enough
    
-   gas limits and fees
    

If the transaction passes these checks, it enters the mempool (transaction pool). From there, it is spread to other nodes using the gossip protocol.

Block proposers choose transactions from their mempool and include them in a new block. After the block is proposed, both the execution client and the consensus client verify it. Once the block is accepted, the transaction becomes part of the blockchain and is considered confirmed.

After that, I learned more about the mempool itself.

The mempool is not a single global pool. Each node has its own mempool, so different nodes may see slightly different pending transactions. Transactions in the mempool can be:

-   pending (ready to be included)
    
-   queued (waiting because of nonce order or low fees)
    

I also reviewed the basics of EIP-1559, which explains how base fees and priority fees affect transaction speed. This helped me understand why some transactions are fast, while others get stuck or dropped.

Finally, I studied node synchronization modes.

A new node cannot instantly verify the entire blockchain, so different sync methods exist:

-   Full sync verifies everything from genesis, but is very slow
    
-   Snap sync downloads recent state data quickly and then verifies it later
    
-   Light sync only keeps block headers and asks full nodes for data when needed
    

Snap sync allows a node to become usable faster, while still moving toward full verification over time.
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->







### My Study Notes on Ethereum Nodes and Networking

### Today I studied how Ethereum nodes communicate and how different parts of the system work together, especially after the Ethereum Merge.

### First, I learned about RPC (Remote Procedure Call).  
RPC is a computer communication protocol. It allows a program (the client) to call a function or method in another program (the server), usually on another machine over the network. The key idea is that it feels like calling a local function, and the developer does not need to handle network details by hand.

### Then I looked at Web3 Libraries, such as web3.js and [web3.py](http://web3.py).  
These libraries are used to interact with Ethereum nodes. They sit between user applications (like wallets or dApps) and Ethereum nodes, send requests, and receive results.

### I also learned about the two main Ethereum clients:

-   The execution client is responsible for executing smart contracts, updating account balances, and calculating transaction results.
    
-   The consensus client is responsible for blockchain consensus, such as block proposals and validations.
    

### In Web3 libraries, different interfaces connect to different clients:

-   `web3.eth` connects to the execution client.
    
-   `web3.beacon` connects to the consensus client.  
    These interfaces act as communication channels between the Web3 library and the two separate clients.
    

### After that, I studied the Engine API.  
The Engine API is a set of internal JSON-RPC interfaces introduced after the Ethereum Merge. Its job is to connect the execution client and the consensus client, which now run as separate programs but must work closely together.

### Next, I learned about bootnodes (bootstrap nodes).  
Bootnodes are full nodes that stay online for a long time and publish their addresses. When a new node joins the Ethereum network for the first time, it contacts these bootnodes to enter the network.

### Node Discovery Process

### The first step is Discovery, which means how new nodes find other nodes.

1.  Check if a node is online  
    A new node sends a Ping to a bootnode, and the bootnode replies with Pong. This confirms that the node is alive.
    
2.  Ask for contacts (node addresses)  
    The new node sends a FindNode request.  
    The response is:
    
    -   Neighbors in discv4, or
        
    -   Nodes in discv5,  
        which contain contact information of other active nodes.
        

### After getting these neighbors, the new node repeats the same process with them: Ping/Pong and FindNode. Step by step, it fills its own neighbor table until it reaches a set limit.  
This process is based on Kademlia-style DHT (Distributed Hash Table).

### In Ethereum, node lookup works by getting closer to a target node ID at each hop. This greatly reduces broadcast traffic and improves discovery efficiency.

### Building a Secure Connection

### After discovery, nodes build a secure connection using TCP + RLPx / devp2p.

-   TCP is used first to create a stable and reliable connection.
    
-   RLPx runs on top of TCP and allows multiple protocols to be multiplexed inside a single TCP connection.
    

### So overall, Ethereum’s P2P network is built from:

-   UDP-based Node Discovery, and
    
-   TCP-based RLPx/devp2p communication.
    

### Message Propagation

### There are two main propagation models:

1.  Gossip (messages spread gradually through the network)
    
2.  Request–Response (a node asks for specific data and gets a reply)
    

### So node communication can be summarized in three stages:

1.  Discovery stage (UDP + Kademlia)
    
2.  Connection stage (TCP + RLPx/devp2p)
    
3.  Propagation stage (Gossip + request/response)
    

### Types of Ethereum Nodes

### I also studied different types of Ethereum nodes.

### A full node stores:

-   The current blockchain state,
    
-   All block headers and block bodies (transactions and receipts),
    
-   But only keeps the recent states (typically the last ~128 blocks). Older states are removed by pruning to save disk space.
    

### A full node:

-   Runs both an execution client and a consensus client,
    
-   Verifies the blockchain from genesis (or from a trusted checkpoint using snap/fast sync),
    
-   Independently checks whether new blocks and transactions are valid,
    
-   Provides JSON-RPC interfaces for wallets, dApps, and scripts,
    
-   Can theoretically reconstruct any past state, though very old states may require help from archive nodes.
    

### An archive node is a full node without pruning.  
It stores all historical states from genesis to the present.  
These nodes are mainly used by:

-   Block explorers (like Etherscan),
    
-   On-chain data analysis companies,
    
-   Advanced debugging and backtesting tools.
    

### A light node only stores block headers and requests detailed data from full nodes when needed.

### Overall, this study helped me understand that Ethereum is not just a blockchain, but a complex distributed system made of different clients, protocols, and node types, all carefully designed for efficiency, security, and decentralization.
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->








Today I learned about the structure of an Ethereum node after The Merge. After The Merge, a full Ethereum node is no longer one single program. Instead, it is built from two main clients, the execution client and the consensus client. If a user only wants to sync the blockchain and check blocks and transactions, running these two clients is enough. However, if someone wants to produce blocks and earn staking rewards, they also need to run a validator client. The validator client works like a plugin of the consensus client and is used for signing messages, voting, and proposing blocks.

The execution client is responsible for executing all transactions in a block. It calculates account balances and smart contract states and then gives the final execution results. In simple words, the execution client does the math but does not decide which block is the official one. The consensus client does not execute transactions by itself. Instead, it checks the execution results from the execution client and runs the Proof of Stake rules to decide which block is valid and which chain should be accepted as the official chain. This means the execution client gives the results, and the consensus client decides whether to accept them.

The execution client and the consensus client communicate through a special interface called the Engine API. This API is a private connection used only between these two clients. It is not a public RPC or REST API. Through the Engine API, they send block data and execution results to each other. A simple example is that the execution client is like a clerk, the consensus client is like a judge, and the Engine API is their private phone line. From today’s study, I understand that The Merge is not only a change of consensus mechanism, but also a new design of Ethereum’s node structure, which makes the system more modular and easier to upgrade in the future.
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->









亲爱的助教老师您好，明天上午10点我要考电动力学，这学期的最后一门考试，今天实在是没有时间学习其他的了。之后我会补上今天的内容，谢谢啦！
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->










Today, I attended an online sharing session on Web3 compliance. After listening to the lawyer's presentation, I felt a chill run down my spine, but I also felt fortunate to have learned these lessons in time. I used to think that as long as the technology wasn't "evil" and the company held an overseas license, everything was safe. However, today's notes completely overturned my "naive" thoughts.

The first thing that alerted me was the due diligence for job hunting. I used to think that a project with a US MSB license was a legitimate player, but the lawyer bluntly stated that this might just be "entry-level compliance" and does not represent deep compliance. If I can access their official website without a VPN, or register with a Mainland Chinese ID and phone number, the legal risk is still extremely high, even if they superficially claim "not to do business in the Mainland" . Especially if I apply for a position involving derivatives like contracts or options, if something goes wrong, it is very easy to be characterized as "opening a casino" in the domestic judicial environment, and it is hard to defend oneself with "technological neutrality".

Salary issues are also a huge blind spot. I used to think getting paid in USDT was convenient, but the notes clearly mentioned that paying wages in virtual currency is often not supported in Mainland labor arbitration . If the company wants to renege on the debt, or if I want to defend my rights, the law might not support me getting these "wages" back at all. Signing a contract is also a gamble: signing with an overseas entity makes it hard to defend rights, while signing with a domestic entity makes it easy to be implicated in the company's illegal business. The balance here is really hard to grasp.

Regarding the safety of cashing out, which everyone cares about most, the lawyer's words were very realistic: there is currently no 100% safe and perfect solution. Although going through licensed exchanges in Hong Kong is relatively the safest path, the tax cost is high. If I use other methods like I still do, I must always be wary of "path attacks"—even if I haven't done anything, just passively receiving involved funds can lead to frozen cards. If I really encounter a frozen card or investigation, truthfully explaining the situation and providing evidence for the first time usually allows for unfreezing; but if there is a "prior record" and operations continue, it is easy to be presumed as "knowingly" committing a crime, and the nature of it changes completely .

Finally, I deeply understood that **"**compliance" is not just what is written on paper. Having a set of beautiful legal texts is called "formal compliance," but true safety lies in "deep compliance"—cutting off illegal fundraising and Mainland users in business substance . As the lawyer lamented at the end, often the biggest risk comes from the human heart, such as partners absconding with funds. In this industry full of temptations, perhaps "do no evil" is the most important for us.
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->











Today I reviewed the core structure of blockchain systems. A blockchain consists of a sequence of blocks, each containing transaction records and the hash of the previous block. This chained hash structure ensures data integrity and makes historical records effectively immutable.

Wallet balances on a blockchain are not stored explicitly. Instead, balances are derived by scanning the entire transaction history associated with a given wallet address. Although all transactions are publicly accessible, wallet ownership remains anonymous because addresses are randomly generated and not directly linked to real-world identities.

Transaction validation and block production are performed by distributed nodes (miners). These nodes verify transactions through consensus mechanisms and are incentivized by block rewards, typically paid in Bitcoin. This incentive-driven design replaces centralized trust with cryptographic verification and economic motivation.

Bitcoin has several notable properties: a fixed supply that prevents inflation, transparent and public transactions, anonymous accounts, immutability, and permissionless value transfer. These features differentiate it fundamentally from traditional financial systems.

I also clarified that Web3.0 is conceptually different from Web3. Web3 generally refers to decentralized applications built on blockchain infrastructure. A typical Web3 development stack includes React for the frontend, ethers.js for blockchain interaction, Solidity for smart contracts, and IPFS for decentralized storage.

Finally, I examined scalability challenges in public blockchains, such as increased latency and low throughput as node numbers grow. Common solutions include sharding and Layer 2 architectures, which aim to improve performance while preserving decentralization.
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->












Completed most of the tasks except Ethereum Chinese Weekly Meeting because of time conflict.

Today I attended a sharing session hosted by SmartDeer, where I gained a clearer understanding of the current state of the industry and its potential future job demands. As a junior undergraduate student, I realized that technical positions generally require at least basic coding skills.

At the same time, due to the relatively “chaotic” nature of the industry, there is a strong demand for legal and compliance expertise. This made me reflect that, as a practitioner, one should not focus solely on technology, but also develop a solid understanding of legal frameworks across different countries.

I also learned about several promising startups in this field and began to think about what kind of people are truly driving innovation, rather than simply aiming to secure a well-paid job.

As it is currently the final exam period, I have not had much time to dive deeply into coding. After the 16th, however, I plan to accelerate my learning.
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
