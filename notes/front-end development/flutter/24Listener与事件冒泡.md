---
title: 24 Listener与事件冒泡
top: 24
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';
import 'package:flutter/widgets.dart';

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
        body: const Center(
          child: MyStatefulWidget(),
        ),
      ),
    );
  }
}

class MyStatefulWidget extends StatefulWidget {
  const MyStatefulWidget({Key? key}) : super(key: key);

  @override
  State<MyStatefulWidget> createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  int _downCounter = 0;
  int _upCounter = 0;
  double x = 0.0;
  double y = 0.0;

/**
 * PointerEvent类中包括当前指针的一些信息
 *  1.position：它是鼠标相对于当对于全局坐标的偏移
 *  2.delta：两次指针移动事件（PointerMoveEvent）的距离
 *  3.pressure：按压力度，如果手机屏幕支持压力传感器(如iPhone的3D Touch)，
 *    此属性会更有意义，如果手机不支持，则始终为1
 *  4.orientation：指针移动方向，是一个角度值
 */
  void _incrementDown(PointerEvent details) {
    _updateLocation(details);
    setState(() {
      _downCounter++;
    });
  }

  void _incrementUp(PointerEvent details) {
    _updateLocation(details);
    setState(() {
      _upCounter++;
    });
  }

  void _updateLocation(PointerEvent details) {
    setState(() {
      x = details.position.dx;
      y = details.position.dy;
    });
  }

  @override
  Widget build(BuildContext context) {
    //ConstrainedBox 限制类容器
    return ListView(
      children: [
        ConstrainedBox(
          //constraints 限制的内容
          constraints: BoxConstraints.tight(const Size(300.0, 200.0)),
          child: Listener(
            onPointerDown: _incrementDown, //手指按下
            onPointerMove: _updateLocation, //手指移动
            onPointerUp: _incrementUp, //手指抬起
            child: Container(
              color: Colors.lightBlueAccent,
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center, //内容居中
                children: <Widget>[
                  const Text(
                      'You have pressed or released in this area this many times:'),
                  Text(
                    '$_downCounter presses\n$_upCounter releases',
                    style: Theme.of(context).textTheme.headline4, //已定义了的字体样式
                  ),
                  Text(
                    'The cursor is here: (${x.toStringAsFixed(2)}, ${y.toStringAsFixed(2)})',
                    //toStringAsFixed 保留整数 自动四舍五入
                  ),
                ],
              ),
            ),
          ),
        ),
        Stack(
          children: <Widget>[
            Listener(
              child: ConstrainedBox(
                constraints: BoxConstraints.tight(Size(300.0, 200.0)),
                child:
                    DecoratedBox(decoration: BoxDecoration(color: Colors.blue)),
              ),
              onPointerDown: (event) => print("down0"),
            ),
            Listener(
              child: ConstrainedBox(
                constraints: BoxConstraints.tight(Size(200.0, 100.0)),
                child: Center(child: Text("左上角200*100范围内非文本区域点击")),
              ),
              onPointerDown: (event) => print("down1"),
              //behavior: HitTestBehavior.translucent, //放开此行注释后可以"点透"
              //层叠组件中的Listener设置behavior: HitTestBehavior.translucent 则可以击穿该层
            )
          ],
        )
      ],
    );
  }
}

```

