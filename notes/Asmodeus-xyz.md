---
timezone: UTC+8
---

# Asmodeus-xyz

**GitHub ID:** Asmodeus-xyz

**Telegram:** 

## Self-introduction

QS600+金融本硕；Web3初学者

## Notes

<!-- Content_START -->
# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->
# 一、EOA的定义与控制方式

## EOA（Externally Owned Account，外部拥有账户）

-   有私钥
    
-   能“主动发起交易”
    
-   没有合约代码
    

## EOA状态

-   地址（address）：从公钥派生，20 字节，以 0x 开头的 40 个十六进制字符。
    
-   nonce（交易计数器）：表示这个账户已经发送过几笔交易。
    
-   balance（余额）：账户持有的 ETH 数量，以 wei 计（1 ETH = 10¹⁸ wei）。
    
-   code / storage：code 为空，没有合约代码；storage 也为空。
    

# 二、合约账户的概念与创建流程

## 合约账户（Contract Account）

-   无私钥，由部署时的代码“锁死行为”
    
-   不能主动发起交易，只能“被调用”后按合约逻辑执行
    
-   有代码和存储（storage）
    

## 合约账户

-   nonce：曾经用 CREATE / CREATE2 创建过多少个合约。
    
-   balance：账户持有的 ETH 数量。
    
-   storageRoot：该合约存储 trie 的根哈希（合约状态的入口）。
    
-   codeHash：EVM 字节码的哈希，用于在底层存储中定位合约代码。
    

## 创建合约交易

-   起点是某个 EOA 的签名（只有EOA能发起合约）
    
-   看到 to == null，识别这是创建合约交易
    
-   运行 data 里的 init code
    
-   init code 的最后会 RETURN 一段字节码——这就是 runtime code
    
-   EVM 把这段返回值作为新合约账户的 code，写入世界状态
    

## 合约地址的计算

-   普通 CREATE：sender + nonce 决定地址
    
-   CREATE2：0xff + deployer + salt + keccak(init\_code)
    
    -   和 nonce 无关，因此可以跨链用相同的组合在多条 EVM 链上得到同一地址
        
    -   0x表示16进制
        

# 三、合约账户的余额与状态存储

| 属性 | EOA | 合约账户 |
| 私钥 | 有私钥，可签名交易 | 无私钥，不能主动签名 |
| balance | 有余额字段 | 同样有余额字段（可以像 EOA 一样持有 ETH /代币） |
| codeHash | 逻辑上“无代码”；规范中存的是空代码哈希 | 有字节码哈希，用于合约执行与识别 |
| storageRoot | 逻辑上“无存储”；规范中是空存储根 | 有 trie 根，用于存储合约状态 |

-   补充解释 codeHash
    
    -   在以太坊中，codeHash 是合约账户（Contract Account）状态的一部分，表示该合约账户的字节码（bytecode）的哈希值。
        
    -   对于 EOA，由于没有合约代码，codeHash 通常为空字符串的哈希…
        

# 四、EOA 与合约账户的互相调用机制

## 外部交易：EOA到合约账户

EOA 用户通过私钥签名发起外部交易（external transaction），交易中会包含：

-   to：目标合约地址（或 EOA 地址）
    
-   data：编码后的调用数据（函数选择器 + 参数）
    
-   value：附带发送的 ETH（可选）
    
-   gas 与 gasPrice / maxFee 等字段
    

## 内部交易：合约账户之间

-   call：普通调用，对方在自己的存储中执行逻辑，msg.sender 变为当前合约；
    
-   delegatecall：在当前合约的存储上下文中执行对方代码——存储读写仍落在发起方（调用者）合约上，msg.sender/msg.value 保持外层值，常用于代理 / 升级模式；
    
-   staticcall：只读调用，不允许修改状态，只能 view/pure。
    

# 五、MetaMask 钱包对 EOA 的管理

当你初次使用 MetaMask 创建钱包时，它会在本地生成一个 12 个英文单词的Secret Recovery Phrase（SRP，助记词），遵循 BIP-39 标准，从这个种子可以派生出多个私钥与 EOA 地址。

## 六、代币与合约账户的关系

-   代币操作是合约调用
    
-   “转代币” 本质上是 “调用代币合约的一段代码 + 改一行存储”
    
-   用户持有代币，是合约里那份“账”
    

# 七、合约部署后的不可篡改性与销毁

## 代码不可更改

