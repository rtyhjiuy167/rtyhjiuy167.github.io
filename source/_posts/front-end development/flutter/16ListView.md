---
title: 16 ListView
top: 16
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class ListViewDemo extends StatefulWidget {
  const ListViewDemo({Key? key}) : super(key: key);

  @override
  _ListViewDemoState createState() => _ListViewDemoState();
}

class _ListViewDemoState extends State<ListViewDemo> {
  late ScrollController _controller;
  bool isShow = false;
  @override
  void initState() {
    super.initState();
    _controller = ScrollController();

    //为_controller 监听事件
    //滚动条在一定区域内消失，在一定区域内存在
    _controller.addListener(() {
      if (_controller.offset > 10 && isShow == false) {
        setState(() {
          isShow = true;
        });
      } else if (_controller.offset <= 10 && isShow == true) {
        setState(() {
          isShow = false;
        });
  //         print("maxScrollExtent:${_scrollController!.position.maxScrollExtent}");
  //    print("extentBefore:${_scrollController!.position.extentBefore}");
  //    print("extentInside:${_scrollController!.position.extentInside}");
 //     print("extentAfter:${_scrollController!.position.extentAfter}");
  //    print(
  //        "${_scrollController!.position.maxScrollExtent} and ${_scrollController!.position.extentBefore + _scrollController!.position.extentAfter}");
      }
    });
  }

  @override
  void dispose() {
    super.dispose();
    _controller.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("flutter"),
        centerTitle: true,
      ),
      body: Column(
        children: [
          Container(
            child: Text("hello"),
          ),
          //Expanded 默认沾满 Flex 的弹性布局的方向的所有空间（除了非弹性的）
          Expanded(
              child: Container(
            height: 200,
            //将 可滑动组件限制其宽高即可与其它组件一起并列使用
            color: Colors.blue[100],
            child: Scrollbar(
                //在可滚动组件外层包裹 Scrollbar 可实现并显示滚动条
                //没有设置 itemCount 是 Scrollbar 无法移动
                // ListView.builder 可以实现上下缓冲
                child: ListView.builder(
              //拉到顶或底部时 能否再拉
              //  BouncingScrollPhysics(), //ios 默认的
              //  ClampingScrollPhysics(), //android 默认的
              physics: ClampingScrollPhysics(),
              //ListView.builder 的主要参数 其它参数在其它的可滚动组件中也有
              // 参数一 itemBuilder 方法
              // 参数二 itemCount  有几个
              // 参数三  controller
              itemBuilder: (BuildContext context, int index) {
                return index % 2 == 0
                    ? Text("1234")
                    : Divider(
                        color: Colors.blue,
                      ); //对于用构造函数来添加Divider 可以将 ListView.builder 换成 ListView.separated
                //它多了一个 separatorBuilder 参数 该参数是一个分割组件生成器
              },
              itemCount: 30, // 不加则为无限个 最好加上，不然有很多时，拖动滚动条会很卡 因为其不知道待加载组件的宽或高
              controller: _controller,
              itemExtent: 100.0, //强制children的“滚动方向上子组件的长度”为itemExtent的值
              //指定itemExtent比让子组件自己决定自身长度会更高效 无需频繁去计算列表高度
              shrinkWrap: false, //该属性表示是否根据子组件的总长度来设置ListView的长度，默认值为false
              //默认情况下，ListView的会在滚动方向尽可能多的占用空间。当ListView在一个无边界(滚动方向上)的容器中时，shrinkWrap必须为true
            )),
          ))
        ],
      ),
      floatingActionButton: isShow
          ? FloatingActionButton(
              onPressed: () {
                // _controller.offset 是 滚动条当前的位置
                //_controller.animateTo 滚动条跳到
                _controller.animateTo(0, //跳转的位置
                    duration: Duration(microseconds: 500), //跳转的共计时间
                    curve: Curves.easeInBack); //跳转的动画
              },
              child: Icon(Icons.backup))
          : null,
    );
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/16ListView.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: ListViewDemo(),
    );
  }
}

```

