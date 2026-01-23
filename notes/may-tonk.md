---
timezone: UTC+8
---

# may-tonk

**GitHub ID:** may-tonk

**Telegram:** @maytonk

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->
# 一、**钱包 ≠ 账号**

这是 Web3 新手**最常见、也最危险的误解**。

* * *

## 1️⃣ Web2 的“账号”是什么？

在 Web2 中：

-   账号 = 身份
    
-   账号 = 权限
    
-   账号 = 状态
    

服务器：

-   记录你是谁
    
-   记录你登录没
    
-   决定你能干什么
    

你“登录成功”，是**服务器认可了你**。

* * *

## 2️⃣ Web3 根本没有“账号系统”

在 Web3 中：

> **链上没有账号概念，只有地址和签名**

-   地址只是一个 **公钥的映射**
    
-   没有“登录态”
    
-   没有“我记住你了”
    

钱包真正做的只有三件事：

1.  管私钥
    
2.  用私钥签名
    
3.  把签名结果交给你或链
    

👉 **钱包不是“你的身份”，而是“你的钥匙串”**

* * *

## 3️⃣ 为什么“地址 ≠ 登录态”这么重要？

因为这意味着：

-   你无法“踢用户下线”
    
-   你无法“封号”
    
-   你无法“找回密码”
    
-   你无法“撤销签名”
    

**只要用户持有私钥，他永远有完整权限**

* * *

## 必须形成的认知

> **DApp 不是在“管理用户”，  
> 而是在“验证签名是否有效”。**

* * *

# 二、三种交互方式的本质区别（非常重要）

你看到的 `ethers.js` / `web3.js` API 名字只是表象，  
真正的区别在于：**有没有消耗共识资源**。

* * *

## 1️⃣ read（call）：**读取状态，不是交易**

### 它在做什么？

-   在本地节点“模拟执行”
    
-   不进入 mempool
    
-   不上链
    
-   不消耗 gas
    
-   不需要签名
    

👉 本质是：

> **“如果这笔交易现在执行，结果会是什么？”**

* * *

### 特点总结

-   任何人都能调用
    
-   不会失败（除非逻辑 revert）
    
-   结果是“当前区块高度下的快照”
    

* * *

### 常见误区

❌ 以为 call 是“调用合约”  
✅ 实际上是“模拟执行合约代码”

* * *

## 2️⃣ write（sendTransaction）：**真正的链上行为**

### 它在做什么？

-   钱包弹窗
    
-   用户确认
    
-   使用私钥签名
    
-   交易广播
    
-   进入 mempool
    
-   被打包进区块
    
-   状态发生变化
    

👉 本质是：

> **“请求整个区块链网络修改世界状态”**

* * *

### 非常重要的现实点

-   用户**付 gas**
    
-   可能失败
    
-   可能被抢跑
    
-   可能被回滚
    
-   可能长时间 pending
    

* * *

### 为什么“确认交易 ≠ 成功”？

因为：

-   用户确认 ≠ 被打包
    
-   被打包 ≠ 执行成功
    
-   执行成功 ≠ 不会被 reorg
    

* * *

## 3️⃣ signMessage：**不上链，但极其重要**

这是 Web3 世界**非常独特的能力**。

### 它在做什么？

-   钱包签一段任意消息
    
-   不产生交易
    
-   不消耗 gas
    
-   不改变状态
    

👉 本质是：

> **“证明：这个地址的私钥持有人，认可这段内容”**

* * *

### 它真正的用途

-   登录（Sign-In with Ethereum）
    
-   绑定链下账号
    
-   授权 API
    
-   防女巫（证明控制权）
    
-   Off-chain 投票
    

* * *

### 极其重要的一点

> **签名 ≠ 同意转账**

这就是为什么钓鱼项目会：

-   诱导用户签 message
    
-   再在别的地方复用签名
    

* * *

## 你必须形成的分类直觉

| 行为 | 是否上链 | 是否花钱 | 是否永久 |
| --- | --- | --- | --- |
| call | ❌ | ❌ | ❌ |
| sendTransaction | ✅ | ✅ | ✅ |
| signMessage | ❌ | ❌ | ❌ |

* * *

# 三、一个真实 DApp 的完整用户流程（重点）

现在我们把所有东西**串成一条真实时间线**。

* * *

## 1️⃣ 用户打开 DApp

前端做的事：

-   检测是否有钱包插件
    
-   检测当前链 ID
    
-   检测是否已有授权地址
    

**注意：**

> 前端不能“自动连接钱包”，必须用户主动

* * *

## 2️⃣ 用户连接钱包

发生了什么？

-   钱包返回一个地址
    
-   **没有任何签名**
    
-   **没有任何链上操作**
    

👉 这一步只是：

> **“钱包告诉你：我可以代表这个地址”**

* * *

## 3️⃣ 前端读取链上状态（call）

前端会：

-   查询余额
    
-   查询 NFT
    
-   查询合约状态
    
-   查询授权情况
    

👉 所有这些，都是 read，不花钱。

* * *

## 4️⃣ 用户发起操作（写操作）

比如：

-   mint NFT
    
-   swap
    
-   stake
    
-   approve
    

流程是：

1.  前端构造交易数据
    
2.  钱包弹窗
    
3.  用户确认
    
4.  签名
    
5.  广播交易
    

* * *

## 5️⃣ 等待确认（这是最容易被忽略的一步）

真实情况是：

-   交易可能 pending 很久
    
-   交易可能失败
    
-   交易可能被抢跑
    
-   交易可能被重组
    

👉 **前端不能假设“用户点了就成功了”**

* * *

## 6️⃣ 前端监听事件 / 轮询状态

成熟 DApp 会：

-   监听合约事件
    
-   或轮询链上状态
    
-   或结合 indexer（The Graph）
    

👉 **UI 更新 ≠ 交易发起  
UI 更新 = 链上状态确认**
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->

# 一、第一条规则：**链 ≠ 以太坊**

很多人潜意识里以为：

> Web3 = 以太坊主网

这是**完全不够的**。

* * *

## 1️⃣ 什么叫“链”？

在 Web3 里，一条“链”本质上是：

> **一套共识规则 + 状态机 + 执行环境 + 经济参数**

也就是说：

-   EVM 只是“执行环境”
    
-   以太坊只是**其中一条实现**
    

* * *

## 2️⃣ L1 与 L2 的本质区别（不是“快不快”）

### L1（以太坊主网）

-   安全性最高
    
-   去中心化最强
    
-   Gas 极贵
    
-   状态最“庄重”
    

适合：

-   大额资产
    
-   底层协议
    
-   长期价值结算
    

### L2（Arbitrum / Optimism / opBNB 等）

-   把执行“搬到链下”
    
-   把结果“提交回 L1”
    
-   安全性来自 L1，但不是等价
    
-   用户体验更好，成本更低
    

👉 **L2 的本质不是“替代以太坊”，而是“扩展以太坊”**

* * *

## 3️⃣ 不同链 ≠ 换 RPC 这么简单

对 DApp 开发者来说，不同链意味着：

-   **Gas 模型不同**
    
-   **确认时间不同**
    
-   **用户心智不同**
    
-   **生态成熟度不同**
    
-   **攻击成本不同**
    

举个现实例子：

-   你在以太坊主网写一个有漏洞的合约  
    → 攻击成本极高，黑客更谨慎
    
-   你在低成本 L2 / 侧链  
    → 自动化脚本疯狂扫漏洞
    

👉 **链的选择，本身就是安全设计的一部分**

* * *

## 你要形成的认知

> **链不是“部署地点”，而是“规则环境”**  
> 换链 ≈ 换一套社会制度

* * *

# 二、第二条规则：**合约不是部署完就结束**

这是 Web3 和 Web2 最大的心智差异之一。

* * *

## 1️⃣ Web2 的默认逻辑

-   Bug → 修代码 → 上线
    
-   用户误操作 → 改数据库
    
-   出问题 → 运维兜底
    

* * *

## 2️⃣ Web3 的残酷现实

> **合约一旦部署，逻辑不可更改，错误永久存在**

这意味着：

-   没有“回滚数据库”
    
-   没有“管理员帮你改余额”
    
-   没有“客服补偿转错的钱”
    

* * *

## 3️⃣ 所以一个“上线中的合约”必须做什么？

### （1）合约验证（Verification）

不是为了炫技，而是为了：

-   让用户信任你
    
-   让安全工具分析你
    
-   让社区监督你
    

**未验证合约 = 高风险项目**

* * *

### （2）异常监控（Monitoring）

现实中你必须关心：

-   是否出现异常大额转账
    
-   是否出现异常频繁调用
    
-   是否有人在探测边界条件
    

成熟项目会：

-   监听事件
    
-   监听失败交易
    
-   对可疑行为预警
    

👉 **Web3 项目不是“发布”，而是“运行”**

* * *

### （3）处理用户误操作（但不是“帮他改”）

你不能改链上状态，但你可以：

-   设计更安全的交互
    
-   防止明显误操作
    
-   提供明确风险提示
    
-   在合约层限制危险行为
    

比如：

-   禁止转到 `address(0)`
    
-   禁止明显异常参数
    
-   提供撤销窗口（如果可能）
    

* * *