## SELFDESTRUCT：从删除到废弃

## metamorphic 合约模式已经失效：SELFDESTRUCT + CREATE2

-   地址不变 → 代理合约
    

## 代理合约模式（Proxy Pattern）

-   主合约（proxy）永不变，只是可指向不同的实现合约地址，通过 delegatecall 调用新的逻辑，实现功能升级，同时保留原始地址和状态。
    
-   换实现合约地址
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->

# 一、节点与客户端

## 节点

-   运行以太坊客户端软件并连接到其他节点的电脑
    
-   你的应用 / dApp / 脚本，以太坊节点（Ethereum Node），P2P 网络（p2p）
    

## 客户端

是对以太坊协议的具体实现

-   执行客户端（Execution Client，EL）
    
    -   执行交易和智能合约
        
    -   法院里的书记员 + 案卷系统，负责接收材料、执行具体流程、更新“案卷状态”
        
    -   常见有Geth（go-ethereum）、Nethermind、Besu、Erigon、Reth
        
-   共识客户端（Consensus Client，CL）：
    
    -   权益证明 PoS 共识
        
    -   法官，根据书记员提供的案卷（执行结果），做出“这是不是合法区块、最终选哪条链”的裁决
        
    -   常见有Lighthouse、Prysm、Teku、Nimbus、Lodestar
        
-   验证者客户端（Validator）——作为共识客户端的“插件”。
    
    -   提议新区块，attestation（投票）
        
    -   陪审团代表，按规则投票并在应该发言时站出来，拿质押的
        
    -     ETH 当“保证金”
        
-   . Engine API：执行层和共识层之间的“专线电话”
    
    -   一个专门给 EL ↔ CL 用的 JSON-RPC 接口
        
    -   是法官与书记员之间的专用内线电话，上面通的是区块骨架和执行结果，而不是对外公开的 REST / RPC API
        

## 节点和客户端的关系

-   一台电脑上跑着 1 个执行客户端 + 1 个共识客户端 → 这是一个全节点；如果再挂上一个或多个验证者 → 这是一个验证节点；
    
-   以太坊节点 = 一台电脑 + 两个必备客户端 + （可选）一个验证者客户端
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MGFhOThhZTJlOWQyODE3YTBmZmFmNjAxZjIyNWNhMzVfTGtSbHQ5TXZTZjkwV2hwb2lwRmZ1aW9sNDNVSmdGNkdfVG9rZW46RXpvb2Jsc2Rwb0FjaEZ4bHYyMGNDcEtxbnRkXzE3Njg0ODIwNjk6MTc2ODQ4NTY2OV9WNA)

# 二、节点间的连接与通信方式

## 发现阶段（UDP + Kademlia）：

先通过引导节点 + Ping/Pong/FindNode，逐步认识一群“距离 ID 较近”的 邻居，把自己的“通讯录”填满。

## 连接阶段（TCP + RLPx/devp2p）：

对选中的邻居建立加密的 TCP 连接，在同一条线路上多路复用区块同步、 交易传播等子协议。

## 传播阶段（Gossip + 请求/响应）：

新交易和区块通过 Gossip 像办公室八卦一样在全网扩散；缺失的历史数据 则通过点对点请求-响应按需拉取。

# 三、全节点、轻节点、归档节点的区别

全节点 = 当前状态 + 全部区块（但老状态会被“裁剪”）；

归档节点 = 全节点 + 从创世到现在的“所有历史状态”；

轻节点 = 只存区块头，需要时向全节点要具体数据。

# 四、执行与共识客户端的区别

## 执行客户端（Execution Client）：EVM + 状态 + RPC

-   RPC = Remote Procedure Call，即“远程过程调用”
    

## 共识客户端（Consensus Client）：PoS + Beacon + Finality

# 五、节点同步

## 三种模式

-   Snap 模式需本地重构状态树并“恢复”同步状态
    
-   Full 模式需逐块执行每笔交易并验证状态根
    
-   Light 节点同步无需构建状态，仅验证区块头与 Merkle 证明
    

## 区块 = 区块头+区块体

-   区块头
    
    -   字段名称 + 父区块哈希（Previous Block Hash / Parent Hash）+ 时间戳（Timestamp） + Merkle root（Merkle 树根）+ 与共识机制相关的字段
        
    -   区块头就像章节开头的目录 +简介：告诉你这一章是第几章（父区块哈希就像前一章的章节编号），这一章的主题是什么（Merkle root 就像章节内容摘要），什么时候写的（时间戳），这个版本的书是哪年印刷的（版本号／难度等类似）。
        
