---
title: 使用mysql数据库
top: 99
tags:
  - python
categories:
  - python
---

<h4>连接数据库</h4>

第三方库`pymysql`文档：https://pypi.org/project/PyMySQL/	执行`pip install pymysql`安装第三方库

```python
# 
import pymysql

# 1.连接mysql数据库
db = pymysql.connect(host='localhost',
                     user='root',
                     password='',
                     database='test',
                     cursorclass=pymysql.cursors.DictCursor)

# 2. 创建游标对象
cursor = db.cursor()

# 3.准备sql语句
sql = 'select version()'

# 4.执行sql语句
cursor.execute(sql)

# 5.提取结果
data = cursor.fetchall()

# 6.关闭数据库连接
db.close()

print(data)
```

```python
#	优化代码
import pymysql

# 1.连接mysql数据库
db = pymysql.connect(host='localhost',
                     user='root',
                     password='',
                     database='test',
                     cursorclass=pymysql.cursors.DictCursor)
try:
    # 2. 创建游标对象
    cursor = db.cursor()

    # 3.准备sql语句
    sql = 'select version()'

    # 4.执行sql语句
    cursor.execute(sql)

    db.commit()  # 在执行sql语句时，注意进行提交

    # 5.提取结果
    data = cursor.fetchall()
except:
    db.rollback()  # 当代码出现错误时，进行回滚，mysql语句不会生效
finally:
    # 6.关闭数据库连接
    db.close()

print(data)
```

