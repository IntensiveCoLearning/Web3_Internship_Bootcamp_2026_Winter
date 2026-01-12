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
# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->
# Day1 共学第一天

今天是加入实习计划第一天，作为一个只打过gamejam的大学生、真正意义上的纯小白，看到群内一起共学的伙伴都有相关经历感觉非常的厉害。为了认识更多的朋友，我建立了一个山东地区的共学小组（十分紧张），希望接下来的时间通过学习我也可以成为自己现在仰慕的业内人士并加入LX DAO ^^

# 学习笔记

## 网络钓鱼Phishing Attempt

### 1\. 假装客服

对方会假装是钱包客服来询问钱包的\*\*助记词Seed phrase\*\* , 事实上钱包客服是绝对不会去询问的。助记词是是恢复钱包的私钥，如果别人得到了它就立刻拥有了你全部资产的控制权。

**如何保护钱包**

\- 不要给任何人助记词

\- 将助记词存在线下一个安全的地方

\- 使用硬件钱包

\- 如果你收到信息问你要助记词那这基本上就是钓鱼

\- 怀疑时立刻结束对话并联系官方渠道

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
