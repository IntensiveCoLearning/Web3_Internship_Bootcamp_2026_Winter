---
timezone: UTC+8
---

# Jimmy Wang

**GitHub ID:** Jorrrmungandr

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
今天主要学习了NFT相关的知识

NFT 不是一张图片，而是一个\*\*智能合约（Smart Contract）\*\*，记录了

_​_\*Token ID\*\*​ （唯一的编号）

_​_\*Owner Address\*\*​（所有者地址）

_​_\*Metadata URI\*\*​（元数据链接，通常是一个指向 IPFS 或服务器的 URL）

一个NFT合约需要满足代币标准ERC-721，其中最核心的是两个状态变量：

1\. `_owners`​: `mapping(uint256 => address)`

\* 输入 Token ID，返回持有者的钱包地址。

2\. `_balances`\*: `mapping(address => uint256)`

\* 输入钱包地址，返回该地址持有的 NFT 数量。

一个NFT合约还需要实现代币标准ERC-721规定的所有方法，比如：

\* balanceOf(owner): 查询某人有多少个 NFT。

\* ownerOf(tokenId): 查询某个 ID 的主人是谁。

\* safeTransferFrom(from, to, tokenId): 安全转账。

\* approve(to, tokenId): 授权别人（如 OpenSea）操作你的这一个 NFT。

\* setApprovalForAll(operator, approved): 授权别人操作你所有的 NFT（钓鱼攻防挑战中提到）。

\* tokenURI(tokenId): 获取某个 ID 对应 NFT 的元数据链接。

后续还有新的ERC-1155对 NFT 进行了拓展，单个合约可以管理多种类型的代币（\*\*包括同质化和非同质化\*\*）。支持批量转账，极大节省 Gas 费。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

计划：

1、继续学习ethereum基本概念  
2、用wagmi实现转账等基本操作
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


计划：  
1、体验一下wagmi的基本功能

2、做几关钓鱼攻防（连接钱包、授权、交易等操作）

打卡：

1、体验了连接钱包的功能，学习了useConnect hook  
2、了解了DApp和钱包的一些交互，包括签名、授权、转账等，了解不同授权协议之间的区别，比如Uniswap的Permit3协议
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




## 学习内容

1.  在水龙头领取测试币并转账给其他同学
    
2.  完成了NFT铸造任务
    
3.  参加了ETH中文周会、Co-Learning和行业介绍会
    

## 学习感悟

今天主要学习了钱包的基本使用，包括浏览器钱包插件、网络的切换以及发送/接收代币等功能

在My First NFT网站中实践NFT铸造时，由于我是web2开发人员，比较关注网络请求的链路，我发现web3应用中，链上的请求无法在浏览器的console中查看到。问了wachi老师之后才知道，链上的网络交互都是由钱包插件完成，而浏览器页面只需要给插件传递一个json即可，然后钱包会发送rpc请求给链上节点
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
