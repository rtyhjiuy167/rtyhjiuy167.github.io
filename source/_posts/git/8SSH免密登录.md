---
title: 8 SSH免密登录
top: 8
tags:
  - git
categories:
  - git
---

1.检查`C:\Users\用户名`的文件夹里有没有`.ssh`文件夹，有跳过此步

```bash
# 生成公钥和密钥 -t 加密协议 rsa 一种非对称加密协议
$ ssh-keygen -t rsa -C "github账户邮箱"
```

2.进入`.ssh`文件夹，把公钥里的内容复制到`github`里的`settings`->`SSH and GPG keys`->`New SSH key`的`key`里，`title`是放别名的

以后用SSH替换HTTPS即可

