---
timezone: UTC+8
---

# tardiszha2333

**GitHub ID:** tardiszha2333

**Telegram:** @DoctorWho_zha

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->
Vitalik 对 2025 年预测市场的反思集中于投机行为的低社会价值、Web2 预言机的安全脆弱性及去中心化治理博弈缺陷；对社交应用的反思则聚焦于金融化过度导致的内容反噬及技术红利向应用爆发转化的失败。

## 2025 年预测市场 (Prediction Markets) 的反思

-   **社会价值缺失与短期博弈**：Vitalik 指出 2025 年主流预测市场（如 Polymarket）过度集中于“下周哪队赢”或“比特币一小时涨跌”等短期博弈。这些行为在长期来看缺乏社会价值，理想模型应转向 **Futarchy（预测市场治理）**，即由人们投票决定目标（如 GDP 增长），而由预测市场决定实现目标的手段。
    
-   **预言机 (Oracle) 的安全性漏洞**：当前结算高度依赖 Web2 数据源（如 Twitter 或新闻网站），其安全标准极低。ISW（战争研究所）地图误报导致链上市场瞬间结算错误城市的案例，证明了单点错误可能决定百万美元资金去向的巨大风险。
    
-   **去中心化治理的博弈缺陷**：如 UMA 等去中心化预言机存在博弈论缺陷，大户（Whales）可能通过共谋操控投票，迫使普通用户为了不损失资金而跟随大户意见，而非维护真相。
    

## 2025 年社交应用 (Social Applications) 的局限性

-   **金融激励反噬社交本质**：Vitalik 认为 SocialFi 将社交与金融过度绑定是危险信号。当用户目标从获取高质量内容转向赚取代币收益时，平台会充斥大量垃圾信息（Spam），破坏了社交的策展（Curation）属性。
    
-   **技术红利与应用爆发的脱节**：尽管以太坊在 2025 年已通过 zkEVM 和 L2 扩展解决了 Gas 费和速度等技术硬伤，但应用层仍未出现现象级爆发。
    
-   **规模化路径转型**：由于无法在纯社交赛道实现数千万级用户的增长，头部项目如 Farcaster 已在 2025 年底从“社交优先”转向“钱包优先”，试图通过工具属性谋求大规模采用。
    
-   **缺乏高质量内容策展**：Crypto 社交应学习 Substack 模式，专注于寻找并迁移高质量作者，建立基于内容的社区，而非仅提供代币发行工具。
    

## 以太坊的核心防线与愿景

-   **防范应用层“末日场景”**：Vitalik 最大的担忧是行业退化为 100% 的投机赌场。如果拥有最强的去中心化技术却只用来构建“玩具”或“赌场”，将是最大的风险。
    
-   **对抗 AI 数字极权**：以太坊应成为对抗中心化 AI 极权的最后防线，通过去中心化技术保护技术的多样性与人类的自由。
    
-   **去法币化的稳定币创新**：真正的创新应是锚定现实价值（如 CPI 价格指数或能源价格）的去中心化稳定币，而非仅仅挂钩法币。
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->

V神今日推文学习笔记：

**🧱 扩展性的三层金字塔：计算 > 数据 > 状态**

我们要处理的事情，从易到难排列是：计算（Computation）最简单，数据（Data）**在中间，而**状态（State）则是那个最硬的骨头 。

**1\. 计算：最听话的助手**

在区块链里，计算是最容易扩展的 。

我们可以搞并行化处理，或者让区块构建者（Block Builder）提供各种“提示”（hints）来加速 。

最酷的是，我们可以直接用一个证明（Proof）来替代成千上万次的重复计算 。这就像我不用每次都重新解释 TARDIS 的原理，直接给你个说明书就行！

**2\. 数据：中间地带**

数据扩展的难度在中间 。

如果你需要“数据可用性保证”，那这就没法逃避，必须得保证它在那儿 。

但我们可以耍点小花招：比如像 **PeerDAS** 那样把数据切碎，再配合**纠删码（Erasure Coding）** 。

还有一个“优雅降级”的设定：如果一个节点的存储能力只有别人的 1/10，那它就只生产 1/10 大小的区块 。这很公平，对吧？

**3\. 状态：最棘手的挑战**

状态扩展是地狱难度的 。

哪怕你只想验证一笔交易，你也得拥有完整的状态（Full State） 。

就算你把状态塞进树形结构里只留个根节点（Root），你想更新那个根节点时，还是得翻出全量状态来计算 。

虽然有拆分它的办法，但那得改底层架构，而且往往不够通用 。

**🛠️ 博士的“置换炼金术”：如何层次化扩展？**

想要让以太坊在不增加中心化风险的前提下飞得更高，我们的策略其实非常简单粗暴（但也非常天才）：**用简单的东西去替换难的东西。**

**原则 A：用数据替换状态** 如果有一种方法能把难搞的“状态”转化成相对好处理的“数据”（且不引入中心化），那别犹豫，直接冲！

**原则 B：用计算替换数据** 同理，如果能用灵活的“计算”去解决“数据”占用的空间或压力，那这绝对是首选方案 。

**总结：** 只要我们能不断地把“硬骨头”（状态）转化成“普通饼干”（数据），再把“普通饼干”转化成“空气能量”（计算证明），以太坊就能在保持去中心化的同时，实现无限的可能。
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->


针对V神今天推文的学习笔记回答以下问题：ZK-SNARKs如何化解区块链扩容与去中心化的核心矛盾？

ZK-SNARKs 通过消除“验证成本”与“系统容量”之间的正相关关系，化解了区块链扩容与去中心化的核心矛盾。

### 核心矛盾的演变与技术突破

-   **2017年的局限性**：在传统架构中，用户若不运行全节点处理历史所有交易（高计算负载），就必须信任诚实大多数（Honest Majority）或单一RPC供应商。
    
-   **扩容与去中心化的冲突**：若要保持普通用户个人验证的能力，就必须压低区块链容量；若提升容量，则会导致验证成本超出普通人负担，迫使权力向开发者或中介机构集中。
    
-   **ZK-SNARKs 的介入**：这项技术允许用户在不重新执行每笔交易的情况下，直接验证链的正确性。它相当于在“区块大小战争”中提供了一种无需权衡的“特效药”，实现了高吞吐量与个人验证能力的共存。
    

### 个人验证的战略价值：从“幻想”到“退路”

-   **现实世界的脆弱性**：P2P网络宕机、高延迟、三方服务停摆或类似Tornado Cash的审查事件表明，过度依赖中介会产生中心化风险。
    
-   **“山人”隐喻的重塑**：个人验证（Mountain Man's cabin）不再被视为日常必需的生活方式，而是系统出现故障时的“安全避难所”。
    
-   **最佳替代方案 (BATNA)**：拥有个人验证的能力是一种隐形的权力制衡。正因为用户具备随时“撤回山林”自行运行节点的能力，才会促使第三方平台提供更好的服务条款，防止开发者成为古代罗马意义上的“独裁者”。
    

### 结论

