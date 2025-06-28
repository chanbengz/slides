---
layout: section
---
# Tuning Rust Code for Performance

让你的代码极速飞驰 🚀

---
level: 2
---
# Parallelism with Rayon

<br>
<v-click>

安装 rayon：`cargo add rayon`

</v-click>

````md magic-move {lines: true}
```rust
// 顺序处理
let data = vec![1, 2, 3, 4, 5];
let doubled: Vec<_> = data.iter().map(|&x| x * 2).collect();
```

```rust
// 并行处理
use rayon::prelude::*;

let data = vec![1, 2, 3, 4, 5];
let doubled: Vec<_> = data.par_iter().map(|&x| x * 2).collect();
```

```rust
// 并行任务
use rayon::prelude::*;

let data = vec![5, 1, 8, 22, 0, 44];
rayon::join(
    || println!("总和：{}", data.iter().sum::<i32>()),
    || println!("乘积：{}", data.iter().product::<i32>()),
);
```
````

---
level: 2
---
# Optimizing Parallel Workloads

<br>
<v-click>

**问题**：小任务的线程开销

</v-click>

<v-click>

**解决方案**：使用分块来平衡工作负载

</v-click>

```rust
use rayon::prelude::*;

let data: Vec<i32> = (0..1_000_000).collect();

// 使用分块优化
let result: Vec<_> = data
    .par_chunks(1024)           // 分块处理
    .flat_map(|chunk| {
        chunk.iter().map(|&x| x * x)  // 每个块的重计算
    })
    .collect();
```

<v-click>

🎯 **规则**：平衡线程创建成本与并行性收益

</v-click>

---
level: 2
---
# SIMD (Single Instruction, Multiple Data)

<br>
<v-click>

**SIMD**：在多个数据上同时执行相同操作

</v-click>

<v-click>

⚠️ **需要 nightly Rust**：`rustup default nightly`

</v-click>

```rust
#![feature(portable_simd)]
use std::simd::prelude::*;

fn simd_sum(data: &[f32]) -> f32 {
    const LANES: usize = 8; // 一次处理 8 个浮点数
    
    let (prefix, body, suffix) = data.as_simd::<LANES>();
    
    let simd_sum = body.iter()
        .copied()
        .sum::<Simd<f32, LANES>>()
        .reduce_sum();
        
    let scalar_sum: f32 = prefix.iter().chain(suffix).sum();
    
    simd_sum + scalar_sum
}
```

---
level: 2
---
# Compilation Optimizations

<br>
<v-click>

配置 `Cargo.toml` 以获得最大性能：

</v-click>

```toml
[profile.release]
opt-level = 3           # 最大优化
lto = "fat"            # 链接时优化
codegen-units = 1      # 更好的优化
panic = "abort"        # 更小的二进制大小
```

<v-clicks>

- **opt-level 3**：激进的编译器优化
- **LTO**：跨 crate 边界优化
- **codegen-units**：用编译时间换取运行时性能

</v-clicks>

---
level: 2
---
# Build Commands

<br>

```bash
# 调试构建（快速编译，慢速执行）
cargo build

# 发布构建（慢速编译，快速执行）
cargo build --release

# 使用优化运行
cargo run --release

# 目标特定优化
RUSTFLAGS="-C target-cpu=native" cargo build --release
```

<v-click>

🔥 **专业提示**：总是使用 `--release` 标志进行基准测试！

</v-click>

---
level: 2
---
# Performance Best Practices

<br>
<v-clicks>

1. **先测量**：使用 `cargo bench` 或分析工具
2. **避免分配**：重用向量，尽可能使用切片
3. **选择正确的数据结构**：`Vec` vs `VecDeque` vs `HashMap`
4. **迭代器链**：通常比手动循环更快
5. **内联**：为热点函数使用 `#[inline]`
6. **LLVM 提示**：帮助编译器更好地优化

</v-clicks>

<v-click>

```rust
#[inline(always)]
fn hot_function(x: f64) -> f64 {
    x * x + 2.0 * x + 1.0
}
```

</v-click>