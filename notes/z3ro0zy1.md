---
timezone: UTC+8
---

# RonnaldL

**GitHub ID:** z3ro0zy1

**Telegram:** @ronaldl3e

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-29
<!-- DAILY_CHECKIN_2026-01-29_START -->
# 今天重新回顾了一下Cos的黑暗森林手册

* * *

### 核心安全哲学：两大基本法

在进行任何链上交互或资产管理前，必须刻入脑内的两条原则：

1.  **零信任：** 始终保持怀疑，不轻信任何链接、私聊、甚至是看似官方的公告。
    
2.  **持续验证：** 信任必须建立在可验证的基础上。不仅要验证信息来源，更要验证交互背后的逻辑（如签名内容）。
    

* * *

### 第一部分：钱包与环境安全

1\. 钱包创建与管理

**隔离原则：** 永远不要把鸡蛋放在一个篮子里。建议至少区分：**冷钱包**（大额长期存放）、**温钱包**（常用 DeFi/NFT 交互）和**热钱包**（临时尝试新项目、小额资金）。

**下载安全：** 仅通过官网下载。防止下载被中间人劫持或篡改的恶意安装包。

**助记词/私钥：**物理备份（纸笔、密文板）是首选。

2\. 操作环境隔离

**系统安全：** 保持操作系统及时更新。不要在存放资产的设备上安装盗版软件或不明来源的脚本。

**浏览器安全：** Web3 交互建议使用独立干净的浏览器（如 Brave 或 Chrome 独立 Profile），减少插件干扰，防止恶意插件读取私钥或篡改前端页面。

* * *

### 第二部分：交互深水区

1\. 签名安全

**所见即所签：** 在确认钱包弹窗前，必须核对签名信息。

**授权钓鱼：** 注意授权限额，尽量避免无限授权。

**_警惕 离线签名钓鱼，这类攻击不需要 Gas 费，黑客通过骗取你的离线签名，即可在后续直接转走你的代币。_**

**_反常识签名： 如果一个简单的领取空投操作要求你签署_** `SetApprovalForAll` **_或类似的复杂权限，这就是钓鱼。_**

2\. DeFi 交互逻辑

**合约审计：** 优先选择经过多家知名审计机构审计的项目。学会看审计报告中的权限过大风险（如 Admin 是否能随时提取池子资金）。

**前端安全：** 警惕前端劫持。即使合约安全，如果前端被黑，转账地址也可能被篡改。交互时需二次确认收款地址。

这一点我很同意，因为如果只比对前5位和后5位是不行的。 很多钓鱼地址是中间不一样，但是首尾相同。

* * *

### 第三部分：高级威胁与人性安全

1\. 常见攻击变体

**_首尾号钓鱼： 黑客生成与你常用地址首尾字符高度相似的地址，通过零转账等方式出现在你的交易历史中，诱导你在下次转账时误复制。_**

**社群钓鱼：**关闭私聊功能是 Web3 玩家的基本功。

2\. 开发者视野下的安全

对于正在构建 Web3 工具（如监控系统）的开发者，需额外注意：

**第三方依赖包安全：** 定期进行 `npm audit`，防止依赖项被植入后门。

* * *

### 第四部分：应急预案

如果发现资产正在被非法转移，请按以下顺序行动：

**止损第一：**立即转移剩余资产。如果涉及授权泄露，第一时间使用工具**取消授权**。

**保护现场：**记录中毒/被盗时的所有操作、截图、恶意网站 URL、设备状态，不要急于重装系统。

**链上追踪：**追踪资金流向。如果资金进入中心化交易所（CEX），立即联系交易所风控部门配合冻结。

**报案溯源：**整理证据链，尝试通过安全团队或司法途径寻求帮助。

* * *

## Kimi 2.5的Agent使用

用agent尝试做了一个ppt，感觉对于创建初版内容还是很有用的。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-29-1769693440779-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-29-1769693358763-image.png)

先继续准备一下考试 😭
<!-- DAILY_CHECKIN_2026-01-29_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->

* * *

### 正好今天有Vibe Coding 的Zoom会议，借此机会配置一下Gemini Cli和Antigravity的Skills

一开始位置安装错了

本应该直接 git clone xxx .gemini/ \\ #就行了

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-26-1769426796812-image.png)

在GUI中安装：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-26-1769428335481-image.png)

antigravity测试

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-26-1769430997568-image.png)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-26-1769431308920-image.png)

gemini测试

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-26-1769431138882-image.png)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-26-1769431371931-image.png)
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->


## 学习使用yt-dlp下载视频，以及压缩与分离音频

最新想下载一些会员视频到本地于是

（这个代码是我说明诉求，然后AI帮写的，效果很好）

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-25-1769348867080-image.png)

其中这个--js-runtimes node 很关键，由于视频网站本身有加密，需要通过js来解密。

压缩方便上传

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-25-1769348836905-image.png)

* * *

## 关于Agent Skills

这个最近也总在时间线上出现，因此特别去看了一下。

