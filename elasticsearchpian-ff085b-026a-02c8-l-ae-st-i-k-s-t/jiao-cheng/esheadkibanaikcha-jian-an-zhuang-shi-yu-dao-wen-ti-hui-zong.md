# es、head、kibana、ik插件安装时遇到问题汇总

1.java.nio.file.AccessDeniedException: /data/wwwroot/elasticsearch-6.2.4/config/jvm.options

原因：当前用户没有执行权限

解决方法： chown linux用户名 elasticsearch安装目录 -R

例如：

chown ealsticsearch /data/wwwroot/elasticsearch-6.5.1 -R

或

chown elk:users /home/elk/elasticsearch-6.5.1 -R

说明：命令中ealsticsearch是用户，users代表组，两个命令是分表1.授予用户权限2.授予用户权限以及用户组权限

PS：其他Java软件报.AccessDeniedException错误也可以同样方式解决，给 执行用户相应的目录权限即可

添加用户命令：

[https://www.cnblogs.com/irisrain/p/4324593.html](https://www.cnblogs.com/irisrain/p/4324593.html)

2.启动elasticsearch时，报错

```
ERROR: [3] bootstrap checks failed
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
[2]: max number of threads [3820] for user [elk] is too low, increase to at least [4096]
[3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```

解决方式：

\[1\]\[2\]的解决方案如下：

```
切换到root账户后，修改/etc/security/limits.conf文件
添加
* soft nofile 65536
* hard nofile 131072
* soft nproc 2048
* hard nproc 4096
```

\[3\]的解决方案如下：

```
elasticsearch启动时遇到的错误问题翻译过来就是：elasticsearch用户拥有的内存权限太小，至少需要262144；

切换到root用户
执行命令：
sysctl -w vm.max_map_count=262144
查看结果：
sysctl -a|grep vm.max_map_count
显示：
vm.max_map_count = 262144

上述方法修改之后，如果重启虚拟机将失效，所以：
解决办法：
在/etc/sysctl.conf文件最后添加一行
vm.max_map_count=262144
即可永久修改
```

3.启动Elasticsearch时使用新建的elk用户，启动时报错：max number of threads \[3895\] for user \[elk\] is too low, increase to at least \[4096\]；查资料后，查看服务器当前用户的最大线程数为3895，修改配置文件/etc/security/limits.d/20-nproc.conf（Centos7）中的nproc为4096后，切换到elk用户查看当前最大线程数还是为3895，请问这个怎么修改啊？Elasticsearch启动要求最大线程数至少为4096.

![](/assets/import-es-002.png)

解决方式：

_**重启服务器，就永久生效了**_

