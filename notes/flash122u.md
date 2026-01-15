---
timezone: UTC+8
---

# flash122u

**GitHub ID:** flash122u

**Telegram:** @flash_su

## Self-introduction

目标是掌握Solidity 基础，可以写一个小项目

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
**  
Solidity 合约类型总结**

| 核心维度 | 具体内容 |
| --- | --- |
| 合约类型定义 | Solidity 中合约本身是一种数据类型，与 uint、address 等基础类型地位等同，可声明合约类型变量（如 Hello h）。 |
| 合约创建方式 | 使用 new 关键字在合约中部署新合约（如 h = new Hello()），返回新合约实例。 |
| 合约交互方式 | 通过合约类型变量调用目标合约的公开函数（如 h.sayHi()），实现合约间函数调用。 |
| 类型转换规则 | 合约类型 ↔ 地址类型可双向转换： 1. 合约转地址：address(hello) 2. 地址转合约：Hello(helloAddr) |
| 核心应用场景 | 构建复杂合约系统（如工厂合约创建子合约、多合约协作交互）。 |
| 关键注意事项 | 仅能调用目标合约的公开/外部函数；转换地址需确保对应合约已部署且类型匹配。 |

```
pragma solidity ^0.8.0;
​
// 1. 定义 Counter 合约
contract Counter {
    // 计数状态变量
    uint public count;
​
    // 增加计数的函数
    function increment() public {
        count += 1;
    }
​
    // 获取当前计数的函数
    function getCount() public view returns (uint) {
        return count;
    }
}
​
// 2. 定义 CounterFactory 工厂合约
contract CounterFactory {
    // 存储创建的 Counter 合约实例
    Counter public counter;
​
    // 创建新的 Counter 合约
    function createCounter() public returns (address) {
        counter = new Counter();
        return address(counter);
    }
​
    // 调用 Counter 合约的 increment 函数
    function incrementCounter() public {
        // 检查合约是否已创建
        require(address(counter) != address(0), "Counter not created yet");
        counter.increment();
    }
​
    // 获取 Counter 合约的当前计数值
    function getCounterValue() public view returns (uint) {
        require(address(counter) != address(0), "Counter not created yet");
        return counter.getCount();
    }
}
```

## **Solidity 枚举类型总结**

| 核心维度 | 具体内容 |
| --- | --- |
| 定义方式 | 使用 enum 关键字定义，格式：enum 枚举名 { 常量1, 常量2, ... } |
| 数值特性 | 1. 从 0 开始递增编号 2. 底层存储为 uint8 类型 3. 默认值为第一个枚举值 |
| 类型转换 | 1. 枚举 ↔ uint 可相互转换 2. 整型转枚举需用 require 检查值范围，避免越界 |
| 核心操作 | 1. 支持 ==/!= 比较运算 2. 通过 type(枚举名).min/max 获取取值范围 |
| 典型应用场景 | 表示有限状态集合：订单状态、任务工作流、合约运行状态等 |
| 最佳实践 | 1. 替代“魔法数字”提升代码可读性 2. 状态转换时添加合法性验证 3. 状态变更时触发事件 |
| 核心优势 | 1. 代码可读性/可维护性提升 2. 限制变量取值范围，减少非法状态 |

-   枚举是**值类型**，赋值时会进行拷贝，而非引用；
    
-   枚举定义后不可动态增删常量，适合表示固定的状态集合；
    
-   结合结构体、映射可实现复杂的状态管理（如订单/任务系统）。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->


Solidity 布尔类型小结

| 分类 | 核心内容 | 关键说明/示例 |
| --- | --- | --- |
| 基础特性 | 关键字、取值、默认值 | 关键字：bool；取值：true/false；默认值：false |
| 核心运算符 | 逻辑运算符：&&（与）、\|\|（或）、!（非） 比较运算符：==/!=/</<=/>/>= | 1. &&/\|\| 支持短路求值； 2. 所有比较运算符返回布尔值 |
| 短路求值 | 规则、优化技巧 | 规则：&&左false/\|\|左true 则右侧不执行； 技巧：按概率前置条件节省Gas |
| 典型应用 | 权限控制、状态管理、功能开关、条件判断 | 示例： 1. 权限：user == owner \|\| admins[user]； 2. 开关：!featureEnabled |
| 最佳实践 | 命名、Gas优化、安全校验 | 1. 命名：isXXX/hasXXX； 2. Gas：bool与小类型打包存储； 3. 校验：require(布尔条件, 报错信息) |

* * *

## **Solidity 地址类型小结**

