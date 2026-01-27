---
timezone: UTC+8
---

# Eddie-534

**GitHub ID:** Eddie-534

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->
26.1.27 今天继续学习Hardhat，问题已经基本解决，现在学会创建项目、编写合约和部署脚本，还有实时交互一些简单的操作。

![屏幕截图 2026-01-27 153326.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-27-1769528865746-_____2026-01-27_153326.png)
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->

26.1.26 今天学习使用 Hardhat ，中途还是遇到一些困难，如 Hardhat 、 Node 版本问题等，还在琢磨中。

![屏幕截图 2026-01-26 233429.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-26-1769441689788-_____2026-01-26_233429.png)![屏幕截图 2026-01-26 233403.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-26-1769441700346-_____2026-01-26_233403.png)
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->


26.1.25 今天继续挑战 Solidity ethernaut ，感觉还是有难度的，剩余关卡会接下来逐步通过。

![屏幕截图 2026-01-25 234953.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-25-1769356213587-_____2026-01-25_234953.png)![屏幕截图 2026-01-25 235002.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-25-1769356224689-_____2026-01-25_235002.png)
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->



26.1.24 今天完成了 Solidity ethernaut 的部分挑战，对 Solidity 代码有了更深的理解，明天计划完成全部挑战。

![屏幕截图 2026-01-24 231701.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-24-1769269307128-_____2026-01-24_231701.png)![屏幕截图 2026-01-24 234710.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-24-1769269638605-_____2026-01-24_234710.png)
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->




26.1.23 今天运行并理解了solidity基础的部分代码

-   第一个合约：DataLocations
    

功能：教学合约，展示 Solidity 中三种数据位置的区别：

storage：永久存储在区块链上的数据（如硬盘）

memory：临时内存数据（函数执行期间存在）

calldata：只读的函数参数（最省gas）

用途：学习如何正确使用数据位置来节省gas费用。

-   第二个合约：ReceiveEther & SendEther
    

功能：演示以太币的接收和发送：

ReceiveEther部分：

展示如何接收以太币：使用 receive() 和 fallback() 函数

提供余额查询功能

SendEther部分：

展示三种发送以太币的方法：transfer、send、call

推荐使用 call 方法（现代标准）

用途：学习如何编写能处理以太币转账的合约。

-   第三个合约：AssemblyIf
    

功能：展示 Solidity 中的内联汇编（Yul） 语法：

在汇编中使用 `if` 条件判断

在汇编中使用 `switch-case` 多条件判断

演示低级代码编写

用途：学习如何用汇编语言写更高效、更省gas的代码（高级话题）。

-   总结：
    

1\. 数据基础 → 2. 资金操作 → 3. 高级优化

从基础概念（数据存储），到实际应用（以太币转账），再到高级技巧（汇编优化）的学习路径。

![屏幕截图 2026-01-23 232651.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-23-1769182172797-_____2026-01-23_232651.png)![屏幕截图 2026-01-23 233413.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-23-1769182534610-_____2026-01-23_233413.png)![屏幕截图 2026-01-23 233458.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-23-1769182577991-_____2026-01-23_233458.png)
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->





26.1.22 今天继续学习solidity基础部分剩下内容。

-   归纳整理：
    

1.  可见性与接口: private/internal/public/external 四件套终于弄清了，Interface 像是合约的“使用说明书”
    
2.  式转账操作: transfer/send/call 三种打钱方式：
    

transfer()：最安全但 gas 固定

send()：返回 bool（可能会默默失败）

call()：功能最强但风险也最大

3.  特殊函数：
    

receive()：纯收钱入口

fallback()：万能兜底

delegatecall：高级玩法，能让合约“借用”其他合约的代码

4.  安全与优化：
    

try/catch 终于能优雅处理错误了

用 library 复用代码

ABI 编码解码是合约对话的“翻译官”

unchecked 可以省 Gas 但得自己保证安全

5.  硬核 Assembly：
    

直接写底层代码，性能拉满但写错一点就炸

-   特别注意：
    

用 call 转账时差点忘了检查返回值，幸好只是在测试网。

![屏幕截图 2026-01-22 145346.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-22-1769064838564-_____2026-01-22_145346.png)

![屏幕截图 2026-01-22 145101.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-22-1769064850471-_____2026-01-22_145101.png)
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->






26.1.21 今天学习了Solidity的基础部分的部分内容。

-   归纳：
    

