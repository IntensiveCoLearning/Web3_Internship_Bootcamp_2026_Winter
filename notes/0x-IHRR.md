---
timezone: UTC+8
---

# .小白

**GitHub ID:** 0x-IHRR

**Telegram:** @Ox_IHRR

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
# 2026-01-28
<!-- DAILY_CHECKIN_2026-01-28_START -->
两个事件：

event Transfer(address indexed from, address indexed to, uint256 value);

event Approval(address indexed owner, address indexed spender, uint256 value);

它们不会直接影响状态，但：

-   区块浏览器都是靠监听 `Transfer` 事件来显示“这笔代币转账”
    
-   钱包可以通过扫描 `Transfer` 事件，知道哪些地址持有某个代币
    
-   分析工具/风控系统也通过事件追踪资金流向
    

概念直觉：ERC‑20 = 代币账本 + 授权机制 + 标准接口

基本接口：6 函数 + 2 事件，知道各自干嘛

一个最小实现合约，可以直接在 Remix/Hardhat/Foundry 里跑
<!-- DAILY_CHECKIN_2026-01-28_END -->

# 2026-01-27
<!-- DAILY_CHECKIN_2026-01-27_START -->

-   ERC = Ethereum Request for Comment，可以理解成“以太坊上的技术规范/提案编号”
    
-   20 = 这是第 20 号提案，所以叫 ERC‑20
    

它定义的是一类“**可替代代币**”的标准，比如 USDT、UNI、MKR、LP 代币等等。  
“可替代”意思是：每个单位是一样的，1 个 A 代币和另 1 个 A 代币没有区别，就像 1 元人民币之间没有区别。

ERC‑20 做的事只有一件：  
让“任何人发的代币”，都按照**同一套接口**来写，这样：

-   钱包可以通用显示余额、转账
    
-   Dex、借贷协议可以通用集成
    
-   你写一个 DeFi 合约时，不需要关心“这是哪个项目的币”，只要它是 ERC‑20，就能按统一接口交互
    

-   有一个总账本：`address -> balance`  
    也就是 Solidity 里的：
    
-   还有一个“授权表”：谁允许谁帮自己花多少币：
    

然后这个系统对外暴露一堆“按钮”（函数），别人只能通过这些按钮操作账本，不能直接改数据。  
ERC‑20 标准其实就是规定：这些“按钮”必须叫什么名字、参数是什么、返回值是什么。

**1）代币“基本信息类”** 

这些一般不涉及安全问题，但钱包 UI 会用到：

-   `name()`：代币名字，比如 `"CAT Token"`、`"USD Coin"`
    
-   `symbol()`：代币符号，比如 `"CAT"`、`"USDC"`
    
-   `decimals()`：小数位数，绝大部分 ERC‑20 用 18
    

这些通常就是返回一些 `storage` 里的常量/变量。

**2）查询类（只读，不改状态）** 

-   `totalSupply()`：当前这个代币的总发行量  
    实际上就是返回你在合约里存的一个 `uint256 totalSupply_`。
    
-   `balanceOf(address owner)`：查某个地址有多少这个代币  
    实现就是查 `balances[owner]`。
    
-   `allowance(address owner, address spender)`：查授权表里，`owner` 允许 `spender` 花多少  
    实现就是查 `allowed[owner][spender]`。
    

这三个都是 `view` 函数，不会花 gas（从外部 RPC 读的时候）。

**3）转账 / 授权类（会改状态）** 

这是 ERC‑20 的核心，会改 storage、会花 gas。

1.  `transfer(address to, uint256 amount)`  
    从 `msg.sender` 给 `to` 转 `amount` 个代币。一个最朴素的实现：直觉：你自己给别人转钱。  
    注意点：
    
    -   不能给 `0x000...000`，否则容易出问题
        
    -   要检查余额是否够
        
    -   记得 `emit Transfer` 事件，钱包/区块浏览器靠这个显示记录
        
2.  `approve(address spender, uint256 amount)`  
    授权：我允许 `spender` 以后可以从我这里最多划走 `amount` 个代币。朴素实现：直觉：
    
    -   你给一个 DeFi 协议（合约地址 A）授权：最多可以花你 1000 USDC
        
    -   将来协议里“存入”“借贷”“质押”时，协议合约就是通过 `transferFrom` 真正把你的币划走。安全坑点：
        
    
    -   直接从 `X -> Y` 修改授权可能出现“旧授权 + 新授权”竞态问题
        
    -   OpenZeppelin 推荐用 `increaseAllowance` / `decreaseAllowance` 或先设置为 0 再设新值
        
3.  `transferFrom(address from, address to, uint256 amount)`  
    使用授权来代替别人转账。朴素实现：直觉：
    
    -   用户先 `approve(DEX合约, 1000)`
        
    -   DEX 合约在撮合成交时，调用代币的 `transferFrom(用户, 对手方, 实际成交量)`
        
    -   所以“授权 + transferFrom”就是一套标准的“代币托管”模式
<!-- DAILY_CHECKIN_2026-01-27_END -->

# 2026-01-26
<!-- DAILY_CHECKIN_2026-01-26_START -->


