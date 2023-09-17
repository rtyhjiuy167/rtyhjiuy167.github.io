---
title: dart新内容
top: 100
tags:
  - flutter
categories:
  - flutter
---

```dart
/*? 可空类型 */
String? getData(String? apiUrl) {
  if (apiUrl != null) {
    return "this is server data";
  }
  print(apiUrl!.length);
  return null;
}

/*! 类型断言  
    当其为空时抛出异常，不为空时执行后续代码
*/

/*try catch 
    若 try 里抛出异常，则 catch 会接收到异常并执行其里的代码
*/
void printLength(String? str) {
  try {
    print(str!.length);
  } catch (e) {
    print("str is null");
  }
}

class Person {
  late String name; //没有构造函数时，使用 late 延迟初始化
  late int age;
  //Person(this.name, this.age);
  void setName(String name, int age) {
    this.name = name;
    this.age = age;
  }

  String getName() {
    return "${this.name}---${this.age}";
  }
}

void main() {
  Person p = new Person();
  p.setName("张三", 20);
  print(p.getName());
  print(printUserInfo("213", age: 20, sex: "女"));
}

//命名参数中必须有 required 在类里也是一样的
//也可赋默认值
String printUserInfo(String username, {int age = 10, required String sex}) {
  if (age != 0) {
    return "姓名：$username---性别：$sex---年龄：$age";
  }
  return "姓名：$username---性别：$sex---年龄保密";
}

```

