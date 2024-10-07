---
layout: section
---
# Functional Programming in Rust

Expression, Pattern Matching, Closure, Iterator and Monad

---
level: 2
---
# FP 101

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

- Variables are **IMMUTABLE**
    - Rust supports mutable `mut` but immutable is default, so half-functional

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
    let sum = v.into_iter().reduce(|a, b| a + b);
    assert_eq!(Some(15), sum);
    ```

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
# Type Classes
No interfaces, no abstract classes

In pure functional programming language, like Haskell, type classes is must-have feature.

````md magic-move {lines: true}
```haskell
add_int :: Int -> Int -> Int
add_int x y = x + y

add_float :: Float -> Float -> Float
add_float x y = x + y

... -- So dump, so hard
```

```haskell
data Num = Int | Float | Double | ... -- Integers, Decimals are all numbers

class Num a where
    add :: a -> a -> a
    add x y = x + y
```
````
<br>
<v-click>
And you can define a Adhoc Polymorphism

```haskell
class Num a where
    showType :: a -> String

instance Num Int where
    showType _ = "Int"

instance Num Float where
    showType _ = "Float"

...
```
</v-click>

---
level: 2
---
# Type Classes: Generics & Trait
Rust is not a OOP language!

Similarly, Rust has the generics feature

````md magic-move {lines: true}
```rust
fn add_i8(a:i8, b:i8) -> i8 {
    a + b
}
fn add_i32(a:i32, b:i32) -> i32 {
    a + b
}
fn add_f64(a:f64, b:f64) -> f64 {
    a + b
}
```

```rust
fn add<T>(a:T, b:T) -> T {
    a + b
}
```

```rust
// Contrain T to Add trait
fn add<T: std::ops::Add<Output = T>>(a:T, b:T) -> T {
    a + b
}
```
````
<br>
<v-click>
And you can define a Adhoc Polymorphism in rust with `trait`

```rust
pub trait Num {
    fn show_type(&self) -> String;
}

impl Num for i32 {
    fn show_type(&self) -> String {
        format!("i32: {}", self)
    }
}
```

[Read More](https://course.rs/basic/trait/trait.html)
</v-click>

---
level: 2
---
# Back to FP 101
Functor, Applicative and Monad


---
level: 2
---
# Monad: Option & Result


---
level: 2
---
# Pattern Matching
Throw away if-else

