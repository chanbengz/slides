---
level: 2
---
# Environment Setup

<v-clicks>

- 此挑战仅专注于 **CPU 优化**
- 假设具有基本编程知识，不需要 Rust 经验
- 最佳平台：**Linux/macOS** > Docker > Windows (WSL2)

</v-clicks>

---
level: 2
---
# Platform Recommendations

<v-clicks>

1. **Linux/macOS**：原生环境，设置最快
2. **Docker**：容器化，但需要网络设置
3. **Windows**：使用 WSL2 或 Docker Desktop

</v-clicks>

<v-click>

💡 **提示**：Rust 生态系统使用 Cargo（包管理器）和 [crates.io](https://crates.io)

</v-click>

---
level: 2
---
# Benchmarking Environment

<div class="grid grid-cols-2 gap-4">
<div>

**硬件：**
- CPU: Intel Xeon Platinum 8350C
- 核心：20 × 2 插槽 (2.40GHz)
- GPU: N/A

</div>
<div>

**软件：**
- Rust: stable (可能是 1.87.0) / nightly
- 主机操作系统：Linux 4.18.0-372.32.1.el8_6.x86_64
- 容器：Singularity 3.7.1

</div>
</div>

<v-click>

⚠️ **注意**：执行期间无网络访问，忽略编译成本

</v-click>