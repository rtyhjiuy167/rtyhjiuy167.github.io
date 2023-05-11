---
title: 31 使用InAppWebView远程加载页面
top: 31
tags:
  - flutter
categories:
  - flutter
---

```shell
flutter pub add html
flutter pub add dio
flutter pub add flutter_inappwebview
```

```
//尝试运行程序
//根据报错信息修改文件内容
//android/app/build.gradle

defaultConfig {
        // TODO: Specify your own unique Application ID (https://developer.android.com/studio/build/application-id.html).
        applicationId "com.example.flutter_app"
        minSdkVersion 19  //<-----根据报错信息修改到合适的值
        targetSdkVersion 30
        versionCode flutterVersionCode.toInteger()
        versionName flutterVersionName
    }
```

<h4>main.dart</h4>

```dart
import 'package:flutter/material.dart';
import 'package:flutter_app/routes/Routes.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      onGenerateRoute: onGenerateRoute,
      initialRoute: '/',
    );
  }
}
```

<h3>views</h3>

<h4>HomePage.dart</h4>

```dart
import 'package:flutter/material.dart';
import 'package:flutter/widgets.dart';
import 'package:dio/dio.dart';
import 'dart:convert';

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  List _list = [];
  int _page = 1;
  bool hasMore = true;
  ScrollController _scrollController = new ScrollController();
  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    //获取数据
    this._getData();
    //
    _scrollController.addListener(() {
      print(_scrollController.position.pixels); //获取滚动条下拉的距离
      print(
          _scrollController.position.maxScrollExtent); //获取整个页面的高度 指ListView的总高
      if (_scrollController.position.pixels >
          _scrollController.position.maxScrollExtent - 40) {
        this._getData();
      }
    });
  }
//项目中记得释放

  _getData() async {
    if (this.hasMore) {
      var apiUrl =
          "http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=$_page";
      var response = await Dio().get(apiUrl);
      var res = json.decode(response.data)["result"];
      setState(() {
        //因为发送过来的数据时json格式的，需要解码
        //this._list = json.decode(response.data)["result"]; //这样写，每次都换新的了
        this._list.addAll(res); //数据拼接
        this._page++; //每获取一次换到下一页
      });
      //在已知最后一页只有不到20的长度的情况下，用于下次请求不进行
      if (res.length < 20) {
        this.hasMore = false;
      }
    }
  }

//RefreshIndicator下拉刷新组件要求的是Future<void> 类型的 且是 async 的
  Future<void> _onRefresh() async {
    print("下拉刷新");
    await Future.delayed(Duration(microseconds: 500), () {
      _getData();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(),
        body: Column(
          children: [
            Container(
                width: double.infinity,
                height: 300,
                child: this._list.length > 0 //当_list.length为0 说明还没有拉取到数据，则显示加载中
                    ? RefreshIndicator(
                        onRefresh: _onRefresh,
                        child: ListView.builder(
                            controller: _scrollController,
                            itemCount: this._list.length,
                            itemBuilder: (context, index) {
                              //拉取到底部时
                              //因为每次都是会在底部左右就会向list添加数据，所以只有最后才有
                              //当index == this._list.length - 1 时，可能网慢，则显示转圈圈
                              if (index == this._list.length - 1) {
                                return Column(
                                  children: [
                                    ListTile(
                                      title: Text(
                                        "${this._list[index]["title"]}",
                                        maxLines: 1,
                                      ),
                                      onTap: () {
                                        Navigator.pushNamed(context, '/news',
                                            arguments: {
                                              "aid": this._list[index]["aid"]
                                            });
                                      },
                                    ),
                                    Divider(),
                                    _getMoreWidget()
                                  ],
                                );
                              } else {
                                return Column(
                                  children: [
                                    ListTile(
                                      title: Text(
                                        "${this._list[index]["title"]}",
                                        maxLines: 1,
                                      ),
                                      onTap: () {
                                        Navigator.pushNamed(context, '/news',
                                            arguments: {
                                              "aid": this._list[index]["aid"]
                                            });
                                      },
                                    ),
                                    Divider(),
                                  ],
                                );
                              }
                            }))
                    : Text("正在加载中"))
          ],
        ));
  }

//加载中的圈圈
  Widget _getMoreWidget() {
    //为防止最后一直显示转圈圈，加个if判断
    //要拉取新数据就转圈圈，不然就显示到底了
    if (hasMore) {
      return Center(
        child: Padding(
          padding: EdgeInsets.all(10.0),
          child: Row(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.center,
            children: <Widget>[
              Text(
                '加载中...',
                style: TextStyle(fontSize: 16.0),
              ),
              CircularProgressIndicator(
                strokeWidth: 1.0,
              )
            ],
          ),
        ),
      );
    } else {
      return Center(
        child: Text("--我是有底线的@QAQ@--"),
      );
    }
  }
}

```

<h4>NewPage.dart</h4>

```dart
import 'package:flutter/material.dart';
import 'package:dio/dio.dart';
import 'dart:convert';
import 'package:flutter_html/flutter_html.dart';
import 'package:flutter_html/style.dart';
import 'package:flutter_inappwebview/flutter_inappwebview.dart';
import 'package:html/dom.dart' as dom;

class NewPage extends StatefulWidget {
  dynamic arguments;
  NewPage({Key? key, this.arguments}) : super(key: key);

  @override
  _NewPageState createState() => _NewPageState();
}

class _NewPageState extends State<NewPage> {
  bool _flag = true;
  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    print(widget.arguments);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(),
        body: Column(children: [
          this._flag ? _getMoreWidget() : Text(""),
          Expanded(
              child: InAppWebView(
            //老版本：initialUrl    新版本：initialUrlRequest
            initialUrlRequest: URLRequest(
                url: Uri.parse(
                    "https://www.phonegap100.com/newscontent.php?aid=${widget.arguments['aid']}")),
            //页面加载时触发
            onProgressChanged:
                (InAppWebViewController controller, int progress) {
              print(progress / 100);

              if (progress / 100 > 0.9999) {
                setState(() {
                  this._flag = false;
                });
              }
            },
          ))
        ]));
  }

  //加载中的圈圈
  Widget _getMoreWidget() {
    return Center(
      child: Padding(
        padding: EdgeInsets.all(10.0),
        child: Row(
          mainAxisAlignment: MainAxisAlignment.center,
          crossAxisAlignment: CrossAxisAlignment.center,
          children: <Widget>[
            Text(
              '加载中...',
              style: TextStyle(fontSize: 16.0),
            ),
            CircularProgressIndicator(
              strokeWidth: 1.0,
            )
          ],
        ),
      ),
    );
  }
}

```

<h3>routes</h3>

<h4>Routes.dart</h4>

```dart
import 'package:flutter/material.dart';
import 'package:flutter_app/views/HomePage.dart';
import 'package:flutter_app/views/NewPage.dart';

//配置路由
final Map<String, Function> routes = {
  '/': (context) => HomePage(),
  '/news': (context, {arguments}) => NewPage(arguments: arguments),
};
//固定写法
var onGenerateRoute = (RouteSettings settings) {
  // 统一处理
  final String? name = settings.name;
  final Function? pageContentBuilder = routes[name];
  if (pageContentBuilder != null) {
    if (settings.arguments != null) {
      final Route route = MaterialPageRoute(
          builder: (context) =>
              pageContentBuilder(context, arguments: settings.arguments));
      return route;
    } else {
      final Route route =
          MaterialPageRoute(builder: (context) => pageContentBuilder(context));
      return route;
    }
  }
};
```

