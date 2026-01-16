---
timezone: UTC+8
---

# jasmine-pixel363

**GitHub ID:** jasmine-pixel363

**Telegram:** @rjazzminq

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->
今日任务：

1.  学习手册
    
2.  看扩展资源
    
3.  参加Co-L
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->

今日任务：

1.  参加分享会
    
2.  学习1h并整理笔记
    

### Commit–Reveal Scheme（承诺–揭示机制）

hash the commit

reveal the secret

一个秘密值 𝑠，比如：

投票选择 NFT metadata 出价 随机数 未来要 reveal 的内容

把你要提交的内容 先算一个哈希值，只把这个哈希写到链上，内容本身暂时不公开。

特性：不可逆（别人看不到 s）不可篡改（你之后改不了 s）

到了约定的时间， 你把当初的秘密 𝑠 公开。

**call a reveal function**

区块链上所有状态变化都要通过函数调用

reveal 本身是一次交易, 需要 gas, 需要有人主动触发

### 为什么 Web3 非常需要这套机制？

因为区块链是：完全透明、所有交易可预测（mempool）

如果你直接 reveal：会被抢跑（front-running），会被操纵结果

### 典型应用场景：

**NFT / 元数据**

mint 时：hash(metadata + salt) 上链（hash the commit）

mint 结束：reveal metadata 社区验证 hash 是否一致

**链上投票 / DAO 治理**

**拍卖（sealed-bid auction）**

**链上随机数（避免矿工操纵）**

commit 一个随机种子 区块生成后 reveal 和区块哈希一起算随机数

### 一些概念：

**1 gwei = 10^-9 ETH**

\*\*Griefing：\*\*攻击者不一定自己获利，但通过花少量成本，故意让别人付出更高成本、失败或系统变坏

Commit–Reveal 中的 griefing（最典型）

正常流程：commit reveal 结算

Griefing 方式： 攻击者 commit 但 故意不 reveal

后果：系统卡住 别人没法结算 或者要等很久 / 失败

**ENS-Etherscan Reverse record**

需要3 次交易来完成一个ens id的创建

**Wrapped Token** = 把原生币“包装”成 ERC-20 代币，以便在以太坊生态里无缝使用 DeFi、DEX、智能合约。

WETH 直接在链上 wrap / unwrap，去中心化

WBTC 依赖托管，稍微中心化

都是 1:1 锚定资产，不用担心价格差异太大

在 DeFi 里流动性和智能合约兼容性是它们的主要价值

This contract runs on Ethereum as the **substrate**（下面自动触发学英语技能）

**Custodial Wallet** 托管钱包 钱包由第三方托管 用户 不直接控制私钥，第三方帮你管理资产

MetaMask 的 Custodial 选项（和交易所账户绑定）

Non-Custodial Wallet（非托管钱包）

**Seed Phrase**（助记词 / 种子短语） 是一组 通常 12 或 24 个单词，用来生成你的 钱包私钥和地址，也就是你的资产的“万能钥匙”。

centralized exchange / decentralized exchange

**hold reserve for both** = 为两个对象都保留（储备）资金或资产

**Arbitrage（套利）** 指 利用不同市场或平台之间的价格差异进行买卖，从中获取无风险利润。

自动化交易（Bot）：套利大多靠高频交易机器人执行

跨交易所套利（Exchange Arbitrage）

跨链套利（Cross-chain Arbitrage）

三角套利（Triangular Arbitrage）

去中心化交易所（DEX）套利 同一链上不同 DEX 的价格差示例：Uniswap ↔ SushiSwap ↔ Curve

**Sybil Attack**（雪崩攻击 / 冒名攻击）

指 一个人或实体创建大量虚假身份来操控网络或投票权 的攻击行为。 源自 “Sybil” 一词，来自一位多重人格患者的名字

**在 Web3 尤其重要，因为：**

DAO 投票权通常按地址计算

DeFi 激励按用户数量发放

**一个攻击者可以通过大量虚假账户作弊**

