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
