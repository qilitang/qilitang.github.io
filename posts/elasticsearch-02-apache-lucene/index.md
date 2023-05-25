# Apache Lucene

什么是 Apache Lucene
<!--more-->
---

###  有必要了解的有Apache

[Apache](https://baike.baidu.com/item/Lucene/6753302)软件基金会（也就是Apache Software Foundation，简称为ASF）是专门为运作一个开源软件项目的Apache 的团体提供支持的非盈利性组织，这个开源软件的项目就是 Apache 项目。
最初，Apache基金会的开发爱好者开发并维护一个叫Apache的HTTP服务器。
后来，Apache服务器越来越火，就启动了更多的项目，比如PHP、Java Apache以及更多的子项目。比如Jakarta。

### Jakarta

[Jakarta](https://baike.baidu.com/item/Jakarta/15952232#viewPageContent)是为了发展Java容器而启动的Java Apache的项目。后来随着Java的火爆而成为了囊括了众多基于Java语言开源软件子项目的项目。比如从这里孵化出了[Tomcat](https://baike.baidu.com/item/tomcat/255751?fr=aladdin)、[ant](https://baike.baidu.com/item/apache ant/1065741?fr=aladdin)、[Struts](https://baike.baidu.com/item/Struts/485073)、[Lucene](https://baike.baidu.com/item/Lucene)。Jakarta

### Lucene

[Lucene](https://baike.baidu.com/item/Lucene/6753302)是[Apache](https://lucene.apache.org/)软件基金会4 jakarta项目的子项目。它是一个开源的全文检索引擎工具包。但它并不是一个完整的全文检索引擎，而是一个全文检索引擎的架构，提供了完整的查询引擎和索引引擎，部分文本分析引擎（英文与德文两种西方语言）。Lucene的目的是为软件开发人员提供一个简单易用的工具包，以方便的在目标系统中实现全文检索的功能，或者是以此为基础建立起完整的全文检索引擎。Lucene是一套用于全文检索和搜寻的开源程式库，由Apache软件基金会支持和提供。Lucene提供了一个简单却强大的应用程式接口，能够做全文索引和搜寻。在Java开发环境里Lucene是一个成熟的免费开源工具。
最后，引用来自《[Elasticsearch权威指南](https://es.xiaoleilu.com/010_Intro/05_What_is_it.html)》书中关于Lucene的描述作为总结：

> Lucene可以被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。
> 但是，Lucene只是一个库。想要使用它，你必须使用Java来作为开发语言并将其直接集成到你的应用中，更糟糕的是，Lucene非常复杂，你需要深入了解检索的相关知识来理解它是如何工作的。

### 常见的开源搜索引擎

基于Lucene的搜索引擎，Java开发，包括：

- Lucene
- Solr
- elasticsearch
- katta
- compass

基于C++开发的：

- Sphinx

你可以想想Lucene的强大。接下来简要的介绍各搜索引擎的特点。

#### Lucene

Lucene的开发语言是Java，也是Java家族中最为出名的一个开源搜索引擎，在Java世界中已经是标准的全文检索程序，它提供了完整的查询引擎和索引引擎，没有中文分词引擎，需要自己去实现，因此用Lucene去做一个搜素引擎需要自己去架构，另外它不支持实时搜索。
优点：

- 成熟的解决方案，有很多的成功案例。apache 顶级项目，正在持续快速的进步。庞大而活跃的开发社区，大量的开发人员。它只是一个类库，有足够的定制和优化空间：经过简单定制，就可以满足绝大部分常见的需求；经过优化，可以支持 10亿+ 量级的搜索

缺点：

- 需要额外的开发工作。所有的扩展，分布式，可靠性等都需要自己实现；非实时，从建索引到可以搜索中间有一个时间延迟，而当前的“近实时”(Lucene Near Real Time search)搜索方案的可扩展性有待进一步完善

#### Solr

Solr是一个企业级的高性能、采用Java开发，基于Lucene的全文搜索服务器。
文档通过Http利用XML加到一个搜索集合中。

查询该集合也是通过 http收到一个XML/JSON响应来实现。它的主要特性包括：高效、灵活的缓存功能，垂直搜索功能，高亮显示搜索结果，通过索引复制来提高可用性，提 供一套强大Data Schema来定义字段，类型和设置文本分析，提供基于Web的管理界面等。
优点：

- Solr有一个更大、更成熟的用户、开发和贡献者社区
- 支持添加多种格式的索引，如：HTML、PDF、微软 Office 系列软件格式以及 JSON、XML、CSV 等纯文本格式
- Solr比较成熟、稳定
- 不考虑建索引的同时进行搜索，速度更快

缺点：

- 建立索引时，搜索效率下降，实时索引搜索效率不高

#### Sphinx

Sphinx一个基于SQL的全文检索引擎，特别为一些脚本语言（PHP,Python，Perl，Ruby）设计搜索API接口。

Sphinx是一个用C++语言写的开源搜索引擎，也是现在比较主流的搜索引擎之一，在建立索引的时间方面比Lucene快50%，但是索引文件比Lucene要大一倍，因此Sphinx在索引的建立方面是空间换取时间的策略，在检索速度上，和lucene相差不大，但检索精准度方面Lucene要优于Sphinx，另外在加入中文分词引擎难度方面，Lucene要优于Sphinx.其中Sphinx支持实时搜索，使用起来比较简单方便.

Sphinx可以非常容易的与SQL数据库和脚本语言集成。当前系统内置MySQL和PostgreSQL 数据库数据源的支持，也支持从标准输入读取特定格式 的XML数据。通过修改源代码，用户可以自行增加新的数据源（例如：其他类型的DBMS 的原生支持）
Sphinx的特点：

- 高速的建立索引(在当代CPU上，峰值性能可达到10 MB/秒)
- 高性能的搜索(在2 – 4GB 的文本数据上，平均每次检索响应时间小于0.1秒)
- 可处理海量数据(目前已知可以处理超过100 GB的文本数据, 在单一CPU的系统上可 处理100 M 文档)
- 提供了优秀的相关度算法，基于短语相似度和统计（BM25）的复合Ranking方法
- 支持分布式搜索
- 支持短语搜索
- 提供文档摘要生成
- 可作为MySQL的存储引擎提供搜索服务
- 支持布尔、短语、词语相似度等多种检索模式
- 文档支持多个全文检索字段(最大不超过32个)
- 文档支持多个额外的属性信息(例如：分组信息，时间戳等)
- 支持断词

#### ElasticSearch

ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。
优点：

- 分布式：节点对外表现对等，加入节点自动均衡
- elasticsearch完全支持Apache Lucene的接近实时的搜索
- 各节点组成对等的网络结构，当某个节点出现故障时会自动分配其他节点代替期进行工作
- 横向可扩展性，如果你需要增加一台服务器，只需要做点配置，然后启动就完事了
- 高可用：提供复制（replica）机制，一个分片可以设置多个复制，使得某台服务器宕机的情况下，集群仍旧可以照常运行，并会把由于服务器宕机丢失的复制恢复到其它可用节点上；这点也类似于HDFS的复制机制（HDFS中默认是3份复制）

缺点：

- 不支持事物
- 相对吃内存

------

see also：[Apache Lucene](https://lucene.apache.org/) | [开源搜索引擎分类](https://blog.csdn.net/belalds/article/details/82667692) | [ElasticSearch vs Solr多维度分析对比](https://www.cnblogs.com/zlslch/p/6612639.html) | [Lucene：基于Java的全文检索引擎简介](https://www.chedong.com/tech/lucene.html) 欢迎斧正，that's all

---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/elasticsearch-02-apache-lucene/  

