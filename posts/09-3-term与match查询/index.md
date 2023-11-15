# 文档操作-term于match查询

## 一 match查询

### 1.1 准备数据

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

### 1.2 match系列之match（按条件查询）

我们查看来自顾家的都有哪些人。

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

### 1.3 match系列之match_all（查询全部）

除了按条件查询之外，我们还可以查询`lqz`索引下的`doc`类型中的所有文档，那就是查询全部：

```python
GET lqz/doc/_search
{
  "query": {
    "match_all": {}
  }
}
```

`match_all`的值为空，表示没有查询条件，那就是查询全部。就像`select * from table_name`一样。

查询结果如下：

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
      },
      {
        "_index" : "lqz",
        "_type" : "doc",
        "_id" : "2",
        "_score" : 1.0,
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
        "_id" : "4",
        "_score" : 1.0,
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
        "_score" : 1.0,
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

返回的是`lqz`索引下`doc`类型的所有文档！

### 1.4 match系列之match_phrase（短语查询）

我们现在已经对match有了基本的了解，match查询的是散列映射，包含了我们希望搜索的字段和字符串。也就说，只要文档中只要有我们希望的那个关键字，但也因此带来了一些问题。

首先来创建一些示例：

```python
PUT t1/doc/1
{
  "title": "中国是世界上人口最多的国家"
}
PUT t1/doc/2
{
  "title": "美国是世界上军事实力最强大的国家"
}
PUT t1/doc/3
{
  "title": "北京是中国的首都"
}
```

现在，当我们以`中国`作为搜索条件，我们希望只返回和`中国`相关的文档。我们首先来使用`match`查询：

```python
GET t1/doc/_search
{
  "query": {
    "match": {
      "title": "中国"
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
    "max_score" : 0.68324494,
    "hits" : [
      {
        "_index" : "t1",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.68324494,
        "_source" : {
          "title" : "中国是世界上人口最多的国家"
        }
      },
      {
        "_index" : "t1",
        "_type" : "doc",
        "_id" : "3",
        "_score" : 0.5753642,
        "_source" : {
          "title" : "北京是中国的首都"
        }
      },
      {
        "_index" : "t1",
        "_type" : "doc",
        "_id" : "2",
        "_score" : 0.39556286,
        "_source" : {
          "title" : "美国是世界上军事实力最强大的国家"
        }
      }
    ]
  }
}
```

虽然如期的返回了`中国`的文档。但是却把和`美国`的文档也返回了，这并不是我们想要的。是怎么回事呢？因为这是elasticsearch在内部对文档做分词的时候，对于中文来说，就是一个字一个字分的，所以，我们搜`中国`，`中`和`国`都符合条件，返回，而美国的`国`也符合。

而我们认为`中国`是个短语，是一个有具体含义的词。所以elasticsearch在处理中文分词方面比较弱势。后面会讲针对中文的插件。

但目前我们还有办法解决，那就是使用短语查询：

```python
GET t1/doc/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "中国"
      }
    }
  }
}
```

这里`match_phrase`是在文档中搜索指定的词组，而`中国`则正是一个词组，所以愉快的返回了。

那么，现在我们要想搜索`中国`和`世界`相关的文档，但又忘记其余部分了，怎么做呢？用`match`也不行，那就继续用`match_phrase`试试：

```python
GET t1/doc/_search
{
  "query": {
    "match_phrase": {
      "title": "中国世界"
    }
  }
}
```

返回结果也是空的，因为没有`中国世界`这个短语。

我们搜索`中国`和`世界`这两个指定词组时，但又不清楚两个词组之间有多少别的词间隔。那么在搜的时候就要留有一些余地。这时就要用到了`slop`了。相当于正则中的`中国.*?世界`。这个间隔默认为0，导致我们刚才没有搜到,现在我们指定一个间隔。

```python
GET t1/doc/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "中国世界",
        "slop": 2
      }
    }
  }
}
```

现在，两个词组之间有了2个词的间隔，这个时候，就可以查询到结果了：

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
    "total" : 1,
    "max_score" : 0.7445889,
    "hits" : [
      {
        "_index" : "t1",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.7445889,
        "_source" : {
          "title" : "中国是世界上人口最多的国家"
        }
      }
    ]
  }
}
```

`slop`间隔你可以根据需要适当改动。

短语查询， 比如要查询：python系统

会把查询条件python和系统分词，放到列表中，再去搜索的时候，必须满足python和系统同时存在的才能搜出来

"slop":6 ：python和系统这两个词之间最小的距离

### 1.4 match系列之match_phrase_prefix（最左前缀查询）

现在凌晨2点半，单身狗小黑为了缓解寂寞，就准备搜索几个`beautiful girl`来陪伴自己。但是由于英语没过2级，但单词`beautiful`拼到`bea`就不知道往下怎么拼了。这个时候，我们的智能搜索要帮他啊，elasticsearch就看自己的词库有啥事`bea`开头的词，结果还真发现了两个：

```python
PUT t3/doc/1
{
  "title": "maggie",
  "desc": "beautiful girl you are beautiful so"
}
PUT t3/doc/2
{
  "title": "sun and beach",
  "desc": "I like basking on the beach"
}
```

但这里用`match`和`match_phrase`都不太合适，因为小黑输入的不是完整的词。那怎么办呢？我们用`match_phrase_prefix`来搞：

```python
GET t3/doc/_search
{
  "query": {
    "match_phrase_prefix": {
      "desc": "bea"
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
    "total" : 2,
    "max_score" : 0.39556286,
    "hits" : [
      {
        "_index" : "t3",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.39556286,
        "_source" : {
          "title" : "maggie",
          "desc" : "beautiful girl,you are beautiful so"
        }
      },
      {
        "_index" : "t3",
        "_type" : "doc",
        "_id" : "2",
        "_score" : 0.2876821,
        "_source" : {
          "title" : "sun and beach",
          "desc" : "I like basking on the beach"
        }
      }
    ]
  }
}
```

前缀查询是短语查询类似，但前缀查询可以更进一步的搜索词组，只不过它是和词组中最后一个词条进行前缀匹配（如搜这样的`you are bea`）。应用也非常的广泛，比如搜索框的提示信息，当使用这种行为进行搜索时，最好通过`max_expansions`来设置最大的前缀扩展数量，因为产生的结果会是一个很大的集合，不加限制的话，影响查询性能。

```python
GET t3/doc/_search
{
  "query": {
    "match_phrase_prefix": {
      "desc": {
        "query": "bea",
        "max_expansions": 1
      }
      
    }
  }
}
```

但是，如果此时你去尝试加上`max_expansions`测试后，你会发现并没有如你想想的一样，仅返回一条数据，而是返回了多条数据。

`max_expansions`执行的是搜索的编辑（Levenshtein）距离。那什么是编辑距离呢？编辑距离是一种计算两个字符串间的差异程度的字符串度量（string  metric）。我们可以认为编辑距离就是从一个字符串修改到另一个字符串时，其中编辑单个字符（比如修改、插入、删除）所需要的最少次数。俄罗斯科学家Vladimir Levenshtein于1965年提出了这一概念。

我们再引用elasticsearch官网的一段话：**该max_expansions设置定义了在停止搜索之前模糊查询将匹配的最大术语数，也可以对模糊查询的性能产生显着影响。但是，减少查询字词会产生负面影响，因为查询提前终止可能无法找到某些有效结果。重要的是要理解max_expansions查询限制在分片级别工作，这意味着即使设置为1，多个术语可能匹配，所有术语都来自不同的分片。此行为可能使其看起来好像max_expansions没有生效，因此请注意，计算返回的唯一术语不是确定是否有效的有效方法max_expansions。**。

我想你也没看懂这句话是啥意思，但我们只需知道该参数工作于分片层，也就是Lucene部分，超出我们的研究范围了。

我们快刀斩乱麻的记住，使用前缀查询会非常的影响性能，要对结果集进行限制，就加上这个参数。

### 1.5 match系列之multi_match（多字段查询）

现在，我们有一个50个字段的索引，我们要在多个字段中查询同一个关键字，该怎么做呢？

```python
PUT t3/doc/1
{
  "title": "maggie is beautiful girl",
  "desc": "beautiful girl you are beautiful so"
}
PUT t3/doc/2
{
  "title": "beautiful beach",
  "desc": "I like basking on the beach,and you? beautiful girl"
}
```

我们先用原来的方法查询：

```python
GET t3/doc/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "title": "beautiful"
          }
        },
        {
          "match": {
            "desc": "beautiful"
          }
        }
      ]
    }
  }
}
```

使用`must`来限制两个字段（值）中必须同时含有关键字。这样虽然能达到目的，但是当有很多的字段呢，我们可以用`multi_match`来做：

```python
GET t3/doc/_search
{
  "query": {
    "multi_match": {
      "query": "beautiful",
      "fields": ["title", "desc"]
    }
  }
}
```

我们将多个字段放到`fields`列表中即可。以达到匹配多个字段的目的。

除此之外，`multi_match`甚至可以当做`match_phrase`和`match_phrase_prefix`使用，只需要指定`type`类型即可：

```python
GET t3/doc/_search
{
  "query": {
    "multi_match": {
      "query": "gi",
      "fields": ["title"],
      "type": "phrase_prefix"
    }
  }
}
GET t3/doc/_search
{
  "query": {
    "multi_match": {
      "query": "girl",
      "fields": ["title"],
      "type": "phrase"
    }
  }
}
```

小结：

- match：返回所有匹配的分词。
- match_all：查询全部。
- match_phrase：短语查询，在match的基础上进一步查询词组，可以指定`slop`分词间隔。
- match_phrase_prefix：前缀查询，根据短语中最后一个词组做前缀匹配，可以应用于搜索提示，但注意和`max_expanions`搭配。其实默认是50.......
- multi_match：多字段查询，使用相当的灵活，可以完成`match_phrase`和`match_phrase_prefix`的工作。

## 二 term查询

默认情况下，elasticsearch在对文档分析期间（将文档分词后保存到倒排索引中），会对文档进行分词，比如默认的标准分析器会对文档进行：

- 删除大多数的标点符号。
- 将文档分解为单个词条，我们称为token。
- 将token转为小写。

完事再保存到倒排索引上，当然，原文件还是要保存一分的，而倒排索引使用来查询的。

例如`Beautiful girl!`，在经过分析后是这样的了：

```python
POST _analyze
{
  "analyzer": "standard",
  "text": "Beautiful girl!"
}
# 结果
["beautiful", "girl"]
```

而当在使用match查询时，elasticsearch同样会对查询关键字进行分析：

```python
PUT w10
{
  "mappings": {
    "doc":{
      "properties":{
        "t1":{
          "type": "text"
        }
      }
    }
  }
}

