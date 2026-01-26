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
# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->
# 总览

Vibe coding 一个DApp——Done

# 详情

Vibe coding 一个DApp——[Create Next App](https://polymarket-dashboard-gamma.vercel.app/)
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->


# 总览

完成入门技术的3个任务：

Hardhat——Done

Foundry——Done

ERC标准10个，并且总结成笔记——pending

链上留言板——Done

# 详情

### 链上留言板

合约——已经通过 Remix 部署到 Sepolia 并且验证

前端——git clone到本地，然后部署到 github page 上即可

在页面上配置好合约地址和钱包即可。

前端代码里主要由三个部分组成：： HTML（骨架） 、 CSS（皮肤） 、 JavaScript（逻辑）。

HTML：设置这个网页的基本信息，框架，编码方式等。

CSS：各种样式，包括颜色形状特效等，

JS：交互方面的，input和output等。

具体一点，样式<style>一般放在<head>这个部分，可以被下面的<body>引用。<script>就是交互的内容，
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->






# 总览

学习一下dAPP开发——今天学了一会前端

# 详情

学了一下如何利用 rainbowkit 调用钱包

rainbowkit 前一层是wagmi再前一层是viem

利用如下命令在目标项目配置环境

```
npm install @rainbow-me/rainbowkit wagmi viem@2.x @tanstack/react-query
```

```
// 伪代码：这就是 Next.js 渲染时的逻辑
<_app.tsx>
  {/* 这里是公共配置，比如 RainbowKit */}
  
  <index.tsx>
    {/* 这里是首页的具体内容 */}
    <Header />
    <main>欢迎来到我的第一个 DApp</main>
  </index.tsx>

</_app.tsx>
```

\_app.tsx 是配置类，index 也就是一般说的首页，可以在首页里放置我需要的某个部件，比如页面的 Title ，或者是链接钱包的按钮。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-25-1769351545043-image.png)
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->








# 总览

开了个会，尝试加入LXDAO。

今日休息~
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->










# 总览

根据手册先做运营任务再做进阶技术，拷打 Austin，写总结。

-   Figma 安装或在线创建账号——Done
    
-   完成专业文章排版——Done，让豆包帮忙排了然后审阅了下，应该没什么问题。
    
-   Figma 实战任务——Done，下了两个字体后轻松完成。
    
-   完成一次ZK Vote——Done，那天分享会完了后都快忘完了，这个第一次ZK Vote帮我回想起了很多。
    

# 详情

## 周总结

