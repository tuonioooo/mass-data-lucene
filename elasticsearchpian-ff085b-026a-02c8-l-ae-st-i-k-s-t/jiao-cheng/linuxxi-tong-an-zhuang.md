# linux系统安装es

* ### 软件下载地址

```
# elasticsearch 官网下载地址：
https://www.elastic.co/cn/downloads/elasticsearch

# elasticsearch-head 官网下载地址：
https://github.com/mobz/elasticsearch-head

# kibana 官网下载地址：
https://www.elastic.co/downloads/kibana
```

* ### elasticsearch安装

因为es安装运行，对root用户运行有问题，所以需要新建立一个用户，来操作es，步骤如下：

```
[root@localhost ~]# useradd -u 544 -d /home/elk -g users -m elk
```

下载es

```
[root@localhost elk]# wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.5.2.tar.gz
```

解压

```
[root@localhost elk]# tar -zxvf elasticsearch-6.5.2.tar.gz
```

给elk用户，授予/home/elk 主目录的执行权限

```
[root@localhost elk]# chown elk:users /home/elk/elasticsearch-6.5.2 -R
```

切换elk用户，运行es

```
[root@localhost elk]# su elk
[elk@localhost root]$ cd /home/elk/elasticsearch-6.5.2/bin/
[elk@localhost bin]$ ./elasticsearch &   #后台运行
```

> 以上就是一个简单的es安装过程，非常简单

* ### elasticsearch-head安装

Elasticsearch Head是集群管理、数据可视化、增删改查、查询语句可视化工具，它的安装方式有两种，一种是使用命令安装，一种是下载包安装

1.命令安装

```
./bin/plugin -install mobz/elasticsearch-head(*)

提示错误，错误信息是：ERROR: unknown command [-install]. Use [-h] option to list available commands，这是因为Elasticsearch在2.0以上的版本将-install变成了install。

故而执行命令 ./bin/plugin install mobz/elasticsearch-head即可。

详细信息请看：https://github.com/mobz/elasticsearch-head下面的README.md文件。

```



