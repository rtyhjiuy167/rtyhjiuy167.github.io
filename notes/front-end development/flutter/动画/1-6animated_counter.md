---
title: 45 animated_counter
top: 45
tags:
  - flutter
categories:
  - flutter
---

```dart
class AnimantedCounter extends StatelessWidget {
  final Duration duration;
  final double count;

  final double width;
  final double height;
  final Color color;
  late double fontSize;
  //命名参数
  AnimantedCounter({
    required this.duration,
    required this.count,
    this.width = 200,
    this.height = 300,
    this.color = Colors.blue,
  }) {
    this.fontSize = this.height;
  }
  @override
  Widget build(BuildContext context) {
    return Container(
      //在这里使用 alignment: Alignment.center 无效 这是子组件里的 Stack 导致
      color: color,
      width: width,
      height: height,
      child: TweenAnimationBuilder(
          tween: Tween(end: count),
          duration: duration,
          builder: (BuildContext context, double value, Widget? child) {
            final whole = value ~/ 1; //整数
            final decimal = value - whole; //小数
            print("$whole+$decimal");
            return Stack(
              children: [
                Positioned(
                  top: (-fontSize) * 0.058 - fontSize * decimal, //对子组件的位置略微调整
                  child: Opacity(
                    opacity: 1.0 - decimal,
                    child: Container(
                      height: height,
                      width: width,
                      alignment: Alignment.center, //即时居中了，上下的宽度还是略微有点不协调
                      child: Text(
                        "$whole",
                        style: TextStyle(
                          fontSize: fontSize,
                        ),
                      ),
                    ),
                  ),
                ),
                Positioned(
                  top: (-fontSize) * 0.058 + fontSize - fontSize * decimal,
                  child: Opacity(
                    opacity: decimal,
                    child: Container(
                      height: height,
                      width: width,
                      alignment: Alignment.center, //即时居中了，上下的宽度还是略微有点不协调
                      child: Text(
                        "${whole + 1}",
                        style: TextStyle(
                          fontSize: fontSize,
                        ),
                      ),
                    ),
                  ),
                ),
              ],
            );
          }),
    );
  }
}

```

