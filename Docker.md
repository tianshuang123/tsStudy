#  

# Docker

## 1.Docker概述

### 1.1Docker为什么出现

一款产品：开发--上线   两套环境！应用环境，应用配置！

开发----运维 。问题：我在我的电脑上可以运行

环境配置十分麻烦，每一个机器都要部署环境！费时费力

docker用来带着环境安装打包。

java  --- jar（环境）---打包项目带上环境（镜像）---（Docker仓库：商店）---下载我们发布的镜像---直接运行即可！



Docker  给出了解决方案！

 ![image-20230110220234335](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230110220234335.png)



Docker的思想就来自于集装箱！

JRE--多个应用（端口冲突）--原来都是交叉的！

隔离：Docker核心思想！打包装箱！每个箱子都是相互隔离的



Docker通过隔离机制将服务器利用到极致。

本质：所有的技术都是因为出现了一些问题，我们需要去解决，采取学习！



### 1.2Docker的历史

1. 2010年, 几个年轻的美国人成立了一家公司叫做dotCloud，这家公司主要做pass云计算服务，其底层技术上，dotCloud 平台利用了 Linux 容器技术,他们将自己的的技术命名为docker.
2. docker刚诞生的时候, 并没有引起行业的注意, dotCloud公司越来越难, 经济效益也不景气, 后来就要活不下去了, 他们有强烈的愿望, 希望能活下去. 于是, 想了一个办法, 将docker开源.
3. 2013年, docker开源了. 并且他们也将公司正式改名为Docker. docker受到越来越多的人关注. 渐渐的docker就火了. 活了以后, docker每个月就会更新一个版本. 2014年.4.9 docker1.0发布.

从这里, 我们可以学到什么?



没有谁的成功是一帆风顺的, 如果不是因为dotCloud公司效益不景气, 如果不是大家齐心协力想要活下去, 可能docker会更晚和我们见面. 遇到问题, 没关系, 一定要想办法克服困难, 克服了的困难就不是困难了.

Docker是基于Go语言开发的！开源项目！

官网：https://www.docker.com/

![image-20230110222207274](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230110222207274.png)

文档地址：https://docs.docker.com/     Docker的文档是超级详细的

仓库地址：https://hub.docker.com/    



### 1.3Docker的作用

之前的技术

![image-20230110222604667](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230110222604667.png)



虚拟机技术的缺点：

1.占用的资源十分多

2.冗余步骤多

3.启动很慢



容器化技术：

容器化技术不是模拟的一个完整的操作系统

![image-20230110222821608](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230110222821608.png)

  

计较Docker和虚拟机技术的不同：

1.传统虚拟机，虚拟出一套硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件

2.容器内的应用直接运行在宿主机的内容，容器是没有自己的内核的，也没有虚拟我们的硬件，所以就轻便了

3.每个容器间是相互隔离，每个容器内都有一个自己的文件系统。

 

> DevOps(开发运维)

**应用更快的交付和部署**

传统：一堆帮助文档，安装程序

Docker：打包镜像发布测试，一键运行

**更快捷的升级和扩缩容**

使用了Docker之后，我们部署应用就和搭积木一样！

**更简单的系统运维**

在容器化之后，我们的开发，测试环境都是高度一致的。

**更高效的计算资源利用：**

Docker是内核级别的虚拟化，可以在一个物理机上运行很多的容器实例！服务器的性能可以被压榨到极致。



## 2.Docker安装

### 2.1Docker的基本组成

![image-20230111202933205](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230111202933205.png)

**镜像（image）**

docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，tomcat===>run===>tomcat01容器（提供服务器），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）

**容器（container）**

Docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建的。

启动，停止，删除，基本命令！

目前可以把这个容器理解为一个简易的linux系统

**仓库（repository）**

仓库就是存放镜像的地方！

仓库分为公有仓库和私有仓库！

Docker Hub （默认是国外的）



