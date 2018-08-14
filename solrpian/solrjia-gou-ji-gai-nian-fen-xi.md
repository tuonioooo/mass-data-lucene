# Solr架构及概念分析

## 概念

Solr是一个基于Apache Lucene构建的搜索服务器，Apache Lucene是一个基于Java的开源信息检索库。

Solr是一个独立的企业级搜索应用服务器，它对外提供类似于Web-service的API接口。用户可以通过http请求，向搜索引擎服务器提交一定格式的XML文件，生成索引；也可以通过Http Get操作提出查找请求，并得到XML格式的返回结果。

