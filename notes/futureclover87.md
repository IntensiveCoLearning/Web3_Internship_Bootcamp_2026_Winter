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
# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->
Practice

1，Redmix Basic 一些些

2，ZK Vote
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->

周末水卡

1，投研分享会

2，X402分享会
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->


Solidity 101 2nd half

1, pure/view 函数

pure（纯函数） ：既不读取也不修改状态。完全依赖输入参数来计算结果。

view（视图函数） ：只读取状态（比如查询余额），但不修改状态。

payable ：表示函数可以接收以太币。加法运算不需要接收以太币。

2, Storage/Memory

Storage = 硬盘（更贵、永久、状态变量在此）。

Memory = 内存（更便宜、临时、赋值时常发生拷贝）。

引用：当你将 storage 赋值给本地 storage 指针时，它们指向同一个地址，改一个另一个也变。memory 赋值给 memory 同理。

拷贝：当你在不同的存储位置之间转换时（例如从 storage 到 memory，或者从 memory 到 storage），系统会创建一个独立的副本。修改这个副本（memory）的值，不会影响到原始的状态变量（storage）。

3, 映射

在 Solidity 中，映射（Mapping）只能存在于 storage 中。

原因：因为 Mapping 的实现原理是基于哈希查找的“稀疏存储”，它需要直接操作合约的状态存储空间。它无法在 memory（内存）或 calldata 中创建，因为内存和 calldata 的空间分配是连续的，无法容纳映射这种不确定容量且依赖哈希查找的结构。

4，常量/不变量

| 特性 | constant (常量) | immutable (不变量) |
| 初始化时间 | 必须在声明时初始化 | 可以在声明时，或在 constructor（构造函数）中初始化 |
| 确定值的时间 | 编译时 (Compile-time) | 部署时 (Deployment-time) |
| 支持的类型 | 仅限数值类型、string 和 bytes (字面量) | 仅限值类型 (如 uint, address, bool 等) |

看到 constant ：想它由于是“死”的，定义时必须给值。

看到 immutable ：想到它比较“灵活”，可以在部署时计算（比如存个 msg.sender），但它不喜欢复杂的 string。

看到\*\*“会变”\*\* 的数据：绝对不能加这两个词。

5，bytes长度

bytes1 是固定长度的字节类型，长度为 1 字节（8 位）。

一个字节（8 bits）在十六进制中用两个字符表示。因此，全 0 的 1 字节表示为 0x00

bytes1 →→ 0x00

bytes32 →→ 0x0000...00 (64个0)

6, 事件

Solidity中的事件（event）是EVM上日志的抽象，它具有两个特点：