PUT w10/doc/1
{
  "t1": "Beautiful girl!"
}
PUT w10/doc/2
{
  "t1": "sexy girl!"
}
GET w10/doc/_search
{
  "query": {
    "match": {
      "t1": "Beautiful girl!"
    }
  }
}
```

也就是对查询关键字`Beautiful girl!`进行分析，得到`["beautiful", "girl"]`，然后分别将这两个单独的token去索引`w10`中进行查询，结果就是将两篇文档都返回。

这在有些情况下是非常好用的，但是，如果我们想查询确切的词怎么办？也就是精确查询，将`Beautiful girl!`当成一个token而不是分词后的两个token。

这就要用到了term查询了，term查询的是没有经过分析的查询关键字。

但是，这同样需要限制，如果你要查询的字段类型（如上例中的字段`t1`类型是`text`）是`text`（因为elasticsearch会对文档进行分析，上面说过），那么你得到的可能是不尽如人意的结果或者压根没有结果：

```python
GET w10/doc/_search
{
  "query": {
    "term": {
      "t1": "Beautiful girl!"
    }
  }
}
```

如上面的查询，将不会有结果返回，因为索引`w10`中的两篇文档在经过elasticsearch分析后没有一个分词是`Beautiful girl!`，那此次查询结果为空也就好理解了。

所以，我们这里得到一个论证结果：**不要使用term对类型是text的字段进行查询**，要查询text类型的字段，请改用match查询。

学会了吗？那再来一个示例，你说一下结果是什么：

```python
GET w10/doc/_search
{
  "query": {
    "term": {
      "t1": "Beautiful"
    }
  }
}
```

答案是，没有结果返回！因为elasticsearch在对文档进行分析时，会经过小写！人家倒排索引上存的是小写的`beautiful`，而我们查询的是大写的`Beautiful`。

所以，要想有结果你这样：

```python
GET w10/doc/_search
{
  "query": {
    "term": {
      "t1": "beautiful"
    }
  }
}
```

那，term查询可以查询哪些类型的字段呢，例如elasticsearch会将keyword类型的字段当成一个token保存到倒排索引上，你可以将term和keyword结合使用。

最后，要想使用term查询多个精确的值怎么办？我只能说：亲，这里推荐卸载es呢！低调又不失尴尬的玩笑！

这里推荐使用`terms`查询：

```python
GET w10/doc/_search
{
  "query": {
    "terms": {
      "t1": ["beautiful", "sexy"]
    }
  }
}
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-3-term%E4%B8%8Ematch%E6%9F%A5%E8%AF%A2/  

