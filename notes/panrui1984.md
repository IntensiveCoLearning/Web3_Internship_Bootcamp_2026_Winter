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
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
# Polymarket 技术向精简学习笔记

Polymarket 是基于区块链的去中心化预测市场标杆平台，核心依托**智能合约**、**AMM 机制**、**预言机**实现事件概率代币化交易与自动结算，底层部署于 Polygon 二层网络。

## 一、 底层网络选型：Polygon PoS 二层链

1.  **核心优势**
    
    -   **超低 Gas 成本**：单笔交易费 0.01–0.05 美元，适配小额预测交易；
        
    -   **高吞吐低延迟**：TPS 达 65,000+，交易确认 2–3 秒，优于以太坊主网；
        
    -   **EVM 兼容**：直接复用 Solidity 合约，降低开发适配成本；
        
    -   **数据安全**：依托以太坊主网做最终结算，兼顾效率与安全性。
        
2.  **架构层级**
    
    应用层（前端 / API）→ 合约层（市场 / AMM / 结算合约）→ Polygon 二层网络 → 以太坊主网（最终结算）
    

## 二 交易核心：CFMM 自动化做市商机制

1.  **价格逻辑**：`YES 价格 = y/(x+y)`，`NO 价格 = x/(x+y)`，价格实时反映市场共识；
    
2.  **流动性管理**：平台注入初始流动性，支持第三方 LP 加入并分润 0.25% 手续费；小众事件启用 0.5% 动态手续费防价格操纵；
    
3.  **优势**：无需对手方，即时成交，替代传统订单簿，降低合约复杂度。
    

## 三、 关键组件：预言机（事件结果数据源）

1.  **工作流程**：预设节点列表 → 多节点独立提交结果 → 超 2/3 节点共识验证 → 触发结算合约；
    
2.  **技术特点**：多签机制防篡改，结果全上链可审计；支持 DAO 投票仲裁争议结果；未采用通用预言机，定制专用网络适配离散型事件判断。
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

Bitlayer的调研报告

一、 项目概况与背景

Bitlayer 于2023 年创立，是首个基于 BitVM 架构的比特币Layer 2，目

标是成为“比特币世界的 Arbitrum”，愿景是释放 BTC 的 DeFi 潜力，构建

安全、高效、EVM 兼容的 BTC L2 网络。

• 团队成员情况

核心成员来自 Bitmain、Polygon、Polkadot 等头部项目，技术负责人

Kevin 擅长底层架构，联合创始人 Charlie 拥有丰富 BD 与生态拓展背景，团

队整体国际化，专注构建比特币原生 Layer 2。

• 融资信息见图

二、生态版图分析

• 生态阶段：2024 年初上线测试网，2024 年 Q2 上线主网，当前处于生

态冷启动阶段

• 生态项目类型：

o DeFi 协议：BitSwap、LendBTC、BTC Aggregator

o 稳定币项目：BTC 原生稳定币试验项目

o 桥接工具：BitBridge

o 开发工具类：BitScan、Explorer、Wallet 插件类工具

• 龙头项目举例：

o BitSwap：首个在 Bitlayer 上部署的 AMM 协议

o BitBridge：官方主推的跨链桥，承接 BTC 入金出金

三、发展动态

• Hackathon/Grant：

o Bitlayer Foundation 已开启首轮“生态激励计划”，支持早期

项目部署，申请 Grant 或代币预分配

o 举办首期“Bitlayer Global Hackathon”，与 DoraHacks 联合

推出

• 线下活动：

o 在香港、东京、韩国等地参与比特币开发者线下聚会

• 生态增长指标：

o 主网上线 2 个月后 TVL 破 1 亿美金，增长快

o 日活钱包数与交互笔数持续上涨中

四、潜在合作机会点

• DeFi 方向：借贷、衍生品协议（Bitlayer 上尚无成熟的 lending 协

议）

• RWA & 稳定币：可探索 BTC 质押发行稳定币，或结合现实资产进行跨

链锚定

• 基础设施工具：数据分析仪表盘、钱包插件、消息订阅类服务（如

Dune、Zapper 角色）

• 合作方式建议：

o 可申请官方 Grant（开发者基金）

o 联合举办线上黑客松、技术 AMA

o 与 Bitlayer 社区和基金会对接生态资源、技术指导等

五、如果我部署一个项目，我会做什么？

项目设想：

构建一个「BTC L2 的衍生品交易平台」，允许用户基于 BTC 做期权 / 合约交

易。理由如下：

• BTC 资产体量大，但 DeFi 场景稀缺

