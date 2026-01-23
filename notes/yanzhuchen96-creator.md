---
timezone: UTC+8
---

# yanzhuchen96-creator

**GitHub ID:** yanzhuchen96-creator

**Telegram:** @cydot0906

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->
## 漏洞修复笔记

今天学习了漏洞修复的相关内容，重点学习了重入攻击 Reentrancy。慢慢学下来，我发现所谓重入攻击，本质就是：**合约在给别人转钱时，把控制权交了出去，但还没来得及更新自己的内部状态，结果对方在这个空档又跑回来重复执行提现逻辑，把同一份余额当成多份来用。**

我学习的例子是一个很典型的“存取款”合约。

```
// ❌ 有漏洞
function withdraw() public {
    require(balance[msg.sender] > 0);
    (bool sent,) = msg.sender.call{value: balance[msg.sender]}("");
    require(sent);
    balance[msg.sender] = 0;
}

// ✅ 修复后
function withdraw() public {
    uint256 amount = balance[msg.sender];
    balance[msg.sender] = 0;
    (bool sent,) = msg.sender.call{value: amount}("");
    require(sent);
}
```

里面有个 mapping(address => uint256) balances;，就像一个账本，给每个地址记它在合约里的余额。存款的时候，会调用类似 balances\[msg.sender\] += msg.value; 这样的代码，也就是把这次转进来的 ETH 加到调用者的余额里。 提现的时候，先用 require(balances\[msg.sender\] >= \_amount, "Not enough balance"); 检查调用者的账面余额够不够，然后用 msg.sender.call{value: \_amount}("") 把钱转出去，最后再 balances\[msg.sender\] -= \_amount; 把余额减掉。刚开始我以为这很自然：先给用户钱，再改账本，结果事实恰好相反，这正是重入攻击的入口。

真正搞清楚问题，是把角色拆成合约 A 和合约 B 之后。A 是受害者 Bank 合约，负责记账和存取款；B 是攻击者写的合约。平时用户（或者合约 B）存钱时，A 里会记 balances\[B\] += 存入金额。当 B 调用 A 的 withdraw 时，A 先检查 balances\[B\] >= \_amount，然后执行 msg.sender.call{value: \_amount}("") 把钱转给 B。如果 B 是合约，这个转账会触发 B 的 receive() 或 fallback() 函数，在那个回调里，B 可以再次调用 A.withdraw()。关键在于：此时 A 还没执行 balances\[B\] -= \_amount，也就是说账本上的余额还没减少，第二次进入 withdraw 时，require 一样能通过，于是 A 又给 B 转一次钱。这样在一次交易之内，B 可以不停重入，反复用同一条 balances\[B\] 记录，把 A 这个公共资金池里的钱往自己这边拖走， 而其他老老实实存钱的用户就变成了帮 B 垫背的人。

还有一点可以帮助理解这个漏洞：合约里有两个层面的钱。一层是 address(this).balance，这是整个合约账户现在实际有多少 ETH，就像一个总资金池；另一层是 balances\[某地址\]，这只是账本上的数字，表示这个人理论上可以提多少。重入攻击的目标不是把 balances\[B\] 改大，而是用同一个 balances\[B\]，在同一笔交易里调用多次提现逻辑，让资金池里的钱一次次流到 B 那边。require(balances\[msg.sender\] >= \_amount) 只保证“你至少得有这一份余额”，但它挡不住“你多次利用同一份余额”的问题。

在弄懂攻击原理之后，我动手做了一个属于自己的漏洞修复练习。先让 ai 写一个有问题的版本（和教程里的类似）：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MyVulnerableBank {
    mapping(address => uint256) public balances;

    function deposit() external payable {
        // 存款：给调用者记账
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint256 _amount) external {
        // 取款前检查余额够不够
        require(balances[msg.sender] >= _amount, "Not enough balance");

        // ❌ 有漏洞：先对外转钱
        (bool success, ) = msg.sender.call{value: _amount}("");
        require(success, "Transfer failed");

        // ❌ 再改余额
        balances[msg.sender] -= _amount;
    }
}
```

这个版本的问题已经很清楚：一旦 msg.sender 是合约 B，就可以在收钱时重入 withdraw，利用还没更新的 balances\[msg.sender\] 重复提款。 所以我按照“检查‑效果‑交互”（Checks‑Effects‑Interactions）模式，把它改成安全版：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MySafeBank {
    mapping(address => uint256) public balances;

    function deposit() external payable {
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint256 _amount) external {
        // 1. Checks：检查调用者余额是否足够
        uint256 userBalance = balances[msg.sender];
        require(userBalance >= _amount, "Not enough balance");

        // 2. Effects：先更新内部状态
        balances[msg.sender] = userBalance - _amount;

        // 3. Interactions：最后才对外部地址转账
        (bool success, ) = msg.sender.call{value: _amount}("");
        require(success, "Transfer failed");
    }
}
```

在这个修复版里，如果 B 第一次调用 withdraw，在发起转账之前，balances\[B\] 已经先被减去了 \_amount。即便 B 在 fallback 中再次重入 withdraw，第二次进入时读到的 userBalance 已经是减少后的值（可能是 0），require(userBalance >= \_amount) 会直接失败，不再给它发第二笔钱。 对我来说，这个改动让我非常直观地体会到了 CEI 模式的力量：仅仅调整几行代码的顺序，就能把一个典型重入漏洞“掐死在入口”。

总结一下，修复这类漏洞的第一件事，就是记住一个很经典的开发模式：**Checks‑Effects‑Interactions（检查‑效果‑交互）**。把那段不安全的 withdraw 改写成三步：第一步先 require 检查余额足够；第二步立刻更新状态，把 balances\[msg.sender\] 减掉，甚至直接清零；第三步才调用外部的 call 去给 msg.sender 转钱。 这样做之后，就算对方在转账回调里重入，第二次进来看到的 balances\[msg.sender\] 已经是更新后的值（通常是 0），在 require 那一步就过不去了，自然拿不到第二轮的钱。等于说：**重入还是可能发生，但因为状态已经先动了，所以攻击逻辑失效了。**

作为 Solidity 初学者，这次围绕重入攻击的学习，让我从一开始只会照搬别人写好的 withdraw，变成会拿着代码从攻击者视角去审一遍：这里有没有 call、delegatecall、transfer 之类的外部调用？在这些调用之前，我有没有先把余额、状态位、计数器等敏感变量更新好？ 以后每当要写“给别人转钱”的逻辑，无论是简单的提款合约，还是复杂一点的质押、借贷、DEX，我都会先在脑子里问一句自己：“如果对方是个恶意合约，它在 fallback 里立刻再调回来，我现在这段顺序顶得住吗？”
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


# DAPP学习笔记

## 基本概念与本质

**学术一点的解释**：DAPP（Decentralized Application，去中心化应用）是运行在区块链上的应用，相比传统应用不依赖单一中心化服务器，而依托区块链的去中心化特性，因此具有去中心化、去信任、去中介等特征。如果严格要求全链上、全去中心化，访问速度、使用流程等体验会非常差，所以现实中的 DAPP 往往在链上只保留核心逻辑，其余查询、展示类功能交给中心化服务处理。例如像 Uniswap 这样的去中心化交易所，代币列表、价格、交易量等信息常由中心化服务先从链上抓取和索引，再通过 Web API 或 GraphQL 接口供前端使用，从而在保证安全的同时提升用户体验。

**通俗版**：只要应用需要与区块链进行读写交互、核心逻辑围绕智能合约展开，就可以视为 DAPP，而不是必须让前后端全部完全去中心化。智能合约本身也可以被视作一个 DAPP 的核心，即使没有图形界面，用户依然可以直接通过钱包或脚本与合约交互，只是为了让更多普通用户使用，通常会额外配一层交互界面。

## 架构

从整体架构上看，一个典型 DAPP 可以粗略拆成三部分：**中心化的前端**、**中心化的后端**以及**去中心化的智能合约**，其中真正运行在链上的只有合约这部分。不过，要让 DAPP 真正稳定可用，还离不开若干基础设施：去中心化文件存储 IPFS、提供链上读写接口的 RPC 服务商、用来做复杂查询的索引服务商，以及在链上引入现实世界数据的预言机等。

IPFS（星际文件系统）可以看作一个去中心化的文件存储网络，文件会被分布式存放在多台服务器上，通过内容哈希来定位和去重，用户只要知道哈希就能找到对应的文件内容。免费存储的代价是数据可能会被清理，因此出现了结合 Filecoin 的付费持久化服务，通过经济激励让节点长期保存数据，使 DAPP 可以更可靠地存放图片、元数据等静态资源。

## 交互

