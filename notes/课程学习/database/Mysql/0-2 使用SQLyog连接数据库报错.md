---
title: 0-2 使用SQLyog连接数据库报错
top: 0.2
tags:
  - mysql
categories:
  - mysql
---

使用SQLyog连接数据库报错plugin caching_sha2_password could not be loaded

```shell
# 更换验证方式即可

# 修改加密规则
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; 

# 更新一下用户的密码 
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'; 

# 刷新权限
FLUSH PRIVILEGES; 

# 重设密码
alter user 'root'@'localhost' identified by '123456'; 
```