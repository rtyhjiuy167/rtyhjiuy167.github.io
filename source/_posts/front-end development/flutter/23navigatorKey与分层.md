---
title: 23 navigatorKey与分层
top: 23.5
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';
import 'package:flutter_app/view/ViewModel.dart';

void main() {
  runApp(MultiProvider(
    providers: [
      ChangeNotifierProvider(
        create: (context) => ViewModel(),
          //将ViewModel 设置成全局状态管理后 导包 调方法即可
      )
    ],
    child: MyApp(),
  ));
}

final GlobalKey<NavigatorState>? navigatorKey = GlobalKey<NavigatorState>();

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      navigatorKey: navigatorKey, //全局context
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
      body: Text("nihao"),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () {
          Navigator.push(context, MaterialPageRoute(
            builder: (context) {
              return APage();
            },
          ));
        },
      ),
    );
  }
}

class APage extends StatelessWidget {
  const APage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("a page"),
      ),
      body: Container(),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () {
          context.read<ViewModel>().back2();
        },
      ),
    );
  }
}
```

<h4>ModelView(处理业务)</h4>

```dart
import 'package:flutter/material.dart';
import 'package:flutter_app/model/model.dart';
import 'package:flutter_app/main.dart';

class ViewModel extends ChangeNotifier {
  Model _model = Model();
    
  void get(String id) async {
    var reuslt = await _model.get(id);
  }

   //back1 与 back2 的效果类似
  void back1() {
    Navigator.of(navigatorKey!.currentState!.context).pop();
  }

  void back2() {
    BuildContext context = navigatorKey!.currentState!.overlay!.context;
    Navigator.of(context).pop();
  }
}

```

<h4>Model(网络加载)</h4>

```dart
import 'package:dio/dio.dart';
import 'package:flutter/material.dart';

class Model {
  /**
   * 方法
   * 参数
   */
  dynamic get(String id) async {
    return await Dio().get("http://api.td0f7.cn:8083/dio/dio/test");
  }
}

```

<h4>View(UI)</h4>
