---
created: 2026-01-05 00:27
updated: 2026-01-05 00:30
tags:
  - Docker
status: seed
aliases:
---

## 0. 配置文件位置

- **Linux**：`/etc/docker/daemon.json`
- **Docker Desktop (Windows/Mac)**：在 Docker Desktop 的图形界面中配置 (Settings -> Docker Engine)。

---

## 1. 镜像加速器 (Registry Mirrors)

这是最常用也最重要的一项配置，用于解决从 Docker Hub 拉取镜像慢的问题。

**目的**：配置一个或多个国内的镜像仓库，作为 Docker Hub 的 "CDN"。

**配置示例 (`daemon.json`)**：

```json
{
  "registry-mirrors": [
    "https://mirror.ccs.tencentyun.com",
    "https://hub-mirror.c.163.com",
    "https://mirrors.aliyun.com/docker-ce/linux/centos/"
  ]
}
```

- **格式**：一个 JSON 对象，包含一个 `"registry-mirrors"` 键，其值为一个包含镜像源 URL 字符串的数组。
- **注意**：
	- 如果你的 Linux 系统是基于 CentOS/Alinux，并且已使用 `yum-config-manager --add-repo` 添加了官方或阿里云的 `docker-ce.repo`，那么通常**不需要**再单独配置 `registry-mirrors`，`yum` 安装时会自动配置好。但手动配置更明确。
	- Docker Desktop 在 GUI 中添加即可，无需手动编辑文件。

**生效方式**：修改后需要重启 Docker 服务。
- Linux: `sudo systemctl restart docker`
- Docker Desktop: 点击 "Apply & restart"

---

## 2. 数据目录 (Data Root)

指定 Docker 存储镜像、容器、卷等数据的位置。

**场景**：
- 系统盘空间不足，想将 Docker 数据迁移到大容量盘。
- 在虚拟机环境中，挂载了单独的数据盘。

**配置示例 (`daemon.json`)**：

```json
{
  "data-root": "/mnt/docker-data"
}
```

**重要**：
- 执行此配置前，必须**停止 Docker 服务**。
- 将 `/var/lib/docker`（默认路径）下的所有内容**手动迁移**到新的 `/mnt/docker-data` 目录。
- 迁移完成后，再启动 Docker 服务。
- `/mnt/docker-data` 目录必须存在且 Docker 进程有读写权限。

**生效方式**：修改后需要重启 Docker 服务。

---

## 3. 网络配置 (Bip, Ipv6, Fixed-cidr)

Docker 默认会创建一个名为 `docker0` 的网桥，并自动分配 IP 地址。这些配置可以进行微调。

- `bip`: 指定 `docker0` 网桥的 IP 地址和子网掩码。

	```json
    {
      "bip": "192.168.99.1/24"
    }
    ```

- `ipv6`: 启用 IPv6 支持。

	```json
    {
      "ipv6": true
    }
    ```

- `fixed-cidr`: 为容器分配静态 IP 的 CIDR 地址段（不推荐，通常用 `--ip` 或 Docker Network 更好）。

	```json
    {
      "fixed-cidr": "10.10.0.0/16"
    }
    ```

- **组合配置**：

	```json
    {
      "bip": "192.168.99.1/24",
      "ipv6": true,
      "fixed-cidr-v6": "fd00::/64"
    }
    ```

**生效方式**：修改后需要重启 Docker 服务。

---

## 4. 日志驱动 (Log Driver)

默认情况下，Docker 容器的日志会通过 `docker logs` 命令查看（使用 `json-file` 驱动）。在生产环境中，通常会配置其他驱动将日志发送到集中的日志系统。

**常用驱动**：
- `json-file` (默认)：保存为 JSON 文件。
- `syslog`：发送到 syslog 服务器。
- `journald`：发送到 systemd journal。
- `fluentd`：发送到 Fluentd 收集器。
- `awslogs`：发送到 AWS CloudWatch Logs。

**配置示例 (`daemon.json`)**：将日志发送到 syslog 服务器。

```json
{
  "log-driver": "syslog",
  "log-opts": {
    "syslog-address": "tcp://192.168.1.100:514",
    "tag": "docker/{{.Name}}/{{.ID}}"
  }
}
```

- `log-driver`: 指定日志驱动的名称。
- `log-opts`: 驱动的特定选项。`syslog-address` 是 syslog 驱动的必填项。`tag` 可以自定义日志标签。

**生效方式**：修改后需要重启 Docker 服务。
- **注意**：此配置是全局的，会影响所有新建容器的日志处理方式。也可以在 `docker run` 时通过 `--log-driver` 和 `--log-opt` 参数为单个容器指定。

---

## 5. 凭证帮助程序 (Credential Helpers)

用于安全地存储和检索 Docker 仓库的登录凭证（用户名/密码），而不是将它们明文保存在本地。

**配置示例 (`daemon.json`)**：使用 `store` 凭证助手（会将凭证存储在本地凭证管理器，如 Linux 的 `~/.docker/config.json` 或 macOS 的 Keychain）。

```json
{
  "cred-helpers": [
    "store"
  ]
}
```

**更安全的选项**：`ecr-login` (AWS ECR)、`wincred` (Windows Credential Manager) 等。

**生效方式**：修改后需要重启 Docker 服务。首次使用时，Docker 会提示你输入凭证，并由助手进行保存。

---

## 6. 常用配置的合并

`daemon.json` 文件是 JSON 格式，所有配置项都写在一个文件中。

**完整示例 (`daemon.json`)**：

```json
{
  "registry-mirrors": [
    "https://mirror.ccs.tencentyun.com",
    "https://hub-mirror.c.163.com"
  ],
  "data-root": "/mnt/docker-data",
  "log-driver": "syslog",
  "log-opts": {
    "syslog-address": "tcp://192.168.1.100:514",
    "tag": "docker/{{.Name}}"
  },
  "bip": "192.168.99.1/24"
}
```

**总结**：
- `/etc/docker/daemon.json` 是 Docker 守护进程的**全局配置文件**。
- 大部分修改需要**重启 Docker 服务**才能生效。
- Docker Desktop 用户通过 GUI 配置，本质上也是修改其内部的 `daemon.json`。
