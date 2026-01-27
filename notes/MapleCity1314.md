---
timezone: UTC+8
---

# Presto

**GitHub ID:** MapleCity1314

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->
昨天在准备科目三忘打卡了...今天刚考完试 简单偷偷懒

## 基本小计划：

1.  准备hackathon idea （√）
    
2.  学习了AI相关的ERC标注呢（√）
    
3.  重新温习了一下web3的基本概念相关基本知识（√）
    

## 今天发现的问题

1.  自己对社区治理，生态理念理解还不够（有点过于技术了）
    
2.  自己需要一定程度上的双修
    
3.  自己作息需要调整
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->

1月25这天太忙了，主要在肝x402和新的合约

然后和好朋友大老板交流了AI时代程序员何去何从的发展

-   x402拜读
    
-   **Reactive Contracts拜读**
    
-   **尝试完成spoonOS的Agent中**
    
-   **拜读polymarket知识中**
    

文档读的太多了 脑袋爆炸
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->


## 002

原合约：

```bash
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Fallback {
    mapping(address => uint256) public contributions;
    address public owner;

    constructor() {
        owner = msg.sender;
        contributions[msg.sender] = 1000 * (1 ether);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "caller is not the owner");
        _;
    }

    function contribute() public payable {
        require(msg.value < 0.001 ether);
        contributions[msg.sender] += msg.value;
        if (contributions[msg.sender] > contributions[owner]) {
            owner = msg.sender;
        }
    }

    function getContribution() public view returns (uint256) {
        return contributions[msg.sender];
    }

    function withdraw() public onlyOwner {
        payable(owner).transfer(address(this).balance);
    }

    receive() external payable {
        require(msg.value > 0 && contributions[msg.sender] > 0);
        owner = msg.sender;
    }
}
```

### 问题：

这个合约的核心漏洞在 `receive()`：

```solidity
receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
}

```

攻击者只需要：

1.  先调用 `contribute()` 发送一笔很小的金额（< 0.001 ETH），让 `contributions[msg.sender] > 0` 成立
    
2.  然后**直接给合约地址转账**触发 `receive()`，就会把 `owner` 改成攻击者
    
3.  接着调用 `withdraw()` 把合约余额全部转走
    

### fix：

每次 `contribute()` 后，如果你的累计贡献超过当前 owner（或超过最高贡献），就成为 owner

```bash
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract FallbackDynamicOwner {
    mapping(address => uint256) public contributions;
    address public owner;

    uint256 public constant MIN_DELTA = 0.0001 ether;

    event Contributed(address indexed user, uint256 amount);
    event OwnershipChanged(address indexed oldOwner, address indexed newOwner);
    event Withdraw(address indexed to, uint256 amount);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "caller is not the owner");
        _;
    }

    function contribute() external payable {
        require(msg.value > 0, "no value");
        require(msg.value < 0.001 ether, "too much");

        contributions[msg.sender] += msg.value;
        emit Contributed(msg.sender, msg.value);

       
        if (contributions[msg.sender] >= contributions[owner] + MIN_DELTA) {
            address old = owner;
            owner = msg.sender;
            emit OwnershipChanged(old, msg.sender);
        }
    }

    function withdraw() external onlyOwner {
        uint256 bal = address(this).balance;
        (bool ok, ) = payable(owner).call{value: bal}("");
        require(ok, "withdraw failed");
        emit Withdraw(owner, bal);
    }

    // ✅ 修复点：receive 不允许改 owner
    receive() external payable {
        contributions[msg.sender] += msg.value;
        emit Contributed(msg.sender, msg.value);
    }
}

```
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->



在 spoonOS 的设计哲学中，Agent 被定义为具备感知（Perception）、记忆（Memory）、规划（Planning）与行动（Action）能力的计算实体。其核心运行机制基于“推理-行动”（Reasoning and Acting, ReAct）范式，即模型通过逻辑推理决定下一步操作，获取环境反馈后再次进行推理，形成闭环。

构建一个标准的 spoonOS Agent 涉及以下四个关键维度的配置与开发：

1\. **推理核心（Inference Engine）**：通常挂载大参数量的 LLM（如 DeepSeek, GPT-4 等），负责处理自然语言理解与逻辑推演。

2\. **指令集（Instruction Set）**：即 System Prompt，用于定义 Agent 的行为边界、角色设定及输出规范。

3\. **能力扩展（Tool/Skill Integration）**：通过定义明确的 API 接口，使 Agent 具备操作文件系统、网络请求及数据库交互的确定性能力。

4\. **记忆模块（Memory Module）**：管理短期对话上下文与长期向量存储，确保多轮交互的逻辑连贯性。

Agent 的实例化首先需要建立与推理后端的连接。在 spoonOS SDK 中，通过 `ModelWrapper` 类实现对底层模型的抽象封装。在此阶段，参数 `temperature` 的设定至关重要：对于工具调用类任务，建议将其设置为 0 或极低值（如 0.1），以降低模型输出的随机性，确保工具参数生成的准确性。

