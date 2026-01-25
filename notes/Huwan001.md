---
timezone: UTC+8
---

# Huwan001

**GitHub ID:** Huwan001

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生
請大家多多指教！

## Notes

<!-- Content_START -->
# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->
小挑戰合約內容

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleStorage {
    // 這個變量用來存一個數字
    uint256 public myNumber;

    // 寫入函數：把數字存進去 (需要發送交易，消耗 Gas)
    function store(uint256 _num) public {
        myNumber = _num;
    }

    // 新增：把數字 +1 的函數 (同樣會修改狀態，消耗 Gas)
    function increment() public {
        myNumber = myNumber + 1;
    }

    // 讀取函數：把數字讀出來 (view 函數，不消耗 Gas)
    function retrieve() public view returns (uint256) {
        return myNumber;
    }
}
```

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-25-1769351538211-image.png)

-   **第一個版本 (**`SimpleStorage`**)：** 函數名稱叫 `store`。
    
-   **第二個版本 (**`SimpleCounter`**)：** 為了配合計數器邏輯，函數改名成了 `setNumber`。
    
-   ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-25-1769351712364-image.png)

這邊顯示的兩個合約

主要原因有

1.  區塊鏈的「不可竄改性」(Immutability)
    
2.  你部署了兩個不同的合約類別
    

## 重新佈署

-   在傳統開發（Web2）中，如果你發現程式碼有錯，你只需修改程式碼並「存檔/重啟」，原本的網頁就會直接更新。
    
-   在區塊鏈（Web3）中，因為  
    1.不可篡改性 (Immutability) -> 合約一旦部署到鏈上，它的程式碼（邏輯）就**永遠無法更改**。  
    2.**新合約 = 新地址**：當修改了程式碼，就必須將這份「新邏輯」重新發送到區塊鏈上。這會產生一個**全新的合約地址**，而舊的合約會帶著舊的邏輯繼續存在於原地址，互不干擾。(就等於是兩者是獨立的概念，因為已經產生，具有不可串改性，他會一直存在區塊練上面，包含刪除都無法)
    

重新佈署 = 發布一個全新的合約 (取代舊的**功能**
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->

今日稍微休息了一下 沒怎麼念書  
把前幾天學到的東西簡單整理了一下  
還有聽了兩場同學們舉辦的AMA
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->


今日參與線上共學 聽了 evo Swift、ECHO兩位老師分享了許多行業八卦，吃了一些瓜 哈哈  
提到了在找崗位時需要注意什麼事情，分享了老師們自身或是身邊友人的例子。  
內容涵蓋

-   技術詐騙-寫的開源代碼被不肖人士拿去利用被抓走
    
-   社交平台-TG被駭，被拿去冒用該人公司信息去向親近的友人騙取資金等
    
-   項目割韭菜-很多項目目的在於上所，割一波就跑路、換個皮膚再回來  
    

晚上參與分享會，每位同學都有準備投影片，都很認真，講的面向也有許多，對於使用找崗位同學分享的主題，可以去抓取網上公開信息，針對這些崗位去做分析，看適不適合自己  
和用AI建立知識庫，可以多多避免拖延症，將yt、B站、X書籤等等想說收藏起來到時候再看的內容，由AI協助整合並列出重點

比較Remix、foundry、Hardhat並分析自己使用的體驗，

-   **Remix** 適合拿來練習、試錯及學習，我這個階段挺適合先從這個去入手，但到項目結構較大以後，使用可能會需要進行 自動化回歸、腳本化、多鏈部屬，驗證上麻煩、不適合交互。
    
-   **Foundry** 測試較快、工具鏈緊湊，但在Win下使用需要較強的命令習慣，還有團隊統一性，否則容易在運行上出問題。
    
-   **Hardhat** 是交付型工程框架，這就適合用在大型項目，適合團隊協作多鏈上線，具有自動化測試系統、腳本化部屬交互可付現複用、多鏈配置集中管理、插件生態完善，JG以及Typescript語言前後端協作也較為流暢。
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->





今日恰好想到加密算法和一般算法有何不同，於是稍微查詢了一下

-   一般算法(General Algorithms)  
    主要用於 **解決問題**、\*\*完成任務，\*\*著重於效率、準確性  
    目的性以效率為主
    
-   加密演算法(Encryption Algorithms)  
    著重在於**機密性**、**完整性**、**不可否認性**  
    追求安全性
    

|   | 一般演算法 | 加密演算法 |
| --- | --- | --- |
| 目的 | 提高效率、解決邏輯任務 | 保護資訊、防止未經授權存取 |
| 可逆性 | 性通常是單向處理（如算出結果後不一定能推回輸入) | 必須是可逆的（加密後必須能解密還原） |
| 關鍵要素 | 邏輯 與 數學結構 | 密鑰 (Key) 是核心 |
| 面對攻擊 | 考慮的是極端資料情況 | 考慮的是惡意的人為破解 |

加密演算法本質上也是一連串的**數學運算**，(ex: 矩陣換位、模運算、位元互斥、XOR)牽扯到密碼學，  
_是將純文字訊息轉換為加密密文的程序。_

原始訊息(明文plaintext) ->**(透過約定的密鑰key)加密** -> 密文(cyphertext)  
需透過 **(約定的密鑰key)** 才可還原明文

### GPT舉例:

-   **一般演算法：** 就像是一台「果汁機」，你給它水果（輸入），它按照步驟把它變成果汁（輸出）。
    
-   **加密演算法：** 就像是一個「保險箱」，你把信件放進去並上鎖。只有擁有正確鑰匙的人，才能按照特定的步驟（演算法）把保險箱打開並讀取內容。
    

# 在電腦科學界中，程式 = 資料結構 + 演算法

-   **資料結構（Data Structure）：** **_How 怎麼存資料_**  
    就像是\*\*「容器」\*\*。它是研究資料在記憶體中如何「擺放」與「組織」。
    
    -   _例子：_ 陣列 (Array)、鏈結串列 (Linked List)、堆疊 (Stack)、樹 (Tree)。
        
-   **演算法（Algorithm）：** **_How 怎麼處理資料_**  
    就像是\*\*「食譜」\*\*。它是處理資料的「步驟」。
    
    -   _例子：_ 如何在陣列中找到最大值？如何把打亂的數字排好？
        

在撰寫程式中，要先掌握一些底層概念，基礎的**資料結構**，

1.  **陣列 (Array)：** 最基本的結構，資料在記憶體中是連續排列的
    
2.  **字串 (String)：** 本質上是字元的陣列（舉例:顏色碼 `#FFFFFF` 其實就是一種字串）。
    
