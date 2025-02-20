---
layout: section
---
# I/O and Format String

---
level: 2
---
# Input from user (via the blackbox)
可能就有小朋友会问了, console是啥

上一节lab, 我们见过这样的代码
````md magic-move {lines: true}
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int a = scanner.nextInt();
        int b = scanner.nextInt();
        System.out.println(a + b);
        
        scanner.close(); // Dont forget!
    }
}
```

```java {1-2}
// What's Scanner?
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int a = scanner.nextInt();
        int b = scanner.nextInt();
        System.out.println(a + b);

        scanner.close(); // Dont forget!
    }
}
```

```java {5-6}
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        // What's System.in and why we do that?
        Scanner scanner = new Scanner(System.in);
        int a = scanner.nextInt();
        int b = scanner.nextInt();
        System.out.println(a + b);

        scanner.close(); // Dont forget!
    }
}
```

```java {6-8}
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        // What am I doing?
        int a = scanner.nextInt();
        int b = scanner.nextInt();
        System.out.println(a + b);

        scanner.close(); // Dont forget!
    }
}
```
````
<div v-click="4">
    <span v-mark.red="4">空格, tab和换行是分割符</span>, 用来分割输入的数据, <span v-mark.circle.red="4">不会</span>被读入到程序中, 
    比如上面输入<code>1 2</code>
</div>

<br>

<div v-click="5">
    要通过最后的<span v-mark.circle.orange="5">换行符</span>来告诉程序你的输入结束了
</div>

---
level: 2
---
# If you like whitespaces...
还是有办法的

如果你想输入空格tab, 必须要用`nextLine()`方法

```java
    String s = scanner.nextLine();
    // Input: Hello, World!
```

<div v-clicks>
<v-click>
注意！ 如果先使用了<code>next()</code> 再使用<code>nextLine()</code>会出问题

```java
    int a = input.nextInt();
    String b = input.nextLine();

    System.out.println("a = " + a);
    System.out.println("b = " + b);
}

$ java Main
12
a = 12
b =
```
</v-click>

<v-click>
    因为<code>nextInt()</code>只读取了数字, 但<code>nextLine()</code>读取了<code>nextInt()</code>后的换行符
</v-click>

</div>

---
level: 2
---
# Multiple lines
有点点麻烦, 但不是不行

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder inputBuffer = new StringBuilder();

        while (scanner.hasNextLine()) { // another method in Scanner
            inputBuffer.append(scanner.nextLine()).append("\n");
        }

        String input = inputBuffer.toString();
        System.out.println("Your input is:");
        System.out.println(input);

        scanner.close();
    }
}
```

要用EOF结束输入, 也就是`^D`(Mac)或者`Ctrl+Z`(Windows)

---
level: 2
---
# Format String
print/println直接输出, printf格式化

当使用`printf`输出, 或用`String.format`构造字符串的时候, 使用特殊的占位符来表示格式

```
%<flag><width><.precision><conversion-character>
```

1. 先是基本的格式化`System.out.printf("%d + %d = %d", 1, 2, 3);`输出`1 + 2 = 3`
2. 然后如果需要特定宽度, 就加上`<width>`, 例如`%5d`输出`    1`, 默认右对齐
3. 如果需要左对齐, 加上`-`(就是`<flag>`), 例如`%-5d`输出`1    `
4. 浮点数的精度`<.precision>`, 例如`%.2f`输出`3.14`

<div v-click>

| Conversion | Type | Conversion | Type | Conversion | Type |
| --- | --- | --- | --- | --- | --- |
| %s | String | %d | byte, short, int, long | $o | 八进制 |
| %x, %X | 十六进制(大小写) | %f | float, double | %e, %E | 科学计数法 |
| %g | 自动选择 %f 或 %e | %c, %C | char | %b | boolean |

</div>
