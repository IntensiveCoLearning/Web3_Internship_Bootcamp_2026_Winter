---
timezone: UTC+8
---

# Kurtney

**GitHub ID:** KurtneySh

**Telegram:** 

## Self-introduction

get hands dirty

## Notes

<!-- Content_START -->
# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->
其他助教总结的求职资源：[https://share.note.youdao.com/ynoteshare/index.html?id=0355cbd9824ca3037933341cc49726bb&type=notebook&\_time=1768991251724#/WEB860cb16e6da9ed8ee68702752cc285bd](https://share.note.youdao.com/ynoteshare/index.html?id=0355cbd9824ca3037933341cc49726bb&type=notebook&_time=1768991251724#/WEB860cb16e6da9ed8ee68702752cc285bd)
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->

1.  学习分享帖子
    
2.  学习 ERC 7962
    
3.  学习 reactive contracts
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->


1.  参加 Speedrun Ethereum Basic workshop
    
2.  复习 uniswap 机制：[https://docs.google.com/presentation/d/12jWBmU08H8L6961tD6gg28fTBS8y6m9FOzJozWwi4-A/edit?pli=1&slide=id.g3bab87f6ebd\_0\_0#slide=id.g3bab87f6ebd\_0\_0](https://docs.google.com/presentation/d/12jWBmU08H8L6961tD6gg28fTBS8y6m9FOzJozWwi4-A/edit?pli=1&slide=id.g3bab87f6ebd_0_0#slide=id.g3bab87f6ebd_0_0)
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->



学习dapp架构

![9716d6d2cbdfc263148e45a661774d2a.PNG](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/KurtneySh/images/2026-01-22-1769076812007-9716d6d2cbdfc263148e45a661774d2a.PNG)
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->




1.  参加 colearning 了解职业发展
    
2.  学习 uniswap 机制
    
3.  学习 ai 工具 notebook llm
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->





1.  remix 学习
    
2.  solidity 学习
    
    -   gas 优化：减少存储操作（Storage Write）、使用位压缩（Bit Packing，将多个变量压缩到一个 uint256、循环优化（减少不必要的运算）、函数可见性选择（external 比 public 更节省 gas）
        
    -   合约安全：重入攻击 Reentrancy、预言机操纵 Oracle Manipulation、整数溢出/下溢、权限控制缺失、未初始化代理、前置交易/三明治攻击
        
    -   Ethernaut 闯关（fallback、fallout）
        
3.  实战
    
    -   链上留言板 dapp 开发、部署前端到 pinata
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->






1.  参加以太坊中文周会
    
2.  参加线上Co-Learning
    

思考问题：

X402：是什么？尝试解决什么问题？解决了么？如何实现了Agent2Agent之间的付款

是建立在 HTTP 402 状态码之上的支付协议；

尝试解决 **AI 时代的支付问题**：

传统支付系统不适合 AI 时代

-   在微型支付里，手续费比支付金额还高
    
-   传统金融结算时间过长
    
-   需要账户、kyc等门槛
    

解决了吗？

还在发展当中

如何实现 Agent 到 Agent 的付款？

核心流程：

-   客户端请求资源 → 服务器返回 402 状态码和支付要求
    
-   客户端准备支付 → 创建签名的支付载荷（PaymentPayload）
    
-   附带支付重试 → 在 HTTP 请求头中包含 X-PAYMENT 头部
    
-   验证和结算 → 通过 Facilitator验证支付并在区块链上结算
    
-   授权访问 → 验证成功后服务器返回 200 OK 和请求的资源
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->







1.  学习mpc钱包代码
    
2.  参加Key Hash Based Tokens: 从 ERC-721 到 ERC-7962 分享会
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->








初步梳理下周分享会主题提纲——初探以太坊账户抽象

以太坊账户抽象：从概念到实践

一、什么是账户抽象？

核心概念

技术本质

账户抽象的创新

二、历史演进

三、ERC-4337

核心架构

运行流程

功能实现

四、EIP-7702

核心思想

与 4337 的对比

未来展望

五、应用举例

六、合规视角

七、总结回顾
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->









**学习：香港的数字金融实验：IPO上链能否成为下一个风口？**

时代背景

1.  链上原生货币的崛起
    
2.  宏观前提之二：监管框架的清晰化
    
3.  宏观前提之三：传统金融机构的战略入场
    
4.  AI Agent
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->










ERC-8004: 智能体的链上护照

身份注册表(Identity Registry)

1.  基于ERC-721标准
    
2.  提供唯一、可转移的链上ID
    

声誉系统(Reputation Registry)

1.  记录不可篡改的反馈评分(0-100分)
    
2.  防止垃圾信息：仅付费客户可评价
    

验证机制(Validation)

1.  TEE (可信执行环境)或zkML证明
    
2.  解决核心问题：“你是谁”和“你可信吗
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->











直观查看交易的完整生命周期：[https://txcity.io/v/eth-btc](https://txcity.io/v/eth-btc)

Builder/Validator（挑选） [https://beaconcha.in/block/24224597#votes](https://beaconcha.in/block/24224597#votes)

Block（落盘） [https://beaconcha.in/block/24224597](https://beaconcha.in/block/24224597)

查看节点软件占比：[https://ethernodes.org/](https://ethernodes.org/)

Rated Network Explorer 查看 Validators（验证者） [https://explorer.rated.network/network?network=mainnet&timeWindow=1d&rewardsMetric=average&geoDistType=all&hostDistType=all&soloProDist=stake](https://explorer.rated.network/network?network=mainnet&timeWindow=1d&rewardsMetric=average&geoDistType=all&hostDistType=all&soloProDist=stake)

思考题

资产自托管：如何提高安全性、降低管理私钥的复杂度？

账户抽象

-   4337将会被7702取代
    
-   7702的侵入性更小，是现在的主流实现，和主推方向。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->












# 以太坊中文周会纪要总结（2026/01/12）

## **一、技术进展**

-   **Fusaka升级**：已于2025年12月3日成功激活，实现PeerDAS数据可用性采样，blob容量上限提升至21
    
-   **L2生态**：zkSync将于Q1 2月份进行Atlas升级；Arbitrum因Robinhood代币化股票业务而活跃；StarkNet近期经历18分钟宕机
    

## **二、市场动态**

-   **预测市场**：Polymarket与Calibration等平台受关注
    
-   **永续DEX竞争**：HyperLiquid面临Lighter、Aster挑战，市场份额下滑
    
-   **dYdX**：已进入美国市场推出Solana现货交易
    

## **三、趋势展望**

-   **稳定币**：USDT、USDC及人民币稳定币持续发展
    
-   **ETF与机构化**：Crypto、Fireblock等大型经济体参与
    
-   **DeFi**：衍生品市场工具持续升级
    
-   **RWA代币化**：2025年增长229%，美国国债代币化规模翻倍，2026年预计持续增长
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
