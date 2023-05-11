```shell
sudo yum install postgresql
sudo yum install postgresql-server
postgres --version
sudo passwd postgres # 之后设置成123456
```

```shell
cd ../ #以root用户切换到根目录
mkdir postgresql_js 
chown -R postgres:postgres postgresql_js
```

```shell
su postgres
initdb -D postgresql_js 
```

```shell
pg_ctl start -D postgresql_js
```

使用

```shell
su postgres # 切换到postgres用户，密码为123456
```

```shell
# 进入到数据库命令行界面
psql
```

```shell
# 查看所有数据库
\l
```

```shell
# 创建一个数据库
create database test;
```

```shell
# 进入指定数据库
\c test;
```

```shell
# 退出数据库命令行界面
\q
```

Postgresql使用的端口是5432