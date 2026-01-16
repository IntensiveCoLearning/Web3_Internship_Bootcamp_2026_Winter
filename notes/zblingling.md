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
