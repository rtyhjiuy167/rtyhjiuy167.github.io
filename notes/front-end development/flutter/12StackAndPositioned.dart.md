---
title: 12 StackAndPositioned.dart
top: 12 
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class StackAndPositionedDemo extends StatelessWidget {
  const StackAndPositionedDemo({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //一般而言可以在 Stack 外层包裹 Container 限制其大小 这也决定了其 top bottom left right
    // 在Stack 里alignment: Alignment.center 可以将未定位的组件居中
    return //Stack允许子组件堆叠
        Stack(
      // alignment: Alignment.center, //未用 Positioned 包裹的组件全部居中层叠到一起
      //fit: StackFit.expand, //未定位的widget占满Stack整个空间 且自定义宽高无效
      children: [
        //若 Stack没有被其它有明确宽高的组件包裹
        //则其中若存在子组件不用 Positioned 包裹
        // 其表示 Stack 的大小范围 之后的 Positioned 均不会超出
        // 该组件一般放在前面 不然就会覆盖掉其它的了
        Container(
          width: 300,
          height: 350,
          color: Colors.green,
        ),
        //当存在两个以上的为被 Positioned 包裹的组件时 堆叠的宽与高取加起来的最大
        Container(
          width: 400,
          height: 200,
          color: Colors.black,
        ),
        // Positioned 定位
        //Stack Positioned 配合使用可实现绝对定位
        Positioned(
            //Positioned 设置了宽高，则其子组件的宽高无效
            //若Positioned 与其子组件未设置宽高，则默认占满屏幕
            width: 200,
            height: 200,
            //Positioned的宽高与其子组件的宽高是不同的
            child: Container(
              color: Colors.blue,
            )),
        Positioned(
            //Positioned 设置了宽高，则其子组件的宽高无效
            width: 200,
            height: 200,
            bottom: 30, //此时已设置了 height与bottom top 会自动计算出 不能自己设置
            child: Container(
              color: Colors.red,
            )),
        Positioned(
            //Positioned 设置了宽高，则其子组件的宽高无效
            width: 200,
            height: 200,
            right: 10,
            bottom: 10,
            child: Container(
              color: Colors.yellow,
            )),
      ],
    );
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/12StackAndPositioned.dart';

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
      body: StackAndPositionedDemo(),
    ); //脚手架
  }
}

```

