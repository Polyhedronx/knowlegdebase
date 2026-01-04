---
created: 2026-01-01 15:10
updated: 2026-01-02 23:12
tags:
  - Linux
  - DevOps
status: seed
aliases:
---

返回 [Linux 命令速查](../30_Maps/Linux%20命令速查.md)

## 1. tar + gzip（`.tar.gz` / `.tgz`）

**创建压缩包**

最经典也最常用的组合，打包目录并压缩。

```bash
tar -czvf archive.tar.gz myfolder/
```

- `c` = create（创建）
- `z` = gzip 压缩
- `v` = verbose（显示过程）
- `f` = 指定文件名

**解压**

```bash
tar -xzf archive.tar.gz
```

- `x` = extract（解压）
- `z` = gzip
- `f` = 文件

**场景**

- 项目源码打包发给别人
- 备份目录
- 上传到服务器

---

## 2. zip / unzip（`.zip`）

**创建 zip 包**

Windows 友好，跨平台使用。

```bash
zip -r archive.zip myfolder/
```

- `-r` = 递归打包目录

**解压**

```bash
unzip archive.zip
```

**场景**

- 与 Windows 用户共享文件
- 快速压缩小项目或单个文件夹

---

## 3. 单文件 gzip（`.gz`）

**压缩**

压缩单个文件，不打包多个文件。

```bash
gzip file.txt
```

生成 `file.txt.gz`（原文件默认被删除，可加 `-k` 保留原文件）

**解压**

```bash
gunzip file.txt.gz
```

**场景**

- 压缩日志文件
- 减少单文件大小，提高传输速度
