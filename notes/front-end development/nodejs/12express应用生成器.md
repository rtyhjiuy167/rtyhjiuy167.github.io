---
title: 12 express应用生成器
top: 12
tags:
  - nodejs
categories:
  - nodejs
---

<h4>Express 应用生成器</h4>

https://www.expressjs.com.cn/starter/generator.html

<h5>项目生成</h5>

```bash
# 方法1：直接生成项目文件 推荐 npx 命令不会污染全局
# $ npx express-generator -ejs 项目名   即可在 bash 的目录下创建项目
$ npx express-generator -e project
```

```bash
# 方法2：第一条语句不会直接生成项目文件
$ npm install -g express-generator
# 该项目要用到ejs模板引擎，执行项目初始化时要加 -e
# $ express -ejs 项目名   即可在 bash 的目录下创建项目
$ express -e project
```

<h5>下载依赖项</h5>

```shell
# 步骤2
npm i
```

```shell
# package.json 里的 start 命令可执行入口文件
node ./bin/www
```