### 2.2Docker安装

> 环境准备

````shell
#系统内核是3.10以上的
[root@VM-8-10-centos /]# uname -r
3.10.0-1160.71.1.el7.x86_64
````

````shell
#系统版本
[root@VM-8-10-centos /]# cat /etc/os-release 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
````

> 安装

````shell
#1、卸载旧版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
#2、需要的安装包
yum install -y yum-utils
#3、设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo#默认是国外的
    
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo    #配置阿里云镜像

  
#更新yum软件包索引
yum makecache fast
    
#4、安装docker相关的   docker-ce社区   ee企业版
yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin

#5、启动docker
systemctl start docker
#6、使用docker version查看是否安装成功
````

![image-20230111225637286](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230111225637286.png)

````shell
#7、hello-world
docker run hello-world
````

![image-20230111225834294](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230111225834294.png)

````shell
#8、查看镜像是否存在
docker images
````

![image-20230111225939727](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230111225939727.png)

了解：卸载docker

````shell
#1、卸载依赖
yum remove docker-ce docker-ce-cli containerd.io docker-compose-plugin

#2、删除资源
rm -rf /var/lib/docker     #docker默认工作目录
rm -rf /var/lib/containerd
````





## 3.将本地镜像推送到腾讯云

### 3.1.登录腾讯云容器镜像服务 Docker Registry

````shell
docker login ccr.ccs.tencentyun.com --username=100027209973
密码：******
````

![image-20230201221310051](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230201221310051.png)



### 3.2.向 Registry 中推送镜像

````shell
docker tag de1492807016 ccr.ccs.tencentyun.com/tsdocker/myubuntu1.3:1.3
````

![image-20230201221435717](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230201221435717.png)

````shell
 docker push ccr.ccs.tencentyun.com/tsdocker/myubuntu1.3:1.3
````

![image-20230201221615125](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230201221615125.png)



### 3.3.测试是否推送成功，从 Registry 拉取镜像

````shell
docker pull ccr.ccs.tencentyun.com/tsdocker/myubuntu1.3:1.3
````

![image-20230201222700118](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230201222700118.png)





## 4.搭建私库

### 4.1.下载镜像Docker Registry

````shell
docker pull registry
````

### 4.2.运行私有库Registry，相当于本地有个私有Docker hub

````shell
docker run -d -p 5000:5000 -v /zzyyuse/myregistry/:/tmp/registry --privileged=true registry
````

### 4.3.案例演示创建一个新镜像，ubuntu安装ifconfig命令

