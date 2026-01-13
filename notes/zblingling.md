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
# 2026-01-13
<!-- DAILY_CHECKIN_2026-01-13_START -->
> link: [Rust Cource-圣经](https://course.rs/basic/intro.html)

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
