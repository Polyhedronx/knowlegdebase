---
created: 2026-01-01 15:21
updated: 2026-01-02 23:19
tags:
  - linux
  - DevOps
status: seed
aliases:
---

返回 [Linux 基础](../30_Maps/Linux%20基础.md)

## 1. `man` (manual)

功能：用来查看系统自带的手册页。

```bash
man ls
```

显示 `ls` 命令的详细用法。想查看其它命令只需模仿该写法。

---

## 2. `echo`

功能：输出文本。

常见用法：

```bash
echo "Hello World"
echo $HOME        # 输出环境变量
echo -n "No newline"   # 不换行
echo -e "Line1\nLine2" # 解析转义字符
```

---

## 3. `printf`

功能：类似 C 语言的 `printf` 支持格式化，比 `echo` 更强大可控。

常见用法：

```bash
printf "Hello %s!\n" "World"
printf "Number: %d, Float: %.2f\n" 42 3.14159
printf "%-10s %-8s\n" Name Age
printf "%-10s %-8s\n" Alice 20
printf "%-10s %-8s\n" Bob   25
```

输出结果（表格对齐）：

```bash
Name       Age     
Alice      20      
Bob        25      
```

---

## 4. `date`

功能：显示或设置系统日期时间。

常见用法：

```bash
date             # 显示当前时间
date "+%Y-%m-%d %H:%M:%S"   # 格式化输出，年-月-日 时:分:秒
date -u          # 以 UTC 时间显示
```

---

## 5. `cal`

功能：显示日历。

常见用法：

```bash
cal           # 显示本月日历
cal 2025      # 显示 2025 年整年的日历
cal 10 2025   # 显示 2025 年 10 月的日历
```

输出示例：

```css
    October 2025
Su Mo Tu We Th Fr Sa
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28 29 30 31
```

- 快速查看日历（不用打开图形界面）。
- 在排班、计划脚本里获取月份。

---

## 6. `clear`

功能：清除终端上的输出，屏幕恢复到空白状态，本质上就是输出很多换行符，把旧内容推上去。

---

## 7. `info`

功能：比 `man` 更全面，适合学习命令的详细用法。
