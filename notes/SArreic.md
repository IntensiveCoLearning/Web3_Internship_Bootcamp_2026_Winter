---
timezone: UTC+8
---

# SArreic

**GitHub ID:** SArreic

**Telegram:** @sarreic

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->
Today's meeting summary features renowned Web3 developer educator Austin Griffith, founder of Scaffold-ETH and BuildGuidl, who delivers a dense, practical session demystifying core Ethereum development concepts. He begins by emphasizing on-chain governance and fund security, explaining multi-signature wallets. For instance, in a crowdfunding scenario like buying an island, funds should be held in a smart contract requiring 10 out of 15 trusted members to sign for any transaction, thereby eliminating single points of failure and trust issues.

A major point of confusion for newcomers is the "Approve & Transfer" pattern in smart contracts. Austin clarifies that users cannot send tokens directly to a contract like a vending machine. Instead, they must first approve the token contract, authorizing it to allow the vending machine to withdraw a specific amount, after which the machine's function is called to execute the transfer. This ensures the contract can perform its logic, such as dispensing an item, simultaneously with receiving payment.

The discussion then covers foundational DeFi mechanisms. Decentralized exchanges (DEX) like Uniswap rely on constant product formulas and incentivize liquidity providers (LPs) with fees to create permissionless, always-on trading hubs. A pillar of DeFi is over-collateralized lending, where borrowers must lock collateral worth more than the loan value to prevent bad debt. If the collateral value falls below a threshold, liquidators can trigger a liquidation function, selling the collateral for a reward, which maintains the system's solvency.

Austin also highlights a critical pitfall: generating true randomness on-chain is difficult due to blockchain's deterministic nature. Using a previous block hash is insecure and predictable. A robust solution is the Commit-Reveal pattern, where a user first submits an encrypted number, reveals it after block confirmation, and combines it with a block hash to produce a random number even node operators cannot manipulate.

For beginners, Austin recommends starting with core concepts rather than memorizing syntax, suggesting resources like Speedrun Ethereum to grasp ideas like the state machine and understand why the EVM doesn't support decimals, requiring amounts to be handled as integers. Leveraging AI tools like Cursor or ChatGPT can aid development—for example, by generating a vending machine contract—but the key is comprehending the underlying logic, such as the Approve pattern. Finally, he advises reading battle-tested contracts from top projects on Etherscan, such as OpenZeppelin, Uniswap, or Aave, to learn how they handle security and edge cases.
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->

Today's learning material focuses on the **architecture of a Decentralized Application (DApp)**, illustrating the critical components and data flow that differentiate it from a traditional Web2 application.

The core concept is the **separation between on-chain and off-chain operations**. At the heart lies the **Blockchain**, serving as the decentralized, immutable source of truth (like `balances[address] = 123`). However, reading data directly from the blockchain via an RPC provider is slow, with latencies in the thousands of milliseconds. Therefore, a robust off-chain infrastructure is essential for performance. A **Server** (built with frameworks like Next.js) hosts the application logic. It interacts with a **relational database** (e.g., for user profiles) and crucially, employs an **off-chain cache layer** to efficiently serve blockchain data. This cache is populated by **indexing** services like Dune or The Graph, which prefetch and transform on-chain data into a queryable format, enabling fast queries that would be impossible directly from the RPC.

On the client side, the **User Browser** runs the **Frontend**. It connects to the blockchain via a **Wallet Extension** (like MetaMask) for transaction signing. The frontend uses libraries like **Wagmi** to simplify interactions, calling smart contract functions and reading cached balances from the server's backend. The entire architecture forms a loop: user actions trigger on-chain transactions, which are indexed, cached, and then reflected back in the application's user interface, all while relying on the blockchain as the ultimate ground truth.
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->


Today's learning about smart contract development provides a foundational introduction to Solidity smart contract development. It begins with the **Ethereum Virtual Machine (EVM)** architecture, explaining its four key execution areas: **Stack, Storage, Memory, and Calldata**. A crucial focus throughout is **Gas optimization**, highlighting that storage operations are the most expensive. Core strategies involve minimizing storage writes and using efficient data types like smaller integers (`uint8`) instead of strings where possible.