• Bitlayer 为 BTC 提供 EVM 环境，天然适合部署 ETH 世界的成熟模式

• 衍生品项目门槛高，竞争小，机会窗口尚在早期

所需商务支持：

• 来自 Bitlayer 基金会的技术支持（节点部署、数据同步等）

• 生态 Grant 支持初始开发资金

• 与 Bitlayer 上其他协议（如 AMM、稳定币）协作做初始流动性引导

• 联合市场宣传资源（AMA、Twitter space）
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


Circle Arc生态调研报告

一．项目背景

Arc 是一个专为稳定币金融和资产代币化而打造的 EVM 兼容 Layer-1 区块链。

Arc项目希望解决如下痛点：

原生代币Gas 费价格波动剧烈带来了不可预测的运营成本与会计复杂度；

商业交易所需的隐私保护功能缺失；-

完全公开、无需许可的交易提交机制催生了“最大可提取价值”（MEV）现象；

稳定币流动性与应用分布在多条链上，造成用户与开发者的高摩擦成本

Arc提出的解决方案要点是：

• **USDC as Native Gas**​：以稳定的 USDC 支付费用，使成本可预测。

• **Built-in FX Engine**​：链上无缝的货币互换。

• **Sub-Second Finality**​：交易在不到一秒内确认，确定且可靠。

• **Opt-in Privacy**​：使用受信任执行环境（TEEs）进行机密计算，如社区回复所指出。

• **Full Circle Integration**​：利用 Circle 的平台，便于稳定币的发行和管理。

• **EVM Compatibility**​：开发者可以使用熟悉的工具进行开发，让 DeFi 应用（包括潜在的 meme 代币发行）更易上手。

二．团队背景

Arc由Circle 主导开发，核心团队成员包括Gordon Y. Liao、Rachel Mayer、Adrian Soghoian 等，均来自 Circle。

