### 安装Docker

1）安装相关配置：

```
yum install gcc
yum install gcc-c++
```

```
sudo yum install -y yum-utils
```

2）设置镜像仓库：

```
sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

3）更新yum软件包索引：

```
yum makecache fast
```

4）安装DOCKER CE:

```
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

5）开启docker：

```
systemctl start docker
# 开机自启
systemctl enable docker.service
# 取消开启自启
systemctl disable docker.service
```

6）安装可视化界面

```bash
# 创建容器卷
docker volume create portainer_data

#查看已创建数据卷
docker volume ls

#下载Portainer server镜像并运行容器，Portainer服务端本质也是运行在容器环境里
docker run -d -p 9000:9000  --restart=always  -v /var/run/docker.sock:/var/run/docker.sock  --name portainer  docker.io/portainer/portainer

# 删除不使用且未绑定的的数据卷
docker volume ls   --filter dangling=true | grep local |awk '{print $2}'|xargs docker volume rm
```

7）访问并创建初始用户

```
http://IP:9000
admin
dockerQSC123
```

本地没有 hello-world镜像，则会下载。

8）镜像加速：前往阿里云配置容器镜像服务



### 停止与卸载

停止：

```
systemctl stop docker
```

卸载：

```bash
sudo yum remove docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

### docker与虚拟机

1）docker 有着比虚拟机更少的抽象层

由于 docker 不需要Hypervisor虚拟机实现硬件资源虚拟化，运行在 docker 容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上 docker 将会在效率上有明显优势。

2）docker 利用的是宿主机的内核，而不需要加载操作系统内核

当新建一个容器时，docker 不需要和虚拟机一样重新加载一个操作系统内核。

### 命令

##### 帮助启动类命令

```bash
systemctl start docker

systemctl stop docker

# 重启 docker: 
systemctl restart docker

# 查看 docker 状态: 
systemctl status docker

# 开启启动: 
systemctl enable docker

# 查看docker概要信息:
docker info

# 查看 docker 总体帮助文档：
docker --help

# 查看 docker 具体命令帮助文档：
docker 具体命令 --help
```

##### 镜像命令

```bash
# 列出主机上的镜像
# -a 列出本地所有的镜像包括历史印象层
# 
docker images

# 查询有什么镜像可下载
docker search --limit 5 镜像名字

# 从 docker 镜像源服务器拉起指定镜像
docker pull 镜像名字[:TAG]

# 查看镜像/容器/数据卷所占的空间
docker system df

# 删除 
# -f 强制删除
docker rmi 镜像ID

# 删除所有虚悬镜像
docker image prune

# 删除全部
docker rmi -f $(docker images -qa)
```

虚悬镜像值仓库名、标签都是\<none\>的镜像，没什么用，删除

##### 容器命令

```bash
# --name="容器新名字"
# -i 以交互模式运行容器 -t 为容器重新分配一个伪输入端 一般一起使用 -it
# -p 指定端口映射
# -d 指定容器在后台运行，不过必须有一个前台进程在运行
# 带shell脚本的交互ubuntu 一个镜像可以运行后成为容器
docker run -it --name=myubuntu ubuntu /bin/bash

# 列出容器
# -q 显示容器ID
# -a 列出当前正在运行+历史上运行过的容器
# -n 列出最近指定个数创建的容器
docker ps

# 进入后容器以停止方式退出
exit

# 进入后容器退回，容器不会停止
ctrl+p+q

# 重启容器
docker start 容器ID或容器名

# 停止容器 
docker stop 容器ID或容器名

# 强制停止容器
docker kill 容器ID或容器名

# 删除已停止的容器
docker rm 容器ID

# 删除所有容器
docker rm -f $(docker ps -a -q)

# 查看容器日志
docker logs 容器ID

# 查看容器内运行的进程
docker top 容器ID

# 查看容器内部细节 可以查看挂在哪
docker inspect 容器ID

# 使用exec进入容器，打开新终端，用exit退出不会导致容器停止
docker exec -it 容器ID

# 使用dispatch进入容器，进入之前的终端，用exit退出会导致容器停止
docker attach -it 容器ID

# 拷贝容器内容到主机上
docker cp 容器ID:容器内部路径 主机路径

# 从 tar 包中的内容创建一个新的文件系统导入到容器
# docker import - REPOSITORY:TAG
cat 文件名.tar | docker import - haha:1.0

