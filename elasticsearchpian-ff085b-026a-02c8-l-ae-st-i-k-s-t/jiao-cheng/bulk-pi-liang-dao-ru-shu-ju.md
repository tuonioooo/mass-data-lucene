# \_bulk 批量导入数据

```
curl -X POST "localhost:9200/_bulk" -H 'Content-Type: application/json' -d'
{ "index" : { "_index" : "test", "_type" : "_doc", "_id" : "1" } }
{ "field1" : "value1" }
{ "delete" : { "_index" : "test", "_type" : "_doc", "_id" : "2" } }
{ "create" : { "_index" : "test", "_type" : "_doc", "_id" : "3" } }
{ "field1" : "value3" }
{ "update" : {"_id" : "1", "_type" : "_doc", "_index" : "test"} }
{ "doc" : {"field2" : "value2"} }
'


或


curl -X POST "localhost:9200/test/_doc/_bulk" -H 'Content-Type: application/json' -d'
{ "index" : {"_id" : "1" } }
{ "field1" : "value1" }
{ "create" : {"_id" : "2" } }
{ "field1" : "value2" }
'

参考：
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html


相关的完整实例如下：

参考：https://www.jianshu.com/p/1c8ba834e15c?utm_source=oschina-app

1.创建索引

curl -XPUT 'http://localhost:9200/iktest' -H 'Content-Type: application/json' -d '{
    "settings" : {
        "analysis" : {
            "analyzer" : {
                "ik" : {
                    "tokenizer" : "ik_max_word"
                }
            }
        },
        "number_of_shards": 5,
        "number_of_replicas": 0
    },
    "mappings" : {
        "article" : {
            "dynamic" : true,
            "properties" : {
                "subject" : {
                    "type" : "text",
                    "analyzer" : "ik_max_word"
                }
            }
        }
    }
}'


2.批量插入数据

curl -XPOST "http://localhost:9200/iktest/article/_bulk" -H 'Content-Type: application/json' -d'
{ "index" : { "_id" : "1" } }
{"subject" : "＂闺蜜＂崔顺实被韩检方传唤 韩总统府促彻查真相" }
{ "index" : { "_id" : "2" } }
{"subject" : "韩举行＂护国训练＂ 青瓦台:决不许国家安全出问题" }
{ "index" : { "_id" : "3" } }
{"subject" : "媒体称FBI已经取得搜查令 检视希拉里电邮" }
{ "index" : { "_id" : "4" } }
{"subject" : "村上春树获安徒生奖 演讲中谈及欧洲排外问题" }
{ "index" : { "_id" : "5" } }
{"subject" : "希拉里团队炮轰FBI 参院民主党领袖批其“违法”" }
{ "index" : { "_id" : "6" } }
{"subject" : "卢沟桥天天（一）" }
{ "index" : { "_id" : "7" } }
{"subject" : "卢沟桥石狮子（二）" }
{ "index" : { "_id" : "8" } }
{"subject" : "卢沟桥拱桥（三）" }
{ "index" : { "_id" : "9" } }
{"subject" : "卢本伟" }
{ "index" : { "_id" : "10" } }
{"subject" : "山沟" }
{ "index" : { "_id" : "11" } }
{"subject" : "天桥" }
'

注意：JSON结尾需要有一个换行

3. 查询 “希拉里和韩国”

curl -XPOST http://localhost:9200/iktest/article/_search -H 'Content-Type: application/json'  -d'
{
    "query" : { "match" : { "subject" : "希拉里和韩国" }},
    "highlight" : {
        "pre_tags" : ["<font color='red'>"],
        "post_tags" : ["</font>"],
        "fields" : {
            "subject" : {}
        }
    }
}'
```