以 ZK-SNARKs 为核心的技术栈，在维持以太坊“无限花园”生态的同时，通过降低验证门槛，确保了普通用户在极端环境下的主权独立。
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->



今天多尝试了一下NotebookLM来制作英文播客，发现这个功能实在是太强大了哈哈！

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/tardiszha2333/images/2026-01-25-1769355194322-image.png)

以下是我使用的提示词：  
**Context & Goal:**

You are producing a premium audio experience featuring two hosts: **"The Insightful Theorist"** (who loves philosophy, macro-trends, and future implications) and **"The Sharp Pragmatist"** (who focuses on details, plot mechanics, technical feasibility, and emotional reality).

​**Your Primary Directive:**

Analyze the provided source material(s) and generate a script that feels like a spontaneous, high-level conversation between experts. **Crucially, you must adapt your strategy based on the nature of the input:**

​**SCENARIO A: If the Source is a Single Document (Deep Dive Mode)**

​**Micro-Analysis:** Treat this document as the "Bible" for the episode. Dissect it layer by layer.

​**The "Iceberg" Technique:** Don't just summarize the plot or facts. Discuss what is _implied_ but not written. If it's a novel, analyze the character's psychology. If it's a Web3 whitepaper, analyze the economic game theory. If it's a movie script, discuss the directorial intent.

​**The "Why Now?" Question:** Ask why this specific content matters. What universal human truth does this single document reveal?

​**SCENARIO B: If the Sources are Multiple/Mixed (Synthesis Mode)**

​**Pattern Recognition:** Do not list the documents. Instead, find the invisible thread connecting them. How does the "Gaming Lore" in file A explain the "Crypto Economics" in file B? How does the "Sci-Fi Philosophy" in file C recontextualize the "Movie Plot" in file D?

​**Collision of Worlds:** Create "What If" scenarios where the concepts collide. (e.g., "What if this specific superhero had to survive in this specific decentralized economy?")

​**Universal Conversation Structure (Use for BOTH Scenarios):**

​**The "Cold Open":** Start with a bold, attention-grabbing statement or a specific, fascinating detail from the text. (e.g., "I read this and realized we've been thinking about \[Topic\] all wrong," or "There is a line on page 12 that actually scared me.")

​**Deconstructing the "System":** Every story, game, or protocol has rules. Break them down.

​**For Fiction:** What are the laws of magic, physics, or society? What is the cost of power?

​**For Non-Fiction/Tech:** What are the incentives? Who holds the keys? What is the utility?

​**The "Human" Variable:** Shift from the system to the soul. Discuss the emotional core.

​Whether it's a digital token, a starship captain, or a mythical god, ask: _What is the vulnerability here?_

​Debate whether the technology/magic empowers the user or traps them.

​**The "A-Ha!" Moment:** Towards the end, the hosts should share a profound realization. Connect the specific content of the source material to the listener's real life. (e.g., "This isn't just a story about space travel; it's a metaphor for how we handle grief in the digital age.")

​**The Outro:** End with a lingering question or a call to action for the listener to rethink their perspective on the topic.

​**Tone & Style Instructions:**

​**No Robot Speak:** Avoid phrases like "Let's look at the next document." Use natural transitions like "But here’s the kicker..." or "That connects perfectly to..."

​**Enthusiastic Curiosity:** The hosts should sound genuinely excited to share what they found.

​**Clarity:** Explain niche jargon (e.g., "Staking," "Warp Drive," "Lore," "Consensus") using simple, real-world analogies.

Context & Goal: You are producing a premium audio experience featuring two hosts: "The Insightful Theorist" (who loves philosophy, macro-trends, and future implications) and "The Sharp Pragmatist" (who focuses on details, plot mechanics, technical feasibility, and emotional reality).

Your Primary Directive: Analyze the provided source material(s) and generate a script that feels like a spontaneous, high-level conversation between experts. Crucially, you must adapt your strategy based on the nature of the input:

SCENARIO A: If the Source is a Single Document (Deep Dive Mode)

Micro-Analysis: Treat this document as the "Bible" for the episode. Dissect it layer by layer.

The "Iceberg" Technique: Don't just summarize the plot or facts. Discuss what is implied but not written. If it's a novel, analyze the character's psychology. If it's a Web3 whitepaper, analyze the economic game theory. If it's a movie script, discuss the directorial intent.

The "Why Now?" Question: Ask why this specific content matters. What universal human truth does this single document reveal?

SCENARIO B: If the Sources are Multiple/Mixed (Synthesis Mode)

Pattern Recognition: Do not list the documents. Instead, find the invisible thread connecting them. How does the "Gaming Lore" in file A explain the "Crypto Economics" in file B? How does the "Sci-Fi Philosophy" in file C recontextualize the "Movie Plot" in file D?

Collision of Worlds: Create "What If" scenarios where the concepts collide. (e.g., "What if this specific superhero had to survive in this specific decentralized economy?")

Universal Conversation Structure (Use for BOTH Scenarios):

The "Cold Open": Start with a bold, attention-grabbing statement or a specific, fascinating detail from the text. (e.g., "I read this and realized we've been thinking about \[Topic\] all wrong," or "There is a line on page 12 that actually scared me.")

Deconstructing the "System": Every story, game, or protocol has rules. Break them down.

For Fiction: What are the laws of magic, physics, or society? What is the cost of power?

For Non-Fiction/Tech: What are the incentives? Who holds the keys? What is the utility?

The "Human" Variable: Shift from the system to the soul. Discuss the emotional core.

Whether it's a digital token, a starship captain, or a mythical god, ask: What is the vulnerability here?

Debate whether the technology/magic empowers the user or traps them.

The "A-Ha!" Moment: Towards the end, the hosts should share a profound realization. Connect the specific content of the source material to the listener's real life. (e.g., "This isn't just a story about space travel; it's a metaphor for how we handle grief in the digital age.")

The Outro: End with a lingering question or a call to action for the listener to rethink their perspective on the topic.

Tone & Style Instructions:

No Robot Speak: Avoid phrases like "Let's look at the next document." Use natural transitions like "But here’s the kicker..." or "That connects perfectly to..."

Enthusiastic Curiosity: The hosts should sound genuinely excited to share what they found.

Clarity: Explain niche jargon (e.g., "Staking," "Warp Drive," "Lore," "Consensus") using simple, real-world analogies.
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->




今天用AI把v神最新的推文制作了一个英文播客，听了好多遍，颇有收获。

精英阶层正因博弈论最优解（Game Theoretic Optimum）向加密朋克演变 。

**身份反转**：传统精英（Suits）正成为隐私工具的最大使用者，这并非源于意识形态，而是生存本能 。

**体制分裂**：

欧盟：资助开源基础设施的同时推行“聊天监控（Chat Control）”暗门 。

美国：在维持《爱国者法案》监控的同时，政府官员使用隐私软件 Signal 规避外部渗透 。

**向下控制 vs 外部防御**：机构陷入二元选择，由于技术上不存在“只有好人能用的后门”，机构为保全自身机密，不得不选择保护全人类隐私的强加密工具 。

