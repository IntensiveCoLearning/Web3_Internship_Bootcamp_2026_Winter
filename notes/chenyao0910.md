---
timezone: UTC+8
---

# Yao Chen

**GitHub ID:** chenyao0910

**Telegram:** 

## Self-introduction

Hi, I'm Yao, a junior developer

## Notes

<!-- Content_START -->
# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->
try to do some coding challenage  
  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/chenyao0910/images/2026-01-20-1768923759746-image.png)
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->

智能合約

運行在以太坊上面的程序合同，只要輸入條件滿足，就會自動執行

deploy上去之後，如果要更新代碼，必須透過proxy模式

優點

1.無需中介，自動執行

2.可預測，同樣input → 同樣output

3.公開紀錄，操作紀錄都在鏈上

4.  以太坊让你不必暴露实名就能参与网络，但在链上留下的一切行为都是公 开可追踪的，隐私需要额外设计
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->


一個完整的以太節點，需要包括

執行客戶端以及共識客戶端，想賺到質押受益還需要加上驗證端

1.  執行客戶端 (Execution Client，EL)
    
    a.負責鏈上的業務邏輯以及狀態  
    b.算交易
    

2共識客戶端

a.跑pos,選區塊,投票

b.他們都透過engine api進行溝通

這兩個加起來就叫做全節點

多加上validator，他就會是驗證節點
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->



偷偷往後看了一些我有興趣的東西

# **為什麼地址都是0x開頭呢**

其實0x代表的是16進位的意思(hex)  
也就是說平常看到的address ⇒ 0xavbeasdcrqwe  
其實都是hex的形式，他背後是可以轉換成010101的這種二進位表示  
  
而已0x表示的地址，實際上有40位  
而一個hex = 要用4個bit去表示  
所以一個以太的地址用 binary表示 就會是0101…..10101010 總共 4\*40 = 160bits  
  
而0x不算是地址的一個部分，他只是用來告訴我們他是 hex表示的地址

而EIP-55讓大小寫可以變得不敏感，所以他的checksum是忽略大小寫的
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->




Rollup 是layer2 的主流路線，目的是要幫助L1 擴容，提高吞吐量

Optimistic Rollup → 默認交易正確，只有錯誤的才要提出

ZK Rollup → 利用零知識證明

  
  
一個完整的以太節點，需要包括

執行客戶端以及共識客戶端，想賺到質押受益還需要加上驗證端
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->





Rollup 是layer2 的主流路線，目的是要幫助L1 擴容，提高吞吐量

Optimistic Rollup → 默認交易正確，只有錯誤的才要提出

ZK Rollup → 利用零知識證明  
  
  
另外完成了faucet的提領

![截圖 2026-01-13 凌晨12.13.09.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/chenyao0910/images/2026-01-13-1768320128057-___2026-01-13___12.13.09.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->






Gas (BaseFee + Priority Fee)

1.  做任何互動都需要，以防資源浪費 (都需要節點驗證)
    
2.  Base Fee (由協議自動計算，會up & down ，會直接銷毀)
    
3.  Priority Fee
    
    -   自行出價以激勵驗證者加速處理你的transaction
<!-- DAILY_CHECKIN_2026-01-13_END -->
<!-- Content_END -->
