---
title: tab切换
top: 80
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class Tabs extends StatefulWidget {
  final Map? arguments;
  const Tabs({Key? key, this.arguments}) : super(key: key);

  @override
  _TabsState createState() => _TabsState();
}

class _TabsState extends State<Tabs> {
  int _currentIndex = 0;
  late PageController _pageController;
  final List<Widget> _pageList = [
    const Page(),
    const Page(),
    const Page(),
    const Page()
  ];
  @override
  void initState() {
    super.initState();
    _pageController = PageController(initialPage: _currentIndex);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: PageView(
        controller: _pageController,
        children: _pageList,
        physics: const NeverScrollableScrollPhysics(), //禁止pageView滑动，不配置默认可以滑动
        onPageChanged: (index) {
          setState(() {
            _currentIndex = index;
          });
        },
        // physics: NeverScrollableScrollPhysics(),  //禁止pageView滑动，不配置默认可以滑动
      ),
      bottomNavigationBar: BottomAppBar(
        color: Colors.white,
        child: Row(
          children: [
            buildBotomItem(_currentIndex, 0, Icons.home, "首页"),
            buildBotomItem(_currentIndex, 1, Icons.library_music, "发现"),
            buildBotomItem(_currentIndex, 2, Icons.email, "消息"),
            buildBotomItem(_currentIndex, 3, Icons.person, "我的"),
          ],
        ),
      ),
    );
  }

  buildBotomItem(int selectIndex, int index, IconData icon, String title) {
    //未选中状态的样式
    TextStyle textStyle = const TextStyle(fontSize: 12.0, color: Colors.grey);
    MaterialColor iconColor = Colors.grey;
    double iconSize = 20;
    EdgeInsetsGeometry padding = const EdgeInsets.only(top: 8.0);

    if (selectIndex == index) {
      //选中状态的文字样式
      textStyle = const TextStyle(fontSize: 13.0, color: Colors.blue);
      //选中状态的按钮样式
      iconColor = Colors.blue;
      iconSize = 25;
      padding = const EdgeInsets.only(top: 6.0);
    }
    Widget padItem = const SizedBox();

    padItem = Padding(
      padding: padding,
      child: Container(
        color: Colors.white,
        child: Center(
          child: Column(
            children: <Widget>[
              Icon(
                icon,
                color: iconColor,
                size: iconSize,
              ),
              Text(
                title,
                style: textStyle,
              )
            ],
          ),
        ),
      ),
    );

    Widget item = Expanded(
      flex: 1,
      child: GestureDetector(
        onTap: () {
          if (index != _currentIndex) {
            setState(() {
              _currentIndex = index;
              _pageController.jumpToPage(_currentIndex);
            });
          }
        },
        child: SizedBox(
          height: 52,
          child: padItem,
        ),
      ),
    );
    return item;
  }
}

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Tabs(),
    );
  }
}

class Page extends StatelessWidget {
  const Page({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```