传统的 AI 只是在进行概率预测，而 Skills 为它提供了**外部工具**。AI 不需要内置所有功能。通过 Skill，我可以让 AI 在需要时调用 Python 脚本、访问数据库或操作本地系统。Skill 运行在本地环境中如 `~/.antigravity/skills` ，这意味着敏感数据不需要上传到云端，AI 只是下达指令。

### 结构：定义与逻辑

一个成熟的 Agent Skill 通常包含两个部分：

**描述信息：**告诉 AI 这个技能是干什么的、需要输入什么参数（比如这是一个“压缩视频”的技能，参数是文件路径）。

**执行代码**：AI 决定调用后，后台实际运行的代码逻辑。

后续考完试再深入了解。

* * *

## 关于最近爆火的语音输入法：Typeless与质谱的AutoGLM。

This learning note is completed by me using voice input.

（此句子为我语音输入的）

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-25-1769347492746-image.png)

其他的慢慢研究吧

* * *

还剩2周参加IELTS考试。😭
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->



# 关于Vibe coding的入门与理解

最近用Antigravity做了一个用于入门学习React的前端例子：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-23-1769179779370-image.png)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-23-1769180001107-image.png)

此时我在想关于当下学习一门新语言，只要学习到能看出错误错在哪里似乎就够用了：

**语法降权：** 具体的 API 调用，这些负担交给了Antigravity。

**逻辑加权：** 我的核心能力变成了Debug。你需要理解语言的底层哲学，比如 Python 的异步模型，不是为了写出它，而是为了在 AI 生成的代码逻辑跑偏时，一眼看出这里的状态管理有问题。

但是似乎能看出哪有错误或者不合适也许更难？

* * *

回到Vibe coding出的这个代码中，由于我看完了官方文档的入门教程后，希望通过例子来进一步巩固。

# React学习

## 2.1范型

泛型让代码具有通用性。它不知道你会存什么，但通过 <T>，它可以灵活适配任何类型。

## 2.2 自定义Hook

  
React 的 Hook 是逻辑复用的利器。当你发现自己多处都在操作 localStorage 时，就应该把它抽离出来。  
代码实例

```

export function useLocalStorage<T>(key: string, initialValue: T) {
  // 逻辑：初始化 -> 读取磁盘 -> 监听变化 -> 存回磁盘
  // 这种复杂的逻辑被封装后，在 App.tsx 中只需要一行代码：
  // const [tasks, setTasks] = useLocalStorage<Task[]>('tasks-list', []);
}
```

## 2.3 全局状态管理

  
当状态需要在很多毫不相关的组件间共享时，Context 提供了一个广播。  

````

// 创建 Context
const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

// 核心：Provider 包装。App 及其所有后代都能直接“收听”这个信号
export const ThemeProvider = ({ children }) => { ... };

// 消费：子组件通过 useTheme() 勾住这个信号
const { theme, toggleTheme } = useTheme();
```
````

## 依旧在准备IELTS 今日看的比较少 😭
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->




# Uniswap 入门学习

* * *

## Uniswap V2：恒定乘积公式的数学之美

V2是最纯粹的**AMM自动做市商**模型。

它的核心逻辑是：不管你怎么交易，池子里两种代币数量的**乘积**必须保持不变。

### 举例：ETH/USDC 交易池

假设一个池子里现在有：

**10 ETH** (x)

**20,000 USDC** (y)

**恒定乘积 k**\= $10 X $20,000 = $200,000

**场景：你想从池子里买走 1 个 ETH**

1.  **池子剩下的 ETH**：$10 - 1 = 9$ 个。
    
2.  **为了保持 $k$ 不变**：池子里必须有的 USDC 数量 = $200,000 \\div 9 \\approx 22,222.22$ USDC。
    
3.  **你需要支付的价格**：$22,222.22 - 20,000 = 2,222.22$ USDC。
    

**滑点**：显然，虽然当前市场价是 2000 USDC/ETH，但你实际支付了 2222 USDC。这是因为你的交易改变了池子比例。交易量越大，滑点越高。

**无人值守**：整个过程只有x乘y=k，在后面计算。

* * *

V2的问题

由于X✖️Y = K，这是个单侧双曲线，显然其理论上导致X的范围是（0，∞），这意味着，这堆代币构成的曲线在数学上是**延展到无穷远**的。无论 ETH 的价格涨到1亿USD，还是跌到 0.00001USD，V2 的公式都能给出一个对应的X和Y组合。

**本质上是说：你的这部分资金，正在为极低概率的价格点提供理论上的成交可能。**

* * *

## Uniswap V3：集中流动性的效率革命

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-21-1768996238553-image.png)

-   **Xreal和 Yread**：是钱包里真实存入池子的代币数量。
    
-   **Xoffset和Yoffset**：是**偏移量**。它们是由区间边界（Pa,Pb）和流动性L决定的常数
    

### 设定参数：

**当前价格P**：$2500 USDC/ETH , P^1/2 == 50

**价格下限Pa**：$2304 USDC/ETH Pa^1/2 ==48

**价格上限Pb**：$2704 USDC/ETH Pb^1/2 = 52

**设定的流动性L**：1000

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-21-1768996631054-image.png)

由此：

**Xoffset** \== 19.23

**Yoffset ==**48000

Xreal == 0.77ETH

Yreal == 2000USDC

通过这个例子发现一些事情：

**资产耗尽点的控制**：当价格上涨到Pb=52^2 时，此时Xreal == 0。**这意味着：** 恰好在价格到达上限时， ETH 被换光了。偏移量 Xoffset的存在，确保了你不需要无穷多的钱就能撑到价格上限。

**资本效率的来源**：

在 V2 中，要达到同样的交易深度（同样的 L=1000），你需要存入真实的20ETH+ 对应数量的USDC。

但在 V3 的这个区间里，你只需要存入 **0.77 ETH+对应的USDC**。**效率提升：**用 1/26 的资金，就实现了和 V2 一样的抗滑点能力。

* * *

除此之外试了一下用Antigravity 来 vide coding 一个React的学习例子

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-21-1768999045525-image.png)

现在还自带调试功能了，当时用cursor的时候还没有。。 加油熟悉吧～

继续备考英语
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->





# React继续学习

## 找出 UI 精简且完整的 State 表示

**State**是组件的记忆。为了让应用尽可能高效，要遵循 **Don't Repeat Yourself**原则：只保存最核心的变化数据，**能算出来的统统不要存。**

但是在学习react的时候，需要理解数据的流向。父组件的状态更新可以向下传递，但是子组件的变化这么让上层知道？

## 反向数据流

这目前，数据已经可以从顶层通过 Props 流向底层的输入框。但问题是：**用户在底层里打字，顶层的 State 怎么更新？**

### 核心实现：

父组件不仅把 `state` 传给子组件，还要把**修改 state 的函数**也传下去。

子组件（SearchBar）监听浏览器的原生事件（如 `onChange`），在事件触发时，调用父组件传下来的那个函数。

```
// 父组件 FilterableProductTable
function FilterableProductTable() {
  const [filterText, setFilterText] = useState('');

  return (
    <SearchBar 
      filterText={filterText} 
      onFilterTextChange={setFilterText} // 1. 把“改数据的权力”传下去
    />
  );
}

