---
title: 3 PictureAndIcon
top: 3
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class PictureAndIconDemo extends StatelessWidget {
  //在外部定义样式
  final ButtonStyle style =
      ElevatedButton.styleFrom(textStyle: const TextStyle(fontSize: 20));
  @override
  Widget build(BuildContext context) {
    return ListView(children: [
      //图标按钮 无文本
      IconButton(onPressed: () {}, icon: Icon(Icons.home)),
      //仅图标
      Icon(Icons.add),
      //网络图片
      Image.network(
        "https://img-blog.csdnimg.cn/20210418154500371.jfif?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3J0eWhqaXV5MTU=,size_16,color_FFFFFF,t_70",
        width: 50.0,
        height: 50.0,
      ),
      //本地图片 需要加入到项目根目录下的 images 目录里 并在 pubspec.yaml 里设置 assets
      Image.asset(
        "images/QQ.png",
        width: 50.0,
        height: 50.0,
      ),
      Container(
        width: double.infinity,
        color: Colors.amber[600],
        child: Row(
          children: [
            Container(
              height: 100.0,
              width: 100.0,
              color: Colors.white,
              child: Image.network(
                "https://img-blog.csdnimg.cn/20210418154500371.jfif?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3J0eWhqaXV5MTU=,size_16,color_FFFFFF,t_70",
                fit: BoxFit.fill,
              ),
            ),
            Column(
              children: [
                Text("BoxFit.fill 会拉伸填充满显示空间"),
                Text("图片本身长宽比会发生变化，图片会变形"),
              ],
            )
          ],
        ),
      ),
      Container(
        width: double.infinity,
        color: Colors.amber[600],
        child: Row(
          children: [
            Container(
              height: 100.0,
              width: 100.0,
              color: Colors.white,
              child: Image.network(
                "https://img-blog.csdnimg.cn/20210418154500371.jfif?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3J0eWhqaXV5MTU=,size_16,color_FFFFFF,t_70",
                fit: BoxFit.contain,
              ),
            ),
            Column(
              children: [
                Text("BoxFit.contain 这是图片的默认适应规则，"),
                Text("图片会在保证图片本身长宽比不变的情况下"),
                Text("缩放以适应当前显示空间，图片不会变形"),
              ],
            )
          ],
        ),
      ),
      Container(
        width: double.infinity,
        color: Colors.amber[600],
        child: Row(
          children: [
            Container(
              height: 100.0,
              width: 100.0,
              color: Colors.white,
              child: Image.network(
                "https://img-blog.csdnimg.cn/20210418154500371.jfif?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3J0eWhqaXV5MTU=,size_16,color_FFFFFF,t_70",
                fit: BoxFit.cover,
              ),
            ),
            Column(
              children: [
                Text("BoxFit.cover 会按图片的长宽比放大后居中"),
                Text("填满显示空间,图片不会变形，"),
                Text("超出显示空间部分会被剪裁"),
              ],
            )
          ],
        ),
      ),
      Container(
        width: double.infinity,
        color: Colors.amber[600],
        child: Row(
          children: [
            Container(
              height: 100.0,
              width: 100.0,
              color: Colors.white,
              child: Image.network(
                "https://img-blog.csdnimg.cn/20210418154500371.jfif?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3J0eWhqaXV5MTU=,size_16,color_FFFFFF,t_70",
                fit: BoxFit.fitHeight,
              ),
            ),
            Column(
              children: [
                Text("BoxFit.fitHeight"),
                Text("图片的高度会缩放到显示空间的高度,"),
                Text("宽度会按比例缩放，然后居中显示，"),
                Text("图片不会变形，超出显示空间部分会被剪裁"),
              ],
            )
          ],
        ),
      ),
      Container(
        width: double.infinity,
        color: Colors.amber[600],
        child: Row(
          children: [
            Container(
              height: 100.0,
              width: 100.0,
              color: Colors.white,
              child: Image.network(
                "https://img-blog.csdnimg.cn/20210418154500371.jfif?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3J0eWhqaXV5MTU=,size_16,color_FFFFFF,t_70",
                fit: BoxFit.fitWidth,
              ),
            ),
            Column(
              children: [
                Text("BoxFit.fitWidth"),
                Text("图片的宽度会缩放到显示空间的宽度,"),
                Text("高度会按比例缩放，然后居中显示，"),
                Text("图片不会变形，超出显示空间部分会被剪裁"),
              ],
            )
          ],
        ),
      ),
      Container(
        width: double.infinity,
        color: Colors.amber[600],
        child: Row(
          children: [
            Container(
              height: 100.0,
              width: 100.0,
              color: Colors.white,
              child: Image.network(
                "https://img-blog.csdnimg.cn/20210418154500371.jfif?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3J0eWhqaXV5MTU=,size_16,color_FFFFFF,t_70",
                fit: BoxFit.none,
              ),
            ),
            Column(
              children: [
                Text("BoxFit.none"),
                Text("图片没有适应策略，会在显示空间内显示图片,"),
                Text("如果图片比显示空间大，"),
                Text("则显示空间只会显示图片中间部分"),
              ],
            )
          ],
        ),
      ),
    ]);
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/1Text.dart';
import 'widgets/2Button.dart';
import 'widgets/3PictureAndIcon.dart';
import 'widgets/4Check.dart';
import 'widgets/5ProgressIndicator.dart';
import 'widgets/6Click.dart';
import 'widgets/7TextFieldAndForm.dart';
import 'widgets/8Navigator.dart';

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
      body: PictureAndIconDemo(),
    ); //脚手架
  }
}

```