\`\`\`python

import spoon\_os\_sdk as spoon

from spoon.core.llm import DeepSeekModel

\# 初始化推理引擎

\# 采用低温度设定以保证逻辑严密性与工具调用的确定性

inference\_engine = DeepSeekModel(

model\_name="deepseek-v3",

temperature=0.1,

max\_tokens=4096

)

\# 实例化 Agent 对象，配置滑动窗口记忆机制

agent\_instance = spoon.Agent(

name="LogAnalysisBot",

llm=inference\_engine,

memory\_policy=spoon.MemoryPolicy.SlidingWindow(k=10)

)

\`\`\`

系统提示词并非简单的自然语言指令，而是对模型行为空间的约束。在学术定义上，它是为了收敛模型的解空间。编写高效 Prompt 需遵循“立项-约束-示例”的原则（Role-Constraint-FewShot）。

\* **角色定义**：明确 Agent 的功能域。

\* **约束条件**：明确禁止的行为（如禁止编造数据）。

\* **思维链（Chain of Thought, CoT）引导**：要求模型在执行动作前显式输出推理过程，这有助于提高复杂任务的准确率。

\`\`\`python

SYSTEM\_PROMPT = """

你是一个运行在 spoonOS 环境下的日志分析专家。

你的主要职责是解析非结构化的工作日志，提取关键指标，并生成结构化报告。

行为准则：

1\. 在执行任何外部操作前，必须先进行思维链推理（Think step-by-step）。

2\. 对于缺失的关键信息（如日期、项目ID），必须向用户发起追问，严禁进行概率性猜测。

3\. 最终输出格式必须严格遵循 Markdown 规范。

"""

agent\_instance.set\_system\_instruction(SYSTEM\_PROMPT)

\`\`\`

大模型本身是概率性的，而业务逻辑（如读写文件）是确定性的。工具（Tools）是连接两者的桥梁。在 spoonOS 中，利用 Python 的装饰器模式将本地函数注册为 Agent 可感知的工具。

**关键技术点**：必须提供详尽的 Docstring（文档字符串）和类型注解（Type Hinting）。大模型依赖这些元数据来理解工具的语义功能及参数结构。

\`\`\`python

from spoon.decorators import tool

@tool(name="fetch\_system\_logs", description="检索指定日期范围内的系统运行日志")

def fetch\_system\_logs(start\_date: str, end\_date: str) -> str:

"""

Args:

start\_date: ISO 8601 格式的开始日期 (YYYY-MM-DD)

end\_date: ISO 8601 格式的结束日期 (YYYY-MM-DD)

Returns:

包含日志内容的纯文本字符串

"""

\# 实现与操作系统文件系统的交互逻辑

return [spoon.fs.search](http://spoon.fs.search)(path="/var/logs", range=(start\_date, end\_date))

\# 将工具注入 Agent 的执行上下文中

agent\_instance.bind\_tools(\[fetch\_system\_logs\])

\`\`\`

\---

幻觉（Hallucination）与参数虚构

**现象描述**：Agent 在缺乏相关信息时，偶尔会虚构工具的调用参数（如编造不存在的文件名）。

**理论分析**：这是生成式模型的固有特性，模型倾向于补全合理的文本序列而非基于事实查证。

**解决方案**：

1\. **Prompt 强化**：在 System Prompt 中加入“工具调用失败时必须停止并报错”的强约束。

2\. **后处理校验（Post-execution Verification）**：在工具函数内部增加参数合法性校验逻辑，一旦检测到非法参数，抛出特定的错误信息反馈给 LLM，迫使其进行自我修正。

无限循环与任务发散

**现象描述**：Agent 在执行任务时陷入逻辑死循环，反复调用同一工具或无法终止思考过程。

**解决方案**：

1\. **最大迭代阈值（Max Iterations）**：在 SDK 层面设置 ReAct 循环的最大次数（如 10 次），强制终止超长任务。

2\. **停止序列（Stop Sequence）**：配置特定的 Token 作为停止信号，防止模型生成多余的无效文本。

上下文窗口限制（Context Window Constraint）

**现象描述**：随着对话轮数增加，Token 数量超出模型限制，导致早期关键指令丢失。

**解决方案**：采用 spoonOS 提供的 `SummaryMemory` 策略。当上下文达到阈值时，自动触发另一个 LLM 实例对历史对话进行摘要压缩，仅保留关键状态信息，而非原始的逐字记录。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->




今天没有涉及web3的内容哎 主要是和py还有AI相关的，windows上安装embedding python以及pytorch

## 目标

-   在项目中内置 Python 运行时（Windows）
    
-   准备 PyTorch + torchaudio + [pyannote.audio](http://pyannote.audio) 的离线 wheelhouse
    
-   为后续语音转写/说话人分离打底
    

## 关键步骤

1.  安装 Python 3.10
    
    -   `py -3.10` 不存在时，先安装 Python 3.10。
        
2.  下载依赖到 wheelhouse（可离线用）
    
    -   使用 PyTorch CPU 源：
        
    -   典型命令：
        
        ```
        py -3.10 -m pip download --dest wheelhouse --only-binary=:all: ^
          --index-url <https://pypi.org/simple> ^
          --extra-index-url <https://download.pytorch.org/whl/cpu> ^
          "pyannote.audio==3.1.1" torch torchaudio
        
        ```
        
    -   部分包没有 wheel，需要允许源码包：
        
        ```
        py -3.10 -m pip download --dest wheelhouse --no-binary=:all: docopt
        py -3.10 -m pip download --dest wheelhouse --no-binary=:all: antlr4-python3-runtime==4.9.3
        py -3.10 -m pip download --dest wheelhouse --no-binary=:all: julius
        
        ```
        
3.  离线安装到嵌入式 Python 目录
    
    -   目标目录：`src-tauri/lib/python310/Lib/site-packages`
        
    -   命令：
        
        ```
        py -3.10 -m pip install --no-index --find-links wheelhouse ^
          --target src-tauri/lib/python310/Lib/site-packages ^
          pyannote.audio==3.1.1
        
        ```
        
4.  Whisper CLI 和模型
    
    -   `whisper-cli.exe` 放到 `src-tauri/lib/whisper/`
        
    -   复制一份命名为 `w-cli.exe`，作为默认可执行路径
        

## 遇到的问题与解决

-   `py -3.10` 找不到：安装 Python 3.10 解决
    
-   `docopt`/`antlr4-python3-runtime`/`julius` 没有 wheel：允许源码包下载
    
-   pip 解析冲突：升级 pip / 约束版本 / 单独下载源码包
    
-   Windows 目录删除失败：文件被占用，需要先关闭相关进程再删
    

## 安全提醒

-   HF\_TOKEN 不应该写进代码或日志，建议只放到环境变量中并定期重置。
    

## 明日计划

-   完成 Tauri 侧 pipeline 调用联调
    
-   测试一条完整音频流程
    
-   缩减打包体积（清理旧 Python 目录）
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->





# 合约优化记录

## 现有合约概览（Memo.sol）

-   结构体 `Message`：`string text` + `address author` + `uint40 timestamp`
    
-   主要常量：`MAX_MESSAGE_LENGTH = 280`、`MAX_PAGE_SIZE = 50`
    
-   主要功能：
    
    -   `postMessage`：校验长度后写入 `messages` 数组
        
    -   `messageCount`：返回总数
        
    -   `getMessage`：单条读取
        
    -   `getMessages`：分页读取（返回三个并行数组）
        

> 现有代码位置：`0xMemo/src/Memo.sol`

## 当前存储与读写特征

-   `Message` 中 `string` 为动态类型：仅在结构体槽中保存指针，实际内容在单独的存储区域。
    
-   `address` + `uint40` 可以打包进同一个 32 字节槽（节省一槽），但仍需要为 `string` 单独付出存储成本。
    
-   `postMessage` 是主要存储写入点；读取函数只读，不写。
    

## 优化方向与尝试记录

### 1) 减少存储操作（SSTORE 次数）

**目标**：减少 `postMessage` 的存储写入成本或降低写入频率。

可能方案：

-   **事件替代存储（Log 方案）**：
    
    -   将消息只写入事件（`emit MessagePosted(...)`），链上不保存 `messages`。
        
    -   优点：存储成本大幅降低。
        
    -   缺点：链上无法直接分页读取历史消息，需要靠索引服务；不满足链上可查询需求时才可用。
        
-   **仅存储摘要/哈希**：
    
    -   链上存 `keccak256(text)` + `author` + `timestamp`，原文只存在链下。
        
    -   优点：大幅减少存储。
        
    -   缺点：链上不可取回原文。
        

结论：如果必须链上可读全文，无法完全避免 `string` 的存储成本，只能在结构设计上折中。

### 2) 位压缩 / 结构体打包

**目标**：在不改变业务语义的前提下减少存储槽。

当前结构：

-   `string`：独立存储区
    
-   `address + uint40`：可打包为 1 槽（已天然满足）
    

可尝试的结构变体：

-   **短文本内联存储**：
    
    -   例如 `bytes32 textShort` + `uint8 textLen`，仅支持 32 字节内短消息。
        
    -   若需要更长文本，需额外存储或使用映射，逻辑复杂。
        
-   **变体存储（Dual Storage）**：
    
    -   `bytes32` 存短文本；超过阈值再走 `string`。
        
    -   读取逻辑更复杂，但可显著降低大量短消息的成本。
        

结论：位压缩在 `address+uint40` 已经完成，收益点主要在文本存储策略。

### 3) 循环与读取优化

**目标**：降低 `getMessages` 的读取 gas（虽为 view，但在链上调用仍计 gas）。

可尝试：

-   **缓存** `messages` **到局部变量**（避免重复 SLOAD）：
    
    -   例如 `Message[] storage msgs = messages;` 再读取。
        
-   `unchecked` **计数器**：
    
    -   在 `for` 中使用 `unchecked { ++i; }`，避免溢出检查。
        
-   **避免临时拷贝**：
    
    -   当前 `Message memory msgItem = messages[offset + i]` 会触发结构体拷贝；
        
    -   可改为按字段读：`Message storage m = messages[offset + i];` 再逐字段赋值。
        

结论：这些优化对 view 调用收益有限，但在链上调用仍可节省少量 gas。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->






## Gas 优化

> 本笔记用于记录在 Solidity / EVM 开发实习过程中，对 Gas 成本产生原因、常见优化技巧以及实战案例 的理解与总结。

* * *

## 一、Gas 原理理解与思考

### 1\. 什么是 Gas

-   **Gas** 是以太坊中衡量计算和存储资源消耗的计量单位
    
-   每一条 EVM 指令（opcode）都有固定或动态的 Gas 成本
    
-   用户实际支付费用：
    

```
实际费用 = Gas Used × Gas Price

```

Gas 的存在目的是：

-   防止无限计算（DoS）
    
-   激励矿工 / 验证者执行交易
    
-   让链上资源以“市场化”的方式被使用
    

* * *

### 2\. Gas 主要消耗来源

### （1）存储（Storage）

-   `SSTORE`、`SLOAD` 是 **最昂贵** 的操作
    
-   写入 storage（尤其是从 `0 → 非0`）消耗极高
    
-   读取 storage 也远比内存昂贵
    

> 结论：减少 storage 读写是 Gas 优化的核心目标

### （2）计算（Computation）

-   算术运算、条件判断、函数调用
    
-   虽然单次便宜，但在循环中会被放大
    

### （3）内存（Memory）

-   比 storage 便宜，但比 stack 贵
    
-   动态扩容 memory 会产生额外 Gas
    

### （4）调用与数据拷贝

-   外部调用（`call` / `delegatecall`）
    
-   calldata ↔ memory 拷贝
    

* * *

### 3\. Gas 优化的基本原则

1.  **少用 storage，多用 memory / calldata**
    
2.  **减少写操作，合并写操作**
    
3.  **用计算换存储**（在合理范围内）
    
4.  **避免无意义的循环与重复计算**
    
5.  **写给 EVM 看的代码，而不仅是给人看**
    

* * *

## 二、常见 Gas 优化技巧

### 1\. 减少存储操作

### （1）缓存 storage 到 memory

```solidity
uint256 temp = myStorageVar; // 只读一次 storage
for (...) {
    use(temp);
}

```

避免在循环中多次读取同一个 storage 变量。

### （2）合并写入

```solidity
// 不推荐
count++;
total++;

// 推荐：一次性写回
uint256 _count = count;
uint256 _total = total;
_count++;
_total++;
count = _count;
total = _total;

```

* * *

### 2\. 使用位压缩（Bit Packing）

Solidity 的 storage slot 大小为 **32 字节**，可以在同一个 slot 中存放多个小类型变量。

```solidity
struct User {
    uint128 balance;
    uint64  lastLogin;
    uint64  level;
}

```

优势：

-   多个变量共用一个 storage slot
    
-   减少 `SLOAD` / `SSTORE` 次数
    

注意：

-   顺序很重要（从大到小排列）
    
-   只适合数据范围可控的场景
    

* * *

### 3\. 循环优化

### （1）缓存数组长度

```solidity
uint256 len = arr.length;
for (uint256 i = 0; i < len; i++) {
    ...
}

```

避免每次循环都读取 `arr.length`。

### （2）使用 `unchecked`

```solidity
unchecked {
    i++;
}

```

在明确不会溢出的情况下，跳过 Solidity 0.8+ 的溢出检查。

### （3）避免链上大循环

-   能 off-chain 处理的逻辑尽量 off-chain
    
-   使用分页、批量提交
    

* * *

## 三、实战：链上留言板 Gas 优化

### 1\. 需求描述

-   用户可以在链上发布留言
    
-   支持查询历史留言
    

* * *

### 2\. 优化前实现（存在问题）

```solidity
contract MessageBoard {
    string[] public messages;

    function leaveMessage(string memory msg) public {
        messages.push(msg);
    }
}

```

### 存在的问题

1.  `string` 是动态类型，存储成本高
    
2.  每条留言完整存入 storage
    
3.  无限制增长，长期 Gas 成本不可控
    

* * *

### 3\. 优化后实现

### 优化思路

-   留言内容不上链，仅存 **hash / 简要信息**
    
-   使用 `event` 存储完整内容（便宜且可索引）
    
-   减少 storage 写入
    

### 优化后代码示例

```solidity
contract MessageBoardOptimized {
    event Message(address indexed user, string content);

    uint256 public messageCount;

    function leaveMessage(string calldata msg) external {
        emit Message(msg.sender, msg);
        unchecked {
            messageCount++;
        }
    }
}