## 你要形成的认知

> **合约上线 ≠ 项目完成  
> 而是项目开始承担责任**

* * *

# 三、第三条规则：**Token / NFT 是经济系统，不是代码接口**

这是**最容易被低估、但最致命的一点**。

* * *

## 1️⃣ ERC 标准只是“语法”，不是“价值”

ERC20 / ERC721 解决的是：

-   怎么转
    
-   怎么查
    
-   怎么授权
    

但它**完全不解决**：

-   为什么要持有
    
-   为什么要参与
    
-   为什么不被薅羊毛
    

* * *

## 2️⃣ Token/NFT 的核心其实是三件事

### （1）发行逻辑（Distribution）

谁先拿到？  
拿多少？  
是否有锁仓？  
是否可预测？

错误设计会导致：

-   早期集中
    
-   后期没人参与
    
-   市场预期崩塌
    

* * *

### （2）激励机制（Incentive）

用户为什么要：

-   用你的 DApp？
    
-   长期留下来？
    
-   不刷、不作恶？
    

如果：

-   奖励 = 免费钱  
    那一定会被脚本刷爆
    

* * *

### （3）防刷 / 防女巫（Sybil Resistance）

这是 Web3 项目的“长期生存问题”。

因为：

-   一个用户 = 一个地址（几乎 0 成本）
    
-   你面对的是“无限账号对手”
    

所以你必须思考：

-   成本约束在哪里？
    
-   是否有真实消耗？
    
-   是否有长期绑定？
    

* * *

## 3️⃣ 为什么这是“规则层”，而不是“技术层”？

因为：

-   代码可以写对
    
-   接口可以标准
    
-   合约可以无漏洞
    

但只要：

-   激励设计错
    
-   成本模型错
    
-   规则被利用
    

👉 **项目照样会失败**
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->


# 为什么 storage 写入特别贵？

# 一、结论

> **storage 写入贵，不是因为“写硬盘慢”，  
> 而是因为“你在要求整个以太坊网络为你永久记账”。**

这是一种**经济约束设计**，不是单纯的技术性能问题。

* * *

# 二、EVM 三种数据空间，本质差异是什么？

你已经知道三种空间，我们现在**换一种理解方式**：

## 1️⃣ memory ——「我自己用，用完就丢」

-   生命周期：**仅限一次函数调用**
    
-   存在哪：**当前 EVM 执行上下文**
    
-   结束条件：函数 return / revert
    
-   其他节点：**完全不关心**
    

👉 本质：

> **这是“计算过程中的草稿纸”**

所以：

-   便宜
    
-   可以随便用
    
-   不影响链的长期状态
    

* * *

## 2️⃣ calldata ——「你带进来的原始材料」

-   生命周期：**整个交易**
    
-   只读
    
-   存在哪：交易本身
    
-   不可修改
    

👉 本质：

> **这是“你提交给区块链的输入参数”**

特点：

-   已经写进区块了
    
-   所有节点都会看到
    
-   但不会进入“世界状态”
    

所以：

-   比 memory 还便宜
    
-   非常适合做函数参数
    
-   不能当变量用
    

* * *

## 3️⃣ storage ——「全世界必须记住的事实」

这是重点。

-   生命周期：**永久**
    
-   存在哪：**全节点的世界状态树（Merkle Patricia Trie）**
    
-   影响范围：**整个网络**
    

👉 本质一句话：

> **storage 不是“你的变量”，而是“区块链的公共账本”**

* * *

# 三、为什么 storage 写入是“全网级成本”？

这是理解 storage 贵的**核心逻辑**。

## 1️⃣ 你写 storage，发生了什么？

当你执行一条 `SSTORE`：

1.  当前节点修改本地状态
    
2.  状态树发生变化
    
3.  新状态 hash 被计算
    
4.  区块包含新的 state root
    
5.  **所有全节点必须同步并存储这条变化**
    

也就是说：

> **你不是在写自己的数据  
> 你是在要求所有参与以太坊的节点，为你保存一份永久副本**

* * *

## 2️⃣ 为什么“永久”是最致命的？

memory / calldata 的数据：

-   用完即丢
    
-   不膨胀历史
    

storage：

-   一旦写入
    
-   **理论上永远不会被删除**
    
-   状态只增不减（除非 burn）
    

👉 结果：

> **全网状态体积不断膨胀  
> 新节点同步越来越慢  
> 运行全节点门槛越来越高**

这会直接威胁：

-   去中心化
    
-   网络安全性
    

* * *

# 四、Gas：不是收费，而是“强制你节制”

这是一个非常重要的认知转变。

## Gas 的真实角色是：

> **“用经济手段限制你滥用公共资源”**

storage 写入贵，就是在告诉你：

> “你可以写，但你要为‘全网长期负担’买单”

* * *

## 为什么是“非常贵”，而不是“稍微贵”？

因为如果 storage 便宜，会发生三件事：

1.  合约随意存字符串 / JSON
    
2.  垃圾数据爆炸
    
3.  全节点无法运行 → 网络中心化
    

👉 Gas 是 **治理工具，不是计价工具**。

* * *

# 五、SSTORE 的细节：为什么从 0 → 非 0 最贵？

这是 EVM 非常“精心设计”的地方。

### 三种情况：

| 操作 | Gas 逻辑 |
| --- | --- |
| 0 → 非 0 | 最贵（新增状态） |
| 非 0 → 非 0 | 较贵（修改状态） |
| 非 0 → 0 | 退部分 gas（释放状态） |

**原因：**

-   0 → 非 0：状态体积增加
    
-   非 0 → 0：状态体积减少，对网络有益
    

👉 这是在**用经济激励你清理状态**。

* * *

# 六、为什么“mapping + uint256”成了标配？

你以后会发现，几乎所有成熟合约都这样：

```
mapping(address => uint256) balance;
```

原因不是巧合，而是：

-   mapping 是 **稀疏结构**
    
-   只在真正使用时占 storage
    
-   不会连续占用 slot
    
-   非常节省状态空间
    

* * *

# 七、在写合约时，应该形成的条件反射

当你准备写一个 storage 变量时，**脑子里必须自动弹出这些问题**：

1.  这个数据是否真的需要永久保存？
    
2.  能不能只存在 calldata？
    
3.  能不能只在 memory 中计算？
    
4.  能不能通过事件 + off-chain 还原？
    
5.  能不能合并 storage slot？
    

如果你没问这些问题，就直接写 storage ——  
那你写的是\*\*“昂贵合约”\*\*。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->



### 一、交易是如何被执行的

很多初学者有一个**非常常见但非常致命的误解**：

> **“我调用了一个函数” = “链上执行了一个函数”**

这是不准确的。

* * *

## 1️⃣ 一次交易，本质上是什么？

在以太坊 / EVM 世界里：

> **交易（Transaction）是唯一能改变链上状态的东西**

交易包含的信息大致是：

-   from（谁发起）
    
-   to（目标合约地址，或空表示部署）
    
-   value（ETH 数量）
    
-   data（函数选择器 + 参数）
    
-   gasLimit / gasPrice
    

**注意：**

-   EVM 并不知道你“想调用哪个函数”
    
-   它只知道：**一段 data + 一个地址**
    

* * *

## 2️⃣ 从“交易”到“函数执行”的完整链路

真正发生的是这样一条流水线：

```
用户签名交易
   ↓
交易进入内存池（mempool）
   ↓
区块打包
   ↓
EVM 执行交易
   ↓
加载目标合约的字节码
   ↓
逐条执行 opcode
   ↓
修改内存 / storage / balance
   ↓
若成功 → 状态提交
若 revert → 状态不提交
```

**关键点来了：**

> **EVM 执行的是“字节码”，不是 Solidity 函数**

Solidity 只是：

-   高级语言
    
-   编译后 → 字节码
    
-   字节码 → opcode
    

* * *

## 3️⃣ revert 到底是什么意思？

很多人以为：

> revert = “把已经执行的代码回滚”

**这是错的。**

正确理解是：

> **EVM 在一个“临时状态”里执行交易  
> 只有最后没出错，才把结果写入世界状态树**

所以：

-   revert 并不是“回滚”
    
-   而是 **拒绝提交**
    

你可以理解为：

> “这笔交易从来没发生过，但 gas 已经消耗了”

* * *

## 你应该形成的直觉

当你看到一个函数时，脑中要自动翻译成：

> “这不是一个函数，这是一段可能被打包进交易、在 EVM 中逐条 opcode 执行的程序”

* * *

# 二、Gas 的本质（不是手续费，是资源定价）

这是 **Web2 程序员最容易误判的地方**。

* * *

## 1️⃣ Gas 到底是什么？

一句话定义：

> **Gas 是 EVM 对“计算 + 存储 + IO”的统一计量单位**

Gas 的作用只有两个：

1.  防止死循环
    
2.  防止滥用链上资源
    

**Gas 本身不是钱**  
钱是你用 ETH 去“买 Gas”

* * *

## 2️⃣ 为什么 storage 写入特别贵？

这是一个**设计层面的选择**，不是技术限制。

### EVM 里有三种“空间”：

| 类型 | 特点 | 成本 |
| --- | --- | --- |
| memory | 临时、函数结束即释放 | 便宜 |
| calldata | 只读、交易输入 | 很便宜 |
| storage | 永久写入全网节点 | 非常贵 |