响应：应用程序（[ethers.js](https://learnblockchain.cn/docs/ethers.js/api-contract.html#id18)）可以通过RPC接口订阅和监听这些事件，并在前端做响应。

经济：事件是EVM上比较经济的存储数据的方式，每个大概消耗2,000 gas；相比之下，链上存储一个新变量至少需要20,000 gas。

很多链上分析工具包括Nansen和Dune Analysis都是基于事件工作的。

7，父合约&子合约

Solidity 中子合约如何调用父合约函数的两种合法语法

| 调用方式 | 语法 | 理解 |
| 直接调用 | A.pop(); | “我要用 A 合约里的那个 pop 函数。” |
| super 调用 | super.pop(); | “我要用继承链里我后面那个人的 pop 函数。” |

在 Solidity 中，继承的顺序非常重要，必须遵循一个核心原则：“从基类到派生类” (Most base-like to most derived-like) 。

简单来说，就是 先写“辈分最高”的（父类），再写“辈分较低”的（子类） 

8，error/require/assert 命令

error是solidity 0.8.4版本新加的内容，方便且高效（省gas）地向用户解释操作失败的原因，同时还可以在抛出异常的同时携带参数，帮助开发者更好地调试。

必须搭配 revert ：在逻辑判断中，error 的唯一正确出场方式是配合 revert 语句。

require命令是solidity 0.8版本之前抛出异常的常用方法，目前很多主流合约仍然还在使用它。它很好用，唯一的缺点就是gas随着描述异常的字符串长度增加，比error命令要高。

assert命令一般用于程序员写程序debug，因为它不能解释抛出异常的原因（比require少个字符串）
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->



产品经理要做啥

eg产品方向：用bonding curve来拍卖NFT的功能

 交互界面： VI视觉（主题，风格，logo/吉祥物，颜色），组件，页面逻辑

 用户洞察：早鸟红利，交互-有趣，有关联，有意义；

 经济模型：Bonding Curve+ 其他价值（持有 NFT 才有分红、社交权限或游戏功能）

Bonding curve联合曲线，P=f(S)，增函数。

供应越多，价格越高，数学公式人为设定了“后来者必须为前人的风险和流动性买单”的激励机制。

没有卖家，只有“自动贩卖机”（智能合约）奖励早期的“风险承担者”& 确保流动性（随时能卖掉）

 当有人以高价买入 NFT 时，他们支付的资金会被锁定在合约的储备池（Reserve Pool）里。由于价格是上涨的，新买家进入时存入的钱，永远多于老玩家卖出时需要的钱。这保证了：无论何时你想离场，合约里永远有足够的钱把你的 NFT 买回来。

 Bonding Curve 并不是一个“大家都能赚钱”的保险箱，而是一个极其残酷的、全透明的实时博弈场：

流动性承诺：它承诺你“任何时候都能卖掉”，但不承诺你“以赚钱的价格卖掉”。

逃跑速度的竞争：当项目热度下降时，这变成了参与者之间比拼谁先点击“卖出”按钮的游戏。

 营销方式&渠道：

1，利用平台流量： [Pump.fun](http://Pump.fun) “上推”

首发视觉冲突：NFT 的图片必须第一眼就能抓住眼球（甚至可以是争议性的图片）。

评论区互动（Bumping） ：在平台上刷热度，让评论区飞速滚动。高频互动的项目通常会被推荐算法排在首页。

快速完成初段曲线：在冷启动的前 10 分钟，必须有一定的成交额。许多开发者会准备多个钱包进行自买自卖（Wash Trading），从而触发平台的“热门（Trending）”逻辑。King of the Hill，当一个代币/NFT 达到某个市值，它会自动被推到首页最显眼的位置，甚至直接上线 DEX（如 Raydium）。

叙事升级：一旦上了首页推荐，营销话术要立刻从“赚钱”转向“社区文化”，引导散户长期持有而不是立刻砸盘，以冲击最终的目标（如上线主流交易所）。

2，社媒传播：Twitter (X) 与 Telegram

Meme（模因）营销：NFT/代币起一个极具传播性的名字（如蹭热点新闻、名流、搞笑梗）。

KOL/Alpha 社区合作：联系加密货币领域的“喊单员”（Callers）。给他们提供早期低价入场的位置，利用他们的影响力（Alpha Groups）瞬间把曲线拉起。

机器人自动播报：建立 Telegram 频道，每当有人买入（尤其是大单）或者价格翻倍时，机器人自动发送带有“火箭”表情的财富播报，不断冲击用户视觉。

3， 节点奖励：裂变机制

推荐佣金（Affiliate）

4， 制造“公平启动”的叙事

无预留、无预挖（Fair Launch） ：强调开发者也得在曲线上和大家一起买。

权限丢弃：宣传合约已经审计且权限已丢弃，谁也无法撤走流动性（因为储备金锁在 Bonding Curve 合约里）。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->




Solidity 101-1st half

I, 整段十六进制 0x4d696e69536f6c69646974790000000000000000000000000000000000000000 可以看作：

1.  0x : 表示这是一个十六进制数。
    
2.  4d696e69536f6c6964697479 : 字符串 "MiniSolidity" 的内容（12 字节,24个十六制位）。
    
3.  0000...00 : 填充位（20 字节的零，40个十六制位），以凑满 bytes32 的长度。
    

字符串在变量中是以原始字节形式存在的。因为 bytes32 是定长的，所以内容不满 32 字节时，会在右侧补零对齐。这种存储方式比动态类型的 string 更节省 Gas（交易手续费），但缺点是它只能存储最多 32 个字符。

bytes4 包含 4 个字节, 4 字节×2 位/字节=8 个十六进制位 (1个字节由 2 个十六进制位组成, 16\*\*2=256，正好对应一个字节 0-255 的范围）

II, Solidity 的变量类型主要可以分为三类：数值类型（Value Types） 、引用类型（Reference Types） 和 映射类型（Mapping Types） 。其核心区别在于：数值类型在赋值或传参时总是直接拷贝值；引用类型则可以通过多个变量指向同一个存储位置。

**一、 数值类型 (Value Types)**

这类变量在赋值或传递时，会创建一个独立的副本。

1.  布尔型 (bool) : 取值：true 或 false。
    
2.  整型 (int / uint) :int: 有符号整数（支持正负）uint: 无符号整数（仅支持正数）
    
    -   步长以 8 为单位，如 uint8, uint16 ... 直到 uint256（uint 默认是 uint256）
        
3.  地址类型 (address) :
    
    -   address: 存储一个 20 字节的以太坊地址。
        
    -   address payable: 比普通地址多了 transfer 和 send 方法，可以接收转账。
        
4.  定长字节数组 (Fixed-size byte arrays) :
    
    -   从 bytes1 到 bytes32。例如 bytes32 常用于存储哈希值。
        
5.  枚举 (enums) :
    
    -   用户自定义的类型，用于表示一组离散的状态（如 enum Status { Open, Closed }）。
        
6.  函数类型 (Function Types) :
    
    -   可以将函数赋值给变量。
        

**二、 引用类型 (Reference Types)**

这类类型比较占用空间，因此需要显式指定数据的存储位置：storage（永久存储）、memory（内存占用）或 calldata（输入参数）。

1.  动态字节数组 (bytes) :
    
    -   不写数字的 bytes 是动态长度的，类似于 byte\[\]。
        
2.  字符串 (string) :
    
    -   动态长度的 UTF-8 编码字符串。
        
3.  数组 (arrays) :
    
    -   定长数组: 如 uint\[5\] 动态数组: 如 uint\[\]。
        
4.  结构体 (structs) :
    
    -   允许将几种不同类型的变量组合成一个自定义类型。
        

**三、 映射类型 (Mapping Types)**

-   mapping:
    
    -   格式为 mapping(\_KeyType => \_ValueType)。
        
    -   类似于哈希表或字典，常用于根据地址查询余额等场景：mapping(address => uint) public balances;。
        

**四、 全局变量 (Global Variables / Special Variables)**

虽然它们不是“用户定义”的类型，但在 Solidity 中随处可用，常用的包括：

-   msg.sender : 当前调用者的地址。
    
-   msg.value : 随交易发送的以太币数量（wei）。
    
-   block.timestamp : 当前区块的时间戳。
    
-   block.number : 当前区块号。
    

**总结**

| 类型 | 特点 | 例子 |
| 数值类型 | 总是深拷贝值，简单直接 | uint, bool, address, bytes32 |
| 引用类型 | 传递引用/指针，需指定存储位置 | string, uint[], struct |
| 映射 | 键值对存储 | mapping(address => uint) |

注意： Solidity 中没有浮点数类型（float/double），所有涉及到小数的运算通常通过放大倍数（如使用 uint 配合 18 位精度）来处

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-21-1768988451517-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-21-1768988436510-image.png)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->





Ethernaut 测试笔记

Fallback

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-20-1768917903777-image.png)

要触发 owner = msg.sender（即夺取所有权），你需要满足两个条件：

1.  发送的金额大于 0 (msg.value > 0)。
    
2.  你当前的贡献值大于 0 (contributions\[msg.sender\] > 0)。
    

一旦你成为了 owner，你就拥有了执行 withdraw 函数的权限：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-20-1768917987815-image.png)