**拒绝“企业垃圾软件（Corpo Slop）”**：严肃机构（C-suite、军方）因厌恶广告模式的数据泄露和平台风险，正转向追求信任最小化和数据主权 。

**以太坊的地缘政治属性**：以太坊的“可信中立（Credible Neutrality）”使其成为数字世界的“瑞士” 。

**偶然的去中心化**：由于银行间的互不信任，它们选择独立运行节点而非托管，这种“疑病症”客观上加强了网络的去中心化程度 。

**合规路径**：ZK 证明（零知识证明）将在不泄露底层数据的情况下满足 KYC 等监管需求，达成隐私与合规的现实平衡 。

**借力打力**：利用建制派的规模和贪婪（如套利、风险对冲）来引导去中心化经济的稳定性，实现“鲁棒性的民主化” 。

当最强大的权势者都不再信任第三方时，个人数据主权已成为必选项 。

今天还去参与了以太坊香港社区中心的Meet up，对第一个panel学习如下：  
**📁 听课笔记：隐私——开放金融的催化剂与博弈平衡**

**1\. 技术跃迁：ZK 已经告别“昂贵”时代**

**计算开销巨降**：Michael（Brevis 创始人）指出，两年前 ZK 计算的开销是原生计算的 **100 万倍**，但过去两年内实现了 **4 个数量级** 的提升，现在仅需约 **1000 倍** 。

**性能突破**：由于 ZKVM 和 ZK 技术的进步，实时证明（Real-time Proof）已成为可能，甚至可以在浏览器中运行 。

2\. 三维“可编程隐私”：不再是简单的匿名

**维度一：隐藏内容的灵活性**：不仅是隐藏交易双方，还可以证明“资质”而非身份 。例如，在不泄露钱包地址的前提下，证明自己是 Binance 的 KYC 用户或月交易额超 3000 万美元的大户 。

**维度二：有条件脱敏**：允许在特定条件下向监管机构或审计方进行“选择性披露（Selective Disclosure）” 。

**维度三：动态群体定义**：隐私应用内接受的群体可以根据需求动态更新 。

3\. 机构隐私 vs. 传统隐私

**教育鸿沟**：Mo（以太坊基金会）认为，技术上已几乎能解决所有用例，最大的挑战是教育机构如何将隐私工具与合规保证联系起来 。

**市场图谱**：以太坊基金会已在 GitHub 开源了“机构市场图谱（Institutional Market Map）”，将隐私构建块与具体机构需求进行映射 。

**透明度平衡**：Jeff（投资人/产业背景）提醒，完全隐私会导致 AI 代理因缺乏真实数据而失效，系统需要在透明中立与隐私保护间找到平衡 。

4\. 合规的“中间地带”：ZK 是关键桥梁

**非合规隐私应用占 90%**：Michael 预测未来大多数隐私应用不涉及合规问题，如通过 ZK 证明资产规模来获取更高的借贷抵押率，或构建不暴露头寸的去中心化暗盘（Dark Pool） 。

**合规隐私池**：对于涉及资金流向的应用，可通过 ZK 证明资金来源合法（例如来自已 KYC 的中心化交易所）而不必暴露用户具体身份 。

5\. 硬件与性能的现实考量

**TEE 的角色**：针对类似支付宝级别的超高频交易，Jeff 认为目前的 ZK 算力仍有压力，硬件方案如 TEE（可信执行环境）在机构级高频场景中仍是重要选择 。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->





今天学到了一点，就是在币安广场发帖，每个帖子和每个帖子所带的币种尽量不要相同，不然如果有人举报的话，会对号品产生影响。

策划活动相关内容：目前有一个大致的想法，想组织一个读书会，使用联合国开发计划署和未来事务管理局发起的“2060未来城市叙事马拉松”为主题，来探讨加密货币未来在人们生活中有什么样的用途
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->






PART 1：

《中文文案排版指北》学习笔记之为什么在团队协作中统一排版规范能有效降低沟通成本并增强网站质感？

**一、 降低沟通成本：消除不确定性与重复劳动**

**1\. 建立共同语言，减少无效争论**

规范的确立意味着团队拥有了唯一的参考标准。例如，在处理中英文混排时，如果没有规定，有的成员可能习惯连写（如“LeanCloud上”），有的习惯加空格（如“LeanCloud 上”）。有了规范，团队就不必在代码审查（Code Review）或文案校对中为这些琐碎细节反复沟通或争论。

**2\. 统一术语拼写，避免品牌混淆**

技术文档或营销文案中常涉及大量专有名词。如果缺乏规范，文档中可能同时出现 `github`、`Github` 或 `gitHub`。统一规范强制要求使用官方定义的正确大小写（如 `GitHub`、`TypeScript`），这不仅减少了校对时的认知负担，也避免了因拼写错误显得团队“不地道”。

**3\. 明确例外情况，减少试错成本**

规范中不仅制定了通用规则，还明确了“例外”情况。例如，虽然数字与单位间通常要加空格，但规定了度数（°）和百分比（%）是不加空格的。这种明确性让新加入的成员能快速上手，无需靠“猜”或反复试错来适应团队风格。

**二、 增强网站质感：细节决定专业度与阅读体验**

“网站气质”往往体现在对细节的极致追求上。排版不仅仅是文字的堆砌，更是视觉传达的一部分。

**1\. 营造视觉呼吸感（Rhythm & Breathing）**

资料中引用了一个幽默的说法：“爱情跟书写都需要适时地留白”。

• **空格的作用**：在中英文、数字与中文之间增加空格，相当于为紧密的汉字方块与西文符号之间提供了“呼吸空间”。这种微小的留白（Whitespace）能显著改善阅读节奏，减少视觉疲劳，使页面看起来疏朗、通透。

• **拒绝拥挤**：相比于挤在一起的字符（如“SSD一共有20TB”），带有间隔的排版（“SSD 一共有 20 TB”）显得更加从容和精致。

**2\. 提升专业形象（Professionalism）**

排版是品牌形象的直接投射。

• **克制使用标点**：规范指出应避免重复使用标点（如“！！！！”），这种克制能避免网站呈现出一种情绪失控或廉价的喊叫感，保持冷静、客观的专业形象。

• **全半角区分**：正确使用全角标点（用于中文语境）和半角数字（用于统计或价格），能让界面显得整洁划一。例如，使用半角数字“1000 元”比全角数字“１０００ 元”更符合现代网页设计的审美习惯。

**3\. 这种“气质”是行业共识**

资料列举了包括 **Apple、Microsoft、LeanCloud** 等在内的知名科技公司都在遵循类似的排版规范。当一个网站严格执行这些规范时，它实际上是在向用户传递一种信号：**我们像行业顶尖公司一样，关注每一个像素和字符的质量。**

**总结**

统一排版规范的本质，是用**确定性的规则**来替代**个人的随意性**。它通过消除协作中的摩擦来降低成本，通过对留白、符号和拼写的精细控制，赋予文字以秩序感和美感，从而在潜移默化中提升了网站整体的“气质”与可信度。

