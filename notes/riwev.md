---
timezone: UTC+8
---

# riwev

**GitHub ID:** riwev

**Telegram:** @17709847521

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->
## **Hardhat 跟练**

## **Uniswap V2**

### **Automated Market Maker  自动做市商**

### **Periphery**

外部智能合约很有用，但并非 Uniswap 运行的必要条件。新的外围合约可以随时部署，而无需迁移流动性

### **Position**

由上限和下限刻度定义的流动性实例，以及其中包含的流动性总量。

### **Protocol Fees**

费用奖励给协议本身，而不是流动性提供者。
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->


Challenge #1 : Crowdfunding

Uniswap - V2 : **swapTokensForExactTokens**

```
    // NOTE: swap min input for specified output
    // max in = 3000 DAI
    // out =  1 WETH
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external virtual override ensure(deadline) returns (uint[] memory amounts) {
        // NOTE: calculates amounts based on the desired amountOut
        amounts = UniswapV2Library.getAmountsIn(factory, amountOut, path);
        // NOTE: checks if the amounts is less than or equal to the user's max input
        require(amounts[0] <= amountInMax, 'UniswapV2Router: EXCESSIVE_INPUT_AMOUNT');
        // NOTE: transfers the user's input token to the first pair contract for trading
        TransferHelper.safeTransferFrom(
            path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]
        );
        // NOTE: performs the swap in the loop, traversing through all pairs in the path
        _swap(amounts, path, to);
    }
```
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->



