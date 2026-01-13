---
timezone: UTC+8
---

# Eeeeye

**GitHub ID:** Eeeeye

**Telegram:** @Eeeeye

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
### 1\. 概念辨析：Web3 的认识论 (Epistemology)

-   **Web 3.0 (Semantic Web, 1999)**:
    
    -   **提出者**: Tim Berners-Lee.
        
    -   **核心逻辑**: **数据互操作性**。通过 RDF/OWL 等标准，使非结构化数据转变为机器可理解的“知识图谱”，旨在解决信息孤岛。
        
-   **Web3 (Decentralized Web, 2014)**:
    
    -   **提出者**: Gavin Wood.
        
    -   **核心逻辑**: **状态持久性与所有权**。基于密码学背书的分布式账本，构建一个去信任（Trustless）的价值传输协议。
        
    -   **本质**: 它是对互联网协议栈（TCP/IP）的一层“价值补丁”，实现了**信息流与价值流的原子化绑定**。
        

### 2\. 以太坊本体论：准图灵完备的单例状态机

从计算机科学视角定义，以太坊不仅是区块链，而是一个 “全球单例状态机” (Global Singleton State Machine)。

-   **状态转换函数**: 以太坊的本质由公式 `σ_{t+1} ≡ Υ(σ_t, T)` 定义。
    
    -   `σ` (Sigma): 全局状态（账户余额、合约存储）。
        
    -   `T`: 交易（即经过签名的指令）。
        
    -   `Υ`: 状态转换函数（即 EVM 的规则）。
        
-   **Gas 的计算理论基础**: Gas 机制并非单纯的经济收费设计，而是为了解决计算机科学中的\*\*“停机问题” (Halting Problem)\*\*。
    
    -   由于 EVM 是图灵完备的，无法预知一段代码是否会无限循环。
        
    -   Gas 引入了强制的“执行边界”，确保任何计算过程在有限步骤内必然终止（无论成功或由于 Gas 耗尽而失败），从而保证网络的**活性 (Liveness)**。
        

### 3\. 共识机制

共识机制不仅是达成一致的算法，更是分布式系统中的**抗女巫攻击 (Sybil Resistance)** 方案与**分叉选择规则 (Fork Choice Rule)** 的结合。

-   **PoW (Work)**: 基于**热力学**的安全性。
    
    -   利用物理世界的稀缺资源（能源/硬件）构建攻击成本。
        
    -   **局限性**: 仅具备**概率性最终确定性 (Probabilistic Finality)**，安全性随区块深度线性增加，无法在协议层杜绝回滚。
        
-   **PoS (Stake)**: 基于**内生博弈论**的安全性。
    
    -   利用**虚拟化的稀缺资源**（原生代币 ETH）构建验证门槛。
        
    -   **Casper FFG 协议**: 引入了**经济罚没 (Slashing)** 机制，使得攻击者的成本是显性的、可量化的资产销毁。
        
    -   **确定性 (Finality)**: 引入 Checkpoint 机制，使得区块状态在经过 2 个 Epoch 后达到“经济上不可逆”的绝对安全状态。
        

### 4\. 系统语言范式：Rust 的工程必要性

在底层基础设施（Layer 1/Layer 2 节点、ZK 电路）的构建中，Rust 正在取代 Go/C++ 成为标准，其核心驱动力源于**系统安全性**需求。

-   **内存安全 (Memory Safety)**: Rust 的所有权 (Ownership) 模型在编译阶段杜绝了空指针、数据竞争等内存错误，这对处理千亿美金资产的区块链节点至关重要。
    
-   **确定性与零抽象成本**: 区块链要求严格的**确定性执行 (Deterministic Execution)**。Rust 无垃圾回收 (No-GC) 的特性避免了运行时（Runtime）的不确定性停顿，同时在零知识证明（ZK）涉及的大量多项式运算中提供了极致性能。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

## **1/12**

### **以太坊中文周会**

![image-20260112140428024.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eeeeye/images/2026-01-12-1768227646059-image-20260112140428024.png)

完整看完周会，发现内容相对深入且相对前沿，观看门槛较高，希望可以通过持续学习适应周会的知识密度

## **Faucet**

由于先前注册过Metamask, 故直接前往Faucet网站输入metamask钱包Ethereum网络地址，获取Sepolia ETH（0.05）,并测试给陌生钱包地址转账：

![截图 2026-01-12 21-41-30.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eeeeye/images/2026-01-12-1768227659898-___2026-01-12_21-41-30.png)

交易完成后 获取tx link，在区块浏览器上查看结果：

![image-20260112215007110.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eeeeye/images/2026-01-12-1768227679997-image-20260112215007110.png)

## **My First NFT**

[https://nft.myfirst.io/](https://nft.myfirst.io/)

进入LXDAO的教学网站，连接至metamask钱包，直接使用网站的前端页面进行铸造，使用前面领的Sepolia ETH作为gas，成功铸造

![image-20260112220713587.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eeeeye/images/2026-01-12-1768227694172-image-20260112220713587.png)

## **Unphishable 钓鱼攻防挑战**

### **No.0x0001**

这一章是简单的钱包注册，以及测试币领取，前面已完成

![image-20260112221309750.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eeeeye/images/2026-01-12-1768227707961-image-20260112221309750.png)

### **No.0x0002**

第二章是一个钓鱼网站的模拟，通过恢复钱包帮助的方式欺骗，诱导输入私钥，拒绝输入即可通过

![image-20260112221811624.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eeeeye/images/2026-01-12-1768227729308-image-20260112221811624.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
