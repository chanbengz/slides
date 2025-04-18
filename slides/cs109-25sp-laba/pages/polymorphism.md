---
layout: section
---
# Polymorphism

---
level: 2
---
# What is polymorphism?
A could be B but B could not be A.

多态(polymorphism)是指同一个概念在不同的环境(context)下, 可以有不同的解释和不同的执行结果.
- 比如说在小学数学中, `+`表示加法, 在集合论中, `+`表示并集, 在图论中, `+`表示连接.
- `+` 这个操作在不同的意义下有不同的解释和不同的执行结果.
- 这个特性有一个好处: 你可以做更复杂的抽象并保持灵活性, 因为一个方法可以有不同的实现.

<div v-click="1">

A little quiz here. Which one is correct?

```java
// class Cat extends Animal
Cat cathy = new Animal();
Animal a = new Cat();
```

</div>

<div v-click="2">

答案是: `Animal a = new Cat();` But why?

</div>

---
level: 2
---
# A Trivial Example

<v-switch>

<template #0>

假如我们定义了一个类`Animal`表示动物，`Cat`表示猫，`Dog`表示狗。

```java
abstract class Animal {  
    abstract void eat();  
}  
  
class Cat extends Animal {  
    public void eat()  { System.out.println("吃鱼");  }
    public void work() { System.out.println("抓老鼠");}
}  
  
class Dog extends Animal {  
    public void eat()  { System.out.println("吃骨头");}
    public void work() { System.out.println("看家"); }
}
```

很显然, `Cat`和`Dog`都可以是`Animal`, 但是`Animal`不能是`Cat`或`Dog`.

因为`extends`表示有属性(attribute)和方法(method)的拓展, 这些拓展的部分不是`Animal`所拥有的.

</template>

<template #1>

```java
public class Main {
    public static void main(String[] args) {
        show(new Cat());  // 以 Cat 对象调用 show 方法
        show(new Dog());  // 以 Dog 对象调用 show 方法
                
        Animal a = new Cat();  // 向上转型  
        a.eat();               // 调用的是 Cat 的 eat
        Cat c = (Cat) a;       // 向下转型  
        c.work();              // 调用的是 Cat 的 work
    }
            
    public static void show(Animal a)  {
        a.eat();  
        if (a instanceof Cat) {
            Cat c = (Cat) a;  
            c.work();  
        } else if (a instanceof Dog) {
            Dog c = (Dog) a;  
            c.work();  
        }  
    }  
}

```

</template>

</v-switch>

---
level: 2
---
# Look Deeper
Runtime vs. Compile-time and Dynamic binding

Compile-time(编译时)表示`javac`在看到你的代码时能确定的状态。
- 假如你是编译器, 你能从代码中读出什么信息?

<div v-click="1">

Runtime(运行时)表示代码中必须要运行时才能确定的状态。

```java
Animal a = null;
String s = input.nextLine(); // where the chaos begins
if (s.equals("cat")) {
    a = new Cat();
} else if (s.equals("dog")) {
    a = new Dog();
} // what "a" should be if you are here?
```

</div>

<div v-click="2">

Dynamic binding and Virtual method(动态绑定和虚方法)一个抽象方法有不同的实现, 在运行中才会确定具体调用哪个实现.
重写(override)跟重载(overload)是不同的. 类的变量(attribute)和静态属性不受动态绑定的影响.

```java
a.eat(); // Animal.eat() -> Cat.eat()
```

</div>

---
level: 2
---
# Quiz Again!

```java
public class Main {
    public static void main(String[] args) {
        Super sup = new Sub();
        System.out.println(sup.a); // 100, ez
        System.out.println(sup.b); // 100, ez
        System.out.println(sup.aAdd()); // 101, why?
        System.out.println(sup.bAdd()); // 201, why?
    }
}

class Super {
    static int a = 100;
    int b = 100;
    public static int aAdd() { return ++a; }
    public int bAdd() { return ++b; }
}

class Sub extends Super {
    static int a = 200;
    int b = 200;
    public static int aAdd() { return ++a; }
    public int bAdd() { return ++b; }
}
```
