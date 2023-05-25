# Elasticsearch 简单操作

Elasticsearch 简单操作
<!--more-->
---

## 前言

现在，让我们启动一个节点和kibana。
接下来的一切操作都在`kibana`中`Dev Tools`下的`Console`里完成。

### 创建一篇文档

现在，我们试图将小黑的小姨妈的个人信息录入elasticsearch。我们只要输入：

```
PUT t1/doc/1
{
 "name": "小黑的小姨妈",
 "age": 18
}
```

PUT表示创建命令。虽然命令可以小写，但是我们推荐大写。在以`REST ful`风格返回的结果中：

```
{
  "_index" : "t1",
  "_type" : "type1",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```

结果中的`result`则是操作类型，现在是`created`，表示第一次创建。如果我们再次点击执行该命令，那么`result`则会是`updated`。我们细心则会发现`_version`开始是1，现在你每点击一次就会增加一次。表示第几次更改。

### 查询所有索引

现在，我们再来学习一条命令：

```
GET _cat/indices?v
```

返回的结果如下图：

[![img](/home/zhang/Desktop/hexo-blog/public/img/1168165-20190321193235561-808372096.png)](https://img2018.cnblogs.com/blog/1168165/201903/1168165-20190321193235561-808372096.png)

上图中，展示当前集群中索引情况，包括，索引的健康状况、UUID、主副分片个数、大小等信息。你发现我们创建的`t1`索引了吗？

### 查询指定的索引信息

我们来单独看看`t1`索引：

```
GET t1
```

返回的结果如下：

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
        "creation_date" : "1553163739688",
        "number_of_shards" : "5",
        "number_of_replicas" : "1",
        "uuid" : "_7jNW5XATheeK84zKkPwlw",
        "version" : {
          "created" : "6050499"
        },
        "provided_name" : "t1"
      }
    }
  }
}
```

返回了`t1`索引的创建信息。

### 查询文档信息

那我们来查看我们刚才创建的那篇文档：

```
GET t1/doc/1
```

返回的结果如下：

```
Copy{
  "_index" : "t1",
  "_type" : "doc",
  "_id" : "1",
  "_version" : 2,
  "found" : true,
  "_source" : {
    "name" : "小黑的小姨妈",
    "age" : 18
  }
}
```

返回了我们刚才创建的文档信息。
我们再来为小黑添加两个姨妈：

```
CopyPUT t1/doc/2
{
 "name": "小黑的二姨妈",
 "age": 16
}
PUT t1/doc/3
{
 "name": "小黑的三姨妈",
 "age": 19
}
```

刚才，我们学会了查询小黑的一个姨妈，那么该如何查询所有姨妈呢？

```
GET t1/doc/_search
```

返回结果如下：

```
Copy{
  "took" : 7,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 3,
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "t1",
        "_type" : "doc",
        "_id" : "2",
        "_score" : 1.0,
        "_source" : {
          "name" : "小黑的二姨妈",
          "age" : 16
        }
      },
      {
        "_index" : "t1",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "name" : "小黑的小姨妈",
          "age" : 18
        }
      },
      {
        "_index" : "t1",
        "_type" : "doc",
        "_id" : "3",
        "_score" : 1.0,
        "_source" : {
          "name" : "小黑的三姨妈",
          "age" : 19
        }
      }
    ]
  }
}
```

现在小黑跟他的姨妈们闹了别扭，就想删除这个姨妈，该怎么办呢？

### 删除指定索引

我们其实直接删除这个`t1`索引就可以了：

```
DELETE /t1
```

DELETE 是删除命令，返回结果如下：

```
{
  "acknowledged" : true
}
```

返回结果提示删除确认成功。

如果此时再查询索引情况，则会发现`t1`已经不存在了，所有的文档也就不存在了。

------

欢迎斧正，that's all

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/elasticsearch-08-es%E7%AE%80%E5%8D%95%E6%93%8D%E4%BD%9C/  

