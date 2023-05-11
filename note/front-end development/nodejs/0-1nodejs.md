---
title: 0-1 nodejs
top: 0.1
tags:
  - nodejs
categories:
  - nodejs
---

nodejs官网：http://nodejs.cn/

查看node版本：`node -v`

查看npm本地仓库：`npm list -global`

查看当前镜像命令：`npm config get registry`<br>配置淘宝镜像：`npm config set registry=https://registry.npm.taobao.org`<br>将淘宝镜像改回npmjs镜像：`npm config set registry https://registry.npmjs.org/`

node会自带npm，可能不是最新的，可执行`npm install npm -g`更新一下

### centos 安装

```bash
cd /usr/local
```

```bash
wget https://nodejs.org/dist/v16.15.0/node-v16.15.0-linux-x64.tar.xz
```

```bash
tar -xJf node-v16.15.0-linux-x64.tar.xz
```

```bash
vi /etc/profile
```

直接在首行加入：

```bash
export NODE_HOME=/usr/local/node-v16.15.0-linux-x64
 
export PATH=$NODE_HOME/bin:$PATH
```

```bash
source /etc/profile
```

### nvm管理

nvm是node版本的管理工具，建议使用。

官网：https://github.com/coreybutler/nvm-windows/releases

```bash
# 安装指定版本
nvm install 版本

# 卸载指定版本
nvm uninstall 版本

# 开启 nodejs版本管理
nvm on

# 关闭nodejs版本管理
nvm off

# 查看已安装的 node 版本
nvm list

# 查看 nvm 版本号
nvm version

# 使用指定版本的 node
nvm use 版本
```

修改安装目录下的`setting.txt`，在最后加上：

```
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```

查看全局默认安装位置：

```
npm config get prefix
```

设置自定义安装位置：

```
npm config set cache "D:\nvm\nodjs\node_cache"
npm config set prefix "D:\nvm\nodjs\node_global"
```

把`D:\nvm\nodjs\node_cache`和`D:\nvm\nodjs\node_global`添加到Path中。
