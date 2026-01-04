---
created: 2026-01-01 14:39
updated: 2026-01-03 22:13
tags:
  - SSH
  - ComputerWeb
status: seed
aliases:
  - ssh key
---

返回 [计算机学习](../30_Maps/计算机学习.md)

## 怎么查看

创建的 SSH key 默认都存放在用户目录的一个隐藏文件夹中：

```
C:\Users\13543\.ssh\
```

里面会有两个文件：

```
id_ed25519 (私钥，不能泄露)
id_ed25519.pub (公钥，安心上传)
```

在 Git Bash 中输入：

```
ls -al ~/.ssh
```

能看到类似：

```
id_ed25519
id_ed25519.pub
known_hosts
```

如果只是想重新复制公钥，可以：

```
cat ~/.ssh/id_ed25519.pub
```

然后复制整行，从 ```ssh-ed25519``` 开始，到邮箱结尾。
