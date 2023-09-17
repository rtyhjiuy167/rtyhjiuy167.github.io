---
title: 6 GestureDetector的简单使用
top: 6
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class ClickDemo extends StatelessWidget {
  const ClickDemo({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        //GestureDetector 可以把没有点击事件的组件加上点击事件
        GestureDetector(
          onTap: () {}, //单击事件
          onDoubleTap: () {}, //双击事件
          onLongPress: () {}, //长按
          child: Text("GestureDetector"),
        )
      ],
    );
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/6Click.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    //根目录
    return MaterialApp(
        //去除debug
        debugShowCheckedModeBanner: false,
        home: HomePage());
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
      body: TextFieldAndFormDemo(),
    ); //脚手架
  }
}

```

