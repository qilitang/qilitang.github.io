# 文档操作-mappings的其他设置

## 一 前言

上一小节中，根据`dynamic`的状态不同，我们对字段有了更多可自定义的操作。现在再来补充一个参数，使自定义的属性更加的灵活。

## 二 index

首先来创建一个`mappings`：

```
PUT m4
{
  "mappings": {
    "doc": {
      "dynamic": false,
      "properties": {
        "name": {
          "type": "text",
          "index": true
        },
        "age": {
          "type": "long",
          "index": false
        }
      }
    }
  }
}
```

可以看到，我们在创建索引的时候，为每个属性添加一个`index`参数。那会有什么效果呢？

先来添加一篇文档：

```
PUT m4/doc/1
{
  "name": "小黑",
  "age": 18
}
```

再来查询看效果：

```
GET m4/doc/_search
{
  "query": {
    "match": {
      "name": "小黑"
    }
  }
}

GET m4/doc/_search
{
  "query": {
    "match": {
      "age": 18
    }
  }
}
```

以`name`查询没问题，但是，以`age`作为查询条件就有问题了：

```
{
  "error": {
    "root_cause": [
      {
        "type": "query_shard_exception",
        "reason": "failed to create query: {\n  \"match\" : {\n    \"age\" : {\n      \"query\" : 18,\n      \"operator\" : \"OR\",\n      \"prefix_length\" : 0,\n      \"max_expansions\" : 50,\n      \"fuzzy_transpositions\" : true,\n      \"lenient\" : false,\n      \"zero_terms_query\" : \"NONE\",\n      \"auto_generate_synonyms_phrase_query\" : true,\n      \"boost\" : 1.0\n    }\n  }\n}",
        "index_uuid": "GHBPeT5pRnSi3g6DkpIkow",
        "index": "m4"
      }
    ],
    "type": "search_phase_execution_exception",
    "reason": "all shards failed",
    "phase": "query",
    "grouped": true,
    "failed_shards": [
      {
        "shard": 0,
        "index": "m4",
        "node": "dhkqLLTsRemm7qEgRdpvTg",
        "reason": {
          "type": "query_shard_exception",
          "reason": "failed to create query: {\n  \"match\" : {\n    \"age\" : {\n      \"query\" : 18,\n      \"operator\" : \"OR\",\n      \"prefix_length\" : 0,\n      \"max_expansions\" : 50,\n      \"fuzzy_transpositions\" : true,\n      \"lenient\" : false,\n      \"zero_terms_query\" : \"NONE\",\n      \"auto_generate_synonyms_phrase_query\" : true,\n      \"boost\" : 1.0\n    }\n  }\n}",
          "index_uuid": "GHBPeT5pRnSi3g6DkpIkow",
          "index": "m4",
          "caused_by": {
            "type": "illegal_argument_exception",
            "reason": "Cannot search on field [age] since it is not indexed."
          }
        }
      }
    ]
  },
  "status": 400
}
```

返回的是报错结果，这其中就是`index`参数在起作用。

小结：`index`属性默认为`true`，如果该属性设置为`false`，那么，`elasticsearch`不会为该属性创建索引，也就是说无法当做主查询条件。

## 三 copy_to

现在，再来学习一个`copy_to`属性，该属性允许我们将多个字段的值复制到组字段中，然后将组字段作为单个字段进行查询。

```
PUT m5
{
  "mappings": {
    "doc": {
      "dynamic":false,
      "properties": {
        "first_name":{
          "type": "text",
          "copy_to": "full_name"
        },
        "last_name": {
          "type": "text",
          "copy_to": "full_name"
        },
        "full_name": {
          "type": "text"
        }
      }
    }
  }
}

PUT m5/doc/1
{
  "first_name":"tom",
  "last_name":"ben"
}
PUT m5/doc/2
{
  "first_name":"john",
  "last_name":"smith"
}

GET m5/doc/_search
{
  "query": {
    "match": {
      "first_name": "tom"
    }
  }
}

GET m5/doc/_search
{
  "query": {
    "match": {
      "full_name": "tom"
    }
  }
}
```

