---
layout: section
---
# Functions and Functional Programming

用于编写健壮和富有表现力代码的高级概念 🛠️

---
level: 2
---
# Function Syntax

```rust
fn function_name(param1: Type1, param2: Type2) -> ReturnType {
    // 函数体
    param1 + param2  // 没有分号 = 返回值
}

fn no_return_value() {
    println!("这返回单元类型 ()");
} // 等同于 -> ()
```

<v-clicks>

- **返回类型**：使用 `->` 显式声明或隐式单元类型 `()`
- **返回值**：最后一个没有 `;` 的表达式或显式的 `return`
- **单元类型**：`()` 类似其他语言中的 `void`

</v-clicks>

---
level: 2
---
# Option Type

<v-click>

安全地处理可空值：

</v-click>

````md magic-move {lines: true}
```rust
fn get_value(condition: bool) -> Option<i32> {
    if condition {
        Some(42)  // 有值
    } else {
        None      // 无值
    }
}
```

```rust
fn main() {
    let value = get_value(true);
    
    // 安全解包
    match value {
        Some(v) => println!("得到：{}", v),
        None => println!("无值"),
    }
}
```

```rust
fn main() {
    let value = get_value(false);
    
    // 便利方法
    let result = value.unwrap_or(0);        // 默认值
    let doubled = value.map(|x| x * 2);     // 如果是 Some 则转换
}
```
````

---
level: 2
---
# Result Type

<v-click>

处理可能失败的操作：

</v-click>

```rust
fn divide(a: i32, b: i32) -> Result<i32, String> {
    if b == 0 {
        Err("不能除以零".to_string())
    } else {
        Ok(a / b)
    }
}

fn main() {
    let result = divide(10, 2);
    match result {
        Ok(value) => println!("结果：{}", value),
        Err(error) => println!("错误：{}", error),
    }
}
```

---
level: 2
---
# The `?` Operator
优雅地传播错误：

```rust
fn calculate() -> Result<i32, String> {
    let a = divide(10, 2)?;  // 如果有错误则传播
    let b = divide(20, 4)?;  // 否则继续
    Ok(a + b)
}

// 等同于：
fn calculate_verbose() -> Result<i32, String> {
    let a = match divide(10, 2) {
        Ok(val) => val,
        Err(e) => return Err(e),
    };
    // ... b 也类似
    Ok(a + b)
}
```

---
level: 2
---
# Pattern Matching
强大的 `match` 表达式：

```rust
enum Number {
    Integer(i32),
    Float(f64),
    Complex(f64, f64),
    Zero,
}

fn process(num: Number) -> String {
    match num {
        Number::Integer(i) => format!("整数：{}", i),
        Number::Float(f) => format!("浮点数：{:.2}", f),
        Number::Complex(r, i) => format!("{}+{}i", r, i),
        Number::Zero => "零".to_string(),
    }
}
```

---
level: 2
---
# If Let and While Let
简化的模式匹配：

<div class="grid grid-cols-2 gap-4">
<div>

**if let：**
```rust
let num = Some(42);

if let Some(value) = num {
    println!("得到：{}", value);
}

// 而不是完整的 match
match num {
    Some(value) => println!("得到：{}", value),
    None => {}
}
```

</div>
<div>

**while let：**
```rust
let mut stack = vec![1, 2, 3];

while let Some(item) = stack.pop() {
    println!("项目：{}", item);
}

// 比 loop + match 更清洁
```

</div>
</div>

---
level: 2
---
# Iterator Methods
使用迭代器进行函数式编程：

```rust
let numbers = vec![1, 2, 3, 4, 5];

// 转换和收集
let doubled: Vec<i32> = numbers
    .iter()
    .map(|x| x * 2)           // 转换每个元素
    .filter(|&x| x > 4)       // 保留值 > 4
    .collect();               // 收集到 Vec 中

// 聚合操作
let sum: i32 = numbers.iter().sum();
let product: i32 = numbers.iter().product();

// 自定义折叠/归约
let result = numbers.iter().fold(0, |acc, x| acc + x);
```

---
level: 2
---
# Closures
捕获环境的匿名函数：

```rust
fn main() {
    let multiplier = 3;
    
    // 捕获 `multiplier` 的闭包
    let multiply = |x| x * multiplier;
    
    let numbers = vec![1, 2, 3, 4, 5];
    let result: Vec<i32> = numbers
        .iter()
        .map(multiply)           // 使用闭包
        .collect();
        
    println!("{:?}", result);   // [3, 6, 9, 12, 15]
}
```

<v-click>

🚀 **闭包** 可以通过引用、可变引用或所有权进行捕获！

</v-click>