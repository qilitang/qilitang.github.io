# 文档操作-聚合函数

## 一 前言


聚合函数大家都不陌生，elasticsearch中也没玩出新花样，所以，这一章相对简单，只需要记得：

- avg
- max
- min
- sum

以及各自的用法即可。先来看求平均。

## 二 准备数据

```
PUT lqz/doc/1
{
  "name":"顾老二",
  "age":30,
  "from": "gu",
  "desc": "皮肤黑、武器长、性格直",
  "tags": ["黑", "长", "直"]
}

PUT lqz/doc/2
{
  "name":"大娘子",
  "age":18,
  "from":"sheng",
  "desc":"肤白貌美，娇憨可爱",
  "tags":["白", "富","美"]
}

PUT lqz/doc/3
{
  "name":"龙套偏房",
  "age":22,
  "from":"gu",
  "desc":"mmp，没怎么看，不知道怎么形容",
  "tags":["造数据", "真","难"]
}


PUT lqz/doc/4
{
  "name":"石头",
  "age":29,
  "from":"gu",
  "desc":"粗中有细，狐假虎威",
  "tags":["粗", "大","猛"]
}

PUT lqz/doc/5
{
  "name":"魏行首",
  "age":25,
  "from":"广云台",
  "desc":"仿佛兮若轻云之蔽月,飘飘兮若流风之回雪,mmp，最后竟然没有嫁给顾老二！",
  "tags":["闭月","羞花"]
}
```

## 三 avg

现在的需求是查询`from`是`gu`的人的平均年龄。

```

select max(age) as my_avg

GET lqz/doc/_search
{
  "query": {
    "match": {
      "from": "gu"
    }
  },
  "aggs": {
    "my_avg": {
      "avg": {
        "field": "age"
      }
    }
  },
  "_source": ["name", "age"]
}
```

上例中，首先匹配查询`from`是`gu`的数据。在此基础上做查询平均值的操作，这里就用到了聚合函数，其语法被封装在`aggs`中，而`my_avg`则是为查询结果起个别名，封装了计算出的平均值。那么，要以什么属性作为条件呢？是`age`年龄，查年龄的什么呢？是`avg`，查平均年龄。

返回结果如下：

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
    "total" : 3,
    "max_score" : 0.6931472,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "4",
        "_score" : 0.6931472,
        "_source" : {
          "name" : "石头",
          "age" : 29
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "name" : "顾老二",
          "age" : 30
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "3",
        "_score" : 0.2876821,
        "_source" : {
          "name" : "龙套偏房",
          "age" : 22
        }
      }
    ]
  },
  "aggregations" : {
    "my_avg" : {
      "value" : 27.0
    }
  }
}
```

上例中，在查询结果的最后是平均值信息，可以看到是27岁。

虽然我们已经使用`_source`对字段做了过滤，但是还不够。我不想看都有哪些数据，只想看平均值怎么办？别忘了`size`!

```
GET lqz/doc/_search
{
  "query": {
    "match": {
      "from": "gu"
    }
  },
  "aggs": {
    "my_avg": {
      "avg": {
        "field": "age"
      }
    }
  },
  "size": 0, 
  "_source": ["name", "age"]
}
```

上例中，只需要在原来的查询基础上，增加一个`size`就可以了，输出几条结果，我们写上0，就是输出0条查询结果。

查询结果如下：

```
{
  "took" : 8,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 3,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "my_avg" : {
      "value" : 27.0
    }
  }
}
```

查询结果中，我们看`hits`下的`total`值是3，说明有三条符合结果的数据。最后面返回平均值是27。

## 四 max

那怎么查最大值呢？

```
GET lqz/doc/_search
{
  "query": {
    "match": {
      "from": "gu"
    }
  },
  "aggs": {
    "my_max": {
      "max": {
        "field": "age"
      }
    }
  },
  "size": 0
}
```

上例中，只需要在查询条件中将`avg`替换成`max`即可。

返回结果如下：

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
    "total" : 3,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "my_max" : {
      "value" : 30.0
    }
  }
}
```

在返回的结果中，可以看到年龄最大的是30岁。

## 五 min

那怎么查最小值呢？

