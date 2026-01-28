---
timezone: UTC+8
---

# james_1022

**GitHub ID:** ARZER-TW

**Telegram:** @Lain_1022

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->
今天最大的收穫就是完成了SPACE活動，第一次主持 Space，緊張是一定的。

但聊完之後覺得，這個主題確實值得討論。很多人進幣圈這麼久，可能真的沒認真想過「為什麼需要區塊鏈」這個問題。包括我自己，也是最近才開始補這一課。

主持方面還有很多要學，但至少這次跨出去了。下次會更好。

之後就是要準備撰寫SPACE回顧了，老實講因為當下要主持所以心思其實並沒有放太多在嘉賓的對話內容上，所以還要將整個SPACE重聽一遍，這也是我之後需要加強的部分吧，還是有太多東西要學了。
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->

跟同學們進行了腦爆PT2，正式確定了題目。

完成了漏洞修復，也完成了運行**Solidity by Example。**
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->



# 重入攻擊漏洞修復案例 - 學習筆記

> Web3 實習手冊 - 合約安全章節作業
> 
> 作者：James 日期：2026-01-26

## 1\. 漏洞概述

### 什麼是重入攻擊（Reentrancy Attack）？

重入攻擊是智能合約安全中最經典也最危險的漏洞之一。其核心原理是：

**當合約 A 調用外部合約 B 時，B 可以在 A 完成狀態更新之前「重新進入」A 的函數，反覆提取資金。**

這就像 ATM 機在扣款之前就先吐錢，而攻擊者可以在扣款發生前反覆按「提款」按鈕。

### 歷史背景：The DAO 事件

2016 年 6 月，The DAO（一個去中心化自治組織）遭受重入攻擊，約 **6000 萬美元** ETH 被盜。這次事件直接導致：

-   以太坊社區分裂
    
-   硬分叉產生 ETH（主鏈）和 ETC（經典鏈）
    
-   智能合約安全意識的覺醒
    

## 2\. 漏洞原理分析

### 2.1 問題代碼模式（Checks-Effects-Interactions 違反）

```
❌ 有漏洞的順序：
1. 檢查條件 (Checks)
2. 執行外部調用 (Interactions) ← 危險！
3. 更新狀態 (Effects)
```

當外部調用發生時，控制權暫時轉移到外部合約。如果狀態還沒更新，外部合約可以利用這個窗口再次調用原函數。

### 2.2 攻擊流程圖

```
攻擊者                     有漏洞的合約
   │                            │
   │──── deposit 1 ETH ────────>│
   │                            │ balance[attacker] = 1 ETH
   │                            │
   │──── withdraw() ───────────>│
   │                            │ 1. check: balance > 0 ✓
   │                            │ 2. send 1 ETH ─────────┐
   │<───────────────────────────│                        │
   │                            │                        │
   │  receive() {               │                        │
   │    withdraw() ─────────────────────────────────────>│
   │  }                         │ 1. check: balance > 0 ✓ (還沒更新！)
   │                            │ 2. send 1 ETH ─────────┐
   │<───────────────────────────│                        │
   │                            │                        │
   │  receive() {               │                        │
   │    withdraw() ─────────────────────────────────────>│
   │  }                         │ ... (重複直到合約餘額歸零)
   │                            │
   │                            │ 3. balance[attacker] = 0 (太晚了！)
```

### 2.3 為什麼 `call` 特別危險？

```solidity
// 這三種轉帳方式的 gas 限制不同：
msg.sender.transfer(amount);  // 固定 2300 gas，無法重入
msg.sender.send(amount);      // 固定 2300 gas，無法重入
msg.sender.call{value: amount}(""); // 轉發所有 gas，可以重入！
```

`call` 是目前推薦的轉帳方式（因為 gas 成本變化），但它會轉發所有剩餘 gas，讓接收方有足夠 gas 執行複雜邏輯，包括重入攻擊。

## 3\. 案例實作

### 3.1 有漏洞的合約 (`VulnerableVault.sol`)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract VulnerableVault {
    mapping(address => uint256) public balances;
    
    function deposit() external payable {
        balances[msg.sender] += msg.value;
    }
    
    function withdraw() external {
        uint256 amount = balances[msg.sender];
        require(amount > 0, "No balance");
        
        // ❌ 危險：先轉帳，後更新狀態
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success, "Transfer failed");
        
        balances[msg.sender] = 0;  // 太晚了！
    }
    
    function getBalance() external view returns (uint256) {
        return address(this).balance;
    }
}
```

### 3.2 攻擊合約 (`Attacker.sol`)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "./VulnerableVault.sol";

contract Attacker {
    VulnerableVault public vault;
    address public owner;
    uint256 public attackCount;
    
    constructor(address _vault) {
        vault = VulnerableVault(_vault);
        owner = msg.sender;
    }
    
    // 發起攻擊
    function attack() external payable {
        require(msg.value >= 1 ether, "Need at least 1 ETH");
        
        // 先存入一些 ETH 作為提款憑證
        vault.deposit{value: msg.value}();
        
        // 觸發第一次提款，開始重入循環
        vault.withdraw();
    }
    
    // 接收 ETH 時自動觸發重入
    receive() external payable {
        if (address(vault).balance >= 1 ether) {
            attackCount++;
            vault.withdraw();  // 重入！
        }
    }
    
    // 提取偷來的資金
    function withdraw() external {
        require(msg.sender == owner, "Not owner");
        payable(owner).transfer(address(this).balance);
    }
    
    function getBalance() external view returns (uint256) {
        return address(this).balance;
    }
}
```

### 3.3 修復後的合約 (`SecureVault.sol`)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

contract SecureVault is ReentrancyGuard {
    mapping(address => uint256) public balances;
    
    event Deposit(address indexed user, uint256 amount);
    event Withdrawal(address indexed user, uint256 amount);
    
    function deposit() external payable {
        balances[msg.sender] += msg.value;
        emit Deposit(msg.sender, msg.value);
    }
    
    // ✅ 修復方案 1: Checks-Effects-Interactions 模式
    function withdraw() external nonReentrant {  // ✅ 修復方案 2: 重入鎖
        uint256 amount = balances[msg.sender];
        require(amount > 0, "No balance");
        
        // ✅ 先更新狀態（Effects）
        balances[msg.sender] = 0;
        
        // ✅ 最後才執行外部調用（Interactions）
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success, "Transfer failed");
        
        emit Withdrawal(msg.sender, amount);
    }
    
    function getBalance() external view returns (uint256) {
        return address(this).balance;
    }
}
```

## 4\. 修復方案詳解

### 方案 1：Checks-Effects-Interactions (CEI) 模式

這是最基本也最重要的防護原則：

```
✅ 正確的順序：
1. Checks   - 檢查所有條件
2. Effects  - 更新所有狀態變數
3. Interactions - 最後才執行外部調用
```

**原理**：即使攻擊者重入，狀態已經更新，條件檢查會失敗。

### 方案 2：重入鎖（Reentrancy Guard）

OpenZeppelin 的 `ReentrancyGuard` 使用一個狀態變數作為鎖：

```solidity
abstract contract ReentrancyGuard {
    uint256 private constant NOT_ENTERED = 1;
    uint256 private constant ENTERED = 2;
    uint256 private _status = NOT_ENTERED;
    
    modifier nonReentrant() {
        require(_status != ENTERED, "ReentrancyGuard: reentrant call");
        _status = ENTERED;
        _;
        _status = NOT_ENTERED;
    }
}
```

**原理**：第一次進入時上鎖，重入時檢測到已上鎖就 revert。

### 方案 3：Pull Payment 模式

不主動推送資金，讓用戶自己來提取：

```solidity
mapping(address => uint256) public pendingWithdrawals;

function requestWithdrawal() external {
    uint256 amount = balances[msg.sender];
    balances[msg.sender] = 0;
    pendingWithdrawals[msg.sender] += amount;
}

function claimWithdrawal() external {
    uint256 amount = pendingWithdrawals[msg.sender];
    pendingWithdrawals[msg.sender] = 0;
    payable(msg.sender).transfer(amount);
}
```

## 5\. 測試驗證

使用 Foundry 進行測試，驗證：

1.  **攻擊成功**：對有漏洞合約的重入攻擊能夠竊取資金
    
2.  **防護有效**：對修復後合約的攻擊會失敗
    

詳見 `test/ReentrancyTest.t.sol`

## 6\. 進階思考

### 6.1 跨函數重入（Cross-function Reentrancy）

重入不只發生在同一個函數，也可能跨函數：

```solidity
contract CrossFunctionVulnerable {
    mapping(address => uint256) public balances;
    
    function withdraw() external {
        uint256 amount = balances[msg.sender];
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success);
        balances[msg.sender] = 0;
    }
    
    function transfer(address to, uint256 amount) external {
        require(balances[msg.sender] >= amount);
        balances[msg.sender] -= amount;
        balances[to] += amount;
    }
}
```

攻擊者可以在 `withdraw` 的回調中調用 `transfer`，因為 balance 還沒清零。

### 6.2 跨合約重入（Cross-contract Reentrancy）

當多個合約共享狀態時，攻擊者可能通過合約 A 的回調來操作合約 B。

### 6.3 Read-only Reentrancy

即使沒有狀態修改，讀取過時的狀態也可能造成問題（常見於 DeFi 協議的價格計算）。

## 7\. 最佳實踐清單

-   \[ \] 遵循 Checks-Effects-Interactions 模式
    
-   \[ \] 對涉及 ETH/Token 轉移的函數加上 `nonReentrant` 修飾符
    
-   \[ \] 使用 OpenZeppelin 的 `ReentrancyGuard`
    
-   \[ \] 審計時特別關注 `call`、`delegatecall`、`transfer`、`send`
    
-   \[ \] 考慮使用 Pull Payment 模式處理批量支付
    
-   \[ \] 注意跨函數和跨合約的重入風險
    

## 8\. 參考資料

-   [The DAO Hack Explained](https://www.gemini.com/cryptopedia/the-dao-hack-makerdao)
    
-   [OpenZeppelin ReentrancyGuard](https://docs.openzeppelin.com/contracts/4.x/api/security#ReentrancyGuard)
    
-   [SWC-107: Reentrancy](https://swcregistry.io/docs/SWC-107)
    
-   [Damn Vulnerable DeFi - Reentrancy Challenges](https://www.damnvulnerabledefi.xyz/)
    

* * *

## 總結

重入攻擊的本質是**狀態更新的時序問題**。理解這個漏洞不僅需要知道「怎麼修」，更重要的是理解「為什麼會發生」。

核心教訓：

1.  **永遠假設外部調用會嘗試重入**
    
2.  **狀態更新必須在外部調用之前**
    
3.  **多重防護優於單一防護**（CEI + ReentrancyGuard）
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->




實習計畫第二周結束，本周最讓我印象深刻的是「與人合作」這個課題。

  

準備舉辦 Space 活動、黑客松組隊，都是我之前較少接觸的領域。過程中發現，要把每個人串接起來，不管是嘉賓還是組員，時間成本都遠比想像中高。每個看似簡單的活動背後，所需的準備與付出都超乎預期。

  

我以前參加黑客松幾乎都是單人組隊，出發點一直是「學習、提升技術」。但聽了 @0xwachi\_ 老師在群組中的分享：

  

「我始終覺得是人與人之間真摯的關係，以及你得知道這個人靠不靠譜。」 「黑客松就是一次絕佳的機會，再次相互確認工作態度、性格和能力。基於此，一個人參賽一點意義都沒有。」

  

這些話讓我換了一個角度去理解黑客松。

  

假日和組員們進行了第一次腦暴，氛圍比想像中融洽，大家都願意主動拋出想法，最後透過投票收斂出共識，整個過程都十分順暢。

  

這讓我重新思考：參加黑客松，我到底能獲得什麼？ 或許技術只是副產品，真正的價值是找到值得一起打仗的人。

  

@ETHPanda\_Org @LXDAO\_Official @web3careerbuild #ETHPanda #Web3實習計畫 #LXDAO
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->





* * *

## 今日學習心得：第一次 ZK 投票體驗

* * *

### 學習概覽

今天實際操作了 My First ZKVote，完成一次 ZK 投票並在 Etherscan 上追蹤交易細節。這次學習讓我釐清了一個一直以來的誤解：**ZK 不等於完全匿名**。

* * *

### 核心問題：為什麼 ZK 投票還是看得到我的錢包地址？

完成投票後去 Etherscan 查，`From` 欄位清清楚楚顯示我的地址，那 ZK 到底「零知識」了什麼？

答案是：**交易層和應用層是分開的**。

| 層級 | 公開內容 | ZK 能保護嗎 |
| --- | --- | --- |
| 交易層 | From 地址、Gas、Nonce | ❌ Ethereum 協議本身的要求 |
| 應用層 | 投票選項、身份秘密 | ✅ ZK 發揮作用的地方 |

My First ZKVote 的隱私模型是：**知道你來投票了，但不知道你投給誰**——跟現實世界的選舉一樣。

* * *

### 如何用 Etherscan 判斷是否為 ZK 交易

| 檢查點 | 一般投票 | ZK 投票 |
| --- | --- | --- |
| Gas 消耗 | ~40,000 | ~300,000+（pairing 運算貴） |
| Input Data | 很短，參數可讀 | 一堆 32-byte hex（proof 數據） |
| Event Logs | 直接有 voter 地址、投票選項 | 只有 nullifier、merkleTreeRoot 等 |

另外，如果合約沒有在 Etherscan 上 verify，就看不到 proof、root 這些欄位名稱，只會顯示原始 hex bytes。

* * *

### 小結

ZK 是一個**選擇性隱藏**的工具，設計者決定保護什麼、公開什麼。這次實作讓我理解 ZK 投票的核心價值是「可審計但不可追溯」：任何人都能驗證投票過程合法，但沒人能知道具體的投票內容。

* * *
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->






完整的學習了GAS優化技巧，一步步跟著AI學習，讓AI出題給出未優化合約我在手動修改。  
Gas 優化技巧總結

* * *

### 1\. Storage → Memory 緩存

**問題**：多次讀寫同一個 storage 位置

```solidity
// ❌ 每次都戳 storage
userInfo[msg.sender].balance += 100;
userInfo[msg.sender].lastUpdate = block.timestamp;
userInfo[msg.sender].isActive = true;

// ✅ 讀一次，改完，寫一次
UserInfo memory user = userInfo[msg.sender];
user.balance += 100;
user.lastUpdate = block.timestamp;
user.isActive = true;
userInfo[msg.sender] = user;
```

**原理**：Storage 讀寫超貴（2100+ gas），Memory 很便宜（3 gas）

* * *

### 2\. Bit Packing

**問題**：變數類型太大，浪費 storage slot

```solidity
// ❌ 佔 3 個 slots
struct UserInfo {
    uint256 balance;     // 32 bytes → slot 0
    uint256 lastUpdate;  // 32 bytes → slot 1
    bool isActive;       // 1 byte   → slot 2（浪費 31 bytes）
}