## uniswap-v2 slippage

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-24-1769224978730-image.png)![slippage.png](https://github.com/Cyfrin/advanced-defi-2024/blob/main/excalidraw/amm/slippage.png?raw=true)

## RC 解锁哪些内容（3 个支柱）

**Cross-chain execution  跨链执行**

-   Listen to events on one chain, execute logic, and trigger callbacks on another chain.  
    监听一条链上的事件，执行逻辑，并触发另一条链上的回调。
    

**On-chain automation  链上自动化**

-   Automate smart contract workflows without relying on an off-chain bot being “online”.  
    无需依赖链下机器人“在线”，即可自动执行智能合约工作流程。
    
-   例如：  
    自动收取多个资金池的费用；链上止损单；清算保护；定期重新平衡/清扫/家务
    

**Modularity (add features without modifying the original contract)**

构建能够对现有合约中的事件做出反应的“扩展”，而不是升级或分叉它们。  
这就是止损单（在去中心化交易所交易池中）等模式的工作原理：你无需修改 Uniswap，只需订阅其事件并做出反应即可。（ [https://dev.reactive.network/education/use-cases/use-case-3](https://dev.reactive.network/education/use-cases/use-case-3) ）

**反应式合约与传统合约：** 与传统的智能合约不同，反应式合约能够自主监控区块链事件并执行操作，无需用户干预，从而提供更动态、响应更迅速的系统。

**控制反转：** RC 合约通过允许合约本身根据预定义的事件决定何时执行，从而颠覆了传统的执行模型，消除了对机器人或用户等外部触发器的需求。

**去中心化自动化：** RC 实现完全去中心化的运营，无需中心化的中介机构即可自动执行数据收集、DEX 交易和流动性管理等流程。

**跨链交互：** RC 可以与多个区块链和数据源进行交互，从而实现复杂的用例，例如跨链套利和多预言机数据聚合。

**实际应用：** RC 有多种应用，包括从预言机收集数据、实施 UniSwap 止损单、执行 DEX 套利以及自动在交易所之间重新平衡资金池。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->







## uniswap v2 笔记：

在调用任何需要代币的方法之前，必须先将代币转移到交易对（ Flash Swaps 是唯一的例外）

v2 交易对不支持直接使用 ETH ，因此 ETH 与 ERC-20 交易对必须使用 WETH 进行模拟

![{C29B0E51-5F73-466E-B027-1007A6DFA8DA}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-23-1769166114070-_C29B0E51-5F73-466E-B027-1007A6DFA8DA_.png)
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->









1、参加Co-Learning

2、参加Dapp Workshop

3、参加南塘 Dao 主题

4、做了拼豆DApp，萌

![{66BCF939-CDB6-49A1-BFB3-C49609947C16}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-22-1769070031149-_66BCF939-CDB6-49A1-BFB3-C49609947C16_.png)

5、Polymarket架构与链上数据解码

**Market（市场）**：对应具体的 Yes/No 问题，是交易发生的基本单位。一些事件只有一个市场（例如简单的二元事件），而有些事件包含多个市场形成一个多结果事件。

**Condition（条件）**

每个市场在链上的"登记身份"。创建市场时，会调用 CTF 合约的 `prepareCondition` 方法注册一个条件。`ConditionId` 是通过哈希计算得出的唯一标识：

```
conditionId = keccak256(oracle, questionId, outcomeSlotCount)
```

**Position（头寸）**

头寸指的是用户持有的某市场某结果的份额（又称 Outcome Share）。Polymarket 将每个头寸实现为一个 ERC-1155 标准的可交易代币（又称 PositionId 或 TokenId）。每种结果对应一个不同的 TokenId，用于区分 YES 和 NO 两种头寸。

**CollectionId（集合 ID）**

在条件代币框架中，中间引入了集合的概念，用于表示特定条件下某个结果集合。计算方法为：

```
collectionId = keccak256(parentCollectionId, conditionId, indexSet)
```

**TokenId（PositionId）**

最后，用抵押品代币地址和集合 ID 一起计算得到 ERC-1155 的 Token ID：

```
tokenId = keccak256(collateralToken, collectionId)
```

在 Polymarket 中，对于每个条件(市场)，会产生两个 TokenId —— 一个对应 YES 份额，一个对应 NO 份额。这两个 TokenId 是在该市场上交易的标的资产，代表了对同一预测问题的两种相反结果的头寸。

**Collateral（抵押品）**

Polymarket 市场的押注资金均以稳定币 USDC (Polygon 上为 USDC.e，地址 `0x2791...Aa84174`) 作为抵押品。每份 Outcome Token 背后对应 1 USDC 的抵押，当市场结算时兑现。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->












## **Solidity**

1.  EVM 四块内存（越往后越贵） Stack ➜ 计算临时区  
    Memory ➜ 函数内临时，不上链  
    Calldata ➜ 只读输入参数  
    Storage ➜ 永久上链，省它就是省 Gas
    
2.  Gas 口诀  
    少写 Storage → 缓存变量 → 小位宽(uint96) → external 比 public 省 → ++i 比 i++ 省
    
3.  数据类型  
    值类型（便宜）：bool / uint× / address / bytes32  
    引用类型（贵）：string / bytes / array / mapping  
    mapping 只能当「哈希表」用，无法遍历
    
4.  ERC20 最小接口  
    name() symbol() decimals() totalSupply()  
    balanceOf(owner) transfer(to,amount)  
    allowance(owner,spender) approve(spender,amount)  
    Transfer(from,to,amount) Approval(owner,spender,amount)
    
5.  函数可见性（从左到右权限递减，Gas 也递减）  
    external > public > internal > private  
    只读标记：view（读链）pure（不读链）
    
6.  构造函数  
    constructor() 仅部署时跑 1 次，用来初始化 name/symbol/decimals/totalSupply
    
7.  Mint 安全模板  
    function mint(…) external onlyOwner {  
    \_mint(to, amount); // internal 实际铸币  
    }
    
8.  错误处理  
    require(条件,“错误信息”) ← 最常用  
    revert() 无消息省 Gas  
    assert() 几乎不用（吞剩余 Gas）
    
9.  溢出  
    0.8+ 自动检查；想省 Gas 在确定无溢出块外加 unchecked{ … }
    
10.  Modifier 样板  
     modifier onlyOwner() {  
     require(msg.sender == owner);  
     \_;  
     }  
     用法：function foo() external onlyOwner { … }
     

## **modifier 是什么？具体操作逻辑是怎样的？**

1\. 基本定义

\- 功能定位：Solidity 中的函数修饰符，用于封装前置条件检查逻辑

\- 核心价值：代码复用、权限统一、Gas 优化

2\. 与传统 require 对比

| 方式 | 代码示例 | 缺点 |
| --- | --- | --- |
| 每个函数单独require | 每个函数都写 require(msg.sender ==owner, “…”) | 代码重复、部署 Gas 高、维护困难 |
| 使用 modifier | 函数后加 onlyOwner 修饰符 | 一次定义，多处复用，Gas 更优 |

3\. 扩展应用场景

重入保护：nonReentrant 修饰符防止重入攻击

时间锁：onlyAfter(block.timestamp)限制执行时间

角色权限：onlyAdmin、onlyMinter 等多角色控制

参数校验：validAmount(amount)统一校验参数有效性

4\. Gas 优化原理

部署时：modifier 逻辑只编译一次，减少合约字节码大小

执行时：跳转到统一校验代码，比分散 require 更高效

开发时：减少代码冗余，降低出错概率

# **Uniswap 工作原理解析**

![5e5221b845a3a9976faee516f00facca.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-21-1768996117227-5e5221b845a3a9976faee516f00facca.jpg)![aa1fb98e5b4e982c2107827aa0a96e7a.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-21-1768997600977-aa1fb98e5b4e982c2107827aa0a96e7a.jpg)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->
















# **Challenge #0 - Tokenization**

我完全懂力！教程非常详细，只是ipfs不好使就按照群里同学换了pinata

![{521BB8C2-59FC-40D9-846F-9C57CC326C46}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768882005193-_521BB8C2-59FC-40D9-846F-9C57CC326C46_.png)![{C80EA707-917B-4A2B-AD7E-FDC55AF7B160}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768882045742-_C80EA707-917B-4A2B-AD7E-FDC55AF7B160_.png)![{AEAF3589-FE55-46CA-9EAD-10DF440267F5}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768882017120-_AEAF3589-FE55-46CA-9EAD-10DF440267F5_.png)![{575D10C8-461B-43BD-9C76-F5E6E32E3FB8}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768882340399-_575D10C8-461B-43BD-9C76-F5E6E32E3FB8_.png)![{A45B1E07-6488-4AF4-BD8E-98C8AAE7D4C6}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768882366020-_A45B1E07-6488-4AF4-BD8E-98C8AAE7D4C6_.png)![{F6D95CC3-B3ED-40EA-B51E-4EB8E0B06C9D}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768887553064-_F6D95CC3-B3ED-40EA-B51E-4EB8E0B06C9D_.png)![{ABCF5070-036C-42B9-AA53-F6AB23793995}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768887654096-_ABCF5070-036C-42B9-AA53-F6AB23793995_.png)![{100F752C-0D70-4A0C-A514-94C630231DE5}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768888676054-_100F752C-0D70-4A0C-A514-94C630231DE5_.png)![{28D61E3D-2BC0-471C-8D84-F8AE8A5A7937}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768888767486-_28D61E3D-2BC0-471C-8D84-F8AE8A5A7937_.png)![{8260F019-F270-43F1-A002-F03B4270BF07}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768889036600-_8260F019-F270-43F1-A002-F03B4270BF07_.png)![{132F4201-28E7-46FF-8CF4-6F88B72C9A23}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768889406598-_132F4201-28E7-46FF-8CF4-6F88B72C9A23_.png)![{17AEDD68-56FC-45F7-8670-270E27515BDF}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768895867604-_17AEDD68-56FC-45F7-8670-270E27515BDF_.png)![{911A8F9B-0767-4416-B53B-EBFC8C9E8970}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768896990997-_911A8F9B-0767-4416-B53B-EBFC8C9E8970_.png)

# [**Gas 优化**](https://web3intern.xyz/zh/smart-contract-development/#_1-gas-%E4%BC%98%E5%8C%96)

[**常见优化技巧**](https://web3intern.xyz/zh/smart-contract-development/#%E5%B8%B8%E8%A7%81%E4%BC%98%E5%8C%96%E6%8A%80%E5%B7%A7)

1.  **减少存储操作（Storage Write）**
    
    -   读取存储第一次需 2100 gas（后续 100 gas），而内存读取仅 3 gas。推荐多次访问同一存储数据时，将其缓存到内存以减少 SLOAD 次数
        
    -   每次写入 `storage` 的成本高达 20,000 gas；优先使用 `memory`。
        
2.  **使用位压缩（Bit Packing）**
    
    -   将多个变量压缩到一个 `uint256` 中以节省存储空间。
        
3.  **循环优化**
    
    -   减少不必要的运算，如 `array.length` 缓存到变量中。
        
4.  **函数可见性选择** - `external` 比 `public` 更节省 gas，适用于仅被外部调用的函数。
    

![{79D05BE9-431F-4C6E-8A2D-61925A9364A8}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-20-1768911995657-_79D05BE9-431F-4C6E-8A2D-61925A9364A8_.png)

# **Web3 公共物品资金第一节课**

**什么是公共物品？**

非排他性和非竞争性

**期待良心杀**
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->



























# **My first zk vote**

![{A67417D0-33A1-49ED-B5B7-CC10EBBD196F}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-19-1768810634689-_A67417D0-33A1-49ED-B5B7-CC10EBBD196F_.png)

## **零知识证明基础：证明什么、向谁证明？**

### **Prover / Verifier 模型**

零知识证明通常用两个角色来描述：

-   **证明者（Prover）**：掌握某个秘密信息，希望向他人证明自己「确实知道」这个秘密；
    
-   **验证者（Verifier）**：希望确认某个断言是真的，但不想（或不允许）知道该秘密本身。
    

### **Statement / Witness：你在“证明”什么？**

我们把「要被证明的陈述」称为 **语句（statement）**，把证明者掌握的秘密称为 **见证（witness）**。

见证则包括：

-   你的私有身份秘密（如某个随机生成的 `identitySecret`）；
    
-   证明你是选民集合成员所需的 Merkle 路径；
    
-   你选择的具体投票选项等私密数据
    

### **零知识证明必须满足的三个性质**

形式上，一个零知识证明系统需要同时满足三个核心性质：

1.  **完备性（Completeness）**
    
    如果语句是真的，并且证明者与验证者都按照协议诚实地执行，那么验证者最终一定会被说服。
    
2.  **可靠性（Soundness）**
    
    如果语句是假的，那么任何试图伪造证明的「作弊」证明者，都几乎不可能骗过诚实的验证者。
    
3.  **零知识性（Zero-Knowledge）**
    
    验证者在整个证明过程中，除了“语句是真的”这一事实之外**不会获得任何额外信息**。
    
    换句话说，验证者无法从证明内容中，还原出证明者所掌握的秘密。
    

## ZK 投票流程：

![{04A9FE53-36D4-4996-9D14-238D5555848D}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-19-1768810813928-_04A9FE53-36D4-4996-9D14-238D5555848D_.png)

**链上公开：我提交了一个“有效的证明”。 链上不公开：我是谁、我投了什么。**

### **身份注册与选民集合**

![身份承诺、Merkle 树与 nullifier 示意](https://zkvote.0xtmp.xyz/images/merkle-nullifier.svg)

# Remix 使用：

![{27AF75D7-9075-44D8-8C16-8337F286776F}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-19-1768813052368-_27AF75D7-9075-44D8-8C16-8337F286776F_.png)
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->






























# foundry 创建新项目的 forge 测试

Foundry 提供以下以太坊开发工具：

-   `forge`: 帮助构建、测试、调试、部署和验证智能合约
    
-   `anvil`: 本地开发节点，完全兼容以太坊 JSON-RPC 规范
    
-   `cast`: 命令行工具，用于与链上应用交互
    

**初始化项目**

```bash
forge init Counter
```

**测试合约**

```bash
# Compile your contracts
forge build

# Run your test suite
forge test
```

![{E74AD52D-E213-4618-B739-E8420B693853}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-18-1768708410319-_E74AD52D-E213-4618-B739-E8420B693853_.png)

**启动本地节点**

```bash
anvil
```

![{FA0B0033-59E2-4F32-BE12-11A7446AEC41}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-18-1768708428260-_FA0B0033-59E2-4F32-BE12-11A7446AEC41_.png)

启动本地节点后发现多了cache和out两个新的目录

![{ABF5EAAD-AB4B-4C5D-BDB0-E09EBC0DD0F3}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-18-1768708537042-_ABF5EAAD-AB4B-4C5D-BDB0-E09EBC0DD0F3_.png)

`out` 目录包含你的合约工件( artifact ，例如 ABI ，而 `cache` 目录被 `forge` 使用来（记录），以便仅仅去重新编译那些必要编译的内容。

# **Hardhat（推荐，现代以太坊开发框架）**

这个很熟不记录了

```bash
npm install --global hardhat
mkdir eth-dev && cd eth-dev
npx hardhat
```

选择“创建一个基本示例项目”，会自动生成合约、测试和配置。

**启动本地节点**

```bash
npx hardhat node
```

**部署合约**

```bash
npx hardhat run scripts/deploy.js --network localhost
```

# **以太坊钱包和前端交互**

-   推荐使用 MetaMask 浏览器插件作为开发钱包。
    
-   前端推荐使用 Viem 和 Wagmi。
    

# **其他常用工具**

-   Remix IDE（网页版，适合快速测试合约）：[**https://remix.ethereum.org**](https://remix.ethereum.org)
    
-   OpenZeppelin 合约库：`npm install @openzeppelin/contracts`
    
-   [**Chainlink 测试环境**](https://docs.chain.link/resources/link-token-contracts) - 预言机集成指南
    

# **分享会 - Key Hash Based Tokens: 从 ERC-721 到 ERC-7962**

## ERC-7962 Idea Design:

会员身份属于隐私数据，但是如果把ZK应用其中，就能够在不暴露隐私的情况下进行数据交易，在现实社会中非常有意义。

ZK：零知识证明

在不暴露具体细节的情况下，能够向第三方证明数据的某种特性。

![{8651F227-48E7-4AF6-9868-491722C33D87}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-18-1768738509892-_8651F227-48E7-4AF6-9868-491722C33D87_.png)![{E063B236-B548-4754-81A8-BDF421160C23}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-18-1768738793146-_E063B236-B548-4754-81A8-BDF421160C23_.png)

## ERC-7962 核心概念：

-   以 KeyHash 而非地址作为资产所有权表示
    
-   所有权与发起地址解耦，不在链上存储地址
    
-   覆盖 ERC-20 与 ERC-721 两类资产模型
    
-   用户只需要签名表达意图，无需持币、无需理解 Gas 、无需管理地址隐私，也能完成资产操作
    

## ERC-7962解决的关键问题

-   隐私问题：避免地址可追踪，降低身份与资产的关联性
    
-   UX 问题：用户只需要签名表达意图，无需持币、无需理解 Gas 、无需管理地址隐私，也能完成资产操作
    
-   拓展性问题：天然支持批量交易、Relayer 、账户抽象场景
    

## Gas 与交易模型

-   任何地址都可以提交交易
    
-   所有权只由签名控制
    
-   天然支持 Gas Sponsorship
    
-   批量转账
    
-   Relayer / Payment
    

## 优势总结

-   更强隐私
    
-   更加用户体验（免 ETH / Gas 代付）
    
-   更高灵活性
    
-   与 ERC-5564 观念一致
    

**局限**： Gas 更高、 key 轮换
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->



































# 准备以太坊开发环境

## [Foundry安装](https://getfoundry.sh/introduction/installation)

```
curl -L https://foundry.paradigm.xyz | bash
```

-   如果你是windows显示bash不是内部命令或者外部命令则需要借助[git bash](https://git-scm.com/install/)或者wsl，下面简单介绍git bash流程
    

进入官网下载，安装时勾选"Use Git and optional Unix tools from the Command Prompt"，将bash相关命令添加到PATH

```
bash --version
```

配置环境变量后再验证

```
where bash
```

返回bash完整路径即成功

![{D4134E63-AFCB-426C-BF44-D03B6B3EF3AD}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-17-1768655107916-_D4134E63-AFCB-426C-BF44-D03B6B3EF3AD_.png)

按照它说的运行source …再运行foundryup

如果你想安装由[Tempo 的 Foundry 分支](https://github.com/tempoxyz/tempo-foundry)分发的二进制文件

```
foundryup -n tempo
```

![{2ED67B66-B616-4824-993E-94FADD4D74F6}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-17-1768655284812-_2ED67B66-B616-4824-993E-94FADD4D74F6_.png)

### **从源头构建**

**先决条件**

你需要[Rust](https://rust-lang.org/)编译器和 Cargo（Rust 的包管理器）。最简单的安装方法是使用[https://rustup.rs/](https://rustup.rs/)

Foundry 通常只支持使用最新稳定版 Rust 进行构建。如果您使用的是旧版 Rust，可以通过以下命令进行更新`rustup`：

```
rustup update stable
```

**建筑**

[您可以使用Foundryup](https://getfoundry.sh/introduction/installation#using-foundryup)提供的各种标志：

```
foundryup --branch masterfoundryup --path path/to/foundry
```

或者，您也可以使用以下命令通过 Cargo 安装：

```
cargo install --git https://github.com/foundry-rs/foundry --profile release --locked forge cast chisel anvil
```

[您也可以从Foundry 代码库](https://github.com/foundry-rs/foundry)的本地副本手动构建：

```
# clone the repository
git clone https://github.com/foundry-rs/foundry.git
cd foundry
# install Forge
cargo install --path ./crates/forge --profile release --force --locked
# install Cast
cargo install --path ./crates/cast --profile release --force --locked
# install Anvil
cargo install --path ./crates/anvil --profile release --force --locked
# install Chisel
cargo install --path ./crates/chisel --profile release --force --locked
```

### **使用 GitHub Actions 进行 CI 安装**

有关在 CI 流水线中设置 Foundry 的说明，请参阅[foundry-rs/foundry-toolchain](https://github.com/foundry-rs/foundry-toolchain) GitHub Action。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->




































# [**Day 5: Stuck Transactions, Gas Limits, Multisigs, L2s, Lending…**](https://www.youtube.com/watch?v=11QTT6BK5j0&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=5)

## Gas费用的构成与调整

-   **Gas Limit**：交易允许的最大计算资源，防止智能合约无限执行（涉及“停机问题”）。
    
-   **Max Fee**：交易愿意支付的最高Gas价格，是矿工选择打包交易的竞价。
    

## Nonce的重要性：

-   Nonce用于防止交易重放攻击，确保每笔交易唯一且按顺序执行。
    
-   如果一个Nonce的交易卡壳，后续的交易即使Gas价格更高也无法执行，导致用户体验困扰。
    
-   解决方法是“加速”或“取消”卡壳交易，MetaMask提供了相应按钮。
    

MetaMask的“重置账户”功能：只清除交易历史，不涉及私钥或资金安全，适用于交易卡壳或链ID混乱时的修复操作。

## “取消交易”的原理及实践

-   取消交易本质是发送一笔相同Nonce但无实际转账（0 ETH，发送给自己）的交易，以覆盖原交易。
    
-   MetaMask的取消按钮实际上是加速并替换交易，手动操作虽可行，但有时界面限制导致操作不便。
    

展示如何直接通过Etherscan智能合约界面调用DAI合约`transfer`函数进行转账，注意输入金额时需要考虑代币的小数位（通常是18位），并演示了Gas费用设置对交易成功的影响。

## 资产从以太坊主网桥接到Layer 2（L2）网络Optimism的过程

-   L1主网安全性高但交易慢且费高。
    
-   L2利用扩容技术实现更快更便宜的交易，同时保证安全性（通过在L1进行结算）。
    
-   桥接操作涉及跨链存款，存在“资金中转期”，用户需要切换MetaMask网络到Optimism使用资产。
    

## 借贷平台Aave的基本操作

-   以太坊主网交易费高，借贷操作成本昂贵（几十美元Gas费），不适合小额操作。
    
-   演示抵押ETH作为抵押品，生成代表抵押品的aToken。
    
-   讲解借贷原理，包括借贷杠杆、清算风险等金融概念。
    

借贷手续费和Gas费用目前主网操作成本较高。

## **Gnosis Safe多签钱包**

-   多签钱包通过设置多个签名者和签名阈值（例如2/4），显著提升资金安全。
    
-   多签钱包可防止单点私钥泄露导致资金被盗。
    
-   演示部署过程、添加签名者、设置阈值，以及转账流程中的多签协作。
    

# [Unphishable](https://unphishable.io/) 钓鱼攻防挑战

## 0x0010:剪贴板钓鱼

**这种攻击的运作原理：**

1.  攻击者创建了一个看似合法的恶意网站。
    
2.  当您点击“复制”按钮时，恶意 JavaScript 代码会悄悄替换您复制的地址。
    
3.  粘贴地址后如果不进行核实，可能会将资金发送给攻击者。
    

**如何确保安全：**

-   粘贴地址后务必仔细核对。
    
-   考虑使用带有地址验证功能的硬件钱包
    
-   在电脑上复制敏感信息（例如钱包地址）时务必小心——即使使用 Ctrl+C，恶意软件也可能修改剪贴板内容。
    

## 0x0011:**Microsoft Teams 恶意软件网络钓鱼**

**关键学习要点：**

-   务必检查网址是否为官方域名（[teams.microsoft.com](http://teams.microsoft.com)）。
    
-   要警惕简化的界面和缺失的标准安全功能。
    
-   请仅从官方来源下载应用程序
    
-   对异常的安装过程保持警惕
    

# 第一周例会

群友们分享后我觉得可以借鉴的清晰完整的思路：

1.  明确学习目标
    
2.  借用工具无论是思维导图还是ai
    
3.  关注行业资讯，看v神发言肯定常看常新
    
4.  学习是自己的
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->







































# [**Day 4: NFTs!!! ERC20 vs ERC721, IPFS, Metadata**](https://www.youtube.com/watch?v=NOdrEpnoCiM&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=4)

## NFT的多样性和市场现状

-   NFT类型包括限量收藏品（如CryptoPunks、Bored Apes）及其他类别
    
-   需关注作品的稀缺性和社区认同
    
-   限量发行限制了NFT总量，保障数字稀缺性和价值
    
-   市场只认可原创合约铸造的NFT
    

## NFT的“实用性”：NFT具备功能性价值

-   如Token Gating，拥有特定NFT即可进入专属聊天室
    
-   NFT可作为投票权工具（如Tally投票系统）
    
-   游戏化NFT示例：Chicken Derby等区块链游戏内资产
    
-   NFT可基于不同区块链Layer 2扩展，OpenSea支持多链接入
    

## 链上NFT（On-Chain NFT）与链下NFT的区别

-   链上NFT的图像和元数据完全存储在智能合约中，成本较高但具备高度可组合性和动态交互能力
    
-   链下NFT通常将元数据存储在IPFS等去中心化存储网络，智能合约**仅**保存指向IPFS内容的哈希值
    
-   链上NFT具备游戏与艺术创作的扩展潜力
    

## NFT铸造流程

-   用户通过连接MetaMask钱包到DApp，调用智能合约的mint方法购买NFT
    
-   交易涉及向智能合约地址转账，有一定信任风险，建议用户查看合约源码验证安全性
    
-   NFT自动归属于用户钱包，支持跨平台流转和展示
    

## Web2与Web3平台的差异

-   Web2平台存在内容审核和“审查”行为，决定展示什么资产
    
-   Web3强调**抗审查性**，任何符合标准的NFT应被市场接受并展示
    

## 智能合约读取NFT元数据的过程

-   通过tokenURI方法获取存储在IPFS上的元数据JSON文件
    
-   元数据包含名称、描述、图像地址等信息
    
-   如何访问IPFS内容，理解内容寻址和不可篡改性
    
-   说明IPFS基于点对点存储，存在内容“丢失”风险，需要引入激励机制和“pinning”服务确保数据持久性
    

## IPFS内容地址的特性

| 变更内容示例 | 生成的IPFS哈希变化情况 | 说明 |
| --- | --- | --- |
| 内容为“buffalo” | 哈希值结束为 h19d | 内容不变，地址不变 |
| 内容改为“bison” | 哈希值变化为 xcxq | 内容变更，地址相应改变 |

## FT与NFT智能合约的核心区别

| 特性 | 同质化代币（ERC-20 类似） | 非同质化代币（ERC-721 类似） |
| --- | --- | --- |
| 资产表现形式 | 持有者拥有某个数量的代币余额 | 持有者拥有某个唯一的资产编号（tokenID） |
| 资产区分度 | 代币相互可替换，余额总和为所有资产总量 | 每个tokenID对应唯一资产，拥有独特的元数据和所有权 |
| 转账逻辑 | 减少发送方余额，增加接收方余额 | 更改资产tokenID的所有者地址 |

## NFT交易的本质

-   NFT转移是区块链账本所有权状态的更新
    
-   所有节点同步更新所有权，保证分布式账本的一致性
    
-   用户通过签名交易授权所有权变更
    

# [**Unphishable**](https://unphishable.io/) **钓鱼攻防挑战**

## **0x0008：Telegram Token 钓鱼**

关于助记词都不给

## 0x0009:**Punycode 网络钓鱼攻击**

**Punycode是什么？**

Punycode 是一种编码系统，它允许将非 ASCII 字符（例如西里尔字母、中文等）转换为 ASCII 字符，以便在域名系统中使用。攻击者经常使用视觉上相似的字符来创建看似合法的域名。例如，某些特殊字符看起来几乎与拉丁字母完全相同，但它们实际上是不同的字符：可以使用[Punycoder](https://www.punycoder.com/)在 Unicode 域名和 Punycode 域名之间进行转换。

| 显示域 | 实际 Punycode 域名 | 描述 |
| --- | --- | --- |
| trẹzor.com | trẹzor.com | 使用特殊字符替换某些字母 |

# AI概念及分享会

## AI智能体的演进

-   传统大语言模型（LLM）本质是“高级复读机”，缺乏\*\*状态记忆\*\*、\*\*工具调用能力\*\*和\*\*法律身份\*\*，只能“动嘴不动腿”。
    
-   未来的agentic AI（2026年及以后）将具备：
    

1.  **私有记忆**
    
2.  **工具调用能力**（API、执行代码等）
    
3.  **链上身份**（ERC8004标准）
    

## AI智能体的三大核心组件

-   **大语言模型（LLM）**：推理引擎，负责理解意图和生成指令。
    
-   **记忆系统**：向量数据库实现私有化记忆，支持上下文连续交互。
    
-   **工具接口**：API调用（如函数调用、代码执行），实现“说到做到”的能力。
    

当前AI智能体面临的三大技术缺口：**身份不可验证；行为不可审计；部署不可管控**

## ERC-8004：智能体的链上护照

1.  身份注册表（Identity Registry）：使得AI Agent与现有NFT生态兼容，每个AIAgent通过调用register函数注册，获得一个唯一的agentId.
    
2.  声誉注册表（Reputation Registry）：与身份注册表关联紧密，在部署时需传入身份注册表的地址，确保只有已经注册的用户才能获得声誉记录
    
3.  验证注册表（Validation Registry）：提供通用Hook用于请求和记录独立验证结果，支持多种验证机制。
    

_ERC-8004是AIAgent生态的身份层协议_

## x402协议：HTTP原生的机器支付(链上支付协议)

核心创新在于激活互联网未充分利用的HTTP402状态码，实现API调用即是支付，通过稳定币或者加密货币完成结算，解决了传统支付中高摩擦的问题。

应用场景：按文件付费，按算力付费，无需订阅

工作流程是四次握手

## **Spoon OS：智能合约与SDK框架**

\- 封装钱包管理、身份验证、支付和签名逻辑，降低开发门槛。

\- 支持多模型、多agent协作及复杂任务编排。

## 安全与合规：从"黑盒"到“透明审计”

-   合规需求
    
-   持续监控
    
-   欧盟AI法案
    

## Web3解决方案

-   **不可篡改**的审计日志
    
-   **所有推理**、交易、验证记录**永久存证**
    
-   满足最严格的**监管审计要求**
    

## Spoon OS框架架构概览

| 层级 | 功能描述 |
| --- | --- |
| 输入层 | 数据采集（链上数据、文档）、预处理、向量化存储 |
| 核心层 | 模型统一API接口、智能体编排引擎、多agent协作、记忆管理器 |
| 协议层 | 实现ERC8004身份管理、X402支付网关、钱包托管与签名 |
| 输出层 | 与外部世界交互（DeFi、跨链桥、API调用等） |
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->












































# [Day 3: ENS, DEX, Identity, Inventory, Sybil](https://www.youtube.com/watch?v=wYSMNdIRoII&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3)

## ETH交易与Gas机制

-   以太坊交易需要支付Gas，Gas动态调整
    
-   EIP-1559升级让Gas估算更智能，交易更顺畅
    
-   交易执行速度与Gas出价相关，出价高交易更快
    
-   交易通过“内存池”（Mempool）等待矿工打包
    

## 地址可读性问题

-   以太坊地址由长字符串组成，难以记忆和使用
    
-   MetaMask中提供地址的区块预览图辅助识别
    
-   ENS的出现就是为了解决地址难记问题，提供人类可读的域名式地址
    

## 交易确认与区块浏览器

-   交易完成后，通过第三方区块浏览器（如Etherscan）确认交易成功
    
-   交易公开透明，任何人都可查验
    

## ENS与DNS相似性

-   DNS（域名系统）将IP地址映射为易记域名，ENS类似地将以太坊地址映射为易记名称
    
-   ENS支持子域名，例如game.eda.eth
    
-   ENS允许创建多个身份（多账户、多子域），提升灵活性
    

三字母ENS域名非常抢手，价格高昂

## commit-reveal机制

-   首先提交commit交易，隐藏想注册的名称哈希
    
-   等待至少一分钟后提交reveal交易，公开名称并完成注册
    
-   防止他人监控网络抢注相同名称
    
-   只有提交commit的账户能完成reveal，确保注册安全性
    

## ENS名称使用示范

-   在钱包中直接输入“sanfordstout.eth”即可发起转账，无需复制复杂地址
    
-   ENS支持正向解析（名称→地址）和反向解析（地址→名称），反向解析便于网站显示用户名
    

## 反向解析设置

-   通过发送交易设置反向记录，使地址能够映射回ENS名称
    
-   反向解析交易必须由**对应地址**发出，防止伪造身份
    
-   反向解析提升用户体验，应用可直接显示人类可读名称
    

## Uniswap去中心化交易所（DEX）

-   Uniswap为智能合约，持有不同代币储备，实现自动做市和交易
    
-   价格由储备比例自动调整，套利机器人帮助维持价格平衡
    
-   用户可直接通过智能合约进行代币兑换，无需信任第三方
    

### DEX核心原理

| 术语 | 说明 |
| --- | --- |
| Reserves | 智能合约中存放的两种代币数量 |
| AMM | 根据储备比例自动调整兑换价格 |
| Arbitrage | 机器人利用不同交易所价格差异进行交易，维持价格平衡 |
| LP | 向合约提供代币储备，获得交易手续费奖励 |

## 身份与资产同步

在[Zapper](https://www.zapper.com/)等Web3工具中输入ENS名称即可查看该地址持有的资产和历史。

-   ENS名称作为身份标识，资产和交易记录跟随该身份跨平台显现
    
-   反映Web3中“身份+资产”统一的核心理念
    

## 稳定币简介与重要性

-   以太坊资产价格波动大，影响用户体验和支付可靠性
    
-   稳定币如DAI锚定美元，保持相对稳定价值
    
-   稳定币通过复杂机制维持价格稳定，包括供应调节和抵押贷款
    

## DAI稳定机制

| 机制 | 说明 |
| --- | --- |
| 铸币与销毁 | 价格偏离时调节供应，通过铸造或销毁DAI影响价格 |
| Maker代币 | 作为辅助代币，价格波动时用于稳定系统 |
| 超额抵押贷款 | 用户锁定超过借贷价值的ETH，防止清算风险 |
| 清算机制 | 当抵押不足时自动清算，保护系统安全 |

## 资产多平台显示演示

-   MetaMask需手动添加DAI代币合约地址才能显示资产
    
-   ENS身份绑定资产，形成完整数字身份体系
    

# [Unphishable](https://unphishable.io/) 钓鱼攻防挑战

## 0x0006:假代币空投

**如何防范域名欺骗：**

-   务必逐字逐句地仔细检查域名。
    
-   要**警惕使用数字**而不是字母的域名。
    
-   将官方网站添加到收藏夹，而不是点击电子邮件中的链接。
    
-   使用仅对合法域名自动填充密码的密码管理器。
    
-   安装浏览器扩展程序，以防范钓鱼网站
    

## 0x0007：模拟质押合约

**可疑质押平台的危险信号：**

-   不切实际的回报（日均 50% 的回报在数学上是不可持续的）
    
-   代币审批次数不限，而非仅限于特定数量。
    
-   **没有可验证的智能合约代码或审计**
    
-   紧迫感和害怕错过 (FOMO) 营销策略
    
-   匿名团队或虚假身份
    

安全提示：务必将代币审批数量限制在所需的确切数量内！

# Web3安全分享会

## 2025年Web3安全态势总览

-   **安全事件数量**：2025年共发生超过1200起较严重的安全事件。
    
-   **损失金额**：累计损失超过35亿美元。
    
-   **攻击趋势**：同时存在“精准猎杀”与“广撒网”两种并行的攻击模式。
    
-   **攻击手法**：多样化且愈发复杂，技术融合与智能化特征明显。
    

## 攻击趋势详解

| 攻击模式 | 特点 | 主要攻击类型 | 影响与挑战 |
| --- | --- | --- | --- |
| 精准猎杀 | 针对性强，锁定高价值目标 | 重大资产损失事件（Top事件） | 单次攻击损失高，影响深远 |
| 广撒网 | 高频率、覆盖面广，利用大众用户的安全薄弱环节 | 私钥窃取、钓鱼攻击、Rug Token诈骗 | 频发导致用户信任下降，AI助力钓鱼攻击难以识别 |

-   **私钥窃取**：依然是最直接且高危的资产盗取手段。
    
-   **钓鱼攻击**：随着AI技术普及，攻击手法更加隐蔽，识别难度加大。
    
-   **Rug Token**：攻击呈现职业化和团伙化趋势，诈骗方式更加系统化。
    

## 典型案例与攻击手法演变

-   **技术融合**：攻击者将多种技术手段结合，包括新型协议漏洞、供应链攻击、人工智能工具和自动化技术。
    
-   **智能化与自动化**：攻击越来越依赖自动化工具，减少人力成本，提高攻击效率和隐蔽性。
    
-   **新型攻击范式**：不再局限于单一漏洞利用，而是通过多层次、多渠道联合攻击，形成更强破坏力。
    

| 更多案例及详细分析链接：https://x.com/GoPlusZHhttps://x.com/GoPlusSecurity |

## AI时代下的安全新挑战

-   **AI助攻攻击**：钓鱼邮件和诈骗手段变得更加精准和难以识别，传统防护手段面临挑战。
    
-   **自动化攻击工具普及**：攻击者利用AI和自动化工具快速扫描并利用漏洞，攻击频率和效率大幅提升。
    
-   **安全防御压力加大**：Web3安全防护需适应AI带来的新型威胁，传统安全机制亟需升级。
    

## GoPlus安全建议

-   安全作为基础设施
    
-   全面安全防护
    
-   主动防御与风险管理
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->













































# Ethereum 官网 [Overview](https://ethereum.org/learn/) 基础内容学习

这是一个官方的社区，所有基础的问题都可以得到解答。而且可以通过小测试来看自己的基础知识熟练程度。能复习到很多想不起来的知识）

![{8179956E-E244-4958-940C-4B5BC2CD08D4}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768270534204-_8179956E-E244-4958-940C-4B5BC2CD08D4_.png)![{A1861CFD-AECA-4472-8AB1-E8E17352A4E7}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768270810279-_A1861CFD-AECA-4472-8AB1-E8E17352A4E7_.png)![{E34D033C-F1A4-4A26-8D4F-D4FFD1ABF7C1}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768271809835-_E34D033C-F1A4-4A26-8D4F-D4FFD1ABF7C1_.png)![{F2198AB6-DF0F-40A7-926B-525E385615E7}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768270903389-_F2198AB6-DF0F-40A7-926B-525E385615E7_.png)![{1B13C47B-EBAE-4E21-9FD4-BD208D636A48}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768272004331-_1B13C47B-EBAE-4E21-9FD4-BD208D636A48_.png)![{616A74C7-F5CA-4D0D-B501-76F92ECB9BD6}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768272340840-_616A74C7-F5CA-4D0D-B501-76F92ECB9BD6_.png)![{3791158C-00DD-49D5-B43C-9074330D1ACC}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768272318029-_3791158C-00DD-49D5-B43C-9074330D1ACC_.png)![{7015DDF1-226C-44F1-A640-733B62751438}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768273008285-_7015DDF1-226C-44F1-A640-733B62751438_.png)![{7C5B78F1-920E-42AE-8BEB-2F7ACE3858B8}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768273416423-_7C5B78F1-920E-42AE-8BEB-2F7ACE3858B8_.png)

# [**Day 1: A Developer’s Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro

Austin Griffith、Eda 和 Carlos关于这个课程的安排：

第1周：成为以太坊的用户；设置 MetaMask 钱包；了解 gas 费用和交易流程；学习使用去中心化应用（DApps）、ENS 域名服务；体验去中心化交易所（DEX）兑换代币；认识 NFT 及 ERC-20、ERC-721 标准

第2周：脚本与智能合约交互；编写脚本调用智能合约；了解提供者（Providers）、签名者（Signers）和 ethers.js 库；学习测试框架 Hardhat

第3周：速成实战(通过[speedrunethereum](https://speedrunethereum.com/)项目实战：构建 NFT、质押应用、代币售卖器、掷骰子游戏);进阶学习 DEX、多签钱包、SVG NFT、FTS 等

第4周：开放式项目开发与导师指导。自由发挥，导师团队提供支持和指导

**区块链技术简介与核心概念**

-   区块链是一个去中心化、无中央控制的网络系统，通过全球数千节点点对点通信实现共识。
    
-   目前共识机制以工作量证明PoW（Proof of Work）为主，未来将过渡到权益证明PoS（Proof of Stake）
    
-   区块链本质是一个网络层级的分布式账本，数据以区块链形式串联，节点协作维护账本完整性。
    
-   用户通过私钥生成地址，无需中介即可进行资金收发
    
-   信任最小化（Trust Minimization）是区块链的核心优势，智能合约充当“自动化托管（Escrow）”的角色，保障交易双方在互不信任的情况下安全交互
    

**智能合约与信任机制**

-   智能合约相当于程序化的自动售货机，基于“if-then”规则运行。
    

举例说明：在买卖鞋子的场景中，智能合约作为中间托管，确保买卖双方同时满足条件时才完成资产交换。传统托管依赖人为信任，智能合约基于代码执行，降低了信任成本。

**区块链应用及其特点**

-   智能合约部署后不可更改，具备不可阻断性（Unstoppable）
    
-   网络高度冗余，每个节点保存完整数据，保证服务持续和抗审查能力
    
-   以太坊生态的核心是信任代码即法律，程序即规则
    
-   传统开发者需要时间建立新的思维模型，理解区块链的全局架构与机制
    

**当前以太坊应用场景举例**

| 应用类型 | 功能描述 | 说明 |
| --- | --- | --- |
| 去中心化金融（DeFi） | 通过智能合约实现资产投资、借贷、流动性挖矿等金融活动 | 流动性池（LP），赚取手续费 |
| 去中心化自治组织（DAO） | 组织治理以代币投票决定方向，强化社区自治 | Snapshot、Tally 等投票工具 |
| 非同质化代币（NFT） | 数字收藏品、身份验证、权限管理等 | SVG NFT（链上渲染）、Token Gated 应用 |

**区块链现存问题与改进方向**

区块链存在性能瓶颈（例如交易延迟、手续费高昂），以及用户私钥管理风险（私钥丢失即资产丢失）等问题，

未来技术改进方向包括分层扩容（L2）、多重签名钱包设计等增强用户体验和安全性的方案。

**未来展望与技术潜力**

以太坊等区块链技术可能在更大范围内实现跨国协作与协调机制

例如在国家层面进行军事预算等复杂协调，智能合约可作为**无信任的自动执行工具**，替代传统的人为中介

技术赋予个人更大改变世界的能力，尤其是开发者能通过智能合约构建各种创新的协调和信任机制

**成功路径分三个层级：**

| 序号 | 职业目标 | 具体方向 |
| --- | --- | --- |
| 1 | 获得区块链相关职位 | 掌握基础知识，能胜任行业内岗位 |
| 2 | 自主开发项目 | 构建并运营自己的去中心化应用或产品 |
| 3 | 成为智能合约安全审计专家 | 深入学习安全漏洞，参与合约审计，提高行业安全性 |

# [**Day 2: Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)

重点在以太坊钱包的设置与使用。

**MetaMask安装与使用基础**

MetaMask是主流的以太坊浏览器插件钱包，注重安全性优先，用户体验可能略显笨重。

支持Chrome插件、iOS和Android手机应用。

安装时需注意：确保从官网（[metamask.io](http://metamask.io)）下载，防止钓鱼网站。

MetaMask的标志性狐狸头像是安全设计元素，难以被伪造。

**助记词顺序确认与安全性讨论**

用户需按正确顺序确认12词助记词，防止错误。

有疑问：是否有人能通过“穷举”助记词组合找到别人的钱包？

统计学角度解释：虽然理论上可能，但因词汇量约2048个，12词组合极其庞大，穷举需要宇宙寿命级别的时间，即使用全世界所有计算机也无法实现。

**交易发送与验证过程**

以太坊地址以“0x”开头，用户需仔细核对地址避免转错。

发送交易时需要支付少量“Gas费”，即交易手续费，视网络拥堵情况变化。

交易完成后，用户可通过Etherscan等区块浏览器查询交易详情，包括发送方、接收方、金额、手续费等。

交易一旦确认无法撤销，无客户服务介入，用户需谨慎操作。

交易可以通过提高Gas费用“加速”或“替换”未确认交易。

**Etherscan区块浏览器的作用**

Etherscan是以太坊重要的公共基础设施，允许用户查询链上所有交易和账户状态。

用户可通过交易哈希查看交易是否成功，确认资金流动。

支持查看主网及各测试网数据。

提供透明、不可篡改的链上数据验证，帮助用户独立核实资产安全。

**私钥与签名机制**

私钥是签署交易和消息的唯一凭证，必须严加保护。

签名允许在不暴露私钥的情况下验证交易发起者身份。

任何修改签名信息都会导致签名验证失败，确保数据不可篡改。

交易哈希是交易数据及签名的哈希值，用于唯一确定交易。

**托管钱包与非托管钱包的区别**

托管钱包（如Coinbase）由第三方持有私钥，用户实际拥有的是数据库中的账户余额记录。

非托管钱包（如MetaMask）用户自行管理私钥，资产真实存在区块链上。

“不拥有私钥即不拥有资产”——托管风险包括黑客攻击、平台倒闭等。

托管钱包适合初学者和小额资产，非托管钱包适合有一定经验且注重资产安全的用户。

**其他钱包推荐与硬件钱包简介**

除MetaMask外，还有Rainbow、Coinbase Wallet等多种以太坊钱包可选，体验和设计各异。

Rainbow钱包注重移动端用户体验，流程相似，均基于12词助记词。

硬件钱包是一种物理设备，用于安全存储私钥，私钥永不离开设备。

硬件钱包适合长期冷存储，交易时需连接电脑签名，安全性高但使用繁琐。

# [Unphishable](https://unphishable.io/) 钓鱼攻防挑战

## 0x0004:空投骗局

在实际情况下，千万不要在不了解交易内容的情况下签署任何交易，其实还是能看出来的这个测试中小狐狸也有提示红色感叹号来告知可能存在的风险。

![{910987EA-1363-4EE1-95A6-597D281D2C4A}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768309310538-_910987EA-1363-4EE1-95A6-597D281D2C4A_.png)

## 0x0005:**USDT审批钓鱼**

除了批准之外，还要注意增加权限的功能。

# 分享会 - Web3 运行原理：从钱包出块、从应用到协议

## 钱包、私钥和个人主权

**私钥**：私钥泄露钱包里所有资产都面临危险（**私钥→公钥：1→1**）

**助记词**：可读形式的私钥，十几个到二十四个单词。助记词是所有资产的“母体”，它可以派生出无数个账户（**助记词→私钥：1→N**）

**地址**：公钥截取后的字符串，公开收款号。由私钥推导而来。

**公钥**：通过私钥计算，用来验证签名

## 交易与签名

### **交易tx=你要做的事+手续费+防重放序号**

-   你要做的事情：转账、调用合约、投票等
    
-   手续费:为网络资源付费，提供激励，可以自主提高手续费来换取优先处理
    
-   防重放序号Nonce：随机数，可以避免同一笔交易被重复执行
    

### 数字签名

这里使用现代密码学教程中对于数字签名的原理部分来帮助理解

![{4FBC5BCD-9FB7-4EA5-A200-A1F9BD776EEE}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768311184008-_4FBC5BCD-9FB7-4EA5-A200-A1F9BD776EEE_.png)![{BF5E3EA1-5600-4460-8FBD-285925A6DFBB}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-13-1768311240617-_BF5E3EA1-5600-4460-8FBD-285925A6DFBB_.png)

## 区块链网络运行

### [交易的完整生命周期](https://txcity.io/v/eth-btc)：

-   Wallet（签名）
    
-   RPC/Node（传播）
    
-   Mempool（排队）
    
-   Builder（挑选）
    
-   Block（落盘）
    
-   Explorer（可查）
    

**为什么“不可篡改需要时间”？**

| 为什么要等确认？区块会引用上一块hash区块越往后叠，推翻的成本越高PoS下12-13分钟达到更稳定的最终状态 |
| --- |

### 钱包、RPC、节点、网络：如何链接

| 关键关系：钱包连一个RPC（访问节点的API）RPC背后是节点/节点集群RPC往往是“中心化入口风险点” |

用[Chainlist](https://chainlist.org/)查看/添加网络

## 智能合约

_本质：区块链账本里的“可执行代码”_

社会学意义：

-   规则可验证、按代码执行
    
-   部署后难以更改
    
-   减少中介与摩擦
    

## 区块链协议如何升级：EIP和节点客户端多样性

## Web3总结

**web3是跨学科的领域**

-   社会学：去中心化治理与共识形成
    
-   经济学：激励设计保证安全与可持续（Gas、质押、惩罚）
    
-   密码学：签名、哈希、ZK提供可信基础
    

| 技术层面：用私钥签名证明你是谁用共识网络保证账本可信用智能合约让规则自动执行 |
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->















































# 开营仪式

一开始很紧张，但正在努力跟上大家。前面听这个实习计划的介绍感觉对我这个p人非常友好又能推着我前进。我个人是学过一点solidity的，看到入门技术可以帮助建立认识并且回忆基础非常开心。希望学有余力的情况下可以在深度技术的边缘试探。目前是对黑客松了解不多但是听到成员们反复提到也有很大的兴趣。后面的破冰真是天地之大，我之渺小，群内的职业跨度和地区、年龄跨度都远超我的想象，也更说明了web3是给了所有人一个机会。紧张但是喜欢这样的环境，感觉行动起来了。

# Unphishable 钓鱼攻防

钓鱼攻击大家并不陌生，你点击了一个假冒网站或者链接，然后个人信息被盗取，银行卡等被盗刷，这种犯罪手段为大家所熟知。它依赖于一个核心逻辑：获取你的信任，然后让你主动交出敏感信息。到了区块链领域，网络钓鱼攻击演化出更危险的形式，攻击者也更加针对你来设置骗局，他们化身各种角色：有时是"技术支持"告诉你钱包需要升级，有时是"客服"承诺帮你找回丢失的资产。而当你把助记词发送或者存储在云端被盗取时，你的数字资产就像打开了闸门的洪水，瞬间流向攻击者的钱包，且永远无法追回。

## 0x0002：助记词恢复骗局

-   助记词可以恢复私钥，而私钥与money挂钩
    
-   助记词不云端存储，写在纸上
    
-   **不泄露给任何人**
    
-   使用硬件钱包
    

## 0x0003：骗取签名

啥也没说，权限全给，金额无限，倾家荡产来的，太危险了弹出来我就想点。

-   检查许可证类型并了解授权范围
    
-   验证付款人地址是否来自可信来源
    
-   请注意授权金额，谨防无限制授权。
    
-   确认来源
    

# 尝试测试币转账

领取测试币方面，使用下来对于新手最友好的是[Sepolia Pow Faucet](https://sepolia-faucet.pk910.de/)

1.  测试币顾名思义只是为了测试，为使用者提供便利，方便做合约部署或者钱包功能之间的测试。领水大部分要求用户在主网有0.01ETH，但是初始的买入又没办法购买过少，这时不需要有有初始的主网余额，会更方便初学者入门参与到测试币转账当中省时又省力。
    
2.  水龙头由于免费分发测试币，很多设置了上限，就比如在[Ethereum Sepolia Faucet](https://cloud.google.com/application/web3/faucet/ethereum/sepolia)上对于sepolia测试币，官方上限0.05ETH，好奇的东西很多又不够使用。
    

综上sepolia绝对是新手无脑可以选择的领水网站。

# Web3 实习手册入门导读部分

基础知识简单略过，记一下这几个FAQ

-   \*\*比特币有什么价值？\*\*它具有货币属性。不通涨。区块链特性使得它账户匿名、交易公开透明、不可篡改、完全在虚拟网络中，相比传统跨境转账结算更方便。
    
-   \*\*区块链如何实现交易公开透明同时保护隐私并且匿名？\*\*在整个网络中，你的交易通过一个随机生成的钱包地址完成，与这个地址相关的交易等在网络上全部是公开透明的。别人不知道你和这个钱包有关联所以是匿名。
    

# 021 学习以太坊第 1 章

## EIP-4844与Dencun升级

-   引进携带blob的交易类型，为Rollup提供临时数据空间
    
-   blob数据只在共识层保存，不进入永久状态
    
-   L2发布数据的成本大幅下降（今天听中文周会时候对此产生了疑惑，好像L1主网现在也没有很高的成本了。Fusaka升级增加了blob数，更多的blob意味着更复杂的数据传输和验证逻辑。）
    
-   初期最多每个区块6个blob，现在从9个到48个（目标值）涉及到PeerDas技术（中文周会有讲，目前了解不深）
    

## POS机制下ETH在共识层定位

1.  **质押资产**：（质押：存入32个以太币激活验证者软件的行为）质押后更加安全更加可持续。
    
2.  **获得奖励**：共识层奖励和执行层奖励
    
3.  **提供经济安全**：保证金
    

## dApp开发的缺点

-   **维护困难**
    
-   **拓展困难、性能开销**
    
-   **网络拥堵，gas成本**
    
-   **再中心化倾向**：有些看起来是dapp的产品会把前端放在传统CDN上，链下算关键逻辑，只把结果写到链上。重新引入单点新人和审查风险
    

## 以太坊到底能不能无限制参与

| 以太坊的网络结构=理论上对任何人开放（permissionless），现实中受限于硬件和带宽，但通过L2+数据分片等方案，把更多人参与和系统还能跑得动两件事尽量同时做到。 |
| --- |

# 我的第一个NFT

回顾基础知识，逐步熟悉了铸造流程，极简风=和我一样脑袋空空

![{D1AB2FE1-20A6-4F6E-9195-B7C4661FA1AF}.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/riwev/images/2026-01-12-1768215828333-_D1AB2FE1-20A6-4F6E-9195-B7C4661FA1AF_.png)

# 以太坊中文周会

-   以太坊fusaka升级，blob上限提升。
    
-   web3迈入下半场：在合规边界内，以稳定币和代币化资产为载体，将链上技术能力嵌入真实的支付、清算和资本市场流程，并最终被主流资金入口与分发渠道接受。
    
-   重点赛道预测：RWA、永续合约、web3企业链
    
-   监管方面：不在游离在监管之外，而是逐步被纳入传统金融市场体系，受到宏观经济政策、宏观流动性以及地理因素的共同影响
    

老实说这一周在这一部分学到的东西很少，老师很专业讲的很快，大部分听的一知半解，希望下一周的这部分有进步。

# Web3行业全局介绍&岗位概览

-   **行业情况**：行业蛋糕非常大。初级人员过剩，精英人员短缺。
    
-   **面试方面**：70%的面试会考察AI工具使用能力和链上交互记录。不要学的太浅，会被ai代替。先积累自己的作品作为敲门砖。
    
-   **渠道选择**：普通的传统招聘网站覆盖率很低，web3有一些垂直的招聘平台（smartdeer，web3career，remote3等）
    

社交网络：X，领英，discord

隐形渠道：Github Contribution，DAO治理提案

-   **岗位需求**：技术>项目管理>运营/市场>投研/合规/审计
    
-   **技能要求**：开发：Solidity 、Rust、Move、Cario以及安全意识；运营/市场：口语、Tokenomics；产品/投研：懂底层逻辑
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
