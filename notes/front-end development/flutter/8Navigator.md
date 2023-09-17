---
title: 8 Navigator
top: 8
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

// MaterialPageRoute的各个参数
//builder 是一个WidgetBuilder类型的回调函数，它的作用是构建路由页面的具体内容，返回值是一个widget。我们通常要实现此回调，返回新路由的实例
//settings 包含路由的配置信息，如路由名称、是否初始路由（首页）
//maintainState：默认情况下，当入栈一个新路由时，原来的路由仍然会被保存在内存中，如果想在路由没用的时候释放其所占用的所有资源，可以设置maintainState为false
//fullscreenDialog

//注意：点击左上角的返回是不会传参的

//命名路由
final routes = {
  '/': (context) => LoginPage(),
  'menu': (context) => MenuPage(),
  'nextpage': (context) => NextPage(),
  'finalpage': (context) => FinalPage(),
  'welecomepage': (context) => WeleComePage(),
};

class LoginPage extends StatelessWidget {
  const LoginPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("登录"),
      ),
      body: ElevatedButton(
        onPressed: () async {
          //Navigator.push(   ,   )
          //第一个参数为上下文对象  第二个参数为 Route 对象
          // Route对象里 builder 方法 接收上下文对象 return 入栈的路由
          // 页面不传参时，可以不用 async 和 await
          // result 接收 上个页面的 pop() 方法里的参数 当上个页面pop()执行时，该页面就会收到，不需要点击按钮再执行
          var result = await Navigator.push(
              //等待其取得返回值后再执行 print
              //Navigator.push 将给定的路由入栈（即打开新的页面），返回值是一个Future对象，用以接收新路由出栈（即关闭）时的返回数据
              context,
              MaterialPageRoute(
                builder: (context) {
                  return MenuPage(); //当然 跳转到新页面时 可通过 widget 传参
                },
                settings: RouteSettings(name: ""),
                maintainState: false, //默认为 true
              ));
          print(result);
        },
        child: Text("跳转"),
      ),
    );
  }
}

class MenuPage extends StatelessWidget {
  const MenuPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("菜单"),
      ),
      body: Column(children: [
        ElevatedButton(
            onPressed: () {
              //返回到上个界面
              //pop 可接收一个任意类型的参数 用于页面传参
              Navigator.of(context).pop("result");
              //将栈顶路由出栈，"result"  为页面关闭时返回给上一个页面的数据
            },
            child: Text("返回")),
        ElevatedButton(
            onPressed: () {
              Navigator.of(context)
                  .push(MaterialPageRoute(
                      builder: (context) {
                        return NextPage();
                      },
                      settings:
                          RouteSettings(arguments: "MenoPage turn NextPage")))
                  //由pop到此页且 pop 有传值时才会走 then
                  .then((value) => print(value));
            },
            child: Text("下一页")),
      ]),
    );
  }
}

class NextPage extends StatelessWidget {
  const NextPage({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    dynamic _arguments = ModalRoute.of(context)
        ?.settings
        .arguments; //setting 要判断是否为 null 在 . 前加 ? 即可
    //接收到此页面的 ModelRoute 里的 setting.arguments
    return Scaffold(
        appBar: AppBar(
          title: Text(_arguments),
        ),
        body: ElevatedButton(
            onPressed: () {
              Navigator.of(context).pushNamed("finalpage",
                  arguments: //这里相当于是settings.arguments
                      "参数字符串 已跳到FinalPage "); //若上个页面是通过 settings.arguments 传值的 这里 then 是接收不到的
            },
            child: Text("命名路由传参")));
  }
}

class FinalPage extends StatelessWidget {
  const FinalPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    dynamic _arguments = ModalRoute.of(context)?.settings.arguments;
    return Scaffold(
        appBar: AppBar(
          title: Text(_arguments),
        ),
        body: ElevatedButton(
          onPressed: () {
            print(_arguments);
            Navigator.of(context).popAndPushNamed("welecomepage",
                arguments:
                    "WeleComePage"); //popAndPushNamed 相对于先执行 pop 再执行 pushNamed //注意：pop到上一页是会执行回调
          },
          child: Text("点击跳转WeleComePage"),
        ));
  }
}

class WeleComePage extends StatelessWidget {
  const WeleComePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    dynamic _arguments = ModalRoute.of(context)?.settings.arguments;
    return Scaffold(
        appBar: AppBar(
          title: Text(_arguments),
        ),
        body: ElevatedButton(
          onPressed: () {
            print(_arguments);
          },
          child: Text("你好"),
        ));
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/8Navigator.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    //根目录
    return MaterialApp(
      //去除debug
      debugShowCheckedModeBanner: false,
      routes: routes,
      initialRoute: '/', //默认也是用 '/'作首页
      //home: LoginPage(),//有初始化路由了，就不需要 home 了
    );
  }
}
```

