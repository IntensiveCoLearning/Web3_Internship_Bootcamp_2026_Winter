---
timezone: UTC+8
---

# JingLi13

**GitHub ID:** cs124jing

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->
**投研基础框架建立**

_什么是投研？_

-   研究项目的基本面：技术/团队/社区
    
-   研究项目背景：了解行业/赛道/竞争格局
    
-   研究经济模型：分析代币供应/分配/效用
    

_投研的本质_

主流币不需要投研，只针对山寨币以下的资产需要（流动性外溢效应）

-   深度挖掘
    
-   精准评估
    
-   时机把握
    

_当前市场的现状_：山寨币市场市值越来越低，其根本原因在于流动性不足

_流动性不足的原因_

-   资金谨慎
    
-   市场存量信心受挫
    
-   资金避险情绪浓厚
    

_未来的机会_

-   量化宽松
    
-   技术创新
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->

**Speedrun Basic workshop**

_如何设计智能合约以应对潜在的恶意行为，并确保参与者的资金安全？_

-   智能合约需要有人主动执行，以便将状态从存款转移到成功或失败状态
    
-   设计时须考虑恶意行为：假设参与者中可能存在不诚实的人
    
-   确保每个人都能取回自己的资金，防止资金被锁定或被盗
    
-   如果目标资金未达到，应确保所有参与者能够安全地取回自己的投资
    

_在区块链环境中如何安全地管理资金和代币？_

-   多签名钱包的重要性以及智能合约的应用
    
-   在组织中使用多签名钱包的必要性，以确保资金的安全性
    
-   代币转移过程中智能合约之间的通信机制
    

_如何使用简化的方法构建以太坊智能合约？_

-   通过工具创建以太坊合约变得更加简单，无需编写复杂的代码
    
-   骰子游戏：参与者可以通过攻击合约来赢取奖金。
    
-   如何利用区块哈希进行随机数攻击
    

_在区块链环境中随机数生成的公平性和安全性_

-   彩票系统中的应用
    
-   真实随机数/伪随机数
    

_以太坊协议中如何管理借贷的抵押品以确保资金安全？_

-   借贷过程中的抵押品机制：用150美元的以太坊作为借100美元玉米的抵押
    
-   借贷中必须保持的抵押品与借贷金额的比例，以避免损失
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->


**DApp：去中心化应用&逻辑和未来**

DApp是一个“自动贩卖机”

![截屏2026-01-23 09.28.22.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/cs124jing/images/2026-01-23-1769182105711-__2026-01-23_09.28.22.png)

_智能合约的执行逻辑_

-   api-> smart contract -> web3代码作为中间人
    
-   the ledger：可以查看所有人的交易记录
    
-   the flue：gas/governance 参与DApp的治理
    

_DApp工程化架Hybrid Stack_

-   **_Hardhat（TypeScript）_**：负责工程管理、部署流程与集成测试
    
-   **_Foundry（Rust）_**：负责合约编译、单元测试与 Fuzzing
    
-   混合使用 Hardhat 与 Foundry 是当前主流的 DApp 工程化方案
    

_how to initialize the project?_  
recom env：Node.js v20（LTS）+ pnpm

-   通过 Hardhat 初始化并注入 Foundry
    
-   Solidity 合约统一放在 contracts 目录，测试与构建流程走 Foundry
    

_智能合约工程化设计_

-   使用 Custom Errors 替代字符串报错以降低 gas
    
-   通过 Events 支持前端状态同步与索引
    
-   遵循 Checks–Effects–Interactions 模式以保证安全性
    
-   合约设计默认面向前端交互与长期维护
    

_测试策略（分层）_  
**Foundry：**

-   使用 Solidity 编写单元测试
    
-   通过 Fuzzing 自动生成随机输入，覆盖边界情况
    

**Hardhat + Viem：**

-   使用 TypeScript 编写集成测试
    
-   模拟真实前端对合约的调用流程
    

_部署方式升级_

-   采用 Hardhat Ignition 进行声明式部署
    
-   通过描述目标状态完成部署，支持自动重试、Gas 管理与模块化
    
-   避免使用传统命令式脚本
    

_前端架构_

-   使用 Next.js + Wagmi 构建现代 Web3 前端
    
-   解决 SSR 与浏览器状态不一致导致的 Hydration 问题
    

_Wagmi 配置要点_

-   启用 SSR 模式
    
-   使用 Cookie Storage 统一服务端与客户端状态
    
-   避免页面刷新闪烁与状态不同步问题
    

_核心写入交互流程_  
交易状态流转：

-   Idle → Pending → Mining → Success
    
-   使用 useWriteContract 发起交易
    
-   使用 useWaitForTransactionReceipt 监听交易确认
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->



**Uniswap工作原理解析**

_Uniswap v2: CPMM（Constant Product Market Maker—）/恒积做市商_

-   CPMM：不用撮合买卖双方、也不用挂单簿，池子自己按公式给你报价
    
-   _Uniswap v2：_很多“长尾币”（冷门币）主要靠它这种 DEX 去交易，它属于 AMM 自动做市商（Automated Market Maker）
    
