---
created: 2026-01-01 15:19
updated: 2026-01-02 23:23
tags:
  - Linux
  - DevOps
status: seed
aliases:
---

返回 [Linux 命令速查](../30_Maps/Linux%20命令速查.md)

## 1. `grep`

功能：文本搜索（Global Regular Expression Print）。

常见用法：

```bash
grep "error" app.log        # 在文件中搜 "error"
grep -i "error" app.log     # 忽略大小写（Error, ERROR 都能搜到）
grep -r "config" ./         # 常用：递归搜索当前目录下所有包含 "config" 的文件
grep -v "info" app.log      # 反向搜索（排除包含 info 的行）
ps aux | grep nginx         # 经典组合：在进程列表中搜 nginx
```

---

## 2. `find`

功能：文件查找（按属性找文件）。功能最强，但速度取决于硬盘扫描速度。

常见用法：

```bash
find /etc -name "hosts"     # 按名字查找
find . -name "*.log"        # 查找当前目录所有的 log 文件
find / -size +100M          # 常用：查找大于 100M 的大文件
find . -type f              # 只找文件（忽略目录）
```

---

## 3. `locate`

功能：极速查找文件（查索引）。比 `find` 快很多，但新建的文件可能搜不到。

常见用法：

```bash
locate nginx.conf           # 瞬间找到所有叫 nginx.conf 的路径
sudo updatedb               # 手动更新索引（如果找不到刚才建的文件，敲一下这个）
```

---

## 4. `cut`

功能：简单粗暴的 " 按列提取 " 文本。

常见用法：

```bash
# -d 指定分隔符 (delimiter)，-f 取第几列 (field)
cut -d ':' -f 1 /etc/passwd # 提取系统所有用户名（以冒号分隔的第一列）
```

---

## 5. `sort`

功能：给文本内容排序。

常见用法：

```bash
sort names.txt              # 默认按字典序排序 (A-Z)
sort -n numbers.txt         # 按数值大小排序 (Number)
sort -r numbers.txt         # 倒序排列 (Reverse)
sort -k 2 data.txt          # 按第二列内容排序
```

---

## 6. `uniq`

功能：去重（Unique）。**注意：它只能去掉 " 相邻 " 的重复行，所以必须配合 `sort` 使用。**

常见用法：

```bash
sort data.txt | uniq        # 排序并去重
sort data.txt | uniq -c     # 常用：统计每一行出现的次数（词频统计）
sort data.txt | uniq -d     # 只把重复的行找出来
```

---

## 7. `wc`

功能：统计数量（Word Count）。

常见用法：

```bash
wc -l file.txt              # 常用：统计有多少行 (Line)
ls | wc -l                  # 统计当前目录下有多少个文件
wc -c file.txt              # 统计字节数 (Size)
```

---

## 8. `diff`

功能：比较两个文本文件的差异。

常见用法：

```bash
diff file1.txt file2.txt    # 比较文件
diff -u file1 file2         # 以 Unified 格式显示（Git diff 同款格式，更易读）
diff -r dir1/ dir2/         # 比较两个文件夹里的差异
```

---

## 9. `cmp`

功能：逐字节比较文件。主要用于比较**二进制文件**（如图片、程序）是否完全一致。

常见用法：

```bash
cmp prog_v1.bin prog_v2.bin
# 如果没有任何输出，说明两个文件一模一样。
# 如果不一样，会提示：differ: byte 5, line 1
```