```

* * *

### 4\. 优化效果总结

| 对比项 | 优化前 | 优化后 |
| --- | --- | --- |
| Storage 写入 | 高（string 数组） | 极低（仅计数） |
| 单次 Gas | 高 | 明显降低 |
| 可扩展性 | 差 | 好 |
| 可读性 | 一般 | 良好 |
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->







````markdown
# 实际完成内容
- 阅读 `Memo` 合约源码，理解 `Message` 结构体与消息存储方式
- 梳理 `postMessage` 写入流程及长度校验的边界条件
- 分析 `getMessage` / `getMessages` 的读取策略与分页限制

# 技术细节

**1 消息数据结构与存储方式**  
用途：定义消息内容与作者信息，使用动态数组维护历史消息。  
```solidity
struct Message {
    string text;
    address author;
    uint40 timestamp;
}

Message[] private messages;
```

**2 消息发布写入流程**  
用途：限制消息长度与空消息，保证写入数据合规，并记录作者与时间戳。  
```solidity
function postMessage(string calldata text) external {
    uint256 length = bytes(text).length;
    require(length > 0, "Empty message");
    require(length <= MAX_MESSAGE_LENGTH, "Message too long");

    messages.push(
        Message({text: text, author: msg.sender, timestamp: uint40(block.timestamp)})
    );
}
```

**3 单条消息读取与边界保护**  
用途：确保读取索引合法，避免越界访问。  
```solidity
function getMessage(uint256 index) external view returns (string memory, address, uint40) {
    require(index < messages.length, "Index out of bounds");
    Message memory msgItem = messages[index];
    return (msgItem.text, msgItem.author, msgItem.timestamp);
}
```

**4 分页读取与限制策略**  
用途：限制分页参数范围、避免过大返回，同时支持越界偏移安全返回空数组。  
```solidity
function getMessages(
    uint256 offset,
    uint256 limit
) external view returns (string[] memory, address[] memory, uint40[] memory) {
    uint256 total = messages.length;
    if (offset >= total) {
        return (new string[](0), new address[](0), new uint40[](0));
    }
    require(limit > 0, "Limit is zero");
    require(limit <= MAX_PAGE_SIZE, "Limit too large");
    ...
}
```

# 遇到的问题
- `getMessages` 对 `offset >= total` 的处理方式是直接返回空数组，而不是 revert，需在前端或调用方明确预期返回语义，避免误判为异常情况。

# 收获与总结
- 理解了该合约采用“写入严格校验、读取容错返回”的设计思路：写入路径强校验，读取路径对越界做安全返回。
- 对消息类合约的分页读取策略有了更清晰认知，尤其是 `MAX_PAGE_SIZE` 与 `limit` 组合限制在防止滥用和优化 gas 的作用。
````

[https://github.com/MapleCity1314/0x-presto-memo/tree/main](https://github.com/MapleCity1314/0x-presto-memo/tree/main)
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->








**ERC-8004**是一个关于\*\*信任最小化代理人（Trustless Agent, TA）\*\*的标准，它提出了在以太坊或类似区块链网络中如何定义和实现去中心化代理人。这个标准主要用于为智能合约和区块链上的去中心化应用（dApp）提供可信的代理人架构。ERC-8004定义了如何通过去中心化的方式管理和操作信任最小化代理，确保代理人能够在没有第三方信任的情况下执行任务。

### **ERC-8004的关键目标**

1.  **去中心化执行**：通过使用智能合约和区块链技术，代理人能够在无需第三方的情况下执行任务。
    
2.  **信任最小化**：设计目标是使所有的操作都能保证在没有信任中介的情况下验证和执行。
    
3.  **可扩展性**：ERC-8004旨在允许开发者根据需求扩展代理的功能，可以支持各种任务的执行，例如数据验证、合同管理、交易处理等。
    
4.  **兼容性**：ERC-8004要求其标准能够与现有的区块链和代币标准（如ERC-20, ERC-721等）兼容，确保广泛的适用性。
    

### **ERC-8004的核心组成部分**

ERC-8004标准主要包括以下几个核心部分：

### 1\. **代理人注册（Agent Registration）**

-   代理人需要在区块链上注册并获得唯一的标识符（如DID - Decentralized Identifier）。该过程确保代理人是可追溯和可信的。
    
-   注册的代理人可以是任何执行任务的实体，如智能合约、自动化程序、去中心化自治组织（DAO）等。
    

### 2\. **代理人行为定义（Agent Action Definition）**

-   代理人可以执行的任务或行为必须是通过智能合约清晰定义的。这些行为可能包括：
    
    -   执行合同条款
        
    -   数据验证与签名
        
    -   执行去中心化的金融（DeFi）操作
        
    -   进行交易确认
        
-   每个行为都是一个智能合约函数，能够确保行为的执行是在无信任情况下进行的。
    

### 3\. **身份验证（Authentication）**

-   每个代理人通过去中心化身份（DID）来进行身份验证。通过使用**DID**，代理人可以确保它们的身份不会被篡改或伪造。
    
-   代理人的身份验证通常由数字签名、密钥对等加密机制支持。
    

### 4\. **事件和日志（Event and Logging）**

-   ERC-8004要求代理人的行为必须被记录和存证。这意味着，所有的代理行为都应该产生链上的事件，并且日志信息是不可篡改的。
    
-   通过这种方式，所有代理人执行的任务可以被追踪、审计和验证。
    

### 5\. **任务请求和响应（Task Request and Response）**

-   其他智能合约或外部系统可以向代理人发送任务请求。代理人接受请求后，会根据任务定义和授权条件来执行任务。
    
-   任务执行后，代理人会返回执行结果，所有这些操作都必须通过智能合约来确保无信任验证。
    

### 6\. **奖励和惩罚机制（Incentives and Penalties）**

-   代理人完成任务后可以获得奖励（例如，代币奖励、服务费等）。同样，如果代理人未能完成任务或违反协议，也可能受到惩罚。
    
-   ERC-8004建议使用**去中心化激励机制**，例如通过**ERC-20代币**支付奖励。
    

### 7\. **合约执行和治理（Contract Execution and Governance）**

-   代理人执行的合约通常包含预定义的规则，确保所有参与方按照共识执行协议。ERC-8004通常包括治理机制，用于在代理人执行过程中进行决策、更新合约等。
    

### **ERC-8004的智能合约接口**

ERC-8004标准定义了与代理人相关的一些基本接口和函数。以下是一些典型的接口方法：

1.  **registerAgent(address agentAddress, string agentType)**
    
    -   注册一个新的代理人，并为其分配唯一的地址和类型（例如去中心化身份或角色）。
        
2.  **executeTask(address agent, string taskType, bytes data)**
    
    -   代理人执行某个任务。输入任务类型和相关数据后，代理人按照协议执行任务。
        
3.  **getAgentStatus(address agentAddress)**
    
    -   查询代理人的状态，例如它是否在线、是否准备好接受任务等。
        
4.  **rewardAgent(address agentAddress, uint256 amount)**
    
    -   向代理人支付奖励。奖励通常为代币或其他形式的支付。
        
5.  **penalizeAgent(address agentAddress, uint256 penaltyAmount)**
    
    -   对于未能按时完成任务的代理人，进行惩罚。
        
6.  **logAgentAction(address agentAddress, string actionType, string details)**
    
    -   记录代理人的行为，包括任务执行情况、状态更新等。
        

### **ERC-8004的应用场景**

ERC-8004主要应用于以下几个领域：

1.  **去中心化金融（DeFi）**
    
    -   代理人可以用来自动化资产管理、交易执行、去中心化借贷等行为。
        
2.  **自动化合约执行**
    
    -   在智能合约中，代理人可以执行合约条款，比如在某个特定条件满足时执行支付、数据验证等操作。
        
3.  **去中心化身份管理**
    
    -   使用DID（去中心化身份）来代表代理人，确保其身份是可验证的且不能被篡改。
        
4.  **DAO（去中心化自治组织）**
    
    -   在DAO中，代理人可以执行治理决策和执行协议条款。
        
5.  **数据验证与验证系统**
    
    -   代理人可以被用来验证某些外部事件或数据，如交易的合法性、协议的合规性等。
        

### **ERC-8004与其他标准的结合**

ERC-8004标准设计时考虑到与其他ERC标准的兼容性，尤其是：

-   **ERC-20**：用于奖励代理人或支付手续费。
    
-   **ERC-721**：可用于表示特定任务或活动的非同质化代币。
    
-   **ERC-1155**：可以在一个合约中同时管理多种类型的代币。
    
-   **ERC-223/777**：可能用于支付过程中的代币转账。
    

* * *
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->









Uniswap是一个基于以太坊区块链的去中心化交易所（DEX），使用自动化做市商（AMM）模型，让用户能够在没有中心化交易平台的情况下进行代币交易。下面是Uniswap的简单入门笔记：

### 1\. **什么是Uniswap？**

Uniswap 是一个去中心化的交易协议，它通过智能合约来提供交易服务，允许用户无需通过传统的订单簿系统即可进行代币交换。它通过自动化做市商（AMM）算法来实现代币交易，而不需要订单簿。其目的是提供无需信任的、无需许可的交易方式。

### 2\. **Uniswap如何工作？**

Uniswap 使用的是自动化做市商（AMM）模型，而不是传统的基于订单簿的交易系统。AMM通过提供流动性池（Liquidity Pool）来支持交易。每个流动性池由两种代币组成，例如ETH和USDT。交易者可以通过这些池进行交换，而流动性提供者（LP）则提供资金来维护这些池，并通过交易费获得奖励。

-   **流动性池：** 每个池包含两种代币，池中的代币按一定比例存入（例如1:1比例）。每次交易会对池中的代币比例进行微小调整。
    
-   **自动化做市商：** 通过智能合约自动执行买卖交易，不依赖于买家和卖家的相互匹配。价格根据池中两种代币的比例自动计算。
    

### 3\. **如何使用Uniswap进行交易？**

1.  **连接钱包：** 使用如MetaMask等钱包连接Uniswap。
    
2.  **选择代币：** 选择你想交换的代币，输入金额。
    
3.  **确认交易：** 查看交易的汇率和预计的手续费，确认交易。
    
4.  **执行交易：** 钱包会弹出提示，确认交易后就会执行，交易完成后你会收到目标代币。
    

### 4\. **如何成为流动性提供者？**

作为流动性提供者，你需要将两种代币存入Uniswap的流动性池。例如，你可以选择ETH和USDT的流动性池，按比例存入这两种代币。作为回报，你将获得池中的流动性提供者代币（LP代币），并根据你在池中所占的比例获得交易费。

### 5\. **风险与注意事项**

-   **无常损失（Impermanent Loss）：** 如果池中的两种代币价格波动很大，你可能会遇到无常损失，即你提供的流动性在一段时间后可能会导致你亏损。
    
-   **交易费用：** 每次交易都会产生交易费用（通常为0.3%），这笔费用会分配给流动性提供者。
    
-   **代币的安全性：** 与所有DeFi项目一样，Uniswap的智能合约可能存在漏洞，所以需要谨慎操作。
    

### 安装依赖

首先，我们需要安装`web3.js`库来与以太坊区块链进行交互。可以通过以下命令安装：

```bash
npm install web3
```

### 设置Web3连接

在代码中，我们需要连接到以太坊网络，通常我们会使用像Infura这样的服务来连接到以太坊主网。

```javascript
const Web3 = require('web3');

