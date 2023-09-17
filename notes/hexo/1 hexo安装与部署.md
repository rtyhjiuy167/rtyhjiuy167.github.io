---
title: hexo 安装与部署
top: 1
tags:
  - hexo
categories:
  - hexo

---

## 一、相关工具

1.安装`git`与`node.js`

2.安装Hexo

在cmd中输入并执行：`npm install -g hexo-cli `

3.安装deployer

在cmd中输入并执行：`npm install hexo-deployer-git --save`

## 二、安装与部署至github

1.在自己想要放博客文章的地方,输入下列命令：

```bash
$ hexo init
```

2.配置远程仓库

在github创建仓库后，可进行ssh密钥绑定，为之后可通过命令来部署博客。

```bash
#  创建ssh密钥,之后全部回车，默认处理
$ ssh-keygen -t rsa -C "your_email@example.com"
#  复制密钥到剪切板
$ clip < ~/.ssh/id_rsa.pub
#  在github上配置ssh密钥后，测试，如果要输入yes/no，输入yes即可
$ ssh -T git@github.com
```

修改`_config.yml`中的配置：

```yml
deploy:
  type: git
  repository: git@github.com:xxx.github.io.git # 仓库地址 https
  branch: main # 注意查看是否为main分支
```

在`source/_posts`目录下放入md文档，如果没有该目录则自己手动创建

3.清除静态文件

```bash
$ hexo clean
```

4.生成静态文件

在某些情况下，对博客的修改的效果查看时，若无修改的效果，可执行该命令：

```bash
$ hexo g
```

5.可以尝试本地运行查看效果

```bash
$ hexo s
```

6.部署

```bash
$ hexo d
```

## 三、部署hexo

#### 1.将githubPage仓库作为镜像仓库

#### 2.解析绑定域名

打开`cmd`，执行`ping vercel为你生成的域名（例如169abc-123.vercel.app）`

vercel 部署后，会被墙，需要添加如下域名解析：

在解析时，分别添加`@ A记录 默认 76.223.126.88`和`www CNAME 默认 cname-china.vercel-dns.com`

vercel 已被墙，改用 https://docs.netlify.com/