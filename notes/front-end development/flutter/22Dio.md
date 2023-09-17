---
title: 22 Dio
top: 22
tags:
  - flutter
categories:
  - flutter
---

```
flutter pub add dio
```

```dart
import 'package:dio/dio.dart';
import 'package:flutter/material.dart';

class DioDemo extends StatefulWidget {
  const DioDemo({Key? key}) : super(key: key);

  @override
  _DioDemoState createState() => _DioDemoState();
}

class _DioDemoState extends State<DioDemo> {
  Dio _dio = Dio();
  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    _dio.options.baseUrl = "http://api.td0f7.cn:8083/";
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        ElevatedButton(
          onPressed: _get,
          child: Text("Get"),
        ),
      ],
    );
  }

  void _get() async {
    /**
     * get 查询
     * post 一般用于 登录 注册
     * put 修改
     * delete 删除
     * 
     */
    var result1 = await Dio().get("http://api.td0f7.cn:8083/dio/dio/test?id=1");
    print('result1： $result1 ');
    //在写下 baseUrl 后就不用写 IP 地址和端口号了
    var result2 = await _dio.get("dio/dio/test?id=1");
    print('result2： $result2 ');
    //传参的另一种写法
    var result3 = await _dio.get("dio/dio/test", queryParameters: {"id": "1"});
    print('result3： $result3 ');

    //头部参数传递
    var result4 = await _dio.get(
      "dio/dio",
      queryParameters: {"id": "1"},
      //options 接收Options对象其有headers属性，接收Map类型数据
      options: Options(headers: {"token": "12345"}),
    );
    print('result4： $result4 ');
  }
}
```

```dart
import 'package:flutter/material.dart';
import 'widgets/22Dio.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("flutter"),
        centerTitle: true,
      ),
      body: DioDemo(),
    );
  }
}
```

更多详细用法，请参考：https://github.com/flutterchina/dio/blob/master/README-ZH.md

文件下载：在 AndroidManifext.xml 加入权限

```xml
<!--在安卓文件夹中的app\src\profile\AndroidManifest.xml中添加权限-->

<!--网络权限-->   
<uses-permission android:name="android.permission.INTERNET"/>
<!-- 读写存储权限 -->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

