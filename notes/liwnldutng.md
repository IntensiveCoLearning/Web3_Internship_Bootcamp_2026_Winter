---
timezone: UTC+8
---

# liwnldutng

**GitHub ID:** liwnldutng

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->
今天学的不多，主要是分享会还有周例会内容，周六日准备把solidity剩下部分做完，学习下uniswap这几个系列。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->

今天没有学习太多，简单看了下实习手册的智能合约部分，晚上听了分享会，大概总结为erc-8004借助x402为ai agent提供身份，可验证性Tee并引入评价机制，spoonos优化了ai-agent执行erc-8004并上链的操作。之前就参加过erc-8004的残酷共学，那时候还是啥也不懂，现在稍微能明晰一些。或许后面可以尝试下erc-8004与spoonos。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->


今天两场分享会感觉都比较有趣，可能因为实例比较多，跟现实关联更明显，感觉找工作还是没那么简单。

code方面跟着视频和github拖沓地把Raffle.sol的config写完了，除了辨别版本号那块，总体流程还是感觉区别不大，config主要还是根据不同block.chainid自动进行合约的配置。在localnetwork上做一定的mock。

学习了abstract contract 与interface、constructor的区别

| 能力 | abstract contract | contract | interface |
| --- | --- | --- | --- |
| 有 constructor | ✅ | ✅ | ❌ |
| 有状态变量 | ✅ | ✅ | ❌ |
| 有逻辑函数实现 | ✅ | ✅ | ❌ |
| 只能声明函数签名 | ❌ | ❌ | ✅ |
| 可继承 / override | ✅ | ✅ | ❌ |

总的来说，abstract contract 是一种不能直接部署、但可以包含状态、构造函数和部分实现的合约，用来作为可复用的基类；  
interface 只用于描述函数签名，不包含任何实现或状态；  
constructor 是部署时执行的初始化逻辑，interface 中不能存在。（此处参考chatgpt的回答）
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->



今天没有学太多，先设定下四周的目标吧：

岗位方向：优先技术岗 目标：多学习，找到个较满意的实习机会或者工作

第一周：完成之前solidity剩下的学习（YouTube课程）

第二周-第三周：完成或初步完成rust的学习，视困难程度变化（YouTube课程），看看一些大型hackson的项目，进行一些针对性学习。

第四周：完成casual-hackson，视项目需求进行学习，有空余继续学习rust或者move、cario（在完成rust的前提下）
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




今天看了以太坊一些背景知识，比如说pos的共识机制和slashing机制，NFT的含义、节点运行与质押，L2与L1的功能协作分片机制，L2的职责，扩容实现的技术路线。目前很多只知道一些名词概念，后面继续了解吧。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
