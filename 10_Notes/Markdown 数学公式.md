---
created: 2026-01-01 14:26
updated: 2026-01-03 22:12
tags:
  - Markdown
status: seed
aliases:
  - markdown语法
---

返回 [计算机学习](../30_Maps/计算机学习.md)

## 1. 行内公式

在文字里插入数学符号：

```markdown
爱因斯坦方程：$E = mc^2$
```

效果：爱因斯坦方程：$E = mc^2$

---

## 2. 独立公式

单独一行公式用 `$$ … $$`

```markdown
$$
\int_a^b f(x)\,dx = F(b) - F(a)
$$
```

效果：

$$
  
\int_a^b f(x),dx = F(b) - F(a)  
$$

---

## 3. 常用公式语法（LaTeX 写法）

- 上标 / 下标：`x^2` → $x^2$，`x_{i}` → $x_{i}$
- 分数：`\frac{a}{b}` → $\frac{a}{b}$
- 根号：`\sqrt{x}` → $\sqrt{x}$
- 求和：`\sum_{i=1}^n i` → $\sum_{i=1}^n i$
- 积分：`\int_a^b f(x)\,dx` → $\int_a^b f(x),dx$
- 希腊字母：`\alpha, \beta, \pi` → $\alpha, \beta, \pi$

---

## 4. 矩阵 / 方程组

```markdown
$$
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
$$
```

$$
  
\begin{bmatrix}  
1 & 2 \\
3 & 4  
\end{bmatrix}  
$$

对齐推导：

```markdown
$$
\begin{align}
a &= b + c \\
  &= d + e
\end{align}
$$
```

$$
  
\begin{align}  
a &= b + c \\  
&= d + e  
\end{align}  
$$