# 导出容器的内容作为一个 tar 归档文件
docker exprot 容器ID > 名字.tar

# 提交容器副本使之成为一个新的镜像
docker commit -m="消息" -a="作者" 容器ID 包名/TAG
```

### docker Registry

本地私服库

docker默认不允许http推送镜像，需要 `vim /etc/docker/daemon.json`，并修改为：

```json
{
    "registry-mirrors": ["https://4leifcdf.mirror.aliyuncs.com"],
    "insecure-registries":["IP:Registry的端口"]
}
```

之后重启docker，再启动私服库。

```bash
# -p 5000:5000 主机端口:容器端口
# -v 添加自定义容器卷 容器卷可以把数据备份到宿主机
# /zzyyuse/myregistry/ 宿主机的路径 没有会自动创建
# /tmp/registry 容器内路径
# /tmp/registry:ro 容器只读 read only
# --privileged=true 容器中具有root权限
docker run -d -p 5000:5000 -v /zzyyuse/myregistry/:/tmp/registry --privileged=true registry


# 推送镜像到私服库
docker tag 镜像名:TAG IP:Registry的端口/镜像名:TAG

# 从私服库拉取镜像
docker pull IP:Registry的端口/镜像名:TAG
```

### 镜像

镜像是一种轻量级、可执行的独立软件包，它包含运行某个软件所需的所有内容，我们把应用程序和配置依赖打包好形参一个可交付的运行环境（包括代码、运行时需要的库、环境变量和配置文件等），这个打包好的运行环境就是image镜像文件。

只有通过镜像文件才能生成镜像实例。

### UnionFS

UnionFS（联合文件系统）：Union文件系统是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下。Union文件系统是Docker镜像的基础。镜像可以通过分层来进行基础，基于基础镜像，可以指作各种具体的应用镜像。

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统。

### Docker镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统就是UnionFS。

bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是引导文件系统bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs(root file system)，在bootfs之上。包含的就是典型Linux系统中的/dev, /proc, /bin,/etc等标准目录和文件。roots就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。

### 为什么Docker里的OS如此小

对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接用Host的kernel,自己只需要提供 rootfs就行了。由此可见对于不同的linux发行版, bootfs基本是一致的, rootfs会有差别,因此不同的发行版可以公用bootfs 。

### 数据卷

数据卷可以在容器之间共享和重用数据

卷的更改可以直接在容器中直接实时生效，即使容器没有开启，但开启后即会同步

数据卷的生命周期一直持续到没有容器使用它为止

### 安装软件

##### 安装mysql

官网：https://hub.docker.com/_/mysql

```bash
docker pull mysql

# 即使容器示例删了，如果卷关联相同，数据仍可恢复
docker run -d -p 3306:3306 --privileged=true -v /zzyyuse/mysql/log:/var/log/mysql -v /zzyyuse/mysql/data:/var/lib/mysql -v /zzyyuse/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 --name mysql mysql

docker exec -it mysql /bin/bash
```

```bash
# 从容器中进入mysql
mysql -uroot -p
# 修改旧密码 必须都执行 8.0 sql_secret
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';
ALTER USER 'root' IDENTIFIED WITH mysql_native_password BY '新密码';
```

可通过https://www.ipip.net/网站查自己所在的公网IP，设置安全组

```sql
# 查看字符集
show variables like 'character%';
```

```bash
# 回到宿主机后
cd /zzyyuse/mysql/conf

vim my.cnf
```

粘贴：

```
[client]
default_character_set=utf8
[mysqld]
collation_server = utf8_general_ci
character_set_server = utf8
```

```bash
docker restart mysql
```

设置自动启动：

```bash
sudo docker update mysql --restart=always
```

##### 安装redis

官网：https://hub.docker.com/_/redis

下载redis镜像：

```bash
docker pull redis:6.2.7
```

创建redis配置文件的目录结构：

```bash
mkdir -p /mydata/redis/conf
```

创建redis配置文件：

```bash
cd /mydata/redis/conf

touch redis.conf
```

运行并配置容器卷：

```bash
docker run -p 6379:6379 --name redis --privileged=true -d -v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf -v /mydata/redis/data:/data redis:6.2.7 redis-server /etc/redis/redis.conf --requirepass 'redies123'
```

进入redis：

```
docker exec -it redis redis-cli
exit
```

修改配置文件：

```bash
vi /mydata/redis/conf/redis.conf
```

粘贴：

```properties
appendonly yes
```

重启容器：

```bash
docker restart redis
```

设置自动启动：

```bash
sudo docker update redis --restart=always
```

##### 安装tomcat

```bash
# 下载最新的
docker pull tomcat

