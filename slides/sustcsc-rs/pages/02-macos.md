---
layout: section
---
# Running Rust on macOS

<v-click>

macOS 类似 Unix，所以它和 Linux 相似！🍎

</v-click>

---
level: 2
---
# Package Manager: Homebrew

<v-clicks>

- 开发者需要一个命令行包管理器
- **[Homebrew](https://brew.sh)** 是最受欢迎的选择
- 提供简便的软件安装和管理

</v-clicks>

```bash
# 安装 Xcode 命令行工具
xcode-select --install

# 安装 Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

---
level: 2
---
# Installing Rust

<br>
<v-click>

使用 Homebrew 获取库和 Rust 工具链很简单：

```bash
brew install rustup curl pkgconf unzip util-linux wget && \
. "$HOME/.cargo/env" && \
rustup default stable # 或 nightly
```

</v-click>

<v-click>

**验证安装：**

```bash
rustc --version
```

</v-click>

<v-click>

💡 **推荐**：安装带有 rust-analyzer 扩展的 VS Code

</v-click>