3.  **物件/字典 (Object/Dictionary/Hash Map)：** 透過「鍵值對」(Key-Value) 來儲存資料，查找速度極快。
    
4.  **堆疊與佇列 (Stack & Queue)：** 處理「先進後出」或「先進先出」的邏輯。
    

而對於開發者來說，加密領域屬於比較進階的範圍了，不屬於基礎資料結構的部分

-   **_知道_** 什麼時候該用加密（例如存密碼不能用明文）。
    
-   **_知道_** 如何「呼叫」現成的加密函式庫（如 AES, SHA-256）。
    
-   **_不需要_** 親自去寫加密的數學邏輯。**在資安界有一句名言：「永遠不要發明自己的加密演算法」**，因為那極度困難且容易出錯。
    

### 搭配昨天線上共學的部分，

-   **初學者的技術棧：** 文件建議初學者統一使用 **JavaScript/TypeScript (JS/TS)** 生態系 。這能讓你同時處理前端與後端邏輯 。
    
-   **資料結構的應用：** 在 Web3 中，你需要處理像是 `viem` 或 `ethers.js` 等庫 ，這涉及到如何操作物件 (Object) 和陣列 (Array) 來與區塊鏈互動。
    
-   **加密演算法的位置：** 雖然會議提到智能合約工程師需要理解 EVM 機制和安全漏洞 ，但對於初學者，建議先從編寫 **Solidity + JavaScript** 的套利或量化腳本開始 ，此時你只需要「調用」現成的加密函數，而不需要深入研究演算法底層。
    

全站工程師除了基本智能合約(但需求較少，維護頻率較低)，**React/Vue** 前端與 **Node.js** 後端技能也是相當重要的。  
進階部分涉及 **底層公鏈** 與 **基礎設施** ，可以考慮進修**Go、Rust 或 C++**，那涉及**分散式計算**、**共識機制**與 **P2P 消息傳遞**等等電腦科學的「深水區」知識 。  
(ps.分散式運算是將一套軟體分散在不同電腦或伺服器上執行，透過網路互相連接，達成共同的任務。)

# (以下為今日線上共學同學的筆記檔案)

![0122線上共學.PNG](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-22-1769096764843-0122____.PNG)![ad8d07eca762b2a8bce8e5ed1535b704.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-22-1769096996237-ad8d07eca762b2a8bce8e5ed1535b704.jpg)
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->










## 今日線上共學環節，聽了Wachi老師給了建議。

### 1.關於小白要如何在黑客松中找到idea，去進行實作，如何給予AI下指令。

比如說告訴AI你想要參與一個黑客松，但目前對於方向沒有想法，希望他以一個小白的角度，搭配去中心化，使用什麼技術為核心，能夠去製作一個可以實際應用的場景，針對上述資訊思考一個主題，像這種方式是一個很好去做的方向，去跟AI溝通並去尋找自己想做的主題。

2.也分享了在Web3中找技術職的相關內容。

在做中學，小白在學習過程中可能會遇到許多困難導致堅持不下去，可以先學一個概念就好，看想做的應用實際會運用到的東西是什麼語言，再針對其去做較深入的鑽研。

Wachi老師建議不要直接學go，舉例可以學Typescript，前後端能夠一把抓，後端是JavaScripts，小白不要python入門，可以直接以solidity入門學習基礎，也可以直接去學鏈的開發。

實際建議部分，可以小白先了解solidity語言，去寫套利、量化腳本運用typescript費率套利、期限套利等等，賺錢後

花錢請人做高頻交易、MVE鏈上套利等等，只要告訴他們策略讓他們去開發。

純寫智能合約的工程師較難生存，建議往Web3全端開發的目標去前進，再更進階可以做合約審計工程師。

# (以下是同學整理的筆記)

![Day3线上共学会议纪要_page-0001.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-21-1769001276236-Day3_________page-0001.jpg)![Day3线上共学会议纪要_page-0002.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-21-1769001303678-Day3_________page-0002.jpg)![Day3线上共学会议纪要_page-0003.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-21-1769001457865-Day3_________page-0003.jpg)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->













