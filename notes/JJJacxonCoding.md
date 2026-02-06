---
timezone: UTC+8
---

# JJJacxon

**GitHub ID:** JJJacxonCoding

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-02-06
<!-- DAILY_CHECKIN_2026-02-06_START -->
## 学习目标

通过实战构建一个针对 Polymarket 市场和交易的链上数据索引器，理解从链上原始数据到业务语义层数据对齐的完整流程。具体目标：

-   理解链上日志数据如何与 Polymarket 应用层的市场概念对应，并设计流程定期同步链上数据。
    
-   实现一个最小可用的索引服务，能够扫描 Polygon 上 Polymarket 合约的交易数据，将市场和交易信息存储到本地数据库，并提供查询接口。
    
-   培养工程上的健壮性考虑，包括数据一致性校验、错误重试、断点续传、幂等写入等机制，确保索引器长期稳定运行。
    

* * *

## 系统架构概览

一个 Polymarket 数据索引系统大体上由**数据源**、**处理管道**、和**数据库/接口**三部分组成。

### 数据源 (Data Sources)

### 链上数据 (Primary)

主要来源是区块链节点的 RPC 接口或区块链事件订阅。我们需要从 Polygon 链上获取与 Polymarket 相关的合约事件，例如：

-   `OrderFilled`, `OrdersMatched`（来自交易撮合合约）
    
-   `ConditionPreparation`, `PositionSplit`, `PositionsMerge`, `PayoutRedemption` 等（来自 ConditionalTokens 或适配器合约）
    

这些链上事件是权威的交易和状态记录，构成最终事实依据。

### 链下数据 (Secondary)

Polymarket 提供的 **Gamma API**。这属于中心化服务，提供了市场的元数据（例如市场的 slug、标题描述、分类标签、是否负风险、状态等）和某些链上参数的缓存（如 `conditionId`、`questionId`、`clobTokenIds` 等）。虽然这些数据在链上有对应来源，但通过 API 获取可以简化解析过程，并提供丰富的语义信息（例如市场问题的文本描述、截止时间等）。Secondary 数据用于补充说明和验证，但应与 Primary 数据交叉核对，以确保一致。

* * *

## 索引流程 (Pipeline)

索引器需要持续地发现新市场、同步交易，并提供查询服务。可以将流程分解为几个阶段：

### 1\. 市场发现 (Market Discovery)

定期或在启动时，从 Gamma API 拉取事件列表和市场列表。由于 Gamma 将 Market 组织在 Event 下，我们可以选取感兴趣的 Event（例如通过 slug 明确指定）或者通过 Gamma 的 `/markets` 列表端点获取所有当前活动的市场。

对于每个 Market 数据，提取其中的关键信息：

-   `slug`：市场短标识符，用于友好地标记市场。
    
-   `conditionId`：链上条件 ID。
    
-   `questionId`：链上问题 ID（对应 UMA Oracle 的问题哈希）。
    
-   `oracle`：预言机合约地址（通常 UMA Adapter 合约地址）。
    
-   `enableOrderBook` / `status` 等：指示该市场是否开启交易，以及市场状态（未结算/已结算等）。
    
-   `clobTokenIds`：这通常是一个包含两个 tokenId 的列表，分别对应市场的 Yes 和 No 头寸。在 Gamma API 文档中，提到 Market 映射到"一对 CLOB token ids"。
    

**验证**：对于拿到的 `conditionId` 和 `clobTokenIds`，可以本地计算一遍（参考阶段一任务 B 中 MarketDecoder）以确保 Gamma 数据可靠。即使用 `conditionId` 和 collateral USDC 地址计算 yes/noTokenId，核对是否和 `clobTokenIds` 吻合。如果不符，需谨慎处理（可能是数据错误或版本差异）。

将新的市场记录插入数据库的 `markets` 表（结构见下文），或更新已有市场的信息（例如市场状态改变）。

### 2\. 历史数据同步 (Backfill/Sync)

针对数据库中已登记的市场，抓取其历史和实时交易事件。实现上可以有两种策略：

**按区块高度顺序全局扫描**：如果 Polymarket 的交易主要发生在固定的几个合约（如前述两个 Exchange 合约地址），我们可以针对这些合约地址，用 `eth_getLogs` 按区块范围批量获取日志。例如每次获取 Exchange 合约在某区块范围内的所有 `OrderFilled` 事件。这样效率较高且实现简单。由于 Polymarket 两个主要撮合合约地址已知，我们可以通过 topics 过滤指定 `OrderFilled` 事件的签名和这些地址来抓取所有成交记录。

**按市场逐个扫描**：(不太必要) 也可依据每个市场的 tokenId 过滤 Transfer 或 PositionSplit，但 Polymarket 没有单独的市场合约，所以一般不这么做。而是直接扫描交易所合约的事件即可拿到全部交易，再按 tokenId 分类归属市场。

实际工程中，会设置一个起始区块 `fromBlock`（比如 Polymarket 上线 Polygon 的开始区块或上次中断的区块），以及一个每次处理的 `toBlock` 批次范围。循环调用 `getLogs` 获取这些区块间的所有相关事件日志。需要考虑限流和响应大小：可以按比如 10000 区块为一批，逐段扫描。同时，维护一个全局的同步状态（如 `sync_state` 表或本地文件）记录当前已处理到的最新区块高度，以支持断点续传。

### 3\. 解码与处理 (Decoding & Processing)

对于获取到的 `OrderFilled` 日志列表，逐条解析（利用阶段一编写的 TradeDecoder）。解析出交易详情后，需要将其归类到对应市场：

-   日志中的 `makerAssetId` 或 `takerAssetId` 非零值，就是交易涉及的头寸 TokenId（例如 `tokenId = 0x...1234`）。通过查找数据库 `markets` 表，找到记录中 `yes_token_id` 或 `no_token_id` 等于该 TokenId 的市场。这样即可确定此交易属于哪个市场。
    
-   获取该市场的主键 `market_id`，连同交易信息一起准备写入 `trades` 表。
    
-   同时可以确定交易的具体 outcome（是 Yes 还是 No）。例如如果匹配的是 `yes_token_id`，可以在需要时标注这笔交易在买卖 "YES" 头寸；匹配 `no_token_id` 则是针对 "NO" 头寸。（在基本要求中，可以不特别标明，但在扩展应用中可能有用，比如区分买卖的到底是哪一边结果）。
    

**注意处理重复数据**：由于链上日志唯一由 `(tx_hash, log_index)` 标识，我们可以在插入数据库时设置该组合为唯一键，避免重复插入同一条链上记录。解析过程中也可以自行去重（比如用集合暂存已处理过的 tx+index）。

此外，需要处理其他可能的日志类型：

-   `PositionsSplit` / `PositionsMerge`：虽然我们的重点是交易，但这些日志可以帮助确认市场创建和销毁情况。尤其 `PositionSplit` 可以作为市场存在和初始流动的证据，可选地记录下来。
    
-   `OrdersMatched`：此事件主要将一系列 `OrderFilled` 关联起来（提供撮合订单哈希等信息），一般可用于统计撮合次数，但对成交列表本身影响不大，可暂时忽略或者简单记录。
    
-   如果考虑负风险市场的转换，还可能有 `PositionsConverted` 事件。初期索引器可以不深入解析转换事件，只要记录有交易发生即可。
    

### 4\. 存储 (Storage)

将解析好的交易数据批量写入数据库中的 `trades` 表，以及相应更新市场状态：

-   对每条交易，插入一行包含交易哈希、市场 ID、价格、数量、方向、时间戳等信息的记录。
    
-   时间戳可以通过交易所在区块的时间获得（可从区块缓存或通过 RPC 获取区块信息）。有的日志提供 `blockTimestamp`（如果 RPC 支持 `eth_getLogs` 返回），否则需要额外查询区块。
    
-   市场表可能也需要更新一些信息，比如最后交易时间、累计交易量等，可在此阶段一起完成（或由查询时计算）。
    
-   更新 `sync_state` 存储新的 `last_block`，高度至少到 `toBlock`，下次扫描从那里开始。这样即使程序中断，也能从上次中止处继续，不会漏掉或重复。
    

整个流程运行后，就建立了链上数据和本地语义数据的索引映射：每个市场知道了自己的 tokenId，预言机等信息，交易表记录了所有和这些市场相关的成交历史。这为后续提供查询和分析服务奠定了基础。

* * *

## 数据库设计 (Schema)

为了存储上述数据，我们需要设计关系型表结构来高效查询。根据任务描述，建议的表结构如下：

### `markets` 表

市场基本信息，每条记录对应 Polymarket 的一个市场（通常一个问题的 Yes/No 预测）：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| id | INTEGER PRIMARY KEY | 自增主键 |
| slug | VARCHAR | 市场短标识符 |
| condition_id | VARCHAR | 链上条件 ID |
| question_id | VARCHAR | 链上问题 ID |
| oracle | VARCHAR | 预言机合约地址 |
| collateral_token | VARCHAR | 抵押品代币地址 |
| yes_token_id | VARCHAR | YES 头寸 Token ID |
| no_token_id | VARCHAR | NO 头寸 Token ID |
| enable_neg_risk | BOOLEAN | 是否为负风险市场 |
| status | VARCHAR | 市场状态 |
| created_at | TIMESTAMP | 创建时间 |