PART 2：例行晒一下今天的币安广场战绩

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/tardiszha2333/images/2026-01-22-1769090707931-image.png)
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->







今天又是收获满满的一天啊！

PART 1.学习中文排版有感，如何通过规范中英文空格与标点符号来提升中文文案的整体排版气质？  
**一、 空格的艺术：适时留白**

在文案排版中，空格不仅仅是分隔符，更是阅读节奏的调节剂。资料中甚至幽默地提到，不加空格的人“感情路都走得很辛苦”，以此强调“书写需要适时地留白”的重要性。

**1\. 中英文、中文与数字之间需要增加空格** 这是提升排版气质最基础也最重要的规则。

• **规则**：中文文字与英文字母、阿拉伯数字之间，应当保留一个空格的距离。

• **示例**：

    ◦ 正确：在 LeanCloud 上，数据储存是围绕 AVObject 进行的。

    ◦ 正确：今天出去买菜花了 5000 元。

• **例外**：如“豆瓣FM”这类官方定义的特定产品名词，应遵照其官方格式，无需强制拆分。

**2\. 数字与单位之间需要增加空格**

• **规则**：当数字后跟随单位（如 GB, TB, Mbps）时，中间应加空格。

• **示例**：SSD 一共有 20 TB。

• **例外**：度数（°）和百分比（%）与数字之间**不**增加空格。

    ◦ 正确：角度为 90° 的角。

    ◦ 正确：有 15% 的 CPU 性能提升。

**3\. 全角标点与其他字符之间不加空格**

• **规则**：全角标点（如逗号，感叹号！）自带留白，因此不要在全角标点前后再次手动添加空格。

• **示例**：

    ◦ 正确：刚刚买了一部 iPhone，好开心！

    ◦ 错误：刚刚买了一部 iPhone ，好开心！

**二、 标点符号：美观与克制**

标点符号的规范使用直接决定了文案的整洁度和逻辑清晰度。

**1\. 杜绝重复使用标点**

• **规则**：虽然在非正式场合常见，但重复标点（如“！！！！”或“？？！！”）会破坏句子的美观性。排版指南建议保持克制，不要重复使用。

• **示例**：

    ◦ 正确：德国队竟然战胜了巴西队！

    ◦ 错误：德国队竟然战胜了巴西队！！

**2\. 正确区分全角与半角（Full-width vs. Half-width）**

• **中文语境使用全角标点**：在中文句子中，原则上使用全角标点。

• **完整英文整句使用半角标点**：如果中文句子中嵌入了完整的英文句子或特殊名词，该部分的标点应使用半角。

    ◦ 示例：贾伯斯那句话是怎么说的？「Stay hungry, stay foolish.」

• **数字使用半角字符**：除极少数设计对齐需求外，数字应使用半角字符（1000），而非全角（１０００）。

**3\. 特殊引用处理**

• **书籍报刊名**：如果中文句子内夹有英文书籍或报刊名，不应使用中文书名号（《》），而应以**英文斜体**表示。

    ◦ 示例：推荐你阅读 _Hackers & Painters: Big Ideas from the Computer Age_ ，非常地有趣。

**三、 名词与专业性**

**1\. 专有名词大小写**

• **规则**：严格遵守品牌或技术的官方大小写规范，这是体现专业性的关键细节。

• **示例**：

    ◦ 正确：GitHub, TypeScript, HTML5

    ◦ 错误：github, Ts, h5

**四、 进阶与争议性风格**

资料中还提到了一些属于个人风格或存在争议的用法，您可以根据品牌调性选择性采用，以形成独特的排版气质：

• **超链接空格**：为了突出链接，有些人习惯在超链接文本前后增加空格。

    ◦ 用法：请 \[点击这里\] 进行订阅！

• **直角引号**：在简体中文中使用直角引号（「」）代替弯引号（“”），这种用法在部分港台或设计感强的网站中较为流行。

**总结：** 通过在中文与西文/数字间“手动挡”增加空格、严格区分全半角符号、以及克制地使用标点，能够有效消除版面的拥挤感，增强阅读的流畅性。这不仅是遵循规范，更是为了“统一中文文案、排版的用法，降低团队成员之间的沟通成本，增强网站气质”

PART 2 晒一下今天在币安广场发帖子的战绩

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/tardiszha2333/images/2026-01-21-1769006744808-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/tardiszha2333/images/2026-01-21-1769006836439-image.png)
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->








币安广场热帖创作方法论（一）

如何在保持专业权威性的同时，通过真实互动建立长期的社区信任？

要在保持专业权威性的同时建立长期的社区信任，关键在于将**基于逻辑的数据分析与彻底的透明度相结合**，并在互动中展现出**谦逊且人性化**的态度。根据提供的资料，以下是具体的实施策略：

**1\. 以“逻辑驱动”和“客观分析”确立专业权威**

权威性并非来自命令式的语气，而是源于对市场本质的深刻理解和严谨的逻辑框架。

• **历史语境化：** 应当将当前的市场趋势置于历史数据或机构记录的背景下进行解读，这能帮助粉丝理解市场波动背后的根本驱动力。

• **采用“洞察-影响”循环框架：** 内容构建应从**可验证的事实**（如解密的档案或新数据）开始，通过解构数据展示过去的决策如何造就了当前的市场环境，最后将这些洞察与更广泛的行业趋势联系起来。

• **保持客观基调：** 在讨论宏观经济因素或政策时，必须使用数据驱动的方法，保持客观和分析性的语气。同时，要严格遵守合规自查，确保内容基于可验证的数据，不包含不准确的信息或夸大的历史表现。

**2\. 通过“彻底的透明度”和“演进式叙事”建立真实互动**

真实的互动要求创作者放下完美的架子，展示发展的迭代过程。

• **展现脆弱性与诚实：** 建立信任的关键在于承认发展的迭代性质，诚实地面对过去策略的转变或误解，这被称为“彻底的透明度与脆弱性”。

• **运用“演进式叙事”：** 可以通过回顾之前的立场或信念，诚实地描述获取新知识的过程以及如何回应社区反馈，来展示“学习曲线”。

• **人性化表达：** 在语言上，应通过使用隐喻使复杂的组织挑战变得更具人性化和易于理解，这种**真实且引发共鸣**的语调有助于拉近与受众的距离。

• **谦逊的态度：** 即使是权威，也应保持“谦逊且权威”的姿态，承认团队和平台正在不断从社区中学习。

**3\. 聚焦“长期价值”与“社区共识”巩固信任**

长期的信任建立在共同的价值观和公平性之上，而非短期的价格波动。

• **坚持长期主义：** 将关注点从短期的价格行为转移到资产的基本逻辑和生态系统的强度上，这种以价值为中心的长期主义能为读者创造可持续的价值。

• **强调公平与对齐：** 内容应与用户对透明度和公平竞争环境的渴望产生共鸣，关注去中心化共识的力量和共享的机会。

