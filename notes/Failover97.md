---
timezone: UTC+8
---

# Failover97

**GitHub ID:** Failover97

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
## Exposed Data

区块链看似匿名的特性可能会给用户带来虚假的安全感。只要链上拥有足够的数据，用户的匿名性就很容易被破解。个人身份信息（PII）

**数据保护合规性**

对于将PII存储在链上时违反例如(GDPR)欧盟的《通用数据保护条例》的要求。

除了PII之外IP地址和电子邮件等信息存储在链上也会造成巨大风险。

## FrontRunning（今天太累了关于抢跑还有一些内容没学完，明天补充 ）

区块链网络上的每笔交易在执行前都会在内存池中经历一段可见期。这种透明性使得网络参与者能够在交易被打包进区块之前查看并对其做出响应。攻击者可以利用这种信息泄露来影响交易的执行过程。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

# JS的特点

在 JavaScript 里，某些操作（比如**读取文件、网络请求、数据库查询、区块链交互**等）需要**一定的时间**才能完成。JavaScript 采用**异步编程（异步的意思指的是好几个耗时线程可以并行操作，不会被block)**来避免阻塞主线程。`await` 让代码 **按顺序执行**，避免出现未完成的任务导致错误。 默认会把 **I/O 操作（如数据库、网络请求、区块链调用）** 设为异步

### 什么是合约工厂？

合约工厂（Contract Factory）**是用于部署新智能合约的对象**。在 Hardhat + Ethers.js 中，你不能直接创建合约实例，而是需要先获取合约工厂，然后再部署合约。

**这和现实中的“工厂”很类似**：

-   **合约文件 (**`DiceMarket.sol`**)** 就像**蓝图**。
    
-   **合约工厂 (**`getContractFactory("DiceMarket")`**)** 就像一个**工厂**，可以根据**蓝图**生产多个合约实例。
    
-   **部署后的合约 (**`DiceMarket.deploy(...)`**)** 就像**最终生产出来的产品**。
    

# 关键字

### async

-   申明一个异步函数，返回值始终是Promise
    
-   如果函数内部返回的是普通值，JavaScript会自动封装成Promise.resolve(返回值）
    

```
async function hello() {
    return "Hello, world!";
}

hello().then(console.log); // 输出: Hello, world!由于关键字的作用，所以需要用.then()处理
```

### await

该关键字只能在async function内部使用，

它会暂停函数的执行，直到Promise解析（resolve),然后返回Promise解析之后的值

```
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function run() {
    console.log("开始");
    await delay(2000); // 等待 2 秒
    console.log("2 秒后执行");
}

run();
```

代码里的语法：

