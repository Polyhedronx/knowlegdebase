---
created: 2026-01-01 14:02
updated: 2026-01-03 22:25
tags:
  - git
status: seed
aliases:
  - status
---

## 0. 基本概念

`git status` 是 Git 中最常用的命令之一，它用来 **查看当前仓库的状态**，告诉你哪些文件被修改、哪些文件已暂存、哪些文件未被跟踪。理解它能让你在提交前对工作区和暂存区一清二楚。

---

## 1. 基本用法

```bash
git status
```

运行后，Git 会输出类似信息：

```terminal
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   file1.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file2.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file3.txt
```

---

## 2. 输出内容解释

### 当前分支信息

```
On branch main
Your branch is up to date with 'origin/main'.
```

- 当前所在分支
- 当前分支与远程分支的同步状态（落后/领先/同步）

### 已暂存文件

```
Changes to be committed:
        modified:   file1.txt
```

- 已经用 `git add` 添加到暂存区的文件
- 下一次 `git commit` 会把这些文件包含进去
- 如果想取消暂存：

```bash
git restore --staged file1.txt
```

### 未暂存文件

```
Changes not staged for commit:
        modified:   file2.txt
```

- 文件被修改过，但还没有暂存
- 如果想提交，需要先 `git add file2.txt`
- 如果想丢弃修改，恢复到上一次 commit 的状态：

```bash
git restore file2.txt
```

### 未跟踪文件

```
Untracked files:
        file3.txt
```

- 新创建的文件 Git 没有管理
- 提交前需要 `git add file3.txt`
- 如果不想提交，可以忽略或加入 `.gitignore`

---

## 3. 常用选项

**简洁模式**

```bash
git status -s
```

输出：

```
M  file1.txt   # 已修改但未暂存
A  file2.txt   # 已暂存，准备提交
?? file3.txt   # 未跟踪文件
```

**只查看未暂存文件**

```bash
git status -uno
```

---

## 4. 使用场景

- 确认哪些文件被修改、哪些文件未暂存
- 如果 commit 没包含改动，先 `git status` 看是不是忘记 add
- 确认分支与远程仓库同步状态
