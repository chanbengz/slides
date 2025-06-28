---
layout: section
---
# Running Rust on Linux

<v-click>

如果你使用 Linux，你可能知道怎么做！

</v-click>

<v-click>

但如果你不知道，这里有一个快速指南 📚

</v-click>

---
level: 2
---
# Find Your Distribution

```bash
uname -a
# Linux your-hostname 6.14.6-arch1-1 #1 SMP PREEMPT_DYNAMIC...

cat /etc/os-release
# NAME="Arch Linux"
# PRETTY_NAME="Arch Linux"
# ID=arch
# BUILD_ID=rolling
# ...
```

<v-click>

一旦你知道你的发行版，安装所需的包 📦

</v-click>

---
level: 2
---
# Package Installation

<div class="grid grid-cols-2 gap-4 text-sm">
<div>

**Debian/Ubuntu:**
```bash
sudo apt-get update && \
sudo apt-get install -y build-essential \
    ca-certificates curl \
    pkg-config unzip util-linux wget
```

**Fedora:**
```bash
sudo dnf makecache --refresh && \
sudo dnf group install c-development && \
sudo dnf install ca-certificates curl \
    pkg-config unzip util-linux wget
```

</div>
<div>

**RHEL:**
```bash
sudo dnf makecache --refresh && \
sudo dnf groupinstall "Development tools" && \
sudo dnf install epel-release && \
sudo /usr/bin/crb enable && \
sudo dnf install ca-certificates curl \
    pkg-config unzip util-linux wget
```

**Arch:**
```bash
sudo pacman -Syyu && \
sudo pacman -S base-devel ca-certificates curl \
    pkg-config unzip util-linux wget
```

</div>
</div>

---
level: 2
---
# Installing Rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh && \
. "$HOME/.cargo/env" && \
rustup default stable # 或 nightly
```

<v-click>

**验证安装：**

```bash
rustc --version
```

</v-click>

<v-click>

🎉 **你已经准备好了！**

</v-click>