-   区块体
    
    -   该区块中的交易+所有交易的详细信息（发送者／接收者地址、金额、手续费、签名等）+ ……
        
    -   区块体就像章节的正文内容：包含所有故事情节（交易详情），对话、事件、细节等。
        

# 六、节点间交换的数据类型

## 信息类型

-   交易信息、区块数据、加密哈希、账户信息等
    

## 私钥公钥和助记词之间的关系

-   种子->私钥->公钥->地址
    

# 七、Gossip 协议

Gossip 协议相当于以太坊的“去中心化广播系统”： 它让每个节点只需和少数邻居聊天，就能在几秒内把新交易、新区块及 PoS 投票散播到整个网络，从而在没有中心服务器的前提下，实现高可用、高容错、可扩展的全网信息同步。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->


# 一、Web3合规性要求与常见法律风险

## 核心法律风险梳理

-   代币发行与交易行为的法律风险：禁止融资，其他环节同样犯法
    
-   赌博、传销、洗钱等刑事风险：随机获得NFT赌场罪、多级推广传销罪
    
-   场外交易中的洗钱与非法经营风险：非法外汇、黑钱
    
-   民商事争议：合同无效
    

1.  全球监管背景与趋势：FATF
    

## Web3 入职法律风险防范指南

-   雇佣关系新形态：灵活用工、无社保公积金
    
-   薪酬结构及风险提示：含有代币，与《劳动法》规定相悖
    
-   虚拟货币出金与合规风险：对方非法集资
    
-   项目合法性需提前审查
    

## Web3 项目的刑事风险及案例介绍

-   开设赌场罪、赌博罪
    
-   非法经营罪
    
-   非法吸收公众存款罪
    
-   组织、领导传销活动罪
    
-   洗钱罪
    
-   其他风险：虚拟货币风险、代币发行的风险、挖矿的风险
    

# 二、 常见网络安全风险与防护措施

## 常见网络安全风险与攻击方式

-   钓鱼攻击：伪造邮件/网站、社交平台钓鱼、假冒客服/好友
    
-   恶意软件/木马：伪装成面试/学习软件、剪贴板劫持、浏览器扩展/插件后门、远程控制
    
-   社交工程攻击：冒充 HR/导师/同学、群聊钓鱼、“好友”求助
    
-   供应链/第三方依赖攻击：恶意浏览器插件/扩展、开源库后门、官方渠道被劫持
    
-   地址污染与扫描木马：剪贴板劫持、输入框监听
    
-   传统隐私与账号安全风险：弱密码/密码复用、邮箱/SIM 卡劫持、双因素认证缺失
    

## 防护清单（学生版）

-   只用官方 Zoom/Teams/腾讯会议等公开工具
    
-   任何要求“连接钱包”“签名验证”“输入助记词/私钥”的网页，99% 是骗局
    
-   空投、福利只用专门测试钱包，主钱包冷存储，绝不轻易暴露
    
-   转账前务必核对地址前 6 位和后 4 位，防止剪贴板被劫持
    
-   助记词/私钥只离线保存，绝不截图、上传网盘或发给任何人
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->



DeFi（去中心化金融）、NFT（非同质化代币）、DAO（去中心化自治组织）与 MEME（模因币）四大领域

# 一、DeFi（Decentralized Finance 去中心化金融）

## Uniswap：去中心化交易所（DEX）

-   引入“自动做市商”（AMM）模式，通过“恒定乘积公式” 实现代币定价。
    
-   用户通过存入流动性池（如 ETH 和 DAI）成为流动性提供者（LP），赚取交易手续费。
    

## Compound：去中心化借贷协议

-   Compound 是一个去中心化的借贷平台，允许用户借入或借出加密资产。
    
-   用户可以通过这个代币赚取利息，同时也可以借入其他资产，但需要提供超额担保。
    

## MakerDAO（现已更名为 Sky）：稳定币系统

-   MakerDAO 是一个去中心化的借贷协议，它允许用户通过超额抵押资产来生成 DAI，一种与美元挂钩的稳定币。DAI 的价值稳定在 1 美元左右，因此可以作为一个避险工具或支付手段。MakerDAO 通过智能合约管理抵押资产，并利用 稳定费率 来调整系统中 DAI 的供需关系。
    