// ✅ 佔 1 個 slot
struct UserInfo {
    uint128 balance;     // 16 bytes ┐
    uint64 lastUpdate;   // 8 bytes  ├─ slot 0
    bool isActive;       // 1 byte   ┘
}
```

**原理**：每個 slot 寫入要 20,000 gas，slot 越少越省

* * *

### 3\. 迴圈優化

**問題**：重複讀取 `length` + 溢位檢查

```solidity
// ❌ 每次都讀 storage 的 length
for (uint256 i = 0; i < arr.length; i++) { }

// ✅ 緩存 length + unchecked
uint256 len = arr.length;
for (uint256 i = 0; i < len;) {
    // ...
    unchecked { ++i; }
}
```

**原理**：`length` 在 storage，迴圈計數器不可能溢位

* * *

### 4\. external + calldata

**問題**：不必要的參數複製

```solidity
// ❌ 複製參數到 memory
function process(uint256[] memory data) public { }

// ✅ 直接讀 calldata
function process(uint256[] calldata data) external { }
```

**原理**：`calldata` 是唯讀，不用複製；`external` 函數才能用 `calldata`

* * *

### 5\. Custom Errors

**問題**：錯誤訊息字串存在鏈上

```solidity
// ❌ 字串要存到 bytecode
require(amount > 0, "Amount must be positive");

// ✅ 只存 4 bytes
error ZeroAmount();
if (amount == 0) revert ZeroAmount();
```

**原理**：Custom Error 只存 selector（4 bytes），不存完整字串
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->







完整閱讀並學習了這兩篇在 X 上非常熱門的 Claude Code 指南——  
[https://x.com/affaanmustafa/status/2014040193557471352?s=20](https://x.com/affaanmustafa/status/2014040193557471352?s=20) 和  
[https://x.com/affaanmustafa/status/2012378465664745795?s=20，](https://x.com/affaanmustafa/status/2012378465664745795?s=20，)  
作者是 **Anthropic 黑客松的獲獎者 Affaan Mustafa（@affaanmustafa）**，他也是那套「Everything Claude Code」生產級配置套件的創作者。

我依照文中的說明，大幅調整了自己的 Claude Code 設定檔，花了相當多時間重新組織 agent、skills、rules 和 hooks，過程中學到了很多原本完全不知道的高階功能，例如：

-   用 subagent 做 PLANNING、REVIEW、REFACTOR、TDD、E2E 等專業分工；
    
-   用 hooks 在每次 Edit / tool use 時自動觸發 lint、type‑check、安全檢查；
    
-   用 rules 撰寫「永遠必須遵守」的 coding style、security、testing 等規則，讓 Claude 在撰寫時自然貼近團隊規範。​
    

之後打算先用這套配置做一個小專案來實驗，例如從零開始搭一個 Web3 DApp，同時觀察實際開發流程是否真能像文中描述那樣「像 senior engineer 一樣」接管規劃、實現、測試與部署。 看了他的 GitHub（`affaan-m/everything-claude-code`）後，也發現他有開發過 Solana 鏈上分析與 on‑chain data pipeline 的工具，這代表這一整套配置在 Web3 / Solana 類型的鏈上開發上，確實有實務驗證過，穩定性與擴充性都相當可期。

不過畢竟是別人的工作流，後面應該還是需要依照個人習慣，針對：

-   agent 的角色與分工（例如要不要把 security‑reviewer、e2e‑runner 抽離成 subagent）；
    
-   rules 的嚴格程度（例如是否強制 80% test coverage、是否強制不用 console.log）；
    
-   skills 的領域（例如是否加入 Web3 / Solidity / Sui / Rust / 鏈上 protocol 等專屬 skill）；
    

做進一步微調。
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->








晚上聽了Uniswap分享會再度複習了一遍v2v3v4，剛好下午在學習Intent跟The compact這個也是跟Uniswap發布的新架構，以前Intent跟The compact我都有聽過相關的分享會，但是一直沒有到非常熟悉，我還以為他們兩者間是競爭關係，今天總算弄懂了，也知道兩者怎麼協同運作。**小抱怨一下最近作息已經很亂了都是早上睡覺，好死不死樓上在裝修固定早上9點到下午5點，還在我房間正上方，搞得我整個人心神不寧，這幾天任務也沒什麼解，好在今天的聲音到1點就停了，這應該代表裝修完了吧 ! 對吧 !?** 等等半夜趕快把入門技術向任務補一補，大概看了一下應該很快能搞定。  
  
DeFi 交易系統演進：從 AMM 到 Intent-Based 交易

> 學習總結 | 涵蓋 Uniswap V2/V3/V4、Intent 交易、ERC-7683、The Compact

* * *

## 1\. 概覽：交易範式的演進

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        DeFi 交易範式演進                                     │
│                                                                             │
│   2018          2020           2021           2023           2024+          │
│    │             │              │              │              │             │
│    ▼             ▼              ▼              ▼              ▼             │
│ ┌──────┐    ┌──────┐       ┌──────┐      ┌──────┐      ┌──────────┐       │
│ │ V1   │    │ V2   │       │ V3   │      │ V4   │      │ Intent   │       │
│ │      │    │      │       │      │      │      │      │ Based    │       │
│ │單一池│    │任意對│       │集中  │      │Hooks │      │          │       │
│ │      │    │恆定積│       │流動性│      │模組化│      │用戶只說  │       │
│ │      │    │      │       │      │      │      │      │想要什麼  │       │
│ └──────┘    └──────┘       └──────┘      └──────┘      └──────────┘       │
│                                                                             │
│   資金效率:  1x      →      1x      →    4000x    →   可編程   →   最優執行  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 核心轉變

| 維度 | 傳統 AMM | Intent-Based |
| --- | --- | --- |
| 用戶角色 | 指定「怎麼做」 | 只說「想要什麼」 |
| 執行方式 | 直接與池子交互 | Solver 競爭最優路徑 |
| MEV 歸屬 | 被攻擊者提取 | 返還給用戶 |
| 跨鏈支援 | 需要手動橋接 | 原生整合 |
| Gas 支付 | 用戶自付 | 可由 Solver 代付 |

* * *

## 2\. Uniswap V2：AMM 基礎

### 2.1 恆定乘積公式

```
核心公式：x × y = k

其中：
- x = Token A 的儲備量
- y = Token B 的儲備量  
- k = 常數（每次交易後維持不變，僅在添加/移除流動性時改變）

價格關係：
- Token A 的價格（以 B 計價）：P_A = y / x
- Token B 的價格（以 A 計價）：P_B = x / y
```

**圖解：**

```
y (Token B)
│
│  ╲
│    ╲  x × y = k（雙曲線）
│      ╲
│        ╲
│          ╲___________
│                       
└─────────────────────── x (Token A)

交易 = 沿著曲線移動
價格 = 曲線的切線斜率
```

### 2.2 交易機制

**買入 Token A（賣出 Token B）：**

```
交易前：(x, y) 滿足 x × y = k
用戶投入：Δy
用戶獲得：Δx

交易後：(x - Δx) × (y + Δy) = k

實際計算（含 0.3% 手續費）：
Δx = (x × Δy × 997) / (y × 1000 + Δy × 997)
```

### 2.3 套利機制

```
場景：CEX 價格 1 ETH = 2000 USDC，Uniswap 價格 1 ETH = 1900 USDC

套利流程：
┌─────────────────────────────────────────────────────────────┐
│  1. 套利者在 Uniswap 用 1900 USDC 買入 1 ETH               │
│  2. 在 CEX 賣出 1 ETH 獲得 2000 USDC                       │
│  3. 利潤：2000 - 1900 = 100 USDC（扣除手續費和 Gas）       │
│                                                             │
│  結果：Uniswap 上 ETH 減少 → ETH 價格上升 → 趨近市場價格   │
└─────────────────────────────────────────────────────────────┘

這就是 AMM 的價格發現機制：套利者是「免費的勞工」，推動價格對齊
```

### 2.4 無常損失（Impermanent Loss）

**定義：** LP 因價格變動，相比單純持有資產所產生的相對損失。

```
初始狀態：
- 存入 1 ETH + 2000 USDC（假設 1 ETH = 2000 USDC）
- 總價值：4000 USD

價格變動後（ETH 漲到 4000 USDC）：
- 如果只是持有：1 ETH × 4000 + 2000 USDC = 6000 USD
- 作為 LP：根據 x × y = k，池子會再平衡
  - 新狀態約：0.707 ETH + 2828 USDC = 5656 USD

無常損失：(6000 - 5656) / 6000 ≈ 5.7%
```

**無常損失速查表：**

| 價格變動 | 無常損失 |
| --- | --- |
| 1.25x | 0.6% |
| 1.50x | 2.0% |
| 2x | 5.7% |
| 3x | 13.4% |
| 4x | 20.0% |
| 5x | 25.5% |

### 2.5 閃電貸（Flash Loan）

```solidity
// V2 的 Flash Swap 機制
function swap(
    uint amount0Out, 
    uint amount1Out, 
    address to, 
    bytes calldata data  // 如果 data 不為空，觸發回調
) external {
    // 1. 先把代幣轉給用戶
    if (amount0Out > 0) _safeTransfer(token0, to, amount0Out);
    if (amount1Out > 0) _safeTransfer(token1, to, amount1Out);
    
    // 2. 如果有 data，回調用戶合約
    if (data.length > 0) {
        IUniswapV2Callee(to).uniswapV2Call(msg.sender, amount0Out, amount1Out, data);
    }
    
    // 3. 檢查還款（必須滿足 k 值不減少）
    uint balance0 = IERC20(token0).balanceOf(address(this));
    uint balance1 = IERC20(token1).balanceOf(address(this));
    require(balance0 * balance1 >= reserve0 * reserve1, 'K');
}
```

**應用場景：** 套利、清算、抵押品置換

### 2.6 TWAP 預言機

**Time-Weighted Average Price：** 防止閃電貸操縱價格

```
原理：
- 每個區塊記錄「價格 × 時間」的累積值
- TWAP = (累積值_end - 累積值_start) / (時間_end - 時間_start)

特點：
- 操縱成本 = 操縱幅度 × 持續時間
- 時間窗口越長，越難操縱
```

* * *

## 3\. Uniswap V3：集中流動性

### 3.1 核心創新：自定義價格區間

```
V2 的問題：
┌─────────────────────────────────────────────────────────────┐
│  流動性均勻分佈在 (0, ∞) 的價格範圍                         │
│  但 99% 的交易發生在很窄的價格區間                          │
│  大量資金閒置，資金效率極低                                  │
└─────────────────────────────────────────────────────────────┘

V3 的解法：
┌─────────────────────────────────────────────────────────────┐
│  LP 可以選擇只在 [P_low, P_high] 區間提供流動性             │
│  同樣的資金量，在更窄的區間 = 更深的流動性                  │
│  理論上可提升最高 4000 倍資金效率                            │
└─────────────────────────────────────────────────────────────┘
```

**圖解：**

```
流動性
  │
  │     ┌─────┐
  │     │ V3  │ ← 集中在 [1800, 2200] 區間
  │     │     │
  │  ┌──┴─────┴──┐
  │  │    V2     │ ← 均勻分佈
  │  │           │
  └──┴───────────┴────────────────── 價格
     1000  1800  2200  3000
```

### 3.2 Tick 系統

**Tick 是什麼？** 價格的離散化表示。

```
公式：price = 1.0001^tick

常用 Tick 間距：
- tickSpacing = 1:   適用於 0.01% 手續費池（穩定幣）
- tickSpacing = 10:  適用於 0.05% 手續費池
- tickSpacing = 60:  適用於 0.3% 手續費池
- tickSpacing = 200: 適用於 1% 手續費池

範例：
- tick = 0      → price = 1.0001^0 = 1
- tick = 100    → price = 1.0001^100 ≈ 1.01
- tick = 23027  → price ≈ 10（約 10:1 的價格比）
```

**為什麼用 Tick？**

```
優點：
1. 價格變化是乘法關係，tick 是加法 → 計算更簡單
2. 流動性只在 tick 邊界變化 → 高效追蹤
3. 支援任意精度的價格範圍
```

### 3.3 流動性聚合

```
場景：三個 LP 在不同區間提供流動性

        LP1: [1800, 2200]  流動性 L1
        LP2: [1900, 2100]  流動性 L2
        LP3: [2000, 2400]  流動性 L3

當前價格 = 2050 時：

價格
  │
  │                    ┌───────────────┐
  │           ┌────────┤    L3         │
  │    ┌──────┤  L2    └───────────────┘
  │    │ L1   │        │
  │────┴──────┴────────┴─────────────────
  │   1800   1900  2000│2100  2200  2400
  │                    │
  │               當前價格
  │
  └─▶ 有效流動性 = L1 + L2 + L3
```

### 3.4 √P 與 L 公式

V3 使用 `√P`（價格平方根）而非直接使用價格 P，原因：

```
傳統公式：x × y = k
V3 公式：L = √(x × y)，其中 L = 流動性

轉換關係：
- √P = √(y/x)
- x = L / √P
- y = L × √P

為什麼用 √P？
1. 交易時只需要更新一個變數（√P）
2. 避免大數乘法溢出
3. 流動性 L 在區間內是常數，計算更簡單
```

### 3.5 手續費分配

```
手續費檔位：0.01%、0.05%、0.3%、1%

分配邏輯：
1. 手續費按「有效流動性佔比」分配
2. 只有「在價格範圍內」的流動性才能賺取手續費
3. 區間越窄 → 佔比越大 → 賺得越多（但風險也越高）

範例：
- LP_A 在 [1900, 2100] 提供 1000 流動性
- LP_B 在 [1000, 3000] 提供 1000 流動性
- 當前價格 2000，交易產生 10 USDC 手續費

LP_A 的有效流動性佔比更高 → LP_A 分到更多手續費
```

### 3.6 區間設置策略

| 資產類型 | 建議區間 | 手續費檔位 | 原因 |
| --- | --- | --- | --- |
| 穩定幣對 (USDC/USDT) | ±0.1% | 0.01% | 價格幾乎不變 |
| 主流資產 (ETH/USDC) | ±10~20% | 0.3% | 中等波動 |
| 高波動資產 | ±30~50% | 1% | 需要更寬區間 |

* * *

## 4\. Uniswap V4：Hooks 與模組化

### 4.1 Hooks 機制

**Hook = 在交易生命週期中插入自定義邏輯的鉤子**

```
交易生命週期中的 Hook 插入點：

  beforeInitialize ──▶ 池子初始化 ──▶ afterInitialize
          │
          ▼
  beforeAddLiquidity ──▶ 添加流動性 ──▶ afterAddLiquidity
          │
          ▼
  beforeRemoveLiquidity ──▶ 移除流動性 ──▶ afterRemoveLiquidity
          │
          ▼
  beforeSwap ──▶ 交易執行 ──▶ afterSwap
          │
          ▼
  beforeDonate ──▶ 捐贈 ──▶ afterDonate
