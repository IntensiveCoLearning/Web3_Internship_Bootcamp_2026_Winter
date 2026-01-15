---
timezone: UTC+8
---

# Jinge Wang

**GitHub ID:** jingedawang

**Telegram:** @Jinge

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
几个问题

-   web3比web2快？我理解其实不会，只是上去了合规审查的部分，出海更方便了。Ricky也委婉表达了这个观点。
    
-   web2 API与web3 agent的区别？应该只是省去了apikey支付的麻烦，以及使得调用、log更透明。有一些有点，但需要基于web生态。当然，一个agent可以同时支持web2调用和web3调用。
    
-   agent身份的问题。目前agent只是在web3中暴露一个API接口，实际代码还是部署在链下。所以身份只是一个API接口的身份，后端的实现可以随时更改。这样可能并不是特别让人信服。应该是把agent实现固定后放在TEE里面，不一定开源，但至少让用户知道是固定的，这样才有身份可言。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

Web3安全讲座

![image.png](attachment:6a6c418b-5304-48d6-9677-50a505645c06:image.png)

三种最典型的攻击方式。Rug Token是发一种类似代币的假代币，让人们误以为是真的。

![image.png](attachment:dc99694f-978f-4c0f-90d0-83d1f05d35de:image.png)

2025年很多用了很久的应用都被发现了漏洞，说明AI在其中应该是起到了大作用。2026年预期会有更多新的漏洞被发现，项目方需要提前做准备。

攻击者窃取到私钥后不一定会立即行动，会变得更有目的性。

![image.png](attachment:d2e71393-eeae-4c5e-ad25-242bcd83093d:image.png)

使用Deepfake技术实施社会工程学攻击变得很流行。

![image.png](attachment:9d7850e3-2e6f-4fb1-bb28-994f7ecd43dd:image.png)

邓小宇讲Web3合规

ICO（Initial Coin Offering）公开融资。发币不违法，但借助发币非法融资则违法。

逐利性执法，打击交易所获取收益。

Web2中心化的法律不适合Web3去中心化的理念。

PolyMarket如果开在中国，肯定会被定性为赌博。

交易所出金可能会收到黑钱，然后引起警方关注。

U商套利原理

![image.png](attachment:2c8e6bd1-f44a-4743-b5da-14e02c25ac28:image.png)

U卡？

项目方风险。

KOL衍生风险。

KOL帮交易所宣传，当交易所被当作赌场被抓后，KOL会被当作共犯。

中国不支持用虚拟币付工资。

入金风险小，出金风险大。

出金收到赃款，可能被警方追查，然后要求退钱。警方可能强行实施。

以太坊的生态系统由多层架构组成，包括 **L1（主网）、L2（二层扩展解决方案）、侧链（Sidechains）** 等，共同支持高吞吐量和低费用的交易处理。

1.  **Layer 1（L1）**
    
    -   **以太坊主网**：核心区块链，负责最终安全性与共识。
        
    -   **EVM**：以太坊虚拟机，执行智能合约代码。
        
    -   **账户系统**：外部账户（EOA）与合约账户（CA）共同构成网络基础。
        
2.  **Layer 2（L2）**
    
    -   **Rollup**：通过将交易批量处理后提交至 L1，降低 Gas 费。
        
        -   **Optimistic Rollup**：假设交易合法，仅在争议时验证。
            
        -   **ZK Rollup**：通过零知识证明验证交易，无需链上争议。
            
        
        注意，L2并不是链，而是中心化的网络服务。为了提高效率降低成本，将这些中心化的L2服务接入L1链，从而实现安全和效率的平衡。
        
3.  **侧链（Sidechains）**：独立运行的链，通过桥接与主网交互。
    

### [**以太坊生态分层架构**](https://web3intern.xyz/zh/overview-of-ethereum/#%E4%BB%A5%E5%A4%AA%E5%9D%8A%E7%94%9F%E6%80%81%E5%88%86%E5%B1%82%E6%9E%B6%E6%9E%84)

从软件栈的功能层次来看，以太坊生态可以分为以下几个层次：

### [**1. 应用层（Application Layer）**](https://web3intern.xyz/zh/overview-of-ethereum/#_1-%E5%BA%94%E7%94%A8%E5%B1%82-application-layer)

用户直接交互的应用和界面：

-   **DeFi 应用**：Uniswap（去中心化交易所）、Aave（借贷协议）、Compound（借贷协议）
    
-   **NFT 平台**：OpenSea、Foundation、SuperRare
    
-   **钱包应用**：MetaMask、Coinbase Wallet、Rainbow
    
-   **DAO 工具**：Snapshot、Aragon、Colony
    

