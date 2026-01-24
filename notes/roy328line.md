---
timezone: UTC+8
---

# roy328line

**GitHub ID:** roy328line

**Telegram:** @roy328328

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
## **Day 14 学习计划**

2026/01/25 总体学习计划如下：
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->

## **Day 13 学习计划**

2026/01/24 总体学习计划如下：

-   [Solidity 101](https://www.wtf.academy/zh/course/solidity101)
    

# Solidity 101

### **1）课程地图**

| 主题 | 要掌握的产出物 |
| --- | --- |
| HelloWeb3（3 行合约） | 会在 Remix 新建合约、编译、部署，并通过 public 变量 getter 读到返回值 |
| Value Types（值类型） | 能区分 value / reference / mapping / function；会用 bool、int/uint、address、bytesN、enum |
| Function（函数） | 会写 visibility（public/private/internal/external）与 pure/view/payable；理解哪些操作算“改状态” |
| Function Output（返回） | 会 returns/return、多返回值、具名返回、解构赋值（取全部或部分） |
| Data Storage and Scope | 会选 storage/memory/calldata；理解赋值是“引用”还是“拷贝”；会分辨状态变量与局部变量 |
| Array & Struct | 会静态数组、动态数组、bytes；会 push/pop/length；会 4 种 struct 初始化方式 |
| Mapping | 会声明 mapping；理解 key 类型限制；理解 mapping 只能在 storage；会写入与读取 |
| Initial Value | 记住常见类型默认值；会用 public getter 验证默认值 |
| Constant and Immutable | 会区分 constant 与 immutable 初始化时机；知道哪些类型可用 immutable |
| Control Flow | 会 if/for/while/do-while/三元；能读懂插入排序逻辑与常见坑 |
| constructor and modifier | 会用 constructor 初始化 owner；会写 onlyOwner modifier 做访问控制 |
| Events | 会声明 event、emit；理解 indexed 与 topics/data；会在交易详情里查日志 |
| Inheritance | 会写 is 继承、virtual/override；理解多重继承下 super 调用顺序 |
| Abstract and Interface | 会区分 abstract contract 与 interface；知道 interface 的限制与用途 |
| Errors | 会用 require/revert/assert 与自定义 error；理解自定义 error 的 gas 优势方向 |

### **2）类型、数据结构、数据位置速查表**

| 模块 | 关键点 | 典型坑 | 自测方式（Remix） |
| --- | --- | --- | --- |
| 值类型（Value Types） | bool、int/uint、address 与 address payable、bytesN、enum | bytesN 是值类型，bytes 是动态字节数组（引用类型) | 部署后直接点 public 变量按钮查看值 |
| 地址（address） | address payable 才有 transfer/send 成员，且可查 balance | 把 address 当成可转账地址用会报错或写不出代码 | 声明 address payable，读取 balance，必要时在 payable 函数里收 ETH |
| 数组（array） | 静态 T[k] 与动态 T[]；dynamic 才能 push/pop；memory 动态数组可 new，但长度固定 | memory 动态数组 new 完不能再变长；bytes 推荐胜过 bytes1[]，且不要用 byte[] | 写一个函数返回 length，或对 storage 动态数组 push/pop 看变化 |
| 结构体（struct） | struct 可含值类型与引用类型；可作为 array 或 mapping 的 value；4 种赋值方式（storage 引用、直接赋值、构造器、键值） | 方法 1 是 storage 引用，改它会影响原 struct（这是特性，不是 bug） | 分别调用 4 种 init 方法，读 student.id/score |
| 映射（mapping） | 格式 mapping(Key => Value)；Key 只能用内置类型（如 uint、address），不能用自定义 struct；mapping 必须在 storage；public mapping 自动生成 getter | 试图把 mapping 放进函数参数或返回值（尤其 public）会报错 | 调用写入函数后，用 getter 用 Key 读 Value |
| 数据位置（storage/memory/calldata） | storage 上链、gas 贵；memory 与 calldata 临时；calldata 只读，常用于函数参数 | storage → storage 通常是引用；storage ↔ memory 通常是拷贝；搞错会引起“改了副本没生效”或“意外改到原数据” | 写两组函数：一组用 storage 引用改数组，一组用 memory 拷贝改数组，对比结果 |

### **3）函数、返回、控制流速查表**

| 模块 | 关键点 | 你要记住的规则 | 自测方式（Remix） |
| --- | --- | --- | --- |
| 函数签名 | function name(params) visibility pure/view/payable returns(...) | visibility 必须写；pure 不能读写状态；view 只能读不能写；payable 才能收 ETH；“改状态”的行为包含写状态变量、emit 事件、创建合约、转 ETH 等 | 写 add / addView / addPure 三个版本，对照编译器报错与运行结果 |
| 函数返回 | returns 声明返回类型与变量名；return 在函数体返回值；支持多返回值、具名返回；支持解构赋值取全部或部分 | 解构赋值可以留空位，但数量要对齐；具名返回可以不写 return 也会自动返回 | 写 returnNamed，再用 (_a,_b,_c)=returnNamed 与 (, _b, )=returnNamed |
| 控制流 | if-else、for/while/do-while、三元；continue/break | 循环在链上有 gas 风险，复杂逻辑更易出错；课程用插入排序举例说明“看似简单但容易写错” | 跑插入排序示例，自己改一版并用多组数组测试边界情况 |

### **4）合约组织、可维护性与链上可观测性**

| 模块 | 核心用途 | 关键语法点 | 常见坑 |
| --- | --- | --- | --- |
| constructor | 部署时执行一次，用于初始化（如 owner = msg.sender） | 新版用 constructor 关键字；旧版曾用“同名函数”做构造器，容易写错导致安全问题 | 忘记初始化关键参数，或把部署者权限逻辑写反 |
| modifier | 抽离重复校验逻辑，常用于访问控制（onlyOwner） | modifier 内用 require 校验，_; 表示继续执行函数体 | require 不带错误信息不利排查；修饰器叠加顺序要可预测 |
| events | 把关键行为写入 EVM 日志，前端或分析工具可订阅；存事件通常比存新状态变量便宜 | event 可 indexed 参数进入 topics；其余进 data；topics 最多 4 个，通常第 1 个是事件签名哈希 | 把大数据放 indexed（会变成哈希或不易直接读）；忘记 emit 导致链上不可观测 |
| inheritance | 复用逻辑与扩展能力；多重继承时需要显式 override，理解 super 调用链 | virtual/override；多重继承下 super 依据线性化顺序向上调用 | override 写漏或继承顺序不当导致行为与预期不同 |
| abstract / interface | interface 用来定义标准化能力边界；abstract 可留未实现函数给子合约实现 | interface 通常只含函数声明与事件；实现方按接口签名实现 | 误把 interface 当成可部署合约；函数签名不一致导致无法编译 |
| errors | require/revert/assert 与自定义 error；自定义 error 往往更省 gas | error Name(params); revert Name(params) | 断言与前置条件混用；错误信息设计不利于定位问题 |
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->



## **Day 12 学习计划**

2026/01/23 总体学习计划如下：

-   **Speedrun Ethereum Basic**
    

# **Speedrun Ethereum Basic**

# Challenge #0 **Tokenization**

### **1) 核心知識點**

| 主題 | 一句話結論 | 對應方法/位置 |
| --- | --- | --- |
| Tokenization | 把「獨一份所有權」映射成鏈上可驗證狀態，並可被其他合約組合使用 | ERC-721 + composability |
| Ownership | NFT 的所有權由 ownerOf(tokenId) 決定 | Debug Contracts / 合約函式 |
| Gas | 每筆鏈上交易都要付 Gas；沒錢就無法 mint/transfer | faucet / 餘額 |
| Transfer 權限 | 只有 owner 或被授權者能轉移 | approve / setApprovalForAll |
| 持有數量 | 一個地址持有多少 NFT 用 balanceOf(address) | balanceOf |
| 事件 | 轉移會觸發 Transfer event（狀態原子更新） | Etherscan / logs |

### **2) 合約與腳本位置**

| 內容 | 路徑 | 需要看什麼 |
| --- | --- | --- |
| ERC-721 合約 | packages/hardhat/contracts/YourCollectible.sol | mint、transfer、ownerOf、approve |
| 部署腳本 | packages/hardhat/deploy/01_deploy_your_collectible.ts | 部署流程與參數 |
| 前端頁面 | packages/nextjs/app/…（如 /myNFTs/page.tsx） | UI 與合約互動呼叫 |

### **3)** 開發指令速查表 (CLI Cheatsheet)

