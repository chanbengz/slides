---
layout: section
---
# Arithmetic

---
level: 2
---
# Expression vs. Statement

- Expression(表达式), with mathematical meaning, has a value

```java
int a = 3, b;
System.out.println(a = b = 4); // 4, a = 4, b = 4
System.out.println(a++);       // 4, a = 5
System.out.println(++a);       // 6, a = 6
```

- Statement(语句), with programming meaning, has an effect and may not have a value

```java
int a, b, c; // declaration statement
if (a > 0) { // if statement with block
    System.out.println("a is positive");
}
int a = if (b > 0) 1 else 0; // error
```

一句话, 有没有返回值区分表达式和语句。写法上, Statement会有分号或者大括号, Expression不会。

---
level: 2
---
# Type Conversion

数据类型的级别由低至高的顺序：

```
byte -> short -> int -> long -> float -> double
```

- 隐式转换: 低级别向高级别赋值，可以自动转换
- 显式转换: 高级别向低级别赋值，需要在低级别数据前加括号里面标识高级别数据类型

隐式(自动)转换:

```java
int number1 = 30;
double number2 = number1; 
```

显式(强制)转换:

```java
double number1 = 25.5;
int number2 = (int) number1;
```

强制转换后，可能会导致部分数据丢失
