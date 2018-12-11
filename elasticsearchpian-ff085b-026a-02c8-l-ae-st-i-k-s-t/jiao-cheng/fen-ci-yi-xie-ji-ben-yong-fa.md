# 分词一些基本用法

* 查看settings配置

```
http://192.168.61.251:9200/res_/_settings #res_是索引，不加索引，会查询所有索引的settings
```

* 查看mapping配置

```
http://192.168.79.131:9200/res_/_mappings #res_是索引，不加索引，会查询所有索引的mappings
```

* 查询分词

```
_analyze


{
  "analyzer":"ik_max_word",
  "text":"北京"
}
```