**File Explorers 新增文件、管理文件夾**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-20-1768891210785-image.png)

\-

**Solidity Compiler 用來檢查代碼、並翻譯成機器語言**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-20-1768891238313-image.png)

\-

**Deploy & Run 把合約發到鏈上、進行交互**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-20-1768891253301-image.png)

\-

**Compiler** 版本號 > (代碼中=0.8.0) 即可

**Auto compile** 自動檢查錯誤

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-20-1768891277366-image.png)

運行代碼(綠色勾勾出現->可運行

將合約部屬至**Java虛擬機**上，以便我們**測試智能合約**，而非直接部署到以太坊

\-

**Environment**  
選擇環境，此處選Cancun  
(在**瀏覽器**裡跑的 ”假區塊鏈” ，速度極快)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-20-1768891337471-image.png)

\-

**Account**  
系統預設15個測試帳號，提供用戶進行切換並完成交互

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-20-1768891449999-image.png)

\-

\*\*Contract (合約)  
\*\*上方提供到代碼中設定的變量，用來存取後面會輸入的數字

```
contract SimpleStorage
```

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-20-1768891676815-image.png)

\-

1.     點擊 Deploy

2.、3. 出現綠色打勾並顯示SimpleStorage

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-20-1768892135168-image.png)

\-

**1.在Store中鍵入數字(寫操作) -> 寫入函數  
看見後台的日誌有紀錄顯示->模擬交易**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-20-1768918757646-image.png)

\-

**2.點擊retrieve，顯示”** **0: uint256: 888”**

**這是一個“讀操作”，點擊後返回結果**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-20-1768918862627-image.png)

