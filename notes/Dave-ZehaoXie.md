---
timezone: UTC+8
---

# Dave

**GitHub ID:** Dave-ZehaoXie

**Telegram:** @Davezehao

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->
今天听了colearning  
会议讨论了 Figma 使用问题、活动策划审核流程以及 DFT 架构等相关内容，具体如下：

-   **Figma 使用问题**：
    
    -   **复制文字修改问题**：复制内容粘贴到自己项目有全部权限可更改，更改文本框内容需先选中 “test” 再双击编辑；若提示缺少字体，可从谷歌下载并上传到字体库。
        
    -   **筹备申请查看问题**：露娜老师同步审核，审核通过会沟通，着急可问陆楠老师审核结果。
        
-   **活动策划审核流程**：
    
    -   **嘉宾邀请环节**：通常先确定策划案和主题提交选题，通过后一起看能否邀请符合主题嘉宾；若个人渠道提前谈好嘉宾且未用官方名号，会为策划增分；通过审核后可自行或协助邀请嘉宾，后续有助教沟通执行阶段。
        
    -   **活动执行条件**：执行关键在于是否用官方名号接触外部事务，未用官方名号可先执行部分准备工作。
        
-   **DFT 架构介绍**：
    
    -   **前端与后端**：前端运行在用户浏览器，加载 JavaScript、CSS 等文件，定义按钮交互和样式；后端用 next js 或 Hono 等框架编写，部署到 Heroku 或 VPS 等环境，通过 jsonrpc 与 RPC 服务商通信。
        
    -   **数据库类型**：包括关系型数据库和 Redis 数据库，关系型数据库以表结构存储数据，通过 SQL 语句读写；Redis 数据库用于缓存，读取速度快但存储格式不同。
        
    -   **数据读取优化**：为提高用户体验，可提前将链上数据缓存到数据库，此流程叫 index，如 subgraph 就做此类事情。
        
    -   **前端与区块链交互**：前端通过库创建 client 与区块链交互，转账等操作调用钱包插件完成。
        
-   **其他问题解答**：
    
    -   **Web3 产品经理交流**：需了解拍卖、NFT 等行业知识，熟悉业态，考虑玩家痛点，设计交互流畅、用户体验好的产品，还需懂链上分析，筛选有价值项目。
        
    -   **今晚活动预告**：7 点小海老师做 DM 的 workshop，8 点半是梁青道合作伙伴南塘道的主题活动。
        
-   **任务**
    
    -   **产品体验提升**：体验更多 WEB3 产品，洞察市场和用户需求，达到业界优秀产品经理能让 MMB 市值翻 3 倍、流动性激增等产出标准
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->

![屏幕截图 2026-01-21 174155.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-21-1769010531648-_____2026-01-21_174155.png)![屏幕截图 2026-01-21 174532.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-21-1769010546716-_____2026-01-21_174532.png)![屏幕截图 2026-01-21 174746.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-21-1769010561970-_____2026-01-21_174746.png)![屏幕截图 2026-01-21 180041.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-21-1769010573742-_____2026-01-21_180041.png)![屏幕截图 2026-01-21 230307.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-21-1769010587049-_____2026-01-21_230307.png)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->


今天对象和我闹分手了
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->



笔记标题为笔记：以太坊中文周会（2026/01/19）

**1\. 每周简报 (News Briefing)**

-   **以太坊更新**：拒绝了7个EIP，将 **EIP-7954** 纳入考量（合约大小上限从24KB提升至32KB）。
    
-   **Vitalik 展望**：2026年将重聚焦于“自主权”和“去信任”，计划通过 zkEVM、Helios 等技术简化协议并进行“垃圾回收”（清除复杂代码）。
    
-   **Starknet 升级**：区块生产速度提升40%，最终确认时间降至1秒。
    
-   **市场与安全**：2025年加密盗窃案创历史新高（约40.4亿美元）；比特币和以太坊 ETF 全年吸引310亿美元流入。
    

**2\. 技术分享：新 EIP 与稳定币支付**