# 二、NFT（Non-Fungible Token 非同质化代币）

## NFT 的本质

-   数字资产的唯一性和所有权
    

## 智能合约

-   自动化的所有权转移和交易
    

## CryptoPunks：NFT 的先锋

-   CryptoPunks 是一系列由 10,000 个像素风格的头像组成，每个头像都有其独特的属性（如不同的发型、眼睛、面部特征等）。
    
-   但随着 NFT 和加密货币市场的崛起，CryptoPunks 成为了最早的 NFT 收藏品之一，并且逐渐形成了专属的社群和文化。
    

## OpenSea：NFT 交易的中心

-   OpenSea 是目前全球最大的 NFT 交易平台，基本上覆盖了所有主流 NFT 的交易。
    
-   OpenSea 通过区块链技术让任何人都能轻松买卖 NFT。
    

# 三、DAO（Decentralized Autonomous Organization，去中心化自治组织）

-   你和一群志同道合的朋友，不用任何老板，也不需要复杂的公司结构，通过投票、分享资源，共同决定一个组织的未来——这就是 DAO 的核心理念。
    

## Nouns DAO：社区驱动的 NFT 艺术 DAO

-   卖头像存入金库，购买者共同决策
    

## LXDAO：支持 Web3 公共物品的建设者

-   大家根据兴趣和专长，承担不同的任务。完成任务后，你会获得相应的奖励。
    

## ConstitutionDAO：一场疯狂的拍卖

-   购买美国宪法，失败
    
-   价格公开，信息不对称
    

# 四、MEME（模因币）

-   MEME 原意为“迷因”，即网络文化中的梗、表情包、搞笑段子等。在加密圈，MEME 通常指那些以网络文化为基础的代币，比如 DOGE（狗狗币）、PEPE（青蛙币）、SHIB（柴犬币）等。
    

## DOGE：MEME 币的开山鼻祖

-   依赖马斯克名人效应
    

## PEPE：社区驱动的 MEME 币

-   PEPE 基于“悲伤青蛙”形象，发行总量高达 420.69 万亿枚。2024 年市值突破 58 亿美元，成为 MEME 币“战神”。
    

## 五、交叉创新领域

## DeFi + NFT：数字资产金融化

-   BendDAO 的 P2Pool 模型：Floor Price 预言机实时获取 NFT 地板价，确定可借贷金额
    
-   碎片化流动性解决方案：通过 Bonding Curve 算法自动调整不同稀有度 NFT 的价格
    

## DAO + MEME：社区文化与治理融合

-   社区代币分发创新：购买 KOL 的“Key”获得私聊权限和收益分成
    
-   MEME 币社区治理模式：投票决定协议升级、新功能开发、合作伙伴选择
    
-   创新应用场景：Nouns DAO每天拍卖一个新的 Noun NFT，收益进入 DAO 金库，持有者可以提案使用资金，如资助创意项目、举办活动、开发工具
    

## AI + DeFi：智能化金融服务

-   智能投资策略、风险管理与预测、用户体验优化
    

## WEB3 + 乡建：南塘 DAO 的探索

# 六、2025新型趋势

## Intent-Based 交易

-   从具体操作到给出意图
    

## 账户抽象与智能钱包

-   从助记词到智能操作
    

## 模块化区块链

-   分工
    

## AI + Web3 融合

-   AI代理
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NjFmNjM2M2U0MmNmOWM1ODVkNTM0MmVkMDg5NzY3OWFfZDl5cW10Y3hsTUdvOHQxNFNDYnZ4amNyR3pMMFN1dWlfVG9rZW46WmVVR2JUZWE1b01Qb1J4TTlRSGM3YUgxblNoXzE3NjgyMzYzNzE6MTc2ODIzOTk3MV9WNA)

# 一、基础类

| 黑话 | 英文全称 | 含义 |
| DYOR | Do Your Own Research | 投资前请自行研究，项目方常用于免责 |
| FOMO | Fear of Missing Out | 害怕错过，指因贪婪而追高的情绪 |
| FUD | Fear, Uncertainty, Doubt | 恐慌、不确定、怀疑，指唱衰情绪或舆论攻击 |
| WAGMI | We're All Gonna Make It | 大家都会发财，社区常用打气口号 |
| NGMI | Not Gonna Make It | Not Gonna Make It，讽刺某人/项目做法不行 |
| REKT | Wrecked | 被"爆锤"，损失惨重，如投资失败、合约被黑等 |
| Airdrop | \ | 空投，免费发放代币以激励用户参与项目 |
| Whale | \ | 加密大户、巨鲸，能影响市场价格的人 |
| Degen | \ | "投机狗"，不问项目质量只冲高风险高回报机会的人 |
| Shill | \ | 宣传、推销（常含贬义），如"shill 项目"指恶意安利 |
| Exit Scam | \ | 项目方跑路，携款失联 |

