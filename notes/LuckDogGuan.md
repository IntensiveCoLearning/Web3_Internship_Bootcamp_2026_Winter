---
timezone: UTC+8
---

# 流浪少年

**GitHub ID:** LuckDogGuan

**Telegram:** 

## Self-introduction

base 厦门 俩年C++/C嵌入式工程师，INTJ,对web3/交易 感兴趣，希望转行到web3行业. 方向是defi＋后端，已经学完了go基础，最近边上班边学习solidity.
方向是defi＋后端，已经学完了go基础，最近边上班边学习solidity.

## Notes

<!-- Content_START -->
# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->
**Web3 合规篇：法律红线与风险识别**

作为开发人员，了解合规不仅是为了项目安全，更是为了**自我保护**。

• **核心逻辑：法律的滞后性**

    ◦ 法律具有滞后性，而 Web3 业务形态（如 DeFi、DePIN）非常超前。

    ◦ 在中国大陆语境下，合规的本质是**风险控制**，因为政策收紧，活动空间较小。

• **三大负面清单（大陆重点打击）**

    1. **ICO/公开融资：** 打击借发币名义面向不特定对象募资的行为。

    2. **虚拟货币交易所：** 境内展业风险极高。例如，**永续合约**在司法实践中常被认定为“开设赌场”，即便技术人员辩解“仅负责代码实现”，也可能因提供技术支持被定罪。

    3. **虚拟货币挖矿：** 已被禁止。目前合规路径通常是转型为“AI 算力中心”或“超算中心”并与政府签约。

• **后端开发需警惕的罪名**

    ◦ **非法吸收公众存款/集资诈骗：** 只要涉及向公众募资，该罪名常用于兜底。

    ◦ **非法经营罪：** 特别是涉及法币与代币“对敲”的跨境兑换业务。

    ◦ **帮信罪/非法利用信息网络罪：** 如果为被认定为赌博网站的平台（如某些合约平台）提供技术支持或投放广告，技术人员和 KOL 都有入狱风险。

• **出入金风险：**

    ◦ 核心风险是收到“赃款”导致银行卡被冻结。

    ◦ **“镀金攻击”：** 境外资金盘通过强制向境内账户汇入赃款，利用大陆公安的风控机制触发账户冻结，以此增加洗钱难度或实施打击

\--------------------------------------------------------------------------------

**Web3 安全篇：技术漏洞与攻击趋势**

对于 DeFi 后端开发，安全是 Web3 的生命线。2025 年的安全形势呈现出“两极分化”趋势。

• **攻击两大趋势**

    1. **精准猎杀：** 针对高 TVL（总锁仓量）协议、CEX（交易所）或大户。攻击者通过深度挖掘智能合约漏洞或实施 APT（高级持续性威胁）渗透。

    2. **广洒网：** 利用自动化工具进行钓鱼攻击、地址投毒（粉尘攻击）或通过“代币工厂”批量制造貔貅盘。

• **核心攻击手段**

    ◦ **私钥泄露：** 2025 年头号威胁，多见于管理不当或通过社交工程学渗透。

    ◦ **智能合约漏洞：** 包括**重入攻击**、**预言机操纵**、计算漏洞（整数溢出）等。

    ◦ **AI 深度伪造（Deepfake）：** 黑客伪装成合作伙伴或投资人进行视频会议，骗取信任后发送恶意链接或程序。

    ◦ **软件供应链攻击：** 在 AI 编程时代，恶意代码可能通过插件或 AI 生成的代码片段引入。

• **新兴技术风险：EIP-7702 钓鱼**

    ◦ 以太坊新提案赋予了普通地址（EOA）智能合约的能力。

    ◦ 黑客利用此特性，诱导用户授权恶意合约，实现资产自动转走，而用户从前端看地址依然如常。

\--------------------------------------------------------------------------------

**后端/DeFi 开发者防御指南**

作为未来的 DeFi/后端开发，您应遵循以下**全生命周期安全管理**建议：

1\. **开发阶段（安全左移）：**

    ◦ 使用成熟的安全模版（如 **OpenZeppelin**）。

    ◦ 引入自动化审计工具，在部署前进行静态和动态分析。

2\. **代码审计：** 智能合约必须经过专业的人工审计，因为 AI 虽能辅助挖坑，也能辅助找坑，但复杂的业务逻辑仍需人工把关。

3\. **运行阶段：**

    ◦ 建立**应急响应机制**（如暂停合约、资产冻结功能）。

    ◦ 监控链上异常交易，提前防范黑客大规模撤资。

4\. **个人安全意识（“四不大法”）：**

    ◦ **不点：** 不点任何社交平台发送的未知链接。

    ◦ **不签：** 拒绝**盲签**。签名时务必核对内容（如领空投却弹出 USDT 授权）。

    ◦ **不装：** 不随意安装第三方插件或不明软件，尤其是涉及 AI coding 的工具。

    ◦ **不转：** 仔细核对转账地址，防止地址投毒钓鱼。

**复习要点小结：**

• **合规**是为了在法律允许的边界内展业，规避大陆的“刑事风险”。

• **安全**是为了在技术对抗中存续。**DeFi 的核心是金融，金融的核心是风控**。没有安全，Web3 就无法实现大规模应用
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->

# web3[Unphishable 钓鱼挑战](https://unphishable.io/)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/LuckDogGuan/images/2026-01-14-1768369761218-image.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->


# solidity测试网发送接收代币

Fund 发送，getFund接收

[https://github.com/LuckDogGuan/solidity\_learn/blob/main/ChainlinkWeb3Learn/3.FundMe/FundMe.sol](https://github.com/LuckDogGuan/solidity_learn/blob/main/ChainlinkWeb3Learn/3.FundMe/FundMe.sol)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/LuckDogGuan/images/2026-01-13-1768287549845-image.png)
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->



# 开营仪式

发现参加web3的各行各业都有，非常震惊，但是同时也是开心的。

说明web3是一个增长的行业，包容开放正是web3的魅力。希望后续的学习可以跟的上哈哈哈。

# 注册小狐狸&领取测试币和转账

谷歌水龙头

[https://cloud.google.com/application/web3/faucet/ethereum/sepolia](https://cloud.google.com/application/web3/faucet/ethereum/sepolia)  
  
转账记录

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/LuckDogGuan/images/2026-01-12-1768181642241-image.png)
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
