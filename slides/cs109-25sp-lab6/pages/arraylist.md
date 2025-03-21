---
layout: section
---
# ArrayList

---
level: 2
---
# Why ArrayList
就是数组不好用呗

数组的一个致命缺陷: 长度固定, 多了浪费, 少了不够用. 而且大部分时候我们并不知道需要多大的数组

所以我们需要一个可以动态扩展的数组, 就是ArrayList, 支持

- `add(E e)`: 添加元素
- `remove(int index)`: 删除元素
- `get(int index)`: 获取元素
- `set(int index, E e)`: 修改元素
- `size()`: 获取元素个数
- for-each 迭代
- ...

---
level: 2
---
# Basic Usage
Ez

初始化, `ArrayList<T>` 里面的 `T` 是元素的类型, 后面的 `<>` 里面可以不写
```java
ArrayList<Integer> list = new ArrayList<>();
```

添加, 删除, 获取, 修改元素
```java
list.add(1); // [1]
list.add(2); // [1, 2]
list.remove(0); // [2]
list.get(0); // 2
list.set(0, 3); // [3]
```

遍历, 但是lambda
```java
list.forEach(System.out::println);
list.forEach((e) -> {
    e += 1;
    System.out.println(e);
});
```
