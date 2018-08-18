# Solr独立模式

### 手动建立核心参考：

[https://www.cnblogs.com/liushuan/p/8892717.html](https://www.cnblogs.com/liushuan/p/8892717.html)

[https://blog.csdn.net/u010066934/article/details/51489055](https://blog.csdn.net/u010066934/article/details/51489055) 

https://www.cnblogs.com/youqc/p/9075569.html

遇到问题：

> Error CREATEing SolrCore 'core0': Couldn't persist core properties to /var/solr/data/core0/core.properties : /var/solr/data/core0/core.properties

有可能是该文件没有权限进行保存，解决方式：

**chmod 777 core0**

### 命令建立核心：

```
[solr@god solr]$ bin/solr create -c core1
WARNING: Using _default configset with data driven schema functionality. NOT RECOMMENDED for production use.
         To turn off: bin/solr config -c core1 -p 8983 -action set-user-property -property update.autoCreateFields -value false
INFO  - 2018-08-15 04:19:32.617; org.apache.solr.util.configuration.SSLCredentialProviderFactory; Processing SSL Credential Provider chain: env;sysprop

Created new core 'core1'
```

创建命令

> bin/solr create -c core1

注意：

> 用命令创建时切记是用solr用户，这个用户在安装solr时会默认安装

用root用户创建solr核心，出错场景如下：

```
[root@god solr]# bin/solr create -c core1
WARNING: Using _default configset with data driven schema functionality. NOT RECOMMENDED for production use.
         To turn off: bin/solr config -c core1 -p 8983 -action set-user-property -property update.autoCreateFields -value false
WARNING: Creating cores as the root user can cause Solr to fail and is not advisable. Exiting.
         If you started Solr as root (not advisable either), force core creation by adding argument -force
```

> 上面场景是用root用户创建核心，出现的警告，根据警告提示，需要用-force

```
[root@god solr]# bin/solr create -c core1 -force
WARNING: Using _default configset with data driven schema functionality. NOT RECOMMENDED for production use.
         To turn off: bin/solr config -c core1 -p 8983 -action set-user-property -property update.autoCreateFields -value false
INFO  - 2018-08-15 04:15:50.100; org.apache.solr.util.configuration.SSLCredentialProviderFactory; Processing SSL Credential Provider chain: env;sysprop

ERROR: Error CREATEing SolrCore 'core1': Couldn't persist core properties to /var/solr/data/core1/core.properties : /var/solr/data/core1/core.properties
```

> 此时，虽然可以运行命令，但是创建核心会失败，没有权限这一类的，总而言之，当我们用命令行操作时，要 su solr 用户在操作，否则会出现不可预之的错误



