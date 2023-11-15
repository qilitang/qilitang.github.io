# 文档操作-布尔查询

## 一 前言

布尔查询是最常用的组合查询，根据子查询的规则，只有当文档满足所有子查询条件时，elasticsearch引擎才将结果返回。布尔查询支持的子查询条件共4中：

- must（and）
- should（or）
- must_not（not）
- filter

下面我们来看看每个子查询条件都是怎么玩的。

## 二 准备数据

```python
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

## 三 must

现在，我们用布尔查询所有`from`属性为`gu`的数据：

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "from": "gu"
          }
        }
      ]
    }
  }
}
```

上例中，我们通过在`bool`属性（字段）内使用`must`来作为查询条件，那么条件是什么呢？条件同样被`match`包围，就是`from`为`gu`的所有数据。

这里需要注意的是`must`字段对应的是个列表，也就是说可以有多个并列的查询条件，一个文档满足各个子条件后才最终返回。

结果如下：

```python
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
          "age" : 29,
          "from" : "gu",
          "desc" : "粗中有细，狐假虎威",
          "tags" : [
            "粗",
            "大",
            "猛"
          ]
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "name" : "顾老二",
          "age" : 30,
          "from" : "gu",
          "desc" : "皮肤黑、武器长、性格直",
          "tags" : [
            "黑",
            "长",
            "直"
          ]
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "3",
        "_score" : 0.2876821,
        "_source" : {
          "name" : "龙套偏房",
          "age" : 22,
          "from" : "gu",
          "desc" : "mmp，没怎么看，不知道怎么形容",
          "tags" : [
            "造数据",
            "真",
            "难"
          ]
        }
      }
    ]
  }
}
```

上例中，可以看到，所有`from`属性为`gu`的数据查询出来了。

那么，我们想要查询`from`为`gu`，并且`age`为`30`的数据怎么搞呢？

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "from": "gu"
          }
        },
        {
          "match": {
            "age": 30
          }
        }
      ]
    }
  }
}
```

上例中，在`must`列表中，在增加一个`age`为`30`的条件。

结果如下：

```python
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
    "total" : 1,
    "max_score" : 1.287682,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 1.287682,
        "_source" : {
          "name" : "顾老二",
          "age" : 30,
          "from" : "gu",
          "desc" : "皮肤黑、武器长、性格直",
          "tags" : [
            "黑",
            "长",
            "直"
          ]
        }
      }
    ]
  }
}
```

上例，符合条件的数据被成功查询出来了。

注意：**现在你可能慢慢发现一个现象，所有属性值为列表的，都可以实现多个条件并列存在**

## 四 should

那么，如果要查询只要是`from`为`gu`或者`tags`为`闭月`的数据怎么搞？

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "from": "gu"
          }
        },
        {
          "match": {
            "tags": "闭月"
          }
        }
      ]
    }
  }
}
```

上例中，或关系的不能用`must`的了，而是要用`should`，只要符合其中一个条件就返回。

结果如下：

```python
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
    "total" : 4,
    "max_score" : 0.6931472,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "4",
        "_score" : 0.6931472,
        "_source" : {
          "name" : "石头",
          "age" : 29,
          "from" : "gu",
          "desc" : "粗中有细，狐假虎威",
          "tags" : [
            "粗",
            "大",
            "猛"
          ]
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "5",
        "_score" : 0.5753642,
        "_source" : {
          "name" : "魏行首",
          "age" : 25,
          "from" : "广云台",
          "desc" : "仿佛兮若轻云之蔽月,飘飘兮若流风之回雪,mmp，最后竟然没有嫁给顾老二！",
          "tags" : [
            "闭月",
            "羞花"
          ]
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "name" : "顾老二",
          "age" : 30,
          "from" : "gu",
          "desc" : "皮肤黑、武器长、性格直",
          "tags" : [
            "黑",
            "长",
            "直"
          ]
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "3",
        "_score" : 0.2876821,
        "_source" : {
          "name" : "龙套偏房",
          "age" : 22,
          "from" : "gu",
          "desc" : "mmp，没怎么看，不知道怎么形容",
          "tags" : [
            "造数据",
            "真",
            "难"
          ]
        }
      }
    ]
  }
}
```

返回了所有符合条件的结果。

## 五 must_not

