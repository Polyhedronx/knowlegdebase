---
created: 2026-01-04 20:42
updated: 2026-01-04 21:04
tags:
  - Docker
  - DevOps
status: seed
aliases:
---

> **核心目标**：理解容器化原理，掌握 Docker CLI 操作，精通 Dockerfile 编写与镜像优化。

## 0. 核心概念与原理

*构建心理模型，理解 Docker 是如何工作的。*

- [[Docker 是什么]] - 容器 vs 虚拟机，解决了什么问题（"Build once, Run anywhere"）。
- [[Docker 架构详解]] - Client-Server 架构，Docker Daemon，Containerd。
- **三大核心要素**：
	- [[Docker 镜像 (Image)]] - 只读模板，分层存储原理（UnionFS）。
	- [[Docker 容器 (Container)]] - 镜像的运行实例，进程隔离（Namespace & Cgroups）。
	- [[Docker 仓库 (Repository)]] - 存放镜像的地方（Docker Hub, 私有库）。

---

## 1. 环境准备与配置

- [[Docker 安装指南]] - Windows (WSL2), macOS (Docker Desktop/OrbStack), Linux。
- [[配置国内镜像加速]] - 解决拉取速度慢的问题（阿里云、腾讯云等配置）。
- [[Docker 常用配置]] - `daemon.json` 配置详解（日志大小限制、存储驱动等）。

---

## 2. 命令行基础

*日常高频使用的命令清单。*

### 容器生命周期管理

- [[启动与运行容器]] - `docker run` 参数详解 (`-d`, `-p`, `-v`, `--name`)。
- [[查看与停止容器]] - `docker ps`, `docker stop`, `docker kill`。
- [[进入容器调试]] - `docker exec -it` 与 `docker attach` 的区别。
- [[查看容器日志]] - `docker logs` (跟随、时间戳、尾部查看)。
- [[清理容器]] - `docker rm` 与 `docker prune`。

### 镜像管理

- [[拉取与推送镜像]] - `docker pull`, `docker push`。
- [[查看与删除镜像]] - `docker images`, `docker rmi`。
- [[镜像导出与导入]] - `docker save/load` (离线迁移场景)。

---

## 3. Dockerfile 与镜像构建

*如何将代码打包成镜像。*

### 基础语法

- [[Dockerfile 指令大全]] - 常用指令速查。
	- `FROM`: 基础镜像选择。
	- `WORKDIR`: 设定工作目录。
	- `COPY` vs `ADD`: 文件复制的区别与最佳场景。
	- `RUN`: 执行构建命令。
	- `CMD` vs `ENTRYPOINT`: 容器启动命令的差异与配合用法。
	- `ENV` vs `ARG`: 环境变量与构建参数的区别。
	- `EXPOSE`: 声明端口。

### 构建操作

- [[Docker Build 命令]] - `docker build -t`，构建上下文（Context）的理解。
- [[使用 .dockerignore]] - 排除无关文件（如 `.git`, `node_modules`）以减小上下文传输。

---

## 4. 镜像构建最佳实践

*从能跑到生产级的进阶。*

- [[多阶段构建 (Multi-stage Builds)]] - **核心技巧**：在构建阶段编译，在运行阶段仅保留产物（极大减小体积）。
- [[镜像分层与缓存利用]] - 编写顺序对缓存命中的影响（先 COPY package.json，后 COPY 源代码）。
- [[基础镜像选择策略]] - Alpine vs Debian-slim vs Distroless。
- [[最小权限原则]] - 创建非 root 用户运行容器服务。
- [[Docker 镜像瘦身指南]] - 减少层数、清理缓存（`apt-get clean` 等）。

---

## 5. 数据持久化与网络

*虽然重点是镜像，但运行需要了解的基础。*

- [[Docker Volume (数据卷)]] - 数据持久化，容器删除数据不丢失。
- [[Bind Mounts (挂载)]] - 方便开发环境代码实时同步。
- [[Docker 网络模式]] - Bridge, Host, None 模式简介，容器互联。

---

## 6. 常见场景实战

- [[实战：构建 Node.js 镜像]]
- [[实战：构建 Python 镜像]]
- [[实战：构建 Go 语言镜像]] (多阶段构建典型案例)
- [[实战：构建 Nginx 静态网站镜像]]

---

### 附录与资源

- [[Docker 常见错误排查 (Troubleshooting)]]
- [[Docker 官方文档链接]]
- [Docker Compose 入门](Docker%20Compose%20入门.md)
