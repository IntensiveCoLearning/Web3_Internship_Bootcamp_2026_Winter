---
timezone: UTC+8
---

# Duamixu

**GitHub ID:** Duamixu1

**Telegram:** @Duamieee

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->
# Hyperledger Fabric External Builders (外部建構器) 詳解筆記

## 1\. 背景與演進 (Background & Evolution)

### Fabric 2.0 之前的限制 (Peer-Centric Model)

在 Hyperledger Fabric 2.0 版本之前，Chaincode 的生命週期（構建、啟動）與 Peer 節點緊密耦合。其運作邏輯如下：

-   **硬編碼流程**：Chaincode 的構建邏輯被寫死在 Peer 的程式碼中，僅支援特定的語言（Go, Java, Node.js）。
    
-   **Docker 依賴**：Peer 必須訪問 Docker Daemon 才能創建並啟動 Chaincode 容器。這導致了「Docker-in-Docker」的部署複雜度，且在 Kubernetes 等環境中需要較高的權限（通常需 Root 權限），存在安全隱患。
    
-   **靈活性不足**：開發者難以自訂構建環境（例如需特殊依賴庫），且 Chaincode 必須隨 Peer 一起部署，無法作為獨立的長期服務運行。
    

### Fabric 2.0 之後的解決方案 (External Builder Model)

Fabric 2.0 引入了 **External Builders and Launchers** 機制，將 Chaincode 的生命週期管理從 Peer 核心中解耦。

-   **自訂構建環境**：允許維運人員透過編寫腳本（Buildpacks）來定義如何編譯和打包 Chaincode。
    
-   **去 Docker 化**：不再強制 Peer 直接操作 Docker Daemon。Chaincode 可以運行在任何地方（Pod、虛擬機、裸機），只要 Peer 能透過網路連接即可。
    
-   **Chaincode as a Service (CaaS)**：這是此功能帶來的最大變革，Chaincode 可作為獨立的伺服器程序運行，大幅提升部署彈性。
    

* * *

## 2\. 核心運作機制 (Core Mechanism)

External Builder 的設計靈感源自 Heroku Buildpacks。其本質是一組位於 Peer 節點文件系統中的可執行腳本。當 Peer 接收到安裝或啟動 Chaincode 的請求時，會依序呼叫這些腳本。

一個完整的 External Builder 必須包含以下四個標準腳本（通常位於 `bin/` 目錄下）：

### A. `bin/detect` (偵測)

-   **功能**：判斷此 Builder 是否適用於當前安裝的 Chaincode Package。
    
-   **輸入**：Chaincode 源代碼元數據（Metadata）。
    
-   **輸出**：
    
    -   Exit code `0`：表示適用，Peer 將使用此 Builder。
        
    -   Exit code `1`：表示不適用，Peer 將嘗試下一個 Builder。
        

### B. `bin/build` (建構)

-   **功能**：將 Chaincode 源碼轉換為可執行的產物（Artifact）。
    
-   **輸入**：源代碼路徑、輸出路徑。
    
-   **行為**：在此階段可以執行編譯（如 `go build`）、依賴安裝（如 `npm install`），或僅僅是準備連接資訊（針對 CaaS 模式）。
    

### C. `bin/release` (釋出 - Optional)

-   **功能**：提供關於 Chaincode 的元數據給 Peer（通常用於 Docker 容器環境）。
    
-   **重點**：在「Chaincode as a Service」模式下，此步驟通常較少使用或僅返回空結構。
    

### D. `bin/run` (執行)

-   **功能**：啟動 Chaincode。
    
-   **行為**：在標準模式下，此腳本會啟動 Chaincode 流程。但在 CaaS 模式下，此腳本僅需將 Chaincode 的連接端點（Endpoint）及 TLS 憑證回傳給 Peer，告知 Peer 如何連接已在外部運行的 Chaincode 服務。
    

* * *

## 3\. 配置實作 (Configuration Implementation)

要啟用此功能，需在 Peer 的配置文件 `core.yaml` 中定義 `externalBuilders` 區塊。

### 配置範例 (`core.yaml`)

YAML

```
chaincode:
  externalBuilders:
    - path: /opt/hyperledger/builders/golang  # Builder 腳本所在路徑
      name: external-go-builder               # Builder 名稱
      propagateEnvironment:
        - GOPROXY                             # 需要傳遞給 Builder 的環境變數
```

### 部署流程小結

1.  **準備 Builder**：編寫 `detect`, `build`, `run` 等腳本。
    
2.  **配置 Peer**：修改 `core.yaml` 指向腳本路徑。
    
3.  **重啟 Peer**：載入新的配置。
    
4.  **安裝 Chaincode**：Peer 會自動遍歷配置的 Builders，直到 `detect` 返回 `0`。
    

* * *

## 4\. 應用場景與優勢 (Scenarios & Benefits)

### 1\. Chaincode as a Service (CaaS)

這是 External Builder 最強大的應用。

-   **運作方式**：Chaincode 不由 Peer 啟動，而是由維運人員（或 K8s Operator）手動部署為一個獨立的 Deployment/Service。
    
-   **流程**：`bin/run` 腳本不啟動程式，而是回傳 `connection.json`（包含 Chaincode 的 IP 和 Port）。
    
-   **優點**：Peer 變成了單純的 gRPC 客戶端，Chaincode 變成了伺服器端。
    

### 2\. Kubernetes 原生整合

-   **解決痛點**：移除了 Peer 對 Docker Socket 的依賴，避免了特權容器（Privileged Containers）的安全風險。
    
-   **架構優化**：Chaincode 可以是獨立的 Pod，與 Peer Pod 分離，便於資源管理與監控。
    

### 3\. 開發效率提升 (Hot Reload)

-   **傳統模式**：修改代碼 -> 重新打包 -> 重新安裝 -> 重新實例化 (耗時數分鐘)。
    
-   **外部構建模式**：
    
    -   在本地或開發伺服器啟動 Chaincode。
        
    -   修改代碼後，僅需重啟 Chaincode 進程（秒級）。
        
    -   Peer 透過固定的地址連接，無需重新執行鏈上的 Lifecycle 交易。
        

* * *

## 5\. 總結 (Summary)

| 特性 | 傳統 Peer 構建 (Pre-2.0) | 外部構建器 (External Builder) |
| 依賴性 | 強依賴 Docker Daemon | 無特定依賴，可視需求自訂 |
| 靈活性 | 僅限特定語言 (Go/Java/Node) | 支援任意可執行二進制文件 |
| 安全性 | 需 Docker Socket 權限 (高風險) | 無需特殊權限，符合最小權限原則 |
| 維運模式 | Chaincode 隨 Peer 啟動 | Chaincode 可作為獨立服務 (CaaS) |
| 主要用途 | 簡單測試、非容器化環境 | K8s 生產環境、複雜 CI/CD、本地快速除錯 |

**一句話總結：**

External Builder 透過將 Chaincode 的編譯與執行權限從 Peer 剝離，實現了 Chaincode 的「微服務化」，徹底解決了 Fabric 在 Kubernetes 上部署的架構痛點，並大幅提升了開發迭代速度。
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->

学习马老师的学习day3  
**練習 hardhat**

使用 bun，效能更好

```
mkdir my-hardhat-project
cd my-hardhat-project
bun init -y
```

安裝 hardhat

```
bun add --dev hardhat
```

初始化

```
bunx hardhat --init
```

在 ignition那邊可以部署

ignition/modules/Counter.ts

```
import { buildModule } from "@nomicfoundation/hardhat-ignition/modules";

export default buildModule("CounterModule", (m) => {
  const counter = m.contract("Counter");

  m.call(counter, "incBy", [5n]);

  return { counter };
});
```

然後開啟 node

```
bunx hardhat node
```

部署

```
npx hardhat ignition deploy ignition/modules/Counter.ts --network localhost
```
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->


🚀 本周Web3修炼手册：技术、乡村与未来

**💡 认知刷新**

1.  **信任迁移**：从“信人”转变为“信机器”，但机器之下依然需要社区文化的支撑。
    
2.  **角色重定义**：在AI时代，Web3开发者 = 架构师 + 审计员 + 经济模型设计师。
    
3.  **技术祛魅**：区块链不是万能药，它是解决“信任成本”和“价值确权”的专用手术刀。
    

**🛠️ 技能树点亮**

-   **极速开发**：RainbowKit + Next.js，10分钟搭建DApp脚手架。
    
-   **黄金组合**：Foundry写逻辑（快） + Hardhat做部署（稳）。
    
-   **安全铁律**：警惕访问控制漏洞，坚持 Checks-Effects-Interactions 模式。
    
-   **成本优化**：告别Sepolia，全面转向 Base/Arbitrum 等L2测试网。
    

**🌾 案例：南塘DAO的启示**

-   **故事账本**：把冷冰冰的Hash值变成村民看得懂的“功劳簿”。
    
