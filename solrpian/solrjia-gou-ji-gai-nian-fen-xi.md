# Solr架构及概念分析

## 概念

Solr是一个基于Apache Lucene构建的搜索服务器，Apache Lucene是一个基于Java的开源信息检索库。

Solr是一个独立的企业级搜索应用服务器，它对外提供类似于Web-service的API接口。用户可以通过http请求，向搜索引擎服务器提交一定格式的XML文件，生成索引；也可以通过Http Get操作提出查找请求，并得到XML格式的返回结果。

## 特性

Solr是一个高性能，采用Java5开发，基于Lucene的全文搜索服务器。同时对其进行了扩展，提供了比Lucene更为丰富的查询语言，同时实现了可配置、可扩展并对查询性能进行了优化，并且提供了一个完善的功能管理界面，是一款非常优秀的全文搜索引擎。

![](/assets/import-solr-01.png)



![](/assets/import-solr-02.png)

