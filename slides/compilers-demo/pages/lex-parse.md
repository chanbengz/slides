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
- [logos](https://github.com/maciejhirsz/logos): 社区活跃，好，就是它了！

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
Create ridiculously fast Lexers

一个高性能lexer，关键是写起来很舒服。

```rust
#[derive(Debug, Logos, PartialEq, Clone)]
#[logos(error = LexicalError)] // 后面再说
#[logos(skip r"[ \t\r\n\f]+")] // 跳过空白字符
pub enum Token {
    #[end]
    EndOfProgram,

    // Operators
    #[token(">")]
    OpGreaterThan,
    // ...

    #[regex("true|false", |lex| lex.slice() == "true")] 
    LiteralBool(bool),
    #[regex(r"(?:0|[1-9]\d*)?\.\d+(?:[eE][+-]?\d+)?", |lex| lex.slice().parse::<f32>().unwrap(), priority = 10)]
    LiteralFloat(f32),
```

---
level: 2
---
正则难写？不写正则!    

```rust
    // ...
    #[regex(r"//[^\n]*\n?", logos::skip)]
    LineComment,
    #[token("/*", process_block_comment)]
    BlockComment,
}

fn process_block_comment(lex: &mut logos::Lexer<Token>) -> FilterResult<(), LexicalError> {
    if let Some(len) = lex.remainder().find("*/")  {
        lex.bump(len + 2);
        FilterResult::Skip
    } else {
        FilterResult::Error(LexicalError::UnexpectedEndOfProgram)
    }
}
```

支持自定义处理函数(callback function)，可以处理更复杂的情况。

`lex.bump` 有点像 `flex` 的 `yyless/yymore` 但是直观很多。

---
level: 2
---
Logos 生成的 token stream 还带有 Span 信息，可以用来生成更详细的错误信息。

```rust
impl<'input> Iterator for Lexer<'input> {
    type Item = Spanned<Token, usize, LexicalError>;

    fn next(&mut self) -> Option<Self::Item> {
        self.token_stream
        .next()
        .map(|(token, span)|
        match token {
            Ok(token) => Ok((span.start, token, span.end)),
            _ => Ok((span.start, Token::Error, span.end)),
        })
    }
}  
```

哦对，还有错误处理，可以自定义错误类型。

```rust
#[derive(Default, Debug, Clone, PartialEq)]
pub enum LexicalError {
    InvalidInteger(String), // ...
    #[default]
    UnknownToken // matches any unexpected token
}
```

---
level: 2
---
# LALRPOP
真是开了个大坑

为什么选择了 lalrpop ? <del> 一时脑抽 </del>
- 有 3.1k stars，最近有更新 -> 猜测是活跃的社区
- 需要一个能写文法的Parser
    - nom 是 combinator, 写起来像递归下降，实际上用起来也确实是, 否决
    - pest 很优雅，写起来简单，但不能自己写error handler，工作量- -，分数- -
    - lalrpop 能写文法，有error recovery，是LR(1)的，可以用外部的lexer

事实上呢，lalrpop 的文档写的很简略，很多高级用法都是看其他项目的源码才学会的。

而且lalrpop限制文法必须是LR(1)的，不支持任何二义性，precedence 功能也不好用，shift/reduce reduce/reduce conflict 的唯一解决办法就是重写文法。

用lalrpop的几k stars的大项目：不写错误处理/写的很简单，不用precedence。教训：不要相信stars

---
level: 2
---
# Quick Start
简单讲讲如何用 lalrpop

`Cargo.toml` 里面一定要这么写，来自 lalrpop 官方文档

```toml
[dependencies]
lalrpop-util = "0.22.0"

[build-dependencies]
lalrpop = { version = "0.22.0", default-features = false }
```

然后在项目根目录下创建一个`build.rs`文件

```rust
fn main() {
    lalrpop::process_src().unwrap();
}
```

最后在 `src/grammar.lalrpop` 下写你的文法，`build.rs`会先生成一个`grammar.rs`，这个是根据文法生成的LR(1)的parser，然后在`lib.rs`里面导出这个parser，或者写一个`main.rs`直接调用。

当然你也可以explicitly生成`grammar.rs`, 相信我, 你不会想去修改这个文件。

---
level: 2
---
然后是lalrpop的文法写法，其实跟写yacc/bison很像，以下文法发自我的项目

```rust
use lalrpop_util::ErrorRecovery;
use spl_lexer::tokens::{Token, LexicalError, Span};
use spl_ast::tree;

grammar<'err>(errors: &'err mut Vec<ErrorRecovery<usize, Token, LexicalError>>, source: &str);

pub Program: tree::Program = {
    <prg:ProgramPart*> => {
        tree::Program::Program(
            prg
        )
    }
}

ProgramPart: tree::ProgramPart = {
    <stmt: Stmt> => tree::ProgramPart::Statement(Box::new(stmt)),
    <func: FuncDec> => tree::ProgramPart::Function(func)
}
```

中间的`grammar`是parser对外的接口，可以<del>无限自定义</del>

别忘了定义你的AST，这里的`tree`是我们定义的AST。每个production都返回一个AST的节点。

---
level: 2
---
然后是一些繁琐的东西，导入你的Token，跟bison导入flex的token一样

```rust
extern {
    type Location = usize;
    type Error = LexicalError;

    enum Token {
        "identifier" => Token::Identifier(<String>),
    
        "int" => Token::LiteralInt(<u32>),
        "float" => Token::LiteralFloat(<f32>),
        "bool" => Token::LiteralBool(<bool>),
        "char" => Token::LiteralChar(<char>),
        "string" => Token::LiteralString(<String>),
        "typeint" => Token::TypeInt,
        // ...
    }
}
```

`extern` 就是用你的代码覆盖lalrpop的东西，高定制性！不过引用的时候一定要带双引号

```rust
Term: Box<tree::CompExpe> = {
    "+"? <n: "int"> => Box::new(tree::CompExpr::Value(tree::Value::Integer(n))),
}
```

---
level: 2
---
有一些Rust的特性，比如说强大的macro语法, `*/+/?`表示某个production可以出现0次或多次...

```rust
pub Body: tree::Body = {
    <stmt:Expr*> => tree::Body::Body(stmt)
}

Term: Box<tree::CompExpr> = {
    // ...
    <vl:@L> <ident: Identifier> "(" <args:ArgList?> ")" <vr:@R>=> {
        Box::new(tree::CompExpr::FuncCall(tree::Function::FuncReference(Box::new(ident), args.unwrap_or(Vec::new()))))
    },
}
```

下面还有一个很方便的宏，可以简化代码。比如可以简写像`a, b, c`这样的production

```rust
pub ParaDecs = Comma<ParaDec>;
StructDecs = Comma<StructDec>;
```

然后这个`Comma`宏是这样定义的

---
level: 2
---

```rust
Comma<T>: Vec<T> = {
    <l:@L> <mut v:Comma<T>> "," <e:T?> <r:@R> => match e {
        None => {
            if v.len() != 0 {
                errors.push(ErrorRecovery {
                    error: ParseError::User {
                        error: LexicalError::MissingLexeme(Span {
                            source: source.to_string(),
                            start: l,
                            end: r
                        }, "variable".to_string())
                    },
                    dropped_tokens: Vec::new(),
                });
            }
            v
        },
        Some(e) => { v.push(e); v }
    },
    <e:T?> => match e {
        Some(e) => vec![e],
        None => Vec::new()
    }
};
```

---
level: 2
---
# Error Recovery
Monad God 曾说过：开源parser错误处理很玄学

回顾一下Error Recovery的原理：parser在当前的state遇到了在表内没有action的token，就会产生一个特殊的error token，然后再通过特殊的规则来处理带有error token的production。A trivial example:

```rust
pub CompExpr: Box<tree::CompExpr> = {
    <lhs:CompExpr> "+" <l:@L> <rhs:!> <r:@R> => {
        errors.push(ErrorRecovery {
            error: ParseError::User {
                error: LexicalError::MissingLexeme(Span {
                    source: source.to_string(),
                    start: l, end: l + 1
                }, "Exp after +".to_string())
            },
            dropped_tokens: Vec::new(),
        });
        Box::new(tree::CompExpr::Error)
    },
    <l:@L> <lhs:CompExpr> "+" <rhs:CompExpr> <r:@R> => {
        Box::new(tree::CompExpr::BinaryOperation(lhs, tree::BinaryOperator::Add, rhs))
    },
}
```

---
level: 2
---
开源的parser怎么可能不翻车。。。 如果遇到了一个Invalid Token，parser直接摆烂了

```sh
// Input: 1 + $
---- grammar stdout ----
thread 'grammar' panicked at src/main.rs:168:10:
called `Result::unwrap()` on an `Err` value: InvalidToken { location: 4 }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

既然是开源的，有什么是一个Issue不能解决的呢？如果有，那就再开一个PR

<div style="float: left;">
<img src="https://chanbengz.github.io/slides/compilers-demo/lalrpop.png" width="400"/>
</div>
<div style="float: left;">
<img src="https://chanbengz.github.io/slides/compilers-demo/pr.png" width="450"/>
</div>

---
level: 2
---
# Solving Ambiguity
To shift or to reduce, that is the question.

---
level: 2
---
# Precedence
Don't know how to use it yet.

当然，重写文法太麻烦了，所以有时候还是可以偷懒用一下lalrpop的特性。

（很抱歉dangling else并不能这么解决）

```rust
pub CompExpr: Box<tree::CompExpr> = {
    #[precedence(level="0")] // Highest precedence
    Term,
    #[precedence(level="1")] #[assoc(side="left")] // Left associative
    <l:CompExpr> "*" <r:CompExpr> => l * r,
    <l:CompExpr> "/" <r:CompExpr> => l / r,
    #[precedence(level="2")] #[assoc(side="left")]
    <l:CompExpr> "+" <r:CompExpr> => l + r,
    <l:CompExpr> "-" <r:CompExpr> => l - r,
};
```