（实际实现中，字符串长度可根据需要调整为定长或 TEXT。）

### `trades` 表

交易记录，每条记录对应链上一笔 `OrderFilled` 事件（可能是部分成交，也可能是完整撮合的一部分）：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| id | INTEGER PRIMARY KEY | 自增主键 |
| market_id | INTEGER FOREIGN KEY | 关联的市场 ID |
| tx_hash | VARCHAR | 交易哈希 |
| log_index | INTEGER | 日志索引 |
| maker | VARCHAR | 挂单方地址 |
| taker | VARCHAR | 吃单方地址 |
| side | VARCHAR | 买卖方向 (BUY/SELL) |
| outcome | VARCHAR | 结果类型 (YES/NO) |
| price | DECIMAL | 成交价格 |
| size | DECIMAL | 成交数量 |
| timestamp | TIMESTAMP | 成交时间 |

**唯一索引**：`(tx_hash, log_index)` 以确保重复插入时违反唯一性，从而实现幂等。

### `sync_state` 表

存储同步进度等信息：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| key | VARCHAR PRIMARY KEY | 状态键名 |
| last_block | INTEGER | 最后处理的区块高度 |
| updated_at | TIMESTAMP | 更新时间 |

可以将 key 设成 `'market_sync'` 和 `'trade_sync'` 分别跟踪市场和交易的同步进度，或者简单用 `'global_indexer'` 一个 key 记录整体同步进度。

* * *

## 任务拆解

### 任务 A: Market Discovery Service 实现

**需求**：编写一个脚本或服务，负责定期发现新的市场并存储市场信息。

### 实现步骤

**1\. 获取市场列表**

利用 Polymarket Gamma API 提供的接口获取市场数据。Gamma 提供了按事件获取市场和直接获取所有市场等方式。例如，可以调用 `GET /markets` 或基于给定的 Event slug 调用 `GET /events/{slug}/markets`。假设我们有事件 slug，如 `"will-there-be-another-us-government-shutdown-by-january-31"`，就获取该事件下的所有市场列表。

**2\. 解析市场数据**

对于每个市场条目，提取我们关心的信息：

-   `slug`（有的市场 Slug 可能和事件 slug 相关或带序号，如 `fed-rate-jan2024-unchanged` 之类）
    
-   `conditionId`, `questionId`, `oracleAddress`, `outcomeSlotCount` 等链上参数
    
-   `collateralToken`（一般应为 USDC 地址，可默认为 USDC，如果 API 未提供就填默认值）
    
-   `clobTokenIds` 列表（大小为 2 的数组，对应 Yes/No tokenId）。有些 API 输出可能标记为 token0/token1 或类似字段，要对照文档理解
    
-   市场描述、状态（active/inactive/resolved）、截至时间等额外信息也可获取保存
    

**3\. 校验数据**

用链上公式校验 `clobTokenIds` 是否正确：

-   根据拿到的 `conditionId`，使用我们在阶段一编写的算法重新计算 yes/noTokenId，确保与 API 给出值吻合。如果不一致，需打印警告或暂不记录该市场，避免错误数据。
    
-   确认 `collateralToken` 是预期的 USDC 地址。
    

**4\. 存储到数据库**

将市场信息插入或更新到 `markets` 表：

-   如果表中已存在相同的 `conditionId`（或 slug），则更新其信息（可能是状态变化或补充标题等）。
    
-   如不存在，则插入新记录，生成新的 id。
    
-   特别地，记录 `enable_neg_risk`（是否负风险事件，可从 Gamma 的 `event.negRisk` 字段或市场数量判断：如果同一事件有多市场则为 true）。
    
-   记录 `created_at`（可取 Gamma 提供的创建时间，或通过链上 `ConditionPreparation` 的区块时间，但前者获取方便）。
    

**5\. 定期运行**

将上述流程设置为一个周期任务，每隔一段时间（如每小时）调用，以捕获新上线的市场。或者在启动时拉取一次全量市场。也可以根据 Gamma 提供的"最新市场 ID"增量拉取。

通过 Market Discovery，我们确保索引器掌握最新的市场清单，为后续同步交易做准备。

* * *

### 任务 B: Trades Indexer 实现

**需求**：实现一个函数如 `run_indexer(from_block, to_block)`，扫描指定区块范围内 Polymarket 的交易日志，并将交易存入数据库。

### 实现要点

**1\. 获取日志**

使用 RPC 的 `eth_getLogs` 接口，构造过滤参数：

-   `address`: 设置为 Polymarket Exchange 合约地址列表（可以两个地址都填上，以同时获取 Binary 和 NegRisk 交易所的事件）。
    
-   `topics[0]`: 设置为 `OrderFilled` 事件的事件签名哈希（可以在合约 ABI 中找到，或根据已知字段计算）。这将仅返回我们需要的成交事件。
    
-   `fromBlock` 和 `toBlock`: 设置为函数参数，或者从上次同步位置读取。本次要处理的区块区间。注意不要一次跨度太大以免超时，根据需要拆分多次调用。
    

示例：

```jsx
getLogs({
  address: [exch1, exch2],
  topics: [ORDER_FILLED_TOPIC],
  fromBlock: 40000000,
  toBlock: 40010000
})

```

**2\. 解析日志列表**

对返回的每条日志，应用 TradeDecoder（阶段一任务 A）提取结构化数据：

-   解出 `tx_hash`, `log_index`, `maker`, `taker`, `makerAssetId`, `takerAssetId`, `amounts`, `price`, `tokenId`, `side` 等。
    
-   通过 `tokenId` 找到所属市场的 `market_id`（查询数据库 `markets` 表）。如未找到匹配市场，可能意味着出现了尚未记录的新市场：
    
    -   对于这种情况，可调用 Market Discovery 流程补充该市场（例如某些市场是在我们上次获取后新创建但 Gamma 未及时提供，或者我们的市场列表滞后）。这相当于动态市场发现：索引交易时发现未知 tokenId 则立即去 Gamma API 获取它的市场信息并入库。
        
    -   如果仍无法匹配，需记录异常日志以便调查。
        
-   确定 outcome 类型：若 `tokenId` 等于 `market.yes_token_id`，则 `outcome = "YES"`，若等于 `no_token_id` 则为 `"NO"`。
    
-   整理 price 和 size：price 已有，size 则取成交的头寸数量（记得换算为实际单位，一般 `size = takerAmountFilled / 1e6` 如果 `takerAssetId` 是头寸，或 `makerAmountFilled / 1e6` 如果 `makerAssetId` 是头寸，总之就是 OutcomeToken 的实际张数）。
    
-   时间戳：日志本身不含时间，需要查询该日志所在区块时间。可以在获取日志时顺便获取 `blockNumber`，然后批量查区块时间，或者利用本地缓存。也可以在事后通过交易哈希调用 `eth_getTransactionReceipt` 再取 blockNumber + 再查时间。但为了效率，推荐批量区块查询。常用方法是：维护一个简单缓存字典，遇到新 blockNumber 用 `eth_getBlockByNumber` 查询一次时间戳，存入缓存。
    

**3\. 写入数据库**

采用批量插入或逐条插入 `trades` 表：

-   插入前可以先按照 `(tx_hash, log_index)` 排序数据，确保写入顺序一致（可选）。
    
-   利用数据库的唯一键防止重复插入。如果数据库支持 UPSERT，可以直接 UPSERT，否则捕获重复错误后忽略。
    
-   每个插入包括所有解析出的字段。Decimal 类型字段注意以合适类型写（或转为字符串后由 SQL 转换）。
    
-   建议在同一事务中完成一批区块范围的插入，确保原子性。
    

**4\. 更新同步点**

当该批次 `toBlock` 的日志处理完毕，更新 `sync_state.last_block` 为 `toBlock` 或 `toBlock_processed`。这样即使程序停止，下次启动知道从哪里继续。更新时也记录当前时间。

**5\. 循环运行**

可以将上述流程放在一个循环或调度中，不断向前推进块高，直至最新区块。同步初期可能需要从 Polymarket 上线以来的早期区块开始 backfill，完成历史数据入库。之后进入实时同步模式，可每隔几秒/块查询一次新日志。

> 注意：使用 eth\_getLogs 批量获取历史数据时，要小心 RPC 提供商限频和数据量。可以加上指数退避重试，以及对 fromBlock - toBlock 区间根据返回数据量动态调整大小。如果链上交易频繁，一个区间日志很多，可以缩短区间，以避免单次返回过大数据。Polymarket 每日交易量相对适中，但遇到热点事件可能有大量成交，应考虑性能。

* * *

### 任务 C: 查询 API 服务

最后，为了提供方便的数据查询接口，我们可以构建一个简单的 REST API（使用 FastAPI、Flask 等皆可）来查询数据库内容。两个基础接口：

### `GET /markets/{slug}`

输入市场 slug，返回该市场的详细信息（对应 `markets` 表的内容）。包括 `conditionId`, `questionId`, `tokenIds`，以及市场的文本描述、状态等。

示例返回：