-   **新提案 (Pectra后)**：
    
    -   **EIP-778x**：修复 Gas Refund 漏洞。
        
    -   **EIP-7008**：为 ETH 转账增加 Log，便于追踪。
        
    -   **Slot Number Opcode**：解耦区块号与时间。
        
    -   **EIP-824**：增加 SWAPN/DUPN 指令，突破堆栈深度限制。
        
-   **稳定币支付**：Paradigm/Stripe 报告指出稳定币在跨国支付中可节省高达 80% 成本并实现实时结算。
    

**3\. 硬核技术：Rollup 与 Monad 开源**

-   **Ink 混合 Rollup**：结合 Based Rollup（抗审查）与 Sequencer Rollup（低延迟）的优势。
    
-   **网络编码 (RLNC)**：探讨比 Reed-Solomon 更高效的数据可用性采样编码方式，降低带宽开销。
    
-   **Monad 开源项目**：
    
    -   **存储优化**：替代 MPT 树，减少磁盘 I/O。
        
    -   **无状态验证 (Stateless Validator)**：利用更小的 Witness 降低节点门槛。
        
    -   **Monad EVM**：支持更大合约（~500KB）及异步 I/O 优化。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->




Alex Tian 导师在“Web3 实习计划”分享会中提到 **ERC-7962（Key Hash Based Tokens）**

该协议由 DataDance 团队提出，旨在通过零知识证明（ZK）原理解决区块链资产所有权的隐私与用户体验问题

ERC-7962 是在 ERC-721 基础上应用 ZK 原理的创新，实现了资产所有权与交易发起地址的解耦

**所有权标识：** 资产所有权不再绑定到以太坊地址，而是使用 **KeyHash（公钥哈希）** 作为标识

**地址不上链：** 链上不存储持有者的真实地址，从而避免了通过地址追踪资产归属

**多资产支持：** 同时覆盖了 **ERC-KeyHash721**（非同质化代币）和 **ERC-KeyHash20**（同质化代币）两类资产模型  
  
适用场景：

**隐私与匿名资产：** 适用于对隐私保护有极高要求的匿名 NFT 或资产交易 。

**Web3 新手入门：** 配合“新手友好钱包”，降低用户对私钥管理、Gas 费等概念的认知门槛 。

**企业级应用：** 方便企业进行大批量的资产分发、空投，或在 Paymaster 体系下代付 Gas 费 。

  
  
Alex 老师提到，星巴克的 Odyssey 项目虽然是 Web3 的尝试，但面临一个核心痛点：**Web2 用户难以理解“钱包地址”和“Gas 费”**。

  
**痛点一：** 用户需要备份私钥/助记词，一旦丢失资产就找不回。

**痛点二：** 任何链上操作（如领取勋章）都需要账户里有原生代币（如 MATIC）作为 Gas 费。

### ERC-7962 如何解决该场景问题

在分享中，星巴克案例被用来展示 ERC-7962 的“用户友好型设计”：

-   **无感化的身份验证：** 通过 `KeyHash`，用户不需要直接操作长长的十六进制地址。身份可以与用户的现有社交账号或简单的签名密钥绑定。
    
-   **所有权与发起权分离：** 在星巴克场景下，用户只需要进行一次简单的 **ECDSA 签名**。实际将资产写入区块链的操作（即支付 Gas 费的过程）可以由星巴克官方或第三方转发器（Relayer/Paymaster）来完成。
    
-   **资产隐私保护：** 传统的 ERC-721 会暴露某个地址持有星巴克的所有勋章。使用 ERC-7962 后，外界在链上只能看到一个哈希值（KeyHash），无法直接通过浏览器关联到用户的真实钱包地址或现实身份，实现了会员权益的隐私化。
    

### 3\. 核心结论

Alex 认为，像星巴克这样的大型品牌应用，不需要用户去学习“助记词管理”或“买币付 Gas”。ERC-7962 提供了一种签名即所有权的逻辑：

1.  **用户：** 仅需签名（非常类似 Web2 的登录体验）。
    
2.  **品牌方：** 负责 Gas 代付（Sponsorship）和上链。
    
3.  **协议：** 确保了资产的去中心化所有权和隐私性。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->





![屏幕截图 2026-01-17 210033.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-17-1768662485478-_____2026-01-17_210033.png)