```
GET lqz/doc/_search
{
  "query": {
    "match": {
      "from": "gu"
    }
  },
  "aggs": {
    "my_min": {
      "min": {
        "field": "age"
      }
    }
  },
  "size": 0
}
```

最小值则用`min`表示。

返回结果如下：

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
    "total" : 3,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "my_min" : {
      "value" : 22.0
    }
  }
}
```

返回结果中，年龄最小的是22岁。

## 六 sum

那么，要是想知道它们的年龄总和是多少怎么办呢？

```
GET lqz/doc/_search
{
  "query": {
    "match": {
      "from": "gu"
    }
  },
  "aggs": {
    "my_sum": {
      "sum": {
        "field": "age"
      }
    }
  },
  "size": 0
}
```

上例中，求和用`sum`表示。

```
{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 3,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "my_sum" : {
      "value" : 81.0
    }
  }
}
```

从返回的结果可以发现，年龄总和是81岁。

## 七  分组查询

现在我想要查询所有人的年龄段，并且按照`15~20，20~25,25~30`分组，并且算出每组的平均年龄。

分析需求，首先我们应该先把分组做出来。

```
GET lqz/doc/_search
{
  "size": 0, 
  "query": {
    "match_all": {}
  },
  "aggs": {
    "age_group": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 15,
            "to": 20
          },
          {
            "from": 20,
            "to": 25
          },
          {
            "from": 25,
            "to": 30
          }
        ]
      }
    }
  }
}
```

上例中，在`aggs`的自定义别名`age_group`中，使用`range`来做分组，`field`是以`age`为分组，分组使用`ranges`来做，`from`和`to`是范围，我们根据需求做出三组。

```
{
  "took" : 3,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 5,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "age_group" : {
      "buckets" : [
        {
          "key" : "15.0-20.0",
          "from" : 15.0,
          "to" : 20.0,
          "doc_count" : 1
        },
        {
          "key" : "20.0-25.0",
          "from" : 20.0,
          "to" : 25.0,
          "doc_count" : 1
        },
        {
          "key" : "25.0-30.0",
          "from" : 25.0,
          "to" : 30.0,
          "doc_count" : 2
        }
      ]
    }
  }
}
```

返回的结果中可以看到，已经拿到了三个分组。`doc_count`为该组内有几条数据，此次共分为三组，查询出4条内容。还有一条数据的`age`属性值是`30`，不在分组的范围内！

那么接下来，我们就要对每个小组内的数据做平均年龄处理。

```
GET lqz/doc/_search
{
  "size": 0, 
  "query": {
    "match_all": {}
  },
  "aggs": {
    "age_group": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 15,
            "to": 20
          },
          {
            "from": 20,
            "to": 25
          },
          {
            "from": 25,
            "to": 30
          }
        ]
      },
      "aggs": {
        "my_avg": {
          "avg": {
            "field": "age"
          }
        }
      }
    }
  }
}
```

上例中，在分组下面，我们使用`aggs`对`age`做平均数处理，这样就可以了。

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
   "total" : 5,
   "max_score" : 0.0,
   "hits" : [ ]
 },
 "aggregations" : {
   "age_group" : {
     "buckets" : [
       {
         "key" : "15.0-20.0",
         "from" : 15.0,
         "to" : 20.0,
         "doc_count" : 1,
         "my_avg" : {
           "value" : 18.0
         }
       },
       {
         "key" : "20.0-25.0",
         "from" : 20.0,
         "to" : 25.0,
         "doc_count" : 1,
         "my_avg" : {
           "value" : 22.0
         }
       },
       {
         "key" : "25.0-30.0",
         "from" : 25.0,
         "to" : 30.0,
         "doc_count" : 2,
         "my_avg" : {
           "value" : 27.0
         }
       }
     ]
   }
 }
}
```

在结果中，我们可以清晰的看到每组的平均年龄（`my_avg`的`value`中）。

注意：**聚合函数的使用，一定是先查出结果，然后对结果使用聚合函数做处理**

小结：

- avg：求平均
- max：最大值
- min：最小值
- sum：求和

---

欢迎斧正，that's all


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-9-elasticsearch%E4%B9%8B%E8%81%9A%E5%90%88%E5%87%BD%E6%95%B0/  

