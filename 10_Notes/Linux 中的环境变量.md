---
created: 2026-01-01 15:21
updated: 2026-01-03 22:37
tags:
  - Linux
  - DevOps
status: seed
aliases:
---

返回 [Linux 命令速查](../30_Maps/Linux%20命令速查.md)

## 1. 临时生效 - 当前终端窗口

**场景**：测试、调试、临时运行脚本。关闭窗口后失效。

```bash
# 语法
export 变量名=变量值

# 示例
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin
```

---

## 2. 永久生效 - 当前用户

**场景**：开发环境配置、个人工具，不影响其他用户。
**文件**：`~/.bashrc` (如果你用 zsh 则是 `~/.zshrc`)

**步骤**：
1. 打开文件：`vim ~/.bashrc`
2. 在文件**末尾**添加：

   ```bash
   export PORT=8080
   export PATH=$PATH:/opt/my_tool/bin
   ```

3. **立即生效**：`source ~/.bashrc`

---

## 3. 永久生效 - 所有用户

**场景**：安装全局软件（如 Node.js, Go, JDK）供所有人使用。

### 方法 A：推荐模块化管理

**文件**：`/etc/profile.d/*.sh`
Linux 启动时会自动加载 `/etc/profile.d/` 下所有以 `.sh` 结尾的文件。

1. 新建文件：`sudo vim /etc/profile.d/my_env.sh`
2. 写入配置：

   ```bash
   export JAVA_HOME=/usr/lib/jvm/java-11
   export PATH=$PATH:$JAVA_HOME/bin
   ```

3. 立即生效：`source /etc/profile`

### 方法 B：简单键值对

**文件**：`/etc/environment`
**特点**：这不是脚本文件，只能写 `KEY="VALUE"`，**不支持** `$变量` 引用（即不能写 `$PATH:…`）。

1. 打开文件：`sudo vim /etc/environment`
2. 直接修改 `PATH` 这一行，用冒号分隔：

   ```text
   PATH="/usr/local/sbin:/usr/local/bin:…:/opt/new_tool/bin"
   MY_VAR="hello"
   ```

3. 重启或重新登录生效。

---

## 4. 关于 `PATH` 的拼接

配置 `PATH` 时，**切记**要加上原本的 `$PATH`，否则系统命令（ls, cd, vi）会找不到，导致系统瘫痪。

```bash
# 正确写法：保留原有路径，在后面追加
export PATH=$PATH:/new/path

# 错误写法：直接覆盖（千万别这样写！）
export PATH=/new/path
```

---

## 5. 总结：方法论图表

| 需求场景           | 修改文件                    | 生效命令                  | 特点                         |
| :------------- | :---------------------- | :-------------------- | :------------------------- |
| **仅仅试一下**      | 无 (命令行输入)               | 立即                    | 关窗口即逝                      |
| **我自己用** (最常用) | `~/.bashrc`             | `source ~/.bashrc`    | 只对当前用户有效，重装系统保留 /home 即可恢复 |
| **所有人用** (脚本)  | `/etc/profile.d/xxx.sh` | `source /etc/profile` | 干净卫生，方便管理，不污染主文件           |
| **所有人用** (简单)  | `/etc/environment`      | 重启/重登录                | 仅支持静态值，不支持变量引用             |