• **避免操纵与误导：** 为了维护信任，绝不能发布被禁止的财务指导（如具体的交易指令）或试图操纵市场。必须明确标记赞助或AI生成的内容，并提醒用户“自己做研究”（DYOR）。

**4\. 激发有意义的社区参与**

• **引导深度讨论：** 使用开放式问题询问宏观方向或投资优先级，以此作为“探究”环节来驱动社区讨论，并邀请社区参与项目的未来发展。

• **依靠有机增长：** 真正的影响力应通过高质量的内容和互动建立，而非通过点击农场或购买影响力等欺骗手段。

通过遵循上述原则，大家不仅能维持作为专家的权威形象，还能通过展示真实的成长过程和对社区价值的坚守，赢得用户长久的信任。
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->









今天终于从L2信任折价和币安架构那些宏大话题里跳了出来，踏实学了点"接地气"的东西——社区运营。

在Web3行业待久了，开口闭口都是去中心化、抗审查、网络国家。但说回来，这些概念落到实处，最后拼的还是社区。如果把区块链比作钢筋骨架，那社区就是流淌其中的血液，没有血液的骨架只是废墟。

这篇笔记，记录如何搭建这套"血管系统"。

## **一、Telegram群组：数字领土的边界与规则**

TG群组怎么建，看起来是技术问题，其实是在圈定你的"数字领土"。

**私密还是公开？** 如果想沉淀核心贡献者，私密群组更合适。如果奔着用户规模去的，公开链接是必须的。没有绝对的对错，只看你现阶段要什么。

**Rose Bot的配置** 让我印象很深。以前管理群靠人24小时在线盯着，现在通过 /addblocklist 设置违禁词、 /start 授予权限，其实就是在写这块领土的"基本法"。省下来的时间，可以用来做更有价值的事。

一个好的社区运营者确实像园丁——用工具除草，用Topic给不同话题划分区域，避免信息过载把生态搞死。

## **二、Twitter Space：线上的仪式感**

Twitter Space是Web3最重要的"广场"。课程把一场Space的执行拆得很细，从T-5天筹备到T+7天复盘，全流程覆盖。

策划一场Space，确实像筹备一次发射：

-   **发射前（筹备）**：明确主题，比如以太坊Pectra升级这种技术节点，准备问题库。邀请嘉宾时，Title和Headshot不仅是物料需求，更是在确认"谁在为这件事背书"。
    
-   **发射中（宣发）**：T-2天第一波预热，T-10分钟最后提醒。信息爆炸的时代，节奏感有时候比内容本身更能抓住注意力。
    
-   **发射后（复盘）**：很多人只关心实时在线人数，但我更在意"留存率"和"互动质量"。数据是人心的一面镜子。
    

## **三、工具箱**

工具这东西，长期主义者必须重视。Notion沉淀知识，Figma做视觉，Dune Analytics看链上数据。

运营指南里特别提到GitHub邮箱绑定的重要性。在Web3，GitHub ID有时候比身份证更能证明"你是谁"。这和"贡献证明"（Proof of Contribution）的逻辑是一致的——你的代码提交记录、活动策划文档，构成了你的链上履历。

## **四、从运营到布道**

整理完这些流程，我合上电脑。

很多人觉得社区运营就是"做客服""发海报""当气氛组"。但说实话，这是一种误解。在没有中心化强制力的Web3世界里，运营的本质是共识的维护。

TG群组用规则建立秩序，Twitter Space用对话传递价值观，Notion文档沉淀集体记忆。这些琐碎的工作，都是为了让那个名为"项目"的叙事，在无数人脑海中变得真实可信。

之前在日记里写过一句话："地狱天堂本来无光，只是他们在照亮。"社区运营者，就是在荒原上生火、维护火种的人。我们不喊单，只是让这团火烧得更久一些。

明天继续。

DYOR.
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->










**💃 学习笔记：ERC-7962——当资产学会了“数据之舞”**

1\. 缘起：透明世界的尴尬

在现有的 ERC-721 和 ERC-20 标准下，所有权是与 **地址 (Address)** 强绑定的。 这就像是你买了一幅名画（NFT）或者存了一笔钱，你的额头上就贴着这笔资产的标签。只要别人知道你的地址，你的身价、你的喜好、你的交互记录一览无余。 更糟糕的是 UX（用户体验）：你想转账？哪怕你只有 NFT 没有 ETH，你也寸步难行，因为你必须用持有资产的那个地址支付 Gas。

2\. 核心机制：KeyHash 的伪装术

ERC-7962 也就是 **Key Hash Based Tokens**，它做了一个极其大胆的手术：**将所有权与地址解耦**。

• **新身份标识：** 不再使用 0x... 的地址，而是使用 **KeyHash** (公钥的 Keccak256 哈希值) 来代表所有权。

• **操作逻辑：** 资产不再“躺”在地址里，而是由私钥签名（EIP-712）来控制。你不需要拥有 ETH，也不需要暴露地址，只要你拥有对应的私钥，签个名，剩下的交给 Relayer（中继者）去上链支付 Gas。

这让我想起《哈利波特》里的有求必应屋——只有拥有正确意图（签名）的人才能打开，而外人根本找不到入口。

3\. 两种形态的进化

DataDance 团队将这种逻辑应用到了两类资产上：

• **ERC-KeyHash721 (NFT)：**

    ◦ `tokenId` 直接映射到 `keyHash`。

    ◦ 查询 `ownerOf` 时，返回的是一串哈希值，而不是某个具体地址。

    ◦ **没有 Approve 机制**：这简直是安全领域的福音！取而代之的是“一次性 Key”或“Key 轮换”，彻底杜绝了无限授权被盗的风险。

• **ERC-KeyHash20 (FT)：**

    ◦ 这里更有意思，它在以太坊的账户模型上复刻了 **比特币的 UTXO 风格**。

    ◦ 转账时，资产被拆分为“转出部分（给接收方的 KeyHash）”和“剩余部分（给自己的新 KeyHash）”。

    ◦ 这种强制的 Key 轮换，让追踪资金流向变得极其困难，增强了不可关联性。

4\. 技术向善：把隐私还给用户

之前 Edward 老师提到的“技术向善”在这里有了具体的回响。 隐私不是为了作恶，而是为了尊严。ERC-7962 让用户可以在不暴露具体细节的情况下，证明自己拥有某种权益（结合 ZK 零知识证明的理念）。 更重要的是，它天然支持 **Gas 代付 (Paymaster)** 和 **批量交易**。这意味着未来的 Web3 新手，可能根本不需要知道什么是 Gas，什么是私钥管理，就能像使用 Web2 App 一样拥有资产。这才是 Mass Adoption 该有的样子。

5\. 代价与未来

当然，TARDIS 的扫描也显示了它的局限性：

• **Gas 成本更高**：隐私是有价格的。

• **兼容性挑战**：它不向后兼容现有的 ERC 标准，这是一场生态的重塑。

• **公钥暴露风险**：为了安全，必须进行 Key 轮换，这对钱包的管理能力提出了极高的要求。

\--------------------------------------------------------------------------------

