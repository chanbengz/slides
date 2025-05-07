---
layout: section
---
# Generic

---
level: 2
---
# A Trivial Example of Generic
Generic is everywhere

什么事generics(泛型)？你早就见过了

```java
List<String> names = new ArrayList<>();
List<Integer> numbers = new ArrayList<>();
```

- 泛型允许我们创建可以处理不同数据类型的类、接口和方法。
- 你可以看到，`names` 和 `numbers`共享`ArrayList`的代码。不同的是，里面的元素类型。
- 类型作为参数在尖括号 `<>` 中指定。

<div v-click="1">

看上去好像跟多态差不多? 其实不是

</div>

<div v-click="2">

那哪里不一样? Generic是一种“元编程 (meta programming)”

元编程是指, 编写一套用来生成代码的代码

</div>

---
level: 2
---
# Generic Class
Write generic yourself

<v-switch>

<template #0>

```java
public class Box<T> {
    private T content;
    
    public void set(T content) {
        this.content = content;
    }
    
    public T get() {
        return content;
    }
}

Box<String> stringBox = new Box<>();
stringBox.set("Hello");
String value = stringBox.get(); // 不需要类型转换
```

</template>

<template #1>

```java
public class Box {
    private String content;
    
    public void set(String content) {
        this.content = content;
    }
    
    public String get() {
        return content;
    }
}

Box stringBox = new Box();
stringBox.set("Hello");
String value = stringBox.get(); // 不需要类型转换
```

</template>

</v-switch>

What happened? 这里`Box<String>`是一个新类型, 当你写下这个时, 相当于你写了一个

`Box<T>` 会替换 `T` 为你想要的类型！我们管 `T` 叫泛型标记符(generic parameter)

---
level: 2
---
# Generic Method

```java
public class Utils {
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.println(element);
        }
    }
}

Integer[] intArray = {1, 2, 3};
String[] stringArray = {"Hello", "World"};
Utils.printArray(intArray);
Utils.printArray(stringArray);
```

类型由编译器自动推导，不需要你写，因为是静态的。

---
level: 2
---
# Generic Parameter
and example of multiple parameters

泛型标记符有这些, 前四个使用上没有区别, 只用于区分多个不同的标记符
- `T` (type) 表示具体的一个java类型
- `E` (element) 代表Element
- `K` `V` (key value) 分别代表java键值中的Key Value
- `?` 表示不确定的 java 类型

`?` 跟 `T` 最大的区别在于下面写法`T`可以但是`?`不行

```java
T t = getSomething(); // ? t = getSomething();
```

多parameter的写法

```java
private <T, E> E two_type(T arg1, E arg2){
    // arg1 and arg2 可以是不同的类型
    return arg2;
}
```

---
level: 2
---
# Why Generic and Generic vs. Polymorphism
It really helps

现在你应该知道generic和polymorphism的区别了
- Polymorphism 是动态的, 编译时具体类型不确定
- Generic 是静态的, 相当于编译器用你写的模版重写了几份代码, 编译时都是确定的
    - `Box<String>` 和 `Box<Integer>` 相当于你写了一个 `BoxString` 和 `BoxInt` 类(暂时这么叫)
    - 这两个类没有任何关联, 不是继承也不是, 不能用多态

什么时候用generic? ~~当你想偷懒的时候~~

多态要求你针对不同的类有不同的实现, 但是泛型不要求。~~写一份泛型, 多一倍的类~~

泛型的一大优势: 静态，编译器可以帮你检查类型。劣势是不如多态灵活。

<div v-click="1">

## 谁说不可以把它俩结合起来?

</div>

---
level: 2
---
# Bounded Generic
Combine generic and polymorphism

<v-switch>

<template #0>

假如我们写了个一个简单的泛型类

```java
class Pair<T> {
    private T first, last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { return first; }
    public T getLast() { return last; }
}
```

以及一个静态方法

```java
static int add(Pair<Number> p) {
    Number first = p.getFirst();
    Number last = p.getLast();
    return first.intValue() + last.intValue();
}
```

</template>

<template #1>

```java
int sum = PairHelper.add(new Pair<Number>(1, 2));
```

是正常的，因为多态在这里起作用了，但是

```java
Pair<Integer> p = new Pair<>(123, 456);
```

报错了，说是`incompatible types: Pair<Integer> cannot be converted to Pair<Number>`

也就是说，跟之前说的一样，`Pair<Number>`和`Pair<Integer>`是没有继承关系的。

那咋办？ 用`?`通配符加上bound, 让泛型也具有多态。

</template>

<template #2>

甚至不需要改`Pair<T>`本身，因为它已经用了模版。只需要改传入类型即可

```java
static int add(Pair<? extends Number> p) {
    Number first = p.getFirst();
    Number last = p.getLast();
    return first.intValue() + last.intValue();
}
```

然后就会变成`<? extends Number> getFirst();`, `?`虽然是任意类型，但被`extends`限定必须是`Number`的子类。
这个叫 Upper Bound (上界限定符)。

同样也可以限定父类是谁, 用`<? super T>`。细心的同学会发现这里用了`T`作为下界，想想为啥以及怎么用?

```java
static <T> void copy(List<? super T> dst, List<T> src){
    for (T t : src) {
        dst.add(t);
    }
}
```

</template>

<template #3>

如果你想在写模版的时候就做限定, 那最好不过了！

```java
class Pair<T extends Number> {
    private T first, last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { return first; }
    public T getLast() { return last; }
}
```

所有的非`Number`都会造成compile error。

```java
Pair<Integer> p;
Pair<String> ps; // error
```

</template>

</v-switch>

