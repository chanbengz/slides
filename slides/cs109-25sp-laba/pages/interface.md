---
layout: section
---
# Interface

---
level: 2
---
# Type Class
Ad hoc polymorphism

一个很简单的例子: 假如用户想要一个能判断任意对象相等的函数, 即类似`.equals()`的函数

如果是基本类型, 你可以直接用`==`, 但是如果是对象呢？而且你没办法重写方法, 因为这样太苦力了
- 有没有更优雅的办法呢? 这就需要引入一个新的概念: Type Class
- 它告诉你任何对象都可以有一个共同的type, 这里暂且叫做`Eq`. 也就是说所有的对象都在`Eq`这个type class里

```haskell
type Eq a where
    equals :: a -> a -> Bool
```

(上面的写法有点抽象) 成为这个type class的成员, 你只需要实现`equals`这个方法

```java
class Person {
    int age;
    public boolean equals(Object o) {
        if (o instanceof Person) {
            Person p = (Person) o; return this.age == p.age;
        } return false;
    }
}
```

---
level: 2
---
# Interface
Interface is NOT class

Interface(接口) 不是类(class), 因为它只要求你实现某些方法, 而不要求你继承某个类. 

```java
interface Eq { boolean equals(Object o); }
interface Comparable { int compareTo(Object o); }
```

因为只需要实现方法, 你可以实现多个接口.

```java
class Person implements Eq, Comparable {
    int age;
    public boolean equals(Object o) {
        if (o instanceof Person) {
            Person p = (Person) o; return this.age == p.age;
        } return false;
    }
    public int compareTo(Object o) {
        if (o instanceof Person) {
            Person p = (Person) o; return this.age - p.age;
        } return 0;
    }
}
```

---
level: 2
---
# Flexibility
Do something elegant

有了interface, 你可以写一些很灵活的代码, 比如

```java
public static boolean isEqual(Eq a, Eq b) {
    return a.equals(b); // interface ensures that "equals" exists
}
```

然后你就可以实现这个功能, 而无需辛苦地用重载去枚举每一个`isEqual`

```java
Eq a = new Integer(1), b = new Integer(2);
System.out.println(isEqual(a, b)); // false
Eq p1 = new Person(1), p2 = new Person(2);
System.out.println(isEqual(p1, p2)); // false
```

当你要实现一个新的类, 只需要实现`Eq`接口, 就可以直接用`isEqual`了

你可以在interface里声明变量, 但必须是`static final`, 实际上这就是常量

~~聪明的小朋友会发现这个也是一种多态~~
