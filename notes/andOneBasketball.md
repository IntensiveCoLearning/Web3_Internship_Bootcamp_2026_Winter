---
timezone: UTC+8
---

# 0xSevenK

**GitHub ID:** andOneBasketball

**Telegram:** @HaroldKKK

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
# 📚 共学营 Day 14 学习笔记（Ethernaut 14–16）

今天通关并实操了 Ethernaut 的 **14–GatekeeperTwo、15–NaughtCoin、16–Preservation**，核心围绕三个主题：  
👉 构造函数阶段的 EVM 特性、ERC20 逻辑限制绕过、delegatecall 与 storage 布局错位漏洞。

* * *

## 🧩 Level 14 — GatekeeperTwo

### 🎯 关卡目标

通过三个 gate，其中最关键的是：

-   `gateTwo`：`extcodesize(caller()) == 0`
    
-   `gateThree`：对 `msg.sender` 进行 `keccak256` 后再与 `_gateKey` 做异或校验
    

* * *

### ✅ 核心突破点

✅ 构造函数阶段 extcodesize == 0

在合约构造函数执行期间：

```
代码尚未存储到链上 → extcodesize(address(this)) == 0
```

因此必须在 **constructor 中完成攻击调用**。

这是一个非常经典的 EVM 行为特性：

> 合约部署中 ≠ 合约已存在

* * *

✅ gateKey 构造方式

条件本质是：

```
uint64(bytes8(keccak256(msg.sender))) ^ uint64(_gateKey) == type(uint64).max
```

因此：

```
_gateKey = bytes8(uint64(keccak256(msg.sender)) ^ type(uint64).max)
```

即：

> 与 hash 结果逐位取反即可

* * *

### 🧠 收获总结

-   constructor 是一个**特殊执行阶段**
    
-   可用于绕过：
    
    -   extcodesize
        
    -   isContract 检测
        
-   现实中很多反机器人合约曾被此方式绕过
    

* * *

## 🧩 Level 15 — NaughtCoin

### 🎯 关卡目标

合约表面限制：

```
transfer() 在锁定期内不可用
```

但本质是一个 ERC20。

* * *

### ✅ 核心突破点

虽然不能：

```
transfer(to, amount) ❌
```

但可以：

```
approve(attacker, amount)
transferFrom(player, attacker, amount) ✅
```

因为：

-   时间锁只限制了 `transfer`
    
-   没有限制 `approve / transferFrom`
    

* * *

### 🧠 收获总结

-   ERC20 是一组函数接口，不是只看 transfer
    
-   只封一个入口 ≠ 资产安全
    
-   现实项目中必须：
    
    -   全路径资产流转控制
        
    -   考虑 allowance 间接转移
        

这是非常典型的：

> 业务规则绕过型漏洞

* * *

## 🧩 Level 16 — Preservation

### 🎯 关卡目标

通过 delegatecall 修改合约 owner。

* * *

### ⚠️ 漏洞本质

原合约设计：

-   存了两个 library 地址
    
-   使用 delegatecall 调用 library 的 `setTime`
    

delegatecall 特性：

> 执行 library 代码  
> 使用 Preservation 合约的 storage

* * *

### ✅ 攻击两步走

✅ Step 1：覆盖 library 地址

第一次调用：

```
setFirstTime(uint256(address(attackerLib)))
```

因为 storage layout 对齐：

```
slot0 → timeZone1Library
```

成功把 library 地址换成攻击者合约。

* * *

✅ Step 2：delegatecall 到恶意实现

第二次调用：

```
setFirstTime(any)
```

此时 delegatecall 到攻击者库：

```
owner = tx.origin;
```

由于仍在 Preservation storage 上执行：

```
slot2 → owner 被成功覆盖
```

* * *

### 🧠 收获总结

-   delegatecall = 代码复用 + storage 共享
    
-   一旦 library 地址可变 = 完全接管权限
    
-   storage layout 是升级合约的生命线
    

这也是为什么现在主流升级方案都使用：

-   EIP-1967 固定 slot
    
-   OpenZeppelin upgradeable 模板
    
-   严格 storage append-only 规则
    

* * *

## ✅ 今日整体总结

| 关卡 | 核心主题 |
| --- | --- |
| GatekeeperTwo | 构造函数阶段的 EVM 特性 |
| NaughtCoin | ERC20 业务逻辑绕过 |
| Preservation | delegatecall + storage 布局错位 |

今天的三个关卡非常工程化，全部都是真实世界出现过的漏洞模式：

-   反合约检测失效
    
-   接口级绕过转账限制
    
-   升级机制导致权限接管
    

对理解 **DeFi 合约真实攻击路径** 非常有帮助。
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

