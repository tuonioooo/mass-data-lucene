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

https://www.cnblogs.com/irisrain/p/4324593.html

