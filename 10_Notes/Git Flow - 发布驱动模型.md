---
created: 2026-01-02 16:05
updated: 2026-01-02 22:58
tags:
  - Cooperation
  - Git
status: sapling
aliases:
---

## 0. 核心概念

Git Flow 是一种非常经典且严格的 Git 分支管理模型，最早由 Vincent Driessen 提出。它不是一个软件，而是一套**基于分支的约定**。

它的核心逻辑是 " 发布驱动 "（Release Driven），即所有的开发活动都是围绕着 " 构建下一个版本 " 进行的。这非常适合版本号清晰（如 v1.0, v2.0）的传统软件开发，如手机 App、桌面客户端或系统级软件。

---

## 1. 五大分支体系

在 Git Flow 模型中，分支被分为两类：**长期分支**和**短期分支**。

### 长期分支 (Long-lived Branches)

这两个分支在项目的整个生命周期中一直存在：

1. **master** (或 main): 存放**随时可发布**到生产环境的代码。严禁直接 Push 代码，只能通过合并进入。所有 Commit 必须打上 Tag（如 `v1.0.1`）。
2. **develop**: 开发主干。存放**下一个版本**的最新开发进展。相当于 " 集散地 "，包含所有已完成的特性。

### 短期分支 (Short-lived Branches)

这些分支用完即删：

1. **feature**: 功能分支。用于开发新功能。
2. **release**: 预发布分支。用于版本发布前的准备（修 Bug、写文档、改版本号）。
3. **hotfix**: 热修复分支。用于紧急修复生产环境的 Bug。

---

## 2. 场景化工作流演示

假设正在维护一个电商 App，当前线上版本是 `v1.0`。

### 场景一：开发新功能 (Feature)

**目标**：开发 " 购物车 " 功能。

1. **起点**：从 `develop` 分支检出。
2. **过程**：开发人员在 `feature/cart` 上提交代码。
3. **终点**：开发完成，合并回 `develop`，**不合并**到 `master`。

```bash
# 1. 创建功能分支
git checkout -b feature/shopping-cart develop

# 2. 开发并提交
git commit -m "feat: add add-to-cart button"

# 3. 完成开发，合并回 develop
git checkout develop
git merge --no-ff feature/shopping-cart
git branch -d feature/shopping-cart
```

> **注意**：使用 `--no-ff` (No Fast Forward) 可以在历史记录中保留分支存在的痕迹，便于回溯特性群。

### 场景二：准备发布新版本 (Release)

**目标**：功能开发差不多了，准备发布 `v1.1`。

1. **起点**：从 `develop` 分支检出 `release/v1.1`。
2. **过程**：
	- 修改版本号元数据。
	- 测试人员介入，发现小 Bug 直接在该分支修复。
	- **严禁**在此分支开发新功能。
3. **终点**：
	- 合并到 `master`（发布）。
	- 合并回 `develop`（同步 Bug 修复代码）。
	- 在 `master` 上打 Tag。

```bash
# 1. 创建预发布分支
git checkout -b release/v1.1 develop

# 2. 模拟修复发布过程中的小bug
git commit -m "fix: adjust button padding for v1.1"

# 3. 正式发布：合并到 master 并打标签
git checkout master
git merge --no-ff release/v1.1
git tag -a v1.1 -m "Release version 1.1"

# 4. 同步回 develop (确保 develop 也有刚才修的 bug)
git checkout develop
git merge --no-ff release/v1.1

# 5. 删除分支
git branch -d release/v1.1
```

### 场景三：线上紧急事故 (Hotfix)

**目标**：线上 `v1.1` 版本支付功能崩溃，需要立即修复，不能等待下个版本。

1. **起点**：从 `master` 分支（即线上代码）直接检出。
2. **过程**：修复 Bug。
3. **终点**：
	- 合并回 `master` 并打补丁版本 Tag（如 `v1.1.1`）。
	- **同时**合并回 `develop`（防止下个版本又出现该 Bug）。

```bash
# 1. 从 master 建立热修复分支
git checkout -b hotfix/v1.1.1 master

# 2. 紧急修复
git commit -m "fix: critical payment gateway timeout"

# 3. 合并回 master
git checkout master
git merge --no-ff hotfix/v1.1.1
git tag -a v1.1.1 -m "Hotfix v1.1.1"

# 4. 合并回 develop
git checkout develop
git merge --no-ff hotfix/v1.1.1

# 5. 删除分支
git branch -d hotfix/v1.1.1
```

---

## 3. 分支行为对比表

| 分支类型 | 来源 (Checkout from) | 归宿 (Merge to) | 命名规范示例 | 作用域 |
| :--- | :--- | :--- | :--- | :--- |
| **Feature** | develop | develop | `feature/user-login` | 开发具体功能 |
| **Release** | develop | master & develop | `release/v1.0` | 冻结代码，预发布测试 |
| **Hotfix** | master | master & develop | `hotfix/v1.0.1` | 生产环境紧急修复 |
| **Develop** | master (初始) | N/A | `develop` | 永远的 " 下个版本 " |
| **Master** | N/A | N/A | `master` / `main` | 永远的 " 生产环境 " |

---

## 4. 适用场景与局限性

### 适合场景

- **版本化产品**：手机 App、PC 客户端、嵌入式固件。
- **发布周期长**：一个月甚至一个季度发布一次大版本。
- **团队规模**：中大型团队，需要严格的代码准入控制。

### 不适合场景

- **Web SaaS / 持续部署**：如果你的代码每天要上线 10 次（如 GitHub、Facebook），Git Flow 过于繁琐。
- **CI/CD 高度自动化**：Git Flow 的手动合并步骤较多，容易阻碍自动化流水线。

---

## 5. 最佳实践总结

1. **使用辅助工具**：记不住命令没关系，安装 `git-flow` 插件（通常集成在 Sourcetree 或 via `brew install git-flow`），它能用简单的 `git flow feature start xxx` 替代一连串命令。
2. **No-FF 是精髓**：合并时始终保留 `--no-ff`，这能让 Git 历史像一串葡萄一样结构清晰，而不是乱成一团的面条。
3. **Develop 必须能跑**：不要将编译不通过的代码提交到 `develop`，它是团队的公共地基。
4. **Tag 这里的版本号**：遵循语义化版本规范。