````shell
[root@VM-8-10-centos ~]# docker run -it ubuntu /bin/bash
root@5d28db52c965:/# apt-get update
root@5d28db52c965:/# apt-get install net-tools
root@5d28db52c965:/# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.5  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:05  txqueuelen 0  (Ethernet)
        RX packets 12473  bytes 26400264 (26.4 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 12477  bytes 1156667 (1.1 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
 
[root@VM-8-10-centos ~]# docker commit -m "ifconfig cmd add" -a="ts" 2f1008c97f2b tsubuntu:1.2

````



### 4.4.curl验证私服库上有什么镜像

````shell
[root@VM-8-10-centos ~]# curl -XGET http://43.139.173.211:5000/v2/_catalog
{"repositories":[]}

````

### 4.5.将新镜像zzyyubuntu:1.2修改符合私服规范的Tag

````shell
[root@VM-8-10-centos ~]# docker tag tsubuntu:1.2 43.139.173.211:5000/tsubuntu:1.2
````

### 4.6.修改配置文件使之支持http

由于docker默认是不允许http方式推送镜像，通过配置选项来取消这个限制

````shell
vim /etc/docker/daemon.json

{
        "registry-mirrors": ["https://aa25jngu.mirror.aliyuncs.com"],
        "insecure-registries":["43.139.173.211:5000"]

}


````

重新启动docker配置生效

````shell
[root@VM-8-10-centos docker]# systemctl restart docker
````

### 4.7.push推送到私服库

````shell
[root@VM-8-10-centos docker]# docker push 43.139.173.211:5000/tsubuntu:1.2
The push refers to repository [43.139.173.211:5000/tsubuntu]
9e587908c6ac: Pushed 
6515074984c6: Pushed 
1.2: digest: sha256:2ab92da69d916b11fdc82a5a6f71e8e3ac5104c3db2c223cc02fa350970dd16d size: 741
````

### 4.8.curl验证私服库上有什么镜像

````shell
[root@VM-8-10-centos docker]# curl -XGET http://43.139.173.211:5000/v2/_catalog
{"repositories":["tsubuntu"]}
````

### 4.9.pull到本地并运行

````shell
[root@VM-8-10-centos docker]# docker pull 43.139.173.211:5000/tsubuntu:1.2
1.2: Pulling from tsubuntu
6e3729cf69e0: Already exists 
ea21e510a4da: Already exists 
Digest: sha256:2ab92da69d916b11fdc82a5a6f71e8e3ac5104c3db2c223cc02fa350970dd16d
Status: Downloaded newer image for 43.139.173.211:5000/tsubuntu:1.2
43.139.173.211:5000/tsubuntu:1.2
[root@VM-8-10-centos docker]# docker images
REPOSITORY                                    TAG       IMAGE ID       CREATED          SIZE
43.139.173.211:5000/tsubuntu                  1.2       f01998a5c474   38 minutes ago   120MB
ccr.ccs.tencentyun.com/tsdocker/myubuntu1.3   1.3       de1492807016   8 days ago       178MB
tomcat                                        9.0       aa0e5599989e   2 weeks ago      477MB
tomcat                                        latest    ad4994520144   2 weeks ago      475MB
nginx                                         latest    a99a39d070bf   3 weeks ago      142MB
ubuntu                                        latest    6b7dfa7e8fdb   7 weeks ago      77.8MB
mysql                                         5.7       d410f4167eea   8 weeks ago      495MB
mysql                                         8.0       7484689f290f   8 weeks ago      538MB
registry                                      latest    81c944c2288b   2 months ago     24.1MB
centos                                        centos7   eeb6ee3f44bd   16 months ago    204MB
centos                                        latest    5d0da3dc9764   16 months ago    231MB
[root@VM-8-10-centos docker]# docker run -it f01998a5c474 /bin/bash
root@359679b16e98:/# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.3  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:03  txqueuelen 0  (Ethernet)
        RX packets 6  bytes 516 (516.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

root@359679b16e98:/# 
````

心中无女人，编码自然神

忘掉心上人，抬手灭红尘

人间清醒，挣钱第一

好好学习，天天向上



## 5.Docker容器数据卷

卷就是目录或文件，存在于一个或者多个容器中，由docker挂载到容器，但不属于联合文件系统，因此能够绕过Union File System提供一些用于持续存储或共享数据的特性；

卷的设计的目的就是数据的持久化，完全独立于容器的生存周期，因此Docker不会在容器删除时删除其挂载的数据卷。

### 5.1.运行带有容器卷功能的容器实例

````shell
docker run -d -p 5000:5000 -v /zzyyuse/myregistry/:/tmp/registry --privileged=true registry

#docker run -it --privileged=true -v /宿主机绝对路径目录：/容器内目录  镜像名
````

将运用与运行的环境打包镜像，run后形成容器实例运行 ，但是我们对数据的要求希望是持久化的Docker容器产生的数据，如果不备份，那么当容器实例删除后，容器内的数据自然也就没有了。为了能保存数据在docker中我们使用卷。
特点:
1: 数据卷可在容器之间共享或重用数据
2: 卷中的更改可以直接实时生效，爽
3: 数据卷中的更改不会包含在镜像的更新中
4:数据卷的生命周期一直持续到没有容器使用它为止



### 5.2.查看挂载信息

````shell
[root@VM-8-10-centos ~]# docker inspect b3686f5d5b8a

 "Mounts": [
            {
                "Type": "bind",
                "Source": "/tmp/host_data",
                "Destination": "/tmp/docker_data",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
           
````



### 5.3.设置容器内只读权限

````shell
#docker run -it --privileged=true -v /宿主机绝对路径目录：/容器内目录:ro  镜像名

#ro   为read only


#此时容器内不能在写东西了
root@22559583f7a6:/tmp/abc# touch a.txt 
touch: cannot touch 'a.txt': Read-only file system
````



### 5.4.容器卷的继承

````shell
#docker run -it --privileged=true --volumes-from 父类 --name u2 ubunu
````



## 6.Docker常规安装简介

### 6.1总体步骤

- **搜索镜像**
- **拉取镜像**
- **查看镜像**
- **启动镜像**
- **停止容器**
- **移除容器**



### 6.2案例：安装tomcat

docker hub上面查找tomcat镜像

![image-20230203203821766](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20230203203821766.png)

从docker hub上拉取tomcat镜像到本地

````shell
#docker pull tomcat:版本号    不加版本号默认为最新的
````

docker images查看是否有拉取到的tomcat

````shell
#docker images
````

使用tomcat镜像创建容器实例(也叫运行镜像)

````shell
#docker run -d -p 8080:8080 --name=tomcatTs tomcat 
````

**访问猫首页**

````shell
#docker exec -it 418c3cd8d997 /bin/bash
````

目前访问不了，404.因为下载之后的webapps目录下没有内容，需要将其删除掉，将webapps.dist重命名为webapps



### 6.3案例：安装mysql

#### 6.3.1简单版

会出现中文乱码，数据丢失问题

````shell
#1.docker pull mysql:5.7
#2.docker images
#3.docker run -p3306:3306 --name tsMysql -e MYSQL_ROOT_PASSWORD=2920749aaqq,.A -d mysql:5.7
#4.docker ps
#5. docker exec -it tsMysql /bin/bash
````

#### 6.3.2实战版

````shell
#1.docker run -d -p3306:3306 --privileged=true -v /tsuse/mysql/log:/var/log/mysql -v /tsuse/mysql/data:/var/lib/mysql -v /tsuse/mysql/conf:/etc/mysql/conf.d --name tsMysql -e MYSQL_ROOT_PASSWORD=2920749aaqq,.A -d mysql:5.7
#2.cd /tsuse/mysql/conf/
#3.vim my.cnf
#4.将下面粘进去
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
[mysqld]
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve
#5.进入容器内的mysql
show VARIABLES like 'character%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec)
#6.此时在进行新建表，插入中文数据则可成功了。

````



### 6.4安装redis

````shell
#1.docker pull redis:6.0.8
#2.mkdir -p /app/redis
#3.拷贝一个原始的redis.conf文件并将其bind127.0.0.1注释掉
#4.docker run  -p6379:6379 --privileged=true -v /app/redis/redis.conf:/etc/redis/redis.conf -v /app/soft/redis/data:/data -d --name redis redis:6.0.8 redis-server /etc/redis/redis.conf
#5.docker ps
#6. docker exec -it redis /bin/bash  
#7.redis-cli
#8.ping   回应pong证明成功
````



### 6.5安装nginx















## 7.mysql主从搭建

1.dock





## 8.DockerFile

### 8.1DockerFile是什么？

DockerFile是用来构建Docker镜像的文本文件，是由一条条构建镜像所需的指令和参数构成的脚本。

### 8.2DockerFile内容基础知识

1. **每条保留字指令都必须为大写字母且后面要跟随至少一个参数**
2. **指令按照从上到下，顺序执行**
3. **#表示注释**
4. **每条指令都会创建一个新的镜像层并对镜像进行提交**

### 8.3Docker执行DockerFile的大致流程

1. **docker从基础镜像运行一个容器**
2. **执行一条指令并对容器作出修改**
3. **执行类似docker commit的操作提交一个新的镜像层**
4. **docker再基于刚提交的镜像运行一个新容器**
5. **执行dockerfile中的下一条指令直到所有指令都执行完成**

### 8.4DockerFile常用保留字

- FROM
- MAINTANINER
- RUN
- EXPOSE
- WORKDIR
- ENV
- ADD
- COPY
- VOLUME
- CMD
- ENTRYPOINT
- ONBUILD



#### 8.4.1FROM

基础镜像，即当前新镜像是基于哪个镜像创建的。

```shell
#基于openjdk:8 创建镜像
FROM openjdk:8
```

#### 8.4.2MAINTAINER

镜像维护者的姓名和邮箱地址

```shell
MAINTAINER 田爽<tian970623@163.com>
```

#### 8.4.3RUN

容器构建时需要运行的指令

```shell
RUN mkdir -p /conf/my.cnf
```

#### 8.4.4EXPOSE

当前容器对外暴露的端口

```shell
#暴露出MyCat的所需端口
EXPOSE 8066 9066
```

#### 8.4.5WORKDIR

指定在创建容器后，终端默认登录的进来工作目录

```shell
#容器数据卷，用于数据保存和持久化工作
WORKDIR /usr/local/mycat
```

#### 8.4.6ENV

用来在构建镜像过程中设置环境变量

```shell
#用来在构建镜像过程中设置环境变量
ENV MYCAT_HOME=/usr/local/mycat
```

这个环境变量可以在后续的任何RUN指令中使用，这就如同在命令前面指定了环境变量前缀一样;也可以在其它指令中直接使用这些环境变量。

如：

```shell
RUN $MYCAT_HOME/mycat
```

#### 8.4.7ADD 和 COPY

ADD：

将宿主机目录下的文件拷贝进镜像，并且ADD命令会自动处理URL和解压tar压缩包

```shell
ADD centos-6-docker.tar.xz / 
```


COPY：

类似ADD,拷贝文件和目录到镜像中。

将从构建上下文目录中<源路径>的文件/目录复制到新的一层的镜像内的<目标路径>位置

```shell
COPY src destCOPY ["src" "dest"]
```

#### 8.4.8VOLUME

容器数据卷，用于数据持久化和数据保存。

```shell
#将mycat的配置文件的地址暴露出映射地址,启动时直接映射宿主机的文件夹VOLUME /usr/local/mycat
```

#### 8.4.9CMD 和 ENTRYPOINT

CMD 

CMD的指令和RUN相似，也是两种格式：

- shell格式：CMD<命令>
- exec格式：CMD ["可执行文件“，”参数1“，”参数2“…]



Dockerfile中可以有多个CMD指令，但只有最后一个生效，CMD会被docker run之后的参数替换。



ENTRYPOINT

指定一个容器启动时要运行的命令。

ENTRYPOINT的目的和CMD一样，都是在指定容器启动程序及参数。

区别：

在这里先简单说明一下区别，你可以将CMD理解为覆盖

```shell
CMD cat /conf/my.cnf CMD /bin/bash
```

这两条指令都写在Dockerfile文件中，只会执行CMD /bin/bash ，而不会执行CMD cat /conf/my.cnf,因为CMD /bin/bash把上一条直接覆盖掉了。

而ENTRYPOINT则不同，你可以将ENTRYPOINT简单理解为追加。

主要体现在docker run 上，如果使用dockerfile文件中最后是CMD结尾，则在运行时不能够额外追加命令，否则会覆盖掉Dockerfile中的CMD命令。

而Dockerfile文件中最后一行为ENTRYPOINT结尾时，你可以在docker run命令后追加一些命令.

#### 8.4.10、ONBUILD

当构建一个被继承的Dockerfile时运行命令，父镜像在被子继承后，父镜像的onbuild被触发。
