---
title: 18 Container
top: 18
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MyPage(),
    );
  }
}

class MyPage extends StatefulWidget {title: 17 加载本地图片
top: 17
tags:
  - flutter
categories:
  - flutter

  const MyPage({Key? key}) : super(key: key);

  @override
  _MyPageState createState() => _MyPageState();
}

class _MyPageState extends State<MyPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Container(
        alignment: Alignment.center,
        width: 100,
        height: 100,
        decoration: BoxDecoration(
          color: Colors.black26,
          border: Border(
            bottom: BorderSide(color: Colors.red),
            left: BorderSide(color: Colors.red),
          ),
        ),
        child: Text("123"),
      ),
    );
  }
}

```

