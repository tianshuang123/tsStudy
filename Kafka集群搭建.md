# Kafka集群搭建

## 1.下载Kafka

下载地址：

https://kafka.apache.org/downloads

![image-20240220221003224](http://117.72.43.140:9000/weblog/image-20240220221003224.png)

这里以Kafka2.8.1为例进行下载，前面的2.12、2.13代表开发kafka的scala语言的版本，后面的2.8.1是kafka应用的版本。Scala是一种运行于JVM虚拟机之上的语言。在运行时，只需要安装JDK就可以了，选哪个Scala版本没有区别。但是如果要调试源码，就必须选择对应的Scala版本。因为Scala语言的版本并不是向后兼容的。

## 2.上传至服务器

运用自己常用的连接服务器的工具将Kafka上传到服务器即可。这里采用三台机器的集群。

我这里上传目录为/app

![image-20240220221515028](http://117.72.43.140:9000/weblog/image-20240220221515028.png)

## 3.解压



```shell
tar -xvf kafka_2.13-2.8.1.tgz
```

![image-20240220221634373](http://117.72.43.140:9000/weblog/image-20240220221634373.png)

## 4.查看Kafka目录文件

![image-20240220221930087](http://117.72.43.140:9000/weblog/image-20240220221930087.png)

## 5.修改zookeeper配置文件

从Apache Kafka 2.8.0版本开始，Kafka开始提供内置的ZooKeeper服务。在此之前，Kafka依赖外部的ZooKeeper集群来提供协调服务。内置的ZooKeeper服务使得Kafka的部署更加简单，因为不再需要单独安装和管理ZooKeeper集群。

进入/app/kafka_2.13-2.8.1/config

```shell
cd /app/kafka_2.13-2.8.1/config
vim zookeeper.properties
```

![image-20240220225318794](http://117.72.43.140:9000/weblog/image-20240220225318794.png)

三台服务器分别为图所示进行修改

> 其中，clientPort 2181是对客户端开放的服务端口。
>
> 集群配置部分， server.x这个x就是节点在集群中的myid。后面的2888端口是集群内部数据传输使用的端口。3888是集群内部进行选举使用的端口。

## 6.新建myid文件

在每台服务器的dataDir目录下创建myid文件，分别为1、2、3，与上边的集群节点信息进行对应

```
echo 1 > /app/zookeeper/data/myid
```

![image-20240220230147010](http://117.72.43.140:9000/weblog/image-20240220230147010.png)

![image-20240220230210000](http://117.72.43.140:9000/weblog/image-20240220230210000.png)

![image-20240220230233046](http://117.72.43.140:9000/weblog/image-20240220230233046.png)

## 7.启动Zookeeper

```shell
cd /app/kafka_2.13-2.8.1/
nohup bin/kafka-server-start.sh config/server.properties &
```

启动之后jps查看是否有QuorumPeerMain进程

![image-20240220225509788](http://117.72.43.140:9000/weblog/image-20240220225509788.png)          

## 8.修改Kafka配置文件

```shell
cd /app/kafka_2.13-2.8.1/config
vim zookeeper.properties
```

修改如下几处：

```properties
#每台服务器修改为不同的即可  可分别为1、2、3，只能为数字
broker.id=1   
#数据文件地址。
log.dirs=/app/kafka/logs
#默认的每个Topic的分区数
num.partitions=4
#zookeeper的服务地址
zookeeper.connect=192.168.228.129:2181,192.168.52.128:2181,192.168.228.130:2181
#可以选择指定zookeeper上的基础节点。
#zookeeper.connect=192.168.228.129:2181,192.168.52.128:2181,192.168.228.130:2181/kafka
```

> broker.id需要每个服务器上不一样，分发到其他服务器上时，要注意修改一下。
>
> 多个Kafka服务注册到同一个zookeeper集群上的节点，会自动组成集群。
>
> 配置文件中的注释非常细致，可以关注一下。下面是server.properties文件中比较重要的核心配置
>
> | **Property**               | **Default**                | **Description**                                              |
> | :------------------------- | :------------------------- | :----------------------------------------------------------- |
> | broker.id                  | 0                          | broker的“名字”，你可以选择任意你喜欢的数字作为id，只要id是唯每个broker都可以用一个唯一的非负整数id进行标识；这个id可以作为一的即可。 |
> | log.dirs                   | /tmp/kafka-logs            | kafka存放数据的路径。这个路径并不是唯一的，可以是多个，路径之间只需要使用逗号分隔即可；每当创建新partition时，都会选择在包含最少partitions的路径下进行。 |
> | listeners                  | PLAINTEXT://127.0.0.1:9092 | server接受客户端连接的端口，ip配置kafka本机ip即可            |
> | zookeeper.connect          | localhost:2181             | zookeeper连接地址。hostname:port。如果是Zookeeper集群，用逗号连接。 |
> | log.retention.hours        | 168                        | 每个日志文件删除之前保存的时间。                             |
> | num.partitions             | 1                          | 创建topic的默认分区数                                        |
> | default.replication.factor | 1                          | 自动创建topic的默认副本数量                                  |
> | min.insync.replicas        | 1                          | 当producer设置acks为-1时，min.insync.replicas指定replicas的最小数目（必须确认每一个repica的写数据都是成功的），如果这个数目没有达到，producer发送消息会产生异常 |
> | delete.topic.enable        | false                      | 是否允许删除主题                                             |

## 9.启动Kafka

```sh
cd /app/kafka_2.13-2.8.1/
bin/kafka-server-start.sh -daemon config/server.properties
```

> -daemon表示后台启动kafka服务，这样就不会占用当前命令窗口。

​	通过jps指令可以查看Kafka的进程。

![image-20240220230742469](http://117.72.43.140:9000/weblog/image-20240220230742469.png)



至此集群搭建完成。