### [**2. 协议层（Protocol Layer）**](https://web3intern.xyz/zh/overview-of-ethereum/#_2-%E5%8D%8F%E8%AE%AE%E5%B1%82-protocol-layer)

以太坊的核心基础设施：

-   **共识层客户端**：Prysm、Lighthouse、Nimbus、Teku
    
-   **执行层客户端**：Geth、Nethermind、Erigon、Besu
    
-   **核心协议**：EVM、状态管理、Gas 机制
    

### [**3. 扩展层（Scaling Layer）**](https://web3intern.xyz/zh/overview-of-ethereum/#_3-%E6%89%A9%E5%B1%95%E5%B1%82-scaling-layer)

提升性能和降低成本的解决方案：

-   **Layer 2 Rollups**：Arbitrum、Optimism、Polygon zkEVM、zkSync Era
    
-   **侧链**：Polygon PoS、xDAI（Gnosis Chain）
    
-   **状态通道**：Lightning Network for Ethereum
    

以太坊的账户分为**外部账户（EOA）和合约账户（CA）**。前者是普通用户的钱包，后者是部署的智能合约。也就是说，智能合约的地位和一个钱包账户类似，它不属于某个钱包，而是单独的地址。合约账户部署后生成一个地址，没有私钥。

每个账户都包含四个关键字段：

-   **Nonce**：防止重复交易的计数器（EOA 记录发送次数，CA 记录创建合约次数）。
    
-   **余额**：账户持有的 ETH 数量（单位为 Wei）。
    
-   **CodeHash**：EOA 为空哈希，CA 存储合约字节码的哈希值。
    
-   **StorageRoot**：记录账户数据的 Merkle 树根哈希（如 NFT 归属关系）。
    

任何交易都需要付燃料费。

Gas 费用 = **用多少 × 每单位多少钱**，就像你打车一样：

-   **Gas Limit（限额）**：你最多愿意“烧”多少燃料。 比如你觉得最多可能需要 15 万单位，就设置 150,000。
    
-   **Gas Price（单价）**：每单位燃料多少钱，用 Gwei 表示（1 Gwei = 0.000000001 ETH）。 网络越拥堵，价格越贵，就像打车高峰期加价。
    

所以，**总费用 = Gas Limit × Gas Price**。

以前，Gas Price 全部给矿工。 现在，分成两部分：

-   **基础费用（Base Fee）**：每个区块都会有，自动计算，直接销毁（直接消失）来帮助 ETH 通缩，毕竟物以稀为贵。
    
-   **小费（Tip）**：你额外加的钱，用来鼓励矿工优先处理你。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


## **以太坊概览**

以太坊是区块链2.0，比特币是区块链1.0。

由于支持在链上执行智能合约，所以可以构建

-   去中心化应用：DApps（Decentralized Applications）
    
-   去中心化金融：DeFi（Decentralized Finance）
    
-   数字艺术品：NFT（Non-Fungible Token，非同质化代币）
    
-   去中心化自治组织：DAO（Decentralized Autonomous Organization）
    

2022年从工作量证明（PoW）切换到权益证明（PoS，Proof of Stake）。

借助分片和 Layer 2 技术提升效率与扩展性。

以太坊上的所有操作都需要支付手续费，称为燃料费（Gas Fee），而且必须使用以太币ETH支付。

以太坊和比特币的区别

| 维度 | 比特币（Bitcoin） | 以太坊（Ethereum） |
| --- | --- | --- |
| 目标与定位 | 去中心化的数字货币，强调安全、稳定和稀缺性（总量 2100 万枚） | 去中心化平台，支持智能合约和 Dapps，定位为“区块链 2.0” |
| 编程能力 | 脚本语言有限，仅支持简单的交易验证逻辑 | 图灵完备的编程语言（如 Solidity），可开发复杂智能合约 |
| 共识机制 | 工作量证明（PoW），矿工通过算力竞争记账权 | 从 PoW 转向权益证明（PoS），通过 The Merge 实现能源效率优化 |
| 交易速度 | 每 10 分钟生成一个区块，交易确认较慢 | 区块时间约 12 秒，交易确认更快，适合高频应用 |
| 经济模型 | 总量固定，强调抗通胀属性 | 供应灵活，通过 EIP-1559 等机制可能呈现通缩趋势 |

可以说，两者已经差异化定位了。比特币作为“数字黄金”，用来存储价值。而以太坊则作为去中心化的生态系统，构建整个web3的大厦。

![image.png](attachment:5b00fb53-e47e-47c8-af2e-a40cfd4ee4c3:image.png)

为什么攻击成本需控制全网67%的质押ETH？

以太坊近几年在持续升级，目的是降低交易费用、提高效率，从而让Web3普及到更多应用场景。这里面Layer2技术和分片技术很重要。具体原理这里面没讲。

