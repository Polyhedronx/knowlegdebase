---
created: 2026-01-01 15:09
updated: 2026-01-02 23:20
tags:
  - linux
  - DevOps
status: seed
aliases:
---

返回 [linux 基础](../30_Maps/linux%20基础.md)

## 1. 概览

- APT（Advanced Package Tool） 是 Debian / Ubuntu 的包管理系统的总体名称。
- **`apt-get`**：传统、稳定、脚本友好（底层工具，适合脚本 / 自动化）。
- **`apt`**：较新的交互式前端，整合了 `apt-get` 和 `apt-cache` 的常用功能，输出更友好（进度条、颜色），适合人工交互使用，但不推荐在脚本中替代 `apt-get`（因为 `apt` 输出和行为可能为交互优化）。

---

## 2. 核心概念

1. **`sudo apt update`** — 刷新包索引（把仓库里最新的软件包列表取下来）。
2. **`sudo apt upgrade`** — 升级已安装的包（不会做会导致依赖更改或移除的操作）。
3. **`sudo apt full-upgrade`**（等价 `apt-get dist-upgrade`）— 更激进的升级，可安装/移除包以满足依赖。
4. **`sudo apt install <pkg>`** — 安装软件。
5. **`sudo apt remove <pkg>`** — 卸载保留配置文件。
6. **`sudo apt purge <pkg>`** — 卸载并删除配置文件（彻底清理）。
7. **`sudo apt autoremove`** — 自动移除不再需要的依赖包。
8. **`sudo apt clean` / `sudo apt autoclean`** — 清理本地下载的包缓存（释放磁盘）。

---

## 3. 常用命令

```bash
# 刷新索引（总是先做这步）
sudo apt update

# 查看可更新包
apt list --upgradable

# 升级所有包（交互）
sudo apt upgrade

# 升级并自动处理依赖（会安装/删除包以满足）
sudo apt full-upgrade

# 安装包
sudo apt install curl git

# 卸载（保留配置）
sudo apt remove package-name

# 卸载并删除配置
sudo apt purge package-name

# 自动移除无人依赖的包
sudo apt autoremove

# 修复依赖（当 apt 报错时常用）
sudo apt --fix-broken install

# 清理缓存
sudo apt autoclean   # 删除旧的包缓存
sudo apt clean       # 删除全部缓存 .deb 文件

# 查询包信息 / 状态
apt show package-name
apt policy package-name
dpkg -l | grep package-name

# 搜索包（交互）
apt search keyword

# 在脚本中建议使用（更稳定）
sudo apt-get update
sudo apt-get install -y package-name
sudo apt-get upgrade -y
```

---

## 4. `apt` vs `apt-get`

- 交互式使用：用 `apt`（更友好、带进度）。
- 脚本 / 自动化：用 `apt-get` 和 `apt-cache`（行为更稳定、向后兼容）。
- 细粒度选项（某些老脚本或文档提到）：仍用 `apt-get`（例如 `apt-get -o` 等特定选项）。

---

## 5. 常见问题

- **先 `update` 再 `install`**：很多初学者直接 `apt install` 会因索引过期失败。
- **`upgrade` vs `full-upgrade`（或 `dist-upgrade`）**：`upgrade` 不会移除已安装的包；`full-upgrade` 会在必要时移除或安装包来满足依赖（用于重大更新）。
- **`autoremove` 安全使用**：先看 `apt autoremove --dry-run`（或先 `apt autoremove` 观察提示），确认不会误删你想要的包。
- **磁盘清理**：`sudo apt clean` 会把 `/var/cache/apt/archives` 下的 `.deb` 全删，能回收空间；`autoclean` 只删旧版本。
- **修复损坏依赖**：`sudo apt --fix-broken install`（或 `sudo apt-get -f install`）常用于修复半安装/冲突状态。
- **保持备份**：做系统级升级或 `full-upgrade` 前，若是生产环境或重要服务器，先备份或在可回滚环境里测试。

---

## 6. 仓库、密钥与第三方源

- Apt 使用 **仓库（repositories）**，配置文件在：
	- `/etc/apt/sources.list`
	- `/etc/apt/sources.list.d/`（每个第三方源通常一个 `.list` 文件）
- 仓库通常用 GPG 签名保证包来源可信。**不要随意添加不可信的仓库或密钥**。
- 旧方法 `apt-key add` 已被标记为不推荐（会把密钥放到全局 keyring）。推荐做法是把第三方公钥放到 `/usr/share/keyrings/` 并在 `.list` 文件中使用 `signed-by=/usr/share/keyrings/xxx.gpg`。（这是安全实践，也是现代发行版推荐的方式。）
- 在 Ubuntu 上，常用 `sudo add-apt-repository ppa:xxx/yyy` 快速添加 PPA（背后会处理 key 和 sources）。

---

## 7. 进阶命令

- `apt policy <pkg>`：查看版本来源、优先级和已安装版本。
- `apt show <pkg>`：显示包描述、依赖、维护者、大小等信息。
- `dpkg -l | grep <pkg>`：低层次查看安装状态。
- `apt-mark hold <pkg>` / `apt-mark unhold <pkg>`：锁定某个包不要被升级（hold）。
- `apt list --installed`：列出已安装包。

---

## 8. 安全与最佳实践

- 只给受信任的源添加密钥与仓库。
- 生产环境先在预生产或容器里做一次 `apt update` + `full-upgrade` 测试。
- 脚本中尽量使用 `apt-get`（抗变更），并写好错误处理（检查返回值）。
- 遇到包冲突，先读错误提示，不要盲目 `--force-yes` 或随意删除关键包。

---

## 9. 小练习

在系统里（推荐在非关键机器或 WSL / 虚拟机上），可以按顺序跑下面三条命令来熟悉流程：

```bash
sudo apt update
apt list --upgradable
sudo apt upgrade
```

这会：刷新索引 → 列出可升级包 → 升级。观察输出，注意哪些包被更新、是否需要 `full-upgrade`。
