---
timezone: UTC+8
---

# Boling Zhou

**GitHub ID:** zblingling

**Telegram:** 

## Self-introduction

Web3 实习计划 2025 冬季实习生

## Notes

<!-- Content_START -->
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
