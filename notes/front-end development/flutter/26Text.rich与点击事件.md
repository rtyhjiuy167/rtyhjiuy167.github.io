---
title: 26 Text.rich与点击事件
top: 26
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';
import 'package:flutter/widgets.dart';
import 'package:flutter/gestures.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  static const String _title = 'Flutter Code Sample';

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: _title,
      home: Scaffold(
        appBar: AppBar(title: const Text(_title)),
        body: _TapGestureRecognizer(),
      ),
    );
  }
}

class _TapGestureRecognizer extends StatefulWidget {
  @override
  _TapGestureRecognizerState createState() => new _TapGestureRecognizerState();
}

class _TapGestureRecognizerState extends State<_TapGestureRecognizer>
    with SingleTickerProviderStateMixin {
  TapGestureRecognizer _tapGestureRecognizer = new TapGestureRecognizer();
  bool _toggle = false;

  @override
  void dispose() {
    //用到GestureRecognizer的话一定要调用其dispose方法释放资源
    _tapGestureRecognizer.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text.rich(TextSpan(children: [
          TextSpan(text: "你好世界"),
          TextSpan(
            text: "点我变色",
            style: TextStyle(
                fontSize: 30.0, color: _toggle ? Colors.blue : Colors.red),
            recognizer: _tapGestureRecognizer
              ..onTap = () {
                setState(() {
                  _toggle = !_toggle;
                });
              },
          ),
          TextSpan(text: "你好世界"),
        ])),
      ],
    );
  }
}

```

