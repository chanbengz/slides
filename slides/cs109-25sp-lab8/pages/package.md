---
layout: section
---
# Package

---
level: 2
---
# Directory as Structure
A move-on from class as a file

之前我们提到, 一个文件就是一个类. 那类似的, 一个文件夹就是一个package, 包括了一组类.

```java
package sustech.cs109;

public class Student {
    public String name;
    public int age;
}
```

编译这个代码会生成一个文件 `sustech/cs109/Student.class`, 通过将`/`替换为`.`来引用这个类

```java
import sustech.cs109.Student;

public class Main {
    public static void main(String[] args) {
        Student s = new Student();
        s.name = "Alice";
        s.age = 20;
    }
}
```

---
level: 2
---
# Ha?
为什么要这样做呢?

大家应该明白了`package`的作用: 就是指定生成的`.class`文件的位置, 作为引用的时候的路径.

但是, 为什么要这样做呢? 为什么不直接生成到一个文件夹里呢?

答案就是: 方便管理, 以及避免命名冲突.

~~当你们project做的越来越大的时候就会明白了, 这么做总没错的~~

<v-click>
这张slide结束了
</v-click>
