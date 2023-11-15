# 文档操作-mapping

## 一 前言

我们应该知道，在关系型数据库中，必须先定义表结构，才能插入数据，并且，表结构不会轻易改变。而我们呢，我们怎么玩elasticsearch的呢：

```
PUT t1/doc/1
{
  "name": "小黑"
}
PUT t1/doc/2
{
  "name": "小白",
  "age": 18
}
```

文档的字段可以是任意的，原本都是`name`字段，突然来个`age`。还要elasticsearch自动去猜，哦，可能是个`long`类型，然后加个映射！之后发什么什么？肯定是：猜猜猜，猜你妹！

难道你不想知道elasticsearch内部是怎么玩的吗？

当我们执行上述第一条`PUT`命令后，elasticsearch到底是怎么做的：

```
GET t1
```

结果：

```
{
  "t1" : {
    "aliases" : { },
    "mappings" : {
      "doc" : {
        "properties" : {
          "name" : {
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
    },
    "settings" : {
      "index" : {
        "creation_date" : "1553334893136",
        "number_of_shards" : "5",
        "number_of_replicas" : "1",
        "uuid" : "lHfujZBbRA2K7QDdsX4_wA",
        "version" : {
          "created" : "6050499"
        },
        "provided_name" : "t1"
      }
    }
  }
}
```

由返回结果可以看到，分为两大部分，第一部分关于`t1`索引类型相关的，包括该索引是否有别名`aliases`，然后就是`mappings`信息，包括索引类型`doc`，各字段的详细映射关系都收集在`properties`中。

另一部分是关于索引`t1`的`settings`设置。包括该索引的创建时间，主副分片的信息，UUID等等。

我们再执行第二条`PUT`命令，再查看该索引是否有什么变化，返回结果如下：

```
{
  "t1" : {
    "aliases" : { },
    "mappings" : {
      "doc" : {
        "properties" : {
          "age" : {
            "type" : "long"
          },
          "name" : {
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
    },
    "settings" : {
      "index" : {
        "creation_date" : "1553334893136",
        "number_of_shards" : "5",
        "number_of_replicas" : "1",
        "uuid" : "lHfujZBbRA2K7QDdsX4_wA",
        "version" : {
          "created" : "6050499"
        },
        "provided_name" : "t1"
      }
    }
  }
}
```

由返回结果可以看到，`settings`没有变化，只是`mappings`中多了一条关于`age`的映射关系，这一切都是elasticsearch自动的，但特定的场景下，需要我们更多的设置。

所以，接下来，我们研究一下`mappings`到底是怎么回事！

## 二 映射是什么？

其实，映射`mappings`没那么神秘！说白了，就相当于原来由elasticsearch自动帮我们定义表结构。现在，我们要自己来了，旨在创建索引的时候，有更多定制的内容，更加的贴合业务场景。OK，坐好了，开车！

`elasticsearch`中的映射用来定义一个文档及其包含的字段如何存储和索引的过程。例如，我们可以使用映射来定义：

- 哪些字符串应该被视为全文字段。
- 哪些字段包含数字、日期或者地理位置。
- 定义日期的格式。
- 自定义的规则，用来控制动态添加字段的的映射。

## 三 映射类型

每个索引都有一个映射类型（这话必须放在elasticsearch6.x版本后才能说，之前版本一个索引下有多个类型），它决定了文档将如何被索引。

映射类型有：

- 元字段（meta-fields）：元字段用于自定义如何处理文档关联的元数据，例如包括文档的`_index`、`_type`、`_id`和`_source`字段。
- 字段或属性（field or properties）：映射类型包含与文档相关的字段或者属性的列表。

继续往下走！

## 四 字段的数据类型

- 简单类型，如文本（`text`）、关键字（`keyword`）、日期（`date`）、整形（`long`）、双精度（`double`）、布尔（`boolean`）或`ip`。
- 可以是支持`JSON`的层次结构性质的类型，如对象或嵌套。
- 或者一种特殊类型，如`geo_point`、`geo_shape`或`completion`。

为了不同的目的，以不同的方式索引相同的字段通常是有用的。例如，字符串字段可以作为全文搜索的文本字段进行索引，也可以作为排序或聚合的关键字字段进行索引。或者，可以使用标准分析器、英语分析器和法语分析器索引字符串字段。

这就是多字段的目的。大多数数据类型通过fields参数支持多字段。

## 五 映射约束

在索引中定义太多的字段有可能导致映射**爆炸**！因为这可能会导致内存不足以及难以恢复的情况，为此。我们可以手动或动态的创建字段映射的数量：

- index.mapping.total_fields.limit：索引中的最大字段数。字段和对象映射以及字段别名都计入此限制。默认值为1000。
- index.mapping.depth.limit：字段的最大深度，以内部对象的数量来衡量。例如，如果所有字段都在根对象级别定义，则深度为1.如果有一个子对象映射，则深度为2，等等。默认值为20。
- index.mapping.nested_fields.limit：索引中嵌套字段的最大数量，默认为50.索引1个包含100个嵌套字段的文档实际上索引101个文档，因为每个嵌套文档都被索引为单独的隐藏文档。

## 六 一个简单的映射示例

```
PUT mapping_test1
{
  "mappings": {
    "test1":{
      "properties":{
        "name":{"type": "text"},
        "age":{"type":"long"}
      }
    }
  }
}
```

上例中，我们在创建索引`PUT mapping_test1`的过程中，为该索引定制化类型（设计表结构），添加一个映射类型`test1`；指定字段或者属性都在`properties`内完成。

```
GET mapping_test1
```

通过`GET`来查看。

```
{
  "mapping_test1" : {
    "aliases" : { },
    "mappings" : {
      "test1" : {
        "properties" : {
          "age" : {
            "type" : "long"
          },
          "name" : {
            "type" : "text"
          }
        }
      }
    },
    "settings" : {
      "index" : {
        "creation_date" : "1550469220778",
        "number_of_shards" : "5",
        "number_of_replicas" : "1",
        "uuid" : "7I_m_ULRRXGzWcvhIZoxnQ",
        "version" : {
          "created" : "6050499"
        },
        "provided_name" : "mapping_test1"
      }
    }
  }
}
```

返回的结果中你肯定很熟悉！映射类型是`test1`，具体的属性都被封装在`properties`中。而关于`settings`的配置，我们暂时不管它。

我们为这个索引添加一些数据：

```
put mapping_test1/test1/1
{
  "name":"张开嘴",
  "age":16
}
```

上例中，`mapping_test1`是之前创建的索引，`test1`为之前自定义的`mappings`类型。字段是之前创建好的`name`和`age`。

```
GET mapping_test1/test1/_search
{
  "query": {
    "match": {
      "age": 16
    }
  }
}
```

上例中，我们通过`age`条件查询。

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
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "mapping_test1",
        "_type" : "test1",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "name" : "张开嘴",
          "age" : 16
        }
      }
    ]
  }
}
```

返回了预期的结果信息。


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-10-elasticsearch%E4%B9%8Bmappings/  

