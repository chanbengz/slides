---
layout: section
---
# Classes

---
level: 2
---
# Abstraction & Meta
Software Engineering

Abstraction, 即抽象, 原意是指将事物的共性质提取出来, 从而形成概念的过程.

在CS中, 这个指忽略对象的具体细节, 只关注对象的行为和属性, 从而使得程序更易于理解和维护.

```java
int[] a = {8, 21, 17, 6, 12, 3, 9, 7};
Arrays.sort(a); // who cares how it's done?
```

我们总是假定class的实现是正确的 ~~, 错了再说.~~

<div v-click=1>

Meta, 即元, 原意是指事物的本质, 事物的基本属性. 在这里并不是指Template (Metaprogramming), 而是说写class的时候要从更高维度思考你的代码架构.
但这要求你能抽象地想象出你的代码会怎么执行, 数据会怎么流动。

```java
public class NewWorld {
    private life[] creatures;
    // ...
    void run_one_planck_time() { /* ... */ }
}
```

</div>

---
level: 2
---
# Procedure-Oriented or Object-Oriented?
Both are not necessary

面向过程, 程序的执行过程是一系列调用. 通常是为了消除重复代码, 简化代码结构.

```java
static void funcA() { /* ... */ }
static void funcB() { /* ... */ }
public static void main(String[] args) {
    funcA();
    funcB();
}
```

面向对象, 对象是程序的<a href="https://steve-yegge.blogspot.com/2006/03/execution-in-kingdom-of-nouns.html">一等公民</a>,
所有的操作都是对象的操作, 通过对象之间的交互来实现程序的功能.

```java
class A { void funcA() { /* ... */ } }
class B { void funcB() { /* ... */ } }
public static void main(String[] args) {
    A a = new A(); B b = new B();
    a.funcA();
    b.funcB();
}
```

<div v-click=1>

没了这俩代码还能继续写, 只是会痛苦一点.  你说递归怎么写? 模拟呗

</div>

---
layout: image-right
level: 2
image: https://github.com/chanbengz/slides/blob/main/slides/cs109-25sp-lab6/public/BOTTOM.png?raw=true
---
# BottomUp or TopDown
That's the question

大家写Project的时候都会有一个问题, 是先写底层还是先写顶层?
我的建议
1. Top-Down, 画框架图, 跟队友约定一些接口
2. Bottom-Up, 先写底层的类, 然后再写顶层的类
3. Top-Down, 写完之后测试跟踪代码的执行流程

<div v-click=1>
<img src="https://github.com/chanbengz/slides/blob/main/slides/cs109-25sp-lab6/public/TOP.png?raw=true" width="300" align="middle">
</div>

---
level: 2
---
# Public & Private Members
控制访问权限

Public, 任何class外部的代码都可以访问. Private, 只有class内部的代码可以访问.

````md magic-move {lines: true}
```java
public class A {
    public int x;
    public void func() { /* ... */ }
}

public void main(String[] args) {
    A a = new A();
    a.x = 5;
    a.func();
}
```

```java
public class A {
    private int x;
    private void func() { /* ... */ }
}

public void main(String[] args) {
    A a = new A();
    a.x = 5;  // error, x is private
    a.func(); // error, func is private
}
```
````

<div v-click=2>

有什么用? 保护数据, 防止外部代码随意修改. 保护接口, 防止外部代码随意调用.

```java
public class User{
    private String password;
    private void verifyPassword(String input) { /* ... */ }
}
```

</div>

---
level: 2
---
# Class as a File
一个非常极端的OOP

Java 中一个`public class`对应一个文件, 不带`public`的类外部无法使用.

为什么这样? 因为文件结构即代码结构, 比如我们调用在其他代码文件里写好的类, 只需要`import`这个类

```java
/* com/
 *  └── example/
 *      └── MyClass.java
 */

import com.example.MyClass;

public class Main {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
    }
}
```

Main 永远不是必须的. 你可以写一个library, 然后别人import你的代码.