**storage 写入贵的原因：**

-   每个全节点都要存
    
-   永久存在
    
-   增加全网状态体积
    

所以 EVM 用 Gas 强行告诉你：

> **“不要随便往 storage 里写东西”**

* * *

## 3️⃣ SSTORE 和 SLOAD 的差异

这是你以后做性能优化必须懂的点。

-   `SLOAD`：从 storage 读取
    
-   `SSTORE`：写入 storage
    

特点：

-   读比写便宜
    
-   从 0 → 非 0 最贵
    
-   非 0 → 0 有 gas refund
    

这也是为什么你会看到很多合约：

-   用 `uint256 packed`
    
-   用 `mapping` 而不用数组
    
-   尽量减少 storage 写入次数
    

* * *

## 你应该形成的直觉

> **每一次 storage 写入，都是在“花全网的钱”**

* * *

# 三、call / delegatecall 的差异（升级与攻击的核心）

这是 **99% 新手理解不深，但 100% DeFi 必须懂的点**。

* * *

## 1️⃣ call 是什么？

`call` 的语义是：

> **“去别人的合约，执行他的代码，用他的 storage”**

特点：

-   `msg.sender` 会变化
    
-   storage 是被调用合约的
    
-   非常安全、边界清晰
    

* * *

## 2️⃣ delegatecall 是什么？

`delegatecall` 的语义是：

> **“借用别人的代码，但在我自己的 storage 上执行”**

重点来了：

| 项目 | delegatecall |
| --- | --- |
| 执行代码 | 被调用合约 |
| storage | 当前合约 |
| msg.sender | 原始调用者 |

* * *

## 3️⃣ 为什么升级合约必须用 delegatecall？

因为：

-   合约地址一旦部署不可变
    
-   但 storage（用户数据）必须保留
    

代理合约的经典结构是：

-   Proxy：存数据
    
-   Logic：存代码
    

Proxy 用 `delegatecall` 调用 Logic  
这样：

-   数据不丢
    
-   逻辑可换
    

* * *

## 4️⃣ 为什么 delegatecall 是 DeFi 攻击高发区？

因为：

-   storage 布局错一位 = 灾难
    
-   被恶意合约 delegatecall = 任意写 storage
    

很多历史攻击的本质都是：

> **delegatecall + 权限检查不严**
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->




### **  
ERC-1155 介绍**

ERC-1155 是 Ethereum 上的一个多代币标准（Multi Token Standard），它允许一个智能合约同时管理多种类型的代币，包括可互换代币（Fungible Tokens，像 ERC-20）和不可互换代币（Non-Fungible Tokens，像 ERC-721）。它于 2018 年提出，由 Enjin Coin 团队主导，旨在解决现有标准（如 ERC-20 和 ERC-721）的局限性，例如需要为每个代币类型部署单独合约，导致代码冗余和交易成本高企。ERC-1155 通过一个合约处理无限种类的代币，每个代币 ID 可以代表一种独特的代币类型，具有自己的元数据、供应量和属性。

**动机和优势**

-   **动机**：在游戏、NFT 收藏或 DeFi 等场景中，用户可能需要处理多种代币（如游戏道具中的武器、金币、皮肤）。ERC-20 需要每个类型一个合约，ERC-721 则每个 ID 只代表一个 NFT，导致部署成本高和交互复杂。ERC-1155 支持批量操作（如批量转账），减少 gas 消耗，并允许原子交易（e.g., 交换多种代币）。
    
-   **优势**：
    
    -   **效率**：批量转移减少交易次数和 gas 费。
        
    -   **灵活性**：混合处理 FT 和 NFT（e.g., ID 1 是 1000 个金币，ID 2 是独特艺术品）。
        
    -   **元数据**：每个 ID 可有独立 URI 指向 JSON 元数据，支持本地化。
        
    -   **兼容性**：实现 ERC-165 接口，便于查询支持。
        
    -   **应用场景**：游戏资产、收藏品、批量空投、DeFi 流动性池。
        

相比 ERC-20（纯 FT）和 ERC-721（纯 NFT），ERC-1155 更通用，但实现更复杂，需要注意安全（如重入攻击）。

**接口定义**

ERC-1155 的核心接口（Solidity）如下（必须实现 ERC-165 的 supportsInterface 返回 0xd9b67a26）：

solidity

```
interface IERC1155 {
    function balanceOf(address account, uint256 id) external view returns (uint256);
    function balanceOfBatch(address[] calldata accounts, uint256[] calldata ids) external view returns (uint256[] memory);
    function setApprovalForAll(address operator, bool approved) external;
    function isApprovedForAll(address account, address operator) external view returns (bool);
    function safeTransferFrom(address from, address to, uint256 id, uint256 amount, bytes calldata data) external;
    function safeBatchTransferFrom(address from, address to, uint256[] calldata ids, uint256[] calldata amounts, bytes calldata data) external;
}
```

接收合约需实现 IERC1155Receiver 以支持安全转移回调。

**事件**

事件用于跟踪状态变化，便于 off-chain 索引和重构余额：

-   **TransferSingle(address indexed operator, address indexed from, address indexed to, uint256 id, uint256 value)**: 单代币转移（包括铸造/销毁）。
    
-   **TransferBatch(address indexed operator, address indexed from, address indexed to, uint256\[\] ids, uint256\[\] values)**: 批量转移。
    
-   **ApprovalForAll(address indexed account, address indexed operator, bool approved)**: 批准/撤销操作员。
    
-   **URI(string value, uint256 indexed id)**: 元数据 URI 更新
    

用在什么地方（应用场景）

ERC-1155 特别适合需要处理多种资产的场景，2026 年在 Web3 生态中广泛应用，尤其游戏和 NFT 领域。

-   **游戏资产管理**：
    
    -   地方：游戏 dApp（如链上游戏、Play-to-Earn）。
        
    -   示例：ID 1 = 金币（FT，可批量转移），ID 2 = 武器（半 NFT，可有限供应），ID 3 = 皮肤（纯 NFT，amount=1）。玩家可批量交易道具，减少 gas。
        
    -   为什么适合：支持批量空投奖励、交换物品原子操作（e.g., 交易剑 + 100 金币）。
        
-   **NFT 收藏与艺术**：
    
    -   地方：NFT 市场（如 OpenSea 支持 ERC-1155）、限量版艺术。
        
    -   示例：一个系列 NFT（如艺术卡片），每个 ID 是不同版本，amount 表示供应量（e.g., ID 1 = 100 张限量卡）。批量 mint 给收藏者。
        
    -   为什么适合：比 ERC-721 更高效（单一合约管理整个系列），支持半同质化（semi-fungible，如限量版）。
        
-   **DeFi 和流动性**：
    
    -   地方：DeFi 协议、流动性池、RWA（真实世界资产）。
        
    -   示例：表示 LP 代币（FT） + 独特衍生品（NFT）；批量转移在 AMM（如 Uniswap V3 NFT 位置，但扩展到多资产）。
        
    -   为什么适合：批量操作减少交易成本，在 PayFi/RWA 中表示多种资产捆绑（e.g., 房产份额 + 租金代币）。
        
-   **批量空投和营销**：
    
    -   地方：DAO、社区活动、Web3 营销。
        
    -   示例：项目方批量 mint 并转移多种代币给用户（e.g., 代币 + NFT 徽章）。
        
    -   为什么适合：原子批量转移，确保所有接收者同时收到。
        
-   **其他新兴场景（2026 年）**：
    
    -   AI + Web3：表示 AI 生成资产（e.g., ID 为不同模型输出，amount 为副本）。
        
    -   跨链：结合 CCIP/LayerZero，实现多链资产转移。
        
    -   社交：表示用户徽章/成就（混合 FT/NFT）。
        

3\. 为什么使用 ERC-1155（理由和优势）

ERC-1155 不是必须的，但它解决了 ERC-20/721 的痛点，尤其在多资产场景中。以下是核心理由：

-   **效率和成本节约**：
    
    -   单一合约管理无限 ID，减少部署/维护成本（ERC-20/721 需多个合约）。
        
    -   批量函数（balanceOfBatch、safeBatchTransferFrom）减少交易次数，gas 节省 20–50%（e.g., 转移 10 种道具只需 1 笔交易）。
        
    -   理由：Web3 用户痛点是高 gas，在 2026 年高 TPS 链（如 Base/Optimism）上更明显。
        
-   **灵活性和混合支持**：
    
    -   支持 FT（amount >1）、NFT（amount=1）、SFT（有限供应）。
        
    -   原子操作：批量转移确保“全成功或全失败”，防部分失败（e.g., 游戏交易）。
        
    -   理由：现实世界资产多样（如游戏中货币+独特物品），单一标准简化开发/集成。
        
-   **安全和标准化**：
    
    -   内置安全转移（回调机制），防止代币丢失在不支持合约中。
        
    -   兼容 ERC-165，便于查询；事件标准化，便于索引（如 The Graph 子图查询历史）。
        
    -   理由：减少自定义代码漏洞；生态支持好（钱包如 MetaMask、市场如 OpenSea 内置兼容）。
        
