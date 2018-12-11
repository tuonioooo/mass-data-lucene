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

**1.命令安装（适合6.0版本之前，6.0版本之后不适合）**

```
./bin/plugin -install mobz/elasticsearch-head(*)

提示错误，错误信息是：ERROR: unknown command [-install]. Use [-h] option to list available commands，这是因为Elasticsearch在2.0以上的版本将-install变成了install。

故而执行命令 ./bin/plugin install mobz/elasticsearch-head即可。

详细信息请看：https://github.com/mobz/elasticsearch-head下面的README.md文件。
```

**2.下载安装（推荐）**

1\)、head插件源码在git上，先安装git

```
[root@localhost bin]# yum install git
```

2\)、获取源码elasticsearch-head

```
[root@localhost elk]# git clone https://github.com/mobz/elasticsearch-head
```

3\)、.head插件需要node.js的支持

```
[root@localhost elk]# wget https://nodejs.org/dist/v10.9.0/node-v10.9.0-linux-x64.tar.xz // 下载
[root@localhost elk]# tar xf node-v10.9.0-linux-x64.tar.xz // 解压
[root@localhost elk]# cd node-v10.9.0-linux-x64 // 进入解压目录
[root@localhost node-v10.9.0-linux-x64]# ./bin/node -v // 执行node命令 查看版本
v10.9.0

解压文件的 bin 目录底下包含了 node、npm 等命令，我们可以使用 ln 命令来设置软连接：
[root@localhost bin]# ln -s /home/elk/node-v10.9.0-linux-x64/bin/npm /usr/local/bin/
[root@localhost bin]# ln -s /home/elk/node-v10.9.0-linux-x64/bin/node /usr/local/bin/
```

4\)、使用命令node -v 或者npm -v验证是否安装成功

```
[elk@localhost elasticsearch-head]$ npm -v
6.2.0
[elk@localhost elasticsearch-head]$ node -v
v10.9.0
```

5\)、修改elasticsearch的配置文件，elasticsearch安装目录/config/elasticsearch.yml，同时开启es远程访问

```
network.host: 0.0.0.0
http.cors.enabled: true
http.cors.allow-origin: "*"
```

6\)、运行head

```
[elk@localhost elasticsearch-head]$ npm install  #时间较长
[elk@localhost elasticsearch-head]$ npm run start

> elasticsearch-head@0.0.0 start /home/elk/elasticsearch-head
> grunt server

(node:5116) ExperimentalWarning: The http2 module is an experimental API.
Running "connect:server" (connect) task
Waiting forever...
Started connect web server on http://localhost:9100
npm run start
```

8\)、用elk用户重新运行elasticsearch

```
[elk@localhost bin]$ ./elasticsearch
```

9\)、访问 [http://192.168.127.127:9100](http://192.168.127.127:9100) ，外部访问时，需要开通端口号：

```
[root@localhost elasticsearch-head]# firewall-cmd --zone=public --add-port=9100/tcp --permanent
success
[root@localhost elasticsearch-head]# firewall-cmd --reload
success
```

![](/assets/import-es-001.png)参考：

[https://github.com/mobz/elasticsearch-head](https://github.com/mobz/elasticsearch-head)

[https://www.2cto.com/kf/201802/723573.html](https://www.2cto.com/kf/201802/723573.html)

[https://blog.csdn.net/qq3401247010/article/details/78742524](https://blog.csdn.net/qq3401247010/article/details/78742524)

[https://jingyan.baidu.com/article/3065b3b64eb570becff8a4da.html](https://jingyan.baidu.com/article/3065b3b64eb570becff8a4da.html)

[https://blog.csdn.net/qq3401247010/article/details/78742524](https://blog.csdn.net/qq3401247010/article/details/78742524)

