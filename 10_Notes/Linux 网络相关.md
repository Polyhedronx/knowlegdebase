---
created: 2026-01-01 15:14
updated: 2026-01-02 23:20
tags:
  - Linux
  - DevOps
status: seed
aliases:
---

返回 [Linux 命令速查](../30_Maps/Linux%20命令速查.md)

## 1. `ping`

功能：测试网络连通性和延时（ICMP 协议）。

常见用法：

```bash
ping www.baidu.com          # 持续 ping，按 Ctrl+C 停止
ping -c 4 www.baidu.com     # 只 ping 4 次后自动停止（脚本中常用）
ping -i 0.2 www.baidu.com   # 快速 ping（间隔 0.2秒），也就是“由于太快可能被当作攻击”
```

---

## 2. `curl`

功能：直接模拟浏览器发送请求。

常见用法：

```bash
curl https://www.baidu.com    # 请求一个网页，会看到一大堆HTML源码
curl -I https://www.baidu.com    # 只看响应头（调试时常用）
```

---

## 3. `wget`

功能：命令行下载工具，比 `curl` 更擅长下载文件（支持断点续传、后台下载）。

常见用法：

```bash
wget https://example.com/file.iso       # 直接下载
wget -O new_name.iso https://...        # 下载并重命名（类似 curl -o）
wget -c https://...                     # 常用：断点续传（下载中断后，再次运行接着下）
wget -b https://...                     # 后台下载，会生成一个 wget-log 日志文件
```

---

## 4. `netstat`

功能：查看网络端口、连接状态（老牌工具，逐渐被 `ss` 取代，但很多老系统默认只有它）。

常见用法：

```bash
netstat -tunlp              # 查看所有监听的端口和对应的进程
# -t (tcp) -u (udp) -n (不解析域名，只显示IP) -l (listening) -p (显示PID/进程名)

netstat -tunlp | grep 80    # 快速检查 80 端口被谁占用了
netstat -an | grep ESTABLISHED | wc -l  # 统计当前有多少个活跃连接
```

---

## 5. `ss`

功能：查看套接字统计（Socket Statistics）。`netstat` 的现代替代品，在大并发下速度更快。

常见用法：

```bash
ss -tunlp                   # 用法和 netstat 几乎一样，显示监听端口
ss -s                       # 打印摘要信息（快速看有多少个 TCP 连接，不刷屏）
ss -pl | grep ssh           # 看 ssh 服务开了没
```

---

## 6. `telnet`

功能：早期的远程登录协议（明文传输，极不安全）。**现在主要用于测试端口连通性**。

常见用法：

```bash
telnet 192.168.1.1 80       # 测试目标的 80 端口通不通
# 如果显示 "Connected to ... " 说明通了
# 如果一直 "Trying ..." 说明防火墙拦了或者服务没起

# 退出方法：
# 按 Ctrl + ] ，然后输入 quit 回车
```

---

## 7. `scp`

功能：基于 SSH 的安全文件拷贝（Secure Copy）。

常见用法：

```bash
# 推送（本地 -> 远程）
scp local_file.txt root@192.168.1.5:/home/root/

# 拉取（远程 -> 本地）
scp root@192.168.1.5:/home/root/remote_file.txt .

# 常用参数
scp -P 2222 ...             # 如果 SSH 端口不是 22，用 -P (大写) 指定
scp -r folder/ ...          # 递归复制整个文件夹
```

---

## 8. `rsync`

功能：远程数据同步工具。**核心优势是 " 增量同步 "**（只传修改过的部分），比 `scp` 快且节省带宽。

常见用法：

```bash
# 本地同步（类似 cp，但更智能）
rsync -av /source/ /dest/

# 远程同步（本地 -> 远程）
rsync -avz /local/dir/ user@host:/remote/dir/
# -a: 归档模式（保留权限、时间等所有属性）
# -v: 显示过程
# -z: 传输时压缩

# 常用高级组合
rsync -avz --delete /local/ /remote/
# --delete: 极其危险但也很有用。如果本地删了文件，远程也会同步删除，保持完全一致。
# （如果不加 --delete，本地删了，远程还会保留）

rsync -P ...               # 显示进度条和断点续传支持
```
