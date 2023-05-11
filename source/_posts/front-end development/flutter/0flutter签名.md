---
title: 打包
top: 0.5
tags:
  - flutter
categories:
  - flutter
---

直接打包`flutter build apk`，若报错，则执行`flutter clean`，试着运行程序，再`flutter build apk`

签名：

参考：https://www.jianshu.com/p/0f48ec10fd32

```shell
flutter doctor -v
```

<img src="https://img-blog.csdnimg.cn/e36b4b9cd2ce4c3f85c5320fac28d22e.png" alt="4" style="zoom:80%;" />

```shell
# cd 进入箭头所指的bin目录下（若将bin配置到环境变量中，则不需要cd进去）
# -keystore d:/key.jks 存储目录及文件名（文件名固定） 
# -keysize 长度  
# -keyalg RSA -keysize 2048 使用 2048 位 RSA 算法对签名加密
# -validity  有效时间
# -alias 别名
keytool -genkey -v -keystore d:/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key1
# 之后的口令 123456就ok了 Enter*6 y 
```

<img src="https://img-blog.csdnimg.cn/66a6ef27bba742248cf4f159e3aad676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3J0eWhqaXV5MTU=,size_16,color_FFFFFF,t_70" alt="5" style="zoom:80%;" />

导入创建好的证书：`< flutter 项目>/android/app/key/`，将`key.jks`放到该key目录下

创建 `key.properties` 文件:`< flutter 项目>/android/key.properties`