Gordon Liao ​​Chief Economist and Head of Research at [Circle](https://www.circle.com/en/)

Rachel Mayer

Adrian Soghoian     Principal Software Engineer 

Adrian Soghoian

Sanket Jain   Principal Software Engineer, Gateway CEO

团队成员在稳定币，区块链基础设施搭建等领域均具备深厚积累。

三．投资方

项目白皮书中未提及Circle以外的投资方，我们可以简单的认为目前阶段Arc完全由Circle主导。

根据招股说明书显示（424B4 Prospectus \[Rule 424(b)(4)\] Accession Number: 0001193125-25-181916 链接：[424B4](https://www.sec.gov/Archives/edgar/data/1876042/000119312525181916/d839239d424b4.htm)），目前Circle的主要持有方及比例如下：

![](blob:https://intensivecolearn.ing/d6149a21-dab6-4f14-868b-123101425acb)

（最后一列未投票权比例）

四．项目规划

根据白皮书公布的项目路线图，Arc已知如下关键节点：

1\. 2025年秋季发布测试网；

2\. 主网Beta阶段包括包含稳定费率架构，亚秒级结算等核心功能；

3\. 后续升级包括隐私增强，mev缓解等，并过渡至PoS机制；

已公开项目活动有两项

\> Private testnet after public testnet launch

\> Grab the “Architect” role on Circle DC:

根据白皮书揭露信息，Arc 当前不强调发币（不排除未来可能），但从定位来看，很可能成为 Circle 打通全球支付和链上金融的关键拼图。

五．项目生态

项目生态聚焦三大领域

![](blob:https://intensivecolearn.ing/0a8e9a09-a4f5-4061-9d6c-39a5b596083b)

1． 赋予企业/开发者/消费者以及资产发行方；

2． 支撑原生应用（外部，支付和代币化资产等场景）；

3． 通过Circle的平台，衔接链上（跨链）和链下（传统金融）的互通；

我们可以得出推论，Arc的项目承接着Circle通过稳定币打通传统金融和链下资金流通的诉求，实现全场景的资金在链上的流动。

六．竞品分析

目前，Tether、Circle、Stripe等众多公司正纷纷推出其专用区块链，旨在服务于全球稳定币支付。项目的核心诉求均相似，因此存在很大的竞争关系。

![](blob:https://intensivecolearn.ing/68199e75-dcf8-4209-9401-b1fbf593e88e)

七．商务机会

Arc是一款专为稳定币金融打造的开放式 Layer-1 区块链。正如Arc描述的痛点所述，之前Defi项目面临着Gas波动，隐私审计缺失，使得机构用户使用充满疑虑，因此借助于Arc的稳定币特性，针对机构用户开发新一代Defi应用，获取客群使用。

可以有如下合作切入点：

1） 稳定币生态

支持 USDC/EURC、USDC / 人民币等合规货币对的 24/7 链上交易， 实现近似于传统外汇市场的流动性；

2） 低费率

利用 Arc 的 稳定 Gas 费率 降低用户借贷成本，以及快速成交特性，满足机构用户所需；

八．部署项目

若在Arc上部署项目，给予项目生态的描述，可以考虑部署一套企业跨境支付平台，功能要点有：

1） 跨境支付

Arc的Fx模块支持7×24 小时可编程支付对支付（PvP）结算，实现实时汇率的结算，支持多种稳定币的支付。打通法币与稳定的衔接；

2） 根据白皮书路线图所述，Arc规划selective transparency，满足保护敏感信息的同时支持监管的审计；

所需商务支持：

生态对接。接入Circle平台生态，实现与链下金融和跨链支持。

活动联合：联合 Circle 开展行业推广活动，触达中小企业客户。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



编写了一份研究报告

一、 项目概况与背景

Bitlayer 于 2023 年创立，是首个基于 BitVM 架构的比特币 Layer 2，目

标是成为“比特币世界的 Arbitrum”，愿景是释放 BTC 的 DeFi 潜力，构建

安全、高效、EVM 兼容的 BTC L2 网络。

• 团队成员情况

核心成员来自 Bitmain、Polygon、Polkadot 等头部项目，技术负责人

Kevin 擅长底层架构，联合创始人 Charlie 拥有丰富 BD 与生态拓展背景，团

队整体国际化，专注构建比特币原生 Layer 2。

• 融资信息见图

二、生态版图分析

• 生态阶段：2024 年初上线测试网，2024 年 Q2 上线主网，当前处于生

态冷启动阶段

• 生态项目类型：

o DeFi 协议：BitSwap、LendBTC、BTC Aggregator

o 稳定币项目：BTC 原生稳定币试验项目

o 桥接工具：BitBridge

o 开发工具类：BitScan、Explorer、Wallet 插件类工具• 龙头项目举例：

o BitSwap：首个在 Bitlayer 上部署的 AMM 协议

o BitBridge：官方主推的跨链桥，承接 BTC 入金出金

三、发展动态

• Hackathon/Grant：

o Bitlayer Foundation 已开启首轮“生态激励计划”，支持早期

项目部署，申请 Grant 或代币预分配

o 举办首期“Bitlayer Global Hackathon”，与 DoraHacks 联合

推出

• 线下活动：

o 在香港、东京、韩国等地参与比特币开发者线下聚会

• 生态增长指标：

o 主网上线 2 个月后 TVL 破 1 亿美金，增长快

o 日活钱包数与交互笔数持续上涨中

四、潜在合作机会点

• DeFi 方向：借贷、衍生品协议（Bitlayer 上尚无成熟的 lending 协

议）

• RWA & 稳定币：可探索 BTC 质押发行稳定币，或结合现实资产进行跨

链锚定

• 基础设施工具：数据分析仪表盘、钱包插件、消息订阅类服务（如

Dune、Zapper 角色）

• 合作方式建议：

o 可申请官方 Grant（开发者基金）

o 联合举办线上黑客松、技术 AMA

o 与 Bitlayer 社区和基金会对接生态资源、技术指导等五、如果我部署一个项目，我会做什么？

项目设想：

构建一个「BTC L2 的衍生品交易平台」，允许用户基于 BTC 做期权 / 合约交

易。理由如下：

• BTC 资产体量大，但 DeFi 场景稀缺

• Bitlayer 为 BTC 提供 EVM 环境，天然适合部署 ETH 世界的成熟模式

• 衍生品项目门槛高，竞争小，机会窗口尚在早期

所需商务支持：

• 来自 Bitlayer 基金会的技术支持（节点部署、数据同步等）

• 生态 Grant 支持初始开发资金

• 与 Bitlayer 上其他协议（如 AMM、稳定币）协作做初始流动性引导

• 联合市场宣传资源（AMA、Twitter space）
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




Web3工具

# 社交工具

-   X 公域平台，通过公开的渠道了解行业方向和重大事件
    
-   TG侧重于私域沟通，项目方和用户在群组中更深入的讨论和互动
    
-   Discord主要用于社区建设和协作
    

# 代币信息

CoinMarketCap/Coingecko

最常用的加密货币市场数据聚合平台，为用户提供实时的市场价格，交易量，代币信息和项目细节

-   市场价格
    
-   代币信息
    
-   交易所信息
    
-   历史价格
    
-   社媒和社区信息
    

DexScreener

聚合链上Dex平台

# 链上数据

Dune Analytics

集合查询，可视化，共享和探索公共信息的数据分析平台

DefiLlama

目前最全的defi数据聚合平台

区块链浏览器

查询链上数据交易的公共信息

# 钱包

EOA类钱包，私钥由用户持有

常见的有metamask，phantom

交易所钱包

okx，binance钱包

硬件钱包等
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->





以下记录

1 Gas优化方式

在 EVM 中，Gas 主要分为三类：

1.  **计算消耗**：运算、函数调用
    
2.  **存储消耗**：对 storage 的读写
    
3.  **交易消耗**：部署合约、转账、事件日志
    

其中**storage 写入**Gas消耗尤为重要

可以考虑如下场景

优化前：

function A(uint256 x) external {

    for (uint i = 0; i < 10; i++) {

        y += x;

    }

}

// 优化后：

function B(uint256 x) external {

    uint256 tmp = y;

    for (uint i = 0; i < 10; i++) {

        tmp += x;

    }

    y = tmp;

}

优化前Gas消耗量

20000 + 1000 +  20000 _10 + 10_ （计算操作的gas消耗量）  = 221000

其中循环10次写入

优化后

20000 + 1000 + 10\* （计算gas消耗量） + 20000 （最后一次写入）

可以看出gas的消耗量大幅优化

2.  solidity
    

智能合约开发中，重放攻击属于最常见的安全威胁，仍以《web3实习手册》中提出的漏洞为例

漏洞

// ❌ 有漏洞

function withdraw() public {

    require(balance\[msg.sender\] > 0);

    (bool sent,) = [msg.sender.call](http://msg.sender.call){value: balance\[msg.sender\]}("");

    require(sent);

    balance\[msg.sender\] = 0;

}

// ✅ 修复后

function withdraw() public {

    uint256 amount = balance\[msg.sender\];

    balance\[msg.sender\] = 0;

    (bool sent,) = [msg.sender.call](http://msg.sender.call){value: amount}("");

    require(sent);

}

示例中采用扣除后更新状态的方案

此外，也可以使用装饰器加锁的方案，代码如下

Contract example{

       bool private lock;

    mapping(address => uint256) private balance;

       modifier reentrancy(){

       require(!lock, ‘no reentrancy);

       lock = true;

\_;

Lock = false;

}

   Function withdraw() public reentrancy {

    Uint256 amount = balance\[msg.sender\];

       Balance\[msg.sender\] = 0;

           (bool sent,) = [msg.sender.call](http://msg.sender.call){value: amount}("");

           require(sent, “failed to send);

}
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->






24-25区块链主要进展记录

Ordi开启的BRC20铭文

Pepe开启的meme coin周期

Pump Fun 成为新资产放行的标准和范式，大幅降低了发币的门槛

引发两个赛道

1， 钱包

2， Telegram Trading Bot

3， 引领Cex融合链上交易（Alpha）

4， Polymarket

5， Hyperliquid

外部

1， 比特币和以太坊现货ETF

[https://www.theblock.co/data/crypto-markets/](https://www.theblock.co/data/crypto-markets/)

比特币ETF

![图片1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/panrui1984/images/2026-01-18-1768740044435-__1.png)

以太坊ETF

![图片2.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/panrui1984/images/2026-01-18-1768740052855-__2.png)

2， 币股

[BitcoinTreasuries.NET](http://BitcoinTreasuries.NET) [\- Top Bitcoin Treasury Companies](https://bitcointreasuries.net/)

![图片3.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/panrui1984/images/2026-01-18-1768740064641-__3.png)

3， 稳定币法案

[Total Stablecoin Supply](https://www.theblock.co/data/stablecoins/usd-pegged/total-stablecoin-supply)

![图片4.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/panrui1984/images/2026-01-18-1768740075955-__4.png)

4， RWA

美股，美债等更多资产上链

ICO合规化（Project Crypto）

5，401k

允许美国养老金 401K 投资加密货币、黄金、私募股权（PE）等另类资产
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->







模仿写的调研报告

一.   项目介绍

Blast 是以太坊上的一个 Layer 2 网络，它不仅通过Optimistic Rollups提高交易吞吐量，还能为桥接的 ETH 和稳定币自动生成原生收益。通过将质押和去中心化金融（DeFi）收益策略直接整合到网络中，Blast 为用户提供了一种无需复杂设置即可赚取被动收入的便捷方式。

该技术会先在链下捆绑多笔交易，然后将它们作为单笔交易提交到以太坊主网。这种方法在显著降低 gas 费的同时，仍能保持以太坊的安全保障。

以下是 Blast 技术架构的独特之处：

-   它采用自动rebasing机制，可自动分配 ETH 质押奖励
    
-   协议会桥接资产获取产生收益而无需任何额外步骤
    
-   gas 费收益分成为用户提供了额外的收入来源
    
-   智能合约会自动将无收益的 ETH 转换为有收益的 ETH
    

项目叙事着眼于用户收益，是第一条提供ETH和稳定币质押固定收益的Layer2，为 Layer 2 账户内的资金提供被动生息的可能性。

二.   发展介绍

2023 年 11 月，Blast 由 NFT 平台 Blur 的创始人 PacMan 创立，

2024 年 2 月，主网启动

2024 年 6 月，启动空头

三．Layer2市场分析

以太坊Layer2生态呈现显著集中化趋势。Arbitrum位于领先地位，市占率41.12%，Base以22.53%位于第二，Optimis则以15.47%居于第三。这三大平台共同占有了以太坊Layer2 DeFi TVL的80%，而剩余20%的份额由包括Blast分占，各家平台的差距不大。

数据截至2024年12月31日

四．代币经济学

Blast的token总计1000亿枚，50%分配给社区，25.5%分配给核心投资者。

第一阶段空头占总供应量的17%，积分持有者可以分配到70亿的token，黄金积分持有者分配70亿，基金会分配30亿。

二阶段空头原计划2025年6月结束，总计分配100亿Token。由于Blast 移动应用的发布，项目方转向新的代币经济模式，让用户和去中心化应用能够直接获得可流通的 BLAST 奖励，而非通过积分和黄金。

实际操作结果反映项目方实质上保护了散户的利益，但由于大户的线性释放策略（6个月释放完成）,造成“散户先跑，大户殿后”的结果。

五．核心指标

提交AI给出的Layer2核心指标可以分为4类：

**1）性能与可扩展性指标**

交易吞吐量（TPS），交易手续费和交易最终确认时间;

**2）经济指标**

a.总锁定价值（Total Value Locked, TVL）：存入二层网络生态中的资产总价值（通常以美元计价），直接体现用户对该网络的信任程度 ——TVL 越高，说明用户愿意投入的资产越多，网络经济基础越稳固。

b.交易量（Trading Volume）：二层网络上交易所处理的资产交易总价值，反映网络内的市场活跃度和流动性 —— 交易量越高，说明网络内的资产流通越频繁，生态实用性越强。

**3）安全与基础设施指标**

这类指标决定二层网络的安全性、抗风险能力及去中心化程度，是保障用户资产安全的核心前提

**4）用户与社区指标**

包含活跃用户和社区氛围

从运营角度，我们关注经济指标和用户指标。

**其中，TVL 是 Layer 2的最核心指标，是判断项目开展的标准**。

六. 项目运营策略

Blast项目方设计了一套积分策略。

针对最关键的核心要素用户和社区，分别制定了Blast Point（用户）和Blast Golden（DApp项目）两套方案，各占50%比例。

也可以看出，Blast的设计优点为规则明确，简单易懂。

因其团队Blur的背景， Blast 在积分空投的高预期下，仅在三个多月内迅速吸引高达 23 亿美元的巨额资金。

七．运营建议

根据第二节市占率分析，Blast在6月空投后经历了大规模用户流失，面临复苏挑战。Blast的总锁定价值（TVL）从历史最高点下跌62%，每日活跃用户数量降至六个月来的最低水平。相较于Base和Arbitrum差距拉大。

不仅空投参与者对Blast的策略不满（忽略大户的利益），项目生态也面临逃离的风险。

例如，曾是该二层网络上最大的模因币Pacmoon正迁移至Solana。Pacmoon的Lamboland在X平台上表示：“我们认为，代币、社区和文化是区块链成功的关键。然而，Blast从未关注这些。实际上，他们创建了一个系统，使得Blast上的原生代币受到抑制，并且未提供任何社会支持。”

分析这种问题原因包括：

1．NFT市场的降温

2．代币价格的大幅下跌

3．积分策略和真实生态的错配

我们认为，Blast面临挑战有区块链市场热度发展的客观因素，其自身的策略更多的关注于短期因素，即使不能否认项目方重视项目生态，但相较于竞争对手，忽略了生态的持续发展。造成了用户为了积分参与，空头发放即巅峰的现实结果。
<!-- DAILY_CHECKIN_2026-01-17_END -->

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
