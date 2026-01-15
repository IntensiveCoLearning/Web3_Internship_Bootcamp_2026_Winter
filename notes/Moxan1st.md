---
timezone: UTC+8
---

# Moxan1st

**GitHub ID:** Moxan1st

**Telegram:** @Moxan1st

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
# 总览

1.  跟GPT学foundry的基本操作——已经把foundry自带counter部署到sepolia [合约地址](https://sepolia.etherscan.io/address/0x02343bfb4ce8e6e5add0d95562187787fd5ce2ec#readContract) 并且通过命令存变量，加一。
    
2.  理解和写简易的项目——看懂remix自带storage、owner和ballot
    
3.  《021学习以太坊第四章》——当作词典查缺补漏一下
    

# 详情

## Foundry部署合约

### 上链操作

```
forge script script/Counter.s.sol   --rpc-url $SEPOLIA_RPC_URL   --private-key $PRIVATE_KEY   --broadcast
```

forge：Foundry的主命令类似npm

script：部署/调用合约

\--broadcast：广播出去，上链

### 返回结果

```
Chain 11155111

Estimated gas price: 2.01715883 gwei

Estimated total gas used for script: 203856

Estimated amount required: 0.00041120993044848 ETH

==========================

##### sepolia
✅  [Success] Hash: 0x885152ed592b64843b3e87fb4b85b9a7f7fdd24e2020f784705dc4b22e47b682
Contract Address: 0x02343bFb4CE8E6E5Add0D95562187787Fd5Ce2ec
Block: 10046638
Paid: 0.000161505770940765 ETH (156813 gas * 1.029925905 gwei)

✅ Sequence #1 on sepolia | Total Paid: 0.000161505770940765 ETH (156813 gas * avg 1.029925905 gwei)
```

-   `11155111` 是 Sepolia 的 **chainId**
    
-   Q：为什么gas实际值<预估值？A：实际EVM执行中会有没走到的分支、没用到的 storage。gas会退回
    
-   区块号Block: 10046638
    
-   合约地址Contract Address: 0x02343bFb4CE8E6E5Add0D95562187787Fd5Ce2ec
    

```
cast call 0x02343bFb4CE8E6E5Add0D95562187787Fd5Ce2ec "number()" --rpc-url $SEPOLIA_RPC_URL
```

-   cast：Foundry的链上交互工具
    
-   call：读取，不改变链上状态，不生成交易所以不消耗gas
    
-   send：写入
    
-   ”number()“：函数签名
    
-   Q：我没有写number()这个函数，为什么可以直接调用？A：编译器自动生成了这个函数
    

```
function number() external view returns (uint256) {
    return number;
}
```

### 存变量

```
cast send 0x02343bFb4CE8E6E5Add0D95562187787Fd5Ce2ec "setNumber(uint256)" 100 --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY
```

-   send：上链
    
-   “setNumber(uint256)”：函数名，形参要省去
    
-   100：传入形参的具体值
    
-   private-key：签名交易，支付gas；相当于在metamask里点确认
    
-   Q：实际发生了什么？A：EVM在调用这些函数的时候，实际是先利用函数选择器keccak256("setNumber(uint256)")\[:4\]，计算出对应的数值取前面4个字节，一开始定义它的时候也计算到同样的数值，当这个数值匹配上，就跳到number()对应的代码段。然后calldata = selector + 参数编码。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->




# 总览

1.学习学分榜前几名同学的笔记

-   yoona333——感觉是进阶选手，但是笔记干货少，AI内容比较多，提示词很丰富，对应生成的网页像模像样。
    
-   huahuahua1223——肯定是进阶选手，提供了一些合约部署的细节
    
-   Jimmy Wang——学习了一些链上支付的概念
    
-   ShihaoZhou-NEU——终于找到跟我一样的入门选手了，主要学习021第一章的概念
    

2.《021学习以太坊第二章》——乱麻了，有些叙述是重复的，有些Section前后缺乏关联。更像是一本词典。

3.《021学习以太坊第三章》——大部分很顺；虽然有一些内容重复，但是可读性比前两章有提高。

4.在Remix学习基础代码——完成简单合约的配置了！

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-14-1768391493272-image.png)

5.配置Foundry——手册中的方式下载速度很慢，利用Clash代理端口转发WSL下的数据科学上网后速度加快

```
//添加镜像
export FOUNDRYUP_GITHUB_API=https://ghproxy.com/https://api.github.com
foundryup
```

6.WEB3安全讲座：

-   新型骗局：精准猎杀（面对高价值目标）、广撒网（普通目标）、Vibe coding中发生的AI数据污染导致的漏洞等
    
