---
created: 2026-01-08 21:47
updated: 2026-01-08 21:50
tags:
  - Docker
status: seed
aliases:
---

## 0. 核心

镜像瘦身不仅仅是为了 " 省硬盘 "，更重要的是**加快传输速度**（CI/CD 发布更快）和**减少攻击面**（包含的软件越少，漏洞越少）。

瘦身公式：

$$
 \text{最终体积} = \text{基础镜像} + \text{依赖库} + \text{业务代码} - \text{构建废料} 
$$

**洋葱模型**：
Docker 镜像像洋葱一样分层。如果你在第一层 RUN 下载了 100MB 文件，在第二层 RUN 删除了它，**镜像体积依然包含这 100MB**。因为删除操作只是在顶层标记了 " 不可见 "，底层的数据依然存在。

---

## 1. 基础镜像选择

这是起跑线，选错了基础镜像，后续优化事倍功半。

| 镜像类型                   | 典型大小    | 兼容性     | 推荐场景                         |
| :--------------------- | :------ | :------ | :--------------------------- |
| **Official (Default)** | > 800MB | 完美      | 仅限本地调试，严禁生产使用                |
| **Slim**               | < 150MB | 优秀      | **Node.js/Python Web 应用的首选** |
| **Alpine**             | < 10MB  | 风险      | Go/Rust 等静态编译语言，或极客玩家        |
| **Distroless**         | < 20MB  | 无 Shell | 高安全需求，不需要进入容器调试              |

**建议**：对于 Python/Node 项目，盲目上 `Alpine` 可能导致编译 C 依赖库时体积反增（需要安装 gcc 等）。无脑推荐 `xxx-slim` 版本。

---

## 2. 多阶段构建

这是瘦身效果最显著的手段。将编译环境和运行环境分离，把厚重的编译器扔在构建阶段。

**场景**：Golang 应用打包。
- 编译需要：Go SDK (500MB+) + 源代码
- 运行需要：仅二进制文件 (10MB)

```dockerfile
# --- 阶段一：臃肿的构建层 ---
FROM golang:1.19 AS builder
WORKDIR /app
COPY . .
# 编译出二进制文件，禁止依赖 C 库 (CGO_ENABLED=0)
RUN CGO_ENABLED=0 go build -ldflags="-s -w" -o my-server main.go

# --- 阶段二：清爽的运行层 ---
# 使用 scratch (空镜像) 或 alpine
FROM scratch
WORKDIR /root/
# 只拿这一样东西，其他的全部丢弃
COPY --from=builder /app/my-server .
CMD ["./my-server"]
```

*注：`ldflags="-s -w"` 可以去除符号表和调试信息，让二进制文件再小 20%-30%。*

---

## 3. 单层指令优化

针对 `RUN` 指令，遵循 " 安装 - 清理 - 合并 " 的原则。

### 错误示范

每一行都是一层，apt 缓存和压缩包被永久锁死在中间层里。

```dockerfile
FROM ubuntu
RUN apt-get update                    # Layer 1: 下载了索引列表 (几十MB)
RUN apt-get install -y nginx          # Layer 2: 安装软件
RUN rm -rf /var/lib/apt/lists/*       # Layer 3: 试图删除 Layer 1 的数据 (无效！)
```

### 正确示范

使用 `&&` 连接，确保垃圾文件在**同一层**内产生并消失。

```dockerfile
FROM ubuntu
RUN apt-get update \
    && apt-get install -y --no-install-recommends nginx \
    && rm -rf /var/lib/apt/lists/*
```

*技巧：`--no-install-recommends` 告诉 apt 不要安装推荐的包（通常是文档或非必要工具）。*

---

## 4. 上下文过滤

防止 `COPY . .` 指令将无关的大文件带入镜像。

**必须检查**：项目根目录下的 `.dockerignore`。

**场景**：
如果不忽略，`.git` 目录（可能包含数年的历史代码）和 `node_modules`（包含数万个碎片文件）会被全部打包进镜像，导致体积爆炸且构建极慢。

**标准模板**：

```text
.git
.gitignore
node_modules
venv
*.log
*.mp4
Dockerfile
```

---

## 5. 分析工具 (Dive)

肉眼看不出镜像里到底藏了什么垃圾，需要使用 X 光扫描工具。

**工具推荐**：`dive`
它能以图形化界面展示每一层的具体内容，并标记出**重复文件**和**空间浪费**。

**使用方法**：

```bash
# 分析指定镜像
dive my-app:latest
```

**界面解读**：
- **Total Image Size**: 总大小。
- **Potential Wasted Space**: 潜在浪费空间（通常是因为在不同层覆盖或删除了文件）。
- 按 `Ctrl+U` 可以只显示修改过的文件，快速定位是哪一层引入了 `.tar.gz` 没删掉。

---

## 6. 进阶

### docker-slim

这是一个自动化的瘦身工具，它会动态监测容器运行，分析出应用到底用到了哪些文件，然后把没用到的系统库全删了。

- **效果**：Node.js 镜像能从 800MB 缩到 30MB。
- **风险**：可能会误删某些动态加载的库，导致生产环境偶发崩溃。

### UPX 压缩

针对二进制文件（Go/Rust/C++）的可执行程序压缩壳。

```bash
# 在构建阶段压缩二进制文件
RUN apt-get install upx && upx --best --lzma my-server
```

- **效果**：二进制文件体积减少 50%-70%。
- **代价**：启动时需要解压，增加毫秒级的启动延迟。
