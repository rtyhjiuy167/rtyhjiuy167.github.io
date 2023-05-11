#### Shell 可识别的命令类型

1. 单条命令

2. 串行命令

   以分号的形式隔开不同的单条命令，从左到有一次执行。

   ```shell
   # 例如
   who;pwd;date
   ```

3. 命令组

   * 使用圆括号形式的命令组

     执行结果与串行命令相同，但所有命令的输出会作为整体输出到指定文件中

     ```shell
      # 例如
     (date;pwd;who) > mdata
      # 结果可用 cat mdata 进行查看
     ```

   * 使用管道形式的命令组

     管道用于连接两条命令，把第一条命令的执行结果传送给第二条命令，作为其运行参数

     ```shell
     # 例如
     ls |grep lib
     # grep命令可查找指定字符串所在行
     ```

4. 后台命令

   将程序放到后台执行。在命令的后面加上 & 

### Shell的内部命令

1. time

   time命令用来显示命令（程序）的执行时间，常常在程序调试优化时使用。使用方式就是在普通命令的前面加上time。

   ```shell
   $ time ls
   
   real	0m0.001s
   user	0m0.000s
   sys	0m0.001s
   # user 表示用户程序部分的运行时间，sys表示系统程序部分的运行时间
   # 大多数情况下,real > user + sys，因为在多用户、多进程环境下，程序的总运行时间中还包含进程的等待和调度所花费的时间
   ```

2. echo

   输出。

   ```shell
   echo "hello,world"
   ```

   ```shell
   # -e 选项开启反斜线的转义功能 -n表示禁止换行
   echo -e "hello,\nworld"
   ```

   ```shell
   # \c 禁止回车符
   # \b 回退符
   echo -e "Input your choice (y/n) [ _ ]\b\b\b\c";read answer
   ```


#### 标准输入/输出重定向

```shell
# cat 输入来源 输出方向 标准错误输出 标准错误输出方向
cat <aa >bb 2> cc
```



