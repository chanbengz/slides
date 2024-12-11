---
layout: section
---
# Intro

---
layout: two-cols
level: 2
---

用 Rust 实现一个 <del>SPL</del> 的编译器
<v-clicks> 

- 不完全是 SPL, 魔改了一些语法 
    - <del> 先定义再计算太不符合直觉 </del>
    - SPL 是弱类型的，改成了强类型 
    - 部分 Semantic Errors 在 Parse 阶段检查, 减少semantic工作
    - <del> 选了个要求严格的PEG，后面再讲 </del> 

- 为啥用 Rust 
    - <del> 吃饱了撑的 </del>
    - <del> Rewrite everything in Rust </del>
    - rustc 自举，用 Rust 写编译器完全可行
    - 这学期学 OS 恰好也在用 Rust

</v-clicks>

::right::

<img src="https://chanbengz.github.io/slides/compilers-demo/screenshot.png" width="400"/>
