---
layout: section
---
# Intro

---
level: 2
---

用 Rust 实现一个 <del>SPL</del> 的编译器
<v-clicks> 

<li> 不完全是 SPL, 魔改了一些语法 
    <li> <del> 先定义再计算太不符合直觉 </del> </li>
    <li> SPL 是弱类型的，改成了强类型 </li>
    <li> 部分 Semantic Errors 在 Parse 阶段检查, 减少semantic工作 </li>
    <li> <del> 选了个要求严格的PEG，后面再讲 </del> </li>
</li>

<li> 为啥用 Rust 
    <li> <del> 吃饱了撑的 </del> </li>
    <li> <del> Rewrite everything in Rust </del> </li>
    <li> rustc 自举，用 Rust 写编译器完全可行 </li>
    <li> 这学期学 OS 恰好也在用 Rust </li>
</li>

</v-clicks>