-   x\*y=k 价格曲线:拿走 A，就必须放进一些 B，让乘积维持住
    
    -   x：池子里 Token A 的数量
        
    -   y：池子里 Token B 的数量
        
    -   k：常数
        

**_uniswap是什么？_**

-   _核心机制：_用 AMM + 流动性池做交易，不靠订单簿；池子里两种资产的比例决定价格，交易会改变比例->价格被推动（price impact）。
    
-   _套利：_当 Uniswap 价格和中心化交易所（如 Binance）不一致时，套利者低买高卖，把池子价格“拉回”市场价；池子越深（流动性越高）价格越稳、滑点越小。
    
-   _闪电操作/闪电贷_：一笔交易里“先借到资金->套利/清算->归还”，不满足条件就整体回滚，主要成本是 gas。
    
-   _LP（提供流动性）收益与风险_：LP赚手续费，但币价相对变化大时会出现 无常损失(IL）,相较于单纯持币可能更亏, 价格回到原区间 IL 才可能减少。
    
-   区间流动性与 tick：流动性在不同价格区间才“有效”，用tick限制在离散价格点加/撤流动性；价格跨tick时更新累计数据，用来计算 LP 在各区间应得的手续费。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->




**Solidity walk through**

**_EVM（Ethereum Virtual Machine）_**

-   一个Sandbox，所有的智能合约都在这个沙盒里运行，彼此隔离，且不直接接触外部硬件。它的核心任务是根据Transaction来更新区块链的“状态”
    
-   与solidity的关系：EVM 并不直接读懂 Solidity 语言。它们之间有一个转换过程：
    
    1.  **write :** 开发者写 Solidity 源代码
        
    2.  **compile:** 使用编译器将 Solidity 翻译成 EVM 字节码 (Bytecode)
        
    3.  **run:** 字节码被部署到链上，由 EVM 执行
        
-   solidity storage space:
    
    -   _Stack:_ 存放临时变量，读写极快，但空间很小（限制 1024 层）。
        
    -   _Memory_**:** 临时的、可擦除的空间，函数运行完就清空。
        
    -   _Storage:_ 永久保存在区块链上的空间，这是最贵的地方，需要支付大量的 Gas 费。
        
-   gas机制
    
    -   为了防止有人写死循环代码导致网络瘫痪，EVM 引入了 **Gas 机制**
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->





**Web3公共物品资金分配**

1.  _what is public good?_
    
    -   non-excludable
        
    -   non-rivalrous
        
    -   eg. public good in web3: 开源代码/协议安全/开发者工具或文档
        
    -   the free-rider problem:如果每个人都能免费使用，理性的选择是等待别人付费
        
2.  _三种资助机制_
    
    -   委员会制：
        
        -   pro:简单快速/决策效率高
            
        -   con:权力集中/偏好固化与偏见/有限的知识
            
    -   二次方资助（QF）：
        
        -   人数>金额
            
        -   配捐池的资金分配由贡献者的数量决定，放大社会的共识
            
        -   为公共性付费而不是为资本付费
            
    -   RetroPGF回溯性自助
        
        -   先创造价值，再获得奖励
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->






**Key Hash Based Tokens: 从 ERC-721 到 ERC-7962**

-   _ERC-721_
    
    -   经典NFT标准，定义了每个NFT的唯一性，只能mint/transfer/ownerOf
        
    -   没有approve机制，强调一次性key/key轮换
        
    -   ownerOf返回keyHash而不是地址
        
    -   NFT是基于ERC-721的实践
        
-   _ERC-7962_
    
    -   可扩展的NFT
        
    -   在ERC-721的基础上应用ZK原理保证了向第三方证明其归属，但不暴露真实地址
        
    -   **特点：**
        
        -   用KeyHash而不是address作为资产所有权标识
            
    -   **解决的关键问题：**
        
        -   隐私问题：避免地址可追踪
            
        -   UX问题：用户只需要签名表达意图
            
        -   扩展性问题：天然支持批量交易
            
    -   实际应用：在不暴露隐私的情况下进行数据交易
        

_What is zero knowledge?_

-   不暴露细节的情况下，向第三方证明数据的某些特点
    
-   eg. Proof(X< Y) to proof X + Y = Z but no reverse
    

_Gas和交易模型_

-   任何地址都可以提交交易
    
-   所有权只由签名控制
    
-   支持gas sponsorship/批量转账/replayer
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->







**Agent跟web3的融合**

_AI agent的结构_：the brain（LLM） --> the memory（RAG流程：抓取数据然后上下文存储） --> the tools(API调用）

_agent的缺点：_

-   identity spoofing：在传统网络中无法验证操作者是人类还是机器人。攻击人可以模仿agent执行未经授权操作
    
-   Untraceability不可审计：缺乏不可篡改的日志，如果ai签署不利合同，责任难以追述
    
-   Shadow AI：agent结构的安全部署缺乏统一治理框架
    

_Web3 ai协议栈_：为agent提供了不可篡改的身份证明和原生银行账户

-   **Application**：
    
    -   SpoonOS Framework:连接web2开发（python/longchain)与web3基础设施(wallet, smart contracts)的桥梁
        
        -   架构概览
            

![截屏2026-01-17 08.30.54.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/cs124jing/images/2026-01-17-1768660273271-__2026-01-17_08.30.54.png)

-   **Identity**：
    
    -   ERC-8004标准（链上护照）：
        
        -   Identity registry：提供唯一可转移的链上ID
            
        -   Reputation registry：记录不可篡改的反馈评分/防止垃圾信息
            
        -   Validation：解决核心问题（你是谁/你可信吗）
            
-   **Transaction**：X402协议（HTTP原生机器支付）
    

![截屏2026-01-17 08.28.49.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/cs124jing/images/2026-01-17-1768660142855-__2026-01-17_08.28.49.png)

web3是唯一能承载高频，低频机器交易的基础设施
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->








**web3安全**

-   精准猎杀
    
    -   CeFi：管理员私钥被盗，热钱包私钥被盗用
        
    -   DeFi漏洞：攻击者利用ai技术挖掘只能合约漏洞的能力越来越强
        
    -   社会工程：伪装成web3公司的招聘人员，通过技术面试让员工下载有后门的测试代码
        
    -   洗钱工业化：洗钱链条高低压自动化
        
-   广撒网（跟每一个用户息息相关）
    
    -   私钥窃取：基于木马病毒，社工合供应链渗透
        
    -   钓鱼攻击：ai生成钓鱼，全平台化
        
    -   Rug token：碎片化撤池，职业化
        
-   安全建议：
    
    -   不点，不签，不装，不转
        
    -   全周期安全管理：安全左移->全周期监控（集成安全API，透明化锁仓）->快速应急->协议层升级
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->









**web3运行原理**

1.  _私钥（private key）：_
    

-   从助记词分叉处的一个账户，可以算出公钥（public key）
    
-   用于验证签名
    
-   无法重置
    

2.  _助记词（seed phrase）：_一对多
    
3.  _数字签名是什么_
    
    -   本地生成签名-->光波导RPC（网络）--> RPC节点执行ecrecover验证
        
4.  _gas fee_：去中心网络的激励机制，根据每个transaction的gas fee多少来判断进入区块时间
    
5.  _区块交易完整周期_：wallet-->RPC/Node-->Mempool（排队）-->Builder/Validator selection--> Block --> Explorer（可查询阶段）
    
6.  不可篡改需要时间：等待一段时间才能确认新的区块，会引用上一个block的hash。区块越往后叠，历史被推翻的成本越高
    
    -   比特币：十分钟一个block
        
    -   以太坊：12/13秒
        
7.  共识机制：全球节点如何认可同一本账？
    
    -   PoW（proof of work) BTC: 算力竞争写账
        
    -   PoS（proof of stake）ETH：交押金->进列表当记账员，押得越多/信誉好概率更高，若作假会被罚没押金
        
