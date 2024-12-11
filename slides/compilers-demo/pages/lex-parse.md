---
layout: section
---
# Lexer & Parser

---
level: 2
---

调研了几个开源社区的工具，[rust-langdev](https://github.com/Kixiron/rust-langdev)

<v-click>

很多parser都自带一个lexer, 但是还是决定分开写
- [lrlex](https://crates.io/crates/lrlex): lex/flex 的 rust binding, unsafe
- [lexgen](https://crates.io/crates/lexgen): 但是不维护了, 不好
- [Logos](https://github.com/maciejhirsz/logos): 社区活跃，好，就是它了！

</v-click>

<v-click>

Parser 也有两种选择, parser combinator or parsing expression generator
- 有啥区别？combinator 就是递归下降..., peg 就是 yacc/bison-like 的LL(1)/LR(1)/LALR(1)
- [peg](https://crates.io/crates/peg): rust-peg鼻祖，太简单了以及不维护了
- [nom](https://crates.io/crates/nom): 社区活跃好用，但是combinator
- [pest](https://pest.rs/): 非常优雅，但是error handling不可控，玄学
- [lalrpop](https://github.com/lalrpop/lalrpop): 社区活跃，跟yacc很像，可定制性强，兼容手写/其他的lexer, 就是它了！

</v-click>

<v-click>

写完才发现北大的[编译原理课](https://pku-minic.github.io/online-doc/#/)用的就是lalrpop

</v-click>

---
level: 2
---
# Logos


---
level: 2
---
# LALRPOP


---
level: 2
---
# Error Recovery
