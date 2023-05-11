### Gitlab安装

1）安装相关依赖：

```
yum -y install policycoreutils openssh-server openssh-clients postfix
yum -y install policycoreutils-python  
```

2）启动ssh服务并设置为开启启动：

```
systemctl enable sshd && sudo systemctl start sshd
```

3）启动postfix服务并设置为开启启动：

```
systemctl enable postfix && systemctl start postfix
```

4）开放ssh以及http服务，然后重新加载防火墙列表，如果防火墙处于关闭状态，则无需做该配置。

```
firewall-cmd --add-service=ssh --permanent
```

```
firewall-cmd --add-service=http --permanent
```

```
firewall-cmd --reload
```

5）下载gitlab

```
wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-12.4.2-ce.0.el7.x86_64.rpm
```

6）修改配置

```
cd /etc/gitlab
```

```
vi gitlab.rb
```

修改下列信息：

```
external_url 'http://IP:82'
```

```
nginx['listen_port'] = 82
```

7）重载配置

```
gitlab-ctl reconfigure
```

8）启动gitlab

需要约10分钟

```
gitlab-ctl restart
```

9）把端口添加到防火墙，如果防火墙处于关闭状态，则无需做该配置。

```
filewall-cmd --zone=public --add-port=82/tcp -permanent
```

```
filewall-cmd --reload
```

10）更新密码

```
12345678
```

11）登录

```
root
12345678
```

## jenkins安装

官网：https://www.jenkins.io/zh/



安装完后访问：http://localhost:8080/创建用户并配置插件镜像：

```
https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```

将`updates/default.json`文件中的`updates.jenkins.io/download`换成`mirrors.tuna.tsinghua.edu.cn/jenkins`

将`www.google.com`替换为`www.baidu.com`

## Windows

#### 忘记密码

Windows下进入`C:\ProgramData\Jenkins\.jenkins\users`目录，该目录里不同文件夹存储不同用户信息，点击进入待修改用户的文件夹，打开`config.xml`，将文件里`passwordHash`标签里的内容改为:

```
#jbcrypt:$2a$10$DdaWzN64JgUtLdvxWIflcuQu2fgrrMSAMabF5TSrGK5nXitqK9ZMS
```

之后进入任务管理器，重启jenkins服务。

此时登录密码已重置为111111。

### 配置环境变量

登录进Jenkins，在`Dashboard`页面下的左侧栏处点击`Manage Jenkins`，之后在右侧的`System Configuration`下找到`Configure System`，点击进入后找到`Environment variables`，勾选后点击`Add`添加。

`Name`即键填`PATH`，值把系统环境变量的PATH的值以文本复制一遍，并把其中表示引用的%%的替换掉。

点击`Apply`并重启服务即可。

### 安装必要插件 

1）Credentials Binding

登录进Jenkins，在`Dashboard`页面下的左侧栏处点击`Manage Jenkins`，之后在右侧中找到`Manage Plugins`，搜索`Credentials Binding`，找到后勾选并点击`install without restart`。

等待安装完成后，在`Dashboard`的 `Manage Jenkins`右侧找到`Security`中的`Manage Credentials`，点击进入后，找到在右侧的`Stores scoped to Jenkins`下的第一行，`Store`为`Jenkins`，`Domains`为`(golbal)`，将鼠标放置上`(golbal)`，会出现下拉框，点击创建凭证。

2）Git

登录进Jenkins，在`Dashboard`页面下的左侧栏处点击`Manage Jenkins`，之后在右侧中找到`Manage Plugins`，搜索`Git`，找到后勾选并点击`install without restart`。

在本机上安装Git，并配置相应账户信息。

3）Maven Integration

在`Global Tool Configuration`中配置MAVEN相关变量。在`name`中自定义名字，不勾选`Install automatically`，而是手动添加`MAVEN_HOME`环境变量

```
java -jar jar包名
```

4）nodejs

安装好插件后，在`Dashboard`页面下的左侧栏处点击`Global Tool Configuration`，找到`NodeJS`，点击`Add NodeJS`

5）Publish Over SSH

安装好插件后，在`Dashboard`页面下的左侧栏处点击`Configure System`，找到`Publish over SSH`，点击`add`，`Name`是方便在Jenkins上做以区别的，`Hostname`即IP地址，`Username`登录用户，如果使用秘钥登录，可在上方的`Key`中复制秘钥，如果使用密码登录，勾选`Use password authentication, or use a different key`，在`Passphrase / Password`出填写密码即可

构建项目时，在`Post Steps`处点击`Add post-build step`并选择`Send files or execute commands over SSH`，`Source files`处填写`**/*.jar`（可在构建的日志中找到生成的地址，这里用通配即可），`Remote directory`处填写传送文件的目的地址，会传动家目录里。

传送结束即启动，`Exec command`处填写，`nohup java -jar 家目录/填写的地址/jar包 >mylog.log 2>&1 &`

超时机制：在`Advanced`中的`Exec timeout (ms)`设置

执行前操作：

在`Pre Steps`处点击`Add pre-build step`并选择`Send files or execute commands over SSH`，在`Exec command`处输入`./x.sh`（路径为相对于家目录）

在家目录下，`vi x.sh`，并复制一下内容：

```bash
#!/bin/bash

# 删除历史数据  rm -rf 目录
rm -rf website-springboot

# 获取传入的参数
appname=$1

# 获取正在运行的 jar 包的 pid
pid=`ps -ef | grep $appname | grep 'java -jar' | awk '{printf $2}'`

#空值判断
if [ -z $pid ];
	then 
		echo "$appname not find"
	else
		# 结束进程 -9 无条件结束
		kill -9 $pid
		check=`ps -ef | grep -w $pid | grep java`
		if [ -z $check ];
			then
				echo "Failed to end $appname pid:$pid"
			else
				echo "$appname is killed"
		fi
fi
```

更改权限：`chmod 777 x.sh`

5)



### 构造

```bash
node --version
npm --version

```

```
npm i --registry https://registry.npm.taobao.org
```

```
npm run build
```

build项目在路径：`C:\ProgramData\Jenkins\.jenkins\workspace`

```bash
# 切换到nginx安装目录下的html文件夹
cd "D:\nginx-1.22.0\html"

# 移除web目录及其所有子目录及文件 
# /s 表示移除目标目录及其所有子目录及文件 
# /q 表示移除时不需要确认 
rd /s/q web

# 创建该项目文件夹 web
md web

# 复制 jenkins
# /Y 禁止提示
# /E 表示复制目标目录及子目录包括空的
xcopy "C:\ProgramData\Jenkins\.jenkins\workspace\build" "D:\nginx-1.22.0\html\web" /E /Y
```
