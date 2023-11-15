# ElasticSearch常见错误

## 一 read_only_allow_delete" : "true"

当我们在向某个索引添加一条数据的时候，可能（极少情况）会碰到下面的报错：

```
{
  "error": {
    "root_cause": [
      {
        "type": "cluster_block_exception",
        "reason": "blocked by: [FORBIDDEN/12/index read-only / allow delete (api)];"
      }
    ],
    "type": "cluster_block_exception",
    "reason": "blocked by: [FORBIDDEN/12/index read-only / allow delete (api)];"
  },
  "status": 403
}
```

上述报错是说索引现在的状态是只读模式（read-only），如果查看该索引此时的状态：

```
GET z1/_settings
# 结果如下
{
  "z1" : {
    "settings" : {
      "index" : {
        "number_of_shards" : "5",
        "blocks" : {
          "read_only_allow_delete" : "true"
        },
        "provided_name" : "z1",
        "creation_date" : "1556204559161",
        "number_of_replicas" : "1",
        "uuid" : "3PEevS9xSm-r3tw54p0o9w",
        "version" : {
          "created" : "6050499"
        }
      }
    }
  }
}
```

可以看到`"read_only_allow_delete" : "true"`，说明此时无法插入数据，当然，我们也可以模拟出来这个错误：

```
PUT z1
{
  "mappings": {
    "doc": {
      "properties": {
        "title": {
          "type":"text"
        }
      }
    }
  },
  "settings": {
    "index.blocks.read_only_allow_delete": true
  }
}

PUT z1/doc/1
{
  "title": "es真难学"
}
```

现在我们如果执行插入数据，就会报开始的错误。那么怎么解决呢？

- 清理磁盘，使占用率低于85%。
- 手动调整该项，具体参考[官网](https://www.elastic.co/guide/en/elasticsearch/reference/current/disk-allocator.html)

这里介绍一种，我们将该字段重新设置为：

```
PUT z1/_settings
{
  "index.blocks.read_only_allow_delete": null
}
```

现在再查看该索引就正常了，也可以正常的插入数据和查询了。

## 二 illegal_argument_exception

有时候，在聚合中，我们会发现如下报错：

```
{
  "error": {
    "root_cause": [
      {
        "type": "illegal_argument_exception",
        "reason": "Fielddata is disabled on text fields by default. Set fielddata=true on [age] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead."
      }
    ],
    "type": "search_phase_execution_exception",
    "reason": "all shards failed",
    "phase": "query",
    "grouped": true,
    "failed_shards": [
      {
        "shard": 0,
        "index": "z2",
        "node": "NRwiP9PLRFCTJA7w3H9eqA",
        "reason": {
          "type": "illegal_argument_exception",
          "reason": "Fielddata is disabled on text fields by default. Set fielddata=true on [age] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead."
        }
      }
    ],
    "caused_by": {
      "type": "illegal_argument_exception",
      "reason": "Fielddata is disabled on text fields by default. Set fielddata=true on [age] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead.",
      "caused_by": {
        "type": "illegal_argument_exception",
        "reason": "Fielddata is disabled on text fields by default. Set fielddata=true on [age] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead."
      }
    }
  },
  "status": 400
}
```

这是怎么回事呢？是因为，聚合查询时，指定字段不能是`text`类型。比如下列示例：

```
PUT z2/doc/1
{
  "age":"18"
}
PUT z2/doc/2
{
  "age":20
}

GET z2/doc/_search
{
  "query": {
    "match_all": {}
  },
  "aggs": {
    "my_sum": {
      "sum": {
        "field": "age"
      }
    }
  }
}
```

当我们向`elasticsearch`中，添加一条数据时（此时，如果索引存在则直接新增或者更新文档，不存在则先创建索引），首先检查该`age`字段的映射类型。如上示例中，我们添加第一篇文档时（`z1`索引不存在），`elasticsearch`会自动的创建索引，然后为`age`字段创建映射关系（es就猜此时`age`字段的值是什么类型，如果发现是`text`类型，那么存储该字段的映射类型就是`text`），此时`age`字段的值是`text`类型，所以，第二条插入数据，`age`的值也是`text`类型，而不是我们看到的`long`类型。我们可以查看一下该索引的`mappings`信息：

```
GET z2/_mapping
# mapping信息如下
{
  "z2" : {
    "mappings" : {
      "doc" : {
        "properties" : {
          "age" : {
            "type" : "text",
            "fields" : {
              "keyword" : {
                "type" : "keyword",
                "ignore_above" : 256
              }
            }
          }
        }
      }
    }
  }
}
```

上述返回结果发现，`age`类型是`text`。而该类型又不支持聚合，所以，就会报错了。解决办法就是：

- 如果选择动态创建一篇文档，映射关系取决于你添加的第一条文档的各字段都对应什么类型。而不是我们看到的那样，第一次是`text`，第二次不加引号，就是`long`类型了不是这样的。
- 如果嫌弃上面的解决办法麻烦，那就选择手动创建映射关系。首先指定好各字段对应什么类型。后续才不至于出错。

## 三 Result window is too large

很多时候，我们在查询文档时，一次查询结果很可能会有很多，而elasticsearch一次返回多少条结果，由`size`参数决定：

```
GET e2/doc/_search
{
  "size": 100000,
  "query": {
    "match_all": {}
  }
}
```

而默认是最多范围一万条，那么当我们的请求超过一万条时（比如有十万条），就会报：

```
Result window is too large, from + size must be less than or equal to: [10000] but was [100000]. See the scroll api for a more efficient way to request large data sets. This limit can be set by changing the [index.max_result_window] index level setting.
```

意思是一次请求返回的结果太大，可以另行参考 `scroll API`或者设置`index.max_result_window`参数手动调整`size`的最大默认值：

```
# kibana中设置
PUT e2/_settings
{
  "index": {
    "max_result_window": "100000"
  }
}
# Python中设置
from elasticsearch import Elasticsearch
es = Elasticsearch()
es.indices.put_settings(index='e2', body={"index": {"max_result_window": 100000}})
```

如上例，我们手动调整索引`e2`的`size`参数最大默认值到十万，这时，一次查询结果只要不超过10万就都会一次返回。

注意，这个设置对于索引`es`的`size`参数是永久生效的。


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/20-elasticsearch%E4%B9%8B%E5%B8%B8%E8%A7%81%E9%94%99%E8%AF%AF/  

