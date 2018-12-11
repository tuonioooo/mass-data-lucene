# elasticSearch复制索引并修改字段

elasticsearch是不支持动态的修改mapping字段的，但是我们如何实现修改某一个字段呢？

方法为：

1、先创建修改后的mapping字段，字段名字一致，类型不一致

2、将老索引中的数据复制到新的索引中。

elasticsearch语句：

创建索引后设置新的mapping字段

```
PUT my_index
{
  "mappings": {
    "my_type": {
      "properties": {
        "date": {
          "type":   "date",
          "format": "yyyy-MM-dd"
        }
      }
    }
  }
}
```

将老的索引中的数据复制到新的索引中：

```
POST _reindex
{
  "source": {
    "index": "metricbeat-*"（老的索引名）
  },
  "dest": {
    "index": "metricbeat"（新的索引名）
  }
}
```