The document systematically guides through building an **ERC-20 token contract**. It covers the necessary state variables (like `totalSupply`), the dual-ledger system for balances and allowances, and the implementation of core functions such as `transfer` and `approve`. Key programming concepts are explained in the context of security and efficiency: **function visibility** (`public`, `external`), **state mutability** (`view`, `pure`), the **event** system for off-chain listening, and **error handling** using `require`, `revert`, and custom errors. The **modifier** feature is emphasized for code reuse and access control, directly contrasting it with repetitive `require` statements.

Finally, the material addresses practical development and security. It recommends using **Remix IDE** for writing, compiling, and deploying contracts, and stresses the importance of the Solidity version pragma. Beyond the standard, it introduces essential ecosystem tools: **OpenZeppelin** for audited contract libraries, **Proxy patterns** for upgradeability, and the **Diamond Standard (EIP-2535)** for modular contracts. A critical security section deconstructs a common scam where a wallet's private keys are leaked: the address is often a restricted contract, and front-running bots instantly steal any gas sent to it, underscoring the paramount rule of never importing unknown private keys.
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->



Today's learning material focused on practical Web3 community operation skills, particularly on building and managing **Telegram groups** and organizing **Twitter Spaces**. The process for Telegram involves creating a group, configuring basic settings, utilizing management bots like **@MissRose\_bot** for moderation, and monitoring group statistics. It's crucial to verify bot authenticity to avoid phishing scams.

Organizing a Twitter Space follows a structured lifecycle: **Preparation, Promotion, Execution, and Review**. The preparation phase requires defining the topic, building a question bank, and inviting suitable guests with clear communication. Promotion involves creating materials (graphics, copy, the Space link) and distributing them across channels with a planned timeline. During execution, the host should manage the flow: introduction, guest Q&A, and audience interaction. Finally, the review stage involves analyzing key metrics like viewership and peak listeners to identify areas for improvement in future events.
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->




Today's Ethereum Chinese weekly meeting covered the latest dynamics in the blockchain ecosystem and emerging technical proposals. Key updates included Ethereum's clarified future focus on decentralization, self-sovereignty, and privacy, with plans to integrate ZK-EVM and BAL for node simplification, and to leverage ORAM and PIR technologies for privacy protection. The core developers' meeting also ratified four new EIPs to address gas refund vulnerabilities, enhance ETH transfer logs, and improve developer experience. Significant progress was noted in other ecosystems: BSC completed a chain upgrade, boosting block production speed by 40% and reducing finality to seconds; Klaytn reduced the max supply of its K token; and Tron achieved integration with MetaMask, facilitating cross-chain asset swaps.

Several new technical solutions were introduced. The **Base Rollup hybrid scheme** by Retailink combines the strengths of Base and Sequencer Rollups, using a "slot-ending block" mechanism to ensure low latency while maintaining synchronous composability with L1. An improvement to **Data Sampling (DS)** algorithms was discussed, proposing a shift from fixed-rate coding to more efficient **"Rateless Coding,"** which could significantly lower client-side network sampling and bandwidth requirements. Additionally, the MAGAIS team open-sourced three projects: **Tiny Merkle Authenticated Trie**, a new data structure designed to replace Merkle Patricia Tries and reduce I/O and memory overhead; **Staylist Validator**, a lightweight validator built on this structure; and **MAGAIS EVM**, a supporting virtual machine that allows for larger contract code sizes and employs a multi-dimensional gas model. Market analysis highlighted the current policy-driven phase, with 2025 seeing a peak in crypto regulation and significant growth in scam cases, while Bitcoin ETFs attracted approximately $31 billion in net inflows, becoming a major institutional investment vehicle.
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->