// 使用Infura或其他服务提供的RPC URL
const web3 = new Web3(new Web3.providers.HttpProvider('https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID'));
```

### 获取账户信息

我们假设你已经在MetaMask或其他钱包中有一个账户。通过web3.js，我们可以获取钱包的地址。

```javascript
// 获取账户地址
async function getAccount() {
  const accounts = await web3.eth.getAccounts();
  return accounts[0]; // 获取第一个账户
}

getAccount().then(account => {
  console.log('当前账户地址：', account);
});
```

### 与Uniswap智能合约交互

Uniswap的智能合约是基于以太坊ERC-20标准的代币交换协议，因此我们可以通过合约地址和ABI与其交互。

1.  **Uniswap的合约地址**：  
    Uniswap的V2合约地址（比如ETH和USDT的交易对）可以在etherscan上找到。你需要在实际代码中填写正确的合约地址。
    
2.  **ABI**：ABI是合约的应用二进制接口，它定义了如何与智能合约进行交互。在Uniswap的情况下，ABI可以在Uniswap的GitHub上找到，或者你可以从etherscan获取。
    

假设我们要交易ETH和USDT的代币对：

```javascript
const uniswapRouterABI = [ /* Uniswap Router合约ABI */ ];
const uniswapRouterAddress = '0x5C69bEe701ef814a2B6a3EDD47dB8fD5d42a08b2'; // Uniswap V2的Router合约地址

const uniswapRouter = new web3.eth.Contract(uniswapRouterABI, uniswapRouterAddress);

// 交换ETH为USDT的函数
async function swapETHForUSDT(amountInETH) {
  const account = await getAccount();
  
  // ETH转为Wei，1ETH = 10^18 Wei
  const amountInWei = web3.utils.toWei(amountInETH.toString(), 'ether');
  
  const path = ['0xC02aaA39Cc39cB917f28A9C8aD9bF5014b7F8F7B', '0xdac17f958d2ee523a2206206994597c13d831ec7']; // WETH -> USDT 地址
  const to = account; // 交易接收地址
  const deadline = Math.floor(Date.now() / 1000) + 60 * 20; // 20分钟后的截止时间
  
  const tx = await uniswapRouter.methods.swapExactETHForTokens(
    0, // 最小输出数量，设为0表示接受任何数量
    path,
    to,
    deadline
  ).send({
    from: account,
    value: amountInWei
  });
  
  console.log('交易成功！', tx);
}