-   **治理智慧**：用Web3固定规则，用罗伯特规则保护表达，解决“人情社会”的治理难题。
    

**🔮 趋势洞察**

-   **DeFi进化**：Aave V4架构优化，DEX恒定乘积公式
    
    $$x \\cdot y = k$$
    
    仍是基石。
    
-   **三层隐私**：网络层(Tor) -> RPC层 -> 交易层(ZK)，构建全方位防护。
    
-   **未来已来**：AI Agent 将成为链上交互的主要活跃用户。
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->



今天抄马老师 作业  
整體而言跟平常設置 ai agent 很像

是用範例搞的，deepwiki 讀取 repo 來回答問題

先搞個 DeepWikiAgent 的 class

```
class DeepWikiAgent(SpoonReactMCP):
    """Agent that can analyze GitHub repositories via DeepWiki MCP"""
    name: str = "DeepWikiAgent"
    system_prompt: str = """You are a helpful assistant that can analyze GitHub repositories using DeepWiki.
When asked about a repository, use the read_wiki_structure tool with the repoName parameter.
For example, if asked about "XSpoonAi/spoon-core", call read_wiki_structure with repoName="XSpoonAi/spoon-core".
Always use the tool to get information about repositories before answering questions."""

    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        # Initialize with empty tool manager, will load in initialize()
        self.available_tools = ToolManager([])

    async def initialize(self):
        # Create MCP tool for DeepWiki (no API key needed!)
        # Important: Use the actual tool name from the server: "read_wiki_structure"
        print("Connecting to DeepWiki MCP server...")
        try:
            deepwiki_tool = MCPTool(
                name="read_wiki_structure",  # Use actual tool name, not "deepwiki"
                description="Analyze GitHub repositories and get their structure and documentation",
                mcp_config={
                    "url": "https://mcp.deepwiki.com/sse",
                    "transport": "sse",
                    "connection_timeout": 30, # Explicit timeout key for MCPTool
                    "timeout": 30,
                }
            )
            # Pre-load parameters so LLM can see the correct schema
            await deepwiki_tool.ensure_parameters_loaded()
            self.available_tools = ToolManager([deepwiki_tool])
            print("Successfully connected and loaded DeepWiki tools.")
        except Exception as e:
            print(f"Failed to connect to DeepWiki MCP: {e}")
            print("Continuing with no tools...")
```

會`self.available_tools = ToolManager([])` 來接入tools

`initialize` 這邊設定too

```
deepwiki_tool = MCPTool(
                name="read_wiki_structure",  # Use actual tool name, not "deepwiki"
                description="Analyze GitHub repositories and get their structure and documentation",
                mcp_config={
                    "url": "https://mcp.deepwiki.com/sse",
                    "transport": "sse",
                    "connection_timeout": 30, # Explicit timeout key for MCPTool
                    "timeout": 30,
                }
            )
```

然後 `await deepwiki_tool.ensure_parameters_loaded()` 先預載入，起到了「同步元資料」的作用。簡單來說，它是為了讓 Agent 在正式運作之前，先搞清楚這個工具（Tool）到底需要哪些參數

再來 `self.available_tools = ToolManager([deepwiki_tool])` 來放入管理的tools中

最後在main 中呼叫這個class並，並run

```
 agent = DeepWikiAgent(
        llm=ChatBot(
            llm_provider=provider, 
            model_name=model  
        )
    )
    
    print("Initializing agent and loading MCP tool...")
    await agent.initialize()
    
    # Query the agent with a clear request
    query = "What is XSpoonAi/spoon-core about? Please analyze the repository and summarize its purpose."
    print(f"\nQuery: {query}")
    
    try:
        response = await agent.run(query)
        print("\nResponse:")
        print(response)
    except Exception as e:
        print(f"\nAgent execution failed: {e}")
```

成果：

![Screenshot 2026-01-25 at 12.15.06 AM.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Toby1009/images/2026-01-24-1769271615155-Screenshot_2026-01-25_at_12.15.06_AM.png)
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->




-   以 **pigeons** 作為通訊層，串接 **Trust Wallet**（對應的原生能力/介面），完成錢包相關核心流程：
    
    -   **金鑰生成**（key generation）
        
    -   **交易/資料簽章**（signing / signature）
        
    
-   專案整體架構採用 **MVVM**，並以 **Riverpod** 作為狀態管理與依賴注入方案：
    
    -   View / ViewModel 職責切分清晰
        
    -   狀態、流程與資料來源可測試化（可替換的 providers / repository 抽象）
        
    
-   測試策略同時涵蓋：
    
    -   **單元測試**：聚焦在 ViewModel、use case、repository、provider 行為與邏輯分支
        
    -   **整合測試**：驗證多層協作（通訊層 ⇄ domain ⇄ UI/狀態）與關鍵流程的端到端一致性
        
    
-   在工程化流程上使用 **git hooks**（例如 pre-push hook），於 **push 前**強制完成完整測試套件：
    
    -   統一開發節奏、避免未通過測試的提交進入遠端
        
    -   降低回歸與 CI 失敗機率，提升主幹穩定性
        
    
-   另外完成 **生物識別**能力整合（如指紋/臉部辨識等）：
    
    -   用於敏感操作的二次驗證或解鎖流程
        
    -   強化安全性與使用體驗的一致性
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->





今天做运营复盘：下面给你一份“最终版”的 **掉粉归因分析 + 解决方案**（结合你前面两轮信息：账号目前偏快讯/中英翻译风格；但你实际是“体育预测市场项目官号”）。

* * *

## **一、掉粉归因分析（按可能性从高到低）**

### **1) 平台清理僵尸/低活跃粉丝（结构性掉粉）**

从 1000 → 500 这种“腰斩式”很符合 X 周期性清理机器人/低质量账号的特征：

-   早期涨的粉如果很大一部分来自互关、抽奖、泛流量、机器人，清理时会集中掉。
    
    **结论**：这部分掉粉你“救不回”，但也不该焦虑——它不代表真实用户变少。
    

### **2) 内容“可替代性过高”，被用户当成搬运号/翻译号取关**

你现在内容形态接近：**JUST IN 快讯 + 中英翻译 + 外链导流**。这会导致两件事：

-   用户感受：信息别处也能看到，而且更快更全 → “关注你没必要”；
    
-   平台判定：模板化、重复度高、像自动化搬运 → 推荐权重下降。
    
    **结论**：你缺少“只有你们项目能给”的价值（赔率解读、市场机制、交易视角）。
    

### **3) 官号“过广告/过导流”，降低分发与留存**

官号天然更容易被用户防御：

-   外链明显、推广感强 → 点赞转发更少；
    
-   X 对带外链内容通常分发更谨慎（尤其频繁时）。
    
    **结论**：即使内容没问题，呈现方式也在“压分发 + 促取关”。
    

### **4) 用户预期错位：你是体育预测市场，但内容偏“政治/泛热点预测”**

截图里出现政治类市场/新闻（例如 Trump/Greenland、地缘冲突等）。

如果你的项目定位是“体育领域预测市场”，长期发泛预测热点会让**体育用户觉得跑题**，而对政治圈又没形成差异化。

**结论**：受众被你“筛走”——留下的也不精准。

* * *

## **二、核心策略：把账号从“快讯搬运号”改造成“体育预测市场的赔率解释器”**

你的官号最强竞争力不是“消息”，而是：

-   你们**上架哪些市场**、
    
-   市场的**概率如何变化**、
    
-   变化背后的**原因与机制**、
    
-   用户如何用它做判断（不等于喊单）。
    

一句话目标：**让关注你=每天获得更会用赔率的能力**。

* * *

## **三、解决方案（可直接执行）**

### **A. 账号层面：先止血（1天内完成）**

1.  **简介与定位**：明确“体育预测市场 + 赔率异动解读 + 复盘”
    
2.  **置顶改成新读者入口线程**（替代“参会/通告/广告”置顶）
    
    结构：你们是谁→看什么栏目→如何开始→风险提示→链接（链接尽量只在置顶出现）
    
3.  **降低外链密度**：
    

-   主推尽量不带外链；
    
-   外链放评论区/置顶线程；
    
-   每天最多 1 条带外链。
    

### **B. 内容层面：内容矩阵（最重要）**

按比例执行（适合体育预测市场官号）：

**1）赛前赔率解读（40%）**

-   “今晚 XX vs XX：市场概率=XX% / 关键变量 3 个 / 反转点 2 个”
    
-   用你们盘口变化证明“你们在做市场”，而不是“在报新闻”。
    

**2）赛中异动解释（20%）**

-   “5分钟从 A→B：常见触发=伤停/阵容/大额成交/临场消息”
    
    重点是“解释机制”，不要做成“喊大家冲”。
    

**3）赛后复盘（30%）**（最容易涨粉/收藏）

-   “市场定价错在哪个变量？”
    
