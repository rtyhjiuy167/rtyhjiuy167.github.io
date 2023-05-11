---
title: 14 MarginAndPadding
top: 14
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class MarginAndPaddingDemo extends StatelessWidget {
  const MarginAndPaddingDemo({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Container(
            color: Colors.blue,
            width: 100,
            height: 100, //仅限制于此组件下的 子组件的 height + 该组件的内边距 <= height
            margin: EdgeInsets.fromLTRB(8, 8, 8, 8), //外边距
            padding: EdgeInsets.fromLTRB(10, 2, 2, 10), //内边距
            child: Row(
              children: [
                Container(
                    color: Colors.red,
                    width: 40,
                    height: 40,
                    margin: EdgeInsets.fromLTRB(
                        0, 1, 0, 1)), //可以在父组件的padding的基础上再移动
                Container(
                    color: Colors.red,
                    width: 40,
                    height: 40,
                    margin: EdgeInsets.fromLTRB(0, 1, 0, 0))
              ],
            ))
      ],
    );
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/14MarginAndPadding.dart';

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
      body: MarginAndPaddingDemo(),
    );
  }
}

```