```

### 4.2 Hook 應用場景

```solidity
// 範例：動態手續費 Hook
contract DynamicFeeHook is BaseHook {
    function beforeSwap(
        address sender,
        PoolKey calldata key,
        IPoolManager.SwapParams calldata params,
        bytes calldata hookData
    ) external override returns (bytes4) {
        // 根據波動率動態調整手續費
        uint24 volatility = getVolatility(key);
        uint24 dynamicFee = calculateFee(volatility);
        
        // 設置新的手續費
        poolManager.updateDynamicLPFee(key, dynamicFee);
        
        return BaseHook.beforeSwap.selector;
    }
}
```

**常見 Hook 應用：**

| Hook 類型 | 功能 | 範例 |
| --- | --- | --- |
| 動態手續費 | 根據市場條件調整 | 高波動時提高手續費 |
| 限價單 | 價格觸發自動執行 | 「ETH 到 2500 時賣出」 |
| TWAMM | 時間加權做市 | 大單拆分執行 |
| MEV 保護 | 防止三明治攻擊 | 私有訂單流 |
| 預言機整合 | 自動價格更新 | 結合 Chainlink |

### 4.3 Singleton 架構

```
V3 架構：每個池子一個合約
┌──────┐ ┌──────┐ ┌──────┐
│Pool 1│ │Pool 2│ │Pool 3│  ← 每個都是獨立合約
└──────┘ └──────┘ └──────┘

V4 架構：所有池子在一個合約
┌─────────────────────────────────────┐
│           PoolManager               │
│  ┌──────┐ ┌──────┐ ┌──────┐       │
│  │Pool 1│ │Pool 2│ │Pool 3│       │  ← 共享狀態
│  └──────┘ └──────┘ └──────┘       │
└─────────────────────────────────────┘

優點：
- 跨池子操作更高效（閃電記賬）
- 部署新池子更便宜
- 更好的可組合性
```

* * *

## 5\. Intent-Based 交易：新範式

### 5.1 什麼是 Intent？

```
傳統交易：
用戶說：「用這個路由，在這個 DEX，用這個 Gas 價格，執行這筆交易」
        ↓
      用戶負責一切細節

Intent 交易：
用戶說：「我想把 1 ETH 換成最多的 USDC」
        ↓
      Solver 競爭找最佳方案
```

### 5.2 Intent 的運作流程

```
┌─────────────────────────────────────────────────────────────────┐
│                     Intent 交易流程                             │
│                                                                 │
│  ① 用戶簽署 Intent                                              │
│     └─▶ 「我要把 1 ETH 換成至少 2900 USDC」                     │
│                                                                 │
│  ② Intent 進入訂單池                                            │
│     └─▶ 離線廣播，不上鏈                                        │
│                                                                 │
│  ③ Solver/Filler 競標                                           │
│     ├─▶ Solver A: 「我能給 2920 USDC」                         │
│     ├─▶ Solver B: 「我能給 2935 USDC」                         │
│     └─▶ Solver C: 「我能給 2950 USDC」 ← 勝出                  │
│                                                                 │
│  ④ 執行與結算                                                   │
│     └─▶ Solver C 執行交易，用戶收到 2950 USDC                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 5.3 Intent 解決的問題

| 問題 | 傳統方式 | Intent 方式 |
| --- | --- | --- |
| MEV 攻擊 | 被三明治夾擊 | Solver 競爭，MEV 返還用戶 |
| 路由複雜 | 用戶選擇 DEX | Solver 找最佳路徑 |
| Gas 費用 | 用戶支付 | 可由 Solver 代付 |
| 跨鏈交易 | 手動橋接 | 一次簽名完成 |
| 失敗交易 | 用戶損失 Gas | 不執行不收費 |

### 5.4 Dutch Auction 機制

大多數 Intent 系統使用荷蘭拍賣決定價格：

```
價格
  │
  │  起始價: 3100 USDC
  │  ●───────┐
  │          │
  │          │ 隨時間衰減
  │          │
  │          ●───────┐
  │                  │
  │                  ●───── Filler 在此接單
  │                        （2950 USDC）
  │
  │  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─  最低價: 2800 USDC
  │
  └────────────────────────────────▶ 時間
       0      2min    5min    10min

優點：
1. 價格發現自動化
2. Filler 競爭 → 用戶獲得更好價格
3. 防止 Filler 故意等待壓價
```

* * *

## 6\. ERC-7683：跨鏈 Intent 標準

### 6.1 為什麼需要標準？

```
問題：每個 Intent 系統各自為政

UniswapX ──▶ 自己的訂單格式 ──▶ 自己的 Filler 網路
Across   ──▶ 自己的訂單格式 ──▶ 自己的 Relayer 網路
CoW      ──▶ 自己的訂單格式 ──▶ 自己的 Solver 網路

結果：
- Filler 網路碎片化
- 用戶無法享受最優價格
- 開發者重複造輪子
```

### 6.2 ERC-7683 的解決方案

```
ERC-7683 定義統一標準：

所有 Intent 系統 ──▶ 相同的訂單格式 ──▶ 共享的 Filler 網路
                         │
                         ▼
            ┌────────────────────────┐
            │   CrossChainOrder      │
            │   ISettlementContract  │
            └────────────────────────┘
```

### 6.3 核心資料結構

```solidity
// Gasless 版本（用戶離線簽名）
struct GaslessCrossChainOrder {
    address originSettler;      // 源鏈結算合約
    address user;               // 用戶地址
    uint256 nonce;              // 防重放
    uint256 originChainId;      // 源鏈 ID
    uint32 openDeadline;        // 訂單開放截止
    uint32 fillDeadline;        // 填充截止
    bytes32 orderDataType;      // 子類型標識
    bytes orderData;            // 自定義資料（價格、目標鏈等）
}

// 解析後的標準格式
struct ResolvedCrossChainOrder {
    address user;
    uint256 originChainId;
    uint32 openDeadline;
    uint32 fillDeadline;
    Input[] maxSpent;           // 用戶最多付出
    Output[] minReceived;       // 用戶最少收到
    FillInstruction[] fillInstructions;  // 執行指令
}
```

### 6.4 Settlement 介面

```solidity
// 源鏈結算合約
interface IOriginSettler {
    // 開啟訂單（鏈上版本）
    function open(OnchainCrossChainOrder calldata order) external;
    
    // 開啟訂單（Gasless 版本）
    function openFor(
        GaslessCrossChainOrder calldata order,
        bytes calldata signature,
        bytes calldata originFillerData
    ) external;
    
    // 解析訂單
    function resolve(OnchainCrossChainOrder calldata order) 
        external view returns (ResolvedCrossChainOrder memory);
}

// 目標鏈結算合約
interface IDestinationSettler {
    // Filler 填充訂單
    function fill(
        bytes32 orderId,
        bytes calldata originData,
        bytes calldata fillerData
    ) external;
}
```

### 6.5 ERC-7683 的靈活性

```
ERC-7683 只標準化「介面」，不規定「實現」：

可自定義的部分：
├── 價格機制（Dutch Auction、固定價格、Oracle）
├── 結算驗證（Optimistic、ZK Proof、Native Bridge）
├── Filler 激勵（手續費、MEV 分潤）
└── 執行順序（先 fill 後 open，或反過來）
```

* * *

## 7\. The Compact：資源鎖定基礎設施

### 7.1 定位

```
The Compact ≠ Intent 系統
The Compact = Intent 系統的「底層基礎設施」

類比：
- ERC-7683 = HTTP 協議（定義請求格式）
- The Compact = 資料庫引擎（處理資料儲存）
```

### 7.2 解決的核心問題

```
跨鏈交易的根本問題：原子性不可能

源鏈操作 ──────────────────▶ 目標鏈操作
    │                            │
    │   不同鏈，異步執行          │
    │   無法保證同時成功/失敗     │
    ▼                            ▼

The Compact 的解法：可編程的資源鎖定 + 條件釋放
```

### 7.3 核心概念

Resource Lock（資源鎖）

```
用戶存款 ──▶ Resource Lock ──▶ ERC-6909 Token
                 │
                 ├── token: WETH
                 ├── amount: 10 ETH
                 ├── owner: User（保留所有權）
                 ├── allocator: 防止雙花
                 ├── scope: multichain/singlechain
                 └── resetPeriod: 強制提款等待期
```

Compact（承諾）

```
Resource Lock 可以支援多個 Compact：

Lock #42 (10 ETH)
    │
    ├── Compact A: 承諾 2 ETH 給訂單 A
    ├── Compact B: 承諾 3 ETH 給訂單 B
    └── 剩餘 5 ETH 可用於其他用途

Compact = 「如果條件 X 滿足，則 Y 可以 claim Z」
```

### 7.4 四大角色

```
┌─────────────────────────────────────────────────────────────────┐
│                        The Compact 角色                         │
│                                                                 │
│  ┌─────────────┐    ┌─────────────┐                            │
│  │  Allocator  │    │   Arbiter   │                            │
│  │             │    │             │                            │
│  │ • 追蹤承諾額度│    │ • 驗證條件  │                            │
│  │ • 防止雙花   │    │ • 處理claim │                            │
│  │ • 授權使用   │    │ • 跨鏈證明  │                            │
│  └─────────────┘    └─────────────┘                            │
│                                                                 │
│  ┌─────────────┐    ┌─────────────┐                            │
│  │  Tribunal   │    │  Emissary   │                            │
│  │             │    │             │                            │
│  │ • 跨鏈swap  │    │ • 智能錢包  │                            │
│  │   結算引擎   │    │   後備驗證  │                            │
│  │ • 拍賣機制   │    │ • 簽名恢復  │                            │
│  └─────────────┘    └─────────────┘                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 7.5 The Compact 的應用場景

```
The Compact 不只用於 Intent，還可用於：

1. 跨鏈條件支付
   └── 「Bob 在 Polygon 完成交付 → 釋放 Ethereum 上的付款」

2. 去中心化訂閱
   └── 「每月自動扣款，服務停止則自動停止」

3. 跨鏈借貸抵押
   └── 「A 鏈借款，B 鏈抵押品，觸發清算時跨鏈執行」

4. DAO 資金管理
   └── 「同一筆資金，不同場景不同授權規則」

5. 預言機質押
   └── 「誠實報價 → 領獎勵；作惡 → 被 slash」

通用模式：「如果 X 發生，則 Y 可以獲得 Z」
```

* * *

## 8\. Intent 與 The Compact 的整合

### 8.1 完整交互流程

```
┌─────────────────────────────────────────────────────────────────┐
│                     Phase 0: 用戶存款                           │
│                                                                 │
│  User ──deposit()──▶ The Compact                                │
│           │                │                                    │
│           │                ▼                                    │
│           │         Resource Lock #42                           │
│           │         (10 ETH, Allocator: Autocator)              │
│           │                                                     │
│           ◀───── ERC-6909 Token ─────                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                  Phase 1: 創建 Intent                           │
│                                                                 │
│  User 簽署 ERC-7683 CrossChainOrder:                            │
│  {                                                              │
│    originSettler: CompactSettler,                               │
│    orderData: {                                                 │
│      resourceLockId: #42,        ← 連接到 The Compact           │
│      inputAmount: 1 ETH,                                        │
│      outputToken: USDC,                                         │
│      minOutputAmount: 2900 USDC,                                │
│      destinationChain: Arbitrum,                                │
│      allocatorSignature: <預授權>  ← Allocator 簽名             │
│    }                                                            │
│  }                                                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                  Phase 2: Allocator 授權                        │
│                                                                 │
│  Allocator 追蹤 Lock #42 狀態：                                 │
│  ┌─────────────────────────────────────┐                       │
│  │ total: 10 ETH                       │                       │
│  │ committed: 0 → 1 ETH                │                       │
│  │ available: 10 → 9 ETH               │                       │
│  └─────────────────────────────────────┘                       │
│                                                                 │
│  簽署 allocatorSignature → 放入 orderData                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                  Phase 3: Filler 執行                           │
│                                                                 │
│  Filler 在 Arbitrum 墊付:                                       │
│  ┌─────────────────────────────────────┐                       │
│  │ DestinationSettler.fill()           │                       │
│  │   └── 轉 2950 USDC 給 User          │                       │
│  │   └── emit OrderFilled(...)         │                       │
│  └─────────────────────────────────────┘                       │
│                                                                 │
│  User 收到 2950 USDC ✓                                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                  Phase 4: 跨鏈驗證 + Claim                      │
│                                                                 │
│  Filler 提交跨鏈證明到 Ethereum:                                 │
│  ┌─────────────────────────────────────┐                       │
│  │ Tribunal (Arbiter) 驗證：           │                       │
│  │   1. Arbitrum 上的 fill 事件        │                       │
│  │   2. 金額 >= minOutput              │                       │
│  │   3. recipient 正確                 │                       │
│  │   4. 在 deadline 之前               │                       │
│  └─────────────────────────────────────┘                       │
│                              │                                  │
│                              ▼                                  │
│  ┌─────────────────────────────────────┐                       │
│  │ The Compact.claim()                 │                       │
│  │   └── 驗證 Allocator 授權           │                       │
│  │   └── 從 Lock #42 轉 1 ETH 給 Filler│                       │
│  │   └── 更新 ERC-6909 餘額            │                       │
│  └─────────────────────────────────────┘                       │
│                                                                 │
│  Filler 收到 1 ETH ✓                                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 8.2 資料結構對應

```
ERC-7683                              The Compact
════════                              ══════════

CrossChainOrder
├── originSettler ──────────────────▶ CompactSettler
│
├── orderData
│   ├── resourceLockId ─────────────▶ Resource Lock ID
│   ├── inputAmount ────────────────▶ Compact 承諾金額
│   ├── allocatorSignature ─────────▶ Allocator 預授權
│   └── arbiter ────────────────────▶ Tribunal / Custom Arbiter
│
└── fillDeadline ───────────────────▶ Compact expiry
```

### 8.3 為什麼要這樣分層？

```
分層的好處：

1. 關注點分離
   ├── ERC-7683: 專注訂單表達和 Filler 競爭
   └── The Compact: 專注資金安全和跨鏈驗證

2. 模組化可替換
   ├── 可以換 Allocator（不同信任假設）
   ├── 可以換 Arbiter（不同驗證方式）
   └── 訂單格式可以擴展（不改底層）

3. 安全性
   ├── The Compact 已審計、不可升級
   ├── 單次存款可支援多個 Compact
   └── 強制提款保護用戶最終控制權
```

* * *

## 9\. 現有 Intent 系統比較

### 9.1 系統對比

| 系統 | 資金鎖定 | 防雙花 | 結算驗證 | 跨鏈 |
| --- | --- | --- | --- | --- |
| UniswapX | Permit2（軟鎖定） | Nonce + 簽名 | 即時 | 開發中 |
| Across | SpokePool（硬鎖定） | depositId | Optimistic Oracle | 原生 |
| CoW Protocol | VaultRelayer（軟鎖定） | orderUid | Batch 內原子 | 有限 |
| 1inch Fusion | Permit/Approve | Nonce | 即時 | 有限 |
| The Compact | ERC-6909 Resource Lock | Allocator | 可插拔 Arbiter | 原生 |

