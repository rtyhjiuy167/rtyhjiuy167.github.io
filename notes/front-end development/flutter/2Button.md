---
title: 2 Button
top: 2
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class ButtonDemo extends StatelessWidget {
  //在外部定义样式
  final ButtonStyle style =
      ElevatedButton.styleFrom(textStyle: const TextStyle(fontSize: 20));
  @override
  Widget build(BuildContext context) {
    //Column 有居中的特性
    return Column(children: [
      //文本按钮
      TextButton(
        //在内部定义样式样式
        style: TextButton.styleFrom(
          padding: EdgeInsets.all(16.0),
          primary: Colors.black, //文本颜色
          textStyle: TextStyle(fontSize: 20),
          backgroundColor: Colors.blue[200], //背景色
        ),
        onPressed: () {},
        child: const Text('TextButton1'),
      ),
      ElevatedButton(
        style: style,
        onPressed: () {},
        child: const Text('ElevatedButton1'),
      ),
      OutlinedButton.icon(
        //.icon 为设置带图标的按钮
        onPressed: () {},
        icon: Icon(Icons.add), //图标
        label: Text("OutlinedButton.icon 带图标的"), //文本
      ),
      TextButton(
        child: Text("TextButton 原始样子"),
        onPressed: () {},
      ),
      ElevatedButton(
        child: Text("ElevatedButton 原始样子"),
        onPressed: () {},
      ),
      OutlinedButton(
        child: Text("OutlinedButton 原始样子"),
        onPressed: () {},
      ),
      IconButton(onPressed: () {}, icon: Icon(Icons.home)) //图标按钮 无文本
    ]);
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/2Button.dart';

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
      body: ButtonDemo(),
    ); //脚手架
  }
}

```

