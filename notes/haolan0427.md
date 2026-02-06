---
timezone: UTC+8
---

# haolan0427

**GitHub ID:** haolan0427

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-02-06
<!-- DAILY_CHECKIN_2026-02-06_START -->
-   gorm笔记 gorm博客 ✅
    
-   [project.md](http://project.md) ❌
    
-   algorithm ✅
    
-   调研下实习岗位 ❌
    
-   vibe coding ✅
    
-   openclaw
    
    -   homebrew
        
    -   Tailscale，无需公网的内网穿透；
        
    -   brid， X
        
    -   clawdhub， skills
        
    -   github
        
    -   summarize❌
<!-- DAILY_CHECKIN_2026-02-06_END -->

# 2026-02-04
<!-- DAILY_CHECKIN_2026-02-04_START -->

-   gorm笔记 gorm博客 1h
    
    -   让 AI 整理大纲，自己跟着源码再做一遍 ✅
        
    -   快速了解博客架构❌（忍不住从头开始看），补充知识点
        
-   [project.md](http://project.md) 1.5h
    
    -   按部就班 ✅
        

新建 `./task-management-system/internal/worker/pool.go` 实现工作池模式，用于处理后台异步任务

新建 `./task-management-system/internal/repository/user.go` 完整实现了用户表的 CRUD 等操作

新建 `./task-management-system/internal/repository/task.go` 完整实现了任务表的 CRUD 操作

新建 `./task-management-system/internal/service/user.go` 实现用户注册和登录的业务逻辑

新建 `./task-management-system/internal/service/task.go` 协调数据库和缓存，实现任务编排逻辑

上传到 github 上，提交信息为 `"实现工作池模式、用户和任务表等增删改查、用户注册和登录哦逻辑、任务分配"`。

-   algorithm 1个知识点 ✅
    
    -   速度要快，因为博客❌比较难做
        
-   vibe coding 1h ✅
    
-   web3 2h
    
    -   Solidity-by- examples
        
    -   快速刷任务
        
-   openclaw ✅
    

连接上 Discord，能使用Google Chrome
<!-- DAILY_CHECKIN_2026-02-04_END -->

# 2026-02-02
<!-- DAILY_CHECKIN_2026-02-02_START -->


在 openclaw 中启用 Brave Search，去[官网](https://brave.com/search/api/)注册账号并申请 API key，输入 `openclaw configure —-section web` ，⇒ Where will the Gateway run? ⇒ Local (this machine) ⇒ Enable web\_search (Brave Search) ⇒ yes ⇒ Brave Search API key (paste it here; leave blank to use BRAVE\_API\_KEY) ⇒ “粘贴你的 api” ⇒ Enable web\_fetch (keyless HTTP fetch) ⇒ yes

-   [project.md](http://project.md)
    

在 `./task-management-system/internal/model/` 路径下新建 `user.go` `task.go` ，声明 2 个结构体，对应着数据库中的 users 和 tasks 这 2 个表，

在 `./task-management-system/pkg/database/` 路径下新建 `mysql.go` ，实现数据库的连接以及表的迁移

上传到 github 上，提交信息为 `“实现数据库连接和表迁移函数”` 。
<!-- DAILY_CHECKIN_2026-02-02_END -->

# 2026-01-31
<!-- DAILY_CHECKIN_2026-01-31_START -->



GoLang

-   [project.md](http://project.md) ，1h ✅
    

初始化了本地仓库，并创建 `.gitignore` 文件；

搭建好了项目架构，并初始化 `Go Modules` ；

在 `./task-management-system/configs/config.yaml` 中设置 `server` 、 `database` 、 `redis` 、 `jwt` 、 `rate_limit` 等配置；

在根目录下创建 `.env` 和 `.env.example` ，分别用于存储私密信息（不上传到 github）和供其他开发者参考配置信息；

创建 Go 语言的配置管理模块， `./task-management-system/internal/config/config.go` ，用于从 `.env` 和 `config.yaml` 加载配置信息并替换环境变量。并安装好需要的包， `go get github.com/joho/godotenv github.com/spf13/viper` ；

上传到 github 上，提交信息为 `“完成配置信息设置”` 。

* * *

Gorm ，1h ✅

一对多关系完成；

正在学习多对多关系

* * *

Solidity by Example

-   完成 Basic ，1h ✅
<!-- DAILY_CHECKIN_2026-01-31_END -->

# 2026-01-30
<!-- DAILY_CHECKIN_2026-01-30_START -->




GoLang

-   实操 [project.md](http://project.md) ，1h
    

输入提示词让 AI 生成的基于 Go 的后端项目，

搞懂不清晰的概念：✅

JWT（JSON Web Token），常用于微服务、RESTful API，像是互联网世界的数字通行证。组成部分，Header (头部)，声明类型和所使用的签名算法；Payload (负载)：存放实际的数据；Signature (签名)：防止数据被篡改的核心。库 `golang-jwt/jwt` 。

Redis（Remote Dictionary Server），常用于处理高并发请求，后端加速器，基于内存的键值型（Key-Value）数据库。用途，缓存层，减轻关系型数据库（如 MySQL）的压力；分布式锁，`SETNX` 指令，在多实例部署的 Go 服务中保证逻辑的原子性；消息队列，`List` 或 `Pub` / `Sub` 甚至 `Stream` 机制实现轻量级消息解耦；计数器/排行榜，`INCR` 和 `ZSet` 实现点赞数或实时排名。库，`redis/go-redis` 。

Worker Pool（工作池），Goroutine 是 Go 并发的“原子”，那么工作池就是管理这些原子的“调度室”，是一种并发设计模式，维护固定数量的 Goroutine 集合来处理一系列任务。为什么需要工作池，虽然 Go 的 Goroutine 很轻量，但在极端高并发下，会造成内存耗尽、调度开销（过多的上下文切换）、资源过载。则成，Jobs Channel（一般带缓冲），存放待处理任务的队列；Workers，一组运行中的 Goroutine，监听 Jobs 管道；Results Channel 用于接收 Workers 处理完后的返回结果。避免重复造轮子，`ants` 开源库。

| 并发设计模式对比 | 核心区别 | 适用场景 |
| --- | --- | --- |
| Worker Pool | 固定数量的协程，任务排队 | 保护下游资源（如数据库连接）、控制 CPU 占用 |
| Goroutine per Request | 按需创建，一个请求/任务一个协程 | 绝大多数普通的 I/O 密集型 Web 服务 |
| Semaphore (信号量) | 不限制协程总数，但限制同时运行的任务数 | 简单的限流，不需要复用协程，只需要限流 |
| Thread Pool (线程池) | Java/C++ 的概念，管理昂贵的操作系统线程。 | 解决线程创建成本高的问题（Go 原生已解决此问题） |

Middleware（中间件），是一种代码逻辑，在 Request(请求) 到达业务逻辑之前执行，或者Response（响应）返回给客户端之后执行。用途，身份认证；日志记录；错误处理；跨域处理；限流。在 Go 中，本质是一个接收 `http.Handler` 并返回 `http.Handler` 的函数。库，`net/http` 。

Viper，用途，帮你“读取和管理程序配置”的一站式工具。在结构体加上 `mapstructure` 标签，而不是普通 `json` 标签或 `gorm` 标签。在 Go 中可以对结构体字段写多个标签。在项目中，将从文件中读配置和与数据库对接这两个功能分开，分为两个结构体存储。

Zap，专门为高性能设计的日志组件，替代 Go 标准库 `log` 。

Validator，在处理用户提交的表单、API 请求的 JSON 数据时，先进行校验。库，`go-playground/validator`。通过“扫描”结构体 `validate` 标签来运行，将结构体实例作为参数传入 `Validate.Struct()` 。

Testify，代替Go 标准库 `test` 。

* * *

Gorm

-   B 站课程，1h ✅
    

完成“一对一”关系的学习，

正在学习“一对多”关系的学习

* * *

Web3 实习计划（冬季）

-   Solidity by Example
    
    -   完成 Basic ，1h ✅
        
    -   开始 Applications ，1h ❌
        

* * *

Vibe Coding ，1h ✅
<!-- DAILY_CHECKIN_2026-01-30_END -->

# 2026-01-29
<!-- DAILY_CHECKIN_2026-01-29_START -->





GoLang

-   [整理笔记](https://www.notion.so/Go-2eb730847f49808e9581ce95f0432eb6?pvs=21)，1h ✅
    
-   理清楚项目头绪，跟着 [project.md](http://project.md) 做 ✅
    

* * *

Gorm

-   B 站课程，1h ✅ （效率很低，需要在其他时间补上一些进度 ✅）
    

* * *

**Web3 实习计划（冬季）**

-   Solidity by Example ，1h ✅ （加一个任务， 看 Applications ❌）
<!-- DAILY_CHECKIN_2026-01-29_END -->

# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->






### Gorm

### Solidity by example
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->







### Solidity by Example

### Gorm
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->








### web2 to web3 week2 的课程补完 ✅

### 在 Go 中使用传统 SQL 语句 ✅

### 使用 scaffold-ETH 学习 solidity by example ✅
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->











### 学习并总结当前我能接触到的智能合约安全漏洞

[笔记链接](https://joyous-stamp-b17.notion.site/2f2730847f4980fbae5bdaa84faea19f)

### 了解基SQL语句

[笔记链接](https://joyous-stamp-b17.notion.site/GORM-2f0730847f4980eba05df6f4d07c7926)
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->













### 开始学习GORM

确定好了学习路线，[笔记](https://joyous-stamp-b17.notion.site/GORM-2f0730847f4980eba05df6f4d07c7926)

### 补充GoLang对网络、文件目录以及错误的基础知识

[笔记](https://joyous-stamp-b17.notion.site/GoLang-2eb730847f49808e9581ce95f0432eb6)

### 完成SpeedRunEthereum第二节任务

### 追web2-to-web3的week2的课

[笔记](https://joyous-stamp-b17.notion.site/web2-to-web3-2ed730847f4980d98b3fc8c8f671f54a)
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->














### Web2 to Web3 Week2 Day2 Reading and Writing
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->















### 把Golang基本语法过了

明天把笔记喂给AI，让它帮我根据笔记情况为我准备一个后端的项目，目的是为了巩固所学的知识，然后整理笔记，最好是能用一个例子涵盖一个章节下的知识点。

### 粗略过了一下“入门技术”的任务

### 还在补web2-to-web3的课程
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->
















### 继续补web2-to-web3第一周的课，已经补上，笔记如下链接：

[week1主要是教如何在Ethereum上交互的](https://joyous-stamp-b17.notion.site/web2-to-web3-2ed730847f4980d98b3fc8c8f671f54a?pvs=74)

### 学习Remix

[笔记，要尽快掌握hardhat，为之后的学习能带上一件趁手的工具](https://joyous-stamp-b17.notion.site/Remix-2ee730847f49804e8d7ee6510565da03?pvs=74)

### 学习Solidity

[笔记，主要是学习了一下数据值存储的位置，以及Gas优化，只停留在理论上，还没有进行代码实战](https://www.notion.so/Solidity-2ee730847f4980628669fa731b74b33f)
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->

















### 学习GoLang基础语法，预计明天可以开始Vibe Coding一个基于GoLang语言的项目来检验最近的学习情况。

[记录在Notion上的关于GoLang基础语法的笔记](https://joyous-stamp-b17.notion.site/GoLang-2eb730847f49808e9581ce95f0432eb6)

### Web2-to-Web3课程，进度有些延后，不过我相信能马上把进度赶上！！！

[记录在Notion上的关于Web3智能合约开发的笔记](https://joyous-stamp-b17.notion.site/web2-to-web3-2ed730847f4980d98b3fc8c8f671f54a)

### 现在正在学习Vibe Coding的思维、工具使用和小型项目试手，后面会开始复杂项目实战

[记录在Notion上的关于Vibe Coding的笔记](https://joyous-stamp-b17.notion.site/Vibe-Coding-2ec730847f4980ab95e5da6d0222c5f2)

### 温习《021学习以太坊》，并将其中所有web3的概念整理

[记录在Notion上的关于Ethereum以及Web3的核心概念](https://joyous-stamp-b17.notion.site/Ethereum-Concepts-2eb730847f49806f9455e729846d464c)

之后的笔记提交形式，是写清楚今天相较于前一天做出了哪些改变。
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->


















# 总结过去的一周

在web3实习计划的第一周已经结束了，遇到了很多实力强劲、深耕多年和见解不凡的老师，也有很多勤勉、有趣、积极和热心的同好。这一周学习了很多知识，也改变了许多之前的见解，同时对加密行业的认识更进一步。从基本的工具使用，如社群类的X、tg，会议类的Zoom，笔记类的Notion和Figma，日程规划类的Calendar，以及钱包插件MetaMask等；到对以太坊系统的详尽认识，[《从021学习以太坊》](https://github.com/XiaoHai67890/021Ethereum)这本书是在是太适合投喂小白了，娓娓道来，让人百读不倦；再到对个人在加密行业中的规划，有了更加清晰的认识，未来几年里最主要的任务是在基于GoLang的backend engineer职务上深耕。感谢这次活动，刚过1/4就让我收获匪浅，增长了见识，有vibe coding非常厉害的实例，有刚运营个媒体账户就很成果的大佬，也有来自各行各业，五湖四海的人，我个人非常有幸与参加这次活动。既然明确了未来的发展路线，那还有什么可以好说的呢？哦不～，最后送一句话给大家

`"I've lost my mind doing this game, like Vincent van Gogh. He dedicated his life to his art and lost his mind in the process. That has happened to me. But fuck it.`

`When that gold belt is around my waist, and when my mother has a big mansion, and when my girlfriend has a different car for every day of the week, and when my kids' kids have everything they ever want, then it will pay.`

`Then I'm happy I lost my mind."`
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->



















重新刷《021学习以太坊》

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/haolan0427/images/2026-01-17-1768663964030-image.png)

学习Hardhat，正在看官方文档[https://hardhat.org/docs/getting-started](https://hardhat.org/docs/getting-started)
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->




















《021学习以太坊》第5章，了解EVM以及ta在以太坊生态中的意义，也深刻理解了为什么以太坊虚拟机能被称为真正意义上的世界计算机了，也学习了一笔交易的Gas是如何组成的，以及在编写智能合约时，如何让Gas是合理的。

《021学习以太坊》第6章，这章主要讲述了以太坊是如何以及为什么从最初的PoW共识转移到PoS共识机制、以太坊目前PoS共识机制的核心组成和机理、以太坊系统的历史以及一些重要的升级。
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->





















《021学习以太坊》第三章，了解了以太坊账户，真正能主动发起交易的EOA账户，和只能被动调用的合约账户，也学习了无论是EOA账户还是合约账户都以一个四元组的形式存储在world state中，还学习了不同账户之间的交互，以及ERC20和ERC721协议，也知道了，一旦合约上链，后期的更改是做不到的，如何有合约的升级的要求，可以采用代理合约模式。

《021学习以太坊》第四章，谈到了以太坊的核心之一智能合约，首先谈到了相比于传统合约，智能合约code is law解决了很多人为纠纷和减去了第三可信方的参与成本，又介绍了在以太坊生态上编写智能合约的最佳语言——Solidity的特性以及它的主要组件，也学习了现在主流的开发智能合约的工具链，也学习了编写、编译、测试网部署、测试、审计到最后上链的整个流程，学习部署合约的成本估算方法，也明白了需要有很好的业务边界能力和提前应对异常情况的准备，以及谨慎严谨和、丰富的经验和科学的开发流程，再加上后期的审计才可能把出现严重漏洞的情况降到最低，还学习了合约的升级方法。

《AI及其基础概念》分享会，分享人给我们展示了一个生动的AI与web3结合的例子，也是ta们正在做的事情，是把大语言模型收费的标准token使用代币量化，集成了很多AI模型，通过封装API，在平台让用户可以使用不同的AI模型，对于提供AI模型的服务商，又提出新的利益分配机制并基于X402协议，实得便于支付、体验丰富以及公开可见。
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->























学习了《021学习以太坊》的第一和第二章节，在第一章节中主要了解了以太坊是什么，有什么特点，发展历史，以及基于ta的各种应用；在第二章中，了解了以太坊生态中如执行客户端、共识客户端、用户、应用是如何有机地组成一个整体的，如何进行消息传递、状态同步、不断打包出块。

参加了安全会议，在会议上，分享人从2025年发生的重大资金窃失的案例说起，从这些案例中可以知道，即使是那些很有影响力组织或机构都会遇到黑客的攻击，损失一笔巨大的资产，让我意识得到在加密行业中，谨慎和风险意识是相当重要的，同时，作为一个未来想在加密行业从事后端工程师职位者，也应该在设计时考量到各方面的漏洞和bug，也要在正式使用前在测试网上不断的测试，还要进行深度的代码审核，同时一些补救冻结措施也当提前准备好，再有在上链运行后，实施实时监管也是很有必要的动作。

又参加了合规会议，我想不如换一个叙事方式，web3是将信息赋予价值的互联网，而不是为了所谓的去中心化，毕竟就这一点，就很难让区块链技术实现服务人民大众，而是服务一些政府、金融、医疗、物流机构或组织。

我对自己的区块链后端工程师的规划已经快设计到可以真正实施的阶段了，把目标改成了——达到实习的要求，因为我在很多招聘网站上面找到的应聘所需要的技术栈都很变态。还是要把心态调整好，不要贪快、贪速成，还是要稳扎稳打、长期积累，要有远大理想，但也要接受现实，脚踏实地，一步一步来。
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
























2026-1-13：

目标：确定在本次学习中，具体专研学习的方向。

任务拆解：

-   了解行业有哪些赛道；
    
-   了解行业有哪些岗位；
    
-   如何完成地开发一个DApps；
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->

























规划一下今日任务安排，在**赛道**中只有**运营&技术通用任务**栏有任务，

1.  查看开营仪式回放，主要再看一次哪位助教比较适合自己（！！！还未上传，查漏补缺2025-1-12-00:30）；
    
2.  创建钱包，并尝试测试币转账；
    
3.  在My First NFT mint 第一个NFT；
    
    1.  DYOR，Do You Own Research，自己做研究来决定你下一步的行为。如购买行为。你愿意随便给一个人送钱吗？不的话，请DYOR。
        
    2.  书写逻辑【理论】，了解加密货币前，需要了解其底层技术，分析了区块链的组成和结构，从而得出ta的特性——不可篡改、公开透明、匿名（比特币钱包生产一对随机数作为公私钥对，在网络上进行交易，只会显示和这个钱包有关的交易，是公开透明的，但是别人无法知道是你拥有这个钱包）以及能快速交易，提到了分布式网络，需要共识机制来达成对区块链记录的一致性，这使得区块链上的记录不被篡改以及按照合规的方式继续记录，也带来了去中心化这一新特性。众所周知，去中心化的分布式系统参与的节点越多，那么系统将会更加安全，那如何吸大家来参与呢？给奖励，给代币，如比特币BTC，ta具有货币的属性——稀缺性、流动性，但由于不稳定，很难成为正真的法币，容易收到热点影响而剧烈波动，背后没有监管和限制。当然加密货币也有很多缺点，如缺乏监管和限制，使得不法分子有机可乘，每个区块可打包的交易数有限，所以难以实现真正大规模的应用。再谈区块链核心组成，一、区块链运行在去中心化网络上，该网络由很多节点来维持，这个过程叫做挖矿，bitcoin is secure by mining；二、网络得以持续运行下去的缘由——代币奖励。区块链项目运行逻辑，创建一个项目/服务，吸引来投资和用户，用户使用服务产生的交易费吸引来“矿工”，矿工进一步挖矿维护项目/服务的稳定和安全。发起一笔交易的流程，用户发起交易、交易广播、节点验证、打包成块、链接上链、奖励发放。公链（公共公园-去中心化），成为节点：无需申请、自由进出。公共管理数据模式，所有人可见、去中心化决策，缺点是效率低。联盟链（多公司联合的董事会-多中心化），成为节点的方法，需要邀请或申请、权限分级（如决策者和观察者）。共同管理数据的模式，半公开数、联合决策，优点是比公链效率略高、隐私也较好，但不如私链灵活。私链（私人俱乐部-中心化），成为节点的方法，严格审核、固定成员。共同管理数据的模式，数据完全私有、boss说了算，效率极高、隐私性强，但不透明。web2（当前互联网）范式，就像租房子，你可以装饰（发内容)，但房东（平台）随时能收回钥匙（封号）-中心服务器-生产内容不拥有内容-广告、平台抽佣商业模式。web3.0（语义网）范式，就像把图书馆的每本书都贴上了详细标签（作者、主题、关联书籍），让图书管理员能快速找到相关资料-描述数据关系的标准-按照标准组织出来的信息便于机器理解-知识图谱构建关系网络。web3（去中心化互联网）范式，像自己买地盖房（数据自托管），用智能合约管理水电费（自动结算）-用户拥有数据主权-无需信任机制-真正的数字资产-金融服务无门槛-应用可自由组合-内容永不消失。去中心化优势，信任最小化、抗审查和高弹性、用户自主管理、开发创新生态；挑战，可拓展性、安全与治理难题、用户体验与成本、法律与合规风险。以太坊，智能合约，基于智能合约和以太坊协议的应用，以太币ETH，gas fees（！！！查漏补缺26-1-12-01:38）。非同质化代币FT，tokens之间能任意等额交换。同质化代币NFT，tokens之间难以平等交换，特性，独一无二、不可分割、不可篡改、公开透明、万物皆可NFT、完全控制权、确权成本低、无限想象空间。应用，游戏 、版权确认、买卖房产、艺术品收藏、交换物品、通信证。购买时需要DYOR，为什么买入？情感共鸣、认同背后的团队社区文化、价值投资、FOMO情绪，可从以下几个方面了解，作品展示效果、项目团队以及背景、项目的规划和落实是否现实、周围人的看法，评估某NFT的价值可从纪念、艺术、工具、社区价值来衡量，得出你为此愿意付出多少钱。
        
    3.  实操，以太坊钱包，MetaMask，在Chrome安装MetaMask插件，创建钱包，记牢助记词，不外透露，界面操作（切换区块链网络、获取钱包私钥），重要组成（钱包地址-密码学-由私钥计算出公钥再由公钥得来、)。以太坊12个单词的助记词可以生成多个钱包的公钥和私钥对，每一对对应一个钱包，助记词丢失，全部钱包丢失，单个钱包私钥泄密，单个钱包丢失。助记词由BIP39标准生成。不要将助记词/私钥/Mnemoni/secret recovery phrase/BIP32 root key透露出去。web3安全知识，钱包助记词、私钥泄露或丢失；钓鱼，冒充官方网站、协议、NFTs，黑官方账号并推文引流，给你项目白皮书和官网链接；浏览器插件攻击。应对，分散资产，规划好钱包用途，冷、热钱包，周期性的创建新钱包进行资产转移（将资产放入硬件钱包，每次需要对外交互的时候，都创新创建一个新钱包，用完即废弃）；官方网站（信息、公告、项目），不在第三方网站下载软件，看到消息多思考，官方账号也可能被黑。从交易所向web3钱包转入ETH，链接钱包，
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