# 二、技术类

| 黑话 | 含义 |
| L1 / L2 | Layer 1（主链，如以太坊）和 Layer 2（扩容方案，如 Arbitrum、zkSync） |
| EVM | Ethereum Virtual Machine，以太坊虚拟机，运行智能合约的核心 |
| Smart Contract | 智能合约，自动执行合约逻辑的链上程序 |
| Bridge | 跨链桥，用于在多条链之间转移资产 |
| Oracle | 预言机，用于向链上提供链下数据 |
| Fork | 分叉，指复制已有代码的项目，或区块链协议分裂 |
| Mint | 铸造，生成新的 NFT 或代币 |
| Burn | 销毁代币，减少总量，用于通缩机制 |
| Rugpull | 抽地毯，项目方卷款跑路（尤其在 DeFi 项目） |

# 三、投资类

| 黑话 | 含义 |
| Pump | 拉盘，代币价格快速上涨 |
| Dump | 砸盘，代币价格快速下跌 |
| HODL | 原为"Hold"打错，后来变成文化，意为坚定持币不卖 |
| Bagholder | "接盘侠"，高位买入亏损后长期持币的人 |
| Alpha | 内部消息/潜在机会，表示未公开但价值潜力大的信息 |
| Tokenomics | 代币经济学，研究代币发行、分配、用途等机制 |

# 四、社区与文化类

| 黑话 | 英文全称 | 含义 |
| gm / gn | good morning / good night | Web3 社区日常打招呼方式 |
| Anon | Anonymous | 匿名者，社区中不透露真实身份的成员 |
| KOL | Key Opinion Leader | 意见领袖，影响力人物 |
| CT | Crypto Twitter | 加密行业活跃的信息与讨论来源 |
| DAO | Decentralized Autonomous Organization | 去中心化自治组织，社区驱动型的组织管理形式 |
| NFT PFP | Non-Fungible Token Profile Picture | NFT 头像项目，如 CryptoPunks、BAYC，PFP = Profile Picture |

# 五、开发类

| 黑话 | 英文全称 | 含义 |
| Hardhat / Foundry | \ | 常用的智能合约开发框架 |
| RPC | Remote Procedure Call | 链上节点访问接口 |
| Gas | \ | 交易费用，以太坊中以 Gwei 计费 |
| Etherscan | \ | 区块浏览器，用于查看合约、交易、地址信息 |
| Viem / Ethers.js / Wagmi | \ | Web3 前端交互库 |
| ABI | Application Binary Interface | 合约调用所需的接口信息 |
| Mainnet / Testnet | \ | 主网 / 测试网，分别对应正式和测试环境 |
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




是什么，分类，对比，优势与挑战

# 一、区块链

## 定义

区块链=区块+链=（交易信息+之前的区块摘要）+顺序

## 特点

-   原有特性
    
    -   不可篡改（前后关联）
        
    -   公开透明、匿名（信息公开，但无身份）
        
    -   快速交易（不分地区和金额大小）
        
-   新特性：基于分布式网络
    
    -   去中心化、真正不可篡改（节点多、分布广泛）
        

# 二、比特币

## 存在原因

网络节点服务提供商的奖励（代币）

## 其他信息

-   货币属性：不会通胀、账户匿名、公开交易透明、不可篡改
    
-   价格决定：供需决定，无监管
    
-   交易公开透明、保护隐私、匿名：随机生成的钱包地址完成交易，但是与你关系未知
    
-   缺点
    
    -   黑产
        
    -   打包需要10分钟，不能实时
        

# 三、区块链的核心组成部分

## 核心

-   去中心化的网络
    
-   维持网络运行的代币激励：提供服务称为挖矿，维持服务的人是矿工，燃料费gas fee是工资，节点是矿机
    

## 运行

用户发起交易、交易广播、节点验证、打包成块、链接上链、奖励发放

