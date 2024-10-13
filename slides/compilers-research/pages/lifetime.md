---
layout: section
---
# Ownership and Lifetime in Rust
Using this, we try to demonstrate how static analysis works.

---
level: 2
---
# Preface
Good prgrammers select language, while great language selects programmers.

**Ownership** and **lifetimes** are core concepts in the Rust language. In fact, these concepts exist not only in Rust but also in C/C++. Nearly all memory safety issues stem from the incorrect use of ownership and lifetimes. Any programming language that does not use garbage collection to manage memory faces these challenges. 

<v-click>
However, Rust <span v-mark.underline.orange="1">explicitly</span> defines these concepts at the language level and provides language features that allow users to explicitly <span v-mark.underline.orange="1">control ownership transfer</span> and <span v-mark.underline.orange="1">declare lifetimes</span>.  Additionally, the compiler checks for various misuse errors, enhancing the program's memory safety.
</v-click>
<br>
<br>
<v-click>
I believe that these concepts are still way too abstract to understand. But I also believe most of us encounter problems like <code>segmentation fault</code>, <code>dangling pointer</code>, <code>buffer overflow</code>, etc. in our daily programming. These problems are all related to memory safety.
</v-click>

---
level: 2
---
# A Simple Example
Write you a Rust for safe

**Ownership** means control over a memory region associated with a variable. This region can exist in various memory locations (like heap, stack, or code segment). In high-level languages, accessing these memory regions typically requires associating them with variables, unlike in low-level languages where direct access is possible.

````md magic-move {lines: true}
```cpp {1-6,8-9|7}
#include <iostream>
using namespace std;

int main() {
    int values[3]= { 1,2,3 };
    cout<<values[0]<<","<<values[3]<<endl;
    // Buffer overflow
    return 0;
}
```

```cpp {1-11,14-15|12-13}
#include <iostream>
using namespace std;

struct Point { int x; int y; };

Point* newPoint(int x,int y) {
    Point p { .x=x, .y=y };  return &p;
}

int main() {
    Point *p = (Point*)malloc(sizeof(Point));
    cout<<p->x<<","<<p->y<<endl;
    // Segmentation fault
    return 0;
}
```

```cpp {1-11,14-15|12-13}
#include <iostream>
using namespace std;

struct Point { int x; int y; };

Point* newPoint(int x,int y) {
    Point p { .x=x, .y=y };  return &p;
}

int main() {
    Point p = NULL;
    cout<<p->x<<endl;
    // Dereferencing null pointer
    return 0;
}
```

```cpp {1-11,14-15|12-13}
#include <iostream>
using namespace std;

struct Point { int x; int y; };

Point* newPoint(int x,int y) {
    Point p { .x=x, .y=y };  return &p;
}

int main() {
    Point *p = newPoint(10,10);
    delete p;
    // Dangling pointer and Invalid memory deallocation
    return 0;
}
```
````

---
level: 2
---
# How do Rust cope with such problems?
From C++ to Rust

In Rust, the **ownership** concept comes with binding instead of assigning likewise in C++.

````md magic-move {lines: true}
```cpp
#include <iostream>

int main()
{
    int a = 1;
    std::cout << &a << std::endl;   /* 0x62fe1c */
    a = 2;
    std::cout << &a << std::endl;   /* 0x62fe1c */
}
// In this case, the address of `a` remains the same. We are assigning values to `a`.
```
    
```rust
fn main() {
    let a = 1; // Creating an immutable binding
    println!("a:{}",a);     /* 1 */
    println!("&a:{:p}",&a); /* 0x9cf974 */
    a = 2;                /* Error: cannot assign twice to immutable variable `a` */
    let a = 2;              /* Rebinding */
    println!("&a:{:p}",&a); /* 0x9cf9a4 */ 
}
// In this case, the address of `a` changes.
// An immutable binding means that a variable is bound to a memory address and given ownership.
```

```rust {*|2}
fn main() {
    let mut b = 1; // Creating a mutable binding
    println!("b:{}",b);     /* 1 */
    println!("&b:{:p}",&b); /* 0x9cfa6c */
    b = 2;
    println!("b:{}",b);     /* 2 */
    println!("&b:{:p}",&b); /* 0x9cfa6c */
    let b = 2;              /* Rebinding */
    println!("&b:{:p}",&b); /* 0x9cfba4 */
}
// In this case, the address of `b` remains the same.
// A mutable binding allows the variable to modify the data in the associated memory region.
```
````
<v-click>
<b>Assignment</b> is the act of writing a value into the memory region associated with a variable, while <b>binding</b> is the process of establishing the relationship between a variable and a memory region. In Rust, this also involves transferring ownership of that memory region to the variable.
</v-click>
