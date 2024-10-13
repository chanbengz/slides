---
layout: section
---
# Intro

---
layout: two-cols
level: 2
---
# What is Rust?

<v-click> 
<li> 🦀 High Security </li>
<li> ⚡ High Performance </li>
</v-click>

<v-click>

<br>

Kills the Bugs $\rightarrow$ Kills the Programmers who write Bugs.

</v-click>
<v-click> 
<li> Default Immutable </li>
<li> Ownership </li>
<li> Lifetimes </li>
</v-click>

::right::

<v-click>


<div align=center>
<img src="/public/fRustacean.webp" width="300"/>
</div>

</v-click>

<br>

<v-click>

<div align=center>
<img src="/public/writing_about_rust.jpg" width="300"/>
</div>

</v-click>

---
level: 2
---
# Default Immutable

````md magic-move {lines: true}
```c
#include<stdio.h>

int main() {
    int a = 5;
    a += 1;
    printf("%d", a)；

    return 0；
}
```

```rust
fn main() {
    let a = 5;
    a += 1;
    print!("{}", a);
}
```

```rust
fn main() {
    let a = 5;
    a += 1;
    print!("{}", a);
}
```

```rust
fn main() {
    let mut a = 5;
    a += 1;
    print!("{}", a);
}
```
````

<div v-click="2">

```rust
error[E0382]: borrow of moved value: `a`
 --> src/main.rs:4:18
  |
2 |     let a = String::from("Hello, World! ");
  |         - move occurs because `a` has type `String`, which does not implement the `Copy` trait
3 |     let b = a;
  |             - value moved here
4 |     print!("{}", a);
  |                  ^ value borrowed here after move
  |
  = note: this error originates in the macro `$crate::format_args` which comes from the expansion of the macro `print`
(in Nightly builds, run with -Z macro-backtrace for more info)
help: consider cloning the value if the performance cost is acceptable
  |
3 |     let b = a.clone();
  |              ++++++++

For more information about this error, try `rustc --explain E0382`.
```

</div>

---
level: 2
---
# Ownership


````md magic-move 
```c
#include <stdio.h>

int main()
{
   char* a = "Hello, World! ";
   char* b = a;
   printf("%s", a);
   printf("%s", b);

   return 0;
}
```

```rust
fn main() {
    let a = String::from("Hello, World! ");
    let b = a;
    print!("{}", a);
    print!("{}", b);
}
```

```rust
fn main() {
    let a = String::from("Hello, World! ");
    let b = a;
    print!("{}", a);
    print!("{}", b);
}
```

```rust
fn main() {
    let a = String::from("Hello, World! ");
    let b = &a;
    print!("{}", a);
    print!("{}", b);
}
```
````

<div v-click="2">

```rust
error[E0382]: borrow of moved value: `a`
 --> src/main.rs:4:18
  |
2 |     let a = String::from("Hello, World! ");
  |         - move occurs because `a` has type `String`, which does not implement the `Copy` trait
3 |     let b = a;
  |             - value moved here
4 |     print!("{}", a);
  |                  ^ value borrowed here after move
  |
  = note: this error originates in the macro `$crate::format_args` which comes from the expansion of the macro `print` (in Nightly builds, run with -Z macro-backtrace for more info)
help: consider cloning the value if the performance cost is acceptable
  |
3 |     let b = a.clone();
  |              ++++++++

For more information about this error, try `rustc --explain E0382`.
```

</div>