-   **扩展性和未来证明**：
    
    -   可扩展：加 Ownable/Pausable/Supply 等模块。
        
    -   2026 年趋势：与 ZK/AA（ERC-4337）结合，提升用户体验（e.g., 批量签名）。
        
    -   理由：比老标准更现代，适合大规模应用；社区活跃，易找资源/审计。
        
-   **潜在缺点 & 何时不使用**：
    
    -   复杂：实现比 ERC-20 难，需注意重入/批准滥用。
        
    -   如果只需纯 FT，用 ERC-20 更简单；纯 NFT 用 ERC-721/404。
        
    -   理由：选择标准取决于需求——如果不需多类型/批量，别过度使用。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->





# 一、ERC-721 的整体认知（相关代码上传在GitHub）

## [ERC721代码相关链接](https://github.com/may-tonk/my_web3_study/blob/master/contracts/_ERC721.sol)

ERC-721 解决的核心问题只有一句话：

> **如何在链上唯一、可验证地表示“这个 NFT 属于谁”**

因此它围绕三件事展开：

1.  **所有权（ownership）**
    
2.  **转移（transfer）**
    
3.  **授权（approval）**
    

你看到的所有函数，本质都服务于这三点。

* * *

# 二、IERC721 中的核心函数（必须掌握）

## 1️⃣ `balanceOf(address owner)`

### 用途

> 查询某个地址**拥有多少个 NFT**

```
function balanceOf(address owner) external view returns (uint256);
```

### 使用场景

-   NFT 钱包页面显示「你拥有 X 个 NFT」
    
-   DApp 判断用户是否有资格（比如持有至少 1 个 NFT）
    

### 注意点（非常重要）

-   **不能传** `address(0)`
    
-   `address(0)` 在 ERC-721 中表示“无效地址”
    

👉 合规实现里必须 `require(owner != address(0))`

* * *

## 2️⃣ `ownerOf(uint256 tokenId)`

### 用途

> 查询某个 **tokenId 属于谁**

```
function ownerOf(uint256 tokenId) external view returns (address);
```

### 使用场景

-   NFT 市场展示当前持有人
    
-   合约校验「你是不是这个 NFT 的主人」
    

### 注意点

-   如果 `tokenId` **不存在**，必须 revert
    
-   **不能返回 address(0)**
    

* * *

## 3️⃣ `safeTransferFrom`（最重要的转移函数）

### 用途

> **安全转移 NFT**

```
function safeTransferFrom(
  address from,
  address to,
  uint256 tokenId
) external;
```

### 为什么叫 “safe”

-   如果 `to` 是合约地址
    
-   会检查它是否实现 `onERC721Received`
    
-   防止 NFT 被转进「收不回来的合约」
    

### 使用场景

-   NFT 市场转移
    
-   钱包转账 NFT
    
-   游戏资产流转
    

### 注意点（新手高频踩坑）

1.  **from 必须是当前 owner**
    
2.  **msg.sender 必须有权限**
    
3.  **to 不能是 address(0)**
    

> 💡 实际开发中  
> **99% 的 NFT 转移都应该用** `safeTransferFrom`

* * *

## 4️⃣ `transferFrom`（不安全转移）

### 用途

> 直接转移 NFT（不检查接收方）

```
function transferFrom(
  address from,
  address to,
  uint256 tokenId
) external;
```

### 什么时候用？

-   接收方是 EOA（普通钱包）
    
-   且你明确知道风险
    

* * *

## 5️⃣ `approve(address to, uint256 tokenId)`

### 用途

> **授权某个地址操作指定 NFT**

```
function approve(address to, uint256 tokenId) external;
```

### 典型场景

-   用户授权 NFT 市场合约出售 NFT
    
-   授权游戏合约使用 NFT
    

### 授权规则

-   只能授权 **一个地址**
    
-   覆盖旧授权
    
-   `to = address(0)` 表示取消授权
    

### 注意点

-   授权 ≠ 转移
    
-   授权后对方可以 `transferFrom`
    

* * *

## 6️⃣ `getApproved(uint256 tokenId)`

### 用途

> 查询某个 NFT 当前被授权给谁

```
function getApproved(uint256 tokenId) external view returns (address);
```

### 使用场景

-   NFT 市场判断是否已被授权
    
-   前端展示授权状态
    

### 注意点

-   tokenId 不存在必须 revert
    

* * *

## 7️⃣ `setApprovalForAll(address operator, bool approved)`

### 用途

> **批量授权（非常重要）**

```
function setApprovalForAll(address operator, bool approved) external;
```

### 典型场景

-   一次授权 NFT 市场管理你**所有 NFT**
    
-   比单个 approve 更省 Gas
    

## ERC721核心声明

![屏幕截图 2026-01-18 213619.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/may-tonk/images/2026-01-18-1768743602248-_____2026-01-18_213619.png)

## ERC-721 合约函数使用总结

### 一、ERC-721 本质在解决什么

ERC-721 的本质只有一件事：  
**在链上，明确、唯一、可验证地记录“某个 NFT 属于谁，并且如何安全地转移和授权”。**

因此，所有函数都围绕三点展开：

1.  所有权（Ownership）
    
2.  转移（Transfer）
    
3.  授权（Approval）
    

* * *

### 二、所有权相关函数（“谁拥有 NFT”）

-   `balanceOf(owner)`  
    用来统计一个地址拥有多少个 NFT。  
    常用于前端展示和资格判断。  
    注意：不能查询 `address(0)`。
    
-   `ownerOf(tokenId)`  
    查询某个 NFT 当前属于谁。  
    tokenId 不存在必须直接报错（revert）。  
    这是 NFT 所有权的“最终真相来源”。
    

* * *

### 三、转移相关函数（“NFT 如何流动”）

-   `safeTransferFrom`**（首选）**  
    安全转移 NFT。  
    如果接收方是合约，会检查它是否能正确接收 NFT，防止资产被锁死。  
    实际 DApp 中，几乎所有 NFT 转移都应该使用它。
    
-   `transferFrom`**（不安全）**  
    不做接收方检查。  
    新手阶段不建议使用，容易造成 NFT 无法找回。
    

转移时必须满足：

-   from 是当前 owner
    
-   msg.sender 是 owner 或被授权者
    
-   to 不能是 `address(0)`
    

* * *

### 四、授权相关函数（“谁可以代我操作 NFT”）

-   `approve(to, tokenId)`  
    授权某个地址操作某一个 NFT。  
    常用于 NFT 市场出售单个 NFT。  
    授权可以被覆盖或取消。
    
-   `setApprovalForAll(operator, approved)`  
    批量授权，允许某个地址操作你所有 NFT。  
    NFT 市场最常用的授权方式。  
    安全风险高，必须让用户明确知情。
    
-   `getApproved(tokenId)` **/** `isApprovedForAll(owner, operator)`  
    查询授权状态，用于前端展示和合约判断。
    

* * *

### 五、元数据相关函数（“NFT 显示给人看的内容”）

-   `name()` **/** `symbol()`  
    NFT 集合的名称和简称。
    
-   `tokenURI(tokenId)`**（非常关键）**  
    返回 NFT 的元数据地址。  
    一般指向 IPFS / Arweave / HTTP。  
    NFT 图片、属性、描述全部依赖它。
    

常见错误：

-   所有 tokenId 返回同一个 URI
    
-   URI 指向的 JSON 不符合标准
    
-   tokenId 不存在却返回 URI
    

* * *

### 六、事件（Event）的重要性

ERC-721 强依赖事件，而不是函数返回值：

-   `Transfer`  
    mint、转移、销毁都会触发。  
    钱包和 NFT 市场靠它同步状态。
    
