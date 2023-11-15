# 文档操作- ik分词器

## 一 前言

在知名的中分分词器中，ik中文分词器的大名可以说是无人不知，elasticsearch有了ik分词器的加持，就像男人有了神油.......要了解ik中文分词器，就首先要了解一下它的由来。

## 二 ik分词器的由来

IK Analyzer是一个开源的，基于java语言开发的轻量级的中文分词工具包。从2006年12月推出1.0版开始， IK  Analyzer已经推出了4个大版本。最初，它是以开源项目Luence为应用主体的，结合词典分词和文法分析算法的中文分词组件。从3.0版本开始，IK发展为面向Java的公用分词组件，独立于Lucene项目，同时提供了对Lucene的默认优化实现。在2012版本中，IK实现了简单的分词歧义排除算法，标志着IK分词器从单纯的词典分词向模拟语义分词衍化。

IK Analyzer 2012特性：

- 采用了特有的`正向迭代最细粒度切分算法`，支持细粒度和智能分词两种切分模式。在系统环境：Core2 i7 3.4G双核，4G内存，window 7 64位， Sun JDK 1.6_29 64位 普通pc环境测试，IK2012具有160万字/秒（3000KB/S）的高速处理能力。
- 2012版本的智能分词模式支持简单的分词排歧义处理和数量词合并输出。
- 采用了多子处理器分析模式，支持：英文字母、数字、中文词汇等分词处理，兼容韩文、日文字符
- 优化的词典存储，更小的内存占用。支持用户词典扩展定义。特别的，在2012版本，词典支持中文，英文，数字混合词语。

后来，被一个叫medcl（曾勇 elastic开发工程师与布道师，elasticsearch开源社区负责人，2015年加入elastic）的人集成到了elasticsearch中， 并支持自定义字典.......

