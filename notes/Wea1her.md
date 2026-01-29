---
timezone: UTC+8
---

# Weather

**GitHub ID:** Wea1her

**Telegram:** @jaycupup

## Self-introduction

Web3 实习计划 2026 冬季实习生 | INTJ | 学习技术&运营
INTJ | 黑客松准备中
INTJ

## Notes

<!-- Content_START -->
# 2026-01-29
<!-- DAILY_CHECKIN_2026-01-29_START -->
黑客松Day1,脑暴中....
<!-- DAILY_CHECKIN_2026-01-29_END -->

# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->

# 预测市场的展望：

1.人机共生 ai agent做市 解决长尾流动性

2\. 万物皆可预测 接入LLM+乐观预言机(拍照由LLM识别并验证)，降低结算成本

3.隐形嵌入式服务，比如扩展插件，让用户随时可预测
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->


1.  学习**OGBC-Intern-Project**
    
2.  学习Foundry
    
3.  vibe一个链上留言板
    
4.  学习uniswap V2
    

留言板Dapp开发完成，在vibe的过程中发现自己总是遗漏很多必须应该告诉AI的细节，导致时间的浪费，总之吸取教训。
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->






# 今日任务：

1.  参加晚上的Vibe Coding + 黑客松经验分享
    
2.  完成新的入门技术任务
    
3.  完成深度技术中的uniswap
    

draken老师的教程完全跟下来发现不能跑通，于是去了hardhat和foundary的官方文档跑通流程
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->








# 听了space在web3工作，英语有多重要?

### 1\. 信息的“时差”优势

Web3 是一个极其依赖\*\*前沿叙事（Narratives）\*\*的行业。

-   **第一手资料：** 几乎所有核心协议的白皮书、EIP 提案、技术文档（如 Solidity, Rust, Zero-Knowledge）都是以英文首发。
    
-   **资讯速度：** 当一个重大的安全漏洞或市场动态在 X (Twitter) 上引爆时，英文圈的反应通常比中文社区快数小时甚至数天。在 Web3，**信息差就是金钱**。
    

### 2\. 工作环境的“去中心化”特质

Web3 公司大多采用 **Remote（远程办公）** 模式，团队成员可能分布在纽约、柏林、新加坡和首尔。

-   **沟通媒介：** Discord, Slack 和 Telegram 是核心办公软件。无论你是开发者还是市场人员，都需要用英文撰写文档、记录 Issue 或在会议中表达观点。
    
-   **协作效率：** 英语不流利会极大增加沟通成本，导致你在分布式协作中被边缘化。
    

3.允许自己说不太完整的句子。
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->










# 踏浪者行：

听了Serena的分享，对第一次参加黑客松的同学给出了三点建议：

1.  组建性格相同的团队+分工协作
    
2.  先实现MVP(让裁判眼前一亮)
    
3.  先确定想法再去开发 防止中途废弃
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->











今天学习了Dapp架构，了解了**Scaffold-ETH 2** 脚手架，打算使用它来进行DAPP开发。

更新了昨天和前天的笔记到个人博客(这两天有点懒，该恢复效率了)

# 今日目标：

1.  参加下午co-Learning
    
2.  参加晚上的例会，听一下大家的学习成果
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->












# 今日任务：

1.  完成“链上留言板”的开发
    
2.  准备休闲黑客松，实现一个demo
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->













# Solidity Walk ThroughQ&A：

## **一、在推特等社交平台看到有人故意暴露钱包助记词，导入后发现：**

**1\. 账户有余额但无法转出（提示需要 gas）**

**2\. 转入 gas 后，gas 立即被转走**

**3\. 这是一种什么实现逻辑？是抢跑吗？**

1\. 合约层权限限制

\- 账户类型：暴露的地址可能是合约账户（非普通 EOA 钱包）

\- 权限控制：通过 modifier 修饰符限制转账权限

\- 表面余额假象：账户显示有余额，但转账方法被 onlyRealOwner 修饰，只有预

设的 realOwnerAddress 才能转出资金

\- 诱导行为：骗子通过公开助记词诱导用户转入 gas，但用户永远无法转出原始

余额

2\. 抢跑机器人（Front-running Bot）

\- 监听机制：Bot 监听特定地址的所有交易事件（Event）

\- 触发条件：一旦检测到该地址有 gas 转入

