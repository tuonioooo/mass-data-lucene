# 索引介绍

向Solr索引添加内容，并在必要时修改该内容或将其删除。通过向索引添加内容，我们可以通过Solr进行搜索。

Solr索引可以接受来自许多不同来源的数据，包括XML文件，逗号分隔值（CSV）文件，从数据库中的表中提取的数据以及常用文件格式（如Microsoft Word或PDF）中的文件。

以下是将数据加载到Solr索引的三种最常用方法：

* 使用基于Apache Tika构建的[Solr Cell](https://lucene.apache.org/solr/guide/7_4/uploading-data-with-solr-cell-using-apache-tika.html#uploading-data-with-solr-cell-using-apache-tika)框架来提取二进制文件或结构化文件，如Office，Word，PDF和其他专有格式。

* 通过从可以生成此类请求的任何环境向Solr服务器发送HTTP请求来上载XML文件。

* 编写自定义Java应用程序以通过Solr的Java客户端API（在[客户端API](https://lucene.apache.org/solr/guide/7_4/client-apis.html#client-apis)中更详细地描述）来提取数据。如果您正在使用提供Java API的应用程序（如内容管理系统（CMS）），则使用Java API可能是最佳选择。

无论用于摄取数据的方法如何，都有一个通用的基本数据结构，用于将数据输入Solr索引：包含多个_字段_的_文档，_每个_字段_都有一个_名称_并包含_内容，_这些_字段_可能为空。其中一个字段通常被指定为唯一的ID字段（类似于数据库中的主键），尽管Solr并不严格要求使用唯一的ID字段。

如果在与索引关联的架构中定义字段名称，则在对内容进行标记化时，与该字段关联的分析步骤将应用于其内容。如果存在与字段名称匹配的字段，那么未在架构中显式定义的字段将被忽略或映射到动态字段定义（请参阅[文档，字段和架构设计](https://lucene.apache.org/solr/guide/7_4/documents-fields-and-schema-design.html#documents-fields-and-schema-design)）。

* [Solr示例目录](#)

* [传输文件的curl实用程序](https://lucene.apache.org/solr/guide/7_4/introduction-to-solr-indexing.html#the-curl-utility-for-transferring-files)