### 9.2 軟鎖定 vs 硬鎖定

```
軟鎖定（Permit2）            硬鎖定（The Compact）
══════════════════          ════════════════════

優點：                       優點：
├── Gas 效率高               ├── 確定性高
├── 資金靈活                 ├── 支援長期訂單
└── UX 好                    └── 跨鏈更安全

缺點：                       缺點：
├── 用戶可「背叛」           ├── 額外 Gas
├── Filler 承擔風險          ├── 資金鎖定期間無法使用
└── 不適合長期訂單           └── 複雜度增加
```

### 9.3 The Compact 的改進

```
The Compact 結合兩者優點：

1. 資金是「硬鎖定」的 → 安全性
2. 用 ERC-6909 表示所有權 → 靈活性
3. 單次存款可支援多個 Compact → 效率
4. 強制提款機制 → 用戶最終控制權
```

* * *

## 10\. 常見問題 Q&A

### Uniswap 相關

**Q1：無常損失是否可以避免？**

A：無常損失是 AMM 機制固有的，無法完全避免。主要通過提高手續費收入來補償。V3 的集中流動性可以提高手續費收入，但同時也放大了無常損失。

**Q2：滑點和價格影響是一回事嗎？**

A：不是。

-   **價格影響（Price Impact）**：交易對池子深度的直接結果，可以精確計算
    
-   **滑點（Slippage）**：實際成交價與預期價的不可預測偏差（MEV、區塊延遲等）
    

**Q3：V3 中流動性區間如何設置？**

A：根據資產特性決定：

-   穩定幣：±0.1%，0.01% 手續費
    
-   主流資產：±10-20%，0.3% 手續費
    
-   高波動資產：±30-50%，1% 手續費
    

**Q4：如何防止搶跑攻擊？**

A：

-   使用 Flashbots 等隱私交易池
    
-   設置合理的滑點容忍度
    
-   使用 V4 的 Hooks 機制
    
-   使用 Intent-based 交易
    

**Q5：V4 的 Hooks 能做什麼？**

A：在交易生命週期各節點插入邏輯：

-   動態手續費（根據波動率調整）
    
-   限價單（價格觸發執行）
    
-   TWAMM（大單時間分散）
    
-   MEV 保護
    

**Q6：流動性池會被掏空嗎？**

A：不會。交易前會檢查儲備量，確保不會超額提取。AMM 的數學特性決定了價格會趨向無窮大/無窮小，但永遠不會歸零。

**Q7：V3 手續費檔位如何選擇？**

A：

-   0.01%：穩定幣對
    
-   0.05%：相關性高的資產
    
-   0.3%：主流資產對
    
-   1%：高波動或長尾資產
    

### Intent 相關

**Q8：Intent 交易比傳統 AMM 更好嗎？**

A：各有優劣：

-   Intent 優勢：MEV 保護、跨鏈原生、Gas 抽象
    
-   AMM 優勢：即時執行、完全去中心化、無需信任 Solver
    

**Q9：The Compact 和 Intent 是什麼關係？**

A：

-   ERC-7683（Intent 標準）定義「訂單格式」
    
-   The Compact 提供「資金鎖定基礎設施」
    
-   兩者結合實現安全的跨鏈 Intent
    

**Q10：Solver/Filler 怎麼賺錢？**

A：

-   用戶預期價格與實際執行價格的差價
    
-   批量交易節省的 Gas
    
-   MEV 提取（但需返還用戶部分）
    

**Q11：強制提款是什麼？**

A：如果 Allocator 或 Arbiter 失效，用戶可以等待 resetPeriod 後強制提款，保護資金不會被永久鎖定。

* * *

## 11\. 延伸閱讀與資源

### 官方文檔

