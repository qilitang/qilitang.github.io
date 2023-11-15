# 文档操作-查询的两种方式

## 一 前言

简单的没挑战，来点复杂的，elasticsearch提供两种查询方式：

- 查询字符串(query string)，简单查询，就像是像传递URL参数一样去传递查询语句，被称为简单搜索或查询字符串(query string)搜索。
- 另外一种是通过DSL语句来进行查询，被称为DSL查询(Query DSL),DSL是Elasticsearch提供的一种丰富且灵活的查询语言，该语言以json请求体的形式出现，通过restful请求与Elasticsearch进行交互。

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

## 三 查询字符串

```python
GET lqz/doc/_search?q=from:gu
```

还是使用`GET`命令，通过`_serarch`查询，查询条件是什么呢？条件是`from`属性是`gu`家的人都有哪些。最后，别忘了`_search`和`from`属性中间的英文分隔符`?`

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

我们来重点说下`hits`，`hits`是返回的结果集——所有`from`属性为`gu`的结果集。重点中的重点是`_score`得分，得分是什么呢？根据算法算出跟查询条件的匹配度，匹配度高得分就高。后面再说这个算法是怎么回事。

## 四 结构化查询

我们现在使用DSL方式，来完成刚才的查询，查看来自顾家的都有哪些人。

```python
GET lqz/doc/_search
{
  "query": {
    "match": {
      "from": "gu"
    }
  }
}
```

上例，查询条件是一步步构建出来的，将查询条件添加到`match`中即可，而`match`则是查询所有`from`字段的值中含有`gu`的结果就会返回。

当然结果没啥变化：

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


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-2-elasticsearch%E4%B9%8B%E6%9F%A5%E8%AF%A2%E7%9A%84%E4%B8%A4%E7%A7%8D%E6%96%B9%E5%BC%8F/  

