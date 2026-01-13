---
timezone: UTC+8
---

# lingxingdell

**GitHub ID:** lingxingdell

**Telegram:** womianfeile

## Self-introduction

I am an undergraduate interested in application of Web3 in the creative field，including On-chain Pipeline, Decentralized ​Render Network and Programmable Royalties.

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
# 1.13

因为直播比较卡顿的缘故，打算明天等B站回放，于是自己看实习手册

主要收获是公链、联盟链、私链的区别、 web3.0 与 web3 的区别

复习了一下之前看的 Stanford 区块链与加密货币课程（接下来简称introduction），我将会按照实习手册的顺序结合MIT的课程，写一些想法

* * *

### 区块链基础概念

区块链的是怎么样 introduction 已经介绍过，我觉得区块链有点像数据结构中的链表，当然更复杂

区块如何连接：（以 bitcoin blockchain 为例）

![Screenshot_2026-01-13-23-10-31-558_tv.danmaku.bili.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/lingxingdell/images/2026-01-13-1768317049000-Screenshot_2026-01-13-23-10-31-558_tv.danmaku.bili.png)

block headers包含hash、timestamp（时间戳）、transactions hash、Nonce 四个字段（field）

交易被打包进区块当中

hash是 32 bytes 的字符串

![Screenshot_2026-01-13-23-07-29-808_tv.danmaku.bili.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/lingxingdell/images/2026-01-13-1768316956948-Screenshot_2026-01-13-23-07-29-808_tv.danmaku.bili.png)

将 block n-1 的所有数据通过哈希函数得到哈希值，这就是block n 中的 hash 字段

由此区块链以串行（serial progression）方式发展，每个区块都依赖于之前的所有区块

现在有更新的区块链并不遵循这种严格的串行结构

* * *

公链、联盟链和私链具有不同的去中心化程度、效率以及信息访问权限管理方式，我认为是为了适配不同需求

web3.0（语义网）是我新了解到的概念，从实习手册介绍来看，我目前觉得他主要只是一种数据管理方式的升级，可以构建语义关系网，既可以在 web2 中使用，也可以在 web3 中使用。（集成支付是语义网带来的升级吗？我觉得这份表格是否僵硬？）

实习手册上介绍的 web3 的去中心化是通过区块链网络实现，每个节点都将会存储一份相同的区块链数据，只要没人能控制51%以上的节点，就能保证区块链数据的准确。那与 PoW / PoS 的关系是什么？

更重要的是，如果通过复制式储存数据，而区块链的数据是随着时间递增的，我担心会面临严重的储存问题。（V神最近好像提出了分布式储存？）

高频社交为什么不适合 web3 ？
<!-- DAILY_CHECKIN_2026-01-13_END -->
<!-- Content_END -->
