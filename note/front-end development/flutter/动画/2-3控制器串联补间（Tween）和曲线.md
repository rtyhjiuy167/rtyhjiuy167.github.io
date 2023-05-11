---
title: 48 控制器串联补间（Tween）和曲线
top: 48
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
    super.initState();
    _animationController = AnimationController(
      duration: Duration(seconds: 1),
      vsync: this,
    )..repeat();

    //上诉代码与下面的代码效果相同
    /*     
    _animationController = AnimationController(
      duration: Duration(seconds: 1),
      vsync: this,
    );
    _animationController.repeat(); 
    */
    // 两点即表示 回传本身
  }

  @override
  void dispose() {
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
          });
        },
        child: Icon(Icons.refresh),
      ),
      body: Column(children: [
        FadeTransition(
          opacity: _animationController.drive(Tween(begin: 0.0, end: 1.0)),
          //不设置lowerbound 与 upperbound 可用.drive(Tween(begin: 0.0, end: 1.0)) 来设置
          //drive 驱动  Tween 补间
          child: Icon(
            Icons.refresh,
            size: 50,
          ),
        ),
        SlideTransition(
          position: _animationController
              .drive(Tween(begin: Offset(0, 0), end: Offset(1, 1))),
          //不用 drive 来驱动 可以这样写：
          //Tween(begin: Offset(0, 0), end: Offset(1, 1)).animate(_animationController)
          //这样写可以在中间实现效果叠加
          child: Container(
            width: 40,
            height: 40,
            color: Colors.black,
          ),
        ),
        SizedBox(height: 10),
        SlideTransition(
          position: Tween(begin: Offset(0, 0), end: Offset(1, 1))
              .chain(CurveTween(curve: Curves.elasticInOut))
              .chain(CurveTween(
                  curve: Interval(
                      0.3, 0.5))) //Interval(0.3, 0.5) 指动画仅在 30%~50% 的时间段里完成
              .animate(_animationController),
          child: Container(
            width: 40,
            height: 40,
            color: Colors.black,
          ),
        ),
      ]),
    );
  }
}

```

