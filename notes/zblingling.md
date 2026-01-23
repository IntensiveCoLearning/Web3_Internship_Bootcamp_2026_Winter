---
timezone: UTC+8
---

# Boling Zhou

**GitHub ID:** zblingling

**Telegram:** 

## Self-introduction

se major, into crypto

## Notes

<!-- Content_START -->
# 2026-01-23
<!-- DAILY_CHECKIN_2026-01-23_START -->
Uniswap v4 (from v3)

\* 数据结构：Singleton (单例)

\* v3: Factory -> Pool Contract (多合约，Deployment 贵)

\* v4: PoolManager.sol (单合约，所有池子都在里面)

\* 关键变量: mapping(PoolId id => Pool.State) public pools

\* 优势: 跨池路径无需多次转账，创建池子成本忽略不计

\* 结算机制：Flash Accounting (闪电记账)

\* 核心: 基于 EIP-1153 瞬态存储

\* 流程:

\* 调用 manager.unlock()

\* 在回调 lockAcquired 里操作 swap/modifyLiquidity

\* 内部修改 currencyDelta（欠款账单）

\* 退出前必须结算 settle（给钱）或 take（拿钱），使 delta 归零

\* 代码:

// 外部不再直接 swap，而是先解锁

manager.unlock(abi.encode(params))

\* 核心扩展：Hooks (挂钩)

\* 原理: 在核心逻辑执行前/后回调外部合约

\* 标识: Hook 地址的 前 14 bits（前缀）决定启用的钩子类型（如 0x8000... 代表开启 beforeInitialize）

\* 生命周期:

\* before/afterInitialize

\* before/afterAddLiquidity / RemoveLiquidity

\* before/afterSwap

\* before/afterDonate (新增：直接捐赠给 LP)

\* 资产表达：Native ETH & ERC-6909

\* Native ETH: 不再强制 WETH，直接存取 address(0)，省去 Wrap Gas

\* ERC-6909: 内部记账标准

\* 交易产生的 delta 可以不提现，转为 6909 额度留在 Manager

\* 下次交易直接消耗额度，实现 0 外部转账 交易

\* 池子标识：PoolKey

\* 定义: (Currency0, Currency1, fee, tickSpacing, hooks)

\* ID: keccak256(PoolKey)

\* 注意: 同一币对如果 Hook 不同，即为不同的池子

\* Gas 优化点总结

\* Transient Storage: 取代 SSTORE，操作结束后自动清空，读写极廉价

\* Multi-hop: 无论路径多深，只有首尾涉及 ERC20.transfer
<!-- DAILY_CHECKIN_2026-01-23_END -->

# 2026-01-20
<!-- DAILY_CHECKIN_2026-01-20_START -->

-   特征对象 \[sol中慎用\]
    
    总是通过**引用**: 通过 `&` 引用或者 `Box<T>` 智能指针的方式来创建特征对象
    
    用法上是泛型的延伸, 因为在同一结构体中不能同时存储不同类型的对象 `let v: Vec<T: Draw> = vec![1.1f64, 8u8];`是错误的
    
    泛型-静态分发; 特征对象-动态分发
    
-   Self 和 self
    
    ```
    trait Draw {
        fn draw(&self) -> Self;
    }
    ​
    #[derive(Clone)]
    struct Button;
    impl Draw for Button {
        fn draw(&self) -> Self {
            return self.clone()
        }
    }
    ​
    // self 指的是当前的实例对象, Self 指的是 Button 类型
    ```
    
-   对象安全
    
    没有泛型参数; 没有方法的返回值类型是 Self
<!-- DAILY_CHECKIN_2026-01-20_END -->

# 2026-01-19
<!-- DAILY_CHECKIN_2026-01-19_START -->


