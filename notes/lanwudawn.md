---
timezone: UTC+8
---

# c5

**GitHub ID:** lanwudawn

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
DAY10

完成 solidity 101 挑战
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->

DAY9  

今天补一下落下的进度
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->


DAY8

TODOLIST: 1. 学习 Solidity by Example-basic 部分 ×  
  
  
回顾分享会，运营工作不轻松，很有挑战性。  
  
  
等我憋个大的笔记。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




DAY 7

今天跟着实习计划的小教程用remix写了一个简单的智能合约

````
```solit// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleStorage {
    // 这个变量用来存一个数字
    uint256 public myNumber;

    // 写入函数：把数字存进去 (要花 Gas)
    function store(uint256 _num) public {
        myNumber = _num;
    }
    // 自增函数
    function plus_1 public{
        myNumber++;
    }
    // 读取函数：把数字读出来 (免费)
    function retrieve() public view returns (uint256) {
        return myNumber;
    }
}
````

Deployed Contracts这里面再每次更新后会有新的拓展。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->





DAY6

TODOLIST: 1 手册 智能合约 部分 2 熟悉安装开发环境

看了智能合约开发的内容，定义很多，一时间理解不了，还是要在实践中慢慢理解。  
\------  
**常用 JSON-RPC 方法**：

| 方法名 | 功能 | 示例 |
| --- | --- | --- |
| eth_getBalance | 查询账户余额 | eth_getBalance(address, block) |
| eth_blockNumber | 获取最新区块号 | eth_blockNumber() |
| eth_sendTransaction | 发送交易 | eth_sendTransaction(txObject) |
| eth_call | 调用合约（只读） | eth_call(callObject, block) |
| eth_getTransactionReceipt | 获取交易收据 | eth_getTransactionReceipt(txHash) |
| eth_getLogs | 查询事件日志 | eth_getLogs(filterObject) |
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->








### **DAY 5**

**今天应该是DAY7的，前两天事情有点多，落下了功课。**  
**下周要正式开始学习技术了，要做好计划，最近没有调整好生活节奏，懈怠。看到那么多优秀的同学，要努力啊shell！**
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->










**_DAY4_**  
**_TODOLIST: 1 阅读教程并整理_**  
  
**_这两天我要答辩太忙了。只参加了今晚AI的分享会，有一个观点我印象比较深刻：Web3、区块链的不可篡改性赋予AI的真正身份。_**
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->












### **DAY3**

**TODOLIST : 1. 今天有两个分享会❌ 2.完成NFT头像创建✔**

**这两天我要忙毕业设计开题，好累。好多笔记没记下，X上还没分享。这几天也在参与Akasha Insight社区的建设，创了一个github资源库，嗯还好。以后我的残酷共学内容形式应该是：1todolist+笔记 2碎碎念**  

![mfnft.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/lanwudawn/images/2026-01-14-1768401467495-mfnft.png)

myfirstNFT  
  
有点焦虑，因为毕设，分享会今天也缺席了，据说内容很好，之后看回放补吧！
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->














DAY2  
昨天晚上赶在ddl之前仓促提交笔记。下定决心一定要早些笔记。  
\------  
对于昨天的遇到的问题，通过请教**森理**同学、**Stariv**助教、**Hickerzed**助教、**XiaoHai67890**助教，得到了解答：我的情况实属正常情况，况且实际上不影响接收和发送。（我发现有时候特较劲，其实效率很低下。  
\-------
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->















**_DAY1_**  
我目前遇到一个问题：我的MetaMask在水龙头网站自动断开测试网的连接，而我又不知道怎么在水龙头网站连接测试网，我怀疑是这个原因让我领不了测试币。  
\------  
今天学习了一下入门导读，我粗浅的看法跟入营时的想法差不多：web3的用户能真正拥有自己的数据和资产，不受平台的控制，是互联网很好的发展趋势；区块链是一种去中心化的技术，我目前的理解三个词概括是公共，透明，牵一发而动全身。  
\------
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
