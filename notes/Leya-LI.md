---
timezone: UTC+8
---

# LI LEYA

**GitHub ID:** Leya-LI

**Telegram:** @AlinaLeya

## Self-introduction

大家好，我是Alina。最初是因为课程和身边圈子接触到web3，后来去跑了TOKEN2049和SFF展会，心态慢慢从“好奇看看”变成了想真正扎根在这个行业。 希望通过这次实习，一边系统理解Web3，一边在社区持续输出！

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
### **1\. Getting Started with MetaMask**

-   **Official Installation:** Go to [**metamask.io**](http://metamask.io) and install the Chrome extension \[[01:21](http://www.youtube.com/watch?v=_GjPeRLCREA&t=81)\]. Be extremely cautious of phishing sites with similar URLs \[[01:52](http://www.youtube.com/watch?v=_GjPeRLCREA&t=112)\].
    
-   **The Fox Icon:** The animated 3D fox in MetaMask acts as a subtle security feature that is difficult for simple phishing sites to replicate \[[02:15](http://www.youtube.com/watch?v=_GjPeRLCREA&t=135)\].
    
-   **Local Password vs. Seed Phrase:** You will set a password, but this is only for **local encryption** on your device \[[03:30](http://www.youtube.com/watch?v=_GjPeRLCREA&t=210)\]. If you lose this password, you can still recover your wallet using your seed phrase \[[03:36](http://www.youtube.com/watch?v=_GjPeRLCREA&t=216)\].
    

### **2\. The 12-Word Seed Phrase (Mnemonic)**

-   **Your Master Key:** This 12-word phrase is the master key to your entire wallet \[[02:44](http://www.youtube.com/watch?v=_GjPeRLCREA&t=164)\]. Anyone with these words can empty your account \[[05:59](http://www.youtube.com/watch?v=_GjPeRLCREA&t=359)\].
    
-   **Safe Storage Practices:**
    
    -   **Do:** Write it on paper and store it in a secure physical location (e.g., a "sock drawer" or safe) \[[06:11](http://www.youtube.com/watch?v=_GjPeRLCREA&t=371)\].
        
    -   **Do:** Use a reputable password manager (e.g., 1Password) \[[07:03](http://www.youtube.com/watch?v=_GjPeRLCREA&t=423)\].
        
    -   **Don't:** Take screenshots, store it in your phone's notes, or send it via messaging apps like WhatsApp \[[06:39](http://www.youtube.com/watch?v=_GjPeRLCREA&t=399)\].
        
-   **Brute Force Immunity:** While mathematically possible to guess a phrase, the entropy is so high that it would take a trillion computers guessing a trillion times per second the entire lifetime of the universe to find a specific account \[[48:32](http://www.youtube.com/watch?v=_GjPeRLCREA&t=2912)\].
    

### **3\. Understanding Key Pairs & Addresses**

-   **Private Keys:** Derived from your seed phrase, this is a 64-character hex string that signs transactions \[[37:54](http://www.youtube.com/watch?v=_GjPeRLCREA&t=2274)\].
    
-   **Public Address:** Derived from the private key; this is what you share with others to receive funds \[[16:03](http://www.youtube.com/watch?v=_GjPeRLCREA&t=963)\].
    
-   **Deterministic Nature:** One seed phrase can generate an infinite number of accounts (Account 1, Account 2, etc.). These will always be the same every time you restore that specific seed phrase \[[30:59](http://www.youtube.com/watch?v=_GjPeRLCREA&t=1859)\].
    

### **4\. Transactions and Block Explorers**

-   **Disinterested Third Parties:** Use **Etherscan** to verify transactions independently of your wallet software \[[23:39](http://www.youtube.com/watch?v=_GjPeRLCREA&t=1419)\].
    
-   **No "Undo" Button:** Once a transaction is mined on the blockchain, it cannot be reversed. There is no customer support to call \[[01:06:47](http://www.youtube.com/watch?v=_GjPeRLCREA&t=4007)\].
    
-   **Gas Fees:** You pay "gas" to have your transaction processed. On Mainnet, this is real money; on Testnets (like Kovan or Goerli), it is free \[[18:26](http://www.youtube.com/watch?v=_GjPeRLCREA&t=1106)\].
    

### **5\. Custodial vs. Non-Custodial**

-   **Custodial (Exchanges):** Services like Coinbase own the keys and manage your balance in their database. You are reliant on their security \[[01:04:15](http://www.youtube.com/watch?v=_GjPeRLCREA&t=3855)\].
    
-   **Non-Custodial (MetaMask/Hardware):** You own the keys. "Not your keys, not your coins" \[[01:04:36](http://www.youtube.com/watch?v=_GjPeRLCREA&t=3876)\]. This offers more control but places 100% of the security responsibility on you \[[01:07:05](http://www.youtube.com/watch?v=_GjPeRLCREA&t=4025)\].
    

### **6\. Advanced Security Options**

-   **Hardware Wallets:** Physical devices (like Ledger or Trezor) that keep private keys offline and air-gapped \[[01:12:35](http://www.youtube.com/watch?v=_GjPeRLCREA&t=4355)\].
    
-   **Smart Contract Wallets (Multi-sig):** Wallets like Gnosis Safe or Argent that require multiple signatures (e.g., 2-of-3) to move funds, providing a "2FA" feel for large sums of money \[[01:14:29](http://www.youtube.com/watch?v=_GjPeRLCREA&t=4469)\].
    

[https://www.youtube.com/watch?v=\_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3](https://www.youtube.com/watch?v=_GjPeRLCREA&list=PLJz1HruEnenAf80uOfDwBPqaliJkjKg69&index=3)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

**Web2 to Web3 - Day 1 Intro Notes**

-   **Course Structure:**
    
    -   **Week 1 (Power User):** Wallets, Gas, dApps, NFTs, ERC20/721.
        
    -   **Week 2 (Scripting):** Providers, Signers, Ethers.js, Hardhat.
        
    -   **Week 3 (Speedrun):** Building apps like Staking, Token Vendors, and Dice Games.
        
    -   **Week 4:** Open-ended build phase.
        
-   **Key Concepts:**
    
    -   **Trustless Escrow:** Smart contracts allow parties (Alice & Bob) to trade without trusting each other.
        
    -   **Incentives:** Essential for decentralized systems (e.g., paying users to trigger “cron jobs”).
        
    -   **Unstoppable Code:** Once deployed, smart contracts cannot be stopped or censored.
        
-   **Success Paths:**
    
    1.  Get hired by a Web3 organization.
        
    2.  Build and launch your own product.
        
    3.  Become a Smart Contract Auditor (Level 3).
        

[https://www.youtube.com/watch?v=zuJ-elbo88E](https://www.youtube.com/watch?v=zuJ-elbo88E)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