-   “如果只看数据会误判什么？”
    
    复盘越强，越能建立专业信任。
    

**4）产品/活动（10%）**

-   上新、功能、活动要“案例化”：
    
    “为什么开这个盘？怎么玩？这类盘最常见误区是什么？”
    

### **C. 表达形式：避免“机器人感”**

-   **中英双语别堆在同一条**：中文主推 + 英文放回复/第二条。
    
-   少用“JUST IN”模板，改用你们自己的栏目抬头：
    
    《今日概率榜》《赔率异动》《赛后复盘》《新手一分钟》。
    

### **D. 增长动作：恢复推荐的两条“硬操作”**

1.  **每天去 10 个体育大号/记者/数据博主下高质量回复**
    
    用你们赔率变化作为“独家数据点”，容易被路人点进主页。
    
2.  **每条主推结尾设计可回答问题**
    
    让评论区“活起来”，X 会给更多推荐。
    

* * *

## **四、7天“救号计划”（最小可行版本）**

-   Day1：改简介+置顶新手入口线程+停外链轰炸
    
-   Day2：发《今日热门比赛概率榜》一图
    
-   Day3：发《赔率异动解释》
    
-   Day4：发《新手教程：如何读体育预测市场概率》线程
    
-   Day5：发《赛前关键变量清单》+投票
    
-   Day6：发《本周最离谱定价Top5》线程
    
-   Day7：复盘数据：收藏/转发最高的内容类型，下周加倍
    

* * *

## **五、你应该期待的“正确结果”**

-   粉丝数不一定立刻回到 1000（因为清理掉的是虚粉）；
    
-   但如果你做对了，会先出现：
    
    **曝光回升 → 收藏/转发变高 → 主页访问变多 → 粉丝缓慢回涨**，同时转化更精准（真用户）。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->






# **任務 A、B 筆記（Rust CLI / 鏈上事件抓取）**

## **1\. 任務概述**

-   任務：A、B
    
-   形式：以 Rust 實作 CLI（命令列工具）
    
-   目的：抓取鏈上資料 → 整理清理 → 輸出結果
    

## **2\. 主要想法**

-   任務 A 出現一個有趣的實作點：抓取鏈上資料不一定需要 ABI 來讀合約方法
    
-   取而代之的是透過 RPC 的 logs 介面，使用事件過濾（Filter Event / fetch filter event）抓取事件資料
    
-   關鍵依據是事件簽名（Event Signature），搭配合約地址與區塊範圍進行篩選
    

## **3\. 事件抓取方式（核心機制）**

-   事件簽名常數：ORDER\_FILLED\_EVENT\_SIGNATURE = OrderFilled(bytes32,address,address,uint256,uint256,uint256,uint256,uint256)
    
-   抓取流程概念：
    
    -   建立 Filter
        
    -   設定 address = exchange\_address（目標合約地址）
        
    -   設定 event = ORDER\_FILLED\_EVENT\_SIGNATURE
        
    -   設定 from\_block / to\_block
        
    -   透過 provider.get\_logs(filter) 拉取 logs
        
    -   將 logs 交由 process\_logs 解析與轉換
        
    -   最終回傳結構化輸出（Vec）
        
    

## **4\. Pipeline（端到端流程）**

-   Step 1：定位目標合約
    
    -   找到要抓取的合約地址（exchange\_address）
        
    
-   Step 2：抓取事件
    
    -   依合約地址 + 事件簽名 + 區塊範圍建立 Filter
        
    -   取得 logs
        
    
-   Step 3：事件整理與清理
    
    -   logs 解析（將鏈上 log 轉成可用欄位）
        
    -   資料清洗（型別轉換、欄位正規化、可能的去噪/去重）
        
    -   結果映射成輸出資料結構（TradeOutput）
        
    
-   Step 4：輸出
    
    -   CLI 輸出（stdout 或檔案）
        
    -   輸出內容為整理後的結構化資料
        
    

## **5\. 模組規劃（結構定位）**

-   model
    
    -   用途：整理資料模型與輸出結構
        
    -   內容：事件解析後的中間結構、最終輸出結構（如 TradeOutput）、欄位與型別定義、序列化/反序列化相關
        
    
-   utils
    
    -   用途：放置確定性計算（deterministic）
        
    -   內容範圍：單位換算、數值精度處理、固定規則的欄位計算、格式化、校驗等
        
    
-   CLI / main（入口）
    
    -   用途：解析參數、調用 pipeline、控制輸出目的地與格式
        
    

## **6\. 截圖與記錄**

-   Screenshot 2026-01-21 at 9.55.58 PM.png
    

## **7\. Repo**