Today's presentation explores a forward-looking Ethereum token standard proposal: **ERC-7962 - Key Hash Based Tokens**. It aims to address two critical shortcomings of current standards (like ERC-20 and ERC-721): **privacy** and **user experience (UX)**. Traditionally, token ownership is publicly tied to on-chain addresses, making transactions traceable and identities potentially linkable. ERC-7962's core innovation replaces the ownership identifier from an address to a `keyHash` (the hash of a public key). This means asset ownership is proven off-chain via ECDSA signatures against the `keyHash`, while the actual owner's address or public key never needs to be stored on the blockchain, significantly enhancing privacy.

The design offers substantial UX improvements. Since transactions are validated by signatures rather than the sender holding ETH for gas, it enables seamless **gas sponsorship**, where any relayer can pay the transaction fee. This allows users to transfer assets without needing to hold the native cryptocurrency, understand gas mechanics, or even manage wallet addresses actively. The standard naturally supports batch transactions and integrates smoothly with account abstraction and paymaster infrastructures. It defines two subtypes: **ERC-KeyHash721** for NFTs and **ERC-KeyHash20** for fungible tokens, with the latter employing a UTXO-like model for further privacy.

However, these benefits come with trade-offs. The gas cost is higher than traditional standards due to signature verification and on-chain computations. While the public key itself is not stored on-chain, it is exposed in the transaction calldata during operations, necessitating key rotation for long-term privacy. The standard is not backward compatible with existing ERC-20/721 tokens and places a higher responsibility on users for secure key management. Proposed use cases include privacy-sensitive assets, beginner-friendly wallets that abstract away complexity, enterprise-grade batch distributions, and as a core component within broader account abstraction ecosystems.
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->






In today's reading of "Ze’s Crypto Journal: From Underground Gambling in Texas to the Monad Airdrop—The Unchanging Law of the Jungle," a profound parallel is drawn between the opaque power structures of the gray economy and the seemingly decentralized yet deeply manipulated crypto space. The narrative follows the author's former boss attempting to lure a debt-ridden young woman into running an illegal Texas Hold’em club—a high-risk endeavor where legal consequences would fall entirely on her, while the orchestrator remained shielded. This mirrors the recent controversy surrounding the Monad airdrop, where early contributors were excluded, fake links proliferated, and insiders enjoyed preferential treatment. In both cases, rules are designed not to ensure fairness, but to serve those who control them.

The reflection extends to the mechanisms of exploitation prevalent in crypto trading. On platforms like Hyperliquid, vulnerabilities such as flash squeezes and wick manipulation allow large players to liquidate retail traders with surgical precision—events that have led to millions in losses within hours. These are not merely technical failures but systemic features that benefit the powerful. Just as the gray economy preys on the desperate with promises of quick wealth, the crypto market often operates under a facade of decentralization while enabling centralized control and selective enforcement.

At its core, the essay challenges the original promise of blockchain—transparency, fairness, and decentralization—and contrasts it with today’s reality: a space where data can be cherry-picked, rules reinterpreted, and retail participants systematically marginalized. It argues that in any arena, whether offline or on-chain, morality often becomes a constraint for the vulnerable, while rules serve as tools for the powerful. True empowerment does not come from chasing speculative gains within rigged systems, but from building competence, understanding risk, and—where possible—shaping the rules rather than merely following them. The path forward in Web3 lies not in blind participation, but in conscious, critical engagement that acknowledges both its transformative potential and its recurring failures of justice.
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->







Today’s learning focused on the convergence of Agentic AI and Web3, highlighting the shift from reactive conversational models to autonomous, action-oriented systems. While traditional large language models (LLMs) generate text based on prompts, Agentic AI integrates reasoning, memory through vector databases, and external tool usage via APIs—enabling persistent, goal-driven execution in real-world environments.

A critical gap in current AI infrastructure is the lack of verifiable identity and trust. Issues such as identity spoofing, untraceable actions, and ungoverned “shadow AI” deployments pose significant risks. Blockchain emerges as a foundational solution, providing tamper-proof identity registries, reputation systems, and validation mechanisms using TEE or zkML proofs. This enables what is termed an “on-chain passport” for AI agents, answering the essential questions of “Who are you?” and “Can you be trusted?”