| 環境情境 | 指令 (Command) | 功能說明 | | 啟動本地鏈 | yarn chain | 啟動 Hardhat 本地虛擬區塊鏈節點。 | | 部署合約 | yarn deploy | 將智能合約部署到目前的網路 (預設為 Localhost)。 | | 啟動前端 | yarn start | 開啟 Next.js 開發伺服器 (預設 localhost:3000)。 | | 生成部署帳號 | yarn generate | 產生一個新的部署用錢包 (Deployer Account) 與私鑰。 | | 查看帳號資訊 | yarn account | 查看部署用錢包的地址與 ETH 餘額。 | | 合約驗證 | yarn verify --network sepolia | 將合約原始碼上傳至 Etherscan 進行驗證。 |

### **4)** 關鍵設定檢查表 (Config Checklist)

| 檔案路徑 | 修改參數 | 目的 | | packages/hardhat/hardhat.config.ts | defaultNetwork: “sepolia” | 讓 Hardhat 預設指向 Sepolia 測試網，而非本地鏈。 | | packages/nextjs/scaffold.config.ts | targetNetwork: \[chains.sepolia\] | 讓前端 UI 能夠識別並連接到 Sepolia 網路。 | | packages/nextjs/scaffold.config.ts | onlyLocalBurnerWallet: false | 解除限制，讓使用者在測試網也能選擇使用 Burner Wallet (或連接 MetaMask)。 | | packages/hardhat/.env | ALCHEMY\_API\_KEY | (建議) 設定自己的 API Key 以避免共用限制。 |
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->






## **Day 11 学习计划**

2026/01/22 总体学习计划如下：

-   Uniswap 工作原理
    

|   | Uniswap v2 | Uniswap v3 | Uniswap v4 |
| --- | --- | --- | --- |
| 定位與做市模型 | CPMM／恒定乘積做市商 | CLMM／集中流動性做市商 | PLMM／可程式化流動性做市商 |
| 定價核心 | x · y = k | 以 tick 將價格離散化，並在不同區間內分段計算 swap | 引入 dynamic fee／tick spacing 等可配置參數（由 PoolKey 等結構識別池子） |
| LP 流動性形態 | LP 需嚴格按比例添加或移除流動性 | LP 可指定價格區間；流動性在 swap 過程中會隨價格跨點位而變化，需要輪詢處理 | 在 v3 的區間流動性基礎上，進一步把池子行為模組化，可透過 hooks 在關鍵流程插入自訂邏輯 |
| swap 計算特徵／難點 | 以恒定乘積推導 Δx、Δy，並在扣除費用後滿足 k 不變 | 價格推進時會遇到多個流動性變化點位，需逐段計算推到下一點位消耗的 x 或 y，再更新流動性 | 以 flash accounting 先記帳、最後淨額結算，利於多步組合操作與 gas 效率 |
| 費率與刻度設計 | 提及 price impact、slippage、IL、flash loan、TWAP oracle 等概念體系 | fee tier 與 tick spacing 綁定（0.01%→1；0.05%→10；0.30%→60；1.00%→200） | tick spacing 與 fee 可更自由配置（dynamic fee／tick spacing） |
| 狀態與資料結構要點 | v2 的 swap() 與恒定乘積曲線體系 | 用 tick bitmap 與 tick 結構體記錄流動性變化點位；LP 費用統計依賴 feeGrowthGlobal 與 feeGrowthOutside | singleton 單體合約架構；6909 claim token 作為領取憑證代幣；支援 native ETH；donate 機制 |
| 可擴展性與開發切入點 | 主要是理解 AMM 定價、滑點與套利、TWAP、flash loan 等機制 | 主要是理解集中流動性、tick 與費用累積分攤的結構設計 | hooks 是核心擴展點，可在 initialize、加減流動性、swap、donate 等生命週期前後掛載邏輯 |
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->







## **Day 10 学习计划**

2026/01/21 总体学习计划如下：

