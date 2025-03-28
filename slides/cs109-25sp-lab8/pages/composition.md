---
layout: section
---
# Composition of classes

---
level: 2
---
# Static Variables
Before going into composition...

`Static` 标记的变量表示这个变量是所有同一个类的实例共享的。

```java
class Student {
    static int count = 0;
    String name;
    public Student(String name) {
        this.name = name;
        count++;
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student("Alice"); // count = 1
        Student s2 = new Student("Bob");   // count = 2
        System.out.println(s1.count == s2.count); // True, because count is shared
    }
}
```

---
level: 2
---
# Composition
就是套娃

软件设计中, 不可避免地会有层级的设计, 在OOP中的表象就是类的嵌套, 例如exercise6

```java
public class VelvetRoom {
    private ArrayList<Persona> personas;
}

public class Persona {
    private Arcana arcana;
}
```

有方法/属性的依赖, 比如`VelvetRoom.toString()`需要`Persona.toString()`

```java
public class VelvetRoom {
    public String toString() {
        StringBuilder sb = new StringBuilder();
        for (Persona p : personas) {
            sb.append(p.toString());
        }
        return sb.toString();
    }
}
```