如果你在浏览器控制台中操作，命令如下：

1.  进行初始贡献：  
    await contract.contribute({value: toWei("0.0001")})
    
2.  直接转账触发 ownership 转移：  
    await contract.sendTransaction({value: toWei("0.0001")})
    
3.  检查你是否已成为 owner (可选)：  
    await contract.owner() // 检查返回地址是否是你自己的地址
    
4.  提款以清空余额：  
    await contract.withdraw()
    

Fallout

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-20-1768918005297-image.png)

这个关卡（Ethernaut Level 2: Fallout）的核心漏洞是一个拼写错误（Typo） 。

-   合约名称是：Fallout 函数名称是：Fal1out
    
-   结论：任何人只要调用 Fal1out() 函数，就能成为合约的 owner。
    

 打开 [Remix IDE](https://remix.ethereum.org/)。

1.  新建一个文件，命名为 Fallout.sol
    
2.  点击 Compile Fallout.sol。
    
3.  点击左侧的 Deploy & Run Transactions（部署与运行）图标。
    
4.  在 Environment（环境）下拉菜单中，选择 Injected Provider - MetaMask。
    
5.  找到 At Address 输入框, 复制你当前关卡实例的地址（Instance Address）。点击 At Address 按钮。
    
6.  在 Remix 底部展开加载出来的 FALLOUT 合约。找到黄色按钮 Fal1out,点击 Fal1out 按钮
    
7.  MetaMask 会弹出交易确认，点击确认。
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-20-1768918040567-image.png)

Or 控制台

await contract.Fal1out({value: toWei("0.0001", "ether")})
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->






0基础程序小白自学笔记-Hardhat/Foundry/Remix 教程案例跑通

1， 窗口调用

Git or VS code Terminal, Terminal 更user-friendly一些，Git进去每次要重新配置，Terminal可以多窗口

