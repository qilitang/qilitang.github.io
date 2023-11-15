# 文档操作-mappings之dynamic的三种状态

## 一 前言

一般的，`mapping`则又可以分为动态映射（dynamic mapping）和静态（显式）映射（explicit mapping）和精确（严格）映射（strict mappings），具体由`dynamic`属性控制。

## 二 动态映射（dynamic：true）

现在有这样的一个索引：

```
PUT m1
{
  "mappings": {
    "doc":{
      "properties": {
        "name": {
          "type": "text"
        },
        "age": {
          "type": "long"
        }
      }
    }
  }
}
```

通过`GET m1/_mapping`看一下`mappings`信息：

```
{
  "m1" : {
    "mappings" : {
      "doc" : {
        "dynamic" : "true",
        "properties" : {
          "age" : {
            "type" : "long"
          },
          "name" : {
            "type" : "text"
          }
        }
      }
    }
  }
}
```

添加一些数据，并且新增一个`sex`字段：

```
PUT m1/doc/1
{
  "name": "小黑",
  "age": 18,
  "sex": "不详"
}
```

当然，新的字段查询也没问题：

```
GET m1/doc/_search
{
  "query": {
    "match": {
      "sex": "不详"
    }
  }
}
```

返回结果：

```
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 1,
    "max_score" : 0.5753642,
    "hits" : [
      {
        "_index" : "m1",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.5753642,
        "_source" : {
          "name" : "小黑",
          "age" : 18,
          "sex" : "不详"
        }
      }
    ]
  }
}
```

现在，一切都很正常，跟elasticsearch自动创建时一样。那是因为，当 Elasticsearch 遇到文档中以前未遇到的字段，它用动态映射来确定字段的数据类型并自动把新的字段添加到类型映射。我们再来看`mappings`你就明白了：

```
{
  "m1" : {
    "mappings" : {
      "doc" : {
        "dynamic" : "true",
        "properties" : {
          "age" : {
            "type" : "long"
          },
          "name" : {
            "type" : "text"
          },
          "sex" : {
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

通过上例可以发下，elasticsearch帮我们新增了一个`sex`的映射。所以。这一切看起来如此自然。这一切的功劳都要归功于`dynamic`属性。我们知道在关系型数据库中，字段创建后除非手动修改，则永远不会更改。但是，elasticsearch默认是允许添加新的字段的，也就是`dynamic：true`。

其实创建索引的时候，是这样的：

```
PUT m1
{
  "mappings": {
    "doc":{
      "dynamic":true,
      "properties": {
        "name": {
          "type": "text"
        },
        "age": {
          "type": "long"
        }
      }
    }
  }
}
```

上例中，当`dynamic`设置为`true`的时候，`elasticsearch`就会帮我们动态的添加映射属性。也就是等于啥都没做！

这里有一点需要注意的是：`mappings`一旦创建，则无法修改。因为Lucene生成倒排索引后就不能改了。

## 三 静态映射（dynamic：false）

现在，我们将`dynamic`值设置为`false`：

```
PUT m2
{
  "mappings": {
    "doc":{
      "dynamic":false,
      "properties": {
        "name": {
          "type": "text"
        },
        "age": {
          "type": "long"
        }
      }
    }
  }
}
```

现在再来测试一下`false`和`true`有什么区别：

```
PUT m2/doc/1
{
  "name": "小黑",
  "age":18
}
PUT m2/doc/2
{
  "name": "小白",
  "age": 16,
  "sex": "不详"
}
```

第二条数据相对于第一条数据来说，多了一个`sex`属性，我们以`sex`为条件来查询一下：

```
GET m2/doc/_search
{
  "query": {
    "match": {
      "sex": "不详"
    }
  }
}
```

结果如下：

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
    "total" : 0,
    "max_score" : null,
    "hits" : [ ]
  }
}
```

结果是空的，也就是什么都没查询到，那是为什呢？来`GET m2/_mapping`一下此时`m2`的`mappings`信息：

``{   "m2" : {     "mappings" : {       "doc" : {          "dynamic" : "false",         "properties" : {           "age" : {             "type" : "long"           },           "name" : {              "type" : "text"           }         }       }     }   } }`可以看到elasticsearch并没有为新增的`sex`建立映射关系。所以查询不到。 当elasticsearch察觉到有新增字段时，因为`dynamic:false`的关系，会忽略该字段，但是仍会存储该字段。 在有些情况下，`dynamic:false`依然不够，所以还需要更严谨的策略来进一步做限制。

## 四 严格模式（dynamic：strict）

让我们再创建一个`mappings`，并且将`dynamic`的状态改为`strict`：

```
PUT m3
{
  "mappings": {
    "doc": {
      "dynamic": "strict", 
      "properties": {
        "name": {
          "type": "text"
        },
        "age": {
          "type": "long"
        }
      }
    }
  }
}
```

现在，添加两篇文档：

```
PUT m3/doc/1
{
  "name": "小黑",
  "age": 18
}
PUT m3/doc/2
{
  "name": "小白",
  "age": 18,
  "sex": "不详"
}
```

第一篇文档添加和查询都没问题。但是，当添加第二篇文档的时候，你会发现报错了：

```
{
  "error": {
    "root_cause": [
      {
        "type": "strict_dynamic_mapping_exception",
        "reason": "mapping set to strict, dynamic introduction of [sex] within [doc] is not allowed"
      }
    ],
    "type": "strict_dynamic_mapping_exception",
    "reason": "mapping set to strict, dynamic introduction of [sex] within [doc] is not allowed"
  },
  "status": 400
}
```

错误提示，严格动态映射异常！说人话就是，当`dynamic:strict`的时候，elasticsearch如果遇到新字段，会抛出异常。

上述这种严谨的作风洒家称为——严格模式！

小结：

- 动态映射（dynamic：true）：动态添加新的字段（或缺省）。
- 静态映射（dynamic：false）：忽略新的字段。在原有的映射基础上，当有新的字段时，不会主动的添加新的映射关系，只作为查询结果出现在查询中。
- 严格模式（dynamic： strict）：如果遇到新的字段，就抛出异常。

一般静态映射用的较多。就像`HTML`的`img`标签一样，`src`为自带的属性，你可以在需要的时候添加`id`或者`class`属性。

当然，如果你非常非常了解你的数据，并且未来很长一段时间不会改变，`strict`不失为一个好选择。


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-11-elasticsearch-mappings%E4%B9%8Bdynamic%E7%9A%84%E4%B8%89%E7%A7%8D%E7%8A%B6%E6%80%81/  

