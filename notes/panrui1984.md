---
timezone: UTC+8
---

# Rui Pan

**GitHub ID:** panrui1984

**Telegram:** @RayPanChina

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
以下纪录今天学习以太坊开发的部分内容

### **交易**

-   一条交易包含以下内容：
    
    -   from：交易发送者地址
        
    -   to：交易接收者地址，如果为空则表示是在创建智能合约
        
    -   value：交易金额，即发送方要给接收方转移的以太币数量（wei 为单位）
        
    -   data：交易数据，如果是创建智能合约则是智能合约代码，如果是调用智能合约则是调用的函数名和参数
        
    -   gasPrice：交易的 gas 价格，即每单位 gas 的价格（wei 为单位）
        
    -   gasLimit：交易的 gas 上限，即交易允许执行的最大 gas 数量
        
    -   nonce：交易的序号，即发送者已经发送的交易数量
        
-   除此之外发送的交易数据包还需要包含：
    
    -   hash：交易的哈希值，由前面的内容和 chainId 计算得到
        
    -   v、r、s：交易签名的三个部分，由发送者私钥对交易哈希值进行签名得到
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

今天晚上的法律合规专题课意义十分重大，可以说非常有帮助。

正巧，今天在人民法院报公众号一篇文章，

[https://yuanbao.tencent.com/chat/naQivTmsDa?from=WeixinAskAi&tridChannel=wxllqndtabgzhwzzj.lqybh5xzrk.pc&askAi=%7B%22coverUrl%22%3A%22https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz\_jpg%2Fqrrm9dFnnpS9d0qQTibdAaibcMFa1FM97JJglO3XGib5VZPXwIKfhFuofUaPvhsnbj62fKkiaQVRyyHJGFQLvian8EA%2F0%3Fwx\_fmt%3Djpeg%22%2C%22link%22%3A%22https%3A%2F%2Fmp.weixin.qq.com%2Fs%2F6NUlNDibHR\_KmEmMi311hQ%22%2C%22title%22%3A%22%E6%B6%89%E8%99%9A%E6%8B%9F%E8%B4%A7%E5%B8%81%E7%8A%AF%E7%BD%AA%E6%A1%88%E4%BB%B6%E7%9A%84%E9%80%82%E6%B3%95%E7%BB%9F%E4%B8%80%E6%8E%A2%E8%AE%A8%E2%80%94%E2%80%94%E7%AC%AC%E5%9B%9B%E6%9C%9F%E2%80%9C%E8%87%B3%E6%AD%A3%C2%B7%E7%90%86%E8%AE%BA%E5%AE%9E%E5%8A%A1%E5%90%8C%E8%A1%8C%E2%80%9D%E5%88%91%E4%BA%8B%E5%AE%A1%E5%88%A4%E7%A0%94%E8%AE%A8%E4%BC%9A%E7%BB%BC%E8%BF%B0%22%7D&retry=1&nonce=maeDZrC6xsbwctJf&state=yf\_wechat\_login&code=041EudGa1TTXZK0UVEHa1ZcvMp4EudGA](https://yuanbao.tencent.com/chat/naQivTmsDa?from=WeixinAskAi&tridChannel=wxllqndtabgzhwzzj.lqybh5xzrk.pc&askAi=%7B%22coverUrl%22%3A%22https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2Fqrrm9dFnnpS9d0qQTibdAaibcMFa1FM97JJglO3XGib5VZPXwIKfhFuofUaPvhsnbj62fKkiaQVRyyHJGFQLvian8EA%2F0%3Fwx_fmt%3Djpeg%22%2C%22link%22%3A%22https%3A%2F%2Fmp.weixin.qq.com%2Fs%2F6NUlNDibHR_KmEmMi311hQ%22%2C%22title%22%3A%22%E6%B6%89%E8%99%9A%E6%8B%9F%E8%B4%A7%E5%B8%81%E7%8A%AF%E7%BD%AA%E6%A1%88%E4%BB%B6%E7%9A%84%E9%80%82%E6%B3%95%E7%BB%9F%E4%B8%80%E6%8E%A2%E8%AE%A8%E2%80%94%E2%80%94%E7%AC%AC%E5%9B%9B%E6%9C%9F%E2%80%9C%E8%87%B3%E6%AD%A3%C2%B7%E7%90%86%E8%AE%BA%E5%AE%9E%E5%8A%A1%E5%90%8C%E8%A1%8C%E2%80%9D%E5%88%91%E4%BA%8B%E5%AE%A1%E5%88%A4%E7%A0%94%E8%AE%A8%E4%BC%9A%E7%BB%BC%E8%BF%B0%22%7D&retry=1&nonce=maeDZrC6xsbwctJf&state=yf_wechat_login&code=041EudGa1TTXZK0UVEHa1ZcvMp4EudGA)

# **涉虚拟货币犯罪案件的适法统一探讨——第四期“至正·理论实务同行”刑事审判研讨会综述**

以下是我对其重要观点做了一些总结

1.  **关于“主观明知”的认定（洗钱罪）**
    
    -   **核心**：必须证明行为人“知道或应当知道”钱来自特定上游犯罪
        
2.  **关于“非法经营罪”的认定**
    
    -   **核心**：经常性提供人民币与外币之间的兑换服务并牟利，属于“变相买卖外汇”
        
    -   **区分**：个人偶尔炒币不构罪，但以此为业则可能构成
        

**简单来说**：

-   **怎么算“明知”洗钱？**​ 不能乱推定，需要实锤证据。
    
-   **什么时候算“洗钱成功”？**​ 赃款换成U币的那一刻就算。
    
-   **什么换汇行为算犯罪？**​ 把虚拟货币当生意做，专门帮人跨境兑换就违法。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


“Web3 运行原理”课程笔记

### 交易（TX）

-   交易 = 操作 + Gas + nonce
    
-   类型：转账、合约、govence
    
-   nonce 防重放：确保执行一次，防止重放攻击
    

### 钱包

-   功能：提供UI填交易 → 使用私钥签名 → 通过 RPC 广播到网络
    
-   签完即广播，Etherscan 可查状态
    

### 数字签名原理

-   私钥签名 → 全网可验，无法伪造
    
-   私钥，助记词
    

### Gas

-   作用：防垃圾交易、激励验证者、维持经济模型
    
-   网络拥堵时 gasPrice 上涨
    

### 交易从发起到上链

1.  签名
    
2.  经 RPC 发给节点
    
3.  进内存池排队
    
4.  Builder 从交易池中挑选高 Gas的 交易进行打包
    
5.  Validator节点进行 出块 + 共识确认
    
6.  写入区块链（约每 12 秒一区块，含数百笔交易）
    

### 区块确认

-   新块初始为 Unfinalized，需 12 分钟才不可逆
    

### 智能合约

-   链上可执行的代码
    
-   执行：结果永久上链
    

### 以太坊升级？

-   EIP
    
-   硬分叉
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




今天完成的任务如下：

-   钱包任务（测试网airdrop，转账以及铸造NFT）
    
-   完成[Unphishable](https://unphishable.io/)初级和中级的题目
    
-   下午答疑会议及晚上的职业指南
    

我的一些感想：

接触web3已经已有大约半年的时间了，对web3已经有了一定的认知。但是第一天的跟班学习是仍然受益匪浅，系统性的知识明显比零散的学习效率更高。比如防钓鱼的题目过一遍还是对自己有一定的警醒。尤其是《021学习以太坊》更是让我对以太坊有更深入的认知。

参加学习的第一天，我总结了训练营给初学者的学习指南（我会分享给我的老婆一起学习）：

**第一阶段：建立基础认知**

了解web3基础的概念，通过一些简单的实践对web3有初步了解，具体包含：

**一、先获得 Web3 基础能力**

钱包，助记词，网络，转账，授权，区块浏览器

**二、建立 Web3 信息源**

关注 高质量账号，Builder，Researcher等；

三、**选一个方向**

社区运营（Discord / TG社区），技术等
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