助记词生成大量私钥，选取其中一个，再生成公钥。公钥取hash，再取后20位作为地址。

避免私钥泄露，不要放在剪贴板、网络平台、截图里面。

签名者把原始信息、签名以及自己的地址通过RPC发布出去。接收到的节点使用原始消息的哈希和签名恢复公钥，然后用恢复的公钥和作者声称的地址比较，一致则验证通过。

![image.png](attachment:ab65c6cb-1784-48c3-97eb-a0c52565c2da:image.png)

RPC是中心化入口。不过RPC服务提供者也有很多，也是去中心化的。

![image.png](attachment:ae13dddf-37ae-4e1f-acde-f5725d652960:image.png)

智能合约消除了依赖第三方公证机构的麻烦。直接把公正规则放在链上，自动执行。

![image.png](attachment:0b2995ed-cc75-4c1e-8ba4-1ff3577ec003:image.png)

EIP（Ethereum Improvement Proposal）会持续升级以太坊协议。达成共识后直接硬分叉。

以太坊上线十几年从未宕机。

![image.png](attachment:60ffa28c-86c3-48b1-85e9-5b6fb43b1480:image.png)

![image.png](attachment:da893a79-2a87-4751-af88-6383b352146f:image.png)

MetaMask这种钱包App生成私钥后会保存在本地，设置的密码用来对私钥加密。但如果电脑被盗或被植入病毒，私钥文件泄露后，黑客就有可能破解MetaMask的密码从而拿到私钥。另外，如果电脑坏了、硬盘格式化、重装系统，就有可能销毁私钥，从而丢失钱包。所以，应该自己把钱包的私钥和助记词手抄一份存下来。最保险的做法是使用硬件冷钱包，类似于U盘的东西。私钥写入硬件，硬件不联网，只用来签名，永远不会泄露，除非被偷或丢失。

还要注意，钱包地址生成器也可能存在漏洞，导致可以从公钥推出私钥。需要选择可靠的生成器，比如MetaMask这种公认的，而不是某个野鸡网站帮你自动生成的。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



## **区块链基础概念**

区块链特性：

-   区块链上的信息不可篡改，因为有哈希确保无法单独修改某个区块。
    
-   链上信息完全公开、透明。
    
-   交易无需通过中央机构认证，上链即自动完成。
    

分布式记账节点确保了区块链

-   去中心化
    
-   真正的不可篡改
    

区块链交易是公开且匿名的。公开是因为链上信息完全透明，匿名是因为别人不知道某个钱包地址是你的。如果你自己在网上公开自己的钱包地址，那你的账户就变成了公开且实名的。不过，别人仍然只能看你的账户操作，无法控制你的账户，因为私钥还在你手里。

区块链生态系统的运行包含以下几个关键步骤：

1.  **用户发起交易**：用户通过钱包应用发起转账、智能合约调用等操作
    
2.  **交易广播**：交易信息被广播到整个网络中的各个节点
    
3.  **节点验证**：网络中的矿工节点验证交易的合法性（余额是否足够、签名是否正确等）
    
4.  **打包成块**：通过共识机制（如工作量证明），矿工将验证过的交易打包成新的区块
    
5.  **链接上链**：新区块被添加到区块链上，更新全网的账本状态
    
6.  **奖励发放**：成功打包区块的矿工获得代币奖励和交易手续费
    

三种模式

![image.png](attachment:67559bb0-9084-4a98-a40e-7435927e01c4:image.png)

从Web2、Web3.0到Web3

![image.png](attachment:6b204c80-c979-46ff-b974-8dd4f78446d9:image.png)

![image.png](attachment:64d355d2-0335-43a7-af6c-c1d30579653b:image.png)

显然，Web3.0比较凉，根本没有推广。这种结构化的倡议一般情况下都不靠谱。就像知识图谱一样，只是小部分人的自娱自乐，LLM一出来直接就废了。

![image.png](attachment:32a3c0cd-ecd8-4fd5-aac3-5acc00f6bf6c:image.png)

Web3和Web2相比最大的区别是数据真正属于每个用户，不受平台监管。不像现在的平台想删帖就删帖想封号就封号。

但Web3似乎并不是完全用来替代Web2。Web3更适合金融、产权、隐私场景，与交易有关，且频率不高。而高频的社交场景，比如微博，就更适合Web2。

虽然Web3有众多优点，但去中心化带来的自由同时也增加了用户的责任。所谓能力越大，责任越大。用户现在需要完全为自己的行为负责，如果丢失密码或者别窃取资金，没有任何监管机构可以帮忙找回来。对于中国人来说，这种巨大的责任估计是不太容易承受。我们更乐于生活在国家和政府的庇佑下，见到美国这种更自由的社会形态就会觉得可怕，更别说Web3了。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
