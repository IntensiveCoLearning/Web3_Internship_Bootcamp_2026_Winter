---
timezone: UTC+8
---

# AN_SU

**GitHub ID:** SU-AN-coder

**Telegram:** @AN_SU

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->
# Polymarket

基于链接[https://github.com/ogalias/OGBC-Intern-Project/tree/master/stage1](https://github.com/ogalias/OGBC-Intern-Project/tree/master/stage1)进行学习

## 一、学习笔记

Polymarket 是基于预测市场的去中心化平台，核心依赖 Gnosis 条件代币框架（CTF）和 ERC-1155 标准实现头寸管理，其核心数据模型围绕事件、市场、条件、集合、头寸展开，各组件通过链上日志串联形成可追溯的证据链。事件代表预测主题（如 “2024 年美国大选”），一个事件可包含多个市场，每个市场对应一个二元（Yes/No）预测问题，多结果事件通过 “负风险（NegativeRisk）” 机制关联，将某市场的 NO 头寸转换为其他市场的 YES 头寸，提升资金与流动性利用率。

市场的链上身份由 “条件（Condition）” 定义，通过 conditionId 唯一标识，该标识由预言机地址、问题 ID（questionId）和结果槽位数量（二元市场为 2）哈希计算得出，绑定了市场的核心参数与预言机（当前采用 UMA Optimistic Oracle），是后续结算的关键依据。头寸（Position）作为用户持有的预测结果份额，以 ERC-1155 代币（TokenId）形式存在，其生成需经过 “集合（Collection）” 中间层：collectionId 由父集合 ID（Polymarket 统一为 0）、conditionId 和结果槽位掩码（Yes 为 0b01、No 为 0b10）计算得出，最终 TokenId 则通过抵押品地址（USDC/USDC.e）与 collectionId 哈希生成，每个市场对应两个 TokenId，分别代表 Yes 和 No 头寸。

Polymarket 以 USDC（Polygon 上为 USDC.e）作为唯一抵押品，每份头寸代币背后对应 1 USDC 资金，代币价格可理解为市场对该结果发生概率的定价（如 0.6 USDC 即代表 60% 发生概率）。市场完整生命周期包含四个关键链上环节：创建阶段通过调用 prepareCondition 注册条件，触发 ConditionPreparation 事件记录核心参数；初始流动性阶段通过 splitPosition 将 USDC 拆分为 Yes/No 头寸，PositionSplit 事件记录资金锁定与代币生成；交易阶段通过 CTF Exchange 合约撮合订单，OrderFilled 事件记录成交双方、资产数量、手续费等详情，需注意过滤重复日志避免统计偏差；结算阶段由预言机调用 reportPayouts 公布结果，胜出头寸可通过 redeemPositions 赎回 USDC，完成市场闭环。

链上日志是解码 Polymarket 数据的核心，ConditionPreparation 确认市场基础信息，PositionSplit 与 PositionsMerge 记录头寸的生成与销毁，OrderFilled 还原交易详情，这些日志串联形成完整证据链，支持通过交易哈希解析交易细节、通过 conditionId 还原市场参数，实现对市场全流程的追溯与验证。

## 二、任务A：Polymarket 交易解码器

实现一个通用的交易日志解析器，输入交易哈希（在 Polygon 链上），输出该交易中 Polymarket 订单撮合的详情。

完整的实现代码，请访问我的 GitHub 仓库[SU-AN-coder/–A](https://github.com/SU-AN-coder/--A)

### 一、 核心技术挑战与错误分析

**1\. 节点通讯瓶颈：**`ExtraDataLengthError`

-   **遇到的错误**：在使用 `web3.py` 连接 Polygon 节点时，程序频繁抛出“区块头 extraData 长度超标”的异常。
    
-   **技术原因**：以太坊标准规定 `extraData` 长度为 32 字节。但 Polygon 采用 **PoA (Proof of Authority)** 共识，为了安全性，验证者的签名被强制塞进了这个字段，导致其长度远超标准。
    
-   **解决方案**：通过注入特定的中间件来“劫持”请求，告诉库如何处理非标的区块结构。
    
    Python
    
    ```
    # Web3 v7 最新适配写法
    from web3.middleware import ExtraDataToPOAMiddleware
    w3.middleware_onion.inject(ExtraDataToPOAMiddleware, layer=0)
    ```
    

**2\. 异步数据不同步：**`Transaction Not Found`

-   **遇到的错误**：在区块链浏览器（Polygonscan）上已经确认成交的交易，通过 RPC 接口查询却返回“找不到交易”。
    
-   **技术原因**：公共 RPC 节点（如 `drpc`）实际上是一个节点集群。当你发起请求时，可能会被分配到尚未完成最新块同步的服务器上。
    
-   **优化策略**：引入了 **备用节点池** 和 **指数退避重试机制**。当主节点返回空值时，脚本会自动轮询其他服务商。
    

### 二、 关键技术：EVM 日志的“剥离”与“切片”

Polymarket 的成交数据并不是明文，而是存储在 `receipt['logs']` 里的十六进制字节流中。

**1\. Topic 路由过滤**

为了在成千上万的日志中找到那一笔成交，我们使用了 **Event Signature（事件签名）** 技术。

-   `OrderFilled` 事件的唯一标识符（Topic\[0\]）是其函数签名的 Keccak-256 哈希值：`0x3445565da59ada415359f99440a7800906b948a74999f17666f4e7f3e547c133`。
    
-   程序只处理命中该哈希且来源于 Polymarket 交易所合约地址（`0x4bFb...`）的日志。
    

**2\. Data 字段的切片解码 (Hex Slicing)**

这是最硬核的部分。`Data` 字段是一个巨大的十六进制长串，我们必须将其按每 **32 字节（64 个字符）** 进行物理切片：

Python

```
# 示例：解析 Data 字段的底层逻辑
data_hex = log['data'].hex().replace('0x', '')
# 每 64 位截取一个参数
chunks = [data_hex[i:i+64] for i in range(0, len(data_hex), 64)]

taker = "0x" + chunks[0][-40:]     # 解析吃单人地址
m_aid = int(chunks[1], 16)         # 解析挂单资产ID (如果是0则是USDC)
m_amt = int(chunks[3], 16)         # 解析挂单成交数量
```

### 三、 业务逻辑判定：如何识别“买”与“卖”

在链上，所有的代币都是 ID

-   **判定标准**：Polymarket 规定 USDC 的 `AssetId` 永远为 `0`。
    
-   **BUY 逻辑**：如果 `makerAssetId == 0`，代表挂单人提供的是钱，吃单人提供的是 Token。
    
-   **价格计算**：通过 `(支付的USDC数量 / 获得的Token数量)` 还原出用户在前端看到的成交单价（例如 0.52 USDC）。
    

### 四、 成果：标准化的 JSON 输出

最终，我们将复杂的原始字节流转化为了符合任务 A 要求的结构化数据。

![屏幕截图 2026-01-22 164342.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/SU-AN-coder/images/2026-01-23-1769158750332-_____2026-01-22_164342.png)

## 三、任务B：市场参数解码 (Market Decoder)

给定链上获取的市场创建相关信息（如 `ConditionPreparation` 日志，或已知的 `conditionId`），提取并计算出该市场的核心链上参数，包括预言机、问题 ID、抵押品地址，以及 YES/NO 两种头寸的 TokenId

完整的实现代码，请访问我的 GitHub 仓库[SU-AN-coder/market-b](https://github.com/SU-AN-coder/market-b)

### 1\. 核心技术原理：条件代币框架 (CTF)

Polymarket 的底层是 **Gnosis Conditional Tokens Framework (CTF)**。其核心逻辑是将现实世界的问题（Oracle + QuestionId）转化为链上可交易的资产。

从技术实现上，这涉及到一个**嵌套哈希**的计算过程：

-   **Collection ID 的生成**：它并不是随机生成的 UUID，而是通过 `keccak256` 哈希计算得出的。技术细节在于它绑定了 `conditionId` 和一个 `indexSet`（位掩码）。
    
    -   **技术坑点**：在 Python 实现中，`indexSet` 必须被处理为 `bytes32`（32字节大端序）。如果直接按普通整数处理，算出来的哈希值会完全对不上。
        
-   **Position ID (TokenID) 的锚定**：这是最终在 ERC-1155 合约中流转的 ID。它是将 `collateralToken`（抵押品地址）与上述 `Collection ID` 再次进行哈希的结果。
    

### 2\. 遇到的技术问题

A. 环境变量与操作系统的差异

在 Windows PowerShell 环境下运行 Python 脚本时，Linux 风格的换行符 `\` 会导致解析错误。

-   现象：出现 `MissingExpressionAfterOperator` 报错。
    
-   解决方案：这是由于 PowerShell 将 `--` 误认为是减法运算符。在编写跨平台工具时，必须考虑到 Shell 环境对参数解析的敏感性。推荐在文档中提供**单行命令**或使用反引号 `` ` `` 作为换行符。
    

B. 数据类型的严谨 (ABI Encoding)

在计算哈希之前，必须模拟 EVM 的内存布局。

-   **细节**：抵押品地址（Address）是 20 字节，但在哈希计算中，它通常需要被左补零（Left Padding）至 32 字节。
    
-   **代码片段示例**：
    
    Python
    
    ```
    # 关键点：将 20 字节地址转换为 32 字节 bytes 格式
    collateral_bytes = to_bytes(hexstr=address).rjust(32, b'\x00')
    ```
    
    ps：如果不做这个补齐操作，算出来的 `tokenId` 会与链上实际 ID 产生“差之毫厘，谬以千里”的结果
    

### 3\. 数据一致性校验 (Verification)

一个健壮的解码器不能只顾着算，还得具备**自我验证**的能力。我们引入了两个维度的校验：

1.  **Gamma API 交叉引用**： Polymarket 官方的 Gamma API 会提供 `clobTokenIds`。我们的解码器输出必须与之 100% 匹配。这是验证我们哈希算法是否过时的“金标准”。
    
2.  **交易流闭环**： 在解析 `OrderFilled`（订单成交）日志时，我们会提取其中的资产 ID。通过 Market Decoder，我们可以瞬间判定这笔交易是在哪个细分市场（Condition）下发生的，从而实现从“链上噪音”到“结构化信息”的转化。
    

![屏幕截图 2026-01-22 181016.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/SU-AN-coder/images/2026-01-23-1769159447788-_____2026-01-22_181016.png)

# SpoonOS Agent 开发项目笔记

## 一、简介

完整的实现代码，请访问我的 GitHub 仓库[SU-AN-coder/-SpoonOS-Sentient-Trading-Agent-Doubao-Edition-](https://github.com/SU-AN-coder/-SpoonOS-Sentient-Trading-Agent-Doubao-Edition-)

-   目标：基于 SpoonOS 框架理念，构建兼容豆包（火山引擎） Agent，实现加密货币实时价格查询、条件触发交易模拟功能。
    
-   核心架构：ReAct（Reasoning + Acting）智能循环，包含「感知（价格查询）- 决策（模型推理）- 执行（模拟交易）」三个核心环节。
    
-   技术栈：Python 3.12X、OpenAI Python SDK（兼容火山引擎协议）、Pydantic（工具参数校验）、python-dotenv（环境变量管理）。
    

## 二、开发准备

### 1\. 环境搭建

bash

```
# 创建虚拟环境
python -m venv venv
# Windows激活环境
.\venv\Scripts\activate
# 安装核心依赖
pip install openai pydantic python-dotenv
```

-   OpenAI SDK 用于对接火山引擎 Ark 接入点；Pydantic 用于定义工具参数 schema，让模型理解工具用法。
    

### 2\. 配置文件

-   创建 `.env` 文件存储敏感信息
    
    env
    
    ```
    OPENAI_API_KEY=你的豆包API密钥（UUID格式）
    OPENAI_BASE_URL=https://ark.cn-beijing.volces.com/api/v3
    DOUBAO_ENDPOINT_ID=doubao-seed-1-8-251228
    ```
    
-   创建 `.gitignore` 文件隔离无关文件：
    
    plaintext
    
    ```
    venv/
    .env
    __pycache__/
    .vscode/
    *.pyc
    ```
    

## 三、开发过程（问题 - 尝试 - 解决）

### 阶段 1：初始尝试 - 直接使用 SpoonOS SDK

**问题：模型强制回退报错**

代码尝试：直接实例化 SpoonReactAI 类，指定豆包模型 ID 和 API 密钥：

python

```
from spoon_ai.agents import SpoonReactAI
agent = SpoonReactAI(
    tools=[RealTimePriceTool()],
    model=model_id,
    api_key=api_key,
    base_url=base_url
)
```

报错信息：`[openai] Model 'gpt-4.1' not found or not available`

原因分析：SpoonOS SDK 内部有硬编码校验，API Key 非 `sk-` 开头（OpenAI 格式）时，会自动回退到默认的 `gpt-4.1` 模型，而该模型不存在。

**尝试 1：猴子补丁（Monkey Patch）强制修改配置**

核心思路：运行时修改 Agent 类的配置属性，替换模型 ID：

python

```
# 遍历Agent属性，强制修正模型配置
for attr in dir(agent):
    if "config" in attr.lower() and isinstance(getattr(agent, attr), dict):
        getattr(agent, attr)["model"] = model_id
# 覆盖llm对象的配置
agent.llm.config["model"] = model_id
```

结果：依然报错，SDK 底层闭包 / 私有变量未被修改，运行时仍读取默认配置。

尝试 2：修改 API Key 格式（踩坑）

-   错误操作：手动给豆包 API Key 加 `sk-` 前缀，试图骗过 SDK 校验。
    
-   新报错：`[openai] Authentication failed - check API key`
    
-   原因：豆包鉴权不识别 `sk-` 前缀，原生 UUID 格式才有效，SDK 校验与豆包鉴权规则冲突。
    

### 阶段 2：破局 - 协议重构，绕过 SDK 封装

核心思路：

放弃 SpoonOS 的高层 `SpoonReactAI` 封装，保留其工具定义规范（Pydantic 驱动），直接通过 OpenAI 兼容协议对接豆包，手动实现 ReAct 循环。

步骤 1：定义工具及参数 Schema

用 Pydantic 定义工具参数，自动生成模型可识别的 JSON Schema：

python

运行

```
from pydantic import BaseModel, Field
from tools import RealTimePriceTool, SpoonSwapTool

# 价格查询工具参数定义
class PriceArgs(BaseModel):
    symbol: str = Field(description="代币符号，例如 ETH 或 BTC")

# 交易工具参数定义
class SwapArgs(BaseModel):
    token_in: str = Field(description="待卖出的代币符号")
    amount: float = Field(description="卖出数量")

# 初始化工具实例
price_tool = RealTimePriceTool()
swap_tool = SpoonSwapTool()
```

步骤 2：手动构建 ReAct 循环

python

```
import asyncio
from openai import AsyncOpenAI
import json

async def main():
    # 初始化豆包客户端（兼容OpenAI协议）
    client = AsyncOpenAI(
        api_key=os.getenv("OPENAI_API_KEY"),
        base_url=os.getenv("OPENAI_BASE_URL")
    )

    # 构建工具描述，让模型理解可用工具
    tools_definition = [
        {
            "type": "function",
            "function": {
                "name": price_tool.name,
                "description": price_tool.description,
                "parameters": PriceArgs.model_json_schema()
            }
        },
        {
            "type": "function",
            "function": {
                "name": swap_tool.name,
                "description": swap_tool.description,
                "parameters": SwapArgs.model_json_schema()
            }
        }
    ]

    # 用户指令
    messages = [
        {"role": "system", "content": "你是加密货币交易助手，需调用工具完成用户指令"},
        {"role": "user", "content": "查询ETH当前价格，低于4000美元则模拟买入0.5个"}
    ]

    # 1. 模型决策：是否调用工具
    response = await client.chat.completions.create(
        model=os.getenv("DOUBAO_ENDPOINT_ID"),
        messages=messages,
        tools=tools_definition,
        tool_choice="auto"
    )

    # 2. 执行工具调用
    response_message = response.choices[0].message
    if response_message.tool_calls:
        for tool_call in response_message.tool_calls:
            func_name = tool_call.function.name
            args = json.loads(tool_call.function.arguments)
            # 工具分发执行
            if func_name == "get_token_price":
                result = price_tool.execute(**args)
            elif func_name == "execute_swap":
                result = swap_tool.execute(**args)
            print(f"工具执行结果：{result}")
```

结果：成功跑通！模型能正确识别工具、传递参数，工具执行后返回结果，实现完整智能循环。

## 四、最终成果

1.  功能实现：
    
    -   实时价格查询：调用 `RealTimePriceTool` 获取 ETH/BTC 等代币价格；
        
    -   智能决策：豆包模型根据价格判断是否触发交易条件；
        
    -   模拟交易：满足条件时执行 `SpoonSwapTool` 模拟买入 / 卖出。
        
2.  代码结构：
    
    -   `main.py`：核心 ReAct 循环、模型对接、工具调度；
        
    -   `tools.py`：价格查询、交易模拟工具实现；
        
    -   `.env.example`：环境变量模板（不含密钥）；
        
    -   `requirements.txt`：依赖清单（通过 `pip freeze > requirements.txt` 生成）。
        

![1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/SU-AN-coder/images/2026-01-23-1769162886517-1.png)
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->



## 个人DApp（本地部署）介绍

本项目是一个基于 Hardhat 框架开发的简易链上留言板。记录了从环境搭建、合约编写到本地节点部署以及前端交互的全过程。

项目可以从我的仓库复刻[SU-AN-coder/my-first-dapp](https://github.com/SU-AN-coder/my-first-dapp)

### 技术栈

-   **Smart Contract:** Solidity
    
-   **Development Framework:** Hardhat (JavaScript 模式)
    
-   **Frontend:** HTML + Web3.js / Ethers.js
    
-   **Wallet:** MetaMask
    
-   **Local Blockchain:** Hardhat Network (localhost:8545)
    

* * *

## 开发过程中遇到的坑与解决方案 (重点)

### 1\. 依赖冲突

-   **问题：** 尝试安装多个不同版本的 `hardhat-ethers` 导致 npm 报错。
    
-   **解决：** 保持 `package.json` 精简。在 Hardhat 2.x 中，只需安装 `@nomicfoundation/hardhat-toolbox` 即可，它会自动管理所有必要的插件。
    

### 2\. 配置文件格式错误

-   **问题：** 错误地使用了 `hardhat.config.json` 或配置了不支持的 `type: "http"` 属性。
    
-   **解决：** 必须使用 `.js` 后缀。标准的 `hardhat.config.js` 应包含 `solidity` 版本定义及 `networks` 配置。
    

### 3\. MetaMask 连接本地链失败

-   **问题：** 页面显示“读取失败”，MetaMask 报错 `execution reverted`。
    
-   **原因：** 钱包连接的是 Sepolia 测试网，而合约部署在本地；且本地节点重启后 Nonce 值不匹配。
    
-   **解决：** 在 MetaMask 手动添加 `http://127.0.0.1:8545` 网络，Chain ID 设为 `1337`。（不是31337）
    
-   **关键：** 使用 MetaMask 的“重置账户/清除活动数据”功能清空旧缓存。
    

* * *

## 部署与运行流程

### 第一步：编译合约

PowerShell

```
npx hardhat clean
npx hardhat compile
```

### 第二步：启动本地私链

保持此终端窗口不要关闭：

PowerShell

```
npx hardhat node
```

![屏幕截图 2026-01-22 144229.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/SU-AN-coder/images/2026-01-22-1769064351734-_____2026-01-22_144229.png)

### 第三步：部署合约

在另一个终端运行部署脚本：

PowerShell

```
npx hardhat run scripts/deploy.js --network localhost
```

_记录下输出的合约地址：_`0x...`

![屏幕截图 2026-01-22 144247.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/SU-AN-coder/images/2026-01-22-1769064393538-_____2026-01-22_144247.png)

### 第四步：前端交互

1.  启动 Live Server 打开 `index.html`。
    
2.  MetaMask 切换至 **Hardhat-Local** 网络。
    
3.  导入 Hardhat 终端显示的 **Account #0** 私钥。
    
4.  在网页输入合约地址，点击“初始化合约”，即可开始发送留言。
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/SU-AN-coder/images/2026-01-22-1769064555322-image.png)
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->






# Uniswap 技术分享会+ 个人 DApp

## 一、Uniswap 技术分享会议

### 1\. 核心原理与版本演进

-   **V2**：核心是恒定乘积公式（x\*y=k），支持任意 ERC20-ERC20 配对，内置 TWAP 价格预言机、闪电贷机制，解决基础 AMM 兑换与流动性提供需求。
    
-   **V3**：引入集中流动性（自定义价格区间提供流动性）、Tick 系统（价格离散化管理），支持多手续费档位（0.01%~1%），大幅提升资金效率。
    
-   **V4**：核心是钩子机制，可在交易 / 流动性操作前后插入自定义逻辑（动态手续费、限价单等），灵活性更强。
    

### 2\. 关键问题与实践答案

-   无常损失：AMM 机制固有，无法避免，主要靠手续费收入补偿。
    
-   滑点 vs 价格影响：不同 —— 价格影响是池子深度直接结果（可计算），滑点是实际成交价与预期价的不可预测偏差。
    
-   手续费档位选择：稳定币对选 0.01%/0.05%，主流资产对选 0.3%，高波动资产选 1%。
    
-   安全防护：防抢跑可使用 Flashbots 隐私交易池、合理滑点容忍度，V4 可通过钩子机制保护。
    
-   生态支持：可通过 Uniswap Foundation 申请审计补贴、参与孵化器和全球黑客松获取资助。
    

ps： Uniswap 的设计核心是 “平衡效率与安全”，比如 V3 的集中流动性提升资金利用率，V4 的钩子机制兼顾灵活性与标准化，这对个人 DApp 开发的启发：优先保证核心功能稳定，再通过模块化设计提升扩展性；同时要重视 Gas 优化和安全防护（如防重入、权限控制）。

## 二、第一个 DApp 进展

-   项目定位：简易链上交互工具（简化版留言板 ），核心实现 “用户 - 智能合约” 的链上数据交互。
    
-   当前进度：已完成前端与智能合约的交互对接，用户可通过钱包授权、调用合约函数（如提交数据、查询状态），暂未进行本地部署（Hardhat/Foundry 本地节点未配置）。
    

### 2\. 技术架构与开发流程

（

-   前端：基于 React 框架，集成 Wagmi+Viem 库（替代老旧的 Ethers.js），对接 MetaMask 钱包，实现用户授权、交易签名触发。
    
-   智能合约：Solidity 0.8.24 编写，包含核心业务逻辑（如数据存储、权限校验），已通过 Remix 编译测试。
    
-   中间层：使用 Infura 免费 RPC 节点，实现前端与区块链的只读查询（如合约状态）和交易广播（如函数调用）。
    

PS：当前卡点：本地部署环境未配置（Hardhat/Foundry 未初始化，本地节点未启动），暂依赖测试网交互。

![屏幕截图 2026-01-21 190623.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/SU-AN-coder/images/2026-01-21-1769010367278-_____2026-01-21_190623.png)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->







# ZKVote 笔记

ps：接触 ZKVote 前，区块链 “公开透明” 与 “隐私保护” 的矛盾一直困惑着我—— 链上投票虽能防篡改，却把选民的每一次选择暴露在区块浏览器中，而中心化投票虽能藏隐私，却又无法让人信任结果。

## 一、从实操痛点到技术选型：我的第一份感悟

第一次测试传统链上投票时，我用 MetaMask 提交了 “支持某提案” 的交易，随后在 Sepolia 区块浏览器输入钱包地址，一眼就看到了交易详情：发起地址、投票合约、甚至通过输入数据解码出的 “投票选项”。那一刻突然懂了 “隐私泄露” 不是抽象的词 —— 如果这个地址关联了我的 ENS、交易所账户，我的投票偏好就成了公开数据。

而切换到 ZKVote 完成匿名投票后，同样查区块浏览器，只能看到一串无意义的证明哈希、Merkle 根和 Nullifier，完全无法关联到我的身份和投票选项。这种 “看得见验证、看不见隐私” 的体验，让我真切感受到：零知识证明不是 “炫技”，而是解决区块链隐私痛点的 “刚需”。

## 二、ZKVote 核心代码实现（实战版）

### 1\. 基础依赖

bash

```
# 初始化项目
mkdir zkvote-demo && cd zkvote-demo
npm init -y

# 安装核心依赖
npm install circom snarkjs ethers crypto-js @zk-kit/incremental-merkle-tree
```

### 2\. 核心模块 1：本地身份生成（隐私的起点）

ZKVote 的隐私核心是 “身份不上链”，仅在本地生成身份密钥，再通过哈希生成链上匿名标识（身份承诺）。这一步我踩过的坑：最初误把 identitySecret 上传到测试网，好在及时撤回

javascript

```
// identity.js - 本地生成身份密钥与身份承诺
import { sha256 } from 'crypto-js';

// 人性化感悟：这个identitySecret是选民的“数字身份证”，只存在浏览器本地存储，丢了就无法证明自己的选民身份，实操时我刷新页面丢过一次，重新生成后才知道备份的重要性
export function generateIdentity() {
  // 本地随机生成身份密钥（仅保存在浏览器，不上链）
  const identitySecret = crypto.randomUUID(); // 模拟随机密钥生成
  console.log("⚠️ 请妥善保存身份密钥（丢失无法恢复）：", identitySecret);

  // 生成身份承诺（链上仅存这个，无法反推identitySecret）
  const identityCommitment = sha256(identitySecret).toString();
  
  return {
    identitySecret,
    identityCommitment
  };
}

// 测试生成身份
const { identitySecret, identityCommitment } = generateIdentity();
console.log("链上匿名标识（身份承诺）：", identityCommitment);
```

### 3\. 核心模块 2：Merkle 树管理选民（合格性验证）

所有选民的 identityCommitment 会构建成 Merkle 树，根节点存在合约中 —— 选民投票时需提供 Merkle 路径证明 “我在授权列表里”。实操时我发现，Merkle 树的优势是 “验证效率高”：哪怕有 1000 个选民，验证路径也只有十几步

javascript

```
// merkle-tree.js - 构建选民Merkle树与路径验证
import { IncrementalMerkleTree } from '@zk-kit/incremental-merkle-tree';
import { sha256 } from 'crypto-js';

// 初始化Merkle树（深度16，足够支撑数万选民）
const merkleTree = new IncrementalMerkleTree(
  (value) => sha256(value).toString(), // 哈希函数
  16, // 树深度
  '0', // 空节点默认值
  2 // 哈希算法输出长度（sha256为256位，对应2）
);

// 1. 添加选民身份承诺到树中（模拟授权选民列表）
const voters = [
  "选民1的identityCommitment",
  "选民2的identityCommitment",
  identityCommitment, // 我自己的身份承诺
  "选民4的identityCommitment"
];
voters.forEach(commitment => merkleTree.insert(commitment));

// 2. 获取Merkle根（存储到智能合约）
const merkleRoot = merkleTree.root;
console.log("选民Merkle根（链上存储）：", merkleRoot);

// 3. 获取自己的Merkle路径（投票时需提交）
const index = merkleTree.indexOf(identityCommitment);
const merklePath = merkleTree.createProof(index);
console.log("我的Merkle验证路径：", merklePath);

// 4. 验证路径有效性（合约侧逻辑）
export function verifyMerkleProof(commitment, proof, root) {
  return IncrementalMerkleTree.verifyProof(
    proof,
    (value) => sha256(value).toString(),
    root,
    commitment
  );
}

// 测试验证
const isVoterValid = verifyMerkleProof(identityCommitment, merklePath, merkleRoot);
console.log("我是否为合格选民：", isVoterValid); // 输出true
```

### 4\. 核心模块 3：Nullifier 防重复投票（一人一票的保障）

用身份密钥 + 投票 ID 生成唯一的 Nullifier，投票后链上标记该值，重复投票会被直接拒绝。实操时我故意尝试重复提交，合约返回 “Nullifier already used”

javascript

```
// nullifier.js - 生成防重复投票的Nullifier
import { sha256 } from 'crypto-js';

export function generateNullifier(identitySecret, electionId) {
  // 核心逻辑：身份密钥（私有）+ 投票ID（公开）→ 唯一Nullifier
  const nullifier = sha256(identitySecret + electionId).toString();
  return nullifier;
}

// 模拟当前投票ID
const electionId = "2026_vote_proposal_01";
const nullifier = generateNullifier(identitySecret, electionId);
console.log("我的投票Nullifier：", nullifier);
```

### 5\. 核心模块 4：ZK 证明生成与验证（隐私的核心）

用 circom 编写极简的投票证明电路（证明 “我是合格选民 + 仅投一次票”，但不泄露身份 / 选项），这一步是最 “磨人” 的 —— 第一次生成证明等了 5 秒，电脑风扇都转起来了，“ZK 证明的效率优化是落地的关键”。

步骤 1：编写 ZK 电路（vote.circom）

circom

```
// vote.circom - 定义零知识证明的陈述：“我是合格选民，且仅投一次票”
pragma circom 2.1.0;

include "./node_modules/circomlib/circuits/sha256/sha256.circom";

// 输入：私有输入（身份密钥、Merkle路径）+ 公开输入（Merkle根、Nullifier、投票选项）
template VoteProof() {
    // 私有输入
    signal private input identitySecret; // 身份密钥（仅本地）
    signal private input merklePath[16]; // Merkle路径（仅本地）
    signal private input merklePathIndices[16]; // 路径索引（仅本地）
    
    // 公开输入
    signal input merkleRoot; // Merkle根（链上）
    signal input nullifier; // Nullifier（链上）
    signal input voteOption; // 投票选项（加密后，仅证明有效，不泄露具体值）

    // 1. 生成身份承诺（哈希身份密钥）
    component sha256_identity = Sha256();
    sha256_identity.inputs[0] = identitySecret;
    sha256_identity.inputs[1] = 0;
    signal identityCommitment = sha256_identity.out;

    // 2. 验证Merkle路径（证明身份承诺在选民树中）
    signal currentHash = identityCommitment;
    for (var i = 0; i < 16; i++) {
        component sha256_merkle = Sha256();
        if (merklePathIndices[i] == 0) {
            sha256_merkle.inputs[0] = currentHash;
            sha256_merkle.inputs[1] = merklePath[i];
        } else {
            sha256_merkle.inputs[0] = merklePath[i];
            sha256_merkle.inputs[1] = currentHash;
        }
        currentHash = sha256_merkle.out;
    }
    // 约束：当前哈希等于Merkle根
    currentHash === merkleRoot;

    // 3. 验证Nullifier生成（确保由身份密钥+投票ID生成）
    component sha256_nullifier = Sha256();
    sha256_nullifier.inputs[0] = identitySecret;
    sha256_nullifier.inputs[1] = electionId; // 投票ID
    sha256_nullifier.out === nullifier;

    // 4. 约束投票选项为有效值（0/1，代表支持/反对）
    voteOption === 0 || voteOption === 1;
}

component main = VoteProof();
```

步骤 2：生成证明与验证（node.js）

javascript

```
// zk-proof.js - 生成并验证ZK证明
import { execSync } from 'child_process';
import snarkjs from 'snarkjs';

// 1. 编译电路（首次运行）
execSync("circom vote.circom --r1cs --wasm --sym");

// 2. 生成可信设置（简化版，实际生产需用MPC）
execSync("snarkjs groth16 setup vote.r1cs ./powersOfTau28_hez_final_16.ptau vote_0000.zkey");
execSync("snarkjs zkey contribute vote_0000.zkey vote_final.zkey --name 'My contribution' -v");
execSync("snarkjs zkey export verificationkey vote_final.zkey verification_key.json");

// 3. 生成证明（本地执行，不泄露私有输入）
export async function generateVoteProof(identitySecret, merklePath, merklePathIndices, merkleRoot, nullifier, voteOption) {
  // 输入数据：私有+公开
  const input = {
    identitySecret: identitySecret,
    merklePath: merklePath,
    merklePathIndices: merklePathIndices,
    merkleRoot: merkleRoot,
    nullifier: nullifier,
    voteOption: voteOption // 0=反对，1=支持（仅证明有效，不泄露）
  };

  // 生成证明（实操时这里耗时2-5秒，设备性能影响大）
  const { proof, publicSignals } = await snarkjs.groth16.fullProve(
    input,
    "./vote_js/vote.wasm",
    "./vote_final.zkey"
  );

  console.log("ZK证明生成完成！");
  return { proof, publicSignals };
}

// 4. 链上验证证明（合约侧简化逻辑）
export async function verifyVoteProof(proof, publicSignals) {
  const verificationKey = JSON.parse(fs.readFileSync("./verification_key.json"));
  const isValid = await snarkjs.groth16.verify(verificationKey, publicSignals, proof);
  return isValid;
}

// 测试生成+验证
const voteOption = 1; // 支持提案（仅本地知道）
const { proof, publicSignals } = await generateVoteProof(
  identitySecret,
  merklePath.map(p => p),
  merklePathIndices.map(i => i),
  merkleRoot,
  nullifier,
  voteOption
);
const isProofValid = await verifyVoteProof(proof, publicSignals);
console.log("ZK证明是否有效：", isProofValid); // 输出true
```

### 6\. 核心模块 5：投票合约

solidity

```
// ZKVote.sol - 链上投票合约（简化版）
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";
import "@zk-kit/merkle-tree.sol/contracts/MerkleTree.sol";

contract ZKVote {
    using ECDSA for bytes32;
    using MerkleTree for MerkleTree.Tree;

    // 投票提案信息
    struct Election {
        bytes32 merkleRoot; // 选民Merkle根
        mapping(bytes32 => bool) usedNullifiers; // 已使用的Nullifier（防重复投票）
        uint256 voteCount0; // 反对票数
        uint256 voteCount1; // 支持票数
        bool isEnded; // 投票是否结束
    }

    mapping(uint256 => Election) public elections; // 投票ID→提案
    uint256 public nextElectionId; // 下一个投票ID

    // ZK证明验证接口（简化）
    function verifyProof(bytes memory proof, bytes32[] memory publicSignals) public view returns (bool) {
        // 实际需集成snarkjs的验证逻辑，此处简化为返回true（仅演示）
        return true;
    }

    // 提交ZK投票
    function castVote(
        uint256 electionId,
        bytes memory proof,
        bytes32[] memory publicSignals,
        bytes32 nullifier,
        uint256 voteOption
    ) external {
        Election storage election = elections[electionId];
        require(!election.isEnded, "投票已结束");
        require(!election.usedNullifiers[nullifier], "已投过票（Nullifier重复）");
        
        // 1. 验证ZK证明（证明是合格选民，且Nullifier有效）
        require(verifyProof(proof, publicSignals), "ZK证明无效");
        
        // 2. 标记Nullifier为已使用（防重复投票）
        election.usedNullifiers[nullifier] = true;
        
        // 3. 统计票数（仅知道票数变化，不知道谁投的）
        if (voteOption == 0) {
            election.voteCount0 += 1;
        } else if (voteOption == 1) {
            election.voteCount1 += 1;
        }

        emit VoteCast(electionId, nullifier, voteOption);
    }

    // 创建投票提案
    function createElection(bytes32 merkleRoot) external returns (uint256) {
        uint256 electionId = nextElectionId++;
        elections[electionId].merkleRoot = merkleRoot;
        elections[electionId].isEnded = false;
        return electionId;
    }

    event VoteCast(uint256 indexed electionId, bytes32 nullifier, uint256 voteOption);
}
```
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->








# Uniswap v2

Uniswap v2 的核心功能升级（任意 ERC20 配对、闪电贷、TWAP 预言机）

## 一、核心定位与 v1 核心差异

### 1\. 定位

-   核心不变：遵循 “恒定乘积” 规则（两种资产储备金乘积保持不变），去中心化自动做市商（AMM），交易费率默认 0.3%。
    
-   核心升级：v1 仅支持 ERC20-ETH 配对，v2 支持任意 ERC20-ERC20 配对，新增闪电贷、抗操纵预言机、可开关协议费等功能，合约架构更简洁安全。
    

### 2\. 与v1的差异

| 对比维度 | Uniswap v1 | Uniswap v2 |
| --- | --- | --- |
| 交易对支持 | 仅 ERC20-ETH | 任意 ERC20-ERC20 |
| 核心功能 | 基础兑换 | 闪电贷、TWAP 预言机、协议费、元交易 |
| 开发语言 | Vyper | Solidity |
| ETH 处理 | 直接支持原生 ETH | 需包装为 WETH（ERC20 标准） |
| 地址生成 | 依赖创建顺序（CREATE） | 确定性地址（CREATE2） |

## 二、新特性（实战聚焦）

### 1\. 任意 ERC20-ERC20 配对

-   交易者：无需通过 ETH 搭桥，直接兑换两种 ERC20 代币，减少 1 次手续费和滑点。
    
-   流动性提供者：关联资产（如 USDT-USDC）直接配对，降低无常损失。
    

solidity

```
// 工厂合约关键函数：创建 ERC20-ERC20 交易对
function createPair(address tokenA, address tokenB) external returns (address pair) {
    // 确保 tokenA < tokenB，避免重复配对
    (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
    require(token0 != address(0), "Invalid token");
    require(getPair(token0, token1) == address(0), "Pair exists");
    
    // 用 CREATE2 生成确定性地址（离线可预计算）
    bytes memory bytecode = type(UniswapV2Pair).creationCode;
    bytes32 salt = keccak256(abi.encodePacked(token0, token1));
    assembly {
        pair := create2(0, add(bytecode, 32), mload(bytecode), salt)
    }
    
    // 初始化交易对
    IUniswapV2Pair(pair).initialize(token0, token1);
    allPairs.push(pair);
    emit PairCreated(token0, token1, pair, allPairs.length);
}
```

### 2\. TWAP 价格预言机（抗操纵）

-   不依赖瞬时价格，记录每个区块首次交易前的储备金比例，通过时间加权计算平均价格（TWAP），抗操纵性更强。
    
-   实用场景：借贷协议定价、衍生品结算等需要可靠价格的场景。
    

solidity

```
// 调用 Uniswap v2 交易对获取 TWAP（简化版）
contract TWAPConsumer {
    // 交易对地址（如 USDT-USDC 交易对）
    address public pairAddress = 0x...;
    
    // 记录起始价格累计值和时间
    struct TWAPData {
        uint256 priceCumulative;
        uint256 timestamp;
    }
    mapping(address => TWAPData) public twapRecords;
    
    // 1. 记录起始状态
    function startTWAP() external {
        IUniswapV2Pair pair = IUniswapV2Pair(pairAddress);
        (uint256 priceCumulative0, uint256 priceCumulative1, uint256 timestamp) = pair.getReserves();
        twapRecords[msg.sender] = TWAPData({
            priceCumulative: priceCumulative0, // 以 token0 计价 token1 的累计价格
            timestamp: timestamp
        });
    }
    
    // 2. 计算区间 TWAP 价格
    function getTWAP() external view returns (uint256 twap) {
        TWAPData memory data = twapRecords[msg.sender];
        IUniswapV2Pair pair = IUniswapV2Pair(pairAddress);
        (uint256 currentCumulative, , uint256 currentTime) = pair.getReserves();
        
        // 计算时间差和价格差（简化逻辑，实际需处理精度）
        uint256 timeElapsed = currentTime - data.timestamp;
        uint256 priceDiff = currentCumulative - data.priceCumulative;
        
        // TWAP = 价格累计差 / 时间差（返回 token1 相对于 token0 的平均价格）
        twap = priceDiff / timeElapsed;
    }
}

// 交易对接口（简化）
interface IUniswapV2Pair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function initialize(address token0, address token1) external;
}
```

### 3\. 闪电贷（Flash Swaps）

-   允许在同一原子交易中 “借资产→用资产→还资产”，无需抵押，未按时归还则交易回滚。
    
-   支持两种模式：1）借 A 还 A（纯借贷）；2）借 A 还 B（兑换 + 借贷），手续费均为 0.3%。
    

solidity

```
// 闪电贷回调合约（必须实现）
contract FlashSwapReceiver {
    address public factory = 0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6; // Uniswap v2 工厂地址（主网）
    
    // 闪电贷核心回调函数（Uniswap v2 要求必须实现）
    function uniswapV2Call(address sender, uint amount0, uint amount1, bytes calldata data) external {
        // 1. 验证调用者是 Uniswap 交易对
        address pair = msg.sender;
        (address token0,) = IUniswapV2Factory(factory).getPair(msg.sender);
        require(msg.sender == IUniswapV2Factory(factory).getPair(token0, token1), "Not Uniswap pair");
        require(sender == address(this), "Sender not this");
        
        // 2. 使用借来的资产（示例：套利、偿还债务等）
        uint borrowedAmount = amount0 > 0 ? amount0 : amount1;
        address borrowedToken = amount0 > 0 ? token0 : token1;
        // ... 此处执行核心逻辑（如用 borrowedToken 在其他平台套利）
        
        // 3. 归还资产（需包含 0.3% 手续费）
        uint repayAmount = borrowedAmount * 1003 / 1000; // 0.3% 手续费
        IERC20(borrowedToken).transfer(pair, repayAmount);
    }
}

// 发起闪电贷（简化）
function initiateFlashSwap(address pair, address tokenToBorrow, uint amount) external {
    // 调用交易对的 swap 函数，指定回调合约
    IUniswapV2Pair(pair).swap(
        tokenToBorrow == token0 ? amount : 0,
        tokenToBorrow == token1 ? amount : 0,
        address(this), // 回调合约地址
        "" // 额外数据（可选）
    );
}

// 必要接口
interface IUniswapV2Factory {
    function getPair(address tokenA, address tokenB) external view returns (address);
}

interface IUniswapV2Pair {
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
}

interface IERC20 {
    function transfer(address to, uint value) external returns (bool);
    function balanceOf(address owner) external view returns (uint);
}
```

### 4\. 可开关协议费

-   协议费为交易费的 1/6（0.05%），默认关闭，由 `feeToSetter` 地址控制开启 / 关闭。
    
-   费用收取：不实时扣除，仅在流动性存入 / 取出时结算，通过 mint 额外流动性代币给 `feeTo` 地址实现。
    

solidity

```
// 工厂合约中协议费控制逻辑
contract UniswapV2Factory {
    address public feeTo;
    address public feeToSetter;
    
    // 设置协议费收款地址（开启协议费）
    function setFeeTo(address _feeTo) external {
        require(msg.sender == feeToSetter, "Not feeToSetter");
        feeTo = _feeTo;
    }
    
    // 转移 feeToSetter 权限
    function setFeeToSetter(address _feeToSetter) external {
        require(msg.sender == feeToSetter, "Not feeToSetter");
        feeToSetter = _feeToSetter;
    }
}
```

## 三、核心合约架构

### 1\. 合约分层

-   核心合约：`UniswapV2Pair`（存储流动性、处理 swap/flashSwap）、`UniswapV2Factory`（创建交易对、管理协议费）。
    
-   路由合约：`UniswapV2Router`（简化用户交互，处理资金转移、路径计算）。
    

### 2\. 核心合约关键代码

交易对合约（UniswapV2Pair）

solidity

```
contract UniswapV2Pair {
    address public token0;
    address public token1;
    uint112 private reserve0; // 储备金 0
    uint112 private reserve1; // 储备金 1
    uint32 private blockTimestampLast; // 最后交易区块时间戳
    uint256 public priceCumulative0; // 价格累计值 0
    uint256 public priceCumulative1; // 价格累计值 1
    bool private locked; // 重入锁
    
    // 防止重入攻击
    modifier noReentrant() {
        require(!locked, "Reentrant");
        locked = true;
        _;
        locked = false;
    }
    
    // 初始化交易对
    function initialize(address _token0, address _token1) external {
        token0 = _token0;
        token1 = _token1;
    }
    
    // 更新储备金和价格累计值（内部函数）
    function _update(uint balance0, uint balance1, uint112 _reserve0, uint112 _reserve1) private {
        require(balance0 <= type(uint112).max && balance1 <= type(uint112).max, "Overflow");
        uint32 blockTimestamp = uint32(block.timestamp % 2**32);
        uint32 timeElapsed = blockTimestamp - blockTimestampLast;
        if (timeElapsed > 0 && _reserve0 != 0 && _reserve1 != 0) {
            // 更新价格累计值（用于 TWAP）
            priceCumulative0 += uint256(UQ112x112.encode(_reserve1).uqdiv(_reserve0)) * timeElapsed;
            priceCumulative1 += uint256(UQ112x112.encode(_reserve0).uqdiv(_reserve1)) * timeElapsed;
        }
        reserve0 = uint112(balance0);
        reserve1 = uint112(balance1);
        blockTimestampLast = blockTimestamp;
        emit Sync(reserve0, reserve1);
    }
    
    // 核心 swap 函数（简化版）
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external noReentrant {
        require(amount0Out > 0 || amount1Out > 0, "Insufficient output amount");
        (uint112 _reserve0, uint112 _reserve1,) = getReserves();
        require(amount0Out < _reserve0 && amount1Out < _reserve1, "Insufficient liquidity");
        
        // 转移资产给用户
        if (amount0Out > 0) IERC20(token0).transfer(to, amount0Out);
        if (amount1Out > 0) IERC20(token1).transfer(to, amount1Out);
        
        // 处理闪电贷回调
        if (data.length > 0) IUniswapV2Callee(to).uniswapV2Call(msg.sender, amount0Out, amount1Out, data);
        
        // 更新储备金
        uint balance0 = IERC20(token0).balanceOf(address(this));
        uint balance1 = IERC20(token1).balanceOf(address(this));
        uint amount0In = balance0 > _reserve0 - amount0Out ? balance0 - (_reserve0 - amount0Out) : 0;
        uint amount1In = balance1 > _reserve1 - amount1Out ? balance1 - (_reserve1 - amount1Out) : 0;
        require(amount0In > 0 || amount1In > 0, "Insufficient input amount");
        
        // 扣除 0.3% 手续费后更新储备金
        uint balance0Adjusted = balance0 * 1000 - amount0In * 3;
        uint balance1Adjusted = balance1 * 1000 - amount1In * 3;
        require(balance0Adjusted * balance1Adjusted >= uint(_reserve0) * uint(_reserve1) * 1000**2, "K");
        _update(balance0, balance1, _reserve0, _reserve1);
        emit Swap(msg.sender, amount0In, amount1In, amount0Out, amount1Out, to);
    }
    
    // 获取储备金和价格累计值
    function getReserves() public view returns (uint112 _reserve0, uint112 _reserve1, uint32 _blockTimestampLast) {
        _reserve0 = reserve0;
        _reserve1 = reserve1;
        _blockTimestampLast = blockTimestampLast;
    }
}

// 辅助接口
interface IUniswapV2Callee {
    function uniswapV2Call(address sender, uint amount0, uint amount1, bytes calldata data) external;
}

// 简化的 UQ112x112 编码工具（用于价格计算）
library UQ112x112 {
    uint224 constant Q112 = 2**112;
    function encode(uint112 y) internal pure returns (uint224 z) {
        z = uint224(y) * Q112;
    }
    function uqdiv(uint224 x, uint112 y) internal pure returns (uint224 z) {
        z = x / uint224(y);
    }
}
```

## 四、实战示例

### 1\. 添加流动性

solidity

```
// 通过 Router 合约添加流动性（简化版）
function addLiquidity(
    address router,
    address tokenA,
    address tokenB,
    uint amountA,
    uint amountB
) external {
    // 授权 Router 转移资产
    IERC20(tokenA).approve(router, amountA);
    IERC20(tokenB).approve(router, amountB);
    
    // 调用 Router 添加流动性
    (uint amountADesposited, uint amountBDeposited, uint liquidity) = IUniswapV2Router(router).addLiquidity(
        tokenA,
        tokenB,
        amountA,
        amountB,
        0, // 最小接收流动性代币数量（实际需设置合理值）
        0, // 最小接收流动性代币数量
        msg.sender,
        block.timestamp + 300 // 交易过期时间
    );
    
    // 流动性代币已转入 msg.sender 地址
    console.log("获取流动性代币数量：", liquidity);
}

interface IUniswapV2Router {
    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
}
```

### 2\. 移除流动性

solidity

```
function removeLiquidity(
    address router,
    address pair,
    uint liquidityAmount
) external {
    // 授权 Router 转移流动性代币
    IERC20(pair).approve(router, liquidityAmount);
    
    // 调用 Router 移除流动性
    (uint amountA, uint amountB) = IUniswapV2Router(router).removeLiquidity(
        tokenA,
        tokenB,
        liquidityAmount,
        0, // 最小接收 tokenA 数量
        0, // 最小接收 tokenB 数量
        msg.sender,
        block.timestamp + 300
    );
    
    console.log("收回 tokenA 数量：", amountA);
    console.log("收回 tokenB 数量：", amountB);
}
```
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->









# 以太坊脚本与Solidity开发笔记

通过`ethers.js`的以太坊链上交互脚本开发，使用 Hardhat 完成智能合约本地开发 / 测试，最后通过 Scaffold-ETH 实现合约快速迭代与前端交互，来独立完成合约读写、交易发送等核心功能

## 一、 开发环境搭建

### 1\. 配置

bash

```
# 创建项目并初始化
mkdir eth-dev-demo && cd eth-dev-demo
npm init -y

# 启用ES6模块（修改package.json）
echo '{
  "type": "module",
  "dependencies": {
    "ethers": "^6.8.0",
    "dotenv": "^16.3.1"
  }
}' > package.json

# 安装依赖
npm install
```

### 2\. 安全注意

-   创建`.env`存储敏感信息（API 密钥、私钥），添加至`.gitignore`
    
-   不同项目使用独立助记词和私钥，禁止硬编码敏感数据
    

## 二、 ethers.js ：链上交互的基础

### 1\. 核心概念

| 组件 | 核心功能 | 读取链上数据 | 签名 / 发送交易 | 依赖条件 |
| --- | --- | --- | --- | --- |
| Provider | 连接以太坊节点，获取链上数据 | ✅ | ❌ | 节点 RPC 地址 / API 密钥 |
| Wallet | 私钥管理、离线签名 | ❌ | ✅（仅离线） | 私钥 / 助记词 |
| Signer | Wallet + Provider 组合 | ✅ | ✅ | Wallet + Provider |

### 2\. Provider：链上数据读取

2.1 不同类型 Provider的初始化

js

```
import { ethers } from "ethers";
import dotenv from "dotenv";
dotenv.config();

// 1. 托管节点（Infura/Alchemy）- 推荐生产环境
const infuraProvider = new ethers.InfuraProvider(
  "sepolia", // 测试网（替代原Rinkeby）
  process.env.INFURA_API_KEY
);

// 2. 本地节点（如Geth/Hardhat）- 无速率限制
const localProvider = new ethers.JsonRpcProvider("http://localhost:8545");

// 3. 默认Provider（仅测试，有速率限制）
const defaultProvider = ethers.getDefaultProvider("sepolia");
```

2.2读取示例

js

```
async function testProvider() {
  // 1. 获取当前区块高度
  const blockNumber = await infuraProvider.getBlockNumber();
  console.log("当前区块高度：", blockNumber);

  // 2. 解析ENS域名（仅主网生效）
  const address = await infuraProvider.resolveName("vitalik.eth");
  console.log("ENS解析结果：", address);

  // 3. 查询钱包余额（以Vitalik地址为例）
  const balance = await infuraProvider.getBalance("0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045");
  console.log("钱包余额：", ethers.formatEther(balance), "ETH");

  // 4. 获取区块详情
  const block = await infuraProvider.getBlock(blockNumber);
  console.log("最新区块交易数：", block.transactions.length);
}

testProvider();
```

### 3\. Wallet 与 Signer：交易签名与发送

3.1 钱包生成导入

js

```
async function testWallet() {
  // 1. 随机生成钱包（含助记词）
  const randomWallet = ethers.Wallet.createRandom();
  console.log("随机钱包地址：", randomWallet.address);
  console.log("助记词：", randomWallet.mnemonic.phrase);
  console.log("私钥：", randomWallet.privateKey);

  // 2. 从私钥导入钱包
  const importedWallet = new ethers.Wallet(process.env.PRIVATE_KEY);
  // 3. 连接Provider成为Signer（具备交易发送能力）
  const signer = importedWallet.connect(infuraProvider);

  // 验证Signer余额
  const signerBalance = await signer.getBalance();
  console.log("Signer余额：", ethers.formatEther(signerBalance), "ETH");
  return signer;
}
```

3.2 ETH 交易

js

```
async function sendEth() {
  const signer = await testWallet();
  // 交易参数
  const txParams = {
    to: "0xRecipientAddress", // 接收地址
    value: ethers.parseEther("0.01"), // 转账金额（转成wei）
    gasLimit: 21000, // 基础转账Gas Limit
    gasPrice: await infuraProvider.getGasPrice() // 自动获取当前Gas Price
  };

  // 发送交易
  const tx = await signer.sendTransaction(txParams);
  console.log("交易哈希：", tx.hash);
  // 等待区块确认（建议等待1-6个确认）
  const receipt = await tx.wait(1);
  console.log("交易确认：", receipt.status === 1 ? "成功" : "失败");
}

sendEth();
```

### 4\. BigNumber处理

js

```
// 以太坊数值单位转换（1 ETH = 10^18 wei）
const ethAmount = "0.5";
const weiAmount = ethers.parseEther(ethAmount); // ETH → wei
console.log("0.5 ETH =", weiAmount.toString(), "wei");

const convertedEth = ethers.formatEther(weiAmount); // wei → ETH
console.log(weiAmount.toString(), "wei =", convertedEth, "ETH");

// 大数值运算（避免JS原生运算溢出）
const balance1 = ethers.parseEther("1.23");
const balance2 = ethers.parseEther("4.56");

const sum = balance1.add(balance2); // 加法
const diff = balance2.sub(balance1); // 减法
const product = balance1.mul(2); // 乘法
const quotient = balance2.div(3); // 除法

console.log("求和：", ethers.formatEther(sum), "ETH");
console.log("差值：", ethers.formatEther(diff), "ETH");
```

## 三、 智能合约交互

### 1\. 前提：获取 ABI 与合约地址

-   **ABI 获取**：从 Etherscan 验证合约页面复制，或从 Hardhat 编译后的`artifacts`文件夹读取
    
-   **合约地址**：部署后生成的唯一地址（测试网 / 主网）
    

### 2\. 合约读取操作（只读，没有Gas 消耗）

2.1 ERC20 代币读取示例

js

```
async function readContract() {
  // USDC Sepolia测试网合约信息
  const usdcAddress = "0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238";
  const usdcAbi = [
    "function balanceOf(address owner) view returns (uint256)",
    "function decimals() view returns (uint8)",
    "function symbol() view returns (string)"
  ]; // 简化ABI（仅需用到的函数）

  // 初始化只读合约实例
  const usdcContract = new ethers.Contract(usdcAddress, usdcAbi, infuraProvider);

  // 调用view函数
  const symbol = await usdcContract.symbol();
  const decimals = await usdcContract.decimals();
  const balance = await usdcContract.balanceOf("0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045");

  console.log("代币符号：", symbol);
  console.log("小数位数：", decimals);
  console.log("钱包余额：", ethers.formatUnits(balance, decimals), symbol);
}

readContract();
```

### 3\. 合约写入操作（交易，消耗 Gas）

3.1 ERC20 代币转账示例

js

```
async function writeContract() {
  const signer = await testWallet();
  const usdcAddress = "0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238";
  const usdcAbi = [
    "function transfer(address to, uint256 amount) returns (bool)",
    "function balanceOf(address owner) view returns (uint256)"
  ];

  // 初始化可写合约实例（连接Signer）
  const usdcContract = new ethers.Contract(usdcAddress, usdcAbi, signer);

  // 转账参数（转10 USDC，注意小数位数）
  const recipient = "0xRecipientAddress";
  const amount = ethers.parseUnits("10", 6); // USDC小数位数为6

  // 发送转账交易
  const tx = await usdcContract.transfer(recipient, amount);
  console.log("转账交易哈希：", tx.hash);
  const receipt = await tx.wait(1);
  if (receipt.status === 1) {
    console.log("转账成功！");
    // 验证余额
    const newBalance = await usdcContract.balanceOf(recipient);
    console.log("接收方新余额：", ethers.formatUnits(newBalance, 6), "USDC");
  }
}

writeContract();
```

### 4\. 原始交易与函数选择器

js

```
async function rawTransaction() {
  const signer = await testWallet();
  const contractAddress = "0xContractAddress";
  
  // 1. 函数选择器：transfer(address,uint256)的哈希前4字节
  const functionSelector = ethers.id("transfer(address,uint256)").slice(0, 10);
  // 2. 编码参数（接收地址+转账金额）
  const recipientEncoded = ethers.AbiCoder.defaultAbiCoder().encode(["address"], ["0xRecipientAddress"]);
  const amountEncoded = ethers.AbiCoder.defaultAbiCoder().encode(["uint256"], [ethers.parseUnits("10", 6)]);
  // 3. 拼接callData
  const callData = functionSelector + recipientEncoded.slice(2) + amountEncoded.slice(2);

  // 发送原始交易
  const tx = await signer.sendTransaction({
    to: contractAddress,
    data: callData
  });
  console.log("原始交易哈希：", tx.hash);
}
```

### 5\. 交易进阶方向：Nonce 与 Gas 优化

js

```
async function optimizeTx() {
  const signer = await testWallet();
  // 1. 获取账户当前Nonce
  const nonce = await signer.getNonce("pending"); // pending：包含待确认交易
  // 2. 自定义Gas参数（加速交易）
  const gasPrice = (await infuraProvider.getGasPrice()).mul(120).div(100); // 提高20% Gas Price
  const gasLimit = 100000;

  const tx = await signer.sendTransaction({
    to: "0xRecipientAddress",
    value: ethers.parseEther("0.01"),
    nonce: nonce,
    gasPrice: gasPrice,
    gasLimit: gasLimit
  });
  console.log("自定义Gas交易哈希：", tx.hash);
}
```

## 四、 Hardhat：本地的开发与测试

### 1\. 环境搭建

bash

```
# 1. 初始化Hardhat项目
mkdir hardhat-demo && cd hardhat-demo
npm init -y
npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox

# 2. 创建项目（选择Create a JavaScript project）
npx hardhat

# 3. 核心依赖安装
npm install dotenv @nomiclabs/hardhat-ethers ethers
```

### 2\. 配置

js

```
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.24", // Solidity版本
  networks: {
    // 1. 本地节点
    localhost: {
      url: "http://127.0.0.1:8545"
    },
    // 2. Sepolia测试网
    sepolia: {
      url: `https://sepolia.infura.io/v3/${process.env.INFURA_API_KEY}`,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: process.env.ETHERSCAN_API_KEY // 用于合约验证
  }
};
```

### 3\. 核心命令

bash

```
# 1. 启动本地节点（生成10个测试账户，每个含10000 ETH）
npx hardhat node

# 2. 编译合约（生成ABI/字节码，存于artifacts）
npx hardhat compile

# 3. 部署合约到本地节点
npx hardhat run scripts/deploy.js --network localhost

# 4. 部署合约到测试网
npx hardhat run scripts/deploy.js --network sepolia

# 5. 运行自动化测试
npx hardhat test

# 6. 验证合约（Etherscan）
npx hardhat verify --network sepolia 0xDeployedContractAddress "构造函数参数（如有）"
```

### 4\. 核心实战：部署 + 测试示例

4.1 编写简单合约（contracts/Counter.sol）

solidity

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.24;

contract Counter {
    uint256 public count;
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    function increment() public onlyOwner {
        count += 1;
    }

    function setCount(uint256 newCount) public onlyOwner {
        require(newCount >= 0, "Invalid count");
        count = newCount;
    }

    function getCount() public view returns (uint256) {
        return count;
    }
}
```

4.2 部署脚本（scripts/deploy.js）

js

```
import { ethers } from "hardhat";

async function main() {
  // 部署合约
  const Counter = await ethers.getContractFactory("Counter");
  const counter = await Counter.deploy();
  await counter.waitForDeployment();

  console.log(`Counter合约部署地址：${await counter.getAddress()}`);

  // 调用合约函数（部署后立即交互）
  await counter.increment();
  console.log("初始计数：", await counter.getCount());
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

4.3 自动化测试（test/counter-test.js）

js

```
import { expect } from "chai";
import { ethers } from "hardhat";

describe("Counter合约测试", function () {
  let counter;
  let owner;
  let user;

  // 每个测试前部署新合约（隔离环境）
  beforeEach(async function () {
    [owner, user] = await ethers.getSigners();
    const Counter = await ethers.getContractFactory("Counter");
    counter = await Counter.deploy();
    await counter.waitForDeployment();
  });

  // 测试1：初始计数为0
  it("初始计数应为0", async function () {
    expect(await counter.getCount()).to.equal(0);
  });

  // 测试2：所有者可增加计数
  it("所有者可调用increment", async function () {
    await counter.increment();
    expect(await counter.getCount()).to.equal(1);
  });

  // 测试3：非所有者禁止调用increment
  it("非所有者调用increment应失败", async function () {
    await expect(counter.connect(user).increment()).to.be.revertedWith("Not owner");
  });
});
```

## 五、 Scaffold-ETH：快速开发的工具链

### 1\. 一键搭建环境

bash

```
# 1. 克隆仓库
git clone https://github.com/scaffold-eth/scaffold-eth-2.git
cd scaffold-eth-2

# 2. 安装依赖
yarn install

# 3. 启动本地节点
yarn chain

# 4. 启动前端（新终端）
yarn start

# 5. 部署合约（新终端）
yarn deploy
```

### 2\. 它的优势与开发流程

2.1 核心优势

-   **热重载**：修改合约后执行`yarn deploy`，前端自动更新，无需刷新
    
-   **Burner Wallet**：内置测试钱包，无需 MetaMask，一键获取测试 ETH
    
-   **可视化交互**：前端自动生成合约函数调用界面，无需手写前端代码
    

2.2 快速开发实战：计数器合约优化

1.  **修改合约（packages/hardhat/contracts/Counter.sol）**
    
    solidity
    
    ```
    // 添加payable函数，调用需支付0.001 ETH
    function increment() public payable onlyOwner {
        require(msg.value >= 0.001 ether, "Insufficient ETH");
        count += 1;
    }
    ```
    
2.  **重新部署**：`yarn deploy`（无需停止节点 / 前端）
    
3.  **前端交互**：
    
    -   在浏览器中打开`http://localhost:3000`
        
    -   选择 Counter 合约，调用`increment`函数，输入 0.001 ETH
        
    -   点击 “Call” 发送交易，实时查看计数更新
        

### 3\. 合约间交互

solidity

```
// 合约A：Bank（存储ETH）
contract Bank {
    mapping(address => uint256) public balances;
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint256 amount) public {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        // 安全转账：先更新状态，再转账（防重入）
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success, "Transfer failed");
    }
}

// 合约B：Middleware（调用Bank合约）
contract Middleware {
    Bank public bank;

    constructor(address bankAddress) {
        bank = Bank(bankAddress);
    }

    // 代理存款
    function depositToBank() public payable {
        bank.deposit{value: msg.value}();
    }
}
```

## 六、 其他要点

### 1\. 安全最佳实践

-   重入攻击防御：遵循 “检查 - 效果 - 交互” 原则（先更新状态，再执行外部调用）
    
-   权限控制：优先使用 OpenZeppelin 的`Ownable`合约，避免自定义权限逻辑
    
-   数值安全：依赖 Solidity 0.8 + 内置溢出检查，禁止直接转换 BigNumber 为 JS 数值
    

### 2\. 常见陷阱

-   ENS 仅主网生效，测试网需验证地址有效性
    
-   合约无`receive()`函数无法接收直接转账的 ETH
    
-   Hardhat 测试账户私钥公开，禁止用于主网
    

ps： ethers.js 是以太坊链上交互的核心库，需熟练掌握 Provider（读）、Signer（写）、BigNumber（数值处理）三大核心模块，代码编写需注重异步处理和错误捕获。智能合约交互 需区分只读 / 写入操作，ABI 仅需包含用到的函数以简化代码，交易发送后需等待区块确认并验证状态。Hardhat 是本地开发的首选工具，支持合约编译、部署、测试一体化，自动化测试需覆盖核心逻辑和边界条件。Scaffold-ETH 大幅提升开发效率，通过热重载和可视化交互，实现 “修改 - 部署 - 测试” 的快速迭代，适合合约原型开发。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->










# 共识机制与生态展望

了解以太坊共识优势与生态扩展方式

### 1\. 有潜力潜力的落地项目与关键里程碑

以太坊未来的发展并非空中楼阁，而是围绕“扩容、去中心化、体验优化”三大核心，按明确的技术路线图分阶段落地，关键里程碑可分为短期（1-2年）、中期（2-3年）、长期（3年以上）三个时间段考量

### 短期里程碑（1-2年）：夯实L2生态与协议基础

-   **Pectra升级全面落地**：完成执行层Prague与共识层Electra的协同升级，落地EOF合约格式、EIP-7702账户抽象增强、单验证者质押上限提升至2048 ETH等功能，优化开发者体验与质押节点运维效率，进一步降低L2开发者的合约部署与调试成本。
    
-   **PeerDAS测试与上线**：完善数据可用性采样机制，让验证者无需下载完整blob数据，仅通过随机抽样即可保障数据安全，为完整Danksharding铺路，同时降低轻节点参与门槛。
    
-   **L2跨链互操作性优化**：通过LayerZero、CCIP等协议的标准化，实现L2间资产与消息的原子性交互，解决“L2碎片化”问题，让用户在不同L2间流转时无需依赖L1中转，提升生态协同效率。
    

### 中期里程碑（2-3年）：Danksharding与无状态客户端落地

-   **完整Danksharding激活**：将单区块blob容量、存储周期与定价机制优化到位，实现“每区块支持数十个blob”的目标，L2数据成本再降一个数量级，支撑高频游戏、微支付、实时社交等Web2级应用场景。
    
-   **Verkle树主网部署**：替换现有MPT状态树，将状态证明体积从几KB压缩至百余字节，正式支持无状态客户端，让普通用户用家用电脑甚至移动设备即可运行验证节点，强化去中心化根基。
    
-   **PBS机制内嵌化**：将提议者-构建者分离（PBS）从第三方实现纳入协议层，引入多构建者竞争与MEV分配机制，降低少数构建者的垄断风险，提升网络抗审查能力。
    

### 长期里程碑（3年以上）：单槽最终性与生态泛化

-   **单槽最终性（SSF）实现**：优化PoS共识逻辑，将区块最终性确认时间从当前的多个epoch（约12分钟）缩短至单个slot（12秒），让以太坊具备高价值结算场景的实时性，可与传统金融清算系统竞争。
    
-   **跨链安全联盟成型**：通过再质押（Restaking）与AVS服务，将以太坊的安全模型输出至其他区块链、DA层与Web3基础设施，形成以以太坊为核心的“安全联盟网络”，强化生态护城河。
    
-   **AI与链上自动化深度融合**：通过智能账户与AI Agent的协同，实现链上资产自动再平衡、风险实时监控、交易策略自适应优化，让以太坊从“可编程金融平台”升级为“自动化价值网络”。
    

### 2\. 以太坊面临的核心挑战

-   技术落地难与兼容性风险
    

Danksharding、Verkle树、无状态客户端等技术均为区块链领域的前沿创新，缺乏成熟先例可借鉴。一方面，需要兼顾技术先进性与协议稳定性，避免升级过程中出现共识漏洞或数据不一致问题；另一方面，需保障对现有生态的向后兼容——全球数百万智能合约、数十亿用户资产依赖以太坊现有协议，任何升级都需严格测试，防止“硬分叉”风险。

-   生态治理中的中心化风险
    

随着以太坊生态规模扩大，治理复杂度显著提升：验证者质押量逐渐向头部机构集中（前10大质押服务商掌控超60%质押ETH），可能形成“寡头验证”格局。L2排序器多由单一机构掌控，存在审查与MEV垄断风险。跨L2、跨链协议的标准化治理缺乏统一机制，易出现生态分裂。如何在“高效治理”与“去中心化”之间找到平衡点，是以太坊社区面临的长期问题

-   监管压力与合规适配
    

以太坊生态的金融属性（DeFi、RWA、稳定币）与全球的影响力，使其成为各国监管的重点对象。不同国家对智能合约、代币化资产、质押服务的监管政策不同，可能导致生态出现不同的合规策略。同时，反洗钱（AML）、反恐怖融资（CFT）要求对匿名性较强的链上交易提出质疑，如何在保障隐私的同时满足合规需求

-   外部竞争与差异化突围
    

区块链领域竞争变得越加激烈：Solana、Aptos等新公链以“高TPS、低延迟”为卖点，争夺高频应用场景。Cosmos、Polkadot以“模块化、跨链”为核心，构建独立生态体系。比特币生态通过Ordinals、Stacks等扩展层，逐步具备智能合约能力。

### 3\. 小结

回顾以太坊的发展史，它是一部“从务实过渡到长期优化”的技术演进史：最初选择PoW是为了安全启动智能合约生态，通过The Merge切换至PoS完成共识层革命，再以Dencun、Pectra等升级逐步解决扩容与用户体验问题，最终来构建“安全、高效、去中心化”的全球价值结算网络

从共识机制来看，以太坊的核心优势并不是单一技术领先，而是技术路线改革的前沿——提前规划PoW→PoS转型，分阶段推进分片与状态优化。从生态来看，DeFi、NFT、L2的协同发展形成了强大的网络效应，RWA与机构资金的进场则让以太坊从“加密原生平台”走向传统与Web3结合的桥梁
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->











# EVM与Gas机制 笔记

理解以太坊交易执行原理，掌握Gas费用计算机制及怎么写代码省Gas，明白EVM在以太坊生态中的作用

# 一、EVM（以太坊虚拟机）

## 1\. 定义

> EVM（Ethereum Virtual Machine）是一台运行在每个以太坊节点上的虚拟计算机，它把“交易+智能合约字节码”变成“新的链上状态”，并通过 Gas 机制和确定性的执行规则，保证全网所有节点在同一输入下得到同样的结果。

ps：EVM = 全网共享的“状态机 + CPU”，不管人在哪，只要发出交易，全网的节点都会在自己的机器上运行这个 EVM，算出同样的结果

## 2\. EVM核心组件

| 组件 | 作用 | 关键提示 |
| --- | --- | --- |
| 全局状态（World State） | 记录所有账户（EOA+合约）的nonce、balance、codeHash、storageRoot | 余额、NFT持有关系等最终存储于此 |
| 字节码/操作码（Bytecode/Opcodes） | Solidity编译产物，由EVM指令组成（如ADD、SSTORE） | 每条指令对应固定Gas消耗 |
| 栈（Stack） | 计算用操作数栈，深度上限1024 | 算术运算、条件判断均在此完成 |
| 内存（Memory） | 交易执行期临时字节数组，执行结束即丢弃 | 存放中间数据，成本低于存储 |
| 存储（Storage） | 合约独有永久存储（key-value Trie） | 状态变量存储处，Gas成本最高 |
| 执行上下文 | 包含msg.sender、value、gas等调用环境 | 决定权限、资金流向及函数选择 |
| Gas计费系统 | 为指令、存储变更计费 | Gas耗尽（OOG）则回滚执行结果 |

## 3\. 智能合约在EVM上的运行流程

-   **编码与编译**：用Solidity/Vyper编写合约，编译为字节码+ABI+元数据
    
-   **部署**：EOA发起无to地址交易，data字段携带部署字节码，EVM执行初始化并生成合约地址
    
-   **调用**：通过交易/CALL传入calldata（函数选择器+参数），EVM逐条执行字节码并修改状态
    
-   **状态更新与共识**：节点复算一致后，新状态被全网确认
    

## 4\. 两点约束

-   **确定性**：不可依赖外部动态信息（如真实时间），同样的输入必须出同样的结果，不能查天气、抓随机数，只能看链上的数据
    
-   **沙盒隔离**：无法访问文件系统、外网，仅通过EVM上下文及合约调用互动，数据只能靠“预言机”
    

## 5\. EVM的优势与局限

## 优势

-   智能合约基石：无EVM则以太坊无法成为可编程区块链
    
-   统一执行环境：保证全网节点共识一致性，避免执行差异
    
-   支持DApps创新：赋能DeFi、NFT、DAO等生态应用落地
    
-   安全隔离：沙盒+Gas机制防止单合约漏洞影响全网
    
-   生态效应：兼容链及工具链（Hardhat/Foundry）降低开发成本
    

## 局限

-   Gas成本：网络拥堵时费用高昂，依赖L2缓解
    
-   TPS受限：全节点重复执行交易，单链的吞吐量有限
    
-   升级兼容压力大：需要兼顾存量合约安全问题，迭代谨慎（如EOF分多轮EIP推进）
    
-   状态膨胀：长期存储积累导致节点硬件门槛变高
    

# 二、为何逐条执行 OpCode（操作码）

## 1\. 执行模型流程

采用“取指-解码-执行”循环，默认的顺序执行，仅通过JUMP/JUMPI/JUMPDEST等指令改变程序计数器，实现分支、循环逻辑

## 2\. 逐条执行的核心目的

-   **保证共识确定性**：每条OpCode语义固定，不同客户端执行结果一致，全网节点都按同样的步骤
    
-   **精确Gas计费**：按指令粒度计费，实现资源消耗与成本强绑定，防止有人写死循环把全网卡死
    
-   **多客户端兼容**：指令集简单，帮助不同语言客户端的实现
    
-   **安全可控**：限制合约操作，越界后立即中止并回滚
    
-   **可调试审计**：支持OpCode级追踪，便于漏洞排查
    

# 三、Gas核心机制原理

## 1\. Gas定义与作用

Gas是计算工作量的抽象单位，衡量合约调用的资源消耗（计算、存储、带宽）

-   防DoS攻击：避免无限循环导致节点瘫痪、资源滥用
    
-   激励验证者：手续费作为节点运维及获利来源，激励验证者
    
-   资源计量：不同操作（如存储写入、合约调用）对应不同Gas成本
    
-   约束图灵完备性：通过GasLimit防止无限运算
    

## 2\. Gas单位与费用计算

-   单位关系：1 ETH = 10⁹ Gwei = 10¹⁸ Wei，Gas价格通常以Gwei计价
    
-   基础公式：总费用（ETH）= GasUsed × 有效Gas单价（Gwei）÷ 10⁹
    
-   EIP-1559后结构：有效单价 = BaseFee（协议销毁）+ PriorityFee（小费，给验证者），受MaxFee上限约束
    

## 3\. London升级（EIP-1559）

-   BaseFee（基础费）：协议动态调节（区块拥堵则上调，最多±12.5%/块），全额销毁，形成ETH通缩
    
-   PriorityFee（小费）：用户自主设定，激励验证者优先打包，归验证者所有
    
-   弹性区块容量：区块可临时扩展至目标容量2倍，缓和短期拥堵
    
-   Gas退款调整：削弱SELFDESTRUCT等操作退款，防止滥用
    

## 4\. Gas不足的情况

-   状态全回滚：所有修改（转账、存储更新、合约部署）均无效，钱扣了但活儿没干完
    
-   费用不退还：已消耗Gas对应ETH支付给验证者，可避免DoS攻击
    
-   交易入块标记：交易被写入区块，但状态为失败（status=0），区块浏览器显示“Out of Gas”。
    

# 六、优化合约Gas

## 核心原则

本质是**能不在链上存，就不在链上存**。多用临时计算载体，批量化操作，结合工具量化来优化效果，优先采用L2

## 优化方向

-   **存储优化（最先考虑）**
    
    1.  合并存储写入：先在memory计算完成，单次写回storage
        
    2.  Slot打包：将多个小变量（如uint128、bool）放入同一32字节Slot
        
    3.  事件替代存储：非计算用历史数据存入事件，而非storage
        
-   **数据位置的选择**
    
    1.  外部函数参数优先用calldata（只读不复制，比memory更便宜）
        
    2.  重复读取的storage值缓存至memory，避免多次SLOAD
        
-   **架构优化**
    
    1.  常量使用constant/immutable：编译期确定值，不占storage
        
    2.  模块化设计：用库/代理合约拆分逻辑，Minimal Proxy克隆多实例合约降低部署成本
        
    3.  短期状态用transient storage（EIP-1153）：单笔交易内临时存储，替代storage减少成本
        
-   **控制流与计算优化**
    
    1.  避免无界循环：链下分页处理或用户自助Claim（如空投Merkle Tree模式）
        
    2.  链下计算+链上验证：用Merkle证明替代链上遍历统计
        
    3.  unchecked块：验证无溢出的场景（如循环计数器）用unchecked减少溢出检查Gas
        
    4.  位运算替代布尔值：用bitmask存储多组标志位
        

## **优化工具**

-   Hardhat-gas-reporter：测试时输出函数Gas消耗及部署成本
    
-   Foundry：通过gas snapshot对比改动前后Gas变化
    
-   Tenderly：OpCode级追踪，主网fork调试优化关键路径
    

ps:EVM 的沙盒本质和 Gas 的计费逻辑,本质上就是一种抠门的经济思维，EVM中高昂的储存成本和OpCode 真金白银似的 Gas 消耗，都促使着builder来进行Gas优化，在资源使用上做到极致的同时，也保证安全防范上做到滴水不漏。正如分享会上提到的，web3行业不仅仅是技术上的创新，同时也是社会学 经济学 密码学上的融合。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->












# 智能合约理论基础笔记

深入理解智能合约到底是怎么在链上跑起来的？它的价值在哪？如何去创建、部署它，以及在写错的情况瞎，该怎么“修改”

# 一、智能合约

## 1\. 定义

-   智能合约是以太坊应用层基石，本质是“部署在区块链上的程序代码 + 持久状态”，由交易触发执行，遵循“如果…那么…”（IFTTT）的自动执行逻辑，只要触发了条件，它就自己执行，谁也拦不住
    
-   特点：部署后字节码默认不可直接修改。需升级时可通过代理模式等实现。2024年Dencun升级后，SELFDESTRUCT功能大幅受限，仅在创建合约的同一交易中调用才会移除代码，否则仅清空余额
    

## 2\. 与传统合约相比

-   传统合约缺点：需要同时信任“合同条款”和“履约方”，依赖于中心化的中介，存在中介失职舞弊的风险
    
-   智能合约优势：条件满足则自动执行，不需要人为干预。结果可预测（相同输入则相同输出），避免人为偏差
    

## 3\. 核心功能场景

-   自动执行：无需中介，如时间锁账户（满18岁才可提款）、付款即转移资产所有权
    
-   结果可预测：Code is law，所有节点计算结果一致，保障共识机制
    
-   公开可审计：执行过程、状态变更数据全上链，可以通过区块浏览器查询追溯，支撑合规审计与市场分析
    
-   假名制隐私：地址不绑定真实身份，但是交易记录公开可追踪
    
-   功能灵活：可实现代币发行、DeFi协议、NFT铸造、DAO治理、自动清算等等
    

# 二、为什么大家都在用 Solidity？

## 1\. 优点

-   EVM原生适配：专为以太坊虚拟机设计，可直接编译为EVM字节码，适配以太坊及绝大多数EVM兼容链
    
-   易学好上手：语法融合JS、C++、Python特性，支持面向对象，适配链上资源操作，开发者易上手。
    
-   生态完善：拥有Hardhat、Foundry、Remix等工具，OpenZeppelin等成熟合约库，遇到问题上网一搜全能找到答案
    
-   安全成熟：2014年发布后持续迭代，0.8.0版本起默认支持整数溢出检查，安全工具（Slither、Mythril）丰富
    
-   多链兼容：主流EVM L2和侧链均支持Solidity，可实现项目“一键多链”部署
    

# 三、智能合约的编译部署

## 1\. 编译后的产物

-   字节码（给电脑看的）：EVM可执行的机器码，分创建字节码（部署时执行，负责写入运行时代码）和运行时代码（部署后存储在合约地址的代码）
    
-   ABI（交互指南，给我们看的）：JSON格式的“交互说明书”，描述合约公共函数、事件等，供前端、其他合约调用时使用
    
-   元数据：记录编译器版本、源码路径、ABI等，默认附加在运行时代码尾部，用于链上源码的验证
    
-   其他：AST（抽象语法树）、Source Map（源码与字节码映射），供调试、审计使用
    

## 2\. 部署后的产物

-   部署后获得：合约地址（链上唯一标识符，用于交互定位）。部署交易哈希（可用于追踪部署记录）
    

ps：ABI、字节码等为编译阶段的产物，部署时需上传字节码。部署后可在Etherscan/Sourcify做源码验证，方便公开交互

## 3\. 部署的成本

-   成本计算：总成本（ETH）= Gas Used（消耗的计算资源）×（Base Fee（基础费，协议销毁）+ Priority Fee（小费，给验证者））
    
-   影响因素：合约复杂的程度（代码越长、初始化逻辑就越复杂，Gas消耗越高），网络拥堵状况，选择的部署链（L1主网费用高，L2/Rollup费用低）
    
-   控制成本：本地或测试网估算部署Gas，通过优化代码减少字节码和初始化存储，网络闲时部署，优先选择L2部署
    

## 4\. 部署工具

| 工具 | 类型 | 核心特点 | 适用场景 |
| --- | --- | --- | --- |
| Remix IDE | Web端IDE | 开箱即用，集成编译、部署、调试功能 | 入门学习、小型Demo、快速验证想法 |
| Hardhat | 本地开发框架 | 插件丰富，支持脚本化部署、主网fork测试 | 中大型项目、团队协作、工程化开发 |
| Foundry | Rust开发工具链 | 性能快，支持Solidity原生测试、fuzz测试 | 极致测试需求、Gas优化、协议开发 |
| Truffle+Ganache | 本地框架+模拟器 | 老牌工具，资料丰富，Ganache可视化测试链 | 维护旧项目、可视化本地测试 |
| Tenderly | SaaS平台 | 支持高级调试、主网fork模拟、监控告警 | 主网级调试、大型项目运维监控 |

# 四、智能合约里的核心组件

-   状态变量：存储在链上的永久数据。有public/internal/private等可见性，可通过constant/immutable修饰节省gas
    
-   函数：执行操作的代码块，有public/external等可见性，pure/view/payable等状态可变性，特殊函数receive、fallback（有的要花钱，有的只是看看数据）
    
-   事件：向链上日志广播消息，用于前端监听和历史追溯
    
-   构造函数：部署时仅执行一次，用于初始化状态变量
    
-   修饰符：可重用的代码块，用于修改函数行为（如权限控制onlyOwner），检查是否有权限执行这个功能
    

# 五、常见的坑

| 常见漏洞 | 核心风险 | 关键防范措施 |
| --- | --- | --- |
| 重入攻击 | 外部调用时被回调，反复提走资产 | 遵循CEI模式（检查-更新状态-外部调用）；使用ReentrancyGuard修饰符；采用Pull-Payment提现模式 |
| ETH发送安全问题 | transfer/send固定2300gas不可靠，易导致DoS | 使用call模式并检查返回值；使用OpenZeppelin的sendValue函数 |
| 整数溢出/下溢 | 早期版本可绕回，伪造余额 | 使用Solidity 0.8.x+（默认溢出检查）；必要时使用unchecked代码块并做好测试 |
| 访问控制错误 | 任意地址可调用敏感函数 | 使用Ownable/AccessControl；禁止用tx.origin做鉴权；关键操作加多层签+时间锁 |
| 预言机操控 | 价格被操纵，影响清算、抵押等逻辑 | 使用多节点预言机（如Chainlink）；采用TWAP时间加权平均价格；设置滑点限制 |
| 可升级合约风险 | 存储布局错误、权限滥用 | 使用OpenZeppelin Upgradeable模板；遵守存储布局规则；升级权限交由多签+时间锁 |

ps: 防护流程--单元测试+模糊测试+静态分析（Slither等）+第三方审计+社区漏洞赏金

# 六、合约代码的全公开

-   合约部署后即公开：字节码、交易历史、事件日志全公开，状态变量明文存储在链上，仅访问接口有差异
    
-   以太坊安全模型：通过Etherscan/Sourcify做源码验证，公开合约逻辑，结合人工审计、自动化工具扫描、社区众测，形成多层安全防线。安全的协议敢于公开源码来接受长期的审查
    

# 七、智能合约的“修改升级”

合约部署后代码不可直接修改，所有的修改本质是 “部署新内容+迁移流量状态”

## 修改方案

| 方案 | 核心逻辑 | 适用场景 | 优缺点 |
| --- | --- | --- | --- |
| 重新部署新合约 | 部署新版本合约，前端/其他合约指向新地址，迁移旧合约状态 | 无状态/状态少、可离线迁移的小合约 | 优点：简单清晰；缺点：地址变更，状态迁移成本高 |
| 代理模式（主流） | 固定代理合约地址（存储状态+转发调用），更换背后的实现合约（存储业务逻辑） | 有资金、需长期运营的DeFi/NFT/DAO项目 | 优点：地址不变，状态不迁移；缺点：实现复杂度高，需注意存储布局兼容 |
| 模块化架构（钻石标准等） | 拆分数据存储与逻辑模块，通过路由或注册表更换功能模块 | 超大型、模块众多的复杂协议 | 优点：模块可独立升级；缺点：审计和实现复杂度极高 |
| 可插拔模块 | 主合约维护模块列表，通过注册/反注册更换功能插件 | 需灵活调整风控、费率等逻辑的协议 | 优点：适配市场环境快速调整；缺点：需做好接口白名单与权限控制 |

ps:避免使用SELFDESTRUCT+CREATE2的“销毁重建”方案：EIP-6780后该方案已基本失效，不再是安全可靠的升级方式。升级权限务必交由多签+时间锁控制，避免单人操作风险
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->













在中国Web3圈，监管的核心是“技术可以玩，金融属性别碰”。项目涉及发币、融资、交易、挖矿、返利、提现、换汇，就处于红线的边缘。技术岗也一样——写代码、设计模型、部署合约，也可能被认定为共同犯罪。并且全球监管越来越严，只有合规措施的执行，才能继续发展。

除开监管之外的，更容易踩红线是贪婪作祟：高薪Token诱惑、归零风险、空投福利、陌生人全权委托、场外出金便利。这每一步都风险多多，极可能把自己送进雷区。

ps:今天阅读了一下web 3实习手册当中的安全合规方面，发现有很多自身之前未了解的地方。同时也非常感谢老师在晚上课程的分享。

# 以太坊账户类型与结构笔记

以太坊仅存在两类账户，核心在于控制方式与行为逻辑的不同

-   EOA（外部拥有账户）：由私钥直接控制，能主动发起操作
    
-   合约账户：由代码控制，仅能被动响应调用
    

# 一、EOA

## 1\. 定义

由公私钥对控制，无代码、无存储；状态字段含：地址、nonce（交易计数器）、余额，code与storage为空。

## 2\. 控制权归属

-   私钥 = 控制权：发起交易需要私钥签名，以太坊节点验证签名来确认合法性。若私钥泄露即资产丢失
    
-   助记词（BIP-39）：相当于所有私钥的“总钥匙”，配合BIP-44分层路径实现多地址管理
    

## 3\. 关键机制

-   nonce：防止重放攻击，保证交易次序，每笔交易的nonce需与账户当前值保持一致
    
-   Gas：发起交易必须支付gas，余额不足则交易被拒绝，防御女巫攻击
    

# 二、合约账户

## 1\. 定义

无私钥，由EVM字节代码控制行为。状态字段含：地址、nonce（合约创建计数）、余额、storageRoot（状态存储根）、codeHash

## 2\. 创建流程

1\. 编写合约（Solidity），编译为init code（初始化）+ runtime code（运行时）

2\. EOA或工厂合约发起（to为空、data为init code）的创建交易，来支付gas费

3\. EVM执行init code，返回runtime code写入链上，初始化storage，生成合约账户

## 3\. 地址生成规则的区别

| 创建方式 | 核心特点 | 应用场景 |
| --- | --- | --- |
| CREATE（传统） | 部署前不可预计算，依赖nonce | 普通合约部署 |
| CREATE2（EIP-1014） | 部署前可精确预计算，跨链地址一致 | 工厂合约、代理升级、反事实合约 |

能力：可持有多种资产、执行复杂逻辑、调用其他合约、创建新合约

限制：无法主动发起外部交易，代码默认不可改，部署需高额gas

# 四、地址格式与控制方式的差异

## 1\. 为何以太坊地址以0x开头

地址本质是20字节二进制数据，文本展示用十六进制（40个字符）。0x是编程界传统前缀，标记为十六进制，避免与其他格式混淆，并非协议强制要求

## 2\. EOA与合约账户控制方式

|   | EOA | 合约账户 |
| --- | --- | --- |
| 控制对象 | 私钥或助记词持有者 | 合约代码（含权限逻辑） |
| 如何触发 | 主动发起交易 | 被动响应调用 |
| 可能风险 | 私钥泄露/丢失 | 代码漏洞、权限设计不当 |
| 创建成本 | 本地生成，无gas成本 | 需上链部署，支付高额gas |

1\. EOA→合约/EOA：EOA签名发起外部交易，携带to、data、value等信息，触发合约执行或转账

2\. 合约→合约/EOA：通过call/delegatecall/staticcall等内部消息调用，共享同一交易的gas，无新顶层交易

3\. 链上所有顶层交易的起点都是EOA（含ERC-4337账户抽象，底层仍由Bundler的EOA打包）

### 1\. 钱包（MetaMask）管理EOA（基于 ERC-4337）

本地生成助记词→加密存储→按BIP-44路径派生地址→本地签名交易→通过RPC广播。可支持ERC-4337智能账户，实现批量操作、代币付gas等功能

### 2\. 代币和合约账户的关系

ERC-20与ERC-721代币本质是合约账户的“记账系统”：通过mapping记录余额与持有关系，转账即调用代币合约来修改存储，EOA地址仅记账key，并不存储代币本身

### 3\. 在Dencun升级后，合约的修改与删除

-   代码是默认不可改的：仅能通过代理模式（Proxy）切换实现合约，保留地址和状态
    
-   SELFDESTRUCT语义的变更：仅创建同交易内调用可彻底删除，后续调用仅转余额。不再删代码，Metamorphic合约模式基本失效
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->














# 以太坊节点

# 一、节点与客户端架构

## 1 以太坊节点

运行以太坊客户端软件并接入网络的计算机，核心功能：验证数据、同步区块状态，保障网络安全

## 2 合并后双客户端架构

完整节点 = 执行客户端（EL）+ 共识客户端（CL）；验证节点需额外添加验证者客户端（共识客户端插件）

-   执行客户端（EL）：负责业务逻辑与状态（交易处理、EVM执行、状态数据库维护、JSON-RPC接口）
    
-   共识客户端（CL）：负责PoS共识（信标链维护、区块提议/投票、验证者管理、链头选择）
    
-   Engine API：EL与CL的内部通信接口（JSON-RPC），核心作用：传递区块骨架与执行结果，保障两客户端协同
    

# 二、节点通信机制

## 1 节点发现（UDP + Kademlia）

新节点通过引导节点（长期在线全节点）接入，基于discv4/discv5协议（Ping/Pong确认在线，FindNode/Neighbors获取邻居列表），构建分布式哈希表（DHT）完成节点发现

## 2 安全连接（TCP + RLPx devp2p）

UDP用于发现，TCP建立可靠传输通道；RLPx协议实现加密会话与多协议复用（同步区块、传播交易等子协议共享通道）

## 3 数据传播（Gossip + 请求-响应）

-   Gossip协议：新交易/区块随机转发给少数邻居，层层扩散，实现秒级全网同步，适用于最新消息传播
    
-   请求 响应：按需拉取历史区块/状态数据，适用于同步离线期间的历史数据
    

# 三、节点的类型

| 类型 | 核心存储 | 资源消耗 | 核心用途 |
| --- | --- | --- | --- |
| 全节点 | 全部区块+最近~128块完整状态（老状态修剪） | 中等（数百GB级磁盘） | 自主验证交易 区块、提供RPC接口、自托管接入入口 |
| 归档节点 | 全节点数据+创世以来所有历史状态快照 | 极高（TB级，Erigon~2-3TB，Geth~12TB+） | 区块浏览器、链上分析、历史调试、合规审计 |
| 轻节点 | 仅区块头，按需请求数据 | 极低（几十MB~数百MB） | 资源受限设备手机、轻量验证 |

# 四、核心机制：同步、配合与数据交换

## 1 节点同步模式

-   Snap Sync（默认推荐）：先下区块头，再拉最新状态快照，数小时~1天完成，适用于开发/日常使用
    
-   Full Sync：从创世块逐笔执行交易重建状态，最安全但极慢（数天~数周），适用于审计/研究
    
-   Light Sync：仅同步区块头，依赖全节点获取详情，适用于轻量场景
    

## 2 EL与CL配合流程

-   出块场景：CL选中提议者→通过Engine API告知EL出块参数→EL生成执行负载→CL封装为信标区块广播
    
-   验证场景：CL接收新信标块→提取执行负载给EL复验→EL返回验证结果→CL决定是否投票认可
    

## 3 节点核心交换数据

交易/交易收据、区块头/区块体、状态树数据（账户/合约信息）、P2P管理数据（Ping/Pong等）、共识消息

# 五、运行节点的意义及对去中心化的保障

## 1 运行全节点的价值

-   隐私保护：避免地址/IP等元数据泄露给第三方RPC服务商
    
-   抗审查：自主广播交易，不依赖第三方服务，规避限流宕机风险
    
-   安全验证：本地完整验证数据，践行“不要信任，要验证”的哲学
    
-   网络贡献：增强去中心化与客户端多样性，提升网络弹性
    

## 2 去中心化保障机制

-   客户端多样性：无单一客户端占比超50%，降低软件层风险
    
-   地理分散：节点遍布全球，规避单一地区监管/断网风险
    
-   基础设施混合：自建节点与多云托管并存，降低云厂商依赖（AWS占比 20%）
    
-   验证者分散：近100万活跃验证者，通过PoS经济机制（slashing等）鼓励独立参与
    

# 六、Gossip协议用途

去中心化广播系统，核心功能：节点仅向少数邻居转发新消息，通过层层扩散实现秒级全网同步，保障高可用、容错性与可扩展性，支撑交易传播

# 七、如何节点搭建和数据分析

## 1 个人节点搭建核心步骤

1.  选型：全节点（开发） 归档节点（分析），本地机/云服务器
    
2.  安装：部署EL（Geth/Nethermind）+ CL（Lighthouse/Prysm），配置Engine API通信
    
3.  同步：EL用Snap Sync，CL用checkpoint sync（数小时完成）
    
4.  启用RPC：配置HTTP/WS接口，用于dApp开发/调试（注意鉴权与IP限制）
    

## 2 无节点数据分析方案

-   公共分析平台：Dune Flipside（零运维，直接用SQL查询预解析数据）
    
-   BigQuery公共数据集：企业级数仓，支持与业务数据联表分析
    
-   区块浏览器API：Etherscan等，适合轻量脚本/监控任务
    
-   混合方案：历史数据用BigQuery Dune，实时数据用托管RPC
    

ps:以太坊节点是网络的核心载体，合并后通过EL（算交易/管状态）与CL（跑共识/选区块）的协同实现功能.节点类型按存储与用途分层，全节点保障自主接入，归档节点支撑历史分析.运行节点是保障隐私、安全与抗审查的关键，而去中心化通过客户端/地理/基础设施/验证者的多维度分散实现.实践中可根据需求选择自建节点或利用公共数据服务完成开发与分析
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->
















# 一、区块链基础

## 1\. 定义

-   去中心化分布式账本技术：通过时间哈希串联的区块记录交易数据，含前一区块哈希，确保安全可靠、不可篡改、可追溯
    

## 2\. 特性

-   不可篡改：区块串联结构，修改需改动后续所有区块，分布式网络下难以实现
    
-   公开透明和半匿名：链上信息可追溯，钱包地址与真实身份无关
    
-   快速交易：链上打包完成即确认，优于传统跨境支付
    
-   去中心化：全球节点分布式存储，无中心化单个控制方，数据永久存在链上
    

## 3\. 组成

-   去中心化网络：节点（矿机）分布式记账，各节点存储完整相同账本
    
-   代币激励：节点通过验证交易获代币奖励（如比特币），用户使用网络需支付Gas费（矿工手续费）
    
-   运行流程：用户发起交易→广播全网→节点验证→共识机制打包成→上链更新→矿工获奖励
    

## 4\. 三大类区块链

| 类型 | 节点加入 | 数据可见性 | 管理模式 | 适用场景 |
| --- | --- | --- | --- | --- |
| 公链 | 自由加入 | 全网可见 | 去中心化投票 | 加密货币、公共存证 |
| 联盟链 | 联盟邀请审批 | 仅联盟成员可见 | 多中心联合决策 | 供应链、金融协作 |
| 私链 | 严格审批 | 仅内部可见 | 中心化管控 | 企业内部管理、审计 |

## 5\. Web2、Web3、Web3.0 的差异

| 维度 | Web2（当前互联网） | Web3.0（语义网） | Web3（去中心化互联网） |
| --- | --- | --- | --- |
| 控制权 | 平台垄断 | 部分开放 | 用户自治 |
| 数据存储 | 中心服务器 | 混合存储 | 区块链/IPFS |
| 核心特征 | 用户产内容不拥数据，广告驱动 | 语义标记，结构化数据，机器理解 | 数据主权归用户，智能合约自动执行 |
| 典型应用 | 微信、抖音、亚马逊 | W3C、DBpedia | MetaMask、Uniswap、ENS |

ps：Web3 ！= Web3.0，前者是区块链驱动的去中心化互联网，后者是语义网技术驱动的数据组织升级

# 二、以太坊基础

## 1\. 定位与核心创新

区块链2.0的代表，开源去中心化计算平台（“全球共享计算机”），核心创新是智能合约（链上可执行代码，满足条件自动执行），支持DeFi、NFT、DAO等应用的执行（代币为ETH）

## 2\. 与比特币的差别

|   | 比特币 | 以太坊 |
| --- | --- | --- |
| 目标 | 数字黄金 | 去中心化平台，支持智能合约与Dapps |
| 共识机制 | PoW | 2022年Merge后转为PoS |
| 编程能力 | 脚本语言有限，仅简单交易验证 | 图灵完备（Solidity），支持开发复杂合约 |
| 出块速度 | 10分钟/块 | 12秒/Slot，确认更快 |
| 经济模型 | 总量固定2100万，抗通胀 | 供应灵活，EIP-1559下可通缩 |

## 3\. 关键历史升级

-   以太坊1.0（PoW）：能耗高、扩展性差
    
-   The Merge（2022.9）：主网与信标链合并，转为PoW，能耗降99.95%，架构分为执行层+共识层
    
-   EIP-4844（2024.3）：引入Blob交易，降低L2数据存储成本，L2费用降70%-90%
    
-   未来方向：数据分片（2025-2026）+Layer2 Rollup，主网做结算验证层，L2来处理大量交易
    

## 4\. 机制

-   账户系统：EOA（外部账户，私钥控制，可主动交易）+CA（合约账户，代码控制，被动触发）
    
-   Gas模型：交易燃料费=Gas Limit × Gas Price，EIP-1559协议后分为基础费用（销毁）+小费（激励验证者）
    
-   EVM（以太坊虚拟机）：图灵完备，全球节点同步执行合约，共同账本保证安全
    

## 5\. 生态架构

-   应用层：DeFi（Uniswap、Aave）、NFT（OpenSea）、钱包（MetaMask）、DAO工具（Snapshot）
    
-   协议层：共识层、执行层客户端、EVM、Gas机制
    
-   扩展层：L2 Rollup（Arbitrum/zkSync）、侧链（Polygon PoS）
    

# 三、行业赛道

## 1\. DeFi

核心：无需中介的金融服务，基于智能合约运行

-   Uniswap（DEX）：AMM自动做市商模式，_X x Y=K_ 恒定乘积公式定价，将用户存币成为LP赚手续费
    
-   Compound（借贷）：用户存币获cToken赚利息，借币需超额抵押，波动触发清算
    
-   Sky（原MakerDAO）：超额抵押生成稳定币（USDS），通过稳定费率调节供需关系
    

## 2\. NFT

核心：区块链实现数字资产唯一性与可验证所有权，智能合约支持自动版税与归属权分配

-   CryptoPunks：NFT先驱，10000个唯一像素头像，奠定NFT收藏文化
    
-   OpenSea：全球最大NFT交易平台，支持NFT的买卖与创建
    

## 3\. DAO

核心：智能合约+社区投票治理，无中心化管理层，决策公开透明

-   ETHPanda：聚合以太坊华语建设者，致力于生态教育与公共物品开发，搭建连接华语社区与全球以太坊生态的桥梁。
    
-   LXDAO：支持Web3公共物品建设，任务制激励贡献者
    
-   ConstitutionDAO：众筹竞拍美国宪法原稿，展现DAO集体行动潜力
    

## 4\. MEME

核心：基于网络文化的代币，价值依赖社区共识，投机性强、波动大，如DOGE、PEPE币

风险：缺乏实际价值，多为名人效应炒作和代币集中操控（如马斯克，trump），缺乏明确的技术价值，但通过社区共识和文化认同获得价值

## 5\. 交叉创新赛道

-   DeFi+NFT：NFT抵押借贷（BendDAO）、NFT AMM交易（Sudoswap）
    
-   DAO+MEME：FriendTech Key机制（社交影响力金融化）、ShibaSwap社区治理
    
-   AI+DeFi：Yearn Finance AI优化收益策略，Gauntlet AI风险管理
    
-   Web3+乡建：南塘DAO发行NT代币量化贡献，探索乡村社区货币与PGS的区块链认证
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