[https://x.com/Moxan1st/status/2014651451872563539?s=20](https://x.com/Moxan1st/status/2014651451872563539?s=20)

细化了一下今天晚上的汇报内容，把关键内容总结成了推文。

[https://docs.google.com/presentation/d/1\_lO4oddayAnZkJNBzbQkB-uH9DHyv3a9fd7T2VnvQBE/edit?usp=sharing](https://docs.google.com/presentation/d/1_lO4oddayAnZkJNBzbQkB-uH9DHyv3a9fd7T2VnvQBE/edit?usp=sharing)

## ZK Vote

解决的是隐私问题，个人总结：

在链上的投票（可以引申到其它类型的交易）身份和投票内容，可以通过本地哈希方式进行加密。

而我投票交互的合约地址也并不是实际的投票地址，而是一个处理中心。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->














# 总览

-   完成挑战 Challenge #0 - Tokenization——Done
    
-   通过 Ethernaut 前 3 关——Done
    
-   安装 Figma——暂未
    
-   计划三天的学习内容——Done
    
-   良心杀治理建议——Done，并且花精力调研和写思路了。
    
-   准备两个问题给 Austin Griffith
    
-   周五汇报的 PPT, 顺便排练一下——Done，时长大概8分半
    

# 详情

## 挑战 Challenge #0 - Tokenization

根据官方的代码直接部署，直接可用，我哭了。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-22-1769092842423-image.png)

## 通过 Ethernaut 前 3 关

第一次通过 F12 跟页面互动，攻防挑战还挺好玩，本质就是找代码逻辑的漏洞。

## 计划三天的学习内容

周五：根据手册先做运营任务再做进阶技术，拷打 Austin，写总结。

周六：学学 uniswap，想想黑客松的创意

周天：查漏补缺

## 良心杀治理建议

根据脑袋里面的认知发散了一下，在定量和定性分析后分配资金，差点忘了还要找狼人。

感觉狼人的目的已经被发现了。[https://forum.lxdao.io/t/xiaohai-x-draken-draft-s9/3396/7?u=moxan1st](https://forum.lxdao.io/t/xiaohai-x-draken-draft-s9/3396/7?u=moxan1st)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-22-1769093348581-image.png)

## 汇报的 PPT

内容上跟 xiaohai 分享的有重叠，当个乐子逗大家笑吧。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-22-1769093382419-image.png)

## 准备两个问题给 Austin Griffith

1.I'm a new comer for solidity coding. When I was deploying the challenge 0, there were several options including Foundry and Hardhat. I chose Foundry, and i found YourContract.sol and the website were crude compared with Hardhat. Did you designed it for advanced coder or it is waiting to be completed?

2.Personally what do you do for a living?
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->

















# 总览

-   完成挑战 Challenge #0 - Tokenization
    
-   通过 Ethernaut 前 3 关
    
-   安装 Figma
    
-   计划两到三天后的学习内容
    

# 详情

## 挑战 Challenge #0 - Tokenization

今天往框架里写合约。Gemini 建议我从 ERC-20 开始写，并且避免重复造轮子。我先学会调用吧。

`totalSupply` = 这个 ERC20 代币在链上“一共存在多少个单位”

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-21-1768958350880-image.png)

铸造、转账和销毁都会经过 \_update 函数，这个函数目的是**更新账本中客户的余额以及代币总数。**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-21-1768961936374-image.png)

虽然销毁的时候，代币转向的是 address(0)，但是

1.  账本并没有记录 balance(address(0))
    
2.  **代码路径从来不会给它赋值**
    

**Scaffold-ETH 2** 有一个核心组件叫 `ContractUI`它会**遍历** ABI 里的每一个函数。如果发现是 `view` 函数，就渲染一个“Read”显示框；如果发现是 `public` 或 `external` 函数，就渲染一个带“Send”按钮的表单。这样的话部署完合约就会有一个可用的前端网页提供测试。

在部署了 ERC-20 后，出现的问题：

-   打开的网页右上角并不是我部署这个合约所使用的EOA的地址。因此如果需要转账，要先去日志里面找到部署者的地址和密钥，通过这个来发起第一笔转账。
    
-   转账的时候使用的账户必须有测试 ETH，否则不能满足 Gas 要求。也就是说在这个链上用的依旧是 ETH 作为 Gas 代币。
    

### 部署到 Sepolia

需要保证钱包密钥是准确的，可以通过查看钱包地址反推密钥是否正确。

在这个前提下保证 sepETH 充足。

部署成功后在 Metamask 中添加合约地址即可查看代币了。

### 前端上网

利用 Vercel 托管项目，可以实现自动化部署，还有免费的域名。

### 挑战提交

需要提交前端网页和合约地址网页

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-21-1768985854906-image.png)

### 周五分享内容

学习方法：双 AI 问答辅助代码学习。

-   回答我们的疑问：帮助我们理解问题，无论是开放性的还是确定性的。
    
-   索取建议：学习方向的抉择不定时，学习建议。
    
-   辅助目的：帮忙配环境，分析 BUG 。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->



















# 总览

-   提交 Gas 优化案例——Done
    
-   完成挑战 Challenge #0 - Tokenization——还在写
    
-   安装 Figma——没干
    
-   计划两天后的学习内容——留给明天
    

# 详情

## Gas优化

### 核心概念：Storage vs. Stack

在 EVM 执行指令时：

-   **Storage (存储)**：像电脑的“硬盘”，数据永久保存在区块链上。读写一次都要好几百甚至上千 Gas。
    
-   **Stack (栈)**：像电脑的“寄存器”，函数运行时的临时计算空间。操作一次只需要 3 Gas。
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-20-1768880447159-image.png)

