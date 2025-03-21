---
layout: section
---
# Objects

---
layout: image-right
level: 2
image: https://www.runoob.com/wp-content/uploads/2013/12/object-class.jpg
---
# Initialization
现实中对象不能new

把class当作模板，new出来的对象是实例. 

`this` 指当前object的内部变量/方法

````md magic-move
```java
public class Dog {
    String breed, color;
    int age;
    void bark() { System.out.println("Woof"); }
}

public static void main(String[] args) {
    Dog myDog = new Dog();
    myDog.breed = "Labrador";
    myDog.bark();
}
```

```java
public class Dog {
    String breed, color;
    int age;
    void bark() { 
        if (this.breed.equals("Labrador")) {
            System.out.println("Woof");
        } else {
            System.out.println("Yap");
        }
    }
}

public static void main(String[] args) {
    Dog myDog = new Dog();
    myDog.breed = "Labrador";
    myDog.bark();
}
```
````

---
level: 2
---
# NULL Reference
Nobody likes it

如果你不初始化一个对象，它的值就是null

```java
Dog myDog; // myDog = null
myDog.bark(); // NullPointerException
```

Null, 即"空"，是一个特殊的值，表示没有对象. 相当于没有指向任何对象的引用.

访问一个null对象的属性或方法会导致NullPointerException. 这么做会安全一点

```java
if (myDog != null) myDog.bark();
```

如何删除一个对象? 让它变成null就行了

```java
myDog = null; // it's gone :(
```

---
level: 2
---
# Shallow & Deep Copy
就是lab4讲到的引用

浅拷贝: 两个对象的引用指向同一个对象 (一般不推荐, 除非你知道你在干什么)

```java
Dog myDog = new Dog();
Dog yourDog = myDog; // my dog is your dog
```

深拷贝: 两个对象的引用指向不同的对象, 会复制对象的所有属性(变量)

```java
Dog myDog = new Dog();
Dog yourDog = myDog.clone(); // my dog is not your dog, but they share the same breed, age and color

public class Dog implements Cloneable {
    String breed, color;
    int age;
    public Dog clone() {
        Dog dog = new Dog();
        dog.breed = this.breed;
        dog.color = this.color;
        dog.age = this.age;
        return dog;
    }
}
```
