---
timezone: UTC-8
---

# Miao

**GitHub ID:** 89lorry

**Telegram:** @mkylorry

## Self-introduction

大家好，我是Miao，毕业于南京大学，做过区块链与信贷系统科研，对实战感兴趣，希望通过本次学习能获得实战经历与对Web3更系统的认知 :)

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
今天去tesla试驾，本来对电车无感的，但是自动驾驶真的很厉害，完全不用动，以后会考虑。晚上喂了小浣熊几个葡萄。明天：

-   简明法语教程两集
    
-   web3两集，做nft
    
-   准备accenture终面
    
-   pte core了解一下大概，时间合适报名
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

Web3运行原理

-   私钥：一个随机生成的字符串，由其派生出公钥和钱包地址（如以太坊地址为公钥末20字节加“0x”前缀）
    
-   助记词：通常为12或24个单词序列，用于派生和管理多个私钥，便于备份。
    
-   交易生命周期：从签名到上链 交易包含核心三要素：操作内容（如转账）、手续费（Gas费）以及防重放的序号（Nonce）。
    
-   数字签名确保交易真实性与不可伪造：钱包用私钥对交易信息签名，网络可通过公钥验证签名真伪。 Gas费是网络运行的“燃料”与安全机制：由Gas价格（单位成本）和Gas用量（操作复杂度）共同决定，用于防止垃圾交易、激励维护者，并在网络拥堵时形成价格竞争。 交易广播后需经排序、打包与验证：验证者节点（如以太坊需质押32个ETH）将交易按Gas价格优先级打包进区块（约12秒一个），并投票验证其有效性。
    
-   区块链的信任基础：共识、不可篡改与智能合约
    
-   区块链的不可篡改性需要时间来实现：新区块通过哈希值指向前一区块形成链条，初期并不安全。以太坊约需12-13分钟达到“最终确定”状态，此时篡改可能性几乎为零。
    
-   共识机制： 1. 工作量证明（PoW）：节点通过算力竞争打包权。 2. 权益证明（PoS）：节点通过质押资产获得随机打包权，作弊将被罚没。
    
-   智能合约是存储在链上的可执行代码：在虚拟机中运行，逻辑公开、不可篡改且可追溯，实现了“代码即法律”，将信任从人/机构转移至程序。 智能合约能显著降低交易信任成本：以自动交换合约为例，可无需中介（如律师、法院）自动完成资产互换，减少摩擦。
    
-   节点与验证者的分散性增强网络韧性：以太坊约有1.37万个节点（地理上美国、德国、中国较多）和约90.7万个验证者，数量越多，网络抗攻击和中断能力越强。
    
-   Web3的核心特性围绕去中心化构建： 无需许可，创建钱包和参与无需审核，但需支付Gas费防垃圾信息。 抗审查：交易难以被阻止，节点和钱包可替换。 开放开源，代码与交易记录完全公开。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


-   用Etherscan验证交易在链上是否真实发生：
    

查以太坊主网 `https://etherscan.io/`

查Sepolia testnet  `https://sepolia.etherscan.io/`

-   做了unphihable的beginner部分，下面这道题还挺好玩的，学了punycode，解码网址`https://www.punycoder.com/`
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/89lorry/images/2026-01-14-1768370020555-image.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



### 1.12

-   三年前就注册过metamask但是是为了写论文，没有实际操作过。今天尝试用测试币在两个账户间转账，用的faucet是google cloud web3，got 0.05 Sepolia ETH. in chinese 活水
    
-   转的是0.013 ETH，扣的是0.01305468 ETH，多出来的就是 Gas，模拟真实以太坊。去向，Base Fee直接销毁，Priority Fee给出块的验证者，所以刚刚那 0.000055 ETH一部分被永久销毁，一部分给了 Sepolia 验证节点。 MetaMask 赚钱不是从Gas里抽，它赚钱来自：内置 swap 的 0.875% fee（换汇），内置 bridge（跨链），某些聚合路由。普通转账它完全是零收入。
    
-   有用的笔记：ISyaxinLiu
    
-   教程：[https://humdrum-volleyball-4a2.notion.site/2e54867ff3e180b5bc84e33a7e2f9835](https://humdrum-volleyball-4a2.notion.site/2e54867ff3e180b5bc84e33a7e2f9835)
    
-   用了半年google cloud才知道还有个web3 testnet： [https://cloud.google.com/application/web3/protocols](https://cloud.google.com/application/web3/protocols)
    

![截屏2026-01-12 12.10.40.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/89lorry/images/2026-01-12-1768257676926-__2026-01-12_12.10.40.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
