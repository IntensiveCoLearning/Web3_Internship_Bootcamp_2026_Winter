---
timezone: UTC+8
---

# futureclover87

**GitHub ID:** futureclover87

**Telegram:** 

## Self-introduction

前Web2 从业者（500强+互联网）产品+市场+销售+供应链都呆过 据说很适合创业 
2021 年开始了解Web3 中断许久 终于回来了
INTJ-but 兴趣广泛 近期皮艇/桨板/网球/书法
2021 年开始了解Web3 中断许久 终于回来了
INTJ-but 兴趣广泛 近期皮艇/桨板/网球/书法 
人生走了太多应该路线，想走一些“非应该”路线
2021 年入Web3"坑“ 中断许久 Now I'm back
INTJ-but 兴趣广泛 近期皮艇/桨板/网球/书法 喜欢在秋高气爽的天气去徒步
人生走了太多应该路线，想走一些“非应该”路线
2021 年入Web3"坑“ 但是一直没找到志同道合的朋友，别看了，说的就是你~
INTJ-but 兴趣广泛 近期皮艇/桨板/网球/书法 喜欢在秋高气爽的天气去徒步
人生走了太多应该路线，想走一些“非应该”路线
2021 年入Web3"坑“ 但是一直没找到志同道合的朋友，别看了，说的就是你~
INTJ-but 兴趣广泛 皮艇/桨板/网球/书法 喜欢在秋高气爽的天气去徒步
人生走了太多应该路线，想走一些“非应该”路线
2021 年入Web3"坑“ 但是一直没找到志同道合的朋友，别看了，说的就是你~
INTJ-but 兴趣广泛 皮艇/桨板/网球/书法 喜欢在秋高气爽的天气去徒步
人生走了太多应该路线，想走一些“非应该”路线

## Notes

<!-- Content_START -->
# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
核心深挖了区块链钱包原理

a, 核心关联：助记词推导出私钥，私钥推导出公钥（地址），过程不可逆

b, 钱包和账号的关系：助记词+ECDSA Pair可以推出私钥+公钥，助记词相当于大钱包，下面可以有多个小钱包，小钱包之间彼此独立不相关联

c, 公钥的作用：指令+签名 可以进行全网发布，任何人可以用ECDSA倒推出地址，如果信息被篡改，则地址也会发生改变，和原有地址必须互相验证，从而验证签名

d, 是否可被攻破？128位的随机二进制序列是真正安全性核心，这是助记词的源头，但是用户通常不会看到

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-14-1768391042314-image.png)

e, 是否可能被撞中扫走地址上的资产？并不是随机组合的2048个单词里面随机选择12个就可以生成助记词，必须要符合效验和验证

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-14-1768390925169-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-14-1768390950460-image.png)

e, 助记词原理：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-14-1768390963027-image.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->

今天核心学习收获

1.  了解Web 3.0 和 Web 3 的区别，之前经常会混用，而Web 3 才是真正区块链的指向，待学习深入了解RDF，OWL，SPARQL的语言
    
2.  Web2 到Web3 的渐进式过渡 在考量和现实（用户习惯）与区块链技术之间建起的一座桥梁，Web2.5，举例了Reddit的积分制度，待研究下区块链的应用很多也有类似的平衡，从一些知名的应用去统计，如Roblox
    
3.  公链，联盟链，私链的区别，以及各自的优缺点，私联其实也是一条中心化的链条，小团队内的中心化决策，适合特定专业领域还是需要有一定中心化介入的功能
    
4.  ENS域名 新了解到的知识点，Web 3也还是有很多user-friendly的技术应用待发掘
    
5.  做了第一个NFT~
    

|   | Web 2 | Web 3.0 | Web 3 |
| 控制权 | 平台垄断 | 部分开放 | 用户自治 |
| 数据存储 | 中心服务器 | 混合存储 | 区块链 / IPFS |
| 支付系统 | 信用卡 / 支付宝 | 集成支付 | 加密货币 |
| 典型技术 | JavaScript | RDF / OWL | 智能合约 |
| 代表企业 | 腾讯 / 阿里 | W3C / DBpedia | Uniswap / ConsenSys |

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-13-1768292641672-image.png)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-13-1768292904122-image.png)

**与传统域名的关键区别**

-   所有权模型：ENS域名是 你钱包里持有的NFT资产，你拥有绝对控制权。传统域名是中心化机构管理的租赁服务。
    
-   结算与支持：ENS运行在 以太坊区块链 上，其注册和管理通过智能合约完成，费用用ETH支付。传统域名依赖于ICANN及其认证的注册商。
    
-   集成生态：ENS原生地与整个以太坊和Web3生态集成（钱包、DApp、智能合约），而传统域名主要用于Web2的网站和邮箱。
    

 **如何获得一个ENS域名？**

1.  访问 [app.ens.domains](http://app.ens.domains) 官网。
    
2.  连接你的以太坊钱包（如MetaMask）。
    
3.  搜索你想注册的名称（如 yourname.eth）。
    
4.  根据名称的“热度”支付年费（普通名称大约每年5美元等值的ETH）。
    
5.  完成注册并设置解析记录（将域名指向你的地址或其他资源）。
<!-- DAILY_CHECKIN_2026-01-13_END -->
<!-- Content_END -->