```json
{
  "slug": "fed-rate-jan2024-unchanged",
  "title": "美联储1月是否维持利率不变？",
  "conditionId": "0xabc...123",
  "oracle": "0xUMAAdapterV2...",
  "yesTokenId": "0xYYY...",
  "noTokenId": "0xZZZ...",
  "status": "active",
  "created_at": "2024-01-01T00:00:00Z"
}

```

如果找不到该 slug，返回 404 或空结果。

### `GET /markets/{slug}/trades?limit=100&offset=0`

分页查询该市场的历史交易记录。可以通过 join `trades` 表获取数据，或在 `trades` 表直接筛选 `market_id` 来获取。返回 JSON 列表，每项包含交易详情（时间戳、价格、数量、买卖方等）。

示例返回：

```json
[
  {
    "timestamp": "2024-01-05T12:00:00Z",
    "side": "BUY",
    "outcome": "YES",
    "price": "0.45",
    "size": "100.0",
    "maker": "0xMaker...",
    "taker": "0xTaker..."
  },
  { "..." }
]

```

列表默认按时间排序（可以按 `tx_hash` 顺序近似认为按时间，因为同区块内顺序可忽略）。可提供参数控制排序或过滤（比如按地址过滤该用户的交易）。

### 实现提示

-   使用一个轻量的 web 框架建立路由，对接数据库查询即可。对于频繁查询，可以添加简单的缓存。
    
-   注意对 slug 或输入做校验，防止 SQL 注入（如果直接拼接 SQL 的话）。
    
-   若性能要求高，可在 `trades` 表建立 `market_id`, `timestamp` 的索引，加速按市场查询排序。
    

通过这些 API，前端或用户就可以方便地按市场获取链上的交易数据，验证我们的索引结果。

* * *

## 工程实现关键点

### 断点续传

务必确保索引器可以随时中断和重启而不造成数据丢失或重复。`sync_state` 的 `last_block` 是核心。实现时，可以在启动时读取 `last_block`，从 `last_block + 1` 开始继续扫描（因为 `last_block` 可能已处理完）。在处理过程中，先不立即更新 `last_block`，待确认该批次完整写入成功后再更新，以免中途失败导致进度丢失。对于市场列表，同样可以有类似机制（记录最后同步的市场 ID 等）。但由于 Gamma API 可以直接获取全量市场，简单起见每次都全量拉取更新也未尝不可。

### 错误重试

链上 RPC 有时会超时或失败。应对每次 `getLogs` 和 `getBlock` 请求做好异常处理和重试策略。例如，封装一个带有**指数退避 (exponential backoff)** 的重试，出现网络错误或服务端错误时等待一段时间再重试，最多重试 N 次。在批量同步初期，大量请求可能触发速率限制，合理安排请求节奏和并发。

### 数据一致性

由于我们结合链上和链下数据，必须注意两者的同步。**永远以链上数据为准**：即使 Gamma API 提供的市场列表可能有延迟或错误，我们也不能漏掉链上实际发生的交易。因此，才需要在解析交易时动态发现未知市场并补录。同时，当市场结算时，Gamma API 会更新其状态为 resolved，我们也应从链上检测到 `ConditionResolution` 或通过预言机事件知道市场结束，从而更新数据库状态。这两种来源可以互相验证。如出现冲突（链上显示结算但 Gamma 未更新或相反），应倾向相信链上并进行记录。

### 幂等写入

如前所述，插入数据库时利用唯一键避免重复。对于市场数据，也可用 `conditionId` 作为唯一键，后插入的相同 `conditionId` 更新已有记录而不是新增。幂等保证即使某段区块重复处理（可能因为重启时 `last_block` 没更新好，或者手工重新跑某范围），不会产生重复交易记录。

### 性能和扩展

随着 Polymarket 市场数量和交易量增长，注意数据库索引和查询效率。例如 `trades` 表按 `market_id` 查询需要索引支撑。批量插入可以提升写入性能。可以考虑使用异步方式处理 I/O（如使用 asyncio 或多线程）来同时获取链上数据和 Gamma 数据。进一步扩展可以引入消息队列，分离出获取和处理的阶段，但在本任务范围内简单顺序流程即可。

* * *

## 进阶思考

### 链上重组 (Reorg) 处理

在罕见情况下，Polygon 区块链可能发生区块重组，使某些已确认的交易被替换或日志被回滚。如果索引器只按照区块高度推进，一旦遇到重组，可能会记录一些幽灵交易或漏掉重组后的新交易。

一个稳健的方案是在同步最新数据时**延后几个区块确认**：比如当前区块高度是 N，索引器只处理到 N-5 或 N-10，然后等待。当检测到链上出现重组（可通过比较已处理区块的哈希变化），需要删除回滚的那部分数据并重跑。因此，维护每条交易的 `blockNumber` 以及 `blockHash` 在数据库也有帮助，用于比对。当然，在 Polygon 上大规模 reorg 极少，但小的临时 reorg 是可能的，索引器应有相应设计。

### 复杂交易拆解

在负风险市场中，一个用户的下单可能触发更复杂的撮合场景。例如，一笔 Order 可能同时与多个对手单成交，或者通过 NegRiskAdapter 将一种头寸转换为多种头寸。链上体现为多条 `OrderFilled` 和 `PositionsConverted` 等事件。

在我们的模型中，我们将每个 `OrderFilled` 都记录为独立交易记录。这对于统计成交量等已足够。但如果要还原更高层的意图（比如用户一次操作的整体情况），需要结合多条日志分析，这属于更高级的解析，不在当前索引范围内。初期索引器可以忽略这种拆解需求，只聚焦于逐笔链上成交记录的简单、完整记录。

* * *

## 总结

通过完成阶段二的索引器构建，我们能够将阶段一学到的 Polymarket 链上数据解析知识应用到实际工程中，搭建起从链上数据到业务数据库的桥梁。在确保准确性、一致性的基础上，索引器可以支持丰富的应用，比如实时行情、历史数据分析、用户盈亏计算等，为 Polymarket 生态提供重要的数据基础设施。

* * *

## 参考实现示例

以下是基于本项目代码的具体示例，供学习和参考。

### 环境配置

1.  复制 `.env.example` 为 `.env` 并填入必要配置：
    

```bash
cp .env.example .env

```

1.  安装依赖：
    

```bash
pip install -r requirements.txt

```

1.  配置必要的环境变量（`.env` 文件）：
    

```
RPC_URL=https://polygon-mainnet.g.alchemy.com/v2/YOUR_API_KEY
DB_PATH=./data/indexer.db

```

### 示例数据

项目中使用的示例数据：

-   **示例交易哈希**：`0x916cad96dd5c219997638133512fd17fe7c1ce72b830157e4fd5323cf4f19946`
    
-   **示例事件 Slug**：`will-there-be-another-us-government-shutdown-by-january-31`
    

### 数据库 Schema 实现

`src/db/schema.py` 定义了数据库表结构，使用 SQLite：

```python
# 初始化数据库
conn = init_db("./data/indexer.db")

```

表结构包括：

-   `events` - 事件信息表
    
-   `markets` - 市场信息表（包含 `yes_token_id`, `no_token_id` 等）
    
-   `trades` - 交易记录表（唯一索引 `tx_hash + log_index`）
    
-   `sync_state` - 同步状态表
    

### 数据存储实现

`src/db/store.py` 提供了数据访问层函数：

```python
# 保存市场信息
upsert_market(conn, market_dict)

# 保存交易记录
insert_trades(conn, trades_list)

# 查询市场
market = fetch_market_by_slug(conn, "market-slug")

# 查询交易
trades = fetch_trades_for_market(conn, market_id, limit=100, offset=0)

```

### 索引器核心实现

`src/indexer/run.py` 中的 `run_indexer` 函数是核心入口：

```python
results = run_indexer(
    w3=w3,
    conn=conn,
    settings=settings,
    from_block=from_block,
    to_block=to_block,
    exchange_address=exchange_address,
    neg_risk_exchange=neg_risk_exchange,
    ctf_address=ctf_address,
    exchange_abi=exchange_abi,
    ctf_abi=ctf_abi,
    include_ctf=False,
    include_exchange=True,
    include_neg_risk=True,
    event_slug="will-there-be-another-us-government-shutdown-by-january-31",
)

```

### API Server 实现

`src/api/server.py` 提供了 HTTP API 服务，支持以下端点：

| 端点 | 说明 |
| --- | --- |
| GET /events/{slug} | 获取事件详情 |
| GET /events/{slug}/markets | 获取事件下的所有市场 |
| GET /markets/{slug} | 获取市场详情 |
| GET /markets/{slug}/trades | 获取市场交易记录（支持分页） |
| GET /tokens/{token_id}/trades | 按 TokenId 获取交易记录 |

查询参数：

-   `limit` - 返回条数限制（默认 100）
    
-   `cursor` - 分页偏移量
    
-   `fromBlock` / `toBlock` - 区块范围过滤
    

* * *

## 验证命令规范

> 重要提示：任务验收将严格按照以下规范进行。请确保你的实现能够通过下述所有验证命令，并产出符合规定格式的输出。不符合规范的提交将无法通过验收。

