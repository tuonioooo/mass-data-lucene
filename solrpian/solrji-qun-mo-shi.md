# Solr集群模式

> vi /etc/default/solr.in.sh 
>
> SOLR\_PID\_DIR="/var/solr"
>
> SOLR\_HOME="/var/solr/data"
>
> LOG4J\_PROPS="/var/solr/log4j2.xml"
>
> SOLR\_LOGS\_DIR="/var/solr/logs"
>
> SOLR\_PORT="8983"
>
> ZK\_HOST=god.cn  \#如果是集群模式，必须加这个属性，如果不启用集群，注释即可
>
> SOLR\_HOST=god.cn



