# ElasticSearch删除映射类型

## 一 前言

官方解释：[https://www.elastic.co/guide/en/elasticsearch/reference/6.0/removal-of-types.html](https://www.elastic.co/guide/en/elasticsearch/reference/6.0/removal-of-types.html)

在elasticsearch6.0.0或更高的版本中创建索引仅能包含单个映射类型。在具有多种映射类型的5.x版本中创建的索引将继续像以前一样在elasticsearch6.x中运行。类型将在elasticsearch7.0.0中的API中弃用，并在8.0.0中完全删除。

## 二 什么是映射类型？

从elasticsearch发布以来，每个文档都存储在单个索引中并分配了单个映射类型。映射类型用于表示要编制索引的文档或实体的类型。例如微博（twitter）索引可能具有用户（user）类型和推文（tweet）两个类型。

每种映射类型都可以有自己的字段，因此用户（user）类型可能有`full_name`、`user_name`、`email`字段；而推文（tweet）类型可能有`content`、`tweet_at`字段和用户（user）类型的`user_name`字段。

每个文档都有一个`_type`包含类型名称的元字段，通过在URL中指定类型名称，搜索可以限制为一种或多种类型：

```
GET twitter/user,tweet/_search
{
  "query":{
    "match":{
      "user_name":"kimchy"
    }
  }
}
```

该`_type`字段与文档组合`_id`以生成`_uid`字段，因此具有相同类型的文档`_id`可以存储在单个索引中。

映射类型也用于在文档中建立[父子关系](https://www.elastic.co/guide/en/elasticsearch/reference/6.0/mapping-parent-field.html)，因此类型的文档`question`可以是类型文档的父类`answer`。

扯了半天淡，一切不都是挺好的嘛？那还为啥要删除映射类型呢？

## 三 为什么要删除映射类型？

最初（其实到现在），为了便于理解elasticsearch的数据组织，通常拿elasticsearch和关系型数据库做对比，比如我们谈到一个es索引（index）时，通常将它比喻为类似于SQL数据库中的`database`，而类型（type）等同于SQL数据库中的表。

这真是一个糟糕的比喻！让我们有了错误理解。因为在SQL数据库中，表彼此独立，一个表中的字段与另一个表中具有相同名称的字段无关，而映射类型中的字段不是这种情况。

在elasticsearch的索引中，不同映射类型具有相同名称的字段在内部由相同的Lucene字段支持。换句话说，使用上面的示例，用户（user）类型中的`user_name`字段存储在和推文（tweet）类型中的`user_name`字段完全相同的字段中，而且两种类型中的`user_name`字段必须具有相同的映射（定义）。

当我们希望删除一个类型的日期字段和同一个索引中另一个类型的布尔字段时，这可能会导致挫败感（可以理解为删除失败）。

最重要的是，在同一索引中存储具有很少或没有共同字段的不同实体会导致稀疏数据并干扰Lucene有效压缩文档的能力。

出于这些原因，我们决定从elasticsearch中删除映射类型的概念。

## 四 映射类型的替代方法

### 4.1 将映射类型分开存储在索引中

第一种方法是每个文档类型都有一个索引，例如微博（twitter）索引中，我们可以将推文（tweet）类型和用户（user）类型分开，分别存储在独立的索引中。这样两个相互的索引就不会引起字段冲突了。

这中方法有两个好处：

- 数据更可能是密集的，因此受益于Lucene中使用的压缩技术。
- 用于全文搜索评分的词条统计将会更精确，应为同一索引中的所有文档都代表单个实体。

每个索引的大小可以根据其包含的文档数量进行适当的调整，比如我们为用户（user）类型分配较少的主分片，而为推文（tweet）类型分配较多的主分片。

### 4.2 自定义类型字段回到顶部

当然了，集群中可以存储多少个主分片是有限制的，我们不希望仅为几千个文档的集合而浪费整个分片。在这种情况下，我们可以实现自己的自定义`type`字段，该字段的工作方式与旧的`_type`相似。

还是上面微博（twitter）例子，最初，它的映射类型看起来是这样的：

```
PUT twitter
{
  "mappings": {
    "user":{
      "properties":{
        "name":{
          "type":"text"
        },
        "user_name":{
          "type":"keyword"
        },
        "email":{
          "type":"keyword"
        }
      }
    },
    "tweet":{
      "properties":{
        "content":{
          "type":"text"
        },
        "user_name":{
          "type":"keyword"
        },
        "tweet_at":{
          "type":"date"
        }
      }
    }
  }
}

PUT twitter/user/kimchy
{
  "name":"狗子",
  "user_name":"二狗子",
  "email":"dog@twodog.com"
}

PUT twitter/tweet/1
{
  "name":"kimchy",
  "tweet_ad":"2019-04-30T10:26:20Z",
  "content":"单身狗求包养"
}


GET twitter/tweet/_search
{
  "query": {
    "match": {
      "user_name": "kimchy"
    }
  }
}
```

如上示例，请在5.x及以下版本测试

我们也可以通过添加自定义`type`字段来实现相同目的：

```
PUT twitter
{
  "mappings": {
    "doc":{
      "properties":{
        "type":{
          "type":"keyword"
        },
        "name":{
          "type":"text"
        },
        "user_name":{
          "type":"keyword"
        },
        "email":{
          "type":"text"
        },
        "content":{
          "type":"text"
        },
        "tweet_at":{
          "type":"date"
        }
      }  
    }
  }
}

PUT twitter/doc/user-kimchy
{
  "type":"user",
  "name":"狗子",
  "user_name":"二狗子",
  "email":"dog@twodog.com"
}


PUT twitter/doc/tweet-1
{
  "type":"tweet",
  "user_name":"kimchy",
  "tweet_at":"2019-04-30T10:26:20Z",
  "content":"单身狗求包养"
}

GET twitter/_search
{
  "query": {
    "bool": {
      "must":[
        {
          "match": {
            "user_name": "kimchy"
          }
        }
      ],
      "filter": {
        "match":{
          "type":"tweet"
        }
      }
    }
  }
}
```

上述示例6.5.4版本运行无误。

## 五 没有映射类型的父/子

以前，通过将一个映射类型设置为父级，将一个或多个其他映射类型设置为子级来表示父子关系。现在，没有了多类型，我们就不能再使用这种语法了。除了表示文档之间的关系方式已改为使用新的[join字段](https://www.elastic.co/guide/en/elasticsearch/reference/6.0/parent-join.html)之外，父子特征将继续像以前一样运行。

## 六 删除映射类型的计划

这个删除映射类型的计划，对于用户来说是一个很大的变化，所以我们试图让它尽可能轻松，更改将如下所示：

在elasticsearch5.6.0中：

- index.mapping.single_type:true在索引上设置将启用在6.0中强制执行的单索引类型。
- 父子的[join字段](https://www.elastic.co/guide/en/elasticsearch/reference/6.0/parent-join.html)替换可用于在5.6中创建索引。

在elasticsearch6.x中：

- 在5.x中创建的索引将继续在6.x中运行，就像在5.x中一样。
- 在6.x中创建的索引仅允许每个索引使用单一类型，任何字段都可以用于该类型，但必须是唯一的。
- 该`_type`名称可以不再与`_id`组合形成`_uid`字段，`_uid`字段已成为`_id`字段的别名。
- 新索引不再支持旧的父/子关系，而是应该使用连接字段。
- 不推荐使用`_default_mapping`类型。
- 在6.7中，索引创建、索引模板和映射API支持查询字符串参数（include_type_name），该参数仅表示请求和响应是否应该包含类型名称，默认为true，应该设置为一个显式值，以便准备升级到7.0。未设置`include_type_name`将导致一个弃用警告，没有显式类型的索引将使用默认的类型名称`_doc`。

在elasticsearch7.x中：

- 不推荐在请求中指定类型。例如，索引文档不再需要文档类型。对于自动生成的id，新的索引API在显式ids和`POST {index_name}/_doc`的情况下是`PUT {index_name}/_doc/{id}`。
- 索引创建，索引模板和映射API中的`include_type_name`参数将默认为false，未设置参数将导致启动警告。
- 删除了`_default_mapping`类型。

在elasticsearch8.x中：

- 不在支持在请求中指定类型。
- `include_type_name`参数已删除。

## 七将多类型索引迁移到单一类型

[Reindex API](https://www.elastic.co/guide/en/elasticsearch/reference/6.0/docs-reindex.html)可用于将多类型索引转换为单类型索引。下面的例子可以在Elasticsearch 5.6或Elasticsearch 6.x中使用。在6.x中，不需要指定`index.mapping`。默认为单一类型。

### 7.1 每种文档类型的索引

第一个示例将微博（twitter）索引拆分为推文（tweets）索引和用户（users）索引：

```
PUT users
{
  "mappings": {
    "user":{
      "properties":{
        "name":{
          "type":"text"
        },
        "user_name":{
          "type":"keyword"
        },
        "email":{
          "type":"keyword"
        }
      }
    }
  }
}

PUT tweets
{
  "mappings": {
    "tweet":{
      "properties":{
        "content":{
          "type":"text"
        },
        "user_name":{
          "type":"keyword"
        },
        "tweet_at":{
          "type":"date"
        }
      }
    }
  }
}

POST _reindex
{
  "source": {
    "index":"twitter",
    "type":"user"
  },
  "dest": {
    "index":"users"
  }
}

POST _reindex
{
  "source": {
    "index":"twitter",
    "type":"tweet"
  },
  "dest": {
    "index": "tweets"
  }
}
```

上述代码在6.5.4版本中运行无误。

上述的示例意思是，在之前我们在微博（twitter）索引中，有两个类型（tweet和user）。现在要将两个类型分开，成为独立的索引。所以，首先先创建出各自的索引（tweets和users），然后通过`POST _reindex`来完成迁移工作。

### 7.2 自定义类型字段

第二个示例添加自自定义的`type`字段，并将其设置为原始值`_type`。它还添加了类型到id，以防有任何不同类型的文档具有冲突的id：

```
PUT new_twitter
{
  "mappings": {
    "doc":{
      "properties":{
        "type":{
          "type":"keyword"
        },
        "name":{
          "type":"text"
        },
        "user_name":{
          "type":"keyword"
        },
        "email":{
          "type":"keyword"
        },
        "content":{
          "type":"text"
        },
        "tweet_at":{
          "type":"date"
        }
      }
    }
  }
}


POST _reindex
{
  "source": {
    "index":"twitter"
  },
  "dest":{
    "index": "new_twitter"
  },
  "script": {
    "source": """
      ctx._source.type = ctx._type;
      ctx._id = ctx._type + "-" + ctx._id;
      ctx._type = "doc";
      
    """
  }
}
```

上述代码在6.5.4版本运行无误。

## 八 总结

总之，通篇看下来，如果对elasticsearch，尤其是各版本不太了解的话，这篇文档看着索然无味！重要的是看不懂，如果我们是新手，接触elasticsearch的时候，就是从6.x版本开始的，那只要记得，一个索引下面只能创建一个类型就行了，其中各字段都具有唯一性，如果在创建映射的时候，如果没有指定文档类型，那么该索引的默认索引类型是`_doc`，不指定文档id则会内部帮我们生成一个id字符串。其他的，who care？


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/17-%E6%89%A9%E5%B1%95%E9%98%85%E8%AF%BB-%E5%88%A0%E9%99%A4%E6%98%A0%E5%B0%84%E7%B1%BB%E5%9E%8B/  