# 📚 共学营 Day 12 学习笔记（Ethernaut 10–13）

今天完成了 Ethernaut 的 Level 10 到 Level 13，通过实战进一步加深了对合约安全问题和 EVM 执行机制的理解，覆盖了重入攻击、外部合约信任问题、链上隐私误区以及 gas 精准控制等关键知识点。

* * *

## ✅ Level 10 — Re-entrancy

**核心问题：**  
合约在转账前未先更新用户余额，导致可重入调用。

**攻击方式：**  
攻击合约在 `receive / fallback` 中再次调用 `withdraw`，在余额尚未更新前反复提现，直到目标合约余额耗尽。

**学习要点：**

-   必须遵循 **Checks → Effects → Interactions** 编程模式
    
-   外部调用放在状态更新之后
    
-   可使用 `ReentrancyGuard` 防止重入
    

* * *

## ✅ Level 11 — Elevator

**核心问题：**  
合约信任了外部合约返回的逻辑判断结果。

**攻击方式：**  
攻击合约在 `isLastFloor()` 中通过内部状态控制：

-   第一次返回 `false`
    
-   第二次返回 `true`  
    欺骗目标合约设置 `top = true`。
    

**学习要点：**

-   外部合约的 view 函数同样不可信
    
-   同一函数在一笔交易中可能被多次调用
    
-   不应将关键状态判断依赖外部合约
    

* * *

## ✅ Level 12 — Privacy

**核心问题：**  
误以为 `private` 变量在链上不可读取。

**攻击方式：**  
通过 RPC 直接读取 storage slot，提取对应 bytes 数据并构造解锁 key。

**学习要点：**

-   区块链上不存在真正的隐私
    
-   `private` 只是 Solidity 级别访问限制
    
-   敏感数据应避免上链或进行加密/哈希处理
    

* * *

## ✅ Level 13 — GatekeeperOne

**核心问题：**  
通过多重 gate 条件限制调用者和执行路径，其中包含对 `gasleft()` 的精确校验。

**难点分析：**

-   `gasleft() % 8191 == 0` 对 opcode 执行路径极其敏感
    
-   internal call 失败通常只显示 `revert without reason`，难以定位失败原因
    

**优化后的攻击方案：**

-   不在 TS 中多次发送交易 brute force gas
    
-   改为在攻击合约内通过循环 + `call{gas: ...}` 尝试不同 gas offset
    
-   一笔交易内完成多次尝试，提高成功率
    

**学习要点：**

-   gas 消耗与具体执行路径强相关
    
-   有些校验机制本质是反自动化设计
    
-   brute force 不一定非要在链下完成
    

* * *

## ✅ 今日整体收获

通过这四关的练习，进一步认识到：

-   合约安全问题不仅是语法或权限问题，更是**执行模型和业务假设的问题**
    
-   链上数据默认全部公开，隐私必须在设计层面解决
    
-   gas 不只是费用参数，也可能参与业务逻辑判断
    

对 EVM 执行过程和合约安全边界有了更立体的理解。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


## 📚 共学营 Day 11 学习笔记（Ethernaut 6–9）

### ✅ 6. Delegation —— delegatecall 与存储上下文

-   使用 `delegatecall` 时：
    
    -   执行的是被调用合约的代码
        
    -   但 **读写的是当前合约的 storage**
        
-   如果代理合约 fallback 直接 delegatecall 给逻辑合约：
    
    -   任何人都可以调用逻辑合约中的函数
        
    -   等价于拥有了代理合约的权限
        
-   核心认知：
    
    -   delegatecall 是升级合约基础
        
    -   也是权限漏洞高发点
        

👉 启示：代理合约必须做函数选择器白名单或权限控制。

* * *

### ✅ 7. Force —— selfdestruct 强制转账

-   `selfdestruct(target)` 可以：
    
    -   无视 target 的 payable / fallback
        
    -   直接修改 target.balance
        
-   合约无法阻止别人给自己打钱
    
-   因此：
    
    -   不能用 `address(this).balance` 作为业务状态
        
    -   必须自己在 storage 中记账
        

👉 启示：链上余额是外部可变状态，不可信。

* * *

### ✅ 8. Vault —— private ≠ secret

-   `private` 只是 Solidity 语法级限制
    
-   链上 storage 对所有人可读
    
-   只要知道 slot 位置就能直接读出变量
    

本关 storage 布局：

-   slot0：bool locked
    
-   slot1：bytes32 password
    

通过 RPC 直接读 slot1 即可解锁。

👉 启示：  
链上永远不存在真正意义上的“密码”，只能做哈希校验。

* * *

### ✅ 9. King —— transfer 导致的 DoS 攻击

