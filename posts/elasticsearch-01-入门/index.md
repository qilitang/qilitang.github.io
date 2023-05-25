# 什么是ElasticSearch

什么是ElasticSearch
<!--more-->
---

### 什么是ElasticSearch

现在，你还离得开搜索吗？无论是Google还是百度提供的搜索入口，还是项目自己的搜索，比如QQ提供的搜索入口等等，都大大的方便了我们的工作、生活。但是你有没有想过——搭建属于自己的搜索服务，应用于你的博客项目、公司项目……
无论你想不想，都要学习！因为随着公司业务的增长，数据也爆炸性增长。对于数据的处理、日志分析，如果还采用传统的方法，这恐怕是灾难性的。所以，我们是时候学习一个先进的搜索引擎了。
Elasticsearch是一个基于Apache Lucene(TM)的开源搜索引擎。无论在开源还是专有领域，Lucene可以被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。
但是，Lucene只是一个库。想要使用它，你必须使用Java来作为开发语言并将其直接集成到你的应用中，更糟糕的是，Lucene非常复杂，你需要深入了解检索的相关知识来理解它是如何工作的。
Elasticsearch也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单。
你以为这些就完了？elasticsearch除了Lucene和全文搜索，我们还可以描述它：

- 分布式的实时文件存储，每个字段都被索引并可被搜索
- 分布式的实时分析搜索引擎
- 可以扩展到上百台服务器，处理PB级结构化或非结构化数据。

并且，这些功能都被集成到一个服务里面，elasticsearch也提供的与其它语言的接口，其中包括：

