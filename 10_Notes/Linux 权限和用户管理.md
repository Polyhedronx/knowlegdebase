---
created: 2026-01-01 15:13
updated: 2026-01-02 23:22
tags:
  - Linux
  - DevOps
status: seed
aliases:
---

返回 [Linux 命令速查](../30_Maps/Linux%20命令速查.md)

## 1. `chmod`

功能：修改文件权限（Change Mode）。

常见用法：

```bash
chmod +x script.sh          # 给文件加上“可执行”权限（运行脚本前必备）
chmod 755 dist/             # 设置目录权限为 rwxr-xr-x（最常用的 Web 目录权限）
chmod -R 777 public/        # 慎用：递归赋予所有人读写执行权限（暴力解决权限问题，不安全）
```

---

## 2. `chown`

功能：修改文件所有者（Change Owner）。

常见用法：

```bash
chown alice file.txt        # 把文件主人改为 alice
chown alice:dev file.txt    # 同时修改主人为 alice，所属组为 dev
chown -R www:www /var/www   # 常用：递归修改整个目录的主人和组（部署网站时常用）
```

---

## 3. `chgrp`

功能：仅修改文件的所属组。

> 小技巧：通常习惯直接用 `chown :dev file` 来代替它。

常见用法：

```bash
chgrp docker docker.sock    # 把文件归属到 docker 组
```

---

## 4. `whoami`

功能：我是谁？显示当前登录的用户名。

常见用法：

```bash
whoami
# 输出：root 或 ubuntu
# 场景：脚本里判断当前是不是 root 用户；或者切换多次用户后确认身份。
```

---

## 5. `id`

功能：显示用户的详细身份信息（UID、GID、属于哪些组）。

常见用法：

```bash
id                          # 看自己的信息
id nginx                    # 看 nginx 用户属于哪些组
# 场景：你明明是 admin 却无法运行 docker？用 id 看看你是不是没在 docker 组里。
```

---

## 6. `su`

功能：切换用户身份（Substitute User）。

常见用法：

```bash
su -                        # 推荐：切换到 root，并加载 root 的环境变量
su - postgres               # 切换到 postgres 用户
# 注意：一定要带横杠 (-)，否则环境变量还是旧用户的，容易导致命令报错。
```

---

## 7. `sudo`

功能：以管理员（root）权限执行一条命令。

常见用法：

```bash
sudo apt update             # 普通用户安装软件必须加 sudo
sudo -i                     # 获取一个 root 的交互式 shell（不用输多次 sudo）
sudo !!                     # 刚才那条命令忘了加 sudo？输入这个直接重跑，不用重新打字
```