-   合约逻辑依赖：
    

```
payable(king).transfer(msg.value);
```

-   transfer 特点：
    
    -   只给 2300 gas
        
    -   失败直接 revert
        

攻击方式：

-   让一个无法接收 ETH 的合约成为 king
    
-   后续所有挑战都会在 transfer 时失败
    
-   游戏被永久锁死（拒绝服务）
    

👉 启示：

-   不要在状态更新前做外部转账
    
-   应使用 pull-payment 模式（用户自己提钱）
    

* * *

## 💡 今日额外反思

-   King 关卡原本通关合约里写死了 `1 ETH`，  
    实际只要 **大于当前 prize 即可**，改小额度就行。
    
-   说明：
    
    -   攻击思路比具体金额更重要
        
    -   不要被示例代码限制住思路
        

* * *

## 🎯 今日核心能力提升

今天几关集中强化了三个关键底层认知：

1.  **EVM 执行上下文与 storage 归属（delegatecall）**
    
2.  **账户余额不可信（selfdestruct 强制注资）**
    
3.  **外部调用不可控（transfer DoS）**
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



## 📒 共学营 Day 10 学习笔记

今天继续复盘 Ethernaut 关卡学习，并总结了 Solidity、Hardhat 与 ethers 的实践经验：

-   **接口与函数可见性**：在 `interface` 中只能声明 `external` 函数，即便原合约函数是 `public`，接口中也必须标注 `external`。
    
-   **ethers v6 合约部署与调用**：必须使用 `waitForDeployment()` 来等待合约完成部署，否则后续调用会失败。使用 `waitNextBlock` 可以区分本地链和线上链，确保部署与调用不会被打包在同一个区块导致调用失败。
    
-   **整数溢出漏洞**：Solidity 0.7.x 及以下版本存在整数溢出/下溢漏洞，需要注意旧版本合约中 `uint` 运算可能导致余额异常或逻辑绕过。
    
-   **Hardhat 命名账户**：回顾了 `hardhat.config.js` 中 `namedAccounts` 的配置，并理解了 `getNamedAccounts()` 如何根据网络和 index 返回对应地址或 signer，便于跨网络部署和测试。
    
-   **Ethernaut 实战关卡**：完成了 2-Fallout、3-CoinFlip、4-Telephone、5-Token 关卡的练习，学习到：
    
    -   **Fallout**：合约构造函数未正确命名导致可被攻击者任意初始化。在 Solidity 0.4.x 及之前版本，构造函数名称必须与合约名称相同，否则会被当作普通 `public` 函数暴露出来。Solidity 0.4.22+ 引入了统一 `constructor` 关键字，从此避免了构造函数命名错误导致的初始化漏洞。
        
    -   **CoinFlip**：利用区块哈希预测合约内随机数逻辑，并结合本地链时间/区块控制完成攻击。
        
    -   **Telephone**：理解了 `tx.origin` 与 `msg.sender` 的区别，以及如何通过代理调用修改拥有者。
        
    -   **Token**：观察 ERC20 转账函数中整数下溢可能导致余额覆盖，巩固了整数溢出/下溢漏洞的实战场景。
        

今天的学习将 Solidity 的安全漏洞、Hardhat 测试实践和 ethers v6 合约调用结合起来，提升了对链上逻辑和测试环境差异的理解。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




# **📒 共学营 Day 9 学习笔记**

**主题**：Ethernaut 合约学习与历史项目排查

## 今日收获

1.  **Hello Ethernaut 关卡完成**
    
    -   今天顺利通过了 Ethernaut 的 `Hello Ethernaut` 关卡。
        
    -   复习了关卡合约的基本结构和事件触发逻辑，加深了对 Solidity 中 `constructor`、`msg.sender` 与事件触发机制的理解。
        
    -   实践中对 Remix 部署流程和合约交互有了更熟练的操作体验。
        
