---
title: 20 DataTable
top: 20
tags:
  - flutter
categories:
  - flutter
---

```dart
import 'package:flutter/material.dart';

class DataTableDemo extends StatefulWidget {
  const DataTableDemo({Key? key}) : super(key: key);

  @override
  _DataTableDemoState createState() => _DataTableDemoState();
}

class _DataTableDemoState extends State<DataTableDemo> {
  late List<Map> list;
  bool isSelect = true;

  int _sortColumnIndex = 0;
  bool _sortAscending = true;
  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    list = [];
    for (var i = 1; i < 5; i++) {
      list.add({
        "name": "QAQ" * i,
        "age": (20 + i).toString(),
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        DataTable(
            //表头
            columns: [
              DataColumn(label: Text("姓名")),
              DataColumn(label: Text("年龄")),
            ],
            //成员
            rows: [
              DataRow(
                  cells: [DataCell(Text("张三")), DataCell(Text("20"))],
                  selected: isSelect, //是否选中 默认为 false
                  onSelectChanged: (v) {
                    setState(() {
                      isSelect = !isSelect;
                    });
                  }), //只要有一个写下此属性 则会在表格左边加一列 方框用于选中与执行回调
              //其它没写的 选中时方框呈灰色
              DataRow(
                cells: [DataCell(Text("李四")), DataCell(Text("18"))],
                selected: !isSelect,
              ),
              DataRow(
                cells: [DataCell(Text("王五")), DataCell(Text("22"))],
                selected: false,
              )
            ]),
        SizedBox(
          height: 20,
        ),
        DataTable(
            sortColumnIndex: _sortColumnIndex, //以哪一列的数据内容来排序 从 0 开始
            sortAscending: _sortAscending, //是否按升序排列 默认为 true
            columns: [
              DataColumn(
                  label: Text("姓名"),
                  onSort: (columnIndex, ascending) {
                    setState(() {
                      _sortColumnIndex = columnIndex;
                      _sortAscending = ascending;
                      list.sort((a, b) {
                        if (!ascending) //如果不是升序
                        {
                          return b["name"].toString().length -
                              a["name"].toString().length;
                        }
                        return a["name"].toString().length -
                            b["name"].toString().length;
                      });
                    });
                  }),
              DataColumn(
                  label: Text("年龄"),
                  onSort: (columnIndex, ascending) {
                    print(columnIndex);
                    print(ascending);
                    setState(() {
                      _sortColumnIndex = columnIndex;
                      _sortAscending = ascending;
                      list.sort((a, b) {
                        if (!ascending) //上一种写法与该写法效果相同
                        {
                          return b["age"].compareTo(a["age"]);
                        }
                        return a["age"].compareTo(b["age"]);
                      });
                    });
                  }),
            ],
            rows: list
                .map((e) => DataRow(
                      cells: [
                        DataCell(Text(e["name"])),
                        DataCell(Text(e["age"]))
                      ],
                    ))
                .toList()),
      ],
    );
  }
}

```

```dart
import 'package:flutter/material.dart';
import 'widgets/20DataTable.dart';

void main() => runApp(MyApp());

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
      body: DataTableDemo(),
    );
  }
}

```