-   [Solidity by Example | 0.8.26](https://solidity-by-example.org/) Basic 部分
    

# [Solidity by Example | 0.8.26](https://solidity-by-example.org/) Basic

| 主題 | 主軸 | 誤區 |
| --- | --- | --- |
| Variables | 分清 local / state / global，決定資料是否上鏈 | 把會變動的資料放 local，結果狀態不會保存 |
| Constants | constant 編譯期固定，通常更省 Gas | 以為 runtime 能改，實際不可 |
| Immutable | immutable 只能在 constructor 設一次 | 以為跟 constant 完全一樣（其實設定時機不同） |
| State Variables | 改狀態要交易；讀狀態可免費 RPC | 把讀寫成本混在一起估 Gas |
| Ether / Wei | 金額用 Wei，1 ether = 1e18 wei | 用浮點思維處理金額，導致精度錯誤 |
| Gas | 交易成本與執行路徑、寫入 storage 高度相關 | 在迴圈中寫 storage，Gas 爆炸 |
| If / Else、Loop | 控制流程都會影響 Gas | 在 on-chain 迴圈做大量計算或遍歷動態陣列 |
| Mapping | Key → Value 的儲存結構，不可迭代、無長度 | 想「列出所有 key」，做不到 |
| Array | 動態陣列可 push/pop，刪除要設計策略 | 刪除後元素搬移造成 O(n) |
| Enum / Struct | enum 表狀態、struct 表資料模型 | 忽略預設值（enum 預設第 0 個） |
| Data Locations | reference type 必須分清 storage/memory/calldata | 不小心拷貝整個陣列，Gas 變貴 |
| Function | 回傳可用 named returns、解構、tuple | visibility 預設與呼叫方式搞混 |
| View / Pure | view 不改狀態；pure 不讀不改 | 在 view 中讀到會變的狀態以為「不耗 Gas」但仍需 RPC |
| Error | require/revert/assert/custom error 分工不同 | 用 assert 做參數檢查（語意不對） |
| Modifier | 把前置條件抽象成可重用的「守門」 | modifier 過度複雜，降低可讀性 |
| Events | 用來留鏈上 log，利於前端/索引器查詢 | indexed 用太多或塞大資料，查詢與成本不佳 |
| Inheritance | is 多重繼承，super 依線性化呼叫 | state variable shadowing 造成混亂 |
| Payable / Sending Ether | 收款靠 receive/fallback payable；付款多用 call | 仍用 transfer/send 忽略 Gas 限制與失敗處理 |
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->









## **Day 9 学习计划**

2026/01/20 总体学习计划如下：

-   Web3 实习手册[「智能合约开发」](https://web3intern.xyz/zh/smart-contract-development/)
    

## **Web3 实习手册**[「智能合约开发」](https://web3intern.xyz/zh/smart-contract-development/)**阅读**

## 一、Dapp 架構速記

### 1) 架構元件與資料流

| 元件 | 在做什麼 | 你要掌握的接口 / 產物 | 典型輸出 |
| --- | --- | --- | --- |
| 前端 UI | 顯示狀態、發起互動 | Provider / Wallet 注入、合約 ABI、合約地址 | 讀狀態、送交易 |
| 錢包 | 身分與簽名器 | eth_requestAccounts、交易簽名 | 已簽名交易 |
| RPC 節點 | 讀鏈、發送交易、訂閱事件 | JSON-RPC（HTTP / WebSocket） | balance、receipt、logs |
| 智能合約 | 鏈上邏輯與狀態機 | Solidity、事件 Events、合約介面 | 狀態改變 + Logs |
| Indexer | 把 Events 變成可查詢資料 | 事件解析、DB schema、同步策略 | PostgreSQL / API |
| 去中心化存儲 | 放大檔案與非結構化資料 | IPFS / Arweave | metadata、圖片、文件 |

### **2) 開發流程拆解**

| 階段 | 核心輸入 | 核心輸出 | 驗收標準 |
| --- | --- | --- | --- |
| 需求分析 | 功能與風險假設 | 合約介面草案 + UX 流程 | 能列出狀態、權限、事件 |
| 合約開發 | Solidity、測試 | 合約 + 測試用例 | 測試通過、事件完整 |
| Indexer | Events | 可查詢資料庫 / API | 同步穩定、可重放 |
| 前端開發 | ABI + 地址 | 可用 UI | 完成讀寫、錯誤處理 |
| 部署上線 | 測試網 → 主網 | 合約地址、前端部署 | Etherscan 可驗證、監控就緒 |

## 二、以太坊開發環境：工具選型表

### 1) Foundry vs Hardhat vs Remix

| 工具 | 定位 | 你會用到的核心命令 / 功能 | 適合場景 |
| --- | --- | --- | --- |
| Foundry | 高性能合約開發套件 | forge（build/test/deploy）、anvil（本地節點）、cast（命令行交互) | 重度合約工程、測試與腳本化部署 |
| Hardhat | JS 生態的以太坊框架 | hardhat node、本地部署腳本 | 與前端 / TS 生態整合、團隊協作 |
| Remix IDE | 免安裝的快速試驗場 | 編譯、部署、互動、檢視交易 | 新手入門、快速驗證想法 |

### **2) 最小環境清單**

| 類別 | 必要項 | 補充 |
| --- | --- | --- |
| 基礎 | Node.js（nvm 管理）、npm / yarn、Git | 用 LTS 版本降低相依性風險 |
| 合約工具鏈 | Foundry 或 Hardhat | 二選一即可起步 |
| 常用依賴 | OpenZeppelin、Chainlink 測試環境 | 優先用成熟庫做權限、重入保護等 |

## 三、RPC 節點：從概念到工程落地

### 1) RPC 的職責地圖

| RPC 能力 | 對應行為 | 產品中會用在哪 |
| --- | --- | --- |
| 讀鏈 | 查餘額、合約狀態、區塊、Gas 價格 | 首頁資料、持倉、歷史 |
| 發交易 | 廣播已簽名交易、查確認數、估 Gas | swap、stake、mint |
| 事件 | eth_getLogs、WebSocket 訂閱事件 | 通知、Indexer、即時 UI 更新 |

### **2) 常用 JSON-RPC 方法速查**

| 方法 | 用途 | 該記住的點 |
| --- | --- | --- |
| eth_getBalance | 查地址餘額 | params 帶 block tag（latest |
| eth_call | 只讀呼叫合約 | 不改狀態、通常不付 Gas |
| eth_sendTransaction | 發送交易 | 實務上多由錢包簽名後走 provider |
| eth_getTransactionReceipt | 查回執 | 看 status、gasUsed、logs |
| eth_getLogs | 查事件 | Indexer 的基礎能力 |

## 四、Solidity：最小可用知識面

### 1) 型別與資料結構

| 類別 | 代表型別 | 用途 | 注意點 |
| --- | --- | --- | --- |
| 基礎 | bool、uint / int、address、bytes、string | 狀態、計算、地址、序列化資料 | bytes / string 的 Gas 成本與存儲策略 |
| 複合 | array、mapping、struct、enum | 集合、索引、結構化資料 | mapping 不可遍歷，常需 Indexer 輔助 |

### **2) 可見性與狀態修飾符（合約介面設計核心）**

| 維度 | 選項 | 何時用 |
| --- | --- | --- |
| 可見性 | public / external / internal / private | external 多用於對外接口且更省 Gas；internal 給繼承或內部模組 |
| 狀態修飾 | pure / view / payable / 無修飾 | read-only 用 view；收款用 payable；其餘按是否改狀態 |

### **3) 合約結構與事件**

| 結構元素 | 作用 | 實務建議 |
| --- | --- | --- |
| pragma + license | 指定編譯器版本與授權 | 版本鎖定避免編譯差異 |
| 狀態變數 | 永久存儲於鏈上 | 常量 constant、immutable 的使用時機要清楚 |
| 函數 | 讀寫狀態、執行業務 | 對外接口要搭配事件與權限 |
| 事件 Events | 記錄狀態變化、供前端 / Indexer 監聽 | 重要動作必打事件，indexed 用在查詢維度 |

## **五、Remix 實操：把閉環跑通**

| 步驟 | 在 Remix 做什麼 | 驗收 |
| --- | --- | --- |
| 編譯 | 選編譯器版本、Compile | 無 error / warning |
| 部署 | 選環境（測試網 / 注入錢包）、Deploy | 拿到合約地址 |
| 互動 | 呼叫函數（write）並簽名送出交易 | Etherscan 看到交易與 Logs |
| 讀取 | 使用 call 讀狀態或讀 mapping / view function | 前端可同步展示 |

## **六、交易與 Gas：產品視角的必要理解**

| 概念 | 定義 | 對產品的影響 |
| --- | --- | --- |
| call | 只讀模擬執行，不改狀態 | 適合頁面資料讀取，通常不付 Gas |
| send | 交易上鏈改狀態 | 需要錢包簽名、付 Gas、等待確認 |
| receipt | 交易回執 | 用於更新 UI、顯示成功 / 失敗、解析 logs |
| events / logs | 合約發出的可索引訊號 | Indexer 與通知的核心輸入 |

## **七、部署到測試網：檢查清單**

| 事項 | 目的 | 要保存的關鍵產物 |
| --- | --- | --- |
| 選測試網（Sepolia / Holesky） | 與主網隔離風險 | chainId、RPC URL |
| 部署合約 | 生成鏈上地址 | 合約地址、部署者地址 |
| 觸發事件並在瀏覽器檢視 | 驗證 write path 正常 | 交易 hash、event logs |

## 八、前端整合：從 ABI 到可用原型

| 階段 | 動作 | 失敗時要處理的錯誤類型 |
| --- | --- | --- |
| 初始化 | 檢測 Web3 provider 並連線 | provider 不存在、網路不匹配 |
| 授權 | eth_requestAccounts | 使用者拒絕授權 |
| 實例化 | ABI + 合約地址建立合約實例 | 地址無效、ABI 不匹配 |
| 讀取 | .call() 讀狀態 | RPC 超時、資料不同步 |
| 寫入 | .send() 送交易並等待 receipt | Gas 不足、交易被拒、revert |

## 九、高階內容：優化、安全、審計、協作、L2

### 1) Gas 優化速查

| 技巧 | 目標 | 典型做法 |
| --- | --- | --- |
| 減少 storage 操作 | 降低最貴的 SSTORE / SLOAD 成本 | 先讀到 memory 再一次寫回 |
| 位壓縮 | 更省存儲槽位 | 用 uint128 等打包 struct |
| 迴圈優化 | 降低重複讀取與運算 | cache length、用 ++i |
| external 優先 | 降低 calldata 複製成本 | 對外接口用 external |

### **2) 常見漏洞與防護**

| 漏洞 | 典型成因 | 防護要點 |
| --- | --- | --- |
| 重入 Reentrancy | 外部 call 回調重進入 | Checks-Effects-Interactions、ReentrancyGuard |
| 預言機操縱 | 價格源不可靠 | Chainlink、多源驗證、TWAP |
| 溢出 / 下溢 | 舊版或 unchecked | Solidity 0.8+、謹慎使用 unchecked |
| 權限缺失 | 管理函數未保護 | onlyOwner、AccessControl、多簽 |
| 未初始化代理 | proxy 未初始化被接管 | 初始化流程與防呆 |
| 三明治 / 搶跑 | MEV 前後夾擊 | 交易保護、滑點與路由策略 |

### **3) 合約審計：工具與流程**

| 類別 | 工具 / 方法 | 解決什麼 |
| --- | --- | --- |
| 靜態分析 | Slither、MythX 等 | 規範問題、已知漏洞掃描 |
| 動態測試 | Fuzzing / property testing | 邊界條件與攻擊模擬 |
| 人工 Review | 資深審計員審業務邏輯 | 邏輯漏洞與經濟攻擊面 |
| 報告輸出 | Audit report | 問題分級、修復建議 |

### **4) GitHub 協作與 PR 流程**

| 主題 | 建議做法 |
| --- | --- |
| 分支策略 | main / develop / feature / fix / release |
| commit 規範 | feat、fix、docs、refactor、test、chore |
| PR 檢查 | 測試通過、lint 通過、文件齊全、至少 1 位 reviewer |
| 開源禮儀 | 變更配測試與文件、公開透明溝通、重大改動走 Discussion |

### **5) Layer 2：Rollup 對比**

| 類型 | 原理 | 優點 | 缺點 |
| --- | --- | --- | --- |
| Optimistic Rollup | 欺詐證明期 | EVM 相容、成本低 | 提現延遲（文中給出 1–2 週量級） |
| ZK Rollup | 零知識證明 | 安全性高、可快速提現 | 開發難、EVM 相容性不完整 |
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->












2026/01/19 总体学习计划如下：

-   021 学习以太坊第 4 章
    

## **《021学习以太坊》第四章学习**

### 智能合約典型用例地圖

| 類別 | 代表用例 | 典型合約能力 |
| --- | --- | --- |
| 貨幣/代幣 | ERC-20、ERC-721、ERC-1155、自訂標準 | 鑄造、轉帳、授權、事件紀錄 |
| DeFi | 借貸、DEX、衍生品、穩定幣、收益聚合 | 定價、清算、抵押、費率、結算 |
| NFT & 遊戲 | 鑄造、交易、盲盒、道具經濟 | 生成/分發、稀缺性、規則可驗證 |
| DAO & 治理 | 投票、國庫、多簽、Timelock | 權限、提案、延遲執行、金庫管理 |
| 存取控制 | 白名單、權限控管、訪問日誌、License | 角色管理、可驗證授權、審計軌跡 |
| 自動結算機器人 | 定時任務、再平衡、預言機驅動 | 條件觸發、策略執行、風控閾值 |
| ZK 隱私/驗證 | 不透露數據，只證明「滿足條件」 | 驗證證明、隱私資產/身份邏輯 |

### 為什麼 Solidity 仍是 EVM 生態「第一優先」

| 原因 | 意味著什麼 |
| --- | --- |
| EVM 原生 | 最貼近執行環境，行為可預期 |
| 生態最強 | 範例多、庫多、審計經驗多 |
| 標準成熟 | ERC 系列、常用模式與工具鏈完善 |
| 多鏈相容 | 多數 L2 / EVM 鏈可直接沿用 |
| 安全經驗最多 | 常見坑與防線有大量最佳實務可抄 |

### 影響部署成本的四大因子

| 因子 | 影響機制 | 你能做什麼 |
| --- | --- | --- |
| 合約複雜度 & bytecode 體積 | 代碼越大、初始化寫入 storage 越多，gasUsed 越高 | 減少重複邏輯、拆模組、用 library、少在 constructor 寫大量狀態 |
| Base Fee（網路擁堵） | 熱點期間暴漲，閒時降低 | 選低峰部署，先看 gas tracker |
| Priority Fee（小費） | 想快確認就加碼 | 不急可壓低，設定合理上限 |
| 部署鏈別（L1 vs L2） | L1 最貴最安全；L2 通常便宜很多 | 能上 L2 先上 L2 做首發/驗證 |

### 預估與控制部署成本 Checklist

| 步驟 | 你要做什麼 | 你要看什麼指標 |
| --- | --- | --- |
| 先跑本地/測試網 | Hardhat/Foundry/Truffle 部署一次 | gasUsed（部署消耗） |
| 上線前看即時 Gas | 查 Base Fee 與建議 Priority Fee | 當前費率與波動 |
| 做代碼與 storage 優化 | 降 bytecode、減 constructor 寫入 | bytecode size、部署 gas |
| 優先 L2 首發 | 先在 Rollup 上驗證商業閉環 | 成本、速度、用戶體驗 |

### 一頁式安全表

| 層面 | 建議 | 典型目的 |
| --- | --- | --- |
| 設計模式 | CEI、Pull-Payment、nonReentrant | 防重入、避免外部交互順序錯誤 |
| 語言層 | Solidity ≥ 0.8（預設溢出檢查）；謹慎用 unchecked | 防溢出、降低意外行為 |
| 權限控制 | Ownable/AccessControl + 多簽 + Timelock；禁用 tx.origin 鑑權 | 降權限風險、可審批升級 |
| 外部呼叫 | 用 call 並檢查回傳；ERC-20 用 SafeERC20 | 降兼容與轉帳陷阱 |
| 預言機 | 多源 + TWAP + 閾值/時延保護 | 防操縱、抗極端波動 |
| 隨機數 | VRF 或 commit–reveal；別用區塊屬性硬湊 | 防可預測/可操縱 |
| 可升級 | 走 Proxy 標準模式；不要依賴 selfdestruct | 可治理、可控升級 |
| 測試與審計 | 單元/邊界 + fuzz + invariant；必要時形式化驗證 | 提前捕捉極端狀態 |

### 工具鏈選型矩陣

| 情境 | 首選工具 | 理由 |
| --- | --- | --- |
| 入門 Demo / 小實驗 | Remix | 零安裝、快速上手 |
| 中大型工程化 | Hardhat 或 Foundry | 插件/工程化 vs 高性能測試與 gas 導向 |
| 高強度 fuzz/invariant | Foundry（配 anvil/mainnet fork） | 真實場景壓測、定位問題快 |
| 老專案維護 | Truffle + Ganache | 能用就用，逐步遷移 |
| 高級調試/模擬/監控 | Tenderly（加在任意工具之上） | Trace、回放、Debug 體驗大幅提升 |

### 透明上鏈與可審計性

| 事實 | 含義 | 行動建議 |
| --- | --- | --- |
| 合約部署後是公開的 | bytecode、交易、事件對所有人可見 | 以「公開可被檢查」為前提設計 |
| 狀態多為明文存儲 | 只是讀取介面不同 | 別把機密寫進 storage |
| 審計是安全模型的一部分 | 真正安全協議敢公開源碼 | 做源碼驗證 + 靜態分析 + 人工審計 + 漏洞賞金 |

### 升級策略

| 場景 | 推薦策略 | 權限配置 |
| --- | --- | --- |
| 小玩具 / 無資金 Demo | 直接重部署，前端改地址 | 不必升級系統 |
| 有資金、長期運營 | Proxy（Transparent 或 UUPS） | 多簽 + Timelock |
| 超大型模組化協議 | Diamond / 模組化架構 | 額外審計與工具支援 |
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->















## **Day 7 学习计划**

2026/01/18 总体学习计划如下：

-   学习中文排版规范：[https://github.com/sparanoid/chinese-copywriting-guidelines](https://github.com/sparanoid/chinese-copywriting-guidelines)
    

## 学习中文排版规范  
**1)** 空格規範（Spacing）

| 場景 | 規則 | 常見錯法/例外 |
| --- | --- | --- |
| 中文 ↔ 英文 | 需要加空格（提升可讀性與視覺節奏） | 例外：依官方定義的產品名（如「豆瓣FM」）照官方格式 (GitHub) |
| 中文 ↔ 數字 | 需要加空格 | 例如「買菜花了 5000 元」 (GitHub) |
| 數字 ↔ 單位 | 需要加空格（10 Gbps、20 TB） | 例外：度數/百分比不加（90°、15%） (GitHub) |
| 全形標點 ↔ 其他字符 | 不加空格（逗號、句號、驚嘆號等全形標點前後不留白） | 常見錯法：在「，」「！」前後加空格 (GitHub) |

## **2)** 標點符號（Punctuation）

-   **中文內容**：使用**全形中文標點**（！？，（）等）。
    
-   **數字**：一般情況使用**半形數字**；僅在海報/設計稿為了少量數字對齊，才可考慮全形數字。
    
-   **完整英文整句或特殊名詞**：其內容使用**半形標點**（例如英文引句內的逗號、句號）。
    
-   例外提醒：中文句子內若夾英文書名、報刊名，不建議借用中文書名號，應以英文斜體處理。
    

## **3)** 名詞與大小寫（Proper Nouns）

### 專有名詞用正確大小寫

避免 github/GITHUB/Github/gitHub 這類非官方寫法；若視覺上需要全大寫/全小寫，建議在 HTML 用標準大小寫，外觀再用 `text-transform` 控制。

### 不要使用不地道縮寫

例如把 TypeScript 寫成 Ts、把 HTML5 寫成 h5、把 React/Next.js 亂縮成 RJS/nextjs 等，會降低專業度與可讀性。

## **4)** 一頁速查清單

-   中英文、中文與數字、數字與單位：該不該加空格？
    
-   中文段落：是否使用全形中文標點？英文整句：是否使用半形標點？
    
-   標點：是否有「！！」「？？」等重複堆疊？
    
-   專有名詞：大小寫是否符合官方寫法？是否用了不地道縮寫（Ts、h5、RJS…）？
    
-   超連結前後：是否採用團隊一致的空格策略（本指南將其列為「爭議」項）？
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->
















## **Day 6 学习计划**

2026/01/17 总体学习计划如下：

-   Web3 实习手册[「社区运营指南」](https://web3intern.xyz/zh/community-intern/)部分
    

## **Web3 实习手册**[「社区运营指南」](https://web3intern.xyz/zh/community-intern/)**阅读**

### 1) 社区运营在 Web3 的定位

-   社区运营是连接用户与项目的桥梁，核心目标是用“内容、互动、活动、协作”把注意力与信任沉淀到社群，并形成可复用的工作流程。
    

### **2) 核心职责总表**

| 模块 | 要做到什么 | 常见输出物 |
| --- | --- | --- |
| 日常内容与社群维护 | 维持社媒活跃、管理秩序、答疑、反垃圾、设计话题引导讨论 | 日更或周更贴文、FAQ、社群规范、话题脚本 |
| 内容发布与互动引导 | 活动预热、开播提醒、多渠道分发，促进查阅与转发 | 活动海报、宣发文案、提醒信息、置顶帖 |
| 活动策划与组织 | 线上 Space 课程分享、黑客松，线下 meetup/workshop；覆盖前中后全流程 | 活动 flow、问题库、主持稿、活动回顾 |
| 对外合作与社群联动 | 联合 AMA、KOL 传播、媒体快讯，扩大曝光 | 联动方案、媒体 brief、KOL 名单与跟进表 |

### **3) 工具与平台地图**

| 目的 | 建议工具 | 需要熟练的点 |
| --- | --- | --- |
| 对外传播与叙事扩散 | X、微信公众号、Medium/Substack | 平台调性、分发节奏、可视化素材与转发动线 |
| 社群承接与支持 | Discord、Telegram、微信群 | 频道结构、权限与 Bot、AMA 支持与客服 |
| 内容生产与协作 | Notion、ChatGPT、Figma/Canva、Tally/Typeform | 文档管理、海报生成、报名与调研、协作分工 |
| 数据与调研 | Etherscan、Dune、CoinGecko/CoinMarketCap、Token Terminal | 链上查证、指标面板、价格与基本面、增长分析 |
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->





















## **Day 5 学习计划**

2026/01/16 总体学习计划如下：

-   开发环境并熟悉：[Remix IDE](https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.30+commit.73712a01.js)
    
-   熟悉[DeFiLlama、](https://defillama.com/)[Dune Analytics、](https://dune.com/)[Etherscan](https://etherscan.io/)
    
-   观看[**Stuck Transactions, Gas Limits, Multisigs, L2s, Lending…**](https://www.youtube.com/watch?v=11QTT6BK5j0&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=5)
    

## **Remix IDE** 熟悉

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/roy328line/images/2026-01-16-1768547482343-image.png)

## **DeFiLlama、Dune Analytics、Etherscan** 熟悉

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/roy328line/images/2026-01-16-1768548416087-image.png)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/roy328line/images/2026-01-16-1768548656918-image.png)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/roy328line/images/2026-01-16-1768548711623-image.png)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/roy328line/images/2026-01-16-1768548747789-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/roy328line/images/2026-01-16-1768548957655-image.png)

