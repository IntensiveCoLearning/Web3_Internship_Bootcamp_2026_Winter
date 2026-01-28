---
timezone: UTC+8
---

# Tianyu Li

**GitHub ID:** 0xBrick-Li

**Telegram:** @Brick0v0

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->
# To do list

-   kite ai 研究
    
-   个人简历任务一定要完成
    
-   想想黑客松选题
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->

# To do list

-   学习如何制作简历
    
-   完成简历和职业规划的任务
    
-   学习使用Kite Ai
    
-   收听hauhua和yoona的space
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->


# To do list

-   [Challenge #2 - token vendor](https://speedrunethereum.com/challenge/token-vendor)
    
-   spoonOS 完成一半
    
-   学习 Web3 简历如何制作/改简历 没完成
    
-   思考了黑客松题目
    

打黑客松一定不要放弃，哪怕交一个视频上去，完整的将自己的想法呈现出来，而不是做一个功能完备的项目

流程的完整比功能完整更重要，一定要让别人明白你在做什么

念稿子直接ai生成ttsmaker

比赛框架 next.js  scaffold-ETH
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->




# To do list

-   推特上发布学习总结 ✅
    
-   排版文章 ❌
    
-   深度技术任务 （2/3）
    

Note

学习过程中跟着教程做，一步一步来，不要逃课！
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->






# To do list

-   注册figma ✅
    
-   深度技术剩余的3个任务
    
-   参加 space ✅
    

总结  
今天完成了figma优化任务  
参加了space发言，听到了bruce老师的小灶  
参加了LXDAO周会  
  
明天目标是 撰写文章 / 优化简历 / 完成深度技术剩余任务
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->








# To do list

-   figma
    
-   推特会员开通
    
-   spoonOS 研究
    
-   阅读 uniswap 源码
    

# Note

Austin 分享 学习自己写代码过程中尽量关掉AI

### 看了Draken老师推特上关于自己心路历程的文章，Draken老师真的很有勇气和执行力。  
回顾一下我的时间线

2025.7 我在公司自学Web3的时候看到了第一期实习计划的帖子，但是我真的太忙了，主业在 Web2 中厂做后端实习，副业在做电商，每天回到家还要拆退货包裹，完全没有自己的生活。当时在6月份报了一个openbuild的 web3 数据分析训练营，7月的时候这个数据分析我一点都没空学，我想着这个训练营都没空学，何必再报一个呢，于是我就这样错过了第一期训练营的报名。

在公司实习的时候，我一边做着 Web2 的工作，一边抽空学习 Web3 的知识。那时我虽然错过了报名，但是我偶尔会跟着第一期实习计划“偷学”，在B站上看分享会的回放、看 Web3 实习手册、黑暗森林手册，（差不多能算第一期的permissionless学员？）  
  
先写到这，等会儿再写。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->









# To do list

-   Uniswap V2 实操
    
-   Polymarket架构与链上数据解码
    
-   Reactive Network
    

# Note

### 重入攻击 (Reentrancy)

```
function withdraw() public notCompleted{
        if (openToWithdraw == false){
            revert NotOpenToWithdraw();
        }
        uint256 refund_amount = balances[msg.sender];
        balances[msg.sender] = 0;//现改状态
        (bool success, ) = msg.sender.call{value : refund_amount}("");//转账
        if (success == false){
            revert WithdrawTransferFailed(msg.sender, refund_amount);
        }
        openToWithdraw == false;
    }
```

如果先转账，再扣余额，黑客在收到钱的fallback里再次调用提款函数，此时余额还没扣，导致合约被掏空。

### gas优化

不变的变量声明为immutable，直接嵌入合约字节码中，读取几乎不花钱（类似常量）。

```
uint256 public  deadline = block.timestamp + 2 hours ;//deadline部署后不变
//用immutable优化，嵌入合约字节码，读取几乎不消耗gas
uint256 public immutable deadline = block.timestamp + 2 hours ;
```
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->











# To do list

-   Ethernaut 前 3 关 ✅
    
-   remix 中运行 [Solidity by Example | 0.8.26](https://solidity-by-example.org/) Basic 部分的代码
    
-   Uniswap V2 实操 部分完成
    
-   复习 solidity ✅
    
-   今天你发推特了吗
    
-   阅读源码/写代码 ✅
    

# Note

香港共识大会学生pass申请成功，期待第一次跑会

## Uniswap

Tick 是价格的“离散化坐标”

P(i)=(1.0001)^i i:Tick 的索引

## 合约部署/验证

合约部署（Deploy）= 把字节码写入区块链，让合约“存在并可执行”

-   看不到源码
    
-   看不到函数签名（ABI）  
      
    合约验证（Verify）= 向区块浏览器证明：这个字节码是由哪份源码编译出来的
    
-   可以看到源码
    
-   可被第三方平台解析
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->













# To do list

-   Uniswap v2 源码
    
-   solidity 快速复习
    
-   重新制作 web3 简历
    
-   学习运营、投研
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->














# To do list

-   **完成“链上留言板”智能合约开发 ✅**
    
-   部署合约到 Sepolia 测试网 **✅**
    
-   完成一个简单 DApp（含前端） **✅**
    
-   搭建本地区块链节点 **✅**
    
-   **挑战：Crowdfunding ✅**
    
-   **Uniswap V2 实操**
    

# Note

```
转账标准写法
(bool success, bytes memory data) = payable(to).call{value: amount}("");

(bool success, ) = msg.sender.call{value : refund_amount}("");
```

solidity改复习一下子了，明天抓紧复习，争取优化实习手册
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->
















# To do list

-   今天你commit了吗 ✅
    
-   今天你发推了吗 ✅
    
-   Go
    
-   Uniswap ✅
    
-   solidity ✅
    
-   Challenge1
    
-   Solana Task1
    

昨天看到Bruce老师分享的notebooklm，今天就用了一下，真的是震惊我一整年，导入URL/文件/YouTube视频就能直接生成思维导图、图片、报告、幻灯片、甚至是播客内容。以后我的笔记靠它了，notebooklm在我这里上升到和Claude Code、Genimi一个地位，今天鱼头必须朝着notebooklm，notebooklm，你是我滴神

# Note

重新回顾了下公私钥加密的密码学原理和关系：地址address是 keccak256(public key) 的后 20 bytes

ERC7962中的 keyHash = keccak256(public key) 地址为 keyHash 的后 20 bytes

# ERC-7962入门指南：解密基于密钥哈希的代币新标准

## 1\. 引言：为什么我们需要一种新的代币标准？

传统的代币标准，如我们熟知的 `ERC-20` (同质化代币) 和 `ERC-721` (非同质化代币，即NFT)，存在一个核心痛点：它们将代币的所有权直接与一个公开的以太坊地址绑定。这意味着，任何人都可以通过区块链浏览器追踪一个地址的所有资产和交易历史，这无疑会暴露用户的财务活动，损害个人隐私。

为了应对这一挑战，**ERC-7962** 标准应运而生。它是一种创新的解决方案，旨在通过一种名为“密钥哈希”（keyHash）的新机制，从根本上改变代币所有权的记录方式，从而为用户提供更强的隐私保护。

## 2\. 核心思想：从“地址所有”到“密钥哈希所有”

ERC-7962最核心的变革，就是不再使用公开的 `地址 (address)` 来记录谁拥有代币，而是使用一个全新的标识符——`密钥哈希 (keyHash)`。

`密钥哈希` 是如何产生的呢？它非常简单直接：

-   用户首先生成一对密钥：一个私钥（自己保管）和一个公钥 (`key`)。根据标准，这个`key`必须是一个带有`0x04`前缀的、65字节长度的未压缩secp256k1公钥。
    
-   然后，通过对这个公钥进行 `keccak256` 哈希计算，就得到了独一无二的 `keyHash`。
    
    -   **计算公式**: `keyHash = keccak256(key)`
        

这个 `keyHash` 只是一个32字节的哈希值，它本身并不直接暴露用户的公钥或地址。智能合约在链上只记录这个 `keyHash` 作为所有权的凭证。

为了更清晰地理解这一变革，我们可以通过下表对比一下传统标准与ERC-7962的根本区别：

| 特性 | 传统标准 (ERC-20 / ERC-721) | ERC-7962 标准 |
| 所有权标识 | 以太坊地址 (例如 0xAb...) | 密钥哈希 (一个32字节的哈希值) |
| 链上记录 | ownerOf(tokenId) 返回一个地址 | ownerOf(tokenId) 返回一个密钥哈希 |
| 隐私性 | 较低，地址易于追踪和关联 | 较高，哈希值不直接暴露身份 |
| 验证方式 | 交易发起者必须是地址的所有者 | 交易发起者提供有效签名即可 |

理解了“密钥哈希”这个新概念后，接下来让我们通过一次具体的转账过程，看看它是如何在实际操作中保护我们的隐私并带来更多灵活性的。

## 3\. ERC-7962如何运作：一次转账的幕后故事

想象一下，Alice 想要将她拥有的一个ERC-7962代币转给 Bob。整个过程可以分为以下四个步骤：

1.  **准备阶段（所有者）**
    
    -   Alice（代币所有者）使用她的**私钥**，对包含转账核心信息（如接收方 `toKeyHash`、代币ID `tokenId`、防止重放攻击的 `nonce` 以及签名有效期 `deadline`）的数据进行签名。
        
    -   这个过程会生成一个独一无二的数字\*\*`签名 (signature)`\*\*，这相当于Alice对这笔转账的授权书。
        
    -   **关键点**：在这个阶段，Alice _不需要_ 亲自向以太坊网络提交交易，也_不需要_ 支付任何Gas费。
        
2.  **提交阶段（任何人）**
    
    -   现在，任何人（可以是一个服务提供商、一个DApp后端，或者任何愿意支付Gas费的第三方，我们称之为“中继者”或“Relayer”）都可以获取到Alice的\*\*`签名 (signature)`\*\* 和她的\*\*`公钥 (key)`\*\*。
        
    -   中继者调用智能合约的 `transfer` 函数，将这些信息提交上链，并为这笔交易支付所需的Gas费。
        
3.  **验证阶段（智能合约）**
    
    -   智能合约收到请求后，会像一个严谨的银行柜员一样，执行一系列关键的验证步骤来确保交易的安全与合法性：
        
        -   **验证所有权**: 合约会先计算中继者提交的 `公钥 (key)` 的哈希值 (`keccak256(key)`)，然后检查计算结果是否与链上记录的该代币当前的 `keyHash` **完全匹配**。这一步确认了发起授权的人确实是代币的当前所有者。
            
        -   **验证签名与防篡改**: 接着，合约会使用 **EIP-712标准** 来验证签名的有效性。这意味着签名是针对一个结构化的、清晰易读的数据包，其中包含了所有关键交易参数（如`tokenId`, `toKeyHash`, `nonce`, `deadline`等）。这可以有效防止签名被恶意用于其他意图或被篡改。
            
        -   **防止重放攻击**: 合约还会检查签名中的 `nonce` (一个只能使用一次的数字)。每个`keyHash`都有一个关联的`nonce`计数器，成功交易后会加一，确保同一个签名不会被重复提交。同时，`deadline` 参数设定了签名的过期时间，防止过时的签名被恶意使用。
            
4.  **完成阶段（所有权变更）**
    
    -   一旦以上所有验证全部通过，智能合约就会更新链上记录，将该代币的所有权从Alice的旧`keyHash`变更为Bob的新`keyHash`。转账完成！
        

这个巧妙的流程设计，自然而然地引出了ERC-7962标准最吸引人的两大核心优势。

## 4\. ERC-7962的两大核心优势

### 4.1 优势一：增强用户隐私

由于链上只记录 `keyHash`，而不是一个固定的、反复使用的以太坊地址，用户可以为每一笔交易或持有的每一个代币都生成并使用一对全新的密钥。

我们可以用一个简单的比喻来理解：

这就像每次网上购物都使用一个一次性的、匿名的邮政信箱来接收包裹，而不是你永久的家庭住址。这样一来，外界就无法将你的多次购物行为关联起来，从而保护了你的消费隐私。

同样，ERC-7962的这种“一次一密钥”模式，大大降低了用户的交易活动被追踪和关联的风险，为实现链上匿名交易和资产持有提供了坚实的基础。

值得注意的是，该标准存在一个隐私上的细微之处：虽然链上只记录匿名的`keyHash`，但在转账发生时，完整的公钥`key`会在交易数据中被公开。正如源文档所建议的，为了保持最大程度的隐私，用户应始终将代币（或剩余余额）转移到一个_全新的_`keyHash`，而不是重复使用旧的密钥。

### 4.2 优势二：实现第三方支付燃料费（Gas）

还记得转账流程的第2步吗？“提交交易的人”和“代币的所有者”可以是完全不同的人。

这种“所有权与交易发起权相分离”的设计，使得第三方为用户代付Gas费变得轻而易举。这在许多场景下都极具价值：

-   **改善用户体验**: 新用户刚进入一个去中心化应用（DApp）时，钱包里可能没有ETH来支付Gas费。项目方可以作为“中继者”为用户的首次操作代付Gas，极大地降低了新用户的入门门槛。
    
-   **批量交易与成本优化**: 服务商可以收集多个用户的有效签名，然后将这些交易打包在一起，一次性提交给网络。这种方式不仅方便，还可能通过优化Gas使用来节省总成本。
    

为了满足不同代币类型的需求，ERC-7962标准并非单一接口，而是提供了一个包含多种实现的代币家族。

## 5\. ERC-7962代币家族简介

ERC-7962提案中主要包含了两种核心的代币接口，分别对应NFT和同质化代币的场景：

-   `ERC-KeyHash721`
    
    -   **用途**: 用于非同质化代币（NFTs）。
        
    -   **关键点**: 在这个标准下，每一个独一无二的代币（由`tokenId`标识）的所有权，都与一个独立的`keyHash`相关联。
        
-   `ERC-KeyHash20`
    
    -   **用途**: 用于同质化代币（类似于ERC-20）。
        
    -   **关键点**: 在这个标准下，账户的余额与`keyHash`关联。其转账设计非常独特，**模仿了比特币的UTXO模型**：当一笔转账发生后，支付后剩余的余额会被发送到一个全新的`leftKeyHash`，而不是留在原来的`keyHash`里。标准强制执行一种“严格模式”，即`leftKeyHash`**绝不能**与`fromKeyHash`（发送方）或`toKeyHash`（接收方）相同。这种强制性的密钥轮换设计，是防止交易关联、最大化隐私的关键。
        

## 6\. 总结：重新思考链上所有权

ERC-7962的核心创新在于，它通过引入\*\*`密钥哈希 (keyHash)`\*\* 的概念，成功地将代币的所有权与公开、固定的以太坊地址解耦。

这一看似简单的改变，却为用户带来了两大核心价值：**前所未有的隐私保护**和**极其灵活的交易执行方式**。通过借鉴并扩展ERC-5564（隐身地址）等标准中的隐私原则，ERC-7962不仅增强了代币本身的隐私性，更为一个集成了私密链上交互和去中心化身份的生态系统铺平了道路。

## 问题：

1.  允许第三方支付gas，那如果有人故意制造大量交易但不自己支付gas，则造成资源浪费？
    
2.  优化 gas 的时候服务商打包提交，在交易数量过少时是否会导致延迟过高？
    
3.  gas fee的计算和什么相关？
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->


















# To do list

-   参加LXDAO周会 ✅
    
-   今天你Commit了吗 ✅
    
-   今天你发推了吗 ✅
    
-   学习Go
    
-   学习Uniswap
    
-   学习Solidity ✅
    
-   完成Speed Run Etherum Challenge0 ✅
    

# Note

LXDAO周会Random Talk环节Bruce老师答疑：

-   初级程序员在学习过程中的代码自己写，熟练以后AI辅助
    
-   英语非常重要，不会英语岗位减少一半
    

已部署Speed Run Etherum Challenge0至vercel，欢迎大家来挖自己的NFT  
[https://challenge-tokenization-nextjs-five.vercel.app](https://challenge-tokenization-nextjs-five.vercel.app/myNFTs)

这个小挑战真的很好，将合约、前端、本地部署、测试网部署都跑了一遍，对小白理解dapp的项目结构和部署流程非常友好
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->




















# To do list

-   今天你commit了吗？
    
-   Go日常学习
    
-   021以太坊 ✅
    
-   Web3实习手册 ✅
    
-   一定要参加周会！学习别人怎么写笔记的 ✅
    
-   Unphishable完成3章 ✅
    
-   Speed Run Ethereum Challenge 0
    

# Note

周会真的浓度太高，被狠狠的卷到了，分享的同学不是卷王就是大佬。天呐，残酷共学残酷的不是学习强度，而是同学们。  
我今天听的都是什么分享啊  
多的不说，周末狠狠恶补。

一个督促自己开始任务的话：拉屎的是10%，剩下的90%还在便秘呢

今天立flag在这里

-   我要早起、自律；
    
-   每天commit，每天阅读源码
    
-   每天发推
    

从今往后，开始每天在推特和github上拉一坨

话又说回来，这么多及其优秀的同学想进入Web3，说明Web3有一个美好的未来。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->























# To do list

-   推特持续关注行业名单 ✅
    
-   021以太坊学习第四章
    
-   阅读uniswap
    
-   阅读Web3实习手册 ✅
    
-   今天你Commit了吗？✅
    
-   制定职业规划，完成职业规划任务 ✅
    
-   Unphishable完成至少5章 ✅
    

# 职业规划与学习计划

**目标求职意向**

Web3后端远程实习  Go + Solidity + 以太坊 L2

**4周后达成目标：**

-   Go工程能力
    
-   Web3 实战经验
    
-   可展示项目
    

**Week 1  Go 工程基础**

-   gin 框架
    
-   REST API
    
-   Go完成简单的后端项目
    

**Week 2  Solidity + 链交互**

-   solidity中级
    
-   学会后端与链交互
    

**Week 3：Go Indexer**

-   go-ethereum
    
-   订阅区块 / event
    
-   ABI 解码
    

**Week 4：面试准备**

-   README 优化
    
-   简历打磨
    
-   面试准备
    

Web3知识量大，迭代快，多了嚼不烂，专注一个方向更容易成功。

学一门好，多了嚼不烂。 -----Jintol老师
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->


























# To do list

-   推特发学习总结 ✅
    
-   021以太坊阅读 ✅
    
-   实习手册阅读 ✅
    
-   Unphishable至少5章
    
-   安装欺诈检测插件
    
-   今天你Commit了吗：Arbireum Rust Task4 ✅
    
-   Go 工程项
    
-   参会 ✅
    

# Note

### CREATE 合约地址不可预测性

1.  CREATE 地址计算公式  
    公式：address = keccak256(rlp(\[sender, nonce\]))\[12:\]  
    sender：部署合约的账户地址（EOA 或合约）  
    nonce：部署时账户当前的 nonce  
    rlp(…)：以太坊递归长度前缀编码  
    keccak256：哈希结果256 bits(32 bytes)  
    \[12:\]：取后 20 bytes 作为地址
    
2.  nonce 定义  
    链上 nonce：账户已执行交易数量，所有节点最终一致  
    pending nonce：节点根据链上 nonce + 本地 mempool 中的未确认交易数推算  
    每个节点不同，不参与共识，只是本地估算值
    
3.  内存池（Mempool）  
    每个节点本地缓存尚未打包进区块的交易  
    每个节点独立维护，节点间不一致  
    生命周期：发送交易 → 节点收到 → 加入 mempool → 广播 → 矿工打包 → 上链 → 从 mempool 删除
    

### CREATE 地址不可预测的核心原因

查询的 pending nonce ≠ 出块节点实际使用的 nonce

-   节点视图不一致：不同节点 mempool 中交易数量不同，不同节点看到的 pending nonce 不一致
    
-   交易替换：钱包自动加价重发交易
    
-   交易乱序传播：节点先收到后续 nonce 交易
    
-   区块重组（Reorg）：区块回滚导致 nonce 回退
    

### CREATE2 的可预测性

-   地址公式：keccak256(0xff ++ deployer ++ salt ++ keccak256(init\_code))
    
-   只依赖 deployer、salt、init\_code，不依赖 nonce 或 mempool
    
-   因此在任何节点、时间、环境下可稳定预测
    

### 总结

-   CREATE 不可预测性是分布式系统一致性问题，不是密码学问题
    
-   核心链路：nonce ∈ 分布式局部状态 → 局部视图不一致 → 工程不可预测
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->





























# To do list

-   学习alloy ✅
    
-   以太坊回放 ✅
    
-   Web3实习手册提交PR ❌
    
-   Unphishable继续完成 ✅
    
-   学习Web3实习手册 ✅
    
-   021学习以太坊 ✅
    
-   推特继续关注账号 ✅
    

# Note

## EVM

由分布在全世界的“以太坊**全节点**（Full Nodes）”来执行，每一个全节点都会运行一个以太坊客户端软件。

-   **内置引擎**：客户端软件内部都集成了 EVM 的实现代码。
    
-   **人人参与，人人计算**：为了验证交易是否合法，全网每一个正在工作的全节点都会在自己的 CPU 上，通过自己本地的 EVM 副本，把代码跑一遍。
    

**确定性（Deterministic）**： EVM **最核心**的特质。同样的初始状态 + 同样的输入指令，在全世界任何一台电脑的 EVM 上运行，结果百分之百相同。

### EVM如何实现全网同步？（核心难点）

**关键机制：通过“共识机制”与“状态根（State Root）”同步。**

1.  **打包与广播**： 某个验证者（Validator）会将一批交易打包成一个区块。在打包过程中，它会在本地 EVM 里运行这些交易，计算出执行后的“新世界状态”。
    
2.  **生成“摘要”**： 这个验证者并不会把整个巨大的数据库发给别人，而是将所有账户的新余额、新合约数据通过一种叫 **Merkle Patricia Tree** 的算法压缩成一个很短的哈希值，叫做 **State Root（状态根）**，放在区块头里广播出去。
    
3.  **全网验证（重放交易）**： 其他节点收到这个区块后，不会盲目相信。它们会把区块里的交易拿出来，在**自己本地**的 EVM 里按顺序再跑一遍（重放）。
    
4.  **比对结果**： 如果其他节点跑完后，算出的本地“状态根”与区块头里的“状态根”**完全一致**，说明这笔账没问题，大家就同步更新本地的数据库；如果不一致，说明发送者造假，该区块会被全网拒绝。
    

### 区块链扩展性难题（The Scalability Trilemma）

既然每一台节点都要重新运行一遍交易来验证，这意味着以太坊的计算速度取决于**其中单台最慢节点的计算能力**

解决方案：执行与结算的分离（Modular Blockchain），在 L2 执行，在 L1 结算。

-   Optimistic Rollups（乐观汇总，如 Optimism, Arbitrum）  
    默认交易都正 确，只有在有人质疑时才通过“欺诈证明（fraud proof）”回滚
    
-   ZK-Rollups（零知识证明汇总，如 ZKsync, Starknet）
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->































# To do list

## Tasks

-   创建钱包，并尝试测试币转账 ✅
    
-   完成 [Unphishable](https://unphishable.io/) 钓鱼攻防挑战 ✅ 已完成初级部分
    
-   在 [My First NFT](https://nft.myfirst.io/) mint 第一个 NFT ✅
    
-   线上 Co-Learning ✅
    
-   阅读Web3实习手册入门导读部分+总结笔记 ✅
    
-   以太坊中文周会 ❌
    

# Note

Web3实习手册-入门导读  

**Web2 vs Web 3.0 vs Web3**

-   **Web2**（当前主流互联网）：平台拥有数据，用户贡献内容
    
-   **Web 3.0**（语义网）：让机器更好“理解”数据（语义层升级） 是传统互联网的数据组织升级
    
-   **Web3**（去中心化互联网）：用户真正拥有数据与资产（权属层升级）
    

Web3挑战：扩展性、安全治理、用户体验、合规  

### 零知识证明

1\. 算术化 (Arithmetication) 将计算机程序或逻辑断言“翻译”成数学语言。

-   拍平 (Flattening)：将复杂的代码分解成简单的算式。
    
-   R1CS (Rank-1 Constraint System)：将算式转化成一组向量约束，程序变成互相关联的方程。
    

2\. 多项式转换 (Polynomial Mapping)

证明者不会直接发送成千上万个方程式的解，而是将这些约束编码进**多项式**中。

**原理**：如果两个多项式在某个随机点上的取值相同，那么它们在所有点上几乎肯定完全相同。这使得证明者只需要证明“我的多项式在某一点是正确的”，就能代表整个逻辑是正确的。

3\. 非交互式（Non-Interactive）

-   **原理**：证明者不再等验证者给随机数，而是利用多项式的哈希值（Hash）自己生成一个“伪随机数”作为挑战。
    
-   **结果**：证明者生成一个极其简短的“证明文件”，直接发到链上。任何人（包括以太坊智能合约）随时看到这个文件，都能独立验证其真实性，且不需要与证明者进行任何沟通。
    

## 会议：Web3行业介绍

2025年新增岗位：6.6万个，增长47%

结构性缺口严重：初级运营供过于求，懂Rust语言的工程师和合规人才稀缺  
AI影响：**70%面试**考察AI工具使用能力

链上简历比传统pdf简历更重要，HR会重点看链上的经历， **GitHub代码记录**、DAO投票记录、Mirror文章

## 目标和计划

1.  坚持每月至少有20天Commit代码，多提交PR
    
2.  了解DAO的运行模式，加入DAO
    
3.  了解如何Mirror文章
    
4.  除了Vibe Coding之外，掌握AI在其他工作场景的应用（会议记录、推文、群聊管理）
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
