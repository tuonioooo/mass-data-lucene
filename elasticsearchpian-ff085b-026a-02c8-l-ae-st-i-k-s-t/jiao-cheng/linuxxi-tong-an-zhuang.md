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