2，版本

node js 版本（目前最新的是24，但是测试版容易不兼容，20版本太低，新版hardhat不支持，可以使用22，直接调用指令 nvm use 22；如果系统有更早版本的，建议删掉避免不兼容)

Hardhat 2.0（虽然第 3 版是推荐，但目前小白教程都是基于第 2 版写的。避免新旧版本差异导致报错。）

3,  常见错误

bash: syntax error near unexpected token 'newline'，命令没输完就换行了，或者在命令末尾多输入了一个符号

复制粘贴指令 留意有没有换行，有没有去掉<>，都会无法被识别

报错后可以看下提示，有时候解决方案就在提示里面，eg Foundry 里面强制 broadcast， npx hardhat --init 强制初始化命令

5，端口占用

Git Bash 虽然关闭了，但它在后台留下了一个“僵尸进程（Ghost Process）”。即使你关闭了窗口，那个 node.exe 依然死死霸占着端口不放。

暴力清除

taskkill /F /IM node.exe

6，结束程序

关闭 Hardhat 节点时，先在终端里按 Ctrl + C ，等它显示出原来的命令提示符（比如 PS C:\\...>）后再关闭窗口，这样最干净

7, 复制粘贴

快捷键-Git Bash 里面的粘贴是 Shift+Insert, 复制是Ctrl+Insert, 避免用Ctrl+C，是退出任务执行

用原生记事本粘贴指令，避免用其他notebook之类会有其他格式带入命令

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-19-1768818439814-image.png)
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->







今天看了Vitalik和ETHPanda的访谈，了解了一些新的概念 RPGF/Zuzala/MPC&FHE

1 , RPGF 的全称是 Retroactive Public Goods Funding，即追溯性公共产品资助

先建设，后拨款

以 Optimism Collective（最早大规模实践RPGF的生态）为例，一轮完整的RPGF通常包含以下阶段：

1.  成果周期：一个预定的时间段（如一个季度或一年），供建设者自由创造。此期间没有任何资金承诺。
    
2.  提名与申请：周期结束后，开放提名窗口。社区可以提名自己或他人的成果（需提供证明，如GitHub仓库、使用数据链接）。
    
3.  徽章持有者评审：由一组经社区选举或委任的、代表不同领域的专家（“徽章持有者”）对提名项目进行审议、分类和打分。
    
4.  投票与分配：更广泛的代币持有者社区根据评审结果进行投票，最终按票数比例分配事先准备好的资金池。
    
5.  发放资金：奖励以生态系统的原生代币（如OP）形式分发给获奖项目。
    

除了 Optimism，以太坊的Protocol Guild、Celo 等生态也在积极探索各自的RPGF模式

| 概念 | 核心理念 | 资助逻辑与时机 | 关键特点 | 典型代表 |
| RPGF(追溯性公共产品资助) | “奖励已验证的成果” | 先有成果，后有奖励。项目在证明其价值后获得追溯性奖励。 | 1. 成果导向2. 社区驱动投票3. 旨在解决公共产品融资不足 | Optimism Collective |
| 传统VC/拨款 | “投资未来的潜力” | 先有承诺，后有资金。基于团队的商业计划、路线图和未来前景进行投资。 | 1. 前景导向2. 中心化决策3. 追求财务回报 | 红杉资本、各类基金会 |
| Gitcoin Grants(二次方融资) | “用民主匹配支持公共产品” | 边建设，边筹款。项目在进行中通过社区的小额捐赠来获得匹配资金。 | 1. 偏好聚合2. 抗垄断（小额捐赠权重高）3. 依赖前期筹集的匹配池 | Gitcoin平台上的公益轮 |
| DAO 社区拨款 | “用社区国库资助提案” | 先有提案，后有资金。项目提交未来工作的详细提案和预算，经治理投票后获得资金。 | 1. 提案导向2. 治理投票决定3. 资金用于未来工作 | Uniswap Grants, Aave Grants |

2, Zuzala项目 

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-18-1768730539308-image.png)

创新项目的孵化器：许多重要的加密项目在此孕育或获得关键推动，例如：

-   Privacy Pools：一个将合规与隐私结合的突破性ZK协议。
    
-   DeSci（去中心化科学）：推动了长寿科学和生物医学研究的去中心化协作模式。
    
-   ZK身份凭证：加速了相关工具的开发和应用。
    

文化现象与后续影响：Zuzalu催生了“Zuzalu文化”，其参与者成为加密社区中重要的思想节点。其模式也被后来者借鉴，例如后续出现的 “Zuconnect”、“Zuzalu Istanbul” 等活动，形成了一个松散的“Zuzalu宇宙”。

 3, MPC& FHE

