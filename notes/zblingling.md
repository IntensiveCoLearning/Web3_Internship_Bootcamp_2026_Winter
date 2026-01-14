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
