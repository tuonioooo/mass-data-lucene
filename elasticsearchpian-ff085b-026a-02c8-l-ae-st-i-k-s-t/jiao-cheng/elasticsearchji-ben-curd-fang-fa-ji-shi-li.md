# elasticsearch基本CURD方法及示例

一.索引添加、删除、修改

* 添加语法

```
PUT /my_index
{
    "settings": { ... any settings ... },
    "mappings": {
        "type_one": { ... any mappings ... },
        "type_two": { ... any mappings ... },
        ...
    }
}
```

* 创建索引的示例

```
PUT /my_index
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  },
  "mappings": {
    "my_type": {
      "properties": {
        "my_field": {
          "type": "text"
        }
      }
    }
  }
}

或

curl -XPUT '192.168.127.98:9200/my_index?pretty' -d '{   "settings": {     "number_of_shards": 1,     "number_of_replicas": 0   },   "mappings": {     "my_type": {       "properties": {         "my_field": {           "type": "text"         }       }     }   } }'
```

* 查看所有索引

```
curl '192.168.127.98:9200/_cat/indices?v'
```

* 修改索引

```
PUT /my_index/_settings
{
    "number_of_replicas": 1
}

或

curl -XPUT '192.168.127.98:9200/my_index?pretty' -d '{"number_of_replicas": 1 }'
```

* 删除索引

```
DELETE /my_index
DELETE /index_one,index_two
DELETE /index_*
DELETE /_all


可以设置下面的属性，使DELETE /_all 失效，必须指定索引名称，才可以删除。
elasticsearch.yml
action.destructive_requires_name: true
```

二、文档的添加、删除、修改

* 添加文档语法

```
PUT /{index}/{type}/{id}
{
"field": "value",
...
}
```

* 添加文档

```
PUT /my_index/my_type/1
{
  "name": "John Doe"
}

或

curl -XPUT 'localhost:9200/my_index/my_type/1?pretty' -d '{"name": "John Doe"}'
```

* 获取所有文档

```
PUT /my_index/_search
```



