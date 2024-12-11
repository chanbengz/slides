---
layout: section
---
# AST

---
level: 2
---
# Abstract Syntax Tree
持续挖坑，，，鬼知道我们改了多少次AST，，，

LALRPOP生成的AST是一个枚举类型，每个枚举值代表了一个语法规则的产生式。我们可以通过这个AST来进行语义分析。

```rust
#[derive(Clone, Debug, PartialEq)]
pub enum Program{
    Program(Vec<ProgramPart>),
    Error
}

#[derive(Clone, Debug, PartialEq)]
pub enum ProgramPart {
    Statement(Box<Statement>),
    Function(Box<Function>),
}

...
```

---
level: 2
---
这样的语法树结构是一个递归的结构，每个节点都是一个枚举类型，每个枚举值都是一个AST节点。并且可以根据我们的需要存储一些额外的信息，比如变量的类型，变量的值等等。

```rust
#[derive(Clone, Debug, PartialEq)]
pub enum Value {
    Integer(u32),
    Float(f32),
    String(String),
    Char(char),
    Bool(bool),
    Struct(String),
    Null
}
```

---
level: 2
---
也可以用来存储一些语法树的位置信息，比如在源代码中的定位信息。

```rust
#[derive(Clone, Debug, PartialEq)]
pub enum Expr{
    If(If, Span),
    Loop(Loop, Span),
    VarManagement(Vec<Variable>, Span),
    FuncCall(Function, Span),
    Body(Body, Span),
    Break(Span),
    Continue(Span),
    Return(CompExpr, Span),
    Error
}
```
这种语法树结构的好处是很清晰，每个节点都是一个枚举类型，可以根据不同的枚举值来进行不同的操作。但是坏处也很明显，我们每次需要获取到语法树结构中的某个节点的时候，都需要从上而下遍历一次AST，这样实在看起来不是很优雅。


---
level: 2
---
# Strong Typing
又挖了一个大坑

正是有了AST的强力支持，我们的语法Parsing得以支持强类型检查，理论上来讲，这种做法可以大大减少我们语义分析的工作量。<del>但出于各种原因我们还是当了一回怨种重新写了一个语义分析</del>

我们做的第一个限制，if/while里面的条件必须是bool类型

```
CondExpr -> CondTerm | CompExpr OpEqual CompExpr | CompExpr OpNotEqual CompExpr | CompExpr OpLessThan CompExpr
    // ...
    CondExpr OpEqual CondExpr | CondExpr OpAnd CondExpr | CondExpr OpOr CondExpr | OpNot CondExpr

CondTerm -> LiteralBool | LeftParen CondExpr RightParen
```

第二个限制，左值一定是变量。我们写了一个production专门处理变量的声明和赋值。缺点就是我们不支持这种`a = b = 1`的赋值，也不支持`1 + 2;`这种无意义的statement, i.e., 我们的stmt没有返回值, 不是形如`Stmt -> Expr SEMI`这种语法。

```
VarManagement -> VarDecs | VarAssigns
VarDecs -> Specifier VarDecList
VarAssigns -> VarAssign COMMA VarAssigns | VarAssign
VarAssign -> Identifier AssignOp CompExpr // +
```

---
level: 2
---

第三个限制，我们在语法分析的过程当中就把Struct类型和普通类型区分开了，Struct的定义和赋值与普通变量不同。同时，在Function的定义和调用的时候使用的也会构建不同的AST节点。

```rust
#[derive(Clone, Debug, PartialEq)]
pub enum Variable {
    // VarAssignment allows only for VarReference and StructReference.
    VarAssignment(Box<Variable>, Box<CompExpr>),

    // Variable can be a single value or an array.
    VarReference(Box<String>, Box<Vec<CompExpr>>), // varname, offsets
    VarDeclaration(Box<String>, Box<Value>, Box<Vec<CompExpr>>), // varname, type, offsets
    
    // Struct definition and declaration
    StructDefinition(Box<String>, Box<Vec<Variable>>),
    StructDeclaration(Box<String>, Box<String>, Box<Vec<CompExpr>>),
    StructReference(Box<Vec<Variable>>),
    
    // Function Parameter
    FormalParameter(Box<String>, Box<Value>, Box<Vec<usize>>),
    Error
}
```

我们把一个变量的声明、赋值、引用、结构体的定义、结构体的引用、函数的定义、函数的调用等等都放在了Variable这个枚举类型中，这样我们可以根据不同的枚举值来进行不同的操作。