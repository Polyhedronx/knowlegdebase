---
created: 2026-01-01 14:40
updated: 2026-01-02 14:42
tags:
  - ssh
  - 计算机网络
status: seed
aliases:
---

返回 [[../30_Maps/计算机基础能力|计算机基础能力]]

### 1. 基本概念

- **SSH (Secure Shell)**：安全远程登录协议，可以在本地终端控制远程服务器。
- 作用：加密传输，避免明文密码被窃听。

---

### 2. 登录命令

```bash
ssh 用户名@服务器IP
# 示例
ssh root@192.168.1.100      # 默认连接 22 端口
ssh -p 2222 root@1.1.1.1    # 连接非标准端口（如 2222）
ssh -i key.pem root@ip      # 使用密钥文件登录（而不是密码）
ssh user@ip "ls -la"        # 不登录进去，直接在远程执行命令并返回结果
```

- 第一次会提示 " 是否信任该主机（yes/no）"，然后保存服务器公钥到本地。
- 输入密码后进入远程终端。

---

### 3. 基于密钥的免密登录

1. 生成密钥：

```bash
ssh-keygen -t ed25519 -C "你的注释"
```

默认会在 `~/.ssh/id_ed25519`（私钥）、`id_ed25519.pub`（公钥）。

1. 把公钥传到服务器：

```bash
ssh-copy-id 用户名@服务器IP
```

或者手动追加 `id_ed25519.pub` 内容到远程服务器的 `~/.ssh/authorized_keys` 文件。

1. 下次登录就不用输入密码了。

---

### 4. 在 VSCode 中的实践

1. 安装插件：**Remote - SSH**
2. 添加远程服务器配置（编辑 `~/.ssh/config`）：

```txt
  Host myserver
    HostName 192.168.1.10
    User root
    IdentityFile ~/.ssh/id_ed25519
```

1. 在 VSCode 左下角 → 远程连接 → 选择 `myserver` → 就能直接在 VSCode 里操作远程服务器文件和终端。
