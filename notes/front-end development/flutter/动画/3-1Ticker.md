---
title: 51 Ticker
top: 51
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';
import 'package:flutter/scheduler.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(home: MyPage());
  }
}

class MyPage extends StatefulWidget {
  const MyPage({Key? key}) : super(key: key);

  @override
  _MyPageState createState() => _MyPageState();
}

//不继承 TickerProviderStateMixin 而是自己写
class _MyPageState extends State<MyPage> {
  late Ticker _ticker;
  double _height = 300;
  @override
  void initState() {
    _ticker = Ticker((elapse) {
      print(elapse);
      //setState 每秒的执行速度 取决于 是 60Hz 还是120Hz
      setState(() {
        _height--;
        if (_height <= 0) _height = 300;
      });
    });
    _ticker.start();
    super.initState();
  }

  @override
  void dispose() {
    super.dispose();
    _ticker.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: AnimatedContainer(
          color: Colors.blue.shade300,
          duration: Duration(seconds: 1),
          width: 100,
          height: _height,
        ),
      ),
    );
  }
}

```

