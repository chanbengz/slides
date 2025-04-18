---
layout: section
---
# Abstract Class

---
level: 2
---
# Why being abstract?

<v-clicks>

- Interface 只能定义方法的签名, 不能有任何实现, 这有一点不太方便.
- 我们希望剥离一些属性和方法让子类继承, 但是又不希望被实例化 -> 强制要求继承
    - 这一点通常是因为父类的方法的实现是不重要的, 即我们不需要一个默认的实现.
- 也就是说, 我们需要一个
    - 只定义方法的签名, 这个在`abstract class`中称为`abstract method`
    - 但是又有一些已实现的方法和属性的类.

</v-clicks>

<div v-click="4">

```java
abstract class Animal {
    // abstract method
    public abstract void makeSound();

    // concrete method
    public void eat() {
        System.out.println("Animal is eating");
    }
}
```

```java
Animal a = new Animal(); // Error: Cannot instantiate the abstract class "Animal"
```

</div>

---
level: 2
---
# Something to note...
不用我说大家也知道了

- `abstract class` 与 `class` 最大的区别是前者必须被继承, 不能被实例化
    - 一个小的区别是只有 `abstract class` 能有`abstract method`
- `abstract class` 可以没有`abstract method`, 这个不是强制要求的
- `abstract method` 强制子类必须实现的, 跟Interface一样
- `abstract class` 可以继承`abstract class`
- `constructor` 和 `static method` 不能是`abstract`
- `implements` 可以有多个, 但是`extends`只能有一个
    - 用`abstract class` 还是 `interface` 要根据需求考虑清楚
- `interface` 没有 `constructor`, 因为不是class也没有变量, 所有的方法都是`abstract method`
