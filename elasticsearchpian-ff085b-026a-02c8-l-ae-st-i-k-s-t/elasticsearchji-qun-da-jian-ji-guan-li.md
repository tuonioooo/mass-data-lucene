# elasticsearch集群搭建及管理

# 一、ES中的基本概念 {#一es中的基本概念}

### cluster {#cluster}

> 代表一个集群，集群中有多个节点，其中有一个为主节点，这个主节点是可以通过选举产生的，主从节点是对于集群内部来说的。es的一个概念就是去中心化，字面上理解就是无中心节点，这是对于集群外部来说的，因为从外部来看es集群，在逻辑上是个整体，你与任何一个节点的通信和与整个es集群通信是等价的。

### shards {#shards}

> 代表索引分片，es可以把一个完整的索引分成多个分片，这样的好处是可以把一个大的索引拆分成多个，分布到不同的节点上。构成分布式搜索。分片的数量只能在索引创建前指定，并且索引创建后不能更改。

### replicas {#replicas}

> 代表索引副本，es可以设置多个索引的副本，副本的作用一是提高系统的容错性，当某个节点某个分片损坏或丢失时可以从副本中恢复。二是提高es的查询效率，es会自动对搜索请求进行负载均衡。

### recovery {#recovery}

> 代表数据恢复或叫数据重新分布，es在有节点加入或退出时会根据机器的负载对索引分片进行重新分配，挂掉的节点重新启动时也会进行数据恢复。

### river {#river}

> 代表es的一个数据源，也是其它存储方式（如：数据库）同步数据到es的一个方法。它是以插件方式存在的一个es服务，通过读取river中的数据并把它索引到es中，官方的river有couchDB的，RabbitMQ的，Twitter的，Wikipedia的。

### gateway {#gateway}

> 代表es索引快照的存储方式，es默认是先把索引存放到内存中，当内存满了时再持久化到本地硬盘。gateway对索引快照进行存储，当这个es集群关闭再重新启动时就会从gateway中读取索引备份数据。es支持多种类型的gateway，有本地文件系统（默认），分布式文件系统，Hadoop的HDFS和amazon的s3云存储服务。

### discovery.zen {#discovery.zen}

> 代表es的自动发现节点机制，es是一个基于p2p的系统，它先通过广播寻找存在的节点，再通过多播协议来进行节点之间的通信，同时也支持点对点的交互。

### Transport {#transport}

> 代表es内部节点或集群与客户端的交互方式，默认内部是使用tcp协议进行交互，同时它支持http协议（json格式）、thrift、servlet、memcached、zeroMQ等的传输协议（通过插件方式集成）。

## 二、部署环境

采用三台CentOS7.3部署Elasticsearch集群，部署Elasticsearch集群就不得不提索引分片，以下是索引分片的简单介绍。

| 系统 | 节点名 | IP |
| :--- | :--- | :--- |
| CentOS7.3 | els1 | 172.18.68.11 |
| CentOS7.3 | els2 | 172.18.68.12 |
| CentOS7.3 | els3 | 172.18.68.13 |

> ES集群中索引可能由多个分片构成，并且每个分片可以拥有多个副本。通过将一个单独的索引分为多个分片，我们可以处理不能在一个单一的服务器上面运行的大型索引，简单的说就是索引的大小过大，导致效率问题。不能运行的原因可能是内存也可能是存储。由于每个分片可以有多个副本，通过将副本分配到多个服务器，可以提高查询的负载能力。

# 三、部署Elasticsearch集群 {#三部署elasticsearch集群}

### 1.安装JDK {#安装jdk}

Elasticsearch是基于Java开发是一个Java程序，运行在Jvm中，所以第一步要安装JDK

```
yum install -y java-1.8.0-openjdk-devel
```

### 2.下载elasticsearch {#下载elasticsearch}

