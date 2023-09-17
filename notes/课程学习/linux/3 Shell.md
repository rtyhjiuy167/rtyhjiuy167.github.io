#### 执行Shell脚本

注意：

1. 使用Shell调用脚本

   `bash 文件名`

2. 给脚本赋予执行权限

   `chmod u+x 文件名`，之后直接调用该脚本名即可运行该脚本。
   
   注意：以`文件名`形式直接调用，应保证脚本文件放在 PATH 所列出的几个目录之中
   
   ```shell
   #
   PATH=$PATH: ./
   #
   export PATH
   ```
   
   或者使用决定路径和相对路径形式来调用

### 功能性语句

#### read命令

```shell
# 读取单个字符串
echo -e "Input your name:\c";
read uname;
echo -e "your name is $uname";
```

```shell
# 读取多个字符串 字符串之间以空格隔开
echo -e "Input date with format yyyy mm dd:\c";
read year month day
echo -e "Today is $year/$month/$day";
```

#### expr命令

expr 要求运算符的左、右两侧都至少要加上一个空格。在命令行上“\*”是具有文件名通配符含义的特殊字符，因此在用“\*”进行乘法运算时，需要在其前面加上反斜线来去掉其特殊字符的含义。

```shell
expr 12 + 5 \* 3
```

### 结构性语句

#### 测试语句

|     测试运算符     |           测试内容           |
| :----------------: | :--------------------------: |
| string1 = string2  |     string1 等于 string2     |
| string1 != string2 |    string1 不等于 string2    |
|       string       |        string 不为空         |
|     -z string      |       string 的长度为0       |
|     -n string      |      string 的长度不为0      |
|   int1 -eq int2    |        int1 等于 int2        |
|   int1 -ne int2    |       int1 不等于 int2       |
|   int1 -gt int2    |        int1 大于 int2        |
|   int1 -ge int2    |      int1 大于等于 int2      |
|   int1 -lt int2    |        int1 小于 int2        |
|   int1 -le int2    |      int1 小于等于 int2      |
|    -b filename     |      该文件是块设备文件      |
|    -c filename     |     该文件是字符设备文件     |
|    -d filename     |       该文件是目录文件       |
|    -f filename     |       该文件是普通文件       |
|    -g filename     | 该文件设置了 set-group-ID 位 |
|    -p filename     |       该文件是管道文件       |
|    -r filename     |          该文件可读          |
|    -s filename     |       该文件大小不为0        |
|    -u filename     | 该文件设置了 set-user-ID 位  |
|    -w filename     |          该文件可写          |
|    -x filename     |         该文件可执行         |

#### if⋯then⋯fi

if与fi是一对语句括号，必须成对使用

```shell
if 表达式
	then
		命令表
fi
# 可把 fi 看成结束的关键字
```

```shell
if [ 2 -eq 2 ]
then
	echo " 2 = 2"
fi
```

#### if⋯then⋯else⋯fi

#### case⋯esac

可使用"[ ]"、"?"和”*“通配符来标识某个范围的字符串

```shell
echo -e "Input your choice:(y/n) [ _ ]\b\b\b\c";
read choice;
case "$choice" in
	[Yy])
		echo -e "Yes"
		;;
	[Nn])
		echo -e "No"
		;;
	*)
		echo -e "error"
		;;
esac
```

#### for⋯do⋯done

```shell
for city in Beijing Chengdu Guangzhou Shanghai Tianjing
do
	echo "Good moring,$city"
done
```

```shell
# 复制指定文件或全部文件到 $HOME/backup
flist=`ls`
for file in $flist
do
	if [ $# -eq 1 ]	# 命令行有一个参数时
	then
		if [ "$file" = "$1" ]
		then
			cp $file $HOME/backup
			echo "$file copied"
			exit
		fi
	else
		cp $file $HOME/backup
		echo "$file copied"
	fi
done
# bash test "test"
```

#### while⋯do⋯done

#### until⋯do⋯done

#### 循环控制语句 continue break

### Shell函数

```shell
check_user()
{
	user=`who | grep $1`
	if [ -n $user ]
	then
		return 0
	else 
		return 1
	fi
}
while true
do
	echo -n "Input username:"
	read uname
	check_user $uname
	if [ $? -eq 0 ] # #? 上一条命令的退出状态
	then
		echo "user $uname online"
	else
		echo "user $uname offline"
	fi
done
```







