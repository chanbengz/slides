---
layout: section
---
# Git

---
level: 2
---
# Why This
The Mythical Man-Month

虽然这部份与Java无关, 但是对project的管理非常重要

大家做project的时候, 分工合作的时候经常会有这种情况

<v-clicks>

- 如何传输你写的代码给别人? 发zip包?
- 手动合并代码?
    - 如果你们分别改了两个文件, 那问题不大
    - 但是如果改了同一个文件, 怎么办?
- 如何在开发新功能的时候, 保证原来的代码不会被破坏?
    - 保存一份能跑的代码? 很难记住每个版本有啥功能
    - You tell me
- 解决方案?

</v-clicks>

---
level: 2
---
# Version Control
Solves the above problems

我们需要一个工具来帮助我们管理代码, 它能够:

<v-clicks>

- 记录每个代码的变化, 同时保存一些注释来解释代码的更改
- 让你回到任何一个版本
- 有分支记录功能, 每个人在不同分支上更改
- 如果有代码冲突, 能清晰地告诉你哪里冲突了并让你手动合并
- 如果没有冲突, 自动合并
- 有一个中央服务器, 保存所有代码的版本, 每个人都可以从这个服务器上下载代码

</v-clicks>

---
level: 2
---
# Git
The most popular version control system

- Created by Linus Torvalds in 2005
- Distributed version control system
    - Every developer has a full copy of the repository
- Free and open source
- ...

## Installation

Windows: 下载[Git for Windows](https://gitforwindows.org/)

Mac/Linux: `xcode-select --install` or `sudo apt-get install git`

一些教程: https://www.runoob.com/manual/git-guide/

---
level: 2
---
# Basic Usage
Learn Git by using it

创建一个新的文件夹, 在这个文件夹下打开命令行, 创建一个新的git仓库

```bash
mkdir myproject
cd myproject
git init # 初始化一个git仓库
``` 

当你写完一部分代码, 想要保存这个版本, 你可以使用`git add`和`git commit`并附上一些注释

```bash
echo "Hello, World" > hello.txt
git add hello.txt # 添加这个文件到暂存区
git commit -m "Add hello.txt" # 提交这个文件
```

每次`commit`就是一个版本, 你可以使用`git log`来查看历史记录。如果要回退到某个版本, 使用`git reset`

```bash
git log # 查看历史记录
git reset --hard <commit-id> # 回退到某个版本
```

---
level: 2
---
# Concept
Git的一些概念

- Repository: 代码仓库, 保存所有的版本
- Working Directory: 你正在工作的目录, 实际的文件
- Stage: 暂存区, 用来存放你想要提交的(多个)文件
- Commit: 提交, 一个版本, 记录了你的代码变化
- HEAD: 指向当前版本的指针

<br>

<img src="https://github.com/rogerdudler/git-guide/blob/gh-pages/img/trees.png?raw=true" width="500px">

---
level: 2
---
# Branch
隔离开发与稳定版本

代码一般分为两个分支: `master`和`develop`
- `master`是稳定版本, 保证这部份代码是可以稳定运行的
- `develop`是开发版本, 你可以在这个分支上开发新功能, 测试完毕后合并到`master`

```bash
git checkout -b develop # 创建并切换到develop分支
git checkout master # 切换到master分支
git branch -d develop # 删除develop分支
```

<br>
<img src="https://github.com/rogerdudler/git-guide/blob/gh-pages/img/branches.png?raw=true" width="500px">

---
level: 2
---
# Merge
Apply changes to the main branch

当你在`develop`分支上开发完毕, 你需要将这个分支合并到`master`分支上

```bash
git checkout master # 切换到master分支
git merge develop # 将develop分支合并到master分支
```

一般情况下, Git会自动合并代码, 但是

如果有冲突, 你需要手动解决冲突, 然后再次`commit`

```bash
vim hello.txt # 手动解决冲突
git add hello.txt # 标记冲突已解决
```

---
level: 2
---
# Remote
Github

如果你有远程服务器, 你可以将你的代码上传到这个服务器上, 这样其他人就可以下载你的代码

```bash
git remote add origin <server>
git clone <url> # 从远程服务器下载代码
```

如果本地与远程服务器的代码不同, 你可以使用

```bash
git pull # 从远程服务器下载代码并合并
git push # 将本地代码上传到远程服务器
```

一般我们使用[Github](https://github.com)来作为远程服务器, 因为它提供了很多方便的功能
- Issue: 记录bug和新功能, 与用户交流
- Pull Request: 提交你的代码, 让别人review, 为开源社区做贡献
- Star: 收藏你喜欢的项目
- ~~Show off: 展示你的项目, 方便找工作~~