所以省 Gas 的一个关键思路就是减少对 Storage 的写操作

Remix环境下部署三种方案，测试结果如下

Normal: 0x72fa8f163b3065f3074de108412cd12e22a9db037511c2886d67323dc2655197

Cached: 0x2ef9a8d7c82b8ccb61a1465eeddb63c1cd1bdf362511fe6bfc8217efc8ef0e37

Unchecked: 0x407884377de7e5cababcfc3d90762810cfabc68860b2ed927de2c6e48aaa1ca1

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-20-1768893016323-image.png)

## 漏洞修复案例

### 案例1 重入

mapping 用作账本映射。`mapping(address => uint256) public balances；` 非常常见。

`msg.sender.call{value: amount}("");`

**危险的核心就在这个** `.call` **带来的“控制权移交”：**

1.  当你执行这行代码时，EVM 会暂停合约。
    
2.  EVM 跳到 `msg.sender` 的地址去执行代码（如果是合约账户）。
    
3.  如果对方是个恶意合约，它的代码里写着：`再次调用合约的 withdraw()`。
    
4.  合约还没跑到 `balances[msg.sender] = 0` 呢，就被迫重新开始了。
    

解决：

1.  检查-生效-交互模式（CEI Pattern）：执行顺序发生改变。
    
2.  重入锁（Reentrancy Guard）：有个保镖守着，只能按流程顺序访问。
    

### 案例2 **访问控制**

前面的人存都是正常的，直到有第一个人取，它马上就被付给msg.sender了。

解决：在敏感操作（转账）的时候注意权限管理。

### **案例3 整数溢出防护**

例子里面的2^256 - 1实际是很大的数，是不是可以考虑修改为比较现实的例子。

解决：限制上限 + 使用最新编译器

使用最新编译器的时候，Counter 超过2^256 - 1的时候会报错，交易回滚。

## 挑战 Challenge #0 - Tokenization

由于没有用 Hardhat，利用 Gemini 辅助配置了这个挑战的环境。

并且遇到了一些问题，最大的意外：子模块依赖丢失 (Git Submodules)

-   **现象：** 报错提示找不到 `forge-std/Vm.sol` 或 `Script.sol`。
    
-   应对：**手动克隆：** 用 `git clone` 强行把库下载到 `lib`。**重写地图：** 手写 `remappings.txt` 告诉编译器“不要去网上找了，看我本地的文件夹”。
    

部署了环境，ERC-20还在写......  

## 周三计划

-   完成挑战 Challenge #0 - Tokenization
    
-   通过 Ethernaut 前 3 关
    
-   安装 Figma
    
-   计划两到三天后的学习内容
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->























# 总览

### 提交任务

-   设置开发环境并熟悉 Remix IDE——Done
    
-   在 remix 中运行 Solidity by Example | 0.8.26 Basic 部分的代码——Done
    
-   部署合约到 Sepolia 测试网——Done
    
-   优化 Web3 实习计划官方平台——Done，见1月18日笔记
    
-   完成一个 Gas 优化案例
    

# 详情
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->


























# 总览

1.整理 week 2 中已完成的任务，明天直接提交：

-   Remix 环境
    
-   Gas 优化任务
    

2.规划第二周细节，计划每日 To-do list（周一周二）

3.确定一个 Web3实习计划 官方平台的痛点，分析和寻找改进方案，提交给助教。

# 详情

Web3 实习计划官方平台 周折叠和学分图表改进建议：

## 背景

-   当前平台课程安排模块所有周次任务平铺展示，用户需频繁滚动页面查找目标周任务，操作效率低：比如我刚学到第二周，每次需要滚动到最下面才能看到具体的学习内容。
    
-   仅以单一数值展示学分情况，无法直观知晓各类学分的获取进度。保留课程任务等全部原有内容的前提下，通过轻量化交互优化，可在不改变核心内容的基础上提升用户操作体验。
    

## 目标

-   降低用户查找周任务的操作成本，通过周维度折叠功能减少无效滚动，提升任务定位效率；
    