\- 执行策略：

1\. 立即以更高的 gas fee 发起转账交易

2\. 利用矿工优先打包高 gas 交易机制，抢先转走资金

3\. 可配置自动化：转入→转走→再转入（循环诱骗）

\- 技术基础：依赖以太坊交易排序机制，非合约逻辑缺陷安全建议

\- 绝不导入来历不明的助记词/私钥

\- 测试小额：如需测试，先转入极小金额验证

\- 查看合约：通过区块浏览器检查地址是否为合约账户

## **二、modifier 是什么？具体操作逻辑是怎样的？**

1\. 基本定义

\- 功能定位：Solidity 中的函数修饰符，用于封装前置条件检查逻辑

\- 核心价值：代码复用、权限统一、Gas 优化

2\. 与传统 require 对比

方式

代码示例

缺点

**每个函数单独**

**require**

每个函数都写 require(msg.sender ==

owner, "...")

代码重复、部署 Gas 高、

维护困难

**使用 modifier**

函数后加 onlyOwner 修饰符

一次定义，多处复用，

Gas 更优

3\. 扩展应用场景

\- 重入保护：nonReentrant 修饰符防止重入攻击

\- 时间锁：onlyAfter(block.timestamp)限制执行时间

\- 角色权限：onlyAdmin、onlyMinter 等多角色控制

\- 参数校验：validAmount(amount)统一校验参数有效性

4\. Gas 优化原理

\- 部署时：modifier 逻辑只编译一次，减少合约字节码大小

\- 执行时：跳转到统一校验代码，比分散 require 更高效

\- 开发时：减少代码冗余，降低出错概率

## 三、

**问题 1：发币是否需要多个合约？**

\- 每个 ERC20 代币都是独立合约：每个代币项目都需要单独部署自己的 ERC20

合约

\- 链上数据不可变性：

\- 部署时设定的参数（名称、符号、精度等）永久记录在链上

\- 除非合约内预设了修改方法，否则这些参数永远无法更改

\- 示例：部署时设置 name="LXDAO"，该代币将永远叫这个名字

\- 实际意义：要发行自己的代币，必须编写并部署独立的 ERC20 合约

**问题 2：EVM 是否存在并发问题？**

\- 无传统并发：EVM 是单线程顺序执行，不存在多线程并发问题- 但有重入风险：

\- 防护措施：

1\. 检查-生效-交互模式：先更新状态，再执行外部调用

2\. 重入锁：使用 modifier 实现

四、

## **问题 1：新代币标准是否兼容 ERC20？**

标准独立性

\- ERC-721：NFT 标准，每个 token 唯一

\- ERC-1155：多代币标准，可同时包含同质化和非同质化代币

\- 各自独立：每个标准是独立的接口规范，不存在“兼容”关系

为什么需要标准？

1\. 钱包支持：钱包应用需要统一的接口来显示余额、转账

2\. 交易所集成：交易所需要标准方法进行充提币

3\. DApp 交互：DeFi 协议需要标准接口进行代币操作

非标准代币的问题

\- 如果代币 A 的转账方法叫 sendToken，代币 B 叫 transferFunds

\- 每个 DApp 都需要为每个非标代币编写特殊逻辑

\- 钱包无法统一显示和管理

**问题 2：标准能否升级？接口是否可变？**

接口稳定性原则

\- 接口不变：ERC20 标准的方法签名和参数不可更改

\- 实现可变：合约内部逻辑可以自由实现和优化

为什么接口不能变？

1\. 向后兼容：已有数千个 ERC20 代币和数百个集成应用

2\. 生态依赖：钱包、交易所、DApp 都依赖固定接口

3\. 升级成本：改变标准会导致整个生态需要更新

如何扩展功能？

\- 新标准：创建新的 ERC 标准（如 ERC-777、ERC-1363）

\- 封装模式：在标准接口基础上添加额外功能

\- 代理模式：通过可升级合约添加新方法

## **五、除了标准，还有哪些“轮子”可用？**

1\. OpenZeppelin 合约库

\- 定位：经过审计的标准合约实现库

\- 优势：

\- 免去重复编写标准逻辑

\- 经过多次安全审计- 持续维护更新

2\. 可升级合约模式

问题背景

\- 传统合约部署后无法修改