普通 DAPP 不需要自己“维护”区块链，只需要通过接口“使用”区块链功能，因此会依赖专业 RPC 服务商运行各类节点，并按照以太坊 JSON‑RPC 规范对外暴露读写接口。开发者通过这些接口可以查询区块、交易、账户状态，或发送交易、调用合约方法，就像调用普通 Web API 一样，只是请求格式更加规范化。

但仅有 RPC 远远不够，因为 RPC 更像是“查当前状态”，缺乏对历史数据做复杂聚合的能力，这时就需要索引服务商把每个区块里的相关数据提取出来写入自建数据库。借助这类索引服务，开发者可以通过 GraphQL 等方式方便地查询某个合约所有历史 mint、transfer、burn 记录，或统计某个 NFT 合约的所有持仓者，大幅减少自己从创世区块开始逐块扫描的开发负担。

## 预言机与链下数据

预言机（Oracle）是连接区块链和现实世界数据的桥梁，解决“链上系统如何安全获得链下信息”的问题，例如获取 ETH 的实时价格以供 DeFi 协议使用。常见做法是从多个交易所（如 Coinbase、Binance、Kraken）拉取价格数据，使用加权平均和离群值剔除等策略得到一个相对可靠的价格，并要求预言机节点质押代币，如果上报错误数据就会被罚没，从而通过经济机制保证诚实性。

以 Chainlink 为代表的预言机网络已经为成千上万个 DeFi 协议提供喂价服务，像 Uniswap 上的交易定价、Aave 上的借贷清算条件，都高度依赖这类价格数据的准确性和实时性。在 DAPP 设计时，只要智能合约逻辑涉及外部世界（价格、预言事件等），就几乎离不开一个可信、去中心化的预言机网络来提供输入。

## DAPP 开发工具链概览

在通用技术栈上，优先学好 JavaScript，因为掌握 JS 后基本可以一人跑通从前端到后端再到链上交互的全流程。前端方面常用 Next.js 作为框架，配合 Tailwind CSS 做样式，结合 shadcn/ui 等组件库完成界面搭建，再用 Zustand、Zod、TanStack 等工具处理状态管理、表单校验和数据请求等通用需求。

部署和协作方面，可以用 Vercel 快速上线前端，用 Coolify、NorthFlank 等方案处理更复杂后端部署，再借助 Figma、Excalidraw 完成设计和原型沟通。后端可选 Next.js 的 API Routes + serverless 函数应付简单场景，复杂业务则使用 NestJS + Prisma + PostgreSQL 这类组合，存储层使用 Infura 或 Pinata 提供的 IPFS 服务保存与链上 NFT、元数据相关的文件。​
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->





# 1.21 学习笔记：dApp 留言板实战 + gas 优化

## 一、从“前端在哪跑”开始搞懂 dApp 架构

今天一开始，我照着教程部署了留言板合约到 Sepolia，但我不确定前端代码到底应该在哪里跑。通过折腾之后，我终于搞清楚：合约跑在区块链节点上，而 messageboard.html 这样的前端只是一个普通网页，只要通过 HTTP/HTTPS 在浏览器里打开就行，关键是它要能访问到 MetaMask 暴露的 window.ethereum。

我一开始是直接双击本地文件，用 file:///...messageboard.html 打开的，结果网页怎么都连不上钱包。后来了解到 MetaMask 出于安全原因不会给 file:// 页面注入 provider，只有在 http:// 或 https:// 下才会注入，所以我必须用本地服务器来跑这份 HTML，比如用 VS Code 的 Live Server 或 Python 的简单 HTTP server。

## 二、用 VS Code Live Server 跑起本地前端

接下来我在 VS Code 里一步步配置 Live Server。首先需要用 “File → Open Folder” 打开包含 `messageboard.html` 的整个文件夹，而不是只打开单个文件，这样 Live Server 的右键菜单和右下角 “Go Live” 按钮才会出现。

打开文件夹后，我在资源管理器中双击 messageboard.html，在编辑区右键就能看到 “Open with Live Server”。点了之后浏览器地址变成类似 http://127.0.0.1:5500/messageboard.html，这时 MetaMask 能正常注入，连接钱包弹窗也能弹出来。我也知道了这种 localhost 链接只在我这台电脑上有效，想让别人访问必须部署到 GitHub Pages、Vercel 之类的静态网站托管，或者用 ngrok 把本地端口临时暴露出去。

## 三、理解 storage、SLOAD 和“缓存 length”的 gas 优化

在看教程的 Gas 优化部分时，我碰到了那段示例代码：

```
// ❌ 非优化写法
mapping(address => uint256) public balances;
function deposit() public payable {
    balances[msg.sender] += msg.value;
}

// ✅ 优化写法（一次读，一次写）
function deposit() public payable {
    uint256 current = balances[msg.sender];
    balances[msg.sender] = current + msg.value;
}
```

一开始我不明白这两段看起来一样的代码为什么说后面更省 gas，后来才知道关键在于 SLOAD。

我学到：合约的持久化状态存在 storage 里，访问像 arr.length、balances\[user\] 这样的 storage 变量时，EVM 会用 SLOAD 指令从链上存储读出 256 位数据；而 SLOAD 是比读内存、读栈贵很多的操作。如果在循环条件里直接写 i < arr.length，每次循环都可能触发一次 SLOAD；而先写 uint256 len = arr.length; 只在循环开始前读一次 storage，后面循环里只是和内存变量 len 做比较，成本就低很多。

我也顺带搞清了 SLOAD 的概念：就是“从合约的 storage 某个 slot 读 256 位数据到栈上”的底层指令，它是 gas 优化里经常被提到的“贵操作”之一。所以教材强调的“缓存 length”本质就是减少重复 SLOAD，把多次读 storage 变成一次读 storage + 多次读内存。

## 四、给 MessageBoard 合约做 gas 优化

然后我用今天学到的东西，对教程里的 MessageBoard 合约做了几处简单优化，目标就是贴合那篇文章里的“入门级 gas 优化”，而不是大改架构。

原来的关键部分是这样的：

```
// 发送一条留言
    function leaveMessage(string memory _msg) public {
        messages[msg.sender].push(_msg);     // 添加到发言记录
        emit NewMessage(msg.sender, _msg);   // 发出事件
    }
```

我做的改动有两点：

改了留言板的合约代码

**改动 1：public → external**

留言板这个场景里，leaveMessage 只会被用户通过交易/前端调用，不会在合约内部调用自己，所以用 external 没问题，gas 费也更低。

**改动 2：memory → calldata**

memory 的写法含义是：

-   先把 calldata 里的参数拷贝一份到内存（多一步拷贝）；
    
-   再从内存把字符串写进 storage
    

把 msg 的数据位置从 memory 改成 calldata，Solidity 就可以在写入 storage 时，直接从 calldata 读取 msg，避免“calldata → memory”的中间拷贝步骤。

于是优化后的版本是：

```
function leaveMessage(string calldata _msg) external {
        messages[msg.sender].push(_msg);
        emit NewMessage(msg.sender, _msg);
    }
```

通过这两处改动，我体会到“简单但正确”的 gas 优化：不需要写很花哨的技巧，只要根据函数的真实用途选对可见性（external / public），为大对象参数用 calldata，以及在循环里避免重复 SLOAD，就能在不牺牲可读性的前提下把合约写得更省。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->






今天做入门技术的一个任务，啃完了 Ethernaut 的前三关，花的时间比自己想象中的要久，作为一个 Solidity 初学者，要一行一行读懂智能合约还是有点难度的。通过 Hello Ethernaut、Fallback 和 Fallout 这三关，我从完全没用过浏览器控制台，到能看懂合约逻辑、定位漏洞并写出攻击代码，感觉自己被硬生生推着跨了一小步门槛，过程很痛苦，但进步还挺大。

## 第 0 关：Hello Ethernaut

这一关其实没有真正的漏洞，更像是一个“新手村教程”。我第一次学会在浏览器的 Console 里，用 JavaScript 去调用链上的合约方法，比如

```
await contract.infoNum()
```

