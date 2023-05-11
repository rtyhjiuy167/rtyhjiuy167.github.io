下载 vscode 插件：Tencent Serverless Toolkit for VS Code

下载安装完成后，左侧栏会新增一项，点击，在云端函数中点击添加新用户，输入appId

appId查询网址：https://console.cloud.tencent.com/developer

之后输入SecretId：

SecretId查询网址：https://console.cloud.tencent.com/cam/capi

在腾讯云上开通日志服务：https://console.cloud.tencent.com/cls

#### serverless

```bash
# 安裝 serverless：
npm i serverless@2
# npm install -g serverless-cloud-framework

# 创建 serverless 项目名
serverless
# scf init eggjs-starter --name 项目名

cd 项目名

# 如果没有安裝启动依赖则安装
npm i egg-bin -D
```

直接上传`node_modues`会导致上传时间过长，在`serverless.yml`中配置部署忽略的文件：

```yml
inputs:
  src:
    src: ./
    exclude:
      - .env
      - .git
      - node_modules
```

```bash
# 发布项目
serverless deploy
# scf deploy
```

登录到腾讯云，进入到`Serverless`应用：https://console.cloud.tencent.com/sls

点击项目名，再点击其云函数名称，打开自动安装依赖，点击部署。

之后可通过url访问服务。