-   [Uniswap V2 Whitepaper](https://uniswap.org/whitepaper.pdf)
    
-   [Uniswap V3 Whitepaper](https://uniswap.org/whitepaper-v3.pdf)
    
-   [Uniswap V4 Documentation](https://docs.uniswap.org/)
    
-   [ERC-7683 Specification](https://eips.ethereum.org/EIPS/eip-7683)
    
-   [The Compact GitHub](https://github.com/Uniswap/the-compact)
    

### 技術深入

-   [UniswapX Whitepaper](https://uniswap.org/whitepaper-uniswapx.pdf)
    
-   [Across Protocol Documentation](https://docs.across.to/)
    
-   [CoW Protocol Documentation](https://docs.cow.fi/)
    

### 學習資源

-   Uniswap 基金會 Grant Program
    
-   Uniswap 孵化器和黑客松
    
-   審計補貼計畫
    

* * *

## 附錄：公式速查

### AMM 公式

```
恆定乘積：x × y = k
價格：    P = y / x
交易：    Δx = (x × Δy × 997) / (y × 1000 + Δy × 997)
```

### V3 公式

```
流動性：  L = √(x × y)
價格：    √P = √(y/x)
關係：    x = L / √P
          y = L × √P
Tick：    price = 1.0001^tick
```

### 無常損失

```
IL = 2 × √(price_ratio) / (1 + price_ratio) - 1

price_ratio = 新價格 / 舊價格
```
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->









# Elon 老師 Solidity 課程心得

## 核心收穫：從 EVM 底層理解 Solidity

這堂課最有價值的地方，不是教語法，而是從 **EVM 的運行機制** 出發來理解 Solidity。這讓我意識到：學 Solidity 不能只當成「又一個程式語言」，而是要理解它背後的執行環境。

### EVM 四大執行區的直覺理解

| 執行區 | 類比 | 特性 | Gas 成本 |
| --- | --- | --- | --- |
| Stack | CPU 暫存器 | 棧式運算，指令入棧出棧 | 最低 |
| Storage | 硬碟 | 永久上鏈，合約狀態存這 | 極高（寫入 ~20,000 gas） |
| Memory | RAM | 函數執行期間的臨時空間 | 中等 |
| Calldata | 函數參數 | 唯讀，用戶傳入的輸入數據 | 低 |

**關鍵洞察**：合約優化的核心就是「少碰 Storage」。初始化寫入約 20,000 gas，修改約 5,000 gas。這個量級差異直接決定了合約設計的方向。

* * *

## 資料型別的 Storage 意識

### 值類型 vs 引用類型的本質差異

這不只是語法差異，而是 **storage 開銷的差異**：

**值類型**（直接存值，成本可控）：

-   `bool`：布林值
    
-   `uint` / `int`：整數，可指定位寬（uint8 ~ uint256）
    
-   `address`：地址類型，有內建檢查
    
-   `bytes32`：定長字節
    

**引用類型**（會開新空間，成本更高）：

-   `string`：字串
    
-   動態陣列
    
-   `bytes` / `bytes[]`
    
-   `mapping`
    

**實務原則**：

```solidity
// ❌ 浪費 storage
string userAddress = "0x1234...";

// ✅ 正確做法
address userAddress = 0x1234...;
```

位寬選擇也影響成本：確定數值不會超過 255？用 `uint8` 而非 `uint256`。

* * *

## ERC20 的兩本賬本設計

這是理解代幣合約的核心 mental model：

```solidity
// 賬本一：餘額賬本
mapping(address => uint256) balances;

// 賬本二：授權賬本（雙層 mapping）
mapping(address => mapping(address => uint256)) allowance;
```

**授權賬本的結構**：`allowance[owner][spender] = amount`

-   owner：代幣持有者
    
-   spender：被授權者（通常是 DeFi 協議）
    
-   amount：授權額度
    

這解釋了為什麼與 DeFi 協議互動時，第一步總是 **approve**。

* * *

## 函數可見性與狀態修飾符

### 可見性四種層級

| 修飾符 | 外部可調用 | 繼承合約可用 | 內部可用 |
| --- | --- | --- | --- |
| external | ✅ | ❌ | ❌ |
| public | ✅ | ✅ | ✅ |
| internal | ❌ | ✅ | ✅ |
| private | ❌ | ❌ | ✅ |

### 狀態可變性

| 修飾符 | 讀取鏈上狀態 | 修改鏈上狀態 |
| --- | --- | --- |
| view | ✅ | ❌ |
| pure | ❌ | ❌ |
| （無） | ✅ | ✅ |

**設計模式**：`external` + `internal` 分層

```solidity
// 外部接口：權限校驗
function mint(address to, uint256 amount) external onlyOwner {
    _mint(to, amount);
}

// 內部邏輯：實際狀態變更
function _mint(address to, uint256 amount) internal {
    balances[to] += amount;
    totalSupply += amount;
    emit Mint(to, amount);
}
```

這樣設計的好處：

1.  外部函數專注權限控制
    
2.  內部函數可被多個外部函數複用
    
3.  測試更方便
    

* * *

## 錯誤處理三兄弟

| 機制 | 用途 | Gas 特性 |
| --- | --- | --- |
| require | 條件校驗 + 錯誤訊息 | 字串訊息增加 gas |
| revert | 回滾，可不返回訊息 | 較省 gas |
| assert | 「不可能失敗」的斷言 | 失敗消耗剩餘 gas，現已少用 |

**Gas 優化方向**：用 custom error 取代 require 的字串

```solidity
// ❌ 傳統寫法
require(amount > 0, "Amount must be greater than zero");

// ✅ 優化寫法
error InvalidAmount();
if (amount == 0) revert InvalidAmount();
```

* * *

## Modifier：前置條件的優雅抽象

```solidity
modifier onlyOwner() {
    require(msg.sender == owner, "Not owner");
    _;  // 繼續執行原函數
}

// 使用
function mint(address to, uint256 amount) external onlyOwner {
    // 只有 owner 能執行到這裡
}
```

**應用場景**：

-   權限控制（onlyOwner, onlyAdmin）
    
-   重入保護（nonReentrant）
    
-   狀態檢查（whenNotPaused）
    

* * *

## 事件（Event）：鏈上到鏈下的橋樑

```solidity
event Transfer(address indexed from, address indexed to, uint256 value);
event Approval(address indexed owner, address indexed spender, uint256 value);

// 觸發事件
emit Transfer(msg.sender, to, amount);
```

**indexed 的意義**：最多 3 個參數可加 indexed，用於鏈下快速檢索。

**為什麼事件重要**：

1.  DEX 等協議依賴事件監聽同步狀態
    
2.  比頻繁讀 storage 更高效
    
3.  區塊瀏覽器的交易紀錄就是解析事件
    

* * *

## 助記詞陷阱的兩種實現機制

這部分很有意思，課堂上提問者問的問題：

### 機制一：合約層權限限制

```solidity
modifier onlyOwner() {
    require(msg.sender == owner, "Not owner");
    _;
}

function transfer(address to, uint256 amount) external onlyOwner {
    // 只有 owner 能轉出
}
```

結果：你導入私鑰後看到餘額，但無法轉出（不是 owner）。

### 機制二：抢跑 Bot

1.  Bot 監聽目標地址的所有交易（透過事件）
    
2.  偵測到有人轉入 gas
    
3.  立即用更高 gas fee 發起交易，搶先轉走資金
    
4.  利用的是 mempool 的可見性 + gas priority
    

* * *

## unchecked：謹慎使用的優化手段

Solidity 0.8+ 預設開啟整數溢位檢查。

```solidity
// 確定不會溢位時，可用 unchecked 省 gas
unchecked {
    counter++;
}
```

**我的看法**：初學階段不要碰這個。安全第一。

* * *

## 學習路線建議

1.  **先理解 EVM**：storage/memory/calldata 的差異
    
2.  **手寫 ERC20**：在 Remix 部署，給朋友轉測試代幣
    
3.  **閱讀協議源碼**：
    
    -   Uniswap V3：AMM 邏輯、tick 機制
        
    -   Aave：借貸邏輯、storage 優化
        
4.  **關注 storage 設計**：這是合約優化的核心
    

* * *

## 我的反思

1.  **EVM 的確定性執行** 與密碼學中的確定性函數有相似的設計哲學——相同輸入必須產生相同輸出，否則共識無法達成。
    
2.  **storage 的 Merkle 結構**（雖然課上沒講）與我之前學的 Merkle Tree 直接相關——這是理解輕客戶端驗證、狀態證明的基礎。
    
3.  **事件的設計**讓我想到鏈上/鏈下的邊界問題——鏈上狀態是 trustless 的，但鏈下監聽事件的系統呢？這裡有安全研究的空間。
    

### 安全視角的觀察

課上提到的幾個點值得深入：

1.  **重入攻擊**：單線程順序執行不代表沒有重入問題。外部調用可能觸發惡意合約的 fallback，再次調用原合約。
    
2.  **授權機制的風險**：infinite approval 是常見的 UX 優化，但也是安全隱患。
    
3.  **modifier 的正確使用**：modifier 只是語法糖，不當使用可能造成意料之外的行為。
    

* * *

## 課程筆記速查表

```
EVM 四大區：Stack / Storage / Memory / Calldata
Storage 成本：初始化 ~20,000 gas | 修改 ~5,000 gas

值類型：bool, uint/int, address, bytes32
引用類型：string, 動態陣列, bytes, mapping

可見性：external > public > internal > private
狀態：view（只讀）| pure（無狀態）| 無（可寫）

錯誤：require（帶訊息）| revert（不帶）| assert（斷言）
事件：indexed 最多 3 個，用於鏈下檢索

modifier：前置條件抽象，避免重複 require
unchecked：跳過溢位檢查，慎用
```
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->










与马铃薯还有功夫小马同学打算组织一场X SPACE活动，完成"**从 0 到 1 策划、组织、复盘一场活动"这个任务，按照实习手册的sop依序完成了确定活动背景与目标、准备流程按时接节点拆解(T-5至T-4天:启动准备)。**

在群里跟大家一起讨论了中本聪的真身

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/ARZER-TW/images/2026-01-19-1768837554767-image.png)

感觉这个Order of the Satoshi很有MPC的感觉。澳本聪坚称自己是中本聪所以他可能真的持有一份 share但他始终无法用创世区块私钥签名，单一 share 无法生成有效签名，其他成员保持沉默不愿配合凑齐门限，中本聪「消失」后私钥再也没动过代表成员们无法/不愿协调合作。从密码学角度看，这个阴谋论反而变得有点技术上的 plausibility 了。非常有趣哈哈哈。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->












1.將昨天撰寫的文章再度修改後上傳至x  
  
2.完成"**确立岗位目标意向，提供四周学习规划"** 這個任務  

* * *

## 崗位目標意向

**Blockchain Security Research（區塊鏈安全研究）**

選擇這個方向的原因：

-   我是先接觸密碼學才接觸區塊鏈，對共識機制、底層協議這些東西genuinely 有興趣，這是最重要的原因——做自己喜歡的事才能持續
    
-   根據分享會內容，安全審計屬於技術崗中薪資天花板高、且不易被 AI 取代的領域
    
-   目前產業對懂底層原理的安全研究員有結構性缺口，符合「精英短缺」的趨勢
    

* * *

## 四周學習規劃

在參與實習計畫的同時，我會從安全研究的角度來學習課程內容，並利用課餘時間補充相關知識：

### Week 1：基礎能力對齊與 NFT 實踐 + 安全視角建立

| 項目 | 內容 |
| --- | --- |
| 實習課程 | 以太坊與 Solidity 核心概念回顧、NFT 合約實踐 |
| 安全補充 | 開始練習 Damn Vulnerable DeFi 前 5 題，從攻擊者角度理解合約漏洞 |
| 輸出 | 針對 NFT 合約常見漏洞（如 reentrancy、mint 邏輯缺陷）整理筆記 |

### Week 2：合約工程化與 DeFi 入門 + 協議安全分析

| 項目 | 內容 |
| --- | --- |
| 實習課程 | Ethers.js / Hardhat 工具鏈、Uniswap V2 架構與核心邏輯 |
| 安全補充 | 從安全角度閱讀 Uniswap V2：理解 price oracle manipulation、flash loan attack 的原理 |
| 輸出 | 整理一份「Uniswap V2 潛在攻擊面」的學習筆記 |

### Week 3：協議機制與系統設計 + 真實漏洞研究

| 項目 | 內容 |
| --- | --- |
| 實習課程 | AMM 機制進階、Uniswap V3、黑客松項目開發 |
| 安全補充 | 挑一個真實的 DeFi exploit（如 Euler Finance 或 Curve reentrancy）做深度研究 |
| 輸出 | 開始撰寫漏洞分析文章草稿，用 Foundry 嘗試復現攻擊 |

### Week 4：綜合項目實踐 + 公開輸出

| 項目 | 內容 |
| --- | --- |
| 實習課程 | DEX / DeFi 綜合項目實踐、技術崗位能力模型 |
| 安全補充 | 完成漏洞分析文章，包含：漏洞根因、攻擊流程、PoC 程式碼、預防方式 |
| 輸出 | 發布到 Mirror 或個人 blog，同步在 Twitter/X 發摘要，更新 GitHub repo |

* * *

## 四周後希望達到的目標與成就

1.  **完成實習計畫課程**，具備參與黑客松的工程能力
    
2.  **有一篇可展示的深度漏洞分析文章**（含 Foundry PoC 程式碼）
    
3.  **建立安全研究視角**：能從攻擊者角度審視 DeFi 協議
    
4.  **開始建立公開軌跡**：GitHub 有相關 repo、Twitter 有技術輸出紀錄
    

* * *
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->













# 從 Cypherpunks 到比特幣：區塊鏈背後的政治理想

## 前言：技術之外的問題

如果你已經在區塊鏈領域待了一段時間，你大概聽過很多技術名詞：共識機制、智能合約、Layer 2、ZK-Rollup。你可能也寫過 Solidity，部署過合約，甚至參加過幾場 Hackathon。

但你有沒有想過一個問題：**為什麼我們需要區塊鏈？**

這個問題聽起來很基本，但我發現大多數人的答案都停留在技術層面：「因為去中心化」、「因為不可篡改」、「因為透明」。這些答案都對，但它們只回答了「是什麼」，沒有回答「為什麼」。

為什麼去中心化是重要的？為什麼我們要花這麼大的力氣，犧牲效能和便利性，只為了讓系統沒有中心？

這個問題的答案不在技術裡，而在技術背後的政治理想裡。

這篇文章是我的學習筆記，試著追溯比特幣和區塊鏈的思想源頭。我會聚焦在兩個主題：**Cypherpunks 的世界觀**，以及**比特幣的設計選擇如何體現這些價值觀**。

* * *

## 第一章：Cypherpunks 的世界觀

### 1.1 密碼學的「解禁」

要理解 Cypherpunks，我們需要先理解一個歷史背景：在 1970 年代以前，**密碼學是軍事機密**。

這不是誇飾。在美國，密碼學被歸類為「軍火」(munitions)，受到和武器相同的出口管制。研究密碼學需要政府許可，發表密碼學論文可能觸犯法律。普通人根本接觸不到強加密技術。

這個情況在 1970 年代開始改變。1976 年，**Whitfield Diffie** 和 **Martin Hellman** 發表了一篇劃時代的論文，提出了「公鑰密碼學」(public-key cryptography) 的概念。這個概念後來成為我們今天所有網路安全的基礎——從 HTTPS 到比特幣。

Diffie 和 Hellman 不是軍方人員，他們是學術界的研究者。他們的論文把密碼學從軍事領域帶到了公眾視野。

但這只是開始。真正讓密碼學成為大眾工具的，是接下來的「加密戰爭」(Crypto Wars)。

### 1.2 加密戰爭與 PGP

1991 年，一個叫 **Phil Zimmermann** 的軟體工程師做了一件事：他寫了一個叫 **PGP (Pretty Good Privacy)** 的軟體，讓任何人都可以加密自己的電子郵件，然後把它免費發布到網路上。

這在今天聽起來沒什麼大不了。但在當時，這是一個激進的行為。

因為美國政府把強加密軟體視為「軍火」，出口強加密軟體等同於出口武器。Zimmermann 把 PGP 放到網路上，理論上就是向全世界「出口」了軍火。

美國政府對 Zimmermann 展開了長達三年的刑事調查。他面臨著可能的牢獄之災。

Zimmermann 和他的支持者採取了一個巧妙的抗議方式：他們把 PGP 的原始碼印成一本書出版。書籍受到美國憲法第一修正案的保護，屬於言論自由的範疇。如果程式碼印成書就是合法的，為什麼同樣的程式碼以電子形式存在就是違法的？

這個案子最終在 1996 年不了了之，政府撤銷了調查。但它確立了一個重要的先例：**程式碼是一種言論，受到言論自由的保護。**

Zimmermann 後來說了一句被廣泛引用的話：

> 「如果隱私被定為犯罪，只有罪犯才會擁有隱私。」

這句話概括了整個 Cypherpunk 運動的核心信念。

### 1.3 1992 年的聚會

1992 年，在加州灣區，三個人開始定期在其中一人的家裡聚會。他們是：

-   **Eric Hughes**：數學家，柏克萊畢業
    
-   **Timothy C. May**：Intel 前首席科學家，已經財務自由
    
-   **John Gilmore**：Sun Microsystems 的早期員工，也是財務自由的狀態
    

這三個人有一個共同的擔憂：**數位時代的隱私正在消失。**

他們看到的趨勢是：隨著越來越多的通訊轉移到數位管道，政府和企業將擁有前所未有的監控能力。每一封電子郵件、每一筆電子交易、每一次網路瀏覽，都可能被記錄、被分析、被用來對付你。

在那個年代（1992 年！），這聽起來可能有點偏執。網路才剛剛起步，大多數人連電子郵件都沒用過。但三十年後的今天，當我們知道了稜鏡計畫 (PRISM)、知道了 Cambridge Analytica、知道了大型科技公司如何追蹤我們的每一個點擊，這些擔憂顯得相當有先見之明。

這個小型聚會很快擴展成一個郵件列表。他們給自己取了一個名字：**Cypherpunks**。這個詞是 **cipher**（密碼）和 **punk**（龐克）的結合，由 Jude Milhon 創造，帶有一種反體制的意味。

到了 1994 年，這個郵件列表已經有 700 個訂閱者。到 1997 年，超過 2000 人。這些人包括數學家、密碼學家、程式設計師、律師、自由意志主義者，還有一些我們後來會聽到的名字：Julian Assange（WikiLeaks 創辦人）、Hal Finney（比特幣第一筆交易的接收者）、Adam Back（Hashcash 發明者）、Wei Dai（b-money 提出者）、Nick Szabo（智能合約概念的提出者）。

### 1.4 密碼朋克宣言

1993 年 3 月 9 日，Eric Hughes 發表了 **《A Cypherpunk's Manifesto》**（密碼朋克宣言）。這份文件只有大約 1500 字，但它奠定了整個運動的思想基礎。

宣言的開頭就點出核心：

> 「Privacy is necessary for an open society in the electronic age.」 （隱私是電子時代開放社會的必要條件。）

注意 Hughes 的用詞：他說的是「開放社會」(open society)，不是「封閉社會」。這回應了一個常見的誤解：追求隱私的人是想要躲藏、是有什麼見不得人的事。

Hughes 的論點恰恰相反：**正是因為我們想要一個開放、自由的社會，我們才需要隱私。**

為什麼？因為沒有隱私的社會是一個人人自危的社會。當你知道你的每一句話、每一個行為都可能被記錄和審視，你會開始自我審查。你會避免說有爭議的話、避免探索非主流的想法、避免和「敏感」的人來往。

這不是理論上的擔憂。看看歷史上的極權社會：東德的史塔西 (Stasi)、蘇聯的 KGB，它們最強大的武器不是逮捕異議人士，而是讓每個人都知道自己可能被監視。這種「全景監獄」(panopticon) 效應會讓整個社會陷入寒蟬效應。

隱私是自由思考、自由表達、自由結社的前提。

**隱私不是保密**

宣言接著做了一個重要的區分：

> 「Privacy is not secrecy. A private matter is something one doesn't want the whole world to know, but a secret matter is something one doesn't want anybody to know. Privacy is the power to selectively reveal oneself to the world.」
> 
> （隱私不是保密。私人的事是你不想讓全世界知道的事，秘密的事是你不想讓任何人知道的事。隱私是選擇性向世界揭露自己的權力。）

這個區別值得細想。

**保密** (secrecy) 是「我不想讓任何人知道這件事」。 **隱私** (privacy) 是「我有權選擇要讓誰知道這件事」。

舉個例子：你的薪水。你可能願意告訴你的配偶、你的會計師、也許還有你最親近的朋友。但你不希望它被公開在網路上讓所有人看到。這不是因為你的薪水有什麼見不得人，而是因為你想要控制這個資訊的流向。

再舉一個例子：你在商店買東西。你願意讓店員知道你買了什麼（這是交易必要的資訊），但你不希望店員知道你的全名、住址、過去的所有購買紀錄、你的政治傾向、你的健康狀況。

隱私是關於**控制權**。是關於你有權決定自己的資訊要分享給誰、在什麼情境下分享。

**不能依賴別人來保護你**

Hughes 接著提出一個很現實的觀察：

> 「We cannot expect governments, corporations, or other large, faceless organizations to grant us privacy out of their beneficence.」
> 
> （我們不能期待政府、企業或其他大型組織會出於善意給予我們隱私。）

政府和企業有太多理由想要獲取你的資訊：

-   政府想要監控以維護「國家安全」和「公共秩序」
    
-   企業想要你的數據以便更精準地向你投放廣告
    
-   執法機構想要後門以便「打擊犯罪」
    

這些理由聽起來都很正當。但問題是：一旦監控的基礎設施建立起來，它就可能被濫用。今天用來「打擊恐怖主義」的工具，明天可能被用來打壓政治異議。今天用來「改善用戶體驗」的數據收集，明天可能被賣給出價最高的人。

Hughes 的結論是：**如果你想要隱私，你必須自己保護它。**

**「Cypherpunks write code」**

這是整份宣言最著名的一句話：

> 「Cypherpunks write code. We know that someone has to write software to defend privacy, and since we can't get privacy unless we all do, we're going to write it.」
> 
> （密碼朋克寫程式碼。我們知道必須有人寫軟體來捍衛隱私，而且除非我們都這樣做否則無法獲得隱私，所以我們要寫。）

它的意思是：**不要請願，不要遊說，不要期待別人來保護你。直接用技術建造保護隱私的工具。**

為什麼是程式碼而不是法律？因為法律可以被修改、可以被忽視、可以被選擇性執行。但數學不會。如果你的訊息被正確加密，沒有任何法院命令可以強迫數學規律改變。

這是一種非常務實的行動主義。Cypherpunks 不是在象牙塔裡空談理想，他們真的寫了很多程式碼：

-   **PGP**：電子郵件加密（Phil Zimmermann）
    
-   **Tor**：匿名瀏覽網路
    
-   **BitTorrent**：去中心化檔案分享（Bram Cohen）
    
-   **Hashcash**：工作量證明系統（Adam Back）
    
-   **b-money**：去中心化數位貨幣的提案（Wei Dai）
    
-   **Bit Gold**：另一個數位貨幣提案（Nick Szabo）
    

這些專案不是紙上談兵。它們是真實運作的軟體，被數百萬人使用。

### 1.5 在比特幣之前的嘗試

比特幣不是憑空出現的。在 Satoshi Nakamoto 之前，已經有很多人嘗試過創造「數位現金」。

**David Chaum 與 DigiCash（1989）**

最早的嘗試者是密碼學家 David Chaum。他在 1989 年創立了 DigiCash 公司，試圖創造一種可以匿名使用的數位貨幣。

Chaum 發明了一種叫做「盲簽章」(blind signature) 的技術。簡單來說，它讓銀行可以「簽署」一筆數位貨幣，證明它是有效的，但銀行卻無法追蹤這筆貨幣後來被用在哪裡。這就像銀行在你的鈔票上蓋章，但蓋章的時候眼睛是閉著的。

DigiCash 曾經很接近成功。微軟曾經想要把它整合到 Windows 95 中，但談判破裂了。最終 DigiCash 在 1998 年破產。

DigiCash 失敗的原因很多，但有一個根本問題：**它還是中心化的。** 它依賴一個公司來運營，而這個公司可以倒閉、可以被收購、可以被政府關閉。

**Wei Dai 與 b-money（1998）**

1998 年，Wei Dai 在 Cypherpunks 郵件列表上發表了 b-money 的提案。這是第一個描述「去中心化數位貨幣」概念的文件。

b-money 的核心想法是：不需要中央機構，讓網路中的所有參與者共同維護一個帳本，記錄誰擁有多少錢。

Wei Dai 提出了兩個版本的協議。第一個版本要求所有參與者都維護一份帳本副本——這正是比特幣後來採用的方式。第二個版本只讓一部分參與者（他稱為「伺服器」）維護帳本，其他人可以驗證。

b-money 沒有被實作出來，但它的影響是深遠的。比特幣白皮書引用的第一篇參考文獻就是 Wei Dai 的 b-money。

**Adam Back 與 Hashcash（1997）**

Adam Back 發明的 Hashcash 原本是用來對抗垃圾郵件的。它的想法是：如果你要發送一封郵件，你必須先完成一個需要消耗計算資源的「謎題」。對於正常用戶來說，為一封郵件花幾秒鐘的計算時間不是問題。但對於想要發送數百萬封垃圾郵件的人來說，這個成本就太高了。

這個「工作量證明」(Proof of Work) 的概念後來成為比特幣的核心機制。

**Nick Szabo 與 Bit Gold（1998）**

Nick Szabo 是另一個關鍵人物。他提出了「智能合約」(smart contract) 的概念，也設計了一個叫 Bit Gold 的數位貨幣系統。

Bit Gold 的設計和比特幣驚人地相似：使用工作量證明來「創造」新的貨幣單位，用密碼學簽章來驗證所有權，用去中心化的網路來防止雙重支付。

Szabo 說他從來沒有實作 Bit Gold，因為他沒有解決一些技術問題。這些問題後來被 Satoshi Nakamoto 解決了。

**Hal Finney 與 RPOW（2004）**

Hal Finney 是另一個重要人物。他是 PGP 的早期開發者之一，也是 Cypherpunks 郵件列表的活躍成員。

2004 年，他發布了 RPOW (Reusable Proof of Work)，一個可以把工作量證明的「結果」當作代幣轉移的系統。

四年後，當 Satoshi Nakamoto 在郵件列表上發表比特幣白皮書時，Hal Finney 是最早回應的人之一。他下載了比特幣軟體，成為最早的礦工之一，也是第一筆比特幣交易的接收者——Satoshi 發給他 10 個比特幣。

### 1.6 比特幣的誕生

2008 年 10 月 31 日，一個化名 **Satoshi Nakamoto** 的人在密碼學郵件列表上發表了一篇論文：《Bitcoin: A Peer-to-Peer Electronic Cash System》（比特幣：一種點對點的電子現金系統）。

這個郵件列表的訂閱者大多是 Cypherpunks 或對密碼學感興趣的人。Satoshi 顯然知道他的聽眾是誰。

論文的摘要開宗明義地說：

> 「A purely peer-to-peer version of electronic cash would allow online payments to be sent directly from one party to another without going through a financial institution.」
> 
> （一個純粹的點對點電子現金版本將允許線上支付直接從一方發送到另一方，而不需要經過金融機構。）

這正是 Cypherpunks 追求了十幾年的目標。

兩個月後，2009 年 1 月 3 日，Satoshi 挖出了比特幣的第一個區塊——創世區塊 (Genesis Block)。

六天後，他發布了比特幣軟體的第一個版本。

比特幣就這樣誕生了。沒有 ICO，沒有風險投資，沒有行銷活動。只是一篇論文和一個可以運行的軟體。

WikiLeaks 創辦人 Julian Assange 曾說，Satoshi Nakamoto 是一位密碼朋克。這是有道理的。比特幣不是憑空出現的創新，它是幾十年密碼學研究和政治理想的結晶。

* * *

## 第二章：比特幣的設計選擇

比特幣白皮書是一份純技術文件，幾乎沒有談到政治。但當你理解了 Cypherpunk 的背景，你會發現比特幣的每一個重要設計選擇都不是任意的技術決定，而是特定價值觀的體現。

### 2.1 創世區塊的政治宣言

讓我們從創世區塊開始。

在比特幣的第一個區塊的 coinbase 交易中，Satoshi 嵌入了一段文字：

> **「The Times 03/Jan/2009 Chancellor on brink of second bailout for banks」**

這是 2009 年 1 月 3 日《泰晤士報》的頭條：財政大臣即將對銀行進行第二次紓困。

這不是隨機選擇的。這段文字有兩個作用：

**第一，它是時間戳記。** 它證明了這個區塊不可能在 2009 年 1 月 3 日之前被創建，因為那一天的報紙頭條不可能更早知道。這確保了 Satoshi 沒有「預挖」(pre-mine) 大量比特幣。

**第二，它是一個政治宣言。** 它明確表達了比特幣的立場：這是對 2008 年金融危機、對銀行紓困、對「大到不能倒」的回應。

2008 年金融危機的根源是什麼？簡單來說：銀行承擔了過度的風險，當風險爆發時，政府用納稅人的錢來拯救它們。風險被私有化（銀行賺取利潤），損失被社會化（全民買單）。

這正是 Cypherpunks 長期批評的「系統性權力失衡」的一個例子。普通人必須為自己的錯誤負責，但大型金融機構可以綁架整個經濟系統，迫使政府救援。

比特幣的設計試圖創造一個不同的系統：**沒有「大到不能倒」，因為沒有中心；沒有人可以被紓困，因為沒有人有特權。**

### 2.2 固定供給：對抗通貨膨脹

比特幣最著名的特性之一是它的供給上限：**永遠只會有 2100 萬個比特幣。**

這個數字是寫死在協議中的。沒有任何人——沒有 Satoshi，沒有礦工，沒有任何政府——可以改變它。

為什麼要這樣設計？

要理解這個選擇，我們需要稍微談一下經濟學。

**奧地利學派的貨幣觀**

在經濟學中，有一個學派叫做「奧地利學派」(Austrian School)。這個學派的核心人物包括 Carl Menger、Ludwig von Mises、Friedrich Hayek 等人。

奧地利學派對貨幣有一個關鍵的觀點：**通貨膨脹不是自然現象，而是政策選擇。** 當政府印更多鈔票時，每一張現有鈔票的購買力就會下降。這是一種隱形的稅收，因為它把財富從持有貨幣的人轉移到最先獲得新印鈔票的人（通常是政府和銀行）。

1976 年，諾貝爾經濟學獎得主 Hayek 出版了一本小書，叫做《Denationalisation of Money》（貨幣的去國家化）。他的論點是：政府壟斷貨幣發行權是問題的根源。讓私人機構發行相互競爭的貨幣，最穩定、最不會貶值的貨幣會勝出。

歐洲中央銀行曾經在一份報告中指出：**比特幣的理論根源在於 Hayek 的這本書。**

比特幣的固定供給體現了奧地利學派的理念：好的貨幣應該是不會被任意增發的「硬通貨」(hard money)。黃金之所以被用作貨幣幾千年，正是因為它的供給不受任何人控制——你不能印黃金，你只能開採它，而開採成本很高。

比特幣試圖在數位世界中重現這種「稀缺性」。

**為什麼不能「適度通膨」？**

你可能會問：適度的通貨膨脹不是對經濟有好處嗎？主流經濟學不是說 2% 的通膨目標是健康的嗎？

這是一個複雜的辯論，我不會在這裡深入。但 Cypherpunks 和奧地利學派的觀點是：

1.  「適度通膨」是一個滑坡。一旦你接受政府可以控制貨幣供給，你就無法阻止它濫用這個權力。歷史上有太多政府因為戰爭、福利、貪腐而大量印鈔，最終導致惡性通膨。
    
2.  通膨即使「適度」，長期累積的效果也是巨大的。每年 2% 的通膨，30 年後你的貨幣購買力會減少約 45%。這不是中性的——它懲罰儲蓄者，獎勵借貸者。
    
3.  誰來決定「適度」是多少？這是一個巨大的權力。把這個權力交給一小群央行官員，就是把你的財富命運交到他們手上。
    

比特幣的設計選擇是：**不要相信任何人來「適度」管理貨幣供給。用數學規則取代人為裁量。**

### 2.3 工作量證明：為什麼要「浪費」能源？

比特幣最受爭議的設計之一是它的共識機制：**工作量證明 (Proof of Work, PoW)**。

批評者說 PoW 浪費能源。這個批評從純技術角度看有一定道理——確實有更「高效」的共識機制，比如 Proof of Stake (PoS)。

但如果你理解 Cypherpunk 的思維，你會發現 PoW 的「浪費」恰恰是它的特點，而不是缺點。

**PoW 的本質**

PoW 要求礦工解決一道計算難題。這道難題的特性是：解決它很難（需要大量計算），但驗證答案很容易（只需要一次計算）。

具體來說，礦工需要找到一個 nonce 值，使得區塊頭的 hash 值小於一個目標值。由於 hash 函數的特性，沒有捷徑可以找到這個 nonce——你只能不斷嘗試，直到找到為止。

這意味著：**要產生一個有效的區塊，你必須消耗真實世界的資源（電力、硬體）。**

**這帶來什麼？**

PoW 創造了一個「昂貴的投票機制」。如果你想要影響比特幣網路——比如讓你的區塊被接受、讓你的版本成為「真相」——你必須投入大量資源。

這有幾個重要的後果：

**第一，攻擊成本很高。** 如果你想要篡改比特幣的歷史（比如雙重支付），你需要擁有超過全網 50% 的算力，並且持續投入資源來維持這個優勢。這在經濟上是極其昂貴的。

**第二，無需許可。** 任何人只要願意投入資源，就可以參與挖礦，不需要任何人的批准。沒有白名單，沒有 KYC，沒有「你必須是認證節點」。這是真正的開放參與。

**第三，把外部世界錨定到數位世界。** 這是最微妙但也最重要的一點。比特幣的安全性不是來自網路內部的規則（那些可以被更改），而是來自現實世界的物理定律（電力必須被消耗，熵必須增加）。

**與 PoS 的本質差異**

Proof of Stake 的運作方式不同。在 PoS 中，你的「投票權」來自於你持有多少代幣。持有越多代幣，你對下一個區塊的影響力就越大。

這聽起來更「高效」——不需要浪費電力。但它有一個根本的不同：**PoS 的權力來自於系統內部，PoW 的權力來自於系統外部。**

在 PoS 系統中，早期參與者和大戶天然擁有更大的影響力，而且這種優勢會自我強化（持有越多代幣 → 獲得越多獎勵 → 持有更多代幣）。這不一定是壞事，但它確實創造了一種特權結構。

在 PoW 系統中，你的歷史地位不重要。即使你是最早的參與者，如果你今天不投入算力，你今天就沒有影響力。每一天都是新的競爭。

Satoshi 選擇 PoW，是因為它更符合「無需許可」、「沒有特權」的理念。

**能源消耗的另一個視角**

關於「浪費能源」的批評，還有另一個視角值得考慮。

比特幣的能源消耗是公開的、可測量的。你可以計算出全網消耗多少電力。但傳統金融系統的能源消耗呢？銀行的辦公大樓、資料中心、ATM 網路、現金運輸車、金庫⋯⋯這些加起來消耗多少能源？很少有人計算過，但它肯定不是零。

更重要的是：**能源消耗本身不是問題，問題是這些能源換來了什麼。**

如果比特幣的能源消耗換來的是一個任何人都可以使用、沒有人可以控制、不會被通膨侵蝕的貨幣系統，那這個交換是否值得？這是一個價值判斷，不是純技術問題。

### 2.4 UTXO 模型：一種不同的記帳方式

比特幣使用一種叫做 **UTXO (Unspent Transaction Output)** 的模型來追蹤餘額。這和傳統的「帳戶餘額」模型非常不同。

**帳戶模型 vs UTXO 模型**

在傳統銀行系統（以及以太坊），資料庫記錄的是「帳戶餘額」：

```
Alice: 100 元
Bob: 50 元
```

當 Alice 轉 30 元給 Bob，系統會：

1.  把 Alice 的餘額減少 30，變成 70 元
    
2.  把 Bob 的餘額增加 30，變成 80 元
    

這很直觀，但它有一個特性：**這是一種「狀態快照」式的記帳。** 你只知道當前的狀態（Bob 有 80 元），但資金流動的路徑被簡化成了數字的加減。

比特幣的 UTXO 模型不同。在比特幣中，不存在「Alice 擁有多少錢」這樣的紀錄。只有散落在區塊鏈上的「未花費交易輸出」(UTXO)。

每一筆 UTXO 都明確記載：

-   它的金額是多少
    
-   它可以被誰花費（用公鑰鎖定）
    
-   它是從哪一筆交易產生的
    

當 Alice 想要轉帳給 Bob，她必須：

1.  找到一筆（或多筆）屬於她的 UTXO 作為「輸入」
    
2.  「銷毀」這些輸入
    
3.  創建新的 UTXO 作為「輸出」——一個給 Bob（轉帳金額），一個給自己（找零）
    

**交易鏈：可追溯的歷史**

UTXO 模型的結果是：**每一筆比特幣都帶著它的完整歷史。**

每一個 UTXO 都連結到創建它的交易。那筆交易的輸入又連結到更早的交易。你可以順著這條鏈一路回溯，最終回溯到這些比特幣被「挖出來」的那個區塊——也就是它們作為區塊獎勵被創造出來的那一刻。

這就像每一張鈔票上都印著它曾經經過的每一隻手。在物理世界中，這是不可能的。但在比特幣的數位世界中，這是預設的。

這意味著比特幣是一個**完全可審計的帳本**。所有交易都是公開的，任何人都可以查閱、驗證。

**透明與隱私的張力**

等等，這不是和 Cypherpunk 的隱私理念矛盾嗎？

表面上看是的。但 Satoshi 的設計有一個巧妙之處：**比特幣把隱私放在身份層，而不是交易層。**

交易是公開的，但交易者可以是匿名的。你的比特幣地址不需要連結到你的真實身份。你可以為每一筆交易創建新的地址。只要你的地址和身份之間沒有連結，你的交易就是匿名的。

這和傳統金融系統相反。在傳統系統中，你必須實名開戶（KYC），但你的交易細節對外部是不透明的。你知道你轉帳給了銀行帳號 12345，但你不知道 12345 的交易歷史。

比特幣翻轉了這個模式：交易歷史對所有人透明，但帳戶可以匿名。

這是 Eric Hughes 所說的「選擇性揭露」的一種實現：你可以選擇揭露你的身份（如果你想要建立聲譽或接受支付），也可以選擇保持匿名。

當然，這種匿名性不是完美的。如果你在交易所用實名買比特幣，或者如果你的交易模式被分析，你的匿名性可能被破解。這是後來隱私幣（如 Monero、Zcash）試圖解決的問題。

### 2.5 為什麼故意「低效」？

比特幣每秒只能處理約 7 筆交易。以太坊大約 15-30 筆。相比之下，Visa 可以處理超過 24,000 筆。

很多人認為這是區塊鏈的致命缺陷。「這怎麼可能取代現有的支付系統？」

但這個批評誤解了比特幣的設計目標。

**Social Scalability：一種不同的擴展觀**

**Nick Szabo**——智能合約概念的提出者、Bit Gold 的設計者——在 2017 年寫了一篇重要的文章，叫做 "Money, Blockchains, and Social Scalability"。他提出了一個關鍵的概念：**社會可擴展性 (social scalability)**。

Szabo 的論點是：我們習慣用「計算效率」來衡量技術，但這不是唯一重要的維度。還有一個維度叫做「社會可擴展性」——一個系統可以容納多少互不信任的參與者，而不會崩潰。

傳統的支付系統（如 Visa）計算效率很高，但社會可擴展性很低。為什麼？因為它依賴於一系列「可信第三方」：銀行、清算所、監管機構。這些機構彼此認識、彼此簽約、彼此監督。如果你想加入這個網路，你需要獲得許可、通過審核、簽署協議。

這個系統運作得很好——只要你在它的信任網路之內。但如果你在這個網路之外呢？如果你是一個不被銀行服務的人？如果你在一個金融基礎設施不完善的國家？如果你的帳戶因為政治原因被凍結？

比特幣的設計選擇是：**犧牲計算效率，換取社會可擴展性。**

任何人都可以加入比特幣網路，不需要任何人的許可。你不需要通過 KYC，不需要有銀行帳戶，不需要任何機構為你「擔保」。你只需要遵守協議的規則——而這些規則是公開的、可驗證的、對所有人一視同仁的。

**Hal Finney 的早期洞見**

比特幣第一筆交易的接收者 **Hal Finney** 在 2010 年的一篇論壇貼文中，提出了一個有先見之明的觀點：

> 「Bitcoin itself cannot scale to have every single financial transaction in the world be broadcast to everyone and included in the block chain. There needs to be a secondary level of payment systems which is lighter weight and more efficient.」
> 
> （比特幣本身無法擴展到讓世界上每一筆金融交易都廣播給所有人並記錄在區塊鏈上。需要有第二層的支付系統，更輕量、更高效。）

Finney 很早就理解：**比特幣不是要處理每一筆咖啡交易。它是一個結算層，一個信任的錨點。**

你可以把它想像成黃金。歷史上，黃金不是用來買麵包的。日常交易用的是銀幣、銅幣、紙鈔。但這些日常貨幣的價值，最終錨定在黃金上。銀行之間的大額結算，用的是黃金。

比特幣想要扮演的是類似的角色——一個數位時代的「最終結算層」。日常的小額支付可以在更高效的第二層進行（如 Lightning Network），但最終的信任根基是比特幣主鏈。

**為什麼選擇低效？**

從這個角度看，比特幣的「低效能」是有意為之的。為了實現真正的去中心化和抗審查，它必須確保：

1.  **任何人都可以運行全節點。** 如果區塊太大、數據增長太快，只有大型機構才能負擔運行節點的成本。這就會走向中心化。所以比特幣選擇限制區塊大小。
    
2.  **沒有人可以被切斷。** 使用 P2P 網路和「流言傳播」(gossiping) 機制，訊息像病毒一樣在節點之間傳播。沒有中央伺服器可以被關閉。
    
3.  **系統極難被攻擊。** 犧牲效率換取冗餘和韌性。每一個全節點都保存完整的交易歷史。即使 90% 的節點被摧毀，剩下的 10% 仍然可以重建整個網路。
    

這是一個有意識的權衡：**用效能換取信任最小化。**

Szabo 用一個精妙的比喻來說明這一點：比特幣就像一輛「計算上的悍馬」——油耗很高、速度不快，但它能開過任何地形。傳統支付系統就像一輛跑車——在公路上很快，但只能在公路上跑。

如果你的目標是讓人買咖啡更方便，比特幣是一個糟糕的選擇。但如果你的目標是創造一個任何人都可以使用、沒有人可以控制的價值轉移系統，這些設計選擇就很合理。

零售支付的問題可以在更高的層級解決（如 Lightning Network）。但基礎層必須是去中心化的、抗審查的。如果基礎層被妥協了，上面建造的一切都會被妥協。

* * *

## 第三章：反思與當代張力

### 3.1 比特幣「背叛」了密碼朋克嗎？

如果早期的 Cypherpunks 看到今天的加密貨幣產業，他們會怎麼想？

2014 年，密碼朋克元老 **Amir Taaki** 寫了一份 《libbitcoin Manifesto》。他在其中批評比特幣社群「去華盛頓請求使用加密貨幣的許可」，並質問：

> 「Why would you willingly give your power away? Are you stupid?」
> 
> （為什麼你要主動放棄你的力量？你是傻了嗎？）

這反映了一個真實的張力。

早期 Cypherpunks 的理念是「不要請求許可，直接建造」。密碼學是武器，程式碼是抵抗。你不需要說服任何人同意你——你只需要寫出不可阻擋的軟體。

但隨著加密貨幣成為主流，越來越多的參與者開始尋求：

-   監管合規（「我們需要清晰的法規」）
    
-   機構認可（「我們需要 ETF」）
    
-   與傳統金融的整合（「我們需要銀行接受我們」）
    

這是「背叛」嗎？還是務實的演化？

我沒有標準答案。但理解這個張力，有助於你在這個領域裡找到自己的位置。

一方面，完全拒絕與現有系統互動，可能意味著永遠停留在邊緣。如果你想要比特幣被數十億人使用，你可能需要一些妥協。

另一方面，每一次妥協都可能侵蝕比特幣存在的理由。如果比特幣變成另一個需要政府批准、需要銀行合作、需要實名認證的支付系統，它和 PayPal 有什麼區別？

**區塊大小之戰**

這個張力在 2015-2017 年的「區塊大小之戰」(Block Size War) 中達到高峰。

爭論的核心是：比特幣的區塊大小應該增加嗎？

一派（主要是企業和礦工）認為應該增加。更大的區塊意味著更多的交易、更低的手續費、更好的用戶體驗。

另一派（主要是核心開發者和早期社群）認為不應該。更大的區塊意味著運行全節點更昂貴，會導致中心化。保持小區塊是保護去中心化的必要代價。

這場爭論最終導致了比特幣現金 (Bitcoin Cash) 的分叉。但比特幣主鏈維持了 1MB 的區塊限制（後來通過 SegWit 有效增加到約 2-4MB）。

這場戰爭不只是技術之爭，它是價值觀之爭：**效能和用戶體驗更重要，還是去中心化和抗審查更重要？**

主鏈的選擇是後者。這是否「正確」，取決於你認為比特幣的目的是什麼。

### 3.2 程式碼作為政治行動

回到 Hughes 的那句話：「Cypherpunks write code.」

這句話不只是說「我們用技術解決問題」。它有一個更深的含義：**程式碼是一種權力，寫程式碼是一種政治行動。**

當你選擇用什麼共識機制、如何處理隱私、如何分配治理權——你在做政治決定，即使你不這樣描述它。

很多工程師喜歡說「技術是中立的，重要的是人怎麼使用它」。但 Cypherpunks 的歷史告訴我們，這個說法過於簡化。

-   **選擇 PoW 還是 PoS**，決定了權力來自於外部資源還是內部持有量
    
-   **選擇透明帳本還是隱私保護**，決定了誰可以審視交易
    
-   **選擇大區塊還是小區塊**，決定了誰可以參與驗證
    

這些選擇不是中性的。它們創造了不同的權力結構，服務於不同的利益。

這不是說工程師應該變成政治家。但我們應該意識到：**我們建造的系統會影響權力如何分配。**

如果你不思考這些問題，別人會替你思考。而他們的答案可能不是你想要的。

* * *

## 結語：回到最初的問題

我寫這篇文章，是因為我發現自己在學習區塊鏈技術的時候，經常缺少一個「為什麼」的框架。

我可以學會怎麼寫智能合約、怎麼設計代幣經濟學、怎麼優化 gas 費用。但這些技能服務於什麼目的？如果我不知道這個領域的根本理念，我怎麼判斷一個專案是在推進這些理念，還是在背離它們？

理解 Cypherpunks 和比特幣的思想根源，給了我一個判斷的標準。

當我看到一個專案宣稱「去中心化」，我會問：它真的是去中心化的嗎？誰控制升級？誰可以審查交易？如果有一個實體可以單方面改變規則，那它就不是去中心化的。

當我看到一個專案追求「高效能」，我會問：這個效能是以什麼為代價的？如果代價是只有少數節點可以驗證，那這個效能是否值得？

當我看到一個專案尋求「監管合規」，我會問：這個合規是否侵蝕了系統的抗審查性？如果政府可以命令凍結帳戶，那這個系統和銀行有什麼區別？

這不是說每個專案都必須是「純粹的」Cypherpunk 項目。不同的專案可以有不同的定位。一個面向企業的區塊鏈和一個面向個人隱私的區塊鏈，可以有完全不同的設計選擇。

但你應該知道你在做什麼。你應該知道你的選擇意味著什麼。你應該能夠清楚地說出：這個專案服務於誰，為了什麼目的，做出了什麼權衡。

這就是理解這些政治理想的用處。不是為了變成意識形態狂熱者，而是為了成為一個有意識的建造者。

* * *

## 延伸閱讀

如果這篇文章引起了你的興趣，以下是一些延伸閱讀：

**原典文件（強烈建議閱讀原文）：**

-   Eric Hughes, "A Cypherpunk's Manifesto" (1993) — 只有一頁，幾分鐘就能讀完
    
-   Timothy C. May, "The Crypto Anarchist Manifesto" (1992) — 同樣很短
    
-   Satoshi Nakamoto, "Bitcoin: A Peer-to-Peer Electronic Cash System" (2008) — 比特幣白皮書
    

**歷史與脈絡：**

-   Steven Levy, "Crypto: How the Code Rebels Beat the Government" (2001) — 加密戰爭的完整紀錄
    
-   Nathaniel Popper, "Digital Gold" (2015) — 比特幣早期歷史
    

**經濟學背景：**

-   Friedrich Hayek, "Denationalisation of Money" (1976) — 貨幣去國家化的論證
    
-   Saifedean Ammous, "The Bitcoin Standard" (2018) — 從奧地利學派視角分析比特幣
    

**技術深入：**

-   Andreas Antonopoulos, "Mastering Bitcoin" — 前兩章適合所有人，後面是技術細節
    
-   密碼龐克台灣的資源整理：[https://cypherpunks-core.github.io/](https://cypherpunks-core.github.io/)
    

* * *

_本文是我的學習筆記，如有錯誤歡迎指正。_
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->















## 2026/01/16 學習筆記

今天重讀了余哲安老師的〈兩個記憶工程的故事（三）〉和比特幣白皮書。

老實說，我接觸區塊鏈也有一段時間了，寫過 Solidity、參加過 hackathon、做過一些安全相關的研究。但我發現自己一直把比特幣當成「技術」在看——共識機制怎麼運作、UTXO 結構長什麼樣、PoW 的數學原理是什麼。

今天讀完才意識到，我好像從來沒有認真問過一個更根本的問題：**中本聰為什麼要做這個東西？**

不是「技術上怎麼做到」，而是「為什麼覺得需要做」。

* * *

文章最後談到「比特幣系統的靈魂：無主權、密碼學、反叛」，這個部分對我衝擊最大。

區塊鏈不只是一個技術方案，它背後有一整套政治哲學在驅動。中本聰不是單純想做一個「更快更便宜的支付系統」，他想解決的是一個更深層的問題——人類社會如何在不依賴權威的情況下建立信任？

我以前從來沒有從這個角度思考過。

* * *

接下來我想做的事情是往回挖。

比特幣不是憑空出現的，它有它的思想源頭：密碼朋克運動、奧地利經濟學派、自由意志主義。中本聰在白皮書裡引用了 Wei Dai 的 b-money，創世區塊裡嵌入了英國銀行紓困的新聞標題。這些線索都指向一個更大的脈絡。

我想搞清楚這個脈絡。不只是為了寫文章，而是覺得如果要真正理解區塊鏈在做什麼，就不能只看技術層面。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
















## 2026/01/15 學習筆記

### 今天做了什麼

晚上參加了 LXDAO 的「AI 機器基礎概念」分享會（20:00-21:00），分享會提到了 ERC-8004 這個 AI Agent 信任框架，聽完後我對它的聲譽系統有些疑問，跟 Claude 深入討論後延伸到 AI Agent 對區塊鏈安全的影響。另外，針對昨天提交 PR 時遇到的側邊欄 404 問題，今天花時間研究了底層原因。

* * *

### 分享會延伸討論：從 ERC-8004 到 AI Agent 安全

**ERC-8004 是什麼？**

分享會提到了 ERC-8004，這是今年八月由 MetaMask、Ethereum Foundation、Google、Coinbase 一起提出的 AI Agent 信任框架，包含三個註冊表：身份（Identity）、聲譽（Reputation）、驗證（Validation）。

**我的疑問：聲譽系統能防女巫嗎？**

聽完之後我在想，這套聲譽系統真的能防 Sybil Attack 嗎？仔細看會發現 ERC-8004 只定義了「有哪些 Agent」、「可以存什麼數據」，但真正的核心問題——Reputation 怎麼算、怎麼防刷分——都還沒定義。

攻擊者完全可以：

1.  批量註冊一堆「合法」Agent
    
2.  養號累積 Reputation
    
3.  同時發動攻擊後換下一批
    

所以 ERC-8004 比較像是「基礎設施的基礎設施」，真正的防禦能力要等後續的 Reputation 算法設計。

**延伸思考：AI Agent 大規模化後的攻擊問題**

順著這個思路往下想，如果聲譽系統擋不住，那 AI Agent 大規模化之後會發生什麼事？

AI Agent 跟傳統 Bot 不一樣的地方在於，它可以動態判斷攻擊時機、模擬正常用戶行為、甚至形成群體智能。這讓傳統的 spam 攻擊變得更難防。想像一下，如果有人把攻擊能力包裝成「服務」賣出去（AaaS），不懂技術的人付錢就能癱瘓一條鏈——這在 Web2 已經很成熟了，區塊鏈版本只是時間問題。

**那直接 Ban 非註冊 AI 呢？**

這條路也走不通：

1.  鏈上交易只有 from、to、data、signature，根本分不出是人還是 AI
    
2.  協議層要 ban 就得硬分叉，這違背 Permissionless 的設計
    
3.  DCA 機器人、清算機器人、止損單這些正常的自動化也會被誤殺
    

**比較務實的方向**

討論下來的結論是：與其想辦法「阻止進入」，不如「讓攻擊變得不划算」。像是加密 Mempool 讓攻擊者拿不到資訊優勢、本地費用市場讓攻擊影響範圍受限、或是分層服務讓驗證過的用戶有優先權但不強制所有人驗證。

之後可以深入看看 zkKYC 或 Reputation 算法設計——特別是怎麼設計一個真正能抵抗 Sybil Attack 的聲譽系統。

* * *

### 📚 VuePress 側邊欄 404 問題研究

**問題背景**

為 Web3-Internship-Handbook 提交繁體中文翻譯（PR #33）後，線上側邊欄連結出現 404，但本地測試正常。

**問題根因**

notes.ts 配置：

```typescript
{ text: '區塊鏈基礎概念', link: 'part1/blockchain-basic.md' }
```

Markdown frontmatter：

```yaml
permalink: /zh-tw/blockchain-basic/
```

**Plume notes.ts 的 link 解析邏輯：**

| link 格式 | Plume 處理方式 | 生成的 href |
| --- | --- | --- |
| part1/blockchain-basic.md | 檔案路徑推導 → 轉 .html | /zh-tw/part1/blockchain-basic.html |
| blockchain-basic/ | 直接當路徑使用 | /zh-tw/blockchain-basic/ |

這裡的關鍵是：VuePress 本身的 internal link 機制理論上會尊重 permalink，但 **Plume theme 的 notes 模組在處理** `.md` **連結時，只用檔案路徑推導，不會去查該頁有沒有設 permalink**。

所以即使 Markdown 檔案設了 `permalink: /zh-tw/blockchain-basic/`，Plume 還是傻傻地用檔案路徑算出 `/zh-tw/part1/blockchain-basic.html`。

結果：

```
側邊欄指向：/zh-tw/part1/blockchain-basic.html  ← 不存在
頁面位置：  /zh-tw/blockchain-basic/            ← 在這裡
```

**為什麼本地沒發現？**

| 環境 | 結果 |
| --- | --- |
| npm run docs:dev | ✅ 正常 |
| npm run docs:build + npx serve dist | ✅ 正常 |
| GitHub Pages | ❌ 404 |

可能原因：

-   本地構建緩存（`node_modules/.cache` 或 `.vuepress/.cache`）保留了舊的解析結果
    
-   GitHub Actions 的 Node.js / VuePress 版本與本地不同
    
-   測試時序問題：本地測試時可能配置狀態跟最終提交的不一致
    

核心教訓：本地測試通過不代表線上一定正常，線上環境是最終驗證標準。

**解決方案**

將 link 從檔案路徑格式改為 permalink 格式：

```typescript
// ❌ 問題配置
{ link: 'part1/blockchain-basic.md' }

// ✅ 正確配置  
{ link: 'blockchain-basic/' }
```

原則：當 Markdown 有自訂 permalink 時，sidebar link 應該直接對齊 permalink，不要用 `.md` 檔案路徑。

**提交前驗證 SOP**

```bash
# 1. 生產構建
npm run docs:build

# 2. 靜態伺服器預覽
npx serve docs/.vuepress/dist

# 3. 驗證清單
#    □ 關鍵頁面正常載入
#    □ 側邊欄 href 值正確（DevTools 檢查）
#    □ 點擊連結回傳 HTTP 200

# 4. 合併後追蹤
#    □ 確認 GitHub Actions 部署成功
#    □ 線上實際測試關鍵功能
```

**關鍵學習點**

1.  **Plume notes 的行為**：處理 `.md` 連結時只看檔案路徑，不查 permalink
    
2.  **permalink 決定頁面位置**：sidebar link 必須配合，不能假設檔案路徑 = 最終 URL
    
3.  **環境差異**：開發環境可能比較寬容，生產環境最嚴格
    
4.  **兩種解法**：要嘛 link 對齊 permalink，要嘛乾脆不用 permalink 讓路徑完全由檔案位置決定
    

**相關 PR**

| PR | 內容 | 狀態 |
| --- | --- | --- |
| #33 | 新增繁體中文翻譯 | ✅ 已合併 |
| #35 | 修復側邊欄 404 | 🚀 待合併 |

* * *

### 今天的收穫

研究方向上，從 ERC-8004 的聲譽系統出發，發現 Sybil-resistant 的 Reputation 算法設計是個有價值的研究缺口。

技術上搞懂了 VuePress Plume 的路由解析機制，也理解了開發環境「太寬容」反而會埋坑這件事。

PR #35 等合併後要追蹤一下線上是否正常。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->


















## 今日完整工作總結

* * *

### 任務一：提交繁體中文翻譯 PR #33

**目標**：為 Web3-Internship-Handbook 新增繁體中文（zh-TW）版本

**完成步驟**：

1.  **Fork & Clone** 專案，建立 `feat/add-traditional-chinese` 分支
    
2.  **複製簡體中文目錄** `docs/zh` → `docs/zh-tw`
    
3.  **使用 OpenCC (s2twp)** 批量轉換簡繁體
    
4.  **更新配置文件**：`config.ts`、`plume.config.ts`、`notes.ts`、`navbar.ts`
    
5.  **修復側邊欄消失問題**：將所有 `.md` 文件的 `permalink: /zh/...` 改為 `/zh-tw/...`
    
6.  **提交 PR** 並成功合併
    

**PR #33 成果**：

-   205 個檔案變更
    
-   維護者 @brucexu-eth 評論：「非常的贊，辛苦」
    
-   Commit `3497ebe` 合併到 main
    

* * *

### 任務二：發現並修復部署後 404 問題

**現象**：PR 合併部署後，線上側邊欄連結 404，但本地開發模式正常

**根因**：

```
notes.ts 側邊欄連結        Markdown permalink
part1/blockchain-basic.md  ≠  /zh-tw/blockchain-basic/
```

**修復**：移除 `notes.ts` 中的 `part1/`、`part2/` 子目錄前綴

**驗證方式**：

```bash
npm run docs:build              # 生產構建
npx serve docs/.vuepress/dist   # 模擬真實部署
```

* * *
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->



















* * *

# Web3 運行原理分享會筆記

**日期**：2026/01/13 **講者**：Bruce 老師

* * *

## 錢包與密鑰

這部分算是基礎複習，但 Bruce 老師講得蠻清楚的。

私鑰就是 64 位的十六進制字串，本質上就是一個很大的隨機數。重點是：丟了就沒了，沒有「忘記密碼」這種選項。這點對新手來說其實蠻反直覺的，畢竟 Web2 的世界什麼都能重設。

助記詞是私鑰的人類可讀版本，12 或 24 個單詞，透過 BIP-39 標準可以確定性地派生出一堆金鑰對。所以備份助記詞等於備份整個錢包樹。

地址生成的流程：

```
私鑰 → (ECDSA) → 公鑰 → (Keccak-256, 取後 20 bytes) → 地址
```

這個單向性是整個系統安全的基礎。

* * *

## 交易機制

一筆交易包含：

-   操作內容（value、data）
    
-   Gas 費用
    
-   Nonce（防重放攻擊）
    
-   發送方 / 接收方地址
    

數位簽章這塊沒什麼新東西，就是用私鑰簽、公鑰驗。但 Bruce 老師強調的點我覺得蠻重要：這套機制讓我們可以在「不信任任何人」的前提下驗證交易合法性。

Gas 費用的部分，本質上就是付給驗證者的處理費，會根據網路壅塞程度浮動。

* * *

## 區塊鏈運作流程

交易的生命週期大概是這樣：

簽名 → 送到 RPC 節點 → 進交易池排隊 → 驗證者打包 → 廣播驗證 → 上鏈

以太坊現在每 12 秒一個區塊，狀態會從「等待中」→「已納入」→「已確定」。達到最終確定大概要等 12-13 分鐘（64-96 個區塊），之後基本上不可能被回滾。

以太坊 2022 年 The Merge 之後已經全面轉 PoS，質押取代算力競爭。

* * *

## 智能合約

定義上就是部署在鏈上的程式碼，自動執行、（通常）不可修改。

Bruce 老師提到的「Code is Law」這個概念我覺得是 Web3 最核心的價值主張——規則寫在程式碼裡，公開透明，不需要信任任何中介。當然實際上沒那麼理想（可升級代理合約到處都是），但至少理論上是這樣。

合約的狀態存在鏈上，透過交易觸發執行，結果全網可驗證。

* * *

## 協議治理

EIP 流程：提案 → 討論 → 審查 → 測試 → 實施

以太坊有多種客戶端實作（Geth、Nethermind、Besu...），這種多樣性是刻意的，避免單一實作出漏洞就整個網路掛掉。

目前網路規模大概 13,700 多個節點、970,000 多個驗證者。

* * *

## Web3 特性

技術面：去中心化、無需許可、抗審查、開源

隱私這塊要特別注意：地址是「假名」而非「匿名」。所有交易都是公開的，透過鏈上分析還是可以追蹤。要真正的隱私得靠額外的技術（Tornado Cash 之類的，雖然現在被制裁了）。

另外 Bruce 老師有提到 Web3 是個很跨領域的東西，資訊科學、密碼學、經濟學、博弈論、甚至社會學都有涉及。這點我蠻認同的，純技術的角度其實只能理解一部分。

* * *

## Q&A 整理

### AI Agent 支付標準（為鑫）

問 ERC-402、ERC-8004 這些 AI Agent 支付標準的進展。

Bruce 老師說技術上已經準備好了，但應用場景不明確，現在做出來也沒人用。算是個可行的最小可行產品，但時機未到。

### 區塊打包跟搶跑（Lucia）

兩個問題：

1.  區塊大小跟交易金額的關係？ → 沒有直接關係，區塊大小是看 Gas 消耗量。一筆簡單轉帳可能就 21,000 Gas，複雜的合約調用可能幾百萬 Gas。
    
2.  私鑰洩露後的搶跑怎麼運作？ → 付更高 Gas 費用就能優先被打包。因為 12 秒出一個塊，攻擊者跟受害者的交易在同一個時間窗口內競爭。現在有些安全服務會監測異常轉帳，自動幫你搶跑把資產轉走（當然是轉到安全的地方）。
    

### PoS 中心化風險（evo）

問大交易所質押太多會不會造成中心化。

Bruce 老師說確實是個問題，建議自己跑驗證節點或用 Lido 這種分散式質押協議。自己質押收益也比較高（交易所會抽成）。另外以太坊在研究把質押門檻從 32 ETH 降到 1 ETH。

### Web3 定位（Charlie）

問 Web3 到底是要顛覆 Web2 還是只是金融遊戲。

Bruce 老師的觀點我覺得蠻務實的：不是顛覆，是補充。Web3 解決的是 Web2 處理不好的問題——跨境支付（給沒有銀行帳戶的人用）、抗審查通訊、隱私保護（零知識證明相關技術）。

他還提到兩種參與者：

-   **Degen**：投機仔，追短期利益
    
-   **Regen**：關注可持續發展跟真實價值
    

建議跟 Regen 混，創造長期價值。這點我蠻認同的。

### 被盜資產追回（Changyuan）

私鑰洩露被偷了 1.3 ETH，問能不能追回。

Bruce 老師很直接：基本上追不回來，這就是「抗審查」的代價。除非錢進了中心化交易所，可以報案請交易所凍結。大額案件（百萬級）可以找慢霧這種安全公司追蹤，但也不保證能拿回來。

預防比補救重要：

-   用硬體錢包
    
-   不要在網路上輸入私鑰或助記詞
    
-   定期上 [revoke.cash](http://revoke.cash) 檢查授權
    
-   大額資產用多簽錢包
    

* * *

## Bruce 老師留的思考題

### 1\. 自託管模式下怎麼平衡安全性跟便利性？

這題其實是個取捨的問題。

目前比較務實的做法是分層管理：

-   **冷錢包**（硬體錢包、離線裝置）放大額資產，安全性優先
    
-   **熱錢包**（MetaMask 之類）放日常使用的小額，便利性優先
    

另外 MPC 錢包跟智能合約錢包（像 Safe、Argent）也是一個方向。把私鑰拆成多個碎片分散保管，或是用社交恢復機制，讓你可以透過預設的監護人來恢復錢包。這樣就不用在「一組助記詞控制一切」跟「什麼都不能做」之間二選一。

帳戶抽象（ERC-4337）也在解決這個問題，讓錢包可以有更靈活的驗證邏輯，比如設定每日轉帳上限、白名單地址免驗證等等。

但說到底，自託管的本質就是「你對自己負責」，這個責任沒辦法完全外包。

### 2\. 沒有中心化機構時，公共資源（區塊空間）怎麼公平分配？

目前主流的答案就是市場機制——出價高的優先。EIP-1559 之後，以太坊的 Gas 費用分成基礎費（燒掉）跟優先費（給驗證者），讓定價更可預測一點。

但「公平」這件事本身就很難定義。價高者得對有錢人有利，但如果不用價格機制，要怎麼決定誰的交易比較重要？

一些可能的方向：

-   **二層網路**：把大部分交易搬到 L2，降低主鏈的競爭壓力
    
-   **PBS（提議者與建構者分離）**：把出塊權跟排序權分開，減少 MEV 對一般用戶的傷害
    
-   **公平排序協議**：像 Chainlink 的 FSS，用去中心化的方式決定交易順序
    

我自己覺得完全的「公平」大概不存在，但可以透過機制設計讓不公平的程度降低、讓 MEV 的利潤更分散。

### 3\. 抗審查網路裡怎麼處理有害內容？

這題是我覺得最難的。

區塊鏈的核心價值之一就是抗審查，但抗審查也意味著沒辦法刪除有害內容。IPFS 上的東西一旦有人儲存就會一直在，鏈上的交易更是永久存在。

目前能做的大概是：

-   **應用層過濾**：前端介面可以選擇不顯示某些內容，但底層資料還是在
    
-   **社會共識**：社群自發性地不儲存、不傳播有害內容
    
-   **經濟激勵**：讓儲存有害內容的成本變高（但這也可能傷害正常用途）
    

但這些都不是真正的「解決」，只是把問題推到別的層次。

我的想法是，這可能是去中心化系統必須接受的取捨。你沒辦法同時擁有「沒有人能審查內容」跟「有害內容會被移除」。最終可能還是要靠鏈下的法律、社會規範來處理，而不是期待協議層能解決所有問題。

### 4\. 去中心化協作下怎麼做到公平分配？

這題跟 DAO 治理、代幣經濟學設計都有關。

常見的分配機制：

-   **代幣投票**：持幣越多話語權越大，但容易變成富人統治
    
-   **平方投票/募資**：降低大戶的影響力，但容易被女巫攻擊
    
-   **追溯性公共物品募資**：事後根據貢獻分配，像 Optimism 的 RPGF
    

Coordinape 那種點對點的互評機制也蠻有趣的，讓團隊成員互相分配獎勵。

但「公平」本身就是主觀的。對早期貢獻者公平？對後來加入的人公平？對投入時間多的人公平還是對產出影響大的人公平？這些很難用一個機制同時滿足。

我自己的觀察是，目前做得比較好的專案通常是：

1.  規則透明，大家事前知道分配邏輯
    
2.  有迭代的空間，可以根據實際情況調整
    
3.  社群有足夠的發言權來反映不滿
    

完美的公平分配機制大概不存在，但透明度跟可調整性可以讓系統在不公平出現時有修正的能力。

* * *

_2026/01/13_
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




















## 今日學習心得：經歷要說對的語言

最近一直在想，我現在做的事情對未來到底有沒有幫助。打了很多黑客松，也拿了一些不錯的名次，但越打越覺得這跟我真正想走的方向有落差。今天在群裡聊了這件事，有些收穫。

* * *

### 問題出在哪

黑客松本質上是在做產品，不是在做安全研究。但我真正感興趣的是密碼學、共識機制、底層協議這些東西。履歷上卻完全看不出來這點，因為我一直在打黑客松，累積的都是產品類的經歷。

如果之後想往 Blockchain Security Research 走，現在累積的東西可能不夠對口。

* * *

### 別人怎麼說

wachi 老師提到，做自己喜歡的事情才能持續，這是最大的槓桿。wachi 老師還分享了一個帳號 @hhhx402，說可以研究他從八月到現在怎麼快速建立起聲望的。

另外就是要讓研究被看見，不能只是自己默默做。發文是一個方式，但重點不是當 KOL，而是建立一個可以被驗證的研究軌跡。

* * *

### 我自己想了想

與其繼續打黑客松累積不對口的經歷，不如現在開始轉向。幾個可以做的事：

-   挑一個過去的 exploit（像 Euler、Curve 那些）做一篇完整的分析，用 Foundry 把攻擊復現出來，附 PoC
    
-   去 Code4rena 或 Sherlock 打幾場審計比賽，有 findings 就是最直接的證明
    
-   如果對共識機制有興趣，可以研究 L2 的 fraud proof 或跨鏈橋的安全模型，這些是現在的產業痛點
    

一篇有深度的漏洞分析，比十篇淺層科普有用。

* * *

### 下一步

克服一下不愛發文的習慣，先從一篇深度分析開始。不用管流量，重點是把東西做出來、讓人看到我在研究什麼。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
