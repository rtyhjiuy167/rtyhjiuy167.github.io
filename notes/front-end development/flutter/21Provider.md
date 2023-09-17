---
title: 21 Provider
top: 21
tags:
  - flutter
categories:
  - flutter
---

```shell
# 引入第三方包
flutter pub add provider
```

```dart
import 'package:flutter/material.dart';
import 'package:flutter_app/provider/21ProviderDemo.dart';
import 'package:provider/provider.dart';

void main() {
  runApp(
    MultiProvider( 
      //多个状态管理可以List形式写到 providers 中
    providers: [
      //使用了 Provider 一定要用  ChangeNotifierProvider 包裹 不然不生效
      ChangeNotifierProvider(
        create: (context) => CountProvider(),
      )
    ],
    child: MyApp(),
  ));
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
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
      body: Text(Provider.of<CountProvider>(context).count.toString()),
      //全局状态管理下的变量 可以 Provider.of< 继承了ChangeNotifier 的类 >(context).类里的方法 可以得到其值
      //虽然每次使用仍要导包，但是其值是全局的
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () {
          context.read<CountProvider>().add();
        },
      ),
    );
    //对全局管理下的值进行变动时 使用 context.read<继承了ChangeNotifier 的类>().类里的方法 修改其值
  }
}
```

```dart
import 'package:flutter/material.dart';

//全局状态管理需继承  ChangeNotifier
class CountProvider extends ChangeNotifier {
  int _count = 0;

  get count => _count; //get 方法返回 _count
  void add() {
    _count++;
    notifyListeners(); //通知使用了 CountProvider 的UI刷新
  }
}
```