// 子组件 SearchBar
function SearchBar({ filterText, onFilterTextChange }) {
  return (
    <input 
      value={filterText} 
      onChange={(e) => onFilterTextChange(e.target.value)} // 2. 用户一打字，就通知父组件
    />
  );
}
```

今天先写一点，忙于备考IELTS 😭
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->






## **_首先继续昨天的阅读，然后搞一下React前端，因为我发现后续有DAPP开发。_**

* * *

## **The life you want lies within a specific level of mind**

![Image](https://pbs.twimg.com/media/G-ebHTubsAEsAee?format=jpg&name=large)

This is a integram theory. The reality is determined by my level of consciousness.

The Ladder of Consciousness: The author lists nine stages of self-development from the "Impulsive Stage" to the "Unitive Stage". Each higher stage can accommodate more complexity, less prejudice and greater understanding.

Cognitive limitations: If a person are at the "conformist" stage, it is very difficult for him to understand how a strategist thinks, because the filters through which you observe the world are completely different.

The essence of change: True change is not about running faster on the old steps, but leaping to the next one. Most readers are between stage 4 (self-awareness) and stage 8 (constructive awareness), a period that marks the crucial transition from being shaped by society to shaping oneself.

1.  Impulsive: Black and white thinking, also called 二极管思维, I think.
    
2.  Self-protective: Look out exclusively for themselves.
    
3.  Conformist: The rules or norms in your family or social circle feel like reality itself. It is difficult for a person staying this stage to understand why there is someone believing or vote differently.
    
4.  Self-aware: The transition begins when you have an inner life that does not always match the exterior world. You might don't believe what everyone else does. For example, in some underdeveloped regions, people hold a traditional viewpoint that men must marry a woman, and correspondingly, a woman must be married at specific time. If you begin contemplating that such thing seems not necessary.
    
5.  **Conscientious: This is where you build your own system of principles and hold yourself accountable for them. Examples include leaving a family religion after careful study to adopt a personal philosophy or building a career plan based on the belief that right efforts yield right results.**
    
6.  Individualist: Honestly, I begin realizing this that my principles were shaped by my experience and living context. So it is necessary to hold them more loosely.
    

From my perspective, reaching Individualist is a key node for most people. As for later stages, which depend on individuals' personal wisdom. So, I won't write about those things.

**V – Intelligence is the ability to get what you want out of life**

![Image](https://pbs.twimg.com/media/G-eb5tkbEAA8v0u?format=jpg&name=medium)

We can judge intelligence based on the system’s ability to iterate and persist with trial and error.

In conclusion, intelligence is the ability to learn from mistakes.

* * *

# React 继续学习

今日完成了官方手册中入门棋盘联系。

* * *

## 一、 架构思维：状态提升 (Lifting State Up)

当多个组件需要共享数据（例如：棋盘需要显示数据，历史记录列表也需要知道数据）时，我们将状态移至它们的**公共父组件**中。

**Game (大脑)**：拥有唯一真实的数据源（`historyRecords` 和 `activeStep`）。

**Board (执行者)**：不存数据，只通过 Props 接收“当前长什么样”并负责显示。

**Square (末梢)**：纯展示组件，只负责把传进来的值画出来。

* * *

## 二、 核心原则：不可变性 (Immutability)

在 React 中，我们**永远不直接修改**旧的数组或对象。

**做法**：使用 `.slice()` 复制数组，或者使用 `[...]` 展开运算符创建新数组。

**意义**：

**性能优化**：React 只需要对比引用地址（旧数组 vs 新数组）就能知道是否需要重绘。

**时光旅行**：因为每一波数据都是独立保存的“快照”，我们可以轻松跳回任何一步。

**简化逻辑**：避免了复杂的撤销/重放逻辑。

* * *

## 三、 数据流：双向奔赴

React 是单向数据流，但交互是双向的：

-   **向下传数据 (Props)**：父组件把变量传给子组件（如 `visualValue={...}`）。
    
-   **向上传信号 (Callbacks)**：子组件通过执行父组件传下来的函数，把信息反馈回去。
    
    -   _关键技巧_：**匿名函数包装**。为了给函数传参数而不让它立即执行，必须写成 `() => doSomething(i)`。
        

* * *

## 四、 智能状态：派生状态 (Derived State)

**原则**：如果一个值可以通过已有的 State 计算出来，就不要为它开辟新的 `useState`。

**错误示例**：额外设置一个 `const [isXNext, setIsXNext] = useState(true)`。

**正确示例**：`const isNextX = activeStep % 2 === 0;`。

**好处**：保证了数据的一致性。当 `activeStep` 改变时，`isNextX` 永远是同步准确的。

* * *

## 五、 TypeScript 的“合同”约束

TS 最大的作用是安全

1.  **Interface (接口)**：规定组件必须接收哪些 Prop。`visualValue: string | null`：明确告诉开发者，这里可能没值。
    
2.  **函数类型签名**：`() => void`：不带参数。`(data: Type) => void`：带参数的回调。
    
3.  **泛型 State**：`useState<Type[]>(...)` 确保你不会不小心往数组里塞进奇怪的东西。
    

* * *

## 六、 列表渲染与 Key

使用 `.map()` 将数据数组转换为 UI 元素。

-   **Key 的作用**：React 用 Key 来标识哪些元素改变、添加或删除了。
    
-   **最佳实践**：在井字棋历史记录这种“顺序固定”的场景下，使用数组索引 `index` 作为 `key` 是安全的。
    

* * *

## 七、 时光旅行逻辑拆解

这一行代码：

`const nextHistory = [...historyRecords.slice(0, activeStep + 1), latestLayout];`

1.  `slice(0, activeStep + 1)`：截取从开始到当前步的历史。这解决了“回到过去并重新下棋”时，旧的未来会被自动“剪掉”的问题。
    
2.  `[...]`：将新的一步棋无损地贴在旧历史后面。
    

继续备考中 🙈
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->







# **_最近一篇文章火了，今天看一下，就当作雅思阅读了。_**

# **_原文我会用Code来引用，无格式的文本是我的个人感悟_**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-18-1768725253845-image.png)

The author argues that New Year’s resolutions are ineffective, a sentiment I find hard to dispute. Reflecting on my WeChat circle this past January 1st, I saw countless posts herald 2026 as 'their year,' yet my friends have repeated itself since I added them. Ultimately, the true purpose of goal-setting isn't the resolution itself, but the honest admission of one's shortcomings and the recognition of the distance between one's current self and their ideal identity.

* * *

## I – You aren’t where you want to be because you aren’t the person who would be there

```
When it comes to setting big goals, people tend to focus on one of the two requirements for success:
```

```
Changing your actions to make progress toward the goal (least important, second order)
```

```
Changing who you are so that your behavior naturally follows (most important, first order)
```

This hits the nail on the head regarding why people quit their disciplined lifestyles so quickly. We're essentially trying to build a great life on a **rotting foundation**.

```
charismatic dude who can chat up a group without a shred of anxiety entering his mind.
```

Such a person is the one I aspire to become. **Charismatic.**

Regarding the first point, my understanding is this: To achieve a specific outcome, you must first become the type of person who naturally produces that result. Just as someone who constantly oversleeps cannot become a CEO, or someone who is lazy and overeats cannot maintain a great physique, the purpose of the first section is to remind us to focus on the 'path' rather than just the high-level goal.

In other words: Who are you? Do your habits and mental models support what you are trying to achieve? Because behavior is ultimately the result of your mindset, most people fail their goals because they try to change their actions while their 'foundation' remains the same. For someone whose brain is wired for the instant gratification of TikTok, attempting to learn something complex is extremely difficult because their current identity isn't built for that depth. Or on the other words, immersing into douyin, which determines a person can not continue studying for a long time, such as 2 hours.

And, there is also a hint that I should pay more attention on the habits I have disgusting. Real changes happens when I really realize that some habits are destructing my future. So, it is not about want new life, but about rejecting the old.

## **II – You aren’t where you want to be because you don’t _want_ to be there**

People have some unconscious goals that prevent people from doing something. For example, procrastinating support the Ego protection. Honestly, when I started prepare IELTS exam, I also procrastinated the exam date, because I fear failure, especially after longtime study, which means i am not capable. However, I had fixed the date and deal with it as just a test or even a game.

Goal is lens, allowing me to notice relative information. If my goal is protecting my poor ego, then I just fear the judgement from others, which eventually causes procrastinating.

## **III – You aren’t where you want to be because you’re afraid to be there**

**The part conveys a concept: inclusive.**

Low-level identification: "I belong to the XX faction. Anyone who opposes XX is wrong." (Closed)

Advanced self-identity: "I am a person who is constantly evolving. Anything that can help me grow, I will absorb."

你看现在网络上因为意识形态干不停的情况，就是因为彼此之间在挑战。结局是一地鸡毛，没意义。

### 你对应的综合素质不支持你达成目标

### 你没有达成目标是因为你根本不想达成

### 你没有达成目标是因为你害怕达成 （防御机制）

先写到这，一会再补充！
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->








最近Prediction Market大火，借此机会也频繁尝试了钱包的交互功能。在使用像 Trust Wallet 或 MetaMask 这样的 Web3 钱包时，我经常会遇到**_确认Confirm_**和**_签名Signature_**这两个操作。虽然它们看起来都是点一下同意，但在底层的技术逻辑和安全影响上有本质区别。

* * *

**确认 (Confirm)**：通常指授权钱包发起一笔**链上交易**。这会消耗燃料费，并且会改变区块链的状态（比如转账、兑换代币）。

**签名 (Signature)**：通常指你使用私钥对一段**信息**进行加密证明。它可以是**离线**的，用来证明我是这个钱包的主人，常用于登录网站或授权某些协议。

| 特性 | 确认 (Confirm) | 签名 (Signature) |
| 是否上链 | 是 | 通常不直接上链（离线或作为交易一部分） |
| Gas 费用 | 需要消耗 Gas | 通常不需要（免费） |
| 状态改变 | 改变账本 | 不改变账本（仅作为身份验证或授权） |
| 典型场景 | 转账、DEX 交易、质押 | 登录 DApp、设置个人资料、Permit 授权 |

在钱包里点击确认时，后台发生的物理过程就是：

1.  包含谁发给谁、发多少、Gas是多少。
    
2.  用私钥对这一串数据进行数学运算。
    
3.  将原始数据和生成的签名组合在一起，形成一个**已签名的交易对象**。
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-17-1768652333296-image.png)

既然私钥在web3中几乎确定一切，所以我想冷钱包为什么现在越来越被人重视？

冷钱包是**私钥永不联网**。那么它如何完成链上交互？

1.  **构造（联网端）：** 用手机或电脑（观察钱包）构造一个转账请求（未签名的交易）。
    
2.  **传输（未签名）：** 通过 USB、蓝牙或 **二维码**，将这个未签名的交易传给冷钱包硬件。
    
3.  **签名（硬件端）：** 硬件内部通过私钥对交易进行签名。**私钥在这个过程中始终闭环在芯片内。**
    
4.  **传回（已签名）：** 硬件将生成的“签名”传回给手机/电脑。
    
5.  **广播（联网端）：** 手机/电脑将这个带有签名的包裹发送到区块链网络。
    

> **总结：** 冷钱包存在的唯一目的，就是为了在不接触互联网的情况下，给那个“交易包裹”盖上你的数字私钥印章。

* * *

Some thoughts about the Prediction Markets, including Opinion and Polymarket.

Honestly, although there are many top lawyers are defending their binary options nature, which may be a complicated economical definition, I don't understand that. However, it is cannot be doubted that those so-called prediction markets definitely are illegal in many regions. Be that as it may, nobody would ignore its impact, and someone believe that is the trend of future where gamble would become common and acceptable.

**_I'd not like to talk about the Binary outcomes(1USD=1Yes share + 1 No share) and the prices as possibility._**

**_I care about Settlement principles and its Core Philosophy!_**

## 1.Settlement Principle

Blockchain cannot inherently perceive the situations of real world, such as who won the president election? Polymarket utilizes the UMA Optimistic Oracle as the resolution.

1.1 Proposal: Once an event concluded, anyone can propose the outcome by providing a security deposit.

1.2 Challenge period: If no one disputes the proposed result with a certain timeframe, the result is confirmed as TRUTH and the market ends.

1.3 Dispute resolution: If a challenge is created, the decision will be processed further by UMA token holders for a vote. Voters rely on the Schelling Point--the idea that in absence of communication, people tend to the most obvious factual answer. This is because voting with the majority or the common recognition is incentivized with rewards( the oppose voters security deposit), which means voting for an incorrect answer results in a financial penalty.

For example:

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-17-1768655975951-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-17-1768655995355-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-17-1768656029787-image.png)

## 2\. Core Philosophy

It is more that just a betting platform, which underpinned a profound framework.

## 2.1 Skin in the GAME

unlike traditional polls or expert punditry. Prediction market requires participants to back their claims through real capitals like money. This financial commitment forces objectivity, because if a participant allow bias to affect their judgment, they face immediate economic loss.

## 2.2 Information Aggregation

The market acts as a powerful magnet, attracting individuals globally who possess insider information, proprietary data, or superior analytical models. These fragmented pieces of information are synthesized through trading into a single, accurate, and real-time price tag.

Insider Example:

![Image](https://pbs.twimg.com/media/G9uGLKmawAAo8jj?format=jpg&name=large)

备考ielts中 😑
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->









# 图灵完备

图灵完备是计算机科学中最核心的概念之一。

通俗点讲：**只要给它足够的时间和内存，图灵完备的系统可以解决任何可以被算法化的问题。**

* * *

**以太坊 EVM**：

支持循环，是图灵完备的。_代价_：它面临著名的**停机问题**。

* * *

## 停机问题

图灵证明了一个数学定理：**不存在一个_通用的程序_，能够判断任意一个程序是否会在_有限时间内运行结束_，还是会陷入_死循环_。**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-16-1768568945517-image.png)

由于停机问题在逻辑上是不可解的，以太坊这种图灵完备的系统面临一个风险：如果有人在智能合约里写了一个死循环，全网的矿工/验证者都会因为运行这个合约而卡死。

> **_以太坊的解决方案：Gas 机制_**
> 
> **_以太坊并没有从算法上解决停机问题（因为不可能），而是从经济学上解决了它。每一步计算都要花钱（Gas），如果你写了死循环，你的 Gas 很快就会扣光，程序会被强制停止。_**

# 以太坊深入：L1与L2

## 1\. 以太坊区块的核心构成

一个完整的以太坊区块可以划分为三个逻辑层：

### A. Block Header (区块头)

-   **Parent Hash**: 父区块的哈希值，构成了“链”的物理结构。
    

* * *

**State Root（状态根）**: 最关键的字段。它是一个 **Merkle Patricia Trie** 的根哈希，代表了所有交易执行完毕后，整个以太坊网络，包括所有账户、余额、合约状态的全局状态快照。

**Transactions Root**: 当前区块中所有交易构成的默克尔树根。

**Receipts Root**: 交易收据（如产生的 Log、消耗的 Gas）构成的树根。

**_上面这三棵树相比起Bitcoin中只有交易树不一样！_**

**State Trie (状态树)**：

**Key**: 账户地址的哈希。

**Value**: 包含 `nonce`, `balance`, `storageRoot`, `codeHash` 的编码。

**意义**：这使得以太坊成为“全状态”机器，节点只需根据这个根就能验证任何账户的状态，而不需要回溯所有历史。

**Transactions Trie (交易树)**：

记录本区块内交易的顺序和内容。

**Receipts Trie (收据树)**：

记录交易执行的结果（成功/失败）和生成的事件日志（Logs）。

* * *

-   **Base Fee Per Gas**: EIP-1559 引入的基础费用，随网络拥堵情况动态调整。
    
-   **Withdrawals Root**: 记录了从共识层提取质押奖励到执行层的操作。
    
-   **Blob Gas 相关字段**: 记录当前区块处理了多少 Blob 数据。
    

### B. Execution Payload (执行负载)

这是区块的“躯干”，包含了实际要执行的数据：

-   **Transactions**: 本区块打包的所有交易列表。
    
-   **Withdrawals**: 验证者提现记录列表。
    

### C. Blobs (Binary Large Objects) —— 扩展层

这是 Dencun 升级（EIP-4844）后的核心产物。为了给 Layer 2 降费，区块现在可以挂载额外的临时数据空间（Blobs）。

* * *

## 默克尔树入门：

### 为什么用默克尔树验证很容易？

假设一个区块里有 8 笔交易。我想证明交易T3确实在这个区块里。

只需要给验证者提供以下 **3 个** 哈希值:

1.  **H4**
    
2.  **H12**
    
3.  **H58**
    

作为结果我可以自行计算H34 -> H1234 -> H12345678(This is the Merkle root)

**Then compare the calculated Merkle root with original root.**

* * *

这对于区块链的和L2 扩容至关重要。

| 交易总数 (n) | 线性扫描 (发送整个区块) | 默克尔证明 (发送 Proof) |
| 16 | 16 条交易数据 | 4 个哈希值 |
| 1,024 | 1,024 条交易数据 | 10 个哈希值 |
| 1,048,576 | 约 100 万条数据 | 仅 20 个哈希值 |

即使区块里有 100 万笔交易，证明其中一笔交易存在的成本也极低。

依旧备考IELTS 😭 昨天还漏了一天打卡
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->










# **CEX入门研究**

```
最近Hyperliquid，lighter，aster都挺热门的，所以今天研究一下。
```

| 特性 | DEX (去中心化) | CEX (中心化) |
| 资金存放 | 用户钱包（安全归个人） | 平台钱包 |
| KYC 要求 | 通常不需要（保护隐私） | 强制要求（符合监管） |
| 交易成本 | 极低 Gas + 少量手续费 | 固定手续费 + 出入金损耗 |
| 资产范围 | 极广（涵盖所有新币/RWA） | 较窄（需审核上市） |
| 技术门槛 | 较高（需管理助记词） | 较低（邮箱注册即可） |

_我觉得真的吸引人的有以下两大块_

### **性能飞跃：**

极高吞吐量：新一代 DEX，如 Hyperliquid，采用原生 L1 或并行执行技术，可支持每秒数万甚至十万次以上的交易处理。

亚秒级结算：交易确认延迟缩短至毫秒级，操作手感已非常接近币安等中心化巨头。

零/低 Gas 费：随着 Hyperliquid 这种交易模型和 Solana 等低成本链的普及，链上交易的摩擦成本已大幅低于传统模式。

### **产品创新：**

**极致杠杆与资产多样性：** 某些永续合约 DEX 提供高达 1,001 倍的杠杆，并支持美股、大宗商品等现实世界资产的代币化交易。

如图中的黄金perps：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-14-1768400442921-image.png)

# Meme入门了解：

**_感觉是纯赌场，看谁手段多信息快，然后把别人割了。_**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Web3_Internship_Bootcamp_2026_Winter/main/assets/z3ro0zy1/images/2026-01-14-1768400515366-image.png)

暂时不喜欢这个内容，感觉就是纯互相割韭菜。

不过大多数散户都被KOL喊单整治了

# React入门学习

## 函数的传递：引用 vs 调用

| 语法 | 行为类型 | 后果 |
| onSquareClick={handleClick(i)} | 立即调用 | 渲染时直接触发，导致组件更新，进而引发无限循环。 |
| onSquareClick={() => handleClick(i)} | 匿名函数包装 | 创建一个“包装盒”，只有在真实点击发生时，才执行内部代码。 |

* * *

## React 核心编程思想

### 声明式 vs 命令式

**命令式（面向过程）**：你告诉浏览器“去找到那个按钮，把它变红”。

**声明式（React）**：你告诉 React “如果被选中了，它应该是红色的”。你只管维护 **状态 (State)**，UI 会自动根据状态“重画”。

### 不变性

在修改 `squares` 数组时，我们使用 `slice()` 复制一份新数组，而不是直接修改原数组：

**原因**：React 通过比对新旧数组的内存地址来判断是否需要更新。如果地址没变，React 可能认为数据没变，从而不刷新界面。**性能**：这种比对（引用比对）极快，比遍历整个数组检查内容要高效得多。

* * *

## 底层运作原理

### DOM (文档对象模型)

**什么是 DOM**：浏览器将 HTML 转化成的一棵可编程的对象树。

**虚拟 DOM**：React 在内存中运行的一套轻量级假 DOM。它会找出真正需要改变的部分，最后一次性更新到真实 DOM 上。

### 为什么 TS/JS 运行很快？

**编译 vs 解释**：TS 在运行前会编译成 JS，并删除所有类型信息，运行负担小。

**V8 引擎**：现代浏览器使用 JIT 编译技术，将频繁运行的 JS 代码直接转化成底层机器码，速度接近 C++。

依旧备考语言中。。 😭
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->











# 以太坊零知识证明学习与回顾

```
本科学的密码学基础此时稍微派上了点用场，起码看得懂RSA和模运算等东西了（虽然忘了很多东西）
```

## 1\. PoW

Miner实际上在寻找一个Nouce，使得：

SHA256(SHA256(Block Header + Nonce)) < Target，其成功概率为 P= Target/2^256

这个纯靠大力出奇迹

## 2.PoS

在 PoS 中，你被选为记账人的概率P正比于质押的代币S， 同时为了防止富有者通过频繁出块获得更多奖励，导致系统迅速中心化，通常会引入币龄**，**随机信标来增加随机性。

## 3.零知识证明 (ZKP)：代数几何的巅峰

```
可惜，由于本身数学水平局限于非数学专业本科生水平，短期无法看懂其包含抽象代数与代数几何等抽象高深内容。
```

**_因此我暂且通过传统的简单色盲猜球来理解。_**

# React入门回顾Day1

```
（大学学过Vue，现在忘了，现在学一下react前端，为全栈打个基础）
```

## 一、 TSX 与组件本质

**组件即函数**：在 TSX 中，组件是一个返回 HTML 标记（JSX）的 JavaScript 函数。

**关注点分离**：不再追求 HTML 和 JS 文件分离，而是按**功能单元**封装代码。

**命名规范**：组件函数名必须**首字母大写**（如 `MyButton`），以便 React 区分原生 HTML 标签。

**TS 类型加持**：通过 `interface` 定义 Props 的形状，确保在编写组件时获得精准的自动补全和纠错。

## 二、 Props：组件间的“参数传递”

-   **本质**：Props 就像函数的形参，是外部向组件内部传递数据的唯一通道。
    
-   **单向数据流**：数据总是从父组件通过 Props 流向子组件。
    
-   **只读性**：组件内部不能修改接收到的 Props，只能读取。
    
-   **解构赋值**：常用 `const { count } = props` 或直接在参数位 `{ count }: Props` 解构。
    

## 三、 useState：组件的“记忆体”

**语法**：`const [state, setState] = useState(initialValue);`解构机制：`useState` 返回一个数组，第一个是当前快照值，第二个是更新函数。

**异步快照更新**：调用 `setState` 不会立刻改变当前函数作用域内的变量。React 会等当前函数执行结束后，才进行“批处理”更新并触发重新渲染。

**金句**：_“当前函数里的状态变量永远代表过去，只有下一次渲染它才会变成未来。”_

## 四、 作用域与状态提升

**同步逻辑**：若多个组件需要共享数据并保持同步，需将状态定义在它们共同的**父级作用域**中（状态提升）。

**独立逻辑**：若组件需互不干扰，则将 `useState` 定义在组件内部。

## 五、 React 更新机制

**虚拟 DOM**：React 在内存中对比新旧树的差异，仅对真实的浏览器 DOM 进行精准修改。

**局部更新**：虽然逻辑上感觉像重新跑了整个 App，但物理上只更新了变化的那一小块 DOM，因此性能极高。

**组件拆分的必要性**：将状态下放到小组件，可以避免整个大页面跟着无谓的重新渲染。**代码解耦**：避免 `App.tsx` 逻辑过载，提高复用性。

## 六、 交互监听与受控组件

**onChange**：在 React 中是实时触发的（每打一个字触发一次），常用于捕获用户输入并同步到 `state`。

**受控组件模式**：表单的值由 `state` 驱动（`value={count}`），通过 `onChange` 更新 `state`，形成闭环控制。

同样，最近忙于准备IELTS考试，阅读英语令人头疼🤕，第一周笔记暂且短一点。
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->












# **实习手册入门导读**

## 1\. Bitcoin入门

### **1.1 Block Header**

**版本号，Previous Blcok Header Hash， 默克尔根，时间戳，难度目标，随机数。、**

**其中，默克尔根是把许多交易浓缩为32字节，方便后续计算Hash。 而对于时间戳来说，从和Gemini的交流中发现，他被允许大于前11个区块的中位数，于是可能会出现Timestamp（N）< Timestamp（N-1）的情况。 这种倒挂是一种逻辑顺序优先的原则。**

**至于难度与随机数**

```
nonce = 0
header_fixed = version + prev_hash + merkle_root + timestamp + nbits  \\ 头信息
while hash(header_fixed + nonce) >= target: \\hash头信息
    nonce += 1
    if nonce > 0xFFFFFFFF:
        # Nonce 溢出了
        update_other_fields()
