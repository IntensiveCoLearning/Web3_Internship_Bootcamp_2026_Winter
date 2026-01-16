---
timezone: UTC+8
---

# Rosemo1

**GitHub ID:** Rosemo1

**Telegram:** rosemo

## Self-introduction

cooking at solidity

## Notes

<!-- Content_START -->
# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->
今晚读完了《021学习以太坊的最后一章》，以下是概括性总结：

### 一、早期共识机制：工作量证明（PoW）

以太坊主网 2015 年上线时采用 PoW 共识，核心基于比特币验证过的安全模型，选用 Ethash 算法保障去中心化。PoW 阶段的核心价值的是：依托成熟技术快速启动网络，吸引普通 GPU 矿工参与，为智能合约生态落地争取时间，同时作为过渡方案，为后续转向权益证明（PoS）铺路。

### 二、The Merge的意义

2022 年 9 月完成的 “The Merge” 是以太坊里程碑式升级，核心是将原 PoW 执行层（主网）与 PoS 信标链（共识层）合并，关闭挖矿机制，由验证者接管出块与共识。其关键影响包括：能耗骤降约 99.95%，契合 ESG 合规需求；安全模型从 “算力竞争” 转为 “质押经济约束”，引入 Slashing 惩罚机制；未直接提升 TPS，但为后续扩容（分片、Rollup）清理技术债；叠加 EIP-1559 销毁机制，使 ETH 供给具备通缩潜力。

### 三、权益证明（PoS）的本质

PoS 的核心逻辑是 “质押权益换共识权限”：验证者需质押 ETH（最低 32 ETH），协议按质押量、随机性等因素选择出块者，诚实行为获奖励，作恶则面临 Slashing（罚没质押）。相比 PoW，PoS 带来多重变革：能源效率大幅提升，经济攻击成本可量化；为分片技术奠定基础，出块节奏更稳定。

### 四、验证者惩罚机制

Slashing 是 PoS 安全的核心保障，针对恶意行为实施多层惩罚：即时扣除约 1 ETH 罚金；36 天强制退出期内持续损失质押；若大规模验证者协同作恶，触发关联性罚金，极端情况下可能损失全部质押；被罚没者永久失去验证资格，声誉与经济双重受损。

### 五、PoS 对能耗与发行速度的影响

能耗方面，PoS 彻底告别挖矿算力竞争，能耗仅为 PoW 时代的 0.05%，从 “能源密集型” 转为 “低功耗质押网络”。发行速度上，PoW 时代年通胀率约 4%-4.6%，PoS 时代降至 0.5% 左右，当 EIP-1559 销毁量超过新发行量时，ETH 呈现净通缩状态，强化其价值存储属性。

### 六、历次核心升级的系统改进

以太坊升级沿 “稳定 - 优化 - 扩容” 路径演进：早期 Frontier/Homestead 实现主网启动与基础稳定；Byzantium/Constantinople 引入隐私预编译、优化 Gas 成本；London 升级落地 EIP-1559，重构手续费市场；The Merge 完成共识切换；Shanghai/Capella 解锁质押 ETH 提款，提升流动性；Dencun 通过 EIP-4844 引入 Blob 交易，大幅降低 L2 数据成本；Pectra 升级优化账户抽象、提升验证者质押上限，进一步适配 L2 生态。

### 七、分片、Danksharding 与 Verkle 树

三者是以太坊扩容与轻量化的核心技术组合：分片（Sharding）通过拆分数据与计算负载，缓解全节点压力；Danksharding（数据分片）聚焦为 Rollup 提供廉价数据存储（Blob 交易），通过数据可用性采样（DAS）保障安全，是 “Rollup-centric” 路线的关键；Verkle 树替换传统 MPT 状态树，缩小证明体积，为无状态客户端铺路，降低节点存储与带宽门槛，强化去中心化。

### 八、DeFi、NFT 与 Layer-2 生态亮点

-   DeFi：流动质押成为最大板块，再质押将安全性代币化，真实世界资产上链成为新增长点，跨链消息协议与 AI+DeFi 组合优化用户体验。
    
-   NFT：从 PFP 投机转向多元化场景，游戏道具、品牌会员凭证、票务凭证成为主流，与 RWA、链上身份深度结合。
    
-   Layer-2：Rollup 成为扩容主力，Dencun 升级后手续费大幅下降，Optimistic Rollup 与 ZK-Rollup 并行发展，主网逐渐转型为高安全数据可用性层。
    

  
因为有一点solidity基础，明天开始对入门技术进行开发争取尽快完成然后进入深度技术完成任务。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->

## 学习了《021学习以太坊》的四五章 以下是一些概括性笔记

