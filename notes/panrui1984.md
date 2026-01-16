---
timezone: UTC+8
---

# Rui Pan

**GitHub ID:** panrui1984

**Telegram:** @RayPanChina

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->
今天模仿写的调研报告

# Circle Arc生态调研报告

# 一．项目背景

Arc 是一个专为稳定币金融和资产代币化而打造的 EVM 兼容 Layer-1 区块链。

Arc项目希望解决如下痛点：

原生代币Gas 费价格波动剧烈带来了不可预测的运营成本与会计复杂度；

商业交易所需的隐私保护功能缺失；-

完全公开、无需许可的交易提交机制催生了“最大可提取价值”（MEV）现象；

稳定币流动性与应用分布在多条链上，造成用户与开发者的高摩擦成本

Arc提出的解决方案要点是：

-   **USDC as Native Gas**​：以稳定的 USDC 支付费用，使成本可预测。
    
-   **Built-in FX Engine**​：链上无缝的货币互换。
    
-   **Sub-Second Finality**​：交易在不到一秒内确认，确定且可靠。
    
-   **Opt-in Privacy**​：使用受信任执行环境（TEEs）进行机密计算，如社区回复所指出。
    
-   **Full Circle Integration**​：利用 Circle 的平台，便于稳定币的发行和管理。
    
-   **EVM Compatibility**​：开发者可以使用熟悉的工具进行开发，让 DeFi 应用（包括潜在的 meme 代币发行）更易上手。
    

# 二．团队背景

Arc由Circle 主导开发，核心团队成员包括Gordon Y. Liao、Rachel Mayer、Adrian Soghoian 等，均来自 Circle。