```

### 1.2 Block Body

Transactions + Signature，还有默克尔树。

比特币的思路和其他新兴公链有极大的类似，因此我认为深度研究BTC技术问题对学习ETH，Solana等有很大帮助。

### 1.3 关于BTC的一些其他问题以及与AI的探讨：

-   **Merkle Tree 到底是怎么把几千笔交易压缩成一个哈希值的？**
    

**最底层：** 对每笔交易(Transaction)进行哈希运算（SHA-256），得到一组哈希值：H1,H2,H3,H4。

**中间层：** 将相邻的两个哈希值拼在一起再哈希。比如：H12=SHA(H1+H2)

**最顶层：** 继续合并。H1234=SHA(H12+H34)。这个最终得到的唯一哈希值就是 **Merkle Root**。

-   **如果两个矿工同时挖出了不同的区块（分叉），网络如何决定听谁的？**
    

最长链原则，网络此时一分为二，矿工们会基于自己先收到的那个区块继续挖矿。但是这种竞争通常在下一个区块就会结束。如果基于区块 A 的矿工先挖出了下一个区块C，那么 A + C 这条链就比 B 链长了。 被抛弃的区块 B ，里面原本打包的交易如果没在 A 链中出现，会重新回到内存池等待下次打包。

## 2\. ETH入门

### 2.1 智能合约

上文提到BTC内是纯粹的交易信息，比如Alice给了Bob 100元，这是一种纯粹的金额加减。

而对于ETH的智能合约而言：

它是一段存储在以太坊区块链上的**代码和数据**。

它有自己的地址，但没有私钥，只有代码逻辑。

它平时是静止的。只有当外部账户向它发送一笔交易，或者另一个合约调用它时，它才会执行。

其核心特性：

**不可篡改性**：一旦部署到链上，任何人（包括开发者）都无法修改代码逻辑。

**确定性**：同样的输入，在任何时间、由任何节点运行，结果都必须完全一致。

**自执行性**：不需要中介。如果代码说“周五下午三点把钱转给 A”，那么时间一到，只要有触发条件，钱就会自动转走

### 2.2 以太坊虚拟机EVM，全球统一的引擎

EVM 是一个完全隔离的虚拟环境。合约代码在 EVM 内部运行，无法访问宿主机的文件系统或网络。

这种隔离保证了即使合约有漏洞或恶意代码，也不会拖垮节点电脑。

开发者通常使用 **Solidity** 语言编写合约。但 EVM 听不懂 Solidity，它只能看懂**字节码**。

今天先这样，忙于IELTS考试中 😭
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