## **Stuck Transactions, Gas Limits, Multisigs, L2s, Lending… 学习**

### 1\. 交易机制与疑难排解

-   **卡住的交易与 Gas 费：** 演示如何故意设置极低的 Gas 费来让交易“浮动”在内存池（Mempool）中而不被处理。
    
-   **Nonce 的重要性：** 解释 Nonce（随机数）的概念，说明交易必须按顺序处理。如果 Nonce 较小的交易卡住，后续所有交易都会停滞。
    
-   **加速与取消交易：** 介绍如何使用 MetaMask 的“加速（Speed Up）”功能，以及通过发送一笔具有相同 Nonce 但 Gas 费较高、金额为 0 的交易来“取消”原交易。
    
-   **重置账户（Reset Account）：** 演示当 MetaMask 显示数据异常时，如何通过“高级设置”中的“重置账户”清空本地交易记录(这不会导致资产损失)。
    

### 2\. 直接与智能合约互动

-   **Etherscan 操作：** 演示不通过 dApp 界面，直接在 Etherscan 上连接钱包并调用智能合约（如 DAI 合约）的 `transfer` 函数来转账。
    
-   **ERC-20 批准模式（Approve Pattern）：** 解释在使用 DEX（如 Uniswap）交换代币时，为什么通常需要两笔交易：第一笔是“授权（Approve）”合约动用代币，第二笔才是实际的“交换（Swap）”。
    

