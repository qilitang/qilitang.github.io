# 文档操作-排序查询

## 一 准备数据

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

## 二 排序查询：sort

### 2.1 降序：desc

想到排序，出现在脑海中的无非就是升（正）序和降（倒）序。比如我们查询顾府都有哪些人，并根据age字段按照降序，并且，我只想看`nmae`和`age`字段：

```python
GET lqz/doc/_search
{
  "query": {
    "match": {
      "from": "gu"
    }
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ]
}
```

上例，在条件查询的基础上，我们又通过`sort`来做排序，根据`age`字段排序，是降序呢还是升序，由`order`字段控制，`desc`是降序。

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
    "total" : 3,
    "max_score" : null,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "1",
        "_score" : null,
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
        },
        "sort" : [
          30
        ]
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "4",
        "_score" : null,
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
        },
        "sort" : [
          29
        ]
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "3",
        "_score" : null,
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
        },
        "sort" : [
          22
        ]
      }
    ]
  }
}
```

上例中，结果是以降序排列方式返回的。

### 2.2 升序：asc

那么想要升序怎么搞呢？

```python
GET lqz/doc/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "age": {
        "order": "asc"
      }
    }
  ]
}
```

上例，想要以升序的方式排列，只需要将`order`值换为`asc`就可以了。

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
    "total" : 5,
    "max_score" : null,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "2",
        "_score" : null,
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
        },
        "sort" : [
          18
        ]
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "3",
        "_score" : null,
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
        },
        "sort" : [
          22
        ]
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "5",
        "_score" : null,
        "_source" : {
          "name" : "魏行首",
          "age" : 25,
          "from" : "广云台",
          "desc" : "仿佛兮若轻云之蔽月,飘飘兮若流风之回雪,mmp，最后竟然没有嫁给顾老二！",
          "tags" : [
            "闭月",
            "羞花"
          ]
        },
        "sort" : [
          25
        ]
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "4",
        "_score" : null,
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
        },
        "sort" : [
          29
        ]
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "1",
        "_score" : null,
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
        },
        "sort" : [
          30
        ]
      }
    ]
  }
}
```

上例，可以看到结果是以`age`从小到大的顺序返回结果。

## 三 不是什么数据类型都能排序

那么，你可能会问，除了`age`，能不能以别的属性作为排序条件啊？来试试：

```python
GET lqz/chengyuan/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "name": {
        "order": "asc"
      }
    }
  ]
}
```

上例，我们以`name`属性来排序，来看结果：

```python
{
  "error": {
    "root_cause": [
      {
        "type": "illegal_argument_exception",
        "reason": "Fielddata is disabled on text fields by default. Set fielddata=true on [name] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead."
      }
    ],
    "type": "search_phase_execution_exception",
    "reason": "all shards failed",
    "phase": "query",
    "grouped": true,
    "failed_shards": [
      {
        "shard": 0,
        "index": "lqz",
        "node": "wrtr435jSgi7_naKq2Y_zQ",
        "reason": {
          "type": "illegal_argument_exception",
          "reason": "Fielddata is disabled on text fields by default. Set fielddata=true on [name] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead."
        }
      }
    ],
    "caused_by": {
      "type": "illegal_argument_exception",
      "reason": "Fielddata is disabled on text fields by default. Set fielddata=true on [name] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead.",
      "caused_by": {
        "type": "illegal_argument_exception",
        "reason": "Fielddata is disabled on text fields by default. Set fielddata=true on [name] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead."
      }
    }
  },
  "status": 400
}
```

结果跟我们想象的不一样，报错了！

注意：在排序的过程中，只能使用可排序的属性进行排序。那么可以排序的属性有哪些呢？

- 数字
- 日期

其他的都不行！


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-4-elasticsearch%E4%B9%8B%E6%8E%92%E5%BA%8F%E6%9F%A5%E8%AF%A2/  

