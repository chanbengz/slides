---
layout: section
---
# About Rust
`rustc` is what makes Rust great!

---
level: 2
---
# Why Rust?
When we talk about Rust, we refer to words like **memory safety**, **efficiency**, **expressiveness**, **open-source** and so on... Nowadays, when we talk about system safety, we can't avoid mentioning Rust.
### What exactly makes Rust different from other languages?
Rust Compiler `rustc` may give you the answers.
- <v-click> It has a very powerful frontend system whereas the backend is backed up by the famous <span v-mark.underline.orange>LLVM. (The same backend for compilers like clang/intel DPC/C++ compilers)</v-click>
- <v-click>The explicit definition of ownership and borrowing rules enbales the compiler to perform <span v-mark.underline.orange>static analysis</span> on the code, resolving issues only using frontend.</v-click
- <v-click> <code>rustc</code> moves from traditioal <span v-mark.underline.orange> Pass-based</span> compilation to  <span v-mark.underline.orange>Demand-driven</span> compilation, which is more efficient.</v-click>
- <v-click> <code>rustc</code> is a <span v-mark.underline.orange>self-hosted</span> compiler, which means it is written in Rust itself. With this, the compiler can be more <span v-mark.underline.orange>flexible</span> and <span v-mark.underline.orange>maintainable</span>.</v-click>
---
level: 2
---
# Compiling Rust


---
level: 2
---
# The features we are talking about today