**Civil Resistance = 非暴力反抗（守规则改变结果）**

### Uniswap

是 最著名的 Ethereum DEX，基于 AMM（Automated Market Maker，自动做市商）

用户界面友好

操作感和中心化交易所差不多 你甚至可以看价格、选择 slippage、确认交易 但底层实际上是 智能合约执行 swap

### **Uniswap 的底层工作原理**

1.  **资产在流动性池里（Pool）**
    

每个交易对（比如 ETH/USDT）有一个池子，流动性提供者（LP）存入等值代币

2.  **价格由公式决定**
    

x \* y = k（恒定乘积公式），交易时会根据池子余额自动调价

3.  **交易不经过中心化服务器**
    

完全在链上执行 交易结果即时写入区块链

### Etherscan

在 Etherscan → Overview 里：

Balance ：你这个地址当前持有的 ETH 数量

Value：这些 ETH 按当前价格折算成的 美元价值（USD）

Token： 你这个地址持有的 代币（ERC-20 / ERC-721 / ERC-1155）

为什么 Value 不包含 Token？

因为：

1.  ETH 是链的原生资产（Native Asset）
    
2.  ERC-20 / NFT 的价格：依赖外部市场 流动性不同 很难统一准确估值
    

**ERC-20 Tokens：DAI USDC UNI WETH**

**NFT（ERC-721 / ERC-1155）：NFT 收藏品 游戏道具 数字艺术**

3.  Etherscan 是 区块浏览器（block explorer） 不是资产管理工具
    

## DAI 如何保持其稳定币（stablecoin）价值稳定？

### DAI 是什么

DAI 是一个 去中心化稳定币（decentralized stablecoin），主要目标是 1 DAI ≈ 1 USD

它是由 MakerDAO 系统发行和维护的

不像 USDT/USDC 那样有中心化储备金，而是 通过\*\*抵押资产（collateral）\*\*生成

### 核心机制

DAI 保持稳定的核心方法是 供应-需求调节 + 超额抵押（over-collateralized） + 激励与惩罚机制。

1.  **超额抵押生成 DAI（Over-collateralized Loan）**
    

用户把 加密资产（ETH、WBTC 等）抵押在 MakerDAO 的智能合约里

然后 借出 DAI → 这就是 CDP / Vault

CDP / Vault = **Collateralized Debt Position**，中文叫 **抵押债仓**

抵押要求 超额抵押（Over-collateralization），比如抵押价值 150 USD，最多借出 100 DAI

这样即使抵押物价格下跌，也保证 DAI 背后有足够资产支撑

2.  **稳定目标：DAI Price ≈ 1 USD 系统通过 Stability Fee、Liquidation、Burn/Print 来调节**
    

MakerDAO 有 Target Price（目标价） = 1 USD

DAI 的市场价格可能高于或低于 1 USD

**##Burn & Print（销毁与铸造）**

当 DAI 价格偏高（>1 USD） 系统鼓励 更多人生成 DAI（print DAI）

当 DAI 价格偏低（<1 USD） 鼓励 用户用 DAI 还债并销毁（burn）

这就是 Supply-Demand Mechanism（供需机制） 的核心逻辑

**##Stability Fee（稳定费 / 利息）**

借 DAI 的人需要支付 利息（Stability Fee）通过调节利息来控制DAI的价格 本质上也是供需关系

**##Liquidation（清算机制）**

抵押物价值下跌到 低于安全阈值（Collateralization Ratio）

例如 ETH 价值下降，Vault 债务过高 系统会 自动清算（liquidate） 抵押物

**##Marker / Secondary Token（MKR）**

MKR = MakerDAO 治理代币 / 备用代币

用途：支付 Stability Fee 在系统亏损时稀释持有者

**Collateral（抵押物） = 用来支持债务或稳定币的资产**

即使 ETH 跌破借出的 DAI 价值，**DAI 持有人仍然 1:1 USD 安全**，**亏损由借款人抵押物和系统备用代币 MKR 承担**。

