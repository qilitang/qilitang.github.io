# ElasticSearch倒排索引


随着央视诗词大会的热播，小史开始对诗词感兴趣，最喜欢的就是飞花令的环节。

![](https://pic2.zhimg.com/80/v2-5228b0a4ad31e39b9b731044085426f9_hd.jpg#alt=img)

但是由于小史很久没有背过诗词了，飞一个字很难说出一句，很多之前很熟悉的诗句也想不起来。

![](https://pic3.zhimg.com/80/v2-280b7ab44c3f4e8b8a48000fdc9b7e6e_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-def705467a1fc78311bdd0e746de76a1_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-213a96e616c2f5bf6de0fed96cdc4bfc_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-e8ec11557e537d46c39e85f566b3e0c3_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-db9f8919ab6e563c97f2ebf295af227d_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-e28d0e3e54ae2f9c0faa18a05e2ba934_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-7938aff727bedac3dea1d88297235475_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-7cde8e1cdc5866dae381a3957ccde57f_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-cb0626c05647361e033f2a0ab4f779bb_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-cad16f954ef40e1c18fa4e52b070e647_hd.jpg#alt=img)

**倒排索引**

![](https://pic3.zhimg.com/80/v2-8ec0b0da336ab8abf9932b90b9f9811e_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-d4b5505fc53c6dd793695c82a84da884_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-5b4afa42eb818cb1597798c186211868_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-f180f28669673936d3754bb343a29078_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-1f6267412401d52292e93b216f1a29ae_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-09d80e27b9c735b3e5dfa48926b3ad38_hd.jpg#alt=img)

**吕老师：**但是我让你说出带“前”字的诗句，由于没有索引，你只能遍历脑海中所有诗词，当你的脑海中诗词量大的时候，就很难在短时间内得到结果了。

![](https://pic4.zhimg.com/80/v2-c177e6781bdd53c06baba54e40f3d5e3_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-7377d47938ffab9b49e13f25ff0d03b2_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-fd6276182c73bda022aebbe17f07e47f_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-ee45432b984d48ad04b6673bc3fb536e_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-c803bc3a21d8558d47219c0daaa49309_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-6ecdea9437561f66a8f0fe89ebd57020_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-d8db0996eb9adadc34b28581699829bb_hd.jpg#alt=img)

**索引量爆炸**

![](https://pic4.zhimg.com/80/v2-ed850c291b0b51ffd51232ce2b306ed3_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-215645eb9bf0914e4a12fc17cab9b306_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-3388d6dcf0a21c8278e881d369e1f31a_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-499a8eb7a79dcd525a05b73e0253852d_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-7c28ef4bfdcfe8e9873cfd8c61f69b6b_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-83b2acab0ed6fcce1f07bc4ec6338d1d_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-d639d0ef78264ea66899f11da9bd004d_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-e5580feed3fe5e714736313fd8309bce_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-21a0d93b2816b2c83ae335db99438e71_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-dc71c47cffd58f180b0355713bdc72ca_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-96da7202725c62c3d234be6fcc85480e_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-d1ecdbae17586f1e21f65324d04ed7ac_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-b4726e6972818ac46250e54682c0f939_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-9ec9f32b71e22980978a2626b68167ac_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-418b06473996de90a416f30b56245d8c_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-22ddabc7f1b02260e4f1fa10bafa509d_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-949fc2f3c8427d41589733b3cf98697d_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-48ec5853f184655c487e5d070005d3af_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-a0a23507caca6e22b6a1d3eca7bf67d9_hd.jpg#alt=img)

**搜索引擎原理**

![](https://pic2.zhimg.com/80/v2-eeefe2669de4ee10d9ad0fb492bb9fe5_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-c88b7b533f3821cce041cfd177d08d3d_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-6894533d32bfc7c142ff3b06695f1bc1_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-6be627794c85dbee98021a654cf4e2a4_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-2a5889d293f8147193b4d1a832656713_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-25eafa285109ad9fbf9c66ff86756429_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-c3d0c44de959ddf31b26a069ec8893ef_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-f3168f6ef74186c3f3905bbdba1bc434_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-5306b7be95032503bafe9c8d4a5e7d39_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-9306b11457eb769ea72a9c7a85d615e1_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-4962264f39e89c9825a4d906271b4e3e_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-b1c49ee21caaa76b3ab1d19e1b61fc66_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-1da4339c8e3d2721823cab6cf84183a7_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-4d63ccecb5d53a951f0ed3cda1321267_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-ec368c121b4dc74b069e001d98cab836_hd.jpg#alt=img)

**Elasticsearch 简介**

![](https://pic2.zhimg.com/80/v2-07f415f308049708123a27b9de263415_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-4f4507aa008f4fcbbad0acc6dc202efa_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-1280aa01af7bf7c25e8ee50721c3028e_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-735711c52660b5e282bd1257991c1df8_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-4b1c0467ba842cb05e2f7ba217158e1e_hd.jpg#alt=img)

**吕老师：**但是 Lucene 还是一个库，必须要懂一点搜索引擎原理的人才能用的好，所以后来又有人基于 Lucene 进行封装，写出了 Elasticsearch。

![](https://pic4.zhimg.com/80/v2-64894289738f789839753831ce18ddcf_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-4f36c0d5c4483cb0306e1bafd3d946eb_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-19bb767a4743d87281d5463b0c03ce43_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-ea3933f1d1dda0eaf7025cf5852240fa_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-a7e9078dd2f91ab8d996bcb7427a9d4c_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-4ee7ca992fb19d055b2428f1bb08cb91_hd.jpg#alt=img)

**Elasticsearch 基本概念**

![](https://pic1.zhimg.com/80/v2-328809d09498d1da1eeb96ca28b3f208_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-b5a6834c5ce28d9917b3c4e73a646146_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-47845232d4a753d76364fcbbe4e37631_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-6ac957659142d3d9787cc2a055456df0_hd.jpg#alt=img)

![](https://pic2.zhimg.com/80/v2-0034f2976c9adb1bbefb062ce8532be1_hd.jpg#alt=img)

**吕老师：**类型是用来定义数据结构的，你可以认为是 MySQL 中的一张表。文档就是最终的数据了，你可以认为一个文档就是一条记录。

![](https://pic4.zhimg.com/80/v2-7290f618429029f5fbdd7475fbd469c7_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-64ec15aae5d926c07e3a788925e56c10_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-416ad2d83b1f5ad573c60543d286c0da_hd.jpg#alt=img)

**吕老师：**比如一首诗，有诗题、作者、朝代、字数、诗内容等字段，那么首先，我们可以建立一个名叫 Poems 的索引，然后创建一个名叫 Poem 的类型，类型是通过 Mapping 来定义每个字段的类型。

比如诗题、作者、朝代都是 Keyword 类型，诗内容是 Text 类型，而字数是 Integer 类型，最后就是把数据组织成 Json 格式存放进去了。

![](https://pic1.zhimg.com/80/v2-1897d394e89794a9776d83e47ec3da98_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-8add22f70cd54238ab461380cc0adef3_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-857fb40d14e66653bfd487f71f6b82e2_hd.jpg#alt=img)

**吕老师：**这个问题问得好，这涉及到分词的问题，Keyword 类型是不会分词的，直接根据字符串内容建立反向索引，Text 类型在存入 Elasticsearch 的时候，会先分词，然后根据分词后的内容建立反向索引。

![](https://pic1.zhimg.com/80/v2-3dbfc23c1aa4cbda9428ca104e771a38_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-b1e0b6dbfd4465fe07e53afe0ad2fd42_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-511e081c2a08a01c8edf30d6e01ee7a6_hd.jpg#alt=img)

**吕老师：**之前我们说过，Elasticsearch 把操作都封装成了 HTTP 的 API，我们只要给 Elasticsearch 发送 HTTP 请求就行。

比如使用 curl -XPUT '[http://ip](https://link.zhihu.com/?target=http%3A//ip):port/poems'，就能建立一个名为 Poems 的索引，其他操作也是类似的。

![](https://pic2.zhimg.com/80/v2-2ab73675d30fc2658d6ca21eafdf1c61_hd.jpg#alt=img)

**Elasticsearch 分布式原理**

![](https://pic3.zhimg.com/80/v2-e57359d3a25af8fd744c6ca3e15e15da_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-5e34cae2f4e05bc639812494afe17a96_hd.jpg#alt=img)

**吕老师：**没错，Elasticsearch 也是会对数据进行切分，同时每一个分片会保存多个副本，其原因和 HDFS 是一样的，都是为了保证分布式环境下的高可用。

![](https://pic3.zhimg.com/80/v2-43e459084a161beff1791f57345e4966_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-cd85b49fde2984c42c9a6afa0959564a_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-111fa7d3a88637c974a6f8db28ff3457_hd.jpg#alt=img)

**吕老师：**没错，在 Elasticsearch 中，节点是对等的，节点间会通过自己的一些规则选取集群的 Master，Master 会负责集群状态信息的改变，并同步给其他节点。

![](https://pic4.zhimg.com/80/v2-e3a489113b722705b6adbd7a72aa052b_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-346b1ecc1723bccbb5e3f56b41f8f3c7_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-1f758edf1d868d0631ccf54b1052b0e3_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-d71b92cbf7d9272068602e9442fd20db_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-4e5b3eb17cae9985fcca60cf223e2f16_hd.jpg#alt=img)

**吕老师：**注意，只有建立索引和类型需要经过 Master，数据的写入有一个简单的 Routing 规则，可以 Route 到集群中的任意节点，所以数据写入压力是分散在整个集群的。

![](https://pic3.zhimg.com/80/v2-5cb0eb47ff98e11782283f6ce1c997f6_hd.jpg#alt=img)

**ELK 系统**

![](https://pic4.zhimg.com/80/v2-956e6b35a83b4e8e6884e8c011e8f153_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-7c3a30bc816d898fe62248b49212088b_hd.jpg#alt=img)

**吕老师：**其实很多公司都用 Elasticsearch 搭建 ELK 系统，也就是日志分析系统。其中 E 就是 Elasticsearch，L 是 Logstash，是一个日志收集系统，K 是 Kibana，是一个数据可视化平台。

![](https://pic1.zhimg.com/80/v2-e68267289f6f73bb352c50e7b148cb6c_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-a1e3d18990a1690d3ff4e7364be08bdc_hd.jpg#alt=img)

![](https://pic3.zhimg.com/80/v2-1b535771a532c7179817d8072885a066_hd.jpg#alt=img)

**吕老师：**分析日志的用处可大了，你想，假如一个分布式系统有 1000 台机器，系统出现故障时，我要看下日志，还得一台一台登录上去查看，是不是非常麻烦？

![](https://pic3.zhimg.com/80/v2-8829653d55c64ac8af08001e2fbb421a_hd.jpg#alt=img)

![](https://pic4.zhimg.com/80/v2-0176b33be901e992f9cc22fb8fac322f_hd.jpg#alt=img)

**吕老师：**但是如果日志接入了 ELK 系统就不一样。比如系统运行过程中，突然出现了异常，在日志中就能及时反馈，日志进入 ELK 系统中，我们直接在 Kibana 就能看到日志情况。如果再接入一些实时计算模块，还能做实时报警功能。

![](https://pic4.zhimg.com/80/v2-23d0a8aa318cea3201664d835b451693_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-69d240d9468895c392a290e58aba15fc_hd.jpg#alt=img)

![](https://pic1.zhimg.com/80/v2-b8906defdfa9684d1032afef361aff70_hd.jpg#alt=img)

**搜索引擎原理**

小史学完了 Elasticsearch，在笔记本上写下了如下记录：

- 反向索引又叫倒排索引，是根据文章内容中的关键字建立索引。
- 搜索引擎原理就是建立反向索引。
- Elasticsearch 在 Lucene 的基础上进行封装，实现了分布式搜索引擎。
- Elasticsearch 中的索引、类型和文档的概念比较重要，类似于 MySQL 中的数据库、表和行。
- Elasticsearch 也是 Master-slave 架构，也实现了数据的分片和备份。
- Elasticsearch 一个典型应用就是 ELK 日志分析系统。

写完，又高高兴兴背诗去了。


---

> 作者: [Qilitang](https://github.com/qilitang)  
> URL: https://www.qilitang.top/posts/16-%E6%89%A9%E5%B1%95%E9%98%85%E8%AF%BB-%E5%80%92%E6%8E%92%E7%B4%A2%E5%BC%95/  