-   [https://github.com/Duamixu1/OGBC-Intern-Project](https://github.com/Duamixu1/OGBC-Intern-Project)
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->







# 第五章：EVM 架构与 Gas 经济机制

## I. 理论基础：EVM 的定义与属性

> **核心定义**：以太坊虚拟机（EVM）是一个图灵完备（准）的、孤立的、基于栈的虚拟执行环境。它是以太坊协议共识机制的核心，负责处理智能合约的状态转换。

-   **分布式状态机 (Distributed State Machine)**
    
    -   以太坊不仅仅是分布式账本，更是一个**单例状态机 (Singleton State Machine)**。
        
    -   状态转换函数：如果不考虑区块奖励，以太坊的状态转换可形式化为：
        
        $$\\boldsymbol{\\sigma}\_{t+1} \\equiv \\Upsilon(\\boldsymbol{\\sigma}\_t, T)$$
        
        -   $\\boldsymbol{\\sigma}$：世界状态（World State）。
            
        -   $T$：交易（Transaction）。
            
        -   $\\Upsilon$：EVM 状态转换函数。
            
    -   **确定性 (Determinism)**：给定相同的初始状态 $\\boldsymbol{\\sigma}\_t$ 和交易输入 $T$，全网所有节点必须计算出完全一致的 $\\boldsymbol{\\sigma}\_{t+1}$。
        
-   **沙盒环境 (Sandboxed Environment)**
    
    -   EVM 代码在完全隔离的环境中运行。
        
    -   **限制性**：合约代码无法访问网络、文件系统或其他进程，确保了安全性与一致性。
        

## II. EVM 内部架构与运行时环境

> EVM 采用基于栈（Stack-based）的架构，而非寄存器架构，机器字长（Word Size）为 256 位（32 字节）。

-   **易失性数据区 (Volatile Data)**
    
    -   **栈 (Stack)**
        
        -   _功能_：执行操作码（Opcodes）的主要计算区域（如 PUSH, POP, ADD）。
            
        -   _限制_：最大深度为 1024 层。超过限制会导致 `Stack Overflow` 异常。
            
    -   **内存 (Memory)**
        
        -   _功能_：线性可寻址的字节数组，用于存放临时数据（如函数参数、返回值）。
            
        -   _生命周期_：仅在函数调用期间存在，调用结束即清除。
            
        -   _计费_：按二次方增长（扩展内存越大会越昂贵）。
            
-   **持久性数据区 (Persistent Data)**
    
    -   **存储 (Storage)**
        
        -   _功能_：合约的永久性状态数据库，映射为 Merkle Patricia Trie 结构。
            
        -   _特性_：读写成本极高（Gas 消耗大），因为数据需全网同步并永久保存。
            
    -   **代码区 (Code)**
        
        -   _功能_：存储合约编译后的字节码（Bytecode），只读且不可变。
            
-   **特殊数据区**
    
    -   **Calldata**
        
        -   _定义_：存储交易调用参数的只读、字节寻址区域。
            
        -   _特性_：类似于内存，但不可修改且由外部调用者（tx.origin）初始化。
            

## III. Gas 机制：解决停机问题的经济手段

> **理论背景**：根据图灵停机问题（Halting Problem），无法通过静态分析判断程序是否会无限循环。Gas 机制引入了执行成本，强制限制计算步数，防止拒绝服务攻击（DoS）。

-   **Gas 的本质**
    
    -   **计算单位**：Gas 是衡量计算工作量和资源占用的抽象单位，与 ETH 市场价格解耦。
        
    -   计价公式：
        
        $$Transaction\\ Fee = Gas\\ Used \\times Gas\\ Price$$
        
-   **Gas 消耗层级 (Gas Schedule)**
    
    -   **基础操作**：`ADD`, `SUB` 等算术运算消耗极低（约 3-5 Gas）。
        
    -   **环境操作**：`BALANCE`, `EXTCODESIZE` 等涉及状态读取的操作较贵（数百 Gas）。
        
    -   **状态修改**：`SSTORE`（写入存储）最为昂贵（可达 20,000+ Gas），因其永久占用链上资源。
        
-   **执行限制**
    
    -   **Gas Limit (交易级)**：用户授权的最大消耗量。耗尽将导致 `Out of Gas (OOG)` 异常。
        
    -   **Block Gas Limit (区块级)**：单个区块容纳的总 Gas 上限，决定了网络的吞吐量（TPS）。
        

## IV. 费率市场改革：EIP-1559

> 2021 年伦敦升级引入 EIP-1559，将费率机制从“首价拍卖”转变为“基础费+小费”模式。

-   **费用结构**
    
    -   **Base Fee (基础费)**：
        
        -   由协议算法根据上一区块利用率自动调整。
            
        -   **通缩机制**：该部分 ETH 被**销毁 (Burn)**，从而实现 ETH 供应量的动态调节。
            
    -   **Priority Fee (小费)**：
        
        -   用户直接支付给验证者（Validator）的激励费用，用于在拥堵时获得打包优先权。
            
-   **弹性区块大小**
    
    -   区块大小不再固定，而是围绕“目标 Gas 使用量”（Target Gas Usage）波动，最高可达目标的 2 倍，以应对突发流量。
        

## V. 交易原子性与异常处理

> **原子性 (Atomicity)**：以太坊交易遵循 ACID 原则中的原子性，即“全有或全无”（All-or-Nothing）。

-   **成功执行**：状态更新被提交，Gas 费用由发送方支付。
    
-   **执行失败 (Revert)**
    
    -   **场景**：代码抛出异常、Gas 耗尽（OOG）、违反断言（Assert）。
        
    -   **状态回滚**：所有在交易期间对状态（Storage, Balance）的修改全部撤销，恢复至交易前状态。
        
    -   **费用结算**：
        
        -   若因代码逻辑错误（如 `require` 失败）：仅消耗执行至错误处的 Gas。
            
        -   若因 Gas 耗尽：消耗全部设置的 Gas Limit。
            

## VI. 工程实践：Gas 优化策略与案例

> 在 Solidity 开发中，优化 Gas 消耗不仅是成本控制，更是衡量代码质量的关键指标。

-   **策略一：存储布局优化 (Storage Packing)**
    
    -   _原理_：EVM 每次读取 32 字节（Slot）。如果多个变量能塞入一个 Slot，可减少读取次数。
        
    -   _案例分析_：
        
        -   **低效写法**：
            
            Solidity
            
            ```
            uint128 a;
            uint256 b;
            uint128 c; // 需要 3 个 Slot，因为 b 无法与 a 共享
            ```
            
        -   **优化写法**：
            
            Solidity
            
            ```
            uint128 a;
            uint128 c; // a 和 c 共用 1 个 Slot
            uint256 b; // 占用 1 个 Slot，总计 2 个 Slot
            ```
            
-   **策略二：数据位置选择 (Calldata vs Memory)**
    
    -   _原理_：对于外部函数（External Function）的只读引用类型参数，使用 `calldata` 避免了数据复制成本。
        
    -   _对比_：
        
        -   `memory`：将参数从调用数据区复制到内存（消耗 Gas）。
            
        -   `calldata`：直接读取指针，零拷贝（省 Gas）。
            
-   **策略三：缓存机制 (Caching)**
    
    -   _原理_：`SLOAD`（读存储）远比 `MLOAD`（读内存）昂贵。在循环中应避免反复读取状态变量。
        
    -   _案例分析_：
        
        -   **高耗能代码**：
            
            Solidity
            
            ```
            uint public total;
            function update() external {
                for(uint i=0; i<10; i++) {
                    total += 1; // 循环 10 次，执行 10 次 SLOAD 和 SSTORE
                }
            }
            ```
            
        -   **优化代码**：
            
            Solidity
            
            ```
            function update() external {
                uint temp = total; // 1 次 SLOAD
                for(uint i=0; i<10; i++) {
                    temp += 1; // 内存操作，极其廉价
                }
                total = temp; // 1 次 SSTORE
            }
            ```
            
-   **策略四：利用不可变变量**
    
    -   使用 `constant` 和 `immutable` 关键字。这些变量在编译时直接嵌入字节码，运行时无需访问 Storage，大幅降低 Gas。
        

## VII. 总结与展望

-   **核心地位**：EVM 确立了去中心化计算的标准，通过确定性执行保证了全网共识。
    
-   **经济闭环**：Gas 机制有效防御了资源滥用，并通过 EIP-1559 建立了 ETH 的价值捕获机制。
    
-   **演进方向**：
    
    -   **Layer 2 扩容**：通过 Rollup 技术将计算移至链下，仅将数据可用性留存链上。
        
    -   **EOF (EVM Object Format)**：即将到来的升级，旨在规范字节码格式，提高解析效率与安全性。
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->









My First ZKVote

完成之后的目的

-   自己完整做一遍，验证理解是否一致
    
-   把“名词—流程—安全性保证”串起来
    
-   记录仍然不确定的点，后续集中补齐
    

核心疑问起点

-   文档描述：每个选民在本地生成随机秘密 identitySecret
    
-   使用方式：nullifier = Hash(identitySecret, electionId)
    
-   表面担忧：如果每次投票都重新生成 identitySecret，则 nullifier 会变化，commitment 也会变化，看起来无法阻止重复投票
    

关键澄清

-   identitySecret 在常见 ZK 身份/投票系统里通常不是“每次投票随机生成”，而是“身份层面的长期秘密”
    
-   随机生成只发生在身份创建阶段，后续在同一身份生命周期内保持不变
    
-   重复投票拦截依赖的是“同一身份在同一 electionId 下生成同一个 nullifier”，而不是依赖地址身份
    

典型流程（从资格到投票）

1.  身份创建（本地）
    

-   生成 identitySecret（长期保存，不上链）
    
-   计算 identityCommitment = Hash(identitySecret) 或 Hash(identityPublicKey)（用于公开登记）
    

2.  注册/加入选民集合（链上或链下准入后上链）
    

-   identityCommitment 被加入成员集合（常见形式是 Merkle Tree 的叶子）
    
-   合约保存最新 merkleRoot，代表当前有效选民集合
    

3.  投票（链上验证 + 零知识证明）
    

-   计算 nullifier = Hash(identitySecret, electionId)
    
-   提交 proof，证明包含两类断言
    
    -   成员资格：某个 commitment 属于 merkleRoot（但不泄露是哪一个）
        
    -   防重：nullifier 正确由该身份与 electionId 导出（并可被合约记录）
        
    
-   合约记录 nullifier 已使用，后续相同 nullifier 的投票直接拒绝
    

对“换 identitySecret 重投”的结论

-   如果真的更换 identitySecret，相当于创建了一个新身份
    
-   想用新身份再次投票，必须让新 identityCommitment 进入选民集合（加入 Merkle Tree）
    
-   因此防重复投票的边界条件不在“能否生成新 secret”，而在“是否能重复加入选民集合”
    
-   若注册阶段是受控准入（allowlist、签名授权、KYC、一次性凭证、资格 token 等），则可限制“一人一身份”或“一人一次加入”
    

为什么不依赖 msg.sender

-   地址是可关联、可追踪的标识，绑定资格会削弱匿名性
    
-   ZKVote 的目标通常是：资格验证与投票行为不与链上地址直接绑定
    
-   地址更多承担交易发起与 gas 支付角色，不承担“选民是谁”的判定
    

安全性保证拆解（系统到底保证了什么）

-   匿名性：证明成员资格但不公开具体成员是谁
    
-   防重复：同一身份在同一 electionId 下生成唯一 nullifier，合约用 nullifier set 阻止二次提交
    
-   完整性：proof 验证通过才计票，未在集合内的身份无法生成有效证明
    
-   可审计性：链上可公开验证 proof 与 nullifier 是否重复，但无法反推出身份
    

名词速记（统一口径）

-   identitySecret：身份私密材料，长期持有
    
-   identityCommitment：对身份的承诺值，可公开登记
    
-   Merkle Tree / Merkle Root：成员集合的压缩表示，root 作为集合快照
    
-   Merkle Path：证明 membership 的路径证据（作为 witness 的一部分）
    
-   nullifier：与 electionId 绑定的防重标识，公开但不可反推身份
    
-   witness：生成证明时的私密输入（secret、路径、投票值等）
    
-   verifier：链上验证 proof 的逻辑/合约
    

与 MPC / FHE 的差异思路

-   ZK（此处）：重点在“可验证但不泄露”，证明某事为真而不暴露细节
    
-   MPC：重点在“多方共同计算，任何单方都看不到完整输入”
    
-   FHE：重点在“对密文直接计算，计算过程数据全程保密”
    
-   FHE + ZK：常见组合目标是“隐私计算 + 可验证正确性”，FHE 负责隐私，ZK 负责证明计算按规则执行且结果正确
    
-   选型直觉：
    
    -   需要公开可验证且不泄露输入 → ZK 强项
        
    -   需要多机构联合算且互不信任 → MPC 强项
        
    -   需要把数据交给外部计算但不想暴露 → FHE 强项
        
    -   需要外包隐私计算同时还要第三方可验证 → FHE + ZK 常见
        
    

待补齐问题清单

-   注册阶段的准入模型：如何实现“一人只能加入一次”，以及如何处理身份遗失/更换
    
-   nullifier 的哈希域分离与抗碰撞实践：Hash 选择、域分离 tag、electionId 编码规则
    
-   投票内容是否加密：是否需要“匿名 + 保密投票内容”的双重需求（可能涉及 commit-reveal、加密计票或 FHE）
    
-   成员撤销与 root 更新：新 root 产生后旧 proof 是否仍有效，如何处理投票期内集合变化
    

Ethernaut 1~3

总体记录方式

-   目标：拿到通关条件（通常是改变合约关键状态或触发某个验证条件）
    
-   观察：先看权限、状态变量、关键入口（fallback/receive/constructor/initializer）
    
-   利用点：找不符合预期的状态转移或权限绕过
    
-   步骤：最短路径从“可控输入”到“关键状态变化”
    
-   结果：验证通关条件达成，截图留证
    

Level 1

-   难度感受：跟着引导走即可
    
-   关键动作：拿到密码后调用认证逻辑完成验证
    
-   结果：通关（已截图记录）
    

Level 2（Fallback）

目标

-   取得 owner 身份后执行提现逻辑
    

关键观察

-   提现函数受 onlyOwner 限制，因此核心任务是成为 owner
    
-   存在一个可改变 owner 的入口在 receive 路径上
    
-   receive 的前置条件要求：发送金额大于 0 且 contributions 记录大于 0
    
-   contributions 记录可通过 contribute 以极小金额建立
    

利用链路（因果关系）

-   先让 contributions 对当前地址为正 → 满足 receive 的第二个条件
    
-   再发送一笔普通转账触发 receive → owner 被改为当前地址
    
-   owner 到手后调用提现 → 资金转出
    

要点总结

-   这关的关键不是金额大小，而是“状态变量 owner 在 receive 中被不安全地赋值”
    
-   只要能满足 receive 的条件，就能完成权限接管
    
-   常见防御方向是：不要在被动收款入口中变更关键权限；或把 owner 变更绑定显式授权流程
    

结果

-   成功夺取 owner 并提现（已截图记录）
    

Level 3（Fallout）

目标

-   成为 owner
    

关键观察

-   构造函数命名/写法错误导致初始化函数未在部署时执行
    
-   原本应只在部署阶段运行的初始化逻辑，变成可被外部直接调用的普通函数
    
-   直接调用该初始化函数即可设置 owner 为调用者
    

要点总结

-   典型初始化漏洞：constructor 拼写/版本语法错误，或 initializer 未加保护（只执行一次）
    
-   审计重点：所有初始化入口是否具备一次性与权限控制
    

结果

-   成功拿到 owner（已截图记录）
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->










這段筆記我幫你「補齊背景＋講清楚做法＋把思路寫得更像可複用的解題模板」，你可以直接貼進你的學習筆記裡。

* * *

## **ETH / Smart Contract 測試題筆記拓展：讓**

## **ONLY ONE**

## **functional test 失敗**

### **任務描述**

題目的目標非常明確：

> 修改 source code，使得 **只有一個 functional test 會 FAIL**（其他全部仍然 PASS）。

這種題型的本質不是在「修 bug」，而是在考你對 **測試覆蓋範圍 / 依賴關係 / 合約邏輯邊界** 的理解：

你要能精準控制「改動的爆炸半徑（blast radius）」。

* * *

## **解題總策略：找「最邊緣」的改動點**

核心原則一句話：

**越邊緣的地方越安全，越不會連鎖影響其他測試。**

因為 functional tests 通常會大量覆蓋核心流程（存取款、權限、狀態遷移、事件、revert 條件…），你動核心邏輯，幾乎必定是一串測試一起爆。

所以策略是：

1.  **不要碰核心邏輯（core logic）**
    
    -   例如：資金流、狀態轉移、權限控制、重要 require / revert、關鍵計算（reward、interest、share price）。
        
2.  **優先動“介面層/輸出層”（interface / view / getter / metadata）**
    
    -   尤其是一些 view、pure、或合約的「資訊回報」類函數：
        
        -   name() / symbol()
            
        -   version()
            
        -   getUptime() / uptime()
            
        -   decimals()（也常見，但有時會被很多測試依賴，要小心）
            
3.  **鎖定一個“只被單一測試用到”的點**
    
    -   目標是找到一個功能或數值：**只有某個測試在 assert 它**，其他測試完全不關心。
        

* * *

## **操作流程（可複用模板）**

我做題時的思路是這樣走：

### **Step 1：先跑一次測試，熟悉測試結構**

先確認 baseline：所有測試都應該先 PASS（或至少你知道原本期望是什麼）。

### **Step 2：定位「低耦合」區域**

我會優先找：

-   合約中不影響狀態的函數（view/pure）
    
-   “資訊性欄位”或回傳值
    
-   不會被其他函數再利用的 getter
    

因為這類變動通常只會影響 **assert 回傳值** 的那條測試。

### **Step 3：挑一個最容易控制只炸一個測試的點**

這題答案其實很多（改字串、改常數、改某個不重要的 return 值…都可能成功）。

我最後選了一個非常典型的「邊緣改動點」：**Uptime**。

* * *

## **我選的改法：把 Uptime 改成 0**

我隨便挑了一個回傳 uptime 的地方（大概率是某個 getter 或 interface function），把它改成：

-   原本：回傳正常 uptime（可能是 block timestamp 差值、或某個常數）
    
-   我改：直接回傳 0
    

為什麼這招常常好用？

-   uptime 多半屬於「監控/資訊」用途
    
-   很多核心流程不會依賴它
    
-   但測試裡可能會有一條專門檢查它「不應為 0 / 應大於某值」
    

所以它很容易做到：

-   **只有那條 assert uptime 的測試 FAIL**
    
-   其他功能測試依然 PASS
    

* * *

## **結果驗證**

最後就是標準收尾流程：

1.  **重新編譯（compile）**
    
2.  **重新跑全部測試（run tests）**
    
3.  觀察結果：
    
    ✅ 其他全部 PASS
    
    ❌ 只有 1 條 functional test FAIL
    

Boom，Bingo，任務完成。

* * *

## **小結：這題真正想訓練的能力**

這題其實是在逼你建立一個工程直覺：

-   **測試 ≈ 規格**
    
-   修改程式碼不是只有「能跑就好」，而是要能預測「會影響哪些行為」
    
-   最實用的心法是：
    
    **控制改動半徑**（改越偏、越小、越可控越好
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->












## **第一章：一个平台——“世界计算机”的操作系统**

以太坊的核心不是“转账网络”，而是一套**可编程的状态机（state machine）**：

所有节点对同一组交易执行同一套规则，得到同一个全局状态。

### **1）EVM：确定性执行环境**

-   **EVM（Ethereum Virtual Machine）**是统一的执行规范：给定同样的输入与状态，必须得到同样的输出（确定性）。
    
-   合约运行不是“跑在某台服务器”，而是“在所有验证节点上被重复执行并达成一致”。
    

### **2）状态：这台计算机到底“记住了什么”**

以太坊维护的是“全局状态”，主要包括：

-   账户余额（ETH）
    
-   账户 nonce（防重放、防重复交易）
    
-   合约代码（合约账户才有）
    
-   合约存储（storage：持久化状态变量）
    
-   还有一些执行产生的日志（logs / events，用于链上可检索的“行为痕迹”）
    

### **3）Gas：资源计价与抗滥用机制**

-   Gas 本质是**计算与存储资源的计量单位**：不同指令/操作有不同的Gas成本。
    
-   交易要设置 gas limit、gas price（或 EIP-1559 的 max fee / priority fee），确保网络资源可被市场化分配。
    
-   Gas 解决两件事：
    
    1.  防止无限循环/恶意占用资源；2) 让稀缺资源有价格信号。
        
    