ps：elasticsearch的ik中文分词器插件由medcl的github上下载，而 IK Analyzer 这个分词器，如果百度搜索的，在开源中国中的提交者是[林良益](https://www.oschina.net/p/ikanalyzer)，由此推断之下，才有了上面的一番由来...........

才有了接下来一系列的扯淡..........

## 三 IK分词器插件的安装

- 打开`Github`官网，搜索`elasticsearch-analysis-ik`，单击`medcl/elasticsearch-analysis-ik`。或者直接[点击](https://github.com/medcl/elasticsearch-analysis-ik/releases)

![](https://img2018.cnblogs.com/blog/1168165/201903/1168165-20190318191757485-1981917922.bmp#alt=img)

- 在`readme.md`文件中，下拉选择历史版本连接。

![](https://img2018.cnblogs.com/blog/1168165/201903/1168165-20190318191829088-847504045.bmp#alt=img)

- 由于`ik`与`elasticsearch`存在兼容问题。所以在下载`ik`时要选择和`elasticsearch`版本一致的，也就是选择`v6.5.4`版本，单击`elasticsearch-analysis-ik-6.5.4.zip`包，自动进入下载到本地。

![](https://img2018.cnblogs.com/blog/1168165/201903/1168165-20190318191844439-1785689661.bmp#alt=img)

- 本地下载成功后，是个`zip`包。

![](https://img2018.cnblogs.com/blog/1168165/201903/1168165-20190318191855331-831606928.bmp#alt=img)

### 3.1 安装

- 首先打开`C:\Program Files\elasticseach-6.5.4\plugins`目录，新建一个名为`ik`的子目录，并将`elasticsearch-analysis-ik-6.5.4.zip`包解压到该`ik`目录内也就是`C:\Program Files\elasticseach-6.5.4\plugins\ik`目录。

![](https://img2018.cnblogs.com/blog/1168165/201903/1168165-20190318191910004-1297092259.bmp#alt=img)

### 3.2 测试

- 首先将`elascticsearch`和`kibana`服务重启。
- 然后地址栏输入`http://localhost:5601`，在`Dev Tools`中的`Console`界面的左侧输入命令，再点击绿色的执行按钮执行。

```
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": "上海自来水来自海上"
}
```

右侧就显示出结果了如下所示：

```
{
  "tokens" : [
    {
      "token" : "上海",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "自来水",
      "start_offset" : 2,
      "end_offset" : 5,
      "type" : "CN_WORD",
      "position" : 1
    },
    {
      "token" : "自来",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 2
    },
    {
      "token" : "水",
      "start_offset" : 4,
      "end_offset" : 5,
      "type" : "CN_CHAR",
      "position" : 3
    },
    {
      "token" : "来自",
      "start_offset" : 5,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 4
    },
    {
      "token" : "海上",
      "start_offset" : 7,
      "end_offset" : 9,
      "type" : "CN_WORD",
      "position" : 5
    }
  ]
}
```

![](https://img2018.cnblogs.com/blog/1168165/201903/1168165-20190318191716062-2127554231.bmp#alt=img)

OK，安装完毕，非常的简单。

### 3.3 ik目录简介

我们简要的介绍一下ik分词配置文件：

- IKAnalyzer.cfg.xml，用来配置自定义的词库
- main.dic，ik原生内置的中文词库，大约有27万多条，只要是这些单词，都会被分在一起。
- surname.dic，中国的姓氏。
- suffix.dic，特殊（后缀）名词，例如`乡、江、所、省`等等。
- preposition.dic，中文介词，例如`不、也、了、仍`等等。
- stopword.dic，英文停用词库，例如`a、an、and、the`等。
- quantifier.dic，单位名词，如`厘米、件、倍、像素`等。

## 四 ik分词器的使用

**before**

- 首先将`elascticsearch`和`kibana`服务重启，让插件生效。
- 然后地址栏输入`http://localhost:5601`，在`Dev Tools`中的`Console`界面的左侧输入命令，再点击绿色的执行按钮执行。

### 4.1 第一个ik示例

来个简单的示例。

```
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": "上海自来水来自海上"
}
```

右侧就显示出结果了如下所示：

```
{
  "tokens" : [
    {
      "token" : "上海",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "自来水",
      "start_offset" : 2,
      "end_offset" : 5,
      "type" : "CN_WORD",
      "position" : 1
    },
    {
      "token" : "自来",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 2
    },
    {
      "token" : "水",
      "start_offset" : 4,
      "end_offset" : 5,
      "type" : "CN_CHAR",
      "position" : 3
    },
    {
      "token" : "来自",
      "start_offset" : 5,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 4
    },
    {
      "token" : "海上",
      "start_offset" : 7,
      "end_offset" : 9,
      "type" : "CN_WORD",
      "position" : 5
    }
  ]
}
```

![](https://img2018.cnblogs.com/blog/1168165/201903/1168165-20190328115659298-1742391491.bmp#alt=img)

那么你可能对开始的`analyzer：ik_max_word`有一丝的疑惑，这个家伙是干嘛的呀？我们就来看看这个家伙到底是什么鬼！

### 4.2 ik_max_word

现在有这样的一个索引：

```
PUT ik1
{
  "mappings": {
    "doc": {
      "dynamic": false,
      "properties": {
        "name": {
          "type": "text",
          "analyzer": "ik_max_word"
        }
      }
    }
  }
}
```

上例中，ik_max_word参数会将文档做最细粒度的拆分，以穷尽尽可能的组合。

接下来为该索引添加几条数据：

```
PUT ik1/doc/1
{
  "content":"今天是个好日子"
}
PUT ik1/doc/2
{
  "content":"心想的事儿都能成"
}
PUT ik1/doc/3
{
  "content":"我今天不活了"
}
```

现在让我们开始查询，随便查！

```
GET ik1/_search
{
  "query": {
    "match": {
      "content": "心想"
    }
  }
}
```

查询结果如下：

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
    "max_score" : 0.2876821,
    "hits" : [
      {
        "_index" : "ik1",
        "_type" : "doc",
        "_id" : "2",
        "_score" : 0.2876821,
        "_source" : {
          "content" : "心想的事儿都能成"
        }
      }
    ]
  }
}
```

成功的返回了一条数据。我们再来以**今天**为条件来查询。

```
GET ik1/_search
{
  "query": {
    "match": {
      "content": "今天"
    }
  }
}
```

结果如下：

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
    "total" : 2,
    "max_score" : 0.2876821,
    "hits" : [
      {
        "_index" : "ik1",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "content" : "今天是个好日子"
        }
      },
      {
        "_index" : "ik1",
        "_type" : "doc",
        "_id" : "3",
        "_score" : 0.2876821,
        "_source" : {
          "content" : "我今天不活了"
        }
      }
    ]
  }
}
```

上例的返回中，成功的查询到了两条结果。

与`ik_max_word`对应还有另一个参数。让我们一起来看下。

### 4.3 ik_smart

与`ik_max_word`对应的是`ik_smart`参数，该参数将文档作最粗粒度的拆分。

```
GET _analyze
{
  "analyzer": "ik_smart",
  "text": "今天是个好日子"
}
```

上例中，我们以最粗粒度的拆分文档。

结果如下：

```
{
  "tokens" : [
    {
      "token" : "今天是",
      "start_offset" : 0,
      "end_offset" : 3,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "个",
      "start_offset" : 3,
      "end_offset" : 4,
      "type" : "CN_CHAR",
      "position" : 1
    },
    {
      "token" : "好日子",
      "start_offset" : 4,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 2
    }
  ]
}
```

再来看看以最细粒度的拆分文档。

```
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": "今天是个好日子"
}
```

结果如下：

```
{
  "tokens" : [
    {
      "token" : "今天是",
      "start_offset" : 0,
      "end_offset" : 3,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "今天",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 1
    },
    {
      "token" : "是",
      "start_offset" : 2,
      "end_offset" : 3,
      "type" : "CN_CHAR",
      "position" : 2
    },
    {
      "token" : "个",
      "start_offset" : 3,
      "end_offset" : 4,
      "type" : "CN_CHAR",
      "position" : 3
    },
    {
      "token" : "好日子",
      "start_offset" : 4,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 4
    },
    {
      "token" : "日子",
      "start_offset" : 5,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 5
    }
  ]
}
```

由上面的对比可以发现，两个参数的不同，所以查询结果也肯定不一样，视情况而定用什么粒度。

在基本操作方面，除了粗细粒度，别的按照之前的操作即可，就像下面两个短语查询和短语前缀查询一样。

### 4.4 ik之短语查询

ik中的短语查询参照之前的短语查询即可。

```
GET ik1/_search
{
  "query": {
    "match_phrase": {
      "content": "今天"
    }
  }
}
```

结果如下：

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
    "total" : 2,
    "max_score" : 0.2876821,
    "hits" : [
      {
        "_index" : "ik1",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "content" : "今天是个好日子"
        }
      },
      {
        "_index" : "ik1",
        "_type" : "doc",
        "_id" : "3",
        "_score" : 0.2876821,
        "_source" : {
          "content" : "我今天不活了"
        }
      }
    ]
  }
}
```

### 4.5 ik之短语前缀查询

同样的，我们第2部分的快速上手部分的操作在ik中同样适用。

```
GET ik1/_search
{
  "query": {
    "match_phrase_prefix": {
      "content": {
        "query": "今天好日子",
        "slop": 2
      }
    }
  }
}
```

结果如下：

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
    "total" : 2,
    "max_score" : 0.2876821,
    "hits" : [
      {
        "_index" : "ik1",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "content" : "今天是个好日子"
        }
      },
      {
        "_index" : "ik1",
        "_type" : "doc",
        "_id" : "3",
        "_score" : 0.2876821,
        "_source" : {
          "content" : "我今天不活了"
        }
      }
    ]
  }
}
```


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/09-15-elasticsearch-ik%E5%88%86%E8%AF%8D%E5%99%A8/  

