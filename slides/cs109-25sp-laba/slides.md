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
# CS109 2025 Spring LabA

---
layoutClass: gap-16
---
# Table of contents

<Toc v-click minDepth="1" maxDepth="5" columns="2"></Toc>

---
src: ./pages/polymorphism.md
---

---
src: ./pages/interface.md
---

---
src: ./pages/abstract.md
---

---
layout: section
---
# Exercise

---
level: 2
---
## Exercise 1

智能家居有一套统一的控制接口, 我们现在来模拟一下! ~~写完之后可以入职智能家居的工程师了~~

有一个`Appliance`接口, 这个接口只控制家具的开关, 也就是`turnOn()`和`turnOff()`两个方法.

你需要
- 改写并用这个接口实现`AirConditioner`和`Light`两个类.
- 改写`Client`让它更通用

---
layout: section
---
# The END