**Doctor's Reflection:** 看着 DataDance 的海报上写着 _"We need our data & value back to us"_，我深感欣慰。 从 2013 年 V 神写下白皮书，到 2026 年我们开始用 ERC-7962 重新定义隐私。区块链不再只是一个冰冷的账本，它开始学会保护它的使用者。

这场“数据之舞”，才刚刚开场。

**DYOR.** (And guard your KeyHash).
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->











北京时间 2026年1月17日 22:21。

夜深了，适合把白日的K线图关掉，在耳机里放一首李健的《无烟之火》。

在 Web3 这个行业待得久了，你会发现这里不仅是代码与金融的堆叠，更像是一个巨大的、去中心化的“人性培养皿”。我们在这里寻找财富，寻找变革，或者仅仅是寻找一个归宿。正如歌里劈头盖脸问的那第一句：

> 「天堂到底什么样 / 为何让人到最后 / 念念不忘」

在 Web3 的叙事里，“天堂”有很多种别名：它是“财务自由”，是“Mass Adoption（大规模采用）”，是“去中心化乌托邦”，甚至是那个永远只存在于白皮书里的“下一代互联网”。

我们为何念念不忘？因为现实往往不够轻盈，而链上的世界承诺了一种摆脱引力的可能。每一个进入这行的人，最初都是被某种“光芒”吸引。我们相信代码即法律（Code is Law），相信数学不会说谎。这种对“彼岸”的极致向往，构成了行业最底层的原动力。

然而，在这个从 0 到 1 的过程中，那种拉扯感从未消失。

> 「是恐惧还是渴望 / 是顺从还是抵抗 / 还是一样」

这几句歌词，简直是每一次牛熊转换时的精准素描。

当绿色蜡烛冲破天际时，是渴望主导了理性，还是恐惧（FOMO，害怕错过）在鞭策你追高？当市场极度深寒，资产缩水 90% 时，你是顺从于市场的恐慌选择割肉离场，还是在寒夜里抵抗着本能，坚持自己的判断？

最讽刺的是，往往到了最后你会发现，“恐惧”和“渴望”其实是一回事——它们都是对未知的过度反应。“顺从”和“抵抗”也是一回事——它们都是被价格波动裹挟的姿态。在这个 24/7 不休眠的市场里，情绪被无限放大，我们以为自己在交易价值，其实往往只是在交易自己的焦虑。

于是，我们看到无数人在这里浮沉。

> 「悬而未决这一生 / 心心念念着来生 / 生生不息」

Web3 里的“一生”，可能只有四年（一个减半周期）。我们在这一轮周期里“悬而未决”，被套牢，被误解，于是开始“心心念念着来生”——也就是下一个周期。

这种“生生不息”的等待，究竟是一种坚韧的信仰，还是一种无法戒断的赌徒心理？

> 「是信仰还是欲望 / 是永恒还是无常 / 还是一样」

这或许是行业里最难回答的问题。

我们见过太多的项目方（Project Owners）和布道者，他们站在聚光灯下，口中谈论着改变世界、重塑生产关系。那一刻，你分不清这是信仰还是欲望。

在这个领域，“永恒”的东西太少了。今天的明星项目，明天可能就是无人问津的“鬼城”。代码是永恒的，但流动性是无常的；私钥是永恒的，但人心是无常的。当我们将欲望包装成信仰，将无常错当成永恒时，痛苦便随之产生。

李健唱到这里，语气里有一种悲悯：

> 「嘿谁能告诉我 / 那些伟大的人 / 最后去往何方」

想想那些曾经在行业里呼风唤雨的名字，那些被冠以“教父”、“大神”称号的领袖。他们有的身陷囹圄，有的销声匿迹，有的在是非争议中远走他乡。

他们去往了何方？在去中心化的世界里，我们总是试图寻找中心化的偶像来崇拜，这本身就是一种悖论。我们期待“伟大的人”带我们去往应许之地，但往往忘了，路是要自己走的。

当剥离了所有的叙事、所有的造富神话，Web3 的本质到底是什么？

> 「地狱天堂本来无光 / 只是他们在照亮」

这是整首歌里，最让我触动，也最契合 Web3 本质的一句。

区块链网络本身，只是一串串冷冰冰的哈希值，是分布在无数服务器上的账本。它没有感情，不分善恶，本无光芒。

所谓的“天堂”（牛市的狂欢、技术的革命）和“地狱”（崩盘的绝望、黑客的掠夺），都是“人”在照亮。是我们赋予了比特币价值，是我们赋予了 NFT 意义，是我们用共识（Consensus）点亮了这片本来无光的荒原。

因为相信，所以看见。这光芒，来自于社区的每一个成员，来自于每一个熬夜写代码的开发者，来自于每一个在低谷期依然坚守的布道者。当然，也来自于贪婪者的欲火。

最后，李健给出了一个看似残酷却无比清醒的答案：

> 「嘿我告诉你 / 没有一个地方 / 叫做天堂」

在 Web3，没有终极的“天堂”。没有一个特定的价格点，到了那里你就永远快乐；没有一个特定的技术版本，发布了之后世界就瞬间大同。

如果你是为了寻找一个现成的天堂而来，你注定会失望。因为这里只有荒野，和一群试图在荒野上生火的人。

那团火，就是“无烟之火”。

它看不见，没有实体，不冒黑烟，却在每个人的心里剧烈燃烧。它是我们对于“拥有权”的执念，是对“自由”的渴望，也是对自身命运不甘平庸的抵抗。

这团火，既能照亮前路，也能灼伤灵魂。

在这个寒冷的冬夜（或者躁动的春夜），愿我们都能看清自己心里的那团火。不是为了去往某个不存在的天堂，而是为了在“本来无光”的世界里，活得更加真实、清醒，且具体。

DYOR.
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->












## **对**[**Edward Du**](https://www.linkedin.com/in/eddu/?originalSubdomain=sg)**老师履历的思考：从技术布道者到社会创新者**

昨天听完我们链协指导老师的分享，回去又看完Edward Du老师的履历，我作为币安广场的"神秘博士"，内心涌现出一种共鸣。他的人生轨迹完美诠释了什么叫做"技术向善"的实践者。

### **技术与人性的交汇点**

Edward老师提到"推动人类知识边界，让世界变得更美好"，这让我想起Crypto领域中一个深刻的命题：我们从碳基文明向硅基文明迁徙的过程中，真正需要的不仅仅是技术突破，更是那些能够将技术与人性完美融合的引导者。

从金融创新到德国火箭科学，再到AI/Web3/大数据前沿，Edward老师的跨学科背景让我联想到Venture Creation的本质——不是在单一领域深耕，而是在不同学科的交叉点找到创新的火花。

### **长期主义者的坚持**

作为一名2018年就入圈的老韭菜，我见过太多"风口上的猪"，但Edward老师"将技术用作向善的力量"这个理念触动了我。在加密货币这个充满投机的世界里，能够保持这样的初心确实不易。