函数结构：function关键字+函数名+参数+可见性+状态修饰符+returns  
可见性：public（全网可调用）、private（仅合约内部）、internal（继承合约可用）  
状态修饰符：view（只读不耗Gas）、pure（纯计算）、payable（可收ETH）  
特殊函数：receive()和fallback()像是合约的“自动回复消息”，处理意外情况很实用

-   特别注意点：写returns时千万别漏s，函数参数记得标明类型，不然remix编译器会疯狂报错……
    

![屏幕截图 2026-01-21 225532.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-21-1769007457235-_____2026-01-21_225532.png)

![屏幕截图 2026-01-21 225646.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-21-1769007469499-_____2026-01-21_225646.png)

![屏幕截图 2026-01-21 225715.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-21-1769007512849-_____2026-01-21_225715.png)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->







26.1.20 今天学习了Web3实习手册的智能合约开发部分

-   核心概念
    

Dapp架构：前端（连接钱包）+ 智能合约（链上逻辑）+ 索引器（同步事件到数据库）+ 去中心化存储（存文件）

开发流程：写合约 → 测合约 → 部署测试网 → 写索引器 → 开发前端 → 主网上线

-   重点技术
    

1\. 开发环境

Foundry：速度快，现代（推荐）

Hardhat：生态好，适合新手

本地节点：测试用anvil（Foundry）或hardhat node

2\. RPC节点

作用：前端与区块链的桥梁

推荐：开发用Infura/Public Node（免费），生产用Alchemy

注意：API Key要放.env，别上传GitHub！

3\. Solidity要点

状态变量：永久存链上

函数类型：

view/pure：只读，不花Gas

payable：可收ETH

安全必做：

防重入：先更新状态再转账

权限控制：敏感函数加onlyOwner

整数溢出：用Solidity 0.8+版本

4\. 部署实战

1.  写合约 → 2. 编译 → 3. 领测试币（水龙头）→ 4. 部署Sepolia测试网 → 5. Etherscan查结果
    

5\. 前端整合

javascript :

// 连接钱包

await window.ethereum.request({method: 'eth\_requestAccounts'});

// 调用合约

contract.methods.函数名().send()  // 写操作，花Gas

contract.methods.函数名().call()  // 读操作，免费

-   个人总结
    

实际比想象复杂：不只是写.sol文件，要考虑测试、部署、前端交互、数据查询

下一步计划：用Hardhat跑通留言板项目，尝试自己部署一个简单合约到测试网

关键认知：智能合约一旦部署无法修改，安全第一，测试要全面

核心收获：理解了Dapp完整开发流程，明确了从本地开发到测试网部署的关键步骤。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->








26.1.19 今天学习了021以太坊教材的第四章，了解了智能合约的工作原理与价值。

-   智能合约
    

链上自动执行程序，像“数字自动售货机”

条件满足就自动运行，无需人工

部署后代码默认不可改（升级需用代理模式）

-   核心特点
    

解决信任问题：代码执行替代中介担保

公开透明：所有操作链上可查

确定性强：相同输入必得相同输出

-   Solidity语言优势
    

专为EVM设计，语法友好

生态完善（Hardhat/Foundry/OpenZeppelin）

多链通用（主网/L2都兼容）

-   编译产物
    

字节码：真正执行的机器码

ABI：接口说明书（JSON格式）

元数据：编译器版本、源码哈希等

-   部署
    

合约地址（链上门牌号）

交易哈希（可查证）

ABI（用于调用合约）

-   个人总结
    

智能合约 = 代码化的信任机制。

学习重点不是语法，而是理解“代码即规则”的思维转变。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->









26.1.18 今天继续跟进B站Chainlink预言机教程，学完了第三章剩下的部分。

-   内容梳理：
    

1.  token vs coin
    

区分了token和coin的概念，明白了coin是底层链原生货币（如ETH），而token则是建立在链上的“自定义资产”，ERC-20就是最常见的一种token标准。

2.  创建一个通证合约
    

第一次动手写了一个最简单的通证合约，虽然只有名称和符号，但算是迈出了第一步！

3.  Solidity 继承
    

学习了继承的语法和用途，知道怎么通`is`关键字复用合约逻辑，感觉这和面向对象编程里的继承挺像的。

4.  ERC-20标准
    

详细学习了ERC-20的六大核心函数和两个事件：

`balanceOf`

`transfer`

`transferFrom`

`approve`

`allowance`

`totalSupply`

理解了它是如何实现代币转账、授权和余额查询的。