###   
第四章：智能合约理论基础

\-智能合约是部署在区块链的代码与持久状态，通过 Solidity 等语言编写，经编译部署后自动执行，支持逻辑升级与公开审计。

关键要点

1.  概念与功能：遵循 “如果那么” 逻辑，自动执行合约条款，支持代币发行、DeFi、NFT 等场景，具备公开可验证、结果可预测特性。
    
2.  Solidity 语言：适配 EVM，语法易上手，生态工具丰富，支持多种高级特性，是 EVM 生态主流合约语言。
    
3.  编译产物：含字节码（创建时与运行时）、ABI（交互说明书）、元数据等，为部署和交互提供基础。
    
4.  部署与成本：部署后获得唯一合约地址，成本按 Gas 计算，受代码复杂度、网络拥堵程度影响。
    
5.  安全与工具：常见漏洞包括重入、整数溢出等，需通过 CEI 模式、安全库防范；部署工具涵盖 Remix、Hardhat、Foundry 等，适配不同开发场景。
    
6.  公开性与升级：部署后字节码、交易历史公开，支持源码验证；逻辑升级以代理模式为主，避免直接修改已部署代码。
    

* * *

### 第五章：EVM 与 Gas 机制

\-EVM 是以太坊的执行引擎，负责运行合约字节码，Gas 机制通过计价与限额防止网络滥用，同时影响交易成本与执行效率，可通过优化降低 Gas 消耗。

1.  EVM 本质：运行在每个节点的虚拟计算机，维护栈、内存、存储等组件，确保合约执行的确定性与一致性。
    
2.  重要性：提供统一执行环境，支持智能合约与 DApp 运行，保障全网状态同步，是以太坊生态的核心基础。
    
3.  运行机制：合约经编译为字节码，通过 opcode 逐条执行，按规则更新链上状态，执行过程受 Gas 限制。
    
4.  Gas 机制： opcode 均有 Gas 成本，防止 DoS 攻击与无限循环；London 升级引入 BaseFee（销毁）与 Tip（奖励验证者），费用更可预测。
    
5.  计量单位：Gas 是计算单位，Gwei 是常用计价单位，1 ETH=10^9 Gwei，总费用 = GasUsed×（BaseFee+Tip）。
    
6.  异常与优化：Gas 耗尽会导致交易回滚，已耗 Gas 不退还；可通过减少存储写入、优化数据位置、避免无限制循环等降低 Gas 成本。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


完成任务：  
读完了XiaoHai老师的《021学习以太坊》前三章。以下是个人总结的一些比较重要的概括性笔记。

###   
第一章：认识以太坊

1.  起源与发展：2013 年 Vitalik 提出，2015 年主网上线，2022 年完成 PoW 到 PoS 的 “合并” 升级。
    
2.  核心定位：可编程公共区块链，以 EVM 支持智能合约，是 DeFi、NFT 等生态的核心载体。
    
3.  ETH 职能：网络燃料、PoS 质押保证金、价值储存与交换媒介。
    
4.  生态特点：与比特币差异显著，生态涵盖 DeFi、NFT、DAO，通过 L2 Rollup 扩容。
    

* * *

### 第二章：网络结构与节点类型

1.  节点构成：合并后由执行客户端和共识客户端组成，通过 Engine API 协同。
    
2.  节点类型：全节点、归档节点以及轻节点。
    
3.  通信与同步：通过 UDP+Kademlia 发现节点，TCP+RLPx 建立加密连接，Gossip 协议传播数据；同步模式分全同步、快照同步、轻同步。
    
4.  搭建要点：选节点类型与环境，安装 EL+CL 客户端，配置同步模式与 RPC，可通过云服务器或本地设备部署。
    

* * *

### 第三章：账户类型与结构

1.  账户类型：EOA、合约账户。
    
2.  地址生成规则：CREATE 模式由部署者地址 + nonce 计算，CREATE2 模式可通过部署者地址 + salt+init\_code 哈希预计算地址，支持可预测部署。
    
3.  核心差异：EOA 依赖私钥，合约账户依赖代码；EOA 主动交易，合约账户被动执行。
    
4.  核心机制：两者可互相调用，ERC-20/721 代币本质是合约账户的记账系统，合约部署后代码默认不可篡改。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



Task1：创建钱包并进行测试币转账

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Rosemo1/images/2026-01-12-1768230265244-image.png)

交易id：0xefd8ba3bfa5b7cae25f9c37ef42356e6da34805f8b42906576b801339d12cd74  
  
Task2：google canlendar

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Rosemo1/images/2026-01-12-1768230599039-image.png)

  
Task3：中文周会  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Rosemo1/images/2026-01-12-1768231585322-image.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