\- 发现 bug 或需要升级时，必须部署新合约并迁移用户

升级流程

1\. 部署新的 Logic 合约（V2）

2\. 更新 Proxy 中的逻辑合约地址指向 V2

3\. 用户继续使用原 Proxy 地址，自动获得新功能

3\. 钻石架构（EIP-2535）

\- 多逻辑合约：一个代理可路由到多个实现合约

\- 模块化升级：不同功能模块独立升级

\- Gas 优化：避免重复部署存储合约
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->














# 在复习WTF-Solidity中遇到的Q&A

## 1.库合约为什么不能被继承？

1.  **设计目的不同**：
    
    -   库合约是为**代码复用**而设计的（如工具函数），通过 `DELEGATECALL` 调用，节省 gas。
        
    -   合约继承用于**扩展逻辑**，而库合约不需要被扩展。
        
2.  **编译器限制**：
    
    -   Solidity 编译器明确禁止库合约被继承，以避免误用（如状态变量冲突）。
        
    -   库合约不能有状态变量，而继承可能引入状态变量，导致逻辑混乱。
        
3.  **安全性**：
    
    -   库合约的 `DELEGATECALL` 机制依赖于调用者的存储布局。继承会破坏这种假设，可能导致存储冲突或安全漏洞。
        

明天手搓DID
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->
















# Gas优化：

gas优化，就是尽可能的不与存储在storage上的变量进行操作，并尽可能使用memory来操作变量，

例如：

浪费gas的写法（在循环中多次 SLOAD/SSTORE）：

````
```solidity
uint256 public totalReward;

function updateReward(uint256[] memory rewards) public {
    for(uint256 i = 0; i < rewards.length; i++) {
        // 每次循环都在写状态变量，极度昂贵
        totalReward += rewards[i]; 
    }
}
````

真正的gas优化（内存缓存）：

````
```solidity
function updateReward(uint256[] memory rewards) public {
    uint256 tempTotal = totalReward; // 1次 SLOAD
    for(uint256 i = 0; i < rewards.length; i++) {
        tempTotal += rewards[i]; // 在 Stack 上操作，几乎免费
    }
    totalReward = tempTotal; // 1次 SSTORE
}
````

将状态变量转存为局部变量，在**循环（Loop）或者多次读写同一状态变量**时会有质的优化飞跃。

# 重入攻击：