看到返回各种谜语一样的字符串提示，然后根据提示一步步调用下一个函数。以前看教程都只说“合约有函数”，但我第一次意识到：在前端，其实是用 Web3/ethers 这类库，把合约当成一个 JS 对象 contract，然后 [contract.xxx](http://contract.xxx)() 就是在发起 RPC 调用。为了拿到真正的返回值，我得在 Console 里加上 await，不然拿到的只是一个 Promise，打印出来就是 “\[object Promise\]”。这让我第一次搞明白了异步和 await 在 Web3 里的实际用法：await 就是“等链上的调用完成，把结果拿回来再继续”。

在这一关里，密码本身是通过

```
contract.password() 
```

这种公开的 getter 暴露出来的，最后再用

```
contract.authenticate(password) 
```

通关。对我来说，这一关最大的收获不是“破解”，而是学会了三件事：

-   怎么在 Ethernaut 里点 “Get new instance” 拿到自己的靶场合约实例；
    
-   怎么在浏览器控制台里用
    

```
await contract.xxx() 
```

和合约交互；

-   以及怎么理解“public 变量自动带 getter 方法”这个 Solidity 特性——比如
    

```
string public password; 
```

会自动生成 password() 让外部读。

## 第 1 关：Fallback

Fallback 这关的合约让我认真看了一份完整 Solidity 源码，并且从“读代码”到“设计攻击步骤”完整走了一遍。合约核心是：用 mapping(address => uint256) public contributions; 记录每个地址的贡献额，用 address public owner; 记录当前 owner，在构造函数里把部署者设成 owner 并把 contributions\[owner\] 设成一个巨大的值。只有 owner 才能调用 withdraw() 把余额提走，这里的权限控制通过 modifier onlyOwner 和 require(msg.sender == owner) 实现，这让我第一次明确理解了 modifier 的语法：\_ 代表真正的函数体插入在这里执行。

真正有意思的是两个入口：一个是 contribute()，一个是 receive()。contribute() 里有 require(msg.value < 0.001 ether);，然后把 msg.value 加到 contributions\[msg.sender\] 上；如果当前地址的贡献超过 owner，就把 owner 改成这个地址。另一个是 receive() external payable：只要我之前贡献过（contributions\[msg.sender\] > 0），再给合约发一笔纯转账（带 msg.value > 0、没有调用 data），这个 receive() 就会被触发，直接 owner = msg.sender;。这让我第一次理解了 Solidity 里的特殊函数：receive() 是“收钱但没有 data 时会触发的函数”，而它如果设计得不安全，就会成为一个隐藏的攻击入口。

在实战通关时，我写出了看起来“有点像脚本”的 Console 调用流程：先 await contract.contribute({ value: web3.utils.toWei(“0.0005”,“ether”) })，再 await contract.sendTransaction({ value: web3.utils.toWei(“0.0005”,“ether”) })，利用 web3.utils.toWei(“0.0005”,“ether”) 把人类好读的 ETH 小数转换成链上用的 wei 整数。之前我完全不知道 web3.utils.toWei 是干嘛的，现在我知道凡是给 value 填 ETH 数的时候，都应该想到它。最后，成为 owner 以后，再 await contract.withdraw() 把合约余额清零。这个过程把很多零散概念串起来：msg.value 的单位是 wei、public payable 函数可以收钱、合约地址本身也有 balance，以及“隐藏在 fallback/receive 的权限漏洞”。

## 第 2 关：Fallout

Fallout 这一关让我感受到“一个小小的命名错误，会直接变成致命漏洞”。合约用 using SafeMath for uint256; 引入了 OpenZeppelin 的 SafeMath，对 allocations 这个 mapping(address => uint256) 做安全加法，同时定义了 address payable public owner; 来保存 owner。但是非常关键的一行是：function Fal1out() public payable { … }。注释写着 /\* constructor \*/，但函数名实际上是 Fal1out（中间是数字 1），而合约名是 Fallout。在 0.6 版本里，构造函数应该写成 constructor()，这种“同名函数当构造函数”的老写法已经失效，所以这段其实是一个普通的 public 函数，任何人都可以后来调用。里面的逻辑却是构造函数该干的事：owner = msg.sender; allocations\[owner\] = msg.value;。也就是说，只要我随时调用 Fal1out()，就能把 owner 改成自己。

这关的实战非常直接，我觉得甚至比第一关还简单，也有可能是因为我第一关仔细研究过后打好了基础。我先 await contract.owner() 看当前 owner，然后直接 await contract.Fal1out()（可选地附带一点 value），再查一次 owner，就已经变成了我的地址。拿到所有权之后，还可以调用 collectAllocations()：这个函数被 onlyOwner 修饰，只允许 owner 调用，内部是 msg.sender.transfer(address(this).balance);，一行代码把合约所有余额转给当前 owner。相比上一关，这一关的“攻击代码”反而更短，但难点在于对语言细节的理解——构造函数语法的变化、函数名和合约名严格一致的重要性、以及 address payable 和 .transfer 的搭配。

这一关也让我重新审视注释的可信度：代码里写着 /\* constructor \*/，但编译器真正认的是语法和名字，不是注释。作为审计或者攻防的一方，不能只相信注释说明，要精确看函数签名和 Solidity 版本。在 0.6+ 里如果还写“同名函数构造”，反而会变成一个任何人都能调用的后门。

## 收获

通关前三关之后，我最大的变化是：不再把智能合约当作“黑盒 API”，而是开始习惯先看源码、找 owner 是怎么被设置的、有哪些函数能修改 owner，再根据控制台能调用的接口，设计出一条通往目标的交互路径。在 Hello Ethernaut 里，我只是机械地照着提示调 infoNum()、info42()、theMethodName()、password()、authenticate()；在 Fallback 里，我开始关注 receive() 和 contribute() 的条件组合，意识到“特殊函数 + 条件不严”就是攻击面；到了 Fallout，我则专注于构造函数和权限初始化，体会到版本变更对安全的影响。

技术细节上，我也习惯了几个模式：在浏览器 Console 里，所有链上交互几乎都要加 await 才能拿到返回值；如果遇到金额，就用 web3.utils.toWei(“数字”,“ether”) 转成 wei；遇到 mapping、modifier、receive、constructor 时，会顺手想一想它们可能带来的攻击面。对一个刚入门 Solidity 的我来说，这三个小关更像是一个“安全视角”的语法练习：每一行代码不仅是“怎么写”，更是“写错会怎样被人利用”。这也让我对后面更复杂的 DeFi 合约和 Web3 实习计划有了点底气——至少，现在再看到一份合约，不会完全不知道从哪里下手看起了。

![截屏2026-01-20 17.37.50.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yanzhuchen96-creator/images/2026-01-20-1768903496433-__2026-01-20_17.37.50.png)

最后感慨一下，很感谢 OpenZeppelin 能设计出这么好的“游戏式实战编程”，相比看视频和看书简直”无痛“太多了，开发者也很会给情绪价值，每通过一个关卡都有“牛逼”霸屏。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->








# 1.19 学习笔记

## ZK Vote 学习笔记

我之前对区块链投票的理解停留在"透明公开"这个维度，但通过 My First ZKVote 的体验，我深刻理解到了一个核心矛盾：

**区块链能提供的「不可篡改、端到端可验证」与用户需要的「投票隐私」这两个目标似乎无法同时实现。**

接着我接触到了「零知识证明」——证明一个事实而不泄露该事实的内容。

### **普通链上投票 VS ZK Vote**

| 维度 | 普通区块链投票 | ZK Vote |
| --- | --- | --- |
| 投票过程 | 直接调用 vote(proposalId, option) 函数 | 本地生成ZK证明后再提交 |
| 链上可见信息 | 钱包地址、投票时间、选择的选项 | 地址、nullifierHash、proof |
| 身份隐私 | 投票选择与钱包地址永久关联 | 投票选择与身份解耦 |
| 投票内容隐私 | 选项在Input Data中公开可见 | 选项被 voteCommitment 加密保护 |
| 可验证性 | 能验证：谁投了什么，什么时候投的 | 能验证：投票是否合法、是否防重复，但无法反推内容 |
| 防重复投票 | 用钱包地址直接检查 | 用nullifier（密码学标记）间接检查 |
| 关联分析风险 | 高风险：地址可被关联到真实身份，投票偏好随之暴露 | 低风险：即使知道钱包地址，也看不出投票内容 |
| 计算地点 | 链上（所有验证者执行） | 本地浏览器（证明生成）+ 链上（证明验证） |
| 适用场景 | 社区治理、不涉及隐私的投票 | 政治投票、薪酬评审、敏感话题投票 |

### **零知识证明的三个核心性质**

在学习过程中，我反复遇到这三个概念，它们是ZK系统的数学基础：

| 性质 | 含义 | 在投票中的体现 |
| --- | --- | --- |
| 完备性（Completeness） | 诚实的证明者总能说服验证者 | 我有投票权，证明就一定有效 |
| 可靠性（Soundness） | 欺骗者无法伪造证明 | 没有投票权的人无法通过 |
| 零知识性（Zero-Knowledge） | 证明不泄露秘密本身 | 看得到我投票了，看不出我投什么 |

**这三个性质缺一不可**，它们定义了什么是真正的零知识证明。

**区块浏览器能看到我的钱包地址 ≠ ZK投票无效**！因为：

-   交易必须由某个地址发出（这是区块链的基本特性）
    
-   但ZK通过 nullifierHash 和 voteCommitment 隐藏了投票内容
    
-   所以旁观者知道"这个地址投票了"，但不知道"投了什么"
    

### **防重复投票机制：Nullifier**

```
nullifier = Hash(identitySecret + electionId)
```

**为什么不直接用钱包地址防重复？**

-   如果用钱包地址：链上会显示这个钱包投过票，任何人都能看到「这个钱包投了票」的完整投票历史，隐私破坏
    
-   用nullifier：链上显示一个哈希值，无法反推身份，但能防重复
    

**这体现了ZK设计的巧妙：用密码学单向性实现隐私。**

### **Merkle树的用途**

Merkle树的作用是**组织并证明身份归属**：

所有 identityCommitment 组成一棵树，我持有我节点到树根的路径（Merkle Proof），我用ZK证明：“我知道一条路径能连到这个根”，但不暴露我在树的哪个位置。

**等价于现实中的场景**：我能证明"我在某个白名单里"，但不告诉别人白名单的具体内容。

## **完整投票流程**

1.  在浏览器本地生成 identitySecret（不上链）
    
2.  计算 identitySecret 并加入 Merkle 树
    
3.  投票时，浏览器用 Proving Key 在本地生成 proof
    
4.  提交 proof + publicInputs 给合约
    
5.  合约用 Verifier 验证 proof 有效性
    
6.  检查 nullifier 未被使用，投票成功
    

**ZK并不是完全隐藏，而是隐藏关键部分，保留可验证性。**

## identitySecret 和 identitySecret 分别是什么？

identitySecret 是一个由用户在**本地浏览器**随机生成的秘密数值，用于推导所有与身份相关的密码学参数（如 identityCommitment、nullifierHash 等），永远不会被上链或发送到任何服务器。

identityCommitment 是对 identitySecret 进行哈希计算得到的承诺值，即 identityCommitment = Hash(identitySecret)，是公开的并上链**存储在 Merkle 树**中，用于在不暴露身份秘密的情况下证明用户拥有投票权。

## 主要收获

**传统链上投票解决了什么问题，又带来了哪些隐私风险？**

传统链上投票通过区块链的不可篡改性和公开透明性解决了传统投票系统中的中心化信任问题，让投票过程端到端可验证——任何人都能查询完整的投票记录和计票过程，无需依赖单个机构；但这种完全的透明性带来了严重的隐私风险：钱包地址、投票时间和投票内容都被永久记录在区块链上，任何人都可以看到该地址投了什么，进而通过关联分析将投票偏好映射到真实身份，导致政治立场、宗教信仰等敏感信息被暴露，面临言论审查、职业歧视或人身安全威胁的风险。

**零知识证明在投票系统中分别承担哪些责任：**

-   身份隐私；
    
-   有资格性验证；
    
-   防止一人多投；
    

**在工程实践中，部署 ZK 投票系统需要注意的关键假设与安全边界**

在工程实践中部署 ZK 投票系统需要注意的关键假设与安全边界包括：密码学层面要确保哈希函数的单向性和 Setup 阶段秘密参数的安全销毁（否则攻击者可伪造证明），系统层面要保证 identitySecret 用户妥善保管、Merkle 树根不被篡改、合约代码无漏洞以及 nullifier 去重机制正常工作，同时要明确安全边界——ZK 投票无法隐藏交易发起者（from 地址）、无法完全隐藏投票集合规模、无法防止链上统计分析，但正是这些权衡保证了在可验证性和隐私保护之间的平衡；因此部署前必须进行严格的形式化验证、第三方审计、长期测试网测试以及用户教育，任何一个环节的疏漏都可能导致整个隐私保护系统的失效。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->












## **分享会 - Key Hash Based Tokens: 从 ERC-721 到 ERC-7962 AI提炼总结**

本次分享围绕一个从 ERC-721 演进出来的新协议 **ERC-7962** 展开，目的是在保持数字藏品（NFT）属性的同时，引入更强的隐私保护和更好的用户体验。讲者首先回顾了传统 NFT 的特点：基于 ERC-721 标准，每个 token 的 owner 是一个公开可查的地址，谁持有什么资产、做过哪些交易都可以在链上被分析。这样带来了两个问题，一是隐私缺失，容易被构建“资产图谱”；二是对普通 Web2 用户不友好，需要自己装钱包、管私钥、付 gas 费，这阻碍了 Web2 用户向 Web3 迁移。

在现实业务场景中，典型需求是“带隐私的会员身份与权益验证”。以星巴克会员和机场贵宾厅为例：机场需要验证某人是不是星巴克会员，以便提供休息室服务，但星巴克不能把全部会员名单或用户地址直接给机场，否则构成隐私泄露甚至违法；机场也不想长期保存大量用户隐私数据，只在验证当下需要“知道此人有资格”。这类场景要求一种机制：在不暴露完整会员列表或用户地址的条件下，向第三方证明“某人满足某种条件（是会员）”。

在这个背景下，ERC-7962 引入了两个核心设计：第一，将所有权从“地址”变为“公钥哈希（kHash）”。在合约中 ownerOf(tokenId) 不再是某个链上地址，而是某个公钥的哈希值，使得外界无法直接把 token 持有人和链上地址图谱对应起来。第二，将“资产所有者”和“交易发起者”彻底解耦，任何地址都可以作为 relayer 发起交易并付 gas，只要交易里包含了由真正所有者签名的指令，合约就可以验证该签名与 kHash 是否匹配。这让商家、平台可以替用户“代付 gas、代为发交易”，而用户只需要做签名动作表达自己的意图。

零知识证明（ZK）的思想在这里贯穿始终，但实现层面并没有直接使用复杂的 ZK-SNARK 电路，而是采用“承诺 + 揭示 + 哈希验证”这种较简单的方式。直观理解是：用户先提交一个哈希承诺，真正需要证明时再公开原文，用数学上不可逆的哈希关系来证明“这是同一个值”，而中间不暴露其他信息。这类方式特别适合“是/否”或“等于/不等于”的判断，比如“某人是否属于某个会员集合”。讲者也对比了更完整的 ZK 代数体系（可以证明加减乘除等复杂关系）以及 StarkNet 等为 ZK 优化的底层链，说明在不用重构底层链的前提下，也可以利用较轻量的 ZK 思路实现针对隐私场景的协议。

为了进一步增强隐私，协议在余额模型上采用类似比特币 UTXO 的风格：每个 kHash 只使用一次，一旦发生转账，原 kHash 就视为“花掉”，对剩余余额生成一个新的 kHash。这样即使外部观察者看到某些 kHash 的变动，也很难把多次交易关联到同一主体身上，因为新旧 kHash 之间在链上没有直接关联。与此同时，协议刻意取消了 ERC-20/ERC-721 中常见的 approve 机制，因为 approve 会让某个 key 在链上长期暴露，复用性高、隐私差，不符合“一次性、难关联”的设计目标。

在用户体验（UX）和账户抽象方面，ERC-7962 的目标是让 Web2 用户“看起来只是在用普通互联网服务”，而底层逻辑实际在区块链上执行。对于普通用户来说：不需要理解 gas、不需要持有链上主币、不需要操作复杂的钱包；对于商家来说，可以在后端统一部署合约、统一发起交易、统一承担 gas 成本，把链下业务逻辑（比如会员体系、积分、权益）映射到链上不可篡改的状态上。讲者提到，自己已经在某商业项目中用一个偏中心化的实现帮助商家完成这一点：由商家部署合约并作为唯一发起地址，用户只是通过 App/网页确认操作，整个过程对用户而言与传统 Web2 相似。

安全设计上，协议通过多层机制防止参数篡改和重放攻击。首先，用户签名会覆盖所有关键参数（例如转出金额、接收方 kHash、deadline 等），relayer 无法在不破坏签名的前提下修改这些参数，从而保证交易内容不能被中途篡改。其次，引入 nonce、deadline 等字段，确保同一签名无法被重复利用，过期签名自动失效，避免 relayer 或其他人恶意延后或多次提交交易。协议还考虑了签名可塑性问题，要求对签名格式做严格校验。讲者也承认，为了引入隐私与解耦，协议的 gas 成本必然高于原生 ERC-20/ERC-721，实现复杂度、密钥管理难度也更高，需要开发者和应用在工程上做取舍与封装。

总体而言，ERC-7962 针对的是三类核心场景：隐私型资产/会员身份（用户不希望资产/身份与公开地址紧密绑定）、Web3 新手友好钱包/入口（降低钱包和 gas 的学习成本）、企业级批量分发与空投（商家代付 gas、大规模操作用户资产/权益）。讲者强调，这一协议并不是唯一方案，而是一种在现有以太坊环境下权衡隐私、可用性、去中心化程度的实践结果。论坛上已有不少开发者提出 alternative 设计和 challenge，团队也通过讨论不断迭代规范和实现细节。

## Q & A

**区块链强调透明，零知识证明强调隐私，这两者会不会矛盾？会不会助长内幕交易和洗钱，让散户更恐慌？**

区块链的“透明”指交易和状态公开可验证，而零知识证明提供的是“在不暴露具体细节的前提下证明某种性质成立”的能力，两者并不矛盾。ZK 不会自动导致内幕交易或洗钱，它只是让某些性质可被验证而细节不泄露，真正的合规和监管仍需要在制度和应用层解决。

**现在已有很多零知识相关方案（如 zk-SNARK、StarkNet 等），你们的做法与这些有什么关系和区别？**

从数学上看，ZK 有多种实现路径：有的只解决“等于”之类简单关系，有的能支持完整代数体系（加减乘除等），而 ERC-7962 使用的是相对简单的哈希承诺式方式，用来解决“是否属于某集合”这类等式问题。从工程角度看，StarkNet 之类是专门为 ZK 优化的底层链，可以降低证明验证成本，而 7962 是在现有以太坊环境中，通过合约和协议设计在不改底层的前提下引入隐私特性。

**你提到用户可以“免 gas”，那 gas 费到底是谁付？总得有人承担算力和能源成本吧？**

任何上链交易必然有人付 gas，这点不会改变。所谓“免 gas”是对终端用户而言，他们不需要亲自付 gas；在星巴克与机场的例子里，可以由星巴克、机场或其他业务方承担这部分费用，因为他们因获得新的商业场景和用户价值能回收成本。协议设计只是让“第三方代付 gas”在技术上安全可行，而不是消灭 gas 成本。

**Web2 用户不用钱包、不管私钥，那他们究竟“持有”的是什么？是怎么像 QQ 账号那样使用的？**

从密码学角度看，用户真正“持有”的仍然是某个公钥/私钥对，只不过协议把链上的所有权表示成公钥哈希（kHash）。在实际产品中，这个密钥可以被封装为“密码 + 设备 + 生物信息”等形式，由应用（比如星巴克 App）帮助用户管理。对用户来说，就像记住 QQ 号和密码一样使用服务，而底层则通过公钥、kHash 和签名来证明“这个用户确实是对应的 owner”。

**既然要隐私，为什么不直接每次换一个地址？一次性地址不能解决问题吗？**

简单地“每次换地址”并不能完全解决隐私问题，因为只要某个地址在合约的 `ownerOf(tokenId)` 中出现，就会长期与该 token 绑定，很容易被索引和关联；并且同一个地址在不同场景的多次使用也会被图谱分析关联。ERC-7962 使用的是一次性 kHash + UTXO 式余额拆分：合约只存哈希，地址和公钥只在需要时短暂出现，且交易完后立刻轮换，使得从链上原始数据构建稳定的身份图谱变得极其困难，安全等级远高于“只换地址”。

**由星巴克之类的中心实体统一发起合约调用并代付 gas，会不会增加女巫攻击或中心化作恶的风险？**

在“商家统一发起交易”的偏中心化方案中，确实需要部分信任商家不会滥用权限，这属于在去中心化与可用性之间做的现实权衡。若采用完全去中心化的 relayer 模式，则通过“签名绑定参数、防重放、防签名可塑性”等安全措施，限制 relayer 能做的事情——他们无法篡改交易内容，只能按照用户签名的指令原样提交，最多选择是否/何时提交，从协议层降低作恶空间。

**7962 在实际项目中的应用状态如何？有什么推荐的学习资料？**

讲者已经在某商业项目中以简化版实现了该协议的核心思路，主要用于隐私会员身份与商家代付 gas 的场景。想进一步学习，可以参考该协议的 GitHub 代码和在相关论坛（如 Magic 论坛）上的讨论贴，那里有不少开发者提出 alternative 设计与质疑，讲者对很多问题做了详细回复，有助于理解设计权衡与实现细节。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->













# 1.17 学习笔记

今天我跟着教程 [Challenge #0 - Tokenization](https://speedrunethereum.com/challenge/tokenization) （[**中文版链接**](https://www.notion.so/Challenge-0-2afbbd63be8780dbbd56cb01490da15e?pvs=21)）按照步骤学习和操作，完成了人生中的第一个 Web3 小项目，搭建并部署了一个 NFT dApp。

先是在 AI 的帮助下搭建好了环境 （因为教程主要面向有基础的人群，所以没对这一部分有过多描述），然后按照文档启动了三个终端窗口：第一个跑 yarn chain 启动本地区块链，第二个跑 yarn deploy 部署合约，第三个跑 yarn start 启动前端。接着打开浏览器访问 localhost:3000，mint 了一些 NFT，还挺可爱的。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yanzhuchen96-creator/images/2026-01-17-1768648171018-image.png)

在 Debug Contracts 页面，我第一次直接调用了合约函数 ownerOf(tokenId)，输入 NFT 的编号（比如 1、2），就能查到它归哪个钱包地址所有。同时在 AI 的帮助下知道了那些卡片上显示的数字 42、38、15 是 NFT 的属性分数，而不是 tokenId，解决了我一开始的认知混淆问题。Anyway，这让我真实感受到了"链上所有权"是怎么回事，每个 NFT 的主人都精确记录在区块链上，任何人都能查，没法篡改。

## 部署合约

本地跑通后，接下来要部署到 Sepolia 公共测试网。按照教程，我先用 yarn generate 创建了一个专门用于部署的钱包，设置了密码加密私钥。这个账户和我日常用的 MetaMask 分开，就算私钥泄露也不会影响主钱包，安全隔离做得挺好。这里我又搞混了一个概念，以为部署合约用的账户就是合约账户，后来才知道部署账户和我的 Metamask 一样都属于 EOA，用两个 EOA 的目的就是进行安全隔离，降低私钥泄露的风险，部署账户只负责部署合约，只存少量测试币，即使泄露损失也不大。

接着用 yarn account 查看部署账户的地址和二维码，然后从我的 MetaMask 转了一些 Sepolia 测试网 ETH 过去。测试网的币是免费领的，不值钱，但部署合约需要支付 Gas 费，所以这一步必不可少。

充值完成后，我运行 yarn deploy --network sepolia，输入密码，合约就开始部署了。终端显示了部署交易的哈希值和合约地址 0x9f66652be09f29E52fD7E1F15096A7e932684Bd9。我把这个地址粘贴到 Sepolia Etherscan 里搜索，真的能看到合约已经上链了，虽然还没有交易记录，但标记为 “Contract”，说明部署成功了，有点小激动。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yanzhuchen96-creator/images/2026-01-17-1768649239059-image.png)

