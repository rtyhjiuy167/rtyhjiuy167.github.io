---
title: 9 RowAndColumn
top: 9 
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

//对于线性布局，有主轴和纵轴之分，如果布局是沿水平方向，那么主轴就是指水平方向，而纵轴即垂直方向；
//如果布局沿垂直方向，那么主轴就是指垂直方向，而纵轴就是水平方向。
//在线性布局中，有两个定义对齐方式的枚举类MainAxisAlignment和CrossAxisAlignment，分别代表主轴对齐和纵轴对齐

class RowAndColumnDemo extends StatelessWidget {
  const RowAndColumnDemo({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Column(
      //Column 的宽为子组件的最大宽都
      children: [
        Container(
          color: Colors.grey,
          child: Row(
            //Row 的高为子组件的最大高度
            //Row 的宽 mainAxisSize 默认为 MainAxisSize.max 占满一行
            mainAxisSize: MainAxisSize.max, //另一个值为MainAxisSize.min 即子组件加在一起的宽
            mainAxisAlignment: MainAxisAlignment
                .spaceBetween, //Row里的子组件间隔放置 若设置了MainAxisSize.min，则此属性无意义 Column类似
            children: [
              Container(
                color: Colors.red,
                width: 100,
                height: 100,
              ),
              Container(
                color: Colors.blue,
                width: 100,
                height: 150,
              ),
              Container(
                color: Colors.green,
                width: 100,
                height: 100,
              ),
            ],
          ),
        )
      ],
    );
  }
}
//如果Row里面嵌套Row，或者Column里面再嵌套Column，那么只有最外面的Row或Column会占用尽可能大的空间，里面Row或Column所占用的空间为实际大小
//如果要让里面的Column占满外部Column，可以使用Expanded 组件
```

```dart
import 'package:flutter/material.dart';
import 'widgets/9RowAndColumn.dart';

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
      body: RowAndColumnDemo(),
    ); //脚手架
  }
}

```

