---
title: 12-1 requests
top: 12.1
tags:
  - python
categories:
  - python
---

中文文档：https://docs.python-requests.org/zh_CN/latest/

安装requests库：`pip install requests`

```python
import requests

# 定义请求的 url
url = 'https://www.baidu.com'

# 发起get 请求
res = requests.get(url)

# 获取响应结果
print(res)  # <Response [200]>

# 获取二进制文本流
print(res.content)

# 把二进制文本流按照utf-8的字符集转化为普通字符串
print(res.content.decode('utf-8'))

# 获取响应的内容
print(res.text)

# 用json接收返回的数据
# print(res.json())

# 获取响应头信息
print(res.headers)

# 获取请求状态码
print(res.status_code)  # 200

# 获取请求的url
print(res.url)

# 获取请求头信息
print(res.request.headers)
```



```python
headers = {
    # User-Agent 用户代理 有些网站会拒绝python程序访问，可以使用用户代理，伪造身份
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:92.0) Gecko/20100101 Firefox/92.0'
}
```

```python
res = requests.post(url2, headers=headers,data=data)
```

```python
req = requests.session()
res = req.post(url2, headers=headers,data=data)
```