上例中，我们将`first_name`和`last_name`都复制到`full_name`中。并且使用`full_name`查询也返回了结果：

```
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 1,
    "max_score" : 0.2876821,
    "hits" : [
      {
        "_index" : "m5",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "first_name" : "tom",
          "last_name" : "ben"
        }
      }
    ]
  }
}
```

返回结果表示查询成功。那么想要查询`tom`或者`smith`该怎么办？

```
GET m5/doc/_search
{
  "query": {
    "match": {
      "full_name": {
        "query": "tom smith",
        "operator": "or"
      }
    }
  }
}
```

将查询条件以空格隔开并封装在`query`内，`operator`参数为多个条件的查询关系也可以是`and`，也有简写方式：

```
GET m5/doc/_search
{
  "query": {
    "match": {
      "full_name": "tom smith"
    }
  }
}
```

`copy_to`还支持将相同的属性值复制给不同的字段。

```
PUT m6
{
  "mappings": {
    "doc": {
      "dynamic":false,
      "properties": {
        "first_name":{
          "type": "text",
          "copy_to": "full_name"
        },
        "last_name": {
          "type": "text",
          "copy_to": ["field1", "field2"]
        },
        "field1": {
          "type": "text"
        },
        "field2": {
          "type": "text"
        }
      }
    }
  }
}


PUT m6/doc/1
{
  "first_name":"tom",
  "last_name":"ben"
}
PUT m6/doc/2
{
  "first_name":"john",
  "last_name":"smith"
}
```

上例中，只需要将`copy_to`的字段以数组的形式封装即可。无论是通过`field1`还是`field2`都可以查询。

小结：

- `copy_to`复制的是属性值而不是属性
- `copy_to`如果要应用于聚合请将`filddata`设置为`true`
- 如果要将属性值复制给多个字段，请用数组，比如`copy_to:["field1", "field2"]`

## 四 对象属性

现在，有一个个人信息文档如下：

```
PUT m7/doc/1
{
  "name":"tom",
  "age":18,
  "info":{
    "addr":"北京",
    "tel":"10010"
  }
}
```

首先，这样嵌套多层的`mappings`该如何设计呢？

```
PUT m7
{
  "mappings": {
    "doc": {
      "dynamic": false,
      "properties": {
        "name": {
          "type": "text"
        },
        "age": {
          "type": "text"
        },
        "info": {
          "properties": {
            "addr": {
              "type": "text"
            },
            "tel": {
              "type" : "text"
            }
          }
        }
      }
    }
  }
}
```

那么，如果要以`name`或者`age`属性作为查询条件查询难不倒我们。

现在如果要以`info`中的`tel`为条件怎么写查询语句呢？

```
GET mapping_test9/doc/_search
{
  "query": {
    "match": {
      "info.tel": "10086"
    }
  }
}
```

上例中，`info`既是一个属性，也是一个对象，我们称为`info`这类字段为对象型字段。该对象内又包含`addr`和`tel`两个字段，如上例这种以嵌套内的字段为查询条件的话，查询语句可以以字段点子字段的方式来写即可。

## 五 settings设置

### 5.1 设置主、复制分片

在创建一个索引的时候，我们可以在`settings`中指定分片信息：

```
PUT s1
{
  "mappings": {
    "doc": {
      "properties": {
        "name": {
          "type": "text"
        }
      }
    }
  }, 
  "settings": {
    "number_of_replicas": 1,
    "number_of_shards": 5
  }
}
```

`number_of_shards`是主分片数量（每个索引默认5个主分片），而`number_of_replicas`是复制分片，默认一个主分片搭配一个复制分片。


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-12-elasticsearch%E4%B9%8Bmappings%E7%9A%84%E5%85%B6%E4%BB%96%E8%AE%BE%E7%BD%AEindexcopy_to%E5%AF%B9%E8%B1%A1%E5%B1%9E%E6%80%A7settings/  