### 3\. Layer 2 (L2) 解决方案与跨链桥

-   **Optimism 跨链：** 演示如何将资产从以太坊主网跨链至 Optimism，并解释 L2 的优势在于更低廉的费用与更快的交易速度。
    
-   **L2 实操：** 在 Optimism 上快速铸造 NFT（Optimistic Loogie），并在二层网络的市集（Quixotic）上架销售，其成本远低于主网。
    

### 4\. 去中心化金融 (DeFi) 与借贷

-   **Aave 借贷：** 介绍如何提供抵押品（如 ETH）来借出其他资产。
    
-   **风险提示：** 讨论杠杆与清算（Liquidation）的风险，提醒开发者应专注于构建工具而非沉迷于投机博弈。
    

### 5\. 多重签名钱包 (Multisig) - Gnosis Safe

-   **安全性：** 解释助记词一旦泄露资产就会归零的风险，并推荐使用智能合约钱包（如 Gnosis Safe）来提升安全性。
    
-   **设置与操作：** 现场建立了一个 2/4（四人中需两人签名）的多签钱包，并演示如何利用 **WalletConnect** 将多签钱包连接到 Uniswap 进行交易提案与签署。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->

























## **Day 4 学习计划**

2026/01/15 总体学习计划如下：

-   学习《021学习以太坊》中的第三章
    
