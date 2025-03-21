---
theme: seriph
class: text-center
drawings:
  persist: false
transition: fade-out
mdc: true
overviewSnapshots: true
background: java-bg.png
---
# CS109 2025 Spring Lab6

---
layoutClass: gap-16
---
# Table of contents

<Toc v-click minDepth="1" maxDepth="5" columns="2"></Toc>

---
src: ./pages/class.md
---

---
src: ./pages/object.md
---

---
src: ./pages/method.md
---

---
src: ./pages/arraylist.md
---

---
layout: section
---
# Exercise

---
level: 2
---
## Exercise 1

实现一个简单的仓库`VelvetRoom`, 用户存储/删除/显示`Persona`对象, 即实现
- `VelvetRoom()` 作为构造函数, 初始化一个空的`ArrayList<Persona>`
- `void addPersona(Persona persona)` 添加一个`Persona`对象到`ArrayList`
- `void destroyPersona(String name)` 根据`name`从`ArrayList`中删除一个`Persona`对象
- `void showPersonas()` 打印所有`Persona`对象

每个`Persona`有`name`, `level` 和 `arcana`属性, `toString` 可以用任意格式输出。

---
layout: section
---
# The END
