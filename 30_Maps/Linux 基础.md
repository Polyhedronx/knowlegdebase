---
created: 2026-01-04 20:53
updated: 2026-01-04 20:58
tags:
  - Linux
status: seed
aliases:
---

> **核心目标**：脱离图形界面（GUI），熟练掌握终端（Terminal）操作，理解 Linux 文件系统与权限模型，具备基础的服务器维护能力。

---

## 0. 认识 Linux 与环境搭建

*建立对 Linux 的基本认知，不再害怕黑窗口。*

- [[Linux 哲学与历史]] - 为什么服务器都用 Linux？一切皆文件（Everything is a file）。
- [[Linux 发行版选择]] - Ubuntu (Debian 系) vs CentOS/Rocky (RHEL 系) vs Alpine 的区别。
- [[Linux 目录结构 (FHS)]] - **必读**。理解 `/`, `/home`, `/etc`, `/var`, `/bin` 等目录分别存什么。
- [[Shell 是什么]] - Bash 与 Zsh 的区别，配置 `.bashrc` 或 `.zshrc`。

---

## 1. 文件与目录管理

*在系统中走动和搬运东西。*

### 基础操作

- [[路径导航]] - 相对路径 vs 绝对路径，`cd`, `pwd`, `ls` (常用参数 `-l`, `-a`, `-h`)。
- [[文件增删改查]] - `touch`, `mkdir`, `cp` (复制), `mv` (移动/重命名), `rm` (删除及其危险性 `rm -rf`).
- [[查看文件内容]] - `cat` (小文件), `less` (大文件分页), `head` & `tail` (查看头尾，重点：`tail -f` 查看实时日志)。

### 文件查找与归档

- [[文件搜索]] - `find` (按时间、大小、名称查找), `locate` (快速查找)。
- [[压缩与解压]] - `tar` (最常用 `xvzf`/`cvzf`), `zip`/`unzip`。

---

## 2. 权限与用户管理

*Linux 安全的核心，解决 "Permission denied" 报错的根源。*

- [[理解 Linux 权限]] - `rwx` (读写执行) 的含义，数字表示法 (755, 644)。
- [[修改权限与所有者]] - `chmod` (改权限), `chown` (改所有者)。
- [[用户与用户组管理]] - `useradd`, `usermod`, `passwd`, `/etc/passwd` 文件解析。
- [[Sudo 权限配置]] - `visudo`，如何让普通用户拥有管理员权限。

---

## 3. 文本处理与编辑器

*Linux 的精髓在于处理文本流。*

- [[Vim 极速入门]] - 模式切换（普通/插入/命令），保存退出 (`:wq`)，基本移动与搜索。
- [[Nano 基础]] - 新手友好的简单编辑器。
- **流处理入门**：
	- [[Grep 文本搜索]] - 过滤特定关键词 (`grep -r`).
	- [[Sed 基础替换]] - 批量替换文本内容。
	- [[Awk 基础列处理]] - 提取日志中的特定列。
- [[管道与重定向]] - `|` (管道), `>` (覆盖), `>>` (追加) 的妙用。

---

## 4. 软件安装与包管理

- [[APT 包管理 (Ubuntu/Debian)]] - `apt update`, `apt install`, `apt upgrade`.
- [[YUM/DNF 包管理 (CentOS/RHEL)]] - 常用命令。
- [[源码编译安装 (Make)]] - 遇到没有包的情况怎么办 (`./configure`, `make`, `make install`)。

---

## 5. 系统管理与进程监控

*服务器为什么卡了？*

- [[进程管理]] - `ps -ef` (查看进程), `top`/`htop` (实时监控资源), `kill`/`killall` (结束进程)。
- [[服务管理 (Systemd)]] - `systemctl` 常用命令 (start, stop, enable, status)。
- [[磁盘与内存检查]] - `df -h` (磁盘空间), `du -sh` (文件夹大小), `free -h` (内存使用)。
- [[查看系统信息]] - `uname -a`, `lscpu`, `whoami`.

---

## 6. 网络基础与远程连接

- [[SSH 远程登录]] - `ssh` 命令，免密登录配置 (`ssh-keygen`, `ssh-copy-id`)。
- [[网络连接与端口]] - `ip addr` (看 IP), `ping` (测连通), `netstat`/`ss` (看端口占用)。
- [[网络下载与请求]] - `curl` (API 测试神器), `wget` (下载文件)。
- [[防火墙基础]] - `ufw` (Ubuntu) 或 `firewalld` (CentOS) 简单开关端口。

---

## 7. Shell 脚本入门

*开始自动化的第一步。*

- [[Shebang 与执行]] - `#!/bin/bash`，执行权限。
- [[变量与环境变量]] - 局部变量 vs `export` 环境变量。
- [[基本逻辑控制]] - `if` 判断，`for` 循环。
- [[Cron 定时任务]] - `crontab -e` 语法与配置。

---

### 常见问题与实战场景

- [[如何排查磁盘空间不足]] (`du` 排查大文件)
- [[如何排查 CPU 100% 占用]]
- [[忘记 Root 密码怎么办]]
- [[实战：部署一个 Nginx 静态站]]
