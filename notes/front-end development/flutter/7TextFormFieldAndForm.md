---
title: 7 TextFormFieldAndForm
top: 7
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class TextFieldAndFormDemo extends StatefulWidget {
  const TextFieldAndFormDemo({Key? key}) : super(key: key);

  @override
  _TextFieldAndFormDemoState createState() => _TextFieldAndFormDemoState();
}

//表达要求：
//用户名不能为空，如果为空则提示“用户名不能为空”。
//密码不能小于6位，如果小于6为则提示“密码不能少于6位”

//FormState为Form的State类，可以通过Form.of()或GlobalKey获得。我们可以通过它来对Form的子孙FormField进行统一操作

//1.FormState.validate()：调用此方法后，会调用Form子孙FormField的validate回调，
// 如果有一个校验失败，则返回false，所有校验失败项都会返回用户返回的错误提示。

//2.FormState.save()：调用此方法后，会调用Form子孙FormField的save回调，用于保存表单内容

//3.FormState.reset()：调用此方法后，会将子孙FormField的内容清空

//validator 接收的参数 value 在做操作时，需要判断是否为 null 返回值为 string 或者 null
class _TextFieldAndFormDemoState extends State<TextFieldAndFormDemo> {
  final GlobalKey<FormState> _formKey = GlobalKey<FormState>();
  TextEditingController _Controller1 = TextEditingController();
  TextEditingController _Controller2 =
      TextEditingController(text: "hello world");
  FocusNode focusNode1 = new FocusNode();
  FocusNode focusNode2 = new FocusNode();
  late FocusScopeNode focusScopeNode; //未初始化的变量需加 late

  @override
  void initState() {
    // TODO: implement initState
    super.initState();
  }

  @override
  void dispose() {
    // TODO: implement dispose
    //对于已初始化的变量，退出时应释放掉，否则占用内存
    super.dispose();
    _Controller1.dispose();
    _Controller2.dispose();
    focusNode1.dispose();
    focusNode2.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Form(
        key: _formKey,
        child: Column(
          children: <Widget>[
            TextFormField(
              focusNode: focusNode1, //关联focusNode1
              controller: _Controller1, //监听文本
              maxLength: 10, //输入框文本的最大长度
              maxLines: 1, //输入框的最大行数，默认为1
              decoration: InputDecoration(
                  labelText: "用户名", //未取得焦点时的提示信息 取得焦点时移至左上方
                  hintText: "用户名或邮箱", //取得焦点时的提示信息
                  prefixIcon: Icon(Icons.person) //图标 可以用 icon 但不美观
                  ),
              onChanged: (v) {
                print(_Controller1.text); //_unameController.text 可以得到文本框的内容
              },
              validator: (String? value) {
                //验证 若没有输入则 Please enter some text
                if (value == null || value.isEmpty) {
                  return 'Please enter some text'; //只要不为 null   _formKey.currentState!.validate()为 false
                }
                return null; //此时 _formKey.currentState!.validate()为 true
              },
              textInputAction:
                  TextInputAction.next, //键盘上点击确认后焦点会移至下一个 TextField
            ),
            TextFormField(
              focusNode: focusNode2, //该TextFormField 关联focusNode2
              decoration: InputDecoration(
                  labelText: "密码",
                  hintText: "您的登录密码",
                  prefixIcon: Icon(Icons.lock)),
              obscureText: true, //输入隐藏
              validator: (String? value) {
                if (value == null || value.trim().length <= 5) {
                  //必须判断是否为 null 否则报错
                  //trim() 取除首尾空格
                  return "密码不能少于6位";
                }
                return null;
              },
            ),
            ElevatedButton(
                onPressed: () {
                  FocusScope.of(context).requestFocus(focusNode2); //切换焦点zhi
                },
                child: Text("切换焦点")),
            Container(//需在 From 里创建一个 Builder 才能得到
                child: Builder(builder: (context) {
              return ElevatedButton(
                  onPressed: () {
                    if (_formKey.currentState!.validate()) {
                      // 将之前 retrun 的内容 显示在相应的文本框底
                      //之前 return 有内容 则为 false
                      // Process data.
                    }
                    print((_formKey.currentState as FormState)
                        .validate()); //as 是强制转换的意思
                    //if 里的写法 与该句写法效果相同

                    // 当所有编辑框都失去焦点时键盘就会收起
                    focusNode1.unfocus();
                    focusNode2.unfocus();
                    //需在 From 里创建一个 Builder 才能得到
                    print(Form.of(context).toString());
                  },
                  child: Text('Submit'));
            }))
          ],
        ));
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/7TextFieldAndForm.dart';

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
      body: TextFieldAndFormDemo(),
    ); //脚手架
  }
}

```