[https://artifacts.elastic.co/downloads/elasticsearch/](https://artifacts.elastic.co/downloads/elasticsearch/)是ELasticsearch的官方站点，如果需要下载最新的版本，进入官网下载即可。可以下载到本地电脑然后再导入CentOS中，也可以直接在CentOS中下载。

```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.0.1.rpm
```

### 3.配置目录 {#配置目录}

安装完毕后会生成很多文件，包括配置文件日志文件等等，下面几个是最主要的配置文件路径

```
/etc/elasticsearch/elasticsearch.yml                            # els的配置文件
/etc/elasticsearch/jvm.options                                  # JVM相关的配置，内存大小等等
/etc/elasticsearch/log4j2.properties                            # 日志系统定义
/var/lib/elasticsearch                                          # 数据的默认存放位置
```

### 4.创建用于存放数据与日志的目录 {#创建用于存放数据与日志的目录}

数据文件会随着系统的运行飞速增长，所以默认的日志文件与数据文件的路径不能满足我们的需求，那么手动创建日志与数据文件路径，可以使用NFS、可以使用Raid等等方便以后的管理与扩展

```
mkdir /els/{log,date}
chown -R elasticsearch.elasticsearch /els/*
```

### 5.集群配置 {#集群配置}

集群配置中最重要的两项是`node.name`与`network.host`，每个节点都必须不同。其中`node.name`是节点名称主要是在Elasticsearch自己的日志加以区分每一个节点信息。  
`discovery.zen.ping.unicast.hosts`是集群中的节点信息，可以使用IP地址、可以使用主机名\(必须可以解析\)。

```
vim /etc/elasticsearch
cluster.name: aubin-cluster                                 # 集群名称
node.name: els1                                             # 节点名称，仅仅是描述名称，用于在日志中区分

path.data: /var/lib/elasticsearch                           # 数据的默认存放路径
path.logs: /var/log/elasticsearch                           # 日志的默认存放路径

network.host: 192.168.0.1                                   # 当前节点的IP地址
http.port: 9200                                             # 对外提供服务的端口，9300为集群服务的端口

discovery.zen.ping.unicast.hosts: ["172.18.68.11", "172.18.68.12","172.18.68.13"]       
# 集群个节点IP地址，也可以使用els、els.shuaiguoxia.com等名称，需要各节点能够解析

discovery.zen.minimum_master_nodes: 2                       # 为了避免脑裂，集群节点数最少为 半数+1
```

### 6.JVM配置 {#jvm配置}

由于Elasticsearch是Java开发的，所以可以通过`/etc/elasticsearch/jvm.options`配置文件来设定JVM的相关设定。如果没有特殊需求按默认即可。  
不过其中还是有两项最重要的`-Xmx1g`与`-Xms1g`JVM的最大最小内存。如果太小会导致Elasticsearch刚刚启动就立刻停止。太大会拖慢系统本身

```
vim /etc/elasticsearch/jvm.options
-Xms1g                                                  # JVM最大、最小使用内存
-Xmx1g
```

### 7.启动Elasticsearch {#启动elasticsearch}

由于启动Elasticsearch会自动启动daemon-reload所以最后一项可以省略。

```
systemctl enable elasticsearch.service
systemctl start elasticsearch
systemctl daemon-reload                                 # 可以省略
```

### 8.测试 {#测试}

Elasticsearch直接听过了http接口，所以直接使用curl命令就可以查看到一些集群相关的信息。

> 可以使用curl命令来获取集群的相关的信息，
>
> * \_cat代表查看信息
> * nodes为查看节点信息，默认会显示为一行，所以就用刀了?preety让信息更有好的显示
> * ?preety让输出信息更友好的显示

```
curl -XGET 'http://172.18.68.11:9200/_cat/nodes?pretty'
172.18.68.12 18 68 0 0.07 0.06 0.05 mdi - els2
172.18.68.13 25 67 0 0.01 0.02 0.05 mdi * els3             #  *号表示为当前节点为主节点的意思
172.18.68.11  7 95 0 0.02 0.04 0.05 mdi - els1
```

如果你要想查看更多有关于集群信息、当前节点统计信息等等，可以使用一下命令来获取到所有可以查看的信息。

```
curl -XGET 'http://172.18.68.11:9200/_cat?pretty'
```

参考：

https://www.2cto.com/kf/201802/723573.html

https://elasticsearch.cn/article/465

http://www.cnblogs.com/ljhdo/p/4959412.html