-   对于用户：不点、不签、不装、不转。拒绝盲签，从官方渠道下载应用，妥善保管私钥，资产分离管理
    
-   对于项目方：全生命周期安全管理，代码审计，建立应急响应
    

7.WEB3合规讲座：

一、核心禁止（三大负面清单）

1\. 绝对禁止：ICO公开融资、虚拟币交易所业务（永续合约涉赌）；

2\. 挖矿限制：仅AI算力超算中心等合规路径可操作

二、关键业务风险与合规提示

1\. 出金：核心风险为收赃款、镀金攻击（故意打赃款冻账户）；

建议香港合规交易所出金，务必税务申报；

2\. U商：大陆基本不可合规，易遭洗钱团伙盯上，U卡需核查发卡方资质；

3\. 项目方：融资忌非吸/集资诈骗、法币对敲忌非法经营、返佣忌传销；警惕陪侦公司取证

三、安全与防护要求

1\. 用户：不点不签不装不转，拒盲签、官渠下应用、存好私钥、资产分离；

2\. 项目方：全生命周期安全管理，做代码审计，建应急响应；

3\. 新型风险：精准猎杀/广撒网骗局、AI数据污染漏洞，需重点防范

# 详情

## 第二章

### Section1以太坊软件

以太坊软件的作用和区分：

|   | EL | CL | Validator |
| --- | --- | --- | --- |
| 功能 | 处理交易与合约逻辑 | 管理 PoS 共识流程 | 专注单个验证者的共识操作 |
| 存在形式 | 独立软件 | 独立软件 | 多数为共识客户端的内置模块，部分可作为附属进程 |
| 依赖关系 | 独立 | 依赖EL | 依赖CL |
| 投票角色 |   | 投票的 “调度者 + 汇总者”负责验证其他节点投票的合法性（如签名有效性、slot 是否合规），统计投票达成的共识进度 | 投票的 “执行者 + 签名者”仅生成自身的投票数据并签名，不处理其他节点的投票 |

### Section2通信过程

1.  发现阶段（UDP+Kademlia）：新节点通过UDP加boot nodes好友后发送FindNode请求，boot nodes回复一套邻居或者节点，新节点接着对这些节点要通讯录，直到填满。
    
2.  Kademlia：节点有256位ID，节点之间的距离用异或计算的结果度量，异或结果越大，距离越远。
    
3.  建立安全连接（TCP+RLPx）：TCP是连接基础，RLPx是多路复用等子协议
    
4.  传播阶段（Gossip+请求/响应）：Gossip随机向没听说过的邻居传播最新消息；请求/响应类似于专门去档案室翻某一天档案。
    

### Section3节点类型

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-14-1768354634983-image.png)

### ~Section8客户端协同和同步机制

-   执行与共识客户端协同：CL 通过 Engine API 向 EL 请求区块执行结果，EL 返回执行负载（execution payload），CL 封装为 Beacon 区块并广播，其他节点通过 EL 复验执行结果后确认共识。
    
-   节点同步模式：
    
    -   全同步（Full Sync）：从创世块逐块执行交易，重建所有状态，最安全但耗时久；
        
    -   快照同步（Snap Sync）：下载最新状态快照 + 区块头，并发填充数据，数小时～1 天完成（默认推荐）；
        
    -   轻同步（Light Sync）：仅同步区块头，依赖全节点提供证明，适合轻量化场景。
        

### ~Section12

-   运行全节点的必要性：保障隐私（不泄露地址 / IP）、抗审查（自主广播交易）、数据自主验证（无需依赖第三方）、强化网络去中心化与客户端多样性。
    
-   个人节点搭建流程：选择节点类型（全节点 / 归档节点）→ 配置硬件（全节点需 4 核 CPU+1TB NVMe SSD）→ 安装 EL+CL 客户端→ 选择同步模式（Snap Sync 优先）→ 开启 RPC 接口供开发 / 使用。
    
-   不跑节点的替代方案：通过公共分析平台（Dune/Flipside）、BigQuery 公共数据集、区块浏览器 API（Etherscan）、Web3 数据服务商（Covalent）实现链上分析
    

## 第三章

### Section1 EOA账户（用户）

由私钥直接控制，无合约代码，可主动发起交易（转账、调用合约）。核心特征是 “私钥 = 控制权”，状态仅包含 nonce（交易计数器）、balance（余额），无 code 和 storage。

### Section2 合约账户（自助售货机）

由部署在链上的 EVM 字节码控制，无私钥，不能主动发起交易，仅能被 EOA 或其他合约调用后执行预设逻辑。状态包含 nonce（合约创建计数）、balance、storageRoot（存储状态根）、codeHash（字节码哈希）。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-14-1768360218772-image.png)

### Section3 0x的来历

