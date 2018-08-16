# Solr独立模式

### 手动建立核心参考：

[https://www.cnblogs.com/liushuan/p/8892717.html](https://www.cnblogs.com/liushuan/p/8892717.html)

遇到问题：

> Error CREATEing SolrCore 'core0': Couldn't persist core properties to /var/solr/data/core0/core.properties : /var/solr/data/core0/core.properties

有可能是该文件没有权限进行保存，解决方式：

**chmod 777 core0**

### 命令建立核心：



