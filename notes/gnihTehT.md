---
timezone: UTC+8
---

# ShadowDogee

**GitHub ID:** gnihTehT

**Telegram:** @gnihTehT

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->
# Day16

## 一个探索通过削弱“钱包感”让非技术群体参与到Web3的计划

这段学习后，我越来越觉得 web3 像阿凡达的世界观（想想之前比喻成了航海有点尴尬😅）

在这里：每个人都把信息传上链，上链后永不消失，时间久了链上就是一个数字版的你。而艾娃更像是神经系统的互联没有中心化大脑，区块链也是，它没有主服务器。艾娃不偏袒任何人，她只遵循自然平衡。合约也不会偏袒谁，它只是执行自己的代码。

这也引出了 Web3 最迷人也最难实现的一个命题：存在感”与“跨时空身份” 。

目前所有“元宇宙”项目的共同痛点：如果只是为了玩，Web2 游戏确实做得更好；但如果为了“活着”，Web3 的逻辑才有意义。

**Web2 的身份**像是在租一件衣服。你今天穿上狮子的皮，明天服务器关了，或者账号被封了，你的狮子身份就彻底“蒸发”了。它没有历史，没有积淀。

**Web3 的身份则**像是在修一个灵魂，不是一段临时代码，而是一个 **DID（去中心化身份）**。

但是，这就产生一个矛盾点，如果想让普通用户来使用web3产品那么就必须要让他理解，并且要削弱钱包感、陌生感带来的恐惧，但web3 的“灵魂永存”在现阶段是一个“逻辑上的真理”，但在“体验上是苍白的”。就像在 1990 年你告诉别人“互联网能让你足不出户看世界”，对方只会看着慢如蜗牛的拨号上网说：“我还不如去报个旅行团。” 现在的 Web3 也是如此，在大面积普及（互操作性打通）之前，这种“身份的厚度”确实很难感知。

我体验了Mastodon发现，它和web2没有什么特别大的差异，如果用户感知不到差别，他们为什么要搬家？Web3 领域最尴尬的现状：大家都说自己牛，但用户用起来觉得“也就那样，还更麻烦”。

因此，我打算探索**如何让“没有任何 Web3 基础”的群体产生代入感这**一议题。

时间不够了23：58了，先上传今天的笔记，剩下的明天写。
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->


# Day 15

### 上留言板开发速通笔记

**开头：** 今天速通了链上留言板的开发并在 GitHub Pages 进行了托管，以防有谁能看到想要尝试一下（应该没人？😅）。