### **4）ETH：燃料 + 安全抵押品**

-   **燃料**：支付交易与合约执行费用（Gas 以 ETH 结算）。
    
-   **安全**：在 PoS 下，ETH 也是验证者的质押资产，作恶会被惩罚（slash/罚没或损失收益）。
    

### **5）共识：从 PoW 到 PoS（定位变化）**

-   以太坊已从 PoW 切换为 PoS：安全性来自**经济抵押与惩罚**，而非算力竞争。
    
-   PoS 下可以更自然地支持：更稳定的出块节奏、验证者委员会、最终性（finality）等机制概念。
    

* * *

## **第二章：一套网络——全球节点如何“像一台机器一样工作”**

这台“世界计算机”之所以能成立，是因为它是一套**分布式系统**：节点靠网络传播信息、靠共识对“区块/交易顺序”达成一致。

### **1）节点分工：你提到的三类可以再补一层“职责”**

-   **全节点（Full Node）**：验证区块与交易、维护最新状态；是去中心化的“基本盘”。
    
-   **轻节点（Light Node）**：不下载全部数据，通过验证头部与证明来“信任最小化”地查询信息（更省资源）。
    
-   **归档节点（Archive Node）**：保存所有历史状态，便于历史查询/分析/区块浏览器等，但成本最高。
    