| 分类 | 核心内容 | 关键说明/示例 |
| --- | --- | --- |
| 基础定义 | 地址类型作用、两种类型 | 作用：表示以太坊账户/合约地址（20字节） 类型： 1. address：普通地址，不可接收ETH 2. address payable：可支付地址，可接收ETH |
| 类型转换 | 普通地址转可支付地址 | 语法：address payable ap = payable(addr); 注意：合约地址需实现receive/fallback才能接收ETH |
| 常用操作 | 1. 地址比较 2. 查询余额 3. ETH转账 | 1. 比较：==/!=，零地址：address(0) 2. 余额：addr.balance（单位wei，address(this).balance查合约余额） 3. 转账：推荐to.call{value: msg.value}(“”)，需校验返回值 |
| 重要提示 | 开发最佳实践与风险防范 | 1. 校验零地址：require(addr != address(0)) 2. 转账优先用call，避免transfer/send的2300 Gas限制 3. 防范重入攻击 |

```
//存钱罐合约
pragma solidity ^0.8.0;
​
contract PiggyBank {
    address public owner;
​
    constructor() {
        owner = msg.sender;
    }
​
    // 接收 ETH
    receive() external payable {}
​
    // 查询余额
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
​
    // 提取 ETH（只有 owner 可以）
    function withdraw() public {
        require(msg.sender == owner, "Only owner can withdraw");
        (bool success, ) = payable(owner).call{value: address(this).balance}("");
        require(success, "Withdraw failed");
    }
}
```

* * *
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->



## Solidity 数据类型小结

| **分类**                     | **核心内容**                                                 | **关键特性**                                                 |
| ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **类型体系**                 | 分为**值类型**、**引用类型**、**映射类型**三大类             | Solidity 是**静态类型语言**，变量类型编译时确定              |
| **值类型**                   | 包含布尔、整型（uint/int）、地址、定长字节数组、枚举、函数类型等 | 1. 赋值/传参时**值拷贝**，修改副本不影响原变量<br>2. 数据占用 ≤32 字节<br>3. 示例：`uint a=10; uint b=a; b=20;` → `a` 仍为 10 |
| **引用类型**                 | 包含数组、结构体、string/bytes（动态字节数组）、合约         | 1. 数据占用通常 >32 字节，支持**引用传递**减少拷贝开销<br>2. 必须显式指定**数据位置**（memory/storage/calldata）<br>3. 赋值规则：<br>   - 不同位置赋值 → 拷贝副本<br>   - 相同位置赋值 → 创建引用（修改新变量会影响原变量） |
| **数据位置（引用类型专属）** | 4 种位置（transient 不支持引用类型）                         |                                                              |
|                              | **storage**                                                  | 存储状态变量，永久保存在区块链，Gas 消耗最高                 |
|                              | **memory**                                                   | 函数内临时存储，函数执行后释放，Gas 中等                     |
|                              | **calldata**                                                 | 存储 external 函数参数，只读不可修改，Gas 消耗最低           |
|                              | **transient**                                                | 单次交易内有效，不支持引用类型定义                           |
| **映射类型**                 | 键值对结构（`mapping(KeyType => ValueType)`）                | 1. 类似字典/哈希表，仅支持作为**状态变量**（storage）<br>2. 详细用法需结合后续数据结构章节学习 |

1. **值类型 vs 引用类型**
    - 值类型：拷贝传递，独立存储，适合简单数据。
    - 引用类型：需指定数据位置，支持引用传递，适合复杂数据（数组、结构体等），可节省 Gas。
2. **数据位置选择原则**
    - 状态变量默认 `storage`；
    - 函数内临时变量用 `memory`；
    - `external` 函数参数优先用 `calldata`；
    - 交易内临时共享数据用 `transient`（仅限值类型）。
3. **赋值行为关键规则**
  
    > 不同位置赋值→拷贝，相同位置赋值→引用。





## Solidity 整型（uint与int）小结

| **类别**              | **核心内容**                                                 |
| --------------------- | ------------------------------------------------------------ |
| **类型定义**          | 1. 两类整型：`uint`（无符号整数，非负）、`int`（有符号整数，可正负）<br>2. 位数范围：`uint8`-`uint256`、`int8`-`int256`，**8位步进**<br>3. 默认类型：`uint`等价`uint256`，`int`等价`int256` |
| **取值范围**          | 1. `uintN`：`0 ~ 2^N - 1`（如`uint8`：`0~255`）<br>2. `intN`：`-2^(N-1) ~ 2^(N-1) - 1`（如`int8`：`-128~127`）<br>3. 默认值：所有整型变量未赋值时默认**0** |
| **运算规则**          | 1. **支持运算符**：比较（`>`/`<`/`==`等）、算术（`+`/`-`/`*`/`/`/`%`/`**`）、位运算（`&`/`|`/`^`/`~`）、移位（`<<`/`>>`）<br>2. **溢出处理**（Solidity 0.8.0+）：<br>   - 默认`checked`模式：溢出直接**交易回退**<br>   - `unchecked{...}`代码块：溢出不检查，数值循环（如`uint8(255)+1=0`）<br>3. **特殊运算**：整数除法**截断小数**（如`5/2=2`）；除零直接回退 |
| **Gas 优化**          | 确定运算无溢出时，使用`unchecked`模式可**节省 Gas**（单次约省0.26%，循环场景效果显著） |
| **使用场景选择**      | 1. **优先用 uint**：代币余额、数组索引、时间戳、计数器等**非负场景**<br>2. **使用 int**：温度、价格波动、数值差值等**需表示正负的场景** |
| **常见陷阱&避坑方案** | 1. **无符号下溢**：`uint x=0; x-1`会回退 → 先判断`x>0`再减法<br>2. **除法截断**：需精度时放大分子（如`(5*1e18)/2`）<br>3. **类型转换截断**：大类型转小类型前，用`require(val <= type(uint8).max)`校验<br>4. **跨类型比较**：`int`与`uint`直接比较易出错 → 统一转换为相同类型再比较 |
| **进阶技巧**          | 用`type(X).min`/`type(X).max`获取整型`X`的最值（如`type(uint8).max=255`） |
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->