![屏幕截图 2026-01-17 210038.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-17-1768662464045-_____2026-01-17_210038.png)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->






阅读了ethpanda发布的100年后的数字生活  
下面写了些观后感和ai总结

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-16-1768578897732-image.png)

**时间轴：** [1:53](https://x.com/ETHPanda_Org/status/1846109488442757627?t=113) - 一百年后还有没有身份证？还是所有身份都是基于 DID 的？ [11:36](https://x.com/ETHPanda_Org/status/1846109488442757627?t=696) - 基于无限的数字分身和 DID，未来的信用体系会如何运转？ [14:39](https://x.com/ETHPanda_Org/status/1846109488442757627?t=879) - 未来大家的主流身份认同感更多的是国际主义吗？ [22:12](https://x.com/ETHPanda_Org/status/1846109488442757627?t=1332) - 一百年后人类和 AI 如何进行合理分配？ [27:08](https://x.com/ETHPanda_Org/status/1846109488442757627?t=1628) - 开源项目如何合理分配收到的捐赠和收入？ [30:48](https://x.com/ETHPanda_Org/status/1846109488442757627?t=1848) - 包括几十亿人和 AI 的网络社会，是否会产生新的社会制度？ [35:31](https://x.com/ETHPanda_Org/status/1846109488442757627?t=2131) - 未来除了 QF、RPGF 等，还会有新的公共物品资助方式吗？ [44:56](https://x.com/ETHPanda_Org/status/1846109488442757627?t=2696) - 未来是否会有全新的开源协议，可以支持收益自动化分配？ [47:04](https://x.com/ETHPanda_Org/status/1846109488442757627?t=2824) - 未来世界如何确认开源代码和数字公共物品的所属权？ [52:58](https://x.com/ETHPanda_Org/status/1846109488442757627?t=3178) - 一百年后的科研是如何进行的？ [1:00:09](https://x.com/ETHPanda_Org/status/1846109488442757627?t=3609) - 一百年后我们会不会在火星上建设以太坊节点？ [1:02:57](https://x.com/ETHPanda_Org/status/1846109488442757627?t=3777) - 构建一百年后数字社会，我们还缺少什么加密算法？（Cypherpunk） [1:08:28](https://x.com/ETHPanda_Org/status/1846109488442757627?t=4108) - 学习 ZK 有没有好的建议和学习资料推荐？ [1:11:35](https://x.com/ETHPanda_Org/status/1846109488442757627?t=4295) - 是否有 EMO 或者心态崩了的时候，如何调节心理健康？ [1:14:14](https://x.com/ETHPanda_Org/status/1846109488442757627?t=4454) - 饮食习惯和运动习惯，如何实现长寿和身体健康？ [1:16:15](https://x.com/ETHPanda_Org/status/1846109488442757627?t=4575) - 为什么关注和支持 Longevity（长寿）？ [1:20:28](https://x.com/ETHPanda_Org/status/1846109488442757627?t=4828) - 推荐一本最近在看的比较有意思的书？ [1:24:48](https://x.com/ETHPanda_Org/status/1846109488442757627?t=5088) - 小彩蛋  
  
  
观后感： 这不仅仅是聊技术，更多是在聊\*\*“一种新的文明形态”\*\*。从身份证的消亡，到人类如何与 AI 分钱，再到火星上的以太坊节点，核心逻辑只有一个：**当物理边界被打破，代码和算法如何构建新的社会秩序？**  
  
  
**ai总结：**1\. 身份与社会：不管是人是鬼，先要有 DID

_(01:53 - 14:39)_

-   **身份证要进博物馆：** 100年后，物理世界的“身份证”大概率会被淘汰。一切基于 DID（去中心化身份）。不仅证明“我是我”，还能承载信誉。
    
-   **国际主义是主流：** 现在的国籍概念会变淡。在链上，大家更多是一种“国际主义”的身份认同。
    
-   **数字分身：** 未来的信用体系不是查你的银行流水，而是基于你在数字世界留下的痕迹（和你的无限数字分身）。
    

### 2\. AI 与新制度：当 AI 比人多时怎么办？

_(22:12 - 30:48)_

-   **资源分配难题：** 以后社会里不仅有几十亿人，还有数不清的 AI Agent。它们也要消耗资源，也要交易。现有的公司制度、国家制度肯定不够用了，需要新的博弈机制。
    
-   **社会制度重构：** 这是一个巨大的脑洞——当网络社会的成员主体从“人”变成了“人+AI”，甚至 AI 占大头时，我们可能需要一种全新的“社会契约”。
    

### 3\. 搞钱与开源：不再用爱发电

_(27:08 - 47:04)_

-   **公共物品资助（Public Goods）：** 这是 Web3 的老生常谈了。除了现在的 QF（二次方募资）和 RPGF（追溯性募资），未来肯定会有新玩法。
    
-   **自动化分配：** 这一点最性感——未来的开源协议，可能自带“收银台”。代码被调用了，收益自动分配给贡献者。**确权即收益**，不需要人为地去“捐赠”，而是写在协议层里。
    

### 4\. 硬核技术与星辰大海

_(52:58 - 1:08:28)_

-   **火星节点：** 在火星上跑以太坊节点，最大的敌人是光速（延迟太高）。这不仅是技术问题，也是为了人类成为多行星物种做准备。
    
-   **科研范式转移：** 现在的科研圈子太封闭，未来科研本身可能也会 DAO 化，数据更透明。
    
-   **ZK（零知识证明）：** 依然是显学，不仅是扩容，更是为了隐私和安全。
    

### 5\. 个人修行：长寿与心态

_(1:11:35 - 结尾)_

-   **为什么要长寿：** 对于这种着眼于“100年后”的人来说，活得久不仅仅是怕死，而是为了**看到自己亲手铺设的未来变成现实**。
    
-   **心态调节：** 即使是大佬也有 EMO 的时候。保持健康（饮食、运动）和心理调节，是在这个疯狂的数字世界里保持理性的基石。
    

**个人感想：** 这期内容跨度极大，从底层的密码学一直聊到了人类寿命。 最打动我的一点是：**他们是真的在把“100年后”当做一个工程项目来规划，而不是仅仅在写科幻小说。** 无论是 DID 还是 RPGF，都是为了解决那个未来社会可能出现的问题而埋下的伏笔。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->







[unphishable.io](http://unphishable.io)  
今天主要是在这个网站做了一些钓鱼攻击的安全测试  
  
  
写了四周运营求职计划

![屏幕截图 2026-01-15 235539.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-15-1768492602254-_____2026-01-15_235539.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-15-1768492575712-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-15-1768492620287-image.png)
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->








今天没有太多高密度的学习  
倒是有些感悟 就当日记来写了  

深入Web 3和AI行业后，雨后春笋般的项目，目不暇接的技术革新，让我感到焦虑，感觉自己本身在行业里就是新人，特别落后，但时代还在马不停蹄地向未来发展。一旦一天没有学习，就没了紧跟时代的紧迫感，慵懒会趁虚而入。但这份慵懒，却意外让我更专注当下的生活——一顿美味的午饭、和对象相处的快乐时光，在暖阳下漫无目的地散步，晒着太阳放空发呆，或是什么都不做、单纯沉浸在自然里的一整天，这些小而美，总能带来踏实的幸福感。

我留意到身边00年之前的朋友，有些人成了资本链条上的齿轮。其实做齿轮并不可怕，毕竟一份稳定的收入，才是生活最基本的保障。但可怕的是安于现状、就此躺平，把机械重复的工作当成最终归宿，白天的劳作耗尽所有精力，下班之后便只顾着歇缓，既没心力琢磨如何脱离这种“用时间换钱”的循环，更谈不上保留那份不拘泥于做执行者的心气。要知道，一旦陷入这样的状态，命运里那些稍纵即逝的机会，大概率都没有兑现的可能。

每每想到身边比我大的朋友，这让我暗自松了口气，毕竟我才20岁，不必为了稳定收入，困在这种重复劳动里，反而有精力去探索构造有价值的项目或商业模式。每次想到这些，焦虑的心情就会放缓，20岁的年纪，本就有着无限可能。

脱离行业的高密度信息裹挟时，没有了焦虑，反倒能更真切地感受生活的美好。我想，人总要在互联网行业的信息焦虑，和现实生活的小确幸之间找到平衡。况且，行业高速发展带来的焦虑，也在催着我们更接近未来世界的真相，这本身也不是什么坏事。碰巧最近在看《西部世界》，没准未来人类马上就被仿生人控制了呢。

13.01.2026
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->









今天学习吸收内容比较多：

1.colearning聊了社区投票权重的问题

问题：发言越多投票权重越大，但如果发言对社区治理没有实际意义呢，例如：我说十句没用的话，他说了一句有用的话，最后他在投票权重大，这个问题如何解决？

明天进行调研

![屏幕截图 2026-01-13 170912.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319088070-_____2026-01-13_170912.png)

2.bruce老师讲解web3相关技术原理

大多数内容为我先前已经知道的，中途退出了，明天再看一遍回放

![微信图片_20260113210719_255_22.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319223925-_____20260113210719_255_22.png)

3.通过lxdao制作的 [https://nft.myfirst.io/](https://nft.myfirst.io/) 重新补充了我对nft的认知  
  
同时我今天在colearning中问了一个问题：

开发网站，组织活动，需要大量人力和精力，组织者likeBruce，为什么要举办这些训练营，有什么目的？

answer：web3目前整体网络环境较差，属于混沌的发展阶段，需要有人来积极推动这个行业正向发展  

我的感受：原本创始人是带着使命感在做这些事情。从宏观行业角度，已经上升到推动行业发展的高度了，确实伟大。从围观个人角度，让我链接到了行业很多人才朋友，同时相比我一开始通过gemini做我的老师自学web3和炒币，这样的实习训练营丰富了我的知识，让我的学习更加系统。

再次感谢ethpanda和lxdao的创始人们！@ETHPanda和@LXDAO

![屏幕截图 2026-01-13 220114.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319062110-_____2026-01-13_220114.png)![屏幕截图 2026-01-13 213128.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319033696-_____2026-01-13_213128.png)![屏幕截图 2026-01-13 213110.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768318952319-_____2026-01-13_213110.png)![屏幕截图 2026-01-13 213118.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768318961336-_____2026-01-13_213118.png)![屏幕截图 2026-01-13 213227.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319727267-_____2026-01-13_213227.png)![屏幕截图 2026-01-13 213308.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319743527-_____2026-01-13_213308.png)![屏幕截图 2026-01-13 213749.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319750642-_____2026-01-13_213749.png)![屏幕截图 2026-01-13 214250.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319759459-_____2026-01-13_214250.png)![屏幕截图 2026-01-13 214306.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319774515-_____2026-01-13_214306.png)![屏幕截图 2026-01-13 214346.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319785667-_____2026-01-13_214346.png)![屏幕截图 2026-01-13 214545.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319792525-_____2026-01-13_214545.png)![屏幕截图 2026-01-13 214552.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319815272-_____2026-01-13_214552.png)![屏幕截图 2026-01-13 220004.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319822500-_____2026-01-13_220004.png)![屏幕截图 2026-01-13 221243.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319835073-_____2026-01-13_221243.png)

# 第一个NFT!（LXDAO的项目）

![屏幕截图 2026-01-13 221311.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-13-1768319880038-_____2026-01-13_221311.png)

今天时间紧张 部分图片注释明天补上
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->










web3行业找工作讲座：

![微信图片_20260112224813_245_22.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-12-1768231439546-_____20260112224813_245_22.png)

![微信图片_20260112224815_247_22.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-12-1768231459926-_____20260112224815_247_22.png)

![微信图片_20260112224812_244_22.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-12-1768231473758-_____20260112224812_244_22.png)

![微信图片_20260112224810_242_22.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-12-1768231490666-_____20260112224810_242_22.png)

领水，转账：  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Dave-ZehaoXie/images/2026-01-12-1768232992080-image.png)

[https://sepolia.etherscan.io/tx/0x6ac42aebbc616da8369348e9142d77c963957f3724276c8437f438e622c24a15](https://sepolia.etherscan.io/tx/0x6ac42aebbc616da8369348e9142d77c963957f3724276c8437f438e622c24a15)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