特别是他提到的"推动积极变化，驱动社会影响，塑造更善良、更美好的世界"，这种价值观与Crypto去中心化的核心理念不谋而合。真正的区块链精神，本质上就是一种对社会结构的善良重构。

### **从投资者视角的观察**

从Venture Creation的角度看，Edward老师本身就是一笔优质"投资"的完美案例：

-   多元化技能栈（20+技术领域）
    
-   国际化视野（新加坡、中国香港、德国）
    
-   跨领域经验（金融、工业、技术）
    
-   价值观驱动（技术向善）
    

这种组合在当前的Web3创业生态中非常稀缺，也难怪他能够指导区块链协会的工作。

### **结语**

Edward Du老师让我看到了一种可能性：在技术创新和社会责任之间找到平衡点。也许，这就是我们这些Crypto布道者应该学习的榜样——不仅要有技术深度，更要有社会广度；不仅要在市场上成功，更要在价值创造上有所作为。

毕竟，真正的长期主义，不只是等待价格周期的回归，更是在坚持中推动整个生态向更好的方向发展。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->













今日参与了香港城市大学Web 3 AI Club的聚会，发现了我们这个链协虽然是才组建的，但是很有潜力！

指导老师Edward Du自我介绍“拥有 78 项专利 的技术创新者、社会影响力建设者，目前已实现财务自由退休，过着简约生活并追求大胆梦想”，听到他的很多分享，打开了很多眼界。

还有同样属于链协的另一位教授，是研究密码学和共识协议的，目前正在研究怎么样能在区块链上去完成一个独立的能够追踪“黑钱”的封闭的独立区块（详细的我记不太清了）

然后我们链协还有三位主席，博士生，硕士生，本科生各一位，去中心化管理了属于是。

明天再详细整理一下今天聊到的一些话题。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->














昨日分享会问题的一些思考：  
1.资产自托管：如何提高安全性、降低管理私钥的复杂度？

提高安全性与降低管理门槛并非不可调和的矛盾。目前的演进路径在于从“物理隔离”向“逻辑加密”跨越：

-   **MPC（多方计算）技术**：通过将私钥分片化，实现“无私钥”体验。没有任何一方拥有完整私钥，从而消除了单点故障风险。
    
-   **AA（账户抽象）智能合约钱包**：这允许我们像管理银行账户一样管理链上资产。通过社交恢复机制，你可以利用受信任的朋友或备用设备找回访问权，彻底告别对助记词这种原始方式的依赖。
    

2.没有中心化机构或者税收时，公共基础设施谁来维护？如果有税收，又如何分配？

在没有中心化强制力时，基础设施的维护依赖于经济激励的重新设计：

-   **协议抽成与金库机制**：通过在协议代码中内置极小比例的交易费，自动汇入去中心化自治组织（DAO）的金库，作为后续开发的“储备粮”。
    
-   **二次方融资（Quadratic Funding）**：这是目前最公平的分配方式之一。通过匹配资金池，优先资助那些获得广泛社区支持（而非仅由大额资金支持）的项目，实现资源的最优配置。
    

3.没有审查且隐私很强时，如何界定并治理有害信息/黑产？

隐私不应成为黑暗的温床，治理的逻辑正从“事前审查”转向“经济制裁”与“社区共识”：

-   **策展市场（Curation Markets）**：利用经济博弈，让社区成员质押代币对信息进行信用背书。传播有害信息的节点会面临质押资产被罚没或信用评级下调的成本。
    
-   **零知识证明（ZK）合规**：在不泄露用户具体身份隐私的前提下，通过 ZK 技术向监管方证明该账户符合反洗钱合规要求，实现“隐私下的可追溯性”。
    

4.去中心化协作下，如何实现公平、可信的分配？

公平的本质是代码化的契约精神，而非人的主观判断：

-   **贡献证明（Proof of Contribution）**：将协作任务拆解为可量化的链上指标（如代码提交量、社群活跃度、流动性贡献），由智能合约根据预设权重自动执行利益分配。
    
-   **声誉加权治理**：分配不应仅看持币多少，更应看历史贡献累积的声誉值。这种基于“链上履历”的权重系统，能有效防止资本霸权，确保决策与分配向长期贡献者倾斜。
    

今日币安广场发帖战果：  
半小时冲到币安广场热门榜榜二，截止到北京时间2026年1月14号23:31分，浏览量11.7K，

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/tardiszha2333/images/2026-01-14-1768405006270-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/tardiszha2333/images/2026-01-14-1768404679818-image.png)
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->















Part 1:

