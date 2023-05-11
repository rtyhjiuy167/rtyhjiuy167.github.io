---
title: go
top: 0
tags:
  - go
categories:
  - go
---

Go官方镜像站（推荐）：https://golang.google.cn/dl/下载`.msi`

```shell
# 安装完成后在命令提示符执行 为了之后安装依赖成功
go env -w GOPROXY=https://goproxy.cn,direct
```

```shell
# 项目创建时在根目录执行
# go mod init 项目名 （可加域名）
# 例如 go mod init github.com/用户名/项目名
```

`go env`可查看详细配置信息

在根目录下创建`main.go`文件，在弹出的窗口点击`install all`，等待安装成功即可

```shell
# 敲完代码，在项目根目录下编译 执行下面代码即可生成以根目录命名的可执行文件
go build
# 或者自命名
go build -o 自定义名字
#执行后在终端执行该文件即可 用 Tab 键补全
```

```shell
# 如果只有一个文件，可执行下面代码执行程序
# cmd
go run 文件名
# powershell
go run 文件夹名
```

学习go推荐：https://www.liwenzhou.com/posts/Go/go_menu/ //适合有基础的