![DAI的价值稳定机制.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/jasmine-pixel363/images/2026-01-15-1768476941582-DAI_______.png)

### More English：

All of a sudden, the collateral was worth as much as the token, so they needed to start selling off to cover that and that crashed too, both of these things crash at once sending it into a death spiral.

### **Civil Resistance**

（公民抗争 / 非暴力抵制） 的核心不是“对抗”，

而是：在不使用暴力、不破坏系统规则的前提下，通过集体行为、拒绝配合或重新分配权力，来对抗不公平的制度或权力结构。

**“规则内反抗规则”（Resistance within the system**）

Civil resistance ≠ 破坏系统

而是：不 fork 链、不 DDoS、不 hack

而是：用 open-source、用 permissionless access、用 exit（离开）而不是 voice（申诉）

这在政治理论里叫：Exit as resistance（用离开本身作为反抗）

Sybil Attack是“用假身份操纵结果”

**Token Drop vs Scam（骗局）**
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->










今日任务：

1.  整理昨天分享会内容
    
2.  参加Co-learining，继续学习手册
    
3.  参加分享会
    

# Web3 运行原理

### 交易（Transaction or tx）

\= 要做的事（如转账、调用合约mint、投票等）+手续费（Gas fee，为网络资源付费，并提供激励）+防重放序号（Nonce，避免同一笔交易被重复执行）

### 钱包的作用：

组装交易内容

**用私钥签名**（生成数字签名，证明是你授权）：

用私钥签名一段消息、任何都都无法伪造你的签名除非拿到私钥、任何人都可以用你的地址验签

广播到区块链网络

**Gas Fee：**

提高做恶成本，防止垃圾交易淹没网络

激励打包者/验证者提供算力与带宽

形成经济闭环：资源使用者付费→网络可持续运作

**从一笔交易到出块的流程**：

Wallet（签名） → RPC/Node（传播）→Mempool（排队）→Builder/Validator（挑选）→Block（落盘）→Explorer（可查）

### 为什么不可篡改需要时间:

区块像账本，需要引用上一页的指纹，区块越往后叠，被推翻的成本越高。

PoS下存在“最终确认（finality）”：通常需要约12-13分钟达到更稳定的最终状态

（PS：以太坊12秒打包一个块，但是要12-13分钟来finalize这个块 使得交易完成）

### 共识机制: PoW vs. PoS

匿名且不可信任的全球节点，如何认同同一本账？

**PoW（工作量证明）BTC：**

算力竞争写账、谁先算出谁记账、全网快速验算

**PoS（权益证明）ETH：**

质押 ＋ 随机选人写账（更节能）

类似于“押保证金参加抽签当记账员”：押得多/信誉好概率更高；作恶会被罚没押金

### RPC：

钱包通常不直连全网：它连一个RPC（访问节点的API），RPC背后是节点或节点集群，所以RPC往往是“中心化入口风险点”。

打开钱包查看网络设置：RPC URL/Chain ID

### 智能合约：

Code is law 难篡改 可追溯

合约写好放到这个区块链上  节点发出请求指向一个合约  拉到虚拟机运行

以太坊如何改规则：

### EIP的基本路径：

先讨论（Magicians 论坛）→再形成文档（EIPs）→再进入升级（每年1-2次）

通过社区讨论推进 每一次升级都是一次硬分叉

### 节点客户端：

协议需要节点客户端来落实，共识层和执行层是一个客户端的共同组成部分：

共识层负责链和区块的选择、确定等共识；执行层负责交易执行、状态等具体活动

节点客户端具有多样性：多样性越高越不怕单一软件bug/分叉风险，以太坊自上线来，从未有过宕机

实操:看主网节点/验证者分布数据