-   地址本质是 20 字节（160bit）二进制数据，文本展示时采用十六进制格式，前缀 “0x” 是编程界传统，用于明确标记 “后续为十六进制数据”，避免与普通数字混淆。
    
-   0x 并非协议强制要求，但已成为行业标准，工具（钱包、浏览器）均支持识别带 / 不带 0x 的地址格式，且通过 EIP-55 混合大小写实现地址校验。
    

### Section4 概念补充

多签：要求多人签名才能执行的智能合约钱包

时间锁：时延NPC，交给这个时延保管一阵，随时可以审查、挑战和撤出资金。

### Section5、7 账户交互机制

### 1\. EOA 调用合约账户

-   EOA 发起交易，通过 calldata（函数选择器 + 参数）指定调用逻辑，EVM 加载合约字节码执行，过程中可修改合约存储、触发事件、转账 ETH。
    
-   交易执行需支付 Gas，失败时状态回滚，但已消耗 Gas 不退还。
    

### 2\. 合约账户互调

-   通过 call、delegatecall、staticcall 等 opcode 实现：
    
    -   call：普通调用，对方合约在自身存储上下文执行；
        
    -   delegatecall：在调用方存储上下文执行对方代码（适用于代理升级）；
        
    -   staticcall：只读调用，不允许修改状态。
        
-   互调属于内部消息调用，不生成新交易，消耗当前交易的 Gas。
    

### 3\. 合约账户无法主动发起交易

-   即使通过 ERC-4337（账户抽象）实现 “类 EOA 交互”，底层仍需 Bundler（EOA）打包交易，合约本身无签名能力
    

### Section8、9 代币关联

-   代币本质是合约账户的 “记账系统”：ERC-20 通过 mapping 记录余额和授权额度，ERC-721 通过 mapping 绑定 tokenId 与持有者。
    
-   用户 “持有代币” 本质是合约存储中记录该地址的余额 / 所有权，钱包通过调用合约的 balanceOf、ownerOf 等接口查询并展示。
    

### Section10

-   **不可篡改性**：合约字节码部署后默认不可修改，需通过代理模式（Transparent Proxy、UUPS）实现逻辑升级（状态存代理合约，切换实现合约）。
    
-   **销毁机制**：SELFDESTRUCT opcode 在 Dencun 升级（EIP-6780）后语义变更，仅在 “创建合约的同一交易中调用” 时会删除代码和存储，其他场景仅清空余额，不再销毁代码。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->










# 总览

-   领取sepolia测试币并且转账——Done
    
-   《021学习以太坊》——第一章后半的阅读感受：行文结构比较杂乱，没有分轻重点，有点不知所云。
    
-   Day2 youtube视频——与实习手册的任务有重合
    
-   铸造第一个DFT——好耶，有点意思
    
-   布鲁斯老师的5个课后问题——按照自己理解，回答了一通
    

# 详情

## Section7

1.  去中心化实现机制中，并没有区分这个视角下的分层与L1,L2分层的差异，以及为什么需要有这个差异。
    
2.  “综合来看，以太坊的去中心化并不是单一机制的结果，而是多层叠加的产物”，个人感觉这句话应该作为“总分总”结构的“总览部分”，然后开始分点介绍各个层级的作用：
    

|   |   |
| --- | --- |
| 共识层 | PoS + 经济惩罚（核心约束） |
| 网络层 | P2P 架构 + 轻节点支持（参与无门槛） |
| 实现层 | 多客户端多样性（避免技术垄断） |
| 扩容路径 | Rollup - 中心 + 数据分片（分散负载） |
| 治理层 | 开放共识 + 社区驱动（无中心决策） |

总结：以太坊通过 “PoS 经济约束 + P2P 网络 + 多客户端 + Rollup 扩容 + 开放治理”，从经济、技术、参与权三方面实现去中心化，核心是 “任何人可参与、任何人可验证、无单一主体能控制”。

## Section8

1.硬件有门槛

2.常见指标：吞吐量、时延、资源消耗和容错能力

3.终于介绍L1和L2了：

-   L1是链本身
    
-   L2是“加速层”，包含：Rollup汇总、状态通道（双方指的是哪双方？）、侧链（作用是什么没介绍）
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-13-1768291110067-image.png)

个人总结：以太坊是一个以去中心化共识为核心的开放式（permissionless）区块链网络，L1 保证安全与去信任，L2 承担扩展性，从而在去中心化、安全性和性能之间取得平衡。

## Section9,10

具体应用和核心的创新，内容上是有重复的地方，如果不分轻重层次的话可以放在一起讲。

## Section11

### 社区的核心作用