// 调用函数进行交易
swapETHForUSDT(0.1); // 例如交换0.1 ETH为USDT
```

### 解释：

1.  **合约ABI和地址**：我们用Uniswap的合约地址和ABI创建了一个`uniswapRouter`对象，这样我们可以调用Uniswap的`swapExactETHForTokens`方法。
    
2.  **交易路径**：我们设置了ETH和USDT的交易路径，`path`数组中包含了两种代币的地址——WETH（Wrapped ETH）和USDT。
    
3.  **交换ETH为USDT**：在调用`swapExactETHForTokens`时，我们传入了ETH的数量、交易路径、接收地址和交易截止时间。
    
4.  **发送交易**：通过调用`send()`方法，我们发起了交易。此时，钱包会要求用户签名并发送交易。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->










\# 深度思考：Web3 + AI 的终局——隐私计算与无许可代理（Trustless Agents）

\## 1. 核心论点：从“工具”到“经济主体”的跃迁

在 Web2 时代，AI 是 **SaaS（软件即服务）**，是人类手中的工具；

在 Web3 + AI 时代，AI 将变成 **Agent（代理）**，成为独立的 **经济主体（Economic Actor）**。

这个转变的核心矛盾在于：\*\*如果 AI 要拥有资产、执行交易、签署合同，它需要什么样的基础设施？\*\*

目前的公链（如 Ethereum, Solana）是透明的，而目前的 AI 模型（如 GPT-4）是中心化且黑盒的。两者直接硬拼凑无法解决“信任”问题。

这就是 **Trustless Agent** 诞生的背景，也是 **Oasis** 这类隐私计算网络存在的根本原因。

\---

\## 2. 关键拼图一：Trustless Agent（无许可代理）

**什么是 Trustless Agent？**

它不仅仅是一个能聊天的 Bot，而是一个拥有 Crypto Wallet（钱包）和 Private Key（私钥）的链上实体。它可以：

\* 自主发现套利机会。

\* 自主支付 API 费用。

\* 与其他 Agent 进行资源交换（算力换数据）。

**存在的痛点：**

目前的 Agent 大多是“虚假去中心化”的。由于区块链的公开性，你不敢把私钥交给一个跑在公链上的智能合约（因为私钥会由 Validator 看到）。因此，大多数 Agent 的私钥还是存在中心化的服务器里。这这就不是 Trustless，而是 Custodial（托管的）。

**思考沉淀：**

\> 如果 AI Agent 不能在非托管的情况下持有私钥，那么 Web3 + AI 就只是伪命题。真正的 Trustless Agent 必须是在加密环境中运行代码，外界无法窥探其内部逻辑和状态。

\---

\## 3. 关键拼图二：Oasis 与 TEEs（隐私计算层）

这里是 **Oasis Network** 发挥核心作用的地方。

**技术逻辑：TEEs (Trusted Execution Environments)**

Oasis（特别是 Sapphire 平行链）利用 TEE 技术（如 Intel SGX）。简单来说，就是在 CPU 里划出一块“黑盒子”。

1\. **数据进，黑盒子关上。**

2\. **计算发生（模型推理、私钥签名）。**

3\. **结果出，黑盒子销毁/重置。**

4\. **生成证明（Proof）上链。**

**Oasis 的价值捕获：**

Oasis 最近推出的 **ROFL (Runtime Off-chain Logic)** 框架非常关键。它允许繁重的 AI 计算在链下（Off-chain）的 TEE 中运行，但通过共识机制保证其结果的可验证性。

**应用场景推演：**

\* **私有 DEX 交易员：** Agent 可以在 TEE 里运行策略，链上只看到买卖结果，看不到策略逻辑（防止被 MEV 攻击）。

\* **私密意图（Private Intents）：** 用户告诉 Agent “帮我买那个币，但别告诉别人我是谁”，Oasis 处理意图匹配，保护隐私。

\---

\## 4. 关键拼图三：Scoop / 0xScope (数据与认知层)

_注：假设你提到的 scoop 指向的是数据洞察或 0xScope 类的 AI 数据层。_

如果 Agent 是“执行者”，Oasis 是“安全屋”，那么 **Data Layer** 就是“燃料”。

**Web3 AI 的数据困境：**

目前的 LLM 是基于 Web2 数据训练的。Web3 的链上数据（交易、持仓、关系）是高度结构化但极其复杂的图谱。

**AI 甚至比人更需要“清洗过的数据”：**

Trustless Agent 不会去读 Etherscan 的网页，它需要像 **0xScope** 或 **The Graph** 提供的 API，甚至更进一步——需要去中心化的数据喂养。

**思考沉淀：**

\> AI Agent 的智商取决于数据质量。在 Web3，\*\*“数据主权”\*\* 将变成 **“数据变现”**。未来的模式可能是：用户通过 Oasis 的隐私技术上传个人数据（Scoop data），Agent 支付 Token 购买这些数据的“使用权”进行训练，而无需看到原始数据（Data tokenization + Privacy Preserving Compute）。

\---

\## 5. 融会贯通：未来的架构图景

当我们把这三者（Web3 AI Agent + Oasis Privacy + Data/Scoop）结合起来，会看到一种全新的商业模式：

**“自主代理经济体 (The Autonomous Agent Economy)”**

1\. **身份与钱包：** 一个 AI Agent 初始化，私钥生成在 Oasis 的 TEE 中，任何人都无法导出（甚至开发者自己）。

2\. **资金来源：** Agent 通过发行 DAO Token 或借贷获得初始资金。

3\. **数据摄入 (Scoop)：** Agent 付费调用 0xScope 或 Dune 的数据接口，分析链上趋势。

4\. **决策与执行：**

\* Agent 决定买入某个资产。

\* 计算在 TEE 内完成，策略不泄露。

\* 通过 TEE 内的私钥签署交易，广播到以太坊或 Solana。

5\. **协作：** Agent A (分析师) 将结果卖给 Agent B (交易员)。

\### 风险与反思

\* **算力瓶颈：** TEE 的性能远不如 H100 GPU 集群。目前的 Web3+AI 只能做轻量级推理（Inference），无法做大规模训练（Training）。

\* **安全性边界：** 硬件级别的漏洞（如 TEE 侧信道攻击）是最大的黑天鹅风险。

\* **监管地狱：** 如果一个无许可的 AI Agent 用 TEE 隐藏身份并进行洗钱或攻击，法律责任谁来负？

\## 6. 总结与投资/建设视角

对于这个赛道的思考，不应局限于“哪个币会涨”，而应关注\*\*架构的合理性\*\*：

1\. **去中心化算力（Render, Akash, Gensyn）** 解决的是“生产力”问题。

2\. **数据层（Graph, 0xScope, Ocean）** 解决的是“原材料”问题。

3\. **隐私计算层（Oasis, Phala, Secret）** 解决的是“安全性与自主性”问题。

**Oasis + Trustless Agent 是最性感的叙事之一**，因为它真正赋予了 AI “生命”——即\*\*持有私产和保守秘密的能力\*\*。

如果你在关注这个领域，建议重点观察 **Oasis 的 ROFL 框架进展** 以及基于此构建的 **Agent 基础设施（如 OPL - Oasis Privacy Layer）** 的实际采用率。这可能是 Web3 乃至 AI 领域下一个周期的 Alpha 所在。“  
”  
希望下一次周会能够参与Trustless Agent的讨论
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->











### 传统LLM的核心局限

-   2023年的大语言模型本质上是自回归投影预测器,只能进行文本生成
    
-   传统LLM存在三个致命限制:
    
    -   **无状态**:每次对话从零开始,不记得用户身份和历史对话,早期上下文窗口仅32K-64K
        
    -   **无工具**:只能生成文本,无法执行代码、调用API或访问外部系统
        
    -   **无身份**:不能代表任何人或组织行动
        

### Agent AI的技术突破

-   2026年的Agent AI突破了传统限制:
    
    -   通过向量库实现持久化记忆
        
    -   通过Function Calling和MCP实现工具调用能力
        
    -   通过ERC-8004获得链上身份
        
-   Gartner预测到2028年,33%的企业应用将集成Agent AI(2024年该比例不到1%)
    
-   2025年被定义为"AI问责元年" - 当AI代替人类签合同、花钱、做决策时,法律责任归属成为关键问题
    

### 完整AI Agent的核心组件

-   **大脑(AI模型)**:
    
    -   主流模型包括GPT-4.2、DeepSeek、Claude等
        
    -   国产DeepSeek支持本地化部署
        
    -   负责理解意图、制定计划、生成行动指令
        
-   **记忆(向量数据库)**:
    
    -   通过FireCrawl等工具采集网页文档数据
        
    -   使用Embedding模型(如OpenAI TextEmbedding、清文塔Embedding)将文本转换为高维向量
        
    -   存储到Pinecone、Weaviate、Milvus等向量数据库
        
    -   用户提问时通过相似度搜索检索相关信息并放入Prompt
        
-   **工具(API接口/Function Calling/MCP)**:
    
    -   定义函数集(发邮件、查数据库、执行代码等)
        
    -   LLM判断需要调用函数时输出结构化调用请求
        
    -   程序执行函数并返回结果给LLM继续生成
        
    -   形成"思考-行动-观察"的ReAct循环
        

### 当前互联网架构的三大基础设施缺口

-   **身份欺诈问题**:
    
    -   HTTP协议无法验证请求方是人类还是AI
        
    -   案例:秘书服务公司Ripley揭露的商业间谍案 - 竞争对手伪装成合法自动化工具渗透内部系统窃取数据
        
    -   缺乏AI Agent的身份基础设施,任何人都可以声称是某公司的Agent
        
-   **审计追溯问题**:
    
    -   AI代表企业签署合同和交易时需要不可篡改的日志
        
    -   传统系统日志存储在本地服务器,可被修改、删除或伪造
        
    -   BEC法律专家指出:AI错误签署合同导致损失时,责任难以追溯
        
-   **影子AI问题**:
    
    -   Durios调研显示59%的组织在Agent AI安全方面缺乏统一治理框架
        
    -   大量AI Agent以"游击队"方式在企业内部运行
        
    -   IT部门不知道这些AI何时访问数据、做了什么决定、发送了什么邮件
        

### AI Agent的支付困境

-   **身份验证障碍**:
    
    -   传统注册流程(验证码、KYC、人脸识别)无法被AI完成
        
    -   CAPTCHA系统设计初衷就是证明"你不是机器人"
        
    -   AI没有身份证和人脸,无法通过KYC验证
        
-   **支付颗粒度问题**:
    
    -   信用卡交易有最低成本 - 手续费2-3%加固定处理费约0.3美元
        
    -   AI Agent的单次API调用可能仅消耗0.001美元
        
    -   传统金融体系无法处理如此小额的微支付
        
-   **结算速度问题**:
    
    -   传统支付结算周期为T+1到T+3,跨境支付更长
        
    -   AI Agent交易以毫秒计,可能一秒内调用十个不同API并分别付费
        
    -   等待三天结算完全不可行
        
-   **订阅模式错配**:
    
    -   传统SaaS采用月度或年度订阅模式
        
    -   AI Agent工作负载高度动态 - 有些每天处理海量请求,有些大部分时间闲置
        
    -   固定订阅模式不适合按需使用的场景
        

### Web3解决方案概述

-   **核心价值**:
    
    -   AI Agent在Web3上不需要KYC,有钱包地址即可
        
    -   支持微支付(0.001美元级别)
        
    -   结算速度快(毫秒级)
        
    -   7×24小时运转
        
-   **两大核心能力**:
    
    -   不可伪造的身份证(ERC-8004)
        
    -   可编程的银行账户(区块链钱包)
        

### X-402支付协议详解

-   **协议设计**:
    
    -   将支付能力嵌入HTTP协议本身
        
    -   利用HTTP 402状态码(Payment Required) - 该状态码在HTTP 1.1规范中定义但数十年来一直是保留状态
        
    -   服务器返回402时,响应头包含钱包地址、链信息和代币类型
        
    -   Agent钱包自动签署交易、发送付款、附上链上支付证明、重新发起请求
        
-   **支付流程**:
    
    1.  客户端向服务端发起请求
        
    2.  服务端返回402状态码,包含支付要求(金额、代币类型、链/网络信息)
        
    3.  客户端发起链上交易并获取交易哈希
        
    4.  服务端验证后提供服务
        
-   **支付证明验证方案**:
    
    -   **方案一(链上查询)**:服务器查询区块链确认交易哈希和转账确实存在,需等待几秒确认
        
    -   **方案二(签名验证)**:客户端签署付款承诺,服务器验证签名后先提供服务后异步结算,速度更快但需信任机制防止反悔
        
-   **成本优化方案**:
    
    -   **状态通道**:双方在链上锁定押金,链下多次交易,最后结算时把净额上链(类似最后AA总账)
        
    -   **批量结算**:累积小额交易达到一定金额后一次性结算
        
-   **支持的支付方式**:稳定币、原生代币、Layer 1/Layer 2(如Base、Arbitrum等,Gas费更低、确认速度更快)
    
-   **应用场景**:
    
    -   按量计费API(每千token 0.01美元)
        
    -   数据库查询付费
        
    -   内容付费墙(AI Agent获取付费文章/数据)
        

### ERC-8004身份标准详解

-   **设计理念**:
    
    -   以太坊社区专门为AI Agent设计的身份标准
        
    -   基于ERC-721 NFT标准实现
        
    -   为每个AI铸造唯一、可转移的链上身份Token
        
-   **三大核心注册表**:
    
    -   **主注册表(身份注册表)**:
        
        -   每个Agent身份是一个NFT,具有全局唯一性、可验证性、可转移性、可编程性
            
        -   包含AI类型(通用助手/专业代理/自动化交易员)
            
        -   能力声明(签署交易/执行代码等)
            
        -   运行模型哈希(用于验证)
            
    -   **声誉注册表**:
        
        -   1-100分评分系统,防刷分设计
            
        -   只有链上有转账记录的地址才能评价
            
        -   评价者需支付Gas费
            
        -   评价权重与交易金额挂钩
            
        -   评分不可篡改,永久存在
            
        -   时间衰减机制 - 近期评价权重更高,让AI有改过自新的机会但不能抹去黑历史
            
    -   **验证注册表**:
        
        -   支持TEE(可信执行环境):使用Intel SGX或ARM TrustZone等硬件安全模块,AI在TEE内运行并生成知识证明
            
        -   支持zkML(零知识机器学习):用零知识证明技术证明使用某模型对输入做推理得到输出,不泄露模型参数和输入数据
            

### SpawnOS框架架构

-   **定位**:
    
    -   AI Agent的"安卓系统"
        
    -   将底层复杂功能(钱包管理、身份验证、支付协议、交易签名)封装为简单接口
        
    -   开发者专注于Agent逻辑、工具调用、算法和Prompt,无需处理Web3细节
        
-   **核心功能**:
    
    -   钱包托管服务 - 实现复杂签名算法
        
    -   一键接入ERC-8004身份系统
        
    -   支持X-402支付流程
        
-   **四层架构**:
    
    -   **数据层(第一层)**:
        
        -   支持各种Web3工具(以太坊浏览器、链上数据获取)
            
        -   向量数据库和RAG支持
            
        -   文档清洗、分块、Embedding生成
            
    -   **核心LLM层(第二层)**:
        
        -   统一LLM接口(支持OpenAI、Anthropic、Google、DeepSeek等)
            
        -   智能体编排引擎(类似LangGraph) - 处理复杂任务的多Agent协作
            
        -   记忆管理器(短期记忆如对话上下文,长期记忆通过向量数据库)
            
        -   工具调用器(管理Function Calling和工具执行)
            
        -   支持多种Agent架构:单体Agent、Router模式(任务分发)、协作模式(多Agent共识)
            
    -   **协议层(第三层)**:
        
        -   ERC-8004标准实现(身份注册、声誉更新、证明验证)
            
        -   Payment Gateway实现X-402协议(处理支付请求、签署交易、验证支付证明)
            
        -   钱包托管服务和安全存储私钥
            
        -   TEE验证支持
            
    -   **应用层(第四层)**:
        
        -   DeFi连接器(连接UniSwap、DEX等)
            
        -   跨链桥支持
            
-   **开发效率**:Python开发者使用SpawnOS可在两天内让AI Agent具有Web3能力,自己造轮子可能需要数月
    

### 监管合规要求

-   **欧盟AI法案(2024年生效)**:
    
    -   全球首个综合性AI监管法规
        
    -   AI系统分为四个风险等级:
        
        -   不可接受风险(社会评分系统、远程生物识别)
            
        -   高风险(金融信用评估、招聘筛选)
            
        -   有限风险(聊天机器人需说明其非真人)
            
        -   低风险(游戏等)
            
    -   高风险系统要求:建立风控体系、详细活动日志、人类监督、透明性和可追溯性
        
-   **审计要求**:
    
    -   传统AI审计:日志存储在本地服务器,可被修改、删除或伪造
        
    -   区块链审计优势:
        
        -   日志一旦写入无法篡改
            
        -   时间戳由网络共识决定,非企业自定
            
        -   公开透明,任何人可验证
            
        -   永久存储
            
        -   所有资金流、身份变更、权限修改、所有权转移都有记录
            
    -   监管查询时可提供:链上记录(时间戳、交易哈希)+ 链下完整日志,哈希可对应验证
        

### 未来经济展望

-   **稳定币交易规模**:
    
    -   Coinbase报告:稳定币月结算量达3900亿美元
        
    -   Visa全球月交易量仅1万亿美元
        
    -   稳定币交易量已接近Visa的40%,其中很多来自AI Agent或DeFi协议的自动化交易
        
-   **API经济增长**:
    
    -   Postman统计:全球API调用量年增长30%以上
        
    -   API是机器与机器之间的接口,API经济增长即机器经济增长
        
    -   RPA(机器人流程自动化)市场预计2025年达250亿美元
        
-   **AI Agent渗透预测**:
    
    -   2028年33%企业软件将集成Agent AI
        
    -   未来百万级Agent同时在线,每秒产生上万笔交易
        
    -   调用API、购买数据、支付算力、分配利润等都需要结算
        
    -   假设100万Agent同时运行,每分钟10笔交易,信用卡系统无法处理
        
-   **Web3优势**:
    
    -   区块链支持数万TPS,Layer 2更高
        
    -   手续费仅几美分
        
    -   7×24小时不间断
        
    -   区块链将成为AI的"金融操作系统"
        

### 核心总结

-   **三大转变**:
    
    -   从陪聊到打工:从无状态到有记忆(RAG)、从纯文本到动手干活(Function Calling/MCP)、从单次交互到持续任务(ReAct循环)
        
    -   挑战:身份不可验证、行为不可审计、支付不可实现
        
-   **协议方案**:
    
    -   ERC-8004:基于ERC-721的链上唯一ID + 不可篡改声誉评分 + TEE/zkML验证机制
        
    -   X-402:HTTP原生支付协议 + 微支付支持 + 毫秒级结算
        
    -   两个协议结合提供完整工具包:可验证的身份 + 可编程的钱包
        
-   **SpawnOS价值**:
    
    -   将复杂功能封装为简单API
        
    -   钱包托管服务
        
    -   统一接口支持各种LLM和区块链
        
    -   Slogan:为干活的AI打造类似"安卓"的操作系统
        

## 传统 LLM 与 Agent AI 的范式对比与进一步思考

从更宏观的角度看，传统 LLM 更接近一种“语言层的概率引擎”。它擅长在给定上下文中完成高质量的文本补全，却始终停留在**被动响应**这一角色之中：没有长期身份、没有持续记忆、没有独立执行权，也不对现实世界产生直接后果。它的价值主要体现在“辅助人类决策”，而非“替代人类行动”。

Agent AI 则走向了完全不同的方向。  
当记忆被外置并可长期积累、当工具调用成为模型决策链的一部分、当身份与钱包让 AI 能够真实地“签名、付费、交易、履约”，AI 就不再只是软件功能，而开始具备**经济主体的雏形**。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->












## 智能合约开发入门

### 一、 DAPP架构和开发流程

开发 Dapp 需要理解和掌握去中心化技术栈、智能合约编程以及前端与区块链的交互方式。

1.  前端（UI） DApp 前端不会直接连接区块链网络，而是通过 钱包注入的 Provider 或第三方 RPC 节点 与区块链交互。通过RPC Node对智能合约发起 read only调用，获取链上数据 对需要修改状态的操作 前端构造对智能合约的交易调用，交由钱包完成签名后通过RPCNode广播到区块链网络
    
2.  智能合约：智能合约通过执行自动化规则来确保交易和操作的透明性与不可篡改性 在以太坊平台上，智能合约通常使用 **Solidity** 编程语言编写，并通过 **Ethereum Virtual Machine (EVM)** 执行。
    
3.  数据检索器： 智能合约通常以 `Event` 形式释放日志事件，比如释放代表 NFT 转移的 `Transfer` 事件，数据检索器会检索这些数据并将其写入到 PostgreSQL 等传统数据库中
    
4.  区块链和去中心化存储：如IPFS，Arweave
    

## 二、Dapp开发流程

1.  需求分析
    
    -   确定功能需求
        
    -   选择区块链平台
        
    -   设计用户体验（UIUX）
        
2.  智能合约开发
    
    -   编写智能合约
        
    -   编写测试用例 （思考：AI自动审批智能合约？）
        
    -   审计与优化
        
3.  检索器开发
    
    -   确定功能需要的数据内容
        
    -   编写检索器程序
        
    -   部署和运维
        
4.  前端开发
    
    -   选择前端框架
        
    -   链接钱包
        
    -   显示区块链数据
        
    -   处理交易签名与确认
        
5.  与区块链交互
    
    前端和智能合约通过 **Viem（推荐）、Ethers.js 或 Wagmi** 等现代化库进行交互。b
    
6.  部署与上线 可以部署在IPFS或者传统Web服务（如Vercel）
    

## 二、以太坊开发环境搭建

以太坊开发环境的搭建主要有以下几种常用方式，适合不同的开发需求：

### 基础环境准备

我这里使用的是Windows平台 + WSL2，其他环境可能有所不同，命令来自开发手册，后续更新同步我自己博客

```bash
# 安装 nvm（如未装）。推荐参考文档 <https://github.com/nvm-sh/nvm> 安装最新版本
curl -o- <https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh> | bash