Moreover, the rise of machine-to-machine (M2M) economies demands new payment models. Traditional subscription-based or manual payment systems are ill-suited for microtransactions and API-based usage. Protocols like x402 enable native HTTP-level machine payments—supporting pay-per-file, pay-per-inference, and other granular settlement models. Notably, by 2025, bots and autonomous agents already facilitated 70% of stablecoin transfers, signaling a shift toward an economy where AI agents operate with their own wallets and financial autonomy.

Frameworks such as SpoonOS abstract away the complexity of building such integrated systems. They provide developers with ready-to-use components for identity, payment, and compliance—allowing them to focus on enhancing AI capabilities rather than underlying infrastructure. Use cases include DeFi trading agents with verifiable performance history, data marketplace agents that monetize datasets via microtransactions, and autonomous coding assistants that charge per bug fix.

From a regulatory perspective, immutable on-chain logs offer transparent and auditable trails of AI decisions and transactions, aligning with requirements like the EU AI Act for high-risk systems. This moves AI operations from “black box” models to accountable, monitorable systems.
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->








Today’s learning focused on the evolving threat landscape in Web3, highlighted by key data, attack methodologies, and emerging challenges. In 2025 alone, the ecosystem suffered over $3.5 billion in losses across more than 1,200 major security incidents, reflecting a trend toward both highly targeted attacks and widespread automated threats. High-profile breaches—such as the multi-signature compromise at Bybit, AI-driven deepfake social engineering at ULXINK, and complex DeFi contract exploits like Balancer—illustrate how attackers are leveraging advanced techniques, supply chain vulnerabilities, and AI to execute sophisticated and damaging campaigns.

Notably, attack vectors have diversified. “Precision hunting” tactics, often linked to state-sponsored actors like North Korea’s Lazarus Group, accounted for the majority of high-value institutional thefts. Meanwhile, “spray-and-pray” methods such as phishing, private key theft, and rug pulls remained pervasive, increasingly enhanced by AI-generated content and automated tooling. Emerging threats also include novel phishing schemes exploiting EIP-7702 delegation features, self-propagating supply chain worms like Shai-Huud, and AI-aided smart contract vulnerability discovery—marking a shift toward more automated, scalable, and stealthy attacks.

From a legal and compliance perspective, while specific regulations around Web3 terminologies remain underdeveloped in many jurisdictions, the industry is increasingly expected to adopt proactive security governance. This includes implementing security-by-design in development, conducting continuous monitoring, establishing rapid response protocols, and enhancing user education on practices such as verifying transactions, isolating sensitive wallets, and using safety tools like Scam Sniffer.

Overall, today’s review underscores that security in Web3 is no longer merely a technical afterthought, but a foundational requirement for mainstream adoption. Both individual users and projects must cultivate stronger security habits, embrace multi-layered protection strategies, and stay informed on evolving threats—especially as AI continues to reshape the offensive and defensive dynamics of the space.
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->









In today’s review, I revisited essential concepts and practical skills in Web3, with a focus on wallet security and risk prevention. I learned that wallet signature authorizations can expose funds to theft—even without private key leakage—due to malicious or vulnerable website interactions. Tools such as the Scam Sniffer browser extension help detect dangerous sites, though manual judgment remains essential. Best practices include keeping only minimal gas funds in MetaMask and considering Apple devices for added security.

The session also covered token identification, emphasizing that the contract address is the true identifier rather than the token name. Anyone can create tokens with identical names or displayed prices, making it critical to verify addresses on decentralized exchanges and look for “verified” labels or warning symbols. For careers in Web3, practical takeaways include maintaining a strong GitHub portfolio for technical roles, curating case studies for operations positions, and leveraging Twitter to build a personal brand—beginning with content in one’s native language for better engagement.

