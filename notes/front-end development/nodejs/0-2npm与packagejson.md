---
title: 0-2 npm与packagejson
top: 0.2
tags:
  - nodejs
categories:
  - nodejs
---

<h4>package.json</h4>

每一个项目都应有一个 `package.json` 文件，通过`npm init`创建<br>之后写入项目名，name(默认为npm-demo)<br>版本号，version(默认1.0.0)<br>

| 版本号 |    名称    |   递增条件    |            特别说明            |
| :----: | :--------: | :-----------: | :----------------------------: |
|   X    |  主版本号  | API兼容性变化 |                                |
|   Y    | 功能版本号 |   功能增减    | 偶数为稳定版本，奇数为开发版本 |
|   Z    | 修订版本号 |    BUG修复    |                                |

项目的描述信息 description<br>项目入口 entry point(默认为index.js)<br>text command<br>github仓库地址 git repository<br>关键字 keywords<br>作者 author<br>开源许可证 license(默认为ISC)

在创建完`package.json` 文件之后，下载第三方包时，应加上`--save`，这样可以在`package.json` 文件中看见依赖信息`dependencies`

如果`node_modules`丢失，可在项目根目录终端中输入`npm install`。其在有`package.json` 文件时会找到该文件里的`dependencies`，将其中的所有依赖项都下载一遍

<h4>npm</h4>

`nmp`node package manager

`nmp`是在下载安装完node就有了的软件

<h5>命令</h5>

`nmp init`

​	-`nmp init -y`可以跳过向导，快速生成

`nmp install`<br>一次性把dependencies选项中的依赖项全部安装

`nmp install 包名`<br>	-只下载包<br>	-简写：`npm i 包名`

`nmp install --save 包名`或`nmp install 包名 --save`<br>	-下载包并保存依赖项<br>	-简写：`npm i -S 包名`

`npm uninstall 包名`<br>	只删除包，保留依赖项<br>	-简写：`npm un 包名`

`npm uninstall --save 包名`<br>	删除包与依赖项<br>	-简写：`npm un -S 包名`

`npm help`<br>	-查看使用帮助

`npm 命令 --help`<br>	查看指定命令的使用帮助<br>	例如`npm install --help`

`npm view 包名 versions `<br>	查看所有的版本

<h5>npm被墙问题</h5>

安装淘宝的cnpm`npm install --global cnpm`<br>之后把要原本要安装包时的命令中的`npm`换成`cnpm`<br>这样就从国外的npm服务器换成的淘宝的服务器了

如果不想下载，可每次安装包时可`npm install 包名 --registry=https://registry.npm.taobao.org`<br>但是每次手动这样加参数会很麻烦，我们可以把这个加入配置文件中：`npm config set registry https://registry.npm.taobao.org`<br>查看npm配置信息`npm config list`