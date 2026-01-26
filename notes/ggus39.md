---
timezone: UTC+8
---

# ggus39

**GitHub ID:** ggus39

**Telegram:** @cyyaaq

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->
参加会议 偷学几招
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->

下午跑了一下午代码，战败cg
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->


做了一些深度技术，部署了一下foundry，晚点再看情况
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->



简单过了一下合约安全漏洞，回看了一些课，今天没什么劲，小歇，明天开始做一些深度技术的吧
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->





昨天其实还建立了一些GitHub仓库，接下来有准备慢慢完善，今天继续跟着学一会

![QQ_1769064229922.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ggus39/images/2026-01-22-1769064448667-QQ_1769064229922.png)

关键字：`blockhash(uint blockNumber)`: (`bytes32`) 给定区块的哈希值 – 只适用于最近的256个区块, 不包含当前区块。

-   `block.coinbase`: (`address payable`) 当前区块矿工的地址
    
-   `block.gaslimit`: (`uint`) 当前区块的gaslimit
    
-   `block.number`: (`uint`) 当前区块的number
    
-   `block.timestamp`: (`uint`) 当前区块的时间戳，为unix纪元以来的秒
    
-   `gasleft()`: (`uint256`) 剩余 gas
    
-   `msg.data`: (`bytes calldata`) 完整call data
    
-   `msg.sender`: (`address payable`) 消息发送者 (当前 caller)
    
-   `msg.sig`: (`bytes4`) calldata的前四个字节 (function identifier)
    
-   `msg.value`: (`uint`) 当前交易发送的 `wei` 值
    
-   `block.blobbasefee`: (`uint`) 当前区块的blob基础费用。这是Cancun升级新增的全局变量。
    
-   `blobhash(uint index)`: (`bytes32`) 返回跟当前交易关联的第 `index` 个blob的版本化哈希（第一个字节为版本号，当前为`0x01`，后面接KZG承诺的SHA256哈希的最后31个字节）。若当前交易不包含blob，则返回空字节。这是Cancun升级新增的全局变量。
    

![QQ_1769072398777.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ggus39/images/2026-01-22-1769072408972-QQ_1769072398777.png)

过了三关，等会看下回访，晚上要提交一些任务
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->






今天学习了solidity的DAICO,简单了解合约运行优选以及合约项目筹资

接下来会看一下做一些任务，多看一些技术向，过一些solidity101吧

过了三关，学的有点慢，之前学的和这个顺序不一样

![QQ_1768986935058.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ggus39/images/2026-01-21-1768986939512-QQ_1768986935058.png)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->







今天要把链上投票相关内容看完，详细看一下手册，会再看看昨天那个Ethernaut，然后再过几关的样子，时间够再按自己的节奏学一些solidity

![QQ_1768892030560.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ggus39/images/2026-01-20-1768892035405-QQ_1768892030560.png)![QQ_1768893474831.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ggus39/images/2026-01-20-1768893551149-QQ_1768893474831.png)
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->








今天看了一下之前的回放，测试了测试币转账，测试了一下链上投票

![QQ_1768803279473.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ggus39/images/2026-01-19-1768803359063-QQ_1768803279473.png)

，了解了一些基础，接下来会继续学习Solidity，先听一下周会，呼~，大约三个小时左右，通关The Ethernaut三关，这个比诈骗难哈哈哈，累了，休息完会看会回放
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->









周日，做一些小总结吧，等会会去看一下之前的一些课 以及之前的课件，大概率是会写一些小笔记，今天也会在推发一个小总结

进入web3或者说了解web3的开始大概是在2025.5左右d 样子，刚开始一两个月买了一些现货，后来到8月左右通过tg了解到一个社区做合约，合约在一定程度存在赌的性质，贪心，不甘心，是做合约最忌讳的事情，大概到10月我翻仓应该是有的，10.11晚上我是空仓的躲过了黑天鹅，但是不久之后同样的十月就亏了不少，因为考试，那段时间压力大，到考完j不就没动，考完一直上情绪把剩下的部分都亏掉了，是不多，也是因为这些我在推上进入了web3实习有幸被录取，

这一周主要是基本概念吧，安全合法以及运行原理，
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->










时间较少，平板写了一些智能合约的基础代码
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->











今天了解了NFT的基本概念，但是没有铸造NTF，也不着急嘛，接下来应该是要更多学习智能合约了，有的东西还是缺点，NFT目前的发展是在一个上升期甚至是一个爆发期，但是种种原因，web3在我看来入行还是要去境外好一点，我需要更长的时间沉淀一些
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->













今天把钓鱼的基本过完了，推的总结还在思考，看了一些回放，还在思考，得加一点时间，整体看起来有些东西占的时间太长了
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->















今天休息。。。。

简单了解了一些智能合约，还有一些平板的笔记，最近事情有点多，感觉没有头绪

![企业微信截图_1768390316297.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ggus39/images/2026-01-14-1768390326502-_______1768390316297.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

















今天过了十关多一点的关于诈骗钓鱼的，在GitHub上开了一个自己的小记录，下来可能会在推上也开始记录，不仅仅记录web3了可能就，也会在睡前写一个小日记，看了一些昨天的web3行业岗位，看了一些NFT的东西吧，坚持输出与学习！！！

![QQ_1768294824119.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ggus39/images/2026-01-13-1768294830637-QQ_1768294824119.png)

![74cf66bcbc111cedc7d64ecba7de9344.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ggus39/images/2026-01-13-1768294862401-74cf66bcbc111cedc7d64ecba7de9344.png)![a5f969307ddd3cee1f78de9348945702.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ggus39/images/2026-01-13-1768294866429-a5f969307ddd3cee1f78de9348945702.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->


















day1

今天过了一些无法钓鱼的那些钓鱼的几关，理解了一些钓鱼常用的手段，多个事实证明web3虽然有很大的发展空间，但是在一些方面仍然是有一些不足或者是危险的地方，谨慎又谨慎才能在这个地方活下来，之前就有注册钱包metamask，然后把测试网安装好，把node，一些编程的基础环境安装好了,也有在跟gemini学习模式学基本的智能合约

![f9dd704a-4568-4b2d-8c77-b45b41e799ca.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ggus39/images/2026-01-12-1768218880626-f9dd704a-4568-4b2d-8c77-b45b41e799ca.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
