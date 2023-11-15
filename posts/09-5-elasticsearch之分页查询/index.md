# 文档操作-分页查询

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

## 二 分页查询：from/size

我们来看看elasticsearch是怎么将结果分页的：

```python
GET lqz/doc/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ], 
  "from": 2,
  "size": 1
}
```

上例，首先以`age`降序排序，查询所有。并且在查询的时候，添加两个属性`from`和`size`来控制查询结果集的数据条数。

- from：从哪开始查
- size：返回几条结果

如上例的结果：

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
      }
    ]
  }
}
```

上例中，在返回的结果集中，从第2条开始，返回1条数据。

那如果想要从第2条开始，返回2条结果怎么做呢？

```python
GET lqz/doc/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ], 
  "from": 2,
  "size": 2
}
```

上例中，我们指定`from`为2，意为从第2条开始返回，返回多少呢？`size`意为2条。

还可以这样：

```python
GET lqz/doc/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ], 
  "from": 4,
  "size": 2
}
```

上例中，从第4条开始返回2条数据。

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
      }
    ]
  }
}
```

上例中仅有一条数据，那是为啥呢？因为我们现在只有5条数据，从第4条开始查询，就只有1条符合条件，所以，就返回了1条数据。

学到这里，我们也可以看到，我们的查询条件越来越多，开始仅是简单查询，慢慢增加条件查询，增加排序，对返回结果进行限制。所以，我们可以说：**对于`elasticsearch`来说，所有的条件都是可插拔的，彼此之间用`,`分割**。比如说，我们在查询中，仅对返回结果进行限制：

```python
GET lqz/doc/_search
{
  "query": {
    "match_all": {}
  },
  "from": 4,
  "size": 2
}
```

上例中，在所有的返回结果中，结果从4开始返回2条数据。

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
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "3",
        "_score" : 1.0,
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

但我们只有1条符合条件的数据。


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-5-elasticsearch%E4%B9%8B%E5%88%86%E9%A1%B5%E6%9F%A5%E8%AF%A2/  