MPC 安全多方计算Secure Multi-Party Computation

 关注的是 “过程隐私”：如何让多个互不信任的方，在不暴露各自私密输入的情况下，共同完成一项计算。

1\. 核心思想：

MPC允许多个参与者（如不同公司、机构）基于各自的私密数据（如销售额、用户列表、基因组数据）共同执行一个计算函数（如求平均值、数据比对、联合建模），并在计算结束后，每个参与者只能得到约定的输出结果，而无法窥探其他任何参与方的原始输入数据。

2\. 一个经典例子（姚氏百万富翁问题）：

两个富翁想知道谁更富有，但都不想透露自己的具体财富。通过MPC协议，他们可以在不披露具体金额的情况下，只得出“甲比乙富”或“乙比甲富”的结论。

3\. 主要应用场景：

-   联合风控：多家银行在不共享客户敏感数据的前提下，共同构建反欺诈模型。
    
-   密钥管理：将一个私钥拆分成多个分片，由不同方持有，需要超过阈值数量的分片才能联合签名（避免了单点故障和单点泄露风险）。这正是许多MPC钱包的核心原理。
    
-   隐私数据查询：在不暴露查询方条件和数据库内容的情况下，完成查询（如“我是否在黑名单中？”）。
    
-   保密竞价：多个买家出价，最终只公布中标者和价格，而不泄露其他出价。
    

 FHE 全通太加密Fully Homomorphic Encryption

关注的是 “状态隐私”：如何让一个不可信的第三方（如云服务器），直接对加密数据进行计算，而无需解密，且得到加密的结果。

1\. 核心思想：

FHE允许对加密后的数据（密文）直接进行任意复杂的代数运算（加、减、乘、除等），运算得到的结果仍然是加密的。当结果被拥有密钥的人解密后，得到的结果与对原始明文数据进行同样计算的结果完全一致。

2\. 一个简单比喻：

假设加密是给数据加上一把锁。普通加密只能“存储和传输”带锁的数据，要计算必须先开锁（解密）。而FHE允许你直接对带锁的数据进行加工，加工完成后，数据仍然带着锁。只有拥有钥匙的人打开锁后，才能看到加工后的成品。

3\. 主要应用场景：

-   隐私云计算：将加密的敏感数据（如医疗记录、财务信息）上传至公有云，云服务商可以在不解密的情况下直接进行数据分析或机器学习模型训练，最后返回加密的结果给数据所有者。
    
-   保护隐私的区块链：实现智能合约对加密数据的处理，实现完全保密的交易（如隐藏转账金额、合约状态）。
    
-   安全外包计算：将复杂的计算任务外包给算力强大的第三方，同时确保第三方无法获知原始数据和最终结果。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->








今日学习 交易实践+Remix 环境准备+AI 回放课

交易实践

1,加速交易-从地址1到地址2的交易，如果设置了low gas fee，则需要等待很长时间的打包，可以同时并行完全一样的交易（hash不同）设置更高的gas fee，在etherscan上可以同时查到这两笔交易

2, metamask设置reset account，可以清除所有pending的record，并不是把账户重置

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-16-1768565027239-image.png)

3, 取消交易-用一笔nonce 相同的但是无意义的交易eg自己给自己传0eth (but metamask advance上暂没有找到 nonce设置）

 4, etherscan interface上可以用dai smart contract写入transfer

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-16-1768565095407-image.png)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/futureclover87/images/2026-01-16-1768565113751-image.png)
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->









ENS的应用 [app.ens.domains](http://app.ens.domains)

1，ENS作为user-friendly的应用，可以简化地址的复杂性，可以花费少量eth获得一个expected duration的ENS

2，每个ENS有一个manager和owner，初始应该都是自己的ENS，看到owner有指向其他ENS的

3，reverse transaction-反向解析 具体步骤

a, 在“我的账户”或“反向记录”部分，找到你的地址。

b,在下拉菜单中，选择你拥有的一个ENS域名（例如 myname.eth），然后点击“保存”或“设置”。

c,支付一笔Gas费来完成这笔链上交易。

4, 反向解析作用

a, 一对多关系的处理：一个地址可以拥有多个ENS域名，但反向解析只需要指向一个“主”域名用于显示。这个设置让地址所有者自己来指定哪个是“主域名”。

b, 你可以在不转移域名所有权的情况下，随时更改反向记录，指向你拥有的另一个域名
<!-- DAILY_CHECKIN_2026-01-15_END -->

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