完成任务后，请使用以下统一命令进行验证。所有命令均在 `stage2/` 目录下执行。

### 前置检查

```bash
# 确保环境配置正确
cp .env.example .env
# 编辑 .env 填入有效的 RPC_URL

# 安装依赖
pip install -r requirements.txt

```

### 任务 A：Market Discovery 验证

Market Discovery 功能集成在 `run_indexer` 中，会自动从 Gamma API 获取并保存市场信息。

```bash
# 运行 demo 时会自动执行 Market Discovery
python -m src.demo \\\\
    --tx-hash 0x916cad96dd5c219997638133512fd17fe7c1ce72b830157e4fd5323cf4f19946 \\\\
    --event-slug will-there-be-another-us-government-shutdown-by-january-31 \\\\
    --reset-db

```

验证数据库中是否有市场数据：

```bash
sqlite3 ./data/demo_indexer.db "SELECT slug, condition_id, yes_token_id, no_token_id FROM markets LIMIT 5;"

```

### 任务 B：Trades Indexer 验证

```bash
# 基础用法：索引单个区块（包含示例交易的区块）
python -m src.demo \\\\
    --tx-hash 0x916cad96dd5c219997638133512fd17fe7c1ce72b830157e4fd5323cf4f19946 \\\\
    --event-slug will-there-be-another-us-government-shutdown-by-january-31 \\\\
    --output ./data/demo_output.json

# 指定区块范围索引
python -m src.demo \\\\
    --from-block 66000000 \\\\
    --to-block 66001000 \\\\
    --event-slug will-there-be-another-us-government-shutdown-by-january-31 \\\\
    --db ./data/indexer.db

# 重置数据库后重新索引
python -m src.demo \\\\
    --tx-hash 0x916cad96dd5c219997638133512fd17fe7c1ce72b830157e4fd5323cf4f19946 \\\\
    --event-slug will-there-be-another-us-government-shutdown-by-january-31 \\\\
    --reset-db \\\\
    --output ./data/demo_output.json

```

**预期输出格式**：

```json
{
  "stage2": {
    "from_block": 66000000,
    "to_block": 66000000,
    "inserted_trades": 5,
    "market_slug": "will-there-be-another-us-government-shutdown-by-january-31",
    "market_id": 1,
    "sample_trades": [
      {
        "tx_hash": "0x...",
        "log_index": 123,
        "block_number": 66000000,
        "timestamp": "2024-01-15T12:00:00",
        "side": "BUY",
        "outcome": "YES",
        "price": "0.45",
        "size": "100.0",
        "token_id": "12345..."
      }
    ],
    "db_path": "./data/demo_indexer.db"
  }
}

```

验证数据库中的交易数据：

```bash
sqlite3 ./data/demo_indexer.db "SELECT tx_hash, side, outcome, price, size FROM trades LIMIT 10;"

```

### 任务 C：API Server 验证

```bash
# 启动 API 服务器
python -m src.api.server --db ./data/demo_indexer.db --port 8000

```

在另一个终端中测试 API 端点：

```bash
# 获取事件信息
curl <http://127.0.0.1:8000/events/will-there-be-another-us-government-shutdown-by-january-31>

# 获取市场信息
curl <http://127.0.0.1:8000/markets/will-there-be-another-us-government-shutdown-by-january-31>

# 获取市场交易记录（带分页）
curl "<http://127.0.0.1:8000/markets/will-there-be-another-us-government-shutdown-by-january-31/trades?limit=10&cursor=0>"

# 按 TokenId 获取交易
curl "<http://127.0.0.1:8000/tokens/><token_id>/trades?limit=10"

```

**API 响应格式示例**：

`GET /markets/{slug}`:

```json
{
  "market_id": 1,
  "slug": "will-there-be-another-us-government-shutdown-by-january-31",
  "condition_id": "0xabc...123",
  "question_id": "0xdef...456",
  "oracle": "0x157Ce2d672854c848c9b79C49a8Cc6cc89176a49",
  "collateral_token": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
  "yes_token_id": "12345...",
  "no_token_id": "67890...",
  "status": "active"
}

```

`GET /markets/{slug}/trades`:

```json
[
  {
    "trade_id": 1,
    "market_id": 1,
    "tx_hash": "0x...",
    "log_index": 123,
    "block_number": 66000000,
    "timestamp": "2024-01-15T12:00:00",
    "maker": "0x...",
    "taker": "0x...",
    "side": "BUY",
    "outcome": "YES",
    "price": "0.45",
    "size": "100.0"
  }
]

```

### 综合验证流程

```bash
# 1. 初始化并索引数据
python -m src.demo \\\\
    --tx-hash 0x916cad96dd5c219997638133512fd17fe7c1ce72b830157e4fd5323cf4f19946 \\\\
    --event-slug will-there-be-another-us-government-shutdown-by-january-31 \\\\
    --reset-db \\\\
    --db ./data/demo_indexer.db \\\\
    --output ./data/demo_output.json

# 2. 检查输出文件
cat ./data/demo_output.json

# 3. 验证数据库内容
sqlite3 ./data/demo_indexer.db "SELECT COUNT(*) FROM markets;"
sqlite3 ./data/demo_indexer.db "SELECT COUNT(*) FROM trades;"

# 4. 启动 API 服务并测试（在后台运行）
python -m src.api.server --db ./data/demo_indexer.db --port 8000 &

# 5. 测试 API
curl <http://127.0.0.1:8000/markets/will-there-be-another-us-government-shutdown-by-january-31>
curl "<http://127.0.0.1:8000/markets/will-there-be-another-us-government-shutdown-by-january-31/trades?limit=5>"

# 6. 停止 API 服务
kill %1

```

### 验证清单（必须全部通过）

以下所有检查项均为**必须通过**的验收标准：

-   \[ \] 数据库正确初始化，包含 `events`、`markets`、`trades`、`sync_state` 表
    
-   \[ \] Market Discovery 能从 Gamma API 获取市场并存入数据库
    
-   \[ \] 市场数据包含正确的 `yes_token_id` 和 `no_token_id`
    
-   \[ \] Trades Indexer 能扫描指定区块范围的 `OrderFilled` 事件
    
-   \[ \] 交易记录正确关联到对应的市场（通过 TokenId 匹配）
    
-   \[ \] 交易记录包含正确的 `outcome`（YES/NO）
    
-   \[ \] 重复插入相同交易不会产生重复数据（幂等性）
    
-   \[ \] `sync_state` 正确记录最后处理的区块高度
    
-   \[ \] API 服务正常启动并响应请求
    
-   \[ \] `GET /markets/{slug}` 返回正确的市场信息
    
-   \[ \] `GET /markets/{slug}/trades` 返回分页的交易记录
    
-   \[ \] API 支持 `limit`、`cursor`、`fromBlock`、`toBlock` 查询参数
    

### 验收标准说明

1.  **命令格式**：验收时将使用上述规定的命令格式运行你的代码，请确保命令行参数与规范一致。
    
2.  **输出格式**：JSON 输出必须包含规定的所有字段，字段名称必须与示例完全一致（区分大小写）。
    
3.  **数据正确性**：使用示例交易哈希 `0x916cad96dd5c219997638133512fd17fe7c1ce72b830157e4fd5323cf4f19946` 和示例事件 Slug `will-there-be-another-us-government-shutdown-by-january-31` 进行验证。
    
4.  **数据库结构**：数据库表结构必须与文档中定义的 Schema 一致，包括字段名称和类型。
    
5.  **API 规范**：API 端点路径和响应格式必须与文档规定一致，评审将使用 `curl` 命令进行验证。
    
6.  **代码可运行**：提交的代码必须在配置好 `.env` 后能够直接运行，不能有额外的手动配置步骤。
<!-- DAILY_CHECKIN_2026-02-06_END -->

# 2026-02-04
<!-- DAILY_CHECKIN_2026-02-04_START -->

修改简历，投简历。
<!-- DAILY_CHECKIN_2026-02-04_END -->

# 2026-02-03
<!-- DAILY_CHECKIN_2026-02-03_START -->


学习了解DeFi、NFT、Layer2 等赛道和一些运营工具。
<!-- DAILY_CHECKIN_2026-02-03_END -->

# 2026-02-02
<!-- DAILY_CHECKIN_2026-02-02_START -->



今天关注了一些核心媒体和社群，了解了相关赛道。
<!-- DAILY_CHECKIN_2026-02-02_END -->

# 2026-02-01
<!-- DAILY_CHECKIN_2026-02-01_START -->




听了休闲黑客松 Demoday 的演示，我也想参加一次黑客松了
<!-- DAILY_CHECKIN_2026-02-01_END -->

# 2026-01-31
<!-- DAILY_CHECKIN_2026-01-31_START -->





完成兴趣小组 mini 线上活动。
<!-- DAILY_CHECKIN_2026-01-31_END -->

# 2026-01-30
<!-- DAILY_CHECKIN_2026-01-30_START -->






复习回顾相关章节，规划自己的职业发展。
<!-- DAILY_CHECKIN_2026-01-30_END -->

# 2026-01-29
<!-- DAILY_CHECKIN_2026-01-29_START -->







