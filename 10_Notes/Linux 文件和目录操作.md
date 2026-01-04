---
created: 2026-01-01 15:17
updated: 2026-01-02 23:22
tags:
  - Linux
  - DevOps
status: seed
aliases:
---

返回 [Linux 命令速查](../30_Maps/Linux%20命令速查.md)

## 1. `pwd`

功能：告诉你在哪（Print Working Directory）。

```bash
pwd        # 输出如：/home/user/project
```

---

## 2. `ls`

功能：列出目录内容。

常用组合：

```bash
ls -a           # 显示所有文件（包括以 . 开头的隐藏文件，如 .git）
ls -l           # 列表显示（详细信息：权限、所有者、大小、时间）
ls -lh          # 列表显示 + 人类可读的大小（显示 1K, 23M, 1G 而不是字节数，常用）
ls -lah         # 上述三个的组合
```

---

## 3. `cd`

功能：切换目录（Change Directory）。

常见用法：

```bash
cd /var/log     # 绝对路径跳转
cd ..           # 返回上一级
cd -            # 回到上一次所在的目录（类似电视遥控器的“回看”键）
cd ~            # 回到当前用户的主目录（home）
```

---

## 4. `cat`

功能：查看小文件内容，或将多个文件 " 连接 " 输出。

常见用法：

```bash
cat config.json             # 查看文件内容
cat file1.txt file2.txt     # 依次显示 file1 和 file2
cat file.txt | grep "error" # 配合管道符，搜索文件中的关键词
```

---

## 5. `less`

功能：分页查看大文件（比 `cat` 安全，不会刷屏）。

常用交互：

```bash
less big_log.log
# 操作键：
# 空格 / f   : 下一页
# b          : 上一页
# /keyword   : 向下搜索关键词（按 n 跳转下一个）
# q          : 退出
```

---

## 6. `mkdir`

功能：创建目录。

常见用法：

```bash
mkdir code                  # 建一个文件夹
mkdir -p project/src/main   # 递归创建。如果 project 不存在，会自动创建它，不会报错
```

---

## 7. `cp`

功能：复制文件或目录。

常见用法：

```bash
cp config.yml config.yml.bak    # 备份文件
cp -r src/ dst/                 # 递归复制整个目录（复制文件夹必须加 -r）
```

---

## 8. `mv`

功能：移动文件（剪切）或重命名。

常见用法：

```bash
mv old_name.txt new_name.txt    # 重命名
mv *.log /var/logs/             # 把所有 log 文件移动到指定目录
```

---

## 9. `rm`

功能：删除文件或目录（Linux 没有回收站，**删了就真没了**）。

常见用法：

```bash
rm file.txt                 # 删除文件
rm -r build/                # 删除目录（recursive）
rm -rf /tmp/*               # 强制删除目录下的所有内容，不提示
# 建议加 -i 参数 (rm -i)，删除前会询问，防止手滑
```

---

## 10. `head` / `tail`

功能：只看头几行 / 只看尾几行。

常见用法：

```bash
head -n 5 data.csv          # 查看文件前 5 行（确认数据格式时常用）
tail -n 100 error.log       # 查看日志最后 100 行
tail -f access.log          # 实时滚动监视文件尾部（用于排查正在发生的服务器日志）
```

---

## 11. `touch`

功能：新建空文件 / 更新文件时间戳。

常见用法：

```bash
touch README.md             # 如果文件不存在，就新建一个空文件
touch existing_file.txt     # 如果文件存在，就只更新它的“修改时间”（常用于触发构建工具检测）
```