> link: [Rust Course-圣经](https://course.rs/basic/intro.html)

-   特征对象 (Trait Objects)
    
    实现多态：允许存储不同类型但实现相同 Trait 的实例
    
    `dyn` 关键字：表示动态分发（Dynamic Dispatch），在运行时确定具体调用哪个方法
    
    ```
    // 必须使用指针：&dyn Trait 或 Box<dyn Trait>
    pub struct Screen {
        pub components: Vec<Box<dyn Summary>>,
    }
    ```
    
    -   限制：只有满足“对象安全”的 Trait 才能拥有特征对象（如：方法不返回 `Self`，不包含泛型参数）。
        

-   错误处理 (Error Handling)
    
    Rust 不支持 Exception，通过返回类型处理异常
    
    -   不可恢复错误：`panic!("crash and burn")`
        
    -   可恢复错误：`Result<T, E>` 枚举
        
    
    ```
    enum Result<T, E> {
        Ok(T),
        Err(E),
    }
    ​
    // 传播错误：? 运算符（简写 match）
    fn read_username() -> Result<String, io::Error> {
        let mut s = String::new();
        File::open("hello.txt")?.read_to_string(&mut s)?;
        Ok(s)
    }
    ```
<!-- DAILY_CHECKIN_2026-01-19_END -->

# 2026-01-16
<!-- DAILY_CHECKIN_2026-01-16_START -->



> link: [Rust Course-圣经](https://course.rs/basic/intro.html)

-   泛型 (Generics)
    
    ```
    fn add<T>(a:T, b:T) -> T {
        a + b
    }
    ​
    // 类型限制: 规定泛型必须实现的特征
    // 也可以写作 fn fn_name <T>() where T: std::fmt::Debug
    fn display_array<T: std::fmt::Debug, const N: usize>(arr: [T; N]) {
        println!("{:?}", arr);
    }
    ```
    
-   特征 (Trait)
    
    孤儿规则: 如果要为类型 A 实现特征 T, A 和 T 至少有一个是在当前作用域中定义的
    
    ```
    pub trait Summary {
        // 可以包含该方法的默认实现
        fn summarize(&self) -> String {
            String::from("(Read more...)")
        }
    }
    ```
    
    使用特征作为函数参数: 特征作为泛型约束条件 的另一种写法
    
    ```
    pub fn notify(item: &impl Summary) {
        println!("Breaking news! {}", item.summarize());
    }
    ​
    // same as
    pub fn notify<T: Summary>(item: &T) {
        println!("Breaking news! {}", item.summarize());
    }
    ```
<!-- DAILY_CHECKIN_2026-01-16_END -->

# 2026-01-15
<!-- DAILY_CHECKIN_2026-01-15_START -->




> link: [Solana basics](https://solana.com/docs)

-   concepts
    
    account：合约是无状态的stateless，程序逻辑与数据彻底分离，不同于eth中同时storage存储数据和合约代码
    
    并行的要点：合约提前声明需要读写哪些账户
    
    三大支柱
    
    PDA
    
    rent账户需要预存sol来维持生存
    
    spl token是sol官方的一套标准代币合约
<!-- DAILY_CHECKIN_2026-01-15_END -->

# 2026-01-14
<!-- DAILY_CHECKIN_2026-01-14_START -->





> link: [Rust Course-圣经](https://course.rs/basic/intro.html)

-   所有权和借用
    
    &和\*：引用（Reference）与解引用（Dereference）
    
    ```
    let x = String::from("hello");
    // let x : &str = "hello" 仅引用，再赋值也不会发生所有权转移
    let y = x; // 所有权发生了“移动”（Move）！
    ```
    
    函数传参也会造成移动
    
    借用 (borrow)
    
    &mut 可变引用
    
    & 和 &mut 不能同时存在，&mut 同时只能存在一个，引用作用域的结束位置是最后一次使用的位置
    

-   Option 枚举: 处理空指针
    
    ```
    enum Option<T> {
        None,
        Some(T),
    }
    ​
    // 用 match 处理, 示例
    fn plus_one(x: Option<i32>) -> Option<i32> {
        match x {
            None => None,
            Some(i) => Some(i + 1),
        }
    }
    ```
    

-   模式 (Pattern): 条件语句 + 解构
    
    正则表达式
    

-   方法 (Method)
    
    ```
    struct File { ... }
    ​
    impl File {
        fn new() -> File { ... } // 构造器默认名称 new
        fn someFunc() { ... }
    }
    ```
    
    对象和方法定义分离
    
    Enum / Trait 也可以实现方法
    
    -   self 的用法:
        
        ```
        impl Rectangle {
            // &self = self: &Self
            fn area(&self) -> u32 {
                self.width * self.height
            }
        }
        ```
        
        &self 表示该方法对 Rectangle 的不可变借用 &mut self 表示可变借用
<!-- DAILY_CHECKIN_2026-01-14_END -->

# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->






> link: [Rust Course-圣经](https://course.rs/basic/intro.html)

-   variable blindings
    
    rust 支持手动变量的可变性，变量的**所有权**通过绑定进行转移
    
    Shadowing: 同变量名在内部作用域遮蔽，在类型不同时不冲突
    
-   所有权和借用
    
    &和\*：引用（Reference）与解引用（Dereference）
    
    ```
    let x = String::from("hello");
    // let x : &str = "hello" 仅引用，再赋值也不会发生所有权转移
    let y = x; // 所有权发生了“移动”（Move）！
    ```
<!-- DAILY_CHECKIN_2026-01-13_END -->

# 2026-01-12
<!-- DAILY_CHECKIN_2026-01-12_START -->









## **rust basics ongoing**

> link: [Rust by example](https://doc.rust-lang.org/rust-by-example/custom_types/enum.html), [Rust Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024)

-   custom type: struct/enum 区别
    
    match 判定 enum 的类型
    
    ```
    enum WebEvent {
        PageLoad,
        ...
        Click { x: i64, y: i64 },
    }
    ​
    fn inspect(event: WebEvent) {
        match event {
            WebEvent::PageLoad => println!("page loaded"),
            ...
            WebEvent::Click { x, y } => {
                println!("clicked at x={}, y={}.", x, y);
            },
        }
    }
    ```
<!-- DAILY_CHECKIN_2026-01-12_END -->
<!-- Content_END -->
