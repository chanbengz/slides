---
layout: section
---
# Semantic Analysis

---
level: 2
---

语义分析最主要的两个模块可以划分为：<bold>Symbol Table符号表的创建</bold>以及<bold>Type Checking类型检查</bold>。这两者在理论上其实在我们使用Lalrpop做语法分析的过程中便可以直接完成，但是出于以下两个原因，我们还是把语义分析单独拿了出来

<v-click>

- 协同上并不方便，如果两个部分交给了不同的人的话，merging是很痛苦的事情，不如划分成两个独立的模块
- 语法分析的Error Handling的工作是一件非常Trivial的工作，而语义分析更具系统性，在代码整齐度上面，把两个模块分开也更便于我们对代码进行管理

</v-click>

<v-click>

优点上面已经说了，那么缺点在于什么呢？

- 目前来说，我们需要在语义分析里面单独地遍历一次AST，这样就会有一定的效率损失
- 同时，如果我们的语法树重构了的话，随之而来的，我们的语义分析模块也要大改...这样就会带来一定（不少）的工作量<del>（不要问工作量怎么统计的，问就是亲测）</del>.

</v-click>

---
level: 2
---
# Symbol Table
LinkedList退退退

<v-switch>
<template #1>
Symbol Table有两种方式来维护不同Scope的变量，一种是使用HashMap + Orthogonal List，另一种是使用Stack + HashMap。我们选择了后者，因为它更加直观，同时也更加容易实现。<del>绝对不是因为Rust的语言特性会导致HashMap里面内容的所有权非常难以管理</del> 
</template>
<template #2>
Symbol Table的问题在于，我们什么时候需要创建一个新的Scope，什么时候需要销毁一个Scope，而使用了强类型的AST后，我们可以直接识别Body节点，这样就可以很方便地进行Scope的管理。

```rust
// AST
#[derive(Clone, Debug, PartialEq)]
pub enum Body {
    Body(Vec<Expr>)
}
// Semantic Analysis
fn traverse_body(&mut self, body: &Body) {
    match body {
        Body::Body(exprs) => {
            self.symbol_tables.extend_scope(); 
            for expr in exprs {
                self.traverse_expr(expr);
            }
            self.symbol_tables.exit_scope().map_err(
                |e| self.errors.add_error(e)
            ).ok()?;
        }
    }
}
```
</template>
</v-switch>

---
level: 2
---
# Type Checking
你知道的，有时候设计了再多最后还是会写出史山的。

我们实现的Type Checking参考了高中同学的一点结构上的思路（具体请参考 [SysYc编译器实现](https://github.com/rrvm-project/SysYc/blob/main/frontend/typer/src/visitor.rs) ），我们有两个主要的模块，一个是Walker，一个是Typer，在设计支出，是希望将Walker的功能尽量地简化，只负责遍历AST，而Typer则负责具体的类型检查工作。<del>但在Work起来的时候发现有些东西没有办法使用Typer来实现，需要Walker来对接Symbol Table</del>

```rust
// VarType
#[derive(Clone, Debug)]
pub enum VarType {
	Primitive(PrimType),
	Array(ArrayType)
}

pub type PrimType = BasicType;
pub type ArrayType = (BasicType, Vec<usize>);
pub type StructType = (String, Vec<(String, VarType)>);

pub enum BasicType { // Matching the type Value in AST
	Int,
    ...
}
```

---
level: 2
---
Typer最大一部分的作用是用来缩短Walker的复杂代码，让遍历的逻辑看起来更流畅，把一些类型检查的杂活单独拎了出来。但是在某些地方也会很有作用，比如用于存储某个Scope下Function的返回值类型，这样我们就可以在Function的调用的时候，直接检查返回值类型是否匹配。
<v-switch>
<template #1>
```rust
// This is used to check the type (if it is an array) and the indices.
pub fn check_type(&self, var_type: VarType, reference: &Vec<usize>) -> Result<VarType, SemanticError> {
    match var_type {
        VarType::Array((basic_type, dims)) => {
            let num_indices = reference.len();
            let num_dims = dims.len();
            if num_indices > num_dims {
                // Error Handling
            } else if num_indices == num_dims {d
                return Ok(VarType::Primitive(basic_type));
            } else {
                let remaining_dims = dims[num_indices..].to_vec();
                return Ok(VarType::Array((basic_type, remaining_dims)));
            }
        },
        VarType::Primitive(basic_type) => {
            return Ok(VarType::Primitive(basic_type));
        }
    }
}
```
</template>
<template #2>
```rust
pub fn check_ret_type(&self, type_t: BasicType) -> Result<(), SemanticError>{
    if type_t == self.func_ret_type { // When enter a function scope, set the return type, 
    // then you can check the return type whenever you traverse a return statement
        Ok(())
    } else {
        // Error Handling
    }
}
```
</template>
</v-switch>
---
level: 2
---
# Error Handling
这个Error我是见过的...

Rust的Result机制能让我们更好地进行错误管理，我们在语义分析的过程中，会遇到很多错误，比如变量未定义，变量重定义，类型不匹配等等。我们会把这些错误都放在SemanticError里面，然后在遍历AST的过程中，如果遇到了错误，我们就会把这个错误存放到ErrorManager里面，ErrorManager的作用就是根据遍历的位置来更新错误的位置信息，在存储错误的时候标记错误的位置。使用thiserror的包会帮助你更好地使用Error。

<v-switch>
<template #1>
```rust
use thiserror::Error;
pub enum SemanticError {
	#[error("[Semantic Error] Type Mismatch Error[{id:?}] at line {line:?}: {message:?}")]
	TypeError{
        id: usize,
        message: String,
        line: usize,
    },
	#[error("[Semantic Error] Undefined Reference Error[{id:?}] at line {line:?}: {variable:?} undefined.")]
	ReferenceError{
        id: usize,
        variable: String,
        line: usize,
    },
    ...
}
```
</template>
<template #2>

```rust
pub struct SemanticErrorManager {
    cnt: usize,
    errors: Vec<SemanticError>,
    line: usize, 
    // When traversing a node that contains location information, update the line number using Span. 
    // The locality of errors might not be precise, but it is enough for debugging.
}
```
</template>
</v-switch>