5.  抽象合约 & 虚函数
    

接触`abstract contractvirtual`关键字，明白了如何设计可扩展的合约接口。

6.  重写
    

通`override`关键字重写父类函数，实现自定义逻辑，感觉就像在“修补”标准模板。

7.  继承 ERC-20 合约
    

直接继承OpenZeppelin的ERC-20合约，快速构建合规通证，省去重复造轮子的时间。

8.  自定义 ERC-20 合约
    

尝试在标准ERC-20基础上添加了铸造和销毁功能，让通证具备可调控的供应量。

9.  合约测试
    

虽然没有实际跑测试，但了解了测试的重要性和基本方法，尤其是对ERC-20各类功能的验证流程。

-   学习心得：
    

今天的内容从理论过渡到实战，尤其是ERC-20标准那一块，刚开始看函数名有点懵，但跟着写一遍之后，就清晰多了。继承和重写这部分让我感觉Solidity也挺“现代”的，结构清晰，复用性强。最后自己写一个可铸币的ERC-20合约的时候，真的有种“发行自己的代币”的感觉。

![屏幕截图 2026-01-18 203331.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-18-1768739994283-_____2026-01-18_203331.png)![屏幕截图 2026-01-18 203321.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-18-1768740084596-_____2026-01-18_203321.png)![屏幕截图 2026-01-18 203340.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-18-1768740095863-_____2026-01-18_203340.png)
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->










26.1.17跟着B站上的Chainlink预言机教程，继续学习Solidity的进阶部分

-   内容梳理：
    

1.  通过函数发送 ETH
    

学习了如何在智能合约中接收ETH，并理`payable`修饰符的作用，还区分`transfersendcall`这三种转账方式的区别和使用场景。

2.  预言机设置最小额度
    

为了避免小额攻击和价格波动影响，需要通过预言机设置一个最小支付金额。这部分让我意识到链上数据依赖外部喂价的重要性。

3.  Chainlink 喂价
    

第一次接触Chainlink预言机的实际集成，学习了如何通过Data Feed获取实时价格，并理解了解析返回值的格式（比`int256uint80`的区别）。

4.  从智能合约中提取 ETH
    

实现了合约所有者提取ETH的功能，同时也学习了如何设置权限控制，避免非授权调用。

5.  时间锁
    

为了安全性，引入了时间锁机制，让关键操作（如提款）延迟执行，防止恶意操作。

6.  修改器
    

复习`modifier`的使用，这次用在了权限控制和条件检查上，代码结构更清晰了。

7.  部署和测试
    

虽然没有实际部署，但了解了测试流程和注意事项，尤其是预言机在测试网中的使用

-   学习心得：
    

今天的内容信息量挺大的，尤其是Chainlink预言机的集成部分，让我真正理解了“去中心化数据”怎么在智能合约里用起来。之前总觉得预言机很抽象，今天动手写代码之后感觉亲切多了，时间锁和权限控制这些安全机制也让我意识到，写合约不仅是实现功能，更要防患于未然。

![屏幕截图 2026-01-17 232220.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-17-1768663750869-_____2026-01-17_232220.png)![屏幕截图 2026-01-17 232238.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-17-1768663766018-_____2026-01-17_232238.png)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->











26.1.16 今天在B站系统学习了区块链预言机相关课程，并重点通过Remix平台进行了Solidity基础实践。初步理解了智能合约的开发环境与编写逻辑，成功部署了第一个HelloWorld合约，体验了从代码编写到链上交互的全程。

-   收获：
    

1\. 掌握了Remix在线IDE的基本操作，包括编译、部署与调试；

2\. 理解了Solidity合约的基本结构（如pragma声明、合约定义、状态变量与函数）；

3\. 亲手实现了输出“Hello World”的简易合约，并验证了合约调用；

4\. 对预言机在链下数据获取中的作用建立了初步认知。

-   反思：
    

作为刚入门的新手，明显感受到理论知识与动手实践之间的衔接挑战，后续需加强变量类型、函数修饰符等基础语法的系统学习，同时逐步探索更复杂的合约逻辑设计。

![屏幕截图 2026-01-16 234708.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-16-1768578491482-_____2026-01-16_234708.png)![屏幕截图 2026-01-16 234656.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-16-1768578592017-_____2026-01-16_234656.png)
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->












26.1.15学习了以太坊教材的第二章和第三章

-   第二章：网络结构与节点类型
    

