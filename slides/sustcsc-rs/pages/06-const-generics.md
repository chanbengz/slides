---
level: 2
---
# Trait and Generics

本章将回顾 Rust 的类型系统，并解释使用 trait 和泛型时的一些常见危险和陷阱。

<v-click>

虽然类型系统看起来很灵活，但它仍然非常静态，需要你的注意 - **这就是 Rust 的魅力所在**。

</v-click>

---
level: 2
---
# Generics
你可能从 C++ 或 Java 中学过。在 Rust 中非常相似。

````md magic-move {lines: true}
```rust
fn add<T>(a: T, b: T) -> T {
    a + b // T 必须实现 `Add` trait
}
```

```rust
fn main() {
    let a = 1;
    let b = 2;
    let c = add(a, b); // T 被推断为 i32
    println!("{} + {} = {}", a, b, c);
}
```

```rust
add::<i32>(a, b); // T 被显式指定为 i32
```

```rust
use std::ops::Add;

fn add<T: Add<Output = T>>(a: T, b: T) -> T {
    a + b // T 必须实现 `Add` trait
}
```
````

---
level: 2
---
# Generics with Structs

泛型也可以与结构体和枚举一起使用，允许你定义可以与任何数据类型工作的类型。

```rust
struct Point<T> {
    x: T,
    y: T,
}
```

---
level: 2
---
# Trait and `impl`

<v-click>

Trait 是在 Rust 中定义 **共享行为** 的一种方式。你可以把它们想象成其他语言中的接口。

</v-click>

<div v-click>

以下是如何定义一个 trait：

```rust
trait Shape {
    fn area(&self) -> f64;
}
```

</div>

<div v-click>

然后，你可以为不同的类型实现这个 trait：

```rust
impl Shape for Point<f64> {
    fn area(&self) -> f64 {
        0.0 // 点没有面积
    }
}
```

</div>

---
level: 2
---
# `impl` Blocks

<v-click>

正如你可能注意到的，`p.area()` 看起来像 OOP 风格。Rust 不是 OOP 语言，但它支持为结构体或枚举定义函数，这被称为 **`impl` 块**。

</v-click>

````md magic-move {lines: true}
```rust
fn main() {
    let p = Point { x: 1.0, y: 2.0 };
    println!("点的面积：{}", p.area()); // 调用 area 方法
}
```

```rust
impl Point<f64> {
    fn new(x: f64, y: f64) -> Self {
        Point { x, y }
    }
}
```
````

---
level: 2
---
# Derive Macros

<v-click>

除了显式实现 trait 之外，Rust 还提供了一种使用 **默认方法** 实现 trait 的方式，使用宏。

</v-click>

```rust
#[derive(Debug, Clone, Copy)]
struct Point<T> {
    x: T,
    y: T,
}
```

<v-click>

现在你为 `Point<T>` 实现了 `Copy`、`Clone` 和 `Debug` trait。**这些繁重的工作已经一去不复返了！** 🎉

</v-click>

---
level: 2
---
# Display Trait

我们必须提到另一个有用的 trait，`Display`，你可以定义类型在打印时应该如何格式化。

```rust
use std::fmt;

impl<T: fmt::Display> fmt::Display for Point<T> {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "Point({}, {})", self.x, self.y)
    }
}

fn main() {
    let p = Point { x: 1.0, y: 2.0 };
    println!("点：{}", p); // 调用 Display trait
}
```

---
level: 2
---
# Const Generics

<v-click>

常量泛型是 Rust 中的一个功能，允许你定义一个 **带有常量值的泛型类型**。

</v-click>

<v-click>

你无法想象在此之前，Rust 实际上有一个 **愚蠢的问题**：

</v-click>

```rust
fn main() {
    let v1 = [1u32; 32];
    let v2 = [1u32; 33];

    println!("{:?}", v1);   // ✅ 工作正常
    println!("{:?}", v2);   // ❌ 错误！
    //               ^^ `[u32; 33]` 无法使用 `{:?}` 格式化
    //                  因为它没有实现 `std::fmt::Debug`
}
```

---
level: 2
---
# The Problem Before Const Generics

<v-click>

因为 Rust 的 libcore 通过扩展宏为大小从 **0 到 32** 的所有数组实现了 `std::fmt::Debug`，但没有为 33 实现。

</v-click>

<v-click>

这看起来很奇怪，所以 Rust 引入了一个叫做 **常量泛型** 的新功能。

</v-click>

<v-click>

我们可以想象 `fmt` 变得简单：

```rust
impl<T: fmt::Debug, const N: usize> fmt::Debug for [T; N] {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        fmt::Debug::fmt(&&self[..], f)
    }
}
```

</v-click>

---
level: 2
---
# Const Generics Use Cases

<v-click>

如你所见，类似于数组，常量泛型可以用来定义带有常量值的泛型类型，比如 **矩阵** 和 **向量**，这在数值计算中非常有用。

</v-click>

<v-click>

让我们看一个使用常量泛型的矩阵实现的实际例子...

</v-click>

---
level: 2
---
# Matrix Example - Structure

```rust
use std::{
    fmt::Debug,
    ops::{Add, Mul},
};

#[derive(Copy, Clone, Debug)]
struct Matrix<T: Copy + Debug, const N: usize, const M: usize>([[T; M]; N]);

impl<T: Copy + Debug, const N: usize, const M: usize> Matrix<T, N, M> {
    pub fn new(v: [[T; M]; N]) -> Self {
        Self(v)
    }

    pub fn with_all(v: T) -> Self {
        Self([[v; M]; N])
    }
}
```

<v-click>

这里，`N` 和 `M` 是表示矩阵维度的 **常量泛型参数**。

</v-click>

---
level: 2
---
# Matrix Example - Default Implementation

```rust
impl<T: Copy + Default + Debug, const N: usize, const M: usize> Default for Matrix<T, N, M> {
    fn default() -> Self {
        Self::with_all(Default::default())
    }
}
```

---
level: 2
---
# Matrix Example - Multiplication

```rust
impl<T, const N: usize, const M: usize, const L: usize> Mul<Matrix<T, M, L>> for Matrix<T, N, M>
where
    T: Copy + Default + Add<T, Output = T> + Mul<T, Output = T> + Debug,
{
    type Output = Matrix<T, N, L>;

    fn mul(self, rhs: Matrix<T, M, L>) -> Self::Output {
        let mut out: Self::Output = Default::default();

        for r in 0..N {
            for c in 0..M {
                for l in 0..L {
                    out.0[r][l] = out.0[r][l] + self.0[r][c] * rhs.0[c][l];
                }
            }
        }

        out
    }
}
```

---
level: 2
---
# Matrix Example - Usage

```rust
type Vector<T, const N: usize> = Matrix<T, N, 1usize>;

fn main() {
    let m = Matrix::new([
        [1f64, 0f64, 0f64], 
        [1f64, 2f64, 0f64], 
        [1f64, 2f64, 3f64]
    ]);
    let v = Vector::new([[10f64], [20f64], [40f64]]);

    println!("{:?} * {:?} = {:?}", m, v, m * v);
}
```

<v-click>

编译时的 **类型安全**！维度在编译时进行检查。🚀

</v-click>