---
timezone: UTC-5
---

# sophiez

**GitHub ID:** sophiezhu557

**Telegram:** @sophie08267

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
《021学习以太坊》概念自学总结：  
  
EL和CL其实各有链，但不是独立平行关系：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/sophiezhu557/images/2026-01-16-1768529594331-image.png)

Beacon chain：

-   决定时间、顺序、finality
    
-   负责 fork choice
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/sophiezhu557/images/2026-01-16-1768529617899-image.png)

平时说的 block header，多数指的是 Execution Block Header。基本只有validator才会看到共识区块的样子。

**Execution Chain**

-   只是一条“被认可的执行结果序列”
    
-   没有独立共识权
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/sophiezhu557/images/2026-01-16-1768529647861-image.png)

**ExecutionPayload：**一个“执行区块（Execution Block）的完整内容”，被塞进 Beacon Block 里，供共识层投票用。

state trie：

在逻辑上独立于CL和EL存在。

-   当前以太坊的世界状态 = 一棵state trie
    
-   每个区块执行后 → 得到新的 trie
    
-   用它的 root hash（stateRoot）放进 block header
    

在实现层：

**EL：**

-   **完整维护 state trie**
    
-   执行交易时：
    
    -   读 trie
        
    -   写 trie
        
    -   更新 storage trie
        
-   计算新的 stateRoot
    
-   把 stateRoot 塞进 executionPayload
    

EL **理解并操作 trie**

### CL:

-   **完全不理解 trie**
    
-   只做一件事：检查 executionPayload.stateRoot 是否一致
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

今天看了Bruce讲的web3运行原理。结合前几天看的《021学习以太坊》，稍微整理了一部分知识：

-   私钥、公钥
    
    -   固定的助记词可以生成多个私钥。理论上私钥只有用户自己知道。
        
    -   私钥在hash变换以后生成一个公钥，无法通过公钥反推出私钥。公钥即所谓的钱包地址，收款的时候就是给别人这个公钥
        
    -   一个私钥就是一个钱包。任何人都可以随时创建私钥，即拥有一个钱包，而无需通过任何机构的审核，使用也不受地理限制。
        
-   交易：
    
    -   用户要做的事+手续费（gas fee）+防重放序号（nonce）
        
    -   每个用户都有各自的nonce标记当次交易的历史顺序
        
    -   数字签名：在交易最后由用户确认，用私钥生成。任何人拿到签名以后都可以验证是不是发起者本人签的名。
        

数字签名和验证流程：1. 私钥+交易信息可以生成一个数字签名；

1.  通过RPC广播到区块链中；
    
2.  其他节点接收到的信息be like：一条交易信息（from xxx to xxx， amount， nonce）；发送的地址（公钥）；数字签名
    
3.  节点进行验证，计算出数字签名的签署者，与声称发送的地址进行比对。如果一模一样则交易成立。（这个验证应该是由validator进行的？）
    

-   一笔交易的生命周期：
    
    1.  钱包签名
        
    2.  RPC传播到各节点（共识客户端CL）
        
    3.  mempool排队
        
    4.  builder/validator（挑选）
        
        交易被打包进区块（status还是unfinalized）
        
        随机挑选validator投票验证
        
    5.  block落盘
        
    6.  explorer（可查）
        

“不可篡改性” 需要时间，因为同一个parent block下可能同时出现多个子区块，这时候得看哪个子区块最长。

-   以太坊的结构：
    

节点+beacon chain

节点：CL（共识客户端）+ EL（执行客户端）

validator是一个挂靠在CL的签名模块，不进行判断，只在验证结果上签名。不算一个客户端。

CL的工作就是实现写在beacon chain上的协议，维护、同步beacon chain。每个 CL 节点本地都维护的一份账本，也就是**beacon chain**

beacon chain就是账本，负责记录时间（slot）、单位时间的随机性、validator列表、每个validator的state、投票结果、最终确定区块、区块之间的头尾连接。beacon chain存储在CL里，每次CL们之间同步就是在同步beacon的状态。

在投票生成新区块的过程中，是CL 根据 Beacon Chain 的状态和规则，在每个 12s slot 本地算出：谁是 proposer、谁是 attesters（投票者），由 **EL（执行客户端）** 进行交易验证，最终由validator进行签名（attest），决定新区块是不是 canonical（fork choice rule）以及是否最终确定（finalized）。  
proposer和attester都是节点。是proposer在mempool中捞起各种交易打包成区块的，然后由attesters投票决议是否通过这个区块。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->


这是第一篇打卡，记录一下进入共学营迄今为止的摄入的一些web3知识和资源：

从上周五开始，在ChatGPT的帮助下细啃Xiaohai老师的《021学习以太坊》，获益匪浅。大大提升了我以太坊的认知，看了几章下来以后，感觉自己终于知道web3的很多讨论是在说什么了，以及更理解了以太坊这类可编程区块链的革新之处；

今天听了Emily老师讲web3领域的一些岗位分类。这其实是本人一直绕不开在思考的一个问题，就是我这样的背景究竟适合哪类岗位：本硕学统计出身，代码能力不强；喜欢了解技术细节（有时候有执念的程度，因为学统计做科研会用到很多统计学模型，用之前不搞清楚原理会非常难受），能从代码实践中获得成就感，虽然总是失败；喜欢从上到下，由宏观到微观了解各类政治的、社会的事件（在关注故事的时候这个点比较有用吧可能）。目前我无法给自己确定出非常具体的岗位走向，但短期目标还是比较确定的，就是多做一些项目，多和大佬聊天，多认识一些人。我i相信这个世界上有很多可能性，要知道怎么走向他们得先看到他们。

一点收尾：

对我来说很多事情是“万事开头难”，找到进入的门一般会花比较久的时间。开始关注web3至今其实已经快两年，两年前的我听张潇雨的播客《得意忘形》就捕捉到了web3这个关键词，但那时候学业压力很大，而且精神状态堪忧，自己其实没有心力仔细审视自己的人生，所以一直没有迈出心理上的舒适区。后来跌跌撞撞读完硕士，硕士期间依然断断续续关注web3，了解一些底层技术概念，依然不得其门而入，所以乱撞至今。去年决定不搞学术，找到了工作，终于有喘口气思考人生的机会了，现在觉得能有夜晚和周末的时间来学这些，还能认识不同背景的朋友，真是挺不错的hhhh
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
