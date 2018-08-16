# Solr集群模式

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