> 实务提示：大多数 DApp 前端并不直接连轻节点，而是连 RPC 服务（自建或第三方），这在“去中心化程度”和“工程可用性”之间做了权衡。

### **2）P2P + Gossip：信息如何扩散**

-   交易与区块通常通过 **Gossip** 在P2P网络中扩散：节点把收到的信息转发给邻居，最终覆盖全网。
    
-   这带来经典分布式权衡：
    
    去中心化（任何人可入网）↔ 延迟（传播速度）↔ 带宽（冗余转发）↔ 抗审查（没有中心开关）
    

### **3）两条“数据流”**

-   **交易流**：用户签名交易 → 广播 → 进入各节点的内存池（mempool） → 被打包进区块
    
-   **区块流**：出块者发布区块 → 全网验证 → 链头推进 → 最终性增强
    

* * *

## **第三章：两类账户——所有交互的“身份与入口”**

以太坊不是“地址=人”，而是“地址=账户对象”。账户决定了你能做什么、状态如何保存、权限如何定义。

### **1）EOA（外部账户）：由私钥控制的行动者**

-   关键特征：**能发起交易**（因为能签名）。
    
-   核心风险：私钥即资产与权限；丢失/泄露=不可逆损失。
    
-   nonce 的意义：
    
    -   防止交易重放
        
    -   也决定交易的“顺序一致性”（同一 EOA 的交易按 nonce 递增执行）
        
    

### **2）合约账户：由代码控制的自动体**

-   关键特征：**不能“凭空发起交易”**，只能被交易或其他合约调用而触发执行。
    
-   合约包含两部分：
    
    -   code（字节码逻辑）
        
    -   storage（持久化状态，读写成本高）
        
    

### **3）交易与调用：两种“触发方式”**

-   **交易（Transaction）**：EOA → 网络层广播 → 进入区块后执行并改变状态
    
-   **消息调用（Message Call）**：合约内部的调用（合约A调用合约B），发生在一次交易执行过程里，不单独上链成“交易”，但会影响状态与产生日志
    

* * *

## **第四章：一种程序——智能合约作为“公开可验证的自动化规则”**

智能合约是以太坊“可编程”的落点：把规则写进链上，让它在共识下自动执行。

### **1）开发与部署链路（工程视角更清晰）**

-   Solidity/Vyper 等高级语言编写
    
-   编译得到：
    
    -   **Bytecode**：部署到链上的机器码
        
    -   **ABI**：前端/外部与合约交互的接口说明（“怎么调用它”）
        
    
-   部署后获得合约地址，成为全网可调用的“公共组件”
    

### **2）不可更改≠不能升级：升级是“架构选择”**

你说的“一旦部署通常不可更改”是对的，但实践中常见两种路线：

-   **不可升级合约**：逻辑固定，简单透明，但发现漏洞就只能迁移。
    
-   **可升级模式**（如代理合约 Proxy）：地址保持不变，通过代理指向新逻辑合约升级。
    
    代价是复杂度与信任假设上升（谁有升级权限？治理怎么做？）
    

### **3）事件（Events）与可观测性**

-   合约执行不仅“改状态”，也会发出 **logs/events**，便于链下系统（前端、索引器、分析平台）监听与检索。
    
-   这形成链上链下协作：链上负责可信执行，链下负责高效检索与复杂展示。
    

### **4）安全与边界：合约是“自动执行”，也是“自动承担后果”**

-   合约漏洞往往不可逆：重入、权限配置、整数处理、预言机依赖、升级权限滥用等。
    
-   以太坊提供的是“可信执行环境”，不保证你写的逻辑“正确”。
    

* * *
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->













## **1) 智能合约 vs dApps：不是“大小”区别，而是“层次”区别**

### **智能合约（Smart Contract）**

**更精确的定义：**

智能合约是运行在区块链虚拟机（如 EVM）上的**确定性程序**，拥有：

-   **代码（code）**：部署后通常不可变（除非用代理/升级模式实现“可升级”）
    
-   **持久状态（state）**：合约地址名下的存储（storage），记录余额、权限、配置等
    
-   **可验证执行（verifiable execution）**：每个节点在同样输入下得到同样输出，才能形成共识
    

**核心特征不是“如果…那么…”**，而是：

-   **可强制执行的规则**：写进链上后，不靠平台信用、靠全网共识强制执行
    
-   **可组合（composability）**：合约可以调用合约，形成“乐高式金融/协议堆栈”（DeFi 之所以爆发的底层原因之一）
    
-   **可审计**：状态与交易可公开验证（但这也带来隐私与 MEV 问题）
    

> 常见误区：把合约当“自动化脚本”。其实合约更像**公共规则引擎 + 公共账本接口**，脚本只是表象。

* * *

### **dApp（Decentralized Application）**

**更精确的定义：**

dApp 是一个“产品形态”，通常由多层拼装：

-   **链上：合约层**（后端规则/资产托管/结算）
    
-   **链下：交互层**（前端 Web/移动端、钱包、签名、索引服务）
    
-   **链下：数据与可用性层**（RPC 节点、Indexer、The Graph、缓存、数据库）
    
-   **链下：现实世界输入层**（Oracle，如价格、随机数、身份、事件）
    

所以 dApp 的“去中心化程度”不是非黑即白，而是一个光谱：

-   合约开源、不可升级、无管理员密钥 → **更强抗审查/更可信**
    
-   前端托管在中心化服务器、依赖单一 RPC、管理员可暂停 → **更像“链上结算 + Web2 外壳”**
    

**一句话区分：**

-   智能合约：**链上可验证的规则与状态**
    
-   dApp：**围绕这些规则与状态做成可用的产品系统（含大量链下组件）**
    

* * *

## **2) Token 如何产生与管理：本质是“状态机里的记账标准”**

你写的“合约存储空间里的账本”很对，可以再深化为三层：

### **(1) Token 的本质：合约状态 + 规则**

Token 不是“链上真的有一堆币”，而是合约里维护一个状态机，例如：

-   balanceOf\[address\]：余额映射
    
-   allowance\[owner\]\[spender\]：授权额度
    
-   totalSupply：总量
    

交易（转账/铸造/销毁）就是**状态转移**。

### **(2) 标准化：Token 为什么能“通用”**

Token 能被钱包、交易所、DeFi 协议识别，靠的是接口标准：

-   **ERC-20**：同质化代币（可分割、互换）
    
-   **ERC-721**：NFT（唯一）
    
-   **ERC-1155**：多资产/半同质化（游戏道具常用）
    

标准化带来生态“可组合”：你的 Token 一旦符合标准，就能被一堆协议“即插即用”。

### **(3) 价值从哪来：不是“写出来就有价值”**

合约能“铸造余额”，但价值来自链外与链内共同形成的共识：

-   **效用**：治理权、手续费折扣、抵押品、访问权、积分权益等
    
-   **稀缺性与规则可信度**：发行规则能否被随意改？是否可增发？谁能增发？
    
-   **流动性**：能否在 DEX/CEX 交易、是否有做市深度
    
-   **风险定价**：智能合约风险、预言机风险、治理攻击风险
    

### **Gas 的位置：为什么“必须花 ETH”**

Gas 不是为了“让你付钱”，而是为了**把计算/存储变成稀缺资源**，避免滥用，并补偿节点执行与带宽成本。

因此 Token 的转账、mint、复杂交互都会消耗 Gas（以 ETH 计价）。

* * *

## **3) PoS 机制原理：把“记账权”变成可惩罚的抵押合约**

你写的“质押越多越容易被选中”对，但 PoS 的关键不是抽签，而是**经济安全模型**：

### **(1) 两类角色动作**

-   **提议（Propose）**：某个 slot 被选中者提出新区块
    
-   **投票/见证（Attest）**：其他验证者对“哪条链是头部 + 哪些区块应最终确定”投票
    

### **(2) 安全性的关键：可罚没（slashing）+ 可恢复（finality）**

PoS 的本质是：

> 让攻击成本从“买算力+电费”变成“抵押资产被系统销毁”，并且攻击后资产难以撤退。

最终性（finality）让链在一定条件下不可逆，攻击者若想回滚最终确定的区块，需要付出极高的罚没代价与协调成本。

* * *

## **4) PoW vs PoS 的安全性来源：攻击成本“付费方式”不同**

### **PoW：物理世界成本**

-   安全性来自**算力与能源**
    
-   51% 攻击需要持续投入巨额算力与电费，并承受硬件折旧与外部监管风险
    

### **PoS：金融世界成本（可罚没、可追溯）**

-   安全性来自**可惩罚抵押**（slashing/离线罚没）
    
