---
timezone: UTC+8
---

# vincentxthen

**GitHub ID:** vincentxthen

**Telegram:** @VincentXthen

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->
DAY7

今天主要做任务

-   X上发布心得体会，帮忙宣传，也帮自己活跃一下账户
    
-   水龙头测试币任务了解到了测试网
    
-   mintNFT任务发现了浏览器的钱包冲突BUG，wachi老师基于这个案例分享了如下知识：
    

### **一、技术问题根源：钱包注入冲突**

-   **历史背景**：最早只有 MetaMask 钱包，它将自身注入浏览器的全局变量 `window.ethereum`。后续其他钱包（如 OKX、360 等）也沿用同一名称注入，导致 **DApp 调用时出现竞争条件（race condition）**，可能调用到非预期的钱包（例如“360 版 ethereum”）。
    
-   **解决方案**：**EIP-6963（Multi Injected Provider Discovery）** 被提出，要求钱包在注入时提供标准化的元数据（如名称、图标、ID），让 DApp 能识别并选择具体的钱包提供商，从而解决冲突。
    

* * *

### **二、连接工具与账户抽象演进**

-   **中间件工具**：为解决多钱包连接问题，出现了 **Privy、RainbowKit、ConnectKit（Particle Network）** 等工具，它们封装了钱包发现与连接逻辑，并增加了 **社交登录（Social Login）** 等账户抽象功能，简化 DApp 集成。
    
-   **账户抽象（AA）**：通过智能合约钱包实现更灵活的认证方式（如社交恢复、无 Gas 交易），提升用户体验。
    

* * *

### **三、交易所钱包的商业逻辑演变**

-   **币安钱包的崛起**：
    
    -   **背景**：早期中心化交易所（CEX）不愿做钱包，因担心用户流失导致手续费收入下降。用户迁移成本低（私钥可导出）。
        
    -   **关键转折**：币安推出 **无私钥（MPC）钱包**，用户无法导出私钥，从而将用户绑定在币安生态内，实现“既保留流量，又渗透链上”。
        
    -   **增长策略**：通过 **Alpha 季** 等活动（如早期项目上线低门槛打新），吸引大量用户注册币安账号并转化使用其钱包，形成裂变效应。
        

* * *

### **四、行业现状与挑战**

-   **Alpha 模式依赖**：交易所通过出让项目方（VC）利益吸引用户，但项目上线后仍需自身基本面支撑，否则难以持续。
    
-   **技术为商业服务**：MPC 等技术早已存在，但直到币安将其与商业场景结合（绑定用户、提升平台粘性），才真正爆发价值。
    
-   **未来方向**：钱包从单纯的工具，逐渐演变为 **流量入口、用户身份载体与交易平台结合体**，成为 CEX 拓展链上生态的核心抓手。
    

* * *

### **总结要点**

1.  **技术驱动**：从 `window.ethereum` 单点注入到 EIP-6963 标准化，解决了多钱包冲突问题。
    
2.  **产品迭代**：连接工具（如 RainbowKit）和账户抽象提升了开发体验与用户体验。
    
3.  **商业创新**：币安通过无私钥钱包 + Alpha 活动，将钱包转化为用户留存和链上扩张的工具。
    
4.  **行业本质**：钱包不仅是技术产品，更是生态战略的一环，核心在于平衡用户体验、安全性与平台利益。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->

DAY6

//成都雾霾连续爆表一个月 今天鼻炎终于大爆发，疯狂喷嚏鼻涕，头昏脑胀

成都的排放问题一直不解决的话，冬天真的呆不得了

今天脑图几乎没更新，明天补上吧

![1768656702449.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-17-1768656702543-1768656702449.jpg)![1768656682981.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-17-1768656683096-1768656682981.jpg)![1000023608.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-17-1768656207624-1000023608.jpg)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->



DAY5  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-16-1768527930951-image.png)![ef61c24d894cda2d950ed415f9db039e.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-16-1768527943018-ef61c24d894cda2d950ed415f9db039e.jpg)
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->




DAY4  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-15-1768485305863-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-15-1768485449721-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-15-1768485466886-image.png)
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->





DAY3

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-14-1768394102130-image.png)![0bd6693eb6550f7edbf4e0e06f669ca6.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-14-1768394044000-0bd6693eb6550f7edbf4e0e06f669ca6.jpg)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->






DAY2  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-13-1768296425226-image.png)![5256c0f3e7c881eda8158c6049c39b84.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-13-1768296346499-5256c0f3e7c881eda8158c6049c39b84.jpg)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->







共学Day1  
1.了解了共学的模式  
2学习了区块链基本的概览章节

![Mindmap (Community).jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-12-1768213148155-Mindmap__Community_.jpg)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-12-1768213164269-image.png)![376b48efa5da9737b0262a9e558803f9.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/vincentxthen/images/2026-01-12-1768212961139-376b48efa5da9737b0262a9e558803f9.jpg)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
