---
layout: section
---
# Loops and Arrays

迭代和数据结构的基本概念 🔄

---
level: 2
---
# For Loops
<br>
<v-click>

Rust 没有 C 风格的 `for` 循环。相反，它使用基于迭代器的语法：

</v-click>

````md magic-move {lines: true}
```rust
// 范围（不包含结束）
for i in 0..10 {
    println!("i = {}", i); // 打印 0 到 9
}
```

```rust
// 范围（包含结束）
for i in 0..=10 {
    println!("i = {}", i); // 打印 0 到 10
}
```

```rust
// 遍历集合
let arr = [1, 2, 3, 4, 5];
for item in arr {
    println!("item = {}", item);
}
```
````

---
level: 2
---
# Loop and While

<div class="grid grid-cols-2 gap-4">
<div>

**无限循环：**
```rust
loop {
    println!("永远！");
    // 使用 `break;` 退出
}
```

**条件循环：**
```rust
let mut x = 0;
while x < 5 {
    println!("x = {}", x);
    x += 1;
}
```

</div>
<div>

**带返回值的循环：**
```rust
let result = loop {
    if condition {
        break 42; // 返回 42
    }
};
```

**循环标签：**
```rust
'outer: loop {
    loop {
        break 'outer; // 跳出外层循环
    }
}
```

</div>
</div>

---
level: 2
---
# Iterators
迭代器是 **惰性的** 和 **可组合的**：

```rust
let numbers: Vec<i32> = (0..10)
    .filter(|&x| x % 2 == 0)  // 保留偶数
    .map(|x| x * x)           // 平方它们
    .collect();               // 收集到 Vec 中

println!("{:?}", numbers); // [0, 4, 16, 36, 64]
```

<v-click>

**步进迭代：**

```rust
for i in (0..10).step_by(2) {
    println!("i = {}", i); // 0, 2, 4, 6, 8
}
```

</v-click>

---
level: 2
---
# Arrays - Fixed Size
数组有编译时已知的大小：

```rust
let mut arr: [i32; 5] = [0; 5]; // 5 个零的数组
let arr2 = [1, 2, 3, 4, 5];     // 推断类型：[i32; 5]

arr[0] = 10; // 修改元素
println!("第一个元素：{}", arr[0]);

// 遍历数组
for (index, value) in arr.iter().enumerate() {
    println!("arr[{}] = {}", index, value);
}
```

<v-click>

⚠️ **注意**：不同大小的数组是不同的类型！

</v-click>

---
level: 2
---
# Vectors - Dynamic Size
向量可以在运行时增长和收缩：

```rust
let mut v: Vec<i32> = Vec::new(); // 空向量
let mut v2 = vec![1, 2, 3];       // 初始化宏

// 添加元素
v.push(10);
v.push(20);
v.extend([30, 40, 50]);

// 访问元素
println!("第一个：{}", v[0]);
println!("长度：{}", v.len());

// 安全访问
match v.get(10) {
    Some(value) => println!("找到：{}", value),
    None => println!("索引超出范围"),
}
```

---
level: 2
---
# Slices
切片是数组或向量的视图：

```rust
let arr = [1, 2, 3, 4, 5];
let vec = vec![10, 20, 30, 40, 50];

// 不同的切片语法
let slice1 = &arr[1..4];    // [2, 3, 4]
let slice2 = &vec[2..];     // [30, 40, 50]
let slice3 = &arr[..3];     // [1, 2, 3]
let slice4 = &vec[..];      // 整个向量

for item in slice1 {
    println!("item = {}", item);
}
```

---
level: 2
---
# ndarray for Numerical Computing
安装 crate：`cargo add ndarray`

```rust
use ndarray::{array, Array2, s};

// 创建矩阵
let zeros = Array2::<f64>::zeros((3, 4)); // 3x4 零矩阵
let matrix = array![[1, 2, 3], [4, 5, 6]]; // 2x3 矩阵

// 索引访问
println!("元素 [0,1]：{}", matrix[[0, 1]]); // 2

// 切片
let col = matrix.slice(s![.., 1]); // 第二列
let row = matrix.slice(s![0, ..]); // 第一行
```

<v-click>

🚀 **性能**：优化的内存布局和操作！

</v-click>