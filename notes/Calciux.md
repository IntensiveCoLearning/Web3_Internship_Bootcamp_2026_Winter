---
timezone: UTC+8
---

# Calciux

**GitHub ID:** Calciux

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
## 学习以太坊 1-2

### 以太坊

-   Tokens
    
    -   ERC-20 fungible token的统一标准,使所有代币在钱包、交易所、DeFi 协议中可互操作
        
    -   ERC-721 non-fungible token 每个token彼此不相同,是独立不可替代的实体,相互不等价ERC-1155 同时包含FT/NFT/半同质化, 通过不同的标识来区分不同token的性质
        
-   Decun 升级
    
    -   Proto-Danksharding
        
    -   引入 blob: 临时的专门给Rollup存放交易数据的区域 主链不会保存完整的内容而是爆粗哪一个commitment, 能够验证
        
-   完整的danksharding
    
    -   blob变成了多个data shards, 每个shards只保存一部分数据
        
    -   Data Availability Sampling(DAS) 允许节点不下载整个区块而可以验证数据的真实性
        
    -   DAS利用Reed-Solomon ensure code(是一种maxium distance seperable code), 只要其中的部分数据就能恢复全部数据, 而大量节点的同时抽样验证保证了数据的生产者几乎不可能隐藏数据, 从而确保了只需要下载少量数据就可以验证数据是否被真实发布.
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->


# Web3实习手册C1

### 去中心化?

1.  Permission model: Permissionless; No privileged participants; Replaceability
    
2.  [BFT](https://bitcoin.org/bitcoin.pdf): PoW 50%; PoS 33%
    

## 区块链的运行

-   **用户发起交易**：用户通过钱包应用发起转账、智能合约调用等操作
    
-   **交易广播**：交易信息被广播到整个网络中的各个节点
    
    -   gossip protocol: 验证交易是否有效,放入mempool; inventory 避免重复传播
        
-   **节点验证**：网络中的矿工节点验证交易的合法性（余额是否足够、签名是否正确等）
    
-   **打包成块**：通过共识机制（如工作量证明），矿工将验证过的交易打包成新的区块
    
-   共识:谁有权力写入下一个区块; 写入的内容是否被全网接受
    

PoW: 寻找满足条件的N. 安全性来源于target的阈值极小, 需要大量暴力搜索

```
f(D)>SHA256(SHA256(H-(T||TX||D||N)))
```

PoS: Stake 竞争被选中记账的概率, 质押越多概率越大(balance)

```
SHA256(SHA256(prevBlock, A, t))<=balance(A)*m
```

-   **链接上链**：新区块被添加到区块链上，更新全网的账本状态
    

**奖励发放**：成功打包区块的矿工获得代币奖励和交易手续费

# Jan12 分享会

RWA**现实世界中存在的、有价值的资产**，通过 **tokenization（代币化）** 的方式被映射到区块链上，形成可在链上流通、拆分、抵押、交易的数字化资产。

DePIN资源上链

Rust\\合规\\Research analyst AI usage & On-chain Reputation

DAO/开源社区/投研社区

**Solidity：**  
Web3 最广泛和通用的开发语言，易上手，建议新手要学习一下，理解智能合约的编程思维逻辑。

**趋势：**  
Rust 的应用越来越多，很多链和自创语言基于 Rust 开发。

**学习和关注 Web3 新语言：**  
Move、Cairo、Func 等。

**合约安全意识；系统设计；对 DeFi / L2 的理解**

ai工具: 智能合约审计\\自动化
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