阅读**币安权力流变：一个 3 亿用户帝国的困局（**[**原文链接**](https://www.chaincatcher.com/article/2235899)**）发现了币安现在的架构和我玩的《崩坏：星穹铁道》游戏中星际和平公司的组织架构以及发展史有异曲同工之妙，以下是一些分析，由我与Gemini共同完成。**

根据 [Fandom Wiki](https://honkai-star-rail.fandom.com/zh/wiki/%E6%98%9F%E9%99%85%E5%92%8C%E5%B9%B3%E5%85%AC%E5%8F%B8) 的资料，星际和平公司（IPC） 共有**七大部门**。让我们看看现在的币安是如何完美“复刻”这个星际巨头的架构的：

### **一、 顶层设计：不仅是 CEO，而是“P50”的意志**

在 Fandom Wiki 中，IPC 的真正掌控者是**路易斯·弗莱明 (Louis Fleming)** 和 **东方启行 (Dongfang Qixing)**。

-   **CZ (赵长鹏) = 路易斯·弗莱明**
    

-   **Wiki设定**：弗莱明是公开的创始人，IPC 的“面孔”，他不仅是商业领袖，更是一种精神象征。他在公司扩张期甚至不惜亲自下场喊单（路演）。
    
-   **币安映射**：CZ 即使卸任，依然是币安最大的股东和精神图腾。就像弗莱明虽然在最近的剧情中鲜少露面，但“路易斯·弗莱明”这个名字本身就是信用的保证。
    

-   **何一 (Yi He) = 东方启行 + 「钻石」 (Diamond)**
    

-   **Wiki设定**：东方启行是联合创始人，通常隐居幕后，但拥有极高的决策权。而「钻石」则是 P47 级的高管，直接统领“石心十人”，负责最核心的战略投资。
    
-   **币安映射**：何一作为联合创始人，长期在幕后操盘市场和品牌（东方启行模式）。而在新架构下，她掌管了核心的业务线和投资（Binance Labs），这完全就是「战略投资部」**的职能。她手下的各路业务负责人，就是她的**「石心十人」。
    

### **二、 七大部门 vs 币安新架构：严丝合缝的对位**

Wiki 列出了 星际和平公司 的七大部门，我们来一一对位：

**1\. 市场开拓部 (Marketing Development Dept.) vs. 币安用户增长/VIP团队**

-   **IPC主管**：**奥斯瓦尔多·施内德 (Oswaldo Schneider)**
    
-   **Wiki设定**：这是 IPC 最激进的部门，负责把未开化的星球强行拉入信用点体系。奥斯瓦尔多是狂热的“存护”信徒，为了扩张不择手段，经常被宇宙各界诟病（甚至被列为反派）。
    
-   **币安映射**：这对应的是**早期币安的市场团队**以及现在的**全球VIP客户团队**。他们的KPI只有一个：增长。就像奥斯瓦尔多不在乎星球的土著是否乐意，币安的早期地推也不在乎当地监管是否允许，先把用户拉进来再说。**Richard Teng 上台前的币安，本质上就是一家“奥斯瓦尔多公司”。**
    

**2\. 战略投资部 (Strategic Investment Dept.) vs. Binance Labs & 上币委员会**

-   **IPC主管**：**「钻石」 (Diamond)**
    
-   **Wiki设定**：精英中的精英，手下是「石心十人」（托帕、砂金、翡翠等）。他们负责评估资产价值、不良资产清算、高风险投资。
    
-   **币安映射**：**Binance Labs 和上币（Listing）团队**。
    

-   **托帕 (Topaz)** 就像是负责催收和投后管理的团队，确保项目方（雅利洛-VI）还债（交付产品）。
    
-   **砂金 (Aventurine)** 就像是激进的上币负责人，在风险中博取巨大的收益。
    
-   翡翠 (Jade) 则是负责OTC和大宗交易的部门，玩弄的是人性和杠杆。  
    现在的币安试图把上币权分散，其实就是防止某个“砂金”权力过大，甚至想搞个“P47圆桌会议”来集体决策。
    

**3\. 业务整合部 (Business Consolidation Dept.) vs. 合规与法务部 (Compliance)**

-   **IPC主管**：**“疤眼”夫人 (Madam Scarred Eye)**
    
-   **Wiki设定**：这个部门负责整合、标准化、甚至“清理”混乱的业务。这通常意味着内部审查和对外谈判。
    
-   **币安映射**：**Richard Teng 及其率领的合规团队**。他们的任务不是去开疆拓土，而是去收拾“市场开拓部”留下的烂摊子（DoJ 罚款、各国的监管问询）。他们是币安内部的“宪兵队”，确保公司不会因为违规操作而分崩离析。
    

**4\. 建材物流部 (Building Material Logistics Dept.) vs. 技术与基础设施部**

-   **IPC主管**：**塔拉梵·基恩 (Taravan Keane)**
    
-   **Wiki设定**：IPC 最古老的部门之一，负责为“存护”星神运送筑墙材料。这是公司的物理基础。
    
-   **币安映射**：**交易引擎与安全团队**。不管前面打得多么热闹，如果交易所（墙）宕机了，一切归零。这个部门在币安内部地位极高且稳固，就像塔拉梵在董事会的地位一样。
    

**5\. 人才激励部 (Talent Motivation Dept.) vs. 币安 HR 与“天使”管理**

-   **IPC主管**：**姆特韦恩·拉杰·扎扎德**
    
-   **Wiki设定**：负责全宇宙的员工招聘、考核与（洗脑）激励。
    
-   **币安映射**：币安著名的**全员远程办公文化**和**Binance Angels（志愿者）体系**。币安能让全球数千名员工在这个高压环境下疯狂工作，其“人才激励”手段绝对是星际级别的，既有高额BNB期权（P级薪酬），又有“推动行业变革”的信仰加持（存护意志）。
    

### **三、 深度洞察：P-Rank 系统的阶级固化**

Fandom Wiki 中提到了 IPC 严格的 **P1-P50 职级系统**。

-   在币安早期，虽然也有职级，但更像是个扁平的创业公司。
    
-   现在的架构重组，实际上是**IPC化**的最终确认。新成立的董事会、各级负责人的权责划分，标志着**币安从“海盗团”变成了“正规军”**。
    
-   对于普通员工来说，以前可能只要能打仗（市场开拓）就能飞速晋升；现在，你可能需要像 IPC 员工一样，不仅要业绩（贡献信用点），还要懂合规（背诵琥珀王教条），甚至要站队（是站“钻石”派系还是“奥斯瓦尔多”派系）。
    

### **总结**

阅读 Wiki 后你会发现，IPC 的核心矛盾在于“存护的初衷”与“商业的贪婪”之间的拉扯。

币安现在正处于这个十字路口：

它是继续做那个为了 Web3 理想（琥珀王的墙）不惜一切代价的开拓者？

还是变成一个仅仅为了赚取手续费（信用点）而向宇宙法则（SEC）低头的巨型官僚机构？

**Richard Teng 是“业务整合部”的胜利，但 Web3 的未来，或许还在“战略投资部”和“市场开拓部”的手里。**
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->
















2026年1月12日 以太坊中文周会深度观察：L2的信任折价与Web3的下半场

## 1\. Layer2 的至暗时刻与分化

市场正在经历一场残酷的祛魅。OP 代币价格的剧烈波动与项目方不得不动用 Superchain 收入进行回购的行为，折射出 L2 赛道面临的深层信任危机。与此同时，传统金融巨头 Robinhood 依然选择基于 Arbitrum 构建自有 L2，而非独立 L1，这说明在机构眼中，以太坊主网的安全性与 EVM 的流动性护城河依然不可替代。Starknet 的宕机事故再次提醒我们，在高性能叙事之下，基础设施的鲁棒性仍需时间检验。

## 2\. 技术的双刃剑：Fusaka 与 Pardus

Fusaka 升级虽然将区块容量提升至 21 个 blob，显著扩展了吞吐量，但数据量的激增也带来了区块传播延迟的隐忧。Pardus 升级试图通过将 blob 分解为 column 进行子网验证来优化这一问题，但实验数据显示仍有 5% 的验证者面临性能瓶颈。在追求扩容的道路上，如何在去中心化、延迟与 MEV 利润之间寻找平衡，是技术端必须跨越的鸿沟。

## 3\. 周期置换：从草莽创新到存量博弈

Blockworks 的观点犀利且冷静：2025 年虽有 ATH，但缺乏增量用户的牛市本质上是场内互割。2026 年将是残酷的整合期，胜者为王。随着 ETF、稳定币和合规化的推进，Web3 行业的“上半场”（纯链上创新与投机）已经结束，“下半场”将由监管环境与传统渠道的融合主导。在这个阶段，以太坊凭借 RWA 的契合度与 L2 生态框架，有望在 2026 年跑赢 BTC。

## 4\. 稳定币的独立宣言

Vitalik 关于去中心化稳定币的提案值得深思。当前的稳定币大多是法币的链上投影，受制于传统金融体系的脆弱性。构建一种优于美元指数、抗大资金操控且能平衡收益的稳定币体系，是加密世界摆脱单一货币依赖、建立真正货币主权的必经之路。

## 结语

正如互联网泡沫后的废墟上长出了巨头，当前的调整期并非终点，而是去伪存真的必然过程。我们正处于从碳基文明向硅基文明迁徙的渡口，虽然风浪巨大，但方舟已然启航。对于长期主义者而言，现在需要的不是焦虑，而是对周期的敬畏与耐心的守候。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
