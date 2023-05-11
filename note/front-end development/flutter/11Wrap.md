---
title: 11 Wrap
top: 11 
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class WrapDemo extends StatefulWidget {
  const WrapDemo({Key? key}) : super(key: key);

  @override
  _WrapDemoState createState() => _WrapDemoState();
}

class _WrapDemoState extends State<WrapDemo> {
  List<int> list = [];
  @override
  void initState() {
    super.initState();
    for (var i = 0; i < 16; i++) {
      list.add(i);
    }
  }

  Widget build(BuildContext context) {
    //换成Wrap后溢出部分则会自动折行
    return Wrap(
        //默认主轴方向为 水平方向
        direction: Axis.vertical, //主轴方向为垂直方向
        spacing: 1, //主轴方向子widget的间距
        runSpacing: 1, //纵轴方向widget的间距
        alignment: WrapAlignment.center,
        children: list
            .map((e) => Container(
                  height: 100,
                  width: 100,
                  child: Text(e.toString()),
                  color: Colors.blue,
                ))
            .toList()); //map 得到的并不是一个数组需要用.toList() 转成数组
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/11Wrap.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    //根目录
    return MaterialApp(
      //去除debug
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
      body: WrapDemo(),
    ); //脚手架
  }
}

```

