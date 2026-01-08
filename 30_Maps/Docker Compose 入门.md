---
created: 2026-01-04 21:02
updated: 2026-01-08 22:49
tags:
  - Docker
status: seed
aliases:
---

> **核心目标**：掌握单机多容器编排，通过一个 YAML 文件定义和运行复杂的应用程序（如 Web 服务 + 数据库 + 缓存），告别繁琐的 `docker run` 命令。

---

## 0. 概念与基础

*理解为什么需要它，以及它解决了什么痛点。*

- [[Docker Compose 是什么]] - 为什么 `docker run` 不够用？" 基础设施即代码 " 的轻量级实现。
- [[Compose V1 vs V2]] - 了解 `docker-compose` (旧) 与 `docker compose` (新, 集成在 Docker CLI 中) 的区别。
- [[工程目录结构规范]] - 如何组织代码、Dockerfile 和 docker-compose.yml 文件。

---

## 1. 核心配置文件详解

*一切的核心：`docker-compose.yml` 语法手册。*

### 基础结构

- [[Compose 文件版本 (version)]] - 3.x 版本的通用性。
- [[Services (服务定义)]] - 定义应用中的各个组件（如 frontend, backend, db）。
- [[Networks (网络定义)]] - 自定义网络名称与驱动。
- [[Volumes (数据卷定义)]] - 声明命名卷 (Named Volumes)。

### 常用指令

- [[build 与 context]] - 指向 Dockerfile 路径，自动构建镜像。
- [[image]] - 指定使用现有的远程镜像。
- [[ports]] - 端口映射 (Host:Container)。
- [[volumes 与 bind mounts]] - 路径挂载，区分 `./` (开发代码同步) 与 `data_vol:` (数据库持久化)。
- [[environment 与 env_file]] - 注入环境变量的两种方式。
- [[command 与 entrypoint]] - 覆盖镜像默认的启动命令。
- [[depends_on]] - **关键**：控制服务启动顺序 (如 Web 等待 DB 启动)。
- [[restart]] - 重启策略 (always, on-failure)。

---

## 2. 命令行操作

*如何指挥这套编排系统。*

- [[启动与停止]] - `docker compose up -d` (后台运行) 与 `docker compose down` (停止并清理网络/容器)。
- [[构建与重构]] - `docker compose build` (仅构建) 与 `docker compose up --build` (强制重构启动)。
- [[查看状态]] - `docker compose ps` (查看服务列表), `docker compose top` (查看进程)。
- [[日志管理]] - `docker compose logs -f` (聚合查看所有服务的日志，不同颜色区分)。
- [[容器内执行]] - `docker compose exec [service_name] bash` (进入特定服务)。
- [[清理资源]] - `docker compose down -v` (危险：同时删除数据卷)。

## 3. 网络与服务发现

*容器之间是如何互相找到对方的？*

- [[Compose 默认网络]] - 为什么在 Compose 里不需要手动 `--link`。
- [[基于服务名的通信]] - 代码里连接数据库直接写 `host: "postgres-db"` 而不是 IP 地址。
- [[跨项目网络通讯]] - 如何让 Project A 的容器访问 Project B 的数据库 (external network)。

## 4. 环境变量与配置管理

- [[使用 .env 文件]] - 自动读取同级目录下的 `.env` 文件。
- [[变量替换语法]] - `${VARIABLE}` 与 `${VARIABLE:-default}` (默认值写法)。
- [[多环境配置技巧]] - 开发 (`docker-compose.dev.yml`) vs 生产 (`docker-compose.prod.yml`) 的覆盖策略 (`-f` 参数)。

## 5. 实战场景

- [[实战：Python Flask + Redis]] - 经典的 Web + 缓存结构。
- [[实战：WordPress + MySQL]] - 常见的 CMS 快速部署。
- [[实战：Nginx 反向代理配置]] - 使用 Nginx 容器代理后端的 API 服务。

---

### 附录与进阶

- [[Docker Compose 常见报错]] - 端口冲突、数据库连接拒绝等。
- [[Compose 资源限制]] - `deploy` 节点下的 `resources` (CPU/内存限制)。
- [[关联：从 Compose 到 Kubernetes]] - 概念对比，何时需要升级架构。