new Promise((resolve,reject)=>{.....}

resolve是一个函数，表示成功时候调用

reject也是一个函数，表示失败时候调用

setTimeout(resolve,ms)

setTimeout是JavaScript的定时器函数，作用是等待一段时间后执行某个操作

setTimeout(resolve,ms)代表等待ms毫秒后执行resolve()

### console

在 JavaScript 中，`console` 是一个**全局对象**，主要用于**打印调试信息**，帮助开发者查看代码的执行情况。最常用的方法是 `console.log()`，但 `console` 还有很多其他功能。

```
console.log("Hello, JavaScript!");
```

console的其他用法

![](https://cdn.nlark.com/yuque/0/2025/png/34360919/1741086687786-5e0d8cd5-cb6c-401b-aa9d-4a4a85756fe5.png)

# 变量声明和数据类型

### 变量声明的三种方式

```
// let - 推荐使用,可以重新赋值
let age = 25;
age = 26; // 允许

// const - 用于常量,不能重新赋值
const PI = 3.14;
// PI = 3.15; // 这会报错

// var - 旧版本使用,现在不推荐
var name = "John";
```

### 基本数据类型

```
// 数字 (Number)
let count = 42;        // 整数
let price = 99.99;     // 小数
let infinity = Infinity;//特殊用法，表示无穷大
let notANumber = NaN;//not a number

// 字符串 (String)
let firstName = "Alice";
let greeting = 'Hello';
let message = `Hello ${firstName}`; // 模板字符串

// 布尔值 (Boolean)
let isActive = true;
let isLoggedIn = false;

// 空值
let empty = null;
let undefined;

// 数组 (Array);❤这里的数组跟python里面的列表特别像
let colors = ['red', 'green', 'blue'];
let numbers = [1, 2, 3, 4, 5];

// 对象 (Object) 对象非常像C语言里面的结构体
let person = {
    name: "Alice",
    age: 25,
    isStudent: true
};
```

### 类型转换

```
// 转换为数字
let strNumber = "123";
let num1 = Number(strNumber);    // 123
let num2 = parseInt(strNumber);  // 123
let num3 = parseFloat("123.45"); // 123.45  
let bigNum=BigInt("123455677766");//bigint专门用于大整数

// 转换为字符串
let number = 123;
let str1 = String(number);      // "123"
let str2 = number.toString();   // "123"

// 转换为布尔值
let bool1 = Boolean(1);         // true
let bool2 = Boolean("");        // false
let bool3 = Boolean("hello");   // true
```

NaN

not a number；通常用于用户的输入校验

```
// 1. 验证用户输入是否为有效数字
function validateUserInput(input) {
    let number = parseFloat(input);
    if (isNaN(number)) {
        return "请输入有效的数字";
    }
    return "输入有效";
}

// 2. 数学计算错误检查
function divideNumbers(a, b) {
    let result = a / b;
    if (isNaN(result)) {
        return "计算出错";
    }
    return result;
}
```

注意校验NaN不需要用==号，可以直接用isNaN

### Hoisting

function关键字定义的函数会被完整的提升，可以在定义前调用

```
hello(); // 输出: Hello, world!

function hello() {
    console.log("Hello, world!");
}
/*其实就相当于*/
function hello() { // 函数声明被提升
    console.log("Hello, world!");
}

hello(); // 调用
```

### 箭头函数

箭头函数的语法

**const 函数名 = (参数) => { 函数体 }**

```
const add = (a, b) => {
    return a + b;
};
console.log(add(2, 3)); // 输出: 5
```

**隐式返回**

如果函数体只有一行return语句，可以省略{}和return

```
const multiply = (a, b) => a * b;
console.log(multiply(3, 4)); // 输出: 12
```

**参数规则**

![](https://cdn.nlark.com/yuque/0/2025/png/34360919/1740129457676-5a38cb23-b7a6-4133-b9da-904012844f42.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


**unchecked:**

避免solidity 0.8.0开始的编译器自动对合约做数学安全检查，消耗gas.(高频函数非常在意gas)

避免使用unchecked的场景：

1.  输入来自用户
    
2.  条件依赖外部合约
    
3.  没有显式边界检查
    
4.  金额/索引/balance操作
    

## 危险调用

### **abi.encodePacked()**

功能：把几段字符拼接在一起，不加分隔符

比较安全的方法，拼接固定字节

-   `uint256` 固定 32 字节
    
-   `address` 固定 20 字节
    
-   `bytes32` 固定 32 字节
    

abi.encodePacked+动态类型 +hash/授权/校验 =！！！风险

### Ambiguous evaluation order

```jsx
function foo() public pure returns(uint){
uint x=5;
return x*x++;//could be 25 or 30
}
```

vulnerability mitigation:storing intermidiate results in temporary variables.

### Approval Vulnerabilities

**无限审批漏洞（权限失控）**

```jsx
approve(spender,type(uint256).max)
```

特点：授权额度无限、授权时间永久、不绑定具体交易\\业务条件

攻击路径：

1、用户授权无限审批（合约A）

2、合约A：

-   被黑
    
-   有后门
    
-   owner作恶
    

3、攻击者获得合约控制权

4、调用

```jsx
token.transferFrom(
	user,
	attacker,
	token.balanceOf(user)
	);
```

**审批抢先**

攻击者在受害者修改授权额度之前，提前花完原有的授权额度。（ERC20）

例子：

1.  通过此`approve x`功能，用户允许智能合约系统转移
    
    其 ERC20 代币。
    
2.  后来，他们选择修改授权金额，`approve` y 并因此发送了另一份请求。
    
3.  与此同时，在用户提供的交易被包含之前，攻击者启动了从用户钱包中`transferFrom x`删除代币的功能。
    
4.  如果攻击者的交易先被处理，然后用户的新`approve y`交易才被处理，那么恶意行为者就可以转移额外的代币。
    
5.  未经授权的转账总额将达到`x+y`代币级别。
    

```jsx
//allowances状态变量
mapping(address=>mapping(address=>uint256) private_allowances;
_allowances[owner][spender]=授权额度还剩下多少 
```

```jsx
//ERC20授权模型
approve(spender,value);//设置授权额度
transferFrom(owner,to,x);//在额度内转账
```

**Recommended Best Practices for Approval and Allowance Management**

针对 **ERC-20** 授权更新可能引发的抢跑风险，一个直接的缓解措施是优化 allowance 管理方式。相比通过approve以覆盖式写入新授权值，更推荐采用**增量调整**的模式，例如使用 OpenZeppelin SafeERC20提供的 safeIncreaseAllowance 与 safeDecreaseAllowance 来按差值更新授权额度，从而降低在“修改授权额度”场景下出现竞态条件的概率。

另一方面，“approval phishing（审批钓鱼）”虽不属于合约代码层面的漏洞，但在前端遭受内容注入或被篡改时，攻击者可将授权目标从预期合约地址替换为其控制地址，诱导用户完成恶意授权。为降低风险，开发者应遵循最小权限原则，仅授权业务所需的最小额度，并在必要时采用更严格的交互流程（例如两步操作、限额或延时机制）以减少单次恶意行为的影响；用户则应养成定期检查与撤销不再使用授权的习惯，可借助 [revoke.cash](http://revoke.cash) 等工具进行授权清理。

# Remix

### contract按键显示规则

**变量**

Public状态变量，自动形成蓝色按钮变成getter

**函数**

View/Pure函数，自动变成蓝色按钮，只读

External/Public函数，橙色按钮（会改变状态）

Payable函数，红色按钮（可以接受ETH）

![image.png](attachment:5a65c35f-3909-407a-9257-37db4007463b:image.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



![image.png](attachment:5211f265-6361-475e-ae51-59654d33eee4:image.png)

![image.png](attachment:5c11b668-535e-4415-b6b6-8f19c1688dd6:image.png)

![image.png](attachment:324ff638-f490-47ae-9054-3bf3a6d3c66d:image.png)

## grammar

**四种变量可见性函数**

| 可见性 | 允许调用位置 | 常见用途 |
| --- | --- | --- |
| public | 合约内部+外部+子合约 | 默认接口+对外方法 |
| external | 外部调用 only | 对外API，节省gas |
| internal | 合约内部+自合约 | 内部逻辑 |
| private | 仅当前合约 | 封装细节 |

**modifier**:修饰符，给函数创建前置条件

```jsx
modifier onlyOwner(){
require(msg.sender == owner, "not owner");
_;//占位符，若符合条件在这里执行函数
}
```

**constructor:**构造函数，用于初始化设置（合约部署时自动执行一次）

```jsx
constructor(){
	owner=msg.sender;
	}
```

**receive()**:收钱函数;

```jsx
receive() external payable{} //专门从外部接受Eth
```

**calldata:** 外部调用的不可修改的函数参数；（省gas)

```jsx
function transferMany(address[] calldata users)//通常是external的函数参数，或者是复杂大的参数
```

**keccak256:**任意长度的bytes类型，变成32位的hash串

```jsx
bytes32 h = keccak256(data)
```

**require**(条件，“错误信息”）；

if true —→ 继续执行后续代码

if false —→ 立即停止，报错并且回滚所有操作 ❌

**emit:用来出发event的关键字，往区块链日志里写一条事件记录。**

```jsx
event Transfer(address from, address to, uint256 value);
function send(address to, uint256 amount) public{
balances[msg.sender]-=amount;
balances[to]+=amount;

emit Transfer(msg.sender,to,amount);
}
```

设计目的：

1、给前端/dApp用

2、给链下系统用 后端可以监听链上行为

3、比存storage便宜很多（省gas)

**call: low-level call**

```jsx
(bool ok,)=users[i].call{value:reward)(" ");//给用户地址i转reward wei数量的ehter 并且读取是否成功数据
address.call(bytes) //发起一次底层call,返回true或者false的数据

（bool ok,) = users[i].call{value: reward}(" ");
require(ok,"ETH transfer failed");
```

## 危险调用

**abi.encodePacked()**

功能：把几段字符拼接在一起，不加分隔符

比较安全的方法，拼接固定字节

-   `uint256` 固定 32 字节
    
-   `address` 固定 20 字节
    
-   `bytes32` 固定 32 字节
    

abi.encodePacked+动态类型 +hash/授权/校验 =！！！风险

## 英语单词

revert:回滚
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