-   进攻者要控制共识，需要锁定大量资产，且作恶会导致资产销毁
    
-   额外优势：攻击者“逃跑”更难（链上身份与证据可验证）
    

> 你可以用一句类比总结：

> PoW 是“用电费买安全”，PoS 是“用保证金买安全”。

* * *

## **5) Dencun / Proto-Danksharding / Danksharding：核心是 DA（数据可用性）路线**

你这段已经很完整了，我帮你把“为什么 blob 能降费”讲透：

### **Dencun 升级（Deneb + Cancun）**

关键影响可以概括为一句话：

**把 Rollup 的“发数据到 L1 的方式”从昂贵的 calldata，迁移到更便宜、更专用的 blob 数据通道。**

### **Proto-Danksharding（EIP-4844）的技术抓手：Blob 交易**

-   Rollup 需要把压缩后的交易数据发布到 L1，才能让任何人重放并验证 L2 状态（这是“DA”的核心）
    
-   以前主要塞进执行层的 calldata：**贵、和执行资源竞争**
    
-   Blob：为 DA 设计的“数据载体”，与执行层资源**部分解耦**，形成独立费用市场，从而显著降低 Rollup 发布数据成本
    

### **Danksharding（完整形态）**

目标不是“让 L1 处理更多交易”，而是：

**让 L1 提供更大规模、可验证的数据可用性带宽**，从而支撑更多 Rollup 扩容。

所以它服务的是“Rollup-centric roadmap”。

* * *

## **6) Rollup 机制：把区块链拆成“执行层”和“结算/DA层”的分工体系**

你写的“职责分离”非常关键，可以再补上一个“可信边界”视角：

### **Rollup 在做什么**

-   **L2 执行**：在链下/二层高频执行交易，追求吞吐与低成本
    
-   **L1 结算 + DA**：L1 保存关键数据并提供最终安全性，让任何人可以挑战或验证 L2 的正确性
    

### **两种主流验证范式**

-   **Optimistic Rollup**：默认正确，靠挑战窗口+欺诈证明兜底
    
    -   风险点：挑战机制是否可用（审查、DoS、证明复杂度）
        
    
-   **ZK Rollup**：每批状态更新附带有效性证明
    
    -   优势：更快确定性（不依赖挑战窗口）
        
    -   难点：证明系统工程复杂、成本结构不同
        
    

> 一句话总结：

> OR 用“可挑战”保证正确，ZK 用“可证明”保证正确。

* * *

## **7) ETH 的核心作用：把“资源、激励、安全、价值锚”统一到一个资产上**

你列得很全，我帮你归为四大类，逻辑更清晰：

1.  **资源计价（Resource Pricing）**
    

-   Gas：交易与计算资源的价格机制
    

2.  **安全抵押（Economic Security）**
    

-   Staking 作为保证金
    
-   通过奖励与惩罚调节网络行为（在线率、诚实投票、反作恶）
    

3.  **协议内结算资产（Settlement Asset）**
    

-   DeFi 抵押品与流动性锚（很多协议默认以 ETH/ETH衍生品计价或作为基础抵押）
    

4.  **价值载体（Store of Value / Monetary Premium）**
    

-   市场共识形成的价值储存属性（这部分更偏经济叙事与市场行为）
    

* * *

## **8) 验证者的工作：不是“开机挖矿”，而是“在线参与共识 + 运维安全工程”**

你笔记里有图（我看不到），我给你补一份“文字版职责清单”：

### **日常职责**

-   运行共识客户端 + 执行客户端（并保持同步）
    
-   按要求参与：
    
    -   slot 被选中时提议区块
        
    -   大多数时间进行 attestation 投票
        
    -   可能参与同步委员会相关任务（视协议安排）
        
    
-   维护高在线率与低延迟（否则有离线罚没/机会成本）
    

### **安全工程职责（更重要但常被忽略）**

-   私钥管理：签名密钥与提款密钥隔离、硬件/离线备份
    
-   防止双签：避免同一验证者密钥在两台机器同时运行（典型 slashing 来源）
    
-   监控：客户端版本、链分叉、磁盘、网络、异常投票提示
    

* * *

## **9) Slashing：罚的是“破坏共识安全的可证明恶意行为”，不是“偶尔掉线”**

你对三类典型 slashing 行为解释得很好，我帮你加上两点“机制层理解”：

### **(1) Slashing 的判定特征**

-   必须是**链上可验证的矛盾签名证据**（例如同一 slot 的双提议、同高双投票、环绕投票）
    
-   因此它不是“主观判断你坏不坏”，而是“你签名证据自证矛盾”
    

### **(2) 罚没结构（建议写成“原则 + 近似值”，避免死记数字）**

-   **立即罚没**：先扣一部分有效余额
    
-   **强制退出活跃集**：进入较长的退出/惩罚期（常见量级是“数周”）
    
-   **相关性惩罚**：同一时间作恶者越多，罚得越狠（防串谋攻击）
    
-   **举报奖励**：鼓励网络监督（让作恶更难隐藏）
    

补充你写得很重要：短时间离线通常是**离线罚没/损失收益**，不等于 slashing。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->














## **课堂后反思笔记：Web3的“去中心化体验”与合规现实**

### **1) 认知转变：从“自主掌控”到“合规介入”**

-   **最初感受**：认为Web3更自由、限制更少。
    
-   **资料学习后的修正**：现实中的Web3并非“法外之地”，尤其在\*\*资金进出（on/off-ramp）**与**可监管主体（VASP/CEX）环节，合规约束极强。
    

* * *

### **2) 现象观察：DEX的“无人值守”只发生在链上局部**

-   **使用场景**：在 Uniswap 等 **DEX** 上交易时，表面上像“无人值守”，但：
    
    -   **法币入金通道**常常需要对接**中心化交易所/合规支付机构**，从而触发严格监管要求（KYC/AML）。
        
    
-   **KYC摩擦的具体表现**：
    
    -   常规KYC：身份证件上传、地址/身份信息核验等。
        
    -   强化KYC（部分辖区）：实时自拍/活体检测、地理位置校验等（例如印度对加密服务提供商的强化KYC要求出现“活体自拍+定位”等做法）。
        
    
-   **结论**：所谓“去中心化”的顺滑体验，**在起点（入金）与终点（出金/法币结算）往往仍被传统合规抓手牢牢控制**。
    

* * *

### **3) 核心机制：Travel Rule让“完全匿名”在合规体系内很难成立**

-   **Travel Rule的含义**：当转账超过一定门槛，服务提供商需**收集并传递付款人与收款人信息**，使交易具备可追踪的“随行数据”。这源自FATF关于支付透明度/电汇信息要求（Recommendation 16），并持续被更新与强化。
    
-   **直接影响**：用户对“完全匿名”的想象会被现实校正——**链上可见性 + 合规主体的数据留存与交换**，共同塑造了可追溯性。
    

* * *

### **4) 关键理解：合规是正规Web3服务的“硬成本”，并直接塑造用户体验**

-   **合规不再是“附加项”**：对多数面向大众的Web3服务而言，KYC/AML、制裁筛查、交易监测、可疑报告、旅行规则对接等是进入主流市场的门槛。
    
-   **产品层面的含义**：
    
    -   只要创新涉及**用户资金流转**，“了解你的客户（KYC）/反洗钱（AML）”就必须在**架构设计之初**纳入核心模块，而不是上线后补丁。
        
    

* * *

### **5) 行业阶段判断：从“颠覆/躲避”走向“合规/融合”**

-   **阶段变化**：Web3行业正在从强调“颠覆”“规避”的草莽阶段，转向寻求“合规化”“制度化对接”的建设阶段。
    
-   **伴随心理落差**：早期进入者可能会沮丧——这不再是想象中的乌托邦；但从“非刚需”走向“逐渐成为刚需”的过程中，**国家监管介入几乎不可避免**。
    

* * *

### **6) 全球监管分化：辖区选择成为“战略大于技术”的决策**

-   **分化格局（简化分类）**
    
    1.  **“拥抱派”**：尝试把创新纳入体系——例如美国围绕稳定币建立联邦层面的框架（GENIUS Act 讨论/立法路径即属此类）。
        
    2.  **“规范派”**：以统一牌照与全面规则建立市场秩序——欧盟 **MiCA** 提供了覆盖加密资产服务与稳定币等的系统性监管框架，并由欧盟监管机构推进实施细则。
        
    3.  **“出清派”**：以直接禁止或强限制方式压缩市场空间（不同国家/地区实践差异很大）。
        
    
-   **推论**：Web3项目若要长期生存，必须把“在哪个司法辖区运营、拿什么牌照、如何满足监管接口”作为**第一优先级战略问题**；很多时候**合规风险 > 技术风险**。
    

* * *

### **7) 风险提示：代码“不可阻挡”不等于法律“不可触达”**

