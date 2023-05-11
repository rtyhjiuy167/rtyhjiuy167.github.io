---
title: 25 GestureDetector的深入与CircleAvatar
top: 25
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
        body: _Drag(),
      ),
    );
  }
}

class _Drag extends StatefulWidget {
  @override
  _DragState createState() => new _DragState();
}

class _DragState extends State<_Drag> with SingleTickerProviderStateMixin {
  double _top = 0; //距顶部的偏移
  double _left = 0.0; //距左边的偏移
  double _top1 = 0.0;
  double _width = 180;
  @override
  Widget build(BuildContext context) {
    //拖动其实用到层叠组件 子组件再用 Positioned 定位
    return Column(
      children: [
        Container(
          width: double.infinity,
          height: 300,
          child: Stack(
            children: <Widget>[
              Positioned(
                top: _top,
                left: _left,
                child: GestureDetector(
                  //圆圈控件
                  child: CircleAvatar(
                    child: Text("任意"),
                    radius: 30, //半径大小
                  ),
                  //手指按下时会触发此回调
                  onPanDown: (DragDownDetails e) {
                    //打印手指按下的位置(相对于屏幕)
                    print("用户手指按下：${e.globalPosition}");
                  },
                  //手指滑动时会触发此回调
                  onPanUpdate: (DragUpdateDetails e) {
                    //用户手指滑动时，更新偏移，重新构建
                    setState(() {
                      if (_top < 0) _top = 0;
                      if (_top > 100) _top = 100;
                      _left += e.delta.dx; //两次之间的x
                      _top += e.delta.dy;
                    });
                  },
                  onPanEnd: (DragEndDetails e) {
                    //打印滑动结束时在x、y轴上的速度
                    print(e.velocity);
                  },
                ),
              ),
              Positioned(
                top: _top1,
                child: GestureDetector(
                    child: CircleAvatar(child: Text("仅垂直"), radius: 30),
                    //垂直方向拖动事件
                    onVerticalDragUpdate: (DragUpdateDetails details) {
                      setState(() {
                        if (_top1 < 300) _top1 += details.delta.dy;
                      });
                    }),
              )
            ],
          ),
        ),
        Container(
          width: double.infinity,
          height: 200,
          child: GestureDetector(
            //指定宽度，高度自适应
            child: Image.network(
                "https://img-blog.csdnimg.cn/20210418154500371.jfif?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3J0eWhqaXV5MTU=,size_16,color_FFFFFF,t_70",
                width: _width),
            onScaleUpdate: (ScaleUpdateDetails details) {
              setState(() {
                //缩放倍数在0.8到10倍之间 在图片上双指张开、收缩就可以放大、缩小图片
                _width = 200 * details.scale.clamp(.8, 10.0);
              });
            },
          ),
        )
      ],
    );
  }
}

```

