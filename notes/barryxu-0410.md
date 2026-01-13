---
timezone: UTC+8
---

# barryxu-0410

**GitHub ID:** barryxu-0410

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
今日继续阅读web3实习手册，参加了线上co-learning和Bruce老师的Web3运营原理分享

俩会议中，co-learning探讨了DAO组织的治理，钱的影响是否违背了去中心化的初衷；晚上的分享会议上，Bruce老师介绍了web3的技术原理，我了解许多新名词如RPC，EIP，ERC，EVM。

另外我还自学了solidity语法，今日学习了error，interface等部分

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;
interface ICounter {
    function count() external view returns (uint256);
    function increment() external;
}

contract Counter is ICounter{
    uint256 public count;
    function increment() external override{
        count +=1;
    }
}

contract MyRobot{
    function call(address _counterAddress) external {
        ICounter counterScript = ICounter(_counterAddress);
        counterScript.increment();
    }

}
```
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




今天是web3实习的第一天，今日学习内容：阅读web3实习手册,阅读《021学习以太坊》，参加emily老师的行业分享会议以及以太坊中文周会

之前有过对web3行业碎片化的了解，今天通过上述资源进一步系统性全面认识了web3，内容包括：以太坊，比特币，行业赛道概览，以及就业现状；

会议纪要如下图：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/barryxu-0410/images/2026-01-12-1768228159593-image.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
