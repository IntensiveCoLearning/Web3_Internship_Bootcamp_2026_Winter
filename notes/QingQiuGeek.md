---
timezone: UTC+8
---

# QingQiuGeek

**GitHub ID:** QingQiuGeek

**Telegram:** 

## Self-introduction

河南科技大学软件工程专业，大四，已有两段Java后端实习，计划转web3。对web3、ai感兴趣，掌握JavaWeb、docker、linux、数据库等常见后端开发技能以及react、js、ts、next.js基础前端知识，有独立开发全栈web项目的经验，对ai agent、mcp也有一定了解。

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
# **Web3 行业全局介绍 & 岗位概览**

## 发展规模

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768484798780-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485033688-image.png)

## 求职岗位

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485051000-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485156432-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485182238-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485292440-image.png)

## 求职面试

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485424613-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485505401-image.png)

# Web3运行原理

助记词和私钥一对多，私钥可以自己生成。

交易就是要做的事+gas费+防重放序号nonce，而钱包则要组装交易内容并私钥签名，然后广播到区块链网络。

## 交易流程

-   当发起一笔交易后，这笔交易首先被发送给区块链其中一个节点A，在进入节点A前，先验证交易的格式、签名、nonce 等基本合法性，通过**验证**后，这笔交易会被**暂存**到节点A的内存池中，这个池子用来暂存已验证合法但尚未被打包的交易的池子。
    

-   然后节点A立刻广播到邻居节点，然后才在自己的池中按照 Gas 费用高低**排序**，矿工/验证者会优先选择价格更高的交易进行打包，打包成新区块添加到自己的区块链中（每个节点都有自己的区块链，一个个区块首尾相连形成的链）。
    
-   邻居节点收到交易后也会先进行合法性校验，通过后再转发给它们的邻居，不断重复上一步骤，如此病毒式传播，直到该区块链大部分节点都记录这笔交易，此时就可以认为这笔交易在链上达成了共识，被认可。
    

但并非所有节点都会接收或保留这笔交易，有些节点可能因 Gas 费过低而拒绝加入自己的内存池、可能因内存池已满而丢弃低优先级交易。

  
POW：工作量证明，一种验证交易的方式，验证者/矿工需要投入大量算力来解决复杂数学题，验证成功后才能获得奖励（挖矿）。这种方式能耗很高。

POS：权益证明，一种更环保的验证方式，验证者/矿工需要先质押一定数量的代币，才有资格去验证交易。验证成功后会获得奖励，相比 PoW 更节能。

智能合约：虚拟机中设定好的代码程序，不可篡改且自动执行
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

今天参与了以太坊中文周会和web3行业全局介绍&岗位概览分享

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-12-1768223118971-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-12-1768223106935-image.png)

## 发展史

-   最初由 HTTP 的发明者提出，是指一个集成的通信框架，互联网数据可以**跨越各个应用和系统**实现机器可读。Web 3.0 通常也被称作为“语义网”
    
-   2014 年，以太坊创始人重新定义了 Web3 概念，指一种区块链技术，可以\*\*基于“无须信任的交互系统”\*\*在“各方之间实现创新的交互模式”。
    

### Web1.0

-   时间：1994-2004
    
-   特点：Web 1.0 主要是静态的 HTML 网页，用户之间很少交互，有聊天转账功能但是无法保证安全。
    

实际在 1968 年，一个名为“ARPANET”的美国政府项目就已经启动了 Web 1.0。ARPANET 最初是由军方承包商和大学教授组成的一个用于交换数据小型网络。Web 1.0 时代中最具创新力的企业就是**必胜客**。他们在 1995 年开发了一个订购披萨的网页，消费者可以在页面中下单，等到披萨送到后再付现金。

### Web2.0

-   时间：2004 - 至今
    
-   特点：动态网页，用户之间互动性显著提升，用户使用互联网时必须**授权中心化的第三方平台管理大量数据**。因此这些中心化的实体在数据和内容权限方面被赋予了巨大的权力和影响力。
    

### Web3.0

-   时间：2008 - 至今
    
-   特点：**基于数学加密证明，而非信任第三方数据中心平台**，即去中心化。
    

2008 年中本聪提出了点对点的数字货币（比特币）和区块链概念。

智能合约出现后，去中心化互联网模式才逐渐流行，虚拟货币实现了点对点交易，智能合约不是传统的法律约束，而是可编程协议，通过编程实现约定的逻辑，好比商品贩卖机，规定输入硬币，选择商品后，规定输出商品。

## Web3 核心要素

-   区块链
    

安全性和去中心化水平都极高的网络，可以在一个**共享账本**中储存数据、交换价值并记录交易活动，而且账本**不受任何中心化**实体控制。区块链是 Web3 的基石

-   加密资产
    

基于区块链构建的数字货币或代币，类似现实的货币

-   智能合约、去中心化应用
    

智能合约是区块链上不可篡改的程序，利用“如果 x 是真实的，则执行 y”的代码逻辑自动执行交易。通过智能合约可以构建去中心化应用 dAPP，给用户使用。

-   预言机
    

智能合约要充分实现其潜力，就必须能够与区块链网络以外的数据和系统交互。预言机能**将区块链连接至真实世界中的数据和系统**，并提供关键的基础架构，打造一个具有互操作性且统一的 Web3 生态。

-   代币 Token
    

代币不是货币，和 BTC 这种加密货币不一样，\*\*代币构建在已有区块链之上的数字资产\*\*，比如用人民币买了游戏币、藏品，那么人民币就可理解为已有区块链之上的加密货币，游戏币、藏品就是代币，不能当钱花（除非有人愿意买）  
非同质化代币（Non-Fungible Token 简写 NFT ），像比特币这种原生的加密货币，每一单位都一样，可以互换（1 BTC = 1 BTC），是同质化，“非同质化”则是每个 NFT 都独一无二，不能互相替代，比如某个网站域名者签名球衣，可能全世界仅此一份。

-   Gas 费
    

因为区块链每笔交易都在链上进行加密等计算最终上块，肯定要消耗计算机资源，那么收点过路费也不为过吧

## 举例

假设某季节中的降雨量必须超过 20 英寸，才能保障农作物不欠收。一个农民希望买保险来对冲恶劣天气造成的不利影响。正常情况下他必须要经过漫长的理赔流程，并且要依靠**中心化的保险机构**来核实降雨量。但是如果在 Web 的世界，一切都是事先编程约定好的合约，保险流程会非常高效迅速，并基于简单的二元逻辑判断农民是否应获得理赔。物联网传感器在当季记录的降雨量是否低于 20 英寸？如果是，则立即支付赔偿金给农民。如果否，则不支付赔偿金给农民。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