那么，如果我想要查询`from`既不是`gu`并且`tags`也不是`可爱`，还有`age`不是`18`的数据怎么办？

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "must_not": [
        {
          "match": {
            "from": "gu"
          }
        },
        {
          "match": {
            "tags": "可爱"
          }
        },
        {
          "match": {
            "age": 18
          }
        }
      ]
    }
  }
}
```

上例中，`must`和`should`都不能使用，而是使用`must_not`，又在内增加了一个`age`为`18`的条件。

结果如下：

```python
{
  "took" : 9,
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
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "5",
        "_score" : 1.0,
        "_source" : {
          "name" : "魏行首",
          "age" : 25,
          "from" : "广云台",
          "desc" : "仿佛兮若轻云之蔽月,飘飘兮若流风之回雪,mmp，最后竟然没有嫁给顾老二！",
          "tags" : [
            "闭月",
            "羞花"
          ]
        }
      }
    ]
  }
}
```

上例中，只有魏行首这一条数据，因为只有魏行首既不是顾家的人，标签没有可爱那一项，年龄也不等于18！

这里有点需要补充，条件中`age`对应的`18`你写成整形还是字符串都没啥……

## 6 filter

那么，如果要查询`from`为`gu`，`age`大于`25`的数据怎么查？

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "from": "gu"
          }
        }
      ],
      "filter": {
        "range": {
          "age": {
            "gt": 25
          }
        }
      }
    }
  }
}
```

这里就用到了`filter`条件过滤查询，过滤条件的范围用`range`表示，`gt`表示大于，大于多少呢？是25。

结果如下：

```python
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
    "total" : 2,
    "max_score" : 0.6931472,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "4",
        "_score" : 0.6931472,
        "_source" : {
          "name" : "石头",
          "age" : 29,
          "from" : "gu",
          "desc" : "粗中有细，狐假虎威",
          "tags" : [
            "粗",
            "大",
            "猛"
          ]
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "name" : "顾老二",
          "age" : 30,
          "from" : "gu",
          "desc" : "皮肤黑、武器长、性格直",
          "tags" : [
            "黑",
            "长",
            "直"
          ]
        }
      }
    ]
  }
}
```

上例中，`age`大于`25`的条件都已经筛选出来了。

那么要查询`from`是`gu`，`age`大于等于`30`的数据呢？

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "from": "gu"
          }
        }
      ],
      "filter": {
        "range": {
          "age": {
            "gte": 30
          }
        }
      }
    }
  }
}
```

上例中，大于等于用`gte`表示。

结果如下：

```python
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
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "name" : "顾老二",
          "age" : 30,
          "from" : "gu",
          "desc" : "皮肤黑、武器长、性格直",
          "tags" : [
            "黑",
            "长",
            "直"
          ]
        }
      }
    ]
  }
}
```

那么，要查询`age`小于`25`的呢？

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "filter": {
        "range": {
          "age": {
            "lt": 25
          }
        }
      }
    }
  }
}
```

上例中，小于用`lt`表示，结果如下：

```python
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
    "total" : 2,
    "max_score" : 0.0,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "2",
        "_score" : 0.0,
        "_source" : {
          "name" : "大娘子",
          "age" : 18,
          "from" : "sheng",
          "desc" : "肤白貌美，娇憨可爱",
          "tags" : [
            "白",
            "富",
            "美"
          ]
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "3",
        "_score" : 0.0,
        "_source" : {
          "name" : "龙套偏房",
          "age" : 22,
          "from" : "gu",
          "desc" : "mmp，没怎么看，不知道怎么形容",
          "tags" : [
            "造数据",
            "真",
            "难"
          ]
        }
      }
    ]
  }
}
```

在查询一个`age`小于等于`18`的怎么办呢？

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "filter": {
        "range": {
          "age": {
            "lte": 18
          }
        }
      }
    }
  }
}
```

上例中，小于等于用`lte`表示。结果如下：

```python
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
    "max_score" : 0.0,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "2",
        "_score" : 0.0,
        "_source" : {
          "name" : "大娘子",
          "age" : 18,
          "from" : "sheng",
          "desc" : "肤白貌美，娇憨可爱",
          "tags" : [
            "白",
            "富",
            "美"
          ]
        }
      }
    ]
  }
}
```

要查询`from`是`gu`，`age`在`25~30`之间的怎么查？

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "from": "gu"
          }
        }
      ],
      "filter": {
        "range": {
          "age": {
            "gte": 25,
            "lte": 30
          }
        }
      }
    }
  }
}
```

上例中，使用`lte`和`gte`来限定范围。结果如下：

