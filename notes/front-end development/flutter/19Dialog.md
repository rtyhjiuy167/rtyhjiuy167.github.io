---
title: 19 Dialog
top: 19
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class DialogDemo extends StatefulWidget {
  const DialogDemo({Key? key}) : super(key: key);

  @override
  _DialogDemoState createState() => _DialogDemoState();
}

class _DialogDemoState extends State<DialogDemo> {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        ElevatedButton(
            onPressed: () async {
              int result = await _showAlertDialog();
              print(result);
            },
            child: Text("AlertDialog")),
        ElevatedButton(
            onPressed: () async {
              int result = await _showListDialog();
              print(result);
            },
            child: Text("SimpleDialog"))
      ],
    );
  }

  Future<dynamic> _showAlertDialog() {
    //AlertDialog 弹出框相当于是一个页面需要包裹在 showDialog 方法下，用其参数 builder 返回AlertDialog
    //showDialog 前必须要有return 不然拿不到返回值 或者可以像_showListDialog()那样写
    return showDialog(
        context: context,
        builder: (BuildContext context) {
          return AlertDialog(
              title: Text(" AlertDialog"),
              content: Text("你确定要删除吗？"),
              actions: [
                ElevatedButton(
                    onPressed: () => Navigator.of(context).pop(1),
                    child: Text("确认")),
                ElevatedButton(
                    onPressed: () => Navigator.of(context).pop(2),
                    child: Text("取消")),
              ]);
        });
  }

  Future<dynamic> _showListDialog() async {
    var result = await showDialog(
        barrierDismissible: false, //点击空白处是否关闭对话框：false
        context: context,
        builder: (BuildContext context) {
          return SimpleDialog(
            title: Text("标题"),
            children: [
              GestureDetector(
                  child: Text(
                    "中文",
                    style: TextStyle(fontSize: 20, color: Colors.blue),
                  ),
                  onTap: () => Navigator.of(context).pop(1)),
              GestureDetector(
                  child: Text(
                    "英文",
                    style: TextStyle(fontSize: 20, color: Colors.blue),
                  ),
                  onTap: () => Navigator.of(context).pop(2)),
            ],
          );
        });
    return result;
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/19Dialog.dart';

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
      body: DialogDemo(),
    );
  }
}

```

