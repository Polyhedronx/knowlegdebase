---
created: 2026-01-01 15:04
updated: 2026-01-01 22:58
tags:
  - git
status: seed
aliases:
---

## 额外操作

- `git status` 查看当前状态，详细见 [[git status]]
- `git stash` 临时保存当前修改，不提交也不丢失，详细见 [[git stash]]
- `git restore 文件名` 撤销工作区的修改，恢复到最近一次 commit 的状态
- `git restore --staged 文件名` 将文件从暂存区移回工作区，本地修改保留，只是不再被 commit 记录
- `git restore --source=HEAD --staged --worktree 文件名` 把文件恢复到最近一次 commit 的状态，暂存区和工作区都会被重置，作用等于撤销一切改动
- `git log` 查看提交历史
- `git diff <commit1> <commit2>` `<commit1>` 和 `<commit2>` 可以是：完整或简短的提交哈希（`SHA`），分支名（如 `main`、`feature-x`）或标签名（如 `v1.0`）；结果显示 `commit1 → commit2` 的改动
- `git diff --name-only a1b2c3 d4e5f6` 仅查看哪些文件改动
- `git diff <commit1> <commit2> -- 文件名` 只显示 `文件名` 的改动
- `git remote -v` 确认本地仓库连接了哪些远程仓库

---

## 学习分支

- `git branch feature-x` 新建分支
- `git checkout feature-x` 或 `git switch feature-x（推荐）` 切换分支
- `git checkout -b feature-x` 新建并切换分支
- `git merge feature-x` 将 `feature-x` 分支上的历史提交合并（取决于你目前在什么分支，一般先 `git switch main` 再使用该命令就可以合并到 `main` 分支）
- `git branch -d feature-x` 删除分支

### 解释

- `feature-`：通常是团队约定的前缀，用来表示这是一个新功能分支。
- `x`：功能编号或名称，可以用任何字母、数字或短划线命名，例如：
	- `feature-login`（登录功能）
	- `feature-shopping-cart`（购物车功能）

这只是命名习惯，Git 并不要求分支名必须带 `feature-`。

---

## 回退操作

### 临时回退到某个 commit（不会丢失后续 commit）

```bash
git switch --detach <commit-hash>
```

- 工作区会变成该 commit 的状态
- HEAD 脱离分支（Detached HEAD）
- 后续修改可以查看或测试，但如果直接提交，会脱离原分支，需要新建分支才能保存：

```bash
git switch -c test-branch
```

**特点**：安全，不会破坏分支历史

---

### 永久回退分支到某个 commit

如果你确定要 **让分支回到某个 commit，并丢弃之后的修改**：[[]]

```bash
git reset --hard <commit-hash>
```

- 当前分支指针直接移动到该 commit
- 工作区和暂存区都会被重置
- 之后的 commit 会被丢弃（除非通过 `git reflog` 恢复）

**示例**：

```bash
git log --oneline
# 假设最近三次 commit：
# 9fceb02 修复登录 bug
# a1b2c3 添加登录功能
# 789abc 初始化项目
git reset --hard a1b2c3
```

- 分支回退到 `a1b2c3`
- `9fceb02` 的修改会被删除

---

### 保留工作区修改，但回退分支指针

```bash
git reset --soft <commit-hash>
```

- HEAD 指向目标 commit
- 保留暂存区和工作区修改
- 适合想重新组织 commit，但保留改动

返回 [[../30_Maps/计算机基础能力|计算机基础能力]]