docker run -d -p 8080:8080 --name tocat1 tomcat
```

```
docker exec -it tocat1 /bin/bash
```

10版的tomcat的webapps目录下无文件。

```
cd webapps
```

检查webapps目录下文件：

```
ls -l
```

新版的tomcat里没有文件

```bash
cd ..
rm -r webapps
mv webapps.dist webapps
```

##### 安装 nginx

官网：https://hub.docker.com/_/nginx

```bash
docker pull nginx:1.21.6

mkdir -p /data/nginx/{conf,html,logs}
mkdir -p /data/conf
# 如果提示为文件夹，则手动创建
vim /data/conf/nginx.conf
```

`vim /data/nginx/conf/nginx.conf`，内容如下：

```
# 对应 cpu 内核数
worker_processes  1;


events {
	# 每个 worker 可以连接数
    worker_connections  1024;
}

http {
	#	引入其它配置文件
	# mime.types 里配置了相应的后缀对应的浏览器的解析方式
    include       mime.types; 
    
    # 如果后缀没有对应的方式，及 mime.types中没有定义，则按 application/octet-stream 方式
    default_type  application/octet-stream;

	# 零拷贝，nginx不加载文件，把一信号发给网络接口，让其把文件发给用户，nginx自己不读取文件
    sendfile        on;
    
    # 保持长连接的时间
    keepalive_timeout  65;
    
    # 一个 server 就是一个虚拟主机,nginx 可以开启多个虚拟主机
    server {
    	# 监听的端口，每个虚拟主机的监听端口与server_name的组合不能相同
        listen       80;
        
        # 主机名，可以配置IP或域名
        # docker 内必须配置IP或域名，不能为 localhost
        server_name  localhost;

		# 虚拟主机下的 / 目录，每个虚拟主机可以有多个 location
        location / {
        
        	# 匹配到 location 后，存放网页的文件目录，该目录为根目路下的文件夹，这里即为 html 文件夹
        	# 须为容器内的路径 
            root   /usr/share/nginx/html/;
           
            # 文件目录下展示的默认网页，即将 html 文件下的下面文件进行展示
            index  index.html index.htm;
        }
	
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }

}
```

```bash
# 里面的80对应外面的3000
docker run --name nginx -d -p 3000:80 -v /data/nginx/html:/usr/share/nginx/html -v /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /data/nginx/logs:/var/log/nginx -v /home/ruoyi/uploadPath:/home/ruoyi/uploadPath -d nginx:1.21.6
```

##### 安装elasticsearch

官网：https://hub.docker.com/_/elasticsearch

下载7.17.5版本的elasticsearch镜像：

```bash
docker pull elasticsearch:7.17.5
```

创建文件目录：

```bash
mkdir -p /mydata/elasticsearch/config
mkdir -p /mydata/elasticsearch/data
echo "http.host: 0.0.0.0" >> /mydata/elasticsearch/config/elasticsearch.yml
```

赋予权限：

```bash
chmod -R 777 /mydata/elasticsearch/
```

运行：

```bash
docker run --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" -v /mydata/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v /mydata/elasticsearch/data:/usr/share/elasticsearch/data -v /mydata/elasticsearch/plugins:/usr/share/elasticsearch/plugins -d elasticsearch:7.17.5
```

中文分词器下载安装：

```bash
cd /mydata/elasticsearch/plugins

mkdir ik

cd /mydata/elasticsearch/plugins/ik

wget https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.17.5/elasticsearch-analysis-ik-7.17.5.zip

yum install -y unzip zip

unzip elasticsearch-analysis-ik-7.17.5.zip

。。。内存2G不够用
```

尝试访问：

```
IP:9200
```

```
# 增或修改 指定索引、类型、id
PUT http://IP:9200/索引/类型/ID 内容

# 增 指定索引、类型
POST http://IP:9200/索引/类型 内容 

# 更新 指定索引、类型、id 最后加 /_update
POST http://IP:9200/索引/类型/ID/_update 内容 

# 查 指定索引、类型、id
GET http://IP:9200/索引/类型/ID

