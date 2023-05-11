---
title: 2 配置用户信息 与 Git 命令
top: 2
tags:
  - git
categories:
  - git
---

<h4>配置用户信息</h4>

配置后每次Git提交都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被纳入历史记录中。Git首次安装必须设置一下用户签名，否则无法提交代码。<br>这里设置用户签名和登录 GitHub（或其他代码托管中心）的账号没有任何关系。

```bash
# 首先输入命令查看是否配置了信息
$ git config --list
```

```bash
# 出现user.name 与 user.email 说明已配置过。
# 重复配置会修改上一次所配置的内容
user.name=name
user.email=123456@qq.com
```

```bash
# 系统中对所有用户都普遍适用的配置
$ git config --system user.name "name"
$ git config --system user.email 123456@qq.com
```

```bash
# 用户目录下的配置文件只适用于该用户
$ git config --global user.name "name"
$ git config --global user.email 123456@qq.com
# 生成的配置文件在 C:\Users\当前使用的用户\.gitconfig 里

# 取消配置
$ git config --global --unset user.name
$ git config --global --unset user.email
```

```bash
# 当前项目的 Git 目录中的配置文件
$ git config user.name "name"
$ git config user.email 123456@qq.com

# 取消配置
$ git config --unset user.name
$ git config --unset user.email
```

`系统`可以有多个`用户`，一个`用户`可以有多个`项目`，如果都配置了，则才用`项目`的

<h4>Git底层概念</h4>

它使用的是`linux`命令，而`cmd`是`dos`（也称`windows`）命令

```bash
$ clear  # 清屏
$ echo 'hello' # 往控制台里输出信息
$ echo 'hello' >text.txt # 创建文件，并把hello存入到该文件
$ ll # 将当前文件目录下的子文件和子目录(不包含隐藏的)平铺在控制台（会显示创建者、时间）
$ ll -a  # 将当前文件目录下的子文件和子目录(包含隐藏的)平铺在控制台（会显示创建者、时间）
$ find 目录名 # 将对应目录的子孙文件和子孙目录平铺在控制台
$ find 目录名 -type f # 将对应目录下的的文件平铺在控制台
$ rm 文件名 # 删除文件
$ mv 文件名 重命名后的文件名 # 重命名
$ cat 文件的url # 查看对应文件的内容 文件的url 即bash命令下的相对路径
$ vim 文件的url #（在英文模式下，即右下角会有个中或英）
	按 i 进入编辑
	按 esc 建后，输入 : 进入命令的执行
					wq 保存退出
					q! 强制退出
					set nu 设置行号
```

git链接潮超时时，修改 https 为 git