● 节点软件占比:[https://ethernodes.org/](https://ethernodes.org/)

● Rated Network Explorer 查看 Validators(验证者)

● Etherscan Node Tracker(节点服务器) [https://etherscan.io/nodetracker](https://etherscan.io/nodetracker)

以太坊：“97万个validator 每个质押32个ETH，约一万三千多台服务器支持（矿机） ”

### Web3的关键特性：

钱包创建：高度去中心化（任何人都可以生成钥匙）

交易广播：可能去中心化（RPC入口、前端入口）

网络运行：节点越分散越安全

客户端越多样越稳（减少单点软件风险）

**严格来说，CEX 和 DEX 都是数字资产交易平台，但它们在 Web3 的概念中属于不同阵营**。

典型 Web3 应用：去中心化、用户掌控私钥、数据透明、无需中心化机构

### DEX（Decentralized Exchange，去中心化交易所） ：

基于**智能合约**运行（Uniswap、SushiSwap、Curve）

用户资产**保存在自己的钱包**里（例如 MetaMask）

**无需 KYC**，**交易在链上直接执行**

依赖流动性池（**AMM**）或链上订单簿

### CEX（Centralized Exchange，中心化交易所）：

由公司/机构运营（比如 **Binance、Coinbase、OKX**）

用户**资产托管在交易所钱包**，**交易通过交易所内部撮合**

\*\*KYC（实名认证）\*\*和合规要求严格

提供\*\*高流动性、快速交易、\*\*法币充值/提现

→用户无法完全控制资产私钥，数据和交易由中心化平台掌控。

\*\*钱包类型：\*\*软件钱包（Metamask等 有风险 看具体）、硬件钱包（OneKey等 风险低）、交易所钱包（Binance、Coinbase）

# Co-learning经验分享：

“从推上热门的活动分析 手把手教怎么参加 分析成本与预估收益 在推上不断输出 不断分析 对找运营工作有用 最好用中文”

“Lighter 和after是链上的 可以查看数据面板 需要自己去参与”

“活动的内容、规则、他们的一个CPR、FTT是多少,活动参与路径是怎样的。积分活动可以大致推断他们的活动成本”

”比如某个CEX出了对应的活动,你可以去分析这个活动他的单次注册成本(CPR)是多少、首次入金(FTT)的成本是多少?也就是把自己放在活动设计者的角度,去看一个活动的数值、任务、文案等的设计”

“所以像是參與項目的過程，所做的事情都可以進行分析描述，當作案例分析這樣寫進履歷這樣嗎。新手教程 感想 活动为什么办 好不好 能不能解决问题”

“开发者关系、产品PM 技术和运营“”

[“https://blockworks.co/news/maker-rebrands-as-sky-dai-will-be-changed-to-usds](https://blockworks.co/news/maker-rebrands-as-sky-dai-will-be-changed-to-usds) DAI 的升級新聞。感覺就是由去中心變成中心化的穩定幣了,包含了凍結代幣的功能”

“gamefi更多的是使用nft绑定装备,或者使用一些经济模型绑定里面的资源重点还是金融属性 不去考虑游戏性”

“链上数据分析：Dune”

### 补充一点稳定币的内容：

\- **中心化稳定币**(USDT/USDC):在不同链上由发行公司部署,需与发行公司谈判并付费

\- **去中心化稳定币**(如DAI):

\- 通过超额抵押实现,锚定一揽子货币价格

\- 利用套利机制维持1美元锚定:

\- 低于1美元时有套利空间,促使购买

\- 高于1美元时有动机卖出

\- 风险在于经济模型漏洞,不仅是技术漏洞
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->















# 今日任务：

完成钱包以及NFT任务

看Web3手册并做笔记

# 笔记：

## 钱包

链接钱包（Connect Wallet）是Web3应用的重要特征，链接后网站将会读取你再区块链上的全部信息。你的钱包就是你的账户，是匿名的，无需任何密码或联系信息。

## 助记词、私钥、公钥

一个助记词可以产生无限个钱包。

每个钱包包含一个钱包地址（公钥）和一个私钥。钱包地址可公开，私钥不行。

如果助记词丢失，全部钱包将丢失。如果单个钱包私钥泄露，仅对应钱包受影响。

### 如何应对助记词或私钥泄露或丢失？

1.  使用Bitwarden 或 1Password等专业密码管理器管理
    
2.  保存或转发时，使用 分段复制+手动输入+断网等多种方式
    
3.  有大量资产及丰富加密货币经验的用户，使用硬件钱包
    
4.  不安装来历不明的软件、不点击有风险的链接
    

### 钓鱼攻击

原理是：伪造官网、协议或攻击官方账号发布虚假信息引诱你转账、购买虚假NFT等。

常见攻击方式是：

1.  伪造协议让你签署，然而实质协议内容是恶意转账等。比如一些不明来源的Signature Request
    
2.  钓鱼邮件攻击，里面的链接为钓鱼网站
    
3.  在Opensea等交易平台，伪造相似头像或名词，让你误解并购买
    
4.  在第三方网站下载的钱包，攻击者会替换或插入恶意代码
    
5.  社交媒体攻击，“热心”群友诈骗 知名案例：周杰伦的300多万人民币的NFT被钓鱼盗走
    

应对方式：

1.  分散资产，规划钱包用途。比如储蓄账户就减少操作，日常高频使用账户仅存放少量资金。
    
2.  操作越多，风险越多，不要乱点。
    
3.  警惕任何人发的消息，即时是认识的朋友或官方渠道，因为他们有可能被黑了。
    
4.  刷空头和参与新项目是，最好咨询有经验的朋友或查看合约代码等，确保没问题再操作。
    
5.  “但凡在GMGN/Dexscreen上粘贴合约地址没有找到的项目、或有风险提示的项目，千万不要买入就对了。警惕没有社群、没有项目方推特，甚至在x上没有任何讨论度的项目，最好只相信有大投资机构投资背书的项目。”
    
6.  “新手最好在大交易所玩，也不要一上来就玩meme玩合约，把你的投入控制在你能接受亏空的范围之内，被骗了也不会太崩溃。”
    

### 浏览器插件攻击

浏览器插件具备二点权限比较高，可以修改网页代码来替换和篡改正常的合约，从而实现钓鱼攻击

应对方式：新开一个Chrome Profile仅安装MetaMask插件，不安装其他插件。需要使用钱包交互时仅用这个Profile操作。参考：[https://support.google.com/chrome/answer/2364824](https://support.google.com/chrome/answer/2364824)

## FT vs. NFT

FT (Fungible Token) 是可以被任意交换的代币，像钱

### NFT (Non-fungible Token)

是独一无二，不可分割，难以用来平等交换的代币

1 ETH +2 ETH = 2 ETH, whereas Every NFT is unique, can’t be divided, can’t be equally traded.

万物皆可NFT, 可以在虚拟世界代表任何非钱币的实体，具体功能和玩法由参与者定义，具有无限想象空间

NFT基于区块链技术，任何人都可以查看且不可篡改；验证版权归属成本很低，可快速确认版权；NFT所有者对其具有完全控制权，不可被剥夺 (大多数数字藏品不属于真正的NFT，因为没有上链去中心化，可以被平台控制)

### NFT的现状

目前大部分NFT是头像类型的项目。但也开始逐渐出现更多的NFT玩法，比如社区通行证、游戏道具、门票等。

目前NFT市场还处于非常早期的阶段，作品质量参差不齐。所以在参与购买NFT时，需要DYOR。

### 什么是DYOR?

DYOR (Do Your Own Research)：意思时要你自己做研究来决定是否购买，是一个免责声明。

时刻问自己：你愿意随便给一个陌生人送钱吗？

PS：FOMO（Fear Of Missing Out） FUD (Fear, Uncertainty and Doubt)

[https://nft.myfirst.io/](https://nft.myfirst.io/)

![mfnft.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/jasmine-pixel363/images/2026-01-13-1768291577611-mfnft.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->






















今日任务：

1.  参加今日三次会议（以太坊周会、Co-learning、行业分享会）
    
2.  完成4个通用任务提交
    
3.  完成入门导读部分学习
    
4.  看学习以太坊第一章
    

（专注运营、有余力再学习入门技术）
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