核心理解：

以太坊不是一个“服务器”，而是一个由全球成千上万节点组成的P2P网络。每个节点都是一台运行以太坊客户端软件的电脑，它们互相连接、同步数据、共同维护一条“世界计算机”。

重点摘要：

1\. 节点与客户端的区别

节点：任何运行以太坊客户端并连接到网络的计算机。

客户端：以太坊协议的具体实现软件，分为：

执行客户端（EL）：负责执行交易、维护状态（如Geth、Nethermind）。

共识客户端（CL）：负责PoS共识、出块与投票（如Prysm、Lighthouse）。

合并（The Merge）后，一个完整节点需要同时运行EL + CL。

2\. 节点类型对比

全节点:所有区块 + 最近状态 ；开发者、自托管用户 ；自己验证一切，最安全

归档节点:全节点 + 所有历史状态 ； 数据分析、审计、浏览器 ； 可查询任意历史状态，存储巨大（TB级）

轻节点:只存区块头 ； 手机、资源受限设备 ； 依赖全节点提供数据，可验证但不存储全部

3\. 节点如何通信？

发现节点：用UDP + Kademlia协议“找邻居”。

建立连接：用TCP + RLPx协议加密传输。

传播信息：用Gossip协议（像八卦一样扩散交易和区块）。

4\. 为什么开发者要自己跑节点？

隐私保护：不依赖第三方RPC（如Infura），避免地址被追踪。

抗审查：自己的节点永远可用，不怕被屏蔽。

自主验证：遵循“Don’t trust, verify”原则，自己验证链上数据。

支持去中心化：增加节点数量，增强网络健壮性。

5\. 同步模式

全同步：从创世块开始执行所有交易，慢但最安全。

快照同步：先下载状态快照，再补区块，速度快（推荐）。

轻同步：只下载区块头，适合轻钱包。

我的理解：

原来以太坊的“去中心化”不是口号，是真的靠我们每个人都可以运行的节点撑起来的。自己跑节点就像自己开服务器，既安全又自由，还能为网络做贡献。虽然硬件要求不低，但云服务器+快照同步已经让门槛降低了很多。

-   第三章：账户类型与结构
    

核心理解：

以太坊上有两种账户：EOA（由私钥控制）和合约账户（由代码控制）。它们组成了以太坊上所有人与程序交互的基础。

重点摘要：

1\. EOA（外部拥有账户）

控制方式：私钥/助记词。

特点：

能主动发起交易。

没有代码，只有余额和nonce。

像“银行卡”，谁有密码谁控制。

典型代表：MetaMask里的钱包地址。

2\. 合约账户

控制方式：部署在链上的智能合约代码。

特点：

不能主动发起交易，只能被调用。

有代码、存储、余额。

像“自动售货机”，按写好的规则执行。

典型代表\*\*：Uniswap、NFT合约。

3\. 地址为什么以0x开头？

