---
title: 31 beego
top: 31
tags:
  - go
categories:
  - go
---

下载安装 bee 工具：`go get github.com/beego/bee`。被墙，可尝试用手机热点

下载完成后，会在`GOPATH`目录下新增`bin`文件夹，进入`bin`文件夹，将此路径添加到`PATH`中

在`cmd`中执行`bee`，若出现详细的命令信息，则安装成功<br>

在当前文件夹创建新项目`bee new 项目名称`，其会自动生成`go.mod`

在项目根目录下执行`bee run`运行项目 <br>可能会报错提示你下载：`go get github.com/shiena/ansicolor`，执行下载，重新运行即可

可对`./views/index.tql`作修改，将右下角的`纯文本`改成`html`可方便修改

GO升级，通过该方式，用户可以升级`beego`框架：`go get -u github.com/astaxie/beego`

