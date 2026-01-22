---
timezone: UTC+8
---

# enderzcx

**GitHub ID:** enderzcx

**Telegram:** @enderzcx

## Self-introduction

小白一枚，求带

## Notes

<!-- Content_START -->
# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->
# **Uniswap 工作原理解析**

[https://github.com/Web3-Club/Solidity-by-example\_Chinese/blob/main/04%20%E5%8F%98%E9%87%8F.md](https://github.com/Web3-Club/Solidity-by-example_Chinese/blob/main/04%20%E5%8F%98%E9%87%8F.md)这一章直接COPY代码会Warning（REMIX Windows桌面版）  
以下是我修改后不会报错的版本：

# **变量**

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Variables {
    string public text = "Hello";
    uint public num = 123;

    function doSomething() public view returns (uint,uint, address) {
        uint i = 456;
        uint timestamp = block.timestamp;
        address sender = msg.sender;
        return (i,timestamp, sender);
    }
}
```

## **扩充**

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Variables {
    //Local存在函数内存中 调用的时候才有
    //state 在函数之外声明 存储在区块链上
        //blockchain 存在链上 需要消耗GAS 
    //global是默认的全局变量,提供有关区块链的信息
    string public text = "Hello";  //存在区块链上
    uint public num = 123;

    function doSomething() public view returns (uint,uint,address){

        uint i = 456;   //内存变量 在调用这个函数的时候才会使用此变量
 

        uint times = block.timestamp;  //当前区块的时间戳,全局变量
        address sender = msg.sender;   
        return (i,times,sender);
    }
}
```
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->


收看「Web3 公共物品资金分配第一节课」回放

我也想参与！我也想当坏人！
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->



**查看「Key Hash Based Tokens: 从 ERC-721 到 ERC-7962」回放**  
**后面学习完智能合约我要试试ERC7962在代码层面和一般的有什么不一样，从代码层面是如何实现的**  
Solidity学习路线（**021学习以太坊**）：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-20-1768886692829-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-20-1768886705747-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-20-1768897672853-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/enderzcx/images/2026-01-20-1768897692149-image.png)
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->






**体验 My first zk vote**  
**成功把一个智能合约部署在测试网上并且发送了一段信息 Remix有点难用 不知道是我的网络问题还是什么**  
[**3.2 审计标准流程**](https://web3intern.xyz/zh/smart-contract-development/#_3-2-%E5%AE%A1%E8%AE%A1%E6%A0%87%E5%87%86%E6%B5%81%E7%A8%8B)

Solidity 审计流程

1.  **静态分析（Static Analysis）**：使用工具如 Slither、Mythril 扫描代码缺陷
    
2.  **动态测试（Fuzzing/Property Testing）**：模拟攻击行为和极限条件
    
3.  **人工审查（Manual Review）**：由资深审计员检查业务逻辑漏洞
    
4.  **审计报告生成（Audit Report）**：明确发现的问题及修复建议
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->









Web3 实习手册[「智能合约开发」](https://web3intern.xyz/zh/smart-contract-development/)部分  
学习中文排版规范：[https://github.com/sparanoid/chinese-copywriting-guidelines](https://github.com/sparanoid/chinese-copywriting-guidelines)  
学习AI 及其基础概念
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->











已完成 [Unphishable](https://unphishable.io/) 钓鱼攻防挑战  
Trezor的域名到底是什么 试了很多都不对。。。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->












《021学习以太坊》: 比特币更像是“价值锚”， 以太坊更像是“应用发动机” 已学习到32页
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->













学习了Web3 实习手册[「安全与合规」](https://web3intern.xyz/zh/security/)部分  
开发环境并熟悉：  
[Remix IDE](https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.30+commit.73712a01.js)

[Remix 使用教程](https://www.notion.so/2bc1a400ef948075ab7ec8b8259c7746?pvs=21)  
同时开始在B站上学习智能合约编程[https://www.bilibili.com/video/BV1Ca411n7ta/?spm\_id\_from=333.1007.top\_right\_bar\_window\_default\_collection.content.click&vd\_source=6be132124d2a630c6fcea0ba05bfbbf9](https://www.bilibili.com/video/BV1Ca411n7ta/?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click&vd_source=6be132124d2a630c6fcea0ba05bfbbf9)
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->














这两天系统性学习了**区块链基础概念、以太坊概览、行业赛道全览、Web3 工作方式**，果然比我之前东学一点西看一些有效率有条理多了。  
**收纳了很多富有见解的笔记：**

| Web3 行业全局介绍 & 岗位概览How to DeFi: 入门篇 | Web3入门计划总结系列之 一 |
| --- | --- |
| DeFi 进阶指南 | Web3入门计划总结系列之 二 |
| Web3 运行原理 |   |
| Vibe Coding 方法论：不会编程的人如何用 AI 写出能跑的代码 |   |
| 写给00后：为什么我们不能错过 Web3 |   |

**一些工具和网站:**

| 官方文档 | 研究报告 | 中文资源： |
| --- | --- | --- |
| Ethereum.org - 以太坊官方文档 | Messari - 区块链项目深度分析 | ETHPanda - 以太坊中文社区 |
| Uniswap Docs - Uniswap 协议文档 | The Block - 行业数据和趋势 | LXDAO |
| OpenSea Learn - NFT 入门指南 | Delphi Digital - 加密市场研究 |   |

其他工具和网站

|   | 找工作 |
| --- | --- |
| 用AI设计网站：Stitch - Design with AI | DeJob |
|   | The Ethereum Season of Internships - 英文，以太坊实习季 |
<!-- DAILY_CHECKIN_2026-01-14_END -->
<!-- Content_END -->