地址本质是20字节的十六进制数，\`0x\`是编程中表示十六进制的约定俗成前缀。

4\. 合约部署后能否修改？

代码默认不可改，除非使用代理模式升级。

SELFDESTRUCT 现在已被弱化（Dencun升级后），不再推荐用于“删除合约”。

主流升级方案：代理合约（如OpenZeppelin UUPS）。

5\. ERC-20/ERC-721代币本质

代币不是“存在钱包里”，而是合约账户里的一个记账表。

转账本质是调用合约函数，更新合约内部的映射表。

我的理解：

以前我以为钱包地址就是“账户”，现在才知道它分EOA和合约账户。EOA是我自己能控制的“人”，合约账户是写死规则的“机器人”。代币也不是真的“转来转去”，只是合约里改个数字而已。这种设计既灵活又安全，但也意味着私钥一旦丢失，一切都完了

-   学习心得小结：
    

1\. 节点是网络的细胞，跑节点是以太坊精神的体现。

2\. 账户分两种：EOA是人，合约账户是机器。

3\. 安全是第一原则：私钥管理大于一切。

4\. 以太坊是一层层协议和代码堆起来的可理解系统。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->













26.1.14学习了web3实习手册安全与合规部分

-   一、Web3合规性：国内政策与风险
    

1\. 国内对虚拟货币基本是“全面禁止”态度，尤其涉及： ICO、IEO、IDO（任何形式的公开融资）；虚拟货币交易所、撮合服务、支付功能 ；挖矿、矿池运营

2\. 学生容易踩的雷：

代币空投、白名单、积分返利，可能被认定为“非法集资”

游戏类项目若有“充值-抽奖-提现”闭环，可能构成“开设赌场罪”

微信群/Telegram群推广“邀请返佣”，可能涉嫌“传销”

3\. 出金要格外小心：

用USDT兑换人民币，可能被用于洗钱或非法换汇

银行卡容易被冻结，还可能背上“帮信罪”

-   二、全球监管趋势
    

1\. 欧美在合规化：美国有稳定币法案，欧盟有MiCA，要求严格合规

2\. 香港开放但严格：允许交易所持牌经营，但必须KYC/AML

3\. FATF旅行规则：大额转账（>1000美元）需实名，对DeFi挑战很大

-   三、实习入职要注意的法律问题
    

1\. 合同可能无效：很多Web3公司注册在海外，国内无主体，签的合同可能不被法律认可

2\. 薪酬可能不稳定：

用Token/USDT发工资，不被国内法律认可

Token可能暴跌甚至归零

3\. 社保公积金可能没有：影响落户、买房、子女教育等长远问题

-   四、网络安全风险（学生最容易被坑）
    

1\. 钓鱼攻击最常见：

假面试邮件、假奖学金链接、假空投

要求“安装专用软件”“连接钱包签名”的，99%是骗子

2\. 木马/恶意软件： 伪装成“面试软件”“学习资料” ；会劫持剪贴板，偷换你的钱包地址

3\. 社交工程攻击： 冒充HR、学长、客服，诱导转账或泄露助记词

-   五、防护清单
    

面试/实习：

只用Zoom/腾讯会议等公开工具，不装“专用软件”

任何要求提前安装软件的，先上网查安全性

钱包与转账：

助记词绝不截图、不上传、不告诉任何人

转账前核对地址前6位和后4位

主钱包冷存储，测试钱包用于空投

账号安全：

重要账号开2FA（不用短信，用Google验证器）

不同平台用不同密码，建议用密码管理器

软件安装：

只从官网/官方商店下载

浏览器插件不超过3个，且只用知名钱包插件

-   六、总结与感悟
    

Web3很酷，但风险也很大，法律红线别碰，网络安全别懒，钱包权限别乱给。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->














26.1.13 学习021以太坊学习教材第一章

-   核心总结
    

以太坊不是一个“单纯的数字货币”，而是一个全球可编程的区块链平台。它就像一台“世界计算机”，任何人都可以在上面写代码、跑应用、发资产，而且这台计算机不属于任何人，却能被所有人使用。

-   关键点
    

1.  以太坊是“可编程的区块链”，比特币只能记账转账，以太坊可以跑程序（智能合约）。 智能合约就像自动售货机：投币（付Gas）→ 选择商品（调用函数）→ 自动出货（执行逻辑），全程无需人工干预。
    
2.  以太币不只是“币”，还是“燃料” ETH 有三种核心作用:①支付Gas费 ②参与质押 ③DeFi/NFT的基石资产
    
3.  PoS（权益证明）已经取代PoW（工作量证明) 。2022年9月 The Merge 后，以太坊不再用矿机挖矿，改由质押ETH的人来验证交易。好处：能耗降低约99.95%，更环保，更适合普通人参与。
    
4.  以太坊和比特币的区别: 简单说，比特币是“价值链”，以太坊是“应用发动机”。
    

-   几个让我印象深刻的比喻
    

1.  Gas费：就像开车加油，路越堵（网络越忙），油越贵。
    
2.  DApp（去中心化应用）：像一家永不打烊的24小时自动商店，没有总开关，谁也关不掉。
    
3.  PoS质押：就像你押一笔保证金（32 ETH）当“陪审团代表”，诚实工作有奖励，作恶会被罚没。
    

-   疑问与思考
    

1.  虽然PoS更环保，但32 ETH（大概6万多美元）的质押门槛对普通学生来说还是很高，有没有“合伙质押”的办法？
    
2.  智能合约一旦部署就不可篡改，那如果发现漏洞怎么办？
    
3.  以太坊的“全球性”很好，但如果某个国家屏蔽了以太坊网络，用户还能访问吗？
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->















1.12 学习了Web3实习手册入门导读部分，对Web3基础概念和知识有了一定了解

![74C62B939E052BA7BE518B6EF3F8CFC7.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Eddie-534/images/2026-01-12-1768220950720-74C62B939E052BA7BE518B6EF3F8CFC7.jpg)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
