---
title: 47 animationController
top: 47
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
    return MaterialApp(home: MyPage());
  }
}

class MyPage extends StatefulWidget {
  const MyPage({Key? key}) : super(key: key);

  @override
  _MyPageState createState() => _MyPageState();
}

class _MyPageState extends State<MyPage> with SingleTickerProviderStateMixin {
  // with SingleTickerProviderStateMixin 可以获得 vsync

  late AnimationController _animationController;
  bool isloading = true;
  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    _animationController = AnimationController(
      duration: Duration(seconds: 1),
      lowerBound: 0.0, //开始的起点 默认0.0
      upperBound: 1.0, //终点 默认1.0
      vsync: this, // 垂直同步 屏幕什么时候渲染新的一帧 因为不同的设备有不同的刷新频率 不同赫兹的手机的频率不同
    );
    _animationController.addListener(() {
      print("${_animationController.value}");
      //在一定时间内打印了 lowerBound ~ upperBound 的值
    });
  }

  @override
  void dispose() {
    // TODO: implement dispose
    super.dispose();
    _animationController.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          setState(() {
            if (isloading) {
              _animationController.repeat(); //循环
            } else {
              _animationController.stop(); // 暂停
            }
            isloading = !isloading;
            //_animationController.forward(); //进行一次
            //_animationController.reset(); // 暂停并重置
          });
        },
        child: Icon(Icons.refresh),
      ),
      body: Center(
        child: FadeTransition(
          // FadeTransition   不透明度  opacity
          // RotationTransition  旋转   turns
          // ScaleTranition 放缩   scale
          // 它们都接收  _animationController
          //  并根据 lowerBound ~ upperBound 的值来实现
          opacity: _animationController,
          child: Icon(
            Icons.refresh,
            size: 50,
          ),
        ),
      ),
    );
  }
}

```

输入`sta`可快速创建带有动画控制器的类
