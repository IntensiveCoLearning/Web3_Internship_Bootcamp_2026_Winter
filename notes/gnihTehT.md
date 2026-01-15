---
timezone: UTC+8
---

# ShadowDogee

**GitHub ID:** gnihTehT

**Telegram:** @gnihTehT

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
# DAT4

这次分享会的内容比较复杂所以不是特别明白，下面这句话是由GPT帮我整理的。

这次分享的核心：AI 正在从“只会说话的工具”进化成“能干活、能收钱、能承担责任的经济主体”，而 Web3（区块链）是支撑这种“机器经济”的唯一可行基础设施。

今天刚刚考完试，没来得及参加co-learning很可惜，但是寒假已经开始了，作为学生能有大把时间在没有打断的情况下用来学习web3了！因为考了一整天，今天经历不是很多，就把昨天与前天的内容又过了一遍。

## 学习内容

今天在Remix里学习了部署合约到Sepolia测试网

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;
contract Counter{
    uint256 public count;

    function set(uint256 _n) public {
        count = _n;
    }

    function add() public {
        count += 1;
    }
}
```

这个合约的内容是在链上存一个数，任何人可以改，任何人可以+1，任何人可以读。

并按照`count`、 `add`、 `count` 的顺序实验了功能。

## 接下来的打算

接下来我打算给这个合约添加条件，变为只有我能修改、不允许超过十

## 新认知

我一直望文生义感觉合约就像是一个代码版的合同，但是事实上它和合同的最大的区别的，合约不在乎你是谁、你想干什么、合不合理，因此这也引出了钱包安全的重要性。它只是一个被拿来执行“合同式规则”的不可变程序，就算是黑客也能利用它得到东西。就像之前的分享会还要实习手册中有提到过，Smart Contract这个名字起的不是特别的好，它的本体只是一个“残酷”执行的程序。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->



DAY3

今天准备去学习剩下solidity入门课程，把入门通了一遍。

明天考试，今天大多时间都在复习，请假一次笔记
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->





# Day 2

今天开始学习Solidity🎉

发现Solidity和C#还是有很大的相似之处

目前是学习到了biginner章节，过了一遍变量Variables，函数Functions感觉接受还是很良好的。我决定快速过一遍后就开始尝试一下部署合约或者深度技术里的挑战。

今天听了布老师对Web3运行原理的讲解，对Web3运作有了一个更清晰的概念。不过也产生了一些小疑惑或许可以随时间解开（比如，**如何才能更好的利用Web3来保护版权？**），目前接触到的新信息和我在能系统学习接触之前对Web3的想象有相似也有很多的不同，有些甚至让我有些震惊。

在我看来目前Web3是结合了社区自治和代码治理的一个新互联网概念，虽然目前不能完全规避中心化，但是社区都做尽了力。

布老师在最后提出的问题也非常有思考价值，先贴在笔记里供自己慢慢思考。

![c01a22e4543fbc5b056bfd7d89a1e382.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-13-1768312866082-c01a22e4543fbc5b056bfd7d89a1e382.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->








# Day1 共学第一天

今天是加入实习计划第一天，作为一个只打过gamejam的大学生、真正意义上的纯小白，看到群内一起共学的伙伴都有相关经历感觉非常的厉害。为了认识更多的朋友，我建立了一个山东地区的共学小组（十分紧张），希望接下来的时间通过学习我也可以成为自己现在仰慕的业内人士并加入LX DAO ^^

# 学习笔记

## 网络钓鱼Phishing Attempt

### 1\. 假装客服

对方会假装是钱包客服来询问钱包的**助记词Seed phrase** , 事实上钱包客服是绝对不会去询问的。助记词是是恢复钱包的私钥，如果别人得到了它就立刻拥有了你全部资产的控制权。

**如何保护钱包**

-   不要给任何人助记词
    
-   将助记词存在线下一个安全的地方
    
-   使用硬件钱包
    
-   如果你收到信息问你要助记词那这基本上就是钓鱼
    
-   怀疑时立刻结束对话并联系官方渠道
    

### 2\. 钓鱼授权

一旦在主网或有价值资产的地址上签过，对方就可以随时把你钱包里的USDC直接转走，不需要你再确认。

**示例**

```
{
  "domain": {
    "name": "USD Coin",
    "version": "2",
    "chainId": 11155111,
    "verifyingContract": "0x74A4A85C611679B73F402B36c0F84A7D2CcdFDa3"
  },
  "types": {
    "Permit": [
      {
        "name": "owner",
        "type": "address"
      },
      {
        "name": "spender",
        "type": "address"
      },
      {
        "name": "value",
        "type": "uint256"
      },
      {
        "name": "nonce",
        "type": "uint256"
      },
      {
        "name": "deadline",
        "type": "uint256"
      }
    ]
  },
  "message": {
    "owner": "0x0A874a7687695Af1BA3c823fE73ED208867BE554",
    "spender": "0x1234567890123456780012345678901234567890", //骗子地址
    "value": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff", //unit256 最大值=无限额度授权
    "nonce": 378622,
    "deadline": 1770786450 //长期有效
  },
  "signature": "0x18bcb7f1f51a7d5d19e8e2316079cbfac59c509b48713ac19044860e6a42cb4d417e51afaa8cb3a4c329d9a5ff70a7b7d0a8d26157cb0d29fd96dc931e8eed231c",
  "r": "0x18bcb7f1f51a7d5d19e8e2316079cbfac59c509b48713ac19044860e6a42cb4d",
  "s": "0x417e51afaa8cb3a4c329d9a5ff70a7b7d0a8d26157cb0d29fd96dc931e8eed23",
  "v": 28
}
```

# **转测试币练习**

## 1\. 打开钱包点击发送

![QQ_1768202253475.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202256483-QQ_1768202253475.png)

## 2\. 选择SepoliaETH

![QQ_1768202286051.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202289155-QQ_1768202286051.png)

## 3\. 输入钱包地址并继续

![QQ_1768202306857.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202309830-QQ_1768202306857.png)

## 4\. 点`活动`可以查看状态

![QQ_1768202330049.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202338716-QQ_1768202330049.png)

## 5\. 点击这个状态然后`在区块浏览器上查看`

![QQ_1768202389649.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202394518-QQ_1768202389649.png)

## 6.复制**Transaction Hash**即可提交作业

![QQ_1768202463799.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202468191-QQ_1768202463799.png)

## 如何查看收款记录？

由于MetaMask`活动`UI里只会显示由你发起的活动，所以收款记录要在[https://sepolia.etherscan.io](https://sepolia.etherscan.io)上输入钱包地址查看。

## 名词解释：

**Etherscan**以太坊区块链浏览器

**Etherscan tx link**是某一笔链上交易的“官方浏览器页面链接”

每笔交易都有一个**Transaction Hash**交易哈希

把它丢进 **Etherscan**，就能看到：

-   谁 → 给谁
    
-   干了什么
    
-   调了哪个合约
    
-   授权了什么
    
-   成功还是失败
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