- Java
- JavaScript
- Groovy
- .NET
- PHP
- Perl
- Python
- Ruby
- 以及社区贡献的[更多接口](https://www.elastic.co/guide/en/elasticsearch/client/community/current/index.html)

使用我们喜欢的语言通过RESTful API接口，访问9200端口，就可以与elasticsearch玩耍了。
上手elasticsearch非常容易，它提供了许多合理的缺省值，并对初学者隐藏了复杂的搜索引擎理论。它开箱即用（安装即可使用），只需很少的学习既可在生产环境中使用。
随着越学越深入，还可以利用Elasticsearch更多高级的功能，整个引擎可以很灵活地进行配置。可以根据自身需求来定制属于自己的Elasticsearch。

### ElasticSearch 模糊历史

多年前，一个叫做Shay Banon的刚结婚不久的失业开发者，由于妻子要去伦敦学习厨师，他便跟着也去了。在他找工作的过程中，为了给妻子构建一个食谱的搜索引擎，他开始构建一个早期版本的Lucene。
直接基于Lucene工作会比较困难，所以Shay开始抽象Lucene代码以便Java程序员可以在应用中添加搜索功能。他发布了他的第一个开源项目，叫做“Compass”。

后来Shay找到一份工作，这份工作处在高性能和内存数据网格的分布式环境中，因此高性能的、实时的、分布式的搜索引擎也是理所当然需要的。然后他决定重写Compass库使其成为一个独立的服务叫做Elasticsearch。
第一个公开版本出现在2010年2月，在那之后Elasticsearch已经成为Github上最受欢迎的项目之一，代码贡献者超过300人。一家主营Elasticsearch的公司就此成立，他们一边提供商业支持一边开发新功能，不过Elasticsearch将永远开源且对所有人可用。
Shay的妻子依旧等待着她的食谱搜索……



### ElasticSearch ： 面向文档（工作原理）

我们知道，关系型数据库以记录和行的形式存储数据，但是在elasticsearch中，是以文档的形式存储数据。
但区别在于，文档要比数据表的行更加灵活。因为文档可以是多层次的，它（文档）鼓励你将属于一个逻辑实体的数据保存在同一个文档中，而不是散落在各个表的不同行中。这样查询效率很高，因为我们无需连接其他的表，我们学习关系型数据库时，一定知道连表查询（尤其是连接多张表）是多么的费时吧！

### 成功案例

之前有人说，elasticsearch的缺点之一是没有成熟的案例加持，那我们就来看看elasticsearch都有哪些成熟的案例：

- 维基百科使用Elasticsearch来进行全文搜做并高亮显示关键词，以及提供search-as-you-type、did-you-mean等搜索建议功能。
- 英国卫报使用Elasticsearch来处理访客日志，以便能将公众对不同文章的反应实时地反馈给各位编辑。
- StackOverflow将全文搜索与地理位置和相关信息进行结合，以提供more-like-this相关问题的展现。
- GitHub使用Elasticsearch来检索超过1300亿行代码，可以参考[A Whole New Code Search](https://github.blog/2013-01-23-a-whole-new-code-search/)
- 每天，Goldman Sachs使用它来处理5TB数据的索引，还有很多投行使用它来分析股票市场的变动。
- 苏宁在大数据平台使用es存储600TB数据，集群规模包括：搭建超过500+物理机，30万shards，80000index。[参考](https://elasticsearch.cn/slides/203#page=5)
- 腾讯在日志实时分析中采用es，处理高并发100W/S，PE级数据。
- 更多[参考](https://elasticsearch.cn/slides/category-38)

所以，elasticsearch可以灵活的应用于我们的项目中。

###  如何学elasticsearch

除了万能的百度和Google 之外，我们还有一些其他的学习途径：

- elasticsearch[官方文档](https://www.elastic.co/guide/index.html)：这个比较好点，可以多多参考
- elasticsearch[博客](https://www.elastic.co/cn/blog?ultron=cn-es-getting-started-webinar&blade=touch&hulk=email&mkt_tok=eyJpIjoiWXpBeE9UbG1OemMwTnpBNCIsInQiOiI5SUlMWkNjaXZPRWxOb3B2ZnUyOVZDcFwvTDQ2MXVTUEdPcnpPR1RobDc4NEtURTAwZWkrVDZKSDVPemdVb0drbk5QXC9rVkpCZWVUcTRYMkEzazVKdzdKV2JGT01IWW1yMTZwdjlOcDdOeUYzWGdjZ2ZDSTQyN2J5bWNWVlNmdUdNIn0%3D)：这个吧，看看就行
- elasticsearch[社区](https://elasticsearch.cn/?ultron=cn-es-getting-started-webinar&blade=touch&hulk=email&mkt_tok=eyJpIjoiWXpBeE9UbG1OemMwTnpBNCIsInQiOiI5SUlMWkNjaXZPRWxOb3B2ZnUyOVZDcFwvTDQ2MXVTUEdPcnpPR1RobDc4NEtURTAwZWkrVDZKSDVPemdVb0drbk5QXC9rVkpCZWVUcTRYMkEzazVKdzdKV2JGT01IWW1yMTZwdjlOcDdOeUYzWGdjZ2ZDSTQyN2J5bWNWVlNmdUdNIn0%3D)：社区还是很好的
- elasticsearch[视频](https://www.elastic.co/cn/videos/)：包括入门视频什么的
- elasticsearch实战：该书籍的质量还是不错的。
- elasticsearch权威指南：同样的，这个也不错。

### ElasticSearch 能处理的数据量

一个很好地问题，不幸的是，单一索引的极限取决于存储索引的硬件、索引的设计、如何处理数据以及你为索引备份了多少副本。

通常来说，一个Lucene索引（也就是一个elasticsearch分片）不能处理多于21亿篇文档，或者多于2740亿的唯一词条。但达到这个极限之前，我们可能就没有足够的磁盘空间了！
当然，一个分片如何很大的话，读写性能将会变得非常差。

扯了半天的淡，让我们开始一个灵活的学习之旅吧。

------

see also： [入门指南](https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started.html) | [Elasticsearch Clients](https://www.elastic.co/guide/en/elasticsearch/client/index.html) | [搜索引擎选择： Elasticsearch与Solr](https://zhuanlan.zhihu.com/p/24286279) | [Elasticsearch 基本介绍及其与 Python 的对接实现](https://cuiqingcai.com/6214.html) | [Elasticsearch权威指南中文版（截止本博客发表，还没有翻译完.......）](https://es.xiaoleilu.com/010_Intro/05_What_is_it.html) 欢迎斧正，that's all





---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/elasticsearch-01-%E5%85%A5%E9%97%A8/  