-   治理机制：通过 AllCoreDevs 会议（技术决策）、以太坊魔术师论坛（FEM，EIP 讨论）、GitHub EIP 流程推动协议升级，过程全公开。
    
-   参与者构成：全球多元群体（开发者、研究员、设计师、用户、DAO 成员等），通过黑客松、meetup、社区会议参与生态建设。
    
-   核心价值观：开源透明、去中心化、协作优先，基金会仅 “管理而非控制”。
    

### 开发生态的核心支撑

-   工具与基础设施：Remix（入门 IDE）、Hardhat/Foundry/Truffle（开发测试框架）、Infura/Alchemy（节点服务）、Scaffold-ETH/Wagmi（前端集成工具）。
    
-   教育与培训：[g官方](http://ethereum.org)文档、Devcon/Devconnect 大会、黑客松（每年催生上千新项目）。
    
-   生态创新：DeFi、NFT、DAO、L2、RWA 等应用层由独立团队 / DAO 构建，需求反向反馈至 EIP 与客户端开发；协议升级（Merge、Dencun、Pectra）由多团队协作迭代。
    

# 第二章

## Section1

1.  运行以太坊客户端软件的电脑，就是以太坊节点
    
2.  以太坊节点 = 一台电脑 + EL执行客户端（干活）+CL共识客户端（投票） +（可选）一个验证者客户端
    
3.  EL跟CL之间通过EngineAPI协作
    

# 直播作业

Q1:资产自托管:如何提高安全性、降低管理私钥的复杂度?

A：这两个需求是需要平衡的吧？提高安全性可以通过改进存储方式，或者“狡兔三窟”的方式，但是对应的复杂度也会增加呢。

Q2:没有中心化机构/税收时，公共基础设施谁来维护?

A:通过奖励维护基础设施的行为鼓励大家自愿维护，这个奖励的源头可以从税收（Gas）中抽取，也可以由社群、项目收益捐助。

Q3:如果有税收，又如何分配?

A:可以酌情分配给基础设施维护，生态发展和应急储备。

Q4:没有审查且隐私很强时，如何界定并治理有害信息黑产?

A:不是很懂如何定义这个有害信息黑产，但是对于治理，可以通过链上数据分析资金流向给执法机关提供线索。

Q5:去中心化协作下，如何实现公平、可信的分配?

A:把规则转化为代码，把过程公开，贡献与激励挂钩即可。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->















**Week 1 day 1**

# **总览**

-   基础工具安装——Done
    
-   必学导读资料——《021学习以太坊第1章》概念多且零散，例子虽然多但是缺少一个情景或者例子把多个概念**同时整合**起来。部分原理（如图）解释过于简化了。
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-12-1768216018539-image.png)
-   辅助资料——未观看
    

# 详情

## 《实习手册——入门导读》

大部分内容很形象，结构化的对比数据非常直观。主要描述了

1.  区块链、BTC的概念及其引申的去中心化理念。
    
2.  着重区分和对比了公联、私链和联盟链。
    
3.  澄清了一些概念误区，并且强调web3金融属性。
    

## 《021学习以太坊第1章》

## Section2

1.  智能合约和DApp的概念有部分重叠，可以加以区分。
    
    1.  **智能合约**：写在区块链上的程序
        
    2.  **DApp**：用户真正使用的去中心化应用（前端 + 合约 + 交互）
        
2.  PoW到PoS：这个对比非常适合使用表格的形式，还可以展现各自的优势和劣势。也是ETH的特别之处。
    
3.  第一次提及Blob的时候可以加入对比升级前后具体交易的详细过程和结果。
    
4.  分片概念是利用分区+采样获得（怎样的）收益，那“有把握”这样的概念有具体量化的方法吗？
    
5.  叔块也是ETH有别于BTC的一个特点，“安慰奖”非常直观。
    
6.  “开发者友好”总览图我看着不是很友好，可能不适合我。
    

## Section3

1.  Gas中的Base Fee既然要被直接销毁为什么还需要被计算？
    
2.  ETH的作用：GAS, Staking(rewards and slashing), 价值载体
    
3.  区分同质化（强调货币属性）和非同质化货币（强调唯一性和所有权）
    

## Section4

ETH的优势

1.  区块链的技术基础
    
2.  智能合约——以太坊的可编程性
    
3.  图灵完备——？？？？？
    

## Section5

BTC——数字黄金，ETH——“自定义”程度无穷

## Section6

dApp的劣势也源于优势：

1.  维护困难，要求“不出错”
    
2.  L1性能上限已经被锚定，只能通过L2层改进扩容
    
3.  GAS受网络拥堵程度影响，导致“高峰期很贵”的结果
    
4.  新用户的学习成本高
    
5.  用户的中心化使用习惯和去中心化应用的用途
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
