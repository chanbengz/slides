---
layout: section
---
# Running Rust inside Docker

<v-click>

如果你来自 **Windows** 设置，跳到 **启动 Rust** 部分！🚀

</v-click>

---
level: 2
---
# Installing Docker
<br>

**Linux**：遵循 [Docker 安装指南](https://docs.docker.com/engine/install/)

<v-click>

**网络问题？** 尝试使用镜像：

```bash
echo '{
    "registry-mirrors": [
        "https://docker-proxy.benx.dev"
    ]
}' > /etc/docker/daemon.json
sudo systemctl daemon-reload
sudo systemctl restart docker
```

</v-click>

<v-click>

**macOS**：从 [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-mac) 安装 Docker Desktop - 只需几次点击！🖱️

</v-click>

---
level: 2
---
# Bootup Rust

<br>

Docker 安装完成后，使用提供的 Docker compose 文件：

```bash
git clone https://github.com/chanbengz/sustcsc-rs
cd sustcsc-rs
docker-compose up -d dev # 或者 stable/nightly 仅用于运行
```

<v-click>

🛠️ **`dev`** 提供交互式开发环境：
- 在容器外编写代码
- 在容器内运行代码
- 在环境内执行任何命令

</v-click>