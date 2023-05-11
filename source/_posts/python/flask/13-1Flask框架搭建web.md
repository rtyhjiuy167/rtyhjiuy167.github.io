---
title: 13-1 Flask框架搭建web
top: 13.1
tags:
  - python
categories:
  - python
---

<h4>flask框架基本实现</h4>

安装Flask库：`pip install -U Flask`，文档：https://pypi.org/project/Flask/

```python
from flask import Flask

# 实例化
app = Flask(__name__)

# 定义路由
@app.route("/")
def hello():
    return "Hello, World!"


if __name__ == '__main__':
    app.run(debug=False, host='127.0.0.1', port='8080')
```

### 交互

后端：

```python
from flask import Flask
from flask import jsonify
from flask import request
import json

# 实例化
app = Flask(__name__)


# 定义路由
@app.route("/")
def hello():
    return "Hello, World!"


@app.route("/get_test", methods=['GET'])
def get_test():
    return jsonify({"get_test": "测试"})  


@app.route("/post_test", methods=['POST'])
def post_test():
    if request.method == 'POST':
        data = request.get_data()
        data = json.loads(data)
        print(data)
        return jsonify({"post_test": "测试", "msg": "", "status": "000", "results": data})


if __name__ == '__main__':
    app.run(debug=False, host='127.0.0.1', port='8080')
```

前端：

```python
import requests
import json

text = "你好。"
data = {"texts": text, "userID": "1234567"}

url = "http://192.168.1.103:8080/emotion_test"
headers = {"Content-Type": "application/json"}
r = requests.post(url=url, headers=headers, data=json.dumps(data))
if r:
    print(r.json())
```

