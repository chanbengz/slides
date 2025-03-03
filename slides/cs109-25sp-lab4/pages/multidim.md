---
layout: section
---
# Multi-dimensional Array

---
level: 2
---
# Understanding n-D Array
我们知道引用是怎么回事了, 那多维数组呢

二维数组, 是元素为数组引用的数组

```java
int[][] mat1 = new int[3][4]; // 3行4列
int[][] array2 = new int[3][];  // 3行, 每行是null
array2[0] = new int[2], array2[2] = new int[3];
int[][] array3 = {{1, 1}, {4, 5, 1}, {4, 1, 9, 1}}; // 直接初始化
```

<img src="https://github.com/zhuym1219/java-basic-materials/blob/master/pictures/Part06-2.png?raw=true" width=500px /> 

---
level: 2
---
# Iterating n-D Array
Knowing where you are

正常遍历即可, 需要多层循环

````md magic-move
```java
for (int i = 0; i < mat1.length; i++) {
    for (int j = 0; j < mat1[i].length; j++) {
        System.out.print(mat1[i][j] + " ");
    }
    System.out.println();
}
```

```java
for (int[] row : mat2) {
    for (int ele : row) {
        System.out.print(ele + " ");
    }
    System.out.println();
}
```
````

- 知道你在哪一层, 以及每一层的长度, `mat1[i].length`获取每一行的长度, 无需考虑是否等长
- `mat1[i]` 可以看作是一个一维数组, 再索引这个数组`mat1[i][j]`
- foreach 写法也类似, 但是要注意, `row` 是一个一维数组

<div v-click="2">

那推广到n维数组, ~~各位小天才应该知道怎么做了~~

`int[][][] arrn = new int[2][3][4];` 表示这个数组的元素是`int[3][4]`, 然后递推下去

</div>
