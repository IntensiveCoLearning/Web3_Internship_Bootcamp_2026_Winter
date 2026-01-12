---
timezone: UTC+8
---

# chenlillia

**GitHub ID:** chenlillia

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->
1月12日学习笔记：实习手册

# 一、为什么比特币不会通胀

1.     总量上限写进了代码，不存在“随意增发”的可能，通胀最根本的来源堵死，供给曲线**不可调、不可操纵**

2.     比特币类似于数字黄金

黄金：自然稀缺、开采成本递增

比特币：算法稀缺、挖矿难度递增

二者都具备稀缺性、抗通胀属性、价值储藏功能（Store of Value）

3.     此外，比特币存在**通缩倾向**：私钥丢失、长期不流通地址会导致实际可用供给 < 理论供给

# 二、什么是RDF

1.     RDF 是一种由 W3C 提出的语义数据模型，它用“主语–谓语–宾语”的三元组来描述资源及其关系。  
它的优势在于强调语义和可链接性，天然适合构建知识图谱，在语义网、数据集成以及区块链链上数据分析中都有很强的应用价值。

RDF 的核心结构：三元组（Triple）：<Subject> <Predicate> <Object>

| 元素 | 含义 |
| Subject | 被描述的资源 |
| Predicate | 资源的属性或关系 |
| Object | 属性值或另一个资源 |

# 三、什么是OWL

1.     OWL 是 W3C 制定的语义网本体语言，它建立在 RDF 之上，用于定义概念、关系和逻辑约束，并支持自动推理。  
RDF 负责描述事实，而 OWL 负责定义规则，使机器能够从已有数据中推导出新的知识。

**2.    OWL 的核心能力**

**1️⃣ 定义“类（Class）”**

例如：

Person

Student

Teacher

BlockchainAddress

**2️⃣ 定义“关系（Property）”**

对象属性（ObjectProperty）：连接两个资源

hasOwner

transfersTo

数据属性（DataProperty）：连接到具体值

hasBalance

hasAge

**3️⃣ 定义“约束和规则”**

这是 OWL 的灵魂。

常见约束包括：

**继承**（Student ⊆ Person）

**互斥**（Student ⊓ Teacher = ⊥）

**等价类**（Wallet ≡ Address ∧ hasBalance > 0）

**基数限制**（一个人只能有一个出生日期）

3.     OWL 基于**描述逻辑（Description Logic）**，属于：一阶逻辑的可判定子集，可以被自动推理

常见推理包括：分类推理（Class Inference），一致性检查（Consistency Check），实例归类（Instance Checking）

4.     在区块链场景中，OWL 非常有用：

可以用 OWL 定义地址、交易、合约等概念的语义关系，再通过推理自动识别关联账户、异常交易或潜在洗钱模式。

例如：

若多个地址共享同一控制者 → 推理为同一实体

若合约调用模式符合某规则 → 推理为风险合约

这在 **链上取证、反欺诈、合规分析**中非常重要。

# 四、本体工程是什么

1.     本体工程就是把现实世界的概念、关系和规则，形式化为机器可理解、可推理的知识结构。

2.     本体工程主要做四件事：

1️⃣ **概念建模**：  
世界里有哪些“东西”？（类 / 概念）

2️⃣ **关系建模**：  
这些东西如何相互关联？（属性 / 关系）

3️⃣ **约束与规则建模**：  
什么是必须的、互斥的、可推理的？（逻辑）

4️⃣ **演化与维护**：  
概念体系如何随时间更新而不崩？

它不是写代码那么简单，而是**在做“语义架构设计”**。

**3.    本体工程的标准流程**

**1️⃣ 领域分析（Domain Analysis）**

明确领域边界（比如：区块链）

确定核心问题（分析什么？推理什么？）

**2️⃣ 概念抽取（Conceptualization）**

提取核心概念

Address

Transaction

SmartContract

User / Entity

**3️⃣ 本体建模（Formalization）**

用 OWL / RDF Schema 表达：

类的层级关系

属性定义

约束规则

**4️⃣ 推理与验证**

使用 Reasoner（如 HermiT、Pellet）

检查：

是否自相矛盾

推理结果是否合理

**5️⃣ 迭代与维护**

现实世界变化

新概念加入

旧规则废弃

# 五、什么是SPARQL

1\. SPARQL 是“在语义图上查询事实与关系的语言”，相当于 RDF 世界里的 SQL。

2\. SPARQL 在解决什么问题？

传统查询（SQL）擅长：

·         表

·         行

·         列

但 RDF 世界里：

·         数据是 **三元组**

·         结构是 **图**

·         Schema 可能不统一

👉 SPARQL 的任务是：

**在一个可能异构、跨源、可链接的语义图中，查找符合语义约束的模式。**

3\. SPARQL 的基本结构

PREFIX ex: <[http://example.org/](http://example.org/)\>

SELECT ?var

WHERE {

  ?s ex:predicate ?o .

}

**四个核心部分**

| 部分 | 含义 |
| PREFIX | 命名空间 |
| SELECT / ASK / CONSTRUCT / DESCRIBE | 查询类型 |
| WHERE | 图模式 |
| 变量 | ?x、?y |

**4.    SPARQL 的 4 种查询类型**

**1️⃣ SELECT**

返回变量绑定结果（最常用）

**2️⃣ ASK**

返回 true / false  
👉 是否存在某种关系？

**3️⃣ CONSTRUCT**

返回新的 RDF 图  
👉 推理 / 数据重构常用

**4️⃣ DESCRIBE**

返回某资源的相关描述（探索性）

**5.    SPARQL 的“语义能力”来源**

**1️⃣ 基于 RDF/OWL 的语义**

能利用：类继承/等价类/属性特性

若启用推理器：查询能命中**隐含事实**

**2️⃣ 路径查询（Property Path）**

?x ex:sendsTo+ ?y

👉 查找任意长度的转账路径

这在：资金流分析/社交网络/区块链追踪非常重要。

6.     在区块链链上数据分析中，可以把地址、交易、合约表示为 RDF 知识图谱，再用 SPARQL 查询复杂的交易路径、地址关联关系或异常模式，这种方式比传统 SQL 更适合表达关系型问题。

例如：

多跳转账追踪

共同控制地址识别

合约交互网络分析

# 六、链接数据是什么

1\. **链接数据 = 语义化的数据 + 可互相指向的链接**

就像：

-   Web：网页 → 网页
    
-   Linked Data：**实体 → 实体**
    

2.     链接数据的 4 条基本原则（Tim Berners-Lee）

1️⃣ **用 URI 标识事物**  
不是用字符串，而是全局唯一的 URI。

2️⃣ **用 HTTP URI**  
保证别人可以访问和引用。

3️⃣ **访问 URI 时，返回有意义的信息（RDF）**  
机器可读，而不仅是给人看的页面。

4️⃣ **在数据中链接到其他 URI**  
让数据形成“网”。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
