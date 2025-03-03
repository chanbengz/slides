---
layout: section
---
# Array 101

---
level: 2
---
# 学习数组, 理解数组, 用好数组

数组: 元素类型一样, 相互挨着, 数组长度固定

<v-switch>
  <template #0>

| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|-------|---|---|---|---|---|---|---|---|---|---|
| Value | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

  </template>
  <template #1>

| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|-------|---|---|---|---|---|---|---|---|---|---|
| Value | 0 | 0 | 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

  </template>
  <template #2>

| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|-------|---|---|---|---|---|---|---|---|---|---|
| Value | 0 | 0 | 3 | 0 | 0 | 0 | 0 | 5 | 0 | 0 |

  </template>
</v-switch>

代码这么写

```java
int[] a = new int[10];
a[2] = 3;
a[7] = 5;
int[] b = {5, 4, 3, 2, 1};
```

- `int[]` 表示元素为`int`的一维数组
- `new int[10]` 表示创建一个<span v-mark.orange="0">长度为10</span>的数组, 初始化为<span v-mark.orange="0">0</span>
- `new` 在<span v-mark.orange="0">堆</span>上分配内存, 或者指定初始化的值, 在<span v-mark.circle.red="0">栈</span>上分配内存
- 下标(index) 从<span v-mark.orange="0">0</span>开始, 通过下标访问数组元素, <span v-mark.red="0">不能越界！</span>

---
level: 2
---
# Iterate All
我们学过了for循环, 是时候用它来遍历数组了

`for` 有两种写法, 第二种我们称为<span v-mark.orange="0">enhanced for</span> 或者 <span v-mark.orange="0">foreach</span> 或者 迭代器

````md magic-move
```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(a[i]);
}
```

```java
for (int x : arr) {
    System.out.println(x);
}
```
````

- `array.length` 可以获取数组长度

Difference!
- 第一种方式, 通过下标访问数组元素, 可以修改元素
- 第二种方式, 其实只是"借用"了数组元素(赋值给了`x`), <span v-mark.circle.red="1">不能</span>修改元素

---
layout: two-cols
level: 2
---
# Memory
The good old days

<img src="https://courses.grainger.illinois.edu/cs225/fa2022/assets/notes/stack_heap_memory/memory_layout.png"/>

::right::

JVM 有两种内存空间: 栈(Stack) 和 堆(Heap)
- 栈: 存放固定大小的数据, 速度快, 但是大小固定
- 堆: 存放动态分配的数据, 速度慢, 但是数据大小可变

当你写下`new int[10]`的时候, JVM会在堆上分配10个`int`的空间, 并且返回一个指向这个空间的引用,
于是`int[] arr`在栈上存有这个引用

```java
int[] arr1 = new int[10];
int[] arr2 = arr1;

arr2[0] = 1; // arr1[0] 也会变成1
System.out.println(arr1[0]); // 1
```

引用也可以被借用

- ~~JVAV 没有指针/地址的概念~~