**项目地址：**[my-dapp](https://gnihteht.github.io/BlockchainMessageBoard/)

开发过程遇到的几个困难及解决方案：

**1\. CORS 跨域与公共节点（RPC）的博弈**

-   **困难**：本地开发时，浏览器报错 `CORS policy`，拒绝连接合约节点。
    
-   **教训**：公共节点（如 Blockpi）有时会限制本地 `localhost` 的访问。
    
-   **方案**：更换了更稳定的公共 RPC 节点（如 `Cloudflare` 或 `PublicNode`），并学会了如何通过更换 `SEPOLIA_RPC_URL` 来解决连接问题。
    

**2\. “幽灵合约”：钱包地址 vs 合约地址**

-   **困难**：代码一直报错 `CALL_EXCEPTION`，查不到任何数据。
    
-   **教训**：误把**自己的钱包地址**（EOA）当成了**合约地址**。钱包地址里只有钱，没有逻辑代码。
    
-   **方案**：在 Etherscan 上查看交易记录，找到 `Contract Creation` 产生的那笔交易，点进去提取真正的 `Contract Address`。
    

**3\. 游客模式（无钱包）的兼容性实现**

-   **困难**：只有连接了 MetaMask 才能看到留言，普通用户（无钱包/无痕模式）打开是一片空白。
    
-   **教训**：`BrowserProvider` 依赖插件，而 `JsonRpcProvider` 可以独立于钱包工作。
    
-   **方案**：重构了 `getContract` 逻辑。当检测到用户没有安装 MetaMask 时，自动切换到 `JsonRpcProvider` 连接公共节点，实现“无感知”的数据读取。
    

**4\. ABI 的“严丝合缝”**

-   **困难**：即便地址对了，读取数据时依然报错，显示函数不存在或调用异常。
    
-   **教训**：精简版的 ABI 字符串（Human-readable ABI）在处理结构体数组（struct array）时容易出错。
    
-   **方案**：改用标准的 **JSON 格式 ABI**，明确定义了 `tuple[]` 和每一个 `internalType`（如 `uint256`），确保前端与智能合约的通信“频道”完全对齐。
    

**5\. 部署环境的“平行宇宙”**

-   **困难**：本地终端显示部署成功，但 Sepolia Etherscan 搜不到地址。
    
-   **教训**：Hardhat 默认部署在本地内存网络，关闭终端数据即消失。
    
-   **方案**：在部署命令后强制添加 `--network sepolia` 参数，并将合约真正“刻”在公链上。
    

* * *

💡 核心心得：

Web3 开发最难的不是写代码，而是**调试网络链路**。

-   **钱包模式**看权限，**游客模式**看节点。
    
-   **Etherscan** 是开发者最好的朋友，一切异常先看链上交易。
    
-   **GitHub Pages** 部署后，别忘了更新最新的 `CONTRACT_ADDRESS` 并在本地 `git push`！
    

* * *

**结尾：** 虽然目前只是个简单的留言板，但它完整经历了从合约编写、节点通信到公网发布的全过程。

下一步计划：优化一下那个长得吓人的 Unix 时间戳，再给用户地址加个缩略显示！
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->



# Day14

学习心得已发布平台^^

challenge #0终于挑战成功！
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->




# Day12

今日还在挑战 Challenge #0 但是，我有预感它快被我打通了。

目前踩了俩坑，就是没有配置和 node.js 还有 yarn
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->





# Day12

放假时间长了，开启了夜猫模式

听分享会学习了Obsidian新方法

发现大家也有卡 Challenge #0 Tokenization 所以没那么沮丧了，打算再多尝试几次，希望能够成功
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->






# Day11

今天这个挑战还是没过去😭，但是不是我操作问题是我下错版本了，诶继续加油！总会成功的

以下是 GPT 老师给我总结的雷

1.  **Node 版本过高 / 不稳定**
    
    -   **踩点**：Node 24.x
        
    -   **症状**：`Assertion failed: !(handle->flags & UV_HANDLE_CLOSING)`
        
    -   **原因**：Windows + Node 高版本 + libuv 原生模块崩
        
    -   **教训**：Windows 下务必用 **LTS（20.x）**
        
    -   **解决办法**：卸掉所有 Node、彻底清理 PATH → 装 Node 20 LTS → 重启
        
2.  **Yarn / npm 冲突**
    
    -   **踩点**：yarn v1 + node v24 / 内置 corepack
        
    -   **症状**：无法找到 `where yarn` 或 `where node`，环境残留
        
    -   **原因**：Windows PATH 残留多版本 Node/Yarn
        
    -   **解决办法**：彻底卸掉 Node + Yarn → npm 安装 yarn → rebuild 项目依赖
        
3.  **PowerShell 执行策略**
    
    -   **踩点**：`yarn chain` 报 `无法加载文件 ... 因为在此系统上禁止运行脚本`
        
    -   **原因**：PowerShell 默认 ExecutionPolicy 限制
        
    -   **解决办法**：`Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`
        
4.  **残留幽灵 Node**
    
    -   **踩点**：运行 yarn 脚本能跑，但 verify 直接崩
        
    -   **原因**：corepack 或旧 shell 继承了 Node 24 runtime
        
    -   **解决办法**：关掉所有终端 → 重启 → 确认 `node -v` & `where node`
        

* * *

# 🟡 **合约 / Hardhat 雷区**

1.  **部署 vs 验证**
    
    -   **踩点**：合约部署成功，但 `yarn verify` 崩
        
    -   **原因**：Node / 环境问题，不是 Hardhat
        
    -   **教训**：部署和测试成功 ≠ 验证成功，验证依赖原生模块 + 网络
        
2.  **测试链 / 钱包**
    
    -   **踩点**：生成钱包后余额为 0
        
    -   **症状**：mintItem 仍然可以通过 Hardhat 测试
        
    -   **教训**：Hardhat 本地链可以测试逻辑，不依赖实际网络 ETH
        
3.  **网络连接**
    
    -   **踩点**：无法连接 Sepolia / 其他 testnets
        
    -   **原因**：网络超时、API Key、Node 异常
        
    -   **教训**：网络问题先排除环境再排查合约
        

* * *

# 🟢 **日常操作雷区 / UX坑**

1.  **残留 lockfile**
    
    -   **踩点**：yarn.lock 旧版本 / SWC patch
        
    -   **教训**：`rd /s /q node_modules` + `del yarn.lock` + `yarn install` 可以复原
        
2.  **合约地址 / Etherscan**
    
    -   **踩点**：verify 提示 “Already verified”
        
    -   **教训**：只要页面显示源码可见 → ✅ 这步过了
        
3.  **虚拟钱包 / 本地测试**
    
    -   **踩点**：生成钱包后忘记密码 → 数据加密解密失败
        
    -   **解决办法**：务必记住密码，或者重生成钱包
        

* * *

# 💎 **终极教训总结**

1.  **Node 环境优先于合约 / Web3 逻辑**
    
    > 你这几天所有的崩溃，都是 Node + Windows 工具链问题，不是代码问题。
    
2.  **测试链和本地 Hardhat 可以先跑**
    
    > 不要依赖真实网络去调试合约，先保证本地脚本能跑
    
3.  **验证阶段最容易被忽略**
    
    > Verify 依赖 Node 原生模块 + 网络，环境干净才安全
    
4.  **PowerShell / PATH / Cache 是常见隐形坑**
    
    > 每次换 Node 版本 + Yarn 版本，先彻底清理 node\_modules + lockfile + cache
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->







# Day10

今天正在挑战Tokenization#0，但是我的网络配置有问题，花了我一下午的时间都没解决好TuT，本来想着或许可以在剩下的时间完成，但是发现马上不够记笔记打卡了。

但是今天有个好消息，成功申请上 了 Hong Kong Consensus 的学生票（多谢群里大佬的攻略！），还找到了一个大使机会，希望以后也可以走上 Web3 资深老手之路！
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->









# Day 9

今日通过了 Ethernaut 前 3 关

## 通关流程

第一关是关于console 操作

```
await contract.info()
→ info1()

await contract.info1()
→ info2("hello")

await contract.info2("hello")
→ 看 infoNum

await contract.infoNum()
→ 42

await contract.info42()
→ 看 theMethodName

await contract.theMethodName()
→ "method7123949"

await contract.method7123949()
→ 提示调用 authenticate(password)
```

第二关是关于ownership主要用到了这个，通过合约转账再提款

```
\\贡献一点
await web3.eth.estimateGas({
  from: player,
  to: contract.address,
  value: web3.utils.toWei("0.001", "ether")
})
\\验证
await contract.getContribution()
\\提取
await contract.withdraw({ from: player })
\\检查余额
await web3.eth.getBalance(contract.address)
```

第四关是拼写错误

```
function Fal1out()
```

旧版本的solidity的构造函数是

```
function Fallout() public { }
```

写成Fal1out意味着名字不匹配合约名，它只是一个普通 public 函数。

任何人、在任何时间、都可以调用这个函数、并成为 owner。

## 总结

这三关本质上是在建立一个最重要的 Web3 心智模型：合约 ≠ 你以为的“有界面函数”，而是一坨可以被“用各种方式触发”的状态机。

-   第一关 Fallback 理解不通过 ABI 也能转账，从而触发 receive / fallback 改 owner”；
    
-   第二关 Fallout 让你理解“构造函数如果写错，就会变成任何人都能调用的普通函数，从而重置 owner”；
    
-   第三关，理解“链上真正发生的是一笔交易，而不是 JS 函数调用，gas、value、data 才是本体”
    

三关合起来就是一句话：安全不取决于你‘以为’用户怎么用合约，而取决于“合约在 EVM 层面能被怎么调用”。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->










# Day 8

## 运营

今天初步搭建了 GameFi 共研小组，不过大部分事情，比如，如何去组件活动还在商议中^^

我的想法是通过 [Itch.io](http://Itch.io) 平台办一场 GameFi GameJam

我阅读了**星星狗 Karen** 老师的运营起号笔记，其中有一句话对我的启发很大：

“Web3时代，推特主要用于打造个人名片，是一个具有极强用户特色的平台，在这里“赛道”并不是最重要的，如何让别人“**认出**”你才是最重要的“

在加上我们都知道，自我发展与学校学习的最大区别就是：学校里要求我们查缺补漏，去补段板，而自我发展则更要求我们去发挥自己的最大优势，去无限延长自己的长处。在绞尽脑汁的思考过后，我给自己简单进行了一下SWOT分析，发现自己在这里最有可能成为记忆点的地方就是我的背景：

**游戏和绘画**

Karen 老师的笔记里还写了”**创作要有利他性**“，而绘画和游戏最大的利他性是什么——**情感利他**，所以我果断选择了以漫画为载体来记录我的 Web3 之旅，不过最终原因还是因为群内大佬太多我只能用这个作为我的记忆点。/(ㄒoㄒ)/

最后，预祝我们的 GameFi 共研小组能够成功，如果有机会希望真的从这里走出可以上链且火爆的 GameFi Game

\*★,°\*:.☆(￣▽￣)/$:\*.°★\* 。

## 技术

今天技术部分暂时没有进行，但是因为预习也打上了卡嘿嘿

**群内大佬太牛使我焦虑**，所以上星期就多学了一点solidity，没想到竟然还可以打上深度技术的卡！

希望我双修成功！！！！
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->












# DAY7

做了一周的总结，画成了漫画。

我认为这一周的时间里，我已经开始熟悉的Web3的环境，并且已经开始接触了solidity。

再次庆幸能够参加到这个实习计划中来，不然我的进度会远比这慢好多好多。

已经开始筹划了接下来的计划，我打算主攻深度技术里的solidity，然后运营为辅助。

我已经写好了一个Dapp的策划案，只剩下我把solidity技术补齐，然后完成剩余的部分。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->













# DAY 6

今天把昨天的第一次例会又过了一遍，并在早上参加了LXDAO的周会，因为太i了怯场没能在这次自我介绍，很遗憾，希望能够趁下次机会加入！

我终于定下了未来几周的计划，我希望我能够学成solidity，最好能够过完深度技术的所有内容，然后挑战一下自我，尝试过一下运营的课程。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->















# **Day5**

今天听了co learning和第一次例会，非常拓宽眼界！明天将把今晚的内容整理成一个笔记，今天的干货真的太多了，比在学校的学习知识密度要高很多倍！

今天花了大半天的时间从学校收拾回家，如果没有继续写东西那么就请假一天。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
















# DAT4

这次分享会的内容比较复杂所以不是特别明白，下面这句话是由GPT帮我整理的。

这次分享的核心：AI 正在从“只会说话的工具”进化成“能干活、能收钱、能承担责任的经济主体”，而 Web3（区块链）是支撑这种“机器经济”的唯一可行基础设施。

今天刚刚考完试，没来得及参加co-learning很可惜，但是寒假已经开始了，作为学生能有大把时间在没有打断的情况下用来学习web3了！因为考了一整天，今天精力不是很多，就把昨天与前天的内容又过了一遍，学习部署了一个简单的合约。

## 学习内容

今天在Remix里学习了部署合约到Sepolia测试网

```remix-solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;
contract Counter{
    uint256 public count;

    function set(uint256 _n) public {
        count = _n;
    }

    function add() public {
        count += 1;
    }
}
```

这个合约的内容是在链上存一个数，任何人可以改，任何人可以+1，任何人可以读。

并按照`count`、 `add`、 `count` 的顺序实验了功能。

## 接下来的打算

接下来我打算给这个合约添加条件，变为只有我能修改、不允许超过十

## 新认知

我一直望文生义感觉合约就像是一个代码版的合同，但是事实上它和合同的最大的区别的，合约不在乎你是谁、你想干什么、合不合理，因此这也引出了钱包安全的重要性。它只是一个被拿来执行“合同式规则”的不可变程序，就算是黑客也能利用它得到东西。就像之前的分享会还要实习手册中有提到过，Smart Contract这个名字起的不是特别的好，它的本体只是一个“残酷”执行的程序。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->




















DAY3

今天准备去学习剩下solidity入门课程，把入门通了一遍。

明天考试，今天大多时间都在复习，请假一次笔记
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->






















# Day 2

今天开始学习Solidity🎉

发现Solidity和C#还是有很大的相似之处

目前是学习到了biginner章节，过了一遍变量Variables，函数Functions感觉接受还是很良好的。我决定快速过一遍后就开始尝试一下部署合约或者深度技术里的挑战。

今天听了布老师对Web3运行原理的讲解，对Web3运作有了一个更清晰的概念。不过也产生了一些小疑惑或许可以随时间解开（比如，**如何才能更好的利用Web3来保护版权？**），目前接触到的新信息和我在能系统学习接触之前对Web3的想象有相似也有很多的不同，有些甚至让我有些震惊。

在我看来目前Web3是结合了社区自治和代码治理的一个新互联网概念，虽然目前不能完全规避中心化，但是社区都做尽了力。

布老师在最后提出的问题也非常有思考价值，先贴在笔记里供自己慢慢思考。

![c01a22e4543fbc5b056bfd7d89a1e382.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-13-1768312866082-c01a22e4543fbc5b056bfd7d89a1e382.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

























# Day1 共学第一天

今天是加入实习计划第一天，作为一个只打过gamejam的大学生、真正意义上的纯小白，看到群内一起共学的伙伴都有相关经历感觉非常的厉害。为了认识更多的朋友，我建立了一个山东地区的共学小组（十分紧张），希望接下来的时间通过学习我也可以成为自己现在仰慕的业内人士并加入LX DAO ^^

# 学习笔记

## 网络钓鱼Phishing Attempt

### 1\. 假装客服

对方会假装是钱包客服来询问钱包的**助记词Seed phrase** , 事实上钱包客服是绝对不会去询问的。助记词是是恢复钱包的私钥，如果别人得到了它就立刻拥有了你全部资产的控制权。

**如何保护钱包**

-   不要给任何人助记词
    
-   将助记词存在线下一个安全的地方
    
-   使用硬件钱包
    
-   如果你收到信息问你要助记词那这基本上就是钓鱼
    
-   怀疑时立刻结束对话并联系官方渠道
    

### 2\. 钓鱼授权

一旦在主网或有价值资产的地址上签过，对方就可以随时把你钱包里的USDC直接转走，不需要你再确认。

**示例**

```
{
  "domain": {
    "name": "USD Coin",
    "version": "2",
    "chainId": 11155111,
    "verifyingContract": "0x74A4A85C611679B73F402B36c0F84A7D2CcdFDa3"
  },
  "types": {
    "Permit": [
      {
        "name": "owner",
        "type": "address"
      },
      {
        "name": "spender",
        "type": "address"
      },
      {
        "name": "value",
        "type": "uint256"
      },
      {
        "name": "nonce",
        "type": "uint256"
      },
      {
        "name": "deadline",
        "type": "uint256"
      }
    ]
  },
  "message": {
    "owner": "0x0A874a7687695Af1BA3c823fE73ED208867BE554",
    "spender": "0x1234567890123456780012345678901234567890", //骗子地址
    "value": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff", //unit256 最大值=无限额度授权
    "nonce": 378622,
    "deadline": 1770786450 //长期有效
  },
  "signature": "0x18bcb7f1f51a7d5d19e8e2316079cbfac59c509b48713ac19044860e6a42cb4d417e51afaa8cb3a4c329d9a5ff70a7b7d0a8d26157cb0d29fd96dc931e8eed231c",
  "r": "0x18bcb7f1f51a7d5d19e8e2316079cbfac59c509b48713ac19044860e6a42cb4d",
  "s": "0x417e51afaa8cb3a4c329d9a5ff70a7b7d0a8d26157cb0d29fd96dc931e8eed23",
  "v": 28
}
```

# **转测试币练习**

## 1\. 打开钱包点击发送

![QQ_1768202253475.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202256483-QQ_1768202253475.png)

## 2\. 选择SepoliaETH

![QQ_1768202286051.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202289155-QQ_1768202286051.png)

## 3\. 输入钱包地址并继续

![QQ_1768202306857.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202309830-QQ_1768202306857.png)

## 4\. 点`活动`可以查看状态

![QQ_1768202330049.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202338716-QQ_1768202330049.png)

## 5\. 点击这个状态然后`在区块浏览器上查看`

![QQ_1768202389649.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202394518-QQ_1768202389649.png)

## 6.复制**Transaction Hash**即可提交作业

![QQ_1768202463799.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/gnihTehT/images/2026-01-12-1768202468191-QQ_1768202463799.png)

## 如何查看收款记录？

由于MetaMask`活动`UI里只会显示由你发起的活动，所以收款记录要在[https://sepolia.etherscan.io](https://sepolia.etherscan.io)上输入钱包地址查看。

## 名词解释：

**Etherscan**以太坊区块链浏览器

**Etherscan tx link**是某一笔链上交易的“官方浏览器页面链接”

每笔交易都有一个**Transaction Hash**交易哈希

把它丢进 **Etherscan**，就能看到：

-   谁 → 给谁
    
-   干了什么
    
-   调了哪个合约
    
-   授权了什么
    
-   成功还是失败
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
