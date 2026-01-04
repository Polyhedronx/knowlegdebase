---
created: 2026-01-04 20:31
updated: 2026-01-04 20:36
tags:
  - Cooperation
  - DevOps
status: seed
aliases:
---

## 0. 概念

Secrets 管理是指在软件开发生命周期中，对**敏感凭据**（Authentication Credentials）进行安全存储、分发、使用和销毁的系统化流程。

**什么是 Secrets？**

不仅仅是用户密码，还包括：

- API 密钥（如 Stripe, AWS Access Keys）
- 数据库凭据（Database Credentials）
- 私有证书和 SSH 密钥
- 加密令牌（Encryption Tokens）

**核心场景：**

想象一名开发人员将 AWS 的 `Access Key` 硬编码在 Python 脚本中，并将其推送到 GitHub 公共仓库。几秒钟内，扫描机器人就会发现该密钥，并利用它开启数千台昂贵的 EC2 实例挖矿。Secrets 管理的核心目的就是**杜绝凭据进入代码仓库**。

---

## 1. 原则

在设计系统安全架构时，必须遵循以下四大原则：

### 代码与配置分离

**原则**：代码仓库（Git）中**永远**不应包含任何真实的 Secrets。

**实践**：代码只定义逻辑，Secrets 应当作为环境配置注入。即使是开发环境的测试密码，也不应提交到版本控制中。

### 最小权限原则

**原则**：应用程序只应获取其运行所需的**最小范围**和**最短时间**的权限。

**实践**：
- 不要给 Web 服务器使用 `root` 数据库账号，而应创建一个仅有 `SELECT` 和 `INSERT` 权限的账号。
- 不同环境（Dev, Staging, Prod）必须使用完全不同的 Secrets。

### 集中化管理

**原则**：避免 Secrets 散落在 Wiki、Slack、邮件或分散的配置文件中。

**实践**：使用专门的 **Secrets Store**（如 HashiCorp Vault, AWS Secrets Manager）作为单一可信源（SSOT）。

### 动态轮换

**原则**：假设任何 Secret 最终都会泄露，因此必须定期更换。

**实践**：静态密码（长期有效）是安全隐患。理想状态是使用动态 Secrets（例如，应用启动时申请一个有效期仅 30 分钟的数据库临时凭据）。

---

## 2. 技术实现方案

根据项目规模和安全需求，通常有以下分层实现方式：

| 方案层级 | 适用场景 | 实现方式 | 优缺点 |
| :--- | :--- | :--- | :--- |
| **基础层** | 本地开发 | `.env` 文件 + `.gitignore` | **优**：简单快捷<br>**缺**：难以同步，易误提交 |
| **进阶层** | 容器化部署 | Docker Secrets / K8s Secrets | **优**：与编排工具集成<br>**缺**：K8s Secrets 默认仅 base64 编码，非真正加密 |
| **企业层** | 生产环境 | HashiCorp Vault / Cloud Providers | **优**：动态轮换、审计日志、强加密<br>**缺**：引入运维复杂度 |

### 代码演示：从硬编码到环境变量

**错误示范（Hardcoding）：**

```python
import boto3

def connect_to_s3():
    # 极度危险：凭据直接暴露在源码中
    client = boto3.client(
        's3',
        aws_access_key_id='AKIAIOSFODNN7EXAMPLE',
        aws_secret_access_key='wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY'
    )
    return client
```

**正确示范（Environment Variables）：**

```python
import boto3
import os

def connect_to_s3():
    # 安全：从环境变量读取，代码本身不包含敏感信息
    # 并在部署时通过 CI/CD 管道注入这些变量
    client = boto3.client(
        's3',
        aws_access_key_id=os.environ.get('AWS_ACCESS_KEY_ID'),
        aws_secret_access_key=os.environ.get('AWS_SECRET_ACCESS_KEY')
    )
    return client
```

---

## 3. 常见反模式

1. 提交 `.env` 文件
	- 场景：新手开发者为了方便同事运行项目，将包含真实 Key 的 `.env` 文件移除了 `.gitignore` 限制。
	- 后果：历史记录永久污染，必须立即吊销所有相关 Key 并 `git filter-branch` 清洗历史。

2. 镜像构建时注入 Secrets
	- 场景：在 Dockerfile 中使用 `ENV MY_PASSWORD=secret` 或在构建时 `COPY` 密钥文件。
	- 风险：Secrets 会被永久保存在 Docker 镜像的层（Layer）中。任何人 `docker history` 都能看到。
	- 修正：使用 Docker 的 `--secret` 挂载功能，或仅在容器运行时（Runtime）注入。

3. 缺乏审计日志
	- 场景：发生数据泄露时，无法知道是谁、在什么时候、使用了哪个 Key 访问了数据库。
	- 修正：Secrets 管理工具必须开启 Access Logs，记录每一次凭据的读取请求。

---

## 4. 工作流

下图展示了一个生产级应用如何通过集中式管理器（如 Vault）获取凭据，而不是在本地存储。

```mermaid
sequenceDiagram
    participant App as 应用程序 (Pod)
    participant SM as Secrets Manager (Vault)
    participant DB as 数据库 (Target)

    Note over App: 应用程序启动
    App->>SM: 1. 身份验证 (我是谁)
    SM-->>App: 2. 验证通过，颁发 Token
    
    App->>SM: 3. 请求数据库凭据 (使用 Token)
    Note over SM: 检查权限策略 (Policy Check)
    SM->>DB: 4. 动态创建临时账号 (TTL 1h)
    DB-->>SM: 返回临时账号/密码
    SM-->>App: 5. 返回临时凭据
    
    App->>DB: 6. 使用临时凭据连接
    Note over App, DB: 正常业务交互
    
    Note over SM: 1小时后
    SM->>DB: 7. 自动撤销/删除该临时账号
```
