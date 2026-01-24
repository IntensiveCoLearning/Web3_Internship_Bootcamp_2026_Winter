---
timezone: UTC+8
---

# Pair Whites

**GitHub ID:** Duibai49

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->
![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Duibai49/images/2026-01-24-1769221141289-image.png)
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->

每个 Solidity 文件必须以 **SPDX 许可证标识符**和 **Pragma** 开头。// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20; // 指定编译器版本

contract HelloWorld {

string public greeting = "Hello, Web3!";

}

## 2\. 数据类型 (Data Types)

### 值类型 (Value Types)

-   **uint / int**: 无符号/有符号整数（如 `uint256`）。
    
-   **bool**: 布尔值（`true` / `false`）。
    
-   **address**: 以太坊地址（20 字节）。
    
    -   `address.balance`: 查询余额。
        
    -   `address.transfer(amount)`: 转账（失败时报错）。
        
-   **bytes1 至 bytes32**: 定长字节数组。
    

### 引用类型 (Reference Types)

-   **string**: 字符串。
    
-   **struct**: 自定义结构体。
    
-   **mapping**: 键值对映射（`mapping(address => uint) public balances`）。
    
-   **array**: 动态或定长数组（`uint[]` 或 `uint[5]`）。
    
-   这是一份为您整理的 **Solidity 核心知识点学习笔记**。内容涵盖了从基础语法到高级特性的关键信息，采用了适合 Markdown 编辑器（如 Obsidian、Notion 或 VS Code）的格式，方便您直接复制保存。
    
    * * *
    
    # Solidity 开发核心笔记
    
    ## 1\. 基础结构与版本
    
    每个 Solidity 文件必须以 **SPDX 许可证标识符**和 **Pragma** 开头。
    
    Solidity
    
    ```
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.20; // 指定编译器版本
    
    contract HelloWorld {
        string public greeting = "Hello, Web3!";
    }
    ```
    
    * * *
    
    ## 2\. 数据类型 (Data Types)
    
    ### 值类型 (Value Types)
    
    -   **uint / int**: 无符号/有符号整数（如 `uint256`）。
        
    -   **bool**: 布尔值（`true` / `false`）。
        
    -   **address**: 以太坊地址（20 字节）。
        
        -   `address.balance`: 查询余额。
            
        -   `address.transfer(amount)`: 转账（失败时报错）。
            
    -   **bytes1 至 bytes32**: 定长字节数组。
        
    
    ### 引用类型 (Reference Types)
    
    -   **string**: 字符串。
        
    -   **struct**: 自定义结构体。
        
    -   **mapping**: 键值对映射（`mapping(address => uint) public balances`）。
        
    -   **array**: 动态或定长数组（`uint[]` 或 `uint[5]`）。
        
    
    * * *
    
    ## 3\. 函数权限与状态控制
    
    ### 函数可见性 (Visibility)
    
    | 关键字 | 说明 |
    | external | 仅限外部调用（合约内需用 this.f()）。 |
    | public | 内部/外部均可调用，自动为状态变量生成 Getter。 |
    | internal | 仅限当前合约及继承合约调用。 |
    | private | 仅限当前合约内部调用。 |
    

### 状态可变性 (Mutability)

-   **view**: 读取状态，但不修改状态。
    
-   **pure**: 不读取也不修改状态（如纯数学计算）。
    
-   **payable**: 允许该函数接收 Ether。
    
-   4\. 存储位置 (Data Storage)
    
    Solidity 中的数据存储位置决定了 Gas 消耗和生命周期：
    
    1.  **storage**: 永久存储在区块链上（状态变量默认位置）。
        
    2.  **memory**: 内存中临时存储，函数执行结束即释放。
        
    3.  **calldata**: 类似于 memory，但不可修改（常用于外部函数参数）。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->


![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Duibai49/images/2026-01-22-1769053508623-image.png)
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->



![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Duibai49/images/2026-01-21-1768974994477-image.png)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->




Solidity基础语法学习
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->





![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Duibai49/images/2026-01-19-1768793761701-image.png)
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->






Dapp 架构由前端，智能合约，数据检索器，区块链和去中心化存储
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->







周六休息
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->








![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Duibai49/images/2026-01-16-1768536167975-image.png)

学会了Remix IDE 的入门使用
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->









AI机器基础概念
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->










Web3 已从野蛮生长转向合规化，了解各国监管政策的同时注意安全反诈，警惕钓鱼链接，冒充官方，凡是助记词，账号密码等敏感信息谨慎输入，插件软件从官方下，面试只用Zoom/腾讯会议等主流平台，绝不下“专门面试平台”
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->











Web1: 只能看（只读）。像看报纸，新浪、搜狐发什么你看什么。

Web2: 能看能互动（读+写）。你在抖音、微博发东西，但数据是平台的。平台想封你就封你，你是给大厂打工的。

Web3: 读 + 写 + 拥有 (Own)。最关键的就是这个“拥有”。你的账号、资产、数据全在你手里，不是在腾讯或谷歌的服务器里Web3 靠什么撑起来？

1\. 区块链 (Blockchain) —— 终极“记账本”

不是存在某台电脑里，是全球几万台电脑同步记账。

重点： 没法改。只要写上去了，天王老子也删不掉。这解决了“信任”问题。

2\. 智能合约 (Smart Contracts) —— “自动贩卖机”

这玩意儿其实就是一段代码。

逻辑： 只要 A 发生了，就自动执行 B。不需要中介（比如律师、银行、公证处）。

吐槽： 虽然叫“智能”，其实挺死板的，代码写错一个字母钱就可能打水漂了。

3\. 钱包 & 私钥 —— 你的“赛博身份证”

Web2: 账号密码。忘了？找回密码就行。

Web3: 私钥（一串乱码）。千万不能丢！ 丢了谁也帮不了你，钱直接变死钱。

核心： 钱包不是存钱的，只是你操作区块链上资产的“遥控器”。

它是怎么跑起来的？（以买个 NFT 为例）

连钱包： 进到网站，点 Connect Wallet。这步相当于登录，但不用输密码，点个签名就行。

触发合约： 你点“购买”。这时候后台的智能合约就开始动了：检查你钱够不够，够的话就把这图的所有权划给你。

全网确认： 矿工（节点）开始干活，把这笔交易打包。

到账： 刷新一下，区块链上就记住了：这个 NFT 归你了。全程没经过任何公司的财务审批。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->












初级岗位竞争激烈，资深岗位人才缺口大
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