# 四、公链、私链、联盟链

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=Mzg2ODVkNzM2MGY0MzM0Mjc0MzZjZWRiNDI0YzZmYzlfQjRDQ0R5VnVMQ2FXQ0o0aHdDNmZPNnd4Y2ZSa1pWODFfVG9rZW46QU5VUGIzMlVlb0RUSzN4NGRRc2NPVUpMbnpkXzE3NjgyMjcyOTY6MTc2ODIzMDg5Nl9WNA)

## 公链 = 公共花园

-   无需申请，自由进出
    
-   所有人可见，去中心化决策
    
-   决策效率低，维护成本高
    

## 联盟链 = 多公司联合的董事会

-   需要申请
    
-   权限分级
    
-   半公开数据，联合决策
    

## 私链 = 私人俱乐部

-   严格审批，固定成员
    
-   数据完全私有，老板说了算
    

| 区块链类型 | 节点加入方式 | 数据可见性 | 管理模式 | 适合场景 |
| 公链 | 任何人自由加入 | 所有人可见 | 去中心化（大家投票） | 加密货币、公共存证 |
| 联盟链 | 需联盟成员邀请/审批 | 仅联盟成员可见 | 多中心化（董事会决策） | 供应链、金融协作 |
| 私链 | 由老板严格审批 | 仅内部成员可见 | 中心化（老板说了算） | 企业内部管理、审计 |

# 五、Web3 vs Web3.0 vs Web2 的范式革命

## Web2 互联网

-   中心化控制：数据存储在巨头服务器
    
-   用户角色：生产者
    
-   商业模式：广告
    

## Web3.0 语义网

-   语义标记（RDF、OWL）：RDF = Resource Description Framework，OWL = Web Ontology Language
    
-   机器理解信息：每个事物都贴上结构化数据标签
    

## Web3 去中心化互联网

-   数据主权归用户，无需信任中介
    
-   优势为金融、产权、隐私场景，不适用高频社交
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NDJlMWFjZDI4YmFhM2FmZTI2NDdjYTEzODg0MGRiY2NfQ0VPTVhONVhXQVRqY3VraGIzVThOQjVPSkd5Z1hGa1BfVG9rZW46UG1SRGJIaUtDb0RmdWp4ZGF4MmNWSHpBbmJoXzE3NjgyMjcyOTY6MTc2ODIzMDg5Nl9WNA)

| 维度 | Web2 | Web 3.0 | Web3 |
| 控制权 | 平台垄断 | 部分开放 | 用户自治 |
| 数据存储 | 中心服务器 | 混合存储 | 区块链 / IPFS |
| 支付系统 | 信用卡 / 支付宝 | 集成支付 | 加密货币 |
| 典型技术 | JavaScript | RDF / OWL | 智能合约 |
| 代表企业 | 腾讯 / 阿里 | W3C / DBpedia | Uniswap / ConsenSys |

# 六、优势与挑战

## 优势

-   信任最小化，抗审查与高弹性，用户自主管理，开放创新生态
    

## 挑战

-   可扩展性瓶颈（公链），安全与治理难题，用户体验与成本，法律与合规风险
    

时间结构、空间结构、机制

# 一、以太坊的介绍

## 定义

以太坊（Ethereum）是一个**开源的去中心化区块链平台**，通过其**原生加密货币以太币**（Ether，简称 ETH）提供去中心化的**以太虚拟机**（EVM）来处理点对点合约。

## 创新

智能合约是存储在区块链上的可执行代码，能够在满足预设条件时自动执行操作，无需人工干预。代表了从数字货币到生态系统的基础设施的进步。

## 定位

“区块链 2.0”的代表，通过智能合约和可编程性，以太坊的目标是成为全球范围内的“世界计算机”

# 二、Ethereum和Bitcoin的差异

| 维度 | 比特币（Bitcoin） | 以太坊（Ethereum） |
| 目标与定位 | 去中心化的数字货币，强调安全、稳定和稀缺性（总量 2100 万枚） | 去中心化平台，支持智能合约和 Dapps，定位为“区块链 2.0” |
| 编程能力 | 脚本语言有限，仅支持简单的交易验证逻辑 | 图灵完备的编程语言（如 Solidity），可开发复杂智能合约 |
| 共识机制 | 工作量证明（PoW），矿工通过算力竞争记账权 | 从 PoW 转向权益证明（PoS），通过 The Merge 实现能源效率优化 |
| 交易速度 | 每 10 分钟生成一个区块，交易确认较慢 | 区块时间约 12 秒，交易确认更快，适合高频应用 |
| 经济模型 | 总量固定，强调抗通胀属性 | 供应灵活，通过 EIP-1559 等机制可能呈现通缩趋势 |

