---
title: 0-1 mysql安装及操作
top: 0.1
tags:
  - mysql
categories:
  - mysql
---

<h4>mysqpl安装</h4>

下载地址：https://dev.mysql.com/downloads/mysql/

点击 `Windows(x86,64-bit),ZIP Archive` 右边的 `Download`，在跳转的新页面里点击 `No thanks,just start my download`

解压到自定义的文件夹里，例如`D:\mysql-8.0.25-winx64`，打开后即有`bin`、`docs`、`include`、`lib`、`share`文件夹和`LICENSE`、`README`文件

`8.0.23`之前需要添加`my.ini`文件

在`D:\mysql-8.0.25-winx64`目录下新建`my.ini`文件，在其写入如下内容

```ini
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:\mysql-8.0.25-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\mysql-8.0.25-winx64\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password

[mysql]
#设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```



配置环境变量：将`bin`的路径添加到`PATH`中

以下所有`cmd`均以管理与身份打开

```shell
# 直接以管理员身份打开命令行提示符或在bin目录下打开命令行提示符，初始化配置
mysqld --initialize-insecure --user=mysql --console
# 该命令没有设置密码
```

出现`Install/Remove of the Service Denied!`，即为不在管理身份下执行命令后出现的内容

```shell
# 以管理员身份打开命令行提示符
mysqld -install
# 出现 Service successfully installed. 即为成功
```

```shell
# 启动数据库服务
net start mysql
```

如果报错，错误信息为”发生系统错误2 系统找不到指定文件“，则在`bin`目录下打开终端，依次执行下面的三个命令即可：

```shell
mysqld --remove
mysqld --install
net start mysql
```

```shell
# 关闭数据库服务
net stop mysql
```

```shell
# 进入数据库 h 地址 P 端口号 u 用户名 p 密码
mysql -h localhost -P 3306 -u root -p
# 简写
mysql -u root -p
# 出现要输入密码，直接回车即可（因为没有设置）
```

此时出现`mysql>`即成功进入

```shell
# 修改密码，只有进入到数据库才能改密码，毕竟进前要输入密码确认身份嘛
alter user user() identified by "新密码"
```

```shell
# 查看 mysql 的版本 --已进入到 mysql 里的
select version();
# 在外边查看版本 
mysql --version 
# 简写
mysql -V
```

`Mysql` 里不区分大小写

<h4>mysql基础操作</h4>

```mysql
# 注意：命令须以分号或\g结尾
# 创建数据库
create database test default charset=utf8mb4;
create database test default charset=utf8;

create database if not exists test default charset=utf8;

# 查看所有的数据库
show databases;

# 进入某个数据库
use test;

# 查看某个库里的表
show tables from mysql;

# 在进入某个库的前提下查看库里的表
show tables

# 查看当前所在的库
select database();

# 在当前所在的库创建表 create table stuinfo(字段名 类型 字段约束, 字段名 类型 字段约束...);
# varchar(长度) 字符串
create table stuinfo(id int, name varchar(20))engine=innodb default charset=utf8;

create table stuinfo(id int, name varchar(20)); # 默认是的时候，可简写

# 删除库 drop database 库
drop database test;

# 在当前所在的库删除表      drop table 表
drop table stuinfo;

# 显示指定的表的结构
desc stuinfo;

# 显示指定的表的所有成员      select * from  表
select * from stuinfo;

# 显示指定的表的某些特定成员      select * from  表 where 字段=某个值
select * from stuinfo where id=1;

# 向指定的表中插入数据        insert into 库 (参数)  values(要传的参数)
insert into stuinfo (id,name) values(1,'john');

# 修改表中的数据           update 库 set 要修改的value=新值 where 根据其value定位
update stuinfo set name='lie' where id=1;

# 修改表中的数据           update 库 set 要修改的value=新值 where 根据其value定位
update stuinfo set name='lie' where id=1;

# 删除表中的数据
delete from studin where id=1;
```

表引擎推荐使用innodb
