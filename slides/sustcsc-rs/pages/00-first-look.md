---
layout: section
---
# First Look at Rust

<v-click>

欢迎来到 Rust 的世界！🦀

</v-click>

<v-click>

⚠️ **注意**：Rust 比其他语言稍微复杂一些，但是安全性和性能带来的好处是值得的！

</v-click>

---
level: 2
---
# Creating Your First Project

<br>
<v-click>

使用 Cargo 创建你的第一个 Rust 项目：

```bash
$ cargo new hello-world --bin
$ cd hello-world
$ tree .
.
├── Cargo.toml
└── src
    └── main.rs
```

</v-click>

<v-clicks>

- `--bin`: 创建一个独立的可执行项目
- `--lib`: 创建一个库项目  
- `Cargo.toml`: 项目配置和依赖
- `src/main.rs`: 带有 `main()` 函数的入口点

</v-clicks>

---
level: 2
---
# Your First Rust Code

```rust
fn main() {
    println!("Hello world!");
}
```

<v-clicks>

🔍 **关键观察点：**
- `fn` 定义一个函数
- `println!` 是一个 **宏**（注意 `!`）- 不是函数
- 不需要像 C/C++ 那样写 `return 0;`
- Rust 自动处理退出代码

</v-clicks>

---
level: 2
---
# Variables and Type Inference

<br>
<v-click>

Rust 有强大的类型推断功能 - 你很少需要指定类型：

```rust
fn main() {
    let x = 5;              // i32 (推断)
    let y = 5.0;            // f64 (推断) 
    let name = "Rust";      // &str (推断)
    let numbers = vec![1, 2, 3]; // Vec<i32> (推断)
    
    println!("x = {}, y = {}", x, y);
    println!("Hello, {}!", name);
    println!("Numbers: {:?}", numbers);
}
```

</v-click>

<v-click>

✨ **Rust 的编译器很聪明** - 它能从上下文推断类型！

</v-click>

---
level: 2
---
# Mutability
为了安全，变量 **默认是不可变的**：

````md magic-move {lines: true}
```rust
fn main() {
    let x = 5;
    println!("x = {}", x);
    x = 6; // ❌ 错误：不能对不可变变量进行二次赋值
}
```

```rust
fn main() {
    let mut x = 5;  // ✅ 用 `mut` 使其可变
    println!("x = {}", x);
    x = 6;          // ✅ 现在可以工作了！
    println!("x = {}", x);
}
```
````

---
level: 2
---
# Type Annotations (When Needed)
有时你需要帮助编译器：

```rust
fn main() {
    let x: i32 = 5;         // 显式类型注解
    let y = 10u32;          // 类型后缀
    let z = 3.14_f32;       // f32 而不是默认的 f64
    
    // 当推断失败时：
    let mut v = Vec::new(); // ❌ 什么类型？
    let mut v: Vec<i32> = Vec::new(); // ✅ 清楚了！
    
    // 或者提供上下文：
    let mut v = Vec::new();
    v.push(42); // ✅ 现在 Rust 知道它是 Vec<i32>
}
```

---
level: 2
---
# Ownership: Rust's Superpower
Rust 在 **编译时** 防止内存错误：

````md magic-move {lines: true}
```rust
fn main() {
    let s1 = String::from("Hello");
    let s2 = s1; // 将所有权转移给 s2
    println!("{}", s1); // ❌ 错误：s1 不再有效
}
```

```rust
fn main() {
    let s1 = String::from("Hello");
    let s2 = s1.clone(); // 克隆数据
    println!("{}", s1);  // ✅ s1 仍然有效
    println!("{}", s2);  // ✅ s2 有自己的副本
}
```

```rust
fn main() {
    let s1 = String::from("Hello");
    let s2 = &s1;        // 借用一个引用
    println!("{}", s1);  // ✅ s1 仍然有效
    println!("{}", s2);  // ✅ s2 只是一个引用
}
```
````

---
level: 2
---
# Running Your Code

<v-click>

构建并运行你的项目：

```bash
# 一步编译并运行
$ cargo run

# 只编译（检查错误）
$ cargo check

# 使用优化编译（发布模式）
$ cargo build --release
```

</v-click>

<v-click>

🚀 **Cargo** 是 Rust 的构建系统和包管理器 - 它处理一切！

</v-click>