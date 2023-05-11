---
title: 1 Text
top: 1
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class TextDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    //Column 有居中的特性
    return Column(
      children: [
        Text(
            //文本内容
            "文本1 文本2 文本3 本文4 " * 4, //字符串重复4次
            maxLines: 2,
            overflow: TextOverflow.ellipsis, //超出的所属部分：省略
            textDirection: TextDirection.rtl //文本开始的位置：从右到左
            ),
        Text(
          "Hello world",
          textScaleFactor: 1.5, //字体大小：1.5倍
        ),
        Text(
          "Hello world " * 6, //字符串重复六次
          textAlign: TextAlign.left, //文本的对齐方式:左对齐
        ),
        Text(
          "Hello world",
          //style设置样式
          style: TextStyle(
              color: Colors.blue, //字体颜色
              fontSize: 18.0, //设置字体大小:18.0px
              height: 1.2, //行高:fontsize*1.2
              fontFamily: "Courier", //字体集
              decoration: TextDecoration.underline, //设置下划线
              decorationStyle: TextDecorationStyle.dashed //下划线为虚线
              ),
        ),
        //其实就是多个行内块组在一起
        Text.rich(
            //TextSpan仅为Text.rich里的组件 其有四个元素
            TextSpan(//对于不同样式的文本内容，Text.rich需要一个TextSpan去 list
                //1.List<TextSpan> children,
                children: [
          TextSpan(text: "Home: "),
          TextSpan(
            text: "https://flutterchina.club", // 2.Sting text,
            style: TextStyle(color: Colors.blue), // 3.TextStyle style,
            //还有一个 4. GestureRecognizer recognizer 手势
          ),
        ])),
        //若想多个文本同一个样式可将其包装到DefaultTextStyle
        // 这样其里的Text会默认设置为该样式
        DefaultTextStyle(
          //1.设置文本默认样式 style
          style: TextStyle(
            color: Colors.red,
            fontSize: 20.0,
          ),
          textAlign: TextAlign.start, //处于某组件里的文本相对于该组件的对齐方式：首对齐
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.end, //Column里的组件的竖直对齐方式：尾对齐
            children: <Widget>[
              Text("hello world " * 7),
              Text("I am Jack"),
              Text(
                "I am Jack",
                style: TextStyle(
                    inherit: false, //2.该文本不继承默认样式
                    color: Colors.grey),
              ),
            ],
          ),
        ),
      ],
    );
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/1Text.dart';

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
      body: TextDemo(),
    ); //脚手架
  }
}

```

