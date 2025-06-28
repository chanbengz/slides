---
layout: section
---
# Running Rust on Windows

<br>
<v-click>

没有人 *想要* 在 Windows 上开发 Rust... 😅

</v-click>

<v-click>

但如果你 **必须这样做**，这里有替代解决方案：

</v-click>

---
level: 2
---
# Option 1: Using WSL/WSL2

<br>
<v-clicks>

- **Windows Subsystem for Linux** 提供 Linux 环境
- Windows 用户推荐的方法
- 接近原生 Linux 性能

</v-clicks>

<v-click>

📚 **设置**：遵循 [WSL 官方文档](https://learn.microsoft.com/en-us/windows/wsl/install)

</v-click>

<v-click>

➡️ 然后按照 **Linux** 安装步骤

</v-click>

---
level: 2
---
# Option 2: Using Docker Desktop

<br>
<v-clicks>

- 容器化开发环境
- 在不同系统间保持一致
- 比 WSL 略有开销

</v-clicks>

<v-click>

📚 **设置**：遵循 [Docker Desktop 文档](https://docs.docker.com/desktop/windows/install/)

</v-click>

<v-click>

➡️ 然后按照 **Docker** 设置步骤

</v-click>