Other key points included tools like Dune for on-chain analytics and RootData for aggregated project insights. Roles such as Developer Relations and Web3 Product Manager demand both technical depth and community engagement, while BD positions vary between driving user growth and leveraging partnerships. From a technical perspective, stablecoins were explained in terms of centralized issuance versus decentralized, over-collateralized models like DAI, which rely on arbitrage to maintain peg stability.

Finally, the discussion touched on blockchain fundamentals such as 51% attacks, the trade-offs of IPFS versus centralized storage, and the competitive landscape among public chains. Innovation—whether in speed, privacy, or ZK proofs—remains a key differentiator for new chains, rather than short-term speculative gains. Overall, today’s review reinforced both the tactical knowledge needed to operate safely in Web3 and the strategic mindset required to grow within this evolving ecosystem.
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->











Today, I installed the MetaMask and Solflare wallet extensions on my Edge browser, enabling me to manage transactions on both Ethereum and Solana networks directly through supported websites. I also acquired Sepolia ETH via a Proof-of-Work faucet using my Ethereum testnet wallet address, allowing me to begin interacting with testnet applications.

During today’s learning session, I reviewed the foundational principles of Web3 and blockchain technology, covering core functions and underlying architectural logic—such as how transactions are executed and how smart contracts operate on Ethereum and other blockchain platforms. This refreshed my understanding of the technical groundwork that supports decentralized applications and ecosystems.

Building on this, I also explored current industry trends discussed in recent Web3 forums, including the growing emphasis on layer-2 scaling solutions and real-world asset tokenization. These developments further illustrate how blockchain infrastructure continues to evolve to support broader adoption and more complex use cases.

Overall, today’s hands-on setup and conceptual review strengthened my practical readiness and theoretical clarity as I continue to engage with the Web3 space—whether through testing dApps, contributing to projects, or preparing for a career in this fast-evolving field.
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->













Today, I expanded my understanding of the Web3 space by attending the Ethereum Chinese weekly meeting and reviewing industry materials. The discussion covered Ethereum’s Fusca upgrade, L2 developments, and recent security incidents. From a market perspective, 2026 is viewed as a “foundation year” for industry consolidation, with Real World Assets (RWA), prediction markets, and compliance emerging as key trends—signaling a shift from early speculation toward integration with traditional finance.

Currently pursuing a master’s degree in Singapore, I have been involved in building RWA-based business applications in a real industry setting. This year, I contributed to the founding of a Non-Fungible Digital Asset program supported by Alpha Leddar Group, where I helped design the architecture and implement key components. While RWA is still an emerging field with few proven successes, I see it as a promising area with significant future potential—one where I can apply my technical skills, including Solidity, to deliver tangible contributions.

I also gained valuable insights into the Web3 job market. Competition remains intense, with an estimated applicant-to-job ratio of 450:1, and strong demand in regions like North America, the Middle East, and Southeast Asia. Employers generally value global exposure, adaptability, and Web2 experience over specific blockchain backgrounds. Salaries range widely, from entry-level community roles to senior engineering positions exceeding ¥100,000 per month.

Building an on-chain reputation and mastering emerging skills are critical. Beyond Solidity, Rust is increasingly essential, and AI tool proficiency is often evaluated in interviews. Effective job searching now depends on Web3-native platforms, Twitter, and GitHub activity, rather than traditional job boards. Additionally, the ability to identify and avoid high-risk projects—those that are anonymous, unaudited, or inactive—is a necessary skill in this industry.

As a developer intern at a CEX handling fiat and crypto transactions, I have already navigated the Web3 job search process firsthand. I’ve learned that seizing opportunities requires proactive learning and willingness to explore, rather than hesitating in the face of uncertainty.

Overall, today’s learning helped clarify both the direction of Web3 and actionable steps for entering the field. My next steps include attending the Ethereum global AMA on January 14 and beginning to build my on-chain presence through open-source contributions and content creation—laying a practical foundation for my future in Web3.
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