策化运营mini活动海报，简历投递。
<!-- DAILY_CHECKIN_2026-01-29_END -->

# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->








继续学习了解运营相关数据分析和结构化模板，可借鉴的思路。
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->









今天浅浅地学习一些技术
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->










调研个人品牌账号，寻求可借鉴运营模式。实操小组组队，开始制作海报。
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->











今天主要完成以太坊生态相关内容的学习。
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->












学习、复盘。今天第一次参加线下黑客松，大佬挺多，继续学习吧！
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->













今日分析理解了黑客松2个案例，学习操作运用figma、chatgpt 工具。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->














今天开始做休闲黑客松的筹备组申请还有准备，期待明天能有进展。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->















今天就本周的运营任务开始组队。从 0 到 1 策划、组织、复盘一场活动，形式包括但不限于 Twitter Space、兴趣小组 mini 线上活动，等等。

找到了伙伴并开始思考主题和尝试邀请嘉宾。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->
















### 学习总结：Web3 运营实操（社群与活动）

一、 Telegram 社群搭建与管理

**1\. 基础设置与架构**

-   **群组类型 (Group Type)：**
    
    -   **Private（私密）：** 仅能通过邀请加入，适合工作群或内部群。
        
    -   **Public（公开）：** 可定制链接，方便用户搜索和加入，适合开放型社群。
        
-   **权限管理 (Permissions)：** 需根据社群需求设置权限，例如限制成员修改群信息或添加频道的权限，防止恶意操作。
    
-   **话题功能 (Topics)：** 类似于文件夹分类，适合内容较多的社群；但话题不宜过多以免造成信息过载，不活跃的话题建议关闭。
    

**2\. 数据与运营分析 (Statistics)**

-   **核心指标：** 关注 Members（成员数）、Messages（消息数）以及 Growth（增长曲线），通过增长曲线可以验证特定活动（如实习计划宣发）的引流效果。
    
-   **用户画像：** 关注 Language（语言分布），若发现外语用户增长（如英文用户），需考虑招募对应语种的管理员。
    
-   **活跃时段 (Top Hours)：** 观察群内发言的高峰时间段，选择在这些时段发布重要内容以提高触达率。
    

**3\. 自动化管理工具 (Rose Bot)**

-   **安全警示：** 必须认准官方账号，防止拉入假冒机器人导致钓鱼链接风险。
    
-   **设置流程：** 先私聊 Bot `/start` -> 拉入群组 -> 赋予管理员权限 -> 使用命令配置。
    
-   **常用功能：**
    
    -   **Blocklist (黑名单)：** 可设置敏感词（如“赚钱”等），触发即自动屏蔽或处理。
        
    -   通过 `/help` 命令可以查看支持的所有指令。
        

* * *

二、 Twitter Space 活动全流程策划 (SOP)

**1\. 准备阶段 (Preparation)**

-   **定主题与嘉宾：**
    
    -   确定活动主题（如 ERC-404、技术标准等）和时间。
        
    -   准备问题库（Question Bank），用于评估嘉宾是否合适以及后续控场。
        
-   **邀请话术：** 必须包含主题、时间、拟涉及的话题，让嘉宾在不知情的情况下也能快速判断。
    
-   **物料制作：**
    
    -   **海报四要素：** 嘉宾名字、Twitter Handle（用户名）、Title（头衔/组织）、头像。
        
    -   **文案：** 需包含主题、时间、嘉宾、Space 链接，并务必 @嘉宾 Handle 以增加互动。
        
    -   **确认机制：** 所有对外宣发物料（海报、文案）发出前，最好发给嘉宾确认无误。
        
-   **宣发节奏：**
    
    -   **预热：** 提前 2-3 天创建链接并全渠道分发，让听众预留时间。
        
    -   **提醒：** 活动当天再次预热，开始前 5 分钟提醒上线。
        

**2\. 执行阶段 (Execution)**

-   **开场流程：**
    
    -   主办方/品牌介绍（露露品牌）。
        
    -   嘉宾自我介绍。
        
    -   背景介绍（为什么做这个主题）。
        
-   **控场：** 按照问题库进行问答，若嘉宾提前回答了后续问题，主持人需灵活跳过或调整。
    

**3\. 复盘阶段 (Review)**

-   **关键数据：**
    
    -   **实时在线人数 (Real-time listeners)：** 最重要的指标，反映活动的真实火爆程度。
        
    -   **总收听 (Total Tune-ins)：** 该数据容易包含重复进入或虚假数据，参考价值次之。
        
-   **识别假数据：** 听众列表中，头像被折叠（Folded）的用户通常是买粉或机器人，展示出来的头像是真实用户。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->




















作为一名从 Web2 转型过来的运营，虽然我已经熟悉了钱包、私钥、Gas 费这些基础概念，甚至能熟练教用户怎么防骗，但很长一段时间里，我对“协议标准（Protocol Standard）”的理解还停留在表面——知道 ERC-721 是 NFT，ERC-20 是代币。

今天的分享会对我来说是一次很好的“祛魅”与“深潜”**。Alex 讲的 ERC-7962 让我意识到，那些我平时挂在嘴边的痛点（如隐私、门槛），是如何在**代码底层（Infrastructure）被重新架构和解决的。

这不再是简单的概念科普，而是让我真正理解了 Web3 产品的“底层商业逻辑”。

* * *

### **一、 技术认知的颗粒度升级：不仅是“无 Gas”，更是“所有权解耦”**

以前看到“账户抽象”或者“代付 Gas”，我只知道这是功能。今天通过 ERC-7962 的技术拆解，我理解了它背后的实现路径：**所有权与交易发起者的解耦** 。

**1\. 对“地址（Address）”的重新思考**

我之前一直默认“地址=账户=身份”。但 ERC-7962 提出用 **KeyHash（公钥哈希）** 来作为资产标识 。

-   **深层理解：** 这意味着链上不再记录“谁拥有这个资产”，而是记录“哪把钥匙（Key）能控制这个资产”。
    
-   **运营启示：** 这让我对“隐私”有了更底层的理解。我们在向高净值用户（Whale）推介时，不能只说“安全”，而要能解释清楚：**“协议层面切断了地址追踪的可能性”** 。这种基于技术原理的解释，比苍白的营销话术更有说服力。
    

**2\. 签名（Signature）的威力**

之前我知道签名是用来授权的，但这次分享提到了 **EIP-712 结构化签名**、**Nonce（防重放）** 和 **Deadline（有效期）**。

-   **深层理解：** 这让我明白了为什么“免 Gas”是安全的。用户签的是一个“不仅包含了意图，还限制了时间和次数”的指令。
    
-   **运营启示：** 理解了这个，我在设计运营活动（比如限时领取 NFT）时，就能跟开发讨论更复杂的玩法——比如让用户的签名只在活动当天的 1 小时内有效，过期作废。这不仅仅是技术，这是运营风控的一部分。
    

* * *

### **二、 痛点即机会：协议特性如何转化为增长策略**

作为运营，我们每天都在处理用户流失和体验摩擦。ERC-7962 的技术特性，在我眼里全是\*\*增长策略（Growth Strategy）\*\*的弹药。

**1\. 彻底消灭 Onboarding 的“第一公里”障碍**

**技术特性：** 支持 Relayer/Paymaster 代付 Gas 9，用户只需签名。

**运营思考：** 以前我们做转化漏斗（Funnel），在“充值 ETH”这一步会流失 80% 的 Web2 用户。现在协议层支持了，我们可以把 Web3 产品做成 Web2 的体验——**“你只管点（签名），费我来付”**。这让大规模获取圈外用户（Mass Adoption）成为可能。

**2\. 重新定义“空投”与“分发”效率**

**技术特性：** 天然支持批量交易（Batch Transfer）。

**运营思考：** 以前做社区激励，发几百个地址简直是噩梦，Gas 贵且操作繁琐。如果利用这个特性，企业级的批量分发将变得极低成本。这意味着我们可以更高频地进行“微激励”，激活社区活跃度。

**3\. 诚实面对技术的“代价”**

**技术特性：** Gas 成本比传统 ERC-20/721 高，且不向后兼容 。

**运营思考：** 这次深入学习让我明白了“没有免费的午餐”。作为运营，由于我知道了成本更高，我就需要在设计商业模式时，算清楚 ROI（投资回报率）——**为了隐私和体验多付出的 Gas，能否通过更高的用户留存赚回来？** 这才是成熟运营该有的计算。

* * *

### **三、 思考：运营为什么要懂“基建”？**

今天的会让我更加确信一点：**Web3 的运营不能只做“传声筒”，要做“产品架构师”的伙伴。**

1.  从“知道是什么”到“判断能做什么”：
    
    以前我只能被动接受开发给的功能。现在了解了 ERC-7962 这种底层协议，我甚至可以反向向产品经理提需求：“既然协议支持 Key 轮换，我们能不能做一个‘阅后即焚’的隐私钱包功能？”
    
2.  建立技术共情（Technical Empathy）：
    
    懂一点公钥、私钥和哈希的原理，让我在看 EIP 提案时不再畏惧。我知道了 Account Abstraction（账户抽象）并不是一个虚词，而是由一个个像 ERC-7962 这样的协议堆出来的未来。
    

