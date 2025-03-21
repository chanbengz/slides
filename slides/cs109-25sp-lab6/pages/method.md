---
layout: section
---
# Methods

---
level: 2
---
# Non-Static Methods
只能通过实例对象调用

Static Methods: 不使用任何<span v-mark.red=0>会变</span>的东西, 比如实例变量/非静态方法

Dynamic Methods: 使用了实例变量/非静态方法, 比如调用了`this.func()`

<v-switch>

<template #0>

```java
public class Foo {
    String bar;
    public void dynamicMethod() {
        this.bar = "Hello";
        this.printBar();
    }

    public void printBar() {
        System.out.println(this.bar);
    }

    public static void staticMethod() {
        // System.out.println(this.bar); 静态方法不能直接调用
        // this.dynamicMethod();         静态方法不能直接调用
    }
}
```

</template>

<template #1>

```java
public class Dog {
    String breed, color;
    int age;

    static void bark(Dog dog) { 
        if (dog.breed.equals("Labrador")) {
            System.out.println("Woof");
        } else {
            System.out.println("Yap");
        }
    }
}
```

</template>
</v-switch>

---
level: 2
---
# Constructors
创世纪之初

有一些时候, 你希望(private)变量只赋值一次, 或者执行某些操作 (比如计数)

这个意思是, 有些代码你可以在创建对象的时候执行.
一定与<span v-mark.red=0>类名相同, 没有返回值</span>

```java
public class User { 
    static int ustCnt = 0;
    private String name, password;

    public User(String name, String password) {
        this.name = name;
        this.password = password;
        ustCnt++;
    }
}

public class Main {
    public static void main(String[] args) {
        User user = new User("Ben", "Bigben123");
    }
}
```

---
level: 2
---
# Setter & Getter
**不知道有什么用

一句话: 用来设置/获取私有变量的值. 那为什么不直接用`public`赋值呢?

答案很简单, 因为取值赋值的过程可能会有很多验证, 计算等操作, 比如

```java
public class User {
    private String password;
    public boolean validatePassword(String password) {
        return this.password.equals(password);
    }
    public void setPassword(String newPassword, String oldPassword) {
        if (validatePassword(oldPassword)) {
            this.password = newPassword;
        }
    }
}
```

有些时候, 你不希望外部能直接访问一些内部使用的变量

---
level: 2
---
# toString
一个很特殊的方法, 所有类都有

当你将一个对象传递给`System.out.println()`时, 会自动调用`toString()`方法

```java
System.out.println(new User("Ben", ""));
String.format("%s", new User("Ben", ""));
// User@1f5e2e1
```

你可以重写这个方法, 以便更好的展示对象的信息

```java
public class User {
    public String toString() {
        return this.name + "@User";
    }
}
// Ben@User
```
