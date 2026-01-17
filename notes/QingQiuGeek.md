---
timezone: UTC+8
---

# QingQiuGeek

**GitHub ID:** QingQiuGeek

**Telegram:** 

## Self-introduction

河南科技大学软件工程专业，大四，已有两段Java后端实习，计划转web3。对web3、ai感兴趣，掌握JavaWeb、docker、linux、数据库等常见后端开发技能以及react、js、ts、next.js基础前端知识，有独立开发全栈web项目的经验，对ai agent、mcp也有一定了解。

## Notes

<!-- Content_START -->
# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->
\## Node.js 是什么？  
  
Node.js = V8 引擎 + 事件驱动 I/O + 模块系统  
  
\- V8 引擎：Google Chrome 的 JavaScript 引擎（用 C++ 编写），可将 JS 通过 JIT 及时编译为机器码高速执行，同时拥有自动垃圾回收 GC  
\- 事件循环（Event Loop）：非阻塞 I/O 模型，适合高并发场景（如 Web API、实时聊天）  
  
!\[\](image.png)  
  
说到这里不得不提到 JS，JS 在设计之初是浏览器脚本语言，只能在浏览器中运行（因为浏览器有 JS 运行所需的 V8 引擎），NodeJS（内嵌 V8 引擎） 的出现让 JS 脱离了浏览器，让 JS 有了\*\*后端运行时\*\*，成为通用编程语言，也就是可以作为后端开发语言！  
  
\## 什么是 V8 引擎  
  
V8 是 Google 开发的开源 JavaScript 和 WebAssembly 引擎，用 C++ 编写，负责将 JS 代码编译为机器码并执行  
  
\## NodeJS 和 JS 区别  
  
!\[\](image-1.png)  
  
\## NodeJS 后端框架  
  
!\[\](image-2.png)  
  
\## JS 模块系统  
  
JavaScript 有两种主流模块系统：  
  
\- CommonJS（Node.js 传统标准）  
语法：require() / module.exports  
特点：  
同步加载（适合服务器）  
运行时动态加载  
  