-   直观展示学分构成，让用户清晰掌握各类学分的完成情况。
    

## 方案

模块 1（周折叠）：对 “课程安排” 按周做折叠处理，示意图中仅折叠第 0 周，保留周标题核心信息，点击可展开 / 折叠，不改动其他内容布局。

模块 2（学分图表）：新增条形图展示学分构成（打卡 200/300、作业提交 150/250、挑战完成 100/200），图表嵌入现有页面，完全保留课程任务等全部原有内容

## 预期结果

操作层面，周折叠功能减少用户滚动页面的频次，大幅缩短查找目标周任务的时间，提升核心操作效率；

体验层面，条形图让学分构成可视化，用户可快速知晓各类学分完成进度。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-18-1768738163121-image.png)

## 第二周的每日 To-do list

### 周一

-   设置开发环境并熟悉 Remix IDE
    
-   在 remix 中运行 Solidity by Example | 0.8.26 Basic 部分的代码
    
-   部署合约到 Sepolia 测试网
    
-   优化 Web3 实习计划官方平台
    
-   完成一个 Gas 优化案例
    

### 周二

-   完成挑战 Challenge #0 - Tokenization
    
-   通过 Ethernaut 前 3 关
    
-   安装 Figma
    
-   计划两到三天后的学习内容
    

## ERC-7962分享会

**主要应用场景示例**：  
星巴克会员与机场合作的案例 - 用户可以向机场证明自己是星巴克会员，但不泄露具体身份信息。商家（如星巴克）可以代付gas费用，用户无需持有钱包即可完成链上交易。

**技术原理**：

-   使用哈希算法实现零知识证明，用户持有原始值X，对外只展示哈希值Y
    
-   交易是一次性的，不可篡改和重放
    
-   在区块链浏览器上显示的是公钥，但无法追溯到真实身份
    

**主要问答内容**：

1.  隐私与透明的平衡：零知识证明从数学层面保证隐私不被泄露
    
2.  Gas费支付：商业场景中商家愿意代付gas费以实现商业目的
    
3.  与其他ZK技术的关系：ERC-7962使用简单的哈希方式，适合"是或否"的验证场景
    
4.  用户体验：用户无需管理钱包，交互体验类似Web2
    
5.  监管问题：技术本身是解决现有隐私泄露问题，而非制造新问题
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->































# 总览

-   Solidity 101——做题感觉自己有学到东西，查漏补缺很关键
    
-   总结全周内容，转化为推特文章，写第一篇推特稿件
    

# 详情

## 推特稿件

[https://x.com/Moxan1st/status/2012530632543641972?s=20](https://x.com/Moxan1st/status/2012530632543641972?s=20)

1.  总结精华内容的层级，分为三个部分：概念梳理→工具实操→生态思考
    
2.  根据以上结构加入学习心得的细节，增强活人感。
    
3.  在最后利用官方图片引流，假装自己是大V。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->

































# 总览

-   Remix中的例程学习Ballot——细节还挺多的
    
-   评估一下Ethereumspeedrun是不是必要用foundry过一遍——没干
    
-   学习一下ERC-20，写一个简化版的——去了一下午医院，全然没有学捏
    

# 详情

### Ballot例程学习

-   定义了Voter、Proposal结构体
    
-   为什么 voters 用 mapping，而 proposals 用 array？
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Moxan1st/images/2026-01-16-1768528945006-image.png)
-   proposalName指的是投票的选项，而不是议题的选项，这是一个单议题多选项的投票合约。
<!-- DAILY_CHECKIN_2026-01-16_END -->

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
    
-   Q：实际发生了什么？A：EVM在调用这些函数的时候，实际是先利用函数选择器keccak256(“setNumber(uint256)”)\[:4\]，计算出对应的数值取前面4个字节，一开始定义它的时候也计算到同样的数值，当这个数值匹配上，就跳到number()对应的代码段。然后calldata = selector + 参数编码。
    

```
cast send 0x02343bFb4CE8E6E5Add0D95562187787Fd5Ce2ec "increment
()" --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY
```

链发生变化了，会有gas，需要签名
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
