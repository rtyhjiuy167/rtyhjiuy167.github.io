---
title: 13 Align
top: 13
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

//如果我们只想简单的调整一个子元素在父元素中的位置的话，使用Align组件会更简单一些

class AlignDemo extends StatelessWidget {
  const AlignDemo({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Column(children: [
      Container(
          width: 200,
          height: 200,
          color: Colors.green,
          child: Align(
            // widthFactor和heightFactor是用于确定Align 组件本身宽高的属性；
            //  它们是两个缩放因子，会分别乘以子元素的宽、高，最终的结果就是Align 组件的宽高。
            //   如果值为null，则组件的宽高将会占用尽可能多的空间。
            alignment: Alignment(1, 0), // -1,-1 为左上角  1,1 为右下角 可以超出父组件的范围
            child: FlutterLogo(
              size: 60,
            ),
          )),
      Container(
          width: 200,
          height: 200,
          color: Colors.yellow,
          child: Align(
            alignment: FractionalOffset(1, 1), //0,0  为左上角  1,1 为右下角 可以超出父组件的范围
            child: FlutterLogo(
              size: 60,
            ),
          )),
      //当widthFactor或heightFactor为null时组件的宽高将会占用尽可能多的空间，这一点需要特别注意
      Container(
        color: Colors.red,
        //Center 继承自Align，它比Align只少了一个 alignment 参数
        //此时 Center无父组件限制其 宽高 缺少 widthFactor或heightFactor时 组件的宽高将会占用尽可能多的空间
        child: Center(
          child: Text(
            "xxx",
          ), //Text限制了高 行高 = fontSize * height
        ),
      ),
      Container(
        color: Colors.red,
        child: Center(
          widthFactor: 1,
          heightFactor: 1,
          child: Text("xxx"),
        ),
      )
    ]);
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/13Align.dart';

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
      body: AlignDemo(),
    ); //脚手架
  }
}

```

