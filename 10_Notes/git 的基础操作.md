---
created: 2026-01-01 15:03
updated: 2026-01-01 15:54
tags:
  - git
status: seed
aliases:
---

返回 [[../30_Maps/计算机基础能力|计算机基础能力]]

## 第一步：理解 Git 是什么

Git 是一个**分布式版本控制系统**。

- 它帮你保存代码的不同版本（像游戏存档一样）。
- 它能让你在不同分支上试验功能，最后合并。
- 它能让多人协作，每个人都能在本地完整拥有一份历史记录。

---

## 第二步：在本地用 Git 创建一个仓库

试试看：

```bash
# 新建一个目录
mkdir git-demo
cd git-demo

# 初始化 git 仓库
git init
```

会发现多了一个隐藏文件夹 `.git/`

---

## 第三步：添加文件并保存快照

在 `git-demo` 目录里，新建一个文件：

```bash
echo "Hello Git" > hello.txt
```

然后运行：

```bash
# 查看状态
git status

# 添加到暂存区
git add hello.txt

# 提交到版本库
git commit -m "第一次提交：添加 hello.txt"
```

现在 Git 已经保存了第一个快照。

---

## 第四步：连接 GitHub

在 GitHub 上新建一个仓库（空的，不要勾选 `README`），比如叫 `git-demo`。

然后在本地运行：

```bash
# 添加远程仓库地址（SSH 方式）
git remote add origin git@github.com:Polyhedronx/git-demo.git

# 把本地代码推送到 GitHub
git push -u origin master
```

刷新 GitHub 仓库页面，就能看到文件了。

---

## 第五步：常用操作

- 拉取并合并最新代码，相当于 `git fetch` + `git merge`，直接合并到当前分支：`git pull`
- 从远程拉取最新提交到本地，不会自动合并：`git fetch`
- 把改动放进暂存区：`git add . `
- 保存一次快照：`git commit -m "描述改动"`
- 推送改动上传到远程仓库：`git push`

进一步了解请看 [[git 的进阶操作]]