8.  钱包-->连接RPC（访问节点的API）
    
9.  智能合约：区块链账本的可执行代码
    
    -   在虚拟机里运行
        
    -   交易触发执行-->改变链上状态
        
    -   写进区块链：难以篡改
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->










**行业全局介绍&岗位预览**

**_行业概览_**

-   对于复合型人才的需求在增加
    
-   目前加密货币总市值已达到3.1万亿美元。预计到2026年，Web3市场规模将达到69.4亿美元，年复合增长率高达49.8%，
    

**_Expected candidate_**

-   创始人通常来自传统互联网行业
    
-   偏好Top院校毕业生。
    
-   no prior experience is fine
    
-   English required
    
-   对某个领域深入研究会受到项目方的欢迎。
    
-   项目方非常重视开发者的安全意识。
    
-   运营:偏好有海外媒体运营经验的人选，英文口语能力重要。
    
-   产品和投研:更倾向于有相关背景的人。
    

**_个人职业发展及进入Web3行业的建议_**

-   选择项目很重要，尤其是刚毕业的求职者
    

**_Web3行业中高级投研岗位的要求和新人入行的建议。_**

-   高级投研岗位通常要求候选人具备一定的经验和过往项目成果。
    
-   投研岗位在整体工作岗位中占比较小，新人进入该行业的机会有限。
    
-   新人需具备代码阅读能力、代币经济学分析能力和数据分析能力，并准备相关报告作为入行的敲门砖。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->











**比特币&区块链是什么？**

-   区块链：一个按照顺序先后相连接区块的链条，具有不可被篡改，透明，快速交易的特点
    
-   比特币：一个具有货币属性的加密货币，网络节点服务运营商提供分布式服务的奖励。
    

**以太坊&以太币**

-   以太坊：一条区块链with智能合约
    
-   以太币：用于在以太坊上进行交易的货币
    

**FT(Fungible Token) &NFT(Non-fungible Token)**

-   Fungible Token: 无区别的代币，比如currency，每个不同的currency都一样。
    
-   Non-fungible Token: 每个代币都是独特的，比如一个艺术作品
    

**DYOR（Do your own research)**
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
