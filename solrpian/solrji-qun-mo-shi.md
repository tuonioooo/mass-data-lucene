# SolrCloud集群模式

### 修改环境变量

```
vi /etc/default/solr.in.sh
#加入以下环境变量：
SOLR_PID_DIR="/var/solr"
SOLR_HOME="/var/solr/data"
LOG4J_PROPS="/var/solr/log4j2.xml"
SOLR_LOGS_DIR="/var/solr/logs"
SOLR_PORT="8983"
ZK_HOST=god.cn  #如果是集群模式，必须加这个属性，如果不启用集群，注释即可
SOLR_HOST=god.cn
```

> 注意：以上脚本修改，仅是单个solr示例，集群模式下，需要逐一的添加

### 修改solr\_home中的zoo.cfg，我的是`/var/solr/data`

```
[root@god data]# cd /var/solr/data/
[root@god data]# vi zoo.cfg 
# the port at which the clients will connect
# clientPort=2181
# NOTE: Solr sets this based on zkRun / zkHost params
clientPort=2181
zkHost=god.cn #这个是zookeeper的主机名称 或者zkRun=god.cn
```

> 注意：以上脚本修改，仅是单个solr示例，集群模式下，需要逐一的添加



