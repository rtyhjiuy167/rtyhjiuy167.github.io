---
title: 12-2 Xpath
top: 12.2
tags:
  - python
categories:
  - python
---

文档：https://www.w3school.com.cn/xpath/index.asp

```python
from lxml import etree

text = '''
<!DOCTYPE html>
<html lang="zh-CN">
<head>
</head>
<body>
    <ul>
        <li><a href="/a">python</li>
        <li><a href="/b">java</li>
        <li><a href="/c">go</li>
    </ul>
</body>
</html>
'''
# 方法1：直接解析字符串
html = etree.HTML(text)
print(html)  # <Element html at 0x17a55daf480>

# 方法2：读取一个html文件并解析
html = etree.parse('./test.html', etree.HTMLParser())
```

```python
from lxml import etree

text = '''
<!DOCTYPE html>
<html lang="zh-CN">
<head>
</head>
<body>
    <ul>
        <li><a href="/a">python</li>
        <li><a href="/b">java</li>
        <li><a href="/c">go</li>
    </ul>
    <div class="123">
        <ul>
            <li><a href="/aa">js</li>
            <li><a href="/bb">php</li>
            <li><a href="/cc">C++</li>
        </ul>
    </div>
    <div class="321">
        <ul>
            <li><a href="/1a">C</li>
            <li><a href="/1a">dart</li>
        </ul>
    </div>
</body>
</html>
'''

html = etree.HTML(text)

# 提取数据
r = html.xpath('/html/body/ul/li')
print(r)
# [<Element li at 0x15fdf37d100>, <Element li at 0x15fdf37d080>, <Element li at 0x15fdf37d140>]

# 获取所有符合的信息
r = html.xpath('/html/body/ul/li/a/text()')
print(r)  # ['python', 'java', 'go']

# Xpath 里的索引从1开始
r = html.xpath('/html/body/ul/li[1]/a/text()')
print(r)  # ['python']

# // 
r = html.xpath('//li/a/text()')  # //li 选取所有li元素
print(r)  # ['python', 'java', 'go', 'js', 'php', 'CS']

# @
r = html.xpath('//div[@class="123"]//li/a/@href')
print(r)  # ['/aa', '/bb', '/cc']
```