Gordon Liao               Chief Economist and Head of Research at [Circle](https://www.circle.com/en/)

Rachel Mayer

Adrian Soghoian     Principal Software Engineer 

Adrian Soghoian

Sanket Jain   Principal Software Engineer, Gateway CEO

团队成员在稳定币，区块链基础设施搭建等领域均具备深厚积累。

# 三．投资方

项目白皮书中未提及Circle以外的投资方，我们可以简单的认为目前阶段Arc完全由Circle主导。

根据招股说明书显示（424B4 Prospectus \[Rule 424(b)(4)\] Accession Number: 0001193125-25-181916 链接：[424B4](https://www.sec.gov/Archives/edgar/data/1876042/000119312525181916/d839239d424b4.htm)），目前Circle的主要持有方及比例如下：

（最后一列未投票权比例）

# 四．项目规划

根据白皮书公布的项目路线图，Arc已知如下关键节点：

1.       2025年秋季发布测试网；

2.       主网Beta阶段包括包含稳定费率架构，亚秒级结算等核心功能；

3.       后续升级包括隐私增强，mev缓解等，并过渡至PoS机制；

已公开项目活动有两项

\> Private testnet after public testnet launch

\> Grab the “Architect” role on Circle DC:

根据白皮书揭露信息，Arc 当前不强调发币（不排除未来可能），但从定位来看，很可能成为 Circle 打通全球支付和链上金融的关键拼图。

# 五．项目生态

项目生态聚焦三大领域

1． 赋予企业/开发者/消费者以及资产发行方；

2． 支撑原生应用（外部，支付和代币化资产等场景）；

3． 通过Circle的平台，衔接链上（跨链）和链下（传统金融）的互通；

我们可以得出推论，Arc的项目承接着Circle通过稳定币打通传统金融和链下资金流通的诉求，实现全场景的资金在链上的流动。

# 六．竞品分析

目前，Tether、Circle、Stripe等众多公司正纷纷推出其专用区块链，旨在服务于全球稳定币支付。项目的核心诉求均相似，因此存在很大的竞争关系。

# 七．商务机会

Arc是一款专为稳定币金融打造的开放式 Layer-1 区块链。正如Arc描述的痛点所述，之前Defi项目面临着Gas波动，隐私审计缺失，使得机构用户使用充满疑虑，因此借助于Arc的稳定币特性，针对机构用户开发新一代Defi应用，获取客群使用。

可以有如下合作切入点：

1） 稳定币生态

支持 USDC/EURC、USDC / 人民币等合规货币对的 24/7 链上交易， 实现近似于传统外汇市场的流动性；

2） 低费率

利用 Arc 的 稳定 Gas 费率 降低用户借贷成本，以及快速成交特性，满足机构用户所需；

# 八．部署项目

若在Arc上部署项目，给予项目生态的描述，可以考虑部署一套企业跨境支付平台，功能要点有：

1） 跨境支付

Arc的Fx模块支持7×24 小时可编程支付对支付（PvP）结算，实现实时汇率的结算，支持多种稳定币的支付。打通法币与稳定的衔接；

2） 根据白皮书路线图所述，Arc规划selective transparency，满足保护敏感信息的同时支持监管的审计；

所需商务支持：

生态对接。接入Circle平台生态，实现与链下金融和跨链支持。

活动联合：联合 Circle 开展行业推广活动，触达中小企业客户。

稳定币的三个环节

发行（tether，circle）-》流通（转账和支付 Ethereum/tron） => 交易
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->

以下纪录今天学习以太坊开发的部分内容

### **交易**

-   一条交易包含以下内容：
    
    -   from：交易发送者地址
        
    -   to：交易接收者地址，如果为空则表示是在创建智能合约
        
    -   value：交易金额，即发送方要给接收方转移的以太币数量（wei 为单位）
        
    -   data：交易数据，如果是创建智能合约则是智能合约代码，如果是调用智能合约则是调用的函数名和参数
        
    -   gasPrice：交易的 gas 价格，即每单位 gas 的价格（wei 为单位）
        
    -   gasLimit：交易的 gas 上限，即交易允许执行的最大 gas 数量
        
    -   nonce：交易的序号，即发送者已经发送的交易数量
        
-   除此之外发送的交易数据包还需要包含：
    
    -   hash：交易的哈希值，由前面的内容和 chainId 计算得到
        
    -   v、r、s：交易签名的三个部分，由发送者私钥对交易哈希值进行签名得到
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->


今天晚上的法律合规专题课意义十分重大，可以说非常有帮助。

正巧，今天在人民法院报公众号一篇文章，

[https://yuanbao.tencent.com/chat/naQivTmsDa?from=WeixinAskAi&tridChannel=wxllqndtabgzhwzzj.lqybh5xzrk.pc&askAi=%7B%22coverUrl%22%3A%22https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz\_jpg%2Fqrrm9dFnnpS9d0qQTibdAaibcMFa1FM97JJglO3XGib5VZPXwIKfhFuofUaPvhsnbj62fKkiaQVRyyHJGFQLvian8EA%2F0%3Fwx\_fmt%3Djpeg%22%2C%22link%22%3A%22https%3A%2F%2Fmp.weixin.qq.com%2Fs%2F6NUlNDibHR\_KmEmMi311hQ%22%2C%22title%22%3A%22%E6%B6%89%E8%99%9A%E6%8B%9F%E8%B4%A7%E5%B8%81%E7%8A%AF%E7%BD%AA%E6%A1%88%E4%BB%B6%E7%9A%84%E9%80%82%E6%B3%95%E7%BB%9F%E4%B8%80%E6%8E%A2%E8%AE%A8%E2%80%94%E2%80%94%E7%AC%AC%E5%9B%9B%E6%9C%9F%E2%80%9C%E8%87%B3%E6%AD%A3%C2%B7%E7%90%86%E8%AE%BA%E5%AE%9E%E5%8A%A1%E5%90%8C%E8%A1%8C%E2%80%9D%E5%88%91%E4%BA%8B%E5%AE%A1%E5%88%A4%E7%A0%94%E8%AE%A8%E4%BC%9A%E7%BB%BC%E8%BF%B0%22%7D&retry=1&nonce=maeDZrC6xsbwctJf&state=yf\_wechat\_login&code=041EudGa1TTXZK0UVEHa1ZcvMp4EudGA](https://yuanbao.tencent.com/chat/naQivTmsDa?from=WeixinAskAi&tridChannel=wxllqndtabgzhwzzj.lqybh5xzrk.pc&askAi=%7B%22coverUrl%22%3A%22https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2Fqrrm9dFnnpS9d0qQTibdAaibcMFa1FM97JJglO3XGib5VZPXwIKfhFuofUaPvhsnbj62fKkiaQVRyyHJGFQLvian8EA%2F0%3Fwx_fmt%3Djpeg%22%2C%22link%22%3A%22https%3A%2F%2Fmp.weixin.qq.com%2Fs%2F6NUlNDibHR_KmEmMi311hQ%22%2C%22title%22%3A%22%E6%B6%89%E8%99%9A%E6%8B%9F%E8%B4%A7%E5%B8%81%E7%8A%AF%E7%BD%AA%E6%A1%88%E4%BB%B6%E7%9A%84%E9%80%82%E6%B3%95%E7%BB%9F%E4%B8%80%E6%8E%A2%E8%AE%A8%E2%80%94%E2%80%94%E7%AC%AC%E5%9B%9B%E6%9C%9F%E2%80%9C%E8%87%B3%E6%AD%A3%C2%B7%E7%90%86%E8%AE%BA%E5%AE%9E%E5%8A%A1%E5%90%8C%E8%A1%8C%E2%80%9D%E5%88%91%E4%BA%8B%E5%AE%A1%E5%88%A4%E7%A0%94%E8%AE%A8%E4%BC%9A%E7%BB%BC%E8%BF%B0%22%7D&retry=1&nonce=maeDZrC6xsbwctJf&state=yf_wechat_login&code=041EudGa1TTXZK0UVEHa1ZcvMp4EudGA)

# **涉虚拟货币犯罪案件的适法统一探讨——第四期“至正·理论实务同行”刑事审判研讨会综述**

以下是我对其重要观点做了一些总结

1.  **关于“主观明知”的认定（洗钱罪）**
    
    -   **核心**：必须证明行为人“知道或应当知道”钱来自特定上游犯罪
        
2.  **关于“非法经营罪”的认定**
    
    -   **核心**：经常性提供人民币与外币之间的兑换服务并牟利，属于“变相买卖外汇”
        
    -   **区分**：个人偶尔炒币不构罪，但以此为业则可能构成
        

**简单来说**：

-   **怎么算“明知”洗钱？**​ 不能乱推定，需要实锤证据。
    
-   **什么时候算“洗钱成功”？**​ 赃款换成U币的那一刻就算。
    
-   **什么换汇行为算犯罪？**​ 把虚拟货币当生意做，专门帮人跨境兑换就违法。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->



“Web3 运行原理”课程笔记

### 交易（TX）

-   交易 = 操作 + Gas + nonce
    
-   类型：转账、合约、govence
    
-   nonce 防重放：确保执行一次，防止重放攻击
    

### 钱包

-   功能：提供UI填交易 → 使用私钥签名 → 通过 RPC 广播到网络
    
-   签完即广播，Etherscan 可查状态
    

### 数字签名原理

-   私钥签名 → 全网可验，无法伪造
    
-   私钥，助记词
    

### Gas

-   作用：防垃圾交易、激励验证者、维持经济模型
    
-   网络拥堵时 gasPrice 上涨
    

### 交易从发起到上链

1.  签名
    
2.  经 RPC 发给节点
    
3.  进内存池排队
    
4.  Builder 从交易池中挑选高 Gas的 交易进行打包
    
5.  Validator节点进行 出块 + 共识确认
    
6.  写入区块链（约每 12 秒一区块，含数百笔交易）
    

### 区块确认

-   新块初始为 Unfinalized，需 12 分钟才不可逆
    

### 智能合约

-   链上可执行的代码
    
-   执行：结果永久上链
    

### 以太坊升级？

-   EIP
    
-   硬分叉
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->





今天完成的任务如下：

-   钱包任务（测试网airdrop，转账以及铸造NFT）
    
-   完成[Unphishable](https://unphishable.io/)初级和中级的题目
    
-   下午答疑会议及晚上的职业指南
    

我的一些感想：

接触web3已经已有大约半年的时间了，对web3已经有了一定的认知。但是第一天的跟班学习是仍然受益匪浅，系统性的知识明显比零散的学习效率更高。比如防钓鱼的题目过一遍还是对自己有一定的警醒。尤其是《021学习以太坊》更是让我对以太坊有更深入的认知。

参加学习的第一天，我总结了训练营给初学者的学习指南（我会分享给我的老婆一起学习）：

**第一阶段：建立基础认知**

了解web3基础的概念，通过一些简单的实践对web3有初步了解，具体包含：

**一、先获得 Web3 基础能力**

钱包，助记词，网络，转账，授权，区块浏览器

**二、建立 Web3 信息源**

关注 高质量账号，Builder，Researcher等；

三、**选一个方向**

社区运营（Discord / TG社区），技术等
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
