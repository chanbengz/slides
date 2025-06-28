---
level: 2
---
# Hints

一些帮助你开始本章练习的内容。

<v-click>

首先，你应该已经知道这个挑战要求你做什么，并从仓库下载起始代码。

</v-click>

---
level: 2
---
# Parallelization

<v-click>

除了 `main.rs` 之外，起始代码中的所有内容都是 **可并行化的** 和 **可修改的**。

</v-click>

<v-click>

例如，加密和解密只是简单地遍历网格并加密/解密每个单元格，所以你可以使用 `par_iter` 来并行化这个过程。

</v-click>

<v-click>

`update_grid` 函数也是如此，它根据生命游戏的规则更新网格。

</v-click>

---
level: 2
---
# Simple SIMD

<v-click>

你不需要手写 SIMD 代码，因为 `tfhe-rs` 库已经为 LWE 加密方案提供了 SIMD 实现。

</v-click>

<v-click>

**搜索如何启用这个功能。** 🔍

</v-click>

---
level: 2
---
# Compilation

<v-click>

你知道该怎么做。😉

</v-click>

---
level: 2
---
# Ad-hoc Optimizations

<v-click>

我能想到的最有效的优化之一是克隆 `tfhe-rs` 并优化它，但这对于这个挑战来说并不是真正必要的。

</v-click>

<v-click>

第二好的方法是 **基准测试** 为什么起始代码很慢，比如 FHE 类型如 `FheUInt8` 的计算成本。

</v-click>

<v-click>

同时，有一些不必要的或写得很愚蠢的东西，比如 **不必要的克隆**。

</v-click>

<v-click>

**你应该移除它。** 🧹

</v-click>

---
level: 2
---
# (Bonus) Switching to Different Algorithms

<v-click>

Babai 的最近平面算法并不是 LWE 解密的最佳算法。

</v-click>

<v-click>

我注意到我们有其他可能工作得更好的算法，包括但不限于：

</v-click>

<div v-click>

- **BKZ 算法**
- **pnj-bkz**
- **筛法（G6k）**
- **你告诉我。**

</div>

<v-click>

**我非常兴奋地想看看你能想出什么。给我惊喜！** 🎉

</v-click>