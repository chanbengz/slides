---
theme: seriph
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
overviewSnapshots: true
background: images/ferris.jpg
---
# Rust: Title
<br>
肖翊成，朱家润，陈贲

---
layout: two-cols
layoutClass: gap-16
---
# Table of contents

Things we will cover

::right::

<Toc v-click minDepth="1" maxDepth="2"></Toc>

---
layout: section
---
# Functional Programming in Rust

Expression, Pattern Matching, Closure, Iterator and Monad

---
level: 2
---
# FP Basic

What is functional programming?

<div grid="~ cols-2 gap-2" m="t-2">

<v-clicks depth=2>

- Expression and Statement are different
    - Expression returns a value `114 + 514`
    - Statement has no value `let x = 114 + 514;`

```rust
fn add(a: i32, b: i32) -> i32 {
    let c = a + b;
    c  // or `return c;`
}
```
</v-clicks>

<v-clicks depth=2>

- Variables are **IMMUTABLE**: Rust supports mutable `mut` but immutable is default, so half-functional

```rust
let x = 114;
x = 514;  // Error: cannot assign twice to immutable variable
let mut y = 114;
y = 514;  // OK
```
</v-clicks>

<v-clicks depth=2>

- Function is first-class citizen: can be passed as argument, returned as value, assigned to variable
    - Currying supported!
- And `map`, `filter`, `fold`, `zip` are implemented

```rust
fn curry_add(x: i32) -> impl Fn(i32) -> i32 {
    move |y| x + y
}

fn main() {
    let add_nine = curry_add(9);
    let result = add_nine(1);
    println!("Result: {}", result); // 10
}
```
</v-clicks>

</div>

---
level: 2
---
# Pattern Matching
Throw away if-else

---
layout: center
---
# Thank you!