2.  **历史 Ethernaut 项目排查**
    
    -   找到了一个 5 年前的开源项目：[https://github.com/MrToph/ethernaut](https://github.com/MrToph/ethernaut)。
        
    -   在尝试运行和交互时发现，官方 Ethernaut 合约已经发生了更新，导致：
        
        -   合约 ABI 与事件 (`event`) 定义不完全一致。
            
        -   老项目中监听的事件可能无法正常捕获，或者数据字段顺序/名称发生变化。
            
    -   通过对比历史 ABI 与最新合约，学习了如何调整本地脚本以匹配新版本事件，巩固了对 Solidity `event` 机制的理解。
        
3.  **实践技巧**
    
    -   当官方合约更新时，历史项目可能会出现 `event` 或 `function` 不匹配的问题。
        
    -   解决方法包括：
        
        1.  拉取最新合约 ABI。
            
        2.  对比旧事件和新事件的字段，调整监听逻辑。
            
        3.  使用 Remix 或 Hardhat 测试部署新合约，确保交互正常。
            
    -   这一过程帮助理解 Solidity 事件、ABI 编码及前端监听机制之间的关系。
        

## 遇到问题与思考

-   **问题**：老项目的事件监听脚本报错或没有触发。
    
-   **分析**：事件定义或 ABI 与当前合约版本不一致。
    
-   **解决方案**：
    
    -   使用官方最新合约 ABI 替换老项目 ABI。
        
    -   调整事件监听逻辑，确保字段对应正确。
        
-   **思考**：
    
    -   区块链项目的前后端/脚本高度依赖 ABI，版本管理很重要。
        
    -   历史项目排查也是学习 Solidity、事件机制和合约更新兼容性的好方法。
        

## 明日计划

-   尝试其他 Ethernaut 基础关卡，如 `Fallback` 或 `Vault`，加深对以太坊存储和访问控制机制的理解。
    
-   继续整理老项目排查经验，尝试写一套 ABI 版本兼容工具或脚本。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->





# 📒 共学营 Day 8 学习笔记

今天继续复盘 Uniswap V2 面试题，从第 9 题到第 12 题，主要聚焦 **LP Token 机制、Flash Swap、重入防护、TWAP 价格安全设计**。

* * *

## Q9：LP Token 的本质与流动性份额计算

**问题：**  
Uniswap V2 中 LP Token 的本质是什么？它如何表示你在池子中所占份额？移除流动性时如何计算可取回的两种代币数量？

**回答：**

-   LP Token 本质是 **提供两种代币流动性的凭证**，代表在池子中的份额。
    
-   拥有的 LP Token 数量 ÷ 池子总 LP Token 数量 = 在池子中的占比。
    
-   移除流动性时，可取回代币数量 = 占比 × 当前池子代币余额。
    
-   实际代码中 `_mintFee` 会根据 `√k` 的增长计算协议费，LP Token 的总量和占比会影响可取回数量。
    

**补充说明：**

-   `kLast` 用于记录上次的 reserve 乘积，协议费只在 `feeTo` 地址非空时生效。
    
-   协议抽取 LP 手续费比例为 **1/6 √k 增长**，等价于总交易额的 0.05%，其余归 LP。
    

* * *

## Q10：重入攻击与 lock 机制

**问题：**  
为什么在 swap / mint / burn 中使用 `lock`？仅靠 lock 是否能完全防止所有重入风险？

**回答：**

-   lock 用于 **防止同一 Pair 合约内的函数重入**，例如 swap → swap、swap → mint。
    
-   攻击者可能在回调中尝试再次进入核心函数，利用中间状态套利。
    
-   lock 不能防止跨合约调用或 token 回调（ERC777/fee-on-transfer）产生的重入。
    
-   Uniswap V2 的安全依赖多层防御：
    
    -   `lock` 控制流安全
        
    -   balance 差值计算保证资产真实性
        
    -   invariant 校验保证数学模型不被破坏
        

* * *

## Q11：为什么用 balance 差值而不是 amountIn

**问题：**  
为什么协议在 mint / swap / burn 时，不直接使用用户传入的 `amountIn`，而要用 `balanceOf(pair) - reserve` 计算实际输入？

**回答：**

-   用户传入的 `amountIn` 可能与实际到账量不一致，尤其是 fee-on-transfer 或 ERC777 代币。
    
-   `balanceOf(pair) - reserve` 计算实际资产变化，保证交易逻辑基于真实流入资产。
    
-   这样可防止攻击者利用 token 回调或手续费设计绕过恒定乘积约束。
    

* * *

## Q12：TWAP 的作用与 MEV 区分

**问题：**  
为什么 Uniswap V2 使用 TWAP（时间加权平均价格）而不是直接使用当前价格？TWAP 是否能防止 MEV 攻击？

**回答：**

-   TWAP 用于防止依赖价格的协议被瞬时大额交易操纵，例如闪电贷攻击。
    
-   通过累积一段时间的价格，平滑短时波动，降低借贷、清算等敏感操作风险。
    
-   TWAP **不能防止 MEV** 或交易被前置，它只保护 **协议引用的价格安全**。
    
-   MEV 需要额外机制（交易排序策略、Flashbots 等）来防护。
    

* * *

💡 **学习感悟：**  
今天复盘 Q9~Q12，加深了对 Uniswap V2 **LP Token、Flash Swap、重入防护与价格安全设计**的理解。  
尤其是 lock + balance 校验 + invariant 的多层防御逻辑，让我对 **安全意识内化到工程实践**的理念有更直观的认识。  
对于 Web3 开发者来说，理解这些设计不仅是面试题，更是编写安全、高可用 DeFi 协议的基础。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->






# 📒 共学营 Day 5 学习笔记

今天继续让 ChatGPT 模拟面试官，对 **Uniswap V2** 进行面试式复盘，重点学习了 **Flash Swap 的执行机制、安全保护设计，以及 TWAP 价格累计器的源码实现方式**。整体感觉是：Uniswap V2 的设计是多层机制叠加形成安全闭环，对理解 DeFi 可组合性和抗攻击设计很有启发。

* * *

## ✅ Q6：Uniswap V2 的 Flash Swap 是什么？如何实现“先拿钱、后还钱”？

### 一、Flash Swap 的本质

-   允许用户在一次交易中先从池子拿走代币
    
-   在回调中完成套利或资金操作
    
-   交易结束前归还资产 + 手续费
    
-   不满足池子约束则整体回滚
    

### 二、触发方式

-   通过 `UniswapV2Pair.swap(amount0Out, amount1Out, to, data)`
    
-   当 `data.length > 0` 时，调用接收方合约 `uniswapV2Call` 回调
    

### 三、为什么可以先转币再校验

1.  transfer token 给接收方
    
2.  执行回调函数
    
3.  读取最终余额
    
4.  校验恒定乘积 + 手续费
    

-   校验在回调后发生，因此可以先使用资金
    

### 四、必须归还多少资产

-   归还 = 借出数量 + 0.3% 手续费
    
-   swap 末尾校验恒定乘积，不满足则 revert
    

### 五、常见用途

-   套利
    
-   抵押借贷清算
    
-   杠杆头寸构建
    
-   跨协议资金迁移
    

* * *

## ✅ Q7：Uniswap V2 如何防止 Flash Swap 回调中的重入攻击？

### 一、函数级重入保护

-   `lock` 修饰符保护 swap、mint、burn、skim、sync
    
-   一次调用未结束前无法再次进入
    
-   Flash Swap 回调中无法重入调用 swap
    

### 二、资金层面校验

-   swap 末尾做恒定乘积 + 手续费校验
    
-   不满足条件直接 revert，保证池子资产安全
    

### 三、双重保险机制

-   lock 锁：防止函数重入
    
-   最终校验：防止套利型攻击
    
-   两者结合保证 Flash Swap 可组合但安全
    

### 四、链上纠偏注意

-   如果项目方单独 transfer 再 sync 可能被抢跑
    
-   正确做法：在一个合约函数里同时 transfer + sync 原子操作
    

* * *

## ✅ Q8：Uniswap V2 的 TWAP 是如何实现的？为什么需要价格累计器？

### 一、为什么不能用即时价格

-   spot price = reserve1 / reserve0
    
-   易被闪电贷操纵
    
-   若用于抵押或清算可能被攻击
    

### 二、价格累计器核心变量

-   `price0CumulativeLast`、`price1CumulativeLast`
    
-   `blockTimestampLast`
    
-   含义：价格 × 时间 的累计积分值
    

### 三、累计方式（源码逻辑）

```
price0CumulativeLast += (reserve1 / reserve0) * timeElapsed
price1CumulativeLast += (reserve0 / reserve1) * timeElapsed
```

-   每次状态更新（swap/mint/burn/sync）时累加
    
-   使用的是**上一次 reserve**
    

### 四、TWAP 计算方式

-   读取两个时间点的 cumulative：P1, P2
    
-   计算公式：`TWAP = (P2 - P1) / (T2 - T1)`
    
-   即时间区间内平均价格
    

### 五、抗操纵原理

-   攻击者必须在整个时间窗口持续维持异常价格
    
-   成本高且易被套利者拉回
    
-   避免了使用瞬时价格被闪电贷操纵
    

* * *

## 📌 今日总结

-   今天的学习重点是协议的安全与可组合性设计
    
-   Flash Swap 的原子性 + lock 重入保护
    
-   TWAP 的时间积分机制
    
-   这些都是 DeFi 可组合操作的核心安全保障
    
-   对自己未来做链上系统和金融类合约设计非常有借鉴意义
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->







# 📒 共学营 Day 5 学习笔记

今天我继续让 ChatGPT 模拟面试官，针对 **Uniswap V2 的高级机制** 进行了面试式问答训练。重点关注 **流动性添加规则、AMM 定价以及 Pair 合约的状态同步机制**。通过答题 + 讲解，我对协议设计与安全逻辑有了更深入的理解。

* * *

## 1️⃣ 面试题三：池子已存在时如何添加流动性？比例不一致会发生什么？

### 问题回顾

当交易对池子已经存在时，用户再次添加流动性，需要按照什么比例存入两种 Token？如果用户提供的数量比例不一致，会发生什么？Router 和 Pair 各自负责什么逻辑？

### 核心点

-   必须按照当前池子储备比例添加：`amountA / amountB = reserveA / reserveB`
    
-   Router 会计算最优投入量，以数量较小的一侧为准，多余的 Token 会退回给用户
    
-   Pair 合约只根据实际转入数量铸造 LP Token：
    
    ```
    liquidity = min(amountA * totalSupply / reserveA, amountB * totalSupply / reserveB)
    ```
    
-   Router 负责比例计算和退币，Pair 只负责铸币
    

### 学习收获

-   理解 Router 与 Pair 的职责边界
    
-   明白比例不对不会报错，而是自动按最小量匹配，多余 Token 会退回
    

* * *

## 2️⃣ 面试题四：Uniswap V2 的价格是如何计算和维持的？

### 问题回顾

Uniswap V2 中价格是如何计算的？`x * y = k` 在合约中是如何被保证的？

### 核心点

-   AMM 采用常数乘积模型 `x * y = k`
    
-   价格由储备比例决定：
    
    ```
    priceA = reserveB / reserveA
    priceB = reserveA / reserveB
    ```
    
-   交易时扣除 0.3% 手续费：`dx' = dx * 997 / 1000`
    
-   输出数量公式：`dy = y * dx' / (x + dx')`
    
-   手续费留在池子里，每次交易后 k 实际增加，这部分增长就是 LP 收益来源
    

### 学习收获

-   理解公式背后的逻辑，不仅仅是记公式
    
-   掌握交易与 LP 收益的关系
    

* * *

## 3️⃣ 面试题五：为什么需要 skim() 和 sync()？

### 问题回顾

UniswapV2Pair 合约中存在 `skim()` 和 `sync()` 函数，但在 Router 中并不会调用，它们是做什么用的？有没有安全风险？

### 核心点

-   `reserve` 是内部记账值，`balance` 是链上真实余额，两者可能不一致
    
-   `skim()`: 把多出来但未计入 reserve 的 Token 转走，用于回收误转资金或套利修正
    
-   `sync()`: 用真实余额更新 reserve，用于项目方注资或纠偏价格
    
-   安全提示：如果 transfer 与 sync 分开两笔交易，中间可能被别人调用 skim 抢走资金
    
-   正确做法：通过管理合约在同一笔交易内原子执行 transfer + sync
    

### 学习收获

-   理解 Pair 的记账机制与链上状态的差异
    
-   认识到原子性在 DeFi 中的重要性
    
-   掌握异常纠偏和资金安全的操作思路
    

* * *

## 4️⃣ 今日总结

-   理解了 Router 与 Pair 的职责分工
    
-   熟悉 AMM 定价与手续费对 LP 收益的作用
    
-   理解 reserve 与真实余额差异可能带来的安全问题
    
-   掌握原子操作在池子注资和纠偏中的重要性
    

今天的面试练习让我对 Uniswap V2 的核心机制和安全边界有了更深入的理解，也为后续学习 Flash Swap 和套利机制打下了基础。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->








# 📒 共学营 Day 4 学习笔记

今天我让 ChatGPT 来模拟面试官，针对 **Uniswap V2** 对我进行了一轮面试式提问，通过答题 + 讲解的方式复盘了协议的核心设计，收获还是挺大的，主要集中在 **协议合约结构** 和 **LP Token 机制** 两个问题上。

* * *

## ✅ 面试题一：Uniswap V2 的核心合约有哪些？各自职责是什么？

### 🧩 合约分层结构

Uniswap V2 的合约整体分为 **Core（协议核心层）** 和 **Periphery（外围交互层）** 两部分。

### 🔹 Core 层

-   **UniswapV2Factory**
    
    -   负责创建和管理所有交易对（Pair）
        
    -   通过 `createPair(tokenA, tokenB)` 使用 `CREATE2` 部署 Pair
        
    -   保证任意代币对只会存在一个池子
        
-   **UniswapV2Pair**
    
    -   真正持有资金池资产的合约
        
    -   内部实现 `mint / burn / swap / sync / skim`
        
    -   所有交易和流动性变化都发生在 Pair 中
        
-   **UniswapV2ERC20**
    
    -   Pair 继承的 ERC20 实现
        
    -   用于实现 LP Token
        
    -   支持 `permit`（EIP-2612），方便无 gas 授权
        

### 🔹 Periphery 层

-   **UniswapV2Router02**
    
    -   面向用户的统一交互入口
        
    -   提供添加/移除流动性接口
        
    -   封装多跳 swap 路径
        
    -   处理 ETH 与 WETH 的转换逻辑
        

📌 总结：Router 只负责调度和路径计算，**不托管资产**，真正的资产始终在 Pair 合约中。

* * *

## ✅ 面试题二：LP Token 有什么作用？数量是如何计算的？

### 🎯 LP Token 的作用

-   LP Token 是流动性提供者在池子中的**份额凭证**
    
-   代表持有者在池中拥有的资产比例
    
-   可通过 `burn()` 随时赎回对应比例的两种代币
    
-   本身是 ERC20，可转让、可组合进其他 DeFi 协议
    

* * *

### 📐 LP Token 的计算方式

1️⃣ 第一次添加流动性（初始化池子）

```
liquidity = sqrt(amountA * amountB) - MINIMUM_LIQUIDITY
```

-   `MINIMUM_LIQUIDITY = 1000` 会被永久锁定
    
-   用于防止池子被完全抽空以及除零等边界问题
    

* * *

2️⃣ 后续添加流动性（池子已存在）

```
liquidity = min(
  amountA * totalSupply / reserveA,
  amountB * totalSupply / reserveB
)
```

-   按资产在池子中的**占比**计算 LP Token
    
-   取 `min` 是为了保持当前价格比例不被破坏
    
-   多余的一侧资产会被退回给用户
    

* * *

### 🧠 核心理解

> 后续加池不是按投入的绝对数量算，而是按能提供多少“等比例流动性份额”来算。

* * *

今天通过面试式复盘的方式，把 Uniswap V2 的基础结构和 LP 机制又系统过了一遍，也发现一些以前只是记结论、但没认真推过公式的地方，后面打算继续往 swap 过程和价格累计机制方向深入。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->









# 📝 Uniswap V2 学习记录（实习第 3 天）

今天主要复习了 Uniswap V2 的整体架构与核心交易机制，加深了对 AMM 型 DEX 工作原理的理解。

## 一、整体架构

Uniswap V2 主要由三部分组成：

-   **Factory**：负责创建交易对 Pair，并记录 token 对应的 Pair 地址
    
-   **Pair**：真正存资金并执行 AMM 逻辑的合约，每个 Pair 管理一对 Token
    
-   **Router**：用户交互入口，负责路径计算、转账和调用 Pair
    

三者职责清晰：Factory 管创建，Pair 管资金和定价，Router 管用户体验。

* * *

## 二、定价机制（AMM）

核心公式是恒定乘积：

> x · y = k

交易时会收取 0.3% 手续费，实际参与定价的是：

> amountIn × 0.997

交易会改变池子中两种资产比例，因此产生**滑点**。

* * *

## 三、流动性与 LP

-   添加流动性会获得 **LP Token**，代表池子份额
    
-   移除流动性时销毁 LP Token，按比例取回两种资产
    
-   手续费不会直接分发，而是体现在池子余额增长中
    

* * *

## 四、Router01 与 Router02 的区别

Router01 假设：

> 转进 Pair 的数量 = 用户实际输入数量

但很多代币存在转账扣税机制，导致 Pair 实际收到更少 token，从而 swap 失败。

Router02 新增了：

-   `SupportingFeeOnTransferTokens` 系列函数
    
-   通过 **读取 Pair 实际余额变化** 来计算真实输入量
    

因此 Router02 能兼容扣税币，现在主流 DEX 基本都使用 Router02。

* * *

## 五、设计特点

-   Pair 使用 CREATE2 部署，地址可预测
    
-   价格计算逻辑集中在 Library，链上链下结果一致
    
-   合约拆分清晰，便于二次开发和组合创新
    

* * *

## 六、个人理解

Uniswap V2 的核心在于：

-   Pair 极度稳定，几乎不改动
    
-   创新主要发生在 Router 和外围激励机制
    
-   非常适合作为 DEX 二开和 GameFi 资产交易的基础模块
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->










# 📒 共学营 Day 2 学习笔记｜以太坊的去中心化是如何被“工程化”的

今天在《021 学习以太坊》中了解到一个非常震撼的事实：  
**以太坊是一个拥有百万级验证者参与的全球公链系统。**  
如此规模的分布式节点还能保持同步与安全，本身就是一项极其复杂的系统工程。

更让我佩服的是：  
以太坊不仅在机制上追求去中心化，**在实现方式上也主动对抗单点风险**，比如通过多客户端来摊平单一实现 Bug 带来的系统性风险。  
这意味着更高的研发成本和维护复杂度，但也换来了更强的系统韧性。

逐步理解后发现：  
👉 **以太坊的去中心化并不是单一机制的结果，而是多层设计叠加形成的整体系统工程。**

* * *

## 🧩 一、共识层：经济博弈保障的安全性

-   采用 **PoS + Gasper 共识机制**
    
    -   出块权与投票权分离
        
    -   由百万级验证者共同参与确认区块
        
-   引入强约束机制：
    
    -   **Slashing**：对明确作恶行为直接罚没质押本金
        
    -   **Inactivity Leak**：对长期不在线节点持续削减权重
        
-   本质是：
    
    > 用可惩罚的经济成本，替代算力博弈来保障安全性与最终性
    

* * *

## 🌐 二、网络层：真正无中心的 P2P 结构

-   全球成千上万节点通过 **P2P 网络直接连接**
    
-   没有中心服务器或调度节点
    
-   多种节点角色共同参与：
    
    -   全节点
        
    -   轻节点
        
    -   质押验证节点
        
-   网络本身具备：
    
    -   去中心发现
        
    -   多路径传播
        
    -   抗局部网络故障能力
        

* * *

## 🛠️ 三、实现层：多客户端战略对抗系统性风险

-   同一协议由多个团队、多个语言实现：
    
    -   Geth、Nethermind、Besu、Erigon（执行层）
        
    -   Prysm、Lighthouse、Teku、Nimbus（共识层）
        
-   官方长期刻意维持客户端占比的健康分布
    
-   目标不是效率最大化，而是：
    
    > 防止“一个 Bug 干翻整个网络”的灾难级风险
    

这是非常少见的工程取舍，本质是在用研发复杂度换系统安全边界。

* * *

## 🚀 四、扩容路径：Rollup 中心化执行，L1 专注共识与数据

-   扩展策略不是堆大 L1，而是：
    
    -   把执行和应用创新推向 **大量 L2 Rollup**
        
    -   L1 专注三件事：
        
        -   共识
            
        -   数据可用性
            
        -   去中心化安全
            
-   形成结构分工：
    
    -   L2 = 性能与应用实验场
        
    -   L1 = 安全与最终性锚点
        

这是一条明确的 **模块化区块链路线**。

* * *

## 🏛️ 五、治理层：社会共识优先于代币投票

-   没有形式化链上治理系统
    
-   协议升级依赖：
    
    -   公开讨论
        
    -   EIP 流程
        
    -   社区协调与客户端同步升级
        
-   避免：
    
    > 简单等同「代币持有量 = 治理权」
    

本质是承认：  
**区块链最终仍然是社会系统，而不仅是代码系统。**

* * *

## ✍️ 总结感受

以太坊的去中心化不是靠某个“神奇机制”实现的，而是：

> 在共识、网络、实现、扩容、治理多个层面同时做出工程取舍，  
> 用复杂度换取系统级安全与韧性。

这是一种非常“反商业直觉”、但极其工程理性的设计路线，  
也让我第一次真正理解了：

> 公链不是一个产品，而是一整套长期演化的社会—技术系统。

![Snipaste_2026-01-13_17-04-03.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/andOneBasketball/images/2026-01-13-1768295056827-Snipaste_2026-01-13_17-04-03.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->











> **Web3 实习计划 · Day 1 学习记录**

今天是 Web3 实习计划的第一天。昨晚通过线上 Zoom 会议认识了不少在 Web3 行业持续深耕、真正做实事的伙伴，对接下来的学习节奏和方向也更有信心了。

截至目前，我已经基本通读完《Web3 实习手册》的核心内容。结合自己以往的积累，目前已经具备使用 **Hardhat 与 Foundry 进行 Solidity 合约开发与测试** 的实战经验，对常规合约开发流程较为熟悉。

在接下来的训练营阶段，我希望进一步 **向交易系统与协议底层深入**，以 **DEX 与 CEX 的核心功能模块** 为切入点，系统性研究成熟开源项目的实现方式，例如：

-   CEX 方向：订单簿与撮合引擎（如 `i25959341/orderbook`）
    
-   DEX 方向：Uniswap V2 / V3 / V4 的合约架构、流动性机制与路由设计
    

通过源码级别的阅读、调试和复现，将学习成果逐步沉淀为：

-   可复用的技术总结
    
-   可展示的 Demo 项目
    
-   以及可写入简历的工程经验
    

技术栈规划上，短期仍以 **Golang + Solidity** 为主，重点加强：

-   撮合引擎与交易系统后端能力（Go）
    
-   协议与链上业务逻辑理解（Solidity）
    

中期目标是逐步补齐 **Next.js + Web3 前端交互** 能力，形成从合约 → 后端 → 前端的完整闭环开发能力。

长期希望能在 Web3 行业中找到与自身技术背景和兴趣高度匹配的 **Remote 开发岗位**，持续参与有真实业务价值的项目建设。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
