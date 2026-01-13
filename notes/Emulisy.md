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