-   **现实冲突点**：智能合约的“不可阻挡”吸引人，但在法律语境下，若项目机制涉及：
    
    -   抽奖/博彩结构
        
    -   层级返佣/拉人头结构
        
        可能在部分国家被识别为**赌博或传销**，从合规处罚升级为**刑事风险**（即便合约还能运行，团队与相关主体仍可能承担责任）。
        
    
-   **结论**：面向大众用户的Web3项目核心竞争力，可能越来越体现在：
    
    -   清晰的合规架构
        
    -   完整的KYC/AML与交易监测体系
        
    -   与传统金融系统对接的合法桥梁（银行、支付、托管、牌照等）
        
    

* * *

## **关键词速记**

-   **DEX/CEX**：去中心化交易所 / 中心化交易所
    
-   **On/Off-ramp**：法币入金/出金通道
    
-   **KYC/AML**：客户身份识别 / 反洗钱
    
-   **Travel Rule（FATF R.16）**：大额转账信息随行与传递要求
    
-   **MiCA（欧盟）**：欧盟统一加密资产监管框架
    
-   **GENIUS Act（美国）**：围绕支付稳定币的联邦框架尝试
    

* * *

## **对“未来参与/创业”的行动要点**

-   把 **KYC/AML/制裁合规/旅行规则对接**当作“从0到1”的核心工程，而不是上线后的运营补丁。
    
-   先做“辖区—牌照—业务边界”设计，再做技术选型；必要时按不同地区做产品与合规拆分。
    
-   在机制设计阶段做法律风险评估（博彩/传销/证券属性/消费者保护），避免把“创新结构”做成“高风险结构”。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->















-   上期回顾
    
    -   比特币：《比特币：一种点对点电子货币系统》
        
    -   目标：解决 Web2 贸易“高度依赖中介信任”的问题
        
    -   延伸：区块链技术 + 去中心化理念 ≠ 只用于记账，可能性更大
        
    
-   2014：以太坊的出现
    
    -   Vitalik Buterin：构建“下一代加密货币与去中心化应用平台”
        
    -   Ethereum（以太坊）：让区块链从“记账”变成“可运行程序/应用的平台”
        
    -   EVM：图灵完备的虚拟机，可执行智能合约代码
        
    
-   名称与核心创新
    
    -   “Ethereum”：假设存在的、遍布宇宙并允许光传播的不可见介质
        
    -   核心创新：智能合约（Smart Contracts）
        
        -   本质：存储在区块链上的可执行代码
            
        -   特性：满足预设条件自动执行，无需人工干预
            
        
    
-   生态能力扩展
    
    -   以太坊不仅是数字货币载体，更是基础设施
        
    -   支撑：Dapps / DeFi / NFT 等生态系统
        
    
-   历史变迁（1.0 → 2.0）
    
    -   以太坊 1.0：PoW（工作量证明）
        
        -   TPS 约 30
            
        -   问题：速度慢、费用高
            
        -   多次升级但若不改变 PoW，核心问题难根治
            
        
    -   2020.12：信标链（Beacon Chain）启动
        
        -   新链专门运行 PoS 共识
            
        -   主网仍 PoW，两条链并行运行、互不干扰
            
        
    -   PoW 能耗问题：大量电力驱动 GPU/矿机哈希计算（被研究类比为接近中小国家能耗）
        
    -   The Merge：两链合并 → 进入以太坊 2.0 时代
        
    
-   多层架构
    
    -   L1（Layer 1）：主网，负责最终安全性与共识
        
    -   L2（Layer 2）：交易批量处理后提交至 L1，降低 Gas 费
        
    -   侧链（Sidechains）：独立运行，通过桥与主网交互
        
    
-   钱包与安全（Austin Griffith）
    
    -   主题：钱包设置、助记词（Mnemonics）、钱包安全
        
    -   MetaMask 安装演示
        
    -   强调：种子短语（Seed Phrase）必须保管好
        
        -   若他人拿到 12 个单词，可在任何地方重建钱包并转走所有资金
            
        
    -   去中心化优势：无需许可创建账户
        
        -   银行开户：审核 + 你是数据库记录
            
        -   以太坊账户：生成随机数（这里体现为 12 个单词）即可参与
            
        
    
-   实操记录
    
    -   在 Edge 与 iPhone 分别下载 MetaMask
        
    -   MetaMask 默认隐藏测试网
        
        -   需手动开启「显示/隐藏测试网络」
            
        
    -   领取测试币
        
        -   使用「Google Clond Web3」
            
        -   收到 0.05 Sepolia ETH 测试币
            
        -   Faucet 例：QuickNode（Sepolia）
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->
















### 1\. 宏观叙事的“去魅”与“重构”：从密码朋克到华尔街

**核心思考：** 行业周期的演进不仅仅是技术的迭代，更是**话语权的转移**。

-   **“正规军”入场的双刃剑：** 笔记指出2024年后的核心变化是ETF通过和监管松绑。这标志着Web3从“对抗传统金融”的1.0叙事（比特币初期）转向“融合传统金融”的4.0叙事。
    
    -   **利：** 资金体量从“投机热钱”变为“配置型资金”，提供了长期流动性。
        
    -   **弊：** 门槛极速升高。合规（Compliance）成为与代码（Code）同等重要的生存基石。对于早期Builder而言，单纯的技术创新若无合规护城河，极易被资本巨头通过RWA或ETF通道降维打击。
        
-   **周期的错位感：** 尽管宏观环境转暖（特朗普当选、ETF上市），但技术应用层（Layer2, GameFi）仍在寻找大规模落地场景。当前的矛盾在于：**金融通道已打通（基础设施过剩），但承接资金的优质资产（DApp应用）仍匮乏。** 这正是“资产上链”（RWA）和DePIN成为新热点的原因——行业急需外部造血，而非内部互割。
    

### 2\. 人才市场的“哑铃效应”：极度内卷与极度稀缺

**核心思考：** 数据揭示了Web3就业市场残酷的**结构性错配**，而非单纯的“缺人”。

-   **红海中的幻觉（450:1）：** 初级岗位450:1的录用比 揭示了一个残酷现实：**“会写Solidity”不再是稀缺技能。** 随着AI工具的普及，基础代码生成的门槛被抹平。单纯的初级开发和Mod（社区运营）已沦为低价值劳动力。
    
-   **蓝海的护城河：** 真正的稀缺性在于\*\*“复合型深水区”\*\*。
    
    -   **语言护城河：** Rust工程师的稀缺（溢价50%） 暗示了高性能公链（如Solana）和底层基础设施建设仍是硬核战场。
        
    -   **认知护城河：** Tokenomics研究员和安全审计的高薪 说明，行业最缺的不是“写代码的人”，而是“设计经济系统”和“保障资产安全”的人。这两个领域AI暂时无法完全替代，因为涉及复杂的博弈论和非标逻辑。
        

### 3\. “人造杠杆”下的组织形态进化

**核心思考：** Web3重新定义了\*\*“人效比”\*\*，这对从业者的综合素质提出了极高要求。

-   **极高的人效比（TVL/Headcount）：** “10人团队管理10亿美金TVL” 是Web3最迷人也最危险的特征。
    
    -   这意味着每个个体必须是\*\*“超级个体”\*\*。在传统互联网，容错率可以靠QA团队堆叠；在Web3，一行代码的漏洞就是10亿美金的归零。
        
    -   这也解释了为什么面试开始考察“链上交互记录（On-chain Reputation）”。学历和简历可以造假，但链上的Tx Hash、Github的Commit History是不可篡改的“工作量证明”（PoW）。
        

### 4\. 地缘政治与职能分工的割裂

**核心思考：** 全球化协作背后的**地缘功能固化**。

-   **硅谷造轮子，亚洲做市场：** 笔记提到硬核技术集中在北美，而交易所/运营集中在亚洲和中东。
    
    -   这种分工某种程度上限制了亚洲从业者的天花板。如果身处亚洲，想要突破薪资天花板进入“第一梯队（技术与合规）”，必须主动打破地缘刻板印象，更多地参与全球开源社区、底层协议开发，而非仅仅停留在应用层运营或交易所业务。
        
-   **Remote的常态化：** 既然“国际化合作、英语工作环境”是常态，那么**英语能力**和**异步协作能力**实际上成为了比具体技术栈更底层的通用能力。
    

### 总结：破局点在哪里？

在这个“初级人才过剩、精英人才断层”的时代，想要摆脱 450:1 的红海竞争，路径非常清晰：

1.  **拒绝纯粹的“代码工具人”定位：** 转向**Rust**（高性能方向）或**智能合约安全/审计**（高壁垒方向）。
    
2.  **建立链上声誉：** 用Github贡献和链上交互替代传统的PDF简历。
    
3.  **拥抱AI与合规：** 学会利用AI提高效率，同时必须具备基本的合规与金融常识，因为未来的Web3不再是法外之地。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