-   `Approval` **/** `ApprovalForAll`  
    市场监听授权变化。
    

没有正确触发事件，外部世界就“看不见”你的 NFT。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->






# 今日复习hash的处理（详细代码上传在GitHub）

[GitHub中hash代码链接](https://github.com/may-tonk/my_web3_study/blob/master/contracts/_hash.sol)

## **一、合约功能概览**

这个合约主要是**演示 Solidity 中哈希函数的使用**，核心功能包括：

1.  存储字符串的哈希，用于后续验证。
    
2.  将多个变量（整数、字符串、地址）拼接后计算哈希。
    
3.  检查输入字符串的哈希是否与存储的哈希一致。
    
4.  比较两个输入字符串的哈希是否相同。
    

* * *

## **二、核心概念与知识点**

### 1\. 哈希函数（Hash Function）

-   哈希函数是一种**单向映射函数**，把任意长度的输入映射为固定长度输出（以太坊用 32 字节 = 256 位）。
    
-   特性：
    
    1.  **确定性**：相同输入永远得到相同输出。
        
    2.  **不可逆**：无法从哈希值推回原始输入。
        
    3.  **抗碰撞**：不同输入得到相同哈希的概率极低。
        
    4.  **雪崩效应**：输入哪怕只有一位不同，哈希值差异巨大。
        
-   在 Solidity 中常用：
    
    -   `keccak256(bytes memory) returns (bytes32)`
        
        > Ethereum 中的标准哈希函数，基于 Keccak-256 算法。
        

* * *

### 2\. 编码函数 `abi.encodePacked` 与 `abi.encode`

-   **abi.encodePacked(...)**
    
    -   紧凑拼接参数生成字节流。
        
    -   输出长度可变，适合哈希计算或签名。
        
    -   注意 **多类型拼接可能导致碰撞**：
        
        ```
        abi.encodePacked(uint16(0x1234), uint8(0x56)) 
        // 与 abi.encodePacked(uint8(0x12), uint16(0x3456)) 可能产生相同字节流
        ```
        
-   **abi.encode(...)**
    
    -   使用标准 ABI 编码（每个参数固定长度，防止碰撞）。
        
    -   如果担心 `abi.encodePacked` 的拼接碰撞问题，可使用 `abi.encode`。
        

* * *

### 3\. Solidity 中字符串、整数、地址的哈希

-   **整数与地址**：Solidity 会把它们转换为对应的字节形式再哈希。
    
-   **字符串**：需要先转换成字节流，常用：
    
    ```
    keccak256(abi.encodePacked("hello"))
    ```
    
-   **哈希拼接**：
    
    ```
    keccak256(abi.encodePacked(x, name, add))
    ```
    
    -   `x` 是 `uint`，`name` 是 `string`，`add` 是 `address`。
        
    -   返回 32 字节 `bytes32` 哈希值。
        

* * *

### 4\. Storage vs Memory

-   **storage**：存储在区块链上的变量，修改需要 Gas。
    
-   **memory**：临时变量，仅存在函数调用期间。
    
-   函数类型：
    
    -   `view`：只读取 storage，不修改状态。
        
    -   `pure`：不读取也不修改 storage，只依赖输入参数计算。
        

在你的合约中：

-   `hash()`、`justice()` 是 `view`，读取了 storage。
    
-   `justic2()` 是 `pure`，不访问 storage。
    

* * *

### 5\. 哈希对比方法

-   检查字符串是否一致：
    

```
keccak256(abi.encodePacked(st)) == _msg
```

-   比较两个字符串是否相同：
    

```
keccak256(abi.encodePacked(st1)) == keccak256(abi.encodePacked(st2))
```

-   **为什么要用哈希比较？**
    
    -   Solidity 不能直接用 `==` 比较 `string memory`。
        
    -   使用哈希比较可以快速判断相等性，节省 Gas。
        

* * *

## **三、合约代码逐行分析**

1.  **初始化存储哈希**
    

```
bytes32 public _msg = keccak256(abi.encodePacked("0xAA"));
```

-   将字符串 `"0xAA"` 转为字节流，再生成 32 字节哈希存储在 `_msg`。
    
-   后续可用 `justice()` 检查输入是否等于 `"0xAA"`。
    

2.  **示例状态变量**
    

```
uint public x = 10;
string public name = "0xff";
address public add = 0x7A58c0Be72BE218B41C608b7Fe7C5bB630736C71;
```

-   用于 `hash()` 函数演示多类型变量哈希拼接。
    

3.  **hash() 函数**
    

```
function hash() public view returns(bytes32) {
    return keccak256(abi.encodePacked(x, name, add));
}
```

-   拼接三个变量生成哈希。
    
-   可用于数据完整性验证，比如检查变量组合是否被篡改。
    

4.  **justice() 函数**
    

```
function justice(string memory st) public view returns(bool) {
    return keccak256(abi.encodePacked(st)) == _msg;
}
```

-   输入字符串 `st` 与存储的 `_msg` 哈希比较。
    
-   返回 `true` 或 `false`。
    

5.  **justic2() 函数**
    

```
function justic2(string memory st1, string memory st2) public pure returns(bool) {
    return keccak256(abi.encodePacked(st1)) == keccak256(abi.encodePacked(st2));
}
```

-   比较两个字符串是否相等。
    
-   `pure` 函数不依赖合约状态，仅根据输入计算结果。
    

* * *

## **四、哈希应用场景**

1.  **身份验证**
    
    -   存储密码哈希而不是明文：
        
        ```
        bytes32 public passwordHash;
        passwordHash = keccak256(abi.encodePacked("mypassword"));
        ```
        
    -   用户输入时比较哈希，而不是明文存储。
        
2.  **数据完整性校验**
    
    -   文件或数据上传前计算哈希，防止篡改。
        
3.  **签名验证**
    
    -   与 `ecrecover` 搭配，验证消息是否由特定地址签名。
        
4.  **Merkle 树**
    
    -   大量数据哈希组合，常用于链上存证和证明。
        

* * *

## **五、扩展与注意事项**

1.  **abi.encodePacked 碰撞风险**
    
    -   拼接多类型数据可能产生哈希碰撞。
        
    -   安全起见：
        
        -   使用 `abi.encode`（带类型长度信息）。
            
        -   或者在拼接前加定长前缀。
            
2.  **字符串大小写**
    
    -   `"0xAA"` 与 `"0xaa"` 哈希不同，大小写敏感。
        
3.  **Gas 成本**
    
    -   `keccak256` 是低成本操作，但 storage 写入哈希较贵。
        
    -   读取 storage 哈希比直接计算更节省 Gas。
        

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

/*
  合约名：Hash
  功能：
  1. 演示 keccak256 哈希函数的使用
  2. 通过 abi.encodePacked 生成哈希
  3. 验证输入字符串是否与存储的哈希一致
  4. 比较两个输入字符串哈希是否一致
*/
contract Hash {

    // -------------------------
    // 1) 初始化存储哈希
    // -------------------------
    // abi.encodePacked("0xAA") 将字符串 "0xAA" 转换为紧凑字节流
    // keccak256(...) 返回 32 字节哈希
    // _msg 存储了 "0xAA" 的哈希，用于后续验证
    bytes32 public _msg = keccak256(abi.encodePacked("0xAA"));

    // -------------------------
    // 2) 示例状态变量
    // -------------------------
    uint public x = 10; // uint256 类型的整数
    string public name = "0xff"; // 字符串
    address public add = 0x7A58c0Be72BE218B41C608b7Fe7C5bB630736C71; // 示例地址

    // -------------------------
    // 3) hash() 函数
    // -------------------------
    // 作用：返回 x、name、add 拼接后的哈希
    // view：因为只读取了 storage（x, name, add），没有修改状态
    function hash() public view returns(bytes32) {
        // abi.encodePacked 将多个参数紧凑拼接为字节流
        // keccak256 对拼接后的字节流计算哈希
        return keccak256(abi.encodePacked(x, name, add));
    }

    // -------------------------
    // 4) justice() 函数
    // -------------------------
    // 作用：检查输入字符串 st 的哈希是否等于 _msg
    // view：因为读取了 storage 变量 _msg
    function justice(string memory st) public view returns(bool) {
        // 将输入字符串 st 编码为紧凑字节流
        // 然后计算哈希与 _msg 对比
        return keccak256(abi.encodePacked(st)) == _msg;
    }

    // 作用：比较两个输入字符串 st1 和 st2 的哈希是否相同
    // pure：不读取或修改任何 storage
    function justic2(string memory st1, string memory st2) public pure returns(bool) {
        return keccak256(abi.encodePacked(st1)) == keccak256(abi.encodePacked(st2));
    }

}
```
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->







# 关于ETH的部分总结理解：

### ETH的运用场景详细讲解

ETH（Ether）是以太坊网络的原生加密货币，不仅是一种数字资产，还承载着多种功能。它不像比特币那样单纯作为“数字黄金”，而是作为以太坊生态的“燃料”和“价值锚定”。以下从多个维度详细讲解其运用场景（基于2026年初的生态现状，以太坊已完成Dencun升级并推进分片早期阶段）：

1.  **支付网络费用（Gas费）**
    
    -   **核心用途**：每笔以太坊交易（如转账、执行智能合约）都需要消耗Gas，ETH用于支付这些费用。Gas机制确保网络不被滥用（防止无限循环或垃圾交易）。
        
    -   **实际场景**：在DeFi中借贷（如Aave平台），用户需支付ETH作为Gas费；NFT铸造或交易（如OpenSea）也如此。EIP-1559（2021年实施）引入基础费燃烧机制，使ETH部分通缩（截至2026年，ETH年供应增长率约0.2%-0.5%，取决于网络活动）。
        
    -   **扩展影响**：在Layer 2 (L2) 上，Gas费更低，但最终结算仍需L1的ETH。
        
2.  **价值存储与投资**
    
    -   **核心用途**：ETH被视为“数字石油”，用于对冲通胀或作为投资资产。机构投资者（如BlackRock的ETH ETF，2024年批准）将其纳入 portfolio。
        
    -   **实际场景**：持有ETH作为长期资产，受益于以太坊升级（如The Merge后PoS带来的通缩）；在熊市中作为抵押品借出稳定币（如USDC）。
        
    -   **数据参考**：2026年ETH市值约占加密市场的25%-30%，价格波动受宏观经济（如美联储利率）和生态事件（如分片升级）影响。
        
3.  **Staking（质押）与网络安全**
    
    -   **核心用途**：在PoS共识下，用户质押ETH成为验证者，维护网络安全并赚取奖励（年化收益率约3%-5%，取决于总质押量）。
        
    -   **实际场景**：个人质押32 ETH运行节点，或通过Lido/Rocket Pool等流动staking协议参与（无需32 ETH门槛）。这支持网络去中心化，避免PoW的高能耗。
        
    -   **扩展影响**：2026年后，分片（Sharding）将增加staking需求，进一步提升ETH效用。
        
4.  **DeFi（去中心化金融）中的角色**
    
    -   **核心用途**：ETH作为抵押品、流动性提供或治理代币。
        
    -   **实际场景**：在Uniswap提供ETH/稳定币流动性池赚取手续费；在MakerDAO抵押ETH生成DAI稳定币；在Aave借贷ETH赚取利息。TVL（总锁定价值）中ETH占比约40%。
        
5.  **NFT、游戏与社交**
    
    -   **核心用途**：ETH用于购买/交易NFT，或在链游中作为经济通证。
        
    -   **实际场景**：OpenSea上用ETH买艺术品；Axie Infinity-like游戏中用ETH购买资产；ENS（Ethereum Name Service）域名注册需ETH。
        
6.  **跨链与桥接**
    
    -   **核心用途**：ETH在多链生态中桥接（如从L1到L2，或到其他EVM链如Polygon）。
        
    -   **实际场景**：使用Wormhole或LayerZero桥将ETH转移到Arbitrum进行低费交易。
        
7.  **企业与现实世界应用（RWA）**
    
    -   **核心用途**：ETH支持token化资产，如房地产或债券。
        
    -   **实际场景**：2026年，更多机构（如JPMorgan）用以太坊结算供应链；碳信用token化项目用ETH支付。
        

# 关于ETH的L1，L2的理解

以太坊生态分层设计解决“区块链三难困境”（去中心化、安全、可扩展性）。L1是基础层，L2是扩展层。

Layer 1 (L1)：以太坊主网

-   **定义**：L1是以太坊的核心区块链，直接运行共识、验证和数据存储。区块时间约12秒，TPS（每秒交易数）约15-30（未经扩展）。
    
-   **用途**：
    
    -   **安全结算层**：所有交易最终在L1确认，确保不可篡改。L2依赖L1的最终性。
        
    -   **数据可用性**：存储历史数据，支持查询（如Etherscan）。
        
    -   **共识维护**：PoS下，验证者用ETH staking，确保网络安全。
        
    -   **实际应用**：高价值交易（如大额DeFi转移）直接在L1进行，避免L2风险；DAO治理投票（如MakerDAO）。
        
-   **优势**：高度去中心化（>10万验证者），安全性强。
    
-   **局限**：Gas费高（拥堵时>10美元/交易），扩展性差，导致用户转向L2。
    
-   **2026年现状**：Dencun升级（EIP-4844，2024年）引入Proto-Danksharding，降低L2数据成本90%；分片早期阶段启动，提升L1容量。
    

Layer 2 (L2)：扩展解决方案

-   **定义**：L2构建在L1之上，批量处理交易并提交到L1结算。主流类型：Rollups（Optimistic和ZK）。
    
-   **用途**：
    
    -   **提升扩展性**：TPS可达数千（如Arbitrum ~1000 TPS），Gas费低（<0.1美元/交易）。
        
    -   **用户友好**：支持相同EVM环境，开发者无缝迁移。
        
    -   **实际应用**：
        
        -   **Optimistic Rollup**（如Optimism、Arbitrum、Base）：假设交易有效，有7天挑战期。用于DeFi（如Uniswap on Arbitrum）和游戏（低延迟）。
            
        -   **ZK Rollup**（如zkSync Era、Polygon zkEVM、Scroll）：用零知识证明即时验证，更安全。用于隐私密集应用（如匿名交易）。
            
        -   **其他**：Validium（数据离链，成本更低，但可用性弱）；侧链（如Polygon PoS，非严格L2）。
            
    -   **生态作用**：2026年，L2 TVL占以太坊总TVL 70%以上，推动“Rollup-centric”路线图。Superchain（如Optimism生态）整合多个L2，实现跨链无缝。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->








# 一、Solidity 核心概念总览表（回温版）

1️⃣ 语言与语法层

| 模块 | 核心概念 | 关键点 | 回温关注点 |
| --- | --- | --- | --- |
| 合约结构 | pragma / contract | ^0.8.x 内置溢出检查 | 构造函数、可见性 |
| 变量 | 状态变量 / 局部变量 | storage 持久化 | gas 成本 |
| 数据类型 | uint / address / bool | 默认 uint256 | 显式类型 |
| 引用类型 | array / mapping / struct | mapping 不能遍历 | storage vs memory |
| 函数 | external / public | calldata 只能 external | ABI 设计 |
| 修饰符 | modifier | 执行顺序 _; | 权限与校验 |

2️⃣ 数据位置与内存模型

| 关键字 | 存储位置 | 是否可修改 | Gas 成本 | 常见错误 |
| --- | --- | --- | --- | --- |
| storage | 链上 | ✅ | 高 | 误以为是拷贝 |
| memory | 函数内 | ✅ | 中 | 生命周期误判 |
| calldata | 入参 | ❌ | 最低 | 用在 public |

3️⃣ 函数与执行控制

| 类型 | 说明 | 你需要记住的点 |
| --- | --- | --- |
| view | 读状态 | 不消耗 gas（call） |
| pure | 不读不写 | 常用于工具函数 |
| payable | 可收 ETH | 必须显式声明 |
| fallback | 无函数匹配 | ETH 接收兜底 |
| receive | 仅收 ETH | 无 calldata |

4️⃣ 调用上下文（msg / tx / block）

| 变量 | 含义 | 安全注意 |
| --- | --- | --- |
| msg.sender | 当前调用者 | 权限判断唯一可靠 |
| msg.value | 转入 ETH | payable |
| msg.data | 原始 calldata | 函数选择器 |
| tx.origin | 交易发起人 | ❌ 不可用于鉴权 |
| block.timestamp | 区块时间 | 可被微调 |

5️⃣ EVM 执行与 Gas

| 行为 | 结果 |
| --- | --- |
| revert | 状态回滚，gas 消耗 |
| require | 条件失败即回滚 |
| 外部调用 | 可能重入 |
| storage 写入 | gas 最贵 |

6️⃣ 常见安全问题总表

| 风险 | 本质 | 防御方式 |
| --- | --- | --- |
| 重入攻击 | 状态未锁 | CEI / ReentrancyGuard |
| 溢出 | 数值越界 | ^0.8.x 自动检查 |
| 授权钓鱼 | approve 滥用 | 限额 / revoke |
| 权限失控 | modifier 错误 | onlyOwner |
| 签名重放 | nonce 缺失 | nonce / domain |

7️⃣ 数字签名

| 步骤 | 说明 |
| --- | --- |
| hash | keccak256 |
| 前缀 | \x19Ethereum Signed Message |
| 恢复 | ecrecover |
| 用途 | permit / 白名单 |

## 复习的相关代码以上传本地的GitHub

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

// 从 Chainlink 导入价格数据接口
import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/shared/interfaces/AggregatorV3Interface.sol";

// 定义一个名为 fundyou 的库（library）
// 库里面封装了一些与价格转换相关的函数
contract fundme{
    uint256 public minnal = 1e18;
    address[] public funders;
    AggregatorV3Interface public datafeed;

    constructor(address datafeedadd){
        datafeed = AggregatorV3Interface(datafeedadd);//进行相关数据的转换
    }

    function fund() public payable{
        require(getconversion(msg.value)>=minnal,"send last 1 ETH");
        funders.push(msg.sender);

    }
    // 获取 ETH / USD 价格
    function getprice() public view returns (uint256) {
        // Chainlink ETH/USD 预言机地址（Sepolia 测试网）
        AggregatorV3Interface pricefeed = datafeed /*AggregatorV3Interface(
            0x694AA1769357215DE4FAC081bf1f309aDC325306)*/;

        // 获取最新一轮价格数据
        // latestRoundData() 返回五个值，我们只需要第二个 price
        (, int256 price, , , ) = pricefeed.latestRoundData();

        // Chainlink 返回的 ETH/USD 价格精度是 8 位小数（即 1e8）
        // 这里乘上 1e10，把它转换成 1e18 的形式（和以太币精度保持一致）
        return uint256(price * 1e10);
    }

    // 把 ETH 数量转换成等值的 USD
    function getconversion(uint256 ethAmount) public view returns (uint256) {
        uint256 ethprice = getprice(); // 获取当前 1 ETH 的美元价格
        // 计算：ETH 数量 × ETH 价格 / 1e18（因为 ETH 的单位是 Wei）
        uint256 ethusd = (ethprice * ethAmount) / 1e18;
        return ethusd; // 返回等值的 USD 金额
    }

    // 获取 Chainlink Aggregator 的版本号
    function getversion() public view returns(uint) {
        AggregatorV3Interface pricefeed = datafeed/*AggregatorV3Interface(
            0x694AA1769357215DE4FAC081bf1f309aDC325306)相关sepolia的接口*/;
        return pricefeed.version();
    }
    

    //外部人员进行直接转账，没有调用合约是触发的一个条件
    receive() external payable {
        fund();
     }

     fallback() external payable{
        fund();
     }

}
```