总结：

这次分享会对我来说，是一次从“应用层”下潜到“协议层”的探索。Web3 的魅力在于技术与金融的强绑定，只有理解了底层的砖石（协议），才能描绘出更宏伟的大厦（生态）。

下一步计划：深入研究一下 PPT 里提到的 **EIP-712** 签名标准，看看它在具体的 DApp 前端交互里长什么样，彻底打通从代码到用户体验的最后一环。
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->





















学习来源： Web3 实习手册 & ZetaChain 社区负责人 Julie 访谈

学习目标： 理解 Web3 社区运营的核心逻辑、掌握实操工具与流程、确立职业成长路径。

## 一、 核心认知：什么是 Web3 社区运营？

通过学习，我认识到 Web3 社区运营不仅仅是“管群”，它是**连接用户与项目的重要桥梁**。

1.  **运营的本质是“共识”**：
    
    -   正如 Julie 所言，在去中心化世界里，**共识就是生产力**。社区的热度、信任度和活跃度直接决定了项目的成败。
        
    -   运营工作的核心目标是让用户相信项目、觉得好玩、并愿意参与其中。
        
2.  **不仅是品牌，更是增长 (ROI)**：
    
    -   区别于 VC 的品牌营销（重资产、重形象），项目方的运营更看重 **ROI（投入产出比）**。
        
    -   每一分投入都需要拉动真实的用户增长、留存或开发者转化。
        

## 二、 关键技能树与工具箱

根据手册指引，我梳理了作为初学者需要掌握的硬技能和工具：

### 1\. 四大核心职责

-   **日常维护**：更新社媒（Twitter/公众号），管理 Discord/TG 秩序，设计话题引导讨论。
    
-   **内容发布**：发布活动预热、AMA 公告，撰写符合品牌调性的内容并分发。
    
-   **活动策划**：组织线上（Twitter Space、黑客松）和线下（Meetup、Workshop）活动。
    
-   **对外合作**：与其他项目联动（联合 AMA）、KOL 合作、媒体宣发。
    

### 2\. 必备工具栈

-   **社交媒体**：Twitter (主阵地)、Medium/Substack (长文)、微信公众号 (中文区)。
    
-   **社群沟通**：**Discord** (功能复杂，适合核心运营)、**Telegram** (轻量级，适合 AMA/客服)。
    
-   **内容创作**：Notion (文档/协作)、Figma/Canva (海报设计)、ChatGPT (文案辅助)。
    
-   **数据分析**：Etherscan (链上数据)、Dune (数据可视化)、Galxe/Zealy (任务平台)。
    

## 三、 实战方法论：从 0 到 1 的运营策略

结合 Julie 的实战经验和手册的执行模板，我总结了以下落地方法：

### 1\. 社区冷启动策略

-   **任务可感知化**：不要只做简单的转发抽奖。围绕**链上真实操作**设计任务（如 Galxe 任务），让用户真正体验产品。
    
-   **内容做减法**：用生活化的类比拆解晦涩的 Web3 概念（例如 ZetaChain 的“一次部署，随处可用”），降低准入门槛。
    
-   **大使计划**：建立清晰的贡献通道与激励机制，让核心用户成为社区的建设者。
    
-   **高频互动**：通过表情包大赛、连麦等方式增强粘性，把社区变成“唠家常”的地方。
    

### 2\. 活动策划执行 (SOP)

-   **前期筹备 (T-4周)**：
    
    -   **选题**：结合“叙事+热点”（如结合以太坊升级讨论赛道红利）。
        
    -   **嘉宾**：不仅要看 Title，更要看是否愿意配合宣发。
        
    -   **物料**：海报、文案、倒计时图需提前备好。
        
-   **执行阶段**：
    
    -   **线上**：注重 Space 的流程控制（开场、问答、观众互动），设置奖励提高留存。
        
    -   **线下**：细节决定成败，从签到体验到现场应急（断电、网络）都要有预案。
        
-   **复盘阶段 (关键)**：
    
    -   活动结束后不是结束。必须整理 Transcript（文字回顾）、发布数据总结贴，形成**二次传播**。
        

## 四、 新人成长路径建议

作为 Web3 运营初学者，我从 Julie 的成长经历中获得了以下职业建议：

1.  **拥抱 "Dirty Work" (脏活累活)**：
    
    -   不要眼高手低。翻译、会议记录、排版、写研报等基础工作是积累行业认知和能力的**加速器**。
        
    -   Julie 正是通过大量翻译和主持 50+ 场 AMA，才积累了深厚的行业资源。
        
2.  **建立个人“长板”**：
    
    -   找到自己最擅长的一点（如双语主持、研报撰写、活动策划）并做到极致。
        
3.  **持续输出与“被看见”**：
    
    -   运营自己的 Twitter，写观察笔记或经验总结。
        
    -   **主动连接**：多给项目方提反馈、做贡献。在 Web3，机会属于主动出现的人。
        
4.  **保持好奇心**：
    
    -   Web3 变化极快，唯一的门槛是**学习与输出的持续性**，而不是背景或资源。
        

* * *

总结心得：

Web3 运营是一项这就要求“手艺活”的工作。它既需要宏观的叙事理解能力，也需要微观的执行细节把控（如海报设计、活动控场）。作为新人，我将从最基础的“脏活”做起，熟练掌握工具，通过持续的输出建立自己的行业认知，努力成为一名既懂增长又懂社区的 Web3 运营者。
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->






















## NFT 的本质是数据指针，不是 JPEG

以前看大家炒作 NFT，总觉得是泡沫。今天拆解了合约代码才明白，它在技术上其实是一种**数字所有权的确权机制**。

1\. ERC-20 与 ERC-721 的数据结构差异

这一点从代码层面看非常清晰：

-   **ERC-20 (Token)**：本质是一个 `Mapping(address => uint256)`。账本只记录“谁有多少钱”。A 转给 B，就是 A 的余额减，B 的余额加，同质化且可分割。
    
-   **ERC-721 (NFT)**：核心是 `Mapping(uint256 => address)`。账本记录的是“这个 ID 归谁”。因为 ID 是唯一的（Unique Key），所以不可分割，具有非同质化属性。
    
-   **笔记**：所谓的“购买 NFT”，在链上操作仅仅是将那个 Token ID 映射的 Address 修改为我的地址。
    

2\. 存储逻辑：链上 vs 链下

这是一个很大的误区纠正。我原以为图片都在区块链上，其实大部分 NFT 的合约里只存了一个 URI 字符串。

-   **脆弱的 Web2 链接**：如果 URI 指向中心化服务器，服务器挂了，NFT 就变成了空壳。
    
-   **IPFS (Content Addressing)**：利用内容的哈希值寻址。只要内容变了，哈希就变，这保证了元数据的**不可篡改性**。这才是 Web3 该有的样子。
    
-   **On-Chain SVG**：把 SVG 代码直接写进合约。虽然 Gas 费贵到离谱，但这种数据的“长存性”最符合极客审美。
    

3\. 实用技巧

学会了绕过前端网页（UI），直接在 Etherscan 的 Read/Write Contract 页面调用函数。如果以后热门项目前端崩溃，直接操作合约层才是降维打击。

* * *

## 解构交易 (Transaction) 与 Gas 机制

今天解决了我一直以来的困惑：为什么交易会卡住（Pending）？以及 MetaMask 到底在背后做了什么。

1\. 内存池 (Mempool) 与竞价模型

区块链本质上是一个空间有限的区块数据库。

Gas 费不仅是手续费，更是一场区块空间的拍卖。

-   `Base Fee`：协议销毁的底价。
    
-   `Priority Fee`：给矿工的小费。
    
-   **理解**：交易卡住，通常是因为我的出价在当前的拍卖中没有竞争力，被矿工暂时搁置在内存池里了。
    

2\. Nonce 的严格序列

这很像数据库的事务序列号。

-   每个账户的 Nonce 从 0 开始递增 (0, 1, 2...)。
    
-   **死锁原因**：以太坊要求必须按顺序打包。如果 Nonce=5 的交易因为 Gas 给低了没确认，Nonce=6 的交易给再多 Gas 也会被阻塞。
    
-   **解决方案**：发送一笔**相同 Nonce** 但 **更高 Gas** 的交易覆盖掉前一笔（Replacment Transaction）。以前觉得这是魔法，现在看懂了原理其实就是数据库的更新操作。
    

3\. 跨链桥的“原子性”错觉

L1 到 L2 的跨链不是瞬间完成的。资产在 L1 锁定后，L2 需要时间确认并铸造。中间的等待期是状态同步的物理延迟，不是钱丢了。作为技术人员，对这种延迟应该有预期。

* * *

## Ethers.js 脚本 —— 程序员的舒适区

终于开始写代码了。相比于虚无缥缈的金融概念，JavaScript 和 Node.js 让我感觉踏实多了。

**1\. 基础设施搭建**

-   使用了 `.env` 管理环境变量。
    
-   **安全红线**：私钥（Private Key）绝对不能硬编码在脚本里，更不能上传 GitHub。这是常识，但 Web3 对此容错率为零。
    

2\. Provider vs Signer

