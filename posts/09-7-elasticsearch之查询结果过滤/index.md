# 文档操作-查询结果过滤

## 一 前言

在未来，一篇文档可能有很多的字段，每次查询都默认给我们返回全部，在数据量很大的时候，是的，比如我只想查姑娘的手机号，你一并给我个喜好啊、三围什么的算什么？

所以，我们对结果做一些过滤，清清白白的告诉elasticsearch

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
```

## 三 结果过滤：_source

现在，在所有的结果中，我只需要查看`name`和`age`两个属性，其他的不要怎么办？

```
GET lqz/doc/_search
{
  "query": {
    "match": {
      "name": "顾老二"
    }
  },
  "_source": ["name", "age"]
}
```

如上例所示，在查询中，通过`_source`来控制仅返回`name`和`age`属性。

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
    "total" : 1,
    "max_score" : 0.8630463,
    "hits" : [
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.8630463,
        "_source" : {
          "name" : "顾老二",
          "age" : 30
        }
      }
    ]
  }
}
```

在数据量很大的时候，我们需要什么字段，就返回什么字段就好了，提高查询效率


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-7-elasticsearch%E4%B9%8B%E6%9F%A5%E8%AF%A2%E7%BB%93%E6%9E%9C%E8%BF%87%E6%BB%A4/  