具体所以代码查看[my\_web3\_study/contracts/fundme.sol at master · may-tonk/my\_web3\_study --- my\_web3\_study/contracts/fundme.sol at master · may-tonk/my\_web3\_study](https://github.com/may-tonk/my_web3_study/blob/master/contracts/fundme.sol)
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->









# **order book(订单薄)和AMM(**自动做市商)(采用ChatGPT纠正总结)

## 一、什么是订单簿（Order Book）

**订单簿**是交易所用来实时展示市场买卖意愿的核心工具，本质上反映的是：

> 市场中“谁愿意用什么价格，买 / 卖 多少资产”。

无论是 **Web2 股票市场** 还是 **Web3 加密交易所（如 Binance、OKX、Uniswap v4 的链下撮合部分）**，订单簿的逻辑是相通的。

* * *

## 二、订单簿的三大核心组成

### 1️⃣ 买入订单（Buy Orders / Bids）

-   表示买方**愿意出的价格** + **想买的数量**
    
-   本质是“需求侧”
    
-   价格越高，越靠近成交
    

### 2️⃣ 卖出订单（Sell Orders / Asks）

-   表示卖方**愿意卖的价格** + **卖出的数量**
    
-   本质是“供给侧”
    
-   价格越低，越容易成交
    

### 3️⃣ 成交历史（Order / Trade History）

-   已经发生的真实交易记录
    
-   用来观察：
    
    -   最近成交价
        
    -   成交量
        
    -   市场活跃度
        

* * *

## 三、订单簿的“核心位置”：Top of the Book

**订单簿顶部 = 市场最关键的信息**

-   **最高买价（Highest Bid）**
    
-   **最低卖价（Lowest Ask）**
    

这两者之间的差值叫：

> **点差（Spread）**

📌 含义：

-   点差越小 → 市场越活跃、流动性越好
    
-   点差越大 → 流动性差、滑点风险高（Web3 非常重要）
    

* * *

## 四、订单簿 + K线的配合意义

订单簿通常会配合 **K 线图（Candlestick Chart）** 使用：

-   **K线**：反映“已经发生了什么”（结果）
    
-   **订单簿**：反映“现在市场想做什么”（意图）
    

👉 在 Web3 交易中：

-   K 线 = 历史行为
    
-   订单簿 = 即时博弈状态
    

* * *

## 五、订单簿能帮交易者做什么（实战价值）

### 1️⃣ 判断是谁在推动市场

-   大量小单 → 更像散户
    
-   集中大额订单 → 更像机构 / 做市商 / 巨鲸
    

在加密市场中：

-   很多时候是 **做市商 + 机器人** 在主导
    

* * *

### 2️⃣ 发现短期方向信号（订单失衡）

**订单失衡（Order Imbalance）**：

-   买单明显多于卖单 → 买压大 → 短期偏多
    
-   卖单明显多于买单 → 卖压大 → 短期偏空
    

⚠️ 注意（Web3 特别重要）：

-   订单是可以**随时撤单的**
    
-   存在“假墙 / 钓鱼单”
    

* * *

### 3️⃣ 判断支撑位与阻力位

这是订单簿非常实用的一点：

-   某个价格有**大量买单堆积**
    
    -   → 潜在**支撑位**
        
-   某个价格有**大量卖单堆积**
    
    -   → 潜在**阻力位**
        

在 Web3 中你会常听到：

-   **Buy Wall（买墙）**
    
-   **Sell Wall（卖墙）**
    

## 一、AMM 的核心组成（对应“订单簿的三大组成”）

### 1️⃣ 流动性池（Liquidity Pool）

-   由**两种（或多种）资产**构成，例如 ETH / USDC
    
-   本质不是“买单或卖单”，而是**可随时被交易的资金库存**
    
-   池中资产的数量比例，直接决定当前价格
    

👉 对应订单簿中的：

-   买单 + 卖单的**集合体**
    
-   但不区分“谁在买、谁在卖”
    

* * *

### 2️⃣ 定价函数（Pricing Function）

-   最经典的是恒定函数：`x * y = k`
    
-   价格不是报价，而是：
    
    > **“在当前池子状态下，再多买 1 个，会导致什么变化”**
    
-   每一笔交易都会改变池子状态，也就改变价格
    

👉 对应订单簿中的：

-   不同价格档位的挂单结构
    
-   但在 AMM 中，价格是**连续函数**，而不是离散档位
    

* * *

### 3️⃣ 链上成交记录（On-chain Swap History）

-   每一笔 swap 都是链上交易
    
-   可以看到：
    
    -   成交价格
        
    -   成交数量
        
    -   时间戳
        
    -   交易者地址
        

👉 对应订单簿中的：

-   成交历史（Trade History）
    
-   区别在于：**AMM 的成交历史是完全可验证的**
    

* * *

## 二、AMM 的“核心位置”（对应 Top of the Book）

在 AMM 中，**不存在最高买价 / 最低卖价**，但存在一个等价概念：

### 当前池子隐含价格（Spot Price）

-   由池中两种资产的数量比例决定
    
-   是：
    
    > “在极小交易量下的理论成交价格”
    

📌 含义：

-   池子越大 → 单笔交易对价格影响越小 → 滑点越低
    
-   池子越小 → 价格对交易极其敏感 → 滑点风险高
    

👉 对应订单簿中的：

-   点差（Spread）
    
-   但 AMM 中的“点差”体现为 **滑点曲线的陡峭程度**
    

* * *

## 三、AMM + K 线的配合意义（和订单簿类似，但逻辑不同）

-   **K 线**：反映“已经发生过的交换结果”
    
-   **AMM 池子状态**：反映“当前价格是如何被资金结构支撑的”
    

👉 在 Web3 交易中：

-   K 线 = 历史交换结果
    
-   AMM = 资金结构驱动下的价格函数
    

换句话说：

> 订单簿看的是**人怎么想**  
> AMM 看的是**钱怎么放**

## order book 和 AMM 对比：

| 对比维度 | Order Book（订单簿模式） | AMM（自动做市商模式） |
| --- | --- | --- |
| 主要使用场景 | 中心化交易所（CEX） | 去中心化交易所（DEX） |
| 典型代表 | Binance、OKX、Bybit、Coinbase | Uniswap、Curve、Balancer、PancakeSwap |
| 撮合方式 | 买卖双方挂单撮合 | 通过数学公式自动定价 |
| 是否需要做市商 | 需要（人工或量化做市） | 不需要传统做市商 |
| 流动性来源 | 做市商 + 交易者挂单 | 流动性提供者（LP） |
| 定价机制 | 市场竞价形成价格 | 数学公式（如 x*y=k） |
| 是否有订单簿 | 有 | 没有传统订单簿 |
| 价格发现效率 | 高（深度好时） | 中等（依赖池子规模） |
| 流动性深度 | 与做市规模强相关 | 与资金池规模强相关 |
| 滑点表现 | 深度好时滑点低 | 大额交易滑点明显 |
| 高频 / 量化友好度 | 非常友好 | 不友好 |
| 小币种支持能力 | 上线门槛高 | 上线门槛极低 |
| 抗审查能力 | 弱（中心化） | 强（链上合约） |
| 透明度 | 内部撮合，不完全透明 | 完全链上透明 |
| 资金托管 | 交易所托管 | 用户自托管 |
| 技术复杂度 | 高（撮合系统） | 高（智能合约 + 数学） |
| Web3 原生程度 | 偏 Web2 架构 | Web3 原生 |

## order book总结：

首先是即时供需关系。买单和卖单的数量分布，直接反映了市场中买方与卖方谁更积极。如果某一侧的挂单明显更厚，说明该方向的意愿更强，但这只是“意愿”，不等于一定会成交。

其次是短期价格压力方向。当买单在当前价格下方密集堆积，而卖单相对稀疏时，价格向下的阻力会更大，短期更容易被托住；反之，如果上方卖单密集、买单稀薄，价格向上会面临明显阻力。这更多适用于短周期和高频视角。

第三是流动性与滑点风险。订单簿越深、挂单越连续，说明流动性越好，大额成交对价格的冲击越小；如果订单稀疏、价格档位断层明显，说明流动性差，容易出现较大的滑点。这一点在 Web3 小币种或新上线交易对中尤为重要。

第四是支撑位与阻力位的潜在位置。在某个价格附近，如果出现持续存在的大量买单，往往被视为潜在支撑位；如果某个价格附近长期堆积大量卖单，则可能形成阻力位。但需要注意，这些挂单随时可以撤销，因此只能作为参考，而不是确定性的结论。

第五是市场参与者行为特征。大量、整齐、规律的挂单，往往来自做市商或量化程序；零散、情绪化的挂单，更像散户行为。在加密市场中，订单簿的“形态”有时比数量本身更有信息价值。

第六是短期情绪与博弈意图。买卖单的快速增减、某一价位反复挂单又撤单，可能反映出试探市场、诱导情绪或制造假支撑/阻力的行为。这在 Web3 市场中非常常见，需要结合成交记录一起观察。

第七是成交可能性与执行成本。通过观察当前最优买价和最优卖价，可以判断使用市价单是否会立即成交，以及大致会在什么价格区间内完成成交，这对于策略执行和风控非常关键。

总体而言，订单簿告诉你的不是“未来一定会涨还是跌”，而是：现在这一刻，市场上各方愿意用什么价格参与博弈，以及这种博弈是否有明显失衡。在 Web3 学习和交易中，订单簿更适合用于理解短期结构、流动性和风险，而不是单独用来做中长期判断。

## AMM总结：

AMM 的核心不在“挂单”，而在**资金池 + 定价函数**。市场中不再有买单和卖单，而是由两种资产组成的流动性池，用户直接与池子交易。池中资产的数量比例，决定当前价格；每一笔交易都会改变池子状态，从而推动价格变化。

AMM 的价格不是人为报价，而是由数学函数计算得出，最典型的是 `x * y = k`。这意味着价格是连续变化的，不存在订单簿那样的离散价位结构。交易规模越大、池子越小，对价格的冲击就越明显，这种冲击体现为滑点。

在 AMM 中，没有“最高买价 / 最低卖价”，但存在一个等价概念，即**池子的隐含现价（Spot Price）**。池子越深，价格越稳定，滑点越低；池子越浅，价格对交易越敏感，风险越高。这在效果上，相当于订单簿中的点差和深度。

AMM 和 K 线的关系是：K 线反映已经发生的交换结果，而 AMM 池子状态反映当前价格是如何被资金结构支撑的。订单簿关注的是“人愿意怎么出价”，AMM 关注的是“钱是怎么放的
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->










## 今天分享solidity复盘和最新学习的进展(已上传在本人自己的GitHub)和在学习过程中关于区块的一些疑惑(下面有解决）

-   **复习solidity内容(ERC20)**
    

1.  主要检查关于代币铸造和payable进一步的了解[my\_web3\_study/contracts/\_ERC20.sol at master · may-tonk/my\_web3\_study](https://github.com/may-tonk/my_web3_study/blob/master/contracts/_ERC20.sol)
    
2.  学习白名单[my\_web3\_study/contracts/\_DucthAuvtion.sol at master · may-tonk/my\_web3\_study](https://github.com/may-tonk/my_web3_study/blob/master/contracts/_DucthAuvtion.sol)
    

-   **如何识别钓鱼授权**
    

1.  **授权（approve）** = 允许某合约未来可以操作你代币
    
2.  **钓鱼授权** = 授权对象是恶意合约，一旦签署，对方可随时转走资产
    
3.  **防范**
    
4.  **核对合约地址和官方文档**
    
5.  **不要无限授权**，尽量选择“最小额度”
    
6.  **只操作自己主动访问的可信网站**
    
7.  **使用钱包内授权管理检查并撤销可疑授权**
    
8.  **分钱包管理**：主钱包、DApp交互钱包分开
    

-   **内容治理方式**
    

1.  **规则层**：平台政策、奖励/惩罚、透明流程
    
2.  **社区层**：投票、举报、仲裁、DAO 决策
    
3.  **技术层**：智能合约执行规则、链上存证、AI 审核
    

-   **在去中心化协作下怎么实现公平可信分配机制**
    

在去中心化协作中，核心特点是：

1.  **没有中心化管理者**
    
    -   所有参与者平等参与协作和治理。
        
2.  **多方贡献难量化**
    
    -   不同成员贡献类型不同：代码、内容、创意、审核、推广等。
        

### **设计原则**

1.  **透明性（Transparency）**
    
    -   分配规则、计算公式、操作记录全链上公开。
        
2.  **去中心化（Decentralization）**
    
    -   分配权不依赖单一机构，由智能合约或社区投票决定。
        
3.  **贡献导向（Contribution-Oriented）**
    
    -   奖励与实际贡献挂钩，避免“一刀切”。
        
4.  **可验证性（Verifiability）**
    
    -   每个参与者可独立验证分配结果，防止操纵。
        

-   **抗操纵性（Sybil-Resistance）**
    

1.  如何防止作弊、刷量、虚报贡献？
    
2.  **分配需要透明、可验证**
    
3.  每个人都能看到规则和结果，确保公平。
    
4.  **贡献量化**：先确定“谁贡献了什么”，量化成可计算指标
    
5.  **规则透明**：所有分配公式和权重上链
    
6.  **去中心化执行**：智能合约自动分配，无需信任第三方
    
7.  **社区参与**：投票或仲裁处理难以量化或争议内容
    
8.  **防作弊设计**：身份绑定、质押惩罚、多维度验证
    

## **关于Dapp的定义**

![屏幕截图 2026-01-13 213317.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/may-tonk/images/2026-01-13-1768311248576-_____2026-01-13_213317.png)

相关链接[dapps 技术入门 |](https://ethereum.org/developers/docs/dapps/) [ethereum.org](http://ethereum.org) [— Technical introduction to dapps |](https://ethereum.org/developers/docs/dapps/) [ethereum.org](http://ethereum.org)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->












# 今日学习内容：

## **_探索web3深度技术学习有关的内容并对相关部分进行总结_**

**一.DApp 如何建立在以太坊之上**？**DApp ≠ 完全去中心化的 App真正去中心化的只有：资产所有权 + 核心业务逻辑**

换句话说：

-   ❌ 前端不一定去中心化
    
-   ❌ 网站不一定去中心化
    
-   ✅ **合约一定要去中心化**
    
-   ✅ **用户私钥一定要在用户手里**
    

* * *

**二.DApp 的标准三层架构**

## 第一层：前端

### 1️⃣ 前端在 DApp 中的角色

前端本质是一个 **“区块链操作界面”**：

-   展示数据
    
-   收集用户输入
    
-   触发链上交易
    
-   调用合约的只读函数
    

📌 **前端本身不可信，也不需要可信**

* * *

### 2️⃣ 技术栈

常见技术：

-   React / Next.js / Vue
    
-   ethers.js / viem / web3.js
    
-   Wagmi + RainbowKit（现代 Web3 常用）
    

* * *

### 3️⃣ 前端能做什么 / 不能做什么

✅ 能做的：

-   调用 `view / pure` 函数（不花 Gas）
    
-   构造交易参数
    
-   请求用户签名
    
-   监听事件（Event）
    

❌ 不能做的：

-   **不能修改链上状态**
    
-   **不能绕过合约规则**
    
-   **不能代替用户签名**
    
-   **前端只是“遥控器”，不是“执行者”。**
    

* * *

### 4️⃣ 一个关键认知

> **任何人都可以写一个新的前端，直接与你的合约交互**

这意味着：

合约才是“真正的产品”前端只是“皮肤”

## **_完成web3实习手册的阅读并且给出相关的总结和思考_**

1.  \*\*_总结：_\*\*以太坊不仅是加密货币（ETH），而是支持智能合约的“全球共享计算机”，实现去中心化应用（如DeFi、NFT、DAO），代码即规则，无需中介。
    
2.  共识机制演进PoW阶段早期依赖矿工算力，TPS低（约30），能耗高。PoS转向：2020年信标链启动，2022年9月The Merge完成，主网从PoW切换PoS。验证者质押32 ETH，区块时间约12秒，能耗降99.95%。未来升级：EIP-4844（2024年上线，降低L2成本70%-90%）；数据分片（2025-2026年启动，提升扩展性）；其他如EIP-1559（费用机制）、Verkle树等
    
3.  EVM 生态系统架构：应用层（DeFi如Uniswap、Aave；NFT如OpenSea；钱包如MetaMask）；协议层（EVM、共识/执行客户端）；扩展层（L2、侧链）。
    

4.**_相关思考_**\_：\_以太坊的战略意义：作为Web3基础，以太坊通过PoS和L2解决了比特币的局限（如扩展性），但仍面临竞争（如Solana的高TPS）。在2026年，随着数据分片落地，以太坊可能进一步巩固“开发者首选”地位，推动更多企业级应用（如供应链、身份验证）。

1.  生态演化：EVM兼容性形成壁垒，许多公链（如BSC、Polygon）借此分流，但也分散流动性。思考：是否会出现“多链并存”还是“以太坊主导”的格局？L2的碎片化可能增加用户学习曲线，但Rollup-as-a-Service（如Optimism的Superchain）正简化这一问题
    

## **_阅读021学习以太坊并且给出相应心得总结_**

1.  以太坊的定义与定位：以太坊是一个全球可编程区块链，不是单纯的虚拟货币系统。它的目标是让任何人可以在链上运行自定义程序（智能合约），实现去中心化应用（DApp）。
    
2.  以太坊采用区块链技术保证不可篡改、公开透明、去中心化的特性。
    
3.  创始背景与发展简史：以太坊由 Vitalik Buterin 发起，目的在突破比特币单一支付系统的限制，引入图灵完备的智能合约执行能力。相比比特币只支持“记账/转账”，以太坊是一个可以运行任意程序的世界计算机。
    
4.  ETH和BTC核心区别：特币的核心区别以太坊的核心不只是价值转移，还包括应用逻辑执行（合约执行）。以太坊使用账户模型，不同于比特币采用的 UTXO 模型。以太坊有内置编程功能，而比特币脚本较为受限。
    
5.  以太币 (ETH) 的角色：ETH 是以太坊网络中的原生单位，用于支付交易费（Gas）、担保执行资源。ETH 在网络中扮演：用户和合约执行 Gas 支付单位系统内最基本价值载体未来 PoS 网络中质押与奖励的基础资产当你调用合约或发起交易时，你实际上是在用 ETH 购买网络的计算和存储资源
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