这两个对象的职责划分很符合 OOP 思想：

-   **Provider (只读)**：连接节点的接口，负责读取数据（查余额、查区块高度）。类似于数据库的 Read Replica。
    
-   **Signer (读写)**：持有私钥，负责对交易进行签名。
    
-   **感悟**：以前用钱包插件时没感觉，现在写脚本才意识到，查询数据是不需要花钱（Gas）也不需要签名的，只有改变链上状态才需要 Signer。
    

3\. 大数处理 (BigNumber)

JavaScript 的 Number 类型是双精度浮点数，处理不了以太坊 10^18 这种精度。

-   必须使用 `ethers.utils.parseEther()` 和 BigNumber 的方法（`.add`, `.mul`）。
    
-   **笔记**：千万不能用普通的 `+ - * /`，否则会因为精度丢失导致金额计算错误。这在金融系统里是致命 Bug。
    

4\. 异步思维 (Async/Await)

区块链是极致的异步系统。

-   `tx = await signer.sendTransaction(...)`：这只是把交易广播出去了（拿到 Hash）。
    
-   `await tx.wait()`：这才是交易被矿工打包确认（Mined）。
    
-   写脚本时必须处理好这两个阶段的等待，否则脚本跑完了，链上其实还没确认。
    

* * *

本周总结

这周的学习让我祛魅了。Web3 并不是什么神秘的黑科技，它是由一个个具体的协议、数据结构和加密算法堆叠起来的。虽然用户体验（UX）目前还很粗糙（比如复杂的 Nonce 管理和 Gas 估算），但这也正是开发者优化的机会。

**TODO**：

1.  熟练掌握 Ethers.js 的 Event 监听。
    
2.  下周准备啃 Solidity 智能合约开发了。
    

* * *
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->























### **Web3 学习笔记：从钱包、身份到数字所有权**

**第一部分：Web3 的入口——钱包与密钥管理**

进入 Web3 的第一步是建立安全意识。**MetaMask** 是目前最主流的工具，其设计原则是**安全性优先于用户体验**。

-   **助记词 (Seed Phrase) 是最高主权：** 助记词通常由 2048 个单词中随机抽取，从数学概率上来说，想要暴力破解几乎是不可能的。
    
-   **确定性生成 (Deterministic)：** 一个助记词可以派生出无限个账户地址，且顺序永远一致。这意味着如果你丢失了某个特定账户，只要助记词还在，通过“创建新账号”就能按顺序找回它们。
    
-   **资产托管的本质：**
    
    -   **非托管 (Non-custodial)：** 如 MetaMask，你拥有私钥，拥有绝对控制权，但也需承担全部安全责任。
        
    -   **托管 (Custodial)：** 如中心化交易所（CEX），本质上是你把资产存入了他们的数据库，由他们持有私钥。
        

> **【我的思考与疑问】**
> 
> -   **思考：** 助记词的“单向哈希”特性（助记词 → 私钥 → 地址）决定了 Web3 是一个“责任自负”的世界。在 Web2 中，我们习惯了“忘记密码”找回功能，但在 Web3，私钥丢失意味着资产永久锁死。
>     
> -   **疑问：** 如果助记词如此重要，对于普通用户来说，存放在 1Password 等密码管理器中是否真的比离线抄写更安全？毕竟网络环境依然存在被攻击的风险。
>     

* * *

### **第二部分：Web3 身份与交互逻辑 (ENS & DEX)**

在 Web3 中，身份不再是由某个平台定义的账号，而是由地址和关联的链上记录构成的。

-   **ENS (以太坊域名服务)：** 它是 Web3 的“名片”，将复杂的地址映射为人类可读的名称（如 `name.eth`）。
    
    -   **防抢注机制：** 采用\*\*“提交-揭示”（Commit-Reveal）\*\*两步走战略。用户先发送加密承诺，一分钟后再揭示。这种博弈论设计能有效防止机器人监控交易池（Mempool）进行恶意抢注。
        
-   **去中心化交易所 (DEX) 与 AMM：**
    
    -   DEX（如 Uniswap）就像一个**不可停止的自动售货机**，通过智能合约内的代币对（天平模型）自动调节价格。
        
    -   **流动性提供者 (LP)：** 任何人都可以向池中提供资金并赚取交易手续费。
        
-   **稳定币机制 (以 DAI 为例)：** 为了应对波动，DAI 采用**超额抵押**模式，由智能合约通过铸造、销毁和清算逻辑将其价格锚定在 1 美元。
    

> **【我的思考与疑问】**
> 
> -   **思考：** 笔记中提到 ENS 实际上是早期的 NFT。这让我意识到 NFT 不仅仅是艺术品，它更是一种具备功能性的唯一凭证。
>     
> -   **疑问：** 既然 Web3 身份可以轻易伪造（女巫攻击），目前项目方通过“行为评分”来识别真人。但这种基于链上活跃度的评分是否会对不频繁操作的小额用户造成歧视？
>     

* * *

### **第三部分：NFT 与数字所有权的深层逻辑**

NFT（非同质化代币）代表的是一种**独特的、可验证的数字所有权**。

-   **ERC-20 vs. ERC-721：**
    
    -   **ERC-20：** 记录余额（Balance），代币之间是同质的。
        
    -   **ERC-721：** 记录每一个唯一 Token ID 的持有者（Owner）。
        
-   **存储机制：**
    
    -   **IPFS (星际文件系统)：** 基于内容的哈希寻址，确保元数据（Metadata）不可篡改。但如果无人“固定”（Pin）数据，内容仍有丢失风险。
        
    -   **全链上存储 (On-Chain SVG)：** 直接将图像代码写进合约，具有极强的**可组合性**，但存储成本昂贵。
        
-   **Web3 的赋能：** 创作者可以通过代码将**版税规则**直接写入 NFT，从而在作品的二次销售中自动获得收益，不再依赖中介机构的信誉。
    

> **【我的思考与疑问】**
> 
> -   **思考：** Web3 的开放性体现在资产清单（Inventory）的透明性上。无论我使用哪个平台，我的资产和身份都会随我流动。这打破了 Web2 的“数据孤岛”。
>     
> -   **疑问：** 笔记提到某些 Web3 应用存在“Web2 式审查”（如 Foundation 的内容筛选）。那么，如果前端网站屏蔽了我的 NFT，即使它在链上依然存在，对于普通大众来说，它是否还具有同样的价值和流动性？
>     

* * *

### **我的总结**

成为 Web3 权力用户的秘诀在于**实践**：从小额操作开始，熟悉测试网（如 Goerli）、学会使用 Etherscan 查询交易状态，并逐步建立对私钥管理的信心。在追求创新的同时，务必警惕不透明合约所带来的“Red Flag”。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->
























## 安全分享

### 1\. 2025年两大攻击趋势

-   **趋势一：精准猎杀 (Targeted Hunting)**
    
    -   _目标_：头部交易所、大户、特定协议。
        
    -   _特点_：长期潜伏、精心策划（如 Bybit 案）。
        
-   **趋势二：广撒网 (Broad Casting)**
    
    -   _目标_：普通散户。
        
    -   _特点_：自动化、规模化（Rug Pull 工厂、批量钓鱼）。
        

### 2\. 经典案例复盘

-   **Bybit (15亿美金损失案)**
    
    -   _手段_：**供应链攻击**。没攻破交易所本身，而是渗透了多签服务商 (Safe) 的**前端开发人员**。
        
    -   _手法_：植入恶意代码 -> 界面显示正常 -> 后台签名被篡改。
        
-   **UxLink (Deepfake 诈骗)**
    
    -   _手段_：**高端社会工程学**。
        
    -   _流程_：Telegram 冒充资方 -> **AI换脸/变声**视频会议取得信任 -> 会议中发虚假软件链接 -> 植入木马控制设备。
        

### 3\. 新型攻击手法解析

| 攻击名称 | 核心原理 | 关键风险点 |
| --- | --- | --- |
| 地址投毒 (Address Poisoning) | 生成首尾字符相同的黑客地址，空投小额骚扰。 | 利用用户只核对首尾的懒惰习惯，诱导复制错地址。 |
| EIP-7702 钓鱼 | 利用新升级的“委托 (Delegate)”功能。 | 1. 签署恶意Delegate -> 资金秒转。 |

2.  **诱饵陷阱**：故意泄露已签署恶意的私钥，谁充Gas费提现谁被宰。 | | **软件供应链攻击** (AI版) | 在代码库/插件植入后门。 | **AI Coding 隐患**：开发者过度依赖AI写代码，AI可能自动引入有毒的依赖库。 |
    

### 💡 防御口诀：小白“四不大法”

1.  **不点**：不明链接（TG/Zoom/微信）一律不点。
    
2.  **不签**：看不懂的签名请求（盲签）直接拒绝。
    
3.  **不装**：非官方渠道软件不安装。
    
4.  **不转**：转账地址核对全程，不信高息代投。
    

* * *

## 法律与合规

### 1\. 刑事风险红线

-   **发币 = 犯罪？**
    
    -   单纯发币（如Meme）不一定，关键看有没有**公开融资 (ICO)**。融资 -> 非法集资风险。
        