-   观看[**NFTs!!! ERC20 vs ERC721, IPFS, Metadata**](https://www.youtube.com/watch?v=NOdrEpnoCiM&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=4)
    

## **《021学习以太坊》第三章学习**

### **1.** 账户类型总览

| 维度 | EOA（外部拥有账户） | 合约账户（合约/智能合约账户） |
| --- | --- | --- |
| 控制权来源 | 由公私钥对控制，谁持有私钥谁就控制账户。 | 无私钥，行为由账户内的 EVM 字节码决定。 |
| 能否主动发起交易 | 能，签名并广播交易，转账或调用合约。 | 不能主动发起外部交易，只能被 EOA/其他合约调用后执行。 |
| 代码与存储 | 没有代码与存储状态。 | 有 code（逻辑）与 storage（状态）。 |
| 创建方式与成本 | 本地生成 key pair，不需要上链与 gas。 | 通过创建合约交易部署字节码产生，需要支付 gas，并遵循 CREATE/CREATE2 规则。 |

### **2.** 账户长什么样

| 字段 | EOA 含义 | 合约账户含义 |
| --- | --- | --- |
| address | 由公钥派生，20 字节，常见展示为 0x + 40 位 hex。 | 同左。 |
| nonce | 已发送交易数量；同 nonce 只执行一次，用于防重放并保证交易有序。 | 统计用 CREATE/CREATE2 创建过多少个合约。 |
| balance | 持有的 ETH 数量。 | 同样可持有 ETH，也可持有 ERC-20/NFT。 |
| storageRoot | EOA 逻辑上为空。 | 合约存储 trie 的根哈希，是合约状态入口。 |
| codeHash | EOA 逻辑上无代码。 | 合约字节码哈希，用于定位合约代码。 |

### **3.** 代币与合约账户的关系

| 主题 | 教材的关键说法 |
| --- | --- |
| 你“拥有代币”是什么意思 | 资产记录在代币合约的 storage 映射里，不是地址里装了代币。 |
| ERC-20 记账方式 | 合约用 balances、allowances 等映射记账与授权。 |
| ERC-721 记账方式 | 记录 tokenId -> owner，以及余额等结构。 |
| “转代币”的本质 | 由你的 EOA 向代币合约发起交易，data 编码 transfer 等函数；合约执行并更新存储与事件。 |

### **4.** 合约可升级与 SELFDESTRUCT 的新语义

| 主题 | 结论 |
| --- | --- |
| SELFDESTRUCT 现在还能“删合约”吗 | Dencun 后（EIP-6780）只有在合约创建的同一笔交易里调用 SELFDESTRUCT”才会像以前一样删除代码与存储；否则通常只转走余额，不再删除代码与 nonce。 |
| 实务建议 | 新项目不应依赖 SELFDESTRUCT 做升级或清库，主流正确姿势是代理合约，地址不变，替换实现合约地址。 |

### **5.** 钱包如何管理 EOA，以及安全重点

| 项目 | 要点 |
| --- | --- |
| SRP 与本地存储 | MetaMask 创建钱包会本地生成 SRP，SRP/私钥保存在本地并用钱包密码加密，不会上传服务器；丢了 SRP 官方无法找回。 |
| 钱包的工作流 | 本地生成/加密保存 SRP → 本地派生私钥/地址 → 本地签名 → 通过 RPC 广播。 |
| 防诈骗底线 | 不把 SRP/私钥输入网页表单，不发给任何人，警惕钓鱼站与伪造签名弹窗。 |

## **NFTs!!! ERC20 vs ERC721, IPFS, Metadata 学习**

### 1\. NFT 的基础概念与市场

-   **数字所有权与来源证明**：NFT 提供了一种链上的来源证明，让创作者能直接证明作品的真实性，并与支持者建立直接联系。
    
-   **OpenSea 平台展示**：演示了如何使用 OpenSea 查看账户中的 NFT 库存，并解释了 Web3 身份（钱包）如何随用户在不同服务间移动。
    
-   **NFT 的赋能（Utility）**：讨论了 NFT 的实际用途，例如作为进入聊天室的门票（Token Gating）或参与治理投票。
    

### 2\. 铸造（Minting）实作演示

-   **从网站铸造**：演示了如何在项目网站（如 Meta Avatar）上连接钱包并支付费用来铸造一个新的 NFT。
    
-   **通过 Etherscan 直接互动**：展示了即使没有前端界面，开发者也可以直接在 Etherscan 上与智能合约互动来铸造 NFT。
    

### 3\. 技术深挖：ERC-20 vs. ERC-721

-   **代币标准差异**：Austin 在代码层面解释了\*\*同质化代币（ERC-20）**与**非同质化代币（ERC-721）\*\*的区别。
    
    -   **ERC-20**：记录的是账户的余额（Balance），所有代币都是一样的。
        
    -   **ERC-721**：记录的是每个特定代币 ID（Token ID）的拥有者（Owner），每个代币都是独一无二的。
        

### 4\. 元数据与 IPFS 存储

-   **IPFS 的重要性**：大多数 NFT 的图像和资料存储在 \*\*IPFS（星际文件系统）\*\*上，而非直接存储在区块链上以节省成本。
    
-   **内容定址（Content Addressable）**：解释了 IPFS 哈希值的原理——如果内容发生微小变化，地址就会改变，这保证了 NFT 内容的不可变性。
    
-   **链上 SVG**：介绍了一些完全在链上生成的 NFT（如 Loot 或 Loogies），其图像直接由合约代码渲染，虽然昂贵但具有极高的去中心化程度。
    

### 5\. 安全与创作者权益

-   **检查元数据**：警告用户应检查 NFT 的 `tokenURI` 指向何处。如果指向私人服务器而非 IPFS，创作者可能会随时更改图像（Rug Pull 隐患）。
    
-   **版税与激励**：讨论了如何将版税直接写入智能合约，让艺术家在每一次转售中都能自动获得分成。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->






























## **Day 3 学习计划**

2026/01/14 总体学习计划如下：

-   阅读Web3 实习手册[「安全与合规」](https://web3intern.xyz/zh/security/)部分
    
-   学习《021学习以太坊》中的第二章
    
-   观看[**ENS, DEX, Identity, Inventory, Sybil**](https://www.youtube.com/watch?v=wYSMNdIRoII&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3)
    

## **Web3 实习手册阅读**

### **1\. 安全与合规**

| 主题 | 最常踩线/中招点 | 典型后果 | 自保法 |
| --- | --- | --- | --- |
| 代币/募资 | ICO、承诺收益、面向境内募资或推广 | 非法集资/刑责风险 | 不碰“融资功能+可流通”的设计与宣传 |
| 交易/撮合 | 交易所式撮合、为境内用户提供交易服务 | 非法经营/合规风险 | 避免做撮合、出入金、拉盘导流 |
| 传销/拉新返利 | 多级返利、团队计酬、门槛费 | 传销认定 | 拒绝层级奖励与“拉人头”结构 |
| 赌博/链游 | 付费→随机收益→可兑现闭环 | 赌博认定 | 避免随机收益与兑现通道绑定 |
| 洗钱/资金流 | 代收代付、灰色 C2C、无 KYC 大额流转 | 冻卡/涉洗钱调查 | 留存资金来源证据，慎选通道 |

### **2.** 求职与出金

|   | 风险点 | 建议做法 |
| --- | --- | --- |
| 用工主体 | 境外注册、缺劳动保障 | 优先 EOR/合规主体；合同条款写清楚 |
| 薪酬结构 | 全 USDT/代币，波动与税务不确定 | 保留一定法币比例；代币视为高风险浮动薪 |
| C2C 出金 | 对手涉诈涉赌→冻卡 | 少走不明 C2C；分散出金，保留交易与沟通记录 |
| 入职尽调 | 不透明、强激励拉新 | 看白皮书、代币分配、是否面向境内、是否承诺收益 |

### **3.** 高频网安攻击

|   | 常见话术/载体 | 防护要点 |
| --- | --- | --- |
| 钓鱼链接 | 面试通知、奖学金、空投、账号异常 | 只信官方域名；绝不输入助记词/私钥 |
| 恶意软件 | 面试专用软件、破解包、文件投毒 | 仅从官方渠道下载；隔离环境打开附件 |
| 剪贴板/地址替换 | 转账时地址被偷换 | 核对“前后位”；先小额试转 |
| 恶意插件/供应链 | 浏览器扩展后门、依赖投毒 | 插件极简；避免来路不明扩展 |

### **4.** 日常 SOP

|   | 具体做法 |
| --- | --- |
| 分层钱包 | 主资产冷存；日常交互用小钱包；空投用一次性钱包 |
| 最小授权 | 用完就撤权；定期检查授权与已连接站点 |
| 账号安全 | 密码管理器+强 2FA；邮箱优先级最高 |
| 事件处置 | 疑似被攻：立刻断网、撤权、转移资产、保留证据 |

## **《021学习以太坊》第二章学习**

### **1\. 节点组成与分工**

| 模組 | 主要職責 | 對外/對內 |
| --- | --- | --- |
| 執行客戶端（EL） | 執行交易、維護狀態、提供 RPC、交易/區塊在 EL 網路傳播 | 對外：JSON-RPC |
| 共識客戶端（CL） | 跑 PoS、追蹤分叉選擇、驗證/傳播區塊與投票 | 對外：CL 的 P2P |
| 驗證者（可選） | 持鑰簽名，履行提議/證明等職責 | 只在質押時需要 |
| Engine API | 連接 CL↔EL 的內部協作通道 | 對內：專線 |

### **2.** 新节点上线流程（连网→同步）

| 阶段 | 目的 | 核心机制 |
| --- | --- | --- |
| 发现（Discovery） | 找邻居、建立“通讯录” | UDP + Kademlia 思路；引导节点、Ping/Pong、FindNode |
| 连接（Session） | 与邻居建立加密通道 | 加密连接；同一连接多路复用同步/传播等子协议 |
| 同步/传播（Sync & Propagation） | 跟上最新状态、补齐缺口 | Gossip 扩散新交易/新块/投票；请求-响应按需拉历史数据 |

### **3.** Merge 后 CL↔EL 的日常节奏（Engine API 三步）

| 动作 | 谁调用谁 | 一句话理解 |
| --- | --- | --- |
| newPayload | CL→EL | 把执行负载交给 EL 验证/执行 |
| forkchoiceUpdated | CL→EL | 告诉 EL 当前 head/safe/finalized 与分叉选择 |
| getPayload | CL←EL | 需要出块时，从 EL 取回已构建的区块负载 |

### **4\. 节点类型差异**

| 类型 | 存什么 | 成本 | 用途 | 信任/安全 |
| --- | --- | --- | --- | --- |
| 全节点（Full） | 全部区块 + 近期完整状态（旧状态可修剪） | 中等（磁盘数百GB～TB） | 自托管 RPC、参与网络、验证者底座 | 自己验证，抗审查强 |
| 归档节点（Archive） | Full + 全历史状态快照 | 极高（TB 级） | 浏览器、链上分析、回测/调试 | 可回答任意高度的任意状态 |
| 轻节点（Light） | 区块头 + 必要轻数据 | 很低 | 轻钱包/插件等 | 用状态根+Merkle 证明验证，但数据供给依赖全节点 |

### **5\. 选型速查**

| 需求 | 建议 | 重点 |
| --- | --- | --- |
| 本地开发/调试 dApp | Full（EL 快速同步 + 任一 CL） | 优先稳定 RPC、NVMe 与网络 |
| 指标/行为分析/回测 | Archive（或“托管+ETL+数仓”） | 成本高，但能查全历史状态 |
| 先体验、不想砸硬件 | 托管 RPC（Infura/Alchemy/QuickNode 等） | 需求稳定后再自建 |

## **ENS, DEX, Identity, Inventory, Sybil 学习**

### 1\. 钱包管理与账户创建

-   视频展示了如何在 MetaMask 钱包中使用同一个助记词（Seed Phrase）创建多个新账户。
    
-   演示了如何从一个账户发送 ETH 到另一个新账户，并解释了 **Gas 费用**（手续费）的运作机制。
    
-   提到了 **EIP-1559** 协议如何改善了 Gas 估算的准确性。
    

### 2\. ENS (以太坊域名服务) 注册

-   介绍了 **ENS** 的重要性，它类似于互联网的 DNS，能将复杂的钱包地址（如 `0x...`）转换为易读的名称（如 `name.eth`）。
    
-   演示了如何搜索并注册一个 ENS 域名，并讨论了**子域名**（Subdomains）在建立次要身份时的强大功能。
    

### 3\. 去中心化身份 (Identity) 与资产 (Inventory)

-   强调了 Web3 的一个核心概念：用户的**身份与资产会跟随钱包**穿梭于各个不同的去中心化应用（dApps）之间。
    
-   讨论了\*\*女巫攻击（Sybil Attack）\*\*与抵抗机制，解释了去中心化应用如何通过账户活动（如投票记录或多签签名）来判断一个账户是真人还是机器人。
    

### 4\. DEX (去中心化交易所) 与代币兑换

-   解释了 DEX 的运作原理，并将其比喻为一台**自动贩卖机**：你投入一种代币（如 ETH），换出另一种代币（如 DAI）。
    
-   通过实际操作展示了如何在去中心化平台上进行代币交换，并简述了自动做市商（AMM）如何根据池中资产比例调整价格。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->






































## **Day 2 学习计划**

2026/01/13 总体学习计划如下：

-   阅读Web3 实习手册[「入门导读」](https://web3intern.xyz/zh/blockchain-basic/)部分
    
-   学习《021学习以太坊》中的第一章
    
-   观看[**A Developer’s Guide to Building on Ethereum**](https://www.youtube.com/watch?v=zuJ-elbo88E&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=1) - Intro
    
-   观看[**Becoming a Power User - Wallets, Mnemonics, Keypairs**](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=2)
    

## **Web3 实习手册阅读**

### **1\. 行业赛道全览：Web3 的生态版图**

Web3 已从单一支付系统演变为多元生态，手册重点剖析了四大核心领域：

-   **DeFi (去中心化金融)：** 核心在于“无需准入”和“资产自托管”。
    

|   | Uniswap | Compound | MakerDao(SKY) |
| --- | --- | --- | --- |
| 类别 | 去中心化交易所(DEX) | 去中心化借贷协议 | 稳定币系统 |
| 核心机制 | 自动做市商(AMM)，恒定乘积公式：x×y=k | cToken 凭证动态利率 | 超额抵押生成稳定币 |
| 关键特点与创新 | 放弃传统订单簿，以流动性池驱动；价格随池内资产比例自动调整；无需中介，24/7 全天候交易 | 存入资产获得 cToken，代表存款份额与应计利息；利率根据市场供需动态调整；支持超额抵押借贷 | 抵押资产(如 ETH)铸造与美元挂钩的 DAI(升级为 USDS)；通过稳定费率调节供需；以清算机制维护系统稳定 |
| 参与方式 | 交易者：支付手续费兑换代币；流动性提供者(LP)：存入代币对赚取交易手续费(如 0.3%) | 存款人:存入资产赚取利息；借款人:提供抵押品后借出其他资产，需注意清算风险 | 用户：抵押资产获取流动性（稳定币）用于投资/支付；治理者：持有 MKR（现为 SKY）参与提案投票 |

-   **NFT (非同质化代币)：** 实现了数字所有权的唯一性，代表案例包括先锋项目 CryptoPunks 和交易平台 OpenSea。
    
-   **DAO (去中心化自治组织)：** 探索社区驱动的治理模式，如支持公共物品建设的 **LXDAO**。
    
-   **MEME ：** 分析了文化溢价下的 MEME 币现象，以及 AI + Web3、模块化区块链等 2025 年新兴趋势。
    
-   **交叉创新：**
    
    | DeFi + NFT（数字资产金融化） | DAO + MEME（社区文化 × 治理融合） | AI + DeFi（智能化金融服务） | Web3 + 乡建（南塘 DAO 的探索） |
    | --- | --- | --- | --- |
    
-   **新兴趋势:**
    
    |   | Intent-Based 交易 | 账户抽象与智能钱包 | 模块化区块链 | AI + Web3 融合 |
    | --- | --- | --- | --- | --- |
    | 定义 | 用户只表达目标，系统自动找最优路径执行 | 让账户具备智能合约级的灵活性，降低使用门槛 | 将执行、共识、数据可用性、结算拆分，以提升扩展性与可定制性 | 从去中心化 AI 训练到 AI 驱动的自动化交易，形成新应用范式 |
    | 项目 | UniswapX(荷兰拍卖意图交易)；1inch Fusion(零 Gas 意图聚合)；CoW Protocol(强调 MEV 保护的批量交易) | Safe；Argent；ZeroDev | Celestia；Polygon CDK；OP Stack | Fetch.ai；SingularityNET；Render Network |
    

### **2\. Web3 工作方式：远程协作与行业基因**

远程办公是 Web3 世界的生存法则，强调高效、异步且安全的协作习惯。

-   **必备工具箱：** 掌握社交媒体 (Twitter/Discord/Telegram)、资产管理 (MetaMask)、协作文档 (Notion)、代码托管 (GitHub) 及会议工具 (Zoom/Calendly)。
    
-   **远程协作素养：** 学习 OKR 目标管理、跨时区异步沟通技巧（超预期交付、埋点式复盘）以及如何防范日益猖獗的钓鱼和钓鱼诈骗。
    
-   **行业黑话 (Lingo)：** 熟练使用 **DYOR** (自行研究)、**FOMO** (害怕错过)、**WAGMI** (大家都会成功) 等文化语言，这是快速融入 Web3 社区的“门票”。
    

## **《021学习以太坊》第一章学习**

这本书第一章主要讲了三件事：

### **1\. 以太坊运转原理**

-   **世界计算机**：它不只能转账，还能跑智能合约（Smart Contracts），就像一台自动售货机，代码写好了就自动执行。
    
-   **经济账 Gas**：干活得付钱，Base Fee 还会被销毁来让 ETH 更有价值。
    
-   **两种账户**：用的钱包叫 EOA，存代码的叫 CA。
    
    | ETH | 協議層 | 應用層 | 開發者 |
    | --- | --- | --- | --- |
    |   | Gas / 手續費 | 記帳單位&結算貨幣 | 智能合約 |
    |   | PoS質押保證金 | 抵押品 / 流動性資產 | Gas fee 估算 |
    |   | 經濟安全錨定 | NFT | 經濟效益與安全假設 |
    

### **2.** 生態三塊版圖

|   | DeFi | NFT | DAO |
| --- | --- | --- | --- |
| 定义 | 金融操作系统 | 数字资产与身份层 | 组织与治理层 |
| 创新 | AMM/DEX 用公式自动报价撮合，任何人可提供流动性并以手续费形式获利；借贷走资产池与超额抵押，利率可由利用率自动调整，非托管与清算由代码执行；稳定币如 DAI 透过超额抵押与清算维持挂钩，成为 DeFi 计价与结算基础积木 | ERC-721 对应单一唯一资产；ERC-1155 适合在同合约管理多类资产与批量铸造；ERC-6551 让 NFT 绑定合约帐户；ERC-7432 引入角色与时效权限，让 NFT 从所有权延伸到可编程权限管理。 | 协议 DAO 可管理风险参数、金库、升级，治理代币把价值与治理权绑定。以合约为规则，以代币或 NFT 为成员凭证，以链上投票做决策。 |

## **A Developer’s Guide to Building on Ethereum - Intro 学习**

### 1.区块链的核心概念

-   **去中心化网络**：解释了区块链是由全球数千个节点组成的对等网络，具有极强的抗破坏性和抗审查性。
    
-   **信任最小化**：通过交易案例说明智能合约如何取代中心化中介，将信任从“人”或“机构”转移到“开源代码”上。
    

### 2\. 开发者职涯建议

Austin 提出了 Web3 开发者的三个成长阶段：

-   **入行**：掌握基础，获得 Web3 企业录用。
    
-   **独立开发**：具备开发自有产品并获取用户的能力，成为“超级个体”。
    
-   **安全审计**：专注于智能合约安全，成为高薪的合约审计师。
    

## **Becoming a Power User - Wallets, Mnemonics, Keypairs 学习**

### 1\. 设置 MetaMask 钱包

-   演示了在 Chrome 浏览器中安装 MetaMask 的过程。
    
-   **警告**：必须确保从官方网站下载，严防钓鱼网站诈骗。
    

2\. 助记词 (Seed Phrase) 的极端重要性

-   **核心逻辑**：助记词（12 个单词）是你的主私钥。掌握助记词就等于掌握了钱包里所有的资产。
    
-   **安全准则**：不要截图、不要存入云盘、不要发给任何人。建议手写在纸上并离线妥善保存。
    

3\. 本地密码 vs. 助记词

-   澄清了一个误区：MetaMask 的“本地密码”仅用于该设备。如果你在另一台电脑登录，必须使用“助记词”才能还原钱包。如果助记词丢失，资产将永远无法找回。
    

4\. 区块链账户的本质

-   解释了在以太坊上创建账户本质上是生成一个随机的大数（私钥）。这是一个“无需许可”的过程，任何人都可以免费、无限量地创建账户。
    

5\. 成为进阶用户的练习

-   鼓励学习者在不同钱包之间发送测试币（Testnet ETH），熟悉交易流程和区块浏览器（如 Etherscan）的使用，并练习通过助记词还原钱包以确保备份正确。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->


















































Web3 实习笔记 2026/01/12

学习计划

-   Web3intern 实习手册：\*\*[**区块链基础概念**](https://web3intern.xyz/zh/blockchain-basic/) [**以太坊概览**](https://web3intern.xyz/zh/overview-of-ethereum/)
    
-   [《从零到一学以太坊》](https://github.com/XiaoHai67890/021Ethereum) 第一章
    

TL;DR  
区块链是分布式账本，数据以区块形式串成链。  
哈希将任意数据映射为固定长度摘要，输入微小变化会导致输出显著变化。  
重写历史需要长期掌握多数共识资源并持续压过全网，成本极高。

以太坊是可编程区块链平台，智能合约在 EVM 上执行，ETH 用来支付 Gas 费用。  
The Merge 后以太坊从 PoW 切到 PoS，并形成“共识层 + 执行层”的两层架构，能耗显著下降。  
扩容主线是 Rollup，L1 更偏安全与数据可用性，EIP-4844 通过 Blob 交易降低 L2 数据上链成本与费用。

一、区块链是什么

核心定义  
区块链是一种分布式账本，数据被打包成一个个区块，并用前一区块的哈希把区块串接成链。  
在共识机制下，网络会选择唯一有效链；若要重写历史，需要长期掌握多数共识资源并持续产出更长或更重的替代链，因此成本极高而难以成立。

关键名词  
区块（Block）：承载一批交易与元数据的资料单元，通常包含前一区块哈希以及区块头等信息。  
哈希（Hash）：将任意资料映射为固定长度摘要的函数，输入微小变化会导致输出大幅变化，用于快速校验数据是否被改动。  
共识（Consensus）：一套让分布式节点在缺乏中心仲裁的情况下，对“下一块/下一笔有效状态”达成一致的规则；常见机制包括 PoW（算力）与 PoS（质押权重/投票权）。

机制拆解：为什么不可篡改  
前提：攻击者需要长期掌握多数共识资源（算力或质押权重）。  
过程：攻击者必须持续产出更长或更重的替代链，并在共识竞争中长期压过诚实节点；一旦被追上，其替代链会被网络按规则抛弃并发生重组。  
结果：由于获取并维持多数共识资源的成本极高，重写历史的攻击在现实中难以持续成立。

常见误区：不可篡改不是物理上的不能改，而是在既定共识规则下，重写历史需要长期压过全网共识资源，成本与风险远高于潜在收益。

二、比特币（BTC）解决了什么问题

问题  
在没有中心仲裁的情况下，如何让账本状态与交易顺序形成一致。

为什么交易顺序重要  
若交易顺序无法一致，不同节点可能对同一笔资金的先后花费产生分歧，从而出现双花或冲突交易的判定不一致。

**比特币把记账权与区块奖励/交易费绑定，让分布式节点在无中心仲裁下对账本状态与交易顺序形成一致，并用激励约束作恶动机。**

总结  
问题：在无中心仲裁下达成一致的账本状态与交易顺序。  
风险：双花与冲突交易的判定分歧。  
机制：记账权与区块奖励/交易费绑定，通过算力竞争产生下一块。  
结果：作恶需要高成本且成功率不确定，被网络否决会浪费算力与潜在奖励，诚实记账更可能成为均衡。

三、区块链的核心组成

P2P 网络（同步）：负责在无中心服务器的情况下，将交易与区块在节点间广播与同步。  
账本数据结构（记录）：以可校验的方式记录系统状态（如余额与合约状态）及其历史变更。  
共识 + 激励（协调）：让分布式节点对唯一有效链/交易顺序达成一致，并用经济激励约束作恶动机。

四、公链 / 联盟链 / 私链

定义  
公链：任何人可加入并运行节点参与验证，数据通常公开可读。  
联盟链：多机构共同管理，通过授权加入并参与验证，读写权限按联盟规则分层。  
私链：单一机构管理，读写权限由管理者集中控制。

关键对比（金融结算视角）

|   | 公链 | 联盟链 | 私链 |
| --- | --- | --- | --- |
| 节点加入 | 任何人可加入 | 需联盟治理/授权 | 由管理者决定 |
| 数据可见性 | 公开 | 部分公开 | 内部可见 |
| 读写权限 | 开放读，写需按规则参与共识 | 对联盟成员开放读，写对授权节点开放 | 读写由单一机构配置与控制 |
| 适用场景 | 适合面向公众的开放式金融与公开可验证结算 | 适合多机构清算、对账与结算（需要权限与合规边界） | 适合单一机构内部账务、审计与流程系统 |

五、Web2 vs Web3

Web2：平台/中心化组织控制账户体系与资产记账。  
Web3：用户通过私钥控制钱包地址与资产操作权；平台与治理更难被单点任意更改或关停，运行依赖多方节点共同参与。

六、以太坊是什么

核心定义  
以太坊是开源、去中心化区块链平台，核心创新是智能合约，让规则以代码形式自动执行，并支撑 DeFi、NFT、DAO 等应用生态。

七、Ethereum 与 Bitcoin 的差异

|   | Ethereum | Bitcoin |
| --- | --- | --- |
| 定位 | 去中心化应用平台与可编程基础设施 | 数字货币与价值储存 |
| 编程能力 | 支持图灵完备语言与复杂合约 | 脚本能力有限 |
| 共识机制 | PoW 迁移到 PoS（The Merge） | PoW |
| 确认速度 | 约 12 秒一块 | 约 10 分钟一块 |

八、 从 PoW 到 PoS（The Merge）

-   阶段 1：PoW（以太坊 1.0）  
    矿工用算力竞争出块，能耗高；扩展性有限。
    
-   阶段 2：双链并行到合并  
    2020 年 12 月启动信标链 Beacon Chain（先跑 PoS 共识）  
    2022 年 9 月 15 日发生 The Merge，主网切换到由信标链提供 PoS 安全性。
    

九、 PoS 机制拆解

-   准入：质押 32 ETH 成为验证者
    
-   选择：系统随机选择验证者提议与验证区块
    
-   收益：新发行 ETH + 交易费用
    
-   惩罚：作恶会触发 Slashing，质押 ETH 被销毁
    
-   优势：能耗降低约 99.95%，并强调经济安全性与更快确认。
    

十、 账户系统 + Gas + EVM

1.  账户系统  
    \-EOA 外部账户:由私钥控制，可主动发起交易  
    \-CA 合约账户:由合约代码驱动，不能主动发起交易，只能在被触发调用时执行代码逻辑
    
2.  Gas 模型  
    \-目的一：激励矿工或验证者优先处理交易  
    \-目的二：防止资源滥用，避免合约死循环拖垮网络  
    \-计算：Gas Limit × Gas Price  
    EIP-1559 后拆分：Base Fee 自动计算并销毁；Tip 作为额外激励给出块者
    
3.  EVM  
    图灵完备、全网同步执行、隔离安全沙箱式运行，用于保证每个节点对合约执行的结果一致。
    

十一、 升级路线: 主网更聚焦安全与数据可用性，L2 负责大量交易处理

-   **EIP-4844 (Cancun 升级)**
    

| 痛点 | 过去 L2 把数据提交到 L1 成本高 |
| --- | --- |
| 改动 | 引入 Blob 交易类型 |
| 效果 | L2 费用下降约 70%–90% |

-   **全面分片预计 2025-2026 年 启动，重点是 Proto-Danksharding**
    
-   **ZK-Rollup**
    

| 批量处理 | 在链下把大量交易先执行并生成状态变更 |
| --- | --- |
| 零知识证明 | 对这批交易生成一个简洁的正确性证明 |
| 主网验证 | 以太坊只验证证明即可，无需逐笔验证 |

十二、 L1、L2、侧链

按网络层级  
\-L1：主网，最终安全性与共识；EVM；账户系统  
\-L2：Rollup，把交易批量处理后提交到 L1 以降低 Gas  
\-侧链：独立运行，通过桥与主网交互

十三、 三层架构

\-应用层：DeFi、NFT、钱包、DAO 工具等  
\-协议层：共识层客户端、执行层客户端、核心协议如状态管理与 Gas 机制  
\-扩展层：L2 Rollups、侧链、状态通道等
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
