---
timezone: UTC+8
---

# resurrection-i

**GitHub ID:** resurrection-i

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
\- 节点 = EL+CL 双客户端，一个跑交易一个投票；家用电脑开 Snap 同步，一晚上就能追上主网。

\- 全/轻/归档的区别：SSD 容量和是否存历史状态，开发用全节点就够。

\- 账户只有两类：EOA 有私钥能发起，合约只有代码被动等调用；地址 0x 后 40 位，大小写混写是 EIP-55 防手滑。

\- 合约地址可提前算：CREATE 看 nonce，CREATE2 看 salt+init code，撸空投常用后者。

\- 代币余额全在合约的 mapping 里，转帐=调函数改存储，区块浏览器只是读 storage 给你看。

\- 合约部署三步：写 Sol → 编译拿 bytecode+ABI → 发 tx（to=空，data=init code），Gas ≈ 21000+32000+字节费。

\- 安全口诀：先 CEI 再交互，溢出 0.8 已拦，权限别用 tx.origin，升级留时间锁。

\- 工具：Remix 快速验想法，Hardhat 写脚本，Foundry fuzz 速度快，Tenderly 回放救一命。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

📝 链上日志 2026-01-12｜「第一次」

任务清单第 7 项：向 0x… 打出 0.1 Sepolia ETH，区块高度 6 8xx xxx，Gas 25 gwei，Nonce 42。

浏览器刷新的 0.3 秒里，想起大一时第一次用 Gas私链挖矿，风扇转得比今天还响。

两年过去，测试网还是测试网，我已不再把“转出”当成仪式，只把它当调试日志里的一行 INFO。

打卡：✅ 任务「第一次」链上确认，待续。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
