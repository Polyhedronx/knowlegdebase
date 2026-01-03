---
created: 2026-01-01 14:25
updated: 2026-01-03 22:11
tags:
  - markdown
status: seed
aliases:
  - markdown语法
---

返回 [计算机基础能力](../30_Maps/计算机基础能力.md)

## 1. 列表、任务、引用

```markdown
- 无序列表
  - 子项
1. 有序列表
2. 第二项

- [x] 已完成
- [ ] 待完成

> 这是引用
```

效果：

- 无序列表
	- 子项

1. 有序列表
2. 第二项

- [x] 已完成
- [ ] 待完成

> 这是引用

---

## 2. 代码块 & 高亮

```bash
# Bash 示例
git add .
git commit -m "message"
```

```python
def hello():
    print("Hello Markdown")
```

常见语言高亮：bash, python, javascript, json, yaml, html, css, markdown

---

## 3. 表格

```markdown
| 功能     | 命令        | 说明          |
|----------|-------------|---------------|
| 提交代码 | git commit  | 保存一次修改  |
| 查看历史 | git log     | 显示提交历史  |
```

效果：

| 功能   | 命令         | 说明     |
| ---- | ---------- | ------ |
| 提交代码 | git commit | 保存一次修改 |
| 查看历史 | git log    | 显示提交历史 |

---

## 4. 链接 & 图片

```markdown
[Git 官网](https://git-scm.com/)
![图片示例](https://octodex.github.com/images/codercat.jpg)
```

[Git 官网](https://git-scm.com/)

![图片示例](https://octodex.github.com/images/codercat.jpg)

---

## 5. 分隔线 & 脚注

```markdown
---
这是分隔线

文字带脚注[^1]

[^1]: 这是脚注说明
```

效果：

---

这是分隔线

文字带脚注 [^1]

---

## 6. Markdown 扩展

GitHub、GitLab 等支持更丰富的语法：

- ✅ **任务列表**
- 🚀 **emoji**
- ~~删除线~~
- `diff 高亮修改`

```diff
+ 新增内容
- 删除内容
```

---

## 7. 文档编写规范

1. **开头写目录**

```markdown
# 项目文档

## 目录
- [安装](#安装)
- [使用方法](#使用方法)
- [贡献指南](#贡献指南)
```

1. **分块清晰**

- 安装步骤
- 使用示例
- 常见问题（FAQ）
- 开发/贡献指南

1. **示例优于解释**
	多给命令示例、运行截图，而不是只写概念。

2. **README 最小可用**
	别人一看就能跑起来，细节放到 `docs/`

---

## 8. 实战场景

- **README.md**：项目简介、安装、示例
- **CHANGELOG.md**：记录版本更新
- **CONTRIBUTING.md**：贡献指南
- **Wiki / docs/**：写详细文档
- 数学公式语法借用了 LaTeX 的语法，详见：[[Markdown 数学公式]]

[^1]: 这是脚注说明