# 安装 Node.js LTS
nvm install --lts
nvm use --lts

# 安装 yarn（可选）
npm install -g yarn
```

### 以太坊本地开发

我这里只用了Foundry ，别问，速度才是真理 • [**Foundry 官方文档**](https://getfoundry.sh/introduction/getting-started) - [https://getfoundry.sh/introduction/getting-started](https://getfoundry.sh/introduction/getting-started)

```bash
curl -L <https://foundry.paradigm.xyz> | bash
foundryup
```

Foundry 提供以下以太坊开发工具：

-   `forge`: 帮助构建、测试、调试、部署和验证智能合约
    
-   `anvil`: 本地开发节点，完全兼容以太坊 JSON-RPC 规范
    
-   `cast`: 命令行工具，用于与链上应用交互
    

* * *

环境安装出了点问题 暂时先用remix

## RPC节点

在 Web3 开发中，**RPC（Remote Procedure Call，远程过程调用）** 是连接前端应用与区块链网络的关键桥梁。但RPC并不参与共识过程。在集群中，运行RPC的节点扮演着不同的角色

## JSON-RPC协议
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->













主要内容：web3安全 && 合规思考

## 1\. Web3合规性要求与常见法律风险

> 中国对web3态度严峻，已经明确禁止：禁止ICO；禁止虚拟货币交易所运营与提供撮合服务；明确虚拟货币不具有法偿性，不得作为支付工具；禁止境外交易平台向中国境内居民提供服务；打击以虚拟货币为载体的非法集资，传销，洗钱，赌博等活动

目前对于国内来说，技术栈不被禁止，但业务基本都是红区。

1.  代币发行与交易 当前中国法律明令禁止任何单位或者个人通过ICO， IEO，IDO等方式进行融资活动，只要融资具备流通性或者功能，都可能会构成非法金融行为
    
    **技术人员也会被视为共同行为人**
    
2.  赌博，传销，洗钱等刑事风险
    
3.  场外交易中的洗钱与非法经营风险
    
4.  民商事争议
    

## 2\. 全球监管背景与趋势

> 近年来，全球加密货币合规化趋势日益明显，监管框架稳步推进：欧盟的 MiCA 正在逐步落地；新加坡出台新规，将反洗钱/反恐怖融资要求与 FATF 标准接轨；美国国会今年也出台了《稳定币法案》（GENIUS Act）和《稳定币法案》（STABLE Act），为稳定币发行、反洗钱和消费者保护等提供了清晰的框架。
> 
> 更健全的监管框架能够提供更清晰的指导方针，降低风险，增强用户信心，从而为加密货币行业释放新的增长机遇。越来越多的机构正在转变对合规的认识，不再将其视为障碍，而是将其视为战略优势。通过积极采取 KYC、客户尽职调查、地址筛选和交易监控等措施，他们正在全面显著提升合规能力。
> 
> 然而，区块链的匿名性和链上交互（尤其是跨链活动）的复杂性对机构在风险评估、团队协作和报告生成方面提出了重大挑战。

1.  什么是 FATF **（Financial Action Task Force，金融行动特别工作组）**？ 是一个民间组织，全球反洗钱和反恐怖融资标注制定者
    
2.  稳定币监管的意义
    
    1.  稳定币已成为DeFi生态的基础设施，总市值超过 1500 亿美元。如果主要稳定币出现问题，可能引发系统性风险
        
    2.  稳定币可能影响传统金融体系的货币供应
        
3.  全球主要监管趋势
    
    1.  美国监管框架
        
    2.  欧盟监管框架
        
    3.  香港监管框架
        
    4.  监管趋势以及对web3从业者的影响总结
        
        1.  趋严化
            
        2.  协调化
            
        3.  合规成本增加
            
        4.  创新友好
            
        5.  长期利好
            
        6.  市场准入门槛提高
            

## 3\. Web3 入职法律风险

> 主要是对于项目注册地、薪酬结构、用人主体不明确时，新型的雇佣关系、社保的缴纳、工资的发放形式等等

1.  雇佣关系新形态 基本都是远程办公，由于许多项目方在中国境内无注册公司，不具备用工主体资格，因此难以与员工签订有效的《劳动合同》，也无法依法缴纳五险一金。一旦发生薪资争议或因公受伤，员工将难以依照《劳动合同法》享受合法保障。 为弥补上述漏洞，有些项目方会通过 EOR（名义雇主）模式，委托国内合规公司与员工签署合同。通过这种方式可以解决劳动关系里的部分问题，但在发生纠纷的情况下，劳动关系的认定仍然是需要关注的重点。
    
2.  **薪酬结构及风险提示** 一般为人民币+token或者全USDT模式 ⚠根据我国《劳动法》规定，工资应以法定货币（即人民币）支付，不得以实物或虚拟币等形式代替。这意味着，如果薪酬中 Token 或 USDT 部分被认定为工资支付，可能导致支付行为无效。
    
3.  虚拟货币出金与合规风险 容易构成犯罪
    
4.  项目合法性需提前审查
    

## 4\. Web3 项目的刑事风险及案例介绍

> Web3 项目常常通过跨境分区的方式来规避境内的监管风险，通常为在境外开展敏感业务部分，境内主要负责技术研发。
> 
> 但需要注意的是，即便不是主犯，也可能会被认定为共同犯罪或者帮助犯罪。
> 
> 警惕构成赌博、非法经营、非法集资、传销等的风险。

1.  开设赌场罪，赌博罪
    
2.  非法经营罪
    
3.  非法吸取公众存款罪
    
4.  组织、领导传销活动罪
    
5.  洗钱罪
    

## 资源

1.  区块链安全红手册（项目方）：[\*\*](https://www.slowmist.com/redhandbook/)[https://www.slowmist.com/redhandbook/\*\*](https://www.slowmist.com/redhandbook/**)
    
2.  区块链黑暗森林自救手册：[\*\*](https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook/blob/main/README_CN.md)[https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook/blob/main/README\_CN.md\*\*](https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook/blob/main/README_CN.md**)
    
3.  [Revoke.cash](http://Revoke.cash) 授权管理：[\*\*](https://revoke.cash/)[https://revoke.cash/\*\*](https://revoke.cash/**)
    
4.  Scam Sniffer 钓鱼检测：[\*\*](https://www.scamsniffer.io/)[https://www.scamsniffer.io/\*\*](https://www.scamsniffer.io/**)
    
5.  1Password 密码管理器：[\*\*](https://1password.com/)[https://1password.com/\*\*](https://1password.com/**)
    
6.  Bitwarden 密码管理器：[\*\*](https://bitwarden.com/)[https://bitwarden.com/\*\*](https://bitwarden.com/**)
    
7.  SlowMist Hacked 区块链被黑档案库，大量的案例索引：[\*\*](https://hacked.slowmist.io/)[https://hacked.slowmist.io/\*\*](https://hacked.slowmist.io/**)
    
8.  反钓鱼攻防学习平台：[\*\*](https://unphishable.io/)[https://unphishable.io/\*\*](https://unphishable.io/**)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->














# 概念梳理

### Web1 / Web2 / Web3

Web 的演进本质上是**权力与数据归属的变化**。

**Web1** 时代，互联网更像一个“电子公告栏”。内容由少数网站发布，用户只能被动阅读，几乎不存在交互，也不存在“数字资产”这一概念。

**Web2** 解决了交互问题。用户可以发布内容、进行社交、完成交易，但代价是：**身份、数据和规则完全掌握在平台手中**。账号被封、内容被删除、规则变更，用户几乎没有博弈能力。

**Web3** 的提出，核心并不是“技术更酷”，而是**尝试将资产、身份与规则从平台中剥离，交还给用户本身**。区块链在这里充当的是“可信执行与记账层”，用代码与共识机制替代平台信用。

* * *

### 区块链

区块链是一种**牺牲效率来换取信任最小化**的系统设计。

它通过分布式共识，让任何单点都无法单方面篡改数据；通过链式结构，确保历史状态可追溯、可验证。这使得“无需信任对方，也能达成一致”成为可能，这是 Web3 能成立的前提。

需要注意的是：区块链并不适合所有场景，它适合的是**需要公开验证、抗审查、抗篡改**的业务。

### 加密货币

加密货币是区块链系统中最基础、最直接的经济激励工具。

它既是价值载体，也是网络运行的“燃料”和“激励机制”。没有加密货币，区块链就很难在开放环境下长期稳定运行。因此，加密货币并不是附属品，而是系统设计的一部分。

### DAPP

DApp 是跑在分布式网络上的程序，它的后端是**智能合约**，前端则通常托管在 IPFS 等去中心化存储中。

与传统 App 的最大区别在于：**无须许可且不可关停**。即便开发团队跑路了，只要区块链还在运行，你依然可以直接通过调用合约代码来使用它的核心功能。它不依赖于某台特定的服务器，而是寄生于全球共识之中。

### 钱包

Web3 钱包是一个**私钥与签名管理器**。

钱包并不保存资产，资产永远存在于链上。钱包的唯一作用，是用私钥对交易或消息进行签名，从而证明“你拥有这个地址的控制权”。

因此，在 Web3 世界中：

> 不是账号体系在保护你，而是密码学在约束你。

私钥一旦泄露，资产的控制权会在数学意义上彻底丢失，且无法找回。

### Gas

Gas 是 Web3 世界中的“计算成本定价机制”。

在以太坊等智能合约平台上，每一笔交易、每一次合约调用，都对应真实的计算和存储消耗。Gas 的存在可以防止恶意刷链、无限循环合约，同时通过费用机制维持网络安全。

理解 Gas，有助于你从“用户视角”转向“系统视角”看待区块链。

* * *

### 智能合约

智能合约是 Web3 真正的“应用层核心”。

它不是“智能”，而是**不可随意更改、自动执行的合约逻辑**。一旦部署到链上，其行为就受到共识规则约束，而非人为控制。

这也是为什么智能合约开发中，安全性和设计阶段的重要性远高于传统 Web 开发——**上线即不可逆**。

### **去中心化治理 (DAO)**

DAO 是 Web3 的**组织形式**，核心是“法治优于人治”。

它通过代码将组织的财务管理、决策流程固化。没有老板，只有持有治理代币（Governance Token）的成员。大家通过投票决定国库资金怎么花、协议怎么改，所有的决策过程在链上透明可见。

### **跨链桥 (Cross-chain Bridge)**

如果把不同的区块链（如以太坊、Solana）看作相互独立的孤岛，跨链桥就是连接它们的**物流航线**。

它让你能把资产从 A 链“搬运”到 B 链。但要注意，跨链往往涉及复杂的资金池锁定和多签机制，它是目前 Web3 中**遭受攻击最频繁、损失金额最高**的高危地带。

* * *

### 同质化代币（FT）

同质化代币的核心特征是：**每一个单位都等价**。

它们非常适合表示货币、积分、治理权等抽象价值。在 DeFi 中，几乎所有基础协议都围绕 ERC20 构建。

### 非同质化代币（NFT）

NFT 的价值不在于“图片”，而在于**唯一性与可验证所有权**。

它解决的是“数字世界中无法天然区分唯一资产”的问题，使得数字资产第一次具备了明确的产权边界。这也是 NFT 能够扩展到身份、凭证、门票等领域的根本原因。

* * *

### 签名与授权风险

在 Web3 中，**签名本身就是授权行为**。

许多钓鱼攻击并不是让你“转账”，而是诱导你签署一个看似无害的 `Approve` 或 Permit。用户一旦授权，攻击者可以在授权额度内持续转走资产，而用户往往毫无察觉。

这是 Web3 安全中最典型、也是最容易被忽视的风险点。

### PunnyCode 网络钓鱼

PunnyCode 攻击利用的是“人眼无法准确识别 Unicode 差异”的弱点。

在 Web3 场景中，域名往往与钱包签名、合约交互直接绑定，一旦进入假网站，后果往往是**资产级别的损失**，而非账号被盗这么简单。

* * *

* * *

# 以太坊周会纪要

## AnthurineXiang 分享

-   **以太坊完成 Fusca 升级并提升区块容量上限**
    
    以太坊已顺利完成 Fusca 升级，将单区块可容纳的容量上限提升至 21 个。这一变化被认为对提升以太坊主网的扩展性具有重要意义。同时，OPBNB 也完成了主网硬分叉，通过缩短区块时间来改善整体性能和吞吐能力。
    
-   **Polymarket 在委内瑞拉事件判定上引发市场争议**
    
    预测市场平台 Polymarket 在委内瑞拉相关事件的结果判定中，将军事行动定义为“突袭”而非“入侵”，引发大量参与者不满。该事件涉及超过 200 万美元的真实资金投票，暴露出预测市场在主观事件裁定、治理规则以及仲裁机制方面的系统性脆弱性。
    
-   **以太坊 Layer2 整体表现低迷，OP 启动回购计划**
    
    当前以太坊 Layer2 生态整体承压，尤其是 OP 代币价格出现明显下跌。项目方计划在未来 12 个月内使用 Superchain 收入的 50%（约 2934 枚 ETH）进行回购，以试图稳定市场预期。这一举措反映出 Layer2 项目正面临信任与估值双重挑战。
    
-   **多起重大黑客攻击事件再次暴露 DeFi 安全风险**
    
    2025 年 11 月，阿贝项目遭遇攻击，被盗资金随后通过 Tornado Cash 洗钱；2026 年 1 月 9 日，twobits 项目被盗约 8000 枚 ETH（价值约 2600 万美元），其代币价格几乎瞬间下跌 99%。尽管项目方已提醒用户暂停交互并配合执法调查，但资金已流入 Tonicash，显示安全风险依然严峻。
    
-   **Vitalik 提出去中心化稳定币的三大核心难题**
    
    Vitalik 近期重申去中心化稳定币的发展方向，指出必须解决三项关键问题：构建长期优于美元的价值指数、设计可抵御大资金操纵的预言机机制，以及在质押收益、系统安全性与稳定币竞争力之间取得平衡。他强调长期应摆脱对单一法币的依赖，以提升系统整体韧性，这被视为对 USDT 与 USDC 垄断格局的直接回应。
    
-   **Zcash 核心团队与基金会决裂并成立商业公司**
    
    Zcash 核心开发团队 ECC 与基金会在治理机制上出现严重分歧，核心成员集体离职并成立盈利公司 CashZ，转向 Zcash 钱包的商业化开发。该事件引发市场强烈反应，ZEC 短期内下跌约 20%。基金会方面强调，Zcash 网络不依赖单一团队，仍由全球节点共同维护。
    
-   **RB 链在 L2 中活跃度领先并调整基础费模型**
    
    RB 链被认为是当前最活跃的 Layer2 之一，主要得益于 Robinhood 等大型项目支持。其近期升级引入更平滑的定价模型，将基础费用从 0.01 GV 提高至 0.02 GV，以降低市场剧烈波动对网络运行的影响。
    
-   **Robinhood 选择基于以太坊构建 L2 而非独立 L1**
    
    Robinhood 决定基于以太坊构建自有 Layer2，而不是推出独立 L1，核心考量包括以太坊的安全性、去中心化属性以及 EVM 生态的深度流动性。其私有测试网即将上线，计划将目前部署在 Arbitrum 上的 200 多只代币化股票迁移至自有 L2，长期目标扩展至 2000 只。
    
-   **Stripe 旗下 Temple 链因选择独立 L1 架构受到质疑**
    
    作为对比，Stripe 推出的 Temple 链因采用独立 L1 架构而受到市场质疑，进一步凸显当前行业对基于以太坊的 L2 路线的整体偏好。
    
-   **Starknet 再次发生宕机事件暴露技术与用户规模问题**
    
    Starknet 近期发生约 18 分钟宕机，虽较 2025 年 9 月长达 5 小时的事故影响较小，但仍暴露出执行层与证明层之间的状态同步问题，同时也反映出其实际用户规模和承载能力有限。
    
-   **社交平台开始成为加密资产新入口**
    
    Twitter 计划推出 SmartCash Tag 功能，用于实时资产标记，Solana 生态迅速完成集成，显示社交平台正逐步成为加密资产与链上应用的重要分发和交互入口。
    
-   **上市矿企 RIO 抛售比特币并转向 AI 与电力基础设施**
    
    上市矿企 RIO 抛售约 2 亿美元比特币，引发市场对机构仓位调整的关注。部分矿企正出售比特币以融资建设 AI 数据中心和电力设施，反映市场对算力与能源需求增长的长期预期。
    
-   **资本持续加码加密基础设施与并购整合**
    
    包括 a16z 投资 1500 万美元用于构建无需托管的 BTC 抵押借贷方案；Fireblocks 以约 1.3 亿美元收购加密会计平台以完善机构服务；以及一笔约 1–1.25 亿美元的比特币 ATM 公司收购案（覆盖美国 49 个州），旨在扩展 Polygon 的线下支付入口。此外，HZ 完成约 150 亿美元募资，释放出加密资产在美国战略层面的重要性信号。
    

* * *

## Qiang 分享

-   **市场创历史新高但未形成真正牛市**
    
    尽管比特币和以太坊在 2025 年一度创下历史新高，但市场随后普遍回撤 30%–40%，并未形成典型牛市。核心原因在于缺乏新用户流入，现有参与者平均已在市场停留 3–5 年，预期与现实出现明显偏差。
    
-   **行业或于 2026 年进入整合期而非爆发期**
    
    Qiang 将当前阶段类比为互联网发展的中期调整期，认为过度建设可能再次引发系统性回调，并判断 2026 年将进入“胜者为王”的整合阶段。
    
-   **以太坊被看好在 2026 年表现优于比特币**
    
    他对以太坊持相对乐观态度，认为其协议层进展稳健，华尔街资产上链趋势及 Clarity 法案推进构成关键利好；相比之下，他认为比特币在滞胀环境下可能跑输黄金，并面临量子计算等长期叙事压力。
    
-   **Solana 热度回落并面临交易型链的激烈竞争**
    
    Solana 在上一周期中炒作过度，当前热度明显回落，同时在交易和衍生品领域面临 Hyperliquid 等竞争者的压力，新 L1 公链获取突破性机会的难度持续上升。
    
-   **预测市场与永续合约成为交易所争夺焦点**
    
    预测市场与永续合约被认为是当前最受关注的赛道，Coinbase、Robinhood 等中心化交易所纷纷入局。Qiang 认为中心化交易所整体竞争力可能强于去中心化方案，但行业仍高度分散且缺乏明确护城河。
    
-   **以太坊的核心价值在于抗审查与系统韧性**
    
    他强调，以太坊的长期使命是抗审查和主权性，而非单纯追求性能或效率。部分牺牲去中心化换取高速结算的方案，更适合中心化基础设施，而非公共区块链的长期目标。
    
-   **NFT 协议正向更可靠的链上存储方案演进**
    
    Qiang 介绍了多种 NFT 存储模式，并指出链下存储虽成本低但存在数据丢失风险。4804 协议通过新的 URL 规范提升了链上数据可获取性，OpenSea 与 Core 等平台近期的支持反映出行业对可靠链上存储的重视。
    

* * *

## DL 分享

-   **Fusca 与 Pardus 升级提升扩容能力但引入新延迟问题**
    
    DL 指出，Fusca 升级在提升区块容量的同时，也带来了数据量增加和网络延迟上升的问题。Pardus 升级通过将 128KB blob 拆分为 128 个 column 并分配至子网验证，以降低单个验证者负载。
    
-   **实验显示扩容对多数验证者有效但边缘风险存在**
    
    实验数据显示约 95% 的验证者在优化后性能有所提升，但仍有约 5% 出现性能下降，尤其在 blob 数量增加后出现验证成功率反向下降的情况，暴露出潜在不可控风险。
    
-   **MEV 激励可能延长区块释放时间影响稳定性**
    
    DL 提醒，MEV 利润驱动可能导致区块释放时间延长，从而对共识稳定性产生负面影响，建议通过部署超级节点等方式缓解网络传播风险。
    
-   **Web3 正从野蛮生长阶段转向合规与传统金融融合**
    
    他将行业发展划分为上下半场，认为当前已进入以合规、监管清晰化和传统金融对接为核心的下半场，竞争焦点从链上交易量转向合规能力和机构合作能力。
    
-   **稳定币与政策支持成为市场结构变化的关键变量**
    
    稳定币规模在 2025 年增长超过 50%，USDT 与 USDC 占据主导地位。美国政策层面对养老金投资、比特币战略储备及稳定币发行方 IPO 的支持，被视为加密资产融入传统金融体系的重要标志。
    
-   **行业成熟伴随流动性与监管风险并存**
    
    DL 对比特币价格判断相对保守，并警示未来可能出现流动性逆转、监管收紧及道德风险问题。整体而言，加密行业正从封闭投机阶段，逐步迈向规则明确、与传统金融深度融合的成熟阶段。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