## 准备：

**以太坊智能合约开发基础**: https://learnblockchain.cn/article/22542?course_id=93

**IDE**: Remix  https://remix.ethereum.org/

**MetaMask** 钱包: https://metamask.io/zh-CN/download





## Solidity 合约基础小结



1.  **合约基础结构**
    Solidity 合约是部署到区块链的最小单元，类似其他语言的`class`，核心由**状态变量**（存储合约数据）和**合约函数**（处理数据逻辑）组成。
    编写合约需先通过`pragma solidity`声明编译器版本（如`pragma solidity >=0.8.0;`）。

2.  **核心组件解析**

    - **构造函数**：用`constructor`声明，仅在合约部署时执行，用于初始化状态变量；若省略，编译器会自动生成默认构造函数。状态变量可在声明时直接初始化，未初始化则默认值为 0。

    - **变量与函数可见性**：共 4 种，`public`（内部/外部均可访问，状态变量自动生成 getter 函数）、`external`（仅外部可调用，内部调用需用`this.func()`）、`internal`（当前合约及派生合约可访问，默认可见性）、`private`（仅当前合约可访问）。**注意**：链上数据透明，`private`仅限制访问权限，无法隐藏数据。

      | 特性             | public | external | internal | private |
      | ---------------- | :----: | :------: | :------: | :-----: |
      | 修饰函数         |   √    |    √     |    √     |    √    |
      | 修饰变量         |   √    |          |    √     |    √    |
      | 当前合约内可访问 |   √    |          |    √     |    √    |
      | 派生合约可访问   |   √    |          |    √     |         |
      | 外部访问         |   √    |    √     |          |         |

    - **特殊变量类型**：

      - `constant`常量：编译期确定值，不占用存储，仅支持值类型和字符串，赋值不可依赖链上/外部数据。
      - `immutable`不可变量：部署时（构造函数中）赋值，之后不可修改，不占用存储，适合存储合约创建者地址、关联合约地址等固定值。

3.  **函数定义规则**
    函数格式为`function 函数名(参数类型 参数名) 可见性 状态可变性 returns(返回类型)`，包含以下核心特性：

    - **参数与返回值**：参数声明与变量一致；返回值支持命名（可直接赋值）或仅指定类型，用`return`语句返回结果。
    - **状态可变性修饰符**：
      - `view`视图函数：仅读取状态，外部调用无需 Gas，内部被修改状态的函数调用时需消耗 Gas。
      - `pure`纯函数：不读取、不修改状态，仅做计算逻辑。
      - `payable`：允许函数接收以太币，未声明则拒绝接收以太币。
    - **状态修改逻辑**：修改状态变量的函数需通过区块链交易调用，消耗 Gas 并经节点共识确认；仅读取状态的函数可直接在节点执行，无需上链。

4.  合约的核心是**状态转换**



## 总结表格

| 总结         |                                                              |
| ------------ | ------------------------------------------------------------ |
| 合约基础     | 类似`class`，包含状态变量（数据）+ 函数（逻辑）；需用`pragma solidity`声明编译器版本 |
| 构造函数     | `constructor`关键字，部署时执行初始化；可省略（默认生成空构造函数） |
| 可见性修饰符 | public（内外可访问，变量自动生成getter）、external（仅外部调用）、internal（默认，当前/派生合约）、private（仅当前合约） |
| 特殊变量     | constant（编译期定值，不占存储）、immutable（部署时赋值，不可修改） |
| 函数定义     | 格式：`function 名(参数) 可见性 状态可变性 returns(返回类型)` |
| 状态可变性   | view（仅读状态）、pure（不读不改）、payable（可接收以太币）  |
| 核心特性     | 状态修改需上链交易（耗Gas），仅读取可直接节点执行（免Gas）；链上数据透明，private不隐藏数据 |
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
