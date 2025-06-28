---
layout: section
---
# Ownership and Lifetime

Rust 内存安全的秘密武器 🔒

---
level: 2
---
# Memory Management Approaches

<div class="grid grid-cols-2 gap-4">
<div>

**垃圾回收：**
- ✅ 用户友好
- ❌ 运行时开销
- ❌ 不可预测的暂停

**手动管理：**
- ✅ 完全控制
- ❌ 容易出错
- ❌ 内存泄漏/崩溃

</div>
<div>

**所有权（Rust）：**
- ✅ 零成本抽象
- ✅ 编译时安全
- ✅ 无运行时开销
- ❌ 学习曲线

</div>
</div>

<v-click>

🎯 **Rust 防止** 悬空指针、双重释放、内存泄漏在 **编译时**！

</v-click>

---
level: 2
---
# The Three Rules of Ownership

<br>
<v-clicks>

1. **每个值都有唯一的所有者**
2. **当所有者超出作用域时，值被丢弃**
3. **只能有一个可变引用 OR 多个不可变引用**

</v-clicks>

<v-click>

这些规则防止数据竞争和内存损坏！🛡️

</v-click>

---
level: 2
---
# Ownership Transfer (Move)

<br>

````md magic-move {lines: true}
```rust
fn main() {
    let a = String::from("Hello");
    let b = a; // 所有权从 a 转移到 b
    println!("{}", a); // ❌ 错误：移动后使用值
}
```

```rust
fn main() {
    let a = 100;  // i32 实现了 Copy trait
    let b = a;    // 值被复制，不是移动
    println!("{}", a); // ✅ OK：a 仍然有效
}
```

```rust
fn take_ownership(s: String) {
    println!("{}", s);
} // s 超出作用域并被丢弃

fn main() {
    let s = String::from("Hello");
    take_ownership(s); // 所有权移动到函数
    // println!("{}", s); // ❌ 错误：s 不再有效
}
```
````

---
level: 2
---
# Borrowing with References
Use references to avoid transferring ownership:

```rust
fn calculate_length(s: &String) -> usize {
    s.len()
} // s goes out of scope, but doesn't drop the String

fn main() {
    let s = String::from("Hello");
    let len = calculate_length(&s); // Borrow s
    println!("'{}' has length {}", s, len); // ✅ s still valid
}
```

<v-click>

📝 **Borrowing**: Getting a reference without taking ownership

</v-click>

---
level: 2
---
# Mutable References

````md magic-move {lines: true}
```rust
fn main() {
    let mut s = String::from("Hello");
    change(&mut s);  // Mutable borrow
    println!("{}", s); // "Hello, world!"
}

fn change(s: &mut String) {
    s.push_str(", world!");
}
```

```rust
fn main() {
    let mut s = String::from("Hello");
    
    let r1 = &mut s;
    let r2 = &mut s; // ❌ ERROR: cannot have two mutable references
    
    println!("{}, {}", r1, r2);
}
```

```rust
fn main() {
    let mut s = String::from("Hello");
    
    let r1 = &s;
    let r2 = &s;     // ✅ OK: multiple immutable references
    let r3 = &mut s; // ❌ ERROR: cannot mix mutable and immutable
    
    println!("{}, {}, {}", r1, r2, r3);
}
```
````

---
level: 2
---
# Clone vs Copy

<div class="grid grid-cols-2 gap-4">
<div>

**Copy trait** (automatic):
```rust
let a = 42;
let b = a;  // Copied automatically
println!("{}", a); // ✅ Still valid

// Types: i32, f64, bool, char, 
// tuples of Copy types, etc.
```

</div>
<div>

**Clone trait** (explicit):
```rust
let a = String::from("Hello");
let b = a.clone(); // Explicit clone
println!("{}", a); // ✅ Still valid

// Deep copy of heap data
// More expensive operation
```

</div>
</div>

---
level: 2
---
# Lifetimes
Sometimes Rust needs help understanding reference lifetimes:

````md magic-move {lines: true}
```rust
fn longest(s1: &str, s2: &str) -> &str {
    if s1.len() > s2.len() {
        s1
    } else {
        s2
    }
} // ❌ ERROR: Cannot infer lifetime
```

```rust
fn longest<'a>(s1: &'a str, s2: &'a str) -> &'a str {
    if s1.len() > s2.len() {
        s1
    } else {
        s2
    }
} // ✅ OK: Explicit lifetime annotation
```

```rust
fn main() {
    let s1 = String::from("Hello World!");
    let s2 = String::from("World");
    let result = longest(&s1, &s2);
    println!("Longest: {}", result);
}
```
````

---
level: 2
---
# Lifetime in Practice

```rust
fn main() {
    let r;                    // --------+-- 'a
    {                         //         |
        let x = 5;            // -+-- 'b |
        r = &x;               //  |      |
    }                         // -+      |
    println!("r: {}", r);     // --------+
}
// ❌ ERROR: x doesn't live long enough
```

<v-click>

**Rule**: References must always point to valid data

</v-click>

<v-click>

🧠 **Most of the time**, Rust can infer lifetimes automatically!

</v-click>