-   **顏色分類:  
    蓝色随便点，橙色要花钱，红色是打钱。(哈哈**
    
-   **筆記:**  
    **Remix 會自動儲存檔案在歷史紀錄  
    but 重要代碼最好要保存到本地**
    

## 其他案例的代碼

```

// SPDX-License-Identifier: MIT
pragma solidity ^0.7.5;contract HelloWorld {
    string str = "HelloWorld";
    
    function Hello() public view returns(string memory){
        return str;
    }    function World() public view returns(string memory){
        return str;
    }
}
```

參照過後跟Draken老師的合約部屬範例差別在於

老師的是**動態修改**，這份案例為**靜態讀取**
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->























## 今日學習 Remix 的使用教程

## 下午參與Colearning

## 晚上觀看Luna老師的社區運營、活動策畫及執行課程

看見許多同學已經開始使用新上線的任務內容了，大家都好認真呀，許多運營的小夥伴紛紛創立了幾個不同的TG，有加了幾個進去圍觀，還有小夥伴實操代碼，雖然有遇到一些困難，但明日繼續，共學時間大家一起Debug的感覺也挺不錯的呀，
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->

























### 撰寫本週學習總結並發布X，內容如下

  
本週進行了由 @ETHPanda\_Org @LXDAO\_Official 舉辦為期一個月的學習營，目前來到第七日，課程極度扎實，每天都有1-2堂課的內容，每天邀請的講師都是相當專業的，

第一天由Emily老師，介紹目前Web3的行業內的崗位分佈，該如何去針對自己想要進行的工作去做一個努力的方向、不同職位間的差別，做為一個已經在業內擔任重要崗位的前輩來為我們做解惑。

第二天由Bruce老師，介紹了Web3的基本架構、運行原理等等，大多是之前聽過或是本來就知道的內容，但自己的知識來源偏零散，經由這堂課將這些碎片化的知識統整了起來。

第三天有兩位講師，

第一位是Adam老師，為我們科普了目前Web3的安全知識，舉了不少案例讓我們了解到黑客使用的手法有哪些，該如何建立自己的防範意識。

第二位鄧小宇律師站在一個的Web3專門律師的角色來，也是為我們舉例了許多經手過的大小案件，以及我們在從事Web3相關崗位時所需要注意的問題，畢竟這個領域在法律方面，都還是有跟監管機構有許多需要去調整或是避免的地方存在。

第四天Rick老師講述了AI Agents 如何從單純的聊天輔助轉化為能在 Web3 環境中獨立工作的實體。從傳統AI轉型成新型態，要透過哪些協議去進行升級以及進步。

第五天是同學們的分享會，每位分享的同學都有著自己的一套獨立見解，從中學習到了非常多，運營崗位以及技術崗位的分享，對我來說也都是講師級別的，許多內容也是值得我收藏起來反覆看個兩三遍的。

線上共學的部分，原先以為會是就大家一起念書，偶爾會有一兩個問題，但大家的提問以及互動都極其踴躍，其中有同學提到了我一直很在意的問題，就是我都感到我跟不上其他同學，令我覺得我不是孤單的，每個人都有自己的學習步調，與其每天想著要把所有內容都記錄下，不如自己選擇做自己能力所及的部分，按著自己的節奏來便行。

堅持輸出，努力在這條路上不斷成長！  
#ETHPanda #Web3Bootcamp #LXDAO

@ETHPanda\_Org @LXDAO\_Official @web3careerbuild @IntensiveCL

[https://x.com/jason8591/status/2012910758406029439?s=20](https://x.com/jason8591/status/2012910758406029439?s=20)
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->




























今日看見TG有同學需要轉測試幣，於是轉了一點測試幣給對方  
這兩天因為要搬家，學習進度可能稍微停滯，周一恢復  
但這 Web3的新知是不斷的，希望能夠找到即使沒有時間也能夠做到的事，  
為了之後如果真的進了 Web3 的行業做準備！

![殘酷共學轉錢測試.PNG](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-17-1768646770281-________.PNG)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->






























# 今日成功鑄造NFT

![NFT1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-16-1768539711931-NFT1.png)

# 完成領水 / 領取測試幣

![測試轉帳1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-16-1768539769473-____1.png)

# 創建錢包領取測試幣過程

## 使用Chrome加入Metamask錢包

-   創建的新錢包要先去加入 **Unichain Sepolia** 網路  
    點擊「**新增網路**」(Add network) ->「**手動新增網路**」(Add a network manually)。
    

|   |   |
| --- | --- |
| 網路名稱 (Network Name) | Unichain Sepolia |
| 新的 RPC URL | https://sepolia.unichain.org |
| 鏈 ID (Chain ID) | 1301 |
| 貨幣符號 (Currency Symbol) | ETH |
| 區塊瀏覽器 (Optional) | https://sepolia.uniscan.xyz |

初次創建錢包時，裡面不會有Unichain Sepolia 網路，要自己去手動添加，  
爬了一下文，發現蠻多博主有寫教學的，包含測試網路如何新增，以及如何領水，  
那我這邊就有個問題，想說最初的網路要怎麼找到呢?  
於是爬到在官方文檔是有最初的正式文件  
(這邊也是想說要知道如何拿到第一手消息，或是怕可能有些人給的是錯誤的)  
最原始的來源官方文件 -> [Network Information | Unichain Docs](https://docs.unichain.org/docs/technical-information/network-information)  
進去以後看到有兩條通道  
**1.Unichain Mainnet (主網)**  
是正式的版本，在上面的操作都是需要使用真實資產  
**2.Unichain Sepolia Testnet (測試網)**  
測試的網路，裡面的 SepoliaETH 是測試幣，用來開發測試、合約部屬操作等等練習用途。

**創建錢包的兩種方式**

1.  官方來源 (Unichain Docs)  
    
2.  網路聚合工具 (ChainList) -專門整理 EVM 相容鏈（如以太坊、Unichain、BSC 等）參數的工具。  
    
    |   | 使用 ChainList.org | 自己手動添加 (Add Manually) |
    | --- | --- | --- |
    | 難度 | 極低。搜尋網路名稱後，點擊「Add to MetaMask」即可自動填入。 | 較高。需要自己去官網找 RPC、Chain ID 等五個參數並一一複製貼上。 |
    | 準確性 | 極高。它會自動篩選正確的 Chain ID，避免你連錯網路（例如誤把測試網當成主網）。 | 存在風險。如果抄錯一個數字（例如 Chain ID 寫錯），可能會導致交易失敗或看不到資產。 |
    | 節點品質 | 自動優化。它會幫你測試哪一個 HTTPS 連結（RPC）速度最快、延遲最低。 | 固定不變。你填哪一個就用哪一個，如果該節點掛了，你得手動去更換網址。 |
    | 安全性 | 社群監督。它是一個被公認的安全聚合器，能防止你用到駭客提供的假節點（惡意節點）。 | 依賴來源。如果你從不明論壇抄來的參數，該節點可能會記錄你的 IP 或進行隱私追蹤。 |
    
      
    (以上表格為AI整理)  
    
3.  三方服務商 (QuickNode / Tenderly)  
    

-   **RPC URL**：告訴錢包要去哪一台伺服器讀取區塊鏈數據。
    
-   **Chain ID (1301)**：這是網路的唯一身份證，防止你把錢發錯到別條鏈（例如發到同樣是測試網的 Sepolia 或 Base）。
    
-   **區塊瀏覽器 (Uniscan)**：這是讓你可以點擊「查看交易詳情」時跳轉的網頁地址。  
    

## 尋找領水網站

初次創建錢包/領取，要先找到能夠給沒有餘額的錢包領取的領水網，或是找到人給你發一點測試幣

-   有些領水網站無法使用**主網餘額為0**的錢包領取
    

解決方法

1.  使用不需有餘額的網站領取
    
2.  找群友協助轉少量測試幣
    
3.  轉入真實資金當Gas fee
    

| 領水網站 | 網站 | 條件 | 限制 |
| --- | --- | --- | --- |
| Alchemy Sepolia Faucet | Faucet - Ethereum Sepolia \| Alchemy | 需註冊帳戶 | 0.5ETH / 天 |
| MetaMask / Linea | https://www.infura.io/faucet/sepolia | 需註冊帳戶(需要人機驗證) POH | 0.5ETH / 天 |
| Google Cloud | https://cloud.google.com/application/web3/faucet | 登入google帳戶 | 0.5ETH / 天 |
| Sepolia PoW Faucet | https://sepolia-faucet.pk910.de/ | 無須Gas fee | 挖礦方式最多2.5ETH / 天 |

## 許多測試網領水會有限制，為防止Bot大量洗幣，新創建的錢包會需要有主網ETH。

大量洗幣/囤積的可能性原因如下

1.  為了參與「空投潛力項目」  
    項目方有時會給予在測試網積極參與測試、提供數據的用戶「空投」，所以就會有矩陣號的產生，開機器人去領取，為了避免這個方式，所以會有一天只能領取多少的限制存在。
    
2.  測試幣的「黑市」交易  
    雖然測試幣應該是免費的，但當水龍頭領取的門檻變高（例如需要主網餘額）時，測試幣反而變得「稀缺」。  
    對於懶得去解任務或領取門檻不夠的開發者來說，他們可能會花一點點U去**大量的購買測試幣**來節省時間。這讓洗幣者有了獲利空間。
    
3.  大規模壓力測試與開發需求  
    **開發者需求：** 如果是一個大型開發團隊，需要部署複雜的智能合約或進行成千上萬次的自動化測試，每天消耗的測試幣量很大。  
    **惡意攻擊：** 有些人會利用大量測試幣對測試網路進行「垃圾郵件攻擊」（Spamming），意圖癱瘓測試環境或測試網路的極限。
    
4.  稀釋網路資源  
    有些測試網（如早期的 Goerli）因為總量有限，當少數人持有了大部分的測試幣時，普通用戶就領不到幣，這會迫使開發者轉向其他平台，或是變相推高了測試幣的「黑市價值」。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->


































今天參加了線上共學 Co-learning 時，有同學提出了對於目前感到焦慮的心理狀態，內容剛好與我目前的狀態契合，

看著別人撰寫的筆記內容嘎嘎豐富，許多同學的強度合我差了不只是一兩個檔次而已，好像是小學生在看著研究員的感覺，我緊緊追趕著的進度也沒法寫出多有內容的筆記。

每天群組的訊息也是極為豐富，生怕漏了任何一條重要消息，

但老師說到就好像學開車一樣，上路後要注意的有許多方向，前後來車，手腳要如何操作等等，不過最一開始時也是得從頭開始。

目前的狀態也是類似的，許多基本知識都不是很足，與其每天想著要把所有內容都記錄下，不如自己選擇做自己能力所及的部分，這次的目標是學習技術面，從0進步到0.5，就先朝著這個目標前行吧！  

運營部分，我自己學著做推特帳號做了半年左右時間，主要的方向是嘴嚕，

在做的過程當中，就只是為了要寫一些項目的介紹，不斷地用AI產出內容，自己做總結，但流量一直沒有起來，一月 $Brev 發空投時，看到幾位大V只發了一兩篇文章就拿到了幾千U的空投，覺得挺羨慕，但也不知從何調整，今日助教分享到可以參考那些大V的思路，應該要去總結一些自己的觀點，持續的做輸出，別人看到你的文章以後是覺得有價值的，才能夠願意跟你互關，並持續互動。

群友也有提到幾個博主可以做參考，奶子哥、在金、嗯哼、笑蒼生等等，get到了幾個很能夠花時間去鑽研的名單。

舉例殺破狼這個博主，他發的觀點是真的能夠讓他的粉絲跟著賺到錢的，也能夠從他的觀點去取得一些收穫。找一個該賽道的頭部KOL去研究他們的發文邏輯，將其當作榜樣，慢慢地去學習如何將自己的知識輸出是一個很重要的點

技術部分，我們應該做的不只是一個"價值多少的技術員"，應該做到的是有自身價值，當項目方有需要製作的東西時會想到你這個人，知道你"能夠幫他們完成這個專案"、"是一個靠譜的人"、"能夠幫他們解決問題"等等，認可的是你這個"人"，前兩天看見小海老師在群內的分享，有項目方開了一年200,000U請他去做項目，但談下來發現這個不是他想做的東西，有這個底氣去拒絕了這個合作，這建立在小海老師是真材實料的，不會只是單純受到這個工資勉強。

這幾天的收穫真的挺多的，看見了很多自己的問題，也有許多可以改進的地方！
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->




































# **以太坊與比特幣的差異**

|   | BTC | ETH |
| --- | --- | --- |
| 目標與定位 | 去中心化的加密貨幣、總量固定 | 去中心化的平台、智能合約]、DApps、“區塊鏈2.0” |
| 編程能力 | 較基本，腳本語言有限、僅支持簡單的交易驗證邏輯 | 圖靈完全/完備的編程語言(如Solidity)，可開發複雜智能合約 |
| 共識機制 | 工作量證明 PoW (Proof of Work) | PoW -> 權益證明 PoS (Proof of Stak) |
| 交易速度 | 10分/區塊 | ≈ 12s/區塊 ，交易確認快 / 適合高頻 |
| 經濟模型 | 總量固定 / 抗通漲 | 供應靈活，通過EIP-1559 等機制可能呈現通縮趨勢 |

-   **EIP-1559**:為2021的升級，原先手續費全部給礦工，在這之後手續費分為"**基礎費(Base Fee)**"、“**優先費(Priority Fee)**”，多了**銷毀機制**  
    供應變化量=發行量-銷毀量 / 總量減少->資產增值
    
-   **EIP-4844:旨在降低手續費**，引入資料區塊(Blobs)，大幅降低資料儲存成本
    

| EIP-4844 | 升級前() | 升級後 (Blobs/EIP-4844) |
| --- | --- | --- |
| 儲存位置 | 永久存儲在區塊鏈上 | 暫時存儲 (約 18 天) |
| 成本 | 極高 (昂貴的鏈上空間) | 極低 (專屬、限時空間) |
| L2 手續費 | 較貴 (約 0.2 - 1.0u) | 極便宜 (可能降至 < 0.01u) |
| 吞吐量 | 受限 | 大幅提升 為未來分片打基礎 |

-   \*\*ZK-Rollup:\*\*零知識匯總 (Zero-Knowledge Rollup) 將交易在鏈下(L2)做打包，以太坊只需驗證打包後的"證明"即可，完善隱私問題、安全性、提升速度、降低成本
    
-   **The Merge** (2022 年):從高能耗 PoW -> 低能耗 PoS
    
-   **EIP-4844** (2024):讓L2更便宜，普通人也能夠用得起
    
-   **數據分片**(2025-2026):進一步降低 L2 成本，提升整體擴展性
    

![ethereum-roadmap_01-Bi2f1MRr.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/Huwan001/images/2026-01-14-1768394589462-ethereum-roadmap_01-Bi2f1MRr.jpg)

# 以太坊生態組成:L1、L2、Sidechains 等等

## 架構部分

-   L1:**以太坊主網**+**EVM**(主機)+**帳戶系統**(外部帳戶EOA+合約帳戶CA)
    
-   L2:**Rollup**(批量處理降低gas)+**Optimistic Rollup**(假設交易合法，僅在爭議時驗證)+**ZK Rollup**(0知識證明)
    
-   側鏈(**Sidechains**):與主網並行的獨立鏈，僅在交互時與主往連接(Bridge) / 缺點:驗證者較少，易遭駭客攻擊(Bridge)
    

## 生態層次

1.  **應用層** (Application Layer) : _Web3中，用戶直接_**_交互的介面_**  
    Defi應用:Uniswap（去中心化交易所）、Aave（借貸協議）、Compound（借貸協議）  
    NFT平台:OpenSea、Foundation、SuperRare  
    錢包應用:MetaMask、Coinbase Wallet、Rainbow  
    DAO工具:Snapshot、Aragon、Colony
    
2.  **協議層** (Protocol Layer) : _以太坊_**_核心_**_基礎設施_  
    共識層客戶層:Prysm、Lighthouse、Nimbus、Teku  
    執行層客戶層:Geth、Nethermind、Erigon、Besu  
    核心協議:EVM、状态管理、Gas 机制
    
3.  **擴展程** (Scaling Layer) : _提升_**_性能_**_+降低_**_成本_**_的解決方案_  
    L2 Rollups:Arbitrum、Optimism、Polygon zkEVM、zkSync Era  
    側鏈:Polygon PoS、xDAI（Gnosis Chain）  
    狀態通道:Lightning Network for Ethereum
    

# 今日兩場直播課堂

1.  安全主題分享會
    
2.  合規主題分享
    

兩場都屬於是非常硬，也是不管新手老手都必須要去鑽研的深度問題。

### _安全部分_在面對一些駭客的攻擊手法越來越強大的情況下，

-   必須要對自己的每一筆認證都確切核對交易內容，勿做盲簽
    
-   安裝安全插件 ex: scam sniffer
    
-   交易時確認合約/代幣地址
    
-   私鑰、助記詞的備份及存放 / 冷熱錢包的安全分離
    

### **_合規_**在目前當局監管機制下的風險

-   認清自己做過的交易紀錄，避免與黑U做交互，讓錢包帳戶暴露在風險下
    
-   出金務必走合規管道，避免誤觸刑案
    
-   找崗位時也要認清自己的項目是否合規，或存在的風險考輛
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->







































# 以太坊的起源

以太坊是由 Vitalik Buterin 在**2013年**提出，在**2015**/07/30上線主網

-   2013寫白皮書
    
-   2014開發工作啟動
    
-   2014/07-08籌措資金
    
-   2015上線
    
-   2016/03/14 以太坊第一次升級 (Homestead) ->協議穩定化
    

# **以太坊的定位及核心特質**

**Ethereum**為一**去中心化**、**開源**、具備**智能合約**的\*\*_公共區塊鏈平台_\*\*

-   核心組件:以太坊虛擬機 (Ethereum Virtual Machine，EVM)->我們常看到的EVM地址。  
    讓區塊鏈能夠執行合約代碼(開創了能夠**運行應用**的能力)，不再只有最初的記帳功能。
    
-   同時能夠作為 **資產** 及 **燃料(Gas**) 功能
    

## 特點

1.  **_智能合約(Smart Contracts)_**  
    由網路節點執行，儲存在區塊鏈上的程序  
    驗證者/打包者\_(Gas)\_執行
    
2.  **_分布式應用(dApps)_**  
    分布式應用程序部屬在區塊鏈上，不依賴單一服務器，  
    意味著只要以太坊網路不是全部同時崩潰，dApps就依然會存在。
    
3.  **_代幣(Tokens)_**  
    智能合約創造出代幣供dApps應用，分為兩種  
    1.**可替代的(ERC-20)**:如遊戲中的"金幣"、“穩定幣"等不具有唯一獨立性  
    2.\*\*不可替代的(ERC-721/ERC-1155):\*\*如"NFT”、"門票"等具有唯一獨立性之物品  
    代幣的發放能夠激勵早期貢獻者，令所有參與者的利益被綁在一塊 (管理者、投資者、用戶等)，大家希望手上的代幣越有價值，就會愈發的努力發揮自己在其中能做出的貢獻，壯大整個項目，進而提升手上的代幣價值。
    
4.  **_權益證明(PoS)、早期工作量證明(PoW)_**  
    最初採用\*\*_工作量證明 (Proof-of-Work, PoW)_\*\* ，類似BTC的礦工透過算力進爭獲得BTC作為獎勵，  
    2022/09/15的 The Merge"(合併) 升級改為 **權益證明(Proof-of-Stake, PoS)**  
    **POW->POS**  
    1.PoW機制:透過"挖礦"算數，打包區塊進而獲得獎勵/透過硬體算力(EX顯示卡、電力)成本  
    2.PoS機制:透過"質押"ETH(最小32ETH)，數量或時間越久來獲得激勵獎勵/具有資產銷毀的懲罰機制存在
    
5.  **_燃料費(Gas)_**  
    由"基礎費(Base fee)"+"小費(Priority fee)"組成，基礎費會銷毀，維持流通的僅有小費部分，獎勵給驗證者
    

# Web3 運行原理 (直播課程)

(時間問題，內容較為充實，有些地方還有不懂，需要花個幾天時間補，以下內容為AI筆記摘要)

## **這場會議主要討論了 Web3 運行原理，涵蓋以下主題：**

1.  **錢包**與**個人主權**  
    私鑰、助記詞、錢包地址的關係與生成原理 私鑰安全的重要性與注意事項 密碼學如何提供無需許可的個人主權
    
2.  **交易**與**簽名**  
    區塊鏈交易的組成（轉帳內容、Gas 費、防重放序號） 數字簽名的工作原理與驗證機制 Gas Fee 的經濟學意義
    
3.  **區塊鏈網絡運行**  
    從交易提交到出塊的完整生命週期 區塊的打包、排隊與確認機制 共識機制（PoW vs PoS） 節點、驗證者（Validator）的角色 RPC 與中心化風險
    
4.  **智能合約**  
    智能合約的本質（區塊鏈上的可執行代碼） Code is Law 的理念 實際演示 NFT mint 操作
    
5.  **協議升級**  
    EIP（以太坊改進提案）流程 客戶端多樣性的重要性 社區協作治理機制
    
6.  **核心特性**  
    去中心化、無需許可、抗審查 開放開源、隱私保護 跨學科特性（技術、經濟學、社會學、密碼學） 會議最後提出了四個思考問題，關於資產自託管安全性、公共基礎設施資金分配、有害信息治理，以及去中心化協作中的公平分配。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->










































# **區塊鏈是甚麼?**

-   一種 **_去中心化_** 的帳本技術
    
-   鏈由區塊組成 形成->區塊鏈
    
-   特性為\*\*_不可篡改、透明、快速_\*\*
    

**什麼是比特幣?**

-   網路節點服務提供能夠得到名為"BTC"的**代幣獎勵**
    
-   供量有限、可以自由轉帳，具備**貨幣屬性**
    

1.在虛擬網路中，交易方式比跨境轉帳方便，凸顯其價值。  
2.價格完全由市場決定，沒有受到監管機制限制，因此波動幅度相對穩定幣來得大。  
3.交易部分通過隨機生成的錢包完成，網路上的交易信息完全公開，但僅有錢包地址，無個人資料信息，同時具有交易公開透明+保護隱私+匿名。  
4.上述綜合無監管/方便/匿名，因此容易被黑產利用來進行洗錢等非法活動，當前許多新的區塊鏈技術正在逐步優化及解決此問題。

# **區塊鏈核心組成**

-   **去中心化網路** + **區塊鏈**
    
-   **維運** + **獎勵 (代幣激勵)**
    

由無數節點提供服務維持網路運行(維運)，節點通過計算驗證交易獲得代幣(獎勵)，這個動作稱為**挖礦**，是一個使用者付費的動作，使用過程中，支付代幣手續費(即**燃料費Gas fee**)，來支付礦工工資。

# **公鏈、私鏈、聯盟鏈**

1.  **公鏈** - （Public Blockchain）  
    公開訪問(anyone)自由進出、安全、速度慢(人數多效率低)  
    數據共同管理，具備共識機制，但效率低、維護成本高
    
2.  **聯盟鏈** - （Consortium Blockchain）  
    聯合治理、安全與速度兼容  
    需申請加入，每個人權限不同(具備高低階級)，部分數據公開(半公開)，決策需成員共同通過  
    效率>公鏈(成員較少) / 隱私>公鏈(數據辦公開) / 靈活<私鏈(共同決議)
    
3.  **私鏈** - （Private Blockchain）  
    有訪問權限的限制，靈活、速度快  
    加入有一定的資格限制，權限在領頭人身上(決定誰可以幹嘛誰不能幹嘛)，數據完全私有制(僅供內部查看)，單人決策機制。  
    ✅優點:效率最高、隱私最好  
    ❌缺點:缺乏足夠的透明性
    

# **Web3 vs Web 3.0 vs Web2 (Web3 ≠ Web 3.0)**

1.  Web2 (當前互聯網)  
    數據、演算法由中心化角色控制(Googl、FB等)。  
    用戶產出內容，由中心化角色搭配廣告共同維運。
    
2.  Web3.0 (語意網)  
    產生於2001年由W3C提出 (遠早於BTC) ，強調的是**數據的連結性**，非去中心化帳本，核心在於**預先定義好的邏輯框架**。  
    使用RDF、OWL等標準描述數據關係，主要是用於給機器理解，有一個\*\*_標準的程序及寫法_**，以特定的方式建構網路語意關係  
    \->將**純文字**的網頁轉化為**結構化數據\*\*。  
    ❌非區塊鏈技術，為傳統互聯網Web2的數據升級、不依賴AI  
    ✅可與Web3結合 (語意標記+區塊鏈儲存) ，具有互補作用
    
    |   | 優點 | 缺點 |
    | --- | --- | --- |
    | Web3.0 | 描述關係 | 無法保證數據沒被竄改 |
    | Web3 | 提供不可篡改性與所有權證明 | 不同鏈之間數據格式不通 |
    
3.  Web3 (去中心化物聯網)  
    數據主權歸屬用戶，透過區塊鏈儲存用戶**身分**及**資產**  
    自動由智能合約運行
    

-   資產的真正擁有:NFT、遊戲道具
    
-   金融的虛擬化:手機錢包即可操作資產(借貸、理財、交易)
    
-   功能的可組合性:因為代碼的開源性，每個獨立功能使用一個操作即可執行，具備**組合性**，但對於基礎協議的安全性也更加重要。
    
-   永久性:內容存儲在區塊鏈上後會永遠被保存，不會消失。
    

# **技術開發**

| Web2 | Web3 | Web3.0 |
| --- | --- | --- |
| React + Node.js + MySQ | React + Ethers.js + Solidity + IPFS | Python + RDFLib + SPARQL |
| for快速就業 | 知識圖譜和語義搜索 Web 3.0（RDF / OWL） | 去中心化金融 Web3（Solidity / Rust） |

# \-

# **Web3 实习计划（冬季）- Web3 行业全局介绍 & 岗位概览**  
**會議紀錄**

**SmartDeer APP**

有發一些Web3實習的職位，可以上去看看  

## 交易所/資產管理/運營

多座落於**香港**(政策支持)、**新加坡**、**中東**(資本聚集地)

行業重心轉移

Defi->RWA、Depin(硬件設施)、AI融合

平均錄取率 約1/450 (受到AI影響，Junior需求較低)

-   紅海:初階Solidity開發、初級社區運營(MOD)
    
-   藍海:資深Rust工程師、合規相關、複合型
    

70% 多數會考察**_AI工具使用能力_**、**_鏈上交互紀錄_**  

\-初級人才過剩、菁英短缺

需求(複合性人才)-懂技術、商業邏輯  

**傳統平台:Indeed、Boss直聘**

**Web3核心渠道:**

1.  垂直平台-SmartDeer、[Web3.career](http://Web3.career)、Remote3、CryptoJobList
    
2.  社交網路-X (twitter)、Web3媒體(整合定期發布)、Farcaster (Warpcast)、DC項目群、Linkedin、項目官網(Noition)有些founder會直接在這些社交平台上聯繫合適人才
    
3.  隱形渠道-GitHub Contribution (代碼貢獻即簡歷)、DAO治理提案  
    即鏈上簡歷，項目方可直接看到代碼上的實質貢獻，過往經歷
    

  

## 用人多注重

-   菁英體制:包含高學歷、學習能力強、熱情積極 (背景非必須)
    
-   經驗篩選:Web2有經驗、Web3有使用/項目經驗 (背景佔比高)
    

多數為**遠端辦公**(Remote)，有**外語能力**需求，

部分在內地有辦公室，但可能有合規問題。

目前逐漸有**混合辦公**型式  

\-崗位人才需求量

技術>產品經理>運營>市場>社區管理>合規>安全審計>投研

\-薪資待遇

技術研發>產品研究>市場運營>職能支持

\-職涯規劃

技術->CTO/獨立審計師

運營->公司團隊經營/夾帶自身資源 Tokenomics  

## _不建議海投簡歷，較好的方式為專注於投的職位去單一做準備_

### 方向

1.  從Web2轉職Web3 (有原有技能/專業支持)
    
2.  生態切入(適合從0開始積累) EX:Dao、開源社區、投研社區 ->經營Web3簡歷豐富履歷內容再去投or項目方主動尋找  
    撰寫自己對於某事件的看法，發表研究報告 逐漸做出聲量
    
3.  建立鏈上聲譽:經營X、Farcaster  
    

**技術崗位**

語言學習:Solidity必定要了解，為基本概念及首要要求，

再額外去了解其他語言，不同鏈的開發知識(EX:Move、Cairo、Func等)

盡量豐富技能樹，只會寫代碼，競爭力可能稍微不足。  

**運營/市場**

做過社區(X/DC/FB)、媒體運營(外語能力)，還能補充鏈上活躍程度、做過轉化閉環等相關知識  

**產品/投研**

偏好有一定背景的人，具有底層開始設計的經驗的PM

計算機、金融領域專業有優勢

打造鏈上CV，利用區塊鏈技術和鏈上數據，來展示個人技能與成就的履歷，真正去參與到某個生態/項目/社區 之中

##   
額外建議

**技術人員-**\>參與代碼貢獻

**運營**\->參與項目治理、投票、累積鏈上徽章

**真實貢獻** > 傳統履歷PDF

多掌握開發語言 (Solidity為基本知能合約底層邏輯)

熟用AI工具:展示如何使用AI提升智能合約審計或自動化運行的效率->提升效率/能力 EX:寫代碼、內容等

(重點AI工具只是輔助)

識別Rug項目，警惕三無團隊(匿名、無風投支持、無代碼更新)，避免被項目收割，提前預防減少風險
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
