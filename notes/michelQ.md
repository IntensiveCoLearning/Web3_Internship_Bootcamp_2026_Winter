---
timezone: UTC+8
---

# michelQ

**GitHub ID:** michelQ

**Telegram:** @Yukon Kin

## Self-introduction

a web3 newbie

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
今日主要学习了 DApp开发。学习了 DApp 应用的架构设计，交互过程等概念。前端一般使用 ReactJS 或 VueJS 等技术框架，和 Web2 无差异。主要差异在于要使用 Ether.js 或 Web3.js 等框架与合约交互。要了解ABI 的调用方式和方法等。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

今天主要学习了 Solidity 语言相关的内容。学习了 Call，DelegateCall,MultiCall，Gas，合约升级等知识。

1、什么是 Message Call ？

根据 Solidity 官网的描述，通过**Message Call**，合约可以调用其他合约或者发送以太币到其他非合约账号。

Message Call 和 交易很相似，都有类似的数据项：

-   source
    
-   target
    
-   data payload
    
-   Ether
    
-   Gas
    
-   return data
    

事实上，每个事务都由一个顶层消息调用组成，而顶层消息调用又可以创建进一步的消息调用。

消息调用具有一定的深度限制，理论值限制为 1024。实际实践过程中因为Gas 费只能转发63/64 的因素，导致调用深度一般都小于 1000。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/michelQ/images/2026-01-14-1768400995987-image.png)

2、什么是 Delegate Call?

delegate call 是特殊的 message call，在调用过程中，上下文中的 msg.sender 和 msg.value 不变。Delegate Call 可以在运行时动态加载Code，为实现库合约提供了可能性。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/michelQ/images/2026-01-14-1768401025930-image.png)

3、什么是 MultiCall?

以太坊的 Multicall 是一种通过一次区块链请求来批量查询多个智能合约数据的技术。这种方法可以有效减少链上的读取操作，提高效率，并节省 gas 费用。在以太坊生态系统中，Multicall 已被广泛应用于数据聚合和去中心化金融服务中。Multicall 合约的核心是一个能够接收多个调用数据并返回相应结果的智能合约。这个合约通常具有一个 `aggregate` 方法，该方法可以接受多个调用数据，并返回这些调用的数据结果。

4、合约为什么要升级？什么是合约升级？目前有哪些升级方案？

以太坊上的合约是具有不可变性的，一旦部署上链即不可更改。这也是以太坊可信任的基石之一。

合约有Bug需要修复或者项目方需要变更合约功能，则需要对合约进行升级。

为了修复错误或升级合约功能，只能发布一个新的合约替换掉原来的旧合约，但这样会导致现有的用户必须迁移到新的合约地址，并且所有的历史合约数据也都要迁移到新的合约中。这就带来了很多麻烦的事情。

为了解决这个问题，目前主要采用代理模式进行合约升级管理，有如下几种合约升级方案:

1）透明代理

2）UUPS 代理

3）信标代理

钻石标准 (**EIP-2535**) 将代理升级扩展到更精细的级别。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



初步了解了 Web3 的行业情况。关注了推荐的 Web3 行业的 X 账号，可以通过这些账号获取到更多的 Web3 行业资讯。尝试铸造自己的第一个 NFT，可惜网站一直提示 401 错误，目前还未解决，后面再慢慢解决吧。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