# 三、以太坊的定位与演进

-   以太坊是一个基于区块链的 去中心化计算平台 ，它的目标是成为“全球计算机”
    
-   关键节点
    
    -   The Merge（2022 年完成）：从高能耗 PoW → 低能耗 PoS
        
    -   EIP-4844（2024 年完成）：让 L2 更便宜，普通人用得起
        
    -   数据分片（2025-2026 年）：进一步降低 L2 成本，提升整体扩展性
        

## 以太坊1.0（PoW阶段）

-   PoW：proof of work，工作量证明
    
-   消耗算力、电力获得ETH奖励
    
-   能耗高，扩展性差
    

## 以太坊2.0与 The Merge: 从双链并行到完美合并

-   时间
    
    -   2020年12月，信标链试点PoS：proof of stake，权益证明
        
    -   2022年9月，The Merge 发生，合并后分为执行层（原主网）和共识层（信标链）
        
-   POS机制
    
    -   质押 32 ETH 成为验证者，系统随机选择验证者来提议和验证区块，验证者获得新发行的 ETH + 交易费用，作恶者质押的 ETH 被销毁（Slashing）
        
    -   能耗降低，攻击成本高，区块确认更快、更可靠
        
-   信标链
    
    -   管理验证者名单： 谁质押了 32 ETH？谁想进来？谁想退出？
        
    -   分配任务（随机性）： 这是它最核心的功能。为了防止作恶，信标链会随机选择一个验证者来提议下一个区块，并随机选择一小组验证者来给这个区块“投票签名”。这种随机性让黑客无法提前预测该攻击谁。
        
    -   执行奖励和惩罚： 谁干活认真，信标链就发 ETH 奖励；谁不在线或者想搞破坏，信标链就扣除（Slashed）他的质押金。
        

## 未来升级路线图

-   分片技术演进——从执行分片到数据分片
    
-   EIP-4844（Cancun 升级）——“省钱神器”
    
-   ZK-Rollup 技术——“批量验证，一步到位”
    

# 四、以太坊生态概览

## Layer 1

-   以太坊主网：核心区块链，负责最终安全性与共识。
    
-   EVM：以太坊虚拟机，执行智能合约代码。
    
-   账户系统：外部账户（EOA）与合约账户（CA）共同构成网络基础。
    

## Layer 2

-   Rollup：通过将交易批量处理后提交至 L1，降低 Gas 费。
    
    -   Optimistic Rollup：假设交易合法，仅在争议时验证。
        
    -   ZK Rollup：通过零知识证明验证交易，无需链上争议。
        

## 侧链

-   独立运行的链，通过桥接与主网交互。
    

## 以太坊生态分层架构

-   应用层（Application Layer）：用户直接交互的应用和界面（DeFi 应用、NFT 平台、钱包应用、DAO 工具）
    
-   协议层（Protocol Layer）：以太坊的核心基础设施（共识层客户端、执行层客户端、核心协议）
    
-   扩展层（Scaling Layer）：提升性能和降低成本的解决方案（Layer 2 Rollups、侧链、状态通道）
    

# 五、以太坊核心机制：从账户到执行的完整链路

## 账户系统：你的数字身份

-   外部账户EOA（Externally Owned Account）：私钥+公钥，私钥控制，公钥为分享地址
    
-   合约账户CA（Contract Account）：代码控制，由EOA触发，生成CA地址
    
-   结构：
    
    -   Nonce：防止重复交易的计数器（EOA 记录发送次数，CA 记录创建合约次数）。
        
    -   余额：账户持有的 ETH 数量（单位为 Wei）。
        
    -   CodeHash：EOA 为空哈希，CA 存储合约字节码的哈希值。
        
    -   StorageRoot：记录账户数据的 Merkle 树根哈希（如 NFT 归属关系）。
        

## Gas 模型：交易的燃料费

-   EOA交易支付燃料费
    
-   总费用 = Gas Limit × Gas Price = 限额 × 单价（未到限额则返还）
    
-   单价 = 基础费用 + 小费
    

## 以太坊虚拟机（EVM）：代码的执行引擎

-   EVM（Ethereum Virtual Machine）是 以太坊的“大脑”，是专门用来运行智能合约的虚拟计算机。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