````
```solidity
contract Attack {
    EtherStore public etherStore;

    constructor(address _etherStoreAddress) {
        etherStore = EtherStore(_etherStoreAddress);
    }

    // 攻击入口
    function attack() external payable {
        require(msg.value >= 1 ether);
        etherStore.deposit{value: 1 ether}();
        etherStore.withdraw();
    }

    // 关键点：当 EtherStore 转账给本合约时，此函数自动触发
    receive() external payable {
        if (address(etherStore).balance >= 1 ether) {
            // 再次调用 withdraw，此时 EtherStore 还没执行到 balances[msg.sender] = 0
            etherStore.withdraw();
        }
    }

    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
}
````

### 1.攻击过程深度分析

1.  **准备**：`EtherStore` 中存有其他用户的 10 ETH。
    
2.  **触发**：攻击者调用 `Attack.attack()`，存入 1 ETH 并立即调用 `withdraw()`。
    
3.  **第一次交互**：`EtherStore` 执行 `call` 转账给 `Attack`。
    
4.  **劫持流**：`Attack` 收到钱，触发 `receive()`，在 `EtherStore` 执行 `balances[msg.sender] = 0` **之前**，再次调用了 `withdraw()`。
    
5.  **递归**：`EtherStore` 检查 `require(balances[msg.sender] > 0)`，由于上次调用还没结束，余额依然是 1 ETH，检查通过，再次转账。
    
6.  **清空**：这个过程会反复进行，直到 `EtherStore` 的钱被抽干。
    

## Q&A:

### receive()函数是什么？怎么来的？

1.在 Solidity 0.6.0 版本之前，合约只有一个**回退函数（fallback function）用于处理合约交互失败的逻辑**。无论是因为调用的函数名不存在，还是有人直接转账 ETH，都会挤在这个没有任何名字的函数里。

-   **旧版本的痛点**：由于转账逻辑和异常调用逻辑混在一起，开发者经常忘记检查 `msg.data` 是否为空。这导致有些合约在接收转账时，意外执行了本应报错的逻辑。
    
-   **进化**：为了提高安全性，Solidity 官方在 **0.6.0 版本**将这个功能一分为二：
    
    1.  `receive()`：专门处理单纯的 ETH 转账（空的 `msg.data`）。
        
    2.  `fallback()`：专门处理函数名对不上或者带数据的调用。
        

可以理解为：`receive()` 函数就是合约的“”自动感应收银机”**。一旦有人向该合约转账 ETH，且没有携带任何附加数据（msg.data为空），这个函数就会被**立即、自动地触发执行。

2\. 职责分离：明确“转账”与“调用”

在早期版本中，所有逻辑都挤在 `fallback()` 函数里。

-   **设计初衷**：通过引入 `receive()`，Solidity 强制开发者将\*\*“纯资金接收”**逻辑与**“无效合约调用”\*\*逻辑分开。
    
-   **安全价值**：这防止了用户本想给合约转账，却意外触发了合约中复杂的逻辑，或者本想调用一个不存在的函数，却意外完成了转账。
    

3.防止“资金黑洞” (Security by Default)

这是设计上最重要的安全改进。

-   **老问题**：以前如果你向一个没有编写任何接收逻辑的合约转账，有些合约会默认接收并“吞掉”这笔钱，而由于没有代码逻辑，这笔钱可能永远无法提取。
    
-   **新方案**：在 0.6.0 之后的版本中，如果你的合约**没有**显式声明 `receive()` 函数，那么任何外部向该合约的纯 ETH 转账都会被 EVM **直接拒绝（Revert）**。
    
-   **意义**：这保护了用户，确保钱只能进入那些“做好了收钱准备”的合约。
    

所以，重入攻击就是利用合约的执行顺序错误，也就是**“检查-效果-交互”**模式，它在实际扣除用户余额之前，就先进行了转账交互。

使攻击者利用攻击合约无限提取资金。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->



















# 今日感悟：

web3实习计划第一周结束了，这是我第一次以一名学员的身份和大家一同学习。 在以前不管是钻研技术还是生活，总习惯"单打独斗"，甚至觉得社交是一件带有强烈目的性的任务。 但是在学员群里，我感觉自己像是跳进了一个高频振动的能量场，大家都在源源不断的输出知识，这种感觉是我以前从没有体会到的。 这种从"我"到"我们"的转变，让我觉得很有趣，我也希望通过这次经历，能够学到更多。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->




















# 今日目标：

1.  对上一周所学内容进行复盘
    
2.  晚上7点开一个黑客松研讨会，讨论一下现有demo
    

今天周六，假期愉快~

[https://docs.0xweather.com/web3-internship-bootcamp-2026-winter/day5-meeting/](https://docs.0xweather.com/web3-internship-bootcamp-2026-winter/day5-meeting/)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->






















# 昨天做了什么：

1.  参加了分享会，了解到x402和ERC8004协议，对AI+Web3结合有了新的理解。也让我对现有的demo方案有了新的想法。
    
2.  参加了co-Learning，昨天更多的还是在解答关于运营方面的问题，对于想做账号的技术者也有一些帮助。
    

# 今日目标：

1.  参加co-Learning
    
2.  参加晚上的例会，听一听同学们的见解
    
3.  整理笔记发布到个人知识库
    
4.  继续写demo
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->























# 今天做了什么：

1.  对昨晚的直播，Web3合规内容进行整理，并上传到个人知识库。
    
2.  准备写职业规划。
    
3.  继续研究项目Demo。
    

# 今日目标：

1.  参加下午co-Learning和晚上的会议。
    
2.  持续学习。
    

关于实习计划的所有笔记文档都放在：[https://docs.0xweather.com/web3-internship-bootcamp-2026-winter/](https://docs.0xweather.com/web3-internship-bootcamp-2026-winter/)
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
























# 昨天直播的思考：

### 1\. 资产自托管：如何既安全又不复杂？

**初学者的痛点：** 记不住 12 个助记词，总怕弄丢或被盗。

-   **答案：让钱包变得像“App 账户”一样。**
    
    -   **账户抽象（Account Abstraction, AA）：** 这是目前最火的方案。它让钱包变成一个“智能合约”。你可以设置“社交恢复”，比如万一私钥丢了，可以找三位好友共同签名帮你找回，而不需要记那串复杂的助记词。
        
    -   **MPC 钱包（多方计算）：** 这种技术把私钥分成几份，分散在你的手机、云端和服务器上。黑客拿走一份没用，你也不需要管理完整的私钥，就像刷脸支付一样简单，但控制权还在你手里。
        

### 2\. 公共基础设施：没有“收税”的政府，谁来修路？

**初学者的痛点：** 免费的东西没人修，收费的东西没人用。

-   **答案：从“被动纳税”转向“协议激励”与“二次方融资”。**
    
    -   **协议内税收：** 很多 Web3 项目在每一笔交易中自动抽取极小比例存入“国库（Treasury）”，专门用于后续维护。
        
    -   **二次方融资（Quadratic Funding）：** 比如 **Gitcoin**。它不是看谁捐的钱多，而是看谁获得的捐赠人数多。人数越多，池子里的公共配比资金拨付就越多。这让“公道自在人心”变成了代码逻辑，保证资源流向大家真正需要的设施。
        

### 3\. 隐私与治理：没有审核员，坏人横行怎么办？

**初学者的痛点：** 隐私是好事，但如果坏人利用隐私洗钱或造谣，谁管？

-   **答案：用“零知识证明”实现“合规的隐私”。**
    
    -   **零知识证明 (ZKP)：** 这项技术可以让你向系统证明“我是合法公民”或者“我没有发布有害内容”，但不需要暴露你的真实身份或具体内容。
        
    -   **去中心化信誉系统：** 就像 Web3 里的“芝麻信用”。如果一个地址频繁涉及黑产，它的信誉分会变低，从而被主流应用自动屏蔽。治理不是靠“删帖”，而是靠“共识过滤”。
        

### 4\. 去中心化协作：如何实现“多劳多得”的公平？

**初学者的痛点：** 大家天南地北，谁干了活、干得好不好，谁说了算？

-   **答案：用 DAO（去中心化自治组织）和“贡献证明”。**
    
    -   **贡献证明（Proof of Contribution）：** 协作不是看老板心情，而是看链上记录。你写的代码、做的设计、推广的流量都会被记录成数字足迹。
        
    -   **代币化激励：** 所有的贡献直接对应治理代币。分配规则在项目开始前就写在智能合约里，一旦达成条件，钱（代币）自动打到你账上，没有人能赖账。
        

我觉得 Web3 并不是要完全消灭“管理”和“分配”，而是想用**透明的代码**代替**不透明的人心**。虽然现在还在早期（像极了 90 年代初的互联网），但这些方案正让 Web3 变得越来越好用、安全。

# 今天做了什么：

1.  对实习手册提交第2个PR，关于非技术岗职位介绍及要求。
    
2.  对昨日的直播总结，发布到个人知识库
    

# 今日目标：

1.  开始写DID合约 demo
    
2.  继续学习zk
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->



























# 今日目标：

1.  参加下午的Co-Learning
    
2.  参加晚上的Web运行原理分享会
    
3.  复习Solidity，准备写demo
    
4.  学习zk相关的知识
    
5.  将今日所学内容整理为笔记并发布到个人知识库
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




























# 今日目标：

1.  完成任务
    
    -   创建钱包并转账
        
    -   mint第一个NFT
        
2.  把完成过程更新为自己的笔记，发布到个人知识库。
    
3.  参加下午的以太坊中文周会和1小时线上Co-Learning时间。
    
4.  参加晚上的Web行业介绍&岗位预览。
    

由于web3基础已经自学，故不再更新基础知识。只跟进直播和即将要学习的新知识作为个人记录。

# 今日学习成果：

开营仪式笔记：[https://docs.0xweather.com/web3-internship-bootcamp-2026-winter/day1-01/](https://docs.0xweather.com/web3-internship-bootcamp-2026-winter/day1-01/)

中文周会笔记：[https://docs.0xweather.com/web3-internship-bootcamp-2026-winter/week-meeting01/](https://docs.0xweather.com/web3-internship-bootcamp-2026-winter/week-meeting01/)

Web3 行业全局与岗位概览笔记：[https://docs.0xweather.com/web3-internship-bootcamp-2026-winter/day1-02/](https://docs.0xweather.com/web3-internship-bootcamp-2026-winter/day1-02/)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