\`\`\`js  
// app.js (导入)  
const { add } = require('./math');  
console.log(add(2, 3)); // 5  
  
// math.js (导出)  
const add = (a, b) => a + b;  
module.exports = { add };  
\`\`\`  
  
\- ES Modules（ECMAScript 标准，现代方案）  
语法：import / export  
特点：  
静态分析（编译时确定依赖）  
支持 tree-shaking（打包优化）  
浏览器原生支持  
  
\`\`\`js  
// app.js (导入)  
import multiply, { add } from './math.js';  
console.log(add(2, 3)); // 5  
console.log(multiply(2, 3)); // 6  
  
// math.js (导出)  
export const add = (a, b) => a + b;  
export default function multiply(a, b) {  
return a \* b;  
}  
\`\`\`  
  
⚠️ 混合使用陷阱：  
  
CommonJS 不能直接 import ESM，需动态 import()  
  
ESM 不能直接 require CommonJS，需 createRequire()  
  
🔑 Node.js 中如何支持两者？  
  
\- .js 文件：默认 CommonJS  
\- .mjs 文件 或 package.json 中 "type": "module" → 启用 ESM  
  
\## NodeJS 历史包袱  
  
\### CommonJS  
  
Node.js 诞生时，ES Modules（import/export）尚未标准化，于是采用社区方案 CommonJS（require/module.exports），导致后续 ESM 完善后在同一项目中 CommonJS 与 ESM 混用发生冲突。  
  
\### 回调地狱  
  
早期 Node.js 依赖回调函数处理异步 I/O，嵌套过深导致代码难以维护,后来引入 Promise 和 async/await（Koa 框架率先采用），但旧代码仍大量存在。  
  
\`\`\`js  
fs.readFile('file1.txt', 'utf8', (err, data1) => {  
if (err) throw err;  
fs.readFile('file2.txt', 'utf8', (err, data2) => {  
if (err) throw err;  
fs.writeFile('output.txt', data1 + data2, (err) => {  
if (err) throw err;  
console.log('Done!');  
});  
});  
});  
\`\`\`  
  
\### 安全模型弱（无沙箱）  
  
Node.js 默认无权限控制，任何 JS 代码可读写文件、访问网络。  
  
\`\`\`js  
// 恶意 npm 包可能恶意操作系统重要文件  
require('fs').writeFileSync('/etc/passwd', 'hacked');  
require('child\_process').exec('rm -rf /');  
\`\`\`  
  
\### npm 依赖地狱  
  
node\_modules 嵌套过深，依赖包重  
  
\## NodeJS 项目结构  
  
\`\`\`  
my-node-app/  
├── src/ # 源代码目录（核心！）  
│ ├── controllers/ # 路由处理器（业务逻辑）  
│ ├── routes/ # API 路由定义  
│ ├── models/ # 数据模型（如 Mongoose Schema）  
│ ├── middleware/ # 中间件（认证、日志、错误处理）  
│ ├── services/ # 业务服务层（解耦逻辑）  
│ ├── utils/ # 工具函数（日期格式化、加密等）  
│ └── app.js # Express/Koa 应用实例  
│  
├── config/ # 配置文件（数据库、环境变量）  
│ └── db.js # 数据库连接配置  
│  
├── tests/ # 测试文件（Jest, Mocha）  
│ ├── unit/ # 单元测试  
│ └── integration/ # 集成测试  
│  
├── public/ # 静态资源（HTML/CSS/图片，可选）  
├── uploads/ # 用户上传文件（如头像）  
│  
├── .env # 环境变量（API\_KEY, DB\_URL）  
├── .gitignore # 忽略 node\_modules 等  
├── package.json # 项目依赖 + 脚本命令  
├── package-lock.json # 依赖锁定文件  
└── server.js # 服务器入口文件（启动 HTTP 服务）  
\`\`\`  
  
\## NodeJS 替代品  
  
!\[\](image-3.png)  
  
\*\*Bun 同时兼容 NestJS、Koa、Express、Fastify 框架，而 Deno 不兼容\*\*
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->

# 以太坊**分叉**

我们知道智能合约特点就是不可篡改+自动执行，那么部署过的合约，如果真的有漏洞，如何弥补呢？有几个方法：

1.  硬分叉：分叉会改变**整个区块链的状态**，不是只改一个合约。区块链分叉后，之后的交易都打包区块在新的链上，此方法需要区块链的验证者配合执行。硬分叉是**极端措施**，仅用于**重大安全事件**（如 The DAO 黑客事件），**不会为单个合约漏洞发起分叉**（成本太高、破坏去中心化原则），分叉后通常形成**两条链**（如 ETH / ETC），**原链是否“有效”取决于社区选择。**
    

如以太坊每次升级都是硬分叉，[https://eips.ethereum.orq/EIPS/eip-7607](https://eips.ethereum.orq/EIPS/eip-7607)

以太坊EIP：社区讨论->形成文档->升级（每年1-2次）

2.  部署合约时采用代理模式，也就是用户和代理进行交互，代理再和区块链交互，当要升级合约时，只需要让代理指向新的合约即可。
    
3.  紧急暂停，在合约中预设 `pause()` 函数，发现漏洞时立即暂停所有功能，争取时间部署修复方案（如通过代理升级）。
    
4.  资金迁移，部署新合约，提供 `migrate()` 函数让用户主动转移资产。
    
5.  链下补偿，项目方用自有资金赔偿用户损失，**不修改链上状态**，但维护声誉（如 Wormhole 跨链桥被盗后团队垫付 $3.25 亿）。
    

# Web3社会学

技术层面：用私钥签名证明你是谁，用共识网络保证账本可信，用智能合约让规则自动执行。

技术之外：权力的重新分配、数字资产与权利自我控制、无许可、抗审查、公开可信、去中间商。

每个区块可以有多个哈希交易，验证者一定是节点，节点不一定都是验证者，只有质押了区块链原生货币的节点才是验证者，这就是POS。

# 合规性

## ICO（首次代币发行）

法律定性：若面向不特定公众公开募资 → 涉嫌 非法吸收公众存款罪 / 集资诈骗罪，中国大陆已基本禁止公开ICO。

✅ 纯技术发币（无融资）→ 一般不违法

❌ 借发币名义公开募资 → 高风险

## 永续合约（Perpetual Contract）

公安常定性为“赌博软件”。

**法律误区：**

赌博 = “买定离手 + 结果随机”。但实际上合约买定后可以不离手，主动性在自己，不在赌场，性质不一样。

永续合约 = “用户全程可控 + 价格由市场决定” → 属金融衍生品

**风险点：**

1.  技术人员、运营、客服均可能被列为共犯
    
2.  “技术中立”辩护在司法实践中常被驳回
    

## 挖矿业务

政策环境：中国大陆禁止境内挖矿，但允许算力出口。

**合规路径：**

1.  通过“超算中心”形式包装
    
2.  结合外贸模式，将算力服务售予海外客户
    
3.  避免直接持有或交易虚拟资产
    

## 出入金与U卡

出入金就是法币和虚拟货币转换

**U卡（数字货币支付卡）风险：**

非正规发卡方可能绑定虚假“员工”身份，导致账户被冻结，资金易被黑灰产利用，遭遇“资金攻击”。

**合规建议：**

1.  选择持牌/正规U卡服务商
    
2.  主动报税、保留交易记录
    
3.  遭遇异常及时报案，形成“风险隔离”
    

## KOL与推广行为

典型案例：为**海外**永续合约交易所推广 → 被控 “非法利用信息网络罪”

**风险逻辑：**

1.  即使未直接参与运营，推广行为本身可能构成“帮助犯”
    
2.  返佣模式加剧责任认定
    

**合规要点：**

1.  避免推广涉赌、无牌金融产品
    
2.  建立内容审核机制
    
3.  优先选择已与地方监管建立沟通的合规平台合作
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->


# **Web3 行业全局介绍 & 岗位概览**

## 发展规模

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768484798780-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485033688-image.png)

## 求职岗位

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485051000-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485156432-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485182238-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485292440-image.png)

## 求职面试

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485424613-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-15-1768485505401-image.png)

# Web3运行原理

助记词和私钥一对多，私钥可以自己生成。

交易就是要做的事+gas费+防重放序号nonce，而钱包则要组装交易内容并私钥签名，然后广播到区块链网络。

## 交易流程

-   当发起一笔交易后，这笔交易首先被发送给区块链其中一个节点A，在进入节点A前，先验证交易的格式、签名、nonce 等基本合法性，通过**验证**后，这笔交易会被**暂存**到节点A的内存池中，这个池子用来暂存已验证合法但尚未被打包的交易的池子。
    
-   然后节点A立刻广播到邻居节点，然后才在自己的池中按照 Gas 费用高低**排序**，矿工/验证者会优先选择价格更高的交易进行打包，打包成新区块添加到自己的区块链中（每个节点都有自己的区块链，一个个区块首尾相连形成的链）。
    
-   邻居节点收到交易后也会先进行合法性校验，通过后再转发给它们的邻居，不断重复上一步骤，如此病毒式传播，直到该区块链大部分节点都记录这笔交易，此时就可以认为这笔交易在链上达成了共识，被认可。
    

但并非所有节点都会接收或保留这笔交易，有些节点可能因 Gas 费过低而拒绝加入自己的内存池、可能因内存池已满而丢弃低优先级交易。

POW：工作量证明，一种验证交易的方式，验证者/矿工需要投入大量算力来解决复杂数学题，验证成功后才能获得奖励（挖矿）。这种方式能耗很高。

POS：权益证明，一种更环保的验证方式，验证者/矿工需要先质押一定数量的代币，才有资格去验证交易。验证成功后会获得奖励，相比 PoW 更节能。

智能合约：虚拟机中设定好的代码程序，不可篡改且自动执行
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->




今天参与了以太坊中文周会和web3行业全局介绍&岗位概览分享

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-12-1768223118971-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/QingQiuGeek/images/2026-01-12-1768223106935-image.png)

## 发展史

-   最初由 HTTP 的发明者提出，是指一个集成的通信框架，互联网数据可以**跨越各个应用和系统**实现机器可读。Web 3.0 通常也被称作为“语义网”
    
-   2014 年，以太坊创始人重新定义了 Web3 概念，指一种区块链技术，可以\*\*基于“无须信任的交互系统”\*\*在“各方之间实现创新的交互模式”。
    

### Web1.0

-   时间：1994-2004
    
-   特点：Web 1.0 主要是静态的 HTML 网页，用户之间很少交互，有聊天转账功能但是无法保证安全。
    

实际在 1968 年，一个名为“ARPANET”的美国政府项目就已经启动了 Web 1.0。ARPANET 最初是由军方承包商和大学教授组成的一个用于交换数据小型网络。Web 1.0 时代中最具创新力的企业就是**必胜客**。他们在 1995 年开发了一个订购披萨的网页，消费者可以在页面中下单，等到披萨送到后再付现金。

### Web2.0

-   时间：2004 - 至今
    
-   特点：动态网页，用户之间互动性显著提升，用户使用互联网时必须**授权中心化的第三方平台管理大量数据**。因此这些中心化的实体在数据和内容权限方面被赋予了巨大的权力和影响力。
    

### Web3.0

-   时间：2008 - 至今
    
-   特点：**基于数学加密证明，而非信任第三方数据中心平台**，即去中心化。
    

2008 年中本聪提出了点对点的数字货币（比特币）和区块链概念。

智能合约出现后，去中心化互联网模式才逐渐流行，虚拟货币实现了点对点交易，智能合约不是传统的法律约束，而是可编程协议，通过编程实现约定的逻辑，好比商品贩卖机，规定输入硬币，选择商品后，规定输出商品。

## Web3 核心要素

-   区块链
    

安全性和去中心化水平都极高的网络，可以在一个**共享账本**中储存数据、交换价值并记录交易活动，而且账本**不受任何中心化**实体控制。区块链是 Web3 的基石

-   加密资产
    

基于区块链构建的数字货币或代币，类似现实的货币

-   智能合约、去中心化应用
    

智能合约是区块链上不可篡改的程序，利用“如果 x 是真实的，则执行 y”的代码逻辑自动执行交易。通过智能合约可以构建去中心化应用 dAPP，给用户使用。

-   预言机
    

智能合约要充分实现其潜力，就必须能够与区块链网络以外的数据和系统交互。预言机能**将区块链连接至真实世界中的数据和系统**，并提供关键的基础架构，打造一个具有互操作性且统一的 Web3 生态。

-   代币 Token
    

代币不是货币，和 BTC 这种加密货币不一样，\*\*代币构建在已有区块链之上的数字资产\*\*，比如用人民币买了游戏币、藏品，那么人民币就可理解为已有区块链之上的加密货币，游戏币、藏品就是代币，不能当钱花（除非有人愿意买）  
非同质化代币（Non-Fungible Token 简写 NFT ），像比特币这种原生的加密货币，每一单位都一样，可以互换（1 BTC = 1 BTC），是同质化，“非同质化”则是每个 NFT 都独一无二，不能互相替代，比如某个网站域名者签名球衣，可能全世界仅此一份。

-   Gas 费
    

因为区块链每笔交易都在链上进行加密等计算最终上块，肯定要消耗计算机资源，那么收点过路费也不为过吧

## 举例

假设某季节中的降雨量必须超过 20 英寸，才能保障农作物不欠收。一个农民希望买保险来对冲恶劣天气造成的不利影响。正常情况下他必须要经过漫长的理赔流程，并且要依靠**中心化的保险机构**来核实降雨量。但是如果在 Web 的世界，一切都是事先编程约定好的合约，保险流程会非常高效迅速，并基于简单的二元逻辑判断农民是否应获得理赔。物联网传感器在当季记录的降雨量是否低于 20 英寸？如果是，则立即支付赔偿金给农民。如果否，则不支付赔偿金给农民。
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
