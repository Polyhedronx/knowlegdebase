---
created: 2026-01-01 14:01
updated: 2026-01-01 14:02
tags:
  - git
status: seed
aliases:
  - stash
---

`git stash` 的核心作用是 **临时保存你当前的工作区修改，而不提交到分支**，方便切换分支或处理紧急任务。它可以避免丢失改动又不影响分支历史。

---

## 基本概念

- **工作区**：你正在编辑的文件
- **暂存区**：用 `git add` 添加的修改
- **stash 区**：Git 提供的临时 " 抽屉 "，保存未提交的修改

操作效果：

```text
工作区 + 暂存区 → stash
工作区和暂存区 → 清空
```

修改被保存起来，分支回到干净状态

---

## 基本用法

### 保存修改

```bash
git stash
```

- 会把 **工作区 + 暂存区修改** 都保存起来
- 工作区恢复到上一次 commit 的干净状态
- 可选添加消息：

```bash
git stash save "添加登录功能的未完成修改"
```

### 查看 stash 列表

```bash
git stash list
```

输出示例：

```
stash@{0}: WIP on main: 9fceb02 添加登录功能
stash@{1}: WIP on main: a1b2c3 修复 bug
```

`stash@{0}` 是最新保存的 stash

### 恢复修改

```bash
git stash apply stash@{0}
```

- 会把 stash 中的修改应用到当前分支
- **不会删除 stash**
- 如果想应用后删除：

```bash
git stash pop
```

### 删除 stash

删除单个：

```bash
git stash drop stash@{0}
```

删除所有：

```bash
git stash clear
```

---

## 常用选项

|命令|作用|
|---|---|
|`git stash -u` / `--include-untracked`|连未跟踪文件一起保存|
|`git stash -k` / `--keep-index`|只保存工作区修改，暂存区保留|
|`git stash branch <new-branch>`|从 stash 创建新分支，并应用修改|

---

## 使用场景

**切换分支前临时保存修改**

```bash
git stash
git switch feature-y
```

**处理紧急 bug**

当前在 `feature-x` 做修改，但主分支需要修复紧急 bug：

```bash
git stash
git switch main
# 修复 bug，commit, push
git switch feature-x
git stash pop
```

**保留未完成的实验**

当功能还没完成，但想先回到干净状态进行其他操作

---

## 小技巧

查看某个 stash 的内容：

```bash
git stash show -p stash@{0}
```

多个 stash 可以按顺序应用：

```bash
git stash apply stash@{1}
git stash apply stash@{0}
```

---

简单比喻：

- `git stash` = " 把你桌面上没完成的稿子放进抽屉，桌面恢复干净，方便做其他事 "
- `git stash pop` = " 把抽屉里的稿子拿出来继续编辑，同时抽屉清空 "
