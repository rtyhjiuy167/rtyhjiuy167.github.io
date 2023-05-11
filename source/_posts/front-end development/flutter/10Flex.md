---
title: 10 Flex
top: 10 
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

//Flutter中的弹性布局主要通过Flex和Expanded来配合实现
//因为Row和Column都继承自Flex，参数基本相同，所以能使用Flex的地方基本上都可以使用Row或Column

class FlexDemo extends StatelessWidget {
  const FlexDemo({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Flex(
      //Flex 中的一个必传参数 弹性布局的方向
      //Axis.horizontal 为水平方向 vertical 为垂直方向
      direction: Axis.horizontal,
      mainAxisSize: MainAxisSize.min,
      children: [
        Expanded(
          //Expanded 默认沾满 Flex 的弹性布局的方向的所有空间（除了非弹性的）
          child: Container(
            color: Colors.black45,
            width: 50,
            height: 100,
          ),
          flex: 1, //若没有设置 flex ,则所有的默认一致
        ),
        Expanded(
            child: Container(
              width: 50,
              height: 100,
              color: Colors.blue,
            ),
            flex: 3),
        //该Container 没有被 Expanded 包裹 其 width 生效
        Container(
          width: 50,
          height: 100,
          color: Colors.red,
        ),
      ],
    );
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/10Flex.dart';

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
      body: FlexDemo(),
    ); //脚手架
  }
}

```