# 删除索引 指定索引
DELETE http://IP:9200/索引
```

在图形化界面里操作：

```
# 批量插入
POST  索引/类型/_bulk
{"index":{"_id":"1"}}
{"name":"Jack"}
{"index":{"_id":"2"}}
{"name":"Sam"}
```

### 安装kibana

官网：https://hub.docker.com/_/kibana

```bash
docker pull kibana:7.17.5
```

```bash
docker run --name kibana -e ELASTICSEARCH_HOSTS=http://IP:9200 -p 5601:5601 -d kibana:7.17.5
```

##### 安装支持jdk的镜像

官网：https://hub.docker.com/_/openjdk

```bash
vim Dockerfile
```

```bash
 # JDK 版本
FROM openjdk:8

# 暴露镜像内的端口为 8082 一般填项目的访问端口
EXPOSE 8082 

# 工作目录 ADD 中的目录是相对工作目录的
WORKDIR /root 

# 将宿主主机中的文件写入docker
ADD first/*.jar /root/app.jar 

ENTRYPOINT ["java","-jar","/root/app.jar"]
```

```bash
# 构建 名字为 demo
docker build -t demo .
```

```bash
docker run -d --name demo -p 8080:8080 demo
```

### mysql主从复制

待学习...

### Redis集群

待学习...

### Dockerfile

Dockerfile是用来构建Docker镜像的文本文件，由一条条构建镜像所需的指令和参数构成的脚本。

保留字：

* FORM 

  基础镜像，当前新镜像是基于那个镜像的，指定一个已经存在的镜像作为模板。

* MAINTAINER

  镜像围护桩的姓名和邮箱地址

* RUN

  容器构建（build）时执行

* EXPOSE

  当前容器对外暴露的端口

* WORKDIR

  指定创建容器后，中断默认登录的工作目录

* USER

  指定该镜像以什么样的用户去执行，默认为 root

* ENV

  用来在构建镜像过程中设置环境变量。设置的环境变量可以后面其他指令中进行引用

* ADD

  将宿主机目录下的文件拷贝进镜像且会自动处理URL和解压tar压缩包

* COPY

  类似ADD，拷贝文件和目录到镜像中

* VOLUME

  容器卷

* CMD

  指定容器启动后要执行的指令，DokerFile中允许有多个CMD，CMD会执行，但如果传参，只有最后一个CMD生效

  之前在运行容器的指令后面的`/bin/bash` 会覆盖CMD

* ENTRYPOINT

  与CMD一起使用，ENTRYPOINT为定参，CMD为变参

```bash
FROM centos
MAINTAINER rui

ENV MYPATH /user/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80
CMD /bin/bash
```

```bash
# 
docker build -t 新镜像名字:TAG .
```

### Docker 网络

docker启动后会产生`docker0`虚拟网桥。Docker启动一个容器时会根据Docker网桥的网段分配给容器一个IP地址，成为Container-IP。同时，Docker网桥是每个容器的默认网关。因为在同一个宿主机内的容器都介入同一个网桥，这样容器之间就能通过容器的Container-IP直接通信。

```bash
# 查看doker容器里已有的网络模式
docker network ls

# 创建网络
docker network create 名字
```

网络模式：

* bridge

  为每一个容器分配、设置IP，并将容器连接到一个`docker0`虚拟网桥。默认。

* host

  容器将不会虚拟出自己的网卡，而是使用宿主机的IP和端口

* none

  容器有独立的 Network namespace，但并没有对其进行任何网络设置，只有一个本地回环地址。

* container

  新创建的容器不会创建自己的网卡和配置自己的IP，而是和一个指定的容器共享IP、端口范围等

* 自定义网络

  创建默认模式的网络。运行容器时填上`--network 自定义网络`，则不同容器内部可以通过服务名进行彼此访问。

### Docker-Compose

Docker-Compose可以管理由多个Docker容器组成一个应用

官网：https://docs.docker.com/compose/install/compose-plugin/#installing-compose-on-linux-systems

```yml
version:"3"

services:
	microService:
		image: haha:1 # 镜像名:标签
		container_name: haha # 容器名
		prots:
			- "6001:6001" # 端口映射
		volumes:
			- /app/microService:/data
		networks:
			- myentwork # 自定义网络名
		depends_on:
			- mysql
	mysql:
		image: mysql:xxx
		environment:
			MYSQL_ROOT_PASSWORD: "123456"
			MYSQL_ALLOW_EMPTY_PASSWORD: "no"
			MYSQL_DATABASE: "haha" # 连接的数据库名
```

```
docker-compose stop
```

### Pointer

```

```