```python
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
    "total" : 2,
    "max_score" : 0.6931472,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "4",
        "_score" : 0.6931472,
        "_source" : {
          "name" : "石头",
          "age" : 29,
          "from" : "gu",
          "desc" : "粗中有细，狐假虎威",
          "tags" : [
            "粗",
            "大",
            "猛"
          ]
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "name" : "顾老二",
          "age" : 30,
          "from" : "gu",
          "desc" : "皮肤黑、武器长、性格直",
          "tags" : [
            "黑",
            "长",
            "直"
          ]
        }
      }
    ]
  }
}
```

那么，要查询`from`是`sheng`，`age`小于等于`25`的怎么查呢？其实结果，我们可能已经想到了，只有一条，因为只有盛家小六符合结果。

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "from": "sheng"
          }
        }
      ],
      "filter": {
        "range": {
          "age": {
            "lte": 25
          }
        }
      }
    }
  }
}
```

结果果然不出洒家所料！

```python
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
    "max_score" : 0.6931472,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "2",
        "_score" : 0.6931472,
        "_source" : {
          "name" : "大娘子",
          "age" : 18,
          "from" : "sheng",
          "desc" : "肤白貌美，娇憨可爱",
          "tags" : [
            "白",
            "富",
            "美"
          ]
        }
      }
    ]
  }
}
```

但是，洒家手一抖，将`must`换为`should`看看会发生什么？

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "from": "sheng"
          }
        }
      ],
      "filter": {
        "range": {
          "age": {
            "lte": 25
          }
        }
      }
    }
  }
}
```

结果如下：

```python
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
        "_id" : "2",
        "_score" : 0.6931472,
        "_source" : {
          "name" : "大娘子",
          "age" : 18,
          "from" : "sheng",
          "desc" : "肤白貌美，娇憨可爱",
          "tags" : [
            "白",
            "富",
            "美"
          ]
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "5",
        "_score" : 0.0,
        "_source" : {
          "name" : "魏行首",
          "age" : 25,
          "from" : "广云台",
          "desc" : "仿佛兮若轻云之蔽月,飘飘兮若流风之回雪,mmp，最后竟然没有嫁给顾老二！",
          "tags" : [
            "闭月",
            "羞花"
          ]
        }
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "3",
        "_score" : 0.0,
        "_source" : {
          "name" : "龙套偏房",
          "age" : 22,
          "from" : "gu",
          "desc" : "mmp，没怎么看，不知道怎么形容",
          "tags" : [
            "造数据",
            "真",
            "难"
          ]
        }
      }
    ]
  }
}
```

结果有点出乎意料，因为龙套偏房和魏行首不属于盛家，但也被查询出来了。那你要问了，怎么肥四？小老弟！这是因为在查询过程中，优先经过`filter`过滤，因为`should`是或关系，龙套偏房和魏行首的年龄符合了`filter`过滤条件，也就被放行了！所以，**如果在`filter`过滤条件中使用`should`的话，结果可能不会尽如人意！建议使用`must`代替**。

注意：**`filter`工作于`bool`查询内**。比如我们将刚才的查询条件改一下，把`filter`从`bool`中挪出来。

```python
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "from": "sheng"
          }
        }
      ]
    },
    "filter": {
      "range"： {
        "age": {
          "lte": 25
        }
      }
    }
  }
}
```

如上例所示，我们将`filter`与`bool`平级，看查询结果：

```python
{
  "error": {
    "root_cause": [
      {
        "type": "parsing_exception",
        "reason": "[bool] malformed query, expected [END_OBJECT] but found [FIELD_NAME]",
        "line": 12,
        "col": 5
      }
    ],
    "type": "parsing_exception",
    "reason": "[bool] malformed query, expected [END_OBJECT] but found [FIELD_NAME]",
    "line": 12,
    "col": 5
  },
  "status": 400
}
```

结果报错了！所以，`filter`工作位置很重要。

小结：

- `must`：与关系，相当于关系型数据库中的`and`。
- `should`：或关系，相当于关系型数据库中的`or`。
- `must_not`：非关系，相当于关系型数据库中的`not`。
- `filter`：过滤条件。
- `range`：条件筛选范围。
- `gt`：大于，相当于关系型数据库中的`>`。
- `gte`：大于等于，相当于关系型数据库中的`>=`。
- `lt`：小于，相当于关系型数据库中的`<`。
- `lte`：小于等于，相当于关系型数据库中的`<=`。


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-6-elasticsearch%E4%B9%8B%E5%B8%83%E5%B0%94%E6%9F%A5%E8%AF%A2/  