## 用 Vercel 部署前端

本地的前端只有我自己能访问，要让别人看到就得部署到公网。跟着教程走，我先在终端上运行 yarn vercel:login，浏览器自动打开让我用 GitHub 账号登录。登录成功后回到终端，运行 yarn vercel 开始部署。

中间一直回车，确认一些基本的默认设置，一路顺利。等了一两分钟 build，最后给了我两个链接：一个是 Vercel 的管理后台（Inspect），一个是公开访问网址（Production）。我点开 Production 链接 [https://nextjs-3bwzotaht-cydots-projects.vercel.app](https://nextjs-3bwzotaht-cydots-projects.vercel.app)，看到了我的 NFT 网站，虽然页面显示没找到 NFT（因为我还没在测试网上铸造），但网站本身已经活了。

最后就是运行 yarn test 进行一些自动化测试功能，不太明白具体是在干嘛，不过这不是今天的重点。

![屏幕截图 2026-01-17 184135.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yanzhuchen96-creator/images/2026-01-17-1768659148889-_____2026-01-17_184135.png)

然后我在这个 dApp 上连接了自己的 Metamask，mint 了一个水牛 NFT，最后成功在 Metamask 里显示。

![屏幕截图 2026-01-17 185238.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/yanzhuchen96-creator/images/2026-01-17-1768649268208-_____2026-01-17_185238.png)

## 一些感想

首先就是这次的实践终于让我对 Web3 开发栈有了更清晰的初步认识，以前只知道 Next.js, Solidity, blahblah… 各自是什么东西，但是不知道它们具体如何一起运转，支撑起一个项目。一开始觉得很复杂，但实际跟着流程走下来，发现 Scaffold-ETH 把很多底层细节都封装好了，只要理解核心概念（合约是什么、前端怎么调用、部署到哪里），操作上并不难。

最大的收获是真正理解了"去中心化"的含义。我的智能合约部署在公共区块链上，任何人都能调用，我自己也没法关闭或修改。前端部署在 Vercel 上，全世界都能访问。NFT 的所有权记录在链上，不依赖任何公司的服务器。这种感觉和以前写传统 Web 应用完全不同，我发布的不只是一个网站，而是一个真正意义上的去中心化应用。

当然，今天只是跑通了流程，很多细节还不懂。比如 Solidity 合约代码我还没仔细看，Gas 费的优化、安全漏洞的防范这些更是没涉及。但至少我已经迈出了第一步，知道了整个流程是怎么跑的，也有了继续学下去的信心。

接下来打算花点时间看看 YourCollectible.sol 的代码，理解一下 NFT 合约的核心逻辑。然后可以试着改改属性，或者加个新功能玩玩。SpeedRunEthereum 后面还有很多挑战，每个都会教一个新概念，慢慢学下去应该能对 Web3 开发有更系统的理解。

## 最终成果

前端：[**https://nextjs-3bwzotaht-cydots-projects.vercel.app**](https://nextjs-3bwzotaht-cydots-projects.vercel.app)  
合约：[**https://sepolia.etherscan.io/address/0x9f66652be09f29E52fD7E1F15096A7e932684Bd9**](https://sepolia.etherscan.io/address/0x9f66652be09f29E52fD7E1F15096A7e932684Bd9)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->

















## 学习 Solidity 推荐路线

1.  读官方文档 + 做例子：
    

-   从 Solidity 官方文档的 “Introduction to Smart Contracts” 和 “Solidity by Example” 开始。
    
-   用 Remix 这种网页 IDE 写第一个合约（HelloWorld，存／读变量， 转账，事件触发）。
    

2.  做交互式教程：
    

-   用 CryptoZombies、Solidity by Example 的小项目练手，感受 storage / memory、gas、事件、modifier 等细节。
    

3.  学习工具／测试／部署：
    

-   在本地装 Hardhat（适合脚本部署、多网络管理）或 Foundry（适合写大量单测、fuzzing、主网 fork 研究协议）
    
-   学会写单元测试（require 失败、revert、边界条件），**这一步对以后搞 DeFi / NFT 协议非常关键**。
    

4.  安全／最佳实践：
    

-   学习常见漏洞：重入、整数溢出／下溢、访问控制错误、price oracle 操纵、flash-loan 攻击等。
    
-   用 Slither、Mythril 之类的工具做静态分析，配合 Foundry/Hardhat 测试，把显而易见的坑先扫一遍。
    

5.  做一个小项目：
    

-   写一个简单 ERC-20 代币、投票合约，或者小型 DeFi 玩具协议，上 testnet 或某条 L2（如 Sepolia / Arbitrum Sepolia / Base 测试网）部署一遍，完整走完：编译 → 部署 → 前端调用 → 调试的闭环。
    

## Solidity 的整数溢出漏洞是什么？

Solidity 的整数是有上限和下限的，比如 uint8 只能在 0～255 之间摇摆。​老版本 (<0.8.0) 里，如果在 255 上再加 1，就不会报错，而是从 0 重新开始；同理，从 0 再减 1 会绕一圈成 255，这就是 overflow / underflow。

用水杯来打个比方，想象有个固定容量的 255 ml 杯子，你已经倒满 255 ml 水，再多倒 1 ml，本来应该溢出来，但 Solidity 老版本不让用户看到溢出，杯子自动清空成 0 ml，相当于回到起点。​

反过来，如果杯子已经是 0 ml，再倒出1 ml，现实世界是不可能的，但合约会从未来偷水，直接变成 255 ml，这就是 underflow，攻击者就能用这种方式把余额之类的数字变得巨大会很危险。

很多合约会写类似先减掉余额，再检查，在老版本下，balance - amount 如果 underflow 变成一个巨大数字，后面的检查可能就被绕过去，导致别人能多提钱、多铸币等。​攻击者只要精心选择 amount，让运算结果刚好溢出/下溢，就能骗过逻辑，从而偷走合约里的资产，这是历史上真实发生过、亏很多钱的一类问题。

## 智能合约常见组件

| 组件名称 | 定义与作用 | 特点 / 注意事项 | 代码示例 |
| --- | --- | --- | --- |
| 状态变量(State Variables) | 存储在区块链上的数据，构成了合约的永久状态​ | - 持久性：值永久存储在链上，不会在函数调用结束后消失- 可见性：支持 public（自动生成 getter）、internal（合约内部和继承合约可访问）、private（仅当前合约内可访问）- 在 0.8.x 之后常用 constant / immutable 修饰，节省 gas​ | uint256 public totalSupply;address private owner; |
| 函数(Functions) | 智能合约中执行操作的代码块，可修改状态、执行计算并返回结果​ | 可见性：- public：可从合约内部、继承合约或外部调用- external：只能从外部调用（通常用于对外接口）- internal：合约内部或继承合约中调用- private：仅当前合约内部调用状态可变性：- pure：不读取也不修改合约状态- view：读取但不修改状态- payable：可以接收以太币- 默认（无修饰符）：可以修改状态​ | function transfer(address to, uint amount) public { ... } |
| 特殊函数receive() / fallback() | 处理合约收款和未匹配调用的特殊函数​ | - receive()：专门处理「只带 ETH、不带 calldata」的转账- fallback()：处理「调用不存在的函数」或合约没有 receive() 时的 ETH 转账​ | receive() external payable { }fallback() external { } |
| 事件(Events) | 智能合约向区块链日志中「广播」消息的方式​ | - 日志记录：数据存储在区块链的日志中，而不是合约的状态存储中，成本更低- 可监听性：外部应用可以「监听」特定事件，实时更新 DApp 界面或触发相应操作- 可使用 indexed 关键字（最多 3 个）方便前端筛选​ | event Transfer(address indexed from, address indexed to, uint256 value); |
| 修饰符(Modifiers) | 给函数加「前置条件」的可复用代码块，常用于权限控制​ | - 代码复用：避免在多个函数中重复相同的检查逻辑- 安全性：有助于强制执行安全策略（如 onlyOwner）​ | modifier onlyOwner { require(msg.sender == owner); _; } |
| 构造函数(Constructor) | 只在合约部署时执行一次的初始化函数​ | - 一次性执行：部署后不能再次调用- 可接受参数在部署时传入- 现在推荐用 constructor 关键字​ | constructor(address _owner) { owner = _owner; } |
| 结构体(Structs) | 自定义复合数据结构，将多项信息打包在一起​ | - 可用作状态变量、函数参数和返回值- 帮助构建更清晰的业务模型​ | struct Order { address seller; uint256 price; } |
| 枚举(Enums) | 定义有限集合的枚举值，常用于状态机​ | - 适合标记订单/流程状态- 类型安全，可读性强​ | enum Status { Pending, Active, Cancelled } |
| 自定义错误(Custom Errors) | 自 Solidity 0.8.4 起可定义的错误类型，与 revert 配合使用​ | - 比传统 require(..., "string") 更省 gas- 可携带结构化参数，方便调试​ | error InsufficientBalance(uint requested, uint available); |
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->




















# 1.15 学习笔记

今天在学校上了一天学，没有进行阅读，不过听了“AI及其基础概念”的分享会，以下是整理的笔记。

这次分享的主线就是从聊天机器人到“打工人”AI的演变。AI 正在从“高级复读机的聊天机器人”进化成“能自己干活、自己收钱的打工人”，而要让这种 agentic AI 真正落地，必须解决三个基础设施问题：**有记忆、有工具、有身份和钱包**。Web3 提供了 AI 身份、声誉和原生微支付的底层协议，SpoonOS 则把这些协议封装成开发框架，让普通开发者几天内就能搭出有 Web3 能力的 AI 智能体。

一个完整的 AI 智能体由三部分组成：**LLM** 是“脑子”，负责理解意图和制定计划；**私有知识库与向量数据库**是“记忆”，通过数据采集、embedding 与相似度检索让 AI 从金鱼记忆升级为大象记忆；**工具调用与 MCP** 是“手脚”，通过函数调用执行发邮件、查库、跑代码、调第三方 API 等操作，形成“思考-行动-观察”的闭环，让 AI 从回答问题进化为长期运行的智能体。

在 Web2 的架构下，AI 的局限在于：

-   服务端无法分辨请求是人还是 AI、是哪个公司或哪个 agent，导致身份欺诈风险高，AI agent 实际上是“能力很强的黑户”。
    
-   绝大多数 AI 日志存本地服务器，可被修改、删除或伪造，监管难以相信现有日志，责任追溯存在巨大漏洞。
    
-   随着 AI 大量调用 API、购买数据、支付算力，传统信用卡和订阅制在 KYC、微支付颗粒度、结算速度等方面都无法支撑“每秒成千上万笔 0.001 美元级交易”的机器经济需求。
    

Web3 能给 AI 带来两样关键东西：一张**不可伪造的链上身份证**和一个**可编程的钱包账户**。 区块链的去中心化、不可篡改和可编程特性，恰好匹配 AI agent 在身份验证、行为记录和支付结算上的需求。Rick 把他们的架构拆成三层：底层是交易层，用的是 X402 协议，把支付能力直接嵌入 HTTP；中间是身份层，用的是 ERC8004 标准，为 AI 提供唯一链上身份和声誉系统；顶层是应用层，即 SpoonOS，封装前两层能力为简单 API，让开发者不必自己管理私钥和写签名逻辑。整体目标是让 AI agent 像“合法公民”一样在链上参与经济活动，而不是继续做“黑户打工人”。

X402 以 HTTP 402 为基础，把支付能力直接嵌入 HTTP 流程：服务端用 402 响应返回收款地址、金额、代币和链信息，客户端用钱包完成链上交易或签署付款承诺，再携带支付证明获取真实服务。支付证明可通过链上查交易或验证签名承诺来确认，同时结合支付通道和批量结算降低 gas，使按 token 计费、按次付费的 API 成为现实，把互联网从广告/订阅扩展到面向 agent-agent 的精细按需付费模式。​

ERC8004 基于 ERC721，为每个 AI agent 铸造唯一 NFT 身份，元数据中记录 agent 类型、能力声明、运行模型哈希和 owner 地址，便于验证和权限编程。配套的声誉系统用 0–100 分为单维评分，通过“只有与 agent 有真实链上交易的钱包才可评价”“评价需付 gas”“评分上链不可篡改+时间衰减权重”等机制，增加刷分成本并兼顾改过机会，同时预留 TEE 与 ZKML 接口用于证明模型在可信环境中执行而不泄露内部细节。

在此基础上，SpoonOS 的角色是“AI × Web3 的操作系统/框架”，目标是让普通开发者不懂 solidity、不自己管理私钥，也能快速做出可上链、有钱包、有声誉、有支付能力的智能体。四层架构包括：输入层（采集链上数据、文档并做 RAG）、核心层（统一 LLM 接口、多智能体编排引擎、记忆管理器和工具调用器）、协议层（集成 ERC8004 和 X402、钱包托管和链上证明）、输出层（DeFi 连接器和跨链桥），并通过“私钥永不离开安全环境”的设计降低密钥泄露风险。

基于 SpoonOS，可以快速开发 DeFi 交易/套利智能体、数据市场 agent（按量卖数据/API）、代码协作助手（代处理 issue/PR）以及未来的“智能体网络”和“AI 网络游戏”等复杂生态。实践上，量化策略的上限主要取决于人本身的策略水平，AI 提供的是自动化执行和编排能力；建议先用小额资金验证 AI 是否按预期策略行动并能稳定盈利，再逐步放大规模，以降低策略错误或实现偏差带来的损失。

## Q&A

### AI 量化是否一定要单独链上账户？

小规模自用时，可以直接托管自己钱包即可；但如果要做成公开可募资的量化“基金型机器人”，则需要链上身份和可验证记录来建立信任，否则别人无法确认策略真实性、也无法追责。

### Web3 的 TPS 是否真比 Web2 高？

相比跨境电汇等传统体系（手续繁琐且受合规和架构瓶颈限制），多链高 TPS 区块链更适合机器经济那种高频、跨区域、24 小时运行的微支付结算，只是这个比较对象更偏向传统金融基础设施，而不是单一中心化支付巨头。

### 其他讨论

声誉系统目前是针对“agent 而不是开发者”的单一维度评分，所以确实存在开发者“废号重开”的问题；协议还比较早期，未来会探索更复杂的多维度声誉和防女巫机制。对于 DeFi 场景下的策略 agent，核心天花板仍然在人的策略质量上，AI 主要负责自动化执行和编排：如果人本身不懂交易，只想“让 AI 帮忙写一个稳赚策略”，大概率会亏钱，建议先用少量资金小规模实测，再逐步放大。

## 总结

大模型升级为 agentic AI 后，开始“替人干活、替人花钱”，传统 Web2 与金融基础设施在身份、审计和微支付上都无法支撑即将到来的“机器经济”；Web3 通过 ERC8004 和 X402 等协议，为 AI agent 提供链上护照和原生微支付能力，再配合像 SpoonOS 这样的框架，把复杂的链上操作（身份、声誉、钱包、支付、审计）封装起来，从而构建一个“AI 作为经济主体”的新基础设施，让未来的经济活动由数以亿计的 AI agent 在链上以可验证方式持续运转。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->






















# 1.14 学习笔记

# 021学习以太坊第三章重点

## EOA VS 合约账户（类比银行卡 VS 自动售货机）

| 维度 | EOA | 合约账户 |
| --- | --- | --- |
| 控制主体 | 私钥持有者（人 / 程序）​ | 合约代码逻辑本身​ |
| 是否有私钥 | 有（私钥 → 公钥 → 地址）​ | 无，不能签名​ |
| 是否能签名 | 能，对交易签名​ | 不能，所有调用都由外部交易触发​ |
| 发起交易 | 可以主动发起交易​ | 不能主动发起，只能被消息调用（internal call）​ |
| 创建成本与方式 | 生成 EOA 只是本地生成一对 key pair，不需要上链、不花 gas。 | 部署合约账户必须发一笔 “创建合约交易”，把 bytecode 写入链上，需要支付不少 gas。 |
| 状态 | 只有余额和随机 nonce​ | 余额 + 代码 + 存储（状态树）​ |

**可以通过 RPC 方法 eth\_getCode(address) 判断某地址是否为合约账户：如果返回的代码非空，就说明有智能合约存在，是合约账户；否则为 EOA。**

## CREATE VS CREATE2

| 维度 | CREATE | CREATE2 |
| --- | --- | --- |
| 地址是否可预测 | 依赖部署者 nonce，部署前不好精确算地址（要模拟或推演 nonce）。 | 由 部署者地址 + salt + init_code 决定，部署前就能精确算出地址。 |
| 跟什么有关 | 跟“当前第几次部署/发交易”的 nonce 强绑定，顺序变地址就变。 | 跟 sender、salt、init_code 强绑定，和 nonce、部署顺序无关。 |
| 典型用途 | 普通合约部署，一般随手 new 一个合约就用它。 | 需要“先知道地址再部署”的场景：合约工厂、预先授权、钱包预创建等。 |
| 是否能“先占坑后部署” | 基本不行，地址前期不好引用。 | 可以先算出未来地址，提前写进别的合约或权限里，再真正部署。 |
| 地址复用可能性 | 通常视为 “一次性地址”，销毁后也很少复用同址。 | 在旧合约被销毁后，可用同一 sender+salt+init_code 再部署回同一个地址（要非常谨慎）。 |

## 0x是什么？为什么要用0x？必须写吗？

0x相当于 “货币符号”，用来告诉人和程序：后面这一串是十六进制数字。用0x的好处是，一眼能看出是**地址 / hex**，而不是什么别的东西，避免和其它内容混淆，这使得在文档或者代码里查找都更加方便。从 “数学意义” 上看，0x并不算地址的一部分，所以写不写都行，都能识别，但出于能 **“让人和程序一眼识别”** 的目的，推荐带上0x。

## 账户抽象的通俗解释

用一套机制（如 ERC-4337，EIP-7702），让**合约钱包**可以像普通账户那样发起操作，还能自己写规则（怎么验证、怎么付费、怎么玩安全），从而大幅改造 “账户能力”，而不只是“EOA 私钥签交易”这一种模式。

真正 “发交易”的还是 EOA，合约自己不能直接把交易丢进区块链，只能在交易内部继续调用别的合约。账户抽象做的事是搭一层 “中间系统”，对用户来说就像 “这个合约账户在主动发交易、主动操作”。​

EIP-7702 把“EOA 能不能执行合约逻辑”这件事，放进了协议层，用一 种可授权、可撤销、带 nonce 防重放的新交易类型来做；钱包（包括 MetaMask）可以在此基础上继续构建更上层的智能账户体验。

## 在codehash中，为什么要存哈希而不是直接把code塞进账户对象？

在账户里放一整段合约代码很占空间，状态树会变得又大又难同步。代码实际单独存储，账户里只存 codeHash 作为 “指针 + 防篡改承诺”。通过 eth\_getCode(address) 可以根据 codeHash 拿回真实字节码。

## EOA 与合约账户的互相调用机制

外部交易由 EOA 发起，合约间调用被称为 internal transactions / message calls，由 EVM 执行引擎在同一条交易上下文中完成。

## 代币与合约账户的关系

代币储存在合约账户的storage中，转账时，EOA向合约账户发起一笔交易，调用转账函数，合约代码在执行过程中更新双方的余额等存储项，这个过程在EVM里执行。**EOA 地址本身并不存放 ERC-20 / ERC-721 代币对象**，它只在代币合约的映射里作为一行 key 出现。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


































# 1.13 学习笔记

## **节点和客户端的关系以及客户端间的协同配合**

客户端是软件实现，节点是跑着软件（客户端）的“电脑”。有验证者在的节点就是一个“验证节点”。

-   执行客户端就像法院里的书记员 + 案卷系统，负责接收材料、执行具体流程、更新“案卷状态”。**EVM + 状态 + RPC**，相当于“世界计算机”的 CPU + 内存 + 本地数据库 + RPC 网关。
    
-   共识客户端就像法官，根据书记员提供的案卷（执行结果），做出“这是不是合法区块、最终选哪条链”的裁决。**PoS + Beacon + Finality**
    

**上述两项通过Engine API连接**

-   以太坊节点 = 一台电脑 + 两个必备客户端 + （可选）一个验证者客户端
    

**对于开发者来说需要记住的内容：**

调用 eth\_\* / 部署合约 / 查状态找的是 执行客户端（EVM + JSON-RPC）；质押 32 ETH 做验证者 / 关心 attestation、finality、slashing是共识客户端 + 验证者客户端的工作。

**为什么要拆两块？**

为了模块化、易维护、多客户端实现、独立升级，以及更清晰的安全边界： 执行层可以专心把 EVM 和状态做到极致性能，共识层可以独立演进 PoS、PBS 等协议设计。

-   执行客户端负责“算结果、管状态”，共识客户端负责“选哪条结果被全网认可”，两者通过 Engine API 三大动作 newPayload / forkchoiceUpdated / getPayload 互相配合。
    

## 以太坊节点的“社交流程”

先找到人（UDP + Kademlia） → 再拉上加密专线（TCP + RLPx/devp2p） → 最后靠八卦（gossip）把消息传满全网（Gossip + 请求/响应）

## 全节点、轻节点、归档节点的区别

| 类型 | 存储什么数据 | 资源消耗 | 典型用途 | 信任 / 安全属性 |
| --- | --- | --- | --- | --- |
| 全节点 | 全部区块 + 最近若干块的完整状态（老状态会被修剪） | 中等（数百 GB 级磁盘 + 稳定带宽） | 自托管 RPC 节点、参与网络、做验证者的基础 | 自己验证所有交易和区 块，最信任最抗审查 |
| 归档节点 | 全节点数据 + 从创世到现在的所有历史状态快照 | 极高（TB 级磁盘） | 区块浏览器、链上分析、历史调试与回测、节点服务商 | 同全节点，但还能随时 回答“任意高度的任意 状态” |
| 轻节点 | 区块头 + 必要的轻客户端数据，不存完整区块和状态 | 很低（适合手机 / 嵌入式） | 轻钱包、浏览器插件、跨链桥验证、 IoT 设备 | 通过状态根 + Merkle 证明验证数据，安全性 接近全节点，但数据依 赖全节点提供 |

**轻节点不参与共识，不能做验证者**

## RPC节点（服务商）通俗解释

就像一个客服中心，它内部连接数据库或后台系统，查出余额／交易 状态后告诉你答案。 你不需要知道客服后台系统内部结构，也不需要自己去部署整个数据库与服务器；只要有客服电话号码（RPC endpoint）和合适的询问方式（RPC 方法名 + 参数），就能得到你想要的信息。

**更好的方式：**

-   作为普通开发者，自己跑**全节点**，可以独立完成所有事情，更加独立隐私，更高的去中心化程度；同时选择一个“不是头部垄断”的客户端，这实际上就是在非常直接地增强以太坊的弹性和去中心化。此外，还能拥有\*\*“治理话语权”\*\*，拥有升级与分叉的选择权。
    
-   作为**需要大量历史查询 / 调试 / 审计的机构**，自己维护一套**归档节点**。
    

## 节点同步

| 模式 | 核心特点 | 优点 | 缺点 | 适合谁 |
| --- | --- | --- | --- | --- |
| Full 同步 | 从创世块开始，按顺序把所有历史交易都执行一遍。 | 数据最完整，安全性最高，可完全独立验证全链历史。 | 同步时间最长，占用磁盘和带宽最大。 | 交易所、基础设施服务商、对安全和完整历史要求极高的节点。​ |
| Snap 同步 | 从比较新的检查点块开始，同步一份“状态快照”，再从那之后正常追块。 | 同步速度很快，是 Geth 默认模式，资源占用比 Full 小很多。 | 最早那一段历史状态不是本地一点点算出来的，而是基于快照信任网络多数节点。 | 想跑“正常全节点”，但又不想等太久、硬盘没那么豪华的开发者和个人玩家。​ |
| Light 同步 | 只保存区块头，需要具体数据时再向全节点“按需查询”。 | 启动飞快，占用磁盘和带宽极小，非常轻量。 | 依赖外部全节点提供数据，不能离线完整验证全网状态。 | 手机钱包、浏览器插件钱包等，只需查自己相关数据的客户端。 |

## 助记词和私钥的关系

助记词是钱包，私钥是钱包里的夹层。助记词可以推出很多私钥，私钥推不出助记词。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->























































## 今日学习目标（周一 1.12）

1.  **读完 Web3 实习手册**[**「入门导读」**](https://web3intern.xyz/zh/blockchain-basic/)**部分，并写出自己的理解**
    

-   用 2–3 句话回答「什么是区块链、它解决了什么问题」。
    

区块链是一个由很多节点共同维护的**去中心化**账本，按时间顺序把交易打包成区块，串成链，并且记录一旦写入就很难被篡改。它主要解决的是在不完全信任对方、没有单一中心的情况下，大家还能对同一份账本达成一致。

-   列出自己觉得最重要的 3 个概念​。
    

Layer 2（对技术岗十分重要）| Gas （所有链上操作都要付费）| EVM（以太坊“核心引擎”）

**Web3 不等于 Web3.0 ！**

2.  **完成 021 学习以太坊第 1 章的第一次通读**
    

-   知道以太坊大概是干什么的、和比特币的核心区别（不需要记细节）。
    

以太坊更像是一台大家共享的“世界计算机”，能够通过执行智能合约来实现很多功能，有更完整的区块链生态；而比特币作为“数字黄金”，主要在金融相关的领域使用（例如储值和投资），在支付以外的应用生态（DeFi、NFT、游戏等）上，相比以太坊要少得多。

3.  **阅读拓展 & 辅助材料（optional)**
    

-   [Day 1: A Developer’s Guide to Building on Ethereum](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro
    
-   [Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)
    
-   [Uniswap V2 学习官方文档](https://docs.uniswap.org/contracts/v2/overview?utm_source=chatgpt.com)
    

4.  **Web3 行业全局介绍分享会重点**
    

-   竞争情况：热门岗位平均一个职位收到450份简历，初级人才过剩，精英极度短缺。70%的面试会考察AI工具使用能力和链上交互记录。
    
-   薪酬梯队：
    

第一梯队：智能合约开发和合规法务，senior月薪可破10万

第二梯队：研究员和核心产品

第三梯队：运营和增长，初级8000-1.5万，但做好增长的奖金和token分成可能超过开发

-   **如何求职：**
    

**进入Web3的三条主要路径**

1.  从Web2迁移：带着能力和资源从传统互联网转型，特别欢迎有海外工作经验、英文好、能独立交付项目的人才
    
2.  生态嵌入：在DAO里接任务、在开源社区贡献代码、写研究报告，积累贡献后项目方会主动联系
    
3.  建立链上声誉：在Twitter/Farcaster持续输出深度内容、参加Space活动，公域流量池是最好的社交简历
    

**对自己来说，短期更适合的路径可能是：1. 在开源社区贡献代码；2. 在Twitter持续输出深度内容。**

**打造链上简历很重要！！！**

**技术岗：**

1.  掌握Solidity（最基础）
    
2.  了解新语言如Rust、Move、Cairo、Funk
    
3.  具备安全意识和系统设计能力
    
4.  理解DeFi机制
    

**当前短期目标：学会Solidity和Rust**
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