\`\`\`

contract test {

string public text;

function set(string calldata \_text) external {

text = \_text;

// transaction cost(44689 gas) execution cost(23181 gas)

}

function set2(string memory \_text2) external {

text = \_text2;

// transaction cost(25274 gas) execution cost(3766 gas)

}

}

\`\`\`

刚刚测试了一下，calldata 和 memory 之间的 Gas 消耗差距还是挺大的。

这也就是说，在一些场景中，使用 calldata 这种只读关键字，比存在内存里的 memory 消耗的 Gas 会更少。如果能用 calldata，就不要用 memory 和 storage。

在编写智能合约的过程中，我们主要关心的是执行 Gas。

交易 Gas 通常包含以下几个部分：

1\. 固定基础 Gas

2\. 执行 Gas

3\. 向智能合约传输数据（Calldata）的 Gas

我们编写智能合约的核心目标是优化逻辑，从而节省执行 Gas。具体的优化手段包括：

1\. 减少循环操作

2\. 减少 SSTORE（存储写入）的操作

3\. 优化代码逻辑

之所以强调执行 Gas，是因为固定 Gas 是不可变的。当我们讨论某种操作比另一种操作更节省 Gas 时，本质上指的都是执行层面的优化。

当然，如果要评估用户最终实际支付的总额，还是需要关注交易 Gas，因为这才是用户的最终成本。但在智能合约的编码阶段，我们应当专注于如何优化执行 Gas，以实现更好的性能和更低的消耗。

**关于显式声明和隐式声明**

\`\`\`

// 显式声明

function updateTodo(uint _index, string calldata_ text, bool \_completed) external {

Todo storage todo = todos\[\_index\]; // 这里是从 storage 状态变量里面复制了一份出来给 todo ，实例化一个对象

todo.text = _text; // 这里的_ text 没有被修改，所以还是 只读 状态

todo.completed = \_completed;

}

// 隐式声明

function updateTodo2(uint _index, string calldata_ text) external {

todos\[\_index\].text = \_text;

}

function get(uint \_index) external view returns (string memory, bool) {

Todo memory todo = todos\[\_index\];

return (todo.text, todo.completed);

// 也可以下面这样写，更简洁的隐式写法

// return (todos\[\_index\].text, todos\[\_index\].completed);

}

function toggleCompleted(uint \_index) external {

// 这里没写 view、 pure、 reutrns ，就是修改了状态变量且没有返回值，在函数调用的时候就是 只修改，但看不到修改之后的状态，要用 get() 方法去查看

return todos\[\_index\].completed = !todos\[\_index\].completed;

}

\`\`\`

**存、读、取** 万能模板

1、存数据（新增）：向容器中添加元素

\- 容器是数组：\*\*数组名.push(结构体实例)\*\* 和上面的 create 函数一样

\- 容器是mapping：\*\*mapping名\[键\] = 结构体实例\*\*

比如 mapping(address => Todo) userTodos;

userTodos\[msg.sender\] = Todo(\_text, false);

\- 关键：创建结构体实例用 **结构体名(值1，值2)** 或 **结构体名({字段名1: 值1，字段名2: 值2})**

2、读数据（查询）：通过 ”定位符“ 获取字段

\- 定位符：数组用 ”索引“ **(数组名\[索引\])** ，mapping 用 “键” **(mapping名\[键\])**

\- 读单个字段：\*\*定位符.字段名\*\* （如 todos\[\_index\].text\]

\- 读多个字段：返回(定位符.字段1, 定位符.字段2)，引用类型返回要加 memory

\- 关键：读数据用 view 修饰符，不消耗 gas

3、改数据（更新）：通过 “定位符” 修改字段

\- 定位符：和读数据一致（数组索引 / mapping键）

\- 修改单个字段：\*\*定位符.字段名 = 新值\*\* 如 todos\[\_index\].text = \_text;

\- 修改布尔字段（切换）：\*\*定位符.字段名 = !定位符.字段名\*\* 如 toggleCompleted 函数）

\- 关键：修改的是 storage 中的原始数据，不需要额外声明 storage （除非要复用定位符）

\`\`\`

function Del(uint \_index) external {

Todo storage todo = todos\[\_index\];

delete todo.text;

}

function Del2(uint \_index) external {

delete todos\[\_index\];

}

\`\`\`

写函数三步法：

\- 明确函数目标：是“ 存、读、取”中的哪一种？（“新增”是存，“查询”是读）

\- 确定参数：需要哪些参数来 “定位数据”（索引 / 键）和“传递数据”（新值）

\- 套用模板：按 存 / 读 / 改 的模板写核心逻辑，引用类型注意 calldata / memory ，返回值注意 memory 和 view 修饰符

可以用加工厂的角度来解读函数工作原理，加工厂需要有原材料（参数），原料需要通过各种运输渠道送到工厂（函数 () 里面的东西），这里就是传递原料的路径，用什么方式传（传参），重货用大卡车（引用类型，如 string 、 struct 、 uint\[\]等），轻货小三轮就可以（uint 、 bool 、 address等），送到工厂之后，就会进入生产车间，生产线上（函数定义后的内部逻辑），最后经历一系列工序，产出成品（函数结果），成品有两种处理方式，一种是输出给外部（return 返回结果），一种是自己消化（没有 return值）

一个个函数就是一条条流水线，很多函数组合起来就是一个巨大的生产车间，一个最终的成品，需要经过很多道工序，也许这条流水线生产出来的东西，会是另一条流水线上的原材料，这也是函数的返回值传到另一个函数当参数，有些没有返回值的，就是已经没有下一首工序的成品或者材料，直到最后所有半成品生产好，再组合到一起去拼装成品，成品就是整个代码展现出的一种功能或者多种功能。

\`\`\`

// 复制待办：把 fromIndex 的待办文本，复制到 toIndex 的待办中

function copyTodo(uint fromIndex, uint toIndex) external {

// 1.调用 get 函数，获取 fromIndex 的待办文本（返回值是 (text, completed)

(string memory copyText, ) = get(fromIndex); // 只取第一个返回值 text ，第二个用 忽略

// 2.把 get 的返回值 copyText ，传给 updateText 当参数

updateText(toIndex, copyText);

}

\`\`\`

**这里有一个要注意的点：** 比如 `get` 函数返回的 `string memorymemory` 类型），可以直接传给 `updateText` 的 `string calldata _textcalldata` 类型）—— 因为两者都是 “函数级生命周期” 的临时数据，编译器会自动兼容。

但如果是 `storage` 类型的返回值（很少见，一般不推荐），就不能直接传给 `calldata` 或 `memory` 参数 —— 因为 `storage` 是永久数据，临时参数接不住

重点：

\- 我需要给函数传什么数据？

\- 函数要返回什么数据给我？

\- 先想这两个问题，再套 ”存读改模板“
<!-- DAILY_CHECKIN_2026-01-26_END -->

# 2026-01-25
<!-- DAILY_CHECKIN_2026-01-25_START -->



Figma 设计界面这节，看完之后，对这个工具的印象明显变了。

刚进文件的时候，第一感觉还是有点被信息量压住。左边一大串图层名字，中间一堆画板，右边一排参数，像是刚打开一个别人写的巨大项目，不知道从哪看起。不过慢慢跟着讲解挨个看，界面好像没那么吓人了，只是规则比较多。

左边那一列图层，之前基本都是无视的，这次才发现原来它才是文件的“骨架”。有命名的 Frame 排得整整齐齐，看上去就很舒服，没命名的那种“Frame 1 / Frame 2 / Rectangle 3”，一眼就透出一种混乱感。意识到这一点的时候，有一点羞愧，因为以前做东西就是随便画，完全没管结构，怪不得过几天再打开都看不懂自己在干嘛。

画布中间那块，最大的变化是开始注意“界面之间的关系”，而不是只盯着单个界面本身。几个 Frame 排在一起，看上去更像一个产品流程，而不是几张单独的截图。缩放、拖动画布这几个动作突然变得很自然，不再紧紧盯在一个画面上，而是会先缩小看一眼整体，再决定在哪个界面上动手。这个习惯以前在代码里是有的（先看目录，再看具体文件），现在慢慢延伸到了设计里。

右边的属性面板，以前觉得就是一堆参数，现在开始能对上号了：位置、大小、圆角、填充、描边、阴影，这些东西组合在一起，其实就构成了“风格”。和写 CSS 有点像，只不过变成可视化的表单。比起“好不好看”这种抽象评价，反而更关注“为什么是这个间距”“为什么是这个圆角”和“阴影到底加多少才不显得脏”。有点像在拆别人的设计：不是单纯感叹好看，而是琢磨配方。

整体看完这节，最明显的变化是：对 Figma 的“恐惧感”少了一些，对它的“秩序感”多了一些。左边是结构，中间是画面，右边是细节调节，这三件事情如果都能养成一点点习惯，后面学形状工具、自动布局、组件的时候，应该就不至于完全迷路。
<!-- DAILY_CHECKIN_2026-01-25_END -->

# 2026-01-24
<!-- DAILY_CHECKIN_2026-01-24_START -->




this：我自己这个合约

address(this) : 我自己这个合约变成一个钱包地址，用 address() 强制转换的

address(this).balance : 我钱包里面现在有多少钱

owner : 合约拥有者的钱包地址

payable(owner) : 不加 payable 这个修饰符的钱包地址不能直接收钱，加了才能收，也是强制转换

payable(owner).transfer(address(this).balance) : 把我现在这个合约转换成的钱包地址里面所有的钱通过 transfer 这个方法转给合约拥有者的钱包地址（现在常用 call ）

int256 public minInt = type(int256).min;

int256 public maxInt = type(int256).max;

type() 这个函数跟python里面不一样，这里是把这个类型表示出来，然后就可以用 .min 、.max 这种内置方法去获取这种类型的最小最大值

bytes1 a = 0xb5; // \[10110101\]

bytes1 b = 0x56; // \[01010110\]

bytes1 表示【1个字节】，1个字节 = 8 bit，所以这里 bytes1 代表 \[00000000\] ~ \[11111111\]

0x开头就是 16进制，一个数字代表 4个bit，换算成 2进制就可以了

address 的默认值是 0x0000000000000000000000000000000000000000 （40个0）

\- 因为Ethereum的地址固定是 20个字节（20 bytes）

\- 1字节 = 8 bit

\- 1字节 = 2 个十六进制字符

\- 所以 20个字节 = 40 个十六进制字符

如果定义了局部变量，但是又没有使用，就会报黄色警告

本地链开发和 Sepolia 测试网开发是没有区别的。主要涉及一些前端代码和智能合约代码。

关于是否有后端代码，目前暂时还没有接触到，因为整个过程是黑盒操作。

不过，从最初的链下开发到部署，再到修改网络，最后将前端代码部署到 Vercel 平台并完成连接，这一整套操作下来，让我对一个 DAPP 的组件构成和上线流程有了更深的了解。
<!-- DAILY_CHECKIN_2026-01-24_END -->

# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->





Gas 优化：才知道原来可以这样去优化Gas，之前从来没这么考虑过，但我觉得这应该要对底层架构有很深的了解才能去优化，或者已经很熟悉最优的路径，不过确实提升了一个视角

!\[\[58de2ef3046dc031233bd4a2de635953.png\]\]

!\[\[83ce2a2ad5fc67af1ae3cf14cee4fba4.png\]\]

uint8 = 0 ～ 256

uint16 = 0 ～ 65535

....

根据不同的字段去选择合适的类型，比如年龄用 uint8，身高用uint16，EVM是以256为一个存储单位的，uint默认会是uint256，但用uint128存储的话，可以在一个单位里面塞两个数，这样就可以省下一些存储空间，还有 i++ 和 ++i，后者比前者会更省，这个确实没想到过，后者是在本体的基础上加 1，在以前不会注意这些，在传统编程中可能也不会过多注意，但在这里，无处没有”性价比“。

溢出的风险在 0.8 版本之后好了很多，现在如果有溢出，会 revert，失败然后状态回滚，但在以前会环绕，257 变成 1 啥的，比较危险....

重入攻击：意思就是调用者往合约里面先存一些钱，按照正常逻辑，合约会判断账户余额是否有钱，如果有钱，就做一些操作，比如 swap ，比如跨链啥的，做完之后，再修改调用者的链上余额状态，所以这里有一个先后顺序，合约不是人类，它只会按照逻辑一步一步执行，这个时候就有攻击者发现，在合约最后一步修改链上状态前，攻击者可以一直调用这个合约取款函数（当然现在也可以），但取款合约的条件是攻击者余额 > 0 就可以，那只要合约没执行最后一步修改链上余额状态的代码，攻击者的余额就一直是满足条件的，他就可以一直取款，用很少的钱，甚至 1 gas也可以，去把合约里面的钱全套出来，直到合约里面没钱，或者没有Gas为止，这就是重入攻击，重复调用函数的攻击

解决办法也很简单，在判断完你的余额满足条件之后，直接先修改链上余额状态，修改完再把钱发给你，这里做恶基本上不可能了，因为可以不守信用的一方是合约了，但因为合约是开源的，公开可见，所以合约层又不会把你的钱吞掉，基本上这样就杜绝重入攻击的可能了。

\`\`\`

// 有漏洞的合约

contract bugContract {

mapping(address => uint256) public balances;

function deposit() external payable {

balances\[msg.sender\] += msg.value;

}

function getContractBalance() external view returns (uint256) {

return address(this).balance;

}

// 有漏洞的提现函数

function withdraw() external {

uint256 bal = balance\[msg.sender\]; // 1行

require(bal > 0, "No Balance"); // 2行

// 危险点：这里是外部调用，会执行 msg.sender 的回调函数

(bool sent, ) = [msg.sender.call](http://msg.sender.call){value: bal}(""); // 3行

require(sent, "Failed to send Ether"); // 4行

// 漏洞

balances\[msg.sender\] = 0; // 5行

}

}

\`\`\`

这里我的解决办法是，把第 5 行的代码，挪到第 2 行之后，第 3 行之前，判断完调用合约的地址如果有余额且满足条件，就先把状态改掉，免得它后面一直用回调函数一直把钱套出去
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-22
<!-- DAILY_CHECKIN_2026-01-22_START -->






这个 memory 和 storage 的使用场景总算是弄的差不多了，\*\*引用类型\*\* / **值类型** ，还有在 **合约层** / **函数层** ，还有在 **函数参数** / **函数内局部变量**

!\[\[Pasted image 20260122095943.png\]\]

还有一个 **calldata** 的修饰词，适合只读不修改的场景

!\[\[Pasted image 20260122100245.png\]\]

**判断流程：**

\- 变量是【合约体中的状态变量】 --> storage (默认)

\- 变量是【函数参数】 --> memory 或 calldata (只读用 calldata ， 可修改用 memory)

\- 变量是【函数内局部变量】 -->

\- 引用合约体中的状态变量 --> storage

\- 全新创建 --> memory

虽然意思都容易懂，但是为什么写代码的时候就那么用顺手呢，不是这里报错就是那里报错，明明一个很简单的东西，逻辑就是绕不过来

\`\`\`

contract DataLocations {

// 定义一个结构体

struct MyStruct {

uint foo;

string text;

}

// 定义一个 地址 => 结构体 的映射

mapping(address => MyStruct) public structs;

// 两个引用类型要用 memory ，返回值也需要，这里没有用 uint\[\] 和 string 返回，直接返回的是整个 结构体

function examples(uint\[\] memory y, string memory s) external returns (MyStruct memory) {

// 先给映射赋个值进去，这里有要注意的地方，这里跟 python 不一样，给字符值赋值的话要严格用 “” 号，不能用 ‘’ 号，不然会报错

structs\[msg.sender\] = MyStruct({foo: 123, text: "Alice"});

// 这里用 storage 从书架上拿本书下来做一下改动，然后再放回书架

MyStruct storage result = structs\[msg.sender\];

[result.foo](http://result.foo) = 456; // 因为是从书架上拿下来做的改动，之后再放回书架，所以长期生效

// 这里用 memory 记一下便利贴，用完就扔

MyStruct memory result2 = structs\[msg.sender\];

result.text = "Bob"; // 不会在链上做改变，函数调用时没看见有改变

return result; // 返回结构体 result，还有一个 result1 没返回

}

}

\`\`\`

对 calldata 意思可以理解，只在函数参数那里使用，跟 memory 差不多，不过 calldata 只读不改，所以会比 memory 更省Gas，如果不需要对这个变量或者值做任何修改，就可以使用 calldata ，但这里没有引用的很好，所以应用场景还是一知半解，脑子好胀.....
<!-- DAILY_CHECKIN_2026-01-22_END -->

# 2026-01-21
<!-- DAILY_CHECKIN_2026-01-21_START -->







解决“映射不能遍历”的问题，把列表的索引和映射的键值对揉合在一起

\`\`\`

contract IterableMapping {

mapping(address => uint) public balances; // 映射一个地址和余额

mapping(address => bool) public inserted; // 这里映射一个布尔值，用来判断是否已插入

address\[\] public Keys; // 定义一个address列表，用来存放地址表，方便后续用来索引遍历

function set(address _Key, uint_ val) external {

balances\[\_Key\] = \_val; // 把外部传入的参数分别赋给 balances 的映射，就是一个赋值给键，一个赋值给值

// 用来判断是否已加入数组Keys\[\]，我这里傻了，bool的默认值是false，我就还奇怪为什么inserted\[\_Key\]是 false ,前面加了个 ！还能继续往下执行，我这里忘记了 if 判断是需要后面是 true 才跑的，前面加一个 ！是对的，不加才不对，我傻了吧唧的，！就是一个取反符，这里用 if (inserted\[\_Key\] == false) 也可以，效果完全一样，不过这个直观一点

if (!inserted\[\_Key\]) {

balances\[\_Key\] = true; // 先标记为已加入

Keys.push(\_Key); // 这里把地址\_Key加入到Keys\[\]里面去，奇怪的是，为什么要先标记再操作？

// 可以反过来，只要是【原子操作】就行，原子操作就是在一个函数里面，要么都执行，要么都不执行

}

}

function getSize() external view returns (uint) {

return keys.length;

}

function first() external view returns (uint) {

return balances\[Keys\[0\]\];

}

function last() external view returns (uint) {

return balances\[Keys\[Keys.length - 1\]\];

}

function get(uint \_index) external view returns (uint) {

require(\_index < Keys.length, "Index out of bounds");

return balances\[keys\[\_index\]\];

}

}

\`\`\`

结构体，跟Python的类用法是一样的，不过solidity的结构体只能存字段，不能在里面写函数，而 Python 的类可以写函数

\`\`\`

contract Structs {

// 结构体，相当于自定义类，可以在里面设置很多自定义字段，车子有型号、出厂年份、车主等信息，还可以设置其它的，只要是你需要

struct Car {

string model;

uint year;

address owner;

}

Car public car; // 存一台汽车的信息，把一个抽象的概念变成一台现实中的车，这个车有型号、出厂年份、车主

Car\[\] public cars; // 存很多台车的信息

mapping(address => Car\[\]) public carsByOwner; // 地址 --> Car数组，存”每个车主对应的多辆车“，你们车太多了！

function examples() external {

// 结构体必须要有一个”存储方式”，memory 是内存，临时使用，函数执行完就销毁；还有一个是 storage ，链上永久存储，修改它会直接改变状态变量

Car memory toyota = Car("Toyota", 1990, msg.sender); // 这里在内存里面实例了一个 toyota 的汽车信息

Car memory lambo = Car({year: 1990, model: "Lambo", owner: msg.sender}); // 可以用键值对的形式赋值，这样就不用考虑顺序了，但要记得用 {} 包裹起来

Car memory tesla; // 先实例一个空的汽车，在下面再单独给汽车某个字段赋值

tesla.model = "Tesla";

tesla.year = 2000;

tesla.owner = msg.sender;

cars.push(toyota); // 把实例的汽车添加到数组里面去，有点放进车库的意思

cars.push(lambo);

cars.push(tesla);

cars.push(Car("Ferrari", 2020, msg.sender)); // 也可以一边放车库，一边实例汽车

// 要获取结构体数组中的值，可以用 Car memory _car = cars\[0\];_ car.model; _car.year;_ car.owner; 这样去获取就行，因为内存中是不能删除和修改的，只有在存储 storage 中才可以，下面是修改的用法

Car storage _car = cars\[0\]; // 在链上存储下，把车库里面的 0 号车的信息给到变量_ car

_car.year = 2026; // 这里_ car 就是 0 号车，把 0 号车的出厂年份改掉，也就改了它在链上的状态

delete \_car.owner; // 用 delete 把 0 号车的车主干掉，无主车，谁都没有真正拥有它，它是一台自由的车！！！

delete cars\[1\]; // 也可以直接把车库里面的 1 号车的所有信息全部清除，相当于把车销毁掉吧

}

}

\`\`\`

枚举类型，固定选项的集合，这个是把类型里面的值给固定住，定义好的枚举，只能用里面定义好的值，理解虽然差不多了，但还是不透彻，还需要在后面使用中慢慢加深理解

\`\`\`

contract Enum {

enum Status {

None, // 无状态,这个里面是用索引去保存的，所以用 ，号隔开，不用 ；号

Pending, // 待处理

Shipped, // 已发货

Completed， // 已完成

Rejected, // 已拒绝

Canceled, // 已取消

}

Status public status; // 定义一个枚举类型的 status

struct Order {

address buyer; // 买家地址

Status status; // 订单状态

}

Order\[\] public orders; // 定义一个 Order\[\] 类型的数组（存多个订单）

function get() view external returns (status) {

return status; // 获取当前 status 的枚举值，但在Remix里有点奇怪，只返回索引，不会返回特定的值

}

function set(Status \_status) external {

status = \_status; // 修改状态，传入枚举的索引，这里会修改，所以还是上面那个疑问，按道理来说，这里应该是可以填 Pending、Shiping、Completed、Rejected、Canceled 这些状态的

}

function ship() external {

status = Status.Shipped; // 上面是灵活修改状态，这里是固定修改成某个状态

}

function reset() external {

delete status; // delete基本上就是把所有状态和数值回归到默认状态

}

function CreateOrder(address _buyer, Status_ status) external {

_buyer =_ buyer == address(0) ? msg.sender : \_buyer; // 外部调用函数时可以不传入地址，默认是当前调用者地址，也可以传入地址参数

orders.push(\_buyer, \_status); // 前面定义了 orders 的结构体类型的数组，把传入的参数 push 进去

}

function getOrder() view external returns (Order\[\] memory) {

return orders; //

}

}

\`\`\`

\---

部署合约：在合约里面去部署合约

\`\`\`

contract TestContract1 {

address public owner = msg.sender;

function setOwner(address \_owner) external {

require(owner == msg.sender, "not owner");

owner = \_owner;

}

}

contract TestContract2 {

address public owner = msg.sender;

uint public value = msg.value;

uint public x;

uint public y;

// payable 是允许付钱、存款的意思，就是这个函数允许在调用时传个 value

constructor(uint _x, uint_ y) payable {

x = \_x;

y = \_y;

}

}

contract Proxy {

event Deploy(address);

function deploy(bytes memory \_code) external payable returns (address addr) {

assembly {

// create(v, p, n)

addr := create(callvalue(), add(\_code, 0x20), mload(\_code))

}

require(addr != address(0), "deploy failed");

emit Deploy(addr);

}

function execute(address _target, bytes memory_ data) external payable {

(bool success, ) = [_target.call_](http://target.call)_{value: msg.value}(_data);

require(success, "failed");

}

}

contract Helper {

function getBytecode1() external pure returns (bytes memory) {

bytes memory bytecode = type(TestContract1).creationCode;

return bytecode;

}

function getBytecode2(uint _x, uint_ y) external pure returns (bytes memory) {

bytes memory bytecode = type(TestContract2).creationCode;

return abi.encodePacked(bytecode, abi.encode(\_x, \_y));

}

function getCalldata(address \_owner) external pure returns (bytes memory) {

return abi.encodeWithSignature("setOwner(address)", \_owner);

}

}

\`\`\`

memory 和 storage 在什么情况下用？在什么情况下又不用？在什么情况下又用哪一个？

\- 局部变量如果是“状态变量的引用”，要用 storage，这样修改局部变量会直接改档案里面的原始数据

\- 函数参数是“临时输入”，用完就丢，没必要永久存，用 memory 更省资源

\- 合约里的”状态变量“（比如 owner）默认存在 storage 里，是永久数据

\- 局部变量用 memory ，”临时写新内容“（在便利贴上写个”下班买咖啡“，写完就扔）

**关键点：只有【引用类型】的函数参数，才需要显式写 memory（或 storage）；值类型的参数，默认不用写，也不能写，所以这里重点是分清【值类型】和【引用类型】**

**值类型：** uint(数字)、address(地址)、bool(布尔)、uint\[5\] (静态数组)

特点：像小纸条，数据本身很小，直接存在栈里，不用额外指定存储位置

**引用类型：** bytes(字节数组)、string(字符串)、mapping(映射)、uint\[\] (动态数组)、结构体、合约类型

特点：像大文件，数据可能很大，必须明确告诉编译器“存在哪里”（memory / storage）

**简单记**：只要参数是 **引用类型** 的，就必须写 memory / storage ；其它值类型，直接写类型名，不用管存储位置
<!-- DAILY_CHECKIN_2026-01-21_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->








关于ZK证明，又比之前更了解了一些，之前只知道零知识证明，是在验证者只知道结果正确的情况下，从而得到整个事件正确，但现在对于传统投票和零知识证明投票之间的优劣性有了更深的认识，链上传统投票，有点像是记名投票，在一些无关紧要的公众调研活动中无所谓，不用过于复杂的去考虑隐私性，但在一些比较敏感的场合下还运用这类“记名式投票”难免对一些人不方便，例如选举或者任何关于利益分配方面的事件，这个时候，零知识证明就派上了用场，三个特性决定了它未来的巨大应用场景，完备性、可靠性、零知识性，除了知道这个地址做了投票这个动作，其它一概不知道，在确认这个地址是否具备投票权，以及限制一个地址只能投票一次上都做了充分的技术保证，从Markle根确权到Nullifier防重复，再到最后的本地生成零证明，已经算是把无记名投票方式的概念充分在链上体现出来了，写到这里，我突然又想到刚刚看到的ERC-7962这个提案，它也是在链上隐私上面下工夫，它的核心就是把链上事件从绑定地址转移到绑定Hash上，那零知识证明可不可以和这个ERC-7962嵌套一下呢，先零知识再ERC-7962，这个最后一个公开信息（address）也被隐藏起来了，虽然说不是真的强隐私，无法追踪，但起码对一些专业技能不够强的人来说，这已经是很难去通过常规链上方法去追踪了，我想应该是会有人这样做的。  
  
映射--之前就一直看到合约里面有 => 这个符号，一直没懂是什么意思，刷课的时候第一遍完全没听懂，问了AI才知道，原来就是个字典，映射本身只负责【“键类型” -> “值类型”的关联存储】，业务含义是开发者通过 【变量名 + 代码逻辑】赋予的，和映射本身无关！

\`\`\`

contract Mapping {

mapping(address => uint) public balances; // 这里把地址当成键，把uint当成值，但是这里是值的类型，不是值本身，后面还要赋值或者传参到这两个 “键” “值” 里面去的，这是的意思是给 address 类型（比如用户钱包地址）映射成 uint 类型（比如地址的余额），balances 是类似于函数名的玩意

mapping(address => mapping(address => bool)) public isFriend; // 这个复杂一点，是嵌套映射，但如果用字典的方式来理解，也就是一个字典的键的值还是一个字典，套娃的感觉，第一层 address 可以理解成（我的地址），第二层 address（别人的地址），最后映射成 bool 类型，还是有点难理解，这里因为是判断两个人是否是朋友，那如果不是判断，而是其它情况呢？映射成 uint 可以吗？

// 通过【键1】找到一个“子映射”，再通过【键2】在子映射里找【值】

function examples() external {

balances\[msg.sender\] = 123; //给当前调用地址的地址余额赋值123，因为是uint类型，但为什么一定是余额呢，难道我这个名字设定成 balances 就一定余额吗，在链上难道就真的是余额吗？uint类型那么多，为什么不会是其它东西呢，比如 nonce 啥的...

uint bal = balances\[msg.sender\]; // 上面是存操作，这里是读操作，把当前调用地址的余额赋值给 bal 这个无符号整型变量

uint bal2 = balances\[address(1)\]; // 原来这个 address(1) 是solidity的测试地址（合法且无效），address()则是一个类型强制转换，跟python里面的 int()、str()一样，我还以为 1 是个什么索引呢，原来就是一个最普通的整数 1，不过 address() 把 1 强制转换成了一个以太坊合法的地址，所以这里用这个 0x000..01 当成键去 balances 对照表里面去寻找地址，因为从来没有被赋值过，所以 address(1) 是 “不存在的键” ，这里就会返回 uint 类型的默认值 0（根据类型的不同，返回不同的默认值）

balances\[msg.sender\] += 456; // 123 + 456 = 579 这个好理解，因为前面已经给这个地址映射了，然后又给了123的余额，这里直接用123+456

delete balances\[msg.sender\]; // 这里最终结果会是 0，delete 操作只能把映射的后的值变成0，也就是字典的值变成 0，但不会把这个把值位置清掉，那样的话键也没了

isFriedn\[msg.sender\]\[address(this)\] = true; // 当前调用者和本合约地址（address(this))是好友关系，在“我的地址”对应的子表里面，给“合约地址”的“是否好友”填true，但为什么这个address就是当前合约的地址呢，前面没有赋值啊，自带的关键字吗？还有这个（this）也不是很理解，是否只要写了 address(this) 就自动代表了当前合约地址？

// this 是Solidity内置的【当前合约实例】，address(this) 是把合约实例转换成地址类型，所以它【天生就代表当前合约的地址】，不需要手动赋值

}

}

\`\`\`
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-18
<!-- DAILY_CHECKIN_2026-01-18_START -->









删除数组元素--通过替换的方法

\`\`\`

contract ArrayReplaceLast {

uint\[\] public arr;

// 这里是准备把数组中最后一个元素替换掉原本要删除的那个元素，然后再 pop() 掉最后一个元素，这样就可以实现删除元素且改变长度的目的，但有一个缺点，就是数组内元素的位置会发生变化，对数组元素位置有要求的就不要使用这个方法，好处就是相比较于上面那个往左移动的方法，会更加节省Gas

// \[1, 2, 3, 4\] --> remove(1) --> \[1, 4, 3\]

function remove(uint \_index) public {

arr\[\_index\] = arr\[arr.length - 1\];

arr.pop();

}

function test() external {

arr = \[1, 2, 3, 4\];

remove(1);

// 断言结果为：\[1, 4, 3\]

assert(arr.length == 3);

assert(arr\[0\] == 1);

assert(arr\[1\] == 4);

assert(arr\[2\] == 3);

}

}

\`\`\`
<!-- DAILY_CHECKIN_2026-01-18_END -->

# 2026-01-17
<!-- DAILY_CHECKIN_2026-01-17_START -->










数组删除元素-通过移动位置方法

\`\`\`

contract ArrayShift {

unit\[\] public arr;

function example() public {

arr = \[1, 2, 3\];

delete arr\[1\]; // 这里的结果会是\[1, 0, 3\]

}

// 想实现删除指定位置元素后，不单单只是变成0，还把那个位置也删掉，改变数组实际长度，这里的实现路径是：先把指定位置的元素删除之后，剩下的右侧所有元素往左移一位，这时就会在最后多出一个元素（最后一个元素值），然后再用pop()弹出即可

// \[1, 2, 3\] --> remove(1) --> \[1, 3, 3\] --> \[1, 3\]

// \[1\] --> remove(0) --> \[1\] --> \[\]

function remove(uint \_index) public {

// 索引不能超过数组的长度，所以需要有一个 require 的报错处理

require(\_index < arr.length, "index out of bound");

// 这里i就是需要删除的值的索引，i的范围我迷糊了一下，问了AI，i < arr.length - 1 这段语句里面的i是以要删除这个值为基准的，也就是说\[1, 2, 3, 4\]这样一个数组，元素4的索引是3，写成 arr.length - 1 的话，remove(2) 的结果就会是 arr\[2\] = arr\[3\],这样就可以把4往左边移一下；但如果写成 i < arr.length 的话，结果就会是 arr\[3\] = arr\[4\],但因为索引最大就是3，没有4的索引，就会出现 undefined 的报错。

for (uint i = \_index; i < arr.length - 1; i++) {

arr\[i\] = arr\[i + 1\]; // 这里可以实现把右侧往左侧移一个位置

}

arr.pop(); // 这里把数组最后一个值弹出，实现改变数组长度的目的

}

function test() external {

arr = \[1, 2, 3, 4, 5\];

remove(2);

// 正确结果需要是：\[1, 2, 4, 5\]

assert(arr\[0\] == 1); // 这里用断言来判断最后结果是否如我们所设想的一样

assert(arr\[1\] == 2);

assert(arr\[2\] == 4);

assert(arr\[3\] == 5);

assert(arr.length == 4);

arr = \[1\];

remove(0);

// 正确结果需要是：\[\]

assert(arr.length == 0);

}

}

\`\`\`
<!-- DAILY_CHECKIN_2026-01-17_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->











今天就学到了可以改owner的方法，简单也简单，用管理员身份去指定下一个管理员，皇帝选皇帝，世袭制，也可以是举贤制，但总的来说，就是管理员权限最大，没有他的支持，当不了下一个管理员。

\`\`\`

// 这是一个很常见的权限管理合约

contract Ownable {

address public owner; // 先定义一个状态亦是，给管理员空间占个茅坑

constructor() {

onwer = msg.sender; // 用构造函数把第一个部署的合约地址赋值给owner

}

modifier onlyOwner() {

require(msg.sender == owner, "not owner"); // 整一个函数修饰器，判断当前调用这个函数的地址是不是管理员，如果是管理员就放行，如果不是，就报错

\_;

}

function setOwner(address \_newOwner) external onlyOwner {

require(\_newOwner != address(0), "invalid address"); // 这是一个用来给管理员指定下一个管理员的函数，但是当前管理员指定的下一个管理员地址不能是0地址，否则会被锁死

owner = \_newOwner; // 把新地址扔给owner

}

function onlyOwnerCatCallThisFunc() external onlyOwner {

// Code 这是只有管理员可以调用的函数

}

function anyOneCanCall() external {

// Code 这是所有人都可以调用的函数，主要是用来测试合约是否可以正常管理权限，我试过了，没问题的

}

}

\`\`\`

智能合约中接收返回值

\`\`\`

contract FunctionOutputs {

// public是为了外部和内部都可以调用，pure纯函数意味着不需要访问状态变量，定义了两种返回类型，这个returns我没有去研究，但猜测是在返回值有两个或以上数量时才会修饰一下，返回的类型就在后面用（）装起来统一定义，跟在数据定义不一样

function returnMany() public pure returns (uint, bool) {

return(1, true);

}

function named() public pure returns (uint x, bool b) {

// 这里可以给返回值弄一个名字，在上面的返回类型里面直接定义

return(1, false);

}

function assigned() public pure returns (uint x, bool b) {

// 这个就直接在里面定义，可以不用return了，我感觉是这样

x = 1;

b = true;

}

function destructingAssigments() public pure {

(uint x, bool b) = returnMany(); // 这就是在函数里面调用其它函数的用法，用（）把需要接收的函数值类型和名称定义好，然后跟变量赋值一样使用

(, bool b) = returnMany(); // 这里是只想接收某一个值，这里只想接收布尔值，这个跟python里面列表切片一样，不想要也需要占个坑，不然会报错，因为返回两个值，只弄了一个值接收，就不知道传那个给它，我猜测是这样

}

}

\`\`\`

solidity里面的数组玩法、以及在内存中如何定义、如何调用返回数组

\`\`\`

contract Array {

uint\[\] public nums = \[1, 2, 3\]; // 给数组赋一个默认值，定义类型就是基础类型后面加个\[\]，其它倒是没什么区别

uint\[3\] public numsFixed = \[4, 5, 6\]; // 定义一个固定数组，与动态数组区别在于类型定义的\[\]里面是否有限定数量

function examples() external {

nums.push(4); // 这个跟git一样，push一下，作用是往原有数组后推入一个新的值，所以这里会把nums变成\[1, 2, 3, 4\]

uint x = nums\[1\]; // 跟其它语言没什么区别，获取数据索引的值，赋值给变量无符号整型x

nums\[2\] = 777; // 跟上面一样，把777赋值给数组索引位置，也就是改操作,这里在会变成\[1, 2, 777, 4\]

delete nums\[1\]; // 我发现solidit这些语言的关键字都会使用一些比较完整的单词，相比较一些简单的语言使用缩写来说，我反而觉得这种形式会更直观一点,但这个删除操作在solidity里面只能把值变成0，不能直接把值真的删除掉，从而改变数组值的数量，所以这里会变成\[1, 0, 777, 4\]

nums.pop(); // 要改变数组数量，就需要用到pop()方法去弹出最后一个值，这里就变成\[1, 0, 777\]

nint len = nums.length; //跟python里面的len()一样

// create array in memory

uint\[\] memory a = new uint\[\](5); // 这里需要用到memory内存关键字，在内存里面定义一个新的数据，但内存中不能使用动态数组，所以需要给这个数组定义一个长度，但这里我不理解的是，为什么不跟前面一样，直接在\[\]里面定义，还需要在\[\]后面再加个()定义长度，真麻烦（噢，这里我明白了，我被这个(5)迷惑住了，这里因为是内存，所以需要在运行时动态获取数组长度，这里其实是type(len)，前面是类型，后面是动态获取到的长度，因为很多时候不知道这个长度是多少，所以不能使用numsFixed，只能是当时获取到多少长度，就用new这个关键字在运行时赋值给数组，uint\[\]是动态数组，就没有限定死长度，大白话就是：用 ‎new T\[\](len) 来在 memory 里分配一个长度为 len 的动态数组

a\[1\] = 123; // 因为只能用固定长度的数组，所以像push()、pop()这些能改变长度的方法就不能使用了，索引和赋值是可以使用的，在内存中局部变量只能够定义定长数组，而动态数组只能够存在于状态变量中

}

// 这个返回类型里面需要输入数组的索引，如果需要数组全部内容，就需要在返回类型的()里面定义一个memory的内存存储类型，这样就可以把数组中的所有元素返回出来

function returnArray() external view returns (uint\[\] memory) {

return nums; // 通过函数来返回数组的全部内容

}

}

\`\`\`
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->












函数修饰器 **modifier** ：这是个关键字，它的主要作用是把重复的代码弄到一块去，这样其它函数需要使用的时候直接修饰一下就可以了，还可以弄三明治形式，插到中间，总的来说，就是可以不用写太多重复代码，跟函数一样，我觉得应该可以算是函数里面的函数，叫函函算了。

\`\`\`

contract FunctionModifier {

pool public paused; // 定义暂停的状态变量，这里默认是false

uint public count; // 定义计数器

function setPaused(bool \_paused) external {

paused = \_paused;

}

modifier whenNotPaused {

require(!paused, "paused"); // 这里的逻辑我绕了一下，！是不的意思，paused是暂停的意思，paused的默认值又是false，所以这里就是 不false，不false=true，那这里就是true的意思，就不会报错，而且会继续执行后续的代码，刚开始对false和true没弄明白，这里是需要去接受外部输入的，就是用户调用，在调用层，用户输入的值如果是false，也就是默认值，这里就不会暂停，但如果用户输入的是true，这里就会是 ！true = 不true = false，那这里就会报错“paused”，就会暂停，因为这里条件不满足，这是require的判断逻辑。

\_; // 这就是函数修饰完之后其它代码插入的地方，我觉得可以理解成占位符吧，反正就是先占个茅坑

}

function inc() external whenNotPaused {

count += 1; // 这里就可以把在函数修饰符里面占的茅坑填上了

}

function dec() external whenNotPaused {

count -= 1; // 这里跟前面的inc()的功能是一样的，所以inc（）和dec（）两个函数甚至更多函数需要用到暂停这个功能，就直接用whenNotPaused修饰一样就可以了，就避免每次都需要写一行暂停，虽然不算麻烦，但整体看上去会更简洁，就是不知道会不会省Gas，应该不会，毕竟工作量还是要有的

}

modifier cap(uint \_x) {

require(\_x < 100, "x >= 100"); // 在函数修饰符里面加上变量，整体读下来，其实跟直接把判断逻辑写在函数里面没区别，就是把这个判断逻辑单独摘出来，然后其它函数可能也能用得上，如果只是单独放在一个函数里面，那就只有那个函数局部可以使用，想到这里，这个函数修饰符的作用，是不是就是把逻辑封装成一个类似于状态变量类型的东西呢？（这里AI纠正了一下，它说把修饰器说成是 逻辑模板/共享规则 会更好，我也觉得是的）状态变量是值，但这里是逻辑，同样是其它函数都可以使用，不过状态变量是函数和代码都可以调用，而这个函数修饰符只是给函数服务而已

\_;

}

// 这里有两个修饰符，按从左往右的顺序运行，先判断是否需要暂停，再判断用户输入的值是否小于100，这里cap的修饰里面需要传参进去

function incBy(uint _x) external whenNotPaused cap(_x) {

count += \_x; // 前面都没问题的话，这里的代码就会插入茅坑

}

modifier sandwich() {

// Code Here 这里就很好理解了，先执行第一步

count += 10;

\_; // 然后占茅坑

// More Code Here

count \*= 2; // 茅坑填完之后，再回到修饰符执行这段代码，跟三明治、汉堡包🍔一样，夹心的

}

function foo() external sandwich {

count += 1;

}

}

\`\`\`

构造函数 **constructor** ：这个玩意只能在Deploy Contract的时候定义一次，难怪叫构造函数，跟Docker里面Build功能差不多吧，定义完之后就不会变动了，暂时也没学到怎么改变构造函数那一块，可能需要重新部署干嘛的

\`\`\`

contract Constructor {

address public owner;

uint public x;

constructor(uint \_x) {

owner = msg.sender; // 这段定义部署者的代码应该是构造函数最常用的，在Deploy Contract时就会把msg.sender的地址传给owner，因为构造函数只能用一次，所以这时就把owner和合约部署者的地址绑定在一起，之后就可以用这个功能去设计一些只有owner可以调用的函数，这个倒是不难理解，msg.sender就是当前调用这个合约的地址，即时的，也是最核心的全局变量之一，但因为是即时的，所以在用户A--调用合约B--再调用合约C时，合约C的msg.sender会是合约B的地址，如果要获取到真实的用户A的地址，还需要其它一系列操作

x = \_x; // 这个就是在部署合约时需要用户输入的一个值，也会绑定给x，在这里没啥作用，就是说明一下可以这样写代码，有这么个玩意在

}

}

\`\`\`
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->













状态变量就是链上的信息，改状态变量就是改链上的数据

数据类型一般是有默认值的：

bool 默认是 false

uint 默认是 0

int 默认是 0

address 默认是 0x1e16

bytes32 默认是 0x1e32

还有一些没学到…

如果在一个文件里面定义了多个contract，在Deploy的时候，默认只会部署第一个contract，这个时候，需要在编译器的CONTRACT那个框框里面去单独选择其它合约Deploy，我还奇怪为什么写了两个contract，Deploy之后却只有一个contract

有一个常量的概念，事先我们是知道一些变量的固定值的，这些变量本身是不能被任何的合约所修改的，比如调用者address或者是一些特殊的编码，这些就会通常定义为常量，可以节省Gas，一般常量命名用大写，这是有一个 **constant** 的修饰符，是专门用来定义常量的，我尝试了一下，使用 constant 定义后的常量读取地址的Gas是 **373** ，没有用 constant 定义的读取地址的Gas是 **2485** ，这个差距有点大，但比起升级之前的Gas来说，已经可以算九牛一毛了，但这里有一个概念没弄明白，最开始是说只读方法可以不用消耗Gas的，但这里是说，常量在写入函数中还会再读取的， 这个时候就会按照是否定义常量来消耗Gas，这里我的理解就是如果不在写入函数，就是在不修改链上状态的情况下，是不用消耗Gas的

if else 条件判断

这个不难理解，修饰符搭配好就行

\`\`\`

function example(uint \_x) external pure returns (uint) {

if (\_x < 10) {

return 1;

} else if (\_x < 20) {

return 2;

}

return 3; // 这里默认返回3，可以加 return ，也可以不加

}

function ternary(uint \_x) external pure returns (uint) {

return \_x < 10 ? 1 : 2; // 三元表达式，这样写比较简洁， ？ 是满足就输出1，：是默认，也就是不满足条件就输出2

}

\`\`\`

循环：这里又学会一个新单词 pools

for循环 and while循环

这个跟python的逻辑是一样的，倒不用花多少时间重新理解，不过这里要注意的一点是while不能用true，这个不像其它语言在一些实时监控场景上可以用到，在区块链上用true会造成Gas无用消耗，就算加个break，最好也限制循环次数，毕竟Gas就是钱

!\[\[Pasted image 20260114110843.png\]\]

这几种错误判断让我迷糊了一下

require、revert、assert

!\[\[Pasted image 20260114114445.png\]\]

**require** 的判断逻辑是：\*\*满足条件就继续执行，不满足条件才抛出错误并且回滚\*\*

在上面截图中是

\`\`\`

require(\_i <= 10, “i > 10”);

// Code

\`\`\`

这里的判断逻辑是如果 \_i 小于并且等于10 ，就会去继续执行下面的代码，但如果不满足这个条件，就直接抛出 “i > 10“的报错信息并且回滚之前的操作，退还多余的Gas，但是已经执行了步骤消耗的Gas是不会退回的。

revert\*\* 的判断逻辑是：\*\*先通过代码判断条件，只要满足【定义的错误条件】，就主动抛出错误并回滚；不满足错误条件，就继续执行，这个就跟python差不多理解了，反正是条件满足就执行里面的语句\*\*

大白话解释：\*\*if(错误条件) { revert("错误提示");} 【我先检查是不是出现了某个错误情况，如果确实出现了，就直接报错，函数啥也不干了，之前的操作全部撤销；如果没出现这个错误情况，就继续执行后面的逻辑】\*\*

\`\`\`

if (\_i > 10) {

revert("i > 10"); // 关键逻辑

}

// More Code 只有 i <= 10 才会执行

\`\`\`

require是满足就执行后续代码，revert是满足就抛出报错，有点意思，反着来

**assert** 的判断逻辑是：\*\*校验一个【内部逻辑上绝对应该成立的条件】，只要这个条件成立，就继续执行；如果条件不成立（说明合约有bug），就强制报错并回滚，且不退还剩余Gas----这也就是断言吧\*\*

大白话解释：\*\*assert（正常条件） 【我这里的逻辑绝对没问题，这个条件肯定成立；如果居然不成立，那就是我的合约写崩了，直接报错终止，所有操作回滚，而且剩下的Gas全部消耗掉（提醒开发者赶紧查bug）】\*\*

\`\`\`

uint public num = 123; // 初始值

// 因为这个读取了状态变量，所以用的view

function testAssert() public view {

assert(num == 123); // 关键校验

// More Code 只有num == 123 才会执行

}

\`\`\`

如果满足条件，也就是断言成功，就不会报错，继续执行下面的代码，如果条件不满足，也就是被其它代码改动了初始值，断言没成功，就会弹系统默认提示，函数停止，这是一个bug，必须要修复。

assert不是用来校验外部输入的，而且用来校验【内部逻辑的不变性】，对内查bug用的，触发了就是代码有问题，去老老实实修bug就行了。

**自定义报错** ：这可以用来节省Gas，因为有些报错信息会很长，会浪费很多Gas，这时就可以用自定义报错，对于我这种英文不好的，我觉得更适合，弄成对中文开发者比较友好的报错信息，就是不知道中文字符会不会比英文字符所消耗的Gas更多。

\`\`\`

error MyError(address caller, uint i); // 这里面还可以加变量参数呢，可以知道当时报错时的值（比如谁触发的错误，参数值是多少） error是关键字，MyError是错误名称

function testCustomError() public view {

if (\_i > 10) {

revert MyError(msg.sender, \_i); // msg.sender(调用者的地址) i(传入的参数值)作为参数传给错误，相当于“报错时带上具体信息”

}

}

\`\`\`

自定义错误有几个好处：更省Gas、更灵活、更规范
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->














public 代表公开

\## 类型和值

bool ：布尔值 true和false

uint ：无符号整型，有uint256、uint8、uint16（往上加8就行）

int ：整型，-2 **256 to 2** 255 - 1

.min : 最小值

.max : 最大值（这些内置函数跟python一样）

address : solidity独特的变量类型，内容就是个16进制数字

bytes32 ：比地址类型长，是32位数字

contract : 合约，后面接函数名

function ：函数

external ： 访问权限关键词，作用跟public一样，控制谁可以调用这个函数，但它只允许外部账户（比如我自己的钱包地址或者其他合约），当前合约内部的其他函数不能够调用

pure ：纯函数类型，不能够读、也不能够写状态变量，只能局部变量，概念还不是很懂，就说是不能对链上进行任何的读写操作

状态变量约等于python的全局变量，但是solidity的状态变量是存储在区块链上的，区块链不死，就可以一直存在，操作不可逆，修改要gas，而python是存储在系统内存中的，随时可以改，程序一关，变量就没了

在函数局部里面，可以直接修改状态变量，这里跟python不同，不需要加global之类的关键字，因为solidity需要便捷的操作它，所以简化了“访问/修改权限”的声明，但是这里有一个要注意的点：如果要修改状态变量，在函数名后的修饰符那里，就不能用 pure 或者 view，pure是不能读也不能写，view是只读不写，如果加了这两个修饰符，就会报错，保持默认或者根据需要加 public / external权限

原来external、internal、public这些关键字是为了省gas来分场景使用的，public内外都可以调用，所以会占用更多空间，external只有外部能调用，所以把内部空间省下了，在高频外部调用场景中，很好用

pure、view和无修饰符，是强制约束 + gas优化，我还以为可以从宽松到严格随便使用

如果只使用局部变量 / 输入参数 ：必须用 pure

如果只读状态变量但不修改 ： 必须用 view

如果需要改状态变量 ：不能用 pure / view

按“从严格到宽松”的顺序来判断：

先看是否改状态（改则无修饰符） 》》不改的话，看是否会读状态（读则 view ，不读则 pure）
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->















今天是实习计划月的第一天，上午把入门计划全部过了一遍，因为在开营之前就把实习的一些教学材料看过一遍，今天才发现原来是那些东西，所以学习压力不算很大，除了视频还没有全部看完，第一周的任务差不多完成了。  
就个人而言的话，很多知识已经有过初步的了解，因为自己对web3很感兴趣，所以也自己摸索了两年，比较明显的缺点就是对于行业没有一个系统的全面了解，一直是东学一点、西学一点，好处是所有赛道都了解一点，坏处就是都了解的不深，这次来这里，主要也是想补齐这个短板，另外关于行业通识这一块，比较有收获的就是行业工作形式和法律合规这一块，对自己的未来道路如何走也有很大的指导作用，特别是准备学走开发一块的话，原来涉及空投逻辑的代码也有可能触及法律。  
如果进度快的话，通识应该有个两三天的时间就可以简单掌握，接下来的计划，也是我认为的难点，就是编码这一块，只有一个python的基础，对于web3来说，肯定是远远不够的，光是语法这一块，就得花很大精力，任重道远啊
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
