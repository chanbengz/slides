---
layout: section
---
# Variables and Numbers

本章涵盖在 Rust 中使用数字进行计算 🔢

---
level: 2
---
# Constants
<br>
<v-clicks>

- 类似于不可变变量，但 **总是** 不可变
- 必须有显式的类型注解
- 可以在任何作用域中定义

</v-clicks>

```rust
const PI: f64 = 3.141592653589793;

fn main() {
    const MAX_SIZE: usize = 100;
    println!("PI = {}, MAX_SIZE = {}", PI, MAX_SIZE);
}
```

---
level: 2
---
# Number Types

<div class="grid grid-cols-2 gap-4 text-sm">
<div>

**有符号整数：**
- `i8`, `i16`, `i32`, `i64`, `i128`
- `isize`（指针大小）

**无符号整数：**
- `u8`, `u16`, `u32`, `u64`, `u128`  
- `usize`（指针大小）

</div>
<div>

**浮点数：**
- `f32`（单精度）
- `f64`（双精度）

**示例：**
```rust
let a: i32 = 42;
let b: f64 = 3.14159;
let c: u8 = 255;
```

</div>
</div>

---
level: 2
---
# Structs
将相关数据组合在一起：

```rust
struct Point {
    x: f64,
    y: f64,
    metadata: String,
}

fn main() {
    let p = Point {
        x: 1.0,
        y: 2.0,
        metadata: String::from("2D 空间中的一个点"),
    };
    println!("点：({}, {})", p.x, p.y);
    println!("元数据：{}", p.metadata);
}
```

---
level: 2
---
# Tuples
不同类型的固定大小集合：

```rust
fn main() {
    let tuple: (i32, f64, char) = (42, 3.14, 'a');
    
    // 通过索引访问
    println!("值：{}, {}, {}", tuple.0, tuple.1, tuple.2);
    
    // 解构
    let (x, y, z) = tuple;
    println!("解构：x={}, y={}, z={}", x, y, z);
}
```

---
level: 2
---
# Enums
定义具有多个变体的类型：

<div class="grid grid-cols-2 gap-4">
<div>

**简单枚举：**
```rust
enum Direction {
    Up,
    Down,
    Left,
    Right,
}
```

</div>
<div>

**带数据的枚举：**
```rust
enum Card {
    Clubs(u8),
    Diamonds(u8),
    Hearts(u8),
    Spades(u8),
}

let card = Card::Hearts(10);
```

</div>
</div>

---
level: 2
---
# Pattern Matching with Enums

```rust
let card = Card::Hearts(10);

match card {
    Card::Clubs(value) => println!("梅花：{}", value),
    Card::Diamonds(value) => println!("方块：{}", value),
    Card::Hearts(value) => println!("红心：{}", value),
    Card::Spades(value) => println!("黑桃：{}", value),
}
```

<v-click>

🎯 **模式匹配** 确保你处理所有可能的情况！

</v-click>