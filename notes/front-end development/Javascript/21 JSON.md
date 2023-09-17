---
title: 23 JSON
top: 23
tags:
  - JavaScript
categories:
  - JavaScript
---

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
    </style>
</head>

<body>
    <script>
        //JSON JavaScript中的对象只有自己认识，其它语言都不认识
        // JSON就是一个特殊的字符串，这个字符串可以被任意的语言所识别
        //  并且可以转换为任意语言的对象，JSON在开发中主要用来数据的交互
        // JSON  JavaScript Object Notation JS对象表示法
        // JSON和JS对象的公式一样，只不过JSON字符串中的属性名必须加双引号
        //      不能是单引号
        // JSON分类 1.对象{}  2.数组[]
        // JSON中允许的值 
        // 1.字符串 2.数字 3.布尔值 4.null 5.普通对象（不能是函数） 6.数组
        var obj = '{"name":"孙悟空","age":18,"gender":"男"}';
        var arr = '[1,2,3,"hello",true]';
        var obj2 = '{"arr":[1,2,3]}';
        var arr2 = '[{"name":孙悟空,"age":18,"gender":"男"},{"name":孙悟空,"age":18,"gender":"男"}]';
        /** 
         * JSON -> JS对象
         * JSON.pars()
         *      参数为JSON字符串
         * 
        */
        var o = JSON.parse(obj);
        console.log(o.gender);//男
        var o2 = JSON.parse(arr);
        console.log(o2[1]);//2
        /** 
         * JS对象 ->JSON
        */
        var obj3 = { "name": "孙悟空", "age": 18, "gender": "男" };
        console.log(JSON.stringify(obj3));

        //eval() 可以执行一段字符串形式的JS代码
        // 如果使用eval()执行的字符串中含有{},它会将{}当成代码块
        //  如果不希望将其当成代码块解析，则需要在字符串前后各加一个()
        // 但在开发中尽量不要使用，有安全隐患,由用户传恶意代码可能造成危险
        var str1 = "alert(1)";
        eval(str1);
        console.log(eval("(" + obj + ")"));

    </script>

</body>

</html>
```

