---
title: 15 Scaffold
top: 15
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

//Material 所在类一般设置静态 而Scaffold所在类则设置成动态的
class ScaffoldDemo extends StatefulWidget {
  const ScaffoldDemo({Key? key}) : super(key: key);

  @override
  _ScaffoldDemoState createState() => _ScaffoldDemoState();
}

class _ScaffoldDemoState extends State<ScaffoldDemo>
    with SingleTickerProviderStateMixin {
  //with SingleTickerProviderStateMixin 混入 解决 vsync: this 报错问题

  late TabController _tabController; //需要定义一个Controller

  //自定义的导航栏底部Tab按钮组
  static const List<Tab> myTabs = <Tab>[
    Tab(
      text: 'LEFT', ////Tab 的参数1
      //参数2：icon: Icon(Icons.add),
      // 参数3：child 自定义组件样式
    ),
    Tab(text: 'RIGHT'),
  ];

  @override
  void initState() {
    // TODO: implement initState
    super.initState();

    //TabController ，它是用于控制/监听Tab菜单切换的 vsync: this 固定写法
    _tabController = TabController(vsync: this, length: myTabs.length);
  }

  @override
  void dispose() {
    _tabController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("App Name"), //标题
        actions: <Widget>[
          IconButton(icon: Icon(Icons.add), onPressed: () {}),
          //导航栏右侧菜单 其接收的是<Widget>List

          //注意actions: 会覆盖掉endDrawer
          //要想实现同时实现，则要加方法，实现右抽屉的打开
          //用builder组件 return IconButton 在 IconButton 用 Scaffold.of(context) 可以获取Scaffold 组件的State对象
          Builder(builder: (context) {
            return IconButton(
                icon: Icon(Icons.dashboard),
                onPressed: () {
                  //IconButton一定要是在builder组件里 //不然为 null
                  Scaffold.of(context).openEndDrawer();
                });
          }),
        ],
        centerTitle: true, //将标题居中

        //导航栏底部Tab按钮组
        bottom: PreferredSize(
          preferredSize: const Size.fromHeight(20.0),
          child: Container(
            width: double.infinity,
            height: 30,
            child: TabBar(
              controller: _tabController,
              tabs: myTabs,
            ),
          ),
        ),
      ),
      drawer: Drawer(child: Text("hello left")), //左抽屉菜单
      //设置了左抽屉菜单时，在默认情况下Scaffold会自动将AppBar的leading设置为菜单按钮，点击它便可打开抽屉菜单
      endDrawer: Drawer(child: Text("hello right")), //右抽屉菜单
      //注意AppBar 里的  actions: 会覆盖掉此按钮

      //TabBarView 实现同步切换和滑动状态同步 就是可以滑动切换
      body: TabBarView(
        //TabBar和TabBarView的controller是同一个 这样才能同步
        controller: _tabController,

        //TabBarView  children 接收<Widget>List
        //一般自己在其它文件中写好不同的页面，在这里用map遍历即可
        children: myTabs.map((Tab tab) {
          final String label =
              tab.text!.toLowerCase(); //得到 之前自定义的 Tab按钮组的text  ! 用于判断 null
          return Center(
            child: Text(
              'This is the $label tab',
              style: const TextStyle(fontSize: 36),
            ),
          );
        }).toList(),
      ),

      //bottomNavigationBar 可用BottomAppBar 与 BottomNavigationBar
      //BottomAppBar 相当于是按钮 用于跳转 widget 而 BottomNavigationBar 相当于 bottom: TabBar 切换并显示当前页面
      bottomNavigationBar: BottomAppBar(
        color: Colors.white,
        shape:
            CircularNotchedRectangle(), // 底部导航栏打一个圆形的洞 //无居中floatingActionButton 时不生效
        //当有 floatingActionButton 时 ，其与底部导航栏会有一点间距 美观
        child: Row(
          children: [
            IconButton(
              icon: Icon(Icons.home),
              onPressed: () {},
            ),
            SizedBox(), //中间位置空出 //相当于空白的占了一个位置留给居中的floatingActionButton
            IconButton(icon: Icon(Icons.business), onPressed: () {}),
          ],
          mainAxisAlignment: MainAxisAlignment.spaceAround, //均分底部导航栏横向空间
        ),
      ),
      //BottomNavigationBar  items:[] List<Widget>  currentIndex 选中第几个，默认为0   onTap:(v){} 点击时回调 , v为第几个

      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        child: Icon(Icons.add),
      ),
      floatingActionButtonLocation:
          FloatingActionButtonLocation.centerDocked, //居中floatingActionButton
    );
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/15Scaffold.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: ScaffoldDemo(),
    );
  }
}

```