-   **永续合约 = 开设赌场？**
    
    -   _高危原因_：买涨买跌 + 无实物交割 + 庄家控盘 $\\approx$ **押大小**。
        
    -   _后果_：交易所被定性为赌场，员工被定性为开设赌场人员。
        
-   **程序员“技术中立”？**
    
    -   _结论_：**行不通**。为赌博/诈骗软件提供支持，通常被认定为“共犯”。
        

### 2\. 灰色地带与生存操作

-   **挖矿还活着吗？**
    
    -   _转型思路_：包装成**AI超算中心**，拿政府补贴；算力以外贸形式卖给海外（名义上支持AI）。
        
-   **远程工作 (Remote) 风险自测**
    
    -   _牌照_：美国 MSB 门槛低，含金量不足以挡灾。
        
    -   _隔离_：**必须**屏蔽大陆IP + 禁止+86注册（否则视为在大陆展业）。
        
    -   _岗位_：合约、期权开发 -> 高危；普通UI/前端 ->相对低。
        

### 3\. 资金流转陷阱

-   **出金风险：反向洗钱 (路径攻击)**
    
    -   _现象_：坏人把诈骗来的“黑钱”打入你的正常账户。
        
    -   _后果_：你的卡因为收了赃款被冻结/立案。
        
-   **U卡 (Crypto Debit Card) 的坑**
    
    -   _挂靠风险_：你被挂靠在某空壳公司名下，公司被查 ->资金连坐。
        
    -   _资金池风险_：钱其实进了私人多签钱包 -> 老板跑路/闹翻 -> 钱没了。
        
-   **发薪风险**
    
    -   用 USDT 发工资、员工离职仲裁、法律不认U、公司被判**用人民币重发一遍**。
        

### 4\. 遇到“黑天鹅”怎么办？

-   **配侦公司 (Web3 赏金猎人)**
    
    -   _身份_：技术公司 + 公安合作。
        
    -   _手段_：链上分析 + **卧底入职** ——> 取证举报拿返点。
        
-   **冻卡应对**
    
    -   _初次_：如实说明，提供交易记录，做笔录（通常问题不大）。
        
    -   _多次_：易被推定为“明知故犯”。
        
    -   _最坏情况_：**退赔解冻**（把涉案赃款退回去才能解封）。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


























# 1\. 理论学习

**1.1 起源与定位**

以太坊（Ethereum）不仅仅是加密货币，更是一个去中心化的全球计算平台（World Computer）\*\*。它通过智能合约（Smart Contracts）将区块链从单纯的记账工具（如 Bitcoin）进化为可编程的通用平台。其核心特性包括承载去中心化应用（DApp）、各类代币标准（ERC-20/721）以及作为燃料的 Gas 机制。

**1.2 Dencun 升级与 EIP-4844**

在 Dencun 升级中，核心改进是引入了 **EIP-4844（Proto-Danksharding）**。

-   **形象比喻：** 此前的 Rollup 数据像乘客一样挤在以太坊主网的“昂贵车厢”（Call Data）里；升级后，引入了 **Blob（二进制大对象）**，就像在主车后挂了一个“临时小挂车”。
    
-   **技术细节：** Blob 数据不直接进入以太坊执行层（EVM），而是作为一种临时存储（约 18 天后自动丢弃），极大地减轻了 L1 的长期存储压力。
    
-   **效果：** 这使得 L2（如 Optimism, Arbitrum）向 L1 发布数据的成本降低了 **90% 以上**，是实现以太坊分片扩容的关键一步。
    

**1.3 ETH 的双重价值属性**

**协议层（原生价值）：**

-   **Gas 与销毁：** 交易费分为 BaseFee 和 Priority Fee。根据 **EIP-1559**，BaseFee 会被直接销毁，从而在网络活跃时赋予 ETH 通缩属性。
    
-   **PoS 共识安全：** 验证者（Validator）需质押 32 ETH。这种经济门槛提高了攻击成本，恶意行为（如双签）会导致质押金被\*\*罚没（Slash）\*\*，从而保障网络安全。
    

**应用层（经济价值）：**

-   ETH 是链上的“现金”和结算单位，也是 DeFi 借贷协议中极其重要的\*\*底层抵押品（Collateral）\*\*，同时是 NFT 市场的计价货币。
    

# 2\. 实践进展

-   **NFT 铸造：** 成功完成首个 NFT 的 Mint 操作，体验了从元数据上传到链上交互的全过程。
    
-   **开发环境搭建：** 初步接触以太坊开发工具链。计划安装并学习 **Node.js** 环境，以及智能合约开发框架 **Hardhat** 或 **Foundry**，掌握编译、部署及测试的基本命令。
    

# 3\. 分享会核心观点：交易与共识

**3.1 一笔交易的生命周期**

用户在 Wallet 签名后，交易并非直接写入区块，而是经历以下流转：

1\. **RPC 传播：** 交易发送至节点。

2\. **Mempool（内存池）：** 交易进入待处理队列“排队”。

3\. **Builder/Validator 挑选：** 验证者根据 Gas 费高低从池中捞取交易打包。

4\. **Block 落盘与广播：** 新区块生成并广播给全网。

5\. **最终性（Finality）：** 经过两个 Epoch（约 12.8 分钟）后，交易不可逆转。

**3.2 常见误区与验证机制**

-   **Gas 费越多，出块越快？纠正：** 以太坊的出块时间是固定的（每 Slot 12 秒）。提高 Gas（主要是 Priority Fee/小费）只能让你插队，增加被当前区块选中的概率，而不能改变区块产生的物理速度
    
-   **去中心化隐忧：**
    
    -   **地理分布：** 验证节点高度集中于美国和德国（占比超 40%），存在地缘政治监管风险。
        
    -   **客户端多样性：** 呼吁 Validator 使用非主流客户端（如 Nethermind、Besu），以防止主流客户端（如 Geth）出现致命 Bug 时导致全网瘫痪。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



























## 1\. 区块链的本质：一种“信任机器”

### 定义：

区块链是一种由多方共同维护、使用密码学保证传输和存储安全、能够实现数据一致性与防篡改的分布式共享账本。

### 核心特性深度解读：

-   **不可篡改性（Immutability）：** 核心在于**哈希算法**。每个区块都包含上一个区块的哈希值，形成了严密的逻辑链条。一旦修改历史数据，会导致后续所有区块的哈希值失效，修改成本极高（需掌握全网51%以上的算力/权益）。
    
-   **透明性与隐私的平衡：** 账本对所有人可见（透明），但账户身份通过密码学公钥表示（伪匿名）。这种设计实现了“交易可追踪，身份难穿透”。
    
-   **抗单点故障：** 数据分布在成千上万个节点上，任何一个节点的损坏或退出都不会影响整个系统的运行。
    

## 2\. BTC：区块链的首个成功应用

比特币是区块链技术的“创世级”应用，它解决了在没有中介的情况下如何防止“双重支付”（Double Spending）的问题。

-   **共识机制：** 通过 **PoW（工作量证明）**，让矿工竞争计算权，确保账本的唯一性和安全性。
    
-   **经济模型：** 核心在于**通缩模型**（总量2100万枚）和**产出减半机制**，赋予了它“数字黄金”的价值属性。
    
-   **核心价值：** 它是人类历史上第一次实现了个人资产的“主权化”——只要拥有私钥，资产便无法被冻结或强行转移。
    

## 3\. 区块链的运转中枢

经济驱动：代币激励 (Tokenomics)

区块链是一套经济学。

-   **激励：** 奖励诚实节点（如区块奖励）。
    
-   **惩罚：** 增加作恶成本（如消耗 Gas Fee 或没收质押金）。
    

交易流转全过程：

1\. **签名发起：** 用户用私钥签署交易，证明资产所有权。

2\. **P2P 传播：** 交易在网络节点间迅速广播。

3\. **矿工验证：** 节点核对余额及签名真实性。

4\. **打包共识：** 矿工将多笔交易封装入块，并通过算法达成共识。

5\. **全网同步：** 新块挂载到链尾，交易正式获得“最终性”。

## 4\. 三大区块链类型的对比分析

| 类型 | 去中心化程度 | 访问权限 | 效率 | 典型案例 |
| --- | --- | --- | --- | --- |
| 公有链 (Public) | 最高 | 完全公开 | 低（共识复杂） | BTC, ETH |
| 联盟链 (Consortium) | 中等 | 预授权组织 | 中（多中心化） | Hyperledger, 蚂蚁链 |
| 私有链 (Private) | 低 | 组织内部 | 极高 | 企业审计、内部管理 |

## 5\. 去中心化的“不可能三角”与挑战

在区块链领域，存在一个著名的不可能三角**（Blockchain Trilemma）**：一个系统很难同时完美兼顾 **去中心化、安全、可扩展性（性能）**。

### 核心痛点：

-   **性能瓶颈：** 为了安全和去中心化，公链每秒处理交易数（TPS）远低于传统金融网关（如 Visa）。
    
-   **合规困境：** 匿名性与金融监管（KYC/AML）存在天然冲突。
    
-   **门槛过高：** 助记词丢失即资产找不回，这种“极致的自由”对普通用户来说也是“极致的风险”。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
